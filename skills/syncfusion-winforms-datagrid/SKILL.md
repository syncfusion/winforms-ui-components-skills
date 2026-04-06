---
name: syncfusion-winforms-datagrid
description: Implements Syncfusion WinForms SfDataGrid component for displaying and managing tabular data in Windows Forms applications. Use this when working with data grids, column management (auto-generation, stacked headers), data operations (filtering, sorting, grouping), or grid editing with validation. The skill covers data summaries, selection modes, export capabilities (Excel/PDF), conditional styling, master-detail views, and drag-and-drop functionality.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WinForms DataGrid

A comprehensive guide for implementing the Syncfusion Windows Forms DataGrid (SfDataGrid) component, a powerful control for displaying and manipulating tabular data with features like columns, filtering, sorting, grouping, editing, summaries, and export capabilities.

## When to Use This Skill

Use this skill when you need to:
- Display tabular data in Windows Forms applications
- Define and customize columns (text, numeric, date, checkbox, combobox, etc.)
- Implement data operations like filtering, sorting, and grouping
- Enable in-grid editing with validation
- Display data summaries and aggregations
- Implement selection functionality (single, multiple, row, cell)
- Export grid data to Excel or PDF
- Apply conditional styling based on data values
- Create master-detail views with hierarchical data
- Enable drag-and-drop operations in grids
- Localize grid text and messages
- Serialize and deserialize grid state

## Component Overview

The Syncfusion WinForms DataGrid (SfDataGrid) is a high-performance data grid control that provides:
- **Column Management**: Auto-generation, manual definition, stacked headers, column types
- **Data Operations**: Filtering (Excel-like UI), sorting, grouping with expand/collapse
- **Editing**: In-grid editing with various modes and validation
- **Summaries**: Group summaries, table summaries, caption summaries
- **Selection**: Row, cell, or any selection with multiple modes
- **Export**: Excel and PDF export with customization
- **Styling**: Conditional styling, custom cell rendering
- **Advanced**: Master-detail views, drag-drop, localization, serialization

### Creating the DataGrid Control

Create the SfDataGrid control programmatically:

```csharp
using Syncfusion.WinForms.DataGrid;

namespace WindowsFormsApplication1
{
    public partial class Form1 : Form
    {
        private SfDataGrid sfDataGrid1;
        
        public Form1()
        {
            InitializeComponent();
            
            // Create SfDataGrid instance
            sfDataGrid1 = new SfDataGrid();
            
            // Add to form's controls collection
            this.Controls.Add(sfDataGrid1);
        }
    }
}
```

## Documentation and Navigation Guide

### Getting Started

**📄 Read:** [references/getting-started.md](references/getting-started.md)

When you need to:
- Set up the SfDataGrid control for the first time
- Understand assembly deployment and NuGet package installation
- Create the control via Designer or in code
- Create data models and collections
- Bind data to the grid
- Configure auto-generated or manually defined columns
- Handle TableControl events (KeyDown, MouseDown)
- Enable basic features (sorting, filtering, grouping, editing)
- See a complete working example

### Column Management

**📄 Read:** [references/columns.md](references/columns.md)

When you need to:
- Define columns (auto-generate vs manual definition)
- Work with different column types (GridTextColumn, GridNumericColumn, GridDateTimeColumn, GridCheckBoxColumn, GridComboBoxColumn, etc.)
- Customize auto-generated columns or use Data Annotations
- Add, remove, or reorder columns programmatically
- Create stacked headers spanning multiple columns
- Hide, show, or resize columns
- Enable column drag-and-drop for reordering

### Editing Data

**📄 Read:** [references/editing.md](references/editing.md)

When you need to:
- Enable or disable editing for the grid or specific columns
- Configure edit modes (single-click, double-click, F2 key)
- Handle edit events (begin edit, end edit, value changed)
- Implement validation during editing
- Programmatically start, end, or cancel editing
- Customize cursor placement in edit mode
- Change cell values programmatically

### Filtering

**📄 Read:** [references/filtering.md](references/filtering.md)

When you need to:
- Implement programmatic filtering (View filtering, Column filtering)
- Enable Excel-like filter UI for users
- Apply different filter types (string, numeric, date)
- Use filter predicates with multiple conditions
- Handle filter events
- Clear or modify existing filters
- Troubleshoot filtering issues

