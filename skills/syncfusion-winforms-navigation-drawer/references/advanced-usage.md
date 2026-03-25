# Advanced Usage

This guide covers advanced scenarios and best practices for implementing complex Navigation Drawer functionality in Windows Forms applications.

## ContentViewContainer Configuration

The ContentViewContainer is the primary area where your main application content resides. Advanced configuration allows for rich, interactive content layouts.

### Rich Content Integration

Integrate complex controls into the ContentView:

```csharp
// Create a comprehensive content layout
private void ConfigureAdvancedContentView()
{
    // Main container panel
    Panel contentPanel = new Panel();
    contentPanel.Dock = DockStyle.Fill;
    contentPanel.BackColor = Color.White;
    
    // Header section
    Panel headerPanel = new Panel();
    headerPanel.Dock = DockStyle.Top;
    headerPanel.Height = 60;
    headerPanel.BackColor = Color.FromArgb(240, 240, 240);
    
    Label titleLabel = new Label();
    titleLabel.Text = "Dashboard";
    titleLabel.Font = new Font("Segoe UI", 18, FontStyle.Bold);
    titleLabel.Location = new Point(20, 15);
    titleLabel.AutoSize = true;
    headerPanel.Controls.Add(titleLabel);
    
    // Content area with multiple controls
    TabControl tabControl = new TabControl();
    tabControl.Dock = DockStyle.Fill;
    
    TabPage page1 = new TabPage("Overview");
    TabPage page2 = new TabPage("Details");
    TabPage page3 = new TabPage("Settings");
    
    tabControl.TabPages.Add(page1);
    tabControl.TabPages.Add(page2);
    tabControl.TabPages.Add(page3);
    
    // Add to content panel
    contentPanel.Controls.Add(tabControl);
    contentPanel.Controls.Add(headerPanel);
    
    // Add to NavigationDrawer
    navigationDrawer1.ContentViewContainer.Controls.Add(contentPanel);
}
```

### Responsive Content Layout

Create layouts that adapt to drawer state:

```csharp
private void SetupResponsiveContent()
{
    TableLayoutPanel layout = new TableLayoutPanel();
    layout.Dock = DockStyle.Fill;
    layout.ColumnCount = 2;
    layout.RowCount = 2;
    
    // Initial layout
    layout.ColumnStyles.Add(new ColumnStyle(SizeType.Percent, 50));
    layout.ColumnStyles.Add(new ColumnStyle(SizeType.Percent, 50));
    
    navigationDrawer1.ContentViewContainer.Controls.Add(layout);
    
    // Adjust layout when drawer opens/closes
    navigationDrawer1.Opened += (s, e) =>
    {
        if (navigationDrawer1.Transition == Transition.Push)
        {
            // Rearrange content for narrower space
            layout.ColumnCount = 1;
            layout.ColumnStyles.Clear();
            layout.ColumnStyles.Add(new ColumnStyle(SizeType.Percent, 100));
        }
    };
    
    navigationDrawer1.Closed += (s, e) =>
    {
        // Restore original layout
        layout.ColumnCount = 2;
        layout.ColumnStyles.Clear();
        layout.ColumnStyles.Add(new ColumnStyle(SizeType.Percent, 50));
        layout.ColumnStyles.Add(new ColumnStyle(SizeType.Percent, 50));
    };
}
```

## Dynamic Item Management

Add, remove, and update drawer items at runtime based on application state.

### Adding Items Dynamically

```csharp
// Add menu items based on user permissions
private void LoadMenuItemsByPermissions(UserPermissions permissions)
{
    // Clear existing items except header
    var header = navigationDrawer1.Items.OfType<DrawerHeader>().FirstOrDefault();
    navigationDrawer1.Items.Clear();
    
    if (header != null)
    {
        navigationDrawer1.Items.Add(header);
    }
    
    // Add items based on permissions
    if (permissions.CanViewDashboard)
    {
        AddMenuItem("Dashboard", Properties.Resources.DashboardIcon);
    }
    
    if (permissions.CanViewReports)
    {
        AddMenuItem("Reports", Properties.Resources.ReportsIcon);
    }
    
    if (permissions.IsAdmin)
    {
        AddMenuItem("Administration", Properties.Resources.AdminIcon);
    }
    
    AddMenuItem("Settings", Properties.Resources.SettingsIcon); // Always available
}

private void AddMenuItem(string text, Image icon)
{
    DrawerMenuItem item = new DrawerMenuItem();
    item.Text = text;
    item.Image = icon;
    item.TextImageRelation = TextImageRelation.ImageBeforeText;
    navigationDrawer1.Items.Add(item);
}
```

