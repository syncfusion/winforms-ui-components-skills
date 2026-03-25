# XPTaskBar Box Structure and Header Customization

## Overview

XPTaskBarBox is the container for items within an XPTaskBar. Each box has a header (containing the box name and collapse button) and a content area (displaying items). Understanding box structure enables you to organize commands logically and customize appearance effectively.

## Box Anatomy

A typical XPTaskBarBox consists of:

```
┌─ Header ─────────────────────────┐
│ [▼] File Operations              │  <- Header with collapse button
├──────────────────────────────────┤
│ • New Document                   │
│ • Open File                      │  <- Items area
│ • Save                           │
└──────────────────────────────────┘
```

### Header Components

- **Collapse Button** - Toggle icon (▼/▶) at the left edge
- **Header Text** - Box name/title
- **Header Background** - Customizable background color or brush

### Content Area

- **Items** - List of XPTaskBarItem objects
- **Child Controls** - Optional panels or other controls

## Header Customization

### Text and Alignment

Set the box name and control header text positioning:

```csharp
XPTaskBarBox box = new XPTaskBarBox();

// Basic header text
box.Text = "File Operations";

// Align header text
box.HeaderTextAlign = System.Drawing.StringAlignment.Center;  // Center
// or StringAlignment.Near (left), StringAlignment.Far (right)

// Control text clipping
box.ClipHeaderText = true;  // Clip if text is too long
```

**VB.NET:**

```vb
Dim box As New XPTaskBarBox()
box.Text = "File Operations"
box.HeaderTextAlign = System.Drawing.StringAlignment.Center
box.ClipHeaderText = True
```

### Colors and Fonts

Customize the visual appearance of the header:

```csharp
// Set colors
box.HeaderBackColor = System.Drawing.Color.LightBlue;
box.HeaderForeColor = System.Drawing.Color.DarkBlue;

// Set font
box.HeaderFont = new System.Drawing.Font(
    "Arial", 
    10F, 
    System.Drawing.FontStyle.Bold, 
    System.Drawing.GraphicsUnit.Point
);
```

**VB.NET:**

```vb
box.HeaderBackColor = System.Drawing.Color.LightBlue
box.HeaderForeColor = System.Drawing.Color.DarkBlue
box.HeaderFont = New System.Drawing.Font("Arial", 10F, System.Drawing.FontStyle.Bold, System.Drawing.GraphicsUnit.Point)
```

### Header Direction

Control text flow direction for RTL (right-to-left) languages:

```csharp
// Left-to-right (default)
box.HeaderDirection = XPTaskBarBox.HeaderDirectionFormat.LeftToRight;

// Right-to-left
box.HeaderDirection = XPTaskBarBox.HeaderDirectionFormat.RightToLeft;
```

## Collapse Button Configuration

### Controlling Collapse Button

Configure the collapse/expand button behavior:

```csharp
// Show or hide the collapse button
box.ShowCollapseButton = true;  // Show button

// Allow clicking the button to toggle
box.ToggleByButton = true;      // Enable toggle

// Programmatically set collapsed state
box.Collapsed = false;  // false = expanded, true = collapsed
```

**VB.NET:**

```vb
box.ShowCollapseButton = True
box.ToggleByButton = True
box.Collapsed = False
```

### Toggling State Programmatically

```csharp
// Collapse a box
box.Collapsed = true;

// Expand a box
box.Collapsed = false;

// Check current state
if (box.Collapsed) {
    // Box is collapsed
}
```

## Managing Multiple Boxes

### Creating Related Boxes

Create multiple boxes for different task categories:

```csharp
XPTaskBar taskBar = new XPTaskBar();

var categories = new[] { "File", "Edit", "View", "Help" };
var boxes = new List<XPTaskBarBox>();

foreach (var categoryName in categories) {
    var box = new XPTaskBarBox();
    box.Text = categoryName;
    box.ShowCollapseButton = true;
    taskBar.Controls.Add(box);
    boxes.Add(box);
}
```

### Iterating Through Boxes

```csharp
// Access all boxes
foreach (Control control in xpTaskBar1.Controls) {
    if (control is XPTaskBarBox box) {
        Console.WriteLine($"Box: {box.Text}, Collapsed: {box.Collapsed}");
    }
}

// Find a specific box
var fileBox = xpTaskBar1.Controls.OfType<XPTaskBarBox>()
    .FirstOrDefault(b => b.Text == "File Operations");
```

## Header Padding

Control spacing between header text and borders:

```csharp
// Horizontal padding
box.PADX = 10;

// Vertical padding
box.PADY = 5;
```

**VB.NET:**

```vb
box.PADX = 10
box.PADY = 5
```

Higher values create more space around the text within the header area.

## Integrating Child Controls

Host nested controls within a box using a Panel:

```csharp
// Create a panel for child controls
Panel childPanel = new Panel();
childPanel.Dock = DockStyle.Fill;

// Add controls to the panel
Button searchButton = new Button { Text = "Search" };
TextBox searchBox = new TextBox { Dock = DockStyle.Top };
childPanel.Controls.Add(searchBox);
childPanel.Controls.Add(searchButton);

// Set preferred height for child controls
box.PreferredChildPanelHeight = 80;

// Add panel to the box
box.Controls.Add(childPanel);
```

## Methods for State Management

XPTaskBarBox provides methods to save and load expanded states:

```csharp
// Load previously saved expanded/collapsed states from AppStateSerializer
box.LoadBoxExpandedStates();

// Save current expanded/collapsed states to AppStateSerializer
box.SaveBoxExpandedStates();
```

These methods work with Syncfusion's AppStateSerializer for persistence across sessions.

## Common Scenarios

### Scenario 1: Create Consistent Headers

```csharp
private XPTaskBarBox CreateStandardBox(string title) {
    var box = new XPTaskBarBox();
    box.Text = title;
    box.HeaderFont = new Font("Segoe UI", 9F, FontStyle.Bold);
    box.HeaderBackColor = Color.FromArgb(230, 235, 245);
    box.HeaderForeColor = Color.FromArgb(40, 60, 90);
    box.ShowCollapseButton = true;
    box.ToggleByButton = true;
    return box;
}
```

### Scenario 2: Collapse All Boxes Except One

```csharp
void CollapseAllExcept(XPTaskBarBox activeBox) {
    foreach (Control control in xpTaskBar1.Controls) {
        if (control is XPTaskBarBox box && box != activeBox) {
            box.Collapsed = true;
        }
    }
    activeBox.Collapsed = false;
}
```

### Scenario 3: Highlight Active Box Header

```csharp
void SetActiveBox(XPTaskBarBox box) {
    // Highlight the active box
    box.HeaderBackColor = Color.LightBlue;
    box.HeaderForeColor = Color.DarkBlue;
    
    // Reset others
    foreach (Control control in xpTaskBar1.Controls) {
        if (control is XPTaskBarBox other && other != box) {
            other.HeaderBackColor = SystemColors.Control;
            other.HeaderForeColor = SystemColors.ControlText;
        }
    }
}
```

## Next Steps

- See [items-and-content.md](items-and-content.md) to manage items within boxes
- See [appearance-customization.md](appearance-customization.md) for advanced brush and image customization
- See [behavior-and-events.md](behavior-and-events.md) to handle collapse/expand events
