# Layout Management

Apply automatic layouts to organize nodes in predefined patterns like hierarchical trees, org charts, radial layouts, and more.

## Table of Contents
- [Hierarchical Layout](#hierarchical-layout)
- [Radial Tree Layout](#radial-tree-layout)
- [Symmetric Layout](#symmetric-layout)
- [Organizational Chart Layout](#organizational-chart-layout)
- [Custom Layout](#custom-layout)

## Hierarchical Layout

Arrange nodes in a tree structure with parent nodes at the top and children below.

### Basic Hierarchical Layout

```csharp
// Create layout manager
HierarchicalLayoutManager layoutManager = new HierarchicalLayoutManager(
    diagram1.Model,
    diagram1.View
);

// Configure layout
layoutManager.VerticalSpacing = 50;
layoutManager.HorizontalSpacing = 30;
layoutManager.Orientation = Orientation.TopToBottom;

// Apply layout
layoutManager.LayoutNodes();

// Update view
diagram1.UpdateView();
```

### Hierarchical Layout Properties

```csharp
HierarchicalLayoutManager layout = new HierarchicalLayoutManager(
    diagram1.Model,
    diagram1.View
);

// Spacing
layout.VerticalSpacing = 60;      // Space between levels
layout.HorizontalSpacing = 40;    // Space between siblings

// Orientation
layout.Orientation = Orientation.TopToBottom;  // Root at top
// Or: BottomToTop, LeftToRight, RightToLeft

// Alignment
layout.Alignment = HorizontalAlignment.Center; // Center-align children
// Or: Left, Right

// Margins
layout.Bounds = new RectangleF(50, 50, 1000, 800);

// Apply layout
layout.LayoutNodes();
diagram1.UpdateView();
```

### Hierarchical Layout with Data Binding

```csharp
// Data source
DataTable employees = new DataTable();
employees.Columns.Add("ID", typeof(int));
employees.Columns.Add("Name", typeof(string));
employees.Columns.Add("ManagerID", typeof(int));

employees.Rows.Add(1, "CEO", 0);
employees.Rows.Add(2, "VP Sales", 1);
employees.Rows.Add(3, "VP Eng", 1);
employees.Rows.Add(4, "Sales Rep 1", 2);
employees.Rows.Add(5, "Sales Rep 2", 2);
employees.Rows.Add(6, "Developer 1", 3);

// Bind data
diagram1.Model.DataSourceSettings.DataSource = employees;
diagram1.Model.DataSourceSettings.Id = "ID";
diagram1.Model.DataSourceSettings.ParentId = "ManagerID";

// Configure node appearance
diagram1.NodeRendered += (evtArgs) =>
{
    DataRowView row = evtArgs.DataSourceNode as DataRowView;
    if (row != null)
    {
        Node node = evtArgs.Node;
        Label label = new Label();
        label.Text = row["Name"].ToString();
        label.Position = Position.Center;
        node.Labels.Add(label);
    }
};

// Apply hierarchical layout
HierarchicalLayoutManager layout = new HierarchicalLayoutManager(
    diagram1.Model,
    diagram1.View
);
layout.VerticalSpacing = 50;
layout.HorizontalSpacing = 30;
layout.LayoutNodes();
diagram1.UpdateView();
```

### Multi-Root Hierarchical Layout

```csharp
// Create multiple root nodes
Rectangle root1 = new Rectangle(100, 100, 100, 60);
Rectangle root2 = new Rectangle(300, 100, 100, 60);

// Create children for each root
Rectangle child1 = new Rectangle(100, 200, 100, 60);
Rectangle child2 = new Rectangle(100, 300, 100, 60);

// Connect
OrthogonalConnector link1 = new OrthogonalConnector(root1.PinPoint, child1.PinPoint);
OrthogonalConnector link2 = new OrthogonalConnector(root1.PinPoint, child2.PinPoint);

diagram1.Model.AppendChild(root1);
diagram1.Model.AppendChild(root2);
diagram1.Model.AppendChild(child1);
diagram1.Model.AppendChild(child2);
diagram1.Model.AppendChild(link1);
diagram1.Model.AppendChild(link2);

// Layout handles multiple roots automatically
HierarchicalLayoutManager layout = new HierarchicalLayoutManager(
    diagram1.Model,
    diagram1.View
);
layout.LayoutNodes();
diagram1.UpdateView();
```

## Radial Tree Layout

Arrange nodes in a circular pattern with the root at the center.

### Basic Radial Layout

```csharp
// Create radial layout manager
RadialTreeLayoutManager layoutManager = new RadialTreeLayoutManager(
    diagram1.Model,
    diagram1.View
);

// Configure layout
layoutManager.Radius = 100;              // Distance from center
layoutManager.RadiusIncrement = 80;     // Spacing between levels
layoutManager.AngularSector = 360;      // Full circle

// Apply layout
layoutManager.LayoutNodes();
diagram1.UpdateView();
```

### Radial Layout Properties

```csharp
RadialTreeLayoutManager layout = new RadialTreeLayoutManager(
    diagram1.Model,
    diagram1.View
);

// Radius settings
layout.Radius = 120;              // Starting radius
layout.RadiusIncrement = 100;     // Increase per level

// Angular settings
layout.AngularSector = 270;       // Use 270 degrees (3/4 circle)
layout.StartAngle = 0;            // Start at 0 degrees (right)

// Center position
layout.CenterNode = diagram1.Model.Nodes[0]; // Specify center node

// Apply layout
layout.LayoutNodes();
diagram1.UpdateView();
```

### Radial Layout Example

```csharp
// Create network diagram with radial layout
private void CreateRadialNetwork()
{
    // Center node
    Ellipse center = new Ellipse(400, 300, 80, 80);
    center.FillStyle.Color = Color.Gold;
    Label centerLabel = new Label { Text = "Server", Position = Position.Center };
    center.Labels.Add(centerLabel);
    diagram1.Model.AppendChild(center);
    
    // Client nodes
    for (int i = 0; i < 8; i++)
    {
        Ellipse client = new Ellipse(0, 0, 60, 60);
        client.FillStyle.Color = Color.LightBlue;
        Label label = new Label { Text = $"Client {i + 1}", Position = Position.Center };
        client.Labels.Add(label);
        diagram1.Model.AppendChild(client);
        
        // Connect to center
        LineConnector link = new LineConnector(center.PinPoint, client.PinPoint);
        diagram1.Model.AppendChild(link);
    }
    
    // Apply radial layout
    RadialTreeLayoutManager layout = new RadialTreeLayoutManager(
        diagram1.Model,
        diagram1.View
    );
    layout.Radius = 150;
    layout.LayoutNodes();
    diagram1.UpdateView();
}
```

## Symmetric Layout

Create balanced, symmetrical layouts.

### Basic Symmetric Layout

```csharp
// Create symmetric layout manager
SymmetricLayoutManager layoutManager = new SymmetricLayoutManager(
    diagram1.Model,
    diagram1.View
);

// Configure layout
layoutManager.SpringLength = 80;
layoutManager.SpringStiffness = 0.5f;
layoutManager.MaximumIterations = 1000;

// Apply layout
layoutManager.LayoutNodes();
diagram1.UpdateView();
```

### Force-Directed Layout

```csharp
SymmetricLayoutManager layout = new SymmetricLayoutManager(
    diagram1.Model,
    diagram1.View
);

// Force settings
layout.SpringLength = 100;           // Desired edge length
layout.SpringStiffness = 0.4f;       // Edge stiffness (0-1)
layout.RepulsionRange = 200;         // Node repulsion distance
layout.MaximumIterations = 2000;     // Convergence iterations

// Bounds
layout.Bounds = new RectangleF(0, 0, 1000, 800);

// Apply layout
layout.LayoutNodes();
diagram1.UpdateView();
```

## Organizational Chart Layout

Specialized layout for organizational hierarchies.

### Org Chart with Assistant Positions

```csharp
// Create org chart with assistants
private void CreateOrgChart()
{
    // CEO
    Rectangle ceo = CreateOrgNode("CEO", Color.Gold);
    diagram1.Model.AppendChild(ceo);
    
    // Executive Assistant (assistant position)
    Rectangle assistant = CreateOrgNode("Exec Assistant", Color.LightYellow);
    diagram1.Model.AppendChild(assistant);
    OrthogonalConnector assistLink = new OrthogonalConnector(
        ceo.PinPoint, 
        assistant.PinPoint
    );
    assistLink.LineStyle.DashStyle = DashStyle.Dash; // Dash for assistant
    diagram1.Model.AppendChild(assistLink);
    
    // VPs (direct reports)
    string[] vpTitles = { "VP Sales", "VP Engineering", "VP Finance" };
    foreach (string title in vpTitles)
    {
        Rectangle vp = CreateOrgNode(title, Color.LightBlue);
        diagram1.Model.AppendChild(vp);
        
        OrthogonalConnector link = new OrthogonalConnector(
            ceo.PinPoint,
            vp.PinPoint
        );
        diagram1.Model.AppendChild(link);
    }
    
    // Apply layout
    HierarchicalLayoutManager layout = new HierarchicalLayoutManager(
        diagram1.Model,
        diagram1.View
    );
    layout.VerticalSpacing = 60;
    layout.HorizontalSpacing = 40;
    layout.Orientation = Orientation.TopToBottom;
    layout.LayoutNodes();
    diagram1.UpdateView();
}

private Rectangle CreateOrgNode(string title, Color color)
{
    Rectangle node = new Rectangle(0, 0, 120, 60);
    node.FillStyle.Color = color;
    Label label = new Label { Text = title, Position = Position.Center };
    node.Labels.Add(label);
    return node;
}
```

## Custom Layout

Implement custom layout algorithms.

### Grid Layout

```csharp
public class GridLayoutManager
{
    private DiagramModel model;
    private int columns;
    private float cellWidth;
    private float cellHeight;
    private float horizontalSpacing;
    private float verticalSpacing;
    
    public GridLayoutManager(DiagramModel model, int columns = 4)
    {
        this.model = model;
        this.columns = columns;
        this.cellWidth = 120;
        this.cellHeight = 80;
        this.horizontalSpacing = 20;
        this.verticalSpacing = 20;
    }
    
    public void LayoutNodes()
    {
        int row = 0;
        int col = 0;
        
        foreach (Node node in model.Nodes)
        {
            // Calculate position
            float x = col * (cellWidth + horizontalSpacing);
            float y = row * (cellHeight + verticalSpacing);
            
            node.PinPoint = new PointF(x, y);
            node.Width = cellWidth;
            node.Height = cellHeight;
            
            // Move to next position
            col++;
            if (col >= columns)
            {
                col = 0;
                row++;
            }
        }
    }
}

// Usage
GridLayoutManager gridLayout = new GridLayoutManager(diagram1.Model, 5);
gridLayout.LayoutNodes();
diagram1.UpdateView();
```

### Circular Layout

```csharp
public class CircularLayoutManager
{
    private DiagramModel model;
    private PointF center;
    private float radius;
    
    public CircularLayoutManager(DiagramModel model, PointF center, float radius)
    {
        this.model = model;
        this.center = center;
        this.radius = radius;
    }
    
    public void LayoutNodes()
    {
        int nodeCount = model.Nodes.Count;
        if (nodeCount == 0) return;
        
        float angleIncrement = 360f / nodeCount;
        float currentAngle = 0;
        
        foreach (Node node in model.Nodes)
        {
            // Convert angle to radians
            float radians = currentAngle * (float)Math.PI / 180f;
            
            // Calculate position
            float x = center.X + radius * (float)Math.Cos(radians);
            float y = center.Y + radius * (float)Math.Sin(radians);
            
            node.PinPoint = new PointF(x, y);
            
            currentAngle += angleIncrement;
        }
    }
}

// Usage
CircularLayoutManager circularLayout = new CircularLayoutManager(
    diagram1.Model,
    new PointF(400, 300),
    200
);
circularLayout.LayoutNodes();
diagram1.UpdateView();
```

### Layered Layout

```csharp
public class LayeredLayoutManager
{
    private DiagramModel model;
    private Dictionary<Node, int> nodeLayers;
    
    public float HorizontalSpacing { get; set; } = 50;
    public float VerticalSpacing { get; set; } = 80;
    
    public LayeredLayoutManager(DiagramModel model)
    {
        this.model = model;
        this.nodeLayers = new Dictionary<Node, int>();
    }
    
    public void LayoutNodes()
    {
        // Assign layers
        AssignLayers();
        
        // Position nodes
        PositionNodesInLayers();
    }
    
    private void AssignLayers()
    {
        // Find root nodes (no incoming connections)
        List<Node> roots = new List<Node>();
        foreach (Node node in model.Nodes)
        {
            bool hasIncoming = false;
            foreach (Connector conn in model.Connectors)
            {
                if (conn.HeadNode == node)
                {
                    hasIncoming = true;
                    break;
                }
            }
            
            if (!hasIncoming)
            {
                roots.Add(node);
                nodeLayers[node] = 0;
            }
        }
        
        // BFS to assign layers
        Queue<Node> queue = new Queue<Node>(roots);
        while (queue.Count > 0)
        {
            Node current = queue.Dequeue();
            int currentLayer = nodeLayers[current];
            
            // Find children
            foreach (Connector conn in model.Connectors)
            {
                if (conn.TailNode == current && !nodeLayers.ContainsKey(conn.HeadNode))
                {
                    nodeLayers[conn.HeadNode] = currentLayer + 1;
                    queue.Enqueue(conn.HeadNode);
                }
            }
        }
    }
    
    private void PositionNodesInLayers()
    {
        // Group nodes by layer
        Dictionary<int, List<Node>> layers = new Dictionary<int, List<Node>>();
        foreach (var kvp in nodeLayers)
        {
            if (!layers.ContainsKey(kvp.Value))
                layers[kvp.Value] = new List<Node>();
            layers[kvp.Value].Add(kvp.Key);
        }
        
        // Position each layer
        foreach (var layer in layers)
        {
            int layerIndex = layer.Key;
            List<Node> nodes = layer.Value;
            
            float y = layerIndex * VerticalSpacing;
            float totalWidth = (nodes.Count - 1) * HorizontalSpacing;
            float startX = -totalWidth / 2;
            
            for (int i = 0; i < nodes.Count; i++)
            {
                float x = startX + i * HorizontalSpacing;
                nodes[i].PinPoint = new PointF(x, y);
            }
        }
    }
}

// Usage
LayeredLayoutManager layeredLayout = new LayeredLayoutManager(diagram1.Model);
layeredLayout.HorizontalSpacing = 60;
layeredLayout.VerticalSpacing = 100;
layeredLayout.LayoutNodes();
diagram1.UpdateView();
```

## Layout Animations

### Animate Layout Transitions

```csharp
public class LayoutAnimator
{
    private Diagram diagram;
    private Dictionary<Node, PointF> startPositions;
    private Dictionary<Node, PointF> endPositions;
    private System.Windows.Forms.Timer animationTimer;
    private int currentFrame = 0;
    private int totalFrames = 30;
    
    public LayoutAnimator(Diagram diagram)
    {
        this.diagram = diagram;
        this.animationTimer = new System.Windows.Forms.Timer();
        this.animationTimer.Interval = 16; // ~60 FPS
        this.animationTimer.Tick += AnimationTimer_Tick;
    }
    
    public void AnimateToLayout(ILayoutManager layoutManager)
    {
        // Save current positions
        startPositions = new Dictionary<Node, PointF>();
        foreach (Node node in diagram.Model.Nodes)
        {
            startPositions[node] = node.PinPoint;
        }
        
        // Apply layout to get end positions
        layoutManager.LayoutNodes();
        
        // Save end positions
        endPositions = new Dictionary<Node, PointF>();
        foreach (Node node in diagram.Model.Nodes)
        {
            endPositions[node] = node.PinPoint;
        }
        
        // Restore start positions
        foreach (Node node in diagram.Model.Nodes)
        {
            node.PinPoint = startPositions[node];
        }
        
        // Start animation
        currentFrame = 0;
        animationTimer.Start();
    }
    
    private void AnimationTimer_Tick(object sender, EventArgs e)
    {
        currentFrame++;
        
        if (currentFrame >= totalFrames)
        {
            // End animation
            animationTimer.Stop();
            
            // Ensure final positions
            foreach (Node node in diagram.Model.Nodes)
            {
                node.PinPoint = endPositions[node];
            }
        }
        else
        {
            // Interpolate positions
            float progress = (float)currentFrame / totalFrames;
            progress = EaseInOutQuad(progress); // Easing function
            
            foreach (Node node in diagram.Model.Nodes)
            {
                PointF start = startPositions[node];
                PointF end = endPositions[node];
                
                float x = start.X + (end.X - start.X) * progress;
                float y = start.Y + (end.Y - start.Y) * progress;
                
                node.PinPoint = new PointF(x, y);
            }
        }
        
        diagram.UpdateView();
    }
    
    private float EaseInOutQuad(float t)
    {
        return t < 0.5f ? 2 * t * t : 1 - (float)Math.Pow(-2 * t + 2, 2) / 2;
    }
}

// Usage
LayoutAnimator animator = new LayoutAnimator(diagram1);

HierarchicalLayoutManager layout = new HierarchicalLayoutManager(
    diagram1.Model,
    diagram1.View
);
animator.AnimateToLayout(layout);
```

## Layout Toolbar Example

```csharp
public class LayoutToolbar : ToolStrip
{
    private Diagram diagram;
    
    public LayoutToolbar(Diagram diagram)
    {
        this.diagram = diagram;
        InitializeControls();
    }
    
    private void InitializeControls()
    {
        // Hierarchical
        ToolStripButton hierarchicalBtn = new ToolStripButton("Hierarchical");
        hierarchicalBtn.Click += (s, e) => ApplyHierarchicalLayout();
        Items.Add(hierarchicalBtn);
        
        // Radial
        ToolStripButton radialBtn = new ToolStripButton("Radial");
        radialBtn.Click += (s, e) => ApplyRadialLayout();
        Items.Add(radialBtn);
        
        // Grid
        ToolStripButton gridBtn = new ToolStripButton("Grid");
        gridBtn.Click += (s, e) => ApplyGridLayout();
        Items.Add(gridBtn);
        
        // Circular
        ToolStripButton circularBtn = new ToolStripButton("Circular");
        circularBtn.Click += (s, e) => ApplyCircularLayout();
        Items.Add(circularBtn);
    }
    
    private void ApplyHierarchicalLayout()
    {
        HierarchicalLayoutManager layout = new HierarchicalLayoutManager(
            diagram.Model,
            diagram.View
        );
        layout.VerticalSpacing = 60;
        layout.HorizontalSpacing = 40;
        layout.LayoutNodes();
        diagram.UpdateView();
    }
    
    private void ApplyRadialLayout()
    {
        RadialTreeLayoutManager layout = new RadialTreeLayoutManager(
            diagram.Model,
            diagram.View
        );
        layout.Radius = 100;
        layout.RadiusIncrement = 80;
        layout.LayoutNodes();
        diagram.UpdateView();
    }
    
    private void ApplyGridLayout()
    {
        GridLayoutManager layout = new GridLayoutManager(diagram.Model, 4);
        layout.LayoutNodes();
        diagram.UpdateView();
    }
    
    private void ApplyCircularLayout()
    {
        RectangleF bounds = diagram.Model.BoundingRectangle;
        PointF center = new PointF(
            bounds.X + bounds.Width / 2,
            bounds.Y + bounds.Height / 2
        );
        
        CircularLayoutManager layout = new CircularLayoutManager(
            diagram.Model,
            center,
            200
        );
        layout.LayoutNodes();
        diagram.UpdateView();
    }
}

// Usage
LayoutToolbar layoutToolbar = new LayoutToolbar(diagram1);
this.Controls.Add(layoutToolbar);
```

## Best Practices

1. **Apply layouts after adding nodes**: Add all nodes first, then apply layout
2. **Consider layout type**: Choose layout based on diagram structure (tree, network, etc.)
3. **Adjust spacing**: Tune spacing values for optimal readability
4. **Animate transitions**: Use animation for smooth layout changes
5. **Save manual adjustments**: Allow users to manually adjust after automatic layout
6. **Re-layout on changes**: Provide button to re-apply layout after modifications

## Next Steps

- Explore advanced features in [advanced-features.md](advanced-features.md)
- Review common issues in [troubleshooting.md](troubleshooting.md)
- Return to main guide: [SKILL.md](../SKILL.md)
