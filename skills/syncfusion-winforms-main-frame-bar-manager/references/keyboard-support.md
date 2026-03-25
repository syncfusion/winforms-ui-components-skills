# Keyboard Support

This guide covers implementing keyboard shortcuts and mnemonics in MainFrameBarManager menu items for keyboard-driven navigation and accessibility.

## Keyboard Shortcuts

Shortcuts provide one-key access to frequently-used menu items without navigating the menu structure.

### Assigning Shortcuts

Use the Shortcut property to assign keyboard shortcuts to menu items:

```csharp
// Create menu items with shortcuts
BarItem newItem = new BarItem() { Text = "&New" };
BarItem openItem = new BarItem() { Text = "&Open" };
BarItem saveItem = new BarItem() { Text = "&Save" };
BarItem printItem = new BarItem() { Text = "&Print" };
BarItem exitItem = new BarItem() { Text = "E&xit" };

// Assign shortcuts
newItem.Shortcut = Shortcut.CtrlN;
openItem.Shortcut = Shortcut.CtrlO;
saveItem.Shortcut = Shortcut.CtrlS;
printItem.Shortcut = Shortcut.CtrlP;
exitItem.Shortcut = Shortcut.AltF4;

mainFrameBarManager1.Items.AddRange(new BarItem[] 
{ 
    newItem, openItem, saveItem, printItem, exitItem 
});
```

### Common Shortcuts

| Shortcut | Enum Value | Usage |
|----------|------------|-------|
| Ctrl+N | Shortcut.CtrlN | New |
| Ctrl+O | Shortcut.CtrlO | Open |
| Ctrl+S | Shortcut.CtrlS | Save |
| Ctrl+P | Shortcut.CtrlP | Print |
| Ctrl+X | Shortcut.CtrlX | Cut |
| Ctrl+C | Shortcut.CtrlC | Copy |
| Ctrl+V | Shortcut.CtrlV | Paste |
| Ctrl+Z | Shortcut.CtrlZ | Undo |
| Ctrl+Y | Shortcut.CtrlY | Redo |
| F1 | Shortcut.F1 | Help |
| F5 | Shortcut.F5 | Refresh |
| F11 | Shortcut.F11 | Full Screen |
| Alt+F4 | Shortcut.AltF4 | Exit |
| Delete | Shortcut.Del | Delete |

### Displaying Shortcuts in Menus

Shortcuts automatically display in menus next to the item text:

```
File
  New                    Ctrl+N
  Open                   Ctrl+O
  Save                   Ctrl+S
  --------
  Exit                   Alt+F4
```

To hide shortcut display (if needed):

```csharp
// This property may need to be set on the manager level
// mainFrameBarManager1.ShowShortcutText = false;  // If available
```

### VB.NET Shortcut Assignment

```vb
' Assign shortcuts in VB.NET
newItem.Shortcut = Shortcut.CtrlN
openItem.Shortcut = Shortcut.CtrlO
saveItem.Shortcut = Shortcut.CtrlS
```

---

## Mnemonics

Mnemonics are underlined characters in menu text that allow keyboard access while the menu is open. Users press the underlined letter after opening the menu.

### Adding Mnemonics

Use the & symbol in Text property to designate the mnemonic character:

```csharp
// & symbol precedes the mnemonic character
BarItem newItem = new BarItem() { Text = "&New" };
BarItem openItem = new BarItem() { Text = "&Open" };
BarItem saveItem = new BarItem() { Text = "&Save" };
BarItem cutItem = new BarItem() { Text = "Cu&t" };
BarItem copyItem = new BarItem() { Text = "&Copy" };
BarItem pasteItem = new BarItem() { Text = "&Paste" };

// Mnemonic characters: N, O, S, T, C, P (respectively)
```

### Mnemonic Display

By default, mnemonics are hidden until Alt is pressed. Make them always visible:

```csharp
BarItem editItem = new BarItem() { Text = "&Edit" };

// Always show mnemonic underlines
editItem.ShowMnemonicUnderlinesAlways = true;

mainFrameBarManager1.Items.Add(editItem);
```

### Accessing Menus via Mnemonic

User workflow:
1. User presses Alt key → menu mnemonics appear underlined
2. User presses mnemonic character (e.g., F for "&File")
3. File menu opens
4. User presses another mnemonic (e.g., O for "&Open")
5. Open dialog appears

Example menu structure:
```
&File menu (Alt+F)
  &New           (Alt+F, N)
  &Open          (Alt+F, O)
  &Save          (Alt+F, S)
  &Exit          (Alt+F, X)

&Edit menu (Alt+E)
  Cu&t           (Alt+E, T)
  &Copy          (Alt+E, C)
  &Paste         (Alt+E, P)
```

### Best Practices for Mnemonics

1. **Unique per Menu:** Use different mnemonic letters within the same menu level
2. **Logical Letters:** Choose the first letter of the action when possible (&File, &Save)
3. **Consistent Positions:** Keep mnemonic in same position across related items
4. **Avoid Numbers:** Don't use numbers as mnemonics
5. **Avoid Conflicts:** Don't duplicate mnemonics in same parent menu

### Common Mnemonic Assignments

