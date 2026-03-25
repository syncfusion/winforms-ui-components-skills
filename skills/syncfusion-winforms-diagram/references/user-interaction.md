# User Interaction

Configure user interactions with diagram elements including selection, dragging, resizing, and rotation.

## Selection

### Single Selection

```csharp
// Enable node selection (enabled by default)
diagram1.Model.SelectionMode = SelectionMode.Single;

// Select a node programmatically
Node node = diagram1.Model.Nodes[0];
diagram1.Controller.SelectNodes(new Node[] { node });
```

### Multiple Selection

```csharp
// Enable multiple selection
diagram1.Model.SelectionMode = SelectionMode.Multiple;

// Select multiple nodes with Ctrl+Click automatically enabled

// Select multiple nodes programmatically
Node[] nodes = new Node[] 
{
    diagram1.Model.Nodes[0],
    diagram1.Model.Nodes[1],
    diagram1.Model.Nodes[2]
};
diagram1.Controller.SelectNodes(nodes);
```

### Select All

```csharp
// Select all nodes
diagram1.Controller.SelectAll();

// Clear selection
diagram1.Controller.ClearSelection();
```

### Selection Events

```csharp
// Node selected event
diagram1.Model.NodeSelected += (sender, e) =>
{
    Console.WriteLine($"Selected: {e.Node.Name}");
    
    // Change appearance
    e.Node.LineStyle.LineColor = Color.Blue;
    e.Node.LineStyle.LineWidth = 3;
    diagram1.UpdateView();
};

// Node deselected event
diagram1.Model.NodeDeselected += (sender, e) =>
{
    Console.WriteLine($"Deselected: {e.Node.Name}");
    
    // Restore appearance
    e.Node.LineStyle.LineColor = Color.Black;
    e.Node.LineStyle.LineWidth = 1;
    diagram1.UpdateView();
};
```

### Selection Handles

```csharp
// Customize selection handle appearance
diagram1.Model.SelectionHandleStyle.FillColor = Color.Blue;
diagram1.Model.SelectionHandleStyle.BorderColor = Color.DarkBlue;
diagram1.Model.SelectionHandleStyle.Size = new SizeF(8, 8);

// Hide selection handles
diagram1.Model.ShowSelectionHandles = false;
```

### Selection Constraints

```csharp
// Prevent selection of specific nodes
Node node = new Rectangle(100, 100, 100, 60);
node.EnableSelection = false;  // Cannot be selected

diagram1.Model.AppendChild(node);
```

## Dragging

### Enable/Disable Dragging

```csharp
// Enable dragging (enabled by default)
Node node = new Rectangle(100, 100, 100, 60);
node.EnableDrag = true;

diagram1.Model.AppendChild(node);

// Disable dragging for specific node
node.EnableDrag = false;
```

### Drag Events

```csharp
// Drag start
diagram1.Model.NodeDragStart += (sender, e) =>
{
    Console.WriteLine($"Drag started: {e.Node.Name}");
    e.Node.FillStyle.ColorAlphaFactor = 128; // Semi-transparent
    diagram1.UpdateView();
};

// Dragging
diagram1.Model.NodeDragging += (sender, e) =>
{
    // Show coordinates during drag
    Console.WriteLine($"Dragging to: {e.Node.PinPoint}");
};

// Drag end
diagram1.Model.NodeDragEnd += (sender, e) =>
{
    Console.WriteLine($"Drag ended: {e.Node.Name}");
    e.Node.FillStyle.ColorAlphaFactor = 255; // Opaque
    diagram1.UpdateView();
};
```

### Constrained Dragging

```csharp
// Constrain dragging to horizontal axis
diagram1.Model.NodeDragging += (sender, e) =>
{
    if (e.Node.Tag as string == "horizontal-only")
    {
        // Keep Y coordinate fixed
        float fixedY = 100;
        e.Node.PinPoint = new PointF(e.Node.PinPoint.X, fixedY);
    }
};

// Tag the node
node.Tag = "horizontal-only";
```

### Drag Boundaries

```csharp
// Constrain dragging within boundaries
RectangleF dragBounds = new RectangleF(0, 0, 800, 600);

diagram1.Model.NodeDragging += (sender, e) =>
{
    PointF point = e.Node.PinPoint;
    
    // Clamp to boundaries
    float x = Math.Max(dragBounds.Left, Math.Min(dragBounds.Right, point.X));
    float y = Math.Max(dragBounds.Top, Math.Min(dragBounds.Bottom, point.Y));
    
    e.Node.PinPoint = new PointF(x, y);
};
```

