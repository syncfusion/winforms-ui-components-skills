# Events & Interaction Handling

## Table of Contents
- [Available Events](#available-events)
- [Event Examples](#event-examples)
- [Context Menu](#context-menu)
- [Tooltips](#tooltips)
- [Complete Examples](#complete-examples)

## Available Events

TabbedMDIManager provides several events for handling user interactions:

| Event | Fired When | Usage |
|-------|-----------|-------|
| `BeforeMDIChildAdded` | Before a child form is added | Validate or configure new document |
| `TabControlAdded` | Tab control is created | Customize tab appearance |
| `TabControlAdding` | Tab control is being added | Configure before display |
| `TabControlRemoved` | Tab control is removed | Cleanup when group deleted |
| `BeforeDropDownPopup` | Dropdown button is clicked | Style the dropdown menu |
| `UnLockingMdIClient` | MDI client is being unlocked | Handle layout changes |

## Event Examples

### BeforeMDIChildAdded Event

Triggered before a new MDI child is added to the container:

```csharp
private void SetupBeforeMDIChildAdded()
{
    tabbedMDIManager.BeforeMDIChildAdded += (sender, e) =>
    {
        // e.NewControl is the form being added
        Form newForm = e.NewControl as Form;

        if (newForm != null)
        {
            Console.WriteLine($"Adding child form: {newForm.Text}");

            // Can modify the form before it's displayed
            if (!newForm.Text.StartsWith("Document"))
            {
                newForm.Text = $"Document - {newForm.Text}";
            }

            // Can prevent adding by setting e.Cancel = true
            // e.Cancel = true;  // Prevents form from being added
        }
    };
}
```

**Use Cases:**
- Validate that a form meets requirements before adding
- Apply initial styling or state to new documents
- Log document creation
- Prevent invalid documents from being added

### TabControlAdded Event

Triggered when a new tab group is created:

```csharp
private void SetupTabControlAdded()
{
    tabbedMDIManager.TabControlAdded += (sender, args) =>
    {
        TabControlAdv tabControl = args.TabControl;

        // Customize the tab control
        tabControl.Alignment = TabAlignment.Top;
        tabControl.Font = new Font("Segoe UI", 10, FontStyle.Regular);

        Console.WriteLine($"New tab group created with {tabControl.TabPages.Count} tabs");
    };
}
```

**Use Cases:**
- Set tab alignment for each group
- Apply fonts and styling
- Initialize group-specific behavior
- Log tab group creation

### TabControlAdding Event

Triggered while a tab control is being added:

```csharp
private void SetupTabControlAdding()
{
    tabbedMDIManager.TabControlAdding += (sender, args) =>
    {
        TabControlAdv tabControl = args.TabControl;

        Console.WriteLine($"Tab control is being added: {tabControl.Name}");

        // Perform setup before control is fully added
        // Similar to TabControlAdded but fires earlier in the process
    };
}
```

### TabControlRemoved Event

Triggered when a tab group is removed:

```csharp
private void SetupTabControlRemoved()
{
    tabbedMDIManager.TabControlRemoved += (sender, args) =>
    {
        TabControlAdv removedTabControl = args.TabControl;

        Console.WriteLine($"Tab group removed: {removedTabControl.Name}");
        Console.WriteLine($"It had {removedTabControl.TabPages.Count} tabs");

        // Cleanup when group is removed
        // Example: Save state, update UI, etc.
    };
}
```

### BeforeDropDownPopup Event

Customize the dropdown menu appearance:

```csharp
private void SetupBeforeDropDownPopup()
{
    tabbedMDIManager.BeforeDropDownPopup += (sender, e) =>
    {
        // Set dropdown visual style
        e.ParentBarItem.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016;

        // Can prevent popup
        // e.Cancel = true;

        Console.WriteLine("Dropdown menu is about to show");
    };
}
```

**Use Cases:**
- Style the dropdown menu to match your app
- Prevent dropdown in certain conditions
- Log when users access the document list

### UnLockingMdIClient Event

Triggered when the MDI client is unlocked:

```csharp
private void SetupUnLockingMdIClient()
{
    tabbedMDIManager.UnLockingMdIClient += (sender, e) =>
    {
        Console.WriteLine("MDI client is being unlocked");

        // Handle any cleanup or state changes
        // This fires when switching from locked to unlocked MDI mode
    };
}
```

## Complete Event Setup Example

```csharp
private void SetupAllEvents()
{
    // BeforeMDIChildAdded - Validate new documents
    tabbedMDIManager.BeforeMDIChildAdded += (sender, e) =>
    {
        Form form = e.NewControl as Form;
        if (form != null)
        {
            Console.WriteLine($"[BeforeMDIChildAdded] {form.Text}");
        }
    };

    // TabControlAdded - Customize tab groups
    tabbedMDIManager.TabControlAdded += (sender, args) =>
    {
        Console.WriteLine("[TabControlAdded] New tab group created");
        args.TabControl.Alignment = TabAlignment.Top;
    };

    // TabControlAdding - Early setup
    tabbedMDIManager.TabControlAdding += (sender, args) =>
    {
        Console.WriteLine("[TabControlAdding] Tab group is being created");
    };

    // TabControlRemoved - Cleanup
    tabbedMDIManager.TabControlRemoved += (sender, args) =>
    {
        Console.WriteLine("[TabControlRemoved] Tab group removed");
    };

    // BeforeDropDownPopup - Style dropdown
    tabbedMDIManager.BeforeDropDownPopup += (sender, e) =>
    {
        Console.WriteLine("[BeforeDropDownPopup] Dropdown is showing");
        e.ParentBarItem.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016;
    };

    // UnLockingMdIClient - Lock/unlock handling
    tabbedMDIManager.UnLockingMdIClient += (sender, e) =>
    {
        Console.WriteLine("[UnLockingMdIClient] MDI client unlocked");
    };
}
```

## Context Menu

### Understanding Context Menu

Right-clicking on a tab shows a default context menu with options like:
- Close tab
- Move to new group
- Close other tabs
- etc.

### Using ContextMenuItem Property

```csharp
private void SetupContextMenu()
{
    using Syncfusion.Windows.Forms.Tools.XPMenus;

    // Create custom bar items
    ParentBarItem contextMenu = new ParentBarItem();

    // Add custom item 1
    BarItem customItem1 = new BarItem();
    customItem1.Text = "Save Document";
    customItem1.MergeOrder = 30;
    contextMenu.Items.Add(customItem1);

    // Add custom item 2
    BarItem customItem2 = new BarItem();
    customItem2.Text = "Print Document";
    customItem2.MergeOrder = 31;
    contextMenu.Items.Add(customItem2);

    // Assign to manager
    tabbedMDIManager.ContextMenuItem = contextMenu;

    // Result: Default menu items + your custom items
}
```

### Custom Context Menu with Event Handlers

```csharp
private void SetupContextMenuWithEvents()
{
    using Syncfusion.Windows.Forms.Tools.XPMenus;

    ParentBarItem contextMenu = new ParentBarItem();

    // Save item
    BarItem saveItem = new BarItem();
    saveItem.Text = "Save";
    saveItem.MergeOrder = 30;
    saveItem.Click += (s, e) =>
    {
        Console.WriteLine("User clicked Save from context menu");
        // Implement save logic
    };
    contextMenu.Items.Add(saveItem);

    // Close item
    BarItem closeItem = new BarItem();
    closeItem.Text = "Close This Tab";
    closeItem.MergeOrder = 31;
    closeItem.Click += (s, e) =>
    {
        Console.WriteLine("User clicked Close from context menu");
        // Implement close logic
    };
    contextMenu.Items.Add(closeItem);

    tabbedMDIManager.ContextMenuItem = contextMenu;
}
```

## Tooltips

### Setting Tooltips for Tabs

```csharp
private void SetupTooltips()
{
    // Get tooltip for a specific form
    Form doc = new Form() { Text = "Document 1", MdiParent = this };
    doc.Show();

    // Set tooltip
    tabbedMDIManager.SetTooltip(doc, "This is Document 1\nLast edited: Today");

    // Get tooltip
    string tooltip = tabbedMDIManager.GetTooltip(doc);
    Console.WriteLine($"Tooltip: {tooltip}");
}
```

### Dynamic Tooltip Updates

```csharp
private void UpdateTooltips()
{
    foreach (Form childForm in this.MdiChildren)
    {
        // Create informative tooltip
        string tooltip = $"{childForm.Text}\n";
        tooltip += $"Created: {DateTime.Now:g}\n";
        tooltip += $"Status: Active";

        tabbedMDIManager.SetTooltip(childForm, tooltip);
    }
}

// Show tooltip on mouse hover over tab
// Tooltips automatically display when user hovers over tabs
```

## Complete Examples

### Example 1: Document Tracking

```csharp
public partial class DocumentTrackingForm : Form
{
    private TabbedMDIManager tabbedMDI;
    private Dictionary<Form, DateTime> documentCreatedTimes = new Dictionary<Form, DateTime>();

    public DocumentTrackingForm()
    {
        InitializeComponent();
        SetupTrackedMDI();
    }

    private void SetupTrackedMDI()
    {
        this.IsMdiContainer = true;
        this.Text = "Document Tracking MDI";

        tabbedMDI = new TabbedMDIManager();
        this.Controls.Add(tabbedMDI);
        tabbedMDI.AttachToMdiContainer(this);
        tabbedMDI.ThemesEnabled = true;

        // Setup event tracking
        tabbedMDI.BeforeMDIChildAdded += (sender, e) =>
        {
            Form newForm = e.NewControl as Form;
            if (newForm != null)
            {
                // Track creation time
                documentCreatedTimes[newForm] = DateTime.Now;
                Console.WriteLine($"[Tracked] {newForm.Text} created at {DateTime.Now:HH:mm:ss}");
            }
        };

        // Setup tooltips with timestamps
        tabbedMDI.TabControlAdded += (sender, args) =>
        {
            // When a tab group is added, update tooltips
            UpdateAllTooltips();
        };

        CreateMenu();
        CreateInitialDocs();
    }

    private void UpdateAllTooltips()
    {
        foreach (Form form in this.MdiChildren)
        {
            if (documentCreatedTimes.TryGetValue(form, out DateTime createdTime))
            {
                string tooltip = $"{form.Text}\n";
                tooltip += $"Created: {createdTime:g}\n";
                tooltip += $"Age: {(DateTime.Now - createdTime).TotalMinutes:F1} minutes";
                tabbedMDI.SetTooltip(form, tooltip);
            }
        }
    }

    private void CreateMenu()
    {
        MenuStrip menu = new MenuStrip();
        this.Controls.Add(menu);
        this.MainMenuStrip = menu;

        ToolStripMenuItem fileMenu = menu.Items.Add("&File") as ToolStripMenuItem;
        fileMenu.DropDownItems.Add("&New", null, (s, e) => CreateNewDocument());
        fileMenu.DropDownItems.AddSeparator();
        fileMenu.DropDownItems.Add("E&xit", null, (s, e) => this.Close());
    }

    private void CreateNewDocument()
    {
        Form doc = new Form();
        doc.Text = $"Document {this.MdiChildren.Length + 1}";
        doc.MdiParent = this;
        doc.Show();
    }

    private void CreateInitialDocs()
    {
        for (int i = 1; i <= 3; i++) CreateNewDocument();
    }
}
```

### Example 2: Advanced Context Menu

```csharp
public partial class AdvancedContextMenuForm : Form
{
    private TabbedMDIManager tabbedMDI;

    public AdvancedContextMenuForm()
    {
        InitializeComponent();
        SetupAdvancedContextMenu();
    }

    private void SetupAdvancedContextMenu()
    {
        using Syncfusion.Windows.Forms.Tools.XPMenus;

        this.IsMdiContainer = true;
        this.Text = "Advanced Context Menu Demo";

        tabbedMDI = new TabbedMDIManager();
        this.Controls.Add(tabbedMDI);
        tabbedMDI.AttachToMdiContainer(this);
        tabbedMDI.ThemesEnabled = true;

        // Create custom context menu
        ParentBarItem contextMenu = new ParentBarItem();

        // Save section
        BarItem saveItem = new BarItem();
        saveItem.Text = "Save";
        saveItem.MergeOrder = 30;
        saveItem.Click += (s, e) => MessageBox.Show("Document saved!");
        contextMenu.Items.Add(saveItem);

        BarItem saveAsItem = new BarItem();
        saveAsItem.Text = "Save As...";
        saveAsItem.MergeOrder = 31;
        saveAsItem.Click += (s, e) => MessageBox.Show("Save As dialog...");
        contextMenu.Items.Add(saveAsItem);

        // Separator
        contextMenu.BeginGroupAt(saveItem);

        // Print section
        BarItem printItem = new BarItem();
        printItem.Text = "Print";
        printItem.MergeOrder = 40;
        printItem.Click += (s, e) => MessageBox.Show("Printing...");
        contextMenu.Items.Add(printItem);

        contextMenu.BeginGroupAt(printItem);

        // Properties
        BarItem propertiesItem = new BarItem();
        propertiesItem.Text = "Properties";
        propertiesItem.MergeOrder = 50;
        propertiesItem.Click += (s, e) => MessageBox.Show("Showing properties...");
        contextMenu.Items.Add(propertiesItem);

        // Assign to manager
        tabbedMDI.ContextMenuItem = contextMenu;

        // Style the dropdown
        tabbedMDI.BeforeDropDownPopup += (sender, e) =>
        {
            e.ParentBarItem.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016;
        };

        CreateMenu();
        CreateSampleDocs();
    }

    private void CreateMenu()
    {
        MenuStrip menu = new MenuStrip();
        this.Controls.Add(menu);
        this.MainMenuStrip = menu;

        ToolStripMenuItem fileMenu = menu.Items.Add("&File") as ToolStripMenuItem;
        fileMenu.DropDownItems.Add("&New", null, (s, e) => CreateNewDocument());
        fileMenu.DropDownItems.AddSeparator();
        fileMenu.DropDownItems.Add("E&xit", null, (s, e) => this.Close());
    }

    private void CreateNewDocument()
    {
        Form doc = new Form();
        doc.Text = $"Document {this.MdiChildren.Length + 1}";
        doc.MdiParent = this;
        doc.Show();
    }

    private void CreateSampleDocs()
    {
        for (int i = 1; i <= 3; i++) CreateNewDocument();
    }
}
```

## Best Practices

1. **Event order** - BeforeMDIChildAdded fires before TabControlAdded
2. **Avoid heavy processing** - Keep event handlers lightweight
3. **Error handling** - Wrap event code in try-catch
4. **Memory management** - Unsubscribe from events if needed
5. **State tracking** - Use dictionaries to track document state

## Troubleshooting

### Issue: Context Menu Items Not Showing
**Solution:** Verify `ContextMenuItem` is assigned BEFORE adding documents

### Issue: Tooltip Not Displaying
**Solution:** Ensure tooltip is set AFTER form is added to MdiChildren:
```csharp
Form doc = new Form() { MdiParent = this };
doc.Show();
tabbedMDI.SetTooltip(doc, "My tooltip");  // Must be after show
```
