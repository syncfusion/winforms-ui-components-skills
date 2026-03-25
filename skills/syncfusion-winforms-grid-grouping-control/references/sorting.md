# Sorting in GridGroupingControl

## Overview

GridGroupingControl allows sorting table data by one or more columns with flexible sort direction control. When sorting is applied, the grid rearranges records to match the sort criteria and displays sort icons in column headers.

**Sorting methods:**
- **Click column headers:** Simple one-click sorting
- **Programmatic:** Add columns to SortedColumns collection
- **Multi-column:** Ctrl+click multiple headers
- **Design-time:** Configure in TableDescriptor property grid

## Programmatic Sorting

Add columns to `SortedColumns` collection to sort data:

### Single Column Sorting

```csharp
// Sort by CustomerID (ascending by default)
gridGroupingControl1.TableDescriptor.SortedColumns.Add("CustomerID");
```

![Sorting Example](Sorting_images/Sorting_img2.jpeg)

### Specify Sort Direction

```csharp
using System.ComponentModel;

// Sort descending
gridGroupingControl1.TableDescriptor.SortedColumns.Add("CustomerID", 
    ListSortDirection.Descending);
```

### Sort Nested Tables

```csharp
// Sort child table
gridGroupingControl1.TableDescriptor.Relations[0]
    .ChildTableDescriptor.SortedColumns.Add("UnitPrice", 
        ListSortDirection.Descending);
```

## Removing Sorting

### Remove Specific Sort

```csharp
// By column name
gridGroupingControl1.TableDescriptor.SortedColumns.Remove("ProductName");

// By index
gridGroupingControl1.TableDescriptor.SortedColumns.RemoveAt(2);
```

### Clear All Sorting

```csharp
// Remove all sorted columns
gridGroupingControl1.TableDescriptor.SortedColumns.Clear();
```

## Preventing Sorting

### Disable Column Sorting

```csharp
// User cannot sort CompanyName column
gridGroupingControl1.TableDescriptor.Columns["CompanyName"].AllowSort = false;
```

### Prevent Sorting via Event

```csharp
gridGroupingControl1.TableControlQueryAllowSortColumn += (s, e) =>
{
    if (e.Column.GetName() == "CompanyName")
    {
        e.AllowSort = false;
    }
};
```

### Disable All Sorting

```csharp
// Disable sorting for entire grid
gridGroupingControl1.TableOptions.AllowSortColumns = false;
```

## Sort Icon Placement

Control where sort icons appear in column headers:

```csharp
// Left side
gridGroupingControl1.SortIconPlacement = SortIconPlacement.Left;

// Right side (default)
gridGroupingControl1.SortIconPlacement = SortIconPlacement.Right;

// Top (above header text)
gridGroupingControl1.SortIconPlacement = SortIconPlacement.Top;
```

![Sort Icon at Left](Sorting_images/Sorting_img3.jpeg)

![Sort Icon at Top](Sorting_images/Sorting_img4.jpeg)

## Multi-Column Sorting

Sort by multiple columns with precedence order:

### Interactive Multi-Column Sort

Hold **Ctrl** key and click column headers sequentially. First column has highest precedence.

![Multi-Column Sort](Sorting_images/Sorting_img5.jpeg)

### Programmatic Multi-Column Sort

```csharp
// Sort by multiple columns
gridGroupingControl1.TableDescriptor.SortedColumns.Add("CompanyName");
gridGroupingControl1.TableDescriptor.SortedColumns.Add("ContactName", 
    ListSortDirection.Descending);
gridGroupingControl1.TableDescriptor.SortedColumns.Add("ContactTitle");

// Sort order: CompanyName (Asc) → ContactName (Desc) → ContactTitle (Asc)
```

![Programmatic Multi-Column Sort](Sorting_images/Sorting_img6.jpeg)

### Disable Multi-Column Sorting

```csharp
// Allow only single column sorting
gridGroupingControl1.TableOptions.AllowMultiColumnSort = false;
```

## Sorting Grouped Columns

When grouping is applied, grouped columns display sort icons in GroupDropArea. Click grouped columns to toggle sort direction.

![Sorting Grouped Columns](Sorting_images/Sorting_img7.jpeg)

### Events for Grouped Column Sorting

```csharp
// Before sorting items in a group
gridGroupingControl1.SortingItemsInGroup += (s, e) =>
{
    Group group = e.Group;
    Console.WriteLine($"Sorting group: {group.Info}");
    // e.Cancel = true; // Prevent sorting
};

// After sorting items in a group
gridGroupingControl1.SortedItemsInGroup += (s, e) =>
{
    Group group = e.Group;
    Console.WriteLine($"Group sorted: {group.Info}");
};
```

