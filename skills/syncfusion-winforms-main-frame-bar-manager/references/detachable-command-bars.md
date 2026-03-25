# Detachable Command Bars

This guide covers creating and managing CommandBar instances that can be detached from the main menu and docked at different positions in your application.

## CommandBar Overview

A CommandBar is a specialized bar component that can float independently of the main MainFrameBarManager. It supports docking to different edges of the form or floating freely.

### CommandBar Features

- **Detachable:** Can be separated from main menu structure
- **Dockable:** Attaches to Top, Bottom, Left, or Right edges
- **Floatable:** Can float as independent window
- **Customizable:** Can add toolbar items like standard bars
- **Persistent:** Maintains position between sessions

## Creating CommandBar via Code

### Basic CommandBar Creation

```csharp
// Create CommandBar instance
CommandBar commandBar1 = new Syncfusion.Windows.Forms.Tools.CommandBar();

// Set properties
commandBar1.Name = "commandBar1";
commandBar1.Text = "Standard Toolbar";
commandBar1.DockState = Syncfusion.Windows.Forms.Tools.CommandBarDockState.Top;

// Add to MainFrameBarManager
mainFrameBarManager1.DetachedCommandBars.Add(commandBar1);
```

### DockState Options

| DockState | Position | Description |
|-----------|----------|-------------|
| **Top** | Top edge | Docks at top of form, below menu bar |
| **Bottom** | Bottom edge | Docks at bottom of form |
| **Left** | Left edge | Docks at left side of form |
| **Right** | Right edge | Docks at right side of form |
| **Floating** | Free-floating | Floats as independent window |

### Setting Dock Position

```csharp
CommandBar commandBar1 = new CommandBar();
commandBar1.Name = "toolBar1";
commandBar1.Text = "Tools";

// Dock at top (most common)
commandBar1.DockState = CommandBarDockState.Top;

// Dock at bottom
commandBar1.DockState = CommandBarDockState.Bottom;

// Dock at left
commandBar1.DockState = CommandBarDockState.Left;

// Float independently
commandBar1.DockState = CommandBarDockState.Floating;

// Set floating position
if (commandBar1.DockState == CommandBarDockState.Floating)
{
    commandBar1.FloatingPoint = new Point(100, 100);
}

mainFrameBarManager1.DetachedCommandBars.Add(commandBar1);
```

## VB.NET Example

```vb
' Create CommandBar
Dim commandBar1 As New Syncfusion.Windows.Forms.Tools.CommandBar()

' Set properties
commandBar1.Name = "commandBar1"
commandBar1.Text = "Standard Toolbar"
commandBar1.DockState = Syncfusion.Windows.Forms.Tools.CommandBarDockState.Top

' Add to manager
mainFrameBarManager1.DetachedCommandBars.Add(commandBar1)
```

## Creating CommandBar via Designer

### Adding CommandBar in Designer

1. Open Windows Forms designer
2. Locate **CommandBar** in toolbox (Syncfusion Tools)
3. Drag onto form - appears in component tray
4. Name the component (e.g., commandBar1)

### Setting Properties in Designer

1. Select CommandBar in component tray
2. In Properties panel, set:
   - **Name:** commandBar1
   - **Text:** "Standard Toolbar"
   - **DockState:** Top (or desired position)

### Designer Integration with MainFrameBarManager

1. In MainFrameBarManager Smart Tag menu
2. Select **DetachedCommandBars...**
3. Collection Editor opens
4. Add existing CommandBar instances
5. Click OK to add collection

## Adding Items to CommandBar

### Adding BarItems to CommandBar

CommandBar can contain the same BarItem types as regular bars:

```csharp
// Create CommandBar
CommandBar commandBar1 = new CommandBar();
commandBar1.Name = "standardToolBar";
commandBar1.Text = "Standard Toolbar";

// Create toolbar items
BarItem newItem = new BarItem() { Text = "New" };
BarItem openItem = new BarItem() { Text = "Open" };
BarItem saveItem = new BarItem() { Text = "Save" };

// Add items to CommandBar
commandBar1.Items.AddRange(new BarItem[] { newItem, openItem, saveItem });

// Add CommandBar to manager
mainFrameBarManager1.DetachedCommandBars.Add(commandBar1);
```

### Adding with Event Handlers

