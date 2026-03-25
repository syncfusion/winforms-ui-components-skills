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

**C#:**
```csharp
// Enable XP themes for 3D borders
textBoxExt1.BorderStyle = System.Windows.Forms.BorderStyle.Fixed3D;
textBoxExt1.ThemesEnabled = true;
```

**VB.NET:**
```vb
' Enable XP themes for 3D borders
textBoxExt1.BorderStyle = System.Windows.Forms.BorderStyle.Fixed3D
textBoxExt1.ThemesEnabled = True
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

**C#:**
```csharp
// Set theme by name
textBoxExt1.ThemeName = "Office2019Colorful";
```

**VB.NET:**
```vb
' Set theme by name
textBoxExt1.ThemeName = "Office2019Colorful"
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

**C#:**
```csharp
using Syncfusion.Windows.Forms;
using System;
using System.Windows.Forms;

static class Program
{
    /// <summary>
    /// The main entry point for the application.
    /// </summary>
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

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms
Imports System.Windows.Forms

Friend NotInheritable Class Program
    Private Sub New()
    End Sub
    
    ''' <summary>
    ''' The main entry point for the application.
    ''' </summary>
    <STAThread>
    Shared Sub Main()
        ' Load theme assemblies
        SkinManager.LoadAssembly(GetType(Syncfusion.WinForms.Themes.Office2016Theme).Assembly)
        SkinManager.LoadAssembly(GetType(Syncfusion.WinForms.Themes.Office2019Theme).Assembly)
        SkinManager.LoadAssembly(GetType(Syncfusion.HighContrastTheme.WinForms.HighContrastTheme).Assembly)

        Application.EnableVisualStyles()
        Application.SetCompatibleTextRenderingDefault(False)
        Application.Run(New Form1())
    End Sub
End Class
```

**Important:** 
- Call `SkinManager.LoadAssembly()` BEFORE `Application.EnableVisualStyles()`
- Load assemblies only once at application startup
- Loading is a one-time operation per application session

## Office2016 Themes

The Office2016 theme family provides four variations with modern flat design.

### Office2016Colorful

Vibrant, colorful theme matching Office 2016's default appearance.

**C#:**
```csharp
// Load assembly in Program.Main()
SkinManager.LoadAssembly(typeof(Syncfusion.WinForms.Themes.Office2016Theme).Assembly);

// Apply in form
textBoxExt1.ThemeName = "Office2016Colorful";
```

**VB.NET:**
```vb
' Load assembly in Program.Main()
SkinManager.LoadAssembly(GetType(Syncfusion.WinForms.Themes.Office2016Theme).Assembly)

' Apply in form
textBoxExt1.ThemeName = "Office2016Colorful"
```

**Characteristics:**
- Bright accent colors
- Modern flat design
- High energy appearance

### Office2016White

Clean white theme with subtle accents.

**C#:**
```csharp
textBoxExt1.ThemeName = "Office2016White";
```

**VB.NET:**
```vb
textBoxExt1.ThemeName = "Office2016White"
```

**Characteristics:**
- White backgrounds
- Light gray borders
- Minimal, clean design
- Professional appearance

### Office2016DarkGray

Dark theme with reduced contrast for comfortable viewing.

**C#:**
```csharp
textBoxExt1.ThemeName = "Office2016DarkGray";
```

**VB.NET:**
```vb
textBoxExt1.ThemeName = "Office2016DarkGray"
```

**Characteristics:**
- Dark gray backgrounds
- Softer contrast than Black theme
- Comfortable for extended use
- Reduced eye strain

### Office2016Black

High-contrast dark theme for dark mode applications.

**C#:**
```csharp
textBoxExt1.ThemeName = "Office2016Black";
```

**VB.NET:**
```vb
textBoxExt1.ThemeName = "Office2016Black"
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

**VB.NET:**
```vb
textBoxExt1.ThemeName = "Office2019Colorful"
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

**VB.NET:**
```vb
textBoxExt1.ThemeName = "HighContrastBlack"
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

### Example 2: Runtime Theme Switching

```csharp
using Syncfusion.Windows.Forms.Tools;
using System;
using System.Windows.Forms;

public class ThemeManager
{
    private TextBoxExt[] textBoxes;
    
    public ThemeManager(params TextBoxExt[] textBoxes)
    {
        this.textBoxes = textBoxes;
    }
    
    public void ApplyTheme(string themeName)
    {
        foreach (var textBox in textBoxes)
        {
            try
            {
                textBox.ThemeName = themeName;
            }
            catch (Exception ex)
            {
                MessageBox.Show(
                    $"Failed to apply theme '{themeName}': {ex.Message}\n\n" +
                    "Ensure theme assembly is loaded in Program.Main()",
                    "Theme Error",
                    MessageBoxButtons.OK,
                    MessageBoxIcon.Error);
            }
        }
    }
}

// Usage in form
public partial class MainForm : Form
{
    private ThemeManager themeManager;
    private TextBoxExt textBox1, textBox2, textBox3;
    
    private void InitializeThemeManager()
    {
        themeManager = new ThemeManager(textBox1, textBox2, textBox3);
    }
    
    private void btnOffice2019_Click(object sender, EventArgs e)
    {
        themeManager.ApplyTheme("Office2019Colorful");
    }
    