### Conditional Item Visibility

```csharp
// Show/hide items based on application state
private void UpdateMenuVisibility(ApplicationState state)
{
    foreach (var item in navigationDrawer1.Items.OfType<DrawerMenuItem>())
    {
        switch (item.Text)
        {
            case "Offline Mode":
                item.Visible = !state.IsOnline;
                break;
            
            case "Sync":
                item.Visible = state.IsOnline && state.HasLocalChanges;
                break;
            
            case "Admin Panel":
                item.Visible = state.CurrentUser.IsAdmin;
                break;
        }
    }
}
```

### Item Badges and Notifications

```csharp
// Add notification badges to menu items
private void AddNotificationBadge(DrawerMenuItem item, int count)
{
    if (count > 0)
    {
        // Update item text with badge
        string originalText = item.Text.Split('(')[0].Trim();
        item.Text = $"{originalText} ({count})";
        
        // Highlight item
        item.BackColor = Color.LightYellow;
    }
}

// Update badges periodically
private async Task UpdateNotificationBadges()
{
    while (true)
    {
        var messagesItem = navigationDrawer1.Items.OfType<DrawerMenuItem>()
            .FirstOrDefault(i => i.Text.StartsWith("Messages"));
        
        if (messagesItem != null)
        {
            int unreadCount = await GetUnreadMessageCount();
            AddNotificationBadge(messagesItem, unreadCount);
        }
        
        await Task.Delay(TimeSpan.FromSeconds(30));
    }
}
```

## Complex Item Hierarchies

Create nested menu structures with expandable sections.

### Simulating Nested Menus

```csharp
// Create a hierarchical menu structure
private void CreateHierarchicalMenu()
{
    DrawerHeader header = new DrawerHeader { Text = "Main Menu" };
    navigationDrawer1.Items.Add(header);
    
    // Parent item
    DrawerMenuItem reportsParent = new DrawerMenuItem();
    reportsParent.Text = "▶ Reports";
    reportsParent.Click += (s, e) => ToggleSubMenu(reportsParent);
    navigationDrawer1.Items.Add(reportsParent);
    
    // Store sub-items with tag
    var subItems = new List<DrawerMenuItem>
    {
        new DrawerMenuItem { Text = "  • Sales Report" },
        new DrawerMenuItem { Text = "  • Financial Report" },
        new DrawerMenuItem { Text = "  • Inventory Report" }
    };
    
    reportsParent.Tag = new { IsExpanded = false, SubItems = subItems };
}

private void ToggleSubMenu(DrawerMenuItem parentItem)
{
    dynamic data = parentItem.Tag;
    bool isExpanded = data.IsExpanded;
    List<DrawerMenuItem> subItems = data.SubItems;
    
    if (isExpanded)
    {
        // Collapse: remove sub-items
        foreach (var subItem in subItems)
        {
            navigationDrawer1.Items.Remove(subItem);
        }
        parentItem.Text = parentItem.Text.Replace("▼", "▶");
        data.IsExpanded = false;
    }
    else
    {
        // Expand: add sub-items
        int insertIndex = navigationDrawer1.Items.IndexOf(parentItem) + 1;
        foreach (var subItem in subItems)
        {
            navigationDrawer1.Items.Insert(insertIndex++, subItem);
        }
        parentItem.Text = parentItem.Text.Replace("▶", "▼");
        data.IsExpanded = true;
    }
}
```

### Categorized Menu Sections

```csharp
// Create menu with visual separators
private void CreateCategorizedMenu()
{
    AddMenuCategory("Main");
    AddMenuItem("Dashboard");
    AddMenuItem("Projects");
    
    AddMenuSeparator();
    
    AddMenuCategory("Reports");
    AddMenuItem("Sales");
    AddMenuItem("Analytics");
    
    AddMenuSeparator();
    
    AddMenuCategory("Settings");
    AddMenuItem("Preferences");
    AddMenuItem("Account");
}

private void AddMenuCategory(string categoryName)
{
    DrawerMenuItem category = new DrawerMenuItem();
    category.Text = categoryName.ToUpper();
    category.ForeColor = Color.Gray;
    category.Font = new Font("Segoe UI", 8, FontStyle.Bold);
    category.Enabled = false; // Non-clickable
    navigationDrawer1.Items.Add(category);
}

private void AddMenuSeparator()
{
    DrawerMenuItem separator = new DrawerMenuItem();
    separator.Height = 1;
    separator.BackColor = Color.LightGray;
    separator.Enabled = false;
    navigationDrawer1.Items.Add(separator);
}
```

