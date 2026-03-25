# Styling and Theming

Complete guide for applying visual styles and themes to EditableList control for modern, professional Windows Forms applications.

## Overview

EditableList supports multiple visual styles that transform its appearance from standard Windows controls to modern, professional UI. The styling system includes:

- **Default** - Standard Windows Forms appearance
- **Metro** - Modern flat design inspired by Microsoft Metro
- **Office2016** - Microsoft Office 2016-inspired themes with multiple color schemes

Styling provides consistent, professional appearance across your application while maintaining full functionality.

## Visual Styles Overview

### Available Styles

| Style | Description | Best For |
|-------|-------------|----------|
| **Default** | Standard Windows Forms look | Legacy applications, standard Windows UI |
| **Metro** | Flat, modern design | Contemporary apps, minimalist interfaces |
| **Office2016** | Microsoft Office 2016 aesthetic | Business applications, professional tools |

### Style Property

The `Style` property controls the visual appearance:

```csharp
this.editableList1.Style = Syncfusion.Windows.Forms.Appearance.Office2016;
```

## Default Style

The Default style uses standard Windows Forms appearance - classic borders, standard colors, and system theme integration.

### Applying Default Style

```csharp
// Set to Default style
this.editableList1.Style = Syncfusion.Windows.Forms.Appearance.Default;
```

**VB.NET:**
```vbnet
' Set to Default style
Me.editableList1.Style = Syncfusion.Windows.Forms.Appearance.Default
```

### When to Use Default

- **Legacy applications** requiring standard Windows appearance
- **System consistency** when matching OS theme
- **Simple requirements** without custom styling needs
- **Compatibility** with existing UI standards

### Default Style Characteristics

- Standard Windows Forms borders
- System colors and fonts
- Classic 3D effects
- Native Windows theme integration

## Metro Style

Metro style provides a modern, flat design aesthetic inspired by Microsoft's Metro design language.

### Applying Metro Style

```csharp
// Set to Metro style
this.editableList1.Style = Syncfusion.Windows.Forms.Appearance.Metro;
```

**VB.NET:**
```vbnet
' Set to Metro style
Me.editableList1.Style = Syncfusion.Windows.Forms.Appearance.Metro
```

### Metro Style Characteristics

- **Flat design** - No gradients or 3D effects
- **Clean lines** - Sharp, defined borders
- **Modern appearance** - Contemporary UI aesthetic
- **Bold colors** - Vibrant, solid color palette

### Metro Style Example

```csharp
using System.Drawing;
using Syncfusion.Windows.Forms.Tools;

private void ApplyMetroStyle()
{
    // Apply Metro style
    this.editableList1.Style = Syncfusion.Windows.Forms.Appearance.Metro;
    
    // Optional: Customize colors for Metro aesthetic
    this.editableList1.BackColor = Color.FromArgb(45, 45, 48);
    this.editableList1.ListBox.BackColor = Color.FromArgb(37, 37, 38);
    this.editableList1.ListBox.ForeColor = Color.White;
    
    // Metro-style font
    this.editableList1.ListBox.Font = new Font("Segoe UI", 10F);
}
```

### When to Use Metro

- **Modern applications** requiring contemporary design
- **Touch-friendly interfaces** with larger hit targets
- **Minimalist aesthetics** with focus on content
- **Mobile-inspired** desktop applications

## Office2016 Style

Office2016 style provides a Microsoft Office 2016-inspired appearance with four distinct color schemes.

### Applying Office2016 Style

```csharp
// Set to Office2016 style
this.editableList1.Style = Syncfusion.Windows.Forms.Appearance.Office2016;

// Set color scheme
this.editableList1.Office2016ColorScheme = ScrollBarOffice2016ColorScheme.Colorful;
```

**VB.NET:**
```vbnet
' Set to Office2016 style
Me.editableList1.Style = Syncfusion.Windows.Forms.Appearance.Office2016

' Set color scheme
Me.editableList1.Office2016ColorScheme = ScrollBarOffice2016ColorScheme.Colorful
```

### Office2016 Color Schemes

Office2016 style includes four color schemes:

#### 1. Colorful Scheme

Vibrant, colorful appearance with bright accents.

