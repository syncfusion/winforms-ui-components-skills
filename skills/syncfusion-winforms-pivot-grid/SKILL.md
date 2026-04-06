---
name: syncfusion-winforms-pivot-grid
description: Guide for implementing Syncfusion Windows Forms Pivot Grid control for data analysis and pivot table functionality. Use this skill when implementing pivot tables, data summarization, cross-tabulated data, or analytical dashboards in Windows Forms applications. Covers data binding, pivot configuration, filtering, sorting, grouping, calculations, conditional formatting, and exporting.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Syncfusion Windows Forms Pivot Grid

The Syncfusion Windows Forms Pivot Grid control is a powerful, Excel-like pivot table component that enables interactive data analysis and cross-tabulated reporting. It transforms large datasets into meaningful summaries with drag-and-drop field organization, drill-down navigation, and advanced calculations.

## When to Use This Skill

Use this skill when you need to:
- Implement pivot table functionality in Windows Forms applications
- Build business intelligence dashboards with data summarization
- Create analytical reports with hierarchical grouping and calculations
- Provide interactive data exploration with drill-down/drill-up navigation
- Display cross-tabulated data from relational or OLAP sources
- Enable users to dynamically reorganize data fields
- Export pivot analysis to Excel, PDF, or Word formats
- Apply conditional formatting based on cell values
- Implement custom aggregations and calculated fields
- Build financial reports, sales dashboards, or KPI tracking interfaces

## Component Overview

**Key Features:**
- **Excel-Like Pivot Tables** - Familiar interface for data pivoting and analysis
- **Interactive Schema Designer** - Built-in Pivot Table Field List for drag-and-drop field management
- **Multiple Data Sources** - Supports IList, IEnumerable, DataTable, DataView, OLAP
- **Drill-Down Navigation** - Interactive expansion/collapse of hierarchical data
- **Advanced Calculations** - Sum, Average, Count, Min, Max, custom expressions, running totals
- **Filtering & Sorting** - Label filters, value filters, multi-level sorting
- **Grouping Bar** - Visual field organization with drag-and-drop
- **Conditional Formatting** - Style cells based on rules and conditions
- **Export Capabilities** - Export to Excel, PDF, Word with formatting preserved
- **Cell Selection** - Single, multiple, range selection modes
- **Editing Support** - Modify cell values and update aggregations
- **Touch Support** - Touch-friendly interactions for tablet devices
- **Localization** - Multi-language support

**Namespace:** `Syncfusion.Windows.Forms.PivotAnalysis`

**Assembly Dependencies:**
- `Syncfusion.Grid.Windows.dll`
- `Syncfusion.PivotAnalysis.Base.dll`
- `Syncfusion.PivotAnalysis.Windows.dll`
- `Syncfusion.Shared.Base.dll`

**NuGet Guidance:**
- Install the latest available version of Syncfusion.PivotTable.WinForms
- Prefer NuGet package references over manual DLL references when possible
- Verify package compatibility with the project target framework before upgrading

**Important UI Setup Note:**
- Always create the control with `new PivotGridControl(this.components);` when working inside a WinForms form.
- Always set `Location` and `Size` first, and ensure the control is larger than 300 x 300 before enabling `ShowPivotTableFieldList`.
- Set `ShowPivotTableFieldList = true` only after the control has been sized correctly.
- This prevents the field list UI from rendering in an unusable layout on small controls.

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly deployment and required dependencies
- Adding Pivot Grid via Visual Studio designer
- Adding Pivot Grid via code-behind
- License key registration
- Basic control initialization
- Initial configuration and setup

### Data Management
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- Binding to IEnumerable collections (List<T>, ObservableCollection)
- Binding to DataTable and DataView
- Binding to OLAP data sources
- Dynamic data updates and refresh
- Data source requirements and best practices
- Performance optimization for large datasets

### Pivot Schema Designer
📄 **Read:** [references/pivot-schema-designer.md](references/pivot-schema-designer.md)
- Enabling Pivot Table Field List
- Drag-and-drop field organization
- Field configuration and customization
- Showing/hiding the schema designer
- Programmatic field manipulation
- Custom field templates

### Rows, Columns, and Summaries
📄 **Read:** [references/rows-columns-summaries.md](references/rows-columns-summaries.md)
- Configuring pivot rows (PivotRows collection)
- Configuring pivot columns (PivotColumns collection)
- Summary types (Sum, Average, Count, Min, Max, etc.)
- Custom aggregation functions
- Calculation information and formatting
- Hierarchical structures and nesting
- Total headers and subtotals

