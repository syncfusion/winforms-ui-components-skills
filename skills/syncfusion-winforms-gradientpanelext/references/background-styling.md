# Background Styling

Comprehensive guide to configuring gradient backgrounds, patterns, colors, and images for GradientPanelExt.

## Table of Contents
- [BackgroundColor Property](#backgroundcolor-property)
- [Brush Styles](#brush-styles)
- [Gradient Styles](#gradient-styles)
- [Multi-Color Gradients](#multi-color-gradients)
- [Pattern Backgrounds](#pattern-backgrounds)
- [Background Images](#background-images)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

---

## BackgroundColor Property

The **BackgroundColor** property uses the **BrushInfo** class to configure all background styling.

**Property Type:** `Syncfusion.Drawing.BrushInfo`

### BrushInfo Constructor

```csharp
// Two-color gradient
BrushInfo brush = new BrushInfo(GradientStyle style, Color backColor, Color foreColor);

// Multi-color gradient
BrushInfo brush = new BrushInfo(GradientStyle style, Color[] gradientColors);

// Solid color
BrushInfo brush = new BrushInfo(BrushStyle.Solid, Color color, Color unused);
```

---

## Brush Styles

The **Style** property determines how the background is rendered.

**Property:** `BackgroundColor.Style`  
**Type:** `BrushStyle` enum

### Available Styles

````markdown
# Background Styling (trimmed)

This file summarizes common background options for `GradientPanelExt` with compact C# examples and a single VB sample for parity.

## BrushInfo and Styles

The `BackgroundColor` property uses `Syncfusion.Drawing.BrushInfo` to configure solid, pattern, and gradient backgrounds.

```csharp
// Two-color gradient (C# compact)
gradientPanel.BackgroundColor = new BrushInfo(GradientStyle.Horizontal, Color.DarkBlue, Color.SkyBlue);

// Solid color
gradientPanel.BackgroundColor = new BrushInfo(BrushStyle.Solid, Color.LightGray, Color.Empty);
```

**VB.NET (compact):**
```vb
' Single compact VB example kept for parity
gradientPanel.BackgroundColor = New BrushInfo(GradientStyle.Horizontal, Color.DarkBlue, Color.SkyBlue)
```

## Multi-Color and Patterns (C#)

```csharp
// Multi-color gradient
gradientPanel.BackgroundColor = new BrushInfo(GradientStyle.PathEllipse, new Color[] { Color.Red, Color.Orange, Color.Yellow });

// Pattern brush
BrushInfo pattern = new BrushInfo();
pattern.Style = BrushStyle.Pattern;
pattern.PatternStyle = PatternStyle.DiagonalCross;
pattern.BackColor = Color.White;
pattern.ForeColor = Color.DarkGoldenrod;
gradientPanel.BackgroundColor = pattern;
```

## Background Images (C#)

```csharp
gradientPanel.BackgroundImage = Properties.Resources.Texture;
gradientPanel.BackgroundImageLayout = ImageLayout.Stretch;
```

## Best Practices
- Prefer subtle two-color gradients for readability.
- Use `PathEllipse/PathRectangle` sparingly for emphasis.
- For patterns, ensure contrast for legibility.

## Related
- Getting started: [getting-started.md](getting-started.md)
- Border settings: [border-corner-settings.md](border-corner-settings.md)
- Primitives: [primitives.md](primitives.md)

````

```csharp
// Verify
System.Diagnostics.Debug.WriteLine($"Style: {panel.BackgroundColor.Style}");
System.Diagnostics.Debug.WriteLine($"GradientStyle: {panel.BackgroundColor.GradientStyle}");
```

### Colors Look Wrong

**Solution:** Use Color.FromArgb for precise RGB values

```csharp
// Precise color control
Color customBlue = Color.FromArgb(30, 144, 255);  // Dodger blue
```

### Pattern Not Showing

**Check:**
- Style is set to Pattern (not Gradient)
- BackColor and ForeColor are different

```csharp
// Correct pattern setup
brush.Style = BrushStyle.Pattern;  // Must be Pattern
brush.PatternStyle = PatternStyle.DarkDownwardDiagonal;
```

### Multi-Color Gradient Not Working

**Check:**
- Array has at least 2 colors
- Constructor with Color[] array is used

```csharp
// Correct multi-color
Color[] colors = new Color[] { Color.Red, Color.Yellow, Color.Green };
BrushInfo brush = new BrushInfo(GradientStyle.Horizontal, colors);
```

---

## Related Topics

- **Getting Started**: Basic setup → [getting-started.md](getting-started.md)
- **Border Settings**: Corners and gaps → [border-corner-settings.md](border-corner-settings.md)
- **Primitives**: Border elements → [primitives.md](primitives.md)
