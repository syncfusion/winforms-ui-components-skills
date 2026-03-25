# Getting Started with Windows Forms Diagram

## Table of Contents
- [Overview](#overview)
- [Assembly Deployment](#assembly-deployment)
- [Structure of Essential Diagram](#structure-of-essential-diagram)
- [Creating Diagram Through Designer](#creating-diagram-through-designer)
- [Creating Diagram Through Code](#creating-diagram-through-code)
- [Adding Nodes to Model](#adding-nodes-to-model)
- [Connecting Nodes](#connecting-nodes)
- [Complete Example](#complete-example)

## Overview

Essential Diagram is an extensible .NET diagramming framework for Windows Forms that enables creating Microsoft Visio-like interactive graphics and diagramming applications. It uses a Model-View-Controller (MVC) architecture to provide clear separation between data, visualization, and user interaction.

**Key Components:**
- **Model** - Contains diagram data (nodes, connectors, layers)
- **View** - Renders the diagram and manages display properties
- **Controller** - Handles user input and tool activation

## Assembly Deployment

Add the following Syncfusion assemblies to your project:

```
Required Assemblies:
- Syncfusion.Core.dll
- Syncfusion.Diagram.Base.dll
- Syncfusion.Diagram.Windows.dll
- Syncfusion.Shared.Base.dll
```

**Installation Paths:**
- NuGet: Install `Syncfusion.Diagram.Windows` package
- Manual: `[Drive]:\Program Files\Syncfusion\Essential Studio\[Version]\precompiledassemblies\[Framework Version]`

## Structure of Essential Diagram

Essential Diagram includes six main controls:

### 1. Diagram Control
The main canvas for rendering and manipulating 2D shapes, text, images, and controls. Supports drag-and-drop, scaling, zooming, rotation, grouping, and many interactive features.

### 2. Overview Control
Provides a perspective view of the diagram model with a movable/resizable viewport window for dynamic pan/zoom operations.

### 3. PaletteGroupBar
A GroupBar-based control that displays symbol palettes with drag-and-drop capability. Each palette occupies a panel selectable by bar buttons.

### 4. PaletteGroupView
Displays symbols from a palette file in a GroupView control. Can be serialized to/from form resources and hosted in PaletteGroupBar.

### 5. PropertyEditor
Displays and edits properties of diagram objects using an embedded PropertyGrid control. Includes a combo box for object selection.

### 6. DocumentExplorer
Tree view visualization of diagram objects. Lists layers under "Layers" node and shapes/links/text under "Nodes" node. Supports rename, delete, hide, and layer operations.

## Creating Diagram Through Designer

**Step 1:** Create a new Windows Forms application

**Step 2:** Open the Designer Form window

**Step 3:** Drag **Diagram** from Toolbox and drop to the Designer Form

The Diagram control will be added to the designer, and its dependent assemblies will be automatically referenced in the project.

**Designer Properties to Configure:**
```csharp
// Common designer settings
diagram1.Size = new Size(800, 600);
diagram1.HScroll = true;
diagram1.VScroll = true;
diagram1.ShowRulers = true;
diagram1.Dock = DockStyle.Fill;
```

## Creating Diagram Through Code

### Basic Setup

```csharp
using Syncfusion.Windows.Forms.Diagram;
using Syncfusion.Windows.Forms.Diagram.Controls;

// Create diagram instance
Diagram diagram = new Diagram();

// Configure scrollbars
diagram.HScroll = true;
diagram.VScroll = true;

// Set size and position
diagram.Size = new Size(800, 600);
diagram.Location = new Point(20, 5);

// Enable rulers
diagram.ShowRulers = true;

// Create and attach model
Model model = new Model();
diagram.Model = model;

// Add to form
this.Controls.Add(diagram);
```

### VB.NET Example

```vb
Imports Syncfusion.Windows.Forms.Diagram
Imports Syncfusion.Windows.Forms.Diagram.Controls

'Create diagram instance
Dim diagram As New Diagram()

'Configure scrollbars
diagram.HScroll = True
diagram.VScroll = True

'Set size and position
diagram.Size = New Size(800, 600)
diagram.Location = New Point(20, 5)

'Enable rulers
diagram.ShowRulers = True

'Create and attach model
Dim model As New Model()
diagram.Model = model

'Add to form
Me.Controls.Add(diagram)
```

## Adding Nodes to Model

### Creating a Simple Rectangle

```csharp
// Create rectangular node
Syncfusion.Windows.Forms.Diagram.Rectangle rectangle = 
    new Syncfusion.Windows.Forms.Diagram.Rectangle(120, 120, 100, 70);

// Style the node - Fill
rectangle.FillStyle.Type = FillStyleType.LinearGradient;
rectangle.FillStyle.Color = Color.FromArgb(128, 0, 0);
rectangle.FillStyle.ForeColor = Color.FromArgb(225, 0, 0);

// Add shadow
rectangle.ShadowStyle.Visible = true;

// Border style
rectangle.LineStyle.LineColor = Color.RosyBrown;
rectangle.LineStyle.LineWidth = 2.0f;
rectangle.LineStyle.LineJoin = LineJoin.Miter;

// Add label
Label label = new Label();
label.Text = "Process";
label.FontStyle.Family = "Arial";
label.FontColorStyle.Color = Color.White;
rectangle.Labels.Add(label);

// Add to model
diagram.Model.AppendChild(rectangle);
```

### Creating an Ellipse Node

```csharp
// Create ellipse (for start/end nodes)
Ellipse ellipse = new Ellipse(50, 50, 120, 80);

// Style with gradient
ellipse.FillStyle.Type = FillStyleType.LinearGradient;
ellipse.FillStyle.Color = Color.LightGreen;
ellipse.FillStyle.ForeColor = Color.Green;
ellipse.FillStyle.GradientAngle = 45;

// Border
ellipse.LineStyle.LineColor = Color.DarkGreen;
ellipse.LineStyle.LineWidth = 2;

// Add label
Label startLabel = new Label();
startLabel.Text = "Start";
startLabel.FontColorStyle.Color = Color.Black;
ellipse.Labels.Add(startLabel);

// Add to model
diagram.Model.AppendChild(ellipse);
```

### Creating a Diamond (Decision Node)

```csharp
// Create diamond shape for decisions
Polygon decision = new Polygon(new PointF[] {
    new PointF(0, 50),   // Left
    new PointF(50, 0),   // Top
    new PointF(100, 50), // Right
    new PointF(50, 100), // Bottom
    new PointF(0, 50)    // Close
});

// Position
decision.PinPoint = new PointF(300, 300);

// Style
decision.FillStyle.Color = Color.LightYellow;
decision.LineStyle.LineColor = Color.Orange;
decision.LineStyle.LineWidth = 2;

// Label
Label decisionLabel = new Label();
decisionLabel.Text = "Decision?";
decisionLabel.FontColorStyle.Color = Color.Black;
decision.Labels.Add(decisionLabel);

// Add to model
diagram.Model.AppendChild(decision);
```

## Connecting Nodes

### Using OrthogonalConnector (90-degree angles)

```csharp
// Create process and decision nodes (as shown above)
Rectangle process = new Rectangle(50, 50, 100, 70);
diagram.Model.AppendChild(process);

Polygon decision = new Polygon(new PointF[] {
    new PointF(0, 50), new PointF(50, 0),
    new PointF(100, 50), new PointF(50, 100),
    new PointF(0, 50)
});
decision.PinPoint = new PointF(250, 250);
diagram.Model.AppendChild(decision);

// Create orthogonal connector
OrthogonalConnector link = 
    new OrthogonalConnector(process.PinPoint, decision.PinPoint);

// Style the connector
link.LineStyle.LineColor = Color.RosyBrown;
link.LineStyle.LineWidth = 2f;

// Add arrow decorator
link.HeadDecorator.DecoratorShape = DecoratorShape.Filled45Arrow;
link.HeadDecorator.Size = new SizeF(8, 8);
link.HeadDecorator.FillStyle.Color = Color.RosyBrown;
link.HeadDecorator.LineStyle.LineColor = Color.RosyBrown;

// Connect nodes via ports
process.CentralPort.TryConnect(link.TailEndPoint);
decision.CentralPort.TryConnect(link.HeadEndPoint);

// Add link to model
diagram.Model.AppendChild(link);
```

### Using LineConnector (Straight Line)

```csharp
// Create two nodes
Ellipse ellipse = new Ellipse(10, 10, 110, 70);
Rectangle rectangle = new Rectangle(300, 50, 50, 80);

diagram.Model.AppendChild(ellipse);
diagram.Model.AppendChild(rectangle);

// Create line connector
LineConnector lineConnector = 
    new LineConnector(ellipse.PinPoint, rectangle.PinPoint);

// Style
lineConnector.LineStyle.LineColor = Color.MidnightBlue;
lineConnector.LineStyle.LineWidth = 2;

// Decorators
lineConnector.HeadDecorator.DecoratorShape = DecoratorShape.Filled45Arrow;
lineConnector.HeadDecorator.FillStyle.Color = Color.MidnightBlue;
lineConnector.HeadDecorator.Size = new SizeF(10, 5);

// Connect via ports
ellipse.CentralPort.TryConnect(lineConnector.TailEndPoint);
rectangle.CentralPort.TryConnect(lineConnector.HeadEndPoint);

// Add to model
diagram.Model.AppendChild(lineConnector);
```

## Complete Example

### Flowchart with Multiple Nodes

```csharp
using Syncfusion.Windows.Forms.Diagram;
using Syncfusion.Windows.Forms.Diagram.Controls;

public partial class Form1 : Form
{
    private Diagram diagram;
    private Model model;

    public Form1()
    {
        InitializeComponent();
        CreateFlowchart();
    }

    private void CreateFlowchart()
    {
        // Initialize diagram
        diagram = new Diagram();
        diagram.Size = new Size(800, 600);
        diagram.Location = new Point(10, 10);
        diagram.HScroll = true;
        diagram.VScroll = true;
        diagram.ShowRulers = true;

        // Create model
        model = new Model();
        diagram.Model = model;

        // Create Start node (Ellipse)
        Ellipse start = new Ellipse(200, 50, 100, 60);
        start.FillStyle.Color = Color.LightGreen;
        start.LineStyle.LineColor = Color.DarkGreen;
        start.LineStyle.LineWidth = 2;
        
        Label startLabel = new Label();
        startLabel.Text = "Start";
        start.Labels.Add(startLabel);
        model.AppendChild(start);

        // Create Process node (Rectangle)
        Rectangle process = new Rectangle(175, 150, 150, 80);
        process.FillStyle.Color = Color.LightBlue;
        process.LineStyle.LineColor = Color.DarkBlue;
        process.LineStyle.LineWidth = 2;
        
        Label processLabel = new Label();
        processLabel.Text = "Process Data";
        process.Labels.Add(processLabel);
        model.AppendChild(process);

        // Create Decision node (Diamond)
        Polygon decision = new Polygon(new PointF[] {
            new PointF(0, 50), new PointF(60, 0),
            new PointF(120, 50), new PointF(60, 100),
            new PointF(0, 50)
        });
        decision.PinPoint = new PointF(250, 300);
        decision.FillStyle.Color = Color.LightYellow;
        decision.LineStyle.LineColor = Color.Orange;
        decision.LineStyle.LineWidth = 2;
        
        Label decisionLabel = new Label();
        decisionLabel.Text = "Valid?";
        decision.Labels.Add(decisionLabel);
        model.AppendChild(decision);

        // Create End node (Ellipse)
        Ellipse end = new Ellipse(200, 450, 100, 60);
        end.FillStyle.Color = Color.LightCoral;
        end.LineStyle.LineColor = Color.DarkRed;
        end.LineStyle.LineWidth = 2;
        
        Label endLabel = new Label();
        endLabel.Text = "End";
        end.Labels.Add(endLabel);
        model.AppendChild(end);

        // Connect Start to Process
        OrthogonalConnector link1 = 
            new OrthogonalConnector(start.PinPoint, process.PinPoint);
        link1.LineStyle.LineColor = Color.Gray;
        link1.LineStyle.LineWidth = 2;
        link1.HeadDecorator.DecoratorShape = DecoratorShape.Filled45Arrow;
        
        start.CentralPort.TryConnect(link1.TailEndPoint);
        process.CentralPort.TryConnect(link1.HeadEndPoint);
        model.AppendChild(link1);

        // Connect Process to Decision
        OrthogonalConnector link2 = 
            new OrthogonalConnector(process.PinPoint, decision.PinPoint);
        link2.LineStyle.LineColor = Color.Gray;
        link2.LineStyle.LineWidth = 2;
        link2.HeadDecorator.DecoratorShape = DecoratorShape.Filled45Arrow;
        
        process.CentralPort.TryConnect(link2.TailEndPoint);
        decision.CentralPort.TryConnect(link2.HeadEndPoint);
        model.AppendChild(link2);

        // Connect Decision to End (Yes path)
        OrthogonalConnector link3 = 
            new OrthogonalConnector(decision.PinPoint, end.PinPoint);
        link3.LineStyle.LineColor = Color.Green;
        link3.LineStyle.LineWidth = 2;
        link3.HeadDecorator.DecoratorShape = DecoratorShape.Filled45Arrow;
        
        // Add label to connector
        Label yesLabel = new Label();
        yesLabel.Text = "Yes";
        link3.Labels.Add(yesLabel);
        
        decision.CentralPort.TryConnect(link3.TailEndPoint);
        end.CentralPort.TryConnect(link3.HeadEndPoint);
        model.AppendChild(link3);

        // Add diagram to form
        this.Controls.Add(diagram);
    }
}
```

### VB.NET Complete Example

```vb
Imports Syncfusion.Windows.Forms.Diagram
Imports Syncfusion.Windows.Forms.Diagram.Controls

Public Class Form1
    Private diagram As Diagram
    Private model As Model

    Public Sub New()
        InitializeComponent()
        CreateFlowchart()
    End Sub

    Private Sub CreateFlowchart()
        'Initialize diagram
        diagram = New Diagram()
        diagram.Size = New Size(800, 600)
        diagram.Location = New Point(10, 10)
        diagram.HScroll = True
        diagram.VScroll = True
        diagram.ShowRulers = True

        'Create model
        model = New Model()
        diagram.Model = model

        'Create Start node
        Dim start As New Ellipse(200, 50, 100, 60)
        start.FillStyle.Color = Color.LightGreen
        start.LineStyle.LineColor = Color.DarkGreen
        start.LineStyle.LineWidth = 2
        
        Dim startLabel As New Label()
        startLabel.Text = "Start"
        start.Labels.Add(startLabel)
        model.AppendChild(start)

        'Add diagram to form
        Me.Controls.Add(diagram)
    End Sub
End Class
```

## Key Takeaways

1. **Always create a Model** and attach it to the Diagram control
2. **Use AppendChild()** to add nodes and connectors to the model
3. **Connect via ports** using `TryConnect()` for proper node connections
4. **Style nodes** using FillStyle, LineStyle, and ShadowStyle properties
5. **Add labels** to make diagrams informative
6. **Choose appropriate connectors** - OrthogonalConnector for flowcharts, LineConnector for simple connections
7. **Enable scrollbars and rulers** for better user experience

## Next Steps

- Explore different node shapes in [nodes-and-shapes.md](nodes-and-shapes.md)
- Learn about connector types in [connectors.md](connectors.md)
- Add interactive drawing tools from [drawing-tools.md](drawing-tools.md)
- Integrate helper controls from [diagram-controls.md](diagram-controls.md)
