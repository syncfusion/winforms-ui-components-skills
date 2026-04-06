# Customization and Styling

## Table of Contents
- [Overview](#overview)
- [Background Customization](#background-customization)
  - [TransparentBackground](#transparentbackground)
  - [BackColor](#backcolor)
  - [BackgroundImage](#backgroundimage)
- [Grid Lines](#grid-lines)
  - [DisplayHorzLines and DisplayVertLines](#displayhorzlines-and-displayvertlines)
  - [GridLineColor](#gridlinecolor)
- [Header Customization](#header-customization)
  - [Buttons3D](#buttons3d)
  - [HeaderBackColor](#headerbackcolor)
  - [HeaderTextColor](#headertextcolor)
- [Visual Styles](#visual-styles)
- [Resizing Features](#resizing-features)
- [Tooltip Support](#tooltip-support)
- [Touch Support](#touch-support)
- [Customization Best Practices](#customization-best-practices)
- [Common Customization Patterns](#common-customization-patterns)

---

## Overview

GridListControl provides extensive customization options to match your application's look and feel. You can customize:
- Background colors and transparency
- Grid line visibility and colors
- Header appearance and colors
- Visual styles and themes
- Row and column sizing
- Tooltips and hover effects
- Touch-friendly interactions

All customization can be done through properties at design time or runtime.

## Background Customization

### TransparentBackground

Controls whether the grid cells display a background color or are transparent.

**Property:** `TransparentBackground`  
**Type:** bool  
**Default:** false

```csharp
this.gridListControl1.TransparentBackground = true;
```

**VB.NET:**
```vb
Me.gridListControl1.TransparentBackground = True
```

**Behavior:**
- **true:** No background color is displayed, cells are transparent
- **false:** Background is filled with the BackColor

**When to use:**
- Layered UI designs
- Custom painted backgrounds
- Glass-style interfaces
- Overlay effects

**Performance Note:** Transparent backgrounds may impact rendering performance. Use only when necessary.

### BackColor

Sets the background color for the GridListControl.

**Property:** `BackColor`  
**Type:** Color  
**Default:** Control (system default)

**⚠️ Important:** TransparentBackground must be set to **false** for BackColor to take effect.

```csharp
this.gridListControl1.TransparentBackground = false;
this.gridListControl1.BackColor = Color.Beige;
```

**VB.NET:**
```vb
Me.gridListControl1.TransparentBackground = False
Me.gridListControl1.BackColor = Color.Beige
```

**Example Colors:**
```csharp
// Predefined colors
gridListControl1.BackColor = Color.AliceBlue;
gridListControl1.BackColor = Color.LightGray;
gridListControl1.BackColor = Color.WhiteSmoke;

// Custom RGB colors
gridListControl1.BackColor = Color.FromArgb(245, 245, 245);

// Theme colors
gridListControl1.BackColor = SystemColors.Window;
```

**Best Practices:**
- Use light colors for better text readability
- Match application theme colors
- Test with different Windows themes
- Consider high-contrast mode requirements

### BackgroundImage

Sets a background image for the control.

**Property:** `BackgroundImage`  
**Type:** Image  
**Default:** null

```csharp
this.gridListControl1.BackgroundImage = Image.FromFile("Cloud.jpg");
```

**VB.NET:**
```vb
Me.gridListControl1.BackgroundImage = Image.FromFile("Cloud.jpg")
```

**Loading from Resources:**
```csharp
// From embedded resource
this.gridListControl1.BackgroundImage = Properties.Resources.CloudBackground;

// From file
this.gridListControl1.BackgroundImage = Image.FromFile(@"C:\Images\background.png");

// From stream
using (var stream = File.OpenRead("background.jpg"))
{
    this.gridListControl1.BackgroundImage = Image.FromStream(stream);
}
```

**Image Layout:**
```csharp
// Control image layout
this.gridListControl1.BackgroundImageLayout = ImageLayout.Stretch;
// Options: None, Tile, Center, Stretch, Zoom
```

**When to use:**
- Branded interfaces
- Watermarks
- Decorative backgrounds
- Theme consistency

**Considerations:**
- Use appropriate image sizes to avoid performance issues
- Ensure text remains readable over images
- Consider using subtle, low-contrast images
- Test with different screen resolutions

## Grid Lines

### DisplayHorzLines and DisplayVertLines

Controls the visibility of horizontal and vertical grid lines.

**Properties:** 
- `Properties.DisplayHorzLines` (bool)
- `Properties.DisplayVertLines` (bool)

**Default:** false (no grid lines)

#### Horizontal Lines

```csharp
this.gridListControl1.Properties.DisplayHorzLines = true;
```

**VB.NET:**
```vb
Me.gridListControl1.Properties.DisplayHorzLines = True
```

**Effect:** Displays horizontal lines between rows, creating a ruled appearance.

#### Vertical Lines

```csharp
this.gridListControl1.Properties.DisplayVertLines = true;
```

**VB.NET:**
```vb
Me.gridListControl1.Properties.DisplayVertLines = True
```

**Effect:** Displays vertical lines between columns, separating data visually.

#### Both Lines

```csharp
// Show full grid
this.gridListControl1.Properties.DisplayHorzLines = true;
this.gridListControl1.Properties.DisplayVertLines = true;
```

**Design Considerations:**

| Grid Line Configuration | Best For |
|------------------------|----------|
| No lines | Clean, modern look |
| Horizontal only | Row-focused data (spreadsheet style) |
| Vertical only | Column-focused data (comparison tables) |
| Both lines | Dense data, accounting, detailed tables |

### GridLineColor

Customizes the color of grid lines.

**Property:** `Grid.Properties.GridLineColor`  
**Type:** Color  
**Default:** System default (typically gray)

```csharp
this.gridListControl1.Grid.Properties.GridLineColor = Color.Blue;
```

**VB.NET:**
```vb
Me.gridListControl1.Grid.Properties.GridLineColor = Color.Blue
```

**Common Color Schemes:**
```csharp
// Subtle lines
gridListControl1.Grid.Properties.GridLineColor = Color.LightGray;

// Prominent lines
gridListControl1.Grid.Properties.GridLineColor = Color.Black;

// Themed lines
gridListControl1.Grid.Properties.GridLineColor = Color.FromArgb(200, 200, 200);

// Brand colors
gridListControl1.Grid.Properties.GridLineColor = Color.FromArgb(0, 120, 215); // Blue accent
```

**Best Practices:**
- Use subtle colors for better readability
- Ensure grid lines don't overpower content
- Match line colors to overall theme
- Test contrast ratios for accessibility

## Header Customization

### Buttons3D

Controls the appearance of row and column headers with a 3D raised effect.

**Property:** `Properties.Buttons3D`  
**Type:** bool  
**Default:** false

```csharp
this.gridListControl1.Properties.Buttons3D = true;
```

**VB.NET:**
```vb
Me.gridListControl1.Properties.Buttons3D = True
```

**Visual Effect:**
- **true:** Headers render with a 3D raised appearance (button-like)
- **false:** Headers render flat

**When to use:**
- Traditional Windows applications
- Classic/business themes
- Applications targeting users familiar with older UIs

**Modern Alternative:**
Consider using flat headers (`Buttons3D = false`) with custom colors for a modern look.

### HeaderBackColor

Sets the background color of column headers.

**Property:** `HeaderBackColor`  
**Type:** Color  
**Default:** System default

```csharp
this.gridListControl1.HeaderBackColor = Color.Red;
```

**VB.NET:**
```vb
Me.gridListControl1.HeaderBackColor = Color.Red
```

**Common Patterns:**
```csharp
// Professional blue
gridListControl1.HeaderBackColor = Color.FromArgb(0, 120, 215);

// Subtle gray
gridListControl1.HeaderBackColor = Color.FromArgb(240, 240, 240);

// Brand colors
gridListControl1.HeaderBackColor = Color.DarkBlue;

// System theme
gridListControl1.HeaderBackColor = SystemColors.Control;
```

### HeaderTextColor

Sets the text color of column headers.

**Property:** `HeaderTextColor`  
**Type:** Color  
**Default:** System default

```csharp
this.gridListControl1.HeaderTextColor = Color.Blue;
```

**VB.NET:**
```vb
Me.gridListControl1.HeaderTextColor = Color.Blue
```

**Color Coordination:**
```csharp
// Dark text on light background
gridListControl1.HeaderBackColor = Color.LightGray;
gridListControl1.HeaderTextColor = Color.Black;

// Light text on dark background
gridListControl1.HeaderBackColor = Color.DarkBlue;
gridListControl1.HeaderTextColor = Color.White;

// High contrast
gridListControl1.HeaderBackColor = Color.Black;
gridListControl1.HeaderTextColor = Color.Yellow;
```

**Accessibility:** Ensure sufficient contrast between HeaderBackColor and HeaderTextColor (WCAG AA: 4.5:1 for normal text).

## Visual Styles

GridListControl supports multiple visual styles for consistent theming.

**Key Features:**
- Office2016 themes
- Metro styles
- Custom color schemes
- System theme integration

**Applying Visual Styles:**
```csharp
// Visual styles are typically applied at the application level
// or through the control's style properties
```

**Note:** Specific visual style APIs depend on the Syncfusion version. Refer to the Syncfusion documentation for your version's theming capabilities.

## Resizing Features

GridListControl supports automatic resizing of columns and rows based on content.

**Features:**
- Auto-resize columns to fit content
- Auto-resize rows based on text height
- Manual column width adjustment
- Manual row height adjustment

**Column Resizing:**
```csharp
// Enable column resizing
// Users can drag column headers to resize

// Fill last column to available space
gridListControl1.FillLastColumn = true;
```

**Use Cases:**
- Dynamic content with varying text lengths
- User-customizable layouts
- Responsive designs
- Multi-resolution displays

## Tooltip Support

GridListControl supports displaying tooltips on mouse hover over cells.

**Features:**
- Cell-level tooltips
- Custom tooltip text
- Hover delay configuration

**When to use:**
- Display additional information on hover
- Show truncated text in full
- Provide help text for complex data
- Display comments or notes

**Implementation:**
Tooltip configuration is done through the control's tooltip properties. Hover over cells to display comment text.

## Touch Support

GridListControl includes touch-optimized features for modern Windows devices.

**Touch Features:**
- Touch-friendly selection
- Swipe gestures
- Optimized hit targets
- Smooth scrolling

**Supported Gestures:**
- **Tap:** Select row
- **Swipe:** Scroll content
- **Pinch:** (if zoom enabled)

**Enabling Touch Support:**
Touch support is built-in and automatically active on touch-enabled devices.

**Best Practices for Touch:**
- Use larger row heights (minimum 40px)
- Increase padding for better tap targets
- Enable MultiSimple selection for easier multi-select
- Test on actual touch devices

## Customization Best Practices

### 1. Theme Consistency

Apply consistent colors across all UI elements:

```csharp
// Define theme colors
Color primaryColor = Color.FromArgb(0, 120, 215);
Color backgroundColor = Color.White;
Color textColor = Color.Black;

// Apply to GridListControl
gridListControl1.BackColor = backgroundColor;
gridListControl1.HeaderBackColor = primaryColor;
gridListControl1.HeaderTextColor = Color.White;
gridListControl1.Grid.Properties.GridLineColor = Color.LightGray;
```

### 2. Performance Optimization

```csharp
// Suspend layout during bulk customization
gridListControl1.SuspendLayout();

gridListControl1.TransparentBackground = false;
gridListControl1.BackColor = Color.White;
gridListControl1.Properties.DisplayHorzLines = true;
gridListControl1.Properties.DisplayVertLines = true;
// ... more properties

gridListControl1.ResumeLayout();
```

### 3. Accessibility Considerations

```csharp
// Ensure sufficient contrast
// WCAG AA: 4.5:1 for normal text, 3:1 for large text

// Good contrast example
gridListControl1.BackColor = Color.White;
gridListControl1.HeaderBackColor = Color.FromArgb(0, 51, 153); // Dark blue
gridListControl1.HeaderTextColor = Color.White;
```

### 4. Responsive Design

```csharp
// Handle form resize
private void Form1_Resize(object sender, EventArgs e)
{
    // GridListControl automatically adjusts
    // if FillLastColumn is true
    gridListControl1.FillLastColumn = true;
}
```

## Common Customization Patterns

### Pattern 1: Professional Business Theme

```csharp
gridListControl1.TransparentBackground = false;
gridListControl1.BackColor = Color.White;
gridListControl1.HeaderBackColor = Color.FromArgb(240, 240, 240);
gridListControl1.HeaderTextColor = Color.FromArgb(51, 51, 51);
gridListControl1.Properties.DisplayHorzLines = true;
gridListControl1.Properties.DisplayVertLines = false;
gridListControl1.Grid.Properties.GridLineColor = Color.FromArgb(230, 230, 230);
gridListControl1.Properties.Buttons3D = false;
```

### Pattern 2: Dark Theme

```csharp
gridListControl1.TransparentBackground = false;
gridListControl1.BackColor = Color.FromArgb(30, 30, 30);
gridListControl1.HeaderBackColor = Color.FromArgb(45, 45, 48);
gridListControl1.HeaderTextColor = Color.White;
gridListControl1.Properties.DisplayHorzLines = true;
gridListControl1.Properties.DisplayVertLines = true;
gridListControl1.Grid.Properties.GridLineColor = Color.FromArgb(60, 60, 60);
```

### Pattern 3: Minimal Clean Look

```csharp
gridListControl1.TransparentBackground = false;
gridListControl1.BackColor = Color.White;
gridListControl1.HeaderBackColor = Color.White;
gridListControl1.HeaderTextColor = Color.Gray;
gridListControl1.Properties.DisplayHorzLines = true;
gridListControl1.Properties.DisplayVertLines = false;
gridListControl1.Grid.Properties.GridLineColor = Color.FromArgb(245, 245, 245);
gridListControl1.Properties.Buttons3D = false;
```

### Pattern 4: High Contrast

```csharp
gridListControl1.TransparentBackground = false;
gridListControl1.BackColor = Color.Black;
gridListControl1.HeaderBackColor = Color.Yellow;
gridListControl1.HeaderTextColor = Color.Black;
gridListControl1.Properties.DisplayHorzLines = true;
gridListControl1.Properties.DisplayVertLines = true;
gridListControl1.Grid.Properties.GridLineColor = Color.White;
```

### Pattern 5: Branded Theme

```csharp
// Using company brand colors
Color brandPrimary = Color.FromArgb(0, 120, 215);  // Company blue
Color brandSecondary = Color.FromArgb(245, 245, 245);

gridListControl1.TransparentBackground = false;
gridListControl1.BackColor = Color.White;
gridListControl1.HeaderBackColor = brandPrimary;
gridListControl1.HeaderTextColor = Color.White;
gridListControl1.Properties.DisplayHorzLines = true;
gridListControl1.Grid.Properties.GridLineColor = brandSecondary;
```

## Troubleshooting Customization

**BackColor not applying**
- Ensure TransparentBackground is false
- Check if BackgroundImage is overriding color
- Verify control is properly initialized

**Grid lines not visible**
- Set DisplayHorzLines/DisplayVertLines to true
- Check GridLineColor contrast with BackColor
- Ensure control is rendering properly

**Headers not showing custom colors**
- Verify ShowColumnHeader is true
- Check if visual styles are overriding colors
- Ensure properties are set after DataSource binding

**Background image not displaying**
- Verify image file path is correct
- Check if image file exists
- Ensure image format is supported (PNG, JPG, BMP)
- Try setting BackgroundImageLayout property

**Performance issues after customization**
- Disable TransparentBackground if not needed
- Remove BackgroundImage if causing slowdown
- Simplify grid line usage
- Use BeginUpdate/EndUpdate for bulk changes
