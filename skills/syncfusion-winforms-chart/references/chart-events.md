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
chartControl1.LegendItemsChanged += (sender, e) =>
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
    else if (e.Region.IsLegend)
    {
        MessageBox.Show("Legend clicked");
    }
    else if (e.Region.IsAxis)
    {
        MessageBox.Show("Axis clicked");
    }
};
```

### Mouse Events
```csharp
chartControl1.MouseMove += (sender, e) =>
{
    ChartRegion region = chartControl1.CalcHitTestInfo(e.Location);
    
    if (region.IsChartPoint)
    {
        // Highlight on hover
        Cursor = Cursors.Hand;
    }
    else
    {
        Cursor = Cursors.Default;
    }
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

chartControl1.MouseMove += (sender, e) =>
{
    ChartRegion region = chartControl1.CalcHitTestInfo(e.Location);
    
    if (region.IsChartPoint)
    {
        highlightedIndex = region.PointIndex;
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
chartControl1.ChartRegionClick += (sender, e) =>
{
    if (e.Region.IsLegend && e.Region.LegendItem != null)
    {
        string seriesName = e.Region.LegendItem.Text;
        ChartSeries series = chartControl1.Series[seriesName];
        
        series.Visible = !series.Visible;
        chartControl1.Refresh();
    }
};
```
