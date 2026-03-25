# Diagram Controls

## Table of Contents
- [Overview](#overview)
- [Overview Control](#overview-control)
- [PaletteGroupBar](#palettegroupbar)
- [PaletteGroupView](#palettegroupview)
- [PropertyEditor](#propertyeditor)
- [DocumentExplorer](#documentexplorer)
- [Complete Diagram Editor Example](#complete-diagram-editor-example)

## Overview

Essential Diagram provides five helper controls that work alongside the main Diagram control to create complete diagramming applications:

1. **Overview Control** - Pan/zoom viewport with minimap
2. **PaletteGroupBar** - Symbol palette manager with grouping
3. **PaletteGroupView** - Symbol palette display
4. **PropertyEditor** - Property inspection and editing
5. **DocumentExplorer** - Object hierarchy tree view

## Overview Control

The Overview control provides a miniature view of the entire diagram with a draggable/resizable viewport rectangle for navigation.

### Creating Overview Through Designer

1. Drag **OverviewControl** from Toolbox to form
2. Set the `Diagram` property to your diagram control
3. Configure docking (typically `DockStyle.Left` or `DockStyle.Right`)

### Creating Overview Through Code

```csharp
using Syncfusion.Windows.Forms.Diagram.Controls;

// Create overview control
OverviewControl overview = new OverviewControl();
overview.Dock = DockStyle.Left;
overview.Size = new Size(200, 200);

// Connect to diagram
overview.Diagram = diagram1;

// Add to form
this.Controls.Add(overview);
```

### VB.NET Example

```vb
Dim overview As New OverviewControl()
overview.Dock = DockStyle.Left
overview.Size = New Size(200, 200)
overview.Diagram = diagram1
Me.Controls.Add(overview)
```

### Overview Properties

```csharp
// Appearance
overview.BackColor = Color.White;
overview.BorderStyle = BorderStyle.Fixed3D;

// Viewport styling
overview.ViewportBorderColor = Color.Blue;
overview.ViewportBorderWidth = 2;
overview.ViewportFillColor = Color.FromArgb(50, 0, 0, 255);

// Behavior
overview.ShowScrollbars = false;
```

### Overview Features

- **Pan:** Click and drag viewport rectangle
- **Zoom:** Resize viewport rectangle by dragging edges
- **Quick Navigation:** Click anywhere to center view
- **Real-time Updates:** Automatically reflects diagram changes

## PaletteGroupBar

A GroupBar-based control for managing multiple symbol palettes with drag-and-drop functionality.

### Creating PaletteGroupBar Through Designer

1. Drag **PaletteGroupBar** from Toolbox
2. Use Properties window to load palettes
3. Configure visual style and docking

### Creating PaletteGroupBar Through Code

```csharp
using Syncfusion.Windows.Forms.Diagram.Controls;

// Create palette group bar
PaletteGroupBar paletteBar = new PaletteGroupBar();
paletteBar.Dock = DockStyle.Left;
paletteBar.Width = 250;
paletteBar.Font = new Font("Arial", 9);
paletteBar.BorderStyle = BorderStyle.None;

// Apply visual style
paletteBar.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2007;
paletteBar.TextAlign = Syncfusion.Windows.Forms.Tools.TextAlignment.Left;

// Load symbol palettes
paletteBar.LoadPalette("BasicShapes.edp");
paletteBar.LoadPalette("FlowchartSymbols.edp");
paletteBar.LoadPalette("NetworkSymbols.edp");

// Add to form
this.Controls.Add(paletteBar);
```

### VB.NET Example

```vb
Dim paletteBar As New PaletteGroupBar()
paletteBar.Dock = DockStyle.Left
paletteBar.Width = 250
paletteBar.Font = New Font("Arial", 9)
paletteBar.BorderStyle = BorderStyle.None

paletteBar.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2007
paletteBar.TextAlign = Syncfusion.Windows.Forms.Tools.TextAlignment.Left

paletteBar.LoadPalette("BasicShapes.edp")
paletteBar.LoadPalette("FlowchartSymbols.edp")

Me.Controls.Add(paletteBar)
```

### Loading Palettes

```csharp
// Load from file
paletteBar.LoadPalette("MySymbols.edp");

// Load from stream
using (FileStream stream = File.OpenRead("Symbols.edp"))
{
    paletteBar.LoadPalette(stream);
}

// Check loaded palettes
foreach (var palette in paletteBar.Palettes)
{
    Console.WriteLine($"Loaded: {palette.Name}");
}
```

### Creating Custom Palettes

Use the **Symbol Designer** utility (included with Essential Diagram):

1. Launch Symbol Designer
2. Create new palette or open existing
3. Draw symbols using drawing tools
4. Set symbol properties (fill, border, ports)
5. Save as `.edp` file
6. Load in application using `LoadPalette()`

### PaletteGroupBar Events

```csharp
// Symbol dragged from palette
paletteBar.SymbolDragStart += (sender, e) =>
{
    Symbol symbol = e.Symbol;
    Console.WriteLine($"Dragging: {symbol.Name}");
};

// Symbol dropped on diagram
paletteBar.SymbolDragEnd += (sender, e) =>
{
    Console.WriteLine("Symbol dropped");
};
```

## PaletteGroupView

Displays symbols from a single palette file in a GroupView control.

### Creating PaletteGroupView Through Designer

1. Drag **PaletteGroupView** from Toolbox
2. Use Properties window to set `Palette` property
3. Browse and select `.edp` file

### Creating PaletteGroupView Through Code

```csharp
// Create palette view
PaletteGroupView paletteView = new PaletteGroupView();
paletteView.Dock = DockStyle.Fill;
paletteView.FlatLook = true;
paletteView.BackColor = Color.White;
paletteView.Font = new Font("Arial", 9);

// Load palette
paletteView.LoadPalette("BasicShapes.edp");

// Add to panel or form
panel1.Controls.Add(paletteView);
```

### VB.NET Example

```vb
Dim paletteView As New PaletteGroupView()
paletteView.Dock = DockStyle.Fill
paletteView.FlatLook = True
paletteView.BackColor = Color.White
paletteView.Font = New Font("Arial", 9)

paletteView.LoadPalette("BasicShapes.edp")
panel1.Controls.Add(paletteView)
```

### PaletteGroupView vs PaletteGroupBar

| Feature | PaletteGroupView | PaletteGroupBar |
|---------|------------------|-----------------|
| Multiple palettes | No (single) | Yes (multiple) |
| Visual style | GroupView | GroupBar with tabs |
| Use case | Simple palette display | Full palette manager |
| Serialization | Form resource | External files |

## PropertyEditor

Displays and edits properties of selected diagram objects using an embedded PropertyGrid.

### Creating PropertyEditor Through Designer

1. Drag **PropertyEditor** from Toolbox
2. Set `Diagram` property to your diagram control
3. Configure appearance

### Creating PropertyEditor Through Code

```csharp
// Create property editor
PropertyEditor propertyEditor = new PropertyEditor();
propertyEditor.Dock = DockStyle.Right;
propertyEditor.Width = 300;
propertyEditor.ShowCombo = true; // Show object selector combo

// Connect to diagram
propertyEditor.Diagram = diagram1;

// Add to form
this.Controls.Add(propertyEditor);
```

### VB.NET Example

```vb
Dim propertyEditor As New PropertyEditor()
propertyEditor.Dock = DockStyle.Right
propertyEditor.Width = 300
propertyEditor.ShowCombo = True

propertyEditor.Diagram = diagram1
Me.Controls.Add(propertyEditor)
```

### PropertyEditor Features

```csharp
// Show/hide combo box for object selection
propertyEditor.ShowCombo = true;

// Configure PropertyGrid appearance
propertyEditor.PropertyGrid.HelpVisible = true;
propertyEditor.PropertyGrid.ToolbarVisible = true;
propertyEditor.PropertyGrid.PropertySort = PropertySort.Categorized;

// Handle property changes
propertyEditor.PropertyValueChanged += (sender, e) =>
{
    PropertyValueChangedEventArgs args = e;
    Console.WriteLine($"Changed: {args.ChangedItem.Label} = {args.ChangedItem.Value}");
    
    // Update diagram
    diagram1.UpdateView();
};
```

### Customizing Displayed Properties

```csharp
// Filter properties by category
propertyEditor.PropertyGrid.BrowsableAttributes = new AttributeCollection(
    new CategoryAttribute("Appearance"),
    new CategoryAttribute("Layout")
);

// Hide specific properties
propertyEditor.PropertyGrid.PropertyTabChanged += (s, e) =>
{
    // Custom filtering logic
};
```

## DocumentExplorer

Tree view control showing diagram object hierarchy (layers and nodes).

### Creating DocumentExplorer Through Designer

1. Drag **DocumentExplorer** from Toolbox
2. Configure docking and appearance
3. Attach to diagram model in code

### Creating DocumentExplorer Through Code

```csharp
// Create document explorer
DocumentExplorer docExplorer = new DocumentExplorer();
docExplorer.Dock = DockStyle.Right;
docExplorer.Width = 250;

// Attach to diagram model
docExplorer.AttachModel(diagram1.Model);

// Add to form
this.Controls.Add(docExplorer);
```

### VB.NET Example

```vb
Dim docExplorer As New DocumentExplorer()
docExplorer.Dock = DockStyle.Right
docExplorer.Width = 250

docExplorer.AttachModel(diagram1.Model)
Me.Controls.Add(docExplorer)
```

### DocumentExplorer Features

- **Layers Node** - Lists all layers
- **Nodes Node** - Lists all shapes, links, text
- **Context Menu** - Rename, delete, hide operations
- **Layer Management** - Add new layers
- **Selection** - Click to select object in diagram
- **Hierarchy** - Shows parent-child relationships

### DocumentExplorer Events

```csharp
// Node selected in explorer
docExplorer.NodeSelected += (sender, e) =>
{
    Node selectedNode = e.Node;
    
    // Select in diagram
    diagram1.Model.SelectionList.Clear();
    diagram1.Model.SelectionList.Add(selectedNode);
    diagram1.UpdateView();
};

// Layer visibility changed
docExplorer.LayerVisibilityChanged += (sender, e) =>
{
    Layer layer = e.Layer;
    Console.WriteLine($"Layer {layer.Name}: Visible = {layer.Visible}");
    diagram1.UpdateView();
};
```

### Customizing Tree View

```csharp
// Configure tree view appearance
docExplorer.TreeView.ShowLines = true;
docExplorer.TreeView.ShowRootLines = true;
docExplorer.TreeView.ShowPlusMinus = true;
docExplorer.TreeView.FullRowSelect = true;
docExplorer.TreeView.HideSelection = false;

// Custom icons
docExplorer.TreeView.ImageList = myImageList;
```

## Complete Diagram Editor Example

Full-featured diagram editor with all helper controls:

```csharp
using Syncfusion.Windows.Forms.Diagram;
using Syncfusion.Windows.Forms.Diagram.Controls;

public class DiagramEditorForm : Form
{
    private Diagram diagram;
    private Model model;
    private OverviewControl overview;
    private PaletteGroupBar paletteBar;
    private PropertyEditor propertyEditor;
    private DocumentExplorer docExplorer;
    private SplitContainer mainSplit;
    private SplitContainer rightSplit;

    public DiagramEditorForm()
    {
        InitializeComponent();
        SetupControls();
        SetupLayout();
    }

    private void SetupControls()
    {
        // Create diagram
        diagram = new Diagram();
        diagram.HScroll = true;
        diagram.VScroll = true;
        diagram.ShowRulers = true;
        
        model = new Model();
        diagram.Model = model;

        // Create overview
        overview = new OverviewControl();
        overview.Diagram = diagram;
        overview.Height = 200;
        overview.Dock = DockStyle.Top;

        // Create palette bar
        paletteBar = new PaletteGroupBar();
        paletteBar.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2007;
        paletteBar.LoadPalette("BasicShapes.edp");
        paletteBar.LoadPalette("FlowchartSymbols.edp");
        paletteBar.Dock = DockStyle.Fill;

        // Create property editor
        propertyEditor = new PropertyEditor();
        propertyEditor.Diagram = diagram;
        propertyEditor.ShowCombo = true;
        propertyEditor.Dock = DockStyle.Fill;

        // Create document explorer
        docExplorer = new DocumentExplorer();
        docExplorer.AttachModel(model);
        docExplorer.Dock = DockStyle.Fill;
    }

    private void SetupLayout()
    {
        // Main split: Left (palette+overview) | Right (diagram+properties)
        mainSplit = new SplitContainer();
        mainSplit.Dock = DockStyle.Fill;
        mainSplit.SplitterDistance = 250;
        mainSplit.FixedPanel = FixedPanel.Panel1;

        // Left panel: Overview + Palette
        Panel leftPanel = new Panel();
        leftPanel.Dock = DockStyle.Fill;
        
        overview.Dock = DockStyle.Top;
        paletteBar.Dock = DockStyle.Fill;
        
        leftPanel.Controls.Add(paletteBar);
        leftPanel.Controls.Add(overview);
        
        mainSplit.Panel1.Controls.Add(leftPanel);

        // Right split: Diagram | Properties+Explorer
        rightSplit = new SplitContainer();
        rightSplit.Dock = DockStyle.Fill;
        rightSplit.SplitterDistance = 600;
        rightSplit.FixedPanel = FixedPanel.Panel2;

        // Center: Diagram
        diagram.Dock = DockStyle.Fill;
        rightSplit.Panel1.Controls.Add(diagram);

        // Right panel: Properties + Explorer tabs
        TabControl rightTabs = new TabControl();
        rightTabs.Dock = DockStyle.Fill;
        
        TabPage propTab = new TabPage("Properties");
        propTab.Controls.Add(propertyEditor);
        
        TabPage explorerTab = new TabPage("Explorer");
        explorerTab.Controls.Add(docExplorer);
        
        rightTabs.TabPages.Add(propTab);
        rightTabs.TabPages.Add(explorerTab);
        
        rightSplit.Panel2.Controls.Add(rightTabs);

        mainSplit.Panel2.Controls.Add(rightSplit);

        // Add main split to form
        this.Controls.Add(mainSplit);
        
        // Set form properties
        this.Text = "Diagram Editor";
        this.Size = new Size(1200, 800);
        this.WindowState = FormWindowState.Maximized;
    }
}
```

### Alternative Layout: Docked Controls

```csharp
private void SetupDockedLayout()
{
    // Left: Palette + Overview
    Panel leftPanel = new Panel();
    leftPanel.Dock = DockStyle.Left;
    leftPanel.Width = 250;
    
    overview.Dock = DockStyle.Top;
    overview.Height = 200;
    paletteBar.Dock = DockStyle.Fill;
    
    leftPanel.Controls.Add(paletteBar);
    leftPanel.Controls.Add(overview);

    // Right: Properties + Explorer
    Panel rightPanel = new Panel();
    rightPanel.Dock = DockStyle.Right;
    rightPanel.Width = 300;
    
    propertyEditor.Dock = DockStyle.Top;
    propertyEditor.Height = 400;
    docExplorer.Dock = DockStyle.Fill;
    
    rightPanel.Controls.Add(docExplorer);
    rightPanel.Controls.Add(propertyEditor);

    // Center: Diagram
    diagram.Dock = DockStyle.Fill;

    // Add to form
    this.Controls.Add(diagram);
    this.Controls.Add(rightPanel);
    this.Controls.Add(leftPanel);
}
```

## Best Practices

### Control Initialization Order

```csharp
// 1. Create diagram and model first
Diagram diagram = new Diagram();
Model model = new Model();
diagram.Model = model;

// 2. Create and attach helper controls
OverviewControl overview = new OverviewControl();
overview.Diagram = diagram;  // After diagram is created

PropertyEditor propertyEditor = new PropertyEditor();
propertyEditor.Diagram = diagram;  // After diagram is created

DocumentExplorer docExplorer = new DocumentExplorer();
docExplorer.AttachModel(model);  // After model is created

// 3. Add to form
this.Controls.Add(diagram);
this.Controls.Add(overview);
this.Controls.Add(propertyEditor);
this.Controls.Add(docExplorer);
```

### Responsive Layout

```csharp
// Use SplitContainer for resizable panels
SplitContainer splitter = new SplitContainer();
splitter.Dock = DockStyle.Fill;
splitter.SplitterDistance = 250;

// Allow user to resize
splitter.IsSplitterFixed = false;

// Set minimum sizes
splitter.Panel1MinSize = 200;
splitter.Panel2MinSize = 400;
```

### Synchronizing Controls

```csharp
// Ensure controls stay synchronized
diagram.Model.SelectionList.SelectionChanged += (s, e) =>
{
    // Update property editor
    propertyEditor.Refresh();
    
    // Highlight in document explorer
    if (diagram.Model.SelectionList.Count > 0)
    {
        Node selected = diagram.Model.SelectionList[0];
        docExplorer.SelectNode(selected);
    }
};
```

## Next Steps

- Learn about layout tools in [layout-organization.md](layout-organization.md)
- Add labels and ports in [labels-ports.md](labels-ports.md)
- Configure view settings in [view-controls.md](view-controls.md)
- Explore advanced features in [advanced-features.md](advanced-features.md)
