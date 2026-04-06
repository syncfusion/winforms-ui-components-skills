# Keyboard Interaction in PopupMenu

Keyboard interaction enhances menu accessibility and efficiency through shortcuts, mnemonics, and touch support. PopupMenu provides comprehensive keyboard navigation capabilities.

## Overview

**Interaction Methods:**
- **Touch Support:** Enabled by default for touch devices
- **Keyboard Shortcuts:** Direct command access (Ctrl+S, F5, etc.)
- **Keyboard Mnemonics:** Menu navigation via Alt+Key combinations
- **Click Events:** Triggered by both mouse and keyboard

## Touch Support

Touch mode enables easy access to PopupMenu on touch devices like tablets and touch-screen monitors.

### Default Behavior

Touch support is **enabled by default** in PopupMenu control. No configuration required.

### Touch Interaction

- **Tap:** Opens popup menu (equivalent to right-click)
- **Tap item:** Executes menu command (equivalent to left-click)
- **Swipe:** Scrolls through long menus
- **Touch-friendly sizing:** Items automatically sized for touch targets

### Benefits

- Improved usability on tablet/touch-screen devices
- No additional code required
- Automatic touch gesture recognition
- Compatible with mouse/keyboard interaction

## Keyboard Shortcuts

Keyboard shortcuts provide direct access to menu commands without opening the menu. The `Shortcut` property assigns standard Windows shortcuts to BarItems.

### Basic Shortcut Assignment

```csharp
BarItem saveItem = new BarItem {
    Text = "Save",
    Shortcut = Shortcut.CtrlS
};
saveItem.Click += (s, e) => SaveDocument();
parentBarItem1.Items.Add(saveItem);
```

### Common Shortcuts

```csharp
// File: CtrlN (New), CtrlO (Open), CtrlS (Save), CtrlP (Print)
// Edit: CtrlZ (Undo), CtrlY (Redo), CtrlX (Cut), CtrlC (Copy), CtrlV (Paste)
// Search: CtrlF (Find), F3 (Find Next), CtrlH (Replace)
// Function: F1 (Help), F2 (Rename), F5 (Refresh)

parentBarItem1.Items.AddRange(new BarItem[] {
    new BarItem { Text = "Save", Shortcut = Shortcut.CtrlS },
    new BarItem { Text = "Copy", Shortcut = Shortcut.CtrlC },
    new BarItem { Text = "Find", Shortcut = Shortcut.CtrlF }
});
```

**Available:** Ctrl/Alt/Shift combinations, F1-F12, Insert/Delete/Home/End, etc.

## Custom Shortcut Text

The `ShortcutText` property allows custom text in place of the standard shortcut display.

### Custom Shortcut Text

```csharp
// Override default shortcut display
BarItem findItem = new BarItem {
    Text = "Find",
    Shortcut = Shortcut.CtrlF,
    ShortcutText = "Press Ctrl+F"  // Custom display
};

// Use cases: multi-key sequences, localization, additional context
new BarItem {
    Text = "Insert Date",
    ShortcutText = "Alt+Shift+D"  // No standard Shortcut enum
}
```

## Keyboard Mnemonics

Mnemonics enable keyboard navigation through menus using Alt+Key combinations. The ampersand (`&`) character designates the mnemonic letter.

### Basic Mnemonic Setup

```csharp
// Use ampersand (&) to designate mnemonic letter
BarItem fileItem = new BarItem {
    Text = "&File",  // Alt+F to trigger
    ShowMnemonicUnderlinesAlways = true,  // Always show underline
    SizeToFit = true
};
parentBarItem1.Items.Add(fileItem);
```

### Standard Menu Bar Mnemonics

```csharp
// Top-level menu items with mnemonics
ParentBarItem fileMenu = new ParentBarItem {
    Text = "&File",  // Alt+F
    ShowMnemonicUnderlinesAlways = true,
    SizeToFit = true
};

ParentBarItem editMenu = new ParentBarItem {
    Text = "&Edit",  // Alt+E
    ShowMnemonicUnderlinesAlways = true,
    SizeToFit = true
};

ParentBarItem viewMenu = new ParentBarItem {
    Text = "&View",  // Alt+V
    ShowMnemonicUnderlinesAlways = true,
    SizeToFit = true
};

ParentBarItem toolsMenu = new ParentBarItem {
    Text = "&Tools",  // Alt+T
    ShowMnemonicUnderlinesAlways = true,
    SizeToFit = true
};

ParentBarItem helpMenu = new ParentBarItem {
    Text = "&Help",  // Alt+H
    ShowMnemonicUnderlinesAlways = true,
    SizeToFit = true
};

rootParent.Items.AddRange(new BarItem[] { fileMenu, editMenu, viewMenu, toolsMenu, helpMenu });
```

### Submenu Item Mnemonics