## Performance Optimization

Optimize drawer performance for large menus and complex layouts.

### Lazy Loading Menu Items

```csharp
// Load menu items only when drawer opens
private bool menuLoaded = false;

private void NavigationDrawer1_Opening(object sender, OpeningEventArgs e)
{
    if (!menuLoaded)
    {
        LoadMenuItemsAsync();
        menuLoaded = true;
    }
}

private async void LoadMenuItemsAsync()
{
    // Show loading indicator
    var loadingItem = new DrawerMenuItem { Text = "Loading..." };
    navigationDrawer1.Items.Add(loadingItem);
    
    // Simulate async data loading
    var menuData = await Task.Run(() => FetchMenuDataFromServer());
    
    // Remove loading indicator
    navigationDrawer1.Items.Remove(loadingItem);
    
    // Add actual menu items
    foreach (var data in menuData)
    {
        AddMenuItem(data.Name, data.Icon);
    }
}
```

### Virtual Scrolling for Large Menus

```csharp
// Implement pagination for very large menus
private const int ItemsPerPage = 20;
private int currentPage = 0;
private List<MenuItemData> allMenuItems;

private void LoadMenuPage(int pageNumber)
{
    int startIndex = pageNumber * ItemsPerPage;
    int endIndex = Math.Min(startIndex + ItemsPerPage, allMenuItems.Count);
    
    // Clear current items (except header)
    var header = navigationDrawer1.Items.OfType<DrawerHeader>().FirstOrDefault();
    navigationDrawer1.Items.Clear();
    if (header != null) navigationDrawer1.Items.Add(header);
    
    // Add page items
    for (int i = startIndex; i < endIndex; i++)
    {
        AddMenuItem(allMenuItems[i].Name, allMenuItems[i].Icon);
    }
    
    // Add pagination controls
    if (endIndex < allMenuItems.Count)
    {
        var moreButton = new DrawerMenuItem { Text = "Load More..." };
        moreButton.Click += (s, e) => LoadMenuPage(++currentPage);
        navigationDrawer1.Items.Add(moreButton);
    }
}
```

### Caching and Reuse

```csharp
// Cache menu item instances for reuse
private Dictionary<string, DrawerMenuItem> menuItemCache = 
    new Dictionary<string, DrawerMenuItem>();

private DrawerMenuItem GetOrCreateMenuItem(string key, string text)
{
    if (!menuItemCache.ContainsKey(key))
    {
        menuItemCache[key] = new DrawerMenuItem { Text = text };
    }
    return menuItemCache[key];
}
```

## Integration with Other Controls

Coordinate the Navigation Drawer with other application components.

### Drawer with Split Container

```csharp
// Use drawer with split container for advanced layouts
private void SetupDrawerWithSplitContainer()
{
    SplitContainer splitContainer = new SplitContainer();
    splitContainer.Dock = DockStyle.Fill;
    splitContainer.Orientation = Orientation.Horizontal;
    
    // Top panel: main content
    splitContainer.Panel1.Controls.Add(mainContentControl);
    
    // Bottom panel: details
    splitContainer.Panel2.Controls.Add(detailsControl);
    
    navigationDrawer1.ContentViewContainer.Controls.Add(splitContainer);
    
    // Adjust splitter when drawer state changes
    navigationDrawer1.Opened += (s, e) =>
    {
        if (navigationDrawer1.Transition == Transition.Push)
        {
            splitContainer.Orientation = Orientation.Vertical;
        }
    };
}
```

### Drawer with Status Bar

```csharp
// Coordinate drawer with status bar
private void SetupDrawerWithStatusBar()
{
    StatusStrip statusStrip = new StatusStrip();
    ToolStripStatusLabel statusLabel = new ToolStripStatusLabel();
    statusStrip.Items.Add(statusLabel);
    
    navigationDrawer1.Opening += (s, e) =>
    {
        statusLabel.Text = "Opening navigation menu...";
    };
    
    navigationDrawer1.Opened += (s, e) =>
    {
        statusLabel.Text = "Navigation menu open";
    };
    
    navigationDrawer1.Closed += (s, e) =>
    {
        statusLabel.Text = "Ready";
    };
    
    this.Controls.Add(statusStrip);
}
```

### Multi-Drawer Configuration

