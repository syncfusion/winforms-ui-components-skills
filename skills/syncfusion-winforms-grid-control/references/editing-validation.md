# Editing and Validation

This guide covers editing functionality and validation in GridControl, including enabling/disabling editing, handling edit events, and implementing validation logic.

## Overview

By default, GridControl allows editing in all cells. You can control editing behavior at the grid level or per-cell basis, and implement custom validation logic using events.

## Enabling and Disabling Editing

### Grid-Level ReadOnly:

```csharp
// Disable editing for entire grid
gridControl1.ReadOnly = true;

// Enable editing (default)
gridControl1.ReadOnly = false;
```

### Cell-Level ReadOnly:

```csharp
// Disable editing for specific cells
gridControl1[2, 2].ReadOnly = true;
gridControl1[3, 3].ReadOnly = true;

// Make specific cells editable in a read-only grid
gridControl1.ReadOnly = true;
gridControl1[5, 5].ReadOnly = false;  // This cell can still be edited
```

### Range-Level ReadOnly:

```csharp
//Make entire row read-only
GridStyleInfo style = new GridStyleInfo();
style.ReadOnly = true;
gridControl1.ChangeCells(GridRangeInfo.Row(3), style);

// Make entire column read-only
gridControl1.ChangeCells(GridRangeInfo.Col(4), style);

// Make range read-only
gridControl1.ChangeCells(GridRangeInfo.Cells(5, 2, 10, 6), style);
```

## Edit Mode and Behavior

### Activating Edit Mode:

Users can activate editing by:
- **Clicking** a cell (if already current cell)
- **Pressing F2** key
- **Typing** directly (if configured)
- **Double-clicking** a cell

### Configuring Edit Behavior:

```csharp
// Allow immediate editing on typing
gridControl1.ActivateCurrentCellBehavior = GridCellActivateAction.DblClickOnCell;

// Require double-click to edit
gridControl1.ActivateCurrentCellBehavior = GridCellActivateAction.ClickOnCell;

// Pressing F2 to edit
gridControl1.ActivateCurrentCellBehavior = GridCellActivateAction.SetCurrent;
```

## Edit Events

### CurrentCellStartEditing Event

Fired when a cell is about to enter edit mode. Use this to prevent editing or customize the edit behavior.

```csharp
gridControl1.CurrentCellStartEditing += GridControl1_CurrentCellStartEditing;

private void GridControl1_CurrentCellStartEditing(object sender, CancelEventArgs e)
{
    int row = gridControl1.CurrentCell.RowIndex;
    int col = gridControl1.CurrentCell.ColIndex;
    
    // Prevent editing in column 3
    if (col == 3)
    {
        e.Cancel = true;
        MessageBox.Show("Column 3 cannot be edited");
        return;
    }
    
    // Conditional editing based on other cell values
    if (row > 1 && string.IsNullOrEmpty(gridControl1[row, 1].Text))
    {
        e.Cancel = true;
        MessageBox.Show("Please fill in the first column first");
    }
}
```

### CurrentCellEditingComplete Event

Fired when editing is complete. Use this for validation and post-edit processing.

```csharp
gridControl1.CurrentCellEditingComplete += GridControl1_CurrentCellEditingComplete;

private void GridControl1_CurrentCellEditingComplete(object sender, CancelEventArgs e)
{
    int row = gridControl1.CurrentCell.RowIndex;
    int col = gridControl1.CurrentCell.ColIndex;
    string value = gridControl1.CurrentCell.Renderer.ControlText;
    
    // Validate the entered value
    if (col == 2 && !IsValidEmail(value))
    {
        MessageBox.Show("Please enter a valid email address");
        e.Cancel = true;  // Keep cell in edit mode
        return;
    }
    
    // Post-processing after successful edit
    if (col == 3)
    {
        // Calculate dependent cell
        UpdateCalculatedCell(row);
    }
}

private bool IsValidEmail(string email)
{
    try
    {
        var addr = new System.Net.Mail.MailAddress(email);
        return addr.Address == email;
    }
    catch
    {
        return false;
    }
}
```

