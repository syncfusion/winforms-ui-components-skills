# Menu Item States

Guide for managing checked/unchecked states and enabling/disabling menu items in ContextMenuStripEx.

## Overview

Menu items can exist in different states that affect their appearance and behavior:

- **Checked/Unchecked:** Visual indicators showing selection status
- **Enabled/Disabled:** Controls whether items are clickable or grayed out
- **Indeterminate:** A third state for partial selection scenarios

These states help users understand current selections, available actions, and application state.

## Checked and Unchecked States

The checked state displays a checkmark next to menu items, useful for toggleable options like "Show Toolbar" or "Word Wrap".

### Key Properties

- **`Checked`** (bool): Whether a check mark appears before the text
- **`CheckState`** (enum): Precise state - Checked, Unchecked, or Indeterminate
- **`CheckOnClick`** (bool): Automatically toggles Checked state when clicked
- **`ShowCheckMargin`** (ContextMenuStripEx property): Must be true for checkmarks to display

### Important Notes

1. Checked states only apply to **MenuItem** (not TextBox or ComboBox)
2. Check marks only display if **`ShowCheckMargin = true`** on the ContextMenuStripEx
3. By default, you must manually toggle states in Click event handlers
4. Setting `CheckOnClick = true` automates the toggle behavior

## Setting Checked State

### Via Designer

1. Select the menu item in the designer
2. In Properties panel, locate **Behavior** section
3. Set **Checked** to True or False
4. Set **CheckState** to Checked, Unchecked, or Indeterminate
5. For the parent ContextMenuStripEx:
   - Set **ShowCheckMargin** to True

### Via Code - Initial State

**C# Example:**
```csharp
using Syncfusion.Windows.Forms.Tools;
using System.Windows.Forms;

// Create context menu with check margin enabled
ContextMenuStripEx contextMenu = new ContextMenuStripEx();
contextMenu.ShowCheckMargin = true;  // REQUIRED for checkmarks to display

// Create menu items with initial checked states
ToolStripMenuItem wordWrapItem = new ToolStripMenuItem();
wordWrapItem.Text = "Word Wrap";
wordWrapItem.Checked = true;  // Initially checked
wordWrapItem.CheckState = CheckState.Checked;

ToolStripMenuItem showToolbarItem = new ToolStripMenuItem();
showToolbarItem.Text = "Show Toolbar";
showToolbarItem.Checked = false;  // Initially unchecked
showToolbarItem.CheckState = CheckState.Unchecked;

contextMenu.Items.AddRange(new ToolStripItem[] {
    wordWrapItem,
    showToolbarItem
});
```

**VB.NET Example:**
```vb
Imports Syncfusion.Windows.Forms.Tools
Imports System.Windows.Forms

' Create context menu with check margin enabled
Dim contextMenu As New ContextMenuStripEx()
contextMenu.ShowCheckMargin = True  ' REQUIRED for checkmarks to display

' Create menu items with initial checked states
Dim wordWrapItem As New ToolStripMenuItem()
wordWrapItem.Text = "Word Wrap"
wordWrapItem.Checked = True  ' Initially checked
wordWrapItem.CheckState = CheckState.Checked

Dim showToolbarItem As New ToolStripMenuItem()
showToolbarItem.Text = "Show Toolbar"
showToolbarItem.Checked = False  ' Initially unchecked
showToolbarItem.CheckState = CheckState.Unchecked

contextMenu.Items.AddRange(New ToolStripItem() {
    wordWrapItem,
    showToolbarItem
})
```

## Toggling States Manually

By default, clicking a menu item does NOT automatically toggle its checked state. You must handle this in the Click event.

### Manual Toggle Pattern

**C# Example:**
```csharp
private void InitializeMenu()
{
    var contextMenu = new ContextMenuStripEx();
    contextMenu.ShowCheckMargin = true;
    
    var toggleItem = new ToolStripMenuItem("Toggle Option");
    toggleItem.Checked = false;
    toggleItem.Click += ToggleItem_Click;
    
    contextMenu.Items.Add(toggleItem);
}

private void ToggleItem_Click(object sender, EventArgs e)
{
    var menuItem = sender as ToolStripMenuItem;
    
    // Manually toggle the state
    menuItem.Checked = !menuItem.Checked;
    
    // Update CheckState to match
    menuItem.CheckState = menuItem.Checked ? CheckState.Checked : CheckState.Unchecked;
    
    // Perform action based on new state
    if (menuItem.Checked)
    {
        EnableFeature();
    }
    else
    {
        DisableFeature();
    }
}
```

**VB.NET Example:**
```vb
Private Sub InitializeMenu()
    Dim contextMenu As New ContextMenuStripEx()
    contextMenu.ShowCheckMargin = True
    
    Dim toggleItem As New ToolStripMenuItem("Toggle Option")
    toggleItem.Checked = False
    AddHandler toggleItem.Click, AddressOf ToggleItem_Click
    
    contextMenu.Items.Add(toggleItem)
End Sub

Private Sub ToggleItem_Click(sender As Object, e As EventArgs)
    Dim menuItem As ToolStripMenuItem = TryCast(sender, ToolStripMenuItem)
    
    ' Manually toggle the state
    menuItem.Checked = Not menuItem.Checked
    
    ' Update CheckState to match
    menuItem.CheckState = If(menuItem.Checked, CheckState.Checked, CheckState.Unchecked)
    
    ' Perform action based on new state
    If menuItem.Checked Then
        EnableFeature()
    Else
        DisableFeature()
    End If
End Sub
```

