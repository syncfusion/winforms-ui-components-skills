---
name: syncfusion-winforms-chart
description: "Guide for implementing Syncfusion Windows Forms Chart control with 35+ chart types, data binding, series configuration, axes customization, and interactive features. Use this when working with Windows Forms charts, ChartControl, chart types, or data visualization in WinForms. This skill covers chart series, chart axes, data binding, legends, styling, and all chart types including Area, Bar, Line, Pie, Column, Bubble, Candle, financial charts, and statistical charts for Windows Forms applications (.NET Framework)."
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion Windows Forms Charts

Essential Chart for Windows Forms is a comprehensive charting control supporting 35+ chart types for business applications. It features extensive customization, data binding, interactive capabilities, and export functionality for creating presentation-quality charts.

## When to Use This Skill

Use this skill when you need to:
- Create any type of chart in Windows Forms applications
- Visualize data with line, area, bar, column, pie, or financial charts
- Implement interactive charts with zoom, pan, or tooltips
- Bind chart data from DataSet, DataTable, or custom data sources
- Configure chart axes, legends, titles, or appearance
- Export charts to PDF, Excel, Word, or images
- Add statistical analysis or formulas to charts
- Customize chart styling, colors, or themes

## Component Overview

**ChartControl** is the main component providing:
- **35+ chart types:** Area, Bar, Bubble, Candle, Column, Combination, Funnel, Gantt, Histogram, Line, Pie, Pyramid, and more
- **Flexible data binding:** DataSet, DataTable, DataView, custom models via interfaces
- **Multiple series support:** Overlay multiple data series with different types
- **Advanced axes:** Primary/secondary axes, DateTime support, logarithmic scales, custom intervals
- **Rich customization:** Skins, palettes, fonts, gradients, shadows
- **Interactive features:** Zooming, panning, tooltips, hit testing, runtime modification
- **Export capabilities:** PDF, Excel, Word, images, printing with preview
- **Statistical support:** Built-in formulas for analysis

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly deployment and NuGet packages
- Adding ChartControl to Windows Forms
- Basic data population with BindingList
- Design-time ChartWizard configuration
- First chart example

### Chart Types
📄 **Read:** [references/chart-types.md](references/chart-types.md)
- Overview of 35+ available chart types
- Chart type categories and use cases
- Y-value requirements per type
- Setting chart type via Type property
- Type-specific configuration

### Data Population
📄 **Read:** [references/data-population.md](references/data-population.md)
- Binding DataSet, DataTable, DataView
- ChartDataBindModel and CategoryAxisDataBindModel
- Custom data models via IChartSeriesModel
- Performance optimization for large datasets

### Chart Series
📄 **Read:** [references/chart-series.md](references/chart-series.md)
- Creating and adding ChartSeries
- ChartPoint class for data points
- Managing X and Y values
- Series styling and customization
- Multiple series configuration

### Axes Configuration
📄 **Read:** [references/axes-configuration.md](references/axes-configuration.md)
- Primary and secondary axes (PrimaryXAxis, PrimaryYAxis)
- Axis value types (Double, DateTime, Category, Logarithmic)
- Range and interval settings
- Custom label formatting and positioning
- Multiple axes support

### Chart Area
📄 **Read:** [references/chart-area.md](references/chart-area.md)
- Chart area properties and layout
- Background, borders, margins
- Interior customization
- Shadow effects

### Appearance and Styling
📄 **Read:** [references/appearance-styling.md](references/appearance-styling.md)
- Built-in skins (Metro, Office, etc.)
- Color palettes and CustomPalette
- Gradient and solid color styles
- Font and text formatting
- BrushInfo for advanced styling

### Legend Configuration
📄 **Read:** [references/legend-configuration.md](references/legend-configuration.md)
- Enabling and positioning legends
- LegendPosition and LegendAlignment
- Inside vs outside placement
- Custom legend items
- Multiple legends

### Chart Title
📄 **Read:** [references/chart-title.md](references/chart-title.md)
- Adding ChartTitle objects
- Title text and positioning
- Font and appearance customization
- Multiple titles support

### Data Labels and Tooltips
📄 **Read:** [references/data-labels-tooltips.md](references/data-labels-tooltips.md)
- Enabling data labels (DisplayText)
- Label formatting and orientation
- Tooltip configuration (ShowToolTips)
- Custom tooltip content via PrepareStyle event
- Appearance customization

### Runtime Features
📄 **Read:** [references/runtime-features.md](references/runtime-features.md)
- Zooming and panning capabilities
- Hit testing for chart regions
- Interactive manipulation
- Dynamic data updates
- Runtime series modification

### Chart Events
📄 **Read:** [references/chart-events.md](references/chart-events.md)
- ChartControl and series events
- PrepareStyle event for customization
- Mouse and interaction events
- Chart region click events
- Event handling patterns

### Exporting and Printing
📄 **Read:** [references/exporting-printing.md](references/exporting-printing.md)
- Export to PDF, Excel, Word
- Image export (PNG, BMP, JPEG)
- Printing and print preview
- Multi-page printing configuration

### Statistical Formulas
📄 **Read:** [references/statistical-formulas.md](references/statistical-formulas.md)
- Built-in statistical functions
- Mean, standard deviation, variance
- Distributions, T-test, F-test, Z-test
- Applying formulas to series data

### Performance Optimization
📄 **Read:** [references/performance-optimization.md](references/performance-optimization.md)
- Large dataset handling strategies
- Data binding performance
- Rendering optimization
- Memory management best practices

