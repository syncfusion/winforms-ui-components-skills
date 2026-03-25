# Form Customization

This guide covers customization options for the SfForm itself, including icon configuration, border styling, shadow effects, and rounded corners.

## Table of Contents
- [Form Icon](#form-icon)
- [Form Icon Alignment](#form-icon-alignment)
- [Form Border](#form-border)
- [Shadow Effect](#shadow-effect)
- [Rounded Corners](#rounded-corners)

## Form Icon

The form icon appears in the title bar and the Windows taskbar. It represents your application visually.

### Changing Form Icon

Use the standard `Icon` property to set the form icon:

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Load icon from file
    this.Icon = new Icon("sfIcon.ico");
}
```

**VB.NET:**
```vb
Public Sub New()
    InitializeComponent()
    
    ' Load icon from file
    Me.Icon = New Icon("sfIcon.ico")
End Sub
```

### Loading Icon from Resources

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Load from embedded resources
    this.Icon = Properties.Resources.MyApplicationIcon;
}
```

**VB.NET:**
```vb
Public Sub New()
    InitializeComponent()
    
    ' Load from embedded resources
    Me.Icon = My.Resources.MyApplicationIcon
End Sub
```

### Icon Best Practices
- Use .ico files for best results (supports multiple sizes)
- Include multiple resolutions in the .ico (16x16, 32x32, 48x48, 256x256)
- Ensure icons look good on both light and dark backgrounds
- Test icon appearance in both title bar and taskbar
- Keep icon simple and recognizable at small sizes

### Hiding the Icon

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Hide the form icon
    this.ShowIcon = false;
}
```

## Form Icon Alignment

SfForm allows you to control both vertical and horizontal alignment of the form icon in the title bar.

### Properties
- `Style.TitleBar.IconVerticalAlignment` - Top, Center, Bottom
- `Style.TitleBar.IconHorizontalAlignment` - Left, Center, Right

### Vertical Alignment

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Align icon to top of title bar
    this.Style.TitleBar.IconVerticalAlignment = VerticalAlignment.Top;
}
```

**VB.NET:**
```vb
Public Sub New()
    InitializeComponent()
    
    ' Align icon to top of title bar
    Me.Style.TitleBar.IconVerticalAlignment = VerticalAlignment.Top
End Sub
```

**Available Values:**
- `VerticalAlignment.Top` - Align to top edge
- `VerticalAlignment.Center` - Center vertically (default)
- `VerticalAlignment.Bottom` - Align to bottom edge

### Horizontal Alignment

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Align icon to right side of title bar
    this.Style.TitleBar.IconHorizontalAlignment = HorizontalAlignment.Right;
}
```

**VB.NET:**
```vb
Public Sub New()
    InitializeComponent()
    
    ' Align icon to right side of title bar
    Me.Style.TitleBar.IconHorizontalAlignment = HorizontalAlignment.Right
End Sub
```

**Available Values:**
- `HorizontalAlignment.Left` - Align to left (default)
- `HorizontalAlignment.Center` - Center horizontally
- `HorizontalAlignment.Right` - Align to right

### Combined Alignment Example

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    this.Icon = new Icon("app.ico");
    this.Style.TitleBar.Height = 40;
    
    // Position icon at top-right
    this.Style.TitleBar.IconVerticalAlignment = VerticalAlignment.Top;
    this.Style.TitleBar.IconHorizontalAlignment = HorizontalAlignment.Right;
}
```

**VB.NET:**
```vb
Public Sub New()
    InitializeComponent()
    
    Me.Icon = New Icon("app.ico")
    Me.Style.TitleBar.Height = 40
    
    ' Position icon at top-right
    Me.Style.TitleBar.IconVerticalAlignment = VerticalAlignment.Top
    Me.Style.TitleBar.IconHorizontalAlignment = HorizontalAlignment.Right
End Sub
```

### Icon Alignment Use Cases
- **Top-Left (default):** Standard Windows convention
- **Center-Center:** For symmetric, minimalist designs
- **Top-Right:** Unusual but can work with custom title bar layouts
- **Custom positioning:** Match your application's visual identity

## Form Border

SfForm provides comprehensive border customization for both active and inactive window states.

### Properties
- `Style.Border` - Border appearance when window is active
- `Style.InactiveBorder` - Border appearance when window is inactive

### Basic Border Customization

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Active border (window has focus)
    this.Style.Border = new Pen(Color.SkyBlue, 2);
    
    // Inactive border (window lost focus)
    this.Style.InactiveBorder = new Pen(Color.LightGray, 2);
}
```

**VB.NET:**
```vb
Public Sub New()
    InitializeComponent()
    
    ' Active border (window has focus)
    Me.Style.Border = New Pen(Color.SkyBlue, 2)
    
    ' Inactive border (window lost focus)
    Me.Style.InactiveBorder = New Pen(Color.LightGray, 2)
End Sub
```

### Border with Different Widths

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Thick border when active
    this.Style.Border = new Pen(Color.FromArgb(0, 122, 204), 4);
    
    // Thin border when inactive
    this.Style.InactiveBorder = new Pen(Color.Gray, 1);
}
```

### Accent Color Border

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Use accent colors for modern look
    Color accentColor = Color.FromArgb(0, 120, 215); // Windows blue
    Color inactiveColor = Color.FromArgb(160, 160, 160);
    
    this.Style.Border = new Pen(accentColor, 3);
    this.Style.InactiveBorder = new Pen(inactiveColor, 3);
}
```

### Gradient Border (Custom)

While direct gradient borders aren't supported, you can simulate this effect:

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Use bright color for active to simulate emphasis
    this.Style.Border = new Pen(Color.FromArgb(0, 120, 215), 5);
    this.Style.InactiveBorder = new Pen(Color.FromArgb(200, 200, 200), 2);
    
    // Combine with shadow for depth
    this.Style.ShadowOpacity = 150;
}
```

