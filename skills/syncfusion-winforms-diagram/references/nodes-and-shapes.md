# Nodes and Shapes

## Table of Contents
- [Overview](#overview)
- [Available Node Types](#available-node-types)
- [Creating Nodes](#creating-nodes)
- [Node Properties](#node-properties)
- [Styling Nodes](#styling-nodes)
- [Node Selections and Behavior](#node-selections-and-behavior)
- [Rendering Styles](#rendering-styles)
- [Custom Shapes](#custom-shapes)
- [Best Practices](#best-practices)

## Overview

Nodes (also called shapes) are the fundamental building blocks of a diagram. Essential Diagram supports over 30 different node types, from basic shapes like rectangles and ellipses to complex nodes like symbols, bitmaps, and custom paths.

**Common Use Cases:**
- Flowchart symbols (process, decision, start/end)
- Organizational chart boxes
- Network diagram elements
- UML diagram shapes
- Custom business objects

## Available Node Types

### Basic Shapes
- **Rectangle** - Rectangular nodes with optional rounded corners
- **RoundRect** - Rectangles with rounded corners
- **Ellipse** - Circular and elliptical shapes
- **Circle** - Perfect circles
- **SemiCircle** - Half-circle shapes
- **CircularArc** - Arc segments
- **Polygon** - Multi-sided shapes (triangles, diamonds, hexagons)

### Line-Based Nodes
- **Line** - Straight lines
- **PolylineNode** - Multiple connected line segments
- **CurveNode** - Smooth curves
- **SplineNode** - Spline curves
- **BezierCurve** - Bezier curve paths
- **ClosedCurveNode** - Closed curve shapes
- **Arc** - Arc segments
- **MeasureLine** - Lines with measurement indicators

### Text Nodes
- **TextNode** - Simple text nodes
- **RichTextNode** - Rich formatted text with styling

### Special Nodes
- **BitmapNode** - Image/bitmap nodes
- **MetafileNode** - Metafile graphics
- **PathNode** - Custom vector paths
- **FilledPath** - Filled custom paths
- **FilledShape** - Custom filled shapes
- **Symbol** - Predefined symbols from palettes
- **ControlNode** - Embedded Windows Forms controls

### Container Nodes
- **Group** - Group multiple nodes together
- **PseudoGroup** - Logical grouping without visual container

### Connector Nodes
(See [connectors.md](connectors.md) for detailed information)
- **LineConnector** - Straight line connectors
- **OrthogonalConnector** - 90-degree angle connectors
- **OrthogonalLine** - Orthogonal line segments
- **PolyLineConnector** - Multi-segment connectors
- **Link** - General link objects

## Creating Nodes

### Creating an Ellipse Node

```csharp
using Syncfusion.Windows.Forms.Diagram;

// Create ellipse at position (10, 10) with size 110x70
Ellipse ellipse = new Ellipse(10, 10, 110, 70);

// Add to diagram model
diagram1.Model.AppendChild(ellipse);
```

**VB.NET:**
```vb
Dim ellipse As New Ellipse(10, 10, 110, 70)
diagram1.Model.AppendChild(ellipse)
```

### Creating a Rectangle Node

```csharp
// Create rectangle at position (50, 50) with size 100x70
Syncfusion.Windows.Forms.Diagram.Rectangle rect = 
    new Syncfusion.Windows.Forms.Diagram.Rectangle(50, 50, 100, 70);

diagram1.Model.AppendChild(rect);
```

### Creating a Rounded Rectangle

```csharp
// RoundRect(x, y, width, height, measureUnits)
RoundRect roundRect = new RoundRect(100, 100, 120, 80, MeasureUnits.Pixel);

// Set corner radius
roundRect.Radius = 15f;

diagram1.Model.AppendChild(roundRect);
```

### Creating a Polygon (Diamond Shape)

```csharp
// Define polygon points for a diamond
PointF[] diamondPoints = new PointF[] {
    new PointF(0, 50),   // Left point
    new PointF(50, 0),   // Top point
    new PointF(100, 50), // Right point
    new PointF(50, 100), // Bottom point
    new PointF(0, 50)    // Close the shape
};

Polygon diamond = new Polygon(diamondPoints);

// Position the polygon
diamond.PinPoint = new PointF(300, 300);

diagram1.Model.AppendChild(diamond);
```

### Creating a Text Node

```csharp
// Create text node
TextNode textNode = new TextNode("Hello Diagram!", new PointF(100, 200));

// Configure text properties
textNode.FontStyle.Family = "Arial";
textNode.FontStyle.Size = 14;
textNode.FontColorStyle.Color = Color.Black;

diagram1.Model.AppendChild(textNode);
```

### Creating a Bitmap Node

```csharp
// Load image
Bitmap image = new Bitmap("logo.png");

// Create bitmap node
BitmapNode bitmapNode = new BitmapNode(image, new RectangleF(50, 50, 100, 100));

diagram1.Model.AppendChild(bitmapNode);
```

## Node Properties

### Position and Size Properties

```csharp
Rectangle node = new Rectangle(0, 0, 100, 60);

// Position properties
node.PinPoint = new PointF(200, 150); // Center point
node.Location = new PointF(150, 120); // Top-left point

// Size properties
node.Width = 120;
node.Height = 80;
node.Size = new SizeF(120, 80);

// Bounding rectangle
RectangleF bounds = node.BoundingRectangle;
```

### Name and Identification

```csharp
node.Name = "ProcessNode1";
node.Tag = "CustomData"; // Store custom data
```

### Visibility and Enabled State

```csharp
node.Visible = true;   // Show/hide node
node.Enabled = true;   // Enable/disable interactions
```

### Transformation Properties

```csharp
// Rotation
node.RotationAngle = 45f; // Rotate 45 degrees

// Scale
node.ScaleHeight = 1.5f; // Scale height by 1.5x
node.ScaleWidth = 1.2f;  // Scale width by 1.2x

// Pin point (rotation center)
node.PinPoint = new PointF(100, 100);
```

### Z-Order (Layering)

```csharp
// Control stacking order
node.ZOrder = 10; // Higher values appear on top
```

## Styling Nodes

### Fill Styles

#### Solid Fill

```csharp
Rectangle node = new Rectangle(50, 50, 100, 70);

// Solid fill
node.FillStyle.Type = FillStyleType.Solid;
node.FillStyle.Color = Color.LightBlue;
node.FillStyle.ColorAlphaFactor = 255; // Fully opaque
```

#### Gradient Fill (Linear)

```csharp
node.FillStyle.Type = FillStyleType.LinearGradient;
node.FillStyle.Color = Color.FromArgb(128, 0, 0);      // Start color
node.FillStyle.ForeColor = Color.FromArgb(225, 0, 0);  // End color
node.FillStyle.GradientAngle = 45;                      // Gradient angle
node.FillStyle.GradientCenter = 0.5f;                   // Gradient center point
```

#### Gradient Fill (Path)

```csharp
node.FillStyle.Type = FillStyleType.PathGradient;
node.FillStyle.Color = Color.AliceBlue;
node.FillStyle.ForeColor = Color.Aquamarine;
node.FillStyle.PathBrushStyle = PathGradientBrushStyle.RectangleCenter;
```

#### Texture Fill

```csharp
node.FillStyle.Type = FillStyleType.Texture;
node.FillStyle.TextureImage = new Bitmap("texture.png");
node.FillStyle.TextureWrapMode = WrapMode.Tile;
```

### Border/Line Styles

```csharp
// Line color and width
node.LineStyle.LineColor = Color.DarkBlue;
node.LineStyle.LineWidth = 2.5f;

// Line style
node.LineStyle.DashStyle = DashStyle.Solid;  // Solid, Dash, Dot, DashDot, etc.
node.LineStyle.LineJoin = LineJoin.Miter;    // Sharp corners
node.LineStyle.LineCap = LineCap.Round;      // Rounded line ends

// Dash pattern for custom dashed lines
node.LineStyle.DashPattern = new float[] { 5, 2, 1, 2 };
node.LineStyle.DashOffset = 0;
```

### Shadow Styles

```csharp
// Enable shadow
node.ShadowStyle.Visible = true;

// Shadow appearance
node.ShadowStyle.Color = Color.Black;
node.ShadowStyle.ForeColor = Color.Gray;
node.ShadowStyle.ColorAlphaFactor = 100; // Transparency

// Shadow offset
node.ShadowStyle.OffsetX = 5;
node.ShadowStyle.OffsetY = 5;

// Blur effect
node.ShadowStyle.Blur = 3;
```

### Complete Styled Node Example

```csharp
// Create process box for flowchart
Rectangle process = new Rectangle(100, 100, 150, 80);

// Gradient fill
process.FillStyle.Type = FillStyleType.LinearGradient;
process.FillStyle.Color = Color.LightSkyBlue;
process.FillStyle.ForeColor = Color.DeepSkyBlue;
process.FillStyle.GradientAngle = 90;

// Border
process.LineStyle.LineColor = Color.DarkBlue;
process.LineStyle.LineWidth = 2;
process.LineStyle.LineJoin = LineJoin.Round;

// Shadow
process.ShadowStyle.Visible = true;
process.ShadowStyle.Color = Color.Gray;
process.ShadowStyle.OffsetX = 3;
process.ShadowStyle.OffsetY = 3;

// Add label
Label label = new Label();
label.Text = "Process Step";
label.FontStyle.Family = "Segoe UI";
label.FontStyle.Size = 12;
label.FontStyle.Bold = true;
label.FontColorStyle.Color = Color.White;
label.HorizontalAlignment = StringAlignment.Center;
label.VerticalAlignment = StringAlignment.Center;
process.Labels.Add(label);

diagram1.Model.AppendChild(process);
```

## Node Selections and Behavior

### EditStyle Properties

Control how nodes can be interacted with:

```csharp
Rectangle node = new Rectangle(50, 50, 100, 70);

// Selection
node.EditStyle.AllowSelect = true;      // Allow node selection
node.EditStyle.AllowDelete = true;      // Allow deletion with DELETE key

// Movement
node.EditStyle.AllowMoveX = true;       // Allow horizontal movement
node.EditStyle.AllowMoveY = true;       // Allow vertical movement

// Resizing
node.EditStyle.AllowResize = true;      // Allow resizing
node.EditStyle.AllowChangeWidth = true;  // Allow width changes
node.EditStyle.AllowChangeHeight = true; // Allow height changes
node.EditStyle.AspectRatio = false;     // Maintain aspect ratio when resizing

// Rotation
node.EditStyle.AllowRotate = true;      // Allow rotation

// Vertex editing (for lines/polygons)
node.EditStyle.AllowVertexEdit = true;

// Handle mode
node.EditStyle.DefaultHandleEditMode = HandleEditMode.Resize; // or Vertex, None
```

### Locking Node Position

```csharp
// Lock node in place (cannot move)
node.EditStyle.AllowMoveX = false;
node.EditStyle.AllowMoveY = false;
```

### Making Node Read-Only

```csharp
// Disable all interactions
node.EditStyle.AllowSelect = true;  // Can still select
node.EditStyle.AllowMoveX = false;
node.EditStyle.AllowMoveY = false;
node.EditStyle.AllowResize = false;
node.EditStyle.AllowRotate = false;
node.EditStyle.AllowDelete = false;
```

### Hiding Handles

```csharp
// Hide pin point handle
node.EditStyle.HidePinPoint = true;

// Hide rotation handle
node.EditStyle.HideRotationHandle = true;
```

### Behavior Settings

```csharp
// Enable/disable node
node.Enabled = true;

// Treat as obstacle for line routing
node.TreatAsObstacle = true;

// Allow node to participate in layout algorithms
node.ExcludeFromLayout = false;
```

## Rendering Styles

Control the visual feedback during drag, resize, and rotate operations:

```csharp
// Set rendering styles for the controller
diagram1.Controller.DraggingStyle = RenderingHelperStyle.SolidOutline;
diagram1.Controller.ResizingStyle = RenderingHelperStyle.GhostCopy;
diagram1.Controller.RotatingStyle = RenderingHelperStyle.DashedOutline;
```

**Available Rendering Styles:**
- **SolidOutline** - Solid outline of the shape
- **GhostCopy** - Semi-transparent copy of the shape
- **DashedOutline** - Dashed outline
- **FilledRectangle** - Filled rectangle placeholder

## Custom Shapes

### Creating Custom Shapes with Symbol Designer

Use the Symbol Designer utility (included with Essential Diagram) to create custom shapes:

1. Launch Symbol Designer from installation directory
2. Create new palette or open existing `.edp` file
3. Draw custom shapes using drawing tools
4. Set properties (fill, border, ports, labels)
5. Save as `.edp` palette file
6. Load palette in your application:

```csharp
PaletteGroupBar paletteBar = new PaletteGroupBar();
paletteBar.LoadPalette("CustomShapes.edp");
```

### Creating Custom Paths Programmatically

```csharp
// Create custom path
GraphicsPath customPath = new GraphicsPath();
customPath.AddEllipse(0, 0, 50, 50);
customPath.AddRectangle(new Rectangle(20, 45, 10, 30));

// Create PathNode from path
PathNode customNode = new PathNode(customPath, new PointF(100, 100));

// Style the custom node
customNode.FillStyle.Color = Color.LightYellow;
customNode.LineStyle.LineColor = Color.Orange;

diagram1.Model.AppendChild(customNode);
```

### Creating a Symbol from Grouped Shapes

```csharp
// Create multiple shapes
Circle head = new Circle(new PointF(100, 50), 30);
Rectangle body = new Rectangle(85, 80, 30, 60);
Line leftArm = new Line(new PointF(85, 100), new PointF(60, 120));
Line rightArm = new Line(new PointF(115, 100), new PointF(140, 120));

// Add to model
diagram1.Model.AppendChild(head);
diagram1.Model.AppendChild(body);
diagram1.Model.AppendChild(leftArm);
diagram1.Model.AppendChild(rightArm);

// Select all shapes
diagram1.Controller.Model.SelectionList.Clear();
diagram1.Controller.Model.SelectionList.Add(head);
diagram1.Controller.Model.SelectionList.Add(body);
diagram1.Controller.Model.SelectionList.Add(leftArm);
diagram1.Controller.Model.SelectionList.Add(rightArm);

// Group them
diagram1.Controller.Group();
```

## Best Practices

### Performance Optimization

```csharp
// Suspend updates during batch operations
diagram1.Model.BeginUpdate();

try
{
    // Add multiple nodes
    for (int i = 0; i < 100; i++)
    {
        Rectangle node = new Rectangle(i * 50, 100, 40, 30);
        diagram1.Model.AppendChild(node);
    }
}
finally
{
    diagram1.Model.EndUpdate(); // Refresh once
}
```

### Naming Conventions

```csharp
// Use descriptive names for nodes
node.Name = "StartNode";
node.Name = "Process_DataValidation";
node.Name = "Decision_CheckStatus";

// Access nodes by name later
Node foundNode = diagram1.Model.FindNodeByName("StartNode");
```

### Reusable Node Creation

```csharp
// Create a helper method for flowchart nodes
private Rectangle CreateProcessNode(string text, float x, float y)
{
    Rectangle node = new Rectangle(x, y, 150, 80);
    
    node.FillStyle.Type = FillStyleType.LinearGradient;
    node.FillStyle.Color = Color.LightBlue;
    node.FillStyle.ForeColor = Color.Blue;
    
    node.LineStyle.LineColor = Color.DarkBlue;
    node.LineStyle.LineWidth = 2;
    
    Label label = new Label();
    label.Text = text;
    label.HorizontalAlignment = StringAlignment.Center;
    label.VerticalAlignment = StringAlignment.Center;
    node.Labels.Add(label);
    
    return node;
}

// Usage
Rectangle process1 = CreateProcessNode("Input Data", 100, 100);
Rectangle process2 = CreateProcessNode("Validate", 100, 200);
diagram1.Model.AppendChild(process1);
diagram1.Model.AppendChild(process2);
```

### Node Templates

```csharp
// Create a template node
Rectangle template = new Rectangle(0, 0, 100, 60);
template.FillStyle.Color = Color.LightGreen;
template.LineStyle.LineColor = Color.DarkGreen;
template.LineStyle.LineWidth = 2;

// Clone for reuse
Rectangle node1 = (Rectangle)template.Clone();
node1.PinPoint = new PointF(100, 100);

Rectangle node2 = (Rectangle)template.Clone();
node2.PinPoint = new PointF(250, 100);

diagram1.Model.AppendChild(node1);
diagram1.Model.AppendChild(node2);
```

## Common Issues and Solutions

### Issue: Nodes Not Visible After Adding

**Solution:** Ensure the node coordinates are within the diagram viewport
```csharp
// Refresh the view after adding nodes
diagram1.UpdateView();

// Or fit the diagram to show all content
diagram1.View.FitToPage();
```

### Issue: Cannot Move or Resize Node

**Solution:** Check EditStyle permissions
```csharp
node.EditStyle.AllowMoveX = true;
node.EditStyle.AllowMoveY = true;
node.EditStyle.AllowResize = true;
```

### Issue: Label Text Not Showing

**Solution:** Ensure label is added and has visible text
```csharp
Label label = new Label();
label.Text = "My Label";
label.Visible = true;
label.FontColorStyle.Color = Color.Black; // Not white on white
node.Labels.Add(label);
```

## Next Steps

- Learn about connecting nodes in [connectors.md](connectors.md)
- Explore adding labels and ports in [labels-ports.md](labels-ports.md)
- Organize nodes with [layout-organization.md](layout-organization.md)
- Add interactivity with [drawing-tools.md](drawing-tools.md)
