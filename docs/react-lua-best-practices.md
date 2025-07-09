# React Lua Best Practices

Essential practices and patterns for building robust, maintainable Roblox UIs with `react-lua`.

## 🎯 Core Principles

`react-lua` is a powerful framework for building robust, maintainable Roblox UIs. While it mirrors ReactJS, understanding Luau specifics and Roblox integration is key.

## 📝 1. Luau Strict Mode is Essential

**Rule:** Always enable Luau strict mode (`--!strict` or `.luaurc`) in `react-lua` files.

**Why:** Provides compile-time type checking, catching common errors (e.g., misspelled props, inconsistent function returns) before runtime. This is critical for reliable and scalable UI development.

**Caveat (Typing `useState`):** When dealing with optional or union types in `useState`, Luau's type inference can be tricky. Use explicit type assertions:

```lua
local myValue: number?, setMyValue = React.useState(nil :: number?)
type MenuState = "open" | "closed"
local menuState: MenuState, setMenuState = React.useState("open" :: MenuState)
```

## 🔑 2. Embrace Component Keys for Lists

**Rule:** Every item in a dynamically generated list of React children **must** have a unique, stable `key` property.

**Why:** React uses keys to efficiently identify, reconcile, and reorder elements. Without them, performance suffers, and UI state can be lost or incorrect when lists change.

```lua
-- Bad (will cause issues if `items` change order or are removed)
return e("Frame", {}, { e(Item), e(Item), e(Item) })

-- Good (using explicit named children as keys)
return e("Frame", {}, {
    FirstItem = e(Item),
    SecondItem = e(Item),
})

-- Good (for dynamic lists, use a unique identifier from your data)
for _, data in items do
    children[data.id] = e(Item, { data = data })
end
```

**Tip (Dynamic Unique Keys):** For truly dynamic lists without stable IDs, create a utility to generate unique keys on the fly. This mitigates most common issues.

## 📊 3. Manage LayoutOrder Programmatically

**Rule:** Avoid hardcoding `LayoutOrder` values for elements within `UIListLayout` (or similar).

**Why:** Manual `LayoutOrder` becomes a maintenance nightmare as UI changes.

**Solution:** Use a simple higher-order function to generate incrementing `LayoutOrder` values:

```lua
local function createNextOrder(): () -> number
    local order = 0
    return function()
        order += 1
        return order
    end
end

local nextOrder = createNextOrder()
-- Use it like: LayoutOrder = nextOrder()
```

## ⚡ 4. Efficient Conditional Rendering

**Rule:** Use Luau's `and` operator for simple conditional rendering.

**Why:** `x and y` returns `y` if `x` is truthy, otherwise `x`. React treats `false` (and `nil`) as "do not render." This is concise and idiomatic.

```lua
local showMenu = true -- Or state variable
return e("Frame", {}, {
    MenuComponent = showMenu and e(Menu),
})
```

**Alternative:** Use `if ... then ... else` expressions for more complex conditions or when both branches render something:

```lua
return e("Frame", {}, {
    Item = if condition then e(VisibleItem) else e(HiddenPlaceholder),
})
```

## 🧩 5. `React.createElement` Arity (Multiple Children)

**Rule:** `React.createElement` accepts multiple arguments after `props` which are all treated as children.

**Why:** This simplifies combining static and dynamic children without needing `table.join` or complex merging logic.

```lua
-- Instead of: e("Frame", {}, join({ StaticChild = e("TextLabel") }, dynamicChildrenTable))
return e("Frame", {},
    { StaticChild = e("TextLabel") }, -- Static children table
    dynamicChildrenTable -- Dynamic children table
)
```

## 🎣 6. Custom Hooks for Reusability

**Rule:** Encapsulate reusable UI logic, side effects, or state patterns into custom hooks.

**Why:** Promotes code reusability, testability, and separation of concerns.

**Examples:**
- `useToggleState`: For boolean state (e.g., menu open/closed, hover).
- `useEventConnection`: For connecting to `RBXScriptSignal`s and ensuring automatic disconnection on unmount.
- `useClock`: For time-based animations synchronized with `RunService`.

**Performance (`useCallback`, `useMemo`):** Use these hooks to memoize functions and values passed to child components or effects, preventing unnecessary re-renders.

## 🌐 7. Prefer React Context Over External State Managers (Often)

