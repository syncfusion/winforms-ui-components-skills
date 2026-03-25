# Getting Started with GridBagLayout

## Assembly Deployment

To use GridBagLayout in your Windows Forms application, you need to reference the Syncfusion assembly.

### Required Assembly
- **Syncfusion.Shared.Base.dll**

### NuGet Package Installation

Install the NuGet package in your Visual Studio project:

```
Install-Package Syncfusion.Shared.Base
```

Or through the NuGet Package Manager UI:
1. Open NuGet Package Manager
2. Search for "Syncfusion.Shared.WinForms"
3. Click Install

For detailed installation instructions, refer to the Syncfusion Windows Forms installation guide.

## Creating Your First Project

### Step 1: Create a Windows Forms Application

In Visual Studio:
1. File → New → Project
2. Select **Windows Forms App (.NET Framework)** or **.NET Core**
3. Name your project
4. Click Create

### Step 2: Add Required Reference

In your project, add the reference to `Syncfusion.Shared.Base.dll`:

**Via Package Manager:**
```powershell
Install-Package Syncfusion.Shared.Base
```

**Via .csproj file:**
```xml
<ItemGroup>
  <Reference Include="Syncfusion.Shared.Base">
    <HintPath>path\to\Syncfusion.Shared.Base.dll</HintPath>
  </Reference>
</ItemGroup>
```

### Step 3: Add Using Statement

In your form code, add the namespace:

```csharp
using Syncfusion.Windows.Forms.Tools;
```

For VB.NET:
```vb
Imports Syncfusion.Windows.Forms.Tools
```

## Adding GridBagLayout Through Designer

### Designer Approach

1. **Open the Toolbox** in Visual Studio
2. **Find GridBagLayout** in the Syncfusion Tools section
3. **Drag GridBagLayout** onto your form
4. A dialog appears automatically asking to confirm the form as container control
5. Click **Yes** to accept the form as the container

### Adding Child Controls Through Designer

1. Once GridBagLayout is set, open the Toolbox again
2. Drag any control (Button, TextBox, Label, etc.) onto the form
3. The control is automatically managed by GridBagLayout
4. Use the **Properties panel** to set GridBagConstraints:
   - GridPostX, GridPostY (position in grid)
   - CellSpanX, CellSpanY (spanning)
   - WeightX, WeightY (space distribution)
   - Anchor, Fill (alignment and sizing)

## Adding GridBagLayout Through Code

### Creating GridBagLayout Programmatically

```csharp
// Create the layout manager instance
GridBagLayout gridBagLayout1 = new GridBagLayout();

// Set the container control (usually the form)
gridBagLayout1.ContainerControl = this;
```

For VB.NET:
```vb
Dim gridBagLayout1 As GridBagLayout = New GridBagLayout()
Me.gridBagLayout1.ContainerControl = Me
```

### Adding Child Controls Through Code

```csharp
// Create controls
ButtonAdv buttonAdv1 = new ButtonAdv();
ButtonAdv buttonAdv2 = new ButtonAdv();
ButtonAdv buttonAdv3 = new ButtonAdv();

// Set properties
buttonAdv1.Text = "Button 1";
buttonAdv2.Text = "Button 2";
buttonAdv3.Text = "Button 3";

// Add to form (GridBagLayout automatically manages them)
this.Controls.Add(buttonAdv1);
this.Controls.Add(buttonAdv2);
this.Controls.Add(buttonAdv3);
```

For VB.NET:
```vb
Dim buttonAdv1 As ButtonAdv = New ButtonAdv()
Dim buttonAdv2 As ButtonAdv = New ButtonAdv()
Dim buttonAdv3 As ButtonAdv = New ButtonAdv()

buttonAdv1.Text = "Button 1"
buttonAdv2.Text = "Button 2"
buttonAdv3.Text = "Button 3"

Me.Controls.Add(buttonAdv1)
Me.Controls.Add(buttonAdv2)
Me.Controls.Add(buttonAdv3)
```

## Basic Layout Example

Here's a complete example setting up a simple 2×2 grid:

```csharp
public partial class Form1 : Form
{
    private GridBagLayout gridBagLayout1;
    
    public Form1()
    {
        InitializeComponent();
        SetupLayout();
    }
    
    private void SetupLayout()
    {
        // Create and configure layout
        gridBagLayout1 = new GridBagLayout();
        gridBagLayout1.ContainerControl = this;
        
        // Create buttons
        ButtonAdv[] buttons = new ButtonAdv[4];
        for (int i = 0; i < 4; i++)
        {
            buttons[i] = new ButtonAdv { Text = $"Button {i + 1}" };
            this.Controls.Add(buttons[i]);
        }
        
        // Layout in 2x2 grid
        int row = 0, col = 0;
        for (int i = 0; i < 4; i++)
        {
            gridBagLayout1.SetConstraints(buttons[i], 
                new GridBagConstraints(col, row, 1, 1, 1, 1, 
                    AnchorTypes.Center, FillType.Both, 
                    new Insets(5, 5, 5, 5), 0, 0, false));
            
            col++;
            if (col == 2) { col = 0; row++; }
        }
    }
}
```

## Rearranging Controls at Design Time

GridBagLayout supports drag-and-drop rearrangement of controls:

1. **Open Form Designer**
2. **Click and drag a control** to a new position
3. **Release to drop** in the new location
4. The layout automatically updates

This works similar to other layout managers in Windows Forms.

## Common Setup Issues & Solutions

### Issue: Controls not visible after adding GridBagLayout

**Solution:** Ensure the ContainerControl is set to the form or parent control:
```csharp
gridBagLayout1.ContainerControl = this;
```

### Issue: Assembly reference errors

**Solution:** Verify all required assemblies are installed:
- Syncfusion.Shared.Base.dll
- Check NuGet package version matches your framework

### Issue: Designer dialog not appearing

**Solution:** Make sure the form is selected (not a panel or other container) before dragging GridBagLayout from the Toolbox.

### Issue: Controls overlapping or misaligned

**Solution:** Verify GridBagConstraints are set correctly. See child-control-constraints reference for proper constraint setup.