### CurrentCellValidating Event

Provides more detailed validation control:

```csharp
gridControl1.CurrentCellValidating += GridControl1_CurrentCellValidating;

private void GridControl1_CurrentCellValidating(object sender, CancelEventArgs e)
{
    string value = gridControl1.CurrentCell.Renderer.ControlText;
    
    // Numeric validation
    if (!double.TryParse(value, out double number))
    {
        MessageBox.Show("Please enter a valid number");
        e.Cancel = true;
    }
}
```

## Validation Patterns

### Required Field Validation:

```csharp
private void ValidateRequiredField(object sender, CancelEventArgs e)
{
    string value = gridControl1.CurrentCell.Renderer.ControlText;
    
    if (string.IsNullOrWhiteSpace(value))
    {
        MessageBox.Show("This field is required");
        e.Cancel = true;
        
        // Highlight the cell
        gridControl1.CurrentCell.StyleInfo.BackColor = Color.LightPink;
    }
}
```

### Range Validation:

```csharp
private void ValidateNumericRange(object sender, CancelEventArgs e)
{
    string value = gridControl1.CurrentCell.Renderer.ControlText;
    
    if (double.TryParse(value, out double number))
    {
        if (number < 0 || number > 100)
        {
            MessageBox.Show("Value must be between 0 and 100");
            e.Cancel = true;
        }
    }
}
```

### Pattern Validation (Regex):

```csharp
private void ValidatePhoneNumber(object sender, CancelEventArgs e)
{
    string value = gridControl1.CurrentCell.Renderer.ControlText;
    string pattern = @"^\d{3}-\d{3}-\d{4}$";  // 123-456-7890
    
    if (!System.Text.RegularExpressions.Regex.IsMatch(value, pattern))
    {
        MessageBox.Show("Please enter phone in format: 123-456-7890");
        e.Cancel = true;
    }
}
```

### Cross-Field Validation:

```csharp
private void ValidateEndDateAfterStartDate(object sender, CancelEventArgs e)
{
    int row = gridControl1.CurrentCell.RowIndex;
    
    // Assuming column 2 is start date, column 3 is end date
    if (gridControl1.CurrentCell.ColIndex == 3)
    {
        DateTime startDate = Convert.ToDateTime(gridControl1[row, 2].CellValue);
        DateTime endDate = Convert.ToDateTime(gridControl1.CurrentCell.Renderer.ControlText);
        
        if (endDate < startDate)
        {
            MessageBox.Show("End date must be after start date");
            e.Cancel = true;
        }
    }
}
```

## Advanced Validation

### Custom Validation with Visual Feedback:

```csharp
private void ValidateWithVisualFeedback(object sender, CancelEventArgs e)
{
    int row = gridControl1.CurrentCell.RowIndex;
    int col = gridControl1.CurrentCell.ColIndex;
    string value = gridControl1.CurrentCell.Renderer.ControlText;
    
    // Clear previous validation styling
    gridControl1[row, col].BackColor = Color.White;
    gridControl1[row, col].CellTipText = "";
    
    // Validate
    string errorMessage = ValidateCell(value, col);
    
    if (!string.IsNullOrEmpty(errorMessage))
    {
        // Show error styling
        gridControl1[row, col].BackColor = Color.LightPink;
        gridControl1[row, col].CellTipText = errorMessage;
        e.Cancel = true;
        MessageBox.Show(errorMessage, "Validation Error");
    }
}

private string ValidateCell(string value, int column)
{
    switch (column)
    {
        case 2:
            if (string.IsNullOrEmpty(value))
                return "Name is required";
            break;
        case 3:
            if (!int.TryParse(value, out int age) || age < 0 || age > 150)
                return "Age must be between 0 and 150";
            break;
        case 4:
            if (!IsValidEmail(value))
                return "Invalid email format";
            break;
    }
    return null;
}
```

### Batch Validation:

