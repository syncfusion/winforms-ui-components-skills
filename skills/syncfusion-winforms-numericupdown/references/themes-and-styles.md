# Themes and Styles for NumericUpDownExt

Complete guide to applying visual themes and styles to the NumericUpDownExt control for professional, modern appearances.

## Overview

The NumericUpDownExt control supports multiple visual styles and themes, including XP Themes, Office 2007, and Office 2016 styles. These theming capabilities allow the control to integrate seamlessly with your application's design and provide a native, professional look.

**Key Feature:** Unlike the standard .NET NumericUpDown control, NumericUpDownExt provides full XP Themes support and modern Office visual styles.

## Visual Styles Overview

Visual styles enhance the appearance of the NumericUpDownExt control beyond basic color customization. Syncfusion provides several built-in visual styles:

- **Default** - Standard appearance
- **Office2007** - Microsoft Office 2007 look with color schemes (Blue, Silver, Black, Managed)
- **Office2016** - Modern Office 2016 appearance (Colorful, White, Black, DarkGray)

## XP Themes Support

The XP Themes feature is a key advantage of NumericUpDownExt over the standard control.

### ThemesEnabled Property

**Type:** `bool`  
**Default:** `false`

### When to Use XP Themes
- Creating applications that match Windows visual styles
- Providing native Windows look and feel
- Ensuring consistency with other themed controls
- Professional, modern appearance

### Enabling XP Themes

```csharp
using Syncfusion.Windows.Forms.Tools;

// Enable XP Themes
numericUpDownExt1.ThemesEnabled = true;
```

**Result:** Control adopts the current Windows theme appearance (XP, Vista, Windows 7, etc.).

### XP Themes with Themed Border

```csharp
private void SetupXPThemes()
{
    NumericUpDownExt themedControl = new NumericUpDownExt();
    
    // Enable XP theming
    themedControl.ThemesEnabled = true;
    themedControl.ThemedBorder = true;
    
    themedControl.Location = new Point(50, 50);
    themedControl.Size = new Size(120, 24);
    themedControl.Value = new decimal(100);
    
    Label lblInfo = new Label();
    lblInfo.Text = "XP Themes Enabled";
    lblInfo.Location = new Point(50, 80);
    lblInfo.AutoSize = true;
    
    this.Controls.Add(themedControl);
    this.Controls.Add(lblInfo);
}
```

**Result:** Control with XP-themed borders and appearance matching Windows visual styles.

### Comparing Standard vs XP Themed

```csharp
private void CompareThemeStates()
{
    // Standard appearance
    NumericUpDownExt standardControl = new NumericUpDownExt();
    standardControl.Location = new Point(50, 30);
    standardControl.Size = new Size(120, 24);
    standardControl.ThemesEnabled = false;
    
    Label lblStandard = new Label();
    lblStandard.Text = "Standard (No Theme)";
    lblStandard.Location = new Point(50, 10);
    lblStandard.AutoSize = true;
    
    // XP Themed appearance
    NumericUpDownExt themedControl = new NumericUpDownExt();
    themedControl.Location = new Point(50, 80);
    themedControl.Size = new Size(120, 24);
    themedControl.ThemesEnabled = true;
    themedControl.ThemedBorder = true;
    
    Label lblThemed = new Label();
    lblThemed.Text = "XP Themes Enabled";
    lblThemed.Location = new Point(50, 60);
    lblThemed.AutoSize = true;
    
    this.Controls.Add(lblStandard);
    this.Controls.Add(standardControl);
    this.Controls.Add(lblThemed);
    this.Controls.Add(themedControl);
}
```

**Result:** Side-by-side comparison showing visual difference between themed and non-themed controls.

## Applying Themes to NumericUpDownExt

### VisualStyle Property

**Type:** `Syncfusion.Windows.Forms.VisualStyle`  
**Default:** `VisualStyle.Default`

**Available Options:**
- `Default` - Standard appearance
- `Office2007` - Office 2007 style
- `Office2016Colorful` - Modern colorful theme
- `Office2016White` - Clean white theme
- `Office2016Black` - Dark black theme
- `Office2016DarkGray` - Dark gray theme

### Office2007 Visual Style

