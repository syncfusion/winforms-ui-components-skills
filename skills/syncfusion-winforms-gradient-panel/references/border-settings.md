# Border Settings

## Table of Contents
- [Overview](#overview)
- [2D Borders FixedSingle](#2d-borders-fixedsingle)
- [3D Borders Fixed3D](#3d-borders-fixed3d)
- [Border Sides](#border-sides)
- [Border Comparison](#border-comparison)
- [Common Scenarios](#common-scenarios)
- [Troubleshooting](#troubleshooting)

## Overview

GradientPanel supports both 2D and 3D border styles with extensive customization options. Borders provide visual definition and depth to panel containers.

**Border capabilities:**
- 2D borders with custom colors
- 3D borders with 10+ style options
- Selective border sides
- Multiple border styles (solid, dotted, dashed, etc.)

**Key properties:**
- `BorderStyle` - Choose between 2D (FixedSingle) or 3D (Fixed3D)
- `BorderColor` - Color for 2D borders
- `Border3DStyle` - Style for 3D borders
- `BorderSingle` - 2D border line style
- `BorderSides` - Which sides have borders

## 2D Borders (FixedSingle)

### Basic 2D Border

```csharp
gradientPanel1.BorderStyle = BorderStyle.FixedSingle;
```

**BorderStyle.FixedSingle:**
- Creates a 2D flat border
- Customizable color via `BorderColor`
- Customizable line style via `BorderSingle`

### BorderColor Property

Set the color for 2D borders:

```csharp
gradientPanel1.BorderStyle = BorderStyle.FixedSingle;
gradientPanel1.BorderColor = Color.Blue;
```

**Important:** `BorderColor` only works when `BorderStyle` is set to `FixedSingle`.

### BorderSingle Property

Customize the 2D border line style:

```csharp
gradientPanel1.BorderStyle = BorderStyle.FixedSingle;
gradientPanel1.BorderColor = Color.Blue;
gradientPanel1.BorderSingle = ButtonBorderStyle.Dashed;
```

**ButtonBorderStyle options:**

#### Solid (default)
Continuous solid line:

```csharp
gradientPanel1.BorderSingle = ButtonBorderStyle.Solid;
```

#### Dotted
Dotted line border:

```csharp
gradientPanel1.BorderSingle = ButtonBorderStyle.Dotted;
```

#### Dashed
Dashed line border:

```csharp
gradientPanel1.BorderSingle = ButtonBorderStyle.Dashed;
```

#### Inset
Border appears inset (sunken):

```csharp
gradientPanel1.BorderSingle = ButtonBorderStyle.Inset;
```

#### Outset
Border appears raised (outset):

```csharp
gradientPanel1.BorderSingle = ButtonBorderStyle.Outset;
```

#### None
No border:

```csharp
gradientPanel1.BorderSingle = ButtonBorderStyle.None;
```

### Complete 2D Border Example

```csharp
GradientPanel panel = new GradientPanel();
panel.Size = new Size(300, 200);
panel.Location = new Point(20, 20);

// Background
panel.BackgroundColor = new BrushInfo(Color.LightBlue);

// 2D border settings
panel.BorderStyle = BorderStyle.FixedSingle;
panel.BorderColor = Color.Blue;
panel.BorderSingle = ButtonBorderStyle.Dashed;

this.Controls.Add(panel);
```

![2D Border](../../../docs/GradientPanel-Images/Overview_img371.jpeg)

## 3D Borders (Fixed3D)

### Basic 3D Border

```csharp
gradientPanel1.BorderStyle = BorderStyle.Fixed3D;
```

**BorderStyle.Fixed3D:**
- Creates a 3D raised or sunken border
- Style controlled by `Border3DStyle` property
- Provides depth and dimension

### Border3DStyle Property

Set the 3D border style:

```csharp
gradientPanel1.BorderStyle = BorderStyle.Fixed3D;
gradientPanel1.Border3DStyle = Border3DStyle.Etched;
```

**Border3DStyle options (10 styles):**

#### RaisedOuter
Raised border, outer edge only:

```csharp
gradientPanel1.Border3DStyle = Border3DStyle.RaisedOuter;
```

**Visual:** Light edge on top/left, panel appears slightly raised

#### RaisedInner
Raised border, inner edge only:

```csharp
gradientPanel1.Border3DStyle = Border3DStyle.RaisedInner;
```

#### SunkenOuter
Sunken border, outer edge only:

```csharp
gradientPanel1.Border3DStyle = Border3DStyle.SunkenOuter;
```

**Visual:** Dark edge on top/left, panel appears slightly recessed

#### SunkenInner
Sunken border, inner edge only:

```csharp
gradientPanel1.Border3DStyle = Border3DStyle.SunkenInner;
```

#### Raised
Full raised border (RaisedOuter + SunkenInner):

```csharp
gradientPanel1.Border3DStyle = Border3DStyle.Raised;
```

**Visual:** Panel appears to protrude from surface

**Use for:** Buttons, active panels, highlighted sections

#### Sunken
Full sunken border (SunkenOuter + RaisedInner):

```csharp
gradientPanel1.Border3DStyle = Border3DStyle.Sunken;
```

**Visual:** Panel appears recessed into surface

**Use for:** Input areas, content wells, inactive panels

#### Etched
Etched (carved) border:

```csharp
gradientPanel1.Border3DStyle = Border3DStyle.Etched;
```

**Visual:** Carved-in appearance, decorative

**Use for:** Separators, decorative panels, section dividers

![3D Etched Border](../../../docs/GradientPanel-Images/Overview_img370.jpeg)

#### Bump
Raised bump border (opposite of etched):

```csharp
gradientPanel1.Border3DStyle = Border3DStyle.Bump;
```

**Visual:** Raised ridge, opposite of etched

**Use for:** Separators, dividers, decorative raised sections

#### Adjust
Adjusts border sides to align with edge:

```csharp
gradientPanel1.Border3DStyle = Border3DStyle.Adjust;
```

#### Flat
Flat 3D border (minimal depth):

```csharp
gradientPanel1.Border3DStyle = Border3DStyle.Flat;
```

**Visual:** Subtle 3D effect, nearly flat

**Use for:** Minimal 3D appearance, modern flat design

### Complete 3D Border Example

```csharp
GradientPanel panel3D = new GradientPanel();
panel3D.Size = new Size(300, 200);
panel3D.Location = new Point(20, 20);

// Background
panel3D.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.LightGray,
    Color.DarkGray
);

// 3D border
panel3D.BorderStyle = BorderStyle.Fixed3D;
panel3D.Border3DStyle = Border3DStyle.Etched;

this.Controls.Add(panel3D);
```

## Border Sides

### BorderSides Property

Specify which sides of the panel should have borders:

```csharp
gradientPanel1.BorderSides = Border3DSide.All;  // Default
```

**Border3DSide options:**

#### All
All four sides (default):

```csharp
gradientPanel1.BorderSides = Border3DSide.All;
```

#### Top
Top side only:

```csharp
gradientPanel1.BorderSides = Border3DSide.Top;
```

#### Bottom
Bottom side only:

```csharp
gradientPanel1.BorderSides = Border3DSide.Bottom;
```

#### Left
Left side only:

```csharp
gradientPanel1.BorderSides = Border3DSide.Left;
```

#### Right
Right side only:

```csharp
gradientPanel1.BorderSides = Border3DSide.Right;
```

### Combining Border Sides

Use bitwise OR to combine sides:

```csharp
// Top and bottom only
gradientPanel1.BorderSides = Border3DSide.Top | Border3DSide.Bottom;

// Left and right only
gradientPanel1.BorderSides = Border3DSide.Left | Border3DSide.Right;

// Top, left, and right (no bottom)
gradientPanel1.BorderSides = Border3DSide.Top | Border3DSide.Left | Border3DSide.Right;
```

### Selective Border Example

```csharp
// Header panel with bottom border only
GradientPanel headerPanel = new GradientPanel();
headerPanel.Dock = DockStyle.Top;
headerPanel.Height = 60;
headerPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.DarkBlue,
    Color.LightBlue
);

// Border only on bottom
headerPanel.BorderStyle = BorderStyle.FixedSingle;
headerPanel.BorderColor = Color.White;
headerPanel.BorderSides = Border3DSide.Bottom;

this.Controls.Add(headerPanel);
```

## Border Comparison

### 2D vs 3D Comparison

```csharp
private void CreateBorderComparison()
{
    // 2D border panel
    GradientPanel panel2D = new GradientPanel();
    panel2D.Size = new Size(200, 150);
    panel2D.Location = new Point(20, 20);
    panel2D.BackgroundColor = new BrushInfo(Color.LightBlue);
    panel2D.BorderStyle = BorderStyle.FixedSingle;
    panel2D.BorderColor = Color.Blue;
    
    Label label2D = new Label();
    label2D.Text = "2D Border (FixedSingle)";
    label2D.BackColor = Color.Transparent;
    label2D.Location = new Point(10, 10);
    label2D.AutoSize = true;
    panel2D.Controls.Add(label2D);
    
    this.Controls.Add(panel2D);
    
    // 3D border panel
    GradientPanel panel3D = new GradientPanel();
    panel3D.Size = new Size(200, 150);
    panel3D.Location = new Point(240, 20);
    panel3D.BackgroundColor = new BrushInfo(Color.LightGreen);
    panel3D.BorderStyle = BorderStyle.Fixed3D;
    panel3D.Border3DStyle = Border3DStyle.Raised;
    
    Label label3D = new Label();
    label3D.Text = "3D Border (Fixed3D)";
    label3D.BackColor = Color.Transparent;
    label3D.Location = new Point(10, 10);
    label3D.AutoSize = true;
    panel3D.Controls.Add(label3D);
    
    this.Controls.Add(panel3D);
}
```

### All 3D Border Styles

```csharp
private void ShowAll3DStyles()
{
    var styles = new[]
    {
        (Border3DStyle.RaisedOuter, "RaisedOuter"),
        (Border3DStyle.RaisedInner, "RaisedInner"),
        (Border3DStyle.SunkenOuter, "SunkenOuter"),
        (Border3DStyle.SunkenInner, "SunkenInner"),
        (Border3DStyle.Raised, "Raised"),
        (Border3DStyle.Sunken, "Sunken"),
        (Border3DStyle.Etched, "Etched"),
        (Border3DStyle.Bump, "Bump"),
        (Border3DStyle.Flat, "Flat")
    };
    
    int x = 10, y = 10;
    
    foreach (var (style, name) in styles)
    {
        GradientPanel panel = new GradientPanel();
        panel.Size = new Size(150, 100);
        panel.Location = new Point(x, y);
        panel.BackgroundColor = new BrushInfo(Color.LightGray);
        panel.BorderStyle = BorderStyle.Fixed3D;
        panel.Border3DStyle = style;
        
        Label label = new Label();
        label.Text = name;
        label.BackColor = Color.Transparent;
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

## Common Scenarios

### Scenario 1: Card with Subtle Border

```csharp
GradientPanel card = new GradientPanel();
card.Size = new Size(200, 150);
card.BackgroundColor = new BrushInfo(
    GradientStyle.PathRectangle,
    Color.AliceBlue,
    Color.SteelBlue
);

// Subtle 2D border
card.BorderStyle = BorderStyle.FixedSingle;
card.BorderColor = Color.SteelBlue;

this.Controls.Add(card);
```

### Scenario 2: Raised Button Panel

```csharp
GradientPanel buttonPanel = new GradientPanel();
buttonPanel.Size = new Size(120, 40);

// Raised 3D border for button-like appearance
buttonPanel.BorderStyle = BorderStyle.Fixed3D;
buttonPanel.Border3DStyle = Border3DStyle.Raised;

buttonPanel.BackgroundColor = new BrushInfo(Color.LightGray);

// Click handler for button behavior
buttonPanel.Click += (s, e) => {
    // Change to sunken on click
    buttonPanel.Border3DStyle = Border3DStyle.Sunken;
};

this.Controls.Add(buttonPanel);
```

### Scenario 3: Content Well with Sunken Border

```csharp
GradientPanel contentWell = new GradientPanel();
contentWell.Dock = DockStyle.Fill;

// Sunken border for content area
contentWell.BorderStyle = BorderStyle.Fixed3D;
contentWell.Border3DStyle = Border3DStyle.Sunken;

contentWell.BackgroundColor = new BrushInfo(Color.White);
contentWell.AutoScroll = true;

this.Controls.Add(contentWell);
```

### Scenario 4: Section Divider

```csharp
GradientPanel divider = new GradientPanel();
divider.Dock = DockStyle.Top;
divider.Height = 2;

// Etched border for separator effect
divider.BorderStyle = BorderStyle.Fixed3D;
divider.Border3DStyle = Border3DStyle.Etched;

this.Controls.Add(divider);
```

### Scenario 5: Header with Bottom Border

```csharp
GradientPanel header = new GradientPanel();
header.Dock = DockStyle.Top;
header.Height = 80;
header.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.DarkSlateBlue,
    Color.SlateBlue
);

// Border only on bottom
header.BorderStyle = BorderStyle.FixedSingle;
header.BorderColor = Color.White;
header.BorderSides = Border3DSide.Bottom;

Label headerLabel = new Label();
headerLabel.Text = "Application Header";
headerLabel.Font = new Font("Segoe UI", 18, FontStyle.Bold);
headerLabel.ForeColor = Color.White;
headerLabel.BackColor = Color.Transparent;
headerLabel.Location = new Point(20, 25);
header.Controls.Add(headerLabel);

this.Controls.Add(header);
```

## Troubleshooting

### Issue: BorderColor not applying

**Cause:** `BorderStyle` is not set to `FixedSingle`

**Solution:**
```csharp
// BorderColor only works with FixedSingle
gradientPanel1.BorderStyle = BorderStyle.FixedSingle;  // Required
gradientPanel1.BorderColor = Color.Blue;
```

### Issue: 3D border not visible

**Causes:**
- BorderStyle not set to Fixed3D
- Background color too similar to border

**Solution:**
```csharp
// Ensure Fixed3D is set
gradientPanel1.BorderStyle = BorderStyle.Fixed3D;
gradientPanel1.Border3DStyle = Border3DStyle.Raised;

// Use contrasting background
gradientPanel1.BackgroundColor = new BrushInfo(Color.LightGray);
```

### Issue: BorderSingle not working

**Cause:** BorderStyle must be FixedSingle

**Solution:**
```csharp
gradientPanel1.BorderStyle = BorderStyle.FixedSingle;  // Required
gradientPanel1.BorderSingle = ButtonBorderStyle.Dashed;
gradientPanel1.BorderColor = Color.Black;
```

### Issue: Selective borders not showing

**Cause:** BorderSides not configured correctly

**Solution:**
```csharp
// Ensure BorderStyle is set first
gradientPanel1.BorderStyle = BorderStyle.FixedSingle;
gradientPanel1.BorderColor = Color.Black;

// Then set specific sides
gradientPanel1.BorderSides = Border3DSide.Top | Border3DSide.Bottom;
```

## Best Practices

1. **Match border to style** - Use FixedSingle for flat designs, Fixed3D for depth
2. **BorderColor with FixedSingle** - Remember BorderColor only works with FixedSingle
3. **Contrast for visibility** - Use contrasting border colors for clear definition
4. **Raised for buttons** - Use Raised 3D border for button-like panels
5. **Sunken for inputs** - Use Sunken 3D border for content wells and input areas
6. **Etched for separators** - Use Etched or Bump for visual dividers
7. **Selective borders** - Use BorderSides for header/footer borders only
8. **Consistent borders** - Use same border style for related panels
9. **Test 3D effects** - 3D borders may not be visible on all backgrounds
10. **Flat modern design** - Consider no border or subtle 2D borders for modern flat UI
