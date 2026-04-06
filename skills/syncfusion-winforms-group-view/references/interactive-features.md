# Interactive Features

This guide covers interactive features of the GroupView control, including ButtonView, selection bounds, tooltips, and context menus.

## ButtonView and Selection Bounds

Visual feedback features that help users understand the current selection state.

### ButtonView Property

Displays selected items in a pressed state for clear visual feedback.

```csharp
this.groupView1.ButtonView = true;  // Pressed/depressed visual state
```

**Use Cases:** Touch interfaces, toolbar-style behavior, accessibility

### ClipSelectionBounds Property

Draws a border around the selected item.

```csharp
this.groupView1.ClipSelectionBounds = true;  // Border around selection
```

**Use Cases:** High-density lists, minimal color differentiation

### Combining Both Features

```csharp
public void EnableEnhancedSelection()
{
    this.groupView1.ButtonView = true;
    this.groupView1.ClipSelectionBounds = true;
    this.groupView1.HighlightText = true;
    this.groupView1.SelectedItemColor = Color.FromArgb(200, 220, 240);
    this.groupView1.SelectedTextColor = Color.Black;
    this.groupView1.FlatLook = true;
    this.groupView1.SelectedItem = 0;
}
```

### Programmatic Selection

Set or get the currently selected item using the SelectedItem property:

```csharp
// Select an item by index (0-based)
this.groupView1.SelectedItem = 3;

// Get current selection
int currentIndex = this.groupView1.SelectedItem;
if (currentIndex != -1)
{
    string selectedText = this.groupView1.GroupViewItems[currentIndex].Text;
    MessageBox.Show($"Selected: {selectedText}");
}
else
{
    MessageBox.Show("No item selected");
}

// Deselect all items
this.groupView1.SelectedItem = -1;
```

## ToolTips

Configure tooltips to display helpful information when users hover over GroupView items.

### ShowToolTips Property

Enable tooltips for items:

```csharp
this.groupView1.ShowToolTips = true;
```

### Setting ToolTip Text

**Design-Time:** Properties → GroupViewItems → Collection Editor → Select item → Set ToolTipText

**Runtime:**

```csharp
this.groupView1.GroupViewItems[0].ToolTipText = "Open My Computer";
this.groupView1.GroupViewItems[1].ToolTipText = "Access Network Resources";
this.groupView1.ShowToolTips = true;

// Or during item creation
this.groupView1.GroupViewItems.AddRange(new GroupViewItem[] {
    new GroupViewItem("Documents", 0, true, "Access your documents folder", "item1"),
    new GroupViewItem("Pictures", 1, true, "View your photo library", "item2")
});
```

### Dynamic ToolTip Updates

```csharp
// Update tooltips based on state
for (int i = 0; i < this.groupView1.GroupViewItems.Count; i++)
{
    GroupViewItem item = this.groupView1.GroupViewItems[i];
    if (item.Text.Contains("Document"))
    {
        int docCount = GetDocumentCount();
        item.ToolTipText = $"Documents folder ({docCount} files)";
    }
}
```

## Context Menus

Implement right-click context menus for GroupView items.

### Context Menu Basics

The GroupView control provides properties to facilitate context menu implementation:

- **ContextMenuItem**: Returns the index of the item under the mouse when right-clicked
- **HighlightedItem**: Returns the index of the currently highlighted item
- **ShowContextMenu event**: Fires when the user right-clicks on the control

### ContextMenuItem Property

Access the item under the mouse cursor during context menu display:

```csharp
// Get the item index under the mouse
int itemIndex = this.groupView1.ContextMenuItem;

if (itemIndex != -1)
{
    // Mouse is over a specific item
    string itemText = this.groupView1.GroupViewItems[itemIndex].Text;
}
else
{
    // Mouse is over empty space in GroupView
}
```

### Implementing Context Menus

