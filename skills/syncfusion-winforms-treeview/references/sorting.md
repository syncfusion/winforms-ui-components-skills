# Sorting

TreeViewAdv supports runtime sorting of nodes alphabetically or using custom logic.

## Basic Sorting

### Sort All Nodes

```csharp
// Sort all nodes alphabetically
treeViewAdv1.Sort();
```

### Sorted Property

Enable automatic sorting:

```csharp
// Auto-sort as nodes are added
treeViewAdv1.Sorted = true;
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

// Apply custom comparer
treeViewAdv1.TreeNodeComparer = new CustomNodeComparer();
treeViewAdv1.Sort();
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
    treeViewAdv1.Sort();
}
finally
{
    treeViewAdv1.EndUpdate();
}
```

## Troubleshooting

**Issue:** Sorting not working
- **Solution:** Ensure `Sorted = true` or call `Sort()` explicitly

**Issue:** Custom sorting ignored
- **Solution:** Set `TreeNodeComparer` before calling `Sort()`
