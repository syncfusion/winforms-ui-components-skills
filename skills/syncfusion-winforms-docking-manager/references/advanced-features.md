# Advanced Features and Configurations

This guide covers advanced DockingManager features including nested docking managers, linked managers, dock restrictions, size constraints, and design-time capabilities.

## Overview

Advanced features enable complex docking scenarios, fine-grained control over docking behavior, and enhanced user experiences in sophisticated applications.

## Nested Docking Managers

### Create Nested Managers

```csharp
// Create parent DockingManager
DockingManager parentManager = new DockingManager(this.components);
parentManager.HostControl = this;

// Create child panel with its own DockingManager
Panel childContainer = new Panel();
childContainer.BackColor = Color.LightGray;

DockingManager childManager = new DockingManager(this.components);
childManager.HostControl = childContainer;

// Enable docking in parent
parentManager.SetEnableDocking(childContainer, true);
parentManager.SetDockLabel(childContainer, "Nested Container");
parentManager.DockControl(childContainer, this, DockingStyle.Left, 300);

// Add dock windows to child manager
Panel nestedPanel1 = new Panel { BackColor = Color.LightBlue };
Panel nestedPanel2 = new Panel { BackColor = Color.LightGreen };

childContainer.Controls.AddRange(new Control[] { nestedPanel1, nestedPanel2 });

childManager.SetEnableDocking(nestedPanel1, true);
childManager.SetEnableDocking(nestedPanel2, true);

childManager.SetDockLabel(nestedPanel1, "Nested Tool 1");
childManager.SetDockLabel(nestedPanel2, "Nested Tool 2");

childManager.DockControl(nestedPanel1, childContainer, DockingStyle.Top, 150);
childManager.DockControl(nestedPanel2, childContainer, DockingStyle.Bottom, 150);
```

**Use case:** Complex layouts with independent docking areas (e.g., plugin containers, MDI children with internal docking).

**VB.NET:**

```vb
' Create nested DockingManager
Dim childContainer As New Panel()
Dim childManager As New DockingManager(Me.components)
childManager.HostControl = childContainer

' Dock container in parent manager
parentManager.SetEnableDocking(childContainer, True)
parentManager.DockControl(childContainer, Me, DockingStyle.Left, 300)
```

## Linked Docking Managers

### Enable Manager Linking

```csharp
// Create two separate forms with DockingManagers
Form form1 = new Form();
DockingManager manager1 = new DockingManager();
manager1.HostControl = form1;

Form form2 = new Form();
DockingManager manager2 = new DockingManager();
manager2.HostControl = form2;

// Enable linking to allow dragging controls between managers
manager1.EnableLinkedManager = true;
manager2.EnableLinkedManager = true;

// Handle transfer events
manager1.TransferredToManager += (s, e) =>
{
    Console.WriteLine($"Control transferred TO manager1: {e.Control.Name}");
};

manager1.TransferringFromManager += (s, e) =>
{
    Console.WriteLine($"Control transferring FROM manager1: {e.Control.Name}");
};
```

**Use case:** Multi-window applications where users can drag dock windows between forms.

## Dock Ability Restrictions

### SetDockAbility - Inner Docking

```csharp
// Allow only specific docking positions
this.dockingManager1.SetDockAbility(panel1, 
    DockAbility.Left | DockAbility.Right);

// Panel1 can only dock to left or right sides
```

**DockAbility Flags:**
- `DockAbility.Left` - Can dock to left side
- `DockAbility.Right` - Can dock to right side
- `DockAbility.Top` - Can dock to top side
- `DockAbility.Bottom` - Can dock to bottom side
- `DockAbility.Tabbed` - Can be tabbed with other windows
- `DockAbility.Floatable` - Can float
- `DockAbility.Fill` - Can fill center area
- `DockAbility.Dockable` - Shortcut for Left | Right | Top | Bottom

**VB.NET:**

```vb
' Restrict docking to left and right only
Me.dockingManager1.SetDockAbility(panel1, _
    DockAbility.Left Or DockAbility.Right)
```

