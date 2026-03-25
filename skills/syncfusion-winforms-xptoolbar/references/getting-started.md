# Getting Started with XPToolBar

The XPToolBar control is a Microsoft Visual Studio-inspired standalone toolbar control that provides a convenient way to load shortcut options and place them anywhere within your WinForms application. It offers a rich set of features including multiple BarItem types, flexible positioning, and extensive customization capabilities.

## Assembly Deployment

### Required Assemblies

To use the XPToolBar control in your application, you need to add the following assembly as the primary reference:

- **Syncfusion.Tools.Windows.dll** - Main assembly for XPToolBar

### Dependency Assemblies

The control also requires these dependent assemblies:

- Syncfusion.Grid.Base.dll
- Syncfusion.Grid.Windows.dll
- Syncfusion.Shared.Base.dll
- Syncfusion.Shared.Windows.dll
- Syncfusion.Tools.Base.dll
- Syncfusion.Licensing.dll

These assemblies can be found at the default installation location:
```
{System Drive}:\Program Files (x86)\Syncfusion\Essential Studio\{Platform}\{Build Version Number}\precompiledassemblies\{Framework Version Number}
```

### NuGet Package

Alternatively, you can install the control via NuGet package:

**Package Name:** `Syncfusion.Tools.WinForms`

This NuGet package includes all required assemblies and dependencies automatically.

> **Important:** Starting with v16.2.0.x, if you refer to Syncfusion assemblies from trial setup or from the NuGet feed, you must include a license key in your projects. Refer to the Syncfusion licensing documentation to learn about registering the license key in your Windows Forms application.

## Adding XPToolBar via Designer

You can add the XPToolBar control through the Visual Studio designer by following these steps:

### Step 1: Add Control to Form

1. Drag and drop the **XPToolBar** control from the toolbox (under the section "Syncfusion Windows Visual Studio Version Toolbox Essential Studio Version") into the designer page.

2. The control will be added to the application along with the required dependent assemblies automatically.

### Step 2: Add Container Panel

To position the toolbar, you need to add a container control such as a Panel:

1. Drag and drop a **Panel** control onto your form from the standard Windows Forms toolbox.

2. The XPToolBar will be hosted within this Panel to enable proper positioning.

> **Note:** The XPToolBar control cannot be directly dropped inside a Panel via the designer. Instead, place both controls in the designer and position them through code-behind.

### Step 3: Add BarItems Using Smart Tag

1. Click on the smart tag of the XPToolBar control to access quick actions.

2. Choose **Items Collection** from the smart tag menu.

3. This opens the **BarItem Collection Editor** window.

4. Click on the down arrow of the **Add** button to display different types of bar items (BarItem, ParentBarItem, DropDownBarItem, ComboBoxBarItem, etc.).

5. Select the appropriate bar item type as per your needs.

### Step 4: Configure BarItem Properties

In the BarItem Collection Editor window:

1. Set the **Text** property under **Appearance > Text** to define the display text.

2. Set the **Image** property under **Appearance > Image** to add an icon.

3. Configure other properties such as **ToolTip**, **Enabled**, etc., as needed.

### Alternative: Using Properties Panel

You can also add bar items by:

1. Right-clicking on the XPToolBar control in the designer and selecting **Properties**.

2. In the **Properties** panel, under **Misc > Items**, open the **BarItem Collection Editor**.

3. Add and configure bar items using the same process described above.

## Adding XPToolBar via Code

The XPToolBar control can be created and configured programmatically using the following approach:

### Step 1: Declare Controls

```csharp
// Control declarations
private Syncfusion.Windows.Forms.Tools.XPMenus.XPToolBar xpToolBar1;
private Syncfusion.Windows.Forms.Tools.XPMenus.BarItem barItem1;
private Syncfusion.Windows.Forms.Tools.XPMenus.ParentBarItem parentBarItem1;
private Syncfusion.Windows.Forms.Tools.XPMenus.DropDownBarItem dropDownBarItem1;
private System.Windows.Forms.Panel panel1;
```

```vb
' Control declarations
Private xpToolBar1 As Syncfusion.Windows.Forms.Tools.XPMenus.XPToolBar
Private barItem1 As Syncfusion.Windows.Forms.Tools.XPMenus.BarItem
Private parentBarItem1 As Syncfusion.Windows.Forms.Tools.XPMenus.ParentBarItem
Private dropDownBarItem1 As Syncfusion.Windows.Forms.Tools.XPMenus.DropDownBarItem
Private panel1 As System.Windows.Forms.Panel
```

