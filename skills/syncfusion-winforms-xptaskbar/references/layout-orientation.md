# Layout Orientation and Responsiveness

## Vertical vs Horizontal Layout

XPTaskBar supports two primary layout orientations: vertical (default) and horizontal. Choosing the right orientation depends on your UI design and available screen space.

### Vertical Layout (Default)

In vertical layout, boxes are stacked on top of each other:

```
┌─────────────────────┐
│ File Operations    │ ← Box 1
├─────────────────────┤
│ • New              │
│ • Open             │
└─────────────────────┘
┌─────────────────────┐
│ Edit Tools         │ ← Box 2
├─────────────────────┤
│ • Cut              │
│ • Copy             │
│ • Paste            │
└─────────────────────┘
```

Enable vertical layout (default):

```csharp
xpTaskBar1.VerticalLayout = true;  // or omit since this is default
```

**VB.NET:**

```vb
xpTaskBar1.VerticalLayout = True
```

### Horizontal Layout

In horizontal layout, boxes are arranged side-by-side in columns:

```
┌──────────────┬──────────────┬──────────────┐
│ File Ops.    │ Edit Tools   │ View Options │
├──────────────┼──────────────┼──────────────┤
│ • New        │ • Cut        │ • Zoom In    │
│ • Open       │ • Copy       │ • Zoom Out   │
│ • Save       │ • Paste      │ • Fit Page   │
└──────────────┴──────────────┴──────────────┘
```

Enable horizontal layout:

```csharp
xpTaskBar1.VerticalLayout = false;
```

**VB.NET:**

```vb
xpTaskBar1.VerticalLayout = False
```

## Horizontal Layout Column Width

When in horizontal layout, control the width of each column:

```csharp
// Set column width for horizontal layout
xpTaskBar1.ColWidthOnHorizontalAlignment = 150;  // 150 pixels wide
```

**VB.NET:**

```vb
xpTaskBar1.ColWidthOnHorizontalAlignment = 150
```

### How Column Width Works

- **Columns flow automatically** - Boxes are distributed across columns based on available width
- **Larger values** create wider columns and more vertical scrolling if needed
- **Smaller values** create narrower columns with more side-by-side boxes
- **Typical range** - 100-250 pixels depending on content and screen size

### Example: Dynamic Column Width

```csharp
// Responsive column width based on form size
private void Form_Resize(object sender, EventArgs e) {
    int availableWidth = this.Width - 20;  // Subtract margins
    int boxCount = xpTaskBar1.Controls.Count;
    
    if (boxCount > 0) {
        int columnWidth = Math.Max(100, availableWidth / 2);  // 2 columns
        xpTaskBar1.ColWidthOnHorizontalAlignment = columnWidth;
    }
}
```

## Switching Orientations Dynamically

Change layout orientation at runtime:

```csharp
// Toggle between vertical and horizontal
public void ToggleOrientation() {
    xpTaskBar1.VerticalLayout = !xpTaskBar1.VerticalLayout;
}

// Or use a button click
private void OrientationButton_Click(object sender, EventArgs e) {
    if (xpTaskBar1.VerticalLayout) {
        xpTaskBar1.VerticalLayout = false;
        xpTaskBar1.ColWidthOnHorizontalAlignment = 120;
        OrientationButton.Text = "Switch to Vertical";
    } else {
        xpTaskBar1.VerticalLayout = true;
        OrientationButton.Text = "Switch to Horizontal";
    }
}
```

**VB.NET:**

```vb
Public Sub ToggleOrientation()
    xpTaskBar1.VerticalLayout = Not xpTaskBar1.VerticalLayout
End Sub

Private Sub OrientationButton_Click(sender As Object, e As EventArgs)
    If xpTaskBar1.VerticalLayout Then
        xpTaskBar1.VerticalLayout = False
        xpTaskBar1.ColWidthOnHorizontalAlignment = 120
        OrientationButton.Text = "Switch to Vertical"
    Else
        xpTaskBar1.VerticalLayout = True
        OrientationButton.Text = "Switch to Horizontal"
    End If
End Sub
```

## Scroll Behavior

### Automatic Scrollbars

XPTaskBar automatically adds scrollbars when content exceeds available space:

**Vertical Layout:**
- Vertical scrollbar appears when boxes are taller than the control
- Enabled by `AutoScroll` property

**Horizontal Layout:**
- Horizontal scrollbar appears when columns exceed the width set by `ColWidthOnHorizontalAlignment`
- Also controlled by `AutoScroll` property

### Controlling Auto-Scroll

```csharp
// Enable auto-scrolling (default)
xpTaskBar1.AutoScroll = true;

// Disable auto-scrolling
xpTaskBar1.AutoScroll = false;

// Set scroll margins (space added to calculated scroll area)
xpTaskBar1.AutoScrollMargin = new System.Drawing.Size(5, 5);  // 5px margin

// Set minimum size before scrolling starts
xpTaskBar1.AutoScrollMinSize = new System.Drawing.Size(200, 300);
```

