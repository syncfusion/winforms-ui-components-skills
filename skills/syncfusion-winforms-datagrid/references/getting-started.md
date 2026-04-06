# Getting Started with WinForms DataGrid

This guide covers the essential steps to get started with the Syncfusion WinForms DataGrid (SfDataGrid) control, including assembly deployment, control creation, data binding, and basic configuration.

## Table of Contents
- [Assembly Deployment](#assembly-deployment)
- [Creating the Control](#creating-the-control)
- [Creating Data Models](#creating-data-models)
- [Binding Data](#binding-data)
- [Defining Columns](#defining-columns)
- [Handling Events](#handling-events)
- [Basic Features Setup](#basic-features-setup)

## Assembly Deployment

### Required Assemblies

To use the SfDataGrid control, you need to add references to the following assemblies:

- `Syncfusion.SfDataGrid.WinForms.dll`
- `Syncfusion.Core.WinForms.dll`
- `Syncfusion.SfInput.WinForms.dll`
- `Syncfusion.SfListView.WinForms.dll`
- `Syncfusion.Data.WinForms.dll`
- `Syncfusion.GridCommon.WinForms.dll`

### NuGet Package Installation

Install the NuGet package in your WinForms application:

```powershell
Install-Package Syncfusion.SfDataGrid.WinForms
```

This will automatically add all required assembly references to your project.

### Required Namespaces

Add these namespace imports at the top of your code file:

```csharp
using Syncfusion.WinForms.DataGrid;
using Syncfusion.WinForms.DataGrid.Enums;
using Syncfusion.WinForms.DataGrid.Events;
```

## Creating the Control

### Adding via Designer

You can add the SfDataGrid control to your form using Visual Studio Designer:

1. Open your Windows Forms project in Visual Studio
2. Open the Toolbox (View > Toolbox)
3. Locate **SfDataGrid** in the Syncfusion WinForms section
4. Drag and drop the control onto your form
5. The required assembly references will be added automatically

### Adding in Code

To add the SfDataGrid control programmatically:

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.WinForms.DataGrid;

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
            // Create SfDataGrid instance
            sfDataGrid1 = new SfDataGrid();
            
            // Set location and size
            sfDataGrid1.Location = new System.Drawing.Point(85, 108);
            sfDataGrid1.Size = new System.Drawing.Size(240, 150);
            
            // Or use Dock to fill the form
            // sfDataGrid1.Dock = DockStyle.Fill;
            
            // Add to form's controls collection
            this.Controls.Add(sfDataGrid1);
        }
    }
}
```

**VB.NET:**

```vb
Imports Syncfusion.WinForms.DataGrid

Namespace WindowsFormsApplication1
    Partial Public Class Form1
        Inherits Form
        Private sfDataGrid1 As SfDataGrid
        
        Public Sub New()
            InitializeComponent()
            InitializeDataGrid()
        End Sub
        
        Private Sub InitializeDataGrid()
            ' Create SfDataGrid instance
            sfDataGrid1 = New SfDataGrid()
            
            ' Set location and size
            sfDataGrid1.Location = New System.Drawing.Point(85, 108)
            sfDataGrid1.Size = New System.Drawing.Size(240, 150)
            
            ' Add to form's controls collection
            Me.Controls.Add(sfDataGrid1)
        End Sub
    End Class
End Namespace
```

### Docking Options

Common docking patterns for the DataGrid:

```csharp
// Fill entire form
sfDataGrid1.Dock = DockStyle.Fill;

// Dock to top (e.g., with toolbar below)
sfDataGrid1.Dock = DockStyle.Top;
sfDataGrid1.Height = 300;

// Dock to bottom
sfDataGrid1.Dock = DockStyle.Bottom;

// Use anchoring for flexible positioning
sfDataGrid1.Anchor = AnchorStyles.Top | AnchorStyles.Bottom | 
                     AnchorStyles.Left | AnchorStyles.Right;
```

## Creating Data Models

### Simple Data Model

Create a data class to represent your business entity:

```csharp
public class OrderInfo
{
    public int OrderID { get; set; }
    public string CustomerID { get; set; }
    public string CustomerName { get; set; }
    public string Country { get; set; }
    public string ShipCity { get; set; }
    
    public OrderInfo(int orderId, string customerName, string country, 
                     string customerId, string shipCity)
    {
        this.OrderID = orderId;
        this.CustomerName = customerName;
        this.Country = country;
        this.CustomerID = customerId;
        this.ShipCity = shipCity;
    }
}
```

**VB.NET:**

```vb
Public Class OrderInfo
    Public Property OrderID() As Integer
    Public Property CustomerID() As String
    Public Property CustomerName() As String
    Public Property Country() As String
    Public Property ShipCity() As String
    
    Public Sub New(orderId As Integer, customerName As String, 
                   country As String, customerId As String, shipCity As String)
        Me.OrderID = orderId
        Me.CustomerName = customerName
        Me.Country = country
        Me.CustomerID = customerId
        Me.ShipCity = shipCity
    End Sub
End Class
```

### Data Collection Class

Create a collection class to hold your data:

```csharp
using System.Collections.ObjectModel;

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

**Why ObservableCollection?**
- Automatically notifies the grid when items are added, removed, or changed
- Keeps the UI synchronized with data changes
- Recommended for real-time data updates

## Binding Data

### Basic Data Binding

Bind the DataGrid to your data source using the `DataSource` property:

```csharp
private void InitializeDataGrid()
{
    sfDataGrid1 = new SfDataGrid();
    sfDataGrid1.Dock = DockStyle.Fill;
    
    // Create data collection
    OrderInfoCollection orderInfoCollection = new OrderInfoCollection();
    
    // Bind to DataSource
    sfDataGrid1.DataSource = orderInfoCollection.Orders;
    
    this.Controls.Add(sfDataGrid1);
}
```

**VB.NET:**

```vb
Private Sub InitializeDataGrid()
    sfDataGrid1 = New SfDataGrid()
    sfDataGrid1.Dock = DockStyle.Fill
    
    ' Create data collection
    Dim orderInfoCollection As New OrderInfoCollection()
    
    ' Bind to DataSource
    sfDataGrid1.DataSource = orderInfoCollection.Orders
    
    Me.Controls.Add(sfDataGrid1)
End Sub
```

### Supported Data Source Types

The SfDataGrid supports various data source types:

```csharp
// ObservableCollection (recommended for automatic updates)
ObservableCollection<OrderInfo> orders = new ObservableCollection<OrderInfo>();
sfDataGrid1.DataSource = orders;

// List
List<OrderInfo> orderList = new List<OrderInfo>();
sfDataGrid1.DataSource = orderList;

// BindingList
BindingList<OrderInfo> bindingList = new BindingList<OrderInfo>();
sfDataGrid1.DataSource = bindingList;

// DataTable
DataTable dataTable = new DataTable();
sfDataGrid1.DataSource = dataTable;

// IEnumerable
IEnumerable<OrderInfo> enumerable = GetOrders();
sfDataGrid1.DataSource = enumerable;
```

### Data Binding Result

After binding, the grid will display:
- One column for each public property in your data class
- One row for each item in your collection
- Column headers derived from property names

## Defining Columns

### Auto-Generated Columns

By default, the SfDataGrid automatically generates columns based on your data source properties. The column type is determined by the property type:

| Property Type | Generated Column Type | Description |
|--------------|----------------------|-------------|
| `string`, `object` | `GridTextColumn` | Text display and editing |
| `int`, `double`, `float`, `decimal` | `GridNumericColumn` | Numeric display with formatting |
| `DateTime` | `GridDateTimeColumn` | Date/time picker |
| `bool` | `GridCheckBoxColumn` | Checkbox control |
| `byte[]` | `GridImageColumn` | Image display |
| `Uri` | `GridHyperLinkColumn` | Clickable hyperlink |

**Enable auto-generation:**

```csharp
sfDataGrid1.AutoGenerateColumns = true; // This is the default
```

### Customizing Auto-Generated Columns

Handle the `AutoGeneratingColumn` event to customize columns before they're added:

```csharp
sfDataGrid1.AutoGeneratingColumn += SfDataGrid1_AutoGeneratingColumn;

private void SfDataGrid1_AutoGeneratingColumn(object sender, AutoGeneratingColumnEventArgs e)
{
    // Customize header text
    if (e.Column.MappingName == "OrderID")
    {
        e.Column.HeaderText = "Order ID";
        e.Column.AllowEditing = false; // Make read-only
    }
    
    // Change column width
    if (e.Column.MappingName == "CustomerName")
    {
        e.Column.Width = 200;
    }
    
    // Cancel column generation (hide column)
    if (e.Column.MappingName == "InternalID")
    {
        e.Cancel = true;
    }
    
    // Change column type
    if (e.Column.MappingName == "Status")
    {
        e.Column = new GridComboBoxColumn()
        {
            MappingName = "Status",
            HeaderText = "Status",
            DataSource = new List<string> { "Pending", "Shipped", "Delivered" }
        };
    }
}
```

### Manual Column Definition

Disable auto-generation and define columns manually for full control:

```csharp
sfDataGrid1.AutoGenerateColumns = false;

// Add GridTextColumn
sfDataGrid1.Columns.Add(new GridTextColumn() 
{ 
    MappingName = "OrderID", 
    HeaderText = "Order ID",
    Width = 100,
    AllowEditing = false
});

// Add GridTextColumn with custom properties
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
```

**VB.NET:**

```vb
sfDataGrid1.AutoGenerateColumns = False

sfDataGrid1.Columns.Add(New GridTextColumn() With {
    .MappingName = "OrderID",
    .HeaderText = "Order ID"
})

sfDataGrid1.Columns.Add(New GridTextColumn() With {
    .MappingName = "CustomerID",
    .HeaderText = "Customer ID"
})
```

### Available Column Types

```csharp
// Text column
sfDataGrid1.Columns.Add(new GridTextColumn() 
{ 
    MappingName = "CustomerName" 
});

// Numeric column
sfDataGrid1.Columns.Add(new GridNumericColumn() 
{ 
    MappingName = "Quantity",
    NumberFormatInfo = new NumberFormatInfo { NumberDecimalDigits = 2 }
});

// DateTime column
sfDataGrid1.Columns.Add(new GridDateTimeColumn() 
{ 
    MappingName = "OrderDate",
    Format = DateTimeFormat.ShortDate
});

// CheckBox column
sfDataGrid1.Columns.Add(new GridCheckBoxColumn() 
{ 
    MappingName = "IsActive" 
});

// ComboBox column
sfDataGrid1.Columns.Add(new GridComboBoxColumn() 
{ 
    MappingName = "Status",
    DataSource = new List<string> { "Pending", "Shipped", "Delivered" }
});

// Button column
sfDataGrid1.Columns.Add(new GridButtonColumn() 
{ 
    MappingName = "Action",
    HeaderText = "Actions"
});
```

## Handling Events

### Why TableControl Events?

You **cannot** handle Key and Mouse events directly on the SfDataGrid because the actual grid is hosted inside a `TableControl`. Always wire events to `sfDataGrid.TableControl`:

```csharp
// ❌ WRONG - This won't work
sfDataGrid1.KeyDown += OnKeyDown; // Won't fire!

// ✅ CORRECT - Use TableControl
sfDataGrid1.TableControl.KeyDown += OnKeyDown;
sfDataGrid1.TableControl.MouseDown += OnMouseDown;
```

### KeyDown Event

```csharp
private void InitializeDataGrid()
{
    sfDataGrid1 = new SfDataGrid();
    sfDataGrid1.Dock = DockStyle.Fill;
    
    // Wire TableControl KeyDown event
    sfDataGrid1.TableControl.KeyDown += OnKeyDown;
    
    this.Controls.Add(sfDataGrid1);
}

private void OnKeyDown(object sender, KeyEventArgs e)
{
    // Handle specific keys
    if (e.KeyCode == Keys.Delete)
    {
        // Custom delete logic
        Console.WriteLine("Delete key pressed");
    }
    else if (e.KeyCode == Keys.Enter)
    {
        // Custom Enter key logic
        Console.WriteLine("Enter key pressed");
    }
    else if (e.Control && e.KeyCode == Keys.C)
    {
        // Copy operation
        Console.WriteLine("Ctrl+C pressed");
    }
}
```

**VB.NET:**

```vb
AddHandler sfDataGrid1.TableControl.KeyDown, AddressOf OnKeyDown

Private Sub OnKeyDown(sender As Object, e As KeyEventArgs)
    If e.KeyCode = Keys.Delete Then
        Console.WriteLine("Delete key pressed")
    End If
End Sub
```

### MouseDown Event

```csharp
private void InitializeDataGrid()
{
    sfDataGrid1 = new SfDataGrid();
    sfDataGrid1.Dock = DockStyle.Fill;
    
    // Wire TableControl MouseDown event
    sfDataGrid1.TableControl.MouseDown += OnMouseDown;
    
    this.Controls.Add(sfDataGrid1);
}

private void OnMouseDown(object sender, MouseEventArgs e)
{
    if (e.Button == MouseButtons.Right)
    {
        // Show context menu
        Console.WriteLine($"Right click at: {e.Location}");
        
        // Get row and column at click position
        var rowColumnIndex = sfDataGrid1.TableControl.PointToCellRowColumnIndex(e.Location);
        if (rowColumnIndex.RowIndex > 0)
        {
            Console.WriteLine($"Clicked Row: {rowColumnIndex.RowIndex}, Column: {rowColumnIndex.ColumnIndex}");
        }
    }
}
```

### Common Grid Events

Beyond TableControl events, handle these SfDataGrid-specific events:

```csharp
// Selection changed
sfDataGrid1.SelectionChanged += (sender, e) =>
{
    if (sfDataGrid1.SelectedItem != null)
    {
        var selectedOrder = sfDataGrid1.SelectedItem as OrderInfo;
        Console.WriteLine($"Selected: {selectedOrder.CustomerName}");
    }
};

// Cell value changed
sfDataGrid1.CurrentCellValueChanged += (sender, e) =>
{
    Console.WriteLine($"Cell value changed at Row: {e.RowColumnIndex.RowIndex}");
};

// Current cell begin edit
sfDataGrid1.CurrentCellBeginEdit += (sender, e) =>
{
    Console.WriteLine($"Begin edit: {e.Column.MappingName}");
};

// Current cell end edit
sfDataGrid1.CurrentCellEndEdit += (sender, e) =>
{
    Console.WriteLine($"End edit: {e.Column.MappingName}");
};
```

## Basic Features Setup

### Enable Sorting

```csharp
// Enable sorting by clicking column headers
sfDataGrid1.AllowSorting = true;

// Programmatic sorting
sfDataGrid1.SortColumnDescriptions.Add(new SortColumnDescription() 
{ 
    ColumnName = "Country",
    SortDirection = ListSortDirection.Ascending
});
```

### Enable Grouping

```csharp
// Enable grouping with drag-drop area
sfDataGrid1.AllowGrouping = true;
sfDataGrid1.ShowGroupDropArea = true;

// Programmatic grouping
sfDataGrid1.GroupColumnDescriptions.Add(new GroupColumnDescription() 
{ 
    ColumnName = "Country" 
});
```

### Enable Filtering

```csharp
// Enable Excel-like filter UI
sfDataGrid1.AllowFiltering = true;

// Programmatic filtering
sfDataGrid1.Columns["CustomerID"].FilterPredicates.Add(new FilterPredicate() 
{ 
    FilterType = FilterType.Equals, 
    FilterValue = "ALFKI" 
});
```

### Enable Editing

```csharp
// Enable in-grid editing
sfDataGrid1.AllowEditing = true;

// Enable row deletion with Delete key
sfDataGrid1.AllowDeleting = true;

// Add new row at bottom
sfDataGrid1.AddNewRowPosition = AddNewRowPosition.Bottom;
```

### Configure Selection

```csharp
// Multiple row selection
sfDataGrid1.SelectionMode = GridSelectionMode.Extended;
sfDataGrid1.SelectionUnit = GridSelectionUnit.Row;

// Get selected items
var selectedItems = sfDataGrid1.SelectedItems;
```

## Complete Getting Started Example

Here's a complete, runnable example that brings everything together:

```csharp
using System;
using System.Collections.ObjectModel;
using System.Windows.Forms;
using Syncfusion.WinForms.DataGrid;
using Syncfusion.WinForms.DataGrid.Enums;

namespace WinFormsDataGridDemo
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
            // Create SfDataGrid
            sfDataGrid1 = new SfDataGrid();
            sfDataGrid1.Dock = DockStyle.Fill;
            
            // Create and bind data
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
            
            // Handle events
            sfDataGrid1.TableControl.KeyDown += OnKeyDown;
            sfDataGrid1.SelectionChanged += OnSelectionChanged;
            
            // Add to form
            this.Controls.Add(sfDataGrid1);
        }
        
        private void OnKeyDown(object sender, KeyEventArgs e)
        {
            if (e.KeyCode == Keys.Delete)
            {
                MessageBox.Show("Delete key pressed");
            }
        }
        
        private void OnSelectionChanged(object sender, EventArgs e)
        {
            if (sfDataGrid1.SelectedItem != null)
            {
                var order = sfDataGrid1.SelectedItem as OrderInfo;
                this.Text = $"Selected: {order.CustomerName}";
            }
        }
    }
    
    // Data model
    public class OrderInfo
    {
        public int OrderID { get; set; }
        public string CustomerID { get; set; }
        public string CustomerName { get; set; }
        public string Country { get; set; }
        public string ShipCity { get; set; }
        
        public OrderInfo(int orderId, string customerName, string country, 
                         string customerId, string shipCity)
        {
            this.OrderID = orderId;
            this.CustomerName = customerName;
            this.Country = country;
            this.CustomerID = customerId;
            this.ShipCity = shipCity;
        }
    }
    
    // Data collection
    public class OrderInfoCollection
    {
        public ObservableCollection<OrderInfo> Orders { get; set; }
        
        public OrderInfoCollection()
        {
            Orders = new ObservableCollection<OrderInfo>();
            GenerateOrders();
        }
        
        private void GenerateOrders()
        {
            Orders.Add(new OrderInfo(1001, "Maria Anders", "Germany", "ALFKI", "Berlin"));
            Orders.Add(new OrderInfo(1002, "Ana Trujilo", "Mexico", "ANATR", "Mexico D.F."));
            Orders.Add(new OrderInfo(1003, "Antonio Moreno", "Mexico", "ANTON", "Mexico D.F."));
            Orders.Add(new OrderInfo(1004, "Thomas Hardy", "UK", "AROUT", "London"));
            Orders.Add(new OrderInfo(1005, "Christina Berglund", "Sweden", "BERGS", "Lula"));
            Orders.Add(new OrderInfo(1006, "Hanna Moos", "Germany", "BLAUS", "Mannheim"));
            Orders.Add(new OrderInfo(1007, "Frederique Citeaux", "France", "BLONP", "Strasbourg"));
            Orders.Add(new OrderInfo(1008, "Martin Sommer", "Spain", "BOLID", "Madrid"));
            Orders.Add(new OrderInfo(1009, "Laurence Lebihan", "France", "BONAP", "Marseille"));
            Orders.Add(new OrderInfo(1010, "Elizabeth Lincoln", "Canada", "BOTTM", "Tsawassen"));
        }
    }
}
```

## What You'll See

When you run the application:

1. **Grid displays** with 10 rows of customer order data
2. **Column headers** showing OrderID, CustomerID, CustomerName, Country, ShipCity
3. **Sorting** - Click any column header to sort
4. **Filtering** - Click filter icon in headers to filter
5. **Grouping** - Drag column headers to the group drop area
6. **Selection** - Click rows to select (Ctrl/Shift for multiple)
7. **Editing** - Double-click cells to edit values

## Next Steps

Now that you have a basic DataGrid running:
- Explore **columns.md** for advanced column types and configurations
- Read **editing.md** for validation and custom editors
- Check **filtering-sorting-grouping.md** for advanced data operations
- Review **selection.md** for complex selection scenarios
- Learn about **summaries.md** for data aggregations
- See **export.md** for Excel and PDF export
- Study **styling.md** for conditional formatting and custom rendering

## Troubleshooting

### Grid not displaying data
- Verify `DataSource` is set to a valid collection
- Check that properties are `public` in your data class
- Ensure `AutoGenerateColumns = true` or columns are manually defined

### Columns not showing
- Check `MappingName` matches property name exactly (case-sensitive)
- Verify property has a `get` accessor
- Ensure column wasn't cancelled in `AutoGeneratingColumn` event

### Events not firing
- For Key/Mouse events, use `TableControl` not the grid directly:
  ```csharp
  sfDataGrid1.TableControl.KeyDown += OnKeyDown;
  ```
- For grid-specific events, wire them to the grid itself

### Performance issues with large datasets
- Use `ObservableCollection` or `BindingList` for better performance
- Consider virtual mode for very large datasets (100,000+ rows)
- Disable auto-column generation and define columns manually
