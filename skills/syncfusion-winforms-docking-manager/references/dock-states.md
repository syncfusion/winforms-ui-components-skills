# Dock States and Layouts

The DockingManager supports multiple dock states that determine how and where controls are displayed. This guide covers all dock states and how to manage them programmatically and through user interaction.

## Table of Contents
- [Dock State Overview](#dock-state-overview)
- [Docked State](#docked-state)
- [Floating State](#floating-state)
- [Auto-Hide State](#auto-hide-state)
- [Tabbed State](#tabbed-state)
- [Fill State](#fill-state)
- [Changing Dock States](#changing-dock-states)
- [Detecting Dock States](#detecting-dock-states)
- [Dock Window Properties](#dock-window-properties)

## Dock State Overview

DockingManager provides five primary dock states:

| Dock State | Description | User Interaction |
|------------|-------------|------------------|
| **Docked** | Attached to form edge (Left, Right, Top, Bottom) | Drag to edge |
| **Floating** | Independent window that can be moved anywhere | Drag away from form |
| **Auto-Hide** | Collapsed to tab, expands on hover | Click pin button |
| **Tabbed** | Grouped with other controls in tabs | Drag onto another control |
| **Fill** | Occupies entire client area | DockToFill property |

## Docked State

Docked windows are attached to the edges of the parent form or another docked control.

### Dock to Form Edges

```csharp
// Dock to left side with width of 200
this.dockingManager1.DockControl(panel1, this, DockingStyle.Left, 200);

// Dock to right side with width of 250
this.dockingManager1.DockControl(panel2, this, DockingStyle.Right, 250);

// Dock to top side with height of 100
this.dockingManager1.DockControl(panel3, this, DockingStyle.Top, 100);

// Dock to bottom side with height of 150
this.dockingManager1.DockControl(panel4, this, DockingStyle.Bottom, 150);
```

**VB.NET:**

```vb
' Dock to sides
Me.dockingManager1.DockControl(panel1, Me, DockingStyle.Left, 200)
Me.dockingManager1.DockControl(panel2, Me, DockingStyle.Right, 250)
Me.dockingManager1.DockControl(panel3, Me, DockingStyle.Top, 100)
Me.dockingManager1.DockControl(panel4, Me, DockingStyle.Bottom, 150)
```

### Dock to Another Window

Controls can be docked relative to other docked controls:

```csharp
// Dock panel2 to the right of panel1
this.dockingManager1.DockControl(panel2, panel1, DockingStyle.Right, 200);

// Dock panel3 to the bottom of panel1
this.dockingManager1.DockControl(panel3, panel1, DockingStyle.Bottom, 100);
```

### Nested Docking

You can create complex layouts by docking controls inside other docked controls:

```csharp
// Create main docked window
this.dockingManager1.DockControl(panel1, this, DockingStyle.Left, 300);

// Dock panel2 inside panel1 (left side)
this.dockingManager1.DockControl(panel2, panel1, DockingStyle.Left, 150);

// Dock panel3 inside panel1 (bottom side)
this.dockingManager1.DockControl(panel3, panel1, DockingStyle.Bottom, 100);
```

## Floating State

Floating windows are independent top-level windows that can be moved anywhere on the screen.

### Float Control Programmatically

```csharp
// Float at specific screen location
Rectangle bounds = new Rectangle(100, 100, 300, 400);
this.dockingManager1.FloatControl(panel1, bounds);

// Float relative to form position
Rectangle formBounds = this.Bounds;
this.dockingManager1.FloatControl(panel2, 
    new Rectangle(formBounds.Right - 300, formBounds.Bottom - 300, 250, 350));
```

### Float by User Interaction

Users can float windows by:
- **Dragging caption** - Click and drag the window caption away from the form
- **Double-clicking caption** - Double-click caption to toggle between docked and floating (if `EnableDoubleClickOnCaption` is true)
- **Context menu** - Right-click caption and select "Floating"

## Auto-Hide State

Auto-hidden windows collapse to a tab on the form edge and expand when the mouse hovers over the tab.

### Set Auto-Hide Mode

```csharp
// Enable auto-hide
this.dockingManager1.SetAutoHideMode(panel1, true);

// Disable auto-hide (return to docked state)
this.dockingManager1.SetAutoHideMode(panel1, false);
```

### Auto-Hide to Specific Side

```csharp
// Auto-hide to top edge with height of 100
this.dockingManager1.DockControlInAutoHideMode(panel1, DockingStyle.Top, 100);

// Auto-hide to left edge with width of 200
this.dockingManager1.DockControlInAutoHideMode(panel2, DockingStyle.Left, 200);

// Auto-hide to right edge
this.dockingManager1.DockControlInAutoHideMode(panel3, DockingStyle.Right, 250);

// Auto-hide to bottom edge
this.dockingManager1.DockControlInAutoHideMode(panel4, DockingStyle.Bottom, 150);
```

### Auto-Hide on Application Load

```csharp
// Set controls to auto-hide when form loads
this.dockingManager1.SetAutoHideOnLoad(panel1, true);
this.dockingManager1.SetAutoHideOnLoad(panel2, true);
```

## Tabbed State

Tabbed windows are grouped together in a tab control, allowing multiple windows to occupy the same space.

### Create Tabbed Group

```csharp
// Tab panel2 with panel1
this.dockingManager1.DockControl(panel2, panel1, DockingStyle.Tabbed, 200);

// Add panel3 to the same tab group
this.dockingManager1.DockControl(panel3, panel1, DockingStyle.Tabbed, 200);

// Add panel4 to the same tab group
this.dockingManager1.DockControl(panel4, panel1, DockingStyle.Tabbed, 200);
```

### Tab Alignment

```csharp
// Set tab position (Bottom is default)
this.dockingManager1.DockTabAlignment = DockTabAlignmentStyle.Bottom;

// Other options:
this.dockingManager1.DockTabAlignment = DockTabAlignmentStyle.Top;
this.dockingManager1.DockTabAlignment = DockTabAlignmentStyle.Left;
this.dockingManager1.DockTabAlignment = DockTabAlignmentStyle.Right;
```

## Fill State

Fill state makes a control occupy the entire client area of the parent form.

### Enable Fill Mode

```csharp
// Enable DockToFill mode
this.dockingManager1.DockToFill = true;

// Dock control with Fill style
this.dockingManager1.DockControl(panel1, this, DockingStyle.Fill, 0);
```

## Changing Dock States

### Using DockControl Method

The `DockControl` method is the primary way to change dock states:

```csharp
// Syntax: DockControl(control, targetControl, dockingStyle, size)

// Dock to form edge
this.dockingManager1.DockControl(panel1, this, DockingStyle.Left, 200);

// Dock to another control
this.dockingManager1.DockControl(panel2, panel1, DockingStyle.Right, 150);

// Tab with another control
this.dockingManager1.DockControl(panel3, panel1, DockingStyle.Tabbed, 0);
```

**Parameters:**
- `control` - The control to dock
- `targetControl` - The target (form or another control)
- `dockingStyle` - Left, Right, Top, Bottom, Tabbed, Fill
- `size` - Width (for Left/Right) or Height (for Top/Bottom)

### Using Context Menu

Users can change dock states using the context menu:
1. Right-click the caption bar
2. Select desired state: "Floating", "Dockable", "Auto-Hide", "Hide"
3. Use "Dock to" submenu to redock at different sides

## Detecting Dock States

### Get Current Dock Style

```csharp
// Get the dock style of a control
DockingStyle style = this.dockingManager1.GetDockStyle(panel1);

switch (style)
{
    case DockingStyle.Left:
        Console.WriteLine("Docked to left");
        break;
    case DockingStyle.Right:
        Console.WriteLine("Docked to right");
        break;
    case DockingStyle.Top:
        Console.WriteLine("Docked to top");
        break;
    case DockingStyle.Bottom:
        Console.WriteLine("Docked to bottom");
        break;
    case DockingStyle.Tabbed:
        Console.WriteLine("Tabbed with another control");
        break;
    case DockingStyle.Fill:
        Console.WriteLine("Filling client area");
        break;
}
```

### Check Specific States

```csharp
// Check if control is floating
bool isFloating = this.dockingManager1.IsFloating(panel1);

// Check if control is auto-hidden
bool isAutoHidden = this.dockingManager1.GetAutoHideMode(panel1);

// Check if control is tabbed
bool isTabbed = this.dockingManager1.IsTabbed(panel1);

// Check if control is in MDI mode
bool isMDI = this.dockingManager1.IsMDIMode(panel1);
```

### Check if Docking is Enabled

```csharp
// Check if control has docking enabled
bool isDockEnabled = this.dockingManager1.GetEnableDocking(panel1);
```

## Dock Window Properties

### Change Window Size

```csharp
// Set size of docked or floating control
this.dockingManager1.SetControlSize(panel1, new Size(300, 400));

// Get size of docked or floating control
Size currentSize = this.dockingManager1.GetControlSize(panel1);
Console.WriteLine($"Width: {currentSize.Width}, Height: {currentSize.Height}");
```

### Set Minimum Size

```csharp
// Prevent resizing below minimum size
this.dockingManager1.SetControlMinimumSize(panel1, new Size(200, 300));
```

### Change Visibility

```csharp
// Hide dock window
this.dockingManager1.SetDockVisibility(panel1, false);

// Show dock window
this.dockingManager1.SetDockVisibility(panel1, true);

// Check visibility
bool isVisible = this.dockingManager1.GetDockVisibility(panel1);
```

### Activate Window

```csharp
// Activate (bring to front) a specific dock window
this.dockingManager1.ActivateControl(panel1);
```

## Dock State Transitions

### Complete Example

```csharp
public class DockStateManager
{
    private DockingManager dockingManager;
    private Panel panel;
    
    // Cycle through dock states
    public void CycleDockStates()
    {
        if (dockingManager.IsFloating(panel))
        {
            // Float -> Docked Left
            dockingManager.DockControl(panel, dockingManager.HostControl, 
                DockingStyle.Left, 200);
        }
        else if (dockingManager.GetDockStyle(panel) == DockingStyle.Left)
        {
            // Docked Left -> Auto-Hide
            dockingManager.SetAutoHideMode(panel, true);
        }
        else if (dockingManager.GetAutoHideMode(panel))
        {
            // Auto-Hide -> Floating
            dockingManager.SetAutoHideMode(panel, false);
            Rectangle bounds = new Rectangle(100, 100, 300, 400);
            dockingManager.FloatControl(panel, bounds);
        }
    }
    
    // Save and restore dock state
    public void SaveRestoreState()
    {
        // Get current state
        DockingStyle currentStyle = dockingManager.GetDockStyle(panel);
        Size currentSize = dockingManager.GetControlSize(panel);
        
        // Later, restore state
        dockingManager.DockControl(panel, dockingManager.HostControl, 
            currentStyle, currentSize.Width);
    }
}
```

## Best Practices

1. **Set dock positions after enabling docking** - Call `SetEnableDocking` before `DockControl`
2. **Specify appropriate sizes** - Provide reasonable width/height values for better UX
3. **Handle state change events** - Use `DockStateChanged` to respond to user actions
4. **Use descriptive labels** - Set clear dock labels with `SetDockLabel`
5. **Consider default state** - Auto-hide panels users might not need immediately
6. **Test with multiple monitors** - Ensure floating windows stay visible
7. **Save layout preferences** - Use serialization to persist user's preferred layout

## Troubleshooting

**Window doesn't dock:**
- Verify `SetEnableDocking` was called with `true`
- Check that the control is added to the form's Controls collection
- Ensure `HostControl` property is set correctly

**Dock position incorrect:**
- Check the size parameter in `DockControl` (width for Left/Right, height for Top/Bottom)
- Verify the target control exists and is docked
- Try different `DockingStyle` values

**Cannot float window:**
- Check `DisallowFloating` property (should be `false`)
- Verify `SetAllowFloating` wasn't called with `false`
- Check if control is set to `FloatOnly` mode