### Border Styling Guidelines

**Width Recommendations:**
- **Subtle:** 1-2 pixels (modern, minimalist)
- **Standard:** 2-3 pixels (balanced visibility)
- **Prominent:** 4-5 pixels (emphasis, accent)
- **Bold:** 6+ pixels (distinctive, branding)

**Color Recommendations:**
- Use brand/accent colors for active border
- Use neutral gray tones for inactive border
- Ensure border contrasts with form background
- Consider accessibility (color blind users)

### Complete Border Example

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Title bar colors
    this.Style.TitleBar.BackColor = Color.FromArgb(0, 120, 215);
    this.Style.TitleBar.ForeColor = Color.White;
    
    // Active state
    this.Style.Border = new Pen(Color.FromArgb(0, 120, 215), 3);
    this.Style.BackColor = Color.White;
    
    // Inactive state
    this.Style.InactiveBorder = new Pen(Color.FromArgb(180, 180, 180), 2);
    this.Style.TitleBar.InactiveBackColor = Color.FromArgb(240, 240, 240);
    
    // Shadow for depth
    this.Style.ShadowOpacity = 120;
    this.Style.InactiveShadowOpacity = 60;
}
```

## Shadow Effect

Shadow effects add depth and visual polish to your forms. SfForm provides customizable shadows for both active and inactive states.

### Properties
- `Style.ShadowOpacity` - Shadow opacity when window is active (0-255)
- `Style.InactiveShadowOpacity` - Shadow opacity when window is inactive (0-255)

### Basic Shadow Configuration

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Active window shadow
    this.Style.ShadowOpacity = 255;  // Maximum opacity
    
    // Inactive window shadow
    this.Style.InactiveShadowOpacity = 100;  // Subtle shadow
}
```

**VB.NET:**
```vb
Public Sub New()
    InitializeComponent()
    
    ' Active window shadow
    Me.Style.ShadowOpacity = 255  ' Maximum opacity
    
    ' Inactive window shadow
    Me.Style.InactiveShadowOpacity = 100  ' Subtle shadow
End Sub
```

### Shadow Opacity Guidelines

**Opacity Values (0-255):**
- **0:** No shadow (disabled)
- **50-80:** Very subtle, minimal depth
- **100-150:** Moderate shadow, professional look
- **150-200:** Prominent shadow, clear elevation
- **200-255:** Strong shadow, maximum depth

### Subtle Shadow (Modern Design)

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Subtle shadows for modern, flat design
    this.Style.ShadowOpacity = 100;
    this.Style.InactiveShadowOpacity = 50;
}
```

### Prominent Shadow (Material Design)

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Stronger shadows for material design feel
    this.Style.ShadowOpacity = 180;
    this.Style.InactiveShadowOpacity = 90;
}
```

