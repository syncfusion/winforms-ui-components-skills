# Appearance and Styling

## Table of Contents
- [Built-in Skins](#built-in-skins)
- [Color Palettes](#color-palettes)
- [Custom Palettes](#custom-palettes)
- [Fonts and Text](#fonts-and-text)
- [BrushInfo for Advanced Styling](#brushinfo-for-advanced-styling)

## Built-in Skins

Apply predefined themes for consistent appearance.

```csharp
chartControl1.Skins = Skins.Metro;
```

**Available skins:**
- `Skins.Metro`
- `Skins.Office2016Colorful`
- `Skins.Office2016White`
- `Skins.Office2016Black`
- `Skins.Office2019Colorful`
- `Skins.None` (custom styling)

## Color Palettes

Built-in color schemes for series.

```csharp
chartControl1.Palette = ChartColorPalette.Metro;
```

**Available palettes:**
- `Metro`, `Office2016`, `Office2019`
- `Nature`, `Pastel`, `EarthTones`
- `Custom` (use CustomPalette)

## Custom Palettes

Define custom color scheme for series.

```csharp
chartControl1.Palette = ChartColorPalette.Custom;
chartControl1.CustomPalette = new Color[]
{
    Color.FromArgb(0, 120, 215),   // Series 1
    Color.FromArgb(255, 185, 0),   // Series 2
    Color.FromArgb(232, 17, 35),   // Series 3
    Color.FromArgb(0, 153, 78)     // Series 4
};
```

**Non-gradient palette:**
```csharp
chartControl1.UseGradientPalette = false;  // Solid colors
```

## Fonts and Text

### Global Font Settings
```csharp
// Chart text
chartControl1.Font = new Font("Arial", 10);
chartControl1.ForeColor = Color.Black;
```

### Component-Specific Fonts
```csharp
// Title font
chartControl1.Titles[0].Font = new Font("Arial", 14, FontStyle.Bold);

// Legend font
chartControl1.Legend.Font = new Font("Segoe UI", 9);

// Axis labels
chartControl1.PrimaryXAxis.Font = new Font("Arial", 8);
chartControl1.PrimaryYAxis.Font = new Font("Arial", 8);
```

### Data Label Fonts
```csharp
series.Style.Font = new Font("Arial", 9, FontStyle.Bold);
series.Style.TextColor = Color.White;
```

## BrushInfo for Advanced Styling

`BrushInfo` provides flexible color/gradient configuration.

### Solid Color
```csharp
series.Style.Interior = new BrushInfo(Color.Blue);
```

### Gradients
```csharp
// Linear gradient
series.Style.Interior = new BrushInfo(
    GradientStyle.Vertical,
    Color.LightBlue,
    Color.DarkBlue
);

// Gradient styles: Vertical, Horizontal, ForwardDiagonal, BackwardDiagonal, PathEllipse, PathRectangle
```

### Pattern Fill
```csharp
series.Style.Interior = new BrushInfo(
    PatternStyle.DiagonalCross,
    Color.Red,
    Color.White
);
```

### Texture
```csharp
series.Style.Interior = new BrushInfo(Image.FromFile("texture.png"));
```

## Series-Specific Styling

```csharp
// Individual series styling
series.Style.Interior = new BrushInfo(Color.Green);
series.Style.Border.Color = Color.DarkGreen;
series.Style.Border.Width = 2;
```

## Chart Background Styling

```csharp
// Chart control background
chartControl1.BackColor = Color.White;
chartControl1.BackInterior = new BrushInfo(GradientStyle.Vertical, Color.LightGray, Color.White);

// Chart area background
chartControl1.ChartArea.BackInterior = new BrushInfo(Color.WhiteSmoke);

// Chart interior (plotting area)
chartControl1.ChartInterior = new BrushInfo(Color.White);
```

## Border Styling

```csharp
// Chart control border
chartControl1.BorderStyle = BorderStyle.FixedSingle;

// Chart area border
chartControl1.ChartArea.Border.Color = Color.Black;
chartControl1.ChartArea.Border.Width = 1;
chartControl1.ChartArea.Border.DashStyle = DashStyle.Solid;
```

## Complete Styling Example

```csharp
// Apply Metro theme
chartControl1.Skins = Skins.Metro;

// Custom series colors
chartControl1.Palette = ChartColorPalette.Custom;
chartControl1.CustomPalette = new Color[]
{
    Color.FromArgb(0, 120, 215),
    Color.FromArgb(255, 185, 0)
};

// Background
chartControl1.BackInterior = new BrushInfo(Color.White);
chartControl1.ChartInterior = new BrushInfo(Color.WhiteSmoke);

// Fonts
chartControl1.Font = new Font("Segoe UI", 10);
```
