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

**Via Designer:**
1. Select menu item in designer
2. In Properties panel, click Events button (lightning bolt)
3. Double-click the **Click** event or type handler name
4. Visual Studio generates the event handler method

**Via Code (Method 1 - Named Handler):**
```csharp
// Subscribe to event
this.toolStripMenuItem1.Click += ToolStripMenuItem1_Click;
this.toolStripMenuItem2.Click += ToolStripMenuItem2_Click;

// Event handler methods
private void ToolStripMenuItem1_Click(object sender, EventArgs e)
{
    MessageBox.Show("Menu Item 1 clicked!");
}

private void ToolStripMenuItem2_Click(object sender, EventArgs e)
{
    MessageBox.Show("Menu Item 2 clicked!");
}
```

**VB.NET (Named Handler):**
```vb
' Subscribe to event
AddHandler Me.toolStripMenuItem1.Click, AddressOf ToolStripMenuItem1_Click
AddHandler Me.toolStripMenuItem2.Click, AddressOf ToolStripMenuItem2_Click

' Event handler methods
Private Sub ToolStripMenuItem1_Click(sender As Object, e As EventArgs)
    MessageBox.Show("Menu Item 1 clicked!")
End Sub

Private Sub ToolStripMenuItem2_Click(sender As Object, e As EventArgs)
    MessageBox.Show("Menu Item 2 clicked!")
End Sub
```

**Via Code (Method 2 - Anonymous/Lambda):**
```csharp
var menuItem1 = new ToolStripMenuItem("Option 1");
menuItem1.Click += (s, e) => {
    MessageBox.Show("Option 1 clicked!");
};

var menuItem2 = new ToolStripMenuItem("Option 2");
menuItem2.Click += (s, e) => {
    // Inline handler logic
    ProcessOption2();
};
```

**Via Code (Method 3 - Constructor Inline):**
```csharp
// Pass handler directly in constructor
var openItem = new ToolStripMenuItem("Open", null, OpenItem_Click);
var saveItem = new ToolStripMenuItem("Save", null, (s, e) => SaveFile());

private void OpenItem_Click(object sender, EventArgs e)
{
    OpenFile();
}
```

### Click Events for Different Item Types

**MenuItem:**
```csharp
toolStripMenuItem.Click += (s, e) => {
    // Handle menu item click
    PerformAction();
};
```

**TextBox:**
```csharp
toolStripTextBox.Click += (s, e) => {
    // Handle textbox click (focus)
    // Note: Usually use TextChanged or KeyPress instead
};
```

**ComboBox:**
```csharp
toolStripComboBox.Click += (s, e) => {
    // Handle combobox click
    // Note: Usually use SelectedIndexChanged instead
};

// Better: Use selection change event
toolStripComboBox.SelectedIndexChanged += (s, e) => {
    string selected = toolStripComboBox.SelectedItem.ToString();
    HandleSelection(selected);
};
```

### Accessing the Clicked Item

**Get the menu item that was clicked:**
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

### Click Events and Keyboard Shortcuts

When keyboard shortcuts are configured, the Click event fires when the shortcut is pressed:

```csharp
var cutItem = new ToolStripMenuItem("Cut");
cutItem.ShortcutKeys = Keys.Control | Keys.X;
cutItem.Click += (s, e) => {
    // Fires on menu click OR when Ctrl+X is pressed
    PerformCut();
};
```

**Note:** The Click event doesn't distinguish between mouse clicks and keyboard shortcuts. Both trigger the same event.

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

### Opened Event

Fires after the menu is fully visible:

```csharp
contextMenu.Opened += (s, e) => {
    // Menu is now visible
    LogMenuOpened();
    StartIdleTimer();
};
```

### Closing Event

Fires when the menu is about to close:

```csharp
contextMenu.Closing += (s, e) => {
    // Menu is about to close
    
    // Can cancel closing if needed
    if (HasUnsavedChanges())
    {
        var result = MessageBox.Show("Discard changes?", "Confirm", 
                                      MessageBoxButtons.YesNo);
        if (result == DialogResult.No)
        {
            e.Cancel = true;  // Keeps menu open
        }
    }
};
```

### Closed Event

Fires after the menu is fully closed:

```csharp
contextMenu.Closed += (s, e) => {
    // Menu is now closed
    CleanupResources();
    SaveLastInteraction();
};
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

### Event Handler Best Practices

1. **Keep handlers focused:** Each handler should do one thing well
2. **Handle exceptions:** Wrap handlers in try-catch to prevent crashes
3. **Avoid blocking operations:** Use async for long-running tasks
4. **Clean up resources:** Unsubscribe when disposing controls
5. **Validate state:** Check application state before performing actions

### AutoClose Guidelines

1. **Default to true:** Most menus should auto-close
2. **Use false sparingly:** Only for multi-selection or interactive menus
3. **Provide close mechanism:** When AutoClose = false, add close button
4. **Consider UX:** Non-closing menus can confuse users

### Event Subscription

**Good Practice:**
```csharp
// Subscribe once during initialization
menuItem.Click += HandleMenuClick;
```

**Bad Practice:**
```csharp
// Don't subscribe in loops or repeatedly
for (int i = 0; i < 10; i++)
{
    menuItem.Click += HandleMenuClick;  // Creates 10 subscriptions!
}
```

### Unsubscribing

```csharp
// When disposing or cleaning up
menuItem.Click -= HandleMenuClick;

// Or in Dispose method
protected override void Dispose(bool disposing)
{
    if (disposing)
    {
        menuItem.Click -= HandleMenuClick;
    }
    base.Dispose(disposing);
}
```

## Troubleshooting

**Click event not firing:**
- Verify event handler is subscribed
- Check that item is Enabled = true
- Ensure no exceptions are thrown in handler
- Confirm menu is properly associated with control

**Menu not closing:**
- Check AutoClose property
- Verify Click handlers aren't preventing closure
- Ensure no code is re-showing the menu

**Opening event not firing:**
- Verify subscription to correct event (Opening, not Opened)
- Check that menu actually displays
- Ensure control's ContextMenuStrip property is set

**Events firing multiple times:**
- Check for duplicate subscriptions
- Verify += is not called repeatedly
- Use -= to unsubscribe before resubscribing
