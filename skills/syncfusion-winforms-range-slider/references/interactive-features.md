# Interactive Features

## Table of Contents
- [Channel Customization](#channel-customization)
- [Range Color Highlighting](#range-color-highlighting)
- [Thumb Customization](#thumb-customization)
- [Slider Sizing](#slider-sizing)
- [Tick Display and Frequency](#tick-display-and-frequency)
- [Event Handling](#event-handling)

## Channel Customization

### Setting Channel Color

Customize the background track color using the `ChannelColor` property:

```csharp
rangeSlider.ChannelColor = Color.DarkGray;
```

```vb
rangeSlider.ChannelColor = Color.DarkGray
```

**Purpose:** The channel is the background track over which the slider thumbs move. Setting channel color provides visual consistency with your application theme.

**Common Colors:**
- `Color.LightGray` - Light, neutral appearance
- `Color.DarkGray` - High contrast option
- `Color.Silver` - Subtle, professional look
- Custom colors for branding

### Setting Channel Height

Adjust the thickness of the channel track using the `ChannelHeight` property:

```csharp
// Thin channel (4 pixels)
rangeSlider.ChannelHeight = 4;

// Thick channel (6 pixels)
rangeSlider.ChannelHeight = 6;
```

```vb
' Thin channel (4 pixels)
rangeSlider.ChannelHeight = 4

' Thick channel (6 pixels)
rangeSlider.ChannelHeight = 6
```

**Property Details:**
- Measured in pixels
- Typical range: 2-10 pixels
- Default: Usually 4 pixels
- Larger values create more prominent tracks
- Smaller values create subtle tracks

### Channel Example

```csharp
private void CustomizeChannel()
{
    rangeSlider.ChannelColor = Color.LightGray;
    rangeSlider.ChannelHeight = 5;
}
```

## Range Color Highlighting

### Setting Range Color

Highlight the selected range between the two thumbs using the `RangeColor` property:

```csharp
rangeSlider.RangeColor = Color.DarkGreen;
```

```vb
rangeSlider.RangeColor = Color.DarkGreen
```

**Purpose:** The range color visually indicates the selected values between the two thumbs. Users immediately see which portion is selected.

**Selection Highlighting:**
- The selected area between SliderMin and SliderMax is highlighted
- Contrasts with the channel color to show selection clearly
- Updates dynamically as thumbs move

### Color Strategy

Create visual hierarchy with complementary colors:

```csharp
// Subtle selection
rangeSlider.ChannelColor = Color.LightGray;
rangeSlider.RangeColor = Color.SkyBlue;

// Bold selection
rangeSlider.ChannelColor = Color.WhiteSmoke;
rangeSlider.RangeColor = Color.DarkGreen;

// Professional appearance
rangeSlider.ChannelColor = Color.Gainsboro;
rangeSlider.RangeColor = Color.DodgerBlue;
```

### Range Color Example

```csharp
private void ApplyColorScheme()
{
    rangeSlider.ChannelColor = Color.LightGray;    // Background
    rangeSlider.RangeColor = Color.DarkGreen;      // Selection
    rangeSlider.ThumbColor = Color.Green;          // Thumbs
}
```

## Thumb Customization

### Setting Thumb Color

Customize the color of both thumb controls using the `ThumbColor` property:

```csharp
rangeSlider.ThumbColor = Color.Teal;
```

```vb
rangeSlider.ThumbColor = Color.Teal
```

**Purpose:** Thumbs are the interactive handles users drag to adjust the range. Distinct thumb color makes them easily identifiable as interactive elements.

**Thumb Color Options:**
- Match the range color for cohesive design
- Use contrasting color for visual emphasis
- Use theme color for brand consistency

### Thumb Color Strategies

**Strategy 1: Monochromatic**
Use varying shades of one color family:

```csharp
rangeSlider.ThumbColor = Color.DarkGreen;
rangeSlider.RangeColor = Color.Green;
rangeSlider.ChannelColor = Color.LightGreen;
```

**Strategy 2: Complementary Contrast**
Use contrasting colors for clear visual distinction:

```csharp
rangeSlider.ChannelColor = Color.LightGray;
rangeSlider.RangeColor = Color.SkyBlue;
rangeSlider.ThumbColor = Color.DarkBlue;
```

**Strategy 3: Material Design**
Follow modern design principles:

```csharp
rangeSlider.ThumbColor = Color.FromArgb(63, 81, 181);  // Deep Blue
rangeSlider.RangeColor = Color.FromArgb(103, 58, 183); // Deep Purple
rangeSlider.ChannelColor = Color.LightGray;
```

### Thumb Example

```csharp
private void SetupThumbAppearance()
{
    rangeSlider.ThumbColor = Color.Teal;
}
```

## Slider Sizing

### Setting Slider Size

Control the dimensions of the thumb controls using the `SliderSize` property:

```csharp
// Small thumbs
rangeSlider.SliderSize = new Size(11, 14);

// Large thumbs
rangeSlider.SliderSize = new Size(11, 18);
```

```vb
' Small thumbs
rangeSlider.SliderSize = New Size(11, 14)

' Large thumbs
rangeSlider.SliderSize = New Size(11, 18)
```

**Size Parameters:**
- Width: Horizontal extent of thumb (usually 10-14 pixels)
- Height: Vertical extent of thumb (usually 14-24 pixels)
- Larger sizes are easier to grab with mouse
- Smaller sizes fit in compact layouts

### Size Recommendations

| Use Case | Size | Reason |
|----------|------|--------|
| Touch interfaces | (14, 24) | Larger hit target |
| Precision selection | (10, 14) | Better control |
| Compact layouts | (8, 12) | Space-saving |
| Accessibility | (14, 20) | Easier interaction |

### Sizing Example

```csharp
private void SetupSliderDimensions()
{
    // Create larger thumbs for easier selection
    rangeSlider.SliderSize = new Size(14, 20);
}
```

## Tick Display and Frequency

### Displaying Ticks

Show tick marks on the slider using the `ShowTicks` property:

```csharp
// Show ticks
rangeSlider.ShowTicks = true;

// Hide ticks
rangeSlider.ShowTicks = false;
```

```vb
' Show ticks
rangeSlider.ShowTicks = True

' Hide ticks
rangeSlider.ShowTicks = False
```

**Purpose:** Ticks are visual indicators placed at regular intervals. They help users understand the scale and select values more accurately.

### Setting Tick Frequency

Control the interval between ticks using the `TickFrequency` property:

```csharp
// Large interval (10 units between ticks)
rangeSlider.TickFrequency = 10;

// Small interval (5 units between ticks)
rangeSlider.TickFrequency = 5;
```

```vb
' Large interval (10 units between ticks)
rangeSlider.TickFrequency = 10

' Small interval (5 units between ticks)
rangeSlider.TickFrequency = 5
```

**Frequency Guidelines:**
- Range 0-100: TickFrequency = 10 or 20
- Range 0-1000: TickFrequency = 100 or 200
- Range 0-360 (angles): TickFrequency = 30 or 45

### Tick Configuration Examples

**Example 1: Percentage Scale**
```csharp
rangeSlider.Minimum = 0;
rangeSlider.Maximum = 100;
rangeSlider.ShowTicks = true;
rangeSlider.TickFrequency = 10;  // Every 10%
```

**Example 2: Price Range**
```csharp
rangeSlider.Minimum = 0;
rangeSlider.Maximum = 1000;
rangeSlider.ShowTicks = true;
rangeSlider.TickFrequency = 100; // Every $100
```

**Example 3: Dense Ticks**
```csharp
rangeSlider.Minimum = 0;
rangeSlider.Maximum = 100;
rangeSlider.ShowTicks = true;
rangeSlider.TickFrequency = 5;   // Every 5 units
```

### Tick and Label Example

```csharp
private void SetupTickMarks()
{
    rangeSlider.ShowTicks = true;
    rangeSlider.TickFrequency = 20;
    rangeSlider.ShowLabels = true;  // Show values too
}
```

## Event Handling

### Scroll Event

The `Scroll` event occurs continuously while the user drags a thumb:

```csharp
rangeSlider.Scroll += new EventHandler(OnRangeSliderScroll);

private void OnRangeSliderScroll(object sender, EventArgs e)
{
    // Called while user is dragging
    // Use for live preview
    UpdatePreview();
}
```

```vb
AddHandler rangeSlider.Scroll, AddressOf OnRangeSliderScroll

Private Sub OnRangeSliderScroll(sender As Object, e As EventArgs)
    ' Called while user is dragging
    ' Use for live preview
    UpdatePreview()
End Sub
```

**Use Cases for Scroll:**
- Real-time filter preview
- Live chart updates
- Immediate feedback display
- Animation during selection

### ValueChanged Event

The `ValueChanged` event fires after the user finishes moving a thumb:

```csharp
rangeSlider.ValueChanged += new EventHandler(OnRangeSliderValueChanged);

private void OnRangeSliderValueChanged(object sender, EventArgs e)
{
    // Called after value change completes
    // Use for final data updates
    ApplyNewValues();
}
```

```vb
AddHandler rangeSlider.ValueChanged, AddressOf OnRangeSliderValueChanged

Private Sub OnRangeSliderValueChanged(sender As Object, e As EventArgs)
    ' Called after value change completes
    ' Use for final data updates
    ApplyNewValues()
End Sub
```

**Use Cases for ValueChanged:**
- Database queries
- Expensive computations
- Final data binding
- Event logging

### Complete Event Example

```csharp
private void SetupEventHandlers()
{
    // Live preview while dragging
    rangeSlider.Scroll += (s, e) =>
    {
        int minVal = rangeSlider.SliderMin;
        int maxVal = rangeSlider.SliderMax;
        lblPreview.Text = $"Range: {minVal} - {maxVal}";
    };

    // Final update after drag completes
    rangeSlider.ValueChanged += (s, e) =>
    {
        int minVal = rangeSlider.SliderMin;
        int maxVal = rangeSlider.SliderMax;
        UpdateDataGrid(minVal, maxVal);
        LogRangeSelection(minVal, maxVal);
    };
}
```

## Combined Customization Example

Create a fully customized range slider:

```csharp
private void SetupCustomRangeSlider()
{
    // Set range bounds
    rangeSlider.Minimum = 0;
    rangeSlider.Maximum = 100;
    rangeSlider.SliderMin = 20;
    rangeSlider.SliderMax = 80;
    
    // Channel appearance
    rangeSlider.ChannelColor = Color.LightGray;
    rangeSlider.ChannelHeight = 6;
    
    // Range highlighting
    rangeSlider.RangeColor = Color.DarkGreen;
    
    // Thumb appearance
    rangeSlider.ThumbColor = Color.Green;
    rangeSlider.SliderSize = new Size(12, 18);
    
    // Tick configuration
    rangeSlider.ShowTicks = true;
    rangeSlider.TickFrequency = 10;
    rangeSlider.ShowLabels = true;
    
    // Event handlers
    rangeSlider.Scroll += (s, e) => UpdatePreview();
    rangeSlider.ValueChanged += (s, e) => ApplyFilter();
}
```

---

**Related:** Value Configuration | Layout and Orientation | Event Handling Best Practices
