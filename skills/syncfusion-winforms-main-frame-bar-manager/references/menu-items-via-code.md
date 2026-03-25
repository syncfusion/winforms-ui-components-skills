# Adding Menu Items via Code

This guide covers programmatic creation of menus, toolbars, and menu items using C# or VB.NET code, providing full control over the menu structure.

## Creating Bar Instances

A Bar is a container that represents a menu, toolbar, or status bar in the MainFrameBarManager.

### Basic Bar Creation

```csharp
// Create a Bar instance
Bar fileBar = new Syncfusion.Windows.Forms.Tools.XPMenus.Bar();

// Configure bar properties
fileBar.BarName = "File";
fileBar.Caption = "File";
fileBar.Manager = mainFrameBarManager1;

// Add bar to manager
mainFrameBarManager1.Bars.Add(fileBar);

// Add to categories for customization UI
mainFrameBarManager1.Categories.Add("Menu");
```

### VB.NET Equivalent

```vb
' Create a Bar instance
Dim fileBar As New Syncfusion.Windows.Forms.Tools.XPMenus.Bar()

' Configure bar properties
fileBar.BarName = "File"
fileBar.Caption = "File"
fileBar.Manager = mainFrameBarManager1

' Add bar to manager
mainFrameBarManager1.Bars.Add(fileBar)

' Add to categories
mainFrameBarManager1.Categories.Add("Menu")
```

## Creating Parent Menu Items

ParentBarItem acts as a container for sub-menu items and can have a hierarchical structure.

### Basic Parent Item

```csharp
// Create parent item
ParentBarItem fileMenu = new ParentBarItem();
fileMenu.Text = "&File";
fileMenu.Category = "Menu";

// Create child items
BarItem newItem = new BarItem() { Text = "&New" };
BarItem openItem = new BarItem() { Text = "&Open" };
BarItem saveItem = new BarItem() { Text = "&Save" };
BarItem exitItem = new BarItem() { Text = "E&xit" };

// Add children to parent
fileMenu.Items.AddRange(new BarItem[] { newItem, openItem, saveItem, exitItem });

// Add all items to manager
mainFrameBarManager1.Items.AddRange(new BarItem[] 
{ 
    fileMenu, newItem, openItem, saveItem, exitItem 
});

// Add parent to bar
fileBar.Items.Add(fileMenu);
```

## Creating Nested Sub-Menus

Create multi-level menu hierarchies by nesting ParentBarItems:

```csharp
// Create main menu
ParentBarItem fileMenu = new ParentBarItem() { Text = "&File" };

// Create submenu
ParentBarItem recentMenu = new ParentBarItem() { Text = "&Recent" };

// Create sub-submenu items
BarItem doc1Item = new BarItem() { Text = "Document1.txt" };
BarItem doc2Item = new BarItem() { Text = "Document2.txt" };
BarItem doc3Item = new BarItem() { Text = "Document3.txt" };

// Build hierarchy
recentMenu.Items.AddRange(new BarItem[] { doc1Item, doc2Item, doc3Item });
fileMenu.Items.Add(recentMenu);
fileBar.Items.Add(fileMenu);

// Add all to manager
mainFrameBarManager1.Items.AddRange(new BarItem[] 
{ 
    fileMenu, recentMenu, doc1Item, doc2Item, doc3Item 
});
```

## Adding Event Handlers

Respond to menu item clicks by subscribing to the ItemClick event:

```csharp
// Create menu item
BarItem saveItem = new BarItem() { Text = "&Save" };

// Add event handler
saveItem.ItemClick += (sender, args) =>
{
    MessageBox.Show("Save clicked!");
};

mainFrameBarManager1.Items.Add(saveItem);
fileBar.Items.Add(saveItem);
```

## Complete Menu Example

Here's a complete example building a File menu with New, Open, Save, Recent, and Exit:

```csharp
// Initialize manager
MainFrameBarManager menuManager = new MainFrameBarManager();
menuManager.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016Colorful;
menuManager.Form = this;

// Create File bar
Bar fileBar = new Bar() { BarName = "File", Caption = "File", Manager = menuManager };

// Create main File menu
ParentBarItem fileMenu = new ParentBarItem() { Text = "&File" };

// Create basic items
BarItem newItem = new BarItem() { Text = "&New" };
BarItem openItem = new BarItem() { Text = "&Open" };
BarItem saveItem = new BarItem() { Text = "&Save" };

// Create Recent submenu
ParentBarItem recentMenu = new ParentBarItem() { Text = "&Recent" };
BarItem recent1 = new BarItem() { Text = "File1.txt" };
BarItem recent2 = new BarItem() { Text = "File2.txt" };
recentMenu.Items.AddRange(new BarItem[] { recent1, recent2 });

// Create Exit item
BarItem exitItem = new BarItem() { Text = "E&xit" };

// Build file menu hierarchy
fileMenu.Items.AddRange(new BarItem[] { newItem, openItem, saveItem, recentMenu, exitItem });

// Add all items to manager
menuManager.Items.AddRange(new BarItem[] 
{ 
    fileMenu, newItem, openItem, saveItem, recentMenu, recent1, recent2, exitItem 
});

// Add menu to bar, bar to manager
fileBar.Items.Add(fileMenu);
menuManager.Bars.Add(fileBar);
menuManager.Categories.Add("Menu");

// Add event handlers
newItem.ItemClick += (s, e) => OnFileNew();
openItem.ItemClick += (s, e) => OnFileOpen();
saveItem.ItemClick += (s, e) => OnFileSave();
exitItem.ItemClick += (s, e) => this.Close();
```

## Key Properties for Menu Items

| Property | Type | Description |
|----------|------|-------------|
| **Text** | string | Display text; use & for mnemonics (e.g., "&Save" → Alt+S) |
| **Category** | string | Organizational category for customization UI |
| **Manager** | MainFrameBarManager | Parent manager instance |
| **Items** | BarItemCollection | Child items (ParentBarItem only) |
| **Shortcut** | Shortcut enum | Keyboard shortcut |
| **Checked** | bool | Whether item shows checked state |
| **Enabled** | bool | Whether item is clickable |
| **Tooltip** | SuperToolTip | Associated tooltip control |
| **ShowTooltip** | bool | Whether to display tooltip |
| **ShowMnemonicUnderlinesAlways** | bool | Always show mnemonic underlines |

## Best Practices

1. **Add to Manager First:** Always add items to mainFrameBarManager1.Items collection before adding to bars.
2. **Consistent Naming:** Use BarName property to distinguish between menus, toolbars, and status bars.
3. **Organize by Categories:** Set Category property for items to organize them in customization dialogs.
4. **Use Mnemonics:** Add & symbol before frequently-used letters for keyboard access.
5. **Group Related Items:** Use ParentBarItem to logically group related menu items.
6. **Separator Items:** Use StaticBarItem with "-" text to create visual separators.
7. **Event Handlers:** Attach ItemClick handlers to respond to user interactions.

## Common Patterns

**Menu Separator:**
```csharp
BarItem separator = new StaticBarItem() { Text = "-" };
fileMenu.Items.Add(separator);
```

**Disabled Item:**
```csharp
BarItem disabledItem = new BarItem() { Text = "Unavailable", Enabled = false };
```

**Checked Menu Item:**
```csharp
BarItem checkableItem = new BarItem() { Text = "Show Grid", Checked = true };
```