### Filtering and Sorting
📄 **Read:** [references/filtering-sorting.md](references/filtering-sorting.md)
- Filter types (Label filter, Value filter)
- Interactive filtering with UI
- Programmatic filter configuration
- Filter conditions and predicates
- Sort modes (Ascending, Descending, Custom)
- Multi-level sorting strategies
- Clearing and resetting filters

### Grouping and Layouts
📄 **Read:** [references/grouping-layouts.md](references/grouping-layouts.md)
- Grouping bar functionality and visibility
- Layout types (Normal, Top summary, No summaries, etc.)
- Drill-down and drill-up navigation
- Expand/collapse behaviors and events
- Custom layout configurations
- Performance optimization for grouped data

### Calculations and Editing
📄 **Read:** [references/calculations-editing.md](references/calculations-editing.md)
- Pivot calculations (Difference, % of, Running total, etc.)
- Creating expression fields and custom formulas
- Editing cell values interactively
- Updating aggregated values programmatically
- Validation rules and constraints
- Calculated fields and custom aggregations

### Cell Selection and Formatting
📄 **Read:** [references/cell-selection-formatting.md](references/cell-selection-formatting.md)
- Cell selection modes (Single, Range, Multiple)
- Selection events and handling
- Conditional formatting rules and conditions
- Format conditions based on cell values
- Styling cells, rows, and columns
- Custom cell appearance and templates

### Data Exploration
📄 **Read:** [references/data-exploration.md](references/data-exploration.md)
- Hyperlink cells for navigation
- Drill-through functionality to view details
- Data visualization techniques
- Interactive navigation patterns
- User interaction events
- Tooltips and cell value display

### Exporting and Printing
📄 **Read:** [references/exporting-printing.md](references/exporting-printing.md)
- Exporting to Excel (XLS, XLSX)
- Exporting to PDF with formatting
- Exporting to Word documents
- Print configuration and preview
- Custom export options and templates
- Handling large exports efficiently

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- Asynchronous data processing for large datasets
- Serialization and deserialization of pivot state
- Freezing row/column headers
- Touch support for tablet devices
- Localization and right-to-left (RTL) support
- Custom cell templates and renderers
- Performance tuning and optimization

## Quick Start Example

### Basic Pivot Grid with Data Binding

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
        private PivotGridControl pivotGridControl1;
        
        public MainForm()
        {
            InitializeComponent();
            InitializePivotGrid();
        }
        
        private void InitializePivotGrid()
        {
            // Create pivot grid instance using the form components container
            pivotGridControl1 = new PivotGridControl(this.components);
            
            // Set size and position first
            pivotGridControl1.Location = new System.Drawing.Point(10, 10);
            pivotGridControl1.Size = new System.Drawing.Size(800, 500);
            pivotGridControl1.Anchor = AnchorStyles.Top | AnchorStyles.Left | 
                                       AnchorStyles.Right | AnchorStyles.Bottom;
            
            // Apply visual style
            pivotGridControl1.GridVisualStyles = GridVisualStyles.Metro;
            
            // Bind data source
            pivotGridControl1.ItemSource = GetSalesData();
            
            // Configure pivot rows
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
            
            // Configure pivot columns
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
            pivotGridControl1.PivotCalculations.Add(new PivotComputationInfo 
            { 
                FieldName = "Quantity", 
                Format = "#,##0" 
            });

            // Enable the field list only after the control has been sized correctly
            pivotGridControl1.ShowPivotTableFieldList = true;
            
            // Add to form
            this.Controls.Add(pivotGridControl1);
        }
        
        private List<ProductSales> GetSalesData()
        {
            // Sample data for demonstration
            return new List<ProductSales>
            {
                new ProductSales 
                { 
                    Product = "Bike", 
                    Date = "FY 2023", 
                    Country = "United States", 
                    Quantity = 120, 
                    Amount = 24000 
                },
                new ProductSales 
                { 
                    Product = "Car", 
                    Date = "FY 2023", 
                    Country = "Canada", 
                    Quantity = 45, 
                    Amount = 135000 
                },
                // Add more data...
            };
        }
    }
    
    public class ProductSales
    {
        public string Product { get; set; }
        public string Date { get; set; }
        public string Country { get; set; }
        public int Quantity { get; set; }
        public double Amount { get; set; }
    }
}
```

## Common Patterns

### Pattern 1: Enable Pivot Schema Designer

```csharp
using Syncfusion.Windows.Forms.PivotAnalysis;

// Show the Pivot Table Field List (schema designer)
pivotGridControl1.ShowPivotTableFieldList = true;

// Users can now drag and drop fields to reorganize the pivot structure
// This provides Excel-like field management functionality
```

### Pattern 2: Apply Conditional Formatting

```csharp
using Syncfusion.Windows.Forms.PivotAnalysis;

