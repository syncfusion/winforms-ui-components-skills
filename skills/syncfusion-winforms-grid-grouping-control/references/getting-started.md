# Getting Started with GridGroupingControl

This guide covers the initial setup and basic configuration of Syncfusion GridGroupingControl in Windows Forms applications.

### NuGet Package Installation

Install via NuGet Package Manager:

```powershell
Install-Package Syncfusion.Grid.Grouping.Windows
```

Or via .NET CLI:

```bash
dotnet add package Syncfusion.Grid.Grouping.Windows
```

The NuGet package automatically includes all required dependencies.

## Creating GridGroupingControl

### Adding Through Designer

1. **Open your Windows Form** in the Visual Studio Designer
2. **Open the Toolbox** (View → Toolbox or Ctrl+Alt+X)
3. **Locate GridGroupingControl** in the Syncfusion Controls section
4. **Drag and drop** the control onto your form
5. The control is automatically added with required assemblies referenced

The Designer will add this code to your form's InitializeComponent method:

```csharp
private Syncfusion.Windows.Forms.Grid.Grouping.GridGroupingControl gridGroupingControl1;

this.gridGroupingControl1 = new Syncfusion.Windows.Forms.Grid.Grouping.GridGroupingControl();
this.gridGroupingControl1.Location = new System.Drawing.Point(12, 12);
this.gridGroupingControl1.Size = new System.Drawing.Size(760, 400);
this.Controls.Add(this.gridGroupingControl1);
```

### Adding Through Code

To add GridGroupingControl programmatically:

```csharp
using Syncfusion.Windows.Forms.Grid.Grouping;

public partial class Form1 : Form
{
    private GridGroupingControl gridGroupingControl1;

    public Form1()
    {
        InitializeComponent();
        
        // Initialize the control
        this.gridGroupingControl1 = new GridGroupingControl();
        
        // Set size and location
        this.gridGroupingControl1.Location = new Point(10, 10);
        this.gridGroupingControl1.Size = new Size(780, 500);
        
        // Or use Dock for responsive sizing
        this.gridGroupingControl1.Dock = DockStyle.Fill;
        
        // Add to form
        this.Controls.Add(this.gridGroupingControl1);
    }
}
```

## Populating Data

GridGroupingControl supports various data sources. The simplest approach is binding to a DataTable.

### Basic Data Binding Example

```csharp
using System.Data;
using Syncfusion.Windows.Forms.Grid.Grouping;

public Form1()
{
    InitializeComponent();
    
    // Create sample data
    DataTable dataTable = CreateSampleData();
    
    // Bind to GridGroupingControl
    this.gridGroupingControl1.DataSource = dataTable;
}

private DataTable CreateSampleData()
{
    DataTable dt = new DataTable("Employees");
    
    // Define columns
    dt.Columns.Add("EmployeeID", typeof(int));
    dt.Columns.Add("Name", typeof(string));
    dt.Columns.Add("Department", typeof(string));
    dt.Columns.Add("Designation", typeof(string));
    dt.Columns.Add("Salary", typeof(decimal));
    dt.Columns.Add("HireDate", typeof(DateTime));
    
    // Add sample rows
    dt.Rows.Add(1, "John Smith", "Sales", "Sales Manager", 75000, new DateTime(2020, 1, 15));
    dt.Rows.Add(2, "Sarah Johnson", "Marketing", "Marketing Director", 85000, new DateTime(2019, 3, 10));
    dt.Rows.Add(3, "Mike Wilson", "Sales", "Sales Executive", 55000, new DateTime(2021, 6, 20));
    dt.Rows.Add(4, "Lisa Brown", "IT", "Software Engineer", 70000, new DateTime(2020, 9, 5));
    dt.Rows.Add(5, "Tom Davis", "IT", "Senior Developer", 80000, new DateTime(2018, 11, 12));
    dt.Rows.Add(6, "Emily White", "Marketing", "Content Writer", 50000, new DateTime(2021, 2, 8));
    dt.Rows.Add(7, "David Lee", "Sales", "Sales Executive", 52000, new DateTime(2022, 1, 25));
    dt.Rows.Add(8, "Anna Martinez", "IT", "QA Engineer", 65000, new DateTime(2020, 7, 30));
    
    return dt;
}
```

### Binding to DataSet

When working with multiple related tables, use a DataSet:

```csharp
// Create DataSet
DataSet dataSet = new DataSet();

// Create and add parent table
DataTable customers = new DataTable("Customers");
customers.Columns.Add("CustomerID", typeof(int));
customers.Columns.Add("CustomerName", typeof(string));
customers.Columns.Add("Country", typeof(string));
dataSet.Tables.Add(customers);

// Create and add child table
DataTable orders = new DataTable("Orders");
orders.Columns.Add("OrderID", typeof(int));
orders.Columns.Add("CustomerID", typeof(int));
orders.Columns.Add("OrderDate", typeof(DateTime));
orders.Columns.Add("Amount", typeof(decimal));
dataSet.Tables.Add(orders);

// Define relationship
DataRelation relation = new DataRelation("CustomerOrders",
    customers.Columns["CustomerID"],
    orders.Columns["CustomerID"]);
dataSet.Relations.Add(relation);

// Bind to grid
this.gridGroupingControl1.DataSource = dataSet;
this.gridGroupingControl1.DataMember = "Customers";
```

