# Chart Types in Pivot Chart

This guide covers all 11 chart types supported by the Syncfusion Pivot Chart control, including when to use each type and how to switch between them dynamically.

## Table of Contents
- [Overview](#overview)
- [Chart Type Enumeration](#chart-type-enumeration)
- [Switching Chart Types](#switching-chart-types)
- [Line Charts](#line-charts)
- [Column Charts](#column-charts)
- [Area Charts](#area-charts)
- [Stacking Charts](#stacking-charts)
- [Step Charts](#step-charts)
- [Choosing the Right Chart Type](#choosing-the-right-chart-type)

## Overview

The Pivot Chart control supports 11 different chart types for visualizing multidimensional data. All chart types automatically adapt to your data structure - when you switch types, the bound fields are automatically transformed to match the target visualization.

### Supported Chart Types

1. **Line** - Trends with straight lines
2. **Spline** - Trends with curved lines
3. **Column** - Vertical bars for comparison
4. **Area** - Filled regions showing magnitude
5. **Spline Area** - Smooth filled regions
6. **Stacking Column** - Stacked vertical bars
7. **Stacking Area** - Stacked filled regions
8. **Stacking Column 100** - Stacked bars showing percentages
9. **Stacking Area 100** - Stacked areas showing percentages
10. **Step Line** - Stepped trend lines
11. **Step Area** - Stepped filled regions

## Chart Type Enumeration

Use the `PivotChartTypes` enumeration to set chart types:

```csharp
using Syncfusion.Windows.Forms.PivotChart;

public enum PivotChartTypes
{
    Line,
    Spline,
    Column,
    Area,
    SplineArea,
    StackingArea,
    StackingColumn,
    StackingArea100,
    StackingColumn100,
    StepLine,
    StepArea
}
```

## Switching Chart Types

### Setting Chart Type on Initialization

```csharp
// Set during control creation
pivotChart1 = new PivotChart();
pivotChart1.ChartTypes = PivotChartTypes.Column;
```

### Changing Chart Type Dynamically

```csharp
// C# - Change chart type at runtime
pivotChart1.ChartTypes = PivotChartTypes.Line;
```

```vb
' VB.NET - Change chart type at runtime
pivotChart1.ChartTypes = PivotChartTypes.Line
```

### User-Driven Chart Type Selection

```csharp
// Add ComboBox or buttons for user selection
private void cmbChartType_SelectedIndexChanged(object sender, EventArgs e)
{
    switch (cmbChartType.SelectedItem.ToString())
    {
        case "Line":
            pivotChart1.ChartTypes = PivotChartTypes.Line;
            break;
        case "Column":
            pivotChart1.ChartTypes = PivotChartTypes.Column;
            break;
        case "Area":
            pivotChart1.ChartTypes = PivotChartTypes.Area;
            break;
        case "Stacking Column":
            pivotChart1.ChartTypes = PivotChartTypes.StackingColumn;
            break;
        // Add other types...
    }
}
```

### Toolbar with Chart Type Buttons

```csharp
private void CreateChartTypeToolbar()
{
    ToolStrip toolbar = new ToolStrip();
    
    // Line chart button
    ToolStripButton btnLine = new ToolStripButton("Line");
    btnLine.Click += (s, e) => pivotChart1.ChartTypes = PivotChartTypes.Line;
    toolbar.Items.Add(btnLine);
    
    // Column chart button
    ToolStripButton btnColumn = new ToolStripButton("Column");
    btnColumn.Click += (s, e) => pivotChart1.ChartTypes = PivotChartTypes.Column;
    toolbar.Items.Add(btnColumn);
    
    // Area chart button
    ToolStripButton btnArea = new ToolStripButton("Area");
    btnArea.Click += (s, e) => pivotChart1.ChartTypes = PivotChartTypes.Area;
    toolbar.Items.Add(btnArea);
    
    // Add toolbar to form
    this.Controls.Add(toolbar);
}
```

## Line Charts

### Line Chart

**Description:** Connects data points with straight lines to show trends over equal intervals.

**Best for:**
- Time-series data
- Trend analysis
- Continuous data
- Comparing multiple series

**Code:**
```csharp
pivotChart1.ChartTypes = PivotChartTypes.Line;
```

**Characteristics:**
- Clean, simple visualization
- Good for showing trends
- Easy to compare multiple series
- Works well with many data points

**Example Use Cases:**
- Sales trends over months/quarters
- Stock price movements
- Temperature changes over time
- Website traffic analytics

### Spline Chart

**Description:** Similar to line chart but uses smooth curved lines instead of straight segments.

**Best for:**
- Natural data progressions
- Smooth trend visualization
- Aesthetic presentations
- Continuous processes

**Code:**
```csharp
pivotChart1.ChartTypes = PivotChartTypes.Spline;
```

**Characteristics:**
- Aesthetically pleasing curves
- Smooth transitions between points
- Better for continuous data
- May smooth out actual variations

**Example Use Cases:**
- Product lifecycle curves
- Natural phenomena (temperature, humidity)
- Growth curves
- Performance metrics

**Line vs Spline:**
- Use **Line** for precise point-to-point visualization
- Use **Spline** for smooth, continuous trends

### Step Line Chart

**Description:** Uses horizontal and vertical lines creating a step-like progression between data points.

**Best for:**
- Discrete changes
- Inventory levels
- Status transitions
- Price tiers

**Code:**
```csharp
pivotChart1.ChartTypes = PivotChartTypes.StepLine;
```

**Characteristics:**
- Shows exact point where values change
- No interpolation between points
- Clear discrete transitions
- Good for non-continuous data

**Example Use Cases:**
- Inventory stock levels
- Price changes over time
- Software version adoptions
- Status level transitions

## Column Charts

### Column Chart

**Description:** Displays data as vertical bars, ideal for comparing values across categories.

**Best for:**
- Category comparison
- Discrete data points
- Ranking visualization
- Period-over-period comparison

**Code:**
```csharp
pivotChart1.ChartTypes = PivotChartTypes.Column;
```

**Characteristics:**
- Clear visual comparison
- Easy to read values
- Works well with few to moderate data points
- Excellent for side-by-side comparison

**Example Use Cases:**
- Sales by product category
- Revenue by region
- Survey results
- Performance metrics by team

**Best Practices:**
- Limit to 10-15 categories for readability
- Use consistent colors for related data
- Start Y-axis at zero for accurate comparison
- Add data labels for precise values

## Area Charts

### Area Chart

**Description:** Similar to line chart but fills the area between line and axis, emphasizing magnitude of change.

**Best for:**
- Showing cumulative totals
- Emphasizing magnitude
- Part-to-whole relationships
- Volume over time

**Code:**
```csharp
pivotChart1.ChartTypes = PivotChartTypes.Area;
```

**Characteristics:**
- Emphasizes volume/magnitude
- Good for showing trends with context
- Filled area provides visual weight
- Can be harder to read with multiple overlapping series

**Example Use Cases:**
- Total sales volume over time
- Market share evolution
- Resource consumption
- Population growth

### Spline Area Chart

**Description:** Combines smooth spline curves with filled areas.

**Best for:**
- Smooth continuous data with magnitude emphasis
- Aesthetic presentations of trends
- Natural phenomena visualization

**Code:**
```csharp
pivotChart1.ChartTypes = PivotChartTypes.SplineArea;
```

**Characteristics:**
- Smooth, visually appealing curves
- Filled areas show magnitude
- Good for presentations
- Best with single or few series

**Example Use Cases:**
- Product adoption curves with volume
- Natural resource consumption
- Environmental data (temperature, precipitation)

### Step Area Chart

**Description:** Combines step line progression with filled areas.

**Best for:**
- Discrete changes with magnitude
- Inventory history
- Pricing tiers with volume

**Code:**
```csharp
pivotChart1.ChartTypes = PivotChartTypes.StepArea;
```

**Characteristics:**
- Shows exact change points
- Fills area to emphasize magnitude
- Clear discrete transitions
- Good for non-continuous data

**Example Use Cases:**
- Inventory levels with fill
- Tiered pricing structures
- Step-function data with volume

## Stacking Charts

Stacking charts show how individual components contribute to a total value.

### Stacking Column Chart

**Description:** Vertical bars stacked on top of each other, showing part-to-whole relationships.

**Best for:**
- Comparing total values across categories
- Showing composition breakdown
- Part-to-whole analysis
- Budget allocation

**Code:**
```csharp
pivotChart1.ChartTypes = PivotChartTypes.StackingColumn;
```

**Characteristics:**
- Shows individual contributions
- Displays total height
- Easy comparison of totals
- Can be hard to compare middle segments

**Example Use Cases:**
- Sales by product category per region
- Revenue breakdown by source
- Cost allocation across departments
- Market share by competitor per year

**Configuration Example:**
```csharp
// Configure for stacking
pivotChart1.ChartTypes = PivotChartTypes.StackingColumn;

// Axis: Regions
pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "Region", TotalHeader = "All Regions" });

// Legend: Product Categories (will be stacked)
pivotChart1.PivotLegend.Add(new PivotItem { FieldMappingName = "Category", TotalHeader = "All Categories" });

// Calculation: Revenue
pivotChart1.PivotCalculations.Add(new PivotComputationInfo { FieldName = "Revenue", Format = "C0" });
```

### Stacking Area Chart

**Description:** Similar to area chart but series stack on top of each other.

**Best for:**
- Showing how components contribute to total over time
- Cumulative trends
- Volume composition

**Code:**
```csharp
pivotChart1.ChartTypes = PivotChartTypes.StackingArea;
```

**Characteristics:**
- Shows cumulative total trend
- Individual series contributions visible
- Good for time-series composition
- Bottom series easiest to read

**Example Use Cases:**
- Revenue sources over time
- Energy consumption by source
- Traffic sources to website
- Resource allocation trends

### Stacking Column 100 Chart

**Description:** Stacked columns normalized to 100%, showing relative proportions.

**Best for:**
- Comparing proportions across categories
- Percentage distribution
- Relative contribution analysis
- Market share comparison

**Code:**
```csharp
pivotChart1.ChartTypes = PivotChartTypes.StackingColumn100;
```

**Characteristics:**
- All columns same height (100%)
- Shows relative proportions
- Easy to compare percentages
- Absolute values not visible (only percentages)

**Example Use Cases:**
- Market share distribution per quarter
- Budget allocation percentages
- Survey response distributions
- Product mix percentages

**When to Use:**
```csharp
// Use StackingColumn when absolute values matter
pivotChart1.ChartTypes = PivotChartTypes.StackingColumn;
// Shows: North=$100k, South=$150k, East=$200k

// Use StackingColumn100 when proportions matter
pivotChart1.ChartTypes = PivotChartTypes.StackingColumn100;
// Shows: North=22%, South=33%, East=45%
```

### Stacking Area 100 Chart

**Description:** Stacked areas normalized to 100%, showing how proportions change over time.

**Best for:**
- Proportion trends over time
- Market share evolution
- Composition changes
- Percentage distributions

**Code:**
```csharp
pivotChart1.ChartTypes = PivotChartTypes.StackingArea100;
```

**Characteristics:**
- Always fills 0-100% range
- Shows proportion changes clearly
- Good for comparing relative contributions
- Absolute values not shown

**Example Use Cases:**
- Market share trends over quarters
- Product mix evolution
- Budget allocation changes
- Browser usage share over time

## Choosing the Right Chart Type

### Decision Guide

**For Trends Over Time:**
- Simple trend → **Line**
- Smooth trend → **Spline**
- Discrete changes → **Step Line**
- Magnitude matters → **Area/Spline Area**
- Composition over time → **Stacking Area**
- Proportions over time → **Stacking Area 100**

**For Category Comparison:**
- Simple comparison → **Column**
- Part-to-whole → **Stacking Column**
- Proportions → **Stacking Column 100**

**For Specific Use Cases:**
- Inventory tracking → **Step Line** or **Step Area**
- Market share → **Stacking Column 100** or **Stacking Area 100**
- Sales analysis → **Column** or **Line**
- Budget breakdown → **Stacking Column**
- Trend with volume → **Area** or **Spline Area**

### Quick Reference Table

| Chart Type | Best For | Avoid When |
|------------|----------|------------|
| Line | Trends, time-series | Need to show volume |
| Spline | Smooth trends | Data has sharp changes |
| Column | Category comparison | Too many categories (>15) |
| Area | Volume over time | Multiple overlapping series |
| Spline Area | Smooth volume trends | Need precise values |
| Stacking Column | Part-to-whole comparison | Components aren't related |
| Stacking Area | Cumulative trends | Series cross each other |
| Stacking Column 100 | Proportions comparison | Absolute values needed |
| Stacking Area 100 | Proportion trends | Absolute values needed |
| Step Line | Discrete changes | Data is continuous |
| Step Area | Discrete volume | Need smooth trends |

## Complete Examples

### Example 1: Dynamic Chart Type Selector

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.PivotChart;
using Syncfusion.PivotAnalysis.Base;

public partial class ChartTypeDemo : Form
{
    private PivotChart pivotChart1;
    private ComboBox cmbChartType;
    
    public ChartTypeDemo()
    {
        InitializeComponent();
        SetupControls();
        BindData();
    }
    
    private void SetupControls()
    {
        // Create chart type selector
        cmbChartType = new ComboBox
        {
            Dock = DockStyle.Top,
            DropDownStyle = ComboBoxStyle.DropDownList
        };
        
        cmbChartType.Items.AddRange(new object[]
        {
            "Line", "Spline", "Column", "Area", "Spline Area",
            "Stacking Column", "Stacking Area", "Stacking Column 100",
            "Stacking Area 100", "Step Line", "Step Area"
        });
        
        cmbChartType.SelectedIndex = 0;
        cmbChartType.SelectedIndexChanged += CmbChartType_SelectedIndexChanged;
        
        // Create pivot chart
        pivotChart1 = new PivotChart
        {
            Dock = DockStyle.Fill,
            ChartTypes = PivotChartTypes.Line
        };
        
        this.Controls.Add(pivotChart1);
        this.Controls.Add(cmbChartType);
    }
    
    private void CmbChartType_SelectedIndexChanged(object sender, EventArgs e)
    {
        switch (cmbChartType.SelectedItem.ToString())
        {
            case "Line":
                pivotChart1.ChartTypes = PivotChartTypes.Line;
                break;
            case "Spline":
                pivotChart1.ChartTypes = PivotChartTypes.Spline;
                break;
            case "Column":
                pivotChart1.ChartTypes = PivotChartTypes.Column;
                break;
            case "Area":
                pivotChart1.ChartTypes = PivotChartTypes.Area;
                break;
            case "Spline Area":
                pivotChart1.ChartTypes = PivotChartTypes.SplineArea;
                break;
            case "Stacking Column":
                pivotChart1.ChartTypes = PivotChartTypes.StackingColumn;
                break;
            case "Stacking Area":
                pivotChart1.ChartTypes = PivotChartTypes.StackingArea;
                break;
            case "Stacking Column 100":
                pivotChart1.ChartTypes = PivotChartTypes.StackingColumn100;
                break;
            case "Stacking Area 100":
                pivotChart1.ChartTypes = PivotChartTypes.StackingArea100;
                break;
            case "Step Line":
                pivotChart1.ChartTypes = PivotChartTypes.StepLine;
                break;
            case "Step Area":
                pivotChart1.ChartTypes = PivotChartTypes.StepArea;
                break;
        }
    }
    
    private void BindData()
    {
        // Bind data and configure pivot fields
        pivotChart1.ItemSource = GetSalesData();
        
        pivotChart1.PivotAxis.Add(new PivotItem 
        { 
            FieldMappingName = "Quarter", 
            TotalHeader = "Total" 
        });
        
        pivotChart1.PivotLegend.Add(new PivotItem 
        { 
            FieldMappingName = "Product", 
            TotalHeader = "Total" 
        });
        
        pivotChart1.PivotCalculations.Add(new PivotComputationInfo 
        { 
            FieldName = "Amount", 
            Format = "C0" 
        });
    }
}
```

### Example 2: Chart Type Based on Data

```csharp
// Automatically choose chart type based on data characteristics
private void AutoSelectChartType()
{
    var data = GetSalesData();
    int seriesCount = data.Select(d => d.Product).Distinct().Count();
    int categoryCount = data.Select(d => d.Quarter).Distinct().Count();
    
    if (seriesCount == 1)
    {
        // Single series - use simple types
        if (categoryCount > 10)
            pivotChart1.ChartTypes = PivotChartTypes.Line; // Many points
        else
            pivotChart1.ChartTypes = PivotChartTypes.Column; // Few points
    }
    else if (seriesCount <= 3)
    {
        // Few series - good for stacking
        pivotChart1.ChartTypes = PivotChartTypes.StackingColumn;
    }
    else
    {
        // Many series - use line for clarity
        pivotChart1.ChartTypes = PivotChartTypes.Line;
    }
}
```

## Performance Considerations

- **Line/Spline:** Fastest rendering, good for many data points
- **Column:** Moderate rendering, limit to <50 categories
- **Area/Stacking:** Slower with many series, keep to 3-5 series
- **Step charts:** Similar performance to line charts

## Best Practices

1. **Match chart type to data characteristics**
2. **Limit series count** - 3-5 series for stacking charts
3. **Provide chart type selector** for user flexibility
4. **Use consistent colors** when switching types
5. **Test with representative data** before deploying
6. **Consider audience** - simpler types for general audiences
7. **Document why you chose specific types** for maintainability

## Next Steps

- Configure **Drill Operations** for interactive exploration
- Customize **Appearance** with colors and styles
- Add **Legend** customization for better series identification
- Implement **Export** to save charts in different formats