### Step 2: Initialize Controls

```csharp
// Initialize controls
this.xpToolBar1 = new Syncfusion.Windows.Forms.Tools.XPMenus.XPToolBar();
this.barItem1 = new Syncfusion.Windows.Forms.Tools.XPMenus.BarItem();
this.parentBarItem1 = new Syncfusion.Windows.Forms.Tools.XPMenus.ParentBarItem();
this.dropDownBarItem1 = new Syncfusion.Windows.Forms.Tools.XPMenus.DropDownBarItem();
this.panel1 = new System.Windows.Forms.Panel();
```

```vb
' Initialize controls
Me.xpToolBar1 = New Syncfusion.Windows.Forms.Tools.XPMenus.XPToolBar()
Me.barItem1 = New Syncfusion.Windows.Forms.Tools.XPMenus.BarItem()
Me.parentBarItem1 = New Syncfusion.Windows.Forms.Tools.XPMenus.ParentBarItem()
Me.dropDownBarItem1 = New Syncfusion.Windows.Forms.Tools.XPMenus.DropDownBarItem()
Me.panel1 = New System.Windows.Forms.Panel()
```

### Step 3: Configure BarItems and Add to Toolbar

```csharp
// Configure bar items
this.barItem1.Text = "File";
this.parentBarItem1.Text = "Edit";
this.dropDownBarItem1.Text = "View";

// Add bar items to the XPToolBar
this.xpToolBar1.Bar.Items.AddRange(new Syncfusion.Windows.Forms.Tools.XPMenus.BarItem[] {
    this.barItem1,
    this.parentBarItem1,
    this.dropDownBarItem1
});
```

```vb
' Configure bar items
Me.barItem1.Text = "File"
Me.parentBarItem1.Text = "Edit"
Me.dropDownBarItem1.Text = "View"

' Add bar items to the XPToolBar
Me.xpToolBar1.Bar.Items.AddRange(New Syncfusion.Windows.Forms.Tools.XPMenus.BarItem() {
    Me.barItem1,
    Me.parentBarItem1,
    Me.dropDownBarItem1
})
```

### Step 4: Add Toolbar to Panel and Form

```csharp
// Add toolbar to panel
this.panel1.Controls.Add(this.xpToolBar1);

// Add panel to form
this.Controls.Add(this.panel1);
```

```vb
' Add toolbar to panel
Me.panel1.Controls.Add(Me.xpToolBar1)

' Add panel to form
Me.Controls.Add(Me.panel1)
```

## Adding BarItems

There are two primary ways to add bar items to the XPToolBar control:

### Using BarItem Collection Editor (Designer)

1. Open the **BarItem Collection Editor** using the smart tag or Properties panel.

2. Click the **Add** dropdown button to select the type of BarItem to add.

3. Configure the properties of each BarItem as needed.

4. Click **OK** to apply the changes.

### Programmatically via Bar.Items.Add

You can add bar items programmatically using the `Bar.Items.Add` method or `Bar.Items.AddRange` for multiple items:

```csharp
// Add single item
BarItem newItem = new BarItem();
newItem.Text = "Options";
this.xpToolBar1.Bar.Items.Add(newItem);

// Add multiple items
this.xpToolBar1.Bar.Items.AddRange(new BarItem[] {
    new BarItem() { Text = "New" },
    new BarItem() { Text = "Open" },
    new BarItem() { Text = "Save" }
});
```

```vb
' Add single item
Dim newItem As New BarItem()
newItem.Text = "Options"
Me.xpToolBar1.Bar.Items.Add(newItem)

' Add multiple items
Me.xpToolBar1.Bar.Items.AddRange(New BarItem() {
    New BarItem() With {.Text = "New"},
    New BarItem() With {.Text = "Open"},
    New BarItem() With {.Text = "Save"}
})
```

## Positioning Requirements

### Panel/Container Usage (REQUIRED)

The XPToolBar control **must** be hosted within a container control such as a Panel, GroupBox, TabControl, or similar control. Direct placement on the form is not supported.

### Why Panel is Needed

The Panel provides:
- Proper layout management for the toolbar
- Consistent positioning across different form sizes
- Support for docking and anchoring behaviors
- Ability to stack multiple toolbars

### Dock Property Configuration

Configure the Panel and/or XPToolBar Dock properties to control positioning:

```csharp
// Dock panel at top of form
this.panel1.Dock = DockStyle.Top;

// Dock toolbar at top of panel
this.xpToolBar1.Dock = DockStyle.Top;
```

