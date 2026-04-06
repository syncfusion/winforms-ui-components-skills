# Styling and Themes

This guide covers visual styling options for GridRecordNavigationControl, including built-in themes and appearance customization.

## Overview

GridRecordNavigationControl supports multiple visual styles to match your application's look and feel. You can apply modern themes like Metro and Office2016 with various color schemes.

## Available Visual Styles

The control supports these built-in visual styles through the `Style` property:

1. **Default** - Classic Windows Forms appearance
2. **Metro** - Modern flat design style
3. **Office2016** - Microsoft Office 2016 appearance with color schemes

## Style Property

Set the visual style using the `Style` property:

```csharp
using Syncfusion.Windows.Forms;

this.recordNavigationControl1.Style = Appearance.Metro;
```

**Available Values:**
- `Appearance.Default`
- `Appearance.Metro`
- `Appearance.Office2016`

## Metro Style

The Metro style provides a modern, flat design appearance.

### Applying Metro Style

**C# Example:**
```csharp
using Syncfusion.Windows.Forms;

this.recordNavigationControl1.Style = Appearance.Metro;
```

**VB.NET Example:**
```vb
Imports Syncfusion.Windows.Forms

Me.recordNavigationControl1.Style = Appearance.Metro
```

### Metro Style Characteristics

- Flat, minimalist design
- Clean lines and borders
- Modern color palette
- Reduced visual clutter
- Touch-friendly interface elements

### Complete Metro Example

```csharp
using Syncfusion.Windows.Forms;
using Syncfusion.Windows.Forms.Grid;
using System.Drawing;

private void ApplyMetroStyle()
{
    // Create navigation control
    var navControl = new GridRecordNavigationControl();
    navControl.Location = new Point(20, 20);
    navControl.Size = new Size(700, 450);
    
    // Apply Metro style
    navControl.Style = Appearance.Metro;
    
    // Create and add grid
    var gridControl = new GridControl();
    gridControl.RowCount = 100;
    gridControl.ColCount = 10;
    navControl.Controls.Add(gridControl);
    
    // Configure navigation
    navControl.MaxRecord = 100;
    navControl.MaxLabel = "of 100";
    
    this.Controls.Add(navControl);
}
```


## Office2016 Style

The Office2016 style provides a Microsoft Office 2016 appearance with multiple color schemes.

### Applying Office2016 Style

**Basic Office2016:**
```csharp
using Syncfusion.Windows.Forms;

this.recordNavigationControl1.Style = Appearance.Office2016;
```

### Office2016 Color Schemes

Office2016 style supports four color schemes through the `Office2016ScrollBarsColorScheme` property:

1. **Black** - Dark theme
2. **Colorful** - Accent colors
3. **DarkGray** - Medium dark theme
4. **White** - Light theme

### Complete Office2016 Configuration

To properly apply Office2016 styling, you need to set three properties:

```csharp
using Syncfusion.Windows.Forms;

// Set the visual style to Office2016
this.recordNavigationControl1.Style = Appearance.Office2016;

// Enable Office2016 scrollbars
this.recordNavigationControl1.GridOfficeScrollBars = OfficeScrollBars.Office2016;

// Apply color scheme
this.recordNavigationControl1.Office2016ScrollBarsColorScheme = 
    ScrollBarOffice2016ColorScheme.Black;
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms

' Apply Office 2016 Theme
Me.recordNavigationControl1.Style = Appearance.Office2016
Me.recordNavigationControl1.GridOfficeScrollBars = OfficeScrollBars.Office2016

' Apply Color Scheme
Me.recordNavigationControl1.Office2016ScrollBarsColorScheme = 
    ScrollBarOffice2016ColorScheme.Black
```

### Office2016 Color Scheme Examples

#### Black Color Scheme
```csharp
this.recordNavigationControl1.Style = Appearance.Office2016;
this.recordNavigationControl1.GridOfficeScrollBars = OfficeScrollBars.Office2016;
this.recordNavigationControl1.Office2016ScrollBarsColorScheme = 
    ScrollBarOffice2016ColorScheme.Black;
```

#### Colorful Color Scheme
```csharp
this.recordNavigationControl1.Style = Appearance.Office2016;
this.recordNavigationControl1.GridOfficeScrollBars = OfficeScrollBars.Office2016;
this.recordNavigationControl1.Office2016ScrollBarsColorScheme = 
    ScrollBarOffice2016ColorScheme.Colorful;
```

#### DarkGray Color Scheme
```csharp
this.recordNavigationControl1.Style = Appearance.Office2016;
this.recordNavigationControl1.GridOfficeScrollBars = OfficeScrollBars.Office2016;
this.recordNavigationControl1.Office2016ScrollBarsColorScheme = 
    ScrollBarOffice2016ColorScheme.DarkGray;
```

