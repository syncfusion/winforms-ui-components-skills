# Load on Demand

Load on demand enables lazy loading of child nodes, improving performance by loading data only when needed.

## Overview

LoadOnDemand delays loading child nodes until parent is expanded, ideal for large hierarchical datasets or remote data sources.

## Enabling Load on Demand

```csharp
// Enable load on demand
treeViewAdv1.LoadOnDemand = true;
```

## BeforeExpand Event

Handle `BeforeExpand` to load children dynamically:

```csharp
treeViewAdv1.BeforeExpand += (sender, e) =>
{
    TreeNodeAdv node = e.Node;
    
    // Check if children already loaded
    if (node.Nodes.Count == 0)
    {
        // Load children from data source
        var children = LoadChildrenFromDatabase(node.Tag);
        
        foreach (var child in children)
        {
            TreeNodeAdv childNode = new TreeNodeAdv(child.Name);
            childNode.Tag = child;
            node.Nodes.Add(childNode);
        }
    }
};
```

## ShowPlusOnExpand

Keep plus sign visible after expansion for reloadable nodes:

```csharp
// Required for ShowPlusOnExpand
treeViewAdv1.LoadOnDemand = true;

TreeNodeAdv node = new TreeNodeAdv("Reloadable");
node.ShowPlusOnExpand = true; // Plus sign remains after expand
```

## GetPath Method

Retrieve node path for loading:

```csharp
// Enable separator at path end
treeViewAdv1.AddSeparatorAtEnd = true;

// Get node path
treeViewAdv1.BeforeExpand += (sender, e) =>
{
    TreeNodeAdv node = e.Node;
    string path = node.GetPath("\\");
    
    // Use path to load data
    var data = LoadDataFromPath(path);
};
```

## Complete Example

```csharp
public class LoadOnDemandExample : Form
{
    private TreeViewAdv treeViewAdv1;
    
    public LoadOnDemandExample()
    {
        treeViewAdv1 = new TreeViewAdv();
        treeViewAdv1.LoadOnDemand = true;
        treeViewAdv1.AddSeparatorAtEnd = true;
        treeViewAdv1.BeforeExpand += TreeViewAdv1_BeforeExpand;
        
        // Add root nodes
        for (int i = 1; i <= 5; i++)
        {
            TreeNodeAdv root = new TreeNodeAdv($"Folder {i}");
            root.Tag = i;
            root.ShowPlusOnExpand = true; // Show plus even after load
            treeViewAdv1.Nodes.Add(root);
        }
        
        this.Controls.Add(treeViewAdv1);
    }
    
    private void TreeViewAdv1_BeforeExpand(object sender, TreeViewAdvCancelableNodeEventArgs e)
    {
        TreeNodeAdv node = e.Node;
        
        // Only load if not already loaded
        if (node.Nodes.Count == 0)
        {
            // Simulate loading from database
            int parentId = (int)node.Tag;
            var children = LoadChildNodes(parentId);
            
            foreach (var child in children)
            {
                TreeNodeAdv childNode = new TreeNodeAdv(child.Name);
                childNode.Tag = child.Id;
                childNode.ShowPlusOnExpand = true; // Allow reloading
                node.Nodes.Add(childNode);
            }
        }
    }
    
    private List<NodeData> LoadChildNodes(int parentId)
    {
        // Simulate database call
        return new List<NodeData>
        {
            new NodeData { Id = parentId * 10 + 1, Name = $"Child {parentId}.1" },
            new NodeData { Id = parentId * 10 + 2, Name = $"Child {parentId}.2" }
        };
    }
}

class NodeData
{
    public int Id { get; set; }
    public string Name { get; set; }
}
```

## Performance Benefits

**Before Load on Demand:**
- 10,000 nodes loaded upfront = 60 seconds

**After Load on Demand:**
- Root nodes only = <1 second
- Children loaded as needed

## Troubleshooting

**Issue:** Plus signs not showing
- **Solution:** Set `LoadOnDemand = true`, ensure nodes have potential children

**Issue:** ShowPlusOnExpand not working
- **Solution:** Requires `LoadOnDemand = true` to function

**Issue:** Children loading multiple times
- **Solution:** Check `if (node.Nodes.Count == 0)` before loading

**Issue:** GetPath returns incorrect path
- **Solution:** Set `AddSeparatorAtEnd = true` for trailing separator
