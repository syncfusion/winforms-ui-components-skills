# Padding, Spacing, and Scrolling Configuration

## XPTaskBar Padding

Control interior spacing of the entire XPTaskBar control.

### DockPadding

Set padding for all sides at once using DockPadding:

```csharp
// Set uniform padding on all sides
xpTaskBar1.DockPadding.All = 10;  // 10 pixels all around
```

**VB.NET:**

```vb
xpTaskBar1.DockPadding.All = 10
```

### Individual Side Padding

Set padding for specific sides:

```csharp
// Set individual sides
xpTaskBar1.DockPadding.Top = 5;
xpTaskBar1.DockPadding.Bottom = 5;
xpTaskBar1.DockPadding.Left = 10;
xpTaskBar1.DockPadding.Right = 10;
```

**VB.NET:**

```vb
xpTaskBar1.DockPadding.Top = 5
xpTaskBar1.DockPadding.Bottom = 5
xpTaskBar1.DockPadding.Left = 10
xpTaskBar1.DockPadding.Right = 10
```

### Horizontal and Vertical Padding

Additional fine-grained spacing control:

```csharp
// Horizontal spacing between elements
xpTaskBar1.HorizontalPadding = 5;

// Vertical spacing between elements
xpTaskBar1.VerticalPadding = 5;
```

**VB.NET:**

```vb
xpTaskBar1.HorizontalPadding = 5
xpTaskBar1.VerticalPadding = 5
```

### Padding Example

```csharp
// Create well-spaced layout
var taskBar = new XPTaskBar();
taskBar.Dock = DockStyle.Left;
taskBar.Width = 200;

// Set outer padding (around entire control)
taskBar.DockPadding.All = 8;

// Set inner spacing between boxes
taskBar.HorizontalPadding = 3;
taskBar.VerticalPadding = 5;
```

## XPTaskBarBox Header Padding

Control spacing within the box header itself.

### PADX and PADY

Set horizontal and vertical padding for the header text area:

```csharp
XPTaskBarBox box = new XPTaskBarBox();
box.Text = "File Operations";

// Horizontal padding in header
box.PADX = 10;

// Vertical padding in header
box.PADY = 5;
```

**VB.NET:**

```vb
Dim box As New XPTaskBarBox()
box.Text = "File Operations"
box.PADX = 10
box.PADY = 5
```

### How Header Padding Works

Header padding controls space between the header text and the header borders:

```
┌─────────────────────┐
│  [10px] Text [10px] │  <- PADX controls horizontal spacing
│ [5px]           [5px] │  <- PADY controls vertical spacing
└─────────────────────┘
```

## Auto-Scroll Configuration

Enable automatic scrollbars when content exceeds available space.

### AutoScroll Property

```csharp
// Enable auto-scrolling (default)
xpTaskBar1.AutoScroll = true;

// Disable auto-scrolling
xpTaskBar1.AutoScroll = false;
```

**VB.NET:**

```vb
xpTaskBar1.AutoScroll = True
xpTaskBar1.AutoScroll = False
```

### AutoScrollMargin

Set the margin added around the scrollable area:

```csharp
// Add 5-pixel margin on all sides of scrollable content
xpTaskBar1.AutoScrollMargin = new System.Drawing.Size(5, 5);
```

**VB.NET:**

```vb
xpTaskBar1.AutoScrollMargin = New System.Drawing.Size(5, 5)
```

Higher values create more whitespace at the edges when scrolling.

### AutoScrollMinSize

Set the minimum size before scrollbars appear:

```csharp
// Show scrollbars when content exceeds 200x300 pixels
xpTaskBar1.AutoScrollMinSize = new System.Drawing.Size(200, 300);
```

**VB.NET:**

```vb
xpTaskBar1.AutoScrollMinSize = New System.Drawing.Size(200, 300)
```

This prevents scrollbars from appearing for small content areas.

## Scroll Behavior in Different Layouts

### Vertical Layout Scrolling

In vertical layout mode, boxes stack vertically. Scrollbar appears when total height exceeds available space:

```csharp
xpTaskBar1.VerticalLayout = true;
xpTaskBar1.AutoScroll = true;

// Vertical scrollbar will appear if boxes exceed control height
```

### Horizontal Layout Scrolling

In horizontal layout mode, boxes flow into columns. Horizontal scrollbar appears based on `ColWidthOnHorizontalAlignment`:

```csharp
xpTaskBar1.VerticalLayout = false;
xpTaskBar1.ColWidthOnHorizontalAlignment = 150;
xpTaskBar1.AutoScroll = true;

// Horizontal scrollbar will appear if columns exceed control width
```

## Complete Spacing Configuration Example

