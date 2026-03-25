# Visual Styles and Themes

This guide covers the comprehensive theming and visual styling options for the GroupBar control. Syncfusion provides professional Office-style themes that make your applications look polished and modern.

## Table of Contents

- [VisualStyle Property Overview](#visualstyle-property-overview)
- [Default Theme](#default-theme)
- [Office2007 Theme](#office2007-theme)
- [Office2007Outlook Theme](#office2007outlook-theme)
- [Office2010 Theme](#office2010-theme)
- [Metro Theme](#metro-theme)
- [Office2016 Themes](#office2016-themes)
- [Managed Colors for Custom Branding](#managed-colors-for-custom-branding)
- [Theme Properties](#theme-properties)
- [Theme Comparison Table](#theme-comparison-table)
- [Setting Themes at Design Time vs Runtime](#setting-themes-at-design-time-vs-runtime)
- [Complete Theme Switching Examples](#complete-theme-switching-examples)

## VisualStyle Property Overview

The **VisualStyle** property controls the overall appearance of the GroupBar control. It provides professionally designed themes that match Microsoft Office applications.

```csharp
// Set visual style
this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2016Colorful;
```

**Available Visual Styles:**
- **Default** - Standard Windows Forms appearance
- **Office2007** - Office 2007 style with color schemes
- **Office2007Outlook** - Specialized Outlook 2007 style
- **Office2010** - Office 2010 style with color schemes
- **Metro** - Modern flat design
- **Office2016Colorful** - Office 2016 colorful theme
- **Office2016White** - Office 2016 white theme
- **Office2016DarkGray** - Office 2016 dark gray theme
- **Office2016Black** - Office 2016 black theme

**When to choose different styles:**
- **Default** - Simple applications, Windows Forms consistency
- **Office2007/2010** - Professional business applications
- **Metro** - Modern, touch-friendly interfaces
- **Office2016** - Current Office look, modern applications

## Default Theme

The Default theme provides standard Windows Forms appearance without special styling.

### Setting Default Theme

```csharp
// Apply default theme
this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Default;
```

### Complete Default Theme Example

```csharp
private void ApplyDefaultTheme()
{
    this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Default;
    this.groupBar1.BorderStyle = BorderStyle.Fixed3D;
    this.groupBar1.BackColor = SystemColors.Control;
    
    Console.WriteLine("Applied: Default theme");
}
```

**When to use Default theme:**
- Internal tools and utilities
- Simple data-entry applications
- Want standard Windows appearance
- Minimal visual styling needed

**Result:** GroupBar displays with standard Windows Forms gray appearance, 3D borders, and system colors.

## Office2007 Theme

The Office2007 theme provides the polished look of Microsoft Office 2007 with multiple color schemes.

### Basic Office2007 Theme

```csharp
// Apply Office 2007 theme
this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2007;
```

### Office2007 Color Schemes

Office2007 supports four color schemes: Blue, Black, Silver, and Managed.

#### Blue Color Scheme

```csharp
// Office 2007 Blue theme
this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2007;
this.groupBar1.Office2007Theme = Syncfusion.Windows.Forms.Office2007Theme.Blue;
```

**When to use Blue:**
- Default Office 2007 look
- Professional business applications
- Most universally accepted color scheme

**Result:** GroupBar displays with blue gradients and professional Office 2007 styling.

#### Black Color Scheme

```csharp
// Office 2007 Black theme
this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2007;
this.groupBar1.Office2007Theme = Syncfusion.Windows.Forms.Office2007Theme.Black;
```

**When to use Black:**
- Dramatic, bold appearance
- Media or creative applications
- High-contrast preference
- Modern, sophisticated look

**Result:** GroupBar displays with black backgrounds and silver/gray accents.

#### Silver Color Scheme

```csharp
// Office 2007 Silver theme
this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2007;
this.groupBar1.Office2007Theme = Syncfusion.Windows.Forms.Office2007Theme.Silver;
```

**When to use Silver:**
- Neutral, professional appearance
- Alternative to blue
- Subtle, understated look

**Result:** GroupBar displays with silver/gray gradients and neutral tones.

#### Managed Color Scheme

The Managed scheme allows custom brand colors.

```csharp
// Office 2007 with custom managed colors
this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2007;
this.groupBar1.Office2007Theme = Syncfusion.Windows.Forms.Office2007Theme.Managed;

// Apply custom color
Syncfusion.Windows.Forms.Office2007Colors.ApplyManagedColors(this, Color.Red);
```

**When to use Managed:**
- Corporate branding requirements
- Custom color schemes
- Unique application identity

**Result:** GroupBar displays with Office 2007 styling in your custom brand color.

### Complete Office2007 Theme Example

```csharp
private void SetupOffice2007Themes()
{
    // Create theme selector
    ComboBox themeSelector = new ComboBox
    {
        DropDownStyle = ComboBoxStyle.DropDownList,
        Dock = DockStyle.Top
    };
    
    themeSelector.Items.AddRange(new object[]
    {
        "Blue",
        "Black",
        "Silver",
        "Managed (Red)"
    });
    
    themeSelector.SelectedIndexChanged += (s, e) =>
    {
        ApplyOffice2007Theme(themeSelector.SelectedItem.ToString());
    };
    
    this.Controls.Add(themeSelector);
    themeSelector.SelectedIndex = 0;
}

private void ApplyOffice2007Theme(string theme)
{
    this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2007;
    
    switch (theme)
    {
        case "Blue":
            this.groupBar1.Office2007Theme = Syncfusion.Windows.Forms.Office2007Theme.Blue;
            break;
        case "Black":
            this.groupBar1.Office2007Theme = Syncfusion.Windows.Forms.Office2007Theme.Black;
            break;
        case "Silver":
            this.groupBar1.Office2007Theme = Syncfusion.Windows.Forms.Office2007Theme.Silver;
            break;
        case "Managed (Red)":
            this.groupBar1.Office2007Theme = Syncfusion.Windows.Forms.Office2007Theme.Managed;
            Syncfusion.Windows.Forms.Office2007Colors.ApplyManagedColors(this, Color.FromArgb(192, 0, 0));
            break;
    }
    
    Console.WriteLine($"Applied: Office 2007 {theme}");
}
```

## Office2007Outlook Theme

A specialized theme matching Microsoft Outlook 2007's appearance.

```csharp
// Apply Office 2007 Outlook theme
this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2007Outlook;
```

**When to use Office2007Outlook:**
- Building Outlook-style applications
- Email or calendar applications
- Want authentic Outlook appearance
- Stacked mode GroupBar

**Result:** GroupBar displays with authentic Outlook 2007 navigation pane styling, optimized for stacked mode.

### Complete Outlook Theme Example

```csharp
private void CreateOutlookInterface()
{
    // Configure for Outlook appearance
    this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2007Outlook;
    this.groupBar1.StackedMode = true;
    this.groupBar1.AllowCollapse = true;
    this.groupBar1.ShowItemImageInHeader = true;
    
    // Configure navigation pane
    this.groupBar1.NavigationPaneHeight = 50;
    this.groupBar1.ShowChevron = true;
    
    Console.WriteLine("Applied: Office 2007 Outlook theme");
}
```

## Office2010 Theme

The Office2010 theme provides the streamlined look of Microsoft Office 2010.

### Basic Office2010 Theme

```csharp
// Apply Office 2010 theme
this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2010;
```

### Office2010 Color Schemes

Like Office2007, Office2010 supports Blue, Black, Silver, and Managed schemes.

#### Blue Color Scheme

```csharp
// Office 2010 Blue theme
this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2010;
this.groupBar1.Office2010Theme = Syncfusion.Windows.Forms.Office2010Theme.Blue;
```

**Result:** Modern Office 2010 blue styling with cleaner lines than Office 2007.

#### Black Color Scheme

```csharp
// Office 2010 Black theme
this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2010;
this.groupBar1.Office2010Theme = Syncfusion.Windows.Forms.Office2010Theme.Black;
```

**Result:** Sleek black appearance with Office 2010 design language.

#### Silver Color Scheme

```csharp
// Office 2010 Silver theme
this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2010;
this.groupBar1.Office2010Theme = Syncfusion.Windows.Forms.Office2010Theme.Silver;
```

**Result:** Professional silver/gray Office 2010 styling.

#### Managed Color Scheme

```csharp
// Office 2010 with custom managed colors
this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2010;
this.groupBar1.Office2010Theme = Syncfusion.Windows.Forms.Office2010Theme.Managed;

// Apply custom color (e.g., corporate orange)
Syncfusion.Windows.Forms.Office2010Colors.ApplyManagedColors(this, Color.FromArgb(255, 140, 0));
```

**Result:** Office 2010 styling with your custom brand color.

### Complete Office2010 Theme Example

```csharp
private void SetupOffice2010Themes()
{
    // Create theme picker UI
    GroupBox themeGroup = new GroupBox
    {
        Text = "Office 2010 Themes",
        Dock = DockStyle.Top,
        Height = 100
    };
    
    RadioButton rbBlue = new RadioButton { Text = "Blue", Location = new Point(10, 20), Checked = true };
    RadioButton rbBlack = new RadioButton { Text = "Black", Location = new Point(10, 45) };
    RadioButton rbSilver = new RadioButton { Text = "Silver", Location = new Point(10, 70) };
    RadioButton rbManaged = new RadioButton { Text = "Managed (Green)", Location = new Point(100, 20) };
    
    rbBlue.CheckedChanged += (s, e) => { if (rbBlue.Checked) ApplyOffice2010Blue(); };
    rbBlack.CheckedChanged += (s, e) => { if (rbBlack.Checked) ApplyOffice2010Black(); };
    rbSilver.CheckedChanged += (s, e) => { if (rbSilver.Checked) ApplyOffice2010Silver(); };
    rbManaged.CheckedChanged += (s, e) => { if (rbManaged.Checked) ApplyOffice2010Managed(); };
    
    themeGroup.Controls.AddRange(new Control[] { rbBlue, rbBlack, rbSilver, rbManaged });
    this.Controls.Add(themeGroup);
    
    ApplyOffice2010Blue();
}

private void ApplyOffice2010Blue()
{
    this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2010;
    this.groupBar1.Office2010Theme = Syncfusion.Windows.Forms.Office2010Theme.Blue;
}

private void ApplyOffice2010Black()
{
    this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2010;
    this.groupBar1.Office2010Theme = Syncfusion.Windows.Forms.Office2010Theme.Black;
}

private void ApplyOffice2010Silver()
{
    this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2010;
    this.groupBar1.Office2010Theme = Syncfusion.Windows.Forms.Office2010Theme.Silver;
}

private void ApplyOffice2010Managed()
{
    this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2010;
    this.groupBar1.Office2010Theme = Syncfusion.Windows.Forms.Office2010Theme.Managed;
    Syncfusion.Windows.Forms.Office2010Colors.ApplyManagedColors(this, Color.Green);
}
```

## Metro Theme

The Metro theme provides a modern, flat design aesthetic.

```csharp
// Apply Metro theme
this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Metro;
```

**Characteristics:**
- Flat design (no gradients)
- Clean, minimalist appearance
- Bold colors
- Modern Windows 8/10 style

**When to use Metro:**
- Modern, touch-friendly applications
- Windows 8/10 style consistency
- Minimalist design preference
- Tablet or touch-screen interfaces

### Complete Metro Theme Example

```csharp
private void ApplyMetroTheme()
{
    // Apply Metro visual style
    this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Metro;
    
    // Configure for modern flat appearance
    this.groupBar1.BorderStyle = BorderStyle.FixedSingle;
    this.groupBar1.BackColor = Color.White;
    this.groupBar1.ForeColor = Color.FromArgb(60, 60, 60);
    
    // Use flat colors
    this.groupBar1.HeaderBackColor = Color.FromArgb(0, 120, 215); // Windows blue
    this.groupBar1.HeaderForeColor = Color.White;
    
    Console.WriteLine("Applied: Metro theme");
}
```

**Result:** GroupBar displays with flat, modern Metro styling suitable for touch interfaces.

## Office2016 Themes

Office 2016 provides four contemporary themes matching the latest Office applications.

### Office2016Colorful Theme

```csharp
// Apply Office 2016 Colorful theme
this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2016Colorful;
```

**Characteristics:**
- Vibrant accent colors
- Clean, modern design
- Colored headers
- Professional appearance

**When to use:**
- Current Office look
- Vibrant, colorful interface
- Modern professional applications

**Result:** GroupBar displays with colorful Office 2016 styling and vibrant accents.

### Office2016White Theme

```csharp
// Apply Office 2016 White theme
this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2016White;
```

**Characteristics:**
- Clean white background
- Subtle gray accents
- Minimalist design
- High contrast

**When to use:**
- Clean, uncluttered appearance
- Document-focused applications
- Lots of white space
- Professional, subtle look

**Result:** GroupBar displays with predominantly white Office 2016 styling.

### Office2016DarkGray Theme

```csharp
// Apply Office 2016 Dark Gray theme
this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2016DarkGray;
```

**Characteristics:**
- Dark gray background
- Good contrast
- Modern, professional
- Easy on the eyes

**When to use:**
- Reduced eye strain
- Long working hours
- Professional dark theme
- Code editors, development tools

**Result:** GroupBar displays with dark gray Office 2016 styling.

### Office2016Black Theme

```csharp
// Apply Office 2016 Black theme
this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2016Black;
```

**Characteristics:**
- Pure black background
- Maximum contrast
- Modern, bold appearance
- OLED-friendly

**When to use:**
- Maximum contrast needed
- Dark mode preference
- Media applications
- High-end professional tools

**Result:** GroupBar displays with black Office 2016 styling.

### Complete Office2016 Theme Example

```csharp
private void SetupOffice2016ThemeSelector()
{
    // Create theme menu
    MenuStrip menuStrip = new MenuStrip();
    ToolStripMenuItem themesMenu = new ToolStripMenuItem("Office 2016 Themes");
    
    ToolStripMenuItem colorfulItem = new ToolStripMenuItem("Colorful");
    ToolStripMenuItem whiteItem = new ToolStripMenuItem("White");
    ToolStripMenuItem darkGrayItem = new ToolStripMenuItem("Dark Gray");
    ToolStripMenuItem blackItem = new ToolStripMenuItem("Black");
    
    colorfulItem.Click += (s, e) => ApplyOffice2016Colorful();
    whiteItem.Click += (s, e) => ApplyOffice2016White();
    darkGrayItem.Click += (s, e) => ApplyOffice2016DarkGray();
    blackItem.Click += (s, e) => ApplyOffice2016Black();
    
    themesMenu.DropDownItems.AddRange(new ToolStripItem[]
    {
        colorfulItem, whiteItem, darkGrayItem, blackItem
    });
    
    menuStrip.Items.Add(themesMenu);
    this.Controls.Add(menuStrip);
    
    // Apply default theme
    ApplyOffice2016Colorful();
}

private void ApplyOffice2016Colorful()
{
    this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2016Colorful;
    this.BackColor = Color.White;
    Console.WriteLine("Applied: Office 2016 Colorful");
}

private void ApplyOffice2016White()
{
    this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2016White;
    this.BackColor = Color.White;
    Console.WriteLine("Applied: Office 2016 White");
}

private void ApplyOffice2016DarkGray()
{
    this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2016DarkGray;
    this.BackColor = Color.FromArgb(62, 62, 66);
    Console.WriteLine("Applied: Office 2016 Dark Gray");
}

private void ApplyOffice2016Black()
{
    this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2016Black;
    this.BackColor = Color.FromArgb(37, 37, 38);
    Console.WriteLine("Applied: Office 2016 Black");
}
```

## Managed Colors for Custom Branding

Managed colors allow custom brand colors with Office 2007 and Office 2010 themes.

### Applying Managed Colors

```csharp
// Office 2007 with custom color
this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2007;
this.groupBar1.Office2007Theme = Syncfusion.Windows.Forms.Office2007Theme.Managed;
Syncfusion.Windows.Forms.Office2007Colors.ApplyManagedColors(this, Color.Purple);

// Office 2010 with custom color
this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2010;
this.groupBar1.Office2010Theme = Syncfusion.Windows.Forms.Office2010Theme.Managed;
Syncfusion.Windows.Forms.Office2010Colors.ApplyManagedColors(this, Color.Teal);
```

### Corporate Branding Example

```csharp
private void ApplyCorporateBranding()
{
    // Define corporate colors
    Color corporatePrimary = Color.FromArgb(0, 114, 188);    // Corporate Blue
    Color corporateSecondary = Color.FromArgb(255, 140, 0);  // Corporate Orange
    
    // Apply to Office 2010 theme
    this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2010;
    this.groupBar1.Office2010Theme = Syncfusion.Windows.Forms.Office2010Theme.Managed;
    Syncfusion.Windows.Forms.Office2010Colors.ApplyManagedColors(this, corporatePrimary);
    
    // Apply form branding
    this.BackColor = Color.White;
    this.Text = "Corporate Application";
    
    Console.WriteLine($"Applied corporate branding: {corporatePrimary}");
}
```

### Multiple Managed Color Examples

```csharp
private void ShowManagedColorExamples()
{
    // Create color picker
    ComboBox colorPicker = new ComboBox
    {
        DropDownStyle = ComboBoxStyle.DropDownList,
        Dock = DockStyle.Top
    };
    
    colorPicker.Items.AddRange(new object[]
    {
        "Red", "Blue", "Green", "Purple", "Orange", "Teal"
    });
    
    colorPicker.SelectedIndexChanged += (s, e) =>
    {
        Color selectedColor = GetColorByName(colorPicker.SelectedItem.ToString());
        ApplyManagedColor(selectedColor);
    };
    
    this.Controls.Add(colorPicker);
    colorPicker.SelectedIndex = 1; // Default to Blue
}

private Color GetColorByName(string colorName)
{
    return colorName switch
    {
        "Red" => Color.FromArgb(192, 0, 0),
        "Blue" => Color.FromArgb(0, 112, 192),
        "Green" => Color.FromArgb(0, 176, 80),
        "Purple" => Color.FromArgb(112, 48, 160),
        "Orange" => Color.FromArgb(255, 140, 0),
        "Teal" => Color.FromArgb(0, 176, 176),
        _ => Color.Blue
    };
}

private void ApplyManagedColor(Color color)
{
    this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2010;
    this.groupBar1.Office2010Theme = Syncfusion.Windows.Forms.Office2010Theme.Managed;
    Syncfusion.Windows.Forms.Office2010Colors.ApplyManagedColors(this, color);
    
    Console.WriteLine($"Applied managed color: {color}");
}
```

## Theme Properties

### Office2007Theme Property

```csharp
// Get current Office 2007 theme
Syncfusion.Windows.Forms.Office2007Theme currentTheme = this.groupBar1.Office2007Theme;

// Set Office 2007 theme
this.groupBar1.Office2007Theme = Syncfusion.Windows.Forms.Office2007Theme.Blue;
```

**Available values:**
- `Blue` - Blue color scheme
- `Black` - Black color scheme
- `Silver` - Silver color scheme
- `Managed` - Custom managed colors

### Office2010Theme Property

```csharp
// Get current Office 2010 theme
Syncfusion.Windows.Forms.Office2010Theme currentTheme = this.groupBar1.Office2010Theme;

// Set Office 2010 theme
this.groupBar1.Office2010Theme = Syncfusion.Windows.Forms.Office2010Theme.Silver;
```

**Available values:**
- `Blue` - Blue color scheme
- `Black` - Black color scheme
- `Silver` - Silver color scheme
- `Managed` - Custom managed colors

## Theme Comparison Table

| Theme | Gradient | Flat Design | Color Schemes | Best For |
|-------|----------|-------------|---------------|----------|
| **Default** | No | Yes | N/A | Simple apps, utilities |
| **Office2007** | Yes | No | 4 (Blue, Black, Silver, Managed) | Professional 2007-era apps |
| **Office2007Outlook** | Yes | No | Fixed | Outlook clones |
| **Office2010** | Subtle | Mostly | 4 (Blue, Black, Silver, Managed) | Modern professional apps |
| **Metro** | No | Yes | N/A | Touch interfaces, modern apps |
| **Office2016Colorful** | No | Yes | Fixed | Current Office look, vibrant |
| **Office2016White** | No | Yes | Fixed | Clean, document-focused |
| **Office2016DarkGray** | No | Yes | Fixed | Dark mode, professional |
| **Office2016Black** | No | Yes | Fixed | Maximum contrast, dramatic |

## Setting Themes at Design Time vs Runtime

### Design Time

1. Select GroupBar control
2. Open Properties window
3. Find **VisualStyle** property
4. Choose from dropdown
5. If Office2007/2010, set **Office2007Theme** or **Office2010Theme**

### Runtime

```csharp
// Set at runtime
this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2016Colorful;

// Set with color scheme
this.groupBar1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2010;
this.groupBar1.Office2010Theme = Syncfusion.Windows.Forms.Office2010Theme.Blue;
```

### Configuration File

Store theme preference in app.config:

```xml
<appSettings>
  <add key="Theme" value="Office2016Colorful" />
</appSettings>
```

```csharp
private void LoadThemeFromConfig()
{
    string themeName = ConfigurationManager.AppSettings["Theme"] ?? "Office2016Colorful";
    
    if (Enum.TryParse<Syncfusion.Windows.Forms.VisualStyle>(themeName, out var visualStyle))
    {
        this.groupBar1.VisualStyle = visualStyle;
        Console.WriteLine($"Loaded theme from config: {themeName}");
    }
}
```

### User Preference Storage

Save user's theme choice:

```csharp
private void SaveThemePreference(string themeName)
{
    Properties.Settings.Default.PreferredTheme = themeName;
    Properties.Settings.Default.Save();
}

private void LoadThemePreference()
{
    string themeName = Properties.Settings.Default.PreferredTheme;
    if (!string.IsNullOrEmpty(themeName))
    {
        if (Enum.TryParse<Syncfusion.Windows.Forms.VisualStyle>(themeName, out var visualStyle))
        {
            this.groupBar1.VisualStyle = visualStyle;
        }
    }
}
```

## Complete Theme Switching Examples

### Example 1: Theme Switcher with Dropdown

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class ThemeSwitcherForm : Form
{
    private GroupBar groupBar1;
    private ComboBox themeSelector;
    private Label previewLabel;

    public ThemeSwitcherForm()
    {
        this.Text = "GroupBar Theme Switcher";
        this.Size = new Size(800, 600);
        
        CreateThemeSelector();
        CreateGroupBar();
        CreatePreviewArea();
    }

    private void CreateThemeSelector()
    {
        Panel topPanel = new Panel
        {
            Dock = DockStyle.Top,
            Height = 60,
            BackColor = Color.FromArgb(240, 240, 240),
            Padding = new Padding(10)
        };
        
        Label label = new Label
        {
            Text = "Select Theme:",
            AutoSize = true,
            Location = new Point(10, 18),
            Font = new Font("Segoe UI", 10F)
        };
        
        this.themeSelector = new ComboBox
        {
            DropDownStyle = ComboBoxStyle.DropDownList,
            Location = new Point(120, 15),
            Width = 200,
            Font = new Font("Segoe UI", 10F)
        };
        
        this.themeSelector.Items.AddRange(new object[]
        {
            "Default",
            "Office 2007 Blue",
            "Office 2007 Black",
            "Office 2007 Silver",
            "Office 2007 Outlook",
            "Office 2010 Blue",
            "Office 2010 Black",
            "Office 2010 Silver",
            "Metro",
            "Office 2016 Colorful",
            "Office 2016 White",
            "Office 2016 Dark Gray",
            "Office 2016 Black"
        });
        
        this.themeSelector.SelectedIndexChanged += ThemeSelector_SelectedIndexChanged;
        
        topPanel.Controls.AddRange(new Control[] { label, this.themeSelector });
        this.Controls.Add(topPanel);
        
        this.themeSelector.SelectedIndex = 9; // Default to Office 2016 Colorful
    }

    private void CreateGroupBar()
    {
        this.groupBar1 = new GroupBar
        {
            Dock = DockStyle.Left,
            Width = 220,
            BorderStyle = BorderStyle.Fixed3D
        };
        
        // Create sample items
        for (int i = 1; i <= 5; i++)
        {
            GroupBarItem item = new GroupBarItem
            {
                Text = $"Section {i}"
            };
            
            Panel panel = new Panel
            {
                BackColor = Color.White,
                Dock = DockStyle.Fill
            };
            
            Label label = new Label
            {
                Text = $"Content for Section {i}",
                Dock = DockStyle.Top,
                Padding = new Padding(10),
                Font = new Font("Segoe UI", 11F)
            };
            
            panel.Controls.Add(label);
            item.Client = panel;
            this.groupBar1.Controls.Add(panel);
            this.groupBar1.GroupBarItems.Add(item);
        }
        
        this.groupBar1.SelectedItem = 0;
        this.Controls.Add(this.groupBar1);
    }

    private void CreatePreviewArea()
    {
        this.previewLabel = new Label
        {
            Dock = DockStyle.Fill,
            Font = new Font("Segoe UI", 14F),
            Text = "Theme preview area\n\nChange themes to see different styles.",
            TextAlign = ContentAlignment.MiddleCenter,
            BackColor = Color.White
        };
        
        this.Controls.Add(this.previewLabel);
    }

    private void ThemeSelector_SelectedIndexChanged(object sender, EventArgs e)
    {
        string selectedTheme = this.themeSelector.SelectedItem.ToString();
        ApplyTheme(selectedTheme);
    }

    private void ApplyTheme(string themeName)
    {
        switch (themeName)
        {
            case "Default":
                this.groupBar1.VisualStyle = VisualStyle.Default;
                this.BackColor = SystemColors.Control;
                break;
                
            case "Office 2007 Blue":
                this.groupBar1.VisualStyle = VisualStyle.Office2007;
                this.groupBar1.Office2007Theme = Office2007Theme.Blue;
                this.BackColor = Color.FromArgb(191, 219, 255);
                break;
                
            case "Office 2007 Black":
                this.groupBar1.VisualStyle = VisualStyle.Office2007;
                this.groupBar1.Office2007Theme = Office2007Theme.Black;
                this.BackColor = Color.FromArgb(83, 83, 83);
                break;
                
            case "Office 2007 Silver":
                this.groupBar1.VisualStyle = VisualStyle.Office2007;
                this.groupBar1.Office2007Theme = Office2007Theme.Silver;
                this.BackColor = Color.FromArgb(223, 223, 234);
                break;
                
            case "Office 2007 Outlook":
                this.groupBar1.VisualStyle = VisualStyle.Office2007Outlook;
                this.BackColor = Color.FromArgb(227, 239, 255);
                break;
                
            case "Office 2010 Blue":
                this.groupBar1.VisualStyle = VisualStyle.Office2010;
                this.groupBar1.Office2010Theme = Office2010Theme.Blue;
                this.BackColor = Color.FromArgb(214, 229, 255);
                break;
                
            case "Office 2010 Black":
                this.groupBar1.VisualStyle = VisualStyle.Office2010;
                this.groupBar1.Office2010Theme = Office2010Theme.Black;
                this.BackColor = Color.FromArgb(102, 102, 102);
                break;
                
            case "Office 2010 Silver":
                this.groupBar1.VisualStyle = VisualStyle.Office2010;
                this.groupBar1.Office2010Theme = Office2010Theme.Silver;
                this.BackColor = Color.FromArgb(214, 214, 214);
                break;
                
            case "Metro":
                this.groupBar1.VisualStyle = VisualStyle.Metro;
                this.BackColor = Color.White;
                break;
                
            case "Office 2016 Colorful":
                this.groupBar1.VisualStyle = VisualStyle.Office2016Colorful;
                this.BackColor = Color.White;
                break;
                
            case "Office 2016 White":
                this.groupBar1.VisualStyle = VisualStyle.Office2016White;
                this.BackColor = Color.White;
                break;
                
            case "Office 2016 Dark Gray":
                this.groupBar1.VisualStyle = VisualStyle.Office2016DarkGray;
                this.BackColor = Color.FromArgb(62, 62, 66);
                this.previewLabel.ForeColor = Color.White;
                break;
                
            case "Office 2016 Black":
                this.groupBar1.VisualStyle = VisualStyle.Office2016Black;
                this.BackColor = Color.FromArgb(37, 37, 38);
                this.previewLabel.ForeColor = Color.White;
                break;
        }
        
        this.previewLabel.Text = $"Current Theme:\n{themeName}\n\nExperience the visual styling.";
        Console.WriteLine($"Applied theme: {themeName}");
    }
}
```

**Result:** A complete theme switcher application allowing users to preview all available GroupBar themes with appropriate background colors.

### Example 2: Corporate Branding with Managed Colors

```csharp
public class CorporateBrandingForm : Form
{
    private GroupBar groupBar1;

    public CorporateBrandingForm()
    {
        this.Text = "Corporate Branded Application";
        this.Size = new Size(1000, 700);
        
        CreateBrandedInterface();
    }

    private void CreateBrandedInterface()
    {
        // Define corporate colors
        Color brandPrimary = Color.FromArgb(0, 114, 188);     // Corporate Blue
        Color brandSecondary = Color.FromArgb(255, 140, 0);   // Corporate Orange
        Color brandAccent = Color.FromArgb(0, 176, 80);       // Corporate Green
        
        // Create GroupBar with corporate branding
        this.groupBar1 = new GroupBar
        {
            Dock = DockStyle.Left,
            Width = 240,
            BorderStyle = BorderStyle.FixedSingle,
            VisualStyle = VisualStyle.Office2010,
            Office2010Theme = Office2010Theme.Managed
        };
        
        // Apply managed colors
        Office2010Colors.ApplyManagedColors(this, brandPrimary);
        
        // Create branded sections
        CreateSection("Dashboard", brandPrimary);
        CreateSection("Reports", brandSecondary);
        CreateSection("Analytics", brandAccent);
        CreateSection("Settings", brandPrimary);
        
        this.groupBar1.SelectedItem = 0;
        this.Controls.Add(this.groupBar1);
        
        // Set form branding
        this.BackColor = Color.White;
    }

    private void CreateSection(string name, Color accentColor)
    {
        GroupBarItem item = new GroupBarItem
        {
            Text = name
        };
        
        Panel panel = new Panel
        {
            Dock = DockStyle.Fill,
            BackColor = Color.White,
            Padding = new Padding(20)
        };
        
        Label titleLabel = new Label
        {
            Text = name,
            Font = new Font("Segoe UI", 16F, FontStyle.Bold),
            ForeColor = accentColor,
            Dock = DockStyle.Top,
            Height = 40
        };
        
        panel.Controls.Add(titleLabel);
        item.Client = panel;
        this.groupBar1.Controls.Add(panel);
        this.groupBar1.GroupBarItems.Add(item);
    }
}
```

**Result:** A professionally branded application using corporate colors with Office 2010 managed color scheme.

## Key Takeaways

1. **VisualStyle Property** controls overall theme (Default, Office2007, Office2010, Metro, Office2016)
2. **Office2007/2010** themes support four color schemes: Blue, Black, Silver, Managed
3. **Managed Colors** enable custom brand colors with Office themes
4. **Office2016** provides four modern themes: Colorful, White, DarkGray, Black
5. **Metro** offers flat, modern design for touch interfaces
6. **Theme selection** impacts user experience and application perception
7. **ApplyManagedColors** method applies custom colors to Office themes
8. **Design time vs Runtime** theming both supported for flexibility