    private void btnOffice2016White_Click(object sender, EventArgs e)
    {
        themeManager.ApplyTheme("Office2016White");
    }
    
    private void btnOffice2016Black_Click(object sender, EventArgs e)
    {
        themeManager.ApplyTheme("Office2016Black");
    }
    
    private void btnHighContrast_Click(object sender, EventArgs e)
    {
        themeManager.ApplyTheme("HighContrastBlack");
    }
}
```

### Example 3: Theme Selection ComboBox

```csharp
using Syncfusion.Windows.Forms.Tools;
using System;
using System.Windows.Forms;

public partial class MainForm : Form
{
    private ComboBox themeSelector;
    private TextBoxExt textBoxExt1;
    
    public MainForm()
    {
        InitializeComponent();
        CreateThemeSelector();
        CreateThemedTextBox();
    }
    
    private void CreateThemeSelector()
    {
        themeSelector = new ComboBox();
        themeSelector.Location = new System.Drawing.Point(20, 20);
        themeSelector.Size = new System.Drawing.Size(200, 25);
        themeSelector.DropDownStyle = ComboBoxStyle.DropDownList;
        
        // Add available themes
        themeSelector.Items.AddRange(new object[] {
            "Office2016Colorful",
            "Office2016White",
            "Office2016DarkGray",
            "Office2016Black",
            "Office2019Colorful",
            "HighContrastBlack"
        });
        
        themeSelector.SelectedIndex = 0;
        themeSelector.SelectedIndexChanged += ThemeSelector_SelectedIndexChanged;
        
        this.Controls.Add(themeSelector);
    }
    
    private void CreateThemedTextBox()
    {
        textBoxExt1 = new TextBoxExt();
        textBoxExt1.Location = new System.Drawing.Point(20, 60);
        textBoxExt1.Size = new System.Drawing.Size(300, 25);
        textBoxExt1.Text = "Theme preview textbox";
        textBoxExt1.ThemeName = "Office2016Colorful";
        
        this.Controls.Add(textBoxExt1);
    }
    
    private void ThemeSelector_SelectedIndexChanged(object sender, EventArgs e)
    {
        string selectedTheme = themeSelector.SelectedItem.ToString();
        
        try
        {
            textBoxExt1.ThemeName = selectedTheme;
        }
        catch (Exception ex)
        {
            MessageBox.Show(
                $"Error applying theme: {ex.Message}",
                "Theme Error",
                MessageBoxButtons.OK,
                MessageBoxIcon.Error);
        }
    }
}
```

### Example 4: Application-Wide Theme Configuration

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

### 1. Load Assemblies Once

Load theme assemblies only once in `Program.Main()`:

```csharp
// Good: Single load at startup
static void Main()
{
    SkinManager.LoadAssembly(typeof(Syncfusion.WinForms.Themes.Office2016Theme).Assembly);
    Application.Run(new Form1());
}

// Avoid: Multiple loads
// Don't call SkinManager.LoadAssembly() in form constructors or multiple times
```

### 2. Error Handling

Handle theme loading failures gracefully:

```csharp
private static void SafeLoadTheme(Type themeType)
{
    try
    {
        SkinManager.LoadAssembly(themeType.Assembly);
    }
    catch (Exception ex)
    {
        Console.WriteLine($"Warning: Could not load theme {themeType.Name}: {ex.Message}");
    }
}
```

### 3. Theme Consistency

Apply the same theme across all controls:

```csharp
public void ApplyThemeToAllControls(Control parent, string themeName)
{
    foreach (Control control in parent.Controls)
    {
        if (control is TextBoxExt textBox)
        {
            textBox.ThemeName = themeName;
        }
        
        // Recurse into child controls
        if (control.HasChildren)
        {
            ApplyThemeToAllControls(control, themeName);
        }
    }
}
```

### 4. User Preferences

Remember user's theme choice:

```csharp
// Save preference
Properties.Settings.Default.PreferredTheme = "Office2019Colorful";
Properties.Settings.Default.Save();

// Load preference
string theme = Properties.Settings.Default.PreferredTheme ?? "Office2016Colorful";
textBoxExt1.ThemeName = theme;
```

### 5. Performance

Set `ThemeName` once after control creation, not repeatedly:

```csharp
// Good: Set once
textBoxExt1.ThemeName = "Office2019Colorful";

// Avoid: Setting repeatedly in loops or timers
```

## Troubleshooting

### Theme Not Applied

**Problem:** Setting `ThemeName` has no effect.

**Solutions:**
1. Verify theme assembly is loaded in `Program.Main()`
2. Check assembly reference is added to project
3. Ensure `SkinManager.LoadAssembly()` is called before `Application.EnableVisualStyles()`
4. Verify theme name spelling (case-sensitive)

### Assembly Not Found

**Problem:** Exception when loading theme assembly.

**Solutions:**
1. Install required NuGet package
2. Verify assembly is in output directory
3. Check assembly version matches Syncfusion license
4. Ensure using correct namespace for theme type

### Theme Doesn't Match Expected Appearance

**Problem:** Theme looks different than expected.

**Solutions:**
1. Verify correct theme name is used
2. Check for custom property overrides (BackColor, ForeColor)
3. Ensure control's `Style` property isn't conflicting
4. Test with fresh control instance

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
