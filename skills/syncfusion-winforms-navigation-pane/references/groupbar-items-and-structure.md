# GroupBar Items and Structure

This guide covers the structure and configuration of GroupBarItems, which are the individual navigation tabs in the GroupBar control. Understanding GroupBarItem properties and management is essential for building effective navigation interfaces.

## Table of Contents

- [GroupBarItem Overview](#groupbaritem-overview)
- [Creating GroupBarItem Instances](#creating-groupbaritem-instances)
- [Text Property for Item Labels](#text-property-for-item-labels)
- [Image Property for Item Icons](#image-property-for-item-icons)
- [Client Property - Linking to Controls](#client-property---linking-to-controls)
- [GroupBarItems Collection Management](#groupbaritems-collection-management)
- [SelectedItem Property](#selecteditem-property)
- [Item Selection and Navigation](#item-selection-and-navigation)
- [GroupBarItemSelected Event Handling](#groupbaritemselected-event-handling)
- [Complete Examples](#complete-examples)

## GroupBarItem Overview

A **GroupBarItem** represents an individual navigation tab or button in the GroupBar control. Each item:

- Displays a text label and optional icon
- Can host a client control (the content shown when selected)
- Supports selection states and visual feedback
- Can be customized independently

**Container-Client Model:**
The GroupBar follows a container-client architecture where:
- **GroupBar** = Container (holds navigation items)
- **GroupBarItem** = Navigation tab/button
- **Client Control** = Content displayed when item is selected (typically GroupView or Panel)

```csharp
// Basic relationship structure
GroupBar
  ├── GroupBarItem ("Mail")
  │     └── Client: GroupView (mail folders)
  ├── GroupBarItem ("Calendar")
  │     └── Client: Panel (calendar display)
  └── GroupBarItem ("Contacts")
        └── Client: ListBox (contact list)
```

## Creating GroupBarItem Instances

### Method 1: Via Designer

1. Select GroupBar control
2. Click **GroupBarItems** property in Properties window
3. Click ellipsis (...) to open Collection Editor
4. Click "Add" to create new items
5. Configure properties for each item

### Method 2: Programmatic Creation

```csharp
using Syncfusion.Windows.Forms.Tools;

// Create individual items
GroupBarItem mailItem = new GroupBarItem();
GroupBarItem calendarItem = new GroupBarItem();
GroupBarItem contactsItem = new GroupBarItem();

// Configure basic properties
mailItem.Text = "Mail";
calendarItem.Text = "Calendar";
contactsItem.Text = "Contacts";

// Add to GroupBar
this.groupBar1.GroupBarItems.AddRange(new GroupBarItem[] {
    mailItem,
    calendarItem,
    contactsItem
});
```

**When to use programmatic creation:**
- Loading navigation structure from database or configuration
- Dynamic UI based on user permissions
- Runtime modification of navigation items
- Generating items from data models

### Method 3: Inline Initialization

```csharp
// Create and initialize in one statement
GroupBarItem item = new GroupBarItem
{
    Text = "Documents",
    Client = new GroupView()
};
```

## Text Property for Item Labels

The **Text** property sets the display label for the GroupBarItem.

### Basic Text Assignment

```csharp
this.groupBarItem1.Text = "Mail";
this.groupBarItem2.Text = "Calendar";
this.groupBarItem3.Text = "Contacts";
```

### Dynamic Text with Counts

Show dynamic information in item labels:

```csharp
private void UpdateMailItemText(int unreadCount)
{
    if (unreadCount > 0)
    {
        this.mailItem.Text = $"Mail ({unreadCount})";
    }
    else
    {
        this.mailItem.Text = "Mail";
    }
}

// Usage
UpdateMailItemText(12); // Displays "Mail (12)"
```

**When to use dynamic text:**
- Show unread message counts
- Display pending items or notifications
- Indicate data loading status
- Reflect real-time updates

### Text Alignment

Control text alignment across all items:

```csharp
// Left alignment (better for longer text)
this.groupBar1.TextAlign = Syncfusion.Windows.Forms.Tools.TextAlignment.Left;

// Center alignment (default, balanced look)
this.groupBar1.TextAlign = Syncfusion.Windows.Forms.Tools.TextAlignment.Center;

// Right alignment (uncommon, special layouts)
this.groupBar1.TextAlign = Syncfusion.Windows.Forms.Tools.TextAlignment.Right;
```

**Result:** All GroupBarItem text aligns according to the specified setting.

### In-Place Renaming

Allow users to rename items at runtime:

```csharp
// Enable in-place editing for specific item
private void AllowRenameItem(int itemIndex)
{
    this.groupBar1.InplaceRenameItem(itemIndex);
}

// Cancel in-place editing
private void CancelRename()
{
    this.groupBar1.CancelInplaceRenameItem();
}

// Handle rename event
private void GroupBar1_GroupBarItemRenamed(object sender, 
    Syncfusion.Windows.Forms.Tools.GroupItemRenamedEventArgs e)
{
    Console.WriteLine($"Item at index {e.Index} renamed from '{e.OldLabel}' to '{e.NewLabel}'");
    
    // Validate new name
    if (string.IsNullOrWhiteSpace(e.NewLabel))
    {
        MessageBox.Show("Item name cannot be empty.");
        this.groupBar1.GroupBarItems[e.Index].Text = e.OldLabel;
    }
}

// Wire up event
this.groupBar1.GroupBarItemRenamed += GroupBar1_GroupBarItemRenamed;
```

**When to use in-place renaming:**
- User-customizable navigation
- Document or project organization tools
- Personalized workspaces

## Image Property for Item Icons

Add visual identity to items with icons.

### Basic Image Assignment

```csharp
// From resources
this.groupBarItem1.Image = Properties.Resources.MailIcon;
this.groupBarItem2.Image = Properties.Resources.CalendarIcon;
this.groupBarItem3.Image = Properties.Resources.ContactsIcon;
```

### Using ImageList

Manage multiple icons efficiently:

```csharp
// Set up ImageList
ImageList imageList = new ImageList();
imageList.ImageSize = new Size(16, 16);
imageList.Images.Add("mail", Properties.Resources.MailIcon);
imageList.Images.Add("calendar", Properties.Resources.CalendarIcon);
imageList.Images.Add("contacts", Properties.Resources.ContactsIcon);

// Assign images from ImageList
this.groupBarItem1.Image = imageList.Images["mail"];
this.groupBarItem2.Image = imageList.Images["calendar"];
this.groupBarItem3.Image = imageList.Images["contacts"];
```

### Large Image Mode

Display larger icons on item headers:

```csharp
// Enable large image mode
this.groupBarItem1.LargeImageMode = true;
this.groupBarItem1.Image = Properties.Resources.LargeMailIcon; // 32x32 or 48x48
```

**When to use large images:**
- Stacked mode (Outlook-style) navigation
- Touch-friendly interfaces
- Emphasis on visual recognition over text
- Modern flat design patterns

### Complete Image Example

```csharp
private void SetupItemImages()
{
    // Create and configure ImageList
    ImageList smallIcons = new ImageList
    {
        ImageSize = new Size(16, 16),
        ColorDepth = ColorDepth.Depth32Bit
    };
    
    ImageList largeIcons = new ImageList
    {
        ImageSize = new Size(32, 32),
        ColorDepth = ColorDepth.Depth32Bit
    };
    
    // Load images (from resources, files, or embedded resources)
    smallIcons.Images.Add("mail", LoadIcon("mail_16.png"));
    smallIcons.Images.Add("calendar", LoadIcon("calendar_16.png"));
    
    largeIcons.Images.Add("mail", LoadIcon("mail_32.png"));
    largeIcons.Images.Add("calendar", LoadIcon("calendar_32.png"));
    
    // Assign small images
    this.groupBarItem1.Image = smallIcons.Images["mail"];
    this.groupBarItem2.Image = smallIcons.Images["calendar"];
    
    // For stacked mode, use large images
    this.groupBarItem1.NavigationPaneImage = largeIcons.Images["mail"];
    this.groupBarItem2.NavigationPaneImage = largeIcons.Images["calendar"];
}

private Image LoadIcon(string fileName)
{
    string iconPath = System.IO.Path.Combine(Application.StartupPath, "Icons", fileName);
    return Image.FromFile(iconPath);
}
```

## Client Property - Linking to Controls

The **Client** property links a control to the GroupBarItem. This control is displayed when the item is selected.

### Understanding the Client Property

```csharp
// The Client property accepts any Control
public Control Client { get; set; }
```

**Key Concept:** When a GroupBarItem is selected, its Client control becomes visible in the GroupBar's content area, while other clients are hidden.

### Null Client Handling

```csharp
// Item without client (acts as placeholder or separator)
GroupBarItem placeholderItem = new GroupBarItem
{
    Text = "--- Section ---",
    Client = null
};

// Check for null client before operations
if (selectedItem.Client != null)
{
    // Safe to access client properties
    selectedItem.Client.BackColor = Color.White;
}
```

### Common Client Control Types

#### 1. Panel (Simple Content)

```csharp
Panel contentPanel = new Panel
{
    Dock = DockStyle.Fill,
    BackColor = Color.White,
    Padding = new Padding(10)
};

Label label = new Label
{
    Text = "Welcome to Mail",
    Dock = DockStyle.Top,
    Font = new Font("Segoe UI", 14F, FontStyle.Bold)
};

contentPanel.Controls.Add(label);

this.mailItem.Client = contentPanel;
this.groupBar1.Controls.Add(contentPanel);
```

#### 2. GroupView (Hierarchical Navigation)

```csharp
GroupView mailFolders = new GroupView
{
    Name = "MailFolders"
};

mailFolders.GroupViewItems.AddRange(new GroupViewItem[]
{
    new GroupViewItem("Inbox", -1, true, null, "Inbox"),
    new GroupViewItem("Drafts", -1, true, null, "Drafts"),
    new GroupViewItem("Sent Items", -1, true, null, "Sent")
});

this.mailItem.Client = mailFolders;
this.groupBar1.Controls.Add(mailFolders);
```

#### 3. Custom User Control

```csharp
// Assuming you have a UserControl named MailViewControl
MailViewControl mailView = new MailViewControl
{
    Dock = DockStyle.Fill
};

this.mailItem.Client = mailView;
this.groupBar1.Controls.Add(mailView);
```

#### 4. TreeView (Hierarchical Data)

```csharp
TreeView documentTree = new TreeView
{
    Dock = DockStyle.Fill,
    BorderStyle = BorderStyle.None
};

TreeNode rootNode = new TreeNode("Documents");
rootNode.Nodes.Add("Recent");
rootNode.Nodes.Add("Shared");
rootNode.Nodes.Add("Archived");
documentTree.Nodes.Add(rootNode);
documentTree.ExpandAll();

this.documentsItem.Client = documentTree;
this.groupBar1.Controls.Add(documentTree);
```

### Critical: Two-Step Client Assignment

**IMPORTANT:** Always perform both steps when assigning a client:

```csharp
// STEP 1: Assign the control as the item's client
groupBarItem.Client = myControl;

// STEP 2: Add the control to GroupBar's Controls collection
groupBar1.Controls.Add(myControl);

// Missing Step 2 is the most common mistake!
```

**Why both steps are required:**
- Step 1: Links the control to the item
- Step 2: Adds control to the form's control hierarchy for rendering

## GroupBarItems Collection Management

The **GroupBarItems** collection contains all navigation items.

### Adding Items

```csharp
// Single item
this.groupBar1.GroupBarItems.Add(newItem);

// Multiple items
this.groupBar1.GroupBarItems.AddRange(new GroupBarItem[] {
    item1, item2, item3
});

// Insert at specific position
this.groupBar1.GroupBarItems.Insert(0, firstItem); // Add at beginning
```

### Removing Items

```csharp
// Remove specific item
this.groupBar1.GroupBarItems.Remove(mailItem);

// Remove by index
this.groupBar1.GroupBarItems.RemoveAt(0);

// Remove all items
this.groupBar1.GroupBarItems.Clear();
```

### Accessing Items

```csharp
// By index
GroupBarItem item = this.groupBar1.GroupBarItems[0];

// By iteration
foreach (GroupBarItem item in this.groupBar1.GroupBarItems)
{
    Console.WriteLine(item.Text);
}

// Count items
int itemCount = this.groupBar1.GroupBarItems.Count;

// Find item by text
GroupBarItem foundItem = null;
foreach (GroupBarItem item in this.groupBar1.GroupBarItems)
{
    if (item.Text == "Mail")
    {
        foundItem = item;
        break;
    }
}
```

### Reordering Items

```csharp
// Move item to new position
private void MoveItem(int fromIndex, int toIndex)
{
    if (fromIndex >= 0 && fromIndex < this.groupBar1.GroupBarItems.Count &&
        toIndex >= 0 && toIndex < this.groupBar1.GroupBarItems.Count)
    {
        GroupBarItem item = this.groupBar1.GroupBarItems[fromIndex];
        this.groupBar1.GroupBarItems.RemoveAt(fromIndex);
        this.groupBar1.GroupBarItems.Insert(toIndex, item);
    }
}

// Usage: Move first item to last position
MoveItem(0, this.groupBar1.GroupBarItems.Count - 1);
```

## SelectedItem Property

The **SelectedItem** property gets or sets the index of the currently selected item.

### Getting Selected Item

```csharp
// Get selected index
int selectedIndex = this.groupBar1.SelectedItem;

// Get selected GroupBarItem object
if (selectedIndex >= 0 && selectedIndex < this.groupBar1.GroupBarItems.Count)
{
    GroupBarItem selectedItem = this.groupBar1.GroupBarItems[selectedIndex];
    Console.WriteLine($"Selected: {selectedItem.Text}");
}
```

### Setting Selected Item

```csharp
// Select by index
this.groupBar1.SelectedItem = 0; // Select first item

// Select by finding item
int mailIndex = this.groupBar1.GroupBarItems.IndexOf(this.mailItem);
if (mailIndex >= 0)
{
    this.groupBar1.SelectedItem = mailIndex;
}
```

### Handling No Selection

```csharp
// Check if any item is selected (-1 means no selection)
if (this.groupBar1.SelectedItem == -1)
{
    // No item selected, set default
    this.groupBar1.SelectedItem = 0;
}
```

## Item Selection and Navigation

### Programmatic Navigation

```csharp
// Navigate to next item
private void NavigateNext()
{
    int currentIndex = this.groupBar1.SelectedItem;
    int nextIndex = currentIndex + 1;
    
    if (nextIndex < this.groupBar1.GroupBarItems.Count)
    {
        this.groupBar1.SelectedItem = nextIndex;
    }
    else
    {
        // Wrap to first item
        this.groupBar1.SelectedItem = 0;
    }
}

// Navigate to previous item
private void NavigatePrevious()
{
    int currentIndex = this.groupBar1.SelectedItem;
    int previousIndex = currentIndex - 1;
    
    if (previousIndex >= 0)
    {
        this.groupBar1.SelectedItem = previousIndex;
    }
    else
    {
        // Wrap to last item
        this.groupBar1.SelectedItem = this.groupBar1.GroupBarItems.Count - 1;
    }
}
```

### Keyboard Navigation

```csharp
// Add keyboard shortcuts for navigation
private void Form1_KeyDown(object sender, KeyEventArgs e)
{
    if (e.Control)
    {
        switch (e.KeyCode)
        {
            case Keys.D1:
                this.groupBar1.SelectedItem = 0;
                e.Handled = true;
                break;
            case Keys.D2:
                if (this.groupBar1.GroupBarItems.Count > 1)
                    this.groupBar1.SelectedItem = 1;
                e.Handled = true;
                break;
            case Keys.D3:
                if (this.groupBar1.GroupBarItems.Count > 2)
                    this.groupBar1.SelectedItem = 2;
                e.Handled = true;
                break;
            case Keys.PageDown:
                NavigateNext();
                e.Handled = true;
                break;
            case Keys.PageUp:
                NavigatePrevious();
                e.Handled = true;
                break;
        }
    }
}
```

## GroupBarItemSelected Event Handling

The **GroupBarItemSelected** event fires when a user selects a different item.

### Basic Event Handling

```csharp
// Wire up event
this.groupBar1.GroupBarItemSelected += GroupBar1_GroupBarItemSelected;

// Event handler
private void GroupBar1_GroupBarItemSelected(object sender, EventArgs e)
{
    int selectedIndex = this.groupBar1.SelectedItem;
    GroupBarItem selectedItem = this.groupBar1.GroupBarItems[selectedIndex];
    
    Console.WriteLine($"Selected: {selectedItem.Text}");
    
    // Update UI based on selection
    this.Text = $"My Application - {selectedItem.Text}";
}
```

### Advanced Selection Handling

```csharp
private void GroupBar1_GroupBarItemSelected(object sender, EventArgs e)
{
    int selectedIndex = this.groupBar1.SelectedItem;
    
    // Bounds checking
    if (selectedIndex < 0 || selectedIndex >= this.groupBar1.GroupBarItems.Count)
        return;
    
    GroupBarItem selectedItem = this.groupBar1.GroupBarItems[selectedIndex];
    
    // Perform actions based on selected item
    switch (selectedItem.Text)
    {
        case "Mail":
            LoadMailContent();
            break;
        case "Calendar":
            LoadCalendarContent();
            break;
        case "Contacts":
            LoadContactsContent();
            break;
        default:
            LoadDefaultContent();
            break;
    }
    
    // Update status bar
    UpdateStatusBar($"Viewing: {selectedItem.Text}");
}

private void LoadMailContent()
{
    // Lazy load mail data only when needed
    if (mailItem.Client != null)
    {
        GroupView mailView = mailItem.Client as GroupView;
        if (mailView != null && mailView.GroupViewItems.Count == 0)
        {
            // Load mail folders from database
            LoadMailFolders(mailView);
        }
    }
}
```

### Preventing Selection Change

Use the **GroupBarItemSelectionChanging** event to validate or prevent selection changes:

```csharp
this.groupBar1.GroupBarItemSelectionChanging += GroupBar1_GroupBarItemSelectionChanging;

private void GroupBar1_GroupBarItemSelectionChanging(object sender, 
    Syncfusion.Windows.Forms.Tools.GroupBarItemSelectionChangingEventArgs e)
{
    // Check if user has unsaved changes
    if (HasUnsavedChanges())
    {
        DialogResult result = MessageBox.Show(
            "You have unsaved changes. Continue?",
            "Unsaved Changes",
            MessageBoxButtons.YesNo,
            MessageBoxIcon.Warning);
        
        if (result == DialogResult.No)
        {
            // Cancel the selection change
            e.Cancel = true;
            return;
        }
    }
    
    Console.WriteLine($"Switching from index {e.OldSelected} to {e.NewSelected}");
}
```

## Complete Examples

### Example 1: Basic Navigation with Three Items

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class BasicNavigationForm : Form
{
    private GroupBar groupBar1;
    private GroupBarItem mailItem;
    private GroupBarItem calendarItem;
    private GroupBarItem tasksItem;
    private Panel mailPanel;
    private Panel calendarPanel;
    private Panel tasksPanel;

    public BasicNavigationForm()
    {
        InitializeComponent();
        SetupGroupBar();
    }

    private void SetupGroupBar()
    {
        // Create GroupBar
        this.groupBar1 = new GroupBar
        {
            Dock = DockStyle.Left,
            Width = 200,
            BorderStyle = BorderStyle.FixedSingle
        };

        // Create items
        this.mailItem = new GroupBarItem { Text = "Mail" };
        this.calendarItem = new GroupBarItem { Text = "Calendar" };
        this.tasksItem = new GroupBarItem { Text = "Tasks" };

        // Create client panels
        this.mailPanel = CreateContentPanel("Mail Content", Color.AliceBlue);
        this.calendarPanel = CreateContentPanel("Calendar Content", Color.LightYellow);
        this.tasksPanel = CreateContentPanel("Tasks Content", Color.LightGreen);

        // Assign clients
        this.mailItem.Client = this.mailPanel;
        this.calendarItem.Client = this.calendarPanel;
        this.tasksItem.Client = this.tasksPanel;

        // Add clients to GroupBar
        this.groupBar1.Controls.AddRange(new Control[] {
            this.mailPanel,
            this.calendarPanel,
            this.tasksPanel
        });

        // Add items to GroupBar
        this.groupBar1.GroupBarItems.AddRange(new GroupBarItem[] {
            this.mailItem,
            this.calendarItem,
            this.tasksItem
        });

        // Set initial selection
        this.groupBar1.SelectedItem = 0;

        // Handle selection
        this.groupBar1.GroupBarItemSelected += (s, e) =>
        {
            int index = this.groupBar1.SelectedItem;
            string itemText = this.groupBar1.GroupBarItems[index].Text;
            this.Text = $"Navigation Demo - {itemText}";
        };

        // Add to form
        this.Controls.Add(this.groupBar1);
    }

    private Panel CreateContentPanel(string labelText, Color backColor)
    {
        Panel panel = new Panel
        {
            Dock = DockStyle.Fill,
            BackColor = backColor,
            Padding = new Padding(20)
        };

        Label label = new Label
        {
            Text = labelText,
            Dock = DockStyle.Top,
            Font = new Font("Segoe UI", 16F, FontStyle.Bold),
            Height = 40
        };

        panel.Controls.Add(label);
        return panel;
    }
}
```

**Result:** A simple three-item navigation interface where clicking each item displays a different colored panel with a label.

### Example 2: Multiple Items with GroupView Clients

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class OutlookStyleForm : Form
{
    private GroupBar groupBar1;

    public OutlookStyleForm()
    {
        InitializeComponent();
        CreateOutlookInterface();
    }

    private void CreateOutlookInterface()
    {
        // Create GroupBar
        this.groupBar1 = new GroupBar
        {
            Dock = DockStyle.Left,
            Width = 220,
            BorderStyle = BorderStyle.Fixed3D,
            Font = new Font("Segoe UI", 9F)
        };

        // Create navigation items
        CreateMailSection();
        CreateCalendarSection();
        CreateContactsSection();
        CreateTasksSection();

        // Set initial selection
        this.groupBar1.SelectedItem = 0;

        // Handle item selection
        this.groupBar1.GroupBarItemSelected += OnNavigationChanged;

        // Add to form
        this.Controls.Add(this.groupBar1);
        this.Text = "Outlook-Style Navigation";
    }

    private void CreateMailSection()
    {
        GroupBarItem mailItem = new GroupBarItem
        {
            Text = "Mail",
            Image = Properties.Resources.MailIcon // 16x16 icon
        };

        GroupView mailView = new GroupView { Name = "MailView" };
        mailView.GroupViewItems.AddRange(new GroupViewItem[]
        {
            new GroupViewItem("Inbox (15)", 0, true, null, "Inbox"),
            new GroupViewItem("Drafts (3)", 1, true, null, "Drafts"),
            new GroupViewItem("Sent Items", 2, true, null, "Sent"),
            new GroupViewItem("Deleted Items", 3, true, null, "Deleted"),
            new GroupViewItem("Junk Email", 4, true, null, "Junk"),
            new GroupViewItem("Outbox", 5, true, null, "Outbox")
        });

        // Handle folder selection
        mailView.GroupViewItemSelected += (s, e) =>
        {
            int selectedIndex = mailView.SelectedItem;
            if (selectedIndex >= 0)
            {
                string folderName = mailView.GroupViewItems[selectedIndex].Text;
                Console.WriteLine($"Mail folder selected: {folderName}");
            }
        };

        mailItem.Client = mailView;
        this.groupBar1.Controls.Add(mailView);
        this.groupBar1.GroupBarItems.Add(mailItem);
    }

    private void CreateCalendarSection()
    {
        GroupBarItem calendarItem = new GroupBarItem
        {
            Text = "Calendar",
            Image = Properties.Resources.CalendarIcon
        };

        GroupView calendarView = new GroupView { Name = "CalendarView" };
        calendarView.GroupViewItems.AddRange(new GroupViewItem[]
        {
            new GroupViewItem("My Calendar", 0, true, null, "MyCalendar"),
            new GroupViewItem("Team Calendar", 1, true, null, "TeamCalendar"),
            new GroupViewItem("Birthdays", 2, true, null, "Birthdays"),
            new GroupViewItem("Holidays", 3, true, null, "Holidays")
        });

        calendarItem.Client = calendarView;
        this.groupBar1.Controls.Add(calendarView);
        this.groupBar1.GroupBarItems.Add(calendarItem);
    }

    private void CreateContactsSection()
    {
        GroupBarItem contactsItem = new GroupBarItem
        {
            Text = "Contacts",
            Image = Properties.Resources.ContactsIcon
        };

        GroupView contactsView = new GroupView { Name = "ContactsView" };
        contactsView.GroupViewItems.AddRange(new GroupViewItem[]
        {
            new GroupViewItem("All Contacts", 0, true, null, "AllContacts"),
            new GroupViewItem("Colleagues", 1, true, null, "Colleagues"),
            new GroupViewItem("Friends", 2, true, null, "Friends"),
            new GroupViewItem("Family", 3, true, null, "Family")
        });

        contactsItem.Client = contactsView;
        this.groupBar1.Controls.Add(contactsView);
        this.groupBar1.GroupBarItems.Add(contactsItem);
    }

    private void CreateTasksSection()
    {
        GroupBarItem tasksItem = new GroupBarItem
        {
            Text = "Tasks",
            Image = Properties.Resources.TasksIcon
        };

        GroupView tasksView = new GroupView { Name = "TasksView" };
        tasksView.GroupViewItems.AddRange(new GroupViewItem[]
        {
            new GroupViewItem("To Do (8)", 0, true, null, "Todo"),
            new GroupViewItem("In Progress (4)", 1, true, null, "InProgress"),
            new GroupViewItem("Completed", 2, true, null, "Completed"),
            new GroupViewItem("Waiting", 3, true, null, "Waiting")
        });

        tasksItem.Client = tasksView;
        this.groupBar1.Controls.Add(tasksView);
        this.groupBar1.GroupBarItems.Add(tasksItem);
    }

    private void OnNavigationChanged(object sender, EventArgs e)
    {
        int selectedIndex = this.groupBar1.SelectedItem;
        if (selectedIndex >= 0 && selectedIndex < this.groupBar1.GroupBarItems.Count)
        {
            GroupBarItem item = this.groupBar1.GroupBarItems[selectedIndex];
            this.Text = $"Outlook-Style Navigation - {item.Text}";
            
            // Update application state based on selection
            Console.WriteLine($"Navigated to: {item.Text}");
        }
    }
}
```

**Result:** A complete Outlook-style navigation interface with Mail, Calendar, Contacts, and Tasks sections, each containing relevant sub-items in a GroupView.

## Key Takeaways

1. **GroupBarItem** represents individual navigation tabs in the GroupBar
2. **Text Property** sets the display label (supports dynamic updates)
3. **Image Property** adds visual icons to items
4. **Client Property** links controls to items (two-step process: assign + add to Controls)
5. **GroupBarItems Collection** manages all items (add, remove, reorder)
6. **SelectedItem** property controls and queries which item is active
7. **GroupBarItemSelected** event handles navigation logic
8. **Container-Client Model** separates navigation (GroupBar/Items) from content (Client controls)
