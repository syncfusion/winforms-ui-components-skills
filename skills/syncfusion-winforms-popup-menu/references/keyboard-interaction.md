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
BarItem saveItem = new BarItem();
saveItem.Text = "Save";
saveItem.Shortcut = System.Windows.Forms.Shortcut.CtrlS;
saveItem.Click += SaveItem_Click;

parentBarItem1.Items.Add(saveItem);

private void SaveItem_Click(object sender, EventArgs e)
{
    // Save logic executed by either:
    // 1. Clicking menu item
    // 2. Pressing Ctrl+S
    SaveDocument();
}
```

### VB.NET Example

```vb
Dim saveItem As New BarItem()
saveItem.Text = "Save"
saveItem.Shortcut = System.Windows.Forms.Shortcut.CtrlS
AddHandler saveItem.Click, AddressOf SaveItem_Click

parentBarItem1.Items.Add(saveItem)

Private Sub SaveItem_Click(sender As Object, e As EventArgs)
    ' Save logic executed by either:
    ' 1. Clicking menu item
    ' 2. Pressing Ctrl+S
    SaveDocument()
End Sub
```

### Common Keyboard Shortcuts

```csharp
// File operations
new BarItem { Text = "New", Shortcut = Shortcut.CtrlN, Click += New_Click },
new BarItem { Text = "Open", Shortcut = Shortcut.CtrlO, Click += Open_Click },
new BarItem { Text = "Save", Shortcut = Shortcut.CtrlS, Click += Save_Click },
new BarItem { Text = "Save As", Shortcut = Shortcut.CtrlShiftS, Click += SaveAs_Click },
new BarItem { Text = "Print", Shortcut = Shortcut.CtrlP, Click += Print_Click },

// Edit operations
new BarItem { Text = "Undo", Shortcut = Shortcut.CtrlZ, Click += Undo_Click },
new BarItem { Text = "Redo", Shortcut = Shortcut.CtrlY, Click += Redo_Click },
new BarItem { Text = "Cut", Shortcut = Shortcut.CtrlX, Click += Cut_Click },
new BarItem { Text = "Copy", Shortcut = Shortcut.CtrlC, Click += Copy_Click },
new BarItem { Text = "Paste", Shortcut = Shortcut.CtrlV, Click += Paste_Click },
new BarItem { Text = "Select All", Shortcut = Shortcut.CtrlA, Click += SelectAll_Click },

// Search operations
new BarItem { Text = "Find", Shortcut = Shortcut.CtrlF, Click += Find_Click },
new BarItem { Text = "Find Next", Shortcut = Shortcut.F3, Click += FindNext_Click },
new BarItem { Text = "Replace", Shortcut = Shortcut.CtrlH, Click += Replace_Click },

// Function keys
new BarItem { Text = "Help", Shortcut = Shortcut.F1, Click += Help_Click },
new BarItem { Text = "Rename", Shortcut = Shortcut.F2, Click += Rename_Click },
new BarItem { Text = "Refresh", Shortcut = Shortcut.F5, Click += Refresh_Click }
```

### Available Shortcut Values

The `System.Windows.Forms.Shortcut` enum provides standard Windows shortcuts:

**Ctrl Combinations:**
- `CtrlA` through `CtrlZ`
- `Ctrl0` through `Ctrl9`
- `CtrlShiftA` through `CtrlShiftZ`

**Alt Combinations:**
- `AltF1` through `AltF12`
- `AltBksp`, `AltRightArrow`, `AltLeftArrow`, etc.

**Function Keys:**
- `F1` through `F12`
- `ShiftF1` through `ShiftF12`
- `CtrlF1` through `CtrlF12`

**Special Keys:**
- `Insert`, `Delete`, `Home`, `End`
- `ShiftInsert`, `CtrlInsert`, `ShiftDelete`, `CtrlDelete`
- And many more...

### Shortcut Display

Shortcuts automatically appear on the right side of menu items:

```csharp
// Displays as: "Save                  Ctrl+S"
BarItem saveItem = new BarItem {
    Text = "Save",
    Shortcut = Shortcut.CtrlS
};
```

## Custom Shortcut Text

The `ShortcutText` property allows custom text in place of the standard shortcut display.

### Basic Custom Text

```csharp
BarItem findItem = new BarItem();
findItem.Text = "Find";
findItem.Shortcut = System.Windows.Forms.Shortcut.CtrlF;
findItem.ShortcutText = "Press Ctrl+F";  // Custom display text
findItem.Click += FindItem_Click;

