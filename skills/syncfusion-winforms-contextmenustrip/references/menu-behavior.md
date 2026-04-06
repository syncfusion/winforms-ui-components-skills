# Menu Behavior and Event Handling

## Table of Contents
- [Overview](#overview)
- [Auto-Close Behavior](#auto-close-behavior)
- [Click Events](#click-events)
- [Menu Lifecycle Events](#menu-lifecycle-events)
- [Event Handling Patterns](#event-handling-patterns)
- [Best Practices](#best-practices)

## Overview

Context menu behavior determines how menus respond to user interactions, when they open and close, and how events are triggered. Proper configuration ensures intuitive user experience and correct application responses.

**Key Behavioral Aspects:**
- **AutoClose:** Whether menus close automatically after user actions
- **Click Events:** How menu item selections are handled
- **Opening/Closing:** When menus appear and disappear
- **Event Lifecycle:** The sequence of events during menu interaction
- **Keyboard Interaction:** Shortcut keys and navigation

## Auto-Close Behavior

The `AutoClose` property controls whether the context menu automatically closes when users click menu items or interact with the form.

### AutoClose Property

**Property:** `AutoClose` (bool)  
**Default:** `true`  
**Applies to:** ContextMenuStripEx control

| Value | Behavior |
|-------|----------|
| `true` (default) | Menu closes automatically when user clicks a menu item or clicks outside the menu |
| `false` | Menu remains open after user actions; must be closed programmatically |

### Setting AutoClose

**Via Designer:**
1. Select ContextMenuStripEx in component tray
2. In Properties panel → Behavior section
3. Set **AutoClose** to True or False

**Via Code:**
```csharp
// Enable auto-close (default)
contextMenuStripEx.AutoClose = true;

// Disable auto-close
contextMenuStripEx.AutoClose = false;
```

**VB.NET:**
```vb
' Enable auto-close (default)
contextMenuStripEx.AutoClose = True

' Disable auto-close
contextMenuStripEx.AutoClose = False
```

### When to Use AutoClose = false

**Use Cases:**
1. **Multi-selection menus:** Allow users to check multiple items before closing
2. **Interactive menus:** Menus with TextBox/ComboBox that need input before closing
3. **Preview menus:** Show live previews as user hovers over options
4. **Complex workflows:** Multi-step operations within the menu

**Important:** When AutoClose = false, you must close the menu programmatically:

**C# Example:**
```csharp
private void CreateNonAutoCloseMenu()
{
    var contextMenu = new ContextMenuStripEx();
    contextMenu.AutoClose = false;  // Menu stays open
    
    // Add interactive items
    var option1 = new ToolStripMenuItem("Option 1");
    option1.CheckOnClick = true;
    
    var option2 = new ToolStripMenuItem("Option 2");
    option2.CheckOnClick = true;
    
    var option3 = new ToolStripMenuItem("Option 3");
    option3.CheckOnClick = true;
    
    var separator = new ToolStripSeparator();
    
    // Add close button
    var applyButton = new ToolStripMenuItem("Apply");
    applyButton.Click += (s, e) => {
        // Process selections
        ProcessSelections(option1.Checked, option2.Checked, option3.Checked);
        
        // Manually close the menu
        contextMenu.Close();
    };
    
    var cancelButton = new ToolStripMenuItem("Cancel");
    cancelButton.Click += (s, e) => {
        // Just close without applying
        contextMenu.Close();
    };
    
    contextMenu.Items.AddRange(new ToolStripItem[] {
        option1, option2, option3, separator, applyButton, cancelButton
    });
    
    this.textBox1.ContextMenuStrip = contextMenu;
}
```

### Combining AutoClose with CheckOnClick

When using checkable items, consider the user experience:

**AutoClose = true + CheckOnClick:**
- User clicks item → state toggles → menu closes
- Good for single-option toggles (like "Word Wrap")
- Not ideal for multi-selection

**AutoClose = false + CheckOnClick:**
- User clicks items → states toggle → menu stays open
- Good for multi-selection scenarios
- Requires manual close button or mechanism

## Click Events

The `Click` event is the primary mechanism for responding to menu item selections. It fires when users click menu items or press keyboard shortcuts.

### Click Event Basics

**Event:** `Click`  
**Handler Signature:** `void EventHandler(object sender, EventArgs e)`  
**Fires When:**
- User clicks the menu item with mouse
- User presses the configured keyboard shortcut
- Item is activated via keyboard navigation (Enter key)

### Subscribing to Click Events

**Via Designer:** Double-click Click event in Properties panel (Events button)

**Via Code:**
```csharp
// Named handler
this.toolStripMenuItem1.Click += ToolStripMenuItem1_Click;
private void ToolStripMenuItem1_Click(object sender, EventArgs e)
{
    MessageBox.Show("Menu Item 1 clicked!");
}

// Lambda/Anonymous
var menuItem1 = new ToolStripMenuItem("Option 1");
menuItem1.Click += (s, e) => MessageBox.Show("Option 1 clicked!");

// Constructor inline
var openItem = new ToolStripMenuItem("Open", null, (s, e) => OpenFile());
```

### Accessing the Clicked Item

```csharp
private void MenuItem_Click(object sender, EventArgs e)
{
    var clickedItem = sender as ToolStripMenuItem;
    if (clickedItem != null)
    {
        string itemText = clickedItem.Text;
        bool isChecked = clickedItem.Checked;
        object tag = clickedItem.Tag;  // Custom data
        MessageBox.Show($"Clicked: {itemText}");
    }
}
```

**Note:** Click event fires for both mouse clicks and keyboard shortcuts. For ComboBox, use SelectedIndexChanged instead.

## Menu Lifecycle Events

Context menus fire several events during their lifecycle, allowing you to respond at different stages.

### Key Lifecycle Events

| Event | Description | Common Uses |
|-------|-------------|-------------|
| `Opening` | Fires before menu is displayed | Update item states, populate dynamic items |
| `Opened` | Fires after menu is fully displayed | Start timers, log analytics |
| `Closing` | Fires when menu is about to close | Cleanup, validation |
| `Closed` | Fires after menu is fully closed | Release resources, save state |

### Opening Event

**Use this event to:**
- Update enabled/disabled states based on current context
- Show/hide items dynamically
- Populate menu items from current data
- Validate whether menu should appear

**C# Example:**
```csharp
private void InitializeMenu()
{
    var contextMenu = new ContextMenuStripEx();
    
    var cutItem = new ToolStripMenuItem("Cut");
    var copyItem = new ToolStripMenuItem("Copy");
    var pasteItem = new ToolStripMenuItem("Paste");
    
    contextMenu.Items.AddRange(new ToolStripItem[] {
        cutItem, copyItem, pasteItem
    });
    
    // Update states before menu opens
    contextMenu.Opening += (s, e) => {
        bool hasSelection = textBox.SelectionLength > 0;
        bool hasClipboardText = Clipboard.ContainsText();
        
        cutItem.Enabled = hasSelection;
        copyItem.Enabled = hasSelection;
        pasteItem.Enabled = hasClipboardText;
    };
    
    this.textBox.ContextMenuStrip = contextMenu;
}
```

**Cancel Menu Opening:**
```csharp
contextMenu.Opening += (s, e) => {
    if (ShouldPreventMenu())
    {
        e.Cancel = true;  // Prevents menu from showing
    }
};
```

### Other Lifecycle Events

```csharp
// Opened - fires after menu is fully visible
contextMenu.Opened += (s, e) => LogMenuOpened();

// Closing - fires when menu is about to close (can cancel with e.Cancel = true)
contextMenu.Closing += (s, e) => {
    if (HasUnsavedChanges())
        e.Cancel = true;  // Keeps menu open
};

// Closed - fires after menu is fully closed
contextMenu.Closed += (s, e) => CleanupResources();
```

## Event Handling Patterns

### Pattern 1: Context-Aware Menu

Enable/disable items based on current application state:

```csharp
private ContextMenuStripEx CreateContextAwareMenu()
{
    var contextMenu = new ContextMenuStripEx();
    
    var cutItem = new ToolStripMenuItem("Cut");
    cutItem.Click += (s, e) => Cut();
    
    var copyItem = new ToolStripMenuItem("Copy");
    copyItem.Click += (s, e) => Copy();
    
    var pasteItem = new ToolStripMenuItem("Paste");
    pasteItem.Click += (s, e) => Paste();
    
    var separator = new ToolStripSeparator();
    
    var selectAllItem = new ToolStripMenuItem("Select All");
    selectAllItem.Click += (s, e) => SelectAll();
    
    contextMenu.Items.AddRange(new ToolStripItem[] {
        cutItem, copyItem, pasteItem, separator, selectAllItem
    });
    
    // Update item availability when opening
    contextMenu.Opening += (s, e) => {
        bool hasSelection = textBox.SelectionLength > 0;
        bool hasText = textBox.Text.Length > 0;
        bool hasClipboard = Clipboard.ContainsText();
        
        cutItem.Enabled = hasSelection;
        copyItem.Enabled = hasSelection;
        pasteItem.Enabled = hasClipboard;
        selectAllItem.Enabled = hasText && textBox.SelectionLength < textBox.Text.Length;
    };
    
    return contextMenu;
}
```

### Pattern 2: Dynamic Menu Population

Build menu contents on-demand:

```csharp
private ContextMenuStripEx CreateDynamicMenu()
{
    var contextMenu = new ContextMenuStripEx();
    
    var recentMenu = new ToolStripMenuItem("Recent Files");
    
    // Populate when submenu opens
    recentMenu.DropDownOpening += (s, e) => {
        var menu = s as ToolStripMenuItem;
        menu.DropDownItems.Clear();
        
        var recentFiles = GetRecentFiles();
        if (recentFiles.Count == 0)
        {
            var noFiles = new ToolStripMenuItem("(No recent files)");
            noFiles.Enabled = false;
            menu.DropDownItems.Add(noFiles);
        }
        else
        {
            foreach (var file in recentFiles)
            {
                var fileItem = new ToolStripMenuItem(Path.GetFileName(file));
                fileItem.Tag = file;
                fileItem.Click += (sender, args) => {
                    var item = sender as ToolStripMenuItem;
                    OpenFile(item.Tag as string);
                };
                menu.DropDownItems.Add(fileItem);
            }
        }
    };
    
    contextMenu.Items.Add(recentMenu);
    return contextMenu;
}
```

### Pattern 3: Shared Event Handler

Use one handler for multiple items:

```csharp
private void CreateSharedHandlerMenu()
{
    var contextMenu = new ContextMenuStripEx();
    
    var smallFont = new ToolStripMenuItem("Small");
    smallFont.Tag = 8;  // Font size
    smallFont.Click += FontSize_Click;
    
    var mediumFont = new ToolStripMenuItem("Medium");
    mediumFont.Tag = 12;
    mediumFont.Click += FontSize_Click;
    
    var largeFont = new ToolStripMenuItem("Large");
    largeFont.Tag = 16;
    largeFont.Click += FontSize_Click;
    
    contextMenu.Items.AddRange(new ToolStripItem[] {
        smallFont, mediumFont, largeFont
    });
}

private void FontSize_Click(object sender, EventArgs e)
{
    var item = sender as ToolStripMenuItem;
    int fontSize = (int)item.Tag;
    
    SetFontSize(fontSize);
}
```

### Pattern 4: Async Operations

Handle long-running operations triggered from menu:

```csharp
private void CreateAsyncMenu()
{
    var contextMenu = new ContextMenuStripEx();
    
    var loadDataItem = new ToolStripMenuItem("Load Data");
    loadDataItem.Click += async (s, e) => {
        var item = s as ToolStripMenuItem;
        item.Enabled = false;
        item.Text = "Loading...";
        
        try
        {
            await LoadDataAsync();
            MessageBox.Show("Data loaded successfully!");
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Error: {ex.Message}");
        }
        finally
        {
            item.Text = "Load Data";
            item.Enabled = true;
        }
    };
    
    contextMenu.Items.Add(loadDataItem);
}
```

## Best Practices

1. **Keep handlers focused:** Each handler should do one thing well
2. **Handle exceptions:** Wrap handlers in try-catch to prevent crashes
3. **Use async:** For long-running operations
4. **AutoClose:** Default to true; use false only for multi-selection menus
5. **Subscribe once:** Don't subscribe in loops; unsubscribe when disposing

## Troubleshooting

**Click event not firing:** Verify handler subscribed and item Enabled = true  
**Menu not closing:** Check AutoClose property and Click handlers  
**Opening event not firing:** Verify subscription to Opening (not Opened)  
**Events firing multiple times:** Check for duplicate subscriptions
