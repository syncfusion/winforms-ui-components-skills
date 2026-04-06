# Filtering in WinForms DataGrid (SfDataGrid)

## Table of Contents

1. [UI Filtering](#ui-filtering)
2. [Programmatic Filtering](#programmatic-filtering)
3. [Filter Types](#filter-types)
4. [Filter Events](#filter-events)
5. [Edge Cases and Troubleshooting](#edge-cases-and-troubleshooting)

## UI Filtering

### Enable Filtering

**C# Example:**
```csharp
sfDataGrid1.AllowFiltering = true;
```

**VB.NET Example:**
```vb
sfDataGrid1.AllowFiltering = True
```

### Filter Popup Modes

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

### Instant Filtering

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

## Programmatic Filtering

### View Filtering

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

### Column Filtering with FilterPredicates

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

## Filter Types

### String Filters

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

### Numeric Filters

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

### Date Filters

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

### Clear Filters

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

## Filter Events

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

## Edge Cases and Troubleshooting

### Common Issues

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

### Best Practices

- Use appropriate filter behaviors for data types
- Test filtering with various data types
- Consider performance with large datasets
- Use events for custom behaviors
- Validate filter values before applying
- Clear filters when appropriate
- Use programmatic operations for batch changes