```csharp
public void SetupContextMenu()
{
    this.groupView1.ShowContextMenu += GroupView1_ShowContextMenu;
}

private void GroupView1_ShowContextMenu(object sender, EventArgs e)
{
    ContextMenuStrip contextMenu = new ContextMenuStrip();
    contextMenu.Items.Add("Add New Item", null, AddNewItem_Click);
    contextMenu.Items.Add(new ToolStripSeparator());
    
    if (this.groupView1.ContextMenuItem != -1)
    {
        // Item-specific menu
        contextMenu.Items.Add("Rename", null, RenameItem_Click);
        contextMenu.Items.Add("Delete", null, DeleteItem_Click);
        contextMenu.Items.Add("Properties", null, ShowProperties_Click);
    }
    else
    {
        // Empty space menu
        contextMenu.Items.Add("Refresh", null, RefreshView_Click);
    }
    
    contextMenu.Show(this.groupView1, this.groupView1.PointToClient(Cursor.Position));
}

private void AddNewItem_Click(object sender, EventArgs e)
{
    this.groupView1.GroupViewItems.Add(new GroupViewItem(
        "New Item", -1, true, "New item tooltip", $"item{this.groupView1.GroupViewItems.Count}"
    ));
}

private void RenameItem_Click(object sender, EventArgs e)
{
    int idx = this.groupView1.ContextMenuItem;
    if (idx != -1) this.groupView1.InplaceRenameItem(idx);
}

private void DeleteItem_Click(object sender, EventArgs e)
{
    int idx = this.groupView1.ContextMenuItem;
    if (idx != -1 && MessageBox.Show($"Delete '{this.groupView1.GroupViewItems[idx].Text}'?",
        "Confirm", MessageBoxButtons.YesNo) == DialogResult.Yes)
    {
        this.groupView1.GroupViewItems.RemoveAt(idx);
    }
}

private void ShowProperties_Click(object sender, EventArgs e)
{
    int idx = this.groupView1.ContextMenuItem;
    if (idx != -1)
    {
        GroupViewItem item = this.groupView1.GroupViewItems[idx];
        MessageBox.Show($"Item: {item.Text}\nIndex: {idx}\nName: {item.Name}", "Properties");
    }
}

private void RefreshView_Click(object sender, EventArgs e)
{
    this.groupView1.Refresh();
}
```

### Advanced Context Menu

Feature-rich menu with conditional items and submenus:

```csharp
private void ShowAdvancedContextMenu(object sender, EventArgs e)
{
    ContextMenuStrip menu = new ContextMenuStrip();
    int itemIndex = this.groupView1.ContextMenuItem;
    
    if (itemIndex != -1)
    {
        // Item-specific menu
        ToolStripMenuItem openItem = new ToolStripMenuItem("Open") { Font = new Font(menu.Font, FontStyle.Bold) };
        openItem.Click += (s, ev) => OpenItem(itemIndex);
        menu.Items.Add(openItem);
        menu.Items.Add(new ToolStripSeparator());
        
        menu.Items.Add("Cut", null, (s, ev) => CutItem(itemIndex));
        menu.Items.Add("Copy", null, (s, ev) => CopyItem(itemIndex));
        
        ToolStripMenuItem pasteItem = new ToolStripMenuItem("Paste") { Enabled = Clipboard.ContainsText() };
        pasteItem.Click += (s, ev) => PasteItem(itemIndex);
        menu.Items.Add(pasteItem);
        menu.Items.Add(new ToolStripSeparator());
        
        menu.Items.Add("Rename", null, (s, ev) => this.groupView1.InplaceRenameItem(itemIndex));
        menu.Items.Add("Delete", null, (s, ev) => DeleteItem(itemIndex));
        menu.Items.Add(new ToolStripSeparator());
        menu.Items.Add("Properties", null, (s, ev) => ShowProperties(itemIndex));
    }
    else
    {
        // Empty space menu with submenus
        menu.Items.Add("New Item", null, (s, ev) => AddNewItem());
        menu.Items.Add(new ToolStripSeparator());
        
        ToolStripMenuItem sortMenu = new ToolStripMenuItem("Sort by");
        sortMenu.DropDownItems.Add("Name", null, (s, ev) => SortByName());
        sortMenu.DropDownItems.Add("Date", null, (s, ev) => SortByDate());
        menu.Items.Add(sortMenu);
        
        ToolStripMenuItem viewMenu = new ToolStripMenuItem("View");
        viewMenu.DropDownItems.Add(new ToolStripMenuItem("Small Icons") 
            { Checked = this.groupView1.SmallImageView });
        viewMenu.DropDownItems.Add(new ToolStripMenuItem("Large Icons") 
            { Checked = !this.groupView1.SmallImageView });
        menu.Items.Add(viewMenu);
        
        menu.Items.Add(new ToolStripSeparator());
        menu.Items.Add("Refresh", null, (s, ev) => this.groupView1.Refresh());
    }
    
    menu.Show(this.groupView1, this.groupView1.PointToClient(Cursor.Position));
}
```

