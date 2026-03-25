# Chart Types

## Table of Contents
- [Overview](#overview)
- [Setting Chart Type](#setting-chart-type)
- [Type Requirements](#type-requirements)
- [Common Chart Types](#common-chart-types)
- [Financial Charts](#financial-charts)
- [Statistical Charts](#statistical-charts)
- [Specialized Charts](#specialized-charts)

## Overview

Essential Chart supports 35+ chart types set via `ChartSeries.Type` property. Each type has specific Y-value requirements and series limitations.

## Setting Chart Type

```csharp
ChartSeries series = new ChartSeries("Sales");
series.Type = ChartSeriesType.Column;  // Set chart type
chartControl1.Series.Add(series);
```

## Type Requirements

| Chart Type | Min Series | Max Series | Y Values Required |
|-----------|------------|------------|-------------------|
| Area | 1 | Unlimited | 1 |
| Bar | 1 | Unlimited | 1 |
| Bubble | 1 | Unlimited | 2 |
| Candle | 1 | Unlimited | 4 |
| Column | 1 | Unlimited | 1 |
| ColumnRange | 1 | Unlimited | 2 |
| Combination | 2 | Unlimited | 1 |
| Funnel | 1 | 1 | 1 |
| Gantt | 1 | Unlimited | 2 |
| HiLo | 1 | Unlimited | 2 |
| HiLoOpenClose | 1 | Unlimited | 4 |
| Histogram | 1 | Unlimited | 1 |
| Line | 1 | Unlimited | 1 |
| Pie | 1 | 1 | 1 |
| Pyramid | 1 | 1 | 1 |
| Scatter | 1 | Unlimited | 1 |
| Spline | 1 | Unlimited | 1 |
| StackingArea | 2 | Unlimited | 1 |
| StackingBar | 2 | Unlimited | 1 |
| StackingColumn | 2 | Unlimited | 1 |

## Common Chart Types

### Column Chart
Vertical bars for category comparison.

```csharp
ChartSeries series = new ChartSeries("Sales");
series.Type = ChartSeriesType.Column;
series.Points.Add(1, 100);
series.Points.Add(2, 150);
series.Points.Add(3, 120);
```

### Bar Chart
Horizontal bars for category comparison.

```csharp
series.Type = ChartSeriesType.Bar;
```

### Line Chart
Connected points showing trends over time.

```csharp
series.Type = ChartSeriesType.Line;
series.Points.Add(new DateTime(2024, 1, 1), 50);
series.Points.Add(new DateTime(2024, 2, 1), 65);
series.Points.Add(new DateTime(2024, 3, 1), 72);
```

### Area Chart
Filled area under line showing magnitude.

```csharp
series.Type = ChartSeriesType.Area;
```

### Pie Chart
Circular chart showing proportions (single series only).

```csharp
ChartSeries series = new ChartSeries("Market Share");
series.Type = ChartSeriesType.Pie;
series.Points.Add(0, 35);  // Segment 1
series.Points.Add(1, 25);  // Segment 2
series.Points.Add(2, 20);  // Segment 3
series.Points.Add(3, 20);  // Segment 4
```

### Spline Chart
Smooth curved line chart.

```csharp
series.Type = ChartSeriesType.Spline;
```

### Scatter Chart
Points without connecting lines for correlation analysis.

```csharp
series.Type = ChartSeriesType.Scatter;
```

## Financial Charts

### Candle Chart
Shows open, high, low, close prices (4 Y values).

```csharp
ChartSeries series = new ChartSeries("Stock");
series.Type = ChartSeriesType.Candle;

// Add points with 4 Y values: Open, High, Low, Close
series.Points.Add(1, new double[] { 100, 110, 95, 105 });
series.Points.Add(2, new double[] { 105, 115, 100, 112 });
series.Points.Add(3, new double[] { 112, 120, 108, 118 });
```

### HiLo Chart
Shows high and low values (2 Y values).

```csharp
series.Type = ChartSeriesType.HiLo;
series.Points.Add(1, new double[] { 110, 95 });  // High, Low
```

### HiLoOpenClose Chart
Shows open, high, low, close (4 Y values).

```csharp
series.Type = ChartSeriesType.HiLoOpenClose;
series.Points.Add(1, new double[] { 100, 110, 95, 105 });
```

## Statistical Charts

### Histogram
Frequency distribution chart.

```csharp
series.Type = ChartSeriesType.Histogram;
```

### Box and Whisker
Statistical distribution (5 Y values: Min, Q1, Median, Q3, Max).

```csharp
series.Type = ChartSeriesType.BoxAndWhisker;
series.Points.Add(1, new double[] { 10, 20, 25, 30, 40 });
```

### Bubble Chart
Three dimensions: X, Y, and bubble size (2 Y values).

```csharp
series.Type = ChartSeriesType.Bubble;
// Y[0] = Y value, Y[1] = Bubble size
series.Points.Add(1, new double[] { 50, 10 });
series.Points.Add(2, new double[] { 60, 15 });
series.Points.Add(3, new double[] { 55, 20 });
```

## Specialized Charts

### Stacking Charts
Multiple series stacked vertically (minimum 2 series).

```csharp
ChartSeries series1 = new ChartSeries("Product A");
series1.Type = ChartSeriesType.StackingColumn;
series1.Points.Add(1, 30);
series1.Points.Add(2, 40);

ChartSeries series2 = new ChartSeries("Product B");
series2.Type = ChartSeriesType.StackingColumn;
series2.Points.Add(1, 20);
series2.Points.Add(2, 30);

chartControl1.Series.Add(series1);
chartControl1.Series.Add(series2);
```

**Stacking types:**
- `StackingColumn`
- `StackingBar`
- `StackingArea`
- `StackingColumn100` (percentage)
- `StackingBar100`
- `StackingArea100`

### Combination Chart
Different chart types in same chart (minimum 2 series).

```csharp
ChartSeries columnSeries = new ChartSeries("Sales");
columnSeries.Type = ChartSeriesType.Column;
columnSeries.Points.Add(1, 100);
columnSeries.Points.Add(2, 150);

ChartSeries lineSeries = new ChartSeries("Target");
lineSeries.Type = ChartSeriesType.Line;
lineSeries.Points.Add(1, 120);
lineSeries.Points.Add(2, 140);

chartControl1.Series.Add(columnSeries);
chartControl1.Series.Add(lineSeries);
```

### Funnel/Pyramid
Single series showing hierarchical data.

```csharp
series.Type = ChartSeriesType.Funnel;
// or
series.Type = ChartSeriesType.Pyramid;
```

### Gantt Chart
Project timeline chart (2 Y values: Start, End).

```csharp
series.Type = ChartSeriesType.Gantt;
series.Points.Add(1, new double[] { 0, 5 });   // Start at 0, end at 5
series.Points.Add(2, new double[] { 3, 8 });   // Start at 3, end at 8
```

### Polar/Radar Charts
Circular coordinate system.

```csharp
series.Type = ChartSeriesType.Polar;
// or
series.Type = ChartSeriesType.Radar;
```

### Range Charts
Show value ranges (2 Y values: Low, High).

```csharp
series.Type = ChartSeriesType.ColumnRange;
series.Points.Add(1, new double[] { 20, 35 });  // Low, High
series.Points.Add(2, new double[] { 25, 40 });
```

## Type-Specific Configuration

Each chart type may have specific style properties:

```csharp
// Pie/Funnel specific
series.ConfigItems.PieItem.LabelStyle = ChartAccumulationLabelStyle.Outside;
series.ConfigItems.PieItem.ExplodeIndex = 2;  // Explode 3rd slice

// Candle/OHLC specific
series.Style.Interior = new BrushInfo(GradientStyle.Vertical, Color.Green, Color.Red);
```

## Changing Type at Runtime

```csharp
// Change series type dynamically
chartControl1.Series[0].Type = ChartSeriesType.Spline;
chartControl1.Refresh();
```
