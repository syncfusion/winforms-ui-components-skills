# Column Management

## Table of Contents
- [Overview](#overview)
- [Hiding and Showing Columns](#hiding-and-showing-columns)
- [Moving Columns](#moving-columns)
- [Resizing Columns](#resizing-columns)
- [Frozen Columns](#frozen-columns)
- [Unbound Columns](#unbound-columns)
- [Row Height Management](#row-height-management)
- [Common Scenarios](#common-scenarios)
- [Best Practices](#best-practices)

## Overview

GridGroupingControl provides comprehensive column and record management capabilities including hiding/showing, moving, resizing, freezing columns, and creating unbound columns. This guide covers programmatic and interactive column management operations.

### Key Components

- **VisibleColumns** - Collection managing visible column order
- **GridColumnDescriptor** - Column configuration and properties
- **UnboundFields** - Create columns not bound to data source
- **FrozenCount** - Lock columns from scrolling
- **QueryValue/SaveValue** - Events for unbound column data

## Hiding and Showing Columns

### Hide Columns Programmatically

```csharp
// Hide specific column
gridGroupingControl1.TableDescriptor.VisibleColumns.Remove("EmployeeID");

// Hide multiple columns
gridGroupingControl1.TableDescriptor.VisibleColumns.Remove("FirstName");
gridGroupingControl1.TableDescriptor.VisibleColumns.Remove("LastName");
```

### Show Hidden Columns

```csharp
// Add column back to visible collection
gridGroupingControl1.TableDescriptor.VisibleColumns.Add("EmployeeID");

// Insert at specific position (index 2)
gridGroupingControl1.TableDescriptor.VisibleColumns.Insert(2, "EmployeeID");
```

### Hiding Records

```csharp
// Hide specific records by ID
gridGroupingControl1.Table.Records.HideRecord(recordId);

// Hide multiple records
foreach (var record in recordsToHide)
{
    gridGroupingControl1.Table.Records.HideRecord(record.Id);
}

// Show hidden records
gridGroupingControl1.Table.Records.ShowHiddenRecords();
```

### Hide Row/Column Headers

```csharp
// Hide row headers
gridGroupingControl1.TableModel.Options.ShowRowHeaders = false;

// Hide column headers
gridGroupingControl1.TableModel.Options.ShowColumnHeaders = false;

// Show both
gridGroupingControl1.TableModel.Options.ShowRowHeaders = true;
gridGroupingControl1.TableModel.Options.ShowColumnHeaders = true;
```

## Moving Columns

### Interactive Column Reordering

Users can drag-drop column headers to reorder columns. Enable with:

```csharp
// Enable drag-drop column reordering
gridGroupingControl1.TableOptions.AllowDragColumns = true;
```

### Programmatic Column Movement

```csharp
// Move column to new position
// Move "LastName" to position 1 (after index 0)
gridGroupingControl1.TableDescriptor.VisibleColumns.Move(
    gridGroupingControl1.TableDescriptor.VisibleColumns.IndexOf("LastName"), 
    1);

// Move column to end
string columnName = "EmployeeID";
int currentIndex = gridGroupingControl1.TableDescriptor.VisibleColumns.IndexOf(columnName);
int lastIndex = gridGroupingControl1.TableDescriptor.VisibleColumns.Count - 1;
gridGroupingControl1.TableDescriptor.VisibleColumns.Move(currentIndex, lastIndex);
```

### Reorder Multiple Columns

```csharp
// Define new column order
string[] newOrder = { "EmployeeID", "FirstName", "LastName", "Title", "Country" };

// Clear and re-add in new order
gridGroupingControl1.TableDescriptor.VisibleColumns.Clear();
foreach (string colName in newOrder)
{
    gridGroupingControl1.TableDescriptor.VisibleColumns.Add(colName);
}
```

## Resizing Columns

### Set Column Width

```csharp
// Set specific column width
gridGroupingControl1.TableDescriptor.Columns["FirstName"].Width = 150;

// Set default width for all columns
gridGroupingControl1.TableModel.Options.DefaultColumnWidth = 100;
```

### Proportional Column Sizing

```csharp
// Enable proportional column widths
gridGroupingControl1.TableModel.Options.ColumnWidthMode = GridColumnWidthMode.Proportional;

// Set column proportions (FillWidth property)
gridGroupingControl1.TableDescriptor.Columns["FirstName"].FillWeight = 2;  // 2x width
gridGroupingControl1.TableDescriptor.Columns["EmployeeID"].FillWeight = 1; // 1x width
gridGroupingControl1.TableDescriptor.Columns["Title"].FillWeight = 3;      // 3x width
```

### Auto-Fit Columns

```csharp
// Auto-fit all columns to content
foreach (GridColumnDescriptor column in gridGroupingControl1.TableDescriptor.Columns)
{
    column.Width = -1; // -1 triggers auto-fit
}

// Auto-fit specific column
gridGroupingControl1.TableDescriptor.Columns["FirstName"].Width = -1;
```

### User Resizing Control

```csharp
// Allow users to resize columns
gridGroupingControl1.TableOptions.AllowResizeColumns = true;

// Prevent resizing
gridGroupingControl1.TableOptions.AllowResizeColumns = false;

// Prevent resizing specific column
gridGroupingControl1.TableDescriptor.Columns["EmployeeID"].AllowResize = false;
```

## Frozen Columns

Frozen columns remain visible while scrolling horizontally.

### Freeze Leading Columns

```csharp
// Freeze first 2 columns (index 0 and 1)
gridGroupingControl1.TableModel.Options.FrozenCount = 2;

// Unfreeze all columns
gridGroupingControl1.TableModel.Options.FrozenCount = 0;
```

### Freeze Trailing Columns

```csharp
// Freeze last 2 columns
gridGroupingControl1.TableModel.Options.TrailingFrozenCount = 2;
```

### Example: Freeze ID Column

```csharp
// Keep EmployeeID visible while scrolling
gridGroupingControl1.TableModel.Options.FrozenCount = 1;

// Ensure EmployeeID is first column
if (gridGroupingControl1.TableDescriptor.VisibleColumns.IndexOf("EmployeeID") != 0)
{
    gridGroupingControl1.TableDescriptor.VisibleColumns.Move(
        gridGroupingControl1.TableDescriptor.VisibleColumns.IndexOf("EmployeeID"), 
        0);
}
```

## Unbound Columns

Unbound columns display calculated or custom data not present in the data source.

### Create Unbound Column

```csharp
// Add unbound field
GridUnboundField unboundField = new GridUnboundField("FullName");
unboundField.MappingName = "FullName";
gridGroupingControl1.TableDescriptor.UnboundFields.Add(unboundField);

// Add to visible columns
gridGroupingControl1.TableDescriptor.VisibleColumns.Add("FullName");
```

### Populate Unbound Column Data

Use `QueryValue` event to provide data for unbound columns:

```csharp
gridGroupingControl1.TableDescriptor.QueryValue += TableDescriptor_QueryValue;

void TableDescriptor_QueryValue(object sender, FieldValueEventArgs e)
{
    if (e.Field.Name == "FullName")
    {
        // Get record
        Record record = e.Record as Record;
        
        // Calculate value from other fields
        string firstName = record.GetValue("FirstName").ToString();
        string lastName = record.GetValue("LastName").ToString();
        
        e.Value = $"{firstName} {lastName}";
    }
}
```

### Editable Unbound Columns

Handle `SaveValue` event to persist changes:

```csharp
gridGroupingControl1.TableDescriptor.SaveValue += TableDescriptor_SaveValue;

void TableDescriptor_SaveValue(object sender, FieldValueEventArgs e)
{
    if (e.Field.Name == "FullName")
    {
        Record record = e.Record as Record;
        
        // Parse and save to underlying fields
        string[] parts = e.Value.ToString().Split(' ');
        if (parts.Length >= 2)
        {
            record.SetValue("FirstName", parts[0]);
            record.SetValue("LastName", parts[1]);
        }
    }
}
```

### Unbound Column with Complex Calculation

```csharp
// Add calculated "Bonus" column
GridUnboundField bonusField = new GridUnboundField("Bonus");
bonusField.MappingName = "Bonus";
gridGroupingControl1.TableDescriptor.UnboundFields.Add(bonusField);
gridGroupingControl1.TableDescriptor.VisibleColumns.Add("Bonus");

// Calculate bonus based on salary
gridGroupingControl1.TableDescriptor.QueryValue += (s, e) =>
{
    if (e.Field.Name == "Bonus")
    {
        Record record = e.Record as Record;
        decimal salary = Convert.ToDecimal(record.GetValue("Salary"));
        
        // 10% bonus for high performers
        bool isHighPerformer = Convert.ToBoolean(record.GetValue("IsHighPerformer"));
        e.Value = isHighPerformer ? salary * 0.10m : 0;
    }
};
```

## Row Height Management

### Set Row Heights

```csharp
// Default record row height
gridGroupingControl1.TableModel.Options.DefaultRecordRowHeight = 25;

// Column header row height
gridGroupingControl1.TableModel.Options.DefaultColumnHeaderRowHeight = 30;

// Filter bar row height
gridGroupingControl1.TableModel.Options.FilterBarRowHeight = 28;

// Caption row height (group headers)
gridGroupingControl1.TableModel.Options.CaptionRowHeight = 35;
```

### Dynamic Row Heights

```csharp
// Enable variable row heights based on content
gridGroupingControl1.TableModel.Options.RowHeightsMode = GridRowHeightsMode.Variable;

// Set specific row height
gridGroupingControl1.TableControl.Model.RowHeights[5] = 40;
```

## Common Scenarios

### Scenario 1: Employee Grid with Frozen ID

```csharp
// Configure employee grid
gridGroupingControl1.DataSource = employeesTable;

// Hide sensitive columns
gridGroupingControl1.TableDescriptor.VisibleColumns.Remove("SSN");
gridGroupingControl1.TableDescriptor.VisibleColumns.Remove("Salary");

// Freeze EmployeeID column
gridGroupingControl1.TableModel.Options.FrozenCount = 1;

// Set column widths
gridGroupingControl1.TableDescriptor.Columns["EmployeeID"].Width = 80;
gridGroupingControl1.TableDescriptor.Columns["FirstName"].Width = 120;
gridGroupingControl1.TableDescriptor.Columns["LastName"].Width = 120;
gridGroupingControl1.TableDescriptor.Columns["Email"].Width = 200;

// Allow user resizing
gridGroupingControl1.TableOptions.AllowResizeColumns = true;
```

### Scenario 2: Add Full Name Unbound Column

```csharp
// Create unbound column for full name
GridUnboundField fullNameField = new GridUnboundField("FullName");
fullNameField.MappingName = "FullName";
gridGroupingControl1.TableDescriptor.UnboundFields.Add(fullNameField);

// Insert at position 1 (after EmployeeID)
gridGroupingControl1.TableDescriptor.VisibleColumns.Insert(1, "FullName");

// Populate data
gridGroupingControl1.TableDescriptor.QueryValue += (s, e) =>
{
    if (e.Field.Name == "FullName")
    {
        Record record = e.Record as Record;
        e.Value = $"{record.GetValue("FirstName")} {record.GetValue("LastName")}";
    }
};

// Set appearance
gridGroupingControl1.TableDescriptor.Columns["FullName"].Appearance.AnyRecordFieldCell.Font.Bold = true;
```

### Scenario 3: Reorder Columns to Logical Groups

```csharp
// Group related columns together
string[] columnOrder = 
{
    "EmployeeID",      // Identification
    "FirstName",       // Personal Info
    "LastName",
    "BirthDate",
    "Title",           // Job Info
    "Department",
    "HireDate",
    "City",            // Location
    "Country",
    "Email",           // Contact
    "Phone"
};

gridGroupingControl1.TableDescriptor.VisibleColumns.Clear();
foreach (string col in columnOrder)
{
    if (gridGroupingControl1.TableDescriptor.Columns.Contains(col))
    {
        gridGroupingControl1.TableDescriptor.VisibleColumns.Add(col);
    }
}
```

### Scenario 4: Dynamic Column Visibility by User Role

```csharp
// Hide columns based on user permissions
void ConfigureColumnsByRole(UserRole role)
{
    var visibleCols = gridGroupingControl1.TableDescriptor.VisibleColumns;
    
    // Admin sees all columns
    if (role == UserRole.Admin)
    {
        // Show all
        return;
    }
    
    // Regular users can't see salary info
    if (role == UserRole.User)
    {
        visibleCols.Remove("Salary");
        visibleCols.Remove("Bonus");
        visibleCols.Remove("BankAccount");
    }
    
    // Guests see minimal info
    if (role == UserRole.Guest)
    {
        visibleCols.Clear();
        visibleCols.Add("FirstName");
        visibleCols.Add("LastName");
        visibleCols.Add("Department");
        visibleCols.Add("Email");
    }
}
```

## Best Practices

### Column Management

1. **Hide vs. Remove**: Use `VisibleColumns.Remove()` to hide columns (they remain in data source). Don't remove from `Columns` collection.

2. **Column Order Persistence**: Save `VisibleColumns` order to user settings:
   ```csharp
   // Save
   string[] order = gridGroupingControl1.TableDescriptor.VisibleColumns
       .Select(vc => vc.MappingName).ToArray();
   Properties.Settings.Default.ColumnOrder = string.Join(",", order);
   
   // Restore
   string[] savedOrder = Properties.Settings.Default.ColumnOrder.Split(',');
   gridGroupingControl1.TableDescriptor.VisibleColumns.Clear();
   foreach (string col in savedOrder)
       gridGroupingControl1.TableDescriptor.VisibleColumns.Add(col);
   ```

3. **Frozen Column Limit**: Freeze only 1-3 columns. Too many frozen columns reduce scrollable area.

4. **Unbound Performance**: Minimize calculations in `QueryValue`. Cache computed values when possible.

### Resizing

1. **Set Reasonable Defaults**: Provide good default widths. Users can adjust as needed.
   ```csharp
   gridGroupingControl1.TableDescriptor.Columns["ID"].Width = 60;
   gridGroupingControl1.TableDescriptor.Columns["Name"].Width = 150;
   gridGroupingControl1.TableDescriptor.Columns["Description"].Width = 300;
   ```

2. **Proportional for Variable Content**: Use proportional sizing when content length varies:
   ```csharp
   gridGroupingControl1.TableModel.Options.ColumnWidthMode = GridColumnWidthMode.Proportional;
   ```

3. **Save Column Widths**: Persist user-adjusted widths:
   ```csharp
   // Save on ColumnWidthChanged
   gridGroupingControl1.TableControl.Model.ColumnWidthsChanged += (s, e) =>
   {
       // Serialize widths to settings
   };
   ```

### Unbound Columns

1. **Validate Before Saving**: In `SaveValue`, validate and handle errors:
   ```csharp
   void TableDescriptor_SaveValue(object sender, FieldValueEventArgs e)
   {
       try
       {
           // Parse and validate
           // Save to underlying data
       }
       catch (Exception ex)
       {
           MessageBox.Show($"Invalid value: {ex.Message}");
           e.Value = e.Record.GetValue(e.Field.Name); // Revert
       }
   }
   ```

2. **Performance**: For large datasets, consider caching unbound values instead of recalculating in `QueryValue`.

3. **Naming**: Use clear names: "FullName", "TotalPrice", "AgeInYears" (not "Calc1", "Field1").

### General

- Test column operations with grouping enabled (operations affect group structure)
- Refresh display after bulk column changes: `gridGroupingControl1.TableControl.Refresh()`
- Document hidden columns for users (provide "Show All Columns" option)
