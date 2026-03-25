# Columns Reference for WinForms DataGrid (SfDataGrid)

## Table of Contents

1. [Column Types](#column-types)
2. [Auto-Generating Columns](#auto-generating-columns)
3. [Manually Defining Columns](#manually-defining-columns)
4. [Column Manipulation](#column-manipulation)
5. [Stacked Headers](#stacked-headers)
6. [Column Drag and Drop](#column-drag-and-drop)
7. [Column Hiding](#column-hiding)
8. [Column Resizing](#column-resizing)
9. [Column Sizing Modes](#column-sizing-modes)
10. [Edge Cases and Troubleshooting](#edge-cases-and-troubleshooting)

## Column Types

### Built-in Column Types

SfDataGrid provides the following built-in column types:

#### GridTextColumn
Displays text data and supports editing.

**C# Example:**
```csharp
GridTextColumn textColumn = new GridTextColumn();
textColumn.MappingName = "CustomerID";
textColumn.HeaderText = "Customer ID";
sfDataGrid1.Columns.Add(textColumn);
```

**VB.NET Example:**
```vb
Dim textColumn As New GridTextColumn()
textColumn.MappingName = "CustomerID"
textColumn.HeaderText = "Customer ID"
sfDataGrid1.Columns.Add(textColumn)
```

#### GridNumericColumn
Displays numeric data with format support.

**C# Example:**
```csharp
GridNumericColumn numericColumn = new GridNumericColumn();
numericColumn.MappingName = "UnitPrice";
numericColumn.HeaderText = "Unit Price";
numericColumn.FormatMode = FormatMode.Currency;
numericColumn.CurrencyDecimalDigits = 2;
sfDataGrid1.Columns.Add(numericColumn);
```

**VB.NET Example:**
```vb
Dim numericColumn As New GridNumericColumn()
numericColumn.MappingName = "UnitPrice"
numericColumn.HeaderText = "Unit Price"
numericColumn.FormatMode = FormatMode.Currency
numericColumn.CurrencyDecimalDigits = 2
sfDataGrid1.Columns.Add(numericColumn)
```

#### GridDateTimeColumn
Displays date and time values.

**C# Example:**
```csharp
GridDateTimeColumn dateColumn = new GridDateTimeColumn();
dateColumn.MappingName = "OrderDate";
dateColumn.HeaderText = "Order Date";
dateColumn.Format = DateTimeFormat.Short;
sfDataGrid1.Columns.Add(dateColumn);
```

**VB.NET Example:**
```vb
Dim dateColumn As New GridDateTimeColumn()
dateColumn.MappingName = "OrderDate"
dateColumn.HeaderText = "Order Date"
dateColumn.Format = DateTimeFormat.Short
sfDataGrid1.Columns.Add(dateColumn)
```

#### GridCheckBoxColumn
Displays boolean data as checkboxes.

**C# Example:**
```csharp
GridCheckBoxColumn checkBoxColumn = new GridCheckBoxColumn();
checkBoxColumn.MappingName = "IsClosed";
checkBoxColumn.HeaderText = "Closed";
sfDataGrid1.Columns.Add(checkBoxColumn);
```

**VB.NET Example:**
```vb
Dim checkBoxColumn As New GridCheckBoxColumn()
checkBoxColumn.MappingName = "IsClosed"
checkBoxColumn.HeaderText = "Closed"
sfDataGrid1.Columns.Add(checkBoxColumn)
```

#### GridComboBoxColumn
Displays data with dropdown selection.

**C# Example:**
```csharp
GridComboBoxColumn comboBoxColumn = new GridComboBoxColumn();
comboBoxColumn.MappingName = "ShipCountry";
comboBoxColumn.HeaderText = "Ship Country";
comboBoxColumn.DataSource = countryList;
sfDataGrid1.Columns.Add(comboBoxColumn);
```

#### GridImageColumn
Displays images in cells.

**C# Example:**
```csharp
GridImageColumn imageColumn = new GridImageColumn();
imageColumn.MappingName = "ProductImage";
imageColumn.HeaderText = "Image";
sfDataGrid1.Columns.Add(imageColumn);
```

#### GridHyperlinkColumn
Displays hyperlinks.

**C# Example:**
```csharp
GridHyperlinkColumn hyperlinkColumn = new GridHyperlinkColumn();
hyperlinkColumn.MappingName = "WebLink";
hyperlinkColumn.HeaderText = "Website";
sfDataGrid1.Columns.Add(hyperlinkColumn);
```

#### GridButtonColumn
Displays buttons in cells.

**C# Example:**
```csharp
GridButtonColumn buttonColumn = new GridButtonColumn();
buttonColumn.MappingName = "Action";
buttonColumn.HeaderText = "Actions";
buttonColumn.Text = "Details";
sfDataGrid1.Columns.Add(buttonColumn);
```

#### GridProgressBarColumn
Displays progress bars.

**C# Example:**
```csharp
GridProgressBarColumn progressColumn = new GridProgressBarColumn();
progressColumn.MappingName = "Progress";
progressColumn.HeaderText = "Completion";
progressColumn.Minimum = 0;
progressColumn.Maximum = 100;
sfDataGrid1.Columns.Add(progressColumn);
```

#### GridMultiSelectComboBoxColumn
Displays multi-select dropdown.

**C# Example:**
```csharp
GridMultiSelectComboBoxColumn multiSelectColumn = new GridMultiSelectComboBoxColumn();
multiSelectColumn.MappingName = "Categories";
multiSelectColumn.HeaderText = "Categories";
multiSelectColumn.DataSource = categoryList;
sfDataGrid1.Columns.Add(multiSelectColumn);
```

#### GridMaskColumn
Displays masked text input.

**C# Example:**
```csharp
GridMaskColumn maskColumn = new GridMaskColumn();
maskColumn.MappingName = "PhoneNumber";
maskColumn.HeaderText = "Phone";
maskColumn.Mask = "(000) 000-0000";
sfDataGrid1.Columns.Add(maskColumn);
```

#### GridCheckBoxSelectorColumn
Displays checkboxes for row selection.

**C# Example:**
```csharp
GridCheckBoxSelectorColumn selectorColumn = new GridCheckBoxSelectorColumn();
selectorColumn.MappingName = "SelectorColumn";
selectorColumn.Width = 40;
sfDataGrid1.Columns.Add(selectorColumn);
```

## Auto-Generating Columns

### Auto-Generation Modes

#### AutoGenerateColumnsMode.None
No columns are auto-generated.

**C# Example:**
```csharp
sfDataGrid1.AutoGenerateColumnsMode = AutoGenerateColumnsMode.None;
```

**VB.NET Example:**
```vb
sfDataGrid1.AutoGenerateColumnsMode = AutoGenerateColumnsMode.None
```

#### AutoGenerateColumnsMode.Reset
Clears existing columns and generates new columns.

**C# Example:**
```csharp
sfDataGrid1.AutoGenerateColumnsMode = AutoGenerateColumnsMode.Reset;
```

#### AutoGenerateColumnsMode.ResetAll
Clears all columns including manually defined ones.

**C# Example:**
```csharp
sfDataGrid1.AutoGenerateColumnsMode = AutoGenerateColumnsMode.ResetAll;
```

#### AutoGenerateColumnsMode.RetainOld
Retains existing columns and adds new columns.

**C# Example:**
```csharp
sfDataGrid1.AutoGenerateColumnsMode = AutoGenerateColumnsMode.RetainOld;
```

#### AutoGenerateColumnsMode.SmartReset
Retains columns that match with data source and creates new columns.

**C# Example:**
```csharp
sfDataGrid1.AutoGenerateColumnsMode = AutoGenerateColumnsMode.SmartReset;
```

### Customizing Auto-Generated Columns

**C# Example:**
```csharp
sfDataGrid1.AutoGeneratingColumn += SfDataGrid1_AutoGeneratingColumn;

private void SfDataGrid1_AutoGeneratingColumn(object sender, AutoGeneratingColumnArgs e)
{
    if (e.Column.MappingName == "OrderID")
    {
        e.Column.HeaderText = "Order Number";
        e.Column.AllowEditing = false;
    }
    
    if (e.Column.MappingName == "UnitPrice")
    {
        e.Column.Format = "C2";
    }
    
    // Cancel column generation
    if (e.Column.MappingName == "InternalID")
    {
        e.Cancel = true;
    }
}
```

**VB.NET Example:**
```vb
AddHandler sfDataGrid1.AutoGeneratingColumn, AddressOf SfDataGrid1_AutoGeneratingColumn

Private Sub SfDataGrid1_AutoGeneratingColumn(ByVal sender As Object, ByVal e As AutoGeneratingColumnArgs)
    If e.Column.MappingName = "OrderID" Then
        e.Column.HeaderText = "Order Number"
        e.Column.AllowEditing = False
    End If
    
    If e.Column.MappingName = "UnitPrice" Then
        e.Column.Format = "C2"
    End If
    
    ' Cancel column generation
    If e.Column.MappingName = "InternalID" Then
        e.Cancel = True
    End If
End Sub
```

### Data Annotations Support

**C# Example:**
```csharp
public class OrderInfo
{
    [Display(Name = "Order ID")]
    public int OrderID { get; set; }
    
    [Display(Name = "Customer ID")]
    public string CustomerID { get; set; }
    
    [Display(AutoGenerateField = false)]
    public string InternalID { get; set; }
}
```

## Manually Defining Columns

### Complete Manual Column Definition

**C# Example:**
```csharp
sfDataGrid1.AutoGenerateColumns = false;

// Text column
GridTextColumn orderIDColumn = new GridTextColumn();
orderIDColumn.MappingName = "OrderID";
orderIDColumn.HeaderText = "Order ID";
orderIDColumn.Width = 100;
sfDataGrid1.Columns.Add(orderIDColumn);

// Numeric column
GridNumericColumn priceColumn = new GridNumericColumn();
priceColumn.MappingName = "UnitPrice";
priceColumn.HeaderText = "Unit Price";
priceColumn.FormatMode = FormatMode.Currency;
priceColumn.CurrencyDecimalDigits = 2;
priceColumn.Width = 120;
sfDataGrid1.Columns.Add(priceColumn);

// Date column
GridDateTimeColumn dateColumn = new GridDateTimeColumn();
dateColumn.MappingName = "OrderDate";
dateColumn.HeaderText = "Order Date";
dateColumn.Format = DateTimeFormat.Short;
dateColumn.Width = 150;
sfDataGrid1.Columns.Add(dateColumn);

// CheckBox column
GridCheckBoxColumn closedColumn = new GridCheckBoxColumn();
closedColumn.MappingName = "IsClosed";
closedColumn.HeaderText = "Closed";
closedColumn.Width = 80;
sfDataGrid1.Columns.Add(closedColumn);
```

**VB.NET Example:**
```vb
sfDataGrid1.AutoGenerateColumns = False

' Text column
Dim orderIDColumn As New GridTextColumn()
orderIDColumn.MappingName = "OrderID"
orderIDColumn.HeaderText = "Order ID"
orderIDColumn.Width = 100
sfDataGrid1.Columns.Add(orderIDColumn)

' Numeric column
Dim priceColumn As New GridNumericColumn()
priceColumn.MappingName = "UnitPrice"
priceColumn.HeaderText = "Unit Price"
priceColumn.FormatMode = FormatMode.Currency
priceColumn.CurrencyDecimalDigits = 2
priceColumn.Width = 120
sfDataGrid1.Columns.Add(priceColumn)

' Date column
Dim dateColumn As New GridDateTimeColumn()
dateColumn.MappingName = "OrderDate"
dateColumn.HeaderText = "Order Date"
dateColumn.Format = DateTimeFormat.Short
dateColumn.Width = 150
sfDataGrid1.Columns.Add(dateColumn)

' CheckBox column
Dim closedColumn As New GridCheckBoxColumn()
closedColumn.MappingName = "IsClosed"
closedColumn.HeaderText = "Closed"
closedColumn.Width = 80
sfDataGrid1.Columns.Add(closedColumn)
```

## Column Manipulation

### Adding Columns

**C# Example:**
```csharp
GridTextColumn newColumn = new GridTextColumn();
newColumn.MappingName = "ProductName";
newColumn.HeaderText = "Product";
sfDataGrid1.Columns.Add(newColumn);
```

### Inserting Columns at Specific Index

**C# Example:**
```csharp
GridTextColumn insertColumn = new GridTextColumn();
insertColumn.MappingName = "ShipCity";
insertColumn.HeaderText = "City";
sfDataGrid1.Columns.Insert(2, insertColumn);
```

### Removing Columns

**C# Example:**
```csharp
// Remove by column name
var column = sfDataGrid1.Columns["OrderID"];
sfDataGrid1.Columns.Remove(column);

// Remove by index
sfDataGrid1.Columns.RemoveAt(0);
```

**VB.NET Example:**
```vb
' Remove by column name
Dim column = sfDataGrid1.Columns("OrderID")
sfDataGrid1.Columns.Remove(column)

' Remove by index
sfDataGrid1.Columns.RemoveAt(0)
```

### Clearing All Columns

**C# Example:**
```csharp
sfDataGrid1.Columns.Clear();
```

### Reordering Columns

**C# Example:**
```csharp
sfDataGrid1.AllowDraggingColumns = true;

// Programmatic reordering
var column = sfDataGrid1.Columns["CustomerID"];
sfDataGrid1.Columns.Remove(column);
sfDataGrid1.Columns.Insert(0, column);
```

**VB.NET Example:**
```vb
sfDataGrid1.AllowDraggingColumns = True

' Programmatic reordering
Dim column = sfDataGrid1.Columns("CustomerID")
sfDataGrid1.Columns.Remove(column)
sfDataGrid1.Columns.Insert(0, column)
```

## Stacked Headers

### Creating Stacked Headers

**C# Example:**
```csharp
// Create stacked header rows
GridStackedHeaderRow stackedRow1 = new GridStackedHeaderRow();

// Create stacked column spans
GridStackedHeaderColumn stacked1 = new GridStackedHeaderColumn();
stacked1.ChildColumns = "OrderID,CustomerID,ProductName";
stacked1.HeaderText = "Order Details";
stacked1.MappingName = "OrderDetails";

GridStackedHeaderColumn stacked2 = new GridStackedHeaderColumn();
stacked2.ChildColumns = "ShipCity,ShipCountry";
stacked2.HeaderText = "Shipping Information";
stacked2.MappingName = "ShippingInfo";

// Add stacked columns to row
stackedRow1.StackedHeaderColumns.Add(stacked1);
stackedRow1.StackedHeaderColumns.Add(stacked2);

// Add row to grid
sfDataGrid1.StackedHeaderRows.Add(stackedRow1);
```

**VB.NET Example:**
```vb
' Create stacked header rows
Dim stackedRow1 As New GridStackedHeaderRow()

' Create stacked column spans
Dim stacked1 As New GridStackedHeaderColumn()
stacked1.ChildColumns = "OrderID,CustomerID,ProductName"
stacked1.HeaderText = "Order Details"
stacked1.MappingName = "OrderDetails"

Dim stacked2 As New GridStackedHeaderColumn()
stacked2.ChildColumns = "ShipCity,ShipCountry"
stacked2.HeaderText = "Shipping Information"
stacked2.MappingName = "ShippingInfo"

' Add stacked columns to row
stackedRow1.StackedHeaderColumns.Add(stacked1)
stackedRow1.StackedHeaderColumns.Add(stacked2)

' Add row to grid
sfDataGrid1.StackedHeaderRows.Add(stackedRow1)
```

### Multi-Level Stacked Headers

**C# Example:**
```csharp
// First level
GridStackedHeaderRow stackedRow1 = new GridStackedHeaderRow();
GridStackedHeaderColumn level1Column = new GridStackedHeaderColumn();
level1Column.ChildColumns = "OrderID,CustomerID,ProductName,UnitPrice";
level1Column.HeaderText = "Order Information";
stackedRow1.StackedHeaderColumns.Add(level1Column);

// Second level
GridStackedHeaderRow stackedRow2 = new GridStackedHeaderRow();

GridStackedHeaderColumn level2Column1 = new GridStackedHeaderColumn();
level2Column1.ChildColumns = "OrderID,CustomerID";
level2Column1.HeaderText = "Order Details";

GridStackedHeaderColumn level2Column2 = new GridStackedHeaderColumn();
level2Column2.ChildColumns = "ProductName,UnitPrice";
level2Column2.HeaderText = "Product Details";

stackedRow2.StackedHeaderColumns.Add(level2Column1);
stackedRow2.StackedHeaderColumns.Add(level2Column2);

sfDataGrid1.StackedHeaderRows.Add(stackedRow1);
sfDataGrid1.StackedHeaderRows.Add(stackedRow2);
```

## Column Drag and Drop

### Enabling Column Drag and Drop

**C# Example:**
```csharp
sfDataGrid1.AllowDraggingColumns = true;
```

**VB.NET Example:**
```vb
sfDataGrid1.AllowDraggingColumns = True
```

### Disabling Drag for Specific Columns

**C# Example:**
```csharp
sfDataGrid1.Columns["OrderID"].AllowDragging = false;
```

**VB.NET Example:**
```vb
sfDataGrid1.Columns("OrderID").AllowDragging = False
```

## Column Hiding

### Hiding Columns

**C# Example:**
```csharp
sfDataGrid1.Columns["InternalID"].Visible = false;
```

**VB.NET Example:**
```vb
sfDataGrid1.Columns("InternalID").Visible = False
```

### Programmatically Showing Hidden Columns

**C# Example:**
```csharp
sfDataGrid1.Columns["InternalID"].Visible = true;
```

## Column Resizing

### Enabling Column Resizing

**C# Example:**
```csharp
sfDataGrid1.AllowResizingColumns = true;
```

**VB.NET Example:**
```vb
sfDataGrid1.AllowResizingColumns = True
```

### Disabling Resize for Specific Columns

**C# Example:**
```csharp
sfDataGrid1.Columns["OrderID"].AllowResizing = false;
```

**VB.NET Example:**
```vb
sfDataGrid1.Columns("OrderID").AllowResizing = False
```

### Setting Column Width

**C# Example:**
```csharp
sfDataGrid1.Columns["CustomerID"].Width = 150;
```

### Hidden Column Resizing

**C# Example:**
```csharp
sfDataGrid1.AllowResizingHiddenColumns = true;
```

## Column Sizing Modes

### AllCells
Calculates width based on all cells.

**C# Example:**
```csharp
sfDataGrid1.Columns["ProductName"].AutoSizeColumnsMode = AutoSizeColumnsMode.AllCells;
```

### AllCellsExceptHeader
Calculates width based on cells excluding header.

**C# Example:**
```csharp
sfDataGrid1.Columns["ProductName"].AutoSizeColumnsMode = AutoSizeColumnsMode.AllCellsExceptHeader;
```

### Fill
Fills available space proportionally.

**C# Example:**
```csharp
sfDataGrid1.Columns["ProductName"].AutoSizeColumnsMode = AutoSizeColumnsMode.Fill;
```

### LastColumnFill
Last column fills remaining space.

**C# Example:**
```csharp
sfDataGrid1.AutoSizeColumnsMode = AutoSizeColumnsMode.LastColumnFill;
```

### None
No auto-sizing applied.

**C# Example:**
```csharp
sfDataGrid1.Columns["OrderID"].AutoSizeColumnsMode = AutoSizeColumnsMode.None;
```

### ColumnHeader
Calculates width based on header only.

**C# Example:**
```csharp
sfDataGrid1.Columns["ProductName"].AutoSizeColumnsMode = AutoSizeColumnsMode.ColumnHeader;
```

## Edge Cases and Troubleshooting

### Issue: Columns Not Auto-Generated
**Solution:**
- Ensure `AutoGenerateColumns` is set to `true`
- Verify data source is properly bound
- Check that data source properties are public

### Issue: Column Reordering Not Working
**Solution:**
- Verify `AllowDraggingColumns` is `true`
- Check individual column's `AllowDragging` property
- Ensure columns are not frozen

### Issue: Column Width Not Applying
**Solution:**
- Check if `AutoSizeColumnsMode` is overriding width
- Verify minimum width constraints
- Ensure width value is valid (positive number)

### Issue: Stacked Headers Not Displaying
**Solution:**
- Verify `ChildColumns` property contains valid column names
- Ensure all specified columns exist
- Check that column names match exactly (case-sensitive)

### Issue: Hidden Columns Still Visible
**Solution:**
- Explicitly set `Visible = false`
- Refresh the grid after hiding columns
- Check if column is being re-added during auto-generation

### Best Practices
- Always disable auto-generation when manually defining columns
- Use meaningful `MappingName` and `HeaderText` values
- Set appropriate column widths for better user experience
- Handle `AutoGeneratingColumn` event for consistent customization
- Test column operations with different data sources
- Consider performance when using complex stacked headers
