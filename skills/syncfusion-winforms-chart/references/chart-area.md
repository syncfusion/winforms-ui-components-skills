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
chartControl1.ChartArea.BorderColor = Color.Black;
chartControl1.ChartArea.BorderWidth = 2;
```

## Margins and Spacing

```csharp
chartControl1.ChartArea.ChartAreaMargins = new ChartMargins(50, 50, 50, 50);  // Left, Top, Right, Bottom
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
chartControl1.ChartAreaShadow = true;
chartControl1.ShadowColor = new Syncfusion.Drawing.BrushInfo(System.Drawing.Color.Gray);
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
```