// Add conditional formatting rule
PivotCellStyle style = new PivotCellStyle();
style.BackColor = Color.LightCoral;
style.ForeColor = Color.White;

ConditionalFormat condition = new ConditionalFormat();
condition.Conditions.Add(new PivotCondition 
{ 
    ConditionType = PivotConditionType.LessThan, 
    PredicateValue = 10000 
});
condition.ApplyStyleInfo = style;

pivotGridControl1.TableControl.ConditionalFormats.Add(condition);
```

### Pattern 3: Export to Excel

```csharp
using Syncfusion.Windows.Forms.PivotAnalysis;

// Export pivot grid to Excel file
pivotGridControl1.ExportToExcel("PivotReport.xlsx");

// Export with options
ExcelExportOptions options = new ExcelExportOptions();
options.ExportMode = ExportMode.Value; // or ExportMode.Text
pivotGridControl1.ExportToExcel("PivotReport.xlsx", options);
```

### Pattern 4: Handle Drill-Down Events

```csharp
using Syncfusion.Windows.Forms.PivotAnalysis;

// Subscribe to drill-down event
pivotGridControl1.HyperlinkCellClick += PivotGridControl1_HyperlinkCellClick;

private void PivotGridControl1_HyperlinkCellClick(object sender, 
    HyperlinkCellClickEventArgs e)
{
    // Get the clicked cell information
    string cellValue = e.Text;
    int rowIndex = e.RowIndex;
    int colIndex = e.ColIndex;
    
    // Show drill-through data or navigate to details
    // e.Cancel = true; // Cancel default drill behavior if needed
}
```

### Pattern 5: Programmatic Filtering

```csharp
using Syncfusion.Windows.Forms.PivotAnalysis;
using Syncfusion.PivotAnalysis.Base;

// Add filter to a specific field
PivotItem productRow = pivotGridControl1.PivotRows[0];
productRow.FilterItems = new List<string> { "Bike", "Car" };

// Or use filter expressions
FilterExpression filter = new FilterExpression();
filter.FieldName = "Product";
filter.FilterItems.Add("Bike");
filter.FilterItems.Add("Car");
pivotGridControl1.Filters.Add(filter);

// Refresh the pivot grid to apply filters
pivotGridControl1.TableControl.Refresh();
```

### Pattern 6: Custom Aggregation

```csharp
using Syncfusion.PivotAnalysis.Base;

// Create custom calculation with expression
PivotComputationInfo customCalc = new PivotComputationInfo
{
    FieldName = "Profit",
    CalculationType = CalculationType.Custom,
    Format = "C",
    SummaryType = SummaryType.Custom
};

// Define custom summary calculation
customCalc.CustomSummaryMethod = (items) =>
{
    double total = 0;
    foreach (var item in items)
    {
        total += (double)item.GetType().GetProperty("Amount").GetValue(item, null);
    }
    return total * 0.15; // Calculate 15% profit margin
};

pivotGridControl1.PivotCalculations.Add(customCalc);
```

## Common Use Cases

1. **Sales Analysis Dashboard**
   - Track sales by product, region, and time period
   - Compare performance across multiple dimensions
   - Identify trends and patterns in sales data

2. **Financial Reporting**
   - Budget vs. actual analysis
   - Expense tracking and categorization
   - Revenue breakdown by department/product

3. **Inventory Management**
   - Stock levels by location and product category
   - Movement analysis and turnover rates
   - Reorder point calculations

4. **Business Intelligence**
   - KPI dashboards with drill-down capabilities
   - Cross-functional data analysis
   - Executive summary reports

5. **Customer Analytics**
   - Customer segmentation and profiling
   - Purchase pattern analysis
   - Retention and churn metrics

6. **Operational Reports**
   - Production efficiency tracking
   - Resource utilization analysis
   - Quality metrics and defect tracking

## Key Props and Configuration

**Essential Properties:**
- `ItemSource` - Data source for the pivot grid (IEnumerable, DataTable, etc.)
- `PivotRows` - Collection of fields displayed as row headers
- `PivotColumns` - Collection of fields displayed as column headers
- `PivotCalculations` - Summary calculations (Sum, Average, Count, etc.)
- `ShowPivotTableFieldList` - Enable/disable the schema designer
- `GridVisualStyles` - Apply visual themes (Metro, Office2016, etc.) 

**Customization Properties:**
- `EnableDrillDown` - Allow users to expand/collapse grouped data
- `ShowGrandTotals` - Display grand total rows and columns
- `ShowSubTotals` - Display subtotal rows and columns
- `AllowSelection` - Enable cell selection
- `AllowFiltering` - Enable filtering UI
- `AllowSorting` - Enable sorting functionality

Refer to the reference files above for detailed documentation on all properties, methods, and events.
