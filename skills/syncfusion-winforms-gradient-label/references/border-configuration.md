# Border Configuration

Comprehensive guide to customizing border styles, sides, and colors in GradientLabel for enhanced visual appeal.

## Overview

GradientLabel provides extensive border customization through three main properties:
- **BorderSides**: Which sides display borders
- **BorderStyle**: Visual style of the border (3D effects)
- **BorderColor**: Color for 2D borders

---

## BorderSides Property

Controls which sides of the label display borders.

**Property Type:** `Border3DSide` (enum, flags)  
**Default Value:** `Border3DSide.All`

### Available Sides

| Side | Description |
|------|-------------|
| **Left** | Left edge only |
| **Top** | Top edge only |
| **Right** | Right edge only |
| **Bottom** | Bottom edge only |
| **Middle** | Middle sections |
| **All** | All sides (default) |

### Single Side Border

**C# Example:**
```csharp
// Top border only
gradientLabel.BorderSides = Border3DSide.Top;

// Left border only
gradientLabel.BorderSides = Border3DSide.Left;

// Bottom border only
gradientLabel.BorderSides = Border3DSide.Bottom;
```

**VB.NET Example:**
```vb
' Top border only
gradientLabel.BorderSides = Border3DSide.Top

' Left border only
gradientLabel.BorderSides = Border3DSide.Left

' Bottom border only
gradientLabel.BorderSides = Border3DSide.Bottom
```

---

### Multiple Sides Border

Combine sides using bitwise OR operator.

**C# Example:**
```csharp
// Top and bottom only
gradientLabel.BorderSides = Border3DSide.Top | Border3DSide.Bottom;

// Left and right only
gradientLabel.BorderSides = Border3DSide.Left | Border3DSide.Right;

// Three sides (no bottom)
gradientLabel.BorderSides = Border3DSide.Left | Border3DSide.Top | Border3DSide.Right;
```

**VB.NET Example:**
```vb
' Top and bottom only
gradientLabel.BorderSides = Border3DSide.Top Or Border3DSide.Bottom

' Left and right only
gradientLabel.BorderSides = Border3DSide.Left Or Border3DSide.Right

' Three sides (no bottom)
gradientLabel.BorderSides = Border3DSide.Left Or Border3DSide.Top Or Border3DSide.Right
```

---

### All Sides Border (Default)

**C# Example:**
```csharp
// All sides (default)
gradientLabel.BorderSides = Border3DSide.All;
```

**VB.NET Example:**
```vb
' All sides (default)
gradientLabel.BorderSides = Border3DSide.All
```

---

## BorderStyle Property

Controls the visual appearance and 3D effect of borders.

**Property Type:** `Border3DStyle` (enum)  
**Default Value:** `Border3DStyle.Sunken`

### Available Border Styles

| Style | Description | Visual Effect |
|-------|-------------|---------------|
| **Raised** | Fully raised 3D effect | Appears elevated |
| **RaisedOuter** | Outer edge raised only | Subtle elevation |
| **RaisedInner** | Inner edge raised only | Inverse subtle |
| **Sunken** | Fully sunken 3D effect | Appears pressed |
| **SunkenOuter** | Outer edge sunken only | Subtle depression |
| **SunkenInner** | Inner edge sunken only | Inverse subtle |
| **Etched** | Etched appearance | Carved look |
| **Bump** | Bumped appearance | Embossed effect |
| **Flat** | Flat 2D border | No 3D effect |
| **Adjust** | No border | Removes border |

---

### Raised Border

Creates elevated, button-like appearance.

**C# Example:**
```csharp
gradientLabel.BorderStyle = Border3DStyle.Raised;
gradientLabel.BorderSides = Border3DSide.All;
```

**VB.NET Example:**
```vb
gradientLabel.BorderStyle = Border3DStyle.Raised
gradientLabel.BorderSides = Border3DSide.All
```

**Best For:** Buttons, clickable labels, elevated panels

---

### Sunken Border (Default)

Creates pressed, inset appearance.

**C# Example:**
```csharp
gradientLabel.BorderStyle = Border3DStyle.Sunken;
gradientLabel.BorderSides = Border3DSide.All;
```

**VB.NET Example:**
```vb
gradientLabel.BorderStyle = Border3DStyle.Sunken
gradientLabel.BorderSides = Border3DSide.All
```

**Best For:** Input-like appearance, recessed panels

