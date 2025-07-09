# UI Styling Reference

Complete reference for styling and configuring Roblox UI elements with `react-lua`. This document outlines common styling attributes and configurable parameters for Roblox UI elements when used with `react-lua`.

## 🎨 Common Visual Properties

These apply to most visual UI objects like `Frame`, `TextLabel`, `TextButton`, `ImageLabel`, `ImageButton`, `TextBox`, `ScrollingFrame`, etc.

### Size & Position

- **`Size`**: `UDim2` – Defines the size of the UI element.
  ```lua
  Size = UDim2.new(0.5, 0, 0.2, 0) -- 50% width, 20% height (scale), no pixel offset
  ```
  *Sets the dimension of the UI element.*

- **`Position`**: `UDim2` – Defines the position of the UI element relative to its parent.
  ```lua
  Position = UDim2.fromScale(0.5, 0.5) -- Centers the element (if AnchorPoint is 0.5,0.5)
  ```
  *Sets the location of the UI element.*

- **`AnchorPoint`**: `Vector2` – Sets the origin point for `Position` and rotation. (0,0 is top-left, 0.5,0.5 is center).
  ```lua
  AnchorPoint = Vector2.new(0.5, 0.5) -- Often used with Position.fromScale(0.5, 0.5) for perfect centering
  ```
  *Defines the pivot point for positioning.*

### Color & Transparency

- **`BackgroundColor3`**: `Color3` – The background color of the UI element.
  ```lua
  BackgroundColor3 = Color3.fromRGB(50, 50, 50) -- Dark gray using RGB 0-255
  ```
  *Sets the background fill color.*

- **`BackgroundTransparency`**: `number` – How transparent the background is (0 = opaque, 1 = fully transparent).
  ```lua
  BackgroundTransparency = 0.5 -- 50% transparent background
  ```
  *Controls the visibility of the background color.*

- **`BorderSizePixel`**: `number` – The thickness of the border in pixels. (Set to 0 for no border).
  ```lua
  BorderSizePixel = 0 -- No border
  ```
  *Sets the width of the pixel-based border.*

- **`BorderColor3`**: `Color3` – The color of the pixel-based border.
  ```lua
  BorderColor3 = Color3.new(1, 0, 0) -- Red border
  ```
  *Sets the color of the pixel-based border.*

- **`ClipsDescendants`**: `boolean` – If true, children outside the element's bounds will be clipped.
  ```lua
  ClipsDescendants = true -- Hides parts of children that go outside this element's boundaries
  ```
  *Controls whether child elements are clipped to the parent's bounds.*

### Visibility & Interaction

- **`Visible`**: `boolean` – If true, the UI element is visible.
  ```lua
  Visible = true -- Element is visible
  ```
  *Controls the visibility of the element and its children.*

- **`Active`**: `boolean` – If true, the element can receive user input (clicks, hovers).
  ```lua
  Active = true -- Element can receive mouse/touch input
  ```
  *Determines if the element can be interacted with.*

- **`ZIndex`**: `number` – Determines the drawing order of overlapping UI elements. Higher values draw on top.
  ```lua
  ZIndex = 5 -- Draws this element on top of elements with ZIndex < 5
  ```
  *Controls the stacking order of overlapping elements.*

## 📝 Text-Specific Properties

These properties apply to `TextLabel`, `TextButton`, and `TextBox` elements.

### Text Content & Appearance

- **`Text`**: `string` – The display text.
  ```lua
  Text = "Hello, World!"
  ```
  *Sets the string content displayed by the element.*

- **`TextColor3`**: `Color3` – The color of the text.
  ```lua
  TextColor3 = Color3.new(1, 1, 1) -- White text
  ```
  *Sets the color of the text.*

- **`TextSize`**: `number` – The font size in pixels.
  ```lua
  TextSize = 24 -- 24 pixel font size
  ```
  *Controls the size of the font.*

- **`Font`**: `Enum.Font` – The font family to use.
  ```lua
  Font = Enum.Font.SourceSansBold -- Uses a bold Sans-Serif font
  ```
  *Specifies the font style.*

