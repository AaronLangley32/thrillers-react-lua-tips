
### 1. Vector Graphics (SVGs) Not Supported

Roblox Studio **does not natively support vector graphics (SVGs)** for UI elements. The UI system primarily uses **raster image formats** (PNG, JPG, TGA, BMP).

### 2. Recommended File Type: High-Resolution PNGs

- **Best Practice:** Design UI assets in vector software (e.g., Illustrator, Figma, Inkscape).
- **Export As:** High-resolution PNG files with transparent backgrounds. PNGs offer lossless quality and transparency.

### 3. Recommended Element Sizes (Design Reference)

These are general visual targets; final display size depends on UI layout:

- **Small Icons:** 32x32px to 64x64px
- **Medium Icons:** 64x64px to 128x128px
- **Large Icons/Elements:** 128x128px and up

### 4. Leverage Roblox's Built-In Scaling

- **Export High-Resolution:** Export PNGs at a significantly higher resolution than their anticipated in-game display size (e.g., 512x512px or 1024x1024px for a 64x64px expected display).
- **How it Works:** When a high-res PNG is applied to an ImageLabel or ImageButton, Roblox's engine automatically downscales it to fit the UI element's dimensions.
- **Benefit:** Downscaling from a high-resolution source results in a much sharper, less pixelated appearance, mimicking vector-like crispness without exporting multiple file sizes.

### 5. Dynamic Scaling with Slice 9

- **Purpose:** For UI elements (buttons, frames, panels) that need to stretch while maintaining consistent borders and appearance.
- **How it Works:** Defines nine regions (corners, edges, center) of an image. When resized:
    - **Corners:** Remain fixed.
    - **Edges:** Stretch along their axis.
    - **Center:** Tiles or stretches to fill space.
- **Configuration:** Set ImageLabel or ImageButton's ScaleType to Slice and adjust the SliceCenter property.
