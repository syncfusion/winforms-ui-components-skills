# Series Configuration

Chart series is the visual representation of data on the Smith Chart. The LineSeries class provides properties for binding data and customizing appearance.

## Data Binding Properties

### Core Binding Properties

| Property | Type | Description |
|----------|------|-------------|
| `DataSource` | object | Data collection to bind (ObservableCollection, List, etc.) |
| `ResistanceMember` | string | Property name for resistance (impedance) or conductance (admittance) values |
| `ReactanceMember` | string | Property name for reactance (impedance) or susceptance (admittance) values |

### Basic Data Binding

**C# Example:**
```csharp
LineSeries series = new LineSeries();
series.DataSource = model.Trace1;
series.ResistanceMember = "Resistance";
series.ReactanceMember = "Reactance";
sfSmithChart1.Series.Add(series);
```

**VB.NET Example:**
```vb
Dim series As New LineSeries()
series.DataSource = model.Trace1
series.ResistanceMember = "Resistance"
series.ReactanceMember = "Reactance"
sfSmithChart1.Series.Add(series)
```

**Property Details:**
- **ResistanceMember** represents resistance values in impedance Smith Chart and conductance values in admittance Smith Chart
- **ReactanceMember** represents reactance values in impedance Smith Chart and susceptance values in admittance Smith Chart

## Customizing Line Appearance

Customize the line series using the following properties:

### Line Customization Properties

| Property | Type | Description |
|----------|------|-------------|
| `Interior` | Color | Line color |
| `StrokeWidth` | int | Line thickness in pixels |
| `DashStyle` | DashStyle | Line pattern (Solid, Dash, Dot, DashDot, etc.) |

### Line Style Example

**C# Example:**
```csharp
series.Interior = Color.Red;
series.StrokeWidth = 3;
series.DashStyle = DashStyle.Dash;
```

**VB.NET Example:**
```vb
series.Interior = Color.Red
series.StrokeWidth = 3
series.DashStyle = DashStyle.Dash
```

This creates a red dashed line with 3-pixel thickness.

## Series Visibility

Control whether a series is displayed using the `Visible` property. This is useful for temporarily hiding specific series without removing them from the collection.

**C# Example:**
```csharp
LineSeries series1 = new LineSeries();
series1.MarkerVisible = true;
series1.DataSource = model.Trace1;
series1.ResistanceMember = "Resistance";
series1.ReactanceMember = "Reactance";
sfSmithChart1.Series.Add(series1);

LineSeries series2 = new LineSeries();
series2.MarkerVisible = true;
series2.DataSource = model.Trace2;
series2.ResistanceMember = "Resistance";
series2.ReactanceMember = "Reactance";
series2.Visible = false;  // Hidden
sfSmithChart1.Series.Add(series2);
```

**VB.NET Example:**
```vb
Dim series1 As New LineSeries()
series1.MarkerVisible = True
series1.DataSource = model.Trace1
series1.ResistanceMember = "Resistance"
series1.ReactanceMember = "Reactance"
sfSmithChart1.Series.Add(series1)

Dim series2 As New LineSeries()
series2.MarkerVisible = True
series2.DataSource = model.Trace2
series2.ResistanceMember = "Resistance"
series2.ReactanceMember = "Reactance"
series2.Visible = False  ' Hidden
sfSmithChart1.Series.Add(series2)
```

In this example, `series1` is visible while `series2` is hidden.

## Data Plotting Customization

### ArrangeByIndex Property

By default, data points are plotted after being sorted by resistance values. Use the `ArrangeByIndex` property to plot data points based on their original index order instead.

**When to Use:**
- When the order of points in your data collection is significant
- When plotting time-series data where sequence matters
- When you want to preserve the original data order

**C# Example:**
```csharp
LineSeries series = new LineSeries();
series.ArrangeByIndex = true;
sfSmithChart1.Series.Add(series);
```

**VB.NET Example:**
```vb
Dim series As New LineSeries() 
series.ArrangeByIndex = True
sfSmithChart1.Series.Add(series)
```

**Default Behavior (ArrangeByIndex = false):**
- Points are sorted by resistance values before plotting
- Creates smoother curves when data is not pre-sorted

**With ArrangeByIndex = true:**
- Points are plotted in the order they appear in the data source
- Preserves original sequence regardless of values

## Multiple Series

You can add multiple series to a single Smith Chart for comparison:

**C# Example:**
```csharp
// First series
LineSeries series1 = new LineSeries();
series1.DataSource = model.Trace1;
series1.ResistanceMember = "Resistance";
series1.ReactanceMember = "Reactance";
series1.Interior = Color.Blue;
series1.LegendText = "Trace 1";
sfSmithChart1.Series.Add(series1);

// Second series
LineSeries series2 = new LineSeries();
series2.DataSource = model.Trace2;
series2.ResistanceMember = "Resistance";
series2.ReactanceMember = "Reactance";
series2.Interior = Color.Red;
series2.LegendText = "Trace 2";
sfSmithChart1.Series.Add(series2);
```

**VB.NET Example:**
```vb
' First series
Dim series1 As New LineSeries()
series1.DataSource = model.Trace1
series1.ResistanceMember = "Resistance"
series1.ReactanceMember = "Reactance"
series1.Interior = Color.Blue
series1.LegendText = "Trace 1"
sfSmithChart1.Series.Add(series1)

' Second series
Dim series2 As New LineSeries()
series2.DataSource = model.Trace2
series2.ResistanceMember = "Resistance"
series2.ReactanceMember = "Reactance"
series2.Interior = Color.Red
series2.LegendText = "Trace 2"
sfSmithChart1.Series.Add(series2)
```

## Common Patterns

### Pattern 1: Simple Series with Basic Styling

```csharp
LineSeries series = new LineSeries();
series.DataSource = transmissionData;
series.ResistanceMember = "Resistance";
series.ReactanceMember = "Reactance";
series.Interior = Color.DodgerBlue;
series.StrokeWidth = 2;
series.MarkerVisible = true;
sfSmithChart.Series.Add(series);
```

### Pattern 2: Multiple Series with Different Styles

```csharp
// Series 1 - Solid line
LineSeries measured = new LineSeries();
measured.DataSource = measuredData;
measured.ResistanceMember = "R";
measured.ReactanceMember = "X";
measured.Interior = Color.Green;
measured.LegendText = "Measured";
sfSmithChart.Series.Add(measured);

// Series 2 - Dashed line
LineSeries simulated = new LineSeries();
simulated.DataSource = simulatedData;
simulated.ResistanceMember = "R";
simulated.ReactanceMember = "X";
simulated.Interior = Color.Orange;
simulated.DashStyle = DashStyle.Dash;
simulated.LegendText = "Simulated";
sfSmithChart.Series.Add(simulated);
```

### Pattern 3: Dynamic Series Visibility Toggle

```csharp
// Toggle series visibility
private void ToggleSeries(int seriesIndex)
{
    if (seriesIndex < sfSmithChart.Series.Count)
    {
        var series = sfSmithChart.Series[seriesIndex] as LineSeries;
        series.Visible = !series.Visible;
    }
}
```

## Best Practices

1. **Property Name Matching:** Ensure `ResistanceMember` and `ReactanceMember` exactly match property names in your data model (case-sensitive)

2. **Data Source Types:** Use ObservableCollection<T> for data that changes dynamically to enable automatic updates

3. **Series Colors:** Use distinct colors for multiple series to ensure clear differentiation

4. **Legend Text:** Always set meaningful `LegendText` when using multiple series

5. **Performance:** For large datasets, use `ArrangeByIndex = false` (default) to avoid plotting sorted data unnecessarily
