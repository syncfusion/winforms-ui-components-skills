# Background Styling

Comprehensive guide to configuring gradient backgrounds, patterns, and color effects in GradientLabel using the BackgroundColor property and BrushInfo class.

## Table of Contents
- [BackgroundColor Property Overview](#backgroundcolor-property-overview)
- [Brush Styles](#brush-styles)
- [Gradient Styles](#gradient-styles)
- [Multi-Color Gradients](#multi-color-gradients)
- [Pattern Backgrounds](#pattern-backgrounds)
- [Complete Examples](#complete-examples)

---

## BackgroundColor Property Overview

The **BackgroundColor** property is the key to customizing GradientLabel appearance. It uses the **BrushInfo** class from `Syncfusion.Drawing` namespace.

**Property Type:** `BrushInfo`  
**Namespace:** `Syncfusion.Drawing`

### BrushInfo Class Properties

| Property | Type | Description |
|----------|------|-------------|
| **Style** | `BrushStyle` | Brush style (Solid, Pattern, Gradient, None) |
| **BackColor** | `Color` | Starting color (or solid color) |
| **ForeColor** | `Color` | Ending color for gradients |
| **GradientStyle** | `GradientStyle` | Direction/shape of gradient |
| **PatternStyle** | `PatternStyle` | Pattern when using Pattern style |
| **GradientColors** | `Color[]` | Array for multi-color gradients |

---

## Brush Styles

The **Style** property determines the type of background effect.

### Available Brush Styles

| Style | Description | Use Case |
|-------|-------------|----------|
| **Gradient** | Color transition effect | Headers, decorative labels |
| **Solid** | Single flat color | Simple colored backgrounds |
| **Pattern** | Textured pattern fill | Decorative effects |
| **None** | No background | Transparent label |

---

### Gradient Style (Default)

Creates smooth color transitions.

**C# Example:**
```csharp
using Syncfusion.Drawing;

// Create gradient background
gradientLabel.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.Red,
    Color.Blue
);
```

**VB.NET Example:**
```vb
Imports Syncfusion.Drawing

' Create gradient background
gradientLabel.BackgroundColor = New BrushInfo(
    GradientStyle.Horizontal,
    Color.Red,
    Color.Blue
)
```

---

### Solid Style

Single flat color background.

**C# Example:**
```csharp
// Solid color background
gradientLabel.BackgroundColor = new BrushInfo(BrushStyle.Solid);
gradientLabel.BackgroundColor.BackColor = Color.LightBlue;
```

**VB.NET Example:**
```vb
' Solid color background
gradientLabel.BackgroundColor = New BrushInfo(BrushStyle.Solid)
gradientLabel.BackgroundColor.BackColor = Color.LightBlue
```

---

### Pattern Style

Textured pattern backgrounds.

**C# Example:**
```csharp
// Pattern background
gradientLabel.BackgroundColor = new BrushInfo(BrushStyle.Pattern);
gradientLabel.BackgroundColor.PatternStyle = PatternStyle.DarkDownwardDiagonal;
gradientLabel.BackgroundColor.BackColor = Color.White;
gradientLabel.BackgroundColor.ForeColor = Color.Gray;
```

**VB.NET Example:**
```vb
' Pattern background
gradientLabel.BackgroundColor = New BrushInfo(BrushStyle.Pattern)
gradientLabel.BackgroundColor.PatternStyle = PatternStyle.DarkDownwardDiagonal
gradientLabel.BackgroundColor.BackColor = Color.White
gradientLabel.BackgroundColor.ForeColor = Color.Gray
```

---

### None Style

No background (transparent).

**C# Example:**
```csharp
// No background
gradientLabel.BackgroundColor = new BrushInfo(BrushStyle.None);
```

**VB.NET Example:**
```vb
' No background
gradientLabel.BackgroundColor = New BrushInfo(BrushStyle.None)
```

---

## Gradient Styles

The **GradientStyle** property controls the direction and shape of color transitions.

### Available Gradient Styles

| Style | Description | Visual Effect |
|-------|-------------|---------------|
| **Horizontal** | Left to right transition | → |
| **Vertical** | Top to bottom transition | ↓ |
| **ForwardDiagonal** | Top-left to bottom-right | ↘ |
| **BackwardDiagonal** | Top-right to bottom-left | ↙ |
| **PathRectangle** | Center outward (rectangular) | ⊡ |
| **PathEllipse** | Center outward (elliptical) | ◯ |

---

### Horizontal Gradient

Left-to-right color transition.

**C# Example:**
```csharp
gradientLabel.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.Navy,
    Color.Cyan
);
```

**VB.NET Example:**
```vb
gradientLabel.BackgroundColor = New BrushInfo(
    GradientStyle.Horizontal,
    Color.Navy,
    Color.Cyan
)
```

**Best For:** Wide labels, horizontal headers

---

### Vertical Gradient

Top-to-bottom color transition.

**C# Example:**
```csharp
gradientLabel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.DarkGreen,
    Color.LightGreen
);
```

**VB.NET Example:**
```vb
gradientLabel.BackgroundColor = New BrushInfo(
    GradientStyle.Vertical,
    Color.DarkGreen,
    Color.LightGreen
)
```

**Best For:** Tall labels, vertical menu items

---

### ForwardDiagonal Gradient

Top-left to bottom-right diagonal transition.

**C# Example:**
```csharp
gradientLabel.BackgroundColor = new BrushInfo(
    GradientStyle.ForwardDiagonal,
    Color.Purple,
    Color.Pink
);
```

**VB.NET Example:**
```vb
gradientLabel.BackgroundColor = New BrushInfo(
    GradientStyle.ForwardDiagonal,
    Color.Purple,
    Color.Pink
)
```

**Best For:** Dynamic, eye-catching effects

---

### BackwardDiagonal Gradient

Top-right to bottom-left diagonal transition.

**C# Example:**
```csharp
gradientLabel.BackgroundColor = new BrushInfo(
    GradientStyle.BackwardDiagonal,
    Color.Maroon,
    Color.Orange
);
```

**VB.NET Example:**
```vb
gradientLabel.BackgroundColor = New BrushInfo(
    GradientStyle.BackwardDiagonal,
    Color.Maroon,
    Color.Orange
)
```

**Best For:** Opposite diagonal effect, variety

---

### PathRectangle Gradient

Radiates from center outward in rectangular path.

**C# Example:**
```csharp
gradientLabel.BackgroundColor = new BrushInfo(
    GradientStyle.PathRectangle,
    Color.DarkBlue,
    Color.LightBlue
);
```

**VB.NET Example:**
```vb
gradientLabel.BackgroundColor = New BrushInfo(
    GradientStyle.PathRectangle,
    Color.DarkBlue,
    Color.LightBlue
)
```

**Best For:** Centered focus, spotlight effect

---

### PathEllipse Gradient

Radiates from center outward in elliptical path.

**C# Example:**
```csharp
gradientLabel.BackgroundColor = new BrushInfo(
    GradientStyle.PathEllipse,
    Color.Gold,
    Color.DarkGoldenrod
);
```

**VB.NET Example:**
```vb
gradientLabel.BackgroundColor = New BrushInfo(
    GradientStyle.PathEllipse,
    Color.Gold,
    Color.DarkGoldenrod
)
```

**Best For:** Smooth radial effects, button-like appearance

---

## Multi-Color Gradients

Create gradients with more than two colors using the **GradientColors** array.

### Basic Multi-Color Gradient

**C# Example:**
```csharp
// Create 3-color gradient
gradientLabel.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    new Color[]
    {
        Color.Red,      // Left
        Color.Yellow,   // Middle
        Color.Green     // Right
    }
);
```

**VB.NET Example:**
```vb
' Create 3-color gradient
gradientLabel.BackgroundColor = New BrushInfo(
    GradientStyle.Horizontal,
    New Color() {
        Color.Red,      ' Left
        Color.Yellow,   ' Middle
        Color.Green     ' Right
    }
)
```

---

### Five-Color PathRectangle Gradient

**C# Example:**
```csharp
// Complex multi-color gradient
gradientLabel.BackgroundColor = new BrushInfo(
    GradientStyle.PathRectangle,
    new Color[]
    {
        Color.LavenderBlush,
        Color.LemonChiffon,
        Color.DarkKhaki,
        Color.SandyBrown,
        Color.LightSeaGreen
    }
);
```

**VB.NET Example:**
```vb
' Complex multi-color gradient
gradientLabel.BackgroundColor = New BrushInfo(
    GradientStyle.PathRectangle,
    New Color() {
        Color.LavenderBlush,
        Color.LemonChiffon,
        Color.DarkKhaki,
        Color.SandyBrown,
        Color.LightSeaGreen
    }
)
```

![Multi-Color Gradient](images/multi-color-gradient.png)

---

### Multi-Color Best Practices

**Color Count:**
- 2-3 colors: Clean, professional
- 4-5 colors: Vibrant, eye-catching
- 6+ colors: May look busy, use sparingly

**Color Selection:**
- Use analogous colors (adjacent on color wheel) for harmony
- Use complementary colors for contrast
- Consider color psychology and branding

---

## Pattern Backgrounds

Use pattern styles for textured backgrounds.

### Common Pattern Styles

- **DarkDownwardDiagonal**: Dark diagonal lines (↘)
- **DarkUpwardDiagonal**: Dark diagonal lines (↗)
- **DarkHorizontal**: Dark horizontal lines
- **DarkVertical**: Dark vertical lines
- **DiagonalCross**: Diagonal crosshatch
- **DottedGrid**: Dotted grid pattern
- **Sphere**: Sphere pattern
- **SmallGrid**: Small grid squares

### Pattern Example

**C# Example:**
```csharp
// Create dotted pattern background
gradientLabel.BackgroundColor = new BrushInfo(BrushStyle.Pattern);
gradientLabel.BackgroundColor.PatternStyle = PatternStyle.DottedGrid;
gradientLabel.BackgroundColor.BackColor = Color.White;
gradientLabel.BackgroundColor.ForeColor = Color.DarkBlue;
```

**VB.NET Example:**
```vb
' Create dotted pattern background
gradientLabel.BackgroundColor = New BrushInfo(BrushStyle.Pattern)
gradientLabel.BackgroundColor.PatternStyle = PatternStyle.DottedGrid
gradientLabel.BackgroundColor.BackColor = Color.White
gradientLabel.BackgroundColor.ForeColor = Color.DarkBlue
```

---

## Complete Examples

### Example 1: Professional Header

```csharp
// Create professional section header
GradientLabel header = new GradientLabel
{
    Size = new Size(400, 60),
    Location = new Point(10, 10),
    Text = "Dashboard Overview",
    Font = new Font("Segoe UI", 18, FontStyle.Bold),
    ForeColor = Color.White,
    TextAlign = ContentAlignment.MiddleLeft
};

// Subtle vertical gradient (dark to light blue)
header.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.FromArgb(0, 51, 102),      // Dark blue
    Color.FromArgb(51, 102, 153)     // Medium blue
);
```

---

### Example 2: Status Indicator

```csharp
// Create success status label
GradientLabel statusLabel = new GradientLabel
{
    Size = new Size(120, 35),
    Text = "✓ Success",
    Font = new Font("Arial", 11, FontStyle.Bold),
    ForeColor = Color.DarkGreen,
    TextAlign = ContentAlignment.MiddleCenter
};

// Light green horizontal gradient
statusLabel.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.LightGreen,
    Color.PaleGreen
);
```

---

### Example 3: Attention Banner

```csharp
// Create attention-grabbing banner
GradientLabel banner = new GradientLabel
{
    Size = new Size(500, 70),
    Text = "⚠ Important Announcement",
    Font = new Font("Arial", 16, FontStyle.Bold),
    ForeColor = Color.White,
    TextAlign = ContentAlignment.MiddleCenter
};

// Multi-color attention gradient
banner.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    new Color[]
    {
        Color.DarkOrange,
        Color.Orange,
        Color.Gold,
        Color.Orange,
        Color.DarkOrange
    }
);
```

---

### Example 4: Radial Button Effect

```csharp
// Create button-like label with radial gradient
GradientLabel button = new GradientLabel
{
    Size = new Size(150, 50),
    Text = "Click Here",
    Font = new Font("Arial", 12, FontStyle.Bold),
    ForeColor = Color.White,
    TextAlign = ContentAlignment.MiddleCenter,
    Cursor = Cursors.Hand
};

// Radial gradient (center light, edges dark)
button.BackgroundColor = new BrushInfo(
    GradientStyle.PathEllipse,
    Color.LightBlue,     // Center (light)
    Color.DarkBlue       // Edges (dark)
);

// Add click event
button.Click += (s, e) => MessageBox.Show("Button clicked!");
```

---

## Color Selection Tips

### 1. Ensure Text Readability

```csharp
// Good: High contrast
gradientLabel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.DarkBlue,
    Color.Blue
);
gradientLabel.ForeColor = Color.White;  // White text on dark background

// Poor: Low contrast
// gradientLabel.ForeColor = Color.LightBlue;  // Hard to read on blue background
```

### 2. Use Brand Colors

```csharp
// Company branding colors
Color brandPrimary = Color.FromArgb(0, 120, 215);    // Company blue
Color brandSecondary = Color.FromArgb(0, 153, 255);  // Light blue

gradientLabel.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    brandPrimary,
    brandSecondary
);
```

### 3. Match Application Theme

```csharp
// Dark theme
gradientLabel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.FromArgb(45, 45, 48),      // Dark gray
    Color.FromArgb(60, 60, 65)       // Slightly lighter
);
gradientLabel.ForeColor = Color.White;
```

---

## Performance Considerations

### Simple vs Complex Gradients

**Simple (Fast):**
- 2-color gradients
- Horizontal/Vertical styles
- Solid colors

**Complex (Slower):**
- 5+ color gradients
- PathRectangle/PathEllipse styles
- Pattern backgrounds

### Optimization Tips

1. **Limit gradient complexity** for frequently updated labels
2. **Use solid colors** when gradients aren't necessary
3. **Avoid patterns** unless specifically needed for visual effect
4. **Cache BrushInfo** objects if reusing same gradient across multiple labels

**Example - Reuse BrushInfo:**
```csharp
// Create once, reuse multiple times
BrushInfo headerGradient = new BrushInfo(
    GradientStyle.Vertical,
    Color.DarkBlue,
    Color.LightBlue
);

gradientLabel1.BackgroundColor = headerGradient;
gradientLabel2.BackgroundColor = headerGradient;
gradientLabel3.BackgroundColor = headerGradient;
```

---

## Troubleshooting

### Gradient Not Visible

**Check:**
1. BackgroundColor property is set (not null)
2. Control size is adequate (minimum 30x30)
3. Colors are different (not both same color)
4. Style is set to Gradient (not None or Solid)

### Colors Look Wrong

**Check:**
1. Color values are correct (RGB)
2. GradientColors array order
3. BackColor and ForeColor assignment
4. Form background not interfering

### Performance Issues

**Solutions:**
1. Reduce gradient complexity
2. Use fewer colors
3. Prefer Horizontal/Vertical over PathRectangle/PathEllipse
4. Consider solid colors for less critical labels

---

## Related Topics

- **Getting Started**: Basic setup and initialization → [getting-started.md](getting-started.md)
- **Border Configuration**: Add borders to gradients → [border-configuration.md](border-configuration.md)
- **Serialization**: Save/load gradient settings → [serialization.md](serialization.md)
