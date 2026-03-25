# View Controls

Configure zoom, pan, rulers, grids, and other visual controls for the diagram view.

## Zoom

### Programmatic Zoom

```csharp
// Set zoom level (1.0 = 100%)
diagram1.View.ZoomFactor = 1.5f;  // 150%

// Get current zoom
float currentZoom = diagram1.View.ZoomFactor;
Console.WriteLine($"Current zoom: {currentZoom * 100}%");
```

### Zoom In/Out

```csharp
// Zoom in (increase by 10%)
private void ZoomIn()
{
    float newZoom = diagram1.View.ZoomFactor * 1.1f;
    diagram1.View.ZoomFactor = Math.Min(newZoom, 5.0f); // Max 500%
}

// Zoom out (decrease by 10%)
private void ZoomOut()
{
    float newZoom = diagram1.View.ZoomFactor * 0.9f;
    diagram1.View.ZoomFactor = Math.Max(newZoom, 0.1f); // Min 10%
}
```

### Zoom to Fit

```csharp
// Fit entire diagram in view
private void ZoomToFit()
{
    RectangleF bounds = diagram1.Model.BoundingRectangle;
    
    float zoomX = diagram1.ClientSize.Width / bounds.Width;
    float zoomY = diagram1.ClientSize.Height / bounds.Height;
    
    diagram1.View.ZoomFactor = Math.Min(zoomX, zoomY) * 0.9f; // 90% to add margin
    
    // Center diagram
    diagram1.View.ScrollVirtualBounds(bounds);
}
```

### Zoom to Selection

```csharp
// Zoom to selected nodes
private void ZoomToSelection()
{
    if (diagram1.Model.SelectedNodes.Count > 0)
    {
        RectangleF selectionBounds = GetSelectionBounds();
        
        float zoomX = diagram1.ClientSize.Width / selectionBounds.Width;
        float zoomY = diagram1.ClientSize.Height / selectionBounds.Height;
        
        diagram1.View.ZoomFactor = Math.Min(zoomX, zoomY) * 0.8f;
        diagram1.View.ScrollVirtualBounds(selectionBounds);
    }
}

private RectangleF GetSelectionBounds()
{
    float minX = float.MaxValue, minY = float.MaxValue;
    float maxX = float.MinValue, maxY = float.MinValue;
    
    foreach (Node node in diagram1.Model.SelectedNodes)
    {
        RectangleF bounds = node.BoundingRectangle;
        minX = Math.Min(minX, bounds.Left);
        minY = Math.Min(minY, bounds.Top);
        maxX = Math.Max(maxX, bounds.Right);
        maxY = Math.Max(maxY, bounds.Bottom);
    }
    
    return new RectangleF(minX, minY, maxX - minX, maxY - minY);
}
```

### Zoom at Point

```csharp
// Zoom centered at mouse position
private void ZoomAtPoint(PointF point, float factor)
{
    // Get world coordinates before zoom
    PointF worldPoint = diagram1.View.PixelToWorldPoint(Point.Round(point));
    
    // Apply zoom
    diagram1.View.ZoomFactor *= factor;
    
    // Adjust scroll to keep world point at same screen position
    PointF newScreenPoint = diagram1.View.WorldToPixelPoint(worldPoint);
    diagram1.View.ScrollBy(
        (int)(newScreenPoint.X - point.X),
        (int)(newScreenPoint.Y - point.Y)
    );
}

// Mouse wheel zoom
diagram1.MouseWheel += (sender, e) =>
{
    if (Control.ModifierKeys == Keys.Control)
    {
        float factor = e.Delta > 0 ? 1.1f : 0.9f;
        ZoomAtPoint(e.Location, factor);
        ((HandledMouseEventArgs)e).Handled = true;
    }
};
```

### Zoom Presets

```csharp
// Common zoom levels
private void SetZoom25() => diagram1.View.ZoomFactor = 0.25f;
private void SetZoom50() => diagram1.View.ZoomFactor = 0.5f;
private void SetZoom100() => diagram1.View.ZoomFactor = 1.0f;
private void SetZoom150() => diagram1.View.ZoomFactor = 1.5f;
private void SetZoom200() => diagram1.View.ZoomFactor = 2.0f;
```

