# Getting Started with Windows Forms CommandBar

## Table of Contents
- [Assembly Dependencies](#assembly-dependencies)
- [Adding via Designer](#adding-via-designer)
- [Adding via Code](#adding-via-code)
- [Child Controls](#child-controls)

## Assembly Dependencies

### Required assemblies

Add these references to your project:
- Syncfusion.Grid.Base.dll
- Syncfusion.Grid.Windows.dll
- Syncfusion.Shared.Base.dll
- Syncfusion.Shared.Windows.dll
- Syncfusion.Tools.Base.dll
- Syncfusion.Tools.Windows.dll

### NuGet package

Install via NuGet Package Manager:

```
Install-Package Syncfusion.Tools.Windows
```

When added via NuGet or designer, required assemblies are referenced automatically.

## Adding via Designer

### Step 1: Add CommandBarController

1. Drag `CommandBarController` from the toolbox to your form
2. Designer automatically adds all required assembly references
3. CommandBarController appears in the component tray below the form

### Step 2: Add CommandBar to Controller

1. Select CommandBarController in the component tray
2. Click the smart tag (small arrow) that appears
3. Select "Add CommandBar" from the smart tag menu
4. CommandBar is added and appears as a strip on the form
5. Repeat to add multiple CommandBars

### Step 3: Configure CommandBar

- Set `Text` property for the bar name
- Set `DockState` for position (Top, Bottom, Left, Right)
- Configure visual appearance and behavior

## Adding via Code

### Step 1: Include namespace

```csharp
using Syncfusion.Windows.Forms.Tools;
```

### Step 2: Create CommandBarController

```csharp
CommandBarController commandBarController1 = new CommandBarController();
this.commandBarController1.HostForm = this;
```

### Step 3: Create CommandBar instance

```csharp
CommandBar commandBar1 = new CommandBar();
this.commandBar1.Text = "commandBar1";
this.commandBarController1.CommandBars.Add(this.commandBar1);
```

### Step 4: Complete initialization example

```csharp
public partial class Form1 : Form
{
    private CommandBarController commandBarController1;
    private CommandBar commandBar1;

    public Form1()
    {
        InitializeComponent();
        InitializeCommandBar();
    }

    private void InitializeCommandBar()
    {
        // Create controller
        commandBarController1 = new CommandBarController();
        commandBarController1.HostForm = this;
        
        // Create first command bar
        commandBar1 = new CommandBar();
        commandBar1.Text = "Main Toolbar";
        commandBar1.DockState = CommandBarDockState.Top;
        commandBarController1.CommandBars.Add(commandBar1);
        
        // Create second command bar
        CommandBar commandBar2 = new CommandBar();
        commandBar2.Text = "Formatting Toolbar";
        commandBar2.DockState = CommandBarDockState.Top;
        commandBarController1.CommandBars.Add(commandBar2);
    }
}
```

## Child Controls

### Adding single control

Add single-line controls like TextBox, ComboBox, or Button directly:

```csharp
TextBox textBox1 = new TextBox();
textBox1.Width = 200;
this.commandBar1.Controls.Add(textBox1);
```

```csharp
Button button1 = new Button();
button1.Text = "Search";
this.commandBar1.Controls.Add(button1);
```

### Adding multiple controls

Use a container control like Panel to host multiple controls with proper alignment:

```csharp
Panel panel1 = new Panel();
panel1.Dock = DockStyle.Fill;

Label label1 = new Label();
label1.Text = "Filter:";
label1.TextAlign = ContentAlignment.MiddleCenter;
label1.AutoSize = true;

ComboBox comboBox1 = new ComboBox();
comboBox1.Items.AddRange(new string[] { "All", "Active", "Inactive", "Archived" });
comboBox1.SelectedIndex = 0;
comboBox1.Location = new Point(label1.Width + 10, 0);

panel1.Controls.Add(label1);
panel1.Controls.Add(comboBox1);

this.commandBar1.Controls.Add(panel1);
```

### Complex layout example

```csharp
// Create main layout panel
Panel searchPanel = new Panel();
searchPanel.Dock = DockStyle.Fill;
searchPanel.Padding = new Padding(5);

// Create label
Label searchLabel = new Label();
searchLabel.Text = "Search:";
searchLabel.AutoSize = true;

// Create textbox
TextBox searchBox = new TextBox();
searchBox.Width = 150;
searchBox.Location = new Point(searchLabel.Width + 10, 3);

// Create button
Button searchButton = new Button();
searchButton.Text = "Find";
searchButton.Width = 60;
searchButton.Location = new Point(searchBox.Location.X + searchBox.Width + 5, 3);

// Add to panel
searchPanel.Controls.Add(searchLabel);
searchPanel.Controls.Add(searchBox);
searchPanel.Controls.Add(searchButton);

// Add panel to command bar
this.commandBar1.Controls.Add(searchPanel);
```

## Edge cases and troubleshooting

**Issue: CommandBar not showing when adding via code**

Solution: Ensure `HostForm` is set before adding CommandBars, and the form is visible or shown:

```csharp
commandBarController1 = new CommandBarController();
commandBarController1.HostForm = this;  // Must be set first
commandBar1 = new CommandBar();
commandBarController1.CommandBars.Add(commandBar1);
this.Show();  // Form must be visible
```

**Issue: Child controls overlapping in CommandBar**

Solution: Use Panel container with proper layout and spacing:

```csharp
Panel container = new Panel();
container.Dock = DockStyle.Fill;
// Add controls with calculated positions
container.Controls.Add(control1);
container.Controls.Add(control2);
commandBar1.Controls.Add(container);
```

**Issue: Controls disappearing when switching dock state**

Solution: Ensure controls are properly added to CommandBar before state changes:

```csharp
// Add controls first
commandBar1.Controls.Add(myControl);

// Then change dock state
commandBar1.DockState = CommandBarDockState.Bottom;
```
