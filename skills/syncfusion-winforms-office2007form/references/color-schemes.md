# Color Schemes in Office2007Form

## Table of Contents
- [Overview](#overview)
- [Available Color Schemes](#available-color-schemes)
- [Applying Color Schemes](#applying-color-schemes)
- [Managed Color Scheme](#managed-color-scheme)
- [Background Color Synchronization](#background-color-synchronization)
- [AeroTheme Configuration](#aerotheme-configuration)
- [Code Examples](#code-examples)
- [Best Practices](#best-practices)

## Overview

Office2007Form supports multiple Office-inspired color schemes that can be applied through the `ColorScheme` property. These themes provide consistent visual styling matching Microsoft Office 2007/2010 appearance.

## Available Color Schemes

The Office2007Form supports the following built-in color schemes:

### 1. Blue (Default)
Professional blue theme matching Office 2007's default appearance.

```csharp
this.ColorScheme = Office2007Theme.Blue;
```

### 2. Silver
Modern silver/gray theme for a sleek appearance.

```csharp
this.ColorScheme = Office2007Theme.Silver;
```

### 3. Black
High-contrast black theme for dramatic appearance.

```csharp
this.ColorScheme = Office2007Theme.Black;
```

### 4. Managed
Custom color scheme that allows you to specify your own accent color.

```csharp
this.ColorScheme = Office2007Theme.Managed;
Office2007Colors.ApplyManagedColors(this, Color.DarkMagenta);
```

## Applying Color Schemes

### Using the ColorScheme Property

The simplest way to apply a color scheme is by setting the `ColorScheme` property:

**C# Example:**
```csharp
// Apply Blue color scheme
this.ColorScheme = Office2007Theme.Blue;
```

**VB.NET Example:**
```vb
' Apply Blue color scheme
Me.ColorScheme = Office2007Theme.Blue
```

### Setting Color Scheme in Constructor

**C# Example:**
```csharp
using System;
using Syncfusion.Windows.Forms;

public partial class MainForm : Office2007Form
{
    public MainForm()
    {
        InitializeComponent();
        
        this.Text = "Blue Theme Application";
        this.ColorScheme = Office2007Theme.Blue;
    }
}
```

**VB.NET Example:**
```vb
Imports Syncfusion.Windows.Forms

Partial Public Class MainForm
    Inherits Office2007Form
    
    Public Sub New()
        InitializeComponent()
        
        Me.Text = "Blue Theme Application"
        Me.ColorScheme = Office2007Theme.Blue
    End Sub
End Class
```

## Managed Color Scheme

The Managed color scheme allows you to specify a custom accent color for the form, enabling brand-specific theming.

### Applying Managed Colors

Use the `ApplyManagedColors` method to set a custom color:

**C# Example:**
```csharp
// Set Managed color scheme
this.ColorScheme = Office2007Theme.Managed;

// Apply custom color
Office2007Colors.ApplyManagedColors(this, Color.DarkMagenta);
```

**VB.NET Example:**
```vb
' Set Managed color scheme
Me.ColorScheme = Office2007Theme.Managed

' Apply custom color
Office2007Colors.ApplyManagedColors(Me, Color.DarkMagenta)
```
### Custom Brand Colors

You can use any color for branding purposes:

```csharp
public partial class BrandedForm : Office2007Form
{
    public BrandedForm()
    {
        InitializeComponent();
        
        this.Text = "Company Branded Application";
        
        // Apply company brand color
        this.ColorScheme = Office2007Theme.Managed;
        Color companyBrandColor = Color.FromArgb(0, 120, 215); // Custom blue
        Office2007Colors.ApplyManagedColors(this, companyBrandColor);
    }
}
```

### Common Brand Color Examples

```csharp
// Microsoft Blue
Office2007Colors.ApplyManagedColors(this, Color.FromArgb(0, 120, 215));

// Dark Green
Office2007Colors.ApplyManagedColors(this, Color.DarkGreen);

// Purple
Office2007Colors.ApplyManagedColors(this, Color.Purple);

// Orange
Office2007Colors.ApplyManagedColors(this, Color.DarkOrange);

// Teal
Office2007Colors.ApplyManagedColors(this, Color.Teal);
```

## Background Color Synchronization

The form's background color can be synchronized with the applied color scheme using the `UseOffice2007SchemeBackColor` property.

### Enabling Background Color Sync

**C# Example:**
```csharp
// Enable background color synchronization
this.UseOffice2007SchemeBackColor = true;
```

**VB.NET Example:**
```vb
' Enable background color synchronization
Me.UseOffice2007SchemeBackColor = True
```

### Complete Example

```csharp
public partial class ThemedForm : Office2007Form
{
    public ThemedForm()
    {
        InitializeComponent();
        
        this.Text = "Themed Application";
        
        // Apply color scheme
        this.ColorScheme = Office2007Theme.Silver;
        
        // Synchronize background color
        this.UseOffice2007SchemeBackColor = true;
    }
}
```

### When to Use Background Sync

**Enable (`true`) when:**
- You want consistent visual appearance throughout the form
- Your form contains mostly Syncfusion controls that support theming
- You want minimal visual distraction from the Office theme

**Disable (`false`) when:**
- You need a custom background color
- Your form has custom-painted controls
- You want to maintain the default Windows Forms background

## AeroTheme Configuration

Office2007Form supports Windows Aero glass effects on Windows Vista and Windows 7. The `ApplyAeroTheme` property controls this behavior.

### Understanding AeroTheme

**When AeroTheme is enabled (default):**
- Form displays with glass effect on Windows Vista/7
- Color schemes cannot be applied (Aero takes precedence)
- Modern appearance on compatible OS versions

**When AeroTheme is disabled:**
- Color schemes work on all OS versions
- No glass effect
- Consistent appearance across Windows versions

### Disabling AeroTheme

To apply color schemes on Windows Vista/7, disable AeroTheme:

**C# Example:**
```csharp
// Disable Aero Theme to allow color schemes
this.ApplyAeroTheme = false;

// Now apply color scheme
this.ColorScheme = Office2007Theme.Silver;
```

**VB.NET Example:**
```vb
' Disable Aero Theme to allow color schemes
Me.ApplyAeroTheme = False

' Now apply color scheme
Me.ColorScheme = Office2007Theme.Silver
```

### Complete Example with AeroTheme Control

```csharp
public partial class CrossPlatformForm : Office2007Form
{
    public CrossPlatformForm()
    {
        InitializeComponent();
        
        this.Text = "Cross-Platform Themed Form";
        
        // Disable Aero to ensure color scheme works everywhere
        this.ApplyAeroTheme = false;
        
        // Apply color scheme
        this.ColorScheme = Office2007Theme.Black;
        this.UseOffice2007SchemeBackColor = true;
    }
}
```

### AeroTheme Best Practices

- **Disable for consistency** - If you need the same appearance across Windows versions
- **Enable for modern look** - If targeting Windows Vista/7 and want native Aero effects
- **Consider Windows 8+** - Aero is not available on Windows 8 and later, so disabling has no effect on those OS versions

## Code Examples

### Example 1: Simple Blue Theme

```csharp
public partial class BlueForm : Office2007Form
{
    public BlueForm()
    {
        InitializeComponent();
        this.Text = "Blue Themed Application";
        this.ColorScheme = Office2007Theme.Blue;
    }
}
```

### Example 2: Silver Theme with Background Sync

```csharp
public partial class SilverForm : Office2007Form
{
    public SilverForm()
    {
        InitializeComponent();
        this.Text = "Silver Themed Application";
        this.ColorScheme = Office2007Theme.Silver;
        this.UseOffice2007SchemeBackColor = true;
    }
}
```

### Example 3: Black Theme without Aero

```csharp
public partial class BlackForm : Office2007Form
{
    public BlackForm()
    {
        InitializeComponent();
        this.Text = "Black Themed Application";
        this.ApplyAeroTheme = false;
        this.ColorScheme = Office2007Theme.Black;
        this.UseOffice2007SchemeBackColor = true;
    }
}
```

### Example 4: Custom Brand Color

```csharp
public partial class BrandForm : Office2007Form
{
    public BrandForm()
    {
        InitializeComponent();
        this.Text = "Branded Application";
        
        // Apply custom brand color
        this.ColorScheme = Office2007Theme.Managed;
        Office2007Colors.ApplyManagedColors(this, Color.FromArgb(46, 117, 182));
        this.UseOffice2007SchemeBackColor = true;
    }
}
```

### Example 5: Dynamic Theme Switching

```csharp
public partial class DynamicThemeForm : Office2007Form
{
    public DynamicThemeForm()
    {
        InitializeComponent();
        this.Text = "Theme Switcher";
        
        // Add theme selection menu
        var menuStrip = new MenuStrip();
        var themeMenu = new ToolStripMenuItem("Themes");
        
        themeMenu.DropDownItems.Add("Blue", null, (s, e) => ApplyTheme(Office2007Theme.Blue));
        themeMenu.DropDownItems.Add("Silver", null, (s, e) => ApplyTheme(Office2007Theme.Silver));
        themeMenu.DropDownItems.Add("Black", null, (s, e) => ApplyTheme(Office2007Theme.Black));
        
        menuStrip.Items.Add(themeMenu);
        this.MainMenuStrip = menuStrip;
        this.Controls.Add(menuStrip);
        
        // Apply default theme
        ApplyTheme(Office2007Theme.Blue);
    }
    
    private void ApplyTheme(Office2007Theme theme)
    {
        this.ColorScheme = theme;
        this.UseOffice2007SchemeBackColor = true;
    }
}
```

## Best Practices

### 1. Choose Appropriate Theme for Context

- **Blue** - Default, safe choice for business applications
- **Silver** - Modern, neutral appearance for professional tools
- **Black** - High contrast for media or design applications
- **Managed** - Custom branding for company-specific applications

### 2. Maintain Consistency

Apply the same color scheme across all forms in your application for visual consistency:

```csharp
public class ThemeManager
{
    public static Office2007Theme CurrentTheme { get; set; } = Office2007Theme.Blue;
    
    public static void ApplyToForm(Office2007Form form)
    {
        form.ColorScheme = CurrentTheme;
        form.UseOffice2007SchemeBackColor = true;
    }
}

// Usage in any form
public class MyForm : Office2007Form
{
    public MyForm()
    {
        InitializeComponent();
        ThemeManager.ApplyToForm(this);
    }
}
```

### 3. Handle Aero Properly

For consistent cross-platform appearance:

```csharp
this.ApplyAeroTheme = false; // Disable Aero
this.ColorScheme = Office2007Theme.Silver; // Then apply theme
```

### 4. Test on Multiple OS Versions

- Test on Windows 7 (with Aero)
- Test on Windows 10/11 (without Aero)
- Verify color schemes appear correctly on all platforms

### 5. Consider Accessibility

- Black theme may be difficult to read for some users
- Test color combinations for sufficient contrast
- Provide theme switching options when possible

## Troubleshooting

### Color Scheme Not Applied

**Problem:** Setting `ColorScheme` property has no visible effect

**Cause:** AeroTheme is enabled on Windows Vista/7

**Solution:**
```csharp
this.ApplyAeroTheme = false; // Disable Aero first
this.ColorScheme = Office2007Theme.Silver; // Then apply scheme
```

### Managed Color Not Working

**Problem:** ApplyManagedColors doesn't change appearance

**Cause:** ColorScheme not set to Managed

**Solution:**
```csharp
this.ColorScheme = Office2007Theme.Managed; // Must set this first
Office2007Colors.ApplyManagedColors(this, Color.DarkMagenta); // Then apply color
```

### Background Color Mismatch

**Problem:** Form background doesn't match theme

**Cause:** UseOffice2007SchemeBackColor is false (default)

**Solution:**
```csharp
this.UseOffice2007SchemeBackColor = true; // Enable background sync
```

## Summary

Office2007Form provides flexible theming through built-in color schemes (Blue, Silver, Black) and custom managed colors. Control AeroTheme behavior for cross-platform consistency, and synchronize background colors for a cohesive appearance. Apply themes consistently across your application for professional, polished UI.
