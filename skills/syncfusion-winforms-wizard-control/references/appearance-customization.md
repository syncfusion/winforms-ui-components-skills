# Appearance Customization

This guide covers styling the WizardControl, including foreground settings, background colors and images, border styles, and complete theming examples.

## Table of Contents

- [Foreground Settings](#foreground-settings)
- [Background Settings](#background-settings)
- [Border Styles](#border-styles)
- [Style Property (Themes)](#style-property-themes)
- [Complete Styling Examples](#complete-styling-examples)
- [Troubleshooting Styling Issues](#troubleshooting-styling-issues)

## When to Read This

Read this reference when:
- Customizing wizard appearance to match your application theme
- Setting fonts and colors for wizard pages
- Applying background colors or images
- Configuring border styles
- Using Syncfusion themes (Office2016, Metro, etc.)
- Creating branded wizard experiences

## Foreground Settings

Control the text appearance of wizard controls and pages.

### WizardControl Foreground

**C#:**
```csharp
// Set font for wizard control (affects description and buttons)
wizardControl1.Font = new Font("Segoe UI", 9F, FontStyle.Regular);

// Set default foreground color
wizardControl1.ForeColor = Color.FromArgb(60, 60, 60);
```

**VB.NET:**
```vbnet
' Set font for wizard control
wizardControl1.Font = New Font("Segoe UI", 9F, FontStyle.Regular)

' Set default foreground color
wizardControl1.ForeColor = Color.FromArgb(60, 60, 60)
```

### WizardControlPage Foreground

Each page can have its own font and color:

**C#:**
```csharp
welcomePage.Font = new Font("Segoe UI", 10F, FontStyle.Regular);
welcomePage.ForeColor = Color.Black;

errorPage.ForeColor = Color.FromArgb(200, 50, 50);  // Red for error page
```

**VB.NET:**
```vbnet
welcomePage.Font = New Font("Segoe UI", 10F, FontStyle.Regular)
welcomePage.ForeColor = Color.Black

errorPage.ForeColor = Color.FromArgb(200, 50, 50)  ' Red for error page
```

### Title and Description Styling

**C#:**
```csharp
// Style the title label
wizardControl1.Title.Font = new Font("Segoe UI", 14F, FontStyle.Bold);
wizardControl1.Title.ForeColor = Color.FromArgb(0, 51, 102);

// Style the description label
wizardControl1.Description.Font = new Font("Segoe UI", 9F, FontStyle.Regular);
wizardControl1.Description.ForeColor = Color.FromArgb(80, 80, 80);
```

**VB.NET:**
```vbnet
' Style the title label
wizardControl1.Title.Font = New Font("Segoe UI", 14F, FontStyle.Bold)
wizardControl1.Title.ForeColor = Color.FromArgb(0, 51, 102)

' Style the description label
wizardControl1.Description.Font = New Font("Segoe UI", 9F, FontStyle.Regular)
wizardControl1.Description.ForeColor = Color.FromArgb(80, 80, 80)
```

## Background Settings

### WizardControl Background

**Solid Color:**

**C#:**
```csharp
wizardControl1.BackColor = Color.White;
```

**Background Image:**

**C#:**
```csharp
// Load background image
wizardControl1.BackgroundImage = Image.FromFile("background.png");

// Set image layout
wizardControl1.BackgroundImageLayout = ImageLayout.Stretch;
// Options: None, Tile, Center, Stretch, Zoom
```

**VB.NET:**
```vbnet
' Load background image
wizardControl1.BackgroundImage = Image.FromFile("background.png")

' Set image layout
wizardControl1.BackgroundImageLayout = ImageLayout.Stretch
```

### BannerPanel Background

**Solid Color:**

**C#:**
```csharp
wizardControl1.BannerPanel.BackColor = Color.FromArgb(240, 245, 250);
```

**Gradient Background:**

**C#:**
```csharp
using Syncfusion.Drawing;

// Create gradient brush
BrushInfo gradientBrush = new BrushInfo(
    GradientStyle.Horizontal,
    Color.AliceBlue,
    Color.LightSteelBlue
);

wizardControl1.BannerPanel.BackgroundColor = gradientBrush;
```

**VB.NET:**
```vbnet
Imports Syncfusion.Drawing

' Create gradient brush
Dim gradientBrush As New BrushInfo(
    GradientStyle.Horizontal,
    Color.AliceBlue,
    Color.LightSteelBlue
)

wizardControl1.BannerPanel.BackgroundColor = gradientBrush
```

**Available Gradient Styles:**
- `GradientStyle.Horizontal` - Left to right
- `GradientStyle.Vertical` - Top to bottom
- `GradientStyle.ForwardDiagonal` - Top-left to bottom-right
- `GradientStyle.BackwardDiagonal` - Top-right to bottom-left
- `GradientStyle.PathEllipse` - Elliptical gradient
- `GradientStyle.PathRectangle` - Rectangular gradient

### WizardControlPage Background

**Solid Color:**

**C#:**
```csharp
welcomePage.BackColor = Color.White;
errorPage.BackColor = Color.FromArgb(255, 240, 240);  // Light red
```

**Gradient Background:**

**C#:**
```csharp
using Syncfusion.Windows.Forms.Tools;

// Create page with gradient
welcomePage.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.White,
    Color.FromArgb(245, 250, 255)
);
```

**Background Image:**

**C#:**
```csharp
welcomePage.BackgroundImage = Image.FromFile("welcome-bg.png");
welcomePage.BackgroundImageLayout = ImageLayout.Zoom;
```

## Border Styles

### WizardControl Border

**C#:**
```csharp
// Simple border style
wizardControl1.BorderStyle = BorderStyle.FixedSingle;
// Options: None, FixedSingle, Fixed3D
```

**VB.NET:**
```vbnet
' Simple border style
wizardControl1.BorderStyle = BorderStyle.FixedSingle
```

### BannerPanel Border (3D Styles)

**C#:**
```csharp
using Syncfusion.Windows.Forms.Tools;

// Set 3D border style
wizardControl1.BannerPanel.Border3DStyle = Border3DStyle.Flat;

// Available styles:
// - Border3DStyle.RaisedOuter
// - Border3DStyle.SunkenOuter
// - Border3DStyle.RaisedInner
// - Border3DStyle.Raised
// - Border3DStyle.Etched
// - Border3DStyle.SunkenInner
// - Border3DStyle.Bump
// - Border3DStyle.Sunken
// - Border3DStyle.Adjust
// - Border3DStyle.Flat
```

**2D Border:**

**C#:**
```csharp
// Configure 2D border
wizardControl1.BannerPanel.BorderColor = Color.Gray;
wizardControl1.BannerPanel.BorderSides = 
    Border3DSide.Left | Border3DSide.Top | Border3DSide.Right | Border3DSide.Bottom;
wizardControl1.BannerPanel.BorderSingle = BorderSingle.Solid;
```

**VB.NET:**
```vbnet
' Configure 2D border
wizardControl1.BannerPanel.BorderColor = Color.Gray
wizardControl1.BannerPanel.BorderSides = _
    Border3DSide.Left Or Border3DSide.Top Or Border3DSide.Right Or Border3DSide.Bottom
wizardControl1.BannerPanel.BorderSingle = BorderSingle.Solid
```

### WizardControlPage Border

**C#:**
```csharp
// Set page border
welcomePage.BorderStyle = BorderStyle.None;
detailPage.BorderStyle = BorderStyle.FixedSingle;
```

## Style Property (Themes)

Apply built-in Syncfusion themes:

**C#:**
```csharp
using Syncfusion.Windows.Forms.Tools;

// Apply Office2016 theme
wizardControl1.Style = Syncfusion.Windows.Forms.Tools.WizardStyle.Office2016Colorful;

// Available styles:
// - WizardStyle.Default
// - WizardStyle.Office2016Colorful
// - WizardStyle.Office2016White
// - WizardStyle.Office2016Black
// - WizardStyle.Office2016DarkGray
// - WizardStyle.Metro
```

**VB.NET:**
```vbnet
Imports Syncfusion.Windows.Forms.Tools

' Apply Office2016 theme
wizardControl1.Style = WizardStyle.Office2016Colorful
```

**Style Effects:**
- Applies consistent colors across wizard
- Updates button styles
- Configures banner appearance
- Sets default fonts

## Complete Styling Examples

### Modern Light Theme

**C#:**
```csharp
public class ModernLightWizard : Form
{
    private WizardControl wizardControl1;
    
    private void ApplyModernLightTheme()
    {
        // Wizard control settings
        wizardControl1.BackColor = Color.White;
        wizardControl1.ForeColor = Color.FromArgb(60, 60, 60);
        wizardControl1.Font = new Font("Segoe UI", 9F);
        wizardControl1.BorderStyle = BorderStyle.FixedSingle;
        
        // Banner panel
        wizardControl1.BannerPanel.Height = 80;
        wizardControl1.BannerPanel.BackgroundColor = new BrushInfo(
            GradientStyle.Vertical,
            Color.FromArgb(250, 250, 250),
            Color.FromArgb(235, 240, 245)
        );
        wizardControl1.BannerPanel.BorderSingle = BorderSingle.None;
        
        // Title
        wizardControl1.Title.Font = new Font("Segoe UI Light", 16F);
        wizardControl1.Title.ForeColor = Color.FromArgb(50, 50, 50);
        
        // Description
        wizardControl1.Description.Font = new Font("Segoe UI", 9F);
        wizardControl1.Description.ForeColor = Color.FromArgb(100, 100, 100);
        
        // Navigation buttons
        StyleModernButton(wizardControl1.BackButton, false);
        StyleModernButton(wizardControl1.NextButton, true);
        StyleModernButton(wizardControl1.CancelButton, false);
        StyleModernButton(wizardControl1.FinishButton, true);
        
        // Pages
        foreach (WizardControlPage page in wizardControl1.WizardPages)
        {
            page.BackColor = Color.White;
            page.ForeColor = Color.FromArgb(60, 60, 60);
            page.Font = new Font("Segoe UI", 9F);
        }
    }
    
    private void StyleModernButton(Button button, bool isPrimary)
    {
        button.FlatStyle = FlatStyle.Flat;
        button.Font = new Font("Segoe UI", 9F);
        
        if (isPrimary)
        {
            // Primary button (Next, Finish)
            button.BackColor = Color.FromArgb(0, 120, 215);
            button.ForeColor = Color.White;
            button.FlatAppearance.BorderSize = 0;
            button.FlatAppearance.MouseOverBackColor = Color.FromArgb(0, 99, 177);
            button.FlatAppearance.MouseDownBackColor = Color.FromArgb(0, 78, 138);
        }
        else
        {
            // Secondary button (Back, Cancel)
            button.BackColor = Color.FromArgb(245, 245, 245);
            button.ForeColor = Color.FromArgb(60, 60, 60);
            button.FlatAppearance.BorderSize = 1;
            button.FlatAppearance.BorderColor = Color.FromArgb(200, 200, 200);
            button.FlatAppearance.MouseOverBackColor = Color.FromArgb(235, 235, 235);
            button.FlatAppearance.MouseDownBackColor = Color.FromArgb(225, 225, 225);
        }
        
        button.Cursor = Cursors.Hand;
    }
}
```

**VB.NET:**
```vbnet
Public Class ModernLightWizard
    Inherits Form
    
    Private wizardControl1 As WizardControl
    
    Private Sub ApplyModernLightTheme()
        ' Wizard control settings
        wizardControl1.BackColor = Color.White
        wizardControl1.ForeColor = Color.FromArgb(60, 60, 60)
        wizardControl1.Font = New Font("Segoe UI", 9F)
        wizardControl1.BorderStyle = BorderStyle.FixedSingle
        
        ' Banner panel
        wizardControl1.BannerPanel.Height = 80
        wizardControl1.BannerPanel.BackgroundColor = New BrushInfo(
            GradientStyle.Vertical,
            Color.FromArgb(250, 250, 250),
            Color.FromArgb(235, 240, 245)
        )
        wizardControl1.BannerPanel.BorderSingle = BorderSingle.None
        
        ' Title
        wizardControl1.Title.Font = New Font("Segoe UI Light", 16F)
        wizardControl1.Title.ForeColor = Color.FromArgb(50, 50, 50)
        
        ' Description
        wizardControl1.Description.Font = New Font("Segoe UI", 9F)
        wizardControl1.Description.ForeColor = Color.FromArgb(100, 100, 100)
        
        ' Navigation buttons
        StyleModernButton(wizardControl1.BackButton, False)
        StyleModernButton(wizardControl1.NextButton, True)
        StyleModernButton(wizardControl1.CancelButton, False)
        StyleModernButton(wizardControl1.FinishButton, True)
        
        ' Pages
        For Each page As WizardControlPage In wizardControl1.WizardPages
            page.BackColor = Color.White
            page.ForeColor = Color.FromArgb(60, 60, 60)
            page.Font = New Font("Segoe UI", 9F)
        Next
    End Sub
    
    Private Sub StyleModernButton(button As Button, isPrimary As Boolean)
        button.FlatStyle = FlatStyle.Flat
        button.Font = New Font("Segoe UI", 9F)
        
        If isPrimary Then
            button.BackColor = Color.FromArgb(0, 120, 215)
            button.ForeColor = Color.White
            button.FlatAppearance.BorderSize = 0
            button.FlatAppearance.MouseOverBackColor = Color.FromArgb(0, 99, 177)
            button.FlatAppearance.MouseDownBackColor = Color.FromArgb(0, 78, 138)
        Else
            button.BackColor = Color.FromArgb(245, 245, 245)
            button.ForeColor = Color.FromArgb(60, 60, 60)
            button.FlatAppearance.BorderSize = 1
            button.FlatAppearance.BorderColor = Color.FromArgb(200, 200, 200)
            button.FlatAppearance.MouseOverBackColor = Color.FromArgb(235, 235, 235)
            button.FlatAppearance.MouseDownBackColor = Color.FromArgb(225, 225, 225)
        End If
        
        button.Cursor = Cursors.Hand
    End Sub
End Class
```

### Dark Theme

**C#:**
```csharp
public class DarkThemeWizard : Form
{
    private WizardControl wizardControl1;
    
    private void ApplyDarkTheme()
    {
        // Wizard control settings
        wizardControl1.BackColor = Color.FromArgb(30, 30, 30);
        wizardControl1.ForeColor = Color.FromArgb(220, 220, 220);
        wizardControl1.Font = new Font("Segoe UI", 9F);
        wizardControl1.BorderStyle = BorderStyle.None;
        
        // Banner panel
        wizardControl1.BannerPanel.Height = 80;
        wizardControl1.BannerPanel.BackgroundColor = new BrushInfo(
            GradientStyle.Vertical,
            Color.FromArgb(50, 50, 50),
            Color.FromArgb(30, 30, 30)
        );
        wizardControl1.BannerPanel.BorderSingle = BorderSingle.None;
        
        // Title
        wizardControl1.Title.Font = new Font("Segoe UI Semibold", 14F);
        wizardControl1.Title.ForeColor = Color.White;
        
        // Description
        wizardControl1.Description.Font = new Font("Segoe UI", 9F);
        wizardControl1.Description.ForeColor = Color.FromArgb(180, 180, 180);
        
        // Navigation buttons
        StyleDarkButton(wizardControl1.BackButton, false);
        StyleDarkButton(wizardControl1.NextButton, true);
        StyleDarkButton(wizardControl1.CancelButton, false);
        StyleDarkButton(wizardControl1.FinishButton, true);
        
        // Pages
        foreach (WizardControlPage page in wizardControl1.WizardPages)
        {
            page.BackColor = Color.FromArgb(30, 30, 30);
            page.ForeColor = Color.FromArgb(220, 220, 220);
            page.Font = new Font("Segoe UI", 9F);
        }
        
        // Form background
        this.BackColor = Color.FromArgb(30, 30, 30);
    }
    
    private void StyleDarkButton(Button button, bool isPrimary)
    {
        button.FlatStyle = FlatStyle.Flat;
        button.Font = new Font("Segoe UI", 9F);
        
        if (isPrimary)
        {
            // Primary button
            button.BackColor = Color.FromArgb(0, 120, 215);
            button.ForeColor = Color.White;
            button.FlatAppearance.BorderSize = 0;
            button.FlatAppearance.MouseOverBackColor = Color.FromArgb(16, 110, 190);
            button.FlatAppearance.MouseDownBackColor = Color.FromArgb(0, 90, 158);
        }
        else
        {
            // Secondary button
            button.BackColor = Color.FromArgb(60, 60, 60);
            button.ForeColor = Color.FromArgb(220, 220, 220);
            button.FlatAppearance.BorderSize = 1;
            button.FlatAppearance.BorderColor = Color.FromArgb(80, 80, 80);
            button.FlatAppearance.MouseOverBackColor = Color.FromArgb(70, 70, 70);
            button.FlatAppearance.MouseDownBackColor = Color.FromArgb(50, 50, 50);
        }
        
        button.Cursor = Cursors.Hand;
    }
}
```

### Branded Theme (Corporate Colors)

**C#:**
```csharp
public class BrandedWizard : Form
{
    private WizardControl wizardControl1;
    
    // Corporate brand colors
    private readonly Color BrandPrimary = Color.FromArgb(0, 100, 200);
    private readonly Color BrandSecondary = Color.FromArgb(0, 150, 255);
    private readonly Color BrandAccent = Color.FromArgb(255, 180, 0);
    
    private void ApplyBrandedTheme()
    {
        // Wizard control settings
        wizardControl1.BackColor = Color.White;
        wizardControl1.ForeColor = Color.FromArgb(40, 40, 40);
        wizardControl1.Font = new Font("Arial", 9F);
        wizardControl1.BorderStyle = BorderStyle.FixedSingle;
        
        // Banner panel with brand gradient
        wizardControl1.BannerPanel.Height = 100;
        wizardControl1.BannerPanel.BackgroundColor = new BrushInfo(
            GradientStyle.Horizontal,
            BrandPrimary,
            BrandSecondary
        );
        wizardControl1.BannerPanel.BorderSingle = BorderSingle.None;
        
        // Add brand accent line at bottom of banner
        wizardControl1.BannerPanel.Paint += (sender, e) =>
        {
            using (Pen accentPen = new Pen(BrandAccent, 3))
            {
                e.Graphics.DrawLine(
                    accentPen,
                    0, wizardControl1.BannerPanel.Height - 3,
                    wizardControl1.BannerPanel.Width, wizardControl1.BannerPanel.Height - 3
                );
            }
        };
        
        // Title (white on brand background)
        wizardControl1.Title.Font = new Font("Arial", 16F, FontStyle.Bold);
        wizardControl1.Title.ForeColor = Color.White;
        
        // Description (white on brand background)
        wizardControl1.Description.Font = new Font("Arial", 9F);
        wizardControl1.Description.ForeColor = Color.FromArgb(230, 240, 255);
        
        // Add company logo to banner
        PictureBox logo = new PictureBox
        {
            Image = Properties.Resources.CompanyLogo,  // Your logo resource
            SizeMode = PictureBoxSizeMode.Zoom,
            Size = new Size(80, 40),
            Location = new Point(
                wizardControl1.BannerPanel.Width - 100,
                10
            ),
            BackColor = Color.Transparent
        };
        wizardControl1.BannerPanel.Controls.Add(logo);
        
        // Navigation buttons
        StyleBrandedButton(wizardControl1.BackButton, false);
        StyleBrandedButton(wizardControl1.NextButton, true);
        StyleBrandedButton(wizardControl1.CancelButton, false);
        StyleBrandedButton(wizardControl1.FinishButton, true);
        
        // Pages
        foreach (WizardControlPage page in wizardControl1.WizardPages)
        {
            page.BackColor = Color.FromArgb(248, 250, 252);
            page.ForeColor = Color.FromArgb(40, 40, 40);
            page.Font = new Font("Arial", 9F);
        }
    }
    
    private void StyleBrandedButton(Button button, bool isPrimary)
    {
        button.FlatStyle = FlatStyle.Flat;
        button.Font = new Font("Arial", 9F, FontStyle.Bold);
        
        if (isPrimary)
        {
            button.BackColor = BrandPrimary;
            button.ForeColor = Color.White;
            button.FlatAppearance.BorderSize = 0;
            button.FlatAppearance.MouseOverBackColor = BrandSecondary;
            button.FlatAppearance.MouseDownBackColor = Color.FromArgb(0, 80, 180);
        }
        else
        {
            button.BackColor = Color.White;
            button.ForeColor = BrandPrimary;
            button.FlatAppearance.BorderSize = 2;
            button.FlatAppearance.BorderColor = BrandPrimary;
            button.FlatAppearance.MouseOverBackColor = Color.FromArgb(240, 248, 255);
            button.FlatAppearance.MouseDownBackColor = Color.FromArgb(220, 235, 250);
        }
        
        button.Cursor = Cursors.Hand;
        button.Size = new Size(90, 32);
    }
}
```

### Metro Style (Flat Design)

**C#:**
```csharp
private void ApplyMetroStyle()
{
    // Use built-in Metro style
    wizardControl1.Style = WizardStyle.Metro;
    
    // Or customize manually for Metro look
    wizardControl1.BackColor = Color.White;
    wizardControl1.ForeColor = Color.Black;
    wizardControl1.BorderStyle = BorderStyle.None;
    
    // Flat banner
    wizardControl1.BannerPanel.BackColor = Color.FromArgb(0, 120, 215);
    wizardControl1.BannerPanel.BorderSingle = BorderSingle.None;
    
    wizardControl1.Title.ForeColor = Color.White;
    wizardControl1.Description.ForeColor = Color.FromArgb(230, 240, 255);
    
    // Flat buttons
    foreach (Button btn in new[] 
    { 
        wizardControl1.BackButton, 
        wizardControl1.NextButton,
        wizardControl1.CancelButton,
        wizardControl1.FinishButton
    })
    {
        btn.FlatStyle = FlatStyle.Flat;
        btn.FlatAppearance.BorderSize = 0;
    }
}
```

## Troubleshooting Styling Issues

### Issue: Style Changes Not Appearing

**Solution 1:** Set properties after adding wizard to form:
```csharp
this.Controls.Add(wizardControl1);
ApplyCustomStyling();  // Apply styles after adding
```

**Solution 2:** Refresh the control:
```csharp
wizardControl1.Refresh();
```

### Issue: Banner Gradient Not Showing

**Ensure BrushInfo is used correctly:**
```csharp
using Syncfusion.Drawing;

BrushInfo gradient = new BrushInfo(
    GradientStyle.Vertical,
    Color.White,
    Color.LightBlue
);
wizardControl1.BannerPanel.BackgroundColor = gradient;  // Use BackgroundColor, not BackColor
```

### Issue: Button Styles Reset After Navigation

**Apply button styles in a method and call after page changes:**
```csharp
wizardControl1.BeforePageSelect += (sender, e) =>
{
    // Reapply button styles
    this.BeginInvoke(new Action(() => ApplyButtonStyles()));
};
```

### Issue: Font Changes Don't Apply to All Controls

**Set fonts for both wizard and individual pages:**
```csharp
Font customFont = new Font("Arial", 9F);

wizardControl1.Font = customFont;

foreach (WizardControlPage page in wizardControl1.WizardPages)
{
    page.Font = customFont;
}
```

### Issue: BackgroundImage Not Visible

**Check ImageLayout setting:**
```csharp
wizardControl1.BackgroundImage = myImage;
wizardControl1.BackgroundImageLayout = ImageLayout.Stretch;  // or Zoom, Center, etc.

// Ensure image file exists
if (!System.IO.File.Exists("background.png"))
{
    MessageBox.Show("Background image not found!");
}
```

## Next Steps

After customizing appearance:

1. **Design-Time Features** → Read: [design-time-features.md](design-time-features.md)
   - Use smart tags for quick configuration
   - Work with collection editor
   - Navigate pages in designer
