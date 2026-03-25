# Interactive Features

This guide covers interactive features of the GroupView control, including ButtonView, selection bounds, tooltips, and context menus.

## ButtonView and Selection Bounds

Visual feedback features that help users understand the current selection state.

### ButtonView Property

The **ButtonView** property displays selected GroupView items in a pressed state, providing clear visual feedback that an item is selected.

```csharp
// Enable button view for selected items
this.groupView1.ButtonView = true;
```

**Behavior:**
- **true**: Selected items appear with a pressed/depressed visual state
- **false**: Selected items use only color-based selection indication (default)

**Visual Effect:**
When ButtonView is enabled, the selected item displays with:
- Inset border (appears pressed into the surface)
- Shadow effects suggesting depth
- Clear distinction from non-selected items

**Use Cases:**
- Touch-friendly interfaces where visual feedback is critical
- Applications mimicking toolbar or button panel behavior
- Scenarios where color-based selection may be insufficient

**Example:**

```csharp
public void EnableButtonView()
{
    this.groupView1.ButtonView = true;
    
    // Set initial selection to demonstrate effect
    this.groupView1.SelectedItem = 0;
    
    // Configure selection colors for better visibility
    this.groupView1.HighlightText = true;
    this.groupView1.SelectedItemColor = Color.LightGray;
    this.groupView1.SelectedTextColor = Color.Black;
}
```

### ClipSelectionBounds Property

The **ClipSelectionBounds** property draws a border around the selected GroupView item, providing additional visual emphasis.

```csharp
// Enable selection border
this.groupView1.ClipSelectionBounds = true;
```

**Behavior:**
- **true**: A white (or contrasting) border appears around the selected item
- **false**: No border is drawn (default)

**Visual Effect:**
A distinct border (typically white or light-colored) surrounds the selected item, making it stand out from surrounding items.

**Use Cases:**
- High-density item lists where selection needs extra emphasis
- Interfaces with minimal color differentiation
- Accessibility scenarios requiring clear visual indicators

**Example:**

```csharp
public void EnableSelectionBorder()
{
    this.groupView1.ClipSelectionBounds = true;
    
    // Set initial selection
    this.groupView1.SelectedItem = 2;
}
```

### Combining ButtonView and ClipSelectionBounds

For maximum visual feedback, combine both features:

```csharp
public void EnableEnhancedSelection()
{
    // Enable both visual feedback mechanisms
    this.groupView1.ButtonView = true;
    this.groupView1.ClipSelectionBounds = true;
    
    // Configure colors for optimal appearance
    this.groupView1.HighlightText = true;
    this.groupView1.SelectedItemColor = Color.FromArgb(200, 220, 240);
    this.groupView1.SelectedTextColor = Color.Black;
    
    // Flat appearance works well with ButtonView
    this.groupView1.FlatLook = true;
    
    // Set initial selection
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

Enable or disable tooltips for the GroupView control.

```csharp
// Enable tooltips for all items
this.groupView1.ShowToolTips = true;
```

**Behavior:**
- **true**: Tooltips appear when hovering over items with ToolTipText set
- **false**: Tooltips are suppressed (default)

### Setting ToolTip Text for Items

Configure tooltip text for individual items using the ToolTipText property.

**Via Collection Editor (Design-Time):**
1. Select GroupView control
2. Open Properties window
3. Click GroupViewItems property → Collection Editor
4. Select an item
5. Set the **ToolTipText** property
6. Click OK

**Via Code (Runtime):**

```csharp
// Set tooltip text for each item
this.groupView1.GroupViewItems[0].ToolTipText = "Open My Computer";
this.groupView1.GroupViewItems[1].ToolTipText = "Access Network Resources";
this.groupView1.GroupViewItems[2].ToolTipText = "View Recycle Bin Contents";

