# Chart Area

Chart area is the region containing plotted data, bounded by axes.

## Background and Borders

```csharp
// Background color
chartControl1.ChartArea.BackInterior = new BrushInfo(Color.White);

// Gradient background
chartControl1.ChartArea.BackInterior = new BrushInfo(
    GradientStyle.Vertical,
    Color.LightBlue,
    Color.White
);

// Border
chartControl1.ChartArea.Border.Color = Color.Black;
chartControl1.ChartArea.Border.Width = 2;
chartControl1.ChartArea.Border.DashStyle = DashStyle.Solid;
```

## Margins and Spacing

```csharp
// Auto margins
chartControl1.ChartArea.AutoMargins = true;

// Manual margins
chartControl1.ChartArea.AutoMargins = false;
chartControl1.ChartArea.Margins = new ChartMargins(50, 50, 50, 50);  // Left, Top, Right, Bottom
```

## Chart Interior

The plotting region inside the chart area.

```csharp
// Interior color
chartControl1.ChartInterior = new BrushInfo(Color.White);

// Gradient
chartControl1.ChartInterior = new BrushInfo(
    GradientStyle.Horizontal,
    Color.LightGray,
    Color.White
);
```

## Shadow Effects

```csharp
// Enable shadow
chartControl1.Shadows = true;
chartControl1.ShadowColor = Color.Gray;
chartControl1.ShadowOffset = new Size(5, 5);
chartControl1.ShadowWidth = 10;
```

## Chart Dimensions

```csharp
// Chart area size (relative to control)
chartControl1.ChartArea.Width = 90;   // Percentage
chartControl1.ChartArea.Height = 85;
```

## Background Image

```csharp
chartControl1.ChartArea.BackImage = Image.FromFile("background.png");
chartControl1.ChartArea.BackImageLayout = ChartImageLayout.Tile;
// Options: None, Tile, Center, Stretch, Zoom
```
