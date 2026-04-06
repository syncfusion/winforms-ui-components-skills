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

### Settings Form with Multiple Checkboxes

```csharp
public class SettingsForm : Form
{
    private CheckBoxAdv autoSaveCheckBox;
    private CheckBoxAdv notificationsCheckBox;
    private string connectionString;
    
    private void LoadSettings()
    {
        using (SqlConnection conn = new SqlConnection(connectionString))
        {
            conn.Open();
            SqlDataAdapter adapter = new SqlDataAdapter(
                "SELECT * FROM UserSettings WHERE UserId = @UserId", conn);
            adapter.SelectCommand.Parameters.AddWithValue("@UserId", CurrentUserId);
            
            DataTable dt = new DataTable();
            adapter.Fill(dt);
            
            // Bind multiple checkboxes
            autoSaveCheckBox.DataBindings.Add("BoolValue", dt, "AutoSaveEnabled");
            notificationsCheckBox.DataBindings.Add("BoolValue", dt, "NotificationsEnabled");
            
            // Save changes
            SqlCommandBuilder builder = new SqlCommandBuilder(adapter);
            adapter.Update(dt);
        }
    }
}
```

## Common Patterns

### Auto-Save with Validation

```csharp
private void SetupAutoSaveBinding()
{
    DataTable settings = LoadSettings();
    Binding binding = new Binding("BoolValue", settings, "CriticalSetting");
    
    // Validate before saving
    binding.Parse += (sender, e) =>
    {
        if ((bool)e.Value == true && !ConfirmChange())
        {
            e.Value = false;
        }
    };
    
    checkBoxAdv1.DataBindings.Add(binding);
    
    // Auto-save on change
    checkBoxAdv1.CheckedChanged += (s, e) => SaveSettingsToDatabase(settings);
}
```

### Conditional Binding

```csharp
private void BindBasedOnRole(UserRole role)
{
    DataTable settings = LoadSettings();
    checkBoxAdv1.DataBindings.Add("BoolValue", settings, "AdvancedFeature");
    checkBoxAdv1.ReadOnlyMode = (role != UserRole.Administrator);
}
```

## Troubleshooting

### Common Binding Issues

| Issue | Solution |
|-------|----------|
| Binding not working | Use "BoolValue" or "IntValue" property, not "Checked" |
| IntValue unexpected states | Ensure database values are -1, 0, or 1, or use custom mappings |
| Changes not persisting | Call `dataAdapter.Update(dataTable)` after changes |
| NULL value errors | Use COALESCE in SQL: `SELECT COALESCE(IsEnabled, 0) AS IsEnabled` |
| Multiple binding conflicts | Bind only one property per checkbox |

```csharp
// Correct binding approach
checkBoxAdv1.DataBindings.Add("BoolValue", dataTable, "IsEnabled");

// Custom IntValue mappings for non-standard values
checkBoxAdv1.CheckedInt = 100;
checkBoxAdv1.UncheckedInt = 0;
checkBoxAdv1.IndeterminateInt = 50;

// Persist changes
SqlCommandBuilder builder = new SqlCommandBuilder(dataAdapter);
dataAdapter.Update(dataTable);
```
