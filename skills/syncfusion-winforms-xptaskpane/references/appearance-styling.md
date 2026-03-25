# Appearance and Styling

## Table of Contents
- [Visual Styles](#visual-styles)
- [Font and Foreground Color](#font-and-foreground-color)
- [Task Page Borders](#task-page-borders)
- [Header Customization](#header-customization)
- [Complete Styling Example](#complete-styling-example)

## Visual Styles

XPTaskPane supports two built-in visual themes:

### OfficeXP Style

Classic Office XP appearance with traditional blue colors:

```csharp
xpTaskPane1.VisualStyle = VisualStyle.OfficeXP;
```

```vb
Me.xpTaskPane1.VisualStyle = VisualStyle.OfficeXP
```

**Characteristics:**
- Traditional Office XP look
- Lighter color scheme
- Suited for classic applications
- Default style for older projects

### Office2007 Style

Modern Office 2007+ appearance with gradient backgrounds:

```csharp
xpTaskPane1.VisualStyle = VisualStyle.Office2007;
```

```vb
Me.xpTaskPane1.VisualStyle = VisualStyle.Office2007
```

**Characteristics:**
- Contemporary Office 2007 look
- Gradient backgrounds
- Smoother appearance
- Recommended for modern applications

**Setting Style During Initialization:**

```csharp
public Form1()
{
    InitializeComponent();
    xpTaskPane1.VisualStyle = VisualStyle.Office2007;
}
```

## Font and Foreground Color

### Global Font Settings

Set font for entire XPTaskPane:

```csharp
// Set font for task pane and pages
xpTaskPane1.Font = new Font("Verdana", 8.25F);
```

```vb
' Set font for task pane and pages
Me.xpTaskPane1.Font = New Font("Verdana", 8.25F)
```

**Font Properties:**

```csharp
// Font with styling
xpTaskPane1.Font = new Font("Segoe UI", 9F, FontStyle.Regular);

// Different styling options
Font boldFont = new Font("Arial", 10F, FontStyle.Bold);
Font italicFont = new Font("Arial", 10F, FontStyle.Italic);
Font underlineFont = new Font("Arial", 10F, FontStyle.Underline);
Font boldItalic = new Font("Arial", 10F, FontStyle.Bold | FontStyle.Italic);
```

### Global Foreground Color

Set text color for task pane:

```csharp
// Set text color
xpTaskPane1.ForeColor = System.Drawing.Color.SteelBlue;
```

```vb
' Set text color
Me.xpTaskPane1.ForeColor = System.Drawing.Color.SteelBlue
```

**Color Options:**

```csharp
// Named colors
xpTaskPane1.ForeColor = Color.Navy;
xpTaskPane1.ForeColor = Color.DarkGray;
xpTaskPane1.ForeColor = Color.Black;

// RGB colors
xpTaskPane1.ForeColor = Color.FromArgb(64, 64, 64);

// Hex colors (via int)
xpTaskPane1.ForeColor = Color.FromArgb(0x404040);
```

### Per-Page Font Override

Override global font for individual pages:

```csharp
// Page 1 with different font
xpTaskPage1.Font = new Font("Arial", 9F, FontStyle.Bold);

// Page 2 with default font (inherits from parent)
xpTaskPage2.Font = xpTaskPane1.Font;

// Page 3 with italic font
xpTaskPage3.Font = new Font("Verdana", 8.5F, FontStyle.Italic);
```

```vb
' Page 1 with different font
xpTaskPage1.Font = New Font("Arial", 9F, FontStyle.Bold)

' Page 2 with default font
xpTaskPage2.Font = xpTaskPane1.Font

' Page 3 with italic font
xpTaskPage3.Font = New Font("Verdana", 8.5F, FontStyle.Italic)
```

**Note:** Individual page fonts override XPTaskPane.Font setting.

### Per-Page Foreground Override

Override global color for individual pages:

```csharp
// Page 1 with red text
xpTaskPage1.ForeColor = Color.Red;

// Page 2 with blue text
xpTaskPage2.ForeColor = Color.Blue;

// Page 3 with default color (inherits from parent)
xpTaskPage3.ForeColor = xpTaskPane1.ForeColor;
```

## Task Page Borders

Configure border appearance for individual task pages:

### Border Style

Set 2D or 3D border style:

```csharp
// Set 2D border style
xpTaskPage1.BorderStyle = BorderStyle.FixedSingle;

// Set 3D border style
xpTaskPage1.BorderStyle = BorderStyle.Fixed3D;

// No border
xpTaskPage1.BorderStyle = BorderStyle.None;
```

```vb
' Set 2D border style
xpTaskPage1.BorderStyle = BorderStyle.FixedSingle

' Set 3D border style
xpTaskPage1.BorderStyle = BorderStyle.Fixed3D

' No border
xpTaskPage1.BorderStyle = BorderStyle.None
```

### 3D Border Style

When BorderStyle is Fixed3D, customize 3D appearance:

```csharp
// Various 3D styles
xpTaskPage1.Border3DStyle = Border3DStyle.Raised;
xpTaskPage1.Border3DStyle = Border3DStyle.RaisedOuter;
xpTaskPage1.Border3DStyle = Border3DStyle.RaisedInner;
xpTaskPage1.Border3DStyle = Border3DStyle.Sunken;
xpTaskPage1.Border3DStyle = Border3DStyle.SunkenOuter;
xpTaskPage1.Border3DStyle = Border3DStyle.SunkenInner;
xpTaskPage1.Border3DStyle = Border3DStyle.Etched;
xpTaskPage1.Border3DStyle = Border3DStyle.Bump;
xpTaskPage1.Border3DStyle = Border3DStyle.Adjust;
xpTaskPage1.Border3DStyle = Border3DStyle.Flat;
```

### 2D Border Style

When BorderStyle is FixedSingle, customize 2D line style:

```csharp
// Various 2D line styles
xpTaskPage1.BorderSingle = Border2DStyle.Solid;
xpTaskPage1.BorderSingle = Border2DStyle.Dotted;
xpTaskPage1.BorderSingle = Border2DStyle.Dashed;
xpTaskPage1.BorderSingle = Border2DStyle.Inset;
xpTaskPage1.BorderSingle = Border2DStyle.Outset;
```

### Border Color

Set the color of the border:

```csharp
xpTaskPage1.BorderColor = System.Drawing.Color.SteelBlue;
xpTaskPage1.BorderColor = Color.FromArgb(100, 150, 200);
xpTaskPage1.BorderColor = Color.DarkGray;
```

```vb
Me.xpTaskPage1.BorderColor = System.Drawing.Color.SteelBlue
Me.xpTaskPage1.BorderColor = Color.FromArgb(100, 150, 200)
Me.xpTaskPage1.BorderColor = Color.DarkGray
```

### Border Sides

Specify which sides show the border:

```csharp
// Border on all sides (default)
xpTaskPage1.BorderSides = Border3DSide.All;

// Border on left and right only
xpTaskPage1.BorderSides = Border3DSide.Left | Border3DSide.Right;

// Border on top and bottom only
xpTaskPage1.BorderSides = Border3DSide.Top | Border3DSide.Bottom;

// Border on top only
xpTaskPage1.BorderSides = Border3DSide.Top;
```

### Complete Border Configuration

```csharp
private void ConfigurePageBorder(XPTaskPage page, Color color)
{
    page.BorderStyle = BorderStyle.FixedSingle;
    page.BorderSingle = Border2DStyle.Solid;
    page.BorderColor = color;
    page.BorderSides = Border3DSide.All;
}

// Apply to pages
ConfigurePageBorder(xpTaskPage1, Color.SteelBlue);
ConfigurePageBorder(xpTaskPage2, Color.Navy);
ConfigurePageBorder(xpTaskPage3, Color.DarkGray);
```

## Header Customization

Customize header label styling:

### Header Label Font

```csharp
// Bold header font
xpTaskPane1.HeaderLabel.Font = new Font("Verdana", 9.75F, FontStyle.Bold);

// Large header font
xpTaskPane1.HeaderLabel.Font = new Font("Segoe UI", 11F, FontStyle.Bold);
```

```vb
' Bold header font
Me.xpTaskPane1.HeaderLabel.Font = New Font("Verdana", 9.75F, FontStyle.Bold)

' Large header font
Me.xpTaskPane1.HeaderLabel.Font = New Font("Segoe UI", 11F, FontStyle.Bold)
```

### Header Label Foreground Color

```csharp
// Dark blue header text
xpTaskPane1.HeaderLabel.ForeColor = System.Drawing.Color.Navy;

// Custom header color
xpTaskPane1.HeaderLabel.ForeColor = Color.FromArgb(0, 51, 102);
```

```vb
' Dark blue header text
Me.xpTaskPane1.HeaderLabel.ForeColor = System.Drawing.Color.Navy

' Custom header color
Me.xpTaskPane1.HeaderLabel.ForeColor = Color.FromArgb(0, 51, 102)
```

### Complete Header Styling Example

```csharp
private void StyleHeader()
{
    // Make header prominent
    xpTaskPane1.HeaderLabel.Font = new Font("Segoe UI", 10F, FontStyle.Bold);
    xpTaskPane1.HeaderLabel.ForeColor = Color.Navy;
}
```

## Complete Styling Example

```csharp
private void ApplyCompleteStyling()
{
    // Global visual style
    xpTaskPane1.VisualStyle = VisualStyle.Office2007;

    // Global font and colors
    xpTaskPane1.Font = new Font("Segoe UI", 9F);
    xpTaskPane1.ForeColor = Color.FromArgb(64, 64, 64);

    // Header styling
    xpTaskPane1.HeaderLabel.Font = new Font("Segoe UI", 10F, FontStyle.Bold);
    xpTaskPane1.HeaderLabel.ForeColor = Color.Navy;

    // Page 1 styling
    xpTaskPage1.Font = new Font("Segoe UI", 9F);
    xpTaskPage1.ForeColor = Color.Black;
    xpTaskPage1.BorderStyle = BorderStyle.FixedSingle;
    xpTaskPage1.BorderSingle = Border2DStyle.Solid;
    xpTaskPage1.BorderColor = Color.LightGray;

    // Page 2 styling
    xpTaskPage2.Font = new Font("Segoe UI", 9F);
    xpTaskPage2.ForeColor = Color.Black;
    xpTaskPage2.BorderStyle = BorderStyle.Fixed3D;
    xpTaskPage2.Border3DStyle = Border3DStyle.Raised;
    xpTaskPage2.BorderColor = Color.Silver;

    // Page 3 with different font
    xpTaskPage3.Font = new Font("Arial", 9F, FontStyle.Regular);
    xpTaskPage3.ForeColor = Color.FromArgb(50, 100, 150);
}
```

## Styling Best Practices

**Consistency:**
- Use same visual style throughout application
- Choose one color scheme (navy/gray or similar)
- Use complementary fonts

**Readability:**
- Maintain sufficient contrast between text and background
- Don't mix too many font styles
- Use bold sparingly for headers

**Performance:**
- Set styling once during initialization
- Avoid changing styles frequently at runtime
- Cache Font objects when used multiple times

**Accessibility:**
- Ensure sufficient color contrast
- Don't rely solely on color to convey information
- Use consistent navigation appearance

**Next:** Learn about content scrolling in scrolling-content.md