```csharp
// Apply Office 2007 visual style
numericUpDownExt1.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2007;
numericUpDownExt1.ColorScheme = Syncfusion.Windows.Forms.Office2007Theme.Blue;
```

**Result:** Control displays with Office 2007 Blue theme appearance.

### Office2007 Color Schemes

The Office2007 style supports multiple color schemes.

#### Blue Color Scheme

```csharp
private void ApplyOffice2007Blue()
{
    NumericUpDownExt blueControl = new NumericUpDownExt();
    
    blueControl.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2007;
    blueControl.ColorScheme = Syncfusion.Windows.Forms.Office2007Theme.Blue;
    blueControl.Location = new Point(50, 30);
    blueControl.Size = new Size(120, 24);
    blueControl.Value = new decimal(100);
    
    Label lblBlue = new Label();
    lblBlue.Text = "Office 2007 Blue";
    lblBlue.Location = new Point(50, 10);
    lblBlue.AutoSize = true;
    
    this.Controls.Add(lblBlue);
    this.Controls.Add(blueControl);
}
```

**Result:** Professional Office 2007 Blue themed control.

#### Silver Color Scheme

```csharp
private void ApplyOffice2007Silver()
{
    NumericUpDownExt silverControl = new NumericUpDownExt();
    
    silverControl.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2007;
    silverControl.ColorScheme = Syncfusion.Windows.Forms.Office2007Theme.Silver;
    silverControl.Location = new Point(50, 70);
    silverControl.Size = new Size(120, 24);
    silverControl.Value = new decimal(200);
    
    Label lblSilver = new Label();
    lblSilver.Text = "Office 2007 Silver";
    lblSilver.Location = new Point(50, 50);
    lblSilver.AutoSize = true;
    
    this.Controls.Add(lblSilver);
    this.Controls.Add(silverControl);
}
```

**Result:** Sleek silver-themed control.

#### Black Color Scheme

```csharp
private void ApplyOffice2007Black()
{
    NumericUpDownExt blackControl = new NumericUpDownExt();
    
    blackControl.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2007;
    blackControl.ColorScheme = Syncfusion.Windows.Forms.Office2007Theme.Black;
    blackControl.Location = new Point(50, 110);
    blackControl.Size = new Size(120, 24);
    blackControl.Value = new decimal(300);
    
    Label lblBlack = new Label();
    lblBlack.Text = "Office 2007 Black";
    lblBlack.Location = new Point(50, 90);
    lblBlack.AutoSize = true;
    
    this.Controls.Add(lblBlack);
    this.Controls.Add(blackControl);
}
```

**Result:** Bold black-themed control.

### Office2007 Managed Colors

The Managed color scheme allows custom colors.

```csharp
private void ApplyManagedColors()
{
    NumericUpDownExt managedControl = new NumericUpDownExt();
    
    // Set to Managed color scheme
    managedControl.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2007;
    managedControl.ColorScheme = Syncfusion.Windows.Forms.Office2007Theme.Managed;
    
    // Apply custom color (Orange)
    Syncfusion.Windows.Forms.Office2007Colors.ApplyManagedColors(this, Color.Orange);
    
    managedControl.Location = new Point(50, 50);
    managedControl.Size = new Size(120, 24);
    managedControl.Value = new decimal(400);
    
    Label lblManaged = new Label();
    lblManaged.Text = "Office 2007 Managed (Orange)";
    lblManaged.Location = new Point(50, 30);
    lblManaged.AutoSize = true;
    
    this.Controls.Add(lblManaged);
    this.Controls.Add(managedControl);
}
```

**Result:** Custom orange-themed control using managed colors.

### Multiple Managed Color Examples

```csharp
private void DemonstrateCustomColors()
{
    // Purple theme
    NumericUpDownExt purpleControl = new NumericUpDownExt();
    purpleControl.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2007;
    purpleControl.ColorScheme = Syncfusion.Windows.Forms.Office2007Theme.Managed;
    purpleControl.Location = new Point(50, 30);
    purpleControl.Size = new Size(120, 24);
    Syncfusion.Windows.Forms.Office2007Colors.ApplyManagedColors(this, Color.Purple);
    
    // You can apply different colors for different forms/contexts
    // Note: ApplyManagedColors affects the entire form
    
    this.Controls.Add(purpleControl);
}
```

