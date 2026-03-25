# Styling and Appearance - WinForms Rating Control

This reference covers all styling and visual customization options for the Rating control, including predefined visual styles, Office themes, and custom gradient coloring.

## Table of Contents

- [Visual Styles Overview](#visual-styles-overview)
- [Predefined Visual Styles](#predefined-visual-styles)
- [Office Color Schemes](#office-color-schemes)
- [Office 2016 Style Variants](#office-2016-style-variants)
- [Custom Gradient Coloring](#custom-gradient-coloring)
- [Solid Color Customization](#solid-color-customization)
- [Border and Background Colors](#border-and-background-colors)
- [Complete Styling Examples](#complete-styling-examples)
- [Style Selection Guide](#style-selection-guide)

## Visual Styles Overview

The Rating control provides predefined visual styles through the `VisualStyle` property. These styles ensure consistency with Microsoft Office applications and modern UI design patterns.

### Available Style Categories

1. **Classic Styles**: Default, Metro
2. **Office Legacy**: Office2007, Office2010
3. **Office Modern**: Office2016Colorful, Office2016DarkGray, Office2016White, Office2016Black
4. **Custom**: User-defined gradient and solid colors

## Predefined Visual Styles

### VisualStyle Property

The `VisualStyle` property uses the `Syncfusion.Windows.Forms.Tools.RatingControl.Style` enum:

```csharp
using Syncfusion.Windows.Forms.Tools;

// Access the Style enum through RatingControl
ratingControl1.VisualStyle = RatingControl.Style.Default;
```

### Default Style

The default style provides a clean, neutral appearance suitable for any application.

```csharp
private void ApplyDefaultStyle()
{
    var rating = new RatingControl
    {
        Location = new Point(50, 50),
        Size = new Size(200, 40),
        VisualStyle = RatingControl.Style.Default,
        Shape = Shapes.Star,
        Value = 4
    };

    this.Controls.Add(rating);
}
```

**Use Case:** General-purpose applications, minimal branding requirements, neutral interfaces.

### Metro Style

Modern, flat design with clean lines and solid colors.

```csharp
ratingControl1.VisualStyle = RatingControl.Style.Metro;
```

**Use Case:** Modern Windows applications, touch-optimized interfaces, minimalist designs.

### Office2007 Style

Classic Office 2007 appearance with rounded corners and subtle gradients.

```csharp
ratingControl1.VisualStyle = RatingControl.Style.Office2007;
```

**Use Case:** Legacy Office integration, enterprise applications matching Office 2007 environments.

### Office2010 Style

Office 2010 styling with refined visual elements.

```csharp
ratingControl1.VisualStyle = RatingControl.Style.Office2010;
```

**Use Case:** Applications targeting Office 2010 consistency, business productivity tools.

## Office Color Schemes

Office styles (2007, 2010) support color scheme variations via the `OfficeColorScheme` property.

### Available Color Schemes

1. **Blue** (Default) - Professional blue theme
2. **Silver** - Neutral gray/silver theme
3. **Black** - Dark, high-contrast theme

### Applying Office Color Schemes

```csharp
using Syncfusion.Windows.Forms;

// Office2007 with Blue color scheme
ratingControl1.VisualStyle = RatingControl.Style.Office2007;
ratingControl1.OfficeColorScheme = Office2007Theme.Blue;

// Office2007 with Silver color scheme
ratingControl2.VisualStyle = RatingControl.Style.Office2007;
ratingControl2.OfficeColorScheme = Office2007Theme.Silver;

// Office2007 with Black color scheme
ratingControl3.VisualStyle = RatingControl.Style.Office2007;
ratingControl3.OfficeColorScheme = Office2007Theme.Black;
```

### Office2010 Color Scheme Example

```csharp
// Office2010 with different color schemes
ratingControl1.VisualStyle = RatingControl.Style.Office2010;
ratingControl1.OfficeColorScheme = Office2010Theme.Blue;    // Professional
ratingControl2.OfficeColorScheme = Office2010Theme.Silver;  // Neutral
ratingControl3.OfficeColorScheme = Office2010Theme.Black;   // Dark
```

**When to Use Color Schemes:**
- **Blue**: Default corporate environments, professional applications
- **Silver**: Neutral interfaces, multi-themed applications
- **Black**: High-contrast mode, dark UI themes, accessibility

## Office 2016 Style Variants

Office 2016 styles provide modern, refined appearances matching current Microsoft Office applications.

### Office2016Colorful

Vibrant, colorful theme with accent colors.

```csharp
ratingControl1.VisualStyle = RatingControl.Style.Office2016Colorful;
```

**Use Case:** Modern Office 365 applications, colorful interfaces, engaging user experiences.

**Visual Characteristics:**
- Bright accent colors
- High visual contrast
- Modern, flat design

### Office2016White

Clean white background with subtle accents.

```csharp
ratingControl1.VisualStyle = RatingControl.Style.Office2016White;
```

**Use Case:** Light-themed applications, minimalist designs, professional documents.

**Visual Characteristics:**
- White/light gray backgrounds
- Subtle borders
- Clean, uncluttered appearance

### Office2016DarkGray

Medium-dark gray theme.

```csharp
ratingControl1.VisualStyle = RatingControl.Style.Office2016DarkGray;
```

**Use Case:** Semi-dark themes, reduced eye strain, modern professional applications.

**Visual Characteristics:**
- Medium-dark gray backgrounds
- Balanced contrast
- Professional appearance

### Office2016Black

True dark theme for low-light environments.

```csharp
ratingControl1.VisualStyle = RatingControl.Style.Office2016Black;
```

**Use Case:** Dark mode applications, low-light environments, reduced eye strain, accessibility.

**Visual Characteristics:**
- Dark/black backgrounds
- High contrast with light elements
- Optimal for dark mode

### Office 2016 Variants Comparison Example

```csharp
private void DemonstrateOffice2016Variants()
{
    int yPos = 20;
    int spacing = 70;

    // Office2016Colorful
    CreateLabeledRating("Office 2016 Colorful:", yPos,
        RatingControl.Style.Office2016Colorful);

    // Office2016White
    yPos += spacing;
    CreateLabeledRating("Office 2016 White:", yPos,
        RatingControl.Style.Office2016White);

    // Office2016DarkGray
    yPos += spacing;
    CreateLabeledRating("Office 2016 Dark Gray:", yPos,
        RatingControl.Style.Office2016DarkGray);

    // Office2016Black
    yPos += spacing;
    CreateLabeledRating("Office 2016 Black:", yPos,
        RatingControl.Style.Office2016Black);
}

private void CreateLabeledRating(string labelText, int yPosition, 
    RatingControl.Style style)
{
    Label lbl = new Label
    {
        Text = labelText,
        Location = new Point(20, yPosition + 5),
        AutoSize = true
    };

    RatingControl rating = new RatingControl
    {
        Location = new Point(200, yPosition),
        Size = new Size(200, 40),
        VisualStyle = style,
        Value = 4
    };

    this.Controls.Add(lbl);
    this.Controls.Add(rating);
}
```

## Custom Gradient Coloring

Create custom visual appearances using gradient colors for hover and selection states.

### Gradient Color Properties

The Rating control provides four gradient color properties:

1. **ItemHighlightStartColor** - Gradient start color for hover state
2. **ItemHighlightEndColor** - Gradient end color for hover state
3. **ItemSelectionStartColor** - Gradient start color for selected state
4. **ItemSelectionEndColor** - Gradient end color for selected state

### Enabling Gradient Colors

**Critical Requirement:** Set `ApplyGradientColors = true` to enable gradient properties.

```csharp
// Enable gradient color mode
ratingControl1.ApplyGradientColors = true;
```

**Edge Case:** If `ApplyGradientColors` is false, gradient color properties are ignored, and solid colors (`ItemHighlightColor`, `ItemSelectionColor`) are used instead.

### Hover Gradient Configuration

```csharp
// Configure hover gradient (blue to cyan)
ratingControl1.ApplyGradientColors = true;
ratingControl1.ItemHighlightStartColor = Color.FromArgb(0, 120, 215); // Blue
ratingControl1.ItemHighlightEndColor = Color.FromArgb(0, 204, 255);   // Cyan
```

### Selection Gradient Configuration

```csharp
// Configure selection gradient (gold to orange)
ratingControl1.ApplyGradientColors = true;
ratingControl1.ItemSelectionStartColor = Color.FromArgb(255, 215, 0);  // Gold
ratingControl1.ItemSelectionEndColor = Color.FromArgb(255, 140, 0);    // Dark orange
```

### Complete Gradient Example

```csharp
private void ApplyCustomGradient()
{
    var gradientRating = new RatingControl
    {
        Location = new Point(50, 50),
        Size = new Size(250, 50),
        Shape = Shapes.Star,
        Value = 3,
        
        // Enable gradient mode
        ApplyGradientColors = true,
        
        // Hover gradient (light blue to dark blue)
        ItemHighlightStartColor = Color.LightBlue,
        ItemHighlightEndColor = Color.DarkBlue,
        
        // Selection gradient (yellow to red)
        ItemSelectionStartColor = Color.Yellow,
        ItemSelectionEndColor = Color.Red
    };

    this.Controls.Add(gradientRating);
}
```

### Preset Gradient Themes

```csharp
private void ApplyGradientTheme(string theme)
{
    ratingControl1.ApplyGradientColors = true;

    switch (theme.ToLower())
    {
        case "ocean":
            // Ocean theme: Light blue to deep blue
            ratingControl1.ItemHighlightStartColor = Color.FromArgb(173, 216, 230);
            ratingControl1.ItemHighlightEndColor = Color.FromArgb(0, 105, 148);
            ratingControl1.ItemSelectionStartColor = Color.FromArgb(135, 206, 250);
            ratingControl1.ItemSelectionEndColor = Color.FromArgb(25, 25, 112);
            break;

        case "sunset":
            // Sunset theme: Yellow to purple
            ratingControl1.ItemHighlightStartColor = Color.FromArgb(255, 223, 0);
            ratingControl1.ItemHighlightEndColor = Color.FromArgb(255, 99, 71);
            ratingControl1.ItemSelectionStartColor = Color.FromArgb(255, 140, 0);
            ratingControl1.ItemSelectionEndColor = Color.FromArgb(128, 0, 128);
            break;

        case "forest":
            // Forest theme: Light green to dark green
            ratingControl1.ItemHighlightStartColor = Color.FromArgb(144, 238, 144);
            ratingControl1.ItemHighlightEndColor = Color.FromArgb(34, 139, 34);
            ratingControl1.ItemSelectionStartColor = Color.FromArgb(50, 205, 50);
            ratingControl1.ItemSelectionEndColor = Color.FromArgb(0, 100, 0);
            break;

        case "fire":
            // Fire theme: Yellow to dark red
            ratingControl1.ItemHighlightStartColor = Color.FromArgb(255, 255, 0);
            ratingControl1.ItemHighlightEndColor = Color.FromArgb(255, 69, 0);
            ratingControl1.ItemSelectionStartColor = Color.FromArgb(255, 165, 0);
            ratingControl1.ItemSelectionEndColor = Color.FromArgb(139, 0, 0);
            break;

        default:
            // Default: Gold gradient
            ratingControl1.ItemHighlightStartColor = Color.FromArgb(255, 215, 0);
            ratingControl1.ItemHighlightEndColor = Color.FromArgb(218, 165, 32);
            ratingControl1.ItemSelectionStartColor = Color.Gold;
            ratingControl1.ItemSelectionEndColor = Color.Goldenrod;
            break;
    }
}
```

## Solid Color Customization

When `ApplyGradientColors` is false (default), use solid color properties.

### Solid Color Properties

1. **ItemHighlightColor** - Solid color for hover state
2. **ItemSelectionColor** - Solid color for selected state

### Applying Solid Colors

```csharp
private void ApplySolidColors()
{
    var solidRating = new RatingControl
    {
        Location = new Point(50, 50),
        Size = new Size(200, 40),
        
        // Gradient mode must be OFF for solid colors
        ApplyGradientColors = false,
        
        // Set solid colors
        ItemHighlightColor = Color.LightBlue,
        ItemSelectionColor = Color.Gold,
        
        Value = 3
    };

    this.Controls.Add(solidRating);
}
```

### Brand Color Example

```csharp
private void ApplyBrandColors()
{
    // Company brand colors
    Color brandPrimary = Color.FromArgb(0, 102, 204);    // Corporate blue
    Color brandSecondary = Color.FromArgb(255, 179, 0); // Corporate gold

    ratingControl1.ApplyGradientColors = false;
    ratingControl1.ItemHighlightColor = brandPrimary;
    ratingControl1.ItemSelectionColor = brandSecondary;
}
```

## Border and Background Colors

### BackColor Property

Sets the background color behind rating items.

```csharp
ratingControl1.BackColor = Color.WhiteSmoke;
```

### BorderColor Property

Sets the border color around the rating control.

```csharp
ratingControl1.BorderColor = Color.Gray;
```

### Combined Background and Border Example

```csharp
private void StyleWithBackground()
{
    var styledRating = new RatingControl
    {
        Location = new Point(50, 50),
        Size = new Size(220, 45),
        
        // Background and border
        BackColor = Color.FromArgb(245, 245, 245), // Light gray background
        BorderColor = Color.FromArgb(200, 200, 200), // Medium gray border
        
        // Colors
        ApplyGradientColors = false,
        ItemSelectionColor = Color.OrangeRed,
        ItemHighlightColor = Color.Orange,
        
        Value = 4
    };

    this.Controls.Add(styledRating);
}
```

## Complete Styling Examples

### Example 1: Dark Theme Rating

```csharp
private void CreateDarkThemeRating()
{
    var darkRating = new RatingControl
    {
        Location = new Point(50, 50),
        Size = new Size(250, 50),
        
        // Use Office 2016 Black style
        VisualStyle = RatingControl.Style.Office2016Black,
        
        // Dark theme colors
        BackColor = Color.FromArgb(30, 30, 30),
        BorderColor = Color.FromArgb(60, 60, 60),
        
        // Bright selection for contrast
        ApplyGradientColors = false,
        ItemSelectionColor = Color.FromArgb(255, 200, 0),
        ItemHighlightColor = Color.FromArgb(255, 230, 100),
        
        Shape = Shapes.Star,
        Precision = PrecisionMode.Half,
        Value = 4.5f,
        ShowTooltip = true
    };

    this.Controls.Add(darkRating);
}
```

### Example 2: Colorful Gradient Rating

```csharp
private void CreateColorfulGradientRating()
{
    var colorfulRating = new RatingControl
    {
        Location = new Point(50, 150),
        Size = new Size(250, 50),
        
        // Modern Office style
        VisualStyle = RatingControl.Style.Office2016Colorful,
        
        // Enable gradients
        ApplyGradientColors = true,
        
        // Rainbow gradient for selection
        ItemSelectionStartColor = Color.FromArgb(255, 0, 128),   // Pink
        ItemSelectionEndColor = Color.FromArgb(128, 0, 255),     // Purple
        
        // Soft hover gradient
        ItemHighlightStartColor = Color.FromArgb(255, 182, 193), // Light pink
        ItemHighlightEndColor = Color.FromArgb(221, 160, 221),   // Plum
        
        Shape = Shapes.Heart,
        Value = 5
    };

    this.Controls.Add(colorfulRating);
}
```

### Example 3: Professional Business Rating

```csharp
private void CreateBusinessRating()
{
    var businessRating = new RatingControl
    {
        Location = new Point(50, 250),
        Size = new Size(200, 40),
        
        // Professional Office 2016 White style
        VisualStyle = RatingControl.Style.Office2016White,
        
        // Conservative colors
        BackColor = Color.White,
        BorderColor = Color.FromArgb(210, 210, 210),
        
        ApplyGradientColors = false,
        ItemSelectionColor = Color.FromArgb(0, 102, 204),  // Professional blue
        ItemHighlightColor = Color.FromArgb(100, 149, 237), // Lighter blue
        
        Shape = Shapes.Star,
        Value = 4
    };

    this.Controls.Add(businessRating);
}
```

### Example 4: Dynamic Style Switcher

```csharp
private ComboBox cmbStyles;
private RatingControl dynamicRating;

private void CreateDynamicStyleExample()
{
    // Style selector
    cmbStyles = new ComboBox
    {
        Location = new Point(20, 20),
        Width = 200,
        DropDownStyle = ComboBoxStyle.DropDownList
    };
    cmbStyles.Items.AddRange(new object[] {
        "Default", "Metro", "Office2007", "Office2010",
        "Office2016Colorful", "Office2016White",
        "Office2016DarkGray", "Office2016Black"
    });
    cmbStyles.SelectedIndex = 0;
    cmbStyles.SelectedIndexChanged += CmbStyles_SelectedIndexChanged;

    // Rating control
    dynamicRating = new RatingControl
    {
        Location = new Point(20, 60),
        Size = new Size(250, 50),
        Value = 4,
        ShowTooltip = true
    };

    this.Controls.Add(cmbStyles);
    this.Controls.Add(dynamicRating);
}

private void CmbStyles_SelectedIndexChanged(object sender, EventArgs e)
{
    switch (cmbStyles.SelectedItem.ToString())
    {
        case "Default":
            dynamicRating.VisualStyle = RatingControl.Style.Default;
            break;
        case "Metro":
            dynamicRating.VisualStyle = RatingControl.Style.Metro;
            break;
        case "Office2007":
            dynamicRating.VisualStyle = RatingControl.Style.Office2007;
            break;
        case "Office2010":
            dynamicRating.VisualStyle = RatingControl.Style.Office2010;
            break;
        case "Office2016Colorful":
            dynamicRating.VisualStyle = RatingControl.Style.Office2016Colorful;
            break;
        case "Office2016White":
            dynamicRating.VisualStyle = RatingControl.Style.Office2016White;
            break;
        case "Office2016DarkGray":
            dynamicRating.VisualStyle = RatingControl.Style.Office2016DarkGray;
            break;
        case "Office2016Black":
            dynamicRating.VisualStyle = RatingControl.Style.Office2016Black;
            break;
    }
}
```

## Style Selection Guide

### Choose Default Style When:
- Building quick prototypes
- Neutral appearance required
- No specific branding guidelines

### Choose Metro Style When:
- Modern Windows 10/11 application
- Flat, minimalist design preference
- Touch-optimized interface

### Choose Office 2007/2010 When:
- Matching legacy Office environments
- Enterprise applications with established look
- Compatibility with older systems

### Choose Office 2016 Variants When:
- Modern Office 365 integration
- Supporting multiple theme modes (light/dark)
- Professional, current appearance

### Choose Custom Gradients When:
- Unique branding requirements
- Specific color scheme matching
- Creative, distinctive interfaces

## Troubleshooting

### Issue: Gradient colors not appearing

**Cause:** `ApplyGradientColors` is false.

**Solution:**
```csharp
// Ensure gradient mode is enabled
ratingControl1.ApplyGradientColors = true;
```

### Issue: Solid colors ignored

**Cause:** `ApplyGradientColors` is true, gradient colors take precedence.

**Solution:**
```csharp
// Disable gradient mode to use solid colors
ratingControl1.ApplyGradientColors = false;
```

### Issue: Office color scheme has no effect

**Cause:** Visual style is not Office2007 or Office2010.

**Solution:**
```csharp
// OfficeColorScheme only works with Office2007/2010 styles
ratingControl1.VisualStyle = RatingControl.Style.Office2007;
ratingControl1.OfficeColorScheme = Office2007Theme.Blue;
```
