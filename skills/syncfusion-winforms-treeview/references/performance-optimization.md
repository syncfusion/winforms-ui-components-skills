# Performance Optimization

TreeViewAdv provides several optimization features for handling large datasets with thousands of nodes.

## EnableVirtualization

Virtualization dramatically improves loading time for large trees by rendering only visible nodes.

**Without Virtualization:**
- 20,000 nodes = ~60 seconds load time

**With Virtualization:**
- 20,000 nodes = Minimal delay
- Nodes rendered on-demand during scrolling

```csharp
// Enable virtualization
treeViewAdv1.EnableVirtualization = true;
```

### How It Works

- Only visible nodes are rendered
- Parent nodes added during vertical scroll
- Child nodes added on expand
- Significant memory savings

## SuspendExpandRecalculate

Improves performance by reducing dimension recalculations during expand/collapse operations.

**Performance Impact:**
- Without: 5000 child nodes = 10 milliseconds
- With: 5000 child nodes = ~5 milliseconds (50% faster)

```csharp
// Optimize expand/collapse
treeViewAdv1.SuspendExpandRecalculate = true;
```

**Benefit:** Reduces unnecessary recalculate calls when root nodes are collapsed.

## RecalculateExpansion

Controls whether node dimensions are calculated on load.

```csharp
// Disable for faster loading (default: true)
treeViewAdv1.RecalculateExpansion = false;
```

**Use When:**
- Initial load performance critical
- Dimensions can be calculated later
- Large number of nodes

## BeginUpdate / EndUpdate

Suspend layout during bulk operations:

```csharp
treeViewAdv1.BeginUpdate();
try
{
    for (int i = 0; i < 10000; i++)
    {
        treeViewAdv1.Nodes.Add(new TreeNodeAdv($"Node {i}"));
    }
}
finally
{
    treeViewAdv1.EndUpdate();
}
```

## Combined Optimization

Maximum performance setup:

```csharp
private void OptimizeTreeView()
{
    // Enable virtualization
    treeViewAdv1.EnableVirtualization = true;
    
    // Optimize expand/collapse
    treeViewAdv1.SuspendExpandRecalculate = true;
    
    // Disable initial dimension calculation
    treeViewAdv1.RecalculateExpansion = false;
    
    // Suspend layout during load
    treeViewAdv1.BeginUpdate();
    try
    {
        // Load large dataset
        LoadLargeDataset();
    }
    finally
    {
        treeViewAdv1.EndUpdate();
    }
}
```

## Load on Demand Integration

Combine with LoadOnDemand for optimal performance:

```csharp
treeViewAdv1.EnableVirtualization = true;
treeViewAdv1.LoadOnDemand = true;
treeViewAdv1.SuspendExpandRecalculate = true;

treeViewAdv1.BeforeExpand += (sender, e) =>
{
    // Load children only when expanded
    if (e.Node.Nodes.Count == 0)
    {
        LoadChildNodes(e.Node);
    }
};
```

## Best Practices

1. **Always enable virtualization** for 1000+ nodes
2. **Use LoadOnDemand** for hierarchical data
3. **Suspend layout** during bulk operations
4. **Set RecalculateExpansion = false** for initial load
5. **Enable SuspendExpandRecalculate** for trees with frequent expand/collapse

## Performance Benchmarks

| Nodes | No Optimization | With Virtualization | Full Optimization |
|-------|-----------------|---------------------|-------------------|
| 1,000 | 2 sec | 0.5 sec | 0.2 sec |
| 5,000 | 10 sec | 1 sec | 0.4 sec |
| 20,000 | 60 sec | 3 sec | 1 sec |

## Complete Example

```csharp
public class PerformanceOptimizedTree : Form
{
    private TreeViewAdv treeViewAdv1;
    
    public PerformanceOptimizedTree()
    {
        InitializeOptimizedTree();
        LoadLargeDataset();
    }
    
    private void InitializeOptimizedTree()
    {
        treeViewAdv1 = new TreeViewAdv();
        treeViewAdv1.Size = new Size(400, 600);
        
        // Enable all optimizations
        treeViewAdv1.EnableVirtualization = true;
        treeViewAdv1.SuspendExpandRecalculate = true;
        treeViewAdv1.RecalculateExpansion = false;
        treeViewAdv1.LoadOnDemand = true;
        
        treeViewAdv1.BeforeExpand += TreeViewAdv1_BeforeExpand;
        
        this.Controls.Add(treeViewAdv1);
    }
    
    private void LoadLargeDataset()
    {
        treeViewAdv1.BeginUpdate();
        try
        {
            // Load 10,000 root nodes
            for (int i = 0; i < 10000; i++)
            {
                TreeNodeAdv node = new TreeNodeAdv($"Item {i}");
                node.Tag = i;
                node.ShowPlusOnExpand = true; // Indicate children available
                treeViewAdv1.Nodes.Add(node);
            }
        }
        finally
        {
            treeViewAdv1.EndUpdate();
        }
    }
    
    private void TreeViewAdv1_BeforeExpand(object sender, TreeViewAdvCancelableNodeEventArgs e)
    {
        // Load children on demand
        if (e.Node.Nodes.Count == 0)
        {
            int parentId = (int)e.Node.Tag;
            
            for (int i = 0; i < 100; i++)
            {
                e.Node.Nodes.Add(new TreeNodeAdv($"Child {parentId}.{i}"));
            }
        }
    }
}
```

## Troubleshooting

**Issue:** Still slow with large datasets
- **Solution:** Ensure EnableVirtualization = true, use LoadOnDemand, check data source query performance

**Issue:** UI freezes during load
- **Solution:** Use BeginUpdate/EndUpdate, consider background loading with async/await

**Issue:** Expand/collapse slow
- **Solution:** Set SuspendExpandRecalculate = true

**Issue:** Memory usage high
- **Solution:** Enable virtualization, use LoadOnDemand to avoid loading all nodes
