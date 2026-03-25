# Getting Started with RangeSlider

## Table of Contents
- [Assembly Deployment](#assembly-deployment)
- [Adding via Designer](#adding-via-designer)
- [Adding via Code](#adding-via-code)
- [Basic Configuration](#basic-configuration)

## Assembly Deployment

### Required Assemblies

To use the RangeSlider control, add the following assembly references to your project:

- `Syncfusion.Grid.Base`
- `Syncfusion.Grid.Windows`
- `Syncfusion.Shared.Base`
- `Syncfusion.Shared.Windows`
- `Syncfusion.Tools.Base`
- `Syncfusion.Tools.Windows`

### NuGet Installation

Install via Package Manager Console:

```powershell
Install-Package Syncfusion.Tools.Windows
```

This package includes all required dependencies and will automatically add the necessary assemblies to your project.

### Control Dependencies

Refer to the [control dependencies documentation](https://help.syncfusion.com/windowsforms/control-dependencies#rangeslider) for the complete list of required assemblies for your specific Syncfusion version.

## Adding via Designer

### Step-by-Step Instructions

1. **Create a New Windows Forms Project** in Visual Studio to display the RangeSlider control
2. **Open the Designer View** of your form
3. **Locate RangeSlider in Toolbox** under the Syncfusion section
4. **Drag and Drop** the RangeSlider control onto your form

### Automatic Assembly Addition

When you add the RangeSlider from the toolbox, Visual Studio automatically adds these dependent assemblies:

- Syncfusion.Grid.Base
- Syncfusion.Grid.Windows
- Syncfusion.Shared.Base
- Syncfusion.Shared.Windows
- Syncfusion.Tools.Base
- Syncfusion.Tools.Windows

### Configuring via Properties Panel

After adding to the form, configure basic properties in the Properties panel:

- Set `ShowLabels` to `True` to display value labels
- Set `ShowTicks` to `True` to display tick marks
- Modify `Minimum` and `Maximum` for range bounds
- Adjust `ChannelHeight` for visual sizing

## Adding via Code

### C# Implementation

To add RangeSlider programmatically in C#:

```csharp
using Syncfusion.Windows.Forms.Tools;
using System.Windows.Forms;

public partial class Form1 : Form
{
    private RangeSlider rangeSlider1;

    public Form1()
    {
        InitializeComponent();
    }

    private void Form1_Load(object sender, EventArgs e)
    {
        // Create instance
        rangeSlider1 = new RangeSlider();
        
        // Add to form
        this.Controls.Add(rangeSlider1);
    }
}
```

### VB.NET Implementation

To add RangeSlider programmatically in VB.NET:

```vb
Imports Syncfusion.Windows.Forms.Tools
Imports System.Windows.Forms

Public Partial Class Form1
    Inherits Form

    Private rangeSlider1 As RangeSlider

    Public Sub New()
        InitializeComponent()
    End Sub

    Private Sub Form1_Load(sender As Object, e As EventArgs) Handles MyBase.Load
        ' Create instance
        rangeSlider1 = New RangeSlider()
        
        ' Add to form
        Me.Controls.Add(rangeSlider1)
    End Sub
End Class
```

### Namespace Import

Ensure you import the required namespace:

**C#:**
```csharp
using Syncfusion.Windows.Forms.Tools;
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms.Tools
```

## Basic Configuration

### Display Labels

Show value labels on the slider:

```csharp
rangeSlider1.ShowLabels = true;
```

```vb
rangeSlider1.ShowLabels = True
```

### Set Range Bounds

Define the minimum and maximum values for the range:

```csharp
rangeSlider1.Minimum = 0;
rangeSlider1.Maximum = 100;
```

```vb
rangeSlider1.Minimum = 0
rangeSlider1.Maximum = 100
```

### Initialize Thumb Positions

Set the initial positions of the left and right thumbs:

```csharp
rangeSlider1.SliderMin = 20;
rangeSlider1.SliderMax = 80;
```

```vb
rangeSlider1.SliderMin = 20
rangeSlider1.SliderMax = 80
```

### Complete Setup Example

Combine all basic configuration steps:

```csharp
public partial class Form1 : Form
{
    public Form1()
    {
        InitializeComponent();
    }

    private void Form1_Load(object sender, EventArgs e)
    {
        // Create and configure RangeSlider
        RangeSlider rangeSlider1 = new RangeSlider();
        
        // Set range bounds
        rangeSlider1.Minimum = 0;
        rangeSlider1.Maximum = 100;
        
        // Set initial positions
        rangeSlider1.SliderMin = 25;
        rangeSlider1.SliderMax = 75;
        
        // Display options
        rangeSlider1.ShowLabels = true;
        
        // Add to form
        this.Controls.Add(rangeSlider1);
    }
}
```

### Next Steps

After basic setup, refer to:
- **Value Configuration** for advanced value handling
- **Interactive Features** for customization options
- **Layout and Orientation** for different layouts

---

**Related:** Assembly references | NuGet packages | Designer workflow