### Sorting

**📄 Read:** [references/sorting.md](references/sorting.md)

When you need to:
- Enable UI sorting with click actions
- Apply programmatic sorting (single or multi-column)
- Implement custom sort comparers
- Handle sort events
- Configure tri-state sorting
- Clear or modify existing sorts
- Troubleshoot sorting issues

### Grouping

**📄 Read:** [references/grouping.md](references/grouping.md)

When you need to:
- Enable UI grouping with drag-drop
- Group data programmatically (single or multi-level)
- Customize group captions and formatting
- Expand/collapse groups
- Implement custom grouping logic with KeySelector
- Handle group events
- Troubleshoot grouping issues

### Selection

**📄 Read:** [references/selection.md](references/selection.md)

When you need to:
- Configure selection modes (single, multiple, extended)
- Set selection units (row, cell, any)
- Select rows, columns, or cells programmatically
- Handle selection change events
- Implement CheckBox-based selection
- Disable selection for specific rows or columns
- Work with current cell and selected items

### Data Summaries

**📄 Read:** [references/summaries.md](references/summaries.md)

When you need to:
- Display group summaries (sum, average, count, min, max)
- Add table summaries at top or bottom
- Customize summary rows and formats
- Create caption summaries for groups
- Implement custom aggregate functions
- Position and style summary rows

### Export to Excel and PDF

**📄 Read:** [references/export.md](references/export.md)

When you need to:
- Export grid data to Excel or PDF
- Customize export appearance and formatting
- Export selected rows or filtered data
- Include or exclude specific columns in export
- Handle export events for customization
- Export with styles and themes
- Configure page settings for PDF export

### Conditional Styling and Custom Rendering

**📄 Read:** [references/styling.md](references/styling.md)

When you need to:
- Apply conditional styling based on cell values or row data
- Customize cell, row, and column appearance
- Handle DrawCell event for custom rendering
- Apply themes and visual styles
- Style specific cell types differently
- Implement alternating row colors
- Create custom cell renderers

### Advanced Features

**📄 Read:** [references/advanced-features.md](references/advanced-features.md)

When you need to:
- Implement master-detail views with hierarchical data
- Enable drag-and-drop operations (rows, columns)
- Localize grid text and messages for different cultures
- Serialize and deserialize grid state (columns, sorting, grouping, filtering)
- Configure grid settings and preferences
- Handle complex hierarchical data structures

## Quick Start Example

Here's a complete step-by-step example to create a working WinForms DataGrid application from scratch:

### Step 1: Create Data Model Classes

First, create classes to represent your data:

```csharp
using System;
using System.Collections.ObjectModel;

public class OrderInfo
{
    public int OrderID { get; set; }
    public string CustomerID { get; set; }
    public string CustomerName { get; set; }
    public string Country { get; set; }
    public string ShipCity { get; set; }
    
    public OrderInfo(int orderId, string customerName, string country, string customerId, string shipCity)
    {
        this.OrderID = orderId;
        this.CustomerName = customerName;
        this.Country = country;
        this.CustomerID = customerId;
        this.ShipCity = shipCity;
    }
}

public class OrderInfoCollection
{
    private ObservableCollection<OrderInfo> _orders;
    
    public ObservableCollection<OrderInfo> Orders
    {
        get { return _orders; }
        set { _orders = value; }
    }
    
    public OrderInfoCollection()
    {
        _orders = new ObservableCollection<OrderInfo>();
        GenerateOrders();
    }
    
    private void GenerateOrders()
    {
        _orders.Add(new OrderInfo(1001, "Maria Anders", "Germany", "ALFKI", "Berlin"));
        _orders.Add(new OrderInfo(1002, "Ana Trujilo", "Mexico", "ANATR", "Mexico D.F."));
        _orders.Add(new OrderInfo(1003, "Antonio Moreno", "Mexico", "ANTON", "Mexico D.F."));
        _orders.Add(new OrderInfo(1004, "Thomas Hardy", "UK", "AROUT", "London"));
        _orders.Add(new OrderInfo(1005, "Christina Berglund", "Sweden", "BERGS", "Lula"));
        _orders.Add(new OrderInfo(1006, "Hanna Moos", "Germany", "BLAUS", "Mannheim"));
        _orders.Add(new OrderInfo(1007, "Frederique Citeaux", "France", "BLONP", "Strasbourg"));
        _orders.Add(new OrderInfo(1008, "Martin Sommer", "Spain", "BOLID", "Madrid"));
        _orders.Add(new OrderInfo(1009, "Laurence Lebihan", "France", "BONAP", "Marseille"));
        _orders.Add(new OrderInfo(1010, "Elizabeth Lincoln", "Canada", "BOTTM", "Tsawassen"));
    }
}
```