```csharp
BarItem newItem = new BarItem() { Text = "New" };
newItem.ItemClick += (sender, args) => CreateNewDocument();

BarItem saveItem = new BarItem() { Text = "Save" };
saveItem.ItemClick += (sender, args) => SaveDocument();

CommandBar toolbar = new CommandBar() 
{ 
    Name = "toolbar", 
    Text = "Toolbar",
    DockState = CommandBarDockState.Top 
};

toolbar.Items.AddRange(new BarItem[] { newItem, saveItem });
mainFrameBarManager1.DetachedCommandBars.Add(toolbar);
```

## Multiple CommandBars

### Creating Multiple Toolbars

Separate toolbars by functionality:

```csharp
// Standard toolbar
CommandBar standardToolbar = new CommandBar();
standardToolbar.Name = "standardToolbar";
standardToolbar.Text = "Standard";
standardToolbar.DockState = CommandBarDockState.Top;

BarItem newItem = new BarItem() { Text = "New" };
BarItem openItem = new BarItem() { Text = "Open" };
BarItem saveItem = new BarItem() { Text = "Save" };
standardToolbar.Items.AddRange(new BarItem[] { newItem, openItem, saveItem });

// Format toolbar
CommandBar formatToolbar = new CommandBar();
formatToolbar.Name = "formatToolbar";
formatToolbar.Text = "Format";
formatToolbar.DockState = CommandBarDockState.Top;

BarItem boldItem = new BarItem() { Text = "Bold" };
BarItem italicItem = new BarItem() { Text = "Italic" };
BarItem underlineItem = new BarItem() { Text = "Underline" };
formatToolbar.Items.AddRange(new BarItem[] { boldItem, italicItem, underlineItem });

// View toolbar
CommandBar viewToolbar = new CommandBar();
viewToolbar.Name = "viewToolbar";
viewToolbar.Text = "View";
viewToolbar.DockState = CommandBarDockState.Right;

BarItem zoomInItem = new BarItem() { Text = "Zoom In" };
BarItem zoomOutItem = new BarItem() { Text = "Zoom Out" };
viewToolbar.Items.AddRange(new BarItem[] { zoomInItem, zoomOutItem });

// Add all to manager
mainFrameBarManager1.DetachedCommandBars.Add(standardToolbar);
mainFrameBarManager1.DetachedCommandBars.Add(formatToolbar);
mainFrameBarManager1.DetachedCommandBars.Add(viewToolbar);
```

## Floating CommandBars

### Setting Floating Position

```csharp
CommandBar toolbar = new CommandBar();
toolbar.Name = "floatingToolbar";
toolbar.Text = "Floating Toolbar";
toolbar.DockState = CommandBarDockState.Floating;

// Set floating position
toolbar.FloatingPoint = new Point(200, 100);

// Set size
toolbar.Width = 300;
toolbar.Height = 40;

mainFrameBarManager1.DetachedCommandBars.Add(toolbar);
```

### User Interaction with Floating Bars

Users can:
1. **Drag bar title:** Move floating toolbar
2. **Drag to dock area:** Dock floating toolbar to edge
3. **Right-click:** Access toolbar options (hide, reset, etc.)

This flexibility allows end-users to arrange toolbars as needed.

## CommandBar with Controls

### Hosting Controls in CommandBar

CommandBar can host various WinForms controls:

```csharp
CommandBar toolbar = new CommandBar();
toolbar.Name = "toolbarWithControls";
toolbar.Text = "Tools";
toolbar.DockState = CommandBarDockState.Top;

// Combo box control
ComboBoxBarItem fontCombo = new ComboBoxBarItem();
fontCombo.Text = "Font:";
fontCombo.ChoiceList.AddRange(new string[] { "Arial", "Times", "Courier" });
toolbar.Items.Add(fontCombo);

// Text box control
TextBoxBarItem searchBox = new TextBoxBarItem();
searchBox.Text = "Search:";
toolbar.Items.Add(searchBox);

// Regular buttons
BarItem searchBtn = new BarItem() { Text = "Find" };
searchBtn.ItemClick += (s, e) => PerformSearch(searchBox.TextBoxValue);
toolbar.Items.Add(searchBtn);

mainFrameBarManager1.DetachedCommandBars.Add(toolbar);
```

## Programmatic Control

### Show/Hide CommandBars

