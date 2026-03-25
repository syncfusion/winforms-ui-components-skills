# Getting Started with BorderLayout

## Table of Contents
- [Assembly and NuGet Setup](#assembly-and-nuget-setup)
- [Designer-Based Setup](#designer-based-setup)
- [Code-Based Setup](#code-based-setup)
- [Adding Child Controls](#adding-child-controls)
- [Container Configuration](#container-configuration)

## Assembly and NuGet Setup

### Required Assembly
The BorderLayout control requires the following assembly:
- **Syncfusion.Shared.Base.dll**

### NuGet Package Installation
Install the NuGet package in your Windows Forms project:

```powershell
Install-Package Syncfusion.Shared.Base
```

Or search for "Syncfusion.Shared.Base" in the NuGet Package Manager.

### Namespace Import
Add the following using statement to your code files:

**C#:**
```csharp
using Syncfusion.Windows.Forms.Tools;
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms.Tools
```

## Designer-Based Setup

### Step 1: Drag BorderLayout to Form
1. Open your Windows Forms designer
2. Locate BorderLayout in the Toolbox (under Syncfusion tools)
3. Drag it onto your form
4. A dialog appears asking to add the form as a container control - click **Yes**

### Step 2: Set Container Control
The form is automatically set as the container control when you click Yes in the dialog. The property window shows:
- **ContainerControl**: `<Form Name>`

### Step 3: Add Child Controls
1. Drag controls from the toolbox (buttons, panels, labels, etc.) onto the form
2. Each child control now has a "Position on borderLayout" property
3. Set the position for each control in the Properties window:
   - Select a child control
   - In the Properties panel, find "Position on borderLayout"
   - Choose from: North, South, East, West, Center

### Step 4: Configure Spacing (Optional)
In the Properties window of BorderLayout:
- **HGap**: Set horizontal spacing between controls (in pixels)
- **VGap**: Set vertical spacing between controls (in pixels)

### Example: Designer-Based Setup
```
1. Add BorderLayout to form
2. Set ContainerControl = Form1
3. Add Panel (header) → Set Position = North, Height = 50
4. Add Panel (sidebar) → Set Position = West, Width = 200
5. Add Panel (content) → Set Position = Center
6. Add Panel (footer) → Set Position = South, Height = 40
7. Set HGap = 10, VGap = 10
```

## Code-Based Setup

### Step 1: Create the Project
Create a new Windows Forms Application in Visual Studio.

### Step 2: Add Namespace
```csharp
using Syncfusion.Windows.Forms.Tools;
```

### Step 3: Create BorderLayout Instance
In your form's Load event or constructor (after InitializeComponent):

**C#:**
```csharp
BorderLayout borderLayout1 = new BorderLayout();
this.borderLayout1.ContainerControl = this;
```

**VB.NET:**
```vb
Dim borderLayout1 As BorderLayout = New BorderLayout()
Me.borderLayout1.ContainerControl = Me
```

### Step 4: Add to Form Controls (Optional)
If you want to access the BorderLayout later, add it to the form's controls:

**C#:**
```csharp
this.Controls.Add(borderLayout1);
```

**VB.NET:**
```vb
Me.Controls.Add(borderLayout1)
```

## Adding Child Controls

### Basic Example: Three Controls
```csharp
// Create controls
ButtonAdv topButton = new ButtonAdv();
topButton.Text = "Top Panel";
topButton.Dock = DockStyle.Top;

ButtonAdv centerButton = new ButtonAdv();
centerButton.Text = "Center Content";

ButtonAdv bottomButton = new ButtonAdv();
bottomButton.Text = "Bottom Panel";
bottomButton.Dock = DockStyle.Bottom;

// Add to form
this.Controls.Add(topButton);
this.Controls.Add(centerButton);
this.Controls.Add(bottomButton);

// Position with BorderLayout
borderLayout1.SetPosition(topButton, BorderPosition.North);
borderLayout1.SetPosition(centerButton, BorderPosition.Center);
borderLayout1.SetPosition(bottomButton, BorderPosition.South);
```

### VB.NET Equivalent
```vb
' Create controls
Dim topButton As ButtonAdv = New ButtonAdv()
topButton.Text = "Top Panel"
topButton.Dock = DockStyle.Top

Dim centerButton As ButtonAdv = New ButtonAdv()
centerButton.Text = "Center Content"

Dim bottomButton As ButtonAdv = New ButtonAdv()
bottomButton.Text = "Bottom Panel"
bottomButton.Dock = DockStyle.Bottom

' Add to form
Me.Controls.Add(topButton)
Me.Controls.Add(centerButton)
Me.Controls.Add(bottomButton)

' Position with BorderLayout
borderLayout1.SetPosition(topButton, BorderPosition.North)
borderLayout1.SetPosition(centerButton, BorderPosition.Center)
borderLayout1.SetPosition(bottomButton, BorderPosition.South)
```

### Using Panel Controls
Panel controls work well with BorderLayout:

```csharp
// Create panels
Panel headerPanel = new Panel() { BackColor = Color.LightBlue, Height = 50 };
Panel sidebarPanel = new Panel() { BackColor = Color.LightGray, Width = 200 };
Panel contentPanel = new Panel() { BackColor = Color.White };
Panel footerPanel = new Panel() { BackColor = Color.LightBlue, Height = 40 };

// Add labels for clarity
Label headerLabel = new Label() { Text = "Header", Dock = DockStyle.Fill };
Label sidebarLabel = new Label() { Text = "Sidebar", Dock = DockStyle.Fill };
Label contentLabel = new Label() { Text = "Main Content", Dock = DockStyle.Fill };
Label footerLabel = new Label() { Text = "Footer", Dock = DockStyle.Fill };

headerPanel.Controls.Add(headerLabel);
sidebarPanel.Controls.Add(sidebarLabel);
contentPanel.Controls.Add(contentLabel);
footerPanel.Controls.Add(footerLabel);

// Add panels to form
this.Controls.Add(headerPanel);
this.Controls.Add(sidebarPanel);
this.Controls.Add(contentPanel);
this.Controls.Add(footerPanel);

// Position with BorderLayout
borderLayout1.SetPosition(headerPanel, BorderPosition.North);
borderLayout1.SetPosition(sidebarPanel, BorderPosition.West);
borderLayout1.SetPosition(contentPanel, BorderPosition.Center);
borderLayout1.SetPosition(footerPanel, BorderPosition.South);
```

## Container Configuration

### What is ContainerControl?
ContainerControl specifies which control acts as the parent container for the BorderLayout. All child controls must be added to this container.

### Setting ContainerControl

**C#:**
```csharp
borderLayout1.ContainerControl = this;  // this = Form
// or
borderLayout1.ContainerControl = myPanel;  // Use a panel as container
```

**VB.NET:**
```vb
borderLayout1.ContainerControl = Me  ' Me = Form
' or
borderLayout1.ContainerControl = myPanel  ' Use a panel as container
```

### Typical Scenario
Usually, you set the form itself as the container:

```csharp
public partial class MainForm : Form
{
    private BorderLayout borderLayout1;

    public MainForm()
    {
        InitializeComponent();
        
        // Create and configure BorderLayout
        borderLayout1 = new BorderLayout();
        borderLayout1.ContainerControl = this;  // Form is container
        
        // Add child controls and set positions...
    }
}
```

### Container Requirements
- **Must be a Control**: Typically a Form or Panel
- **Must be set before positioning**: Set ContainerControl before calling SetPosition()
- **All children must belong to container**: Add controls to the same container you set in ContainerControl

## Complete Getting Started Example

Here's a complete, runnable example:

**C#:**
```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class Form1 : Form
{
    private BorderLayout borderLayout1;

    public Form1()
    {
        InitializeComponent();
        this.Text = "BorderLayout Example";
        this.Size = new System.Drawing.Size(600, 400);
    }

    protected override void OnLoad(EventArgs e)
    {
        base.OnLoad(e);

        // Create BorderLayout
        borderLayout1 = new BorderLayout();
        borderLayout1.ContainerControl = this;

        // Create panels
        Panel headerPanel = new Panel() { BackColor = System.Drawing.Color.LightBlue, Height = 50 };
        Panel contentPanel = new Panel() { BackColor = System.Drawing.Color.White };
        Panel footerPanel = new Panel() { BackColor = System.Drawing.Color.LightBlue, Height = 40 };

        // Add labels
        headerPanel.Controls.Add(new Label() { Text = "Header", Dock = DockStyle.Fill });
        contentPanel.Controls.Add(new Label() { Text = "Main Content", Dock = DockStyle.Fill });
        footerPanel.Controls.Add(new Label() { Text = "Footer", Dock = DockStyle.Fill });

        // Add to form
        this.Controls.Add(headerPanel);
        this.Controls.Add(contentPanel);
        this.Controls.Add(footerPanel);

        // Position and configure spacing
        borderLayout1.SetPosition(headerPanel, BorderPosition.North);
        borderLayout1.SetPosition(contentPanel, BorderPosition.Center);
        borderLayout1.SetPosition(footerPanel, BorderPosition.South);
        borderLayout1.HGap = 10;
        borderLayout1.VGap = 10;
    }
}
```

**VB.NET:**
```vb
Imports System
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Public Partial Class Form1
    Inherits Form
    
    Private borderLayout1 As BorderLayout

    Public Sub New()
        InitializeComponent()
        Me.Text = "BorderLayout Example"
        Me.Size = New System.Drawing.Size(600, 400)
    End Sub

    Protected Overrides Sub OnLoad(e As EventArgs)
        MyBase.OnLoad(e)

        ' Create BorderLayout
        borderLayout1 = New BorderLayout()
        borderLayout1.ContainerControl = Me

        ' Create panels
        Dim headerPanel As Panel = New Panel() With {.BackColor = System.Drawing.Color.LightBlue, .Height = 50}
        Dim contentPanel As Panel = New Panel() With {.BackColor = System.Drawing.Color.White}
        Dim footerPanel As Panel = New Panel() With {.BackColor = System.Drawing.Color.LightBlue, .Height = 40}

        ' Add labels
        headerPanel.Controls.Add(New Label() With {.Text = "Header", .Dock = DockStyle.Fill})
        contentPanel.Controls.Add(New Label() With {.Text = "Main Content", .Dock = DockStyle.Fill})
        footerPanel.Controls.Add(New Label() With {.Text = "Footer", .Dock = DockStyle.Fill})

        ' Add to form
        Me.Controls.Add(headerPanel)
        Me.Controls.Add(contentPanel)
        Me.Controls.Add(footerPanel)

        ' Position and configure spacing
        borderLayout1.SetPosition(headerPanel, BorderPosition.North)
        borderLayout1.SetPosition(contentPanel, BorderPosition.Center)
        borderLayout1.SetPosition(footerPanel, BorderPosition.South)
        borderLayout1.HGap = 10
        borderLayout1.VGap = 10
    End Sub
End Class
```

Run this and you'll see a form with a light blue header, white content area, and light blue footer separated by 10-pixel gaps.
