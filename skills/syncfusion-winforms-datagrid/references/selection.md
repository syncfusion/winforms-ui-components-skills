# DataGrid Selection Reference

Complete reference guide for implementing selection functionality in Syncfusion WinForms DataGrid (SfDataGrid).

## Table of Contents

1. [Selection Modes](#selection-modes)
2. [Selection Units](#selection-units)
3. [Navigation Modes](#navigation-modes)
4. [Programmatic Selection](#programmatic-selection)
5. [Getting Selected Items](#getting-selected-items)
6. [Getting Cell Values](#getting-cell-values)
7. [Clearing Selection](#clearing-selection)
8. [Keyboard Navigation](#keyboard-navigation)
9. [Mouse Behavior](#mouse-behavior)
10. [Selection Events](#selection-events)
11. [Selection Appearance](#selection-appearance)
12. [Edge Cases and Troubleshooting](#edge-cases-and-troubleshooting)

---

## Selection Modes

### Single Row or Cell Selection

Select a single row or cell at a time. Use `SingleDeselect` to allow deselection by clicking again.

**C# Example:**
```csharp
// Single selection mode
sfDataGrid.SelectionMode = GridSelectionMode.Single;

// Single selection with deselect capability
sfDataGrid.SelectionMode = GridSelectionMode.SingleDeselect;
```

**VB.NET Example:**
```vb
' Single selection mode
sfDataGrid.SelectionMode = GridSelectionMode.Single

' Single selection with deselect capability
sfDataGrid.SelectionMode = GridSelectionMode.SingleDeselect
```

### Multiple Selection

Select multiple rows or cells using Extended or Multiple mode.

**C# Example:**
```csharp
// Extended mode - use Ctrl/Shift keys for multi-select
sfDataGrid.SelectionMode = GridSelectionMode.Extended;

// Multiple mode - click to select/deselect without modifiers
sfDataGrid.SelectionMode = GridSelectionMode.Multiple;
```

**VB.NET Example:**
```vb
' Extended mode - use Ctrl/Shift keys for multi-select
sfDataGrid.SelectionMode = GridSelectionMode.Extended

' Multiple mode - click to select/deselect without modifiers
sfDataGrid.SelectionMode = GridSelectionMode.Multiple
```

### Disable Selection

**C# Example:**
```csharp
// Disable all selection
sfDataGrid.SelectionMode = GridSelectionMode.None;

// Disable focus for specific column
sfDataGrid.Columns[2].AllowFocus = false;
```

**VB.NET Example:**
```vb
' Disable all selection
sfDataGrid.SelectionMode = GridSelectionMode.None

' Disable focus for specific column
sfDataGrid.Columns(2).AllowFocus = False
```

---

## Selection Units

### Row Selection

**C# Example:**
```csharp
// Select rows
sfDataGrid.SelectionUnit = SelectionUnit.Row;
```

**VB.NET Example:**
```vb
' Select rows
sfDataGrid.SelectionUnit = SelectionUnit.Row
```

### Cell Selection

**C# Example:**
```csharp
// Select cells
sfDataGrid.SelectionUnit = SelectionUnit.Cell;

// Select rows or cells (row selected via row header)
sfDataGrid.SelectionUnit = SelectionUnit.Any;
```

**VB.NET Example:**
```vb
' Select cells
sfDataGrid.SelectionUnit = SelectionUnit.Cell

' Select rows or cells (row selected via row header)
sfDataGrid.SelectionUnit = SelectionUnit.Any
```

---

## Navigation Modes

### Cell Navigation

**C# Example:**
```csharp
// Navigate between cells and rows
sfDataGrid.NavigationMode = NavigationMode.Cell;
```

**VB.NET Example:**
```vb
' Navigate between cells and rows
sfDataGrid.NavigationMode = NavigationMode.Cell
```

### Row Navigation

**C# Example:**
```csharp
// Navigate between rows only
// Note: Cannot use with cell selection
sfDataGrid.NavigationMode = NavigationMode.Row;
```

**VB.NET Example:**
```vb
' Navigate between rows only
' Note: Cannot use with cell selection
sfDataGrid.NavigationMode = NavigationMode.Row
```

---

## Programmatic Selection

### Select by Item or Index

**C# Example:**
```csharp
// Select by record
var record = SelectionHelper.GetRecordAtRowIndex(sfDataGrid, 3);
sfDataGrid.SelectedItem = record;

// Select by index
var recordIndex = sfDataGrid.TableControl.ResolveToRecordIndex(5);
sfDataGrid.SelectedIndex = recordIndex;
```

**VB.NET Example:**
```vb
' Select by record
Dim record = SelectionHelper.GetRecordAtRowIndex(sfDataGrid, 3)
sfDataGrid.SelectedItem = record

' Select by index
Dim recordIndex = sfDataGrid.TableControl.ResolveToRecordIndex(5)
sfDataGrid.SelectedIndex = recordIndex
```

### Select Multiple Items

**C# Example:**
```csharp
// Add multiple items to selection
var records = sfDataGrid.View.Records;

foreach (var record in records)
{
    var obj = record.Data as OrderInfo;
    if (obj.ProductName == "Raclette Courdavault")
        sfDataGrid.SelectedItems.Add(obj);
}
```

**VB.NET Example:**
```vb
' Add multiple items to selection
Dim records = sfDataGrid.View.Records

For Each record In records
    Dim obj = TryCast(record.Data, OrderInfo)
    If obj.ProductName = "Raclette Courdavault" Then
        sfDataGrid.SelectedItems.Add(obj)
    End If
Next record
```

### Select Row Range

**C# Example:**
```csharp
// Select rows from index 3 to 6
sfDataGrid.SelectRows(3, 6);
```

**VB.NET Example:**
```vb
' Select rows from index 3 to 6
sfDataGrid.SelectRows(3, 6)
```

### Select Single Cell

**C# Example:**
```csharp
// Select a specific cell
var record = sfDataGrid.View.Records[5];
var column = sfDataGrid.Columns[2];
sfDataGrid.SelectCell(record, column);
```

**VB.NET Example:**
```vb
' Select a specific cell
Dim record = sfDataGrid.View.Records(5)
Dim column = sfDataGrid.Columns(2)
sfDataGrid.SelectCell(record, column)
```

### Select Cell Range

**C# Example:**
```csharp
// Select range of cells
sfDataGrid.SelectCells(
    sfDataGrid.View.Records[5],
    sfDataGrid.Columns["ProductName"],
    sfDataGrid.View.Records[10],
    sfDataGrid.Columns["Quantity"]
);
```

**VB.NET Example:**
```vb
' Select range of cells
sfDataGrid.SelectCells(
    sfDataGrid.View.Records(5),
    sfDataGrid.Columns("ProductName"),
    sfDataGrid.View.Records(10),
    sfDataGrid.Columns("Quantity")
)
```

### Select All

**C# Example:**
```csharp
// Select all rows or cells
sfDataGrid.SelectAll();
```

**VB.NET Example:**
```vb
' Select all rows or cells
sfDataGrid.SelectAll()
```

### Manage Current Cell

**C# Example:**
```csharp
// Set current item
sfDataGrid.CurrentItem = SelectionHelper.GetRecordAtRowIndex(sfDataGrid, 2);

// Move current cell to specific position
sfDataGrid.MoveToCurrentCell(new RowColumnIndex(3, 3));
```

**VB.NET Example:**
```vb
' Set current item
sfDataGrid.CurrentItem = SelectionHelper.GetRecordAtRowIndex(sfDataGrid, 2)

' Move current cell to specific position
sfDataGrid.MoveToCurrentCell(New RowColumnIndex(3, 3))
```

---

## Getting Selected Items

### Get Selected Rows

**C# Example:**
```csharp
// Get first selected item
var selectedItem = sfDataGrid.SelectedItem;
var selectedIndex = sfDataGrid.SelectedIndex;

// Get all selected items
var selectedItems = sfDataGrid.SelectedItems;
foreach (var item in selectedItems)
{
    var orderInfo = item as OrderInfo;
    Console.WriteLine($"Selected: {orderInfo.OrderID}");
}
```

**VB.NET Example:**
```vb
' Get first selected item
Dim selectedItem = sfDataGrid.SelectedItem
Dim selectedIndex = sfDataGrid.SelectedIndex

' Get all selected items
Dim selectedItems = sfDataGrid.SelectedItems
For Each item In selectedItems
    Dim orderInfo = TryCast(item, OrderInfo)
    Console.WriteLine($"Selected: {orderInfo.OrderID}")
Next item
```

### Get Selected Cells

**C# Example:**
```csharp
// Get selected cells
var selectedCells = sfDataGrid.GetSelectedCells();
foreach (var cellInfo in selectedCells)
{
    Console.WriteLine($"Cell: Row {cellInfo.RowIndex}, Column {cellInfo.ColumnIndex}");
}
```

**VB.NET Example:**
```vb
' Get selected cells
Dim selectedCells = sfDataGrid.GetSelectedCells()
For Each cellInfo In selectedCells
    Console.WriteLine($"Cell: Row {cellInfo.RowIndex}, Column {cellInfo.ColumnIndex}")
Next cellInfo
```

### CurrentItem vs SelectedItem

**C# Example:**
```csharp
// SelectedItem - first selected item
var selected = sfDataGrid.SelectedItem as OrderInfo;

// CurrentItem - item with current focus
var current = sfDataGrid.CurrentItem as OrderInfo;

// In single selection, both are the same
// In multi-selection, they can be different
```

**VB.NET Example:**
```vb
' SelectedItem - first selected item
Dim selected = TryCast(sfDataGrid.SelectedItem, OrderInfo)

' CurrentItem - item with current focus
Dim current = TryCast(sfDataGrid.CurrentItem, OrderInfo)

' In single selection, both are the same
' In multi-selection, they can be different
```

---

## Getting Cell Values

### Get Current Cell Value

**C# Example:**
```csharp
// Get current cell value
if (sfDataGrid.CurrentCell != null)
{
    var currentCellValue = sfDataGrid.CurrentCell.CellRenderer.GetControlValue();
    MessageBox.Show(currentCellValue.ToString(), "Current Cell Value");
}
```

**VB.NET Example:**
```vb
' Get current cell value
If sfDataGrid.CurrentCell IsNot Nothing Then
    Dim currentCellValue = sfDataGrid.CurrentCell.CellRenderer.GetControlValue()
    MessageBox.Show(currentCellValue.ToString(), "Current Cell Value")
End If
```

### Get Current Cell Information

**C# Example:**
```csharp
// Get current cell details
if (sfDataGrid.CurrentCell != null)
{
    var rowIndex = sfDataGrid.CurrentCell.RowIndex;
    var columnIndex = sfDataGrid.CurrentCell.ColumnIndex;
    var column = sfDataGrid.CurrentCell.Column;
    
    Console.WriteLine($"Current Cell: Row {rowIndex}, Column {columnIndex}");
}
```

**VB.NET Example:**
```vb
' Get current cell details
If sfDataGrid.CurrentCell IsNot Nothing Then
    Dim rowIndex = sfDataGrid.CurrentCell.RowIndex
    Dim columnIndex = sfDataGrid.CurrentCell.ColumnIndex
    Dim column = sfDataGrid.CurrentCell.Column
    
    Console.WriteLine($"Current Cell: Row {rowIndex}, Column {columnIndex}")
End If
```

### Get Cell Value by Position

**C# Example:**
```csharp
// Get cell value at specific row and column
string cellValue;
int rowIndex = 5;
int columnIndex = sfDataGrid.TableControl.ResolveToGridVisibleColumnIndex(2);

if (columnIndex < 0)
    return;

var mappingName = sfDataGrid.Columns[columnIndex].MappingName;
var recordIndex = sfDataGrid.TableControl.ResolveToRecordIndex(rowIndex);

if (recordIndex < 0)
    return;

if (sfDataGrid.View.TopLevelGroup != null)
{
    var record = sfDataGrid.View.TopLevelGroup.DisplayElements[recordIndex];
    if (record.IsRecords)
    {
        var data = (record as RecordEntry).Data;
        cellValue = data.GetType().GetProperty(mappingName).GetValue(data, null).ToString();
    }
}
else
{
    var record = sfDataGrid.View.Records.GetItemAt(recordIndex);
    cellValue = record.GetType().GetProperty(mappingName).GetValue(record, null).ToString();
}

MessageBox.Show(cellValue, $"Value at ({rowIndex}, {columnIndex})");
```

**VB.NET Example:**
```vb
' Get cell value at specific row and column
Dim cellValue As String
Dim rowIndex As Integer = 5
Dim columnIndex As Integer = sfDataGrid.TableControl.ResolveToGridVisibleColumnIndex(2)

If columnIndex < 0 Then
    Return
End If

Dim mappingName = sfDataGrid.Columns(columnIndex).MappingName
Dim recordIndex = sfDataGrid.TableControl.ResolveToRecordIndex(rowIndex)

If recordIndex < 0 Then
    Return
End If

If sfDataGrid.View.TopLevelGroup IsNot Nothing Then
    Dim record = sfDataGrid.View.TopLevelGroup.DisplayElements(recordIndex)
    If record.IsRecords Then
        Dim data = (TryCast(record, RecordEntry)).Data
        cellValue = data.GetType().GetProperty(mappingName).GetValue(data, Nothing).ToString()
    End If
Else
    Dim record = sfDataGrid.View.Records.GetItemAt(recordIndex)
    cellValue = record.GetType().GetProperty(mappingName).GetValue(record, Nothing).ToString()
End If

MessageBox.Show(cellValue, $"Value at ({rowIndex}, {columnIndex})")
```

### Get Cell Value on Click

**C# Example:**
```csharp
// Get cell value when clicked
sfDataGrid.CellClick += OnCellClick;

void OnCellClick(object sender, CellClickEventArgs e)
{
    var rowIndex = e.DataRow.RowIndex;
    var columnIndex = e.DataColumn.ColumnIndex;
    var cellValue = sfDataGrid.View.GetPropertyAccessProvider().GetValue(
        e.DataRow.RowData,
        e.DataColumn.GridColumn.MappingName
    );
    
    MessageBox.Show(
        $"Cell Value: {cellValue}\nRow: {rowIndex}\nColumn: {columnIndex}",
        "Cell Value"
    );
}
```

**VB.NET Example:**
```vb
' Get cell value when clicked
AddHandler sfDataGrid.CellClick, AddressOf OnCellClick

Private Sub OnCellClick(sender As Object, e As CellClickEventArgs)
    Dim rowIndex = e.DataRow.RowIndex
    Dim columnIndex = e.DataColumn.ColumnIndex
    Dim cellValue = sfDataGrid.View.GetPropertyAccessProvider().GetValue(
        e.DataRow.RowData,
        e.DataColumn.GridColumn.MappingName
    )
    
    MessageBox.Show(
        $"Cell Value: {cellValue}" & vbLf & $"Row: {rowIndex}" & vbLf & $"Column: {columnIndex}",
        "Cell Value"
    )
End Sub
```

---

## Clearing Selection

### Clear All Selection

**C# Example:**
```csharp
// Method 1: Using ClearSelection
sfDataGrid.ClearSelection();

// Method 2: Set SelectedItem to null
sfDataGrid.SelectedItem = null;

// Method 3: Clear SelectedItems collection
sfDataGrid.SelectedItems.Clear();
```

**VB.NET Example:**
```vb
' Method 1: Using ClearSelection
sfDataGrid.ClearSelection()

' Method 2: Set SelectedItem to nothing
sfDataGrid.SelectedItem = Nothing

' Method 3: Clear SelectedItems collection
sfDataGrid.SelectedItems.Clear()
```

### Unselect Single Cell

**C# Example:**
```csharp
// Unselect a specific cell
var record = sfDataGrid.View.Records[7];
var column = sfDataGrid.Columns[3];
sfDataGrid.UnselectCell(record, column);
```

**VB.NET Example:**
```vb
' Unselect a specific cell
Dim record = sfDataGrid.View.Records(7)
Dim column = sfDataGrid.Columns(3)
sfDataGrid.UnselectCell(record, column)
```

### Unselect Cell Range

**C# Example:**
```csharp
// Unselect range of cells
var firstRecord = sfDataGrid.View.Records[7];
var lastRecord = sfDataGrid.View.Records[8];
var firstColumn = sfDataGrid.Columns[2];
var lastColumn = sfDataGrid.Columns[3];

sfDataGrid.UnselectCells(firstRecord, firstColumn, lastRecord, lastColumn);
```

**VB.NET Example:**
```vb
' Unselect range of cells
Dim firstRecord = sfDataGrid.View.Records(7)
Dim lastRecord = sfDataGrid.View.Records(8)
Dim firstColumn = sfDataGrid.Columns(2)
Dim lastColumn = sfDataGrid.Columns(3)

sfDataGrid.UnselectCells(firstRecord, firstColumn, lastRecord, lastColumn)
```

---

## Keyboard Navigation

### Tab Key Navigation

**C# Example:**
```csharp
// Standard Tab behavior - moves to next control
sfDataGrid.AllowStandardTab = true;

// Default behavior - navigates within grid
sfDataGrid.AllowStandardTab = false;
```

**VB.NET Example:**
```vb
' Standard Tab behavior - moves to next control
sfDataGrid.AllowStandardTab = True

' Default behavior - navigates within grid
sfDataGrid.AllowStandardTab = False
```

---

## Mouse Behavior

### Configure Mouse Selection

**C# Example:**
```csharp
// Enable/disable selection on mouse down
sfDataGrid.AllowSelectionOnMouseDown = true;
```

**VB.NET Example:**
```vb
' Enable/disable selection on mouse down
sfDataGrid.AllowSelectionOnMouseDown = True
```

---

## Selection Events

### CurrentCellActivating Event

**C# Example:**
```csharp
// Cancel current cell movement
sfDataGrid.CurrentCellActivating += OnCurrentCellActivating;

void OnCurrentCellActivating(object sender, CurrentCellActivatingEventArgs e)
{
    // Prevent navigation to specific row
    if ((e.DataRow.RowData as OrderInfo).CustomerID == "FRANS")
    {
        e.Cancel = true;
    }
}
```

**VB.NET Example:**
```vb
' Cancel current cell movement
AddHandler sfDataGrid.CurrentCellActivating, AddressOf OnCurrentCellActivating

Private Sub OnCurrentCellActivating(sender As Object, e As CurrentCellActivatingEventArgs)
    ' Prevent navigation to specific row
    If (TryCast(e.DataRow.RowData, OrderInfo)).CustomerID = "FRANS" Then
        e.Cancel = True
    End If
End Sub
```

### CurrentCellActivated Event

**C# Example:**
```csharp
// Notification when current cell moves
sfDataGrid.CurrentCellActivated += OnCurrentCellActivated;

void OnCurrentCellActivated(object sender, CurrentCellActivatedEventArgs e)
{
    MessageBox.Show(
        $"Current cell moved to ({e.DataRow.RowIndex}, {e.DataColumn.ColumnIndex})"
    );
}
```

**VB.NET Example:**
```vb
' Notification when current cell moves
AddHandler sfDataGrid.CurrentCellActivated, AddressOf OnCurrentCellActivated

Private Sub OnCurrentCellActivated(sender As Object, e As CurrentCellActivatedEventArgs)
    MessageBox.Show(
        $"Current cell moved to ({e.DataRow.RowIndex}, {e.DataColumn.ColumnIndex})"
    )
End Sub
```

### SelectionChanging Event

**C# Example:**
```csharp
// Cancel selection change
sfDataGrid.SelectionChanging += OnSelectionChanging;

void OnSelectionChanging(object sender, SelectionChangingEventArgs e)
{
    // Prevent deselection
    if (e.RemovedItems.Count != 0)
    {
        e.Cancel = true;
    }
}
```

**VB.NET Example:**
```vb
' Cancel selection change
AddHandler sfDataGrid.SelectionChanging, AddressOf OnSelectionChanging

Private Sub OnSelectionChanging(sender As Object, e As SelectionChangingEventArgs)
    ' Prevent deselection
    If e.RemovedItems.Count <> 0 Then
        e.Cancel = True
    End If
End Sub
```

### SelectionChanged Event

**C# Example:**
```csharp
// Notification when selection changes
sfDataGrid.SelectionChanged += OnSelectionChanged;

void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var orderInfo = sfDataGrid.CurrentItem as OrderInfo;
    MessageBox.Show(
        $"\nOrder ID: {orderInfo.OrderID}" +
        $"\nCustomer ID: {orderInfo.CustomerID}" +
        $"\nProduct: {orderInfo.ProductName}",
        "Order Details"
    );
}
```

**VB.NET Example:**
```vb
' Notification when selection changes
AddHandler sfDataGrid.SelectionChanged, AddressOf OnSelectionChanged

Private Sub OnSelectionChanged(sender As Object, e As SelectionChangedEventArgs)
    Dim orderInfo = TryCast(sfDataGrid.CurrentItem, OrderInfo)
    MessageBox.Show(
        vbLf & $"Order ID: {orderInfo.OrderID}" &
        vbLf & $"Customer ID: {orderInfo.CustomerID}" &
        vbLf & $"Product: {orderInfo.ProductName}",
        "Order Details"
    )
End Sub
```

---

## Selection Appearance

### Customize Selection Style

**C# Example:**
```csharp
// Change selection colors
sfDataGrid.Style.SelectionStyle.BackColor = Color.LightSeaGreen;
sfDataGrid.Style.SelectionStyle.TextColor = Color.DarkBlue;
```

**VB.NET Example:**
```vb
' Change selection colors
sfDataGrid.Style.SelectionStyle.BackColor = Color.LightSeaGreen
sfDataGrid.Style.SelectionStyle.TextColor = Color.DarkBlue
```

### Customize Current Cell Style

**C# Example:**
```csharp
// Change current cell appearance
sfDataGrid.Style.CurrentCellStyle.BackColor = SystemColors.Highlight;
sfDataGrid.Style.CurrentCellStyle.TextColor = Color.White;
sfDataGrid.Style.CurrentCellStyle.BorderColor = Color.Red;
sfDataGrid.Style.CurrentCellStyle.BorderThickness = GridBorderWeight.Thick;
```

**VB.NET Example:**
```vb
' Change current cell appearance
sfDataGrid.Style.CurrentCellStyle.BackColor = SystemColors.Highlight
sfDataGrid.Style.CurrentCellStyle.TextColor = Color.White
sfDataGrid.Style.CurrentCellStyle.BorderColor = Color.Red
sfDataGrid.Style.CurrentCellStyle.BorderThickness = GridBorderWeight.Thick
```

---

## Edge Cases and Troubleshooting

### Common Issues

1. **Selection Not Working**
   - Verify `SelectionMode` is not set to `None`
   - Check if `AllowFocus` is true for columns
   - Ensure records are not filtered out

2. **Cannot Select Header or Summary Rows**
   - By design, header rows and table summary rows cannot be selected
   - Unbound rows above/below table summaries also cannot be selected

3. **Current Cell vs Selected Item Confusion**
   - In single selection: both are the same
   - In multi-selection: SelectedItem is first selected, CurrentItem has focus

4. **Keyboard Navigation Not Working**
   - Check `NavigationMode` setting
   - Cannot use `NavigationMode.Row` with cell selection
   - Verify keyboard handlers are not blocked

5. **Tab Key Not Working as Expected**
   - Use `AllowStandardTab` property to control Tab behavior
   - Standard Tab moves to next control, default navigates in grid

6. **Selection Events Not Firing**
   - Ensure event handlers are properly subscribed
   - Check if programmatic changes trigger events differently
   - Verify event is not being canceled elsewhere

### Custom Selection Controllers

**C# Example:**
```csharp
// Custom selection controller for special behavior
sfDataGrid.SelectionController = new CustomSelectionController(sfDataGrid);

public class CustomSelectionController : RowSelectionController
{
    private SfDataGrid sfDataGrid;

    public CustomSelectionController(SfDataGrid dataGrid)
        : base(dataGrid)
    {
        this.sfDataGrid = dataGrid;
    }

    protected override void HandleKeyOperations(KeyEventArgs args)
    {
        // Custom Enter key behavior
        if (args.KeyCode == Keys.Enter)
            return; // Prevent default Enter behavior
        
        // Custom Ctrl+A behavior to select only data rows
        if ((Control.ModifierKeys == Keys.Control) && args.KeyCode == Keys.A)
        {
            base.HandleKeyOperations(args);
            sfDataGrid.SelectedNodeEntries.Clear(); // Remove non-data row selections
            return;
        }

        base.HandleKeyOperations(args);
    }
}
```

**VB.NET Example:**
```vb
' Custom selection controller for special behavior
sfDataGrid.SelectionController = New CustomSelectionController(sfDataGrid)

Public Class CustomSelectionController
    Inherits RowSelectionController
    
    Private sfDataGrid As SfDataGrid

    Public Sub New(dataGrid As SfDataGrid)
        MyBase.New(dataGrid)
        Me.sfDataGrid = dataGrid
    End Sub

    Protected Overrides Sub HandleKeyOperations(args As KeyEventArgs)
        ' Custom Enter key behavior
        If args.KeyCode = Keys.Enter Then
            Return ' Prevent default Enter behavior
        End If
        
        ' Custom Ctrl+A behavior to select only data rows
        If (Control.ModifierKeys = Keys.Control) AndAlso args.KeyCode = Keys.A Then
            MyBase.HandleKeyOperations(args)
            sfDataGrid.SelectedNodeEntries.Clear() ' Remove non-data row selections
            Return
        End If

        MyBase.HandleKeyOperations(args)
    End Sub
End Class
```

### Best Practices

1. **Performance**
   - Use `BeginUpdate()` and `EndUpdate()` when selecting many items programmatically
   - Clear selection before bulk operations if needed

2. **User Experience**
   - Choose appropriate SelectionMode for your scenario
   - Provide visual feedback for selection state
   - Consider using checkbox column for multi-select scenarios

3. **Event Handling**
   - Use SelectionChanging for validation, SelectionChanged for notification
   - Cancel operations in Activating events, not Activated events
   - Be careful with recursive event triggers

4. **Data Binding**
   - Ensure SelectedItem is synchronized with bound properties
   - Use INotifyPropertyChanged for two-way binding scenarios

5. **Accessibility**
   - Support keyboard navigation properly
   - Maintain logical tab order
   - Consider screen reader compatibility
