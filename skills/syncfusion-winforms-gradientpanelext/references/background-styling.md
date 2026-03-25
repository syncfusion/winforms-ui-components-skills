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

| Style | Description |
|-------|-------------|
| **Gradient** | Gradient transitions between colors |
| **Solid** | Single solid color |
| **Pattern** | Pattern fill with colors |
| **None** | No background (transparent) |

---

### Gradient Style (Default)

Creates smooth color transitions.

**C# Example:**
```csharp
gradientPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.Blue,
    Color.LightBlue
);
// Style is automatically set to Gradient
```

**VB.NET Example:**
```vb
gradientPanel.BackgroundColor = New BrushInfo( _
    GradientStyle.Horizontal, _
    Color.Blue, _
    Color.LightBlue _
)
```

---

### Solid Style

Single uniform color (no gradient).

**C# Example:**
```csharp
gradientPanel.BackgroundColor = new BrushInfo(
    BrushStyle.Solid,
    Color.LightGray,
    Color.White  // Ignored for solid
);
```

**VB.NET Example:**
```vb
gradientPanel.BackgroundColor = New BrushInfo( _
    BrushStyle.Solid, _
    Color.LightGray, _
    Color.White _
)
```

---

### None Style

Transparent background.

**C# Example:**
```csharp
gradientPanel.BackgroundColor = new BrushInfo(BrushStyle.None, Color.Empty, Color.Empty);
gradientPanel.BackColor = Color.Transparent;
```

---

## Gradient Styles

The **GradientStyle** property controls the direction and pattern of color transitions.

**Property:** `BackgroundColor.GradientStyle`  
**Type:** `GradientStyle` enum

### Available Gradient Styles

| Style | Description | Direction |
|-------|-------------|-----------|
| **Horizontal** | Left to right transition | → |
| **Vertical** | Top to bottom transition | ↓ |
| **ForwardDiagonal** | Top-left to bottom-right | ↘ |
| **BackwardDiagonal** | Top-right to bottom-left | ↙ |
| **PathRectangle** | Rectangular path from edges to center | ⊡ |
| **PathEllipse** | Elliptical path from edges to center | ◯ |

---

### Horizontal Gradient

Left to right color transition.

**C# Example:**
```csharp
gradientPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.DarkBlue,
    Color.SkyBlue
);
```

**Best For:** Headers, wide panels, left-right progression

---

### Vertical Gradient

Top to bottom color transition.

**C# Example:**
```csharp
gradientPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.Navy,
    Color.White
);
```

**Best For:** Tall panels, top-down hierarchy, sky/ground effects

---

### ForwardDiagonal Gradient

Diagonal from top-left to bottom-right.

**C# Example:**
```csharp
gradientPanel.BackgroundColor = new BrushInfo(
    GradientStyle.ForwardDiagonal,
    Color.Purple,
    Color.Pink
);
```

**Best For:** Dynamic backgrounds, modern designs

---

### BackwardDiagonal Gradient

Diagonal from top-right to bottom-left.

**C# Example:**
```csharp
gradientPanel.BackgroundColor = new BrushInfo(
    GradientStyle.BackwardDiagonal,
    Color.Green,
    Color.LightGreen
);
```

**Best For:** Alternative diagonal effect, variety

---

### PathRectangle Gradient

Rectangular path from edges inward to center.

**C# Example:**
```csharp
gradientPanel.BackgroundColor = new BrushInfo(
    GradientStyle.PathRectangle,
    Color.DarkRed,
    Color.LightCoral
);
```

**Best For:** Centered focus, spotlight effects, attention-grabbing

---

### PathEllipse Gradient

Elliptical/circular path from edges to center.

**C# Example:**
```csharp
gradientPanel.BackgroundColor = new BrushInfo(
    GradientStyle.PathEllipse,
    Color.DarkGreen,
    Color.LightGreen
);
```

**Best For:** Radial effects, soft focus, circular emphasis

---

## Multi-Color Gradients

Use **GradientColors** array for gradients with 3+ colors.

**Property:** `BackgroundColor.GradientColors`  
**Type:** `Color[]` array

### Three-Color Gradient