### Disabling Shadow

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Disable shadow completely
    this.Style.ShadowOpacity = 0;
    this.Style.InactiveShadowOpacity = 0;
}
```

### Shadow Best Practices

1. **Consistency**
   - Keep shadow style consistent across all forms in your application
   - Inactive shadow should be lighter than active shadow

2. **Context**
   - Dialog boxes: Moderate to strong shadow (150-200)
   - Main windows: Subtle to moderate shadow (100-150)
   - Splash screens: No shadow or very subtle (0-50)

3. **Performance**
   - Shadows can affect performance on lower-end hardware
   - Consider disabling shadows for applications with many windows

4. **Visual Hierarchy**
   - Use stronger shadows for modal dialogs to emphasize elevation
   - Use lighter shadows for child windows to show relationship

### Complete Shadow Example

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Modern design with subtle elevation
    this.Style.TitleBar.BackColor = Color.White;
    this.Style.TitleBar.ForeColor = Color.Black;
    this.Style.BackColor = Color.White;
    
    // Border
    this.Style.Border = new Pen(Color.FromArgb(200, 200, 200), 1);
    this.Style.InactiveBorder = new Pen(Color.FromArgb(220, 220, 220), 1);
    
    // Shadow for depth
    this.Style.ShadowOpacity = 120;
    this.Style.InactiveShadowOpacity = 60;
}
```

### Shadow with Dark Theme

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Dark theme
    this.Style.TitleBar.BackColor = Color.FromArgb(30, 30, 30);
    this.Style.TitleBar.ForeColor = Color.White;
    this.Style.BackColor = Color.FromArgb(45, 45, 48);
    
    // Border
    this.Style.Border = new Pen(Color.FromArgb(0, 122, 204), 2);
    this.Style.InactiveBorder = new Pen(Color.FromArgb(70, 70, 70), 2);
    
    // Stronger shadow works better with dark themes
    this.Style.ShadowOpacity = 200;
    this.Style.InactiveShadowOpacity = 100;
}
```

## Rounded Corners

SfForm supports Windows 11-style rounded corners, providing a modern appearance that matches the operating system.

### Property
`AllowRoundedCorners` (bool)

### Important Notes
- **OS Requirement:** Rounded corners are only supported on Windows 11 and later
- **System Rendering:** When enabled, Windows draws the border and shadow
- **No Effect on Older OS:** Setting this property on Windows 10 or earlier has no effect

### Enabling Rounded Corners

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Enable Windows 11 rounded corners
    this.AllowRoundedCorners = true;
}
```

**VB.NET:**
```vb
Public Sub New()
    InitializeComponent()
    
    ' Enable Windows 11 rounded corners
    Me.AllowRoundedCorners = True
End Sub
```

### Rounded Corners with Custom Styling

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Enable rounded corners
    this.AllowRoundedCorners = true;
    
    // Note: When rounded corners are enabled,
    // the OS draws borders and shadows.
    // Custom border/shadow settings may not apply.
    
    // You can still customize title bar
    this.Style.TitleBar.BackColor = Color.FromArgb(0, 120, 215);
    this.Style.TitleBar.ForeColor = Color.White;
    this.Style.BackColor = Color.White;
}
```

### Detecting Windows 11

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Check OS version and enable rounded corners
    if (Environment.OSVersion.Version.Build >= 22000) // Windows 11
    {
        this.AllowRoundedCorners = true;
    }
}
```

### Rounded Corners Best Practices

1. **OS Detection**
   - Always check OS version before relying on rounded corners
   - Provide fallback styling for older Windows versions

2. **Styling Limitations**
   - System-drawn borders replace custom borders when enabled
   - Shadow opacity settings may be overridden by OS
   - Test appearance on both Windows 10 and 11

3. **User Experience**
   - Rounded corners provide modern, polished appearance
   - Matches Windows 11 system aesthetics
   - Improves visual consistency with OS

4. **Design Considerations**
   - Works best with light or neutral color schemes
   - May look unusual with very dark or bright borders
   - Consider your target user base's OS

