# Value Configuration and Events

This guide covers configuring value ranges, divisions, and handling value change events in the RadialSlider control.

## When to Read This

Read this reference when:
- Setting up minimum and maximum value ranges
- Configuring the current value programmatically
- Adding division markers around the dial
- Handling value changes in real-time
- Implementing custom text formatting with DrawText event
- Creating interactive value selectors with feedback

## MinimumValue Property

Sets the starting value of the slider's range.

**Type:** `int`  
**Default:** `0`

**C#:**
```csharp
// Temperature range: -20°C to 50°C
radialSlider1.MinimumValue = -20;
radialSlider1.MaximumValue = 50;
```

**VB.NET:**
```vbnet
' Temperature range: -20°C to 50°C
radialSlider1.MinimumValue = -20
radialSlider1.MaximumValue = 50
```

**Common Use Cases:**
- **Negative ranges:** Temperature controls, financial charts
- **Offset values:** Time zones (UTC-12 to UTC+14)
- **Custom scales:** Decibel levels, pH scales

## MaximumValue Property

Sets the ending value of the slider's range.

**Type:** `int`  
**Default:** `10`

**C#:**
```csharp
// Percentage slider: 0% to 100%
radialSlider1.MinimumValue = 0;
radialSlider1.MaximumValue = 100;

// RPM gauge: 0 to 8000
radialSlider1.MinimumValue = 0;
radialSlider1.MaximumValue = 8000;
```

**VB.NET:**
```vbnet
' Percentage slider: 0% to 100%
radialSlider1.MinimumValue = 0
radialSlider1.MaximumValue = 100

' RPM gauge: 0 to 8000
radialSlider1.MinimumValue = 0
radialSlider1.MaximumValue = 8000
```

## Value Property

Gets or sets the current value of the slider.

**Type:** `int`  
**Default:** `0`

**Setting Value:**

**C#:**
```csharp
// Set initial value to 50
radialSlider1.Value = 50;

// Set to middle of range
radialSlider1.Value = (radialSlider1.MinimumValue + radialSlider1.MaximumValue) / 2;
```

**VB.NET:**
```vbnet
' Set initial value to 50
radialSlider1.Value = 50

' Set to middle of range
radialSlider1.Value = (radialSlider1.MinimumValue + radialSlider1.MaximumValue) \ 2
```

**Getting Value:**

**C#:**
```csharp
private void SaveSettings()
{
    int brightness = radialSlider1.Value;
    int volume = radialSlider2.Value;
    
    // Save to settings
    Properties.Settings.Default.Brightness = brightness;
    Properties.Settings.Default.Volume = volume;
    Properties.Settings.Default.Save();
}
```

**VB.NET:**
```vbnet
Private Sub SaveSettings()
    Dim brightness As Integer = radialSlider1.Value
    Dim volume As Integer = radialSlider2.Value
    
    ' Save to settings
    My.Settings.Brightness = brightness
    My.Settings.Volume = volume
    My.Settings.Save()
End Sub
```

## SliderDivision Property

Configures the number of division markers around the dial.

**Type:** `int`  
**Default:** `10`

**C#:**
```csharp
// 12 divisions (like a clock face)
radialSlider1.SliderDivision = 12;

// 8 divisions (for cardinal/intercardinal directions)
radialSlider1.SliderDivision = 8;

// 24 divisions (24-hour clock)
radialSlider1.SliderDivision = 24;
```

**VB.NET:**
```vbnet
' 12 divisions (like a clock face)
radialSlider1.SliderDivision = 12

' 8 divisions (for cardinal/intercardinal directions)
radialSlider1.SliderDivision = 8

' 24 divisions (24-hour clock)
radialSlider1.SliderDivision = 24
```

**Best Practices:**
- Match divisions to your value range for intuitive reading
- Use factors of the range for even spacing
- 12 divisions work well for percentage scales (0-100)
- Fewer divisions (4-8) for simpler controls
- More divisions (20-24) for precision controls

**Example with Range Matching:**

**C#:**
```csharp
// Brightness: 0-100, 10 divisions = marks every 10%
radialSlider1.MinimumValue = 0;
radialSlider1.MaximumValue = 100;
radialSlider1.SliderDivision = 10;  // Every 10 units

// Speed: 0-120 mph, 12 divisions = marks every 10 mph
radialSlider2.MinimumValue = 0;
radialSlider2.MaximumValue = 120;
radialSlider2.SliderDivision = 12;  // Every 10 units
```