### Step 2: Create and Configure the DataGrid in Your Form

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.WinForms.DataGrid;
using Syncfusion.WinForms.DataGrid.Enums;

namespace WindowsFormsApplication1
{
    public partial class Form1 : Form
    {
        private SfDataGrid sfDataGrid1;
        
        public Form1()
        {
            InitializeComponent();
            InitializeDataGrid();
        }
        
        private void InitializeDataGrid()
        {
            // Create DataGrid instance
            sfDataGrid1 = new SfDataGrid();
            sfDataGrid1.Dock = DockStyle.Fill;
            
            // Create and bind data source
            OrderInfoCollection collection = new OrderInfoCollection();
            sfDataGrid1.DataSource = collection.Orders;
            
            // Enable features
            sfDataGrid1.AllowEditing = true;
            sfDataGrid1.AllowFiltering = true;
            sfDataGrid1.AllowSorting = true;
            sfDataGrid1.AllowGrouping = true;
            sfDataGrid1.ShowGroupDropArea = true;
            
            // Configure selection
            sfDataGrid1.SelectionMode = GridSelectionMode.Extended;
            
            // Auto-generate columns from data model
            sfDataGrid1.AutoGenerateColumns = true;
            
            // Add to form
            this.Controls.Add(sfDataGrid1);
        }
    }
}
```

### Step 3: Define Columns Manually (Optional)

If you want full control over columns instead of auto-generation:

```csharp
private void InitializeDataGrid()
{
    sfDataGrid1 = new SfDataGrid();
    sfDataGrid1.Dock = DockStyle.Fill;
    
    // Disable auto-generation to define columns manually
    sfDataGrid1.AutoGenerateColumns = false;
    
    // Add columns with specific properties
    sfDataGrid1.Columns.Add(new GridTextColumn() 
    { 
        MappingName = "OrderID", 
        HeaderText = "Order ID",
        Width = 100,
        AllowEditing = false // Make this column read-only
    });
    
    sfDataGrid1.Columns.Add(new GridTextColumn() 
    { 
        MappingName = "CustomerID", 
        HeaderText = "Customer ID",
        Width = 120
    });
    
    sfDataGrid1.Columns.Add(new GridTextColumn() 
    { 
        MappingName = "CustomerName", 
        HeaderText = "Customer Name",
        Width = 150
    });
    
    sfDataGrid1.Columns.Add(new GridTextColumn() 
    { 
        MappingName = "Country", 
        HeaderText = "Country",
        Width = 120
    });
    
    sfDataGrid1.Columns.Add(new GridTextColumn() 
    { 
        MappingName = "ShipCity", 
        HeaderText = "Ship City",
        Width = 120
    });
    
    // Bind data source
    OrderInfoCollection collection = new OrderInfoCollection();
    sfDataGrid1.DataSource = collection.Orders;
    
    // Enable features
    sfDataGrid1.AllowSorting = true;
    sfDataGrid1.AllowFiltering = true;
    sfDataGrid1.AllowGrouping = true;
    sfDataGrid1.ShowGroupDropArea = true;
    
    // Add to form
    this.Controls.Add(sfDataGrid1);
}
```

### What You'll See

When you run the application:
- A data grid displaying 10 orders with customer information
- Click column headers to **sort**
- Click filter icons to **filter** data
- Drag column headers to the drop area to **group** by that column
- Double-click cells to **edit** (if AllowEditing is true)
- Multiple selection support with Ctrl/Shift keys

## Basic Data Operations

### Binding Different Data Sources

The DataGrid supports various data source types:

```csharp
// Bind to ObservableCollection (recommended for automatic UI updates)
ObservableCollection<OrderInfo> orders = new ObservableCollection<OrderInfo>();
sfDataGrid1.DataSource = orders;

