# Background Styles

## Table of Contents
- [Overview](#overview)
- [BackgroundColor vs BackColor](#backgroundcolor-vs-backcolor)
- [Solid Style](#solid-style)
- [Pattern Style](#pattern-style)
- [Gradient Style](#gradient-style)
- [Common Scenarios](#common-scenarios)
- [Troubleshooting](#troubleshooting)

## Overview

GradientPanel provides three distinct background styling options through the `BackgroundColor` property using the `BrushInfo` class. Each style offers different visual effects for container panels.

**Background style types:**
- **Solid** - Single solid color
- **Pattern** - Repeating pattern fills (53 patterns available)
- **Gradient** - Color transitions (7 gradient directions)

**Key class:** `BrushInfo` from `Syncfusion.Drawing` namespace

## BackgroundColor vs BackColor

### BackgroundColor Property

Use `BackgroundColor` for gradient, pattern, or solid backgrounds with `BrushInfo`:

```csharp
// BackgroundColor with BrushInfo - for gradients/patterns
gradientPanel1.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.Blue,
    Color.Red
);
```

**Use when:**
- Creating gradient effects
- Applying pattern fills
- Need advanced background rendering

### BackColor Property

Standard `BackColor` provides simple solid color:

```csharp
// BackColor - simple solid color (inherited from Control)
gradientPanel1.BackColor = Color.Blue;
```

**Use when:**
- Simple solid color needed
- No gradient or pattern required
- Matching standard .NET control behavior

**Important:** For gradients and patterns, always use `BackgroundColor` with `BrushInfo`.

## Solid Style

### Basic Solid Background

```csharp
// Solid blue background
gradientPanel1.BackgroundColor = new BrushInfo(Color.MediumBlue);
```

### With Border

```csharp
// Solid background with border
gradientPanel1.BackgroundColor = new BrushInfo(Color.MediumBlue);
gradientPanel1.BorderStyle = BorderStyle.FixedSingle;
gradientPanel1.BorderColor = Color.Red;
```

**Visual result:** Panel with solid blue background and red border

![Solid Style](../../../docs/GradientPanel-Images/GradientPanel_solid.png)

### Custom Colors

```csharp
// Custom RGB color
gradientPanel1.BackgroundColor = new BrushInfo(
    Color.FromArgb(0, 120, 215)
);

// Named color
gradientPanel1.BackgroundColor = new BrushInfo(Color.DarkSlateGray);
```

### Solid Style Use Cases

**Use solid backgrounds for:**
- Simple, clean panel designs
- Matching application theme colors
- Sections that don't need visual complexity
- High contrast requirements
- Professional, minimal designs

## Pattern Style

### Pattern Background Syntax

```csharp
gradientPanel1.BackgroundColor = new BrushInfo(
    PatternStyle.PatternName,  // Pattern type
    Color.Foreground,          // Foreground color
    Color.Background           // Background color
);
```

### Common Pattern Examples

#### LargeCheckerBoard

```csharp
gradientPanel1.BackgroundColor = new BrushInfo(
    PatternStyle.LargeCheckerBoard,
    Color.Turquoise,   // Foreground
    Color.MediumBlue   // Background
);
gradientPanel1.BorderStyle = BorderStyle.FixedSingle;
gradientPanel1.BorderColor = Color.PaleTurquoise;
```

![Pattern Style](../../../docs/GradientPanel-Images/GradientPanel_pattern.png)

#### DiagonalCross

```csharp
gradientPanel1.BackgroundColor = new BrushInfo(
    PatternStyle.DiagonalCross,
    Color.Navy,
    Color.LightBlue
);
```

#### DottedGrid

```csharp
gradientPanel1.BackgroundColor = new BrushInfo(
    PatternStyle.DottedGrid,
    Color.Black,
    Color.White
);
```

#### Horizontal Lines

```csharp
gradientPanel1.BackgroundColor = new BrushInfo(
    PatternStyle.Horizontal,
    Color.Gray,
    Color.WhiteSmoke
);
```

### Available Pattern Styles

**PatternStyle enum (53 patterns):**

**Geometric patterns:**
- `LargeCheckerBoard`, `SmallCheckerBoard`
- `LargeGrid`, `SmallGrid`
- `DottedGrid`, `DottedDiamond`
- `DiagonalCross`, `WideDownwardDiagonal`, `WideUpwardDiagonal`

**Line patterns:**
- `Horizontal`, `Vertical`
- `DarkHorizontal`, `DarkVertical`
- `DashedHorizontal`, `DashedVertical`
- `DashedDownwardDiagonal`, `DashedUpwardDiagonal`

**Diagonal patterns:**
- `ForwardDiagonal`, `BackwardDiagonal`
- `LightDownwardDiagonal`, `LightUpwardDiagonal`
- `DarkDownwardDiagonal`, `DarkUpwardDiagonal`

**Special patterns:**
- `Sphere`, `Wave`, `Weave`
- `Plaid`, `Divot`, `Shingle`
- `Trellis`, `ZigZag`

**And many more...** (53 total)

### Pattern Style Use Cases

**Use pattern backgrounds for:**
- Textured panel appearances
- Visual interest without color complexity
- Sidebar or background panels
- Tool palettes or option panels
- Retro or stylized interfaces

## Gradient Style

### Gradient Background Syntax

```csharp
gradientPanel1.BackgroundColor = new BrushInfo(
    GradientStyle.Direction,  // Gradient direction
    Color.StartColor,         // Start color
    Color.EndColor            // End color
);
```

### Gradient Directions

#### ForwardDiagonal

Top-left to bottom-right diagonal gradient:

```csharp
gradientPanel1.BackgroundColor = new BrushInfo(
    GradientStyle.ForwardDiagonal,
    Color.Red,
    Color.MediumBlue
);
gradientPanel1.BorderStyle = BorderStyle.FixedSingle;
gradientPanel1.BorderColor = Color.Transparent;
```

![Gradient Style](../../../docs/GradientPanel-Images/GradientPanel_gradient.png)

#### BackwardDiagonal

Top-right to bottom-left diagonal gradient:

```csharp
gradientPanel1.BackgroundColor = new BrushInfo(
    GradientStyle.BackwardDiagonal,
    Color.Purple,
    Color.Pink
);
```

#### Horizontal

Left-to-right horizontal gradient:

```csharp
gradientPanel1.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.DarkBlue,
    Color.LightBlue
);
```

**Use for:** Headers, footers, horizontal sections

#### Vertical

Top-to-bottom vertical gradient:

```csharp
gradientPanel1.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.White,
    Color.Gray
);
```

**Use for:** Sidebars, vertical sections, full-form backgrounds

#### PathRectangle

Gradient radiates from center rectangle outward:

```csharp
gradientPanel1.BackgroundColor = new BrushInfo(
    GradientStyle.PathRectangle,
    Color.AliceBlue,
    Color.SteelBlue
);
```

**Visual effect:** Center is lighter, edges are darker (or vice versa)

**Use for:** Card-style panels, spotlights, focused content areas

#### PathEllipse

Gradient radiates from center ellipse outward:

```csharp
gradientPanel1.BackgroundColor = new BrushInfo(
    GradientStyle.PathEllipse,
    Color.Yellow,
    Color.Orange
);
```

**Visual effect:** Circular/elliptical gradient from center

**Use for:** Circular highlights, button-like panels, radial effects

#### None

No gradient (equivalent to solid):

```csharp
gradientPanel1.BackgroundColor = new BrushInfo(
    GradientStyle.None,
    Color.Blue,
    Color.Blue  // Same color, no transition
);
```

### All Gradient Styles

**GradientStyle enum (7 directions):**
1. `None` - No gradient
2. `ForwardDiagonal` - Top-left to bottom-right
3. `BackwardDiagonal` - Top-right to bottom-left
4. `Horizontal` - Left to right
5. `Vertical` - Top to bottom
6. `PathRectangle` - Radial from rectangle center
7. `PathEllipse` - Radial from ellipse center

### Gradient Style Comparison

```csharp
// Create multiple panels to compare gradient directions
private void CreateGradientComparison()
{
    var gradients = new[]
    {
        (GradientStyle.ForwardDiagonal, "Forward Diagonal"),
        (GradientStyle.BackwardDiagonal, "Backward Diagonal"),
        (GradientStyle.Horizontal, "Horizontal"),
        (GradientStyle.Vertical, "Vertical"),
        (GradientStyle.PathRectangle, "Path Rectangle"),
        (GradientStyle.PathEllipse, "Path Ellipse")
    };
    
    int x = 10, y = 10;
    
    foreach (var (style, name) in gradients)
    {
        GradientPanel panel = new GradientPanel();
        panel.Size = new Size(150, 100);
        panel.Location = new Point(x, y);
        panel.BackgroundColor = new BrushInfo(
            style,
            Color.FromArgb(0, 120, 215),
            Color.FromArgb(0, 200, 83)
        );
        panel.BorderStyle = BorderStyle.FixedSingle;
        
        Label label = new Label();
        label.Text = name;
        label.BackColor = Color.Transparent;
        label.ForeColor = Color.White;
        label.AutoSize = true;
        label.Location = new Point(5, 5);
        panel.Controls.Add(label);
        
        this.Controls.Add(panel);
        
        x += 160;
        if (x > 500)
        {
            x = 10;
            y += 110;
        }
    }
}
```

### Gradient Style Use Cases

**ForwardDiagonal / BackwardDiagonal:**
- Dynamic, modern appearance
- Headers and banners
- Accent panels

**Horizontal:**
- Headers and footers
- Navigation bars
- Horizontal sections

**Vertical:**
- Sidebars
- Full-form backgrounds
- Vertical navigation panels

**PathRectangle:**
- Card-style panels
- Content cards
- Spotlight effects
- Button-like panels

**PathEllipse:**
- Circular highlights
- Radial focus areas
- Special content blocks

## Common Scenarios

### Scenario 1: Application Header

```csharp
GradientPanel headerPanel = new GradientPanel();
headerPanel.Dock = DockStyle.Top;
headerPanel.Height = 80;
headerPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.FromArgb(0, 120, 215),   // Start: Blue
    Color.FromArgb(0, 80, 150)     // End: Darker blue
);

Label titleLabel = new Label();
titleLabel.Text = "Application Title";
titleLabel.Font = new Font("Segoe UI", 18, FontStyle.Bold);
titleLabel.ForeColor = Color.White;
titleLabel.BackColor = Color.Transparent;
titleLabel.Location = new Point(20, 25);
headerPanel.Controls.Add(titleLabel);

this.Controls.Add(headerPanel);
```

### Scenario 2: Sidebar with Pattern

```csharp
GradientPanel sidebarPanel = new GradientPanel();
sidebarPanel.Dock = DockStyle.Left;
sidebarPanel.Width = 200;
sidebarPanel.BackgroundColor = new BrushInfo(
    PatternStyle.DarkDownwardDiagonal,
    Color.Gray,
    Color.DarkGray
);
sidebarPanel.BorderStyle = BorderStyle.Fixed3D;

this.Controls.Add(sidebarPanel);
```

### Scenario 3: Content Cards

```csharp
private void CreateContentCards()
{
    FlowLayoutPanel container = new FlowLayoutPanel();
    container.Dock = DockStyle.Fill;
    container.Padding = new Padding(10);
    
    for (int i = 0; i < 6; i++)
    {
        GradientPanel card = new GradientPanel();
        card.Size = new Size(180, 150);
        card.Margin = new Padding(5);
        card.BackgroundColor = new BrushInfo(
            GradientStyle.PathRectangle,
            Color.AliceBlue,
            Color.SteelBlue
        );
        card.BorderStyle = BorderStyle.FixedSingle;
        card.BorderColor = Color.SteelBlue;
        
        Label cardTitle = new Label();
        cardTitle.Text = $"Card {i + 1}";
        cardTitle.Font = new Font("Segoe UI", 12, FontStyle.Bold);
        cardTitle.BackColor = Color.Transparent;
        cardTitle.AutoSize = true;
        cardTitle.Location = new Point(10, 10);
        card.Controls.Add(cardTitle);
        
        container.Controls.Add(card);
    }
    
    this.Controls.Add(container);
}
```

### Scenario 4: Form Background

```csharp
GradientPanel backgroundPanel = new GradientPanel();
backgroundPanel.Dock = DockStyle.Fill;
backgroundPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.WhiteSmoke,
    Color.LightGray
);

this.Controls.Add(backgroundPanel);
backgroundPanel.SendToBack();
```

## Troubleshooting

### Issue: Gradient not displaying

**Causes:**
- Using `BackColor` instead of `BackgroundColor`
- BrushInfo not configured correctly

**Solution:**
```csharp
// INCORRECT
gradientPanel1.BackColor = Color.Blue;

// CORRECT
gradientPanel1.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.Blue,
    Color.Red
);
```

### Issue: Pattern not visible

**Causes:**
- Foreground and background colors too similar
- Pattern style not suitable for color combination

**Solution:**
```csharp
// Use high contrast colors
gradientPanel1.BackgroundColor = new BrushInfo(
    PatternStyle.LargeCheckerBoard,
    Color.Black,      // High contrast
    Color.White
);
```

### Issue: Gradient direction unclear

**Cause:** Similar start and end colors

**Solution:**
```csharp
// Use contrasting colors to see gradient clearly
gradientPanel1.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.Blue,       // Distinct start
    Color.Yellow      // Distinct end
);
```

### Issue: PathRectangle/PathEllipse not centered

**Cause:** This is normal behavior - gradient follows panel dimensions

**Solution:** Center is based on panel size. Resize panel for desired effect:
```csharp
// Square panel for circular PathEllipse
gradientPanel1.Size = new Size(200, 200);
gradientPanel1.BackgroundColor = new BrushInfo(
    GradientStyle.PathEllipse,
    Color.Yellow,
    Color.Orange
);
```

## Best Practices

1. **Use BackgroundColor for gradients** - Always use `BackgroundColor` with `BrushInfo`, not `BackColor`
2. **High contrast for patterns** - Use contrasting colors for pattern visibility
3. **Match gradient to orientation** - Horizontal gradients for headers, vertical for sidebars
4. **PathRectangle for cards** - Best choice for card-style UI elements
5. **Consistent gradient directions** - Use same direction for related panels
6. **Test visibility** - Ensure text/controls are visible over background
7. **Complementary colors** - Choose gradient colors that blend well
8. **Subtle transitions** - For professional look, use similar color shades
9. **Bold transitions** - For modern/vibrant look, use contrasting colors
10. **Pattern sparingly** - Use patterns for accents, not entire interfaces
