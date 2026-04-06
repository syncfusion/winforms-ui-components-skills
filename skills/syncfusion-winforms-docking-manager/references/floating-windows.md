# Floating Windows

Floating windows are independent movable windows that can be repositioned anywhere on the screen. This guide covers creating, controlling, and customizing floating windows.

## Overview

**What:** Dock windows that float independently outside the main form.

**When to use:**
- Temporary or occasional tools
- Multi-monitor workflows
- User needs flexible positioning
- Context-specific panels

**How:** Use `FloatControl()` method or drag by double-clicking caption.

## Creating Floating Windows

### Float Programmatically

```csharp
// Float a control at specific location
Rectangle floatRect = new Rectangle(100, 100, 300, 200); // X, Y, Width, Height
this.dockingManager1.FloatControl(panel1, floatRect);
```

**VB.NET:**

```vb
' Float a control
Dim floatRect As New Rectangle(100, 100, 300, 200)
Me.dockingManager1.FloatControl(panel1, floatRect)
```

### Float at Specific Screen Position

```csharp
// Float at top-right of screen
Rectangle screenBounds = Screen.PrimaryScreen.WorkingArea;
Rectangle floatRect = new Rectangle(
    screenBounds.Right - 400,  // X position
    screenBounds.Top + 100,     // Y position
    350,                        // Width
    250                         // Height
);
this.dockingManager1.FloatControl(panel1, floatRect);
```

### Float on Secondary Monitor

```csharp
// Float on second monitor if available
if (Screen.AllScreens.Length > 1)
{
    Screen secondScreen = Screen.AllScreens[1];
    Rectangle floatRect = new Rectangle(
        secondScreen.WorkingArea.Left + 50,
        secondScreen.WorkingArea.Top + 50,
        400, 300
    );
    this.dockingManager1.FloatControl(panel1, floatRect);
}
```

### Float with Default Size

```csharp
// Float using control's current size
Size currentSize = panel1.Size;
Point floatLocation = new Point(200, 200);
Rectangle floatRect = new Rectangle(floatLocation, currentSize);
this.dockingManager1.FloatControl(panel1, floatRect);
```

## User-Initiated Floating

### Enable Double-Click to Float

```csharp
// Allow users to float windows by double-clicking caption
this.dockingManager1.EnableDoubleClickOnCaption = true;
```

When enabled, users can:
- **Double-click caption** - Float a docked window
- **Double-click caption** of floating window - Re-dock to last position

### Drag to Float

Users can drag dock windows to float them:
- Click and drag the caption bar
- Window detaches and becomes floating
- Release to position the floating window

Configure drag behavior:

```csharp
// Set drag provider style
this.dockingManager1.DragProviderStyle = DragProviderStyle.VS2012;

// Options: Standard, VS2005, VS2008, VS2010, VS2012, 
//          Office2007, Office2016, Whidbey
```

## Restricting Floating

### Disable Floating for All Windows

```csharp
// Prevent all windows from floating
this.dockingManager1.DisallowFloating = true;
```

Users cannot float windows by dragging or double-clicking.

### Disable Floating for Specific Control

```csharp
// Allow most controls to float
this.dockingManager1.DisallowFloating = false;

// But prevent panel1 from floating
this.dockingManager1.SetDockAbility(panel1, 
    DockAbility.Dockable | DockAbility.Tabbed);
// Note: Omitting DockAbility.Floatable prevents floating
```

**VB.NET:**

```vb
' Prevent specific control from floating
Me.dockingManager1.SetDockAbility(panel1, _
    DockAbility.Dockable Or DockAbility.Tabbed)
```

### Allow Floating Only (Float-Only Mode)

```csharp
// Make a control float-only (cannot be docked)
this.dockingManager1.SetFloatOnly(panel1, true);

// Float the control
this.dockingManager1.FloatControl(panel1, new Rectangle(100, 100, 300, 200));
```

When `SetFloatOnly(control, true)`:
- Control can only be in floating state
- Cannot be docked, tabbed, or auto-hidden
- User cannot dock it back
- Useful for floating toolboxes or palettes

**Check if control is float-only:**

```csharp
bool isFloatOnly = this.dockingManager1.GetFloatOnly(panel1);
```