parentBarItem1.Items.Add(findItem);
```

### VB.NET Example

```vb
Dim findItem As New BarItem()
findItem.Text = "Find"
findItem.Shortcut = System.Windows.Forms.Shortcut.CtrlF
findItem.ShortcutText = "Press Ctrl+F"  ' Custom display text
AddHandler findItem.Click, AddressOf FindItem_Click

parentBarItem1.Items.Add(findItem)
```

### Use Cases for Custom Shortcut Text

```csharp
// Provide additional context
new BarItem {
    Text = "Format Code",
    Shortcut = Shortcut.CtrlShiftF,
    ShortcutText = "Ctrl+Shift+F (formats entire document)"
}

// Multi-key sequences
new BarItem {
    Text = "Insert Date",
    ShortcutText = "Alt+Shift+D"  // No standard Shortcut enum value
}

// Descriptive shortcuts
new BarItem {
    Text = "Quick Actions",
    ShortcutText = "Right-click or Ctrl+."
}

// Localized shortcuts
new BarItem {
    Text = "Enregistrer",  // French "Save"
    Shortcut = Shortcut.CtrlS,
    ShortcutText = "Ctrl+S (Enregistrer)"
}
```

## Keyboard Mnemonics

Mnemonics enable keyboard navigation through menus using Alt+Key combinations. The ampersand (`&`) character designates the mnemonic letter.

### Basic Mnemonic Setup

```csharp
BarItem fileItem = new BarItem();
fileItem.Text = "&File";  // Alt+F to trigger
fileItem.ShowMnemonicUnderlinesAlways = true;  // Always show underline
fileItem.SizeToFit = true;

parentBarItem1.Items.Add(fileItem);
```

### VB.NET Example

```vb
Dim fileItem As New BarItem()
fileItem.Text = "&File"  ' Alt+F to trigger
fileItem.ShowMnemonicUnderlinesAlways = True  ' Always show underline
fileItem.SizeToFit = True

parentBarItem1.Items.Add(fileItem)
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

**Unique Letters:**
- Use unique letters within the same menu level
- Prefer first letter: &File, &Edit, &View
- Use second letter if first conflicts: &New, E&xit
- Avoid conflicts: &Copy vs. &Cut → Cu&t

**Common Patterns:**
```csharp
// Recommended mnemonics (Windows standard)
"&File"     // F
"&Edit"     // E
"&View"     // V
"&Tools"    // T
"&Help"     // H

// File menu
"&New"      // N
"&Open"     // O
"&Save"     // S
"Save &As"  // A
"&Close"    // C
"&Print"    // P
"E&xit"     // X

// Edit menu
"&Undo"     // U
"&Redo"     // R
"Cu&t"      // T
"&Copy"     // C
"&Paste"    // P
"Select &All" // A
"&Find"     // F
```

## Click Event Handling

The `Click` event fires when a menu item is activated by **any** method: mouse click, keyboard shortcut, or mnemonic.

### Basic Event Handling

```csharp
BarItem saveItem = new BarItem();
saveItem.Text = "&Save";
saveItem.Shortcut = Shortcut.CtrlS;
saveItem.Click += SaveItem_Click;

private void SaveItem_Click(object sender, EventArgs e)
{
    // This executes when user:
    // 1. Clicks "Save" in menu
    // 2. Presses Ctrl+S
    // 3. Presses Alt+F, S (if in File menu with mnemonics)
    
    SaveDocument();
}
```

### Determining Trigger Source

```csharp
private void SaveItem_Click(object sender, EventArgs e)
{
    // All trigger methods call the same handler
    // Determine source if needed (rarely necessary)
    
    if (ModifierKeys.HasFlag(Keys.Control))
    {
        // Likely triggered by Ctrl+S shortcut
        Console.WriteLine("Triggered by keyboard shortcut");
    }
    else
    {
        // Likely triggered by mouse click or mnemonic
        Console.WriteLine("Triggered by menu selection");
    }
    
    // Execute command regardless of trigger
    SaveDocument();
}
```

### Unified Command Pattern

```csharp
// Best practice: Single handler for multiple triggers
public void ExecuteSaveCommand()
{
    if (currentDocument == null) return;
    
    if (currentDocument.IsModified)
    {
        currentDocument.Save();
        UpdateTitle();
        ShowStatusMessage("Document saved");
    }
}

// Menu item
saveMenuItem.Click += (s, e) => ExecuteSaveCommand();

// Toolbar button
saveButton.Click += (s, e) => ExecuteSaveCommand();

// Keyboard shortcut (via menu)
// Automatically handled by saveMenuItem.Shortcut = Shortcut.CtrlS
```

## Complete Keyboard-Enabled Menu Example

