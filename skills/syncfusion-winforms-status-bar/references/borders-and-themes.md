# Borders and Themes

This guide covers border configuration, Office2016 themes, Metro style, and themed background settings for StatusBarAdv.

## Table of Contents

- [Border Styles](#border-styles)
- [2D Border Configuration](#2d-border-configuration)
- [3D Border Styles](#3d-border-styles)
- [Office2016 Themes](#office2016-themes)
- [Metro Style](#metro-style)
- [Themed Backgrounds](#themed-backgrounds)
- [Complete Theme Examples](#complete-theme-examples)

## When to Read This

Read this reference when:
- Configuring status bar borders (2D or 3D)
- Applying Office2016 themes (Colorful, White, Black, DarkGray)
- Using Metro style for flat design
- Enabling themed backgrounds
- Matching status bar appearance to application theme
- Creating professional-looking borders

## Border Styles

The **BorderStyle** property determines the border type.

### BorderStyle Options

**BorderStyle.None:**
```csharp
statusBarAdv1.BorderStyle = BorderStyle.None;
```
No border (flat, seamless appearance).

**BorderStyle.FixedSingle:**
```csharp
statusBarAdv1.BorderStyle = BorderStyle.FixedSingle;
```
Single-line 2D border. Use with **BorderColor** and **BorderSingle** for customization.

**BorderStyle.Fixed3D:**
```csharp
statusBarAdv1.BorderStyle = BorderStyle.Fixed3D;
```
3D border effect. Use with **Border3DStyle** for different 3D styles.

### BorderStyle Examples

**C#:**
```csharp
// No border (flat)
statusBarAdv1.BorderStyle = BorderStyle.None;

// Simple single-line border
statusBarAdv1.BorderStyle = BorderStyle.FixedSingle;
statusBarAdv1.BorderColor = Color.Gray;

// 3D border
statusBarAdv1.BorderStyle = BorderStyle.Fixed3D;
statusBarAdv1.Border3DStyle = Border3DStyle.Sunken;
```

**VB.NET:**
```vbnet
' No border (flat)
statusBarAdv1.BorderStyle = BorderStyle.None

' Simple single-line border
statusBarAdv1.BorderStyle = BorderStyle.FixedSingle
statusBarAdv1.BorderColor = Color.Gray

' 3D border
statusBarAdv1.BorderStyle = BorderStyle.Fixed3D
statusBarAdv1.Border3DStyle = Border3DStyle.Sunken
```

## 2D Border Configuration

When **BorderStyle** is set to **FixedSingle**, configure the 2D border appearance.

### BorderColor Property

**C#:**
```csharp
// Set border style to FixedSingle first
statusBarAdv1.BorderStyle = BorderStyle.FixedSingle;

// Set border color
statusBarAdv1.BorderColor = Color.DarkSlateGray;

// Or custom RGB color
statusBarAdv1.BorderColor = Color.FromArgb(100, 120, 140);
```

**VB.NET:**
```vbnet
' Set border style to FixedSingle first
statusBarAdv1.BorderStyle = BorderStyle.FixedSingle

' Set border color
statusBarAdv1.BorderColor = Color.DarkSlateGray

' Or custom RGB color
statusBarAdv1.BorderColor = Color.FromArgb(100, 120, 140)
```

### BorderSingle Property

The **BorderSingle** property controls the 2D border line style.

**Options:**
- `ButtonBorderStyle.Solid` - Solid line (default)
- `ButtonBorderStyle.Dashed` - Dashed line
- `ButtonBorderStyle.Dotted` - Dotted line
- `ButtonBorderStyle.Inset` - Inset effect
- `ButtonBorderStyle.Outset` - Outset effect
- `ButtonBorderStyle.None` - No border line

**C#:**
```csharp
statusBarAdv1.BorderStyle = BorderStyle.FixedSingle;

// Solid border
statusBarAdv1.BorderSingle = ButtonBorderStyle.Solid;

// Dashed border
statusBarAdv1.BorderSingle = ButtonBorderStyle.Dashed;

// Dotted border
statusBarAdv1.BorderSingle = ButtonBorderStyle.Dotted;
```

### BorderSides Property

Control which sides of the status bar have borders.

**C#:**
```csharp
// All sides (default)
statusBarAdv1.BorderSides = Border3DSide.All;

// Top only
statusBarAdv1.BorderSides = Border3DSide.Top;

// Top and bottom
statusBarAdv1.BorderSides = Border3DSide.Top | Border3DSide.Bottom;

// Left and right
statusBarAdv1.BorderSides = Border3DSide.Left | Border3DSide.Right;

// Three sides (no bottom)
statusBarAdv1.BorderSides = Border3DSide.Top | Border3DSide.Left | Border3DSide.Right;
```

**VB.NET:**
```vbnet
' All sides (default)
statusBarAdv1.BorderSides = Border3DSide.All

' Top only
statusBarAdv1.BorderSides = Border3DSide.Top

' Top and bottom
statusBarAdv1.BorderSides = Border3DSide.Top Or Border3DSide.Bottom

' Left and right
statusBarAdv1.BorderSides = Border3DSide.Left Or Border3DSide.Right

' Three sides (no bottom)
statusBarAdv1.BorderSides = Border3DSide.Top Or Border3DSide.Left Or Border3DSide.Right
```

### Complete 2D Border Example

**C#:**
```csharp
public void Configure2DBorder()
{
    // Enable 2D border
    statusBarAdv1.BorderStyle = BorderStyle.FixedSingle;
    
    // Dark red dashed border
    statusBarAdv1.BorderColor = Color.DarkRed;
    statusBarAdv1.BorderSingle = ButtonBorderStyle.Dashed;
    
    // All sides
    statusBarAdv1.BorderSides = Border3DSide.All;
}
```

## 3D Border Styles

When **BorderStyle** is set to **Fixed3D**, use **Border3DStyle** for different 3D effects.

### Border3DStyle Options

**10 Available Styles:**

1. **Border3DStyle.RaisedOuter** - Raised outer edge
2. **Border3DStyle.SunkenOuter** - Sunken outer edge
3. **Border3DStyle.RaisedInner** - Raised inner edge
4. **Border3DStyle.SunkenInner** - Sunken inner edge
5. **Border3DStyle.Raised** - Full raised effect
6. **Border3DStyle.Sunken** - Full sunken effect (default)
7. **Border3DStyle.Etched** - Etched effect
8. **Border3DStyle.Bump** - Bump effect
9. **Border3DStyle.Adjust** - Adjusted border
10. **Border3DStyle.Flat** - Flat 3D style

### 3D Border Examples

**C#:**
```csharp
// Set to 3D border mode
statusBarAdv1.BorderStyle = BorderStyle.Fixed3D;

// Raised effect
statusBarAdv1.Border3DStyle = Border3DStyle.Raised;

// Sunken effect (most common for status bars)
statusBarAdv1.Border3DStyle = Border3DStyle.Sunken;

// Etched effect
statusBarAdv1.Border3DStyle = Border3DStyle.Etched;

// Bump effect
statusBarAdv1.Border3DStyle = Border3DStyle.Bump;

// Flat 3D
statusBarAdv1.Border3DStyle = Border3DStyle.Flat;
```

**VB.NET:**
```vbnet
' Set to 3D border mode
statusBarAdv1.BorderStyle = BorderStyle.Fixed3D

' Raised effect
statusBarAdv1.Border3DStyle = Border3DStyle.Raised

' Sunken effect (most common for status bars)
statusBarAdv1.Border3DStyle = Border3DStyle.Sunken

' Etched effect
statusBarAdv1.Border3DStyle = Border3DStyle.Etched
```

### Complete 3D Border Example

**C#:**
```csharp
public void Configure3DBorder()
{
    // Enable 3D border
    statusBarAdv1.BorderStyle = BorderStyle.Fixed3D;
    
    // Sunken style (classic status bar look)
    statusBarAdv1.Border3DStyle = Border3DStyle.Sunken;
    
    // Apply to all sides
    statusBarAdv1.BorderSides = Border3DSide.All;
}
```

## Office2016 Themes

The **Style** property applies Office2016 theme presets.

### Available Office2016 Themes

**StatusbarStyle.Office2016Colorful:**
```csharp
statusBarAdv1.Style = StatusbarStyle.Office2016Colorful;
```
Colorful Office2016 theme with blue accent.

**StatusbarStyle.Office2016White:**
```csharp
statusBarAdv1.Style = StatusbarStyle.Office2016White;
```
White/light Office2016 theme.

**StatusbarStyle.Office2016Black:**
```csharp
statusBarAdv1.Style = StatusbarStyle.Office2016Black;
```
Black/dark Office2016 theme.

**StatusbarStyle.Office2016DarkGray:**
```csharp
statusBarAdv1.Style = StatusbarStyle.Office2016DarkGray;
```
Dark gray Office2016 theme.

### Office2016 Theme Examples

**C#:**
```csharp
// Office2016 Colorful (blue accent, white background)
statusBarAdv1.Style = StatusbarStyle.Office2016Colorful;

// Office2016 White (clean, bright)
statusBarAdv1.Style = StatusbarStyle.Office2016White;

// Office2016 Black (high contrast)
statusBarAdv1.Style = StatusbarStyle.Office2016Black;

// Office2016 Dark Gray (dark, professional)
statusBarAdv1.Style = StatusbarStyle.Office2016DarkGray;
```

**VB.NET:**
```vbnet
' Office2016 Colorful (blue accent, white background)
statusBarAdv1.Style = StatusbarStyle.Office2016Colorful

' Office2016 White (clean, bright)
statusBarAdv1.Style = StatusbarStyle.Office2016White

' Office2016 Black (high contrast)
statusBarAdv1.Style = StatusbarStyle.Office2016Black

' Office2016 Dark Gray (dark, professional)
statusBarAdv1.Style = StatusbarStyle.Office2016DarkGray
```

### Office2016 Theme Application

**C#:**
```csharp
public void ApplyOffice2016Theme(StatusbarStyle theme)
{
    // Apply theme
    statusBarAdv1.Style = theme;
    
    // Themes automatically configure:
    // - Background colors
    // - Border styles
    // - Text colors
    // - Overall appearance
    
    // You can still customize after applying theme
    statusBarAdv1.SizingGrip = true;
}
```

## Metro Style

The **Metro** style provides a flat, modern appearance.

### Applying Metro Style

**C#:**
```csharp
statusBarAdv1.Style = StatusbarStyle.Metro;
```

**VB.NET:**
```vbnet
statusBarAdv1.Style = StatusbarStyle.Metro
```

### Metro Style Customization

**C#:**
```csharp
public void ConfigureMetroStyle()
{
    // Apply Metro style
    statusBarAdv1.Style = StatusbarStyle.Metro;
    
    // Set Metro accent color
    statusBarAdv1.MetroColor = Color.FromArgb(0, 120, 215);
    
    // Use Metro color as border
    statusBarAdv1.UseMetroColorAsBorder = true;
    
    // Flat background
    statusBarAdv1.BackColor = Color.FromArgb(245, 245, 245);
    
    // No sizing grip for ultra-flat look
    statusBarAdv1.SizingGrip = false;
}
```

### Metro vs Default Style

**C#:**
```csharp
// Default style
statusBarAdv1.Style = StatusbarStyle.Default;
// - Traditional appearance
// - 3D effects available
// - Classic Windows look

// Metro style
statusBarAdv1.Style = StatusbarStyle.Metro;
// - Flat, modern appearance
// - Minimal borders
// - Clean, contemporary look
```

## Themed Backgrounds

Configure how StatusBarAdv responds to system themes.

### ThemesEnabled Property

**C#:**
```csharp
// Enable themed background
statusBarAdv1.ThemesEnabled = true;

// Disable themed background
statusBarAdv1.ThemesEnabled = false;
```

**VB.NET:**
```vbnet
' Enable themed background
statusBarAdv1.ThemesEnabled = True

' Disable themed background
statusBarAdv1.ThemesEnabled = False
```

**Note:** When enabled, StatusBarAdv draws a themed background matching the system theme. **BorderStyle** should be set to **None** for best results.

### IgnoreThemeBackground Property

**C#:**
```csharp
// Use theme background
statusBarAdv1.IgnoreThemeBackground = false;

// Ignore theme, use custom BackColor
statusBarAdv1.IgnoreThemeBackground = true;
statusBarAdv1.BackColor = Color.LightSteelBlue;
```

**VB.NET:**
```vbnet
' Use theme background
statusBarAdv1.IgnoreThemeBackground = False

' Ignore theme, use custom BackColor
statusBarAdv1.IgnoreThemeBackground = True
statusBarAdv1.BackColor = Color.LightSteelBlue
```

### Themed Background Example

**C#:**
```csharp
public void ConfigureThemedBackground()
{
    // Enable theming
    statusBarAdv1.ThemesEnabled = true;
    
    // Remove border for seamless theme integration
    statusBarAdv1.BorderStyle = BorderStyle.None;
    
    // Don't ignore theme background
    statusBarAdv1.IgnoreThemeBackground = false;
    
    // StatusBarAdv will match system theme
}
```

## Complete Theme Examples

### Office2016 Colorful Theme

**C#:**
```csharp
public void ApplyOffice2016ColorfulTheme()
{
    statusBarAdv1.Style = StatusbarStyle.Office2016Colorful;
    statusBarAdv1.SizingGrip = true;
    
    // Theme automatically sets:
    // - White/light blue background
    // - Blue accents
    // - Professional appearance
}
```

### Office2016 Dark Theme

**C#:**
```csharp
public void ApplyOffice2016DarkTheme()
{
    statusBarAdv1.Style = StatusbarStyle.Office2016Black;
    statusBarAdv1.ForeColor = Color.White;
    statusBarAdv1.SizingGrip = true;
    
    // Theme automatically sets:
    // - Dark background
    // - Light text (enhanced with ForeColor)
    // - High contrast
}
```

### Metro Flat Theme

**C#:**
```csharp
public void ApplyMetroFlatTheme()
{
    statusBarAdv1.Style = StatusbarStyle.Metro;
    statusBarAdv1.BackColor = Color.White;
    statusBarAdv1.BorderStyle = BorderStyle.None;
    statusBarAdv1.ForeColor = Color.FromArgb(60, 60, 60);
    
    // Metro accent
    statusBarAdv1.MetroColor = Color.FromArgb(0, 120, 215);
    statusBarAdv1.UseMetroColorAsBorder = false;
    
    // No grip for flat design
    statusBarAdv1.SizingGrip = false;
}
```

### Custom Bordered Theme

**C#:**
```csharp
public void ApplyCustomBorderedTheme()
{
    // Custom gradient background
    statusBarAdv1.BackgroundColor = new BrushInfo(
        GradientStyle.Vertical,
        Color.FromArgb(240, 248, 255),
        Color.FromArgb(220, 235, 250)
    );
    
    // 2D border with custom color
    statusBarAdv1.BorderStyle = BorderStyle.FixedSingle;
    statusBarAdv1.BorderColor = Color.FromArgb(160, 190, 220);
    statusBarAdv1.BorderSingle = ButtonBorderStyle.Solid;
    statusBarAdv1.BorderSides = Border3DSide.Top | Border3DSide.Left | Border3DSide.Right;
    
    // Text color
    statusBarAdv1.ForeColor = Color.FromArgb(30, 57, 91);
    
    // Sizing grip
    statusBarAdv1.SizingGrip = true;
}
```

### Professional 3D Theme

**C#:**
```csharp
public void ApplyProfessional3DTheme()
{
    // Light gradient
    statusBarAdv1.BackgroundColor = new BrushInfo(
        GradientStyle.Horizontal,
        Color.FromArgb(235, 240, 245),
        Color.FromArgb(215, 225, 235)
    );
    
    // 3D sunken border (classic look)
    statusBarAdv1.BorderStyle = BorderStyle.Fixed3D;
    statusBarAdv1.Border3DStyle = Border3DStyle.Sunken;
    statusBarAdv1.BorderSides = Border3DSide.All;
    
    // Standard text
    statusBarAdv1.ForeColor = Color.Black;
    
    // Sizing grip
    statusBarAdv1.SizingGrip = true;
}
```

### Complete Theme Switcher Example

**C#:**
```csharp
public partial class ThemeSwitcherForm : Form
{
    private StatusBarAdv statusBar;
    
    public enum AppTheme
    {
        Office2016Colorful,
        Office2016White,
        Office2016Black,
        Office2016DarkGray,
        Metro,
        Custom
    }
    
    public ThemeSwitcherForm()
    {
        InitializeComponent();
        SetupStatusBar();
        ApplyTheme(AppTheme.Office2016Colorful);
    }
    
    private void SetupStatusBar()
    {
        statusBar = new StatusBarAdv
        {
            Dock = DockStyle.Bottom,
            Height = 28
        };
        
        // Add sample panels
        statusBar.Controls.Add(new StatusBarAdvPanel
        {
            Text = "Ready",
            Size = new Size(100, 25)
        });
        
        statusBar.Controls.Add(new StatusBarAdvPanel
        {
            PanelType = StatusBarAdvPanelType.ShortTime,
            Size = new Size(80, 25)
        });
        
        this.Controls.Add(statusBar);
    }
    
    public void ApplyTheme(AppTheme theme)
    {
        switch (theme)
        {
            case AppTheme.Office2016Colorful:
                statusBar.Style = StatusbarStyle.Office2016Colorful;
                statusBar.SizingGrip = true;
                break;
            
            case AppTheme.Office2016White:
                statusBar.Style = StatusbarStyle.Office2016White;
                statusBar.SizingGrip = true;
                break;
            
            case AppTheme.Office2016Black:
                statusBar.Style = StatusbarStyle.Office2016Black;
                statusBar.ForeColor = Color.White;
                statusBar.SizingGrip = true;
                break;
            
            case AppTheme.Office2016DarkGray:
                statusBar.Style = StatusbarStyle.Office2016DarkGray;
                statusBar.ForeColor = Color.LightGray;
                statusBar.SizingGrip = true;
                break;
            
            case AppTheme.Metro:
                statusBar.Style = StatusbarStyle.Metro;
                statusBar.BackColor = Color.White;
                statusBar.BorderStyle = BorderStyle.None;
                statusBar.MetroColor = Color.FromArgb(0, 120, 215);
                statusBar.SizingGrip = false;
                break;
            
            case AppTheme.Custom:
                ApplyCustomBorderedTheme();
                break;
        }
        
        statusBar.Refresh();
    }
    
    private void ApplyCustomBorderedTheme()
    {
        statusBar.BackgroundColor = new BrushInfo(
            GradientStyle.Vertical,
            Color.FromArgb(240, 248, 255),
            Color.FromArgb(220, 235, 250)
        );
        
        statusBar.BorderStyle = BorderStyle.FixedSingle;
        statusBar.BorderColor = Color.FromArgb(160, 190, 220);
        statusBar.ForeColor = Color.FromArgb(30, 57, 91);
        statusBar.SizingGrip = true;
    }
}
```

## Next Steps

After configuring borders and themes:

1. **Handle Events** → Read: [events-and-behavior.md](events-and-behavior.md)
   - Respond to border changes
   - Handle theme change events
   - Configure auto-sizing behavior