```csharp
private void ConfigureSpacingForSidebar() {
    // Create taskbar as left sidebar
    var taskBar = new XPTaskBar();
    taskBar.Dock = DockStyle.Left;
    taskBar.Width = 200;
    taskBar.VerticalLayout = true;
    
    // Outer padding around entire control
    taskBar.DockPadding.All = 10;
    
    // Inner spacing between boxes
    taskBar.HorizontalPadding = 0;
    taskBar.VerticalPadding = 5;
    
    // Scroll settings
    taskBar.AutoScroll = true;
    taskBar.AutoScrollMargin = new System.Drawing.Size(3, 3);
    
    // Create boxes with consistent header padding
    foreach (var title in new[] { "File", "Edit", "View", "Tools" }) {
        var box = new XPTaskBarBox();
        box.Text = title;
        
        // Header padding
        box.PADX = 8;
        box.PADY = 4;
        
        taskBar.Controls.Add(box);
    }
    
    this.Controls.Add(taskBar);
}
```

## Responsive Spacing

Adjust spacing based on form size and DPI:

```csharp
private void AdjustSpacingBasedOnFormSize() {
    if (this.Width > 1200) {
        // Large screen - more padding
        xpTaskBar1.DockPadding.All = 15;
        xpTaskBar1.HorizontalPadding = 8;
        xpTaskBar1.VerticalPadding = 8;
    } else if (this.Width > 800) {
        // Medium screen - moderate padding
        xpTaskBar1.DockPadding.All = 10;
        xpTaskBar1.HorizontalPadding = 5;
        xpTaskBar1.VerticalPadding = 5;
    } else {
        // Small screen - minimal padding
        xpTaskBar1.DockPadding.All = 5;
        xpTaskBar1.HorizontalPadding = 2;
        xpTaskBar1.VerticalPadding = 2;
    }
}

// Call on form resize
protected override void OnResize(EventArgs e) {
    base.OnResize(e);
    AdjustSpacingBasedOnFormSize();
}
```

## Scroll Behavior Control

### Hide Scrollbar While Keeping Scroll Functionality

```csharp
// Disable visual scrollbar but keep scroll functionality
xpTaskBar1.AutoScroll = false;

// Or control individual scrollbars
xpTaskBar1.HorizontalScroll.Visible = false;
xpTaskBar1.VerticalScroll.Visible = false;
```

### Programmatic Scrolling

```csharp
// Scroll to top
xpTaskBar1.AutoScrollPosition = new System.Drawing.Point(0, 0);

// Scroll to bottom (if multiple boxes)
int maxHeight = xpTaskBar1.Controls.OfType<Control>()
    .Sum(c => c.Height);
xpTaskBar1.AutoScrollPosition = new System.Drawing.Point(0, maxHeight);
```

**VB.NET:**

```vb
xpTaskBar1.AutoScrollPosition = New System.Drawing.Point(0, 0)
```

## Common Spacing Scenarios

### Scenario 1: Compact Toolbar

Minimal spacing for dense UI:

```csharp
taskBar.DockPadding.All = 2;
taskBar.HorizontalPadding = 1;
taskBar.VerticalPadding = 1;

foreach (Control control in taskBar.Controls.OfType<XPTaskBarBox>()) {
    control.PADX = 4;
    control.PADY = 2;
}
```

### Scenario 2: Spacious Layout

Maximum spacing for airy UI:

```csharp
taskBar.DockPadding.All = 20;
taskBar.HorizontalPadding = 10;
taskBar.VerticalPadding = 10;

foreach (Control control in taskBar.Controls.OfType<XPTaskBarBox>()) {
    control.PADX = 15;
    control.PADY = 8;
}
```

### Scenario 3: Scrollable Dashboard

Configure for many boxes with scrolling:

```csharp
var dashboard = new XPTaskBar();
dashboard.Dock = DockStyle.Fill;
dashboard.VerticalLayout = true;
dashboard.AutoScroll = true;

// Minimal side padding to maximize content width
dashboard.DockPadding.Left = 5;
dashboard.DockPadding.Right = 5;

// Moderate spacing between boxes
dashboard.VerticalPadding = 8;

// Scroll area settings
dashboard.AutoScrollMargin = new System.Drawing.Size(5, 5);
dashboard.AutoScrollMinSize = new System.Drawing.Size(300, 400);
```

## Performance Note

Excessive padding calculations can impact performance with many boxes. For large taskbars (>20 boxes):

```csharp
// Disable refresh temporarily while adding many boxes
xpTaskBar1.SuspendLayout();

foreach (var title in GetManyBoxTitles()) {
    var box = new XPTaskBarBox { Text = title };
    // Configure box...
    xpTaskBar1.Controls.Add(box);
}

xpTaskBar1.ResumeLayout(true);
```

## Next Steps

- See [layout-orientation.md](layout-orientation.md) for scrolling in different layouts
- See [box-structure.md](box-structure.md) for header padding details
- See [behavior-and-events.md](behavior-and-events.md) for animation configuration
