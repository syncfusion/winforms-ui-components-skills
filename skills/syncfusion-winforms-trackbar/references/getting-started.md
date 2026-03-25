# Getting Started with TrackBarEx

## Assembly Dependencies

Add the following assemblies to your Windows Forms project:
- Syncfusion.Grid.Base
- Syncfusion.Grid.Windows
- Syncfusion.Shared.Base
- Syncfusion.Shared.Windows
- Syncfusion.Tools.Base
- Syncfusion.Tools.Windows

## Adding TrackBarEx via Designer

1. Create a new Windows Forms project in Visual Studio
2. Locate TrackBarEx in the Syncfusion Toolbox
3. Drag the control onto your form
4. Required assemblies are added automatically
5. Access properties through the Properties panel

## Adding TrackBarEx Programmatically

Include the required namespace and create an instance:

```csharp
using Syncfusion.Windows.Forms.Tools;

public class Form1 : Form
{
    public Form1()
    {
        // Create TrackBarEx instance
        TrackBarEx trackBarEx1 = new TrackBarEx();
        
        // Configure basic properties
        trackBarEx1.Minimum = 0;
        trackBarEx1.Maximum = 100;
        trackBarEx1.Value = 50;
        
        // Add to form
        this.Controls.Add(trackBarEx1);
    }
}
```

## Basic Configuration

### Setting Minimum and Maximum Values

Define the value range for the slider:

```csharp
trackBarEx1.Minimum = 10;
trackBarEx1.Maximum = 100;
trackBarEx1.Value = 50;  // Set initial value within range
```

The value must fall between minimum and maximum. If you set a value outside this range, it will be clamped to the nearest boundary.

### Default Configuration

TrackBarEx has sensible defaults:
- **Minimum:** 10
- **Maximum:** 20
- **Value:** 5
- **Orientation:** Horizontal
- **ShowButtons:** true (by default shows increment/decrement buttons)

### Initial Value

Always set the `Value` property after setting `Minimum` and `Maximum` to ensure proper initialization:

```csharp
trackBarEx1.Minimum = 0;
trackBarEx1.Maximum = 100;
trackBarEx1.Value = 30;  // Initial position
```

## Quick Example: Volume Control

Create a simple volume slider with visual buttons:

```csharp
private TrackBarEx volumeSlider;

public void SetupVolumeControl()
{
    volumeSlider = new TrackBarEx();
    volumeSlider.Minimum = 0;
    volumeSlider.Maximum = 100;
    volumeSlider.Value = 50;
    volumeSlider.ShowButtons = true;
    volumeSlider.ButtonColor = System.Drawing.Color.CornflowerBlue;
    
    // Handle volume changes
    volumeSlider.Scroll += (s, e) => {
        System.Diagnostics.Debug.WriteLine($"Volume: {volumeSlider.Value}%");
    };
    
    this.Controls.Add(volumeSlider);
}
```

## Next Steps

- Customize appearance: See appearance-customization.md
- Manage values and increments: See value-management.md
- Handle events: See orientation-events.md
