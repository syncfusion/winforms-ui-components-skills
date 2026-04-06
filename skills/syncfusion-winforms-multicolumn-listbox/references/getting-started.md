# Getting Started with GridListControl

This guide covers the initial setup and basic implementation of GridListControl in Windows Forms applications.

## Assembly Deployment

Before using GridListControl, you need to add the required assembly references to your project.

### Required Assemblies

The following assemblies are required:

| Assembly | Description |
|----------|-------------|
| **Syncfusion.Grid.Windows** | Contains classes that handle all UI operations, fundamentals, and base classes of the GridListControl |
| **Syncfusion.Shared.Base** | Contains style-related properties and various editor controls used in the GridListControl |

### Adding References

**Via Visual Studio:**
1. Right-click your project in Solution Explorer
2. Select "Add Reference"
3. Browse to the Syncfusion installation directory
4. Add `Syncfusion.Grid.Windows.dll` and `Syncfusion.Shared.Base.dll`

**Via NuGet:**
```
Install-Package Syncfusion.Grid.Windows
```
This package includes the required dependencies.

## Creating GridListControl Through Designer

The designer approach is best for static layouts and visual property configuration.

### Step 1: Create a Data Source

If you don't have a data source available:

1. **Drag SqlDataAdapter from Toolbox**
   - Open the Data tab in the Visual Studio toolbox
   - Drag an `SqlDataAdapter` onto your form
   - This opens the Data Adapter Configuration Wizard

2. **Follow the Wizard**
   - Select your database connection
   - Choose or write the SQL query to generate the table
   - The wizard will configure the adapter for you

3. **Generate a DataSet**
   - Right-click the SqlAdapter in the component tray
   - Select "Generate Dataset"
   - Accept the default settings

4. **Automatic Fill Method**
   - The wizard automatically generates code in the Form_Load event
   - This code calls the Fill method on the SqlDataAdapter
   - The Fill method populates the DataSet with data

### Step 2: Add GridListControl to Form

1. **Add Control**
   - Locate `GridListControl` in the toolbox
   - Drag it onto your form
   - Size and position it as needed

2. **Configure DataSource**
   - Select the GridListControl on the form
   - Open the Properties window (F4)
   - Find the `DataSource` property
   - Set it to your DataSet or DataTable created in Step 1

3. **Run the Application**
   - Press F5 to run
   - The GridListControl will display data from your database

### Designer Benefits

- Visual property configuration
- Immediate preview of changes
- No coding required for basic scenarios
- Ideal for rapid prototyping

## Creating GridListControl Through Code

The code approach is best for dynamic controls, runtime customization, and complex logic.

### Basic Code Setup

```csharp
using Syncfusion.Windows.Forms.Grid;
using Syncfusion.Windows.Forms;
using System.Collections;

public partial class Form1 : Form
{
    public Form1()
    {
        InitializeComponent();
        SetupGridList();
    }

    private void SetupGridList()
    {
        // Create ArrayList of custom objects
        ArrayList USStates = new ArrayList();
        USStates.Add(new StateInfo { LongName = "California", ShortName = "CA", ImageIndex = 0 });
        USStates.Add(new StateInfo { LongName = "Texas", ShortName = "TX", ImageIndex = 1 });
        USStates.Add(new StateInfo { LongName = "New York", ShortName = "NY", ImageIndex = 2 });
        
        // Configure GridListControl
        gridListControl1.DataSource = USStates;
        gridListControl1.ImageList = imageList1;  // If using images
        gridListControl1.MultiColumn = true;
        gridListControl1.ShowColumnHeader = true;
        gridListControl1.SelectionMode = SelectionMode.One;
        gridListControl1.FillLastColumn = true;
    }
}

// Custom data class
public class StateInfo
{
    public string LongName { get; set; }
    public string ShortName { get; set; }
    public int ImageIndex { get; set; }
}
```

### VB.NET Example

```vb
Imports Syncfusion.Windows.Forms.Grid
Imports Syncfusion.Windows.Forms

Public Class Form1
    Private Sub SetupGridList()
        ' Sets to array list of states
        gridListBox1.DataSource = USStates
        
        ' ImageList - the images displayed in the list
        gridListBox1.ImageList = ImageList
        
        ' Displays multiple columns
        gridListBox1.MultiColumn = True
        gridListBox1.ShowColumnHeader = True
        gridListBox1.SelectionMode = SelectionMode.One
        
        ' Makes last column wide enough to fill client area
        gridListBox1.FillLastColumn = True
    End Sub
End Class
```

