# Getting Started with MultiColumnComboBox

This guide covers the installation, setup, and basic usage of the `MultiColumnComboBox` control in Windows Forms applications.

## When to Read This

Read this reference when:
- Setting up MultiColumnComboBox for the first time
- Adding the control to a form using the designer or code
- Understanding the basic structure and requirements
- Learning the required namespaces and assemblies
- Creating your first multi-column dropdown implementation

## Assembly Requirements

The MultiColumnComboBox control requires the following assemblies:

**Required Assembly:**
- `Syncfusion.Tools.Windows.dll` - Contains the MultiColumnComboBox control

**Dependent Assembly:**
- `Syncfusion.Shared.Base.dll` - Base functionality

**Namespace:**
```csharp
using Syncfusion.Windows.Forms.Tools;
```

```vbnet
Imports Syncfusion.Windows.Forms.Tools
```

## Installation Methods

### NuGet Installation

Install the MultiColumnComboBox control via NuGet Package Manager:

**Package Manager Console:**
```powershell
Install-Package Syncfusion.Tools.Windows
```

**NuGet Package Manager UI:**
1. Right-click your project → Manage NuGet Packages
2. Search for "Syncfusion.Tools.Windows"
3. Select the package and click Install

### Manual Assembly Reference

If not using NuGet, add assembly references manually:

1. Right-click project → Add Reference → Browse
2. Navigate to Syncfusion installation folder:
   - `C:/Program Files (x86)/Syncfusion/Essential Studio/Windows/{version}/precompiledassemblies/{.NET version}/`
3. Select `Syncfusion.Tools.Windows.dll` and `Syncfusion.Shared.Base.dll`
4. Click OK

## Designer-Based Setup

The MultiColumnComboBox provides full Windows Forms designer support.

### Adding via Toolbox

**Steps:**
1. Open your form in designer view
2. Locate the Syncfusion toolbox section
3. Find "MultiColumnComboBox" control
4. Drag and drop onto your form
5. Configure properties via Property Grid

**Visual Result:**
The control appears on the form with default styling and an empty dropdown.

### Designer Properties

Common properties to set in the designer:

| Property | Purpose | Default |
|----------|---------|---------|
| `MultiColumn` | Enable/disable multi-column display | `true` |
| `ShowColumnHeader` | Display column headers | `false` |
| `DataSource` | Bind data source | `null` |
| `DisplayMember` | Column to show in text area | `""` |
| `ValueMember` | Column to use as selected value | `""` |
| `DropDownWidth` | Width of dropdown popup | Auto |
| `Style` | Visual theme | `Default` |

## Programmatic Creation

### Basic Implementation

Create and add a MultiColumnComboBox in code:

**C#:**
```csharp
using Syncfusion.Windows.Forms.Tools;

public partial class Form1 : Form
{
    private Syncfusion.Windows.Forms.Tools.MultiColumnComboBox multiColumnComboBox1;
    
    public Form1()
    {
        InitializeComponent();
        CreateMultiColumnComboBox();
    }
    
    private void CreateMultiColumnComboBox()
    {
        // Create instance
        this.multiColumnComboBox1 = new Syncfusion.Windows.Forms.Tools.MultiColumnComboBox();
        
        // Set location and size
        this.multiColumnComboBox1.Location = new System.Drawing.Point(20, 20);
        this.multiColumnComboBox1.Size = new System.Drawing.Size(250, 30);
        
        // Add to form
        this.Controls.Add(this.multiColumnComboBox1);
    }
}
```

**VB.NET:**
```vbnet
Imports Syncfusion.Windows.Forms.Tools

Public Partial Class Form1
    Inherits Form
    
    Private multiColumnComboBox1 As Syncfusion.Windows.Forms.Tools.MultiColumnComboBox
    
    Public Sub New()
        InitializeComponent()
        CreateMultiColumnComboBox()
    End Sub
    
    Private Sub CreateMultiColumnComboBox()
        ' Create instance
        Me.multiColumnComboBox1 = New Syncfusion.Windows.Forms.Tools.MultiColumnComboBox()
        
        ' Set location and size
        Me.multiColumnComboBox1.Location = New System.Drawing.Point(20, 20)
        Me.multiColumnComboBox1.Size = New System.Drawing.Size(250, 30)
        
        ' Add to form
        Me.Controls.Add(Me.multiColumnComboBox1)
    End Sub
End Class
```

### Complete Example with Data Binding

Here's a complete example that creates a control and binds it to a DataTable:

