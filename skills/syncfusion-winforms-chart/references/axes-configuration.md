# Axes Configuration

## Table of Contents
- [Overview](#overview)
- [Primary Axes](#primary-axes)
- [Axis Value Types](#axis-value-types)
- [Range and Intervals](#range-and-intervals)
- [Axis Labels](#axis-labels)
- [Axis Titles](#axis-titles)
- [Secondary Axes](#secondary-axes)

## Overview

ChartControl provides primary X and Y axes by default. Access via:
- `chartControl1.PrimaryXAxis` - Horizontal axis
- `chartControl1.PrimaryYAxis` - Vertical axis

## Primary Axes

```csharp
// Access primary axes
ChartAxis xAxis = chartControl1.PrimaryXAxis;
ChartAxis yAxis = chartControl1.PrimaryYAxis;

// Basic configuration
xAxis.ValueType = ChartValueType.Category;
yAxis.ValueType = ChartValueType.Double;
```

## Axis Value Types

### Double (Numeric)
Default type for continuous numeric data.

```csharp
chartControl1.PrimaryYAxis.ValueType = ChartValueType.Double;
```

### DateTime
For time-series data.

```csharp
chartControl1.PrimaryXAxis.ValueType = ChartValueType.DateTime;
chartControl1.PrimaryXAxis.DateTimeFormat = "MMM dd";
chartControl1.PrimaryXAxis.DateTimeInterval = ChartDateTimeIntervalType.Months;

// Add points with DateTime
series.Points.Add(new DateTime(2024, 1, 1), 50);
series.Points.Add(new DateTime(2024, 2, 1), 65);
```

### Category
For categorical labels (strings).

```csharp
chartControl1.PrimaryXAxis.ValueType = ChartValueType.Category;

// Use with CategoryAxisDataBindModel
CategoryAxisDataBindModel model = new CategoryAxisDataBindModel(data);
model.CategoryName = "Product";  // String property for categories
```

### Logarithmic
For exponential data scales.

```csharp
chartControl1.PrimaryYAxis.ValueType = ChartValueType.Logarithmic;
chartControl1.PrimaryYAxis.LogBase = 10;  // Log base
```

### Custom
For custom label implementation.

```csharp
chartControl1.PrimaryXAxis.ValueType = ChartValueType.Custom;
chartControl1.PrimaryXAxis.LabelsImpl = customLabelModel;
```

## Range and Intervals

### Auto Range (Default)
Chart automatically calculates min/max from data.

```csharp
xAxis.RangeType = ChartAxisRangeType.Auto;
```

### Manual Range
Explicit min/max values.

```csharp
yAxis.RangeType = ChartAxisRangeType.Set;
yAxis.Range = new Range(0, 100);  // Min=0, Max=100

// Alternative
yAxis.ValueRange = new Range(0, 100);
```

### Intervals
Control spacing between axis labels.

```csharp
// Auto interval calculation
yAxis.RangeType = ChartAxisRangeType.Auto;

// Manual interval
yAxis.Interval = 10;  // Label every 10 units

// DateTime intervals
xAxis.DateTimeInterval = ChartDateTimeIntervalType.Days;
xAxis.DateTimeIntervalValue = 7;  // Every 7 days
```

### Nice Intervals
Round intervals to "nice" numbers.

```csharp
yAxis.RangeType = ChartAxisRangeType.Auto;
yAxis.RoundDesiredIntervals = true;  // Use rounded intervals (10, 20, 50, etc.)
```

## Axis Labels

### Label Formatting
```csharp
// Numeric format
yAxis.LabelNumberFormat = "N2";  // Two decimal places
yAxis.LabelPrefix = "$";         // Add prefix
yAxis.LabelSuffix = " USD";      // Add suffix

// DateTime format
xAxis.DateTimeFormat = "yyyy-MM-dd";

// Custom format via event
yAxis.Format += (sender, args) =>
{
    args.Label = $"${args.Value:F2}";
};
```

### Label Appearance
```csharp
xAxis.Font = new Font("Arial", 9);
xAxis.ForeColor = Color.Black;
xAxis.LabelRotateAngle = 45;  // Rotate labels
```

### Label Positioning
```csharp
xAxis.LabelAlignment = StringAlignment.Center;
xAxis.LabelsPlacement = ChartPlacement.Inside;  // Inside or Outside
xAxis.LabelsOffset = 5;  // Distance from axis
```

### Hide Labels
```csharp
xAxis.IsVisible = false;
```

### Custom Labels
```csharp
yAxis.Format += (sender, args) =>
{
    if (args.Value == 0) args.Label = "Zero";
    else if (args.Value < 0) args.Label = "Negative";
    else args.Label = "Positive";
};
```

## Axis Titles

```csharp
// Set title
chartControl1.PrimaryXAxis.Title = "Months";
chartControl1.PrimaryYAxis.Title = "Sales (USD)";

// Title appearance
chartControl1.PrimaryYAxis.TitleFont = new Font("Arial", 11, FontStyle.Bold);
chartControl1.PrimaryYAxis.TitleColor = Color.Navy;
```

## Secondary Axes

Create additional axes for series with different scales.

### Create Secondary Axis
```csharp
// Create secondary Y axis
ChartAxis secondaryY = new ChartAxis();
secondaryY.ValueType = ChartValueType.Double;
secondaryY.OpposedPosition = true;  // Right side
secondaryY.Title = "Temperature (°C)";

// Add to chart
chartControl1.Axes.Add(secondaryY);

// Bind series to secondary axis
temperatureSeries.YAxis = secondaryY;
```

### Multiple Axes Example
```csharp
// Primary Y axis for sales
chartControl1.PrimaryYAxis.Title = "Sales";
chartControl1.PrimaryYAxis.Range = new Range(0, 1000);

// Secondary Y axis for temperature
ChartAxis tempAxis = new ChartAxis();
tempAxis.OpposedPosition = true;
tempAxis.Title = "Temperature";
tempAxis.Range = new Range(0, 50);
chartControl1.Axes.Add(tempAxis);

// Bind series
salesSeries.YAxis = chartControl1.PrimaryYAxis;
temperatureSeries.YAxis = tempAxis;
```

## Axis Appearance

### Grid Lines
```csharp
// Major grid
xAxis.GridLineType.MajorGridLines = true;
xAxis.GridLineType.MajorGridLineStyle.DashStyle = DashStyle.Solid;
xAxis.GridLineType.MajorGridLineColor = Color.LightGray;

// Minor grid
xAxis.GridLineType.MinorGridLines = true;
xAxis.GridLineType.MinorGridLineStyle.DashStyle = DashStyle.Dot;
```

### Tick Marks
```csharp
xAxis.TickSize = new Size(5, 5);
xAxis.MinorTicksPerInterval = 4;
xAxis.TickColor = Color.Black;
```

### Axis Line
```csharp
xAxis.LineType.Width = 2;
xAxis.LineType.ForeColor = Color.Black;
xAxis.LineType.Visible = true;
```

### Hide Axis
```csharp
xAxis.Visible = false;
```

## Advanced Configuration

### Opposed Position
Place axis on opposite side (right for Y, top for X).

```csharp
yAxis.OpposedPosition = true;
```

### Axis Origin
Start axis at specific value.

```csharp
yAxis.Origin = 50;  // Axis starts at 50 instead of 0
```

### Inverted Axis
Reverse axis direction.

```csharp
yAxis.IsInversed = true;  // High values at bottom
```

### Multiple X/Y Axes
```csharp
ChartAxis additionalYAxis = new ChartAxis();
chartControl1.Axes.Add(additionalYAxis);

// Bind series
series.YAxis = additionalYAxis;
```

## Common Patterns

### Percentage Axis
```csharp
yAxis.LabelNumberFormat = "P0";  // Percentage format
yAxis.Range = new Range(0, 1);  // 0-100%
```

### Currency Axis
```csharp
yAxis.LabelNumberFormat = "C2";  // Currency format
yAxis.LabelPrefix = "$";
```

### Time Range with Custom Format
```csharp
xAxis.ValueType = ChartValueType.DateTime;
xAxis.DateTimeFormat = "MMM yyyy";
xAxis.DateTimeInterval = ChartDateTimeIntervalType.Months;
xAxis.LabelRotateAngle = 45;
```
