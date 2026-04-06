# Auto-Hide Windows

Auto-hide mode collapses dock windows to tabs at the form edges, providing more workspace. Windows slide out when hovering over the tab and auto-hide again when focus is lost.

## Overview

**What:** Windows that collapse to edge tabs and expand on hover.

**When to use:**
- Maximize workspace for main content
- Occasional-use tools (properties, toolbox)
- Visual Studio-style layouts
- Space-constrained interfaces

**How:** Use `SetAutoHideMode()` or user clicks pin button.

## Enabling Auto-Hide

### Set Auto-Hide Programmatically

```csharp
// Set a control to auto-hide mode
this.dockingManager1.SetAutoHideMode(panel1, true);

// Disable auto-hide mode (return to previous dock state)
this.dockingManager1.SetAutoHideMode(panel1, false);
```

**VB.NET:**

```vb
' Enable auto-hide
Me.dockingManager1.SetAutoHideMode(panel1, True)

' Disable auto-hide
Me.dockingManager1.SetAutoHideMode(panel1, False)
```

### Auto-Hide with Specific Side

```csharp
// Auto-hide to a specific side with size
this.dockingManager1.DockControlInAutoHideMode(panel1, 
    DockingStyle.Left, 250);

// Available sides: Left, Right, Top, Bottom
this.dockingManager1.DockControlInAutoHideMode(panel2, 
    DockingStyle.Right, 200);
```

**VB.NET:**

```vb
' Auto-hide to left side
Me.dockingManager1.DockControlInAutoHideMode(panel1, _
    DockingStyle.Left, 250)
```

### Auto-Hide on Application Load

```csharp
// Set control to auto-hide when application starts
this.dockingManager1.SetAutoHideOnLoad(panel1, true);

// This must be set BEFORE docking the control
```

Typically set in form constructor or `Load` event before arranging windows.

## User-Initiated Auto-Hide

### Enable Pin Button

By default, users can toggle auto-hide using the pin button in the caption.

**Pin Button States:**
- **Unpinned (horizontal)** - Control is in auto-hide mode
- **Pinned (vertical)** - Control is docked normally

### Disable Auto-Hide Functionality

```csharp
// Disable auto-hide for all windows (hide pin button)
this.dockingManager1.AutoHideEnabled = false;
```

Users cannot auto-hide any windows when `false`.

### Hide Pin Button for Specific Control

```csharp
// Hide pin button for specific control
this.dockingManager1.SetAutoHideButtonVisibility(panel1, false);
```

The control can still be auto-hidden programmatically.

## Animation Configuration

### Animation Speed

```csharp
// Set animation step (pixels per frame)
this.dockingManager1.AnimationStep = 20; // Default is 20

// Faster animation
this.dockingManager1.AnimationStep = 40;

// Slower animation
this.dockingManager1.AnimationStep = 10;
```

Higher values = faster animation.

### Disable Animation

```csharp
// Disable slide animation (instant show/hide)
this.dockingManager1.AnimateAutoHiddenWindow = false;
```

Window appears/disappears instantly without sliding effect.

### Auto-Hide Interval

```csharp
// Set hover delay before showing window (milliseconds)
this.dockingManager1.AutoHideInterval = 300; // Default is 300ms

// Shorter delay (more responsive)
this.dockingManager1.AutoHideInterval = 100;

// Longer delay (less sensitive)
this.dockingManager1.AutoHideInterval = 500;
```

## Auto-Hide Tab Customization

### Tab Selection Behavior

```csharp
// Show window on mouse hover (default)
this.dockingManager1.AutoHideSelectionStyle = AutoHideSelectionStyle.MouseHover;

// Show window only on click
this.dockingManager1.AutoHideSelectionStyle = AutoHideSelectionStyle.Click;
```

**MouseHover:** Window appears when hovering over tab.
**Click:** User must click tab to show window.

### Tab Font and Colors

```csharp
// Set auto-hide tab font
this.dockingManager1.AutoHideTabFont = new Font("Segoe UI", 9f, FontStyle.Bold);

// Set tab foreground color
this.dockingManager1.AutoHideTabForeColor = Color.DarkBlue;

// Set tab height
this.dockingManager1.AutoHideTabHeight = 25; // Default is about 20
```

**VB.NET:**

