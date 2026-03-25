# Legend Customization

## Table of Contents
- [Overview](#overview)
- [Enabling Legend](#enabling-legend)
- [Positioning](#positioning)
- [Legend Icon](#legend-icon)
- [Alignment](#alignment)
- [Customization](#customization)
- [Toggle Series Visibility](#toggle-series-visibility)
- [Smart View](#smart-view)

## Overview

The legend displays a list of chart series, helping users identify different data sets. Each series can have custom text, icons, and styling.

## Enabling Legend

Enable the legend by setting the `Visible` property to `true`:

**C# Example:**
```csharp
chart.Legend.Visible = true;
```

**VB.NET Example:**
```vb
chart.Legend.Visible = True
```

### Setting Legend Text for Series

Each series must have a `LegendText` property to appear in the legend:

**C# Example:**
```csharp
series.LegendText = "Transmission1";
```

**VB.NET Example:**
```vb
series.LegendText = "Transmission1"
```

## Positioning

Position the legend using the `DockPosition` property.

### Available Positions

| DockPosition | Description |
|--------------|-------------|
| `Top` | Legend at the top of the chart (default) |
| `Bottom` | Legend at the bottom of the chart |
| `Left` | Legend on the left side of the chart |
| `Right` | Legend on the right side of the chart |
| `Floating` | Float position. |

### Setting Position

**C# Example:**
```csharp
sfSmithChart1.Legend.DockPosition = DockPosition.Bottom;
```

**VB.NET Example:**
```vb
sfSmithChart1.Legend.DockPosition = DockPosition.Bottom
```

## Legend Icon

Customize the symbol associated with each legend item.

### Available Icon Types

| IconType | Description |
|----------|-------------|
| `Circle` | Circular icon (default) |
| `Rectangle` | Rectangular icon |
| `Diamond` | Diamond-shaped icon |
| `Triangle` | Triangular icon |
| `Cross` | Cross (+) icon |
| `Pentagon` | Five-sided icon |
| `Custom` | Custom legend icon will set. |
| `HorizontalLine` | HorizontalLine legend icon will set. |

### Setting Icon Type

**C# Example:**
```csharp
sfSmithChart1.Legend.IconType = SmithChartLegendIconType.Rectangle;
```

**VB.NET Example:**
```vb
sfSmithChart1.Legend.IconType = SmithChartLegendIconType.Rectangle
```

### Icon Size

Customize icon dimensions using `IconHeight` and `IconWidth`:

**C# Example:**
```csharp
sfSmithChart1.Legend.IconHeight = 13;
sfSmithChart1.Legend.IconWidth = 13;
```

**VB.NET Example:**
```vb
sfSmithChart1.Legend.IconHeight = 13
sfSmithChart1.Legend.IconWidth = 13
```

## Alignment

The `Alignment` property controls how legend items are aligned within the legend area.

### Available Alignments

| Alignment | Description |
|-----------|-------------|
| `Center` | Center alignment (default) |
| `Near` | Align to the start (left for horizontal, top for vertical) |
| `Far` | Align to the end (right for horizontal, bottom for vertical) |

### Setting Alignment

**C# Example:**
```csharp
sfSmithChart1.Legend.Alignment = System.Drawing.StringAlignment.Near;
```

**VB.NET Example:**
```vb
sfSmithChart1.Legend.Alignment = System.Drawing.StringAlignment.Near
```

## Customization

Customize legend appearance using various style properties:

### Customization Properties

| Property | Type | Description |
|----------|------|-------------|
| `IconType` | SmithChartLegendIconType | Shape of legend icons |
| `IconHeight` | int | Icon height in pixels |
| `IconWidth` | int | Icon width in pixels |
| `Style.ForeColor` | Color | Text color |
| `Style.BackColor` | Color | Background color |
| `Style.BorderColor` | Color | Border color |
| `Style.BorderWidth` | int | Border thickness |
| `Spacing` | int | Space between border and items |
| `ItemSpacing` | int | Space between legend items |
| `BorderVisible` | bool | Show/hide border |

### Comprehensive Customization Example

**C# Example:**
```csharp
sfSmithChart1.Legend.Style.BorderColor = Color.Red;
sfSmithChart1.Legend.Style.BorderWidth = 7;
sfSmithChart1.Legend.Style.BackColor = Color.LightBlue;
sfSmithChart1.Legend.ItemSpacing = 50;
sfSmithChart1.Legend.Spacing = 5;
sfSmithChart1.Legend.BorderVisible = true;
sfSmithChart1.Legend.IconType = SmithChartLegendIconType.Pentagon;
sfSmithChart1.Legend.IconHeight = 13;
sfSmithChart1.Legend.IconWidth = 13;
sfSmithChart1.Legend.Style.ForeColor = Color.BlueViolet;
```

**VB.NET Example:**
```vb
sfSmithChart1.Legend.Style.BorderColor = Color.Red
sfSmithChart1.Legend.Style.BorderWidth = 7
sfSmithChart1.Legend.Style.BackColor = Color.LightBlue
sfSmithChart1.Legend.ItemSpacing = 50
sfSmithChart1.Legend.Spacing = 5
sfSmithChart1.Legend.BorderVisible = True
sfSmithChart1.Legend.IconType = SmithChartLegendIconType.Pentagon
sfSmithChart1.Legend.IconHeight = 13
sfSmithChart1.Legend.IconWidth = 13
sfSmithChart1.Legend.Style.ForeColor = Color.BlueViolet
```

This creates a legend with:
- Red border (7 pixels thick)
- Light blue background
- Blue-violet text
- Pentagon-shaped icons
- 50-pixel spacing between items
- 5-pixel spacing from border

## Toggle Series Visibility

Enable interactive legend functionality to show/hide series by clicking legend items.

**C# Example:**
```csharp
sfSmithChart1.Legend.ToggleSeriesVisible = true;
```

**VB.NET Example:**
```vb
sfSmithChart1.Legend.ToggleSeriesVisible = True
```

**Behavior:**
- Click a legend item to hide its corresponding series
- Click again to show the series
- Useful for comparing different data sets interactively

## Smart View

For charts with many series, the legend provides automatic scrolling and wrapping capabilities.

### Scrollbar

When the chart area cannot accommodate all legend items:

**Features:**
- Scrollbar automatically appears when needed
- Enables viewing all legend items
- No configuration required

**Example Scenario:**
- Chart with 7+ series
- Limited chart dimensions
- Legend items exceed available space
- Scrollbar appears automatically

### Wrap Items

Wrap legend items to multiple rows/columns instead of scrolling.

**C# Example:**
```csharp
sfSmithChart1.Legend.WrapItems = true;
```

**VB.NET Example:**
```vb
sfSmithChart1.Legend.WrapItems = True
```

**Behavior:**
- Items wrap within ~20% of chart area
- Vertical scrollbar appears if items exceed available height (for Top/Bottom positions)
- Horizontal scrollbar appears if items exceed available width (for Left/Right positions)

**When to Use:**
- Multiple series need simultaneous visibility in legend
- Scrolling is less desirable than wrapping
- Chart has sufficient height/width to accommodate wrapped items

## Common Patterns

### Pattern 1: Basic Legend Setup

```csharp
// Enable legend with series names
sfSmithChart.Legend.Visible = true;

LineSeries series1 = new LineSeries();
series1.LegendText = "Measured Data";
sfSmithChart.Series.Add(series1);

LineSeries series2 = new LineSeries();
series2.LegendText = "Simulated Data";
sfSmithChart.Series.Add(series2);
```

### Pattern 2: Bottom Legend with Interactive Toggle

```csharp
sfSmithChart.Legend.Visible = true;
sfSmithChart.Legend.DockPosition = DockPosition.Bottom;
sfSmithChart.Legend.ToggleSeriesVisible = true;
sfSmithChart.Legend.Alignment = ChartAlignment.Center;
```

### Pattern 3: Styled Legend

```csharp
sfSmithChart.Legend.Visible = true;
sfSmithChart.Legend.Style.BackColor = Color.WhiteSmoke;
sfSmithChart.Legend.Style.BorderColor = Color.DarkGray;
sfSmithChart.Legend.Style.BorderWidth = 2;
sfSmithChart.Legend.BorderVisible = true;
sfSmithChart.Legend.IconType = SmithChartLegendIconType.Diamond;
sfSmithChart.Legend.ItemSpacing = 20;
```

### Pattern 4: Compact Legend for Many Series

```csharp
sfSmithChart.Legend.Visible = true;
sfSmithChart.Legend.WrapItems = true;
sfSmithChart.Legend.IconHeight = 8;
sfSmithChart.Legend.IconWidth = 8;
sfSmithChart.Legend.ItemSpacing = 10;
sfSmithChart.Legend.Spacing = 2;
```

### Pattern 5: Right-Aligned Vertical Legend

```csharp
sfSmithChart.Legend.Visible = true;
sfSmithChart.Legend.DockPosition = DockPosition.Right;
sfSmithChart.Legend.Alignment = ChartAlignment.Near;
sfSmithChart.Legend.ItemSpacing = 15;
```

## Best Practices

1. **Meaningful Names:** Use clear, descriptive text for `LegendText` that identifies each series

2. **Position Choice:** 
   - Use Top/Bottom for charts wider than tall
   - Use Left/Right for charts taller than wide

3. **Interactive Legends:** Enable `ToggleSeriesVisible` for charts where users need to compare subsets of data

4. **Icon Consistency:** Match icon type to series marker type for visual consistency

5. **Wrapping vs Scrolling:** 
   - Use `WrapItems = true` for 3-6 series
   - Rely on scrollbar for 7+ series

6. **Spacing:** Adjust `ItemSpacing` based on text length; longer names need more space

7. **Contrast:** Ensure sufficient contrast between `ForeColor` and `BackColor`

8. **Border:** Use borders when legend background color is similar to chart background

## Troubleshooting

### Legend Not Appearing

- Verify `Legend.Visible = true` is set
- Ensure each series has `LegendText` property defined
- Check that series are actually added to `sfSmithChart.Series` collection

### Legend Items Cut Off

- Increase chart size to provide more space
- Enable `WrapItems = true` to wrap items
- Rely on automatic scrollbar for many items

### Toggle Not Working

- Ensure `ToggleSeriesVisible = true` is set
- Verify series have valid data

### Icons Not Matching Series

- Use same `MarkerType` on series as `IconType` on legend for consistency
- Or intentionally use different shapes for clear distinction
