# Sparkline Types in Windows Forms

The Syncfusion Windows Forms Sparkline control supports three distinct types of sparklines, each suited for different data visualization needs. All sparkline types require data binding and support various data sources.

## Overview

The three sparkline types available are:

1. **Line** - Connects data points with a continuous line
2. **Column** - Represents each data point as a vertical bar
3. **WinLoss** - Similar to column but with equal-length bars

**Key Requirement:** All sparkline types must be bound to a data source before rendering.

**Supported Data Sources:**
- `double[]` arrays
- `DataTable`
- Any type implementing `IEnumerable`
- Any type implementing `ICollection`
- Any type implementing `IList`

## Line Sparkline

### Overview

The Line sparkline type connects a series of data points with a continuous line, creating a smooth visual representation of trends over time or sequence.

**Visual Characteristics:**
- Data points connected by a line
- Smooth curve representation
- Clear visualization of trends and patterns
- Positive values above baseline, negative below

**When to Use Line Sparklines:**
- Displaying continuous data trends
- Showing smooth transitions between values
- Visualizing time-series data
- Emphasizing overall pattern rather than individual values
- Dashboard KPI trends
- Stock price movements
- Temperature or weather data
- Performance metrics over time

### Implementation

```csharp
// Set sparkline data source
this.sparkLine1.Source = new double[] { 30, -20, 80, 20, 40, -50, -30, 70, -40, 50 };

// Set type to Line
this.sparkLine1.Type = SparkLineType.Line;
```

```vb
' VB.NET implementation
Me.sparkLine1.Source = New Double() {30, -20, 80, 20, 40, -50, -30, 70, -40, 50}
Me.sparkLine1.Type = SparkLineType.Line
```

**Result:** A continuous line graph showing the data trend from first to last point.

### Line Sparkline with Customization

```csharp
// Create line sparkline with custom styling
this.sparkLine1.Source = new double[] { 10, 30, 25, 40, 35, 50, 45, 60 };
this.sparkLine1.Type = SparkLineType.Line;

// Customize line appearance
this.sparkLine1.LineStyle.LineColor = Color.DarkBlue;

// Add markers to show data points
this.sparkLine1.Markers.ShowMarker = true;
this.sparkLine1.Markers.MarkerColor = new BrushInfo(Color.Blue);
```

## Column Sparkline

### Overview

The Column sparkline type represents each data point as a vertical column (bar). The height of each column corresponds to the data value, and the column direction indicates positive or negative values.

**Visual Characteristics:**
- Each data point is a discrete vertical bar
- Column height represents value magnitude
- Columns above baseline for positive values
- Columns below baseline for negative values
- Clear visualization of individual value magnitudes

**When to Use Column Sparklines:**
- Emphasizing individual data point values
- Comparing discrete measurements
- Showing magnitude differences clearly
- Visualizing positive vs. negative values
- Sales by month or period
- Profit/loss by category
- Survey responses or ratings
- Count data or frequencies

### Implementation

```csharp
// Set sparkline data source
this.sparkLine1.Source = new double[] { 30, -20, 80, 20, 40, -50, -30, 70, -40, 50 };

// Set type to Column
this.sparkLine1.Type = SparkLineType.Column;
```

```vb
' VB.NET implementation
Me.sparkLine1.Source = New Double() {30, -20, 80, 20, 40, -50, -30, 70, -40, 50}
Me.sparkLine1.Type = SparkLineType.Column
```

**Result:** Each value appears as a vertical column, with positive values extending upward and negative values extending downward from the baseline.

### Column Direction Behavior

**Positive Values:**
- Columns extend upward from the baseline
- Visual indication of positive measurements

**Negative Values:**
- Columns extend downward from the baseline
- Visual indication of negative measurements or losses

**Example with mixed values:**
```csharp
// Mix of positive and negative values
this.sparkLine1.Source = new double[] { 50, -30, 70, 40, -20, 60, -10, 80 };
this.sparkLine1.Type = SparkLineType.Column;

// Highlight negative values
this.sparkLine1.Markers.ShowNegativePoint = true;
this.sparkLine1.Markers.NegativePointColor = new BrushInfo(Color.Red);
```

## WinLoss Sparkline

### Overview

The WinLoss sparkline type is a specialized variant of the column sparkline where all columns have equal length regardless of the actual data value magnitude. It's designed specifically for binary win/loss or pass/fail scenarios.

**Visual Characteristics:**
- Each data point represented by equal-length column
- Only direction (up/down) matters, not magnitude
- All positive values: same height upward
- All negative values: same height downward
- Simplified visualization focusing on outcomes rather than magnitude

**When to Use WinLoss Sparklines:**
- Binary outcome visualization (win/loss, pass/fail, yes/no)
- Success/failure patterns
- Above/below threshold indicators
- Game results or match outcomes
- Test pass/fail results
- Quality control accept/reject data
- Up/down stock day indicators
- Project milestone completion (done/not done)

### Implementation