#### White Color Scheme
```csharp
this.recordNavigationControl1.Style = Appearance.Office2016;
this.recordNavigationControl1.GridOfficeScrollBars = OfficeScrollBars.Office2016;
this.recordNavigationControl1.Office2016ScrollBarsColorScheme = 
    ScrollBarOffice2016ColorScheme.White;
```


## Complete Styling Example

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;
using Syncfusion.Windows.Forms.Grid;

namespace StylingExample
{
    public partial class StyleForm : Form
    {
        private GridRecordNavigationControl navControl;
        private ComboBox styleSelector;
        private ComboBox colorSchemeSelector;
        
        public StyleForm()
        {
            InitializeComponent();
            InitializeControls();
            CreateStyleSelectors();
        }
        
        private void InitializeControls()
        {
            // Create navigation control
            navControl = new GridRecordNavigationControl
            {
                Location = new Point(10, 60),
                Size = new Size(760, 500)
            };
            
            // Create and add grid
            var gridControl = new GridControl
            {
                RowCount = 50,
                ColCount = 8
            };
            navControl.Controls.Add(gridControl);
            navControl.MaxRecord = 50;
            navControl.MaxLabel = "of 50";
            
            this.Controls.Add(navControl);
        }
        
        private void CreateStyleSelectors()
        {
            // Style selector
            styleSelector = new ComboBox
            {
                Location = new Point(10, 10),
                Width = 150,
                DropDownStyle = ComboBoxStyle.DropDownList
            };
            styleSelector.Items.AddRange(new[] { "Default", "Metro", "Office2016" });
            styleSelector.SelectedIndex = 0;
            styleSelector.SelectedIndexChanged += StyleSelector_Changed;
            
            // Color scheme selector (for Office2016)
            colorSchemeSelector = new ComboBox
            {
                Location = new Point(170, 10),
                Width = 150,
                DropDownStyle = ComboBoxStyle.DropDownList,
                Enabled = false
            };
            colorSchemeSelector.Items.AddRange(new[] { "Black", "Colorful", "DarkGray", "White" });
            colorSchemeSelector.SelectedIndex = 0;
            colorSchemeSelector.SelectedIndexChanged += ColorScheme_Changed;
            
            this.Controls.Add(styleSelector);
            this.Controls.Add(colorSchemeSelector);
        }
        
        private void StyleSelector_Changed(object sender, EventArgs e)
        {
            switch (styleSelector.SelectedItem.ToString())
            {
                case "Default":
                    navControl.Style = Appearance.Default;
                    colorSchemeSelector.Enabled = false;
                    break;
                    
                case "Metro":
                    navControl.Style = Appearance.Metro;
                    colorSchemeSelector.Enabled = false;
                    break;
                    
                case "Office2016":
                    navControl.Style = Appearance.Office2016;
                    navControl.GridOfficeScrollBars = OfficeScrollBars.Office2016;
                    colorSchemeSelector.Enabled = true;
                    ApplyColorScheme();
                    break;
            }
        }
        
        private void ColorScheme_Changed(object sender, EventArgs e)
        {
            if (navControl.Style == Appearance.Office2016)
            {
                ApplyColorScheme();
            }
        }
        
