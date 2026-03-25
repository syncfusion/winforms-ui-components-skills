   # Appearance and Styling

## Overview

The Smith Chart provides extensive customization options for controlling the visual appearance, including color palettes, chart area styling, and circle radius adjustment.

## Chart Palette

Control the color scheme of series using the `Palette` property of the `ColorModel`.

### Available Palettes

| Palette | Description |
|---------|-------------|
| `Metro` | Modern metro-style colors (default) |
| `Nature` | Natural, earth-tone colors |
| `None` |No palette is set. |
| `Pastel` | Palette containing pastel colors. |
| `SkyBlueStyle` | Palette that contains mixed SkyBlue and Violet colors. |
| `Triad` | Palette containing triad colors. |
| `Custom` | Use custom color collection |
| `Colorful` | Colorful palette |
| `EarthTone` |Palette containing earth tone colors |
| `WarmCold` |Palette that contains mixed warm and cold colors. |

### Chart-Level Palette

Apply a palette to the entire chart, affecting all series:

**C# Example:**
```csharp
sfSmithChart1.ColorModel.Palette = ChartColorPalette.Nature;
```

**VB.NET Example:**
```vb
sfSmithChart1.ColorModel.Palette = ChartColorPalette.Nature
```

### Series-Level Palette

Override the chart palette for a specific series:

**C# Example:**
```csharp
LineSeries series = new LineSeries();
series.ColorModel.Palette = ChartColorPalette.Metro;
series.DataLabel.Visible = true;
sfSmithChart1.Series.Add(series);
```

**VB.NET Example:**
```vb
Dim series As New LineSeries()
series.ColorModel.Palette = ChartColorPalette.Metro
series.DataLabel.Visible = True
sfSmithChart1.Series.Add(series)
```

**When to Use Series-Level Palette:**
- Highlighting a specific series with distinct colors
- Applying different color schemes to different data sets
- Overriding chart-level palette for individual series

## Chart Area Customization

Customize the chart control and the circular plotting area independently.

### Chart Area Properties

| Property | Type | Description |
|----------|------|-------------|
| `BackColor` | Color | Background color of entire chart control |
| `Style.ChartAreaBackColor` | Color | Background color of circular plotting area |
| `Style.ChartAreaBorderColor` | Color | Border color around circular plotting area |
| `Style.ChartAreaBorderWidth` | int | Border thickness in pixels |

### Complete Customization Example

**C# Example:**
```csharp
sfSmithChart1.BackColor = Color.LightSteelBlue;
sfSmithChart1.Style.ChartAreaBorderColor = Color.SkyBlue;
sfSmithChart1.Style.ChartAreaBackColor = Color.AliceBlue;
sfSmithChart1.Style.ChartAreaBorderWidth = 2;
```

**VB.NET Example:**
```vb
sfSmithChart1.BackColor = Color.LightSteelBlue
sfSmithChart1.Style.ChartAreaBorderColor = Color.SkyBlue
sfSmithChart1.Style.ChartAreaBackColor = Color.AliceBlue
sfSmithChart1.Style.ChartAreaBorderWidth = 2
```

This creates:
- Light steel blue background for the entire control
- Alice blue background for the circular plot area
- Sky blue border around the circular plot area (2 pixels thick)

### Color Combinations

**Professional Look:**
```csharp
sfSmithChart1.BackColor = Color.White;
sfSmithChart1.Style.ChartAreaBackColor = Color.WhiteSmoke;
sfSmithChart1.Style.ChartAreaBorderColor = Color.Gray;
sfSmithChart1.Style.ChartAreaBorderWidth = 1;
```

**High Contrast:**
```csharp
sfSmithChart1.BackColor = Color.Black;
sfSmithChart1.Style.ChartAreaBackColor = Color.DarkSlateGray;
sfSmithChart1.Style.ChartAreaBorderColor = Color.White;
sfSmithChart1.Style.ChartAreaBorderWidth = 2;
```

**Soft Pastel:**
```csharp
sfSmithChart1.BackColor = Color.Lavender;
sfSmithChart1.Style.ChartAreaBackColor = Color.LavenderBlush;
sfSmithChart1.Style.ChartAreaBorderColor = Color.MediumPurple;
sfSmithChart1.Style.ChartAreaBorderWidth = 2;
```

## Circle Radius

Adjust the diameter of the Smith Chart's circular plotting area relative to the available space.

### Radius Property

**Type:** `float`  
**Range:** 0.1 to 1.0  
**Default:** 0.95

- **0.1:** Very small circle (10% of available space)
- **0.95:** Nearly full space (default)
- **1.0:** Maximum size filling entire plot area

### Setting Radius

**C# Example:**
```csharp
sfSmithChart1.Radius = 0.6f;
```

**VB.NET Example:**
```vb
sfSmithChart1.Radius = 0.6F
```

### When to Adjust Radius

**Smaller Radius (0.5-0.8):**
- When you need more space for labels outside the circle
- For legends positioned around the chart
- To create visual breathing room
- When displaying dense data with extensive labels

