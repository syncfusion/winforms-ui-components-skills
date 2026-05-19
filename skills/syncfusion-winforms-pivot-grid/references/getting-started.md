# Getting Started with Pivot Grid

## Table of Contents
- [Overview](#overview)
- [License Key Registration](#license-key-registration)
- [Assembly Deployment](#assembly-deployment)
- [Adding Control via Designer](#adding-control-via-designer)
- [Adding Control via Code](#adding-control-via-code)
- [Adding Control via Syncfusion Reference Manager](#adding-control-via-syncfusion-reference-manager)
- [Basic Data Binding](#basic-data-binding)
- [Defining Pivot Structure](#defining-pivot-structure)
- [Sample Data Model](#sample-data-model)
- [Complete Working Example](#complete-working-example)

## Overview

This guide walks through the complete process of creating and configuring a Syncfusion Windows Forms Pivot Grid control. The Pivot Grid is a powerful component that transforms relational data into interactive, cross-tabulated reports with Excel-like functionality.

## License Key Registration

**Important:** Starting with v16.2.0.x, a license key is required when using Syncfusion assemblies from trial setup or NuGet feed.

Register the license key in your application's entry point before any Syncfusion control is instantiated:

```csharp
using Syncfusion.Licensing;

static class Program
{
    [STAThread]
    static void Main()
    {
        // Register Syncfusion license - MUST be called before using any Syncfusion control
        SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
        
        Application.EnableVisualStyles();
        Application.SetCompatibleTextRenderingDefault(false);
        Application.Run(new MainForm());
    }
}
```

**Get Your License Key:**
- For trial users: Available in the license confirmation email
- For paid users: Download from [Syncfusion License Keys page](https://help.syncfusion.com/common/essential-studio/licensing/overview)
- For community users: Generate from your Syncfusion account

## Assembly Deployment

The Pivot Grid control requires the following assemblies to be referenced in your project:

**Required Assemblies:**
- `Syncfusion.Grid.Windows.dll` - Base grid functionality
- `Syncfusion.PivotAnalysis.Base.dll` - Core pivot analysis logic
- `Syncfusion.PivotAnalysis.Windows.dll` - Pivot Grid control for WinForms
- `Syncfusion.Shared.Base.dll` - Common utilities and base classes

**Installation Options:**

1. **NuGet Package Manager:**
```powershell
Install-Package Syncfusion.PivotTable.WinForms -Version *
```

2. **Manual Reference:**
   - Right-click project → Add Reference → Browse
   - Navigate to Syncfusion installation folder (typically `C:\Program Files (x86)\Syncfusion\Essential Studio\<Version>\precompiledassemblies\<Framework>`)
   - Select and add the required DLLs

3. **Syncfusion Reference Manager:**
   - Use the Visual Studio extension (if installed)
   - Automatically adds all required assemblies

## Adding Control via Designer

The easiest way to add Pivot Grid is through the Visual Studio designer:

**Steps:**

1. **Create a Windows Forms Application**
   - File → New → Project → Windows Forms App (.NET Framework or .NET)
   - Name your project and select target framework

2. **Locate Pivot Grid in Toolbox**
   - After installing Syncfusion, open the Toolbox (Ctrl+Alt+X)
   - Expand "Syncfusion Controls" section
   - Find "PivotGridControl"

3. **Drag and Drop**
   - Drag PivotGridControl from Toolbox to your form
   - The control will appear on the form designer

4. **Automatic Assembly References**
   - Required assemblies are automatically added to project references
   - Verify in Solution Explorer → References node

5. **Configure Properties**
   - Select the control on the designer
   - Use Properties window (F4) to configure basic settings
   - Set Size, Location, Anchor properties as needed

**Result:** The control is now ready for data binding and configuration.

## Adding Control via Code

For programmatic control or dynamic creation, add the Pivot Grid via code-behind:

**Step 1: Add Assembly References**

Manually add references to the four required DLLs (listed above in Assembly Deployment).

**Step 2: Initialize and Add to Form**

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.PivotAnalysis;

namespace PivotGridApp
{
    public partial class MainForm : Form
    {
        private PivotGridControl pivotGridControl1;
        
        public MainForm()
        {
            InitializeComponent();
            InitializePivotGrid();
        }
        
        private void InitializePivotGrid()
        {
            // Create new instance
            pivotGridControl1 = new PivotGridControl(this.components);
            
            // Set size and position
            pivotGridControl1.Size = new Size(800, 500);
            pivotGridControl1.Location = new Point(10, 10);
            
            // Set anchoring for responsive layout
            pivotGridControl1.Anchor = AnchorStyles.Top | AnchorStyles.Left | 
                                       AnchorStyles.Right | AnchorStyles.Bottom;
            
            // Add to form's controls collection
            this.Controls.Add(pivotGridControl1);
        }
    }
}
```

**VB.NET Version:**

```vb
Imports System
Imports System.Drawing
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.PivotAnalysis

Namespace PivotGridApp
    Public Partial Class MainForm
        Inherits Form
        
        Private pivotGridControl1 As PivotGridControl
        
        Public Sub New()
            InitializeComponent()
            InitializePivotGrid()
        End Sub
        
        Private Sub InitializePivotGrid()
            ' Create new instance
            pivotGridControl1 = New PivotGridControl(Me.components)
            
            ' Set size and position
            pivotGridControl1.Size = New Size(800, 500)
            pivotGridControl1.Location = New Point(10, 10)
            
            ' Set anchoring for responsive layout
            pivotGridControl1.Anchor = AnchorStyles.Top Or AnchorStyles.Left Or 
                                       AnchorStyles.Right Or AnchorStyles.Bottom
            
            ' Add to form's controls collection
            Me.Controls.Add(pivotGridControl1)
        End Sub
    End Class
End Namespace
```

## Adding Control via Syncfusion Reference Manager

Syncfusion Reference Manager is a Visual Studio Add-In that simplifies adding Syncfusion controls:

**Steps:**

1. **Create Windows Forms Application**

2. **Open Reference Manager**
   - Right-click project in Solution Explorer
   - Select "Syncfusion Reference Manager..."

3. **Search and Select**
   - In the search box, type "pivot grid"
   - Check the PivotGridControl from results
   - Click "Done"

4. **Confirm Assembly Addition**
   - Review the assemblies to be added
   - Click "OK" to add them to your project

5. **Add Control Programmatically**
   - Use the code from "Adding Control via Code" section above

**Note:** The Reference Manager supports specific frameworks shipped with your Syncfusion version. If you see an error like "Current build v{version} is not supported this framework v{Framework Version}", ensure your project's target framework is compatible with your Syncfusion installation.

## Basic Data Binding

The Pivot Grid requires a data source to display information. Supported data source types:

- `IEnumerable<T>` (List, Collection, ObservableCollection)
- `DataTable`
- `DataView`

**Minimal Binding Example:**

```csharp
using Syncfusion.PivotAnalysis.Base;

// Assign data source
pivotGridControl1.ItemSource = GetSalesData();

// Configure rows
pivotGridControl1.PivotRows.Add(new PivotItem 
{ 
    FieldMappingName = "Product", 
    TotalHeader = "Total" 
});

// Configure columns
pivotGridControl1.PivotColumns.Add(new PivotItem 
{ 
    FieldMappingName = "Country", 
    TotalHeader = "Total" 
});

// Configure calculations (summary values)
pivotGridControl1.PivotCalculations.Add(new PivotComputationInfo 
{ 
    FieldName = "Amount", 
    Format = "C",  // Currency format
    SummaryType = SummaryType.DoubleTotalSum 
});
```

## Defining Pivot Structure

The Pivot Grid requires three key configurations:

### 1. Data Source (ItemSource)

```csharp
pivotGridControl1.ItemSource = ProductSales.GetSalesData();
```

**Requirements:**
- Must be IEnumerable or DataTable
- Should contain properties/columns for rows, columns, and calculations

### 2. Pivot Rows

Define which fields appear as row headers:

```csharp
pivotGridControl1.PivotRows.Add(new PivotItem 
{ 
    FieldMappingName = "Product",  // Property name from data source
    TotalHeader = "Total"          // Header text for total row
});

pivotGridControl1.PivotRows.Add(new PivotItem 
{ 
    FieldMappingName = "Date", 
    TotalHeader = "Total" 
});
```

### 3. Pivot Columns

Define which fields appear as column headers:

```csharp
pivotGridControl1.PivotColumns.Add(new PivotItem 
{ 
    FieldMappingName = "Country", 
    TotalHeader = "Total" 
});

pivotGridControl1.PivotColumns.Add(new PivotItem 
{ 
    FieldMappingName = "State", 
    TotalHeader = "Total" 
});
```

### 4. Pivot Calculations

Define which fields to aggregate in value cells:

```csharp
// Add Amount calculation with currency formatting
pivotGridControl1.PivotCalculations.Add(new PivotComputationInfo 
{ 
    FieldName = "Amount",                   // Field to aggregate
    Format = "C",                           // Display format (Currency)
    SummaryType = SummaryType.DoubleTotalSum  // Aggregation type
});

// Add Quantity calculation with numeric formatting
pivotGridControl1.PivotCalculations.Add(new PivotComputationInfo 
{ 
    FieldName = "Quantity", 
    Format = "#,##0"  // Display format (Number with thousands separator)
});
```

**Common SummaryType Values:**
- `DoubleTotalSum` - Sum of all values
- `Count` - Count of items
- `DoubleAverage` - Average value
- `DoubleMax` - Maximum value
- `DoubleMin` - Minimum value
- `DisplayIfDiscreteValuesEqual` - Display unique value if all are same

## Sample Data Model

Here's a complete example of a data model suitable for Pivot Grid:

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
        string[] countries = { "Australia", "Canada", "France", "Germany", 
                              "United Kingdom", "United States" };
        string[] products = { "Bike", "Car" };
        string[] dates = { "FY 2023", "FY 2024", "FY 2025" };
        
        Random r = new Random(123345345);
        List<ProductSales> salesData = new List<ProductSales>();
        
        for (int i = 0; i < 1000; i++)
        {
            ProductSales sales = new ProductSales
            {
                Product = products[r.Next(products.Length)],
                Date = dates[r.Next(dates.Length)],
                Country = countries[r.Next(countries.Length)],
                State = "State-" + r.Next(1, 5),
                Quantity = r.Next(1, 12),
            };
            
            // Calculate amounts
            double discount = (30000 * sales.Quantity) * (sales.Quantity / 100.0);
            sales.Amount = (30000 * sales.Quantity) - discount;
            sales.UnitPrice = sales.Amount / sales.Quantity;
            sales.TotalPrice = sales.Amount * sales.Quantity;
            
            salesData.Add(sales);
        }
        
        return salesData;
    }
}
```

## Complete Working Example

Here's a full, runnable example that creates a functional Pivot Grid:

```csharp
using System;
using System.Collections.Generic;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.PivotAnalysis;
using Syncfusion.PivotAnalysis.Base;
using Syncfusion.Windows.Forms;

namespace PivotGridExample
{
    public partial class MainForm : Form
    {
        private PivotGridControl pivotGridControl1;
        
        public MainForm()
        {
            InitializeComponent();
            this.Text = "Pivot Grid Demo";
            this.Size = new Size(1000, 600);
            
            InitializePivotGrid();
        }
        
        private void InitializePivotGrid()
        {
            // Create control
            pivotGridControl1 = new PivotGridControl(this.components);
            pivotGridControl1.Location = new Point(10, 10);
            pivotGridControl1.Size = new Size(960, 540);
            pivotGridControl1.Anchor = AnchorStyles.Top | AnchorStyles.Left | 
                                       AnchorStyles.Right | AnchorStyles.Bottom;
            
            // Apply visual style
            pivotGridControl1.GridVisualStyles = GridVisualStyles.Metro;
            
            // Bind data
            pivotGridControl1.ItemSource = ProductSales.GetSalesData();
            
            // Configure rows
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
            
            // Configure columns
            pivotGridControl1.PivotColumns.Add(new PivotItem 
            { 
                FieldMappingName = "Country", 
                TotalHeader = "Total" 
            });
            
            // Configure calculations
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
            
            // Add to form
            this.Controls.Add(pivotGridControl1);
        }
    }
}
```

**Result:** Running this application displays a fully functional pivot grid with:
- Products and dates as row headers
- Countries as column headers
- Sum of amounts and quantities in value cells
- Total rows and columns
- Metro visual style

## Next Steps

Now that you have a basic pivot grid running:

1. **Enable Interactive Features:**
   - Add Pivot Schema Designer: `pivotGridControl1.ShowPivotTableFieldList = true;`
   - Enable Grouping Bar: `pivotGridControl1.ShowGroupBar = true;`
   - Enable Filtering: `pivotGridControl1.AllowFiltering = true;`
   - Enable Sorting: `pivotGridControl1.AllowSorting = true;`

2. **Explore Advanced Features:**
   - Configure drill-down navigation
   - Apply conditional formatting
   - Export to Excel/PDF
   - Customize appearance and themes

3. **Read Reference Documentation:**
   - Data binding options
   - Pivot calculations and expressions
   - Filtering and sorting strategies
   - Export and printing capabilities
