# Themes and Styles

## Table of Contents
- [Overview](#overview)
- [Style Property](#style-property)
- [Office Color Schemes](#office-color-schemes)
- [Custom Colors](#custom-colors)
- [IgnoreThemeBackground Property](#ignorethemebackground-property)
- [Theme vs Appearance Interaction](#theme-vs-appearance-interaction)
- [Complete Theming Examples](#complete-theming-examples)

## Overview

ComboDropDown supports modern visual themes through the `Style` property, which applies professional appearances including Office 2016, Metro, Office 2010, Office 2007, and more. These themes provide consistent, polished looks that integrate with your application's overall visual design.

**Key Properties:**
- **Style** - Main theme property (Office2016Colorful, Metro, Office2010, etc.)
- **Office2007ColorTheme** - Color scheme for Office2007 style (Blue, Silver, Black)
- **IgnoreThemeBackground** - Use BackColor instead of theme background

**Theme vs Appearance:**
- **Themes (this guide):** Complete visual styles with predefined colors and effects
- **Appearance (appearance-customization.md):** Custom borders, colors, and flat styles

Set `Style = Default` to use custom appearance settings, or choose a theme for automatic styling.

## Style Property

The `Style` property applies comprehensive visual themes to the ComboDropDown control.

**Type:** `Syncfusion.Windows.Forms.VisualStyle`

### Available Styles

| Style | Description | Visual Characteristics |
|-------|-------------|------------------------|
| `Office2016Colorful` | Modern Office 2016 colorful theme | Bright accent colors, flat design |
| `Office2016White` | Office 2016 white theme | Clean white background, subtle borders |
| `Office2016DarkGray` | Office 2016 dark gray theme | Dark gray tones, professional |
| `Office2016Black` | Office 2016 black theme | Dark black background, high contrast |
| `Metro` | Windows Metro/Modern UI | Flat, minimalist, bold colors |
| `Office2010` | Office 2010 style | Subtle gradients, modern but traditional |
| `Office2007` | Office 2007 style | Classic ribbon-era Office look |
| `OfficeXP` | Office XP style | Early 2000s Office appearance |
| `Office2003` | Office 2003 style | Classic task pane colors |
| `VS2005` | Visual Studio 2005 style | Developer tool appearance |
| `Default` | Standard Windows Forms | No theme, uses custom appearance settings |

### Basic Usage

```csharp
using Syncfusion.Windows.Forms;

// Office 2016 Colorful theme
this.comboDropDown1.Style = VisualStyle.Office2016Colorful;

// Metro theme
this.comboDropDown1.Style = VisualStyle.Metro;

// Office 2010 theme
this.comboDropDown1.Style = VisualStyle.Office2010;

// Default (no theme)
this.comboDropDown1.Style = VisualStyle.Default;
```

```vb
Imports Syncfusion.Windows.Forms

' Office 2016 Colorful theme
Me.comboDropDown1.Style = VisualStyle.Office2016Colorful

' Metro theme
Me.comboDropDown1.Style = VisualStyle.Metro

' Office 2010 theme
Me.comboDropDown1.Style = VisualStyle.Office2010

' Default (no theme)
Me.comboDropDown1.Style = VisualStyle.Default
```

## Office Color Schemes

When using `Office2007` style, you can specify a color scheme: Blue, Silver, or Black.

**Property:** `Office2007ColorTheme`  
**Type:** `Syncfusion.Windows.Forms.Office2007Theme`  
**Prerequisite:** `Style = VisualStyle.Office2007`

### Available Color Schemes

| Scheme | Description | When to Use |
|--------|-------------|-------------|
| `Blue` | Blue accents (default) | Standard Office 2007 blue theme |
| `Silver` | Silver/gray tones | Professional, neutral appearance |
| `Black` | Black with gray accents | High-contrast, dramatic look |

### Example

```csharp
using Syncfusion.Windows.Forms;

// Office 2007 Blue scheme
this.comboDropDown1.Style = VisualStyle.Office2007;
this.comboDropDown1.Office2007ColorTheme = Office2007Theme.Blue;

// Office 2007 Silver scheme
this.comboDropDown1.Style = VisualStyle.Office2007;
this.comboDropDown1.Office2007ColorTheme = Office2007Theme.Silver;

// Office 2007 Black scheme
this.comboDropDown1.Style = VisualStyle.Office2007;
this.comboDropDown1.Office2007ColorTheme = Office2007Theme.Black;
```

```vb
Imports Syncfusion.Windows.Forms

' Office 2007 Blue scheme
Me.comboDropDown1.Style = VisualStyle.Office2007
Me.comboDropDown1.Office2007ColorTheme = Office2007Theme.Blue

' Office 2007 Silver scheme
Me.comboDropDown1.Style = VisualStyle.Office2007
Me.comboDropDown1.Office2007ColorTheme = Office2007Theme.Silver

' Office 2007 Black scheme
Me.comboDropDown1.Style = VisualStyle.Office2007
Me.comboDropDown1.Office2007ColorTheme = Office2007Theme.Black
```

### Visual Comparison

**Blue:** Classic Office 2007 look with blue accents and gradients  
**Silver:** Neutral gray/silver tones, professional and subtle  
**Black:** Dark theme with black backgrounds and white/gray text

## Custom Colors

Apply custom colors to Office2007 theme using the `Managed` color scheme and `ApplyManagedColors` method.

### Using Managed Theme

```csharp
using Syncfusion.Windows.Forms;

// Set Office2007 style with Managed theme
this.comboDropDown1.Style = VisualStyle.Office2007;
this.comboDropDown1.Office2007ColorTheme = Office2007Theme.Managed;

// Apply custom color
Office2007Colors.ApplyManagedColors(this, Color.Orchid);
```

```vb
Imports Syncfusion.Windows.Forms

' Set Office2007 style with Managed theme
Me.comboDropDown1.Style = VisualStyle.Office2007
Me.comboDropDown1.Office2007ColorTheme = Office2007Theme.Managed

' Apply custom color
Office2007Colors.ApplyManagedColors(Me, Color.Orchid)
```

### How Managed Colors Work

1. Set `Office2007ColorTheme = Office2007Theme.Managed`
2. Call `Office2007Colors.ApplyManagedColors(form, baseColor)`
3. Base color generates a full color scheme (lighter/darker variations)
4. All Office2007-styled controls on the form use this color scheme

### Custom Color Examples

```csharp
// Purple/orchid theme
comboDropDown1.Office2007ColorTheme = Office2007Theme.Managed;
Office2007Colors.ApplyManagedColors(this, Color.Orchid);

// Teal theme
comboDropDown1.Office2007ColorTheme = Office2007Theme.Managed;
Office2007Colors.ApplyManagedColors(this, Color.Teal);

// Orange theme
comboDropDown1.Office2007ColorTheme = Office2007Theme.Managed;
Office2007Colors.ApplyManagedColors(this, Color.DarkOrange);
```

### Scope of ApplyManagedColors

**Important:** `ApplyManagedColors` affects **all controls on the form** that use Office2007 Managed theme.

```csharp
// Applies to entire form
Office2007Colors.ApplyManagedColors(this, Color.Green);
// All ComboDropDown, ButtonAdv, etc. with Managed theme use green scheme
```

To use different colors for different controls, place them on separate forms or containers.

## IgnoreThemeBackground Property

Controls whether the control uses the theme's background color or the custom `BackColor` property.

**Type:** `bool`  
**Default:** `false`

### When to Use

- Override theme background while keeping other theme elements
- Use custom BackColor with themed borders/buttons
- Match control background to specific form design

### Example

```csharp
// Use theme background (default)
this.comboDropDown1.IgnoreThemeBackground = false;

// Use custom BackColor instead of theme background
this.comboDropDown1.IgnoreThemeBackground = true;
this.comboDropDown1.BackColor = Color.LightYellow;
```

```vb
' Use theme background (default)
Me.comboDropDown1.IgnoreThemeBackground = False

' Use custom BackColor instead of theme background
Me.comboDropDown1.IgnoreThemeBackground = True
Me.comboDropDown1.BackColor = Color.LightYellow
```

### Use Case: Custom Background with Office Theme

```csharp
// Office2016 theme with custom background
this.comboDropDown1.Style = VisualStyle.Office2016Colorful;
this.comboDropDown1.IgnoreThemeBackground = true;
this.comboDropDown1.BackColor = Color.FromArgb(255, 250, 240); // Light cream
// Theme borders/button remain, but background is custom
```

## Theme vs Appearance Interaction

Understanding how themes interact with custom appearance settings is crucial for achieving desired results.

### When Style = Default

All custom appearance properties work:

```csharp
// Custom appearance (full control)
comboDropDown1.Style = VisualStyle.Default;
comboDropDown1.FlatStyle = FlatStyle.Flat;
comboDropDown1.FlatBorderColor = Color.Gray;
comboDropDown1.BackColor = Color.White;
comboDropDown1.Border3DStyle = Border3DStyle.Sunken; // Works
```

### When Style = Office2016/Metro/etc.

Theme overrides most custom settings:

```csharp
// Theme appearance (limited custom control)
comboDropDown1.Style = VisualStyle.Office2016Colorful;
comboDropDown1.FlatStyle = FlatStyle.Flat;        // Ignored
comboDropDown1.FlatBorderColor = Color.Gray;      // Ignored
comboDropDown1.Border3DStyle = Border3DStyle.Sunken; // Ignored

// These still work:
comboDropDown1.Font = new Font("Segoe UI", 10F);  // Works
comboDropDown1.IgnoreThemeBackground = true;      // Works
comboDropDown1.BackColor = Color.White;           // Works if IgnoreThemeBackground=true
```

### Property Override Matrix

| Property | Default Style | Themed Style | Notes |
|----------|---------------|--------------|-------|
| `FlatStyle` | ✓ Applied | ✗ Ignored | Theme controls flat/3D appearance |
| `FlatBorderColor` | ✓ Applied | ✗ Ignored | Theme controls border colors |
| `Border3DStyle` | ✓ Applied | ✗ Ignored | Theme controls border style |
| `BorderSides` | ✓ Applied | ✗ Ignored | Theme controls which sides have borders |
| `Font` | ✓ Applied | ✓ Applied | Font always customizable |
| `ForeColor` | ✓ Applied | Partial | Theme may override in some modes |
| `BackColor` | ✓ Applied | ✓ Applied | Works if IgnoreThemeBackground=true |

### Best Practice: Choose One Approach

**Approach 1: Full Custom Appearance**
```csharp
// Complete control over appearance
comboDropDown1.Style = VisualStyle.Default;
comboDropDown1.FlatStyle = FlatStyle.Flat;
comboDropDown1.FlatBorderColor = Color.DodgerBlue;
comboDropDown1.BackColor = Color.White;
```

**Approach 2: Theme with Minimal Customization**
```csharp
// Professional theme with font override
comboDropDown1.Style = VisualStyle.Office2016Colorful;
comboDropDown1.Font = new Font("Segoe UI", 10F);
// Don't try to customize borders or flat style
```

**Avoid Mixing:**
```csharp
// Anti-pattern: theme + extensive custom settings
comboDropDown1.Style = VisualStyle.Metro; // Theme active
comboDropDown1.FlatStyle = FlatStyle.Flat; // Won't work
comboDropDown1.Border3DStyle = Border3DStyle.Sunken; // Won't work
// Result: Confusing, some settings ignored
```

## Complete Theming Examples

### Example 1: Office 2016 Colorful (Modern)

```csharp
using Syncfusion.Windows.Forms;

// Modern Office 2016 appearance
this.comboDropDown1.Style = VisualStyle.Office2016Colorful;
this.comboDropDown1.Font = new Font("Segoe UI", 10F);

// Typical for modern business applications
```

### Example 2: Metro (Minimalist)

```csharp
// Windows Modern UI style
this.comboDropDown1.Style = VisualStyle.Metro;
this.comboDropDown1.Font = new Font("Segoe UI", 10F);

// Clean, flat design for contemporary apps
```

### Example 3: Office 2007 Blue (Classic)

```csharp
// Traditional Office appearance
this.comboDropDown1.Style = VisualStyle.Office2007;
this.comboDropDown1.Office2007ColorTheme = Office2007Theme.Blue;

// Familiar look for legacy application migration
```

### Example 4: Office 2007 Custom (Branded)

```csharp
// Custom brand color (e.g., company purple)
this.comboDropDown1.Style = VisualStyle.Office2007;
this.comboDropDown1.Office2007ColorTheme = Office2007Theme.Managed;
Office2007Colors.ApplyManagedColors(this, Color.FromArgb(128, 0, 128)); // Purple

// Matches corporate branding
```

### Example 5: Dark Theme (Office 2016 Black)

```csharp
// Dark mode appearance
this.comboDropDown1.Style = VisualStyle.Office2016Black;
this.comboDropDown1.Font = new Font("Segoe UI", 10F);
this.comboDropDown1.ForeColor = Color.White; // May be needed for text

// For dark-themed applications
```

### Example 6: Custom Background with Theme

```csharp
// Theme borders/button with custom background
this.comboDropDown1.Style = VisualStyle.Office2010;
this.comboDropDown1.IgnoreThemeBackground = true;
this.comboDropDown1.BackColor = Color.FromArgb(245, 245, 255); // Light blue tint
this.comboDropDown1.Font = new Font("Segoe UI", 9.75F);

// Integrates with custom form background
```

## Application-Wide Theming

For consistent theming across your entire application, apply styles to all controls:

```csharp
public class ThemedForm : Form
{
    public ThemedForm()
    {
        InitializeComponent();
        ApplyThemeToControls(this.Controls);
    }
    
    private void ApplyThemeToControls(Control.ControlCollection controls)
    {
        foreach (Control control in controls)
        {
            // Apply theme to ComboDropDown controls
            if (control is ComboDropDown combo)
            {
                combo.Style = VisualStyle.Office2016Colorful;
                combo.Font = new Font("Segoe UI", 10F);
            }
            
            // Recursively apply to child controls
            if (control.HasChildren)
            {
                ApplyThemeToControls(control.Controls);
            }
        }
    }
}
```

## Best Practices

1. **Choose appropriate theme for your application type**
   - Office2016: Modern business apps
   - Metro: Contemporary, minimalist UI
   - Office2007: Legacy app modernization
   - Default: Full custom control needed

2. **Apply themes consistently** - Use the same theme across all controls in a form or application

3. **Use Managed colors for branding** - Match your company's color scheme with Office2007 Managed theme

4. **Test theme readability** - Ensure text is readable against theme backgrounds, especially in dark themes

5. **IgnoreThemeBackground for special cases** - Override background while keeping themed borders/buttons

6. **Don't mix themes and custom appearance** - Choose theme OR custom styling, not both

7. **Consider user preferences** - Allow users to switch themes if appropriate for your application

8. **Test across Windows versions** - Theme rendering may vary slightly on different Windows versions
