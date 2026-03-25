# Performance Optimization

## Overview

When working with large datasets or frequently updating data points, the Smith Chart provides performance optimization methods to improve rendering speed and responsiveness.

## BeginUpdate and EndUpdate Methods

The primary performance optimization technique is to suspend chart updates while adding or modifying multiple data points, then resume updates once all changes are complete.

### Methods

| Method | Description |
|--------|-------------|
| `BeginUpdate()` | Suspends chart painting and update operations |
| `EndUpdate()` | Resumes painting and triggers a single refresh |

### How It Works

1. **BeginUpdate()** stops the chart from repainting after each data point addition
2. Add all your data points while updates are suspended
3. **EndUpdate()** resumes painting and performs a single, efficient refresh

Without these methods, the chart repaints after every single point addition, causing:
- Slow performance with many points
- Visual flickering
- High CPU usage
- Poor user experience

## Basic Usage

### Adding Points Efficiently

**C# Example:**
```csharp
this.sfSmithChart.BeginUpdate();

// Add multiple points
LineSeries lineSeries = sfSmithChart.Series[0] as LineSeries;
Random random = new Random();
for (int i = 0; i < 100; i++)
{
    double val = random.Next(0, 5);
    double val1 = random.Next(-5, 5);
    lineSeries.Points.Add(val, val1);
}

this.sfSmithChart.EndUpdate();
```

**VB.NET Example:**
```vb
Me.sfSmithChart.BeginUpdate()

' Add multiple points
Dim lineSeries As LineSeries = TryCast(sfSmithChart.Series(0), LineSeries)
Dim random As Random = New Random()

For i As Integer = 0 To 99
    Dim val As Double = random.Next(0, 5)
    Dim val1 As Double = random.Next(-5, 5)
    lineSeries.Points.Add(val, val1)
Next

Me.sfSmithChart.EndUpdate()
```

### Result

- **Without BeginUpdate/EndUpdate:** 100 repaints (one per point) = slow, flickering
- **With BeginUpdate/EndUpdate:** 1 repaint (after all points added) = fast, smooth

## When to Use

Use BeginUpdate/EndUpdate when:
- Adding more than 10 data points at once
- Updating multiple series simultaneously
- Modifying series properties in batch
- Loading data from external sources (files, databases, APIs)
- Refreshing chart data periodically
- Performing bulk operations on chart data

## Common Patterns

### Pattern 1: Loading Data from Collection

```csharp
sfSmithChart.BeginUpdate();

LineSeries series = new LineSeries();
series.MarkerVisible = true;

foreach (var dataPoint in transmissionDataCollection)
{
    series.Points.Add(dataPoint.Resistance, dataPoint.Reactance);
}

sfSmithChart.Series.Add(series);
sfSmithChart.EndUpdate();
```

### Pattern 2: Real-Time Data Updates

```csharp
private void UpdateChartData(List<TransmissionData> newData)
{
    sfSmithChart.BeginUpdate();
    
    LineSeries series = sfSmithChart.Series[0] as LineSeries;
    series.Points.Clear();  // Clear existing data
    
    foreach (var point in newData)
    {
        series.Points.Add(point.Resistance, point.Reactance);
    }
    
    sfSmithChart.EndUpdate();
}
```

### Pattern 3: Multiple Series Update

```csharp
sfSmithChart.BeginUpdate();

// Update first series
LineSeries series1 = sfSmithChart.Series[0] as LineSeries;
series1.Points.Clear();
foreach (var point in dataset1)
{
    series1.Points.Add(point.R, point.X);
}

// Update second series
LineSeries series2 = sfSmithChart.Series[1] as LineSeries;
series2.Points.Clear();
foreach (var point in dataset2)
{
    series2.Points.Add(point.R, point.X);
}

sfSmithChart.EndUpdate();
```

### Pattern 4: Large Dataset Loading

```csharp
private void LoadLargeDataset(string filePath)
{
    sfSmithChart.BeginUpdate();
    
    try
    {
        LineSeries series = new LineSeries();
        series.MarkerVisible = false;  // Disable markers for performance
        
        var data = File.ReadAllLines(filePath);
        foreach (var line in data)
        {
            var values = line.Split(',');
            double r = double.Parse(values[0]);
            double x = double.Parse(values[1]);
            series.Points.Add(r, x);
        }
        
        sfSmithChart.Series.Add(series);
    }
    finally
    {
        sfSmithChart.EndUpdate();  // Always call in finally block
    }
}
```

### Pattern 5: Periodic Data Refresh

```csharp
private Timer refreshTimer;

private void InitializeTimer()
{
    refreshTimer = new Timer();
    refreshTimer.Interval = 1000;  // 1 second
    refreshTimer.Tick += RefreshTimer_Tick;
    refreshTimer.Start();
}

private void RefreshTimer_Tick(object sender, EventArgs e)
{
    sfSmithChart.BeginUpdate();
    
    LineSeries series = sfSmithChart.Series[0] as LineSeries;
    series.Points.Clear();
    
    var newData = GetLatestMeasurements();
    foreach (var point in newData)
    {
        series.Points.Add(point.Resistance, point.Reactance);
    }
    
    sfSmithChart.EndUpdate();
}
```

