# Appearance and Styling

This guide covers customizing the appearance of StatusBarAdv, including background colors, gradients, patterns, Metro colors, and sizing grip configuration.

## Table of Contents

- [Background Settings](#background-settings)
- [Gradient Backgrounds](#gradient-backgrounds)
- [Pattern Styles](#pattern-styles)
- [Metro Color Configuration](#metro-color-configuration)
- [Foreground Colors](#foreground-colors)
- [Sizing Grip](#sizing-grip)
- [Complete Styling Examples](#complete-styling-examples)

## When to Read This

Read this reference when:
- Customizing status bar background colors
- Applying gradient backgrounds
- Using pattern styles
- Configuring Metro color themes
- Setting foreground colors
- Adding or customizing sizing grip
- Creating professional-looking status bars

## Background Settings

The **BackgroundColor** property provides comprehensive background styling options using the **BrushInfo** class.

### Solid Color Background

**C#:**
```csharp
using Syncfusion.Drawing;

// Simple solid color
statusBarAdv1.BackColor = Color.LightSteelBlue;

// Using BrushInfo for solid color
statusBarAdv1.BackgroundColor = new BrushInfo(Color.AliceBlue);
```

**VB.NET:**
```vbnet
Imports Syncfusion.Drawing

' Simple solid color
statusBarAdv1.BackColor = Color.LightSteelBlue

' Using BrushInfo for solid color
statusBarAdv1.BackgroundColor = New BrushInfo(Color.AliceBlue)
```

### BackgroundColor Property Options

The **BackgroundColor** property (BrushInfo) provides these configuration options:

| Property | Description |
|----------|-------------|
| `Style` | Brush style: Solid, Pattern, or Gradient |
| `BackColor` | Background color |
| `ForeColor` | Foreground color (for patterns and gradients) |
| `PatternStyle` | Pattern style when Style = Pattern |
| `GradientStyle` | Gradient style when Style = Gradient |
| `GradientColors` | Array of colors for gradient |
| `VerticalGradient` | Whether gradient is vertical |

## Gradient Backgrounds

Gradient backgrounds provide professional, modern appearances.

### Gradient Styles

Six gradient styles are available:

1. **GradientStyle.ForwardDiagonal** - Top-left to bottom-right
2. **GradientStyle.BackwardDiagonal** - Top-right to bottom-left
3. **GradientStyle.Horizontal** - Left to right
4. **GradientStyle.Vertical** - Top to bottom
5. **GradientStyle.PathRectangle** - Rectangular gradient
6. **GradientStyle.PathEllipse** - Elliptical gradient

### Basic Gradient Configuration

**C#:**
```csharp
using Syncfusion.Drawing;

// Horizontal gradient (blue to lighter blue)
statusBarAdv1.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.SteelBlue,      // Start color
    Color.LightSteelBlue  // End color
);

// Vertical gradient
statusBarAdv1.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.AliceBlue,
    Color.LightBlue
);

// Diagonal gradient
statusBarAdv1.BackgroundColor = new BrushInfo(
    GradientStyle.ForwardDiagonal,
    Color.FromArgb(240, 245, 250),
    Color.FromArgb(220, 230, 240)
);
```

**VB.NET:**
```vbnet
Imports Syncfusion.Drawing

' Horizontal gradient (blue to lighter blue)
statusBarAdv1.BackgroundColor = New BrushInfo(
    GradientStyle.Horizontal,
    Color.SteelBlue,
    Color.LightSteelBlue
)

' Vertical gradient
statusBarAdv1.BackgroundColor = New BrushInfo(
    GradientStyle.Vertical,
    Color.AliceBlue,
    Color.LightBlue
)

' Diagonal gradient
statusBarAdv1.BackgroundColor = New BrushInfo(
    GradientStyle.ForwardDiagonal,
    Color.FromArgb(240, 245, 250),
    Color.FromArgb(220, 230, 240)
)
```

### PathRectangle and PathEllipse Gradients

**C#:**
```csharp
// Rectangular path gradient (center outward)
statusBarAdv1.BackgroundColor = new BrushInfo(
    GradientStyle.PathRectangle,
    Color.NavajoWhite,
    Color.IndianRed
);

// Elliptical path gradient
statusBarAdv1.BackgroundColor = new BrushInfo(
    GradientStyle.PathEllipse,
    Color.LightYellow,
    Color.Orange
);
```

### Multi-Color Gradients

Use the **GradientColors** property for multi-color gradients:

**C#:**
```csharp
BrushInfo gradientBrush = new BrushInfo(GradientStyle.Horizontal);
gradientBrush.GradientColors = new Color[]
{
    Color.FromArgb(240, 248, 255),  // AliceBlue
    Color.FromArgb(176, 224, 230),  // PowderBlue
    Color.FromArgb(135, 206, 235),  // SkyBlue
    Color.FromArgb(70, 130, 180)    // SteelBlue
};

statusBarAdv1.BackgroundColor = gradientBrush;
```

**VB.NET:**
```vbnet
Dim gradientBrush As New BrushInfo(GradientStyle.Horizontal)
gradientBrush.GradientColors = New Color() {
    Color.FromArgb(240, 248, 255),
    Color.FromArgb(176, 224, 230),
    Color.FromArgb(135, 206, 235),
    Color.FromArgb(70, 130, 180)
}

statusBarAdv1.BackgroundColor = gradientBrush
```

### VerticalGradient Property

The **VerticalGradient** property (boolean) provides quick vertical gradient configuration:

**C#:**
```csharp
// Enable vertical gradient
statusBarAdv1.BackgroundColor = new BrushInfo(Color.LightBlue, Color.SteelBlue);
statusBarAdv1.VerticalGradient = true;

// Horizontal gradient
statusBarAdv1.VerticalGradient = false;
```

## Pattern Styles

Pattern backgrounds provide textured appearances.

### Applying Pattern Style

**C#:**
```csharp
BrushInfo patternBrush = new BrushInfo();
patternBrush.Style = Syncfusion.Drawing.BrushStyle.Pattern;
patternBrush.BackColor = Color.White;
patternBrush.ForeColor = Color.LightGray;
patternBrush.PatternStyle = System.Drawing.Drawing2D.HatchStyle.DottedGrid;

statusBarAdv1.BackgroundColor = patternBrush;
```

### Common Pattern Styles

```csharp
// Cross pattern
patternBrush.PatternStyle = HatchStyle.Cross;

// Diagonal cross
patternBrush.PatternStyle = HatchStyle.DiagonalCross;

// Dotted grid
patternBrush.PatternStyle = HatchStyle.DottedGrid;

// Light grid
patternBrush.PatternStyle = HatchStyle.LargeGrid;

// Wave pattern
patternBrush.PatternStyle = HatchStyle.Wave;
```

## Metro Color Configuration

Metro colors provide accent colors for modern flat design.

### MetroColor Property

**C#:**
```csharp
// Set Metro accent color
statusBarAdv1.MetroColor = ColorTranslator.FromHtml("#16a5dc");

// Or use predefined color
statusBarAdv1.MetroColor = Color.DodgerBlue;

// Use Metro color as border
statusBarAdv1.UseMetroColorAsBorder = true;
```

**VB.NET:**
```vbnet
' Set Metro accent color
statusBarAdv1.MetroColor = ColorTranslator.FromHtml("#16a5dc")

' Or use predefined color
statusBarAdv1.MetroColor = Color.DodgerBlue

' Use Metro color as border
statusBarAdv1.UseMetroColorAsBorder = True
```

### UseMetroColorAsBorder

When **UseMetroColorAsBorder** is `true`, the MetroColor is applied as the border color:

**C#:**
```csharp
statusBarAdv1.BorderStyle = BorderStyle.FixedSingle;
statusBarAdv1.MetroColor = Color.FromArgb(0, 120, 215);  // Blue accent
statusBarAdv1.UseMetroColorAsBorder = true;  // Border will be blue
```

### Metro Color with Gradient

**C#:**
```csharp
// Gradient with Metro accent
statusBarAdv1.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.FromArgb(250, 250, 250),
    Color.FromArgb(235, 240, 245)
);

statusBarAdv1.MetroColor = Color.FromArgb(0, 120, 215);
statusBarAdv1.UseMetroColorAsBorder = true;
```

## Foreground Colors

The **ForeColor** property sets the color for text and graphics in the status bar.

**C#:**
```csharp
// Set foreground color for text
statusBarAdv1.ForeColor = Color.DarkSlateGray;

// Dark background with light text
statusBarAdv1.BackColor = Color.FromArgb(45, 45, 48);
statusBarAdv1.ForeColor = Color.White;
```

**VB.NET:**
```vbnet
' Set foreground color for text
statusBarAdv1.ForeColor = Color.DarkSlateGray

' Dark background with light text
statusBarAdv1.BackColor = Color.FromArgb(45, 45, 48)
statusBarAdv1.ForeColor = Color.White
```

## Sizing Grip

The **SizingGrip** property displays a resize grip at the bottom-right corner.

**C#:**
```csharp
// Show sizing grip
statusBarAdv1.SizingGrip = true;

// Hide sizing grip
statusBarAdv1.SizingGrip = false;
```

**VB.NET:**
```vbnet
' Show sizing grip
statusBarAdv1.SizingGrip = True

' Hide sizing grip
statusBarAdv1.SizingGrip = False
```

**When to use:**
- Enable for resizable forms (FormBorderStyle.Sizable)
- Disable for fixed-size forms or maximized windows

## Complete Styling Examples

### Modern Light Theme

**C#:**
```csharp
public void ApplyModernLightTheme(StatusBarAdv statusBar)
{
    // Subtle gradient background
    statusBar.BackgroundColor = new BrushInfo(
        GradientStyle.Vertical,
        Color.FromArgb(250, 250, 250),
        Color.FromArgb(240, 245, 250)
    );
    
    // Border
    statusBar.BorderStyle = BorderStyle.FixedSingle;
    statusBar.BorderColor = Color.FromArgb(220, 220, 220);
    
    // Text color
    statusBar.ForeColor = Color.FromArgb(60, 60, 60);
    
    // Metro accent
    statusBar.MetroColor = Color.FromArgb(0, 120, 215);
    statusBar.UseMetroColorAsBorder = false;
    
    // Sizing grip
    statusBar.SizingGrip = true;
}
```

### Dark Theme

**C#:**
```csharp
public void ApplyDarkTheme(StatusBarAdv statusBar)
{
    // Dark gradient
    statusBar.BackgroundColor = new BrushInfo(
        GradientStyle.Horizontal,
        Color.FromArgb(45, 45, 48),
        Color.FromArgb(37, 37, 38)
    );
    
    // No border or dark border
    statusBar.BorderStyle = BorderStyle.None;
    
    // Light text
    statusBar.ForeColor = Color.FromArgb(220, 220, 220);
    
    // Accent color
    statusBar.MetroColor = Color.FromArgb(0, 122, 204);
    
    // Sizing grip
    statusBar.SizingGrip = true;
}
```

### Blue Professional Theme

**C#:**
```csharp
public void ApplyBlueProfessionalTheme(StatusBarAdv statusBar)
{
    // Blue gradient
    statusBar.BackgroundColor = new BrushInfo(
        GradientStyle.Horizontal,
        Color.FromArgb(229, 241, 251),
        Color.FromArgb(210, 230, 247)
    );
    
    // Themed border
    statusBar.BorderStyle = BorderStyle.FixedSingle;
    statusBar.BorderColor = Color.FromArgb(160, 190, 220);
    
    // Dark blue text
    statusBar.ForeColor = Color.FromArgb(30, 57, 91);
    
    // Sizing grip
    statusBar.SizingGrip = true;
}
```

### Green Natural Theme

**C#:**
```csharp
public void ApplyGreenNaturalTheme(StatusBarAdv statusBar)
{
    // Green gradient
    statusBar.BackgroundColor = new BrushInfo(
        GradientStyle.Vertical,
        Color.FromArgb(240, 248, 240),
        Color.FromArgb(220, 237, 220)
    );
    
    // Green border
    statusBar.BorderStyle = BorderStyle.FixedSingle;
    statusBar.MetroColor = Color.FromArgb(76, 175, 80);  // Material Green
    statusBar.UseMetroColorAsBorder = true;
    
    // Dark green text
    statusBar.ForeColor = Color.FromArgb(27, 94, 32);
    
    // Sizing grip
    statusBar.SizingGrip = true;
}
```

### Office-Style Status Bar

**C#:**
```csharp
public void ApplyOfficeStyle(StatusBarAdv statusBar)
{
    // Light blue gradient (Office-like)
    statusBar.BackgroundColor = new BrushInfo(
        GradientStyle.Horizontal,
        Color.FromArgb(246, 250, 254),
        Color.FromArgb(227, 239, 250)
    );
    
    // Single-line border
    statusBar.BorderStyle = BorderStyle.FixedSingle;
    statusBar.BorderColor = Color.FromArgb(180, 200, 220);
    
    // Standard text color
    statusBar.ForeColor = Color.Black;
    
    // No Metro color
    statusBar.UseMetroColorAsBorder = false;
    
    // Sizing grip
    statusBar.SizingGrip = true;
}
```

### Flat Modern (Metro-Style)

**C#:**
```csharp
public void ApplyFlatModernStyle(StatusBarAdv statusBar)
{
    // Solid flat color
    statusBar.BackColor = Color.FromArgb(240, 240, 240);
    
    // No border for ultra-flat look
    statusBar.BorderStyle = BorderStyle.None;
    
    // Dark gray text
    statusBar.ForeColor = Color.FromArgb(60, 60, 60);
    
    // Accent color
    statusBar.MetroColor = Color.FromArgb(0, 120, 215);
    statusBar.UseMetroColorAsBorder = false;
    
    // Sizing grip
    statusBar.SizingGrip = false;  // Flat design typically doesn't show grip
}
```

### Gradient with Multiple Colors

**C#:**
```csharp
public void ApplyRainbowGradient(StatusBarAdv statusBar)
{
    BrushInfo rainbowGradient = new BrushInfo(GradientStyle.Horizontal);
    rainbowGradient.GradientColors = new Color[]
    {
        Color.FromArgb(255, 240, 245),  // Light pink
        Color.FromArgb(240, 248, 255),  // Alice blue
        Color.FromArgb(240, 255, 240),  // Honeydew
        Color.FromArgb(255, 250, 240)   // Floral white
    };
    
    statusBar.BackgroundColor = rainbowGradient;
    statusBar.BorderStyle = BorderStyle.FixedSingle;
    statusBar.BorderColor = Color.Silver;
    statusBar.ForeColor = Color.DarkSlateGray;
}
```

### Complete Application Example

**C#:**
```csharp
public partial class ThemedApplicationForm : Form
{
    private StatusBarAdv statusBar;
    private enum Theme { Light, Dark, Blue, Green }
    private Theme currentTheme = Theme.Light;
    
    public ThemedApplicationForm()
    {
        InitializeComponent();
        SetupStatusBar();
        ApplyTheme(currentTheme);
    }
    
    private void SetupStatusBar()
    {
        statusBar = new StatusBarAdv
        {
            Dock = DockStyle.Bottom,
            Height = 28
        };
        
        // Add panels
        statusBar.Controls.Add(new StatusBarAdvPanel 
        { 
            Text = "Ready",
            Size = new Size(120, 25)
        });
        
        statusBar.Controls.Add(new StatusBarAdvPanel 
        { 
            PanelType = StatusBarAdvPanelType.ShortTime,
            Size = new Size(80, 25)
        });
        
        this.Controls.Add(statusBar);
    }
    
    public void ApplyTheme(Theme theme)
    {
        currentTheme = theme;
        
        switch (theme)
        {
            case Theme.Light:
                ApplyModernLightTheme(statusBar);
                break;
            
            case Theme.Dark:
                ApplyDarkTheme(statusBar);
                break;
            
            case Theme.Blue:
                ApplyBlueProfessionalTheme(statusBar);
                break;
            
            case Theme.Green:
                ApplyGreenNaturalTheme(statusBar);
                break;
        }
        
        statusBar.Refresh();
    }
    
    // Theme methods here (as shown in examples above)
    private void ApplyModernLightTheme(StatusBarAdv sb) { /* ... */ }
    private void ApplyDarkTheme(StatusBarAdv sb) { /* ... */ }
    private void ApplyBlueProfessionalTheme(StatusBarAdv sb) { /* ... */ }
    private void ApplyGreenNaturalTheme(StatusBarAdv sb) { /* ... */ }
}
```

## Next Steps

After customizing appearance:

1. **Configure Borders and Themes** → Read: [borders-and-themes.md](borders-and-themes.md)
   - Apply Office2016 themes
   - Set border styles
   - Enable themed backgrounds

2. **Handle Events** → Read: [events-and-behavior.md](events-and-behavior.md)
   - Respond to appearance changes
   - Handle gradient and color events
