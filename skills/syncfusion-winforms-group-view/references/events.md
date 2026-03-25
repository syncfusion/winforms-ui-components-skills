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

### Common Scenarios

#### Update UI Based on Selection

```csharp
private void GroupView1_ItemSelected(object sender, EventArgs e)
{
    int index = this.groupView1.SelectedItem;
    
    if (index != -1)
    {
        // Update form title
        this.Text = $"Viewing: {this.groupView1.GroupViewItems[index].Text}";
        
        // Enable/disable buttons
        deleteButton.Enabled = true;
        editButton.Enabled = true;
        
        // Load content in another panel
        LoadContentForItem(index);
    }
    else
    {
        // No selection
        this.Text = "GroupView Demo";
        deleteButton.Enabled = false;
        editButton.Enabled = false;
    }
}
```

#### Navigate Based on Selection

```csharp
private void GroupView1_ItemSelected(object sender, EventArgs e)
{
    int index = this.groupView1.SelectedItem;
    
    switch (index)
    {
        case 0: // My Computer
            ShowComputerView();
            break;
        case 1: // Network
            ShowNetworkView();
            break;
        case 2: // Recycle Bin
            ShowRecycleBinView();
            break;
        default:
            ShowDefaultView();
            break;
    }
}
```

#### Log Selection History