## Pan (Scrolling)

### Programmatic Scrolling

```csharp
// Scroll to specific point
diagram1.View.ScrollTo(new Point(100, 100));

// Scroll by offset
diagram1.View.ScrollBy(50, 30); // 50 pixels right, 30 pixels down

// Get current scroll position
Point scrollPosition = diagram1.View.ScrollPosition;
```

### Pan to Node

```csharp
// Center view on a specific node
private void PanToNode(Node node)
{
    PointF nodeCenter = node.PinPoint;
    Point screenCenter = new Point(
        diagram1.ClientSize.Width / 2,
        diagram1.ClientSize.Height / 2
    );
    
    // Convert node center to screen coordinates
    PointF screenPoint = diagram1.View.WorldToPixelPoint(nodeCenter);
    
    // Calculate scroll offset
    int offsetX = (int)(screenPoint.X - screenCenter.X);
    int offsetY = (int)(screenPoint.Y - screenCenter.Y);
    
    diagram1.View.ScrollBy(offsetX, offsetY);
}
```

### Auto-Pan During Drag

```csharp
// Enable auto-pan when dragging near edges
diagram1.Model.EnableAutoPan = true;
diagram1.Model.AutoPanMargin = 50; // Pan when within 50 pixels of edge
```

### Pan Tool

```csharp
// Enable pan tool (hold Space and drag)
PanTool panTool = new PanTool(diagram1.Controller);
panTool.ActivateOnModifierKeys = Keys.Space;
diagram1.Controller.Tools.Add(panTool);
```

### Scroll Boundaries

```csharp
// Set virtual bounds (scrollable area)
diagram1.Model.VirtualBounds = new RectangleF(-1000, -1000, 5000, 5000);

// Auto-expand on node creation
diagram1.Model.ExpandModelBounds = true;
```

## Rulers

### Enable Rulers

```csharp
// Show horizontal ruler
diagram1.View.ShowHorizontalRuler = true;

// Show vertical ruler
diagram1.View.ShowVerticalRuler = true;
```

### Ruler Appearance

```csharp
// Configure ruler style
diagram1.View.RulerStyle.BackColor = Color.WhiteSmoke;
diagram1.View.RulerStyle.TextColor = Color.Black;
diagram1.View.RulerStyle.TickColor = Color.Gray;
diagram1.View.RulerStyle.MajorTickHeight = 10;
diagram1.View.RulerStyle.MinorTickHeight = 5;
```

### Ruler Units

```csharp
// Set measurement units
diagram1.Model.MeasurementUnits = MeasurementUnits.Pixel;
// Or: Inch, Millimeter, Centimeter, Point, etc.

// Display units on ruler
diagram1.View.RulerStyle.ShowUnits = true;
```

### Ruler Origin

```csharp
// Set ruler origin point
diagram1.View.RulerOrigin = new PointF(0, 0);

// Change origin (e.g., for different coordinate systems)
diagram1.View.RulerOrigin = new PointF(100, 100);
```

## Grid

### Show/Hide Grid

```csharp
// Show grid
diagram1.Model.RenderStyle.ShowGrid = true;

// Hide grid
diagram1.Model.RenderStyle.ShowGrid = false;
```

### Grid Appearance

```csharp
// Configure grid style
diagram1.Model.GridStyle.GridColor = Color.LightGray;
diagram1.Model.GridStyle.GridDashStyle = DashStyle.Dot;
diagram1.Model.GridStyle.GridLineWidth = 1;
```

### Grid Spacing

```csharp
// Set major grid spacing
diagram1.Model.GridStyle.MajorGrid = 50;  // 50 units

// Set minor grid spacing
diagram1.Model.GridStyle.MinorGrid = 10;  // 10 units

// Show only major grid
diagram1.Model.GridStyle.ShowMinorGrid = false;
```

### Grid Pattern

```csharp
// Dot grid
diagram1.Model.GridStyle.GridPattern = GridPattern.Dot;

// Line grid
diagram1.Model.GridStyle.GridPattern = GridPattern.Line;

// Cross grid
diagram1.Model.GridStyle.GridPattern = GridPattern.Cross;
```