## Automatic Toggle with CheckOnClick

The `CheckOnClick` property automatically toggles the Checked state when the item is clicked, eliminating the need for manual toggle code.

**C# Example:**
```csharp
var contextMenu = new ContextMenuStripEx();
contextMenu.ShowCheckMargin = true;

var autoToggleItem = new ToolStripMenuItem("Auto Toggle");
autoToggleItem.CheckOnClick = true;  // Automatic toggle
autoToggleItem.Checked = false;

// Click event now only needs to handle the action
autoToggleItem.Click += (s, e) => {
    var item = s as ToolStripMenuItem;
    // item.Checked is already toggled automatically
    if (item.Checked)
        EnableFeature();
    else
        DisableFeature();
};

contextMenu.Items.Add(autoToggleItem);
```

**VB.NET Example:**
```vb
Dim contextMenu As New ContextMenuStripEx()
contextMenu.ShowCheckMargin = True

Dim autoToggleItem As New ToolStripMenuItem("Auto Toggle")
autoToggleItem.CheckOnClick = True  ' Automatic toggle
autoToggleItem.Checked = False

' Click event now only needs to handle the action
AddHandler autoToggleItem.Click, Sub(s, e)
    Dim item As ToolStripMenuItem = TryCast(s, ToolStripMenuItem)
    If item.Checked Then
        EnableFeature()
    Else
        DisableFeature()
    End If
End Sub

contextMenu.Items.Add(autoToggleItem)
```

## CheckState Enumeration

The `CheckState` enum provides three states:

| State | Description | Use Case |
|-------|-------------|----------|
| `Checked` | Fully selected | Option is active |
| `Unchecked` | Not selected | Option is inactive |
| `Indeterminate` | Partially selected | Some sub-items checked, some not |

### Using Indeterminate State

**C# Example:**
```csharp
var parentItem = new ToolStripMenuItem("Select All Items");
parentItem.CheckState = CheckState.Indeterminate;  // Shows grayed checkmark

// Update based on child selection
private void UpdateParentState(ToolStripMenuItem parent, List<ToolStripMenuItem> children)
{
    int checkedCount = children.Count(c => c.Checked);
    
    if (checkedCount == 0)
        parent.CheckState = CheckState.Unchecked;
    else if (checkedCount == children.Count)
        parent.CheckState = CheckState.Checked;
    else
        parent.CheckState = CheckState.Indeterminate;
}
```

## Enabling and Disabling Menu Items

The `Enabled` property controls whether menu items are active (clickable) or inactive (grayed out).

### Key Properties

- **`Enabled`** (bool): True = active and clickable, False = grayed out and non-clickable
- Applies to **all ToolStripItem types** (MenuItem, TextBox, ComboBox)
- By default, all items are enabled when created

### Setting Enabled State

**Via Designer:**
1. Select the item in the designer
2. In Properties panel → Behavior section
3. Set **Enabled** to True or False

**Via Code:**
```csharp
// Disable items
toolStripMenuItem1.Enabled = false;
toolStripTextBox1.Enabled = false;
toolStripComboBox1.Enabled = false;

// Enable items
toolStripMenuItem1.Enabled = true;
toolStripTextBox1.Enabled = true;
toolStripComboBox1.Enabled = true;
```

## Dynamic State Management

Often you need to update states based on application context or user selection.

### Example 1: Context-Aware Enable/Disable

Update menu states based on current selection:

**C# Example:**
```csharp
private ContextMenuStripEx CreateContextMenu()
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
        bool hasClipboard = Clipboard.ContainsText();
        
        cutItem.Enabled = hasSelection;
        copyItem.Enabled = hasSelection;
        pasteItem.Enabled = hasClipboard;
    };
    
    return contextMenu;
}
```

### Example 2: Radio Button Behavior

Create mutually exclusive checked items (only one checked at a time):

**C# Example:**
```csharp
private void CreateRadioMenu()
{
    var contextMenu = new ContextMenuStripEx();
    contextMenu.ShowCheckMargin = true;
    
    var items = new ToolStripMenuItem[] {
        new ToolStripMenuItem("Option 1"),
        new ToolStripMenuItem("Option 2"),
        new ToolStripMenuItem("Option 3")
    };
    
    // Set first item as checked
    items[0].Checked = true;
    
    // Wire up mutual exclusion
    foreach (var item in items)
    {
        item.Click += (s, e) => {
            var clickedItem = s as ToolStripMenuItem;
            
            // Uncheck all items
            foreach (var otherItem in items)
                otherItem.Checked = false;
            
            // Check only the clicked item
            clickedItem.Checked = true;
            
            // Perform action based on selection
            HandleSelectionChanged(clickedItem.Text);
        };
        
        contextMenu.Items.Add(item);
    }
}
```

