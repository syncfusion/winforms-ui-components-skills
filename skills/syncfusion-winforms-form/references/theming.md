# Theming

This guide covers applying built-in themes to SfForm for consistent, professional appearance across your Windows Forms application.

## Table of Contents
- [Overview](#overview)
- [Loading Theme Assemblies](#loading-theme-assemblies)
- [Applying Themes](#applying-themes)
- [Available Themes](#available-themes)
- [Theme Best Practices](#theme-best-practices)

## Overview

Syncfusion Windows Forms controls support six built-in professional themes that provide consistent styling across your entire application. These themes are designed to match modern design trends and ensure visual cohesion.

### Available Built-in Themes

1. **Office2016Colorful** - Modern, colorful Office theme
2. **Office2016White** - Clean white Office theme
3. **Office2016DarkGray** - Professional dark gray theme
4. **Office2016Black** - High-contrast black theme
5. **Office2019Colorful** - Latest Office-inspired theme
6. **HighContrastBlack** - Accessibility-focused high contrast

### Why Use Themes?

- **Consistency:** All Syncfusion controls share the same visual style
- **Professionalism:** Pre-designed themes look polished and modern
- **Productivity:** No need to manually style every control
- **Accessibility:** HighContrast theme supports users with visual impairments
- **Maintenance:** Easy to switch themes across entire application

## Loading Theme Assemblies

Before applying themes, you must load the required theme assemblies. This is typically done once in the `Program.cs` file before creating any forms.

### Required Theme Assemblies

| Theme | Assembly |
|-------|----------|
| Office2016Colorful, Office2016White, Office2016DarkGray, Office2016Black | `Syncfusion.Office2016Theme.WinForms.dll` |
| Office2019Colorful | `Syncfusion.Office2019Theme.WinForms.dll` |
| HighContrastBlack | `Syncfusion.HighContrastTheme.WinForms.dll` |

### Loading in Program.cs

**C#:**
```csharp
using System;
using System.Windows.Forms;
using Syncfusion.WinForms.Controls;

namespace MyApplication
{
    static class Program
    {
        [STAThread]
        static void Main()
        {
            // Register Syncfusion license (required)
            Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
            
            // Load theme assemblies
            SfSkinManager.LoadAssembly(typeof(Syncfusion.WinForms.Themes.Office2016Theme).Assembly);
            SfSkinManager.LoadAssembly(typeof(Syncfusion.WinForms.Themes.Office2019Theme).Assembly);
            SfSkinManager.LoadAssembly(typeof(Syncfusion.HighContrastTheme.WinForms.HighContrastTheme).Assembly);
            
            Application.EnableVisualStyles();
            Application.SetCompatibleTextRenderingDefault(false);
            Application.Run(new Form1());
        }
    }
}
```

**VB.NET:**
```vb
Imports System.Windows.Forms
Imports Syncfusion.WinForms.Controls

Module Program
    <STAThread>
    Sub Main()
        ' Register Syncfusion license (required)
        Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY")
        
        ' Load theme assemblies
        SfSkinManager.LoadAssembly(GetType(Syncfusion.WinForms.Themes.Office2016Theme).Assembly)
        SfSkinManager.LoadAssembly(GetType(Syncfusion.WinForms.Themes.Office2019Theme).Assembly)
        SfSkinManager.LoadAssembly(GetType(Syncfusion.HighContrastTheme.WinForms.HighContrastTheme).Assembly)
        
        Application.EnableVisualStyles()
        Application.SetCompatibleTextRenderingDefault(False)
        Application.Run(New Form1())
    End Sub
End Module
```

### Loading Specific Theme Assembly

If you only need specific themes, load only the required assemblies:

**C#:**
```csharp
// Load only Office2016 themes
SfSkinManager.LoadAssembly(typeof(Syncfusion.WinForms.Themes.Office2016Theme).Assembly);
```

### NuGet Package Installation

Install theme packages via NuGet:

```powershell
# Office 2016 themes
Install-Package Syncfusion.Office2016Theme.WinForms

# Office 2019 theme
Install-Package Syncfusion.Office2019Theme.WinForms

# High Contrast theme
Install-Package Syncfusion.HighContrastTheme.WinForms
```

### Important Notes

- Load theme assemblies **before** `Application.Run()`
- Load assemblies **once** at application startup
- Loading multiple theme assemblies doesn't affect performance
- Themes are shared across all forms in the application

## Applying Themes

Once theme assemblies are loaded, apply themes using the `ThemeName` property.

### Basic Theme Application

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Apply Office2016Colorful theme
    this.ThemeName = "Office2016Colorful";
}
```

**VB.NET:**
```vb
Public Sub New()
    InitializeComponent()
    
    ' Apply Office2016Colorful theme
    Me.ThemeName = "Office2016Colorful"
End Sub
```

### Applying Different Themes

**C#:**
```csharp
// Office2016White
this.ThemeName = "Office2016White";

// Office2016DarkGray
this.ThemeName = "Office2016DarkGray";

// Office2016Black
this.ThemeName = "Office2016Black";

// Office2019Colorful
this.ThemeName = "Office2019Colorful";

// HighContrastBlack
this.ThemeName = "HighContrastBlack";
```

### Dynamic Theme Switching

**C#:**
```csharp
private void SwitchTheme(string themeName)
{
    this.ThemeName = themeName;
    
    // Refresh form to apply theme
    this.Refresh();
}

// Usage with buttons or menu
private void OnOffice2016ColorfulClick(object sender, EventArgs e)
{
    SwitchTheme("Office2016Colorful");
}

private void OnOffice2016BlackClick(object sender, EventArgs e)
{
    SwitchTheme("Office2016Black");
}
```

### Theme Selection ComboBox

**C#:**
```csharp
private void InitializeThemeSelector()
{
    ComboBox themeComboBox = new ComboBox();
    themeComboBox.DropDownStyle = ComboBoxStyle.DropDownList;
    themeComboBox.Items.AddRange(new string[]
    {
        "Office2016Colorful",
        "Office2016White",
        "Office2016DarkGray",
        "Office2016Black",
        "Office2019Colorful",
        "HighContrastBlack"
    });
    
    themeComboBox.SelectedIndex = 0;
    themeComboBox.SelectedIndexChanged += (s, e) =>
    {
        this.ThemeName = themeComboBox.SelectedItem.ToString();
    };
    
    // Add to form or toolbar
    this.Controls.Add(themeComboBox);
}
```

## Available Themes

### Office2016Colorful

Modern, vibrant theme with colorful accents inspired by Microsoft Office 2016.

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    this.ThemeName = "Office2016Colorful";
}
```

**Characteristics:**
- Bright white backgrounds
- Colorful accent colors (blue, green, orange)
- Modern, clean interface
- Good for general-purpose applications

**Best For:**
- Business applications
- Productivity tools
- Data-heavy applications
- Modern, professional look

### Office2016White

Clean, minimalist white theme with subtle gray accents.

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    this.ThemeName = "Office2016White";
}
```

**Characteristics:**
- Pure white backgrounds
- Light gray borders and separators
- Minimal visual distractions
- Excellent readability

**Best For:**
- Document-centric applications
- Reading interfaces
- Minimalist designs
- Professional environments

### Office2016DarkGray

Professional dark gray theme that reduces eye strain.

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    this.ThemeName = "Office2016DarkGray";
}
```

**Characteristics:**
- Medium gray backgrounds
- White/light gray text
- Reduced contrast compared to black theme
- Comfortable for extended use

**Best For:**
- Development tools
- Design applications
- Long working sessions
- Low-light environments

### Office2016Black

High-contrast black theme for maximum focus and reduced eye strain.

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    this.ThemeName = "Office2016Black";
}
```

**Characteristics:**
- Dark black backgrounds
- Bright white text
- High contrast interface
- Modern, sophisticated look

**Best For:**
- Creative applications
- Video/photo editing
- Presentation mode
- High-contrast preference users

### Office2019Colorful

Latest Office-inspired theme with modern gradients and colors.

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    this.ThemeName = "Office2019Colorful";
}
```

**Characteristics:**
- Updated color palette
- Subtle gradients
- Modern Office 365 aesthetic
- Refined visual elements

**Best For:**
- Modern applications
- Office 365 integration
- Contemporary UI design
- Latest design trends

### HighContrastBlack

Accessibility-focused theme with maximum contrast for visually impaired users.

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    this.ThemeName = "HighContrastBlack";
}
```

**Characteristics:**
- Maximum contrast
- Large, clear text
- Distinct element boundaries
- Accessibility compliant

**Best For:**
- Accessibility requirements
- Visually impaired users
- High-contrast mode users
- Government/compliance applications

## Theme Best Practices

### 1. Consistent Application-Wide Theming

Apply the same theme to all forms in your application:

**C#:**
```csharp
// Create a base form class
public class ThemedForm : SfForm
{
    public ThemedForm()
    {
        // Apply consistent theme
        this.ThemeName = AppSettings.CurrentTheme;
    }
}

// Derive all forms from ThemedForm
public class MainForm : ThemedForm
{
    // Theme is automatically applied
}
```

### 2. User Theme Preference

Save and restore user's theme preference:

**C#:**
```csharp
// Save preference
private void SaveThemePreference(string themeName)
{
    Properties.Settings.Default.PreferredTheme = themeName;
    Properties.Settings.Default.Save();
}

// Load preference
private void LoadThemePreference()
{
    string savedTheme = Properties.Settings.Default.PreferredTheme;
    if (!string.IsNullOrEmpty(savedTheme))
    {
        this.ThemeName = savedTheme;
    }
}

// In Form constructor
public Form1()
{
    InitializeComponent();
    LoadThemePreference();
}
```

### 3. Theme-Aware Custom Controls

Ensure custom controls adapt to theme changes:

**C#:**
```csharp
protected override void OnThemeNameChanged(ThemeChangedEventArgs args)
{
    base.OnThemeNameChanged(args);
    
    // Update custom control colors based on theme
    UpdateCustomControlColors();
}
```

### 4. Test All Themes

Test your application with all available themes to ensure:
- Text is readable
- Colors have sufficient contrast
- Custom controls look appropriate
- Images/icons work with dark and light backgrounds

### 5. Provide Theme Selection

Give users the ability to choose their preferred theme:

**C#:**
```csharp
// Create Options/Preferences dialog with theme selector
public class PreferencesDialog : SfForm
{
    private ComboBox themeSelector;
    
    public PreferencesDialog()
    {
        InitializeComponent();
        
        // Setup theme selector
        themeSelector = new ComboBox();
        themeSelector.Items.AddRange(new string[]
        {
            "Office2016Colorful",
            "Office2016White",
            "Office2016DarkGray",
            "Office2016Black",
            "Office2019Colorful",
            "HighContrastBlack"
        });
        
        // Set current theme
        themeSelector.SelectedItem = this.ThemeName;
    }
}
```

### 6. Theme Loading Performance

Load theme assemblies early to avoid delays:

**C#:**
```csharp
// Load themes before showing splash screen
static void Main()
{
    // 1. Register license
    SyncfusionLicenseProvider.RegisterLicense("KEY");
    
    // 2. Load themes (fast, do early)
    SfSkinManager.LoadAssembly(typeof(Office2016Theme).Assembly);
    
    // 3. Show splash screen
    ShowSplashScreen();
    
    // 4. Load main application
    Application.Run(new MainForm());
}
```

### 7. Accessibility Compliance

Support high-contrast themes for accessibility:

**C#:**
```csharp
// Detect Windows high-contrast mode
private void CheckHighContrastMode()
{
    if (SystemInformation.HighContrast)
    {
        // User has high-contrast enabled in Windows
        this.ThemeName = "HighContrastBlack";
    }
}

// Call in Form_Load
private void Form1_Load(object sender, EventArgs e)
{
    CheckHighContrastMode();
}
```

### 8. Theme-Specific Images

Provide different image assets for light and dark themes:

**C#:**
```csharp
private void UpdateImagesForTheme(string themeName)
{
    bool isDarkTheme = themeName.Contains("Black") || themeName.Contains("DarkGray");
    
    if (isDarkTheme)
    {
        // Use light-colored icons for dark theme
        toolbarButton1.Image = Properties.Resources.Icon_Light;
    }
    else
    {
        // Use dark-colored icons for light theme
        toolbarButton1.Image = Properties.Resources.Icon_Dark;
    }
}

protected override void OnThemeNameChanged(ThemeChangedEventArgs args)
{
    base.OnThemeNameChanged(args);
    UpdateImagesForTheme(args.ThemeName);
}
```

## Complete Theming Example

**Program.cs:**
```csharp
using System;
using System.Windows.Forms;
using Syncfusion.WinForms.Controls;

namespace ThemedApplication
{
    static class Program
    {
        [STAThread]
        static void Main()
        {
            // License registration
            Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
            
            // Load all theme assemblies
            SfSkinManager.LoadAssembly(typeof(Syncfusion.WinForms.Themes.Office2016Theme).Assembly);
            SfSkinManager.LoadAssembly(typeof(Syncfusion.WinForms.Themes.Office2019Theme).Assembly);
            SfSkinManager.LoadAssembly(typeof(Syncfusion.HighContrastTheme.WinForms.HighContrastTheme).Assembly);
            
            Application.EnableVisualStyles();
            Application.SetCompatibleTextRenderingDefault(false);
            Application.Run(new MainForm());
        }
    }
}
```

**MainForm.cs:**
```csharp
using System;
using System.Windows.Forms;
using Syncfusion.WinForms.Controls;

namespace ThemedApplication
{
    public class MainForm : SfForm
    {
        private MenuStrip menuStrip;
        private ToolStripMenuItem viewMenu;
        
        public MainForm()
        {
            InitializeComponent();
            
            this.Text = "Themed Application";
            this.Size = new Size(1000, 700);
            
            // Apply default theme
            this.ThemeName = "Office2016Colorful";
            
            // Setup theme menu
            InitializeThemeMenu();
            
            // Load saved preference
            LoadThemePreference();
        }
        
        private void InitializeThemeMenu()
        {
            menuStrip = new MenuStrip();
            viewMenu = new ToolStripMenuItem("View");
            
            ToolStripMenuItem themeMenu = new ToolStripMenuItem("Theme");
            
            // Add theme options
            AddThemeMenuItem(themeMenu, "Office 2016 Colorful", "Office2016Colorful");
            AddThemeMenuItem(themeMenu, "Office 2016 White", "Office2016White");
            AddThemeMenuItem(themeMenu, "Office 2016 Dark Gray", "Office2016DarkGray");
            AddThemeMenuItem(themeMenu, "Office 2016 Black", "Office2016Black");
            AddThemeMenuItem(themeMenu, "Office 2019 Colorful", "Office2019Colorful");
            AddThemeMenuItem(themeMenu, "High Contrast Black", "HighContrastBlack");
            
            viewMenu.DropDownItems.Add(themeMenu);
            menuStrip.Items.Add(viewMenu);
            
            this.MainMenuStrip = menuStrip;
            this.Controls.Add(menuStrip);
        }
        
        private void AddThemeMenuItem(ToolStripMenuItem parent, string displayName, string themeName)
        {
            ToolStripMenuItem item = new ToolStripMenuItem(displayName);
            item.Click += (s, e) => ApplyTheme(themeName);
            parent.DropDownItems.Add(item);
        }
        
        private void ApplyTheme(string themeName)
        {
            this.ThemeName = themeName;
            SaveThemePreference(themeName);
            this.Refresh();
        }
        
        private void SaveThemePreference(string themeName)
        {
            Properties.Settings.Default.Theme = themeName;
            Properties.Settings.Default.Save();
        }
        
        private void LoadThemePreference()
        {
            string savedTheme = Properties.Settings.Default.Theme;
            if (!string.IsNullOrEmpty(savedTheme))
            {
                this.ThemeName = savedTheme;
            }
        }
    }
}
```

## Troubleshooting

### Theme Not Applying
- Verify theme assembly is loaded in `Program.cs`
- Check theme name spelling (case-sensitive)
- Ensure `ThemeName` is set after `InitializeComponent()`
- Confirm license is registered

### Theme Assembly Not Found
- Install required NuGet packages
- Verify assembly references in project
- Check .dll files are in output directory
- Rebuild solution

### Partial Theme Application
- Some controls may not support all themes
- Custom controls need theme-aware implementation
- Third-party controls won't inherit Syncfusion themes
- Check control compatibility documentation

### Performance Issues
- Loading multiple theme assemblies is normal
- Theme switching is fast, no performance impact
- If slow, check for custom drawing code
- Optimize OnThemeNameChanged handlers
