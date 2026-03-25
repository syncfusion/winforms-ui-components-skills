# Drawing Tools

## Table of Contents
- [Overview](#overview)
- [Tool Activation](#tool-activation)
- [Selection Tool](#selection-tool)
- [Shape Drawing Tools](#shape-drawing-tools)
- [Line Drawing Tools](#line-drawing-tools)
- [Text Tools](#text-tools)
- [Connection Tools](#connection-tools)
- [Special Tools](#special-tools)
- [Tool Configuration](#tool-configuration)
- [Custom Tools](#custom-tools)

## Overview

Essential Diagram provides 15+ interactive drawing tools that enable users to create shapes, lines, text, and connections directly on the diagram canvas. Tools are activated through the Controller and respond to mouse input for intuitive drawing experiences.

**Common Use Cases:**
- Interactive diagram editors
- Flowchart creators
- Circuit design applications
- Network diagram tools
- Whiteboarding applications

## Tool Activation

### Activating a Tool

```csharp
// Activate tool by name
diagram1.Controller.ActivateTool("RectangleTool");

// Check active tool
Tool activeTool = diagram1.Controller.ActiveTool;
if (activeTool is RectangleTool)
{
    // Configure the tool
}
```

### Switching Between Tools

```csharp
// Create buttons for tool switching
private void btnSelect_Click(object sender, EventArgs e)
{
    diagram1.Controller.ActivateTool("SelectTool");
}

private void btnRectangle_Click(object sender, EventArgs e)
{
    diagram1.Controller.ActivateTool("RectangleTool");
}

private void btnLine_Click(object sender, EventArgs e)
{
    diagram1.Controller.ActivateTool("LineTool");
}
```

### Deactivating All Tools

```csharp
// Return to selection mode
diagram1.Controller.ActivateTool("SelectTool");

// Or deactivate completely
diagram1.Controller.ActivateTool(null);
```

## Selection Tool

The default tool for selecting, moving, and resizing nodes.

```csharp
diagram1.Controller.ActivateTool("SelectTool");
```

**Features:**
- Click to select nodes
- Drag to move selected nodes
- Drag handles to resize
- Ctrl+Click for multi-select
- Drag selection rectangle

### Configuring Selection Mode

```csharp
Tool tool = diagram1.Controller.ActiveTool;
if (tool is SelectTool selectTool)
{
    // Set selection mode
    selectTool.SelectionMode = SelectionMode.Default;
    // Or: Invert, Toggle, None
}
```

## Shape Drawing Tools

### RectangleTool

Draw rectangles by clicking and dragging.

```csharp
diagram1.Controller.ActivateTool("RectangleTool");

// Configure default properties
Tool tool = diagram1.Controller.ActiveTool;
if (tool is RectangleTool rectTool)
{
    rectTool.FillStyle.Color = Color.LightBlue;
    rectTool.LineStyle.LineColor = Color.DarkBlue;
    rectTool.LineStyle.LineWidth = 2;
}
```

### RoundRectTool

Draw rectangles with rounded corners.

```csharp
diagram1.Controller.ActivateTool("RoundRectTool");

Tool tool = diagram1.Controller.ActiveTool;
if (tool is RoundRectTool roundRectTool)
{
    roundRectTool.Radius = 15f; // Corner radius
    roundRectTool.FillStyle.Color = Color.LightGreen;
}
```

### EllipseTool

Draw ellipses and circles.

```csharp
diagram1.Controller.ActivateTool("EllipseTool");

Tool tool = diagram1.Controller.ActiveTool;
if (tool is EllipseTool ellipseTool)
{
    ellipseTool.FillStyle.Color = Color.Yellow;
    ellipseTool.LineStyle.LineWidth = 2;
}
```

### PolygonTool

Draw multi-sided polygons by clicking points.

```csharp
diagram1.Controller.ActivateTool("PolygonTool");

// User clicks to add points
// Double-click or right-click to finish
```

**Usage:**
1. Click to place first point
2. Click to add subsequent points
3. Double-click to close polygon

## Line Drawing Tools

### LineTool

Draw straight lines between two points.

```csharp
diagram1.Controller.ActivateTool("LineTool");

Tool tool = diagram1.Controller.ActiveTool;
if (tool is LineTool lineTool)
{
    lineTool.LineStyle.LineColor = Color.Black;
    lineTool.LineStyle.LineWidth = 2;
}
```

### PolyLineTool

Draw multiple connected line segments.

```csharp
diagram1.Controller.ActivateTool("PolyLineTool");
```

**Usage:**
1. Click for start point
2. Click for each vertex
3. Double-click to finish

### CurveTool

Draw smooth curves through points.

```csharp
diagram1.Controller.ActivateTool("CurveTool");
```

### BezierTool

Draw Bezier curves with control points.

```csharp
diagram1.Controller.ActivateTool("BezierTool");
```

**Usage:**
1. Click for start point
2. Click for control point 1
3. Click for control point 2
4. Click for end point

### SplineTool

Draw spline curves.

```csharp
diagram1.Controller.ActivateTool("SplineTool");
```

### ClosedCurveTool

Draw closed smooth curves.

```csharp
diagram1.Controller.ActivateTool("ClosedCurveTool");
```

### PencilTool

Freehand drawing tool.

```csharp
diagram1.Controller.ActivateTool("PencilTool");

Tool tool = diagram1.Controller.ActiveTool;
if (tool is PencilTool pencilTool)
{
    pencilTool.LineStyle.LineColor = Color.Black;
    pencilTool.LineStyle.LineWidth = 3;
}
```

## Text Tools

### TextTool

Insert and edit simple text nodes.

```csharp
diagram1.Controller.ActivateTool("TextTool");
```

**Usage:**
1. Click and drag to define text box size
2. Release to start typing
3. Double-click existing text to edit

### RichTextTool

Insert and edit rich formatted text.

```csharp
diagram1.Controller.ActivateTool("RichTextTool");
```

**Features:**
- Bold, italic, underline
- Multiple fonts and sizes
- Colors and highlighting
- Paragraph formatting

## Connection Tools

Connection tools create connectors between nodes.

### LineLinkTool

Draw straight line connectors.

```csharp
diagram1.Controller.ActivateTool("LineLinkTool");

Tool tool = diagram1.Controller.ActiveTool;
if (tool is LineConnectorTool linkTool)
{
    // Configure decorators
    linkTool.HeadDecorator.DecoratorShape = DecoratorShape.Filled45Arrow;
    linkTool.TailDecorator.DecoratorShape = DecoratorShape.None;
    
    // Configure line style
    linkTool.LineStyle.LineColor = Color.DarkGray;
    linkTool.LineStyle.LineWidth = 2;
}
```

### OrthogonalLinkTool

Draw orthogonal (90-degree) connectors.

```csharp
diagram1.Controller.ActivateTool("OrthogonalLinkTool");

Tool tool = diagram1.Controller.ActiveTool;
if (tool is OrthogonalConnectorTool orthoTool)
{
    orthoTool.HeadDecorator.DecoratorShape = DecoratorShape.Filled45Arrow;
    orthoTool.HeadDecorator.Size = new SizeF(10, 6);
}
```

### DirectedLineLinkTool

Draw smart routed connectors.

```csharp
diagram1.Controller.ActivateTool("DirectedLineLinkTool");

Tool tool = diagram1.Controller.ActiveTool;
if (tool is DirectedLineConnectorTool directedTool)
{
    directedTool.HeadDecorator.DecoratorShape = DecoratorShape.Filled45Arrow;
}
```

### PolyLineLinkTool

Draw multi-segment connectors.

```csharp
diagram1.Controller.ActivateTool("PolyLineLinkTool");

Tool tool = diagram1.Controller.ActiveTool;
if (tool is PolyLineConnectorTool polyTool)
{
    polyTool.HeadDecorator.DecoratorShape = DecoratorShape.Open45Arrow;
}
```

### OrgLineLinkTool

Draw organizational chart connectors.

```csharp
diagram1.Controller.ActivateTool("OrgLineLinkTool");
```

## Special Tools

### BitmapTool

Insert bitmap images.

```csharp
diagram1.Controller.ActivateTool("BitmapTool");

Tool tool = diagram1.Controller.ActiveTool;
if (tool is BitmapTool bitmapTool)
{
    // Set default image
    bitmapTool.Bitmap = new Bitmap("default-image.png");
}
```

**Usage:**
1. Activate tool
2. Click and drag to define image size
3. Image is placed on release

### ConnectionPointTool

Add or remove connection ports on nodes.

```csharp
diagram1.Controller.ActivateTool("ConnectionPointTool");
```

**Usage:**
- **Add port:** Click on node
- **Remove port:** Ctrl+Click on existing port

### PanTool

Pan the diagram view.

```csharp
diagram1.Controller.ActivateTool("PanTool");
```

**Usage:**
- Click and drag to pan the view

### ZoomTool

Zoom the diagram view.

```csharp
diagram1.Controller.ActivateTool("ZoomTool");
```

**Usage:**
- Click to zoom in
- Ctrl+Click to zoom out
- Click and drag to zoom to rectangle

## Tool Configuration

### Setting Default Node Properties

```csharp
// Configure RectangleTool defaults
diagram1.Controller.ActivateTool("RectangleTool");

Tool tool = diagram1.Controller.ActiveTool;
if (tool is RectangleTool rectTool)
{
    // Fill style
    rectTool.FillStyle.Type = FillStyleType.LinearGradient;
    rectTool.FillStyle.Color = Color.LightBlue;
    rectTool.FillStyle.ForeColor = Color.Blue;
    rectTool.FillStyle.GradientAngle = 90;
    
    // Line style
    rectTool.LineStyle.LineColor = Color.DarkBlue;
    rectTool.LineStyle.LineWidth = 2;
    rectTool.LineStyle.DashStyle = DashStyle.Solid;
    
    // Shadow
    rectTool.ShadowStyle.Visible = true;
    rectTool.ShadowStyle.OffsetX = 3;
    rectTool.ShadowStyle.OffsetY = 3;
}
```

### Setting Default Connector Properties

```csharp
diagram1.Controller.ActivateTool("LineLinkTool");

Tool tool = diagram1.Controller.ActiveTool;
if (tool is LineConnectorTool connectorTool)
{
    // Line properties
    connectorTool.LineStyle.LineColor = Color.DarkGray;
    connectorTool.LineStyle.LineWidth = 2;
    
    // Head decorator
    connectorTool.HeadDecorator.DecoratorShape = DecoratorShape.Filled45Arrow;
    connectorTool.HeadDecorator.Size = new SizeF(10, 6);
    connectorTool.HeadDecorator.FillStyle.Color = Color.DarkGray;
    
    // Tail decorator
    connectorTool.TailDecorator.DecoratorShape = DecoratorShape.None;
}
```

### Tool Event Handling

```csharp
// Handle tool activation
diagram1.Controller.ToolActivated += (sender, e) =>
{
    Tool tool = e.Tool;
    Console.WriteLine($"Activated: {tool.Name}");
    
    // Update UI to reflect active tool
    UpdateToolbar(tool.Name);
};

// Handle tool deactivation
diagram1.Controller.ToolDeactivated += (sender, e) =>
{
    Tool tool = e.Tool;
    Console.WriteLine($"Deactivated: {tool.Name}");
};
```

## Custom Tools

### Creating a Custom Drawing Tool

```csharp
using Syncfusion.Windows.Forms.Diagram;

public class CustomShapeTool : DrawingTool
{
    public CustomShapeTool(Controller controller) 
        : base(controller)
    {
        this.Name = "CustomShapeTool";
    }
    
    protected override Node CreateNode()
    {
        // Create custom node
        RoundRect node = new RoundRect(0, 0, 100, 60, MeasureUnits.Pixel);
        node.FillStyle.Color = Color.LightYellow;
        node.LineStyle.LineColor = Color.Orange;
        node.Radius = 20;
        
        // Add default label
        Label label = new Label();
        label.Text = "Custom";
        label.Position = Position.Center;
        node.Labels.Add(label);
        
        return node;
    }
}

// Register and use custom tool
CustomShapeTool customTool = new CustomShapeTool(diagram1.Controller);
diagram1.Controller.RegisterTool(customTool);
diagram1.Controller.ActivateTool("CustomShapeTool");
```

## Complete Tool Palette Example

```csharp
public class DiagramToolPalette : Form
{
    private Diagram diagram;
    private ToolStrip toolStrip;
    
    public DiagramToolPalette()
    {
        InitializeComponent();
        CreateDiagram();
        CreateToolbar();
    }
    
    private void CreateDiagram()
    {
        diagram = new Diagram();
        diagram.Dock = DockStyle.Fill;
        
        Model model = new Model();
        diagram.Model = model;
        
        this.Controls.Add(diagram);
    }
    
    private void CreateToolbar()
    {
        toolStrip = new ToolStrip();
        toolStrip.Dock = DockStyle.Top;
        
        // Selection tool
        ToolStripButton btnSelect = new ToolStripButton("Select");
        btnSelect.Click += (s, e) => ActivateTool("SelectTool");
        toolStrip.Items.Add(btnSelect);
        
        toolStrip.Items.Add(new ToolStripSeparator());
        
        // Shape tools
        ToolStripButton btnRectangle = new ToolStripButton("Rectangle");
        btnRectangle.Click += (s, e) => ActivateTool("RectangleTool");
        toolStrip.Items.Add(btnRectangle);
        
        ToolStripButton btnEllipse = new ToolStripButton("Ellipse");
        btnEllipse.Click += (s, e) => ActivateTool("EllipseTool");
        toolStrip.Items.Add(btnEllipse);
        
        ToolStripButton btnPolygon = new ToolStripButton("Polygon");
        btnPolygon.Click += (s, e) => ActivateTool("PolygonTool");
        toolStrip.Items.Add(btnPolygon);
        
        toolStrip.Items.Add(new ToolStripSeparator());
        
        // Line tools
        ToolStripButton btnLine = new ToolStripButton("Line");
        btnLine.Click += (s, e) => ActivateTool("LineTool");
        toolStrip.Items.Add(btnLine);
        
        ToolStripButton btnPolyline = new ToolStripButton("Polyline");
        btnPolyline.Click += (s, e) => ActivateTool("PolyLineTool");
        toolStrip.Items.Add(btnPolyline);
        
        toolStrip.Items.Add(new ToolStripSeparator());
        
        // Connector tools
        ToolStripButton btnConnector = new ToolStripButton("Connector");
        btnConnector.Click += (s, e) => ActivateTool("OrthogonalLinkTool");
        toolStrip.Items.Add(btnConnector);
        
        toolStrip.Items.Add(new ToolStripSeparator());
        
        // Text tool
        ToolStripButton btnText = new ToolStripButton("Text");
        btnText.Click += (s, e) => ActivateTool("TextTool");
        toolStrip.Items.Add(btnText);
        
        this.Controls.Add(toolStrip);
    }
    
    private void ActivateTool(string toolName)
    {
        diagram.Controller.ActivateTool(toolName);
        
        // Configure tool defaults
        ConfigureActiveTool();
    }
    
    private void ConfigureActiveTool()
    {
        Tool tool = diagram.Controller.ActiveTool;
        
        if (tool is RectangleTool rectTool)
        {
            rectTool.FillStyle.Color = Color.LightBlue;
            rectTool.LineStyle.LineColor = Color.DarkBlue;
            rectTool.LineStyle.LineWidth = 2;
        }
        else if (tool is EllipseTool ellipseTool)
        {
            ellipseTool.FillStyle.Color = Color.LightGreen;
            ellipseTool.LineStyle.LineColor = Color.DarkGreen;
            ellipseTool.LineStyle.LineWidth = 2;
        }
        else if (tool is OrthogonalConnectorTool connectorTool)
        {
            connectorTool.HeadDecorator.DecoratorShape = DecoratorShape.Filled45Arrow;
            connectorTool.LineStyle.LineWidth = 2;
        }
    }
}
```

## Best Practices

### Tool State Management

```csharp
// Save current tool before temporary switch
string previousTool = diagram1.Controller.ActiveTool.Name;

// Switch temporarily
diagram1.Controller.ActivateTool("PanTool");

// Restore previous tool
diagram1.Controller.ActivateTool(previousTool);
```

### Keyboard Shortcuts

```csharp
protected override bool ProcessCmdKey(ref Message msg, Keys keyData)
{
    switch (keyData)
    {
        case Keys.Escape:
            diagram1.Controller.ActivateTool("SelectTool");
            return true;
            
        case Keys.R:
            diagram1.Controller.ActivateTool("RectangleTool");
            return true;
            
        case Keys.L:
            diagram1.Controller.ActivateTool("LineTool");
            return true;
            
        case Keys.T:
            diagram1.Controller.ActivateTool("TextTool");
            return true;
    }
    
    return base.ProcessCmdKey(ref msg, keyData);
}
```

### Visual Feedback for Active Tool

```csharp
private void UpdateToolbarForActiveTool(string toolName)
{
    // Highlight active tool button
    foreach (ToolStripItem item in toolStrip.Items)
    {
        if (item is ToolStripButton button)
        {
            button.Checked = (button.Text == GetToolDisplayName(toolName));
        }
    }
    
    // Update status bar
    statusLabel.Text = $"Active Tool: {GetToolDisplayName(toolName)}";
}

private string GetToolDisplayName(string toolName)
{
    return toolName.Replace("Tool", "").Replace("Link", " Connector");
}
```

## Next Steps

- Configure diagram controls in [diagram-controls.md](diagram-controls.md)
- Learn about node organization in [layout-organization.md](layout-organization.md)
- Add labels to shapes in [labels-ports.md](labels-ports.md)
- Handle user interactions in [user-interaction.md](user-interaction.md)
