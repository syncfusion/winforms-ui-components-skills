# Performance Optimization

## Large Datasets

### Use Data Binding
Avoid adding thousands of points individually.

```csharp
// SLOW
for (int i = 0; i < 10000; i++)
{
    series.Points.Add(i, values[i]);
}

// FAST
ChartDataBindModel model = new ChartDataBindModel(dataTable);
model.XName = "X";
model.YNames = new string[] { "Y" };
series.SeriesModelImpl = model;
```

### Custom Data Models
Implement `IChartSeriesModel` to query data on-demand without loading entire dataset.

```csharp
public class LargeDataModel : IChartSeriesModel
{
    private IDataSource dataSource;
    
    public int Count => dataSource.TotalRows;
    
    public ChartPoint GetPoint(int index)
    {
        // Query only requested point
        return dataSource.GetRow(index);
    }
}
```

## Rendering Optimization

### Suspend Layout During Updates
```csharp
chartControl1.SuspendLayout();

// Multiple operations
chartControl1.Series.Clear();
chartControl1.Series.Add(newSeries1);
chartControl1.Series.Add(newSeries2);
// ... more operations

chartControl1.ResumeLayout(true);
```

### Disable Anti-Aliasing for Speed
```csharp
chartControl1.SmoothingMode = System.Drawing.Drawing2D.SmoothingMode.None;
```

### Reduce Visual Effects
```csharp
// Disable shadows
chartControl1.Shadows = false;

// Disable gradients
chartControl1.UseGradientPalette = false;
series.Style.Interior = new BrushInfo(Color.Blue);  // Solid colors only
```

## Memory Management

### Clear Unused Series
```csharp
chartControl1.Series.Clear();
GC.Collect();  // Force garbage collection if needed
```

### Limit Point Count
```csharp
// Keep only recent data
if (series.Points.Count > 1000)
{
    series.Points.RemoveRange(0, 100);  // Remove oldest 100 points
}
```

### Dispose Resources
```csharp
if (series.SeriesModelImpl is IDisposable disposable)
{
    disposable.Dispose();
}
```

## Update Frequency

### Batch Updates
```csharp
// Collect data first
List<ChartPoint> newPoints = new List<ChartPoint>();
for (int i = 0; i < 10; i++)
{
    newPoints.Add(new ChartPoint(i, GetValue()));
}

// Add in batch
series.Points.AddRange(newPoints);
chartControl1.Refresh();
```

### Throttle Refresh
```csharp
private DateTime lastRefresh = DateTime.MinValue;

void AddDataPoint(double value)
{
    series.Points.Add(DateTime.Now, value);
    
    // Refresh at most once per 100ms
    if ((DateTime.Now - lastRefresh).TotalMilliseconds > 100)
    {
        chartControl1.Refresh();
        lastRefresh = DateTime.Now;
    }
}
```

## Best Practices

1. **Use appropriate chart types:** Line charts render faster than spline or area charts
2. **Limit series count:** Fewer series = faster rendering
3. **Aggregate data:** For large datasets, show aggregated/sampled data instead of all points
4. **Disable unused features:** Turn off tooltips, labels, legends if not needed
5. **Optimize data queries:** Use indexed database queries for data binding
