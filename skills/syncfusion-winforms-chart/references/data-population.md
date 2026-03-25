# Data Population

## Table of Contents
- [Overview](#overview)
- [Direct Point Addition](#direct-point-addition)
- [Data Binding with ChartDataBindModel](#data-binding-with-chartdatabindmodel)
- [Category Data Binding](#category-data-binding)
- [Custom Data Models](#custom-data-models)
- [Performance Considerations](#performance-considerations)

## Overview

Chart supports three data population methods:
1. **Direct addition:** Add points manually to `Points` collection
2. **Built-in binding:** Bind DataSet/DataTable/DataView using `ChartDataBindModel`
3. **Custom models:** Implement `IChartSeriesModel` for custom data sources

## Direct Point Addition

Simple approach for small datasets or programmatic control.

```csharp
ChartSeries series = new ChartSeries("Sales");
series.Type = ChartSeriesType.Column;

// Single Y value
series.Points.Add(1, 100);
series.Points.Add(2, 150);
series.Points.Add(3, 120);

// Multiple Y values (e.g., Candle chart)
series.Points.Add(1, new double[] { 100, 110, 95, 105 });

// DateTime X values
series.Points.Add(new DateTime(2024, 1, 1), 50);

chartControl1.Series.Add(series);
```

## Data Binding with ChartDataBindModel

Bind DataSet, DataTable, or DataView for efficient data management.

### Binding DataSet

```csharp
// Assume dataSet has "Sales" table with "Month" and "Revenue" columns
ChartDataBindModel model = new ChartDataBindModel(dataSet, "Sales");
model.XName = "Month";           // Column for X values
model.YNames = new string[] { "Revenue" };  // Columns for Y values

ChartSeries series = new ChartSeries("Revenue");
series.SeriesModelImpl = model;
chartControl1.Series.Add(series);
```

### Binding DataTable

```csharp
DataTable dt = new DataTable();
dt.Columns.Add("Product", typeof(string));
dt.Columns.Add("Sales", typeof(double));
dt.Rows.Add("Q1", 100);
dt.Rows.Add("Q2", 150);
dt.Rows.Add("Q3", 130);
dt.Rows.Add("Q4", 180);

ChartDataBindModel model = new ChartDataBindModel(dt);
model.XName = "Product";
model.YNames = new string[] { "Sales" };

ChartSeries series = new ChartSeries("Quarterly Sales");
series.SeriesModelImpl = model;
chartControl1.Series.Add(series);
```

### Multiple Y Values

For charts requiring multiple Y values (Candle, HiLo, etc.):

```csharp
// DataTable with Open, High, Low, Close columns
ChartDataBindModel model = new ChartDataBindModel(stockDataTable);
model.XName = "Date";
model.YNames = new string[] { "Open", "High", "Low", "Close" };

ChartSeries series = new ChartSeries("Stock Price");
series.Type = ChartSeriesType.Candle;
series.SeriesModelImpl = model;
chartControl1.Series.Add(series);
```

### Binding Axis Labels

Bind custom labels to axis from data:

```csharp
ChartDataBindAxisLabelModel labelModel = 
    new ChartDataBindAxisLabelModel(dataSet, "Demographics");
labelModel.LabelName = "City";  // Column containing label text

chartControl1.PrimaryXAxis.LabelsImpl = labelModel;
chartControl1.PrimaryXAxis.ValueType = ChartValueType.Custom;
```

## Category Data Binding

Use `CategoryAxisDataBindModel` for categorical X-axis data.

```csharp
using System.ComponentModel;

// Create data model
public class SalesData
{
    public string Month { get; set; }
    public double Revenue { get; set; }
}

// Populate data
BindingList<SalesData> data = new BindingList<SalesData>
{
    new SalesData { Month = "Jan", Revenue = 50 },
    new SalesData { Month = "Feb", Revenue = 65 },
    new SalesData { Month = "Mar", Revenue = 72 }
};

// Bind to chart
CategoryAxisDataBindModel model = new CategoryAxisDataBindModel(data);
model.CategoryName = "Month";  // Property name for categories
model.YNames = new string[] { "Revenue" };  // Property names for Y values

ChartSeries series = new ChartSeries("Monthly Revenue");
series.CategoryModel = model;
chartControl1.Series.Add(series);

// Configure axis for categories
chartControl1.PrimaryXAxis.ValueType = ChartValueType.Category;
```

### Multiple Series from Same Source

```csharp
BindingList<ProductData> data = new BindingList<ProductData>
{
    new ProductData { Product = "A", Sales = 100, Target = 120 },
    new ProductData { Product = "B", Sales = 150, Target = 140 }
};

// Series 1: Sales
CategoryAxisDataBindModel salesModel = new CategoryAxisDataBindModel(data);
salesModel.CategoryName = "Product";
salesModel.YNames = new string[] { "Sales" };

ChartSeries salesSeries = new ChartSeries("Sales");
salesSeries.Type = ChartSeriesType.Column;
salesSeries.CategoryModel = salesModel;

// Series 2: Target
CategoryAxisDataBindModel targetModel = new CategoryAxisDataBindModel(data);
targetModel.CategoryName = "Product";
targetModel.YNames = new string[] { "Target" };

ChartSeries targetSeries = new ChartSeries("Target");
targetSeries.Type = ChartSeriesType.Line;
targetSeries.CategoryModel = targetModel;

chartControl1.Series.Add(salesSeries);
chartControl1.Series.Add(targetSeries);
chartControl1.PrimaryXAxis.ValueType = ChartValueType.Category;
```

## Custom Data Models

Implement `IChartSeriesModel` or `IEditableChartSeriesModel` for advanced scenarios.

### Read-Only Custom Model

```csharp
public class CustomSeriesModel : IChartSeriesModel
{
    private double[] yValues;
    
    public int Count => yValues.Length;
    
    public CustomSeriesModel(double[] data)
    {
        yValues = data;
    }
    
    public ChartPoint GetPoint(int index)
    {
        return new ChartPoint(index, yValues[index]);
    }
    
    // Implement other IChartSeriesModel members...
}

// Usage
CustomSeriesModel model = new CustomSeriesModel(new double[] { 10, 20, 30, 40 });
ChartSeries series = new ChartSeries("Custom");
series.SeriesModelImpl = model;
chartControl1.Series.Add(series);
```

## Performance Considerations

### Large Datasets

For datasets with thousands of points:

1. **Use data binding instead of direct addition**
   ```csharp
   // SLOW: Adding 10,000 points individually
   for (int i = 0; i < 10000; i++)
       series.Points.Add(i, values[i]);
   
   // FAST: Use ChartDataBindModel
   ChartDataBindModel model = new ChartDataBindModel(dataTable);
   model.XName = "X";
   model.YNames = new string[] { "Y" };
   series.SeriesModelImpl = model;
   ```

2. **Implement custom IChartSeriesModel**
   - Query data on-demand
   - Avoid loading entire dataset into memory

3. **Optimize refresh**
   ```csharp
   chartControl1.SuspendLayout();
   // Add/modify data
   chartControl1.ResumeLayout(true);
   ```

### Memory Management

```csharp
// Clear series before rebinding
chartControl1.Series.Clear();

// Dispose models when done
if (series.SeriesModelImpl is IDisposable disposable)
    disposable.Dispose();
```

## Dynamic Data Updates

### Refresh After Data Change

```csharp
// Modify data source
dataTable.Rows[0]["Sales"] = 200;

// Refresh chart
chartControl1.Refresh();
```

### Add Points at Runtime

```csharp
series.Points.Add(newX, newY);
chartControl1.Refresh();
```

### Clear and Reload

```csharp
series.Points.Clear();
// Add new data
chartControl1.Refresh();
```
