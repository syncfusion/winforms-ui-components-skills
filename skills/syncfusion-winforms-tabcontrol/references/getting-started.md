# Getting Started with TabControlAdv

Learn how to add TabControlAdv to Windows Forms applications, create tabs, and configure basic properties.

## Assembly Deployment

### Required Assemblies

TabControlAdv requires the following assembly references:

- `Syncfusion.Grid.Base.dll`
- `Syncfusion.Grid.Windows.dll`
- `Syncfusion.Shared.Base.dll`
- `Syncfusion.Shared.Windows.dll`
- `Syncfusion.Tools.Base.dll`
- `Syncfusion.Tools.Windows.dll`

### NuGet Package Installation

Install the Syncfusion Windows Forms NuGet package:

**Package Manager Console:**
```powershell
Install-Package Syncfusion.Tools.Windows
```

**NuGet Package Manager:**
1. Right-click on your project → Manage NuGet Packages
2. Search for "Syncfusion.Tools.WinForms"
3. Click Install

The package automatically adds required assembly references.

## Adding TabControlAdv Through Designer

### Using the Toolbox

1. Open your Windows Forms designer
2. Locate TabControlAdv in the Toolbox (Syncfusion section)
3. Drag and drop TabControlAdv onto your form

The designer automatically adds required assembly references.

### Adding Tabs Through Designer

**Method 1: Context Menu**
1. Right-click the TabControlAdv
2. Select "Add Tab" option
3. A new TabPageAdv is added

**Method 2: Smart Tag**
1. Click the smart tag arrow on TabControlAdv
2. Select "TabPagesCollection"
3. In the editor, click "Add" to create new TabPageAdv items
4. Set properties for each tab (Text, ImageIndex, etc.)

### Designer Properties

Configure common properties in the Properties window:

```
TabControlAdv Properties:
- Alignment: Top, Bottom, Left, Right
- ActiveTabColor: Color for active tab
- InactiveTabColor: Color for inactive tabs
- ShowTabCloseButton: Enable close buttons
- UserMoveTabs: Enable drag-drop reordering
- LabelEdit: Enable runtime text editing
```

## Adding Control Manually in Code

### Basic Setup

```csharp
using Syncfusion.Windows.Forms.Tools;

public partial class MainForm : Form
{
    private TabControlAdv tabControlAdv1;
    
    public MainForm()
    {
        InitializeComponent();
        InitializeTabControl();
    }
    
    private void InitializeTabControl()
    {
        // Create TabControlAdv
        tabControlAdv1 = new TabControlAdv();
        tabControlAdv1.Location = new Point(10, 10);
        tabControlAdv1.Size = new Size(600, 400);
        
        // Add to form
        this.Controls.Add(tabControlAdv1);
    }
}
```

### Using Namespace

Always include the namespace at the top of your file:

```csharp
using Syncfusion.Windows.Forms.Tools;
```

Or use fully qualified names:

```csharp
Syncfusion.Windows.Forms.Tools.TabControlAdv tabControlAdv1 = 
    new Syncfusion.Windows.Forms.Tools.TabControlAdv();
```

## Adding Tabs Programmatically

### Creating TabPageAdv Items

```csharp
// Create individual tab pages
TabPageAdv tabPageAdv1 = new TabPageAdv();
tabPageAdv1.Text = "Home";
tabPageAdv1.BackColor = Color.White;

TabPageAdv tabPageAdv2 = new TabPageAdv();
tabPageAdv2.Text = "Settings";
tabPageAdv2.BackColor = Color.WhiteSmoke;

TabPageAdv tabPageAdv3 = new TabPageAdv();
tabPageAdv3.Text = "About";
tabPageAdv3.BackColor = Color.White;

// Add to TabControlAdv
this.tabControlAdv1.TabPages.Add(tabPageAdv1);
this.tabControlAdv1.TabPages.Add(tabPageAdv2);
this.tabControlAdv1.TabPages.Add(tabPageAdv3);
```

### Adding Multiple Tabs in Loop

