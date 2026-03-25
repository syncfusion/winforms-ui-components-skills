# GroupView and Child Items

This guide covers the GroupView control, which serves as the client control for GroupBarItems when you need hierarchical navigation. GroupView displays child items in a list format, similar to the Visual Studio toolbox or Outlook folder list.

## Table of Contents

- [GroupView Overview](#groupview-overview)
- [Creating GroupView Instances](#creating-groupview-instances)
- [GroupViewItem for Child Items](#groupviewitem-for-child-items)
- [GroupViewItem Constructor Parameters](#groupviewitem-constructor-parameters)
- [Adding Items to GroupViewItems Collection](#adding-items-to-groupviewitems-collection)
- [Linking GroupView to GroupBarItem](#linking-groupview-to-groupbaritem)
- [Item Selection in GroupView](#item-selection-in-groupview)
- [GroupViewItemSelected Event Handling](#groupviewitemselected-event-handling)
- [Complete Toolbox-Style Example](#complete-toolbox-style-example)
- [Complete Outlook-Style Example](#complete-outlook-style-example)

## GroupView Overview

**GroupView** is a specialized control designed to work as the client control for GroupBarItems. It provides:

- List-based display of child items
- Single or multiple item selection
- Icon support for items
- Integration with GroupBar navigation
- Event handling for item selection

**When to use GroupView:**
- Building Outlook-style folder hierarchies
- Creating Visual Studio toolbox clones
- Displaying categorized lists (mail folders, document categories, tool palettes)
- Need selectable child items under navigation tabs

**When to use alternatives:**
- Simple content display → Use Panel
- Tree-structured data → Use TreeView
- Grid-based data → Use DataGridView
- Custom layouts → Use custom UserControl

### GroupView vs. Other Controls

| Control | Best For | Selection | Hierarchy |
|---------|----------|-----------|-----------|
| **GroupView** | Flat item lists, Outlook folders | Single/Multi | Single level |
| **TreeView** | Tree structures, nested folders | Single | Multi-level |
| **ListBox** | Simple lists | Single/Multi | None |
| **Panel** | Custom layouts | N/A | N/A |

## Creating GroupView Instances

### Method 1: Programmatic Creation

```csharp
using Syncfusion.Windows.Forms.Tools;

// Create GroupView
GroupView mailFolders = new GroupView();
mailFolders.Name = "MailFolders";

// Configure appearance (optional)
mailFolders.BackColor = System.Drawing.Color.White;
mailFolders.ForeColor = System.Drawing.Color.Black;
```

### Method 2: Designer Creation

1. Drag GroupView control from toolbox onto form
2. Set its properties in Properties window
3. Add GroupViewItems using collection editor

**Note:** When using the designer, GroupView is typically added to the form first, then assigned as a client to a GroupBarItem.

### Method 3: Inline Initialization

```csharp
GroupView folders = new GroupView
{
    Name = "DocumentFolders",
    BackColor = System.Drawing.Color.WhiteSmoke,
    IntegralHeight = true
};
```

## GroupViewItem for Child Items

**GroupViewItem** represents individual items within a GroupView. Each item:

- Displays text and optional icon
- Can be visible or hidden
- Supports selection states
- Can store custom data via Tag property
- Has a unique key for identification

### Basic GroupViewItem Creation

```csharp
// Simple text-only item
GroupViewItem item1 = new GroupViewItem("Inbox");

// Item with all parameters
GroupViewItem item2 = new GroupViewItem(
    "Drafts",        // text
    0,               // imageIndex
    true,            // visible
    null,            // tag
    "DraftsKey"      // key
);
```

## GroupViewItem Constructor Parameters

The GroupViewItem constructor accepts five parameters:

```csharp
public GroupViewItem(
    string text,      // Display text
    int imageIndex,   // Icon index (-1 for no icon)
    bool visible,     // Visibility
    object tag,       // Custom data
    string key        // Unique identifier
)
```

### Parameter Details

#### 1. Text (string)

The display text for the item.

```csharp
// Basic text
GroupViewItem inbox = new GroupViewItem("Inbox", -1, true, null, "Inbox");

// Text with counts
GroupViewItem inboxWithCount = new GroupViewItem("Inbox (15)", -1, true, null, "Inbox");

// Dynamic text updates
private void UpdateFolderCount(GroupViewItem item, int count)
{
    string baseName = item.Key; // e.g., "Inbox"
    item.Text = count > 0 ? $"{baseName} ({count})" : baseName;
}
```

**When to include counts in text:**
- Unread message counts (mail folders)
- Pending items (task lists)
- Notification badges (alerts)

#### 2. ImageIndex (int)

Index of the icon in an associated ImageList (-1 means no icon).

```csharp
// Set up ImageList first
ImageList folderIcons = new ImageList();
folderIcons.ImageSize = new Size(16, 16);
folderIcons.Images.Add("inbox", Properties.Resources.InboxIcon);
folderIcons.Images.Add("drafts", Properties.Resources.DraftsIcon);
folderIcons.Images.Add("sent", Properties.Resources.SentIcon);

// Assign ImageList to GroupView
mailFolders.ImageList = folderIcons;

// Create items with image indices
GroupViewItem inbox = new GroupViewItem("Inbox", 0, true, null, "Inbox");
GroupViewItem drafts = new GroupViewItem("Drafts", 1, true, null, "Drafts");
GroupViewItem sent = new GroupViewItem("Sent Items", 2, true, null, "Sent");

// No icon
GroupViewItem separator = new GroupViewItem("---", -1, true, null, "Separator");
```

**Best practices for icons:**
- Use consistent icon size (16x16 for standard, 24x24 for touch)
- Provide visual distinction between item types
- Use recognizable metaphors (envelope for mail, calendar for dates)

#### 3. Visible (bool)

Controls whether the item is displayed.

```csharp
// Always visible
GroupViewItem visible = new GroupViewItem("Always Visible", -1, true, null, "Visible");

// Hidden by default
GroupViewItem hidden = new GroupViewItem("Hidden Folder", -1, false, null, "Hidden");

// Toggle visibility at runtime
private void ToggleItemVisibility(GroupViewItem item)
{
    item.Visible = !item.Visible;
}

// Show items based on condition
private void ShowAdvancedFolders(bool show)
{
    foreach (GroupViewItem item in mailFolders.GroupViewItems)
    {
        if (item.Tag is string tag && tag == "Advanced")
        {
            item.Visible = show;
        }
    }
}
```

**When to use hidden items:**
- User permission-based visibility
- "Show advanced options" features
- Temporarily disabled features
- Filtering/search results

#### 4. Tag (object)

Stores custom data associated with the item.

```csharp
// Store folder ID
GroupViewItem inbox = new GroupViewItem("Inbox", 0, true, 12345, "Inbox");

// Store custom object
public class FolderInfo
{
    public int FolderId { get; set; }
    public string Path { get; set; }
    public DateTime LastAccessed { get; set; }
}

FolderInfo inboxInfo = new FolderInfo
{
    FolderId = 12345,
    Path = "/Mail/Inbox",
    LastAccessed = DateTime.Now
};

GroupViewItem inboxWithInfo = new GroupViewItem("Inbox", 0, true, inboxInfo, "Inbox");

// Retrieve tag data
private void OnItemSelected(GroupViewItem item)
{
    if (item.Tag is FolderInfo folderInfo)
    {
        LoadFolderContents(folderInfo.FolderId);
        Console.WriteLine($"Folder path: {folderInfo.Path}");
    }
    else if (item.Tag is int folderId)
    {
        LoadFolderContents(folderId);
    }
}
```

**Common uses for Tag:**
- Database IDs
- File paths
- Configuration objects
- State information

#### 5. Key (string)

Unique identifier for the item.

```csharp
// Use descriptive keys
GroupViewItem inbox = new GroupViewItem("Inbox", 0, true, null, "Inbox");
GroupViewItem sent = new GroupViewItem("Sent Items", 0, true, null, "SentItems");

// Find item by key
private GroupViewItem FindItemByKey(GroupView groupView, string key)
{
    foreach (GroupViewItem item in groupView.GroupViewItems)
    {
        if (item.Key == key)
            return item;
    }
    return null;
}

// Usage
GroupViewItem inboxItem = FindItemByKey(mailFolders, "Inbox");
if (inboxItem != null)
{
    inboxItem.Text = "Inbox (New Messages)";
}
```

**Key naming conventions:**
- Use CamelCase or PascalCase
- Make keys descriptive
- Avoid spaces and special characters
- Keep keys consistent across application

## Adding Items to GroupViewItems Collection

### Adding Individual Items

```csharp
GroupView mailFolders = new GroupView();

// Create items
GroupViewItem inbox = new GroupViewItem("Inbox", 0, true, null, "Inbox");
GroupViewItem drafts = new GroupViewItem("Drafts", 1, true, null, "Drafts");

// Add one at a time
mailFolders.GroupViewItems.Add(inbox);
mailFolders.GroupViewItems.Add(drafts);
```

### Adding Multiple Items

```csharp
GroupView mailFolders = new GroupView();

// Add array of items
mailFolders.GroupViewItems.AddRange(new GroupViewItem[]
{
    new GroupViewItem("Inbox", 0, true, null, "Inbox"),
    new GroupViewItem("Drafts", 1, true, null, "Drafts"),
    new GroupViewItem("Sent Items", 2, true, null, "Sent"),
    new GroupViewItem("Deleted Items", 3, true, null, "Deleted")
});
```

### Inserting Items at Specific Position

```csharp
// Insert at beginning
mailFolders.GroupViewItems.Insert(0, new GroupViewItem("Priority", 0, true, null, "Priority"));

// Insert at end
mailFolders.GroupViewItems.Insert(
    mailFolders.GroupViewItems.Count,
    new GroupViewItem("Archive", 5, true, null, "Archive")
);

// Insert after specific item
int inboxIndex = FindItemIndex(mailFolders, "Inbox");
if (inboxIndex >= 0)
{
    mailFolders.GroupViewItems.Insert(
        inboxIndex + 1,
        new GroupViewItem("Unread", 0, true, null, "Unread")
    );
}
```

### Removing Items

```csharp
// Remove by reference
GroupViewItem itemToRemove = FindItemByKey(mailFolders, "Deleted");
if (itemToRemove != null)
{
    mailFolders.GroupViewItems.Remove(itemToRemove);
}

// Remove by index
mailFolders.GroupViewItems.RemoveAt(0);

// Remove all items
mailFolders.GroupViewItems.Clear();
```

### Dynamic Item Management

```csharp
// Load folders from database
private void LoadMailFolders(GroupView groupView)
{
    groupView.GroupViewItems.Clear();
    
    // Fetch folder list from database
    List<MailFolder> folders = GetMailFoldersFromDatabase();
    
    foreach (var folder in folders)
    {
        GroupViewItem item = new GroupViewItem(
            $"{folder.Name} ({folder.UnreadCount})",
            GetIconIndex(folder.Type),
            true,
            folder.Id,
            folder.Name
        );
        
        groupView.GroupViewItems.Add(item);
    }
}

private int GetIconIndex(string folderType)
{
    return folderType switch
    {
        "Inbox" => 0,
        "Drafts" => 1,
        "Sent" => 2,
        "Deleted" => 3,
        _ => -1
    };
}
```

## Linking GroupView to GroupBarItem

To display a GroupView when a GroupBarItem is selected, follow the **two-step process**:

### Step 1: Assign GroupView as Client

```csharp
GroupBarItem mailItem = new GroupBarItem();
mailItem.Text = "Mail";

GroupView mailFolders = new GroupView();
// ... add items to mailFolders ...

// Step 1: Set GroupView as the client
mailItem.Client = mailFolders;
```

### Step 2: Add GroupView to GroupBar Controls

```csharp
// Step 2: Add to GroupBar's Controls collection
groupBar1.Controls.Add(mailFolders);
```

### Complete Integration Example

```csharp
private void SetupMailNavigation()
{
    // Create GroupBarItem
    GroupBarItem mailItem = new GroupBarItem
    {
        Text = "Mail"
    };
    
    // Create GroupView
    GroupView mailFolders = new GroupView
    {
        Name = "MailFolders"
    };
    
    // Add folders
    mailFolders.GroupViewItems.AddRange(new GroupViewItem[]
    {
        new GroupViewItem("Inbox (15)", 0, true, null, "Inbox"),
        new GroupViewItem("Drafts (3)", 1, true, null, "Drafts"),
        new GroupViewItem("Sent Items", 2, true, null, "Sent"),
        new GroupViewItem("Deleted Items", 3, true, null, "Deleted"),
        new GroupViewItem("Junk Email", 4, true, null, "Junk")
    });
    
    // CRITICAL: Both steps required
    mailItem.Client = mailFolders;                    // Step 1
    this.groupBar1.Controls.Add(mailFolders);        // Step 2
    
    // Add item to GroupBar
    this.groupBar1.GroupBarItems.Add(mailItem);
}
```

**Common mistake:** Forgetting Step 2. The GroupView won't display without being added to the Controls collection.

## Item Selection in GroupView

### Getting Selected Item

```csharp
// Get selected item index
int selectedIndex = mailFolders.SelectedItem;

// Get selected GroupViewItem
if (selectedIndex >= 0 && selectedIndex < mailFolders.GroupViewItems.Count)
{
    GroupViewItem selectedItem = mailFolders.GroupViewItems[selectedIndex];
    Console.WriteLine($"Selected: {selectedItem.Text}");
}
```

### Setting Selected Item

```csharp
// Select by index
mailFolders.SelectedItem = 0; // Select first item (Inbox)

// Select by finding item
GroupViewItem inboxItem = FindItemByKey(mailFolders, "Inbox");
if (inboxItem != null)
{
    int index = mailFolders.GroupViewItems.IndexOf(inboxItem);
    if (index >= 0)
    {
        mailFolders.SelectedItem = index;
    }
}

// Programmatic selection helper
private void SelectItemByKey(GroupView groupView, string key)
{
    for (int i = 0; i < groupView.GroupViewItems.Count; i++)
    {
        if (groupView.GroupViewItems[i].Key == key)
        {
            groupView.SelectedItem = i;
            break;
        }
    }
}
```

### Multi-Select Support

GroupView supports selecting multiple items:

```csharp
// Enable multi-selection (if supported by version)
mailFolders.MultiSelect = true;

// Get all selected items
private List<GroupViewItem> GetSelectedItems(GroupView groupView)
{
    List<GroupViewItem> selected = new List<GroupViewItem>();
    
    // Implementation depends on control version
    // Check documentation for your specific version
    
    return selected;
}
```

## GroupViewItemSelected Event Handling

The **GroupViewItemSelected** event fires when a user clicks an item in the GroupView.

### Basic Event Handling

```csharp
// Wire up event
mailFolders.GroupViewItemSelected += MailFolders_GroupViewItemSelected;

// Event handler
private void MailFolders_GroupViewItemSelected(object sender, EventArgs e)
{
    GroupView view = sender as GroupView;
    if (view != null)
    {
        int selectedIndex = view.SelectedItem;
        if (selectedIndex >= 0 && selectedIndex < view.GroupViewItems.Count)
        {
            GroupViewItem item = view.GroupViewItems[selectedIndex];
            Console.WriteLine($"Folder selected: {item.Text}");
            
            // Load folder contents
            LoadFolderContents(item.Key);
        }
    }
}
```

### Advanced Event Handling with Tag Data

```csharp
private void MailFolders_GroupViewItemSelected(object sender, EventArgs e)
{
    GroupView view = sender as GroupView;
    if (view == null) return;
    
    int selectedIndex = view.SelectedItem;
    if (selectedIndex < 0 || selectedIndex >= view.GroupViewItems.Count)
        return;
    
    GroupViewItem item = view.GroupViewItems[selectedIndex];
    
    // Use Tag to access folder information
    if (item.Tag is int folderId)
    {
        LoadMailMessages(folderId);
    }
    else if (item.Tag is FolderInfo folderInfo)
    {
        LoadMailMessages(folderInfo.FolderId);
        UpdateRecentFolders(folderInfo);
    }
    
    // Update UI
    UpdateStatusBar($"Viewing: {item.Text}");
    UpdateToolbar(item.Key);
}

private void LoadMailMessages(int folderId)
{
    // Query database for messages in this folder
    // Display in main content area
    Console.WriteLine($"Loading messages for folder ID: {folderId}");
}
```

### Multiple GroupViews Event Handling

When using multiple GroupViews, identify which one fired the event:

```csharp
private void SetupEventHandlers()
{
    // Mail folders
    mailFolders.GroupViewItemSelected += OnFolderSelected;
    mailFolders.Tag = "Mail";
    
    // Calendar views
    calendarViews.GroupViewItemSelected += OnFolderSelected;
    calendarViews.Tag = "Calendar";
    
    // Contacts categories
    contactsCategories.GroupViewItemSelected += OnFolderSelected;
    contactsCategories.Tag = "Contacts";
}

private void OnFolderSelected(object sender, EventArgs e)
{
    GroupView view = sender as GroupView;
    if (view == null) return;
    
    string section = view.Tag as string ?? "Unknown";
    int selectedIndex = view.SelectedItem;
    
    if (selectedIndex >= 0 && selectedIndex < view.GroupViewItems.Count)
    {
        GroupViewItem item = view.GroupViewItems[selectedIndex];
        Console.WriteLine($"{section} - {item.Text} selected");
        
        switch (section)
        {
            case "Mail":
                LoadMailFolder(item);
                break;
            case "Calendar":
                LoadCalendarView(item);
                break;
            case "Contacts":
                LoadContactsCategory(item);
                break;
        }
    }
}
```

## Complete Toolbox-Style Example

A Visual Studio toolbox clone with categorized controls:

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class ToolboxForm : Form
{
    private GroupBar toolbox;
    private Panel contentPanel;
    private Label infoLabel;

    public ToolboxForm()
    {
        this.Text = "Control Toolbox";
        this.Size = new Size(800, 600);
        
        CreateToolbox();
        CreateContentArea();
    }

    private void CreateToolbox()
    {
        // Create GroupBar for toolbox
        this.toolbox = new GroupBar
        {
            Dock = DockStyle.Left,
            Width = 220,
            BorderStyle = BorderStyle.FixedSingle,
            Font = new Font("Segoe UI", 9F)
        };

        // Create control categories
        CreateCommonControlsCategory();
        CreateContainersCategory();
        CreateDataCategory();
        CreateDialogsCategory();

        // Set initial selection
        this.toolbox.SelectedItem = 0;

        // Add to form
        this.Controls.Add(this.toolbox);
    }

    private void CreateCommonControlsCategory()
    {
        GroupBarItem item = new GroupBarItem
        {
            Text = "Common Controls"
        };

        GroupView controlsList = new GroupView
        {
            Name = "CommonControls"
        };

        controlsList.GroupViewItems.AddRange(new GroupViewItem[]
        {
            new GroupViewItem("Pointer", -1, true, typeof(Control), "Pointer"),
            new GroupViewItem("Button", -1, true, typeof(Button), "Button"),
            new GroupViewItem("CheckBox", -1, true, typeof(CheckBox), "CheckBox"),
            new GroupViewItem("RadioButton", -1, true, typeof(RadioButton), "RadioButton"),
            new GroupViewItem("Label", -1, true, typeof(Label), "Label"),
            new GroupViewItem("TextBox", -1, true, typeof(TextBox), "TextBox"),
            new GroupViewItem("ListBox", -1, true, typeof(ListBox), "ListBox"),
            new GroupViewItem("ComboBox", -1, true, typeof(ComboBox), "ComboBox"),
            new GroupViewItem("DateTimePicker", -1, true, typeof(DateTimePicker), "DateTimePicker")
        });

        controlsList.GroupViewItemSelected += ControlsList_ItemSelected;

        item.Client = controlsList;
        this.toolbox.Controls.Add(controlsList);
        this.toolbox.GroupBarItems.Add(item);
    }

    private void CreateContainersCategory()
    {
        GroupBarItem item = new GroupBarItem
        {
            Text = "Containers"
        };

        GroupView controlsList = new GroupView
        {
            Name = "Containers"
        };

        controlsList.GroupViewItems.AddRange(new GroupViewItem[]
        {
            new GroupViewItem("Panel", -1, true, typeof(Panel), "Panel"),
            new GroupViewItem("GroupBox", -1, true, typeof(GroupBox), "GroupBox"),
            new GroupViewItem("TabControl", -1, true, typeof(TabControl), "TabControl"),
            new GroupViewItem("FlowLayoutPanel", -1, true, typeof(FlowLayoutPanel), "FlowLayoutPanel"),
            new GroupViewItem("TableLayoutPanel", -1, true, typeof(TableLayoutPanel), "TableLayoutPanel"),
            new GroupViewItem("SplitContainer", -1, true, typeof(SplitContainer), "SplitContainer")
        });

        controlsList.GroupViewItemSelected += ControlsList_ItemSelected;

        item.Client = controlsList;
        this.toolbox.Controls.Add(controlsList);
        this.toolbox.GroupBarItems.Add(item);
    }

    private void CreateDataCategory()
    {
        GroupBarItem item = new GroupBarItem
        {
            Text = "Data"
        };

        GroupView controlsList = new GroupView
        {
            Name = "Data"
        };

        controlsList.GroupViewItems.AddRange(new GroupViewItem[]
        {
            new GroupViewItem("DataGridView", -1, true, typeof(DataGridView), "DataGridView"),
            new GroupViewItem("BindingSource", -1, true, typeof(BindingSource), "BindingSource"),
            new GroupViewItem("BindingNavigator", -1, true, typeof(BindingNavigator), "BindingNavigator"),
            new GroupViewItem("ListView", -1, true, typeof(ListView), "ListView"),
            new GroupViewItem("TreeView", -1, true, typeof(TreeView), "TreeView")
        });

        controlsList.GroupViewItemSelected += ControlsList_ItemSelected;

        item.Client = controlsList;
        this.toolbox.Controls.Add(controlsList);
        this.toolbox.GroupBarItems.Add(item);
    }

    private void CreateDialogsCategory()
    {
        GroupBarItem item = new GroupBarItem
        {
            Text = "Dialogs"
        };

        GroupView controlsList = new GroupView
        {
            Name = "Dialogs"
        };

        controlsList.GroupViewItems.AddRange(new GroupViewItem[]
        {
            new GroupViewItem("OpenFileDialog", -1, true, typeof(OpenFileDialog), "OpenFileDialog"),
            new GroupViewItem("SaveFileDialog", -1, true, typeof(SaveFileDialog), "SaveFileDialog"),
            new GroupViewItem("FolderBrowserDialog", -1, true, typeof(FolderBrowserDialog), "FolderBrowserDialog"),
            new GroupViewItem("ColorDialog", -1, true, typeof(ColorDialog), "ColorDialog"),
            new GroupViewItem("FontDialog", -1, true, typeof(FontDialog), "FontDialog")
        });

        controlsList.GroupViewItemSelected += ControlsList_ItemSelected;

        item.Client = controlsList;
        this.toolbox.Controls.Add(controlsList);
        this.toolbox.GroupBarItems.Add(item);
    }

    private void CreateContentArea()
    {
        this.contentPanel = new Panel
        {
            Dock = DockStyle.Fill,
            BackColor = Color.White,
            Padding = new Padding(20)
        };

        this.infoLabel = new Label
        {
            Dock = DockStyle.Top,
            Height = 100,
            Font = new Font("Segoe UI", 12F),
            Text = "Select a control from the toolbox"
        };

        this.contentPanel.Controls.Add(this.infoLabel);
        this.Controls.Add(this.contentPanel);
    }

    private void ControlsList_ItemSelected(object sender, EventArgs e)
    {
        GroupView view = sender as GroupView;
        if (view == null) return;

        int selectedIndex = view.SelectedItem;
        if (selectedIndex < 0 || selectedIndex >= view.GroupViewItems.Count)
            return;

        GroupViewItem item = view.GroupViewItems[selectedIndex];
        
        // Get control type from Tag
        if (item.Tag is Type controlType)
        {
            DisplayControlInfo(item.Text, controlType);
        }
    }

    private void DisplayControlInfo(string controlName, Type controlType)
    {
        string info = $"Control: {controlName}\n\n";
        info += $"Type: {controlType.FullName}\n\n";
        info += $"Namespace: {controlType.Namespace}\n\n";
        info += $"Assembly: {controlType.Assembly.GetName().Name}\n\n";
        info += "Click to add this control to your form.";

        this.infoLabel.Text = info;
    }
}
```

**Result:** A Visual Studio-style toolbox with multiple categories (Common Controls, Containers, Data, Dialogs), each displaying relevant control types.

## Complete Outlook-Style Example

A full Outlook clone with mail folders and rich interaction:

```csharp
using System;
using System.Collections.Generic;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class OutlookCloneForm : Form
{
    private GroupBar navigationPane;
    private Panel contentArea;
    private Label titleLabel;
    private ListBox messageList;

    // Sample data
    private Dictionary<string, List<string>> folderContents;

    public OutlookCloneForm()
    {
        this.Text = "Outlook Clone";
        this.Size = new Size(1000, 700);
        
        InitializeSampleData();
        CreateNavigationPane();
        CreateContentArea();
    }

    private void InitializeSampleData()
    {
        folderContents = new Dictionary<string, List<string>>
        {
            { "Inbox", new List<string> { "Welcome message", "Team meeting update", "Project status", "Budget review" } },
            { "Drafts", new List<string> { "Draft: Reply to client", "Draft: Weekly report" } },
            { "Sent", new List<string> { "RE: Project timeline", "FW: Budget approval" } },
            { "Deleted", new List<string> { "Old newsletter", "Spam message" } }
        };
    }

    private void CreateNavigationPane()
    {
        this.navigationPane = new GroupBar
        {
            Dock = DockStyle.Left,
            Width = 240,
            BorderStyle = BorderStyle.Fixed3D,
            Font = new Font("Segoe UI", 9F),
            BackColor = Color.FromArgb(245, 246, 247)
        };

        // Create Mail section
        CreateMailSection();

        // Create Calendar section
        CreateCalendarSection();

        // Create Contacts section
        CreateContactsSection();

        // Create Tasks section
        CreateTasksSection();

        // Set initial selection
        this.navigationPane.SelectedItem = 0;

        this.Controls.Add(this.navigationPane);
    }

    private void CreateMailSection()
    {
        GroupBarItem mailItem = new GroupBarItem
        {
            Text = "Mail"
        };

        GroupView mailFolders = new GroupView
        {
            Name = "MailFolders",
            BackColor = Color.White
        };

        mailFolders.GroupViewItems.AddRange(new GroupViewItem[]
        {
            new GroupViewItem("Inbox (15)", -1, true, folderContents["Inbox"], "Inbox"),
            new GroupViewItem("Drafts (3)", -1, true, folderContents["Drafts"], "Drafts"),
            new GroupViewItem("Sent Items", -1, true, folderContents["Sent"], "Sent"),
            new GroupViewItem("Deleted Items (2)", -1, true, folderContents["Deleted"], "Deleted"),
            new GroupViewItem("Junk Email", -1, true, new List<string>(), "Junk"),
            new GroupViewItem("Outbox", -1, true, new List<string>(), "Outbox"),
            new GroupViewItem("Archive", -1, true, new List<string>(), "Archive")
        });

        mailFolders.GroupViewItemSelected += MailFolders_ItemSelected;

        mailItem.Client = mailFolders;
        this.navigationPane.Controls.Add(mailFolders);
        this.navigationPane.GroupBarItems.Add(mailItem);
    }

    private void CreateCalendarSection()
    {
        GroupBarItem calendarItem = new GroupBarItem
        {
            Text = "Calendar"
        };

        GroupView calendarViews = new GroupView
        {
            Name = "CalendarViews",
            BackColor = Color.White
        };

        calendarViews.GroupViewItems.AddRange(new GroupViewItem[]
        {
            new GroupViewItem("My Calendar", -1, true, null, "MyCalendar"),
            new GroupViewItem("Team Calendar", -1, true, null, "TeamCalendar"),
            new GroupViewItem("Birthdays", -1, true, null, "Birthdays"),
            new GroupViewItem("Holidays", -1, true, null, "Holidays"),
            new GroupViewItem("Meetings", -1, true, null, "Meetings")
        });

        calendarViews.GroupViewItemSelected += CalendarViews_ItemSelected;

        calendarItem.Client = calendarViews;
        this.navigationPane.Controls.Add(calendarViews);
        this.navigationPane.GroupBarItems.Add(calendarItem);
    }

    private void CreateContactsSection()
    {
        GroupBarItem contactsItem = new GroupBarItem
        {
            Text = "Contacts"
        };

        GroupView contactsCategories = new GroupView
        {
            Name = "ContactsCategories",
            BackColor = Color.White
        };

        contactsCategories.GroupViewItems.AddRange(new GroupViewItem[]
        {
            new GroupViewItem("All Contacts (156)", -1, true, null, "AllContacts"),
            new GroupViewItem("Colleagues (45)", -1, true, null, "Colleagues"),
            new GroupViewItem("Friends (32)", -1, true, null, "Friends"),
            new GroupViewItem("Family (18)", -1, true, null, "Family"),
            new GroupViewItem("Vendors (12)", -1, true, null, "Vendors")
        });

        contactsCategories.GroupViewItemSelected += ContactsCategories_ItemSelected;

        contactsItem.Client = contactsCategories;
        this.navigationPane.Controls.Add(contactsCategories);
        this.navigationPane.GroupBarItems.Add(contactsItem);
    }

    private void CreateTasksSection()
    {
        GroupBarItem tasksItem = new GroupBarItem
        {
            Text = "Tasks"
        };

        GroupView tasksLists = new GroupView
        {
            Name = "TasksLists",
            BackColor = Color.White
        };

        tasksLists.GroupViewItems.AddRange(new GroupViewItem[]
        {
            new GroupViewItem("To Do (8)", -1, true, null, "Todo"),
            new GroupViewItem("In Progress (4)", -1, true, null, "InProgress"),
            new GroupViewItem("Completed (23)", -1, true, null, "Completed"),
            new GroupViewItem("Waiting (2)", -1, true, null, "Waiting"),
            new GroupViewItem("Deferred", -1, true, null, "Deferred")
        });

        tasksLists.GroupViewItemSelected += TasksLists_ItemSelected;

        tasksItem.Client = tasksLists;
        this.navigationPane.Controls.Add(tasksLists);
        this.navigationPane.GroupBarItems.Add(tasksItem);
    }

    private void CreateContentArea()
    {
        this.contentArea = new Panel
        {
            Dock = DockStyle.Fill,
            BackColor = Color.White,
            Padding = new Padding(0)
        };

        this.titleLabel = new Label
        {
            Dock = DockStyle.Top,
            Height = 50,
            Font = new Font("Segoe UI", 16F, FontStyle.Bold),
            Text = "Inbox",
            Padding = new Padding(10),
            BackColor = Color.FromArgb(230, 235, 240)
        };

        this.messageList = new ListBox
        {
            Dock = DockStyle.Fill,
            Font = new Font("Segoe UI", 10F),
            BorderStyle = BorderStyle.None
        };

        this.contentArea.Controls.Add(this.messageList);
        this.contentArea.Controls.Add(this.titleLabel);
        this.Controls.Add(this.contentArea);
    }

    private void MailFolders_ItemSelected(object sender, EventArgs e)
    {
        GroupView view = sender as GroupView;
        if (view == null) return;

        int selectedIndex = view.SelectedItem;
        if (selectedIndex < 0 || selectedIndex >= view.GroupViewItems.Count)
            return;

        GroupViewItem item = view.GroupViewItems[selectedIndex];
        this.titleLabel.Text = item.Text;

        // Load messages from Tag
        if (item.Tag is List<string> messages)
        {
            this.messageList.Items.Clear();
            foreach (string message in messages)
            {
                this.messageList.Items.Add(message);
            }
        }
        else
        {
            this.messageList.Items.Clear();
            this.messageList.Items.Add($"No messages in {item.Text}");
        }
    }

    private void CalendarViews_ItemSelected(object sender, EventArgs e)
    {
        GroupView view = sender as GroupView;
        if (view == null) return;

        int selectedIndex = view.SelectedItem;
        if (selectedIndex >= 0 && selectedIndex < view.GroupViewItems.Count)
        {
            GroupViewItem item = view.GroupViewItems[selectedIndex];
            this.titleLabel.Text = item.Text;
            this.messageList.Items.Clear();
            this.messageList.Items.Add($"Calendar view: {item.Text}");
        }
    }

    private void ContactsCategories_ItemSelected(object sender, EventArgs e)
    {
        GroupView view = sender as GroupView;
        if (view == null) return;

        int selectedIndex = view.SelectedItem;
        if (selectedIndex >= 0 && selectedIndex < view.GroupViewItems.Count)
        {
            GroupViewItem item = view.GroupViewItems[selectedIndex];
            this.titleLabel.Text = item.Text;
            this.messageList.Items.Clear();
            this.messageList.Items.Add($"Contacts category: {item.Text}");
        }
    }

    private void TasksLists_ItemSelected(object sender, EventArgs e)
    {
        GroupView view = sender as GroupView;
        if (view == null) return;

        int selectedIndex = view.SelectedItem;
        if (selectedIndex >= 0 && selectedIndex < view.GroupViewItems.Count)
        {
            GroupViewItem item = view.GroupViewItems[selectedIndex];
            this.titleLabel.Text = item.Text;
            this.messageList.Items.Clear();
            this.messageList.Items.Add($"Task list: {item.Text}");
        }
    }
}
```

**Result:** A complete Outlook-style application with Mail, Calendar, Contacts, and Tasks sections. Each section contains relevant sub-items in GroupViews. Clicking a mail folder displays its messages in the content area.

## Key Takeaways

1. **GroupView** displays hierarchical child items for GroupBarItems
2. **GroupViewItem** represents individual child items with text, icons, and data
3. **Five constructor parameters**: text, imageIndex, visible, tag, key
4. **Tag property** stores custom data (IDs, objects, metadata)
5. **Key property** provides unique identifiers for finding items
6. **Two-step integration**: Assign as Client AND add to Controls collection
7. **GroupViewItemSelected** event handles child item selection
8. **Perfect for** Outlook folders, Visual Studio toolbox, categorized lists
