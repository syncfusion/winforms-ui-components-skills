# Docking Events

The DockingManager provides 25+ events to track dock state changes, user actions, and lifecycle events. This guide covers the most important events and their usage.

## Table of Contents
- [Activation Events](#activation-events)
- [Dock State Events](#dock-state-events)
- [Auto-Hide Events](#auto-hide-events)
- [Window Management Events](#window-management-events)
- [Drag and Drop Events](#drag-and-drop-events)
- [Context Menu Events](#context-menu-events)
- [Serialization Events](#serialization-events)
- [Tab Group Events](#tab-group-events)
- [Advanced Events](#advanced-events)

## Activation Events

### DockControlActivated

Fires when a dock window becomes the active window.

```csharp
this.dockingManager1.DockControlActivated += (s, e) =>
{
    string label = this.dockingManager1.GetDockLabel(e.Control);
    Console.WriteLine($"Activated: {label}");
    
    // Update status bar, toolbar, or other UI
    this.Text = $"Active Window: {label}";
};
```

**DockActivationChangedEventArgs Properties:**
- `Control` - The control that was activated

### DockControlDeactivated

Fires when a dock window is no longer the active window.

```csharp
this.dockingManager1.DockControlDeactivated += (s, e) =>
{
    string label = this.dockingManager1.GetDockLabel(e.Control);
    Console.WriteLine($"Deactivated: {label}");
};
```

## Dock State Events

### DockStateChanged

Fires after a control's dock state changes (docked, floating, auto-hide, etc.).

```csharp
this.dockingManager1.DockStateChanged += (s, e) =>
{
    if (e.Controls.Length > 0)
    {
        foreach (Control ctrl in e.Controls)
        {
            string label = this.dockingManager1.GetDockLabel(ctrl);
            Console.WriteLine($"{label}: {e.OldState} → {e.NewState}");
        }
    }
};
```

**DockStateChangeEventArgs Properties:**
- `Controls[]` - Array of controls whose state changed
- `OldState` - Previous DockState (Docked, Floating, AutoHide, etc.)
- `NewState` - New DockState
- `Handled` - Set to true to mark event as handled


### DockStateChanging

Fires before a dock state changes. Can be cancelled.

```csharp
this.dockingManager1.DockStateChanging += (s, e) =>
{
    // Prevent floating for specific control
    if (e.Controls.Length > 0 && e.NewState == DockState.Floating)
    {
        if (e.Controls[0].Name == "panel1")
        {
            e.Handled = true; // Cancel the state change
            MessageBox.Show("This window cannot be floated!");
        }
    }
};
```

Set `e.Handled = true` to cancel the state change.

## Auto-Hide Events

### AutoHideAnimationStart

Fires when auto-hide animation begins (window sliding out).

```csharp
this.dockingManager1.AutoHideAnimationStart += (s, e) =>
{
    Console.WriteLine($"Animation started: {e.DockBorder}");
    Console.WriteLine($"Window bounds: {e.Bounds}");
};
```

**AutoHideAnimationEventArgs Properties:**
- `Bounds` - Rectangle of the animating window
- `DockBorder` - Which side (Left, Right, Top, Bottom)

### AutoHideAnimationStop

Fires when auto-hide animation ends.

```csharp
this.dockingManager1.AutoHideAnimationStop += (s, e) =>
{
    Console.WriteLine($"Animation stopped: {e.DockBorder}");
};
```

## Window Management Events

### DockVisibilityChanged

Fires after a dock window is shown or hidden.

```csharp
this.dockingManager1.DockVisibilityChanged += (s, e) =>
{
    bool isVisible = e.Control.Visible;
    string label = this.dockingManager1.GetDockLabel(e.Control);
    Console.WriteLine($"{label} visibility: {isVisible}");
};
```

**DockVisibilityEventArgs Properties:**
- `Control` - The control whose visibility changed

### DockVisibilityChanging

Fires before visibility changes. Can be cancelled.

```csharp
this.dockingManager1.DockVisibilityChanging += (s, e) =>
{
    // Prevent hiding specific control
    if (!e.Visible && e.Control.Name == "outputPanel")
    {
        e.Cancel = true;
        MessageBox.Show("Output window cannot be closed!");
    }
};
```

**DockVisibilityChangingEventArgs Properties:**
- `Control` - The control whose visibility is changing
- `Visible` - The new visibility state
- `Cancel` - Set to true to cancel the change

### ControlMaximizing

Fires before a floating window is maximized.

```csharp
this.dockingManager1.ControlMaximizing += (s, e) =>
{
    string label = this.dockingManager1.GetDockLabel(e.Control);
    Console.WriteLine($"Maximizing: {label}");
};
```

### ControlMaximized

Fires after a floating window is maximized.

```csharp
this.dockingManager1.ControlMaximized += (s, e) =>
{
    string label = this.dockingManager1.GetDockLabel(e.Control);
    MessageBox.Show($"{label} is now maximized");
};
```

### ControlRestored

Fires after a maximized floating window is restored to normal size.

```csharp
this.dockingManager1.ControlRestored += (s, e) =>
{
    string label = this.dockingManager1.GetDockLabel(e.Control);
    Console.WriteLine($"Restored: {label}");
};
```

### ControlMinimized

Fires when a floating window is minimized.

```csharp
this.dockingManager1.ControlMinimized += (s, e) =>
{
    Console.WriteLine($"Control minimized: {e.Control.Name}");
};
```

## Drag and Drop Events

### DragAllow

Fires when user starts dragging. Can prevent dragging.

```csharp
this.dockingManager1.DragAllow += (s, e) =>
{
    // Prevent dragging specific control
    if (e.TargetControl != null && e.TargetControl.Name == "lockedPanel")
    {
        e.Cancel = true;
    }
};
```

**DragAllowEventArgs Properties:**
- `TargetControl` - The control being dragged
- `Cancel` - Set to true to prevent dragging


### DragFeedbackStart

Fires when drag feedback (visual hints) begins.

```csharp
this.dockingManager1.DragFeedbackStart += (s, e) =>
{
    Console.WriteLine("Drag feedback started");
};
```

### DragFeedbackStop

Fires when drag feedback ends.

```csharp
this.dockingManager1.DragFeedbackStop += (s, e) =>
{
    Console.WriteLine("Drag feedback stopped");
};
```

### PreviewDockHints

Fires before displaying dock hints during drag. Can customize which dock positions are allowed.

```csharp
this.dockingManager1.PreviewDockHints += (s, e) =>
{
    // Prevent docking to right side for specific control
    if (e.DraggingSource != null && e.DraggingSource.Name == "toolbox")
    {
        if (e.IsOuterDockHints)
        {
            // Remove right docking ability from outer hints
            e.DockAbility &= ~DockAbility.Right;
        }
    }
};
```

**PreviewDockHintsEventArgs Properties:**
- `DockAbility` - Available docking positions (can be modified)
- `DraggingSource` - Control being dragged
- `DraggingTarget` - Control being dragged over
- `IsOuterDockHints` - True for outer docking, false for inner/tabbed

## Context Menu Events

### DockContextMenu

Fires when context menu is about to be displayed. Can add/remove menu items.

```csharp
using Syncfusion.Windows.Forms.Tools.XPMenus;

this.dockingManager1.DockContextMenu += (s, e) =>
{
    // Add custom menu item
    BarItem customItem = new BarItem();
    customItem.Text = "Custom Action";
    customItem.Click += (sender, args) =>
    {
        string label = this.dockingManager1.GetDockLabel(e.Owner);
        MessageBox.Show($"Custom action for {label}");
    };
    
    // Insert at beginning
    e.ContextMenu.ParentBarItem.Items.Insert(0, customItem);
    
    // Add separator
    e.ContextMenu.ParentBarItem.Items.Insert(1, new BarItem { BarItemType = BarItemType.Sep });
};
```

**DockContextMenuEventArgs Properties:**
- `ContextMenu` - The context menu (MainFrameBarManager)
- `Owner` - The control whose context menu is shown


### AutoHideTabContextMenu

Fires for auto-hide tab context menus.

```csharp
this.dockingManager1.AutoHideTabContextMenu += (s, e) =>
{
    // Customize auto-hide tab context menu
    BarItem item = new BarItem { Text = "Pin Window" };
    item.Click += (sender, args) =>
    {
        this.dockingManager1.SetAutoHideMode(e.Owner, false);
    };
    e.ContextMenu.ParentBarItem.Items.Add(item);
};
```

### DockMenuClick

Fires when "Dock" menu item is clicked in context menu (re-dock from floating).

```csharp
this.dockingManager1.DockMenuClick += (s, e) =>
{
    Console.WriteLine("User clicked Dock menu item");
};
```

## Serialization Events

### NewDockStateBeginLoad

Fires before loading serialized dock state.

```csharp
this.dockingManager1.NewDockStateBeginLoad += (s, e) =>
{
    Console.WriteLine("Loading dock state...");
    // Show loading indicator
};
```

### NewDockStateEndLoad

Fires after loading serialized dock state.

```csharp
this.dockingManager1.NewDockStateEndLoad += (s, e) =>
{
    Console.WriteLine("Dock state loaded successfully");
    // Hide loading indicator, apply custom settings
};
```


### InitializeControlOnLoad

Fires when loading state for a control that doesn't exist. Recreate the control here.

```csharp
this.dockingManager1.InitializeControlOnLoad += (s, e) =>
{
    if (e.Control == null)
    {
        // Control doesn't exist, create it
        Panel panel = new Panel { Name = e.ControlName };
        this.Controls.Add(panel);
        this.dockingManager1.SetEnableDocking(panel, true);
        e.Control = panel;
    }
};
```

**InitializeControlOnLoadEventArgs Properties:**
- `Control` - Existing control (null if doesn't exist)
- `ControlName` - Name of the control to load
- Set `Control` property to new control instance if recreating

## Tab Group Events

### TabGroupCreating

Fires before a tab group is created. Can be cancelled.

```csharp
this.dockingManager1.TabGroupCreating += (s, e) =>
{
    Console.WriteLine($"Creating tab group for: {e.TargetItem.Name}");
    Console.WriteLine($"Orientation: {e.Orientation}");
    
    // Prevent tab group creation
    // e.Cancel = true;
};
```

**TabGroupCreatingEventArgs Properties:**
- `TargetItem` - The control being tabbed
- `Orientation` - Horizontal or Vertical
- `Cancel` - Set to true to prevent tab group creation

### TabGroupCreated

Fires after a tab group is created.

```csharp
this.dockingManager1.TabGroupCreated += (s, e) =>
{
    Console.WriteLine($"Tab group created:");
    Console.WriteLine($"  Current group: {e.CurrentTabGroup}");
    Console.WriteLine($"  Previous group: {e.PreviousTabGroup}");
    Console.WriteLine($"  Total groups: {e.TabGroups?.Count ?? 0}");
};
```

**TabGroupCreatedEventArgs Properties:**
- `CurrentTabGroup` - The newly created tab group
- `PreviousTabGroup` - The previous tab group
- `TabGroups` - Collection of all tab groups
- `TargetItem` - The control that was tabbed


## Complete Example
A full working example is available in the samples repository.
This documentation focuses on individual API us


## Best Practices

1. **Use events for UI updates** - Update status bars, toolbars based on activation
2. **Log state changes** - Track user behavior for analytics or debugging
3. **Validate operations** - Use *Changing events to prevent invalid actions
4. **Customize context menus** - Add application-specific menu items
5. **Handle serialization events** - Apply custom settings after loading layout
6. **Monitor tab groups** - Track tab group creation for custom logic
7. **Unsubscribe when disposing** - Prevent memory leaks

## Troubleshooting

**Events not firing:**
- Verify event handler is subscribed correctly
- Check if operation is performed programmatically (some events only fire for user actions)
- Ensure DockingManager is properly initialized

**Cannot cancel operation:**
- Use *Changing/*ing events, not *Changed/*ed events
- Set `e.Cancel = true` or `e.Handled = true` depending on event
- Some operations cannot be cancelled programmatically

**Event fires multiple times:**
- Expected for operations affecting multiple controls (e.g., tabbing)
- Check `e.Controls` array length
- Consider debouncing rapid events

**Context menu items don't appear:**
- Insert items at correct index (0 for top)
- Use proper BarItem type
- Verify context menu event fires before menu displays
