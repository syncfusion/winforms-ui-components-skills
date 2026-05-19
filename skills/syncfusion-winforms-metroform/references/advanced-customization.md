# Advanced Customization

This guide covers advanced customization techniques for MetroForm including custom painting, gradient effects, and mouse event handling for caption images.

## Caption Bar Background Customization

### Using CaptionBarBrush

The `CaptionBarBrush` property allows you to apply custom brush effects to the caption bar background, including gradients, textures, and patterns.

**C#:**
```csharp
// Apply horizontal gradient
this.CaptionBarBrush = new LinearGradientBrush(
    new Rectangle(0, 0, this.Width, this.CaptionBarHeight),
    Color.DarkRed,
    Color.Yellow,
    LinearGradientMode.Horizontal
);

// Apply vertical gradient
this.CaptionBarBrush = new LinearGradientBrush(
    new Rectangle(0, 0, this.Width, this.CaptionBarHeight),
    Color.FromArgb(41, 128, 185),
    Color.FromArgb(109, 213, 250),
    LinearGradientMode.Vertical
);

// Apply diagonal gradient
this.CaptionBarBrush = new LinearGradientBrush(
    new Rectangle(0, 0, this.Width, this.CaptionBarHeight),
    Color.DarkRed,
    Color.Yellow,
    LinearGradientMode.BackwardDiagonal
);
```

**VB.NET:**
```vb
' Apply horizontal gradient
Me.CaptionBarBrush = New LinearGradientBrush( _
    New Rectangle(0, 0, Me.Width, Me.CaptionBarHeight), _
    Color.DarkRed, _
    Color.Yellow, _
    LinearGradientMode.Horizontal)

' Apply vertical gradient
Me.CaptionBarBrush = New LinearGradientBrush( _
    New Rectangle(0, 0, Me.Width, Me.CaptionBarHeight), _
    Color.FromArgb(41, 128, 185), _
    Color.FromArgb(109, 213, 250), _
    LinearGradientMode.Vertical)
```

**Gradient Modes:**
- `LinearGradientMode.Horizontal` - Left to right
- `LinearGradientMode.Vertical` - Top to bottom
- `LinearGradientMode.ForwardDiagonal` - Top-left to bottom-right
- `LinearGradientMode.BackwardDiagonal` - Top-right to bottom-left

## Custom Caption Bar Painting

### CaptionBarPaint Event

The `CaptionBarPaint` event allows you to completely customize how the caption bar is drawn, providing full control over its appearance.

**C#:**
```csharp
public MainForm()
{
    InitializeComponent();
    
    // Subscribe to the paint event
    this.CaptionBarPaint += Form1_CaptionBarPaint;
}

// Custom paint handler
void Form1_CaptionBarPaint(object sender, PaintEventArgs e)
{
    // Fill with custom gradient
    using (var brush = new LinearGradientBrush(
        e.ClipRectangle,
        Color.DarkRed,
        Color.Yellow,
        LinearGradientMode.BackwardDiagonal))
    {
        e.Graphics.FillRectangle(brush, e.ClipRectangle);
    }
}
```

**VB.NET:**
```vb
Public Sub New()
    InitializeComponent()
    
    ' Subscribe to the paint event
    AddHandler Me.CaptionBarPaint, AddressOf Form1_CaptionBarPaint
End Sub

' Custom paint handler
Private Sub Form1_CaptionBarPaint(sender As Object, e As PaintEventArgs)
    ' Fill with custom gradient
    Using brush = New LinearGradientBrush( _
        e.ClipRectangle, _
        Color.DarkRed, _
        Color.Yellow, _
        LinearGradientMode.BackwardDiagonal)
        
        e.Graphics.FillRectangle(brush, e.ClipRectangle)
    End Using
End Sub
```

### Advanced Gradient Effects

**Multi-Color Gradient:**

```csharp
void Form1_CaptionBarPaint(object sender, PaintEventArgs e)
{
    // Create multi-color gradient
    using (var brush = new LinearGradientBrush(
        e.ClipRectangle,
        Color.Blue,
        Color.Purple,
        LinearGradientMode.Horizontal))
    {
        // Define blend colors
        ColorBlend colorBlend = new ColorBlend(4);
        colorBlend.Colors = new Color[] 
        { 
            Color.FromArgb(41, 128, 185),
            Color.FromArgb(142, 68, 173),
            Color.FromArgb(231, 76, 60),
            Color.FromArgb(241, 196, 15)
        };
        colorBlend.Positions = new float[] { 0f, 0.33f, 0.66f, 1f };
        
        brush.InterpolationColors = colorBlend;
        e.Graphics.FillRectangle(brush, e.ClipRectangle);
    }
}
```

