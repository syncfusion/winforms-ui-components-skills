# Interactive Features

## Table of Contents
- [Close Button Settings](#close-button-settings)
- [Tooltip Support](#tooltip-support)
- [SuperTooltip Support](#supertooltip-support)
- [Scroll Settings](#scroll-settings)

Configure interactive features like close buttons, tooltips, and scrolling for TabControlAdv.

## Close Button Settings

Enable close buttons on tabs to allow users to close tab pages.

### ShowTabCloseButton Property

Display close button on all tabs:

```csharp
// Show close button on all tabs
tabControlAdv1.ShowTabCloseButton = true;

// Hide close buttons
tabControlAdv1.ShowTabCloseButton = false;
```

### ShowCloseButtonForActiveTabOnly

Display close button only on the currently active tab:

```csharp
// Show close button only on active tab
tabControlAdv1.ShowCloseButtonForActiveTabOnly = true;

// Note: ShowTabCloseButton must be true
tabControlAdv1.ShowTabCloseButton = true;
tabControlAdv1.ShowCloseButtonForActiveTabOnly = true;
```

**When to use:** Document editors, browsers, or applications where only one tab should be closeable at a time.

### CloseTabOnMiddleClick

Close tabs using middle mouse button click:

```csharp
// Enable middle-click to close
tabControlAdv1.CloseTabOnMiddleClick = true;

// Disable middle-click closing
tabControlAdv1.CloseTabOnMiddleClick = false;
```

**User interaction:** Users can middle-click (scroll wheel click) on a tab to close it, similar to browser behavior.

### Handling Tab Close Events

React when a tab is being closed or has been closed:

```csharp
// Before tab closes (can be cancelled)
tabPageAdv1.Closing += (sender, e) =>
{
    var result = MessageBox.Show(
        "Are you sure you want to close this tab?",
        "Confirm Close",
        MessageBoxButtons.YesNo,
        MessageBoxIcon.Question);
    
    if (result == DialogResult.No)
    {
        e.Cancel = true; // Cancel the close operation
    }
};

// After tab is closed
tabPageAdv1.Closed += (sender, e) =>
{
    Console.WriteLine("Tab was closed");
    // Cleanup resources, save state, etc.
};
```

### Complete Close Button Example

```csharp
TabControlAdv documentTabs = new TabControlAdv();
documentTabs.Dock = DockStyle.Fill;
documentTabs.ShowTabCloseButton = true;
documentTabs.CloseTabOnMiddleClick = true;

// Add document tabs
for (int i = 1; i <= 5; i++)
{
    TabPageAdv tab = new TabPageAdv();
    tab.Text = $"Document {i}";
    
    // Handle closing event
    tab.Closing += (sender, e) =>
    {
        var tabPage = sender as TabPageAdv;
        var result = MessageBox.Show(
            $"Save changes to {tabPage.Text}?",
            "Close Document",
            MessageBoxButtons.YesNoCancel);
        
        if (result == DialogResult.Cancel)
        {
            e.Cancel = true;
        }
        else if (result == DialogResult.Yes)
        {
            // Save document logic here
            Console.WriteLine($"Saved {tabPage.Text}");
        }
    };
    
    // Handle closed event
    tab.Closed += (sender, e) =>
    {
        var tabPage = sender as TabPageAdv;
        Console.WriteLine($"{tabPage.Text} closed");
    };
    
    documentTabs.TabPages.Add(tab);
}

this.Controls.Add(documentTabs);
```

## Tooltip Support

Display tooltips when users hover over tabs.

### ShowToolTips Property

Enable tooltip display:

```csharp
// Enable tooltips
tabControlAdv1.ShowToolTips = true;

// Disable tooltips
tabControlAdv1.ShowToolTips = false;
```

### ToolTipText Property

Set tooltip text for individual tabs:

```csharp
// Set tooltip for each tab
tabPageAdv1.ToolTipText = "Home page with dashboard and overview";
tabPageAdv2.ToolTipText = "User settings and preferences";
tabPageAdv3.ToolTipText = "Application information and help";
```

### Complete Tooltip Example

```csharp
TabControlAdv tabsWithTooltips = new TabControlAdv();
tabsWithTooltips.ShowToolTips = true;
tabsWithTooltips.Size = new Size(600, 400);

// Tab 1
TabPageAdv homeTab = new TabPageAdv();
homeTab.Text = "Home";
homeTab.ToolTipText = "Dashboard with key metrics and recent activity";
tabsWithTooltips.TabPages.Add(homeTab);

// Tab 2
TabPageAdv dataTab = new TabPageAdv();
dataTab.Text = "Data";
dataTab.ToolTipText = "View and manage your data records";
tabsWithTooltips.TabPages.Add(dataTab);

// Tab 3
TabPageAdv reportsTab = new TabPageAdv();
reportsTab.Text = "Reports";
reportsTab.ToolTipText = "Generate and view reports (Ctrl+R)";
tabsWithTooltips.TabPages.Add(reportsTab);

this.Controls.Add(tabsWithTooltips);
```

## SuperTooltip Support

Display enhanced tooltips with rich content and formatting.

### ShowSuperToolTips Property

Enable SuperTooltips for the control:

```csharp
// Enable SuperTooltips
tabControlAdv1.ShowSuperToolTips = true;

// Disable SuperTooltips
tabControlAdv1.ShowSuperToolTips = false;
```

### SuperTooltip Property for Individual Tabs

Enable SuperTooltip for specific tabs:

```csharp
// Enable SuperTooltip for a tab
tabPageAdv1.SuperToolTip = superToolTip1;
```

**Note:** SuperTooltips provide richer formatting and content than standard tooltips. Configure SuperToolTip component separately.

### SuperTooltip Example

```csharp
// Create SuperToolTip component
SuperToolTip superToolTip1 = new SuperToolTip();

// Enable on control
tabControlAdv1.ShowSuperToolTips = true;

// Setup rich tooltip for tabs
TabPageAdv homeTab = new TabPageAdv();
homeTab.Text = "Home";

ToolTipInfo homeTooltip = new ToolTipInfo();
homeTooltip.Header.Text = "Home Dashboard";
homeTooltip.Body.Text = "View your personalized dashboard\nwith key metrics and alerts";
homeTooltip.Footer.Text = "Press F5 to refresh";
superToolTip1.SetToolTip(homeTab, homeTooltip);

tabControlAdv1.TabPages.Add(homeTab);
```

## Scroll Settings

Configure scrolling behavior when tabs exceed available space.

### ShowScroll Property

Enable scroll buttons:

```csharp
// Enable scroll buttons
tabControlAdv1.ShowScroll = true;

// Disable scroll buttons (tabs will be hidden)
tabControlAdv1.ShowScroll = false;
```

**When to use:** When you have many tabs and don't want multiline display.

### VSLikeScrollButton Property

Use Visual Studio-style scroll buttons:

```csharp
// VS-like scroll buttons
tabControlAdv1.ShowScroll = true;
tabControlAdv1.VSLikeScrollButton = true;

// Standard scroll buttons
tabControlAdv1.VSLikeScrollButton = false;
```

**Visual Studio style:** Provides familiar navigation for developer tools.

### ScrollIncrement Property

Control scroll behavior - by tabs or by pages:

```csharp
// Scroll one tab at a time
tabControlAdv1.ScrollIncrement = ScrollIncrement.Tab;

// Scroll one page at a time
tabControlAdv1.ScrollIncrement = ScrollIncrement.Page;
```

### ScrollBars for TabPages

Enable scrollbars within tab content areas:

```csharp
// Enable auto-scroll for a tab page
tabPageAdv1.AutoScroll = true;

// Set minimum size before scrollbars appear
tabPageAdv1.AutoScrollMinSize = new Size(800, 600);

// Set scroll margin
tabPageAdv1.AutoScrollMargin = new Size(20, 20);
```

**When to use:** Content in tab page exceeds visible area.

### BringSelectedTabToView Method

Programmatically scroll to ensure selected tab is visible:

```csharp
// Select a tab and bring it into view
tabControlAdv1.SelectedIndex = 10;
tabControlAdv1.BringSelectedTabToView();
```

### Complete Scrolling Example

```csharp
TabControlAdv scrollableTabs = new TabControlAdv();
scrollableTabs.Size = new Size(500, 400);

// Enable scrolling
scrollableTabs.ShowScroll = true;
scrollableTabs.VSLikeScrollButton = true;
scrollableTabs.ScrollIncrement = ScrollIncrement.Tab;
scrollableTabs.Multiline = false; // Single row with scrolling

// Add many tabs (will require scrolling)
for (int i = 1; i <= 20; i++)
{
    TabPageAdv tab = new TabPageAdv();
    tab.Text = $"Document {i}";
    
    // Enable scrollbars in tab content
    tab.AutoScroll = true;
    tab.AutoScrollMinSize = new Size(600, 500);
    
    // Add content that exceeds visible area
    Label label = new Label();
    label.Text = $"Content for Document {i}";
    label.Location = new Point(250, 250);
    tab.Controls.Add(label);
    
    scrollableTabs.TabPages.Add(tab);
}

// Bring a specific tab into view
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
        InitializeForm();
        SetupTabControl();
        AddInteractiveTabs();
    }
    
    private void InitializeForm()
    {
        this.Text = "Interactive Tabs Example";
        this.Size = new Size(800, 600);
    }
    
    private void SetupTabControl()
    {
        tabControl = new TabControlAdv();
        tabControl.Dock = DockStyle.Fill;
        
        // Enable all interactive features
        tabControl.ShowTabCloseButton = true;
        tabControl.ShowCloseButtonForActiveTabOnly = false;
        tabControl.CloseTabOnMiddleClick = true;
        tabControl.ShowToolTips = true;
        tabControl.ShowScroll = true;
        tabControl.VSLikeScrollButton = true;
        tabControl.ScrollIncrement = ScrollIncrement.Tab;
        
        this.Controls.Add(tabControl);
    }
    
    private void AddInteractiveTabs()
    {
        for (int i = 1; i <= 10; i++)
        {
            TabPageAdv tab = new TabPageAdv();
            tab.Text = $"Document {i}";
            tab.ToolTipText = $"Document {i} - Last modified: {DateTime.Now.ToShortDateString()}";
            
            // Add content
            Panel panel = new Panel();
            panel.Dock = DockStyle.Fill;
            panel.Padding = new Padding(20);
            
            Label title = new Label();
            title.Text = $"Document {i}";
            title.Font = new Font("Segoe UI", 16, FontStyle.Bold);
            title.Dock = DockStyle.Top;
            
            TextBox content = new TextBox();
            content.Multiline = true;
            content.Dock = DockStyle.Fill;
            content.Text = $"Content for document {i}...";
            
            panel.Controls.Add(content);
            panel.Controls.Add(title);
            tab.Controls.Add(panel);
            
            // Handle closing
            tab.Closing += OnTabClosing;
            tab.Closed += OnTabClosed;
            
            tabControl.TabPages.Add(tab);
        }
    }
    
    private void OnTabClosing(object sender, TabPageAdvClosingEventArgs e)
    {
        var tab = sender as TabPageAdv;
        var result = MessageBox.Show(
            $"Close {tab.Text}?",
            "Confirm",
            MessageBoxButtons.YesNo,
            MessageBoxIcon.Question);
        
        if (result == DialogResult.No)
        {
            e.Cancel = true;
        }
    }
    
    private void OnTabClosed(object sender, EventArgs e)
    {
        var tab = sender as TabPageAdv;
        Console.WriteLine($"{tab.Text} was closed");
        
        // If no tabs left, add a new one
        if (tabControl.TabPages.Count == 0)
        {
            AddNewTab();
        }
    }
    
    private void AddNewTab()
    {
        TabPageAdv newTab = new TabPageAdv();
        newTab.Text = "New Document";
        newTab.ToolTipText = "Empty document";
        tabControl.TabPages.Add(newTab);
    }
}
```

## Best Practices

### Close Buttons
- Use `ShowCloseButtonForActiveTabOnly` for less clutter
- Always handle `Closing` event to prevent data loss
- Provide visual feedback when close is prevented
- Consider adding "Close All" or "Close Others" in context menu

### Tooltips
- Keep tooltip text concise but informative
- Include keyboard shortcuts in tooltips
- Update tooltips when tab content changes
- Use SuperTooltips for complex information

### Scrolling
- Enable scrolling for more than 8-10 tabs
- Use `ScrollIncrement.Tab` for precise control
- Use `ScrollIncrement.Page` for faster navigation
- Consider multiline tabs as alternative to scrolling
- Test scrolling with various tab counts

### User Experience
- Middle-click closing is expected by many users
- Provide keyboard shortcuts (Ctrl+W, Ctrl+Tab)
- Show visual feedback for interactive elements
- Consider undo functionality for closed tabs
- Save and restore tab states across sessions

## Common Scenarios

### Scenario 1: Web Browser-Style Tabs

```csharp
tabControlAdv1.ShowTabCloseButton = true;
tabControlAdv1.CloseTabOnMiddleClick = true;
tabControlAdv1.ShowScroll = true;
tabControlAdv1.UserMoveTabs = true; // Drag to reorder
```

### Scenario 2: Document Editor

```csharp
tabControlAdv1.ShowTabCloseButton = true;
tabControlAdv1.ShowCloseButtonForActiveTabOnly = true;
tabControlAdv1.ShowToolTips = true;
// Handle Closing event to prompt for unsaved changes
```

### Scenario 3: Fixed Tab Set (No Closing)

```csharp
tabControlAdv1.ShowTabCloseButton = false;
tabControlAdv1.CloseTabOnMiddleClick = false;
tabControlAdv1.ShowToolTips = true;
tabControlAdv1.Multiline = true; // Show all tabs
```

### Scenario 4: Scrollable Dashboard

```csharp
tabControlAdv1.ShowScroll = true;
tabControlAdv1.VSLikeScrollButton = false;
tabControlAdv1.ScrollIncrement = ScrollIncrement.Page;
tabControlAdv1.ShowToolTips = true;
// Many tabs with auto-scroll content
```
