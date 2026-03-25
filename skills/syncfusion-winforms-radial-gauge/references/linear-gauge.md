# LinearGauge

## Table of Contents
- [Overview](#overview)
- [Getting Started](#getting-started)
- [Frame Types](#frame-types)
- [Scales and Labels](#scales-and-labels)
- [Ticks Configuration](#ticks-configuration)
- [Pointer and Needles](#pointer-and-needles)
- [Ranges](#ranges)
- [Scaling Divisions](#scaling-divisions)
- [Frame Appearance](#frame-appearance)
- [Common Scenarios](#common-scenarios)
- [Troubleshooting](#troubleshooting)

## Overview

LinearGauge visualizes numeric values on a **linear scale** (horizontal or vertical) with pointer indicators and color-coded ranges. Ideal for progress bars, level indicators, battery displays, and slider visualizations.

**Key capabilities:**
- Horizontal and vertical orientations
- Pointer placement (Top, Center, Bottom)
- Color-coded value ranges
- Major and minor tick marks
- Value indicator bars
- Professional themes

## Getting Started

### Designer Setup

1. **Add LinearGauge to form:**
   - Drag from Toolbox → Place on form
   - Set Size (e.g., 400x100 for Horizontal, 100x400 for Vertical)

2. **Configure basic properties (Properties window):**
   ```
   LinearFrameType: Horizontal
   MinimumValue: 0
   MaximumValue: 100
   Value: 65
   MajorDifference: 20
   MinorTickCount: 3
   ShowNeedle: True
   PointerPlacement: Center
   ```

3. **Run** - Gauge displays as configured

### Code Setup

```csharp
using Syncfusion.Windows.Forms.Gauge;

// Create gauge
LinearGauge progressBar = new LinearGauge();
progressBar.Size = new Size(400, 100);
progressBar.Location = new Point(20, 20);

// Basic configuration
progressBar.LinearFrameType = LinearFrameType.Horizontal;
progressBar.MinimumValue = 0;
progressBar.MaximumValue = 100;
progressBar.Value = 65;
progressBar.MajorDifference = 20;
progressBar.MinorTickCount = 3;
progressBar.ShowNeedle = true;
progressBar.PointerPlacement = Placement.Center;

// Add to form
this.Controls.Add(progressBar);
```

## Frame Types

### Horizontal

Left-to-right linear gauge.

```csharp
LinearGauge gauge = new LinearGauge();
gauge.LinearFrameType = LinearFrameType.Horizontal;
gauge.Size = new Size(400, 100);  // Width > Height
```

**Use when:**
- Progress bars
- Loading indicators
- Horizontal sliders
- Timeline displays
- Volume meters
- Capacity indicators

**Layout considerations:**
- Width should be 3-5x height for good proportions
- Value increases left to right
- Labels appear above or below scale

### Vertical

Bottom-to-top linear gauge.

```csharp
LinearGauge gauge = new LinearGauge();
gauge.LinearFrameType = LinearFrameType.Vertical;
gauge.Size = new Size(100, 400);  // Height > Width
```

**Use when:**
- Level indicators (fuel, battery)
- Temperature displays (thermometer)
- Building height indicators
- Elevator position
- Vertical sliders
- Sound level meters

**Layout considerations:**
- Height should be 3-5x width for good proportions
- Value increases bottom to top
- Labels appear left or right of scale

## Scales and Labels

### Scale Configuration

```csharp
// Show/hide scale labels
gauge.ShowScaleLabel = true;

// Label font
gauge.Font = new Font("Segoe UI", 9);

// Scale color
gauge.ScaleLabelColor = Color.Black;
```

### Label Placement

For **Horizontal gauges:**
```csharp
// Labels above or below scale (controlled by control height and internal layout)
gauge.MajorTicksHeight = 8;  // Affects label position
```

For **Vertical gauges:**
```csharp
// Labels left or right of scale (controlled by control width and internal layout)
gauge.MajorTicksHeight = 8;
```

### Scale Value Display

```csharp
// Configure value range
gauge.MinimumValue = 0;
gauge.MaximumValue = 100;
gauge.MajorDifference = 25;  // Labels at 0, 25, 50, 75, 100

// Value formatting handled automatically
// For custom formatting, use custom renderer
```

## Ticks Configuration

### Major Ticks

Primary scale marks with labels.

```csharp
// Major tick interval
gauge.MajorDifference = 20;  // Tick every 20 units

// Major tick appearance
gauge.MajorTicksHeight = 10;  // Tick length
gauge.MajorTickMarkColor = Color.Black;
```

### Minor Ticks

Secondary scale marks between majors (no labels).

```csharp
// Number of minor ticks between major ticks
gauge.MinorTickCount = 4;  // 4 minor ticks between each major

// Minor tick appearance
gauge.MinorTickHeight = 5;  // Shorter than major
gauge.MinorTickMarkColor = Color.Gray;
```

**Calculation example:**
```csharp
// Major at 0, 20, 40, 60, 80, 100
gauge.MajorDifference = 20;

// Minor at 5, 10, 15 (between 0-20), etc.
gauge.MinorTickCount = 3;  // Creates 4 equal divisions
```

### Hiding Ticks

```csharp
// Hide all ticks
gauge.ShowTicks = false;

// Hide only major ticks
gauge.MajorTicksHeight = 0;

// Hide only minor ticks
gauge.MinorTickCount = 0;
```

### Tick Colors

```csharp
// Differentiate tick types with colors
gauge.MajorTickMarkColor = Color.Black;
gauge.MinorTickMarkColor = Color.DarkGray;
gauge.ScaleLabelColor = Color.Navy;
```

## Pointer and Needles

### Pointer Configuration

```csharp
// Show/hide pointer (needle)
gauge.ShowNeedle = true;

// Pointer color
gauge.NeedleColor = Color.Red;
```

### Pointer Placement

Controls where the pointer appears relative to the gauge frame.

```csharp
// Top placement (above scale for horizontal, left for vertical)
gauge.PointerPlacement = Placement.Top;

// Center placement (middle of scale)
gauge.PointerPlacement = Placement.Center;

// Bottom placement (below scale for horizontal, right for vertical)
gauge.PointerPlacement = Placement.Bottom;
```

**Horizontal gauge pointer positions:**

| Placement | Visual Position |
|-----------|----------------|
| Top | Above the scale bar |
| Center | Middle of the scale bar |
| Bottom | Below the scale bar |

**Vertical gauge pointer positions:**

| Placement | Visual Position |
|-----------|----------------|
| Top | Left side of scale bar |
| Center | Middle of the scale bar |
| Bottom | Right side of scale bar |

### Pointer Appearance

```csharp
// Pointer style
gauge.NeedleColor = Color.Blue;

// Value indicator bar (colored bar up to current value)
gauge.ValueIndicatorColor = Color.LightBlue;
gauge.ShowValueIndicator = true;
```

### Removing Pointer

For minimal or fill-only displays:

```csharp
gauge.ShowNeedle = false;  // No pointer displayed
```

## Ranges

Color-coded value ranges for visual thresholds.

### Adding Single Range

```csharp
LinearRange range = new LinearRange();
range.StartValue = 0;
range.EndValue = 50;
range.Color = Color.Green;
range.Height = 8;
range.RangePlacement = RangePlacement.Inside;

gauge.Ranges.Add(range);
```

### Multiple Ranges (Status Indicator)

```csharp
// Low range (critical)
LinearRange lowRange = new LinearRange();
lowRange.StartValue = 0;
lowRange.EndValue = 20;
lowRange.Color = Color.Red;
lowRange.Height = 10;
gauge.Ranges.Add(lowRange);

// Medium range (warning)
LinearRange mediumRange = new LinearRange();
mediumRange.StartValue = 20;
mediumRange.EndValue = 70;
mediumRange.Color = Color.Yellow;
mediumRange.Height = 10;
gauge.Ranges.Add(mediumRange);

// High range (good)
LinearRange highRange = new LinearRange();
highRange.StartValue = 70;
highRange.EndValue = 100;
highRange.Color = Color.Green;
highRange.Height = 10;
gauge.Ranges.Add(highRange);
```

### Range Properties

```csharp
LinearRange range = new LinearRange();

// Value boundaries
range.StartValue = 20;
range.EndValue = 80;

// Appearance
range.Color = Color.Blue;
range.Height = 12;  // Thickness perpendicular to gauge direction

// Placement relative to scale
range.RangePlacement = RangePlacement.Inside;  // Inside or Outside
```

### Range Placement

```csharp
// Inside: Range between pointer and center of gauge
range.RangePlacement = RangePlacement.Inside;

// Outside: Range beyond the scale ticks
range.RangePlacement = RangePlacement.Outside;
```

**Visual guide (Horizontal gauge):**
- `Inside`: Range bar appears in main gauge area
- `Outside`: Range bar appears above or below ticks

### Range Use Cases

```csharp
// Battery level indicator
LinearGauge battery = new LinearGauge();
battery.LinearFrameType = LinearFrameType.Horizontal;
battery.MinimumValue = 0;
battery.MaximumValue = 100;

// Critical (red): 0-15%
battery.Ranges.Add(new LinearRange { StartValue = 0, EndValue = 15, Color = Color.Red, Height = 10 });
// Low (orange): 15-30%
battery.Ranges.Add(new LinearRange { StartValue = 15, EndValue = 30, Color = Color.Orange, Height = 10 });
// Normal (green): 30-100%
battery.Ranges.Add(new LinearRange { StartValue = 30, EndValue = 100, Color = Color.Green, Height = 10 });
```

## Scaling Divisions

### Major Divisions

```csharp
// Value range
gauge.MinimumValue = 0;
gauge.MaximumValue = 200;

// Major tick every 25 units
gauge.MajorDifference = 25;  // Creates ticks at 0, 25, 50, 75, 100, 125, 150, 175, 200
```

### Minor Divisions

```csharp
// Major ticks every 20 units
gauge.MajorDifference = 20;

// 4 minor ticks between each major (creates 5 equal segments)
gauge.MinorTickCount = 4;  // Minor ticks at 5, 10, 15 between 0-20
```

**Minor tick calculation:**
- If `MajorDifference = 20` and `MinorTickCount = 4`
- Segment size = 20 / (4 + 1) = 4
- Minor ticks appear at: 4, 8, 12, 16

### Fine-Grained Scaling

```csharp
// Precise measurement gauge
gauge.MinimumValue = 0;
gauge.MaximumValue = 10;
gauge.MajorDifference = 1;    // Every 1 unit
gauge.MinorTickCount = 9;     // 0.1 unit precision (9 divisions between each major)
```

### Value Display Precision

```csharp
// For decimal values
gauge.MinimumValue = 0.0f;
gauge.MaximumValue = 1.0f;
gauge.MajorDifference = 0.2f;  // 0.0, 0.2, 0.4, 0.6, 0.8, 1.0
gauge.Value = 0.65f;
```

## Frame Appearance

### Background Colors

```csharp
// Gauge background
gauge.GaugeBaseColor = Color.LightGray;

// Value indicator color (filled portion)
gauge.ValueIndicatorColor = Color.Blue;
```

### Gradient Styling

```csharp
// Frame gradient
gauge.OuterFrameGradientStartColor = Color.White;
gauge.OuterFrameGradientEndColor = Color.LightGray;

// Inner frame gradient
gauge.InnerFrameGradientStartColor = Color.WhiteSmoke;
gauge.InnerFrameGradientEndColor = Color.Silver;
```

### Border and Frame

```csharp
// Frame border
gauge.FrameBorderColor = Color.Black;
gauge.FrameBorderWidth = 1;

// 3D effect (if available in theme)
gauge.VisualStyle = ThemeStyle.Default;
```

### Size and Proportions

**Horizontal gauge recommendations:**
```csharp
// Good proportions: width 3-5x height
gauge.Size = new Size(400, 100);  // 4:1 ratio
```

**Vertical gauge recommendations:**
```csharp
// Good proportions: height 3-5x width
gauge.Size = new Size(100, 400);  // 4:1 ratio
```

## Common Scenarios

### Scenario 1: Progress Bar

```csharp
LinearGauge progress = new LinearGauge();
progress.Size = new Size(400, 80);
progress.Location = new Point(20, 20);
progress.LinearFrameType = LinearFrameType.Horizontal;
progress.MinimumValue = 0;
progress.MaximumValue = 100;
progress.Value = 45;
progress.MajorDifference = 25;
progress.MinorTickCount = 4;
progress.ShowNeedle = false;  // No pointer needed
progress.ValueIndicatorColor = Color.DodgerBlue;
progress.GaugeBaseColor = Color.LightGray;

// Add to form
this.Controls.Add(progress);

// Update progress
Timer timer = new Timer();
timer.Interval = 100;
timer.Tick += (s, e) => {
    if (progress.Value < 100)
        progress.Value += 1;
    else
        timer.Stop();
};
timer.Start();
```

### Scenario 2: Battery Level Indicator

```csharp
LinearGauge battery = new LinearGauge();
battery.Size = new Size(350, 100);
battery.LinearFrameType = LinearFrameType.Horizontal;
battery.MinimumValue = 0;
battery.MaximumValue = 100;
battery.Value = 35;
battery.MajorDifference = 25;
battery.ShowNeedle = true;
battery.NeedleColor = Color.Black;
battery.PointerPlacement = Placement.Center;

// Battery level ranges
battery.Ranges.Add(new LinearRange { 
    StartValue = 0, EndValue = 15, 
    Color = Color.Red, Height = 12 
});
battery.Ranges.Add(new LinearRange { 
    StartValue = 15, EndValue = 30, 
    Color = Color.Orange, Height = 12 
});
battery.Ranges.Add(new LinearRange { 
    StartValue = 30, EndValue = 100, 
    Color = Color.Green, Height = 12 
});

this.Controls.Add(battery);
```

### Scenario 3: Vertical Temperature Gauge

```csharp
LinearGauge thermometer = new LinearGauge();
thermometer.Size = new Size(100, 350);
thermometer.Location = new Point(20, 20);
thermometer.LinearFrameType = LinearFrameType.Vertical;
thermometer.MinimumValue = -20;
thermometer.MaximumValue = 120;
thermometer.Value = 72;
thermometer.MajorDifference = 20;
thermometer.MinorTickCount = 3;
thermometer.ShowNeedle = true;
thermometer.PointerPlacement = Placement.Center;

// Temperature ranges
thermometer.Ranges.Add(new LinearRange { 
    StartValue = -20, EndValue = 32, 
    Color = Color.Blue, Height = 10 
});
thermometer.Ranges.Add(new LinearRange { 
    StartValue = 32, EndValue = 80, 
    Color = Color.Green, Height = 10 
});
thermometer.Ranges.Add(new LinearRange { 
    StartValue = 80, EndValue = 120, 
    Color = Color.Red, Height = 10 
});

this.Controls.Add(thermometer);
```

### Scenario 4: Fuel Gauge

```csharp
LinearGauge fuel = new LinearGauge();
fuel.Size = new Size(80, 300);
fuel.LinearFrameType = LinearFrameType.Vertical;
fuel.MinimumValue = 0;
fuel.MaximumValue = 100;
fuel.Value = 25;
fuel.MajorDifference = 25;
fuel.ShowNeedle = true;
fuel.NeedleColor = Color.Black;

// Fuel ranges
fuel.Ranges.Add(new LinearRange { 
    StartValue = 0, EndValue = 10, 
    Color = Color.Red, Height = 10 
});
fuel.Ranges.Add(new LinearRange { 
    StartValue = 10, EndValue = 30, 
    Color = Color.Yellow, Height = 10 
});
fuel.Ranges.Add(new LinearRange { 
    StartValue = 30, EndValue = 100, 
    Color = Color.Green, Height = 10 
});

this.Controls.Add(fuel);
```

### Scenario 5: Audio Level Meter

```csharp
LinearGauge audioLevel = new LinearGauge();
audioLevel.Size = new Size(400, 60);
audioLevel.LinearFrameType = LinearFrameType.Horizontal;
audioLevel.MinimumValue = 0;
audioLevel.MaximumValue = 100;
audioLevel.Value = 0;
audioLevel.MajorDifference = 10;
audioLevel.MinorTickCount = 9;
audioLevel.ShowNeedle = false;
audioLevel.ValueIndicatorColor = Color.Lime;

// Peak indicator ranges
audioLevel.Ranges.Add(new LinearRange { 
    StartValue = 0, EndValue = 80, 
    Color = Color.Green, Height = 8 
});
audioLevel.Ranges.Add(new LinearRange { 
    StartValue = 80, EndValue = 95, 
    Color = Color.Yellow, Height = 8 
});
audioLevel.Ranges.Add(new LinearRange { 
    StartValue = 95, EndValue = 100, 
    Color = Color.Red, Height = 8 
});

// Simulate audio input
Timer audioTimer = new Timer();
audioTimer.Interval = 50;
audioTimer.Tick += (s, e) => {
    audioLevel.Value = GetAudioLevel();  // Your audio level logic
};
audioTimer.Start();
```

### Scenario 6: Slider Visualization

```csharp
// Linked to TrackBar control
LinearGauge sliderGauge = new LinearGauge();
sliderGauge.Size = new Size(300, 70);
sliderGauge.LinearFrameType = LinearFrameType.Horizontal;
sliderGauge.MinimumValue = 0;
sliderGauge.MaximumValue = 100;
sliderGauge.ShowNeedle = true;
sliderGauge.PointerPlacement = Placement.Center;

TrackBar trackBar = new TrackBar();
trackBar.Minimum = 0;
trackBar.Maximum = 100;
trackBar.Value = 50;

// Link TrackBar to LinearGauge
trackBar.ValueChanged += (s, e) => {
    sliderGauge.Value = trackBar.Value;
};

sliderGauge.Value = trackBar.Value;
```

## Troubleshooting

### Issue: Gauge appears too narrow/wide

**Cause:** Incorrect size proportions

**Solution:**
```csharp
// Horizontal: Width should be 3-5x height
gauge.Size = new Size(400, 100);  // 4:1 ratio ✓

// Vertical: Height should be 3-5x width
gauge.Size = new Size(100, 400);  // 4:1 ratio ✓
```

### Issue: Pointer not visible

**Causes:**
- `ShowNeedle = false`
- Pointer color matches background
- Wrong pointer placement

**Solution:**
```csharp
gauge.ShowNeedle = true;
gauge.NeedleColor = Color.Red;  // Contrasting color
gauge.PointerPlacement = Placement.Center;
```

### Issue: Labels too crowded

**Cause:** Too many major ticks

**Solution:**
```csharp
// Increase major difference
gauge.MajorDifference = 25;  // Fewer labels

// Reduce minor ticks
gauge.MinorTickCount = 2;  // Less clutter
```

### Issue: Ranges not visible

**Causes:**
- Range height too small
- Range colors too subtle
- Range placement wrong

**Solution:**
```csharp
range.Height = 12;  // Increase thickness
range.Color = Color.Red;  // High contrast
range.RangePlacement = RangePlacement.Inside;  // Verify placement
```

### Issue: Value not updating smoothly

**Cause:** Frequent updates without optimization

**Solution:**
```csharp
// Use data binding for high-frequency updates
gauge.DataSource = dataTable;
gauge.DisplayMember = "SensorValue";
gauge.DisplayRecordIndex = 0;

// Or check before updating
private void UpdateValue(float newValue)
{
    if (Math.Abs(gauge.Value - newValue) > 0.1f)  // Threshold
    {
        gauge.Value = newValue;
    }
}
```

### Issue: Vertical gauge upside down

**Note:** LinearGauge vertical orientation is bottom-to-top by design

**If you need top-to-bottom:**
- Invert value: `gauge.Value = MaximumValue - actualValue;`
- Or use custom renderer to flip scale

### Issue: Ticks misaligned with ranges

**Cause:** Mismatched scaling

**Solution:**
```csharp
// Ensure range boundaries align with tick marks
gauge.MajorDifference = 20;

// Range boundaries should match major ticks
range1.EndValue = 20;  // Aligns with major tick
range2.StartValue = 20;  // Aligns with major tick
range2.EndValue = 40;
```

## Performance Tips

1. **Use data binding** for high-frequency updates instead of direct Value property assignment
2. **Suspend layout** when configuring multiple properties:
   ```csharp
   gauge.SuspendLayout();
   // Configure properties
   gauge.ResumeLayout();
   ```
3. **Minimize range count** - Consolidate similar ranges
4. **Reduce minor tick count** for faster rendering
5. **Update only when needed** - Check if value actually changed before updating
