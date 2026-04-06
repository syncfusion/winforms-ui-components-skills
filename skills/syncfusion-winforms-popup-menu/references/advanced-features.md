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
BarItem saveAsItem = new BarItem { Text = "Save As..." };
BarItem exitItem = new BarItem { Text = "Exit" };

parentBarItem1.Items.AddRange(new BarItem[] { newItem, saveAsItem, exitItem });

// Mark frequently used items (visible by default)
newItem.IsRecentlyUsedItem = true;
exitItem.IsRecentlyUsedItem = true;

// Mark less frequently used items (hidden initially)
saveAsItem.IsRecentlyUsedItem = false;

popupMenu1.ParentBarItem = parentBarItem1;
```

**Note:** By default, `IsRecentlyUsedItem = true` for all items. Set to `false` to hide initially.

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

**Use Cases:** Long menus (10+ items) with mix of common/specialized commands. Skip for short menus (< 8 items) or when all items equally important.

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

// Setup ParentBarItem with items
parentBarItem1.Items.AddRange(new BarItem[] { 
    new BarItem { Text = "File" },
    new BarItem { Text = "Edit" }
});
parentBarItem1.SizeToFit = true;

// Associate PopupMenu with CommandBar
popupMenu1.ParentBarItem = parentBarItem1;
commandBar1.PopupMenu = popupMenu1;  // Key association
commandBar1.DockState = CommandBarDockState.Top;

// Add to controller
commandBarController1.CommandBars.Add(commandBar1);
```

**Benefits:** Consistent UI, centralized management, toolbar integration, docking support, professional layout.

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

### Other Events

```csharp
// Popup - Fired after menu is displayed
popupMenu1.Popup += (s, e) => {
    Console.WriteLine("Menu displayed");
    LogMenuDisplayEvent();
};

// Collapse - Fired when menu is closed
popupMenu1.Collapse += (s, e) => {
    Console.WriteLine("Menu closed");
    SaveMenuPreferences();
};

// ParentBarItemChanged - Fired when ParentBarItem properties change
popupMenu1.ParentBarItemChanged += (s, e) => {
    RefreshMenuDisplay();
};
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

- **Partial Menus:** Use for 10+ items, mark 4-6 as frequently used
- **Bar Manager:** Use for application-wide menu consistency
- **Events:** Use BeforePopup for state updates, keep handlers lightweight
- **Dynamic Menus:** Update in BeforePopup, cache structures, mark with Tag property

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Partial menus not working | Verify `UsePartialMenus = true` and some items have `IsRecentlyUsedItem = false` |
| Bar Manager menu doesn't appear | Check `commandBar.PopupMenu` assigned and CommandBar added to controller |
| BeforePopup doesn't fire | Verify event wired up and PopupMenusManager.SetXPContextMenu() called |
| Events fire multiple times | Check for duplicate registrations, use event -= before += |
| Dynamic items persist | Clear in BeforePopup, use Tag property to identify and remove |