## Resizing

### Enable/Disable Resizing

```csharp
// Enable resizing (enabled by default)
Node node = new Rectangle(100, 100, 100, 60);
node.EnableResize = true;

diagram1.Model.AppendChild(node);

// Disable resizing
node.EnableResize = false;
```

### Resize Events

```csharp
// Resize start
diagram1.Model.NodeResizeStart += (sender, e) =>
{
    Console.WriteLine($"Resize started: {e.Node.Name}");
};

// Resizing
diagram1.Model.NodeResizing += (sender, e) =>
{
    Console.WriteLine($"New size: {e.Node.Size}");
};

// Resize end
diagram1.Model.NodeResizeEnd += (sender, e) =>
{
    Console.WriteLine($"Resize ended: {e.Node.Name}");
};
```

### Constrained Resizing

```csharp
// Maintain aspect ratio during resize
float aspectRatio = 1.5f; // Width / Height

diagram1.Model.NodeResizing += (sender, e) =>
{
    if (e.Node.Tag as string == "maintain-aspect")
    {
        // Adjust height to maintain aspect ratio
        float newHeight = e.Node.Width / aspectRatio;
        e.Node.Height = newHeight;
    }
};

node.Tag = "maintain-aspect";
```

### Minimum/Maximum Size

```csharp
// Set size constraints
SizeF minSize = new SizeF(50, 30);
SizeF maxSize = new SizeF(300, 200);

diagram1.Model.NodeResizing += (sender, e) =>
{
    // Enforce minimum size
    float width = Math.Max(minSize.Width, e.Node.Width);
    float height = Math.Max(minSize.Height, e.Node.Height);
    
    // Enforce maximum size
    width = Math.Min(maxSize.Width, width);
    height = Math.Min(maxSize.Height, height);
    
    e.Node.Size = new SizeF(width, height);
};
```

### Proportional Resizing

```csharp
// Enable proportional resize (Shift+Drag handle)
diagram1.Model.EnableProportionalResize = true;
```

## Rotation

### Enable/Disable Rotation

```csharp
// Enable rotation (enabled by default)
Node node = new Rectangle(100, 100, 100, 60);
node.EnableRotate = true;

diagram1.Model.AppendChild(node);

// Disable rotation
node.EnableRotate = false;
```

### Programmatic Rotation

```csharp
// Rotate node
Node node = diagram1.Model.Nodes[0];
node.RotationAngle = 45f; // 45 degrees

diagram1.UpdateView();
```

### Rotation Events

```csharp
// Rotate start
diagram1.Model.NodeRotateStart += (sender, e) =>
{
    Console.WriteLine($"Rotation started: {e.Node.Name}");
};

// Rotating
diagram1.Model.NodeRotating += (sender, e) =>
{
    Console.WriteLine($"Rotation angle: {e.Node.RotationAngle}");
};

// Rotate end
diagram1.Model.NodeRotateEnd += (sender, e) =>
{
    Console.WriteLine($"Rotation ended: {e.Node.Name}");
};
```

### Rotation Constraints

```csharp
// Snap rotation to 15-degree increments
float rotationIncrement = 15f;

diagram1.Model.NodeRotating += (sender, e) =>
{
    float angle = e.Node.RotationAngle;
    float snappedAngle = Math.Round(angle / rotationIncrement) * rotationIncrement;
    e.Node.RotationAngle = snappedAngle;
};
```

### Rotation Handle

```csharp
// Show/hide rotation handle
diagram1.Model.ShowRotateHandle = true;

// Customize rotation handle appearance
diagram1.Model.RotateHandleStyle.FillColor = Color.Green;
diagram1.Model.RotateHandleStyle.BorderColor = Color.DarkGreen;
```

## Mouse Interaction

### Click Events

```csharp
// Single click
diagram1.MouseClick += (sender, e) =>
{
    Node node = diagram1.Controller.HitTest(e.Location);
    if (node != null)
    {
        Console.WriteLine($"Clicked: {node.Name}");
    }
};

// Double click
diagram1.MouseDoubleClick += (sender, e) =>
{
    Node node = diagram1.Controller.HitTest(e.Location);
    if (node != null)
    {
        // Edit node
        EditNode(node);
    }
};
```

### Hover Effects

