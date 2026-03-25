---
name: syncfusion-winforms-sparkline
description: "Implement Syncfusion Windows Forms Sparkline controls for compact data visualization. Use this when working with sparklines, trend displays in condensed format, or high-density data visualization. This skill covers line, column, and WinLoss chart types, data point markers, high/low value highlighting, and lightweight graphical representations in Windows Forms applications."
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion Windows Forms Sparkline

The Windows Forms Sparkline control provides compact, high-density data visualization for displaying trends and variations in a condensed graphical format. Sparklines present data without axis scales, offering quick overviews of data patterns over time with minimal space requirements.

## When to Use This Skill

Use this skill when you need to:
- Create compact trend visualizations within Windows Forms applications
- Display high-density data in small spaces (table cells, dashboards, reports)
- Show data patterns without detailed axis scales
- Implement line, column, or win/loss sparkline charts
- Highlight specific data points (high, low, start, end, negative values)
- Add visual markers to emphasize important data points
- Bind sparklines to various data sources (DataTable, IEnumerable, ICollection, IList)
- Customize sparkline appearance and styling
- Provide quick data range comparisons
- Build lightweight graphical representations for trends and variations

## Component Overview

**Syncfusion Windows Forms Sparkline** is an information graphic control characterized by:
- **Small size**: Minimal footprint for compact displays
- **High data density**: Maximum information in minimal space
- **Lightweight rendering**: Fast performance with efficient graphics
- **Three chart types**: Line, Column, and WinLoss
- **Marker support**: Visual indicators for key data points
- **Flexible data binding**: Multiple data source types supported

**Key Capabilities:**
- Three sparkline types (Line, Column, WinLoss)
- Data point markers with customizable colors
- High/Low/Start/End/Negative point highlighting
- Custom styling for lines and columns
- Background customization
- Data source binding from multiple formats
- Designer integration with Visual Studio toolbox

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly deployment and NuGet package installation
- Adding Sparkline control to Windows Forms via designer
- Toolbox integration
- Basic data source binding
- First sparkline implementation
- Adding markers to sparklines
- Highlighting high and low values

### Sparkline Types
📄 **Read:** [references/sparkline-types.md](references/sparkline-types.md)
- Overview of three sparkline types
- Line sparkline: connected data points
- Column sparkline: vertical bars with positive/negative representation
- WinLoss sparkline: equal-length columns for win/loss patterns
- Data source support (DataTable, IEnumerable, ICollection, IList)
- Type selection guidance based on use case

### Marker Customization
📄 **Read:** [references/marker-customization.md](references/marker-customization.md)
- Marker properties and configuration
- ShowMarker, ShowHighPoint, ShowLowPoint, ShowStartPoint, ShowEndPoint, ShowNegativePoint
- Marker colors for each point type
- Line sparkline markers (all data points)
- Column sparkline markers (high/low/start/end/negative)
- WinLoss sparkline markers
- Gradient support with BrushInfo
- Complete marker customization examples

### Appearance Customization
📄 **Read:** [references/appearance-customization.md](references/appearance-customization.md)
- LineStyle customization (color, appearance)
- ColumnStyle customization (color, style)
- BackInterior property (background color)
- Complete styling examples

## Quick Start Example

```csharp
using Syncfusion.Windows.Forms.Chart;

// Create sparkline instance
SparkLine sparkLine1 = new SparkLine();

// Set data source
sparkLine1.Source = new double[] { 30, -20, 80, 20, 40, -50, -30, 70, -40, 50 };

// Set sparkline type (Line, Column, or WinLoss)
sparkLine1.Type = SparkLineType.Line;

// Enable markers for high and low points
sparkLine1.Markers.ShowHighPoint = true;
sparkLine1.Markers.ShowLowPoint = true;
sparkLine1.Markers.HighPointColor = new BrushInfo(Color.Blue);
sparkLine1.Markers.LowPointColor = new BrushInfo(Color.Red);

// Customize line appearance
sparkLine1.LineStyle.LineColor = Color.Maroon;

// Add to form
this.Controls.Add(sparkLine1);
```

## Common Patterns