| Menu Item | Text | Mnemonic |
|-----------|------|----------|
| New | &New | N |
| Open | &Open | O |
| Save | &Save | S |
| Save As | "Save &As" | A |
| Exit | "E&xit" | X |
| Cut | "Cu&t" | T |
| Copy | "&Copy" | C |
| Paste | "&Paste" | P |
| Edit | "&Edit" | E |
| View | "&View" | V |
| Tools | "&Tools" | T |
| Help | "&Help" | H |

### Handling Mnemonic Conflicts

When multiple items need the same mnemonic letter, choose different characters:

```csharp
// File menu - use F for File
ParentBarItem fileMenu = new ParentBarItem() { Text = "&File" };

// First occurrence of letter gets first priority
BarItem newItem = new BarItem() { Text = "&New" };      // N
BarItem nextItem = new BarItem() { Text = "N&ext" };    // E (alternate position)
BarItem exitItem = new BarItem() { Text = "E&xit" };    // X
```

---

## Keyboard Navigation Patterns

### Menu Opening with Alt

The standard Windows convention is Alt to activate menu:

```csharp
// Handled automatically by MainFrameBarManager
// User presses Alt key → first menu becomes active
```

### Arrow Keys for Navigation

Once menu is open:
- **Down Arrow:** Move to next menu item
- **Up Arrow:** Move to previous menu item
- **Right Arrow:** Open submenu or next top-level menu
- **Left Arrow:** Close submenu or previous top-level menu
- **Enter:** Execute selected menu item
- **Escape:** Close menu

All these are handled automatically by MainFrameBarManager.

### Tab Between Controls

Tab key navigation between toolbar controls:

```csharp
// ComboBoxBarItem
ComboBoxBarItem fontSizeItem = new ComboBoxBarItem() { Text = "Size:" };
fontSizeItem.ChoiceList.AddRange(new string[] { "8", "10", "12", "14", "16" });

// TextBoxBarItem
TextBoxBarItem searchItem = new TextBoxBarItem() { Text = "Search:" };

// Tab between these controls when in toolbar
mainFrameBarManager1.Bars[0].Items.AddRange(new BarItem[] { fontSizeItem, searchItem });
```

---

## Implementation Example

Complete example with shortcuts and mnemonics:

```csharp
// Create menu manager
MainFrameBarManager menuMgr = new MainFrameBarManager();
menuMgr.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016Colorful;
menuMgr.Form = this;

// Create File bar
Bar fileBar = new Bar() { BarName = "File", Caption = "File", Manager = menuMgr };

// Create File menu
ParentBarItem fileMenu = new ParentBarItem() { Text = "&File" };

// Add items with mnemonics and shortcuts
BarItem newItem = new BarItem() 
{ 
    Text = "&New", 
    Shortcut = Shortcut.CtrlN,
    ShowMnemonicUnderlinesAlways = false  // Show on Alt
};

BarItem openItem = new BarItem() 
{ 
    Text = "&Open", 
    Shortcut = Shortcut.CtrlO 
};

BarItem saveItem = new BarItem() 
{ 
    Text = "&Save", 
    Shortcut = Shortcut.CtrlS 
};

BarItem exitItem = new BarItem() 
{ 
    Text = "E&xit", 
    Shortcut = Shortcut.AltF4 
};

// Build hierarchy
fileMenu.Items.AddRange(new BarItem[] { newItem, openItem, saveItem, exitItem });

// Add to manager
menuMgr.Items.AddRange(new BarItem[] { fileMenu, newItem, openItem, saveItem, exitItem });

// Add to bar
fileBar.Items.Add(fileMenu);
menuMgr.Bars.Add(fileBar);

// Add event handlers
newItem.ItemClick += (s, e) => CreateNewDocument();
openItem.ItemClick += (s, e) => OpenDocument();
saveItem.ItemClick += (s, e) => SaveDocument();
exitItem.ItemClick += (s, e) => this.Close();
```

### Result

Menu displays as:
```
&File (Alt+F or just Alt)
  &New          Ctrl+N
  &Open         Ctrl+O
  &Save         Ctrl+S
  E&xit         Alt+F4
```

Users can access via:
- Keyboard shortcuts: Ctrl+N, Ctrl+O, Ctrl+S
- Menu mnemonics: Alt+F, N (for New)
- Direct menu: Click File → New

---

## Accessibility Considerations

Keyboard support is essential for accessibility:

1. **Shortcuts for Common Tasks:** Users with motor disabilities rely on shortcuts
2. **Consistent Mnemonics:** Predictable mnemonic placement helps power users
3. **Visual Feedback:** ShowMnemonicUnderlinesAlways helps users learn shortcuts
4. **Tooltip with Shortcuts:** Include shortcut info in tooltips to help discoverability

Example accessible implementation:

```csharp
BarItem saveItem = new BarItem() 
{ 
    Text = "&Save", 
    Shortcut = Shortcut.CtrlS,
    ShowMnemonicUnderlinesAlways = true  // Always visible for learning
};

// Tooltip includes shortcut info
ToolTipInfo tip = new ToolTipInfo();
tip.Body.Text = "Save the current document";
tip.Footer.Text = "Keyboard: Ctrl+S (or Alt+F, S)";
superToolTip1.SetToolTip(saveItem, tip);
```

This helps all users, particularly those with disabilities, discover and use keyboard navigation effectively.
