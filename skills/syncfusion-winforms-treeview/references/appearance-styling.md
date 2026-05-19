# Appearance and Styling

## Table of Contents
- [Overview](#overview)
- [Border Customization](#border-customization)
- [Node Appearance](#node-appearance)
- [Themes and Visual Styles](#themes-and-visual-styles)
- [Colors and Fonts](#colors-and-fonts)
- [Styles Architecture](#styles-architecture)

## Overview

TreeViewAdv provides comprehensive appearance customization capabilities including borders, colors, fonts, themes, and node-specific styling. Customize at both control level (global) and node level (individual).

## Border Customization

### BorderStyle Property

Controls the overall border type for TreeViewAdv.

**Values:**
- `None` - No border
- `FixedSingle` - 2D border with customizable style
- `Fixed3D` - 3D border with depth effect (default)

```csharp
// No border
treeViewAdv1.BorderStyle = BorderStyle.None;

// 2D border
treeViewAdv1.BorderStyle = BorderStyle.FixedSingle;

// 3D border
treeViewAdv1.BorderStyle = BorderStyle.Fixed3D;
```

### 2D Border Customization (FixedSingle)

When `BorderStyle = FixedSingle`, customize with these properties:

**BorderSingle** - 2D border style:
- `Solid` (default)
- `Dashed`
- `Dotted`
- `Inset`
- `Outset`
- `None`

**BorderColor** - Border color

```csharp
treeViewAdv1.BorderStyle = BorderStyle.FixedSingle;
treeViewAdv1.BorderSingle = ButtonBorderStyle.Dashed;
treeViewAdv1.BorderColor = Color.SteelBlue;
```

### 3D Border Customization (Fixed3D)

When `BorderStyle = Fixed3D`, customize with:

**Border3DStyle** - 3D effect style:
- `Adjust`
- `Bump`
- `Etched`
- `Flat`
- `Raised`
- `RaisedInner`
- `RaisedOuter`
- `Sunken`
- `SunkenInner`
- `SunkenOuter`

**BorderSides** - Which sides show border:
- `All` - All sides (default)
- `Left`
- `Right`
- `Top`
- `Bottom`
- `Middle`

```csharp
treeViewAdv1.BorderStyle = BorderStyle.Fixed3D;
treeViewAdv1.Border3DStyle = Border3DStyle.Sunken;
treeViewAdv1.BorderSides = Border3DSide.All;
```

### Complete Border Examples

**Example 1: Dashed Blue 2D Border**

```csharp
treeViewAdv1.BorderStyle = BorderStyle.FixedSingle;
treeViewAdv1.BorderSingle = ButtonBorderStyle.Dashed;
treeViewAdv1.BorderColor = Color.SteelBlue;
```

**Example 2: Raised 3D Border (Top and Bottom only)**

```csharp
treeViewAdv1.BorderStyle = BorderStyle.Fixed3D;
treeViewAdv1.Border3DStyle = Border3DStyle.Raised;
treeViewAdv1.BorderSides = Border3DSide.Top | Border3DSide.Bottom;
```

**Example 3: No Border**

```csharp
treeViewAdv1.BorderStyle = BorderStyle.None;
```

## Node Appearance

### Background and Foreground Colors

**Control-Level (All Nodes):**

```csharp
treeViewAdv1.BackColor = Color.White;
treeViewAdv1.ForeColor = Color.Black;
```

### Font Customization

**Control-Level:**

```csharp
treeViewAdv1.Font = new Font("Segoe UI", 10F, FontStyle.Regular);
```

**Node-Level:**

```csharp
TreeNodeAdv node = new TreeNodeAdv("Bold Node");
node.Font = new Font("Arial", 12F, FontStyle.Bold);
```

### Selection Colors

```csharp
// Selected node background
treeViewAdv1.SelectedNodeBackground = new BrushInfo(Color.LightBlue);

// Selected node foreground
treeViewAdv1.SelectedNodeForeColor = Color.White;

// Inactive selection color (when control loses focus)
treeViewAdv1.InactiveSelectedNodeForeColor = Color.Gray;
```

### Hover Effects

```csharp
// Enable hover highlighting
treeViewAdv1.HotTracking = true;

## Themes and Visual Styles

### Office-Style Themes

TreeViewAdv supports Office and Metro themes for modern appearance.

**Available Themes:**
- `Office2016Colorful`
- `Office2016White`
- `Office2016DarkGray`
- `Office2016Black`
- `Office2019Colorful`
- `Metro`

```csharp
// Apply Office 2019 theme
treeViewAdv1.ThemeName = "Office2019Colorful";

// Or use Style property
this.treeViewAdv1.Style = TreeStyle.Office2016Colorful;
```

### Style Property

```csharp
using Syncfusion.Windows.Forms;

// Office 2016 Colorful
this.treeViewAdv1.Style = TreeStyle.Office2016Colorful;

// Metro style
this.treeViewAdv1.Style = TreeStyle.Metro;
this.treeViewAdv1.MetroColor = Color.SteelBlue;
this.treeViewAdv1.MetroArrowColorTable.ArrowNormal = Color.LightBlue;
```

### Theme Application Example

```csharp
private void ApplyOfficeTheme()
{
    // Apply Office2019Colorful theme
    treeViewAdv1.ThemeName = "Office2019Colorful";
    
    // Optional: Customize theme colors
    treeViewAdv1.Office2007ColorScheme = Syncfusion.Windows.Forms.Office2007Theme.Blue;
}
```

## Colors and Fonts

### Comprehensive Color Properties

```csharp
// Background colors
treeViewAdv1.BackColor = Color.White;
treeViewAdv1.BackgroundColor = new BrushInfo(GradientStyle.Vertical, Color.White, Color.LightGray);

// Text colors
treeViewAdv1.ForeColor = Color.Black;

// Line colors
treeViewAdv1.LineColor = Color.Gray;

// Selection colors
treeViewAdv1.SelectedNodeBackground = new BrushInfo(Color.DodgerBlue);
treeViewAdv1.SelectedNodeForeColor = Color.White;
```

### Gradient Backgrounds

```csharp
// Vertical gradient background
treeViewAdv1.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.White,
    Color.LightBlue
);

// Horizontal gradient
treeViewAdv1.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.LightYellow,
    Color.Yellow
);
```

### Font Configuration

```csharp
// Global font
treeViewAdv1.Font = new Font("Segoe UI", 10F, FontStyle.Regular);

// Node-specific fonts
TreeNodeAdv headerNode = new TreeNodeAdv("Section Header");
headerNode.Font = new Font("Segoe UI", 12F, FontStyle.Bold);

TreeNodeAdv italicNode = new TreeNodeAdv("Note");
italicNode.Font = new Font("Segoe UI", 9F, FontStyle.Italic);
```

## Styles Architecture

### Global Styles vs Node Styles

TreeViewAdv uses a hierarchical styling system:

1. **Control-Level Styles** - Applied to all nodes by default
2. **Node-Level Styles** - Override control-level for specific nodes

**Priority:** Node-level > Control-level

### Control-Level Styling

```csharp
// Apply to all nodes
treeViewAdv1.BackColor = Color.White;
treeViewAdv1.ForeColor = Color.Black;
treeViewAdv1.Font = new Font("Arial", 10F);
```

### Node-Level Styling

```csharp
// Overrides control-level for this node
TreeNodeAdv specialNode = new TreeNodeAdv("Important");
specialNode.BackColor = Color.Yellow;
specialNode.ForeColor = Color.Red;
specialNode.Font = new Font("Arial", 10F, FontStyle.Bold);
```

### Conditional Styling Example

```csharp
private void ApplyConditionalStyling()
{
    foreach (TreeNodeAdv node in treeViewAdv1.Nodes)
    {
        // Style based on node data
        if (node.Tag is FileInfo file)
        {
            if (file.Length > 1024 * 1024) // > 1MB
            {
                node.ForeColor = Color.Red;
                node.Font = new Font(treeViewAdv1.Font, FontStyle.Bold);
            }
            else
            {
                node.ForeColor = Color.Black;
                node.Font = treeViewAdv1.Font;
            }
        }
    }
}
```

### TreeNodeAdv Appearance Properties

```csharp
TreeNodeAdv node = new TreeNodeAdv("Customized Node");

// Colors
node.BackColor = Color.LightGreen;
node.ForeColor = Color.DarkGreen;

// Font
node.Font = new Font("Arial", 11F, FontStyle.Bold);

// Text alignment
node.TextAlignment = StringAlignment.Center;

// Height
node.Height = 30;
```

### Complete Styling Example

```csharp
private void SetupCustomAppearance()
{
    // Control-level defaults
    treeViewAdv1.BackColor = Color.White;
    treeViewAdv1.ForeColor = Color.Black;
    treeViewAdv1.Font = new Font("Segoe UI", 10F);
    treeViewAdv1.LineColor = Color.LightGray;
    
    // Border
    treeViewAdv1.BorderStyle = BorderStyle.FixedSingle;
    treeViewAdv1.BorderColor = Color.Gray;
    
    // Selection
    treeViewAdv1.SelectedNodeBackground = new BrushInfo(Color.DodgerBlue);
    treeViewAdv1.SelectedNodeForeColor = Color.White;
    
    // Theme
    treeViewAdv1.ThemeName = "Office2019Colorful";
    
    // Create styled nodes
    TreeNodeAdv header = new TreeNodeAdv("Header");
    header.BackColor = Color.Navy;
    header.ForeColor = Color.White;
    header.Font = new Font("Segoe UI", 12F, FontStyle.Bold);
    
    TreeNodeAdv warning = new TreeNodeAdv("Warning");
    warning.BackColor = Color.Yellow;
    warning.ForeColor = Color.Red;
    
    TreeNodeAdv success = new TreeNodeAdv("Success");
    success.ForeColor = Color.Green;
    success.Font = new Font("Segoe UI", 10F, FontStyle.Bold);
    
    treeViewAdv1.Nodes.AddRange(new[] { header, warning, success });
}
```

## Troubleshooting

**Issue:** Border not visible
- **Solution:** Verify `BorderStyle` is set to `FixedSingle` or `Fixed3D` (not `None`)

**Issue:** Border color not applying
- **Solution:** For 2D borders, ensure `BorderStyle = FixedSingle`, for 3D use `Border3DStyle` instead

**Issue:** Theme not applying
- **Solution:** Ensure `ThemeName` property value matches exact theme name (case-sensitive), or use `Style`/`VisualStyle` property

**Issue:** Node-level colors not showing
- **Solution:** Check if control-level colors are overriding - node colors should take precedence, verify colors aren't set to `Color.Empty`

**Issue:** Gradient background not visible
- **Solution:** Verify `BackgroundColor` property is set (not `BackColor`), ensure both gradient colors are different

**Issue:** Selection colors not changing
- **Solution:** Use `SelectedNodeBackground` (BrushInfo type) not `BackColor`, check if theme is overriding custom colors
