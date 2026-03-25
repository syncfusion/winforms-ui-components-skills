# Troubleshooting

## Data Not Displaying

### Check Series Added to Chart
```csharp
// Ensure series is added
chartControl1.Series.Add(series);
```

### Verify Axis ValueType
```csharp
// For categorical data
chartControl1.PrimaryXAxis.ValueType = ChartValueType.Category;

// For numeric data
chartControl1.PrimaryXAxis.ValueType = ChartValueType.Double;

// For date data
chartControl1.PrimaryXAxis.ValueType = ChartValueType.DateTime;
```

### Check Data Binding
```csharp
// Ensure model is assigned
series.SeriesModelImpl = dataBindModel;  // Or
series.CategoryModel = categoryBindModel;

// Verify column names match
model.XName = "ActualColumnName";  // Must match exactly
model.YNames = new string[] { "ActualYColumn" };
```

### Verify Points Added
```csharp
Console.WriteLine($"Point count: {series.Points.Count}");
```

## Performance Issues

### Too Many Points
Limit points or use data sampling:
```csharp
if (series.Points.Count > 1000)
{
    // Sample or aggregate data
}
```

### Disable Visual Effects
```csharp
chartControl1.Shadows = false;
chartControl1.SmoothingMode = SmoothingMode.None;
```

## Axis Range Issues

### Data Out of Range
```csharp
// Check axis range
Console.WriteLine($"Y Min: {chartControl1.PrimaryYAxis.Range.Min}");
Console.WriteLine($"Y Max: {chartControl1.PrimaryYAxis.Range.Max}");

// Use auto range
chartControl1.PrimaryYAxis.RangeType = ChartAxisRangeType.Auto;
```

### Zero Range
```csharp
// Add margin to range
yAxis.RangeType = ChartAxisRangeType.Set;
yAxis.Range = new MinMaxInfo(minValue - 10, maxValue + 10);
```

## Design-Time Issues

### ChartWizard Not Opening
Ensure Syncfusion toolbox integration is complete. Reinstall if necessary.

### Properties Not Saving
Check form designer code-behind for proper initialization order.

## Common Errors

### "Index out of range"
```csharp
// Check point index before access
if (index >= 0 && index < series.Points.Count)
{
    ChartPoint point = series.Points[index];
}
```

### "Object reference not set"
```csharp
// Initialize before use
if (series != null && series.Points != null)
{
    series.Points.Add(1, 100);
}
```

## Refresh Not Working

Force refresh after changes:
```csharp
chartControl1.Refresh();

// Or invalidate specific region
chartControl1.Invalidate();
```

## Export Issues

### Empty or Black Image
Ensure chart is fully rendered before export:
```csharp
chartControl1.Refresh();
Application.DoEvents();  // Process pending events
chartControl1.SaveImage("chart.png", ChartImageFormat.Png);
```

## Legend Issues

### Series Not Showing in Legend
```csharp
// Verify legend is visible
chartControl1.Legend.Visible = true;

// Check series visibility
series.Visible = true;
series.ShowInLegend = true;
```
