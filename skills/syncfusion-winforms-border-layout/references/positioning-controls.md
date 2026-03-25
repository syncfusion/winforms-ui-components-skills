# Positioning Controls in BorderLayout

## Table of Contents
- [Understanding BorderPosition](#understanding-borderposition)
- [SetPosition() Method](#setposition-method)
- [Position Enumeration](#position-enumeration)
- [Setting Positions](#setting-positions)
- [Common Position Scenarios](#common-position-scenarios)
- [Multiple Controls at Same Position](#multiple-controls-at-same-position)
- [Troubleshooting Position Issues](#troubleshooting-position-issues)

## Understanding BorderPosition

BorderLayout uses the `BorderPosition` enumeration to specify where a child control should be positioned. Each position occupies a specific region:

```
┌─────────────────────────────────────────┐
│              NORTH (Top)                 │
├─────────┬─────────────────────┬─────────┤
│  WEST   │                     │  EAST   │
│ (Left)  │  CENTER (Middle)    │ (Right) │
│         │                     │         │
├─────────┴─────────────────────┴─────────┤
│             SOUTH (Bottom)                │
└─────────────────────────────────────────┘
```

## SetPosition() Method

### Syntax
```csharp
borderLayout1.SetPosition(Control control, BorderPosition position);
```

**Parameters:**
- `control`: The child control to position
- `position`: The BorderPosition enum value (North, South, East, West, or Center)

### Requirements
1. The control must already be added to the form
2. The BorderLayout's ContainerControl must be set first
3. Only one control should occupy each position (typically)

## Position Enumeration

The `BorderPosition` enum has five values:

| Value | Position | Typical Use |
|-------|----------|------------|
| `North` | Top of layout | Headers, toolbars, top navigation |
| `South` | Bottom of layout | Footers, status bars, bottom controls |
| `East` | Right side | Right panel, properties panel |
| `West` | Left side | Sidebars, left navigation, tools panel |
| `Center` | Middle area (fills remaining space) | Main content area, data grids |

## Setting Positions

### Basic Position Assignment

**C#:**
```csharp
// Create panels
Panel headerPanel = new Panel();
Panel contentPanel = new Panel();
Panel footerPanel = new Panel();

// Add to form
this.Controls.Add(headerPanel);
this.Controls.Add(contentPanel);
this.Controls.Add(footerPanel);

// Set positions
borderLayout1.SetPosition(headerPanel, BorderPosition.North);
borderLayout1.SetPosition(contentPanel, BorderPosition.Center);
borderLayout1.SetPosition(footerPanel, BorderPosition.South);
```

**VB.NET:**
```vb
' Create panels
Dim headerPanel As Panel = New Panel()
Dim contentPanel As Panel = New Panel()
Dim footerPanel As Panel = New Panel()

' Add to form
Me.Controls.Add(headerPanel)
Me.Controls.Add(contentPanel)
Me.Controls.Add(footerPanel)

' Set positions
borderLayout1.SetPosition(headerPanel, BorderPosition.North)
borderLayout1.SetPosition(contentPanel, BorderPosition.Center)
borderLayout1.SetPosition(footerPanel, BorderPosition.South)
```

### Setting Sizes with Positions

Combine position with size properties for precise layout:

```csharp
// Header with fixed height
Panel headerPanel = new Panel() { Height = 60, BackColor = Color.LightBlue };
borderLayout1.SetPosition(headerPanel, BorderPosition.North);

// Sidebar with fixed width
Panel sidebarPanel = new Panel() { Width = 250, BackColor = Color.LightGray };
borderLayout1.SetPosition(sidebarPanel, BorderPosition.West);

// Content area fills remaining space
Panel contentPanel = new Panel() { BackColor = Color.White };
borderLayout1.SetPosition(contentPanel, BorderPosition.Center);

// Footer with fixed height
Panel footerPanel = new Panel() { Height = 40, BackColor = Color.LightBlue };
borderLayout1.SetPosition(footerPanel, BorderPosition.South);
```

## Common Position Scenarios

### Scenario 1: Header + Content + Footer (Classic Layout)

```csharp
Panel headerPanel = new Panel() { Height = 50, BackColor = Color.DarkBlue };
Panel contentPanel = new Panel() { BackColor = Color.White };
Panel footerPanel = new Panel() { Height = 40, BackColor = Color.DarkBlue };

this.Controls.Add(headerPanel);
this.Controls.Add(contentPanel);
this.Controls.Add(footerPanel);

borderLayout1.SetPosition(headerPanel, BorderPosition.North);
borderLayout1.SetPosition(contentPanel, BorderPosition.Center);
borderLayout1.SetPosition(footerPanel, BorderPosition.South);
```

**Layout Result:**
```
┌─────────────────────────────────┐
│   Header (DarkBlue, height=50)  │
├─────────────────────────────────┤
│                                 │
│   Content (White, fills space)  │
│                                 │
├─────────────────────────────────┤
│   Footer (DarkBlue, height=40)  │
└─────────────────────────────────┘
```

### Scenario 2: Sidebar + Content (Left Navigation)

```csharp
Panel sidebarPanel = new Panel() { Width = 200, BackColor = Color.LightGray };
Panel contentPanel = new Panel() { BackColor = Color.White };

this.Controls.Add(sidebarPanel);
this.Controls.Add(contentPanel);

borderLayout1.SetPosition(sidebarPanel, BorderPosition.West);
borderLayout1.SetPosition(contentPanel, BorderPosition.Center);
```

**Layout Result:**
```
┌──────────────┬─────────────────────────┐
│              │                         │
│  Sidebar     │   Content (fills)       │
│ (width=200)  │                         │
│              │                         │
└──────────────┴─────────────────────────┘
```

### Scenario 3: Full Dashboard (All Five Positions)

```csharp
Panel topPanel = new Panel() { Height = 50, BackColor = Color.Blue };
Panel bottomPanel = new Panel() { Height = 40, BackColor = Color.Blue };
Panel leftPanel = new Panel() { Width = 150, BackColor = Color.Gray };
Panel rightPanel = new Panel() { Width = 150, BackColor = Color.Gray };
Panel centerPanel = new Panel() { BackColor = Color.White };

this.Controls.Add(topPanel);
this.Controls.Add(bottomPanel);
this.Controls.Add(leftPanel);
this.Controls.Add(rightPanel);
this.Controls.Add(centerPanel);

borderLayout1.SetPosition(topPanel, BorderPosition.North);
borderLayout1.SetPosition(bottomPanel, BorderPosition.South);
borderLayout1.SetPosition(leftPanel, BorderPosition.West);
borderLayout1.SetPosition(rightPanel, BorderPosition.East);
borderLayout1.SetPosition(centerPanel, BorderPosition.Center);
```

**Layout Result:**
```
┌─────┬──────────────────────────────┬─────┐
│     │          Top (50)            │     │
├─────┼──────────────────────────────┼─────┤
│Left │                              │Right│
│150  │   Center (fills)             │ 150 │
│     │                              │     │
├─────┴──────────────────────────────┴─────┤
│          Bottom (40)                      │
└─────────────────────────────────────────┘
```

### Scenario 4: Toolbar + Header + Sidebar + Content + Footer

```csharp
// Toolbar at top (fixed height)
Panel toolbarPanel = new Panel() { Height = 40, BackColor = Color.LightGray };
borderLayout1.SetPosition(toolbarPanel, BorderPosition.North);

// This requires nesting - use another BorderLayout inside a panel
Panel mainPanel = new Panel();
borderLayout1.SetPosition(mainPanel, BorderPosition.Center);

// Inside mainPanel, use another BorderLayout
BorderLayout innerLayout = new BorderLayout();
innerLayout.ContainerControl = mainPanel;

Panel headerPanel = new Panel() { Height = 30, BackColor = Color.DarkBlue };
Panel sidebarPanel = new Panel() { Width = 200, BackColor = Color.LightGray };
Panel contentPanel = new Panel() { BackColor = Color.White };
Panel footerPanel = new Panel() { Height = 35, BackColor = Color.DarkBlue };

mainPanel.Controls.Add(headerPanel);
mainPanel.Controls.Add(sidebarPanel);
mainPanel.Controls.Add(contentPanel);
mainPanel.Controls.Add(footerPanel);

innerLayout.SetPosition(headerPanel, BorderPosition.North);
innerLayout.SetPosition(sidebarPanel, BorderPosition.West);
innerLayout.SetPosition(contentPanel, BorderPosition.Center);
innerLayout.SetPosition(footerPanel, BorderPosition.South);
```

## Multiple Controls at Same Position

### Important Note
While BorderLayout allows setting multiple controls to the same position, **only the last control set for that position will be visible and positioned correctly**. 

### Workaround: Use a Container Control

If you need multiple controls in one position (e.g., multiple buttons in the header), use a container like Panel or GroupBox:

```csharp
// Create a panel to hold multiple controls
Panel headerPanel = new Panel() { Height = 50, BackColor = Color.LightBlue };

// Add controls to the header panel
ButtonAdv homeBtn = new ButtonAdv() { Text = "Home", Dock = DockStyle.Left };
ButtonAdv aboutBtn = new ButtonAdv() { Text = "About", Dock = DockStyle.Left };
ButtonAdv helpBtn = new ButtonAdv() { Text = "Help", Dock = DockStyle.Left };

headerPanel.Controls.Add(homeBtn);
headerPanel.Controls.Add(aboutBtn);
headerPanel.Controls.Add(helpBtn);

// Add header panel to form
this.Controls.Add(headerPanel);

// Position the entire panel
borderLayout1.SetPosition(headerPanel, BorderPosition.North);
```

**VB.NET:**
```vb
' Create a panel to hold multiple controls
Dim headerPanel As Panel = New Panel() With {.Height = 50, .BackColor = Color.LightBlue}

' Add controls to the header panel
Dim homeBtn As ButtonAdv = New ButtonAdv() With {.Text = "Home", .Dock = DockStyle.Left}
Dim aboutBtn As ButtonAdv = New ButtonAdv() With {.Text = "About", .Dock = DockStyle.Left}
Dim helpBtn As ButtonAdv = New ButtonAdv() With {.Text = "Help", .Dock = DockStyle.Left}

headerPanel.Controls.Add(homeBtn)
headerPanel.Controls.Add(aboutBtn)
headerPanel.Controls.Add(helpBtn)

' Add header panel to form
Me.Controls.Add(headerPanel)

' Position the entire panel
borderLayout1.SetPosition(headerPanel, BorderPosition.North)
```

### Example: Button Toolbar in Header

```csharp
Panel headerPanel = new Panel() { Height = 60, BackColor = Color.LightBlue };

// Create toolbar buttons
ButtonAdv newBtn = new ButtonAdv() 
{ 
    Text = "New", 
    Dock = DockStyle.Left, 
    Width = 80 
};

ButtonAdv openBtn = new ButtonAdv() 
{ 
    Text = "Open", 
    Dock = DockStyle.Left, 
    Width = 80 
};

ButtonAdv saveBtn = new ButtonAdv() 
{ 
    Text = "Save", 
    Dock = DockStyle.Left, 
    Width = 80 
};

// Add buttons to header
headerPanel.Controls.Add(newBtn);
headerPanel.Controls.Add(openBtn);
headerPanel.Controls.Add(saveBtn);

// Add header to form
this.Controls.Add(headerPanel);

// Position header
borderLayout1.SetPosition(headerPanel, BorderPosition.North);
```

## Troubleshooting Position Issues

### Issue: Controls Not Appearing in Correct Position

**Cause 1: ContainerControl not set**
```csharp
// Wrong - BorderLayout doesn't know its container
borderLayout1.SetPosition(myControl, BorderPosition.North);

// Correct - Set container first
borderLayout1.ContainerControl = this;
borderLayout1.SetPosition(myControl, BorderPosition.North);
```

**Cause 2: Control not added to form**
```csharp
// Wrong - Control positioned but not added to form
borderLayout1.SetPosition(myPanel, BorderPosition.North);

// Correct - Add to form first, then position
this.Controls.Add(myPanel);
borderLayout1.SetPosition(myPanel, BorderPosition.North);
```

### Issue: Center Control Not Filling Available Space

**Solution: Ensure other controls have fixed sizes**
```csharp
// Header and footer must have explicit heights
Panel headerPanel = new Panel() { Height = 50 };  // Fixed height
Panel contentPanel = new Panel();  // No height - fills remaining
Panel footerPanel = new Panel() { Height = 40 };  // Fixed height

this.Controls.Add(headerPanel);
this.Controls.Add(contentPanel);
this.Controls.Add(footerPanel);

borderLayout1.SetPosition(headerPanel, BorderPosition.North);
borderLayout1.SetPosition(contentPanel, BorderPosition.Center);  // Will fill
borderLayout1.SetPosition(footerPanel, BorderPosition.South);
```

### Issue: Overlapping Controls

**Cause: Multiple controls assigned same position**
**Solution: Use container for multiple controls**
```csharp
// Wrong - Both controls try to occupy West
borderLayout1.SetPosition(control1, BorderPosition.West);
borderLayout1.SetPosition(control2, BorderPosition.West);  // Overlaps control1

// Correct - Use container
Panel westPanel = new Panel();
westPanel.Controls.Add(control1);
westPanel.Controls.Add(control2);
this.Controls.Add(westPanel);
borderLayout1.SetPosition(westPanel, BorderPosition.West);
```

### Issue: Controls Resizing Unexpectedly

**Cause: Conflicting size settings**
**Solution: Set sizes before positioning**
```csharp
// Set height before positioning
Panel sidebarPanel = new Panel() { Width = 200 };  // Fixed width
borderLayout1.SetPosition(sidebarPanel, BorderPosition.West);

// Don't change size after positioning
// sidebarPanel.Width = 300;  // Avoid this
```
