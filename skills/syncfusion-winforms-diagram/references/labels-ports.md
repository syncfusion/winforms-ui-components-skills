# Labels and Ports

## Table of Contents
- [Labels Overview](#labels-overview)
- [Adding Labels](#adding-labels)
- [Label Positioning](#label-positioning)
- [Label Styling](#label-styling)
- [Ports Overview](#ports-overview)
- [Central Port](#central-port)
- [Custom Ports](#custom-ports)
- [Port Configuration](#port-configuration)

## Labels Overview

Labels are text objects attached to nodes and connectors, positioned relative to their parent's coordinates. They support rich formatting and multiple positioning options.

**Common Use Cases:**
- Node names and descriptions
- Connector annotations (Yes/No decisions)
- Status indicators
- Measurements and dimensions

## Adding Labels

### Basic Label on Node

```csharp
// Create node
Rectangle node = new Rectangle(100, 100, 120, 80);

// Create label
Label label = new Label();
label.Text = "Process Step";
label.FontStyle.Family = "Arial";
label.FontStyle.Size = 12;
label.FontColorStyle.Color = Color.Black;

// Add label to node
node.Labels.Add(label);

diagram1.Model.AppendChild(node);
```

### Label with Positioning

```csharp
// Create label with predefined position
Label label = new Label(node, "MyLabel");
label.Text = "Top Center";
label.Position = Position.TopCenter;

node.Labels.Add(label);
```

### Multiple Labels on One Node

```csharp
Rectangle node = new Rectangle(100, 100, 150, 100);

// Title label at top
Label titleLabel = new Label();
titleLabel.Text = "Employee";
titleLabel.Position = Position.TopCenter;
titleLabel.FontStyle.Bold = true;

// Name label in center
Label nameLabel = new Label();
nameLabel.Text = "John Doe";
nameLabel.Position = Position.Center;

// ID label at bottom
Label idLabel = new Label();
idLabel.Text = "ID: 12345";
idLabel.Position = Position.BottomCenter;
idLabel.FontStyle.Size = 9;

node.Labels.Add(titleLabel);
node.Labels.Add(nameLabel);
node.Labels.Add(idLabel);
```

### Label on Connector

```csharp
OrthogonalConnector connector = new OrthogonalConnector(
    new PointF(100, 100),
    new PointF(300, 300)
);

// Add label to connector
Label label = new Label();
label.Text = "Data Flow";
label.Position = Position.MiddleCenter;
label.BackgroundStyle.Color = Color.White; // Background for readability

connector.Labels.Add(label);
diagram1.Model.AppendChild(connector);
```

## Label Positioning

### Predefined Positions

```csharp
// Position enumeration values
label.Position = Position.TopLeft;
label.Position = Position.TopCenter;
label.Position = Position.TopRight;
label.Position = Position.MiddleLeft;
label.Position = Position.Center;
label.Position = Position.MiddleRight;
label.Position = Position.BottomLeft;
label.Position = Position.BottomCenter;
label.Position = Position.BottomRight;
```

### Custom Position (Offset)

```csharp
Label label = new Label();
label.Text = "Custom Position";
label.Position = Position.Custom;

// Offset in percentage (0-100) relative to node size
label.OffsetX = 25;  // 25% from left
label.OffsetY = 50;  // 50% from top

node.Labels.Add(label);
```

### Absolute Position

```csharp
// Get label position in diagram coordinates
PointF position = label.GetPosition();
Console.WriteLine($"Label at: ({position.X}, {position.Y})");
```

### Auto-Update Position

```csharp
// Enable automatic position updates
label.UpdatePosition = true; // Default

// Disable to manually control position
label.UpdatePosition = false;
```

### Label Rotation

```csharp
// Rotate label independent of node
label.RotationAngle = 45f; // 45 degrees

// Keep label horizontal when node rotates
label.AdjustRotationAngle = true;
```

## Label Styling

### Font Properties

```csharp
Label label = new Label();
label.Text = "Styled Label";

// Font family and size
label.FontStyle.Family = "Segoe UI";
label.FontStyle.Size = 14;

// Font weight and style
label.FontStyle.Bold = true;
label.FontStyle.Italic = false;
label.FontStyle.Underline = false;
label.FontStyle.Strikeout = false;
```

### Text Color

```csharp
// Simple color
label.FontColorStyle.Color = Color.DarkBlue;

// Gradient color for text
label.FontColorStyle.Type = FillStyleType.LinearGradient;
label.FontColorStyle.Color = Color.Blue;
label.FontColorStyle.ForeColor = Color.LightBlue;
```

### Background Style

```csharp
// Solid background
label.BackgroundStyle.Type = FillStyleType.Solid;
label.BackgroundStyle.Color = Color.LightYellow;

// Transparent background
label.BackgroundStyle.Color = Color.Transparent;

// Gradient background
label.BackgroundStyle.Type = FillStyleType.LinearGradient;
label.BackgroundStyle.Color = Color.White;
label.BackgroundStyle.ForeColor = Color.LightGray;
```

### Text Alignment

```csharp
// Horizontal alignment
label.HorizontalAlignment = StringAlignment.Near;   // Left
label.HorizontalAlignment = StringAlignment.Center; // Center
label.HorizontalAlignment = StringAlignment.Far;    // Right

// Vertical alignment
label.VerticalAlignment = StringAlignment.Near;   // Top
label.VerticalAlignment = StringAlignment.Center; // Middle
label.VerticalAlignment = StringAlignment.Far;    // Bottom
```

### Text Formatting

```csharp
// Text wrapping
label.WrapText = true;
label.Size = new SizeF(100, 50); // Set size for wrapping

// Text case
label.TextCase = TextCases.Normal;
// Or: Upper, Lower, Title

// Text direction
label.DirectionRightToLeft = false;
label.DirectionVertical = false;
```

### Advanced Formatting Flags

```csharp
// No clipping (allow text overflow)
label.NoClip = false;

// Line limit (only complete lines)
label.LineLimit = false;

// Fit within black box
label.FitBlackBox = true;

// Measure trailing spaces
label.MeasureTrailingSpaces = false;
```

### Complete Styled Label Example

```csharp
Rectangle node = new Rectangle(100, 100, 150, 100);

Label label = new Label();
label.Text = "Important\nProcess";

// Font styling
label.FontStyle.Family = "Segoe UI";
label.FontStyle.Size = 14;
label.FontStyle.Bold = true;
label.FontColorStyle.Color = Color.White;

// Background
label.BackgroundStyle.Type = FillStyleType.LinearGradient;
label.BackgroundStyle.Color = Color.DarkBlue;
label.BackgroundStyle.ForeColor = Color.Blue;

// Alignment
label.HorizontalAlignment = StringAlignment.Center;
label.VerticalAlignment = StringAlignment.Center;
label.Position = Position.Center;

// Wrapping
label.WrapText = true;
label.Size = new SizeF(130, 80);

node.Labels.Add(label);
diagram1.Model.AppendChild(node);
```

## Ports Overview

Ports are connection points on nodes where connectors can attach. They enable precise control over where connections begin and end.

**Common Use Cases:**
- Network diagrams (input/output ports)
- Circuit diagrams (connection terminals)
- Flowcharts (specific entry/exit points)
- UML diagrams (interface connections)

## Central Port

Default connection point at the center of a node.

### Enabling Central Port

```csharp
// Enable central port (enabled by default)
Rectangle node = new Rectangle(100, 100, 100, 60);
node.EnableCentralPort = true;

diagram1.Model.AppendChild(node);
```

### Connecting via Central Port

```csharp
// Create two nodes
Rectangle node1 = new Rectangle(100, 100, 100, 60);
node1.EnableCentralPort = true;
diagram1.Model.AppendChild(node1);

Rectangle node2 = new Rectangle(300, 200, 100, 60);
node2.EnableCentralPort = true;
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

### Disabling Central Port

```csharp
// Disable central port (for custom ports only)
node.EnableCentralPort = false;
```

## Custom Ports

Create multiple connection points at specific positions on a node.

### Creating Custom Ports

```csharp
// Create node
Rectangle node = new Rectangle(100, 100, 100, 60);
node.DrawPorts = true; // Enable port rendering

// Create custom port
ConnectionPoint port = new ConnectionPoint();
port.OffsetX = 100; // Right edge (in pixels)
port.OffsetY = 30;  // Middle height

// Add port to node
node.Ports.Add(port);

diagram1.Model.AppendChild(node);
```

### Multiple Ports on Node

```csharp
Rectangle node = new Rectangle(100, 100, 120, 80);
node.DrawPorts = true;
node.EnableCentralPort = false; // Disable central port

// Top port
ConnectionPoint topPort = new ConnectionPoint();
topPort.OffsetX = 60;  // Center width
topPort.OffsetY = 0;   // Top edge
node.Ports.Add(topPort);

// Right port
ConnectionPoint rightPort = new ConnectionPoint();
rightPort.OffsetX = 120; // Right edge
rightPort.OffsetY = 40;  // Center height
node.Ports.Add(rightPort);

// Bottom port
ConnectionPoint bottomPort = new ConnectionPoint();
bottomPort.OffsetX = 60;  // Center width
bottomPort.OffsetY = 80;  // Bottom edge
node.Ports.Add(bottomPort);

// Left port
ConnectionPoint leftPort = new ConnectionPoint();
leftPort.OffsetX = 0;   // Left edge
leftPort.OffsetY = 40;  // Center height
node.Ports.Add(leftPort);

diagram1.Model.AppendChild(node);
```

### Connecting to Custom Port

```csharp
// Get port
ConnectionPoint port = node.Ports[0];

// Create connector
LineConnector connector = new LineConnector(
    node.PinPoint,
    new PointF(300, 300)
);

// Connect to custom port
port.TryConnect(connector.TailEndPoint);

diagram1.Model.AppendChild(connector);
```

## Port Configuration

### Port Visual Types

```csharp
ConnectionPoint port = new ConnectionPoint();

// Set visual appearance
port.VisualType = PortVisualType.XPort;        // X mark (default)
// Or:
port.VisualType = PortVisualType.CirclePort;   // Circle
port.VisualType = PortVisualType.TrianglePort; // Triangle
port.VisualType = PortVisualType.SquarePort;   // Square
port.VisualType = PortVisualType.RhombPort;    // Diamond
port.VisualType = PortVisualType.Custom;       // Custom shape

node.Ports.Add(port);
```

### Port Styling

```csharp
ConnectionPoint port = new ConnectionPoint();

// Fill style
port.FillStyle.Type = FillStyleType.Solid;
port.FillStyle.Color = Color.LightBlue;
port.FillStyle.ColorAlphaFactor = 180; // Semi-transparent

// Line style
port.LineStyle.LineColor = Color.DarkBlue;
port.LineStyle.LineWidth = 2;
port.LineStyle.DashStyle = DashStyle.Solid;

node.Ports.Add(port);
```

### Port Connection Types

```csharp
ConnectionPoint port = new ConnectionPoint();

// Set connection type
port.ConnectionPointType = ConnectionPointType.Incoming;     // Only incoming
port.ConnectionPointType = ConnectionPointType.Outgoing;     // Only outgoing
port.ConnectionPointType = ConnectionPointType.OutgoingIncoming; // Both (default)

node.Ports.Add(port);
```

### Connection Limits

```csharp
ConnectionPoint port = new ConnectionPoint();

// Limit number of connections
port.ConnectionsLimit = 1;  // Only 1 connection allowed
// Or: 5, 10 (default), int.MaxValue (unlimited)

node.Ports.Add(port);
```

### Auto-Connect on Drag

```csharp
ConnectionPoint port = new ConnectionPoint();

// Auto-create connector when hovering
port.AllowConnectOnDrag = true;

node.Ports.Add(port);
```

### Port Size

```csharp
ConnectionPoint port = new ConnectionPoint();

// Set port size
port.Size = new SizeF(12, 12); // 12x12 pixels

node.Ports.Add(port);
```

## Complete Port Example

```csharp
// Create network device node with 4 ports
Rectangle device = new Rectangle(200, 200, 100, 60);
device.FillStyle.Color = Color.LightGray;
device.DrawPorts = true;
device.EnableCentralPort = false;

// Input port (left, incoming only)
ConnectionPoint inputPort = new ConnectionPoint();
inputPort.OffsetX = 0;
inputPort.OffsetY = 30;
inputPort.ConnectionPointType = ConnectionPointType.Incoming;
inputPort.VisualType = PortVisualType.TrianglePort;
inputPort.FillStyle.Color = Color.LightGreen;
inputPort.ConnectionsLimit = 1;
device.Ports.Add(inputPort);

// Output ports (right, outgoing only)
for (int i = 0; i < 3; i++)
{
    ConnectionPoint outputPort = new ConnectionPoint();
    outputPort.OffsetX = 100; // Right edge
    outputPort.OffsetY = 15 + (i * 15); // Stacked vertically
    outputPort.ConnectionPointType = ConnectionPointType.Outgoing;
    outputPort.VisualType = PortVisualType.CirclePort;
    outputPort.FillStyle.Color = Color.LightCoral;
    outputPort.ConnectionsLimit = 5;
    device.Ports.Add(outputPort);
}

// Label
Label label = new Label();
label.Text = "Router";
label.Position = Position.Center;
device.Labels.Add(label);

diagram1.Model.AppendChild(device);
```

## Best Practices

### Dynamic Labels

```csharp
// Update label based on node state
private void UpdateNodeLabel(Node node, string status)
{
    if (node.Labels.Count > 0)
    {
        Label label = node.Labels[0];
        label.Text = $"{node.Name}\n{status}";
        
        // Change color based on status
        switch (status)
        {
            case "Active":
                label.FontColorStyle.Color = Color.Green;
                break;
            case "Error":
                label.FontColorStyle.Color = Color.Red;
                break;
            default:
                label.FontColorStyle.Color = Color.Black;
                break;
        }
    }
    
    diagram1.UpdateView();
}
```

### Port Management Helper

```csharp
public class PortManager
{
    public static void AddDirectionalPorts(Node node, int portCount = 4)
    {
        node.DrawPorts = true;
        node.EnableCentralPort = false;
        
        float centerX = node.Width / 2;
        float centerY = node.Height / 2;
        
        // Top
        AddPort(node, centerX, 0, PortVisualType.CirclePort);
        
        // Right
        AddPort(node, node.Width, centerY, PortVisualType.CirclePort);
        
        // Bottom
        AddPort(node, centerX, node.Height, PortVisualType.CirclePort);
        
        // Left
        AddPort(node, 0, centerY, PortVisualType.CirclePort);
    }
    
    private static void AddPort(Node node, float x, float y, PortVisualType visual)
    {
        ConnectionPoint port = new ConnectionPoint();
        port.OffsetX = x;
        port.OffsetY = y;
        port.VisualType = visual;
        port.FillStyle.Color = Color.LightBlue;
        node.Ports.Add(port);
    }
}

// Usage
Rectangle node = new Rectangle(100, 100, 80, 60);
PortManager.AddDirectionalPorts(node);
```

### Label Templates

```csharp
public static class LabelFactory
{
    public static Label CreateTitleLabel(string text)
    {
        Label label = new Label();
        label.Text = text;
        label.Position = Position.TopCenter;
        label.FontStyle.Bold = true;
        label.FontStyle.Size = 12;
        label.FontColorStyle.Color = Color.DarkBlue;
        return label;
    }
    
    public static Label CreateBodyLabel(string text)
    {
        Label label = new Label();
        label.Text = text;
        label.Position = Position.Center;
        label.FontStyle.Size = 10;
        label.WrapText = true;
        return label;
    }
    
    public static Label CreateStatusLabel(string text, Color color)
    {
        Label label = new Label();
        label.Text = text;
        label.Position = Position.BottomCenter;
        label.FontStyle.Size = 9;
        label.FontColorStyle.Color = color;
        return label;
    }
}
```

## Next Steps

- Explore diagram features in [features.md](features.md)
- Configure view settings in [view-controls.md](view-controls.md)
- Learn about user interaction in [user-interaction.md](user-interaction.md)
- Apply automatic layouts in [layout-management.md](layout-management.md)
