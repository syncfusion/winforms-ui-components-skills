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
```markdown
# Advanced Usage (Condensed)

This file summarizes advanced NavigationDrawer patterns. For full examples and API details, see the linked reference pages.

- Topics: dynamic items, lazy loading, simple virtualization, multi-drawer coordination, state persistence, accessibility hooks.

## Recommended Patterns

- Dynamic menus: add/remove `DrawerMenuItem` at runtime and keep a small header item.
- Lazy load pages/items on `Opening` to avoid startup cost.
- For very large lists, use simple pagination (`Load More` button) instead of rendering thousands of items.
- Coordinate multiple drawers by closing other drawers in the `Opening` event.
- Persist minimal state (open/position/selected item) to a small JSON file; avoid heavy dependencies — note `System.Text.Json` or `Newtonsoft.Json` must be added if used.

## Compact Examples

### Lazy load (minimal)

```csharp
private bool menuLoaded = false;
private async void NavigationDrawer_Opening(object sender, EventArgs e)
{
    if (menuLoaded) return;
    menuLoaded = true;
    navigationDrawer1.Items.Add(new DrawerMenuItem { Text = "Loading..." });
    var data = await Task.Run(() => FetchMenuData());
    navigationDrawer1.Items.Clear();
    foreach (var d in data) navigationDrawer1.Items.Add(new DrawerMenuItem { Text = d.Name });
}
```

### Save small state (minimal)

```csharp
public record DrawerState(bool IsOpen, SlidePosition Position, string Selected);
void SaveState(DrawerState s) => File.WriteAllText("drawer_state.json", System.Text.Json.JsonSerializer.Serialize(s));
```

## Links

- getting-started.md — basic setup
- drawer-features.md — properties and transitions
- customization.md — theming
- events.md — events and lifecycle

``` 
    DrawerHeader header = new DrawerHeader { Text = "Main Menu" };