## Additional Performance Tips

### 1. Disable Markers for Large Datasets

Markers add rendering overhead. Disable them for datasets with 100+ points:

```csharp
series.MarkerVisible = false;
```

### 2. Use Appropriate Data Binding

For dynamic data, use DataSource binding instead of manual point addition:

```csharp
series.DataSource = observableCollection;
series.ResistanceMember = "Resistance";
series.ReactanceMember = "Reactance";
```

ObservableCollection automatically handles updates efficiently.

### 3. Simplify Visual Elements

When performance is critical:

```csharp
// Disable data labels
series.DataLabel.Visible = false;

// Disable tooltips if not needed
series.TooltipVisible = false;

// Hide minor gridlines
sfSmithChart.HorizontalAxis.MinorGridlinesVisible = false;
sfSmithChart.RadialAxis.MinorGridlinesVisible = false;
```

### 4. Batch Property Changes

```csharp
sfSmithChart.BeginUpdate();

series.Interior = Color.Blue;
series.StrokeWidth = 2;
series.DashStyle = DashStyle.Solid;
series.MarkerVisible = true;
series.MarkerType = MarkerType.Circle;

sfSmithChart.EndUpdate();
```

### 5. Optimize Data Sampling

For very large datasets (1000+ points), consider data sampling:

```csharp
private List<TransmissionData> SampleData(List<TransmissionData> fullData, int targetCount)
{
    if (fullData.Count <= targetCount)
        return fullData;
        
    int step = fullData.Count / targetCount;
    return fullData.Where((x, i) => i % step == 0).ToList();
}

// Usage
var sampledData = SampleData(largeDataset, 200);
series.DataSource = sampledData;
```

## Performance Comparison

### Without Optimization

```csharp
// SLOW: Repaints after each point
LineSeries series = new LineSeries();
for (int i = 0; i < 1000; i++)
{
    series.Points.Add(GetR(i), GetX(i));  // Chart repaints 1000 times!
}
sfSmithChart.Series.Add(series);
```

**Result:** Slow, flickering, high CPU usage

### With Optimization

```csharp
// FAST: Single repaint after all points added
sfSmithChart.BeginUpdate();

LineSeries series = new LineSeries();
for (int i = 0; i < 1000; i++)
{
    series.Points.Add(GetR(i), GetX(i));
}
sfSmithChart.Series.Add(series);

sfSmithChart.EndUpdate();
```

**Result:** Fast, smooth, efficient

## Best Practices

1. **Always Pair BeginUpdate/EndUpdate:** Use try-finally to ensure EndUpdate is always called

```csharp
sfSmithChart.BeginUpdate();
try
{
    // Add points
}
finally
{
    sfSmithChart.EndUpdate();
}
```

2. **Threshold:** Use BeginUpdate/EndUpdate for 10+ operations

3. **Nested Calls:** Avoid nesting BeginUpdate/EndUpdate pairs on the same chart

4. **UI Thread:** Call these methods from the UI thread only

5. **Exception Handling:** Place EndUpdate in finally block to ensure it's called even if errors occur

6. **Async Loading:** For very large datasets, consider async loading:

```csharp
private async Task LoadDataAsync()
{
    var data = await Task.Run(() => LoadLargeDataFromFile());
    
    sfSmithChart.BeginUpdate();
    LineSeries series = new LineSeries();
    foreach (var point in data)
    {
        series.Points.Add(point.R, point.X);
    }
    sfSmithChart.Series.Add(series);
    sfSmithChart.EndUpdate();
}
```

## Benchmarking Example

```csharp
private void BenchmarkPerformance()
{
    // Without optimization
    var sw1 = Stopwatch.StartNew();
    LineSeries series1 = new LineSeries();
    for (int i = 0; i < 500; i++)
    {
        series1.Points.Add(i * 0.01, i * 0.005);
    }
    sw1.Stop();
    Console.WriteLine($"Without optimization: {sw1.ElapsedMilliseconds}ms");
    
    // With optimization
    var sw2 = Stopwatch.StartNew();
    sfSmithChart.BeginUpdate();
    LineSeries series2 = new LineSeries();
    for (int i = 0; i < 500; i++)
    {
        series2.Points.Add(i * 0.01, i * 0.005);
    }
    sfSmithChart.EndUpdate();
    sw2.Stop();
    Console.WriteLine($"With optimization: {sw2.ElapsedMilliseconds}ms");
    
    // Typical result: 10-50x faster with BeginUpdate/EndUpdate
}
```

## Troubleshooting

### Chart Not Updating After EndUpdate

- Verify BeginUpdate was called first
- Ensure EndUpdate is on the same chart instance
- Check that UI thread is used for both calls

### Performance Still Slow

- Verify BeginUpdate/EndUpdate are actually wrapping the operations
- Disable markers and data labels for large datasets
- Consider data sampling
- Check for other bottlenecks (data loading, calculation)

### Visual Glitches

- Ensure EndUpdate is always called (use finally block)
- Avoid partial updates between BeginUpdate/EndUpdate
- Call Refresh() if needed after EndUpdate
