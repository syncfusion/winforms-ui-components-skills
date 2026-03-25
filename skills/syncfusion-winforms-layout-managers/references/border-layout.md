# BorderLayout

BorderLayout is a powerful layout manager that arranges controls along five regions: North (top), South (bottom), East (right), West (left), and Center (middle). It provides a simple yet effective way to create application shells and structured interfaces.

## Table of Contents

- [BorderLayout](#borderlayout)
  - [Table of Contents](#table-of-contents)
  - [What is BorderLayout](#what-is-borderlayout)
    - [Purpose](#purpose)
    - [Key Characteristics](#key-characteristics)
    - [Position-Based Constraints](#position-based-constraints)
  - [Key Features](#key-features)
    - [Five Position Regions](#five-position-regions)
    - [Spacing Control](#spacing-control)
    - [Size Customization](#size-customization)
  - [Position Constraints](#position-constraints)
    - [BorderPosition Enum](#borderposition-enum)
    - [Region Descriptions](#region-descriptions)
    - [Layout Visualization](#layout-visualization)
  - [Adding Controls via Designer](#adding-controls-via-designer)
    - [Step-by-Step Designer Instructions](#step-by-step-designer-instructions)
    - [Setting Constraints in Designer](#setting-constraints-in-designer)
  - [Adding Controls via Code](#adding-controls-via-code)
    - [Complete Code Example with All Positions](#complete-code-example-with-all-positions)
  - [Spacing Configuration](#spacing-configuration)
    - [HGap and VGap Properties](#hgap-and-vgap-properties)
    - [Spacing Code Examples](#spacing-code-examples)
  - [Size Customization](#size-customization-1)
    - [Preferred Size for Border Regions](#preferred-size-for-border-regions)
    - [Size Configuration Example](#size-configuration-example)
  - [Complete Examples](#complete-examples)
    - [MDI-Style Application Shell](#mdi-style-application-shell)
    - [Document Viewer with Toolbars](#document-viewer-with-toolbars)
  - [Common Patterns](#common-patterns)
    - [Application Shell with Navigation](#application-shell-with-navigation)
    - [Document Viewer Pattern](#document-viewer-pattern)
    - [Split View with Header and Footer](#split-view-with-header-and-footer)
  - [Best Practices](#best-practices)
    - [Essential Guidelines](#essential-guidelines)
    - [When to Use BorderLayout](#when-to-use-borderlayout)
    - [When Not to Use BorderLayout](#when-not-to-use-borderlayout)
  - [Troubleshooting](#troubleshooting)
    - [Controls Not Appearing](#controls-not-appearing)
    - [Overlapping Controls](#overlapping-controls)
    - [Spacing Issues](#spacing-issues)
    - [Controls Not Resizing](#controls-not-resizing)

## What is BorderLayout

### Purpose

BorderLayout is a layout manager that divides a container into five distinct regions, allowing you to dock controls along the borders and fill the center area. This approach is similar to .NET's built-in docking support but provides more control and consistency.

BorderLayout is ideal for creating:
- Application main windows with toolbars and status bars
- Document viewers with surrounding controls
- Dashboard layouts with fixed regions
- MDI-style applications with persistent UI elements

### Key Characteristics

- **Five Regions**: North, South, East, West, and Center
- **Manual Configuration**: Unlike FlowLayout or GridLayout, BorderLayout does not automatically arrange children - you must explicitly assign each control to a position
- **Single Control Per Position**: Each region can contain only one control
- **Center Fills Remaining Space**: The center region automatically expands to fill available space after border regions are sized
- **Spacing Support**: HGap and VGap properties control spacing between regions

### Position-Based Constraints

BorderLayout uses position-based constraints through the `BorderPosition` enumeration. Each child control must be assigned to one of the five positions using the `SetPosition()` method:

```csharp
borderLayout1.SetPosition(control, BorderPosition.North);
```

## Key Features

### Five Position Regions

BorderLayout provides five distinct positioning options:

| Position | Location | Sizing Behavior |
|----------|----------|-----------------|
| **North** | Top edge | Full width, preferred height |
| **South** | Bottom edge | Full width, preferred height |
| **East** | Right edge | Preferred width, remaining height |
| **West** | Left edge | Preferred width, remaining height |
| **Center** | Middle | Fills all remaining space |

### Spacing Control

Control the gaps between layout regions:
- **HGap**: Horizontal spacing between regions
- **VGap**: Vertical spacing between regions

### Size Customization

Each border region can have a custom size:
- North/South: Specify height
- East/West: Specify width
- Center: Automatically calculated based on remaining space

## Position Constraints

### BorderPosition Enum

The `BorderPosition` enumeration defines the five available positions:

```csharp
public enum BorderPosition
{
    North,    // Top region
    South,    // Bottom region
    East,     // Right region
    West,     // Left region
    Center    // Center region
}
```

### Region Descriptions

**North Position**
- Located at the top edge of the container
- Spans the full width of the container
- Uses the control's preferred height
- Ideal for: Toolbars, menu bars, title panels

**South Position**
- Located at the bottom edge of the container
- Spans the full width of the container
- Uses the control's preferred height
- Ideal for: Status bars, action buttons, footer information

**East Position**
- Located on the right edge of the container
- Spans the height between North and South regions
- Uses the control's preferred width
- Ideal for: Side panels, property grids, navigation panes

**West Position**
- Located on the left edge of the container
- Spans the height between North and South regions
- Uses the control's preferred width
- Ideal for: Sidebars, tree views, tool palettes

**Center Position**
- Located in the middle of the container
- Fills all space not occupied by border regions
- Automatically resizes with container
- Ideal for: Main content area, document viewers, work areas

### Layout Visualization

```
┌─────────────────────────────────────────┐
│             NORTH                       │
│         (Full Width)                    │
├──────────┬──────────────────┬───────────┤
│          │                  │           │
│   WEST   │     CENTER       │   EAST    │
│ (Height  │  (Fills Space)   │ (Height   │
│  Between │                  │  Between  │
│  N & S)  │                  │  N & S)   │
│          │                  │           │
├──────────┴──────────────────┴───────────┤
│             SOUTH                       │
│         (Full Width)                    │
└─────────────────────────────────────────┘
```

## Adding Controls via Designer

### Step-by-Step Designer Instructions

1. **Add BorderLayout to Form**
   - Open your form in the Visual Studio designer
   - Locate "BorderLayout" in the toolbox (Syncfusion Controls section)
   - Drag and drop BorderLayout onto your form
   - It appears in the component tray at the bottom

2. **Set Container Control**
   - When prompted, click "Yes" to set the form as the container
   - Or manually: Select BorderLayout in component tray → Properties window → Set `ContainerControl` to your form or panel

3. **Add Child Controls**
   - Drag controls (buttons, panels, etc.) from the toolbox onto the container
   - Each control is added to the container's Controls collection

4. **Set Position for Each Control**
   - Select a child control
   - In Properties window, find extended property: "Position on borderLayout1"
   - Choose position: North, South, East, West, or Center

5. **Configure Spacing (Optional)**
   - Select BorderLayout in component tray
   - Set `HGap` property for horizontal spacing
   - Set `VGap` property for vertical spacing

6. **Configure Margins (Optional)**
   - Set `TopMargin`, `BottomMargin`, `HorzNearMargin`, `HorzFarMargin` properties

### Setting Constraints in Designer

The BorderLayout control provides an extended property for each child control called "Position on borderLayout1". This property appears in the Properties window when you select a child control.

**To set the position:**
1. Select the child control in the designer
2. Scroll to the extended properties section (usually at the top)
3. Find "Position on borderLayout1"
4. Select from dropdown: North, South, East, West, or Center

## Adding Controls via Code

### Complete Code Example with All Positions

**C# Example:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class BorderLayoutExample : Form
{
    private BorderLayout borderLayout1;
    private Panel northPanel;
    private Panel southPanel;
    private Panel eastPanel;
    private Panel westPanel;
    private Panel centerPanel;

    public BorderLayoutExample()
    {
        InitializeComponents();
    }

    private void InitializeComponents()
    {
        // Form setup
        this.Text = "BorderLayout Example";
        this.Size = new Size(800, 600);

        // Create BorderLayout instance
        borderLayout1 = new BorderLayout();

        // Set the container control (the form)
        borderLayout1.ContainerControl = this;

        // Configure spacing
        borderLayout1.HGap = 5;
        borderLayout1.VGap = 5;

        // Configure margins
        borderLayout1.TopMargin = 10;
        borderLayout1.BottomMargin = 10;
        borderLayout1.HorzNearMargin = 10;
        borderLayout1.HorzFarMargin = 10;

        // Create North panel (toolbar area)
        northPanel = new Panel
        {
            BackColor = Color.LightBlue,
            Height = 60
        };
        Label northLabel = new Label
        {
            Text = "NORTH - Toolbar Area",
            Dock = DockStyle.Fill,
            TextAlign = ContentAlignment.MiddleCenter,
            Font = new Font("Arial", 12, FontStyle.Bold)
        };
        northPanel.Controls.Add(northLabel);

        // Create South panel (status bar area)
        southPanel = new Panel
        {
            BackColor = Color.LightGreen,
            Height = 40
        };
        Label southLabel = new Label
        {
            Text = "SOUTH - Status Bar",
            Dock = DockStyle.Fill,
            TextAlign = ContentAlignment.MiddleCenter,
            Font = new Font("Arial", 10)
        };
        southPanel.Controls.Add(southLabel);

        // Create East panel (side panel)
        eastPanel = new Panel
        {
            BackColor = Color.LightCoral,
            Width = 150
        };
        Label eastLabel = new Label
        {
            Text = "EAST\nProperties",
            Dock = DockStyle.Fill,
            TextAlign = ContentAlignment.MiddleCenter,
            Font = new Font("Arial", 10)
        };
        eastPanel.Controls.Add(eastLabel);

        // Create West panel (navigation)
        westPanel = new Panel
        {
            BackColor = Color.LightGoldenrodYellow,
            Width = 150
        };
        Label westLabel = new Label
        {
            Text = "WEST\nNavigation",
            Dock = DockStyle.Fill,
            TextAlign = ContentAlignment.MiddleCenter,
            Font = new Font("Arial", 10)
        };
        westPanel.Controls.Add(westLabel);

        // Create Center panel (main content)
        centerPanel = new Panel
        {
            BackColor = Color.White
        };
        Label centerLabel = new Label
        {
            Text = "CENTER\nMain Content Area\n(Automatically fills remaining space)",
            Dock = DockStyle.Fill,
            TextAlign = ContentAlignment.MiddleCenter,
            Font = new Font("Arial", 12)
        };
        centerPanel.Controls.Add(centerLabel);

        // Add all panels to the form
        this.Controls.AddRange(new Control[] 
        { 
            northPanel, 
            southPanel, 
            eastPanel, 
            westPanel, 
            centerPanel 
        });

        // Set positions for each panel
        borderLayout1.SetPosition(northPanel, BorderPosition.North);
        borderLayout1.SetPosition(southPanel, BorderPosition.South);
        borderLayout1.SetPosition(eastPanel, BorderPosition.East);
        borderLayout1.SetPosition(westPanel, BorderPosition.West);
        borderLayout1.SetPosition(centerPanel, BorderPosition.Center);
    }

    [STAThread]
    static void Main()
    {
        Application.EnableVisualStyles();
        Application.Run(new BorderLayoutExample());
    }
}
```

**VB.NET Example:**
```vb
Imports System
Imports System.Drawing
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Public Class BorderLayoutExample
    Inherits Form
    
    Private borderLayout1 As BorderLayout
    Private northPanel As Panel
    Private southPanel As Panel
    Private eastPanel As Panel
    Private westPanel As Panel
    Private centerPanel As Panel

    Public Sub New()
        InitializeComponents()
    End Sub

    Private Sub InitializeComponents()
        ' Form setup
        Me.Text = "BorderLayout Example"
        Me.Size = New Size(800, 600)

        ' Create BorderLayout instance
        borderLayout1 = New BorderLayout()

        ' Set the container control (the form)
        borderLayout1.ContainerControl = Me

        ' Configure spacing
        borderLayout1.HGap = 5
        borderLayout1.VGap = 5

        ' Configure margins
        borderLayout1.TopMargin = 10
        borderLayout1.BottomMargin = 10
        borderLayout1.HorzNearMargin = 10
        borderLayout1.HorzFarMargin = 10

        ' Create North panel (toolbar area)
        northPanel = New Panel With {
            .BackColor = Color.LightBlue,
            .Height = 60
        }
        Dim northLabel As New Label With {
            .Text = "NORTH - Toolbar Area",
            .Dock = DockStyle.Fill,
            .TextAlign = ContentAlignment.MiddleCenter,
            .Font = New Font("Arial", 12, FontStyle.Bold)
        }
        northPanel.Controls.Add(northLabel)

        ' Create South panel (status bar area)
        southPanel = New Panel With {
            .BackColor = Color.LightGreen,
            .Height = 40
        }
        Dim southLabel As New Label With {
            .Text = "SOUTH - Status Bar",
            .Dock = DockStyle.Fill,
            .TextAlign = ContentAlignment.MiddleCenter,
            .Font = New Font("Arial", 10)
        }
        southPanel.Controls.Add(southLabel)

        ' Create East panel (side panel)
        eastPanel = New Panel With {
            .BackColor = Color.LightCoral,
            .Width = 150
        }
        Dim eastLabel As New Label With {
            .Text = "EAST" & vbCrLf & "Properties",
            .Dock = DockStyle.Fill,
            .TextAlign = ContentAlignment.MiddleCenter,
            .Font = New Font("Arial", 10)
        }
        eastPanel.Controls.Add(eastLabel)

        ' Create West panel (navigation)
        westPanel = New Panel With {
            .BackColor = Color.LightGoldenrodYellow,
            .Width = 150
        }
        Dim westLabel As New Label With {
            .Text = "WEST" & vbCrLf & "Navigation",
            .Dock = DockStyle.Fill,
            .TextAlign = ContentAlignment.MiddleCenter,
            .Font = New Font("Arial", 10)
        }
        westPanel.Controls.Add(westLabel)

        ' Create Center panel (main content)
        centerPanel = New Panel With {
            .BackColor = Color.White
        }
        Dim centerLabel As New Label With {
            .Text = "CENTER" & vbCrLf & "Main Content Area" & vbCrLf & "(Automatically fills remaining space)",
            .Dock = DockStyle.Fill,
            .TextAlign = ContentAlignment.MiddleCenter,
            .Font = New Font("Arial", 12)
        }
        centerPanel.Controls.Add(centerLabel)

        ' Add all panels to the form
        Me.Controls.AddRange(New Control() {
            northPanel,
            southPanel,
            eastPanel,
            westPanel,
            centerPanel
        })

        ' Set positions for each panel
        borderLayout1.SetPosition(northPanel, BorderPosition.North)
        borderLayout1.SetPosition(southPanel, BorderPosition.South)
        borderLayout1.SetPosition(eastPanel, BorderPosition.East)
        borderLayout1.SetPosition(westPanel, BorderPosition.West)
        borderLayout1.SetPosition(centerPanel, BorderPosition.Center)
    End Sub

    <STAThread()>
    Shared Sub Main()
        Application.EnableVisualStyles()
        Application.Run(New BorderLayoutExample())
    End Sub
End Class
```

## Spacing Configuration

### HGap and VGap Properties

BorderLayout provides two properties for controlling spacing between regions:

**HGap (Horizontal Gap)**
- Adds spacing between East/West regions and the Center
- Measured in pixels
- Default value: 0

**VGap (Vertical Gap)**
- Adds spacing between North/South regions and other regions
- Measured in pixels
- Default value: 0

### Spacing Code Examples

**C# Example:**
```csharp
// Set horizontal gap of 10 pixels
this.borderLayout1.HGap = 10;

// Set vertical gap of 10 pixels
this.borderLayout1.VGap = 10;

// Result: 10-pixel spacing between all regions
```

**VB.NET Example:**
```vb
' Set horizontal gap of 10 pixels
Me.borderLayout1.HGap = 10

' Set vertical gap of 10 pixels
Me.borderLayout1.VGap = 10

' Result: 10-pixel spacing between all regions
```

**Complete Example with Spacing:**
```csharp
BorderLayout borderLayout1 = new BorderLayout();
borderLayout1.ContainerControl = this;

// Configure spacing and margins
borderLayout1.HGap = 15;
borderLayout1.VGap = 15;
borderLayout1.TopMargin = 20;
borderLayout1.BottomMargin = 20;
borderLayout1.HorzNearMargin = 20;
borderLayout1.HorzFarMargin = 20;

// Add controls...
```

## Size Customization

### Preferred Size for Border Regions

Each border region can have a customized size:

**North and South Regions:**
- Control the **height** of these regions
- Set using the control's `Height` property or `PreferredSize`
- Width automatically spans the full container width

**East and West Regions:**
- Control the **width** of these regions
- Set using the control's `Width` property or `PreferredSize`
- Height automatically spans between North and South regions

**Center Region:**
- Size is automatically calculated
- Fills all remaining space after border regions are sized
- Cannot be explicitly sized

### Size Configuration Example

**C# Example:**
```csharp
// Create panels with specific sizes
Panel northPanel = new Panel { Height = 80 };  // 80 pixels tall
Panel southPanel = new Panel { Height = 30 };  // 30 pixels tall
Panel eastPanel = new Panel { Width = 200 };   // 200 pixels wide
Panel westPanel = new Panel { Width = 150 };   // 150 pixels wide
Panel centerPanel = new Panel();               // Fills remaining space

// Add to container
this.Controls.AddRange(new Control[] 
{ 
    northPanel, southPanel, eastPanel, westPanel, centerPanel 
});

// Set positions
borderLayout1.SetPosition(northPanel, BorderPosition.North);
borderLayout1.SetPosition(southPanel, BorderPosition.South);
borderLayout1.SetPosition(eastPanel, BorderPosition.East);
borderLayout1.SetPosition(westPanel, BorderPosition.West);
borderLayout1.SetPosition(centerPanel, BorderPosition.Center);
```

**VB.NET Example:**
```vb
' Create panels with specific sizes
Dim northPanel As New Panel With {.Height = 80}   ' 80 pixels tall
Dim southPanel As New Panel With {.Height = 30}   ' 30 pixels tall
Dim eastPanel As New Panel With {.Width = 200}    ' 200 pixels wide
Dim westPanel As New Panel With {.Width = 150}    ' 150 pixels wide
Dim centerPanel As New Panel()                    ' Fills remaining space

' Add to container
Me.Controls.AddRange(New Control() {
    northPanel, southPanel, eastPanel, westPanel, centerPanel
})

' Set positions
borderLayout1.SetPosition(northPanel, BorderPosition.North)
borderLayout1.SetPosition(southPanel, BorderPosition.South)
borderLayout1.SetPosition(eastPanel, BorderPosition.East)
borderLayout1.SetPosition(westPanel, BorderPosition.West)
borderLayout1.SetPosition(centerPanel, BorderPosition.Center)
```

## Complete Examples

### MDI-Style Application Shell

This example creates a complete MDI-style application with header, footer, sidebar, and main content area.

**C# Complete Example:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class MDIApplicationShell : Form
{
    private BorderLayout borderLayout1;
    private Panel headerPanel;
    private Panel footerPanel;
    private Panel sidebarPanel;
    private Panel contentPanel;
    private MenuStrip menuStrip;
    private StatusStrip statusStrip;
    private TreeView navigationTree;
    private RichTextBox contentArea;

    public MDIApplicationShell()
    {
        InitializeComponents();
    }

    private void InitializeComponents()
    {
        // Form setup
        this.Text = "MDI Application Shell";
        this.Size = new Size(1024, 768);
        this.StartPosition = FormStartPosition.CenterScreen;

        // Create BorderLayout
        borderLayout1 = new BorderLayout();
        borderLayout1.ContainerControl = this;
        borderLayout1.HGap = 2;
        borderLayout1.VGap = 2;

        // Create Header Panel with Menu
        headerPanel = new Panel { Height = 60, BackColor = Color.FromArgb(0, 120, 215) };
        menuStrip = new MenuStrip
        {
            Dock = DockStyle.Top,
            BackColor = Color.FromArgb(0, 120, 215),
            ForeColor = Color.White
        };
        menuStrip.Items.Add(new ToolStripMenuItem("File"));
        menuStrip.Items.Add(new ToolStripMenuItem("Edit"));
        menuStrip.Items.Add(new ToolStripMenuItem("View"));
        menuStrip.Items.Add(new ToolStripMenuItem("Help"));
        headerPanel.Controls.Add(menuStrip);

        Label titleLabel = new Label
        {
            Text = "My Application",
            Dock = DockStyle.Fill,
            TextAlign = ContentAlignment.MiddleCenter,
            Font = new Font("Segoe UI", 16, FontStyle.Bold),
            ForeColor = Color.White
        };
        headerPanel.Controls.Add(titleLabel);
        titleLabel.BringToFront();

        // Create Footer Panel with Status
        footerPanel = new Panel { Height = 25, BackColor = Color.FromArgb(240, 240, 240) };
        statusStrip = new StatusStrip
        {
            Dock = DockStyle.Fill,
            BackColor = Color.FromArgb(240, 240, 240)
        };
        statusStrip.Items.Add(new ToolStripStatusLabel("Ready"));
        statusStrip.Items.Add(new ToolStripStatusLabel("Line 1, Col 1") 
        { 
            Spring = true, 
            TextAlign = ContentAlignment.MiddleRight 
        });
        footerPanel.Controls.Add(statusStrip);

        // Create Sidebar Panel with Navigation
        sidebarPanel = new Panel { Width = 200, BackColor = Color.FromArgb(245, 245, 245) };
        navigationTree = new TreeView
        {
            Dock = DockStyle.Fill,
            BorderStyle = BorderStyle.None
        };
        TreeNode rootNode = navigationTree.Nodes.Add("Navigation");
        rootNode.Nodes.Add("Home");
        rootNode.Nodes.Add("Documents");
        rootNode.Nodes.Add("Settings");
        rootNode.ExpandAll();
        sidebarPanel.Controls.Add(navigationTree);

        // Create Content Panel
        contentPanel = new Panel { BackColor = Color.White };
        contentArea = new RichTextBox
        {
            Dock = DockStyle.Fill,
            BorderStyle = BorderStyle.None,
            Font = new Font("Consolas", 10),
            Text = "Main content area\n\nThis area automatically fills remaining space."
        };
        contentPanel.Controls.Add(contentArea);

        // Add all panels to form
        this.Controls.AddRange(new Control[]
        {
            headerPanel,
            footerPanel,
            sidebarPanel,
            contentPanel
        });

        // Set BorderLayout positions
        borderLayout1.SetPosition(headerPanel, BorderPosition.North);
        borderLayout1.SetPosition(footerPanel, BorderPosition.South);
        borderLayout1.SetPosition(sidebarPanel, BorderPosition.West);
        borderLayout1.SetPosition(contentPanel, BorderPosition.Center);
    }

    [STAThread]
    static void Main()
    {
        Application.EnableVisualStyles();
        Application.SetCompatibleTextRenderingDefault(false);
        Application.Run(new MDIApplicationShell());
    }
}
```

### Document Viewer with Toolbars

**C# Complete Example:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class DocumentViewer : Form
{
    private BorderLayout borderLayout1;
    private Panel toolbarPanel;
    private Panel statusPanel;
    private Panel propertiesPanel;
    private Panel documentPanel;

    public DocumentViewer()
    {
        InitializeComponents();
    }

    private void InitializeComponents()
    {
        this.Text = "Document Viewer";
        this.Size = new Size(900, 600);

        borderLayout1 = new BorderLayout();
        borderLayout1.ContainerControl = this;
        borderLayout1.HGap = 3;
        borderLayout1.VGap = 3;
        borderLayout1.TopMargin = 5;
        borderLayout1.BottomMargin = 5;
        borderLayout1.HorzNearMargin = 5;
        borderLayout1.HorzFarMargin = 5;

        // Toolbar
        toolbarPanel = new Panel { Height = 50, BackColor = Color.WhiteSmoke };
        Button btnOpen = new Button { Text = "Open", Location = new Point(10, 10), Size = new Size(70, 30) };
        Button btnSave = new Button { Text = "Save", Location = new Point(90, 10), Size = new Size(70, 30) };
        Button btnPrint = new Button { Text = "Print", Location = new Point(170, 10), Size = new Size(70, 30) };
        toolbarPanel.Controls.AddRange(new Control[] { btnOpen, btnSave, btnPrint });

        // Status bar
        statusPanel = new Panel { Height = 30, BackColor = Color.LightGray };
        Label statusLabel = new Label 
        { 
            Text = "Document: Untitled", 
            Dock = DockStyle.Fill, 
            TextAlign = ContentAlignment.MiddleLeft,
            Padding = new Padding(10, 0, 0, 0)
        };
        statusPanel.Controls.Add(statusLabel);

        // Properties panel
        propertiesPanel = new Panel { Width = 180, BackColor = Color.FromArgb(250, 250, 250) };
        Label propTitle = new Label 
        { 
            Text = "Properties", 
            Dock = DockStyle.Top, 
            Height = 30,
            TextAlign = ContentAlignment.MiddleCenter,
            Font = new Font("Arial", 10, FontStyle.Bold),
            BackColor = Color.LightSteelBlue
        };
        propertiesPanel.Controls.Add(propTitle);

        // Document area
        documentPanel = new Panel { BackColor = Color.White };
        RichTextBox documentView = new RichTextBox 
        { 
            Dock = DockStyle.Fill,
            BorderStyle = BorderStyle.None,
            Font = new Font("Arial", 11),
            Text = "Document content goes here..."
        };
        documentPanel.Controls.Add(documentView);

        // Add all panels
        this.Controls.AddRange(new Control[]
        {
            toolbarPanel,
            statusPanel,
            propertiesPanel,
            documentPanel
        });

        // Set positions
        borderLayout1.SetPosition(toolbarPanel, BorderPosition.North);
        borderLayout1.SetPosition(statusPanel, BorderPosition.South);
        borderLayout1.SetPosition(propertiesPanel, BorderPosition.East);
        borderLayout1.SetPosition(documentPanel, BorderPosition.Center);
    }

    [STAThread]
    static void Main()
    {
        Application.EnableVisualStyles();
        Application.Run(new DocumentViewer());
    }
}
```

## Common Patterns

### Application Shell with Navigation

Use BorderLayout to create an application shell with:
- **North**: Title bar and main menu
- **West**: Navigation tree or menu
- **Center**: Main work area
- **South**: Status information

### Document Viewer Pattern

Typical layout for document viewers:
- **North**: Toolbar with actions (Open, Save, Print)
- **East**: Properties or formatting panel
- **Center**: Document display area
- **South**: Status and information bar

### Split View with Header and Footer

Create interfaces with persistent header and footer:
- **North**: Header with branding/navigation
- **South**: Footer with copyright/links
- **Center**: Main content that scrolls

## Best Practices

### Essential Guidelines

1. **Set Container Sizes Appropriately**
   - Ensure the container (form or panel) is large enough for all regions
   - Consider minimum sizes for usability

2. **Center Region Adapts to Remaining Space**
   - Don't try to size the center region explicitly
   - Let it fill naturally based on border region sizes

3. **Use for Fixed-Region Layouts**
   - BorderLayout excels at layouts with persistent, fixed regions
   - Each region maintains its position as the container resizes

4. **One Control Per Position**
   - BorderLayout allows only one control per position
   - If you need multiple controls in a region, use a Panel and add controls to it

5. **Set Appropriate Sizes**
   - North/South: Set Height property
   - East/West: Set Width property
   - Sizes should be based on content requirements

### When to Use BorderLayout

- Application main windows with toolbars and status bars
- Document viewers with surrounding UI
- Dashboard layouts with fixed regions
- When you need docking-like behavior with more control

### When Not to Use BorderLayout

- **Dynamic Control Addition**: If you need to add/remove controls frequently, use FlowLayout or GridLayout
- **Complex Nested Layouts**: For sophisticated arrangements, consider using multiple layout managers with nested panels
- **Advanced Docking**: For complex docking scenarios with floating windows, use Syncfusion's DockingManager instead

## Troubleshooting

### Controls Not Appearing

**Problem**: Child controls are not visible in the layout.

**Solutions:**
1. Verify ContainerControl is set:
   ```csharp
   if (borderLayout1.ContainerControl == null)
   {
       borderLayout1.ContainerControl = this;
   }
   ```

2. Check that position is assigned:
   ```csharp
   // Each control must have a position
   borderLayout1.SetPosition(myPanel, BorderPosition.North);
   ```

3. Ensure controls are added to container:
   ```csharp
   this.Controls.Add(myPanel);  // Must add to container
   ```

### Overlapping Controls

**Problem**: Multiple controls appear in the same position or overlap.

**Solutions:**
1. Only one control per position:
   ```csharp
   // DON'T DO THIS - only one control per position
   borderLayout1.SetPosition(panel1, BorderPosition.North);
   borderLayout1.SetPosition(panel2, BorderPosition.North); // This replaces panel1
   ```

2. Use panels to group controls:
   ```csharp
   // Group multiple controls in a panel
   Panel northPanel = new Panel();
   northPanel.Controls.Add(button1);
   northPanel.Controls.Add(button2);
   borderLayout1.SetPosition(northPanel, BorderPosition.North);
   ```

### Spacing Issues

**Problem**: Unexpected gaps or no spacing between regions.

**Solutions:**
1. Adjust HGap and VGap:
   ```csharp
   borderLayout1.HGap = 10;  // Horizontal spacing
   borderLayout1.VGap = 10;  // Vertical spacing
   ```

2. Check margin settings:
   ```csharp
   borderLayout1.TopMargin = 10;
   borderLayout1.BottomMargin = 10;
   borderLayout1.HorzNearMargin = 10;
   borderLayout1.HorzFarMargin = 10;
   ```

3. Verify control sizes don't include extra padding

### Controls Not Resizing

**Problem**: Border regions don't resize when container resizes.

**Solutions:**
1. Ensure AutoLayout is enabled (default):
   ```csharp
   borderLayout1.AutoLayout = true;
   ```

2. Manually trigger layout if needed:
   ```csharp
   borderLayout1.LayoutContainer();
   ```

3. Check that controls have appropriate size properties set

By following this guide, you can effectively use BorderLayout to create professional, well-structured Windows Forms applications with minimal code and maximum maintainability.