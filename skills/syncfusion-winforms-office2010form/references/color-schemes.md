# Color Schemes in Office2010Form

## Table of Contents
- [Overview](#overview)
- [Built-in Color Schemes](#built-in-color-schemes)
- [Applying Color Schemes](#applying-color-schemes)
- [Managed Color Scheme](#managed-color-scheme)
- [Background Color Synchronization](#background-color-synchronization)
- [Aero Theme Support](#aero-theme-support)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

## Overview

Office2010Form supports multiple color schemes that match Microsoft Office 2010's visual themes. These schemes affect the caption bar, borders, and overall form appearance, providing a consistent Office 2010 look and feel.

**Available Color Schemes:**
- **Blue** - Default Office 2010 blue theme
- **Silver** - Office 2010 silver/gray theme
- **Black** - Office 2010 black theme
- **Managed** - Custom color scheme with user-defined colors

## Built-in Color Schemes

### Blue Color Scheme

The default Office 2010 blue theme with light blue caption bar and professional appearance.

**C# Implementation:**
```csharp
this.ColorScheme = Office2010Theme.Blue;
```

**VB.NET Implementation:**
```vb
Me.ColorScheme = Office2010Theme.Blue
```

**Appearance:**
- Light blue caption bar
- Blue accent colors
- Professional business appearance
- Matches Office 2010 default theme

### Silver Color Scheme

Office 2010 silver/gray theme with neutral appearance.

**C# Implementation:**
```csharp
this.ColorScheme = Office2010Theme.Silver;
```

**VB.NET Implementation:**
```vb
Me.ColorScheme = Office2010Theme.Silver
```

**Appearance:**
- Gray/silver caption bar
- Neutral color palette
- Modern, minimal appearance
- Matches Office 2010 silver theme

### Black Color Scheme

Office 2010 black theme with dark, high-contrast appearance.

**C# Implementation:**
```csharp
this.ColorScheme = Office2010Theme.Black;
```

**VB.NET Implementation:**
```vb
Me.ColorScheme = Office2010Theme.Black
```

**Appearance:**
- Dark black caption bar
- High-contrast design
- Professional dark theme
- Matches Office 2010 black theme

### Managed Color Scheme

Custom color scheme where you specify the theme color.

**C# Implementation:**
```csharp
// Set to Managed scheme
this.ColorScheme = Office2010Theme.Managed;

// Apply custom color
Office2010Colors.ApplyManagedColors(this, Color.DarkMagenta);
```

**VB.NET Implementation:**
```vb
' Set to Managed scheme
Me.ColorScheme = Office2010Theme.Managed

' Apply custom color
Office2010Colors.ApplyManagedColors(Me, Color.DarkMagenta)
```

**Appearance:**
- Caption bar uses specified color
- Dynamically generated theme based on provided color
- Allows brand-specific color schemes
- Full customization control

## Applying Color Schemes

### Basic Color Scheme Application

Apply a color scheme in the form constructor:

```csharp
public partial class MainForm : Office2010Form
{
    public MainForm()
    {
        InitializeComponent();
        
        // Apply Blue color scheme
        this.ColorScheme = Office2010Theme.Blue;
    }
}
```

### Runtime Color Scheme Change

Change color schemes dynamically at runtime:

```csharp
public void ChangeColorScheme(Office2010Theme theme)
{
    this.ColorScheme = theme;
    this.Refresh(); // Force redraw with new theme
}

// Usage examples
private void btnBlue_Click(object sender, EventArgs e)
{
    ChangeColorScheme(Office2010Theme.Blue);
}

private void btnSilver_Click(object sender, EventArgs e)
{
    ChangeColorScheme(Office2010Theme.Silver);
}

private void btnBlack_Click(object sender, EventArgs e)
{
    ChangeColorScheme(Office2010Theme.Black);
}
```

### Complete Theme Switcher Example

```csharp
public partial class ThemeSwitcherForm : Office2010Form
{
    public ThemeSwitcherForm()
    {
        InitializeComponent();
        
        this.Text = "Theme Switcher Demo";
        this.Size = new Size(600, 400);
        
        // Apply default theme
        ApplyTheme(Office2010Theme.Blue);
        
        // Create theme selection buttons
        CreateThemeButtons();
    }
    
    private void CreateThemeButtons()
    {
        int x = 20, y = 50;
        
        // Blue button
        Button btnBlue = CreateThemeButton("Blue", new Point(x, y));
        btnBlue.Click += (s, e) => ApplyTheme(Office2010Theme.Blue);
        this.Controls.Add(btnBlue);
        
        // Silver button
        Button btnSilver = CreateThemeButton("Silver", new Point(x + 110, y));
        btnSilver.Click += (s, e) => ApplyTheme(Office2010Theme.Silver);
        this.Controls.Add(btnSilver);
        
        // Black button
        Button btnBlack = CreateThemeButton("Black", new Point(x + 220, y));
        btnBlack.Click += (s, e) => ApplyTheme(Office2010Theme.Black);
        this.Controls.Add(btnBlack);
    }
    
    private Button CreateThemeButton(string text, Point location)
    {
        return new Button
        {
            Text = text,
            Location = location,
            Size = new Size(100, 40)
        };
    }
    
    private void ApplyTheme(Office2010Theme theme)
    {
        this.ColorScheme = theme;
        this.UseOffice2010SchemeBackColor = true;
    }
}
```

## Managed Color Scheme

### Custom Single Color Theme

Create a custom theme from a single color:

```csharp
public void ApplyCustomTheme(Color baseColor)
{
    // Set to Managed mode
    this.ColorScheme = Office2010Theme.Managed;
    
    // Apply custom color
    Office2010Colors.ApplyManagedColors(this, baseColor);
    
    // Sync background
    this.UseOffice2010SchemeBackColor = true;
}

// Usage examples
ApplyCustomTheme(Color.DarkMagenta);  // Purple theme
ApplyCustomTheme(Color.Teal);          // Teal theme
ApplyCustomTheme(Color.OrangeRed);     // Orange theme
ApplyCustomTheme(Color.ForestGreen);   // Green theme
```

### Brand Color Integration

Use company brand colors:

```csharp
public class BrandedForm : Office2010Form
{
    // Company brand colors
    private static readonly Color CompanyPrimary = Color.FromArgb(0, 120, 215);
    private static readonly Color CompanyAccent = Color.FromArgb(232, 17, 35);
    
    public BrandedForm()
    {
        InitializeComponent();
        
        // Apply brand color as theme
        this.ColorScheme = Office2010Theme.Managed;
        Office2010Colors.ApplyManagedColors(this, CompanyPrimary);
        
        this.UseOffice2010SchemeBackColor = true;
    }
    
    public void SwitchToAccentColor()
    {
        // Switch to accent brand color
        Office2010Colors.ApplyManagedColors(this, CompanyAccent);
    }
}
```

### Dynamic Color Picker Integration

Allow users to select custom colors:

```csharp
private void btnCustomColor_Click(object sender, EventArgs e)
{
    using (ColorDialog colorDialog = new ColorDialog())
    {
        if (colorDialog.ShowDialog() == DialogResult.OK)
        {
            // Apply user-selected color as theme
            this.ColorScheme = Office2010Theme.Managed;
            Office2010Colors.ApplyManagedColors(this, colorDialog.Color);
            this.UseOffice2010SchemeBackColor = true;
        }
    }
}
```

## Background Color Synchronization

### UseOffice2010SchemeBackColor Property

Synchronize the form's background color with the applied color scheme:

**Enable Background Sync:**
```csharp
this.UseOffice2010SchemeBackColor = true;
```

**Disable Background Sync:**
```csharp
this.UseOffice2010SchemeBackColor = false;
```

### Visual Impact

**With UseOffice2010SchemeBackColor = true:**
- Form background matches the color scheme
- Consistent appearance throughout the form
- Professional, cohesive look

**With UseOffice2010SchemeBackColor = false:**
- Form background uses default system color
- Only caption bar shows color scheme
- Mixed appearance (themed caption, standard background)

### Complete Example

```csharp
public partial class BackgroundSyncForm : Office2010Form
{
    public BackgroundSyncForm()
    {
        InitializeComponent();
        
        // Apply color scheme
        this.ColorScheme = Office2010Theme.Blue;
        
        // Sync background with scheme
        this.UseOffice2010SchemeBackColor = true;
        
        // Now entire form uses Blue theme colors
    }
}
```

## Aero Theme Support

### Understanding Aero Theme

Windows Aero provides a glass effect on Windows Vista and later. Office2010Form supports Aero, but there's a compatibility consideration with color schemes.

**Key Limitation:** Color schemes cannot be applied when Aero theme is enabled.

### Enabling Aero Theme

**C# Implementation:**
```csharp
// Enable Aero glass effect
this.ApplyAeroTheme = true;
```

**VB.NET Implementation:**
```vb
' Enable Aero glass effect
Me.ApplyAeroTheme = True
```

**Result:**
- Form displays with Windows Aero glass effect
- Transparent/glass caption bar borders
- ColorScheme property has no effect
- Windows Vista+ required for glass effect

### Disabling Aero to Apply Color Schemes

To use Office 2010 color schemes, disable Aero:

**C# Implementation:**
```csharp
// Disable Aero theme
this.ApplyAeroTheme = false;

// Now color schemes can be applied
this.ColorScheme = Office2010Theme.Blue;
```

**VB.NET Implementation:**
```vb
' Disable Aero theme
Me.ApplyAeroTheme = False

' Now color schemes can be applied
Me.ColorScheme = Office2010Theme.Blue
```

### Conditional Aero Application

Apply Aero only when color scheme is not needed:

```csharp
public void ConfigureAppearance(bool useColorScheme, Office2010Theme theme)
{
    if (useColorScheme)
    {
        // Disable Aero to allow color scheme
        this.ApplyAeroTheme = false;
        this.ColorScheme = theme;
        this.UseOffice2010SchemeBackColor = true;
    }
    else
    {
        // Enable Aero for glass effect
        this.ApplyAeroTheme = true;
        // Note: ColorScheme won't apply
    }
}
```

## Common Patterns

### Pattern 1: User Preference Storage

Save and restore user's theme preference:

```csharp
// Save theme preference
private void SaveThemePreference(Office2010Theme theme)
{
    Properties.Settings.Default.ColorScheme = theme.ToString();
    Properties.Settings.Default.Save();
}

// Load theme preference
private void LoadThemePreference()
{
    string themeName = Properties.Settings.Default.ColorScheme;
    
    if (Enum.TryParse(themeName, out Office2010Theme theme))
    {
        this.ColorScheme = theme;
        this.UseOffice2010SchemeBackColor = true;
    }
    else
    {
        // Default to Blue
        this.ColorScheme = Office2010Theme.Blue;
    }
}
```

### Pattern 2: Theme Menu System

Create a menu for theme selection:

```csharp
private void CreateThemeMenu()
{
    ToolStripMenuItem themeMenu = new ToolStripMenuItem("Themes");
    
    // Blue theme option
    ToolStripMenuItem blueItem = new ToolStripMenuItem("Blue");
    blueItem.Click += (s, e) => ApplyTheme(Office2010Theme.Blue);
    themeMenu.DropDownItems.Add(blueItem);
    
    // Silver theme option
    ToolStripMenuItem silverItem = new ToolStripMenuItem("Silver");
    silverItem.Click += (s, e) => ApplyTheme(Office2010Theme.Silver);
    themeMenu.DropDownItems.Add(silverItem);
    
    // Black theme option
    ToolStripMenuItem blackItem = new ToolStripMenuItem("Black");
    blackItem.Click += (s, e) => ApplyTheme(Office2010Theme.Black);
    themeMenu.DropDownItems.Add(blackItem);
    
    mainMenuStrip.Items.Add(themeMenu);
}

private void ApplyTheme(Office2010Theme theme)
{
    this.ColorScheme = theme;
    this.UseOffice2010SchemeBackColor = true;
    SaveThemePreference(theme);
}
```

## Troubleshooting

### Issue: Color Scheme Not Applied

**Problem:** Setting ColorScheme has no visible effect

**Solutions:**
1. Check `ApplyAeroTheme` is `false` (Aero prevents color schemes)
2. Call `this.Refresh()` after changing scheme
3. Verify `DisableOffice2010Style` is not set to `true`

### Issue: Background Color Doesn't Match Theme

**Problem:** Caption bar is themed but background is not

**Solution:** Enable background synchronization:
```csharp
this.UseOffice2010SchemeBackColor = true;
```

### Issue: Managed Color Not Working

**Problem:** ApplyManagedColors doesn't change appearance

**Solution:** Ensure ColorScheme is set to Managed first:
```csharp
this.ColorScheme = Office2010Theme.Managed;  // Set first
Office2010Colors.ApplyManagedColors(this, yourColor);  // Then apply
```

### Issue: Aero and Color Scheme Conflict

**Problem:** Want both Aero glass and color scheme

**Solution:** This is a platform limitation. Choose one:
- `ApplyAeroTheme = true` for glass effect (no color scheme)
- `ApplyAeroTheme = false` for color schemes (no glass effect)