### Common Dock Ability Combinations

```csharp
// Allow all docking (default)
this.dockingManager1.SetDockAbility(panel1, 
    DockAbility.Dockable | DockAbility.Floatable | DockAbility.Tabbed);

// Dock only, no floating or tabbing
this.dockingManager1.SetDockAbility(panel2, 
    DockAbility.Dockable);

// Float only (cannot be docked)
this.dockingManager1.SetDockAbility(panel3, 
    DockAbility.Floatable);

// Vertical docking only (left or right)
this.dockingManager1.SetDockAbility(panel4, 
    DockAbility.Left | DockAbility.Right | DockAbility.Floatable);

// Horizontal docking only (top or bottom)
this.dockingManager1.SetDockAbility(panel5, 
    DockAbility.Top | DockAbility.Bottom | DockAbility.Floatable);

// Cannot float
this.dockingManager1.SetDockAbility(panel6, 
    DockAbility.Dockable | DockAbility.Tabbed);
```

### Get Current Dock Ability

```csharp
// Get current dock ability settings
DockAbility ability = this.dockingManager1.GetDockAbility(panel1);

if ((ability & DockAbility.Floatable) == DockAbility.Floatable)
{
    Console.WriteLine("Panel1 can float");
}

if ((ability & DockAbility.Left) == DockAbility.Left)
{
    Console.WriteLine("Panel1 can dock to left");
}
```

### SetOuterDockAbility - Outer Docking

```csharp
// Control outer docking (relative to main form edges)
this.dockingManager1.SetOuterDockAbility(panel1, 
    DockAbility.Left | DockAbility.Top);

// Panel1 can only dock to outer left or top edges
```

**Difference between SetDockAbility and SetOuterDockAbility:**
- `SetDockAbility` - Controls docking relative to other windows (inner)
- `SetOuterDockAbility` - Controls docking relative to main form edges (outer)

### Get Outer Dock Ability

```csharp
DockAbility outerAbility = this.dockingManager1.GetOuterDockAbility(panel1);
```

## Size Constraints

### Set Minimum Size

```csharp
// Set minimum size for docked window
this.dockingManager1.SetControlMinimumSize(panel1, new Size(150, 100));
```

Window cannot be resized smaller than this size.

**VB.NET:**

```vb
' Set minimum size
Me.dockingManager1.SetControlMinimumSize(panel1, New Size(150, 100))
```

### Freeze Resizing

```csharp
// Prevent resizing all docked windows
this.dockingManager1.FreezeResizing = true;
```

Users cannot resize any dock windows when `true`. Splitters are disabled.

### Set Control Size

```csharp
// Set size for specific docked control
this.dockingManager1.SetControlSize(panel1, new Size(250, 400));

// Get current size
Size currentSize = this.dockingManager1.GetControlSize(panel1);
```

## Fill Mode

### Dock to Fill Center

```csharp
// Enable fill mode for DockingManager
this.dockingManager1.DockToFill = true;

// Dock control to fill center area
this.dockingManager1.SetEnableDocking(centerPanel, true);
this.dockingManager1.DockControl(centerPanel, this, DockingStyle.Fill, 200);
```

The control fills the remaining center space after other windows are docked.

**Use case:** Main content area, document viewer, canvas.

## Caption Management

### Hide All Captions

```csharp
// Hide captions for all dock windows
this.dockingManager1.ShowCaption = false;
```

### Hide Caption for Specific Control

```csharp
// Hide caption for specific control
this.dockingManager1.SetCaptionVisibility(panel1, false);

// Show caption again
this.dockingManager1.SetCaptionVisibility(panel1, true);
```

### Caption Text Alignment

```csharp
// Set caption label alignment
this.dockingManager1.DockLabelAlignment = DockLabelAlignment.Center;

// Options: Left (default), Center, Right
```

**VB.NET:**

```vb
' Center-align caption text
Me.dockingManager1.DockLabelAlignment = DockLabelAlignment.Center
```

