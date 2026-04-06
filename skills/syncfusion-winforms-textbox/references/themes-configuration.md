# Themes Configuration

## Table of Contents
- [Overview](#overview)
- [ThemesEnabled Property](#themesenabled-property)
- [ThemeName Property](#themename-property)
- [Theme Assembly Requirements](#theme-assembly-requirements)
- [Loading Theme Assemblies](#loading-theme-assemblies)
- [Office2016 Themes](#office2016-themes)
- [Office2019 Themes](#office2019-themes)
- [HighContrast Themes](#highcontrast-themes)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)

## Overview

TextBoxExt supports advanced theming through two mechanisms:
1. **ThemesEnabled** - XP Themes for Fixed3D borders
2. **ThemeName** - Runtime theme selection with theme assembly loading

This guide focuses on the `ThemeName` property and theme assembly integration for application-wide professional styling.

## ThemesEnabled Property

The `ThemesEnabled` property enables Windows XP themes when `BorderStyle` is set to `Fixed3D`.

```csharp
// Enable XP themes for 3D borders
textBoxExt1.BorderStyle = System.Windows.Forms.BorderStyle.Fixed3D;
textBoxExt1.ThemesEnabled = true;
```

![Enabled the themes when the border style is set to Fixed3D in WF TextBoxExt](../../../../../docs/Applying-Themes_images/Applying-Themes_img1.png)

**Note:** This property only affects Fixed3D borders and integrates with Windows XP/Vista/7 visual styles.

## ThemeName Property

The `ThemeName` property allows runtime theme selection from loaded theme assemblies.

**Supported Themes:**
- Office2016Colorful
- Office2016White
- Office2016Black
- Office2016DarkGray
- Office2019Colorful
- HighContrastBlack

```csharp
// Set theme by name
textBoxExt1.ThemeName = "Office2019Colorful";
```

![Set the Office2019theme to WF TextBoxExt](../../../../../docs/Applying-Themes_images/wf-textboxext-theme.png)

**Important:** Theme assemblies must be loaded before setting `ThemeName`.

## Theme Assembly Requirements

Different themes require specific assemblies to be referenced and loaded.

### Assembly Mapping

| Assembly | Themes Provided |
|----------|----------------|
| `Syncfusion.Office2016Theme.WinForms` | Office2016Colorful, Office2016White, Office2016DarkGray, Office2016Black |
| `Syncfusion.Office2019Theme.WinForms` | Office2019Colorful |
| `Syncfusion.HighContrastTheme.WinForms` | HighContrastBlack |

### Adding Assembly References

**Via NuGet Package Manager:**

```powershell
# Office2016 themes
Install-Package Syncfusion.Office2016Theme.WinForms

# Office2019 themes
Install-Package Syncfusion.Office2019Theme.WinForms

# HighContrast themes
Install-Package Syncfusion.HighContrastTheme.WinForms
```

**Via Project References:**

1. Right-click project → Add → Reference
2. Browse to Syncfusion installation directory
3. Select required theme DLLs
4. Click OK

## Loading Theme Assemblies

Theme assemblies must be loaded at application startup using `SkinManager.LoadAssembly()`.

### Basic Theme Loading

Load theme assemblies in `Program.Main()` before running the form:

```csharp
using Syncfusion.Windows.Forms;
using System;
using System.Windows.Forms;

static class Program
{
    [STAThread]
    static void Main()
    {
        // Load theme assemblies
        SkinManager.LoadAssembly(typeof(Syncfusion.WinForms.Themes.Office2016Theme).Assembly);
        SkinManager.LoadAssembly(typeof(Syncfusion.WinForms.Themes.Office2019Theme).Assembly);
        SkinManager.LoadAssembly(typeof(Syncfusion.HighContrastTheme.WinForms.HighContrastTheme).Assembly);

        Application.EnableVisualStyles();
        Application.SetCompatibleTextRenderingDefault(false);
        Application.Run(new Form1());
    }
}
```

**Important:** 
- Call `SkinManager.LoadAssembly()` BEFORE `Application.EnableVisualStyles()`
- Load assemblies only once at application startup
- Loading is a one-time operation per application session

## Office2016 Themes

The Office2016 theme family provides four variations with modern flat design.

### Office2016Colorful

Vibrant, colorful theme matching Office 2016's default appearance.

```csharp
// Load assembly in Program.Main()
SkinManager.LoadAssembly(typeof(Syncfusion.WinForms.Themes.Office2016Theme).Assembly);

// Apply in form
textBoxExt1.ThemeName = "Office2016Colorful";
```

**Characteristics:**
- Bright accent colors
- Modern flat design
- High energy appearance

### Office2016White

Clean white theme with subtle accents.

```csharp
textBoxExt1.ThemeName = "Office2016White";
```

**Characteristics:**
- White backgrounds
- Light gray borders
- Minimal, clean design
- Professional appearance

### Office2016DarkGray

Dark theme with reduced contrast for comfortable viewing.

```csharp
textBoxExt1.ThemeName = "Office2016DarkGray";
```

**Characteristics:**
- Dark gray backgrounds
- Softer contrast than Black theme
- Comfortable for extended use
- Reduced eye strain

### Office2016Black

High-contrast dark theme for dark mode applications.

```csharp
textBoxExt1.ThemeName = "Office2016Black";
```

**Characteristics:**
- Very dark backgrounds
- High contrast with light text
- True dark mode experience
- Professional dark appearance

## Office2019 Themes

The Office2019 theme provides the latest Microsoft Office styling.

### Office2019Colorful

Modern, refined colors matching Office 2019.

**Loading:**
```csharp
// In Program.Main()
SkinManager.LoadAssembly(typeof(Syncfusion.WinForms.Themes.Office2019Theme).Assembly);
```

**Applying:**
```csharp
textBoxExt1.ThemeName = "Office2019Colorful";
```

**Characteristics:**
- Latest Office design language
- Refined color palette
- Contemporary appearance
- Evolved from Office2016

## HighContrast Themes

Accessibility-focused themes for users with visual impairments.

### HighContrastBlack

High contrast theme compliant with accessibility standards.

**Loading:**
```csharp
// In Program.Main()
SkinManager.LoadAssembly(typeof(Syncfusion.HighContrastTheme.WinForms.HighContrastTheme).Assembly);
```

**Applying:**
```csharp
textBoxExt1.ThemeName = "HighContrastBlack";
```

**Characteristics:**
- Very high contrast
- Accessibility compliant
- Clear visual distinction
- Supports WCAG guidelines

**Use Cases:**
- Accessibility requirements
- Visual impairment support
- High ambient light environments
- Government/healthcare applications

## Complete Examples

### Example 1: Application with Theme Loading

**Program.cs:**
```csharp
using Syncfusion.Windows.Forms;
using System;
using System.Windows.Forms;

namespace TextBoxExtTheming
{
    static class Program
    {
        [STAThread]
        static void Main()
        {
            // Load all theme assemblies
            LoadThemes();
            
            Application.EnableVisualStyles();
            Application.SetCompatibleTextRenderingDefault(false);
            Application.Run(new MainForm());
        }
        
        private static void LoadThemes()
        {
            try
            {
                // Office2016 themes
                SkinManager.LoadAssembly(
                    typeof(Syncfusion.WinForms.Themes.Office2016Theme).Assembly);
                
                // Office2019 themes
                SkinManager.LoadAssembly(
                    typeof(Syncfusion.WinForms.Themes.Office2019Theme).Assembly);
                
                // HighContrast themes
                SkinManager.LoadAssembly(
                    typeof(Syncfusion.HighContrastTheme.WinForms.HighContrastTheme).Assembly);
            }
            catch (Exception ex)
            {
                MessageBox.Show($"Error loading themes: {ex.Message}", 
                    "Theme Loading Error", 
                    MessageBoxButtons.OK, 
                    MessageBoxIcon.Warning);
            }
        }
    }
}
```

**MainForm.cs:**
```csharp
using Syncfusion.Windows.Forms.Tools;
using System;
using System.Windows.Forms;

public partial class MainForm : Form
{
    private TextBoxExt textBoxExt1;
    
    public MainForm()
    {
        InitializeComponent();
        CreateThemedTextBox();
    }
    
    private void CreateThemedTextBox()
    {
        textBoxExt1 = new TextBoxExt();
        textBoxExt1.Location = new System.Drawing.Point(20, 20);
        textBoxExt1.Size = new System.Drawing.Size(300, 25);
        textBoxExt1.Text = "Themed TextBox";
        
        // Apply Office2019 theme
        textBoxExt1.ThemeName = "Office2019Colorful";
        
        this.Controls.Add(textBoxExt1);
    }
}
```

### Example 2: Application-Wide Theme Configuration

```csharp
using Syncfusion.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;
using System;
using System.Configuration;
using System.Windows.Forms;

public static class AppThemeManager
{
    private static string currentTheme = "Office2016Colorful";
    
    public static string CurrentTheme
    {
        get { return currentTheme; }
        set
        {
            currentTheme = value;
            SaveThemePreference(value);
        }
    }
    
    public static void Initialize()
    {
        // Load saved theme preference
        currentTheme = LoadThemePreference();
    }
    
    public static void ApplyToForm(Form form)
    {
        foreach (Control control in GetAllControls(form))
        {
            if (control is TextBoxExt textBox)
            {
                textBox.ThemeName = currentTheme;
            }
        }
    }
    
    private static System.Collections.Generic.IEnumerable<Control> GetAllControls(Control container)
    {
        var controls = new System.Collections.Generic.List<Control>();
        
        foreach (Control control in container.Controls)
        {
            controls.Add(control);
            controls.AddRange(GetAllControls(control));
        }
        
        return controls;
    }
    
    private static string LoadThemePreference()
    {
        // Load from config or registry
        try
        {
            return ConfigurationManager.AppSettings["Theme"] ?? "Office2016Colorful";
        }
        catch
        {
            return "Office2016Colorful";
        }
    }
    
    private static void SaveThemePreference(string theme)
    {
        // Save to config or registry
        try
        {
            var config = ConfigurationManager.OpenExeConfiguration(ConfigurationUserLevel.None);
            config.AppSettings.Settings["Theme"].Value = theme;
            config.Save(ConfigurationSaveMode.Modified);
        }
        catch
        {
            // Handle save error
        }
    }
}

// Usage in Program.Main()
static class Program
{
    [STAThread]
    static void Main()
    {
        // Load themes
        SkinManager.LoadAssembly(typeof(Syncfusion.WinForms.Themes.Office2016Theme).Assembly);
        SkinManager.LoadAssembly(typeof(Syncfusion.WinForms.Themes.Office2019Theme).Assembly);
        SkinManager.LoadAssembly(typeof(Syncfusion.HighContrastTheme.WinForms.HighContrastTheme).Assembly);
        
        // Initialize theme manager
        AppThemeManager.Initialize();
        
        Application.EnableVisualStyles();
        Application.SetCompatibleTextRenderingDefault(false);
        Application.Run(new MainForm());
    }
}

// Usage in forms
public partial class MainForm : Form
{
    public MainForm()
    {
        InitializeComponent();
        
        // Apply application theme
        AppThemeManager.ApplyToForm(this);
    }
}
```

## Best Practices

1. **Load assemblies once** in `Program.Main()` before `Application.EnableVisualStyles()`
2. **Handle errors gracefully** when loading theme assemblies
3. **Apply consistent themes** across all controls for unified appearance
4. **Save user preferences** to remember theme choice between sessions
5. **Set ThemeName once** after control creation, not repeatedly

## Troubleshooting

- **Theme not applied**: Verify assembly is loaded in `Program.Main()` and theme name is correct (case-sensitive)
- **Assembly not found**: Install NuGet package and verify assembly is in output directory
- **Unexpected appearance**: Check for custom property overrides (BackColor, ForeColor)

## Summary

Advanced theming in TextBoxExt provides:

- **ThemesEnabled** for XP theme integration with Fixed3D borders
- **ThemeName** for runtime theme selection
- **SkinManager.LoadAssembly()** for loading theme assemblies at startup
- **Six theme options**: Office2016 (4 variants), Office2019Colorful, HighContrastBlack
- Application-wide theme consistency
- Runtime theme switching capability
- Accessibility support with HighContrast themes

These theming capabilities enable professional, consistent, and accessible user interfaces across your Windows Forms applications.