**Larger Radius (0.9-1.0):**
- Maximize plotting area for data visualization
- When labels are inside the circle
- For detailed charts requiring precision
- When chart dimensions are large

## Common Patterns

### Pattern 1: Clean Professional Look

```csharp
// White background with subtle chart area distinction
sfSmithChart1.BackColor = Color.White;
sfSmithChart1.Style.ChartAreaBackColor = Color.WhiteSmoke;
sfSmithChart1.Style.ChartAreaBorderColor = Color.LightGray;
sfSmithChart1.Style.ChartAreaBorderWidth = 1;
sfSmithChart1.Radius = 0.9f;
sfSmithChart1.ColorModel.Palette = ChartColorPalette.Metro;
```

### Pattern 2: High Visibility for Presentations

```csharp
// Bold colors with strong contrast
sfSmithChart1.BackColor = Color.Navy;
sfSmithChart1.Style.ChartAreaBackColor = Color.DarkBlue;
sfSmithChart1.Style.ChartAreaBorderColor = Color.Cyan;
sfSmithChart1.Style.ChartAreaBorderWidth = 3;
sfSmithChart1.Radius = 0.85f;
sfSmithChart1.ColorModel.Palette = ChartColorPalette.SkyBlueStyle;
```

### Pattern 3: Compact Chart with Labels

```csharp
// Smaller circle to accommodate external labels
sfSmithChart1.BackColor = Color.White;
sfSmithChart1.Style.ChartAreaBackColor = Color.Azure;
sfSmithChart1.Style.ChartAreaBorderColor = Color.SteelBlue;
sfSmithChart1.Style.ChartAreaBorderWidth = 2;
sfSmithChart1.Radius = 0.7f;  // Leave room for labels
sfSmithChart1.ColorModel.Palette = ChartColorPalette.Nature;
```

### Pattern 4: Series-Specific Coloring

```csharp
// Chart uses default palette, but one series gets special treatment
sfSmithChart1.ColorModel.Palette = ChartColorPalette.Metro;

LineSeries highlightedSeries = new LineSeries();
highlightedSeries.ColorModel.Palette = ChartColorPalette.SkyBlueStyle;
highlightedSeries.DataSource = importantData;
highlightedSeries.ResistanceMember = "Resistance";
highlightedSeries.ReactanceMember = "Reactance";
sfSmithChart1.Series.Add(highlightedSeries);
```

### Pattern 5: Themed Appearance

```csharp
// Apply a complete theme
void ApplyDarkTheme(SfSmithChart chart)
{
    chart.BackColor = Color.FromArgb(30, 30, 30);
    chart.Style.ChartAreaBackColor = Color.FromArgb(45, 45, 45);
    chart.Style.ChartAreaBorderColor = Color.FromArgb(100, 100, 100);
    chart.Style.ChartAreaBorderWidth = 1;
    chart.Radius = 0.9f;
    chart.ColorModel.Palette = ChartColorPalette.Metro;
    
    // Adjust axes for dark theme   
    chart.HorizontalAxis.Style.MajorGridlinesColor = Color.FromArgb(80, 80, 80);
    chart.RadialAxis.Style.MajorGridlinesColor = Color.FromArgb(80, 80, 80);
}

ApplyDarkTheme(sfSmithChart1);
```

## Best Practices

1. **Color Contrast:** Ensure sufficient contrast between:
   - Chart background and chart area background
   - Gridlines and chart area background
   - Series lines and chart area background

2. **Palette Selection:**
   - Use Metro for modern applications
   - Use Nature for scientific/environmental data
   - Use TomatoSpectrum for alert/warning visualizations

3. **Border Width:**
   - Use 1-2 pixels for subtle distinction
   - Use 3+ pixels for emphasis or presentation mode

4. **Radius Guidelines:**
   - 0.95 (default) for most scenarios
   - 0.7-0.8 when labels are placed outside
   - 0.6 when legend is positioned inside chart area
   - Never below 0.5 for readability

5. **Consistent Theming:** If applying custom colors, maintain consistency across:
   - Chart background
   - Chart area
   - Gridlines
   - Series colors
   - Legend styling

6. **Accessibility:** Consider color-blind friendly palettes for wider audience reach

## Troubleshooting

### Series Colors Look the Same

- Check that different palettes or colors are assigned to each series
- Verify `ColorModel.Palette` is set appropriately

### Chart Area Not Visible

- Ensure `ChartAreaBackColor` differs from `BackColor`
- Check that `ChartAreaBorderWidth` is greater than 0 if relying on border distinction
- Verify colors have sufficient contrast

### Circle Too Small or Too Large

- Adjust `Radius` property (0.1 to 1.0 range)
- Default 0.95 works for most cases
- Reduce for more label space, increase for more data visualization area

### Palette Not Applied

- Verify palette is set on the correct object (chart vs. series level)
- Check that palette property is set before adding series
- Ensure series are added to the chart after palette configuration
