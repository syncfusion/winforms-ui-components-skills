# Themes and Visual Styles

This guide covers theme support and visual styling options for the ProgressBarAdv control.

## Theme Support

The `ThemesEnabled` property enables built-in theme support.

### Enabling Themes

```csharp
this.progressBarAdv1.ThemesEnabled = true;
```

**VB.NET:**
```vb
Me.progressBarAdv1.ThemesEnabled = True
```

When enabled, the progress bar automatically applies the current application theme.

## Progress Style Options

The `ProgressStyle` property determines the visual appearance of the progress bar.

### Available Styles

```csharp
// Office 2016 Colorful (vibrant blue theme)
this.progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016Colorful;

// Office 2016 White (light theme)
this.progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016White;

// Office 2016 Dark Gray (medium-dark theme)
this.progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016DarkGray;

// Office 2016 Black (dark theme)
this.progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016Black;

// System style (Windows default)
this.progressBarAdv1.ProgressStyle = ProgressBarStyles.System;

// Tube style (segmented appearance)
this.progressBarAdv1.ProgressStyle = ProgressBarStyles.Tube;

// Gradient style (smooth gradient effect)
this.progressBarAdv1.ProgressStyle = ProgressBarStyles.Gradient;
```

**VB.NET:**
```vb
Me.progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016Colorful
Me.progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016White
Me.progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016DarkGray
Me.progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016Black
Me.progressBarAdv1.ProgressStyle = ProgressBarStyles.System
Me.progressBarAdv1.ProgressStyle = ProgressBarStyles.Tube
Me.progressBarAdv1.ProgressStyle = ProgressBarStyles.Gradient
```

## Office 2016 Styles

### Office2016Colorful

Vibrant theme with blue accents, best for modern applications:

```csharp
progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016Colorful;
progressBarAdv1.Value = 65;
```

**Characteristics:**
- Blue progress color
- Light gray background
- Modern, professional appearance
- Best for business applications

### Office2016White

Clean, minimal light theme:

```csharp
progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016White;
progressBarAdv1.Value = 65;
```

**Characteristics:**
- White/light gray color scheme
- Subtle borders
- Minimalist design
- Best for content-focused applications

### Office2016DarkGray

Medium-dark theme, reduces eye strain:

```csharp
progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016DarkGray;
progressBarAdv1.Value = 65;
```

**Characteristics:**
- Dark gray background
- Light progress indicator
- Reduced brightness
- Best for extended use or low-light environments

### Office2016Black

High-contrast dark theme:

```csharp
progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016Black;
progressBarAdv1.Value = 65;
```

**Characteristics:**
- Black background
- High contrast
- Modern, sleek appearance
- Best for design-focused applications

## Alternative Styles

### System Style

Uses Windows standard progress bar appearance:

```csharp
progressBarAdv1.ProgressStyle = ProgressBarStyles.System;
progressBarAdv1.Value = 65;
```

**Best for:**
- Windows-native look and feel
- Consistency with OS appearance
- Classic applications

### Tube Style

Segmented tube appearance:

```csharp
progressBarAdv1.ProgressStyle = ProgressBarStyles.Tube;
progressBarAdv1.Value = 65;
```

**Best for:**
- Distinctive visual style
- Loading indicators
- Animated progress effects

### Gradient Style

Smooth gradient effect:

```csharp
progressBarAdv1.ProgressStyle = ProgressBarStyles.Gradient;
progressBarAdv1.Value = 65;
```

**Best for:**
- Smooth, polished appearance
- Modern UI designs
- Custom color schemes

## Style Selection Guidelines

| Style | Best For | Color Scheme | Use When |
|-------|----------|--------------|----------|
| Office2016Colorful | Business apps | Blue/Gray | Professional, modern look |
| Office2016White | Content apps | Light/White | Minimalist design needed |
| Office2016DarkGray | Long sessions | Medium Dark | Reducing eye strain |
| Office2016Black | Media apps | Black/High contrast | Sleek, modern appearance |
| System | Native feel | OS Default | Windows consistency required |
| Tube | Distinctive | Customizable | Unique visual style wanted |
| Gradient | Modern UI | Customizable | Smooth, polished look desired |

## Theme Application Examples

### Application-Wide Theme

```csharp
private void ApplyApplicationTheme(ProgressBarStyles style)
{
    foreach (Control control in this.Controls)
    {
        if (control is ProgressBarAdv progressBar)
        {
            progressBar.ProgressStyle = style;
            progressBar.ThemesEnabled = true;
        }
    }
}

// Usage
ApplyApplicationTheme(ProgressBarStyles.Office2016Colorful);
```

### Theme Switcher

```csharp
private void ThemeComboBox_SelectedIndexChanged(object sender, EventArgs e)
{
    ComboBox combo = sender as ComboBox;
    
    switch (combo.SelectedIndex)
    {
        case 0:
            progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016Colorful;
            break;
        case 1:
            progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016White;
            break;
        case 2:
            progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016DarkGray;
            break;
        case 3:
            progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016Black;
            break;
        case 4:
            progressBarAdv1.ProgressStyle = ProgressBarStyles.System;
            break;
    }
}
```

### System Theme Detection

