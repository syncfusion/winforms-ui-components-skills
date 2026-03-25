# RadialGauge

## Table of Contents
- [Overview](#overview)
- [Getting Started](#getting-started)
- [Frame Types](#frame-types)
- [Scales and Labels](#scales-and-labels)
- [Ticks Configuration](#ticks-configuration)
- [Needles](#needles)
- [Multiple Needles](#multiple-needles)
- [Ranges](#ranges)
- [Scaling Divisions](#scaling-divisions)
- [Label Customization](#label-customization)
- [Frame Appearance](#frame-appearance)
- [Performance Optimization](#performance-optimization)
- [Common Scenarios](#common-scenarios)

## Overview

RadialGauge visualizes numeric values on a **circular or arc scale** with customizable needles, ranges, and labels. Ideal for speedometers, temperature gauges, pressure indicators, and dashboard displays.

**Key capabilities:**
- 4 frame types (FullCircle, HalfCircle, QuarterCircle, Fill)
- Single or multiple needles
- Color-coded value ranges
- Customizable start angle and sweep
- Gradient frame backgrounds
- Label rotation and placement
- Professional themes

## Getting Started

### Designer Setup

1. **Add RadialGauge to form:**
   - Drag from Toolbox → Place on form
   - Set Size (e.g., 300x300 for FullCircle)

2. **Configure basic properties (Properties window):**
   ```
   MinimumValue: 0
   MaximumValue: 200
   Value: 120
   MajorDifference: 20
   MinorDifference: 5
   FrameType: HalfCircle
   ShowNeedle: True
   GaugeLabel: "Speed (MPH)"
   ```

3. **Run** - Gauge displays with configured settings

### Code Setup

```csharp
using Syncfusion.Windows.Forms.Gauge;

// Create gauge
RadialGauge speedometer = new RadialGauge();
speedometer.Size = new Size(300, 300);
speedometer.Location = new Point(20, 20);

// Basic configuration
speedometer.MinimumValue = 0;
speedometer.MaximumValue = 200;
speedometer.Value = 120;
speedometer.MajorDifference = 20;
speedometer.MinorDifference = 5;
speedometer.FrameType = FrameType.HalfCircle;
speedometer.ShowNeedle = true;
speedometer.GaugeLabel = "Speed (MPH)";

// Add to form
this.Controls.Add(speedometer);
```

## Frame Types

### FullCircle

Complete 360° circular gauge (default).

```csharp
RadialGauge gauge = new RadialGauge();
gauge.FrameType = FrameType.FullCircle;
gauge.StartAngle = 135;  // Start position (degrees)
gauge.SweepAngle = 270;  // Arc length (degrees)
```

**Use when:**
- Need complete circular representation
- Compass or direction indicators
- Full-range dial controls
- Clock faces

**Default angles:**
- StartAngle: 135° (bottom-left)
- SweepAngle: 270° (3/4 circle)

### HalfCircle

Semi-circular gauge (180° arc).

```csharp
RadialGauge gauge = new RadialGauge();
gauge.FrameType = FrameType.HalfCircle;
gauge.StartAngle = 180;
gauge.SweepAngle = 180;
```

**Use when:**
- Speedometer-style displays
- Space-constrained dashboards
- Horizontal gauge layouts
- Temperature indicators

### QuarterCircle

Quarter-circle gauge (90° arc).

```csharp
RadialGauge gauge = new RadialGauge();
gauge.FrameType = FrameType.QuarterCircle;
gauge.StartAngle = 180;
gauge.SweepAngle = 90;
```

**Use when:**
- Compact gauge displays
- Corner-positioned indicators
- Minimal space available
- Small dashboard tiles

### Fill (Color Fill)

Gauge where color fills from start to current value.

```csharp
RadialGauge gauge = new RadialGauge();
gauge.FrameType = FrameType.Fill;
gauge.ArcThickness = 15;  // Fill width
gauge.ValueIndicatorColor = Color.Blue;  // Fill color
```

**Use when:**
- Progress indicators
- Percentage displays
- Visual completion status
- Modern UI designs

**Configuration:**
- `ArcThickness` controls fill width
- `ValueIndicatorColor` sets fill color
- No needle required (visual fill is indicator)

## Scales and Labels

### Scale Configuration

```csharp
// Show/hide scale labels
gauge.ShowScaleLabel = true;

// Label placement relative to ticks
gauge.LabelPlacement = LabelPlacement.Inside;  // Inside or Outside

// Text orientation
gauge.TextOrientation = TextOrientation.SlideOver;  // SlideOver or Horizontal

// Scale color
gauge.ScaleLabelColor = Color.Black;
```

**LabelPlacement options:**
- `Inside` - Labels inside the arc (closer to center)
- `Outside` - Labels outside the arc (farther from center)

**TextOrientation options:**
- `SlideOver` - Labels follow arc curve (radial)
- `Horizontal` - Labels always horizontal (easier reading)

### Scale Appearance

```csharp
// Font customization
gauge.Font = new Font("Segoe UI", 10, FontStyle.Bold);

// Label offset from ticks
gauge.LabelOffset = 10;  // Distance in pixels

// Show/hide tick labels
gauge.ShowTicks = true;
gauge.ShowScaleLabel = true;
```

## Ticks Configuration

### Major Ticks

Primary scale marks.

```csharp
gauge.MajorDifference = 20;  // Interval between major ticks
gauge.MajorTickMarkHeight = 10;  // Length of tick mark
gauge.MajorTickMarkColor = Color.Black;
gauge.MajorTickMarkWidth = 2;  // Thickness
```

### Minor Ticks

Secondary scale marks between majors.

```csharp
gauge.MinorDifference = 5;  // Interval between minor ticks
gauge.MinorTickMarkHeight = 5;
gauge.MinorTickMarkColor = Color.Gray;
gauge.MinorTickMarkWidth = 1;
```

### Tick Placement

```csharp
// Tick position relative to scale
gauge.MajorTickPlacement = TickPlacement.Inside;  // Inside, Outside, or Center
gauge.MinorTickPlacement = TickPlacement.Inside;
```

### Removing Ticks

```csharp
// Hide specific ticks
gauge.ShowTicks = false;  // Hides all ticks
gauge.MajorTickMarkHeight = 0;  // Hide major ticks only
gauge.MinorTickMarkHeight = 0;  // Hide minor ticks only
```

### Inter-Lines (Between Ticks)

Lines connecting ticks for visual continuity.

```csharp
gauge.InterLinesColor = Color.LightGray;
gauge.MajorInterLinesColor = Color.DarkGray;
gauge.MinorInterLinesColor = Color.LightGray;
```

## Needles

### Basic Needle Configuration

```csharp
// Show/hide needle
gauge.ShowNeedle = true;

// Needle color
gauge.NeedleColor = Color.Red;

// Needle style
gauge.NeedleStyle = NeedleStyle.Advanced;  // Default, Advanced, or Pointer
```

**NeedleStyle options:**
- `Default` - Simple triangle pointer
- `Advanced` - Stylized needle with detail
- `Pointer` - Arrow-style pointer

### Needle Appearance

```csharp
// Needle length relative to scale
gauge.NeedleLength = 0.7f;  // 70% of radius (0.0 to 1.0)

// Needle pivot settings
gauge.NeedlePivotRadius = 10;  // Center circle radius
gauge.NeedlePivotColor = Color.Black;
```

### Removing Needle

For fill-based or custom indicators:

```csharp
gauge.ShowNeedle = false;  // No needle displayed
```

## Multiple Needles

Display multiple values on the same gauge scale.

### Enabling Custom Needles

```csharp
// Enable multiple needles feature
gauge.EnableCustomNeedles = true;

// Hide default needle when using custom needles
gauge.ShowNeedle = false;
```

### Adding Needles

```csharp
using Syncfusion.Windows.Forms.Gauge;

// Create and configure needles
Needle needle1 = new Needle();
needle1.Value = 60;
needle1.Color = Color.Red;
needle1.NeedleStyle = NeedleStyle.Advanced;

Needle needle2 = new Needle();
needle2.Value = 80;
needle2.Color = Color.Blue;
needle2.NeedleStyle = NeedleStyle.Pointer;

Needle needle3 = new Needle();
needle3.Value = 40;
needle3.Color = Color.Green;
needle3.NeedleStyle = NeedleStyle.Default;

// Add to gauge
gauge.Needles.Add(needle1);
gauge.Needles.Add(needle2);
gauge.Needles.Add(needle3);
```

### Complete Multi-Needle Example

```csharp
// Create gauge for temperature comparison
RadialGauge tempGauge = new RadialGauge();
tempGauge.MinimumValue = 0;
tempGauge.MaximumValue = 100;
tempGauge.MajorDifference = 10;
tempGauge.MinorDifference = 2;
tempGauge.FrameType = FrameType.HalfCircle;
tempGauge.GaugeLabel = "Temperature (°C)";

// Enable multiple needles
tempGauge.EnableCustomNeedles = true;
tempGauge.ShowNeedle = false;

// Current temperature
Needle current = new Needle();
current.Value = 72;
current.Color = Color.Red;
current.NeedleStyle = NeedleStyle.Advanced;
tempGauge.Needles.Add(current);

// Target temperature
Needle target = new Needle();
target.Value = 68;
target.Color = Color.Blue;
target.NeedleStyle = NeedleStyle.Pointer;
tempGauge.Needles.Add(target);

// Average temperature
Needle average = new Needle();
average.Value = 70;
average.Color = Color.Green;
average.NeedleStyle = NeedleStyle.Default;
tempGauge.Needles.Add(average);

this.Controls.Add(tempGauge);
```

### Updating Needle Values

```csharp
// Update individual needle values
gauge.Needles[0].Value = 75;  // First needle
gauge.Needles[1].Value = 80;  // Second needle

// Refresh gauge
gauge.Refresh();
```

**Use cases for multiple needles:**
- Current vs. target values
- Multiple sensor readings
- Min/max/average comparisons
- Historical vs. current data

## Ranges

Color-coded value ranges for visual thresholds.

### Adding Single Range

```csharp
Range range = new Range();
range.StartValue = 0;
range.EndValue = 50;
range.Color = Color.Green;
range.Height = 10;
range.RangePlacement = RangePlacement.Inside;

gauge.Ranges.Add(range);
```

### Multiple Ranges (Traffic Light Pattern)

```csharp
// Green range (safe zone)
Range greenRange = new Range();
greenRange.StartValue = 0;
greenRange.EndValue = 60;
greenRange.Color = Color.Green;
greenRange.Height = 10;
gauge.Ranges.Add(greenRange);

// Yellow range (warning zone)
Range yellowRange = new Range();
yellowRange.StartValue = 60;
yellowRange.EndValue = 80;
yellowRange.Color = Color.Yellow;
yellowRange.Height = 10;
gauge.Ranges.Add(yellowRange);

// Red range (critical zone)
Range redRange = new Range();
redRange.StartValue = 80;
redRange.EndValue = 100;
redRange.Color = Color.Red;
redRange.Height = 10;
gauge.Ranges.Add(redRange);
```

### Range Properties

```csharp
Range range = new Range();

// Value boundaries
range.StartValue = 0;
range.EndValue = 50;

// Appearance
range.Color = Color.LightBlue;
range.Height = 15;  // Thickness of range arc
range.InnerStartOffset = 0;  // Inner radius offset
range.InnerEndOffset = 0;    // Inner radius offset
range.OuterStartOffset = 0;  // Outer radius offset
range.OuterEndOffset = 0;    // Outer radius offset

// Placement
range.RangePlacement = RangePlacement.Inside;  // Inside or Outside
```

### Range Placement

```csharp
// Inside the gauge (between scale and center)
range.RangePlacement = RangePlacement.Inside;

// Outside the gauge (beyond scale)
range.RangePlacement = RangePlacement.Outside;
```

## Scaling Divisions

### Major and Minor Differences

```csharp
// Value range
gauge.MinimumValue = 0;
gauge.MaximumValue = 200;

// Major ticks every 20 units (0, 20, 40, 60...)
gauge.MajorDifference = 20;

// Minor ticks every 5 units (5, 10, 15, 25...)
gauge.MinorDifference = 5;
```

**Calculation:**
- Major ticks: (MaximumValue - MinimumValue) / MajorDifference = count
- Minor ticks: MajorDifference / MinorDifference = count between majors

### Fine-Grained Scaling

```csharp
// Temperature gauge with precise divisions
gauge.MinimumValue = -20;
gauge.MaximumValue = 120;
gauge.MajorDifference = 10;  // Every 10 degrees
gauge.MinorDifference = 2;   // Every 2 degrees
```

### Angle Customization

```csharp
// Custom start and sweep angles
gauge.StartAngle = 120;   // Start from top-left
gauge.SweepAngle = 300;   // Almost full circle
```

**Common angle configurations:**

| Layout | StartAngle | SweepAngle | Visual |
|--------|------------|------------|--------|
| Traditional speedometer | 135 | 270 | Bottom half + sides |
| Top arc | 0 | 180 | Top half |
| Right arc | 270 | 180 | Right half |
| Full circle | 0 | 360 | Complete circle |

## Label Customization

### DrawLabel Event

Customize individual scale labels dynamically.

```csharp
gauge.DrawLabel += Gauge_DrawLabel;

private void Gauge_DrawLabel(object sender, DrawLabelEventArgs e)
{
    // Customize label based on value
    if (e.Value >= 80)
    {
        e.ForeColor = Color.Red;
        e.Font = new Font("Arial", 10, FontStyle.Bold);
    }
    else if (e.Value >= 60)
    {
        e.ForeColor = Color.Orange;
    }
    else
    {
        e.ForeColor = Color.Green;
    }

    // Modify label text
    e.Text = e.Value.ToString() + "°";
}
```

### DrawLabelEventArgs Properties

```csharp
private void Gauge_DrawLabel(object sender, DrawLabelEventArgs e)
{
    // Available properties:
    float value = e.Value;              // Numeric value of label
    string text = e.Text;               // Current label text
    Font font = e.Font;                 // Current font
    Color color = e.ForeColor;          // Current color
    LabelType type = e.LabelType;       // Label type
    StringAlignment align = e.LabelAlignment;  // Alignment
    int offset = e.Offset;              // Offset from scale

    // Modify properties
    e.Text = "$" + value.ToString("F2");
    e.ForeColor = Color.Blue;
    e.Font = new Font("Consolas", 9);
}
```

### Conditional Label Formatting

```csharp
private void Gauge_DrawLabel(object sender, DrawLabelEventArgs e)
{
    // Hide certain labels
    if (e.Value % 20 != 0)
    {
        e.Text = string.Empty;  // Hide non-major labels
    }

    // Custom formatting
    if (e.Value == gauge.MinimumValue)
        e.Text = "MIN";
    else if (e.Value == gauge.MaximumValue)
        e.Text = "MAX";
    else
        e.Text = e.Value.ToString("F1");
}
```

## Frame Appearance

### Frame Thickness

```csharp
// Thicker frame
gauge.FrameThickness = 20;  // Default is 10
```

### Gradient Background

```csharp
// Outer rim gradient
gauge.OuterFrameGradientStartColor = Color.LightBlue;
gauge.OuterFrameGradientEndColor = Color.DarkBlue;

// Inner frame gradient
gauge.InnerFrameGradientStartColor = Color.White;
gauge.InnerFrameGradientEndColor = Color.LightGray;
```

### Arc Colors

```csharp
// Arc thickness (for Fill frame type)
gauge.ArcThickness = 15;

// Value indicator color
gauge.ValueIndicatorColor = Color.Blue;

// Background arc color
gauge.GaugeArcColor = Color.LightGray;
```

### Frame Border

```csharp
// Outer frame appearance
gauge.OuterFrameGradientStartColor = Color.Silver;
gauge.OuterFrameGradientEndColor = Color.Gray;

// Inner frame appearance
gauge.InnerFrameGradientStartColor = Color.White;
gauge.InnerFrameGradientEndColor = Color.WhiteSmoke;
```

## Performance Optimization

### SuspendLayout/ResumeLayout Pattern

When configuring multiple properties or large value ranges:

```csharp
// Suspend layout updates
gauge.SuspendLayout();

try
{
    // Configure multiple properties
    gauge.MinimumValue = 0;
    gauge.MaximumValue = 1000;
    gauge.MajorDifference = 50;
    gauge.MinorDifference = 10;
    gauge.Value = 750;
    
    // Add multiple ranges
    for (int i = 0; i < 10; i++)
    {
        Range range = new Range();
        range.StartValue = i * 100;
        range.EndValue = (i + 1) * 100;
        range.Color = GetColorForRange(i);
        gauge.Ranges.Add(range);
    }
}
finally
{
    // Resume layout and refresh
    gauge.ResumeLayout();
}
```

**Why use this pattern:**
- Prevents multiple redraws during configuration
- Improves performance for complex gauges
- Essential when adding many ranges or needles
- Always use try/finally to ensure ResumeLayout is called

### High-Frequency Updates

For real-time data updates:

```csharp
// Update gauge value efficiently
private void UpdateGaugeValue(float newValue)
{
    if (gauge.Value != newValue)
    {
        gauge.Value = newValue;
        // Gauge auto-refreshes, no manual Refresh() needed
    }
}

// Timer for real-time updates
Timer updateTimer = new Timer();
updateTimer.Interval = 100;  // 10 updates per second
updateTimer.Tick += (s, e) => {
    UpdateGaugeValue(GetSensorReading());
};
```

## Common Scenarios

### Scenario 1: Speedometer

```csharp
RadialGauge speedometer = new RadialGauge();
speedometer.Size = new Size(350, 350);
speedometer.MinimumValue = 0;
speedometer.MaximumValue = 200;
speedometer.MajorDifference = 20;
speedometer.MinorDifference = 5;
speedometer.Value = 65;
speedometer.FrameType = FrameType.HalfCircle;
speedometer.GaugeLabel = "MPH";
speedometer.ShowNeedle = true;
speedometer.NeedleStyle = NeedleStyle.Advanced;
speedometer.NeedleColor = Color.Red;

// Speed ranges
speedometer.Ranges.Add(new Range { StartValue = 0, EndValue = 65, Color = Color.Green, Height = 10 });
speedometer.Ranges.Add(new Range { StartValue = 65, EndValue = 100, Color = Color.Yellow, Height = 10 });
speedometer.Ranges.Add(new Range { StartValue = 100, EndValue = 200, Color = Color.Red, Height = 10 });
```

### Scenario 2: Temperature Gauge with Custom Labels

```csharp
RadialGauge tempGauge = new RadialGauge();
tempGauge.MinimumValue = -20;
tempGauge.MaximumValue = 120;
tempGauge.MajorDifference = 20;
tempGauge.Value = 72;
tempGauge.FrameType = FrameType.FullCircle;
tempGauge.GaugeLabel = "Temperature";

// Custom label formatting
tempGauge.DrawLabel += (s, e) => {
    e.Text = e.Value + "°F";
    if (e.Value >= 90)
        e.ForeColor = Color.Red;
    else if (e.Value <= 32)
        e.ForeColor = Color.Blue;
};
```

### Scenario 3: Progress Indicator (Fill Type)

```csharp
RadialGauge progress = new RadialGauge();
progress.Size = new Size(200, 200);
progress.MinimumValue = 0;
progress.MaximumValue = 100;
progress.Value = 65;
progress.FrameType = FrameType.Fill;
progress.ArcThickness = 20;
progress.ValueIndicatorColor = Color.Blue;
progress.GaugeArcColor = Color.LightGray;
progress.ShowNeedle = false;
progress.GaugeLabel = "65%";
```

### Scenario 4: Industrial Pressure Gauge

```csharp
RadialGauge pressureGauge = new RadialGauge();
pressureGauge.MinimumValue = 0;
pressureGauge.MaximumValue = 200;
pressureGauge.MajorDifference = 25;
pressureGauge.MinorDifference = 5;
pressureGauge.Value = 85;
pressureGauge.FrameType = FrameType.FullCircle;
pressureGauge.StartAngle = 135;
pressureGauge.SweepAngle = 270;
pressureGauge.GaugeLabel = "PSI";

// Pressure ranges
pressureGauge.Ranges.Add(new Range { StartValue = 0, EndValue = 100, Color = Color.Green, Height = 12 });
pressureGauge.Ranges.Add(new Range { StartValue = 100, EndValue = 150, Color = Color.Yellow, Height = 12 });
pressureGauge.Ranges.Add(new Range { StartValue = 150, EndValue = 200, Color = Color.Red, Height = 12 });
```

## Edge Cases and Troubleshooting

### Issue: Needle not visible

**Causes:**
- `ShowNeedle = false`
- Needle length too short
- Needle color matches background

**Solution:**
```csharp
gauge.ShowNeedle = true;
gauge.NeedleLength = 0.7f;  // 70% of radius
gauge.NeedleColor = Color.Red;  // Contrasting color
```

### Issue: Labels overlapping

**Causes:**
- Too many major ticks
- Font too large
- Scale too small

**Solution:**
```csharp
// Increase major difference
gauge.MajorDifference = 20;  // Fewer labels

// Smaller font
gauge.Font = new Font("Arial", 8);

// Use SlideOver orientation
gauge.TextOrientation = TextOrientation.SlideOver;
```

### Issue: Ranges not visible

**Causes:**
- Range placement conflicts with scale
- Range height too small
- Range colors too subtle

**Solution:**
```csharp
range.RangePlacement = RangePlacement.Inside;
range.Height = 12;  // Increase thickness
range.Color = Color.Red;  // High contrast color
```

### Issue: Multiple needles not showing

**Cause:** `EnableCustomNeedles` not set

**Solution:**
```csharp
gauge.EnableCustomNeedles = true;
gauge.ShowNeedle = false;  // Hide default needle
gauge.Needles.Add(new Needle { Value = 60, Color = Color.Red });
```

### Issue: Performance degradation with animations

**Solution:** Reduce update frequency or use data binding

```csharp
// Instead of frequent manual updates
timer.Interval = 100;  // Increase interval

// Or use data binding (more efficient)
gauge.DataSource = dataTable;
gauge.DisplayMember = "Value";
```