```csharp
// Set sparkline data source
this.sparkLine1.Source = new double[] { 30, -20, 80, 20, 40, -50, -30, 70, -40, 50 };

// Set type to WinLoss
this.sparkLine1.Type = SparkLineType.WinLoss;
```

```vb
' VB.NET implementation
Me.sparkLine1.Source = New Double() {30, -20, 80, 20, 40, -50, -30, 70, -40, 50}
Me.sparkLine1.Type = SparkLineType.WinLoss
```

**Result:** All positive values appear as equal-height upward columns, all negative values as equal-height downward columns. The actual magnitude (30 vs 80) doesn't affect column height.

### WinLoss with Binary Data

For true binary scenarios, use 1 and -1:

```csharp
// Binary win/loss data (1 = win, -1 = loss)
this.sparkLine1.Source = new double[] { 1, -1, 1, 1, -1, 1, -1, -1, 1, 1 };
this.sparkLine1.Type = SparkLineType.WinLoss;

// Highlight negative (loss) points in red
this.sparkLine1.Markers.ShowNegativePoint = true;
this.sparkLine1.Markers.NegativePointColor = new BrushInfo(Color.Red);
```

**Pattern Recognition:** This visualization makes it easy to spot winning/losing streaks and overall patterns.

## Type Comparison and Selection Guide

### When to Choose Line

**Choose Line when:**
- Data represents continuous measurements
- Trend visualization is more important than individual values
- You want to show smooth transitions
- Data points are closely related in sequence

**Example scenarios:** Temperature over time, stock prices, website traffic, performance metrics

### When to Choose Column

**Choose Column when:**
- Individual value magnitudes are important
- You need to compare discrete measurements
- Positive/negative distinction is crucial
- Data points are independent measurements

**Example scenarios:** Monthly sales, quarterly profits, survey ratings, category comparisons

### When to Choose WinLoss

**Choose WinLoss when:**
- Only outcome matters, not magnitude
- Data is binary or categorical
- You need to visualize patterns of success/failure
- Simplicity and quick recognition are priorities

**Example scenarios:** Game results, test outcomes, threshold indicators, milestone completion

## Data Source Binding Examples

### Using DataTable

```csharp
// Create and populate DataTable
DataTable sparklineData = new DataTable();
sparklineData.Columns.Add("Value", typeof(double));

// Add rows
sparklineData.Rows.Add(30);
sparklineData.Rows.Add(-20);
sparklineData.Rows.Add(80);
sparklineData.Rows.Add(20);
sparklineData.Rows.Add(40);

// Bind to sparkline
this.sparkLine1.Source = sparklineData;
this.sparkLine1.Type = SparkLineType.Column;
```

### Using List<double>

```csharp
// Use generic list
List<double> values = new List<double> { 30, -20, 80, 20, 40, -50, -30, 70, -40, 50 };

// Bind to sparkline
this.sparkLine1.Source = values;
this.sparkLine1.Type = SparkLineType.Line;
```

### Using IEnumerable

```csharp
// Use LINQ query result (IEnumerable)
var salesData = from order in orders
                select order.TotalAmount;

this.sparkLine1.Source = salesData;
this.sparkLine1.Type = SparkLineType.Column;
```

## Switching Between Types

You can dynamically change sparkline types at runtime:

```csharp
// Toggle button to switch types
private void btnSwitchType_Click(object sender, EventArgs e)
{
    if (sparkLine1.Type == SparkLineType.Line)
    {
        sparkLine1.Type = SparkLineType.Column;
    }
    else if (sparkLine1.Type == SparkLineType.Column)
    {
        sparkLine1.Type = SparkLineType.WinLoss;
    }
    else
    {
        sparkLine1.Type = SparkLineType.Line;
    }
}
```

## Complete Type Comparison Example

```csharp
// Create three sparklines showing same data in different types
double[] data = { 30, -20, 80, 20, 40, -50, -30, 70, -40, 50 };

// Line sparkline
SparkLine sparkLine1 = new SparkLine();
sparkLine1.Source = data;
sparkLine1.Type = SparkLineType.Line;
sparkLine1.Location = new Point(20, 20);
sparkLine1.Size = new Size(200, 40);

// Column sparkline
SparkLine sparkLine2 = new SparkLine();
sparkLine2.Source = data;
sparkLine2.Type = SparkLineType.Column;
sparkLine2.Location = new Point(20, 70);
sparkLine2.Size = new Size(200, 40);

// WinLoss sparkline
SparkLine sparkLine3 = new SparkLine();
sparkLine3.Source = data;
sparkLine3.Type = SparkLineType.WinLoss;
sparkLine3.Location = new Point(20, 120);
sparkLine3.Size = new Size(200, 40);

// Add all to form for comparison
this.Controls.Add(sparkLine1);
this.Controls.Add(sparkLine2);
this.Controls.Add(sparkLine3);
```

This example creates three sparklines with identical data but different types, allowing users to see the visualization differences side by side.