**Radial Gradient:**

```csharp
void Form1_CaptionBarPaint(object sender, PaintEventArgs e)
{
    // Create radial gradient effect
    using (var path = new GraphicsPath())
    {
        path.AddEllipse(e.ClipRectangle);
        
        using (var brush = new PathGradientBrush(path))
        {
            brush.CenterColor = Color.White;
            brush.SurroundColors = new Color[] { Color.FromArgb(41, 128, 185) };
            
            e.Graphics.FillRectangle(brush, e.ClipRectangle);
        }
    }
}
```

**Texture Pattern:**

```csharp
void Form1_CaptionBarPaint(object sender, PaintEventArgs e)
{
    // Use an image as texture
    using (var textureBrush = new TextureBrush(Properties.Resources.Pattern))
    {
        e.Graphics.FillRectangle(textureBrush, e.ClipRectangle);
    }
}
```

## Mouse Events for Caption Images

Caption images support comprehensive mouse event handling, allowing you to create interactive elements in the caption bar.

### Available Mouse Events

- `ImageMouseDown` - Mouse button pressed on caption image
- `ImageMouseUp` - Mouse button released on caption image
- `ImageMouseEnter` - Mouse pointer enters caption image area
- `ImageMouseLeave` - Mouse pointer leaves caption image area
- `ImageMouseMove` - Mouse pointer moves within caption image area

### Event Data

All mouse events provide `MouseEventArgs` with the following properties:

| Property | Type | Description |
|----------|------|-------------|
| `Button` | MouseButtons | Which mouse button was pressed (Left, Right, Middle) |
| `Clicks` | int | Number of times button was clicked |
| `Delta` | int | Mouse wheel rotation count |
| `Location` | Point | Mouse position relative to image |
| `X` | int | Horizontal position |
| `Y` | int | Vertical position |

### Subscribing to Events

**C#:**
```csharp
public MainForm()
{
    InitializeComponent();
    
    // Create caption image
    CaptionImage logoImage = new CaptionImage
    {
        Image = Properties.Resources.AppLogo,
        Location = new Point(10, 8),
        Size = new Size(32, 32),
        BackColor = Color.Transparent
    };
    
    // Subscribe to mouse events
    logoImage.ImageMouseDown += LogoImage_MouseDown;
    logoImage.ImageMouseUp += LogoImage_MouseUp;
    logoImage.ImageMouseEnter += LogoImage_MouseEnter;
    logoImage.ImageMouseLeave += LogoImage_MouseLeave;
    logoImage.ImageMouseMove += LogoImage_MouseMove;
    
    this.CaptionImages.Add(logoImage);
}

void LogoImage_MouseDown(object sender, ImageMouseDownEventArgs e)
{
    Console.WriteLine($"Mouse down: Button={e.Button}, Location={e.Location}");
}

void LogoImage_MouseUp(object sender, ImageMouseUpEventArgs e)
{
    Console.WriteLine($"Mouse up: Button={e.Button}, Clicks={e.Clicks}");
}

void LogoImage_MouseEnter(object sender, ImageMouseEnterEventArgs e)
{
    Console.WriteLine("Mouse entered image");
}

void LogoImage_MouseLeave(object sender, ImageMouseLeaveEventArgs e)
{
    Console.WriteLine("Mouse left image");
}

void LogoImage_MouseMove(object sender, ImageMouseMoveEventArgs e)
{
    Console.WriteLine($"Mouse move: X={e.X}, Y={e.Y}");
}
```

