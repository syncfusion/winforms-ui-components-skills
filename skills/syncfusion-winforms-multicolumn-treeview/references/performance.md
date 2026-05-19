# Performance Optimization

This guide covers techniques for optimizing MultiColumnTreeView performance when working with large datasets.

## SuspendExpandRecalculate

This property significantly improves performance when expanding and collapsing nodes with large hierarchies.

### How It Works

When `SuspendExpandRecalculate` is `true`, the control reduces unnecessary recalculations of node dimensions during expand/collapse operations. This can nearly halve populating time for large trees.

```csharp
// Enable performance optimization
multiColumnTreeView1.SuspendExpandRecalculate = true;
```

**Benefits:**
- Reduces populating time by up to 50%
- Avoids recalculating dimensions for collapsed child nodes
- Improves responsiveness during expand/collapse

**When to Use:**
- Large trees with many nodes (1000+ nodes)
- Deep hierarchies (5+ levels)
- Frequent expand/collapse operations

## BeginUpdate/EndUpdate Pattern

Temporarily suspend visual updates when adding, removing, or modifying multiple nodes.

### Basic Usage

```csharp
multiColumnTreeView1.BeginUpdate();
try
{
    // Add/modify many nodes here
    for (int i = 0; i < 10000; i++)
    {
        TreeNodeAdv node = new TreeNodeAdv();
        node.Text = $"Node {i}";
        multiColumnTreeView1.Nodes.Add(node);
    }
}
finally
{
    multiColumnTreeView1.EndUpdate();
}
```

### With Hierarchy

```csharp
multiColumnTreeView1.BeginUpdate();
try
{
    foreach (var dept in departments)
    {
        TreeNodeAdv deptNode = new TreeNodeAdv { Text = dept.Name };
        
        foreach (var emp in dept.Employees)
        {
            TreeNodeAdv empNode = new TreeNodeAdv { Text = emp.Name };
            empNode.SubItems.Add(new TreeNodeAdvSubItem { Text = emp.Title });
            empNode.SubItems.Add(new TreeNodeAdvSubItem { Text = emp.Salary.ToString() });
            deptNode.Nodes.Add(empNode);
        }
        
        multiColumnTreeView1.Nodes.Add(deptNode);
    }
}
finally
{
    multiColumnTreeView1.EndUpdate();
}
```

**Benefits:**
- Prevents redrawing after each node addition
- Dramatically faster for bulk operations
- Reduces UI flicker

## Load on Demand

Delay loading child nodes until user expands the parent node.

### Enabling Load on Demand

```csharp
// Enable load on demand
multiColumnTreeView1.LoadOnDemand = true;
multiColumnTreeView1.AddSeparatorAtEnd = true; // For path-based loading

// Handle BeforeExpand event
multiColumnTreeView1.BeforeExpand += MultiColumnTreeView1_BeforeExpand;
```

### File System Example

```csharp
private void LoadFileSystem_BeforeExpand(object sender, TreeViewAdvCancelableNodeEventArgs e)
{
    if (e.Node.ExpandedOnce)
        return;
    
    // Get the path for the specific node
    string path = e.Node.GetPath("\\");
    
    try
    {
        // Get directories in this path
        string[] directories = Directory.GetDirectories(path);
        
        foreach (string dir in directories)
        {
            DirectoryInfo dirInfo = new DirectoryInfo(dir);
            TreeNodeAdv dirNode = new TreeNodeAdv { Text = dirInfo.Name };
            e.Node.Nodes.Add(dirNode);
        }
        
        // Get files in this path
        string[] files = Directory.GetFiles(path);
        
        foreach (string file in files)
        {
            FileInfo fileInfo = new FileInfo(file);
            TreeNodeAdv fileNode = new TreeNodeAdv { Text = fileInfo.Name };
            fileNode.SubItems.Add(new TreeNodeAdvSubItem { Text = fileInfo.Length.ToString() });
            e.Node.Nodes.Add(fileNode);
        }
    }
    catch (UnauthorizedAccessException)
    {
        // Handle access denied
    }
}
```

**Benefits:**
- Faster initial load time
- Reduced memory usage
- Better scalability for deep trees

## Practical Performance Tips

### 1. Batch Operations

```csharp
// Good: Batch operations
multiColumnTreeView1.BeginUpdate();
try
{
    multiColumnTreeView1.Nodes.AddRange(new TreeNodeAdv[] { node1, node2, node3 });
}
finally
{
    multiColumnTreeView1.EndUpdate();
}

// Bad: Individual operations
multiColumnTreeView1.Nodes.Add(node1);
multiColumnTreeView1.Nodes.Add(node2);
multiColumnTreeView1.Nodes.Add(node3);
```

### 2. Lazy SubItem Creation

```csharp
// Good: Create subitems only when needed
private void CreateNodeWithSubItems(string text, params string[] subItemTexts)
{
    TreeNodeAdv node = new TreeNodeAdv { Text = text };
    
    foreach (string subItemText in subItemTexts)
    {
        node.SubItems.Add(new TreeNodeAdvSubItem { Text = subItemText });
    }
    
    return node;
}

// Better: Delay until visible
private void LoadVisibleNodeDetails()
{
    foreach (TreeNodeAdv node in GetVisibleNodes())
    {
        if (node.SubItems.Count == 0)
        {
            LoadSubItems(node);
        }
    }
}
```

### 3. Avoid Frequent Refreshes

