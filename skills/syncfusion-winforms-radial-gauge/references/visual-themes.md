# Visual Themes and Styling

## Table of Contents
- [Overview](#overview)
- [VisualStyle Property](#visualstyle-property)
- [Built-in Themes](#built-in-themes)
- [Custom Themes](#custom-themes)
- [Theme Application](#theme-application)
- [Gauge-Specific Theming](#gauge-specific-theming)
- [Best Practices](#best-practices)

## Overview

All three gauge types (RadialGauge, LinearGauge, DigitalGauge) support **professional visual themes** through the `VisualStyle` property. Themes provide consistent, polished appearance with minimal configuration.

**Available themes:**
- 9 built-in professional themes
- Custom theme support
- Consistent styling across all gauge types
- Design-time and runtime application

## VisualStyle Property

Central property for applying themes to gauges.

### Setting Visual Style

```csharp
// Apply theme to RadialGauge
radialGauge.VisualStyle = ThemeStyle.Office2016Colorful;

// Apply theme to LinearGauge
linearGauge.VisualStyle = ThemeStyle.Metro;

// Apply theme to DigitalGauge
digitalGauge.VisualStyle = ThemeStyle.Office2016Black;
```

### Available ThemeStyle Values

```csharp
public enum ThemeStyle
{
    Default,              // Classic Windows Forms style
    Blue,                 // Blue color scheme
    Black,                // Black/dark color scheme
    Silver,               // Silver/gray color scheme
    Metro,                // Modern flat Metro style
    Office2016Colorful,   // Office 2016 colorful theme
    Office2016White,      // Office 2016 white theme
    Office2016DarkGray,   // Office 2016 dark gray theme
    Office2016Black,      // Office 2016 black theme
    Custom                // Custom theme (user-defined)
}
```

## Built-in Themes

### Default Theme

Classic Windows Forms appearance.

```csharp
gauge.VisualStyle = ThemeStyle.Default;
```

**Characteristics:**
- Standard Windows Forms colors
- Traditional 3D effects
- Neutral gray tones
- Good compatibility with all UI styles

**Use when:**
- Legacy application consistency
- No specific theme requirement
- Maximum compatibility

### Blue Theme

Professional blue color scheme.

```csharp
gauge.VisualStyle = ThemeStyle.Blue;
```

**Characteristics:**
- Blue gradients and accents
- Light blue backgrounds
- Dark blue borders
- Professional business appearance

**Use when:**
- Corporate/business applications
- Blue brand colors
- Professional dashboards

### Black Theme

Dark color scheme with high contrast.

```csharp
gauge.VisualStyle = ThemeStyle.Black;
```

**Characteristics:**
- Black/dark gray backgrounds
- White or light text
- High contrast elements
- Modern dark UI

**Use when:**
- Dark mode applications
- Night-time usage
- Reduced eye strain
- Modern gaming/entertainment UI

### Silver Theme

Neutral gray color scheme.

```csharp
gauge.VisualStyle = ThemeStyle.Silver;
```

**Characteristics:**
- Silver/gray gradients
- Neutral appearance
- Soft shadows
- Versatile styling

**Use when:**
- Neutral professional look
- Gray brand colors
- Industrial applications

### Metro Theme

Modern flat design style.

```csharp
gauge.VisualStyle = ThemeStyle.Metro;
```

**Characteristics:**
- Flat design (no gradients)
- Bold solid colors
- Minimal shadows
- Clean modern look

**Use when:**
- Modern Windows applications
- Flat UI design
- Touch-optimized interfaces
- Contemporary styling

### Office2016Colorful Theme

Vibrant Office 2016 style.

```csharp
gauge.VisualStyle = ThemeStyle.Office2016Colorful;
```

**Characteristics:**
- Bright accent colors
- White backgrounds
- Colorful highlights
- Modern Office appearance

**Use when:**
- Office-style applications
- Bright, friendly UI
- Modern productivity apps

### Office2016White Theme

Clean white Office 2016 style.

```csharp
gauge.VisualStyle = ThemeStyle.Office2016White;
```

**Characteristics:**
- Predominantly white
- Subtle gray accents
- Minimalist design
- Clean professional look

**Use when:**
- Minimalist design
- White/light applications
- Document-centric apps

### Office2016DarkGray Theme

Dark gray Office 2016 style.

```csharp
gauge.VisualStyle = ThemeStyle.Office2016DarkGray;
```

**Characteristics:**
- Dark gray backgrounds
- Muted accent colors
- Professional dark theme
- Reduced brightness

**Use when:**
- Dark mode Office-style apps
- Reduced eye strain
- Professional dark UI

### Office2016Black Theme

Black Office 2016 style.

```csharp
gauge.VisualStyle = ThemeStyle.Office2016Black;
```

**Characteristics:**
- True black backgrounds
- High contrast
- Modern dark design
- Maximum eye strain reduction

**Use when:**
- True dark mode
- OLED displays
- Night-time usage
- Professional dark applications

## Custom Themes

Create custom themes using ThemeBrush collections.

### RadialGauge Custom Theme

```csharp
using Syncfusion.Windows.Forms.Gauge;

// Set to Custom theme
radialGauge.VisualStyle = ThemeStyle.Custom;

// Create custom theme brush
RadialGaugeThemeBrush customBrush = new RadialGaugeThemeBrush();

// Frame colors
customBrush.OuterFrameGradientStartColor = Color.DarkSlateBlue;
customBrush.OuterFrameGradientEndColor = Color.MidnightBlue;
customBrush.InnerFrameGradientStartColor = Color.LightSteelBlue;
customBrush.InnerFrameGradientEndColor = Color.SteelBlue;

// Arc colors
customBrush.GaugeArcColor = Color.LightGray;
customBrush.ValueIndicatorColor = Color.DodgerBlue;

// Needle colors
customBrush.NeedleColor = Color.Crimson;
customBrush.NeedlePivotColor = Color.DarkRed;

// Scale and tick colors
customBrush.ScaleLabelColor = Color.White;
customBrush.MajorTickMarkColor = Color.White;
customBrush.MinorTickMarkColor = Color.LightGray;
customBrush.InterLinesColor = Color.Gray;

// Apply custom brush
radialGauge.ThemeBrush = customBrush;
```

### LinearGauge Custom Theme

```csharp
linearGauge.VisualStyle = ThemeStyle.Custom;

LinearGaugeThemeBrush customBrush = new LinearGaugeThemeBrush();

// Frame colors
customBrush.OuterFrameGradientStartColor = Color.Navy;
customBrush.OuterFrameGradientEndColor = Color.DarkBlue;
customBrush.InnerFrameGradientStartColor = Color.LightBlue;
customBrush.InnerFrameGradientEndColor = Color.SkyBlue;

// Gauge colors
customBrush.GaugeBaseColor = Color.LightGray;
customBrush.ValueIndicatorColor = Color.LimeGreen;

// Needle colors
customBrush.NeedleColor = Color.Red;

// Scale colors
customBrush.ScaleLabelColor = Color.White;
customBrush.MajorTickMarkColor = Color.White;
customBrush.MinorTickMarkColor = Color.LightGray;

linearGauge.ThemeBrush = customBrush;
```

### DigitalGauge Custom Styling

DigitalGauge uses direct color properties instead of ThemeBrush:

```csharp
// Custom colors for DigitalGauge
digitalGauge.ForeColor = Color.Cyan;           // Active segments
digitalGauge.BackColor = Color.Black;          // Background
digitalGauge.ShowInvisibleSegments = true;     // Ghost segments

// Frame styling
digitalGauge.RoundCornerRadius = 15;

// Wrap in custom frame
Panel frame = new Panel();
frame.BackColor = Color.DarkSlateGray;
frame.Padding = new Padding(10);
frame.BorderStyle = BorderStyle.FixedSingle;
digitalGauge.Dock = DockStyle.Fill;
frame.Controls.Add(digitalGauge);
```

### Complete Custom Theme Example

```csharp
private void ApplyCustomTheme()
{
    // RadialGauge custom theme
    RadialGauge tempGauge = new RadialGauge();
    tempGauge.VisualStyle = ThemeStyle.Custom;
    
    RadialGaugeThemeBrush tempTheme = new RadialGaugeThemeBrush();
    tempTheme.OuterFrameGradientStartColor = Color.DarkOrange;
    tempTheme.OuterFrameGradientEndColor = Color.OrangeRed;
    tempTheme.InnerFrameGradientStartColor = Color.LightYellow;
    tempTheme.InnerFrameGradientEndColor = Color.Yellow;
    tempTheme.NeedleColor = Color.Red;
    tempTheme.ScaleLabelColor = Color.Black;
    tempTheme.MajorTickMarkColor = Color.DarkRed;
    
    tempGauge.ThemeBrush = tempTheme;
    tempGauge.MinimumValue = -20;
    tempGauge.MaximumValue = 120;
    tempGauge.Value = 72;
    
    this.Controls.Add(tempGauge);
}
```

## Theme Application

### Design-Time Theme Application

In Visual Studio Designer:

1. Select gauge control
2. Open Properties window
3. Find `VisualStyle` property
4. Select theme from dropdown
5. Preview updates immediately

### Runtime Theme Application

```csharp
// Apply theme at runtime
private void ApplyTheme(ThemeStyle theme)
{
    radialGauge.VisualStyle = theme;
    linearGauge.VisualStyle = theme;
    // DigitalGauge doesn't use VisualStyle for themes
}

// Example: ComboBox for theme selection
private void themeComboBox_SelectedIndexChanged(object sender, EventArgs e)
{
    switch (themeComboBox.SelectedItem.ToString())
    {
        case "Blue":
            ApplyTheme(ThemeStyle.Blue);
            break;
        case "Black":
            ApplyTheme(ThemeStyle.Black);
            break;
        case "Metro":
            ApplyTheme(ThemeStyle.Metro);
            break;
        case "Office 2016 Colorful":
            ApplyTheme(ThemeStyle.Office2016Colorful);
            break;
    }
}
```

### Application-Wide Theming

Apply consistent theme to all gauges:

```csharp
private void ApplyApplicationTheme(ThemeStyle theme)
{
    // Find all gauge controls recursively
    ApplyThemeToControls(this.Controls, theme);
}

private void ApplyThemeToControls(Control.ControlCollection controls, ThemeStyle theme)
{
    foreach (Control control in controls)
    {
        if (control is RadialGauge radial)
            radial.VisualStyle = theme;
        else if (control is LinearGauge linear)
            linear.VisualStyle = theme;
        
        // Recurse into child controls
        if (control.HasChildren)
            ApplyThemeToControls(control.Controls, theme);
    }
}

// Usage
ApplyApplicationTheme(ThemeStyle.Office2016DarkGray);
```

### Theme Persistence

Save and load user theme preference:

```csharp
using System.Configuration;

// Save theme preference
private void SaveThemePreference(ThemeStyle theme)
{
    Properties.Settings.Default.GaugeTheme = theme.ToString();
    Properties.Settings.Default.Save();
}

// Load theme preference
private void LoadThemePreference()
{
    string themeName = Properties.Settings.Default.GaugeTheme;
    if (!string.IsNullOrEmpty(themeName))
    {
        ThemeStyle theme = (ThemeStyle)Enum.Parse(typeof(ThemeStyle), themeName);
        ApplyApplicationTheme(theme);
    }
}

// Call in Form_Load
private void MainForm_Load(object sender, EventArgs e)
{
    LoadThemePreference();
}
```

## Gauge-Specific Theming

### RadialGauge Theme Customization

```csharp
RadialGauge gauge = new RadialGauge();
gauge.VisualStyle = ThemeStyle.Blue;  // Base theme

// Override specific colors while keeping theme
gauge.NeedleColor = Color.Red;        // Custom needle
gauge.GaugeLabel = "Custom";
gauge.GaugeLabelColor = Color.Navy;   // Custom label color
```

### LinearGauge Theme Customization

```csharp
LinearGauge gauge = new LinearGauge();
gauge.VisualStyle = ThemeStyle.Metro;  // Base theme

// Override value indicator color
gauge.ValueIndicatorColor = Color.LimeGreen;

// Custom needle color
gauge.NeedleColor = Color.Red;
```

### DigitalGauge Custom Styling

DigitalGauge doesn't support VisualStyle property directly. Style using color properties:

```csharp
// Classic red LED
DigitalGauge ledRed = new DigitalGauge();
ledRed.ForeColor = Color.Red;
ledRed.BackColor = Color.Black;
ledRed.ShowInvisibleSegments = true;

// Modern cyan LCD
DigitalGauge lcdCyan = new DigitalGauge();
lcdCyan.ForeColor = Color.Cyan;
lcdCyan.BackColor = Color.Navy;
lcdCyan.ShowInvisibleSegments = false;

// Green terminal
DigitalGauge terminal = new DigitalGauge();
terminal.ForeColor = Color.LimeGreen;
terminal.BackColor = Color.Black;
terminal.ShowInvisibleSegments = false;
```

### Mixed Theme Dashboard

```csharp
// Dashboard with themed gauges
FlowLayoutPanel dashboard = new FlowLayoutPanel();
dashboard.Dock = DockStyle.Fill;
dashboard.BackColor = Color.FromArgb(45, 45, 48);  // Dark background

// Speed gauge - Blue theme
RadialGauge speed = new RadialGauge();
speed.VisualStyle = ThemeStyle.Blue;
speed.MinimumValue = 0;
speed.MaximumValue = 200;
speed.Value = 85;
speed.GaugeLabel = "Speed";
dashboard.Controls.Add(speed);

// Progress gauge - Metro theme
LinearGauge progress = new LinearGauge();
progress.VisualStyle = ThemeStyle.Metro;
progress.LinearFrameType = LinearFrameType.Horizontal;
progress.MinimumValue = 0;
progress.MaximumValue = 100;
progress.Value = 65;
dashboard.Controls.Add(progress);

// Status display - Custom styling
DigitalGauge status = new DigitalGauge();
status.CharacterType = CharacterType.SevenSegment;
status.ForeColor = Color.Lime;
status.BackColor = Color.Black;
status.Value = "READY";
dashboard.Controls.Add(status);

this.Controls.Add(dashboard);
```

## Theme Interaction with Properties

### Properties That Override Themes

Certain properties override theme colors when set explicitly:

```csharp
gauge.VisualStyle = ThemeStyle.Blue;  // Apply blue theme

// These explicit settings override theme values
gauge.NeedleColor = Color.Red;             // Overrides theme needle color
gauge.ScaleLabelColor = Color.Black;       // Overrides theme label color
gauge.MajorTickMarkColor = Color.White;    // Overrides theme tick color
```

### Resetting to Theme Defaults

To revert to theme colors, reset properties:

```csharp
// Re-apply theme to reset all colors
gauge.VisualStyle = ThemeStyle.Blue;
```

Or set to default/empty values (implementation-specific).

## Best Practices

### 1. Consistent Theme Application

Apply same theme across all gauges in an application:

```csharp
// Good: Consistent theming
radialGauge1.VisualStyle = ThemeStyle.Office2016DarkGray;
radialGauge2.VisualStyle = ThemeStyle.Office2016DarkGray;
linearGauge1.VisualStyle = ThemeStyle.Office2016DarkGray;

// Bad: Mixed themes without purpose
radialGauge1.VisualStyle = ThemeStyle.Blue;
radialGauge2.VisualStyle = ThemeStyle.Metro;  // Inconsistent
```

### 2. Match Application Theme

Choose gauge theme that complements your application:

```csharp
// Dark application
this.BackColor = Color.FromArgb(45, 45, 48);
gauge.VisualStyle = ThemeStyle.Office2016Black;  // Matches

// Light application
this.BackColor = Color.White;
gauge.VisualStyle = ThemeStyle.Office2016White;  // Matches
```

### 3. Test Readability

Ensure theme provides sufficient contrast:

```csharp
// Good contrast
gauge.VisualStyle = ThemeStyle.Black;
// White text on black background - readable

// Poor contrast example to avoid
// Light gray text on white background - hard to read
```

### 4. Provide Theme Options

Let users choose their preferred theme:

```csharp
// Add theme selector to settings
ComboBox themeSelector = new ComboBox();
themeSelector.Items.AddRange(new[] { 
    "Blue", "Black", "Silver", "Metro", 
    "Office 2016 Colorful", "Office 2016 Black" 
});
themeSelector.SelectedIndexChanged += ApplySelectedTheme;
```

### 5. Use Custom Themes Sparingly

Reserve custom themes for brand-specific requirements:

```csharp
// Use built-in themes when possible
gauge.VisualStyle = ThemeStyle.Metro;  // Preferred

// Custom themes for specific branding
if (requiresBrandColors)
{
    gauge.VisualStyle = ThemeStyle.Custom;
    ApplyBrandTheme(gauge);
}
```

### 6. Consider Accessibility

Choose themes with good accessibility:

```csharp
// High contrast for accessibility
gauge.VisualStyle = ThemeStyle.Black;  // High contrast

// Or custom high-contrast theme
if (accessibilityMode)
{
    ApplyHighContrastTheme(gauge);
}
```

### 7. Theme Transitions

Animate or smoothly transition theme changes:

```csharp
// Smooth theme change
private void ChangeTheme(ThemeStyle newTheme)
{
    // Suspend layout
    this.SuspendLayout();
    
    // Apply to all gauges
    ApplyApplicationTheme(newTheme);
    
    // Resume layout
    this.ResumeLayout();
    this.Refresh();
}
```

### 8. Document Custom Themes

When creating custom themes, document color choices:

```csharp
/// <summary>
/// Applies corporate brand theme to gauge.
/// Colors: Primary Blue (#003366), Accent Orange (#FF6600)
/// </summary>
private void ApplyCorporateTheme(RadialGauge gauge)
{
    gauge.VisualStyle = ThemeStyle.Custom;
    RadialGaugeThemeBrush theme = new RadialGaugeThemeBrush();
    theme.OuterFrameGradientStartColor = ColorTranslator.FromHtml("#003366");
    theme.OuterFrameGradientEndColor = ColorTranslator.FromHtml("#002244");
    theme.NeedleColor = ColorTranslator.FromHtml("#FF6600");
    gauge.ThemeBrush = theme;
}
```

## Theme Comparison Table

| Theme | Background | Accent Colors | Use Case |
|-------|-----------|---------------|----------|
| Default | Gray | Mixed | Legacy apps, compatibility |
| Blue | Light blue | Blue tones | Business/corporate |
| Black | Black | White/light | Dark mode, modern UI |
| Silver | Gray | Neutral | Industrial, neutral |
| Metro | Flat colors | Bold colors | Modern Windows apps |
| Office2016Colorful | White | Vibrant | Friendly productivity apps |
| Office2016White | White | Subtle gray | Minimalist, document apps |
| Office2016DarkGray | Dark gray | Muted | Professional dark mode |
| Office2016Black | Black | High contrast | True dark mode, OLED |
| Custom | User-defined | User-defined | Brand-specific requirements |

## Troubleshooting

### Issue: Theme not applied

**Cause:** VisualStyle set after other properties

**Solution:**
```csharp
// Set VisualStyle first
gauge.VisualStyle = ThemeStyle.Metro;

// Then configure other properties
gauge.MinimumValue = 0;
gauge.MaximumValue = 100;
```

### Issue: Custom colors not showing

**Cause:** Theme overriding custom colors

**Solution:**
```csharp
// Set custom colors AFTER theme
gauge.VisualStyle = ThemeStyle.Blue;
gauge.NeedleColor = Color.Red;  // Overrides theme needle color
```

### Issue: DigitalGauge doesn't respond to VisualStyle

**Expected behavior:** DigitalGauge doesn't use VisualStyle

**Solution:**
```csharp
// Use direct color properties for DigitalGauge
digitalGauge.ForeColor = Color.Red;
digitalGauge.BackColor = Color.Black;
```

### Issue: Theme looks different on different systems

**Cause:** System color settings or DPI scaling

**Solution:**
- Test on multiple systems
- Use Custom theme for precise control
- Set explicit colors for critical elements
