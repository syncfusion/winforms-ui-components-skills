# Data Binding with CheckBoxAdv

This guide covers data binding techniques for connecting CheckBoxAdv controls to database sources.

## Table of Contents
- [Data Binding Overview](#data-binding-overview)
- [BoolValue Binding](#boolvalue-binding)
- [IntValue Binding](#intvalue-binding)
- [Complete Examples](#complete-examples)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

## Data Binding Overview

The CheckBoxAdv control provides three properties specifically designed for data binding:

| Property | Data Type | Use Case |
|----------|-----------|----------|
| BoolValue | bool | Binding to boolean or bit fields |
| IntValue | int | Binding to integer fields (values: -1, 0, 1) |
| StringValue | string | Binding to string fields (read-only) |

### Why Use Special Binding Properties?

The standard `Checked` and `CheckState` properties can be problematic for data binding because:
- `Checked` doesn't handle indeterminate state well
- `CheckState` is an enum, not directly compatible with most database types

The `BoolValue` and `IntValue` properties provide seamless two-way binding with databases.

## BoolValue Binding

The `BoolValue` property is designed for binding to boolean or bit fields in databases.

### Property Behavior

```csharp
// BoolValue maps to CheckState:
// CheckState.Checked → BoolValue = true
// CheckState.Unchecked → BoolValue = false
// CheckState.Indeterminate → BoolValue = true

checkBoxAdv1.BoolValue = true;  // Sets CheckState to Checked
checkBoxAdv1.BoolValue = false; // Sets CheckState to Unchecked
```

### Basic BoolValue Binding

```csharp
// Simple binding to a DataTable
DataTable dataTable = GetDataTable();
checkBoxAdv1.DataBindings.Add("BoolValue", dataTable, "IsEnabled");
```

```vb
' Simple binding to a DataTable
Dim dataTable As DataTable = GetDataTable()
checkBoxAdv1.DataBindings.Add("BoolValue", dataTable, "IsEnabled")
```

### Complete SQL Server Example

```csharp
using System;
using System.Data;
using System.Data.SqlClient;
using System.IO;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class Form1 : Form
{
    public static string dataBasePath = Path.GetFullPath(@"..\..\Database1.mdf");
    public string connectString = @"Data Source=(LocalDB)\MSSQLLocalDB;AttachDbFilename=" 
                                   + dataBasePath + ";Integrated Security=True";
    
    public Form1()
    {
        InitializeComponent();
        BindBoolValue();
    }
    
    private void BindBoolValue()
    {
        using (SqlConnection sqlConnection = new SqlConnection(connectString))
        {
            sqlConnection.Open();
            
            // Create data adapter
            SqlDataAdapter dataAdapter = new SqlDataAdapter("SELECT * FROM [Settings]", sqlConnection);
            
            // Fill DataTable
            DataTable dataTable = new DataTable("Settings");
            dataAdapter.Fill(dataTable);
            
            // Display in DataGridView (optional)
            dataGridView1.DataSource = dataTable;
            
            // Bind CheckBoxAdv to bit field
            checkBoxAdv1.DataBindings.Add("BoolValue", dataTable, "IsEnabled");
        }
    }
}
```

```vb
Imports System
Imports System.Data
Imports System.Data.SqlClient
Imports System.IO
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Public Partial Class Form1
    Inherits Form
    
    Public Shared dataBasePath As String = Path.GetFullPath("..\..\Database1.mdf")
    Public connectString As String = "Data Source=(LocalDB)\MSSQLLocalDB;AttachDbFilename=" & _
                                      dataBasePath & ";Integrated Security=True"
    
    Public Sub New()
        InitializeComponent()
        BindBoolValue()
    End Sub
    
    Private Sub BindBoolValue()
        Using sqlConnection As SqlConnection = New SqlConnection(connectString)
            sqlConnection.Open()
            
            ' Create data adapter
            Dim dataAdapter As SqlDataAdapter = New SqlDataAdapter("SELECT * FROM [Settings]", sqlConnection)
            
            ' Fill DataTable
            Dim dataTable As DataTable = New DataTable("Settings")
            dataAdapter.Fill(dataTable)
            
            ' Display in DataGridView (optional)
            dataGridView1.DataSource = dataTable
            
            ' Bind CheckBoxAdv to bit field
            checkBoxAdv1.DataBindings.Add("BoolValue", dataTable, "IsEnabled")
        End Using
    End Sub
End Class
```

### SQL Table Structure for BoolValue

```sql
CREATE TABLE Settings (
    Id INT PRIMARY KEY IDENTITY(1,1),
    SettingName NVARCHAR(100),
    IsEnabled BIT NOT NULL DEFAULT 0,
    Description NVARCHAR(255)
);

-- Insert sample data
INSERT INTO Settings (SettingName, IsEnabled, Description)
VALUES 
    ('AutoSave', 1, 'Automatically save changes'),
    ('ShowNotifications', 0, 'Display system notifications'),
    ('DarkMode', 1, 'Use dark theme');
```

## IntValue Binding

The `IntValue` property is designed for binding to integer fields in databases.

### Property Behavior

The IntValue property maps integer values to CheckState:
- **1** → CheckState.Checked
- **0** → CheckState.Unchecked
- **-1** → CheckState.Indeterminate

You can customize these mappings using:
- `CheckedInt` property (default: 1)
- `UncheckedInt` property (default: 0)
- `IndeterminateInt` property (default: -1)

### Basic IntValue Binding

```csharp
// Binding to integer field with standard values (0, 1, -1)
DataTable dataTable = GetDataTable();
checkBoxAdv1.DataBindings.Add("IntValue", dataTable, "StatusCode");
```

```vb
' Binding to integer field
Dim dataTable As DataTable = GetDataTable()
checkBoxAdv1.DataBindings.Add("IntValue", dataTable, "StatusCode")
```

### Complete SQL Server Example

```csharp
using System;
using System.Data;
using System.Data.SqlClient;
using System.IO;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class Form1 : Form
{
    public static string dataBasePath = Path.GetFullPath(@"..\..\Database1.mdf");
    public string connectString = @"Data Source=(LocalDB)\MSSQLLocalDB;AttachDbFilename=" 
                                   + dataBasePath + ";Integrated Security=True";
    
    public Form1()
    {
        InitializeComponent();
        BindIntValue();
    }
    
    private void BindIntValue()
    {
        using (SqlConnection sqlConnection = new SqlConnection(connectString))
        {
            sqlConnection.Open();
            
            // Create data adapter
            SqlDataAdapter dataAdapter = new SqlDataAdapter("SELECT * FROM [Tasks]", sqlConnection);
            
            // Fill DataTable
            DataTable dataTable = new DataTable("Tasks");
            dataAdapter.Fill(dataTable);
            
            // Display in DataGridView (optional)
            dataGridView1.DataSource = dataTable;
            
            // Bind CheckBoxAdv to integer field
            checkBoxAdv1.DataBindings.Add("IntValue", dataTable, "CompletionStatus");
        }
    }
}
```

```vb
Imports System
Imports System.Data
Imports System.Data.SqlClient
Imports System.IO
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Public Partial Class Form1
    Inherits Form
    
    Public Shared dataBasePath As String = Path.GetFullPath("..\..\Database1.mdf")
    Public connectString As String = "Data Source=(LocalDB)\MSSQLLocalDB;AttachDbFilename=" & _
                                      dataBasePath & ";Integrated Security=True"
    
    Public Sub New()
        InitializeComponent()
        BindIntValue()
    End Sub
    
    Private Sub BindIntValue()
        Using sqlConnection As SqlConnection = New SqlConnection(connectString)
            sqlConnection.Open()
            
            ' Create data adapter
            Dim dataAdapter As SqlDataAdapter = New SqlDataAdapter("SELECT * FROM [Tasks]", sqlConnection)
            
            ' Fill DataTable
            Dim dataTable As DataTable = New DataTable("Tasks")
            dataAdapter.Fill(dataTable)
            
            ' Display in DataGridView (optional)
            dataGridView1.DataSource = dataTable
            
            ' Bind CheckBoxAdv to integer field
            checkBoxAdv1.DataBindings.Add("IntValue", dataTable, "CompletionStatus")
        End Using
    End Sub
End Class
```

### SQL Table Structure for IntValue

```sql
CREATE TABLE Tasks (
    Id INT PRIMARY KEY IDENTITY(1,1),
    TaskName NVARCHAR(100),
    CompletionStatus INT NOT NULL DEFAULT 0,
    -- 1 = Complete, 0 = Not Started, -1 = In Progress
    DueDate DATETIME,
    Priority NVARCHAR(20)
);

-- Insert sample data
INSERT INTO Tasks (TaskName, CompletionStatus, DueDate, Priority)
VALUES 
    ('Design UI', 1, '2026-03-25', 'High'),
    ('Write Tests', 0, '2026-03-30', 'Medium'),
    ('Code Review', -1, '2026-03-28', 'High');
```

### Important IntValue Constraint

The database field bound to `IntValue` **must contain only** -1, 0, or 1:
- Other integer values will cause unexpected behavior
- Use custom mappings with CheckedInt/UncheckedInt/IndeterminateInt if needed

```csharp
// Custom integer mappings
checkBoxAdv1.CheckedInt = 100;      // Complete
checkBoxAdv1.UncheckedInt = 0;      // Not Started
checkBoxAdv1.IndeterminateInt = 50; // In Progress

// Now IntValue will work with 0, 50, 100
checkBoxAdv1.DataBindings.Add("IntValue", dataTable, "StatusCode");
```

## Complete Examples

### Example 1: Simple Settings Form

```csharp
public class SettingsForm : Form
{
    private CheckBoxAdv autoSaveCheckBox;
    private CheckBoxAdv notificationsCheckBox;
    private Button saveButton;
    private string connectionString;
    
    public SettingsForm()
    {
        InitializeComponent();
        LoadSettings();
    }
    
    private void LoadSettings()
    {
        using (SqlConnection conn = new SqlConnection(connectionString))
        {
            conn.Open();
            SqlDataAdapter adapter = new SqlDataAdapter(
                "SELECT * FROM UserSettings WHERE UserId = @UserId", 
                conn
            );
            adapter.SelectCommand.Parameters.AddWithValue("@UserId", CurrentUserId);
            
            DataTable dt = new DataTable();
            adapter.Fill(dt);
            
            // Bind checkboxes
            autoSaveCheckBox.DataBindings.Add("BoolValue", dt, "AutoSaveEnabled");
            notificationsCheckBox.DataBindings.Add("BoolValue", dt, "NotificationsEnabled");
        }
    }
    
    private void saveButton_Click(object sender, EventArgs e)
    {
        // Changes are automatically reflected in the DataTable
        // Use SqlDataAdapter.Update() to save to database
        SaveChangesToDatabase();
    }
}
```

### Example 2: Task List with Status Tracking

```csharp
public class TaskListForm : Form
{
    private DataGridView taskGrid;
    private CheckBoxAdv taskStatusCheckBox;
    private SqlDataAdapter dataAdapter;
    private DataTable tasksTable;
    
    public TaskListForm()
    {
        InitializeComponent();
        LoadTasks();
    }
    
    private void LoadTasks()
    {
        using (SqlConnection conn = new SqlConnection(connectionString))
        {
            conn.Open();
            dataAdapter = new SqlDataAdapter("SELECT * FROM Tasks ORDER BY DueDate", conn);
            
            tasksTable = new DataTable();
            dataAdapter.Fill(tasksTable);
            
            // Show in grid
            taskGrid.DataSource = tasksTable;
            
            // Bind checkbox to selected row
            taskStatusCheckBox.DataBindings.Add("IntValue", tasksTable, "CompletionStatus");
        }
    }
    
    private void taskGrid_SelectionChanged(object sender, EventArgs e)
    {
        // Checkbox automatically updates when row selection changes
    }
    
    private void SaveChanges()
    {
        SqlCommandBuilder builder = new SqlCommandBuilder(dataAdapter);
        dataAdapter.Update(tasksTable);
        MessageBox.Show("Changes saved successfully!");
    }
}
```

### Example 3: Master-Detail Binding

```csharp
public class MasterDetailForm : Form
{
    private BindingSource masterBindingSource;
    private BindingSource detailBindingSource;
    private CheckBoxAdv detailCheckBox;
    
    private void SetupBindings()
    {
        // Load master data
        DataTable orders = LoadOrders();
        masterBindingSource = new BindingSource();
        masterBindingSource.DataSource = orders;
        
        // Load detail data
        DataTable orderDetails = LoadOrderDetails();
        detailBindingSource = new BindingSource();
        detailBindingSource.DataSource = orderDetails;
        
        // Establish relationship
        DataRelation relation = new DataRelation(
            "OrderDetails",
            orders.Columns["OrderId"],
            orderDetails.Columns["OrderId"]
        );
        orders.DataSet.Relations.Add(relation);
        
        // Bind checkbox to detail
        detailCheckBox.DataBindings.Add("BoolValue", detailBindingSource, "IsShipped");
        
        // Navigate master-detail
        masterBindingSource.PositionChanged += (s, e) =>
        {
            detailBindingSource.DataSource = masterBindingSource.Current;
        };
    }
}
```

## Common Patterns

### Pattern 1: Two-Way Binding with Auto-Save

```csharp
private void SetupAutoSaveBinding()
{
    DataTable settings = LoadSettings();
    checkBoxAdv1.DataBindings.Add("BoolValue", settings, "AutoSave");
    
    // Save changes immediately when checkbox changes
    checkBoxAdv1.CheckedChanged += (s, e) =>
    {
        SaveSettingsToDatabase(settings);
    };
}
```

### Pattern 2: Binding Multiple Checkboxes

```csharp
private void BindMultipleCheckBoxes(DataTable dataTable)
{
    // Each checkbox binds to different column
    checkBoxAdv1.DataBindings.Add("BoolValue", dataTable, "Feature1Enabled");
    checkBoxAdv2.DataBindings.Add("BoolValue", dataTable, "Feature2Enabled");
    checkBoxAdv3.DataBindings.Add("BoolValue", dataTable, "Feature3Enabled");
}
```

### Pattern 3: Conditional Binding Based on User Role

```csharp
private void BindBasedOnRole(UserRole role)
{
    DataTable settings = LoadSettings();
    
    if (role == UserRole.Administrator)
    {
        // Full binding
        checkBoxAdv1.DataBindings.Add("BoolValue", settings, "AdvancedFeature");
        checkBoxAdv1.ReadOnlyMode = false;
    }
    else
    {
        // Read-only binding
        checkBoxAdv1.DataBindings.Add("BoolValue", settings, "AdvancedFeature");
        checkBoxAdv1.ReadOnlyMode = true;
    }
}
```

### Pattern 4: Validation Before Committing

```csharp
private void SetupValidatedBinding()
{
    DataTable dt = LoadData();
    Binding binding = new Binding("BoolValue", dt, "CriticalSetting");
    
    binding.Parse += (sender, e) =>
    {
        // Validate before updating data source
        if ((bool)e.Value == true)
        {
            DialogResult result = MessageBox.Show(
                "Enable critical setting?",
                "Confirm",
                MessageBoxButtons.YesNo
            );
            
            if (result == DialogResult.No)
            {
                e.Value = false;
            }
        }
    };
    
    checkBoxAdv1.DataBindings.Add(binding);
}
```

## Troubleshooting

### Issue: Binding Not Working

**Symptoms:** Changes to checkbox don't reflect in database or vice versa.

**Solutions:**
1. Verify column name matches exactly (case-sensitive)
2. Ensure DataTable is filled before binding
3. Check that property name is correct ("BoolValue" not "BooleanValue")

```csharp
// WRONG
checkBoxAdv1.DataBindings.Add("Checked", dataTable, "IsEnabled");

// CORRECT
checkBoxAdv1.DataBindings.Add("BoolValue", dataTable, "IsEnabled");
```

### Issue: IntValue Binding Shows Unexpected States

**Cause:** Database contains integer values other than -1, 0, 1.

**Solution:** Clean data or use custom mappings:

```csharp
// Option 1: Clean the data
UPDATE Tasks SET CompletionStatus = 0 WHERE CompletionStatus NOT IN (-1, 0, 1);

// Option 2: Custom mappings
checkBoxAdv1.CheckedInt = 100;
checkBoxAdv1.UncheckedInt = 0;
checkBoxAdv1.IndeterminateInt = 50;
```

### Issue: Changes Not Persisting to Database

**Cause:** DataAdapter.Update() not called.

**Solution:**

```csharp
// After making changes, update the database
SqlCommandBuilder builder = new SqlCommandBuilder(dataAdapter);
int rowsAffected = dataAdapter.Update(dataTable);
Console.WriteLine($"Updated {rowsAffected} rows");
```

### Issue: Binding Fails with NULL Values

**Cause:** Database field allows NULL, but BoolValue/IntValue don't.

**Solution:** Handle NULL in SQL query or use default values:

```sql
-- Option 1: Use COALESCE in query
SELECT Id, COALESCE(IsEnabled, 0) AS IsEnabled FROM Settings;

-- Option 2: Add NOT NULL constraint with default
ALTER TABLE Settings 
ALTER COLUMN IsEnabled BIT NOT NULL;

ALTER TABLE Settings 
ADD CONSTRAINT DF_IsEnabled DEFAULT 0 FOR IsEnabled;
```

### Issue: Multiple Bindings Conflict

**Symptom:** Binding to both Checked and BoolValue causes errors.

**Solution:** Use only one binding property:

```csharp
// WRONG - Don't bind multiple properties
checkBoxAdv1.DataBindings.Add("Checked", dt, "IsEnabled");
checkBoxAdv1.DataBindings.Add("BoolValue", dt, "IsEnabled");

// CORRECT - Use only BoolValue
checkBoxAdv1.DataBindings.Add("BoolValue", dt, "IsEnabled");
```
