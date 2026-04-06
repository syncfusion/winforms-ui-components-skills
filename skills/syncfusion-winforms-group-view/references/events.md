# GroupView Events

This guide covers all events provided by the GroupView control, including selection, highlighting, renaming, reordering, and double-click events.

## Table of Contents
- [GroupViewItemSelected Event](#groupviewitemselected-event)
- [GroupViewItemHighlighted Event](#groupviewitemhighlighted-event)
- [GroupViewItemRenamed Event](#groupviewitemrenamed-event)
- [GroupViewItemsReordered Event](#groupviewitemsreordered-event)
- [GroupViewItemDoubleClick Event](#groupviewitemdoubleclick-event)
- [ShowContextMenu Event](#showcontextmenu-event)
- [Common Event Patterns](#common-event-patterns)

## GroupViewItemSelected Event

Fires when a GroupView item is selected by the user.

### Event Signature

```csharp
public event EventHandler GroupViewItemSelected;
```

### Event Arguments

- **Type:** `EventArgs`
- **Properties:** None (use GroupView.SelectedItem property to get selected index)

### Usage

```csharp
// Subscribe to event
this.groupView1.GroupViewItemSelected += GroupView1_ItemSelected;

// Event handler
private void GroupView1_ItemSelected(object sender, EventArgs e)
{
    // Get selected item index
    int selectedIndex = this.groupView1.SelectedItem;
    
    if (selectedIndex >= 0 && selectedIndex < this.groupView1.GroupViewItems.Count)
    {
        GroupViewItem selectedItem = this.groupView1.GroupViewItems[selectedIndex];
        string itemText = selectedItem.Text;
        string itemName = selectedItem.Name;
        
        MessageBox.Show($"Selected: {itemText} (Index: {selectedIndex})");
    }
}
```

### Example: Update UI Based on Selection

```csharp
private void GroupView1_ItemSelected(object sender, EventArgs e)
{
    int index = this.groupView1.SelectedItem;
    
    if (index != -1)
    {
        this.Text = $"Viewing: {this.groupView1.GroupViewItems[index].Text}";
        deleteButton.Enabled = true;
        LoadContentForItem(index);
    }
    else
    {
        this.Text = "GroupView Demo";
        deleteButton.Enabled = false;
    }
}
```

## GroupViewItemHighlighted Event

Fires when a GroupView item is highlighted (mouse hover).

### Event Signature

```csharp
public event EventHandler GroupViewItemHighlighted;
```

### Event Arguments

- **Type:** `EventArgs`
- **Properties:** None (use GroupView.HighlightedItem property to get highlighted index)

### Usage

```csharp
// Subscribe to event
this.groupView1.GroupViewItemHighlighted += GroupView1_ItemHighlighted;

// Event handler
private void GroupView1_ItemHighlighted(object sender, EventArgs e)
{
    int highlightedIndex = this.groupView1.HighlightedItem;
    
    if (highlightedIndex >= 0 && highlightedIndex < this.groupView1.GroupViewItems.Count)
    {
        GroupViewItem item = this.groupView1.GroupViewItems[highlightedIndex];
        statusLabel.Text = $"Hover: {item.Text}";
    }
    else
    {
        statusLabel.Text = "Ready";
    }
}
```

### Example: Display Status on Hover

```csharp
private void GroupView1_ItemHighlighted(object sender, EventArgs e)
{
    int index = this.groupView1.HighlightedItem;
    
    if (index != -1)
    {
        GroupViewItem item = this.groupView1.GroupViewItems[index];
        statusLabel.Text = $"Hover: {item.Text}";
    }
    else
    {
        statusLabel.Text = "Ready";
    }
}
```

## GroupViewItemRenamed Event

Fires when a GroupView item has been renamed through in-place editing.

### Event Signature

```csharp
public event GroupItemRenamedEventHandler GroupViewItemRenamed;
```

### Event Arguments

- **Type:** `GroupItemRenamedEventArgs`
- **Properties:**
  - `OldLabel` (string): The original item text before renaming
  - `NewLabel` (string): The new item text after renaming
  - `Item` (int): The index of the renamed item

### Usage

```csharp
// Subscribe to event
this.groupView1.GroupViewItemRenamed += GroupView1_ItemRenamed;

// Event handler
private void GroupView1_ItemRenamed(object sender, EventArgs e)
{
    GroupItemRenamedEventArgs args = (GroupItemRenamedEventArgs)e;
    
    string oldName = args.OldLabel;
    string newName = args.NewLabel;
    int itemIndex = args.Item;
    
    MessageBox.Show($"Item {itemIndex} renamed:\n'{oldName}' → '{newName}'");
}
```



## GroupViewItemsReordered Event

Fires when GroupView items have been reordered through drag-and-drop.

### Event Signature

```csharp
public event EventHandler GroupViewItemsReordered;
```

### Event Arguments

- **Type:** `EventArgs`
- **Properties:** None (examine GroupViewItems collection for new order)

### Usage

```csharp
// Enable drag-and-drop first
this.groupView1.AllowDragDrop = true;

// Subscribe to event
this.groupView1.GroupViewItemsReordered += GroupView1_ItemsReordered;

// Event handler
private void GroupView1_ItemsReordered(object sender, EventArgs e)
{
    MessageBox.Show("Items have been reordered");
    
    // Log new order
    for (int i = 0; i < this.groupView1.GroupViewItems.Count; i++)
    {
        Console.WriteLine($"Position {i}: {this.groupView1.GroupViewItems[i].Text}");
    }
}
```

## GroupViewItemDoubleClick Event

Fires when a GroupView item is double-clicked.

### Event Signature

```csharp
public event GroupViewItemDoubleClickEventHandler GroupViewItemDoubleClick;
```

### Event Arguments

- **Type:** `GroupViewItemDoubleClickEventArgs`
- **Properties:**
  - `SelectedItem` (GroupViewItem): The double-clicked item object

### Usage

```csharp
// Subscribe to event
this.groupView1.GroupViewItemDoubleClick += GroupView1_ItemDoubleClick;

// Event handler
private void GroupView1_ItemDoubleClick(GroupView sender, GroupViewItemDoubleClickEventArgs e)
{
    GroupViewItem clickedItem = e.SelectedItem;
    string itemText = clickedItem.Text;
    string itemName = clickedItem.Name;
    
    MessageBox.Show($"Double-clicked: {itemText}");
}
```

## ShowContextMenu Event

Fires when the user right-clicks on the GroupView control. See the [Interactive Features](interactive-features.md) guide for detailed context menu implementation.

### Event Signature

```csharp
public event EventHandler ShowContextMenu;
```

### Event Arguments

- **Type:** `EventArgs`
- **Properties:** None (use GroupView.ContextMenuItem property)

### Usage

```csharp
// Subscribe to event
this.groupView1.ShowContextMenu += GroupView1_ShowContextMenu;

// Event handler
private void GroupView1_ShowContextMenu(object sender, EventArgs e)
{
    ContextMenuStrip menu = new ContextMenuStrip();
    
    // Check if right-click was over an item
    if (this.groupView1.ContextMenuItem != -1)
    {
        // Item-specific menu
        menu.Items.Add("Open", null, (s, ev) => OpenItem());
        menu.Items.Add("Rename", null, (s, ev) => RenameItem());
        menu.Items.Add("Delete", null, (s, ev) => DeleteItem());
    }
    else
    {
        // General menu
        menu.Items.Add("Add Item", null, (s, ev) => AddItem());
        menu.Items.Add("Refresh", null, (s, ev) => this.groupView1.Refresh());
    }
    
    menu.Show(this.groupView1, this.groupView1.PointToClient(Cursor.Position));
}
```

## Common Event Patterns

### Pattern 1: Coordinated Events

Handle multiple events together for comprehensive functionality:

```csharp
public void SetupCoordinatedEvents()
{
    // Selection changes
    this.groupView1.GroupViewItemSelected += (sender, e) =>
    {
        int index = this.groupView1.SelectedItem;
        if (index != -1)
        {
            statusLabel.Text = $"Selected: {this.groupView1.GroupViewItems[index].Text}";
            detailsPanel.Visible = true;
            LoadDetails(index);
        }
    };
    
    // Hover preview
    this.groupView1.GroupViewItemHighlighted += (sender, e) =>
    {
        int index = this.groupView1.HighlightedItem;
        if (index != -1)
        {
            tooltipLabel.Text = this.groupView1.GroupViewItems[index].ToolTipText ?? "";
        }
    };
    
    // Double-click to open
    this.groupView1.GroupViewItemDoubleClick += (sender, e) =>
    {
        OpenItem(e.SelectedItem);
    };
    
    // Rename validation
    this.groupView1.GroupViewItemRenamed += (sender, e) =>
    {
        var args = (GroupItemRenamedEventArgs)e;
        ValidateAndSaveRename(args.Item, args.OldLabel, args.NewLabel);
    };
    
    // Order persistence
    this.groupView1.GroupViewItemsReordered += (sender, e) =>
    {
        SaveItemOrder();
    };
}
```

### Pattern 2: Event-Driven State Management

Use events to maintain application state:

```csharp
public class StateManagement
{
    private string currentSelection;
    private List<string> recentSelections = new List<string>();
    
    public void SetupStateManagement(GroupView groupView)
    {
        groupView.GroupViewItemSelected += (sender, e) =>
        {
            int index = groupView.SelectedItem;
            if (index != -1)
            {
                currentSelection = groupView.GroupViewItems[index].Name;
                AddToRecent(currentSelection);
            }
        };
        
        groupView.GroupViewItemsReordered += (sender, e) =>
        {
            // Clear state when order changes
            currentSelection = null;
        };
        
        groupView.GroupViewItemRenamed += (sender, e) =>
        {
            var args = (GroupItemRenamedEventArgs)e;
            
            // Update state with new name
            if (currentSelection == args.OldLabel)
            {
                currentSelection = args.NewLabel;
            }
            
            UpdateRecentSelections(args.OldLabel, args.NewLabel);
        };
    }
    
    private void AddToRecent(string item)
    {
        recentSelections.Remove(item); // Remove if exists
        recentSelections.Insert(0, item); // Add to front
        
        if (recentSelections.Count > 10)
        {
            recentSelections.RemoveAt(10); // Keep last 10
        }
    }
    
    private void UpdateRecentSelections(string oldName, string newName)
    {
        for (int i = 0; i < recentSelections.Count; i++)
        {
            if (recentSelections[i] == oldName)
            {
                recentSelections[i] = newName;
            }
        }
    }
}
```

### Pattern 3: Async Operations

Handle events with asynchronous operations:

```csharp
public void SetupAsyncEvents()
{
    this.groupView1.GroupViewItemSelected += async (sender, e) =>
    {
        int index = this.groupView1.SelectedItem;
        if (index != -1)
        {
            statusLabel.Text = "Loading...";
            
            try
            {
                var data = await LoadDataAsync(index);
                DisplayData(data);
                statusLabel.Text = "Ready";
            }
            catch (Exception ex)
            {
                statusLabel.Text = $"Error: {ex.Message}";
            }
        }
    };
    
    this.groupView1.GroupViewItemRenamed += async (sender, e) =>
    {
        var args = (GroupItemRenamedEventArgs)e;
        
        try
        {
            await SaveRenameToServerAsync(args.OldLabel, args.NewLabel);
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Failed to save rename: {ex.Message}");
            // Revert rename
            this.groupView1.GroupViewItems[args.Item].Text = args.OldLabel;
        }
    };
}

private async Task<object> LoadDataAsync(int index)
{
    // Simulate async data loading
    await Task.Delay(500);
    return new { Index = index, Data = "Sample data" };
}

private async Task SaveRenameToServerAsync(string oldName, string newName)
{
    // Simulate async save
    await Task.Delay(200);
}
```



## Best Practices

### Event Handling
- **Unsubscribe from events** when disposing controls to prevent memory leaks
- **Use lambda expressions** for simple event handlers
- **Create named methods** for complex event handlers with multiple operations
- **Handle exceptions** within event handlers to prevent application crashes

### Performance
- **Avoid heavy operations** in frequently fired events (e.g., Highlighted)
- **Use async/await** for I/O operations in event handlers
- **Debounce rapid events** if performing expensive operations
- **Cache frequently accessed data** rather than recalculating in event handlers

### User Experience
- **Provide visual feedback** for all user actions
- **Display appropriate messages** for errors or validation failures
- **Maintain state consistency** across related events
- **Test all event scenarios** including edge cases and rapid interactions
