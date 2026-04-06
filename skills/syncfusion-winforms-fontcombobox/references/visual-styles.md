# Visual Styles and Theming

Comprehensive guide to applying visual themes, color schemes, and custom styles to FontComboBox for consistent application design.

## Overview

FontComboBox supports multiple built-in visual styles that match modern UI frameworks (Office2016, Metro, Office2007) and allows custom color theming for brand-specific designs.

**Available Themes:**
- Office 2016 (Colorful, White, Black, DarkGray)
- Metro
- Office 2010
- Office 2007 (Blue, Silver, Black)
- Default (Windows Forms standard)
- Custom color themes

---

## VisualStyle Property

Sets the overall visual theme for the FontComboBox control.

**Property Type:** `ThemedComboBoxStyles` (enum)  
**Default Value:** `ThemedComboBoxStyles.Default`  
**Namespace:** `Syncfusion.Windows.Forms.Tools`

### Available Styles

| Style | Description | Best For |
|-------|-------------|----------|
| **Office2016Colorful** | Vibrant Office 2016 theme | Modern, colorful UIs |
| **Office2016White** | Clean white Office 2016 | Minimalist designs |
| **Office2016Black** | Dark Office 2016 | Dark mode applications |
| **Office2016DarkGray** | Dark gray Office 2016 | Professional dark themes |
| **Metro** | Windows Metro flat design | Touch interfaces, modern apps |
| **Office2010** | Office 2010 ribbon style | Business applications |
| **Office2007** | Office 2007 glossy style | Classic Office look |
| **Default** | Standard Windows Forms | Match system theme |

---

## Applying Visual Styles

### Office2016Colorful

Vibrant, modern theme with accent colors.

**C# Example:**
```csharp
using Syncfusion.Windows.Forms.Tools;

fontComboBox.VisualStyle = ThemedComboBoxStyles.Office2016Colorful;
```

**VB.NET Example:**
```vb
Imports Syncfusion.Windows.Forms.Tools

fontComboBox.VisualStyle = ThemedComboBoxStyles.Office2016Colorful
```

**Appearance:**
- Bright accent colors
- Modern flat design
- Clear borders and focus states
- Colorful dropdown button

![Office2016Colorful Theme](images/office2016-colorful.png)

---

### Office2016White

Clean, minimalist white theme.

**C# Example:**
```csharp
fontComboBox.VisualStyle = ThemedComboBoxStyles.Office2016White;
```

**VB.NET Example:**
```vb
fontComboBox.VisualStyle = ThemedComboBoxStyles.Office2016White
```

**Appearance:**
- White background
- Light gray borders
- Subtle hover effects
- Clean, professional look

---

### Office2016Black

Dark theme for dark mode applications.

**C# Example:**
```csharp
fontComboBox.VisualStyle = ThemedComboBoxStyles.Office2016Black;
```

**VB.NET Example:**
```vb
fontComboBox.VisualStyle = ThemedComboBoxStyles.Office2016Black
```

**Appearance:**
- Dark background
- Light text
- High contrast
- Ideal for dark mode UIs

---

### Office2016DarkGray

Professional dark gray theme.

**C# Example:**
```csharp
fontComboBox.VisualStyle = ThemedComboBoxStyles.Office2016DarkGray;
```

**VB.NET Example:**
```vb
fontComboBox.VisualStyle = ThemedComboBoxStyles.Office2016DarkGray
```

**Appearance:**
- Medium-dark background
- Professional appearance
- Good contrast
- Less extreme than Office2016Black

---

### Metro

Windows Metro flat design.

**C# Example:**
```csharp
fontComboBox.VisualStyle = ThemedComboBoxStyles.Metro;
```

**VB.NET Example:**
```vb
fontComboBox.VisualStyle = ThemedComboBoxStyles.Metro
```

**Appearance:**
- Flat design language
- Sharp corners
- Minimal shadows
- Touch-friendly

---

### Office2010

Office 2010 ribbon-style theme.

**C# Example:**
```csharp
fontComboBox.VisualStyle = ThemedComboBoxStyles.Office2010;
```

**VB.NET Example:**
```vb
fontComboBox.VisualStyle = ThemedComboBoxStyles.Office2010
```

**Appearance:**
- Subtle gradients
- Rounded corners
- Professional business look

---

### Office2007

Classic Office 2007 glossy theme with color scheme options.

**C# Example:**
```csharp
fontComboBox.VisualStyle = ThemedComboBoxStyles.Office2007;
```

**VB.NET Example:**
```vb
fontComboBox.VisualStyle = ThemedComboBoxStyles.Office2007
```