**Result:** Custom purple-themed appearance.

## Office 2016 Themes

Modern Office 2016 visual styles provide contemporary appearances.

### Office2016Colorful

```csharp
private void ApplyOffice2016Colorful()
{
    NumericUpDownExt colorfulControl = new NumericUpDownExt();
    
    // Apply Office 2016 Colorful theme
    colorfulControl.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2016Colorful;
    colorfulControl.Location = new Point(50, 30);
    colorfulControl.Size = new Size(120, 24);
    colorfulControl.Value = new decimal(500);
    
    Label lblColorful = new Label();
    lblColorful.Text = "Office 2016 Colorful";
    lblColorful.Location = new Point(50, 10);
    lblColorful.AutoSize = true;
    
    this.Controls.Add(lblColorful);
    this.Controls.Add(colorfulControl);
}
```

**Result:** Vibrant, modern Office 2016 Colorful theme.

### Office2016White

```csharp
private void ApplyOffice2016White()
{
    NumericUpDownExt whiteControl = new NumericUpDownExt();
    
    // Apply Office 2016 White theme
    whiteControl.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2016White;
    whiteControl.Location = new Point(50, 70);
    whiteControl.Size = new Size(120, 24);
    whiteControl.Value = new decimal(600);
    
    Label lblWhite = new Label();
    lblWhite.Text = "Office 2016 White";
    lblWhite.Location = new Point(50, 50);
    lblWhite.AutoSize = true;
    
    this.Controls.Add(lblWhite);
    this.Controls.Add(whiteControl);
}
```

**Result:** Clean, minimalist white theme.

### Office2016Black

```csharp
private void ApplyOffice2016Black()
{
    NumericUpDownExt blackControl = new NumericUpDownExt();
    
    // Apply Office 2016 Black theme
    blackControl.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2016Black;
    blackControl.Location = new Point(50, 110);
    blackControl.Size = new Size(120, 24);
    blackControl.Value = new decimal(700);
    
    Label lblBlack = new Label();
    lblBlack.Text = "Office 2016 Black";
    lblBlack.Location = new Point(50, 90);
    lblBlack.AutoSize = true;
    
    // Set form background to match
    this.BackColor = Color.FromArgb(40, 40, 40);
    lblBlack.ForeColor = Color.White;
    
    this.Controls.Add(lblBlack);
    this.Controls.Add(blackControl);
}
```

**Result:** Sophisticated dark theme for modern applications.

### Office2016DarkGray

```csharp
private void ApplyOffice2016DarkGray()
{
    NumericUpDownExt darkGrayControl = new NumericUpDownExt();
    
    // Apply Office 2016 Dark Gray theme
    darkGrayControl.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2016DarkGray;
    darkGrayControl.Location = new Point(50, 150);
    darkGrayControl.Size = new Size(120, 24);
    darkGrayControl.Value = new decimal(800);
    
    Label lblDarkGray = new Label();
    lblDarkGray.Text = "Office 2016 Dark Gray";
    lblDarkGray.Location = new Point(50, 130);
    lblDarkGray.AutoSize = true;
    
    this.Controls.Add(lblDarkGray);
    this.Controls.Add(darkGrayControl);
}
```

**Result:** Balanced dark gray theme.

## Theme Integration with Application

### Application-Wide Theme

```csharp
public class ThemedForm : Form
{
    private Syncfusion.Windows.Forms.VisualStyle appTheme = 
        Syncfusion.Windows.Forms.VisualStyle.Office2016Colorful;
    
    public ThemedForm()
    {
        InitializeComponent();
        ApplyApplicationTheme();
    }
    
    private void ApplyApplicationTheme()
    {
        // Apply theme to all NumericUpDownExt controls
        foreach (Control control in this.Controls)
        {
            if (control is NumericUpDownExt)
            {
                NumericUpDownExt numControl = (NumericUpDownExt)control;
                numControl.VisualStyle = appTheme;
            }
        }
    }
}
```

**Result:** Consistent theming across all controls in the form.

### Dynamic Theme Switching