### Pattern 1: Dashboard Trend Indicators
```csharp
// Small sparklines for dashboard KPI trends
SparkLine salesTrend = new SparkLine();
salesTrend.Source = salesData; // double[] or DataTable
salesTrend.Type = SparkLineType.Line;
salesTrend.Markers.ShowEndPoint = true;
salesTrend.Markers.EndPointColor = new BrushInfo(Color.Green);
salesTrend.Size = new Size(100, 30); // Compact size
```

### Pattern 2: Win/Loss Visualization
```csharp
// WinLoss sparkline for binary outcomes
SparkLine winLoss = new SparkLine();
winLoss.Source = new double[] { 1, -1, 1, 1, -1, 1, -1, -1, 1, 1 };
winLoss.Type = SparkLineType.WinLoss;
winLoss.Markers.ShowNegativePoint = true;
winLoss.Markers.NegativePointColor = new BrushInfo(Color.Red);
```

### Pattern 3: Column Sparkline with Highlights
```csharp
// Column sparkline with high/low highlighting
SparkLine columnChart = new SparkLine();
columnChart.Source = monthlyData;
columnChart.Type = SparkLineType.Column;
columnChart.Markers.ShowHighPoint = true;
columnChart.Markers.ShowLowPoint = true;
columnChart.Markers.ShowStartPoint = true;
columnChart.Markers.ShowEndPoint = true;
columnChart.Markers.HighPointColor = new BrushInfo(Color.Green);
columnChart.Markers.LowPointColor = new BrushInfo(Color.Red);
```

### Pattern 4: Line Sparkline with All Markers
```csharp
// Line sparkline showing all data point markers
SparkLine lineWithMarkers = new SparkLine();
lineWithMarkers.Source = dataPoints;
lineWithMarkers.Type = SparkLineType.Line;
lineWithMarkers.Markers.ShowMarker = true; // Show all points
lineWithMarkers.Markers.MarkerColor = new BrushInfo(Color.DarkBlue);
lineWithMarkers.LineStyle.LineColor = Color.Blue;
```

## Key Properties and Methods

### Essential Properties

**Type** (SparkLineType)
- Specifies sparkline type: `Line`, `Column`, or `WinLoss`
- Default: `Line`
- Use: Determines visual representation style

**Source** (object)
- Gets or sets data source for sparkline data points
- Supports: `double[]`, `DataTable`, `IEnumerable`, `ICollection`, `IList`
- Use: Binds data to sparkline visualization

**Markers** (Markers)
- Enables marker support for sparklines
- Properties: `ShowMarker`, `ShowHighPoint`, `ShowLowPoint`, `ShowStartPoint`, `ShowEndPoint`, `ShowNegativePoint`
- Color properties: `MarkerColor`, `HighPointColor`, `LowPointColor`, `StartPointColor`, `EndPointColor`, `NegativePointColor`
- Use: Highlights specific data points visually

**LineStyle** (LineStyle)
- Customizes line sparkline appearance
- Properties include: `LineColor`
- Use: Controls visual styling for line-type sparklines

**ColumnStyle** (ColumnStyle)
- Customizes column and WinLoss sparkline appearance
- Use: Controls visual styling for column-type sparklines

**BackInterior** (BrushInfo)
- Customizes background color of control
- Default: White
- Use: Sets sparkline background appearance

### Key Methods

**GetHighPoint()** - Retrieves highest point value from sparkline

**GetLowPoint()** - Retrieves lowest point value from sparkline

**GetStartPoint()** - Retrieves start point value from sparkline

**GetEndPoint()** - Retrieves end point value from sparkline

## Common Use Cases

### Use Case 1: Embedded Table Trends
Place sparklines in DataGridView cells to show trends alongside tabular data without expanding table width.

### Use Case 2: Dashboard KPIs
Display multiple compact sparklines on dashboards to show key performance indicator trends at a glance.

### Use Case 3: Financial Data Visualization
Use line sparklines to show stock price movements, column sparklines for volume data, and win/loss for gain/loss patterns.

### Use Case 4: Performance Metrics
Show server performance, response times, or system metrics over time in minimal space.

### Use Case 5: Comparison Widgets
Create side-by-side sparkline comparisons for A/B testing results, campaign performance, or metric comparisons.