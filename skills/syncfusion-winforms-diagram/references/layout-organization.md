# Layout and Organization

## Table of Contents
- [Overview](#overview)
- [Layers](#layers)
- [Grouping](#grouping)
- [Alignment Tools](#alignment-tools)
- [Z-Order Management](#z-order-management)
- [Spacing Tools](#spacing-tools)
- [Sizing Tools](#sizing-tools)
- [Nudge Operations](#nudge-operations)

## Overview

Essential Diagram provides comprehensive tools for organizing and arranging diagram elements including layers, grouping, alignment, Z-order control, and distribution tools.

## Layers

Layers organize nodes into logical groups for visibility control, selection, and management.

### Creating Layers

```csharp
// Create new layer
Layer backgroundLayer = new Layer();
backgroundLayer.Name = "Background";
backgroundLayer.Visible = true;
backgroundLayer.Enabled = true;

// Add to model
diagram1.Model.Layers.Add(backgroundLayer);
```

### Setting Active Layer

```csharp
// Set active layer (new nodes added here)
diagram1.Model.ActiveLayer = backgroundLayer;

// Now nodes will be added to background layer
Rectangle rect = new Rectangle(50, 50, 100, 60);
diagram1.Model.AppendChild(rect); // Added to backgroundLayer
```

### Adding Nodes to Specific Layer

```csharp
// Create layer
Layer connectorLayer = new Layer();
connectorLayer.Name = "Connectors";
diagram1.Model.Layers.Add(connectorLayer);

// Add node directly to layer
LineConnector connector = new LineConnector(
    new PointF(100, 100),
    new PointF(300, 300)
);
connectorLayer.AppendChild(connector);
```

### Layer Visibility

```csharp
// Hide layer
backgroundLayer.Visible = false;
diagram1.Refresh();

// Show layer
backgroundLayer.Visible = true;
diagram1.Refresh();

// Toggle visibility
backgroundLayer.Visible = !backgroundLayer.Visible;
diagram1.Refresh();
```

### Layer Properties

```csharp
Layer layer = new Layer();

// Identification
layer.Name = "MyLayer";

// Visibility
layer.Visible = true;  // Show/hide all nodes in layer

// Interaction
layer.Enabled = true;  // Enable/disable user interaction
layer.Locked = false;  // Lock/unlock editing

// Print settings
layer.Printable = true;  // Include in printing
```

### Enumerating Layers

```csharp
// List all layers
foreach (Layer layer in diagram1.Model.Layers)
{
    Console.WriteLine($"Layer: {layer.Name}, Visible: {layer.Visible}, Nodes: {layer.Nodes.Count}");
}

// Find layer by name
Layer foundLayer = diagram1.Model.Layers
    .Cast<Layer>()
    .FirstOrDefault(l => l.Name == "Background");
```

### Moving Nodes Between Layers

```csharp
// Get node
Rectangle node = diagram1.Model.FindNodeByName("MyNode") as Rectangle;

// Move to different layer
Layer targetLayer = diagram1.Model.Layers
    .Cast<Layer>()
    .FirstOrDefault(l => l.Name = "NewLayer");
    
if (node != null && targetLayer != null)
{
    // Remove from current layer
    node.Parent.RemoveChild(node);
    
    // Add to target layer
    targetLayer.AppendChild(node);
    
    diagram1.UpdateView();
}
```

## Grouping

Group multiple nodes together to move, resize, and manipulate as a single unit.

### Creating Groups

```csharp
// Create nodes
Rectangle rect1 = new Rectangle(100, 100, 80, 60);
Rectangle rect2 = new Rectangle(200, 100, 80, 60);
Ellipse ellipse = new Ellipse(150, 180, 60, 60);

diagram1.Model.AppendChild(rect1);
diagram1.Model.AppendChild(rect2);
diagram1.Model.AppendChild(ellipse);

// Select nodes to group
diagram1.Model.SelectionList.Clear();
diagram1.Model.SelectionList.Add(rect1);
diagram1.Model.SelectionList.Add(rect2);
diagram1.Model.SelectionList.Add(ellipse);

// Group selected nodes
diagram1.Controller.Group();
```

### Ungrouping

```csharp
// Select group
Group group = diagram1.Model.FindNodeByName("Group1") as Group;
diagram1.Model.SelectionList.Clear();
diagram1.Model.SelectionList.Add(group);

// Ungroup
diagram1.Controller.UnGroup();
```

### Programmatic Grouping

```csharp
// Create group manually
Group group = new Group();
group.Name = "MyGroup";

// Add nodes to group
Rectangle node1 = new Rectangle(0, 0, 80, 60);
Rectangle node2 = new Rectangle(100, 0, 80, 60);

group.AppendChild(node1);
group.AppendChild(node2);

// Add group to model
diagram1.Model.AppendChild(group);

// Position group
group.PinPoint = new PointF(200, 200);
```

### Group Properties

```csharp
Group group = new Group();

// Container behavior
group.IsContainer = true;  // Act as container
group.FitToChildren = true;  // Auto-size to fit children

// Styling
group.FillStyle.Color = Color.LightGray;
group.LineStyle.LineColor = Color.DarkGray;
group.LineStyle.DashStyle = DashStyle.Dash;

// Padding
group.PaddingLeft = 10;
group.PaddingRight = 10;
group.PaddingTop = 10;
group.PaddingBottom = 10;
```

## Alignment Tools

Align multiple selected nodes relative to each other.

### Align Left

```csharp
// Select multiple nodes
diagram1.Model.SelectionList.Add(node1);
diagram1.Model.SelectionList.Add(node2);
diagram1.Model.SelectionList.Add(node3);

// Align left edges to first selected node
diagram1.AlignLeft();
```

### Align Center (Horizontal)

```csharp
// Align vertical centers
diagram1.AlignCenter();
```

### Align Right

```csharp
// Align right edges
diagram1.AlignRight();
```

### Align Top

```csharp
// Align top edges
diagram1.AlignTop();
```

### Align Middle (Vertical)

```csharp
// Align horizontal centers
diagram1.AlignMiddle();
```

### Align Bottom

```csharp
// Align bottom edges
diagram1.AlignBottom();
```

### Complete Alignment Example

```csharp
private void AlignSelectedNodes(AlignmentType alignment)
{
    if (diagram1.Model.SelectionList.Count < 2)
    {
        MessageBox.Show("Select at least 2 nodes to align");
        return;
    }
    
    switch (alignment)
    {
        case AlignmentType.Left:
            diagram1.AlignLeft();
            break;
        case AlignmentType.Center:
            diagram1.AlignCenter();
            break;
        case AlignmentType.Right:
            diagram1.AlignRight();
            break;
        case AlignmentType.Top:
            diagram1.AlignTop();
            break;
        case AlignmentType.Middle:
            diagram1.AlignMiddle();
            break;
        case AlignmentType.Bottom:
            diagram1.AlignBottom();
            break;
    }
    
    diagram1.UpdateView();
}

enum AlignmentType
{
    Left, Center, Right, Top, Middle, Bottom
}
```

## Z-Order Management

Control the stacking order of overlapping nodes.

### Bring to Front

```csharp
// Bring selected nodes to top of Z-order
diagram1.Controller.BringToFront();
```

### Send to Back

```csharp
// Send selected nodes to bottom of Z-order
diagram1.Controller.SendToBack();
```

### Bring Forward

```csharp
// Move selected nodes one level up
diagram1.Controller.BringForward();
```

### Send Backward

```csharp
// Move selected nodes one level down
diagram1.Controller.SendBackward();
```

### Programmatic Z-Order Control

```csharp
// Set Z-order directly
Rectangle node1 = new Rectangle(100, 100, 100, 80);
node1.ZOrder = 1;  // Lower number = further back

Rectangle node2 = new Rectangle(120, 120, 100, 80);
node2.ZOrder = 10; // Higher number = further forward

diagram1.Model.AppendChild(node1);
diagram1.Model.AppendChild(node2);
```

### Z-Order Example with Menu

```csharp
private void CreateZOrderMenu()
{
    ContextMenuStrip contextMenu = new ContextMenuStrip();
    
    ToolStripMenuItem bringToFront = new ToolStripMenuItem("Bring to Front");
    bringToFront.Click += (s, e) => diagram1.Controller.BringToFront();
    
    ToolStripMenuItem sendToBack = new ToolStripMenuItem("Send to Back");
    sendToBack.Click += (s, e) => diagram1.Controller.SendToBack();
    
    ToolStripMenuItem bringForward = new ToolStripMenuItem("Bring Forward");
    bringForward.Click += (s, e) => diagram1.Controller.BringForward();
    
    ToolStripMenuItem sendBackward = new ToolStripMenuItem("Send Backward");
    sendBackward.Click += (s, e) => diagram1.Controller.SendBackward();
    
    contextMenu.Items.Add(bringToFront);
    contextMenu.Items.Add(sendToBack);
    contextMenu.Items.Add(new ToolStripSeparator());
    contextMenu.Items.Add(bringForward);
    contextMenu.Items.Add(sendBackward);
    
    diagram1.ContextMenuStrip = contextMenu;
}
```

## Spacing Tools

Distribute selected nodes evenly.

### Space Across (Horizontal)

```csharp
// Select 3 or more nodes
diagram1.Model.SelectionList.Add(node1);
diagram1.Model.SelectionList.Add(node2);
diagram1.Model.SelectionList.Add(node3);

// Distribute evenly horizontally
diagram1.SpaceAcross();
```

### Space Down (Vertical)

```csharp
// Distribute evenly vertically
diagram1.SpaceDown();
```

### Custom Spacing

```csharp
private void ApplyCustomSpacing(float spacing, bool horizontal)
{
    if (diagram1.Model.SelectionList.Count < 2)
        return;
    
    // Sort nodes by position
    var nodes = diagram1.Model.SelectionList
        .Cast<Node>()
        .OrderBy(n => horizontal ? n.PinPoint.X : n.PinPoint.Y)
        .ToList();
    
    // Apply spacing
    for (int i = 1; i < nodes.Count; i++)
    {
        Node prev = nodes[i - 1];
        Node current = nodes[i];
        
        if (horizontal)
        {
            float newX = prev.PinPoint.X + prev.Width / 2 + spacing + current.Width / 2;
            current.PinPoint = new PointF(newX, current.PinPoint.Y);
        }
        else
        {
            float newY = prev.PinPoint.Y + prev.Height / 2 + spacing + current.Height / 2;
            current.PinPoint = new PointF(current.PinPoint.X, newY);
        }
    }
    
    diagram1.UpdateView();
}

// Usage
ApplyCustomSpacing(50, true);  // 50 pixels horizontal spacing
ApplyCustomSpacing(30, false); // 30 pixels vertical spacing
```

## Sizing Tools

Make selected nodes the same size.

### Same Size

```csharp
// Make all selected nodes same width and height as first
diagram1.SameSize();
```

### Same Width

```csharp
// Make all selected nodes same width as first
diagram1.SameWidth();
```

### Same Height

```csharp
// Make all selected nodes same height as first
diagram1.SameHeight();
```

### Sizing Example

```csharp
private void ApplySizing(SizingType sizingType)
{
    if (diagram1.Model.SelectionList.Count < 2)
    {
        MessageBox.Show("Select at least 2 nodes");
        return;
    }
    
    switch (sizingType)
    {
        case SizingType.SameSize:
            diagram1.SameSize();
            break;
        case SizingType.SameWidth:
            diagram1.SameWidth();
            break;
        case SizingType.SameHeight:
            diagram1.SameHeight();
            break;
    }
    
    diagram1.UpdateView();
}

enum SizingType
{
    SameSize, SameWidth, SameHeight
}
```

## Nudge Operations

Move selected nodes by small increments using keyboard or code.

### Nudge Increment

```csharp
// Set nudge distance (in diagram units)
diagram1.NudgeIncrement = 10; // 10 pixels
```

### Nudge Directions

```csharp
// Nudge up
diagram1.NudgeUp();

// Nudge down
diagram1.NudgeDown();

// Nudge left
diagram1.NudgeLeft();

// Nudge right
diagram1.NudgeRight();
```

### Keyboard Nudging

```csharp
protected override bool ProcessCmdKey(ref Message msg, Keys keyData)
{
    // Arrow keys for nudging
    switch (keyData)
    {
        case Keys.Up:
            diagram1.NudgeUp();
            return true;
            
        case Keys.Down:
            diagram1.NudgeDown();
            return true;
            
        case Keys.Left:
            diagram1.NudgeLeft();
            return true;
            
        case Keys.Right:
            diagram1.NudgeRight();
            return true;
            
        // Shift + Arrow for larger nudge
        case Keys.Shift | Keys.Up:
            diagram1.NudgeIncrement = 50;
            diagram1.NudgeUp();
            diagram1.NudgeIncrement = 10;
            return true;
            
        // Add other Shift combinations...
    }
    
    return base.ProcessCmdKey(ref msg, keyData);
}
```

### Custom Nudge

```csharp
private void NudgeNodes(float deltaX, float deltaY)
{
    foreach (Node node in diagram1.Model.SelectionList)
    {
        PointF current = node.PinPoint;
        node.PinPoint = new PointF(
            current.X + deltaX,
            current.Y + deltaY
        );
    }
    
    diagram1.UpdateView();
}

// Usage
NudgeNodes(5, 0);   // Move 5 pixels right
NudgeNodes(0, -10); // Move 10 pixels up
```

## Complete Organization Toolbar

```csharp
public class OrganizationToolbar
{
    private Diagram diagram;
    private ToolStrip toolStrip;
    
    public OrganizationToolbar(Diagram diagram)
    {
        this.diagram = diagram;
        CreateToolbar();
    }
    
    private void CreateToolbar()
    {
        toolStrip = new ToolStrip();
        
        // Alignment section
        AddButton("Align Left", diagram.AlignLeft);
        AddButton("Align Center", diagram.AlignCenter);
        AddButton("Align Right", diagram.AlignRight);
        toolStrip.Items.Add(new ToolStripSeparator());
        
        AddButton("Align Top", diagram.AlignTop);
        AddButton("Align Middle", diagram.AlignMiddle);
        AddButton("Align Bottom", diagram.AlignBottom);
        toolStrip.Items.Add(new ToolStripSeparator());
        
        // Spacing section
        AddButton("Space Across", diagram.SpaceAcross);
        AddButton("Space Down", diagram.SpaceDown);
        toolStrip.Items.Add(new ToolStripSeparator());
        
        // Sizing section
        AddButton("Same Size", diagram.SameSize);
        AddButton("Same Width", diagram.SameWidth);
        AddButton("Same Height", diagram.SameHeight);
        toolStrip.Items.Add(new ToolStripSeparator());
        
        // Z-Order section
        AddButton("Bring to Front", diagram.Controller.BringToFront);
        AddButton("Send to Back", diagram.Controller.SendToBack);
        AddButton("Bring Forward", diagram.Controller.BringForward);
        AddButton("Send Backward", diagram.Controller.SendBackward);
        toolStrip.Items.Add(new ToolStripSeparator());
        
        // Grouping section
        AddButton("Group", diagram.Controller.Group);
        AddButton("Ungroup", diagram.Controller.UnGroup);
    }
    
    private void AddButton(string text, Action action)
    {
        ToolStripButton button = new ToolStripButton(text);
        button.Click += (s, e) =>
        {
            action();
            diagram.UpdateView();
        };
        toolStrip.Items.Add(button);
    }
    
    public ToolStrip GetToolStrip()
    {
        return toolStrip;
    }
}

// Usage
OrganizationToolbar toolbar = new OrganizationToolbar(diagram1);
this.Controls.Add(toolbar.GetToolStrip());
```

## Best Practices

### Layer Management Strategy

```csharp
// Create standard layers for organization
private void InitializeStandardLayers()
{
    // Background layer (grids, watermarks)
    Layer background = new Layer() { Name = "Background", ZOrder = 0 };
    diagram1.Model.Layers.Add(background);
    
    // Content layer (main diagram elements)
    Layer content = new Layer() { Name = "Content", ZOrder = 10 };
    diagram1.Model.Layers.Add(content);
    
    // Connector layer (all links)
    Layer connectors = new Layer() { Name = "Connectors", ZOrder = 5 };
    diagram1.Model.Layers.Add(connectors);
    
    // Annotation layer (labels, notes)
    Layer annotations = new Layer() { Name = "Annotations", ZOrder = 20 };
    diagram1.Model.Layers.Add(annotations);
    
    // Set default active layer
    diagram1.Model.ActiveLayer = content;
}
```

### Selection Validation

```csharp
private bool ValidateSelection(int minCount, string operation)
{
    if (diagram1.Model.SelectionList.Count < minCount)
    {
        MessageBox.Show(
            $"Please select at least {minCount} node(s) for {operation}",
            "Invalid Selection",
            MessageBoxButtons.OK,
            MessageBoxIcon.Information
        );
        return false;
    }
    return true;
}

// Usage
if (ValidateSelection(2, "alignment"))
{
    diagram1.AlignLeft();
}
```

## Next Steps

- Configure labels and ports in [labels-ports.md](labels-ports.md)
- Learn about advanced features in [advanced-features.md](advanced-features.md)
- Explore view controls in [view-controls.md](view-controls.md)
- Handle user interaction in [user-interaction.md](user-interaction.md)