---

### Flat Border

Simple 2D flat border (use with BorderColor).

**C# Example:**
```csharp
gradientLabel.BorderStyle = Border3DStyle.Flat;
gradientLabel.BorderSides = Border3DSide.All;
gradientLabel.BorderColor = Color.DarkGray;  // Custom color
```

**VB.NET Example:**
```vb
gradientLabel.BorderStyle = Border3DStyle.Flat
gradientLabel.BorderSides = Border3DSide.All
gradientLabel.BorderColor = Color.DarkGray  ' Custom color
```

**Best For:** Modern flat design, custom colored borders

---

### Etched Border

Creates carved, engraved appearance.

**C# Example:**
```csharp
gradientLabel.BorderStyle = Border3DStyle.Etched;
gradientLabel.BorderSides = Border3DSide.All;
```

**VB.NET Example:**
```vb
gradientLabel.BorderStyle = Border3DStyle.Etched
gradientLabel.BorderSides = Border3DSide.All
```

**Best For:** Decorative labels, separated sections

---

### No Border

Remove border completely.

**C# Example:**
```csharp
gradientLabel.BorderStyle = Border3DStyle.Adjust;
```

**VB.NET Example:**
```vb
gradientLabel.BorderStyle = Border3DStyle.Adjust
```

---

## BorderColor Property

Sets the color for 2D borders.

**Property Type:** `Color`  
**Effective When:** BorderColor only works when BorderStyle is set to `Border3DStyle.Flat`

**C# Example:**
```csharp
// Red flat border
gradientLabel.BorderStyle = Border3DStyle.Flat;
gradientLabel.BorderColor = Color.Red;
gradientLabel.BorderSides = Border3DSide.All;

// Custom RGB border
gradientLabel.BorderColor = Color.FromArgb(255, 100, 50, 150);
```

**VB.NET Example:**
```vb
' Red flat border
gradientLabel.BorderStyle = Border3DStyle.Flat
gradientLabel.BorderColor = Color.Red
gradientLabel.BorderSides = Border3DSide.All

' Custom RGB border
gradientLabel.BorderColor = Color.FromArgb(255, 100, 50, 150)
```

**Note:** BorderColor has no effect with 3D border styles (Raised, Sunken, etc.).

---

## Complete Border Examples

### Example 1: Underline Effect

```csharp
// Create label with bottom border only (underline)
GradientLabel titleLabel = new GradientLabel
{
    Size = new Size(300, 40),
    Text = "Section Title",
    Font = new Font("Arial", 14, FontStyle.Bold),
    ForeColor = Color.DarkBlue,
    TextAlign = ContentAlignment.BottomLeft
};

// Gradient background
titleLabel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.LightBlue,
    Color.White
);

// Bottom border only (underline effect)
titleLabel.BorderSides = Border3DSide.Bottom;
titleLabel.BorderStyle = Border3DStyle.Flat;
titleLabel.BorderColor = Color.DarkBlue;
```

---

### Example 2: Raised Button

```csharp
// Create button-like label with raised border
GradientLabel buttonLabel = new GradientLabel
{
    Size = new Size(120, 40),
    Text = "Submit",
    Font = new Font("Arial", 11, FontStyle.Bold),
    ForeColor = Color.White,
    TextAlign = ContentAlignment.MiddleCenter,
    Cursor = Cursors.Hand
};

// Gradient background
buttonLabel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.DodgerBlue,
    Color.Blue
);

// Raised 3D border
buttonLabel.BorderStyle = Border3DStyle.Raised;
buttonLabel.BorderSides = Border3DSide.All;
```

---

### Example 3: Left Accent Border

```csharp
// Create label with left accent border
GradientLabel accentLabel = new GradientLabel
{
    Size = new Size(250, 50),
    Text = "Important Message",
    Font = new Font("Arial", 10, FontStyle.Regular),
    ForeColor = Color.Black,
    TextAlign = ContentAlignment.MiddleLeft
};

// Light gradient background
accentLabel.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.LightYellow,
    Color.White
);

// Left border only (accent)
accentLabel.BorderSides = Border3DSide.Left;
accentLabel.BorderStyle = Border3DStyle.Flat;
accentLabel.BorderColor = Color.Orange;

// Add padding for text
accentLabel.Padding = new Padding(10, 0, 0, 0);
```

---

### Example 4: Framed Panel