## Basic Configuration

### Enabling Grouping

Enable the GroupDropArea for drag-and-drop grouping:

```csharp
// Show the group drop area at the top
this.gridGroupingControl1.ShowGroupDropArea = true;

// Users can now drag column headers to group
```

### Programmatic Grouping

Group data by one or more columns programmatically:

```csharp
// Group by Department
this.gridGroupingControl1.TableDescriptor.GroupedColumns.Add("Department");

// Multi-level grouping
this.gridGroupingControl1.TableDescriptor.GroupedColumns.Add("Department");
this.gridGroupingControl1.TableDescriptor.GroupedColumns.Add("Designation");
```

### Enabling Editing

By default, GridGroupingControl allows editing. To control edit behavior:

```csharp
// Enable editing (default)
this.gridGroupingControl1.TableDescriptor.AllowEdit = true;

// Disable editing
this.gridGroupingControl1.TableDescriptor.AllowEdit = false;

// Control cell activation
this.gridGroupingControl1.ActivateCurrentCellBehavior = GridCellActivateAction.DblClickOnCell;
```

### Enabling Sorting

Enable column sorting by clicking headers:

```csharp
// Enable sorting (default is true)
this.gridGroupingControl1.TableOptions.AllowSortColumns = true;

// Disable sorting
this.gridGroupingControl1.TableOptions.AllowSortColumns = false;

// Programmatic sorting
this.gridGroupingControl1.TableDescriptor.SortedColumns.Add("Salary");
```

### Enabling Filtering

Enable the filter bar for column filtering:

```csharp
// Show filter bar row
this.gridGroupingControl1.TopLevelGroupOptions.ShowFilterBar = true;

// Enable filtering for specific columns
this.gridGroupingControl1.TableDescriptor.Columns["Department"].AllowFilter = true;
this.gridGroupingControl1.TableDescriptor.Columns["Designation"].AllowFilter = true;

// Or enable for all columns
foreach (GridColumnDescriptor column in this.gridGroupingControl1.TableDescriptor.Columns)
{
    column.AllowFilter = true;
}
```

## First Complete Example

Here's a complete working example that demonstrates basic setup with grouping and styling:

```csharp
using System;
using System.Data;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Grid.Grouping;

namespace GridGroupingDemo
{
    public partial class Form1 : Form
    {
        private GridGroupingControl gridGroupingControl1;

        public Form1()
        {
            InitializeComponent();
            SetupGrid();
        }

        private void SetupGrid()
        {
            // Create and configure the grid
            this.gridGroupingControl1 = new GridGroupingControl();
            this.gridGroupingControl1.Dock = DockStyle.Fill;
            this.Controls.Add(this.gridGroupingControl1);

            // Create and bind data
            DataTable data = CreateSampleData();
            this.gridGroupingControl1.DataSource = data;

            // Enable grouping features
            this.gridGroupingControl1.ShowGroupDropArea = true;
            this.gridGroupingControl1.TableDescriptor.GroupedColumns.Add("Department");

            // Configure table options
            this.gridGroupingControl1.TableOptions.AllowSortColumns = true;
            this.gridGroupingControl1.TableOptions.ListBoxSelectionMode = SelectionMode.MultiExtended;

            // Show filter bar
            this.gridGroupingControl1.TopLevelGroupOptions.ShowFilterBar = true;
            foreach (GridColumnDescriptor column in this.gridGroupingControl1.TableDescriptor.Columns)
            {
                column.AllowFilter = true;
            }

            // Configure group options
            this.gridGroupingControl1.TopLevelGroupOptions.ShowCaption = true;
            this.gridGroupingControl1.TopLevelGroupOptions.ShowGroupHeader = true;
            
            // Customize appearance
            this.gridGroupingControl1.Appearance.GroupCaptionCell.BackColor = Color.LightBlue;
            this.gridGroupingControl1.Appearance.GroupCaptionCell.TextColor = Color.DarkBlue;
            this.gridGroupingControl1.TableOptions.GridLineBorder = new GridBorder(GridBorderStyle.Solid, Color.LightGray);

            // Format specific columns
            this.gridGroupingControl1.TableDescriptor.Columns["Salary"].Appearance.AnyRecordFieldCell.Format = "C";
            this.gridGroupingControl1.TableDescriptor.Columns["HireDate"].Appearance.AnyRecordFieldCell.Format = "d";
        }

        private DataTable CreateSampleData()
        {
            DataTable dt = new DataTable("Employees");

            dt.Columns.Add("EmployeeID", typeof(int));
            dt.Columns.Add("Name", typeof(string));
            dt.Columns.Add("Department", typeof(string));
            dt.Columns.Add("Designation", typeof(string));
            dt.Columns.Add("Salary", typeof(decimal));
            dt.Columns.Add("HireDate", typeof(DateTime));

            dt.Rows.Add(1, "John Smith", "Sales", "Sales Manager", 75000, new DateTime(2020, 1, 15));
            dt.Rows.Add(2, "Sarah Johnson", "Marketing", "Marketing Director", 85000, new DateTime(2019, 3, 10));
            dt.Rows.Add(3, "Mike Wilson", "Sales", "Sales Executive", 55000, new DateTime(2021, 6, 20));
            dt.Rows.Add(4, "Lisa Brown", "IT", "Software Engineer", 70000, new DateTime(2020, 9, 5));
            dt.Rows.Add(5, "Tom Davis", "IT", "Senior Developer", 80000, new DateTime(2018, 11, 12));
            dt.Rows.Add(6, "Emily White", "Marketing", "Content Writer", 50000, new DateTime(2021, 2, 8));
            dt.Rows.Add(7, "David Lee", "Sales", "Sales Executive", 52000, new DateTime(2022, 1, 25));
            dt.Rows.Add(8, "Anna Martinez", "IT", "QA Engineer", 65000, new DateTime(2020, 7, 30));

            return dt;
        }
    }
}
```

