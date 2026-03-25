---
name: syncfusion-winforms-smith-chart
description: Implement Syncfusion Windows Forms Smith Chart (SfSmithChart) control for visualizing impedance and admittance of transmission lines. Use this when working with high-frequency circuit applications, RF engineering, or transmission line analysis. Covers normalized resistance/reactance plotting, normalized conductance/susceptance visualization, series configuration, axes customization, markers, legends, and tooltips.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Smith Charts

The Syncfusion Windows Forms Smith chart is one of the most useful data visualization tools for high frequency circuit applications. It contains two sets of circles to plot the parameters of transmission lines.

## When to Use This Skill

Use this skill when the user needs to:
- Visualize impedance or admittance of transmission lines
- Create Smith Charts for high-frequency circuit applications
- Plot normalized resistance and reactance values
- Display normalized conductance and susceptance values
- Build RF engineering visualization tools
- Analyze transmission line parameters
- Create interactive circuit analysis charts
- Implement line series with data markers on circular plots

**Key indicators:** User mentions "Smith Chart", "SfSmithChart", "impedance", "admittance", "transmission line", "reactance", "resistance", "RF", "high-frequency circuit", or references circuit parameter visualization.

## Component Overview

The Syncfusion Windows Forms Smith Chart (SfSmithChart) is a specialized data visualization control for plotting transmission line parameters. It consists of two families of circles:
- **Impedance mode:** Normalized resistance circles and reactance curves
- **Admittance mode:** Normalized conductance circles and susceptance curves

### Key Features

- **Dual Rendering Modes:** Switch between impedance and admittance visualization
- **Line Series:** Plot data with customizable line styles and colors
- **Data Markers:** Various shapes (circle, rectangle, diamond) with image support
- **Interactive Legends:** Toggle series visibility, customizable positioning
- **Tooltips:** Configurable tooltips with custom formatting
- **Axes Customization:** Control major/minor gridlines, labels, and styling
- **Data Labels:** Smart alignment with automatic connector lines
- **Appearance:** Multiple color palettes and full styling control
- **Performance:** Optimized for large datasets with BeginUpdate/EndUpdate

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly deployment and NuGet package installation
- Adding Smith Chart via Visual Studio designer
- Adding Smith Chart programmatically through code
- Required namespace imports
- Data binding with DataSource, ResistanceMember, ReactanceMember
- Adding data points directly to series
- Basic chart initialization and first render
- Adding title with Text property

### Series and Data Configuration
📄 **Read:** [references/series-configuration.md](references/series-configuration.md)
- LineSeries creation and configuration
- Data binding properties (DataSource, ResistanceMember, ReactanceMember)
- Line customization (Interior color, StrokeWidth, DashStyle)
- Series visibility control
- Data plotting with ArrangeByIndex property
- Multiple series support and management

### Data Markers
📄 **Read:** [references/data-markers.md](references/data-markers.md)
- Enabling markers with MarkerVisible property
- Marker shapes (Rectangle, Circle, Diamond, etc.)
- Marker customization (size, colors, borders)
- Using images as custom markers
- Data labels overview and smart alignment
- Automatic connector lines for label collision avoidance

### Axes Configuration
📄 **Read:** [references/axes-configuration.md](references/axes-configuration.md)
- Horizontal axis for resistance/conductance values
- Radial axis for reactance/susceptance values
- Major gridlines visibility and styling
- Minor gridlines configuration and count
- Axis line appearance customization
- Label placement (inside/outside chart area)
- Label intersect action for overlapping labels
- LabelCreated event for custom label text

### Legend Customization
📄 **Read:** [references/legend-customization.md](references/legend-customization.md)
- Legend visibility and LegendText for series
- Positioning with DockPosition (Top, Bottom, Left, Right)
- Legend icon types and sizing
- Alignment options (Near, Far, Center)
- Style customization (colors, borders, spacing)
- Interactive toggle for series visibility
- Scrollbar and WrapItems for multiple series

### Appearance and Styling
📄 **Read:** [references/appearance-styling.md](references/appearance-styling.md)
- Chart color palettes (Metro, Nature, etc.)
- Series-level palette customization
- Chart area background and border styling
- Circle radius adjustment
- BackColor and ChartAreaBackColor properties

