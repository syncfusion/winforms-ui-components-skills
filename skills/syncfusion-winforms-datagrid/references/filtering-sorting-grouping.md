# Filtering, Sorting, and Grouping Reference for WinForms DataGrid (SfDataGrid)

## Table of Contents

1. [Filtering](#filtering)
   - [UI Filtering](#ui-filtering)
   - [Programmatic Filtering](#programmatic-filtering)
   - [Filter Types](#filter-types)
2. [Sorting](#sorting)
   - [UI Sorting](#ui-sorting)
   - [Programmatic Sorting](#programmatic-sorting)
   - [Custom Sorting](#custom-sorting)
3. [Grouping](#grouping)
   - [UI Grouping](#ui-grouping)
   - [Programmatic Grouping](#programmatic-grouping)
   - [Custom Grouping](#custom-grouping)
4. [Edge Cases and Troubleshooting](#edge-cases-and-troubleshooting)

## Filtering

### UI Filtering

#### Enable Filtering

**C# Example:**
```csharp
sfDataGrid1.AllowFiltering = true;
```

**VB.NET Example:**
```vb
sfDataGrid1.AllowFiltering = True
```

#### Filter Popup Modes

**CheckBox Filter:**
```csharp
sfDataGrid1.FilterPopupMode = FilterPopupMode.CheckboxFilter;
```

**Advanced Filter:**
```csharp
sfDataGrid1.FilterPopupMode = FilterPopupMode.AdvancedFilter;
```

**Both Modes:**
```csharp
sfDataGrid1.FilterPopupMode = FilterPopupMode.Both;
```

**VB.NET Example:**
```vb
sfDataGrid1.FilterPopupMode = FilterPopupMode.CheckboxFilter
```

#### Instant Filtering

**C# Example:**
```csharp
sfDataGrid1.AllowBlankFilters = false;
sfDataGrid1.ImmediateUpdateColumnFilter = true;
```

**VB.NET Example:**
```vb
sfDataGrid1.AllowBlankFilters = False
sfDataGrid1.ImmediateUpdateColumnFilter = True
```

### Programmatic Filtering

#### View Filtering

**C# Example:**
```csharp
sfDataGrid1.View.Filter = FilterRecord;
sfDataGrid1.View.RefreshFilter();

private bool FilterRecord(object data)
{
    var record = data as OrderInfo;
    if (record != null)
    {
        return record.UnitPrice > 100;
    }
    return false;
}
```

**VB.NET Example:**
```vb
sfDataGrid1.View.Filter = AddressOf FilterRecord
sfDataGrid1.View.RefreshFilter()

Private Function FilterRecord(ByVal data As Object) As Boolean
    Dim record = TryCast(data, OrderInfo)
    If record IsNot Nothing Then
        Return record.UnitPrice > 100
    End If
    Return False
End Function
```

#### Column Filtering with FilterPredicates

**C# Example:**
```csharp
// Single condition
sfDataGrid1.Columns["UnitPrice"].FilterPredicates.Add(new FilterPredicate()
{
    FilterType = FilterType.GreaterThan,
    FilterValue = 100,
    FilterBehavior = FilterBehavior.StronglyTyped,
    PredicateType = PredicateType.And
});

// Multiple conditions
sfDataGrid1.Columns["OrderDate"].FilterPredicates.Add(new FilterPredicate()
{
    FilterType = FilterType.GreaterThanOrEqual,
    FilterValue = new DateTime(2023, 1, 1),
    FilterBehavior = FilterBehavior.StronglyTyped,
    PredicateType = PredicateType.And
});

sfDataGrid1.Columns["OrderDate"].FilterPredicates.Add(new FilterPredicate()
{
    FilterType = FilterType.LessThanOrEqual,
    FilterValue = new DateTime(2023, 12, 31),
    FilterBehavior = FilterBehavior.StronglyTyped,
    PredicateType = PredicateType.And
});
```

**VB.NET Example:**
```vb
' Single condition
sfDataGrid1.Columns("UnitPrice").FilterPredicates.Add(New FilterPredicate() With {
    .FilterType = FilterType.GreaterThan,
    .FilterValue = 100,
    .FilterBehavior = FilterBehavior.StronglyTyped,
    .PredicateType = PredicateType.And
})

' Multiple conditions
sfDataGrid1.Columns("OrderDate").FilterPredicates.Add(New FilterPredicate() With {
    .FilterType = FilterType.GreaterThanOrEqual,
    .FilterValue = New DateTime(2023, 1, 1),
    .FilterBehavior = FilterBehavior.StronglyTyped,
    .PredicateType = PredicateType.And
})

sfDataGrid1.Columns("OrderDate").FilterPredicates.Add(New FilterPredicate() With {
    .FilterType = FilterType.LessThanOrEqual,
    .FilterValue = New DateTime(2023, 12, 31),
    .FilterBehavior = FilterBehavior.StronglyTyped,
    .PredicateType = PredicateType.And
})
```

### Filter Types

#### String Filters

**C# Example:**
```csharp
// Contains
sfDataGrid1.Columns["CustomerID"].FilterPredicates.Add(new FilterPredicate()
{
    FilterType = FilterType.Contains,
    FilterValue = "ALFKI",
    FilterBehavior = FilterBehavior.StringTyped
});

// StartsWith
sfDataGrid1.Columns["ProductName"].FilterPredicates.Add(new FilterPredicate()
{
    FilterType = FilterType.StartsWith,
    FilterValue = "Ch",
    FilterBehavior = FilterBehavior.StringTyped
});

// EndsWith
sfDataGrid1.Columns["CustomerID"].FilterPredicates.Add(new FilterPredicate()
{
    FilterType = FilterType.EndsWith,
    FilterValue = "EP",
    FilterBehavior = FilterBehavior.StringTyped
});

// Equals
sfDataGrid1.Columns["ShipCountry"].FilterPredicates.Add(new FilterPredicate()
{
    FilterType = FilterType.Equals,
    FilterValue = "USA",
    FilterBehavior = FilterBehavior.StringTyped
});
```

#### Numeric Filters

**C# Example:**
```csharp
// Equals
sfDataGrid1.Columns["Quantity"].FilterPredicates.Add(new FilterPredicate()
{
    FilterType = FilterType.Equals,
    FilterValue = 10,
    FilterBehavior = FilterBehavior.StronglyTyped
});

// LessThan
sfDataGrid1.Columns["UnitPrice"].FilterPredicates.Add(new FilterPredicate()
{
    FilterType = FilterType.LessThan,
    FilterValue = 50,
    FilterBehavior = FilterBehavior.StronglyTyped
});

// GreaterThanOrEqual
sfDataGrid1.Columns["Quantity"].FilterPredicates.Add(new FilterPredicate()
{
    FilterType = FilterType.GreaterThanOrEqual,
    FilterValue = 5,
    FilterBehavior = FilterBehavior.StronglyTyped
});

// Between (using multiple predicates)
sfDataGrid1.Columns["UnitPrice"].FilterPredicates.Add(new FilterPredicate()
{
    FilterType = FilterType.GreaterThanOrEqual,
    FilterValue = 10,
    FilterBehavior = FilterBehavior.StronglyTyped,
    PredicateType = PredicateType.And
});

sfDataGrid1.Columns["UnitPrice"].FilterPredicates.Add(new FilterPredicate()
{
    FilterType = FilterType.LessThanOrEqual,
    FilterValue = 100,
    FilterBehavior = FilterBehavior.StronglyTyped,
    PredicateType = PredicateType.And
});
```

#### Date Filters

**C# Example:**
```csharp
// Specific date
sfDataGrid1.Columns["OrderDate"].FilterPredicates.Add(new FilterPredicate()
{
    FilterType = FilterType.Equals,
    FilterValue = new DateTime(2023, 5, 15),
    FilterBehavior = FilterBehavior.StronglyTyped
});

// Date range
sfDataGrid1.Columns["OrderDate"].FilterPredicates.Add(new FilterPredicate()
{
    FilterType = FilterType.GreaterThanOrEqual,
    FilterValue = new DateTime(2023, 1, 1),
    FilterBehavior = FilterBehavior.StronglyTyped,
    PredicateType = PredicateType.And
});

sfDataGrid1.Columns["OrderDate"].FilterPredicates.Add(new FilterPredicate()
{
    FilterType = FilterType.LessThan,
    FilterValue = new DateTime(2024, 1, 1),
    FilterBehavior = FilterBehavior.StronglyTyped,
    PredicateType = PredicateType.And
});
```

#### Clear Filters

**C# Example:**
```csharp
// Clear specific column filter
sfDataGrid1.Columns["CustomerID"].FilterPredicates.Clear();

// Clear all filters
sfDataGrid1.ClearFilters();
```

**VB.NET Example:**
```vb
' Clear specific column filter
sfDataGrid1.Columns("CustomerID").FilterPredicates.Clear()

' Clear all filters
sfDataGrid1.ClearFilters()
```

### Filter Events

**C# Example:**
```csharp
sfDataGrid1.FilterChanging += SfDataGrid1_FilterChanging;
sfDataGrid1.FilterChanged += SfDataGrid1_FilterChanged;

private void SfDataGrid1_FilterChanging(object sender, FilterChangingEventArgs e)
{
    // Cancel filtering for specific column
    if (e.Column.MappingName == "OrderID")
    {
        e.Cancel = true;
    }
}

private void SfDataGrid1_FilterChanged(object sender, FilterChangedEventArgs e)
{
    // Perform actions after filtering
    UpdateRecordCount();
}
```

**VB.NET Example:**
```vb
AddHandler sfDataGrid1.FilterChanging, AddressOf SfDataGrid1_FilterChanging
AddHandler sfDataGrid1.FilterChanged, AddressOf SfDataGrid1_FilterChanged

Private Sub SfDataGrid1_FilterChanging(ByVal sender As Object, ByVal e As FilterChangingEventArgs)
    ' Cancel filtering for specific column
    If e.Column.MappingName = "OrderID" Then
        e.Cancel = True
    End If
End Sub

Private Sub SfDataGrid1_FilterChanged(ByVal sender As Object, ByVal e As FilterChangedEventArgs)
    ' Perform actions after filtering
    UpdateRecordCount()
End Sub
```

## Sorting

### UI Sorting

#### Enable Sorting

**C# Example:**
```csharp
sfDataGrid1.AllowSorting = true;
```

**VB.NET Example:**
```vb
sfDataGrid1.AllowSorting = True
```

#### Disable Sorting for Specific Column

**C# Example:**
```csharp
sfDataGrid1.Columns["OrderID"].AllowSorting = false;
```

**VB.NET Example:**
```vb
sfDataGrid1.Columns("OrderID").AllowSorting = False
```

#### Sort Click Action

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

#### Tri-State Sorting

**C# Example:**
```csharp
sfDataGrid1.AllowTriStateSorting = true;
```

**VB.NET Example:**
```vb
sfDataGrid1.AllowTriStateSorting = True
```

### Programmatic Sorting

#### Adding Sort Columns

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

#### Multi-Column Sorting

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

#### Removing Sort Columns

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

### Custom Sorting

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

### Sort Events

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

## Grouping

### UI Grouping

#### Enable Grouping

**C# Example:**
```csharp
sfDataGrid1.AllowGrouping = true;
sfDataGrid1.ShowGroupDropArea = true;
```

**VB.NET Example:**
```vb
sfDataGrid1.AllowGrouping = True
sfDataGrid1.ShowGroupDropArea = True
```

#### Disable Grouping for Specific Column

**C# Example:**
```csharp
sfDataGrid1.Columns["OrderID"].AllowGrouping = false;
```

**VB.NET Example:**
```vb
sfDataGrid1.Columns("OrderID").AllowGrouping = False
```

### Programmatic Grouping

#### Adding Group Columns

**C# Example:**
```csharp
sfDataGrid1.GroupColumnDescriptions.Add(new GroupColumnDescription()
{
    ColumnName = "ShipCountry"
});
```

**VB.NET Example:**
```vb
sfDataGrid1.GroupColumnDescriptions.Add(New GroupColumnDescription() With {
    .ColumnName = "ShipCountry"
})
```

#### Multi-Level Grouping

**C# Example:**
```csharp
// Group by Country, then by City
sfDataGrid1.GroupColumnDescriptions.Add(new GroupColumnDescription()
{
    ColumnName = "ShipCountry"
});

sfDataGrid1.GroupColumnDescriptions.Add(new GroupColumnDescription()
{
    ColumnName = "ShipCity"
});
```

**VB.NET Example:**
```vb
' Group by Country, then by City
sfDataGrid1.GroupColumnDescriptions.Add(New GroupColumnDescription() With {
    .ColumnName = "ShipCountry"
})

sfDataGrid1.GroupColumnDescriptions.Add(New GroupColumnDescription() With {
    .ColumnName = "ShipCity"
})
```

#### Removing Group Columns

**C# Example:**
```csharp
// Remove specific group
sfDataGrid1.GroupColumnDescriptions.Remove(
    new GroupColumnDescription() { ColumnName = "ShipCountry" });

// Clear all groups
sfDataGrid1.GroupColumnDescriptions.Clear();
```

**VB.NET Example:**
```vb
' Remove specific group
sfDataGrid1.GroupColumnDescriptions.Remove(
    New GroupColumnDescription() With { .ColumnName = "ShipCountry" })

' Clear all groups
sfDataGrid1.GroupColumnDescriptions.Clear()
```

### Group Caption Formatting

**C# Example:**
```csharp
// Default format: {ColumnName}: {Key} - {ItemsCount} Items
sfDataGrid1.GroupCaptionTextFormat = "{Key} : {ItemsCount}";
```

**VB.NET Example:**
```vb
' Default format: {ColumnName}: {Key} - {ItemsCount} Items
sfDataGrid1.GroupCaptionTextFormat = "{Key} : {ItemsCount}"
```

### Expanding/Collapsing Groups

**C# Example:**
```csharp
// Auto expand groups
sfDataGrid1.View.AutoExpandGroups = true;

// Expand all groups
sfDataGrid1.ExpandAllGroup();

// Collapse all groups
sfDataGrid1.CollapseAllGroup();

// Expand/Collapse specific level
sfDataGrid1.ExpandGroupsAtLevel(1);
sfDataGrid1.CollapseGroupsAtLevel(1);

// Expand/Collapse specific group
sfDataGrid1.ExpandGroup(sfDataGrid1.View.TopLevelGroup);
sfDataGrid1.CollapseGroup(sfDataGrid1.View.TopLevelGroup);
```

**VB.NET Example:**
```vb
' Auto expand groups
sfDataGrid1.View.AutoExpandGroups = True

' Expand all groups
sfDataGrid1.ExpandAllGroup()

' Collapse all groups
sfDataGrid1.CollapseAllGroup()

' Expand/Collapse specific level
sfDataGrid1.ExpandGroupsAtLevel(1)
sfDataGrid1.CollapseGroupsAtLevel(1)

' Expand/Collapse specific group
sfDataGrid1.ExpandGroup(sfDataGrid1.View.TopLevelGroup)
sfDataGrid1.CollapseGroup(sfDataGrid1.View.TopLevelGroup)
```

### Custom Grouping

**C# Example:**
```csharp
sfDataGrid1.GroupColumnDescriptions.Add(new GroupColumnDescription()
{
    ColumnName = "OrderDate",
    KeySelector = (string columnName, object o) =>
    {
        var dt = DateTime.Now;
        var item = (o as OrderInfo).OrderDate;
        var days = (int)Math.Floor((dt - item).TotalDays);
        
        if (days <= 7)
            return "LAST WEEK";
        if (days <= 14)
            return "TWO WEEKS AGO";
        if (days <= 30)
            return "LAST MONTH";
        return "OLDER";
    },
    SortGroupRecords = true
});
```

**VB.NET Example:**
```vb
sfDataGrid1.GroupColumnDescriptions.Add(New GroupColumnDescription() With {
    .ColumnName = "OrderDate",
    .KeySelector = Function(columnName As String, o As Object)
        Dim dt = DateTime.Now
        Dim item = CType(o, OrderInfo).OrderDate
        Dim days = CInt(Math.Floor((dt - item).TotalDays))
        
        If days <= 7 Then
            Return "LAST WEEK"
        End If
        If days <= 14 Then
            Return "TWO WEEKS AGO"
        End If
        If days <= 30 Then
            Return "LAST MONTH"
        End If
        Return "OLDER"
    End Function,
    .SortGroupRecords = True
})
```

### Group Events

**C# Example:**
```csharp
sfDataGrid1.GroupExpanding += SfDataGrid1_GroupExpanding;
sfDataGrid1.GroupCollapsing += SfDataGrid1_GroupCollapsing;
sfDataGrid1.GroupExpanded += SfDataGrid1_GroupExpanded;
sfDataGrid1.GroupCollapsed += SfDataGrid1_GroupCollapsed;

private void SfDataGrid1_GroupExpanding(object sender, GroupChangingEventArgs e)
{
    // Cancel expanding for specific group
    if (e.Group.Key.Equals("USA"))
    {
        e.Cancel = true;
    }
}

private void SfDataGrid1_GroupCollapsing(object sender, GroupChangingEventArgs e)
{
    // Cancel collapsing
    if (e.Group.Key.Equals("UK"))
    {
        e.Cancel = true;
    }
}

private void SfDataGrid1_GroupExpanded(object sender, GroupChangedEventArgs e)
{
    // Perform actions after group expanded
}

private void SfDataGrid1_GroupCollapsed(object sender, GroupChangedEventArgs e)
{
    // Perform actions after group collapsed
}
```

**VB.NET Example:**
```vb
AddHandler sfDataGrid1.GroupExpanding, AddressOf SfDataGrid1_GroupExpanding
AddHandler sfDataGrid1.GroupCollapsing, AddressOf SfDataGrid1_GroupCollapsing
AddHandler sfDataGrid1.GroupExpanded, AddressOf SfDataGrid1_GroupExpanded
AddHandler sfDataGrid1.GroupCollapsed, AddressOf SfDataGrid1_GroupCollapsed

Private Sub SfDataGrid1_GroupExpanding(ByVal sender As Object, ByVal e As GroupChangingEventArgs)
    ' Cancel expanding for specific group
    If e.Group.Key.Equals("USA") Then
        e.Cancel = True
    End If
End Sub

Private Sub SfDataGrid1_GroupCollapsing(ByVal sender As Object, ByVal e As GroupChangingEventArgs)
    ' Cancel collapsing
    If e.Group.Key.Equals("UK") Then
        e.Cancel = True
    End If
End Sub

Private Sub SfDataGrid1_GroupExpanded(ByVal sender As Object, ByVal e As GroupChangedEventArgs)
    ' Perform actions after group expanded
End Sub

Private Sub SfDataGrid1_GroupCollapsed(ByVal sender As Object, ByVal e As GroupChangedEventArgs)
    ' Perform actions after group collapsed
End Sub
```

## Edge Cases and Troubleshooting

### Filtering Issues

**Issue: Filter Not Working**
- Verify `AllowFiltering` is `true`
- Check data types match filter values
- Ensure data source supports filtering
- Verify `FilterBehavior` setting

**Issue: Checkbox Filter Empty**
- Check for null/blank values in data
- Set `AllowBlankFilters` appropriately
- Verify data binding is correct

**Issue: Filter Performance**
- Use `FilterBehavior.StronglyTyped` for better performance
- Consider View filtering for complex logic
- Limit number of active filters

### Sorting Issues

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

### Grouping Issues

**Issue: Grouping Not Working**
- Verify `AllowGrouping` is `true`
- Check `ShowGroupDropArea` is `true`
- Ensure column supports grouping
- Verify data binding is correct

**Issue: Groups Not Expanding**
- Check `AutoExpandGroups` setting
- Verify expand/collapse methods are called
- Test with simple grouping first

**Issue: Custom Grouping Not Working**
- Verify KeySelector logic
- Check return values are consistent
- Test with default grouping first

### Best Practices

- Use appropriate filter behaviors for data types
- Implement custom comparers for complex sorting
- Test filtering with various data types
- Consider performance with large datasets
- Use events for custom behaviors
- Document custom sorting/grouping logic
- Test multi-column operations
- Validate filter values before applying
- Clear filters/sorts when appropriate
- Use programmatic operations for batch changes
