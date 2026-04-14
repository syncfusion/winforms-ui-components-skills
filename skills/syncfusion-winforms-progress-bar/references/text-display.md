# Text Display Configuration in ProgressBarAdv

This reference covers text display options in ProgressBarAdv: text styles, alignment, orientation, visibility, shadows, font, and custom text.

## When to Read This

Read this reference when:
- Configuring TextStyle (Percentage, Value, Custom)
- Customizing text alignment, orientation, or visibility
- Setting font and text color
- Implementing dynamic custom text via ValueChanged

## Text Style Options

The `TextStyle` property controls what text is displayed.

### Percentage Style

```csharp
progressBarAdv1.TextStyle = ProgressBarTextStyles.Percentage;
progressBarAdv1.TextVisible = true;
progressBarAdv1.Value = 45;
// Displays: "45%"
```

### Value Style

```csharp
progressBarAdv1.TextStyle = ProgressBarTextStyles.Value;
progressBarAdv1.TextVisible = true;
progressBarAdv1.Value = 45;
// Displays: "45"
```

### Custom Style

Use the `ValueChanged` event to set custom text dynamically:

```csharp
progressBarAdv1.TextStyle = ProgressBarTextStyles.Custom;
progressBarAdv1.TextVisible = true;
progressBarAdv1.ValueChanged += ProgressBarAdv1_ValueChanged;

private void ProgressBarAdv1_ValueChanged(object sender, EventArgs e)
{
    ProgressBarAdv pb = sender as ProgressBarAdv;
    int percentage = (int)((pb.Value - pb.Minimum) * 100.0 / (pb.Maximum - pb.Minimum));
    pb.Text = $"Loading... {percentage}% Complete";
}
```

## Text Alignment

```csharp
// Center (default)
progressBarAdv1.TextAlignment = TextAlignment.Center;

// Left
progressBarAdv1.TextAlignment = TextAlignment.Left;

// Right
progressBarAdv1.TextAlignment = TextAlignment.Right;
```

## Text Orientation

Match `TextOrientation` with `ProgressOrientation` for consistency:

```csharp
// Horizontal (default)
progressBarAdv1.ProgressOrientation = Orientation.Horizontal;
progressBarAdv1.TextOrientation = Orientation.Horizontal;

// Vertical
progressBarAdv1.ProgressOrientation = Orientation.Vertical;
progressBarAdv1.TextOrientation = Orientation.Vertical;
progressBarAdv1.Size = new Size(50, 200);
```

## Text Visibility

```csharp
// Show text
progressBarAdv1.TextVisible = true;

// Hide text
progressBarAdv1.TextVisible = false;
```

## Text Shadow

```csharp
progressBarAdv1.TextShadow = true;
progressBarAdv1.FontColor = Color.White;
progressBarAdv1.BackColor = Color.DarkBlue;
```

## Font and Color

```csharp
// Custom font
progressBarAdv1.Font = new Font("Segoe UI", 10F, FontStyle.Bold);

// Text color (use FontColor, not ForeColor)
progressBarAdv1.FontColor = Color.SteelBlue;

// Dynamic color based on progress
private void SetProgressTextColor()
{
    int pct = (progressBarAdv1.Value * 100) / progressBarAdv1.Maximum;
    if (pct < 30)
        progressBarAdv1.FontColor = Color.Red;
    else if (pct < 70)
        progressBarAdv1.FontColor = Color.Orange;
    else
        progressBarAdv1.FontColor = Color.Green;
}
```

## Custom Text — Common Patterns

### File Processing Counter

```csharp
progressBarAdv1.TextStyle = ProgressBarTextStyles.Custom;
progressBarAdv1.Maximum = totalFiles;
progressBarAdv1.ValueChanged += (s, e) =>
    progressBarAdv1.Text = $"Processing: {progressBarAdv1.Value} / {totalFiles} files";
```

### Download Progress with Speed

```csharp
progressBarAdv1.TextStyle = ProgressBarTextStyles.Custom;
progressBarAdv1.ValueChanged += (s, e) =>
    progressBarAdv1.Text = $"{progressBarAdv1.Value}% – {currentSpeed:F2} MB/s";
```

## Best Practices

- Use `FontColor` (not `ForeColor`) to change text color.
- Match `TextOrientation` with `ProgressOrientation` for vertical bars.
- Keep custom text concise — long strings may be clipped.
- Enable `TextShadow` when using light text on dark backgrounds.
- Set `ThemesEnabled = false` when using custom `FontColor`; theme styling may override it.

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Text not visible | Verify `TextVisible = true`; check `FontColor` contrast |
| Custom text not updating | Ensure `TextStyle = Custom` and `ValueChanged` is wired |
| Text cut off | Reduce font size or widen the progress bar |
| `FontColor` ignored | Set `ThemesEnabled = false` before applying `FontColor` |
| Text alignment not working | Ensure text is not too long to force overflow |

## Related Topics

- [appearance-styling.md](appearance-styling.md) — foreground and background colors
- [orientation-layout.md](orientation-layout.md) — orientation settings
- [themes.md](themes.md) — theme-based text styling
- [events-advanced.md](events-advanced.md) — ValueChanged event details
