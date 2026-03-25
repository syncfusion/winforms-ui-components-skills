# Data Binding in Pivot Chart

This guide covers comprehensive data binding techniques for the Syncfusion Pivot Chart control, including IEnumerable lists, DataTable sources, and configuration of pivot fields.

## Table of Contents
- [Overview](#overview)
- [Data Source Types](#data-source-types)
- [ItemSource Configuration](#itemsource-configuration)
- [PivotAxis Setup](#pivotaxis-setup)
- [PivotLegend Configuration](#pivotlegend-configuration)
- [PivotCalculations](#pivotcalculations)
- [Real-Time Updates](#real-time-updates)
- [Performance Optimization](#performance-optimization)
- [Complete Examples](#complete-examples)

## Overview

The Pivot Chart control retrieves multidimensional data from various sources and transforms it into interactive visualizations. Data binding involves:

1. **ItemSource** - Setting the data source
2. **PivotAxis** - Configuring hierarchical axis fields
3. **PivotLegend** - Defining series categorization
4. **PivotCalculations** - Specifying aggregation fields

## Data Source Types

Pivot Chart supports two primary data source types:

### IEnumerable Lists

Best for: Object collections, LINQ queries, in-memory data

```csharp
using System.Collections.Generic;

List<SalesData> salesList = GetSalesData();
pivotChart1.ItemSource = salesList;
```

### DataTable

Best for: Database queries, ADO.NET data, DataSet relations

```csharp
using System.Data;

DataTable salesTable = GetSalesDataTable();
pivotChart1.ItemSource = salesTable;
```

### Supported Collection Types

- `List<T>`
- `ObservableCollection<T>`
- `BindingList<T>`
- `IEnumerable<T>`
- `DataTable`
- `DataView`
- Any collection implementing `IEnumerable`

## ItemSource Configuration

### Basic IEnumerable Binding

```csharp
using Syncfusion.Windows.Forms.PivotChart;

public class SalesData
{
    public string Product { get; set; }
    public string Region { get; set; }
    public string Quarter { get; set; }
    public int Quantity { get; set; }
    public decimal Amount { get; set; }
}

// Create and bind data
private void BindData()
{
    List<SalesData> salesList = new List<SalesData>
    {
        new SalesData { Product = "Laptop", Region = "North", Quarter = "Q1", Quantity = 50, Amount = 50000 },
        new SalesData { Product = "Laptop", Region = "South", Quarter = "Q1", Quantity = 40, Amount = 40000 },
        new SalesData { Product = "Mouse", Region = "North", Quarter = "Q1", Quantity = 200, Amount = 6000 },
        // More data...
    };
    
    pivotChart1.ItemSource = salesList;
}
```

### DataTable Binding

```csharp
using System.Data;

private void BindDataTable()
{
    DataTable salesTable = new DataTable("Sales");
    
    // Define columns
    salesTable.Columns.Add("Product", typeof(string));
    salesTable.Columns.Add("Region", typeof(string));
    salesTable.Columns.Add("Quarter", typeof(string));
    salesTable.Columns.Add("Quantity", typeof(int));
    salesTable.Columns.Add("Amount", typeof(decimal));
    
    // Add rows
    salesTable.Rows.Add("Laptop", "North", "Q1", 50, 50000);
    salesTable.Rows.Add("Laptop", "South", "Q1", 40, 40000);
    salesTable.Rows.Add("Mouse", "North", "Q1", 200, 6000);
    // More rows...
    
    pivotChart1.ItemSource = salesTable;
}
```

### Database Binding

```csharp
using System.Data.SqlClient;

private void BindFromDatabase()
{
    string connectionString = "Data Source=SERVER;Initial Catalog=DB;Integrated Security=True";
    string query = "SELECT Product, Region, Quarter, Quantity, Amount FROM Sales";
    
    using (SqlConnection conn = new SqlConnection(connectionString))
    {
        SqlDataAdapter adapter = new SqlDataAdapter(query, conn);
        DataTable salesTable = new DataTable();
        adapter.Fill(salesTable);
        
        pivotChart1.ItemSource = salesTable;
    }
}
```

## PivotAxis Setup

PivotAxis defines the hierarchical structure for the chart's X-axis. Items are displayed in order of addition, with each level drilling deeper.

### Single Axis Field

```csharp
using Syncfusion.PivotAnalysis.Base;

// Simple axis with one field
pivotChart1.PivotAxis.Add(new PivotItem 
{ 
    FieldMappingName = "Product",
    TotalHeader = "All Products"
});
```

### Hierarchical Axis (Multiple Levels)

```csharp
// Define hierarchy: Category → Product → Region → State
pivotChart1.PivotAxis.Clear(); // Clear any existing items

pivotChart1.PivotAxis.Add(new PivotItem 
{ 
    FieldMappingName = "Category",
    TotalHeader = "All Categories",
    FieldHeader = "Product Category"
});

pivotChart1.PivotAxis.Add(new PivotItem 
{ 
    FieldMappingName = "Product",
    TotalHeader = "All Products",
    FieldHeader = "Product Name"
});

pivotChart1.PivotAxis.Add(new PivotItem 
{ 
    FieldMappingName = "Region",
    TotalHeader = "All Regions"
});

pivotChart1.PivotAxis.Add(new PivotItem 
{ 
    FieldMappingName = "State",
    TotalHeader = "All States"
});
```

### PivotItem Properties

| Property | Description | Example |
|----------|-------------|---------|
| `FieldMappingName` | Property name in data source (case-sensitive) | `"Product"` |
| `TotalHeader` | Text displayed for aggregated/total rows | `"Total"`, `"All Products"` |
| `FieldHeader` | Display name for the field (optional) | `"Product Name"` |

### Best Practices for Axis Configuration

1. **Order matters** - First item is top level, last is deepest drill level
2. **Limit depth** - 3-4 levels is optimal for usability
3. **Use descriptive TotalHeader** - "All Products" is clearer than "Total"
4. **Match FieldMappingName exactly** - Property names are case-sensitive

## PivotLegend Configuration

PivotLegend defines how data series are categorized and displayed in the chart legend.

### Basic Legend Configuration

```csharp
// Single legend field
pivotChart1.PivotLegend.Add(new PivotItem 
{ 
    FieldMappingName = "Quarter",
    TotalHeader = "All Quarters"
});
```

### Multiple Legend Fields

```csharp
// Nested legend: Year → Quarter
pivotChart1.PivotLegend.Clear();

pivotChart1.PivotLegend.Add(new PivotItem 
{ 
    FieldMappingName = "Year",
    TotalHeader = "All Years"
});

pivotChart1.PivotLegend.Add(new PivotItem 
{ 
    FieldMappingName = "Quarter",
    TotalHeader = "All Quarters"
});
```

### Choosing Axis vs Legend

**Use PivotAxis for:**
- Hierarchical data you want to drill into
- Primary categorization (products, regions, categories)
- Data requiring drill-down navigation

**Use PivotLegend for:**
- Time periods (years, quarters, months)
- Secondary categorization (product types, sales channels)
- Data that benefits from color-coding

## PivotCalculations

PivotCalculations define which numeric fields to aggregate and how to display them.

### Basic Calculation

```csharp
pivotChart1.PivotCalculations.Add(new PivotComputationInfo 
{ 
    FieldName = "Quantity",
    Format = "#,##0"
});
```

### Multiple Calculations

```csharp
pivotChart1.PivotCalculations.Clear();

// Total quantity
pivotChart1.PivotCalculations.Add(new PivotComputationInfo
{
    FieldName = "Quantity",
    Format = "#,##0",
    FieldHeader = "Total Qty",
    SummaryType = SummaryType.DoubleTotalSum
});

// Total revenue (currency format)
pivotChart1.PivotCalculations.Add(new PivotComputationInfo
{
    FieldName = "Amount",
    Format = "C2",
    FieldHeader = "Revenue",
    SummaryType = SummaryType.DoubleTotalSum
});

// Average price
pivotChart1.PivotCalculations.Add(new PivotComputationInfo
{
    FieldName = "UnitPrice",
    Format = "C2",
    FieldHeader = "Avg Price",
    SummaryType = SummaryType.DoubleAverage
});
```

### PivotComputationInfo Properties

| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `FieldName` | string | Property name to aggregate | `"Quantity"` |
| `Format` | string | Number format string | `"#,##0"`, `"C2"`, `"P1"` |
| `FieldHeader` | string | Display name | `"Total Sales"` |
| `SummaryType` | SummaryType | Aggregation method | See table below |

### SummaryType Options

| SummaryType | Description | Use Case |
|-------------|-------------|----------|
| `DoubleTotalSum` | Sum of all values | Total sales, total quantity |
| `DoubleAverage` | Average of values | Average price, average rating |
| `Count` | Count of items | Number of orders |
| `DoubleMaximum` | Largest value | Highest price, max quantity |
| `DoubleMinimum` | Smallest value | Lowest price, min quantity |
| `DoubleStandardDeviation` | Standard deviation | Data variance analysis |

### Format String Examples

```csharp
// Integer with thousand separator
Format = "#,##0"              // 1,234

// Currency with 2 decimals
Format = "C2"                 // $1,234.56

// Percentage with 1 decimal
Format = "P1"                 // 45.6%

// Custom format
Format = "$ #,##0.00"         // $ 1,234.56

// Scientific notation
Format = "0.00E+00"           // 1.23E+03
```

## Real-Time Updates

Enable automatic chart updates when the underlying data changes.

### Enabling Auto-Update

```csharp
// Enable real-time updates
pivotChart1.EnableUpdating = true;
```

### Using Observable Collection

```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;

// Use BindingList for automatic notifications
private BindingList<SalesData> salesData;

private void SetupRealTimeData()
{
    salesData = new BindingList<SalesData>(GetSalesData());
    pivotChart1.ItemSource = salesData;
    pivotChart1.EnableUpdating = true;
    
    // Changes to salesData automatically update the chart
}

private void AddSalesRecord()
{
    // This will automatically update the chart
    salesData.Add(new SalesData 
    { 
        Product = "Tablet", 
        Region = "East", 
        Quarter = "Q2", 
        Quantity = 75, 
        Amount = 37500 
    });
}

private void UpdateSalesRecord()
{
    // Modifying existing item updates the chart
    salesData[0].Quantity = 100;
    salesData.ResetItem(0); // Notify of change
}
```

### Manual Refresh

If `EnableUpdating` is false, manually refresh after data changes:

```csharp
// Modify data
salesData.Add(newItem);

// Manually refresh chart
pivotChart1.Refresh();
```

## Performance Optimization

### BeginUpdate and EndUpdate

For bulk operations, suspend updates to prevent multiple redraws:

```csharp
private void BulkDataOperation()
{
    // Suspend chart updates
    pivotChart1.BeginUpdate();
    
    try
    {
        // Perform multiple operations
        pivotChart1.PivotAxis.Clear();
        pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "Product", TotalHeader = "Total" });
        pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "Region", TotalHeader = "Total" });
        
        pivotChart1.PivotLegend.Clear();
        pivotChart1.PivotLegend.Add(new PivotItem { FieldMappingName = "Quarter", TotalHeader = "Total" });
        
        pivotChart1.PivotCalculations.Clear();
        pivotChart1.PivotCalculations.Add(new PivotComputationInfo { FieldName = "Quantity", Format = "#,##0" });
        
        // Add/modify data items
        var data = (List<SalesData>)pivotChart1.ItemSource;
        for (int i = 0; i < 100; i++)
        {
            data.Add(new SalesData { /* ... */ });
        }
    }
    finally
    {
        // Resume updates - chart refreshes once
        pivotChart1.EndUpdate();
    }
}
```

### Best Practices

1. **Use BeginUpdate/EndUpdate** for bulk field configuration
2. **Disable EnableUpdating** if real-time updates aren't needed
3. **Filter data** before binding rather than binding entire dataset
4. **Use appropriate collections** - `List<T>` for static, `BindingList<T>` for dynamic
5. **Limit hierarchy depth** - Each level increases processing time

## Complete Examples

### Example 1: Sales Analysis by Product and Region

```csharp
using System;
using System.Collections.Generic;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.PivotChart;
using Syncfusion.PivotAnalysis.Base;

public class SalesData
{
    public string Product { get; set; }
    public string Region { get; set; }
    public string Quarter { get; set; }
    public int Quantity { get; set; }
    public decimal Amount { get; set; }
}

public partial class SalesAnalysisForm : Form
{
    private PivotChart pivotChart1;
    
    public SalesAnalysisForm()
    {
        InitializeComponent();
        ConfigurePivotChart();
    }
    
    private void ConfigurePivotChart()
    {
        pivotChart1 = new PivotChart();
        pivotChart1.Dock = DockStyle.Fill;
        
        // Bind data
        pivotChart1.ItemSource = GetSalesData();
        
        // Configure axis: Product → Region hierarchy
        pivotChart1.PivotAxis.Add(new PivotItem 
        { 
            FieldMappingName = "Product", 
            TotalHeader = "All Products" 
        });
        pivotChart1.PivotAxis.Add(new PivotItem 
        { 
            FieldMappingName = "Region", 
            TotalHeader = "All Regions" 
        });
        
        // Configure legend: Quarters
        pivotChart1.PivotLegend.Add(new PivotItem 
        { 
            FieldMappingName = "Quarter", 
            TotalHeader = "All Quarters" 
        });
        
        // Configure calculations
        pivotChart1.PivotCalculations.Add(new PivotComputationInfo
        {
            FieldName = "Quantity",
            Format = "#,##0",
            FieldHeader = "Units Sold"
        });
        
        pivotChart1.PivotCalculations.Add(new PivotComputationInfo
        {
            FieldName = "Amount",
            Format = "C0",
            FieldHeader = "Revenue"
        });
        
        // Set chart type
        pivotChart1.ChartTypes = PivotChartTypes.Column;
        
        // Enable drill-down
        pivotChart1.AllowDrillDown = true;
        
        this.Controls.Add(pivotChart1);
    }
    
    private List<SalesData> GetSalesData()
    {
        return new List<SalesData>
        {
            new SalesData { Product = "Laptop", Region = "North", Quarter = "Q1", Quantity = 50, Amount = 50000 },
            new SalesData { Product = "Laptop", Region = "North", Quarter = "Q2", Quantity = 55, Amount = 55000 },
            new SalesData { Product = "Laptop", Region = "South", Quarter = "Q1", Quantity = 40, Amount = 40000 },
            new SalesData { Product = "Laptop", Region = "South", Quarter = "Q2", Quantity = 45, Amount = 45000 },
            new SalesData { Product = "Mouse", Region = "North", Quarter = "Q1", Quantity = 200, Amount = 6000 },
            new SalesData { Product = "Mouse", Region = "North", Quarter = "Q2", Quantity = 220, Amount = 6600 },
            new SalesData { Product = "Mouse", Region = "South", Quarter = "Q1", Quantity = 180, Amount = 5400 },
            new SalesData { Product = "Mouse", Region = "South", Quarter = "Q2", Quantity = 195, Amount = 5850 },
            // Add more data as needed
        };
    }
}
```

### Example 2: Dynamic Data Binding with Updates

```csharp
using System.ComponentModel;

public partial class DynamicDataForm : Form
{
    private PivotChart pivotChart1;
    private BindingList<SalesData> salesData;
    private Timer updateTimer;
    
    public DynamicDataForm()
    {
        InitializeComponent();
        ConfigureDynamicChart();
    }
    
    private void ConfigureDynamicChart()
    {
        pivotChart1 = new PivotChart();
        pivotChart1.Dock = DockStyle.Fill;
        
        // Create binding list for auto-updates
        salesData = new BindingList<SalesData>(GetSalesData());
        
        // Bind and enable updates
        pivotChart1.ItemSource = salesData;
        pivotChart1.EnableUpdating = true;
        
        // Configure pivot fields
        pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "Product", TotalHeader = "Total" });
        pivotChart1.PivotLegend.Add(new PivotItem { FieldMappingName = "Quarter", TotalHeader = "Total" });
        pivotChart1.PivotCalculations.Add(new PivotComputationInfo { FieldName = "Quantity", Format = "#,##0" });
        
        this.Controls.Add(pivotChart1);
        
        // Simulate real-time updates
        updateTimer = new Timer { Interval = 5000 };
        updateTimer.Tick += UpdateTimer_Tick;
        updateTimer.Start();
    }
    
    private void UpdateTimer_Tick(object sender, EventArgs e)
    {
        // Add new data - chart automatically updates
        Random rand = new Random();
        salesData.Add(new SalesData
        {
            Product = "Tablet",
            Region = "East",
            Quarter = "Q3",
            Quantity = rand.Next(20, 100),
            Amount = rand.Next(10000, 50000)
        });
    }
}
```

## Common Issues

### Issue: FieldMappingName Not Found

**Problem:** Chart shows no data or throws exception

**Solution:** Ensure FieldMappingName matches property name exactly (case-sensitive):

```csharp
// CORRECT
FieldMappingName = "Product"  // Matches: public string Product { get; set; }

// INCORRECT
FieldMappingName = "product"  // Wrong case
FieldMappingName = "Products" // Wrong pluralization
```

### Issue: No Aggregation Displayed

**Problem:** Chart appears but shows no values

**Solution:** Ensure PivotCalculations is configured:

```csharp
// Must have at least one calculation
pivotChart1.PivotCalculations.Add(new PivotComputationInfo 
{ 
    FieldName = "Quantity", 
    Format = "#,##0" 
});
```

### Issue: Performance Degradation

**Problem:** Slow rendering or UI freezing

**Solutions:**
1. Use BeginUpdate/EndUpdate for bulk operations
2. Disable EnableUpdating if not needed
3. Filter data before binding
4. Limit hierarchy depth to 3-4 levels

## Next Steps

- Explore **Chart Types** to visualize data differently
- Enable **Drill Operations** for interactive hierarchy navigation
- Configure **Grouping Bar** for user-driven field arrangement
- Implement **Export** to save charts for reporting
