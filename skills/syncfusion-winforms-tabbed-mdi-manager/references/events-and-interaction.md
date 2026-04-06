# Events & Interaction Handling

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

Triggered before a new MDI child is added to the container. Use for validation, initial styling, or preventing addition.

```csharp
tabbedMDIManager.BeforeMDIChildAdded += (sender, e) =>
{
    Form newForm = e.NewControl as Form;
    if (newForm != null)
    {
        Console.WriteLine($"Adding child form: {newForm.Text}");
        if (!newForm.Text.StartsWith("Document"))
            newForm.Text = $"Document - {newForm.Text}";
        // e.Cancel = true;  // Prevents form from being added
    }
};
```

### TabControlAdded Event

Triggered when a new tab group is created. Use for customizing tab appearance and behavior.

```csharp
tabbedMDIManager.TabControlAdded += (sender, args) =>
{
    args.TabControl.Alignment = TabAlignment.Top;
    args.TabControl.Font = new Font("Segoe UI", 10, FontStyle.Regular);
    Console.WriteLine($"New tab group created with {args.TabControl.TabPages.Count} tabs");
};
```

### TabControlAdding Event

Triggered while a tab control is being added (fires earlier than TabControlAdded).

```csharp
tabbedMDIManager.TabControlAdding += (sender, args) =>
{
    Console.WriteLine($"Tab control is being added: {args.TabControl.Name}");
    // Perform setup before control is fully added
};
```

### TabControlRemoved Event

Triggered when a tab group is removed. Use for cleanup tasks.

```csharp
tabbedMDIManager.TabControlRemoved += (sender, args) =>
{
    Console.WriteLine($"Tab group removed: {args.TabControl.Name} with {args.TabControl.TabPages.Count} tabs");
    // Cleanup: save state, update UI, etc.
};
```

### BeforeDropDownPopup Event

Customize the dropdown menu appearance or prevent display.

```csharp
tabbedMDIManager.BeforeDropDownPopup += (sender, e) =>
{
    e.ParentBarItem.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016;
    // e.Cancel = true;  // Prevents popup
    Console.WriteLine("Dropdown menu is about to show");
};
```

### UnLockingMdIClient Event

Triggered when the MDI client is being unlocked.

```csharp
tabbedMDIManager.UnLockingMdIClient += (sender, e) =>
{
    Console.WriteLine("MDI client is being unlocked");
    // Handle cleanup or state changes when switching from locked to unlocked MDI mode
};
```

## Complete Event Setup

```csharp
// Setup all events at once
tabbedMDIManager.BeforeMDIChildAdded += (sender, e) =>
{
    if (e.NewControl is Form form)
        Console.WriteLine($"[BeforeMDIChildAdded] {form.Text}");
};

tabbedMDIManager.TabControlAdded += (sender, args) =>
{
    Console.WriteLine("[TabControlAdded] New tab group created");
    args.TabControl.Alignment = TabAlignment.Top;
};

tabbedMDIManager.TabControlAdding += (sender, args) =>
    Console.WriteLine("[TabControlAdding] Tab group is being created");

tabbedMDIManager.TabControlRemoved += (sender, args) =>
    Console.WriteLine("[TabControlRemoved] Tab group removed");

tabbedMDIManager.BeforeDropDownPopup += (sender, e) =>
{
    Console.WriteLine("[BeforeDropDownPopup] Dropdown is showing");
    e.ParentBarItem.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016;
};

tabbedMDIManager.UnLockingMdIClient += (sender, e) =>
    Console.WriteLine("[UnLockingMdIClient] MDI client unlocked");
```

## Context Menu

Right-clicking on a tab shows a default context menu (Close, Move to new group, etc.). Customize using `ContextMenuItem` property.

### Custom Context Menu with Event Handlers

```csharp
using Syncfusion.Windows.Forms.Tools.XPMenus;

var contextMenu = new ParentBarItem();

// Add custom menu items with event handlers
contextMenu.Items.Add(new BarItem 
{ 
    Text = "Save", 
    MergeOrder = 30 
}.With(item => item.Click += (s, e) => Console.WriteLine("Save clicked")));

contextMenu.Items.Add(new BarItem 
{ 
    Text = "Save As...", 
    MergeOrder = 31 
}.With(item => item.Click += (s, e) => Console.WriteLine("Save As clicked")));

contextMenu.BeginGroupAt(contextMenu.Items[0] as BarItem);

contextMenu.Items.Add(new BarItem 
{ 
    Text = "Print", 
    MergeOrder = 40 
}.With(item => item.Click += (s, e) => Console.WriteLine("Print clicked")));

contextMenu.Items.Add(new BarItem 
{ 
    Text = "Properties", 
    MergeOrder = 50 
}.With(item => item.Click += (s, e) => MessageBox.Show("Properties...")));

tabbedMDIManager.ContextMenuItem = contextMenu;

// Note: Extension method for fluent syntax
public static T With<T>(this T obj, Action<T> action) { action(obj); return obj; }
```

## Tooltips

Use `SetTooltip` and `GetTooltip` methods to manage tab tooltips.

