# Runtime Features

## Table of Contents
- [Zooming](#zooming)
- [Panning](#panning)
- [Hit Testing](#hit-testing)
- [Dynamic Updates](#dynamic-updates)

## Zooming

Enable interactive zooming to explore data.

```csharp
// Enable zooming
chartControl1.EnableXZooming = true;
chartControl1.EnableYZooming = true;

// Zoom toolbar
chartControl1.ShowToolbar = true;
```

### Programmatic Zoom
```csharp
// Zoom to specific range
chartControl1.PrimaryXAxis.ZoomFactor = 0.5;  // 50% zoom
chartControl1.PrimaryXAxis.ZoomPosition = 0.25;  // Start at 25%

// Reset zoom
chartControl1.ResetOnDoubleClick = true;
```

## Panning

Scroll through zoomed chart.

```csharp
chartControl1.MouseAction = ChartMouseAction.Panning;  // Click and drag to pan
```

## Hit Testing

Detect chart elements under mouse cursor.

```csharp
chartControl1.ChartRegionMouseMove += (sender, e) =>
{
    int seriesIndex = e.Region.SeriesIndex;
    int pointIndex = e.Region.PointIndex;
    ChartPoint point = chartControl1.Series[seriesIndex].Points[pointIndex];

    Console.WriteLine($"Point: {point.YValues[0]}");
};
```

### Click Events
```csharp
chartControl1.ChartRegionClick += (sender, e) =>
{
    if (e.Region.IsChartPoint)
    {
        ChartSeries series = chartControl1.Series[e.Region.SeriesIndex];
        ChartPoint point = series.Points[e.Region.PointIndex];
        
        MessageBox.Show($"Clicked: {point.YValues[0]}");
    }
};
```

## Dynamic Updates

### Add Points at Runtime
```csharp
series.Points.Add(DateTime.Now, GetCurrentValue());
chartControl1.Refresh();
```

### Modify Existing Points
```csharp
series.Points[0].YValues[0] = 150;
chartControl1.Refresh();
```

### Add/Remove Series
```csharp
// Add series
ChartSeries newSeries = new ChartSeries("New Data");
newSeries.Type = ChartSeriesType.Line;
newSeries.Points.Add(1, 50);
newSeries.Points.Add(2, 60);
chartControl1.Series.Add(newSeries);

// Remove series
chartControl1.Series.RemoveAt(0);
chartControl1.Refresh();
```

### Live Data Updates
```csharp
Timer timer = new Timer();
timer.Interval = 1000;  // 1 second
timer.Tick += (sender, e) =>
{
    double newValue = GetLatestValue();
    series.Points.Add(DateTime.Now, newValue);
    
    // Keep only last 50 points
    if (series.Points.Count > 50)
    {
        series.Points.RemoveAt(0);
    }
    
    chartControl1.Refresh();
};
timer.Start();
```

## Context Menu

Custom right-click menu:

```csharp
ContextMenuStrip menu = new ContextMenuStrip();
menu.Items.Add("Export", null, (s, e) => ExportChart());
menu.Items.Add("Reset Zoom", null, (s, e) => chartControl1.ResetOnDoubleClick = true);

chartControl1.ContextMenuStrip = menu;
```

## Interactive Selection

```csharp
chartControl1.ChartRegionClick += (sender, e) =>
{
    if (e.Region.IsChartPoint)
    {
        ChartSeries series = chartControl1.Series[e.Region.SeriesIndex];
        ChartPoint point = series.Points[e.Region.PointIndex];
        
        // Highlight selected point
        series.Style.Symbol.Shape = ChartSymbolShape.Star;
        series.Style.Symbol.Size = new Size(12, 12);
        series.Style.Symbol.Shape = ChartSymbolShape.Star;
    }
};
```