- **`TextTransparency`**: `number` – How transparent the text is (0 = opaque, 1 = fully transparent).
  ```lua
  TextTransparency = 0.2 -- Slightly transparent text
  ```
  *Controls the visibility of the text.*

- **`TextXAlignment`**: `Enum.TextXAlignment` – Horizontal alignment of the text (`Left`, `Center`, `Right`).
  ```lua
  TextXAlignment = Enum.TextXAlignment.Center -- Centers text horizontally
  ```
  *Aligns text horizontally within the element.*

- **`TextYAlignment`**: `Enum.TextYAlignment` – Vertical alignment of the text (`Top`, `Center`, `Bottom`).
  ```lua
  TextYAlignment = Enum.TextYAlignment.Center -- Centers text vertically
  ```
  *Aligns text vertically within the element.*

- **`TextWrapped`**: `boolean` – If true, text will wrap to the next line if it exceeds the element's width.
  ```lua
  TextWrapped = true -- Text will automatically wrap
  ```
  *Enables text wrapping for multiline display.*

- **`TextScaled`**: `boolean` – If true, text automatically scales to fit the element's bounds.
  ```lua
  TextScaled = true -- Text will grow/shrink to fit the element
  ```
  *Automatically adjusts text size to fit the element's dimensions.*

- **`LineHeight`**: `number` – Custom line height multiplier (1.0 is default).
  ```lua
  LineHeight = 1.2 -- 20% more space between lines
  ```
  *Adjusts the spacing between lines of text.*

### Input-Specific (`TextBox`)

- **`ClearTextOnFocus`**: `boolean` – If true, text clears when the TextBox is focused.
  ```lua
  ClearTextOnFocus = true
  ```
  *Clears text when the user clicks/taps to type.*

- **`MultiLine`**: `boolean` – If true, the TextBox accepts multiple lines of text.
  ```lua
  MultiLine = true
  ```
  *Allows the TextBox to display and accept multiple lines of input.*

- **`PlaceholderText`**: `string` – Text displayed when the TextBox is empty and not focused.
  ```lua
  PlaceholderText = "Enter your name..."
  ```
  *Shows a hint when the input field is empty.*

- **`PlaceholderColor3`**: `Color3` – Color of the placeholder text.
  ```lua
  PlaceholderColor3 = Color3.fromRGB(150, 150, 150)
  ```
  *Sets the color of the placeholder text.*

- **`TextEditable`**: `boolean` – If true, the TextBox can be edited by the user.
  ```lua
  TextEditable = true
  ```
  *Determines if the user can modify the text.*

## 🖼️ Image-Specific Properties

These properties apply to `ImageLabel` and `ImageButton` elements.

### Image Source & Display

- **`Image`**: `string` – The `rbxassetid` or content URL of the image asset.
  ```lua
  Image = "rbxassetid://1234567890" -- Your uploaded image asset ID
  ```
  *Sets the image displayed by the element.*

- **`ImageColor3`**: `Color3` – Tints the image with the specified color.
  ```lua
  ImageColor3 = Color3.new(1, 0.5, 0) -- Orange tint
  ```
  *Applies a color tint to the image.*

- **`ImageTransparency`**: `number` – How transparent the image content is (0 = opaque, 1 = fully transparent).
  ```lua
  ImageTransparency = 0.3 -- Slightly transparent image
  ```
  *Controls the visibility of the image itself.*

- **`ScaleType`**: `Enum.ScaleType` – How the image scales within its bounds.
  - `Stretch`: Default, image stretches to fill.
  - `Fit`: Image scales down to fit, maintaining aspect ratio, with empty space.
  - `Crop`: Image scales up to fill, maintaining aspect ratio, cropping parts.
  - `Slice`: For Slice 9 images.
  - `Tile`: Image tiles repeatedly.
  ```lua
  ScaleType = Enum.ScaleType.Slice -- Enables Slice 9 scaling
  ```
  *Determines how the image adjusts to the element's size.*

