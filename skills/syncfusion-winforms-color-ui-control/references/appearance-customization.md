# Appearance and Customization

This guide covers visual customization options for ColorUIControl including border styles, panel sizing, and tab text customization.

## Table of Contents
- [Overview](#overview)
- [Border Styles](#border-styles)
- [Panel Sizing and Stretching](#panel-sizing-and-stretching)
- [Tab Text Customization](#tab-text-customization)
- [Font Styling](#font-styling)
- [Complete Customization Examples](#complete-customization-examples)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

ColorUIControl provides several properties to customize its visual appearance:

- **BorderStyle** - Control the border appearance (FixedSingle, Fixed3D, None)
- **Panel Stretching** - Enable dynamic resizing of color panels
- **Tab Names** - Customize the text displayed on each color group tab
- **Font** - Change the font style for tab text

These customization options help integrate ColorUIControl seamlessly into your application's visual design.

## Border Styles

The `BorderStyle` property controls the appearance of the control's border. This affects the overall visual weight and integration with your form's design.

### Available Border Styles

```csharp
public enum BorderStyle
{
    None = 0,        // No border
    FixedSingle = 1, // Single-line border
    Fixed3D = 2      // 3D border (default)
}
```

### Setting Border Style

```csharp
// Fixed single-line border (flat, modern look)
this.colorUIControl1.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle;

// 3D border (classic Windows look, default)
this.colorUIControl1.BorderStyle = System.Windows.Forms.BorderStyle.Fixed3D;

// No border (seamless integration)
this.colorUIControl1.BorderStyle = System.Windows.Forms.BorderStyle.None;
```

### Border Style Use Cases

- **FixedSingle**: Modern flat UI designs, Material Design/Fluent UI
- **Fixed3D**: Traditional Windows applications (default)
- **None**: Custom containers, popup scenarios

### Example: Border Style Based on Theme

```csharp
private void ApplyTheme(string theme)
{
    switch (theme.ToLower())
    {
        case "modern":
            colorUIControl1.BorderStyle = BorderStyle.FixedSingle;
            break;
        case "classic":
            colorUIControl1.BorderStyle = BorderStyle.Fixed3D;
            break;
        case "minimal":
            colorUIControl1.BorderStyle = BorderStyle.None;
            break;
    }
}
```

## Panel Sizing and Stretching

ColorUIControl provides properties to enable dynamic stretching of the Custom and User color panels when the control is resized. This creates a more responsive and adaptive user interface.

### Panel Stretching Properties

Enable dynamic resizing of color panels when the control is resized:

```csharp
// Enable stretching for both panels
this.colorUIControl1.CustomColorsStretchOnResize = true;
this.colorUIControl1.UserColorsStretchOnResize = true;
this.colorUIControl1.Size = new System.Drawing.Size(300, 250);
```

**Benefits**: Responsive design, better visibility, flexible layouts, improved touch UX

### Example: Responsive Color Picker Panel

**C#:**
```csharp
private void SetupResponsiveColorPicker()
{
    // Create a panel to host the color picker
    Panel container = new Panel();
    container.Dock = DockStyle.Fill;
    container.Padding = new Padding(10);
    
    // Create ColorUIControl
    colorUIControl1 = new ColorUIControl();
    colorUIControl1.Dock = DockStyle.Fill;
    
    // Enable stretching for responsive behavior
    colorUIControl1.CustomColorsStretchOnResize = true;
    colorUIControl1.UserColorsStretchOnResize = true;
    
    // Modern appearance
    colorUIControl1.BorderStyle = BorderStyle.FixedSingle;
    
    // Add to container
    container.Controls.Add(colorUIControl1);
    this.Controls.Add(container);
}
```

## Tab Text Customization

Customize the text displayed on each color group tab to match your application's terminology or localization requirements.

### Tab Name Properties

ColorUIControl provides four properties to customize tab text:

| Property | Default Text | Color Group |
|----------|-------------|-------------|
| `SystemTabName` | "System" | System colors |
| `StandardTabName` | "Standard" | Standard colors |
| `CustomTabName` | "Custom" | Custom colors |
| `UserTabName` | "User" | User colors |

### Setting Custom Tab Names

```csharp
// Customize tab names
this.colorUIControl1.StandardTabName = "Web Colors";
this.colorUIControl1.SystemTabName = "System Colors";
this.colorUIControl1.UserTabName = "My Colors";
this.colorUIControl1.CustomTabName = "Theme Palette";

// Reset individual tab names to default
this.colorUIControl1.ResetStandardTabName();
this.colorUIControl1.ResetSystemTabName();
```

### Tab Name Localization Example

```csharp
private void LocalizeTabNames(string language)
{
    switch (language)
    {
        case "es": // Spanish
            colorUIControl1.SystemTabName = "Sistema";
            colorUIControl1.StandardTabName = "Estándar";
            break;
        case "fr": // French
            colorUIControl1.SystemTabName = "Système";
            colorUIControl1.StandardTabName = "Standard";
            break;
        default: // English
            colorUIControl1.ResetSystemTabName();
            colorUIControl1.ResetStandardTabName();
            break;
    }
}
```

## Font Styling

Change the font style for tab text:

```csharp
// Set font for tab text
this.colorUIControl1.Font = new System.Drawing.Font("Segoe UI", 9F);

// Bold font for emphasis
this.colorUIControl1.Font = new System.Drawing.Font("Arial", 9F, FontStyle.Bold);

// Match application theme
colorUIControl1.Font = this.Font;
```

## Complete Customization Examples

### Example 1: Modern Flat Design

```csharp
private void ApplyModernDesign()
{
    colorUIControl1.BorderStyle = BorderStyle.FixedSingle;
    colorUIControl1.Font = new Font("Segoe UI", 9F);
    colorUIControl1.StandardTabName = "Colors";
    colorUIControl1.CustomColorsStretchOnResize = true;
    colorUIControl1.UserColorsStretchOnResize = true;
    colorUIControl1.BackColor = Color.White;
    colorUIControl1.ColorGroups = ColorUIGroups.StandardColors | 
                                  ColorUIGroups.CustomColors;
}
```

### Example 2: Classic Windows Design

```csharp
private void ApplyClassicDesign()
{
    colorUIControl1.BorderStyle = BorderStyle.Fixed3D;
    colorUIControl1.Font = new Font("Microsoft Sans Serif", 8.25F);
    colorUIControl1.ResetStandardTabName();
    colorUIControl1.CustomColorsStretchOnResize = false;
    colorUIControl1.BackColor = SystemColors.Control;
}
```

### Example 3: High Contrast Accessibility

```csharp
private void ApplyHighContrastDesign()
{
    colorUIControl1.BorderStyle = BorderStyle.FixedSingle;
    colorUIControl1.Font = new Font("Tahoma", 10F, FontStyle.Bold);
    colorUIControl1.CustomColorsStretchOnResize = true;
    colorUIControl1.BackColor = SystemColors.Window;
    colorUIControl1.Size = new Size(280, 240);
}
```

## Best Practices

1. **Match Application Theme**: Inherit form properties (`colorUIControl1.Font = this.Font`)
2. **Use Stretching**: Enable for dockable/resizable containers
3. **Border Styles**: Fixed3D (traditional), FixedSingle (modern), None (embedded)
4. **Localize Tab Names**: Load from resource files for international apps
5. **Accessibility**: Use larger fonts (10pt+), high contrast support
6. **Test Sizing**: Set MinimumSize and MaximumSize appropriately

## Troubleshooting

**Tab Names Not Changing**: Set property after InitializeComponent()

**Stretching Not Working**: Verify control is resizable (Anchor or Dock properly)

**Font Changes Not Visible**: Font affects tab text only, not control content

**Border Not Visible**: Check background color contrast

**Tab Name Reset Not Working**: Use Reset methods (`ResetStandardTabName()`), not empty strings

## Next Steps

- [Color Groups](color-groups.md) - Learn about configuring color groups
- [Events](events.md) - Handle color selection events
- [Popup Integration](popup-integration.md) - Use ColorUIControl in popup menus