// Enable tooltips
this.groupView1.ShowToolTips = true;
```

### Complete ToolTip Example

```csharp
public void ConfigureToolTips()
{
    // Create GroupView
    this.groupView1 = new GroupView();
    this.groupView1.Size = new Size(200, 300);
    this.groupView1.FlatLook = true;
    
    // Add items with tooltips
    this.groupView1.GroupViewItems.AddRange(new GroupViewItem[] {
        new GroupViewItem("Documents", 0, true, "Access your documents folder", "item1"),
        new GroupViewItem("Pictures", 1, true, "View your photo library", "item2"),
        new GroupViewItem("Music", 2, true, "Browse your music collection", "item3"),
        new GroupViewItem("Videos", 3, true, "Watch your video files", "item4")
    });
    
    // Enable tooltips
    this.groupView1.ShowToolTips = true;
    
    // Add to form
    this.Controls.Add(this.groupView1);
}
```

### Dynamic ToolTip Updates

Update tooltip text at runtime based on application state:

```csharp
public void UpdateToolTipsDynamically()
{
    // Update tooltip based on item state
    for (int i = 0; i < this.groupView1.GroupViewItems.Count; i++)
    {
        GroupViewItem item = this.groupView1.GroupViewItems[i];
        
        // Example: Show different tooltip based on item state
        if (item.Text.Contains("Document"))
        {
            int docCount = GetDocumentCount(); // Your method
            item.ToolTipText = $"Documents folder ({docCount} files)";
        }
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

**Method 1: Using ContextMenuStrip (Recommended)**

```csharp
public void SetupContextMenu()
{
    // Subscribe to ShowContextMenu event
    this.groupView1.ShowContextMenu += GroupView1_ShowContextMenu;
}

private void GroupView1_ShowContextMenu(object sender, EventArgs e)
{
    // Create context menu
    ContextMenuStrip contextMenu = new ContextMenuStrip();
    
    // Add general menu items
    contextMenu.Items.Add("Add New Item", null, AddNewItem_Click);
    contextMenu.Items.Add(new ToolStripSeparator());
    
    // Check if mouse is over an item
    if (this.groupView1.ContextMenuItem != -1)
    {
        // Add item-specific menu options
        contextMenu.Items.Add("Rename", null, RenameItem_Click);
        contextMenu.Items.Add("Delete", null, DeleteItem_Click);
        contextMenu.Items.Add(new ToolStripSeparator());
        contextMenu.Items.Add("Properties", null, ShowProperties_Click);
    }
    else
    {
        // Mouse is over empty space
        contextMenu.Items.Add("Refresh", null, RefreshView_Click);
    }
    
    // Display context menu at cursor position
    contextMenu.Show(this.groupView1, this.groupView1.PointToClient(Cursor.Position));
}

private void AddNewItem_Click(object sender, EventArgs e)
{
    GroupViewItem newItem = new GroupViewItem(
        "New Item",
        -1,
        true,
        "New item tooltip",
        $"item{this.groupView1.GroupViewItems.Count}"
    );
    this.groupView1.GroupViewItems.Add(newItem);
}

private void RenameItem_Click(object sender, EventArgs e)
{
    int itemIndex = this.groupView1.ContextMenuItem;
    if (itemIndex != -1)
    {
        this.groupView1.InplaceRenameItem(itemIndex);
    }
}

private void DeleteItem_Click(object sender, EventArgs e)
{
    int itemIndex = this.groupView1.ContextMenuItem;
    if (itemIndex != -1)
    {
        DialogResult result = MessageBox.Show(
            $"Delete '{this.groupView1.GroupViewItems[itemIndex].Text}'?",
            "Confirm Delete",
            MessageBoxButtons.YesNo,
            MessageBoxIcon.Question
        );
        
        if (result == DialogResult.Yes)
        {
            this.groupView1.GroupViewItems.RemoveAt(itemIndex);
        }
    }
}

private void ShowProperties_Click(object sender, EventArgs e)
{
    int itemIndex = this.groupView1.ContextMenuItem;
    if (itemIndex != -1)
    {
        GroupViewItem item = this.groupView1.GroupViewItems[itemIndex];
        MessageBox.Show(
            $"Item: {item.Text}\nIndex: {itemIndex}\nName: {item.Name}",
            "Properties",
            MessageBoxButtons.OK,
            MessageBoxIcon.Information
        );
    }
}

private void RefreshView_Click(object sender, EventArgs e)
{
    // Refresh logic here
    this.groupView1.Refresh();
}
```

**Method 2: Using ContextMenu (Legacy)**

For .NET Framework applications preferring the older ContextMenu class:

```csharp
private void GroupView1_ShowContextMenu(object sender, EventArgs e)
{
    ContextMenu contextMenu = new ContextMenu();
    
    // Add menu items
    contextMenu.MenuItems.Add("Add New Item", AddNewItem_Click);
    contextMenu.MenuItems.Add("-"); // Separator
    
    if (this.groupView1.ContextMenuItem != -1)
    {
        contextMenu.MenuItems.Add("Rename", RenameItem_Click);
        contextMenu.MenuItems.Add("Delete", DeleteItem_Click);
    }
    
    // Show menu
    contextMenu.Show(this.groupView1, this.groupView1.PointToClient(Cursor.Position));
}
```

### Advanced Context Menu Example

Create a feature-rich context menu with conditional items:

```csharp
public class AdvancedContextMenuExample : Form
{
    private GroupView groupView1;
    
    public AdvancedContextMenuExample()
    {
        InitializeComponent();
        SetupAdvancedContextMenu();
    }
    
    private void SetupAdvancedContextMenu()
    {
        this.groupView1.ShowContextMenu += ShowAdvancedContextMenu;
    }
    
    private void ShowAdvancedContextMenu(object sender, EventArgs e)
    {
        ContextMenuStrip menu = new ContextMenuStrip();
        int itemIndex = this.groupView1.ContextMenuItem;
        
        if (itemIndex != -1)
        {
            // Item-specific menu
            GroupViewItem item = this.groupView1.GroupViewItems[itemIndex];
            
            // Open
            ToolStripMenuItem openItem = new ToolStripMenuItem("Open");
            openItem.Font = new Font(openItem.Font, FontStyle.Bold);
            openItem.Click += (s, ev) => OpenItem(itemIndex);
            menu.Items.Add(openItem);
            
            menu.Items.Add(new ToolStripSeparator());
            
            // Cut, Copy, Paste
            menu.Items.Add("Cut", null, (s, ev) => CutItem(itemIndex));
            menu.Items.Add("Copy", null, (s, ev) => CopyItem(itemIndex));
            
            // Paste (enabled only if clipboard has data)
            ToolStripMenuItem pasteItem = new ToolStripMenuItem("Paste");
            pasteItem.Enabled = Clipboard.ContainsText();
            pasteItem.Click += (s, ev) => PasteItem(itemIndex);
            menu.Items.Add(pasteItem);
            
            menu.Items.Add(new ToolStripSeparator());
            
            // Rename and Delete
            menu.Items.Add("Rename", null, (s, ev) => this.groupView1.InplaceRenameItem(itemIndex));
            menu.Items.Add("Delete", null, (s, ev) => DeleteItem(itemIndex));
            
            menu.Items.Add(new ToolStripSeparator());
            
            // Properties
            menu.Items.Add("Properties", null, (s, ev) => ShowProperties(itemIndex));
        }
        else
        {
            // Empty space menu
            menu.Items.Add("New Item", null, (s, ev) => AddNewItem());
            menu.Items.Add(new ToolStripSeparator());
            
            // Sort submenu
            ToolStripMenuItem sortMenu = new ToolStripMenuItem("Sort by");
            sortMenu.DropDownItems.Add("Name", null, (s, ev) => SortByName());
            sortMenu.DropDownItems.Add("Date Created", null, (s, ev) => SortByDate());
            menu.Items.Add(sortMenu);
            
            menu.Items.Add(new ToolStripSeparator());
            
            // View options
            ToolStripMenuItem viewMenu = new ToolStripMenuItem("View");
            
            ToolStripMenuItem smallIconsItem = new ToolStripMenuItem("Small Icons");
            smallIconsItem.Checked = this.groupView1.SmallImageView;
            smallIconsItem.Click += (s, ev) => { this.groupView1.SmallImageView = true; };
            viewMenu.DropDownItems.Add(smallIconsItem);
            
            ToolStripMenuItem largeIconsItem = new ToolStripMenuItem("Large Icons");
            largeIconsItem.Checked = !this.groupView1.SmallImageView;
            largeIconsItem.Click += (s, ev) => { this.groupView1.SmallImageView = false; };
            viewMenu.DropDownItems.Add(largeIconsItem);
            
            menu.Items.Add(viewMenu);
            
            menu.Items.Add(new ToolStripSeparator());
            menu.Items.Add("Refresh", null, (s, ev) => this.groupView1.Refresh());
        }
        
        // Show menu
        menu.Show(this.groupView1, this.groupView1.PointToClient(Cursor.Position));
    }
    
    private void OpenItem(int index) { /* Implementation */ }
    private void CutItem(int index) { /* Implementation */ }
    private void CopyItem(int index) { /* Implementation */ }
    private void PasteItem(int index) { /* Implementation */ }
    private void DeleteItem(int index) { /* Implementation */ }
    private void ShowProperties(int index) { /* Implementation */ }
    private void AddNewItem() { /* Implementation */ }
    private void SortByName() { /* Implementation */ }
    private void SortByDate() { /* Implementation */ }
}
```

## Complete Interactive Features Example

Combine all interactive features in a single implementation:

```csharp
public partial class InteractiveGroupView : Form
{
    private GroupView groupView1;
    private ImageList imageList1;
    
    public InteractiveGroupView()
    {
        InitializeComponent();
        SetupInteractiveGroupView();
    }
    
    private void SetupInteractiveGroupView()
    {
        // Create and configure GroupView
        this.groupView1 = new GroupView();
        this.groupView1.Location = new Point(20, 20);
        this.groupView1.Size = new Size(300, 400);
        this.groupView1.FlatLook = true;
        this.groupView1.BorderStyle = BorderStyle.FixedSingle;
        
        // SELECTION FEEDBACK
        this.groupView1.ButtonView = true;
        this.groupView1.ClipSelectionBounds = true;
        
        // TOOLTIPS
        this.groupView1.ShowToolTips = true;
        
        // Setup ImageList (assuming images are available)
        this.imageList1 = new ImageList();
        // Add images...
        this.groupView1.SmallImageList = this.imageList1;
        this.groupView1.SmallImageView = true;
        
        // Add items with tooltips
        this.groupView1.GroupViewItems.AddRange(new GroupViewItem[] {
            new GroupViewItem("My Computer", 0, true, "Browse computer resources", "item1"),
            new GroupViewItem("Network", 1, true, "Access network locations", "item2"),
            new GroupViewItem("Recycle Bin", 2, true, "View deleted items", "item3"),
            new GroupViewItem("Control Panel", 3, true, "System settings", "item4")
        });
        
        // Set initial selection
        this.groupView1.SelectedItem = 0;
        
        // CONTEXT MENU
        this.groupView1.ShowContextMenu += GroupView1_ShowContextMenu;
        
        // SELECTION EVENT
        this.groupView1.GroupViewItemSelected += (sender, e) =>
        {
            int index = this.groupView1.SelectedItem;
            if (index != -1)
            {
                this.Text = $"Selected: {this.groupView1.GroupViewItems[index].Text}";
            }
        };
        
        // Add to form
        this.Controls.Add(this.groupView1);
    }
    
    private void GroupView1_ShowContextMenu(object sender, EventArgs e)
    {
        ContextMenuStrip menu = new ContextMenuStrip();
        
        if (this.groupView1.ContextMenuItem != -1)
        {
            menu.Items.Add("Open", null, (s, ev) => OpenItem());
            menu.Items.Add(new ToolStripSeparator());
            menu.Items.Add("Rename", null, (s, ev) => RenameItem());
            menu.Items.Add("Delete", null, (s, ev) => DeleteItem());
        }
        else
        {
            menu.Items.Add("Add Item", null, (s, ev) => AddItem());
            menu.Items.Add("Refresh", null, (s, ev) => this.groupView1.Refresh());
        }
        
        menu.Show(this.groupView1, this.groupView1.PointToClient(Cursor.Position));
    }
    
    private void OpenItem()
    {
        int index = this.groupView1.ContextMenuItem;
        MessageBox.Show($"Opening: {this.groupView1.GroupViewItems[index].Text}");
    }
    
    private void RenameItem()
    {
        this.groupView1.InplaceRenameItem(this.groupView1.ContextMenuItem);
    }
    
    private void DeleteItem()
    {
        int index = this.groupView1.ContextMenuItem;
        if (MessageBox.Show("Delete this item?", "Confirm", MessageBoxButtons.YesNo) == DialogResult.Yes)
        {
            this.groupView1.GroupViewItems.RemoveAt(index);
        }
    }
    
    private void AddItem()
    {
        this.groupView1.GroupViewItems.Add(
            new GroupViewItem($"New Item {this.groupView1.GroupViewItems.Count + 1}", -1, true, null, "newitem")
        );
    }
}
```

## Best Practices

### Selection Feedback
- **Use ButtonView** for touch-friendly interfaces or toolbar-style lists
- **Use ClipSelectionBounds** for high-density lists needing extra selection emphasis
- **Combine both features** for maximum visibility in accessibility scenarios
- **Set initial selection** (SelectedItem) when appropriate for user context

### ToolTips
- **Keep tooltip text concise** (under 60 characters preferred)
- **Provide helpful context**, not just repetition of item text
- **Use dynamic tooltips** to display current state or counts
- **Enable tooltips globally** via ShowToolTips property

### Context Menus
- **Check ContextMenuItem** to determine if mouse is over an item
- **Provide different menus** for items vs. empty space
- **Disable inapplicable menu items** rather than hiding them
- **Make default actions bold** and place them at the top
- **Group related items** with separators for clarity
