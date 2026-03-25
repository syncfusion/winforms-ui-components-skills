# Customization and Editing

## Table of Contents
- [Renaming TabItems](#renaming-tabitems)
- [Moving TabItems](#moving-tabitems)
- [Padding Settings](#padding-settings)
- [UseMnemonic Property](#usemnemonic-property)
- [TabPage Border Settings](#tabpage-border-settings)
- [Image Settings](#image-settings)
- [Preventing Specific Tab Movement](#preventing-specific-tab-movement)

Learn how to enable runtime customization features like editing tab names, drag-and-drop reordering, and advanced customization options.

## Renaming TabItems

Enable users to edit tab text at runtime, similar to Microsoft Excel worksheet tabs.

### LabelEdit Property

```csharp
// Enable label editing
tabControlAdv1.LabelEdit = true;

// Disable label editing
tabControlAdv1.LabelEdit = false;
```

### How Users Edit Tab Text

When `LabelEdit` is enabled, users can edit tab text in three ways:

**Method 1: Double-click**
- Double-click on the tab text
- Text enters edit mode
- Type new name
- Press Enter or click elsewhere to save

**Method 2: Right-click**
- Right-click on the tab
- Text automatically enters edit mode
- Type new name
- Press Enter or click elsewhere to save

**Method 3: Programmatic**
```csharp
// Change tab text programmatically
tabPageAdv1.Text = "New Name";
```

### Edit Events

Handle edit events to validate or respond to text changes:

```csharp
// Before editing starts
tabControlAdv1.BeforeEdit += (sender, e) =>
{
    Console.WriteLine($"Editing tab: {e.EditText}");
    
    // Cancel edit for specific tabs
    if (e.EditText == "Protected Tab")
    {
        MessageBox.Show("This tab cannot be renamed");
        // Note: Cannot cancel BeforeEdit, use LabelEdit property instead
    }
};

// After editing completes
tabControlAdv1.AfterEdit += (sender, e) =>
{
    Console.WriteLine($"New name: {e.EditText}");
    
    // Validate new name
    if (string.IsNullOrWhiteSpace(e.EditText))
    {
        MessageBox.Show("Tab name cannot be empty");
        // Revert or set default name
    }
};
```

### LabelEditTextChanged Event

Track text changes during editing:

```csharp
tabControlAdv1.LabelEditTextChanged += (sender, e) =>
{
    Console.WriteLine("Tab text is being edited");
};
```

### Complete Example

```csharp
TabControlAdv editableTabs = new TabControlAdv();
editableTabs.Dock = DockStyle.Fill;
editableTabs.LabelEdit = true;

// Add tabs
for (int i = 1; i <= 5; i++)
{
    TabPageAdv tab = new TabPageAdv();
    tab.Text = $"Tab {i}";
    editableTabs.TabPages.Add(tab);
}

// Handle editing events
editableTabs.BeforeEdit += (sender, e) =>
{
    Console.WriteLine($"Starting edit: {e.EditText}");
};

editableTabs.AfterEdit += (sender, e) =>
{
    // Validate and format
    string newText = e.EditText.Trim();
    if (string.IsNullOrEmpty(newText))
    {
        MessageBox.Show("Tab name cannot be empty", "Invalid Name");
    }
    else
    {
        Console.WriteLine($"Tab renamed to: {newText}");
    }
};

this.Controls.Add(editableTabs);
```

## Moving TabItems

Enable drag-and-drop reordering of tabs.

### UserMoveTabs Property

```csharp
// Enable drag-and-drop reordering
tabControlAdv1.UserMoveTabs = true;

// Disable moving tabs
tabControlAdv1.UserMoveTabs = false;
```

### How Users Move Tabs

When `UserMoveTabs` is enabled:
1. Click and hold on a tab
2. Drag the tab to a new position
3. Release to drop in new location
4. Tab order is updated automatically

### TabsOrderChanged Event

React when tab order changes:

```csharp
tabControlAdv1.TabsOrderChanged += (sender, e) =>
{
    Console.WriteLine("Tab order changed");
    
    // Log new order
    for (int i = 0; i < tabControlAdv1.TabPages.Count; i++)
    {
        Console.WriteLine($"Position {i}: {tabControlAdv1.TabPages[i].Text}");
    }
    
    // Save new order to settings
    SaveTabOrder();
};
```

### TabMoving Event

Handle or cancel tab movement:

```csharp
tabControlAdv1.TabMoving += (sender, e) =>
{
    Console.WriteLine($"Moving tab from {e.From} to {e.Target}");
    
    // Prevent moving specific tab (e.g., index 0)
    if (e.From == 0 || e.Target == 0)
    {
        e.Cancel = true;
        MessageBox.Show("The first tab cannot be moved");
    }
};
```

### Complete Example

```csharp
TabControlAdv movableTabs = new TabControlAdv();
movableTabs.Dock = DockStyle.Fill;
movableTabs.UserMoveTabs = true;

// Add tabs
string[] tabNames = { "Home", "Data", "Charts", "Reports", "Settings" };
foreach (string name in tabNames)
{
    TabPageAdv tab = new TabPageAdv();
    tab.Text = name;
    movableTabs.TabPages.Add(tab);
}

// Track order changes
movableTabs.TabsOrderChanged += (sender, e) =>
{
    Console.WriteLine("New tab order:");
    for (int i = 0; i < movableTabs.TabPages.Count; i++)
    {
        Console.WriteLine($"  {i + 1}. {movableTabs.TabPages[i].Text}");
    }
};

this.Controls.Add(movableTabs);
```

## Padding Settings

Control spacing around text and images in tabs.

### Padding Property

```csharp
// Set padding (X-axis, Y-axis)
tabControlAdv1.Padding = new Point(12, 12);

// More horizontal padding
tabControlAdv1.Padding = new Point(20, 8);

// Minimal padding
tabControlAdv1.Padding = new Point(4, 4);
```

**Effect:** Increases clickable area and visual comfort.

### Example with Different Padding

```csharp
// Compact tabs
TabControlAdv compactTabs = new TabControlAdv();
compactTabs.Padding = new Point(6, 3);

// Spacious tabs
TabControlAdv spaciousTabs = new TabControlAdv();
spaciousTabs.Padding = new Point(20, 10);
```

## UseMnemonic Property

Control whether ampersand (&) characters are interpreted as keyboard shortcuts.

### UseMnemonic Property

```csharp
// Enable mnemonics (& = access key)
tabControlAdv1.UseMnemonic = true;

// Disable mnemonics (& displayed literally)
tabControlAdv1.UseMnemonic = false;
```

### Example with Access Keys

```csharp
tabControlAdv1.UseMnemonic = true;

// Create tabs with access keys
TabPageAdv fileTab = new TabPageAdv();
fileTab.Text = "&File";  // Alt+F activates

TabPageAdv editTab = new TabPageAdv();
editTab.Text = "&Edit";  // Alt+E activates

TabPageAdv viewTab = new TabPageAdv();
viewTab.Text = "&View";  // Alt+V activates

tabControlAdv1.TabPages.Add(fileTab);
tabControlAdv1.TabPages.Add(editTab);
tabControlAdv1.TabPages.Add(viewTab);
```

**When to use:** Ribbon-style interfaces or applications with keyboard-focused workflows.

## TabPage Border Settings

Customize borders for tab content areas.

### BorderStyle Property

```csharp
// Fixed single line border
tabControlAdv1.BorderStyle = BorderStyle.FixedSingle;

// 3D border
tabControlAdv1.BorderStyle = BorderStyle.Fixed3D;

// No border
tabControlAdv1.BorderStyle = BorderStyle.None;
```

### FixedSingleBorderColor

Set custom border color for FixedSingle style:

```csharp
// Set border style
tabControlAdv1.BorderStyle = BorderStyle.FixedSingle;

// Set custom border color
tabControlAdv1.FixedSingleBorderColor = Color.DarkBlue;

// Or match theme
tabControlAdv1.FixedSingleBorderColor = Color.FromArgb(0, 120, 215);
```

### Reset Border Color

```csharp
// Reset to default border color
tabControlAdv1.ResetFixedSingleBorderColor();
```

### Complete Example

```csharp
TabControlAdv borderedTabs = new TabControlAdv();
borderedTabs.Size = new Size(600, 400);

// Configure border
borderedTabs.BorderStyle = BorderStyle.FixedSingle;
borderedTabs.FixedSingleBorderColor = Color.SteelBlue;

// Add tabs
for (int i = 1; i <= 3; i++)
{
    TabPageAdv tab = new TabPageAdv();
    tab.Text = $"Panel {i}";
    
    // Add content to show border
    Panel panel = new Panel();
    panel.Dock = DockStyle.Fill;
    panel.BackColor = Color.White;
    tab.Controls.Add(panel);
    
    borderedTabs.TabPages.Add(tab);
}

this.Controls.Add(borderedTabs);
```

## Image Settings

Add animated GIF images to tabs or tab pages.

### Animated GIF in Tab Headers

TabControlAdv supports animated GIF images in tab headers:

```csharp
// Load GIF image
Image animatedGif = Image.FromFile("loading.gif");

// Set image for tab
tabPageAdv1.Image = animatedGif;

// Set image size
tabPageAdv1.ImageSize = new Size(16, 16);
```

**Important:** Set `ImageIndex = -1` to use the Image property instead of ImageList:

```csharp
// Use Image property instead of ImageList
tabPageAdv1.ImageIndex = -1;
tabPageAdv1.Image = Image.FromFile("animated.gif");
tabPageAdv1.ImageSize = new Size(20, 20);
```

### Complete Animated GIF Example

```csharp
TabControlAdv animatedTabs = new TabControlAdv();
animatedTabs.Size = new Size(500, 350);

// Tab with animated loading indicator
TabPageAdv loadingTab = new TabPageAdv();
loadingTab.Text = "Loading";
loadingTab.ImageIndex = -1;  // Important!
loadingTab.Image = Image.FromFile("loading.gif");
loadingTab.ImageSize = new Size(16, 16);

// Tab with static image
TabPageAdv completeTab = new TabPageAdv();
completeTab.Text = "Complete";
completeTab.ImageIndex = -1;
completeTab.Image = Image.FromFile("check.gif");
completeTab.ImageSize = new Size(16, 16);

animatedTabs.TabPages.Add(loadingTab);
animatedTabs.TabPages.Add(completeTab);

this.Controls.Add(animatedTabs);
```

### Background Images in Tab Content

Set animated or static background images for tab page content:

```csharp
// Set background image for tab page
tabPageAdv1.BackgroundImage = Image.FromFile("background.gif");
tabPageAdv1.BackgroundImageLayout = ImageLayout.Tile;
```

## Preventing Specific Tab Movement

Prevent certain tabs from being moved while allowing others to be reordered.

### Using TabMoving Event

```csharp
// Enable moving
tabControlAdv1.UserMoveTabs = true;

// Prevent moving specific tabs
tabControlAdv1.TabMoving += (sender, e) =>
{
    // Prevent moving first tab (index 0)
    if (e.From == 0 || e.Target == 0)
    {
        e.Cancel = true;
        MessageBox.Show("The Home tab must remain in the first position");
        return;
    }
    
    // Prevent moving last tab
    int lastIndex = tabControlAdv1.TabPages.Count - 1;
    if (e.From == lastIndex || e.Target == lastIndex)
    {
        e.Cancel = true;
        MessageBox.Show("The Settings tab must remain in the last position");
        return;
    }
};
```

### Complete Example with Protected Tabs

```csharp
public class ProtectedTabsExample : Form
{
    private TabControlAdv tabControl;
    
    public ProtectedTabsExample()
    {
        InitializeTabControl();
        AddTabs();
        SetupProtection();
    }
    
    private void InitializeTabControl()
    {
        tabControl = new TabControlAdv();
        tabControl.Dock = DockStyle.Fill;
        tabControl.UserMoveTabs = true;
        this.Controls.Add(tabControl);
    }
    
    private void AddTabs()
    {
        string[] tabNames = { "Home", "Data", "Analysis", "Charts", "Settings" };
        
        foreach (string name in tabNames)
        {
            TabPageAdv tab = new TabPageAdv();
            tab.Text = name;
            
            Label label = new Label();
            label.Text = $"{name} Page";
            label.Location = new Point(20, 20);
            label.AutoSize = true;
            tab.Controls.Add(label);
            
            tabControl.TabPages.Add(tab);
        }
    }
    
    private void SetupProtection()
    {
        // Protect first and last tabs from moving
        tabControl.TabMoving += (sender, e) =>
        {
            int lastIndex = tabControl.TabPages.Count - 1;
            
            // Check if trying to move protected tabs
            if (e.From == 0 || e.Target == 0)
            {
                e.Cancel = true;
                ShowProtectionMessage("Home");
            }
            else if (e.From == lastIndex || e.Target == lastIndex)
            {
                e.Cancel = true;
                ShowProtectionMessage("Settings");
            }
        };
        
        // Visual indicator for protected tabs
        tabControl.TabPages[0].TabForeColor = Color.DarkGreen;
        tabControl.TabPages[0].ToolTipText = "Home (cannot be moved)";
        
        int last = tabControl.TabPages.Count - 1;
        tabControl.TabPages[last].TabForeColor = Color.DarkGreen;
        tabControl.TabPages[last].ToolTipText = "Settings (cannot be moved)";
    }
    
    private void ShowProtectionMessage(string tabName)
    {
        MessageBox.Show(
            $"The {tabName} tab is protected and cannot be moved.",
            "Protected Tab",
            MessageBoxButtons.OK,
            MessageBoxIcon.Information);
    }
}
```

## Combined Customization Example

```csharp
public class FullyCustomizableTabsExample : Form
{
    private TabControlAdv tabControl;
    
    public FullyCustomizableTabsExample()
    {
        InitializeForm();
        SetupTabControl();
        AddTabs();
    }
    
    private void InitializeForm()
    {
        this.Text = "Fully Customizable Tabs";
        this.Size = new Size(800, 600);
    }
    
    private void SetupTabControl()
    {
        tabControl = new TabControlAdv();
        tabControl.Dock = DockStyle.Fill;
        
        // Enable all customization features
        tabControl.LabelEdit = true;
        tabControl.UserMoveTabs = true;
        tabControl.UseMnemonic = true;
        tabControl.Padding = new Point(12, 8);
        
        // Appearance
        tabControl.BorderStyle = BorderStyle.FixedSingle;
        tabControl.FixedSingleBorderColor = Color.Gray;
        
        // Events
        tabControl.AfterEdit += OnAfterEdit;
        tabControl.TabMoving += OnTabMoving;
        tabControl.TabsOrderChanged += OnTabsOrderChanged;
        
        this.Controls.Add(tabControl);
    }
    
    private void AddTabs()
    {
        string[] tabNames = { "&Home", "&Data Entry", "&Reports", "&Settings" };
        
        foreach (string name in tabNames)
        {
            TabPageAdv tab = new TabPageAdv();
            tab.Text = name;
            tab.ToolTipText = "Double-click to rename, drag to reorder";
            
            Panel panel = new Panel();
            panel.Dock = DockStyle.Fill;
            panel.Padding = new Padding(20);
            
            Label instructions = new Label();
            instructions.Text = $"This is the {name.Replace("&", "")} tab.\n\n" +
                               "• Double-click tab name to rename\n" +
                               "• Drag tab to reorder\n" +
                               "• Use Alt+" + name[1] + " to activate";
            instructions.AutoSize = true;
            instructions.Font = new Font("Segoe UI", 10);
            
            panel.Controls.Add(instructions);
            tab.Controls.Add(panel);
            
            tabControl.TabPages.Add(tab);
        }
    }
    
    private void OnAfterEdit(object sender, EditEventArgs e)
    {
        string newName = e.EditText.Trim();
        if (string.IsNullOrEmpty(newName))
        {
            MessageBox.Show("Tab name cannot be empty");
        }
        else
        {
            Console.WriteLine($"Tab renamed to: {newName}");
        }
    }
    
    private void OnTabMoving(object sender, TabMovingEventArgs e)
    {
        // Prevent moving Settings tab (last tab)
        int lastIndex = tabControl.TabPages.Count - 1;
        if (e.From == lastIndex || e.Target == lastIndex)
        {
            e.Cancel = true;
            MessageBox.Show("Settings tab must remain last");
        }
    }
    
    private void OnTabsOrderChanged(object sender, EventArgs e)
    {
        Console.WriteLine("Tab order updated:");
        for (int i = 0; i < tabControl.TabPages.Count; i++)
        {
            Console.WriteLine($"  {i + 1}. {tabControl.TabPages[i].Text}");
        }
    }
}
```

## Best Practices

### Label Editing
- Validate new names in AfterEdit event
- Provide clear visual feedback when editing
- Consider restricting special characters
- Update tooltips when names change

### Tab Movement
- Protect critical tabs from moving (Home, Settings)
- Provide visual indicators for protected tabs
- Save tab order to user preferences
- Restore order on application restart

### Padding
- Use consistent padding across application
- Consider touch targets (12-16px for touch)
- Balance aesthetics and usability
- Test with different screen sizes

### Borders
- Use borders to define content boundaries
- Match border colors with theme
- Consider borderless for seamless integration
- Test with different backgrounds