```csharp
this.editableList1.Style = Syncfusion.Windows.Forms.Appearance.Office2016;
this.editableList1.Office2016ColorScheme = ScrollBarOffice2016ColorScheme.Colorful;
```

**Characteristics:**
- Bright, vibrant colors
- Clear visual hierarchy
- High contrast
- Energetic, modern feel

**Best for:** User-facing applications, creative tools, engaging interfaces

#### 2. White Scheme

Clean, bright white background with subtle accents.

```csharp
this.editableList1.Style = Syncfusion.Windows.Forms.Appearance.Office2016;
this.editableList1.Office2016ColorScheme = ScrollBarOffice2016ColorScheme.White;
```

**Characteristics:**
- Predominantly white background
- Light gray accents
- Minimal visual weight
- Clean, spacious feel

**Best for:** Document-focused applications, data entry forms, professional tools

#### 3. DarkGray Scheme

Medium-dark gray palette for reduced eye strain.

```csharp
this.editableList1.Style = Syncfusion.Windows.Forms.Appearance.Office2016;
this.editableList1.Office2016ColorScheme = ScrollBarOffice2016ColorScheme.DarkGray;
```

**Characteristics:**
- Dark gray backgrounds
- Softer contrast
- Easier on eyes in dim lighting
- Professional appearance

**Best for:** Development tools, data analysis applications, extended use scenarios

#### 4. Black Scheme

Dark theme with black background for minimal eye strain.

```csharp
this.editableList1.Style = Syncfusion.Windows.Forms.Appearance.Office2016;
this.editableList1.Office2016ColorScheme = ScrollBarOffice2016ColorScheme.Black;
```

**Characteristics:**
- Black/very dark backgrounds
- High contrast with light text
- Minimal eye strain
- Modern, sleek appearance

**Best for:** Dark mode preferences, low-light environments, code editors

### Complete Office2016 Examples

**Colorful Scheme Example:**
```csharp
private void ApplyColorfulTheme()
{
    this.editableList1.Style = Syncfusion.Windows.Forms.Appearance.Office2016;
    this.editableList1.Office2016ColorScheme = ScrollBarOffice2016ColorScheme.Colorful;
    
    // The control automatically applies appropriate colors
    // Optionally customize further:
    this.editableList1.ListBox.Font = new Font("Segoe UI", 10F);
}
```

**Black Scheme Example:**
```csharp
private void ApplyBlackTheme()
{
    this.editableList1.Style = Syncfusion.Windows.Forms.Appearance.Office2016;
    this.editableList1.Office2016ColorScheme = ScrollBarOffice2016ColorScheme.Black;
    
    // Black scheme automatically sets dark colors
    // Control maintains readability with light text on dark background
}
```

**VB.NET - All Schemes:**
```vbnet
' Colorful
Me.editableList1.Style = Syncfusion.Windows.Forms.Appearance.Office2016
Me.editableList1.Office2016ColorScheme = ScrollBarOffice2016ColorScheme.Colorful

' White
Me.editableList1.Office2016ColorScheme = ScrollBarOffice2016ColorScheme.White

' DarkGray
Me.editableList1.Office2016ColorScheme = ScrollBarOffice2016ColorScheme.DarkGray

' Black
Me.editableList1.Office2016ColorScheme = ScrollBarOffice2016ColorScheme.Black
```

## Complete Styling Examples

