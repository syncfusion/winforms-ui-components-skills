# Editing

## Table of Contents
- [Overview](#overview)
- [Edit Activation](#edit-activation)
- [Programmatic Editing](#programmatic-editing)
- [Read-Only Mode](#read-only-mode)
- [Browse-Only Mode](#browse-only-mode)
- [CurrentCell Events](#currentcell-events)
- [Customizing Editing](#customizing-editing)
- [Common Scenarios](#common-scenarios)
- [Best Practices](#best-practices)

## Overview

GridGroupingControl provides comprehensive editing capabilities with control over activation behavior, read-only modes, and programmatic editing operations. This guide covers configuring and controlling cell editing behavior.

### Key Components

- **ActivateCurrentCellBehavior** - Controls how cells enter edit mode
- **CurrentCell** - Active cell with editing state and methods
- **ReadOnly** - Prevents editing while allowing cell activation
- **BrowseOnly** - Prevents both editing and cell activation
- **CurrentCell Events** - Track editing lifecycle

## Edit Activation

### Activation Behavior

Control how users activate cells for editing:

```csharp
// Single click activates editing
gridGroupingControl1.ActivateCurrentCellBehavior = GridCellActivateAction.ClickOnCell;

// Double click activates editing (default for many cell types)
gridGroupingControl1.ActivateCurrentCellBehavior = GridCellActivateAction.DblClickOnCell;

// No automatic activation (must press F2 or type)
gridGroupingControl1.ActivateCurrentCellBehavior = GridCellActivateAction.None;

// Set position only (no edit mode)
gridGroupingControl1.ActivateCurrentCellBehavior = GridCellActivateAction.SetCurrent;
```

### Check Edit State

```csharp
// Check if current cell is in edit mode
if (gridGroupingControl1.TableControl.CurrentCell.IsEditing)
{
    Console.WriteLine("Cell is currently being edited");
}

// Check if current cell is active
if (gridGroupingControl1.TableControl.CurrentCell.HasCurrentCell)
{
    int row = gridGroupingControl1.TableControl.CurrentCell.RowIndex;
    int col = gridGroupingControl1.TableControl.CurrentCell.ColIndex;
    Console.WriteLine($"Current cell: Row {row}, Col {col}");
}
```

## Programmatic Editing

### Start Editing

```csharp
// Begin editing current cell
gridGroupingControl1.TableControl.CurrentCell.BeginEdit();

// Move to specific cell and begin editing
gridGroupingControl1.TableControl.CurrentCell.MoveTo(5, 3); // Row 5, Column 3
gridGroupingControl1.TableControl.CurrentCell.BeginEdit();
```

### Commit Changes

```csharp
// End editing and save changes
gridGroupingControl1.TableControl.CurrentCell.EndEdit();

// Alternative: Confirm changes (also closes dropdowns)
gridGroupingControl1.TableControl.CurrentCell.ConfirmChanges();
```

### Cancel Editing

```csharp
// Cancel editing and discard changes
gridGroupingControl1.TableControl.CurrentCell.CancelEdit();

// Revert to original value
gridGroupingControl1.TableControl.CurrentCell.RejectChanges();
```

### Edit Cell Value Programmatically

```csharp
// Set cell value through CurrentCell
gridGroupingControl1.TableControl.CurrentCell.MoveTo(3, 2);
gridGroupingControl1.TableControl.CurrentCell.BeginEdit();
gridGroupingControl1.TableControl.CurrentCell.Renderer.ControlText = "New Value";
gridGroupingControl1.TableControl.CurrentCell.EndEdit();

// Or set directly on record (for data-bound cells)
Record record = gridGroupingControl1.Table.Records[2];
record.SetValue("FirstName", "John");
```

## Read-Only Mode

### Grid-Level Read-Only

```csharp
// Make entire grid read-only
gridGroupingControl1.TableModel.ReadOnly = true;

// Allow editing
gridGroupingControl1.TableModel.ReadOnly = false;
```

### Column-Level Read-Only

```csharp
// Make specific column read-only
gridGroupingControl1.TableDescriptor.Columns["EmployeeID"].Appearance.AnyRecordFieldCell.ReadOnly = true;

// Multiple columns
foreach (string colName in new[] { "EmployeeID", "CreatedDate", "CreatedBy" })
{
    gridGroupingControl1.TableDescriptor.Columns[colName].Appearance.AnyRecordFieldCell.ReadOnly = true;
}
```

### Cell-Level Read-Only

```csharp
// Make specific cells read-only using QueryCellStyleInfo
gridGroupingControl1.QueryCellStyleInfo += GridGroupingControl1_QueryCellStyleInfo;

void GridGroupingControl1_QueryCellStyleInfo(object sender, GridTableCellStyleInfoEventArgs e)
{
    if (e.TableCellIdentity.TableCellType == GridTableCellType.RecordFieldCell ||
        e.TableCellIdentity.TableCellType == GridTableCellType.AlternateRecordFieldCell)
    {
        // Make "FirstName" read-only for specific records
        if (e.TableCellIdentity.Column?.Name == "FirstName")
        {
            Record record = e.TableCellIdentity.DisplayElement.GetRecord();
            
            // Read-only for record ID 5
            if (record.Id == 5)
            {
                e.Style.ReadOnly = true;
                e.Style.BackColor = Color.LightGray; // Visual indicator
            }
        }
    }
}
```

### Modify Read-Only Cells

Use `IgnoreReadOnly` to programmatically change read-only cells:

```csharp
gridGroupingControl1.QueryCellStyleInfo += (s, e) =>
{
    if (e.TableCellIdentity.Column?.Name == "Status")
    {
        Record record = e.TableCellIdentity.DisplayElement.GetRecord();
        
        if (record.GetValue("IsLocked").ToString() == "True")
        {
            e.Style.ReadOnly = true;
            
            // Allow programmatic changes
            gridGroupingControl1.TableModel.IgnoreReadOnly = true;
            e.Style.CellValue = "Locked - Admin Override";
            gridGroupingControl1.TableModel.IgnoreReadOnly = false;
        }
    }
};
```

## Browse-Only Mode

Browse-only prevents editing without showing edit cursor.

### Enable Browse-Only

```csharp
// Disable editing for entire grid
gridGroupingControl1.BrowseOnly = true;

// Users can navigate but not edit
// No edit cursor appears in cells
```

### ReadOnly vs. BrowseOnly

**ReadOnly:**
- Cell activates (shows edit cursor)
- User can see edit mode but can't change values
- Good for copy/paste scenarios

**BrowseOnly:**
- Cell doesn't activate for editing
- No edit cursor
- Prevents any editing interaction

```csharp
// ReadOnly example
gridGroupingControl1.TableModel.ReadOnly = true;
gridGroupingControl1.ActivateCurrentCellBehavior = GridCellActivateAction.ClickOnCell;
// Result: Cell shows edit cursor but user can't type

// BrowseOnly example
gridGroupingControl1.BrowseOnly = true;
// Result: No edit cursor at all
```

### Prevent Editing for Cell Range

```csharp
// Cancel editing for specific cell ranges
gridGroupingControl1.TableControlCurrentCellStartEditing += GridGroupingControl1_TableControlCurrentCellStartEditing;

void GridGroupingControl1_TableControlCurrentCellStartEditing(object sender, GridTableControlCancelEventArgs e)
{
    GridCurrentCell currentCell = e.TableControl.CurrentCell;
    
    // Prevent editing rows 2-6
    if (currentCell.RangeInfo.IntersectsWith(GridRangeInfo.Rows(2, 6)))
    {
        e.Inner.Cancel = true;
    }
    
    // Prevent editing specific columns
    string columnName = e.TableControl.Model[currentCell.RowIndex, currentCell.ColIndex].CellIdentity.Column?.Name;
    if (columnName == "EmployeeID" || columnName == "CreatedDate")
    {
        e.Inner.Cancel = true;
    }
}
```

## CurrentCell Events

Events tracking current cell state and editing lifecycle.

### Key Events

```csharp
// Current cell navigation
gridGroupingControl1.TableControlCurrentCellActivating += CurrentCellActivating;
gridGroupingControl1.TableControlCurrentCellActivated += CurrentCellActivated;

// Editing lifecycle
gridGroupingControl1.TableControlCurrentCellStartEditing += CurrentCellStartEditing;
gridGroupingControl1.TableControlCurrentCellEditingComplete += CurrentCellEditingComplete;

// Value changes
gridGroupingControl1.TableControlCurrentCellChanging += CurrentCellChanging;
gridGroupingControl1.TableControlCurrentCellChanged += CurrentCellChanged;
```

### Event Handlers

#### Activating (Before cell becomes current)

```csharp
void CurrentCellActivating(object sender, GridTableControlCurrentCellActivatingEventArgs e)
{
    // Cancel navigation to specific cells
    GridCurrentCell cc = e.TableControl.CurrentCell;
    
    if (cc.RowIndex == 5 && cc.ColIndex == 3)
    {
        e.Inner.Cancel = true; // Prevent activating this cell
    }
}
```

#### Activated (After cell becomes current)

```csharp
void CurrentCellActivated(object sender, GridTableControlEventArgs e)
{
    GridCurrentCell cc = e.TableControl.CurrentCell;
    Console.WriteLine($"Cell activated: Row {cc.RowIndex}, Col {cc.ColIndex}");
}
```

#### StartEditing (Before editing begins)

```csharp
void CurrentCellStartEditing(object sender, GridTableControlCancelEventArgs e)
{
    GridCurrentCell cc = e.TableControl.CurrentCell;
    
    // Validate conditions before allowing edit
    string columnName = e.TableControl.Model[cc.RowIndex, cc.ColIndex].CellIdentity.Column?.Name;
    
    if (columnName == "Salary")
    {
        // Check user permissions
        if (!CurrentUser.HasPermission("EditSalary"))
        {
            e.Inner.Cancel = true;
            MessageBox.Show("You don't have permission to edit salary.");
        }
    }
}
```

#### EditingComplete (After editing ends)

```csharp
void CurrentCellEditingComplete(object sender, GridTableControlEventArgs e)
{
    GridCurrentCell cc = e.TableControl.CurrentCell;
    string newValue = cc.Renderer.ControlText;
    
    Console.WriteLine($"Editing complete: {newValue}");
    
    // Trigger dependent cell updates
    e.TableControl.Refresh();
}
```

#### Changing (Before value changes)

```csharp
void CurrentCellChanging(object sender, GridTableControlCancelEventArgs e)
{
    GridCurrentCell cc = e.TableControl.CurrentCell;
    string newValue = cc.Renderer.ControlText;
    
    // Validate new value
    string columnName = e.TableControl.Model[cc.RowIndex, cc.ColIndex].CellIdentity.Column?.Name;
    
    if (columnName == "Age")
    {
        if (!int.TryParse(newValue, out int age) || age < 18 || age > 100)
        {
            e.Inner.Cancel = true;
            MessageBox.Show("Age must be between 18 and 100.");
        }
    }
}
```

#### Changed (After value changed)

```csharp
void CurrentCellChanged(object sender, GridTableControlEventArgs e)
{
    GridCurrentCell cc = e.TableControl.CurrentCell;
    
    // Log change
    string columnName = e.TableControl.Model[cc.RowIndex, cc.ColIndex].CellIdentity.Column?.Name;
    Console.WriteLine($"Cell changed: {columnName}");
    
    // Update dependent calculations
    UpdateTotalColumn();
}
```

## Customizing Editing

### Capture Key Events in Editors

```csharp
// Access cell renderer to capture key events
GridTextBoxCellRenderer textBoxRenderer = 
    (GridTextBoxCellRenderer)gridGroupingControl1.TableControl.CellRenderers["TextBox"];

textBoxRenderer.TextBox.KeyUp += TextBox_KeyUp;
textBoxRenderer.TextBox.KeyDown += TextBox_KeyDown;

void TextBox_KeyUp(object sender, KeyEventArgs e)
{
    Console.WriteLine($"Key pressed: {e.KeyCode}");
    
    // Custom behavior for specific keys
    if (e.KeyCode == Keys.F1)
    {
        ShowHelp();
    }
}
```

### Capture Function Keys

```csharp
// Capture function keys during editing
gridGroupingControl1.TableControlCurrentCellControlKeyMessage += CurrentCellControlKeyMessage;

void CurrentCellControlKeyMessage(object sender, GridTableControlCurrentCellControlKeyMessageEventArgs e)
{
    Keys keyCode = (Keys)((int)e.Inner.Msg.WParam) & Keys.KeyCode;
    
    if (keyCode == Keys.F5)
    {
        // Refresh data
        gridGroupingControl1.TableControl.Refresh();
    }
    else if (keyCode == Keys.F2)
    {
        // Custom F2 behavior
        Console.WriteLine("F2 pressed in edit mode");
    }
}
```

### AutoFit with Placeholders

Display placeholder characters when content exceeds cell width:

```csharp
// Show ### for overflowing numeric content
gridGroupingControl1.TableDescriptor.Columns["LongNumber"].Appearance.AnyRecordFieldCell.AutoFit = Syncfusion.Windows.Forms.Grid.AutoFitOptions.Numeric;

// Show ### for overflowing alphabetic content
gridGroupingControl1.TableDescriptor.Columns["LongText"].Appearance.AnyRecordFieldCell.AutoFit = Syncfusion.Windows.Forms.Grid.AutoFitOptions.Alphabet;

// Show ### for both numeric and alphabetic
gridGroupingControl1.TableDescriptor.Columns["Mixed"].Appearance.AnyRecordFieldCell.AutoFit = Syncfusion.Windows.Forms.Grid.AutoFitOptions.Both;
```

## Common Scenarios

### Scenario 1: Form with Read-Only ID and Dates

```csharp
// ID field: Read-only, auto-generated
gridGroupingControl1.TableDescriptor.Columns["EmployeeID"].Appearance.AnyRecordFieldCell.ReadOnly = true;
gridGroupingControl1.TableDescriptor.Columns["EmployeeID"].Appearance.AnyRecordFieldCell.BackColor = Color.LightGray;

// Created/Modified dates: Read-only, system-managed
gridGroupingControl1.TableDescriptor.Columns["CreatedDate"].Appearance.AnyRecordFieldCell.ReadOnly = true;
gridGroupingControl1.TableDescriptor.Columns["ModifiedDate"].Appearance.AnyRecordFieldCell.ReadOnly = true;

// Editable fields: Single-click activation
gridGroupingControl1.ActivateCurrentCellBehavior = GridCellActivateAction.ClickOnCell;
```

### Scenario 2: Conditional Editing Based on Status

```csharp
gridGroupingControl1.TableControlCurrentCellStartEditing += (s, e) =>
{
    GridCurrentCell cc = e.TableControl.CurrentCell;
    Record record = e.TableControl.Table.DisplayElements[cc.RowIndex] as Record;
    
    if (record != null)
    {
        string status = record.GetValue("Status").ToString();
        
        // Prevent editing if status is "Approved" or "Locked"
        if (status == "Approved" || status == "Locked")
        {
            e.Inner.Cancel = true;
            MessageBox.Show($"Cannot edit records with status: {status}");
        }
    }
};
```

### Scenario 3: Validate and Auto-Correct on Edit

```csharp
gridGroupingControl1.TableControlCurrentCellEditingComplete += (s, e) =>
{
    GridCurrentCell cc = e.TableControl.CurrentCell;
    string columnName = e.TableControl.Model[cc.RowIndex, cc.ColIndex].CellIdentity.Column?.Name;
    
    if (columnName == "Email")
    {
        string email = cc.Renderer.ControlText.Trim().ToLower();
        
        // Auto-correct common mistakes
        email = email.Replace(" ", "");
        
        // Validate format
        if (!email.Contains("@") || !email.Contains("."))
        {
            MessageBox.Show("Invalid email format");
            cc.BeginEdit(); // Re-enter edit mode
            return;
        }
        
        // Update with corrected value
        cc.Renderer.ControlText = email;
    }
};
```

### Scenario 4: Browse-Only with Admin Override

```csharp
// Default: Browse-only for regular users
if (CurrentUser.Role == "User")
{
    gridGroupingControl1.BrowseOnly = true;
}
else if (CurrentUser.Role == "Admin")
{
    // Admins can edit
    gridGroupingControl1.BrowseOnly = false;
    gridGroupingControl1.ActivateCurrentCellBehavior = GridCellActivateAction.ClickOnCell;
}

// Or provide runtime toggle
void ToggleEditMode()
{
    if (CurrentUser.Role == "Admin")
    {
        gridGroupingControl1.BrowseOnly = !gridGroupingControl1.BrowseOnly;
        string mode = gridGroupingControl1.BrowseOnly ? "View" : "Edit";
        btnToggle.Text = $"Switch to {mode} Mode";
    }
}
```

## Best Practices

### Activation Behavior

1. **Match User Expectations**: Use `ClickOnCell` for form-like grids, `DblClickOnCell` for read-mostly grids.

2. **Consistency**: Apply same activation behavior across application for consistent UX.

3. **Consider Cell Type**: Some cell types (ComboBox, CheckBox) should use `ClickOnCell` for quick editing.

### Read-Only Configuration

1. **Visual Feedback**: Change background color for read-only cells:
   ```csharp
   e.Style.ReadOnly = true;
   e.Style.BackColor = Color.LightGray;
   ```

2. **Granularity**: Apply read-only at most appropriate level:
   - Grid-level: Entire grid is read-only
   - Column-level: ID, timestamps, calculated fields
   - Cell-level: Conditional based on record state

3. **Tooltips**: Add tooltips explaining why cells are read-only:
   ```csharp
   e.Style.CellTipText = "This field cannot be edited after approval.";
   ```

### Event Handling

1. **Validation**: Use `CurrentCellChanging` to validate before accepting changes. Cancel if invalid.

2. **Business Logic**: Use `CurrentCellEditingComplete` to trigger calculations, dependent updates.

3. **Performance**: Minimize work in frequently-fired events (`CurrentCellActivating`). Defer heavy operations.

4. **Error Handling**: Wrap event handlers in try-catch to prevent crashes:
   ```csharp
   void CurrentCellChanging(object sender, GridTableControlCancelEventArgs e)
   {
       try
       {
           // Validation logic
       }
       catch (Exception ex)
       {
           e.Inner.Cancel = true;
           MessageBox.Show($"Validation error: {ex.Message}");
       }
   }
   ```

### General

- Test editing with keyboard navigation (Tab, Enter, Escape, F2)
- Document editing restrictions for users
- Provide feedback for validation errors (MessageBox, status bar, cell tooltips)
- Save changes promptly (on `EditingComplete` or provide Save button)