```csharp
// Set tooltip for a form
var doc = new Form { Text = "Document 1", MdiParent = this };
doc.Show();
tabbedMDIManager.SetTooltip(doc, "This is Document 1\nLast edited: Today");

// Get tooltip
string tooltip = tabbedMDIManager.GetTooltip(doc);

// Update all tooltips dynamically
foreach (Form childForm in this.MdiChildren)
{
    tabbedMDIManager.SetTooltip(childForm, 
        $"{childForm.Text}\nCreated: {DateTime.Now:g}\nStatus: Active");
}
// Tooltips automatically display on hover

## Complete Examples

### Document Tracking with Events and Tooltips

```csharp
public class DocumentTrackingForm : Form
{
    private TabbedMDIManager tabbedMDI;
    private Dictionary<Form, DateTime> documentTimes = new Dictionary<Form, DateTime>();

    public DocumentTrackingForm()
    {
        InitializeComponent();
        IsMdiContainer = true;
        Text = "Document Tracking MDI";

        tabbedMDI = new TabbedMDIManager { ThemesEnabled = true };
        Controls.Add(tabbedMDI);
        tabbedMDI.AttachToMdiContainer(this);

        // Track document creation
        tabbedMDI.BeforeMDIChildAdded += (sender, e) =>
        {
            if (e.NewControl is Form form)
            {
                documentTimes[form] = DateTime.Now;
                Console.WriteLine($"[Tracked] {form.Text} created at {DateTime.Now:HH:mm:ss}");
            }
        };

        // Update tooltips when tab group is added
        tabbedMDI.TabControlAdded += (sender, args) => UpdateAllTooltips();

        // Setup menu and create initial documents
        var menu = new MenuStrip();
        Controls.Add(menu);
        MainMenuStrip = menu;
        var fileMenu = (ToolStripMenuItem)menu.Items.Add("&File");
        fileMenu.DropDownItems.Add("&New", null, (s, e) => CreateDocument());
        fileMenu.DropDownItems.Add("E&xit", null, (s, e) => Close());

        for (int i = 1; i <= 3; i++) CreateDocument();
    }

    private void UpdateAllTooltips()
    {
        foreach (Form form in MdiChildren)
        {
            if (documentTimes.TryGetValue(form, out DateTime created))
            {
                tabbedMDI.SetTooltip(form, 
                    $"{form.Text}\nCreated: {created:g}\nAge: {(DateTime.Now - created).TotalMinutes:F1} min");
            }
        }
    }

    private void CreateDocument()
    {
        var doc = new Form { Text = $"Document {MdiChildren.Length + 1}", MdiParent = this };
        doc.Show();
    }
}
```

### Advanced Context Menu Example

```csharp
public class AdvancedContextMenuForm : Form
{
    private TabbedMDIManager tabbedMDI;

    public AdvancedContextMenuForm()
    {
        InitializeComponent();
        IsMdiContainer = true;
        Text = "Advanced Context Menu Demo";

        tabbedMDI = new TabbedMDIManager { ThemesEnabled = true };
        Controls.Add(tabbedMDI);
        tabbedMDI.AttachToMdiContainer(this);

        // Create custom context menu
        var contextMenu = new ParentBarItem();
        
        var items = new[]
        {
            new { Text = "Save", Order = 30, Handler = (EventHandler)((s, e) => MessageBox.Show("Saved!")) },
            new { Text = "Save As...", Order = 31, Handler = (EventHandler)((s, e) => MessageBox.Show("Save As...")) },
            new { Text = "Print", Order = 40, Handler = (EventHandler)((s, e) => MessageBox.Show("Printing...")) },
            new { Text = "Properties", Order = 50, Handler = (EventHandler)((s, e) => MessageBox.Show("Properties...")) }
        };

        foreach (var item in items)
        {
            var barItem = new BarItem { Text = item.Text, MergeOrder = item.Order };
            barItem.Click += item.Handler;
            contextMenu.Items.Add(barItem);
            if (item.Order == 40) contextMenu.BeginGroupAt(barItem);
        }

        tabbedMDI.ContextMenuItem = contextMenu;
        tabbedMDI.BeforeDropDownPopup += (sender, e) =>
            e.ParentBarItem.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016;

        // Setup menu
        var menu = new MenuStrip();
        Controls.Add(menu);
        MainMenuStrip = menu;
        var fileMenu = (ToolStripMenuItem)menu.Items.Add("&File");
        fileMenu.DropDownItems.Add("&New", null, (s, e) => CreateDocument());
        fileMenu.DropDownItems.Add("E&xit", null, (s, e) => Close());

        for (int i = 1; i <= 3; i++) CreateDocument();
    }

    private void CreateDocument()
    {
        var doc = new Form { Text = $"Document {MdiChildren.Length + 1}", MdiParent = this };
        doc.Show();
    }
}
```

## Best Practices

- **Event order** - BeforeMDIChildAdded fires before TabControlAdded
- **Performance** - Keep event handlers lightweight; avoid heavy processing
- **Error handling** - Wrap event code in try-catch blocks
- **Memory** - Unsubscribe from events when disposing if needed

## Troubleshooting

**Context Menu Items Not Showing**: Assign `ContextMenuItem` BEFORE adding documents.

**Tooltip Not Displaying**: Set tooltip AFTER form is shown:
```csharp
var doc = new Form { MdiParent = this };
doc.Show();
tabbedMDI.SetTooltip(doc, "My tooltip");  // Must be after Show()
```