**Appearance:**
- Glossy gradients
- Rounded borders
- Classic Office appearance
- Supports color schemes (Blue, Silver, Black)

---

### Default

Standard Windows Forms appearance.

**C# Example:**
```csharp
fontComboBox.VisualStyle = ThemedComboBoxStyles.Default;
```

**VB.NET Example:**
```vb
fontComboBox.VisualStyle = ThemedComboBoxStyles.Default
```

**Appearance:**
- Native Windows look
- Matches system theme
- No custom styling

---

## Office2007ColorScheme Property

When using Office2007 style, select a color scheme.

**Property Type:** `Office2007ColorScheme` (enum)  
**Default Value:** `Office2007ColorScheme.Blue`  
**Applies To:** Office2007 visual style only

### Available Color Schemes

| Scheme | Description |
|--------|-------------|
| **Blue** | Traditional Office blue theme |
| **Silver** | Gray/silver professional theme |
| **Black** | Dark Office theme |

### Applying Color Schemes

**C# Example:**
```csharp
// Blue scheme (default)
fontComboBox.VisualStyle = ThemedComboBoxStyles.Office2007;
fontComboBox.Office2007ColorScheme = Office2007ColorScheme.Blue;

// Silver scheme
fontComboBox.VisualStyle = ThemedComboBoxStyles.Office2007;
fontComboBox.Office2007ColorScheme = Office2007ColorScheme.Silver;

// Black scheme
fontComboBox.VisualStyle = ThemedComboBoxStyles.Office2007;
fontComboBox.Office2007ColorScheme = Office2007ColorScheme.Black;
```

**VB.NET Example:**
```vb
' Blue scheme (default)
fontComboBox.VisualStyle = ThemedComboBoxStyles.Office2007
fontComboBox.Office2007ColorScheme = Office2007ColorScheme.Blue

' Silver scheme
fontComboBox.VisualStyle = ThemedComboBoxStyles.Office2007
fontComboBox.Office2007ColorScheme = Office2007ColorScheme.Silver

' Black scheme
fontComboBox.VisualStyle = ThemedComboBoxStyles.Office2007
fontComboBox.Office2007ColorScheme = Office2007ColorScheme.Black
```

**Note:** Color scheme only affects Office2007 visual style. Other styles ignore this property.

---

## Custom Color Themes

Apply custom brand colors using Office2007ColorTheme with managed colors.

### Office2007ColorTheme Property

**Property Type:** `Office2007Theme` (enum)  
**Values:** `Managed`, `Blue`, `Silver`, `Black`

### Applying Custom Colors

**Step 1: Apply Custom Colors**

**C# Example:**
```csharp
using Syncfusion.Windows.Forms;


// Apply custom color (Orchid example)
Office2007Colors.ApplyManagedColors(this, Color.Orchid);

**Step 2: Set Theme to Managed**

// Set theme to managed mode
fontComboBox.Office2007ColorTheme = Office2007Theme.Managed;

// Other color examples
Office2007Colors.ApplyManagedColors(this, Color.DodgerBlue);
Office2007Colors.ApplyManagedColors(this, Color.Teal);
Office2007Colors.ApplyManagedColors(this, Color.Crimson);
```

**VB.NET Example:**
```vb
Imports Syncfusion.Windows.Forms

' Apply custom color (Orchid example)
Office2007Colors.ApplyManagedColors(Me, Color.Orchid)
' Set theme to managed mode
fontComboBox.Office2007ColorTheme = Office2007Theme.Managed

' Other color examples
Office2007Colors.ApplyManagedColors(Me, Color.DodgerBlue)
Office2007Colors.ApplyManagedColors(Me, Color.Teal)
Office2007Colors.ApplyManagedColors(Me, Color.Crimson)
```

![Custom Orchid Theme](images/custom-orchid-theme.png)

**Important:** `ApplyManagedColors` applies to the entire form, affecting all Syncfusion controls that support theming.

---

### Custom RGB Colors

**C# Example:**
```csharp
// Custom RGB color
Color customColor = Color.FromArgb(255, 102, 51, 153); // Purple
Office2007Colors.ApplyManagedColors(this, customColor);
fontComboBox.Office2007ColorTheme = Office2007Theme.Managed;

// Brand colors
Color brandPrimary = Color.FromArgb(255, 0, 120, 215); // Microsoft blue
Office2007Colors.ApplyManagedColors(this, brandPrimary);
```

---

### Applying to Multiple Controls

Custom colors affect all compatible Syncfusion controls on the form.

