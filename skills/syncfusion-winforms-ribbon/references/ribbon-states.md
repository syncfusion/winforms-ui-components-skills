# Ribbon States and Display Modes

## Table of Contents
- [Overview](#overview)
- [Ribbon Display States](#ribbon-display-states)
- [DisplayOption Property](#displayoption-property)
- [Changing Ribbon State](#changing-ribbon-state)
- [DisplayOptionChanged Event](#displayoptionchanged-event)
- [Keyboard Shortcuts](#keyboard-shortcuts)
- [Button Customization](#button-customization)
- [Troubleshooting](#troubleshooting)

## Overview

The RibbonControlAdv supports three display states that control ribbon visibility and behavior. Understanding these states is crucial for implementing minimize/maximize functionality and providing users with flexible workspace options.

**Three Display States:**
1. **ShowTabsAndCommands** - Full ribbon (maximized)
2. **ShowTabs** - Tabs only (minimized)
3. **AutoHide** - Completely hidden (auto-hide)

## Ribbon Display States

### ShowTabsAndCommands (Maximized State)

**Description:** Shows both tab names and all ribbon commands. This is the default and most common state.

**Visual Appearance:**
- Tab strip visible at top
- All groups and controls visible
- Full ribbon height

**When to use:** Default state for most applications, provides full access to all commands.

**Code Example:**
```csharp
ribbonControlAdv1.DisplayOption = RibbonDisplayOption.ShowTabsAndCommands;
```

### ShowTabs (Minimized State)

**Description:** Shows only tab names. Controls appear temporarily when a tab is clicked.

**Visual Appearance:**
- Only tab strip visible
- Reduced ribbon height
- Clicking a tab shows commands temporarily
- Commands hide when clicking outside ribbon

**When to use:** Maximize screen space while keeping ribbon accessible.

**Code Example:**
```csharp
ribbonControlAdv1.DisplayOption = RibbonDisplayOption.ShowTabs;
```

**Behavior:**
- Single-clicking a tab shows ribbon temporarily
- Ribbon auto-hides when clicking outside
- Double-clicking a tab switches to ShowTabsAndCommands

### AutoHide (Completely Hidden)

**Description:** Ribbon completely hidden, only accessible by clicking at top of window.

**Visual Appearance:**
- No ribbon visible
- Maximum screen space for content
- Click at top to temporarily show ribbon

**When to use:** Full-screen or immersive modes, maximum content focus.

**Code Example:**
```csharp
ribbonControlAdv1.DisplayOption = RibbonDisplayOption.AutoHide;
```

**Behavior:**
- Click at top of window to show ribbon
- Ribbon appears temporarily
- Auto-hides when clicking outside

## DisplayOption Property

The `DisplayOption` property controls the current ribbon state.

### Property Definition

```csharp
public RibbonDisplayOption DisplayOption { get; set; }
```

### Enum Values

```csharp
public enum RibbonDisplayOption
{
    AutoHide = 0,              // Completely hidden
    ShowTabs = 1,              // Tabs only (minimized)
    ShowTabsAndCommands = 2    // Full ribbon (maximized) - Default
}
```

### Getting Current State

```csharp
// Check current display state
RibbonDisplayOption currentState = ribbonControlAdv1.DisplayOption;

if (currentState == RibbonDisplayOption.ShowTabsAndCommands)
{
    // Ribbon is fully expanded
}
else if (currentState == RibbonDisplayOption.ShowTabs)
{
    // Ribbon is minimized (tabs only)
}
else if (currentState == RibbonDisplayOption.AutoHide)
{
    // Ribbon is hidden
}
```

### Setting State Programmatically

```csharp
// Maximize ribbon (show all)
ribbonControlAdv1.DisplayOption = RibbonDisplayOption.ShowTabsAndCommands;

// Minimize ribbon (tabs only)
ribbonControlAdv1.DisplayOption = RibbonDisplayOption.ShowTabs;

// Hide ribbon completely
ribbonControlAdv1.DisplayOption = RibbonDisplayOption.AutoHide;
```

### Setting Initial State at Form Load

```csharp
public Form1()
{
    InitializeComponent();
    
    // Set initial ribbon state
    ribbonControlAdv1.DisplayOption = RibbonDisplayOption.ShowTabs;
}
```

## Changing Ribbon State

Users can change ribbon state through multiple methods.

### Method 1: Display Option Button

The display option button appears in the top-right corner of the ribbon, next to minimize/close buttons.

**Enabling the Button:**
```csharp
// Show display option button
ribbonControlAdv1.ShowRibbonDisplayOptionButton = true;
```

**Button Behavior:**
- Clicking opens a menu with three options:
  - Auto-hide Ribbon
  - Show Tabs
  - Show Tabs and Commands

**Code Example:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Enable display option button
    ribbonControlAdv1.ShowRibbonDisplayOptionButton = true;
}
```

### Method 2: Minimize Button

The minimize button toggles between ShowTabsAndCommands and ShowTabs states.

**Enabling the Button:**
```csharp
// Show minimize button
ribbonControlAdv1.ShowMinimizeButton = true;
```

**Button Behavior:**
- If ribbon is maximized (ShowTabsAndCommands): clicking minimizes to ShowTabs
- If ribbon is minimized (ShowTabs): clicking maximizes to ShowTabsAndCommands
- Does NOT affect AutoHide state

**Code Example:**
```csharp
// Show both buttons
ribbonControlAdv1.ShowMinimizeButton = true;
ribbonControlAdv1.ShowRibbonDisplayOptionButton = true;
```

**Customizing Tooltips:**
```csharp
// Set custom tooltip for minimize button
ribbonControlAdv1.MinimizeToolTip = "Click to collapse ribbon";

// Set custom tooltip for maximize button (when minimized)
ribbonControlAdv1.MaximizeToolTip = "Click to expand ribbon";
```

### Method 3: Double-Click on Tab

**Behavior:**
- Double-clicking any ToolStripTabItem toggles between ShowTabsAndCommands and ShowTabs
- Quick way for users to minimize/maximize ribbon

**Cannot be disabled** - this is built-in behavior.

### Method 4: Context Menu

Right-click on any tab or ribbon item to access context menu.

**Context Menu Option:**
- "Collapse the Ribbon" - Switches to ShowTabs state
- "Pin the Ribbon" - Switches to ShowTabsAndCommands state (when minimized)

**Customizing Context Menu:**
See customization.md for adding custom context menu items.

### Method 5: Programmatic Control

```csharp
// Toggle between maximized and minimized
private void ToggleRibbonState()
{
    if (ribbonControlAdv1.DisplayOption == RibbonDisplayOption.ShowTabsAndCommands)
    {
        // Minimize
        ribbonControlAdv1.DisplayOption = RibbonDisplayOption.ShowTabs;
    }
    else
    {
        // Maximize
        ribbonControlAdv1.DisplayOption = RibbonDisplayOption.ShowTabsAndCommands;
    }
}

// Cycle through all states
private void CycleRibbonStates()
{
    switch (ribbonControlAdv1.DisplayOption)
    {
        case RibbonDisplayOption.ShowTabsAndCommands:
            ribbonControlAdv1.DisplayOption = RibbonDisplayOption.ShowTabs;
            break;
        case RibbonDisplayOption.ShowTabs:
            ribbonControlAdv1.DisplayOption = RibbonDisplayOption.AutoHide;
            break;
        case RibbonDisplayOption.AutoHide:
            ribbonControlAdv1.DisplayOption = RibbonDisplayOption.ShowTabsAndCommands;
            break;
    }
}
```

## DisplayOptionChanged Event

The `DisplayOptionChanged` event fires whenever the ribbon state changes, regardless of how the change was triggered.

### Event Signature

```csharp
public event EventHandler<DisplayOptionChangedEventArgs> DisplayOptionChanged;
```

### EventArgs Properties

```csharp
public class DisplayOptionChangedEventArgs : EventArgs
{
    public RibbonDisplayOption OldValue { get; }  // Previous state
    public RibbonDisplayOption NewValue { get; }  // New state
}
```

### Handling the Event

```csharp
// Subscribe to event
ribbonControlAdv1.DisplayOptionChanged += RibbonControlAdv1_DisplayOptionChanged;

private void RibbonControlAdv1_DisplayOptionChanged(object sender, DisplayOptionChangedEventArgs e)
{
    // Log state change
    Console.WriteLine($"Ribbon state changed from {e.OldValue} to {e.NewValue}");
    
    // Handle specific transitions
    if (e.NewValue == RibbonDisplayOption.ShowTabsAndCommands)
    {
        // Ribbon was maximized
        OnRibbonMaximized();
    }
    else if (e.NewValue == RibbonDisplayOption.ShowTabs)
    {
        // Ribbon was minimized
        OnRibbonMinimized();
    }
    else if (e.NewValue == RibbonDisplayOption.AutoHide)
    {
        // Ribbon was hidden
        OnRibbonHidden();
    }
}
```

### Saving User Preference

```csharp
// Save ribbon state to application settings
private void RibbonControlAdv1_DisplayOptionChanged(object sender, DisplayOptionChangedEventArgs e)
{
    Properties.Settings.Default.RibbonDisplayOption = (int)e.NewValue;
    Properties.Settings.Default.Save();
}

// Restore ribbon state on form load
public Form1()
{
    InitializeComponent();
    
    // Restore saved state
    int savedState = Properties.Settings.Default.RibbonDisplayOption;
    ribbonControlAdv1.DisplayOption = (RibbonDisplayOption)savedState;
}
```

### Preventing State Changes

```csharp
private bool allowStateChange = true;

private void RibbonControlAdv1_DisplayOptionChanged(object sender, DisplayOptionChangedEventArgs e)
{
    if (!allowStateChange)
    {
        // Revert to previous state
        ribbonControlAdv1.DisplayOption = e.OldValue;
        MessageBox.Show("Ribbon state change is not allowed at this time.");
    }
}
```

## Keyboard Shortcuts

### Default Shortcut: Ctrl+F1

The ribbon includes a built-in keyboard shortcut to toggle state.

**Behavior:**
- **Ctrl+F1** toggles between ShowTabsAndCommands and ShowTabs
- Works in any ribbon state
- Provides keyboard accessibility

**Enabling/Disabling:**
```csharp
// Enable shortcut (default)
ribbonControlAdv1.EnableRibbonStateAccelerator = true;

// Disable shortcut
ribbonControlAdv1.EnableRibbonStateAccelerator = false;
```

**Usage Example:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Ensure keyboard shortcut is enabled
    ribbonControlAdv1.EnableRibbonStateAccelerator = true;
    
    // Optionally inform users
    ToolStripLabel hintLabel = new ToolStripLabel();
    hintLabel.Text = "Press Ctrl+F1 to toggle ribbon";
}
```

### Custom Keyboard Shortcuts

```csharp
// Override ProcessCmdKey for custom shortcuts
protected override bool ProcessCmdKey(ref Message msg, Keys keyData)
{
    // Ctrl+R to cycle through ribbon states
    if (keyData == (Keys.Control | Keys.R))
    {
        CycleRibbonStates();
        return true;
    }
    
    // Ctrl+Shift+R to reset to maximized
    if (keyData == (Keys.Control | Keys.Shift | Keys.R))
    {
        ribbonControlAdv1.DisplayOption = RibbonDisplayOption.ShowTabsAndCommands;
        return true;
    }
    
    return base.ProcessCmdKey(ref msg, keyData);
}
```

## Button Customization

### Show/Hide Display Option Button

```csharp
// Show display option button (recommended)
ribbonControlAdv1.ShowRibbonDisplayOptionButton = true;

// Hide display option button
ribbonControlAdv1.ShowRibbonDisplayOptionButton = false;
```

### Show/Hide Minimize Button

```csharp
// Show minimize button
ribbonControlAdv1.ShowMinimizeButton = true;

// Hide minimize button
ribbonControlAdv1.ShowMinimizeButton = false;
```

### Custom Button Tooltips

```csharp
// Customize minimize button tooltip
ribbonControlAdv1.MinimizeToolTip = "Collapse Ribbon (Ctrl+F1)";

// Customize maximize button tooltip (shown when minimized)
ribbonControlAdv1.MaximizeToolTip = "Expand Ribbon (Ctrl+F1)";

// Clear tooltips
ribbonControlAdv1.MinimizeToolTip = string.Empty;
ribbonControlAdv1.MaximizeToolTip = string.Empty;
```

### Complete Customization Example

```csharp
private void CustomizeRibbonStateControls()
{
    // Show both state control buttons
    ribbonControlAdv1.ShowRibbonDisplayOptionButton = true;
    ribbonControlAdv1.ShowMinimizeButton = true;
    
    // Customize tooltips
    ribbonControlAdv1.MinimizeToolTip = "Minimize ribbon to save space";
    ribbonControlAdv1.MaximizeToolTip = "Show full ribbon";
    
    // Enable keyboard shortcut
    ribbonControlAdv1.EnableRibbonStateAccelerator = true;
    
    // Set initial state
    ribbonControlAdv1.DisplayOption = RibbonDisplayOption.ShowTabsAndCommands;
    
    // Handle state changes
    ribbonControlAdv1.DisplayOptionChanged += (s, e) =>
    {
        UpdateStatusBar($"Ribbon: {e.NewValue}");
    };
}
```

## Troubleshooting

### Issue: Minimize Button Doesn't Maximize Ribbon

**Symptom:** Clicking minimize button doesn't restore ribbon to full view.

**Cause:** Ribbon may be in AutoHide state, which minimize button doesn't affect.

**Solution:**
```csharp
// Check current state
if (ribbonControlAdv1.DisplayOption == RibbonDisplayOption.AutoHide)
{
    // Use DisplayOption property to maximize
    ribbonControlAdv1.DisplayOption = RibbonDisplayOption.ShowTabsAndCommands;
}
```

**Prevention:** Use display option button for full state control.

### Issue: Ctrl+F1 Doesn't Work

**Symptom:** Keyboard shortcut not responding.

**Cause:** `EnableRibbonStateAccelerator` is set to false.

**Solution:**
```csharp
// Enable keyboard shortcut
ribbonControlAdv1.EnableRibbonStateAccelerator = true;
```

### Issue: State Changes Not Persisting

**Symptom:** Ribbon state resets to default on restart.

**Cause:** State not saved to application settings.

**Solution:**
```csharp
// Save state on change
ribbonControlAdv1.DisplayOptionChanged += (s, e) =>
{
    Properties.Settings.Default.RibbonState = (int)e.NewValue;
    Properties.Settings.Default.Save();
};

// Restore on load
protected override void OnLoad(EventArgs e)
{
    base.OnLoad(e);
    ribbonControlAdv1.DisplayOption = 
        (RibbonDisplayOption)Properties.Settings.Default.RibbonState;
}
```

### Issue: Ribbon Shows Briefly Then Disappears

**Symptom:** Ribbon flashes and disappears when clicking.

**Cause:** Ribbon is in ShowTabs or AutoHide state with temporary display.

**Solution:** This is normal behavior. To keep ribbon visible:
```csharp
// Double-click tab to pin ribbon
// Or programmatically set state
ribbonControlAdv1.DisplayOption = RibbonDisplayOption.ShowTabsAndCommands;
```

### Issue: Users Can't Find How to Minimize Ribbon

**Symptom:** Users don't know how to collapse ribbon.

**Solution:** Enable visible controls and provide hints:
```csharp
// Show both buttons
ribbonControlAdv1.ShowRibbonDisplayOptionButton = true;
ribbonControlAdv1.ShowMinimizeButton = true;

// Add helpful tooltips
ribbonControlAdv1.MinimizeToolTip = 
    "Minimize Ribbon (or press Ctrl+F1 or double-click any tab)";
```

## Best Practices

1. **Enable both buttons:** Show both minimize and display option buttons for maximum flexibility

2. **Save user preference:** Always persist ribbon state to application settings

3. **Handle state changes:** Implement DisplayOptionChanged event to respond to state changes

4. **Provide keyboard access:** Keep EnableRibbonStateAccelerator enabled (default)

5. **Set appropriate default:** Choose initial state based on application type:
   - Document editors: ShowTabsAndCommands
   - Viewers/browsers: ShowTabs
   - Full-screen apps: AutoHide

6. **Custom tooltips:** Provide clear, helpful tooltips for minimize/maximize buttons

7. **Test all methods:** Verify state changes work via button, double-click, context menu, and keyboard

8. **Consider simplified layout:** For modern UIs, combine with simplified layout mode (see simplified-layout.md)

9. **Accessibility:** Ensure keyboard shortcuts work for users who can't use mouse

10. **Document behavior:** Inform users about double-click and Ctrl+F1 functionality

## Related Topics

- **Simplified Layout** - Learn about modern compact ribbon layout mode
- **Customization** - Add custom context menu items and customize dialogs
- **Quick Access Toolbar** - Provide quick access to commands regardless of ribbon state