## Sort by Summary

Sort groups based on summary values instead of field values:

```csharp
// Define summary
GridSummaryColumnDescriptor summaryColumn = new GridSummaryColumnDescriptor(
    "FreightAverage", 
    SummaryType.DoubleAggregate, 
    "Freight", 
    "{Average:###.00}");
    
GridSummaryRowDescriptor summaryRow = new GridSummaryRowDescriptor();
summaryRow.Name = "Caption";
summaryRow.SummaryColumns.Add(summaryColumn);
gridGroupingControl1.TableDescriptor.SummaryRows.Add(summaryRow);

// Create sort descriptor with summary-based order
SortColumnDescriptor columnDescriptor = new SortColumnDescriptor("ShipCountry");

// Sort groups by average Freight value
columnDescriptor.SetGroupSummarySortOrder(
    summaryColumn.GetSummaryDescriptorName(), 
    "Average");
    
gridGroupingControl1.TableDescriptor.GroupedColumns.Clear();
gridGroupingControl1.TableDescriptor.GroupedColumns.Add(columnDescriptor);
```

## Sort by Display Member

Sort foreign-key combo box columns by display text instead of value:

```csharp
// Remember column location
GridTableDescriptor td = gridGroupingControl1.TableDescriptor;
td.VisibleColumns.LoadDefault();
int lookUpIndex = td.VisibleColumns.IndexOf("Customer");

// Add foreign table to SourceListSet
gridGroupingControl1.Engine.SourceListSet.Add(
    "CustomerLookup", 
    customerTable.DefaultView);

// Create foreign-key relation
GridRelationDescriptor rd = new GridRelationDescriptor();
rd.Name = "CustomerDisplay";
rd.RelationKind = RelationKind.ForeignKeyReference;
rd.ChildTableName = "CustomerLookup";

// Configure display
rd.ChildTableDescriptor.VisibleColumns.Add("CustomerName");
rd.ChildTableDescriptor.SortedColumns.Add("CustomerName");

// Add relation
td.Relations.Add(rd);

// Replace value column with display column
string foreignColumn = rd.Name + "_CustomerName";
td.VisibleColumns.Insert(lookUpIndex, foreignColumn);
td.VisibleColumns.Remove("Customer");

// Now sorting Customer column sorts by CustomerName display text
```

## Custom Sorting

Implement custom sort logic using `IComparer`:

```csharp
public class CustomSortComparer : IComparer
{
    private bool isAscending;
    
    public CustomSortComparer(bool isAscendingSortDirection)
    {
        isAscending = isAscendingSortDirection;
    }
    
    public int Compare(object x, object y)
    {
        Record r1 = x as Record;
        Record r2 = y as Record;
        
        // Get field values
        string val1 = r1.GetValue("Description")?.ToString() ?? "";
        string val2 = r2.GetValue("Description")?.ToString() ?? "";
        
        // Empty cells go to bottom
        if (string.IsNullOrEmpty(val1) && !string.IsNullOrEmpty(val2))
            return 1;
        if (!string.IsNullOrEmpty(val1) && string.IsNullOrEmpty(val2))
            return -1;
        if (string.IsNullOrEmpty(val1) && string.IsNullOrEmpty(val2))
            return 0;
        
        // Normal comparison
        int result = string.Compare(val1, val2, StringComparison.OrdinalIgnoreCase);
        return isAscending ? result : -result;
    }
}

// Apply custom comparer
SortColumnDescriptor descriptor = new SortColumnDescriptor("Description");
descriptor.SortDirection = ListSortDirection.Ascending;
descriptor.Comparer = new CustomSortComparer(true);
gridGroupingControl1.TableDescriptor.SortedColumns.Add(descriptor);
```

## Events

Key sorting events:

```csharp
// Before SortedColumns collection changes
gridGroupingControl1.TableDescriptor.SortedColumns.Changing += (s, e) =>
{
    if (e.Action == ListPropertyChangedType.Add)
    {
        SortColumnDescriptor desc = e.Item as SortColumnDescriptor;
        Console.WriteLine($"Adding sort: {desc.Name}");
        // e.Cancel = true; // Prevent sort
    }
};

// After SortedColumns collection changes
gridGroupingControl1.TableDescriptor.SortedColumns.Changed += (s, e) =>
{
    if (e.Action == ListPropertyChangedType.Add)
    {
        Console.WriteLine("Sort applied successfully");
    }
};

// User hovers or clicks column header
gridGroupingControl1.TableControlQueryAllowSortColumn += (s, e) =>
{
    if (e.Column.GetName() == "CompanyName")
    {
        e.AllowSort = false;
    }
};
```