**VB.NET:**
```vb
Public Sub New()
    InitializeComponent()
    
    ' Create caption image
    Dim logoImage As New CaptionImage With {
        .Image = My.Resources.AppLogo,
        .Location = New Point(10, 8),
        .Size = New Size(32, 32),
        .BackColor = Color.Transparent
    }
    
    ' Subscribe to mouse events
    AddHandler logoImage.ImageMouseDown, AddressOf LogoImage_MouseDown
    AddHandler logoImage.ImageMouseUp, AddressOf LogoImage_MouseUp
    AddHandler logoImage.ImageMouseEnter, AddressOf LogoImage_MouseEnter
    AddHandler logoImage.ImageMouseLeave, AddressOf LogoImage_MouseLeave
    AddHandler logoImage.ImageMouseMove, AddressOf LogoImage_MouseMove
    
    Me.CaptionImages.Add(logoImage)
End Sub

Private Sub LogoImage_MouseDown(sender As Object, e As ImageMouseDownEventArgs)
    Console.WriteLine($"Mouse down: Button={e.Button}, Location={e.Location}")
End Sub

Private Sub LogoImage_MouseUp(sender As Object, e As ImageMouseUpEventArgs)
    Console.WriteLine($"Mouse up: Button={e.Button}, Clicks={e.Clicks}")
End Sub

Private Sub LogoImage_MouseEnter(sender As Object, e As ImageMouseEnterEventArgs)
    Console.WriteLine("Mouse entered image")
End Sub

Private Sub LogoImage_MouseLeave(sender As Object, e As ImageMouseLeaveEventArgs)
    Console.WriteLine("Mouse left image")
End Sub

Private Sub LogoImage_MouseMove(sender As Object, e As ImageMouseMoveEventArgs)
    Console.WriteLine($"Mouse move: X={e.X}, Y={e.Y}")
End Sub
```

### Subscribing to All Images

**C#:**
```csharp
// Subscribe to events for all caption images
foreach (CaptionImage image in this.CaptionImages)
{
    image.ImageMouseUp += Image_MouseUp;
    image.ImageMouseDown += Image_MouseDown;
    image.ImageMouseMove += Image_MouseMove;
    image.ImageMouseEnter += Image_MouseEnter;
    image.ImageMouseLeave += Image_MouseLeave;
}
```

## Common Advanced Patterns

### Pattern 1: Clickable Logo with Feedback

```csharp
public MainForm()
{
    InitializeComponent();
    
    CaptionImage logo = new CaptionImage
    {
        Image = Properties.Resources.Logo,
        Location = new Point(10, 8),
        Size = new Size(32, 32),
        BackColor = Color.Transparent,
        Name = "Logo"
    };
    
    // Click handler
    logo.ImageMouseUp += (sender, e) =>
    {
        if (e.Button == MouseButtons.Left)
        {
            MessageBox.Show("Logo clicked!", "Info");
        }
    };
    
    // Hover effect
    logo.ImageMouseEnter += (sender, e) =>
    {
        Cursor = Cursors.Hand;
    };
    
    logo.ImageMouseLeave += (sender, e) =>
    {
        Cursor = Cursors.Default;
    };
    
    this.CaptionImages.Add(logo);
}
```

### Pattern 2: Interactive Settings Icon

```csharp
private CaptionImage settingsIcon;

public MainForm()
{
    InitializeComponent();
    
    settingsIcon = new CaptionImage
    {
        Image = Properties.Resources.SettingsIcon,
        Location = new Point(this.Width - 100, 12),
        Size = new Size(24, 24),
        BackColor = Color.Transparent
    };
    
    // Click to open settings
    settingsIcon.ImageMouseUp += (sender, e) =>
    {
        if (e.Button == MouseButtons.Left)
        {
            OpenSettingsDialog();
        }
    };
    
    // Visual feedback on hover
    settingsIcon.ImageMouseEnter += (sender, e) =>
    {
        settingsIcon.Image = Properties.Resources.SettingsIconHover;
        Cursor = Cursors.Hand;
    };
    
    settingsIcon.ImageMouseLeave += (sender, e) =>
    {
        settingsIcon.Image = Properties.Resources.SettingsIcon;
        Cursor = Cursors.Default;
    };
    
    this.CaptionImages.Add(settingsIcon);
}

private void OpenSettingsDialog()
{
    using (var settingsForm = new SettingsForm())
    {
        settingsForm.ShowDialog(this);
    }
}
```

### Pattern 3: Animated Caption Bar

