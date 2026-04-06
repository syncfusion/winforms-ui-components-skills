# Tabbed Windows

Tabbed docking groups multiple windows together with tabs, similar to browser tabs or Visual Studio document tabs. Users can switch between windows without losing screen space.

## Overview

**What:** Multiple dock windows combined into a single tabbed group.

**When to use:**
- Related tools in same area (Properties + Events)
- Space-constrained layouts
- Organize similar windows
- Document-style interfaces

**How:** Dock with `DockingStyle.Tabbed` or drag to create groups.

## Creating Tabbed Groups

### Tab Windows Programmatically

```csharp
// Dock panel2 as a tab to panel1
this.dockingManager1.DockControl(panel2, panel1, 
    DockingStyle.Tabbed, 200);

// Add panel3 as another tab to the same group
this.dockingManager1.DockControl(panel3, panel1, 
    DockingStyle.Tabbed, 200);
```

**VB.NET:**

```vb
' Create tabbed group
Me.dockingManager1.DockControl(panel2, panel1, _
    DockingStyle.Tabbed, 200)
Me.dockingManager1.DockControl(panel3, panel1, _
    DockingStyle.Tabbed, 200)
```

**Result:** Panel1, Panel2, and Panel3 are in the same tab group.

### Create Multiple Tab Groups

```csharp
// Create first tab group on left
this.dockingManager1.DockControl(toolbox, this, 
    DockingStyle.Left, 200);
this.dockingManager1.DockControl(serverExplorer, toolbox, 
    DockingStyle.Tabbed, 200);

// Create second tab group on right
this.dockingManager1.DockControl(properties, this, 
    DockingStyle.Right, 250);
this.dockingManager1.DockControl(events, properties, 
    DockingStyle.Tabbed, 250);
```

### User-Created Tab Groups

Users can create tab groups by dragging:
- Drag window caption over another window
- Drop on the center dock hint
- Windows combine into tabbed group

## Tab Alignment

### Set Tab Position

```csharp
// Place tabs at top (default)
this.dockingManager1.DockTabAlignment = DockTabAlignment.Top;

// Place tabs at bottom
this.dockingManager1.DockTabAlignment = DockTabAlignment.Bottom;

// Place tabs on left (vertical)
this.dockingManager1.DockTabAlignment = DockTabAlignment.Left;

// Place tabs on right (vertical)
this.dockingManager1.DockTabAlignment = DockTabAlignment.Right;
```

**VB.NET:**

```vb
' Set tab alignment
Me.dockingManager1.DockTabAlignment = DockTabAlignment.Bottom
```

This affects ALL tab groups in the DockingManager.

## Tab Reordering

### Enable Tab Movement

```csharp
// Allow users to reorder tabs by dragging
this.dockingManager1.AllowTabsMoving = true;
```

Users can drag tabs left/right within the same group to reorder.

### Reorder Tabs Programmatically

```csharp
// Set tab position (0-based index)
this.dockingManager1.SetTabPosition(panel2, 0); // First tab
this.dockingManager1.SetTabPosition(panel1, 1); // Second tab
this.dockingManager1.SetTabPosition(panel3, 2); // Third tab

// Get current tab position
int position = this.dockingManager1.GetTabPosition(panel2);
```

**VB.NET:**

```vb
' Reorder tabs
Me.dockingManager1.SetTabPosition(panel2, 0)

' Get position
Dim position As Integer = Me.dockingManager1.GetTabPosition(panel2)
```

## Detecting Tabbed State

### Check if Control is Tabbed

```csharp
// Check if control is in a tab group
bool isTabbed = this.dockingManager1.IsTabbed(panel1);

if (isTabbed)
{
    Console.WriteLine("Panel1 is in a tab group");
}
```

### Check if Two Controls Share Tab Group

```csharp
// Check if two controls are in the same tab group
bool sameGroup = this.dockingManager1.IsSameTabbedGroup(panel1, panel2);

if (sameGroup)
{
    Console.WriteLine("Panel1 and Panel2 are in the same tab group");
}
```

**VB.NET:**

```vb
' Check tabbed state
Dim isTabbed As Boolean = Me.dockingManager1.IsTabbed(panel1)

' Check same group
Dim sameGroup As Boolean = Me.dockingManager1.IsSameTabbedGroup(panel1, panel2)
```

## Tab Appearance

### Tab Font and Size

```csharp
// Set tab font
this.dockingManager1.DockTabFont = new Font("Segoe UI", 9f, FontStyle.Regular);

// Set tab height
this.dockingManager1.DockTabHeight = 28; // Default is about 22
```

### Tab Colors