## ValueChanged Event

Fires when the slider's value changes, enabling real-time responses.

**Event Type:** `RadialSlider.ValueChangedEventHandler`  
**Event Args:** `RadialSlider.ValueChangedEventArgs`

### Basic Usage

**C#:**
```csharp
public partial class Form1 : Form
{
    public Form1()
    {
        InitializeComponent();
        
        // Subscribe to ValueChanged event
        radialSlider1.ValueChanged += RadialSlider1_ValueChanged;
    }
    
    private void RadialSlider1_ValueChanged(object sender, RadialSlider.ValueChangedEventArgs e)
    {
        // Update display when value changes
        lblValue.Text = $"Current Value: {radialSlider1.Value}";
    }
}
```

**VB.NET:**
```vbnet
Public Partial Class Form1
    Inherits Form
    
    Public Sub New()
        InitializeComponent()
        
        ' Subscribe to ValueChanged event
        AddHandler radialSlider1.ValueChanged, AddressOf RadialSlider1_ValueChanged
    End Sub
    
    Private Sub RadialSlider1_ValueChanged(sender As Object, e As RadialSlider.ValueChangedEventArgs)
        ' Update display when value changes
        lblValue.Text = $"Current Value: {radialSlider1.Value}"
    End Sub
End Class
```

### Complete Example: Audio Equalizer

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class EqualizerControl : Form
{
    private RadialSlider bassSlider;
    private RadialSlider midSlider;
    private RadialSlider trebleSlider;
    private Label lblBass, lblMid, lblTreble;
    
    public EqualizerControl()
    {
        InitializeComponent();
        SetupEqualizer();
    }
    
    private void SetupEqualizer()
    {
        // Bass control (-12 to +12 dB)
        bassSlider = new RadialSlider
        {
            Location = new Point(20, 50),
            Size = new Size(200, 200),
            MinimumValue = -12,
            MaximumValue = 12,
            Value = 0,
            SliderDivision = 12
        };
        bassSlider.ValueChanged += BassSlider_ValueChanged;
        
        lblBass = new Label
        {
            Location = new Point(20, 20),
            Size = new Size(200, 20),
            Text = "Bass: 0 dB",
            TextAlign = ContentAlignment.MiddleCenter
        };
        
        // Mid control
        midSlider = new RadialSlider
        {
            Location = new Point(240, 50),
            Size = new Size(200, 200),
            MinimumValue = -12,
            MaximumValue = 12,
            Value = 0,
            SliderDivision = 12
        };
        midSlider.ValueChanged += MidSlider_ValueChanged;
        
        lblMid = new Label
        {
            Location = new Point(240, 20),
            Size = new Size(200, 20),
            Text = "Mid: 0 dB",
            TextAlign = ContentAlignment.MiddleCenter
        };
        
        // Treble control
        trebleSlider = new RadialSlider
        {
            Location = new Point(460, 50),
            Size = new Size(200, 200),
            MinimumValue = -12,
            MaximumValue = 12,
            Value = 0,
            SliderDivision = 12
        };
        trebleSlider.ValueChanged += TrebleSlider_ValueChanged;
        
        lblTreble = new Label
        {
            Location = new Point(460, 20),
            Size = new Size(200, 20),
            Text = "Treble: 0 dB",
            TextAlign = ContentAlignment.MiddleCenter
        };
        
        // Add all controls
        this.Controls.AddRange(new Control[] 
        {
            bassSlider, lblBass,
            midSlider, lblMid,
            trebleSlider, lblTreble
        });
        
        this.Text = "Audio Equalizer";
        this.Size = new Size(700, 300);
    }
    
    private void BassSlider_ValueChanged(object sender, RadialSlider.ValueChangedEventArgs e)
    {
        int value = bassSlider.Value;
        lblBass.Text = $"Bass: {(value >= 0 ? "+" : "")}{value} dB";
        
        // Apply bass adjustment (pseudo-code)
        // AudioEngine.SetBass(value);
    }
    
    private void MidSlider_ValueChanged(object sender, RadialSlider.ValueChangedEventArgs e)
    {
        int value = midSlider.Value;
        lblMid.Text = $"Mid: {(value >= 0 ? "+" : "")}{value} dB";
        
        // Apply mid adjustment
        // AudioEngine.SetMid(value);
    }
    
    private void TrebleSlider_ValueChanged(object sender, RadialSlider.ValueChangedEventArgs e)
    {
        int value = trebleSlider.Value;
        lblTreble.Text = $"Treble: {(value >= 0 ? "+" : "")}{value} dB";
        
        // Apply treble adjustment
        // AudioEngine.SetTreble(value);
    }
}
```

## DrawText Event

Enables custom text formatting for division labels around the slider.

**Event Type:** `RadialSlider.DrawTextEventHandler`  
**Event Args:** `RadialSlider.DrawTextEventArgs`

### DrawTextEventArgs Properties

| Property | Type | Description |
|----------|------|-------------|
| `Text` | `string` | The text to display (can be modified) |
| `ForeColor` | `Color` | The color of the text (can be modified) |
| `Font` | `Font` | The font for the text (can be modified) |
| `TextType` | `TextType` | Type of text being drawn (Interval, Pointer, Value) |

### TextType Enum

- **`TextType.Interval`** - Division marker labels around the dial
- **`TextType.Pointer`** - The value at the pointer/needle position
- **`TextType.Value`** - The current value display

### Basic Custom Text Example

**C#:**
```csharp
public partial class Form1 : Form
{
    public Form1()
    {
        InitializeComponent();
        
        // Configure slider for percentage (0-100)
        radialSlider1.MinimumValue = 0;
        radialSlider1.MaximumValue = 100;
        radialSlider1.SliderDivision = 10;
        
        // Subscribe to DrawText event
        radialSlider1.DrawText += RadialSlider1_DrawText;
    }
    
