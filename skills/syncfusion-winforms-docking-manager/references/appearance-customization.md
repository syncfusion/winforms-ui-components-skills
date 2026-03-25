# Appearance Customization

The DockingManager provides extensive visual customization options including themes, colors, fonts, and borders. This guide covers all appearance settings.

## Table of Contents
- [Visual Styles](#visual-styles)
- [Office Themes](#office-themes)
- [Custom Color Schemes](#custom-color-schemes)
- [Caption Customization](#caption-customization)
- [Tab Customization](#tab-customization)
- [Document Tab Customization](#document-tab-customization)
- [Auto-Hide Tab Customization](#auto-hide-tab-customization)
- [Metro Customization](#metro-customization)
- [Border Customization](#border-customization)
- [Splitter Customization](#splitter-customization)
- [Drag Provider Styles](#drag-provider-styles)
- [Right-to-Left Support](#right-to-left-support)

## Visual Styles

### Set Visual Style

```csharp
// Apply visual style (affects entire DockingManager)
this.dockingManager1.VisualStyle = VisualStyle.Metro;
```

**Available Visual Styles:**
- `Default` - Standard Windows style
- `OfficeXP` - Office XP look
- `Office2003` - Office 2003 look
- `Office2007` - Office 2007 Ribbon UI style
- `Office2007Outlook` - Outlook 2007 style
- `Office2010` - Office 2010 style
- `VS2003` - Visual Studio 2003 style
- `VS2005` - Visual Studio 2005 style
- `VS2010` - Visual Studio 2010 style
- `Metro` - Modern flat design
- `Office2016Colorful` - Office 2016 colorful theme
- `Office2016White` - Office 2016 white theme
- `Office2016DarkGray` - Office 2016 dark gray theme
- `Office2016Black` - Office 2016 black theme

**VB.NET:**

```vb
' Set visual style
Me.dockingManager1.VisualStyle = VisualStyle.Office2016Colorful
```

### Visual Style Examples

```csharp
// Modern flat appearance
this.dockingManager1.VisualStyle = VisualStyle.Metro;

// Professional Office look
this.dockingManager1.VisualStyle = VisualStyle.Office2016Colorful;

// Dark theme
this.dockingManager1.VisualStyle = VisualStyle.Office2016Black;

// Classic Visual Studio
this.dockingManager1.VisualStyle = VisualStyle.VS2010;
```

## Office Themes

### Office 2007 Themes

```csharp
// Set base style
this.dockingManager1.VisualStyle = VisualStyle.Office2007;

// Set theme color
this.dockingManager1.Office2007Theme = Office2007Theme.Blue;    // Default
// Or: Silver, Black, Managed
```

**Available Office 2007 Themes:**
- `Blue` - Blue color scheme (default)
- `Silver` - Silver/gray color scheme
- `Black` - Black color scheme
- `Managed` - Custom colors (use `ApplyManagedColors`)

### Office 2010 Themes

```csharp
// Set base style
this.dockingManager1.VisualStyle = VisualStyle.Office2010;

// Set theme color
this.dockingManager1.Office2010Theme = Office2010Theme.Blue;
// Or: Silver, Black, Managed
```

**VB.NET:**

```vb
' Office 2010 Silver theme
Me.dockingManager1.VisualStyle = VisualStyle.Office2010
Me.dockingManager1.Office2010Theme = Office2010Theme.Silver
```

## Custom Color Schemes

### Office 2007 Custom Colors

```csharp
using Syncfusion.Windows.Forms.Tools;

// Enable managed color scheme
this.dockingManager1.VisualStyle = VisualStyle.Office2007;
this.dockingManager1.Office2007Theme = Office2007Theme.Managed;

// Create custom color table
Office2007Colors.ApplyManagedColors(this, Color.DarkBlue);
```

### Office 2010 Custom Colors

```csharp
// Enable managed colors
this.dockingManager1.VisualStyle = VisualStyle.Office2010;
this.dockingManager1.Office2010Theme = Office2010Theme.Managed;

// Apply custom color scheme
Office2010Colors.ApplyManagedColors(this, Color.Teal);
```

## Caption Customization

### Caption Background Colors

```csharp
// Active window caption background
this.dockingManager1.ActiveCaptionBackground = 
    new BrushInfo(Color.DarkBlue);

// Inactive window caption background
this.dockingManager1.InActiveCaptionBackground = 
    new BrushInfo(Color.LightGray);
```

**Using Gradients:**

```csharp
using Syncfusion.Drawing;

// Active caption with gradient
this.dockingManager1.ActiveCaptionBackground = new BrushInfo(
    GradientStyle.Vertical,
    new Color[] { Color.DarkBlue, Color.LightBlue }
);

// Inactive caption with gradient
this.dockingManager1.InActiveCaptionBackground = new BrushInfo(
    GradientStyle.Vertical,
    new Color[] { Color.Gray, Color.LightGray }
);
```

### Caption Foreground Colors

```csharp
// Active window caption text color
this.dockingManager1.ActiveCaptionForeGround = Color.White;

// Inactive window caption text color
this.dockingManager1.InActiveCaptionForeGround = Color.DarkGray;
```

**VB.NET:**

```vb
' Caption colors
Me.dockingManager1.ActiveCaptionForeGround = Color.White
Me.dockingManager1.InActiveCaptionForeGround = Color.DarkGray
```

### Caption Fonts

```csharp
// Active window caption font
this.dockingManager1.ActiveCaptionFont = 
    new Font("Segoe UI", 10f, FontStyle.Bold);

// Inactive window caption font
this.dockingManager1.InActiveCaptionFont = 
    new Font("Segoe UI", 9f, FontStyle.Regular);
```

### Caption Button Colors

```csharp
// Active window caption button color
this.dockingManager1.ActiveCaptionButtonForeColor = Color.White;

// Inactive window caption button color
this.dockingManager1.InActiveCaptionButtonForeColor = Color.Gray;
```

### Caption Height

```csharp
// Set caption height (pixels)
this.dockingManager1.CaptionHeight = 30; // Default is about 22

// Maximum value
this.dockingManager1.CaptionHeight = 60;
```

Note: Not applicable for Default and VS2005 visual styles.

### Hide Captions

```csharp
// Hide all captions
this.dockingManager1.ShowCaption = false;

// Hide caption for specific control
this.dockingManager1.SetCaptionVisibility(panel1, false);
```

### Caption Text Alignment

```csharp
// Set caption label alignment
this.dockingManager1.DockLabelAlignment = DockLabelAlignment.Left;    // Default
// Or: Center, Right
```

## Tab Customization

### Dock Tab Fonts and Size

```csharp
// Tab font
this.dockingManager1.DockTabFont = 
    new Font("Segoe UI", 9f, FontStyle.Regular);

// Tab height
this.dockingManager1.DockTabHeight = 26; // Default ~22
```

### Dock Tab Colors

```csharp
// Active tab colors
this.dockingManager1.ActiveDockTabForeColor = Color.White;
this.dockingManager1.ActiveDockTabBackColor = Color.DarkBlue;

// Inactive tab colors
this.dockingManager1.DockTabForeColor = Color.Black;
this.dockingManager1.DockTabBackColor = Color.LightGray;

// Tab panel background
this.dockingManager1.DockTabPanelBackColor = Color.WhiteSmoke;

// Tab separator color
this.dockingManager1.DockTabSeparatorColor = Color.Gray;
```

**VB.NET:**

```vb
' Customize dock tabs
Me.dockingManager1.DockTabFont = New Font("Segoe UI", 9.0F)
Me.dockingManager1.ActiveDockTabForeColor = Color.White
Me.dockingManager1.ActiveDockTabBackColor = Color.DarkBlue
```

## Document Tab Customization

Document tabs are used when EnableDocumentMode is true (TDI mode).

### Document Tab Fonts and Size

```csharp
// Tab font
this.dockingManager1.DocumentWindowSettings.TabFont = 
    new Font("Segoe UI", 9f, FontStyle.Regular);

// Active tab font
this.dockingManager1.DocumentWindowSettings.ActiveTabFont = 
    new Font("Segoe UI", 9f, FontStyle.Bold);

// Tab height
this.dockingManager1.DocumentWindowSettings.TabHeight = 28;
```

### Document Tab Colors

```csharp
using Syncfusion.Windows.Forms.Tools;

// Active tab colors
this.dockingManager1.DocumentWindowSettings.ActiveTabForeColor = Color.White;
this.dockingManager1.DocumentWindowSettings.ActiveTabBackColor = 
    Color.FromArgb(0, 122, 204);

// Inactive tab colors
this.dockingManager1.DocumentWindowSettings.TabForeColor = Color.Black;
this.dockingManager1.DocumentWindowSettings.TabBackColor = 
    Color.FromArgb(240, 240, 240);

// Tab panel background
this.dockingManager1.DocumentWindowSettings.TabPanelBackColor = 
    Color.FromArgb(245, 245, 245);

// Tab panel border
this.dockingManager1.DocumentWindowSettings.TabPanelBorderColor = 
    Color.FromArgb(200, 200, 200);
```

**VB.NET:**

```vb
' Document tab colors
Me.dockingManager1.DocumentWindowSettings.ActiveTabForeColor = Color.White
Me.dockingManager1.DocumentWindowSettings.ActiveTabBackColor = Color.DarkBlue
```

## Auto-Hide Tab Customization

### Auto-Hide Tab Font and Size

```csharp
// Auto-hide tab font
this.dockingManager1.AutoHideTabFont = 
    new Font("Segoe UI", 9f, FontStyle.Regular);

// Tab height
this.dockingManager1.AutoHideTabHeight = 24; // Default ~20

// Tab foreground color
this.dockingManager1.AutoHideTabForeColor = Color.Navy;
```

### Auto-Hide Tab Background

```csharp
// Get auto-hide tab control for left side
Control leftTabs = this.dockingManager1.GetAHTabControl(
    Syncfusion.Windows.Forms.Tools.DockTabAlignmentStyle.Left);

if (leftTabs != null)
{
    leftTabs.BackColor = Color.LightGray;
}

// For other sides: Right, Top, Bottom
Control rightTabs = this.dockingManager1.GetAHTabControl(
    Syncfusion.Windows.Forms.Tools.DockTabAlignmentStyle.Right);
if (rightTabs != null)
    rightTabs.BackColor = Color.LightGreen;
```

**VB.NET:**

```vb
' Customize auto-hide tabs
Me.dockingManager1.AutoHideTabFont = New Font("Segoe UI", 9.0F)
Me.dockingManager1.AutoHideTabForeColor = Color.Navy
Me.dockingManager1.AutoHideTabHeight = 24
```

### Full Captions in Auto-Hide

```csharp
// Display full horizontal captions instead of vertical text
this.dockingManager1.FullCaptionsInAutoHideMode = true;
```

## Metro Customization

### Metro Border Width

```csharp
// Set Metro border width
this.dockingManager1.VisualStyle = VisualStyle.Metro;
this.dockingManager1.MetroBorderWidth = 2; // Default is 1
```

### Metro Colors

```csharp
// Metro caption color
this.dockingManager1.MetroCaptionColor = Color.DarkSlateGray;

// Metro button color
this.dockingManager1.MetroButtonColor = Color.Teal;

// Metro accent color
this.dockingManager1.MetroColor = Color.DeepSkyBlue;
```

### Metro Caption Dotted Lines

```csharp
// Show dotted lines in Metro caption
this.dockingManager1.ShowMetroCaptionDottedLines = true;
```

## Border Customization

### Border Color

```csharp
// Set border color for docked windows
this.dockingManager1.BorderColor = Color.DarkGray;
```

### Paint Borders

```csharp
// Enable/disable border painting
this.dockingManager1.PaintBorders = true; // Default

// Disable borders
this.dockingManager1.PaintBorders = false;
```

### Host Form Client Border

```csharp
// Border style for main form client area
this.dockingManager1.HostFormClientBorder = 
    Syncfusion.Windows.Forms.Tools.FormBorderStyle.Sunken; // Default
// Or: None, Raised, Etched
```

## Splitter Customization

### Splitter Width

```csharp
// Set splitter width between docked windows
this.dockingManager1.SplitterWidth = 6; // Default is 4
```

### Metro Splitter Color

```csharp
// Splitter color for Metro style
this.dockingManager1.VisualStyle = VisualStyle.Metro;
this.dockingManager1.MetroSplitterBackColor = Color.DarkGray;
```

## Drag Provider Styles

```csharp
// Set drag provider style (visual feedback during drag)
this.dockingManager1.DragProviderStyle = DragProviderStyle.VS2012;
```

**Available Drag Provider Styles:**
- `Standard` - Basic drag style
- `VS2005` - Visual Studio 2005 drag hints
- `VS2008` - Visual Studio 2008 drag hints
- `VS2010` - Visual Studio 2010 drag hints
- `VS2012` - Visual Studio 2012 drag hints
- `Whidbey` - Whidbey drag style
- `Office2007` - Office 2007 drag hints
- `Office2016Colorful` - Office 2016 colorful drag hints
- `Office2016White` - Office 2016 white drag hints
- `Office2016DarkGray` - Office 2016 dark gray drag hints
- `Office2016Black` - Office 2016 black drag hints

**VB.NET:**

```vb
' Set drag style
Me.dockingManager1.DragProviderStyle = DragProviderStyle.VS2012
```

## Right-to-Left Support

```csharp
// Enable right-to-left layout
this.dockingManager1.RightToLeft = System.Windows.Forms.RightToLeft.Yes;

// Disable RTL
this.dockingManager1.RightToLeft = System.Windows.Forms.RightToLeft.No;
```

## Complete Example

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Drawing;

public class AppearanceExample : Form
{
    private DockingManager dockingManager1;
    private Panel panel1, panel2, panel3;
    private ComboBox cmbTheme;
    
    public AppearanceExample()
    {
        InitializeComponent();
        SetupDocking();
        ApplyCustomAppearance();
        SetupThemeSelector();
    }
    
    private void SetupDocking()
    {
        // Create DockingManager
        this.dockingManager1 = new DockingManager(this.components);
        this.dockingManager1.HostControl = this;
        
        // Create panels
        panel1 = new Panel { BackColor = Color.White };
        panel2 = new Panel { BackColor = Color.White };
        panel3 = new Panel { BackColor = Color.White };
        
        this.Controls.AddRange(new Control[] { panel1, panel2, panel3 });
        
        // Enable docking
        this.dockingManager1.SetEnableDocking(panel1, true);
        this.dockingManager1.SetEnableDocking(panel2, true);
        this.dockingManager1.SetEnableDocking(panel3, true);
        
        // Set labels
        this.dockingManager1.SetDockLabel(panel1, "Toolbox");
        this.dockingManager1.SetDockLabel(panel2, "Properties");
        this.dockingManager1.SetDockLabel(panel3, "Output");
        
        // Arrange
        this.dockingManager1.DockControl(panel1, this, DockingStyle.Left, 200);
        this.dockingManager1.DockControl(panel2, this, DockingStyle.Right, 250);
        this.dockingManager1.DockControl(panel3, this, DockingStyle.Bottom, 150);
    }
    
    private void ApplyCustomAppearance()
    {
        // Set visual style
        this.dockingManager1.VisualStyle = VisualStyle.Office2016Colorful;
        
        // Caption customization
        this.dockingManager1.CaptionHeight = 28;
        this.dockingManager1.ActiveCaptionBackground = new BrushInfo(
            GradientStyle.Vertical,
            new Color[] { Color.FromArgb(0, 122, 204), Color.FromArgb(0, 99, 177) }
        );
        this.dockingManager1.ActiveCaptionForeGround = Color.White;
        this.dockingManager1.ActiveCaptionFont = 
            new Font("Segoe UI", 10f, FontStyle.Bold);
        this.dockingManager1.ActiveCaptionButtonForeColor = Color.White;
        
        this.dockingManager1.InActiveCaptionBackground = 
            new BrushInfo(Color.FromArgb(245, 245, 245));
        this.dockingManager1.InActiveCaptionForeGround = Color.FromArgb(68, 68, 68);
        this.dockingManager1.InActiveCaptionFont = 
            new Font("Segoe UI", 9f, FontStyle.Regular);
        this.dockingManager1.InActiveCaptionButtonForeColor = Color.Gray;
        
        // Tab customization
        this.dockingManager1.DockTabFont = 
            new Font("Segoe UI", 9f, FontStyle.Regular);
        this.dockingManager1.DockTabHeight = 26;
        this.dockingManager1.ActiveDockTabForeColor = Color.White;
        this.dockingManager1.ActiveDockTabBackColor = Color.FromArgb(0, 122, 204);
        this.dockingManager1.DockTabForeColor = Color.FromArgb(68, 68, 68);
        this.dockingManager1.DockTabBackColor = Color.FromArgb(240, 240, 240);
        this.dockingManager1.DockTabPanelBackColor = Color.FromArgb(245, 245, 245);
        this.dockingManager1.DockTabSeparatorColor = Color.FromArgb(204, 206, 219);
        
        // Border and splitter
        this.dockingManager1.BorderColor = Color.FromArgb(204, 206, 219);
        this.dockingManager1.SplitterWidth = 5;
        
        // Drag style
        this.dockingManager1.DragProviderStyle = 
            DragProviderStyle.Office2016Colorful;
    }
    
    private void SetupThemeSelector()
    {
        // Add theme selector combo box
        cmbTheme = new ComboBox 
        { 
            Dock = DockStyle.Top,
            DropDownStyle = ComboBoxStyle.DropDownList
        };
        
        cmbTheme.Items.AddRange(new object[] 
        {
            "Office2016 Colorful",
            "Office2016 White",
            "Office2016 DarkGray",
            "Office2016 Black",
            "Metro",
            "VS2010",
            "Office2010",
            "Office2007"
        });
        
        cmbTheme.SelectedIndex = 0;
        cmbTheme.SelectedIndexChanged += CmbTheme_SelectedIndexChanged;
        
        panel1.Controls.Add(cmbTheme);
    }
    
    private void CmbTheme_SelectedIndexChanged(object sender, EventArgs e)
    {
        string theme = cmbTheme.SelectedItem.ToString();
        
        switch (theme)
        {
            case "Office2016 Colorful":
                ApplyOffice2016ColorfulTheme();
                break;
            case "Office2016 White":
                ApplyOffice2016WhiteTheme();
                break;
            case "Office2016 DarkGray":
                ApplyOffice2016DarkGrayTheme();
                break;
            case "Office2016 Black":
                ApplyOffice2016BlackTheme();
                break;
            case "Metro":
                ApplyMetroTheme();
                break;
            case "VS2010":
                ApplyVS2010Theme();
                break;
            case "Office2010":
                ApplyOffice2010Theme();
                break;
            case "Office2007":
                ApplyOffice2007Theme();
                break;
        }
    }
    
    private void ApplyOffice2016ColorfulTheme()
    {
        this.dockingManager1.VisualStyle = VisualStyle.Office2016Colorful;
        this.dockingManager1.DragProviderStyle = 
            DragProviderStyle.Office2016Colorful;
    }
    
    private void ApplyOffice2016WhiteTheme()
    {
        this.dockingManager1.VisualStyle = VisualStyle.Office2016White;
        this.dockingManager1.DragProviderStyle = 
            DragProviderStyle.Office2016White;
    }
    
    private void ApplyOffice2016DarkGrayTheme()
    {
        this.dockingManager1.VisualStyle = VisualStyle.Office2016DarkGray;
        this.dockingManager1.DragProviderStyle = 
            DragProviderStyle.Office2016DarkGray;
    }
    
    private void ApplyOffice2016BlackTheme()
    {
        this.dockingManager1.VisualStyle = VisualStyle.Office2016Black;
        this.dockingManager1.DragProviderStyle = 
            DragProviderStyle.Office2016Black;
        this.BackColor = Color.FromArgb(40, 40, 40);
    }
    
    private void ApplyMetroTheme()
    {
        this.dockingManager1.VisualStyle = VisualStyle.Metro;
        this.dockingManager1.MetroBorderWidth = 2;
        this.dockingManager1.MetroCaptionColor = Color.FromArgb(0, 122, 204);
        this.dockingManager1.MetroButtonColor = Color.White;
        this.dockingManager1.MetroColor = Color.FromArgb(0, 122, 204);
        this.dockingManager1.ShowMetroCaptionDottedLines = false;
        this.BackColor = Color.White;
    }
    
    private void ApplyVS2010Theme()
    {
        this.dockingManager1.VisualStyle = VisualStyle.VS2010;
        this.dockingManager1.DragProviderStyle = DragProviderStyle.VS2010;
        this.BackColor = Color.FromArgb(41, 57, 85);
    }
    
    private void ApplyOffice2010Theme()
    {
        this.dockingManager1.VisualStyle = VisualStyle.Office2010;
        this.dockingManager1.Office2010Theme = Office2010Theme.Blue;
        this.BackColor = Color.FromArgb(194, 217, 247);
    }
    
    private void ApplyOffice2007Theme()
    {
        this.dockingManager1.VisualStyle = VisualStyle.Office2007;
        this.dockingManager1.Office2007Theme = Office2007Theme.Blue;
        this.BackColor = Color.FromArgb(191, 219, 255);
    }
}
```

## Best Practices

1. **Choose appropriate theme** - Match your application's overall design
2. **Maintain consistency** - Use same visual style throughout application
3. **Test color contrast** - Ensure text is readable on backgrounds
4. **Consider accessibility** - High contrast for visually impaired users
5. **Use gradients sparingly** - Simple colors often look cleaner
6. **Match drag style to visual style** - Keep drag hints consistent
7. **Test all themes** - Ensure custom colors work with each theme
8. **Save user preferences** - Remember selected theme using serialization

## Troubleshooting

**Custom colors not applying:**
- Some visual styles override custom colors
- Try different VisualStyle or use Managed theme
- Set colors AFTER setting VisualStyle property

**Gradients not showing:**
- Verify `BrushInfo` is created correctly with `GradientStyle`
- Check color array has at least 2 colors
- Some visual styles don't support gradients

**Caption height not changing:**
- Not supported in Default and VS2005 styles
- Change to different VisualStyle
- Maximum value is 60 pixels

**Tab colors different than expected:**
- DocumentWindowSettings for document tabs (TDI mode)
- DockTab properties for regular tabbed windows
- AutoHideTab properties for auto-hide tabs
- Verify you're setting the correct property set