### Complete Modern Example (Windows 11)

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    this.Text = "Modern Windows 11 Application";
    this.Size = new Size(1000, 700);
    
    // Enable Windows 11 features
    if (Environment.OSVersion.Version.Build >= 22000)
    {
        this.AllowRoundedCorners = true;
    }
    
    // Modern title bar
    this.Style.TitleBar.BackColor = Color.White;
    this.Style.TitleBar.ForeColor = Color.Black;
    this.Style.TitleBar.Height = 35;
    
    // Button styling
    this.Style.TitleBar.CloseButtonForeColor = Color.Black;
    this.Style.TitleBar.CloseButtonHoverBackColor = Color.FromArgb(232, 17, 35);
    this.Style.TitleBar.CloseButtonHoverForeColor = Color.White;
    
    this.Style.TitleBar.MinimizeButtonForeColor = Color.Black;
    this.Style.TitleBar.MinimizeButtonHoverBackColor = Color.FromArgb(240, 240, 240);
    
    this.Style.TitleBar.MaximizeButtonForeColor = Color.Black;
    this.Style.TitleBar.MaximizeButtonHoverBackColor = Color.FromArgb(240, 240, 240);
    
    // Client area
    this.Style.BackColor = Color.FromArgb(250, 250, 250);
    
    // Subtle border and shadow (if not overridden by OS)
    this.Style.Border = new Pen(Color.FromArgb(200, 200, 200), 1);
    this.Style.ShadowOpacity = 100;
}
```

## Complete Form Customization Examples

### Example 1: Professional Business Application

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    this.Text = "Business Application";
    this.Size = new Size(1200, 800);
    this.Icon = new Icon("business.ico");
    
    // Title bar
    this.Style.TitleBar.BackColor = Color.FromArgb(0, 70, 130);
    this.Style.TitleBar.ForeColor = Color.White;
    this.Style.TitleBar.Height = 32;
    
    // Icon positioning
    this.Style.TitleBar.IconVerticalAlignment = VerticalAlignment.Center;
    this.Style.TitleBar.IconHorizontalAlignment = HorizontalAlignment.Left;
    
    // Buttons
    this.Style.TitleBar.CloseButtonForeColor = Color.White;
    this.Style.TitleBar.CloseButtonHoverBackColor = Color.FromArgb(220, 50, 50);
    
    // Border and shadow
    this.Style.Border = new Pen(Color.FromArgb(0, 70, 130), 2);
    this.Style.InactiveBorder = new Pen(Color.Gray, 1);
    this.Style.ShadowOpacity = 150;
    this.Style.InactiveShadowOpacity = 75;
    
    // Client area
    this.Style.BackColor = Color.White;
}
```

### Example 2: Creative Application with Bold Styling

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    this.Text = "Creative Studio";
    this.Size = new Size(1400, 900);
    
    // Bold title bar
    this.Style.TitleBar.BackColor = Color.FromArgb(230, 30, 100);
    this.Style.TitleBar.ForeColor = Color.White;
    this.Style.TitleBar.Height = 40;
    
    // Prominent border
    this.Style.Border = new Pen(Color.FromArgb(230, 30, 100), 5);
    this.Style.InactiveBorder = new Pen(Color.FromArgb(150, 150, 150), 3);
    
    // Strong shadow for impact
    this.Style.ShadowOpacity = 200;
    this.Style.InactiveShadowOpacity = 100;
    
    // Light client area for contrast
    this.Style.BackColor = Color.FromArgb(245, 245, 245);
}
```

### Example 3: Minimalist Design

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    this.Text = "Minimalist App";
    this.Size = new Size(900, 600);
    this.AllowRoundedCorners = true;  // Windows 11
    
    // Clean, minimal title bar
    this.Style.TitleBar.BackColor = Color.White;
    this.Style.TitleBar.ForeColor = Color.Black;
    this.Style.TitleBar.Height = 32;
    
    // No icon for ultra-minimal look
    this.ShowIcon = false;
    
    // Subtle border
    this.Style.Border = new Pen(Color.FromArgb(220, 220, 220), 1);
    this.Style.InactiveBorder = new Pen(Color.FromArgb(240, 240, 240), 1);
    
    // Very subtle shadow
    this.Style.ShadowOpacity = 80;
    this.Style.InactiveShadowOpacity = 40;
    
    // Pure white client area
    this.Style.BackColor = Color.White;
}
```

## Troubleshooting

### Border Not Visible
- Ensure `FormBorderStyle` is not set to `None`
- Check that border pen width is greater than 0
- Verify border color contrasts with desktop background

### Shadow Not Showing
- Confirm `ShadowOpacity` is greater than 0
- Check Windows visual effects settings (Performance Options)
- Verify DWM (Desktop Window Manager) is enabled

### Rounded Corners Not Working
- Verify OS is Windows 11 or later
- Check that `AllowRoundedCorners` is set to `true`
- Note that system draws borders when rounded corners are enabled

### Icon Not Displaying
- Ensure `.ico` file exists at specified path
- Check that `ShowIcon` property is `true`
- Verify icon file is valid and not corrupted
- Rebuild project if icon is embedded resource
