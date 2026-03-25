# Multiple Columns Configuration

This guide covers all aspects of multi-column functionality in the MultiColumnComboBox, including column headers, dropdown sizing, filtering, and selection highlighting.

## Table of Contents
- [Overview](#overview)
- [Enabling Multiple Columns](#enabling-multiple-columns)
- [Column Headers](#column-headers)
- [Selection Highlighting](#selection-highlighting)
- [Dropdown Width Configuration](#dropdown-width-configuration)
- [Custom Filtering](#custom-filtering)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

The MultiColumnComboBox displays all DataSource fields as columns in a grid-based dropdown. This provides a spreadsheet-like selection experience with multiple data points visible simultaneously.

**Key Features:**
- Multiple columns enabled by default
- Automatic column generation from DataSource
- Column headers (optional)
- Custom dropdown width
- Advanced filtering capabilities
- Selection color customization

## Enabling Multiple Columns

### MultiColumn Property

The `MultiColumn` property controls whether multiple columns are displayed.

**C#:**
```csharp
// Enable multiple columns (default)
multiColumnComboBox1.MultiColumn = true;

// Disable to show single column (like standard ComboBox)
multiColumnComboBox1.MultiColumn = false;
```

**VB.NET:**
```vbnet
' Enable multiple columns (default)
multiColumnComboBox1.MultiColumn = True

' Disable to show single column (like standard ComboBox)
multiColumnComboBox1.MultiColumn = False
```

**When to Disable:**
- When only one column matters
- For simple dropdown lists
- To save screen space
- When emulating standard ComboBox behavior

## Column Headers

### ShowColumnHeader Property

Display column names at the top of the dropdown grid.

**C#:**
```csharp
// Enable column headers
multiColumnComboBox1.ShowColumnHeader = true;
```

**VB.NET:**
```vbnet
' Enable column headers
multiColumnComboBox1.ShowColumnHeader = True
```

### Complete Example with Headers

**C#:**
```csharp
using System;
using System.Data;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class ProductSelector : Form
{
    private void SetupProductCombo()
    {
        // Create product data
        DataTable products = new DataTable("Products");
        products.Columns.Add("ProductID", typeof(int));
        products.Columns.Add("Product Name", typeof(string));
        products.Columns.Add("Category", typeof(string));
        products.Columns.Add("Price", typeof(decimal));
        products.Columns.Add("In Stock", typeof(int));
        
        products.Rows.Add(101, "Laptop", "Electronics", 899.99, 45);
        products.Rows.Add(102, "Mouse", "Accessories", 29.99, 150);
        products.Rows.Add(103, "Keyboard", "Accessories", 79.99, 85);
        products.Rows.Add(104, "Monitor", "Electronics", 299.99, 30);
        
        // Configure combo with headers
        multiColumnComboBox1.MultiColumn = true;
        multiColumnComboBox1.ShowColumnHeader = true;
        multiColumnComboBox1.DataSource = products;
        multiColumnComboBox1.DisplayMember = "Product Name";
        multiColumnComboBox1.ValueMember = "ProductID";
    }
}
```

**VB.NET:**
```vbnet
Imports System
Imports System.Data
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Public Partial Class ProductSelector
    Inherits Form
    
    Private Sub SetupProductCombo()
        ' Create product data
        Dim products As New DataTable("Products")
        products.Columns.Add("ProductID", GetType(Integer))
        products.Columns.Add("Product Name", GetType(String))
        products.Columns.Add("Category", GetType(String))
        products.Columns.Add("Price", GetType(Decimal))
        products.Columns.Add("In Stock", GetType(Integer))
        
        products.Rows.Add(101, "Laptop", "Electronics", 899.99, 45)
        products.Rows.Add(102, "Mouse", "Accessories", 29.99, 150)
        products.Rows.Add(103, "Keyboard", "Accessories", 79.99, 85)
        products.Rows.Add(104, "Monitor", "Electronics", 299.99, 30)
        
        ' Configure combo with headers
        multiColumnComboBox1.MultiColumn = True
        multiColumnComboBox1.ShowColumnHeader = True
        multiColumnComboBox1.DataSource = products
        multiColumnComboBox1.DisplayMember = "Product Name"
        multiColumnComboBox1.ValueMember = "ProductID"
    End Sub
End Class
```

**Visual Result:** Headers display column names ("ProductID", "Product Name", "Category", "Price", "In Stock") at top of dropdown grid.

## Selection Highlighting

### AlphaBlendSelectionColor Property

Customize the color used for selected row highlighting with alpha blending.

**C#:**
```csharp
using System.Drawing;

// Set selection highlight color
multiColumnComboBox1.AlphaBlendSelectionColor = Color.LightBlue;

// Other color examples
multiColumnComboBox1.AlphaBlendSelectionColor = Color.LightGreen;
multiColumnComboBox1.AlphaBlendSelectionColor = Color.LightCoral;
multiColumnComboBox1.AlphaBlendSelectionColor = Color.FromArgb(255, 220, 240); // Custom RGB
```

**VB.NET:**
```vbnet
Imports System.Drawing

' Set selection highlight color
multiColumnComboBox1.AlphaBlendSelectionColor = Color.LightBlue

' Other color examples
multiColumnComboBox1.AlphaBlendSelectionColor = Color.LightGreen
multiColumnComboBox1.AlphaBlendSelectionColor = Color.LightCoral
multiColumnComboBox1.AlphaBlendSelectionColor = Color.FromArgb(255, 220, 240) ' Custom RGB
```

### Theme-Matched Selection Colors

Match selection colors to your application theme:

**C#:**
```csharp
private void ApplyThemedSelection()
{
    // Office 2016 Blue theme
    if (multiColumnComboBox1.Style == VisualStyle.Office2016Colorful)
    {
        multiColumnComboBox1.AlphaBlendSelectionColor = Color.FromArgb(0, 120, 215);
    }
    // Dark theme
    else if (multiColumnComboBox1.Style == VisualStyle.Office2016Black)
    {
        multiColumnComboBox1.AlphaBlendSelectionColor = Color.FromArgb(60, 60, 60);
    }
    // Light theme
    else
    {
        multiColumnComboBox1.AlphaBlendSelectionColor = Color.LightSkyBlue;
    }
}
```

## Dropdown Width Configuration

### DropDownWidth Property

Control the width of the dropdown popup to accommodate your columns.

**C#:**
```csharp
// Set fixed width (in pixels)
multiColumnComboBox1.DropDownWidth = 400;

// Auto-size based on content
multiColumnComboBox1.DropDownWidth = 0; // Auto

// Wide dropdown for many columns
multiColumnComboBox1.DropDownWidth = 600;
```

**VB.NET:**
```vbnet
' Set fixed width (in pixels)
multiColumnComboBox1.DropDownWidth = 400

' Auto-size based on content
multiColumnComboBox1.DropDownWidth = 0 ' Auto

' Wide dropdown for many columns
multiColumnComboBox1.DropDownWidth = 600
```

### Calculating Optimal Width

Calculate width based on column count:

**C#:**
```csharp
private void SetOptimalDropDownWidth()
{
    if (multiColumnComboBox1.DataSource != null)
    {
        DataTable dt = multiColumnComboBox1.DataSource as DataTable;
        if (dt != null)
        {
            // Estimate: 100 pixels per column + 20 pixel padding
            int columnCount = dt.Columns.Count;
            int estimatedWidth = (columnCount * 100) + 20;
            
            // Clamp between min and max
            int minWidth = 200;
            int maxWidth = 800;
            multiColumnComboBox1.DropDownWidth = Math.Max(minWidth, Math.Min(maxWidth, estimatedWidth));
        }
    }
}
```

### Responsive Width Example

**C#:**
```csharp
private void SetupResponsiveDropDown()
{
    DataTable data = CreateSampleData();
    
    multiColumnComboBox1.DataSource = data;
    multiColumnComboBox1.DisplayMember = "Name";
    multiColumnComboBox1.ShowColumnHeader = true;
    
    // Calculate width: control width * 2
    multiColumnComboBox1.DropDownWidth = multiColumnComboBox1.Width * 2;
    
    // Or match form width
    // multiColumnComboBox1.DropDownWidth = this.ClientSize.Width - 40;
}
```

## Custom Filtering

### AllowFiltering Property

Enable filtering to allow users to search across all columns.

**Basic Filtering:**
```csharp
// Enable default filtering (StartsWith on DisplayMember)
multiColumnComboBox1.AllowFiltering = true;
```

**Default Behavior:**
- Filters on `DisplayMember` column only
- Uses `StartsWith` condition
- Case-insensitive
- Updates as user types

### Filter Property

Implement custom filtering logic using predicates.

**Custom Filter Pattern:**
```csharp
public YourForm()
{
    InitializeComponent();
    
    // Enable filtering
    multiColumnComboBox1.AllowFiltering = true;
    
    // Hook text changed event
    multiColumnComboBox1.TextChanged += MultiColumnComboBox_TextChanged;
}

private void MultiColumnComboBox_TextChanged(object sender, EventArgs e)
{
    // Assign custom filter predicate
    multiColumnComboBox1.Filter = FilterRecords;
}

public bool FilterRecords(object o)
{
    // Your custom filter logic
    // Return true to include item, false to exclude
}
```

### Example 1: Filter by Any Column (Contains)

Filter across all columns with "contains" logic:

**C#:**
```csharp
using System;
using System.Data;
using System.Linq;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class FilteringForm : Form
{
    public FilteringForm()
    {
        InitializeComponent();
        SetupFilteredCombo();
    }
    
    private void SetupFilteredCombo()
    {
        // Create sample data
        DataTable employees = CreateEmployeeData();
        
        // Configure combo
        multiColumnComboBox1.DataSource = employees;
        multiColumnComboBox1.DisplayMember = "Name";
        multiColumnComboBox1.ShowColumnHeader = true;
        multiColumnComboBox1.AllowFiltering = true;
        
        // Attach filter handler
        multiColumnComboBox1.TextChanged += ComboBox_TextChanged;
    }
    
    private void ComboBox_TextChanged(object sender, EventArgs e)
    {
        // Apply custom filter on text change
        multiColumnComboBox1.Filter = FilterAnyColumn;
    }
    
    public bool FilterAnyColumn(object o)
    {
        DataRowView row = o as DataRowView;
        if (row == null) return false;
        
        string searchText = multiColumnComboBox1.TextBox.Text.ToLower();
        
        // Empty search = show all
        if (string.IsNullOrEmpty(searchText))
            return true;
        
        // Check if any column contains the search text
        foreach (DataColumn column in row.Row.Table.Columns)
        {
            string cellValue = row[column.ColumnName]?.ToString()?.ToLower() ?? "";
            if (cellValue.Contains(searchText))
                return true;
        }
        
        return false;
    }
    
    private DataTable CreateEmployeeData()
    {
        DataTable dt = new DataTable();
        dt.Columns.Add("EmployeeID", typeof(int));
        dt.Columns.Add("Name", typeof(string));
        dt.Columns.Add("Department", typeof(string));
        dt.Columns.Add("Location", typeof(string));
        
        dt.Rows.Add(1001, "John Smith", "Engineering", "New York");
        dt.Rows.Add(1002, "Sarah Johnson", "Marketing", "Chicago");
        dt.Rows.Add(1003, "Mike Brown", "Engineering", "Boston");
        dt.Rows.Add(1004, "Emily Davis", "Sales", "New York");
        
        return dt;
    }
}
```

**VB.NET:**
```vbnet
Imports System
Imports System.Data
Imports System.Linq
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Public Partial Class FilteringForm
    Inherits Form
    
    Public Sub New()
        InitializeComponent()
        SetupFilteredCombo()
    End Sub
    
    Private Sub SetupFilteredCombo()
        ' Create sample data
        Dim employees As DataTable = CreateEmployeeData()
        
        ' Configure combo
        multiColumnComboBox1.DataSource = employees
        multiColumnComboBox1.DisplayMember = "Name"
        multiColumnComboBox1.ShowColumnHeader = True
        multiColumnComboBox1.AllowFiltering = True
        
        ' Attach filter handler
        AddHandler multiColumnComboBox1.TextChanged, AddressOf ComboBox_TextChanged
    End Sub
    
    Private Sub ComboBox_TextChanged(sender As Object, e As EventArgs)
        ' Apply custom filter on text change
        multiColumnComboBox1.Filter = AddressOf FilterAnyColumn
    End Sub
    
    Public Function FilterAnyColumn(o As Object) As Boolean
        Dim row As DataRowView = TryCast(o, DataRowView)
        If row Is Nothing Then Return False
        
        Dim searchText As String = multiColumnComboBox1.TextBox.Text.ToLower()
        
        ' Empty search = show all
        If String.IsNullOrEmpty(searchText) Then Return True
        
        ' Check if any column contains the search text
        For Each column As DataColumn In row.Row.Table.Columns
            Dim cellValue As String = row(column.ColumnName)?.ToString()?.ToLower()
            If cellValue IsNot Nothing AndAlso cellValue.Contains(searchText) Then Return True
        Next
        
        Return False
    End Function
    
    Private Function CreateEmployeeData() As DataTable
        Dim dt As New DataTable()
        dt.Columns.Add("EmployeeID", GetType(Integer))
        dt.Columns.Add("Name", GetType(String))
        dt.Columns.Add("Department", GetType(String))
        dt.Columns.Add("Location", GetType(String))
        
        dt.Rows.Add(1001, "John Smith", "Engineering", "New York")
        dt.Rows.Add(1002, "Sarah Johnson", "Marketing", "Chicago")
        dt.Rows.Add(1003, "Mike Brown", "Engineering", "Boston")
        dt.Rows.Add(1004, "Emily Davis", "Sales", "New York")
        
        Return dt
    End Function
End Class
```

### Example 2: Filter by Specific Column

Filter only on a specific column (e.g., Department):

**C#:**
```csharp
public bool FilterByDepartment(object o)
{
    DataRowView row = o as DataRowView;
    if (row == null) return false;
    
    string searchText = multiColumnComboBox1.TextBox.Text;
    
    if (string.IsNullOrEmpty(searchText))
        return true;
    
    // Filter only by Department column
    string department = row["Department"]?.ToString() ?? "";
    return department.StartsWith(searchText, StringComparison.OrdinalIgnoreCase);
}
```

### Example 3: Advanced Multi-Condition Filter

Filter with complex conditions:

**C#:**
```csharp
public bool FilterAdvanced(object o)
{
    DataRowView row = o as DataRowView;
    if (row == null) return false;
    
    string searchText = multiColumnComboBox1.TextBox.Text.ToLower();
    
    if (string.IsNullOrEmpty(searchText))
        return true;
    
    // Complex filter: Name OR Department, AND only active employees
    string name = row["Name"]?.ToString()?.ToLower() ?? "";
    string department = row["Department"]?.ToString()?.ToLower() ?? "";
    bool isActive = Convert.ToBoolean(row["IsActive"]);
    
    bool matchesSearch = name.Contains(searchText) || department.Contains(searchText);
    
    return matchesSearch && isActive;
}
```

## Complete Examples

### Example 1: Employee Search with Filtering

**C#:**
```csharp
using System;
using System.Data;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class EmployeeSearchForm : Form
{
    private MultiColumnComboBox employeeCombo;
    private Label statusLabel;
    
    public EmployeeSearchForm()
    {
        InitializeComponent();
        SetupUI();
    }
    
    private void SetupUI()
    {
        // Create combo
        employeeCombo = new MultiColumnComboBox
        {
            Location = new Point(20, 20),
            Size = new Size(400, 30),
            MultiColumn = true,
            ShowColumnHeader = true,
            AllowFiltering = true,
            DropDownWidth = 500,
            AlphaBlendSelectionColor = Color.LightSkyBlue
        };
        
        // Create status label
        statusLabel = new Label
        {
            Location = new Point(20, 60),
            Size = new Size(400, 20),
            Text = "Type to search across all columns..."
        };
        
        // Load data
        LoadEmployeeData();
        
        // Attach events
        employeeCombo.TextChanged += EmployeeCombo_TextChanged;
        employeeCombo.SelectedValueChanged += EmployeeCombo_SelectedValueChanged;
        
        // Add to form
        this.Controls.Add(employeeCombo);
        this.Controls.Add(statusLabel);
    }
    
    private void LoadEmployeeData()
    {
        DataTable dt = new DataTable();
        dt.Columns.Add("ID", typeof(int));
        dt.Columns.Add("Employee Name", typeof(string));
        dt.Columns.Add("Department", typeof(string));
        dt.Columns.Add("Position", typeof(string));
        dt.Columns.Add("Office", typeof(string));
        
        dt.Rows.Add(1001, "John Smith", "Engineering", "Senior Developer", "New York");
        dt.Rows.Add(1002, "Sarah Johnson", "Marketing", "Marketing Manager", "Chicago");
        dt.Rows.Add(1003, "Michael Brown", "Engineering", "Tech Lead", "Boston");
        dt.Rows.Add(1004, "Emily Davis", "Sales", "Sales Director", "New York");
        dt.Rows.Add(1005, "David Wilson", "Engineering", "Developer", "San Francisco");
        dt.Rows.Add(1006, "Lisa Anderson", "HR", "HR Manager", "Chicago");
        
        employeeCombo.DataSource = dt;
        employeeCombo.DisplayMember = "Employee Name";
        employeeCombo.ValueMember = "ID";
    }
    
    private void EmployeeCombo_TextChanged(object sender, EventArgs e)
    {
        employeeCombo.Filter = FilterAllColumns;
        
        // Update status
        int visibleCount = GetFilteredCount();
        statusLabel.Text = $"Found {visibleCount} matching employees";
    }
    
    private bool FilterAllColumns(object o)
    {
        DataRowView row = o as DataRowView;
        if (row == null) return false;
        
        string search = employeeCombo.TextBox.Text.ToLower();
        if (string.IsNullOrEmpty(search)) return true;
        
        // Search all columns
        foreach (DataColumn col in row.Row.Table.Columns)
        {
            string value = row[col.ColumnName]?.ToString()?.ToLower() ?? "";
            if (value.Contains(search))
                return true;
        }
        
        return false;
    }
    
    private void EmployeeCombo_SelectedValueChanged(object sender, EventArgs e)
    {
        if (employeeCombo.SelectedIndex == -1) return;
        
        ComboBoxBaseDataBound combo = employeeCombo as ComboBoxBaseDataBound;
        DataRowView drv = combo.Items[combo.SelectedIndex] as DataRowView;
        
        if (drv != null)
        {
            string name = drv["Employee Name"].ToString();
            string dept = drv["Department"].ToString();
            string position = drv["Position"].ToString();
            
            statusLabel.Text = $"Selected: {name} - {position} ({dept})";
        }
    }
    
    private int GetFilteredCount()
    {
        ComboBoxBaseDataBound combo = employeeCombo as ComboBoxBaseDataBound;
        return combo?.Items.Count ?? 0;
    }
}
```

## Best Practices

### Column Configuration

**1. Show Relevant Columns Only:**
```csharp
// Hide technical IDs, show user-friendly data
multiColumnComboBox1.ListBox.Grid.Model.Cols.Hidden["ID"] = true;
multiColumnComboBox1.ListBox.Grid.Model.Cols.Hidden["InternalCode"] = true;
```

**2. Use Descriptive Column Names:**
```csharp
// Good: User-friendly names
dt.Columns.Add("Employee Name");
dt.Columns.Add("Department");

// Bad: Technical names
dt.Columns.Add("emp_name");
dt.Columns.Add("dept_id");
```

**3. Order Columns by Importance:**
```csharp
// Most important columns first
dt.Columns.Add("Name");        // Primary
dt.Columns.Add("Department");  // Secondary
dt.Columns.Add("Email");       // Tertiary
dt.Columns.Add("Phone");       // Optional
```

### Filtering Best Practices

**1. Handle Empty Search:**
```csharp
if (string.IsNullOrEmpty(searchText))
    return true; // Show all when empty
```

**2. Case-Insensitive Matching:**
```csharp
string search = text.ToLower();
string value = row[column].ToString().ToLower();
```

**3. Null-Safe Comparisons:**
```csharp
string value = row[column]?.ToString() ?? "";
```

### Performance Tips

**1. Optimize Filter Logic:**
```csharp
// Good: Early exit
if (string.IsNullOrEmpty(searchText)) return true;

// Bad: Unnecessary processing
// ... complex logic when search is empty
```

**2. Limit Dropdown Width:**
```csharp
// Reasonable width for usability
multiColumnComboBox1.DropDownWidth = 600;

// Avoid: Too wide
// multiColumnComboBox1.DropDownWidth = 1200;
```

## Troubleshooting

### Filtering Not Working

**Issue:** Typing doesn't filter items.

**Solutions:**
1. Verify `AllowFiltering = true` is set
2. Check TextChanged event is attached
3. Ensure Filter predicate is assigned in event handler
4. Verify predicate returns `true` for items to show

### Default Filter Behavior Unexpected

**Issue:** Default filter only matches beginning of text.

**Explanation:** Default filter uses `StartsWith` on DisplayMember column only.

**Solution:** Implement custom filter for `Contains` or multi-column matching (see examples above).

### Dropdown Too Narrow

**Issue:** Columns are cut off or cramped.

**Solution:** Increase `DropDownWidth`:
```csharp
multiColumnComboBox1.DropDownWidth = 500; // Adjust as needed
```

### Selection Color Not Visible

**Issue:** Selection highlight color doesn't show.

**Solution:** Ensure AlphaBlendSelectionColor contrasts with background:
```csharp
// Good contrast
multiColumnComboBox1.AlphaBlendSelectionColor = Color.LightSkyBlue;

// Bad: Too similar to white background
// multiColumnComboBox1.AlphaBlendSelectionColor = Color.White;
```

### Headers Not Showing

**Issue:** Column headers don't appear.

**Solution:** Set `ShowColumnHeader = true`:
```csharp
multiColumnComboBox1.ShowColumnHeader = true;
```
