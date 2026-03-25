# Scale Label Settings in Windows Forms Bullet Graph

## Table of Contents
- [Overview](#overview)
- [What Are Scale Labels](#what-are-scale-labels)
- [Label Properties](#label-properties)
- [Customizing Label Appearance](#customizing-label-appearance)
- [Label Formatting](#label-formatting)
- [Label Position](#label-position)
- [Common Patterns](#common-patterns)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

Scale labels are the numeric text values displayed along the quantitative scale that help users interpret measure values. Labels typically correspond to major tick positions and display the numeric value at each tick mark.

**Key Features:**
- Display numeric values for major ticks
- Customizable font size, color, and offset
- Support for numeric formatting (currency, percentages, etc.)
- Configurable positioning (above or below ranges)

## What Are Scale Labels

Scale labels provide the **numeric context** needed to interpret the bullet graph. They answer the question: "What value does this position on the scale represent?"

**Characteristics:**
- Generated automatically based on scale configuration
- Appear at major tick positions
- Display values from `Minimum` to `Maximum` by `Interval`
- Can be styled and formatted independently

**Example:**
```
Scale: Minimum = 0, Maximum = 100, Interval = 20
Labels display: 0, 20, 40, 60, 80, 100
```

## Label Properties

### Core Label Properties

| Property | Type | Description |
|----------|------|-------------|
| `LabelFontSize` | double | Font size of label text |
| `LabelStroke` | Color | Color of label text |
| `LabelOffset` | double | Distance between labels and scale |
| `LabelFormat` | string | Format string for label values |
| `LabelPosition` | BulletGraphLabelsPosition | Position relative to ranges (Above/Below) |

### Label Generation

Labels are automatically generated based on:
- `Minimum`: Starting value
- `Maximum`: Ending value
- `Interval`: Spacing between labels

```csharp
BulletGraph bullet = new BulletGraph();
bullet.Minimum = 0;
bullet.Maximum = 10;
bullet.Interval = 2;
// Generates labels: 0, 2, 4, 6, 8, 10
```

## Customizing Label Appearance

### Font Size

Use `LabelFontSize` to control the size of label text:

```csharp
BulletGraph bullet = new BulletGraph();
bullet.LabelFontSize = 10;  // 10-point font
```

**Size Guidelines:**
- **8-10pt**: Compact displays, dense dashboards
- **10-12pt**: Standard readability
- **12-14pt**: Large displays, presentations
- **14pt+**: Extra large displays, accessibility

### Label Color (Stroke)

Use `LabelStroke` to set label color:

```csharp
BulletGraph bullet = new BulletGraph();
bullet.LabelStroke = Color.Black;  // Black text
```

**Color Selection:**
- Use high-contrast colors for readability
- Black or dark gray for professional appearance
- Consider colorblind-friendly palettes
- Can bind to range colors (see section below)

### Label Offset

Use `LabelOffset` to adjust spacing between labels and the scale:

```csharp
BulletGraph bullet = new BulletGraph();
bullet.LabelOffset = 5;  // 5 pixels from scale
```

**Offset Guidelines:**
- **Default**: Usually 3-5 pixels
- **Increase**: For cleaner separation or larger fonts
- **Decrease**: For compact layouts

### Complete Label Customization

```csharp
BulletGraph bullet = new BulletGraph();
bullet.Dock = DockStyle.Fill;
bullet.FeaturedMeasure = 5;
bullet.ComparativeMeasure = 7;

// Scale configuration
bullet.Minimum = 0;
bullet.Maximum = 10;
bullet.Interval = 2;
bullet.MinorTicksPerInterval = 3;

// Label customization
bullet.LabelOffset = 5;
bullet.LabelFontSize = 10;
bullet.LabelFormat = "#0 K";  // Format: "0 K", "2 K", "4 K"...
bullet.LabelStroke = Color.Red;

// Add ranges
bullet.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 4, RangeCaption = "Bad", RangeStroke = Color.Red });
bullet.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 7, RangeCaption = "Satisfactory", RangeStroke = Color.Yellow });
bullet.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 10, RangeCaption = "Good", RangeStroke = Color.Green });

this.Controls.Add(bullet);
```

## Label Formatting

### Format String

The `LabelFormat` property uses standard .NET numeric format strings to control how values are displayed.

**Common Format Patterns:**

| Format | Example Input | Example Output | Use Case |
|--------|---------------|----------------|----------|
| `"#0"` | 1000 | "1000" | Integer values |
| `"#,##0"` | 1000 | "1,000" | Thousands separator |
| `"#0.0"` | 1234.5 | "1234.5" | One decimal place |
| `"#0.00"` | 1234.5 | "1234.50" | Two decimal places |
| `"$#,##0"` | 1000 | "$1,000" | Currency |
| `"$#,##0.00"` | 1000 | "$1,000.00" | Currency with cents |
| `"#0%"` | 0.75 | "75%" | Percentage |
| `"#0 K"` | 5 | "5 K" | Custom suffix |
| `"0.0E+0"` | 1000 | "1.0E+3" | Scientific notation |

### Basic Formatting Examples

#### Integer Display

```csharp
BulletGraph bullet = new BulletGraph();
bullet.Minimum = 0;
bullet.Maximum = 100;
bullet.Interval = 20;
bullet.LabelFormat = "#0";  // Display as integers: 0, 20, 40, 60, 80, 100
```

#### Currency Display

```csharp
BulletGraph revenue = new BulletGraph();
revenue.Minimum = 0;
revenue.Maximum = 1000;
revenue.Interval = 200;
revenue.LabelFormat = "$#,##0";  // Display: $0, $200, $400, $600, $800, $1,000
```

#### Percentage Display

```csharp
BulletGraph performance = new BulletGraph();
performance.Minimum = 0;
performance.Maximum = 1;  // 0 to 1 scale
performance.Interval = 0.2;
performance.LabelFormat = "#0%";  // Display: 0%, 20%, 40%, 60%, 80%, 100%
```

#### Custom Suffix Display

```csharp
BulletGraph thousands = new BulletGraph();
thousands.Minimum = 0;
thousands.Maximum = 100;
thousands.Interval = 20;
thousands.LabelFormat = "#0 K";  // Display: 0 K, 20 K, 40 K, 60 K, 80 K, 100 K
```

### Advanced Formatting

#### Thousands with Decimals

```csharp
BulletGraph precise = new BulletGraph();
precise.Minimum = 0;
precise.Maximum = 5000;
precise.Interval = 1000;
precise.LabelFormat = "#,##0.0";  // Display: 0.0, 1,000.0, 2,000.0...
```

#### Revenue in Thousands

```csharp
BulletGraph revenueGraph = new BulletGraph();
revenueGraph.Minimum = 0;
revenueGraph.Maximum = 400;
revenueGraph.Interval = 100;
revenueGraph.LabelFormat = "$#0 K";  // Display: $0 K, $100 K, $200 K, $300 K, $400 K
revenueGraph.Caption = "Revenue YTD\n$ in thousands";
```

## Label Position

The `LabelPosition` property controls whether labels appear above or below the qualitative ranges.

### Available Positions

- **Below** (Default): Labels appear below the ranges
- **Above**: Labels appear above the ranges

### Below Position (Default)

```csharp
BulletGraph bullet = new BulletGraph();
bullet.Dock = DockStyle.Fill;
bullet.FeaturedMeasure = 5;
bullet.ComparativeMeasure = 7;

// Labels below ranges (default)
bullet.LabelPosition = BulletGraphLabelsPosition.Below;

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

### Above Position

```csharp
BulletGraph bullet = new BulletGraph();
bullet.Dock = DockStyle.Fill;
bullet.FeaturedMeasure = 5;
bullet.ComparativeMeasure = 7;

// Labels above ranges
bullet.LabelPosition = BulletGraphLabelsPosition.Above;

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

### Position Selection Guidelines

**Use Below (default) when:**
- Standard layout preference
- Consistent with other components
- More space available below

**Use Above when:**
- Caption is positioned below
- Better visual balance needed
- Space constraints below the graph

## Common Patterns

### Financial Dashboard Labels

```csharp
BulletGraph financial = new BulletGraph();
financial.Minimum = 0;
financial.Maximum = 1000000;
financial.Interval = 200000;
financial.LabelFormat = "$#,##0K";  // $0K, $200K, $400K, $600K, $800K, $1,000K
financial.LabelFontSize = 11;
financial.LabelStroke = Color.Black;
financial.Caption = "Annual Revenue";
```

### Percentage-Based KPI

```csharp
BulletGraph kpi = new BulletGraph();
kpi.Minimum = 0;
kpi.Maximum = 100;
kpi.Interval = 25;
kpi.LabelFormat = "#0%";  // 0%, 25%, 50%, 75%, 100%
kpi.LabelFontSize = 10;
kpi.LabelStroke = Color.DarkBlue;
```

### Color-Coded Labels

Bind label colors to range colors for coordinated appearance:

```csharp
BulletGraph colorCoded = new BulletGraph();
colorCoded.Minimum = 0;
colorCoded.Maximum = 100;
colorCoded.Interval = 20;

// Add colored ranges
colorCoded.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 40, RangeStroke = Color.Red });
colorCoded.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 70, RangeStroke = Color.Yellow });
colorCoded.QualitativeRanges.Add(new QualitativeRange() 
    { RangeEnd = 100, RangeStroke = Color.Green });

// Bind label colors to range colors
colorCoded.BindRangeStrokeToLabels = true;

// Labels will be:
// 0, 20 = Red (in red range)
// 40 = Yellow (at yellow range start)
// 60 = Yellow (in yellow range)
// 80, 100 = Green (in green range)
```

### Large Display with Readable Labels

```csharp
BulletGraph presentation = new BulletGraph();
presentation.Minimum = 0;
presentation.Maximum = 500;
presentation.Interval = 100;
presentation.LabelFormat = "#,##0";
presentation.LabelFontSize = 14;  // Larger for presentations
presentation.LabelStroke = Color.Black;
presentation.LabelOffset = 8;  // More spacing
```

### Compact Dashboard

```csharp
BulletGraph compact = new BulletGraph();
compact.Minimum = 0;
compact.Maximum = 100;
compact.Interval = 50;  // Fewer labels
compact.LabelFormat = "#0";
compact.LabelFontSize = 8;  // Smaller font
compact.LabelStroke = Color.DarkGray;
compact.LabelOffset = 3;  // Tighter spacing
```

## Best Practices

### Font Size Selection

Choose font size based on display context:

```csharp
// Compact dashboard (multiple graphs)
bullet.LabelFontSize = 8;

// Standard desktop application
bullet.LabelFontSize = 10;

// Presentation or large screen
bullet.LabelFontSize = 14;
```

### Format Consistency

Maintain consistent formatting across related graphs:

```csharp
string currencyFormat = "$#,##0";

BulletGraph revenue = new BulletGraph();
revenue.LabelFormat = currencyFormat;

BulletGraph profit = new BulletGraph();
profit.LabelFormat = currencyFormat;

BulletGraph expenses = new BulletGraph();
expenses.LabelFormat = currencyFormat;
```

### Readable Intervals

Choose intervals that produce clean, readable labels:

```csharp
// Good: Round numbers
bullet.Interval = 10;   // 0, 10, 20, 30...
bullet.Interval = 25;   // 0, 25, 50, 75, 100
bullet.Interval = 100;  // 0, 100, 200, 300...

// Avoid: Awkward numbers
bullet.Interval = 17;   // 0, 17, 34, 51, 68...
bullet.Interval = 33;   // 0, 33, 66, 99...
```

### Color Contrast

Ensure labels are readable against background:

```csharp
// Good contrast
bullet.LabelStroke = Color.Black;  // On light background

// Poor contrast
bullet.LabelStroke = Color.LightGray;  // On light background - hard to read
```

### Label Positioning

Consider caption position when setting label position:

```csharp
// Caption at bottom, labels at top
bullet.Caption = "Revenue";
bullet.CaptionPosition = BulletGraphCaptionPosition.Near;  // Bottom in horizontal
bullet.LabelPosition = BulletGraphLabelsPosition.Above;     // Labels at top

// Caption at top, labels at bottom (default)
bullet.Caption = "Revenue";
bullet.CaptionPosition = BulletGraphCaptionPosition.Far;    // Top in horizontal
bullet.LabelPosition = BulletGraphLabelsPosition.Below;     // Labels at bottom
```

## Troubleshooting

### Issue: Labels Not Visible

**Problem**: Labels don't appear on the graph.

**Solution**: Check font size and color:
```csharp
// Ensure visible settings
bullet.LabelFontSize = 10;  // Not 0 or extremely small
bullet.LabelStroke = Color.Black;  // Not transparent or matching background
```

### Issue: Labels Overlapping

**Problem**: Labels overlap each other, making them unreadable.

**Solution**: Increase interval or decrease font size:
```csharp
// Option 1: Fewer labels
bullet.Interval = 50;  // Instead of 10

// Option 2: Smaller font
bullet.LabelFontSize = 8;  // Instead of 12
```

### Issue: Format Not Applied

**Problem**: `LabelFormat` property doesn't affect label display.

**Solution**: Ensure format string is valid:
```csharp
// Correct format strings
bullet.LabelFormat = "$#,##0";  // Valid ✓
bullet.LabelFormat = "#0%";     // Valid ✓
bullet.LabelFormat = "#0 K";    // Valid ✓

// Invalid format (won't work)
bullet.LabelFormat = "dollars";  // Invalid ✗
```

### Issue: Labels Too Far from Scale

**Problem**: Labels appear too far from the scale, looking disconnected.

**Solution**: Reduce label offset:
```csharp
bullet.LabelOffset = 3;  // Closer to scale
```

### Issue: Labels Too Close to Scale

**Problem**: Labels overlap with ticks or ranges.

**Solution**: Increase label offset:
```csharp
bullet.LabelOffset = 8;  // More spacing
```

### Issue: Currency Symbol Not Showing

**Problem**: Dollar signs or other currency symbols don't appear.

**Solution**: Escape special characters properly or use correct format:
```csharp
// Correct
bullet.LabelFormat = "$#,##0";  // Dollar sign included in format string

// If still not working, ensure culture settings support the currency symbol
```

### Issue: Percentage Display Incorrect

**Problem**: Percentages show as decimal or incorrect values.

**Solution**: Ensure scale values match percentage format:
```csharp
// For 0-100 display
bullet.Minimum = 0;
bullet.Maximum = 100;
bullet.LabelFormat = "#0%";  // Shows 0% to 100% correctly

// Or for 0-1 scale
bullet.Minimum = 0;
bullet.Maximum = 1;
bullet.Interval = 0.2;
bullet.LabelFormat = "0%";  // Shows 0% to 100% (multiplies by 100)
```