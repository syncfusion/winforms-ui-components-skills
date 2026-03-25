# Scale Tick Mark Settings in Windows Forms Bullet Graph

## Table of Contents
- [Overview](#overview)
- [Tick Types](#tick-types)
- [Scale Configuration](#scale-configuration)
- [Major Ticks](#major-ticks)
- [Minor Ticks](#minor-ticks)
- [Tick Position](#tick-position)
- [Common Patterns](#common-patterns)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

The quantitative scale in a Bullet Graph displays two types of tick marks that help users read and interpret values:

1. **Major Ticks**: Primary scale indicators showing main intervals
2. **Minor Ticks**: Secondary scale indicators falling between major ticks

Together with scale labels, ticks provide the measurement framework for interpreting the featured and comparative measures.

## Tick Types

### Major Ticks

Major ticks are the **primary scale indicators** that mark significant intervals on the scale.

**Characteristics:**
- Larger and more prominent than minor ticks
- Appear at regular intervals defined by the `Interval` property
- Typically have corresponding scale labels
- Default appearance usually sufficient for most use cases

**Use Cases:**
- Mark round numbers (0, 10, 20, 30...)
- Indicate key thresholds
- Provide primary reference points for reading values

### Minor Ticks

Minor ticks are **secondary scale indicators** that provide finer granularity between major ticks.

**Characteristics:**
- Smaller and less prominent than major ticks
- Number per interval controlled by `MinorTicksPerInterval` property
- No labels (usually)
- Help with precise value reading

**Use Cases:**
- Enable more precise value reading
- Add visual polish
- Improve scale readability at higher zoom levels

## Scale Configuration

### Setting Scale Range

Define the scale's range with `Minimum` and `Maximum` properties:

```csharp
BulletGraph bullet = new BulletGraph();
bullet.Minimum = 0;      // Scale starts at 0
bullet.Maximum = 100;    // Scale ends at 100
```

### Setting Interval

The `Interval` property defines spacing between major ticks:

```csharp
bullet.Minimum = 0;
bullet.Maximum = 100;
bullet.Interval = 20;    // Major ticks at 0, 20, 40, 60, 80, 100
```

**Calculation:**
Number of major ticks = `(Maximum - Minimum) / Interval + 1`

Example: `(100 - 0) / 20 + 1 = 6` major ticks

### Setting Minor Ticks Per Interval

The `MinorTicksPerInterval` property defines how many minor ticks appear between major ticks:

```csharp
bullet.Interval = 20;
bullet.MinorTicksPerInterval = 3;  // 3 minor ticks between each major tick
```

**Result**: With interval 20 and 3 minor ticks, you get minor ticks at:
- Between 0 and 20: 5, 10, 15
- Between 20 and 40: 25, 30, 35
- And so on...

## Major Ticks

### Major Tick Properties

| Property | Type | Description |
|----------|------|-------------|
| `MajorTickStroke` | Color | Color of major tick marks |
| `MajorTickSize` | int | Length of major tick marks |
| `MajorTickStrokeThickness` | int | Width/thickness of major tick lines |

### Basic Major Tick Configuration

```csharp
BulletGraph bullet = new BulletGraph();
bullet.Dock = DockStyle.Fill;

// Configure scale
bullet.Minimum = 0;
bullet.Maximum = 10;
bullet.Interval = 2;  // Major ticks at 0, 2, 4, 6, 8, 10

// Style major ticks
bullet.MajorTickStroke = Color.Black;
bullet.MajorTickSize = 15;
bullet.MajorTickStrokeThickness = 2;

// Add ranges
bullet.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 3, RangeStroke = Color.LightGray });
bullet.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 7, RangeStroke = Color.Gray });
bullet.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 10, RangeStroke = Color.DarkGray });

this.Controls.Add(bullet);
```

### Customizing Major Ticks

#### Change Color

```csharp
bullet.MajorTickStroke = Color.Red;  // Red major ticks
```

#### Adjust Size

```csharp
bullet.MajorTickSize = 20;  // Longer tick marks
```

#### Change Thickness

```csharp
bullet.MajorTickStrokeThickness = 3;  // Thicker tick lines
```

### Complete Major Tick Example

```csharp
BulletGraph bullet = new BulletGraph();
bullet.Dock = DockStyle.Fill;
bullet.FeaturedMeasure = 4.5;
bullet.ComparativeMeasure = 7;

// Scale configuration
bullet.Minimum = 0;
bullet.Maximum = 10;
bullet.Interval = 2;

// Major tick styling
bullet.MajorTickStroke = Color.Black;
bullet.MajorTickSize = 15;
bullet.MajorTickStrokeThickness = 2;

// Add ranges
bullet.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 4, RangeCaption = "Bad", RangeStroke = Color.Red });
bullet.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 7, RangeCaption = "Satisfactory", RangeStroke = Color.Yellow });
bullet.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 10, RangeCaption = "Good", RangeStroke = Color.Green });

this.Controls.Add(bullet);
```

## Minor Ticks

### Minor Tick Properties

| Property | Type | Description |
|----------|------|-------------|
| `MinorTickStroke` | Color | Color of minor tick marks |
| `MinorTickSize` | int | Length of minor tick marks |
| `MinorTickStrokeThickness` | int | Width/thickness of minor tick lines |
| `MinorTicksPerInterval` | int | Number of minor ticks between major ticks |

### Basic Minor Tick Configuration

```csharp
BulletGraph bullet = new BulletGraph();
bullet.Dock = DockStyle.Fill;

// Configure scale
bullet.Minimum = 0;
bullet.Maximum = 10;
bullet.Interval = 2;
bullet.MinorTicksPerInterval = 3;  // 3 minor ticks between major ticks

// Style minor ticks
bullet.MinorTickStroke = Color.Gray;
bullet.MinorTickSize = 10;
bullet.MinorTickStrokeThickness = 1;

this.Controls.Add(bullet);
```

### Customizing Minor Ticks

```csharp
BulletGraph bullet = new BulletGraph();
bullet.Dock = DockStyle.Fill;
bullet.FeaturedMeasure = 4.5;
bullet.ComparativeMeasure = 7;

// Scale
bullet.Minimum = 0;
bullet.Maximum = 10;
bullet.Interval = 2;
bullet.MinorTicksPerInterval = 3;

// Major ticks
bullet.MajorTickStroke = Color.Black;
bullet.MajorTickSize = 15;

// Minor ticks - styled differently
bullet.MinorTickStroke = Color.Gray;
bullet.MinorTickSize = 10;         // Smaller than major ticks
bullet.MinorTickStrokeThickness = 1;  // Thinner than major ticks

this.Controls.Add(bullet);
```

### Complete Tick Customization Example

```csharp
BulletGraph bullet = new BulletGraph();
bullet.Dock = DockStyle.Fill;
bullet.FeaturedMeasure = 4.5;
bullet.ComparativeMeasure = 7;

// Scale configuration
bullet.Minimum = 0;
bullet.Maximum = 10;
bullet.Interval = 2;
bullet.MinorTicksPerInterval = 3;

// Major tick styling
bullet.MajorTickStroke = Color.Black;
bullet.MajorTickSize = 15;
bullet.MajorTickStrokeThickness = 2;

// Minor tick styling  
bullet.MinorTickStroke = Color.Green;
bullet.MinorTickSize = 10;
bullet.MinorTickStrokeThickness = 1;

// Add ranges
bullet.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 4, RangeCaption = "Bad", RangeStroke = Color.Red });
bullet.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 7, RangeCaption = "Satisfactory", RangeStroke = Color.Yellow });
bullet.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 10, RangeCaption = "Good", RangeStroke = Color.Green });

this.Controls.Add(bullet);
```

## Tick Position

The `TickPosition` property controls where ticks appear relative to the qualitative ranges.

### Available Positions

- **Below** (Default): Ticks appear below/outside the ranges
- **Above**: Ticks appear above/inside the ranges  
- **Cross**: Ticks span across the ranges

### Below Position (Default)

```csharp
BulletGraph bullet = new BulletGraph();
bullet.TickPosition = BulletGraphTicksPosition.Below;  // Default
bullet.MajorTickSize = 15;
bullet.MinorTickSize = 10;
```

Ticks appear on the outer edge of the scale, below the qualitative ranges.

### Above Position

```csharp
BulletGraph bullet = new BulletGraph();
bullet.TickPosition = BulletGraphTicksPosition.Above;
bullet.MajorTickSize = 15;
bullet.MinorTickSize = 10;
```

Ticks appear on the inner edge, above the qualitative ranges.

### Cross Position

```csharp
BulletGraph bullet = new BulletGraph();
bullet.Dock = DockStyle.Fill;
bullet.ComparativeMeasure = 7;

// Cross position - ticks span the full height
bullet.TickPosition = BulletGraphTicksPosition.Cross;
bullet.MajorTickSize = 30;  // Needs to be larger to span ranges
bullet.MinorTickSize = 30;

bullet.MinorTicksPerInterval = 3;

// Add ranges
bullet.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 4, RangeCaption = "Bad", RangeStroke = Color.Red });
bullet.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 7, RangeCaption = "Satisfactory", RangeStroke = Color.Yellow });
bullet.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 10, RangeCaption = "Good", RangeStroke = Color.Green });

this.Controls.Add(bullet);
```

**Note**: When using `Cross` position, increase tick size to ensure they span the full range height/width.

### Position Comparison Example

```csharp
// Create three bullet graphs with different tick positions
BulletGraph[] bullets = new BulletGraph[3];
BulletGraphTicksPosition[] positions = 
{ 
    BulletGraphTicksPosition.Below, 
    BulletGraphTicksPosition.Above, 
    BulletGraphTicksPosition.Cross 
};

for (int i = 0; i < 3; i++)
{
    bullets[i] = new BulletGraph();
    bullets[i].Dock = DockStyle.Top;
    bullets[i].Height = 100;
    
    bullets[i].Minimum = 0;
    bullets[i].Maximum = 10;
    bullets[i].Interval = 2;
    bullets[i].MinorTicksPerInterval = 3;
    bullets[i].FeaturedMeasure = 6;
    
    // Set position
    bullets[i].TickPosition = positions[i];
    bullets[i].MajorTickSize = (positions[i] == BulletGraphTicksPosition.Cross) ? 30 : 15;
    bullets[i].MinorTickSize = (positions[i] == BulletGraphTicksPosition.Cross) ? 30 : 10;
    
    bullets[i].Caption = positions[i].ToString();
    
    // Add ranges
    bullets[i].QualitativeRanges.Add(new QualitativeRange() 
        { RangeEnd = 4, RangeStroke = Color.LightGray });
    bullets[i].QualitativeRanges.Add(new QualitativeRange() 
        { RangeEnd = 7, RangeStroke = Color.Gray });
    bullets[i].QualitativeRanges.Add(new QualitativeRange() 
        { RangeEnd = 10, RangeStroke = Color.DarkGray });
    
    this.Controls.Add(bullets[i]);
}
```

## Common Patterns

### Standard Scale with Ticks

```csharp
BulletGraph standard = new BulletGraph();
standard.Minimum = 0;
standard.Maximum = 100;
standard.Interval = 20;  // Ticks at 0, 20, 40, 60, 80, 100
standard.MinorTicksPerInterval = 4;  // 4 minor ticks (every 5 units)

standard.MajorTickStroke = Color.Black;
standard.MajorTickSize = 12;
standard.MinorTickStroke = Color.DarkGray;
standard.MinorTickSize = 8;
```

### High-Precision Scale

```csharp
BulletGraph precision = new BulletGraph();
precision.Minimum = 0;
precision.Maximum = 10;
precision.Interval = 1;  // Major tick every 1 unit
precision.MinorTicksPerInterval = 9;  // Minor ticks every 0.1 units

precision.MajorTickSize = 15;
precision.MinorTickSize = 8;
```

### Minimal Scale (No Minor Ticks)

```csharp
BulletGraph minimal = new BulletGraph();
minimal.Minimum = 0;
minimal.Maximum = 100;
minimal.Interval = 25;
minimal.MinorTicksPerInterval = 0;  // No minor ticks

minimal.MajorTickStroke = Color.Black;
minimal.MajorTickSize = 15;
```

### Color-Coded Ticks

```csharp
BulletGraph colorCoded = new BulletGraph();
colorCoded.Minimum = 0;
colorCoded.Maximum = 100;
colorCoded.Interval = 20;
colorCoded.MinorTicksPerInterval = 3;

// Add colored ranges
colorCoded.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 40, RangeStroke = Color.FromArgb(255, 200, 200) });
colorCoded.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 70, RangeStroke = Color.FromArgb(255, 255, 200) });
colorCoded.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 100, RangeStroke = Color.FromArgb(200, 255, 200) });

// Bind tick colors to range colors
colorCoded.BindRangeStrokeToTicks = true;
```

## Best Practices

### Interval Selection

Choose intervals that make sense for your data:

```csharp
// For 0-100 scale, good intervals:
bullet.Interval = 10;  // Very readable: 0, 10, 20, 30...
bullet.Interval = 20;  // Clean: 0, 20, 40, 60...
bullet.Interval = 25;  // Works well: 0, 25, 50, 75, 100

// Avoid awkward intervals:
bullet.Interval = 17;  // Awkward: 0, 17, 34, 51, 68, 85...
```

### Minor Tick Count

Balance readability with clutter:

```csharp
// Good practices:
bullet.Interval = 10;
bullet.MinorTicksPerInterval = 4;  // Divides evenly: every 2 units

bullet.Interval = 20;
bullet.MinorTicksPerInterval = 3;  // Divides evenly: every 5 units

// Avoid:
bullet.Interval = 10;
bullet.MinorTicksPerInterval = 7;  // Creates odd spacing
```

### Tick Size Hierarchy

Maintain clear visual hierarchy:

```csharp
// Good hierarchy
bullet.MajorTickSize = 15;  // Prominent
bullet.MinorTickSize = 10;  // Less prominent (66% of major)

// Poor hierarchy
bullet.MajorTickSize = 12;  
bullet.MinorTickSize = 11;  // Too similar, hard to distinguish
```

### Color Selection

Use colors that ensure visibility:

```csharp
// Good contrast
bullet.MajorTickStroke = Color.Black;      // Strong contrast
bullet.MinorTickStroke = Color.DarkGray;   // Subtle but visible

// Poor contrast (on light background)
bullet.MajorTickStroke = Color.LightGray;  // Too faint
bullet.MinorTickStroke = Color.White;      // Invisible
```

### Tick Position Guidelines

- **Below**: Standard, clean appearance (default choice)
- **Above**: When you want ticks inside the range area
- **Cross**: For emphasis or when showing grid-like structure (increase tick size)

## Troubleshooting

### Issue: Ticks Not Visible

**Problem**: Tick marks don't appear on the scale.

**Solution**: Check tick size and stroke color:
```csharp
// Ensure visible size
bullet.MajorTickSize = 15;  // Not 0 or very small
bullet.MinorTickSize = 10;

// Ensure visible color
bullet.MajorTickStroke = Color.Black;  // Not transparent or same as background
bullet.MinorTickStroke = Color.Gray;
```

### Issue: Too Many Minor Ticks

**Problem**: Scale looks cluttered with minor ticks.

**Solution**: Reduce `MinorTicksPerInterval`:
```csharp
// Before (cluttered)
bullet.MinorTicksPerInterval = 9;

// After (cleaner)
bullet.MinorTicksPerInterval = 3;  // Or even 0 for no minor ticks
```

### Issue: Inconsistent Tick Intervals

**Problem**: Ticks don't appear at expected positions.

**Solution**: Ensure interval evenly divides the range:
```csharp
// Good
bullet.Minimum = 0;
bullet.Maximum = 100;
bullet.Interval = 20;  // 100/20 = 5 intervals ✓

// Problematic
bullet.Minimum = 0;
bullet.Maximum = 100;
bullet.Interval = 30;  // 100/30 = 3.33... intervals ✗
```

### Issue: Cross Position Ticks Too Short

**Problem**: Ticks don't span full range height with `Cross` position.

**Solution**: Increase tick size significantly:
```csharp
bullet.TickPosition = BulletGraphTicksPosition.Cross;
bullet.MajorTickSize = 40;  // Much larger for cross position
bullet.MinorTickSize = 40;
```

### Issue: Minor Ticks Same Size as Major

**Problem**: Can't distinguish between major and minor ticks.

**Solution**: Ensure clear size difference:
```csharp
bullet.MajorTickSize = 15;
bullet.MinorTickSize = 9;  // At least 30-40% smaller
```