```csharp
// Hide toolbar
CommandBar toolbar = mainFrameBarManager1.DetachedCommandBars[0];
toolbar.Visible = false;

// Show toolbar
toolbar.Visible = true;
```

### Access Toolbar Items

```csharp
// Get toolbar
CommandBar toolbar = mainFrameBarManager1.DetachedCommandBars[0];

// Access items
foreach (BarItem item in toolbar.Items)
{
    if (item.Text == "Save")
    {
        item.Enabled = false;  // Disable save
    }
}
```

### Change Dock Position at Runtime

```csharp
CommandBar toolbar = mainFrameBarManager1.DetachedCommandBars["toolbarName"];

// Move to different position
if (userTogglesToobar)
{
    toolbar.DockState = (toolbar.DockState == CommandBarDockState.Top)
        ? CommandBarDockState.Bottom
        : CommandBarDockState.Top;
}
```

## Complete Example

```csharp
public partial class MainForm : Form
{
    private MainFrameBarManager mainFrameBarManager1;
    
    public MainForm()
    {
        InitializeComponent();
        this.Text = "CommandBar Example";
        this.Size = new Size(800, 600);
        
        // Create menu manager
        mainFrameBarManager1 = new MainFrameBarManager();
        mainFrameBarManager1.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016Colorful;
        mainFrameBarManager1.Form = this;
        
        // Create toolbars
        CreateStandardToolbar();
        CreateFormatToolbar();
        CreateViewToolbar();
    }
    
    private void CreateStandardToolbar()
    {
        CommandBar toolbar = new CommandBar();
        toolbar.Name = "standardToolbar";
        toolbar.Text = "Standard";
        toolbar.DockState = CommandBarDockState.Top;
        
        BarItem newItem = new BarItem() { Text = "New" };
        BarItem openItem = new BarItem() { Text = "Open" };
        BarItem saveItem = new BarItem() { Text = "Save" };
        
        newItem.ItemClick += (s, e) => MessageBox.Show("New Document");
        openItem.ItemClick += (s, e) => MessageBox.Show("Open Document");
        saveItem.ItemClick += (s, e) => MessageBox.Show("Save Document");
        
        toolbar.Items.AddRange(new BarItem[] { newItem, openItem, saveItem });
        mainFrameBarManager1.DetachedCommandBars.Add(toolbar);
    }
    
    private void CreateFormatToolbar()
    {
        CommandBar toolbar = new CommandBar();
        toolbar.Name = "formatToolbar";
        toolbar.Text = "Format";
        toolbar.DockState = CommandBarDockState.Top;
        
        BarItem boldItem = new BarItem() { Text = "Bold", Checked = false };
        BarItem italicItem = new BarItem() { Text = "Italic", Checked = false };
        
        boldItem.ItemClick += (s, e) => 
            boldItem.Checked = !boldItem.Checked;
        italicItem.ItemClick += (s, e) => 
            italicItem.Checked = !italicItem.Checked;
        
        toolbar.Items.AddRange(new BarItem[] { boldItem, italicItem });
        mainFrameBarManager1.DetachedCommandBars.Add(toolbar);
    }
    
    private void CreateViewToolbar()
    {
        CommandBar toolbar = new CommandBar();
        toolbar.Name = "viewToolbar";
        toolbar.Text = "View";
        toolbar.DockState = CommandBarDockState.Right;
        
        BarItem zoomInItem = new BarItem() { Text = "+" };
        BarItem zoomOutItem = new BarItem() { Text = "-" };
        
        toolbar.Items.AddRange(new BarItem[] { zoomInItem, zoomOutItem });
        mainFrameBarManager1.DetachedCommandBars.Add(toolbar);
    }
}
```

## Best Practices

1. **Logical Grouping:** Group related functions in separate toolbars
2. **Clear Naming:** Use descriptive names for toolbar identification
3. **Consistent Positioning:** Place frequently-used toolbars at top
4. **Accessibility:** Include tooltips and keyboard shortcuts for items
5. **Persistence:** Enable state persistence so toolbar positions are saved
6. **User Control:** Allow users to show/hide and rearrange toolbars
7. **Icon Usage:** Use 16x16 icons for toolbar items for clarity
8. **Test Docking:** Verify toolbars dock correctly at all positions

CommandBars provide flexible toolbar management, allowing users to customize their workspace according to their preferences.
