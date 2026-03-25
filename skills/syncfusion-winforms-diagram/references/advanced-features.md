# Advanced Features

Advanced techniques including undo/redo, printing, custom shapes, symbol palettes, and performance optimization.

## Table of Contents
- [Undo and Redo](#undo-and-redo)
- [Printing](#printing)
- [Custom Shapes](#custom-shapes)
- [Symbol Palettes](#symbol-palettes)
- [Performance Optimization](#performance-optimization)
- [Multi-Page Diagrams](#multi-page-diagrams)

## Undo and Redo

### Enable History Tracking

```csharp
// History is enabled by default
diagram1.Model.HistoryManager.Enabled = true;

// Set history limit
diagram1.Model.HistoryManager.MaxHistoryCount = 100; // Default 50
```

### Undo/Redo Operations

```csharp
// Undo
private void Undo()
{
    if (diagram1.Model.HistoryManager.CanUndo)
    {
        diagram1.Model.HistoryManager.Undo();
        diagram1.UpdateView();
    }
}

// Redo
private void Redo()
{
    if (diagram1.Model.HistoryManager.CanRedo)
    {
        diagram1.Model.HistoryManager.Redo();
        diagram1.UpdateView();
    }
}

// Check state
bool canUndo = diagram1.Model.HistoryManager.CanUndo;
bool canRedo = diagram1.Model.HistoryManager.CanRedo;
```

### Clear History

```csharp
// Clear undo/redo stack
diagram1.Model.HistoryManager.Clear();
```

### Pause History Tracking

```csharp
// Temporarily disable history (for bulk operations)
diagram1.Model.HistoryManager.Enabled = false;

// Perform operations
for (int i = 0; i < 100; i++)
{
    Rectangle node = new Rectangle(i * 20, i * 20, 50, 30);
    diagram1.Model.AppendChild(node);
}

// Re-enable history
diagram1.Model.HistoryManager.Enabled = true;
```

### Custom History Actions

```csharp
// Create composite action for atomic undo/redo
diagram1.Model.HistoryManager.StartAtomicAction("Add Multiple Nodes");

try
{
    Rectangle node1 = new Rectangle(100, 100, 100, 60);
    Rectangle node2 = new Rectangle(300, 100, 100, 60);
    OrthogonalConnector link = new OrthogonalConnector(
        node1.PinPoint,
        node2.PinPoint
    );
    
    diagram1.Model.AppendChild(node1);
    diagram1.Model.AppendChild(node2);
    diagram1.Model.AppendChild(link);
}
finally
{
    diagram1.Model.HistoryManager.EndAtomicAction();
}

// Now all three operations undo/redo as one action
```

## Printing

### Basic Printing

```csharp
private void PrintDiagram()
{
    PrintDocument printDoc = new PrintDocument();
    printDoc.PrintPage += PrintDoc_PrintPage;
    
    PrintDialog printDialog = new PrintDialog();
    printDialog.Document = printDoc;
    
    if (printDialog.ShowDialog() == DialogResult.OK)
    {
        printDoc.Print();
    }
}

private void PrintDoc_PrintPage(object sender, PrintPageEventArgs e)
{
    Graphics g = e.Graphics;
    
    // Get diagram bounds
    RectangleF bounds = diagram1.Model.BoundingRectangle;
    
    // Calculate scale to fit page
    float scaleX = e.MarginBounds.Width / bounds.Width;
    float scaleY = e.MarginBounds.Height / bounds.Height;
    float scale = Math.Min(scaleX, scaleY);
    
    // Apply transformations
    g.TranslateTransform(e.MarginBounds.X, e.MarginBounds.Y);
    g.ScaleTransform(scale, scale);
    g.TranslateTransform(-bounds.X, -bounds.Y);
    
    // Draw diagram
    diagram1.Model.Draw(g);
    
    e.HasMorePages = false;
}
```

### Print Preview

```csharp
private void ShowPrintPreview()
{
    PrintDocument printDoc = new PrintDocument();
    printDoc.PrintPage += PrintDoc_PrintPage;
    
    PrintPreviewDialog previewDialog = new PrintPreviewDialog();
    previewDialog.Document = printDoc;
    previewDialog.ShowDialog();
}
```

### Multi-Page Printing

```csharp
private int currentPage = 0;
private int totalPages = 1;

private void PrintMultiPage()
{
    // Calculate pages needed
    RectangleF bounds = diagram1.Model.BoundingRectangle;
    PrintDocument printDoc = new PrintDocument();
    
    float pageWidth = printDoc.DefaultPageSettings.PrintableArea.Width;
    float pageHeight = printDoc.DefaultPageSettings.PrintableArea.Height;
    
    int pagesWide = (int)Math.Ceiling(bounds.Width / pageWidth);
    int pagesTall = (int)Math.Ceiling(bounds.Height / pageHeight);
    totalPages = pagesWide * pagesTall;
    currentPage = 0;
    
    printDoc.PrintPage += PrintDoc_PrintPageMulti;
    
    PrintDialog printDialog = new PrintDialog();
    printDialog.Document = printDoc;
    
    if (printDialog.ShowDialog() == DialogResult.OK)
    {
        printDoc.Print();
    }
}

private void PrintDoc_PrintPageMulti(object sender, PrintPageEventArgs e)
{
    Graphics g = e.Graphics;
    
    RectangleF bounds = diagram1.Model.BoundingRectangle;
    float pageWidth = e.MarginBounds.Width;
    float pageHeight = e.MarginBounds.Height;
    
    int pagesWide = (int)Math.Ceiling(bounds.Width / pageWidth);
    int col = currentPage % pagesWide;
    int row = currentPage / pagesWide;
    
    // Calculate visible region
    RectangleF visibleRegion = new RectangleF(
        bounds.X + col * pageWidth,
        bounds.Y + row * pageHeight,
        pageWidth,
        pageHeight
    );
    
    // Draw diagram section
    g.TranslateTransform(e.MarginBounds.X - visibleRegion.X, e.MarginBounds.Y - visibleRegion.Y);
    g.SetClip(visibleRegion);
    diagram1.Model.Draw(g);
    
    currentPage++;
    e.HasMorePages = currentPage < totalPages;
}
```

## Custom Shapes

### GraphicsPath Shape

```csharp
public class CustomShape : Node
{
    public CustomShape(float x, float y, float width, float height)
        : base(x, y, width, height)
    {
    }
    
    protected override void DrawShape(Graphics g)
    {
        // Create custom path (star shape)
        GraphicsPath path = new GraphicsPath();
        
        PointF center = new PointF(Width / 2, Height / 2);
        float outerRadius = Math.Min(Width, Height) / 2;
        float innerRadius = outerRadius * 0.4f;
        int points = 5;
        
        PointF[] starPoints = new PointF[points * 2];
        for (int i = 0; i < points * 2; i++)
        {
            float angle = i * (float)Math.PI / points - (float)Math.PI / 2;
            float radius = (i % 2 == 0) ? outerRadius : innerRadius;
            
            starPoints[i] = new PointF(
                center.X + radius * (float)Math.Cos(angle),
                center.Y + radius * (float)Math.Sin(angle)
            );
        }
        
        path.AddPolygon(starPoints);
        
        // Fill and draw
        g.FillPath(new SolidBrush(FillStyle.Color), path);
        g.DrawPath(new Pen(LineStyle.LineColor, LineStyle.LineWidth), path);
    }
}

// Usage
CustomShape star = new CustomShape(100, 100, 100, 100);
diagram1.Model.AppendChild(star);
```

### Image-Based Node

```csharp
public class ImageNode : Node
{
    private Image image;
    
    public ImageNode(float x, float y, float width, float height, string imagePath)
        : base(x, y, width, height)
    {
        this.image = Image.FromFile(imagePath);
    }
    
    protected override void DrawShape(Graphics g)
    {
        // Draw border
        g.FillRectangle(new SolidBrush(FillStyle.Color), BoundingRectangle);
        g.DrawRectangle(
            new Pen(LineStyle.LineColor, LineStyle.LineWidth),
            Rectangle.Round(BoundingRectangle)
        );
        
        // Draw image
        RectangleF imageBounds = new RectangleF(
            PinPoint.X - Width / 2 + 5,
            PinPoint.Y - Height / 2 + 5,
            Width - 10,
            Height - 10
        );
        
        g.DrawImage(image, imageBounds);
    }
    
    protected override void Dispose(bool disposing)
    {
        if (disposing && image != null)
        {
            image.Dispose();
        }
        base.Dispose(disposing);
    }
}

// Usage
ImageNode imageNode = new ImageNode(200, 200, 150, 150, "icon.png");
diagram1.Model.AppendChild(imageNode);
```

### Composite Shape

```csharp
public class CompositeShape : Group
{
    public CompositeShape(float x, float y)
        : base()
    {
        // Create composite from multiple shapes
        Rectangle header = new Rectangle(x, y, 150, 30);
        header.FillStyle.Color = Color.DarkBlue;
        
        Rectangle body = new Rectangle(x, y + 30, 150, 100);
        body.FillStyle.Color = Color.LightBlue;
        
        Label titleLabel = new Label();
        titleLabel.Text = "Header";
        titleLabel.Position = Position.TopCenter;
        titleLabel.FontColorStyle.Color = Color.White;
        header.Labels.Add(titleLabel);
        
        // Add to group
        this.AppendChild(header);
        this.AppendChild(body);
    }
}

// Usage
CompositeShape composite = new CompositeShape(100, 100);
diagram1.Model.AppendChild(composite);
```

## Symbol Palettes

### Create Symbol Palette

```csharp
// Create PaletteGroupBar
PaletteGroupBar paletteBar = new PaletteGroupBar();
paletteBar.Dock = DockStyle.Left;
paletteBar.Width = 200;

// Create PaletteGroupView for basic shapes
PaletteGroupView basicShapes = new PaletteGroupView();
basicShapes.Name = "Basic Shapes";

// Add symbols
Rectangle rectSymbol = new Rectangle(0, 0, 80, 50);
rectSymbol.Name = "Rectangle";
rectSymbol.FillStyle.Color = Color.LightBlue;
basicShapes.AppendChild(rectSymbol);

Ellipse ellipseSymbol = new Ellipse(0, 0, 80, 50);
ellipseSymbol.Name = "Ellipse";
ellipseSymbol.FillStyle.Color = Color.LightGreen;
basicShapes.AppendChild(ellipseSymbol);

Polygon triangleSymbol = new Polygon(new PointF[]
{
    new PointF(40, 0),
    new PointF(80, 50),
    new PointF(0, 50)
});
triangleSymbol.Name = "Triangle";
triangleSymbol.FillStyle.Color = Color.LightYellow;
basicShapes.AppendChild(triangleSymbol);

// Add view to bar
paletteBar.AppendChild(basicShapes);

// Add to form
this.Controls.Add(paletteBar);

// Link to diagram
diagram1.Model.PaletteGroupBar = paletteBar;
```

### Categorized Palette

```csharp
private void CreateCategorizedPalette()
{
    PaletteGroupBar paletteBar = new PaletteGroupBar();
    paletteBar.Dock = DockStyle.Left;
    paletteBar.Width = 200;
    
    // Flowchart category
    PaletteGroupView flowchart = new PaletteGroupView();
    flowchart.Name = "Flowchart";
    flowchart.AppendChild(CreateFlowchartSymbol("Process", FlowchartShapeType.Process));
    flowchart.AppendChild(CreateFlowchartSymbol("Decision", FlowchartShapeType.Decision));
    flowchart.AppendChild(CreateFlowchartSymbol("Data", FlowchartShapeType.Data));
    paletteBar.AppendChild(flowchart);
    
    // Network category
    PaletteGroupView network = new PaletteGroupView();
    network.Name = "Network";
    network.AppendChild(CreateNetworkSymbol("Server", "server.png"));
    network.AppendChild(CreateNetworkSymbol("Router", "router.png"));
    network.AppendChild(CreateNetworkSymbol("Switch", "switch.png"));
    paletteBar.AppendChild(network);
    
    // UML category
    PaletteGroupView uml = new PaletteGroupView();
    uml.Name = "UML";
    uml.AppendChild(CreateUMLSymbol("Class"));
    uml.AppendChild(CreateUMLSymbol("Interface"));
    paletteBar.AppendChild(uml);
    
    this.Controls.Add(paletteBar);
    diagram1.Model.PaletteGroupBar = paletteBar;
}

private Node CreateFlowchartSymbol(string name, FlowchartShapeType type)
{
    FlowchartNode node = new FlowchartNode(0, 0, 80, 50);
    node.ShapeType = type;
    node.Name = name;
    
    Label label = new Label();
    label.Text = name;
    label.Position = Position.BottomCenter;
    label.FontStyle.Size = 8;
    node.Labels.Add(label);
    
    return node;
}

private Node CreateNetworkSymbol(string name, string iconPath)
{
    ImageNode node = new ImageNode(0, 0, 80, 60, iconPath);
    node.Name = name;
    return node;
}

private Node CreateUMLSymbol(string name)
{
    Rectangle node = new Rectangle(0, 0, 100, 80);
    node.Name = name;
    
    Label label = new Label();
    label.Text = $"<<{name}>>";
    label.Position = Position.TopCenter;
    node.Labels.Add(label);
    
    return node;
}
```

### Drag and Drop from Palette

```csharp
// Drag and drop is automatic when using PaletteGroupBar
// Each symbol dropped creates a copy at the drop location

// To customize drop behavior:
diagram1.DragDrop += (sender, e) =>
{
    // Get dropped node
    Point clientPoint = diagram1.PointToClient(new Point(e.X, e.Y));
    PointF diagramPoint = diagram1.View.PixelToWorldPoint(clientPoint);
    
    // Access the dropped node through the model's most recent addition
    if (diagram1.Model.Nodes.Count > 0)
    {
        Node newNode = diagram1.Model.Nodes[diagram1.Model.Nodes.Count - 1];
        
        // Customize the node
        newNode.FillStyle.Color = Color.LightCyan;
        
        // Add metadata
        newNode.Tag = new { CreatedTime = DateTime.Now };
    }
};
```

## Performance Optimization

### Batch Updates

```csharp
// Suspend updates during bulk operations
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
    diagram1.UpdateView();
}
```

### Virtual Rendering

```csharp
// Enable virtual rendering for large diagrams
diagram1.View.EnableVirtualRendering = true;

// Only visible nodes are rendered
// Improves performance with 1000+ nodes
```

### Disable Anti-Aliasing

```csharp
// Disable for better performance
diagram1.View.SmoothingMode = SmoothingMode.None;
diagram1.View.TextRenderingHint = TextRenderingHint.SystemDefault;

// Re-enable for quality
diagram1.View.SmoothingMode = SmoothingMode.AntiAlias;
diagram1.View.TextRenderingHint = TextRenderingHint.AntiAlias;
```

### Lazy Loading

```csharp
public class LazyDiagramLoader
{
    private Diagram diagram;
    private List<NodeData> nodeData;
    private bool loaded = false;
    
    public LazyDiagramLoader(Diagram diagram, List<NodeData> data)
    {
        this.diagram = diagram;
        this.nodeData = data;
        
        // Load on first view
        diagram.Paint += OnFirstPaint;
    }
    
    private void OnFirstPaint(object sender, PaintEventArgs e)
    {
        if (!loaded)
        {
            LoadNodes();
            loaded = true;
            diagram.Paint -= OnFirstPaint;
        }
    }
    
    private void LoadNodes()
    {
        diagram.Model.BeginUpdate();
        
        try
        {
            foreach (var data in nodeData)
            {
                Rectangle node = new Rectangle(
                    data.X, data.Y,
                    data.Width, data.Height
                );
                node.FillStyle.Color = data.Color;
                diagram.Model.AppendChild(node);
            }
        }
        finally
        {
            diagram.Model.EndUpdate();
        }
    }
}

public class NodeData
{
    public float X { get; set; }
    public float Y { get; set; }
    public float Width { get; set; }
    public float Height { get; set; }
    public Color Color { get; set; }
}
```

### Caching

```csharp
// Cache node bitmaps for complex custom shapes
public class CachedCustomNode : Node
{
    private Bitmap cache;
    private bool cacheValid = false;
    
    protected override void DrawShape(Graphics g)
    {
        if (!cacheValid || cache == null)
        {
            // Render to cache
            cache = new Bitmap((int)Width, (int)Height);
            using (Graphics cacheGraphics = Graphics.FromImage(cache))
            {
                DrawComplexShape(cacheGraphics);
            }
            cacheValid = true;
        }
        
        // Draw cached bitmap
        g.DrawImage(cache, PinPoint.X - Width / 2, PinPoint.Y - Height / 2);
    }
    
    private void DrawComplexShape(Graphics g)
    {
        // Expensive rendering operations
        // ...
    }
    
    protected override void OnPropertyChanged(string propertyName)
    {
        base.OnPropertyChanged(propertyName);
        
        // Invalidate cache on size/style changes
        if (propertyName == "Width" || propertyName == "Height" || 
            propertyName == "FillStyle" || propertyName == "LineStyle")
        {
            cacheValid = false;
        }
    }
}
```

## Multi-Page Diagrams

### Create Multi-Page Document

```csharp
public class MultiPageDiagram
{
    private List<DiagramModel> pages;
    private Diagram viewer;
    private int currentPage = 0;
    
    public MultiPageDiagram(Diagram viewer)
    {
        this.viewer = viewer;
        this.pages = new List<DiagramModel>();
        
        // Create first page
        AddPage();
    }
    
    public void AddPage()
    {
        DiagramModel page = new DiagramModel();
        pages.Add(page);
    }
    
    public void ShowPage(int pageIndex)
    {
        if (pageIndex >= 0 && pageIndex < pages.Count)
        {
            currentPage = pageIndex;
            viewer.Model = pages[pageIndex];
            viewer.UpdateView();
        }
    }
    
    public void NextPage()
    {
        if (currentPage < pages.Count - 1)
        {
            ShowPage(currentPage + 1);
        }
    }
    
    public void PreviousPage()
    {
        if (currentPage > 0)
        {
            ShowPage(currentPage - 1);
        }
    }
    
    public int PageCount => pages.Count;
    public int CurrentPage => currentPage;
}

// Usage
MultiPageDiagram multiPage = new MultiPageDiagram(diagram1);
multiPage.AddPage();
multiPage.ShowPage(0);
```

### Page Navigation UI

```csharp
public class PageNavigationControl : Panel
{
    private MultiPageDiagram multiPage;
    private Label pageLabel;
    
    public PageNavigationControl(MultiPageDiagram multiPage)
    {
        this.multiPage = multiPage;
        this.Height = 40;
        this.Dock = DockStyle.Bottom;
        
        // Previous button
        Button prevBtn = new Button { Text = "< Previous", Width = 80 };
        prevBtn.Click += (s, e) => {
            multiPage.PreviousPage();
            UpdateLabel();
        };
        Controls.Add(prevBtn);
        
        // Page label
        pageLabel = new Label { Width = 100, TextAlign = ContentAlignment.MiddleCenter };
        pageLabel.Left = prevBtn.Right + 10;
        UpdateLabel();
        Controls.Add(pageLabel);
        
        // Next button
        Button nextBtn = new Button { Text = "Next >", Width = 80 };
        nextBtn.Left = pageLabel.Right + 10;
        nextBtn.Click += (s, e) => {
            multiPage.NextPage();
            UpdateLabel();
        };
        Controls.Add(nextBtn);
        
        // Add page button
        Button addBtn = new Button { Text = "+ Add Page", Width = 90 };
        addBtn.Left = nextBtn.Right + 20;
        addBtn.Click += (s, e) => {
            multiPage.AddPage();
            multiPage.ShowPage(multiPage.PageCount - 1);
            UpdateLabel();
        };
        Controls.Add(addBtn);
    }
    
    private void UpdateLabel()
    {
        pageLabel.Text = $"Page {multiPage.CurrentPage + 1} of {multiPage.PageCount}";
    }
}

// Usage
MultiPageDiagram multiPage = new MultiPageDiagram(diagram1);
PageNavigationControl pageNav = new PageNavigationControl(multiPage);
this.Controls.Add(pageNav);
```

## Next Steps

- Review common issues in [troubleshooting.md](troubleshooting.md)
- Return to main guide: [SKILL.md](../SKILL.md)
- Explore specific features in other reference files