**C# Example:**
```csharp
// Apply to entire form
Office2007Colors.ApplyManagedColors(this, Color.DarkOrange);

// All these controls will use the custom theme
fontComboBox1.Office2007ColorTheme = Office2007Theme.Managed;
fontComboBox2.Office2007ColorTheme = Office2007Theme.Managed;
button1.UseVisualStyle = true;
panel1.BorderStyle = BorderStyle.FixedSingle;
```

---

## Complete Theme Examples

### Example 1: Modern Office2016 Application

```csharp
public partial class ModernForm : Form
{
    private FontComboBox fontComboBox;
    
    public ModernForm()
    {
        InitializeComponent();
        InitializeTheming();
    }
    
    private void InitializeTheming()
    {
        // Set form background to white
        this.BackColor = Color.White;
        
        // Create FontComboBox with Office2016 theme
        fontComboBox = new FontComboBox
        {
            Location = new Point(20, 20),
            Size = new Size(250, 25),
            VisualStyle = ThemedComboBoxStyles.Office2016Colorful,
            UseAutoComplete = true,
            DropDownStyle = ComboBoxStyle.DropDownList
        };
        
        this.Controls.Add(fontComboBox);
    }
}
```

---

### Example 2: Dark Mode Application

```csharp
public partial class DarkModeForm : Form
{
    private FontComboBox fontComboBox;
    
    public DarkModeForm()
    {
        InitializeComponent();
        InitializeDarkTheme();
    }
    
    private void InitializeDarkTheme()
    {
        // Dark form background
        this.BackColor = Color.FromArgb(255, 45, 45, 48);
        this.ForeColor = Color.White;
        
        // FontComboBox with dark theme
        fontComboBox = new FontComboBox
        {
            Location = new Point(20, 20),
            Size = new Size(250, 25),
            VisualStyle = ThemedComboBoxStyles.Office2016Black,
            UseAutoComplete = true,
            DropDownStyle = ComboBoxStyle.DropDownList
        };
        
        this.Controls.Add(fontComboBox);
    }
}
```

---

### Example 3: Custom Branded Application

```csharp
public partial class BrandedForm : Form
{
    private FontComboBox fontComboBox;
    
    public BrandedForm()
    {
        InitializeComponent();
        InitializeBrandedTheme();
    }
    
    private void InitializeBrandedTheme()
    {
        // Company brand color
        Color brandColor = Color.FromArgb(255, 0, 120, 215);
        
        // Apply to entire form
        Office2007Colors.ApplyManagedColors(this, brandColor);
        
        // FontComboBox with custom theme
        fontComboBox = new FontComboBox
        {
            Location = new Point(20, 20),
            Size = new Size(250, 25),
            Office2007ColorTheme = Office2007Theme.Managed,
            UseAutoComplete = true,
            DropDownStyle = ComboBoxStyle.DropDownList
        };
        
        this.Controls.Add(fontComboBox);
    }
}
```

---

### Example 4: Metro Touch Interface

```csharp
public partial class TouchForm : Form
{
    private FontComboBox fontComboBox;
    
    public TouchForm()
    {
        InitializeComponent();
        InitializeMetroTheme();
    }
    
    private void InitializeMetroTheme()
    {
        // Metro flat design
        fontComboBox = new FontComboBox
        {
            Location = new Point(20, 20),
            Size = new Size(300, 35), // Larger for touch
            VisualStyle = ThemedComboBoxStyles.Metro,
            UseAutoComplete = true,
            DropDownStyle = ComboBoxStyle.DropDownList,
            ItemHeight = 30 // Larger items for touch
        };
        
        this.Controls.Add(fontComboBox);
    }
}
```

---

### Example 5: Theme Switcher

Allow users to change themes dynamically.

