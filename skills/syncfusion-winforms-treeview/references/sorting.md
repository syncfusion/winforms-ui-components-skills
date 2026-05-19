# Sorting

TreeViewAdv supports runtime sorting of nodes alphabetically or using custom logic.

## Basic Sorting

### Sort All Nodes

```csharp
// Sort all nodes alphabetically
treeViewAdv1.Root.Sort();
```

### Sorted Property

The order in which the sort function must be performed can be specified using the SortOrder that holds the values of Ascending or Descending.

Property Table


 TreeNodeAdv Property | Description |
|---------------------|-------------|
| SortOrder | Specifies the order in which the nodes are sorted. Possible values are **Ascending**, **Descending**, or **None**. |
| SortType | Specifies the field or type based on which the nodes are sorted. Nodes will be sorted according to the selected sort type. |


```csharp
treeNode.SortOrder = System.Windows.Forms.SortOrder.Ascending;
treeNode.SortType = Syncfusion.Windows.Forms.Tools.TreeNodeAdvSortType.CheckBox;
```

## Custom Sorting

### Using IComparer

```csharp
public class CustomNodeComparer : IComparer
{
    public int Compare(object x, object y)
    {
        TreeNodeAdv node1 = x as TreeNodeAdv;
        TreeNodeAdv node2 = y as TreeNodeAdv;
        
        // Custom logic (e.g., by tag value)
        return string.Compare(node1.Text, node2.Text);
    }
}

// Compare the nodes by some other field,
TreeNodeAdv9.CompareOptions = System.Globalization.CompareOptions.IgnoreCase;
TreeNodeAdv9.Comparer = Null
```

## Sorting with Data Binding

When data-bound, sort the data source:

```csharp
DataTable dt = (DataTable)treeViewAdv1.DataSource;
DataView dv = dt.DefaultView;
dv.Sort = "Name ASC";

treeViewAdv1.DataSource = dv.ToTable();
```

## Performance Considerations

For large datasets:
- Sort data source before binding
- Use `BeginUpdate()` / `EndUpdate()` during sort

```csharp
treeViewAdv1.BeginUpdate();
try
{
    treeViewAdv1.Root.Sort();
}
finally
{
    treeViewAdv1.EndUpdate();
}
```

## Troubleshooting

**Issue:** Sorting not working
- **Solution:** Call `Sort()` explicitly

**Issue:** Custom sorting ignored
- **Solution:** Set `TreeNodeComparer` before calling `Sort()`
