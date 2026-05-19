# Data Binding

## Table of Contents
- [Overview](#overview)
- [Supported Data Source Types](#supported-data-source-types)
- [Binding to IEnumerable Collections](#binding-to-ienumerable-collections)
- [Binding to DataTable](#binding-to-datatable)
- [Binding to DataView](#binding-to-dataview)
- [Dynamic Data Updates](#dynamic-data-updates)
- [Refreshing the Pivot Grid](#refreshing-the-pivot-grid)
- [Data Binding Events](#data-binding-events)
- [Performance Considerations](#performance-considerations)

## Overview

The Pivot Grid control is designed to display bound data in a cross-tabulated, pivoted format. Data binding is achieved through the `ItemSource` property, which accepts various data source types. The control automatically processes the data to create pivot structures based on your row, column, and calculation configurations.

## Supported Data Source Types

The Pivot Grid accepts the following data source types:

1. **IEnumerable<T>** - Generic collections
   - `List<T>`
   - `ObservableCollection<T>`
   - `BindingList<T>`
   - Any collection implementing `IEnumerable`

2. **DataTable** - ADO.NET data tables

3. **DataView** - Views of DataTable objects

**Important:** The data source must be assigned to the `ItemSource` property before configuring pivot rows, columns, and calculations.

## Binding to IEnumerable Collections

The most common approach is binding to a strongly-typed collection like `List<T>`.

### Example: ProductSales Collection

```csharp
using System;
using System.Collections.Generic;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.PivotAnalysis;
using Syncfusion.PivotAnalysis.Base;
using Syncfusion.Windows.Forms;

namespace PivotGridDemo
{
    public partial class MainForm : Form
    {
        public MainForm()
        {
            InitializeComponent();
            
            pivotGridControl1.GridVisualStyles = GridVisualStyles.Metro;
            
            // Bind IEnumerable data source
            pivotGridControl1.ItemSource = ProductSalesData.GetSalesData();
            
            // Configure pivot structure
            pivotGridControl1.PivotRows.Add(new PivotItem 
            { 
                FieldMappingName = "Product", 
                TotalHeader = "Total" 
            });
            pivotGridControl1.PivotRows.Add(new PivotItem 
            { 
                FieldMappingName = "Date", 
                TotalHeader = "Total" 
            });
            
            pivotGridControl1.PivotColumns.Add(new PivotItem 
            { 
                FieldMappingName = "Country", 
                TotalHeader = "Total" 
            });
            
            pivotGridControl1.PivotCalculations.Add(new PivotComputationInfo 
            { 
                FieldName = "Amount", 
                Format = "C", 
                SummaryType = SummaryType.DoubleTotalSum 
            });
            pivotGridControl1.PivotCalculations.Add(new PivotComputationInfo 
            { 
                FieldName = "Quantity", 
                Format = "#,##0" 
            });
        }
    }
}
```

### Data Model Class

```csharp
public class ProductSales
{
    public string Product { get; set; }
    public string Date { get; set; }
    public string Country { get; set; }
    public string State { get; set; }
    public int Quantity { get; set; }
    public double Amount { get; set; }
    public double UnitPrice { get; set; }
    public double TotalPrice { get; set; }
}

public class ProductSalesCollection : List<ProductSales>
{
}
```

### Data Generation Method

```csharp
public class ProductSalesData
{
    public static ProductSalesCollection GetSalesData()
    {
        // Geography data
        string[] countries = { "Australia", "Canada", "France", "Germany", 
                              "United Kingdom", "United States" };
        string[] state1 = { "New South Wales", "Queensland", "South Australia" };
        string[] state2 = { "Alberta", "British Columbia", "Brunswick", 
                           "Manitoba", "Ontario", "Quebec" };
        string[] state3 = { "Charente Maritime", "Essonne", 
                           "Garonne (Haute)", "Gers" };
        string[] state4 = { "Bayern", "Brandenburg", "Hamburg", 
                           "Hessen", "Nordrhein Westfalen", "Saarland" };
        string[] state5 = { "England" };
        string[] state6 = { "New York", "North Carolina", "Alabama", 
                           "California", "Colorado", "New Mexico", "South Carolina" };
        
        // Time periods
        string[] dates = { "FY 2023", "FY 2024", "FY 2025" };
        
        // Products
        string[] products = { "Bike", "Car" };
        
        Random r = new Random(123345345);
        ProductSalesCollection listOfProductSales = new ProductSalesCollection();
        
        for (int i = 0; i < 2000; i++)
        {
            ProductSales sales = new ProductSales();
            sales.Country = countries[r.Next(1, countries.Length)];
            sales.Quantity = r.Next(1, 12);
            
            // Calculate amounts with discount
            double discount = (30 * sales.Quantity) * (sales.Quantity / 100.0);
            sales.Amount = (50 * sales.Quantity) - discount;
            sales.TotalPrice = sales.Amount * sales.Quantity;
            sales.UnitPrice = sales.Amount / sales.Quantity;
            sales.Date = dates[r.Next(dates.Length)];
            sales.Product = products[r.Next(products.Length)];
            
            // Assign state based on country
            switch (sales.Country)
            {
                case "Australia":
                    sales.State = state1[r.Next(state1.Length)];
                    break;
                case "Canada":
                    sales.State = state2[r.Next(state2.Length)];
                    break;
                case "France":
                    sales.State = state3[r.Next(state3.Length)];
                    break;
                case "Germany":
                    sales.State = state4[r.Next(state4.Length)];
                    break;
                case "United Kingdom":
                    sales.State = state5[r.Next(state5.Length)];
                    break;
                case "United States":
                    sales.State = state6[r.Next(state6.Length)];
                    break;
            }
            
            listOfProductSales.Add(sales);
        }
        
        return listOfProductSales;
    }
}
```

**VB.NET Version:**

```vb
Public Class ProductSalesData
    Public Shared Function GetSalesData() As ProductSalesCollection
        ' Geography
        Dim countries() As String = { "Australia", "Canada", "France", _
                                     "Germany", "United Kingdom", "United States" }
        ' ... (other arrays)
        
        Dim r As New Random(123345345)
        Dim listOfProductSales As New ProductSalesCollection()
        
        For i As Integer = 0 To 1999
            Dim sales As New ProductSales()
            sales.Country = countries(r.Next(1, countries.Length))
            sales.Quantity = r.Next(1, 12)
            
            Dim discount As Double = (30 * sales.Quantity) * (sales.Quantity / 100.0)
            sales.Amount = (50 * sales.Quantity) - discount
            sales.TotalPrice = sales.Amount * sales.Quantity
            sales.UnitPrice = sales.Amount / sales.Quantity
            ' ... (rest of logic)
            
            listOfProductSales.Add(sales)
        Next i
        
        Return listOfProductSales
    End Function
End Class
```

## Binding to DataTable

DataTable binding is useful when working with ADO.NET, database queries, or legacy applications.

### Example: Binding BusinessObject to DataTable

```csharp
using System.ComponentModel;
using System.Data;
using Syncfusion.PivotAnalysis.Base;
using Syncfusion.Windows.Forms;

namespace PivotGridDemo
{
    public partial class MainForm : Form
    {
        public MainForm()
        {
            InitializeComponent();
            
            pivotGridControl1.GridVisualStyles = GridVisualStyles.Metro;
            
            // Bind DataTable
            pivotGridControl1.ItemSource = BusinessObjectsDataView.GetDataTable();
            
            // Configure pivot structure
            pivotGridControl1.PivotRows.Add(new PivotItem 
            { 
                FieldMappingName = "Fruit", 
                TotalHeader = "Total" 
            });
            
            pivotGridControl1.PivotColumns.Add(new PivotItem 
            { 
                FieldMappingName = "Color", 
                TotalHeader = "Total" 
            });
            
            pivotGridControl1.PivotCalculations.Add(new PivotComputationInfo 
            { 
                FieldName = "Count", 
                Format = "#,##0", 
                SummaryType = SummaryType.DoubleTotalSum 
            });
            pivotGridControl1.PivotCalculations.Add(new PivotComputationInfo 
            { 
                FieldName = "Weight", 
                Format = "#,##0 KG", 
                SummaryType = SummaryType.DecimalTotalSum 
            });
        }
    }
}
```

### Creating DataTable from Collection

```csharp
public class BusinessObjectsDataView : DataView
{
    public static DataView GetDataTable()
    {
        DataTable dt = new DataTable("BusinessObjectsDataTable");
        
        // Create columns from object properties
        PropertyDescriptorCollection propertyDescriptors = 
            TypeDescriptor.GetProperties(typeof(BusinessObject));
        
        foreach (PropertyDescriptor propertyDescriptor in propertyDescriptors)
        {
            dt.Columns.Add(new DataColumn(propertyDescriptor.Name, 
                                         propertyDescriptor.PropertyType));
        }
        
        // Populate rows from collection
        BusinessObjectCollection businessObjectCollection = 
            BusinessObjectCollection.GetList();
        
        foreach (BusinessObject businessObject in businessObjectCollection)
        {
            DataRow dataRow = dt.NewRow();
            
            foreach (PropertyDescriptor propertyDescriptor in propertyDescriptors)
            {
                dataRow[propertyDescriptor.Name] = 
                    propertyDescriptor.GetValue(businessObject);
            }
            
            dt.Rows.Add(dataRow);
        }
        
        return dt.DefaultView;
    }
}
```

### BusinessObject Classes

```csharp
public class BusinessObject
{
    public string Fruit { get; set; }
    public string Color { get; set; }
    public double Weight { get; set; }
    public int Count { get; set; }
}

public class BusinessObjectCollection : List<BusinessObject>
{
    public static BusinessObjectCollection GetList()
    {
        BusinessObjectCollection list = new BusinessObjectCollection();
        
        List<string> fruits = new List<string>(new string[] 
        { 
            "Cherry", "Mango", "Orange", "Grape", "Plum", 
            "Fig", "Apple", "Gooseberry", "Strawberry" 
        });
        
        List<string> colors = new List<string>(new string[] 
        { 
            "Red", "Green", "Yellow", "Orange", "Almond", "White", "Beige" 
        });
        
        Random r = new Random(123345345);
        
        for (int i = 0; i < 2000; i++)
        {
            BusinessObject businessObject = new BusinessObject()
            {
                Fruit = fruits[r.Next(fruits.Count)],
                Color = colors[r.Next(colors.Count)],
                Weight = (int)(r.NextDouble() * 1000) / 10.0,
                Count = r.Next(4) + 1
            };
            list.Add(businessObject);
        }
        
        return list;
    }
}
```

## Binding to DataView

DataView provides a customizable view of a DataTable with filtering and sorting:

```csharp
// Create DataTable
DataTable dt = GetDataTable();

// Create DataView with filter
DataView dv = new DataView(dt);
dv.RowFilter = "Amount > 1000";
dv.Sort = "Date DESC";

// Bind to Pivot Grid
pivotGridControl1.ItemSource = dv;
```

## Dynamic Data Updates

### Rebinding with New Data

To update the data source dynamically:

```csharp
// Method 1: Replace entire data source
private void UpdateData()
{
    pivotGridControl1.ItemSource = null;  // Clear existing
    pivotGridControl1.ItemSource = GetUpdatedData();  // Bind new data
}

// Method 2: Update and refresh
private void RefreshData()
{
    // Modify existing collection
    var currentData = pivotGridControl1.ItemSource as List<ProductSales>;
    if (currentData != null)
    {
        currentData.Add(new ProductSales { /* new data */ });
        pivotGridControl1.TableControl.Refresh(true);  // Repopulate pivot engine
    }
}
```

### Using ObservableCollection for Automatic Updates

For automatic UI updates when data changes:

```csharp
using System.Collections.ObjectModel;

public class DataManager
{
    public ObservableCollection<ProductSales> SalesData { get; set; }
    
    public DataManager()
    {
        SalesData = new ObservableCollection<ProductSales>();
        LoadData();
    }
    
    private void LoadData()
    {
        // Populate collection
        foreach (var sale in ProductSalesData.GetSalesData())
        {
            SalesData.Add(sale);
        }
    }
    
    public void AddSale(ProductSales newSale)
    {
        SalesData.Add(newSale);  // Automatically triggers UI update
    }
}

// In your form
DataManager dataManager = new DataManager();
pivotGridControl1.ItemSource = dataManager.SalesData;
```

## Refreshing the Pivot Grid

The `Refresh` method controls how the Pivot Grid updates its display:

### Refresh Without Repopulating Engine

Updates the visual display only, without recalculating pivot engine data:

```csharp
// Fast refresh - visual update only
pivotGridControl1.TableControl.Refresh(false);
```

**Use when:**
- Applying visual styles or themes
- Changing cell formatting
- Updating colors or fonts

### Refresh With Repopulating Engine

Recalculates all pivot computations and rebuilds the structure:

```csharp
// Full refresh - recalculates all aggregations
pivotGridControl1.TableControl.Refresh(true);
```

**Use when:**
- Data source has changed
- Adding/removing pivot rows or columns
- Modifying calculations
- Filtering or sorting changes

### Complete Rebind

For major data source changes:

```csharp
private void CompleteRebind()
{
    // Clear existing bindings
    pivotGridControl1.PivotRows.Clear();
    pivotGridControl1.PivotColumns.Clear();
    pivotGridControl1.PivotCalculations.Clear();
    
    // Rebind data
    pivotGridControl1.ItemSource = GetNewData();
    
    // Reconfigure pivot structure
    ConfigurePivotStructure();
    
    // Force full refresh
    pivotGridControl1.TableControl.Refresh(true);
}
```

## Data Binding Events

Monitor data source changes and refresh operations:

### ItemSourceChanged Event

Fires when the `ItemSource` property changes:

```csharp
using Syncfusion.Windows.Forms.PivotAnalysis;

pivotGridControl1.ItemSourceChanged += PivotGridControl1_ItemSourceChanged;

private void PivotGridControl1_ItemSourceChanged(object sender, 
    ItemSourceChangedEventArgs e)
{
    // Access old and new data sources
    var oldSource = e.OldValue;
    var newSource = e.NewValue;
    
    Console.WriteLine($"Data source changed from {oldSource?.GetType().Name} " +
                     $"to {newSource?.GetType().Name}");
    
    // Perform post-binding operations
    if (newSource != null)
    {
        ConfigureForNewDataSource();
    }
}
```

### DataRefreshing Event

Fires when refresh operation begins:

```csharp
pivotGridControl1.DataRefreshing += PivotGridControl1_DataRefreshing;

private void PivotGridControl1_DataRefreshing(object sender, 
    DataRefreshingEventArgs e)
{
    // Show loading indicator
    ShowLoadingIndicator();
    
    Console.WriteLine("Pivot grid is refreshing...");
}
```

### DataRefreshed Event

Fires when refresh operation completes:

```csharp
pivotGridControl1.DataRefreshed += PivotGridControl1_DataRefreshed;

private void PivotGridControl1_DataRefreshed(object sender, 
    DataRefreshedEventArgs e)
{
    // Hide loading indicator
    HideLoadingIndicator();
    
    Console.WriteLine("Pivot grid refresh completed");
    
    // Perform post-refresh operations
    ApplyConditionalFormatting();
}
```

## Performance Considerations

### Best Practices for Large Datasets

1. **Use Appropriate Data Structures**
```csharp
// Good: List<T> for static data
List<ProductSales> staticData = GetStaticData();

// Good: ObservableCollection<T> only if you need change notifications
ObservableCollection<ProductSales> dynamicData = new ObservableCollection<ProductSales>();
```

2. **Limit Initial Data Load**
```csharp
// Load subset initially
var initialData = GetSalesData().Take(1000).ToList();
pivotGridControl1.ItemSource = initialData;

// Load remaining data asynchronously if needed
Task.Run(() => LoadRemainingDataAsync());
```

3. **Optimize Refresh Calls**
```csharp
// Batch multiple changes before refreshing
pivotGridControl1.TableControl.BeginUpdate();
try
{
    pivotGridControl1.PivotRows.Add(new PivotItem { FieldMappingName = "Product" });
    pivotGridControl1.PivotColumns.Add(new PivotItem { FieldMappingName = "Country" });
    // ... more changes
}
finally
{
    pivotGridControl1.TableControl.EndUpdate();  // Single refresh
}
```

4. **Async Data Loading**
```csharp
private async void LoadDataAsync()
{
    var data = await Task.Run(() => ProductSalesData.GetSalesData());
    
    pivotGridControl1.ItemSource = data;
}
```

### Memory Management

For very large datasets (100k+ rows):

```csharp
// Dispose old data source if needed
if (pivotGridControl1.ItemSource is IDisposable disposable)
{
    disposable.Dispose();
}

// Clear references
pivotGridControl1.ItemSource = null;
GC.Collect();  // Force garbage collection if necessary

// Bind new data
pivotGridControl1.ItemSource = newData;
```

## Troubleshooting

### Common Issues

**Problem:** Data not displaying after binding
```csharp
// Solution: Ensure pivot structure is configured AFTER binding data
pivotGridControl1.ItemSource = data;  // First
pivotGridControl1.PivotRows.Add(...);  // Then configure
pivotGridControl1.PivotCalculations.Add(...);
```

**Problem:** Property names not found
```csharp
// Solution: Ensure FieldMappingName matches property names exactly (case-sensitive)
pivotGridControl1.PivotRows.Add(new PivotItem 
{ 
    FieldMappingName = "Product"  // Must match property name exactly
});
```

**Problem:** Null reference exceptions
```csharp
// Solution: Check data source is not null and contains data
if (data != null && data.Any())
{
    pivotGridControl1.ItemSource = data;
}
```
