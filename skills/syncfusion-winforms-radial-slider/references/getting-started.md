# Getting Started with RadialSlider

This guide covers the installation, setup, and basic usage of the `RadialSlider` control in Windows Forms applications.

## When to Read This

Read this reference when:
- Setting up RadialSlider for the first time
- Adding the control to a form using the designer or code
- Understanding the basic structure and requirements
- Learning the required namespaces and assemblies
- Configuring slider styles (Default vs Frame)
- Creating your first circular slider implementation

## Assembly Requirements

The RadialSlider control requires the following assemblies:

**Required Assemblies:**
- `Syncfusion.Tools.Windows.dll` - Contains the RadialSlider control
- `Syncfusion.Tools.Base.dll` - Base functionality for Tools controls
- `Syncfusion.Shared.Windows.dll` - Shared Windows Forms components
- `Syncfusion.Shared.Base.dll` - Base shared functionality
- `Syncfusion.Grid.Windows.dll` - Grid support components
- `Syncfusion.Grid.Base.dll` - Grid base functionality

**Namespace:**
```csharp
using Syncfusion.Windows.Forms.Tools;
```

```vbnet
Imports Syncfusion.Windows.Forms.Tools
```

## Installation Methods

### NuGet Installation

Install the RadialSlider control via NuGet Package Manager:

**Package Manager Console:**
```powershell
Install-Package Syncfusion.Tools.Windows
```

**NuGet Package Manager UI:**
1. Right-click your project → Manage NuGet Packages
2. Search for "Syncfusion.Tools.Windows"
3. Select the package and click Install
4. Accept license agreements

### Manual Assembly Reference

If not using NuGet, add assembly references manually:

1. Right-click project → Add Reference → Browse
2. Navigate to Syncfusion installation folder:
   - `C:/Program Files (x86)/Syncfusion/Essential Studio/Windows/{version}/precompiledassemblies/{.NET version}/`
3. Select all 6 required assemblies listed above
4. Click OK

## Designer-Based Setup

The RadialSlider provides full Windows Forms designer support.

### Adding via Toolbox

**Steps:**
1. Open your form in designer view
2. Locate the Syncfusion toolbox section
3. Find "RadialSlider" control
4. Drag and drop onto your form
5. Configure properties via Property Grid

**Visual Result:**
The control appears as a circular dial with default settings (0-10 range, 10 divisions).

![RadialSlider in Toolbox](../images/radialslider-toolbox.png)

### Designer Properties

Common properties to set in the designer:

| Property | Purpose | Default |
|----------|---------|---------|
| `Size` | Control dimensions (width, height) | `282 x 282` |
| `MinimumValue` | Starting value of range | `0` |
| `MaximumValue` | Ending value of range | `10` |
| `Value` | Current selected value | `0` |
| `SliderDivision` | Number of division markers | `10` |
| `SliderStyle` | Visual style (Default, Frame) | `Default` |
| `ShowOuterCircle` | Show outer circle | `true` |

## Programmatic Creation

### Basic Implementation

Create and add a RadialSlider in code:

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class Form1 : Form
{
    private RadialSlider radialSlider1;
    
    public Form1()
    {
        InitializeComponent();
        CreateRadialSlider();
    }
    
    private void CreateRadialSlider()
    {
        // Create instance
        this.radialSlider1 = new RadialSlider();
        
        // Set size (typically square for proper circle rendering)
        this.radialSlider1.Size = new Size(282, 282);
        
        // Set location
        this.radialSlider1.Location = new Point(50, 50);
        
        // Add to form
        this.Controls.Add(this.radialSlider1);
    }
}
```

**VB.NET:**
```vbnet
Imports System
Imports System.Drawing
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Public Partial Class Form1
    Inherits Form
    
    Private radialSlider1 As RadialSlider
    
    Public Sub New()
        InitializeComponent()
        CreateRadialSlider()
    End Sub
    
    Private Sub CreateRadialSlider()
        ' Create instance
        Me.radialSlider1 = New RadialSlider()
        
        ' Set size (typically square for proper circle rendering)
        Me.radialSlider1.Size = New Size(282, 282)
        
        ' Set location
        Me.radialSlider1.Location = New Point(50, 50)
        
        ' Add to form
        Me.Controls.Add(Me.radialSlider1)
    End Sub
