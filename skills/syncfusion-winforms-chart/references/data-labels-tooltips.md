# Data Labels and Tooltips

## Table of Contents
- [Data Labels](#data-labels)
- [Tooltips](#tooltips)
- [Custom Content](#custom-content)

## Data Labels

Display values on data points.

### Enabling Data Labels
```csharp
series.Style.DisplayText = true;
```

### Label Format
```csharp
// Format: {0}=X, {1}=Y, {2}=Y2, etc.
series.Style.TextFormat = "{1}";         // Show Y value
series.Style.TextFormat = "{0}: {1}";    // Show X: Y
series.Style.TextFormat = "${1:N2}";     // Currency format
```

### Label Orientation
```csharp
series.Style.TextOrientation = ChartTextOrientation.Up;
// Options: Up, Down, Left, Right, RegionUp, RegionDown, RegionCenter, Smart
```

### Label Appearance
```csharp
series.Style.Font = new Font("Arial", 9, FontStyle.Bold);
series.Style.TextColor = Color.White;
```

### Label Offset
```csharp
series.Style.TextOffset = 10;  // Distance from point
```

## Tooltips

Interactive popups on hover.

### Enabling Tooltips
```csharp
chartControl1.ShowToolTips = true;
```

### Tooltip Appearance
```csharp
chartControl1.Tooltip.BackgroundColor = new BrushInfo(Color.White);
chartControl1.Tooltip.BorderStyle = BorderStyle.FixedSingle;
chartControl1.Tooltip.Font = new Font("Segoe UI", 10);
```

### Default Tooltip Format
```csharp
series.Style.ToolTip = "{0}: {1}";  // X: Y format
```

## Custom Content

### Custom Labels via PrepareStyle
```csharp
series.PrepareStyle += (sender, args) =>
{
    ChartSeries s = sender as ChartSeries;
    ChartPoint point = s.Points[args.Index];
    
    // Custom label
    args.Style.Text = $"{point.YValues[0]:C2}";
    
    // Conditional formatting
    if (point.YValues[0] > 100)
    {
        args.Style.TextColor = Color.Green;
    }
};
```

### Custom Tooltips via PrepareStyle
```csharp
series.PrepareStyle += (sender, args) =>
{
    ChartSeries s = sender as ChartSeries;
    ChartPoint point = s.Points[args.Index];
    
    // Multi-line tooltip
    args.Style.ToolTip = $"Product: {point.Category}\n" +
                         $"Sales: ${point.YValues[0]:N2}\n" +
                         $"Date: {point.X}";
};
```

### Complex Tooltip Example
```csharp
chartControl1.ShowToolTips = true;
chartControl1.Tooltip.BackgroundColor = new BrushInfo(Color.FromArgb(240, 240, 240));
chartControl1.Tooltip.BorderStyle = BorderStyle.FixedSingle;

series.PrepareStyle += (sender, args) =>
{
    ChartSeries s = sender as ChartSeries;
    ChartPoint point = s.Points[args.Index];
    
    double value = point.YValues[0];
    double percentChange = args.Index > 0 ? 
        (value - s.Points[args.Index - 1].YValues[0]) / s.Points[args.Index - 1].YValues[0] * 100 : 0;
    
    args.Style.ToolTip = $"{point.Category}\n" +
                         $"Value: {value:N2}\n" +
                         $"Change: {percentChange:+0.0;-0.0}%";
};
```

## Label Visibility Control

```csharp
// Hide labels for specific points
series.PrepareStyle += (sender, args) =>
{
    if (args.Index == 2)  // Hide label for 3rd point
    {
        args.Style.DisplayText = false;
    }
};
```

## Smart Label Positioning

Automatically adjust labels to avoid overlap:

```csharp
series.Style.TextOrientation = ChartTextOrientation.Smart;
chartControl1.SmartLabels = true;
```