**C# Example:**
```csharp
gradientPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    new Color[]
    {
        Color.Blue,        // Left
        Color.Purple,      // Middle
        Color.Red          // Right
    }
);
```

**VB.NET Example:**
```vb
gradientPanel.BackgroundColor = New BrushInfo( _
    GradientStyle.Horizontal, _
    New Color() { _
        Color.Blue, _
        Color.Purple, _
        Color.Red _
    } _
)
```

---

### Five-Color Gradient

**C# Example:**
```csharp
gradientPanel.BackgroundColor = new BrushInfo(
    GradientStyle.PathEllipse,
    new Color[]
    {
        Color.LavenderBlush,
        Color.LemonChiffon,
        Color.LightGoldenrodYellow,
        Color.PaleGreen,
        Color.LightBlue
    }
);
```

**Result:** Smooth rainbow-like transition with 5 color stops

---

### Code-Based Multi-Color Setup

```csharp
// Create array
Color[] rainbowColors = new Color[]
{
    Color.Red,
    Color.Orange,
    Color.Yellow,
    Color.Green,
    Color.Blue,
    Color.Indigo,
    Color.Violet
};

// Apply to panel
gradientPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    rainbowColors
);
```

**Note:** First color in array = BackColor, Last color = ForeColor

---

## Pattern Backgrounds

Use **PatternStyle** for patterned fills with two colors.

**Property:** `BackgroundColor.PatternStyle`  
**Type:** `PatternStyle` enum (50+ patterns)

### Common Pattern Styles

| Pattern | Description |
|---------|-------------|
| **DarkDownwardDiagonal** | Diagonal lines (dark) |
| **DarkUpwardDiagonal** | Diagonal lines (dark) |
| **LightHorizontal** | Horizontal lines (light) |
| **LightVertical** | Vertical lines (light) |
| **DottedGrid** | Dotted grid pattern |
| **DiagonalCross** | Diagonal cross-hatch |
| **Cross** | Horizontal/vertical cross |
| **ZigZag** | Zigzag pattern |

### Using Pattern Style

**C# Example:**
```csharp
BrushInfo patternBrush = new BrushInfo();
patternBrush.Style = BrushStyle.Pattern;
patternBrush.PatternStyle = PatternStyle.DarkDownwardDiagonal;
patternBrush.BackColor = Color.White;      // Pattern background
patternBrush.ForeColor = Color.DarkBlue;   // Pattern lines

gradientPanel.BackgroundColor = patternBrush;
```

**VB.NET Example:**
```vb
Dim patternBrush As New BrushInfo()
patternBrush.Style = BrushStyle.Pattern
patternBrush.PatternStyle = PatternStyle.DarkDownwardDiagonal
patternBrush.BackColor = Color.White
patternBrush.ForeColor = Color.DarkBlue

gradientPanel.BackgroundColor = patternBrush
```

---

## Background Images

Add images behind or over gradient backgrounds.

**Property:** `BackgroundImage`  
**Type:** `Image`

**Property:** `BackgroundImageLayout`  
**Type:** `ImageLayout` enum

### ImageLayout Options

| Layout | Description |
|--------|-------------|
| **None** | Top-left, actual size |
| **Tile** | Repeat image |
| **Center** | Centered, actual size |
| **Stretch** | Stretch to fill |
| **Zoom** | Scale proportionally to fit |

### Setting Background Image

**C# Example:**
```csharp
// Load image from resources
gradientPanel.BackgroundImage = Properties.Resources.BackgroundTexture;
gradientPanel.BackgroundImageLayout = ImageLayout.Stretch;

// Or from file
gradientPanel.BackgroundImage = Image.FromFile("background.png");
gradientPanel.BackgroundImageLayout = ImageLayout.Tile;
```

**VB.NET Example:**
```vb
' Load image from resources
gradientPanel.BackgroundImage = My.Resources.BackgroundTexture
gradientPanel.BackgroundImageLayout = ImageLayout.Stretch

' Or from file
gradientPanel.BackgroundImage = Image.FromFile("background.png")
gradientPanel.BackgroundImageLayout = ImageLayout.Tile
```

---

### Combining Image + Gradient