### Check Floating State

```csharp
// Check if control is currently floating
bool isFloating = this.dockingManager1.IsFloating(panel1);

if (isFloating)
{
    Console.WriteLine($"{panel1.Name} is floating");
}
```

**VB.NET:**

```vb
' Check floating state
Dim isFloating As Boolean = Me.dockingManager1.IsFloating(panel1)
```

## Floating Window Customization

### Show Custom Buttons When Floating

```csharp
// Display custom caption buttons in floating windows
this.dockingManager1.ShowCustomButtonsInFloating = true;
```

Not applicable for VS2005 visual style.

### Metro Border Width

```csharp
// Set border width for floating windows (Metro style)
this.dockingManager1.VisualStyle = VisualStyle.Metro;
this.dockingManager1.MetroBorderWidth = 2; // Default is 1
```

### Floating Window Properties

Get properties of floating windows:

```csharp
// Get individual floating control properties
var floatingProps = this.dockingManager1.GetFloatingControlProperties(panel1);

if (floatingProps != null)
{
    Point location = floatingProps.Location;
    Size size = floatingProps.Size;
    string caption = floatingProps.Text;
    
    Console.WriteLine($"Floating at: {location}, Size: {size}");
}
```

### Forward Keyboard Shortcuts

```csharp
// Forward keyboard shortcuts to floating windows
this.dockingManager1.ForwardMenuShortcuts = true;
```

When `true`, menu shortcuts work even when floating windows have focus.

## Maximize Floating Windows

### Enable Maximize for Floating Windows

```csharp
// Enable maximize button on floating windows
this.dockingManager1.MaximizeButtonEnabled = true;
```

Users can maximize floating windows to fill the screen.

### Handle Maximize Events

```csharp
// Handle when floating window is maximized
this.dockingManager1.ControlMaximized += (s, e) =>
{
    MessageBox.Show($"{e.Control.Name} was maximized");
};

// Handle when floating window is restored
this.dockingManager1.ControlRestored += (s, e) =>
{
    MessageBox.Show($"{e.Control.Name} was restored");
};
```

### Double-Click to Maximize

```csharp
// Handle caption double-click for custom maximize logic
this.dockingManager1.OnCaptionDoubleClick += (s, e) =>
{
    if (this.dockingManager1.IsFloating(e.Control))
    {
        // Custom maximize logic
        Form floatForm = e.Control.Parent as Form;
        if (floatForm != null)
        {
            if (floatForm.WindowState == FormWindowState.Normal)
            {
                floatForm.WindowState = FormWindowState.Maximized;
            }
            else
            {
                floatForm.WindowState = FormWindowState.Normal;
            }
        }
    }
};
```


## Complete Example
A complete working example is available in the samples repository.
This documentation focuses on API usage patterns.



## Best Practices

1. **Use appropriate initial positions** - Place floating windows where users expect them
2. **Consider multi-monitor setups** - Detect and utilize additional screens
3. **Save floating positions** - Use serialization to remember user's layout
4. **Enable double-click** - Let users easily float/dock by double-clicking
5. **Set reasonable sizes** - Don't make floating windows too small or too large
6. **Use float-only sparingly** - Most windows should allow re-docking
7. **Test on different screen sizes** - Ensure windows appear on-screen
8. **Handle window state changes** - React to maximize/restore events

## Troubleshooting

**Floating window appears off-screen:**
- Check screen bounds before floating
- Use `Screen.PrimaryScreen.WorkingArea` to ensure visibility
- Validate coordinates are positive and within screen bounds

**Cannot float window:**
- Check `DisallowFloating` is `false`
- Verify `DockAbility` includes `DockAbility.Floatable`
- Ensure control is not in `SetFloatOnly` mode with `false` parameter

**Cannot dock floating window back:**
- Check if `SetFloatOnly(control, true)` was called
- Verify `DockAbility` includes `DockAbility.Dockable`
- Enable double-click or drag to dock

**Custom buttons not showing when floating:**
- Set `ShowCustomButtonsInFloating` to `true`
- Not supported in VS2005 visual style - use different style

**Floating window doesn't maximize:**
- Set `MaximizeButtonEnabled` to `true`
- Check visual style supports maximize button
- Verify another control is docked below the window
