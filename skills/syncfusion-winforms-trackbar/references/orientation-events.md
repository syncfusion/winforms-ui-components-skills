# Orientation and Events

## Table of Contents
- [Orientation](#orientation)
- [Scroll Event](#scroll-event)
- [Event Handling Patterns](#event-handling-patterns)
- [Common Scenarios](#common-scenarios)
- [Best Practices](#best-practices)

## Orientation

### Horizontal Orientation

The default orientation displays the slider horizontally:

```csharp
trackBarEx1.Orientation = Orientation.Horizontal;
```

**Characteristics:**
- Slider moves left to right
- Buttons positioned above/below or on sides
- Best for wide layouts
- Intuitive for most users
- Easier to interact with mouse

### Vertical Orientation

Display the slider vertically for constrained horizontal space:

```csharp
trackBarEx1.Orientation = Orientation.Vertical;
```

**Characteristics:**
- Slider moves top to bottom
- Buttons positioned above/below
- Good for tall narrow layouts
- Less common but useful for specific UIs
- May require layout adjustment

### Choosing Orientation

```csharp
public void SetOrientation(bool isVertical)
{
    if (isVertical)
    {
        trackBarEx1.Orientation = Orientation.Vertical;
        trackBarEx1.Height = 300;  // Tall for vertical space
        trackBarEx1.Width = 50;    // Narrow width
    }
    else
    {
        trackBarEx1.Orientation = Orientation.Horizontal;
        trackBarEx1.Height = 50;   // Short height
        trackBarEx1.Width = 300;   // Wide for horizontal space
    }
}
```

### Responsive Orientation

Adapt orientation based on form dimensions:

```csharp
private void Form_Load(object sender, EventArgs e)
{
    // Auto-select orientation based on form shape
    if (this.Width > this.Height)
    {
        trackBarEx1.Orientation = Orientation.Horizontal;
    }
    else
    {
        trackBarEx1.Orientation = Orientation.Vertical;
    }
}

private void Form_Resize(object sender, EventArgs e)
{
    // Adjust if window resized significantly
    if (this.Width > this.Height * 1.5)
    {
        trackBarEx1.Orientation = Orientation.Horizontal;
    }
    else if (this.Height > this.Width * 1.5)
    {
        trackBarEx1.Orientation = Orientation.Vertical;
    }
}
```

## Scroll Event

### Scroll Event Handler

The `Scroll` event fires when the user moves the slider:

```csharp
trackBarEx1.Scroll += TrackBarEx1_Scroll;

private void TrackBarEx1_Scroll(object sender, EventArgs e)
{
    // Handle value change
    MessageBox.Show($"Current Value: {trackBarEx1.Value}");
}
```

### Event Handler with Lambda Expression

Use lambda for inline event handling:

```csharp
trackBarEx1.Scroll += (s, e) => {
    int newValue = trackBarEx1.Value;
    UpdateDisplay(newValue);
};
```

### Triggering Events

The `Scroll` event fires from:
- User dragging the slider
- User clicking on the track
- User clicking increment/decrement buttons
- Programmatic value changes (if you set the value directly)

## Event Handling Patterns

### Reading Current Value in Event

```csharp
private void trackBarEx1_Scroll(object sender, EventArgs e)
{
    TrackBarEx trackBar = sender as TrackBarEx;
    if (trackBar != null)
    {
        int currentValue = trackBar.Value;
        System.Diagnostics.Debug.WriteLine($"Value changed to: {currentValue}");
    }
}
```

### Cascading Updates

Update related controls when value changes:

```csharp
private void trackBarEx1_Scroll(object sender, EventArgs e)
{
    // Update label
    labelValue.Text = $"Value: {trackBarEx1.Value}";
    
    // Update percentage display
    int percentage = (trackBarEx1.Value - trackBarEx1.Minimum) * 100 
                     / (trackBarEx1.Maximum - trackBarEx1.Minimum);
    labelPercent.Text = $"{percentage}%";
    
    // Apply setting to application
    ApplyValueToApplication(trackBarEx1.Value);
}
```

### Debouncing Rapid Events

Handle rapid changes efficiently:

```csharp
private int debounceValue = 0;
private Timer debounceTimer;

private void trackBarEx1_Scroll(object sender, EventArgs e)
{
    debounceValue = trackBarEx1.Value;
    
    if (debounceTimer == null)
    {
        debounceTimer = new Timer();
        debounceTimer.Interval = 300;  // Wait 300ms after changes stop
        debounceTimer.Tick += (s, e2) => {
            debounceTimer.Stop();
            debounceTimer.Dispose();
            debounceTimer = null;
            
            // Process final value
            ApplyFinalValue(debounceValue);
        };
    }
    
    debounceTimer.Stop();
    debounceTimer.Start();
}
```

## Common Scenarios

### Volume Control with Real-Time Display

```csharp
private Label volumeLabel;

public void SetupVolumeControl()
{
    trackBarEx1.Minimum = 0;
    trackBarEx1.Maximum = 100;
    trackBarEx1.Value = 50;
    trackBarEx1.ShowButtons = true;
    
    volumeLabel = new Label();
    
    trackBarEx1.Scroll += (s, e) => {
        int volume = trackBarEx1.Value;
        volumeLabel.Text = $"Volume: {volume}%";
        ApplyVolume(volume);
    };
}

private void ApplyVolume(int volume)
{
    // Apply volume to audio system
    System.Diagnostics.Debug.WriteLine($"Setting volume to {volume}");
}
```

### Brightness Adjustment

```csharp
private void SetupBrightnessSlider()
{
    trackBarEx1.Minimum = 0;
    trackBarEx1.Maximum = 255;
    trackBarEx1.Value = 128;
    trackBarEx1.Orientation = Orientation.Horizontal;
    
    trackBarEx1.Scroll += (s, e) => {
        int brightness = trackBarEx1.Value;
        ApplyBrightness(brightness);
    };
}

private void ApplyBrightness(int brightness)
{
    // Apply brightness to display or image
    Color newColor = Color.FromArgb(brightness, brightness, brightness);
    this.BackColor = newColor;
}
```

### Range Selector

```csharp
private Label minLabel, maxLabel;

public void SetupRangeControl()
{
    trackBarEx1.Minimum = 1;
    trackBarEx1.Maximum = 100;
    trackBarEx1.Value = 50;
    
    minLabel = new Label();
    maxLabel = new Label();
    
    trackBarEx1.Scroll += (s, e) => {
        int value = trackBarEx1.Value;
        minLabel.Text = $"Min: {value}";
        maxLabel.Text = $"Max: {value + 50}";
    };
}
```

### Zoom Level Control

```csharp
private void SetupZoomSlider()
{
    trackBarEx1.Minimum = 25;   // 25% zoom
    trackBarEx1.Maximum = 400;  // 400% zoom
    trackBarEx1.Value = 100;    // 100% (normal)
    trackBarEx1.SmallChange = 10;
    trackBarEx1.LargeChange = 50;
    
    trackBarEx1.Scroll += (s, e) => {
        int zoomPercent = trackBarEx1.Value;
        ApplyZoom(zoomPercent / 100.0);
    };
}

private void ApplyZoom(double zoomFactor)
{
    System.Diagnostics.Debug.WriteLine($"Zoom: {zoomFactor * 100:F0}%");
}
```

## Best Practices

### Always Initialize in Form Load

Ensure control is fully initialized before handling events:

```csharp
private void Form_Load(object sender, EventArgs e)
{
    // Set up initial values
    trackBarEx1.Minimum = 0;
    trackBarEx1.Maximum = 100;
    trackBarEx1.Value = 50;
    
    // Subscribe to events after initialization
    trackBarEx1.Scroll += TrackBarEx1_Scroll;
}
```

### Avoid Recursive Updates

Don't update the slider value from within its own scroll event:

```csharp
// BAD: Creates infinite loop
private void trackBarEx1_Scroll(object sender, EventArgs e)
{
    trackBarEx1.Value = trackBarEx1.Value + 1;  // Don't do this!
}

// GOOD: Process the value, don't modify it
private void trackBarEx1_Scroll(object sender, EventArgs e)
{
    ProcessValue(trackBarEx1.Value);  // Handle the value safely
}
```

### Unsubscribe from Events

Clean up event handlers when control is disposed:

```csharp
private void Form_FormClosing(object sender, FormClosingEventArgs e)
{
    if (trackBarEx1 != null)
    {
        trackBarEx1.Scroll -= TrackBarEx1_Scroll;
    }
}
```

### Handle Edge Cases

Consider boundary conditions in event handlers:

```csharp
private void trackBarEx1_Scroll(object sender, EventArgs e)
{
    int value = trackBarEx1.Value;
    
    // Check for special values
    if (value == trackBarEx1.Minimum)
    {
        Debug.WriteLine("User set minimum value");
    }
    else if (value == trackBarEx1.Maximum)
    {
        Debug.WriteLine("User set maximum value");
    }
}
```