- **`SliceCenter`**: `Rect` – Defines the center (stretchable) region for `Slice` `ScaleType`. Uses `X0, Y0, X1, Y1` pixel coordinates relative to the image asset.
  ```lua
  SliceCenter = Rect.new(10, 10, 90, 90) -- 10 pixel border on a 100x100 image
  ```
  *Specifies the stretchable area for Slice 9 images.*

- **`TileSize`**: `UDim2` – Defines the size of each tile for `Tile` `ScaleType`.
  ```lua
  TileSize = UDim2.new(0, 32, 0, 32) -- Tiles the image in 32x32 pixel chunks
  ```
  *Sets the dimensions of individual tiles for tiled images.*

- **`ImageRectOffset`**: `Vector2` – Offset within a spritesheet (`ImageRectSize` must also be set).
  ```lua
  ImageRectOffset = Vector2.new(0, 0)
  ```
  *Offsets the starting point for a spritesheet section.*

- **`ImageRectSize`**: `Vector2` – Size of the image rectangle within a spritesheet.
  ```lua
  ImageRectSize = Vector2.new(64, 64) -- Defines a 64x64 pixel sprite
  ```
  *Defines the size of the spritesheet section.*

## 📐 Layout & Arrangement Properties

These are properties passed to layout instances, often as children of a Frame.

- **`FillDirection`**: `Enum.FillDirection` – For `UIListLayout`, determines layout direction (`Horizontal`, `Vertical`).
  ```lua
  FillDirection = Enum.FillDirection.Vertical -- Arranges items top-to-bottom
  ```
  *Sets the primary direction for automatic layout.*

- **`HorizontalAlignment`**: `Enum.HorizontalAlignment` – For `UIListLayout`, aligns items horizontally within the parent.
  ```lua
  HorizontalAlignment = Enum.HorizontalAlignment.Center -- Centers items horizontally
  ```
  *Aligns items horizontally within their layout bounds.*

- **`VerticalAlignment`**: `Enum.VerticalAlignment` – For `UIListLayout`, aligns items vertically within the parent.
  ```lua
  VerticalAlignment = Enum.VerticalAlignment.Center -- Centers items vertically
  ```
  *Aligns items vertically within the element's layout bounds.*

- **`SortOrder`**: `Enum.SortOrder` – For `UIListLayout`/`UIGridLayout`, how children are sorted (`LayoutOrder`, `Name`, `ZIndex`).
  ```lua
  SortOrder = Enum.SortOrder.LayoutOrder -- Sorts based on LayoutOrder property
  ```
  *Determines the sorting method for child elements in a layout.*

- **`Padding`**: `UDim` – Spacing between elements in a `UIListLayout` or `UIGridLayout`.
  ```lua
  Padding = UDim.new(0, 10) -- 10 pixels of padding between items
  ```
  *Sets the spacing between adjacent UI elements.*

- **`CellSize`**: `UDim2` – For `UIGridLayout`, defines the size of each grid cell.
  ```lua
  CellSize = UDim2.new(0, 100, 0, 100) -- Each cell is 100x100 pixels
  ```
  *Sets the fixed dimensions for each cell in a grid layout.*

- **`CellPadding`**: `UDim2` – For `UIGridLayout`, defines padding between cells.
  ```lua
  CellPadding = UDim2.new(0, 5, 0, 5) -- 5 pixel padding around each cell
  ```
  *Sets the spacing between cells in a grid layout.*

- **`AspectRatio`**: `number` – For `UIAspectRatioConstraint`, maintains a fixed width-to-height ratio.
  ```lua
  AspectRatio = 16/9 -- Maintains a 16:9 aspect ratio
  ```
  *Ensures an element maintains a specific width-to-height proportion.*

- **`ConstraintAxis`**: `Enum.AspectRatioAxis` – For `UIAspectRatioConstraint`, which axis (Width, Height, Both) is constrained.
  ```lua
  ConstraintAxis = Enum.AspectRatioAxis.Width -- Constrain height based on width
  ```
  *Defines which dimension is controlled to maintain the aspect ratio.*

## 🖥️ ScreenGui Specific Properties