        private void ApplyColorScheme()
        {
            switch (colorSchemeSelector.SelectedItem.ToString())
            {
                case "Black":
                    navControl.Office2016ScrollBarsColorScheme = 
                        ScrollBarOffice2016ColorScheme.Black;
                    break;
                    
                case "Colorful":
                    navControl.Office2016ScrollBarsColorScheme = 
                        ScrollBarOffice2016ColorScheme.Colorful;
                    break;
                    
                case "DarkGray":
                    navControl.Office2016ScrollBarsColorScheme = 
                        ScrollBarOffice2016ColorScheme.DarkGray;
                    break;
                    
                case "White":
                    navControl.Office2016ScrollBarsColorScheme = 
                        ScrollBarOffice2016ColorScheme.White;
                    break;
            }
        }
    }
}
```

## Styling Properties Reference

### Style Property

**Type:** `Syncfusion.Windows.Forms.Appearance`

**Description:** Sets the visual theme of the control

**Values:**
- `Appearance.Default` - Classic Windows Forms
- `Appearance.Metro` - Modern flat design
- `Appearance.Office2016` - Office 2016 style

**Example:**
```csharp
navControl.Style = Appearance.Office2016;
```

### GridOfficeScrollBars Property

**Type:** `Syncfusion.Windows.Forms.OfficeScrollBars`

**Description:** Enables Office-style scrollbars (required for Office2016 style)

**Values:**
- `OfficeScrollBars.None` - Standard scrollbars
- `OfficeScrollBars.Office2007` - Office 2007 style
- `OfficeScrollBars.Office2010` - Office 2010 style
- `OfficeScrollBars.Office2016` - Office 2016 style
- `OfficeScrollBars.Metro` - Metro style

**Example:**
```csharp
navControl.GridOfficeScrollBars = OfficeScrollBars.Office2016;
```

### Office2016ScrollBarsColorScheme Property

**Type:** `Syncfusion.Windows.Forms.ScrollBarOffice2016ColorScheme`

**Description:** Sets the color scheme for Office2016 style

**Values:**
- `ScrollBarOffice2016ColorScheme.Black` - Dark theme
- `ScrollBarOffice2016ColorScheme.Colorful` - Accent colors
- `ScrollBarOffice2016ColorScheme.DarkGray` - Medium dark
- `ScrollBarOffice2016ColorScheme.White` - Light theme

**Example:**
```csharp
navControl.Office2016ScrollBarsColorScheme = ScrollBarOffice2016ColorScheme.Black;
```

## Best Practices

### Consistent Styling Across Application

Apply the same style to all Syncfusion controls in your application:

```csharp
private void ApplyApplicationTheme(Appearance style)
{
    // Apply to all navigation controls
    foreach (Control control in this.Controls)
    {
        if (control is GridRecordNavigationControl navCtrl)
        {
            navCtrl.Style = style;
            
            if (style == Appearance.Office2016)
            {
                navCtrl.GridOfficeScrollBars = OfficeScrollBars.Office2016;
                navCtrl.Office2016ScrollBarsColorScheme = 
                    ScrollBarOffice2016ColorScheme.Black;
            }
        }
    }
}
```

### Dynamic Style Switching

Allow users to change themes at runtime:

```csharp
private void ApplyUserTheme(string themeName)
{
    switch (themeName.ToLower())
    {
        case "dark":
            navControl.Style = Appearance.Office2016;
            navControl.GridOfficeScrollBars = OfficeScrollBars.Office2016;
            navControl.Office2016ScrollBarsColorScheme = 
                ScrollBarOffice2016ColorScheme.Black;
            break;
            
        case "light":
            navControl.Style = Appearance.Office2016;
            navControl.GridOfficeScrollBars = OfficeScrollBars.Office2016;
            navControl.Office2016ScrollBarsColorScheme = 
                ScrollBarOffice2016ColorScheme.White;
            break;
            
        case "modern":
            navControl.Style = Appearance.Metro;
            break;
            
        default:
            navControl.Style = Appearance.Default;
            break;
    }
}
```

## Troubleshooting

### Office2016 Style Not Applied

**Problem:** Control doesn't show Office2016 appearance

**Solution:** Ensure you set all three required properties:
```csharp
// All three are required
navControl.Style = Appearance.Office2016;
navControl.GridOfficeScrollBars = OfficeScrollBars.Office2016;
navControl.Office2016ScrollBarsColorScheme = ScrollBarOffice2016ColorScheme.Black;
```

### Color Scheme Not Changing

**Problem:** Color scheme property has no effect

**Solution:** Verify Style is set to Office2016 first:
```csharp
if (navControl.Style == Appearance.Office2016)
{
    navControl.Office2016ScrollBarsColorScheme = /* desired scheme */;
}
```

### Inconsistent Appearance

**Problem:** Control appearance doesn't match other controls

**Solution:** Apply same style settings to all Syncfusion controls in your form

## Style Comparison Table

| Style | Appearance | Best For | Color Schemes |
|-------|-----------|----------|---------------|
| Default | Classic Windows Forms | Traditional applications | None |
| Metro | Flat, modern | Touch interfaces, modern apps | None |
| Office2016 | Microsoft Office look | Business applications | Black, Colorful, DarkGray, White |

## Quick Reference

```csharp
// Default style
navControl.Style = Appearance.Default;

// Metro style
navControl.Style = Appearance.Metro;

// Office2016 Black
navControl.Style = Appearance.Office2016;
navControl.GridOfficeScrollBars = OfficeScrollBars.Office2016;
navControl.Office2016ScrollBarsColorScheme = ScrollBarOffice2016ColorScheme.Black;

// Office2016 White
navControl.Style = Appearance.Office2016;
navControl.GridOfficeScrollBars = OfficeScrollBars.Office2016;
navControl.Office2016ScrollBarsColorScheme = ScrollBarOffice2016ColorScheme.White;
```

## Next Steps

- Apply consistent styling across your application
- Test different themes to find the best fit
- Consider user preferences for theme selection
- Explore custom styling options beyond built-in themes
