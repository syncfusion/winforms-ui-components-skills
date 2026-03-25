# Advanced Features in PopupMenu

## Table of Contents
- [Partial Menus](#partial-menus)
- [Bar Manager Integration](#bar-manager-integration)
- [Events](#events)
- [Dynamic Menu Customization](#dynamic-menu-customization)

## Partial Menus

Partial menus prioritize frequently used items while temporarily hiding less common ones. Users can expand to see all items.

### Overview

The partial menus feature displays only the most frequently used commands initially, with an expand control to reveal hidden items. This reduces visual clutter while maintaining access to all commands.

**Key Properties:**
- `UsePartialMenus` (ParentBarItem) - Enable partial menu mode
- `IsRecentlyUsedItem` (BarItem) - Mark item as frequently used

### Enabling Partial Menus

```csharp
ParentBarItem parentBarItem1 = new ParentBarItem();
parentBarItem1.UsePartialMenus = true;  // Enable partial menus

// Add menu items
BarItem newItem = new BarItem { Text = "New", Shortcut = Shortcut.CtrlN };
BarItem openItem = new BarItem { Text = "Open", Shortcut = Shortcut.CtrlO };
BarItem saveItem = new BarItem { Text = "Save", Shortcut = Shortcut.CtrlS };
BarItem saveAsItem = new BarItem { Text = "Save As..." };
BarItem printItem = new BarItem { Text = "Print", Shortcut = Shortcut.CtrlP };
BarItem optionsItem = new BarItem { Text = "Options..." };
BarItem exitItem = new BarItem { Text = "Exit" };

parentBarItem1.Items.AddRange(new BarItem[] {
    newItem, openItem, saveItem, saveAsItem, printItem, optionsItem, exitItem
});

// Mark frequently used items (visible by default)
newItem.IsRecentlyUsedItem = true;
openItem.IsRecentlyUsedItem = true;
saveItem.IsRecentlyUsedItem = true;
exitItem.IsRecentlyUsedItem = true;

// Mark less frequently used items (hidden initially)
saveAsItem.IsRecentlyUsedItem = false;
printItem.IsRecentlyUsedItem = false;
optionsItem.IsRecentlyUsedItem = false;

popupMenu1.ParentBarItem = parentBarItem1;
```

### VB.NET Example

```vb
Dim parentBarItem1 As New ParentBarItem()
parentBarItem1.UsePartialMenus = True  ' Enable partial menus

' Add menu items
Dim newItem As New BarItem With {.Text = "New", .Shortcut = Shortcut.CtrlN}
Dim openItem As New BarItem With {.Text = "Open", .Shortcut = Shortcut.CtrlO}
Dim saveItem As New BarItem With {.Text = "Save", .Shortcut = Shortcut.CtrlS}
Dim saveAsItem As New BarItem With {.Text = "Save As..."}
Dim printItem As New BarItem With {.Text = "Print", .Shortcut = Shortcut.CtrlP}
Dim optionsItem As New BarItem With {.Text = "Options..."}
Dim exitItem As New BarItem With {.Text = "Exit"}

parentBarItem1.Items.AddRange(New BarItem() { _
    newItem, openItem, saveItem, saveAsItem, printItem, optionsItem, exitItem})

' Mark frequently used items
newItem.IsRecentlyUsedItem = True
openItem.IsRecentlyUsedItem = True
saveItem.IsRecentlyUsedItem = True
exitItem.IsRecentlyUsedItem = True

' Mark less frequently used items
saveAsItem.IsRecentlyUsedItem = False
printItem.IsRecentlyUsedItem = False
optionsItem.IsRecentlyUsedItem = False

popupMenu1.ParentBarItem = parentBarItem1
```

### Default Behavior

By default, `IsRecentlyUsedItem = true` for all items. To use partial menus effectively:
1. Enable `UsePartialMenus = true` on ParentBarItem
2. Explicitly set `IsRecentlyUsedItem = false` for items to hide initially

### Dynamic Usage Tracking

Implement automatic usage tracking:

```csharp
private Dictionary<BarItem, int> menuItemUsage = new Dictionary<BarItem, int>();
private const int FREQUENTLY_USED_THRESHOLD = 5;

private void TrackMenuItemUsage(BarItem item)
{
    if (!menuItemUsage.ContainsKey(item))
    {
        menuItemUsage[item] = 0;
    }
    
    menuItemUsage[item]++;
    
    // Update IsRecentlyUsedItem based on usage
    item.IsRecentlyUsedItem = menuItemUsage[item] >= FREQUENTLY_USED_THRESHOLD;
}

// Wire up tracking to all items
foreach (BarItem item in parentBarItem1.Items)
{
    EventHandler clickHandler = (s, e) => {
        TrackMenuItemUsage(item);
        // Original click logic here
    };
    item.Click += clickHandler;
}
```

### Use Cases

**When to Use Partial Menus:**
- Long menu lists (10+ items)
- Mix of common and specialized commands
- Professional/power-user applications
- Following Microsoft Office menu patterns

**When to Skip:**
- Short menus (< 8 items)
- All items equally important
- Simple applications
- Users prefer seeing all options immediately

## Bar Manager Integration

Integrate PopupMenu with Bar Manager for application-wide menu systems and CommandBar dropdown menus.

### Overview

Bar Manager (`CommandBarController`) provides centralized menu management for applications. PopupMenu can be associated with CommandBar controls to create dropdown menus in toolbars.

### CommandBar Association

```csharp
// Initialize components
CommandBarController commandBarController1 = new CommandBarController(this.components);
CommandBar commandBar1 = new CommandBar();
PopupMenu popupMenu1 = new PopupMenu(this.components);
ParentBarItem parentBarItem1 = new ParentBarItem();
BarItem barItem1 = new BarItem { Text = "File" };
BarItem barItem2 = new BarItem { Text = "Edit" };
BarItem barItem3 = new BarItem { Text = "View" };

// Setup ParentBarItem
parentBarItem1.Items.AddRange(new BarItem[] { barItem1, barItem2, barItem3 });
parentBarItem1.SizeToFit = true;

// Associate PopupMenu with CommandBar
popupMenu1.ParentBarItem = parentBarItem1;
commandBar1.PopupMenu = popupMenu1;  // Key association

// Configure CommandBar
commandBar1.DockState = Syncfusion.Windows.Forms.Tools.CommandBarDockState.Top;
commandBar1.Text = "Main Menu";

// Add CommandBar to controller
commandBarController1.CommandBars.Add(commandBar1);
```

### VB.NET Example

```vb
' Initialize components
Dim commandBarController1 As New CommandBarController(Me.components)
Dim commandBar1 As New CommandBar()
Dim popupMenu1 As New PopupMenu(Me.components)
Dim parentBarItem1 As New ParentBarItem()
Dim barItem1 As New BarItem With {.Text = "File"}
Dim barItem2 As New BarItem With {.Text = "Edit"}
Dim barItem3 As New BarItem With {.Text = "View"}

' Setup ParentBarItem
parentBarItem1.Items.AddRange(New BarItem() {barItem1, barItem2, barItem3})
parentBarItem1.SizeToFit = True

' Associate PopupMenu with CommandBar
popupMenu1.ParentBarItem = parentBarItem1
commandBar1.PopupMenu = popupMenu1  ' Key association

' Configure CommandBar
commandBar1.DockState = Syncfusion.Windows.Forms.Tools.CommandBarDockState.Top
commandBar1.Text = "Main Menu"

' Add CommandBar to controller
commandBarController1.CommandBars.Add(commandBar1)
```

### Complete Bar Manager Example

```csharp
// Create bar manager infrastructure
CommandBarController barManager = new CommandBarController(this.components);

// File menu CommandBar
CommandBar fileCommandBar = new CommandBar();
fileCommandBar.Text = "File";
fileCommandBar.DockState = CommandBarDockState.Top;

PopupMenu filePopup = new PopupMenu(this.components);
ParentBarItem fileParent = new ParentBarItem();
fileParent.Items.AddRange(new BarItem[] {
    new BarItem { Text = "New", Shortcut = Shortcut.CtrlN },
    new BarItem { Text = "Open", Shortcut = Shortcut.CtrlO },
    new BarItem { Text = "Save", Shortcut = Shortcut.CtrlS },
    new BarItem { Text = "Exit" }
});

filePopup.ParentBarItem = fileParent;
fileCommandBar.PopupMenu = filePopup;

// Edit menu CommandBar
CommandBar editCommandBar = new CommandBar();
editCommandBar.Text = "Edit";
editCommandBar.DockState = CommandBarDockState.Top;

PopupMenu editPopup = new PopupMenu(this.components);
ParentBarItem editParent = new ParentBarItem();
editParent.Items.AddRange(new BarItem[] {
    new BarItem { Text = "Cut", Shortcut = Shortcut.CtrlX },
    new BarItem { Text = "Copy", Shortcut = Shortcut.CtrlC },
    new BarItem { Text = "Paste", Shortcut = Shortcut.CtrlV }
});

editPopup.ParentBarItem = editParent;
editCommandBar.PopupMenu = editPopup;

// Add to bar manager
barManager.CommandBars.Add(fileCommandBar);
barManager.CommandBars.Add(editCommandBar);
```

### Benefits of Bar Manager Integration

- **Consistent UI:** Unified menu appearance across application
- **Centralized Management:** Single place to configure menus
- **Toolbar Integration:** Menus embedded in CommandBar toolbars
- **Docking Support:** Moveable/dockable menu bars
- **Professional Layout:** Standard Windows application menu structure

## Events

PopupMenu provides events for customizing menu behavior at different lifecycle stages.

### BeforePopup Event

Fired **before** the menu is displayed. Ideal for updating menu states, enabling/disabling items, or modifying content.

```csharp
popupMenu1.BeforePopup += PopupMenu1_BeforePopup;

private void PopupMenu1_BeforePopup(object sender, CancelEventArgs e)
{
    // Update menu states before showing
    
    // Enable/disable based on selection
    bool hasSelection = richTextBox1.SelectionLength > 0;
    cutItem.Enabled = hasSelection;
    copyItem.Enabled = hasSelection;
    
    // Enable paste if clipboard has text
    pasteItem.Enabled = Clipboard.ContainsText();
    
    // Update undo/redo states
    undoItem.Enabled = richTextBox1.CanUndo;
    
    // Update checked states
    wordWrapItem.Checked = richTextBox1.WordWrap;
    
    // Get mouse position for context-sensitive menus
    Point mousePos = e as System.Windows.Forms.MouseEventArgs != null
        ? ((System.Windows.Forms.MouseEventArgs)e).Location
        : Cursor.Position;
    
    // Cancel popup if conditions not met
    if (shouldCancelPopup)
    {
        e.Cancel = true;
    }
}
```

### Popup Event

Fired **after** the menu is displayed.

```csharp
popupMenu1.Popup += PopupMenu1_Popup;

private void PopupMenu1_Popup(object sender, EventArgs e)
{
    // Menu is now visible
    // Use for logging, analytics, or post-display actions
    
    Console.WriteLine("Popup menu displayed at " + DateTime.Now);
    LogMenuDisplayEvent();
}
```

### Collapse Event

Fired when the menu is closed.

```csharp
popupMenu1.Collapse += PopupMenu1_Collapse;

private void PopupMenu1_Collapse(object sender, EventArgs e)
{
    // Menu has been closed
    // Clean up temporary states or track menu session
    
    Console.WriteLine("Popup menu closed");
    SaveMenuPreferences();
}
```

### ParentBarItemChanged Event

Fired when ParentBarItem properties (Text, Image, Font, etc.) are changed.

```csharp
popupMenu1.ParentBarItemChanged += PopupMenu1_ParentBarItemChanged;

private void PopupMenu1_ParentBarItemChanged(object sender, EventArgs e)
{
    // ParentBarItem properties have changed
    Console.WriteLine("ParentBarItem was modified");
    
    // Respond to changes if needed
    RefreshMenuDisplay();
}
```

### Event Usage Patterns

**BeforePopup - Most Common Use:**
```csharp
popupMenu1.BeforePopup += (s, e) => {
    // Update all menu states
    UpdateMenuStates();
    
    // Add dynamic items
    if (hasRecentFiles)
    {
        AddRecentFilesMenu();
    }
    
    // Context-sensitive menu
    if (isReadOnlyMode)
    {
        DisableEditCommands();
    }
};
```

**Event Chaining:**
```csharp
popupMenu1.BeforePopup += BeforeShow_UpdateStates;
popupMenu1.BeforePopup += BeforeShow_CheckPermissions;
popupMenu1.BeforePopup += BeforeShow_AddDynamicItems;

popupMenu1.Popup += AfterShow_StartTimer;
popupMenu1.Collapse += AfterClose_StopTimer;
```

## Dynamic Menu Customization

### Adding Items at Runtime

```csharp
private void AddRecentFilesSubmenu()
{
    // Find or create Recent Files parent
    ParentBarItem recentMenu = new ParentBarItem();
    recentMenu.Text = "Recent Files";
    recentMenu.SizeToFit = true;
    
    // Add recent file items
    List<string> recentFiles = GetRecentFiles();
    foreach (string filePath in recentFiles)
    {
        BarItem fileItem = new BarItem();
        fileItem.Text = Path.GetFileName(filePath);
        fileItem.Tooltip = filePath;
        fileItem.ShowToolTipInPopUp = true;
        fileItem.Tag = filePath;  // Store full path
        fileItem.Click += (s, e) => {
            BarItem item = s as BarItem;
            if (item != null && item.Tag is string path)
            {
                OpenFile(path);
            }
        };
        
        recentMenu.Items.Add(fileItem);
    }
    
    // Add to main menu
    parentBarItem1.Items.Add(recentMenu);
    parentBarItem1.BeginGroupAt(recentMenu);  // Add separator
}
```

### Removing Items at Runtime

```csharp
private void RemoveMenuItem(string itemText)
{
    BarItem itemToRemove = parentBarItem1.Items
        .Cast<BarItem>()
        .FirstOrDefault(item => item.Text == itemText);
    
    if (itemToRemove != null)
    {
        parentBarItem1.Items.Remove(itemToRemove);
    }
}

// Usage
RemoveMenuItem("Advanced Options");
```

### Conditional Menu Structure

```csharp
popupMenu1.BeforePopup += (s, e) => {
    // Clear existing dynamic items
    ClearDynamicItems();
    
    // Add items based on context
    if (currentUser.IsAdmin)
    {
        AddAdminMenuItems();
    }
    
    if (currentDocument != null)
    {
        AddDocumentMenuItems();
    }
    
    if (debugMode)
    {
        AddDebugMenuItems();
    }
};

private void ClearDynamicItems()
{
    // Remove items marked as dynamic
    var itemsToRemove = parentBarItem1.Items
        .Cast<BarItem>()
        .Where(item => item.Tag is string tag && tag == "Dynamic")
        .ToList();
    
    foreach (var item in itemsToRemove)
    {
        parentBarItem1.Items.Remove(item);
    }
}

private void AddAdminMenuItems()
{
    BarItem adminItem = new BarItem {
        Text = "Admin Tools",
        Tag = "Dynamic"  // Mark as dynamic
    };
    adminItem.Click += OpenAdminTools;
    
    parentBarItem1.Items.Add(adminItem);
}
```

### Context-Sensitive Menus

```csharp
popupMenu1.BeforePopup += (s, e) => {
    // Get context from clicked control/position
    Control sourceControl = popupMenusManager1.GetSourceControl();
    
    if (sourceControl is RichTextBox rtb)
    {
        ConfigureTextEditorMenu(rtb);
    }
    else if (sourceControl is DataGridView dgv)
    {
        ConfigureGridMenu(dgv);
    }
    else if (sourceControl is TreeView tv)
    {
        ConfigureTreeMenu(tv);
    }
};

private void ConfigureTextEditorMenu(RichTextBox rtb)
{
    // Enable/disable based on text editor state
    bool hasText = rtb.Text.Length > 0;
    bool hasSelection = rtb.SelectionLength > 0;
    
    cutItem.Enabled = hasSelection;
    copyItem.Enabled = hasSelection;
    selectAllItem.Enabled = hasText;
    findItem.Enabled = hasText;
}
```

## Best Practices

### Partial Menus
- Use for menus with 10+ items
- Mark 4-6 items as frequently used
- Track actual usage patterns if possible
- Test with real users for optimal prioritization

### Bar Manager Integration
- Use for application-wide menu consistency
- Centralize menu creation and management
- Leverage docking and customization features
- Test menu layout at different form sizes

### Events
- Use BeforePopup for state updates (most common)
- Keep event handlers lightweight
- Avoid long-running operations in events
- Handle exceptions in event handlers

### Dynamic Menus
- Update in BeforePopup, not Popup
- Cache menu structures when possible
- Mark dynamic items (use Tag property)
- Clean up dynamic items properly

## Troubleshooting

**Issue: Partial menus not working**
- Verify `UsePartialMenus = true` on ParentBarItem
- Check that some items have `IsRecentlyUsedItem = false`
- Ensure sufficient items (< 8 items won't show partial behavior)

**Issue: Bar Manager menu doesn't appear**
- Verify `commandBar.PopupMenu` is assigned
- Check that CommandBar is added to CommandBarController
- Ensure ParentBarItem has items

**Issue: BeforePopup doesn't fire**
- Verify event is wired up
- Check that menu is actually being displayed
- Ensure PopupMenusManager.SetXPContextMenu() was called

**Issue: Events fire multiple times**
- Check for duplicate event registrations
- Verify event -= before event +=
- Look for cascading event triggers

**Issue: Dynamic items persist**
- Clear dynamic items in BeforePopup
- Use Tag property to identify dynamic items
- Remove items explicitly before adding new ones