```vb
' Customize auto-hide tabs
Me.dockingManager1.AutoHideTabFont = New Font("Segoe UI", 9.0F, FontStyle.Bold)
Me.dockingManager1.AutoHideTabForeColor = Color.DarkBlue
Me.dockingManager1.AutoHideTabHeight = 25
```

### Tab Background Color

```csharp
// Get auto-hide tab control to customize background
// For left-side tabs
Control ahTabControl = this.dockingManager1.GetAHTabControl(
    Syncfusion.Windows.Forms.Tools.DockTabAlignmentStyle.Left);

if (ahTabControl != null)
{
    ahTabControl.BackColor = Color.LightGray;
}

// For other sides: Right, Top, Bottom
Control ahTabRight = this.dockingManager1.GetAHTabControl(
    Syncfusion.Windows.Forms.Tools.DockTabAlignmentStyle.Right);
if (ahTabRight != null)
{
    ahTabRight.BackColor = Color.LightGreen;
}
```

### Full Caption in Auto-Hide Mode

```csharp
// Show full caption instead of vertical text
this.dockingManager1.FullCaptionsInAutoHideMode = true;
```

When `true`, auto-hide tabs display full horizontal captions instead of vertical text.

### Enable Tab Dragging

```csharp
// Allow dragging auto-hide tabs to reorder them
this.dockingManager1.EnableDragAutoHiddenTabs = true;
```

Users can reorder auto-hide tabs by dragging.

### Context Menu for Auto-Hide Tabs

```csharp
// Enable context menu on auto-hide tabs
this.dockingManager1.EnableAutoHideTabContextMenu = true;
```

Right-clicking an auto-hide tab shows a context menu with options.

## Caption Notification on Hover

```csharp
// Customize hover notification display time (milliseconds)
this.dockingManager1.AutoHideHoverTime = 1000; // Default is 1000ms

// Show tooltip when hovering over auto-hide tab
this.dockingManager1.ShowToolTips = true;
```

## Auto-Hide Events

### Animation Events

```csharp
// Handle when auto-hide animation starts
this.dockingManager1.AutoHideAnimationStart += (s, e) =>
{
    Console.WriteLine($"Animation started for: {e.DockBorder}");
    Console.WriteLine($"Window bounds: {e.Bounds}");
};

// Handle when auto-hide animation stops
this.dockingManager1.AutoHideAnimationStop += (s, e) =>
{
    Console.WriteLine($"Animation stopped for: {e.DockBorder}");
};
```

**AutoHideAnimationEventArgs Properties:**
- `Bounds` - Rectangle of the animating window
- `DockBorder` - Which side (Left, Right, Top, Bottom)

### State Change Events

```csharp
// Handle dock state changes
this.dockingManager1.DockStateChanged += (s, e) =>
{
    if (e.NewState == DockState.AutoHide)
    {
        Console.WriteLine($"Control entered auto-hide mode");
    }
    else if (e.OldState == DockState.AutoHide)
    {
        Console.WriteLine($"Control left auto-hide mode");
    }
};
```

## Complete Example
A full working example is available in the samples repository.
This documentation focuses on individual API usage.

## Best Practices

1. **Auto-hide occasional tools** - Ideal for properties, toolbox, rarely-used panels
2. **Keep main content docked** - Don't auto-hide primary work areas
3. **Use appropriate delays** - Balance responsiveness with accidental triggers
4. **Set on load for space savings** - Start with less important tools auto-hidden
5. **Customize tab appearance** - Match your application theme
6. **Enable hover selection** - More intuitive than click-only
7. **Allow tab dragging** - Let users organize auto-hide tabs

## Troubleshooting

**Auto-hide not working:**
- Check `AutoHideEnabled` is `true`
- Verify control is dockable (not floating or fill mode)
- Ensure pin button is visible
- Check `DockAbility` allows auto-hide

**Animation is too slow/fast:**
- Adjust `AnimationStep` property (10-40 range)
- Set `AnimateAutoHiddenWindow` to `false` for instant display
- Check `AutoHideInterval` isn't too long

**Tab doesn't expand on hover:**
- Verify `AutoHideSelectionStyle` is `MouseHover`
- Check `AutoHideInterval` isn't too high
- Ensure window is actually in auto-hide mode

**Cannot customize tab background:**
- Use `GetAHTabControl()` to get tab control
- Set `BackColor` on returned control
- Must be called after windows are docked