## Complete Example: Interactive GroupView

```csharp
private void SetupInteractiveGroupView()
{
    this.groupView1 = new GroupView();
    this.groupView1.Location = new Point(20, 20);
    this.groupView1.Size = new Size(300, 400);
    this.groupView1.FlatLook = true;
    this.groupView1.BorderStyle = BorderStyle.FixedSingle;
    
    // Selection feedback
    this.groupView1.ButtonView = true;
    this.groupView1.ClipSelectionBounds = true;
    
    // Tooltips
    this.groupView1.ShowToolTips = true;
    
    // Add items
    this.groupView1.GroupViewItems.AddRange(new GroupViewItem[] {
        new GroupViewItem("My Computer", 0, true, "Browse computer resources", "item1"),
        new GroupViewItem("Network", 1, true, "Access network locations", "item2"),
        new GroupViewItem("Recycle Bin", 2, true, "View deleted items", "item3")
    });
    
    this.groupView1.SelectedItem = 0;
    
    // Context menu
    this.groupView1.ShowContextMenu += (sender, e) =>
    {
        ContextMenuStrip menu = new ContextMenuStrip();
        if (this.groupView1.ContextMenuItem != -1)
        {
            menu.Items.Add("Open", null, (s, ev) => 
                MessageBox.Show($"Opening: {this.groupView1.GroupViewItems[this.groupView1.ContextMenuItem].Text}"));
            menu.Items.Add("Rename", null, (s, ev) => this.groupView1.InplaceRenameItem(this.groupView1.ContextMenuItem));
            menu.Items.Add("Delete", null, (s, ev) =>
            {
                if (MessageBox.Show("Delete?", "Confirm", MessageBoxButtons.YesNo) == DialogResult.Yes)
                    this.groupView1.GroupViewItems.RemoveAt(this.groupView1.ContextMenuItem);
            });
        }
        else
        {
            menu.Items.Add("Add Item", null, (s, ev) => 
                this.groupView1.GroupViewItems.Add(new GroupViewItem("New Item", -1, true, null, "newitem")));
        }
        menu.Show(this.groupView1, this.groupView1.PointToClient(Cursor.Position));
    };
    
    // Selection event
    this.groupView1.GroupViewItemSelected += (sender, e) =>
    {
        int idx = this.groupView1.SelectedItem;
        if (idx != -1)
            this.Text = $"Selected: {this.groupView1.GroupViewItems[idx].Text}";
    };
    
    this.Controls.Add(this.groupView1);
}
```

## Best Practices

**Selection Feedback:**
- Use ButtonView for touch interfaces; ClipSelectionBounds for high-density lists
- Combine both for maximum visibility

**ToolTips:**
- Keep concise (under 60 characters); provide helpful context
- Use dynamic tooltips for state/counts

**Context Menus:**
- Check ContextMenuItem to differentiate item vs. empty space menus
- Disable (don't hide) inapplicable items
- Make default actions bold; group related items with separators