- **`DisplayOrder`**: `number` – Determines rendering order relative to other `ScreenGui`s. Higher values are on top.
  ```lua
  DisplayOrder = 10 -- This ScreenGui will appear above others with lower DisplayOrder
  ```
  *Controls the stacking order of multiple ScreenGuis.*

- **`IgnoreGuiInset`**: `boolean` – If true, the `ScreenGui` ignores the top inset reserved for Roblox's top bar (usually 36 pixels).
  ```lua
  IgnoreGuiInset = true -- UI takes up full screen, ignores Roblox's top bar
  ```
  *Allows the UI to extend into the area normally reserved for Roblox's top bar.*

- **`ResetOnSpawn`**: `boolean` – If true, `ScreenGui` and its children are destroyed and recreated when the character respawns.
  ```lua
  ResetOnSpawn = false -- GUI persists across character respawns
  ```
  *Determines if the GUI resets when a player's character respawns.*

## 🎮 Events

While not "styling attributes," events are crucial configurable parameters for user interaction. These are prefixed with `React.Event.` or `React.Change.`.

- **`[React.Event.Activated]`**: `(instance: GuiButton) -> ()` – Fired when a button is clicked/tapped.
  ```lua
  [React.Event.Activated] = function() print("Button clicked!") end
  ```
  *Executes a function when the button is interacted with.*

- **`[React.Event.MouseEnter]`**: `(instance: GuiObject) -> ()` – Fired when the mouse enters the element.
  ```lua
  [React.Event.MouseEnter] = function() print("Mouse entered!") end
  ```
  *Executes a function when the mouse enters the element.*

- **`[React.Event.MouseLeave]`**: `(instance: GuiObject) -> ()` – Fired when the mouse leaves the element.
  ```lua
  [React.Event.MouseLeave] = function() print("Mouse left!") end
  ```
  *Executes a function when the mouse leaves the element.*

- **`[React.Change.Text]`**: `(instance: TextBox) -> ()` – Fired when text changes in a TextBox.
  ```lua
  [React.Change.Text] = function(textBox) print("Text changed to:", textBox.Text) end
  ```
  *Executes a function when the text content changes.*

## 🎨 Color Reference

### Color3 Creation Methods

- **`Color3.new(r, g, b)`**: Creates a color using RGB values from 0-1
  ```lua
  Color3.new(1, 0, 0) -- Red
  Color3.new(0, 1, 0) -- Green
  Color3.new(0, 0, 1) -- Blue
  ```

- **`Color3.fromRGB(r, g, b)`**: Creates a color using RGB values from 0-255
  ```lua
  Color3.fromRGB(255, 0, 0) -- Red
  Color3.fromRGB(0, 255, 0) -- Green
  Color3.fromRGB(0, 0, 255) -- Blue
  ```

- **`Color3.fromHex(hex)`**: Creates a color from a hex string
  ```lua
  Color3.fromHex("#FF0000") -- Red
  Color3.fromHex("#00FF00") -- Green
  Color3.fromHex("#0000FF") -- Blue
  ```

## 📋 Common Patterns

### Centering Elements
```lua
return React.createElement("Frame", {
    AnchorPoint = Vector2.new(0.5, 0.5),
    Position = UDim2.fromScale(0.5, 0.5),
    Size = UDim2.new(0, 200, 0, 100),
})
```

### Responsive Sizing
```lua
return React.createElement("Frame", {
    Size = UDim2.new(0.8, 0, 0.6, 0), -- 80% width, 60% height
    Position = UDim2.fromScale(0.1, 0.2), -- 10% from left, 20% from top
})
```

### Button with Hover Effect
```lua
local isHovered, setIsHovered = React.useState(false)

return React.createElement("TextButton", {
    Text = "Click Me",
    BackgroundColor3 = isHovered and Color3.fromRGB(100, 100, 100) or Color3.fromRGB(50, 50, 50),
    [React.Event.MouseEnter] = function() setIsHovered(true) end,
    [React.Event.MouseLeave] = function() setIsHovered(false) end,
})
```

---

*This reference covers the most commonly used properties for Roblox UI development with React Lua.* 