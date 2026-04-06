# Caption Customization in Office2010Form

## Table of Contents
- [Overview](#overview)
- [Caption Text Alignment](#caption-text-alignment)
- [Caption Font Customization](#caption-font-customization)
- [Caption Fore Color](#caption-fore-color)
- [Caption Bar Height](#caption-bar-height)
- [Help Button Support](#help-button-support)
- [Combining Customizations](#combining-customizations)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

## Overview

Office2010Form provides extensive caption bar customization options, allowing you to control the appearance and layout of the form's title bar. You can customize:

- **Text alignment** (left, center, right)
- **Font style** (family, size, weight)
- **Text color** (any color)
- **Bar height** (custom heights for branding or touch interfaces)
- **Help button** (display context-sensitive help)

All caption customizations work seamlessly with color schemes and maintain the Office 2010 visual style.

## Caption Text Alignment

### CaptionAlign Property

Align the form caption text to left, center, or right using the `CaptionAlign` property.

**Property Type:** `System.Windows.Forms.HorizontalAlignment`

**Available Values:**
- `HorizontalAlignment.Left` - Left-aligned caption (default)
- `HorizontalAlignment.Center` - Center-aligned caption
- `HorizontalAlignment.Right` - Right-aligned caption

### Left Alignment (Default)

**C# Implementation:**
```csharp
this.CaptionAlign = HorizontalAlignment.Left;
```

**VB.NET Implementation:**
```vb
Me.CaptionAlign = System.Windows.Forms.HorizontalAlignment.Left
```

**When to Use:**
- Standard Windows application convention
- Traditional desktop applications
- Left-to-right reading languages (default)

### Center Alignment

**C# Implementation:**
```csharp
this.CaptionAlign = HorizontalAlignment.Center;
```

**VB.NET Implementation:**
```vb
Me.CaptionAlign = System.Windows.Forms.HorizontalAlignment.Center
```

**When to Use:**
- Dialog boxes and modal forms
- Branding-focused applications
- Symmetrical UI designs
- Modern application aesthetics

**Example:**
```csharp
public partial class CenteredCaptionForm : Office2010Form
{
    public CenteredCaptionForm()
    {
        InitializeComponent();
        
        this.Text = "Application Settings";
        this.CaptionAlign = HorizontalAlignment.Center;
        this.ColorScheme = Office2010Theme.Blue;
        
        // Good for dialog boxes
        this.FormBorderStyle = FormBorderStyle.FixedDialog;
        this.MaximizeBox = false;
        this.MinimizeBox = false;
    }
}
```

### Right Alignment

**C# Implementation:**
```csharp
this.CaptionAlign = HorizontalAlignment.Right;
```

**VB.NET Implementation:**
```vb
Me.CaptionAlign = System.Windows.Forms.HorizontalAlignment.Right
```

**When to Use:**
- Right-to-left (RTL) languages (Arabic, Hebrew)
- Specialized UI requirements
- Custom branding needs

## Caption Font Customization

### CaptionFont Property

Customize the font family, size, style, and weight of the caption text.

**Property Type:** `System.Drawing.Font`

### Basic Font Customization

**C# Implementation:**
```csharp
this.CaptionFont = new Font("Segoe UI", 12F, FontStyle.Bold);
```

**VB.NET Implementation:**
```vb
Me.CaptionFont = New System.Drawing.Font("Segoe UI", 12F, System.Drawing.FontStyle.Bold)
```

### Font with Multiple Styles

Combine font styles using bitwise OR:

```csharp
this.CaptionFont = new Font("Arial", 14F, FontStyle.Bold | FontStyle.Italic);
```

### Complete Font Customization

**Full Font Constructor:**
```csharp
this.CaptionFont = new Font(
    "Comic Sans MS",           // Font family
    15F,                        // Size in points
    FontStyle.Bold,            // Style
    GraphicsUnit.Point,        // Unit
    ((byte)(0))                // Character set
);
```

**VB.NET Version:**
```vb
Me.CaptionFont = New System.Drawing.Font( _
    "Comic Sans MS", _
    15F, _
    System.Drawing.FontStyle.Bold, _
    System.Drawing.GraphicsUnit.Point, _
    CByte((0)) _
)
```

### Common Font Patterns

**Modern Sans-Serif:**
```csharp
// Segoe UI - Modern Windows font
this.CaptionFont = new Font("Segoe UI", 11F, FontStyle.Regular);
```

**Corporate Branding:**
```csharp
// Custom branded font
this.CaptionFont = new Font("Corporate Font", 10F, FontStyle.Bold);
```

**Large Touch-Friendly:**
```csharp
// Larger for touch interfaces
this.CaptionFont = new Font("Segoe UI", 16F, FontStyle.Bold);
```

**Classic Serif:**
```csharp
// Traditional appearance
this.CaptionFont = new Font("Georgia", 11F, FontStyle.Regular);
```

### Font Example with Complete Form

```csharp
public partial class CustomFontForm : Office2010Form
{
    public CustomFontForm()
    {
        InitializeComponent();
        
        this.Text = "Custom Font Caption";
        this.Size = new Size(800, 600);
        
        // Apply custom caption font
        this.CaptionFont = new Font("Segoe UI", 14F, FontStyle.Bold);
        
        // Apply matching color scheme
        this.ColorScheme = Office2010Theme.Silver;
        this.UseOffice2010SchemeBackColor = true;
    }
}
```

## Caption Fore Color

### CaptionForeColor Property

Customize the color of the caption text independently of the color scheme.

**Property Type:** `System.Drawing.Color`

### Basic Color Application

**C# Implementation:**
```csharp
this.CaptionForeColor = Color.Pink;
```

**VB.NET Implementation:**
```vb
Me.CaptionForeColor = Color.Pink
```

### Using Named Colors

```csharp
// System named colors
this.CaptionForeColor = Color.White;
this.CaptionForeColor = Color.Black;
this.CaptionForeColor = Color.DarkBlue;
this.CaptionForeColor = Color.Crimson;
```

### Using RGB Colors

```csharp
// Custom RGB color
this.CaptionForeColor = Color.FromArgb(255, 102, 178, 255);  // Light blue

// RGB without alpha (fully opaque)
this.CaptionForeColor = Color.FromArgb(102, 178, 255);
```

### Using Hex Colors

```csharp
// Convert hex to Color
this.CaptionForeColor = ColorTranslator.FromHtml("#FF6B35");  // Orange
this.CaptionForeColor = ColorTranslator.FromHtml("#2C3E50");  // Dark gray
```

### Brand Color Integration

```csharp
public class BrandedCaptionForm : Office2010Form
{
    // Company brand colors
    private static readonly Color BrandPrimary = Color.FromArgb(0, 120, 215);
    private static readonly Color BrandWhite = Color.White;
    
    public BrandedCaptionForm()
    {
        InitializeComponent();
        
        this.Text = "Company Application";
        
        // Apply brand color to caption text
        this.CaptionForeColor = BrandWhite;
        
        // Use brand color as managed theme
        this.ColorScheme = Office2010Theme.Managed;
        Office2010Colors.ApplyManagedColors(this, BrandPrimary);
    }
}
```

### High Contrast Example

```csharp
public void ApplyHighContrastCaption()
{
    // Black theme with white caption text
    this.ColorScheme = Office2010Theme.Black;
    this.CaptionForeColor = Color.White;
    
    // Or white theme with black caption text
    this.ColorScheme = Office2010Theme.Silver;
    this.CaptionForeColor = Color.Black;
}
```

## Caption Bar Height

### CaptionBarHeight Property

Adjust the height of the caption bar to accommodate larger fonts, touch interfaces, or branding requirements.

**Property Type:** `int` (height in pixels)

**Default Value:** ~30 pixels (standard Windows caption height)

### Basic Height Adjustment

**C# Implementation:**
```csharp
this.CaptionBarHeight = 50;
```

**VB.NET Implementation:**
```vb
Me.CaptionBarHeight = 50
```

### Common Height Values

**Standard (Default):**
```csharp
this.CaptionBarHeight = 30;  // Standard Windows caption
```

**Touch-Friendly:**
```csharp
this.CaptionBarHeight = 60;  // Larger for touch interfaces
```

**Compact:**
```csharp
this.CaptionBarHeight = 25;  // Minimal height
```

**Branding/Logo:**
```csharp
this.CaptionBarHeight = 80;  // Room for logo or branding
```

### Height with Large Font Example

```csharp
public partial class LargeCaptionForm : Office2010Form
{
    public LargeCaptionForm()
    {
        InitializeComponent();
        
        this.Text = "Large Caption Example";
        
        // Large font requires taller caption bar
        this.CaptionFont = new Font("Segoe UI", 18F, FontStyle.Bold);
        this.CaptionBarHeight = 60;  // Accommodate large font
        
        // Center align for balance
        this.CaptionAlign = HorizontalAlignment.Center;
        
        this.ColorScheme = Office2010Theme.Blue;
    }
}
```

### Dynamic Height Calculation

Calculate height based on font size:

```csharp
public void SetCaptionFontWithHeight(Font font)
{
    this.CaptionFont = font;
    
    // Calculate height: font height + padding
    int calculatedHeight = (int)(font.GetHeight() * 2.5);
    this.CaptionBarHeight = Math.Max(30, calculatedHeight);  // Minimum 30
}

// Usage
SetCaptionFontWithHeight(new Font("Arial", 16F, FontStyle.Bold));
```

## Help Button Support

### HelpButton Property

Display a help button (?) in the caption bar for context-sensitive help.

**Property Type:** `bool`

### Enabling Help Button

**C# Implementation:**
```csharp
this.HelpButton = true;
```

**VB.NET Implementation:**
```vb
Me.HelpButton = True
```

**Requirements for Visibility:**
- `MaximizeBox` must be `false`
- `MinimizeBox` must be `false`
- `HelpButton` must be `true`

### Complete Help Button Setup

```csharp
public partial class HelpForm : Office2010Form
{
    public HelpForm()
    {
        InitializeComponent();
        
        this.Text = "Settings";
        
        // Enable help button (requires max/min disabled)
        this.HelpButton = true;
        this.MaximizeBox = false;
        this.MinimizeBox = false;
        
        // Fixed dialog style common for help-enabled forms
        this.FormBorderStyle = FormBorderStyle.FixedDialog;
        
        // Apply color scheme
        this.ColorScheme = Office2010Theme.Blue;
    }
    
    protected override void OnHelpButtonClicked(CancelEventArgs e)
    {
        // Handle help button click
        MessageBox.Show("Context-sensitive help information here",
                       "Help", MessageBoxButtons.OK, MessageBoxIcon.Information);
        
        // Prevent default help system
        e.Cancel = true;
        
        base.OnHelpButtonClicked(e);
    }
}
```

### Help Button Event Handling

```csharp
// Method 1: Override OnHelpButtonClicked
protected override void OnHelpButtonClicked(CancelEventArgs e)
{
    ShowContextHelp();
    e.Cancel = true;
    base.OnHelpButtonClicked(e);
}

// Method 2: Event handler
public HelpForm()
{
    InitializeComponent();
    this.HelpButtonClicked += HelpForm_HelpButtonClicked;
}

private void HelpForm_HelpButtonClicked(object sender, CancelEventArgs e)
{
    ShowContextHelp();
    e.Cancel = true;
}

private void ShowContextHelp()
{
    // Display help content
    Help.ShowHelp(this, "help.chm", HelpNavigator.Topic, "settings.html");
}
```

## Combining Customizations

### Complete Caption Customization

Combine all caption customizations for full control:

```csharp
public partial class FullyCustomizedForm : Office2010Form
{
    public FullyCustomizedForm()
    {
        InitializeComponent();
        
        this.Text = "Fully Customized Caption";
        this.Size = new Size(800, 600);
        
        // Caption alignment
        this.CaptionAlign = HorizontalAlignment.Center;
        
        // Custom font
        this.CaptionFont = new Font("Segoe UI", 14F, FontStyle.Bold);
        
        // Custom text color
        this.CaptionForeColor = Color.White;
        
        // Increased height
        this.CaptionBarHeight = 50;
        
        // Color scheme
        this.ColorScheme = Office2010Theme.Black;
        this.UseOffice2010SchemeBackColor = true;
    }
}
```

### Branded Application Example

```csharp
public partial class BrandedAppForm : Office2010Form
{
    // Brand colors
    private static readonly Color BrandDark = Color.FromArgb(25, 45, 65);
    private static readonly Color BrandAccent = Color.FromArgb(255, 165, 0);
    private static readonly Color BrandWhite = Color.White;
    
    public BrandedAppForm()
    {
        InitializeComponent();
        
        this.Text = "ACME Corporation Suite";
        this.Size = new Size(1024, 768);
        
        // Center-aligned branding
        this.CaptionAlign = HorizontalAlignment.Center;
        
        // Brand font
        this.CaptionFont = new Font("Segoe UI", 13F, FontStyle.Bold);
        
        // White text on dark background
        this.CaptionForeColor = BrandWhite;
        
        // Taller caption for presence
        this.CaptionBarHeight = 55;
        
        // Custom brand color theme
        this.ColorScheme = Office2010Theme.Managed;
        Office2010Colors.ApplyManagedColors(this, BrandDark);
        this.UseOffice2010SchemeBackColor = true;
    }
}
```

### Touch-Optimized Form

```csharp
public partial class TouchOptimizedForm : Office2010Form
{
    public TouchOptimizedForm()
    {
        InitializeComponent();
        
        this.Text = "Touch Interface";
        
        // Large font for readability
        this.CaptionFont = new Font("Segoe UI", 16F, FontStyle.Bold);
        
        // Tall caption bar for touch targets
        this.CaptionBarHeight = 70;
        
        // Center alignment
        this.CaptionAlign = HorizontalAlignment.Center;
        
        // High contrast
        this.ColorScheme = Office2010Theme.Black;
        this.CaptionForeColor = Color.White;
    }
}
```

## Common Patterns

### Pattern 1: Configuration Method

Create a reusable configuration method:

```csharp
public void ConfigureCaption(
    HorizontalAlignment alignment,
    Font font,
    Color foreColor,
    int height)
{
    this.CaptionAlign = alignment;
    this.CaptionFont = font;
    this.CaptionForeColor = foreColor;
    this.CaptionBarHeight = height;
}

// Usage
ConfigureCaption(
    HorizontalAlignment.Center,
    new Font("Segoe UI", 14F, FontStyle.Bold),
    Color.White,
    50
);
```

### Pattern 2: Theme-Aware Caption Color

Match caption color to theme:

```csharp
public void ApplyThemeWithCaptionColor(Office2010Theme theme)
{
    this.ColorScheme = theme;
    
    // Adjust caption color based on theme
    switch (theme)
    {
        case Office2010Theme.Black:
            this.CaptionForeColor = Color.White;
            break;
        case Office2010Theme.Blue:
        case Office2010Theme.Silver:
            this.CaptionForeColor = Color.Black;
            break;
    }
    
    this.UseOffice2010SchemeBackColor = true;
}
```

### Pattern 3: Base Form with Caption Defaults

Create a base form with standard caption customization:

```csharp
public class AppBaseForm : Office2010Form
{
    public AppBaseForm()
    {
        // Standard caption configuration for all forms
        this.CaptionFont = new Font("Segoe UI", 11F, FontStyle.Regular);
        this.CaptionBarHeight = 35;
        this.ColorScheme = Office2010Theme.Blue;
        this.UseOffice2010SchemeBackColor = true;
    }
}

// Child forms inherit caption settings
public partial class MainForm : AppBaseForm
{
    public MainForm()
    {
        InitializeComponent();
        this.Text = "Main Window";
        // Caption already configured by base class
    }
}
```

## Troubleshooting

### Issue: Caption Font Not Changing

**Problem:** Setting CaptionFont has no visible effect

**Solutions:**
1. Ensure font family is installed on system
2. Verify font size is reasonable (not too small/large)
3. Check if `DisableOffice2010Style = true` (would disable custom caption)

### Issue: Help Button Not Visible

**Problem:** HelpButton set to true but not showing

**Solutions:**
1. Set `MaximizeBox = false`
2. Set `MinimizeBox = false`
3. Both must be false for help button to appear

### Issue: Caption Height Too Small for Font

**Problem:** Large caption font is cut off

**Solution:** Increase CaptionBarHeight:
```csharp
this.CaptionFont = new Font("Segoe UI", 18F, FontStyle.Bold);
this.CaptionBarHeight = 60;  // Increase to fit font
```

### Issue: Caption Color Not Visible

**Problem:** Caption text color blends with background

**Solution:** Ensure sufficient contrast:
```csharp
// Dark theme needs light text
this.ColorScheme = Office2010Theme.Black;
this.CaptionForeColor = Color.White;  // High contrast
```