### Example 1: Professional Business Application

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class ProfessionalForm : Form
{
    private EditableList editableList1;
    
    public ProfessionalForm()
    {
        InitializeComponent();
        SetupProfessionalStyle();
    }
    
    private void SetupProfessionalStyle()
    {
        // Create control
        this.editableList1 = new EditableList();
        this.editableList1.Location = new Point(20, 20);
        this.editableList1.Size = new Size(350, 300);
        
        // Apply Office2016 White theme
        this.editableList1.Style = Syncfusion.Windows.Forms.Appearance.Office2016;
        this.editableList1.Office2016ColorScheme = ScrollBarOffice2016ColorScheme.White;
        
        // Professional font
        this.editableList1.ListBox.Font = new Font("Segoe UI", 10F);
        this.editableList1.TextBox.Font = new Font("Segoe UI", 10F);
        
        // Add sample data
        string[] tasks = {
            "Q1 Report", "Budget Review", "Team Meeting",
            "Client Presentation", "Performance Analysis"
        };
        this.editableList1.ListBox.Items.AddRange(tasks);
        
        this.Controls.Add(this.editableList1);
    }
}
```

### Example 2: Dark Mode Development Tool

```csharp
private void SetupDarkModeStyle()
{
    // Create control
    this.editableList1 = new EditableList();
    this.editableList1.Location = new Point(20, 20);
    this.editableList1.Size = new Size(350, 300);
    
    // Apply Office2016 Black theme
    this.editableList1.Style = Syncfusion.Windows.Forms.Appearance.Office2016;
    this.editableList1.Office2016ColorScheme = ScrollBarOffice2016ColorScheme.Black;
    
    // Dark mode friendly font
    this.editableList1.ListBox.Font = new Font("Consolas", 10F);
    
    // Add development-related items
    string[] files = {
        "Program.cs", "MainForm.cs", "DataManager.cs",
        "Utils.cs", "Config.json"
    };
    this.editableList1.ListBox.Items.AddRange(files);
    
    this.Controls.Add(this.editableList1);
}
```

### Example 3: Modern Metro Application

```csharp
private void SetupMetroStyle()
{
    // Create control
    this.editableList1 = new EditableList();
    this.editableList1.Location = new Point(20, 20);
    this.editableList1.Size = new Size(350, 300);
    
    // Apply Metro style
    this.editableList1.Style = Syncfusion.Windows.Forms.Appearance.Metro;
    
    // Metro-inspired colors
    this.editableList1.BackColor = Color.FromArgb(240, 240, 240);
    this.editableList1.ListBox.BackColor = Color.White;
    
    // Metro font
    this.editableList1.ListBox.Font = new Font("Segoe UI Light", 11F);
    
    // Modern content
    string[] categories = {
        "Photos", "Music", "Videos", "Documents", "Downloads"
    };
    this.editableList1.ListBox.Items.AddRange(categories);
    
    this.Controls.Add(this.editableList1);
}
```

## Theme Consistency Across Forms

Apply the same theme to all controls in your application for consistency.

### Application-Wide Theme

```csharp
// In your main form or startup code
public class ThemeManager
{
    public static Syncfusion.Windows.Forms.Appearance CurrentStyle 
        = Syncfusion.Windows.Forms.Appearance.Office2016;
    
    public static ScrollBarOffice2016ColorScheme CurrentColorScheme 
        = ScrollBarOffice2016ColorScheme.Colorful;
    
    public static void ApplyTheme(EditableList control)
    {
        control.Style = CurrentStyle;
        if (CurrentStyle == Syncfusion.Windows.Forms.Appearance.Office2016)
        {
            control.Office2016ColorScheme = CurrentColorScheme;
        }
    }
}

// Usage in forms
public partial class MyForm : Form
{
    private void SetupControl()
    {
        var editableList = new EditableList();
        ThemeManager.ApplyTheme(editableList);
        this.Controls.Add(editableList);
    }
}
```

### User Theme Selection

Allow users to choose their preferred theme:

```csharp
public partial class SettingsForm : Form
{
    private ComboBox cmbTheme;
    private ComboBox cmbColorScheme;
    private EditableList editableListPreview;
    
    private void SetupThemeSelector()
    {
        // Theme selector
        cmbTheme = new ComboBox();
        cmbTheme.Items.AddRange(new object[] { "Default", "Metro", "Office2016" });
        cmbTheme.SelectedIndexChanged += CmbTheme_SelectedIndexChanged;
        
        // Color scheme selector (for Office2016)
        cmbColorScheme = new ComboBox();
        cmbColorScheme.Items.AddRange(new object[] { 
            "Colorful", "White", "DarkGray", "Black" 
        });
        cmbColorScheme.SelectedIndexChanged += CmbColorScheme_SelectedIndexChanged;
        cmbColorScheme.Enabled = false; // Disable until Office2016 selected
        
        // Preview control
        editableListPreview = new EditableList();
        editableListPreview.ListBox.Items.AddRange(new object[] {
            "Item 1", "Item 2", "Item 3"
        });
    }
    
