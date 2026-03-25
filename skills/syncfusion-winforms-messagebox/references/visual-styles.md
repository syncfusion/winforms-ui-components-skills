# Visual Styles and Themes

This guide covers all visual styles and themes available in MessageBoxAdv, including Office 2007/2010/2013/2016 and Metro themes.

## Table of Contents
- [Overview](#overview)
- [Default Theme](#default-theme)
- [Office2007 Theme](#office2007-theme)
- [Office2010 Theme](#office2010-theme)
- [Metro Theme](#metro-theme)
- [Office2013 Theme](#office2013-theme)
- [Office2016 Theme](#office2016-theme)
- [Theme Comparison](#theme-comparison)

---

## Overview

MessageBoxAdv supports six main visual styles, each with multiple color schemes:

| Theme | Color Schemes | Total Variations |
|-------|---------------|------------------|
| Default | 1 (system) | 1 |
| Office2007 | Black, Blue, Silver, Managed | 4 |
| Office2010 | Black, Blue, Silver, Managed | 4 |
| Metro | 1 (customizable) | 1+ |
| Office2013 | DarkGray, LightGray, White | 3 |
| Office2016 | Colorful, White, DarkGray | 3 |
| **Total** | - | **16+** |

### Setting Theme

Themes are set using the static `MessageBoxStyle` property:

**C#:**
```csharp
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro;
```

**VB.NET:**
```vb
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro
```

**Theme Selection Best Practices:**
- Set theme once in application startup (Form constructor or Program.cs)
- Match MessageBoxAdv theme to your application's UI theme
- Use Office themes for business applications
- Use Metro for modern, flat design applications

---

## Default Theme

The standard Windows Forms theme using system colors.

### Usage

**C#:**
```csharp
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Default;

MessageBoxAdv.Show(this, 
    "Save changes?", 
    "File Modified", 
    MessageBoxButtons.YesNo, 
    MessageBoxIcon.Question);
```

**VB.NET:**
```vb
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Default

MessageBoxAdv.Show(Me, _
    "Save changes?", _
    "File Modified", _
    MessageBoxButtons.YesNo, _
    MessageBoxIcon.Question)
```

**Characteristics:**
- Uses system default colors
- Follows Windows theme settings
- Classic appearance
- Compatible with older applications
- No additional configuration needed

---

## Office2007 Theme

Professional theme matching Microsoft Office 2007 appearance with four color schemes.

### Setting Office2007 Theme

**C#:**
```csharp
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2007;
MessageBoxAdv.Office2007Theme = Office2007Theme.Blue; // or Black, Silver, Managed
```

**VB.NET:**
```vb
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2007
MessageBoxAdv.Office2007Theme = Office2007Theme.Blue ' or Black, Silver, Managed
```

---

### Office2007 Black Color Scheme

Dark theme with black and gray tones.

**C#:**
```csharp
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2007;
MessageBoxAdv.Office2007Theme = Office2007Theme.Black;

MessageBoxAdv.Show(this, 
    "Save changes?", 
    "File Modified", 
    MessageBoxButtons.YesNo, 
    MessageBoxIcon.Question);
```

**VB.NET:**
```vb
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2007
MessageBoxAdv.Office2007Theme = Office2007Theme.Black

MessageBoxAdv.Show(Me, _
    "Save changes?", _
    "File Modified", _
    MessageBoxButtons.YesNo, _
    MessageBoxIcon.Question)
```

**Appearance:**
- Dark gray/black background
- Silver buttons
- Professional, modern look
- Good for low-light environments

---

### Office2007 Blue Color Scheme

Classic Office 2007 blue theme.

**C#:**
```csharp
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2007;
MessageBoxAdv.Office2007Theme = Office2007Theme.Blue;

MessageBoxAdv.Show(this, 
    "Operation completed successfully!", 
    "Success", 
    MessageBoxButtons.OK, 
    MessageBoxIcon.Information);
```

**VB.NET:**
```vb
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2007
MessageBoxAdv.Office2007Theme = Office2007Theme.Blue

MessageBoxAdv.Show(Me, _
    "Operation completed successfully!", _
    "Success", _
    MessageBoxButtons.OK, _
    MessageBoxIcon.Information)
```

**Appearance:**
- Light blue gradient background
- Blue title bar
- Classic Office 2007 appearance
- Most recognizable Office theme

---

### Office2007 Silver Color Scheme

Neutral silver/gray theme.

**C#:**
```csharp
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2007;
MessageBoxAdv.Office2007Theme = Office2007Theme.Silver;

MessageBoxAdv.Show(this, 
    "Are you sure you want to delete this record?", 
    "Confirm Delete", 
    MessageBoxButtons.YesNo, 
    MessageBoxIcon.Warning);
```

**VB.NET:**
```vb
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2007
MessageBoxAdv.Office2007Theme = Office2007Theme.Silver

MessageBoxAdv.Show(Me, _
    "Are you sure you want to delete this record?", _
    "Confirm Delete", _
    MessageBoxButtons.YesNo, _
    MessageBoxIcon.Warning)
```

**Appearance:**
- Light silver/gray gradient
- Neutral, professional look
- Good for corporate applications
- Less vibrant than Blue scheme

---

### Office2007 Managed (Custom Colors)

Apply custom color to Office2007 theme.

**C#:**
```csharp
using Syncfusion.Windows.Forms;

MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2007;
MessageBoxAdv.Office2007Theme = Office2007Theme.Managed;

// Apply custom color (e.g., corporate green)
Office2007Colors.ApplyManagedColors(this, Color.FromArgb(34, 139, 34));

MessageBoxAdv.Show(this, 
    "Backup completed successfully!", 
    "Success", 
    MessageBoxButtons.OK, 
    MessageBoxIcon.Information);
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms

MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2007
MessageBoxAdv.Office2007Theme = Office2007Theme.Managed

' Apply custom color (e.g., corporate green)
Office2007Colors.ApplyManagedColors(Me, Color.FromArgb(34, 139, 34))

MessageBoxAdv.Show(Me, _
    "Backup completed successfully!", _
    "Success", _
    MessageBoxButtons.OK, _
    MessageBoxIcon.Information)
```

**Custom Color Examples:**

```csharp
// Corporate Red
Office2007Colors.ApplyManagedColors(this, Color.FromArgb(192, 0, 0));

// Corporate Blue
Office2007Colors.ApplyManagedColors(this, Color.FromArgb(0, 102, 204));

// Corporate Green
Office2007Colors.ApplyManagedColors(this, Color.FromArgb(34, 139, 34));

// Corporate Purple
Office2007Colors.ApplyManagedColors(this, Color.FromArgb(106, 13, 173));
```

---

## Office2010 Theme

Modern Office 2010 theme with refined appearance and four color schemes.

### Setting Office2010 Theme

**C#:**
```csharp
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2010;
MessageBoxAdv.Office2010Theme = Office2010Theme.Blue; // or Black, Silver, Managed
```

**VB.NET:**
```vb
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2010
MessageBoxAdv.Office2010Theme = Office2010Theme.Blue ' or Black, Silver, Managed
```

---

### Office2010 Black Color Scheme

Sleek black theme for modern applications.

**C#:**
```csharp
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2010;
MessageBoxAdv.Office2010Theme = Office2010Theme.Black;

MessageBoxAdv.Show(this, 
    "Connection to server lost.", 
    "Connection Error", 
    MessageBoxButtons.RetryCancel, 
    MessageBoxIcon.Error);
```

**VB.NET:**
```vb
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2010
MessageBoxAdv.Office2010Theme = Office2010Theme.Black

MessageBoxAdv.Show(Me, _
    "Connection to server lost.", _
    "Connection Error", _
    MessageBoxButtons.RetryCancel, _
    MessageBoxIcon.Error)
```

**Appearance:**
- Dark background with refined gradients
- Modern, professional look
- Suitable for dark-themed applications
- Better contrast than Office2007 Black

---

### Office2010 Blue Color Scheme

Refreshed blue theme with cleaner appearance.

**C#:**
```csharp
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2010;
MessageBoxAdv.Office2010Theme = Office2010Theme.Blue;

MessageBoxAdv.Show(this, 
    "Do you want to save changes?", 
    "Unsaved Changes", 
    MessageBoxButtons.YesNoCancel, 
    MessageBoxIcon.Question);
```

**VB.NET:**
```vb
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2010
MessageBoxAdv.Office2010Theme = Office2010Theme.Blue

MessageBoxAdv.Show(Me, _
    "Do you want to save changes?", _
    "Unsaved Changes", _
    MessageBoxButtons.YesNoCancel, _
    MessageBoxIcon.Question)
```

**Appearance:**
- Lighter blue tones than Office2007
- Cleaner, more refined look
- Popular for business applications
- Good balance between professional and modern

---

### Office2010 Silver Color Scheme

Updated silver theme with better contrast.

**C#:**
```csharp
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2010;
MessageBoxAdv.Office2010Theme = Office2010Theme.Silver;

MessageBoxAdv.Show(this, 
    "File uploaded successfully!", 
    "Upload Complete", 
    MessageBoxButtons.OK, 
    MessageBoxIcon.Information);
```

**VB.NET:**
```vb
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2010
MessageBoxAdv.Office2010Theme = Office2010Theme.Silver

MessageBoxAdv.Show(Me, _
    "File uploaded successfully!", _
    "Upload Complete", _
    MessageBoxButtons.OK, _
    MessageBoxIcon.Information)
```

**Appearance:**
- Light gray with subtle gradients
- Neutral, professional appearance
- Good for enterprise applications
- Better defined than Office2007 Silver

---

### Office2010 Managed (Custom Colors)

Apply custom color to Office2010 theme.

**C#:**
```csharp
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2010;
MessageBoxAdv.Office2010Theme = Office2010Theme.Managed;

// Apply custom corporate color
Office2010Colors.ApplyManagedColors(this, Color.FromArgb(0, 120, 215));

MessageBoxAdv.Show(this, 
    "Welcome to the Company Portal!", 
    "Welcome", 
    MessageBoxButtons.OK, 
    MessageBoxIcon.Information);
```

**VB.NET:**
```vb
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2010
MessageBoxAdv.Office2010Theme = Office2010Theme.Managed

' Apply custom corporate color
Office2010Colors.ApplyManagedColors(Me, Color.FromArgb(0, 120, 215))

MessageBoxAdv.Show(Me, _
    "Welcome to the Company Portal!", _
    "Welcome", _
    MessageBoxButtons.OK, _
    MessageBoxIcon.Information)
```

---

## Metro Theme

Modern flat design theme popularized by Windows 8/10. Highly customizable through MetroColorTable.

### Setting Metro Theme

**C#:**
```csharp
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro;

MessageBoxAdv.Show(this, 
    "Save changes?", 
    "File Modified", 
    MessageBoxButtons.YesNo, 
    MessageBoxIcon.Question);
```

**VB.NET:**
```vb
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro

MessageBoxAdv.Show(Me, _
    "Save changes?", _
    "File Modified", _
    MessageBoxButtons.YesNo, _
    MessageBoxIcon.Question)
```

**Characteristics:**
- Flat design (no gradients or shadows)
- Modern, minimalist appearance
- Clean, readable interface
- Popular for Windows 10-style applications
- Highly customizable via MetroColorTable (see [metro-customization.md](metro-customization.md))

### Basic Metro Example

**C#:**
```csharp
using Syncfusion.Windows.Forms;

public partial class MainForm : Form
{
    public MainForm()
    {
        InitializeComponent();
        
        // Set Metro theme for all message boxes
        MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro;
    }

    private void btnSave_Click(object sender, EventArgs e)
    {
        DialogResult result = MessageBoxAdv.Show(this, 
            "Do you want to save the changes?", 
            "Save Changes", 
            MessageBoxButtons.YesNoCancel, 
            MessageBoxIcon.Question);

        if (result == DialogResult.Yes)
        {
            SaveDocument();
        }
    }
}
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms

Public Class MainForm
    Public Sub New()
        InitializeComponent()
        
        ' Set Metro theme for all message boxes
        MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro
    End Sub

    Private Sub btnSave_Click(sender As Object, e As EventArgs) Handles btnSave.Click
        Dim result As DialogResult = MessageBoxAdv.Show(Me, _
            "Do you want to save the changes?", _
            "Save Changes", _
            MessageBoxButtons.YesNoCancel, _
            MessageBoxIcon.Question)

        If result = DialogResult.Yes Then
            SaveDocument()
        End If
    End Sub
End Class
```

**For advanced Metro customization**, see [metro-customization.md](metro-customization.md).

---

## Office2013 Theme

Clean, minimalist theme matching Microsoft Office 2013 with three color schemes.

### Setting Office2013 Theme

**C#:**
```csharp
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2013;
MessageBoxAdv.Office2013Theme = Office2013Theme.White; // or DarkGray, LightGray
```

**VB.NET:**
```vb
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2013
MessageBoxAdv.Office2013Theme = Office2013Theme.White ' or DarkGray, LightGray
```

---

### Office2013 DarkGray Color Scheme

Dark, modern theme for professional applications.

**C#:**
```csharp
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2013;
MessageBoxAdv.Office2013Theme = Office2013Theme.DarkGray;

MessageBoxAdv.Show(this, 
    "Database backup completed.", 
    "Backup Complete", 
    MessageBoxButtons.OK, 
    MessageBoxIcon.Information);
```

**VB.NET:**
```vb
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2013
MessageBoxAdv.Office2013Theme = Office2013Theme.DarkGray

MessageBoxAdv.Show(Me, _
    "Database backup completed.", _
    "Backup Complete", _
    MessageBoxButtons.OK, _
    MessageBoxIcon.Information)
```

**Appearance:**
- Dark gray background
- Flat, minimalist design
- Good for dark-themed applications
- Modern Office look

---

### Office2013 LightGray Color Scheme

Neutral, clean theme.

**C#:**
```csharp
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2013;
MessageBoxAdv.Office2013Theme = Office2013Theme.LightGray;

MessageBoxAdv.Show(this, 
    "Are you sure you want to exit?", 
    "Confirm Exit", 
    MessageBoxButtons.YesNo, 
    MessageBoxIcon.Question);
```

**VB.NET:**
```vb
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2013
MessageBoxAdv.Office2013Theme = Office2013Theme.LightGray

MessageBoxAdv.Show(Me, _
    "Are you sure you want to exit?", _
    "Confirm Exit", _
    MessageBoxButtons.YesNo, _
    MessageBoxIcon.Question)
```

**Appearance:**
- Light gray tones
- Clean, neutral appearance
- Professional look
- Easy on eyes

---

### Office2013 White Color Scheme

Bright, clean white theme.

**C#:**
```csharp
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2013;
MessageBoxAdv.Office2013Theme = Office2013Theme.White;

MessageBoxAdv.Show(this, 
    "File saved successfully!", 
    "Success", 
    MessageBoxButtons.OK, 
    MessageBoxIcon.Information);
```

**VB.NET:**
```vb
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2013
MessageBoxAdv.Office2013Theme = Office2013Theme.White

MessageBoxAdv.Show(Me, _
    "File saved successfully!", _
    "Success", _
    MessageBoxButtons.OK, _
    MessageBoxIcon.Information)
```

**Appearance:**
- Bright white background
- Clean, minimalist design
- Modern Office appearance
- High contrast, good readability

---

## Office2016 Theme

Latest Office theme with three color schemes matching Microsoft Office 2016.

### Setting Office2016 Theme

**C#:**
```csharp
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2016;
MessageBoxAdv.Office2016Theme = Office2016Theme.Colorful; // or White, DarkGray
```

**VB.NET:**
```vb
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2016
MessageBoxAdv.Office2016Theme = Office2016Theme.Colorful ' or White, DarkGray
```

---

### Office2016 Colorful Color Scheme

Vibrant theme with colorful accents.

**C#:**
```csharp
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2016;
MessageBoxAdv.Office2016Theme = Office2016Theme.Colorful;

MessageBoxAdv.Show(this, 
    "Do you want to save changes?", 
    "Unsaved Changes", 
    MessageBoxButtons.YesNoCancel, 
    MessageBoxIcon.Question);
```

**VB.NET:**
```vb
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2016
MessageBoxAdv.Office2016Theme = Office2016Theme.Colorful

MessageBoxAdv.Show(Me, _
    "Do you want to save changes?", _
    "Unsaved Changes", _
    MessageBoxButtons.YesNoCancel, _
    MessageBoxIcon.Question)
```

**Appearance:**
- Colorful title bar (blue accent)
- White background
- Modern, vibrant look
- Official Office 2016 appearance

---

### Office2016 White Color Scheme

Clean white theme with subtle accents.

**C#:**
```csharp
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2016;
MessageBoxAdv.Office2016Theme = Office2016Theme.White;

MessageBoxAdv.Show(this, 
    "Operation completed successfully!", 
    "Success", 
    MessageBoxButtons.OK, 
    MessageBoxIcon.Information);
```

**VB.NET:**
```vb
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2016
MessageBoxAdv.Office2016Theme = Office2016Theme.White

MessageBoxAdv.Show(Me, _
    "Operation completed successfully!", _
    "Success", _
    MessageBoxButtons.OK, _
    MessageBoxIcon.Information)
```

**Appearance:**
- All-white theme
- Minimalist, clean design
- Professional appearance
- Good for light-themed applications

---

### Office2016 DarkGray Color Scheme

Dark, professional theme.

**C#:**
```csharp
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2016;
MessageBoxAdv.Office2016Theme = Office2016Theme.DarkGray;

MessageBoxAdv.Show(this, 
    "Failed to connect to server.", 
    "Connection Error", 
    MessageBoxButtons.RetryCancel, 
    MessageBoxIcon.Error);
```

**VB.NET:**
```vb
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2016
MessageBoxAdv.Office2016Theme = Office2016Theme.DarkGray

MessageBoxAdv.Show(Me, _
    "Failed to connect to server.", _
    "Connection Error", _
    MessageBoxButtons.RetryCancel, _
    MessageBoxIcon.Error)
```

**Appearance:**
- Dark gray theme
- Modern, sleek design
- Good for dark-mode applications
- Reduces eye strain in low-light

---

## Theme Comparison

### Design Philosophy

| Theme | Design Style | Best For |
|-------|-------------|----------|
| **Default** | System standard | Legacy applications, system consistency |
| **Office2007** | Gradient-based, glossy | Classic Office look, established applications |
| **Office2010** | Refined gradients, polished | Modern business applications |
| **Metro** | Flat, minimalist | Windows 8/10 style, modern UIs |
| **Office2013** | Flat, clean | Contemporary Office applications |
| **Office2016** | Flat with accents | Latest Office look, modern applications |

### When to Use Each Theme

**Default:**
- Matching system appearance
- Legacy application compatibility
- No specific theme requirements

**Office2007:**
- Classic Office-style applications
- Established business software
- Users familiar with Office 2007

**Office2010:**
- Refined Office appearance
- Professional business applications
- Balance between classic and modern

**Metro:**
- Modern Windows 10 applications
- Flat design philosophy
- Highly customizable appearance
- Touch-friendly interfaces

**Office2013:**
- Contemporary Office-style apps
- Minimalist design preference
- Clean, uncluttered interface

**Office2016:**
- Latest Office appearance
- Modern business applications
- Colorful accents desired
- Up-to-date look and feel

---

## Complete Application Example

**C#:**
```csharp
using Syncfusion.Windows.Forms;
using System;
using System.Drawing;
using System.Windows.Forms;

namespace MessageBoxAdvDemo
{
    public partial class MainForm : Form
    {
        public MainForm()
        {
            InitializeComponent();
            
            // Set default theme for application
            SetApplicationTheme(ApplicationTheme.Metro);
        }

        private enum ApplicationTheme
        {
            Default,
            Office2007Blue,
            Office2010Blue,
            Metro,
            Office2013White,
            Office2016Colorful
        }

        private void SetApplicationTheme(ApplicationTheme theme)
        {
            switch (theme)
            {
                case ApplicationTheme.Default:
                    MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Default;
                    break;

                case ApplicationTheme.Office2007Blue:
                    MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2007;
                    MessageBoxAdv.Office2007Theme = Office2007Theme.Blue;
                    break;

                case ApplicationTheme.Office2010Blue:
                    MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2010;
                    MessageBoxAdv.Office2010Theme = Office2010Theme.Blue;
                    break;

                case ApplicationTheme.Metro:
                    MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro;
                    break;

                case ApplicationTheme.Office2013White:
                    MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2013;
                    MessageBoxAdv.Office2013Theme = Office2013Theme.White;
                    break;

                case ApplicationTheme.Office2016Colorful:
                    MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2016;
                    MessageBoxAdv.Office2016Theme = Office2016Theme.Colorful;
                    break;
            }
        }

        private void btnShowInfo_Click(object sender, EventArgs e)
        {
            MessageBoxAdv.Show(this,
                "This is an information message.",
                "Information",
                MessageBoxButtons.OK,
                MessageBoxIcon.Information);
        }

        private void btnShowConfirm_Click(object sender, EventArgs e)
        {
            DialogResult result = MessageBoxAdv.Show(this,
                "Do you want to proceed?",
                "Confirm",
                MessageBoxButtons.YesNo,
                MessageBoxIcon.Question);

            if (result == DialogResult.Yes)
            {
                // Proceed
            }
        }
    }
}
```

---

## Best Practices

### Theme Selection Strategy

1. **Match Your Application:** Choose theme that matches your overall UI
2. **Consistency:** Use same theme throughout application
3. **User Preference:** Consider allowing users to choose theme
4. **Platform:** Match Windows version (Metro for Win10, Office2007 for Win7)

### Performance Considerations

- Theme switching is lightweight
- Set theme once during application startup
- Avoid changing theme frequently during runtime

### Accessibility

All themes support:
- High contrast mode compatibility
- Screen reader functionality
- Keyboard navigation
- Standard Windows accessibility features

---

## Next Steps

- **Metro Customization:** Customize Metro theme colors in detail → [metro-customization.md](metro-customization.md)
- **Localization:** Implement multilanguage support → [localization.md](localization.md)
- **Button Parameters:** Configure buttons, icons, and features → [button-parameters.md](button-parameters.md)
