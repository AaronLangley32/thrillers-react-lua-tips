# Development Setup

Essential setup and configuration for React Lua development with Roblox.

## 🛠️ Project Structure

### Recommended Directory Layout

```
my_project/
├── src/
│   ├── client/
│   │   └── UI/
│   │       ├── MainUI.client.lua
│   │       └── Components/
│   │           ├── Button.lua
│   │           ├── Menu.lua
│   │           └── Hooks/
│   │               └── useToggleState.lua
│   ├── server/
│   │   └── ServerScript.server.lua
│   └── shared/
│       └── Types.lua
├── rojo.json
├── .luaurc
└── README.md
```

### File Naming Conventions

- **Client Scripts**: Use `.client.lua` suffix
- **Server Scripts**: Use `.server.lua` suffix
- **Module Scripts**: Use `.lua` suffix
- **Components**: PascalCase (e.g., `Button.lua`, `MenuComponent.lua`)
- **Hooks**: camelCase with `use` prefix (e.g., `useToggleState.lua`)

## 📁 Module Path Management

### Correct Module `require()` Paths with Rojo

**Problem:** A common pitfall is using `script.ModuleName` for sibling modules when the project is managed by Rojo. This often results in runtime errors.

**Why it Fails:** In Roblox Studio's hierarchy (which Rojo maps to), a module script (`script`) does not contain its sibling modules as direct children. They reside at the same level, under `script.Parent`.

- `script` refers to the current module script itself.
- `script.Parent` refers to the folder or container that the current module script is *directly inside*.

**Rule:** When `requiring` a module that is a *sibling* (at the same hierarchical level as your current script in the Roblox DataModel, as mapped by Rojo), always use `script.Parent.ModuleName`.

### Example File Structure

```
my_project/
├── src/
│   └── client/
│       └── UI/
│           ├── MainUI.client.lua   <-- Your current script
│           └── Components/        <-- Folder containing sibling modules
│               └── Button.lua
```

### Correct `require()` Calls

**In `MainUI.client.lua`:**
```lua
-- If Button.lua is a sibling to MainUI.client.lua within the Roblox DataModel (e.g., PlayerGui.UI)
local Button = require(script.Parent.Components.Button)
```

**Incorrect (would fail):**
```lua
-- This only works if 'Components' is actually nested INSIDE 'MainUI.client.lua', which is rare and not standard practice
local Button = require(script.Components.Button)
```

## ⚙️ Rojo Configuration

### Basic `rojo.json` Setup

```json
{
  "name": "my-react-lua-project",
  "tree": {
    "$className": "DataModel",
    "ReplicatedStorage": {
      "$className": "ReplicatedStorage",
      "Packages": {
        "$path": "Packages"
      }
    },
    "ServerScriptService": {
      "$className": "ServerScriptService",
      "Server": {
        "$path": "src/server"
      }
    },
    "StarterPlayer": {
      "$className": "StarterPlayer",
      "StarterPlayerScripts": {
        "$className": "StarterPlayerScripts",
        "Client": {
          "$path": "src/client"
        }
      }
    }
  }
}
```

### Advanced Configuration with React Lua

```json
{
  "name": "my-react-lua-project",
  "tree": {
    "$className": "DataModel",
    "ReplicatedStorage": {
      "$className": "ReplicatedStorage",
      "Packages": {
        "$path": "Packages"
      },
      "Shared": {
        "$path": "src/shared"
      }
    },
    "ServerScriptService": {
      "$className": "ServerScriptService",
      "Server": {
        "$path": "src/server"
      }
    },
    "StarterPlayer": {
      "$className": "StarterPlayer",
      "StarterPlayerScripts": {
        "$className": "StarterPlayerScripts",
        "Client": {
          "$path": "src/client"
        }
      }
    },
    "StarterGui": {
      "$className": "StarterGui",
      "UI": {
        "$path": "src/client/UI"
      }
    }
  }
}
```

## 🔧 Luau Configuration

### `.luaurc` File

Create a `.luaurc` file in your project root to enable strict mode globally:

```json
{
  "languageMode": "strict"
}
```

### Alternative: Inline Strict Mode

If you prefer to enable strict mode per file, add this at the top of each Lua file:

```lua
--!strict
```

## 📦 Dependencies

### Installing React Lua

1. **Using Wally (Recommended):**
   ```toml
   # wally.toml
   [dependencies]
   React = "jsdotlua/react@^1.0.0"
   ReactRoblox = "jsdotlua/react-roblox@^1.0.0"
   ```

2. **Using Rojo with Git Submodules:**
   ```bash
   git submodule add https://github.com/jsdotlua/react.git Packages/React
   git submodule add https://github.com/jsdotlua/react-roblox.git Packages/ReactRoblox
   ```

### Basic Setup Script

```lua
-- src/client/UI/MainUI.client.lua
local React = require(game:GetService("ReplicatedStorage").Packages.React)
local ReactRoblox = require(game:GetService("ReplicatedStorage").Packages.ReactRoblox)

-- Your React Lua code here
```

## 🚀 Development Workflow

### 1. Project Initialization

```bash
# Create project directory
mkdir my-react-lua-project
cd my-react-lua-project

# Initialize Rojo project
rojo init

# Create source directories
mkdir -p src/client/UI/Components
mkdir -p src/server
mkdir -p src/shared
```

### 2. Development Commands

```bash
# Start Rojo server
rojo serve

# Build project
rojo build -o game.rbxlx

# Sync to Roblox Studio
rojo upload
```

### 3. Testing Setup

```lua
-- src/client/UI/MainUI.client.lua
local React = require(game:GetService("ReplicatedStorage").Packages.React)
local ReactRoblox = require(game:GetService("ReplicatedStorage").Packages.ReactRoblox)

-- Test component
local function TestComponent()
    return React.createElement("Frame", {
        Size = UDim2.new(0, 200, 0, 100),
        Position = UDim2.fromScale(0.5, 0.5),
        AnchorPoint = Vector2.new(0.5, 0.5),
        BackgroundColor3 = Color3.fromRGB(255, 0, 0),
    }, {
        Label = React.createElement("TextLabel", {
            Text = "Hello, React Lua!",
            Size = UDim2.fromScale(1, 1),
            TextColor3 = Color3.new(1, 1, 1),
            TextScaled = true,
        })
    })
end

-- Mount component
local player = game:GetService("Players").LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

local root = ReactRoblox.createRoot(playerGui)
root:render(React.createElement(TestComponent))
```

## 🔍 Debugging Tips

### 1. Enable Strict Mode

Always use strict mode to catch type errors early:

```lua
--!strict
local React = require(game:GetService("ReplicatedStorage").Packages.React)
```

### 2. Use Type Annotations

```lua
--!strict
type Props = {
    text: string,
    onClick: () -> (),
}

local function Button(props: Props)
    return React.createElement("TextButton", {
        Text = props.text,
        [React.Event.Activated] = props.onClick,
    })
end
```

### 3. Debug Component Trees

```lua
-- Add this to debug component rendering
local function DebugComponent(props)
    print("Rendering component:", props.name)
    return React.createElement("Frame", props)
end
```

## 📚 Additional Resources

- [Rojo Documentation](https://rojo.space/)
- [React Lua GitHub](https://github.com/jsdotlua/react-lua)
- [Luau Language Reference](https://luau-lang.org/)
- [Roblox Developer Hub](https://create.roblox.com/)

---

*This setup guide provides the foundation for a robust React Lua development environment.* 