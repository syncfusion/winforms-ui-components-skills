# Getting Started with Pivot Chart

This guide covers the initial setup and basic implementation of the Syncfusion Pivot Chart control in Windows Forms applications.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Assembly Deployment](#assembly-deployment)
- [Adding Control via Code](#adding-control-via-code)
- [Basic Data Binding](#basic-data-binding)
- [Sample Data Structure](#sample-data-structure)
- [Complete Example](#complete-example)

## Prerequisites

Before implementing Pivot Chart, ensure you have:

- **Visual Studio** 2017 or later
- **.NET** .NET 8 or above
## Assembly Deployment

The Pivot Chart control requires the following assemblies:

### Required Assemblies

```
Syncfusion.Chart.Windows.dll
Syncfusion.Grid.Windows.dll
Syncfusion.PivotAnalysis.Base.dll
Syncfusion.PivotAnalysis.Windows.dll
Syncfusion.PivotChart.Windows.dll
Syncfusion.Shared.Base.dll
```

## Adding Control via Designer

The easiest way to add Pivot Chart is through the Visual Studio designer.

### Step-by-Step Process

**Step 1:** Create a new Windows Forms Application

```
File → New → Project → Windows Forms App (.NET Framework)
```

**Step 2:** Open the form in designer view

**Step 3:** Locate Pivot Chart in Toolbox
- If not visible, right-click Toolbox → "Choose Items"
- Browse to `Syncfusion.PivotChart.Windows.dll`
- Check "PivotChart" control

**Step 4:** Drag and drop onto form

![Drag PivotChart from toolbox](../../../../../docs/Pivot-Chart/Getting-Started_images/GettingStarted_img1.png)

**Step 5:** Control is added with all required assemblies

![PivotChart added to form](../../../../../docs/Pivot-Chart/Getting-Started_images/GettingStarted_img2.png)

### Designer Properties

Once added, configure properties in the Properties window:

- **Name:** `pivotChart1` (default)
- **Dock:** Set to `Fill` for full form coverage
- **Size:** Adjust as needed
- **ChartTypes:** Select initial chart type

## Adding Control via Code

For programmatic control creation, follow these steps.

### Step 1: Add Assembly References

Right-click project → Add Reference → Browse to Syncfusion assemblies:

```
Syncfusion.Chart.Windows.dll
Syncfusion.Grid.Windows.dll
Syncfusion.PivotAnalysis.Base.dll
Syncfusion.PivotAnalysis.Windows.dll
Syncfusion.PivotChart.Windows.dll
Syncfusion.Shared.Base.dll
```

### Step 2: Add Using Statements

```csharp
using Syncfusion.Windows.Forms.PivotChart;
using Syncfusion.PivotAnalysis.Base;
```

### Step 3: Create and Initialize Control

```csharp
public partial class Form1 : Form
{
    private PivotChart pivotChart1;
    
    public Form1()
    {
        InitializeComponent();
        
        // Create instance
        pivotChart1 = new PivotChart();
        
        // Set size and location
        pivotChart1.Size = new Size(800, 600);
        pivotChart1.Location = new Point(10, 10);
        
        // Or use docking
        pivotChart1.Dock = DockStyle.Fill;
        
        // Add to form
        this.Controls.Add(pivotChart1);
    }
}
```

### Complete Code Example

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.PivotChart;

namespace PivotChartCodeDemo
{
    public partial class MainForm : Form
    {
        private PivotChart pivotChart1;
        
        public MainForm()
        {
            InitializeComponent();
            InitializePivotChart();
        }
        
        private void InitializePivotChart()
        {
            // Initialize PivotChart
            pivotChart1 = new PivotChart();
            pivotChart1.Dock = DockStyle.Fill;
            
            // Add to form
            this.Controls.Add(pivotChart1);
        }
    }
}
```

**VB.NET Example:**

```vb
Imports Syncfusion.Windows.Forms.PivotChart

Public Class MainForm
    Private pivotChart1 As PivotChart
    
    Public Sub New()
        InitializeComponent()
        InitializePivotChart()
    End Sub
    
    Private Sub InitializePivotChart()
        ' Initialize PivotChart
        pivotChart1 = New PivotChart()
        pivotChart1.Dock = DockStyle.Fill
        
        ' Add to form
        Me.Controls.Add(pivotChart1)
    End Sub
End Class
```

## Basic Data Binding

To display data in Pivot Chart, configure four key properties:

### Required Properties

1. **ItemSource** - The data source (IEnumerable or DataTable)
2. **PivotAxis** - Fields for chart axis (hierarchical)
3. **PivotLegend** - Fields for series/legend
4. **PivotCalculations** - Aggregation fields

### Simple Binding Example

```csharp
using Syncfusion.PivotAnalysis.Base;

// Bind data
pivotChart1.ItemSource = ProductSales.GetSalesData();

// Configure axis fields
pivotChart1.PivotAxis.Add(new PivotItem 
{ 
    FieldMappingName = "Product", 
    TotalHeader = "Total" 
});

pivotChart1.PivotAxis.Add(new PivotItem 
{ 
    FieldMappingName = "Country", 
    TotalHeader = "Total" 
});

// Configure legend field
pivotChart1.PivotLegend.Add(new PivotItem 
{ 
    FieldMappingName = "Date", 
    TotalHeader = "Total" 
});

// Configure calculation
pivotChart1.PivotCalculations.Add(new PivotComputationInfo 
{ 
    FieldName = "Quantity", 
    Format = "#,##0" 
});
```

## Sample Data Structure

Here's a complete sample data class for testing:

```csharp
using System;
using System.Collections.Generic;

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

    public static List<ProductSales> GetSalesData()
    {
        string[] countries = { "Australia", "Germany", "Canada", "United States" };
        string[] states1 = { "New South Wales", "Queensland" };
        string[] states2 = { "Ontario", "Quebec" };
        string[] states3 = { "Bayern", "Brandenburg" };
        string[] states4 = { "New York", "Colorado", "New Mexico" };
        string[] dates = { "FY 2008", "FY 2009", "FY 2010", "FY 2011", "FY 2012" };
        string[] products = { "Bike", "Car" };

        Random r = new Random(123345);
        List<ProductSales> listOfProductSales = new List<ProductSales>();

        for (int i = 0; i < 2000; i++)
        {
            ProductSales sales = new ProductSales();
            sales.Country = countries[r.Next(countries.Length)];
            sales.Quantity = r.Next(1, 12);
            
            double discount = (30000 * sales.Quantity) * (sales.Quantity / 100.0);
            sales.Amount = (30000 * sales.Quantity) - discount;
            sales.TotalPrice = sales.Amount * sales.Quantity;
            sales.UnitPrice = sales.Amount / sales.Quantity;
            sales.Date = dates[r.Next(dates.Length)];
            sales.Product = products[r.Next(products.Length)];

            switch (sales.Country)
            {
                case "Australia":
                    sales.State = states1[r.Next(states1.Length)];
                    break;
                case "Canada":
                    sales.State = states2[r.Next(states2.Length)];
                    break;
                case "Germany":
                    sales.State = states3[r.Next(states3.Length)];
                    break;
                case "United States":
                    sales.State = states4[r.Next(states4.Length)];
                    break;
            }

            listOfProductSales.Add(sales);
        }

        return listOfProductSales;
    }
}
```

## Complete Example

Here's a complete, runnable example:

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.PivotChart;
using Syncfusion.PivotAnalysis.Base;

namespace PivotChartGettingStarted
{
    public partial class MainForm : Form
    {
        private PivotChart pivotChart1;

        public MainForm()
        {
            InitializeComponent();
            this.Text = "Pivot Chart - Getting Started";
            this.Size = new System.Drawing.Size(1000, 700);
            
            InitializePivotChart();
        }

        private void InitializePivotChart()
        {
            // Create control
            pivotChart1 = new PivotChart();
            pivotChart1.Dock = DockStyle.Fill;

            // Bind data source
            pivotChart1.ItemSource = ProductSales.GetSalesData();

            // Configure axis (hierarchical)
            pivotChart1.PivotAxis.Add(new PivotItem 
            { 
                FieldMappingName = "Product", 
                TotalHeader = "Total" 
            });
            pivotChart1.PivotAxis.Add(new PivotItem 
            { 
                FieldMappingName = "Country", 
                TotalHeader = "Total" 
            });
            pivotChart1.PivotAxis.Add(new PivotItem 
            { 
                FieldMappingName = "State", 
                TotalHeader = "Total" 
            });

            // Configure legend
            pivotChart1.PivotLegend.Add(new PivotItem 
            { 
                FieldMappingName = "Date", 
                TotalHeader = "Total" 
            });

            // Configure calculations
            pivotChart1.PivotCalculations.Add(new PivotComputationInfo 
            { 
                FieldName = "Quantity", 
                Format = "#,##0" 
            });

            // Set chart type
            pivotChart1.ChartTypes = PivotChartTypes.Column;

            // Add to form
            this.Controls.Add(pivotChart1);
        }
    }
}
```

### Using BeginUpdate and EndUpdate

For bulk data operations, suspend updates to improve performance:

```csharp
// Suspend updates
pivotChart1.BeginUpdate();

try
{
    // Perform bulk operations
    // Add/modify/remove data items
    // Change pivot configuration
    // Update multiple properties
}
finally
{
    // Resume updates - chart refreshes once
    pivotChart1.EndUpdate();
}
```

## Common Issues

### Issue: Assembly Not Found

**Error:** "Could not load file or assembly 'Syncfusion.PivotChart.Windows'"

**Solutions:**
1. Install correct NuGet package: `Syncfusion.PivotChart.Windows`
2. Verify all required assemblies are referenced
3. Check assembly versions match across all Syncfusion references
4. Clean and rebuild solution

### Issue: No Data Displayed

**Problem:** Chart appears but shows no data

**Checklist:**
1. Verify `ItemSource` is not null
2. Check that `PivotAxis`, `PivotLegend`, and `PivotCalculations` are configured
3. Ensure `FieldMappingName` matches actual property names (case-sensitive)
4. Verify data source contains data (not empty collection)
5. Check for exceptions in Output window

### Issue: Designer Error

**Error:** "Cannot create instance of PivotChart in designer"

**Solutions:**
1. Close and reopen designer
2. Clean and rebuild solution
3. Check that all required assemblies are referenced
4. Verify Syncfusion version compatibility with Visual Studio

## Next Steps

Now that you have a basic Pivot Chart running:

1. **Explore Chart Types** - Try different visualizations (Line, Area, Stacking)
2. **Enable Drill-Down** - Add interactive hierarchy navigation
3. **Customize Appearance** - Style colors, fonts, and layout
4. **Add Grouping Bar** - Let users rearrange fields interactively
5. **Implement Export** - Save charts to Excel or image formats

Refer to the specific reference files for detailed guidance on each feature.
