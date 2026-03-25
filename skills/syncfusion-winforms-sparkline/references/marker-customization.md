# Marker Customization in Windows Forms Sparkline

Markers are visual indicators that highlight specific data points in sparkline graphs. The Sparkline control provides comprehensive marker support for all three sparkline types (Line, Column, WinLoss), allowing you to emphasize important data points with customizable colors and styles.

## Table of Contents
- [Overview](#overview)
- [Marker Properties Reference](#marker-properties-reference)
- [Markers for Line Sparkline](#markers-for-line-sparkline)
- [Markers for Column Sparkline](#markers-for-column-sparkline)
- [Markers for WinLoss Sparkline](#markers-for-winloss-sparkline)
- [Color Customization with BrushInfo](#color-customization-with-brushinfo)
- [Complete Marker Examples](#complete-marker-examples)

## Overview

Markers serve as visual indicators to represent the location of data points in sparkline graphs. They help users quickly identify:
- All data points (Line sparkline)
- Highest values
- Lowest values
- Start points
- End points
- Negative values

**Key Benefits:**
- Immediate visual emphasis on important data points
- Customizable colors for different marker types
- Works across all sparkline types
- Simple boolean properties to enable/disable

## Marker Properties Reference

The `Markers` property provides the following configuration options:

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `ShowMarker` | bool | Shows markers at every data point (Line sparkline only) | False |
| `ShowHighPoint` | bool | Shows marker at the highest value | False |
| `ShowLowPoint` | bool | Shows marker at the lowest value | False |
| `ShowStartPoint` | bool | Shows marker at the first data point | False |
| `ShowEndPoint` | bool | Shows marker at the last data point | False |
| `ShowNegativePoint` | bool | Shows markers at all negative values | False |
| `MarkerColor` | BrushInfo | Color for all data point markers (with ShowMarker) | Default |
| `HighPointColor` | BrushInfo | Color for highest point marker | Default |
| `LowPointColor` | BrushInfo | Color for lowest point marker | Default |
| `StartPointColor` | BrushInfo | Color for start point marker | Default |
| `EndPointColor` | BrushInfo | Color for end point marker | Default |
| `NegativePointColor` | BrushInfo | Color for negative point markers | Default |

**Property Application:**
- **Line Sparkline**: All marker properties available
- **Column Sparkline**: ShowMarker not applicable; other marker types available
- **WinLoss Sparkline**: Same as Column (ShowMarker not applicable)

## Markers for Line Sparkline

Line sparklines support showing markers at all data points or selectively at specific points (high, low, start, end, negative).

### Showing All Data Point Markers

To display markers at every data point location:

```csharp
// Enable markers for all data points
this.sparkLine1.Markers.ShowMarker = true;
```

```vb
' VB.NET example
Me.sparkLine1.Markers.ShowMarker = True
```

**Result:** Small circular markers appear at each data point on the line.

### Customizing All Data Point Marker Color

```csharp
// Enable and customize all data point markers
this.sparkLine1.Type = SparkLineType.Line;
this.sparkLine1.Source = new double[] { 30, -20, 80, 20, 40, -50, -30, 70, -40, 50 };

this.sparkLine1.Markers.ShowMarker = true;
this.sparkLine1.Markers.MarkerColor = new BrushInfo(Color.DarkBlue);
```

**Result:** All data points display with dark blue markers.

### Selective Point Highlighting

Instead of showing all markers, highlight only specific points of interest:

```csharp
// Line sparkline with selective markers
this.sparkLine1.Type = SparkLineType.Line;
this.sparkLine1.Source = new double[] { 30, -20, 80, 20, 40, -50, -30, 70, -40, 50 };

// Show only high, low, and end points
this.sparkLine1.Markers.ShowHighPoint = true;
this.sparkLine1.Markers.ShowLowPoint = true;
this.sparkLine1.Markers.ShowEndPoint = true;

// Customize colors
this.sparkLine1.Markers.HighPointColor = new BrushInfo(Color.Green);
this.sparkLine1.Markers.LowPointColor = new BrushInfo(Color.Red);
this.sparkLine1.Markers.EndPointColor = new BrushInfo(Color.Blue);
```

**Result:** Only the highest value (green), lowest value (red), and last point (blue) are marked.

### Complete Line Marker Example

```csharp
// Line sparkline with all marker types enabled
this.sparkLine1.Type = SparkLineType.Line;
this.sparkLine1.Source = new double[] { 30, -20, 80, 20, 40, -50, -30, 70, -40, 50 };

// Enable all marker types
this.sparkLine1.Markers.ShowMarker = true;
this.sparkLine1.Markers.ShowHighPoint = true;
this.sparkLine1.Markers.ShowLowPoint = true;
this.sparkLine1.Markers.ShowStartPoint = true;
this.sparkLine1.Markers.ShowEndPoint = true;
this.sparkLine1.Markers.ShowNegativePoint = true;

// Customize all marker colors
this.sparkLine1.Markers.MarkerColor = new BrushInfo(Color.Gray);
this.sparkLine1.Markers.HighPointColor = new BrushInfo(Color.Green);
this.sparkLine1.Markers.LowPointColor = new BrushInfo(Color.Red);
this.sparkLine1.Markers.StartPointColor = new BrushInfo(Color.Orange);
this.sparkLine1.Markers.EndPointColor = new BrushInfo(Color.Blue);
this.sparkLine1.Markers.NegativePointColor = new BrushInfo(Color.DarkRed);
```

**Note:** When multiple marker types are enabled for the same point, priority is typically: HighPoint > LowPoint > StartPoint > EndPoint > NegativePoint > Marker.

## Markers for Column Sparkline

Column sparklines support markers for high points, low points, start points, end points, and negative points. The `ShowMarker` property is not applicable to column sparklines since each column is already a distinct visual element.

### Enabling Specific Point Markers

```csharp
// Column sparkline with markers
this.sparkLine1.Type = SparkLineType.Column;
this.sparkLine1.Source = new double[] { 30, -20, 80, 20, 40, -50, -30, 70, -40, 50 };

// Enable specific point markers
this.sparkLine1.Markers.ShowHighPoint = true;
this.sparkLine1.Markers.ShowLowPoint = true;
this.sparkLine1.Markers.ShowStartPoint = true;
this.sparkLine1.Markers.ShowEndPoint = true;
this.sparkLine1.Markers.ShowNegativePoint = true;
```

```vb
' VB.NET example
Me.sparkLine1.Type = SparkLineType.Column
Me.sparkLine1.Source = New Double() {30, -20, 80, 20, 40, -50, -30, 70, -40, 50}

Me.sparkLine1.Markers.ShowHighPoint = True
Me.sparkLine1.Markers.ShowLowPoint = True
Me.sparkLine1.Markers.ShowStartPoint = True
Me.sparkLine1.Markers.ShowEndPoint = True
Me.sparkLine1.Markers.ShowNegativePoint = True
```

**Result:** Columns representing high, low, start, end, and negative points are visually distinguished from other columns.

### Customizing Column Marker Colors

Apply custom colors to emphasize different marker types:

```csharp
// Column sparkline with custom marker colors
this.sparkLine1.Type = SparkLineType.Column;
this.sparkLine1.Source = new double[] { 30, -20, 80, 20, 40, -50, -30, 70, -40, 50 };

// Enable markers
this.sparkLine1.Markers.ShowHighPoint = true;
this.sparkLine1.Markers.ShowLowPoint = true;
this.sparkLine1.Markers.ShowNegativePoint = true;

// Set custom colors
this.sparkLine1.Markers.HighPointColor = new BrushInfo(Color.DarkGreen);
this.sparkLine1.Markers.LowPointColor = new BrushInfo(Color.DarkRed);
this.sparkLine1.Markers.NegativePointColor = new BrushInfo(Color.Crimson);
```

```vb
' VB.NET example with custom colors
Me.sparkLine1.Markers.ShowHighPoint = True
Me.sparkLine1.Markers.ShowLowPoint = True
Me.sparkLine1.Markers.ShowNegativePoint = True

Me.sparkLine1.Markers.HighPointColor = New BrushInfo(Color.DarkGreen)
Me.sparkLine1.Markers.LowPointColor = New BrushInfo(Color.DarkRed)
Me.sparkLine1.Markers.NegativePointColor = New BrushInfo(Color.Crimson)
```

### Advanced Column Marker with Gradient

Use gradient colors for more sophisticated marker appearance:

```csharp
// Column sparkline with gradient marker color
this.sparkLine1.Type = SparkLineType.Column;
this.sparkLine1.Source = new double[] { 30, -20, 80, 20, 40, -50, -30, 70, -40, 50 };

// Enable low point marker
this.sparkLine1.Markers.ShowLowPoint = true;

// Apply gradient color to low point marker
this.sparkLine1.Markers.LowPointColor = new BrushInfo(
    GradientStyle.BackwardDiagonal, 
    Color.Blue, 
    Color.Wheat
);
```

```vb
' VB.NET example with gradient
Me.sparkLine1.Markers.ShowLowPoint = True
Me.sparkLine1.Markers.LowPointColor = New BrushInfo( _
    GradientStyle.BackwardDiagonal, _
    Color.Blue, _
    Color.Wheat _
)
```

**Result:** The lowest point column displays with a blue-to-wheat gradient fill.

## Markers for WinLoss Sparkline

WinLoss sparklines support the same marker types as Column sparklines. Since WinLoss is essentially a specialized column chart, marker behavior is identical.

### Basic WinLoss Markers

```csharp
// WinLoss sparkline with markers
this.sparkLine1.Type = SparkLineType.WinLoss;
this.sparkLine1.Source = new double[] { 30, -20, 80, 20, 40, -50, -30, 70, -40, 50 };

// Enable markers
this.sparkLine1.Markers.ShowHighPoint = true;
this.sparkLine1.Markers.ShowLowPoint = true;
this.sparkLine1.Markers.ShowStartPoint = true;
this.sparkLine1.Markers.ShowEndPoint = true;
this.sparkLine1.Markers.ShowNegativePoint = true;
```

```vb
' VB.NET example
Me.sparkLine1.Type = SparkLineType.WinLoss
Me.sparkLine1.Markers.ShowHighPoint = True
Me.sparkLine1.Markers.ShowLowPoint = True
Me.sparkLine1.Markers.ShowStartPoint = True
Me.sparkLine1.Markers.ShowEndPoint = True
Me.sparkLine1.Markers.ShowNegativePoint = True
```

### WinLoss with Custom Marker Colors

```csharp
// WinLoss with custom colors
this.sparkLine1.Type = SparkLineType.WinLoss;
this.sparkLine1.Source = new double[] { 1, -1, 1, 1, -1, 1, -1, -1, 1, 1 };

// Emphasize negative (loss) points
this.sparkLine1.Markers.ShowNegativePoint = true;
this.sparkLine1.Markers.NegativePointColor = new BrushInfo(Color.Red);

// Highlight start and end
this.sparkLine1.Markers.ShowStartPoint = true;
this.sparkLine1.Markers.ShowEndPoint = true;
this.sparkLine1.Markers.StartPointColor = new BrushInfo(Color.Orange);
this.sparkLine1.Markers.EndPointColor = new BrushInfo(Color.Green);
```

**Use Case:** In a win/loss scenario, this highlights losses in red while showing where the sequence started (orange) and ended (green).

### WinLoss Gradient Markers

```csharp
// WinLoss with gradient marker colors
this.sparkLine1.Type = SparkLineType.WinLoss;
this.sparkLine1.Source = new double[] { 30, -20, 80, 20, 40, -50, -30, 70, -40, 50 };

this.sparkLine1.Markers.ShowHighPoint = true;
this.sparkLine1.Markers.ShowLowPoint = true;
this.sparkLine1.Markers.ShowNegativePoint = true;

// Apply gradient colors
this.sparkLine1.Markers.HighPointColor = new BrushInfo(
    GradientStyle.ForwardDiagonal,
    Color.LightGreen,
    Color.DarkGreen
);

this.sparkLine1.Markers.LowPointColor = new BrushInfo(
    GradientStyle.BackwardDiagonal,
    Color.Blue,
    Color.Wheat
);

this.sparkLine1.Markers.NegativePointColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.Red,
    Color.DarkRed
);
```

```vb
' VB.NET with gradients
Me.sparkLine1.Markers.HighPointColor = New BrushInfo( _
    GradientStyle.ForwardDiagonal, _
    Color.LightGreen, _
    Color.DarkGreen _
)

Me.sparkLine1.Markers.LowPointColor = New BrushInfo( _
    GradientStyle.BackwardDiagonal, _
    Color.Blue, _
    Color.Wheat _
)

Me.sparkLine1.Markers.NegativePointColor = New BrushInfo( _
    GradientStyle.Horizontal, _
    Color.Red, _
    Color.DarkRed _
)
```

## Color Customization with BrushInfo

The `BrushInfo` class provides flexible color customization options for markers.

### Solid Color

```csharp
// Simple solid color
this.sparkLine1.Markers.HighPointColor = new BrushInfo(Color.Green);
```

### Gradient Color

```csharp
// Two-color gradient with style
this.sparkLine1.Markers.LowPointColor = new BrushInfo(
    GradientStyle.BackwardDiagonal,  // Gradient direction
    Color.Blue,                       // Start color
    Color.Wheat                       // End color
);
```

**Available Gradient Styles:**
- `GradientStyle.ForwardDiagonal` - Top-left to bottom-right
- `GradientStyle.BackwardDiagonal` - Top-right to bottom-left
- `GradientStyle.Horizontal` - Left to right
- `GradientStyle.Vertical` - Top to bottom
- And other gradient styles from the GradientStyle enum

## Complete Marker Examples

### Example 1: Dashboard KPI Sparkline

```csharp
// Line sparkline for dashboard with emphasis on current value
SparkLine kpiSparkline = new SparkLine();
kpiSparkline.Type = SparkLineType.Line;
kpiSparkline.Source = new double[] { 100, 110, 105, 120, 115, 130, 125, 140 };
kpiSparkline.Size = new Size(150, 40);

// Show only end point (current value) and high point
kpiSparkline.Markers.ShowEndPoint = true;
kpiSparkline.Markers.ShowHighPoint = true;
kpiSparkline.Markers.EndPointColor = new BrushInfo(Color.Blue);
kpiSparkline.Markers.HighPointColor = new BrushInfo(Color.Green);

this.Controls.Add(kpiSparkline);
```

### Example 2: Sales Performance Column Chart

```csharp
// Column sparkline showing monthly sales with markers
SparkLine salesSparkline = new SparkLine();
salesSparkline.Type = SparkLineType.Column;
salesSparkline.Source = new double[] { 5000, 6500, 4800, 7200, 6800, 8100, 7500, 9000 };
salesSparkline.Size = new Size(200, 50);

// Highlight best and worst months
salesSparkline.Markers.ShowHighPoint = true;
salesSparkline.Markers.ShowLowPoint = true;
salesSparkline.Markers.HighPointColor = new BrushInfo(Color.Gold);
salesSparkline.Markers.LowPointColor = new BrushInfo(Color.OrangeRed);

this.Controls.Add(salesSparkline);
```

### Example 3: Win/Loss Streak Visualization

```csharp
// WinLoss sparkline for game results
SparkLine gameResults = new SparkLine();
gameResults.Type = SparkLineType.WinLoss;
gameResults.Source = new double[] { 1, 1, -1, 1, -1, -1, -1, 1, 1, 1 };
gameResults.Size = new Size(180, 40);

// Emphasize losses and show how it ended
gameResults.Markers.ShowNegativePoint = true;
gameResults.Markers.ShowEndPoint = true;
gameResults.Markers.NegativePointColor = new BrushInfo(Color.Red);
gameResults.Markers.EndPointColor = new BrushInfo(Color.Green);

this.Controls.Add(gameResults);
```

### Example 4: Stock Price with Multiple Markers

```csharp
// Line sparkline showing stock price with comprehensive markers
SparkLine stockPrice = new SparkLine();
stockPrice.Type = SparkLineType.Line;
stockPrice.Source = new double[] { 50.5, 52.3, 51.8, 54.2, 53.1, 49.8, 51.5, 55.0 };
stockPrice.Size = new Size(200, 50);

// Enable all relevant markers
stockPrice.Markers.ShowHighPoint = true;
stockPrice.Markers.ShowLowPoint = true;
stockPrice.Markers.ShowStartPoint = true;
stockPrice.Markers.ShowEndPoint = true;

// Color-code for financial context
stockPrice.Markers.HighPointColor = new BrushInfo(Color.Green);      // Peak
stockPrice.Markers.LowPointColor = new BrushInfo(Color.Red);         // Trough
stockPrice.Markers.StartPointColor = new BrushInfo(Color.Gray);      // Opening
stockPrice.Markers.EndPointColor = new BrushInfo(Color.DarkBlue);    // Current

// Customize line
stockPrice.LineStyle.LineColor = Color.Navy;

this.Controls.Add(stockPrice);
```

### Example 5: All Markers with Gradients

```csharp
// Column sparkline with full marker customization and gradients
SparkLine fullCustom = new SparkLine();
fullCustom.Type = SparkLineType.Column;
fullCustom.Source = new double[] { 30, -20, 80, 20, 40, -50, -30, 70, -40, 50 };
fullCustom.Size = new Size(250, 60);

// Enable all markers
fullCustom.Markers.ShowHighPoint = true;
fullCustom.Markers.ShowLowPoint = true;
fullCustom.Markers.ShowStartPoint = true;
fullCustom.Markers.ShowEndPoint = true;
fullCustom.Markers.ShowNegativePoint = true;

// Apply gradient colors
fullCustom.Markers.HighPointColor = new BrushInfo(
    GradientStyle.Vertical, Color.LightGreen, Color.DarkGreen);
    
fullCustom.Markers.LowPointColor = new BrushInfo(
    GradientStyle.Vertical, Color.LightCoral, Color.DarkRed);
    
fullCustom.Markers.StartPointColor = new BrushInfo(
    GradientStyle.ForwardDiagonal, Color.Yellow, Color.Orange);
    
fullCustom.Markers.EndPointColor = new BrushInfo(
    GradientStyle.BackwardDiagonal, Color.LightBlue, Color.DarkBlue);
    
fullCustom.Markers.NegativePointColor = new BrushInfo(
    GradientStyle.Horizontal, Color.Pink, Color.Red);

this.Controls.Add(fullCustom);
```

## Best Practices

1. **Don't over-mark**: Too many marker types can create visual clutter. Choose markers that serve your specific use case.

2. **Use contrasting colors**: Ensure marker colors contrast with the sparkline background and each other for clear visibility.

3. **Consider context**: In financial contexts, green often means positive/high, red means negative/low. Use conventional color associations when appropriate.

4. **Test readability**: Ensure markers are visible at the sparkline's actual display size.

5. **Prioritize information**: If showing multiple marker types, use brighter/bolder colors for the most important information.

6. **Consistent styling**: Maintain consistent marker color schemes across multiple sparklines in the same interface.
