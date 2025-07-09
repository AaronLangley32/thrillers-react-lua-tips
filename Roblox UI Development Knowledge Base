# Roblox UI Development Knowledge Base

This repository serves as a personal knowledge base and comprehensive guide for building user interfaces in Roblox using `jsdotlua/react-lua`. It covers core concepts, best practices, styling references, and common pitfalls, leveraging insights from official documentation and community wisdom.

---

## Table of Contents

1.  [Roblox UI Graphics Optimization: PNGs, Scaling, and Slice 9](#roblox-ui-graphics-optimization-pngs-scaling-and-slice-9)
2.  [React Lua Development: Essential Practices](#react-lua-development-essential-practices)
3.  [React Lua UI Styling & Configuration Reference](#react-lua-ui-styling--configuration-reference)
4.  [React Lua Resources & Documentation](#react-lua-resources--documentation)
5.  [Understanding Color3.new vs. Color3.fromRGB](#understanding-color3new-vs-color3fromrgb)
6.  [Normal Maps (Bump Map Equivalent) in Roblox](#normal-maps-bump-map-equivalent-in-roblox)
7.  [Facial Rigging with Part-Based Characters](#facial-rigging-with-part-based-characters)
8.  [Introduction to Entity-Component-System (ECS) with JECS](#introduction-to-entity-component-system-ecs-with-jecs)

---

## Roblox UI Graphics Optimization: PNGs, Scaling, and Slice 9

These principles are fundamental to Roblox's UI rendering engine and apply universally, whether using `react-lua` or traditional Roblox UI scripting. `react-lua` provides the tools to *implement* these practices efficiently.

### 1. Vector Graphics (SVGs) Not Supported

Roblox Studio **does not natively support vector graphics (SVGs)** for UI elements. The UI system primarily uses **raster image formats** (PNG, JPG, TGA, BMP).

### 2. Recommended File Type: High-Resolution PNGs

*   **Best Practice:** Design UI assets in vector software (e.g., Illustrator, Figma, Inkscape).
*   **Export As:** High-resolution PNG files with transparent backgrounds. PNGs offer lossless quality and transparency.

### 3. Recommended Element Sizes (Design Reference)

These are general visual targets; final display size depends on UI layout:

*   **Small Icons:** 32x32px to 64x64px
*   **Medium Icons:** 64x64px to 128x128px
*   **Large Icons/Elements:** 128x128px and up

### 4. Leverage Roblox's Built-In Scaling

*   **Export High-Resolution:** Export PNGs at a significantly higher resolution than their anticipated in-game display size (e.g., 512x512px or 1024x1024px for a 64x64px expected display).
*   **How it Works:** When a high-res PNG is applied to an ImageLabel or ImageButton, Roblox's engine automatically downscales it to fit the UI element's dimensions.
*   **Benefit:** Downscaling from a high-resolution source results in a much sharper, less pixelated appearance, mimicking vector-like crispness without exporting multiple file sizes.

### 5. Dynamic Scaling with Slice 9

*   **Purpose:** For UI elements (buttons, frames, panels) that need to stretch while maintaining consistent borders and appearance.
*   **How it Works:** Defines nine regions (corners, edges, center) of an image. When resized:
    *   **Corners:** Remain fixed.
    *   **Edges:** Stretch along their axis.
    *   **Center:** Tiles or stretches to fill space.
*   **Configuration:** Set ImageLabel or ImageButton's `ScaleType` to `Slice` and adjust the `SliceCenter` property.

---

## React Lua Development: Essential Practices

`react-lua` is a powerful framework for building robust, maintainable Roblox UIs. While it mirrors ReactJS, understanding Luau specifics and Roblox integration is key.

### 1. Luau Strict Mode is Essential

*   **Rule:** Always enable Luau strict mode (`--!strict` or `.luaurc`) in `react-lua` files.
*   **Why:** Provides compile-time type checking, catching common errors (e.g., misspelled props, inconsistent function returns) before runtime. This is critical for reliable and scalable UI development.
*   **Caveat (Typing `useState`):** When dealing with optional or union types in `useState`, Luau's type inference can be tricky. Use explicit type assertions:
    ```lua
    local myValue: number?, setMyValue = React.useState(nil :: number?)
    type MenuState = "open" | "closed"
    local menuState: MenuState, setMenuState = React.useState("open" :: MenuState)
    ```

---

## 2. Embrace Component Keys for Lists

*   **Rule:** Every item in a dynamically generated list of React children **must** have a unique, stable `key` property.
*   **Why:** React uses keys to efficiently identify, reconcile, and reorder elements. Without them, performance suffers, and UI state can be lost or incorrect when lists change.
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
*   **Tip (Dynamic Unique Keys):** For truly dynamic lists without stable IDs, create a utility to generate unique keys on the fly. This mitigates most common issues.

---

## 3. Manage LayoutOrder Programmatically

*   **Rule:** Avoid hardcoding `LayoutOrder` values for elements within `UIListLayout` (or similar).
*   **Why:** Manual `LayoutOrder` becomes a maintenance nightmare as UI changes.
*   **Solution:** Use a simple higher-order function to generate incrementing `LayoutOrder` values:
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

---

## 4. Efficient Conditional Rendering

*   **Rule:** Use Luau's `and` operator for simple conditional rendering.
*   **Why:** `x and y` returns `y` if `x` is truthy, otherwise `x`. React treats `false` (and `nil`) as "do not render." This is concise and idiomatic.
    ```lua
    local showMenu = true -- Or state variable
    return e("Frame", {}, {
        MenuComponent = showMenu and e(Menu),
    })
    ```
*   **Alternative:** Use `if ... then ... else` expressions for more complex conditions or when both branches render something:
    ```lua
    return e("Frame", {}, {
        Item = if condition then e(VisibleItem) else e(HiddenPlaceholder),
    })
    ```

---

## 5. `React.createElement` Arity (Multiple Children)

*   **Rule:** `React.createElement` accepts multiple arguments after `props` which are all treated as children.
*   **Why:** This simplifies combining static and dynamic children without needing `table.join` or complex merging logic.
    ```lua
    -- Instead of: e("Frame", {}, join({ StaticChild = e("TextLabel") }, dynamicChildrenTable))
    return e("Frame", {},
        { StaticChild = e("TextLabel") }, -- Static children table
        dynamicChildrenTable -- Dynamic children table
    )
    ```

---

## 6. Custom Hooks for Reusability

*   **Rule:** Encapsulate reusable UI logic, side effects, or state patterns into custom hooks.
*   **Why:** Promotes code reusability, testability, and separation of concerns.
*   **Examples:**
    *   `useToggleState`: For boolean state (e.g., menu open/closed, hover).
    *   `useEventConnection`: For connecting to `RBXScriptSignal`s and ensuring automatic disconnection on unmount.
    *   `useClock`: For time-based animations synchronized with `RunService`.
*   **Performance (`useCallback`, `useMemo`):** Use these hooks to memoize functions and values passed to child components or effects, preventing unnecessary re-renders.

---

## 7. Prefer React Context Over External State Managers (Often)

*   **Rule:** For localized or feature-specific global state, React Context is often sufficient and simpler than external libraries like Rodux/Redux.
*   **Why:** Reduces boilerplate and keeps related state closer to the components that consume it.
*   **Tip (Typing Context):** Explicitly type your context `value` within your Provider to leverage Luau strict mode's checks.
*   **Tip (ContextStack):** Use a `ContextStack` component to prevent deeply nested context provider syntax for better readability.

---

## 8. Handling `TextBox` Inputs

*   **Rule:** Be cautious with standard controlled component patterns (`Text = value, [React.Change.Text] = onChange`) for `TextBox` in `react-lua`, especially on mobile or for input restrictions (e.g., `maxLength`).
*   **Why:** Roblox's `TextBox` can exhibit lag or unexpected behavior when its `Text` property is immediately re-set by React in response to user input.
*   **Solution:** Implement a custom `TextBox` component that uses `React.useRef` to allow Roblox to manage text updates naturally, only stepping in forcefully (e.g., via `textBox.Text = ...`) for specific cases like `maxLength` enforcement.

---

## 9. Bindings vs. State/Props

*   **Rule:** Use `React.Binding` sparingly. Prefer standard `React.useState` and component `props` for most UI updates.
*   **Why:** Bindings, while performant for extremely high-frequency updates (e.g., per-frame animations), are less ergonomic and flexible than state/props. Their performance benefit in `react-lua` is less pronounced than in legacy Roact.
*   **When to Use Bindings:** Only for values that change almost every frame (e.g., animation alphas, real-time physics data).

---

## 10. Mount to Portals for Safe Integration

*   **Rule:** When rendering your primary React tree, `ReactRoblox.createTree` to a dummy `Folder` (e.g., in `PlayerGui`), and then use `ReactRoblox.createPortal` to target the actual Roblox UI parent (e.g., `PlayerGui`).
*   **Why:** Direct mounting (e.g., `createTree(PlayerGui)`) gives React complete ownership over the target instance, potentially destroying or interfering with Roblox's own built-in UI elements (like mobile controls, chat, leaderboards). Portals render React's children into a different instance without React taking full control of that instance's entire child list.

---

## 11. Correct Module `require()` Paths with Rojo

*   **Problem:** A common pitfall is using `script.ModuleName` for sibling modules when the project is managed by Rojo. This often results in runtime errors.
*   **Why it Fails:** In Roblox Studio's hierarchy (which Rojo maps to), a module script (`script`) does not contain its sibling modules as direct children. They reside at the same level, under `script.Parent`.
    *   `script` refers to the current module script itself.
    *   `script.Parent` refers to the folder or container that the current module script is *directly inside*.
*   **Rule:** When `requiring` a module that is a *sibling* (at the same hierarchical level as your current script in the Roblox DataModel, as mapped by Rojo), always use `script.Parent.ModuleName`.
*   **Example File Structure (Local):**
    ```
    my_project/
    ├── src/
    │   └── client/
    │       └── UI/
    │           ├── MainUI.client.lua   <-- Your current script
    │           └── Components/        <-- Folder containing sibling modules
    │               └── Button.lua
    ```
*   **Correct `require()` Call (in `MainUI.client.lua`):**
    ```lua
    -- If Button.lua is a sibling to MainUI.client.lua within the Roblox DataModel (e.g., PlayerGui.UI)
    local Button = require(script.Parent.Components.Button)
    ```
*   **Incorrect `require()` Call (would fail):**
    ```lua
    -- This only works if 'Components' is actually nested INSIDE 'MainUI.client.lua', which is rare and not standard practice
    local Button = require(script.Components.Button)
    ```

---

## React Lua UI Styling & Configuration Reference

This document outlines common styling attributes and configurable parameters for Roblox UI elements when used with `react-lua`. These properties are passed as the `props` table to `React.createElement`.

### 1. Common Visual Properties (Across many UI elements)

These apply to most visual UI objects like `Frame`, `TextLabel`, `TextButton`, `ImageLabel`, `ImageButton`, `TextBox`, `ScrollingFrame`, etc.

#### Size & Position

*   **`Size`**: `UDim2` – Defines the size of the UI element.
    ```lua
    Size = UDim2.new(0.5, 0, 0.2, 0) -- 50% width, 20% height (scale), no pixel offset
    ```
    *   *Sets the dimension of the UI element.*
*   **`Position`**: `UDim2` – Defines the position of the UI element relative to its parent.
    ```lua
    Position = UDim2.fromScale(0.5, 0.5) -- Centers the element (if AnchorPoint is 0.5,0.5)
    ```
    *   *Sets the location of the UI element.*
*   **`AnchorPoint`**: `Vector2` – Sets the origin point for `Position` and rotation. (0,0 is top-left, 0.5,0.5 is center).
    ```lua
    AnchorPoint = Vector2.new(0.5, 0.5) -- Often used with Position.fromScale(0.5, 0.5) for perfect centering
    ```
    *   *Defines the pivot point for positioning.*

#### Color & Transparency

*   **`BackgroundColor3`**: `Color3` – The background color of the UI element.
    ```lua
    BackgroundColor3 = Color3.fromRGB(50, 50, 50) -- Dark gray using RGB 0-255
    ```
    *   *Sets the background fill color.*
*   **`BackgroundTransparency`**: `number` – How transparent the background is (0 = opaque, 1 = fully transparent).
    ```lua
    BackgroundTransparency = 0.5 -- 50% transparent background
    ```
    *   *Controls the visibility of the background color.*
*   **`BorderSizePixel`**: `number` – The thickness of the border in pixels. (Set to 0 for no border).
    ```lua
    BorderSizePixel = 0 -- No border
    ```
    *   *Sets the width of the pixel-based border.*
*   **`BorderColor3`**: `Color3` – The color of the pixel-based border.
    ```lua
    BorderColor3 = Color3.new(1, 0, 0) -- Red border
    ```
    *   *Sets the color of the pixel-based border.*
*   **`ClipsDescendants`**: `boolean` – If true, children outside the element's bounds will be clipped.
    ```lua
    ClipsDescendants = true -- Hides parts of children that go outside this element's boundaries
    ```
    *   *Controls whether child elements are clipped to the parent's bounds.*

#### Visibility & Interaction

*   **`Visible`**: `boolean` – If true, the UI element is visible.
    ```lua
    Visible = true -- Element is visible
    ```
    *   *Controls the visibility of the element and its children.*
*   **`Active`**: `boolean` – If true, the element can receive user input (clicks, hovers).
    ```lua
    Active = true -- Element can receive mouse/touch input
    ```
    *   *Determines if the element can be interacted with.*
*   **`ZIndex`**: `number` – Determines the drawing order of overlapping UI elements. Higher values draw on top.
    ```lua
    ZIndex = 5 -- Draws this element on top of elements with ZIndex < 5
    ```
    *   *Controls the stacking order of overlapping elements.*

### 2. Text-Specific Properties (`TextLabel`, `TextButton`, `TextBox`)

#### Text Content & Appearance

*   **`Text`**: `string` – The display text.
    ```lua
    Text = "Hello, World!"
    ```
    *   *Sets the string content displayed by the element.*
*   **`TextColor3`**: `Color3` – The color of the text.
    ```lua
    TextColor3 = Color3.new(1, 1, 1) -- White text
    ```
    *   *Sets the color of the text.*
*   **`TextSize`**: `number` – The font size in pixels.
    ```lua
    TextSize = 24 -- 24 pixel font size
    ```
    *   *Controls the size of the font.*
*   **`Font`**: `Enum.Font` – The font family to use.
    ```lua
    Font = Enum.Font.SourceSansBold -- Uses a bold Sans-Serif font
    ```
    *   *Specifies the font style.*
*   **`TextTransparency`**: `number` – How transparent the text is (0 = opaque, 1 = fully transparent).
    ```lua
    TextTransparency = 0.2 -- Slightly transparent text
    ```
    *   *Controls the visibility of the text.*
*   **`TextXAlignment`**: `Enum.TextXAlignment` – Horizontal alignment of the text (`Left`, `Center`, `Right`).
    ```lua
    TextXAlignment = Enum.TextXAlignment.Center -- Centers text horizontally
    ```
    *   *Aligns text horizontally within the element.*
*   **`TextYAlignment`**: `Enum.TextYAlignment` – Vertical alignment of the text (`Top`, `Center`, `Bottom`).
    ```lua
    TextYAlignment = Enum.TextYAlignment.Center -- Centers text vertically
    ```
    *   *Aligns text vertically within the element.*
*   **`TextWrapped`**: `boolean` – If true, text will wrap to the next line if it exceeds the element's width.
    ```lua
    TextWrapped = true -- Text will automatically wrap
    ```
    *   *Enables text wrapping for multiline display.*
*   **`TextScaled`**: `boolean` – If true, text automatically scales to fit the element's bounds.
    ```lua
    TextScaled = true -- Text will grow/shrink to fit the element
    ```
    *   *Automatically adjusts text size to fit the element's dimensions.*
*   **`LineHeight`**: `number` – Custom line height multiplier (1.0 is default).
    ```lua
    LineHeight = 1.2 -- 20% more space between lines
    ```
    *   *Adjusts the spacing between lines of text.*

#### Input-Specific (`TextBox`)

*   **`ClearTextOnFocus`**: `boolean` – If true, text clears when the TextBox is focused.
    ```lua
    ClearTextOnFocus = true
    ```
    *   *Clears text when the user clicks/taps to type.*
*   **`MultiLine`**: `boolean` – If true, the TextBox accepts multiple lines of text.
    ```lua
    MultiLine = true
    ```
    *   *Allows the TextBox to display and accept multiple lines of input.*
*   **`PlaceholderText`**: `string` – Text displayed when the TextBox is empty and not focused.
    ```lua
    PlaceholderText = "Enter your name..."
    ```
    *   *Shows a hint when the input field is empty.*
*   **`PlaceholderColor3`**: `Color3` – Color of the placeholder text.
    ```lua
    PlaceholderColor3 = Color3.fromRGB(150, 150, 150)
    ```
    *   *Sets the color of the placeholder text.*
*   **`TextEditable`**: `boolean` – If true, the TextBox can be edited by the user.
    ```lua
    TextEditable = true
    ```
    *   *Determines if the user can modify the text.*

### 3. Image-Specific Properties (`ImageLabel`, `ImageButton`)

#### Image Source & Display

*   **`Image`**: `string` – The `rbxassetid` or content URL of the image asset.
    ```lua
    Image = "rbxassetid://1234567890" -- Your uploaded image asset ID
    ```
    *   *Sets the image displayed by the element.*
*   **`ImageColor3`**: `Color3` – Tints the image with the specified color.
    ```lua
    ImageColor3 = Color3.new(1, 0.5, 0) -- Orange tint
    ```
    *   *Applies a color tint to the image.*
*   **`ImageTransparency`**: `number` – How transparent the image content is (0 = opaque, 1 = fully transparent).
    ```lua
    ImageTransparency = 0.3 -- Slightly transparent image
    ```
    *   *Controls the visibility of the image itself.*
*   **`ScaleType`**: `Enum.ScaleType` – How the image scales within its bounds.
    *   `Stretch`: Default, image stretches to fill.
    *   `Fit`: Image scales down to fit, maintaining aspect ratio, with empty space.
    *   `Crop`: Image scales up to fill, maintaining aspect ratio, cropping parts.
    *   `Slice`: For Slice 9 images.
    *   `Tile`: Image tiles repeatedly.
    ```lua
    ScaleType = Enum.ScaleType.Slice -- Enables Slice 9 scaling
    ```
    *   *Determines how the image adjusts to the element's size.*
*   **`SliceCenter`**: `Rect` – Defines the center (stretchable) region for `Slice` `ScaleType`. Uses `X0, Y0, X1, Y1` pixel coordinates relative to the image asset.
    ```lua
    SliceCenter = Rect.new(10, 10, 90, 90) -- 10 pixel border on a 100x100 image
    ```
    *   *Specifies the stretchable area for Slice 9 images.*
*   **`TileSize`**: `UDim2` – Defines the size of each tile for `Tile` `ScaleType`.
    ```lua
    TileSize = UDim2.new(0, 32, 0, 32) -- Tiles the image in 32x32 pixel chunks
    ```
    *   *Sets the dimensions of individual tiles for tiled images.*
*   **`ImageRectOffset`**: `Vector2` – Offset within a spritesheet (`ImageRectSize` must also be set).
    ```lua
    ImageRectOffset = Vector2.new(0, 0)
    ```
    *   *Offsets the starting point for a spritesheet section.*
*   **`ImageRectSize`**: `Vector2` – Size of the image rectangle within a spritesheet.
    ```lua
    ImageRectSize = Vector2.new(64, 64) -- Defines a 64x64 pixel sprite
    ```
    *   *Defines the size of the spritesheet section.*

### 4. Layout & Arrangement Properties (`UIListLayout`, `UIGridLayout`, etc.)

These are properties passed to layout instances, often as children of a Frame.

*   **`FillDirection`**: `Enum.FillDirection` – For `UIListLayout`, determines layout direction (`Horizontal`, `Vertical`).
    ```lua
    FillDirection = Enum.FillDirection.Vertical -- Arranges items top-to-bottom
    ```
    *   *Sets the primary direction for automatic layout.*
*   **`HorizontalAlignment`**: `Enum.HorizontalAlignment` – For `UIListLayout`, aligns items horizontally within the parent.
    ```lua
    HorizontalAlignment = Enum.HorizontalAlignment.Center -- Centers items horizontally
    ```
    *   *Aligns items horizontally within their layout bounds.*
*   **`VerticalAlignment`**: `Enum.VerticalAlignment` – For `UIListLayout`, aligns items vertically within the parent.
    ```lua
    VerticalAlignment = Enum.VerticalAlignment.Center -- Centers items vertically
    ```
    *   *Aligns items vertically within the element's layout bounds.*
*   **`SortOrder`**: `Enum.SortOrder` – For `UIListLayout`/`UIGridLayout`, how children are sorted (`LayoutOrder`, `Name`, `ZIndex`).
    ```lua
    SortOrder = Enum.SortOrder.LayoutOrder -- Sorts based on LayoutOrder property
    ```
    *   *Determines the sorting method for child elements in a layout.*
*   **`Padding`**: `UDim` – Spacing between elements in a `UIListLayout` or `UIGridLayout`.
    ```lua
    Padding = UDim.new(0, 10) -- 10 pixels of padding between items
    ```
    *   *Sets the spacing between adjacent UI elements.*
*   **`CellSize`**: `UDim2` – For `UIGridLayout`, defines the size of each grid cell.
    ```lua
    CellSize = UDim2.new(0, 100, 0, 100) -- Each cell is 100x100 pixels
    ```
    *   *Sets the fixed dimensions for each cell in a grid layout.*
*   **`CellPadding`**: `UDim2` – For `UIGridLayout`, defines padding between cells.
    ```lua
    CellPadding = UDim2.new(0, 5, 0, 5) -- 5 pixel padding around each cell
    ```
    *   *Sets the spacing between cells in a grid layout.*
*   **`AspectRatio`**: `number` – For `UIAspectRatioConstraint`, maintains a fixed width-to-height ratio.
    ```lua
    AspectRatio = 16/9 -- Maintains a 16:9 aspect ratio
    ```
    *   *Ensures an element maintains a specific width-to-height proportion.*
*   **`ConstraintAxis`**: `Enum.AspectRatioAxis` – For `UIAspectRatioConstraint`, which axis (Width, Height, Both) is constrained.
    ```lua
    ConstraintAxis = Enum.AspectRatioAxis.Width -- Constrain height based on width
    ```
    *   *Defines which dimension is controlled to maintain the aspect ratio.*

### 5. ScreenGui Specific Properties

*   **`DisplayOrder`**: `number` – Determines rendering order relative to other `ScreenGui`s. Higher values are on top.
    ```lua
    DisplayOrder = 10 -- This ScreenGui will appear above others with lower DisplayOrder
    ```
    *   *Controls the stacking order of multiple ScreenGuis.*
*   **`IgnoreGuiInset`**: `boolean` – If true, the `ScreenGui` ignores the top inset reserved for Roblox's top bar (usually 36 pixels).
    ```lua
    IgnoreGuiInset = true -- UI takes up full screen, ignores Roblox's top bar
    ```
    *   *Allows the UI to extend into the area normally reserved for Roblox's top bar.*
*   **`ResetOnSpawn`**: `boolean` – If true, `ScreenGui` and its children are destroyed and recreated when the character respawns.
    ```lua
    ResetOnSpawn = false -- GUI persists across character respawns
    ```
    *   *Determines if the GUI resets when a player's character respawns.*

### 6. Events

While not "styling attributes," events are crucial configurable parameters for user interaction. These are prefixed with `React.Event.` or `React.Change.`.

*   **`[React.Event.Activated]`**: `(instance: GuiButton) -> ()` – Fired when a button is clicked/tapped.
    ```lua
    [React.Event.Activated] = function() print("Button clicked!") end
    ```
    *   *Executes a function when the button is interacted with.*
*   **`[React.Event.MouseEnter]`**: `(instance: GuiObject) -> ()`
