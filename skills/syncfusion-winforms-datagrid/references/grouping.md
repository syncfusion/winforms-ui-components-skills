# Grouping in WinForms DataGrid (SfDataGrid)

## Table of Contents

1. [UI Grouping](#ui-grouping)
2. [Programmatic Grouping](#programmatic-grouping)
3. [Group Caption Formatting](#group-caption-formatting)
4. [Expanding/Collapsing Groups](#expandingcollapsing-groups)
5. [Custom Grouping](#custom-grouping)
6. [Group Events](#group-events)
7. [Edge Cases and Troubleshooting](#edge-cases-and-troubleshooting)

## UI Grouping

### Enable Grouping

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

### Disable Grouping for Specific Column

**C# Example:**
```csharp
sfDataGrid1.Columns["OrderID"].AllowGrouping = false;
```

**VB.NET Example:**
```vb
sfDataGrid1.Columns("OrderID").AllowGrouping = False
```

## Programmatic Grouping

### Adding Group Columns

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

### Multi-Level Grouping

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

### Removing Group Columns

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

## Group Caption Formatting

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

## Expanding/Collapsing Groups

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

## Custom Grouping

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

## Group Events

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

### Common Issues

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

- Test multi-level grouping operations
- Use events for custom behaviors
- Document custom grouping logic
- Clear groups when appropriate
- Use programmatic operations for batch changes
- Consider performance with large datasets