### User Interactions
📄 **Read:** [references/user-interactions.md](references/user-interactions.md)
- Tooltip visibility and configuration
- Tooltip formatting with placeholders
- TooltipOptions for appearance customization
- TooltipOpening event for dynamic content
- Custom tooltip text and styling

### Rendering Modes
📄 **Read:** [references/rendering-modes.md](references/rendering-modes.md)
- Impedance vs Admittance modes
- RenderingMode property configuration
- Understanding resistance/reactance circles
- Understanding conductance/susceptance circles
- Mode-specific data interpretation

### Performance Optimization
📄 **Read:** [references/performance-optimization.md](references/performance-optimization.md)
- BeginUpdate and EndUpdate methods
- Optimizing for large datasets
- Best practices for adding multiple points
- Suspending chart repaints during updates

## Quick Start Example

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using System.Collections.ObjectModel;
using Syncfusion.WinForms.SmithChart;

public class SmithChartExample : Form
{
    public SmithChartExample()
    {
        // Create Smith Chart
        SfSmithChart chart = new SfSmithChart();
        chart.Text = "Impedance Transmission";
        chart.BackColor = Color.White;  
        
        // Configure axes
        chart.HorizontalAxis.MinorGridlinesVisible = true;
        chart.RadialAxis.MinorGridlinesVisible = true;
        
        // Create series with data
        LineSeries series = new LineSeries();
        series.MarkerVisible = true;
        series.LegendText = "Transmission1";
        series.TooltipVisible = true;
        series.DataSource = GetTransmissionData();
        series.ResistanceMember = "Resistance";
        series.ReactanceMember = "Reactance";
        
        // Add series to chart
        chart.Series.Add(series);
        
        // Show legend
        chart.Legend.Visible = true;
        
        // Add chart to form
        this.Controls.Add(chart);
    }
    
    private ObservableCollection<TransmissionData> GetTransmissionData()
    {
        var data = new ObservableCollection<TransmissionData>();
        data.Add(new TransmissionData { Resistance = 0, Reactance = 0.05 });
        data.Add(new TransmissionData { Resistance = 0.3, Reactance = 0.1 });
        data.Add(new TransmissionData { Resistance = 0.5, Reactance = 0.2 });
        data.Add(new TransmissionData { Resistance = 1.0, Reactance = 0.4 });
        data.Add(new TransmissionData { Resistance = 2.0, Reactance = 0.5 });
        data.Add(new TransmissionData { Resistance = 3.5, Reactance = 0.0 });
        data.Add(new TransmissionData { Resistance = 5, Reactance = -1.0 });
        data.Add(new TransmissionData { Resistance = 10, Reactance = -10 });
        return data;
    }
}

public class TransmissionData
{
    public double Resistance { get; set; }
    public double Reactance { get; set; }
}
```

## Common Patterns

### Adding Data via DataSource

When you have a collection of data objects:

```csharp
// Create data model
var model = new SmithChartModel();

// Configure series
LineSeries series = new LineSeries();
series.DataSource = model.TransmissionData;
series.ResistanceMember = "Resistance";  // Property name for X-axis
series.ReactanceMember = "Reactance";    // Property name for Y-axis
sfSmithChart.Series.Add(series);
```

### Adding Data Points Directly

For dynamic data without a predefined collection:

```csharp
LineSeries series = new LineSeries();
series.MarkerVisible = true;

// Add individual points
series.Points.Add(0.5, 0.2);
series.Points.Add(1.0, 0.4);
series.Points.Add(2.0, 0.5);

sfSmithChart.Series.Add(series);
```

### Switching Rendering Modes

Toggle between impedance and admittance:

```csharp
// Impedance mode (default) - shows resistance and reactance
sfSmithChart.RenderingMode = RenderingMode.Impedance;

// Admittance mode - shows conductance and susceptance
sfSmithChart.RenderingMode = RenderingMode.Admittance;
```

### Customizing Series Appearance

```csharp
LineSeries series = new LineSeries();
series.Interior = Color.Red;           // Line color
series.StrokeWidth = 3;                // Line thickness
series.DashStyle = DashStyle.Dash;     // Line pattern
series.MarkerVisible = true;           // Show markers
series.MarkerType = MarkerType.Circle; // Marker shape
series.MarkerBackColor = Color.Blue;   // Marker fill
```

### Performance for Large Datasets

```csharp
sfSmithChart.BeginUpdate();  // Suspend repainting