## Context Menu Management

### Disable Context Menu

```csharp
// Disable context menu for all windows
this.dockingManager1.EnableContextMenu = false;
```

### Customize Context Menu

```csharp
using Syncfusion.Windows.Forms.Tools.XPMenus;

// Add/remove context menu items
this.dockingManager1.DockContextMenu += (s, e) =>
{
    // Remove "Close" menu item
    for (int i = e.ContextMenu.ParentBarItem.Items.Count - 1; i >= 0; i--)
    {
        BarItem item = e.ContextMenu.ParentBarItem.Items[i] as BarItem;
        if (item != null && item.Text == "&Close")
        {
            e.ContextMenu.ParentBarItem.Items.RemoveAt(i);
        }
    }
    
    // Add custom items
    BarItem customItem = new BarItem { Text = "Lock Window" };
    customItem.Click += (sender, args) =>
    {
        // Lock window logic
        this.dockingManager1.SetDockAbility(e.Owner, DockAbility.None);
    };
    e.ContextMenu.ParentBarItem.Items.Add(customItem);
};
```

**VB.NET:**

```vb
Private Sub DockingManager1_DockContextMenu(sender As Object, _
    e As DockContextMenuEventArgs) Handles dockingManager1.DockContextMenu
    
    ' Add custom menu item
    Dim customItem As New BarItem With {.Text = "Custom Action"}
    e.ContextMenu.ParentBarItem.Items.Add(customItem)
End Sub
```

## Splitter Customization

### Splitter Width

```csharp
// Set width of splitters between docked windows
this.dockingManager1.SplitterWidth = 6; // Default is 4
```

### Metro Splitter Color

```csharp
// Set splitter color for Metro style
this.dockingManager1.VisualStyle = VisualStyle.Metro;
this.dockingManager1.MetroSplitterBackColor = Color.DarkGray;
```

## Maximize/Restore Functionality

### Enable Maximize Button

```csharp
// Enable maximize/restore button on dock windows
this.dockingManager1.MaximizeButtonEnabled = true;
```

When enabled:
- Maximize button appears in caption
- Click to maximize window (fills entire docking area)
- Click again to restore original size
- Only appears when another control is docked below

### Handle Maximize Events

```csharp
this.dockingManager1.ControlMaximizing += (s, e) =>
{
    Console.WriteLine($"Maximizing: {e.Control.Name}");
};

this.dockingManager1.ControlMaximized += (s, e) =>
{
    // Window is now maximized
    // Update UI or application state
};

this.dockingManager1.ControlRestored += (s, e) =>
{
    // Window restored to normal size
};
```

## Design-Time Features

### Enable Drag at Design Time

```csharp
// Allow dragging dock windows in designer
this.dockingManager1.DragAtDesignTime = true;
```

Enables repositioning dock windows directly in the Visual Studio designer.

### Enable Tab at Design Time

```csharp
// Allow creating tab groups in designer
this.dockingManager1.TabAtDesignTime = true;
```

### Smart Tags

DockingManager provides Smart Tags in the designer for quick configuration:
- Enable docking for controls
- Set dock labels
- Arrange windows
- Apply visual styles

Access Smart Tags: Click the smart tag glyph (▶) on the DockingManager component.

## Localization

### Localize Context Menu

```csharp
// Use resource files for localization
// Create resx files: DockingManager.resx, DockingManager.fr-FR.resx, etc.

// Set culture
System.Threading.Thread.CurrentThread.CurrentUICulture = 
    new System.Globalization.CultureInfo("fr-FR");

// DockingManager automatically loads localized resources
```

Resource strings to localize:
- `Dockable` - "Ancrable"
- `Floating` - "Flottant"
- `AutoHide` - "Masquer automatiquement"
- `Close` - "Fermer"
- `Hide` - "Masquer"

## Multi-Instance Applications

### Provide Persistence ID

