# Getting Started with FlowLayout

## Assembly References

### Required Assemblies

To use FlowLayout in your Windows Forms application, add the following assembly reference:
- **Syncfusion.Shared.Base** - Core layout manager functionality

### NuGet Installation

Install via Package Manager Console:
```powershell
Install-Package Syncfusion.Shared.Base
```

Or use Package Manager UI in Visual Studio to search for "Syncfusion.Shared.Base".

### Namespace Import

Import the FlowLayout namespace in your code:

**C#:**
```csharp
using Syncfusion.Windows.Forms.Tools;
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms.Tools
```

## Adding FlowLayout via Designer

### Step 1: Create a Windows Forms Application

1. Create a new Windows Forms Application project in Visual Studio
2. Open the form in design view

### Step 2: Add FlowLayout from Toolbox

1. Locate the **FlowLayout** component in the Toolbox (Syncfusion section)
2. Drag it to your form
3. When prompted, confirm that the form should be the container control

**Note:** The FlowLayout will automatically set the form as its `ContainerControl`.

### Step 3: Add Child Controls

1. Drag UI controls (buttons, textboxes, etc.) from the Toolbox to the form
2. The FlowLayout will automatically arrange them based on its configuration
3. Adjust control size and properties as needed

## Adding FlowLayout via Code

### Step 1: Include Namespace

Add the namespace import to your form:

```csharp
using Syncfusion.Windows.Forms.Tools;
```

### Step 2: Create FlowLayout Instance

In your form constructor or designer-generated code:

```csharp
FlowLayout flowLayout1 = new FlowLayout();
flowLayout1.ContainerControl = this;
```

**VB.NET:**
```vb
Dim flowLayout1 As FlowLayout = New FlowLayout()
Me.flowLayout1.ContainerControl = Me
```

### Step 3: Configure Basic Properties

Set initial layout properties:

```csharp
flowLayout1.LayoutMode = FlowLayoutMode.Horizontal;
flowLayout1.HGap = 10;  // Horizontal spacing
flowLayout1.VGap = 10;  // Vertical spacing
```

## Adding Child Controls

Child controls can be added in two ways:

### Method 1: Designer Approach
Simply add controls to the form from the Toolbox. FlowLayout automatically layouts them in the order they're added.

### Method 2: Programmatic Approach

Add controls to the container's Controls collection:

```csharp
// Create controls
Button btn1 = new Button();
btn1.Text = "Save";
btn1.Size = new Size(80, 30);

Button btn2 = new Button();
btn2.Text = "Cancel";
btn2.Size = new Size(80, 30);

// Add to form (automatically managed by FlowLayout)
this.Controls.Add(btn1);
this.Controls.Add(btn2);
```

**VB.NET:**
```vb
' Create controls
Dim btn1 As Button = New Button()
btn1.Text = "Save"
btn1.Size = New Size(80, 30)

Dim btn2 As Button = New Button()
btn2.Text = "Cancel"
btn2.Size = New Size(80, 30)

' Add to form
Me.Controls.Add(btn1)
Me.Controls.Add(btn2)
```

**Important:** Controls are laid out in the order they are added to the Controls collection. Use the designer's "Bring to Front" or "Send to Back" options to reorder them.

## Basic Configuration

### Set Container Control

The container control is where child controls are arranged. This is typically the form:

```csharp
flowLayout1.ContainerControl = this;
```

Or use a Panel as the container:

```csharp
FlowLayout flowLayout1 = new FlowLayout();
flowLayout1.ContainerControl = panel1;
```

### Set Layout Mode

Choose between horizontal and vertical arrangement:

**Horizontal (Default):**
```csharp
flowLayout1.LayoutMode = FlowLayoutMode.Horizontal;
```

**Vertical:**
```csharp
flowLayout1.LayoutMode = FlowLayoutMode.Vertical;
```

### Configure Spacing

Set gaps between controls:

```csharp
flowLayout1.HGap = 15;  // 15 pixels horizontal gap
flowLayout1.VGap = 10;  // 10 pixels vertical gap
```

## Complete Example

Here's a minimal working example:

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class MainForm : Form
{
    private FlowLayout flowLayout1;
    
    public MainForm()
    {
        InitializeComponent();
        
        // Create FlowLayout
        flowLayout1 = new FlowLayout();
        flowLayout1.ContainerControl = this;
        flowLayout1.LayoutMode = FlowLayoutMode.Horizontal;
        flowLayout1.HGap = 10;
        flowLayout1.VGap = 10;
        
        // Create buttons
        for (int i = 1; i <= 4; i++)
        {
            Button btn = new Button
            {
                Text = "Button " + i,
                Size = new Size(80, 30)
            };
            this.Controls.Add(btn);
        }
    }
}
```

**Result:** Four buttons arranged horizontally with 10-pixel gaps, automatically wrapping to new rows as needed.

## Next Steps

- **Layout Modes:** Read [layout-modes.md](layout-modes.md) to configure horizontal/vertical arrangement and reverse flow
- **Spacing:** Read [spacing-and-sizing.md](spacing-and-sizing.md) to manage gaps and auto-sizing
- **Alignment:** Read [alignment-options.md](alignment-options.md) for centering and constraint-based layouts
- **Advanced:** Read [child-control-constraints.md](child-control-constraints.md) for complex form layouts