```csharp
private Node hoveredNode = null;

// Mouse move
diagram1.MouseMove += (sender, e) =>
{
    Node node = diagram1.Controller.HitTest(e.Location);
    
    if (node != hoveredNode)
    {
        // Restore previous hovered node
        if (hoveredNode != null)
        {
            hoveredNode.FillStyle.ColorAlphaFactor = 255;
        }
        
        // Highlight new node
        if (node != null)
        {
            node.FillStyle.ColorAlphaFactor = 200;
        }
        
        hoveredNode = node;
        diagram1.UpdateView();
    }
};

// Mouse leave
diagram1.MouseLeave += (sender, e) =>
{
    if (hoveredNode != null)
    {
        hoveredNode.FillStyle.ColorAlphaFactor = 255;
        hoveredNode = null;
        diagram1.UpdateView();
    }
};
```

### Mouse Cursor

```csharp
// Change cursor based on context
diagram1.MouseMove += (sender, e) =>
{
    Node node = diagram1.Controller.HitTest(e.Location);
    
    if (node != null)
    {
        diagram1.Cursor = Cursors.Hand;
    }
    else
    {
        diagram1.Cursor = Cursors.Default;
    }
};
```

## Keyboard Interaction

### Delete Key

```csharp
// Handle Delete key
diagram1.KeyDown += (sender, e) =>
{
    if (e.KeyCode == Keys.Delete)
    {
        DeleteSelectedNodes();
        e.Handled = true;
    }
};

private void DeleteSelectedNodes()
{
    List<Node> nodesToDelete = new List<Node>(diagram1.Model.SelectedNodes);
    
    foreach (Node node in nodesToDelete)
    {
        diagram1.Model.RemoveChild(node);
    }
}
```

### Arrow Keys (Nudge)

```csharp
// Handle arrow keys for nudging
float nudgeAmount = 5f;

diagram1.KeyDown += (sender, e) =>
{
    if (diagram1.Model.SelectedNodes.Count > 0)
    {
        PointF offset = PointF.Empty;
        
        switch (e.KeyCode)
        {
            case Keys.Up:
                offset = new PointF(0, -nudgeAmount);
                break;
            case Keys.Down:
                offset = new PointF(0, nudgeAmount);
                break;
            case Keys.Left:
                offset = new PointF(-nudgeAmount, 0);
                break;
            case Keys.Right:
                offset = new PointF(nudgeAmount, 0);
                break;
        }
        
        if (offset != PointF.Empty)
        {
            NudgeSelectedNodes(offset);
            e.Handled = true;
        }
    }
};

private void NudgeSelectedNodes(PointF offset)
{
    foreach (Node node in diagram1.Model.SelectedNodes)
    {
        node.PinPoint = new PointF(
            node.PinPoint.X + offset.X,
            node.PinPoint.Y + offset.Y
        );
    }
    
    diagram1.UpdateView();
}
```

### Copy/Paste Shortcuts

```csharp
diagram1.KeyDown += (sender, e) =>
{
    if (e.Control)
    {
        switch (e.KeyCode)
        {
            case Keys.C:
                diagram1.Controller.Copy();
                e.Handled = true;
                break;
            case Keys.X:
                diagram1.Controller.Cut();
                e.Handled = true;
                break;
            case Keys.V:
                diagram1.Controller.Paste();
                e.Handled = true;
                break;
            case Keys.A:
                diagram1.Controller.SelectAll();
                e.Handled = true;
                break;
            case Keys.Z:
                diagram1.Model.HistoryManager.Undo();
                e.Handled = true;
                break;
            case Keys.Y:
                diagram1.Model.HistoryManager.Redo();
                e.Handled = true;
                break;
        }
    }
};
```

## Touch Support

### Enable Touch

```csharp
// Enable touch gestures
diagram1.EnableTouch = true;
```

### Multi-Touch Zoom

```csharp
// Pinch-to-zoom automatically enabled
diagram1.EnableTouchZoom = true;
```

### Touch Panning

```csharp
// Touch pan automatically enabled
diagram1.EnableTouchPan = true;
```

## Interaction Constraints

### Lock Nodes

```csharp
// Lock node (prevent all interactions)
Node node = new Rectangle(100, 100, 100, 60);
node.EnableSelection = false;
node.EnableDrag = false;
node.EnableResize = false;
node.EnableRotate = false;

diagram1.Model.AppendChild(node);
```

### Read-Only Diagram

```csharp
// Make entire diagram read-only
public void SetReadOnly(bool readOnly)
{
    diagram1.Controller.Enabled = !readOnly;
    
    if (readOnly)
    {
        // Disable all tools
        foreach (Tool tool in diagram1.Controller.Tools)
        {
            tool.Enabled = false;
        }
        
        // Clear selection
        diagram1.Controller.ClearSelection();
    }
    else
    {
        // Re-enable tools
        foreach (Tool tool in diagram1.Controller.Tools)
        {
            tool.Enabled = true;
        }
    }
}
```

