# Tab Alignment & Positioning

Customize where tabs appear in your MDI application.

## Tab Alignment Options

The TabbedMDIManager supports tab alignment in four directions:

| Alignment | Position | Use Case |
|-----------|----------|----------|
| `Top` | Top of container | Default, most common |
| `Bottom` | Bottom of container | Less common, taskbar-like |
| `Left` | Left edge | Vertical document list |
| `Right` | Right edge | Side panel layout |

## Setting Tab Alignment

### Method: Using TabControlAdded Event

The `TabControlAdded` event fires when a tab control is created. This is where you set the alignment:

```csharp
private void SetupTabAlignment()
{
    tabbedMDIManager.TabControlAdded += new TabbedMDITabControlEventHandler(
        tabbedMDIManager_TabControlAdded);
}

private void tabbedMDIManager_TabControlAdded(object sender, TabbedMDITabControlEventArgs args)
{
    // Align tabs to the bottom
    args.TabControl.Alignment = TabAlignment.Bottom;
}
```

## Alignment Examples

### Example 1: Top Alignment (Default)

```csharp
private void SetTopAlignment()
{
    tabbedMDIManager.TabControlAdded += (sender, args) =>
    {
        args.TabControl.Alignment = TabAlignment.Top;
    };
}

// Result: Tabs appear at the top of the form
//
// ┌─────────────────────┐
// │ Doc1 │ Doc2 │ Doc3  │  ← Tabs at top
// ├─────────────────────┤
// │                     │
// │   Document Area     │
// │                     │
// └─────────────────────┘
```

### Example 2: Bottom Alignment

```csharp
private void SetBottomAlignment()
{
    tabbedMDIManager.TabControlAdded += (sender, args) =>
    {
        args.TabControl.Alignment = TabAlignment.Bottom;
    };
}

// Result: Tabs appear at the bottom
//
// ┌─────────────────────┐
// │                     │
// │   Document Area     │
// │                     │
// ├─────────────────────┤
// │ Doc1 │ Doc2 │ Doc3  │  ← Tabs at bottom
// └─────────────────────┘
```

### Example 3: Left Alignment

```csharp
private void SetLeftAlignment()
{
    tabbedMDIManager.TabControlAdded += (sender, args) =>
    {
        args.TabControl.Alignment = TabAlignment.Left;
    };
}

// Result: Tabs appear on the left side (vertical)
//
// ┌──┬───────────────┐
// │D │               │
// │o │               │
// │c │ Document Area │
// │1 │               │
// ├──┤               │
// │D │               │
// │o │               │
// │c │               │
// │2 │               │
// └──┴───────────────┘
```

### Example 4: Right Alignment

```csharp
private void SetRightAlignment()
{
    tabbedMDIManager.TabControlAdded += (sender, args) =>
    {
        args.TabControl.Alignment = TabAlignment.Right;
    };
}

// Result: Tabs appear on the right side (vertical)
//
// ┌───────────────┬──┐
// │               │D │
// │               │o │
// │ Document Area │c │
// │               │1 │
// │               ├──┤
// │               │D │
// │               │o │
// │               │c │
// │               │2 │
// └───────────────┴──┘
```

## Dynamic Alignment Changes

### Change Alignment at Runtime

```csharp
private void ChangeAlignmentDynamically()
{
    // Get current tab control in first tab group
    if (tabbedMDIManager.Groups.Count > 0)
    {
        TabControlAdv tabControl = tabbedMDIManager.Groups[0] as TabControlAdv;
        if (tabControl != null)
        {
            // Change to bottom alignment
            tabControl.Alignment = TabAlignment.Bottom;
        }
    }
}
```

### Create Menu for User-Controlled Alignment

```csharp
private void CreateAlignmentMenu()
{
    MenuStrip menuStrip = new MenuStrip();
    this.Controls.Add(menuStrip);

    ToolStripMenuItem viewMenu = menuStrip.Items.Add("View") as ToolStripMenuItem;
    viewMenu.DropDownItems.Add("Tabs at Top", null, (s, e) => SetAlignment(TabAlignment.Top));
    viewMenu.DropDownItems.Add("Tabs at Bottom", null, (s, e) => SetAlignment(TabAlignment.Bottom));
    viewMenu.DropDownItems.Add("Tabs on Left", null, (s, e) => SetAlignment(TabAlignment.Left));
    viewMenu.DropDownItems.Add("Tabs on Right", null, (s, e) => SetAlignment(TabAlignment.Right));
}

private void SetAlignment(TabAlignment alignment)
{
    foreach (var group in tabbedMDIManager.Groups)
    {
        TabControlAdv tabControl = group as TabControlAdv;
        if (tabControl != null)
        {
            tabControl.Alignment = alignment;
        }
    }
}
```

## Alignment with Multiple Tab Groups

