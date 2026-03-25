---
name: syncfusion-winforms-radial-slider
description: Implement and configure Syncfusion RadialSlider control in Windows Forms - a circular slider for visual numeric value selection with rotating needle. Use when creating circular value selection controls, rotary inputs, dial-based controls, or radial numeric input. Covers circular value selection, rotating needle control, dial-based input, visual value picker with divisions, and radial numeric input controls.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Radial Sliders (RadialSlider)

A circular slider control for Windows Forms that provides visual numeric value selection through a rotating needle around a radial dial with customizable divisions and themes.

## When to Use This Skill

Use this skill when you need to:
- **Circular Value Selection**: Implement dial-based numeric input (like volume knobs, temperature dials)
- **Rotating Controls**: Create rotary controls with visual feedback
- **Value Pickers**: Build circular value selectors with minimum/maximum ranges
- **Division Markers**: Display value divisions around a circular dial
- **Visual Indicators**: Show numeric values in a radial/circular format
- **Interactive Dials**: Implement knob-style controls (gauges, sliders, rotary switches)
- **Themed Controls**: Apply Office 2016 visual themes to circular sliders
- **Custom Value Ranges**: Set specific min/max ranges with visual divisions

## Component Overview

The **RadialSlider** is an advanced circular slider control that provides:
- Circular/radial value selection with rotating needle
- Customizable minimum and maximum value ranges
- Division markers around the dial
- Two slider styles (Default with hollow circles, Frame with background)
- Five Office 2016 visual themes plus default style
- Customizable colors (background, circles, needle, foreground)
- Two needle types (Straight Line, Dotted Line)
- ValueChanged event for real-time value tracking
- Custom text formatting for division labels

**Key Capabilities:**
- `MinimumValue` and `MaximumValue` properties for range configuration
- `Value` property for getting/setting current selection
- `SliderDivision` property for configuring division markers
- `SliderStyle` enum (Default, Frame)
- `Style` property with 6 visual themes
- `NeedleType` enum (StraightLine, DottedLine)
- Color customization (BackgroundColor, InnerCircleColor, OuterCircleColor, SliderNeedleColor)
- `ValueChanged` event for tracking value changes
- `DrawText` event for custom text formatting

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

**When to read:** Setting up RadialSlider for the first time, adding to forms, basic configuration

**Topics covered:**
- Assembly requirements and NuGet installation
- Designer-based setup (drag-and-drop from toolbox)
- Programmatic creation and configuration
- Basic size and property configuration
- Slider styles (Default vs Frame)
- ShowOuterCircle property
- Troubleshooting installation issues

### Value Configuration
📄 **Read:** [references/value-configuration.md](references/value-configuration.md)

**When to read:** Configuring value ranges, divisions, handling value changes, custom text formatting

**Topics covered:**
- MinimumValue and MaximumValue properties
- Value property (get/set current value)
- SliderDivision property for division markers
- ValueChanged event handling
- DrawText event for custom text formatting
- TextType enum (Interval, Pointer, Value)
- DrawTextEventArgs properties (Text, ForeColor, Font)
- Practical examples for value tracking

### Appearance Customization
📄 **Read:** [references/appearance-customization.md](references/appearance-customization.md)

**When to read:** Applying visual themes, customizing colors, changing needle appearance

**Topics covered:**
- BackgroundColor property
- InnerCircleColor and OuterCircleColor properties
- SliderNeedleColor property
- ForeColor property
- NeedleType enum (StraightLine, DottedLine)
- Visual styles (6 themes: Default, Office2016Colorful/White/DarkGray/Black)
- Complete theming examples
- Color combination best practices

## Quick Start Example

Create a basic RadialSlider for volume control:

**C#:**
```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class VolumeControl : Form
{
    private RadialSlider volumeSlider;
    private Label lblVolume;
    
    public VolumeControl()
    {
        InitializeComponent();
        SetupVolumeControl();
    }
    
    private void SetupVolumeControl()
    {
        // Create radial slider
        volumeSlider = new RadialSlider
        {
            Location = new System.Drawing.Point(50, 50),
            Size = new System.Drawing.Size(250, 250),
            MinimumValue = 0,
            MaximumValue = 100,
            Value = 50,
            SliderDivision = 10,
            SliderStyle = SliderStyles.Frame
        };
        
        // Create label for volume display
        lblVolume = new Label
        {
            Location = new System.Drawing.Point(50, 310),
            Size = new System.Drawing.Size(250, 30),
            Text = "Volume: 50",
            TextAlign = System.Drawing.ContentAlignment.MiddleCenter
        };
        
        // Handle value changes
        volumeSlider.ValueChanged += VolumeSlider_ValueChanged;
        
        // Add to form
        this.Controls.Add(volumeSlider);
        this.Controls.Add(lblVolume);
    }
    
    private void VolumeSlider_ValueChanged(object sender, RadialSlider.ValueChangedEventArgs e)
    {
        lblVolume.Text = $"Volume: {volumeSlider.Value}";
    }
}
```

**VB.NET:**
```vbnet
Imports System
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Public Partial Class VolumeControl
    Inherits Form
    
    Private volumeSlider As RadialSlider
    Private lblVolume As Label
    
    Public Sub New()
        InitializeComponent()
        SetupVolumeControl()
    End Sub
    
    Private Sub SetupVolumeControl()
        ' Create radial slider
        volumeSlider = New RadialSlider With {
            .Location = New System.Drawing.Point(50, 50),
            .Size = New System.Drawing.Size(250, 250),
            .MinimumValue = 0,
            .MaximumValue = 100,
            .Value = 50,
            .SliderDivision = 10,
            .SliderStyle = SliderStyles.Frame
        }
        
        ' Create label for volume display
        lblVolume = New Label With {
            .Location = New System.Drawing.Point(50, 310),
            .Size = New System.Drawing.Size(250, 30),
            .Text = "Volume: 50",
            .TextAlign = System.Drawing.ContentAlignment.MiddleCenter
        }
        
        ' Handle value changes
        AddHandler volumeSlider.ValueChanged, AddressOf VolumeSlider_ValueChanged
        
        ' Add to form
        Me.Controls.Add(volumeSlider)
        Me.Controls.Add(lblVolume)
    End Sub
    
    Private Sub VolumeSlider_ValueChanged(sender As Object, e As RadialSlider.ValueChangedEventArgs)
        lblVolume.Text = $"Volume: {volumeSlider.Value}"
    End Sub
End Class
```

## Common Patterns

### Pattern 1: Temperature Control with Custom Text

**C#:**
```csharp
private RadialSlider tempSlider;

private void SetupTemperatureControl()
{
    tempSlider = new RadialSlider
    {
        MinimumValue = 0,
        MaximumValue = 100,
        Value = 25,
        SliderDivision = 10,
        Style = RadialSliderStyle.Office2016Colorful
    };
    
    // Custom text formatting with degree symbol
    tempSlider.DrawText += TempSlider_DrawText;
    
    this.Controls.Add(tempSlider);
}

private void TempSlider_DrawText(object sender, RadialSlider.DrawTextEventArgs e)
{
    // Add degree Celsius symbol
    e.Text += "°C";
    
    // Color code by temperature range
    int temp = int.Parse(e.Text.Replace("°C", ""));
    
    if (temp <= 33)
    {
        e.ForeColor = System.Drawing.Brushes.Blue; // Cold
    }
    else if (temp > 33 && temp <= 66)
    {
        e.ForeColor = System.Drawing.Brushes.Orange; // Warm
    }
    else
    {
        e.ForeColor = System.Drawing.Brushes.Red; // Hot
    }
}
```

### Pattern 2: Custom Styled Dial Control

