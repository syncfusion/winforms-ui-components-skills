# Appearance and Styling in ProgressBarAdv

This reference covers foreground styles, background styles, colors, gradients, segments, images, and Office 2016 themes for ProgressBarAdv.

## When to Read This

Read this reference when:
- Customizing foreground or background colors and gradients
- Applying Office 2016 or other built-in styles
- Using segment, tube, or image progress styles
- Configuring border settings
- Troubleshooting appearance issues

## Foreground Settings

The foreground is the filled portion showing actual progress.

### Solid Color

```csharp
progressBarAdv1.ProgressStyle = ProgressBarStyles.Constant;
progressBarAdv1.ForeColor = Color.Turquoise;
progressBarAdv1.FontColor = Color.White;
```

### Segmented Foreground

```csharp
progressBarAdv1.ForeSegments = true;
progressBarAdv1.SegmentWidth = 10;
progressBarAdv1.ForeColor = Color.Green;
```

### Gradient Foreground

```csharp
progressBarAdv1.ProgressStyle = ProgressBarStyles.Gradient;
progressBarAdv1.GradientStartColor = Color.OrangeRed;
progressBarAdv1.GradientEndColor = Color.Yellow;
```

### Multiple Gradient Colors

```csharp
progressBarAdv1.ProgressStyle = ProgressBarStyles.MultipleGradient;
progressBarAdv1.MultipleColors = new Color[]
{
    Color.DarkBlue, Color.RoyalBlue, Color.DeepSkyBlue, Color.LightCyan
};
progressBarAdv1.StretchMultGrad = true;
```

### Tube Style

```csharp
progressBarAdv1.ProgressStyle = ProgressBarStyles.Tube;
progressBarAdv1.TubeStartColor = Color.LimeGreen;
progressBarAdv1.TubeEndColor = Color.DarkGreen;
```

### Image Style

```csharp
progressBarAdv1.ProgressStyle = ProgressBarStyles.Image;
progressBarAdv1.BackgroundStyle = ProgressBarBackgroundStyles.Image;
progressBarAdv1.ForegroundImage = Image.FromFile("progress_texture.png");
progressBarAdv1.StretchImage = true;
```

### Office 2016 Styles

```csharp
// Colorful (blue accents — recommended for business apps)
progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016Colorful;

// White (light/minimalist)
progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016White;

// Dark Gray (reduced brightness)
progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016DarkGray;

// Black (high contrast)
progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016Black;
```

## Background Settings

```csharp
// Solid color background
progressBarAdv1.BackColor = Color.LightGray;

// Gradient background
progressBarAdv1.BackgroundStyle = ProgressBarBackgroundStyles.Gradient;
progressBarAdv1.BackGradientStartColor = Color.IndianRed;
progressBarAdv1.BackGradientEndColor = Color.Aquamarine;

// Tube background
progressBarAdv1.BackgroundStyle = ProgressBarBackgroundStyles.Tube;
progressBarAdv1.BackTubeStartColor = Color.Yellow;
progressBarAdv1.BackTubeEndColor = Color.RosyBrown;

// Multiple gradient background
progressBarAdv1.BackgroundStyle = ProgressBarBackgroundStyles.MultipleGradient;
progressBarAdv1.BackMultipleColors = new Color[]
{
    Color.Blue, Color.Red, Color.Green, Color.Pink, Color.Yellow
};

// Match foreground Office 2016 style
progressBarAdv1.BackgroundStyle = ProgressBarBackgroundStyles.Office2016Colorful;
```

## Border Settings

```csharp
// 3D border
progressBarAdv1.BorderStyle = BorderStyle.Fixed3D;
progressBarAdv1.Border3DStyle = Border3DStyle.Sunken;

// Single solid border
progressBarAdv1.BorderStyle = BorderStyle.FixedSingle;
progressBarAdv1.BorderSingle = ButtonBorderStyle.Solid;
progressBarAdv1.BorderColor = Color.Black;
```

## Coordinated Style Example

```csharp
// Office 2016 Colorful — progress bar matching background
progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016Colorful;
progressBarAdv1.BackgroundStyle = ProgressBarBackgroundStyles.Office2016Colorful;
progressBarAdv1.TextStyle = ProgressBarTextStyles.Percentage;
progressBarAdv1.TextVisible = true;
```

## Best Practices

- Match `ProgressStyle` and `BackgroundStyle` when using Office 2016 themes.
- Use `Office2016Colorful` for modern business applications.
- Test text contrast when combining custom foreground and background colors.
- For high-frequency updates, prefer simple styles (Constant/Office2016) over images.
- Do not manually override colors when `ThemesEnabled = true` — theme will override.

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Colors not showing | Verify `ProgressStyle` matches the color properties used |
| Image not visible | Set both `ProgressStyle` and `BackgroundStyle` to Image; check path |
| Office 2016 looks wrong | Match `ProgressStyle` with `BackgroundStyle`; don't override theme colors |
| Segments not visible | Set `ForeSegments = true` and adjust `SegmentWidth` |

## Related Topics

- [text-display.md](text-display.md) — text styling
- [orientation-layout.md](orientation-layout.md) — layout considerations
- [themes.md](themes.md) — theme configuration
- [events-advanced.md](events-advanced.md) — custom rendering events