### Example 3: Multi-Select with Select All

**C# Example:**
```csharp
private void CreateMultiSelectMenu()
{
    var contextMenu = new ContextMenuStripEx();
    contextMenu.ShowCheckMargin = true;
    
    var selectAllItem = new ToolStripMenuItem("Select All");
    var separator = new ToolStripSeparator();
    
    var option1 = new ToolStripMenuItem("Option 1");
    var option2 = new ToolStripMenuItem("Option 2");
    var option3 = new ToolStripMenuItem("Option 3");
    
    option1.CheckOnClick = true;
    option2.CheckOnClick = true;
    option3.CheckOnClick = true;
    
    var childItems = new[] { option1, option2, option3 };
    
    // Select All handler
    selectAllItem.Click += (s, e) => {
        bool newState = !childItems.All(item => item.Checked);
        foreach (var item in childItems)
            item.Checked = newState;
        selectAllItem.Checked = newState;
    };
    
    // Child items update Select All state
    foreach (var item in childItems)
    {
        item.Click += (s, e) => {
            int checkedCount = childItems.Count(i => i.Checked);
            selectAllItem.Checked = checkedCount == childItems.Length;
            selectAllItem.CheckState = checkedCount == 0 ? CheckState.Unchecked :
                                       checkedCount == childItems.Length ? CheckState.Checked :
                                       CheckState.Indeterminate;
        };
    }
    
    contextMenu.Items.Add(selectAllItem);
    contextMenu.Items.Add(separator);
    contextMenu.Items.AddRange(childItems);
}
```

## Responding to State Changes

Use the `CheckedChanged` event to respond when checked state changes:

**C# Example:**
```csharp
var item = new ToolStripMenuItem("Watch for Changes");
item.CheckOnClick = true;

item.CheckedChanged += (s, e) => {
    var menuItem = s as ToolStripMenuItem;
    Console.WriteLine($"Item checked state changed to: {menuItem.Checked}");
    
    // Perform action when state changes
    UpdateApplicationState(menuItem.Checked);
};
```

## Common Patterns

### Pattern 1: View Options Menu

```csharp
var contextMenu = new ContextMenuStripEx();
contextMenu.ShowCheckMargin = true;

var showToolbar = new ToolStripMenuItem("Show Toolbar");
showToolbar.CheckOnClick = true;
showToolbar.Checked = true;
showToolbar.Click += (s, e) => toolbar.Visible = ((ToolStripMenuItem)s).Checked;

var showStatusBar = new ToolStripMenuItem("Show Status Bar");
showStatusBar.CheckOnClick = true;
showStatusBar.Checked = true;
showStatusBar.Click += (s, e) => statusBar.Visible = ((ToolStripMenuItem)s).Checked;

contextMenu.Items.AddRange(new ToolStripItem[] { showToolbar, showStatusBar });
```

### Pattern 2: Format Options (Bold, Italic, Underline)

```csharp
var contextMenu = new ContextMenuStripEx();
contextMenu.ShowCheckMargin = true;

var boldItem = new ToolStripMenuItem("Bold");
boldItem.CheckOnClick = true;
boldItem.Click += (s, e) => ApplyBold(((ToolStripMenuItem)s).Checked);

var italicItem = new ToolStripMenuItem("Italic");
italicItem.CheckOnClick = true;
italicItem.Click += (s, e) => ApplyItalic(((ToolStripMenuItem)s).Checked);

var underlineItem = new ToolStripMenuItem("Underline");
underlineItem.CheckOnClick = true;
underlineItem.Click += (s, e) => ApplyUnderline(((ToolStripMenuItem)s).Checked);

contextMenu.Items.AddRange(new ToolStripItem[] { boldItem, italicItem, underlineItem });
```

## Best Practices

1. **Always set ShowCheckMargin:** Check marks won't display without this
2. **Use CheckOnClick for simple toggles:** Reduces boilerplate code
3. **Update states in Opening event:** Ensure states reflect current application state
4. **Provide visual feedback:** Use Enabled to show unavailable actions
5. **Consider indeterminate for hierarchies:** Shows partial selection in parent items
6. **Be consistent:** Use checks for toggles, enabled/disabled for availability
7. **Combine states wisely:** Can have checked but disabled items for read-only states

## Troubleshooting

**Check marks not appearing:**
- Verify `ShowCheckMargin = true` on ContextMenuStripEx
- Ensure item is a MenuItem (not TextBox or ComboBox)
- Check that `Checked = true` is set

**State not toggling:**
- If using manual toggle, verify Click event handler toggles state
- If using CheckOnClick, ensure property is set to true
- Check that item is Enabled

**Disabled items still clickable:**
- Verify `Enabled = false` is set correctly
- Check no event handlers are overriding behavior
- Ensure code isn't re-enabling items unexpectedly

**Indeterminate state not showing:**
- Set `CheckState = CheckState.Indeterminate` (not just `Checked = true/false`)
- Verify ShowCheckMargin is true
- Some visual themes may not clearly distinguish indeterminate