```csharp
private void ApplySystemTheme()
{
    try
    {
        using (var key = Microsoft.Win32.Registry.CurrentUser.OpenSubKey(
            @"Software\Microsoft\Windows\CurrentVersion\Themes\Personalize"))
        {
            var value = key?.GetValue("AppsUseLightTheme");
            bool isLightTheme = value != null && (int)value == 1;
            
            progressBarAdv1.ProgressStyle = isLightTheme
                ? ProgressBarStyles.Office2016White
                : ProgressBarStyles.Office2016DarkGray;
        }
    }
    catch
    {
        // Fallback to default
        progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016Colorful;
    }
}
```

## Theme Persistence

### Save Theme Preference

```csharp
private void SaveThemePreference(ProgressBarStyles style)
{
    Properties.Settings.Default.ProgressBarTheme = style.ToString();
    Properties.Settings.Default.Save();
}
```

### Load Theme Preference

```csharp
private void LoadThemePreference()
{
    string savedTheme = Properties.Settings.Default.ProgressBarTheme;
    if (!string.IsNullOrEmpty(savedTheme))
    {
        if (Enum.TryParse<ProgressBarStyles>(savedTheme, out var style))
        {
            progressBarAdv1.ProgressStyle = style;
            progressBarAdv1.ThemesEnabled = true;
        }
    }
}
```

## Best Practices

### Consistent Theming

Apply the same theme across all progress bars:

```csharp
private void InitializeAllProgressBars(ProgressBarStyles style)
{
    foreach (var progressBar in GetAllProgressBars())
    {
        progressBar.ProgressStyle = style;
        progressBar.ThemesEnabled = true;
    }
}

private IEnumerable<ProgressBarAdv> GetAllProgressBars()
{
    return this.Controls.OfType<ProgressBarAdv>();
}
```

### Theme with Custom Colors

Combine themes with custom colors:

```csharp
progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016Colorful;
progressBarAdv1.ForeColor = Color.Green; // Custom progress color
progressBarAdv1.BackColor = Color.LightGray; // Custom background
```

### Accessibility

Ensure sufficient contrast:

```csharp
private bool HasSufficientContrast(Color foreground, Color background)
{
    // WCAG contrast ratio calculation
    double luminance1 = GetRelativeLuminance(foreground);
    double luminance2 = GetRelativeLuminance(background);
    
    double lighter = Math.Max(luminance1, luminance2);
    double darker = Math.Min(luminance1, luminance2);
    
    double contrastRatio = (lighter + 0.05) / (darker + 0.05);
    
    return contrastRatio >= 4.5; // WCAG AA standard
}
```

## Complete Example

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class ThemeDemo : Form
{
    private ProgressBarAdv progressBar;
    private ComboBox themeSelector;
    
    public ThemeDemo()
    {
        this.Text = "Progress Bar Theme Demo";
        this.Size = new Size(500, 200);
        
        InitializeControls();
        LoadThemePreference();
    }
    
    private void InitializeControls()
    {
        // Progress bar
        progressBar = new ProgressBarAdv();
        progressBar.Location = new Point(20, 60);
        progressBar.Size = new Size(400, 30);
        progressBar.Value = 60;
        progressBar.TextStyle = ProgressBarTextStyles.Percentage;
        progressBar.ThemesEnabled = true;
        this.Controls.Add(progressBar);
        
        // Theme selector
        Label label = new Label();
        label.Text = "Select Theme:";
        label.Location = new Point(20, 20);
        label.AutoSize = true;
        this.Controls.Add(label);
        
        themeSelector = new ComboBox();
        themeSelector.Location = new Point(120, 17);
        themeSelector.Width = 200;
        themeSelector.DropDownStyle = ComboBoxStyle.DropDownList;
        themeSelector.Items.AddRange(new object[]
        {
            "Office 2016 Colorful",
            "Office 2016 White",
            "Office 2016 Dark Gray",
            "Office 2016 Black",
            "System",
            "Tube",
            "Gradient"
        });
        themeSelector.SelectedIndex = 0;
        themeSelector.SelectedIndexChanged += ThemeSelector_Changed;
        this.Controls.Add(themeSelector);
    }
    
    private void ThemeSelector_Changed(object sender, EventArgs e)
    {
        ProgressBarStyles style = themeSelector.SelectedIndex switch
        {
            0 => ProgressBarStyles.Office2016Colorful,
            1 => ProgressBarStyles.Office2016White,
            2 => ProgressBarStyles.Office2016DarkGray,
            3 => ProgressBarStyles.Office2016Black,
            4 => ProgressBarStyles.System,
            5 => ProgressBarStyles.Tube,
            6 => ProgressBarStyles.Gradient,
            _ => ProgressBarStyles.Office2016Colorful
        };
        
        progressBar.ProgressStyle = style;
        SaveThemePreference(style);
    }
    
    private void SaveThemePreference(ProgressBarStyles style)
    {
        // Save to settings or registry
    }
    
    private void LoadThemePreference()
    {
        // Load from settings or registry
    }
}
```

## Next Steps

- **Appearance:** See [appearance-styling.md](appearance-styling.md) for custom colors
- **Events:** See [events-advanced.md](events-advanced.md) for advanced customization