### Snap to Grid

```csharp
// Enable snap to grid
diagram1.Model.SnapToGrid = true;

// Set snap grid size
diagram1.Model.SnapGridSize = new SizeF(10, 10); // 10x10 grid
```

## Guidelines

### Show/Hide Guidelines

```csharp
// Enable alignment guidelines
diagram1.Model.ShowGuidelines = true;

// Disable guidelines
diagram1.Model.ShowGuidelines = false;
```

### Guideline Appearance

```csharp
// Configure guideline style
diagram1.Model.GuidelineStyle.LineColor = Color.Red;
diagram1.Model.GuidelineStyle.LineWidth = 1;
diagram1.Model.GuidelineStyle.DashStyle = DashStyle.Dash;
```

### Guideline Tolerance

```csharp
// Set snap distance for guidelines
diagram1.Model.GuidelineTolerance = 5; // 5 pixels
```

## Background

### Background Color

```csharp
// Set solid background color
diagram1.BackColor = Color.White;

// Light gray background
diagram1.BackColor = Color.FromArgb(240, 240, 240);
```

### Background Image

```csharp
// Set background image
diagram1.BackgroundImage = Image.FromFile("background.png");

// Configure image layout
diagram1.BackgroundImageLayout = ImageLayout.Tile;
// Or: None, Center, Stretch, Zoom
```

### Custom Background

```csharp
// Draw custom background
diagram1.Paint += (sender, e) =>
{
    Graphics g = e.Graphics;
    
    // Draw gradient background
    using (LinearGradientBrush brush = new LinearGradientBrush(
        diagram1.ClientRectangle,
        Color.White,
        Color.LightBlue,
        LinearGradientMode.Vertical))
    {
        g.FillRectangle(brush, diagram1.ClientRectangle);
    }
};
```

## Page Settings

### Page Boundaries

```csharp
// Enable page boundaries
diagram1.Model.ShowPageBounds = true;

// Set page size (Letter: 8.5 x 11 inches)
diagram1.Model.PageSettings.Width = 8.5f;
diagram1.Model.PageSettings.Height = 11.0f;
diagram1.Model.PageSettings.Units = GraphicsUnit.Inch;

// Configure page appearance
diagram1.Model.PageSettings.DrawPageBorder = true;
diagram1.Model.PageSettings.PageBorderColor = Color.Gray;
```

### Multiple Pages

```csharp
// Enable multiple pages
diagram1.Model.PageSettings.MultiplePage = true;

// Set page margins
diagram1.Model.PageSettings.Margins = new MarginsF(0.5f, 0.5f, 0.5f, 0.5f);
```

### Page Orientation

```csharp
// Portrait
diagram1.Model.PageSettings.Orientation = PageOrientation.Portrait;

// Landscape
diagram1.Model.PageSettings.Orientation = PageOrientation.Landscape;
```

## Overview Control

The Overview control provides a minimap view of the entire diagram.

### Create Overview

```csharp
// Add Overview control to form
Overview overview = new Overview();
overview.Dock = DockStyle.Right;
overview.Width = 200;
this.Controls.Add(overview);

// Link to diagram
overview.Diagram = diagram1;
```

### Overview Appearance

```csharp
// Configure overview style
overview.BackColor = Color.WhiteSmoke;
overview.BorderStyle = BorderStyle.FixedSingle;

// Configure viewport style (visible area indicator)
overview.ViewportStyle.FillColor = Color.FromArgb(100, Color.Blue);
overview.ViewportStyle.BorderColor = Color.Blue;
overview.ViewportStyle.BorderWidth = 2;
```

### Overview Interaction

```csharp
// Click to navigate
overview.MouseClick += (sender, e) =>
{
    // Clicking overview centers diagram on that location
    // Automatic behavior, no code needed
};

// Drag viewport to pan
// Automatic behavior
```

## Magnification View

### Create Magnifier