```csharp
// For applications that run multiple instances
this.dockingManager1.ProvidePersistenceID += (s, e) =>
{
    // Unique ID for this application instance
    e.PersistenceID = $"Instance_{Process.GetCurrentProcess().Id}";
};
```

Each instance saves its layout separately using the unique ID.

## Complete Advanced Example

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Windows.Forms.Tools.XPMenus;

public class AdvancedExample : Form
{
    private DockingManager dockingManager1;
    private Panel leftPanel, rightPanel, centerPanel, floatingPanel;
    
    public AdvancedExample()
    {
        InitializeComponent();
        SetupAdvancedDocking();
    }
    
    private void SetupAdvancedDocking()
    {
        // Create DockingManager
        this.dockingManager1 = new DockingManager(this.components);
        this.dockingManager1.HostControl = this;
        
        // Enable advanced features
        this.dockingManager1.MaximizeButtonEnabled = true;
        this.dockingManager1.DockToFill = true;
        this.dockingManager1.EnableContextMenu = true;
        
        // Visual configuration
        this.dockingManager1.VisualStyle = VisualStyle.Metro;
        this.dockingManager1.SplitterWidth = 5;
        this.dockingManager1.MetroSplitterBackColor = Color.Gray;
        
        // Create panels
        leftPanel = new Panel { BackColor = Color.LightBlue };
        rightPanel = new Panel { BackColor = Color.LightGreen };
        centerPanel = new Panel { BackColor = Color.White };
        floatingPanel = new Panel { BackColor = Color.LightYellow };
        
        this.Controls.AddRange(new Control[] 
        { 
            leftPanel, rightPanel, centerPanel, floatingPanel 
        });
        
        // Enable docking
        this.dockingManager1.SetEnableDocking(leftPanel, true);
        this.dockingManager1.SetEnableDocking(rightPanel, true);
        this.dockingManager1.SetEnableDocking(centerPanel, true);
        this.dockingManager1.SetEnableDocking(floatingPanel, true);
        
        // Set labels
        this.dockingManager1.SetDockLabel(leftPanel, "Tools");
        this.dockingManager1.SetDockLabel(rightPanel, "Properties");
        this.dockingManager1.SetDockLabel(centerPanel, "Canvas");
        this.dockingManager1.SetDockLabel(floatingPanel, "Floating Palette");
        
        // Configure dock abilities
        // Tools: Can dock left/right, can float, cannot be tabbed
        this.dockingManager1.SetDockAbility(leftPanel, 
            DockAbility.Left | DockAbility.Right | DockAbility.Floatable);
        
        // Properties: Can dock anywhere, can tab, can float
        this.dockingManager1.SetDockAbility(rightPanel, 
            DockAbility.Dockable | DockAbility.Tabbed | DockAbility.Floatable);
        
        // Center: Fill only, cannot float or dock elsewhere
        this.dockingManager1.SetDockAbility(centerPanel, 
            DockAbility.Fill);
        
        // Floating: Float only, cannot be docked
        this.dockingManager1.SetFloatOnly(floatingPanel, true);
        
        // Set size constraints
        this.dockingManager1.SetControlMinimumSize(leftPanel, new Size(150, 200));
        this.dockingManager1.SetControlMinimumSize(rightPanel, new Size(200, 150));
        
        // Arrange windows
        this.dockingManager1.DockControl(leftPanel, this, 
            DockingStyle.Left, 250);
        this.dockingManager1.DockControl(rightPanel, this, 
            DockingStyle.Right, 300);
        this.dockingManager1.DockControl(centerPanel, this, 
            DockingStyle.Fill, 200);
        
        // Float the palette
        this.dockingManager1.FloatControl(floatingPanel, 
            new Rectangle(100, 100, 200, 300));
        
        // Customize context menu
        this.dockingManager1.DockContextMenu += DockingManager1_DockContextMenu;
        
        // Handle maximize events
        this.dockingManager1.ControlMaximized += 
            DockingManager1_ControlMaximized;
        this.dockingManager1.ControlRestored += 
            DockingManager1_ControlRestored;
    }
    