```csharp
private Timer gradientTimer;
private float hueShift = 0f;

public MainForm()
{
    InitializeComponent();
    
    // Animated gradient
    this.CaptionBarPaint += AnimatedGradient_Paint;
}

void AnimatedGradient_Paint(object sender, PaintEventArgs e)
{
    Color color1 = ColorFromHSV(Handle, 0.8, 0.8);
    Color color2 = ColorFromHSV((Handle + 60) % 360, 0.8, 0.8);
    
    using (var brush = new LinearGradientBrush(
        e.ClipRectangle, color1, color2, LinearGradientMode.Horizontal))
    {
        e.Graphics.FillRectangle(brush, e.ClipRectangle);
    }
}

// Helper method to convert HSV to RGB
private Color ColorFromHSV(double hue, double saturation, double value)
{
    int hi = Convert.ToInt32(Math.Floor(hue / 60)) % 6;
    double f = hue / 60 - Math.Floor(hue / 60);

    value = value * 255;
    int v = Convert.ToInt32(value);
    int p = Convert.ToInt32(value * (1 - saturation));
    int q = Convert.ToInt32(value * (1 - f * saturation));
    int t = Convert.ToInt32(value * (1 - (1 - f) * saturation));

    if (hi == 0) return Color.FromArgb(255, v, t, p);
    else if (hi == 1) return Color.FromArgb(255, q, v, p);
    else if (hi == 2) return Color.FromArgb(255, p, v, t);
    else if (hi == 3) return Color.FromArgb(255, p, q, v);
    else if (hi == 4) return Color.FromArgb(255, t, p, v);
    else return Color.FromArgb(255, v, p, q);
}
```

### Pattern 4: Context Menu on Caption Image

```csharp
public MainForm()
{
    InitializeComponent();
    
    CaptionImage menuIcon = new CaptionImage
    {
        Image = Properties.Resources.MenuIcon,
        Location = new Point(10, 12),
        Size = new Size(20, 20),
        BackColor = Color.Transparent
    };
    
    // Show context menu on click
    menuIcon.ImageMouseUp += (sender, e) =>
    {
        if (e.Button == MouseButtons.Left)
        {
            ContextMenuStrip menu = new ContextMenuStrip();
            menu.Items.Add("File", null, (s, ev) => { /* File action */ });
            menu.Items.Add("Edit", null, (s, ev) => { /* Edit action */ });
            menu.Items.Add("View", null, (s, ev) => { /* View action */ });
            
            // Show menu at caption image location
            Point menuLocation = this.PointToClient(
                new Point(menuIcon.Location.X, menuIcon.Location.Y + menuIcon.Size.Height));
            menu.Show(this, menuLocation);
        }
    };
    
    this.CaptionImages.Add(menuIcon);
}
```

## Best Practices

### Custom Painting

1. **Performance** - Minimize complex drawing operations in paint events
2. **Dispose Resources** - Always use `using` statements for brushes, pens, and graphics objects
3. **Clip Rectangle** - Use `e.ClipRectangle` to only paint the required area
4. **Anti-aliasing** - Set `e.Graphics.SmoothingMode` for smoother gradients

### Event Handling

1. **Check Button** - Always verify which mouse button triggered the event
2. **Cursor Feedback** - Change cursor to indicate interactive elements
3. **Visual Feedback** - Provide hover effects to show clickable areas
4. **Cleanup** - Unsubscribe from events when disposing forms

### Gradients

1. **Color Choice** - Use complementary colors for appealing gradients
2. **Direction** - Choose gradient direction that fits your design
3. **Form Resize** - Update gradient bounds on form resize events
4. **Transparency** - Consider using alpha channels for subtle effects

## Troubleshooting

### Custom Paint Not Appearing

**Problem:** CaptionBarPaint event doesn't show custom drawing.

**Solutions:**
- Ensure you're subscribing to the event before form is shown
- Check that you're using `e.ClipRectangle` for the drawing bounds
- Verify brushes and graphics objects are properly initialized
- Call `this.Invalidate()` to force repaint if needed

### Mouse Events Not Firing

**Problem:** Caption image mouse events don't trigger.

**Solutions:**
- Verify image is added to `CaptionImages` collection
- Check image `Size` is greater than 0
- Ensure image `Location` is within caption bar bounds
- Verify event handlers are subscribed before form is shown

### Gradient Doesn't Match Caption Bar

**Problem:** Gradient brush doesn't fill caption bar correctly.

**Solutions:**
- Use `new Rectangle(0, 0, this.Width, this.CaptionBarHeight)` for bounds
- Handle form resize to update gradient bounds
- Use `e.ClipRectangle` in paint event for automatic sizing

### Performance Issues with Animations

**Problem:** Animated caption bar causes flickering or high CPU usage.

**Solutions:**
- Enable double buffering: `this.DoubleBuffered = true;`
- Increase timer interval (50-100ms is usually sufficient)
- Only invalidate the caption bar region, not the entire form
- Consider using simpler drawing operations