## Essential Properties

### Core Properties

| Property | Type | Description |
|----------|------|-------------|
| `DataSource` | object | The data source for the grid |
| `DataMember` | string | Specific table in multi-table data source |
| `ShowGroupDropArea` | bool | Shows area for drag-drop grouping |
| `Dock` | DockStyle | Docking behavior within container |
| `Size` | Size | Control dimensions |

### Table Options

Access via `gridGroupingControl1.TableOptions`:

| Property | Type | Description |
|----------|------|-------------|
| `AllowEdit` | bool | Enables/disables cell editing |
| `AllowSortColumns` | bool | Enables/disables column sorting |
| `ListBoxSelectionMode` | SelectionMode | Row selection mode |
| `GridLineColor` | Color | Color of grid lines |

### Group Options

Access via `gridGroupingControl1.TopLevelGroupOptions`:

| Property | Type | Description |
|----------|------|-------------|
| `ShowFilterBar` | bool | Shows filter bar row |
| `ShowCaption` | bool | Shows group caption rows |
| `ShowGroupHeader` | bool | Shows group header sections |
| `ShowGroupFooter` | bool | Shows group footer sections |

## Common Initial Configurations

### Read-Only Grid

```csharp
this.gridGroupingControl1.TableOptions.AllowEdit = false;
this.gridGroupingControl1.TableOptions.AllowNew = false;
this.gridGroupingControl1.TableOptions.AllowRemove = false;
```

### Multi-Select Grid

```csharp
this.gridGroupingControl1.TableOptions.ListBoxSelectionMode = SelectionMode.MultiExtended;
this.gridGroupingControl1.TableOptions.AllowSelection = GridSelectionFlags.Any;
```

### Compact Display

```csharp
this.gridGroupingControl1.TableOptions.RowHeaderWidth = 0;
this.gridGroupingControl1.TableOptions.RecordRowHeight = 20;
this.gridGroupingControl1.TableOptions.ColumnHeaderRowHeight = 22;
```

## Next Steps

Now that you have a basic GridGroupingControl running:

1. **Explore Data Binding** - See [data-binding.md](data-binding.md) for advanced data source options
2. **Configure Grouping** - See [grouping.md](grouping.md) for grouping features
3. **Add Filtering** - See [filtering.md](filtering.md) for Excel-like filtering
4. **Implement Summaries** - See [summaries.md](summaries.md) for aggregate calculations
5. **Customize Appearance** - See [appearance-and-formatting.md](appearance-and-formatting.md) for styling

## Troubleshooting

### Control Not Appearing

**Issue:** GridGroupingControl not visible on form  
**Solution:** Check Dock property or ensure Size is set properly

```csharp
this.gridGroupingControl1.Dock = DockStyle.Fill;
// OR
this.gridGroupingControl1.Size = new Size(800, 600);
```

### No Data Displayed

**Issue:** Grid is empty after setting DataSource  
**Solution:** Ensure DataSource has at least one record

```csharp
// Verify data exists
if (dataTable.Rows.Count > 0)
{
    this.gridGroupingControl1.DataSource = dataTable;
}
```

### Assembly References Missing

**Issue:** Build errors about missing Syncfusion types  
**Solution:** Ensure all required assemblies are referenced. Use NuGet for automatic dependency resolution.

### License Key Required

**Issue:** License dialog appears at runtime  
**Solution:** Register your Syncfusion license key in application startup

```csharp
// In Program.cs or App.xaml.cs
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
```