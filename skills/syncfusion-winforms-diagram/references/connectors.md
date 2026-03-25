# Connectors and Links

## Table of Contents
- [Overview](#overview)
- [Connector Types](#connector-types)
- [Creating Connectors](#creating-connectors)
- [Decorators (Arrows)](#decorators-arrows)
- [Connecting Nodes](#connecting-nodes)
- [Connector Styling](#connector-styling)
- [Line Bridging](#line-bridging)
- [Line Routing](#line-routing)
- [Rounded Corners](#rounded-corners)
- [Best Practices](#best-practices)

## Overview

Connectors (also called links) are used to establish visual connections between nodes in a diagram. Essential Diagram provides multiple connector types, each optimized for different diagramming scenarios.

**Common Use Cases:**
- Flowchart connections
- Organizational hierarchy lines
- Network connections
- Data flow indicators
- Relationship mappings

## Connector Types

### LineConnector
Straight line connection between two points.

**Best for:** Simple, direct connections, minimal diagrams

```csharp
LineConnector line = new LineConnector(
    new PointF(100, 100),  // Start point
    new PointF(300, 200)   // End point
);
```

### OrthogonalConnector
90-degree angle connections (horizontal and vertical segments only).

**Best for:** Flowcharts, organizational charts, circuit diagrams

```csharp
OrthogonalConnector ortho = new OrthogonalConnector(
    new PointF(100, 100),
    new PointF(300, 300)
);
```

### DirectedLinesConnector
Smart routed connector that automatically finds the best path.

**Best for:** Complex diagrams with many nodes, automatic routing

```csharp
DirectedLinesConnector directed = new DirectedLinesConnector(
    new PointF(100, 100),
    new PointF(300, 300)
);
```

### PolyLineConnector
Multiple straight line segments with custom points.

**Best for:** Custom paths, manual routing control

```csharp
PolyLineConnector polyline = new PolyLineConnector(
    new PointF(100, 100),
    new PointF(300, 300)
);

// Add intermediate points
polyline.AddPoint(new PointF(200, 150));
polyline.AddPoint(new PointF(250, 250));
```

### BezierCurve
Smooth curved connections using Bezier mathematics.

**Best for:** Organic, flowing diagrams, mind maps

```csharp
BezierCurve bezier = new BezierCurve(
    new PointF(100, 100),
    new PointF(300, 300)
);
```

### SplineConnector
Smooth curve through multiple points.

**Best for:** Natural-looking curved paths

```csharp
SplineNode spline = new SplineNode(new PointF[] {
    new PointF(100, 100),
    new PointF(150, 200),
    new PointF(250, 180),
    new PointF(300, 300)
});
```

### CurveConnector
Curved connection with control points.

**Best for:** Smooth, customizable curves

```csharp
CurveNode curve = new CurveNode(new PointF[] {
    new PointF(100, 100),
    new PointF(200, 150),
    new PointF(300, 300)
});
```

### OrgLineConnector
Hierarchical connector for organizational charts.

**Best for:** Org charts, tree structures

```csharp
OrgLineConnector orgLine = new OrgLineConnector(
    new PointF(100, 100),
    new PointF(200, 300)
);
```

## Creating Connectors

### Basic LineConnector

```csharp
using Syncfusion.Windows.Forms.Diagram;

// Create two nodes
Ellipse ellipse = new Ellipse(160, 60, 100, 60);
Rectangle rectangle = new Rectangle(150, 250, 120, 100);

diagram1.Model.AppendChild(ellipse);
diagram1.Model.AppendChild(rectangle);

// Create line connector
LineConnector lineConnector = new LineConnector(
    new PointF(10, 200),
    new PointF(300, 250)
);

// Connect to nodes
ellipse.CentralPort.TryConnect(lineConnector.TailEndPoint);
rectangle.CentralPort.TryConnect(lineConnector.HeadEndPoint);

// Add to model
diagram1.Model.AppendChild(lineConnector);
```

### OrthogonalConnector with Nodes

```csharp
// Create start and end nodes
Rectangle start = new Rectangle(50, 50, 100, 70);
Rectangle end = new Rectangle(300, 250, 100, 70);

diagram1.Model.AppendChild(start);
diagram1.Model.AppendChild(end);

// Create orthogonal connector
OrthogonalConnector connector = new OrthogonalConnector(
    start.PinPoint,
    end.PinPoint
);

// Connect via ports
start.CentralPort.TryConnect(connector.TailEndPoint);
end.CentralPort.TryConnect(connector.HeadEndPoint);

diagram1.Model.AppendChild(connector);
```

### VB.NET Example

```vb
'Create nodes
Dim ellipse As New Ellipse(160, 60, 100, 60)
Dim rectangle As New Rectangle(150, 250, 120, 100)

diagram1.Model.AppendChild(ellipse)
diagram1.Model.AppendChild(rectangle)

'Create connector
Dim lineConnector As New LineConnector(
    New PointF(10, 200),
    New PointF(300, 250)
)

'Connect to nodes
ellipse.CentralPort.TryConnect(lineConnector.TailEndPoint)
rectangle.CentralPort.TryConnect(lineConnector.HeadEndPoint)

diagram1.Model.AppendChild(lineConnector)
```

## Decorators (Arrows)

Decorators are visual markers (arrows, diamonds, circles) at the head and tail of connectors.

### Available Decorator Shapes

- **Arrows:** `Open45Arrow`, `Open60Arrow`, `Filled45Arrow`, `Filled60Arrow`
- **Fancy Arrows:** `OpenFancyArrow`, `FilledFancyArrow`
- **Double:** `DoubleArrow`, `ReverseDoubleArrow`
- **Geometric:** `Diamond`, `FilledDiamond`, `Square`, `FilledSquare`, `Circle`, `FilledCircle`
- **Special:** `Cross45`, `Cross90`, `DoubleCross`, `ReverseArrow`
- **Compound:** `CircleCross`, `CircleReverseArrow`, `CrossReverseArrow`
- **Custom:** `Custom`, `DimensionLine`
- **None:** `None`

### Adding Head Decorator (Arrow)

```csharp
LineConnector connector = new LineConnector(
    new PointF(100, 100),
    new PointF(300, 300)
);

// Set head decorator shape
connector.HeadDecorator.DecoratorShape = DecoratorShape.Filled45Arrow;

// Set size
connector.HeadDecorator.Size = new SizeF(10, 5);

// Set color
connector.HeadDecorator.FillStyle.Color = Color.MidnightBlue;
connector.HeadDecorator.LineStyle.LineColor = Color.MidnightBlue;
```

### Adding Tail Decorator

```csharp
// Set tail decorator
connector.TailDecorator.DecoratorShape = DecoratorShape.FilledCircle;
connector.TailDecorator.Size = new SizeF(8, 8);
connector.TailDecorator.FillStyle.Color = Color.Red;
```

### Bidirectional Arrow

```csharp
// Arrows on both ends
connector.HeadDecorator.DecoratorShape = DecoratorShape.Filled45Arrow;
connector.TailDecorator.DecoratorShape = DecoratorShape.Filled45Arrow;
```

### Diamond Decorator (for UML)

```csharp
// Diamond at tail, arrow at head (composition)
connector.TailDecorator.DecoratorShape = DecoratorShape.FilledDiamond;
connector.TailDecorator.Size = new SizeF(12, 12);

connector.HeadDecorator.DecoratorShape = DecoratorShape.Open45Arrow;
```

## Connecting Nodes

### Using Central Port

```csharp
// Enable central port on nodes
Rectangle node1 = new Rectangle(100, 100, 100, 60);
Rectangle node2 = new Rectangle(300, 200, 100, 60);

node1.EnableCentralPort = true;
node2.EnableCentralPort = true;

diagram1.Model.AppendChild(node1);
diagram1.Model.AppendChild(node2);

// Create connector
OrthogonalConnector link = new OrthogonalConnector(
    node1.PinPoint,
    node2.PinPoint
);

// Connect via central ports
node1.CentralPort.TryConnect(link.TailEndPoint);
node2.CentralPort.TryConnect(link.HeadEndPoint);

diagram1.Model.AppendChild(link);
```

### Using Custom Ports

```csharp
// Create node with custom ports
Rectangle node = new Rectangle(100, 100, 100, 60);
node.DrawPorts = true;

// Add custom port
ConnectionPoint port = new ConnectionPoint();
port.OffsetX = 100; // Right edge
port.OffsetY = 30;  // Middle height
node.Ports.Add(port);

diagram1.Model.AppendChild(node);

// Connect to custom port
LineConnector connector = new LineConnector(
    node.PinPoint,
    new PointF(300, 300)
);

port.TryConnect(connector.TailEndPoint);
diagram1.Model.AppendChild(connector);
```

### Connection Point Types

```csharp
ConnectionPoint port = new ConnectionPoint();

// Set connection type
port.ConnectionPointType = ConnectionPointType.Incoming;  // Only incoming
// Or: Outgoing, OutgoingIncoming (default)

// Limit number of connections
port.ConnectionsLimit = 5;

// Auto-create connector on drag
port.AllowConnectOnDrag = true;

node.Ports.Add(port);
```

## Connector Styling

### Line Properties

```csharp
OrthogonalConnector connector = new OrthogonalConnector(
    new PointF(100, 100),
    new PointF(300, 300)
);

// Line color and width
connector.LineStyle.LineColor = Color.MidnightBlue;
connector.LineStyle.LineWidth = 2.5f;

// Line style
connector.LineStyle.DashStyle = DashStyle.Solid;  // Solid, Dash, Dot, DashDot

// Line cap (end style)
connector.LineStyle.LineCap = LineCap.Round;

// Line join (corner style)
connector.LineStyle.LineJoin = LineJoin.Round;

// Custom dash pattern
connector.LineStyle.DashPattern = new float[] { 5, 2, 3, 2 };
connector.LineStyle.DashOffset = 0;
```

### Complete Styled Connector

```csharp
LineConnector connector = new LineConnector(
    new PointF(100, 100),
    new PointF(300, 300)
);

// Line styling
connector.LineStyle.LineColor = Color.DarkBlue;
connector.LineStyle.LineWidth = 3f;
connector.LineStyle.DashStyle = DashStyle.Solid;

// Head decorator
connector.HeadDecorator.DecoratorShape = DecoratorShape.Filled45Arrow;
connector.HeadDecorator.Size = new SizeF(12, 8);
connector.HeadDecorator.FillStyle.Color = Color.DarkBlue;
connector.HeadDecorator.LineStyle.LineColor = Color.DarkBlue;

// Tail decorator
connector.TailDecorator.DecoratorShape = DecoratorShape.FilledCircle;
connector.TailDecorator.Size = new SizeF(8, 8);
connector.TailDecorator.FillStyle.Color = Color.DarkBlue;

diagram1.Model.AppendChild(connector);
```

## Rounded Corners

Make orthogonal connectors visually smoother with rounded corners.

```csharp
OrthogonalConnector connector = new OrthogonalConnector(
    new PointF(100, 100),
    new PointF(300, 300)
);

// Enable rounded corners
connector.EnableRoundedCorner = true;

// Set corner radius
connector.CurveRadius = 10;

diagram1.Model.AppendChild(connector);
```

**Also works for:**
- OrgLineConnector
- PolyLineConnector

```csharp
OrgLineConnector orgLine = new OrgLineConnector(
    new PointF(100, 100),
    new PointF(200, 300)
);

orgLine.EnableRoundedCorner = true;
orgLine.CurveRadius = 15;
```

## Line Bridging

Line bridging creates visual "jumps" where connectors cross, making diagram relationships clearer.

### Enabling Line Bridging

```csharp
// Enable for entire diagram
diagram1.Model.LineBridgingEnabled = true;

// Or enable for specific connector
LineConnector link = new LineConnector(
    new PointF(10, 10),
    new PointF(200, 200)
);
link.LineBridgingEnabled = true;

diagram1.Model.AppendChild(link);
```

### Configuring Bridge Appearance

```csharp
// Set bridge size
diagram1.Model.LineBridgeSize = 12;

// Set bridge style
diagram1.Model.BridgeStyle = BridgeStyle.Arc;  // Default
// Or: Gap, Square, Side2, Side3, Side4, Side5, Side6, Side7
```

**Bridge Styles:**
- **Arc** - Curved bridge (default)
- **Gap** - Simple gap in the line
- **Square** - Square bridge
- **Side2-Side7** - Angled bridges with different slopes

### Z-Order and Bridging

Connectors bridge over connectors with lower Z-order:

```csharp
LineConnector link1 = new LineConnector(...);
link1.ZOrder = 1;  // Lower - will be bridged over

LineConnector link2 = new LineConnector(...);
link2.ZOrder = 10; // Higher - will bridge over link1

link1.LineBridgingEnabled = true;
link2.LineBridgingEnabled = true;
```

## Line Routing

Automatic line routing avoids nodes in the connector's path.

### Enabling Line Routing

```csharp
// Enable for entire diagram
diagram1.Model.LineRoutingEnabled = true;

// Or enable for specific connector
OrthogonalConnector link = new OrthogonalConnector(
    new PointF(100, 100),
    new PointF(400, 400)
);
link.LineRoutingEnabled = true;
```

### Configuring Line Router

```csharp
// Set distance from obstacles
diagram1.Model.LineRouter.DistanceToObstacles = 20;

// Set routing mode
diagram1.Model.LineRouter.RoutingMode = RoutingMode.Automatic;
// Or: Inactive, SemiAutomatic
```

**Routing Modes:**
- **Inactive** - No automatic routing
- **Automatic** - Fully automatic routing around obstacles
- **SemiAutomatic** - Manual control with routing hints

### Marking Nodes as Obstacles

```csharp
Rectangle obstacle = new Rectangle(200, 200, 100, 80);

// Mark as obstacle for routing
obstacle.TreatAsObstacle = true;

diagram1.Model.AppendChild(obstacle);
```

### Complete Routing Example

```csharp
// Enable routing
diagram1.Model.LineRoutingEnabled = true;
diagram1.Model.LineRouter.DistanceToObstacles = 15;
diagram1.Model.LineRouter.RoutingMode = RoutingMode.Automatic;

// Create obstacle nodes
Rectangle node1 = new Rectangle(100, 100, 80, 60);
node1.TreatAsObstacle = true;
diagram1.Model.AppendChild(node1);

Rectangle node2 = new Rectangle(200, 150, 80, 60);
node2.TreatAsObstacle = true;
diagram1.Model.AppendChild(node2);

Rectangle node3 = new Rectangle(300, 100, 80, 60);
node3.TreatAsObstacle = true;
diagram1.Model.AppendChild(node3);

// Create connector - will route around obstacles
OrthogonalConnector link = new OrthogonalConnector(
    new PointF(50, 130),
    new PointF(450, 130)
);
link.LineRoutingEnabled = true;
diagram1.Model.AppendChild(link);
```

## Best Practices

### Choosing the Right Connector

```csharp
// Flowcharts: OrthogonalConnector
OrthogonalConnector flowLine = new OrthogonalConnector(...);

// Organizational charts: OrgLineConnector
OrgLineConnector orgLine = new OrgLineConnector(...);

// Simple diagrams: LineConnector
LineConnector simpleLine = new LineConnector(...);

// Complex routing: DirectedLinesConnector
DirectedLinesConnector smartLine = new DirectedLinesConnector(...);

// Artistic/organic: BezierCurve or SplineNode
BezierCurve curve = new BezierCurve(...);
```

### Connector Naming

```csharp
// Use descriptive names
connector.Name = "Link_Start_To_Process";
connector.Name = "Connection_Decision_To_End";

// Access later
Node found = diagram1.Model.FindNodeByName("Link_Start_To_Process");
```

### Performance with Many Connectors

```csharp
// Batch add connectors
diagram1.Model.BeginUpdate();

try
{
    for (int i = 0; i < 100; i++)
    {
        LineConnector link = new LineConnector(...);
        diagram1.Model.AppendChild(link);
    }
}
finally
{
    diagram1.Model.EndUpdate();
}
```

### Conditional Decorators

```csharp
private void AddConditionalLink(Node from, Node to, string condition)
{
    OrthogonalConnector link = new OrthogonalConnector(
        from.PinPoint,
        to.PinPoint
    );
    
    // Arrow at head
    link.HeadDecorator.DecoratorShape = DecoratorShape.Filled45Arrow;
    
    // Add condition label
    Label label = new Label();
    label.Text = condition;
    label.Position = Position.MiddleCenter;
    link.Labels.Add(label);
    
    from.CentralPort.TryConnect(link.TailEndPoint);
    to.CentralPort.TryConnect(link.HeadEndPoint);
    
    diagram1.Model.AppendChild(link);
}

// Usage
AddConditionalLink(decisionNode, yesPath, "Yes");
AddConditionalLink(decisionNode, noPath, "No");
```

### Connector Templates

```csharp
// Create reusable connector factory
private OrthogonalConnector CreateStyledConnector(PointF start, PointF end)
{
    OrthogonalConnector connector = new OrthogonalConnector(start, end);
    
    connector.LineStyle.LineColor = Color.DarkBlue;
    connector.LineStyle.LineWidth = 2;
    connector.EnableRoundedCorner = true;
    connector.CurveRadius = 8;
    
    connector.HeadDecorator.DecoratorShape = DecoratorShape.Filled45Arrow;
    connector.HeadDecorator.Size = new SizeF(10, 6);
    connector.HeadDecorator.FillStyle.Color = Color.DarkBlue;
    
    return connector;
}

// Usage
OrthogonalConnector link1 = CreateStyledConnector(
    node1.PinPoint,
    node2.PinPoint
);
diagram1.Model.AppendChild(link1);
```

## Common Issues

### Issue: Connector Not Visible

**Solution:** Ensure both endpoints are set and connector is added to model
```csharp
connector.HeadEndPoint.SetPinPoint(new PointF(300, 300));
connector.TailEndPoint.SetPinPoint(new PointF(100, 100));
diagram1.Model.AppendChild(connector);
diagram1.UpdateView();
```

### Issue: Connector Doesn't Move with Node

**Solution:** Connect via ports, not fixed coordinates
```csharp
// Wrong: Fixed coordinates
connector.HeadEndPoint.SetPinPoint(new PointF(300, 300));

// Correct: Connected to node port
node.CentralPort.TryConnect(connector.HeadEndPoint);
```

### Issue: Line Routing Not Working

**Solution:** Enable routing and mark nodes as obstacles
```csharp
diagram1.Model.LineRoutingEnabled = true;
connector.LineRoutingEnabled = true;
obstacleNode.TreatAsObstacle = true;
```

## Next Steps

- Learn about interactive drawing in [drawing-tools.md](drawing-tools.md)
- Add labels to connectors in [labels-ports.md](labels-ports.md)
- Configure ports in [labels-ports.md](labels-ports.md#ports)
- Style your diagram in [view-controls.md](view-controls.md)
