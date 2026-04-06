# Keyboard Shortcuts and Touch Support

## Table of Contents
- [Overview](#overview)
- [Keyboard Shortcuts](#keyboard-shortcuts)
- [Touch Mode Support](#touch-mode-support)
- [Accessibility Considerations](#accessibility-considerations)
- [Best Practices](#best-practices)

## Overview

ContextMenuStripEx supports both keyboard shortcuts and touch-friendly interactions, making menus accessible to users across different input methods:

- **Keyboard Shortcuts:** Allow users to trigger menu actions via key combinations
- **Touch Mode:** Optimizes menu sizing and spacing for touch devices
- **Keyboard Navigation:** Standard arrow key and Enter/Esc navigation

## Keyboard Shortcuts

Keyboard shortcuts (also called accelerator keys or hotkeys) provide quick access to menu commands without using the mouse.

### Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `ShortcutKeys` | Keys enum | The key combination that triggers the menu item |
| `ShowShortcutKeys` | bool | Whether to display the shortcut text in the menu |
| `ShortcutKeyDisplayString` | string | Custom text to display instead of auto-generated shortcut text |

### Important Notes

1. **MenuItem only:** Shortcuts only work with ToolStripMenuItem (not TextBox or ComboBox)
2. **Click event:** Shortcuts trigger the same Click event as mouse clicks
3. **Form-level:** Shortcuts work application-wide when the form has focus
4. **Keys enum:** Use System.Windows.Forms.Keys for key combinations

### Setting Keyboard Shortcuts

**Via Designer:**
1. Select the menu item
2. In Properties panel → Behavior section
3. Click dropdown for **ShortcutKeys** property
4. Select modifier keys (Ctrl, Alt, Shift)
5. Select the main key
6. Set **ShowShortcutKeys** to True to display it

**Via Code:**
```csharp
using System.Windows.Forms;

// Create menu item with shortcut
var cutItem = new ToolStripMenuItem("Cut");
cutItem.ShortcutKeys = Keys.Control | Keys.X;
cutItem.ShowShortcutKeys = true;
cutItem.Click += (s, e) => PerformCut();

var copyItem = new ToolStripMenuItem("Copy");
copyItem.ShortcutKeys = Keys.Control | Keys.C;
copyItem.ShowShortcutKeys = true;
copyItem.Click += (s, e) => PerformCopy();

var pasteItem = new ToolStripMenuItem("Paste");
pasteItem.ShortcutKeys = Keys.Control | Keys.V;
pasteItem.ShowShortcutKeys = true;
pasteItem.Click += (s, e) => PerformPaste();
```

**VB.NET:**
```vb
Imports System.Windows.Forms

' Create menu item with shortcut
Dim cutItem As New ToolStripMenuItem("Cut")
cutItem.ShortcutKeys = Keys.Control Or Keys.X
cutItem.ShowShortcutKeys = True
AddHandler cutItem.Click, Sub(s, e) PerformCut()

Dim copyItem As New ToolStripMenuItem("Copy")
copyItem.ShortcutKeys = Keys.Control Or Keys.C
copyItem.ShowShortcutKeys = True
AddHandler copyItem.Click, Sub(s, e) PerformCopy()

Dim pasteItem As New ToolStripMenuItem("Paste")
pasteItem.ShortcutKeys = Keys.Control Or Keys.V
pasteItem.ShowShortcutKeys = True
AddHandler pasteItem.Click, Sub(s, e) PerformPaste()
```

### Common Key Combinations

**Standard Editing:**
```csharp
// Cut, Copy, Paste
cutItem.ShortcutKeys = Keys.Control | Keys.X;
copyItem.ShortcutKeys = Keys.Control | Keys.C;
pasteItem.ShortcutKeys = Keys.Control | Keys.V;

// Undo, Redo
undoItem.ShortcutKeys = Keys.Control | Keys.Z;
redoItem.ShortcutKeys = Keys.Control | Keys.Y;

// Select All
selectAllItem.ShortcutKeys = Keys.Control | Keys.A;
```

**File Operations:**
```csharp
newItem.ShortcutKeys = Keys.Control | Keys.N;
openItem.ShortcutKeys = Keys.Control | Keys.O;
saveItem.ShortcutKeys = Keys.Control | Keys.S;
saveAsItem.ShortcutKeys = Keys.Control | Keys.Shift | Keys.S;
```

**Search:**
```csharp
findItem.ShortcutKeys = Keys.Control | Keys.F;
findNextItem.ShortcutKeys = Keys.F3;
replaceItem.ShortcutKeys = Keys.Control | Keys.H;
```

**Function Keys:**
```csharp
helpItem.ShortcutKeys = Keys.F1;
renameItem.ShortcutKeys = Keys.F2;
refreshItem.ShortcutKeys = Keys.F5;
propertiesItem.ShortcutKeys = Keys.F4;
```

**Special Keys:**
```csharp
deleteItem.ShortcutKeys = Keys.Delete;
exitItem.ShortcutKeys = Keys.Alt | Keys.F4;
```

### Custom Shortcut Display Text

Use `ShortcutKeyDisplayString` to override the auto-generated shortcut text:

**C# Example:**
```csharp
var saveItem = new ToolStripMenuItem("Save");
saveItem.ShortcutKeys = Keys.Control | Keys.S;
saveItem.ShowShortcutKeys = true;
saveItem.ShortcutKeyDisplayString = "Ctrl+S (Quick Save)";

var exportItem = new ToolStripMenuItem("Export");
exportItem.ShortcutKeys = Keys.Control | Keys.E;
exportItem.ShowShortcutKeys = true;
exportItem.ShortcutKeyDisplayString = "Press Ctrl+E";

var customItem = new ToolStripMenuItem("Custom Action");
// No actual shortcut, just display text
customItem.ShowShortcutKeys = true;
customItem.ShortcutKeyDisplayString = "See Help";
```

**Use Cases for Custom Display:**
- Add contextual information: "Ctrl+S (Auto-save enabled)"
- Different terminology: "Press Ctrl+N" vs "Ctrl+N"
- Indicate special states: "F5 (Currently disabled)"
- Show alternative methods: "Ctrl+C or middle-click"

### Keys Enum Reference

**Modifier Keys (combine with |):**
- `Keys.Control` - Ctrl key
- `Keys.Alt` - Alt key
- `Keys.Shift` - Shift key

**Letter Keys:**
- `Keys.A` through `Keys.Z`

**Number Keys:**
- `Keys.D0` through `Keys.D9` (main keyboard)
- `Keys.NumPad0` through `Keys.NumPad9` (numeric keypad)

**Function Keys:**
- `Keys.F1` through `Keys.F24`

**Special Keys:**
- `Keys.Enter`, `Keys.Escape`, `Keys.Space`
- `Keys.Delete`, `Keys.Insert`, `Keys.Back` (Backspace)
- `Keys.Home`, `Keys.End`, `Keys.PageUp`, `Keys.PageDown`
- `Keys.Left`, `Keys.Right`, `Keys.Up`, `Keys.Down`

**Full list:** [Microsoft Keys Enum Documentation](https://learn.microsoft.com/en-us/dotnet/api/system.windows.forms.keys)

### Complete Example with Multiple Shortcuts

**C# Example:**
```csharp
private ContextMenuStripEx CreateShortcutMenu()
{
    var contextMenu = new ContextMenuStripEx();
    
    // File operations
    var newItem = new ToolStripMenuItem("New");
    newItem.ShortcutKeys = Keys.Control | Keys.N;
    newItem.ShowShortcutKeys = true;
    newItem.Click += (s, e) => CreateNew();
    
    var openItem = new ToolStripMenuItem("Open");
    openItem.ShortcutKeys = Keys.Control | Keys.O;
    openItem.ShowShortcutKeys = true;
    openItem.Click += (s, e) => OpenFile();
    
    var saveItem = new ToolStripMenuItem("Save");
    saveItem.ShortcutKeys = Keys.Control | Keys.S;
    saveItem.ShowShortcutKeys = true;
    saveItem.Click += (s, e) => SaveFile();
    
    var separator1 = new ToolStripSeparator();
    
    // Edit operations
    var cutItem = new ToolStripMenuItem("Cut");
    cutItem.ShortcutKeys = Keys.Control | Keys.X;
    cutItem.ShowShortcutKeys = true;
    cutItem.Click += (s, e) => Cut();
    
    var copyItem = new ToolStripMenuItem("Copy");
    copyItem.ShortcutKeys = Keys.Control | Keys.C;
    copyItem.ShowShortcutKeys = true;
    copyItem.Click += (s, e) => Copy();
    
    var pasteItem = new ToolStripMenuItem("Paste");
    pasteItem.ShortcutKeys = Keys.Control | Keys.V;
    pasteItem.ShowShortcutKeys = true;
    pasteItem.Click += (s, e) => Paste();
    
    var separator2 = new ToolStripSeparator();
    
    // Search
    var findItem = new ToolStripMenuItem("Find");
    findItem.ShortcutKeys = Keys.Control | Keys.F;
    findItem.ShowShortcutKeys = true;
    findItem.Click += (s, e) => ShowFindDialog();
    
    contextMenu.Items.AddRange(new ToolStripItem[] {
        newItem, openItem, saveItem, separator1,
        cutItem, copyItem, pasteItem, separator2,
        findItem
    });
    
    return contextMenu;
}
```

## Touch Mode Support

Touch mode optimizes ContextMenuStripEx for touch input devices by increasing spacing and sizing for easier tap targets.

### EnableTouchMode Property

**Property:** `EnableTouchMode` (bool)  
**Default:** `false`  
**Applies to:** ContextMenuStripEx control

**When enabled:**
- Menu items have larger hit targets
- Increased spacing between items
- Better touch interaction feedback
- Optimized for tablet and touch screen devices

### Enabling Touch Mode

**Via Designer:**
1. Select ContextMenuStripEx in component tray
2. In Properties panel, locate **EnableTouchMode**
3. Set to True

**Via Code:**
```csharp
// Enable touch mode
contextMenuStripEx.EnableTouchMode = true;
```

**VB.NET:**
```vb
' Enable touch mode
contextMenuStripEx.EnableTouchMode = True
```

### When to Use Touch Mode

**Enable touch mode when:**
- Application runs on tablets or touch screen devices
- Users primarily use touch input
- Application targets touch-first environments (Surface, iPad with Windows)
- Accessibility is important (larger targets benefit all users)

**Keep disabled when:**
- Application is mouse/keyboard only
- Screen space is limited
- Traditional desktop environment with no touch hardware

### Example: Auto-Detect Touch Support

Automatically enable touch mode if touch hardware is detected:

**C# Example:**
```csharp
using System.Windows.Forms;

private void InitializeContextMenu()
{
    var contextMenu = new ContextMenuStripEx();
    
    // Auto-detect touch support
    int touchPointsSupported = SystemInformation.MaximumTouches;
    if (touchPointsSupported > 0)
    {
        contextMenu.EnableTouchMode = true;
    }
    
    // Add menu items...
    var item1 = new ToolStripMenuItem("Option 1");
    var item2 = new ToolStripMenuItem("Option 2");
    var item3 = new ToolStripMenuItem("Option 3");
    
    contextMenu.Items.AddRange(new ToolStripItem[] { item1, item2, item3 });
    
    this.textBox1.ContextMenuStrip = contextMenu;
}
```

### Touch Mode Best Practices

1. **Test on actual devices:** Emulators don't fully represent touch experience
2. **Combine with larger fonts:** Touch mode works best with readable text sizes
3. **Limit menu items:** Touch menus work best with 5-10 items per level
4. **Provide touch feedback:** Ensure hover states are clear on touch
5. **Consider swipe gestures:** Touch users may expect swipe-to-close

## Accessibility Considerations

### Keyboard Navigation

Standard Windows keyboard navigation works with ContextMenuStripEx:

**Navigation Keys:**
- **Tab:** Cycle through focusable items (TextBox, ComboBox)
- **Arrow Up/Down:** Move between menu items
- **Arrow Right:** Open submenu (if item has DropDownItems)
- **Arrow Left:** Close submenu and return to parent
- **Enter:** Activate selected item
- **Escape:** Close current menu level
- **First letter:** Jump to first item starting with that letter

**Example Menu with Navigation:**
```csharp
var contextMenu = new ContextMenuStripEx();

// Items can be accessed by first letter
var apple = new ToolStripMenuItem("Apple");      // Press 'A'
var banana = new ToolStripMenuItem("Banana");    // Press 'B'
var cherry = new ToolStripMenuItem("Cherry");    // Press 'C'

contextMenu.Items.AddRange(new ToolStripItem[] { apple, banana, cherry });
```

### Mnemonic Keys (Underlined Letters)

Add & before a letter in Text property to create mnemonic:

```csharp
var fileMenu = new ToolStripMenuItem("&File");     // Alt+F
var editMenu = new ToolStripMenuItem("&Edit");     // Alt+E
var viewMenu = new ToolStripMenuItem("&View");     // Alt+V

var newItem = new ToolStripMenuItem("&New");       // N when menu open
var openItem = new ToolStripMenuItem("&Open");     // O when menu open
var saveItem = new ToolStripMenuItem("&Save");     // S when menu open
```

**Note:** Mnemonics work differently in context menus vs menu bars:
- In MenuStrip: Alt+letter activates the menu
- In ContextMenuStrip: Letter key activates when menu is already open

### Screen Reader Support

ContextMenuStripEx supports screen readers automatically:
- Menu structure is announced (parent/child relationships)
- Item states are announced (checked, disabled)
- Shortcuts are announced when ShowShortcutKeys is true

**Enhance screen reader support:**
```csharp
var item = new ToolStripMenuItem("Delete");
item.ToolTipText = "Delete the selected item permanently";
item.ShortcutKeys = Keys.Delete;
item.ShowShortcutKeys = true;
// Screen reader announces: "Delete, Delete key, Delete the selected item permanently"
```

### High Contrast Support

ContextMenuStripEx automatically adapts to Windows High Contrast themes.

**Test accessibility:**
1. Enable High Contrast in Windows settings
2. Test keyboard navigation without mouse
3. Enable screen reader (Narrator) and verify announcements
4. Verify touch targets are 44x44 pixels minimum (WCAG 2.1 AAA)

## Best Practices

### Keyboard Shortcuts
1. **Use standard shortcuts:** Follow Windows conventions (Ctrl+C for Copy, etc.)
2. **Be consistent:** Use same shortcuts across your application
3. **Document shortcuts:** Show them with ShowShortcutKeys = true
4. **Provide alternatives:** Don't require shortcuts as only access method

### Touch Mode
1. **Auto-detect:** Check SystemInformation.MaximumTouches
2. **Test on hardware:** Emulators don't match real touch experience
3. **Use larger targets:** Combine with appropriate fonts and spacing

### Accessibility
1. **Always show shortcuts:** Set ShowShortcutKeys = true
2. **Provide keyboard alternatives:** Every mouse action needs keyboard equivalent
3. **Test with Narrator:** Verify screen reader experience
4. **Support high contrast:** Test in High Contrast themes

## Troubleshooting

**Shortcuts not working:** Verify ShortcutKeys set; check form has focus and Click handler attached  
**Shortcut text not displaying:** Ensure ShowShortcutKeys = true and item has sufficient width  
**Touch mode not working:** Verify EnableTouchMode = true on ContextMenuStripEx  
**Keyboard navigation not working:** Verify items enabled and form has proper focus