## Basic Configuration Properties

### DataSource Property

The `DataSource` property binds the GridListControl to a data source.

**Supported Data Sources:**
- ArrayList
- List<T>
- DataTable
- DataSet
- Any collection implementing IList

**Example:**
```csharp
// Using ArrayList
ArrayList data = new ArrayList();
data.Add(new MyObject());
gridListControl1.DataSource = data;

// Using List<T>
List<Customer> customers = GetCustomers();
gridListControl1.DataSource = customers;

// Using DataTable
DataTable dt = GetDataTable();
gridListControl1.DataSource = dt;
```

### MultiColumn Property

Enables multi-column display mode.

```csharp
// Enable multi-column display
gridListControl1.MultiColumn = true;
```

**When false:** Displays as a single-column list (like standard ListBox)  
**When true:** Displays all properties/columns of the data source

### ShowColumnHeader Property

Controls the visibility of column headers.

```csharp
// Show column headers
gridListControl1.ShowColumnHeader = true;
```

**When true:** Headers are visible with column names  
**When false:** No headers displayed (saves vertical space)

### FillLastColumn Property

Makes the last column expand to fill remaining horizontal space.

```csharp
// Auto-fill last column
gridListControl1.FillLastColumn = true;
```

**Benefits:**
- Eliminates empty space on the right
- Automatically adjusts when control is resized
- Creates a polished, professional appearance

### SelectionMode Property

Controls how users can select rows. See [data-binding-selection.md](data-binding-selection.md) for details.

```csharp
// Single selection
gridListControl1.SelectionMode = SelectionMode.One;
```

## ImageList Support

GridListControl can display images in rows using an ImageList.

```csharp
// Create and populate ImageList
ImageList imageList1 = new ImageList();
imageList1.Images.Add(Image.FromFile("icon1.png"));
imageList1.Images.Add(Image.FromFile("icon2.png"));

// Assign to GridListControl
gridListControl1.ImageList = imageList1;

// Data class with ImageIndex property
public class StateInfo
{
    public string LongName { get; set; }
    public string ShortName { get; set; }
    public int ImageIndex { get; set; }  // Index into ImageList
}
```

## Initial Setup Checklist

✅ **Assembly References Added**
- Syncfusion.Grid.Windows.dll
- Syncfusion.Shared.Base.dll

✅ **Data Source Prepared**
- ArrayList, List<T>, DataTable, or other IList

✅ **GridListControl Configured**
- DataSource property set
- MultiColumn enabled (if needed)
- ShowColumnHeader enabled (if headers desired)
- SelectionMode set appropriately

✅ **Optional Features**
- ImageList assigned (if using images)
- FillLastColumn enabled (for better appearance)

## Quick Start Comparison

### Designer Approach
**Pros:**
- Visual configuration
- No coding for basic scenarios
- Rapid prototyping

**Cons:**
- Less flexible for dynamic scenarios
- Generated code can be hard to maintain
- Limited runtime customization

### Code Approach
**Pros:**
- Full control over initialization
- Easy to modify at runtime
- Better for complex logic
- More maintainable

**Cons:**
- Requires more initial setup
- No visual preview during design

## Next Steps

- **Data Binding and Selection:** Learn about selection modes and advanced data binding → [data-binding-selection.md](data-binding-selection.md)
- **Customization:** Style the control with colors, grid lines, and backgrounds → [customization.md](customization.md)
- **ComboBox Integration:** Use GridListControl in dropdowns → [combobox-integration.md](combobox-integration.md)

## Common Issues During Setup

**GridListControl not in toolbox**
- Verify Syncfusion assemblies are installed
- Check that assemblies match your .NET Framework version
- Restart Visual Studio after installation

**Data not appearing**
- Ensure DataSource is set to a non-null object
- Verify data source contains items
- Check if MultiColumn needs to be enabled

**Columns not showing**
- Set MultiColumn = true
- Verify data objects have public properties
- Check if ShowColumnHeader is true

**Designer errors**
- Ensure correct assembly versions
- Check .NET Framework target version
- Rebuild solution and restart Visual Studio