### Localization
📄 **Read:** [references/localization.md](references/localization.md)
- Localizing chart text and labels
- Culture-specific formatting
- Resource file configuration
- Date and number formatting

### Troubleshooting
📄 **Read:** [references/troubleshooting.md](references/troubleshooting.md)
- Common issues and solutions
- Data display problems
- Performance issues
- Design-time configuration problems

## Quick Start Example

```csharp
using Syncfusion.Windows.Forms.Chart;
using System.ComponentModel;

// Create a simple data model
public class SalesData
{
    public string Year { get; set; }
    public double Sales { get; set; }
    
    public SalesData(string year, double sales)
    {
        this.Year = year;
        this.Sales = sales;
    }
}

// Populate data
BindingList<SalesData> dataSource = new BindingList<SalesData>();
dataSource.Add(new SalesData("2020", 30));
dataSource.Add(new SalesData("2021", 45));
dataSource.Add(new SalesData("2022", 60));
dataSource.Add(new SalesData("2023", 75));

// Create data model for binding
CategoryAxisDataBindModel dataModel = new CategoryAxisDataBindModel(dataSource);
dataModel.CategoryName = "Year";
dataModel.YNames = new string[] { "Sales" };

// Create and configure series
ChartSeries series = new ChartSeries("Sales");
series.Type = ChartSeriesType.Column;
series.CategoryModel = dataModel;
series.Style.DisplayText = true;

// Configure chart
chartControl1.PrimaryXAxis.ValueType = ChartValueType.Category;
chartControl1.Series.Add(series);
chartControl1.Skins = Skins.Metro;

// Add title
ChartTitle title = new ChartTitle();
title.Text = "Sales Performance";
chartControl1.Titles.Add(title);

// Configure legend
chartControl1.Legend.Visible = true;
chartControl1.LegendAlignment = ChartAlignment.Center;
chartControl1.Legend.Position = ChartDock.Bottom;
```

## Common Patterns

### Pattern 1: Multiple Series with Different Types
```csharp
// Sales as columns
ChartSeries salesSeries = new ChartSeries("Sales");
salesSeries.Type = ChartSeriesType.Column;
salesSeries.Points.Add(1, 100);
salesSeries.Points.Add(2, 150);
salesSeries.Points.Add(3, 120);

// Target as line
ChartSeries targetSeries = new ChartSeries("Target");
targetSeries.Type = ChartSeriesType.Line;
targetSeries.Points.Add(1, 110);
targetSeries.Points.Add(2, 130);
targetSeries.Points.Add(3, 140);

chartControl1.Series.Add(salesSeries);
chartControl1.Series.Add(targetSeries);
```

### Pattern 2: DateTime Axis with Date Data
```csharp
ChartSeries series = new ChartSeries("Temperature");
series.Type = ChartSeriesType.Line;

// Add points with DateTime X values
series.Points.Add(new DateTime(2024, 1, 1), 18.5);
series.Points.Add(new DateTime(2024, 1, 2), 19.2);
series.Points.Add(new DateTime(2024, 1, 3), 17.8);

// Configure X axis for DateTime
chartControl1.PrimaryXAxis.ValueType = ChartValueType.DateTime;
chartControl1.PrimaryXAxis.DateTimeFormat = "dd/MM/yyyy";
chartControl1.Series.Add(series);
```

### Pattern 3: Custom Tooltips
```csharp
chartControl1.ShowToolTips = true;
chartSeries.PrepareStyle += (sender, args) =>
{
    ChartSeries series = sender as ChartSeries;
    ChartPoint point = series.Points[args.Index];
    args.Style.ToolTip = $"Value: {point.YValues[0]:N2}";
};
```

### Pattern 4: Export Chart to PDF
```csharp
// Export to PDF
chartControl1.SaveImage("chart.pdf", ChartImageFormat.PDF);

// Export to image
chartControl1.SaveImage("chart.png", ChartImageFormat.Png);
```

## Key Properties

**ChartControl:**
- `Series` - Collection of ChartSeries to display
- `PrimaryXAxis`, `PrimaryYAxis` - Main axes configuration
- `Legend` - Legend visibility and appearance
- `Titles` - Collection of chart titles
- `Skins` - Apply predefined themes
- `ShowToolTips` - Enable interactive tooltips

**ChartSeries:**
- `Type` - Chart type (Column, Line, Pie, etc.)
- `Points` - Collection of data points
- `CategoryModel` - Data binding model
- `Style` - Visual appearance settings
- `Text` - Series display name

**ChartAxis:**
- `ValueType` - Data type (Double, DateTime, Category, etc.)
- `Range` - Min/max values
- `Interval` - Spacing between labels
- `Title` - Axis title text

## Common Use Cases

1. **Sales Dashboard:** Column/line charts with multiple series for sales trends
2. **Financial Analysis:** Candle/OHLC charts for stock data visualization
3. **Statistical Reports:** Histogram, box plot for data distribution analysis
4. **Performance Monitoring:** Line charts with DateTime axis for time-series data
5. **Comparison Charts:** Bar charts for category comparison
6. **Data Distribution:** Pie/doughnut charts for percentage breakdown
7. **Scientific Data:** Scatter/bubble charts for correlation analysis
8. **Project Management:** Gantt charts for timeline visualization

## Related Skills

- [Implementing Syncfusion Windows Forms Components](../../) - Main library overview
- For other platforms, see parent documentations folder
