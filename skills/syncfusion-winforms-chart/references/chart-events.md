# Chart Events

## Table of Contents
- [Series Events](#series-events)
- [Chart Events](#chart-events)
- [Interaction Events](#interaction-events)

## Series Events

### PrepareStyle Event
Customize appearance for each data point during rendering.

```csharp
series.PrepareStyle += (sender, args) =>
{
    ChartSeries s = sender as ChartSeries;
    ChartPoint point = s.Points[args.Index];
    
    // Conditional formatting
    if (point.YValues[0] > 100)
    {
        args.Style.Interior = new BrushInfo(Color.Green);
    }
    else
    {
        args.Style.Interior = new BrushInfo(Color.Red);
    }
    
    // Custom tooltip
    args.Style.ToolTip = $"Value: {point.YValues[0]:N2}";
    
    // Custom label
    args.Style.Text = $"${point.YValues[0]:N0}";
};
```

## Chart Events

### ChartFormatAxisLabel
Customize axis label formatting.

```csharp
chartControl1.ChartFormatAxisLabel += (sender, e) =>
{
    if (e.AxisOrientation == ChartOrientation.Horizontal)
    {
        e.Label = e.Label.ToUpper();
    }
    else if (e.AxisOrientation == ChartOrientation.Vertical)
    {
        e.Label = $"${e.Value:N0}";
    }
    
    e.Handled = true;
};
```

### LegendItemsChanged
Modify legend items after creation.

```csharp
chartControl1.Legend.FilterItems += (sender, e) =>
{
    foreach (ChartLegendItem item in chartControl1.Legend.Items)
    {
        item.Text = item.Text.ToUpper();
    }
};
```

## Interaction Events

### ChartRegionClick
Handle clicks on chart regions.

```csharp
chartControl1.ChartRegionClick += (sender, e) =>
{
    if (e.Region.IsChartPoint)
    {
        int seriesIndex = e.Region.SeriesIndex;
        int pointIndex = e.Region.PointIndex;
        ChartSeries series = chartControl1.Series[seriesIndex];
        ChartPoint point = series.Points[pointIndex];

        MessageBox.Show($"Series: {series.Name}\nValue: {point.YValues[0]}");
    }
};
```

### Mouse Events
```csharp
chartControl1.MouseMove += (sender, e) =>
{
    var chartPoint = chartControl1.ChartArea.GetValueByPoint(new Point(e.X, e.Y));

    // chartPoint.X is the X data; chartPoint.YValues[0] is the primary Y
    toolTip1.SetToolTip(chartControl1, $"X={chartPoint.X}, Y={chartPoint.YValues[0]}");
};
```

### ChartRegionMouseDown/Up
```csharp
chartControl1.ChartRegionMouseDown += (sender, e) =>
{
    if (e.Region.IsChartPoint)
    {
        // Start drag operation
    }
};

chartControl1.ChartRegionMouseUp += (sender, e) =>
{
    if (e.Region.IsChartPoint)
    {
        // End drag operation
    }
};
```

## Common Event Patterns

### Highlight on Hover
```csharp
private int highlightedIndex = -1;

series.PrepareStyle += (sender, args) =>
{
    if (args.Index == highlightedIndex)
    {
        args.Style.Interior = new BrushInfo(Color.Yellow);
    }
};

chartControl1.ChartRegionMouseMove += (sender, e) =>
{
    if (e.Region != null && e.Region.SeriesIndex >= 0 && e.Region.PointIndex >= 0)
    {
        // Hovering a data point
        highlightedIndex = e.Region.PointIndex;
    }
    else
    {
        highlightedIndex = -1;
    }

    chartControl1.Refresh();
};
```

### Toggle Series Visibility
```csharp
chartControl1.Legend.MouseClick += (sender, e) =>
{
    // Convert screen position → legend coordinates
    Point p = chartControl1.Legend.PointToClient(Control.MousePosition);

    // Get the legend item at the clicked location
    ChartLegendItem item = chartControl1.Legend.GetItemBy(p);
    if (item == null)
        return;

    // Legend item text is typically the series name
    string seriesName = item.Text;

    ChartSeries series = chartControl1.Series[seriesName];
    if (series == null)
        return;

    // Toggle visibility
    series.Visible = !series.Visible;

    chartControl1.Refresh();

};
```