```csharp
// Root parent
ParentBarItem rootParent = new ParentBarItem();

// File menu with mnemonics and shortcuts
ParentBarItem fileMenu = new ParentBarItem {
    Text = "&File",
    ShowMnemonicUnderlinesAlways = true,
    SizeToFit = true
};

fileMenu.Items.AddRange(new BarItem[] {
    new BarItem {
        Text = "&New",
        Shortcut = Shortcut.CtrlN,
        Image = new ImageExt(Properties.Resources.NewIcon),
        Tooltip = "Create new document (Ctrl+N)",
        ShowToolTipInPopUp = true,
        Click += (s, e) => CreateNewDocument()
    },
    new BarItem {
        Text = "&Open...",
        Shortcut = Shortcut.CtrlO,
        Image = new ImageExt(Properties.Resources.OpenIcon),
        Tooltip = "Open existing document (Ctrl+O)",
        ShowToolTipInPopUp = true,
        Click += (s, e) => OpenDocument()
    },
    new BarItem {
        Text = "&Save",
        Shortcut = Shortcut.CtrlS,
        Image = new ImageExt(Properties.Resources.SaveIcon),
        Tooltip = "Save current document (Ctrl+S)",
        ShowToolTipInPopUp = true,
        Click += (s, e) => SaveDocument()
    },
    new BarItem {
        Text = "E&xit",
        Shortcut = Shortcut.AltF4,
        Click += (s, e) => Application.Exit()
    }
});

// Edit menu with mnemonics and shortcuts
ParentBarItem editMenu = new ParentBarItem {
    Text = "&Edit",
    ShowMnemonicUnderlinesAlways = true,
    SizeToFit = true
};

editMenu.Items.AddRange(new BarItem[] {
    new BarItem {
        Text = "&Undo",
        Shortcut = Shortcut.CtrlZ,
        Image = new ImageExt(Properties.Resources.UndoIcon),
        Click += (s, e) => richTextBox1.Undo()
    },
    new BarItem {
        Text = "Cu&t",
        Shortcut = Shortcut.CtrlX,
        Image = new ImageExt(Properties.Resources.CutIcon),
        Click += (s, e) => richTextBox1.Cut()
    },
    new BarItem {
        Text = "&Copy",
        Shortcut = Shortcut.CtrlC,
        Image = new ImageExt(Properties.Resources.CopyIcon),
        Click += (s, e) => richTextBox1.Copy()
    },
    new BarItem {
        Text = "&Paste",
        Shortcut = Shortcut.CtrlV,
        Image = new ImageExt(Properties.Resources.PasteIcon),
        Click += (s, e) => richTextBox1.Paste()
    }
});

rootParent.Items.AddRange(new BarItem[] { fileMenu, editMenu });

popupMenu1.ParentBarItem = rootParent;
popupMenusManager1.SetXPContextMenu(richTextBox1, popupMenu1);
```

## Best Practices

### Keyboard Shortcuts
- Use standard Windows shortcuts (Ctrl+S, Ctrl+C, etc.)
- Don't override system shortcuts (Alt+Tab, Win+D, etc.)
- Provide shortcuts for frequently used commands
- Display shortcuts in menu items
- Include shortcuts in tooltips

### Mnemonics
- Always provide mnemonics for menu bar items
- Use unique letters within each menu level
- Follow Windows standard patterns
- Set ShowMnemonicUnderlinesAlways = true for clarity
- Test mnemonic uniqueness

### Touch Support
- Rely on default touch support (no configuration needed)
- Test on actual touch devices when possible
- Ensure adequate spacing for touch targets
- Don't disable touch support unless necessary

### Accessibility
- Provide both mouse and keyboard access
- Include tooltips with shortcut information
- Use clear, consistent mnemonic patterns
- Test with keyboard-only navigation
- Support standard Windows accessibility features

## Troubleshooting

**Issue: Shortcut doesn't work**
- Verify Click event is wired up
- Check that Shortcut property is set
- Ensure no conflicting shortcuts in application
- Verify item is enabled
- Check that form has focus

**Issue: Mnemonic doesn't trigger**
- Verify ampersand (`&`) is in Text property
- Check ShowMnemonicUnderlinesAlways setting
- Ensure unique mnemonics within menu level
- Verify menu is visible and accessible

**Issue: Click event fires twice**
- Check for duplicate event handler registration
- Verify shortcut isn't registered elsewhere
- Look for button/menu with same shortcut

**Issue: Touch doesn't work**
- Confirm touch device is properly configured
- Check that control is visible and enabled
- Verify no overlay blocks touch interaction
- Test with mouse to isolate touch-specific issues