End Class
```

### Complete Example with Basic Configuration

Here's a complete example that creates a control with custom range:

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class SpeedControl : Form
{
    private RadialSlider speedSlider;
    private Label lblSpeed;
    
    public SpeedControl()
    {
        InitializeComponent();
        SetupSpeedControl();
    }
    
    private void SetupSpeedControl()
    {
        // Create radial slider for speed control
        speedSlider = new RadialSlider
        {
            Location = new Point(30, 30),
            Size = new Size(300, 300),
            MinimumValue = 0,
            MaximumValue = 120,  // Max speed 120 mph
            Value = 0,           // Start at 0
            SliderDivision = 12  // 12 divisions (every 10 mph)
        };
        
        // Create label to display speed
        lblSpeed = new Label
        {
            Location = new Point(30, 340),
            Size = new Size(300, 30),
            Text = "Speed: 0 mph",
            TextAlign = ContentAlignment.MiddleCenter,
            Font = new Font("Arial", 12, FontStyle.Bold)
        };
        
        // Handle value changes
        speedSlider.ValueChanged += SpeedSlider_ValueChanged;
        
        // Add to form
        this.Controls.Add(speedSlider);
        this.Controls.Add(lblSpeed);
        
        this.Text = "Speed Control";
        this.Size = new Size(380, 420);
    }
    
    private void SpeedSlider_ValueChanged(object sender, RadialSlider.ValueChangedEventArgs e)
    {
        lblSpeed.Text = $"Speed: {speedSlider.Value} mph";
    }
}
```

**VB.NET:**
```vbnet
Imports System
Imports System.Drawing
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Public Partial Class SpeedControl
    Inherits Form
    
    Private speedSlider As RadialSlider
    Private lblSpeed As Label
    
    Public Sub New()
        InitializeComponent()
        SetupSpeedControl()
    End Sub
    
    Private Sub SetupSpeedControl()
        ' Create radial slider for speed control
        speedSlider = New RadialSlider With {
            .Location = New Point(30, 30),
            .Size = New Size(300, 300),
            .MinimumValue = 0,
            .MaximumValue = 120,
            .Value = 0,
            .SliderDivision = 12
        }
        
        ' Create label to display speed
        lblSpeed = New Label With {
            .Location = New Point(30, 340),
            .Size = New Size(300, 30),
            .Text = "Speed: 0 mph",
            .TextAlign = ContentAlignment.MiddleCenter,
            .Font = New Font("Arial", 12, FontStyle.Bold)
        }
        
        ' Handle value changes
        AddHandler speedSlider.ValueChanged, AddressOf SpeedSlider_ValueChanged
        
        ' Add to form
        Me.Controls.Add(speedSlider)
        Me.Controls.Add(lblSpeed)
        
        Me.Text = "Speed Control"
        Me.Size = New Size(380, 420)
    End Sub
    
    Private Sub SpeedSlider_ValueChanged(sender As Object, e As RadialSlider.ValueChangedEventArgs)
        lblSpeed.Text = $"Speed: {speedSlider.Value} mph"
    End Sub
End Class
```

## Slider Styles

The RadialSlider supports two visual styles through the `SliderStyle` property.

### Default Style

Renders the slider with two hollow circles and a small center circle with division ticks.

**C#:**
```csharp
radialSlider1.SliderStyle = SliderStyles.Default;
```

**VB.NET:**
```vbnet
radialSlider1.SliderStyle = SliderStyles.Default
```

**Visual Characteristics:**
- Two concentric hollow circles
- Small filled center circle
- Division tick marks between circles
- Rotating needle from center
- Clean, minimal appearance

### Frame Style

Paints the background with an HQ frame for a more filled appearance.

**C#:**
```csharp
radialSlider1.SliderStyle = SliderStyles.Frame;
```

