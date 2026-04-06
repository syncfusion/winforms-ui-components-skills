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

### Office2016 Color Schemes

Office2016 style includes four color schemes:

| Color Scheme | Appearance | Best For |
|--------------|------------|----------|
| **Colorful** | Vibrant, bright accents | User-facing apps, creative tools |
| **White** | Clean, bright white background | Document-focused apps, data entry |
| **DarkGray** | Medium-dark gray palette | Development tools, extended use |
| **Black** | Dark theme, minimal eye strain | Dark mode, low-light environments |

```csharp
// Apply any color scheme
this.editableList1.Style = Syncfusion.Windows.Forms.Appearance.Office2016;
this.editableList1.Office2016ColorScheme = ScrollBarOffice2016ColorScheme.Colorful; // or White, DarkGray, Black
```

## Complete Styling Examples

### Professional Business Application

```csharp
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

private void SetupProfessionalStyle()
{
    this.editableList1 = new EditableList();
    this.editableList1.Location = new Point(20, 20);
    this.editableList1.Size = new Size(350, 300);
    
    // Apply Office2016 White theme
    this.editableList1.Style = Syncfusion.Windows.Forms.Appearance.Office2016;
    this.editableList1.Office2016ColorScheme = ScrollBarOffice2016ColorScheme.White;
    this.editableList1.ListBox.Font = new Font("Segoe UI", 10F);
    
    this.Controls.Add(this.editableList1);
}
```

### Dark Mode Development Tool

```csharp
private void SetupDarkModeStyle()
{
    this.editableList1 = new EditableList();
    this.editableList1.Style = Syncfusion.Windows.Forms.Appearance.Office2016;
    this.editableList1.Office2016ColorScheme = ScrollBarOffice2016ColorScheme.Black;
    this.editableList1.ListBox.Font = new Font("Consolas", 10F);
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

```csharp
private void ApplyThemeFromSelection(string theme, string colorScheme)
{
    switch (theme)
    {
        case "Default":
            editableList1.Style = Syncfusion.Windows.Forms.Appearance.Default;
            break;
        case "Metro":
            editableList1.Style = Syncfusion.Windows.Forms.Appearance.Metro;
            break;
        case "Office2016":
            editableList1.Style = Syncfusion.Windows.Forms.Appearance.Office2016;
            editableList1.Office2016ColorScheme = (ScrollBarOffice2016ColorScheme)
                Enum.Parse(typeof(ScrollBarOffice2016ColorScheme), colorScheme);
            break;
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

```csharp
private void ApplyCustomStyling()
{
    // Apply base theme
    this.editableList1.Style = Syncfusion.Windows.Forms.Appearance.Office2016;
    this.editableList1.Office2016ColorScheme = ScrollBarOffice2016ColorScheme.White;
    
    // Customize colors and fonts
    this.editableList1.ListBox.BackColor = Color.FromArgb(250, 250, 255);
    this.editableList1.ListBox.ForeColor = Color.FromArgb(30, 30, 30);
    this.editableList1.ListBox.Font = new Font("Segoe UI", 10F);
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

| Issue | Solution |
|-------|----------|
| Theme not applying | Set both `Style` property AND `Office2016ColorScheme` (for Office2016) |
| Colors look wrong | Theme may be overridden by custom BackColor/ForeColor |
| Runtime changes not visible | Call `Refresh()` or `Invalidate()` after theme change |
| Inconsistent appearance | Apply theme to all controls using centralized theme manager |
| Dark theme unreadable | Use Black or DarkGray scheme, not custom dark BackColor |

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