```csharp
// File menu items
fileMenu.Items.AddRange(new BarItem[] {
    new BarItem { Text = "&New", Shortcut = Shortcut.CtrlN },      // Alt+F, N
    new BarItem { Text = "&Open", Shortcut = Shortcut.CtrlO },     // Alt+F, O
    new BarItem { Text = "&Save", Shortcut = Shortcut.CtrlS },     // Alt+F, S
    new BarItem { Text = "Save &As", Shortcut = Shortcut.CtrlShiftS },  // Alt+F, A
    new BarItem { Text = "&Close" },                               // Alt+F, C
    new BarItem { Text = "E&xit", Shortcut = Shortcut.AltF4 }     // Alt+F, X
});

// Edit menu items
editMenu.Items.AddRange(new BarItem[] {
    new BarItem { Text = "&Undo", Shortcut = Shortcut.CtrlZ },     // Alt+E, U
    new BarItem { Text = "&Redo", Shortcut = Shortcut.CtrlY },     // Alt+E, R
    new BarItem { Text = "Cu&t", Shortcut = Shortcut.CtrlX },      // Alt+E, T
    new BarItem { Text = "&Copy", Shortcut = Shortcut.CtrlC },     // Alt+E, C
    new BarItem { Text = "&Paste", Shortcut = Shortcut.CtrlV },    // Alt+E, P
    new BarItem { Text = "Select &All", Shortcut = Shortcut.CtrlA }  // Alt+E, A
});
```

### ShowMnemonicUnderlinesAlways Property

```csharp
// Always visible (recommended for clarity)
barItem.Text = "&Save";
barItem.ShowMnemonicUnderlinesAlways = true;
// Displays: Save (with underline always visible)

// Visible only when Alt is pressed (Windows default)
barItem.Text = "&Save";
barItem.ShowMnemonicUnderlinesAlways = false;
// Displays: Save (underline appears when user presses Alt)
```

### Mnemonic Best Practices

- Use unique letters within each menu level (prefer first letter: **&File**, **&Edit**, **&View**)
- Use second letter if conflict exists (**&New**, E**&xit**, Cu**&t**)
- Windows standard patterns: **&File** (F), **&Edit** (E), **&View** (V), **&Tools** (T), **&Help** (H)

## Click Event Handling

The `Click` event fires for mouse clicks, keyboard shortcuts, or mnemonics.

```csharp
BarItem saveItem = new BarItem {
    Text = "&Save",
    Shortcut = Shortcut.CtrlS
};
saveItem.Click += (sender, e) => SaveDocument();  // Handles all trigger methods
```

## Complete Keyboard-Enabled Menu Example

```csharp
// File menu with mnemonics, shortcuts, and event handlers
ParentBarItem fileMenu = new ParentBarItem {
    Text = "&File",
    ShowMnemonicUnderlinesAlways = true
};

fileMenu.Items.AddRange(new BarItem[] {
    new BarItem { Text = "&New", Shortcut = Shortcut.CtrlN, Click += (s, e) => CreateNewDocument() },
    new BarItem { Text = "&Open...", Shortcut = Shortcut.CtrlO, Click += (s, e) => OpenDocument() },
    new BarItem { Text = "&Save", Shortcut = Shortcut.CtrlS, Click += (s, e) => SaveDocument() },
    new BarItem { Text = "E&xit", Shortcut = Shortcut.AltF4, Click += (s, e) => Application.Exit() }
});

ParentBarItem editMenu = new ParentBarItem {
    Text = "&Edit",
    ShowMnemonicUnderlinesAlways = true
};

editMenu.Items.AddRange(new BarItem[] {
    new BarItem { Text = "&Undo", Shortcut = Shortcut.CtrlZ, Click += (s, e) => richTextBox1.Undo() },
    new BarItem { Text = "Cu&t", Shortcut = Shortcut.CtrlX, Click += (s, e) => richTextBox1.Cut() },
    new BarItem { Text = "&Copy", Shortcut = Shortcut.CtrlC, Click += (s, e) => richTextBox1.Copy() },
    new BarItem { Text = "&Paste", Shortcut = Shortcut.CtrlV, Click += (s, e) => richTextBox1.Paste() }
});

ParentBarItem rootParent = new ParentBarItem();
rootParent.Items.AddRange(new BarItem[] { fileMenu, editMenu });
popupMenu1.ParentBarItem = rootParent;
popupMenusManager1.SetXPContextMenu(richTextBox1, popupMenu1);
```

## Best Practices

- **Shortcuts**: Use standard Windows shortcuts (Ctrl+S, Ctrl+C, etc.); avoid system shortcuts (Alt+Tab, Win+D); display shortcuts in tooltips
- **Mnemonics**: Provide for all menu items; use unique letters per level; follow Windows patterns; set `ShowMnemonicUnderlinesAlways = true`
- **Touch**: Default support enabled; test on actual devices; ensure adequate spacing for touch targets
- **Accessibility**: Provide both mouse/keyboard access; include shortcut info in tooltips; test keyboard-only navigation

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Shortcut doesn't work | Verify `Click` event wired, `Shortcut` property set, no conflicts, item enabled, form has focus |
| Mnemonic doesn't trigger | Check ampersand (`&`) in `Text`, `ShowMnemonicUnderlinesAlways` setting, unique mnemonics |
| Click event fires twice | Check for duplicate handler registration, shortcut registered elsewhere |
| Touch doesn't work | Verify touch device configured, control visible/enabled, no overlay blocking interaction |