```csharp
// Create magnifier control
MagnificationView magnifier = new MagnificationView();
magnifier.Size = new Size(150, 150);
magnifier.Location = new Point(10, 10);
magnifier.Visible = false;
diagram1.Controls.Add(magnifier);

// Link to diagram
magnifier.Diagram = diagram1;
magnifier.MagnificationFactor = 2.0f; // 2x zoom
```

### Show Magnifier on Hover

```csharp
diagram1.MouseMove += (sender, e) =>
{
    if (Control.ModifierKeys == Keys.Alt)
    {
        magnifier.Visible = true;
        magnifier.Location = new Point(e.X + 10, e.Y + 10);
        magnifier.CenterPoint = diagram1.View.PixelToWorldPoint(e.Location);
    }
    else
    {
        magnifier.Visible = false;
    }
};
```

## Minimap Implementation

```csharp
public class MinimapPanel : Panel
{
    private Diagram diagram;
    private RectangleF viewportRect;
    private bool dragging = false;
    
    public MinimapPanel(Diagram diagram)
    {
        this.diagram = diagram;
        this.DoubleBuffered = true;
        this.BorderStyle = BorderStyle.FixedSingle;
        
        diagram.Model.NodeAdded += (s, e) => Invalidate();
        diagram.Model.NodeRemoved += (s, e) => Invalidate();
        diagram.View.ScrollChanged += (s, e) => Invalidate();
        
        MouseDown += OnMouseDown;
        MouseMove += OnMouseMove;
        MouseUp += OnMouseUp;
    }
    
    protected override void OnPaint(PaintEventArgs e)
    {
        base.OnPaint(e);
        
        Graphics g = e.Graphics;
        g.SmoothingMode = SmoothingMode.AntiAlias;
        
        // Get diagram bounds
        RectangleF diagramBounds = diagram.Model.BoundingRectangle;
        
        if (diagramBounds.Width == 0 || diagramBounds.Height == 0)
            return;
        
        // Calculate scale
        float scaleX = (float)Width / diagramBounds.Width;
        float scaleY = (float)Height / diagramBounds.Height;
        float scale = Math.Min(scaleX, scaleY) * 0.9f;
        
        g.ScaleTransform(scale, scale);
        g.TranslateTransform(-diagramBounds.X, -diagramBounds.Y);
        
        // Draw nodes
        foreach (Node node in diagram.Model.Nodes)
        {
            g.FillRectangle(Brushes.LightBlue, node.BoundingRectangle);
            g.DrawRectangle(Pens.Gray, Rectangle.Round(node.BoundingRectangle));
        }
        
        // Draw viewport
        RectangleF viewport = GetViewportRect();
        g.DrawRectangle(new Pen(Color.Red, 2 / scale), viewport.X, viewport.Y, viewport.Width, viewport.Height);
    }
    
    private RectangleF GetViewportRect()
    {
        PointF topLeft = diagram.View.PixelToWorldPoint(Point.Empty);
        PointF bottomRight = diagram.View.PixelToWorldPoint(new Point(diagram.Width, diagram.Height));
        
        return new RectangleF(
            topLeft.X,
            topLeft.Y,
            bottomRight.X - topLeft.X,
            bottomRight.Y - topLeft.Y
        );
    }
    
    private void OnMouseDown(object sender, MouseEventArgs e)
    {
        if (e.Button == MouseButtons.Left)
        {
            dragging = true;
            PanToPoint(e.Location);
        }
    }
    
    private void OnMouseMove(object sender, MouseEventArgs e)
    {
        if (dragging)
        {
            PanToPoint(e.Location);
        }
    }
    
    private void OnMouseUp(object sender, MouseEventArgs e)
    {
        dragging = false;
    }
    
    private void PanToPoint(Point minimapPoint)
    {
        RectangleF diagramBounds = diagram.Model.BoundingRectangle;
        float scaleX = (float)Width / diagramBounds.Width;
        float scaleY = (float)Height / diagramBounds.Height;
        float scale = Math.Min(scaleX, scaleY) * 0.9f;
        
        PointF worldPoint = new PointF(
            minimapPoint.X / scale + diagramBounds.X,
            minimapPoint.Y / scale + diagramBounds.Y
        );
        
        Point screenPoint = Point.Round(diagram.View.WorldToPixelPoint(worldPoint));
        diagram.View.ScrollTo(new Point(
            screenPoint.X - diagram.Width / 2,
            screenPoint.Y - diagram.Height / 2
        ));
    }
}

// Usage
MinimapPanel minimap = new MinimapPanel(diagram1);
minimap.Size = new Size(200, 150);
minimap.Dock = DockStyle.Right;
this.Controls.Add(minimap);
```