    private void RadialSlider1_DrawText(object sender, RadialSlider.DrawTextEventArgs e)
    {
        // Add "%" symbol to all interval labels
        if (e.TextType == TextType.Interval)
        {
            e.Text = e.Text + "%";
        }
        
        // Color the pointer value based on percentage
        if (e.TextType == TextType.Pointer)
        {
            int value = radialSlider1.Value;
            
            if (value < 30)
                e.ForeColor = Color.Red;        // Low: red
            else if (value < 70)
                e.ForeColor = Color.Orange;     // Medium: orange
            else
                e.ForeColor = Color.Green;      // High: green
            
            e.Text = $"{value}%";
        }
    }
}
```

**VB.NET:**
```vbnet
Public Partial Class Form1
    Inherits Form
    
    Public Sub New()
        InitializeComponent()
        
        ' Configure slider for percentage (0-100)
        radialSlider1.MinimumValue = 0
        radialSlider1.MaximumValue = 100
        radialSlider1.SliderDivision = 10
        
        ' Subscribe to DrawText event
        AddHandler radialSlider1.DrawText, AddressOf RadialSlider1_DrawText
    End Sub
    
    Private Sub RadialSlider1_DrawText(sender As Object, e As RadialSlider.DrawTextEventArgs)
        ' Add "%" symbol to all interval labels
        If e.TextType = TextType.Interval Then
            e.Text = e.Text & "%"
        End If
        
        ' Color the pointer value based on percentage
        If e.TextType = TextType.Pointer Then
            Dim value As Integer = radialSlider1.Value
            
            If value < 30 Then
                e.ForeColor = Color.Red
            ElseIf value < 70 Then
                e.ForeColor = Color.Orange
            Else
                e.ForeColor = Color.Green
            End If
            
            e.Text = $"{value}%"
        End If
    End Sub