**C#:**
```csharp
private void CreateCustomStyledDial()
{
    RadialSlider dial = new RadialSlider
    {
        Size = new System.Drawing.Size(280, 280),
        MinimumValue = 0,
        MaximumValue = 200,
        SliderDivision = 20,
        
        // Custom colors
        BackgroundColor = System.Drawing.Color.FromArgb(30, 30, 30), // Dark background
        InnerCircleColor = System.Drawing.Color.FromArgb(60, 60, 60),
        OuterCircleColor = System.Drawing.Color.FromArgb(80, 80, 80),
        SliderNeedleColor = System.Drawing.Color.LimeGreen,
        ForeColor = System.Drawing.Color.White,
        
        // Dotted needle for modern look
        NeedleType = SliderNeedleType.DottedLine,
        SliderStyle = SliderStyles.Frame
    };
    
    this.Controls.Add(dial);
}
```

### Pattern 3: Font Size Selector

**C#:**
```csharp
private RadialSlider fontSizeSlider;
private RichTextBox textPreview;

private void SetupFontSizeSelector()
{
    // Create slider for font size selection
    fontSizeSlider = new RadialSlider
    {
        MinimumValue = 8,
        MaximumValue = 72,
        Value = 12,
        SliderDivision = 8
    };
    
    // Create preview textbox
    textPreview = new RichTextBox
    {
        Text = "Sample Text",
        ReadOnly = true
    };
    
    // Update font size on value change
    fontSizeSlider.ValueChanged += (s, e) =>
    {
        textPreview.SelectionFont = new System.Drawing.Font(
            textPreview.Font.Name,
            (float)fontSizeSlider.Value
        );
    };
    
    this.Controls.Add(fontSizeSlider);
    this.Controls.Add(textPreview);
}
```

## Key Properties

| Property | Type | Default | Purpose |
|----------|------|---------|---------|
| `MinimumValue` | `int` | `0` | Starting value of slider range |
| `MaximumValue` | `int` | `10` | Ending value of slider range |
| `Value` | `int` | `0` | Current selected value |
| `SliderDivision` | `int` | `10` | Number of division markers |
| `SliderStyle` | `SliderStyles` | `Default` | Visual style (Default, Frame) |
| `Style` | `RadialSliderStyle` | `Default` | Theme (Default, Office2016*) |
| `BackgroundColor` | `Color` | System default | Background color |
| `InnerCircleColor` | `Color` | System default | Inner circle color |
| `OuterCircleColor` | `Color` | System default | Outer circle color |
| `SliderNeedleColor` | `Color` | System default | Needle color |
| `ForeColor` | `Color` | System default | Text color |
| `NeedleType` | `SliderNeedleType` | `StraightLine` | Needle appearance |
| `ShowOuterCircle` | `bool` | `true` | Show/hide outer circle |

## Common Use Cases

### Volume/Audio Controls
Use RadialSlider for volume knobs, audio level controls, or media player controls where circular dials provide intuitive control.

### Temperature Settings
Implement temperature selectors for thermostats, climate controls, or weather applications with color-coded ranges.

### Speed Controls
Create speed selectors for motors, fans, or animations with visual feedback through division markers.

### Brightness/Opacity Adjustments
Build brightness controls for displays, image editors, or lighting controls with percentage-based ranges.

### Timer/Duration Selection
Implement countdown timers or duration selectors with minute/second divisions around a circular dial.

### Gauge Displays
Create interactive gauges that users can adjust (fuel gauges, pressure gauges, progress indicators).

### Settings Panels
Use for numeric settings that benefit from visual representation (zoom levels, quality settings, intensity controls).

### Data Visualization
Implement interactive data selectors where circular presentation is preferred over linear sliders.

## Additional Resources

- [Syncfusion RadialSlider API Reference](https://help.syncfusion.com/cr/windowsforms/Syncfusion.Windows.Forms.Tools.RadialSlider.html)
- [Control Dependencies](https://help.syncfusion.com/windowsforms/control-dependencies#radialslider)
- Assembly: `Syncfusion.Tools.Windows`
- Namespace: `Syncfusion.Windows.Forms.Tools`
