# Tab Navigation

Configure navigation controls (TabPrimitives) and keyboard navigation for TabControlAdv.

## TabPrimitives Overview

TabPrimitives are navigation controls that help users navigate through tabs efficiently. They provide buttons for first/last/next/previous navigation and additional features like dropdown lists and close buttons.

### Available TabPrimitive Types

- **FirstTab** - Navigates to the first tab
- **LastTab** - Navigates to the last tab
- **PreviousTab** - Navigates to the previous tab
- **NextTab** - Navigates to the next tab
- **PreviousPage** - Navigates to the previous page (group of tabs)
- **NextPage** - Navigates to the next page (group of tabs)
- **DropDown** - Shows dropdown list of all tabs for quick selection
- **Close** - Closes the TabControlAdv or current tab
- **Custom** - Custom button with user-defined behavior

## Creating TabPrimitives Through Designer

### Steps to Add TabPrimitives in Designer

1. Select the TabControlAdv control
2. Open the Properties window
3. Find `TabPrimitivesHost` property
4. Click on `TabPrimitives` collection property
5. Click the ellipsis button to open the Collection Editor
6. Click "Add" to create a new TabPrimitive
7. Set the `TabPrimitiveType` property to desired type
8. Configure other properties (Image, ToolTip, etc.)
9. Click OK

### Enable TabPrimitives Visibility

After adding TabPrimitives, make them visible:

```csharp
// Make TabPrimitives visible
tabControlAdv1.TabPrimitivesHost.Visible = true;
```

## Creating TabPrimitives Programmatically

### Basic TabPrimitive Creation

```csharp
// Enable TabPrimitivesHost
tabControlAdv1.TabPrimitivesHost.Visible = true;

// Add FirstTab primitive
tabControlAdv1.TabPrimitivesHost.TabPrimitives.Add(
    new TabPrimitive(
        TabPrimitiveType.FirstTab,    // Type
        null,                           // Image
        Color.Empty,                    // Color
        true,                           // Visible
        1,                              // DisplayOrder
        "FirstTab"                      // Name
    )
);

// Add LastTab primitive
tabControlAdv1.TabPrimitivesHost.TabPrimitives.Add(
    new TabPrimitive(
        TabPrimitiveType.LastTab,
        null,
        Color.Empty,
        true,
        2,
        "LastTab"
    )
);
```

### Complete Navigation Set

```csharp
// Enable host
tabControlAdv1.TabPrimitivesHost.Visible = true;

// Add common navigation buttons
var primitives = tabControlAdv1.TabPrimitivesHost.TabPrimitives;

// First
primitives.Add(new TabPrimitive(
    TabPrimitiveType.FirstTab, null, Color.Empty, true, 1, "First"));

// Previous
primitives.Add(new TabPrimitive(
    TabPrimitiveType.PreviousTab, null, Color.Empty, true, 2, "Previous"));

// Next
primitives.Add(new TabPrimitive(
    TabPrimitiveType.NextTab, null, Color.Empty, true, 3, "Next"));

// Last
primitives.Add(new TabPrimitive(
    TabPrimitiveType.LastTab, null, Color.Empty, true, 4, "Last"));

// Dropdown for quick selection
primitives.Add(new TabPrimitive(
    TabPrimitiveType.DropDown, null, Color.Empty, true, 5, "DropDown"));

// Close button
primitives.Add(new TabPrimitive(
    TabPrimitiveType.Close, null, Color.Empty, true, 6, "Close"));
```

## TabPrimitive Features

### Adding Images to TabPrimitives

```csharp
// Create ImageList with navigation icons
ImageList navIcons = new ImageList();
navIcons.ImageSize = new Size(16, 16);
navIcons.Images.Add("first", Properties.Resources.FirstIcon);
navIcons.Images.Add("previous", Properties.Resources.PreviousIcon);
navIcons.Images.Add("next", Properties.Resources.NextIcon);
navIcons.Images.Add("last", Properties.Resources.LastIcon);

// Add primitive with image
TabPrimitive firstTab = new TabPrimitive(
    TabPrimitiveType.FirstTab,
    navIcons.Images["first"],  // Set image
    Color.Empty,
    true,
    1,
    "FirstWithIcon"
);

tabControlAdv1.TabPrimitivesHost.TabPrimitives.Add(firstTab);
```

### Adding ToolTips to TabPrimitives

Configure tooltips through the TabPrimitives Collection Editor or programmatically:

```csharp
// Access primitive and set tooltip
var firstPrimitive = tabControlAdv1.TabPrimitivesHost.TabPrimitives[0];
firstPrimitive.ToolTip = "Go to first tab (Ctrl+Home)";

var lastPrimitive = tabControlAdv1.TabPrimitivesHost.TabPrimitives[1];
lastPrimitive.ToolTip = "Go to last tab (Ctrl+End)";
```

### Visibility Control

Show or hide specific primitives:

```csharp
// Hide a specific primitive
tabControlAdv1.TabPrimitivesHost.TabPrimitives[0].Visible = false;

// Show all primitives
foreach (TabPrimitive primitive in tabControlAdv1.TabPrimitivesHost.TabPrimitives)
{
    primitive.Visible = true;
}

// Conditional visibility
if (tabControlAdv1.TabPages.Count > 10)
{
    // Show dropdown when many tabs
    dropDownPrimitive.Visible = true;
}
```

## Handling TabPrimitive Click Events

### TabPrimitiveClick Event

React when a navigation button is clicked:

```csharp
tabControlAdv1.TabPrimitiveClick += (sender, e) =>
{
    Console.WriteLine($"Clicked: {e.TabPrimitive.Name}");
    
    // You can cancel the default action
    // e.Cancel = true;
};
```

### Custom TabPrimitive Implementation

```csharp
// Add custom primitive
TabPrimitive customPrimitive = new TabPrimitive(
    TabPrimitiveType.Custom,
    Properties.Resources.CustomIcon,
    Color.Empty,
    true,
    10,
    "CustomAbout"
);

tabControlAdv1.TabPrimitivesHost.TabPrimitives.Add(customPrimitive);

// Handle click
tabControlAdv1.TabPrimitiveClick += (sender, e) =>
{
    if (e.TabPrimitive.Name == "CustomAbout")
    {
        // Show custom dialog
        MessageBox.Show("About this application...", "About");
        
        // Cancel default behavior
        e.Cancel = true;
    }
};
```

## Page vs Tab Navigation

### PreviousPage and NextPage

Navigate by pages (groups of visible tabs):

```csharp
// Add page navigation
tabControlAdv1.TabPrimitivesHost.TabPrimitives.Add(
    new TabPrimitive(
        TabPrimitiveType.PreviousPage, 
        null, 
        Color.Empty, 
        true, 
        1, 
        "PrevPage"));

tabControlAdv1.TabPrimitivesHost.TabPrimitives.Add(
    new TabPrimitive(
        TabPrimitiveType.NextPage, 
        null, 
        Color.Empty, 
        true, 
        2, 
        "NextPage"));
```

**When to use:** When you have scrollable tabs and want to jump by visible page instead of single tab.

## Keyboard Navigation

### SwitchPagesForDialogKeys

Enable Ctrl+Tab keyboard navigation:

```csharp
// Enable Ctrl+Tab navigation
tabControlAdv1.SwitchPagesForDialogKeys = true;
```

**Keyboard shortcuts:**
- `Ctrl+Tab` - Next tab
- `Ctrl+Shift+Tab` - Previous tab

### HitTestTabs Method

Programmatically determine which tab is at a specific location:

```csharp
// Get tab at mouse position
Point mousePos = tabControlAdv1.PointToClient(Cursor.Position);
int tabIndex = tabControlAdv1.HitTestTabs(mousePos);

if (tabIndex >= 0)
{
    Console.WriteLine($"Mouse over tab: {tabControlAdv1.TabPages[tabIndex].Text}");
}
```

## Complete Navigation Example

