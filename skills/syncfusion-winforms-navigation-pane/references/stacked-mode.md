# Stacked Mode

This guide covers the StackedMode feature of the GroupBar control, which transforms the standard navigation into an Outlook-style collapsible navigation pane with a bottom button bar. Stacked mode provides a compact, professional navigation experience similar to Microsoft Outlook.

## Table of Contents

- [StackedMode Property Overview](#stackedmode-property-overview)
- [Enabling Stacked Mode](#enabling-stacked-mode)
- [Outlook-Style Navigation Pane Behavior](#outlook-style-navigation-pane-behavior)
- [Navigation Pane at Bottom](#navigation-pane-at-bottom)
- [HeaderHeight Property](#headerheight-property)
- [Collapsed and Expanded State](#collapsed-and-expanded-state)
- [Navigation Between Stacked Items](#navigation-between-stacked-items)
- [Serialization Support for Stacked Layout](#serialization-support-for-stacked-layout)
- [Complete Outlook Clone Example](#complete-outlook-clone-example)

## StackedMode Property Overview

The **StackedMode** property enables a compact navigation layout where:

- Selected item displays at the top with full content
- Non-selected items appear as buttons at the bottom
- Navigation pane provides quick access to all items
- Users can customize which items appear in the navigation pane

```csharp
// Enable stacked mode
this.groupBar1.StackedMode = true;
```

**Visual Comparison:**

| Regular Mode | Stacked Mode |
|--------------|--------------|
| All items visible as tabs | Selected item fills space |
| Click to switch items | Bottom navigation buttons |
| Standard layout | Outlook-style layout |
| Fixed item arrangement | Customizable navigation pane |

**When to use Stacked Mode:**
- Building Outlook-style interfaces
- Need more content space
- Want collapsible navigation
- Users benefit from quick-access buttons
- Professional business applications

**When to avoid Stacked Mode:**
- Few navigation items (3 or less)
- Simple, flat navigation preferred
- Touch-first mobile interfaces
- Users unfamiliar with Outlook

## Enabling Stacked Mode

### Basic Activation

```csharp
using Syncfusion.Windows.Forms.Tools;

// Create GroupBar
GroupBar groupBar1 = new GroupBar
{
    Dock = DockStyle.Left,
    Width = 220
};

// Enable stacked mode
groupBar1.StackedMode = true;
```

**Result:** The GroupBar transforms into stacked layout with the selected item at top and navigation buttons at bottom.

### Complete Setup Example

```csharp
private void SetupStackedGroupBar()
{
    // Create and configure GroupBar
    this.groupBar1 = new GroupBar
    {
        Dock = DockStyle.Left,
        Width = 240,
        BorderStyle = BorderStyle.Fixed3D,
        StackedMode = true,  // Enable stacked mode
        Font = new Font("Segoe UI", 9F)
    };

    // Create items
    CreateMailItem();
    CreateCalendarItem();
    CreateContactsItem();
    CreateTasksItem();

    // Set initial selection
    this.groupBar1.SelectedItem = 0;

    // Add to form
    this.Controls.Add(this.groupBar1);
}
```

### Toggling Stacked Mode at Runtime

Allow users to switch between regular and stacked modes:

```csharp
private void ToggleStackedMode()
{
    // Toggle the mode
    this.groupBar1.StackedMode = !this.groupBar1.StackedMode;
    
    // Update UI to reflect change
    string mode = this.groupBar1.StackedMode ? "Stacked" : "Regular";
    Console.WriteLine($"GroupBar mode: {mode}");
}

// Toolbar button or menu item
private void btnToggleMode_Click(object sender, EventArgs e)
{
    ToggleStackedMode();
    
    // Update button text
    btnToggleMode.Text = this.groupBar1.StackedMode 
        ? "Switch to Regular Mode" 
        : "Switch to Stacked Mode";
}
```

## Outlook-Style Navigation Pane Behavior

In stacked mode, the GroupBar mimics Microsoft Outlook's navigation pane:

### Navigation Pane Buttons

Items appear as buttons in the navigation pane. Control which items appear:

```csharp
// Set which items appear in navigation pane
this.groupBarItem1.InNavigationPane = true;  // Mail - visible
this.groupBarItem2.InNavigationPane = true;  // Calendar - visible
this.groupBarItem3.InNavigationPane = true;  // Contacts - visible
this.groupBarItem4.InNavigationPane = false; // Tasks - in overflow menu
```

**InNavigationPane = true:** Item shows as button in bottom navigation pane.  
**InNavigationPane = false:** Item appears in overflow dropdown menu.

### Navigation Pane Icons

Display icons on navigation buttons:

```csharp
// Load icon images
ImageList navigationIcons = new ImageList
{
    ImageSize = new Size(32, 32),
    ColorDepth = ColorDepth.Depth32Bit
};

navigationIcons.Images.Add("mail", Properties.Resources.MailIcon32);
navigationIcons.Images.Add("calendar", Properties.Resources.CalendarIcon32);
navigationIcons.Images.Add("contacts", Properties.Resources.ContactsIcon32);

// Assign icons to items
this.groupBarItem1.NavigationPaneIcon = navigationIcons.Images["mail"];
this.groupBarItem2.NavigationPaneIcon = navigationIcons.Images["calendar"];
this.groupBarItem3.NavigationPaneIcon = navigationIcons.Images["contacts"];

// Enable large image mode for navigation pane
this.groupBarItem1.LargeImageMode = true;
this.groupBarItem2.LargeImageMode = true;
this.groupBarItem3.LargeImageMode = true;
```

**Result:** Navigation buttons display with 32x32 icons for clear visual identification.

### Navigation Pane Images

Alternatively, use images instead of icons:

```csharp
// Assign images to navigation pane
this.groupBarItem1.NavigationPaneImage = Properties.Resources.MailImage;
this.groupBarItem2.NavigationPaneImage = Properties.Resources.CalendarImage;
this.groupBarItem3.NavigationPaneImage = Properties.Resources.ContactsImage;
```

### Complete Navigation Pane Setup

```csharp
private void ConfigureNavigationPane()
{
    // Create ImageList for navigation icons
    ImageList navIcons = new ImageList
    {
        ImageSize = new Size(32, 32),
        ColorDepth = ColorDepth.Depth32Bit
    };
    
    // Add icons
    navIcons.Images.Add(LoadIcon("mail_32.png"));
    navIcons.Images.Add(LoadIcon("calendar_32.png"));
    navIcons.Images.Add(LoadIcon("contacts_32.png"));
    navIcons.Images.Add(LoadIcon("tasks_32.png"));
    
    // Configure Mail item
    this.mailItem.InNavigationPane = true;
    this.mailItem.NavigationPaneIcon = navIcons.Images[0];
    this.mailItem.LargeImageMode = true;
    
    // Configure Calendar item
    this.calendarItem.InNavigationPane = true;
    this.calendarItem.NavigationPaneIcon = navIcons.Images[1];
    this.calendarItem.LargeImageMode = true;
    
    // Configure Contacts item
    this.contactsItem.InNavigationPane = true;
    this.contactsItem.NavigationPaneIcon = navIcons.Images[2];
    this.contactsItem.LargeImageMode = true;
    
    // Configure Tasks item (in overflow)
    this.tasksItem.InNavigationPane = false; // Appears in dropdown
    this.tasksItem.NavigationPaneIcon = navIcons.Images[3];
    this.tasksItem.LargeImageMode = true;
}

private Icon LoadIcon(string fileName)
{
    string iconPath = Path.Combine(Application.StartupPath, "Icons", fileName);
    return new Icon(iconPath);
}
```

## Navigation Pane at Bottom

The navigation pane appears at the bottom of the GroupBar in stacked mode.

### Navigation Pane Height

Control the height of the navigation pane:

```csharp
// Set navigation pane height
this.groupBar1.NavigationPaneHeight = 45;
```

**Recommended heights:**
- **35-40**: Compact mode (small icons)
- **45-50**: Standard mode (medium icons)
- **55-65**: Large mode (large icons, touch-friendly)

```csharp
// Different sizes for different scenarios
private void SetNavigationPaneSize(string size)
{
    switch (size.ToLower())
    {
        case "compact":
            this.groupBar1.NavigationPaneHeight = 38;
            this.groupBar1.NavigationPaneButtonWidth = 38;
            break;
        case "standard":
            this.groupBar1.NavigationPaneHeight = 48;
            this.groupBar1.NavigationPaneButtonWidth = 48;
            break;
        case "large":
            this.groupBar1.NavigationPaneHeight = 60;
            this.groupBar1.NavigationPaneButtonWidth = 60;
            break;
    }
}
```

### Navigation Pane Button Width

Control individual button widths:

```csharp
// Set button width
this.groupBar1.NavigationPaneButtonWidth = 50;
```

**When to adjust button width:**
- More items in navigation pane
- Larger icons require more space
- Touch interfaces need bigger targets
- Accommodate longer text labels

### Navigation Pane Tooltips

Set custom tooltips for navigation elements:

```csharp
// Configure tooltips
this.groupBar1.NavigationPaneTooltip = "Show Navigation Options";
this.groupBar1.MinimizeButtonToolTip = "Minimize Navigation Pane";
this.groupBar1.ExpandButtonToolTip = "Expand Navigation Pane";
```

### Complete Navigation Pane Configuration

```csharp
private void SetupNavigationPane()
{
    // Enable stacked mode
    this.groupBar1.StackedMode = true;
    
    // Configure navigation pane size
    this.groupBar1.NavigationPaneHeight = 48;
    this.groupBar1.NavigationPaneButtonWidth = 50;
    
    // Show chevron (dropdown for overflow items)
    this.groupBar1.ShowChevron = true;
    
    // Configure tooltips
    this.groupBar1.NavigationPaneTooltip = "Show more navigation options";
    this.groupBar1.MinimizeButtonToolTip = "Minimize the navigation pane";
    this.groupBar1.ExpandButtonToolTip = "Expand the navigation pane";
    
    // Set which items appear in navigation pane
    foreach (GroupBarItem item in this.groupBar1.GroupBarItems)
    {
        // First 4 items in pane, rest in overflow
        int index = this.groupBar1.GroupBarItems.IndexOf(item);
        item.InNavigationPane = (index < 4);
    }
}
```

## HeaderHeight Property

The **HeaderHeight** property controls the height of the GroupBar header in stacked mode.

### Setting Header Height

```csharp
// Standard header height
this.groupBar1.HeaderHeight = 30;

// Hide header completely
this.groupBar1.HeaderHeight = 0;

// Tall header for prominence
this.groupBar1.HeaderHeight = 50;
```

**Header Height Guidelines:**

| Height | Use Case |
|--------|----------|
| 0 | Hide header completely |
| 20-25 | Minimal header |
| 28-32 | Standard header |
| 40-50 | Prominent header |
| 60+ | Extra-large header |

### Hiding the Header

```csharp
// Completely hide the header in stacked mode
this.groupBar1.StackedMode = true;
this.groupBar1.HeaderHeight = 0;
```

**When to hide the header:**
- Maximum content space needed
- Header content is redundant
- Minimalist design requirements
- Mobile/tablet layouts

### Dynamic Header Height

Adjust header height based on content or state:

```csharp
private void UpdateHeaderHeight(bool showDetailedHeader)
{
    if (showDetailedHeader)
    {
        this.groupBar1.HeaderHeight = 50;
        // Show additional header content
    }
    else
    {
        this.groupBar1.HeaderHeight = 30;
        // Show compact header
    }
}
```

### Header with Custom Content

```csharp
private void SetupHeaderWithImage()
{
    // Set header height to accommodate image
    this.groupBar1.HeaderHeight = 40;
    
    // Show selected item's image in header
    this.groupBar1.ShowItemImageInHeader = true;
    
    // Configure items with images
    this.groupBarItem1.Image = Properties.Resources.MailIcon;
    this.groupBarItem2.Image = Properties.Resources.CalendarIcon;
}
```

**Result:** Selected item's icon displays in the header, providing visual context.

## Collapsed and Expanded State

In stacked mode, the GroupBar can be collapsed to save space.

### AllowCollapse Property

Enable collapsing functionality:

```csharp
// Allow users to collapse the navigation pane
this.groupBar1.AllowCollapse = true;
```

### Collapsed Property

Get or set the collapsed state:

```csharp
// Check if collapsed
bool isCollapsed = this.groupBar1.Collapsed;

// Programmatically collapse
this.groupBar1.Collapsed = true;

// Programmatically expand
this.groupBar1.Collapsed = false;
```

### Collapsed Width

Control how wide the collapsed pane is:

```csharp
// Set width when collapsed
this.groupBar1.CollapsedWidth = 40;
```

**Typical collapsed widths:**
- **30-35**: Icon only, very compact
- **40-45**: Icon + minimal padding (recommended)
- **50-60**: Icon + some text
- **60+**: Full vertical text

### Collapsed Text

Set text displayed when collapsed:

```csharp
// Set text for collapsed state
this.groupBar1.CollapsedText = "Navigation Pane";
```

**Result:** Text appears vertically along the collapsed pane edge.

### Complete Collapse Configuration

```csharp
private void ConfigureCollapseFeature()
{
    // Enable collapsing
    this.groupBar1.AllowCollapse = true;
    
    // Set collapsed appearance
    this.groupBar1.CollapsedWidth = 42;
    this.groupBar1.CollapsedText = "Navigation";
    
    // Set custom collapse/expand button images
    this.groupBar1.CollapseImage = Properties.Resources.CollapseIcon;
    this.groupBar1.ExpandImage = Properties.Resources.ExpandIcon;
    
    // Handle collapse state changes
    this.groupBar1.CollapsedChanged += (s, e) =>
    {
        bool collapsed = this.groupBar1.Collapsed;
        Console.WriteLine($"Navigation pane {(collapsed ? "collapsed" : "expanded")}");
        
        // Adjust main content area if needed
        AdjustContentLayout(collapsed);
    };
}

private void AdjustContentLayout(bool navPaneCollapsed)
{
    // Maximize content area when nav pane is collapsed
    if (navPaneCollapsed)
    {
        // Content gets more space
        Console.WriteLine("Content area expanded");
    }
    else
    {
        // Standard layout
        Console.WriteLine("Standard content layout");
    }
}
```

### Toggle Button for Collapse

```csharp
// Add button to toggle collapsed state
private void btnToggleCollapse_Click(object sender, EventArgs e)
{
    this.groupBar1.Collapsed = !this.groupBar1.Collapsed;
    
    // Update button text
    btnToggleCollapse.Text = this.groupBar1.Collapsed 
        ? "Expand Navigation" 
        : "Collapse Navigation";
}
```

## Navigation Between Stacked Items

Users navigate between items using the bottom navigation pane buttons.

### Programmatic Navigation

```csharp
// Navigate to specific item by index
this.groupBar1.SelectedItem = 0; // Mail
this.groupBar1.SelectedItem = 1; // Calendar
this.groupBar1.SelectedItem = 2; // Contacts

// Navigate by finding item
private void NavigateToItem(string itemText)
{
    for (int i = 0; i < this.groupBar1.GroupBarItems.Count; i++)
    {
        if (this.groupBar1.GroupBarItems[i].Text == itemText)
        {
            this.groupBar1.SelectedItem = i;
            break;
        }
    }
}

// Usage
NavigateToItem("Calendar");
```

### Navigation Shortcuts

Implement keyboard shortcuts for quick navigation:

```csharp
private void Form1_KeyDown(object sender, KeyEventArgs e)
{
    // Alt + number for quick navigation
    if (e.Alt)
    {
        switch (e.KeyCode)
        {
            case Keys.D1:
                if (this.groupBar1.GroupBarItems.Count > 0)
                    this.groupBar1.SelectedItem = 0; // Mail
                break;
            case Keys.D2:
                if (this.groupBar1.GroupBarItems.Count > 1)
                    this.groupBar1.SelectedItem = 1; // Calendar
                break;
            case Keys.D3:
                if (this.groupBar1.GroupBarItems.Count > 2)
                    this.groupBar1.SelectedItem = 2; // Contacts
                break;
            case Keys.D4:
                if (this.groupBar1.GroupBarItems.Count > 3)
                    this.groupBar1.SelectedItem = 3; // Tasks
                break;
        }
        e.Handled = true;
    }
}
```

### Navigation Pane Dropdown

Handle the dropdown menu for overflow items:

```csharp
// Handle navigation pane dropdown click
this.groupBar1.NavigationPaneDropDownClick += GroupBar1_NavigationPaneDropDownClick;

private void GroupBar1_NavigationPaneDropDownClick(object sender, 
    Syncfusion.Windows.Forms.Tools.NavigationPaneDropDownClickEventArgs e)
{
    Console.WriteLine("Navigation pane dropdown clicked");
    
    // Access context menu provider
    var menuProvider = e.ContextMenuProvider;
    
    // You can customize the dropdown menu here
}
```

## Serialization Support for Stacked Layout

Save and restore the navigation pane configuration.

### Saving Layout State

```csharp
using Syncfusion.Runtime.Serialization;

private void SaveNavigationLayout()
{
    // Create storage for layout information
    ArrayList layoutInfo = new ArrayList();
    
    // Store which items are in navigation pane
    foreach (GroupBarItem item in this.groupBar1.GroupBarItems)
    {
        if (item.InNavigationPane)
        {
            int index = this.groupBar1.GroupBarItems.IndexOf(item);
            layoutInfo.Add(index);
        }
    }
    
    // Store selected item index
    layoutInfo.Add(this.groupBar1.SelectedItem);
    
    // Store collapsed state
    layoutInfo.Add(this.groupBar1.Collapsed);
    
    // Persist to XML file
    string configPath = Path.Combine(
        Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData),
        "MyApp",
        "NavigationLayout.xml"
    );
    
    Directory.CreateDirectory(Path.GetDirectoryName(configPath));
    
    AppStateSerializer serializer = new AppStateSerializer(
        SerializeMode.XMLFile, 
        configPath
    );
    
    serializer.SerializeObject("NavigationLayout", layoutInfo);
    serializer.PersistNow();
    
    Console.WriteLine("Navigation layout saved");
}
```

### Loading Layout State

```csharp
private void LoadNavigationLayout()
{
    try
    {
        string configPath = Path.Combine(
            Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData),
            "MyApp",
            "NavigationLayout.xml"
        );
        
        if (!File.Exists(configPath))
        {
            Console.WriteLine("No saved layout found");
            return;
        }
        
        // Deserialize layout information
        AppStateSerializer serializer = new AppStateSerializer(
            SerializeMode.XMLFile,
            configPath
        );
        
        ArrayList layoutInfo = serializer.DeserializeObject("NavigationLayout") as ArrayList;
        
        if (layoutInfo == null || layoutInfo.Count == 0)
            return;
        
        // Reset all items
        foreach (GroupBarItem item in this.groupBar1.GroupBarItems)
        {
            item.InNavigationPane = false;
        }
        
        // Restore navigation pane items
        for (int i = 0; i < layoutInfo.Count - 2; i++) // Last 2 are selectedItem and collapsed state
        {
            int itemIndex = (int)layoutInfo[i];
            if (itemIndex >= 0 && itemIndex < this.groupBar1.GroupBarItems.Count)
            {
                this.groupBar1.GroupBarItems[itemIndex].InNavigationPane = true;
            }
        }
        
        // Restore selected item
        int selectedIndex = (int)layoutInfo[layoutInfo.Count - 2];
        if (selectedIndex >= 0 && selectedIndex < this.groupBar1.GroupBarItems.Count)
        {
            this.groupBar1.SelectedItem = selectedIndex;
        }
        
        // Restore collapsed state
        bool collapsed = (bool)layoutInfo[layoutInfo.Count - 1];
        this.groupBar1.Collapsed = collapsed;
        
        Console.WriteLine("Navigation layout loaded");
    }
    catch (Exception ex)
    {
        Console.WriteLine($"Error loading layout: {ex.Message}");
    }
}
```

### Auto-Save on Exit

```csharp
private void Form1_FormClosing(object sender, FormClosingEventArgs e)
{
    // Save layout when application closes
    SaveNavigationLayout();
}

private void Form1_Load(object sender, EventArgs e)
{
    // Load layout when application starts
    LoadNavigationLayout();
}
```

## Complete Outlook Clone Example

A full-featured Outlook-style application with stacked mode:

```csharp
using System;
using System.Drawing;
using System.IO;
using System.Windows.Forms;
using System.Collections;
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Runtime.Serialization;

public class OutlookStackedForm : Form
{
    private GroupBar navigationPane;
    private Panel contentArea;
    private Label titleLabel;
    private RichTextBox contentDisplay;
    private ToolStrip toolbar;

    public OutlookStackedForm()
    {
        this.Text = "Outlook-Style Application";
        this.Size = new Size(1200, 800);
        this.StartPosition = FormStartPosition.CenterScreen;
        
        CreateToolbar();
        CreateNavigationPane();
        CreateContentArea();
        
        this.Load += OutlookStackedForm_Load;
        this.FormClosing += OutlookStackedForm_FormClosing;
    }

    private void CreateToolbar()
    {
        this.toolbar = new ToolStrip
        {
            GripStyle = ToolStripGripStyle.Hidden
        };
        
        // Add toolbar buttons
        ToolStripButton btnToggleNav = new ToolStripButton
        {
            Text = "Toggle Navigation",
            DisplayStyle = ToolStripItemDisplayStyle.Text
        };
        btnToggleNav.Click += (s, e) =>
        {
            this.navigationPane.Collapsed = !this.navigationPane.Collapsed;
        };
        
        ToolStripButton btnSaveLayout = new ToolStripButton
        {
            Text = "Save Layout",
            DisplayStyle = ToolStripItemDisplayStyle.Text
        };
        btnSaveLayout.Click += (s, e) => SaveLayout();
        
        this.toolbar.Items.Add(btnToggleNav);
        this.toolbar.Items.Add(new ToolStripSeparator());
        this.toolbar.Items.Add(btnSaveLayout);
        
        this.Controls.Add(this.toolbar);
    }

    private void CreateNavigationPane()
    {
        this.navigationPane = new GroupBar
        {
            Dock = DockStyle.Left,
            Width = 260,
            BorderStyle = BorderStyle.Fixed3D,
            Font = new Font("Segoe UI", 9F),
            BackColor = Color.FromArgb(245, 246, 247),
            
            // Enable stacked mode
            StackedMode = true,
            
            // Configure collapse feature
            AllowCollapse = true,
            CollapsedWidth = 42,
            CollapsedText = "Navigation",
            
            // Configure navigation pane
            NavigationPaneHeight = 50,
            NavigationPaneButtonWidth = 52,
            ShowChevron = true,
            
            // Configure header
            HeaderHeight = 35,
            ShowItemImageInHeader = true
        };
        
        // Create navigation sections
        CreateMailSection();
        CreateCalendarSection();
        CreateContactsSection();
        CreateTasksSection();
        CreateNotesSection();
        
        // Set initial selection
        this.navigationPane.SelectedItem = 0;
        
        // Handle item selection
        this.navigationPane.GroupBarItemSelected += NavigationPane_GroupBarItemSelected;
        
        this.Controls.Add(this.navigationPane);
    }

    private void CreateMailSection()
    {
        GroupBarItem item = new GroupBarItem
        {
            Text = "Mail",
            InNavigationPane = true,
            LargeImageMode = true,
            NavigationPaneImage = CreateColoredIcon(Color.FromArgb(0, 120, 215), "M")
        };
        
        GroupView folders = new GroupView { Name = "MailFolders", BackColor = Color.White };
        folders.GroupViewItems.AddRange(new GroupViewItem[]
        {
            new GroupViewItem("📥 Inbox (24)", -1, true, "inbox", "Inbox"),
            new GroupViewItem("📝 Drafts (3)", -1, true, "drafts", "Drafts"),
            new GroupViewItem("📤 Sent Items", -1, true, "sent", "Sent"),
            new GroupViewItem("🗑️ Deleted Items", -1, true, "deleted", "Deleted"),
            new GroupViewItem("⚠️ Junk Email", -1, true, "junk", "Junk"),
            new GroupViewItem("📂 Archive", -1, true, "archive", "Archive")
        });
        
        folders.GroupViewItemSelected += Folders_ItemSelected;
        
        item.Client = folders;
        this.navigationPane.Controls.Add(folders);
        this.navigationPane.GroupBarItems.Add(item);
    }

    private void CreateCalendarSection()
    {
        GroupBarItem item = new GroupBarItem
        {
            Text = "Calendar",
            InNavigationPane = true,
            LargeImageMode = true,
            NavigationPaneImage = CreateColoredIcon(Color.FromArgb(208, 69, 37), "C")
        };
        
        GroupView views = new GroupView { Name = "CalendarViews", BackColor = Color.White };
        views.GroupViewItems.AddRange(new GroupViewItem[]
        {
            new GroupViewItem("📅 My Calendar", -1, true, "mycal", "MyCalendar"),
            new GroupViewItem("👥 Team Calendar", -1, true, "teamcal", "TeamCalendar"),
            new GroupViewItem("🎂 Birthdays", -1, true, "birthdays", "Birthdays"),
            new GroupViewItem("🏖️ Holidays", -1, true, "holidays", "Holidays")
        });
        
        views.GroupViewItemSelected += Folders_ItemSelected;
        
        item.Client = views;
        this.navigationPane.Controls.Add(views);
        this.navigationPane.GroupBarItems.Add(item);
    }

    private void CreateContactsSection()
    {
        GroupBarItem item = new GroupBarItem
        {
            Text = "Contacts",
            InNavigationPane = true,
            LargeImageMode = true,
            NavigationPaneImage = CreateColoredIcon(Color.FromArgb(122, 159, 60), "P")
        };
        
        GroupView categories = new GroupView { Name = "ContactsCategories", BackColor = Color.White };
        categories.GroupViewItems.AddRange(new GroupViewItem[]
        {
            new GroupViewItem("👤 All Contacts (186)", -1, true, "allcontacts", "AllContacts"),
            new GroupViewItem("💼 Colleagues (52)", -1, true, "colleagues", "Colleagues"),
            new GroupViewItem("👨‍👩‍👧‍👦 Family (18)", -1, true, "family", "Family"),
            new GroupViewItem("👥 Friends (34)", -1, true, "friends", "Friends")
        });
        
        categories.GroupViewItemSelected += Folders_ItemSelected;
        
        item.Client = categories;
        this.navigationPane.Controls.Add(categories);
        this.navigationPane.GroupBarItems.Add(item);
    }

    private void CreateTasksSection()
    {
        GroupBarItem item = new GroupBarItem
        {
            Text = "Tasks",
            InNavigationPane = true,
            LargeImageMode = true,
            NavigationPaneImage = CreateColoredIcon(Color.FromArgb(232, 17, 35), "T")
        };
        
        GroupView lists = new GroupView { Name = "TasksLists", BackColor = Color.White };
        lists.GroupViewItems.AddRange(new GroupViewItem[]
        {
            new GroupViewItem("✅ To Do (12)", -1, true, "todo", "Todo"),
            new GroupViewItem("🔄 In Progress (5)", -1, true, "inprogress", "InProgress"),
            new GroupViewItem("✔️ Completed", -1, true, "completed", "Completed"),
            new GroupViewItem("⏸️ Waiting (2)", -1, true, "waiting", "Waiting")
        });
        
        lists.GroupViewItemSelected += Folders_ItemSelected;
        
        item.Client = lists;
        this.navigationPane.Controls.Add(lists);
        this.navigationPane.GroupBarItems.Add(item);
    }

    private void CreateNotesSection()
    {
        GroupBarItem item = new GroupBarItem
        {
            Text = "Notes",
            InNavigationPane = false, // In overflow menu
            LargeImageMode = true,
            NavigationPaneImage = CreateColoredIcon(Color.FromArgb(255, 185, 0), "N")
        };
        
        GroupView notebooks = new GroupView { Name = "NotesNotebooks", BackColor = Color.White };
        notebooks.GroupViewItems.AddRange(new GroupViewItem[]
        {
            new GroupViewItem("📔 Personal Notes", -1, true, "personal", "Personal"),
            new GroupViewItem("💼 Work Notes", -1, true, "work", "Work"),
            new GroupViewItem("💡 Ideas", -1, true, "ideas", "Ideas")
        });
        
        notebooks.GroupViewItemSelected += Folders_ItemSelected;
        
        item.Client = notebooks;
        this.navigationPane.Controls.Add(notebooks);
        this.navigationPane.GroupBarItems.Add(item);
    }

    private void CreateContentArea()
    {
        this.contentArea = new Panel
        {
            Dock = DockStyle.Fill,
            BackColor = Color.White
        };
        
        this.titleLabel = new Label
        {
            Dock = DockStyle.Top,
            Height = 60,
            Font = new Font("Segoe UI", 18F, FontStyle.Bold),
            Text = "Inbox",
            Padding = new Padding(15),
            BackColor = Color.FromArgb(230, 235, 240),
            ForeColor = Color.FromArgb(50, 50, 50)
        };
        
        this.contentDisplay = new RichTextBox
        {
            Dock = DockStyle.Fill,
            Font = new Font("Segoe UI", 10F),
            BorderStyle = BorderStyle.None,
            Padding = new Padding(15),
            ReadOnly = true,
            Text = "Select a folder to view its contents."
        };
        
        this.contentArea.Controls.Add(this.contentDisplay);
        this.contentArea.Controls.Add(this.titleLabel);
        this.Controls.Add(this.contentArea);
    }

    private Image CreateColoredIcon(Color color, string text)
    {
        Bitmap bmp = new Bitmap(32, 32);
        using (Graphics g = Graphics.FromImage(bmp))
        {
            g.SmoothingMode = System.Drawing.Drawing2D.SmoothingMode.AntiAlias;
            g.Clear(Color.Transparent);
            
            // Draw colored circle
            using (SolidBrush brush = new SolidBrush(color))
            {
                g.FillEllipse(brush, 0, 0, 31, 31);
            }
            
            // Draw text
            using (Font font = new Font("Segoe UI", 14F, FontStyle.Bold))
            {
                SizeF textSize = g.MeasureString(text, font);
                float x = (32 - textSize.Width) / 2;
                float y = (32 - textSize.Height) / 2;
                g.DrawString(text, font, Brushes.White, x, y);
            }
        }
        return bmp;
    }

    private void NavigationPane_GroupBarItemSelected(object sender, EventArgs e)
    {
        int selectedIndex = this.navigationPane.SelectedItem;
        if (selectedIndex >= 0 && selectedIndex < this.navigationPane.GroupBarItems.Count)
        {
            GroupBarItem item = this.navigationPane.GroupBarItems[selectedIndex];
            this.titleLabel.Text = item.Text;
            this.contentDisplay.Text = $"Viewing: {item.Text}\n\nSelect a folder to see its contents.";
        }
    }

    private void Folders_ItemSelected(object sender, EventArgs e)
    {
        GroupView view = sender as GroupView;
        if (view == null) return;
        
        int selectedIndex = view.SelectedItem;
        if (selectedIndex >= 0 && selectedIndex < view.GroupViewItems.Count)
        {
            GroupViewItem item = view.GroupViewItems[selectedIndex];
            this.titleLabel.Text = item.Text;
            this.contentDisplay.Text = $"Contents of: {item.Text}\n\n";
            this.contentDisplay.Text += $"Folder Key: {item.Key}\n";
            this.contentDisplay.Text += $"Tag Data: {item.Tag}\n\n";
            this.contentDisplay.Text += "Folder contents would be displayed here.";
        }
    }

    private void SaveLayout()
    {
        try
        {
            ArrayList layoutInfo = new ArrayList();
            
            // Save navigation pane items
            foreach (GroupBarItem item in this.navigationPane.GroupBarItems)
            {
                if (item.InNavigationPane)
                {
                    layoutInfo.Add(this.navigationPane.GroupBarItems.IndexOf(item));
                }
            }
            
            // Save selected item and collapsed state
            layoutInfo.Add(this.navigationPane.SelectedItem);
            layoutInfo.Add(this.navigationPane.Collapsed);
            
            // Persist
            string appData = Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData);
            string configPath = Path.Combine(appData, "OutlookClone", "layout.xml");
            Directory.CreateDirectory(Path.GetDirectoryName(configPath));
            
            AppStateSerializer serializer = new AppStateSerializer(SerializeMode.XMLFile, configPath);
            serializer.SerializeObject("Layout", layoutInfo);
            serializer.PersistNow();
            
            MessageBox.Show("Layout saved successfully!", "Save Layout", 
                MessageBoxButtons.OK, MessageBoxIcon.Information);
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Error saving layout: {ex.Message}", "Error", 
                MessageBoxButtons.OK, MessageBoxIcon.Error);
        }
    }

    private void LoadLayout()
    {
        try
        {
            string appData = Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData);
            string configPath = Path.Combine(appData, "OutlookClone", "layout.xml");
            
            if (!File.Exists(configPath))
                return;
            
            AppStateSerializer serializer = new AppStateSerializer(SerializeMode.XMLFile, configPath);
            ArrayList layoutInfo = serializer.DeserializeObject("Layout") as ArrayList;
            
            if (layoutInfo == null) return;
            
            // Reset navigation pane
            foreach (GroupBarItem item in this.navigationPane.GroupBarItems)
            {
                item.InNavigationPane = false;
            }
            
            // Restore navigation pane items
            for (int i = 0; i < layoutInfo.Count - 2; i++)
            {
                int itemIndex = (int)layoutInfo[i];
                if (itemIndex >= 0 && itemIndex < this.navigationPane.GroupBarItems.Count)
                {
                    this.navigationPane.GroupBarItems[itemIndex].InNavigationPane = true;
                }
            }
            
            // Restore selected item
            this.navigationPane.SelectedItem = (int)layoutInfo[layoutInfo.Count - 2];
            
            // Restore collapsed state
            this.navigationPane.Collapsed = (bool)layoutInfo[layoutInfo.Count - 1];
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error loading layout: {ex.Message}");
        }
    }

    private void OutlookStackedForm_Load(object sender, EventArgs e)
    {
        LoadLayout();
    }

    private void OutlookStackedForm_FormClosing(object sender, FormClosingEventArgs e)
    {
        SaveLayout();
    }

    [STAThread]
    static void Main()
    {
        Application.EnableVisualStyles();
        Application.SetCompatibleTextRenderingDefault(false);
        Application.Run(new OutlookStackedForm());
    }
}
```

**Result:** A complete Outlook-style application with:
- Stacked navigation pane with Mail, Calendar, Contacts, Tasks, and Notes
- Collapsible navigation pane with custom width
- Navigation icons with colored backgrounds
- Bottom navigation buttons for quick access
- Overflow menu for additional items
- Layout serialization (saves/restores configuration)
- Toolbar for toggling navigation and saving layout

## Key Takeaways

1. **StackedMode** transforms GroupBar into Outlook-style navigation
2. **InNavigationPane** controls which items appear as bottom buttons
3. **NavigationPaneIcon** and **NavigationPaneImage** provide visual identity
4. **HeaderHeight** controls the top header size (0 to hide)
5. **AllowCollapse** enables collapsible navigation pane
6. **Collapsed/CollapsedWidth/CollapsedText** configure collapsed state
7. **Serialization** preserves user's navigation pane customization
8. **Perfect for** professional business applications mimicking Outlook