**Rule:** For localized or feature-specific global state, React Context is often sufficient and simpler than external libraries like Rodux/Redux.

**Why:** Reduces boilerplate and keeps related state closer to the components that consume it.

**Tip (Typing Context):** Explicitly type your context `value` within your Provider to leverage Luau strict mode's checks.

**Tip (ContextStack):** Use a `ContextStack` component to prevent deeply nested context provider syntax for better readability.

## 📝 8. Handling `TextBox` Inputs

**Rule:** Be cautious with standard controlled component patterns (`Text = value, [React.Change.Text] = onChange`) for `TextBox` in `react-lua`, especially on mobile or for input restrictions (e.g., `maxLength`).

**Why:** Roblox's `TextBox` can exhibit lag or unexpected behavior when its `Text` property is immediately re-set by React in response to user input.

**Solution:** Implement a custom `TextBox` component that uses `React.useRef` to allow Roblox to manage text updates naturally, only stepping in forcefully (e.g., via `textBox.Text = ...`) for specific cases like `maxLength` enforcement.

## 🔗 9. Bindings vs. State/Props

**Rule:** Use `React.Binding` sparingly. Prefer standard `React.useState` and component `props` for most UI updates.

**Why:** Bindings, while performant for extremely high-frequency updates (e.g., per-frame animations), are less ergonomic and flexible than state/props. Their performance benefit in `react-lua` is less pronounced than in legacy Roact.

**When to Use Bindings:** Only for values that change almost every frame (e.g., animation alphas, real-time physics data).

## 🚪 10. Mount to Portals for Safe Integration

**Rule:** When rendering your primary React tree, `ReactRoblox.createTree` to a dummy `Folder` (e.g., in `PlayerGui`), and then use `ReactRoblox.createPortal` to target the actual Roblox UI parent (e.g., `PlayerGui`).

**Why:** Direct mounting (e.g., `createTree(PlayerGui)`) gives React complete ownership over the target instance, potentially destroying or interfering with Roblox's own built-in UI elements (like mobile controls, chat, leaderboards). Portals render React's children into a different instance without React taking full control of that instance's entire child list.

## 📁 11. Correct Module `require()` Paths with Rojo

**Problem:** A common pitfall is using `script.ModuleName` for sibling modules when the project is managed by Rojo. This often results in runtime errors.

**Why it Fails:** In Roblox Studio's hierarchy (which Rojo maps to), a module script (`script`) does not contain its sibling modules as direct children. They reside at the same level, under `script.Parent`.

- `script` refers to the current module script itself.
- `script.Parent` refers to the folder or container that the current module script is *directly inside*.

**Rule:** When `requiring` a module that is a *sibling* (at the same hierarchical level as your current script in the Roblox DataModel, as mapped by Rojo), always use `script.Parent.ModuleName`.

**Example File Structure (Local):**
```
my_project/
├── src/
│   └── client/
│       └── UI/
│           ├── MainUI.client.lua   <-- Your current script
│           └── Components/        <-- Folder containing sibling modules
│               └── Button.lua
```

**Correct `require()` Call (in `MainUI.client.lua`):**
```lua
-- If Button.lua is a sibling to MainUI.client.lua within the Roblox DataModel (e.g., PlayerGui.UI)
local Button = require(script.Parent.Components.Button)
```

**Incorrect `require()` Call (would fail):**
```lua
-- This only works if 'Components' is actually nested INSIDE 'MainUI.client.lua', which is rare and not standard practice
local Button = require(script.Components.Button)
```

## 📋 Best Practices Summary

1. **Enable Strict Mode:** Always use `--!strict` for type safety
2. **Use Keys for Lists:** Every dynamic list item needs a unique key
3. **Programmatic LayoutOrder:** Generate order values automatically
4. **Conditional Rendering:** Use `and` operator for simple conditions
5. **Custom Hooks:** Encapsulate reusable logic
6. **Context Over Redux:** Use React Context for local state
7. **Handle TextBox Carefully:** Use refs for controlled inputs
8. **Sparse Bindings:** Prefer state/props over bindings
9. **Use Portals:** Mount safely to avoid conflicts
10. **Correct Module Paths:** Use `script.Parent` for siblings

---

*These practices ensure maintainable, performant, and reliable React Lua applications.* 