```csharp
private List<string> selectionHistory = new List<string>();

private void GroupView1_ItemSelected(object sender, EventArgs e)
{
    int index = this.groupView1.SelectedItem;
    
    if (index != -1)
    {
        string itemText = this.groupView1.GroupViewItems[index].Text;
        selectionHistory.Add($"{DateTime.Now}: {itemText}");
        
        // Keep last 10 selections
        if (selectionHistory.Count > 10)
        {
            selectionHistory.RemoveAt(0);
        }
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

### Common Scenarios

#### Display Preview on Hover

```csharp
private void GroupView1_ItemHighlighted(object sender, EventArgs e)
{
    int index = this.groupView1.HighlightedItem;
    
    if (index != -1)
    {
        // Show preview in side panel
        GroupViewItem item = this.groupView1.GroupViewItems[index];
        previewPanel.Visible = true;
        previewLabel.Text = item.Text;
        previewDescription.Text = item.ToolTipText ?? "No description available";
    }
    else
    {
        // Hide preview
        previewPanel.Visible = false;
    }
}
```

#### Update Status Bar

```csharp
private void GroupView1_ItemHighlighted(object sender, EventArgs e)
{
    // Ensure HighlightText is enabled
    this.groupView1.HighlightText = true;
    this.groupView1.HighlightItemColor = Color.AliceBlue;
    
    int index = this.groupView1.HighlightedItem;
    
    if (index != -1)
    {
        statusStripLabel.Text = $"Item: {this.groupView1.GroupViewItems[index].Text}";
    }
    else
    {
        statusStripLabel.Text = $"Total items: {this.groupView1.GroupViewItems.Count}";
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

### Common Scenarios

#### Validate Rename Operation

```csharp
private void GroupView1_ItemRenamed(object sender, EventArgs e)
{
    GroupItemRenamedEventArgs args = (GroupItemRenamedEventArgs)e;
    
    // Validate new name
    if (string.IsNullOrWhiteSpace(args.NewLabel))
    {
        MessageBox.Show("Name cannot be empty. Reverting to original name.");
        this.groupView1.GroupViewItems[args.Item].Text = args.OldLabel;
        return;
    }
    
    // Check for duplicates
    foreach (GroupViewItem item in this.groupView1.GroupViewItems)
    {
        if (item.Text == args.NewLabel && item.Name != this.groupView1.GroupViewItems[args.Item].Name)
        {
            MessageBox.Show("An item with this name already exists.");
            this.groupView1.GroupViewItems[args.Item].Text = args.OldLabel;
            return;
        }
    }
    
    // Log the rename
    LogRenameOperation(args.OldLabel, args.NewLabel);
}
```

#### Update External Data

```csharp
private Dictionary<string, string> itemData = new Dictionary<string, string>();

private void GroupView1_ItemRenamed(object sender, EventArgs e)
{
    GroupItemRenamedEventArgs args = (GroupItemRenamedEventArgs)e;
    
    // Update external data structure
    if (itemData.ContainsKey(args.OldLabel))
    {
        string data = itemData[args.OldLabel];
        itemData.Remove(args.OldLabel);
        itemData[args.NewLabel] = data;
    }
    
    // Update database or file system
    UpdatePersistedData(args.OldLabel, args.NewLabel);
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

### Common Scenarios

#### Save New Order

```csharp
private void GroupView1_ItemsReordered(object sender, EventArgs e)
{
    // Save new order to settings or database
    List<string> newOrder = new List<string>();
    
    foreach (GroupViewItem item in this.groupView1.GroupViewItems)
    {
        newOrder.Add(item.Name);
    }
    
    SaveItemOrder(newOrder);
    statusLabel.Text = "Item order saved";
}
```

#### Update Priority

```csharp
private void GroupView1_ItemsReordered(object sender, EventArgs e)
{
    // Update priority based on position (higher position = higher priority)
    for (int i = 0; i < this.groupView1.GroupViewItems.Count; i++)
    {
        GroupViewItem item = this.groupView1.GroupViewItems[i];
        UpdateItemPriority(item.Name, i);
    }
}
```

#### Refresh Dependent UI

```csharp
private void GroupView1_ItemsReordered(object sender, EventArgs e)
{
    // Refresh other controls that depend on item order
    RefreshNavigationMenu();
    RefreshWorkflowSteps();
    
    // Notify user
    toolStripStatusLabel.Text = "Order updated";
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

### Common Scenarios

#### Open or Execute Item

```csharp
private void GroupView1_ItemDoubleClick(GroupView sender, GroupViewItemDoubleClickEventArgs e)
{
    GroupViewItem item = e.SelectedItem;
    
    // Execute action based on item
    switch (item.Name)
    {
        case "itemDocuments":
            OpenDocumentsFolder();
            break;
        case "itemSettings":
            OpenSettingsDialog();
            break;
        case "itemHelp":
            ShowHelpWindow();
            break;
        default:
            DefaultOpenAction(item);
            break;
    }
}
```

#### Enable In-Place Renaming

```csharp
private void GroupView1_ItemDoubleClick(GroupView sender, GroupViewItemDoubleClickEventArgs e)
{
    // Find index of double-clicked item
    int index = -1;
    for (int i = 0; i < this.groupView1.GroupViewItems.Count; i++)
    {
        if (this.groupView1.GroupViewItems[i] == e.SelectedItem)
        {
            index = i;
            break;
        }
    }
    
    if (index != -1)
    {
        // Start in-place rename
        this.groupView1.InplaceRenameItem(index);
    }
}
```

#### Show Details Dialog

```csharp
private void GroupView1_ItemDoubleClick(GroupView sender, GroupViewItemDoubleClickEventArgs e)
{
    GroupViewItem item = e.SelectedItem;
    
    // Show details form
    ItemDetailsForm detailsForm = new ItemDetailsForm();
    detailsForm.ItemName = item.Text;
    detailsForm.ItemTooltip = item.ToolTipText;
    detailsForm.ItemImageIndex = item.ImageIndex;
    detailsForm.ShowDialog();
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

## Complete Events Example

Comprehensive example implementing all events:

```csharp
public partial class ComprehensiveEventsForm : Form
{
    private GroupView groupView1;
    private Label statusLabel;
    private Panel detailsPanel;
    
    public ComprehensiveEventsForm()
    {
        InitializeComponent();
        SetupGroupViewWithAllEvents();
    }
    
    private void SetupGroupViewWithAllEvents()
    {
        // Create and configure GroupView
        this.groupView1 = new GroupView();
        this.groupView1.Location = new Point(20, 20);
        this.groupView1.Size = new Size(250, 400);
        this.groupView1.FlatLook = true;
        this.groupView1.AllowDragDrop = true;
        
        // Add items
        this.groupView1.GroupViewItems.AddRange(new GroupViewItem[] {
            new GroupViewItem("Item 1", -1, true, "First item", "item1"),
            new GroupViewItem("Item 2", -1, true, "Second item", "item2"),
            new GroupViewItem("Item 3", -1, true, "Third item", "item3")
        });
        
        // Selection event
        this.groupView1.GroupViewItemSelected += (sender, e) =>
        {
            int index = this.groupView1.SelectedItem;
            statusLabel.Text = index != -1 
                ? $"Selected: {this.groupView1.GroupViewItems[index].Text}"
                : "No selection";
        };
        
        // Highlight event
        this.groupView1.HighlightText = true;
        this.groupView1.GroupViewItemHighlighted += (sender, e) =>
        {
            int index = this.groupView1.HighlightedItem;
            if (index != -1)
            {
                this.Text = $"Hovering: {this.groupView1.GroupViewItems[index].Text}";
            }
            else
            {
                this.Text = "GroupView Events Demo";
            }
        };
        
        // Rename event
        this.groupView1.GroupViewItemRenamed += (sender, e) =>
        {
            var args = (GroupItemRenamedEventArgs)e;
            MessageBox.Show($"Renamed:\n'{args.OldLabel}' → '{args.NewLabel}'");
        };
        
        // Reorder event
        this.groupView1.GroupViewItemsReordered += (sender, e) =>
        {
            MessageBox.Show("Items reordered!");
        };
        
        // Double-click event
        this.groupView1.GroupViewItemDoubleClick += (sender, e) =>
        {
            // Start rename on double-click
            for (int i = 0; i < this.groupView1.GroupViewItems.Count; i++)
            {
                if (this.groupView1.GroupViewItems[i] == e.SelectedItem)
                {
                    this.groupView1.InplaceRenameItem(i);
                    break;
                }
            }
        };
        
        // Context menu event
        this.groupView1.ShowContextMenu += (sender, e) =>
        {
            ContextMenuStrip menu = new ContextMenuStrip();
            menu.Items.Add("Add Item", null, (s, ev) => {
                this.groupView1.GroupViewItems.Add(
                    new GroupViewItem($"New Item {this.groupView1.GroupViewItems.Count + 1}", -1, true, null, "newitem")
                );
            });
            menu.Show(this.groupView1, this.groupView1.PointToClient(Cursor.Position));
        };
        
        // Add to form
        this.Controls.Add(this.groupView1);
    }
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
