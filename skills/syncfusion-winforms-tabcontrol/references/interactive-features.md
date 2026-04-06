# Interactive Features

## Table of Contents
- [Close Button Settings](#close-button-settings)
- [Tooltip Support](#tooltip-support)
- [SuperTooltip Support](#supertooltip-support)
- [Scroll Settings](#scroll-settings)

Configure interactive features like close buttons, tooltips, and scrolling for TabControlAdv.

## Close Button Settings

Enable close buttons on tabs to allow users to close tab pages. Configure via designer or code.

### Close Button Properties

| Property | Description |
|----------|-------------|
| `ShowTabCloseButton` | Show close button on all tabs |
| `ShowCloseButtonForActiveTabOnly` | Show close button only on active tab |
| `CloseTabOnMiddleClick` | Enable middle-click to close (browser-style) |

### Complete Close Button Example

```csharp
// Setup with events
TabControlAdv documentTabs = new TabControlAdv
{
    Dock = DockStyle.Fill,
    ShowTabCloseButton = true,
    CloseTabOnMiddleClick = true
};

for (int i = 1; i <= 5; i++)
{
    var tab = new TabPageAdv { Text = $"Document {i}" };
    
    // Handle closing with confirmation
    tab.Closing += (sender, e) =>
    {
        var result = MessageBox.Show($"Save changes to {tab.Text}?", "Close Document", MessageBoxButtons.YesNoCancel);
        if (result == DialogResult.Cancel) e.Cancel = true;
        else if (result == DialogResult.Yes) Console.WriteLine($"Saved {tab.Text}");
    };
    
    // Cleanup on close
    tab.Closed += (sender, e) => Console.WriteLine($"{tab.Text} closed");
    
    documentTabs.TabPages.Add(tab);
}

this.Controls.Add(documentTabs);
```

## Tooltip Support

Display tooltips when users hover over tabs. Configure via designer or code.

### Tooltip Example

```csharp
// Enable and configure tooltips
TabControlAdv tabsWithTooltips = new TabControlAdv
{
    ShowToolTips = true,
    Size = new Size(600, 400)
};

tabsWithTooltips.TabPages.Add(new TabPageAdv { Text = "Home", ToolTipText = "Dashboard with key metrics and recent activity" });
tabsWithTooltips.TabPages.Add(new TabPageAdv { Text = "Data", ToolTipText = "View and manage your data records" });
tabsWithTooltips.TabPages.Add(new TabPageAdv { Text = "Reports", ToolTipText = "Generate and view reports (Ctrl+R)" });

this.Controls.Add(tabsWithTooltips);
```

## SuperTooltip Support

Display enhanced tooltips with rich content and formatting. Configure SuperToolTip component separately.

### SuperTooltip Example

```csharp
// Setup SuperTooltip with rich content
var superToolTip1 = new SuperToolTip();
tabControlAdv1.ShowSuperToolTips = true;

var homeTab = new TabPageAdv { Text = "Home" };
var homeTooltip = new ToolTipInfo
{
    Header = { Text = "Home Dashboard" },
    Body = { Text = "View your personalized dashboard\nwith key metrics and alerts" },
    Footer = { Text = "Press F5 to refresh" }
};
superToolTip1.SetToolTip(homeTab, homeTooltip);
tabControlAdv1.TabPages.Add(homeTab);
```

## Scroll Settings

Configure scrolling behavior when tabs exceed available space. Configure via designer or code.

### Scroll Properties

| Property | Values | Description |
|----------|--------|-------------|
| `ShowScroll` | true/false | Enable scroll buttons for many tabs |
| `VSLikeScrollButton` | true/false | Use Visual Studio-style scroll buttons |
| `ScrollIncrement` | Tab/Page | Scroll one tab or one page at a time |
| `BringSelectedTabToView()` | Method | Programmatically scroll to selected tab |

### Complete Scrolling Example

```csharp
// Setup scrollable tabs with content auto-scroll
TabControlAdv scrollableTabs = new TabControlAdv
{
    Size = new Size(500, 400),
    ShowScroll = true,
    VSLikeScrollButton = true,
    ScrollIncrement = ScrollIncrement.Tab,
    Multiline = false
};

// Add many tabs with scrollable content
for (int i = 1; i <= 20; i++)
{
    var tab = new TabPageAdv
    {
        Text = $"Document {i}",
        AutoScroll = true,
        AutoScrollMinSize = new Size(600, 500)
    };
    
    tab.Controls.Add(new Label { Text = $"Content for Document {i}", Location = new Point(250, 250) });
    scrollableTabs.TabPages.Add(tab);
}

// Navigate to specific tab
scrollableTabs.SelectedIndex = 15;
scrollableTabs.BringSelectedTabToView();

this.Controls.Add(scrollableTabs);
```

## Complete Interactive Features Example

```csharp
public class InteractiveTabsExample : Form
{
    private TabControlAdv tabControl;
    
    public InteractiveTabsExample()
    {
        this.Text = "Interactive Tabs Example";
        this.Size = new Size(800, 600);
        
        // Setup with all interactive features
        tabControl = new TabControlAdv
        {
            Dock = DockStyle.Fill,
            ShowTabCloseButton = true,
            CloseTabOnMiddleClick = true,
            ShowToolTips = true,
            ShowScroll = true,
            VSLikeScrollButton = true,
            ScrollIncrement = ScrollIncrement.Tab
        };
        
        // Add interactive tabs
        for (int i = 1; i <= 10; i++)
        {
            var tab = new TabPageAdv
            {
                Text = $"Document {i}",
                ToolTipText = $"Document {i} - Last modified: {DateTime.Now.ToShortDateString()}"
            };
            
            // Add content
            var panel = new Panel { Dock = DockStyle.Fill, Padding = new Padding(20) };
            panel.Controls.Add(new TextBox { Multiline = true, Dock = DockStyle.Fill, Text = $"Content for document {i}..." });
            panel.Controls.Add(new Label { Text = $"Document {i}", Font = new Font("Segoe UI", 16, FontStyle.Bold), Dock = DockStyle.Top });
            tab.Controls.Add(panel);
            
            // Handle close events
            tab.Closing += (s, e) =>
            {
                if (MessageBox.Show($"Close {tab.Text}?", "Confirm", MessageBoxButtons.YesNo) == DialogResult.No)
                    e.Cancel = true;
            };
            
            tab.Closed += (s, e) =>
            {
                Console.WriteLine($"{tab.Text} was closed");
                if (tabControl.TabPages.Count == 0)
                    tabControl.TabPages.Add(new TabPageAdv { Text = "New Document", ToolTipText = "Empty document" });
            };
            
            tabControl.TabPages.Add(tab);
        }
        
        this.Controls.Add(tabControl);
    }
}
```

## Best Practices

| Feature | Best Practice |
|---------|---------------|
| **Close Buttons** | Use `ShowCloseButtonForActiveTabOnly` for less clutter; always handle `Closing` event to prevent data loss |
| **Tooltips** | Keep text concise; include keyboard shortcuts; use SuperTooltips for complex information |
| **Scrolling** | Enable for 8+ tabs; use `ScrollIncrement.Tab` for precise control or `.Page` for faster navigation |
| **UX** | Support middle-click closing; provide keyboard shortcuts (Ctrl+W, Ctrl+Tab); consider undo for closed tabs |

## Common Scenarios

```csharp
// Web Browser-Style Tabs
tabControlAdv1.ShowTabCloseButton = true;
tabControlAdv1.CloseTabOnMiddleClick = true;
tabControlAdv1.ShowScroll = true;
tabControlAdv1.UserMoveTabs = true;

// Document Editor (with close confirmation)
tabControlAdv1.ShowTabCloseButton = true;
tabControlAdv1.ShowCloseButtonForActiveTabOnly = true;
tabControlAdv1.ShowToolTips = true;
// Handle Closing event for unsaved changes

// Fixed Tab Set (No Closing)
tabControlAdv1.ShowTabCloseButton = false;
tabControlAdv1.ShowToolTips = true;
tabControlAdv1.Multiline = true;

// Scrollable Dashboard
tabControlAdv1.ShowScroll = true;
tabControlAdv1.ScrollIncrement = ScrollIncrement.Page;
tabControlAdv1.ShowToolTips = true;
```