```csharp
private void SetupThemeSwitcher()
{
    NumericUpDownExt targetControl = new NumericUpDownExt();
    targetControl.Location = new Point(50, 80);
    targetControl.Size = new Size(150, 24);
    targetControl.Value = new decimal(100);
    
    ComboBox themeSelector = new ComboBox();
    themeSelector.Location = new Point(50, 30);
    themeSelector.Size = new Size(200, 24);
    themeSelector.DropDownStyle = ComboBoxStyle.DropDownList;
    
    // Add theme options
    themeSelector.Items.Add("Default");
    themeSelector.Items.Add("Office 2007 Blue");
    themeSelector.Items.Add("Office 2007 Silver");
    themeSelector.Items.Add("Office 2007 Black");
    themeSelector.Items.Add("Office 2016 Colorful");
    themeSelector.Items.Add("Office 2016 White");
    themeSelector.Items.Add("Office 2016 Black");
    themeSelector.Items.Add("Office 2016 Dark Gray");
    themeSelector.SelectedIndex = 0;
    
    themeSelector.SelectedIndexChanged += (s, e) =>
    {
        switch (themeSelector.SelectedIndex)
        {
            case 0: // Default
                targetControl.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Default;
                break;
            case 1: // Office 2007 Blue
                targetControl.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2007;
                targetControl.ColorScheme = Syncfusion.Windows.Forms.Office2007Theme.Blue;
                break;
            case 2: // Office 2007 Silver
                targetControl.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2007;
                targetControl.ColorScheme = Syncfusion.Windows.Forms.Office2007Theme.Silver;
                break;
            case 3: // Office 2007 Black
                targetControl.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2007;
                targetControl.ColorScheme = Syncfusion.Windows.Forms.Office2007Theme.Black;
                break;
            case 4: // Office 2016 Colorful
                targetControl.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2016Colorful;
                break;
            case 5: // Office 2016 White
                targetControl.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2016White;
                break;
            case 6: // Office 2016 Black
                targetControl.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2016Black;
                break;
            case 7: // Office 2016 Dark Gray
                targetControl.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2016DarkGray;
                break;
        }
    };
    
    Label lblSelector = new Label();
    lblSelector.Text = "Select Theme:";
    lblSelector.Location = new Point(50, 10);
    lblSelector.AutoSize = true;
    
    this.Controls.Add(lblSelector);
    this.Controls.Add(themeSelector);
    this.Controls.Add(targetControl);
}
```

**Result:** Interactive theme switcher allowing users to preview all available themes.

## Style Best Practices

### Choosing the Right Theme

**Office 2007 Themes:**
- Use for applications matching Office 2007/2010 style
- Blue: Professional, traditional
- Silver: Modern, neutral
- Black: Bold, distinctive
- Managed: Custom branding

**Office 2016 Themes:**
- Use for modern, contemporary applications
- Colorful: Vibrant, energetic
- White: Clean, minimalist
- Black: Sophisticated, dramatic
- DarkGray: Balanced, professional

### Consistency Guidelines

```csharp
private void EnsureConsistency()
{
    // Choose ONE theme for your entire application
    var selectedTheme = Syncfusion.Windows.Forms.VisualStyle.Office2016Colorful;
    
    // Apply to ALL NumericUpDownExt controls
    NumericUpDownExt control1 = new NumericUpDownExt();
    control1.VisualStyle = selectedTheme;
    
    NumericUpDownExt control2 = new NumericUpDownExt();
    control2.VisualStyle = selectedTheme;
    
    NumericUpDownExt control3 = new NumericUpDownExt();
    control3.VisualStyle = selectedTheme;
    
    // Don't mix different themes in the same form
}
```

### Theme with Custom Colors

```csharp
private void CombineThemeAndCustom()
{
    NumericUpDownExt themedControl = new NumericUpDownExt();
    
    // Apply theme for overall style
    themedControl.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2016White;
    
    // Customize specific properties as needed
    themedControl.DecimalPlaces = 2;
    themedControl.ThousandsSeparator = true;
    themedControl.TextAlign = HorizontalAlignment.Right;
    
    // Note: Some custom colors may override theme colors
    // Use with caution to maintain theme consistency
    
    themedControl.Location = new Point(50, 50);
    this.Controls.Add(themedControl);
}
```

