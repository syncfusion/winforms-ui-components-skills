# Document Windows (TDI/MDI)

The DockingManager supports two document window modes: TDI (Tabbed Document Interface) and MDI (Multiple Document Interface). This guide covers both modes and their configuration.

## Table of Contents
- [Document Mode Overview](#document-mode-overview)
- [TDI (Tabbed Document Interface)](#tdi-tabbed-document-interface)
- [MDI (Multiple Document Interface)](#mdi-multiple-document-interface)
- [Window Modes](#window-modes)
- [Document Tab Groups](#document-tab-groups)
- [Document Tab Customization](#document-tab-customization)
- [Freeze to Document State](#freeze-to-document-state)

## Document Mode Overview

**TDI (Tabbed Document Interface):**
- Multiple documents displayed as tabs in a central area
- Similar to modern browsers or Visual Studio code editor
- Documents share the same space, only one visible at a time

**MDI (Multiple Document Interface):**
- Multiple document windows within a parent window
- Each document can be independently moved, resized, minimized
- Documents can overlap and be arranged (cascade, tile)

## TDI (Tabbed Document Interface)

### Enable Document Mode

```csharp
// Enable TDI mode
this.dockingManager1.EnableDocumentMode = true;
```

### Adding Document Tabs

Documents should be added in the `NewDockStateEndLoad` event to ensure the dock layout is ready:

```csharp
public Form1()
{
    InitializeComponent();
    
    // Enable document mode
    this.dockingManager1.EnableDocumentMode = true;
    
    // Subscribe to event
    this.dockingManager1.NewDockStateEndLoad += DockingManager1_NewDockStateEndLoad;
}

private void DockingManager1_NewDockStateEndLoad(object sender, EventArgs e)
{
    // Add document tabs
    this.dockingManager1.DockAsDocument(this.panel1);
    this.dockingManager1.DockAsDocument(this.panel2);
    this.dockingManager1.DockAsDocument(this.panel3);
}
```

**VB.NET:**

```vb
Public Sub New()
    InitializeComponent()
    
    ' Enable document mode
    Me.dockingManager1.EnableDocumentMode = True
    
    ' Subscribe to event
    AddHandler Me.dockingManager1.NewDockStateEndLoad, AddressOf DockingManager1_NewDockStateEndLoad
End Sub

Private Sub DockingManager1_NewDockStateEndLoad(sender As Object, e As EventArgs)
    ' Add document tabs
    Me.dockingManager1.DockAsDocument(Me.panel1)
    Me.dockingManager1.DockAsDocument(Me.panel2)
    Me.dockingManager1.DockAsDocument(Me.panel3)
End Sub
```

### Document Tab Features

**Show/Hide Close Button:**

```csharp
// Show common close button for all tabs
this.dockingManager1.DocumentWindowSettings.ShowCloseButton = true;
```

**Show/Hide Tab List Menu:**

```csharp
// Show menu button with list of all document tabs
this.dockingManager1.DocumentWindowSettings.ShowTabList = true;
```

**Close Tab on Middle Click:**

```csharp
// Enable closing tabs with middle mouse button
this.dockingManager1.CloseTabOnMiddleClick = true;
```

**Enable/Disable Tab Dragging:**

```csharp
// Allow dragging document tabs
this.dockingManager1.DocumentWindowSettings.AllowDragging = true;

// Disable dragging
this.dockingManager1.DocumentWindowSettings.AllowDragging = false;
```

**Enable/Disable Tab Reordering:**

```csharp
// Allow reordering tabs by drag-drop
this.dockingManager1.AllowTabsMoving = true;
```

## MDI (Multiple Document Interface)

### Enable MDI Container

First, set the form as an MDI container:

```csharp
public Form1()
{
    InitializeComponent();
    
    // Enable MDI functionality
    this.IsMdiContainer = true;
    
    // Create DockingManager
    this.dockingManager1 = new DockingManager(this.components);
    this.dockingManager1.HostControl = this;
}
```

### Convert Dock Window to MDI Child

```csharp
// Set docked control as MDI child
this.dockingManager1.SetAsMDIChild(panel1, true);
this.dockingManager1.SetAsMDIChild(panel2, true);

// Remove MDI child status (back to dock window)
this.dockingManager1.SetAsMDIChild(panel1, false);
```

**Via Context Menu:**
Users can convert windows using the "MDI Child" option in the context menu.

### MDI Child with Custom Size

```csharp
// Set as MDI child with specific size and position
this.dockingManager1.SetAsMDIChild(panel1, true, 
    new Rectangle(100, 100, 400, 300));
```

### Add Icon to MDI Child Caption

```csharp
// Load icon
System.Drawing.Icon icon = new System.Drawing.Icon(@"C:\Icons\document.ico");

// Set icon for MDI child
this.dockingManager1.SetMDIChildIcon(panel1, icon);
```

### Check if Control is MDI Mode

```csharp
bool isMDI = this.dockingManager1.IsMDIMode(panel1);
if (isMDI)
{
    Console.WriteLine("Control is in MDI mode");
}
```

### Tabbed MDI Using TabbedMDIManager

For tabbed MDI child windows (similar to TDI but using MDI infrastructure):

```csharp
using Syncfusion.Windows.Forms.Tools;

public Form1()
{
    InitializeComponent();
    
    // Enable MDI container
    this.IsMdiContainer = true;
    
    // Create TabbedMDIManager
    TabbedMDIManager tabbedMDI = new TabbedMDIManager();
    tabbedMDI.AttachToMdiContainer(this);
    tabbedMDI.TabStyle = typeof(TabRendererOffice2016Colorful);
    
    // Set controls as MDI children
    this.dockingManager1.SetAsMDIChild(panel1, true);
    this.dockingManager1.SetAsMDIChild(panel2, true);
}
```

### Office 2007 Style MDI Windows

```csharp
// Enable Office 2007 style for MDI children
this.dockingManager1.Office2007MdiChildForm = true;

// Set color scheme
this.dockingManager1.Office2007MdiColorScheme = Office2007Theme.Silver;
// Options: Blue, Black, Silver, Managed
```

## Window Modes

Window mode determines the dockability behavior of a window.

### Tool Window Mode

Tool windows can be docked, floated, auto-hidden, or converted to MDI/TDI:

```csharp
// Set as tool window (default mode)
this.dockingManager1.SetWindowMode(panel1, WindowMode.Tool);
```

**Tool window can:**
- Dock to any side
- Float anywhere
- Auto-hide
- Tab with other windows
- Convert to MDI child or document tab

### Document Window Mode

Document windows can only float or remain as document tabs:

```csharp
// Set as document window
this.dockingManager1.SetWindowMode(panel2, WindowMode.Document);
```

**Document window can:**
- Float (become floating window)
- Remain as document tab
- Cannot dock to form edges
- Cannot auto-hide

### Get Window Mode

```csharp
WindowMode mode = this.dockingManager1.GetWindowMode(panel1);

if (mode == WindowMode.Tool)
{
    Console.WriteLine("Tool window - full docking capabilities");
}
else if (mode == WindowMode.Document)
{
    Console.WriteLine("Document window - limited to float/document state");
}
```

## Document Tab Groups

Create horizontal or vertical tab groups in the document area, similar to Visual Studio.

### Create Tab Group via Context Menu

1. Right-click a document tab
2. Select "New Horizontal Tab Group" or "New Vertical Tab Group"
3. The document splits into two tab groups

### Create Tab Group via Drag Hints

When dragging a document tab, dock hints appear:
- **Center hint** - Tab with existing documents
- **Left/Right hints** - Create vertical tab group
- **Top/Bottom hints** - Create horizontal tab group

### Enable/Disable Tab Groups

```csharp
// Enable tab group creation
this.dockingManager1.DocumentWindowSettings.EnableTabGroup = true;

// Disable tab group creation
this.dockingManager1.DocumentWindowSettings.EnableTabGroup = false;
```

### Tab Group Events

**TabGroupCreating Event:**

```csharp
this.dockingManager1.TabGroupCreating += (s, e) =>
{
    // Cancel tab group creation for horizontal orientation
    if (e.Orientation == Orientation.Horizontal)
    {
        e.Cancel = true;
    }
    
    Console.WriteLine($"Creating {e.Orientation} tab group");
    Console.WriteLine($"Target item: {e.TargetItem.Text}");
};
```

**TabGroupCreated Event:**

```csharp
this.dockingManager1.TabGroupCreated += (s, e) =>
{
    // Customize newly created tab groups
    e.CurrentTabGroup.ActiveTabColor = Color.Purple;
    e.PreviousTabGroup.ActiveTabColor = Color.Green;
    
    Console.WriteLine($"Tab groups count: {e.TabGroups.Count}");
    Console.WriteLine($"Orientation: {e.Orientation}");
};
```

## Document Tab Customization

### Tab Height

```csharp
// Set document tab height (max 60)
this.dockingManager1.DocumentWindowSettings.TabHeight = 38;
```

### Tab Colors

```csharp
// Inactive tab background
this.dockingManager1.DocumentWindowSettings.TabBackColor = Color.SteelBlue;

// Active tab background
this.dockingManager1.DocumentWindowSettings.ActiveTabBackColor = Color.Green;

// Inactive tab foreground
this.dockingManager1.DocumentWindowSettings.TabForeColor = Color.White;

// Active tab foreground
this.dockingManager1.DocumentWindowSettings.ActiveTabForeColor = Color.Yellow;

// Tab panel background
this.dockingManager1.DocumentWindowSettings.TabPanelBackColor = Color.Purple;

// Tab panel border color
this.dockingManager1.DocumentWindowSettings.TabPanelBorderColor = Color.Green;
```

### Tab Fonts

```csharp
// Inactive tab font
this.dockingManager1.DocumentWindowSettings.TabFont = 
    new Font("Arial", 9, FontStyle.Italic);

// Active tab font
this.dockingManager1.DocumentWindowSettings.ActiveTabFont = 
    new Font("Arial", 9, FontStyle.Bold);
```

## Freeze to Document State

Prevent document windows from being moved to other states:

```csharp
// Freeze to document state (cannot dock, float, or auto-hide)
this.dockingManager1.FreezeToDocumentState(panel1, true);

// Unfreeze
this.dockingManager1.FreezeToDocumentState(panel1, false);

// Check if frozen
bool isFrozen = this.dockingManager1.IsFrozenToDocumentState(panel1);
```

## Complete TDI Example

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class Form1 : Form
{
    private DockingManager dockingManager1;
    private Panel document1, document2, document3;
    private Panel toolPanel1, toolPanel2;
    
    public Form1()
    {
        InitializeComponent();
        InitializeDocumentInterface();
    }
    
    private void InitializeDocumentInterface()
    {
        // Create DockingManager
        this.dockingManager1 = new DockingManager(this.components);
        this.dockingManager1.HostControl = this;
        
        // Enable document mode
        this.dockingManager1.EnableDocumentMode = true;
        
        // Create panels
        document1 = new Panel { BackColor = Color.White };
        document2 = new Panel { BackColor = Color.WhiteSmoke };
        document3 = new Panel { BackColor = Color.Linen };
        toolPanel1 = new Panel { BackColor = Color.LightBlue };
        toolPanel2 = new Panel { BackColor = Color.LightGreen };
        
        // Add to form
        this.Controls.AddRange(new Control[] {
            document1, document2, document3, toolPanel1, toolPanel2
        });
        
        // Enable docking
        this.dockingManager1.SetEnableDocking(document1, true);
        this.dockingManager1.SetEnableDocking(document2, true);
        this.dockingManager1.SetEnableDocking(document3, true);
        this.dockingManager1.SetEnableDocking(toolPanel1, true);
        this.dockingManager1.SetEnableDocking(toolPanel2, true);
        
        // Set labels
        this.dockingManager1.SetDockLabel(document1, "Document1.txt");
        this.dockingManager1.SetDockLabel(document2, "Document2.cs");
        this.dockingManager1.SetDockLabel(document3, "Document3.xml");
        this.dockingManager1.SetDockLabel(toolPanel1, "Solution Explorer");
        this.dockingManager1.SetDockLabel(toolPanel2, "Properties");
        
        // Set window modes
        this.dockingManager1.SetWindowMode(document1, WindowMode.Document);
        this.dockingManager1.SetWindowMode(document2, WindowMode.Document);
        this.dockingManager1.SetWindowMode(document3, WindowMode.Document);
        this.dockingManager1.SetWindowMode(toolPanel1, WindowMode.Tool);
        this.dockingManager1.SetWindowMode(toolPanel2, WindowMode.Tool);
        
        // Add document tabs
        this.dockingManager1.NewDockStateEndLoad += (s, e) =>
        {
            this.dockingManager1.DockAsDocument(document1);
            this.dockingManager1.DockAsDocument(document2);
            this.dockingManager1.DockAsDocument(document3);
        };
        
        // Dock tool windows
        this.dockingManager1.DockControl(toolPanel1, this, 
            DockingStyle.Right, 250);
        this.dockingManager1.DockControl(toolPanel2, toolPanel1, 
            DockingStyle.Tabbed, 200);
        
        // Configure document settings
        this.dockingManager1.DocumentWindowSettings.ShowCloseButton = true;
        this.dockingManager1.DocumentWindowSettings.ShowTabList = true;
        this.dockingManager1.DocumentWindowSettings.EnableTabGroup = true;
        this.dockingManager1.CloseTabOnMiddleClick = true;
        
        // Apply visual style
        this.dockingManager1.VisualStyle = VisualStyle.Office2016Colorful;
    }
}
```

## Best Practices

1. **Add documents in NewDockStateEndLoad** - Ensures dock layout is fully initialized
2. **Set appropriate window modes** - Use Document mode for documents, Tool mode for panels
3. **Enable tab groups** - Allows users to view multiple documents side-by-side
4. **Provide close buttons** - Let users close document tabs easily
5. **Use descriptive tab labels** - Include file names or meaningful titles
6. **Consider freezing document state** - Prevent accidental undocking of documents
7. **Test with many documents** - Ensure tab scrolling and navigation work well

## Troubleshooting

**Documents don't appear as tabs:**
- Verify `EnableDocumentMode` is `true`
- Call `DockAsDocument` in `NewDockStateEndLoad` event
- Ensure controls are added to form and docking is enabled

**Cannot create tab groups:**
- Check `DocumentWindowSettings.EnableTabGroup` is `true`
- Verify drag provider style supports tab groups (not VS2005, VS2008, Whidbey)
- Ensure there's at least one active document

**MDI child doesn't work:**
- Set form's `IsMdiContainer` property to `true`
- Call `SetAsMDIChild` after docking is enabled
- Check that DockingManager's HostControl is set correctly
