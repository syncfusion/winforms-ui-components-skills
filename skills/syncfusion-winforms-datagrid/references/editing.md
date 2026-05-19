# Editing Reference for WinForms DataGrid (SfDataGrid)

## Table of Contents

1. [Enable/Disable Editing](#enabledisable-editing)
2. [Edit Modes](#edit-modes)
3. [Cursor Placement](#cursor-placement)
4. [Editing Events](#editing-events)
5. [Programmatic Editing](#programmatic-editing)
6. [Cell Value Changes](#cell-value-changes)
7. [Validation](#validation)
8. [Edge Cases and Troubleshooting](#edge-cases-and-troubleshooting)

## Enable/Disable Editing

### Enabling Editing for Entire Grid

**C# Example:**
```csharp
sfDataGrid1.AllowEditing = true;
```

**VB.NET Example:**
```vb
sfDataGrid1.AllowEditing = True
```

### Disabling Editing for Entire Grid

**C# Example:**
```csharp
sfDataGrid1.AllowEditing = false;
```

**VB.NET Example:**
```vb
sfDataGrid1.AllowEditing = False
```

### Enabling Editing for Specific Column

**C# Example:**
```csharp
sfDataGrid1.Columns["CustomerID"].AllowEditing = true;
```

**VB.NET Example:**
```vb
sfDataGrid1.Columns("CustomerID").AllowEditing = True
```

### Disabling Editing for Specific Column

**C# Example:**
```csharp
sfDataGrid1.Columns["OrderID"].AllowEditing = false;
```

**VB.NET Example:**
```vb
sfDataGrid1.Columns("OrderID").AllowEditing = False
```

## Edit Modes

### Single Click Edit Mode

Enters edit mode on single click.

**C# Example:**
```csharp
sfDataGrid1.EditMode = EditMode.SingleClick;
```

**VB.NET Example:**
```vb
sfDataGrid1.EditMode = EditMode.SingleClick
```

### Double Click Edit Mode

Enters edit mode on double click (default behavior).

**C# Example:**
```csharp
sfDataGrid1.EditMode = EditMode.DoubleClick;
```

**VB.NET Example:**
```vb
sfDataGrid1.EditMode = EditMode.DoubleClick
```

## Cursor Placement

### SelectAll Behavior

Selects all text when entering edit mode.

**C# Example:**
```csharp
sfDataGrid1.EditorSelectionBehavior = EditorSelectionBehavior.SelectAll;
```

**VB.NET Example:**
```vb
sfDataGrid1.EditorSelectionBehavior = EditorSelectionBehavior.SelectAll
```

### MovesToEnd Behavior

Moves cursor to end when entering edit mode.

**C# Example:**
```csharp
sfDataGrid1.EditorSelectionBehavior = EditorSelectionBehavior.MoveLast;
```

**VB.NET Example:**
```vb
sfDataGrid1.EditorSelectionBehavior = EditorSelectionBehavior.MoveLast
```

## Editing Events

### CurrentCellBeginEdit Event

Occurs before a cell enters edit mode.

**C# Example:**
```csharp
sfDataGrid1.CurrentCellBeginEdit += SfDataGrid1_CurrentCellBeginEdit;

private void SfDataGrid1_CurrentCellBeginEdit(object sender, CurrentCellBeginEditEventArgs e)
{
    // Cancel editing for specific column
    if (e.DataColumn.GridColumn.MappingName == "OrderID")
    {
        e.Cancel = true;
    }
    
    // Cancel editing based on row data
    var record = e.DataRow.RowData as OrderInfo;
    if (record != null && record.IsClosed)
    {
        e.Cancel = true;
    }
}
```

**VB.NET Example:**
```vb
AddHandler sfDataGrid1.CurrentCellBeginEdit, AddressOf SfDataGrid1_CurrentCellBeginEdit

Private Sub SfDataGrid1_CurrentCellBeginEdit(ByVal sender As Object, ByVal e As CurrentCellBeginEditEventArgs)
    ' Cancel editing for specific column
    If e.DataColumn.GridColumn.MappingName = "OrderID" Then
        e.Cancel = True
    End If
    
    ' Cancel editing based on row data
    Dim record = TryCast(e.DataRow.RowData, OrderInfo)
    If record IsNot Nothing AndAlso record.IsClosed Then
        e.Cancel = True
    End If
End Sub
```

### CurrentCellEndEdit Event

Occurs after editing is completed.

**C# Example:**
```csharp
sfDataGrid1.CurrentCellEndEdit += SfDataGrid1_CurrentCellEndEdit;

private void SfDataGrid1_CurrentCellEndEdit(object sender, CurrentCellEndEditEventArgs e)
{
    // Perform actions after editing
    if (e.DataColumn.GridColumn.MappingName == "UnitPrice")
    {
        // Recalculate totals
        CalculateTotalPrice();
    }
}
```

**VB.NET Example:**
```vb
AddHandler sfDataGrid1.CurrentCellEndEdit, AddressOf SfDataGrid1_CurrentCellEndEdit

Private Sub SfDataGrid1_CurrentCellEndEdit(ByVal sender As Object, ByVal e As CurrentCellEndEditEventArgs)
    ' Perform actions after editing
    If e.DataColumn.GridColumn.MappingName = "UnitPrice" Then
        ' Recalculate totals
        CalculateTotalPrice()
    End If
End Sub
```

### EditingControlShowing Event

Occurs when the editing control is displayed.

**C# Example:**
```csharp
sfDataGrid1.EditingControlShowing += SfDataGrid1_EditingControlShowing;

private void SfDataGrid1_EditingControlShowing(object sender, DataGridEditingControlShowingEventArgs e)
{
    if (e.DataColumn.GridColumn.MappingName == "ProductName")
    {
        var textBox = e.Control as TextBox;
        if (textBox != null)
        {
            textBox.MaxLength = 50;
            textBox.CharacterCasing = CharacterCasing.Upper;
        }
    }
}
```

**VB.NET Example:**
```vb
AddHandler sfDataGrid1.EditingControlShowing, AddressOf SfDataGrid1_EditingControlShowing

Private Sub SfDataGrid1_EditingControlShowing(ByVal sender As Object, ByVal e As DataGridEditingControlShowingEventArgs)
    If e.DataColumn.GridColumn.MappingName = "ProductName" Then
        Dim textBox = TryCast(e.Control, TextBox)
        If textBox IsNot Nothing Then
            textBox.MaxLength = 50
            textBox.CharacterCasing = CharacterCasing.Upper
        End If
    End If
End Sub
```

## Programmatic Editing

### BeginEdit Method

Starts editing for the current cell.

**C# Example:**
```csharp
// Begin edit on current cell
sfDataGrid1.CurrentCell.BeginEdit();

// Begin edit on specific cell
sfDataGrid1.MoveToCurrentCell(new RowColumnIndex(3, 2));
sfDataGrid1.CurrentCell.BeginEdit();
```

**VB.NET Example:**
```vb
' Begin edit on current cell
sfDataGrid1.CurrentCell.BeginEdit()

' Begin edit on specific cell
sfDataGrid1.MoveToCurrentCell(New RowColumnIndex(3, 2))
sfDataGrid1.CurrentCell.BeginEdit()
```

### EndEdit Method

Ends editing and commits changes.

**C# Example:**
```csharp
// End edit on current cell
sfDataGrid1.CurrentCell.EndEdit();

// End edit and update cell value
sfDataGrid1.CurrentCell.EndEdit(true);
```

**VB.NET Example:**
```vb
' End edit on current cell
sfDataGrid1.CurrentCell.EndEdit()

' End edit and update cell value
sfDataGrid1.CurrentCell.EndEdit(True)
```

### CancelEdit Method

Cancels editing and discards changes.

**C# Example:**
```csharp
sfDataGrid1.CurrentCell.CancelEdit();
```

**VB.NET Example:**
```vb
sfDataGrid1.CurrentCell.CancelEdit()
```

### Batch Updates

**C# Example:**
```csharp
// Suspend updates
sfDataGrid1.View.BeginInit();

try
{
    foreach (var record in sfDataGrid1.View.Records)
    {
        var orderInfo = record.Data as OrderInfo;
        if (orderInfo != null && orderInfo.Quantity > 10)
        {
            orderInfo.Discount = 0.15m;
        }
    }
}
finally
{
    // Resume updates
    sfDataGrid1.View.EndInit();
}
```

**VB.NET Example:**
```vb
' Suspend updates
sfDataGrid1.View.BeginInit()

Try
    For Each record In sfDataGrid1.View.Records
        Dim orderInfo = TryCast(record.Data, OrderInfo)
        If orderInfo IsNot Nothing AndAlso orderInfo.Quantity > 10 Then
            orderInfo.Discount = 0.15D
        End If
    Next record
Finally
    ' Resume updates
    sfDataGrid1.View.EndInit()
End Try
```

## Validation

### Cell-Level Validation

**C# Example:**
```csharp
sfDataGrid1.CurrentCellValidating += SfDataGrid1_CurrentCellValidating;

private void SfDataGrid1_CurrentCellValidating(object sender, CurrentCellValidatingEventArgs e)
{
    if (e.Column.MappingName == "UnitPrice")
    {
        decimal price;
        if (decimal.TryParse(e.NewValue.ToString(), out price))
        {
            if (price < 0)
            {
                e.IsValid = false;
                e.ErrorMessage = "Price cannot be negative";
            }
            else if (price > 10000)
            {
                e.IsValid = false;
                e.ErrorMessage = "Price cannot exceed $10,000";
            }
        }
        else
        {
            e.IsValid = false;
            e.ErrorMessage = "Invalid price format";
        }
    }
}
```

**VB.NET Example:**
```vb
AddHandler sfDataGrid1.CurrentCellValidating, AddressOf SfDataGrid1_CurrentCellValidating

Private Sub SfDataGrid1_CurrentCellValidating(ByVal sender As Object, ByVal e As CurrentCellValidatingEventArgs)
    If e.Column.MappingName = "UnitPrice" Then
        Dim price As Decimal
        If Decimal.TryParse(e.NewValue.ToString(), price) Then
            If price < 0 Then
                e.IsValid = False
                e.ErrorMessage = "Price cannot be negative"
            ElseIf price > 10000 Then
                e.IsValid = False
                e.ErrorMessage = "Price cannot exceed $10,000"
            End If
        Else
            e.IsValid = False
            e.ErrorMessage = "Invalid price format"
        End If
    End If
End Sub
```

### Row-Level Validation

**C# Example:**
```csharp
sfDataGrid1.RowValidating += SfDataGrid1_RowValidating;

private void SfDataGrid1_RowValidating(object sender, RowValidatingEventArgs e)
{
    var record = e.DataRow.RowData as OrderInfo;
    if (record != null)
    {
        // Validate multiple fields
        if (record.Quantity > 0 && record.UnitPrice <= 0)
        {
            e.IsValid = false;
            e.ErrorMessage = "Price must be greater than zero when quantity is specified";
        }
        
        if (record.OrderDate > DateTime.Now)
        {
            e.IsValid = false;
            e.ErrorMessage = "Order date cannot be in the future";
        }
    }
}
```

**VB.NET Example:**
```vb
AddHandler sfDataGrid1.RowValidating, AddressOf SfDataGrid1_RowValidating

Private Sub SfDataGrid1_RowValidating(ByVal sender As Object, ByVal e As RowValidatingEventArgs)
    Dim record = TryCast(e.DataRow.RowData, OrderInfo)
    If record IsNot Nothing Then
        ' Validate multiple fields
        If record.Quantity > 0 AndAlso record.UnitPrice <= 0 Then
            e.IsValid = False
            e.ErrorMessages.Add("UnitPrice", "Price must be greater than zero when quantity is specified")
        End If
        
        If record.OrderDate > DateTime.Now Then
            e.IsValid = False
            e.ErrorMessages.Add("OrderDate", "Order date cannot be in the future")
        End If
    End If
End Sub
```

### Data Annotation Validation

**C# Example:**
```csharp
using System.ComponentModel.DataAnnotations;

public class OrderInfo
{
    [Required(ErrorMessage = "Order ID is required")]
    [Range(1, 999999, ErrorMessage = "Order ID must be between 1 and 999999")]
    public int OrderID { get; set; }
    
    [Required(ErrorMessage = "Customer ID is required")]
    [StringLength(10, ErrorMessage = "Customer ID cannot exceed 10 characters")]
    public string CustomerID { get; set; }
    
    [Range(0.01, 10000, ErrorMessage = "Unit Price must be between $0.01 and $10,000")]
    public decimal UnitPrice { get; set; }
    
    [Range(1, 1000, ErrorMessage = "Quantity must be between 1 and 1000")]
    public int Quantity { get; set; }
}

// Enable data annotation validation
sfDataGrid1.DataValidationMode = DataValidationMode.InView;
sfDataGrid1.ValidationMode = ValidationMode.InView;
```

## Edge Cases and Troubleshooting

### Issue: Editing Not Working
**Solution:**
- Verify `AllowEditing` is set to `true`
- Check if column's `AllowEditing` is not set to `false`
- Ensure data source supports editing (implements INotifyPropertyChanged)
- Verify grid is not in read-only mode

### Issue: Cell Value Not Updating
**Solution:**
- Implement INotifyPropertyChanged in data object
- Call `EndEdit()` to commit changes
- Check if validation is failing
- Verify data binding is correct

### Issue: Edit Mode Not Activating
**Solution:**
- Check `EditMode` setting (SingleClick vs DoubleClick)
- Verify cell is not in a non-editable state
- Ensure CurrentCellBeginEdit event is not canceling edit
- Check if column type supports editing

### Issue: Validation Not Working
**Solution:**
- Wire up validation events properly
- Set `DataValidationMode` and `ValidationMode` properties
- Implement Data Annotations correctly
- Check if validation errors are being cleared

### Issue: Custom Editor Not Showing
**Solution:**
- Verify EditingControlShowing event handler
- Check control type compatibility
- Ensure proper event wiring
- Test with default editor first

### Best Practices
- Always implement INotifyPropertyChanged in data objects
- Use appropriate edit modes for user experience
- Implement comprehensive validation
- Handle editing events for custom business logic
- Test editing with different data types
- Provide clear validation error messages
- Consider read-only columns for calculated fields
- Use BeginEdit/EndEdit for programmatic changes
- Test batch updates for performance
- Document custom editing behaviors
