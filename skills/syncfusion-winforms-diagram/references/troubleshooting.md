# Troubleshooting

Common issues and solutions for Syncfusion Windows Forms Diagram.

## Table of Contents
- [Rendering Issues](#rendering-issues)
- [Performance Problems](#performance-problems)
- [Connection Issues](#connection-issues)
- [Serialization Errors](#serialization-errors)
- [Layout Problems](#layout-problems)
- [Event Handling](#event-handling)

## Rendering Issues

### Nodes Not Visible

**Problem**: Nodes added to diagram but not displaying.

**Solutions**:
```csharp
// Solution 1: Call UpdateView after adding nodes
Rectangle node = new Rectangle(100, 100, 100, 60);
diagram1.Model.AppendChild(node);
diagram1.UpdateView(); // Don't forget this!

// Solution 2: Check if node is within view bounds
diagram1.View.ScrollVirtualBounds(node.BoundingRectangle);

// Solution 3: Verify node visibility
node.Visible = true;

// Solution 4: Check Z-order
int zOrder = diagram1.Model.GetZOrder(node);
Console.WriteLine($"Z-Order: {zOrder}");
```

### Blurry or Pixelated Graphics

**Problem**: Diagram elements appear blurry or pixelated.

**Solutions**:
```csharp
// Solution 1: Enable anti-aliasing
diagram1.View.SmoothingMode = SmoothingMode.AntiAlias;
diagram1.View.TextRenderingHint = TextRenderingHint.AntiAlias;

// Solution 2: Check DPI settings
Graphics g = diagram1.CreateGraphics();
float dpiX = g.DpiX;
float dpiY = g.DpiY;
Console.WriteLine($"DPI: {dpiX} x {dpiY}");
g.Dispose();

// Adjust for high DPI
if (dpiX > 96)
{
    diagram1.View.ScaleFactor = dpiX / 96f;
}

// Solution 3: Use vector graphics instead of bitmaps
// Prefer DrawPath/DrawRectangle over DrawImage when possible
```

### Grid Not Showing

**Problem**: Grid is enabled but not visible.

**Solutions**:
```csharp
// Solution 1: Verify grid is enabled
diagram1.Model.RenderStyle.ShowGrid = true;

// Solution 2: Check grid color contrast
diagram1.Model.GridStyle.GridColor = Color.LightGray;
diagram1.BackColor = Color.White; // Ensure contrast

// Solution 3: Verify grid spacing
diagram1.Model.GridStyle.MajorGrid = 50;
diagram1.Model.GridStyle.MinorGrid = 10;

// Solution 4: Check zoom level
// Grid may not be visible at very low zoom
if (diagram1.View.ZoomFactor < 0.2f)
{
    diagram1.View.ZoomFactor = 1.0f;
}

// Solution 5: Force redraw
diagram1.Invalidate();
diagram1.UpdateView();
```

### Labels Not Displaying

**Problem**: Labels added to nodes but not showing.

**Solutions**:
```csharp
// Solution 1: Verify label text is set
Label label = new Label();
label.Text = "My Label"; // Must set text!
label.Position = Position.Center;
node.Labels.Add(label);

// Solution 2: Check font size
label.FontStyle.Size = 10; // Ensure reasonable size

// Solution 3: Check label color
label.FontColorStyle.Color = Color.Black;
// Make sure it contrasts with background

// Solution 4: Verify label position
label.Position = Position.Center; // Or other valid position

// Solution 5: Check node size
// Label may be clipped if node is too small
node.Size = new SizeF(100, 60);
```

## Performance Problems

### Slow Rendering with Many Nodes

**Problem**: Diagram becomes slow with 100+ nodes.

**Solutions**:
```csharp
// Solution 1: Use batch updates
diagram1.Model.BeginUpdate();
try
{
    for (int i = 0; i < 1000; i++)
    {
        Rectangle node = new Rectangle(i * 20, i * 20, 50, 30);
        diagram1.Model.AppendChild(node);
    }
}
finally
{
    diagram1.Model.EndUpdate();
}

// Solution 2: Enable virtual rendering
diagram1.View.EnableVirtualRendering = true;

// Solution 3: Disable anti-aliasing during operations
diagram1.View.SmoothingMode = SmoothingMode.None;
// Re-enable after
diagram1.View.SmoothingMode = SmoothingMode.AntiAlias;

// Solution 4: Simplify shapes
// Use basic shapes instead of complex custom shapes

// Solution 5: Disable history during bulk operations
diagram1.Model.HistoryManager.Enabled = false;
// ... add nodes ...
diagram1.Model.HistoryManager.Enabled = true;
```

### Lag During Dragging

**Problem**: Nodes lag when dragging.

**Solutions**:
```csharp
// Solution 1: Disable anti-aliasing during drag
diagram1.Model.NodeDragStart += (s, e) =>
{
    diagram1.View.SmoothingMode = SmoothingMode.None;
};

diagram1.Model.NodeDragEnd += (s, e) =>
{
    diagram1.View.SmoothingMode = SmoothingMode.AntiAlias;
};

// Solution 2: Reduce connector segments during drag
diagram1.Model.NodeDragging += (s, e) =>
{
    // Simplify connector rendering
};

// Solution 3: Disable snap during drag if not needed
bool snapEnabled = diagram1.Model.SnapToGrid;
diagram1.Model.NodeDragStart += (s, e) =>
{
    diagram1.Model.SnapToGrid = false;
};

diagram1.Model.NodeDragEnd += (s, e) =>
{
    diagram1.Model.SnapToGrid = snapEnabled;
};
```

### Memory Leaks

**Problem**: Memory usage grows over time.

**Solutions**:
```csharp
// Solution 1: Properly dispose nodes when removing
foreach (Node node in nodesToRemove)
{
    diagram1.Model.RemoveChild(node);
    node.Dispose(); // Explicitly dispose
}

// Solution 2: Clear history periodically
if (diagram1.Model.HistoryManager.MaxHistoryCount > 100)
{
    diagram1.Model.HistoryManager.Clear();
}

// Solution 3: Dispose images in custom nodes
public class ImageNode : Node
{
    private Image image;
    
    protected override void Dispose(bool disposing)
    {
        if (disposing && image != null)
        {
            image.Dispose();
            image = null;
        }
        base.Dispose(disposing);
    }
}

// Solution 4: Unsubscribe from events
diagram1.Model.NodeAdded -= OnNodeAdded;
diagram1.Model.NodeRemoved -= OnNodeRemoved;
```

## Connection Issues

### Connectors Not Attaching to Nodes

**Problem**: Connectors appear but don't stay attached when nodes move.

**Solutions**:
```csharp
// Solution 1: Use TryConnect to attach properly
Rectangle node1 = new Rectangle(100, 100, 100, 60);
Rectangle node2 = new Rectangle(300, 200, 100, 60);
diagram1.Model.AppendChild(node1);
diagram1.Model.AppendChild(node2);

OrthogonalConnector connector = new OrthogonalConnector(
    node1.PinPoint,
    node2.PinPoint
);
diagram1.Model.AppendChild(connector);

// Attach to nodes
node1.CentralPort.TryConnect(connector.TailEndPoint);
node2.CentralPort.TryConnect(connector.HeadEndPoint);

// Solution 2: Enable central ports
node1.EnableCentralPort = true;
node2.EnableCentralPort = true;

// Solution 3: Check port visibility
node1.DrawPorts = true; // If using custom ports
```

### Orthogonal Connectors Have Wrong Angles

**Problem**: Orthogonal connectors don't maintain right angles.

**Solutions**:
```csharp
// Solution 1: Reset connector route
OrthogonalConnector connector = new OrthogonalConnector(start, end);
connector.UpdateRoute(); // Force recalculation

// Solution 2: Enable line routing
diagram1.Model.LineRoutingEnabled = true;

// Solution 3: Manually set intermediate points
OrthogonalConnector connector = new OrthogonalConnector(start, end);
connector.IntermediatePoints = new PointF[]
{
    new PointF(150, 100),
    new PointF(150, 200),
    new PointF(300, 200)
};

// Solution 4: Clear and recreate connector
diagram1.Model.RemoveChild(connector);
OrthogonalConnector newConnector = new OrthogonalConnector(
    node1.PinPoint,
    node2.PinPoint
);
diagram1.Model.AppendChild(newConnector);
node1.CentralPort.TryConnect(newConnector.TailEndPoint);
node2.CentralPort.TryConnect(newConnector.HeadEndPoint);
```

### Line Bridging Not Working

**Problem**: Connectors don't show bridges where they cross.

**Solutions**:
```csharp
// Solution 1: Enable line bridging
connector.EnableLineBridging = true;

// Solution 2: Set bridge type
connector.LineBridgeType = LineBridgeType.Arc;
// Or: Triangle, Rectangle

// Solution 3: Set bridge size
connector.LineBridgeSize = new SizeF(10, 10);

// Solution 4: Force redraw
diagram1.UpdateView();
```

## Serialization Errors

### Deserialization Fails

**Problem**: Loading saved diagram throws exception.

**Solutions**:
```csharp
// Solution 1: Add AssemblyResolve handler
AppDomain.CurrentDomain.AssemblyResolve += CurrentDomain_AssemblyResolve;

private Assembly CurrentDomain_AssemblyResolve(object sender, ResolveEventArgs args)
{
    // Resolve Syncfusion assemblies
    if (args.Name.Contains("Syncfusion.Diagram"))
    {
        return typeof(Diagram).Assembly;
    }
    return null;
}

// Solution 2: Mark custom classes as Serializable
[Serializable]
public class CustomNode : Rectangle
{
    // ...
}

// Solution 3: Handle version mismatches
try
{
    using (FileStream stream = new FileStream(filePath, FileMode.Open))
    {
        BinaryFormatter formatter = new BinaryFormatter();
        formatter.Binder = new VersionDeserializationBinder();
        DiagramModel model = (DiagramModel)formatter.Deserialize(stream);
        diagram1.Model = model;
    }
}
catch (SerializationException ex)
{
    MessageBox.Show($"Unable to load diagram: {ex.Message}");
}

public class VersionDeserializationBinder : SerializationBinder
{
    public override Type BindToType(string assemblyName, string typeName)
    {
        // Handle version changes
        Type typeToDeserialize = null;
        string currentAssembly = Assembly.GetExecutingAssembly().FullName;
        assemblyName = currentAssembly;
        typeToDeserialize = Type.GetType($"{typeName}, {assemblyName}");
        return typeToDeserialize;
    }
}
```

### Custom Properties Not Saved

**Problem**: Custom node properties lost after save/load.

**Solutions**:
```csharp
// Solution 1: Use Tag property for simple data
node.Tag = new { Category = "Process", Priority = 1 };

// Solution 2: Create serializable custom node class
[Serializable]
public class CustomNode : Rectangle
{
    public string Category { get; set; }
    public int Priority { get; set; }
    
    public CustomNode(float x, float y, float width, float height)
        : base(x, y, width, height)
    {
    }
}

// Solution 3: Implement ISerializable for complex data
[Serializable]
public class AdvancedNode : Rectangle, ISerializable
{
    private Dictionary<string, object> customData;
    
    public AdvancedNode(float x, float y, float width, float height)
        : base(x, y, width, height)
    {
        customData = new Dictionary<string, object>();
    }
    
    protected AdvancedNode(SerializationInfo info, StreamingContext context)
        : base(info, context)
    {
        customData = (Dictionary<string, object>)info.GetValue("CustomData", typeof(Dictionary<string, object>));
    }
    
    public override void GetObjectData(SerializationInfo info, StreamingContext context)
    {
        base.GetObjectData(info, context);
        info.AddValue("CustomData", customData);
    }
}
```

## Layout Problems

### Hierarchical Layout Creates Overlaps

**Problem**: Nodes overlap after applying hierarchical layout.

**Solutions**:
```csharp
// Solution 1: Increase spacing
HierarchicalLayoutManager layout = new HierarchicalLayoutManager(
    diagram1.Model,
    diagram1.View
);
layout.VerticalSpacing = 80;  // Increase from default
layout.HorizontalSpacing = 50;
layout.LayoutNodes();

// Solution 2: Set proper bounds
layout.Bounds = new RectangleF(0, 0, 2000, 1500); // Large enough

// Solution 3: Check for cycles in connections
// Hierarchical layout doesn't work well with cyclic graphs

// Solution 4: Use different layout for non-tree structures
if (HasCycles(diagram1.Model))
{
    SymmetricLayoutManager symLayout = new SymmetricLayoutManager(
        diagram1.Model,
        diagram1.View
    );
    symLayout.LayoutNodes();
}
```

### Layout Doesn't Update After Changes

**Problem**: Layout doesn't recalculate when nodes are added.

**Solutions**:
```csharp
// Solution 1: Manually reapply layout
private void AddNodeAndRelayout(Node newNode)
{
    diagram1.Model.AppendChild(newNode);
    
    // Reapply layout
    HierarchicalLayoutManager layout = new HierarchicalLayoutManager(
        diagram1.Model,
        diagram1.View
    );
    layout.LayoutNodes();
    diagram1.UpdateView();
}

// Solution 2: Auto-relayout on node added
diagram1.Model.NodeAdded += (s, e) =>
{
    if (autoLayoutEnabled)
    {
        ApplyLayout();
    }
};

// Solution 3: Batch add then layout
diagram1.Model.BeginUpdate();
// Add multiple nodes
diagram1.Model.EndUpdate();
ApplyLayout();
```

## Event Handling

### Events Not Firing

**Problem**: NodeSelected or other events don't trigger.

**Solutions**:
```csharp
// Solution 1: Verify event subscription
diagram1.Model.NodeSelected += Diagram_NodeSelected;

private void Diagram_NodeSelected(NodeEventArgs evtArgs)
{
    Console.WriteLine($"Selected: {evtArgs.Node.Name}");
}

// Solution 2: Check if events are suppressed
diagram1.Model.EventsEnabled = true;

// Solution 3: Verify node is selectable
node.EnableSelection = true;

// Solution 4: Use correct event signature
// Wrong:
// diagram1.Model.NodeSelected += (s, e) => { }; // Wrong parameters

// Correct:
diagram1.Model.NodeSelected += (evtArgs) =>
{
    Console.WriteLine($"Selected: {evtArgs.Node.Name}");
};
```

### MouseDown Not Working on Nodes

**Problem**: Mouse events don't fire when clicking nodes.

**Solutions**:
```csharp
// Solution 1: Use diagram MouseDown, not form MouseDown
diagram1.MouseDown += Diagram_MouseDown;

private void Diagram_MouseDown(object sender, MouseEventArgs e)
{
    Node node = diagram1.Controller.HitTest(e.Location);
    if (node != null)
    {
        Console.WriteLine($"Clicked: {node.Name}");
    }
}

// Solution 2: Check if tool is intercepting
// SelectTool may handle mouse events
// Consider tool priorities

// Solution 3: Use node-specific events
diagram1.Model.NodeClicked += (evtArgs) =>
{
    Console.WriteLine($"Node clicked: {evtArgs.Node.Name}");
};
```

## Best Practices

### Debugging Checklist

```csharp
public class DiagramDebugHelper
{
    public static void DiagnoseIssues(Diagram diagram)
    {
        Console.WriteLine("=== Diagram Diagnostics ===");
        
        // Model info
        Console.WriteLine($"Nodes: {diagram.Model.Nodes.Count}");
        Console.WriteLine($"Connectors: {diagram.Model.Connectors.Count}");
        
        // View info
        Console.WriteLine($"Zoom: {diagram.View.ZoomFactor * 100}%");
        Console.WriteLine($"Scroll: {diagram.View.ScrollPosition}");
        
        // Render settings
        Console.WriteLine($"Grid visible: {diagram.Model.RenderStyle.ShowGrid}");
        Console.WriteLine($"Anti-aliasing: {diagram.View.SmoothingMode}");
        
        // Check invisible nodes
        int invisibleCount = 0;
        foreach (Node node in diagram.Model.Nodes)
        {
            if (!node.Visible)
                invisibleCount++;
        }
        Console.WriteLine($"Invisible nodes: {invisibleCount}");
        
        // Bounds info
        RectangleF bounds = diagram.Model.BoundingRectangle;
        Console.WriteLine($"Bounds: {bounds}");
        
        // Performance
        Console.WriteLine($"History count: {diagram.Model.HistoryManager.HistoryCount}");
        
        Console.WriteLine("========================");
    }
}

// Usage
DiagramDebugHelper.DiagnoseIssues(diagram1);
```

### Error Prevention

```csharp
// Always wrap diagram modifications in try-catch
try
{
    diagram1.Model.BeginUpdate();
    
    // Modifications
    
    diagram1.Model.EndUpdate();
    diagram1.UpdateView();
}
catch (Exception ex)
{
    diagram1.Model.EndUpdate(); // Ensure update ends
    Console.WriteLine($"Error: {ex.Message}");
    MessageBox.Show($"An error occurred: {ex.Message}");
}

// Validate before operations
if (diagram1.Model.SelectedNodes.Count == 0)
{
    MessageBox.Show("Please select a node first.");
    return;
}

// Check for null references
Node node = diagram1.Controller.HitTest(e.Location);
if (node != null)
{
    // Safe to use node
}
```

## Getting Help

If issues persist:

1. **Check Documentation**: Review the official Syncfusion documentation
2. **Sample Code**: Look at Syncfusion sample projects
3. **Support Forum**: Post on Syncfusion community forums
4. **Contact Support**: Reach out to Syncfusion support (license holders)

## Next Steps

- Return to main guide: [SKILL.md](../SKILL.md)
- Review specific features in other reference files
- Explore official Syncfusion documentation