LineSeries series = sfSmithChart.Series[0] as LineSeries;
for (int i = 0; i < 1000; i++)
{
    series.Points.Add(GetResistance(i), GetReactance(i));
}

sfSmithChart.EndUpdate();  // Resume and repaint once
```

## Key Properties

### SfSmithChart Properties

| Property | Type | Description |
|----------|------|-------------|
| `Text` | string | Chart title displayed at the top |
| `RenderingMode` | RenderingMode | Impedance or Admittance mode |
| `HorizontalAxis` | ChartAxis | Horizontal axis configuration |
| `RadialAxis` | ChartAxis | Radial axis configuration |
| `Series` | SeriesCollection | Collection of chart series |
| `Legend` | ChartLegend | Legend configuration |
| `Radius` | float | Circle diameter (0.1 to 1.0, default 0.95) |
| `BackColor` | Color | Chart background color |

### LineSeries Properties

| Property | Type | Description |
|----------|------|-------------|
| `DataSource` | object | Data collection to bind |
| `ResistanceMember` | string | Property for resistance/conductance values |
| `ReactanceMember` | string | Property for reactance/susceptance values |
| `Interior` | Color | Line color |
| `StrokeWidth` | int | Line thickness |
| `DashStyle` | DashStyle | Line pattern (Solid, Dash, Dot, etc.) |
| `MarkerVisible` | bool | Show/hide data markers |
| `MarkerType` | MarkerType | Marker shape |
| `TooltipVisible` | bool | Enable tooltips |
| `LegendText` | string | Text shown in legend |
| `Visible` | bool | Show/hide series |
| `ArrangeByIndex` | bool | Plot by index vs sorted by resistance |

### ChartAxis Properties

| Property | Type | Description |
|----------|------|-------------|
| `MajorGridlinesVisible` | bool | Show/hide major gridlines |
| `MinorGridlinesVisible` | bool | Show/hide minor gridlines |
| `MinorGridlinesCount` | int | Number of minor gridlines between major |
| `LabelPlacement` | LabelPlacement | Inside or Outside chart area |
| `LabelIntersectAction` | LabelIntersectActions | Handle overlapping labels (Hide) |
| `AxisLineVisible` | bool | Show/hide axis line |

## Common Use Cases

### High-Frequency Circuit Analysis

Use impedance mode to analyze RF circuit behavior:
- Plot measured S-parameters
- Visualize matching network performance
- Analyze antenna impedance characteristics

### Transmission Line Visualization

Display transmission line parameters:
- Show normalized impedance along a transmission line
- Plot reflection coefficients
- Visualize VSWR circles

### Multiple Trace Comparison

Compare different circuit configurations:
- Add multiple series with different LegendText
- Use different colors for each series
- Enable legend with ToggleSeriesVisible for interactive comparison

### Interactive Data Exploration

Provide users with exploration tools:
- Enable tooltips with custom formatting
- Use TooltipOpening event for dynamic content
- Add data labels for key points
- Allow legend-based series toggling

### Custom Marker Visualization

Highlight specific data points:
- Use different marker types per series
- Add image markers for special points
- Customize marker sizes and colors

## Troubleshooting

### Data Not Appearing

- Ensure `DataSource` is set and contains valid data
- Verify `ResistanceMember` and `ReactanceMember` match property names exactly (case-sensitive)
- Check that data values are within valid ranges
- Confirm series is added to `sfSmithChart.Series` collection

### Performance Issues with Large Datasets

- Wrap point additions in `BeginUpdate()` / `EndUpdate()` blocks
- Disable markers if not needed (`MarkerVisible = false`)
- Consider data sampling for extremely large datasets

### Labels Overlapping

- Use `LabelIntersectAction = LabelIntersectActions.Hide` to auto-hide overlapping labels
- Adjust chart size or use `LabelPlacement.Inside` for more space
- Smart alignment automatically handles data label collisions

### Legend Not Showing

- Set `chart.Legend.Visible = true`
- Ensure each series has `LegendText` property set
- Check that chart has enough space for legend based on `DockPosition`