End Class
```

### Advanced Example: Timer with Minutes and Seconds

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class TimerControl : Form
{
    private RadialSlider timerSlider;
    private Label lblTime;
    
    public TimerControl()
    {
        InitializeComponent();
        SetupTimer();
    }
    
    private void SetupTimer()
    {
        // Timer range: 0 to 60 minutes
        timerSlider = new RadialSlider
        {
            Location = new Point(40, 40),
            Size = new Size(320, 320),
            MinimumValue = 0,
            MaximumValue = 60,
            Value = 15,
            SliderDivision = 12  // Every 5 minutes
        };
        
        // Subscribe to events
        timerSlider.DrawText += TimerSlider_DrawText;
        timerSlider.ValueChanged += TimerSlider_ValueChanged;
        
        lblTime = new Label
        {
            Location = new Point(40, 370),
            Size = new Size(320, 30),
            Text = "Timer: 15:00",
            TextAlign = ContentAlignment.MiddleCenter,
            Font = new Font("Arial", 14, FontStyle.Bold)
        };
        
        this.Controls.Add(timerSlider);
        this.Controls.Add(lblTime);
        
        this.Text = "Timer Control";
        this.Size = new Size(420, 450);
    }
    
    private void TimerSlider_DrawText(object sender, RadialSlider.DrawTextEventArgs e)
    {
        if (e.TextType == TextType.Interval)
        {
            // Format interval labels as "Xm" (minutes)
            e.Text = e.Text + "m";
            e.Font = new Font("Arial", 9);
        }
        
        if (e.TextType == TextType.Pointer)
        {
            // Format pointer as "MM:SS"
            int minutes = timerSlider.Value;
            e.Text = $"{minutes:00}:00";
            e.Font = new Font("Arial", 11, FontStyle.Bold);
            
            // Color based on time remaining
            if (minutes <= 5)
                e.ForeColor = Color.Red;       // Urgent
            else if (minutes <= 15)
                e.ForeColor = Color.Orange;    // Warning
            else
                e.ForeColor = Color.Green;     // Plenty of time
        }
    }
    
    private void TimerSlider_ValueChanged(object sender, RadialSlider.ValueChangedEventArgs e)
    {
        int minutes = timerSlider.Value;
        lblTime.Text = $"Timer: {minutes:00}:00";
    }
}
```

### Complete Example: Thermostat with Custom Formatting

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class ThermostatControl : Form
{
    private RadialSlider tempSlider;
    private Label lblStatus;
    
    public ThermostatControl()
    {
        InitializeComponent();
        SetupThermostat();
    }
    
    private void SetupThermostat()
    {
        tempSlider = new RadialSlider
        {
            Location = new Point(50, 50),
            Size = new Size(300, 300),
            MinimumValue = 50,   // 50°F
            MaximumValue = 90,   // 90°F
            Value = 72,          // Default comfortable temperature
            SliderDivision = 8   // Every 5 degrees
        };
        
        tempSlider.DrawText += TempSlider_DrawText;
        tempSlider.ValueChanged += TempSlider_ValueChanged;
        
        lblStatus = new Label
        {
            Location = new Point(50, 360),
            Size = new Size(300, 60),
            Text = "Target: 72°F\nComfortable",
            TextAlign = ContentAlignment.MiddleCenter,
            Font = new Font("Arial", 12)
        };
        
        this.Controls.Add(tempSlider);
        this.Controls.Add(lblStatus);
        
        this.Text = "Thermostat";
        this.Size = new Size(420, 470);
        this.BackColor = Color.FromArgb(240, 240, 240);
    }
    
    private void TempSlider_DrawText(object sender, RadialSlider.DrawTextEventArgs e)
    {
        if (e.TextType == TextType.Interval)
        {
            // Show temperature with degree symbol
            e.Text = e.Text + "°";
            e.Font = new Font("Arial", 9);
        }
        
        if (e.TextType == TextType.Pointer || e.TextType == TextType.Value)
        {
            int temp = tempSlider.Value;
            e.Text = $"{temp}°F";
            e.Font = new Font("Arial", 12, FontStyle.Bold);
            
            // Color code by temperature range
            if (temp < 60)
                e.ForeColor = Color.Blue;           // Cold
            else if (temp <= 75)
                e.ForeColor = Color.Green;          // Comfortable
            else if (temp <= 80)
                e.ForeColor = Color.Orange;         // Warm
            else
                e.ForeColor = Color.Red;            // Hot
        }
    }
    
    private void TempSlider_ValueChanged(object sender, RadialSlider.ValueChangedEventArgs e)
    {
        int temp = tempSlider.Value;
        string status;
        
        if (temp < 60)
            status = "Cold";
        else if (temp <= 68)
            status = "Cool";
        else if (temp <= 75)
            status = "Comfortable";
        else if (temp <= 80)
            status = "Warm";
        else
            status = "Hot";
        
        lblStatus.Text = $"Target: {temp}°F\n{status}";
    }
}
```

## Next Steps

After configuring values and events:

1. **Customize Appearance** → Read: [appearance-customization.md](appearance-customization.md)
   - Apply visual themes
   - Customize colors
   - Change needle types
   - Style for dark/light modes
