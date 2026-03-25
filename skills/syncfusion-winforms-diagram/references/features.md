# Diagram Features

## Table of Contents
- [Context Menu](#context-menu)
- [Serialization](#serialization)
- [Data Binding](#data-binding)
- [Dynamic Properties](#dynamic-properties)
- [Event Handlers](#event-handlers)
- [Clipboard Operations](#clipboard-operations)
- [Guidelines and Snapping](#guidelines-and-snapping)

## Context Menu

Customize right-click menus for nodes, connectors, and the diagram.

### Node Context Menu

```csharp
// Create BarManager for context menu
BarManager barManager = new BarManager();
barManager.Form = this;

// Create PopupMenu
PopupMenu nodeMenu = new PopupMenu(barManager);

// Add menu items
BarButtonItem deleteItem = new BarButtonItem(barManager, "Delete");
deleteItem.ItemClick += DeleteItem_ItemClick;
nodeMenu.AddItem(deleteItem);

BarButtonItem propertiesItem = new BarButtonItem(barManager, "Properties");
propertiesItem.ItemClick += PropertiesItem_ItemClick;
nodeMenu.AddItem(propertiesItem);

// Assign to diagram
diagram1.View.NodeContextMenu = nodeMenu;

// Event handlers
private void DeleteItem_ItemClick(object sender, ItemClickEventArgs e)
{
    foreach (Node node in diagram1.Model.SelectedNodes)
    {
        diagram1.Model.RemoveChild(node);
    }
}

private void PropertiesItem_ItemClick(object sender, ItemClickEventArgs e)
{
    // Show properties dialog
}
```

### Diagram Context Menu

```csharp
PopupMenu diagramMenu = new PopupMenu(barManager);

// Paste
BarButtonItem pasteItem = new BarButtonItem(barManager, "Paste");
pasteItem.ItemClick += (s, e) => {
    diagram1.Controller.Paste();
};
diagramMenu.AddItem(pasteItem);

// Select All
BarButtonItem selectAllItem = new BarButtonItem(barManager, "Select All");
selectAllItem.ItemClick += (s, e) => {
    diagram1.Controller.SelectAll();
};
diagramMenu.AddItem(selectAllItem);

diagram1.View.DiagramContextMenu = diagramMenu;
```

### Conditional Menu Items

```csharp
private void Diagram_MouseUp(object sender, MouseEventArgs e)
{
    if (e.Button == MouseButtons.Right)
    {
        PopupMenu menu = new PopupMenu(barManager);
        
        if (diagram1.Model.SelectedNodes.Count > 0)
        {
            // Node operations
            menu.AddItem(new BarButtonItem(barManager, "Delete"));
            menu.AddItem(new BarButtonItem(barManager, "Duplicate"));
        }
        else
        {
            // Diagram operations
            menu.AddItem(new BarButtonItem(barManager, "Paste"));
            menu.AddItem(new BarButtonItem(barManager, "Select All"));
        }
        
        menu.ShowPopup(PointToScreen(e.Location));
    }
}
```

## Serialization

Save and load diagrams to/from files.

### Save Diagram to File

```csharp
private void SaveDiagram(string filePath)
{
    // Save as binary
    using (FileStream stream = new FileStream(filePath, FileMode.Create))
    {
        AppDomain.CurrentDomain.AssemblyResolve += CurrentDomain_AssemblyResolve;
        
        BinaryFormatter formatter = new BinaryFormatter();
        formatter.Serialize(stream, diagram1.Model);
        
        AppDomain.CurrentDomain.AssemblyResolve -= CurrentDomain_AssemblyResolve;
    }
}

// Handle assembly resolution for deserialization
private Assembly CurrentDomain_AssemblyResolve(object sender, ResolveEventArgs args)
{
    return typeof(Diagram).Assembly;
}
```

### Load Diagram from File

```csharp
private void LoadDiagram(string filePath)
{
    using (FileStream stream = new FileStream(filePath, FileMode.Open))
    {
        AppDomain.CurrentDomain.AssemblyResolve += CurrentDomain_AssemblyResolve;
        
        BinaryFormatter formatter = new BinaryFormatter();
        DiagramModel model = (DiagramModel)formatter.Deserialize(stream);
        
        diagram1.Model = model;
        
        AppDomain.CurrentDomain.AssemblyResolve -= CurrentDomain_AssemblyResolve;
    }
}
```

### Save/Load with Dialog

```csharp
private void SaveDiagramDialog()
{
    SaveFileDialog saveDialog = new SaveFileDialog();
    saveDialog.Filter = "Diagram Files (*.diagram)|*.diagram|All Files (*.*)|*.*";
    saveDialog.DefaultExt = "diagram";
    
    if (saveDialog.ShowDialog() == DialogResult.OK)
    {
        SaveDiagram(saveDialog.FileName);
    }
}

private void LoadDiagramDialog()
{
    OpenFileDialog openDialog = new OpenFileDialog();
    openDialog.Filter = "Diagram Files (*.diagram)|*.diagram|All Files (*.*)|*.*";
    
    if (openDialog.ShowDialog() == DialogResult.OK)
    {
        LoadDiagram(openDialog.FileName);
    }
}
```

### Export to Image

```csharp
private void ExportToImage(string filePath, ImageFormat format)
{
    // Get diagram bounds
    RectangleF bounds = diagram1.Model.BoundingRectangle;
    
    // Create bitmap
    Bitmap bitmap = new Bitmap(
        (int)bounds.Width + 20,
        (int)bounds.Height + 20
    );
    
    // Draw diagram
    Graphics g = Graphics.FromImage(bitmap);
    g.TranslateTransform(-bounds.X + 10, -bounds.Y + 10);
    diagram1.Model.Draw(g);
    
    // Save image
    bitmap.Save(filePath, format);
    
    g.Dispose();
    bitmap.Dispose();
}

// Usage
ExportToImage("diagram.png", ImageFormat.Png);
ExportToImage("diagram.jpg", ImageFormat.Jpeg);
```

## Data Binding

Bind diagram to data sources for automated diagram generation.

### Basic Data Binding

```csharp
// Create data table
DataTable dataTable = new DataTable("Employees");
dataTable.Columns.Add("ID", typeof(int));
dataTable.Columns.Add("Name", typeof(string));
dataTable.Columns.Add("Role", typeof(string));
dataTable.Columns.Add("ManagerID", typeof(int));

dataTable.Rows.Add(1, "CEO", "Executive", 0);
dataTable.Rows.Add(2, "CTO", "Technology", 1);
dataTable.Rows.Add(3, "CFO", "Finance", 1);
dataTable.Rows.Add(4, "Developer 1", "Engineering", 2);
dataTable.Rows.Add(5, "Developer 2", "Engineering", 2);

// Bind to diagram
diagram1.Model.DataSourceSettings.DataSource = dataTable;
diagram1.Model.DataSourceSettings.Id = "ID";
diagram1.Model.DataSourceSettings.ParentId = "ManagerID";

// Configure node appearance
diagram1.NodeRendered += Diagram_NodeRendered;

private void Diagram_NodeRendered(NodeRenderingEventArgs evtArgs)
{
    DataRowView row = evtArgs.DataSourceNode as DataRowView;
    if (row != null)
    {
        Node node = evtArgs.Node;
        
        // Set label
        Label label = new Label();
        label.Text = row["Name"].ToString();
        label.Position = Position.Center;
        node.Labels.Add(label);
        
        // Style based on role
        string role = row["Role"].ToString();
        if (role == "Executive")
        {
            node.FillStyle.Color = Color.Gold;
        }
        else if (role == "Technology" || role == "Finance")
        {
            node.FillStyle.Color = Color.LightBlue;
        }
        else
        {
            node.FillStyle.Color = Color.LightGray;
        }
    }
}
```

### Hierarchical Layout with Data

```csharp
// Apply hierarchical layout
HierarchicalLayoutManager layoutManager = new HierarchicalLayoutManager(
    diagram1.Model,
    diagram1.View
);

layoutManager.VerticalSpacing = 40;
layoutManager.HorizontalSpacing = 30;
layoutManager.LayoutNodes();

diagram1.UpdateView();
```

### Custom Data Object Binding

```csharp
public class Employee
{
    public int ID { get; set; }
    public string Name { get; set; }
    public string Title { get; set; }
    public int ManagerID { get; set; }
}

// Create data
List<Employee> employees = new List<Employee>
{
    new Employee { ID = 1, Name = "Alice", Title = "CEO", ManagerID = 0 },
    new Employee { ID = 2, Name = "Bob", Title = "CTO", ManagerID = 1 },
    new Employee { ID = 3, Name = "Charlie", Title = "Dev", ManagerID = 2 }
};

// Bind
diagram1.Model.DataSourceSettings.DataSource = employees;
diagram1.Model.DataSourceSettings.Id = "ID";
diagram1.Model.DataSourceSettings.ParentId = "ManagerID";
```

## Dynamic Properties

Runtime property access and modification.

### Getting Node Properties

```csharp
// Access node properties
Node node = diagram1.Model.Nodes[0];

// Basic properties
string name = node.Name;
PointF location = node.PinPoint;
SizeF size = node.Size;
float rotation = node.RotationAngle;

// Style properties
Color fillColor = node.FillStyle.Color;
Color borderColor = node.LineStyle.LineColor;
float borderWidth = node.LineStyle.LineWidth;

// Appearance properties
bool visible = node.Visible;
bool enabled = node.Enabled;
int zOrder = diagram1.Model.GetZOrder(node);
```

### Setting Node Properties Dynamically

```csharp
private void UpdateNodeDynamically(Node node, Dictionary<string, object> properties)
{
    foreach (var kvp in properties)
    {
        switch (kvp.Key)
        {
            case "Width":
                node.Width = Convert.ToSingle(kvp.Value);
                break;
            case "Height":
                node.Height = Convert.ToSingle(kvp.Value);
                break;
            case "FillColor":
                node.FillStyle.Color = (Color)kvp.Value;
                break;
            case "BorderColor":
                node.LineStyle.LineColor = (Color)kvp.Value;
                break;
            case "BorderWidth":
                node.LineStyle.LineWidth = Convert.ToSingle(kvp.Value);
                break;
            case "Visible":
                node.Visible = Convert.ToBoolean(kvp.Value);
                break;
        }
    }
    
    diagram1.UpdateView();
}

// Usage
UpdateNodeDynamically(node, new Dictionary<string, object>
{
    { "Width", 150f },
    { "Height", 100f },
    { "FillColor", Color.LightBlue },
    { "BorderWidth", 3f }
});
```

### Custom Properties

```csharp
// Add custom properties using PropertyDescriptor
public class NodeMetadata
{
    public string Category { get; set; }
    public DateTime CreatedDate { get; set; }
    public string Owner { get; set; }
    public Dictionary<string, object> CustomData { get; set; }
}

// Store metadata with node
Node node = new Rectangle(100, 100, 100, 60);
NodeMetadata metadata = new NodeMetadata
{
    Category = "Process",
    CreatedDate = DateTime.Now,
    Owner = "John Doe",
    CustomData = new Dictionary<string, object>()
};

// Store in node's Tag property
node.Tag = metadata;

// Retrieve later
NodeMetadata retrievedMetadata = node.Tag as NodeMetadata;
if (retrievedMetadata != null)
{
    Console.WriteLine($"Category: {retrievedMetadata.Category}");
}
```

## Event Handlers

Respond to diagram and node events.

### Node Events

```csharp
// Node selection
diagram1.Model.NodeSelected += (sender, e) =>
{
    Console.WriteLine($"Node selected: {e.Node.Name}");
};

// Node property changed
diagram1.Model.NodePropertyChanged += (sender, e) =>
{
    Console.WriteLine($"Property changed: {e.PropertyName}");
};

// Node added
diagram1.Model.NodeAdded += (sender, e) =>
{
    Console.WriteLine($"Node added: {e.Node.Name}");
};

// Node removed
diagram1.Model.NodeRemoved += (sender, e) =>
{
    Console.WriteLine($"Node removed: {e.Node.Name}");
};
```

### Connector Events

```csharp
// Connection created
diagram1.Model.ConnectionsChanged += (sender, e) =>
{
    if (e.ChangeType == CollectionExChangeType.Insert)
    {
        IConnector connector = e.Element as IConnector;
        Console.WriteLine($"Connection created");
    }
};

// Connection changed
diagram1.Model.ConnectionChanged += (sender, e) =>
{
    Console.WriteLine($"Connection modified");
};
```

### Mouse Events

```csharp
// Mouse down on node
diagram1.MouseDown += (sender, e) =>
{
    Node node = diagram1.Controller.HitTest(e.Location);
    if (node != null)
    {
        Console.WriteLine($"Clicked on: {node.Name}");
    }
};

// Double-click
diagram1.MouseDoubleClick += (sender, e) =>
{
    Node node = diagram1.Controller.HitTest(e.Location);
    if (node != null)
    {
        // Show edit dialog
        EditNodeProperties(node);
    }
};
```

### Drag and Drop Events

```csharp
// Drag over
diagram1.DragOver += (sender, e) =>
{
    e.Effect = DragDropEffects.Copy;
};

// Drop
diagram1.DragDrop += (sender, e) =>
{
    string data = e.Data.GetData(DataFormats.Text) as string;
    Point clientPoint = diagram1.PointToClient(new Point(e.X, e.Y));
    PointF diagramPoint = diagram1.View.PixelToWorldPoint(clientPoint);
    
    // Create node at drop location
    Rectangle node = new Rectangle(diagramPoint.X, diagramPoint.Y, 100, 60);
    Label label = new Label { Text = data, Position = Position.Center };
    node.Labels.Add(label);
    
    diagram1.Model.AppendChild(node);
};
```

## Clipboard Operations

Copy, cut, and paste operations.

### Copy Selected Nodes

```csharp
private void CopyNodes()
{
    if (diagram1.Model.SelectedNodes.Count > 0)
    {
        diagram1.Controller.Copy();
    }
}
```

### Cut Selected Nodes

```csharp
private void CutNodes()
{
    if (diagram1.Model.SelectedNodes.Count > 0)
    {
        diagram1.Controller.Cut();
    }
}
```

### Paste Nodes

```csharp
private void PasteNodes()
{
    diagram1.Controller.Paste();
}
```

### Duplicate Nodes

```csharp
private void DuplicateNodes()
{
    if (diagram1.Model.SelectedNodes.Count > 0)
    {
        // Copy
        diagram1.Controller.Copy();
        
        // Paste with offset
        diagram1.Controller.Paste();
        
        // Offset pasted nodes
        foreach (Node node in diagram1.Model.SelectedNodes)
        {
            node.PinPoint = new PointF(
                node.PinPoint.X + 20,
                node.PinPoint.Y + 20
            );
        }
        
        diagram1.UpdateView();
    }
}
```

### Custom Clipboard Format

```csharp
private void CopyToCustomClipboard()
{
    List<NodeData> nodeDataList = new List<NodeData>();
    
    foreach (Node node in diagram1.Model.SelectedNodes)
    {
        NodeData data = new NodeData
        {
            Type = node.GetType().Name,
            Location = node.PinPoint,
            Size = node.Size,
            FillColor = node.FillStyle.Color
        };
        nodeDataList.Add(data);
    }
    
    // Serialize and store
    string json = JsonConvert.SerializeObject(nodeDataList);
    Clipboard.SetText(json);
}

[Serializable]
public class NodeData
{
    public string Type { get; set; }
    public PointF Location { get; set; }
    public SizeF Size { get; set; }
    public Color FillColor { get; set; }
}
```

## Guidelines and Snapping

Visual guides and snap-to-grid functionality.

### Enable Grid

```csharp
// Show gridlines
diagram1.Model.RenderStyle.ShowGrid = true;

// Configure grid appearance
diagram1.Model.GridStyle.GridColor = Color.LightGray;
diagram1.Model.GridStyle.GridDashStyle = DashStyle.Dot;
diagram1.Model.GridStyle.MajorGrid = 20;  // Major grid spacing
diagram1.Model.GridStyle.MinorGrid = 5;   // Minor grid spacing
```

### Snap to Grid

```csharp
// Enable snap to grid
diagram1.Model.SnapToGrid = true;
diagram1.Model.SnapGridSize = new SizeF(10, 10); // 10-pixel grid
```

### Guidelines

```csharp
// Enable alignment guidelines
diagram1.Model.ShowGuidelines = true;

// Configure guideline appearance
diagram1.Model.GuidelineStyle.LineColor = Color.Red;
diagram1.Model.GuidelineStyle.LineWidth = 1;
diagram1.Model.GuidelineStyle.DashStyle = DashStyle.Dash;
```

### Snap to Objects

```csharp
// Enable snap to nearby objects
diagram1.Model.SnapToObject = true;
diagram1.Model.SnapObjectDistance = 5; // pixels
```

### Rulers

```csharp
// Show rulers
diagram1.View.ShowHorizontalRuler = true;
diagram1.View.ShowVerticalRuler = true;

// Configure ruler appearance
diagram1.View.RulerStyle.BackColor = Color.WhiteSmoke;
diagram1.View.RulerStyle.TextColor = Color.Black;
diagram1.View.RulerStyle.TickColor = Color.Gray;
```

## Complete Feature Integration Example

```csharp
public class DiagramFeatureManager
{
    private Diagram diagram;
    
    public DiagramFeatureManager(Diagram diagram)
    {
        this.diagram = diagram;
        InitializeFeatures();
    }
    
    private void InitializeFeatures()
    {
        // Grid and snapping
        diagram.Model.RenderStyle.ShowGrid = true;
        diagram.Model.SnapToGrid = true;
        diagram.Model.SnapGridSize = new SizeF(10, 10);
        
        // Guidelines
        diagram.Model.ShowGuidelines = true;
        
        // Rulers
        diagram.View.ShowHorizontalRuler = true;
        diagram.View.ShowVerticalRuler = true;
        
        // Events
        diagram.Model.NodeSelected += OnNodeSelected;
        diagram.Model.NodeAdded += OnNodeAdded;
        diagram.MouseDoubleClick += OnMouseDoubleClick;
    }
    
    private void OnNodeSelected(object sender, NodeEventArgs e)
    {
        Console.WriteLine($"Selected: {e.Node.Name}");
    }
    
    private void OnNodeAdded(object sender, NodeAddedEventArgs e)
    {
        Console.WriteLine($"Added: {e.Node.Name}");
    }
    
    private void OnMouseDoubleClick(object sender, MouseEventArgs e)
    {
        Node node = diagram.Controller.HitTest(e.Location);
        if (node != null)
        {
            ShowPropertiesDialog(node);
        }
    }
    
    private void ShowPropertiesDialog(Node node)
    {
        // Implementation
    }
    
    public void SaveDiagram(string filePath)
    {
        using (FileStream stream = new FileStream(filePath, FileMode.Create))
        {
            BinaryFormatter formatter = new BinaryFormatter();
            formatter.Serialize(stream, diagram.Model);
        }
    }
    
    public void LoadDiagram(string filePath)
    {
        using (FileStream stream = new FileStream(filePath, FileMode.Open))
        {
            BinaryFormatter formatter = new BinaryFormatter();
            diagram.Model = (DiagramModel)formatter.Deserialize(stream);
        }
    }
}
```

## Next Steps

- Configure user interaction in [user-interaction.md](user-interaction.md)
- Adjust view settings in [view-controls.md](view-controls.md)
- Apply automatic layouts in [layout-management.md](layout-management.md)
- Learn advanced techniques in [advanced-features.md](advanced-features.md)