## View Toolbar Example

```csharp
public class DiagramViewToolbar : ToolStrip
{
    private Diagram diagram;
    private ToolStripComboBox zoomCombo;
    
    public DiagramViewToolbar(Diagram diagram)
    {
        this.diagram = diagram;
        InitializeControls();
    }
    
    private void InitializeControls()
    {
        // Zoom In
        ToolStripButton zoomInBtn = new ToolStripButton("Zoom In");
        zoomInBtn.Click += (s, e) => ZoomIn();
        Items.Add(zoomInBtn);
        
        // Zoom Out
        ToolStripButton zoomOutBtn = new ToolStripButton("Zoom Out");
        zoomOutBtn.Click += (s, e) => ZoomOut();
        Items.Add(zoomOutBtn);
        
        // Zoom combo
        zoomCombo = new ToolStripComboBox();
        zoomCombo.Items.AddRange(new object[] { "25%", "50%", "75%", "100%", "150%", "200%" });
        zoomCombo.Text = "100%";
        zoomCombo.SelectedIndexChanged += (s, e) => ApplyZoom();
        Items.Add(zoomCombo);
        
        // Zoom to Fit
        ToolStripButton fitBtn = new ToolStripButton("Fit");
        fitBtn.Click += (s, e) => ZoomToFit();
        Items.Add(fitBtn);
        
        Items.Add(new ToolStripSeparator());
        
        // Grid toggle
        ToolStripButton gridBtn = new ToolStripButton("Grid");
        gridBtn.CheckOnClick = true;
        gridBtn.Checked = true;
        gridBtn.CheckedChanged += (s, e) => diagram.Model.RenderStyle.ShowGrid = gridBtn.Checked;
        Items.Add(gridBtn);
        
        // Rulers toggle
        ToolStripButton rulersBtn = new ToolStripButton("Rulers");
        rulersBtn.CheckOnClick = true;
        rulersBtn.CheckedChanged += (s, e) => {
            diagram.View.ShowHorizontalRuler = rulersBtn.Checked;
            diagram.View.ShowVerticalRuler = rulersBtn.Checked;
        };
        Items.Add(rulersBtn);
    }
    
    private void ZoomIn()
    {
        diagram.View.ZoomFactor = Math.Min(diagram.View.ZoomFactor * 1.1f, 5.0f);
        UpdateZoomCombo();
    }
    
    private void ZoomOut()
    {
        diagram.View.ZoomFactor = Math.Max(diagram.View.ZoomFactor * 0.9f, 0.1f);
        UpdateZoomCombo();
    }
    
    private void ApplyZoom()
    {
        string text = zoomCombo.Text.Replace("%", "");
        if (float.TryParse(text, out float percent))
        {
            diagram.View.ZoomFactor = percent / 100f;
        }
    }
    
    private void ZoomToFit()
    {
        RectangleF bounds = diagram.Model.BoundingRectangle;
        float zoomX = diagram.ClientSize.Width / bounds.Width;
        float zoomY = diagram.ClientSize.Height / bounds.Height;
        diagram.View.ZoomFactor = Math.Min(zoomX, zoomY) * 0.9f;
        diagram.View.ScrollVirtualBounds(bounds);
        UpdateZoomCombo();
    }
    
    private void UpdateZoomCombo()
    {
        zoomCombo.Text = $"{(int)(diagram.View.ZoomFactor * 100)}%";
    }
}

// Usage
DiagramViewToolbar viewToolbar = new DiagramViewToolbar(diagram1);
this.Controls.Add(viewToolbar);
```

## Next Steps

- Apply automatic layouts in [layout-management.md](layout-management.md)
- Explore advanced features in [advanced-features.md](advanced-features.md)
- Review common issues in [troubleshooting.md](troubleshooting.md)