// Bind to List
List<OrderInfo> orderList = new List<OrderInfo>();
sfDataGrid1.DataSource = orderList;

// Bind to DataTable
DataTable dataTable = new DataTable();
sfDataGrid1.DataSource = dataTable;

// Bind to BindingList
BindingList<OrderInfo> bindingList = new BindingList<OrderInfo>();
sfDataGrid1.DataSource = bindingList;
```

### Handling Selection Events

```csharp
// Handle selection changed
sfDataGrid1.SelectionChanged += (sender, e) =>
{
    if (sfDataGrid1.SelectedItem != null)
    {
        var selectedOrder = sfDataGrid1.SelectedItem as OrderInfo;
        Console.WriteLine($"Selected: {selectedOrder.CustomerName}");
    }
};

// Handle cell value changed
sfDataGrid1.CurrentCellValueChanged += (sender, e) =>
{
    var rowData = sfDataGrid1.GetRecordAtRowIndex(e.RowColumnIndex.RowIndex);
    Console.WriteLine($"Cell value changed in row: {e.RowColumnIndex.RowIndex}");
};
```

## Common Patterns

### Manual Column Definition with Selection

```csharp
sfDataGrid.AutoGenerateColumns = false;

// Add columns
sfDataGrid.Columns.Add(new GridTextColumn() 
{ 
    MappingName = "OrderID", 
    HeaderText = "Order ID",
    AllowEditing = false 
});

sfDataGrid.Columns.Add(new GridNumericColumn() 
{ 
    MappingName = "Quantity", 
    HeaderText = "Qty"
});

// Configure selection
sfDataGrid.SelectionMode = GridSelectionMode.Multiple;
sfDataGrid.SelectionUnit = GridSelectionUnit.Row;
```

## Key Properties

### Core Grid Properties
- `DataSource` - The data source to bind
- `AutoGenerateColumns` - Enable/disable automatic column generation
- `AllowEditing` - Enable/disable editing
- `AllowFiltering` - Enable/disable filtering
- `AllowSorting` - Enable/disable sorting
- `AllowGrouping` - Enable/disable grouping
- `AllowDraggingColumns` - Enable/disable column reordering
- `AllowDraggingRows` - Enable/disable row reordering

### Selection Properties
- `SelectionMode` - Single, Multiple, Extended, None
- `SelectionUnit` - Row, Cell, Any
- `SelectedItem` - Get/set selected item
- `SelectedItems` - Get/set multiple selected items

### Visual Properties
- `HeaderRowHeight` - Height of header row
- `RowHeight` - Default row height
- `Style` - Grid styling options
- `ShowGroupDropArea` - Show/hide group drop area
- `ShowRowHeader` - Show/hide row headers

## Common Use Cases

### Use Case 1: E-commerce Order Management
Display orders with filtering by status, sorting by date, grouping by customer, editing for updates, and conditional styling for priority orders.

### Use Case 2: Financial Reporting
Show transaction data with summaries (totals, averages), export to Excel/PDF, conditional styling for negative values, and master-detail for transaction details.

### Use Case 3: Inventory Management
Track products with filtering by category, low-stock conditional styling, editing for quantity updates, and selection for bulk operations.

### Use Case 4: Customer Relationship Management
Manage contacts with multi-column sorting, grouping by region, export capabilities, and custom rendering for status indicators.

### Use Case 5: Project Management
Display tasks with master-detail for subtasks, drag-drop for reordering, conditional styling by status, and summaries for progress tracking.

## Troubleshooting

For common issues and solutions:
- **Column not displaying**: Check MappingName matches property name exactly (case-sensitive)
- **Editing not working**: Verify both grid-level and column-level AllowEditing properties
- **Filter not applying**: Ensure AllowFiltering is enabled and call RefreshFilter() after programmatic changes
- **Export formatting issues**: Customize export options and handle export events
- **Styling not visible**: Ensure DrawCell event is wired and Style properties are set correctly
- **Selection not working**: Check SelectionMode and SelectionUnit are properly configured

Refer to the specific reference files above for detailed troubleshooting guidance in each feature area.

---

## Next Steps

Start with **[Getting Started](references/getting-started.md)** to install and configure the SfDataGrid, then explore specific features through the navigation guide above based on your application requirements.
