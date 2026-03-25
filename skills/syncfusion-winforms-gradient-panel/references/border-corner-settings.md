# Border and Corner Settings

Guide to customizing corner radius and border gaps in GradientPanelExt for rounded or sharp panel edges.

## Overview

GradientPanelExt provides two key border properties:
- **CornerRadius**: Rounds panel corners
- **BorderGap**: Adds spacing between border and margins

These properties allow you to create modern rounded panels or classic sharp-edged rectangles.

---

## CornerRadius Property

Controls the roundness of panel corners. GradientPanelExt is rounded by default.

**Property Type:** `int`  
**Default Value:** Non-zero (rounded)  
**Range:** 0 (sharp corners) to high values (very rounded)

### Setting Corner Radius

**C# Example:**
```csharp
// Rounded corners (default behavior)
gradientPanel.CornerRadius = 10;

// More rounded
gradientPanel.CornerRadius = 20;

// Sharp corners (remove rounding)
gradientPanel.CornerRadius = 0;
```

**VB.NET Example:**
```vb
' Rounded corners
gradientPanel.CornerRadius = 10

' Sharp corners
gradientPanel.CornerRadius = 0
```

---

### Sharp Corners (CornerRadius = 0)

Creates traditional rectangle with 90-degree corners.

**C# Example:**
```csharp
GradientPanelExt sharpPanel = new GradientPanelExt
{
    Size = new Size(300, 150),
    Location = new Point(20, 20),
    CornerRadius = 0  // Sharp corners
};

sharpPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.Navy,
    Color.Blue
);

this.Controls.Add(sharpPanel);
```

**Best For:** Classic designs, formal applications, maximizing usable area

---

### Light Rounding (CornerRadius = 5-10)

Subtle rounded corners for modern look.

**C# Example:**
```csharp
GradientPanelExt subtlePanel = new GradientPanelExt
{
    Size = new Size(300, 150),
    CornerRadius = 8  // Light rounding
};

subtlePanel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.WhiteSmoke,
    Color.LightGray
);

this.Controls.Add(subtlePanel);
```

**Best For:** Modern corporate UI, subtle elegance, balanced design

---

### Medium Rounding (CornerRadius = 12-18)

Noticeable rounded corners for friendly appearance.

**C# Example:**
```csharp
GradientPanelExt friendlyPanel = new GradientPanelExt
{
    Size = new Size(350, 200),
    CornerRadius = 15  // Medium rounding
};

friendlyPanel.BackgroundColor = new BrushInfo(
    GradientStyle.PathEllipse,
    Color.LightBlue,
    Color.White
);

this.Controls.Add(friendlyPanel);
```

**Best For:** User-friendly apps, consumer software, welcoming designs

---

### High Rounding (CornerRadius = 20+)

Very rounded corners, almost pill-shaped for smaller panels.

**C# Example:**
```csharp
GradientPanelExt pillPanel = new GradientPanelExt
{
    Size = new Size(200, 80),
    CornerRadius = 25  // High rounding
};

pillPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.MediumPurple,
    Color.Plum
);

this.Controls.Add(pillPanel);
```

**Best For:** Buttons, badges, playful designs, mobile-inspired UI

---

## BorderGap Property

Sets the spacing between the panel's border and its margins. Creates an "inset" appearance.

**Property Type:** `int`  
**Default Value:** 0 (no gap)  
**Unit:** Pixels

### Setting Border Gap

**C# Example:**
```csharp
// No gap (default)
gradientPanel.BorderGap = 0;

// Small gap
gradientPanel.BorderGap = 5;

// Large gap
gradientPanel.BorderGap = 20;
```

**VB.NET Example:**
```vb
' No gap
gradientPanel.BorderGap = 0

' Medium gap
gradientPanel.BorderGap = 10
```

---

### No Border Gap (BorderGap = 0)

Standard appearance with no spacing.

**C# Example:**
```csharp
GradientPanelExt standardPanel = new GradientPanelExt
{
    Size = new Size(300, 150),
    BorderGap = 0,  // No gap
    CornerRadius = 10
};

standardPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.Green,
    Color.LightGreen
);

this.Controls.Add(standardPanel);
```

---

### Small Border Gap (BorderGap = 5-10)

Subtle inset effect.

**C# Example:**
```csharp
GradientPanelExt insetPanel = new GradientPanelExt
{
    Size = new Size(300, 150),
    BorderGap = 8,  // Small gap
    CornerRadius = 12
};

insetPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.DarkSlateBlue,
    Color.SlateBlue
);

this.Controls.Add(insetPanel);
```

**Visual Effect:** Creates slight padding, panel appears slightly inset

---

### Large Border Gap (BorderGap = 15+)

Pronounced spacing for dramatic effect.

**C# Example:**
```csharp
GradientPanelExt spacedPanel = new GradientPanelExt
{
    Size = new Size(400, 200),
    BorderGap = 25,  // Large gap
    CornerRadius = 15
};

spacedPanel.BackgroundColor = new BrushInfo(
    GradientStyle.PathRectangle,
    Color.DarkOrange,
    Color.LightYellow
);

this.Controls.Add(spacedPanel);
```

**Visual Effect:** Significant margin, floating appearance