**VB.NET:**

```vb
xpTaskBar1.AutoScroll = True
xpTaskBar1.AutoScrollMargin = New System.Drawing.Size(5, 5)
xpTaskBar1.AutoScrollMinSize = New System.Drawing.Size(200, 300)
```

### Scroll Properties Explained

| Property | Purpose |
|----------|---------|
| `AutoScroll` | Enable/disable automatic scrollbar display |
| `AutoScrollMargin` | Extra space added around scrollable area |
| `AutoScrollMinSize` | Size at which scrollbars appear |

## Layout Strategy Selection

### Choose Vertical Layout When

- **Sidebar navigation** - Control docked to left or right side
- **Limited horizontal space** - Narrow form or panel
- **Many boxes** - Stack items vertically to accommodate all boxes
- **Mobile-first design** - Portrait orientation on tablet/phone apps

### Choose Horizontal Layout When

- **Wide horizontal display** - Dashboard or widescreen monitor
- **Few boxes with many items** - Distribute horizontally to use space
- **Tab-like interface** - Boxes arranged like horizontal tabs
- **Landscape orientation** - Wide forms or fullscreen layouts

## Common Layout Scenarios

### Scenario 1: Sidebar Navigation (Vertical)

```csharp
XPTaskBar taskBar = new XPTaskBar();
taskBar.Dock = DockStyle.Left;
taskBar.Width = 200;
taskBar.VerticalLayout = true;
taskBar.AutoScroll = true;

// Add category boxes
var categories = new[] { "Files", "Edit", "View", "Tools", "Help" };
foreach (var cat in categories) {
    var box = new XPTaskBarBox { Text = cat };
    taskBar.Controls.Add(box);
    // Add items to box...
}

this.Controls.Add(taskBar);
```

### Scenario 2: Dashboard Grid (Horizontal)

```csharp
XPTaskBar dashboard = new XPTaskBar();
dashboard.Dock = DockStyle.Fill;
dashboard.VerticalLayout = false;
dashboard.ColWidthOnHorizontalAlignment = 250;
dashboard.AutoScroll = true;

// Add metric boxes
var metrics = new[] { "Sales", "Revenue", "Users", "Conversion" };
foreach (var metric in metrics) {
    var box = new XPTaskBarBox { Text = metric };
    dashboard.Controls.Add(box);
    // Add items or child controls...
}

this.Controls.Add(dashboard);
```

### Scenario 3: Responsive Layout

```csharp
private void SetLayoutBasedOnFormWidth() {
    if (this.Width > 800) {
        // Wide form - use horizontal with 2 columns
        xpTaskBar1.VerticalLayout = false;
        xpTaskBar1.ColWidthOnHorizontalAlignment = (this.Width - 40) / 2;
    } else {
        // Narrow form - use vertical
        xpTaskBar1.VerticalLayout = true;
    }
}

// Call on form load and resize
protected override void OnLoad(EventArgs e) {
    base.OnLoad(e);
    SetLayoutBasedOnFormWidth();
}

protected override void OnResize(EventArgs e) {
    base.OnResize(e);
    SetLayoutBasedOnFormWidth();
}
```

### Scenario 4: Collapsible Toolbar

```csharp
// Start with horizontal layout
XPTaskBar toolbar = new XPTaskBar();
toolbar.Dock = DockStyle.Top;
toolbar.Height = 100;
toolbar.VerticalLayout = false;
toolbar.ColWidthOnHorizontalAlignment = 80;

// Add tool categories
var toolGroups = new[] { "File", "Edit", "Format", "View" };
foreach (var group in toolGroups) {
    var box = new XPTaskBarBox { Text = group };
    toolbar.Controls.Add(box);
}
```

## Performance Considerations

### Large Number of Boxes

```csharp
// For many boxes (>10), consider:

// 1. Use horizontal layout to distribute across screen
xpTaskBar1.VerticalLayout = false;
xpTaskBar1.ColWidthOnHorizontalAlignment = 150;

// 2. Enable scrolling
xpTaskBar1.AutoScroll = true;

// 3. Collapse non-essential boxes
foreach (Control control in xpTaskBar1.Controls.OfType<XPTaskBarBox>().Skip(3)) {
    control.Collapsed = true;
}
```

### Dynamic Box Addition

```csharp
// Add boxes incrementally rather than all at once
public void AddBoxesInBatches(IEnumerable<string> boxNames) {
    foreach (var name in boxNames) {
        var box = new XPTaskBarBox { Text = name };
        xpTaskBar1.Controls.Add(box);
        Application.DoEvents();  // Allow UI to update
    }
}
```

## Next Steps

- See [box-structure.md](box-structure.md) for header customization
- See [behavior-and-events.md](behavior-and-events.md) for animation during expand/collapse
- See [padding-spacing-scrolling.md](padding-spacing-scrolling.md) for detailed scroll configuration
