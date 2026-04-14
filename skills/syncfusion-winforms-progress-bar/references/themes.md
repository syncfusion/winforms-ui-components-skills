# Themes and Visual Styles

This reference covers theme support and visual styling options for ProgressBarAdv.

## When to Read This

Read this reference when:
- Enabling theme support with `ThemesEnabled`
- Applying Office 2016 or other built-in styles
- Implementing a theme switcher
- Choosing the right style for your application

## Enabling Themes

```csharp
progressBarAdv1.ThemesEnabled = true;
```

When enabled, the progress bar applies the current application theme automatically.

## ProgressStyle Options

```csharp
// Office 2016 Colorful — vibrant blue, best for business apps
progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016Colorful;

// Office 2016 White — clean/minimalist
progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016White;

// Office 2016 Dark Gray — reduced brightness, extended use
progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016DarkGray;

// Office 2016 Black — high contrast, dark apps
progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016Black;

// System — native Windows look
progressBarAdv1.ProgressStyle = ProgressBarStyles.System;

// Tube — segmented 3D appearance
progressBarAdv1.ProgressStyle = ProgressBarStyles.Tube;

// Gradient — smooth two-color gradient
progressBarAdv1.ProgressStyle = ProgressBarStyles.Gradient;
```

## Style Selection Guide

| Style | Best For | Color Scheme |
|-------|----------|--------------|
| `Office2016Colorful` | Business apps | Blue/Gray |
| `Office2016White` | Content/minimalist | Light/White |
| `Office2016DarkGray` | Long sessions | Medium Dark |
| `Office2016Black` | Media/dark UI apps | Black/High contrast |
| `System` | Windows-native feel | OS Default |
| `Tube` | Distinctive look | Customizable |
| `Gradient` | Modern/polished UI | Customizable |

## Apply Theme to All Progress Bars

```csharp
private void ApplyApplicationTheme(ProgressBarStyles style)
{
    foreach (Control control in this.Controls)
    {
        if (control is ProgressBarAdv pb)
        {
            pb.ProgressStyle = style;
            pb.ThemesEnabled = true;
        }
    }
}
```

## Theme Switcher

```csharp
private void cboTheme_SelectedIndexChanged(object sender, EventArgs e)
{
    ProgressBarStyles style = cboTheme.SelectedIndex switch
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
    progressBarAdv1.ProgressStyle = style;
}
```

## Combine Theme with Custom Colors

```csharp
progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016Colorful;
progressBarAdv1.ForeColor = Color.Green;    // Override progress color
progressBarAdv1.BackColor = Color.LightGray; // Override background
```

## Detect System Theme and Apply Matching Style

```csharp
private void ApplySystemTheme()
{
    try
    {
        using (var key = Microsoft.Win32.Registry.CurrentUser.OpenSubKey(
            @"Software\Microsoft\Windows\CurrentVersion\Themes\Personalize"))
        {
            var value = key?.GetValue("AppsUseLightTheme");
            bool isLight = value != null && (int)value == 1;
            progressBarAdv1.ProgressStyle = isLight
                ? ProgressBarStyles.Office2016White
                : ProgressBarStyles.Office2016DarkGray;
        }
    }
    catch
    {
        progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016Colorful;
    }
}
```

## Best Practices

- Use the same `ProgressStyle` across all progress bars for a consistent UI.
- Set `ThemesEnabled = true` before setting `ProgressStyle`.
- Avoid manually overriding colors when using Office 2016 themes unless intentional.

## Related Topics

- [appearance-styling.md](appearance-styling.md) — custom colors and gradients
- [events-advanced.md](events-advanced.md) — advanced customization
