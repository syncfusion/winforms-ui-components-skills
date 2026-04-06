# Advanced Features

This guide covers advanced GroupBar features including nested GroupBars, in-place renaming, serialization, localization, and custom control hosting. Master these techniques to build sophisticated navigation interfaces.

## Table of Contents

- [Nested GroupBar Support](#nested-groupbar-support)
- [In-Place Renaming of GroupBarItems](#in-place-renaming-of-groupbaritems)
- [Serialization of Layout State](#serialization-of-layout-state)
- [Localization Support](#localization-support)
- [Link Selection in GroupView](#link-selection-in-groupview)
- [Custom Control Hosting](#custom-control-hosting)
- [Event Handling](#event-handling)
- [AllowCollapse Property](#allowcollapse-property)
- [Complete Advanced Scenarios](#complete-advanced-scenarios)

## Nested GroupBar Support

GroupBar controls can be nested within each other, creating hierarchical navigation structures.

### Creating Nested GroupBars

A nested GroupBar is simply a GroupBar control hosted as the client of a GroupBarItem.

```csharp
// Parent GroupBar
GroupBar parentGroupBar = new GroupBar
{
    Dock = DockStyle.Left,
    Width = 220
};

// Child GroupBar (nested)
GroupBar childGroupBar = new GroupBar
{
    Dock = DockStyle.Fill
};

// Add items to child GroupBar
childGroupBar.GroupBarItems.AddRange(new GroupBarItem[]
{
    new GroupBarItem { Text = "Nested Item 1" },
    new GroupBarItem { Text = "Nested Item 2" },
    new GroupBarItem { Text = "Nested Item 3" }
});

// Create parent item to host child GroupBar
GroupBarItem parentItem = new GroupBarItem
{
    Text = "Nested Navigation",
    Client = childGroupBar
};

// Add child GroupBar to parent's controls
parentGroupBar.Controls.Add(childGroupBar);
parentGroupBar.GroupBarItems.Add(parentItem);

// Add parent to form
this.Controls.Add(parentGroupBar);
```

**When to use nested GroupBars:**
- Multi-level navigation hierarchies
- Sub-categories within categories
- Department → Team → Project navigation
- Document management with deep folder structures

### Complete Nested GroupBar Example

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class NestedGroupBarForm : Form
{
    private GroupBar mainGroupBar;

    public NestedGroupBarForm()
    {
        this.Text = "Nested GroupBar Example";
        this.Size = new Size(900, 700);
        
        CreateNestedNavigation();
    }

    private void CreateNestedNavigation()
    {
        // Create main GroupBar
        this.mainGroupBar = new GroupBar
        {
            Dock = DockStyle.Left,
            Width = 240,
            BorderStyle = BorderStyle.Fixed3D,
            Font = new Font("Segoe UI", 9F)
        };
        
        // Create standard sections
        CreateStandardSection("Dashboard");
        CreateStandardSection("Reports");
        
        // Create nested section
        CreateNestedSection();
        
        // More standard sections
        CreateStandardSection("Settings");
        
        this.mainGroupBar.SelectedItem = 0;
        this.Controls.Add(this.mainGroupBar);
    }

    private void CreateStandardSection(string name)
    {
        GroupBarItem item = new GroupBarItem { Text = name };
        
        Panel panel = new Panel
        {
            Dock = DockStyle.Fill,
            BackColor = Color.White,
            Padding = new Padding(10)
        };
        
        Label label = new Label
        {
            Text = $"{name} Content",
            Dock = DockStyle.Top,
            Font = new Font("Segoe UI", 12F, FontStyle.Bold)
        };
        
        panel.Controls.Add(label);
        item.Client = panel;
        this.mainGroupBar.Controls.Add(panel);
        this.mainGroupBar.GroupBarItems.Add(item);
    }

    private void CreateNestedSection()
    {
        GroupBarItem parentItem = new GroupBarItem
        {
            Text = "Organization"
        };
        
        // Create nested GroupBar
        GroupBar nestedGroupBar = new GroupBar
        {
            Dock = DockStyle.Fill,
            BorderStyle = BorderStyle.None,
            Font = new Font("Segoe UI", 8.5F)
        };
        
        // Add departments with GroupViews
        CreateDepartmentSection(nestedGroupBar, "Sales", new string[]
        {
            "North Region",
            "South Region",
            "East Region",
            "West Region"
        });
        
        CreateDepartmentSection(nestedGroupBar, "Marketing", new string[]
        {
            "Digital Marketing",
            "Content Team",
            "Social Media",
            "Analytics"
        });
        
        CreateDepartmentSection(nestedGroupBar, "Engineering", new string[]
        {
            "Frontend Team",
            "Backend Team",
            "DevOps",
            "QA Team"
        });
        
        // Assign nested GroupBar as client
        parentItem.Client = nestedGroupBar;
        this.mainGroupBar.Controls.Add(nestedGroupBar);
        this.mainGroupBar.GroupBarItems.Add(parentItem);
    }

    private void CreateDepartmentSection(GroupBar parentBar, string deptName, string[] teams)
    {
        GroupBarItem deptItem = new GroupBarItem { Text = deptName };
        
        GroupView teamView = new GroupView { Name = $"{deptName}View" };
        
        foreach (string team in teams)
        {
            teamView.GroupViewItems.Add(new GroupViewItem(team, -1, true, null, team));
        }
        
        teamView.GroupViewItemSelected += (s, e) =>
        {
            GroupView view = s as GroupView;
            if (view != null && view.SelectedItem >= 0)
            {
                string selected = view.GroupViewItems[view.SelectedItem].Text;
                MessageBox.Show($"Selected: {deptName} - {selected}", "Team Selection");
            }
        };
        
        deptItem.Client = teamView;
        parentBar.Controls.Add(teamView);
        parentBar.GroupBarItems.Add(deptItem);
    }
}
```

**Result:** A three-level navigation structure: Main sections → Departments → Teams.

## In-Place Renaming of GroupBarItems

Allow users to rename GroupBarItems at runtime.

### Enabling In-Place Renaming

```csharp
// Trigger rename for specific item
private void RenameItem(int itemIndex)
{
    if (itemIndex >= 0 && itemIndex < this.groupBar1.GroupBarItems.Count)
    {
        this.groupBar1.InplaceRenameItem(itemIndex);
    }
}

// Cancel rename operation
private void CancelRename()
{
    this.groupBar1.CancelInplaceRenameItem();
}
```

### Handling Rename Events

```csharp
// Wire up rename event
this.groupBar1.GroupBarItemRenamed += GroupBar1_GroupBarItemRenamed;

private void GroupBar1_GroupBarItemRenamed(object sender, 
    Syncfusion.Windows.Forms.Tools.GroupItemRenamedEventArgs e)
{
    int itemIndex = e.Index;
    string oldName = e.OldLabel;
    string newName = e.NewLabel;
    
    Console.WriteLine($"Item {itemIndex} renamed from '{oldName}' to '{newName}'");
    
    // Validate new name
    if (string.IsNullOrWhiteSpace(newName))
    {
        MessageBox.Show("Item name cannot be empty.", "Invalid Name", 
            MessageBoxButtons.OK, MessageBoxIcon.Warning);
        
        // Revert to old name
        this.groupBar1.GroupBarItems[itemIndex].Text = oldName;
        return;
    }
    
    // Check for duplicates
    foreach (GroupBarItem item in this.groupBar1.GroupBarItems)
    {
        if (item.Text == newName && this.groupBar1.GroupBarItems.IndexOf(item) != itemIndex)
        {
            MessageBox.Show("An item with this name already exists.", "Duplicate Name",
                MessageBoxButtons.OK, MessageBoxIcon.Warning);
            
            // Revert to old name
            this.groupBar1.GroupBarItems[itemIndex].Text = oldName;
            return;
        }
    }
    
    // Save renamed item to database or config
    SaveItemName(itemIndex, newName);
}

private void SaveItemName(int index, string name)
{
    // Save to database or configuration file
    Console.WriteLine($"Saving: Item {index} = {name}");
}
```

### Complete Rename Example

```csharp
public class RenameableNavigationForm : Form
{
    private GroupBar groupBar1;
    private ContextMenuStrip itemContextMenu;

    public RenameableNavigationForm()
    {
        this.Text = "Renameable Navigation";
        this.Size = new Size(800, 600);
        
        SetupRenameableGroupBar();
    }

    private void SetupRenameableGroupBar()
    {
        this.groupBar1 = new GroupBar
        {
            Dock = DockStyle.Left,
            Width = 220,
            Font = new Font("Segoe UI", 9F)
        };
        
        // Create items
        for (int i = 1; i <= 5; i++)
        {
            GroupBarItem item = new GroupBarItem
            {
                Text = $"Category {i}"
            };
            
            Panel panel = new Panel { Dock = DockStyle.Fill, BackColor = Color.White };
            item.Client = panel;
            this.groupBar1.Controls.Add(panel);
            this.groupBar1.GroupBarItems.Add(item);
        }
        
        // Handle rename event
        this.groupBar1.GroupBarItemRenamed += GroupBar1_ItemRenamed;
        
        // Create context menu for rename
        CreateRenameContextMenu();
        
        // Enable right-click on items
        this.groupBar1.MouseClick += GroupBar1_MouseClick;
        
        this.Controls.Add(this.groupBar1);
    }

    private void CreateRenameContextMenu()
    {
        this.itemContextMenu = new ContextMenuStrip();
        
        ToolStripMenuItem renameItem = new ToolStripMenuItem("Rename");
        renameItem.Click += (s, e) =>
        {
            // Rename currently selected item
            int selectedIndex = this.groupBar1.SelectedItem;
            if (selectedIndex >= 0)
            {
                this.groupBar1.InplaceRenameItem(selectedIndex);
            }
        };
        
        this.itemContextMenu.Items.Add(renameItem);
    }

    private void GroupBar1_MouseClick(object sender, MouseEventArgs e)
    {
        if (e.Button == MouseButtons.Right)
        {
            // Show context menu
            this.itemContextMenu.Show(this.groupBar1, e.Location);
        }
    }

    private void GroupBar1_ItemRenamed(object sender, GroupItemRenamedEventArgs e)
    {
        string newName = e.NewLabel;
        
        // Validate
        if (string.IsNullOrWhiteSpace(newName))
        {
            MessageBox.Show("Name cannot be empty.");
            this.groupBar1.GroupBarItems[e.Index].Text = e.OldLabel;
            return;
        }
        
        // Check length
        if (newName.Length > 50)
        {
            MessageBox.Show("Name is too long (max 50 characters).");
            this.groupBar1.GroupBarItems[e.Index].Text = e.OldLabel;
            return;
        }
        
        Console.WriteLine($"Renamed: '{e.OldLabel}' → '{newName}'");
    }
}
```

## Serialization of Layout State

Save and restore the GroupBar's layout configuration.

### Saving Layout State

```csharp
using Syncfusion.Runtime.Serialization;

private void SaveGroupBarLayout()
{
    // Create storage object
    ArrayList layoutData = new ArrayList();
    
    // Save navigation pane items (for stacked mode)
    foreach (GroupBarItem item in this.groupBar1.GroupBarItems)
    {
        if (item.InNavigationPane)
        {
            int index = this.groupBar1.GroupBarItems.IndexOf(item);
            layoutData.Add(index);
        }
    }
    
    // Save selected item
    layoutData.Add(this.groupBar1.SelectedItem);
    
    // Save collapsed state
    layoutData.Add(this.groupBar1.Collapsed);
    
    // Save stacked mode
    layoutData.Add(this.groupBar1.StackedMode);
    
    // Save dimensions
    layoutData.Add(this.groupBar1.Width);
    
    // Serialize to XML file
    string configPath = GetConfigFilePath();
    Directory.CreateDirectory(Path.GetDirectoryName(configPath));
    
    AppStateSerializer serializer = new AppStateSerializer(
        SerializeMode.XMLFile,
        configPath
    );
    
    serializer.SerializeObject("GroupBarLayout", layoutData);
    serializer.PersistNow();
    
    Console.WriteLine($"Layout saved to: {configPath}");
}

private string GetConfigFilePath()
{
    string appData = Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData);
    return Path.Combine(appData, "MyApplication", "GroupBarLayout.xml");
}
```

### Loading Layout State

```csharp
private void LoadGroupBarLayout()
{
    try
    {
        string configPath = GetConfigFilePath();
        
        if (!File.Exists(configPath))
        {
            Console.WriteLine("No saved layout found");
            return;
        }
        
        // Deserialize from XML
        AppStateSerializer serializer = new AppStateSerializer(
            SerializeMode.XMLFile,
            configPath
        );
        
        ArrayList layoutData = serializer.DeserializeObject("GroupBarLayout") as ArrayList;
        
        if (layoutData == null || layoutData.Count < 5)
        {
            Console.WriteLine("Invalid layout data");
            return;
        }
        
        // Restore stacked mode (index count - 3)
        bool stackedMode = (bool)layoutData[layoutData.Count - 2];
        this.groupBar1.StackedMode = stackedMode;
        
        // Restore width
        int width = (int)layoutData[layoutData.Count - 1];
        this.groupBar1.Width = width;
        
        // Reset navigation pane items
        foreach (GroupBarItem item in this.groupBar1.GroupBarItems)
        {
            item.InNavigationPane = false;
        }
        
        // Restore navigation pane items
        int navItemCount = layoutData.Count - 4; // Exclude selectedItem, collapsed, stacked, width
        for (int i = 0; i < navItemCount; i++)
        {
            int itemIndex = (int)layoutData[i];
            if (itemIndex >= 0 && itemIndex < this.groupBar1.GroupBarItems.Count)
            {
                this.groupBar1.GroupBarItems[itemIndex].InNavigationPane = true;
            }
        }
        
        // Restore selected item
        int selectedItem = (int)layoutData[layoutData.Count - 4];
        if (selectedItem >= 0 && selectedItem < this.groupBar1.GroupBarItems.Count)
        {
            this.groupBar1.SelectedItem = selectedItem;
        }
        
        // Restore collapsed state
        bool collapsed = (bool)layoutData[layoutData.Count - 3];
        this.groupBar1.Collapsed = collapsed;
        
        Console.WriteLine("Layout loaded successfully");
    }
    catch (Exception ex)
    {
        Console.WriteLine($"Error loading layout: {ex.Message}");
    }
}
```

### Auto-Save/Load Pattern

```csharp
private void Form1_Load(object sender, EventArgs e)
{
    // Load layout on startup
    LoadGroupBarLayout();
}

private void Form1_FormClosing(object sender, FormClosingEventArgs e)
{
    // Save layout on exit
    SaveGroupBarLayout();
}
```

## Localization Support

Localize GroupBar text and tooltips for international users.

### Resource-Based Localization

```csharp
// Create resource files: Strings.resx, Strings.fr-FR.resx, Strings.es-ES.resx

private void ApplyLocalization(string cultureName)
{
    // Set culture
    System.Threading.Thread.CurrentThread.CurrentUICulture = 
        new System.Globalization.CultureInfo(cultureName);
    
    // Update item texts from resources
    this.groupBarItem1.Text = Properties.Resources.Navigation_Mail;
    this.groupBarItem2.Text = Properties.Resources.Navigation_Calendar;
    this.groupBarItem3.Text = Properties.Resources.Navigation_Contacts;
    this.groupBarItem4.Text = Properties.Resources.Navigation_Tasks;
    
    // Update tooltips
    this.groupBar1.NavigationPaneTooltip = Properties.Resources.Tooltip_NavigationPane;
    this.groupBar1.ExpandButtonToolTip = Properties.Resources.Tooltip_Expand;
    this.groupBar1.MinimizeButtonToolTip = Properties.Resources.Tooltip_Minimize;
    
    Console.WriteLine($"Localized to: {cultureName}");
}
```

### Complete Localization Example

```csharp
public class LocalizedGroupBarForm : Form
{
    private GroupBar groupBar1;
    private ComboBox languageSelector;

    public LocalizedGroupBarForm()
    {
        this.Text = "Localized Navigation";
        this.Size = new Size(800, 600);
        
        CreateLanguageSelector();
        CreateLocalizedGroupBar();
    }

    private void CreateLanguageSelector()
    {
        this.languageSelector = new ComboBox
        {
            Dock = DockStyle.Top,
            DropDownStyle = ComboBoxStyle.DropDownList
        };
        
        this.languageSelector.Items.AddRange(new object[]
        {
            "English (en-US)",
            "French (fr-FR)",
            "Spanish (es-ES)",
            "German (de-DE)"
        });
        
        this.languageSelector.SelectedIndexChanged += (s, e) =>
        {
            string culture = ExtractCulture(this.languageSelector.SelectedItem.ToString());
            ApplyLocalization(culture);
        };
        
        this.Controls.Add(this.languageSelector);
        this.languageSelector.SelectedIndex = 0;
    }

    private void CreateLocalizedGroupBar()
    {
        this.groupBar1 = new GroupBar
        {
            Dock = DockStyle.Left,
            Width = 220
        };
        
        // Create items (texts will be set by localization)
        for (int i = 0; i < 4; i++)
        {
            GroupBarItem item = new GroupBarItem();
            Panel panel = new Panel { Dock = DockStyle.Fill, BackColor = Color.White };
            item.Client = panel;
            this.groupBar1.Controls.Add(panel);
            this.groupBar1.GroupBarItems.Add(item);
        }
        
        this.Controls.Add(this.groupBar1);
    }

    private string ExtractCulture(string text)
    {
        // Extract culture code from display text
        int start = text.IndexOf('(') + 1;
        int end = text.IndexOf(')');
        return text.Substring(start, end - start);
    }

    private void ApplyLocalization(string cultureName)
    {
        // In real application, use resource files
        // For demo, use hardcoded translations
        
        Dictionary<string, string[]> translations = new Dictionary<string, string[]>
        {
            { "en-US", new[] { "Mail", "Calendar", "Contacts", "Tasks" } },
            { "fr-FR", new[] { "Courrier", "Calendrier", "Contacts", "Tâches" } },
            { "es-ES", new[] { "Correo", "Calendario", "Contactos", "Tareas" } },
            { "de-DE", new[] { "E-Mail", "Kalender", "Kontakte", "Aufgaben" } }
        };
        
        if (translations.ContainsKey(cultureName))
        {
            string[] labels = translations[cultureName];
            for (int i = 0; i < labels.Length && i < this.groupBar1.GroupBarItems.Count; i++)
            {
                this.groupBar1.GroupBarItems[i].Text = labels[i];
            }
        }
        
        this.Text = $"Localized Navigation - {cultureName}";
    }
}
```

## Link Selection in GroupView

Configure how items are selected in GroupView controls.

### Single vs Multiple Selection

```csharp
// Single selection (default)
// Only one item can be selected at a time

// Multiple selection
groupView.MultiSelect = true; // If supported by version
```

### Programmatic Selection

```csharp
// Select item by index
groupView.SelectedItem = 2;

// Clear selection
groupView.SelectedItem = -1;
```

## Custom Control Hosting

Host any .NET control as a GroupBarItem client.

### Hosting DataGridView

```csharp
private void HostDataGridView()
{
    GroupBarItem dataItem = new GroupBarItem
    {
        Text = "Data View"
    };
    
    DataGridView grid = new DataGridView
    {
        Dock = DockStyle.Fill,
        AutoGenerateColumns = true,
        AllowUserToAddRows = false
    };
    
    // Populate with data
    DataTable table = CreateSampleData();
    grid.DataSource = table;
    
    dataItem.Client = grid;
    this.groupBar1.Controls.Add(grid);
    this.groupBar1.GroupBarItems.Add(dataItem);
}

private DataTable CreateSampleData()
{
    DataTable table = new DataTable();
    table.Columns.Add("ID", typeof(int));
    table.Columns.Add("Name", typeof(string));
    table.Columns.Add("Value", typeof(decimal));
    
    table.Rows.Add(1, "Item 1", 100.50m);
    table.Rows.Add(2, "Item 2", 250.75m);
    table.Rows.Add(3, "Item 3", 175.25m);
    
    return table;
}
```

### Hosting TreeView

```csharp
private void HostTreeView()
{
    GroupBarItem treeItem = new GroupBarItem
    {
        Text = "Folder Tree"
    };
    
    TreeView tree = new TreeView
    {
        Dock = DockStyle.Fill,
        BorderStyle = BorderStyle.None
    };
    
    // Build tree structure
    TreeNode root = new TreeNode("Documents");
    root.Nodes.Add("Personal");
    root.Nodes.Add("Work");
    root.Nodes.Add("Projects");
    
    tree.Nodes.Add(root);
    tree.ExpandAll();
    
    treeItem.Client = tree;
    this.groupBar1.Controls.Add(tree);
    this.groupBar1.GroupBarItems.Add(treeItem);
}
```

### Hosting Custom UserControl

```csharp
// Assuming you have a custom UserControl
public class DashboardControl : UserControl
{
    public DashboardControl()
    {
        // Custom dashboard implementation
    }
}

private void HostCustomUserControl()
{
    GroupBarItem dashboardItem = new GroupBarItem
    {
        Text = "Dashboard"
    };
    
    DashboardControl dashboard = new DashboardControl
    {
        Dock = DockStyle.Fill
    };
    
    dashboardItem.Client = dashboard;
    this.groupBar1.Controls.Add(dashboard);
    this.groupBar1.GroupBarItems.Add(dashboardItem);
}
```

## Event Handling

Comprehensive event handling for advanced scenarios.

### GroupBarItemAdded Event

```csharp
this.groupBar1.GroupBarItemAdded += (s, e) =>
{
    GroupBarItem addedItem = e.Item;
    Console.WriteLine($"Item added: {addedItem.Text}");
    
    // Initialize new item
    if (addedItem.Client == null)
    {
        Panel panel = new Panel { Dock = DockStyle.Fill, BackColor = Color.White };
        addedItem.Client = panel;
        this.groupBar1.Controls.Add(panel);
    }
};
```

### GroupBarItemRemoved Event

```csharp
this.groupBar1.GroupBarItemRemoved += (s, e) =>
{
    GroupBarItem removedItem = e.Item;
    Console.WriteLine($"Item removed: {removedItem.Text}");
    
    // Cleanup client control
    if (removedItem.Client != null)
    {
        this.groupBar1.Controls.Remove(removedItem.Client);
        removedItem.Client.Dispose();
    }
};
```

### GroupBarItemSelectionChanging Event

```csharp
this.groupBar1.GroupBarItemSelectionChanging += (s, e) =>
{
    int oldIndex = e.OldSelected;
    int newIndex = e.NewSelected;
    
    Console.WriteLine($"Changing selection: {oldIndex} → {newIndex}");
    
    // Validate before allowing change
    if (HasUnsavedChanges())
    {
        DialogResult result = MessageBox.Show(
            "You have unsaved changes. Continue?",
            "Unsaved Changes",
            MessageBoxButtons.YesNo);
        
        if (result == DialogResult.No)
        {
            e.Cancel = true; // Prevent selection change
        }
    }
};

private bool HasUnsavedChanges()
{
    // Check for unsaved data
    return false; // Placeholder
}
```

### ShowContextMenu Event

```csharp
this.groupBar1.ShowContextMenu += (s, e) =>
{
    Console.WriteLine("Context menu requested");
    
    // Show custom context menu
    ContextMenuStrip menu = new ContextMenuStrip();
    menu.Items.Add("Add Item", null, (sender, args) => AddNewItem());
    menu.Items.Add("Remove Item", null, (sender, args) => RemoveSelectedItem());
    menu.Items.Add(new ToolStripSeparator());
    menu.Items.Add("Settings", null, (sender, args) => ShowSettings());
    
    menu.Show(this.groupBar1, this.groupBar1.PointToClient(Cursor.Position));
};

private void AddNewItem()
{
    GroupBarItem newItem = new GroupBarItem { Text = $"New Item {DateTime.Now.Ticks}" };
    Panel panel = new Panel { Dock = DockStyle.Fill, BackColor = Color.White };
    newItem.Client = panel;
    this.groupBar1.Controls.Add(panel);
    this.groupBar1.GroupBarItems.Add(newItem);
}

private void RemoveSelectedItem()
{
    int selectedIndex = this.groupBar1.SelectedItem;
    if (selectedIndex >= 0)
    {
        this.groupBar1.GroupBarItems.RemoveAt(selectedIndex);
    }
}

private void ShowSettings()
{
    MessageBox.Show("Settings dialog would appear here.");
}
```

## AllowCollapse Property

Control whether users can collapse the navigation pane.

```csharp
// Enable collapsing
this.groupBar1.AllowCollapse = true;

// Configure collapse appearance
this.groupBar1.CollapsedWidth = 40;
this.groupBar1.CollapsedText = "Nav";

// Set custom collapse/expand images
this.groupBar1.CollapseImage = Properties.Resources.CollapseIcon;
this.groupBar1.ExpandImage = Properties.Resources.ExpandIcon;

// Handle collapse state changes
this.groupBar1.CollapsedChanged += (s, e) =>
{
    bool isCollapsed = this.groupBar1.Collapsed;
    Console.WriteLine($"Navigation pane {(isCollapsed ? "collapsed" : "expanded")}");
    
    // Adjust layout accordingly
    AdjustContentLayout(isCollapsed);
};
```

## Complete Advanced Scenarios

### Scenario 1: Dynamic Item Management with Serialization

```csharp
public class DynamicItemManagementForm : Form
{
    private GroupBar groupBar1;
    private ToolStrip toolbar;

    public DynamicItemManagementForm()
    {
        this.Text = "Dynamic Item Management";
        this.Size = new Size(900, 700);
        
        CreateToolbar();
        CreateDynamicGroupBar();
        
        this.Load += Form_Load;
        this.FormClosing += Form_FormClosing;
    }

    private void CreateToolbar()
    {
        this.toolbar = new ToolStrip();
        
        ToolStripButton btnAdd = new ToolStripButton("Add Section");
        btnAdd.Click += (s, e) => AddNewSection();
        
        ToolStripButton btnRemove = new ToolStripButton("Remove Section");
        btnRemove.Click += (s, e) => RemoveCurrentSection();
        
        ToolStripButton btnRename = new ToolStripButton("Rename Section");
        btnRename.Click += (s, e) => RenameCurrentSection();
        
        ToolStripButton btnSave = new ToolStripButton("Save Layout");
        btnSave.Click += (s, e) => SaveLayout();
        
        this.toolbar.Items.AddRange(new ToolStripItem[] 
        { 
            btnAdd, btnRemove, btnRename, 
            new ToolStripSeparator(), 
            btnSave 
        });
        
        this.Controls.Add(this.toolbar);
    }

    private void CreateDynamicGroupBar()
    {
        this.groupBar1 = new GroupBar
        {
            Dock = DockStyle.Left,
            Width = 220,
            Font = new Font("Segoe UI", 9F)
        };
        
        // Handle events
        this.groupBar1.GroupBarItemAdded += GroupBar1_ItemAdded;
        this.groupBar1.GroupBarItemRemoved += GroupBar1_ItemRemoved;
        this.groupBar1.GroupBarItemRenamed += GroupBar1_ItemRenamed;
        
        this.Controls.Add(this.groupBar1);
    }

    private void AddNewSection()
    {
        string name = Prompt.ShowDialog("Enter section name:", "New Section");
        if (string.IsNullOrWhiteSpace(name))
            return;
        
        GroupBarItem item = new GroupBarItem { Text = name };
        
        Panel panel = new Panel
        {
            Dock = DockStyle.Fill,
            BackColor = Color.White,
            Padding = new Padding(10)
        };
        
        Label label = new Label
        {
            Text = $"Content for {name}",
            Dock = DockStyle.Top,
            Font = new Font("Segoe UI", 12F)
        };
        
        panel.Controls.Add(label);
        item.Client = panel;
        this.groupBar1.Controls.Add(panel);
        this.groupBar1.GroupBarItems.Add(item);
        
        // Select new item
        this.groupBar1.SelectedItem = this.groupBar1.GroupBarItems.Count - 1;
    }

    private void RemoveCurrentSection()
    {
        int selectedIndex = this.groupBar1.SelectedItem;
        if (selectedIndex < 0)
        {
            MessageBox.Show("No section selected.");
            return;
        }
        
        string itemName = this.groupBar1.GroupBarItems[selectedIndex].Text;
        DialogResult result = MessageBox.Show(
            $"Remove section '{itemName}'?",
            "Confirm Removal",
            MessageBoxButtons.YesNo,
            MessageBoxIcon.Question);
        
        if (result == DialogResult.Yes)
        {
            this.groupBar1.GroupBarItems.RemoveAt(selectedIndex);
        }
    }

    private void RenameCurrentSection()
    {
        int selectedIndex = this.groupBar1.SelectedItem;
        if (selectedIndex >= 0)
        {
            this.groupBar1.InplaceRenameItem(selectedIndex);
        }
    }

    private void SaveLayout()
    {
        // Save item names and order
        ArrayList layoutData = new ArrayList();
        
        foreach (GroupBarItem item in this.groupBar1.GroupBarItems)
        {
            layoutData.Add(item.Text);
        }
        
        layoutData.Add(this.groupBar1.SelectedItem);
        
        string configPath = Path.Combine(
            Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData),
            "DynamicGroupBar",
            "layout.xml");
        
        Directory.CreateDirectory(Path.GetDirectoryName(configPath));
        
        AppStateSerializer serializer = new AppStateSerializer(
            SerializeMode.XMLFile,
            configPath);
        
        serializer.SerializeObject("Layout", layoutData);
        serializer.PersistNow();
        
        MessageBox.Show("Layout saved!", "Success", MessageBoxButtons.OK, MessageBoxIcon.Information);
    }

    private void LoadLayout()
    {
        try
        {
            string configPath = Path.Combine(
                Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData),
                "DynamicGroupBar",
                "layout.xml");
            
            if (!File.Exists(configPath))
                return;
            
            AppStateSerializer serializer = new AppStateSerializer(
                SerializeMode.XMLFile,
                configPath);
            
            ArrayList layoutData = serializer.DeserializeObject("Layout") as ArrayList;
            if (layoutData == null || layoutData.Count == 0)
                return;
            
            // Recreate items
            this.groupBar1.GroupBarItems.Clear();
            
            for (int i = 0; i < layoutData.Count - 1; i++)
            {
                string itemName = layoutData[i].ToString();
                
                GroupBarItem item = new GroupBarItem { Text = itemName };
                Panel panel = new Panel { Dock = DockStyle.Fill, BackColor = Color.White };
                item.Client = panel;
                this.groupBar1.Controls.Add(panel);
                this.groupBar1.GroupBarItems.Add(item);
            }
            
            // Restore selection
            int selectedIndex = (int)layoutData[layoutData.Count - 1];
            if (selectedIndex >= 0 && selectedIndex < this.groupBar1.GroupBarItems.Count)
            {
                this.groupBar1.SelectedItem = selectedIndex;
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error loading layout: {ex.Message}");
        }
    }

    private void GroupBar1_ItemAdded(object sender, GroupBarItemEventArgs e)
    {
        Console.WriteLine($"Item added: {e.Item.Text}");
    }

    private void GroupBar1_ItemRemoved(object sender, GroupBarItemEventArgs e)
    {
        Console.WriteLine($"Item removed: {e.Item.Text}");
    }

    private void GroupBar1_ItemRenamed(object sender, GroupItemRenamedEventArgs e)
    {
        Console.WriteLine($"Item renamed: {e.OldLabel} → {e.NewLabel}");
    }

    private void Form_Load(object sender, EventArgs e)
    {
        LoadLayout();
    }

    private void Form_FormClosing(object sender, FormClosingEventArgs e)
    {
        SaveLayout();
    }
}

// Simple prompt dialog helper
public static class Prompt
{
    public static string ShowDialog(string text, string caption)
    {
        Form prompt = new Form
        {
            Width = 400,
            Height = 150,
            FormBorderStyle = FormBorderStyle.FixedDialog,
            Text = caption,
            StartPosition = FormStartPosition.CenterScreen
        };
        
        Label textLabel = new Label { Left = 20, Top = 20, Text = text, Width = 350 };
        TextBox textBox = new TextBox { Left = 20, Top = 50, Width = 350 };
        Button confirmation = new Button { Text = "Ok", Left = 250, Width = 100, Top = 80, DialogResult = DialogResult.OK };
        
        confirmation.Click += (sender, e) => { prompt.Close(); };
        
        prompt.Controls.AddRange(new Control[] { textLabel, textBox, confirmation });
        prompt.AcceptButton = confirmation;
        
        return prompt.ShowDialog() == DialogResult.OK ? textBox.Text : string.Empty;
    }
}
```

**Result:** Complete dynamic item management with add, remove, rename, and layout persistence.

### Scenario 2: Custom Control Hosting Showcase

```csharp
public class CustomControlShowcaseForm : Form
{
    private GroupBar groupBar1;

    public CustomControlShowcaseForm()
    {
        this.Text = "Custom Control Showcase";
        this.Size = new Size(1100, 750);
        
        CreateShowcaseGroupBar();
    }

    private void CreateShowcaseGroupBar()
    {
        this.groupBar1 = new GroupBar
        {
            Dock = DockStyle.Left,
            Width = 220,
            Font = new Font("Segoe UI", 9F),
            StackedMode = true
        };
        
        // Host different control types
        HostDataGridSection();
        HostChartSection();
        HostCalendarSection();
        HostBrowserSection();
        HostRichTextSection();
        
        this.groupBar1.SelectedItem = 0;
        this.Controls.Add(this.groupBar1);
    }

    private void HostDataGridSection()
    {
        GroupBarItem item = new GroupBarItem { Text = "Data Grid" };
        
        DataGridView grid = new DataGridView
        {
            Dock = DockStyle.Fill,
            AutoGenerateColumns = true,
            AllowUserToAddRows = false,
            SelectionMode = DataGridViewSelectionMode.FullRowSelect
        };
        
        // Sample data
        DataTable table = new DataTable();
        table.Columns.Add("Product", typeof(string));
        table.Columns.Add("Price", typeof(decimal));
        table.Columns.Add("Stock", typeof(int));
        
        table.Rows.Add("Laptop", 1299.99m, 45);
        table.Rows.Add("Mouse", 29.99m, 150);
        table.Rows.Add("Keyboard", 89.99m, 78);
        
        grid.DataSource = table;
        
        item.Client = grid;
        this.groupBar1.Controls.Add(grid);
        this.groupBar1.GroupBarItems.Add(item);
    }

    private void HostChartSection()
    {
        GroupBarItem item = new GroupBarItem { Text = "Charts" };
        
        // Simple chart using panel and graphics
        Panel chartPanel = new Panel
        {
            Dock = DockStyle.Fill,
            BackColor = Color.White
        };
        
        chartPanel.Paint += (s, e) =>
        {
            Graphics g = e.Graphics;
            g.SmoothingMode = System.Drawing.Drawing2D.SmoothingMode.AntiAlias;
            
            // Draw simple bar chart
            int[] values = { 45, 67, 89, 54, 72 };
            int barWidth = 50;
            int spacing = 20;
            int maxHeight = chartPanel.Height - 50;
            
            for (int i = 0; i < values.Length; i++)
            {
                int x = 50 + i * (barWidth + spacing);
                int height = (int)(values[i] / 100.0 * maxHeight);
                int y = chartPanel.Height - height - 30;
                
                g.FillRectangle(Brushes.SteelBlue, x, y, barWidth, height);
                g.DrawString(values[i].ToString(), this.Font, Brushes.Black, x + 15, y - 20);
            }
        };
        
        item.Client = chartPanel;
        this.groupBar1.Controls.Add(chartPanel);
        this.groupBar1.GroupBarItems.Add(item);
    }

    private void HostCalendarSection()
    {
        GroupBarItem item = new GroupBarItem { Text = "Calendar" };
        
        MonthCalendar calendar = new MonthCalendar
        {
            Location = new Point(10, 10),
            MaxSelectionCount = 1
        };
        
        Panel calendarPanel = new Panel
        {
            Dock = DockStyle.Fill,
            BackColor = Color.White,
            Padding = new Padding(10)
        };
        
        calendarPanel.Controls.Add(calendar);
        
        item.Client = calendarPanel;
        this.groupBar1.Controls.Add(calendarPanel);
        this.groupBar1.GroupBarItems.Add(item);
    }

    private void HostBrowserSection()
    {
        GroupBarItem item = new GroupBarItem { Text = "Web Browser" };
        
        WebBrowser browser = new WebBrowser
        {
            Dock = DockStyle.Fill,
            Url = new Uri("about:blank")
        };
        
        // Simple HTML content
        browser.DocumentText = @"
            <html>
            <body style='font-family: Segoe UI; padding: 20px;'>
                <h1>Embedded Web Browser</h1>
                <p>This is a WebBrowser control hosted in a GroupBarItem.</p>
                <p>You can navigate to any URL or display custom HTML content.</p>
            </body>
            </html>";
        
        item.Client = browser;
        this.groupBar1.Controls.Add(browser);
        this.groupBar1.GroupBarItems.Add(item);
    }

    private void HostRichTextSection()
    {
        GroupBarItem item = new GroupBarItem { Text = "Rich Text" };
        
        RichTextBox richText = new RichTextBox
        {
            Dock = DockStyle.Fill,
            BorderStyle = BorderStyle.None,
            Font = new Font("Segoe UI", 10F)
        };
        
        richText.SelectionFont = new Font("Segoe UI", 16F, FontStyle.Bold);
        richText.AppendText("Rich Text Editor\n\n");
        
        richText.SelectionFont = new Font("Segoe UI", 10F, FontStyle.Regular);
        richText.AppendText("This is a RichTextBox control with formatted text.\n\n");
        
        richText.SelectionColor = Color.Blue;
        richText.AppendText("You can apply different colors, ");
        
        richText.SelectionColor = Color.Red;
        richText.SelectionFont = new Font("Segoe UI", 10F, FontStyle.Bold);
        richText.AppendText("fonts, ");
        
        richText.SelectionColor = Color.Green;
        richText.SelectionFont = new Font("Segoe UI", 10F, FontStyle.Italic);
        richText.AppendText("and styles ");
        
        richText.SelectionColor = Color.Black;
        richText.SelectionFont = new Font("Segoe UI", 10F, FontStyle.Regular);
        richText.AppendText("to the text.");
        
        item.Client = richText;
        this.groupBar1.Controls.Add(richText);
        this.groupBar1.GroupBarItems.Add(item);
    }
}
```

**Result:** Showcase of various controls hosted in GroupBarItems including DataGridView, charts, calendar, web browser, and rich text editor.

## Key Takeaways

1. **Nested GroupBars** enable multi-level navigation hierarchies
2. **In-Place Renaming** allows user customization with validation
3. **Serialization** preserves layout state across sessions
4. **Localization** supports international users with resource files
5. **Custom Control Hosting** accepts any .NET control as client
6. **Event Handling** provides hooks for advanced scenarios
7. **AllowCollapse** enables collapsible navigation panes
8. **Combine features** to create sophisticated applications
9. **Validate user input** in rename and event handlers
10. **Persist user preferences** for better UX