    private void CmbTheme_SelectedIndexChanged(object sender, EventArgs e)
    {
        string selectedTheme = cmbTheme.SelectedItem.ToString();
        
        switch (selectedTheme)
        {
            case "Default":
                editableListPreview.Style = Syncfusion.Windows.Forms.Appearance.Default;
                cmbColorScheme.Enabled = false;
                break;
            case "Metro":
                editableListPreview.Style = Syncfusion.Windows.Forms.Appearance.Metro;
                cmbColorScheme.Enabled = false;
                break;
            case "Office2016":
                editableListPreview.Style = Syncfusion.Windows.Forms.Appearance.Office2016;
                cmbColorScheme.Enabled = true;
                cmbColorScheme.SelectedIndex = 0; // Default to Colorful
                break;
        }
    }
    
    private void CmbColorScheme_SelectedIndexChanged(object sender, EventArgs e)
    {
        if (editableListPreview.Style != Syncfusion.Windows.Forms.Appearance.Office2016)
            return;
        
        string selectedScheme = cmbColorScheme.SelectedItem.ToString();
        
        switch (selectedScheme)
        {
            case "Colorful":
                editableListPreview.Office2016ColorScheme = ScrollBarOffice2016ColorScheme.Colorful;
                break;
            case "White":
                editableListPreview.Office2016ColorScheme = ScrollBarOffice2016ColorScheme.White;
                break;
            case "DarkGray":
                editableListPreview.Office2016ColorScheme = ScrollBarOffice2016ColorScheme.DarkGray;
                break;
            case "Black":
                editableListPreview.Office2016ColorScheme = ScrollBarOffice2016ColorScheme.Black;
                break;
        }
    }
}
```

## Style Comparison

### Quick Reference Table

| Feature | Default | Metro | Office2016 |
|---------|---------|-------|------------|
| **Design Language** | Classic Windows | Modern Flat | Microsoft Office |
| **3D Effects** | Yes | No | Minimal |
| **Color Schemes** | System theme | N/A | 4 options |
| **Best For** | Legacy apps | Modern apps | Business apps |
| **Customization** | Limited | Moderate | Extensive |
| **Visual Weight** | Medium-Heavy | Light | Light-Medium |

### Choosing the Right Style

**Choose Default when:**
- Maintaining existing application look
- Requiring OS theme integration
- Building for conservative enterprise environments
- Legacy system compatibility is priority

**Choose Metro when:**
- Building modern, minimalist interfaces
- Targeting touch-friendly applications
- Wanting clean, flat design
- Appealing to contemporary aesthetic preferences

**Choose Office2016 when:**
- Building professional business applications
- Users are familiar with Microsoft Office
- Need multiple color scheme options
- Want polished, modern professional look

## Custom Styling Beyond Themes

While themes provide comprehensive styling, you can further customize appearance:

### Custom Colors

```csharp
private void ApplyCustomColors()
{
    // Apply base theme
    this.editableList1.Style = Syncfusion.Windows.Forms.Appearance.Office2016;
    this.editableList1.Office2016ColorScheme = ScrollBarOffice2016ColorScheme.White;
    
    // Then customize further
    this.editableList1.ListBox.BackColor = Color.FromArgb(250, 250, 255);
    this.editableList1.ListBox.ForeColor = Color.FromArgb(30, 30, 30);
    this.editableList1.TextBox.BackColor = Color.FromArgb(255, 255, 240);
}
```

### Custom Fonts

```csharp
private void ApplyCustomFonts()
{
    // Brand-specific or accessibility fonts
    Font customFont = new Font("Open Sans", 10F, FontStyle.Regular);
    
    this.editableList1.ListBox.Font = customFont;
    this.editableList1.TextBox.Font = customFont;
}
```

## Best Practices

1. **Consistency:** Apply the same theme across all controls in your application
2. **User Choice:** Consider allowing users to select their preferred theme
3. **Accessibility:** Test theme with high contrast settings and screen readers
4. **Performance:** Theme changes are lightweight; feel free to switch dynamically
5. **Testing:** Verify theme appearance on different display resolutions and DPI settings
6. **Documentation:** Document your theme choice in code comments
7. **Defaults:** Choose sensible defaults (Office2016 Colorful is generally safe)

## Troubleshooting

**Issue:** Theme not applying  
**Solution:** Ensure you set both `Style` property AND `Office2016ColorScheme` (for Office2016)

**Issue:** Colors look wrong  
**Solution:** Theme may be overridden by custom BackColor/ForeColor. Remove custom colors or set after theme.

**Issue:** Theme changes at runtime not visible  
**Solution:** Call `Refresh()` or `Invalidate()` on the control after theme change

**Issue:** Inconsistent appearance across controls  
**Solution:** Apply theme to all Syncfusion controls using a centralized theme manager

**Issue:** Dark theme with light text unreadable  
**Solution:** Verify you're using Black or DarkGray scheme (Office2016), not custom dark BackColor

## Complete Working Example

Here's a complete example with theme switching:

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class ThemeDemo : Form
{
    private EditableList editableList1;
    private Button btnDefault, btnMetro, btnOffice;
    private ComboBox cmbColorScheme;
    
    public ThemeDemo()
    {
        InitializeComponent();
        SetupUI();
    }
    
    private void SetupUI()
    {
        this.Text = "EditableList Theme Demo";
        this.Size = new Size(500, 450);
        
        // Create EditableList
        editableList1 = new EditableList();
        editableList1.Location = new Point(20, 60);
        editableList1.Size = new Size(400, 300);
        editableList1.ListBox.Items.AddRange(new object[] {
            "Task 1", "Task 2", "Task 3", "Task 4"
        });
        this.Controls.Add(editableList1);
        
        // Theme buttons
        btnDefault = new Button { Text = "Default", Location = new Point(20, 20), Size = new Size(80, 30) };
        btnDefault.Click += (s, e) => ApplyDefaultTheme();
        this.Controls.Add(btnDefault);
        
        btnMetro = new Button { Text = "Metro", Location = new Point(110, 20), Size = new Size(80, 30) };
        btnMetro.Click += (s, e) => ApplyMetroTheme();
        this.Controls.Add(btnMetro);
        
        btnOffice = new Button { Text = "Office2016", Location = new Point(200, 20), Size = new Size(80, 30) };
        btnOffice.Click += (s, e) => ApplyOffice2016Theme();
        this.Controls.Add(btnOffice);
        
        // Color scheme selector
        cmbColorScheme = new ComboBox();
        cmbColorScheme.Location = new Point(290, 20);
        cmbColorScheme.Size = new Size(120, 25);
        cmbColorScheme.DropDownStyle = ComboBoxStyle.DropDownList;
        cmbColorScheme.Items.AddRange(new object[] { "Colorful", "White", "DarkGray", "Black" });
        cmbColorScheme.SelectedIndex = 0;
        cmbColorScheme.SelectedIndexChanged += (s, e) => UpdateColorScheme();
        this.Controls.Add(cmbColorScheme);
        
        // Apply initial theme
        ApplyOffice2016Theme();
    }
    
    private void ApplyDefaultTheme()
    {
        editableList1.Style = Syncfusion.Windows.Forms.Appearance.Default;
        cmbColorScheme.Enabled = false;
    }
    
    private void ApplyMetroTheme()
    {
        editableList1.Style = Syncfusion.Windows.Forms.Appearance.Metro;
        cmbColorScheme.Enabled = false;
    }
    
    private void ApplyOffice2016Theme()
    {
        editableList1.Style = Syncfusion.Windows.Forms.Appearance.Office2016;
        cmbColorScheme.Enabled = true;
        UpdateColorScheme();
    }
    
    private void UpdateColorScheme()
    {
        if (editableList1.Style != Syncfusion.Windows.Forms.Appearance.Office2016)
            return;
        
        switch (cmbColorScheme.SelectedItem.ToString())
        {
            case "Colorful":
                editableList1.Office2016ColorScheme = ScrollBarOffice2016ColorScheme.Colorful;
                break;
            case "White":
                editableList1.Office2016ColorScheme = ScrollBarOffice2016ColorScheme.White;
                break;
            case "DarkGray":
                editableList1.Office2016ColorScheme = ScrollBarOffice2016ColorScheme.DarkGray;
                break;
            case "Black":
                editableList1.Office2016ColorScheme = ScrollBarOffice2016ColorScheme.Black;
                break;
        }
    }
}
```

This example provides a complete working demonstration of all styling options with interactive theme switching.
