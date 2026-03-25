# Advanced Customization

## Table of Contents
- [ThemeStyle Property](#themestyle-property)
- [Style Property for Sf Controls](#style-property-for-sf-controls)
- [CanOverrideStyle Property](#canoverridestyle-property)
- [FontHelper Customization](#fonthelper-customization)
- [Custom Styling Examples](#custom-styling-examples)

This guide covers advanced theme customization techniques for fine-tuning the appearance of themed controls beyond base theme colors.

## ThemeStyle Property

The `ThemeStyle` property allows you to customize the appearance of individual controls when using Theme Studio-based themes (Office2019Colorful, HighContrastBlack).

### When to Use ThemeStyle

- **Requirement**: Using Theme Studio-based themes (Office2019Colorful or HighContrastBlack)
- **Control Type**: Non-Sf-prefixed controls (TreeViewAdv, RibbonControlAdv, etc.)
- **Purpose**: Override specific appearance properties while keeping base theme

### Important Limitations

**⚠️ Appearance Customization Restrictions:**
- ThemeStyle/Style customization ONLY works with Theme Studio-based themes
- Does NOT work with built-in themes (Office2007, Office2010, Office2013, Office2016, Metro)
- For built-in themes, control-level appearance settings are ignored

### Basic ThemeStyle Usage

```csharp
// Apply Theme Studio theme first
this.treeViewAdv1.ThemeName = "Office2019Colorful";

// Then customize using ThemeStyle
this.treeViewAdv1.ThemeStyle.BackColor = System.Drawing.Color.White;
this.treeViewAdv1.ThemeStyle.BorderColor = System.Drawing.Color.SteelBlue;
this.treeViewAdv1.ThemeStyle.LineColor = System.Drawing.Color.DarkBlue;
```

```vb
' VB.NET
' Apply Theme Studio theme first
Me.treeViewAdv1.ThemeName = "Office2019Colorful"

' Then customize using ThemeStyle
Me.treeViewAdv1.ThemeStyle.BackColor = System.Drawing.Color.White
Me.treeViewAdv1.ThemeStyle.BorderColor = System.Drawing.Color.SteelBlue
Me.treeViewAdv1.ThemeStyle.LineColor = System.Drawing.Color.DarkBlue
```

### TreeViewAdv ThemeStyle Properties

```csharp
// TreeViewAdv appearance customization
this.treeViewAdv1.ThemeStyle.BackColor = System.Drawing.Color.White;
this.treeViewAdv1.ThemeStyle.BorderColor = System.Drawing.Color.SteelBlue;
this.treeViewAdv1.ThemeStyle.LineColor = System.Drawing.Color.DarkBlue;

// Node-specific styling
this.treeViewAdv1.ThemeStyle.TreeNodeAdvStyle.ArrowColor = System.Drawing.Color.Aqua;
this.treeViewAdv1.ThemeStyle.TreeNodeAdvStyle.TextColor = System.Drawing.Color.Red;
this.treeViewAdv1.ThemeStyle.TreeNodeAdvStyle.HoverTextColor = System.Drawing.Color.Blue;
this.treeViewAdv1.ThemeStyle.TreeNodeAdvStyle.SelectedTextColor = System.Drawing.Color.White;
```

### Common ThemeStyle Properties

Different controls expose different ThemeStyle properties. Common patterns:

**Background and Borders:**
```csharp
control.ThemeStyle.BackColor = Color.White;
control.ThemeStyle.ForeColor = Color.Black;
control.ThemeStyle.BorderColor = Color.Gray;
```

**Hover and Selection:**
```csharp
control.ThemeStyle.HoverColor = Color.LightBlue;
control.ThemeStyle.SelectedColor = Color.Blue;
control.ThemeStyle.HoverTextColor = Color.White;
```

**Disabled State:**
```csharp
control.ThemeStyle.DisabledBackColor = Color.LightGray;
control.ThemeStyle.DisabledForeColor = Color.DarkGray;
```

## Style Property for Sf Controls

Controls with names starting with **"Sf"** (like SfDataGrid, SfButton, SfComboBox) use the `Style` property instead of `ThemeStyle`.

### When to Use Style Property

- **Control Naming**: Control name starts with "Sf" (SfDataGrid, SfButton, etc.)
- **Requirement**: Using Theme Studio-based themes
- **Purpose**: Same as ThemeStyle, but for Sf-prefixed controls

### Basic Style Usage

```csharp
// Apply theme first
this.sfDataGrid1.ThemeName = "Office2019Colorful";

// Customize using Style property
this.sfDataGrid1.Style.BackColor = System.Drawing.Color.White;
this.sfDataGrid1.Style.BorderColor = System.Drawing.Color.Navy;
this.sfDataGrid1.Style.HeaderStyle.BackColor = System.Drawing.Color.LightBlue;
```

```vb
' VB.NET
' Apply theme first
Me.sfDataGrid1.ThemeName = "Office2019Colorful"

' Customize using Style property
Me.sfDataGrid1.Style.BackColor = System.Drawing.Color.White
Me.sfDataGrid1.Style.BorderColor = System.Drawing.Color.Navy
Me.sfDataGrid1.Style.HeaderStyle.BackColor = System.Drawing.Color.LightBlue
```

### SfDataGrid Style Example

```csharp
// SfDataGrid comprehensive styling
this.sfDataGrid1.ThemeName = "Office2019Colorful";

// Grid appearance
this.sfDataGrid1.Style.BackColor = Color.White;
this.sfDataGrid1.Style.AlternatingRowStyle.BackColor = Color.LightGray;

// Header styling
this.sfDataGrid1.Style.HeaderStyle.BackColor = Color.Navy;
this.sfDataGrid1.Style.HeaderStyle.TextColor = Color.White;
this.sfDataGrid1.Style.HeaderStyle.Font = new Font("Segoe UI", 10, FontStyle.Bold);

// Cell styling
this.sfDataGrid1.Style.CellStyle.TextColor = Color.Black;
this.sfDataGrid1.Style.CellStyle.Font = new Font("Segoe UI", 9);

// Selection colors
this.sfDataGrid1.Style.SelectionStyle.BackColor = Color.DodgerBlue;
this.sfDataGrid1.Style.SelectionStyle.TextColor = Color.White;
```

### SfButton Style Example

```csharp
// SfButton styling
this.sfButton1.ThemeName = "Office2019Colorful";

// Button appearance
this.sfButton1.Style.BackColor = Color.DodgerBlue;
this.sfButton1.Style.ForeColor = Color.White;
this.sfButton1.Style.Border = new Pen(Color.Navy, 2);

// Hover state
this.sfButton1.Style.HoverBackColor = Color.RoyalBlue;
this.sfButton1.Style.HoverForeColor = Color.White;

// Pressed state
this.sfButton1.Style.PressedBackColor = Color.MidnightBlue;
```

## CanOverrideStyle Property

The `CanOverrideStyle` property controls whether applying a theme overrides existing appearance customizations.

### Understanding CanOverrideStyle

**Default Value:** `false`

**Behavior:**
- `false` (default): Theme does NOT override user customizations
- `true`: Theme overrides all user customizations

### Use Case: Preserve Customizations

```csharp
// Customize appearance
this.treeViewAdv1.ThemeStyle.TreeNodeAdvStyle.TextColor = System.Drawing.Color.Red;

// Apply theme WITHOUT overriding customization
this.treeViewAdv1.CanOverrideStyle = false; // Default behavior
this.treeViewAdv1.ThemeName = "Office2019Colorful";

// Result: TextColor remains Red (custom setting preserved)
```

### Use Case: Theme Takes Priority

```csharp
// Customize appearance first
this.treeViewAdv1.ThemeStyle.TreeNodeAdvStyle.TextColor = System.Drawing.Color.Red;

// Apply theme and OVERRIDE customization
this.treeViewAdv1.CanOverrideStyle = true;
this.treeViewAdv1.ThemeName = "Office2019Colorful";

// Result: TextColor uses theme's default (custom setting overridden)
```

### Typical Workflow

**Recommended Pattern:**
1. Set `CanOverrideStyle = false` (or leave default)
2. Apply theme using `ThemeName`
3. Customize appearance using `ThemeStyle` or `Style`
4. Theme provides base styling, customizations add specific touches

```csharp
// Recommended workflow
this.treeViewAdv1.CanOverrideStyle = false; // Preserve customizations
this.treeViewAdv1.ThemeName = "Office2019Colorful"; // Base theme
this.treeViewAdv1.ThemeStyle.TreeNodeAdvStyle.TextColor = Color.Red; // Custom touch
```

## FontHelper Customization

When using Theme Studio-based themes, you can customize the font family application-wide using the `FontHelper` class.

### FontHelper Properties

```csharp
FontHelper.CanOverrideFontFamily  // Enable/disable font override
FontHelper.FontFamily              // Set custom font family
```

### Enabling Font Override

**Important:** Set `CanOverrideFontFamily` in the **constructor** before initializing components.

```csharp
using Syncfusion.WinForms.Theme;
using System.Drawing;

public partial class Form1 : Form
{
    public Form1()
    {
        // Enable font override (in constructor, before InitializeComponent)
        FontHelper.CanOverrideFontFamily = true;
        FontHelper.FontFamily = new FontFamily("Algerian");
        
        InitializeComponent();
        
        // Apply theme
        SkinManager skinManager1 = new SkinManager(this.components);
        skinManager1.Controls = this;
        this.ThemeName = "Office2019Colorful";
    }
}
```

```vb
' VB.NET
Imports Syncfusion.WinForms.Theme
Imports System.Drawing

Public Partial Class Form1
    Inherits Form
    
    Public Sub New()
        ' Enable font override (in constructor, before InitializeComponent)
        FontHelper.CanOverrideFontFamily = True
        FontHelper.FontFamily = New FontFamily("Algerian")
        
        InitializeComponent()
        
        ' Apply theme
        Dim skinManager1 As New SkinManager(Me.components)
        skinManager1.Controls = Me
        Me.ThemeName = "Office2019Colorful"
    End Sub
End Class
```

### Common Font Customizations

```csharp
// Segoe UI (Modern Windows look)
FontHelper.FontFamily = new FontFamily("Segoe UI");

// Arial (Classic, readable)
FontHelper.FontFamily = new FontFamily("Arial");

// Calibri (Office-style)
FontHelper.FontFamily = new FontFamily("Calibri");

// Consolas (Monospace, code-style)
FontHelper.FontFamily = new FontFamily("Consolas");

// Custom brand font
FontHelper.FontFamily = new FontFamily("YourCustomFont");
```

### Application-Wide Font Example

```csharp
// Program.cs
static class Program
{
    [STAThread]
    static void Main()
    {
        // Set application-wide font FIRST
        FontHelper.CanOverrideFontFamily = true;
        FontHelper.FontFamily = new FontFamily("Segoe UI");
        
        // Then load theme and run app
        SkinManager.LoadAssembly(typeof(Office2019Theme).Assembly);
        SkinManager.ApplicationVisualTheme = "Office2019Colorful";
        
        Application.EnableVisualStyles();
        Application.Run(new Form1());
    }
}
```

## Custom Styling Examples

### Example 1: Dark Theme with Custom Accents

```csharp
// Apply dark theme
this.treeViewAdv1.ThemeName = "Office2019Colorful"; // Or use custom dark theme

// Customize with brand colors
this.treeViewAdv1.ThemeStyle.BackColor = Color.FromArgb(30, 30, 30);
this.treeViewAdv1.ThemeStyle.BorderColor = Color.FromArgb(0, 120, 215); // Blue accent
this.treeViewAdv1.ThemeStyle.LineColor = Color.FromArgb(60, 60, 60);

// Node styling
this.treeViewAdv1.ThemeStyle.TreeNodeAdvStyle.TextColor = Color.White;
this.treeViewAdv1.ThemeStyle.TreeNodeAdvStyle.HoverTextColor = Color.FromArgb(0, 120, 215);
this.treeViewAdv1.ThemeStyle.TreeNodeAdvStyle.SelectedTextColor = Color.White;
```

### Example 2: High Contrast Accessibility Theme

```csharp
// Apply high contrast base
this.ThemeName = "HighContrastBlack";

// Enhance contrast further
this.treeViewAdv1.ThemeStyle.BackColor = Color.Black;
this.treeViewAdv1.ThemeStyle.BorderColor = Color.Yellow;
this.treeViewAdv1.ThemeStyle.TreeNodeAdvStyle.TextColor = Color.Yellow;
this.treeViewAdv1.ThemeStyle.TreeNodeAdvStyle.SelectedTextColor = Color.White;

// Use large, clear font
FontHelper.CanOverrideFontFamily = true;
FontHelper.FontFamily = new FontFamily("Arial");
```

### Example 3: Corporate Branding with Custom Colors

```csharp
// Corporate color scheme
Color corporatePrimary = Color.FromArgb(0, 102, 204);   // Blue
Color corporateAccent = Color.FromArgb(255, 215, 0);     // Gold
Color corporateBackground = Color.FromArgb(248, 248, 248); // Light gray

// Apply to SfDataGrid
this.sfDataGrid1.ThemeName = "Office2019Colorful";
this.sfDataGrid1.Style.BackColor = corporateBackground;
this.sfDataGrid1.Style.HeaderStyle.BackColor = corporatePrimary;
this.sfDataGrid1.Style.HeaderStyle.TextColor = Color.White;
this.sfDataGrid1.Style.SelectionStyle.BackColor = corporateAccent;
this.sfDataGrid1.Style.SelectionStyle.TextColor = Color.Black;

// Apply to SfButton
this.sfButton1.ThemeName = "Office2019Colorful";
this.sfButton1.Style.BackColor = corporatePrimary;
this.sfButton1.Style.ForeColor = Color.White;
this.sfButton1.Style.HoverBackColor = corporateAccent;
this.sfButton1.Style.HoverForeColor = Color.Black;
```

### Example 4: Mixed Control Styling

```csharp
public Form1()
{
    InitializeComponent();
    
    // Set application font
    FontHelper.CanOverrideFontFamily = true;
    FontHelper.FontFamily = new FontFamily("Segoe UI");
    
    // Apply theme to form
    SkinManager skinManager1 = new SkinManager(this.components);
    skinManager1.Controls = this;
    this.ThemeName = "Office2019Colorful";
    
    // Customize TreeView (non-Sf control)
    this.treeViewAdv1.ThemeStyle.BackColor = Color.White;
    this.treeViewAdv1.ThemeStyle.BorderColor = Color.LightBlue;
    
    // Customize SfDataGrid (Sf control)
    this.sfDataGrid1.Style.HeaderStyle.BackColor = Color.Navy;
    this.sfDataGrid1.Style.HeaderStyle.TextColor = Color.White;
    
    // Customize SfButton (Sf control)
    this.sfButton1.Style.BackColor = Color.Green;
    this.sfButton1.Style.ForeColor = Color.White;
}
```

## Best Practices

1. **Apply Theme First**: Always set ThemeName before customizing ThemeStyle/Style
2. **Use Theme Studio Themes**: Customization only works with Office2019Colorful and HighContrastBlack
3. **Preserve Customizations**: Use `CanOverrideStyle = false` (default) to keep custom styling
4. **Font Override Early**: Set FontHelper properties in constructor before InitializeComponent
5. **Consistent Styling**: Maintain consistent color scheme across all controls
6. **Test Accessibility**: Verify color contrast meets accessibility standards

## Troubleshooting

**Issue:** ThemeStyle/Style customization not applying  
**Solution:** Verify using Theme Studio-based theme (Office2019Colorful or HighContrastBlack), not built-in themes

**Issue:** Custom colors overridden by theme  
**Solution:** Set `CanOverrideStyle = false` before applying theme

**Issue:** Font not changing  
**Solution:** Set `FontHelper.CanOverrideFontFamily = true` in constructor before InitializeComponent

**Issue:** Some controls styled, others not  
**Solution:** Use ThemeStyle for non-Sf controls, Style for Sf-prefixed controls

**Issue:** Can't access ThemeStyle property  
**Solution:** Ensure using Theme Studio compatible theme; property not available with built-in themes
