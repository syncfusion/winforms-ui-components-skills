# Getting Started with Docking Manager

This guide covers the basic setup and configuration of the Syncfusion WinForms DockingManager control.

## Assembly Deployment

Add the following assemblies as references to use DockingManager:

- `Syncfusion.Grid.Base.dll`
- `Syncfusion.Grid.Windows.dll`
- `Syncfusion.Shared.Base.dll`
- `Syncfusion.Shared.Windows.dll`
- `Syncfusion.Tools.Base.dll`
- `Syncfusion.Tools.Windows.dll`

**NuGet Package Installation:**

Install the `Syncfusion.Tools.Windows` NuGet package to automatically add all required assemblies.

```powershell
Install-Package Syncfusion.Tools.Windows
```

## Adding DockingManager via Designer

1. Drag the `DockingManager` control from the toolbox and drop it onto the form
2. The control appears in the component tray (non-visual component)
3. Required assemblies are added automatically
4. The `HostControl` property is automatically set to the parent form

## Adding DockingManager in Code

### Step 1: Create DockingManager Instance

```csharp
using Syncfusion.Windows.Forms.Tools;

public partial class Form1 : Form
{
    private DockingManager dockingManager1;
    
    public Form1()
    {
        InitializeComponent();
        
        // Create DockingManager instance
        this.components = new System.ComponentModel.Container();
        this.dockingManager1 = new DockingManager(this.components);
        
        // Set the host control (parent form)
        this.dockingManager1.HostControl = this;
    }
}
```

**VB.NET:**

```vb
Imports Syncfusion.Windows.Forms.Tools

Public Class Form1
    Private dockingManager1 As DockingManager
    
    Public Sub New()
        InitializeComponent()
        
        ' Create DockingManager instance
        Me.components = New System.ComponentModel.Container()
        Me.dockingManager1 = New DockingManager(Me.components)
        
        ' Set the host control
        Me.dockingManager1.HostControl = Me
    End Sub
End Class
```

## Adding Dock Child Windows

Any control can be converted to a docking window by enabling docking functionality.

### Step 1: Create Controls

```csharp
private Panel panel1;
private Panel panel2;
private Panel panel3;
private Panel panel4;

this.panel1 = new Panel();
this.panel2 = new Panel();
this.panel3 = new Panel();
this.panel4 = new Panel();

// Add controls to form
this.Controls.AddRange(new Control[] { 
    panel1, panel2, panel3, panel4 
});
```

### Step 2: Enable Docking

Use the `SetEnableDocking` method to enable docking for controls:

```csharp
// Enable docking for panels
this.dockingManager1.SetEnableDocking(panel1, true);
this.dockingManager1.SetEnableDocking(panel2, true);
this.dockingManager1.SetEnableDocking(panel3, true);
this.dockingManager1.SetEnableDocking(panel4, true);
```

**VB.NET:**

```vb
' Enable docking for panels
Me.dockingManager1.SetEnableDocking(panel1, True)
Me.dockingManager1.SetEnableDocking(panel2, True)
Me.dockingManager1.SetEnableDocking(panel3, True)
Me.dockingManager1.SetEnableDocking(panel4, True)
```

### Checking if Panel is Docking

Use `GetEnableDocking` to verify if a control has docking enabled:

```csharp
bool isDocking = this.dockingManager1.GetEnableDocking(panel1);
Console.WriteLine($"Panel1 docking enabled: {isDocking}");
```

## Setting Dock Labels (Headers)

The `SetDockLabel` method sets the caption text for dock windows:

```csharp
// Set labels for docked controls
this.dockingManager1.SetDockLabel(panel1, "Solution Explorer");
this.dockingManager1.SetDockLabel(panel2, "Toolbox");
this.dockingManager1.SetDockLabel(panel3, "Properties");
this.dockingManager1.SetDockLabel(panel4, "Output");
```

**VB.NET:**

```vb
' Set labels for docked controls
Me.dockingManager1.SetDockLabel(panel1, "Solution Explorer")
Me.dockingManager1.SetDockLabel(panel2, "Toolbox")
Me.dockingManager1.SetDockLabel(panel3, "Properties")
Me.dockingManager1.SetDockLabel(panel4, "Output")
```

### Getting Dock Label

Retrieve the label of a dock window:

```csharp
string label = this.dockingManager1.GetDockLabel(panel1);
Console.WriteLine($"Panel1 label: {label}");
```

## Arranging Dock Windows

Use the `DockControl` method to position dock windows at specific sides:

```csharp
// Dock to top side with height of 100
this.dockingManager1.DockControl(panel1, this, DockingStyle.Top, 100);

// Dock to bottom side with height of 100
this.dockingManager1.DockControl(panel3, this, DockingStyle.Bottom, 100);

// Dock to left side with width of 200
this.dockingManager1.DockControl(panel2, this, DockingStyle.Left, 200);

// Dock to right side with width of 200
this.dockingManager1.DockControl(panel1, this, DockingStyle.Right, 200);

// Tab panel4 with panel1 (same group)
this.dockingManager1.DockControl(panel4, panel1, DockingStyle.Tabbed, 200);
```