## Comparison with Standard NumericUpDown Theming

### Standard NumericUpDown Limitations

```csharp
// Standard .NET NumericUpDown
System.Windows.Forms.NumericUpDown standardControl = new System.Windows.Forms.NumericUpDown();
standardControl.Location = new Point(50, 30);

// Limited theming:
// - No XP Themes support
// - No Office visual styles
// - Basic color customization only
// - Does not match Windows theme automatically

Label lblStandard = new Label();
lblStandard.Text = "Standard (Limited Theming)";
lblStandard.Location = new Point(50, 10);
lblStandard.AutoSize = true;
```

### NumericUpDownExt Advantages

```csharp
// Syncfusion NumericUpDownExt
NumericUpDownExt syncfusionControl = new NumericUpDownExt();
syncfusionControl.Location = new Point(50, 80);

// Full theming support:
syncfusionControl.ThemesEnabled = true; // XP Themes support
syncfusionControl.ThemedBorder = true;  // Themed borders
syncfusionControl.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2016Colorful;

// Advantages:
// ✓ XP Themes support
// ✓ Office 2007/2016 visual styles
// ✓ Custom color schemes
// ✓ Themed borders
// ✓ Professional appearance

Label lblSyncfusion = new Label();
lblSyncfusion.Text = "NumericUpDownExt (Full Theming)";
lblSyncfusion.Location = new Point(50, 60);
lblSyncfusion.AutoSize = true;
```

## Complete Theme Gallery Example

```csharp
private void CreateThemeGallery()
{
    int yPosition = 30;
    int spacing = 50;
    
    // Array of themes to display
    var themes = new[]
    {
        ("Default", Syncfusion.Windows.Forms.VisualStyle.Default, null),
        ("Office 2007 Blue", Syncfusion.Windows.Forms.VisualStyle.Office2007, 
            Syncfusion.Windows.Forms.Office2007Theme.Blue),
        ("Office 2007 Silver", Syncfusion.Windows.Forms.VisualStyle.Office2007, 
            Syncfusion.Windows.Forms.Office2007Theme.Silver),
        ("Office 2007 Black", Syncfusion.Windows.Forms.VisualStyle.Office2007, 
            Syncfusion.Windows.Forms.Office2007Theme.Black),
        ("Office 2016 Colorful", Syncfusion.Windows.Forms.VisualStyle.Office2016Colorful, null),
        ("Office 2016 White", Syncfusion.Windows.Forms.VisualStyle.Office2016White, null),
        ("Office 2016 Black", Syncfusion.Windows.Forms.VisualStyle.Office2016Black, null),
        ("Office 2016 Dark Gray", Syncfusion.Windows.Forms.VisualStyle.Office2016DarkGray, null)
    };
    
    foreach (var theme in themes)
    {
        Label lblTheme = new Label();
        lblTheme.Text = theme.Item1;
        lblTheme.Location = new Point(30, yPosition);
        lblTheme.Size = new Size(150, 20);
        
        NumericUpDownExt control = new NumericUpDownExt();
        control.Location = new Point(190, yPosition);
        control.Size = new Size(120, 24);
        control.Value = new decimal(12345.67M);
        control.DecimalPlaces = 2;
        control.ThousandsSeparator = true;
        control.VisualStyle = theme.Item2;
        
        if (theme.Item3 != null)
        {
            control.ColorScheme = (Syncfusion.Windows.Forms.Office2007Theme)theme.Item3;
        }
        
        this.Controls.Add(lblTheme);
        this.Controls.Add(control);
        
        yPosition += spacing;
    }
    
    this.Text = "NumericUpDownExt Theme Gallery";
    this.ClientSize = new Size(350, yPosition + 20);
}
```

**Result:** Complete gallery showing all available themes side-by-side.

## Key Takeaways

1. **XP Themes Support** - Major advantage over standard NumericUpDown
2. **Office Visual Styles** - Professional, modern appearances
3. **Color Schemes** - Multiple built-in options plus custom colors
4. **Theme Consistency** - Use same theme across entire application
5. **ThemesEnabled Required** - Must be true for XP themes and themed borders
6. **Managed Colors** - Allow custom branding while maintaining Office style