**C# Example:**
```csharp
// Set gradient
gradientPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.FromArgb(100, 0, 0, 139),    // Semi-transparent dark blue
    Color.FromArgb(100, 135, 206, 250)  // Semi-transparent sky blue
);

// Add background image
gradientPanel.BackgroundImage = Properties.Resources.Texture;
gradientPanel.BackgroundImageLayout = ImageLayout.Tile;
```

**Result:** Image shows through semi-transparent gradient overlay

---

## Complete Examples

### Example 1: Professional Dashboard Panel

```csharp
GradientPanelExt dashboardPanel = new GradientPanelExt
{
    Size = new Size(600, 400),
    Location = new Point(20, 20),
    CornerRadius = 10
};

// Subtle vertical gradient (light theme)
dashboardPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.FromArgb(245, 245, 245),  // Very light gray
    Color.White
);

this.Controls.Add(dashboardPanel);
```

---

### Example 2: Attention-Grabbing Alert Panel

```csharp
GradientPanelExt alertPanel = new GradientPanelExt
{
    Size = new Size(400, 150),
    Location = new Point(50, 50),
    CornerRadius = 12
};

// PathEllipse with warm colors (center glow)
alertPanel.BackgroundColor = new BrushInfo(
    GradientStyle.PathEllipse,
    new Color[]
    {
        Color.Red,
        Color.Orange,
        Color.Yellow
    }
);

this.Controls.Add(alertPanel);
```

---

### Example 3: Multi-Color Rainbow Panel

```csharp
GradientPanelExt rainbowPanel = new GradientPanelExt
{
    Size = new Size(500, 100),
    CornerRadius = 8
};

// Horizontal rainbow
rainbowPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    new Color[]
    {
        Color.FromArgb(255, 0, 0),      // Red
        Color.FromArgb(255, 127, 0),    // Orange
        Color.FromArgb(255, 255, 0),    // Yellow
        Color.FromArgb(0, 255, 0),      // Green
        Color.FromArgb(0, 0, 255),      // Blue
        Color.FromArgb(75, 0, 130),     // Indigo
        Color.FromArgb(148, 0, 211)     // Violet
    }
);

this.Controls.Add(rainbowPanel);
```

---

### Example 4: Pattern Background Panel

```csharp
GradientPanelExt patternPanel = new GradientPanelExt
{
    Size = new Size(350, 250),
    CornerRadius = 10
};

// Diagonal cross pattern
BrushInfo patternBrush = new BrushInfo();
patternBrush.Style = BrushStyle.Pattern;
patternBrush.PatternStyle = PatternStyle.DiagonalCross;
patternBrush.BackColor = Color.LightYellow;
patternBrush.ForeColor = Color.DarkGoldenrod;

patternPanel.BackgroundColor = patternBrush;

this.Controls.Add(patternPanel);
```

---

## Best Practices

### 1. Choose Appropriate Gradient Style

```csharp
// Headers: Horizontal
headerPanel.BackgroundColor = new BrushInfo(GradientStyle.Horizontal, ...);

// Sidebars: Vertical
sidebarPanel.BackgroundColor = new BrushInfo(GradientStyle.Vertical, ...);

// Focus elements: PathEllipse
focusPanel.BackgroundColor = new BrushInfo(GradientStyle.PathEllipse, ...);
```

### 2. Use Subtle Gradients for Readability

```csharp
// Good: Subtle difference
gradientPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.FromArgb(240, 240, 240),
    Color.White
);

// Avoid: Too much contrast makes text hard to read
// Color.Black to Color.White
```

### 3. Coordinate with Content

```csharp
// Dark gradient → Light text
darkPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.DarkSlateGray,
    Color.Gray
);

Label lightText = new Label
{
    ForeColor = Color.White,
    BackColor = Color.Transparent
};
darkPanel.Controls.Add(lightText);
```

### 4. Performance Considerations

- Simple gradients (2 colors) render faster than multi-color (5+)
- PathEllipse/PathRectangle are more resource-intensive than Horizontal/Vertical
- Solid is fastest (no gradient calculation)
- Consider performance for many panels or frequent repaints

---

## Troubleshooting

### Gradient Not Visible

**Check:**
1. BackgroundColor.Style is Gradient (not None or Solid)
2. BackColor and ForeColor are different
3. GradientStyle is set (not None)

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
