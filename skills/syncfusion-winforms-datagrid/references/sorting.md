# Sorting in WinForms DataGrid (SfDataGrid)

## Table of Contents

1. [UI Sorting](#ui-sorting)
2. [Programmatic Sorting](#programmatic-sorting)
3. [Custom Sorting](#custom-sorting)
4. [Sort Events](#sort-events)
5. [Edge Cases and Troubleshooting](#edge-cases-and-troubleshooting)

## UI Sorting

### Enable Sorting

**C# Example:**
```csharp
sfDataGrid1.AllowSorting = true;
```

**VB.NET Example:**
```vb
sfDataGrid1.AllowSorting = True
```

### Disable Sorting for Specific Column

**C# Example:**
```csharp
sfDataGrid1.Columns["OrderID"].AllowSorting = false;
```

**VB.NET Example:**
```vb
sfDataGrid1.Columns("OrderID").AllowSorting = False
```

### Sort Click Action

**C# Example:**
```csharp
// Single click to sort (default)
sfDataGrid1.SortClickAction = SortClickAction.SingleClick;

// Double click to sort
sfDataGrid1.SortClickAction = SortClickAction.DoubleClick;
```

**VB.NET Example:**
```vb
' Single click to sort (default)
sfDataGrid1.SortClickAction = SortClickAction.SingleClick

' Double click to sort
sfDataGrid1.SortClickAction = SortClickAction.DoubleClick
```

### Tri-State Sorting

**C# Example:**
```csharp
sfDataGrid1.AllowTriStateSorting = true;
```

**VB.NET Example:**
```vb
sfDataGrid1.AllowTriStateSorting = True
```

## Programmatic Sorting

### Adding Sort Columns

**C# Example:**
```csharp
SortColumnDescription sortColumn = new SortColumnDescription();
sortColumn.ColumnName = "CustomerID";
sortColumn.SortDirection = ListSortDirection.Ascending;
sfDataGrid1.SortColumnDescriptions.Add(sortColumn);
```

**VB.NET Example:**
```vb
Dim sortColumn As New SortColumnDescription()
sortColumn.ColumnName = "CustomerID"
sortColumn.SortDirection = ListSortDirection.Ascending
sfDataGrid1.SortColumnDescriptions.Add(sortColumn)
```

### Multi-Column Sorting

**C# Example:**
```csharp
// Sort by multiple columns
sfDataGrid1.SortColumnDescriptions.Add(new SortColumnDescription()
{
    ColumnName = "ShipCountry",
    SortDirection = ListSortDirection.Ascending
});

sfDataGrid1.SortColumnDescriptions.Add(new SortColumnDescription()
{
    ColumnName = "CustomerID",
    SortDirection = ListSortDirection.Ascending
});

// Show sort numbers
sfDataGrid1.ShowSortNumbers = true;
```

**VB.NET Example:**
```vb
' Sort by multiple columns
sfDataGrid1.SortColumnDescriptions.Add(New SortColumnDescription() With {
    .ColumnName = "ShipCountry",
    .SortDirection = ListSortDirection.Ascending
})

sfDataGrid1.SortColumnDescriptions.Add(New SortColumnDescription() With {
    .ColumnName = "CustomerID",
    .SortDirection = ListSortDirection.Ascending
})

' Show sort numbers
sfDataGrid1.ShowSortNumbers = True
```

### Removing Sort Columns

**C# Example:**
```csharp
// Remove specific sort
var sortDesc = sfDataGrid1.SortColumnDescriptions.FirstOrDefault(
    col => col.ColumnName == "OrderID");
if (sortDesc != null)
{
    sfDataGrid1.SortColumnDescriptions.Remove(sortDesc);
}

// Clear all sorting
sfDataGrid1.SortColumnDescriptions.Clear();
```

**VB.NET Example:**
```vb
' Remove specific sort
Dim sortDesc = sfDataGrid1.SortColumnDescriptions.FirstOrDefault(
    Function(col) col.ColumnName = "OrderID")
If sortDesc IsNot Nothing Then
    sfDataGrid1.SortColumnDescriptions.Remove(sortDesc)
End If

' Clear all sorting
sfDataGrid1.SortColumnDescriptions.Clear()
```

## Custom Sorting

**C# Example:**
```csharp
// Create custom comparer
public class CustomComparer : IComparer<object>, ISortDirection
{
    public int Compare(object x, object y)
    {
        int nameX, nameY;

        if (x.GetType() == typeof(OrderInfo))
        {
            nameX = ((OrderInfo)x).ProductName.Length;
            nameY = ((OrderInfo)y).ProductName.Length;
        }
        else
        {
            nameX = x.ToString().Length;
            nameY = y.ToString().Length;
        }

        if (nameX.CompareTo(nameY) > 0)
            return SortDirection == ListSortDirection.Ascending ? 1 : -1;
        else if (nameX.CompareTo(nameY) == -1)
            return SortDirection == ListSortDirection.Ascending ? -1 : 1;
        else
            return 0;
    }

    public ListSortDirection SortDirection { get; set; }
}

// Add custom comparer
sfDataGrid1.SortComparers.Add(new Syncfusion.Data.SortComparer()
{
    Comparer = new CustomComparer(),
    PropertyName = "ProductName"
});
```

**VB.NET Example:**
```vb
' Create custom comparer
Public Class CustomComparer
    Implements IComparer(Of Object), ISortDirection
    
    Public Function Compare(ByVal x As Object, ByVal y As Object) As Integer _
        Implements IComparer(Of Object).Compare
        Dim nameX, nameY As Integer

        If x.GetType() Is GetType(OrderInfo) Then
            nameX = CType(x, OrderInfo).ProductName.Length
            nameY = CType(y, OrderInfo).ProductName.Length
        Else
            nameX = x.ToString().Length
            nameY = y.ToString().Length
        End If

        If nameX.CompareTo(nameY) > 0 Then
            Return If(SortDirection = ListSortDirection.Ascending, 1, -1)
        ElseIf nameX.CompareTo(nameY) = -1 Then
            Return If(SortDirection = ListSortDirection.Ascending, -1, 1)
        Else
            Return 0
        End If
    End Function

    Public Property SortDirection As ListSortDirection _
        Implements ISortDirection.SortDirection
End Class

' Add custom comparer
sfDataGrid1.SortComparers.Add(New Syncfusion.Data.SortComparer() With {
    .Comparer = New CustomComparer(),
    .PropertyName = "ProductName"
})
```

## Sort Events

**C# Example:**
```csharp
sfDataGrid1.SortColumnsChanging += SfDataGrid1_SortColumnsChanging;
sfDataGrid1.SortColumnsChanged += SfDataGrid1_SortColumnsChanged;

private void SfDataGrid1_SortColumnsChanging(object sender, SortColumnsChangingEventArgs e)
{
    // Cancel sorting for specific column
    if (e.AddedItems[0].ColumnName == "OrderID")
    {
        e.Cancel = true;
    }
    
    // Cancel scrolling after sorting
    e.CancelScroll = true;
}

private void SfDataGrid1_SortColumnsChanged(object sender, SortColumnsChangedEventArgs e)
{
    // Perform actions after sorting
    UpdateSortIndicators();
}
```

**VB.NET Example:**
```vb
AddHandler sfDataGrid1.SortColumnsChanging, AddressOf SfDataGrid1_SortColumnsChanging
AddHandler sfDataGrid1.SortColumnsChanged, AddressOf SfDataGrid1_SortColumnsChanged

Private Sub SfDataGrid1_SortColumnsChanging(ByVal sender As Object, ByVal e As SortColumnsChangingEventArgs)
    ' Cancel sorting for specific column
    If e.AddedItems(0).ColumnName = "OrderID" Then
        e.Cancel = True
    End If
    
    ' Cancel scrolling after sorting
    e.CancelScroll = True
End Sub

Private Sub SfDataGrid1_SortColumnsChanged(ByVal sender As Object, ByVal e As SortColumnsChangedEventArgs)
    ' Perform actions after sorting
    UpdateSortIndicators()
End Sub
```

## Edge Cases and Troubleshooting

### Common Issues

**Issue: Sorting Not Working**
- Verify `AllowSorting` is `true`
- Check column's `AllowSorting` property
- Ensure data type supports comparison
- Verify no custom comparer conflicts

**Issue: Multi-Column Sort Not Working**
- Check `SortClickAction` setting
- Verify Ctrl key is pressed during sort
- Ensure columns support sorting

**Issue: Custom Sort Not Applied**
- Verify comparer implements ISortDirection
- Check PropertyName matches column MappingName
- Test comparer logic separately

### Best Practices

- Implement custom comparers for complex sorting
- Test multi-column operations
- Use events for custom behaviors
- Document custom sorting logic
- Clear sorts when appropriate
- Use programmatic operations for batch changes