**C#:**
```csharp
using System;
using System.Data;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class EmployeeSelector : Form
{
    private MultiColumnComboBox employeeCombo;
    
    public EmployeeSelector()
    {
        InitializeComponent();
        SetupEmployeeComboBox();
    }
    
    private void SetupEmployeeComboBox()
    {
        // Create control
        employeeCombo = new MultiColumnComboBox
        {
            Location = new System.Drawing.Point(20, 20),
            Size = new System.Drawing.Size(300, 30),
            MultiColumn = true,
            ShowColumnHeader = true
        };
        
        // Create sample data
        DataTable employees = CreateEmployeeData();
        
        // Bind data
        employeeCombo.DataSource = employees;
        employeeCombo.DisplayMember = "Name";
        employeeCombo.ValueMember = "EmployeeID";
        
        // Add to form
        this.Controls.Add(employeeCombo);
    }
    
    private DataTable CreateEmployeeData()
    {
        DataTable dt = new DataTable("Employees");
        dt.Columns.Add("EmployeeID", typeof(int));
        dt.Columns.Add("Name", typeof(string));
        dt.Columns.Add("Department", typeof(string));
        dt.Columns.Add("Location", typeof(string));
        
        dt.Rows.Add(1001, "John Smith", "Engineering", "New York");
        dt.Rows.Add(1002, "Sarah Johnson", "Marketing", "Chicago");
        dt.Rows.Add(1003, "Michael Brown", "Sales", "Los Angeles");
        dt.Rows.Add(1004, "Emily Davis", "HR", "Boston");
        
        return dt;
    }
}
```

**VB.NET:**
```vbnet
Imports System
Imports System.Data
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Public Partial Class EmployeeSelector
    Inherits Form
    
    Private employeeCombo As MultiColumnComboBox
    
    Public Sub New()
        InitializeComponent()
        SetupEmployeeComboBox()
    End Sub
    
    Private Sub SetupEmployeeComboBox()
        ' Create control
        employeeCombo = New MultiColumnComboBox With {
            .Location = New System.Drawing.Point(20, 20),
            .Size = New System.Drawing.Size(300, 30),
            .MultiColumn = True,
            .ShowColumnHeader = True
        }
        
        ' Create sample data
        Dim employees As DataTable = CreateEmployeeData()
        
        ' Bind data
        employeeCombo.DataSource = employees
        employeeCombo.DisplayMember = "Name"
        employeeCombo.ValueMember = "EmployeeID"
        
        ' Add to form
        Me.Controls.Add(employeeCombo)
    End Sub
    
    Private Function CreateEmployeeData() As DataTable
        Dim dt As New DataTable("Employees")
        dt.Columns.Add("EmployeeID", GetType(Integer))
        dt.Columns.Add("Name", GetType(String))
        dt.Columns.Add("Department", GetType(String))
        dt.Columns.Add("Location", GetType(String))
        
        dt.Rows.Add(1001, "John Smith", "Engineering", "New York")
        dt.Rows.Add(1002, "Sarah Johnson", "Marketing", "Chicago")
        dt.Rows.Add(1003, "Michael Brown", "Sales", "Los Angeles")
        dt.Rows.Add(1004, "Emily Davis", "HR", "Boston")
        
        Return dt
    End Function
End Class
```

## Next Steps

After setting up the basic control:

1. **Configure Data Binding** → Read: [data-binding.md](data-binding.md)
   - Learn different data source types (DataView, OleDbDataAdapter, XML)
   - Hide specific columns
   - Access selected row data

2. **Enable Multi-Column Features** → Read: [multiple-columns.md](multiple-columns.md)
   - Enable column headers
   - Configure dropdown width
   - Implement custom filtering

3. **Apply Visual Styles** → Read: [appearance-styling.md](appearance-styling.md)
   - Choose from 9 built-in themes
   - Apply custom colors
   - Customize selection colors

4. **Handle Selection Events** → Read: [event-handling.md](event-handling.md)
   - Respond to user selections
   - Access selected data
   - Validate selections

## Troubleshooting

### Control Not Visible in Toolbox

**Issue:** MultiColumnComboBox doesn't appear in Visual Studio toolbox.

**Solutions:**
1. Verify Syncfusion.Tools.Windows is installed via NuGet
2. Check if assemblies are compatible with your .NET version
3. Right-click toolbox → Choose Items → Browse to assembly location
4. Restart Visual Studio after installation

### Designer Shows Generic Icon

**Issue:** Control appears as generic box in designer.

**Solution:** This is normal before data binding. The control will render properly at runtime once DataSource is set.

### Namespace Not Found

**Issue:** `The type or namespace name 'Tools' does not exist in the namespace 'Syncfusion.Windows.Forms'`

**Solution:**
1. Add reference to `Syncfusion.Tools.Windows.dll`
2. Verify `using Syncfusion.Windows.Forms.Tools;` is present
3. Check that assembly version matches your Syncfusion license

### Columns Not Displaying

**Issue:** Dropdown shows single column instead of multiple columns.

**Solution:**
1. Verify `MultiColumn = true` is set
2. Ensure DataSource has multiple columns
3. Check that data is successfully bound (not null)
