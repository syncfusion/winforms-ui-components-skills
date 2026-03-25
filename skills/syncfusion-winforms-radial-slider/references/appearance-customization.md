# Appearance Customization

This guide covers customizing the visual appearance of the RadialSlider control, including colors, needle types, and built-in themes.

## Table of Contents
- [Overview](#overview)
- [Color Properties](#color-properties)
- [Needle Customization](#needle-customization)
- [Built-in Visual Themes](#built-in-visual-themes)
- [Complete Styling Examples](#complete-styling-examples)
- [Theme Combinations](#theme-combinations)
- [Troubleshooting](#troubleshooting)

## Overview

The RadialSlider provides extensive customization options:
- **5 color properties** for different visual elements
- **2 needle types** (straight line and dotted line)
- **6 built-in visual themes** (Default + 5 Office 2016 styles)
- Full support for custom color schemes

## Color Properties

### BackgroundColor Property

Controls the background color of the slider.

**Type:** `Color`  
**Default:** System default

**C#:**
```csharp
// Light background
radialSlider1.BackgroundColor = Color.White;

// Dark background
radialSlider1.BackgroundColor = Color.FromArgb(30, 30, 30);

// Custom brand color
radialSlider1.BackgroundColor = Color.FromArgb(240, 248, 255);  // AliceBlue
```

**VB.NET:**
```vbnet
' Light background
radialSlider1.BackgroundColor = Color.White

' Dark background
radialSlider1.BackgroundColor = Color.FromArgb(30, 30, 30)

' Custom brand color
radialSlider1.BackgroundColor = Color.FromArgb(240, 248, 255)
```

**Best Practices:**
- Use light backgrounds for bright environments
- Use dark backgrounds for low-light scenarios or modern UIs
- Ensure sufficient contrast with circle and needle colors

### InnerCircleColor Property

Controls the color of the inner circle.

**Type:** `Color`  
**Default:** System default

**C#:**
```csharp
// Standard gray
radialSlider1.InnerCircleColor = Color.Gray;

// Accent color
radialSlider1.InnerCircleColor = Color.FromArgb(0, 120, 215);  // Windows blue

// Match brand colors
radialSlider1.InnerCircleColor = Color.FromArgb(46, 125, 50);  // Green
```

**VB.NET:**
```vbnet
' Standard gray
radialSlider1.InnerCircleColor = Color.Gray

' Accent color
radialSlider1.InnerCircleColor = Color.FromArgb(0, 120, 215)

' Match brand colors
radialSlider1.InnerCircleColor = Color.FromArgb(46, 125, 50)
```

### OuterCircleColor Property

Controls the color of the outer circle.

**Type:** `Color`  
**Default:** System default

**C#:**
```csharp
// Light gray outer ring
radialSlider1.OuterCircleColor = Color.LightGray;

// Contrasting dark ring
radialSlider1.OuterCircleColor = Color.FromArgb(60, 60, 60);

// Subtle border
radialSlider1.OuterCircleColor = Color.FromArgb(200, 200, 200);
```

**VB.NET:**
```vbnet
' Light gray outer ring
radialSlider1.OuterCircleColor = Color.LightGray

' Contrasting dark ring
radialSlider1.OuterCircleColor = Color.FromArgb(60, 60, 60)

' Subtle border
radialSlider1.OuterCircleColor = Color.FromArgb(200, 200, 200)
```

**Tips:**
- Make outer circle slightly darker/lighter than background for definition
- Use same color as inner circle for monochromatic look
- Add contrast for better visual separation

### SliderNeedleColor Property

Controls the color of the rotating needle.

**Type:** `Color`  
**Default:** System default

**C#:**
```csharp
// High-contrast red needle
radialSlider1.SliderNeedleColor = Color.Red;

// Professional blue
radialSlider1.SliderNeedleColor = Color.FromArgb(0, 122, 204);

// Vibrant accent
radialSlider1.SliderNeedleColor = Color.LimeGreen;

// Dark mode friendly
radialSlider1.SliderNeedleColor = Color.FromArgb(255, 185, 0);  // Amber
```

**VB.NET:**
```vbnet
' High-contrast red needle
radialSlider1.SliderNeedleColor = Color.Red

' Professional blue
radialSlider1.SliderNeedleColor = Color.FromArgb(0, 122, 204)

' Vibrant accent
radialSlider1.SliderNeedleColor = Color.LimeGreen

' Dark mode friendly
radialSlider1.SliderNeedleColor = Color.FromArgb(255, 185, 0)
```

**Recommendations:**
- Choose high-contrast colors for better visibility
- Bright colors (red, green, blue) work well for needles
- Match your application's accent color
- Avoid colors too similar to the background

### ForeColor Property

Controls the color of text labels (division markers and values).

**Type:** `Color`  
**Default:** System default

**C#:**
```csharp
// Black text for light backgrounds
radialSlider1.ForeColor = Color.Black;

// White text for dark backgrounds
radialSlider1.ForeColor = Color.White;

// Gray text for subtle appearance
radialSlider1.ForeColor = Color.FromArgb(100, 100, 100);
```

**VB.NET:**
```vbnet
' Black text for light backgrounds
radialSlider1.ForeColor = Color.Black

' White text for dark backgrounds
radialSlider1.ForeColor = Color.White

' Gray text for subtle appearance
radialSlider1.ForeColor = Color.FromArgb(100, 100, 100)
```

**Important:** Ensure text color contrasts well with background for readability.

## Needle Customization

### NeedleType Property

Controls the visual style of the needle.

**Type:** `NeedleType` enum  
**Default:** `NeedleType.StraightLine`

**Available Types:**
- **`NeedleType.StraightLine`** - Solid straight line from center (default)
- **`NeedleType.DottedLine`** - Dotted/dashed line appearance

### StraightLine Needle

**C#:**
```csharp
radialSlider1.NeedleType = NeedleType.StraightLine;
```

**VB.NET:**
```vbnet
radialSlider1.NeedleType = NeedleType.StraightLine
```

**Characteristics:**
- Solid, continuous line
- Classic pointer appearance
- High visibility
- Best for precision reading

**When to use:**
- Professional/business applications
- When clarity is paramount
- Minimal or modern designs

### DottedLine Needle

**C#:**
```csharp
radialSlider1.NeedleType = NeedleType.DottedLine;
```

**VB.NET:**
```vbnet
radialSlider1.NeedleType = NeedleType.DottedLine
```

**Characteristics:**
- Dashed/dotted appearance
- Subtle, less prominent
- Unique visual style
- Softer appearance

**When to use:**
- Creative or artistic interfaces
- When a softer look is desired
- To reduce visual weight
- Decorative applications

### Comparison Example

**C#:**
```csharp
// Create two sliders to compare needle types
RadialSlider straightNeedle = new RadialSlider
{
    Location = new Point(20, 20),
    Size = new Size(250, 250),
    NeedleType = NeedleType.StraightLine,
    SliderNeedleColor = Color.Red
};

RadialSlider dottedNeedle = new RadialSlider
{
    Location = new Point(290, 20),
    Size = new Size(250, 250),
    NeedleType = NeedleType.DottedLine,
    SliderNeedleColor = Color.Red
};

this.Controls.Add(straightNeedle);
this.Controls.Add(dottedNeedle);
```

## Built-in Visual Themes

The RadialSlider provides 6 pre-designed themes through the `Style` property.

**Type:** `Styles` enum  
**Default:** `Styles.Default`

### Available Themes

| Theme | Description | Best For |
|-------|-------------|----------|
| `Styles.Default` | Standard system appearance | Generic applications |
| `Styles.Office2016Colorful` | Vibrant Office 2016 colors | Modern, colorful UIs |
| `Styles.Office2016White` | Clean white theme | Light mode applications |
| `Styles.Office2016DarkGray` | Medium gray theme | Transitional/neutral UIs |
| `Styles.Office2016Black` | Dark black theme | Dark mode applications |

### Default Theme

**C#:**
```csharp
radialSlider1.Style = Styles.Default;
```

**VB.NET:**
```vbnet
radialSlider1.Style = Styles.Default
```

**Characteristics:**
- System colors
- Neutral appearance
- Works in any context
- No specific design language

### Office2016Colorful Theme

**C#:**
```csharp
radialSlider1.Style = Styles.Office2016Colorful;
```

**VB.NET:**
```vbnet
radialSlider1.Style = Styles.Office2016Colorful
```

**Characteristics:**
- Vibrant, saturated colors
- Modern Office aesthetic
- Professional yet colorful
- High visual impact

**Recommended for:**
- Business productivity apps
- Data visualization tools
- Modern professional interfaces
- Applications targeting Office users

### Office2016White Theme

**C#:**
```csharp
radialSlider1.Style = Styles.Office2016White;
```

**VB.NET:**
```vbnet
radialSlider1.Style = Styles.Office2016White
```

**Characteristics:**
- Clean white background
- Subtle gray accents
- Minimalist appearance
- Light and airy

**Recommended for:**
- Light mode applications
- Medical or scientific interfaces
- Minimalist designs
- High-brightness environments

### Office2016DarkGray Theme

**C#:**
```csharp
radialSlider1.Style = Styles.Office2016DarkGray;
```

**VB.NET:**
```vbnet
radialSlider1.Style = Styles.Office2016DarkGray
```

**Characteristics:**
- Medium gray tones
- Balanced contrast
- Professional appearance
- Works in most lighting

**Recommended for:**
- Neutral applications
- Professional tools
- Transitional themes (supports light/dark)
- Long-duration usage scenarios

### Office2016Black Theme

**C#:**
```csharp
radialSlider1.Style = Styles.Office2016Black;
```

**VB.NET:**
```vbnet
radialSlider1.Style = Styles.Office2016Black
```

**Characteristics:**
- Dark black background
- High contrast accents
- Modern dark mode
- Reduced eye strain

**Recommended for:**
- Dark mode applications
- Low-light environments
- Media/creative tools
- Night-time usage
- Developer tools

## Complete Styling Examples

### Example 1: Modern Light Theme

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class ModernLightSlider : Form
{
    public ModernLightSlider()
    {
        InitializeComponent();
        
        RadialSlider slider = new RadialSlider
        {
            Location = new Point(50, 50),
            Size = new Size(300, 300),
            MinimumValue = 0,
            MaximumValue = 100,
            Value = 65,
            SliderDivision = 10,
            
            // Modern light styling
            Style = Styles.Office2016White,
            BackgroundColor = Color.FromArgb(250, 250, 250),
            InnerCircleColor = Color.FromArgb(200, 200, 200),
            OuterCircleColor = Color.FromArgb(180, 180, 180),
            SliderNeedleColor = Color.FromArgb(0, 120, 215),  // Blue accent
            ForeColor = Color.FromArgb(60, 60, 60),
            NeedleType = NeedleType.StraightLine
        };
        
        this.Controls.Add(slider);
        this.BackColor = Color.White;
        this.Text = "Modern Light Theme";
        this.Size = new Size(420, 420);
    }
}
```

### Example 2: Dark Mode Design

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class DarkModeSlider : Form
{
    public DarkModeSlider()
    {
        InitializeComponent();
        
        RadialSlider slider = new RadialSlider
        {
            Location = new Point(50, 50),
            Size = new Size(300, 300),
            MinimumValue = 0,
            MaximumValue = 100,
            Value = 45,
            SliderDivision = 10,
            
            // Dark mode styling
            Style = Styles.Office2016Black,
            BackgroundColor = Color.FromArgb(20, 20, 20),
            InnerCircleColor = Color.FromArgb(80, 80, 80),
            OuterCircleColor = Color.FromArgb(100, 100, 100),
            SliderNeedleColor = Color.FromArgb(0, 200, 100),  // Green accent
            ForeColor = Color.FromArgb(220, 220, 220),
            NeedleType = NeedleType.StraightLine,
            SliderStyle = SliderStyles.Frame
        };
        
        this.Controls.Add(slider);
        this.BackColor = Color.FromArgb(30, 30, 30);
        this.Text = "Dark Mode Theme";
        this.Size = new Size(420, 420);
    }
}
```

### Example 3: Vibrant Accent Theme

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class VibrantSlider : Form
{
    public VibrantSlider()
    {
        InitializeComponent();
        
        RadialSlider slider = new RadialSlider
        {
            Location = new Point(50, 50),
            Size = new Size(300, 300),
            MinimumValue = 0,
            MaximumValue = 100,
            Value = 80,
            SliderDivision = 10,
            
            // Vibrant colorful styling
            Style = Styles.Office2016Colorful,
            BackgroundColor = Color.FromArgb(255, 245, 230),  // Warm background
            InnerCircleColor = Color.FromArgb(255, 152, 0),   // Orange
            OuterCircleColor = Color.FromArgb(255, 87, 34),   // Deep orange
            SliderNeedleColor = Color.FromArgb(211, 47, 47),  // Red
            ForeColor = Color.FromArgb(62, 39, 35),           // Dark brown
            NeedleType = NeedleType.StraightLine
        };
        
        this.Controls.Add(slider);
        this.BackColor = Color.FromArgb(255, 248, 225);
        this.Text = "Vibrant Theme";
        this.Size = new Size(420, 420);
    }
}
```

### Example 4: Professional Blue Theme

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class ProfessionalSlider : Form
{
    public ProfessionalSlider()
    {
        InitializeComponent();
        
        RadialSlider slider = new RadialSlider
        {
            Location = new Point(50, 50),
            Size = new Size(300, 300),
            MinimumValue = 0,
            MaximumValue = 100,
            Value = 55,
            SliderDivision = 10,
            
            // Professional blue styling
            Style = Styles.Office2016DarkGray,
            BackgroundColor = Color.FromArgb(236, 240, 245),
            InnerCircleColor = Color.FromArgb(66, 133, 244),   // Google blue
            OuterCircleColor = Color.FromArgb(51, 103, 214),
            SliderNeedleColor = Color.FromArgb(234, 67, 53),   // Red accent
            ForeColor = Color.FromArgb(60, 64, 67),
            NeedleType = NeedleType.StraightLine,
            SliderStyle = SliderStyles.Frame
        };
        
        this.Controls.Add(slider);
        this.BackColor = Color.FromArgb(248, 249, 250);
        this.Text = "Professional Theme";
        this.Size = new Size(420, 420);
    }
}
```

### Example 5: Minimal Monochrome

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class MinimalSlider : Form
{
    public MinimalSlider()
    {
        InitializeComponent();
        
        RadialSlider slider = new RadialSlider
        {
            Location = new Point(50, 50),
            Size = new Size(300, 300),
            MinimumValue = 0,
            MaximumValue = 100,
            Value = 50,
            SliderDivision = 10,
            
            // Minimal monochrome styling
            Style = Styles.Default,
            BackgroundColor = Color.White,
            InnerCircleColor = Color.FromArgb(120, 120, 120),
            OuterCircleColor = Color.FromArgb(120, 120, 120),
            SliderNeedleColor = Color.Black,
            ForeColor = Color.FromArgb(80, 80, 80),
            NeedleType = NeedleType.DottedLine,
            ShowOuterCircle = false  // Minimal look
        };
        
        this.Controls.Add(slider);
        this.BackColor = Color.White;
        this.Text = "Minimal Monochrome";
        this.Size = new Size(420, 420);
    }
}
```

## Theme Combinations

### Matching System Theme

Detect and apply system theme:

**C#:**
```csharp
private void ApplySystemTheme()
{
    // Check if Windows is in dark mode (Windows 10+)
    bool isDarkMode = IsDarkModeEnabled();
    
    if (isDarkMode)
    {
        // Apply dark theme
        radialSlider1.Style = Styles.Office2016Black;
        radialSlider1.BackgroundColor = Color.FromArgb(30, 30, 30);
        radialSlider1.ForeColor = Color.White;
        this.BackColor = Color.FromArgb(20, 20, 20);
    }
    else
    {
        // Apply light theme
        radialSlider1.Style = Styles.Office2016White;
        radialSlider1.BackgroundColor = Color.White;
        radialSlider1.ForeColor = Color.Black;
        this.BackColor = Color.FromArgb(240, 240, 240);
    }
}

private bool IsDarkModeEnabled()
{
    // Registry check for Windows dark mode
    try
    {
        using (var key = Microsoft.Win32.Registry.CurrentUser.OpenSubKey(
            @"Software\Microsoft\Windows\CurrentVersion\Themes\Personalize"))
        {
            var value = key?.GetValue("AppsUseLightTheme");
            return value is int i && i == 0;
        }
    }
    catch
    {
        return false;  // Default to light mode if check fails
    }
}
```

### Coordinating Multiple Sliders

**C#:**
```csharp
private void CreateCoordinatedSliders()
{
    // Define shared theme properties
    var theme = new
    {
        Style = Styles.Office2016Colorful,
        Background = Color.FromArgb(245, 245, 245),
        InnerCircle = Color.FromArgb(100, 100, 100),
        OuterCircle = Color.FromArgb(150, 150, 150),
        Foreground = Color.Black,
        NeedleType = NeedleType.StraightLine
    };
    
    // Volume slider (blue needle)
    RadialSlider volumeSlider = new RadialSlider
    {
        Location = new Point(20, 20),
        Size = new Size(200, 200),
        Style = theme.Style,
        BackgroundColor = theme.Background,
        InnerCircleColor = theme.InnerCircle,
        OuterCircleColor = theme.OuterCircle,
        ForeColor = theme.Foreground,
        NeedleType = theme.NeedleType,
        SliderNeedleColor = Color.Blue  // Unique needle color
    };
    
    // Bass slider (green needle)
    RadialSlider bassSlider = new RadialSlider
    {
        Location = new Point(240, 20),
        Size = new Size(200, 200),
        Style = theme.Style,
        BackgroundColor = theme.Background,
        InnerCircleColor = theme.InnerCircle,
        OuterCircleColor = theme.OuterCircle,
        ForeColor = theme.Foreground,
        NeedleType = theme.NeedleType,
        SliderNeedleColor = Color.Green  // Unique needle color
    };
    
    this.Controls.Add(volumeSlider);
    this.Controls.Add(bassSlider);
}
```

## Troubleshooting

### Colors Not Applying

**Issue:** Custom colors don't appear after setting properties.

**Solutions:**
1. Ensure `Style` property is set to `Styles.Default` or compatible theme
2. Some themes override custom colors - test with `Styles.Default` first
3. Call `Refresh()` after setting multiple properties:
   ```csharp
   radialSlider1.BackgroundColor = Color.Red;
   radialSlider1.Refresh();
   ```

### Theme Not Visible

**Issue:** Office 2016 theme appears unchanged.

**Solutions:**
1. Verify correct enum value: `Styles.Office2016Black` (not `Style.Black`)
2. Ensure Syncfusion version supports Office 2016 themes
3. Check that form background doesn't hide the slider
4. Try setting `Style` property before other appearance properties

### Low Contrast Between Elements

**Issue:** Needle or text hard to see against background.

**Solutions:**
1. Increase color contrast:
   ```csharp
   // Dark background
   radialSlider1.BackgroundColor = Color.Black;
   radialSlider1.SliderNeedleColor = Color.Yellow;  // High contrast
   radialSlider1.ForeColor = Color.White;
   ```
2. Test with accessibility tools (contrast checkers)
3. Use built-in themes as starting points
4. Avoid similar hue values for adjacent elements

### Needle Not Visible

**Issue:** Needle appears invisible or very faint.

**Solutions:**
1. Set explicit needle color with high contrast:
   ```csharp
   radialSlider1.SliderNeedleColor = Color.Red;
   ```
2. Try different `NeedleType`:
   ```csharp
   radialSlider1.NeedleType = NeedleType.StraightLine;  // More visible
   ```
3. Ensure needle color differs from background
4. Check that `Value` is within `MinimumValue` to `MaximumValue` range

### Text Labels Unreadable

**Issue:** Division markers or value text hard to read.

**Solutions:**
1. Set appropriate `ForeColor`:
   ```csharp
   // For dark backgrounds
   radialSlider1.ForeColor = Color.White;
   
   // For light backgrounds
   radialSlider1.ForeColor = Color.Black;
   ```
2. Use `DrawText` event for custom formatting (see [value-configuration.md](value-configuration.md))
3. Reduce font size if overlapping occurs
4. Adjust `SliderDivision` to reduce label density