```csharp
// Active tab colors
this.dockingManager1.ActiveDockTabForeColor = Color.White;
this.dockingManager1.ActiveDockTabBackColor = Color.DarkBlue;

// Inactive tab colors
this.dockingManager1.DockTabForeColor = Color.Black;
this.dockingManager1.DockTabBackColor = Color.LightGray;

// Tab panel background
this.dockingManager1.DockTabPanelBackColor = Color.WhiteSmoke;

// Tab separator color
this.dockingManager1.DockTabSeparatorColor = Color.Gray;
```

**VB.NET:**

```vb
' Customize tab colors
Me.dockingManager1.ActiveDockTabForeColor = Color.White
Me.dockingManager1.ActiveDockTabBackColor = Color.DarkBlue
Me.dockingManager1.DockTabForeColor = Color.Black
Me.dockingManager1.DockTabBackColor = Color.LightGray
```

## Tab Scroll Buttons

```csharp
// Show scroll buttons when tabs overflow
this.dockingManager1.ShowDockTabScrollButton = true;
```

When `true`, scroll buttons appear to navigate between tabs that don't fit.

## Preventing Tabbing

### Disable Tabbing for Specific Control

```csharp
// Prevent panel1 from being tabbed
this.dockingManager1.SetDockAbility(panel1, 
    DockAbility.Dockable | DockAbility.Floatable);
// Note: Omit DockAbility.Tabbed to prevent tabbing
```

**VB.NET:**

```vb
' Prevent tabbing
Me.dockingManager1.SetDockAbility(panel1, _
    DockAbility.Dockable Or DockAbility.Floatable)
```

Control can be docked and floated but not tabbed to other windows.

## Tab at Design Time

Enable tabbing at design time for easier layout creation:

```csharp
// Allow tabbing windows in designer
this.dockingManager1.TabAtDesignTime = true;
```