```csharp
public partial class ThemeSwitcherForm : Form
{
    private FontComboBox fontComboBox;
    private ComboBox themeComboBox;
    
    public ThemeSwitcherForm()
    {
        InitializeComponent();
        InitializeControls();
    }
    
    private void InitializeControls()
    {
        // Theme selector
        themeComboBox = new ComboBox
        {
            Location = new Point(20, 20),
            Size = new Size(200, 25),
            DropDownStyle = ComboBoxStyle.DropDownList
        };
        
        themeComboBox.Items.AddRange(new object[]
        {
            "Office2016Colorful",
            "Office2016White",
            "Office2016Black",
            "Office2016DarkGray",
            "Metro",
            "Office2010",
            "Office2007",
            "Default"
        });
        
        themeComboBox.SelectedIndex = 0;
        themeComboBox.SelectedIndexChanged += ThemeComboBox_SelectedIndexChanged;
        
        // FontComboBox
        fontComboBox = new FontComboBox
        {
            Location = new Point(20, 60),
            Size = new Size(250, 25),
            UseAutoComplete = true
        };
        
        this.Controls.Add(themeComboBox);
        this.Controls.Add(fontComboBox);
    }
    
    private void ThemeComboBox_SelectedIndexChanged(object sender, EventArgs e)
    {
        string selectedTheme = themeComboBox.SelectedItem.ToString();
        
        switch (selectedTheme)
        {
            case "Office2016Colorful":
                fontComboBox.VisualStyle = ThemedComboBoxStyles.Office2016Colorful;
                break;
            case "Office2016White":
                fontComboBox.VisualStyle = ThemedComboBoxStyles.Office2016White;
                break;
            case "Office2016Black":
                fontComboBox.VisualStyle = ThemedComboBoxStyles.Office2016Black;
                break;
            case "Office2016DarkGray":
                fontComboBox.VisualStyle = ThemedComboBoxStyles.Office2016DarkGray;
                break;
            case "Metro":
                fontComboBox.VisualStyle = ThemedComboBoxStyles.Metro;
                break;
            case "Office2010":
                fontComboBox.VisualStyle = ThemedComboBoxStyles.Office2010;
                break;
            case "Office2007":
                fontComboBox.VisualStyle = ThemedComboBoxStyles.Office2007;
                break;
            case "Default":
                fontComboBox.VisualStyle = ThemedComboBoxStyles.Default;
                break;
        }
        
        fontComboBox.Refresh();
    }
}
```

---

## Best Practices

### 1. Match Application Theme

Choose visual style that matches overall application design.

```csharp
// Modern app
fontComboBox.VisualStyle = ThemedComboBoxStyles.Office2016Colorful;

// Dark mode app
fontComboBox.VisualStyle = ThemedComboBoxStyles.Office2016Black;

// Touch interface
fontComboBox.VisualStyle = ThemedComboBoxStyles.Metro;
```

### 2. Apply Consistent Theming

Use same theme across all Syncfusion controls.

```csharp
// Apply to all controls
var theme = ThemedComboBoxStyles.Office2016Colorful;
fontComboBox1.VisualStyle = theme;
fontComboBox2.VisualStyle = theme;
comboBox1.VisualStyle = theme;
```

### 3. Test Theme Contrast

Ensure readability with chosen theme.

```csharp
// Dark theme needs light form background check
if (fontComboBox.VisualStyle == ThemedComboBoxStyles.Office2016Black)
{
    // Ensure form background is dark
    this.BackColor = Color.FromArgb(255, 45, 45, 48);
}
```

### 4. Custom Colors for Branding

Use managed colors for brand consistency.

```csharp
Color brandColor = Color.FromArgb(255, 0, 120, 215);
Office2007Colors.ApplyManagedColors(this, brandColor);
```

---

## Theme Compatibility

| Visual Style | Windows 7 | Windows 8/10 | Windows 11 |
|--------------|-----------|--------------|------------|
| Office2016* | ✅ | ✅ | ✅ (Recommended) |
| Metro | ✅ | ✅ (Best match) | ✅ |
| Office2010 | ✅ | ✅ | ✅ |
| Office2007 | ✅ | ✅ | ⚠️ (Dated look) |
| Default | ✅ | ✅ | ✅ |

---

## Troubleshooting

### Theme Not Applying

**Solution:**
```csharp
// Ensure using correct namespace
using Syncfusion.Windows.Forms.Tools;

// Set visual style explicitly
fontComboBox.VisualStyle = ThemedComboBoxStyles.Office2016Colorful;

// Force refresh
fontComboBox.Refresh();
```

### Custom Colors Not Working

**Check:**
1. Office2007ColorTheme set to `Managed`
2. ApplyManagedColors called on form
3. Call before control initialization

```csharp
// Correct order
Office2007Colors.ApplyManagedColors(this, Color.DodgerBlue);
fontComboBox.Office2007ColorTheme = Office2007Theme.Managed;
```

### Color Scheme Ignored

**Solution:** Color scheme only works with Office2007 visual style.

```csharp
// Must use Office2007 style
fontComboBox.VisualStyle = ThemedComboBoxStyles.Office2007;
fontComboBox.Office2007ColorScheme = Office2007ColorScheme.Silver;
```

---

## Related Topics

- **Getting Started**: Basic control setup → [getting-started.md](getting-started.md)
- **DropDown Configuration**: Customize appearance → [dropdown-configuration.md](dropdown-configuration.md)