```vb
' Dock panel at top of form
Me.panel1.Dock = DockStyle.Top

' Dock toolbar at top of panel
Me.xpToolBar1.Dock = DockStyle.Top
```

## Basic Setup Example

Here's a complete minimal example that creates an XPToolBar with three menu items and handles click events:

```csharp
using Syncfusion.Windows.Forms.Tools.XPMenus;

public partial class Form1 : Form
{
    private XPToolBar xpToolBar1;
    private BarItem fileItem;
    private BarItem editItem;
    private BarItem viewItem;
    private Panel panel1;

    public Form1()
    {
        InitializeComponent();
        InitializeToolbar();
    }

    private void InitializeToolbar()
    {
        // Create panel
        this.panel1 = new Panel();
        this.panel1.Dock = DockStyle.Top;
        this.panel1.Height = 32;

        // Create toolbar
        this.xpToolBar1 = new XPToolBar();
        this.xpToolBar1.Dock = DockStyle.Top;

        // Create bar items
        this.fileItem = new BarItem();
        this.fileItem.Text = "File";
        this.fileItem.Click += FileItem_Click;

        this.editItem = new BarItem();
        this.editItem.Text = "Edit";
        this.editItem.Click += EditItem_Click;

        this.viewItem = new BarItem();
        this.viewItem.Text = "View";
        this.viewItem.Click += ViewItem_Click;

        // Add items to toolbar
        this.xpToolBar1.Bar.Items.AddRange(new BarItem[] {
            this.fileItem,
            this.editItem,
            this.viewItem
        });

        // Add toolbar to panel
        this.panel1.Controls.Add(this.xpToolBar1);

        // Add panel to form
        this.Controls.Add(this.panel1);
    }

    private void FileItem_Click(object sender, EventArgs e)
    {
        MessageBox.Show("File clicked!");
    }

    private void EditItem_Click(object sender, EventArgs e)
    {
        MessageBox.Show("Edit clicked!");
    }

    private void ViewItem_Click(object sender, EventArgs e)
    {
        MessageBox.Show("View clicked!");
    }
}
```

```vb
Imports Syncfusion.Windows.Forms.Tools.XPMenus

Public Partial Class Form1
    Inherits Form
    
    Private xpToolBar1 As XPToolBar
    Private fileItem As BarItem
    Private editItem As BarItem
    Private viewItem As BarItem
    Private panel1 As Panel

    Public Sub New()
        InitializeComponent()
        InitializeToolbar()
    End Sub

    Private Sub InitializeToolbar()
        ' Create panel
        Me.panel1 = New Panel()
        Me.panel1.Dock = DockStyle.Top
        Me.panel1.Height = 32

        ' Create toolbar
        Me.xpToolBar1 = New XPToolBar()
        Me.xpToolBar1.Dock = DockStyle.Top

        ' Create bar items
        Me.fileItem = New BarItem()
        Me.fileItem.Text = "File"
        AddHandler Me.fileItem.Click, AddressOf FileItem_Click

        Me.editItem = New BarItem()
        Me.editItem.Text = "Edit"
        AddHandler Me.editItem.Click, AddressOf EditItem_Click

        Me.viewItem = New BarItem()
        Me.viewItem.Text = "View"
        AddHandler Me.viewItem.Click, AddressOf ViewItem_Click

        ' Add items to toolbar
        Me.xpToolBar1.Bar.Items.AddRange(New BarItem() {
            Me.fileItem,
            Me.editItem,
            Me.viewItem
        })

        ' Add toolbar to panel
        Me.panel1.Controls.Add(Me.xpToolBar1)

        ' Add panel to form
        Me.Controls.Add(Me.panel1)
    End Sub

    Private Sub FileItem_Click(ByVal sender As Object, ByVal e As EventArgs)
        MessageBox.Show("File clicked!")
    End Sub

    Private Sub EditItem_Click(ByVal sender As Object, ByVal e As EventArgs)
        MessageBox.Show("Edit clicked!")
    End Sub

    Private Sub ViewItem_Click(ByVal sender As Object, ByVal e As EventArgs)
        MessageBox.Show("View clicked!")
    End Sub
End Class
```

This example demonstrates:
- Creating a Panel container
- Initializing the XPToolBar
- Adding multiple BarItems with text
- Handling Click events for each item
- Proper hosting structure (Panel → Toolbar → Form)

With this basic setup in place, you can extend the toolbar by adding more sophisticated BarItem types, configuring appearance properties, and implementing complex menu structures.