```csharp
for (int i = 1; i <= 5; i++)
{
    TabPageAdv tab = new TabPageAdv();
    tab.Text = $"Tab {i}";
    tab.Name = $"tabPage{i}";
    tabControlAdv1.TabPages.Add(tab);
}
```

### Adding Tabs with Images

```csharp
// Setup ImageList
ImageList imageList1 = new ImageList();
imageList1.ImageSize = new Size(16, 16);
imageList1.Images.Add("home", Properties.Resources.HomeIcon);
imageList1.Images.Add("settings", Properties.Resources.SettingsIcon);
imageList1.Images.Add("help", Properties.Resources.HelpIcon);

tabControlAdv1.ImageList = imageList1;

// Create tabs with image indices
TabPageAdv homeTab = new TabPageAdv();
homeTab.Text = "Home";
homeTab.ImageIndex = 0;

TabPageAdv settingsTab = new TabPageAdv();
settingsTab.Text = "Settings";
settingsTab.ImageIndex = 1;

TabPageAdv helpTab = new TabPageAdv();
helpTab.Text = "Help";
helpTab.ImageIndex = 2;

tabControlAdv1.TabPages.Add(homeTab);
tabControlAdv1.TabPages.Add(settingsTab);
tabControlAdv1.TabPages.Add(helpTab);
```

## Adding Controls to Tab Pages

### Adding Controls Programmatically

```csharp
// Create a label for the tab page
Label label1 = new Label();
label1.Text = "Welcome to the Home tab";
label1.Location = new Point(20, 20);
label1.AutoSize = true;

// Create a button
Button button1 = new Button();
button1.Text = "Click Me";
button1.Location = new Point(20, 60);
button1.Click += (s, e) => MessageBox.Show("Button clicked!");

// Add controls to tab page
tabPageAdv1.Controls.Add(label1);
tabPageAdv1.Controls.Add(button1);
```

### Adding Complex Controls

```csharp
// Add a calendar to Settings tab
SfCalendar calendar = new SfCalendar();
calendar.Dock = DockStyle.Fill;
tabPageAdv2.Controls.Add(calendar);

// Add a grid to another tab
DataGridView grid = new DataGridView();
grid.Dock = DockStyle.Fill;
tabPageAdv3.Controls.Add(grid);
```

### Using Panels for Layout

```csharp
// Create a panel for better layout control
Panel panel1 = new Panel();
panel1.Dock = DockStyle.Fill;
panel1.Padding = new Padding(10);

// Add multiple controls to panel
Label titleLabel = new Label();
titleLabel.Text = "Dashboard";
titleLabel.Font = new Font("Segoe UI", 14, FontStyle.Bold);
titleLabel.Dock = DockStyle.Top;

Panel contentPanel = new Panel();
contentPanel.Dock = DockStyle.Fill;
// Add more controls to contentPanel...

panel1.Controls.Add(contentPanel);
panel1.Controls.Add(titleLabel);

// Add panel to tab page
tabPageAdv1.Controls.Add(panel1);
```

## Basic Tab Placement

### Setting Tab Alignment

```csharp
// Tabs at top (default)
tabControlAdv1.Alignment = TabAlignment.Top;

// Tabs at bottom
tabControlAdv1.Alignment = TabAlignment.Bottom;

// Tabs on left side
tabControlAdv1.Alignment = TabAlignment.Left;

// Tabs on right side
tabControlAdv1.Alignment = TabAlignment.Right;
```

### Tab Gap Spacing

```csharp
// Set spacing between tabs
tabControlAdv1.TabGap = 5; // 5 pixels between tabs
```

## Edit Header at Runtime

Enable users to rename tab headers at runtime:

```csharp
// Enable label editing
tabControlAdv1.LabelEdit = true;
```

**How to edit:**
1. **Double-click** the tab text to enter edit mode
2. **Right-click** the tab and the text enters edit mode
3. Type new text
4. Press **Enter** to save or click elsewhere

**Programmatic editing:**
```csharp
// Bring a tab into edit mode programmatically
tabControlAdv1.SelectedTab.Text = "New Name";
```

## Multi-line Tabs

