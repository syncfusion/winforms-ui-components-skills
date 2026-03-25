# Getting Started

Basic setup and first chart implementation for Windows Forms ChartControl.

## Assembly Deployment

**Required assemblies:**
- `Syncfusion.Chart.Windows.dll`
- `Syncfusion.Shared.Base.dll`

**NuGet Package:**
```
Install-Package Syncfusion.Chart.WinForms
```

## Adding ChartControl to Form

**Design-time:**
1. Open form in designer
2. Drag `ChartControl` from toolbox to form
3. ChartWizard opens automatically for configuration
4. Configure properties via Properties grid

**Code-based:**
```csharp
using Syncfusion.Windows.Forms.Chart;

ChartControl chartControl1 = new ChartControl();
chartControl1.Dock = DockStyle.Fill;
this.Controls.Add(chartControl1);
```

## Basic Data Population with BindingList

### Step 1: Create Data Model

```csharp
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
```

### Step 2: Create Data Source

```csharp
BindingList<SalesData> dataSource = new BindingList<SalesData>();
dataSource.Add(new SalesData("2020", 30));
dataSource.Add(new SalesData("2021", 45));
dataSource.Add(new SalesData("2022", 60));
dataSource.Add(new SalesData("2023", 75));
```

### Step 3: Bind to Chart

```csharp
// Create data binding model
CategoryAxisDataBindModel dataModel = new CategoryAxisDataBindModel(dataSource);
dataModel.CategoryName = "Year";  // Property for X values
dataModel.YNames = new string[] { "Sales" };  // Properties for Y values

// Create series
ChartSeries series = new ChartSeries("Sales");
series.CategoryModel = dataModel;
chartControl1.Series.Add(series);

// Set axis type for categories
chartControl1.PrimaryXAxis.ValueType = ChartValueType.Category;
```

## Quick Configuration

### Apply Skin
```csharp
chartControl1.Skins = Skins.Metro;
// Options: Metro, Office2016, Office2019, etc.
```

### Add Title
```csharp
ChartTitle title = new ChartTitle();
title.Text = "Sales Performance";
chartControl1.Titles.Add(title);
```

### Configure Legend
```csharp
chartControl1.Legend.Visible = true;
chartControl1.LegendAlignment = ChartAlignment.Center;
chartControl1.Legend.Position = ChartDock.Bottom;
chartControl1.LegendsPlacement = ChartPlacement.Outside;
```

### Enable Data Labels
```csharp
series.Style.DisplayText = true;
series.Style.TextOrientation = ChartTextOrientation.Up;
```

### Enable Tooltips
```csharp
chartControl1.ShowToolTips = true;
chartControl1.Tooltip.BackgroundColor = new BrushInfo(Color.White);
chartControl1.Tooltip.BorderStyle = BorderStyle.FixedSingle;

// Custom tooltip content
series.PrepareStyle += (sender, args) =>
{
    ChartSeries s = sender as ChartSeries;
    ChartPoint point = s.Points[args.Index];
    args.Style.ToolTip = $"Year: {point.Category}\nSales: {point.YValues[0]}";
};
```

## Complete Example

```csharp
using Syncfusion.Windows.Forms.Chart;
using System.ComponentModel;
using System.Drawing;

public partial class Form1 : Form
{
    public Form1()
    {
        InitializeComponent();
        SetupChart();
    }
    
    private void SetupChart()
    {
        // Create data
        BindingList<SalesData> data = new BindingList<SalesData>
        {
            new SalesData("2020", 30),
            new SalesData("2021", 45),
            new SalesData("2022", 60),
            new SalesData("2023", 75)
        };
        
        // Bind data
        CategoryAxisDataBindModel model = new CategoryAxisDataBindModel(data);
        model.CategoryName = "Year";
        model.YNames = new string[] { "Sales" };
        
        // Create series
        ChartSeries series = new ChartSeries("Sales");
        series.Type = ChartSeriesType.Column;
        series.CategoryModel = model;
        series.Style.DisplayText = true;
        
        // Configure chart
        chartControl1.PrimaryXAxis.ValueType = ChartValueType.Category;
        chartControl1.Skins = Skins.Metro;
        chartControl1.Series.Add(series);
        
        // Add title
        chartControl1.Titles.Add(new ChartTitle { Text = "Sales Performance" });
        
        // Configure legend
        chartControl1.Legend.Visible = true;
        chartControl1.Legend.Position = ChartDock.Bottom;
    }
}

public class SalesData
{
    public string Year { get; set; }
    public double Sales { get; set; }
    
    public SalesData(string year, double sales)
    {
        Year = year;
        Sales = sales;
    }
}
```

## Design-Time Features

**ChartWizard:** Automatically opens when ChartControl is added. Configure:
- Chart type
- Series setup
- Data binding
- Appearance

**Properties Grid:** Modify all chart properties at design-time
- Series collection editor
- Axis configuration
- Legend settings
- Title management

## Next Steps

- For chart types: See chart-types.md
- For data binding options: See data-population.md
- For series configuration: See chart-series.md
- For styling: See appearance-styling.md
