# Appearance and Styling

## Table of Contents
- [Border Customization](#border-customization)
- [Caption Bar Height](#caption-bar-height)
- [Caption Bar Color](#caption-bar-color)
- [Caption Text Customization](#caption-text-customization)
- [Caption Alignment](#caption-alignment)
- [Icon Alignment](#icon-alignment)
- [Caption Buttons](#caption-buttons)
- [Rounded Corners](#rounded-corners)
- [Common Styling Patterns](#common-styling-patterns)

## Border Customization

### Border Thickness

The border thickness controls how thick the form's border appears. The default border provides a subtle outline around the form.

**C#:**
```csharp
// Set border thickness (in pixels)
this.BorderThickness = 10;
```

**VB.NET:**
```vb
' Set border thickness (in pixels)
Me.BorderThickness = 10
```

**Typical Values:**
- `1` - Thin, subtle border
- `2-3` - Standard border
- `5-10` - Thick, prominent border

### Border Color

Customize the color of the form's border to match your application's theme.

**C#:**
```csharp
// Set border color
this.BorderColor = Color.FromArgb(17, 158, 218);

// Common color schemes
this.BorderColor = Color.Blue;          // Solid color
this.BorderColor = Color.FromArgb(41, 128, 185);  // Custom RGB
this.BorderColor = ColorTranslator.FromHtml("#2980b9"); // Hex color
```

**VB.NET:**
```vb
' Set border color
Me.BorderColor = Color.FromArgb(17, 158, 218)

' Common color schemes
Me.BorderColor = Color.Blue
Me.BorderColor = Color.FromArgb(41, 128, 185)
Me.BorderColor = ColorTranslator.FromHtml("#2980b9")
```

![Border Color Example](../images/BorderColor_Example.png)

## Caption Bar Height

### Basic Height Configuration

The caption bar height determines the vertical space allocated for the title bar.

**C#:**
```csharp
// Set caption bar height (in pixels)
this.CaptionBarHeight = 40;
```

**VB.NET:**
```vb
' Set caption bar height (in pixels)
Me.CaptionBarHeight = 40
```

**Recommended Values:**
- `30-35` - Minimal height (standard Windows)
- `40-45` - Comfortable height for standard icons
- `50-60` - Generous height for larger elements

![Caption Bar Height Example](../images/CaptionBarHeight_Example.png)

### Caption Bar Height Modes

Control how the caption bar height behaves when the form is maximized.

**C#:**
```csharp
// Retain same height when maximized
this.CaptionBarHeightMode = Syncfusion.Windows.Forms.Enums.CaptionBarHeightMode.SameAlwaysOnMaximize;

// Default behavior (reduced height when maximized)
this.CaptionBarHeightMode = Syncfusion.Windows.Forms.Enums.CaptionBarHeightMode.Default;
```

**VB.NET:**
```vb
' Retain same height when maximized
Me.CaptionBarHeightMode = Syncfusion.Windows.Forms.Enums.CaptionBarHeightMode.SameAlwaysOnMaximize

' Default behavior (reduced height when maximized)
Me.CaptionBarHeightMode = Syncfusion.Windows.Forms.Enums.CaptionBarHeightMode.Default
```

**Available Modes:**
- `Default` - Caption bar height is reduced when form is maximized (system default)
- `SameAlwaysOnMaximize` - Caption bar height remains the same in both normal and maximized states

## Caption Bar Color

Customize the background color of the caption bar.

**C#:**
```csharp
// Set caption bar color
this.CaptionBarColor = Color.Pink;

// Professional color schemes
this.CaptionBarColor = Color.FromArgb(41, 128, 185);  // Blue
this.CaptionBarColor = Color.FromArgb(44, 62, 80);    // Dark Gray
this.CaptionBarColor = Color.FromArgb(231, 76, 60);   // Red
this.CaptionBarColor = Color.FromArgb(46, 204, 113);  // Green
```

**VB.NET:**
```vb
' Set caption bar color
Me.CaptionBarColor = Color.Pink

' Professional color schemes
Me.CaptionBarColor = Color.FromArgb(41, 128, 185)  ' Blue
Me.CaptionBarColor = Color.FromArgb(44, 62, 80)    ' Dark Gray
Me.CaptionBarColor = Color.FromArgb(231, 76, 60)   ' Red
Me.CaptionBarColor = Color.FromArgb(46, 204, 113)  ' Green
```

![Caption Bar Color Example](../images/CaptionBarColor_Example.png)

## Caption Text Customization

### Caption Fore Color

Customize the color of the caption text (form title).

**C#:**
```csharp
// Set caption text color
this.CaptionForeColor = Color.Black;

// Ensure good contrast
this.CaptionBarColor = Color.White;
this.CaptionForeColor = Color.Black;  // Dark text on light background

this.CaptionBarColor = Color.FromArgb(44, 62, 80);
this.CaptionForeColor = Color.White;  // Light text on dark background
```

**VB.NET:**
```vb
' Set caption text color
Me.CaptionForeColor = Color.Black

' Ensure good contrast
Me.CaptionBarColor = Color.White
Me.CaptionForeColor = Color.Black  ' Dark text on light background

Me.CaptionBarColor = Color.FromArgb(44, 62, 80)
Me.CaptionForeColor = Color.White  ' Light text on dark background
```

![Caption Fore Color Example](../images/CaptionForeColor_Example.png)

### Caption Font

Customize the font used for the caption text.

**C#:**
```csharp
// Set caption font
this.CaptionFont = new Font("Calisto MT", 14.25F, FontStyle.Bold);

// Modern fonts
this.CaptionFont = new Font("Segoe UI", 11F, FontStyle.Regular);
this.CaptionFont = new Font("Arial", 10F, FontStyle.Bold);
this.CaptionFont = new Font("Calibri", 12F, FontStyle.Regular);
```

**VB.NET:**
```vb
' Set caption font
Me.CaptionFont = New Font("Calisto MT", 14.25F, FontStyle.Bold)

' Modern fonts
Me.CaptionFont = New Font("Segoe UI", 11F, FontStyle.Regular)
Me.CaptionFont = New Font("Arial", 10F, FontStyle.Bold)
Me.CaptionFont = New Font("Calibri", 12F, FontStyle.Regular)
```

![Caption Font Example](../images/CaptionFont_Example.png)

## Caption Alignment

### Horizontal Alignment

Align the caption text horizontally within the caption bar.

**C#:**
```csharp
// Right-align caption
this.CaptionAlign = HorizontalAlignment.Right;

// Center-align caption
this.CaptionAlign = HorizontalAlignment.Center;

// Left-align caption (default)
this.CaptionAlign = HorizontalAlignment.Left;
```

**VB.NET:**
```vb
' Right-align caption
Me.CaptionAlign = HorizontalAlignment.Right

' Center-align caption
Me.CaptionAlign = HorizontalAlignment.Center

' Left-align caption (default)
Me.CaptionAlign = HorizontalAlignment.Left
```

**Available Options:**
- `HorizontalAlignment.Left` - Align text to the left
- `HorizontalAlignment.Center` - Center text
- `HorizontalAlignment.Right` - Align text to the right

![Caption Horizontal Alignment Example](../images/CaptionAlign_Example.png)

### Vertical Alignment

Align the caption text vertically within the caption bar.

**C#:**
```csharp
// Top-align caption
this.CaptionVerticalAlignment = Syncfusion.Windows.Forms.VerticalAlignment.Top;

// Center-align caption (default)
this.CaptionVerticalAlignment = Syncfusion.Windows.Forms.VerticalAlignment.Center;

// Bottom-align caption
this.CaptionVerticalAlignment = Syncfusion.Windows.Forms.VerticalAlignment.Bottom;
```

**VB.NET:**
```vb
' Top-align caption
Me.CaptionVerticalAlignment = Syncfusion.Windows.Forms.VerticalAlignment.Top

' Center-align caption (default)
Me.CaptionVerticalAlignment = Syncfusion.Windows.Forms.VerticalAlignment.Center

' Bottom-align caption
Me.CaptionVerticalAlignment = Syncfusion.Windows.Forms.VerticalAlignment.Bottom
```

**Available Options:**
- `VerticalAlignment.Top` - Align text to top
- `VerticalAlignment.Center` - Center text vertically
- `VerticalAlignment.Bottom` - Align text to bottom

![Caption Vertical Alignment Example](../images/CaptionVerticalAlign_Example.png)

## Icon Alignment

Customize the alignment of the form icon in the caption bar.

**C#:**
```csharp
// Right-align icon
this.IconAlign = HorizontalAlignment.Right;

// Center-align icon
this.IconAlign = HorizontalAlignment.Center;

// Left-align icon (default)
this.IconAlign = HorizontalAlignment.Left;
```

**VB.NET:**
```vb
' Right-align icon
Me.IconAlign = HorizontalAlignment.Right

' Center-align icon
Me.IconAlign = HorizontalAlignment.Center

' Left-align icon (default)
Me.IconAlign = HorizontalAlignment.Left
```

![Icon Alignment Example](../images/IconAlign_Example.png)

## Caption Buttons

Caption buttons include the minimize, maximize/restore, and close buttons on the title bar.

### Caption Button Color

Customize the color of the caption buttons.

**C#:**
```csharp
// Set caption button color
this.CaptionButtonColor = Color.Red;

// Match theme colors
this.CaptionButtonColor = Color.White;  // Light buttons
this.CaptionButtonColor = Color.Black;  // Dark buttons
this.CaptionButtonColor = Color.FromArgb(52, 73, 94);  // Custom color
```

**VB.NET:**
```vb
' Set caption button color
Me.CaptionButtonColor = Color.Red

' Match theme colors
Me.CaptionButtonColor = Color.White  ' Light buttons
Me.CaptionButtonColor = Color.Black  ' Dark buttons
Me.CaptionButtonColor = Color.FromArgb(52, 73, 94)  ' Custom color
```

![Caption Button Color Example](../images/CaptionButtonColor_Example.png)

### Caption Button Hover Color

Customize the color of caption buttons when the mouse hovers over them.

**C#:**
```csharp
// Set hover color
this.CaptionButtonHoverColor = Color.Lime;

// Professional hover effects
this.CaptionButtonHoverColor = Color.FromArgb(231, 76, 60);  // Red (close button)
this.CaptionButtonHoverColor = Color.FromArgb(52, 152, 219);  // Blue
this.CaptionButtonHoverColor = Color.LightGray;  // Subtle gray
```

**VB.NET:**
```vb
' Set hover color
Me.CaptionButtonHoverColor = Color.Lime

' Professional hover effects
Me.CaptionButtonHoverColor = Color.FromArgb(231, 76, 60)  ' Red (close button)
Me.CaptionButtonHoverColor = Color.FromArgb(52, 152, 219)  ' Blue
Me.CaptionButtonHoverColor = Color.LightGray  ' Subtle gray
```

![Caption Button Hover Color Example](../images/CaptionButtonHover_Example.png)

## Rounded Corners

MetroForm supports rounded corners on Windows 11 and later operating systems.

**C#:**
```csharp
// Enable rounded corners (Windows 11+ only)
this.AllowRoundedCorners = true;
```

**VB.NET:**
```vb
' Enable rounded corners (Windows 11+ only)
Me.AllowRoundedCorners = true
```

**Important Notes:**
- Rounded corners are **only supported on Windows 11 and later**
- On earlier Windows versions, setting this property has no effect
- When rounded corners are enabled, the border and shadow are drawn by the operating system
- This overrides custom border styling when active

![Rounded Corners Example](../images/RoundedCorners_Example.png)

## Common Styling Patterns

### Pattern 1: Modern Blue Theme

```csharp
public MainForm()
{
    InitializeComponent();
    
    // Modern blue color scheme
    this.CaptionBarHeight = 40;
    this.CaptionBarColor = Color.FromArgb(41, 128, 185);
    this.CaptionForeColor = Color.White;
    this.CaptionFont = new Font("Segoe UI", 10F, FontStyle.Regular);
    
    this.BorderColor = Color.FromArgb(41, 128, 185);
    this.BorderThickness = 2;
    
    this.CaptionButtonColor = Color.White;
    this.CaptionButtonHoverColor = Color.FromArgb(52, 152, 219);
}
```

### Pattern 2: Dark Theme

```csharp
public MainForm()
{
    InitializeComponent();
    
    // Dark theme
    this.CaptionBarHeight = 35;
    this.CaptionBarColor = Color.FromArgb(44, 62, 80);
    this.CaptionForeColor = Color.FromArgb(236, 240, 241);
    this.CaptionFont = new Font("Segoe UI", 10F, FontStyle.Regular);
    
    this.BorderColor = Color.FromArgb(52, 73, 94);
    this.BorderThickness = 1;
    
    this.CaptionButtonColor = Color.FromArgb(189, 195, 199);
    this.CaptionButtonHoverColor = Color.FromArgb(231, 76, 60);
    
    // Set form background to match
    this.BackColor = Color.FromArgb(52, 73, 94);
}
```

### Pattern 3: Minimal White Theme

```csharp
public MainForm()
{
    InitializeComponent();
    
    // Minimal white theme
    this.CaptionBarHeight = 32;
    this.CaptionBarColor = Color.White;
    this.CaptionForeColor = Color.FromArgb(52, 73, 94);
    this.CaptionFont = new Font("Segoe UI", 9F, FontStyle.Regular);
    
    this.BorderColor = Color.FromArgb(189, 195, 199);
    this.BorderThickness = 1;
    
    this.CaptionButtonColor = Color.FromArgb(127, 140, 141);
    this.CaptionButtonHoverColor = Color.FromArgb(231, 76, 60);
    
    this.BackColor = Color.FromArgb(236, 240, 241);
}
```

### Pattern 4: Professional Office Theme

```csharp
public MainForm()
{
    InitializeComponent();
    
    // Professional Office-style theme
    this.CaptionBarHeight = 38;
    this.CaptionBarColor = Color.FromArgb(43, 87, 154);
    this.CaptionForeColor = Color.White;
    this.CaptionFont = new Font("Segoe UI", 10F, FontStyle.Regular);
    this.CaptionAlign = HorizontalAlignment.Left;
    
    this.BorderColor = Color.FromArgb(43, 87, 154);
    this.BorderThickness = 2;
    
    this.CaptionButtonColor = Color.White;
    this.CaptionButtonHoverColor = Color.FromArgb(71, 117, 182);
}
```

### Pattern 5: Centered Title with Rounded Corners (Windows 11)

```csharp
public MainForm()
{
    InitializeComponent();
    
    // Modern centered design with rounded corners
    this.Text = "My Application";
    this.CaptionBarHeight = 45;
    this.CaptionBarColor = Color.FromArgb(88, 86, 214);
    this.CaptionForeColor = Color.White;
    this.CaptionFont = new Font("Segoe UI Semibold", 11F);
    this.CaptionAlign = HorizontalAlignment.Center;
    this.CaptionVerticalAlignment = Syncfusion.Windows.Forms.VerticalAlignment.Center;
    
    this.AllowRoundedCorners = true;  // Windows 11+
    this.BorderColor = Color.FromArgb(88, 86, 214);
    
    this.CaptionButtonColor = Color.White;
    this.CaptionButtonHoverColor = Color.FromArgb(116, 114, 227);
}
```

## Best Practices

### Color Contrast

1. **Ensure readability** - Caption text should have sufficient contrast with the caption bar background
2. **WCAG Guidelines** - Aim for at least 4.5:1 contrast ratio for normal text
3. **Test visibility** - Verify text is readable in different lighting conditions

### Consistency

1. **Match application theme** - Border and caption colors should align with your overall UI theme
2. **Consistent sizing** - Use similar caption bar heights across different forms
3. **Button colors** - Keep caption button colors consistent with the theme

### Performance

1. **Avoid frequent changes** - Minimize runtime changes to appearance properties
2. **Set once** - Configure appearance properties in the constructor
3. **Batch updates** - If multiple properties must change, suspend layout during updates

### Accessibility

1. **High contrast support** - Test with Windows high contrast themes
2. **Font sizes** - Use readable font sizes (9pt minimum, 10-11pt recommended)
3. **Button visibility** - Ensure caption buttons are easily visible and clickable

## Troubleshooting

### Colors Not Applying

**Problem:** Color changes don't appear on the form.

**Solutions:**
- Ensure you're setting properties on the correct form instance
- Check if custom painting is overriding colors (see Advanced Customization)
- Rebuild the project and restart the application
- Verify the form is actually inheriting from MetroForm

### Caption Text Not Visible

**Problem:** Form title text is not showing.

**Solutions:**
- Verify `CaptionForeColor` contrasts with `CaptionBarColor`
- Check that `CaptionFont` size is appropriate for `CaptionBarHeight`
- Ensure `this.Text` property is set
- Verify caption alignment isn't pushing text out of view

### Rounded Corners Not Working

**Problem:** `AllowRoundedCorners` has no effect.

**Solutions:**
- Verify you're running on Windows 11 or later
- Rounded corners only work on Windows 11+
- On earlier OS versions, this feature is not available
- Check Windows version: `Environment.OSVersion.Version`

### Caption Bar Too Small

**Problem:** Caption bar doesn't fit content properly.

**Solutions:**
- Increase `CaptionBarHeight` to accommodate larger fonts or images
- Typical heights: 30-35 (minimal), 40-45 (standard), 50+ (generous)
- Test with different DPI settings
- Account for font metrics and padding