```csharp
public class NavigationExample : Form
{
    private TabControlAdv tabControl;
    
    public NavigationExample()
    {
        InitializeForm();
        SetupTabControl();
        SetupNavigation();
        AddTabs();
    }
    
    private void InitializeForm()
    {
        this.Text = "Tab Navigation Example";
        this.Size = new Size(800, 600);
    }
    
    private void SetupTabControl()
    {
        tabControl = new TabControlAdv();
        tabControl.Dock = DockStyle.Fill;
        tabControl.SwitchPagesForDialogKeys = true;
        this.Controls.Add(tabControl);
    }
    
    private void SetupNavigation()
    {
        // Create icon set
        ImageList icons = new ImageList();
        icons.ImageSize = new Size(16, 16);
        icons.Images.Add("first", CreateArrowBitmap("<<"));
        icons.Images.Add("prev", CreateArrowBitmap("<"));
        icons.Images.Add("next", CreateArrowBitmap(">"));
        icons.Images.Add("last", CreateArrowBitmap(">>"));
        
        // Enable primitives
        tabControl.TabPrimitivesHost.Visible = true;
        
        // Add navigation controls
        AddPrimitive(TabPrimitiveType.FirstTab, icons.Images["first"], 
            "Go to first tab", "First");
        AddPrimitive(TabPrimitiveType.PreviousTab, icons.Images["prev"], 
            "Previous tab (Ctrl+Shift+Tab)", "Previous");
        AddPrimitive(TabPrimitiveType.NextTab, icons.Images["next"], 
            "Next tab (Ctrl+Tab)", "Next");
        AddPrimitive(TabPrimitiveType.LastTab, icons.Images["last"], 
            "Go to last tab", "Last");
        AddPrimitive(TabPrimitiveType.DropDown, null, 
            "Show all tabs", "DropDown");
        
        // Custom refresh button
        TabPrimitive refresh = new TabPrimitive(
            TabPrimitiveType.Custom,
            CreateRefreshIcon(),
            Color.Empty,
            true,
            10,
            "Refresh"
        );
        refresh.ToolTip = "Refresh current tab";
        tabControl.TabPrimitivesHost.TabPrimitives.Add(refresh);
        
        // Handle clicks
        tabControl.TabPrimitiveClick += OnPrimitiveClick;
    }
    
    private void AddPrimitive(TabPrimitiveType type, Image image, 
        string tooltip, string name)
    {
        TabPrimitive primitive = new TabPrimitive(
            type, image, Color.Empty, true, 
            tabControl.TabPrimitivesHost.TabPrimitives.Count, name);
        primitive.ToolTip = tooltip;
        tabControl.TabPrimitivesHost.TabPrimitives.Add(primitive);
    }
    
    private void OnPrimitiveClick(object sender, TabPrimitiveClickEventArgs e)
    {
        if (e.TabPrimitive.Name == "Refresh")
        {
            RefreshCurrentTab();
            e.Cancel = true;
        }
    }
    
    private void RefreshCurrentTab()
    {
        var currentTab = tabControl.SelectedTab;
        MessageBox.Show($"Refreshing {currentTab.Text}...");
    }
    
    private void AddTabs()
    {
        for (int i = 1; i <= 15; i++)
        {
            TabPageAdv tab = new TabPageAdv();
            tab.Text = $"Page {i}";
            
            Label label = new Label();
            label.Text = $"Content for Page {i}";
            label.Font = new Font("Segoe UI", 14);
            label.Location = new Point(50, 50);
            label.AutoSize = true;
            
            tab.Controls.Add(label);
            tabControl.TabPages.Add(tab);
        }
    }
    
    private Bitmap CreateArrowBitmap(string text)
    {
        Bitmap bmp = new Bitmap(16, 16);
        using (Graphics g = Graphics.FromImage(bmp))
        {
            g.Clear(Color.Transparent);
            g.DrawString(text, new Font("Arial", 8, FontStyle.Bold), 
                Brushes.Black, new PointF(0, 0));
        }
        return bmp;
    }
    
    private Bitmap CreateRefreshIcon()
    {
        Bitmap bmp = new Bitmap(16, 16);
        using (Graphics g = Graphics.FromImage(bmp))
        {
            g.Clear(Color.Transparent);
            g.DrawEllipse(Pens.Blue, 2, 2, 12, 12);
            g.DrawString("R", new Font("Arial", 8, FontStyle.Bold), 
                Brushes.Blue, new PointF(4, 2));
        }
        return bmp;
    }
}
```

## Best Practices

### Navigation Design
- Add First/Last for many tabs (>10)
- Add DropDown for quick access (>5 tabs)
- Use Previous/Next for minimal navigation
- Consider page navigation for very long tab lists

### Visual Clarity
- Use clear, recognizable icons
- Provide descriptive tooltips
- Maintain consistent icon sizes (16x16)
- Test with various tab counts

### Custom Primitives
- Use for application-specific actions
- Handle TabPrimitiveClick event properly
- Cancel default behavior with e.Cancel = true
- Provide clear visual feedback

### Keyboard Support
- Enable SwitchPagesForDialogKeys for power users
- Document keyboard shortcuts in tooltips
- Support standard shortcuts (Ctrl+Tab, etc.)
- Consider custom keyboard handlers for complex navigation

## Common Scenarios

### Scenario 1: Minimal Navigation (Few Tabs)

```csharp
// Just Previous/Next for 3-8 tabs
tabControlAdv1.TabPrimitivesHost.Visible = true;
AddPrimitive(TabPrimitiveType.PreviousTab);
AddPrimitive(TabPrimitiveType.NextTab);
```

### Scenario 2: Full Navigation (Many Tabs)

```csharp
// Complete navigation set for 10+ tabs
tabControlAdv1.TabPrimitivesHost.Visible = true;
AddPrimitive(TabPrimitiveType.FirstTab);
AddPrimitive(TabPrimitiveType.PreviousTab);
AddPrimitive(TabPrimitiveType.NextTab);
AddPrimitive(TabPrimitiveType.LastTab);
AddPrimitive(TabPrimitiveType.DropDown);
```

### Scenario 3: Document Editor Navigation

```csharp
// Navigation with close button
tabControlAdv1.TabPrimitivesHost.Visible = true;
AddPrimitive(TabPrimitiveType.PreviousTab);
AddPrimitive(TabPrimitiveType.NextTab);
AddPrimitive(TabPrimitiveType.DropDown);
AddPrimitive(TabPrimitiveType.Close);
tabControl.SwitchPagesForDialogKeys = true;
```

### Scenario 4: Custom Actions

```csharp
// Add custom primitives for app-specific actions
tabControlAdv1.TabPrimitivesHost.Visible = true;
AddCustomPrimitive("Save", SaveIcon, OnSaveClick);
AddCustomPrimitive("Print", PrintIcon, OnPrintClick);
AddCustomPrimitive("Settings", SettingsIcon, OnSettingsClick);
```
