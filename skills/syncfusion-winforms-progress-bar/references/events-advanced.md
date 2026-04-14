# Events and Advanced Customization

This reference covers event handling and advanced customization for ProgressBarAdv.

## When to Read This

Read this reference when:
- Handling the `ValueChanged` event
- Implementing `DrawWaitingCustomRender` for custom rendering
- Enabling waiting/indeterminate mode
- Implementing advanced progress patterns

## ValueChanged Event

Fires each time the `Value` property changes.

```csharp
progressBarAdv1.ValueChanged += ProgressBarAdv1_ValueChanged;

private void ProgressBarAdv1_ValueChanged(object sender, EventArgs e)
{
    ProgressBarAdv pb = sender as ProgressBarAdv;

    // Update custom text
    pb.TextStyle = ProgressBarTextStyles.Custom;
    pb.CustomText = pb.Value < 100 ? $"Processing... {pb.Value}%" : "Complete!";

    // Update related UI
    statusLabel.Text = $"Progress: {pb.Value}%";

    // Enable button when complete
    if (pb.Value >= 100)
        completeButton.Enabled = true;
}
```

### Milestone Triggers

```csharp
private void ProgressBarAdv1_ValueChanged(object sender, EventArgs e)
{
    ProgressBarAdv pb = sender as ProgressBarAdv;
    switch (pb.Value)
    {
        case 25: LogMessage("Phase 1 done"); break;
        case 50: LogMessage("Phase 2 done"); break;
        case 75: LogMessage("Phase 3 done"); break;
        case 100: LogMessage("All done"); OnOperationComplete(); break;
    }
}
```

## DrawWaitingCustomRender Event

Fires during waiting/indeterminate mode when `WaitingCustomRender = true`.

### Setup

```csharp
progressBarAdv1.WaitingGradientEnabled = true;
progressBarAdv1.WaitingCustomRender = true;
progressBarAdv1.DrawWaitingCustomRender += ProgressBarAdv1_DrawWaitingCustomRender;
```

### Event Arguments

| Property | Type | Description |
|----------|------|-------------|
| `Graphics` | `Graphics` | Graphics object for drawing |
| `Rectangle` | `Rectangle` | Bounding rectangle |
| `Handled` | `bool` | Set `true` after custom drawing |

### Custom Gradient Rendering

```csharp
private void ProgressBarAdv1_DrawWaitingCustomRender(
    object sender, ProgressBarAdvDrawEventArgs e)
{
    using (var brush = new LinearGradientBrush(
        e.Rectangle, Color.LightBlue, Color.DarkBlue,
        LinearGradientMode.Horizontal))
    {
        e.Graphics.FillRectangle(brush, e.Rectangle);
    }
    e.Handled = true;
}
```

## ProgressFallBackStyle

Determines rendering when custom drawing is not handled:

```csharp
progressBarAdv1.ProgressFallBackStyle = ProgressBarFallbackStyle.Continuous;
// Options: Continuous, Tube, Gradient, System
```

## Advanced Patterns

### Color-Changing by Progress

```csharp
private void ProgressBarAdv1_ValueChanged(object sender, EventArgs e)
{
    ProgressBarAdv pb = sender as ProgressBarAdv;
    if (pb.Value < 30)       pb.ForeColor = Color.Red;
    else if (pb.Value < 70)  pb.ForeColor = Color.Orange;
    else                     pb.ForeColor = Color.Green;
}
```

### Multi-Stage Progress

```csharp
private readonly string[] _stages = { "Loading", "Processing", "Saving", "Complete" };
private int _currentStage = 0;

private void ProgressBarAdv1_ValueChanged(object sender, EventArgs e)
{
    ProgressBarAdv pb = sender as ProgressBarAdv;
    int stage = pb.Value / 25;
    if (stage != _currentStage && stage < _stages.Length)
    {
        _currentStage = stage;
        pb.TextStyle = ProgressBarTextStyles.Custom;
        pb.CustomText = _stages[_currentStage];
    }
}
```

## Best Practices

- Unsubscribe from `ValueChanged` in `Dispose` to avoid memory leaks.
- Keep `ValueChanged` handlers lightweight — avoid heavy operations inside.
- Cache brushes when using `DrawWaitingCustomRender` to avoid GDI object leaks.
- Always set `e.Handled = true` after custom drawing in `DrawWaitingCustomRender`.

```csharp
// Unsubscribe in Dispose
protected override void Dispose(bool disposing)
{
    if (disposing)
        progressBarAdv1.ValueChanged -= ProgressBarAdv1_ValueChanged;
    base.Dispose(disposing);
}
```

## Related Topics

- [getting-started.md](getting-started.md) — basic setup
- [text-display.md](text-display.md) — custom text via ValueChanged
- [themes.md](themes.md) — visual styling
