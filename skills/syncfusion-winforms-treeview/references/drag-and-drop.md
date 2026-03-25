# Drag and Drop

## Table of Contents
- [Overview](#overview)
- [Drag-Drop Events](#drag-drop-events)
- [Basic Implementation](#basic-implementation)
- [Advanced Scenarios](#advanced-scenarios)
- [Validation and Feedback](#validation-and-feedback)

## Overview

TreeViewAdv provides full drag-and-drop support for reordering nodes, moving nodes between parents, and implementing custom drag-drop behavior. Drag-drop operations are controlled through a comprehensive set of events.

## Drag-Drop Events

| Event | Description | When Fired |
|-------|-------------|------------|
| `ItemDrag` | Drag operation initiated | User begins dragging node |
| `DragEnter` | Item dragged into control | Dragged item enters control bounds |
| `DragOver` | Item dragged over control | Mouse moves while dragging over control |
| `DragLeave` | Item dragged out of control | Dragged item leaves control bounds |
| `DragDrop` | Item dropped | User releases mouse to complete drop |
| `GiveFeedback` | Visual feedback requested | During drag to update cursor/visual |
| `QueryContinueDrag` | Query drag continuation | System checks if drag should continue |

## Basic Implementation

### Enable Drag-Drop

```csharp
// Enable drag-drop on TreeViewAdv
treeViewAdv1.AllowDrop = true;
```

### Implement ItemDrag Event

Initiates the drag operation when user starts dragging a node.

```csharp
private void treeViewAdv1_ItemDrag(object sender, ItemDragEventArgs e)
{
    TreeViewAdv treeView = sender as TreeViewAdv;
    
    // Get the dragged node(s)
    TreeNodeAdv[] nodes = e.Item as TreeNodeAdv[];
    TreeNodeAdv node = nodes[0]; // Get first selected node
    
    // Start drag-drop operation
    DragDropEffects result = treeView.DoDragDrop(node, DragDropEffects.Move);
}
```

### Implement DragOver Event

Provides visual feedback and determines if drop is allowed at current position.

```csharp
private TreeNodeAdv sourceNode; // Track source node

private void treeViewAdv1_DragOver(object sender, DragEventArgs e)
{
    TreeViewAdv treeView = sender as TreeViewAdv;
    
    // Convert screen coordinates to tree coordinates
    Point ptInTree = treeView.PointToClient(new Point(e.X, e.Y));
    
    // Get destination node at mouse position
    TreeNodeAdv destinationNode = treeView.GetNodeAtPoint(ptInTree);
    
    // Check if drop is allowed
    if (e.Data.GetDataPresent(typeof(TreeNodeAdv)))
    {
        sourceNode = (TreeNodeAdv)e.Data.GetData(typeof(TreeNodeAdv));
        
        // Validate drop location
        if (IsValidDropTarget(sourceNode, destinationNode))
        {
            e.Effect = DragDropEffects.Move;
            
            // Highlight destination node
            treeView.SelectedNode = destinationNode;
        }
        else
        {
            e.Effect = DragDropEffects.None;
        }
    }
    else
    {
        e.Effect = DragDropEffects.None;
    }
}

private bool IsValidDropTarget(TreeNodeAdv source, TreeNodeAdv destination)
{
    // Prevent dropping on self
    if (source == destination)
        return false;
    
    // Prevent dropping on own descendant
    TreeNodeAdv parent = destination;
    while (parent != null)
    {
        if (parent == source)
            return false;
        parent = parent.Parent;
    }
    
    return destination != null;
}
```

### Implement DragDrop Event

Performs the actual move operation when node is dropped.

```csharp
private void treeViewAdv1_DragDrop(object sender, DragEventArgs e)
{
    TreeViewAdv treeView = sender as TreeViewAdv;
    Point ptInTree = treeView.PointToClient(new Point(e.X, e.Y));
    
    // Get destination node
    TreeNodeAdv destinationNode = treeView.GetNodeAtPoint(ptInTree);
    
    // Get source node
    if (e.Data.GetDataPresent(typeof(TreeNodeAdv)))
    {
        TreeNodeAdv sourceNode = (TreeNodeAdv)e.Data.GetData(typeof(TreeNodeAdv));
        
        if (destinationNode != null && sourceNode != destinationNode)
        {
            // Remove from original parent
            sourceNode.Remove();
            
            // Add to new parent
            destinationNode.Nodes.Add(sourceNode);
            
            // Expand destination to show moved node
            destinationNode.Expand();
            
            // Select moved node
            treeView.SelectedNode = sourceNode;
        }
    }
}
```

## Advanced Scenarios

### Drag to Insert as Sibling

Instead of making dropped node a child, insert it as a sibling.

```csharp
private void treeViewAdv1_DragDrop(object sender, DragEventArgs e)
{
    TreeViewAdv treeView = sender as TreeViewAdv;
    Point ptInTree = treeView.PointToClient(new Point(e.X, e.Y));
    TreeNodeAdv destinationNode = treeView.GetNodeAtPoint(ptInTree);
    
    if (e.Data.GetDataPresent(typeof(TreeNodeAdv)) && destinationNode != null)
    {
        TreeNodeAdv sourceNode = (TreeNodeAdv)e.Data.GetData(typeof(TreeNodeAdv));
        
        // Remove from original location
        sourceNode.Remove();
        
        // Insert as sibling (same parent as destination)
        if (destinationNode.Parent != null)
        {
            int index = destinationNode.Parent.Nodes.IndexOf(destinationNode);
            destinationNode.Parent.Nodes.Insert(index + 1, sourceNode);
        }
        else
        {
            // Destination is root node
            int index = treeView.Nodes.IndexOf(destinationNode);
            treeView.Nodes.Insert(index + 1, sourceNode);
        }
        
        treeView.SelectedNode = sourceNode;
    }
}
```

### Drag Between Two TreeViewAdv Controls

```csharp
// Source TreeViewAdv
private void treeViewAdv1_ItemDrag(object sender, ItemDragEventArgs e)
{
    TreeNodeAdv[] nodes = e.Item as TreeNodeAdv[];
    TreeNodeAdv node = nodes[0];
    
    // Include source control info in drag data
    DragDropEffects result = DoDragDrop(
        new DragData { Node = node, SourceTree = (TreeViewAdv)sender },
        DragDropEffects.Move | DragDropEffects.Copy
    );
}

// Destination TreeViewAdv
private void treeViewAdv2_DragDrop(object sender, DragEventArgs e)
{
    TreeViewAdv targetTree = sender as TreeViewAdv;
    Point ptInTree = targetTree.PointToClient(new Point(e.X, e.Y));
    TreeNodeAdv destinationNode = targetTree.GetNodeAtPoint(ptInTree);
    
    if (e.Data.GetDataPresent(typeof(DragData)))
    {
        DragData data = (DragData)e.Data.GetData(typeof(DragData));
        TreeNodeAdv sourceNode = data.Node;
        TreeViewAdv sourceTree = data.SourceTree;
        
        // Clone node for copy operation
        TreeNodeAdv newNode = (TreeNodeAdv)sourceNode.Clone();
        
        if (e.Effect == DragDropEffects.Move)
        {
            sourceNode.Remove(); // Remove from source
        }
        
        // Add to destination
        if (destinationNode != null)
            destinationNode.Nodes.Add(newNode);
        else
            targetTree.Nodes.Add(newNode);
    }
}

[Serializable]
class DragData
{
    public TreeNodeAdv Node { get; set; }
    public TreeViewAdv SourceTree { get; set; }
}
```

### Copy vs Move Based on Keyboard

```csharp
private void treeViewAdv1_DragOver(object sender, DragEventArgs e)
{
    // Check if CTRL key is pressed for copy
    if ((e.KeyState & 8) == 8) // CTRL key
    {
        e.Effect = DragDropEffects.Copy;
    }
    else
    {
        e.Effect = DragDropEffects.Move;
    }
}
```

## Validation and Feedback

### Prevent Invalid Drops

```csharp
private bool IsValidDropTarget(TreeNodeAdv source, TreeNodeAdv destination)
{
    if (destination == null)
        return false;
    
    // Can't drop on self
    if (source == destination)
        return false;
    
    // Can't drop parent onto its own descendant
    TreeNodeAdv parent = destination;
    while (parent != null)
    {
        if (parent == source)
            return false;
        parent = parent.Parent;
    }
    
    // Custom business logic
    if (source.Tag is FileInfo && destination.Tag is FileInfo)
        return false; // Can't drop file onto file
    
    return true;
}
```

### Custom Drag Cursor

```csharp
private void treeViewAdv1_GiveFeedback(object sender, GiveFeedbackEventArgs e)
{
    if (e.Effect == DragDropEffects.Move)
    {
        e.UseDefaultCursors = false;
        Cursor.Current = Cursors.Hand;
    }
    else if (e.Effect == DragDropEffects.None)
    {
        e.UseDefaultCursors = false;
        Cursor.Current = Cursors.No;
    }
    else
    {
        e.UseDefaultCursors = true;
    }
}
```

### Visual Drop Indicator

```csharp
private void treeViewAdv1_DragOver(object sender, DragEventArgs e)
{
    TreeViewAdv treeView = sender as TreeViewAdv;
    Point ptInTree = treeView.PointToClient(new Point(e.X, e.Y));
    TreeNodeAdv targetNode = treeView.GetNodeAtPoint(ptInTree);
    
    if (targetNode != null)
    {
        // Highlight target node
        treeView.SelectedNode = targetNode;
        
        // Auto-expand after hovering
        if (DateTime.Now.Subtract(lastDragOverTime).TotalMilliseconds > 1000)
        {
            if (!targetNode.Expanded && targetNode.Nodes.Count > 0)
            {
                targetNode.Expand();
            }
        }
    }
    
    lastDragOverTime = DateTime.Now;
}

private DateTime lastDragOverTime = DateTime.Now;
```

### Complete Example

```csharp
public class TreeViewDragDropExample : Form
{
    private TreeViewAdv treeViewAdv1;
    private TreeNodeAdv draggedNode;
    
    public TreeViewDragDropExample()
    {
        InitializeTreeView();
    }
    
    private void InitializeTreeView()
    {
        treeViewAdv1 = new TreeViewAdv();
        treeViewAdv1.Size = new Size(300, 400);
        treeViewAdv1.AllowDrop = true;
        
        // Subscribe to events
        treeViewAdv1.ItemDrag += TreeViewAdv1_ItemDrag;
        treeViewAdv1.DragOver += TreeViewAdv1_DragOver;
        treeViewAdv1.DragDrop += TreeViewAdv1_DragDrop;
        
        // Add sample nodes
        var root1 = new TreeNodeAdv("Folder 1");
        root1.Nodes.Add(new TreeNodeAdv("Item 1.1"));
        root1.Nodes.Add(new TreeNodeAdv("Item 1.2"));
        
        var root2 = new TreeNodeAdv("Folder 2");
        root2.Nodes.Add(new TreeNodeAdv("Item 2.1"));
        
        treeViewAdv1.Nodes.AddRange(new[] { root1, root2 });
        this.Controls.Add(treeViewAdv1);
    }
    
    private void TreeViewAdv1_ItemDrag(object sender, ItemDragEventArgs e)
    {
        TreeNodeAdv[] nodes = e.Item as TreeNodeAdv[];
        draggedNode = nodes[0];
        DoDragDrop(draggedNode, DragDropEffects.Move);
    }
    
    private void TreeViewAdv1_DragOver(object sender, DragEventArgs e)
    {
        TreeViewAdv tree = sender as TreeViewAdv;
        Point pt = tree.PointToClient(new Point(e.X, e.Y));
        TreeNodeAdv targetNode = tree.GetNodeAtPoint(pt);
        
        if (e.Data.GetDataPresent(typeof(TreeNodeAdv)) && targetNode != null)
        {
            TreeNodeAdv source = (TreeNodeAdv)e.Data.GetData(typeof(TreeNodeAdv));
            
            if (IsValidDrop(source, targetNode))
            {
                e.Effect = DragDropEffects.Move;
                tree.SelectedNode = targetNode;
            }
            else
            {
                e.Effect = DragDropEffects.None;
            }
        }
    }
    
    private void TreeViewAdv1_DragDrop(object sender, DragEventArgs e)
    {
        TreeViewAdv tree = sender as TreeViewAdv;
        Point pt = tree.PointToClient(new Point(e.X, e.Y));
        TreeNodeAdv targetNode = tree.GetNodeAtPoint(pt);
        
        if (e.Data.GetDataPresent(typeof(TreeNodeAdv)) && targetNode != null)
        {
            TreeNodeAdv source = (TreeNodeAdv)e.Data.GetData(typeof(TreeNodeAdv));
            
            source.Remove();
            targetNode.Nodes.Add(source);
            targetNode.Expand();
            tree.SelectedNode = source;
        }
    }
    
    private bool IsValidDrop(TreeNodeAdv source, TreeNodeAdv target)
    {
        if (source == target) return false;
        
        TreeNodeAdv parent = target;
        while (parent != null)
        {
            if (parent == source) return false;
            parent = parent.Parent;
        }
        
        return true;
    }
}
```

## Troubleshooting

**Issue:** Drag-drop not working
- **Solution:** Set `AllowDrop = true` on TreeViewAdv, subscribe to ItemDrag, DragOver, and DragDrop events

**Issue:** Can drop node on itself or descendants
- **Solution:** Implement validation in DragOver event to check parent-child relationships

**Issue:** Visual feedback not showing
- **Solution:** Set `treeView.SelectedNode` in DragOver event to highlight target node

**Issue:** Cursor doesn't change during drag
- **Solution:** Handle GiveFeedback event to customize cursor

**Issue:** Nodes disappearing after drag
- **Solution:** Ensure Remove() is called before adding to new location, check that Add() succeeds