**Available DockingStyle Values:**
- `DockingStyle.Left` - Dock to left side
- `DockingStyle.Right` - Dock to right side
- `DockingStyle.Top` - Dock to top side
- `DockingStyle.Bottom` - Dock to bottom side
- `DockingStyle.Tabbed` - Tab with another control
- `DockingStyle.Fill` - Fill entire area

## Adding Icons to Dock Windows

### Using Icon File

```csharp
// Load icon from file
System.Drawing.Icon icon = new System.Drawing.Icon(@"C:\Icons\myicon.ico");

// Enable caption images
this.dockingManager1.ShowCaptionImages = true;

// Set icon for dock window
this.dockingManager1.SetDockIcon(panel1, icon);
```

### Using ImageList

```csharp
// Create and populate ImageList
ImageList imageList = new ImageList();
imageList.Images.Add(Image.FromFile(@"C:\Icons\icon1.png"));
imageList.Images.Add(Image.FromFile(@"C:\Icons\icon2.png"));

// Assign ImageList to DockingManager
this.dockingManager1.ImageList = imageList;
this.dockingManager1.ShowCaptionImages = true;

// Set icon by index
this.dockingManager1.SetDockIcon(panel1, 0); // First image
this.dockingManager1.SetDockIcon(panel2, 1); // Second image
```

### Hide/Show Caption Images

```csharp
// Show caption images
this.dockingManager1.ShowCaptionImages = true;

// Hide caption images
this.dockingManager1.ShowCaptionImages = false;
```

## Basic Dock State Changes

### Floating a Control

```csharp
// Float control at specific location and size
Rectangle bounds = new Rectangle(100, 100, 300, 400);
this.dockingManager1.FloatControl(panel3, bounds);

// Or float at screen position
Rectangle rectangle = this.Bounds;
this.dockingManager1.FloatControl(panel3, 
    new Rectangle(rectangle.Right - 300, rectangle.Bottom - 300, 200, 200));
```

### Auto-Hiding a Control

```csharp
// Set control to auto-hide state
this.dockingManager1.SetAutoHideMode(panel1, true);

// Remove auto-hide state
this.dockingManager1.SetAutoHideMode(panel1, false);
```

## Complete Example

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class Form1 : Form
{
    private DockingManager dockingManager1;
    private Panel solutionExplorer;
    private Panel toolbox;
    private Panel properties;
    private Panel output;
    
    public Form1()
    {
        InitializeComponent();
        InitializeDocking();
    }
    
    private void InitializeDocking()
    {
        // Create DockingManager
        this.dockingManager1 = new DockingManager(this.components);
        this.dockingManager1.HostControl = this;
        
        // Create panels
        this.solutionExplorer = new Panel() { BackColor = Color.LightBlue };
        this.toolbox = new Panel() { BackColor = Color.LightGreen };
        this.properties = new Panel() { BackColor = Color.LightYellow };
        this.output = new Panel() { BackColor = Color.LightGray };
        
        // Add to form
        this.Controls.AddRange(new Control[] {
            solutionExplorer, toolbox, properties, output
        });
        
        // Enable docking
        this.dockingManager1.SetEnableDocking(solutionExplorer, true);
        this.dockingManager1.SetEnableDocking(toolbox, true);
        this.dockingManager1.SetEnableDocking(properties, true);
        this.dockingManager1.SetEnableDocking(output, true);
        
        // Set labels
        this.dockingManager1.SetDockLabel(solutionExplorer, "Solution Explorer");
        this.dockingManager1.SetDockLabel(toolbox, "Toolbox");
        this.dockingManager1.SetDockLabel(properties, "Properties");
        this.dockingManager1.SetDockLabel(output, "Output");
        
        // Arrange windows
        this.dockingManager1.DockControl(solutionExplorer, this, 
            DockingStyle.Right, 250);
        this.dockingManager1.DockControl(toolbox, this, 
            DockingStyle.Left, 200);
        this.dockingManager1.DockControl(properties, solutionExplorer, 
            DockingStyle.Tabbed, 200);
        this.dockingManager1.DockControl(output, this, 
            DockingStyle.Bottom, 150);
        
        // Apply visual style
        this.dockingManager1.VisualStyle = VisualStyle.Office2016Colorful;
    }
}
```

## Design-Time Features

When using the designer, you can:

1. **Drag panels at design time** - Arrange docking layout visually
2. **Set properties** - Configure DockingManager properties in property grid
3. **Add caption buttons** - Use CaptionButtons collection editor
4. **Configure dock labels** - Right-click dock window, select properties

## Next Steps

- Learn about dock states (docked, floating, auto-hide, tabbed) in `dock-states.md`
- Implement document windows (TDI/MDI) in `document-windows.md`
- Customize caption buttons in `caption-buttons.md`
- Apply visual styles and themes in `appearance-customization.md`