**VB.NET:**
```vbnet
radialSlider1.SliderStyle = SliderStyles.Frame
```

**Visual Characteristics:**
- Filled background frame
- More substantial appearance
- Better for dark themes
- Enhanced visual presence

### Comparison Example

**C#:**
```csharp
private void CreateStyleComparison()
{
    // Default style slider
    RadialSlider defaultSlider = new RadialSlider
    {
        Location = new Point(20, 20),
        Size = new Size(250, 250),
        SliderStyle = SliderStyles.Default,
        MinimumValue = 0,
        MaximumValue = 100
    };
    
    // Frame style slider
    RadialSlider frameSlider = new RadialSlider
    {
        Location = new Point(290, 20),
        Size = new Size(250, 250),
        SliderStyle = SliderStyles.Frame,
        MinimumValue = 0,
        MaximumValue = 100
    };
    
    this.Controls.Add(defaultSlider);
    this.Controls.Add(frameSlider);
}
```

## ShowOuterCircle Property

Control the visibility of the outer circle:

**C#:**
```csharp
// Show outer circle (default)
radialSlider1.ShowOuterCircle = true;

// Hide outer circle for minimal look
radialSlider1.ShowOuterCircle = false;
```

**VB.NET:**
```vbnet
' Show outer circle (default)
radialSlider1.ShowOuterCircle = True

' Hide outer circle for minimal look
radialSlider1.ShowOuterCircle = False
```

**When to hide outer circle:**
- Minimalist design requirements
- Space-constrained layouts
- When only the inner dial matters
- Creating custom visual compositions

## Next Steps

After setting up the basic control:

1. **Configure Value Ranges** → Read: [value-configuration.md](value-configuration.md)
   - Set MinimumValue and MaximumValue
   - Configure SliderDivision
   - Handle ValueChanged events
   - Implement custom text formatting

2. **Customize Appearance** → Read: [appearance-customization.md](appearance-customization.md)
   - Apply visual themes
   - Customize colors (background, circles, needle)
   - Change needle type
   - Apply Office 2016 styles

## Troubleshooting

### Control Not Visible in Toolbox

**Issue:** RadialSlider doesn't appear in Visual Studio toolbox.

**Solutions:**
1. Verify Syncfusion.Tools.Windows is installed via NuGet
2. Check if assemblies are compatible with your .NET version
3. Right-click toolbox → Choose Items → Browse to assembly location
4. Restart Visual Studio after installation
5. Ensure the correct .NET Framework version is targeted

### Designer Shows Generic Icon

**Issue:** Control appears as generic box in designer.

**Solution:** This is normal before runtime. The control will render properly when the application runs.

### Namespace Not Found

**Issue:** `The type or namespace name 'Tools' does not exist in the namespace 'Syncfusion.Windows.Forms'`

**Solution:**
1. Add reference to `Syncfusion.Tools.Windows.dll`
2. Verify `using Syncfusion.Windows.Forms.Tools;` is present
3. Check that assembly version matches your Syncfusion license
4. Ensure all 6 required assemblies are referenced

### Circular Shape Distorted

**Issue:** Slider appears oval or distorted instead of circular.

**Solution:**
1. Ensure Width and Height are equal (square dimensions)
   ```csharp
   radialSlider1.Size = new Size(300, 300); // Square
   ```
2. Avoid anchoring/docking that changes aspect ratio
3. Use minimum size of 150x150 for proper rendering

### Missing Division Markers

**Issue:** No division tick marks visible on the slider.

**Solution:**
1. Verify SliderDivision is set to a value > 0
   ```csharp
   radialSlider1.SliderDivision = 10;
   ```
2. Check that range (MaximumValue - MinimumValue) is divisible by SliderDivision
3. Ensure ForeColor contrasts with background

### Build Errors About Missing Assemblies

**Issue:** Build fails with "Could not load file or assembly" errors.

**Solution:**
1. Verify all 6 required assemblies are referenced
2. Check that assembly versions match across all Syncfusion references
3. Ensure assemblies are set to "Copy Local = True"
4. Clean and rebuild solution
5. Use NuGet for automatic dependency management
