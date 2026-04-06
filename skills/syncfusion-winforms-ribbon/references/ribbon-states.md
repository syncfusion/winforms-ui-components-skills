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

### Display States

```csharp
// ShowTabsAndCommands - Full ribbon (default), all groups and controls visible
ribbonControlAdv1.DisplayOption = RibbonDisplayOption.ShowTabsAndCommands;

// ShowTabs - Tabs only (minimized), commands appear temporarily when tab is clicked
// Single-click shows temporarily, double-click pins to ShowTabsAndCommands
ribbonControlAdv1.DisplayOption = RibbonDisplayOption.ShowTabs;

// AutoHide - Completely hidden, click at top to temporarily show
ribbonControlAdv1.DisplayOption = RibbonDisplayOption.AutoHide;
```

## DisplayOption Property

```csharp
// Enum: AutoHide=0, ShowTabs=1, ShowTabsAndCommands=2 (default)
public RibbonDisplayOption DisplayOption { get; set; }

// Check and set state
var currentState = ribbonControlAdv1.DisplayOption;
ribbonControlAdv1.DisplayOption = RibbonDisplayOption.ShowTabs;

// Set initial state at form load
public Form1()
{
    InitializeComponent();
    ribbonControlAdv1.DisplayOption = RibbonDisplayOption.ShowTabs;
}
```

## Changing Ribbon State

Users can change ribbon state through multiple methods:

```csharp
// Method 1: Display option button (top-right corner, shows menu with 3 options)
ribbonControlAdv1.ShowRibbonDisplayOptionButton = true;

// Method 2: Minimize button (toggles between ShowTabsAndCommands and ShowTabs only)
ribbonControlAdv1.ShowMinimizeButton = true;
ribbonControlAdv1.MinimizeToolTip = "Click to collapse ribbon";
ribbonControlAdv1.MaximizeToolTip = "Click to expand ribbon";

// Method 3: Double-click on any tab (toggles, cannot be disabled)
// Method 4: Context menu on tab (right-click shows "Collapse/Pin the Ribbon")
// Method 5: Programmatic control
ribbonControlAdv1.DisplayOption = ribbonControlAdv1.DisplayOption == RibbonDisplayOption.ShowTabsAndCommands
    ? RibbonDisplayOption.ShowTabs
    : RibbonDisplayOption.ShowTabsAndCommands;

// Cycle through all states
private void CycleRibbonStates() => ribbonControlAdv1.DisplayOption = 
    ribbonControlAdv1.DisplayOption switch {
        RibbonDisplayOption.ShowTabsAndCommands => RibbonDisplayOption.ShowTabs,
        RibbonDisplayOption.ShowTabs => RibbonDisplayOption.AutoHide,
        _ => RibbonDisplayOption.ShowTabsAndCommands
    };
```

## DisplayOptionChanged Event

```csharp
// EventArgs: OldValue, NewValue (both RibbonDisplayOption)
// Fires whenever ribbon state changes (button, double-click, programmatic, etc.)
ribbonControlAdv1.DisplayOptionChanged += (s, e) => {
    Console.WriteLine($"Ribbon: {e.OldValue} → {e.NewValue}");
    
    // Save user preference
    Properties.Settings.Default.RibbonDisplayOption = (int)e.NewValue;
    Properties.Settings.Default.Save();
};

// Restore on form load
public Form1() {
    InitializeComponent();
    ribbonControlAdv1.DisplayOption = 
        (RibbonDisplayOption)Properties.Settings.Default.RibbonDisplayOption;
}

// Prevent state changes conditionally
ribbonControlAdv1.DisplayOptionChanged += (s, e) => {
    if (!allowStateChange) {
        ribbonControlAdv1.DisplayOption = e.OldValue;
        MessageBox.Show("Ribbon state change is not allowed.");
    }
};
```

## Keyboard Shortcuts

```csharp
// Default: Ctrl+F1 toggles between ShowTabsAndCommands and ShowTabs
ribbonControlAdv1.EnableRibbonStateAccelerator = true; // Default

// Custom shortcuts via ProcessCmdKey
protected override bool ProcessCmdKey(ref Message msg, Keys keyData) {
    if (keyData == (Keys.Control | Keys.R)) {
        CycleRibbonStates();
        return true;
    }
    if (keyData == (Keys.Control | Keys.Shift | Keys.R)) {
        ribbonControlAdv1.DisplayOption = RibbonDisplayOption.ShowTabsAndCommands;
        return true;
    }
    return base.ProcessCmdKey(ref msg, keyData);
}
```

## Troubleshooting

| Issue | Cause | Solution |
|-------|-------|----------|
| Minimize button doesn't maximize | Ribbon in AutoHide state | Minimize button only toggles ShowTabs/ShowTabsAndCommands. Use display option button or set `DisplayOption` directly |
| Ctrl+F1 doesn't work | `EnableRibbonStateAccelerator = false` | Set to `true` |
| State not persisting | Not saving to settings | Handle `DisplayOptionChanged` event and save to `Properties.Settings` |
| Ribbon flashes and disappears | ShowTabs/AutoHide temporary display | Normal behavior. Double-click tab or set `DisplayOption = ShowTabsAndCommands` |
| Users can't minimize ribbon | Controls not visible | Enable `ShowRibbonDisplayOptionButton` and `ShowMinimizeButton`, add helpful tooltips |

## Best Practices

1. Enable both minimize and display option buttons for flexibility
2. Always persist ribbon state to application settings via `DisplayOptionChanged` event
3. Keep `EnableRibbonStateAccelerator` enabled (default) for keyboard accessibility
4. Set appropriate default: ShowTabsAndCommands (document editors), ShowTabs (viewers), AutoHide (full-screen)
5. Provide clear tooltips mentioning Ctrl+F1 and double-click functionality
6. Test all state change methods: buttons, double-click, context menu, keyboard, programmatic
7. Inform users about built-in state change methods (double-click, Ctrl+F1)