```csharp
// Bad: Refresh in loop
foreach (TreeNodeAdv node in nodes)
{
    node.Text = GetUpdatedText(node);
    multiColumnTreeView1.Refresh(); // DON'T DO THIS
}

// Good: Refresh once after all updates
multiColumnTreeView1.BeginUpdate();
try
{
    foreach (TreeNodeAdv node in nodes)
    {
        node.Text = GetUpdatedText(node);
    }
}
finally
{
    multiColumnTreeView1.EndUpdate(); // Automatic refresh
}
```

### 4. Memory Management

```csharp
// Clear nodes when form closes
protected override void OnFormClosing(FormClosingEventArgs e)
{
    base.OnFormClosing(e);
    
    multiColumnTreeView1.BeginUpdate();
    try
    {
        multiColumnTreeView1.Nodes.Clear();
    }
    finally
    {
        multiColumnTreeView1.EndUpdate();
    }
}

// Dispose of image lists
protected override void Dispose(bool disposing)
{
    if (disposing)
    {
        leftImageList?.Dispose();
        rightImageList?.Dispose();
        stateImageList?.Dispose();
    }
    base.Dispose(disposing);
}
```

### 5. Virtual Mode Pattern

For extremely large datasets, implement a virtual loading pattern:

```csharp
// Placeholder nodes for unloaded data
private void AddPlaceholderNodes(TreeNodeAdv parent, int count)
{
    for (int i = 0; i < count; i++)
    {
        TreeNodeAdv placeholder = new TreeNodeAdv { Text = "Loading...", Tag = "placeholder" };
        parent.Nodes.Add(placeholder);
    }
}

// Replace placeholder with real data when needed
private void BeforeExpand_VirtualMode(object sender, TreeViewAdvCancelableNodeEventArgs e)
{
    multiColumnTreeView1.BeginUpdate();
    try
    {
        // Remove placeholders
        for (int i = e.Node.Nodes.Count - 1; i >= 0; i--)
        {
            if (e.Node.Nodes[i].Tag as string == "placeholder")
            {
                e.Node.Nodes.RemoveAt(i);
            }
        }
        
        // Load real data
        LoadRealChildren(e.Node);
    }
    finally
    {
        multiColumnTreeView1.EndUpdate();
    }
}
```

## Performance Benchmarks

Typical performance improvements:

| Scenario | Without Optimization | With Optimization | Improvement |
|----------|---------------------|-------------------|-------------|
| Add 10,000 nodes | ~5-8 seconds | ~0.5-1 second | 90% faster |
| Expand large branch | ~2-3 seconds | ~0.2-0.5 seconds | 85% faster |
| Sort 5,000 nodes | ~3-4 seconds | ~0.3-0.6 seconds | 90% faster |

## Complete Example

```csharp
public class PerformanceOptimizedTree
{
    private MultiColumnTreeView treeView;
    
    public PerformanceOptimizedTree(MultiColumnTreeView tree)
    {
        this.treeView = tree;
        ConfigureForPerformance();
    }
    
    private void ConfigureForPerformance()
    {
        // Enable performance features
        treeView.SuspendExpandRecalculate = true;
        treeView.LoadOnDemand = true;
        treeView.AddSeparatorAtEnd = true;
        
        // Handle load on demand
        treeView.BeforeExpand += TreeView_BeforeExpand;
    }
    
    public void LoadLargeDataset(List<DataItem> items)
    {
        treeView.BeginUpdate();
        try
        {
            // Group items by category
            var grouped = items.GroupBy(i => i.Category);
            
            foreach (var group in grouped)
            {
                TreeNodeAdv categoryNode = new TreeNodeAdv 
                { 
                    Text = group.Key,
                    Tag = group.ToList() // Store data for lazy loading
                };
                
                // Add placeholder to show expand icon
                if (group.Count() > 0)
                {
                    categoryNode.Nodes.Add(new TreeNodeAdv { Text = "" });
                }
                
                treeView.Nodes.Add(categoryNode);
            }
        }
        finally
        {
            treeView.EndUpdate();
        }
    }
    
    private void TreeView_BeforeExpand(object sender, TreeViewAdvCancelableNodeEventArgs e)
    {
        if (e.Node.ExpandedOnce)
            return;
        
        treeView.BeginUpdate();
        try
        {
            // Clear placeholder
            e.Node.Nodes.Clear();
            
            // Load actual data
            if (e.Node.Tag is List<DataItem> items)
            {
                foreach (DataItem item in items)
                {
                    TreeNodeAdv node = new TreeNodeAdv { Text = item.Name };
                    node.SubItems.Add(new TreeNodeAdvSubItem { Text = item.Value });
                    e.Node.Nodes.Add(node);
                }
            }
        }
        finally
        {
            treeView.EndUpdate();
        }
    }
}
```

## Best Practices

1. **Always use BeginUpdate/EndUpdate** for bulk operations
2. **Enable SuspendExpandRecalculate** for large trees
3. **Implement load on demand** for trees with many nodes
4. **Batch node additions** using AddRange
5. **Clear unnecessary resources** when closing forms
6. **Test with realistic data volumes** before deployment
7. **Profile performance** to identify bottlenecks
8. **Consider pagination** for extremely large datasets (10,000+ nodes)

## Troubleshooting Performance Issues

**Slow initial load:**
- Implement load on demand
- Use BeginUpdate/EndUpdate
- Load only visible levels initially

**Slow expand/collapse:**
- Enable SuspendExpandRecalculate
- Reduce node complexity
- Limit subitems and images

**High memory usage:**
- Clear nodes when not needed
- Dispose image lists properly
- Implement virtual mode for very large trees

**UI freezing:**
- Use background threads for data loading
- Show progress indicators
- Implement async/await patterns for data fetching