```csharp
// Use multiple drawers from different positions
private void SetupMultipleDrawers()
{
    // Left drawer for navigation
    NavigationDrawer leftDrawer = new NavigationDrawer();
    leftDrawer.Position = SlidePosition.Left;
    leftDrawer.DrawerWidth = 250;
    leftDrawer.DrawerHeight = this.Height;
    this.Controls.Add(leftDrawer);
    
    // Right drawer for settings
    NavigationDrawer rightDrawer = new NavigationDrawer();
    rightDrawer.Position = SlidePosition.Right;
    rightDrawer.DrawerWidth = 300;
    rightDrawer.DrawerHeight = this.Height;
    this.Controls.Add(rightDrawer);
    
    // Ensure only one drawer is open at a time
    leftDrawer.Opening += (s, e) =>
    {
        if (IsDrawerOpen(rightDrawer))
        {
            rightDrawer.ToggleDrawer();
        }
    };
    
    rightDrawer.Opening += (s, e) =>
    {
        if (IsDrawerOpen(leftDrawer))
        {
            leftDrawer.ToggleDrawer();
        }
    };
}

private bool IsDrawerOpen(NavigationDrawer drawer)
{
    // Implementation to check drawer state
    // Could use a flag set in Opened/Closed events
    return false;
}
```

## Best Practices

### State Management

```csharp
// Maintain drawer state across sessions
public class DrawerState
{
    public bool IsOpen { get; set; }
    public string SelectedMenuItem { get; set; }
    public SlidePosition Position { get; set; }
    public NavigationDrawerStyle Theme { get; set; }
}

private void SaveDrawerState()
{
    var state = new DrawerState
    {
        IsOpen = isDrawerOpen,
        SelectedMenuItem = selectedMenuItem?.Text,
        Position = navigationDrawer1.Position,
        Theme = navigationDrawer1.Style
    };
    
    string json = JsonConvert.SerializeObject(state);
    File.WriteAllText("drawer_state.json", json);
}

private void LoadDrawerState()
{
    if (File.Exists("drawer_state.json"))
    {
        string json = File.ReadAllText("drawer_state.json");
        var state = JsonConvert.DeserializeObject<DrawerState>(json);
        
        navigationDrawer1.Position = state.Position;
        navigationDrawer1.Style = state.Theme;
        
        // Restore other state properties
    }
}
```

### Error Handling

```csharp
// Robust error handling for drawer operations
private void SafeToggleDrawer()
{
    try
    {
        navigationDrawer1.ToggleDrawer();
    }
    catch (InvalidOperationException ex)
    {
        Logger.LogError("Failed to toggle drawer", ex);
        MessageBox.Show("Unable to open navigation menu. Please try again.",
            "Error", MessageBoxButtons.OK, MessageBoxIcon.Error);
    }
}
```

### Accessibility

```csharp
// Enhance accessibility
private void ConfigureAccessibility()
{
    // Keyboard shortcuts
    this.KeyPreview = true;
    this.KeyDown += (s, e) =>
    {
        if (e.Control && e.KeyCode == Keys.M)
        {
            navigationDrawer1.ToggleDrawer();
        }
    };
    
    // Screen reader support
    navigationDrawer1.AccessibleName = "Main Navigation Drawer";
    navigationDrawer1.AccessibleDescription = "Contains application navigation menu";
    
    // High contrast mode detection
    if (SystemInformation.HighContrast)
    {
        navigationDrawer1.Style = NavigationDrawerStyle.Default;
        ApplyHighContrastColors();
    }
}
```

## Troubleshooting

### Drawer Not Responding to ToggleDrawer

**Problem:** Drawer doesn't open/close when calling `ToggleDrawer()`.

**Solution:** Ensure the drawer is properly initialized and added to the form:
```csharp
// Verify drawer is in form's control collection
if (!this.Controls.Contains(navigationDrawer1))
{
    this.Controls.Add(navigationDrawer1);
}
```

### Performance Issues with Large Menus

**Problem:** Slow performance with many menu items.

**Solution:** Implement lazy loading and virtualization (see [Performance Optimization](#performance-optimization)).

### Content Not Updating

**Problem:** ContentView doesn't reflect changes after update.

**Solution:** Force refresh:
```csharp
navigationDrawer1.ContentViewContainer.Refresh();
navigationDrawer1.Invalidate();
```

## Next Steps

- **Getting started:** See [getting-started.md](getting-started.md) for basic setup
- **Drawer features:** See [drawer-features.md](drawer-features.md) for core features
- **Customization:** See [customization.md](customization.md) for theming
- **Events:** See [events.md](events.md) for event handling