## Complete Example

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class TabbedWindowExample : Form
{
    private DockingManager dockingManager1;
    private Panel toolbox, serverExplorer, teamExplorer;
    private Panel properties, events, document;
    private Button btnCheckGroup, btnReorder;
    
    public TabbedWindowExample()
    {
        InitializeComponent();
        SetupDocking();
        ConfigureTabs();
        SetupControls();
    }
    
    private void SetupDocking()
    {
        // Create DockingManager
        this.dockingManager1 = new DockingManager(this.components);
        this.dockingManager1.HostControl = this;
        
        // Create panels
        toolbox = new Panel { BackColor = Color.LightBlue };
        serverExplorer = new Panel { BackColor = Color.LightCyan };
        teamExplorer = new Panel { BackColor = Color.PaleTurquoise };
        properties = new Panel { BackColor = Color.LightGreen };
        events = new Panel { BackColor = Color.PaleGreen };
        document = new Panel { BackColor = Color.White };
        
        this.Controls.AddRange(new Control[] { 
            toolbox, serverExplorer, teamExplorer,
            properties, events, document 
        });
        
        // Enable docking
        this.dockingManager1.SetEnableDocking(toolbox, true);
        this.dockingManager1.SetEnableDocking(serverExplorer, true);
        this.dockingManager1.SetEnableDocking(teamExplorer, true);
        this.dockingManager1.SetEnableDocking(properties, true);
        this.dockingManager1.SetEnableDocking(events, true);
        this.dockingManager1.SetEnableDocking(document, true);
        
        // Set labels
        this.dockingManager1.SetDockLabel(toolbox, "Toolbox");
        this.dockingManager1.SetDockLabel(serverExplorer, "Server Explorer");
        this.dockingManager1.SetDockLabel(teamExplorer, "Team Explorer");
        this.dockingManager1.SetDockLabel(properties, "Properties");
        this.dockingManager1.SetDockLabel(events, "Events");
        this.dockingManager1.SetDockLabel(document, "Document");
        
        // Create left tab group (Toolbox, Server Explorer, Team Explorer)
        this.dockingManager1.DockControl(toolbox, this, 
            DockingStyle.Left, 220);
        this.dockingManager1.DockControl(serverExplorer, toolbox, 
            DockingStyle.Tabbed, 220);
        this.dockingManager1.DockControl(teamExplorer, toolbox, 
            DockingStyle.Tabbed, 220);
        
        // Create right tab group (Properties, Events)
        this.dockingManager1.DockControl(properties, this, 
            DockingStyle.Right, 250);
        this.dockingManager1.DockControl(events, properties, 
            DockingStyle.Tabbed, 250);
        
        // Create bottom document area
        this.dockingManager1.DockControl(document, this, 
            DockingStyle.Bottom, 200);
    }
    
    private void ConfigureTabs()
    {
        // Tab alignment
        this.dockingManager1.DockTabAlignment = DockTabAlignment.Bottom;
        
        // Enable tab reordering
        this.dockingManager1.AllowTabsMoving = true;
        
        // Enable scroll buttons
        this.dockingManager1.ShowDockTabScrollButton = true;
        
        // Tab appearance
        this.dockingManager1.DockTabFont = 
            new Font("Segoe UI", 9f, FontStyle.Regular);
        this.dockingManager1.DockTabHeight = 26;
        
        // Active tab colors
        this.dockingManager1.ActiveDockTabForeColor = Color.White;
        this.dockingManager1.ActiveDockTabBackColor = Color.FromArgb(0, 122, 204);
        
        // Inactive tab colors
        this.dockingManager1.DockTabForeColor = Color.Black;
        this.dockingManager1.DockTabBackColor = Color.FromArgb(240, 240, 240);
        
        // Tab panel background
        this.dockingManager1.DockTabPanelBackColor = Color.FromArgb(245, 245, 245);
        
        // Separator color
        this.dockingManager1.DockTabSeparatorColor = Color.FromArgb(200, 200, 200);
        
        // Visual style
        this.dockingManager1.VisualStyle = VisualStyle.Office2016Colorful;
        
        // Set initial tab order for left group
        this.dockingManager1.SetTabPosition(serverExplorer, 0); // First
        this.dockingManager1.SetTabPosition(toolbox, 1);        // Second
        this.dockingManager1.SetTabPosition(teamExplorer, 2);   // Third
    }
    
    private void SetupControls()
    {
        // Add control buttons to toolbox
        btnCheckGroup = new Button 
        { 
            Text = "Check Tab Groups", 
            Dock = DockStyle.Top 
        };
        btnCheckGroup.Click += BtnCheckGroup_Click;
        
        btnReorder = new Button 
        { 
            Text = "Reorder Tabs", 
            Dock = DockStyle.Top 
        };
        btnReorder.Click += BtnReorder_Click;
        
        toolbox.Controls.AddRange(new Control[] 
        { 
            btnReorder, btnCheckGroup 
        });
    }
    
    private void BtnCheckGroup_Click(object sender, EventArgs e)
    {
        // Check which controls are tabbed together
        bool toolboxTabbed = this.dockingManager1.IsTabbed(toolbox);
        bool sameAsServer = this.dockingManager1.IsSameTabbedGroup(
            toolbox, serverExplorer);
        bool sameAsProperties = this.dockingManager1.IsSameTabbedGroup(
            toolbox, properties);
        
        int toolboxPosition = this.dockingManager1.GetTabPosition(toolbox);
        
        string message = $"Toolbox Status:\n\n" +
                        $"Is Tabbed: {toolboxTabbed}\n" +
                        $"Same group as Server Explorer: {sameAsServer}\n" +
                        $"Same group as Properties: {sameAsProperties}\n" +
                        $"Tab Position: {toolboxPosition}";
        
        MessageBox.Show(message, "Tab Group Information");
    }
    
    private void BtnReorder_Click(object sender, EventArgs e)
    {
        // Reorder tabs in left group
        // Reverse the order
        this.dockingManager1.SetTabPosition(teamExplorer, 0);
        this.dockingManager1.SetTabPosition(toolbox, 1);
        this.dockingManager1.SetTabPosition(serverExplorer, 2);
        
        MessageBox.Show("Tabs reordered!", "Tab Order");
    }
}
```

## Best Practices

1. **Group related windows** - Tab similar tools together (Properties + Events)
2. **Use meaningful tab order** - Most important tab first
3. **Enable tab reordering** - Let users customize tab order
4. **Set appropriate tab alignment** - Bottom for documents, top for tools
5. **Use scroll buttons** - Essential for many tabs
6. **Customize active tab colors** - Make selected tab obvious
7. **Keep tab labels short** - Long labels cause overflow
8. **Test with many tabs** - Ensure layout works with 5+ tabs

## Troubleshooting

**Cannot create tab group:**
- Verify both controls are dockable
- Check `DockAbility` includes `DockAbility.Tabbed`
- Ensure target control is already docked
- Use `DockingStyle.Tabbed` when calling `DockControl()`

**Tabs appear in wrong position:**
- Set `DockTabAlignment` to desired position (Top/Bottom/Left/Right)
- This affects all tab groups in the DockingManager

**Cannot reorder tabs:**
- Set `AllowTabsMoving` to `true`
- Verify tabs are in the same group with `IsSameTabbedGroup()`

**Tab text is cut off:**
- Increase `DockTabHeight` property
- Use shorter tab labels
- Enable `ShowDockTabScrollButton` for overflow

**Tab colors not changing:**
- Set both `ActiveDockTabBackColor` and `ActiveDockTabForeColor`
- Some visual styles override custom colors
- Try different visual style if colors don't apply