---

## Combined Examples

### Example 1: Modern Sharp Panel

```csharp
GradientPanelExt modernPanel = new GradientPanelExt
{
    Size = new Size(500, 300),
    Location = new Point(30, 30),
    CornerRadius = 0,  // Sharp for modern flat design
    BorderGap = 0
};

// Flat gradient
modernPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.FromArgb(240, 240, 240),
    Color.White
);

this.Controls.Add(modernPanel);
```

---

### Example 2: Friendly Rounded Panel

```csharp
GradientPanelExt friendlyPanel = new GradientPanelExt
{
    Size = new Size(400, 250),
    CornerRadius = 18,  // Friendly rounding
    BorderGap = 5        // Subtle inset
};

// Warm gradient
friendlyPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.FromArgb(255, 245, 230),  // Light peach
    Color.FromArgb(255, 220, 180)   // Darker peach
);

this.Controls.Add(friendlyPanel);
```

---

### Example 3: Dramatic Inset Panel

```csharp
GradientPanelExt dramaticPanel = new GradientPanelExt
{
    Size = new Size(450, 350),
    CornerRadius = 20,   // Highly rounded
    BorderGap = 30       // Large gap for drama
};

// Bold gradient
dramaticPanel.BackgroundColor = new BrushInfo(
    GradientStyle.PathEllipse,
    Color.DarkMagenta,
    Color.Pink
);

this.Controls.Add(dramaticPanel);
```

---

### Example 4: Card-Style Panel

```csharp
// Modern card appearance
GradientPanelExt cardPanel = new GradientPanelExt
{
    Size = new Size(320, 180),
    CornerRadius = 12,  // Card-like rounding
    BorderGap = 0
};

cardPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.White,
    Color.FromArgb(250, 250, 250)
);

// Add shadow effect via form's DropShadow or custom painting
this.Controls.Add(cardPanel);
```

---

## Design Guidelines

### Corner Radius Guidelines

| Radius | Style | Use Case |
|--------|-------|----------|
| **0** | Sharp | Formal, classic, grid layouts |
| **5-8** | Subtle | Modern corporate, professional |
| **10-15** | Moderate | Friendly, approachable, balanced |
| **16-25** | Rounded | Playful, consumer apps, mobile-style |
| **25+** | Pill | Buttons, badges, small elements |

### Border Gap Guidelines

| Gap | Effect | Use Case |
|-----|--------|----------|
| **0** | None | Standard panels, maximize space |
| **5-10** | Subtle | Slight depth, refined look |
| **15-25** | Moderate | Clear separation, floating effect |
| **25+** | Dramatic | Special emphasis, focal panels |

---

## Best Practices

### 1. Match Design Language

```csharp
// Modern flat design: Sharp corners
modernApp.CornerRadius = 0;

// Friendly consumer app: Rounded
consumerApp.CornerRadius = 15;

// Corporate professional: Subtle rounding
corporateApp.CornerRadius = 8;
```

### 2. Coordinate with Panel Size

```csharp
// Large panel: Moderate rounding
largePanel.Size = new Size(600, 400);
largePanel.CornerRadius = 12;

// Small panel: Light rounding (proportional)
smallPanel.Size = new Size(150, 80);
smallPanel.CornerRadius = 6;
```

### 3. Consider Content Visibility

```csharp
// BorderGap reduces usable area
// Account for it when sizing child controls

panel.BorderGap = 20;
panel.Size = new Size(400, 300);

// Child control must fit within (400-40) x (300-40)
Label label = new Label
{
    Location = new Point(30, 30),  // Account for gap
    MaximumSize = new Size(340, 240)
};
```

### 4. Consistency Across Application

```csharp
// Define constants for consistency
const int STANDARD_CORNER_RADIUS = 10;
const int STANDARD_BORDER_GAP = 0;

// Apply to all panels
panel1.CornerRadius = STANDARD_CORNER_RADIUS;
panel2.CornerRadius = STANDARD_CORNER_RADIUS;
panel3.CornerRadius = STANDARD_CORNER_RADIUS;
```

---

## Troubleshooting

### Corners Not Rounded

**Check:**
- CornerRadius > 0
- Panel size is adequate (very small panels may not show rounding)

```csharp
// Ensure adequate size
gradientPanel.Size = new Size(100, 100);  // Minimum for visible rounding
gradientPanel.CornerRadius = 10;
```

### BorderGap Cutting Off Content

**Solution:** Increase panel size or adjust child control positions

```csharp
panel.BorderGap = 15;
panel.Size = new Size(400, 300);

// Position child controls away from edges
button.Location = new Point(20 + 15, 20 + 15);  // Add BorderGap offset
```

### Inconsistent Appearance

**Solution:** Verify CornerRadius is same across similar panels

```csharp
// Check current value
int currentRadius = panel.CornerRadius;
System.Diagnostics.Debug.WriteLine($"CornerRadius: {currentRadius}");
```

---

## Related Topics

- **Getting Started**: Basic setup → [getting-started.md](getting-started.md)
- **Background Styling**: Gradients → [background-styling.md](background-styling.md)
- **Primitives**: Border elements → [primitives.md](primitives.md)