```csharp
// Create framed panel label
GradientLabel panelLabel = new GradientLabel
{
    Size = new Size(350, 80),
    Text = "Information Panel\n\nThis is a multi-line label with complete frame border.",
    Font = new Font("Arial", 9, FontStyle.Regular),
    ForeColor = Color.Black,
    TextAlign = ContentAlignment.MiddleLeft
};

// Subtle gradient
panelLabel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.WhiteSmoke,
    Color.Gainsboro
);

// All sides with sunken effect
panelLabel.BorderStyle = Border3DStyle.Sunken;
panelLabel.BorderSides = Border3DSide.All;
```

---

## Border Styling Best Practices

### 1. Match Border to Purpose

```csharp
// Interactive elements: Raised
buttonLabel.BorderStyle = Border3DStyle.Raised;

// Display panels: Flat or Sunken
infoLabel.BorderStyle = Border3DStyle.Flat;

// Accent/separator: Single side
headerLabel.BorderSides = Border3DSide.Bottom;
```

### 2. Coordinate Border with Gradient

```csharp
// Dark gradient → Light border looks good
gradientLabel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.DarkBlue,
    Color.Blue
);
gradientLabel.BorderStyle = Border3DStyle.Raised;  // Light raised edge

// Light gradient → Dark border for contrast
lightLabel.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.LightGray,
    Color.White
);
lightLabel.BorderStyle = Border3DStyle.Flat;
lightLabel.BorderColor = Color.DarkGray;
```

### 3. Use Selective Borders for Modern Look

```csharp
// Modern design: bottom border only
modernLabel.BorderSides = Border3DSide.Bottom;
modernLabel.BorderStyle = Border3DStyle.Flat;
modernLabel.BorderColor = Color.FromArgb(0, 120, 215);  // Accent color
```

### 4. Consider Visual Hierarchy

```csharp
// Primary headers: Full border with 3D effect
primaryHeader.BorderStyle = Border3DStyle.Raised;
primaryHeader.BorderSides = Border3DSide.All;

// Secondary headers: Bottom border only
secondaryHeader.BorderStyle = Border3DStyle.Flat;
secondaryHeader.BorderSides = Border3DSide.Bottom;

// Body labels: No border
bodyLabel.BorderStyle = Border3DStyle.Adjust;
```

---

## Common Border Patterns

### Pattern 1: Card-Style

```csharp
gradientLabel.BorderStyle = Border3DStyle.Raised;
gradientLabel.BorderSides = Border3DSide.All;
```

### Pattern 2: Divider/Separator

```csharp
gradientLabel.BorderSides = Border3DSide.Bottom;
gradientLabel.BorderStyle = Border3DStyle.Flat;
gradientLabel.BorderColor = Color.LightGray;
```

### Pattern 3: Sidebar Accent

```csharp
gradientLabel.BorderSides = Border3DSide.Left;
gradientLabel.BorderStyle = Border3DStyle.Flat;
gradientLabel.BorderColor = Color.Blue;
```

### Pattern 4: Minimalist

```csharp
gradientLabel.BorderStyle = Border3DStyle.Adjust;  // No border
```

---

## Troubleshooting

### Border Not Visible

**Check:**
1. BorderStyle is not set to `Adjust`
2. BorderSides includes the expected sides
3. Control size is adequate
4. BorderColor contrasts with background (for Flat style)

### BorderColor Not Working

**Solution:** BorderColor only works with `Flat` or `FixedSingle` styles.

```csharp
// Correct
gradientLabel.BorderStyle = Border3DStyle.Flat;
gradientLabel.BorderColor = Color.Red;  // Will work

// Incorrect
gradientLabel.BorderStyle = Border3DStyle.Raised;
gradientLabel.BorderColor = Color.Red;  // Has no effect
```

### 3D Effect Too Subtle

**Solution:** Try different 3D styles or increase control size.

```csharp
// More pronounced effect
gradientLabel.BorderStyle = Border3DStyle.Raised;  // Instead of RaisedOuter

// Larger control shows 3D better
gradientLabel.Size = new Size(200, 60);  // Instead of 100x30
```

---

## Related Topics

- **Background Styling**: Configure gradients → [background-styling.md](background-styling.md)
- **Getting Started**: Basic setup → [getting-started.md](getting-started.md)
- **Foreground Settings**: Text appearance → [foreground-text-settings.md](foreground-text-settings.md)
