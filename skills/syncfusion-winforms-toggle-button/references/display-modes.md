# Display Modes Configuration

Toggle Button can display either text or images to represent its states. The display mode is controlled by the `DisplayMode` property.

## Text Display Mode

**Text mode** is the default display mode. The button displays text defined in each state's `Text` property.

### Setting Text Display Mode

```csharp
// Set to text display mode
this.toggleButton1.DisplayMode = DisplayType.Text;

// Configure text for each state
this.toggleButton1.ActiveState.Text = "ON";
this.toggleButton1.InactiveState.Text = "OFF";
```

### Visual Basic

```vb
' Set to text display mode
Me.toggleButton1.DisplayMode = DisplayType.Text

' Configure text for each state
Me.toggleButton1.ActiveState.Text = "ON"
Me.toggleButton1.InactiveState.Text = "OFF"
```

### Text Mode Examples

#### Simple ON/OFF Toggle
```csharp
toggleButton.DisplayMode = DisplayType.Text;
toggleButton.ActiveState.Text = "ON";
toggleButton.InactiveState.Text = "OFF";
```

#### Yes/No Toggle
```csharp
toggleButton.DisplayMode = DisplayType.Text;
toggleButton.ActiveState.Text = "YES";
toggleButton.InactiveState.Text = "NO";
```

#### Enabled/Disabled Toggle
```csharp
toggleButton.DisplayMode = DisplayType.Text;
toggleButton.ActiveState.Text = "Enabled";
toggleButton.InactiveState.Text = "Disabled";
```

#### Custom Labels
```csharp
toggleButton.DisplayMode = DisplayType.Text;
toggleButton.ActiveState.Text = "Start";
toggleButton.InactiveState.Text = "Stop";
```

## Image Display Mode

**Image mode** allows the button to display images for each state. This is useful when visual icons better represent your states.

### Setting Image Display Mode

```csharp
// Set to image display mode
this.toggleButton1.DisplayMode = DisplayType.Image;

// Configure images for each state
this.toggleButton1.ActiveState.Image = LoadImage("active_image.png");
this.toggleButton1.InactiveState.Image = LoadImage("inactive_image.png");
```

### Visual Basic

```vb
' Set to image display mode
Me.toggleButton1.DisplayMode = DisplayType.Image

' Configure images for each state
Me.toggleButton1.ActiveState.Image = LoadImage("active_image.png")
Me.toggleButton1.InactiveState.Image = LoadImage("inactive_image.png")
```

### Loading Images

Here are common ways to load images:

#### From Embedded Resources
```csharp
private Image LoadImageFromResources(string imageName)
{
    var assembly = System.Reflection.Assembly.GetExecutingAssembly();
    var resourceName = $"YourNamespace.Resources.{imageName}";
    
    using (var stream = assembly.GetManifestResourceStream(resourceName))
    {
        if (stream != null)
        {
            return Image.FromStream(stream);
        }
    }
    return null;
}

// Usage
toggleButton.ActiveState.Image = LoadImageFromResources("play_icon.png");
toggleButton.InactiveState.Image = LoadImageFromResources("pause_icon.png");
```

#### From File System
```csharp
private Image LoadImageFromFile(string filePath)
{
    if (System.IO.File.Exists(filePath))
    {
        return Image.FromFile(filePath);
    }
    return null;
}

// Usage
toggleButton.ActiveState.Image = LoadImageFromFile(@"C:\Images\active.png");
toggleButton.InactiveState.Image = LoadImageFromFile(@"C:\Images\inactive.png");
```

#### From Application Resources
```csharp
toggleButton.ActiveState.Image = Properties.Resources.ActiveIcon;
toggleButton.InactiveState.Image = Properties.Resources.InactiveIcon;
```

## Switching Display Modes

You can change the display mode at runtime:

```csharp
private void SwitchToTextMode()
{
    toggleButton.DisplayMode = DisplayType.Text;
    toggleButton.ActiveState.Text = "ON";
    toggleButton.InactiveState.Text = "OFF";
}

private void SwitchToImageMode()
{
    toggleButton.DisplayMode = DisplayType.Image;
    toggleButton.ActiveState.Image = Properties.Resources.PowerOn;
    toggleButton.InactiveState.Image = Properties.Resources.PowerOff;
}

// Example: Button to switch modes
private void modeToggleButton_Click(object sender, EventArgs e)
{
    if (toggleButton.DisplayMode == DisplayType.Text)
    {
        SwitchToImageMode();
    }
    else
    {
        SwitchToTextMode();
    }
}
```

