# Layout Patterns with BorderLayout

## Table of Contents
- [Classic Header-Content-Footer](#classic-header-content-footer)
- [Sidebar Navigation](#sidebar-navigation)
- [Multi-Panel Dashboard](#multi-panel-dashboard)
- [Nested BorderLayouts](#nested-borderlayouts)
- [Responsive Considerations](#responsive-considerations)
- [Best Practices](#best-practices)
- [When to Use BorderLayout](#when-to-use-borderlayout)

## Classic Header-Content-Footer

### Overview
The most common and versatile layout pattern. Perfect for applications with:
- Top navigation or toolbar
- Main content area
- Bottom status bar or footer

### Visual Design
```
┌─────────────────────────────────────┐
│         HEADER/TOOLBAR              │ Fixed height
├─────────────────────────────────────┤
│                                     │
│                                     │
│       MAIN CONTENT AREA             │ Fills remaining space
│                                     │
│                                     │
├─────────────────────────────────────┤
│            STATUS/FOOTER            │ Fixed height
└─────────────────────────────────────┘
```

### Implementation

**C#:**
```csharp
// Create BorderLayout
BorderLayout borderLayout1 = new BorderLayout();
borderLayout1.ContainerControl = this;
borderLayout1.HGap = 10;
borderLayout1.VGap = 10;

// Header panel with toolbar
Panel headerPanel = new Panel() 
{ 
    Height = 50, 
    BackColor = Color.FromArgb(51, 102, 153) 
};

// Add toolbar buttons
ButtonAdv newBtn = new ButtonAdv() 
{ 
    Text = "New", 
    Dock = DockStyle.Left 
};
ButtonAdv openBtn = new ButtonAdv() 
{ 
    Text = "Open", 
    Dock = DockStyle.Left 
};
ButtonAdv saveBtn = new ButtonAdv() 
{ 
    Text = "Save", 
    Dock = DockStyle.Left 
};

headerPanel.Controls.Add(newBtn);
headerPanel.Controls.Add(openBtn);
headerPanel.Controls.Add(saveBtn);

// Content panel (main work area)
Panel contentPanel = new Panel() 
{ 
    BackColor = Color.White 
};

// Add content (e.g., DataGrid, RichTextBox, etc.)
DataGrid dataGrid = new DataGrid() 
{ 
    Dock = DockStyle.Fill 
};
contentPanel.Controls.Add(dataGrid);

// Footer panel with status
Panel footerPanel = new Panel() 
{ 
    Height = 30, 
    BackColor = Color.FromArgb(240, 240, 240) 
};

Label statusLabel = new Label() 
{ 
    Text = "Ready", 
    Dock = DockStyle.Left 
};
footerPanel.Controls.Add(statusLabel);

// Add panels to form
this.Controls.Add(headerPanel);
this.Controls.Add(contentPanel);
this.Controls.Add(footerPanel);

// Position panels
borderLayout1.SetPosition(headerPanel, BorderPosition.North);
borderLayout1.SetPosition(contentPanel, BorderPosition.Center);
borderLayout1.SetPosition(footerPanel, BorderPosition.South);
```

**VB.NET:**
```vb
' Create BorderLayout
Dim borderLayout1 As BorderLayout = New BorderLayout()
borderLayout1.ContainerControl = Me
borderLayout1.HGap = 10
borderLayout1.VGap = 10

' Header panel with toolbar
Dim headerPanel As Panel = New Panel() With {.Height = 50, .BackColor = Color.FromArgb(51, 102, 153)}

' Add toolbar buttons
Dim newBtn As ButtonAdv = New ButtonAdv() With {.Text = "New", .Dock = DockStyle.Left}
Dim openBtn As ButtonAdv = New ButtonAdv() With {.Text = "Open", .Dock = DockStyle.Left}
Dim saveBtn As ButtonAdv = New ButtonAdv() With {.Text = "Save", .Dock = DockStyle.Left}

headerPanel.Controls.Add(newBtn)
headerPanel.Controls.Add(openBtn)
headerPanel.Controls.Add(saveBtn)

' Content panel (main work area)
Dim contentPanel As Panel = New Panel() With {.BackColor = Color.White}

' Add content
Dim dataGrid As DataGrid = New DataGrid() With {.Dock = DockStyle.Fill}
contentPanel.Controls.Add(dataGrid)

' Footer panel with status
Dim footerPanel As Panel = New Panel() With {.Height = 30, .BackColor = Color.FromArgb(240, 240, 240)}

Dim statusLabel As Label = New Label() With {.Text = "Ready", .Dock = DockStyle.Left}
footerPanel.Controls.Add(statusLabel)

' Add panels to form
Me.Controls.Add(headerPanel)
Me.Controls.Add(contentPanel)
Me.Controls.Add(footerPanel)

' Position panels
borderLayout1.SetPosition(headerPanel, BorderPosition.North)
borderLayout1.SetPosition(contentPanel, BorderPosition.Center)
borderLayout1.SetPosition(footerPanel, BorderPosition.South)
```

### Use Cases
- Document editors
- Web browsers
- Email clients
- IDE applications
- Data management tools

## Sidebar Navigation

### Overview
Classic layout with left or right sidebar for navigation or tools, with main content area.

### Visual Design (Left Sidebar)
```
┌──────────┬────────────────────────┐
│          │                        │
│ NAV MENU │                        │
│          │  MAIN CONTENT          │
│ • Home   │  (fills remaining)     │
│ • File   │                        │
│ • Edit   │                        │
│          │                        │
└──────────┴────────────────────────┘
 Fixed      Fills remaining
 width      width
```

### Implementation (Left Sidebar)

**C#:**
```csharp
// Create BorderLayout
BorderLayout borderLayout1 = new BorderLayout();
borderLayout1.ContainerControl = this;
borderLayout1.HGap = 5;
borderLayout1.VGap = 5;

// Sidebar panel with menu
Panel sidebarPanel = new Panel() 
{ 
    Width = 200, 
    BackColor = Color.FromArgb(245, 245, 245) 
};

TreeView menuTree = new TreeView() 
{ 
    Dock = DockStyle.Fill 
};
menuTree.Nodes.Add("Home");
menuTree.Nodes.Add("File");
menuTree.Nodes.Add("Edit");
menuTree.Nodes.Add("View");
menuTree.Nodes.Add("Tools");

sidebarPanel.Controls.Add(menuTree);

// Content panel
Panel contentPanel = new Panel() 
{ 
    BackColor = Color.White 
};

RichTextBox contentBox = new RichTextBox() 
{ 
    Dock = DockStyle.Fill,
    Text = "Main content goes here..."
};
contentPanel.Controls.Add(contentBox);

// Add to form
this.Controls.Add(sidebarPanel);
this.Controls.Add(contentPanel);

// Position
borderLayout1.SetPosition(sidebarPanel, BorderPosition.West);
borderLayout1.SetPosition(contentPanel, BorderPosition.Center);
```

### Right Sidebar Pattern
```csharp
// For right sidebar, just use BorderPosition.East
borderLayout1.SetPosition(sidebarPanel, BorderPosition.East);
borderLayout1.SetPosition(contentPanel, BorderPosition.Center);
```

### Use Cases
- IDEs (file explorer on left, code in center, properties on right)
- Email clients (folder list on left, message list center)
- Document management (folder tree on left, documents in center)
- Design tools (tools sidebar on left/right)

## Multi-Panel Dashboard

### Overview
Using all five positions for a comprehensive dashboard layout.

### Visual Design
```
┌──────────────────────────────────────────────┐
│              TOOLBAR (North)                 │
├────────┬───────────────────────────┬────────┤
│ Tools  │   Main Content Area      │ Status │
│ (West) │     (Center)             │ (East) │
│        │                          │        │
├────────┴───────────────────────────┴────────┤
│          STATUS BAR (South)                 │
└──────────────────────────────────────────────┘
```

### Implementation

**C#:**
```csharp
// Create BorderLayout
BorderLayout borderLayout1 = new BorderLayout();
borderLayout1.ContainerControl = this;
borderLayout1.HGap = 8;
borderLayout1.VGap = 8;

// TOOLBAR (North) - Fixed height
Panel toolbarPanel = new Panel() { Height = 40, BackColor = Color.FromArgb(200, 200, 200) };
ButtonAdv refreshBtn = new ButtonAdv() { Text = "Refresh", Dock = DockStyle.Left };
ButtonAdv settingsBtn = new ButtonAdv() { Text = "Settings", Dock = DockStyle.Left };
toolbarPanel.Controls.Add(refreshBtn);
toolbarPanel.Controls.Add(settingsBtn);

// LEFT PANEL (West) - Fixed width
Panel leftPanel = new Panel() { Width = 150, BackColor = Color.FromArgb(240, 240, 240) };
ListBox categoryList = new ListBox() { Dock = DockStyle.Fill };
categoryList.Items.Add("Category A");
categoryList.Items.Add("Category B");
categoryList.Items.Add("Category C");
leftPanel.Controls.Add(categoryList);

// CENTER PANEL (Center) - Fills remaining space
Panel centerPanel = new Panel() { BackColor = Color.White };
DataGrid dataGrid = new DataGrid() { Dock = DockStyle.Fill };
centerPanel.Controls.Add(dataGrid);

// RIGHT PANEL (East) - Fixed width
Panel rightPanel = new Panel() { Width = 200, BackColor = Color.FromArgb(240, 240, 240) };
PropertyGrid propertyGrid = new PropertyGrid() { Dock = DockStyle.Fill };
rightPanel.Controls.Add(propertyGrid);

// FOOTER (South) - Fixed height
Panel footerPanel = new Panel() { Height = 25, BackColor = Color.FromArgb(200, 200, 200) };
Label statusLabel = new Label() { Text = "Ready", Dock = DockStyle.Left };
footerPanel.Controls.Add(statusLabel);

// Add all panels
this.Controls.Add(toolbarPanel);
this.Controls.Add(leftPanel);
this.Controls.Add(centerPanel);
this.Controls.Add(rightPanel);
this.Controls.Add(footerPanel);

// Position all
borderLayout1.SetPosition(toolbarPanel, BorderPosition.North);
borderLayout1.SetPosition(leftPanel, BorderPosition.West);
borderLayout1.SetPosition(centerPanel, BorderPosition.Center);
borderLayout1.SetPosition(rightPanel, BorderPosition.East);
borderLayout1.SetPosition(footerPanel, BorderPosition.South);
```

### Use Cases
- Visual Studio-style IDEs (toolbar top, solution explorer left, properties right, status bar bottom)
- Database tools
- Multimedia editors
- CAD applications
- Project management tools

## Nested BorderLayouts

### Overview
For complex layouts, use BorderLayout within BorderLayout to create multiple levels of organization.

### Scenario: IDE-Like Layout with Multiple Content Areas

```
┌─────────────────────────────────────────┐
│         Main Toolbar (North)            │
├────────────┬──────────────────┬─────────┤
│   Files    │   Code Editor    │  Props  │
│   (West)   │   + Nested BL    │ (East)  │
│            │   (Center)       │         │
│            │  ┌────┬──────┐   │         │
│            │  │Tabs│Code  │   │         │
│            │  ├────┼──────┤   │         │
│            │  │Prob│Output│   │         │
│            │  └────┴──────┘   │         │
├────────────┴──────────────────┴─────────┤
│         Status Bar (South)               │
└─────────────────────────────────────────┘
```

### Implementation

**C#:**
```csharp
// Main form BorderLayout
BorderLayout mainLayout = new BorderLayout();
mainLayout.ContainerControl = this;

// Create main structure
Panel toolbarPanel = new Panel() { Height = 40 };
Panel filePanel = new Panel() { Width = 200 };
Panel contentWrapperPanel = new Panel();  // Will contain nested layout
Panel propertiesPanel = new Panel() { Width = 250 };
Panel statusPanel = new Panel() { Height = 25 };

// Position main panels
mainLayout.SetPosition(toolbarPanel, BorderPosition.North);
mainLayout.SetPosition(filePanel, BorderPosition.West);
mainLayout.SetPosition(contentWrapperPanel, BorderPosition.Center);
mainLayout.SetPosition(propertiesPanel, BorderPosition.East);
mainLayout.SetPosition(statusPanel, BorderPosition.South);

// Add to form
this.Controls.Add(toolbarPanel);
this.Controls.Add(filePanel);
this.Controls.Add(contentWrapperPanel);
this.Controls.Add(propertiesPanel);
this.Controls.Add(statusPanel);

// NESTED BorderLayout inside contentWrapperPanel
BorderLayout nestedLayout = new BorderLayout();
nestedLayout.ContainerControl = contentWrapperPanel;

Panel tabsPanel = new Panel() { Height = 30 };
Panel codePanel = new Panel();
Panel outputPanel = new Panel() { Height = 100 };

nestedLayout.SetPosition(tabsPanel, BorderPosition.North);
nestedLayout.SetPosition(codePanel, BorderPosition.Center);
nestedLayout.SetPosition(outputPanel, BorderPosition.South);

contentWrapperPanel.Controls.Add(tabsPanel);
contentWrapperPanel.Controls.Add(codePanel);
contentWrapperPanel.Controls.Add(outputPanel);
```

### Use Cases
- Complex IDEs with multiple work areas
- Advanced dashboard layouts
- Multi-section document editors
- Scientific or engineering applications

## Responsive Considerations

### Handling Resizing

BorderLayout automatically adjusts the center and border positions when the container is resized:

```csharp
// BorderLayout responds to form resize automatically
this.AutoScaleMode = AutoScaleMode.Font;
this.Size = new Size(800, 600);

// When user resizes form, BorderLayout recalculates positions
// - North/South panels keep their heights
// - East/West panels keep their widths  
// - Center panel fills remaining space
```

### Minimum Size Enforcement

Set minimum form size to prevent overlapping:

```csharp
// Set minimum size to prevent controls from overlapping
this.MinimumSize = new Size(400, 300);

// Or calculate based on panel sizes
int minWidth = 150 + 200 + 100;  // Left + Center + Right
int minHeight = 50 + 100 + 40;   // Top + Center + Bottom
this.MinimumSize = new Size(minWidth, minHeight);
```

## Best Practices

### 1. Set Fixed Sizes First
Always set heights for North/South and widths for East/West before positioning:

```csharp
// Correct - Set sizes first
Panel headerPanel = new Panel() { Height = 50 };
this.Controls.Add(headerPanel);
borderLayout1.SetPosition(headerPanel, BorderPosition.North);

// Avoid changing size after positioning
// headerPanel.Height = 60;  // Don't do this
```

### 2. Use Container Controls for Multiple Items
Never position multiple controls to the same border position; use a container:

```csharp
// Wrong - Both controls try to occupy North
borderLayout1.SetPosition(button1, BorderPosition.North);
borderLayout1.SetPosition(button2, BorderPosition.North);  // Overlaps!

// Correct - Use panel container
Panel toolbarPanel = new Panel();
toolbarPanel.Controls.Add(button1);
toolbarPanel.Controls.Add(button2);
borderLayout1.SetPosition(toolbarPanel, BorderPosition.North);
```

### 3. Configure Spacing Early
Set HGap and VGap immediately after creating BorderLayout:

```csharp
BorderLayout borderLayout1 = new BorderLayout();
borderLayout1.ContainerControl = this;
borderLayout1.HGap = 10;  // Set spacing early
borderLayout1.VGap = 10;
```

### 4. Use Dock Property for Internal Layout
Inside container panels, use Dock for child controls:

```csharp
// Inside a panel, use Dock to arrange children
button1.Dock = DockStyle.Left;
button2.Dock = DockStyle.Left;
label.Dock = DockStyle.Right;
```

### 5. Keep Center Panel Empty of Size Constraints
Don't set explicit Width/Height on center panel:

```csharp
// Wrong - Constrains center panel
Panel contentPanel = new Panel() { Width = 400, Height = 300 };

// Correct - Let it fill remaining space
Panel contentPanel = new Panel();
```

## When to Use BorderLayout

### ✓ Use BorderLayout When:
- You need precise border positioning (North, South, East, West, Center)
- Your layout has fixed-size regions around a content area
- You want a simpler alternative to DockStyle
- Building applications with standard UI patterns (header, sidebar, content, footer)
- You need explicit spacing (HGap, VGap) between regions

### ✗ Don't Use BorderLayout When:
- You need flexible grid-based layouts (use TableLayoutPanel instead)
- You need flow-based layout (use FlowLayoutPanel instead)
- You need absolute positioning (use manual positioning instead)
- All controls need to be the same size
- You need complex wrapping behavior

### Comparison with Alternatives

| Feature | BorderLayout | DockStyle | TableLayoutPanel | FlowLayoutPanel |
|---------|-------------|-----------|------------------|-----------------|
| **Border regions** | ✓ | ✓ | ✗ | ✗ |
| **Explicit spacing** | ✓ | ✗ | ✓ | ✓ |
| **Fixed sizes** | ✓ | ✓ | ✓ | ✗ |
| **Simple setup** | ✓ | ~ | ~ | ✓ |
| **Responsive fill** | ✓ | ✓ | ✓ | ✓ |

### Recommendation
Use BorderLayout for **standard application layouts** with distinct border regions and a central content area. It's more structured than Dock and easier to understand than TableLayoutPanel for simple 5-zone layouts.