## Common Scenarios

### Scenario 1: Default Sort on Load

```csharp
// Sort by multiple columns when grid loads
private void Form_Load(object sender, EventArgs e)
{
    gridGroupingControl1.TableDescriptor.SortedColumns.Add("Country");
    gridGroupingControl1.TableDescriptor.SortedColumns.Add("City");
    gridGroupingControl1.TableDescriptor.SortedColumns.Add("CompanyName");
}
```

### Scenario 2: Toggle Sort Direction

```csharp
private void ToggleSortDirection(string columnName)
{
    var sortedCols = gridGroupingControl1.TableDescriptor.SortedColumns;
    
    if (sortedCols.Contains(columnName))
    {
        SortColumnDescriptor desc = sortedCols[columnName];
        
        // Toggle direction
        desc.SortDirection = (desc.SortDirection == ListSortDirection.Ascending)
            ? ListSortDirection.Descending
            : ListSortDirection.Ascending;
            
        gridGroupingControl1.Table.Refresh();
    }
    else
    {
        sortedCols.Add(columnName);
    }
}
```

### Scenario 3: Sort by Computed Column

```csharp
// Add expression field
ExpressionFieldDescriptor fullName = new ExpressionFieldDescriptor(
    "FullName",
    "[FirstName] + ' ' + [LastName]",
    "System.String");
gridGroupingControl1.TableDescriptor.ExpressionFields.Add(fullName);

// Sort by computed field
gridGroupingControl1.TableDescriptor.SortedColumns.Add("FullName");
```

### Scenario 4: Case-Insensitive Sorting

```csharp
public class CaseInsensitiveComparer : IComparer
{
    public int Compare(object x, object y)
    {
        Record r1 = x as Record;
        Record r2 = y as Record;
        
        string val1 = r1.GetValue("ProductName")?.ToString() ?? "";
        string val2 = r2.GetValue("ProductName")?.ToString() ?? "";
        
        return string.Compare(val1, val2, StringComparison.OrdinalIgnoreCase);
    }
}

SortColumnDescriptor desc = new SortColumnDescriptor("ProductName");
desc.Comparer = new CaseInsensitiveComparer();
gridGroupingControl1.TableDescriptor.SortedColumns.Add(desc);
```

### Scenario 5: Maintain Sort After Data Refresh

```csharp
// Save current sort order
private List<SortColumnDescriptor> SaveSortOrder()
{
    var savedSorts = new List<SortColumnDescriptor>();
    
    foreach (SortColumnDescriptor desc in gridGroupingControl1.TableDescriptor.SortedColumns)
    {
        savedSorts.Add(new SortColumnDescriptor(desc.Name, desc.SortDirection));
    }
    
    return savedSorts;
}

// Restore sort order after data refresh
private void RestoreSortOrder(List<SortColumnDescriptor> savedSorts)
{
    gridGroupingControl1.TableDescriptor.SortedColumns.Clear();
    
    foreach (SortColumnDescriptor desc in savedSorts)
    {
        gridGroupingControl1.TableDescriptor.SortedColumns.Add(desc);
    }
}

// Usage
List<SortColumnDescriptor> sorts = SaveSortOrder();
RefreshData();
RestoreSortOrder(sorts);
```

## Best Practices

1. **Sort before grouping:** Apply SortedColumns before GroupedColumns for predictable results
2. **Limit multi-column sorts:** 3-4 columns maximum for performance
3. **Use custom comparers:** For complex sort logic (nulls, case-sensitivity, custom orders)
4. **Cache sort order:** Save/restore user's sort preferences
5. **Disable unnecessary sorting:** Set AllowSort = false for ID columns
6. **Clear before re-sorting:** Use SortedColumns.Clear() when changing sort criteria

## Common Issues

### Sort Icon Not Showing

```csharp
// Verify sorting is enabled
gridGroupingControl1.TableOptions.AllowSortColumns = true;

// Check column allows sorting
gridGroupingControl1.TableDescriptor.Columns["ColumnName"].AllowSort = true;
```

### Sort Not Working After Data Change

```csharp
// Refresh after data modification
gridGroupingControl1.Table.Refresh();

// Or invalidate summaries if using sort by summary
gridGroupingControl1.Table.InvalidateSummary();
```

### Multi-Column Sort Not Working

```csharp
// Ensure multi-column sorting is enabled
gridGroupingControl1.TableOptions.AllowMultiColumnSort = true;
```