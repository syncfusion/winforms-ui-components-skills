# Chart Series

## Table of Contents
- [Creating Series](#creating-series)
- [ChartPoint Class](#chartpoint-class)
- [Series Properties](#series-properties)
- [Series Styling](#series-styling)
- [Multiple Series](#multiple-series)

## Creating Series

Two methods to create and add series:

### Method 1: Explicit Creation
```csharp
ChartSeries series = new ChartSeries("Sales", ChartSeriesType.Bar);
series.Points.Add(0, 200);
series.Points.Add(1, 300);
chartControl1.Series.Add(series);  // Remember to add to chart
```

### Method 2: Via Chart Model
```csharp
// Automatically adds to chart
ChartSeries series = chartControl1.Model.NewSeries("Sales", ChartSeriesType.Bar);
series.Points.Add(0, 200);
series.Points.Add(1, 300);
```

## ChartPoint Class

Holds data for a single point (X and Y values).

### Single Y Value
```csharp
// Method 1: Constructor
ChartPoint point = new ChartPoint(1, 100);
series.Points.Add(point);

// Method 2: Direct add
series.Points.Add(1, 100);

// DateTime X value
series.Points.Add(new DateTime(2024, 1, 1), 50);
```

### Multiple Y Values
Required for Candle, HiLo, Bubble, Range charts.

```csharp
// Candle chart: Open, High, Low, Close
series.Points.Add(1, new double[] { 100, 110, 95, 105 });

// HiLo chart: High, Low
series.Points.Add(1, new double[] { 110, 95 });

// Bubble chart: Y value, Size
series.Points.Add(1, new double[] { 50, 15 });
```

### ChartPoint Properties
```csharp
ChartPoint point = series.Points[0];
double x = point.X;                    // X value
double[] y = point.YValues;            // Y values array
string category = point.Category;      // Category name (for categorical axes)
bool isEmpty = point.IsEmpty;          // Empty point marker
Color color = point.Color;             // Point-specific color
```

### Custom Point Properties
```csharp
ChartPoint point = new ChartPoint(1, 100);
point.Text = "Peak Sales";             // Custom label
point.Symbol.Shape = ChartSymbolShape.Circle;
point.Symbol.Size = new Size(8, 8);
point.Symbol.Color = Color.Red;
series.Points.Add(point);
```

## Series Properties

### Basic Properties
```csharp
series.Name = "Q1 Sales";              // Series identifier
series.Text = "First Quarter";         // Display name (used in legend)
series.Type = ChartSeriesType.Column;  // Chart type
series.Visible = true;                 // Show/hide series
```

### Data Model Properties
```csharp
// For data binding
series.SeriesModelImpl = dataBindModel;        // Custom data model
series.CategoryModel = categoryBindModel;      // Category binding

// Access points
int count = series.Points.Count;
ChartPoint point = series.Points[0];
```

### Axis Binding
```csharp
// Bind to specific axes (for multiple axes scenarios)
series.YAxis = chartControl1.Axes[1];  // Use secondary Y axis
```

## Series Styling

### Style Property
```csharp
// Colors
series.Style.Interior = new BrushInfo(Color.Blue);
series.Style.Border.Color = Color.Navy;
series.Style.Border.Width = 2;

// Fonts
series.Style.Font = new Font("Arial", 10, FontStyle.Bold);
series.Style.TextColor = Color.White;

// Display text (data labels)
series.Style.DisplayText = true;
series.Style.TextOrientation = ChartTextOrientation.Up;
series.Style.TextFormat = "{1}";  // Format: {0}=X, {1}=Y, {2}=Y2, etc.
```

### Gradient and Effects
```csharp
// Gradient
series.Style.Interior = new BrushInfo(
    GradientStyle.Vertical,
    Color.LightBlue,
    Color.DarkBlue
);

// Shadow
series.Style.Shadow.Visible = true;
series.Style.Shadow.Color = Color.Gray;
series.Style.Shadow.Offset = new Size(3, 3);
```

### Symbols (for Line/Scatter charts)
```csharp
series.Style.Symbol.Shape = ChartSymbolShape.Diamond;
series.Style.Symbol.Size = new Size(10, 10);
series.Style.Symbol.Color = Color.Red;
series.Style.Symbol.Border.Color = Color.Black;
```

### Point-Level Styling via PrepareStyle Event
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
};
```

## Multiple Series

### Adding Multiple Series
```csharp
ChartSeries series1 = new ChartSeries("Product A");
series1.Type = ChartSeriesType.Column;
series1.Points.Add(1, 100);
series1.Points.Add(2, 150);

ChartSeries series2 = new ChartSeries("Product B");
series2.Type = ChartSeriesType.Column;
series2.Points.Add(1, 120);
series2.Points.Add(2, 130);

chartControl1.Series.Add(series1);
chartControl1.Series.Add(series2);
```

### Different Types (Combination Chart)
```csharp
ChartSeries columnSeries = new ChartSeries("Actual");
columnSeries.Type = ChartSeriesType.Column;
columnSeries.Points.Add(1, 100);

ChartSeries lineSeries = new ChartSeries("Target");
lineSeries.Type = ChartSeriesType.Line;
lineSeries.Points.Add(1, 110);

chartControl1.Series.Add(columnSeries);
chartControl1.Series.Add(lineSeries);
```

### Series Management
```csharp
// Access series
ChartSeries series = chartControl1.Series[0];
ChartSeries seriesByName = chartControl1.Series["Sales"];

// Remove series
chartControl1.Series.Remove(series);
chartControl1.Series.RemoveAt(0);
chartControl1.Series.Clear();

// Iterate series
foreach (ChartSeries s in chartControl1.Series)
{
    Console.WriteLine(s.Name);
}
```

### Shared Data Model
Multiple series can share the same underlying data model for efficiency:

```csharp
IChartSeriesModel sharedModel = GetDataModel();

ChartSeries series1 = new ChartSeries("View 1");
series1.SeriesModelImpl = sharedModel;

ChartSeries series2 = new ChartSeries("View 2");
series2.SeriesModelImpl = sharedModel;
```

## Series Configuration Items

Type-specific configuration:

```csharp
// Pie/Funnel charts
series.ConfigItems.PieItem.ExplodeIndex = 2;
series.ConfigItems.PieItem.ExplodeOffset = 20;
series.ConfigItems.PieItem.LabelStyle = ChartAccumulationLabelStyle.Outside;

// 3D effects (if 3D enabled)
series.ConfigItems.ColumnItem.DepthCoef = 0.5;
```

## Empty Points

Mark points as empty for gaps in data:

```csharp
ChartPoint emptyPoint = new ChartPoint(2, 0);
emptyPoint.IsEmpty = true;
series.Points.Add(emptyPoint);

// Empty point style
series.EmptyPointValue = ChartEmptyPointValue.Average;  // Average of adjacent points
// Options: Zero, Average, None
```

## Series Events

```csharp
// Triggered for each point during rendering
series.PrepareStyle += Series_PrepareStyle;

// Point added
series.Points.CollectionChanged += Points_CollectionChanged;
```
