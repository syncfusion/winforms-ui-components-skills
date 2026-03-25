# Panel Configuration in SplitContainerAdv

## Table of Contents
- [Panel Orientation](#panel-orientation)
- [Fixed and Resizable Panels](#fixed-and-resizable-panels)
- [Minimum Panel Sizes](#minimum-panel-sizes)
- [Collapsing and Expanding Panels](#collapsing-and-expanding-panels)
- [Panel Properties](#panel-properties)

## Panel Orientation

Panel orientation determines how the SplitContainerAdv divides its space. You can choose between horizontal and vertical layouts.

### Horizontal Orientation

Creates a vertical splitter dividing left and right panels:

```csharp
this.splitContainerAdv1.Orientation = System.Windows.Forms.Orientation.Horizontal;
```

```vb
Me.splitContainerAdv1.Orientation = System.Windows.Forms.Orientation.Horizontal
```

**Layout:** Panel1 (left) | Splitter | Panel2 (right)

### Vertical Orientation

Creates a horizontal splitter dividing top and bottom panels:

```csharp
this.splitContainerAdv1.Orientation = System.Windows.Forms.Orientation.Vertical;
```

```vb
Me.splitContainerAdv1.Orientation = System.Windows.Forms.Orientation.Vertical
```

**Layout:** Panel1 (top) / Splitter / Panel2 (bottom)

## Fixed and Resizable Panels

Control which panels are fixed during container resizing using the `FixedPanel` property.

### No Fixed Panels (Default)

Both panels resize proportionally when the container is resized:

```csharp
this.splitContainerAdv1.FixedPanel = Syncfusion.Windows.Forms.Tools.Enums.FixedPanel.None;
```

```vb
Me.splitContainerAdv1.FixedPanel = Syncfusion.Windows.Forms.Tools.Enums.FixedPanel.None
```

### Fix Panel1

Panel1 maintains its size while Panel2 resizes:

```csharp
this.splitContainerAdv1.FixedPanel = Syncfusion.Windows.Forms.Tools.Enums.FixedPanel.Panel1;
```

```vb
Me.splitContainerAdv1.FixedPanel = Syncfusion.Windows.Forms.Tools.Enums.FixedPanel.Panel1
```

**Use Case:** Toolbar or menu panel on the left/top that should maintain fixed width/height

### Fix Panel2

Panel2 maintains its size while Panel1 resizes:

```csharp
this.splitContainerAdv1.FixedPanel = Syncfusion.Windows.Forms.Tools.Enums.FixedPanel.Panel2;
```

```vb
Me.splitContainerAdv1.FixedPanel = Syncfusion.Windows.Forms.Tools.Enums.FixedPanel.Panel2
```

**Use Case:** Status panel or details panel that should maintain fixed dimensions

## Minimum Panel Sizes

Set minimum size constraints for each panel to prevent them from becoming too small during resizing.

### Panel1 Minimum Size

```csharp
this.splitContainerAdv1.Panel1MinSize = 50;
```

```vb
Me.splitContainerAdv1.Panel1MinSize = 50
```

Default value is 25 pixels. Set a larger value to ensure Panel1 has adequate space.

### Panel2 Minimum Size

```csharp
this.splitContainerAdv1.Panel2MinSize = 100;
```

```vb
Me.splitContainerAdv1.Panel2MinSize = 100
```

Default value is 25 pixels.

### Complete Example

```csharp
// Horizontal orientation with fixed left panel and minimum sizes
this.splitContainerAdv1.Orientation = Orientation.Horizontal;
this.splitContainerAdv1.FixedPanel = FixedPanel.Panel1;
this.splitContainerAdv1.Panel1MinSize = 150;  // Sidebar width
this.splitContainerAdv1.Panel2MinSize = 200;  // Content area minimum
this.splitContainerAdv1.SplitterDistance = 200; // Initial sidebar width
```

```vb
' Horizontal orientation with fixed left panel and minimum sizes
Me.splitContainerAdv1.Orientation = Orientation.Horizontal
Me.splitContainerAdv1.FixedPanel = FixedPanel.Panel1
Me.splitContainerAdv1.Panel1MinSize = 150   ' Sidebar width
Me.splitContainerAdv1.Panel2MinSize = 200   ' Content area minimum
Me.splitContainerAdv1.SplitterDistance = 200 ' Initial sidebar width
```

## Collapsing and Expanding Panels

Collapse panels at runtime to hide them or expand them to show content.

### Collapse Panel1

```csharp
this.splitContainerAdv1.Panel1Collapsed = true;
```

```vb
Me.splitContainerAdv1.Panel1Collapsed = True
```

### Collapse Panel2

```csharp
this.splitContainerAdv1.Panel2Collapsed = true;
```

```vb
Me.splitContainerAdv1.Panel2Collapsed = True
```

### Set Which Panel Collapses

Specify which panel should collapse when toggled:

```csharp
this.splitContainerAdv1.PanelToBeCollapsed = Syncfusion.Windows.Forms.Tools.CollapsedPanel.Panel1;
```

```vb
Me.splitContainerAdv1.PanelToBeCollapsed = Syncfusion.Windows.Forms.Tools.CollapsedPanel.Panel1
```

### Toggle Collapse Trigger

Set how panels are toggled between collapsed and expanded states:

```csharp
// Collapse/Expand on double-click
this.splitContainerAdv1.TogglePanelOn = Syncfusion.Windows.Forms.Tools.TogglePanelOn.DoubleClick;
```

```vb
' Collapse/Expand on double-click
Me.splitContainerAdv1.TogglePanelOn = Syncfusion.Windows.Forms.Tools.TogglePanelOn.DoubleClick
```

### Complete Collapsible Configuration

```csharp
// Enable collapsible behavior
this.splitContainerAdv1.Panel1Collapsed = false;
this.splitContainerAdv1.PanelToBeCollapsed = CollapsedPanel.Panel1;
this.splitContainerAdv1.TogglePanelOn = TogglePanelOn.DoubleClick;
```

```vb
' Enable collapsible behavior
Me.splitContainerAdv1.Panel1Collapsed = False
Me.splitContainerAdv1.PanelToBeCollapsed = CollapsedPanel.Panel1
Me.splitContainerAdv1.TogglePanelOn = TogglePanelOn.DoubleClick
```

## Panel Properties

Each panel (Panel1 and Panel2) has the following properties available:

### Common Panel Properties

| Property | Type | Description |
|----------|------|-------------|
| `BackColor` | Color | Background color of the panel |
| `BackgroundColor` | BrushInfo | Gradient or solid background |
| `ForeColor` | Color | Text/foreground color |
| `Font` | Font | Font for text in the panel |
| `Enabled` | bool | Whether the panel accepts input |
| `Visible` | bool | Whether the panel is visible |

### Accessing Panel Properties

```csharp
// Set Panel1 properties
this.splitContainerAdv1.Panel1.BackColor = System.Drawing.Color.LightBlue;
this.splitContainerAdv1.Panel1.Font = new System.Drawing.Font("Arial", 10);

// Set Panel2 properties
this.splitContainerAdv1.Panel2.BackColor = System.Drawing.Color.White;
this.splitContainerAdv1.Panel2.ForeColor = System.Drawing.Color.Black;
```

```vb
' Set Panel1 properties
Me.splitContainerAdv1.Panel1.BackColor = System.Drawing.Color.LightBlue
Me.splitContainerAdv1.Panel1.Font = New System.Drawing.Font("Arial", 10)

' Set Panel2 properties
Me.splitContainerAdv1.Panel2.BackColor = System.Drawing.Color.White
Me.splitContainerAdv1.Panel2.ForeColor = System.Drawing.Color.Black
```

## Accessing Panels

Reference Panel1 and Panel2 to add controls or modify properties:

```csharp
// Add control to Panel1
this.splitContainerAdv1.Panel1.Controls.Add(myControl);

// Get Panel1 height
int panel1Height = this.splitContainerAdv1.Panel1.Height;

// Clear Panel2 controls
this.splitContainerAdv1.Panel2.Controls.Clear();
```

```vb
' Add control to Panel1
Me.splitContainerAdv1.Panel1.Controls.Add(myControl)

' Get Panel1 height
Dim panel1Height As Integer = Me.splitContainerAdv1.Panel1.Height

' Clear Panel2 controls
Me.splitContainerAdv1.Panel2.Controls.Clear()
```