    private void DockingManager1_DockContextMenu(object sender, 
        DockContextMenuEventArgs e)
    {
        // Add custom menu items
        BarItem lockItem = new BarItem { Text = "Lock Position" };
        lockItem.Click += (s, args) =>
        {
            // Lock window in place
            this.dockingManager1.SetDockAbility(e.Owner, DockAbility.None);
            MessageBox.Show("Window position locked!");
        };
        
        BarItem unlockItem = new BarItem { Text = "Unlock Position" };
        unlockItem.Click += (s, args) =>
        {
            // Unlock window
            this.dockingManager1.SetDockAbility(e.Owner, 
                DockAbility.Dockable | DockAbility.Floatable | DockAbility.Tabbed);
            MessageBox.Show("Window position unlocked!");
        };
        
        BarItem separator = new BarItem { BarItemType = BarItemType.Sep };
        
        e.ContextMenu.ParentBarItem.Items.Insert(0, lockItem);
        e.ContextMenu.ParentBarItem.Items.Insert(1, unlockItem);
        e.ContextMenu.ParentBarItem.Items.Insert(2, separator);
    }
    
    private void DockingManager1_ControlMaximized(object sender, 
        ControlMaximizedEventArgs e)
    {
        string label = this.dockingManager1.GetDockLabel(e.Control);
        this.Text = $"Advanced Example - {label} Maximized";
    }
    
    private void DockingManager1_ControlRestored(object sender, 
        ControlRestoredEventArgs e)
    {
        this.Text = "Advanced Example";
    }
}
```

**VB.NET Example:**

```vb
Private Sub SetupAdvancedDocking()
    ' Create DockingManager
    Me.dockingManager1 = New DockingManager(Me.components)
    Me.dockingManager1.HostControl = Me
    
    ' Enable features
    Me.dockingManager1.MaximizeButtonEnabled = True
    Me.dockingManager1.DockToFill = True
    
    ' Enable docking
    Me.dockingManager1.SetEnableDocking(panel1, True)
    Me.dockingManager1.SetDockLabel(panel1, "Tools")
    
    ' Restrict docking
    Me.dockingManager1.SetDockAbility(panel1, _
        DockAbility.Left Or DockAbility.Right Or DockAbility.Floatable)
    
    ' Set minimum size
    Me.dockingManager1.SetControlMinimumSize(panel1, New Size(150, 200))
    
    ' Dock
    Me.dockingManager1.DockControl(panel1, Me, DockingStyle.Left, 250)
End Sub
```

## Best Practices

1. **Use dock restrictions wisely** - Guide users to logical docking positions
2. **Set minimum sizes** - Prevent windows from becoming unusably small
3. **Enable maximize for large content** - Let users focus on specific windows
4. **Customize context menus** - Add application-specific actions
5. **Test nested managers** - Ensure proper event bubbling and behavior
6. **Use linked managers carefully** - Can confuse users if not intuitive
7. **Provide reset functionality** - Let users restore default layout
8. **Consider accessibility** - Ensure all features are keyboard accessible

## Troubleshooting

**Cannot dock to specific side:**
- Check `DockAbility` includes the desired side
- Verify `SetOuterDockAbility` allows the position
- Ensure target window accepts docking there

**Maximize button doesn't appear:**
- Set `MaximizeButtonEnabled` to `true`
- Requires another control docked below the window
- Check visual style supports maximize button

**Context menu customization not working:**
- Subscribe to `DockContextMenu` event before showing menu
- Use correct BarItem type (not MenuItem)
- Insert at correct index in Items collection

**Nested manager issues:**
- Ensure child manager's HostControl is set to container panel
- Child container must be docked in parent manager
- Events don't bubble between managers automatically

**Float-only control can be docked:**
- Call `SetFloatOnly(control, true)` AFTER docking
- Or set `DockAbility.Floatable` only (no Dockable)
- Verify control is actually floating after setup
