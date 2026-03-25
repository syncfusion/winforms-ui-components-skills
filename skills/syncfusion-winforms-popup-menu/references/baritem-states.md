# BarItem States in PopupMenu

## Table of Contents
- [Overview](#overview)
- [Checked/Unchecked States](#checkedunchecked-states)
- [Enabled/Disabled States](#enableddisabled-states)
- [State Management Patterns](#state-management-patterns)
- [Overlapping CheckBox and Images](#overlapping-checkbox-and-images)
- [Best Practices](#best-practices)

## Overview

BarItems support multiple states that control their appearance and behavior:
- **Checked/Unchecked:** Visual indicator (checkmark) for toggleable options
- **Enabled/Disabled:** Control whether items respond to user interaction

These states help users understand current settings and available options in your menu system.

## Checked/Unchecked States

The `Checked` property displays a checkmark before the menu item text, indicating the item's selection state. This is ideal for toggleable options like "Word Wrap", "Show Toolbar", or "Enable Feature".

### Basic Checked State

```csharp
BarItem wordWrapItem = new BarItem();
wordWrapItem.Text = "Word Wrap";
wordWrapItem.Checked = true;  // Show checkmark
wordWrapItem.SizeToFit = true;

parentBarItem1.Items.Add(wordWrapItem);
```

### VB.NET Example

```vb
Dim wordWrapItem As New BarItem()
wordWrapItem.Text = "Word Wrap"
wordWrapItem.Checked = True  ' Show checkmark
wordWrapItem.SizeToFit = True

parentBarItem1.Items.Add(wordWrapItem)
```

### Toggle on Click

Implement toggle behavior using the Click event:

```csharp
BarItem wordWrapItem = new BarItem();
wordWrapItem.Text = "Word Wrap";
wordWrapItem.Checked = true;
wordWrapItem.Click += WordWrapItem_Click;

parentBarItem1.Items.Add(wordWrapItem);

private void WordWrapItem_Click(object sender, EventArgs e)
{
    BarItem item = sender as BarItem;
    if (item != null)
    {
        // Toggle checked state
        item.Checked = !item.Checked;
        
        // Apply setting
        richTextBox1.WordWrap = item.Checked;
    }
}
```

### VB.NET Toggle Example

```vb
Dim wordWrapItem As New BarItem()
wordWrapItem.Text = "Word Wrap"
wordWrapItem.Checked = True
AddHandler wordWrapItem.Click, AddressOf WordWrapItem_Click

parentBarItem1.Items.Add(wordWrapItem)

Private Sub WordWrapItem_Click(sender As Object, e As EventArgs)
    Dim item As BarItem = TryCast(sender, BarItem)
    If item IsNot Nothing Then
        ' Toggle checked state
        item.Checked = Not item.Checked
        
        ' Apply setting
        richTextBox1.WordWrap = item.Checked
    End If
End Sub
```

### Multiple Checkable Items

```csharp
// View menu with multiple toggleable options
BarItem showToolbarItem = new BarItem { Text = "Show Toolbar", Checked = true };
BarItem showStatusBarItem = new BarItem { Text = "Show Status Bar", Checked = true };
BarItem showLineNumbersItem = new BarItem { Text = "Show Line Numbers", Checked = false };

showToolbarItem.Click += (s, e) => {
    showToolbarItem.Checked = !showToolbarItem.Checked;
    toolbar1.Visible = showToolbarItem.Checked;
};

showStatusBarItem.Click += (s, e) => {
    showStatusBarItem.Checked = !showStatusBarItem.Checked;
    statusBar1.Visible = showStatusBarItem.Checked;
};

showLineNumbersItem.Click += (s, e) => {
    showLineNumbersItem.Checked = !showLineNumbersItem.Checked;
    editor.ShowLineNumbers = showLineNumbersItem.Checked;
};

parentBarItem1.Items.AddRange(new BarItem[] {
    showToolbarItem,
    showStatusBarItem,
    showLineNumbersItem
});
```

### Radio Button-Style (Mutually Exclusive)

```csharp
BarItem viewNormalItem = new BarItem { Text = "Normal View", Checked = true };
BarItem viewCompactItem = new BarItem { Text = "Compact View", Checked = false };
BarItem viewDetailedItem = new BarItem { Text = "Detailed View", Checked = false };

BarItem[] viewModeItems = { viewNormalItem, viewCompactItem, viewDetailedItem };

viewNormalItem.Click += (s, e) => SetViewMode(viewNormalItem, viewModeItems);
viewCompactItem.Click += (s, e) => SetViewMode(viewCompactItem, viewModeItems);
viewDetailedItem.Click += (s, e) => SetViewMode(viewDetailedItem, viewModeItems);

parentBarItem1.Items.AddRange(viewModeItems);

private void SetViewMode(BarItem selectedItem, BarItem[] allItems)
{
    // Uncheck all items
    foreach (BarItem item in allItems)
    {
        item.Checked = false;
    }
    
    // Check selected item
    selectedItem.Checked = true;
    
    // Apply view mode
    ApplyViewMode(selectedItem.Text);
}
```

### Checked State with ParentBarItem

ParentBarItem also supports the Checked property:

```csharp
ParentBarItem formatMenu = new ParentBarItem();
formatMenu.Text = "Format";
formatMenu.Checked = true;  // Show checkmark on parent menu
formatMenu.SizeToFit = true;

// Add child items
formatMenu.Items.AddRange(new BarItem[] {
    new BarItem { Text = "Bold" },
    new BarItem { Text = "Italic" },
    new BarItem { Text = "Underline" }
});

parentBarItem1.Items.Add(formatMenu);
```

## Enabled/Disabled States

The `Enabled` property controls whether a BarItem responds to user interaction. Disabled items appear grayed out and do not respond to clicks.

### Basic Disabled State

```csharp
BarItem saveItem = new BarItem();
saveItem.Text = "Save";
saveItem.Enabled = false;  // Grayed out, no interaction
saveItem.SizeToFit = true;

parentBarItem1.Items.Add(saveItem);
```

### VB.NET Example

```vb
Dim saveItem As New BarItem()
saveItem.Text = "Save"
saveItem.Enabled = False  ' Grayed out, no interaction
saveItem.SizeToFit = True

parentBarItem1.Items.Add(saveItem)
```

### Conditional Enabling

Enable/disable items based on application state:

```csharp
BarItem cutItem = new BarItem { Text = "Cut", Shortcut = Shortcut.CtrlX };
BarItem copyItem = new BarItem { Text = "Copy", Shortcut = Shortcut.CtrlC };
BarItem pasteItem = new BarItem { Text = "Paste", Shortcut = Shortcut.CtrlV };

// Enable Cut/Copy only if text is selected
richTextBox1.SelectionChanged += (s, e) => {
    bool hasSelection = richTextBox1.SelectionLength > 0;
    cutItem.Enabled = hasSelection;
    copyItem.Enabled = hasSelection;
};

// Enable Paste only if clipboard has text
pasteItem.Click += (s, e) => {
    if (Clipboard.ContainsText())
    {
        richTextBox1.Paste();
    }
};

// Update paste state on menu popup
popupMenu1.BeforePopup += (s, e) => {
    pasteItem.Enabled = Clipboard.ContainsText();
};

parentBarItem1.Items.AddRange(new BarItem[] { cutItem, copyItem, pasteItem });
```

### Dynamic State Management

```csharp
private void UpdateMenuStates()
{
    // Document state
    bool hasDocument = currentDocument != null;
    bool isModified = hasDocument && currentDocument.IsModified;
    bool canUndo = hasDocument && currentDocument.CanUndo;
    bool canRedo = hasDocument && currentDocument.CanRedo;
    
    // Update item states
    saveItem.Enabled = hasDocument && isModified;
    saveAsItem.Enabled = hasDocument;
    closeItem.Enabled = hasDocument;
    undoItem.Enabled = canUndo;
    redoItem.Enabled = canRedo;
    
    // Update checked states
    wordWrapItem.Checked = hasDocument && currentDocument.WordWrap;
    showLineNumbersItem.Checked = hasDocument && currentDocument.ShowLineNumbers;
}

// Call on popup to refresh states
popupMenu1.BeforePopup += (s, e) => UpdateMenuStates();
```

### Disabling Item Groups

```csharp
private void SetEditMenuState(bool enabled)
{
    foreach (BarItem item in editMenuParent.Items)
    {
        item.Enabled = enabled;
    }
}

// Usage: Disable all edit operations when read-only
if (document.IsReadOnly)
{
    SetEditMenuState(false);
}
```

### Note on Enabled State

> **Important:** The `Enabled` property is **not applicable** for `ListBarItem` and `StaticBarItem` as these items don't support user interaction in the same way.

## State Management Patterns

### Pattern 1: BeforePopup Event for Fresh States

Always update menu states before showing the popup:

```csharp
popupMenu1.BeforePopup += PopupMenu1_BeforePopup;

private void PopupMenu1_BeforePopup(object sender, CancelEventArgs e)
{
    // Update enabled states
    cutItem.Enabled = richTextBox1.SelectionLength > 0;
    copyItem.Enabled = richTextBox1.SelectionLength > 0;
    pasteItem.Enabled = Clipboard.ContainsText();
    undoItem.Enabled = richTextBox1.CanUndo;
    
    // Update checked states
    wordWrapItem.Checked = richTextBox1.WordWrap;
}
```

### Pattern 2: State Synchronization

Keep menu states synchronized with application state:

```csharp
private bool _wordWrap = true;

public bool WordWrap
{
    get { return _wordWrap; }
    set
    {
        _wordWrap = value;
        richTextBox1.WordWrap = value;
        wordWrapItem.Checked = value;  // Sync menu state
    }
}

// Usage
WordWrap = false;  // Updates both UI and menu
```

### Pattern 3: State Restoration

Save and restore menu states:

```csharp
private Dictionary<string, bool> SaveMenuStates()
{
    var states = new Dictionary<string, bool>();
    foreach (BarItem item in parentBarItem1.Items)
    {
        states[item.Text] = item.Checked;
    }
    return states;
}

private void RestoreMenuStates(Dictionary<string, bool> states)
{
    foreach (BarItem item in parentBarItem1.Items)
    {
        if (states.ContainsKey(item.Text))
        {
            item.Checked = states[item.Text];
        }
    }
}
```

## Overlapping CheckBox and Images

When both `Checked` state and `Image` are set, you can control their display behavior using the `OverlapCheckBoxImageBounds` property on ParentBarItem.

### OverlapCheckBoxImageBounds = true (Default)

Checkbox and image occupy the same space. A border around the image indicates the checked state.

```csharp
ParentBarItem parentBarItem1 = new ParentBarItem();
parentBarItem1.OverlapCheckBoxImageBounds = true;  // Default

BarItem boldItem = new BarItem();
boldItem.Text = "Bold";
boldItem.Image = new ImageExt(Properties.Resources.BoldIcon);
boldItem.Checked = true;  // Border drawn around image

parentBarItem1.Items.Add(boldItem);
```

### OverlapCheckBoxImageBounds = false

Checkbox and image are displayed separately (checkbox on left, image next to text).

```csharp
ParentBarItem parentBarItem1 = new ParentBarItem();
parentBarItem1.OverlapCheckBoxImageBounds = false;

BarItem boldItem = new BarItem();
boldItem.Text = "Bold";
boldItem.Image = new ImageExt(Properties.Resources.BoldIcon);
boldItem.Checked = true;  // Separate checkmark displayed

parentBarItem1.Items.Add(boldItem);
```

### VB.NET Example

```vb
Dim parentBarItem1 As New ParentBarItem()
parentBarItem1.OverlapCheckBoxImageBounds = False

Dim boldItem As New BarItem()
boldItem.Text = "Bold"
boldItem.Image = New ImageExt(My.Resources.BoldIcon)
boldItem.Checked = True

parentBarItem1.Items.Add(boldItem)
```

### When to Use Each Mode

**OverlapCheckBoxImageBounds = true (Default):**
- Icon-based menus (toolbar-style menus)
- When image is primary visual indicator
- To save horizontal space
- Standard Windows application style

**OverlapCheckBoxImageBounds = false:**
- When both checkbox and icon provide independent information
- Text-heavy menus where separation improves clarity
- When checkbox state is primary information

## Best Practices

### Checked States
- Use for toggleable options (on/off settings)
- Update state immediately on click
- Sync with actual application state
- Use BeforePopup to ensure states are current
- Implement radio-style groups for mutually exclusive options

### Enabled States
- Disable unavailable commands (don't hide them)
- Update on BeforePopup event for accuracy
- Provide visual feedback via DisabledImage property
- Keep disabled state logic simple and predictable

### State Consistency
- Always sync menu state with application state
- Update states before showing menu
- Avoid state changes while menu is visible
- Test state logic with various scenarios

### Visual Clarity
- Don't mix checked and unchecked items randomly
- Group checkable items together
- Use separators to divide checkable groups
- Consider OverlapCheckBoxImageBounds based on design

### Performance
- Cache item references for frequent state updates
- Batch state updates when possible
- Use BeforePopup, not Popup, for state updates
- Avoid recreating items to change states

## Troubleshooting

**Issue: Checked state not visible**
- Verify `Checked = true` is set
- Check that text is visible (SizeToFit = true)
- If using images, check OverlapCheckBoxImageBounds setting

**Issue: Item stays enabled when it should be disabled**
- Verify `Enabled = false` is set
- Check if state update logic is being called
- Ensure BeforePopup event is wired up
- Look for code that re-enables the item

**Issue: State changes don't apply**
- Ensure you're modifying the correct BarItem instance
- Verify item is actually in the Items collection
- Check if menu is being recreated (losing state)
- Confirm state update happens before menu display

**Issue: Toggle doesn't work**
- Verify Click event is wired up
- Check that item.Checked assignment is in Click handler
- Ensure Enabled = true (disabled items don't fire Click)
- Look for exceptions in event handler