## Use Cases and Recommendations

### Text Mode Best Practices

Use text mode when:
- ✅ Users need clear, explicit state labels
- ✅ Multilingual support is required (easy to translate)
- ✅ Accessibility is important (screen readers can read text)
- ✅ Professional, minimalist UI is desired
- ✅ Space is limited and short text works best

**Example: System Settings Toggle**
```csharp
toggleButton.DisplayMode = DisplayType.Text;
toggleButton.ActiveState.Text = "Enable";
toggleButton.InactiveState.Text = "Disable";
toggleButton.ActiveState.BackColor = Color.Green;
toggleButton.InactiveState.BackColor = Color.Red;
```

### Image Mode Best Practices

Use image mode when:
- ✅ Visual representation is more intuitive than text
- ✅ Icons are universally understood (power, play/pause)
- ✅ UI language is not important
- ✅ More visual appeal is desired
- ✅ Limited text space is available

**Example: Media Control Toggle**
```csharp
toggleButton.DisplayMode = DisplayType.Image;
toggleButton.ActiveState.Image = Properties.Resources.PauseIcon;
toggleButton.InactiveState.Image = Properties.Resources.PlayIcon;
toggleButton.Size = new Size(80, 80);
```

## Combined Text and Image

Some scenarios benefit from both text and image. You can create a custom renderer to display both simultaneously. See the Custom Styling reference for implementation details.

## Accessibility Considerations

### Text Mode Accessibility
Text mode is naturally more accessible because:
- Screen readers can interpret the text
- Clear labels help all users understand state
- High contrast text is easy to read

### Image Mode Accessibility
For image mode, consider:
- Adding a text alternative through properties or tooltips
- Ensuring sufficient color contrast between states
- Using universally recognized symbols
- Providing keyboard focus indicators

## Common Display Mode Patterns

### Pattern 1: Power Toggle
```csharp
toggleButton.DisplayMode = DisplayType.Image;
toggleButton.Size = new Size(100, 50);
toggleButton.ActiveState.Image = LoadPowerOnIcon();
toggleButton.InactiveState.Image = LoadPowerOffIcon();
```

### Pattern 2: Recording Toggle
```csharp
toggleButton.DisplayMode = DisplayType.Image;
toggleButton.ActiveState.Image = LoadRecordingIcon();
toggleButton.InactiveState.Image = LoadStoppedIcon();
toggleButton.ActiveState.BackColor = Color.Red;
```

### Pattern 3: Connection Status
```csharp
toggleButton.DisplayMode = DisplayType.Text;
toggleButton.ActiveState.Text = "Connected";
toggleButton.InactiveState.Text = "Offline";
toggleButton.ActiveState.BackColor = Color.Green;
toggleButton.InactiveState.BackColor = Color.Red;
```

### Pattern 4: Debug Mode
```csharp
toggleButton.DisplayMode = DisplayType.Text;
toggleButton.ActiveState.Text = "Debug: ON";
toggleButton.InactiveState.Text = "Debug: OFF";
```

## Performance Tips

- **Preload Images**: Load images during form initialization, not in event handlers
- **Cache Images**: Store Image objects in member variables to avoid repeated loading
- **Use Embedded Resources**: Embedded images are faster than file system access
- **Image Size**: Scale images appropriately to avoid stretching/distortion

```csharp
// Efficient approach: Cache images as member variables
private Image _activeImage;
private Image _inactiveImage;

public Form1()
{
    InitializeComponent();
    LoadImagesOnce();
}

private void LoadImagesOnce()
{
    _activeImage = Properties.Resources.ActiveIcon;
    _inactiveImage = Properties.Resources.InactiveIcon;
    
    toggleButton.DisplayMode = DisplayType.Image;
    toggleButton.ActiveState.Image = _activeImage;
    toggleButton.InactiveState.Image = _inactiveImage;
}
```
