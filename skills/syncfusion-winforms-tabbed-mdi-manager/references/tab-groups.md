# Tab Groups & Organization

## Table of Contents
- [What are Tab Groups](#what-are-tab-groups)
- [Creating Tab Groups](#creating-tab-groups)
- [Arranging Groups](#arranging-groups)
- [Managing Groups](#managing-groups)
- [Complete Examples](#complete-examples)

## What are Tab Groups

Tab Groups allow you to split your MDI container into multiple resizable areas, each with its own set of tabs. This is useful for:
- Comparing documents side-by-side
- Organizing related documents
- Creating complex application layouts (like Visual Studio)

### Single vs. Multiple Groups

**Single Tab Group (Default TabbedMDIManager):**
```
┌────────────────────────┐
│ Doc1 │ Doc2 │ Doc3 │   │
├────────────────────────┤
│      All documents     │
│    in one area         │
│                        │
└────────────────────────┘
```

**Multiple Tab Groups (TabbedGroupedMDIManager):**
```
┌────────────────────────┐
│ Doc1 │ Doc2            │
├────────────────────────┤
│   Group 1 Area    │    │
│                   │    │
│                   ├─ splitter
├─────────────────┼────┤
│ Doc3 │ Doc4     │Doc5│
│                 │    │
│  Group 2 Area   │G3  │
│                 │    │
└─────────────────┴────┘
```

## Creating Tab Groups

### Using TabbedGroupedMDIManager

For multiple tab groups, use `TabbedGroupedMDIManager` instead of `TabbedMDIManager`:

```csharp
using Syncfusion.Windows.Forms.Tools;

// Instead of:
// TabbedMDIManager tabbedMDI = new TabbedMDIManager();

// Use:
TabbedGroupedMDIManager tabbedMDI = new TabbedGroupedMDIManager();
tabbedMDI.AttachToMdiContainer(this);
```

### Basic Setup

```csharp
public partial class MainForm : Form
{
    private TabbedGroupedMDIManager tabbedMDIManager;

    public MainForm()
    {
        InitializeComponent();
        SetupMultiGroupMDI();
    }

    private void SetupMultiGroupMDI()
    {
        // Set MDI container
        this.IsMdiContainer = true;
        this.Text = "Multi-Group Tabbed MDI";

        // Create grouped MDI manager
        tabbedMDIManager = new TabbedGroupedMDIManager();
        this.Controls.Add(tabbedMDIManager);
        tabbedMDIManager.AttachToMdiContainer(this);

        // Enable themes
        tabbedMDIManager.ThemesEnabled = true;

        // Now create child forms - they'll create groups automatically
        CreateDocuments();
    }

    private void CreateDocuments()
    {
        // These will create separate tab groups
        for (int i = 1; i <= 3; i++)
        {
            Form childForm = new Form();
            childForm.Text = $"Document {i}";
            childForm.MdiParent = this;
            childForm.Show();
        }
    }
}
```

## Arranging Groups

### Horizontal vs. Vertical Arrangement

```csharp
// Horizontal arrangement (side-by-side)
tabbedMDIManager.Orientation = Orientation.Horizontal;
//
// ┌──────────┬──────────┐
// │ Group 1  │ Group 2  │
// │          │          │
// └──────────┴──────────┘

// Vertical arrangement (top-bottom)
tabbedMDIManager.Orientation = Orientation.Vertical;
//
// ┌──────────────────┐
// │ Group 1          │
// ├──────────────────┤
// │ Group 2          │
// └──────────────────┘
```

### Complete Orientation Example

```csharp
private void SetupOrientation()
{
    // Set to vertical arrangement
    tabbedMDIManager.Orientation = Orientation.Vertical;

    // Or use horizontal
    // tabbedMDIManager.Orientation = Orientation.Horizontal;
}
```

## Managing Groups Programmatically

### Get All Groups

```csharp
private void ListAllGroups()
{
    for (int i = 0; i < tabbedMDIManager.Groups.Count; i++)
    {
        TabControlAdv tabGroup = tabbedMDIManager.Groups[i] as TabControlAdv;
        if (tabGroup != null)
        {
            Console.WriteLine($"Group {i + 1}: {tabGroup.TabPages.Count} tabs");

            // List tabs in this group
            foreach (TabPage page in tabGroup.TabPages)
            {
                Console.WriteLine($"  - {page.Text}");
            }
        }
    }
}
```

### Assign Form to Specific Group

```csharp
private void AssignFormToGroup(Form childForm, int groupIndex)
{
    if (groupIndex < tabbedMDIManager.Groups.Count)
    {
        TabControlAdv targetGroup = tabbedMDIManager.Groups[groupIndex] as TabControlAdv;
        if (targetGroup != null)
        {
            // Create tab page for this form
            TabPage page = new TabPage(childForm.Text);
            page.Controls.Add(childForm);
            targetGroup.TabPages.Add(page);
        }
    }
}
```

### Get Active Group

```csharp
private void GetActiveGroup()
{
    // Get the currently active tab group
    foreach (TabControlAdv group in tabbedMDIManager.Groups)
    {
        if (group.SelectedTab != null)
        {
            Console.WriteLine($"Active group has {group.TabPages.Count} tabs");
            Console.WriteLine($"Active tab: {group.SelectedTab.Text}");
        }
    }
}
```

## Complete Examples

### Example 1: Horizontal Two-Panel Layout

```csharp
public partial class TwoPanelForm : Form
{
    private TabbedGroupedMDIManager tabbedMDI;

    public TwoPanelForm()
    {
        InitializeComponent();
        SetupTwoPanelLayout();
    }

    private void SetupTwoPanelLayout()
    {
        this.IsMdiContainer = true;
        this.Text = "Two-Panel Document Editor";
        this.Size = new Size(1200, 600);

        tabbedMDI = new TabbedGroupedMDIManager();
        this.Controls.Add(tabbedMDI);
        tabbedMDI.AttachToMdiContainer(this);

        // Side-by-side arrangement
        tabbedMDI.Orientation = Orientation.Horizontal;
        tabbedMDI.ThemesEnabled = true;

        // Create initial documents
        for (int i = 1; i <= 4; i++)
        {
            Form doc = new Form();
            doc.Text = $"Document {i}";
            doc.MdiParent = this;
            doc.Show();
        }

        // Documents are automatically split into groups
    }
}
```

### Example 2: Three-Way Split Layout

```csharp
public partial class ThreeWayForm : Form
{
    private TabbedGroupedMDIManager tabbedMDI;

    public ThreeWayForm()
    {
        InitializeComponent();
        SetupThreeWayLayout();
    }

    private void SetupThreeWayLayout()
    {
        this.IsMdiContainer = true;
        this.Text = "Three-Way Split Document View";

        tabbedMDI = new TabbedGroupedMDIManager();
        this.Controls.Add(tabbedMDI);
        tabbedMDI.AttachToMdiContainer(this);
        tabbedMDI.ThemesEnabled = true;

        // Create 3 document groups
        CreateDocumentGroup("Group 1", new[] { "Doc 1-1", "Doc 1-2" });
        CreateDocumentGroup("Group 2", new[] { "Doc 2-1", "Doc 2-2" });
        CreateDocumentGroup("Group 3", new[] { "Doc 3-1" });

        // Result: Three separate tab groups automatically created
    }

    private void CreateDocumentGroup(string groupName, string[] docNames)
    {
        foreach (string docName in docNames)
        {
            Form doc = new Form();
            doc.Text = $"{groupName} - {docName}";
            doc.MdiParent = this;
            doc.Show();
        }
    }
}
```

### Example 3: Dynamic Group Management

```csharp
public partial class DynamicGroupForm : Form
{
    private TabbedGroupedMDIManager tabbedMDI;

    public DynamicGroupForm()
    {
        InitializeComponent();
        SetupDynamicGroups();
    }

    private void SetupDynamicGroups()
    {
        this.IsMdiContainer = true;
        this.Text = "Dynamic Group Management";

        tabbedMDI = new TabbedGroupedMDIManager();
        this.Controls.Add(tabbedMDI);
        tabbedMDI.AttachToMdiContainer(this);

        // Create menu for group management
        CreateMenu();
    }

    private void CreateMenu()
    {
        MenuStrip menu = new MenuStrip();
        this.Controls.Add(menu);
        this.MainMenuStrip = menu;

        ToolStripMenuItem fileMenu = menu.Items.Add("&File") as ToolStripMenuItem;
        fileMenu.DropDownItems.Add("&New Document", null, (s, e) => CreateNewDocument());
        fileMenu.DropDownItems.Add("&Close Active Group", null, (s, e) => CloseActiveGroup());
        fileMenu.DropDownItems.AddSeparator();
        fileMenu.DropDownItems.Add("E&xit", null, (s, e) => this.Close());

        ToolStripMenuItem viewMenu = menu.Items.Add("&View") as ToolStripMenuItem;
        viewMenu.DropDownItems.Add("&Horizontal Split", null, (s, e) =>
            tabbedMDI.Orientation = Orientation.Horizontal);
        viewMenu.DropDownItems.Add("&Vertical Split", null, (s, e) =>
            tabbedMDI.Orientation = Orientation.Vertical);
        viewMenu.DropDownItems.AddSeparator();
        viewMenu.DropDownItems.Add("&Show Group Info", null, (s, e) => ShowGroupInfo());
    }

    private void CreateNewDocument()
    {
        Form doc = new Form();
        doc.Text = $"Document {DateTime.Now:HH:mm:ss}";
        doc.MdiParent = this;
        doc.Show();
    }

    private void CloseActiveGroup()
    {
        if (tabbedMDI.Groups.Count > 1)
        {
            // Get last group and remove it
            var lastGroup = tabbedMDI.Groups[tabbedMDI.Groups.Count - 1] as TabControlAdv;
            if (lastGroup != null)
            {
                // Close all documents in this group
                for (int i = lastGroup.TabPages.Count - 1; i >= 0; i--)
                {
                    lastGroup.TabPages.RemoveAt(i);
                }
            }
        }
    }

    private void ShowGroupInfo()
    {
        string info = $"Total Groups: {tabbedMDI.Groups.Count}\n";
        for (int i = 0; i < tabbedMDI.Groups.Count; i++)
        {
            TabControlAdv group = tabbedMDI.Groups[i] as TabControlAdv;
            if (group != null)
            {
                info += $"Group {i + 1}: {group.TabPages.Count} tabs\n";
            }
        }
        MessageBox.Show(info, "Group Information");
    }
}
```

## Best Practices

1. **Use TabbedGroupedMDIManager for multiple groups** - Don't try to create groups manually
2. **Set orientation early** - Configure horizontal/vertical before adding many documents
3. **Provide visual feedback** - Show users which group they're in with status bar
4. **Splitter sizing** - Let users resize groups, consider saving group sizes
5. **Document organization** - Use groups logically (related documents together)

## Troubleshooting

### Issue: Only One Group Visible
**Cause:** Documents not properly assigned to different groups
**Solution:** Ensure you're using `TabbedGroupedMDIManager`, not regular `TabbedMDIManager`

### Issue: Splitter Not Resizable
**Cause:** Groups may not support splitter in your Syncfusion version
**Solution:** Check that `ShowGroupBorder` property is enabled:

```csharp
tabbedMDI.ShowGroupBorder = true;
```

### Issue: Groups Rearranging Unexpectedly
**Solution:** Avoid modifying `Groups` collection directly while forms are being added. Let the manager handle group creation automatically.