When using multiple tab groups, each group can have its own alignment:

```csharp
private void SetupMultiGroupAlignment()
{
    tabbedMDIManager.TabControlAdded += (sender, args) =>
    {
        // Check which group it is
        if (tabbedMDIManager.Groups.Count == 1)
        {
            args.TabControl.Alignment = TabAlignment.Top;
        }
        else if (tabbedMDIManager.Groups.Count == 2)
        {
            args.TabControl.Alignment = TabAlignment.Bottom;
        }
    };
}

// Result: First group has tabs at top, second group at bottom
//
// ┌──────────────────────┐
// │ Doc1 │ Doc2 │ Doc3   │  ← Group 1 at top
// ├──────────────────────┤
// │                      │
// │                      │  ← Content area
// │                      │
// ├──────────────────────┤
// │ Doc4 │ Doc5 │ Doc6   │  ← Group 2 at bottom
// └──────────────────────┘
```

## Complete Example: Configurable Tab Alignment

```csharp
public partial class MainForm : Form
{
    private TabbedMDIManager tabbedMDIManager;

    public MainForm()
    {
        InitializeComponent();
        SetupMDI();
    }

    private void SetupMDI()
    {
        this.IsMdiContainer = true;
        this.Text = "Tabbed MDI with Alignment Control";

        tabbedMDIManager = new TabbedMDIManager();
        this.Controls.Add(tabbedMDIManager);
        tabbedMDIManager.AttachToMdiContainer(this);
        tabbedMDIManager.ThemesEnabled = true;

        // Set initial alignment to bottom
        tabbedMDIManager.TabControlAdded += (sender, args) =>
        {
            args.TabControl.Alignment = TabAlignment.Bottom;
        };

        CreateMenu();
    }

    private void CreateMenu()
    {
        MenuStrip menuStrip = new MenuStrip();
        this.Controls.Add(menuStrip);
        this.MainMenuStrip = menuStrip;

        // File menu
        ToolStripMenuItem fileMenu = menuStrip.Items.Add("&File") as ToolStripMenuItem;
        fileMenu.DropDownItems.Add("&New Document", null, (s, e) => CreateNewDocument());
        fileMenu.DropDownItems.AddSeparator();
        fileMenu.DropDownItems.Add("E&xit", null, (s, e) => this.Close());

        // View menu
        ToolStripMenuItem viewMenu = menuStrip.Items.Add("&View") as ToolStripMenuItem;
        viewMenu.DropDownItems.Add("Tabs at &Top", null, (s, e) => SetAlignment(TabAlignment.Top));
        viewMenu.DropDownItems.Add("Tabs at &Bottom", null, (s, e) => SetAlignment(TabAlignment.Bottom));
        viewMenu.DropDownItems.Add("Tabs on &Left", null, (s, e) => SetAlignment(TabAlignment.Left));
        viewMenu.DropDownItems.Add("Tabs on &Right", null, (s, e) => SetAlignment(TabAlignment.Right));
    }

    private void SetAlignment(TabAlignment alignment)
    {
        foreach (var group in tabbedMDIManager.Groups)
        {
            TabControlAdv tabControl = group as TabControlAdv;
            if (tabControl != null)
            {
                tabControl.Alignment = alignment;
                Console.WriteLine($"Alignment changed to: {alignment}");
            }
        }
    }

    private void CreateNewDocument()
    {
        Form childForm = new Form();
        childForm.Text = $"Document {this.MdiChildren.Length + 1}";
        childForm.MdiParent = this;
        childForm.Show();
    }
}
```

## Best Practices

1. **Set alignment early** - Configure in `TabControlAdded` event when control is first created
2. **Consider screen space** - Use Left/Right alignment when you have wide documents, Top/Bottom for tall ones
3. **Consistency** - Keep alignment consistent across the application
4. **Document your choice** - Add menu items or help text explaining the layout
5. **Accessibility** - Top alignment is most familiar to users (Microsoft Office default)

## Troubleshooting

### Issue: Alignment Not Changing
**Solution:** Ensure you're handling `TabControlAdded` event or modifying the correct tab group:

```csharp
// Correct: Set in event handler
tabbedMDIManager.TabControlAdded += (s, e) => e.TabControl.Alignment = TabAlignment.Bottom;

// Won't work: Setting before groups are created
// tabbedMDIManager.Alignment = TabAlignment.Bottom;  // This property doesn't exist
```

### Issue: Only First Group Alignment Works
**Solution:** Multiple groups need to be set individually:

```csharp
// Set alignment for ALL groups
foreach (var group in tabbedMDIManager.Groups)
{
    TabControlAdv tabControl = group as TabControlAdv;
    if (tabControl != null)
    {
        tabControl.Alignment = TabAlignment.Bottom;
    }
}
```