Arrange tabs in multiple rows when they exceed the control width:

```csharp
// Enable multi-line tabs
tabControlAdv1.Multiline = true;

// Multiline text within a single tab
tabControlAdv1.MultilineText = true;

// Keep selected tab in front row
tabControlAdv1.KeepSelectedTabInFrontRow = true;
```

**Example:**
```csharp
TabControlAdv tabControl = new TabControlAdv();
tabControl.Multiline = true;
tabControl.Size = new Size(400, 300);

// Add many tabs - they will wrap to multiple rows
for (int i = 1; i <= 15; i++)
{
    TabPageAdv tab = new TabPageAdv();
    tab.Text = $"Tab {i}";
    tabControl.TabPages.Add(tab);
}
```

## Complete Example

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class TabControlExample : Form
{
    private TabControlAdv tabControlAdv1;
    
    public TabControlExample()
    {
        InitializeComponent();
    }
    
    private void InitializeComponent()
    {
        // Form setup
        this.Text = "TabControlAdv Example";
        this.Size = new Size(800, 600);
        
        // Create TabControlAdv
        tabControlAdv1 = new TabControlAdv();
        tabControlAdv1.Dock = DockStyle.Fill;
        tabControlAdv1.Alignment = TabAlignment.Top;
        tabControlAdv1.LabelEdit = true;
        tabControlAdv1.ShowTabCloseButton = true;
        tabControlAdv1.Multiline = false;
        
        // Create tabs
        CreateHomTab();
        CreateSettingsTab();
        CreateAboutTab();
        
        // Add to form
        this.Controls.Add(tabControlAdv1);
    }
    
    private void CreateHomeTab()
    {
        TabPageAdv homeTab = new TabPageAdv();
        homeTab.Text = "Home";
        
        Label welcomeLabel = new Label();
        welcomeLabel.Text = "Welcome to TabControlAdv!";
        welcomeLabel.Font = new Font("Segoe UI", 16, FontStyle.Bold);
        welcomeLabel.Location = new Point(50, 50);
        welcomeLabel.AutoSize = true;
        
        homeTab.Controls.Add(welcomeLabel);
        tabControlAdv1.TabPages.Add(homeTab);
    }
    
    private void CreateSettingsTab()
    {
        TabPageAdv settingsTab = new TabPageAdv();
        settingsTab.Text = "Settings";
        
        CheckBox option1 = new CheckBox();
        option1.Text = "Enable notifications";
        option1.Location = new Point(30, 30);
        option1.AutoSize = true;
        
        CheckBox option2 = new CheckBox();
        option2.Text = "Auto-save";
        option2.Location = new Point(30, 60);
        option2.AutoSize = true;
        
        settingsTab.Controls.Add(option1);
        settingsTab.Controls.Add(option2);
        tabControlAdv1.TabPages.Add(settingsTab);
    }
    
    private void CreateAboutTab()
    {
        TabPageAdv aboutTab = new TabPageAdv();
        aboutTab.Text = "About";
        
        Label infoLabel = new Label();
        infoLabel.Text = "TabControlAdv Demo Application\nVersion 1.0";
        infoLabel.Location = new Point(30, 30);
        infoLabel.AutoSize = true;
        
        aboutTab.Controls.Add(infoLabel);
        tabControlAdv1.TabPages.Add(aboutTab);
    }
    
    [STAThread]
    static void Main()
    {
        Application.EnableVisualStyles();
        Application.Run(new TabControlExample());
    }
}
```

## Common Issues and Solutions

### Issue: Tabs Not Visible
**Solution:** Check that TabPages have been added to the TabPages collection and the control has a valid size.

### Issue: Assembly Reference Errors
**Solution:** Ensure all required Syncfusion assemblies are referenced. Use NuGet package for automatic reference management.

### Issue: Images Not Displaying
**Solution:** Verify ImageList is assigned to TabControlAdv and ImageIndex is set correctly on each TabPageAdv.

### Issue: Edit Mode Not Working
**Solution:** Set `LabelEdit = true` and ensure user is double-clicking or right-clicking the tab text.
