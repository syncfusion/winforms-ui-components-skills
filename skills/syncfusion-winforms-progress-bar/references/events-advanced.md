# Events and Advanced Customization

This guide covers event handling and advanced customization options for the ProgressBarAdv control.

## Table of Contents
- [ValueChanged Event](#valuechanged-event)
- [DrawWaitingCustomRender Event](#drawwaitingcustomrender-event)
- [Custom Rendering](#custom-rendering)
- [Waiting Mode](#waiting-mode)
- [Advanced Patterns](#advanced-patterns)

## ValueChanged Event

The `ValueChanged` event occurs when the `Value` property changes.

### Event Declaration

```csharp
public event EventHandler ValueChanged;
```

### Subscribing to ValueChanged

```csharp
// Subscribe to the event
progressBarAdv1.ValueChanged += ProgressBarAdv1_ValueChanged;

private void ProgressBarAdv1_ValueChanged(object sender, EventArgs e)
{
    ProgressBarAdv progressBar = sender as ProgressBarAdv;
    Console.WriteLine($"Progress: {progressBar.Value}%");
}
```

**VB.NET:**
```vb
' Subscribe to the event
AddHandler progressBarAdv1.ValueChanged, AddressOf ProgressBarAdv1_ValueChanged

Private Sub ProgressBarAdv1_ValueChanged(sender As Object, e As EventArgs)
    Dim progressBar As ProgressBarAdv = TryCast(sender, ProgressBarAdv)
    Console.WriteLine($"Progress: {progressBar.Value}%")
End Sub
```

### Common Use Cases

**Update Custom Text:**
```csharp
private void ProgressBarAdv1_ValueChanged(object sender, EventArgs e)
{
    ProgressBarAdv pb = sender as ProgressBarAdv;
    
    if (pb.Value < 100)
    {
        pb.TextStyle = ProgressBarTextStyles.Custom;
        pb.CustomText = $"Processing... {pb.Value}%";
    }
    else
    {
        pb.CustomText = "Complete!";
    }
}
```

**Update Related UI Elements:**
```csharp
private void ProgressBarAdv1_ValueChanged(object sender, EventArgs e)
{
    ProgressBarAdv pb = sender as ProgressBarAdv;
    
    // Update label
    statusLabel.Text = $"Progress: {pb.Value}%";
    
    // Enable button when complete
    if (pb.Value >= 100)
    {
        completeButton.Enabled = true;
    }
}
```

**Trigger Actions at Milestones:**
```csharp
private void ProgressBarAdv1_ValueChanged(object sender, EventArgs e)
{
    ProgressBarAdv pb = sender as ProgressBarAdv;
    
    switch (pb.Value)
    {
        case 25:
            LogMessage("25% complete - Phase 1 done");
            break;
        case 50:
            LogMessage("50% complete - Phase 2 done");
            break;
        case 75:
            LogMessage("75% complete - Phase 3 done");
            break;
        case 100:
            LogMessage("100% complete - All phases done");
            OnOperationComplete();
            break;
    }
}
```

## DrawWaitingCustomRender Event

The `DrawWaitingCustomRender` event allows custom rendering of the waiting gradient.

### Event Declaration

```csharp
public event ProgressBarAdvDrawEventHandler DrawWaitingCustomRender;
```

### Event Arguments

**ProgressBarAdvDrawEventArgs properties:**

| Property | Type | Description |
|----------|------|-------------|
| Graphics | Graphics | Graphics object for custom drawing |
| Rectangle | Rectangle | Bounding rectangle for drawing |
| Handled | bool | Set to true if custom drawing is performed |

### Enabling Custom Rendering

Set `WaitingCustomRender` to true to enable the event:

```csharp
this.progressBarAdv1.WaitingCustomRender = true;
this.progressBarAdv1.DrawWaitingCustomRender += ProgressBarAdv1_DrawWaitingCustomRender;
```

### Basic Custom Rendering

```csharp
private void ProgressBarAdv1_DrawWaitingCustomRender(object sender, ProgressBarAdvDrawEventArgs e)
{
    // Perform custom drawing
    using (SolidBrush brush = new SolidBrush(Color.Blue))
    {
        e.Graphics.FillRectangle(brush, e.Rectangle);
    }
    
    // Indicate that custom drawing was performed
    e.Handled = true;
}
```

**VB.NET:**
```vb
Private Sub ProgressBarAdv1_DrawWaitingCustomRender(sender As Object, e As ProgressBarAdvDrawEventArgs)
    ' Perform custom drawing
    Using brush As New SolidBrush(Color.Blue)
        e.Graphics.FillRectangle(brush, e.Rectangle)
    End Using
    
    ' Indicate that custom drawing was performed
    e.Handled = True
End Sub
```

### Custom Gradient Rendering

```csharp
private void ProgressBarAdv1_DrawWaitingCustomRender(object sender, ProgressBarAdvDrawEventArgs e)
{
    // Create custom gradient
    using (LinearGradientBrush brush = new LinearGradientBrush(
        e.Rectangle,
        Color.LightBlue,
        Color.DarkBlue,
        LinearGradientMode.Horizontal))
    {
        e.Graphics.FillRectangle(brush, e.Rectangle);
    }
    
    e.Handled = true;
}
```

### Animated Custom Rendering

```csharp
private int animationOffset = 0;

private void ProgressBarAdv1_DrawWaitingCustomRender(object sender, ProgressBarAdvDrawEventArgs e)
{
    // Draw animated stripes
    int stripeWidth = 20;
    Color color1 = Color.Blue;
    Color color2 = Color.LightBlue;
    
    for (int x = -stripeWidth + animationOffset; x < e.Rectangle.Width; x += stripeWidth * 2)
    {
        Rectangle stripe = new Rectangle(
            e.Rectangle.X + x,
            e.Rectangle.Y,
            stripeWidth,
            e.Rectangle.Height);
        
        using (SolidBrush brush = new SolidBrush(color1))
        {
            e.Graphics.FillRectangle(brush, stripe);
        }
    }
    
    animationOffset = (animationOffset + 1) % (stripeWidth * 2);
    e.Handled = true;
}
```

## Custom Rendering

### Progress FallBack Style

The `ProgressFallBackStyle` property determines the default rendering when custom rendering is not handled:

```csharp
// Continuous style
this.progressBarAdv1.ProgressFallBackStyle = ProgressBarFallbackStyle.Continuous;

// Tube style (segmented)
this.progressBarAdv1.ProgressFallBackStyle = ProgressBarFallbackStyle.Tube;

// Gradient style
this.progressBarAdv1.ProgressFallBackStyle = ProgressBarFallbackStyle.Gradient;

// System style
this.progressBarAdv1.ProgressFallBackStyle = ProgressBarFallbackStyle.System;
```

### Custom Pattern Example

```csharp
private void DrawCheckerboardPattern(Graphics g, Rectangle rect)
{
    int size = 8;
    Color color1 = Color.Blue;
    Color color2 = Color.LightBlue;
    
    for (int y = 0; y < rect.Height; y += size)
    {
        for (int x = 0; x < rect.Width; x += size)
        {
            Color color = ((x / size + y / size) % 2 == 0) ? color1 : color2;
            using (SolidBrush brush = new SolidBrush(color))
            {
                g.FillRectangle(brush, 
                    rect.X + x, 
                    rect.Y + y, 
                    Math.Min(size, rect.Width - x), 
                    Math.Min(size, rect.Height - y));
            }
        }
    }
}

private void ProgressBarAdv1_DrawWaitingCustomRender(object sender, ProgressBarAdvDrawEventArgs e)
{
    DrawCheckerboardPattern(e.Graphics, e.Rectangle);
    e.Handled = true;
}
```

## Waiting Mode

Enable waiting/indeterminate mode for operations of unknown duration.

### Enable Waiting Mode

```csharp
// Enable waiting mode
this.progressBarAdv1.WaitingGradientEnabled = true;
```

### Custom Waiting Gradient

```csharp
this.progressBarAdv1.WaitingGradientEnabled = true;
this.progressBarAdv1.WaitingCustomRender = true;
this.progressBarAdv1.DrawWaitingCustomRender += CustomWaitingRender;

private void CustomWaitingRender(object sender, ProgressBarAdvDrawEventArgs e)
{
    // Custom waiting animation
    e.Handled = true;
}
```

## Advanced Patterns

### Pattern 1: Multi-Stage Progress

```csharp
public class MultiStageProgress
{
    private ProgressBarAdv progressBar;
    private int currentStage = 0;
    private string[] stages = { "Loading", "Processing", "Saving", "Complete" };
    
    public MultiStageProgress(ProgressBarAdv pb)
    {
        progressBar = pb;
        progressBar.ValueChanged += ProgressBar_ValueChanged;
    }
    
    private void ProgressBar_ValueChanged(object sender, EventArgs e)
    {
        int stage = progressBar.Value / 25;
        if (stage != currentStage && stage < stages.Length)
        {
            currentStage = stage;
            progressBar.TextStyle = ProgressBarTextStyles.Custom;
            progressBar.CustomText = stages[currentStage];
            OnStageChanged(stages[currentStage]);
        }
    }
    
    private void OnStageChanged(string stageName)
    {
        Console.WriteLine($"Entered stage: {stageName}");
    }
}
```

### Pattern 2: Color-Changing Progress

```csharp
private void ProgressBarAdv1_ValueChanged(object sender, EventArgs e)
{
    ProgressBarAdv pb = sender as ProgressBarAdv;
    
    // Change color based on progress
    if (pb.Value < 30)
    {
        pb.ForeColor = Color.Red;
    }
    else if (pb.Value < 70)
    {
        pb.ForeColor = Color.Orange;
    }
    else
    {
        pb.ForeColor = Color.Green;
    }
}
```

### Pattern 3: Progress with Callbacks

```csharp
public class ProgressTracker
{
    private ProgressBarAdv progressBar;
    private Action<int> onProgressChanged;
    private Action onCompleted;
    
    public ProgressTracker(ProgressBarAdv pb, Action<int> progressCallback, Action completedCallback)
    {
        progressBar = pb;
        onProgressChanged = progressCallback;
        onCompleted = completedCallback;
        
        progressBar.ValueChanged += ProgressBar_ValueChanged;
    }
    
    private void ProgressBar_ValueChanged(object sender, EventArgs e)
    {
        ProgressBarAdv pb = sender as ProgressBarAdv;
        onProgressChanged?.Invoke(pb.Value);
        
        if (pb.Value >= pb.Maximum)
        {
            onCompleted?.Invoke();
        }
    }
}

// Usage
var tracker = new ProgressTracker(
    progressBarAdv1,
    progress => statusLabel.Text = $"Progress: {progress}%",
    () => MessageBox.Show("Complete!")
);
```

## Best Practices

### Event Handler Management

```csharp
// Subscribe in constructor or Load event
private void Form_Load(object sender, EventArgs e)
{
    progressBarAdv1.ValueChanged += ProgressBarAdv1_ValueChanged;
}

// Unsubscribe in Dispose or FormClosing
protected override void Dispose(bool disposing)
{
    if (disposing)
    {
        progressBarAdv1.ValueChanged -= ProgressBarAdv1_ValueChanged;
    }
    base.Dispose(disposing);
}
```

### Performance Considerations

```csharp
// Avoid heavy operations in ValueChanged
private void ProgressBarAdv1_ValueChanged(object sender, EventArgs e)
{
    // ❌ Bad: Heavy operation
    // LoadLargeDataset();
    
    // ✅ Good: Lightweight update
    statusLabel.Text = $"{progressBarAdv1.Value}%";
}
```

### Custom Rendering Optimization

```csharp
// Reuse graphics objects
private Brush cachedBrush;

private void ProgressBarAdv1_DrawWaitingCustomRender(object sender, ProgressBarAdvDrawEventArgs e)
{
    if (cachedBrush == null)
    {
        cachedBrush = new SolidBrush(Color.Blue);
    }
    
    e.Graphics.FillRectangle(cachedBrush, e.Rectangle);
    e.Handled = true;
}
```

## Complete Example

```csharp
using System;
using System.Drawing;
using System.Drawing.Drawing2D;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class AdvancedProgressForm : Form
{
    private ProgressBarAdv progressBar;
    private Timer progressTimer;
    private int animationPhase = 0;
    
    public AdvancedProgressForm()
    {
        InitializeComponents();
    }
    
    private void InitializeComponents()
    {
        // Setup progress bar
        progressBar = new ProgressBarAdv();
        progressBar.Location = new Point(20, 20);
        progressBar.Size = new Size(400, 30);
        progressBar.Minimum = 0;
        progressBar.Maximum = 100;
        progressBar.Value = 0;
        progressBar.TextStyle = ProgressBarTextStyles.Custom;
        progressBar.CustomText = "Ready";
        
        // Subscribe to events
        progressBar.ValueChanged += ProgressBar_ValueChanged;
        progressBar.WaitingCustomRender = true;
        progressBar.DrawWaitingCustomRender += ProgressBar_DrawWaitingCustomRender;
        
        this.Controls.Add(progressBar);
        
        // Setup timer
        progressTimer = new Timer();
        progressTimer.Interval = 100;
        progressTimer.Tick += ProgressTimer_Tick;
        progressTimer.Start();
    }
    
    private void ProgressBar_ValueChanged(object sender, EventArgs e)
    {
        if (progressBar.Value < 100)
        {
            progressBar.CustomText = $"Processing... {progressBar.Value}%";
        }
        else
        {
            progressBar.CustomText = "Complete!";
            progressTimer.Stop();
        }
    }
    
    private void ProgressBar_DrawWaitingCustomRender(object sender, ProgressBarAdvDrawEventArgs e)
    {
        // Custom animated gradient
        Color startColor = Color.FromArgb(animationPhase, 100, 200);
        Color endColor = Color.FromArgb(255 - animationPhase, 150, 255);
        
        using (LinearGradientBrush brush = new LinearGradientBrush(
            e.Rectangle, startColor, endColor, LinearGradientMode.Horizontal))
        {
            e.Graphics.FillRectangle(brush, e.Rectangle);
        }
        
        animationPhase = (animationPhase + 5) % 256;
        e.Handled = true;
    }
    
    private void ProgressTimer_Tick(object sender, EventArgs e)
    {
        if (progressBar.Value < 100)
        {
            progressBar.Value += 2;
        }
    }
}
```

## Next Steps

- **Getting started:** See [getting-started.md](getting-started.md) for basic setup
- **Text display:** See [text-display.md](text-display.md) for text customization
- **Themes:** See [themes.md](themes.md) for visual styling