```csharp
private bool ValidateAllCells()
{
    bool allValid = true;
    
    for (int row = 2; row <= gridControl1.RowCount; row++)
    {
        for (int col = 2; col <= gridControl1.ColCount; col++)
        {
            string value = gridControl1[row, col].Text;
            string error = ValidateCell(value, col);
            
            if (!string.IsNullOrEmpty(error))
            {
                gridControl1[row, col].BackColor = Color.LightPink;
                gridControl1[row, col].CellTipText = error;
                allValid = false;
            }
            else
            {
                gridControl1[row, col].BackColor = Color.White;
                gridControl1[row, col].CellTipText = "";
            }
        }
    }
    
    if (!allValid)
    {
        MessageBox.Show("Please fix validation errors (cells in pink)");
    }
    
    return allValid;
}
```

## Preventing Edit Mode

### Conditionally Prevent Editing:

```csharp
private void ConditionalEditPrevention(object sender, CancelEventArgs e)
{
    int row = gridControl1.CurrentCell.RowIndex;
    int col = gridControl1.CurrentCell.ColIndex;
    
    // Don't allow editing if row is marked as "Locked"
    if (gridControl1[row, 1].Text == "Locked")
    {
        e.Cancel = true;
        MessageBox.Show("This row is locked and cannot be edited");
    }
    
    // Don't allow editing calculated cells
    if (col == 5)  // Assuming column 5 is calculated
    {
        e.Cancel = true;
    }
}
```

## Edit Completion Actions

### Auto-Calculate on Edit:

```csharp
private void AutoCalculateOnEdit(object sender, CancelEventArgs e)
{
    int row = gridControl1.CurrentCell.RowIndex;
    int col = gridControl1.CurrentCell.ColIndex;
    
    // If quantity or price changed, recalculate total
    if (col == 2 || col == 3)
    {
        double quantity = Convert.ToDouble(gridControl1[row, 2].CellValue ?? 0);
        double price = Convert.ToDouble(gridControl1[row, 3].CellValue ?? 0);
        gridControl1[row, 4].CellValue = quantity * price;
    }
}
```

### Data Synchronization:

```csharp
private void SyncDataOnEdit(object sender, CancelEventArgs e)
{
    int row = gridControl1.CurrentCell.RowIndex;
    int col = gridControl1.CurrentCell.ColIndex;
    string value = gridControl1.CurrentCell.Renderer.ControlText;
    
    // Update underlying data source
    UpdateDatabaseRecord(row, col, value);
}
```

## Best Practices

1. **Use CurrentCellStartEditing** to prevent editing before it starts
2. **Use CurrentCellEditingComplete** for validation and post-processing
3. **Provide clear feedback** for validation failures
4. **Keep validation fast** to avoid UI lag
5. **Use visual cues** (colors, tooltips) for validation errors
6. **Validate early** to catch errors quickly
7. **Allow cancellation** with ESC key behavior
8. **Consider user experience** with helpful error messages

## Common Patterns

### Form Validation on Submit:

```csharp
private void SubmitButton_Click(object sender, EventArgs e)
{
    if (ValidateAllCells())
    {
        SaveData();
        MessageBox.Show("Data saved successfully!");
    }
}
```

### Real-Time Validation:

```csharp
gridControl1.CurrentCellChanged += (s, ev) =>
{
    // Validate previous cell before moving
    if (previousCell != null)
    {
        ValidateCell(previousCell.Row, previousCell.Col);
    }
};
```

## Troubleshooting

### Validation not working
- Ensure events are properly subscribed
- Check if e.Cancel is being set correctly
- Verify validation logic is correct

### Cell stays in edit mode
- Don't set e.Cancel = true unless validation fails
- Check for exceptions in validation code

### Edit mode not activating
- Check ReadOnly property
- Verify ActivateCurrentCellBehavior setting
- Ensure cell type supports editing

## Next Steps

- Implement custom edit controls
- Add data binding with validation
- Create validation rules engine
- Implement conditional formatting based on validation
