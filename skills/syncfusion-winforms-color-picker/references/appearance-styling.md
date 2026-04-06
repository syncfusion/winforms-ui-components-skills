# Appearance and Styling

## Table of Contents
- [Overview](#overview)
- [Visual Styles](#visual-styles)
- [Office2007 Styling](#office2007-styling)
- [Office2007 Color Schemes](#office2007-color-schemes)
- [Custom Colors with Managed Theme](#custom-colors-with-managed-theme)
- [Border Styles](#border-styles)
- [Complete Styling Examples](#complete-styling-examples)

## Overview

ColorPickerUIAdv supports multiple visual styles to match your application's theme. Styles range from classic Windows Forms appearance to modern Office 2016 designs.

**Available Styling Options:**
- Multiple built-in visual styles
- Office 2007/2010/2016 themes
- Custom color schemes
- Border customization
- Theme persistence across sessions

## Visual Styles

The `Style` property controls the overall appearance of the control.

### Available Styles

| Style | Description |
|-------|-------------|
| **Default** | Standard Windows Forms appearance |
| **Office2007** | Microsoft Office 2007 look and feel |
| **Office2010** | Microsoft Office 2010 styling |
| **Metro** | Modern flat design (Windows 8/10 style) |
| **Office2016Colorful** | Office 2016 with colorful accents |
| **Office2016White** | Office 2016 light theme |
| **Office2016Black** | Office 2016 dark theme |
| **Office2016DarkGray** | Office 2016 dark gray theme |

### Setting Visual Style

```csharp
using Syncfusion.Windows.Forms.Tools;

// Office2016 Colorful
colorPickerUIAdv1.Style = ColorPickerUIAdv.visualstyle.Office2016Colorful;

// Office2016 White
colorPickerUIAdv1.Style = ColorPickerUIAdv.visualstyle.Office2016White;

// Office2016 Black
colorPickerUIAdv1.Style = ColorPickerUIAdv.visualstyle.Office2016Black;

// Office2016 DarkGray
colorPickerUIAdv1.Style = ColorPickerUIAdv.visualstyle.Office2016DarkGray;

// Office2010
colorPickerUIAdv1.Style = ColorPickerUIAdv.visualstyle.Office2010;

// Metro
colorPickerUIAdv1.Style = ColorPickerUIAdv.visualstyle.Metro;

// Office2007
colorPickerUIAdv1.Style = ColorPickerUIAdv.visualstyle.Office2007;

// Default
colorPickerUIAdv1.Style = ColorPickerUIAdv.visualstyle.Default;
```

### Style Comparison

**Office2016 Styles:**
- Modern, clean appearance
- Four color variants (Colorful, White, Black, DarkGray)
- Best for contemporary applications

**Office2007/2010:**
- Classic Microsoft Office appearance
- Gradient effects and rounded corners
- Familiar to users of older Office versions

**Metro:**
- Flat, minimalist design
- Bold colors, no gradients
- Suitable for modern Windows apps

**Default:**
- Standard Windows Forms look
- Minimal styling
- Lightweight, system-themed

## Office2007 Styling

Office2007 style is enabled by default and offers additional customization through color schemes.

### UseOffice2007Style Property

Controls whether Office2007 styling is applied:

```csharp
// Enable Office2007 style (default: true)
colorPickerUIAdv1.UseOffice2007Style = true;

// Disable Office2007 style
colorPickerUIAdv1.UseOffice2007Style = false;
```

When `UseOffice2007Style = false`:
- Control reverts to basic Windows Forms appearance
- `Office2007Theme` property has no effect
- Simpler, flat rendering

### Office2007Theme Property

Works in conjunction with `UseOffice2007Style = true`:

```csharp
colorPickerUIAdv1.UseOffice2007Style = true;
colorPickerUIAdv1.Office2007Theme = Office2007Theme.Blue;   // Office 2007 Blue
colorPickerUIAdv1.Office2007Theme = Office2007Theme.Silver; // Office 2007 Silver
colorPickerUIAdv1.Office2007Theme = Office2007Theme.Black;  // Office 2007 Black
colorPickerUIAdv1.Office2007Theme = Office2007Theme.Managed; // Custom colors
```

## Office2007 Color Schemes

Pre-defined color schemes: Blue (default, blue accents), Silver (gray/neutral tones), and Black (dark/high contrast).

### Theme Comparison Example

```csharp
// Create three ColorPickerUIAdv controls with different themes
private void SetupThemeComparison()
{
    // Blue theme
    colorPicker1.UseOffice2007Style = true;
    colorPicker1.Office2007Theme = Office2007Theme.Blue;
    colorPicker1.Location = new Point(20, 20);
    
    // Silver theme
    colorPicker2.UseOffice2007Style = true;
    colorPicker2.Office2007Theme = Office2007Theme.Silver;
    colorPicker2.Location = new Point(240, 20);
    
    // Black theme
    colorPicker3.UseOffice2007Style = true;
    colorPicker3.Office2007Theme = Office2007Theme.Black;
    colorPicker3.Location = new Point(460, 20);
}
```

## Custom Colors with Managed Theme

The `Managed` theme allows applying custom accent colors to the Office2007 style.

### ApplyManagedColors Method

```csharp
// Set theme to Managed
colorPickerUIAdv1.Office2007Theme = Office2007Theme.Managed;

// Apply custom color scheme (affects entire form)
Office2007Colors.ApplyManagedColors(this, Color.Orange);
```

**Method Signature:**
```csharp
public static void ApplyManagedColors(Control control, Color baseColor)
```

**Parameters:**
- `control` - Parent control (typically `this` for form)
- `baseColor` - Base color for the theme

### Applying to All Controls

`ApplyManagedColors` affects all Syncfusion controls on the form:

```csharp
private void ApplyCustomThemeToForm()
{
    // Set all Syncfusion controls to Managed theme
    colorPickerUIAdv1.Office2007Theme = Office2007Theme.Managed;
    // ... set other controls to Managed theme
    
    // Apply custom color once (affects all)
    Office2007Colors.ApplyManagedColors(this, Color.DarkSlateBlue);
}
```

### Brand Color Integration

```csharp
// Define brand colors
private static class BrandColors
{
    public static Color Primary = Color.FromArgb(0, 120, 215);
    public static Color Secondary = Color.FromArgb(16, 110, 190);
    public static Color Accent = Color.FromArgb(255, 185, 0);
}

// Apply brand color theme
private void ApplyBrandTheme()
{
    colorPickerUIAdv1.Office2007Theme = Office2007Theme.Managed;
    Office2007Colors.ApplyManagedColors(this, BrandColors.Primary);
}
```

## Border Styles

Customize the control's border appearance.

### BorderStyle Property

```csharp
// No border (default)
colorPickerUIAdv1.BorderStyle = BorderStyle.None;

// Single-line border
colorPickerUIAdv1.BorderStyle = BorderStyle.FixedSingle;

// 3D border
colorPickerUIAdv1.BorderStyle = BorderStyle.Fixed3D;
```

### BorderStyle Options

| Style | Description | Use Case |
|-------|-------------|----------|
| **None** | No visible border | Inline integration, seamless layouts |
| **FixedSingle** | Thin single-line border | Clear boundaries, simple design |
| **Fixed3D** | Raised 3D effect | Classic Windows Forms, depth effect |

### BorderOffset Property

Controls the height of the border area (default: 3 pixels).

```csharp
// Default offset
colorPickerUIAdv1.BorderOffset = 3;

// Larger border area
colorPickerUIAdv1.BorderOffset = 5;

// No border offset
colorPickerUIAdv1.BorderOffset = 0;
```
 Larger values increase spacing around the control and affect overall control height. Visible only when `BorderStyle` is not `None`.

```csharp
colorPickerUIAdv1.BorderOffset = 3;  // Default
colorPickerUIAdv1.BorderOffset = 5;  // Larger border areaze(220, 200);
}
```

### Example 2: Classic Office2007 Blue

```csharp
private void ApplyClassicStyle()
{
    colorPickerUIAdv1.UseOffice2007Style = true;
    colorPickerUIAdv1.Office2007Theme = Office2007Theme.Blue;
    colorPickerUIAdv1.BorderStyle = BorderStyle.Fixed3D;
    colorPickerUIAdv1.BorderOffset = 4;
}
```

### Example 3: Dark Theme (Office2016 Black)

```csharp
private void ApplyDarkTheme()
{
    // Dark theme styling
    colorPickerUIAdv1.Style = ColorPickerUIAdv.visualstyle.Office2016Black;
    colorPickerUIAdv1.BorderStyle = BorderStyle.None;
    
    // Dark form background
    this.BackColor = Color.FromArgb(45, 45, 48);
}
```

### Example 4: Brand-Themed Color Picker

```csharp
private void ApplyBrandThemedStyle()
{
    // Use Office2007 with custom brand color
    colorPickerUIAdv1.UseOffice2007Style = true;
    colorPickerUIAdv1.Office2007Theme = Office2007Theme.Managed;
    
    // Apply brand color (e.g., company primary color)
    Color brandColor = Color.FromArgb(0, 120, 215); // Microsoft blue
    Office2007Colors.ApplyManagedColors(this, brandColor);
    
    // Border styling
    colorPickerUIAdv1.BorderStyle = BorderStyle.FixedSingle;
    colorPickerUIAdv1.BorderOffset = 3;
}
```

### Example 5: Minimal Metro Style

```csharp
private void ApplyMetroStyle()
{
    colorPickerUIAdv1.Style = ColorPickerUIAdv.visualstyle.Metro;
    colorPickerUIAdv1.BorderStyle = BorderStyle.None;
    colorPickerUIAdv1.BackColor = Color.White;
}emeSelector_SelectedIndexChanged(object sender, EventArgs e)
{
    switch (themeSelector.SelectedIndex)
    {
        case 0:
            colorPickerUIAdv1.Style = ColorPickerUIAdv.visualstyle.Office2016Colorful;
            break;
        case 1:
            colorPickerUIAdv1.Style = ColorPickerUIAdv.visualstyle.Office2016White;
            break;
        case 2:
            colorPickerUIAdv1.Style = ColorPickerUIAdv.visualstyle.Office2016Black;
            break;
        case 3:
            colorPickerUIAdv1.Style = ColorPickerUIAdv.visualstyle.Office2016DarkGray;
            break;
        case 4:
            colorPickerUIAdv1.Style = ColorPickerUIAdv.visualstyle.Office2010;
            break;
        case 5:
            colorPickerUIAdv1.UseOffice2007Style = true;
            colorPickerUIAdv1.Office2007Theme = Office2007Theme.Blue;
            break;
        case 6:
            colorPickerUIAdv1.UseOffice2007Style = true;
            colorPickerUIAdv1.Office2007Theme = Office2007Theme.Silver;
            break;
        case 7:
            colorPickerUIAdv1.UseOffice2007Style = true;
            colorPickerUIAdv1.Office2007Theme = Office2007Theme.Black;
            break;
        case 8:
            colorPickerUIAdv1.Style = ColorPickerUIAdv.visualstyle.Metro;
            break;
        case 9:
            colorPickerUIAdv1.Style = ColorPickerUIAdv.visualstyle.Default;
            break;
    }
}
```

## Best Practices

### Style Selection Guidelines

1. **Office2016 Styles:** Best for modern applications (2015+)
2. **Office2007/2010:** Compatible with older Office-style applications
3. **Metro:** Ideal for Windows 8/10/11 modern apps
4. **Default:** Lightweight, minimal dependencies

### Theme Consistency

Apply the same theme across all Syncfusion controls in your application:

```csharp
private void ApplyConsistentTheme()
{
    // Define theme once
    var theme = ColorPickerUIAdv.visualstyle.Office2016Colorful;
    
    // Apply to all controls
    colorPickerUIAdv1.Style = theme;
    // Apply to other Syncfusion controls similarly
}
```

### Performance Considerations

- **Office2016/Metro:** Slightly heavier than Default
- **Custom Colors:** Minimal performance impact
- **Border Styles:** Negligible performance difference

### Accessibility
- **Style Selection:** Use Office2016 for modern apps, Office2007/2010 for older Office-style apps, Metro for Windows 8/10/11, Default for lightweight applications
- **Theme Consistency:** Apply the same theme across all Syncfusion controls
- **Accessibility:** Test with high-contrast themes; use Office2016Black/DarkGray for dark themes, Office2016White for light themes- **Theme not applying:** Ensure `UseOffice2007Style = true` for Office2007Theme, or use `Style` property for other themes
- **Custom colors not showing:** Verify `Office2007Theme = Managed` is set before calling `ApplyManagedColors`
- **Border not visible:** Check `BorderStyle` is not `None` and `BorderOffset > 0`