### Conditional Interactions

```csharp
// Allow interaction based on custom logic
diagram1.Model.NodeDragStart += (sender, e) =>
{
    // Check permissions
    if (!HasEditPermission(e.Node))
    {
        e.Cancel = true; // Cancel the drag
        MessageBox.Show("You don't have permission to edit this node.");
    }
};

private bool HasEditPermission(Node node)
{
    // Custom permission logic
    return true;
}
```

## Complete Interaction Manager Example

```csharp
public class DiagramInteractionManager
{
    private Diagram diagram;
    private Node hoveredNode;
    
    public DiagramInteractionManager(Diagram diagram)
    {
        this.diagram = diagram;
        InitializeInteractions();
    }
    
    private void InitializeInteractions()
    {
        // Mouse events
        diagram.MouseClick += OnMouseClick;
        diagram.MouseDoubleClick += OnMouseDoubleClick;
        diagram.MouseMove += OnMouseMove;
        
        // Keyboard events
        diagram.KeyDown += OnKeyDown;
        
        // Node events
        diagram.Model.NodeSelected += OnNodeSelected;
        diagram.Model.NodeDragStart += OnNodeDragStart;
        diagram.Model.NodeDragEnd += OnNodeDragEnd;
        diagram.Model.NodeResizing += OnNodeResizing;
    }
    
    private void OnMouseClick(object sender, MouseEventArgs e)
    {
        Node node = diagram.Controller.HitTest(e.Location);
        if (node != null && e.Button == MouseButtons.Left)
        {
            Console.WriteLine($"Clicked: {node.Name}");
        }
    }
    
    private void OnMouseDoubleClick(object sender, MouseEventArgs e)
    {
        Node node = diagram.Controller.HitTest(e.Location);
        if (node != null)
        {
            ShowNodeEditor(node);
        }
    }
    
    private void OnMouseMove(object sender, MouseEventArgs e)
    {
        Node node = diagram.Controller.HitTest(e.Location);
        
        if (node != hoveredNode)
        {
            // Restore previous
            if (hoveredNode != null)
            {
                hoveredNode.LineStyle.LineWidth = 1;
            }
            
            // Highlight current
            if (node != null)
            {
                node.LineStyle.LineWidth = 2;
                diagram.Cursor = Cursors.Hand;
            }
            else
            {
                diagram.Cursor = Cursors.Default;
            }
            
            hoveredNode = node;
            diagram.UpdateView();
        }
    }
    
    private void OnKeyDown(object sender, KeyEventArgs e)
    {
        if (e.KeyCode == Keys.Delete)
        {
            DeleteSelectedNodes();
        }
        else if (e.Control && e.KeyCode == Keys.D)
        {
            DuplicateSelectedNodes();
        }
    }
    
    private void OnNodeSelected(object sender, NodeEventArgs e)
    {
        e.Node.LineStyle.LineColor = Color.Blue;
        diagram.UpdateView();
    }
    
    private void OnNodeDragStart(object sender, NodeDragEventArgs e)
    {
        e.Node.FillStyle.ColorAlphaFactor = 128;
        diagram.UpdateView();
    }
    
    private void OnNodeDragEnd(object sender, NodeDragEventArgs e)
    {
        e.Node.FillStyle.ColorAlphaFactor = 255;
        diagram.UpdateView();
    }
    
    private void OnNodeResizing(object sender, NodeResizeEventArgs e)
    {
        // Enforce minimum size
        if (e.Node.Width < 50) e.Node.Width = 50;
        if (e.Node.Height < 30) e.Node.Height = 30;
    }
    
    private void ShowNodeEditor(Node node)
    {
        // Implementation
    }
    
    private void DeleteSelectedNodes()
    {
        foreach (Node node in diagram.Model.SelectedNodes.ToList())
        {
            diagram.Model.RemoveChild(node);
        }
    }
    
    private void DuplicateSelectedNodes()
    {
        diagram.Controller.Copy();
        diagram.Controller.Paste();
    }
}
```

## Next Steps

- Configure view settings in [view-controls.md](view-controls.md)
- Apply automatic layouts in [layout-management.md](layout-management.md)
- Explore advanced features in [advanced-features.md](advanced-features.md)
- Review common issues in [troubleshooting.md](troubleshooting.md)
