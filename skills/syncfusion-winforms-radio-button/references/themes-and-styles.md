# Themes and Visual Styles

## Table of Contents

- [Overview](#overview)
- [Enabling Themes](#enabling-themes)
- [Visual Styles](#visual-styles)
  - [Default Style](#default-style)
  - [Office2007 Style](#office2007-style)
  - [Metro Style](#metro-style)
  - [Office2016 Styles](#office2016-styles)
- [Office2007 Color Schemes](#office2007-color-schemes)
  - [Blue Scheme](#blue-scheme)
  - [Silver Scheme](#silver-scheme)
  - [Black Scheme](#black-scheme)
  - [Managed Colors](#managed-colors)
- [Choosing the Right Theme](#choosing-the-right-theme)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)

## Overview

The RadioButtonAdv control provides extensive theming and styling capabilities that allow you to match your application's visual design. The control supports multiple built-in themes including Office 2007, Office 2016, and Metro styles, along with customizable color schemes.

### Key Theming Features

- **Windows Themes Support**: Native Windows theme integration
- **Office Styles**: Office 2007 and 2016 visual styles
- **Metro Style**: Modern flat design
- **Color Schemes**: Predefined and custom color schemes
- **Managed Colors**: Full control over theme colors

## Enabling Themes

The `ThemesEnabled` property controls whether the RadioButtonAdv uses Windows themes for rendering.

### Property

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `ThemesEnabled` | bool | Enables or disables Windows themes | False |

### Example

**C#:**
```csharp
// Enable Windows themes
this.radioButtonAdv1.ThemesEnabled = true;
```

**VB.NET:**
```vb
' Enable Windows themes
Me.radioButtonAdv1.ThemesEnabled = True
```

When `ThemesEnabled` is set to `true`, the control renders with the current Windows theme, providing a native look and feel that matches the operating system.

## Visual Styles

The `Style` property provides seven different visual styles for the RadioButtonAdv control.

### Available Styles

| Style | Description |
|-------|-------------|
| `Default` | Standard Windows Forms appearance |
| `Office2007` | Microsoft Office 2007 style |
| `Metro` | Modern flat Metro design |
| `Office2016Colorful` | Office 2016 with colorful accents |
| `Office2016White` | Office 2016 with white theme |
| `Office2016Black` | Office 2016 with black theme |
| `Office2016DarkGray` | Office 2016 with dark gray theme |

### Default Style

The default style provides the standard Windows Forms appearance without any special theming.

**C#:**
```csharp
this.radioButtonAdv1.Style = Syncfusion.Windows.Forms.Tools.RadioButtonAdvStyle.Default;
this.radioButtonAdv1.Text = "Default Style";
```

**VB.NET:**
```vb
Me.radioButtonAdv1.Style = Syncfusion.Windows.Forms.Tools.RadioButtonAdvStyle.Default
Me.radioButtonAdv1.Text = "Default Style"
```

### Office2007 Style

The Office2007 style mimics the appearance of controls in Microsoft Office 2007 applications.

**C#:**
```csharp
this.radioButtonAdv1.Style = Syncfusion.Windows.Forms.Tools.RadioButtonAdvStyle.Office2007;
this.radioButtonAdv1.Text = "Office 2007 Style";
```

**VB.NET:**
```vb
Me.radioButtonAdv1.Style = Syncfusion.Windows.Forms.Tools.RadioButtonAdvStyle.Office2007
Me.radioButtonAdv1.Text = "Office 2007 Style"
```

**When to Use:**
- Applications targeting Office 2007 design language
- Business applications requiring a professional appearance
- When consistency with Office 2007 UI is needed

### Metro Style

The Metro style provides a modern, flat design aesthetic popular in contemporary applications.

**C#:**
```csharp
this.radioButtonAdv1.Style = Syncfusion.Windows.Forms.Tools.RadioButtonAdvStyle.Metro;
this.radioButtonAdv1.Text = "Metro Style";
this.radioButtonAdv1.ForeColor = System.Drawing.Color.White;
this.radioButtonAdv1.BackColor = System.Drawing.Color.FromArgb(0, 114, 198);
```

**VB.NET:**
```vb
Me.radioButtonAdv1.Style = Syncfusion.Windows.Forms.Tools.RadioButtonAdvStyle.Metro
Me.radioButtonAdv1.Text = "Metro Style"
Me.radioButtonAdv1.ForeColor = System.Drawing.Color.White
Me.radioButtonAdv1.BackColor = System.Drawing.Color.FromArgb(0, 114, 198)
```

**When to Use:**
- Modern, touch-friendly applications
- Applications with flat design principles
- Mobile-first or responsive design applications

### Office2016 Styles

Office 2016 styles offer four different color variations, providing flexibility for different UI themes.

#### Office2016Colorful

**C#:**
```csharp
this.radioButtonAdv1.Style = Syncfusion.Windows.Forms.Tools.RadioButtonAdvStyle.Office2016Colorful;
this.radioButtonAdv1.Text = "Office 2016 Colorful";
```

**VB.NET:**
```vb
Me.radioButtonAdv1.Style = Syncfusion.Windows.Forms.Tools.RadioButtonAdvStyle.Office2016Colorful
Me.radioButtonAdv1.Text = "Office 2016 Colorful"
```

#### Office2016White

**C#:**
```csharp
this.radioButtonAdv2.Style = Syncfusion.Windows.Forms.Tools.RadioButtonAdvStyle.Office2016White;
this.radioButtonAdv2.Text = "Office 2016 White";
```

**VB.NET:**
```vb
Me.radioButtonAdv2.Style = Syncfusion.Windows.Forms.Tools.RadioButtonAdvStyle.Office2016White
Me.radioButtonAdv2.Text = "Office 2016 White"
```

#### Office2016Black

**C#:**
```csharp
this.radioButtonAdv3.Style = Syncfusion.Windows.Forms.Tools.RadioButtonAdvStyle.Office2016Black;
this.radioButtonAdv3.Text = "Office 2016 Black";
```

**VB.NET:**
```vb
Me.radioButtonAdv3.Style = Syncfusion.Windows.Forms.Tools.RadioButtonAdvStyle.Office2016Black
Me.radioButtonAdv3.Text = "Office 2016 Black"
```

#### Office2016DarkGray

**C#:**
```csharp
this.radioButtonAdv4.Style = Syncfusion.Windows.Forms.Tools.RadioButtonAdvStyle.Office2016DarkGray;
this.radioButtonAdv4.Text = "Office 2016 Dark Gray";
```

**VB.NET:**
```vb
Me.radioButtonAdv4.Style = Syncfusion.Windows.Forms.Tools.RadioButtonAdvStyle.Office2016DarkGray
Me.radioButtonAdv4.Text = "Office 2016 Dark Gray"
```

**When to Use:**
- **Colorful**: Modern Office applications with vibrant colors
- **White**: Light-themed applications, high contrast
- **Black**: Dark-themed applications, reduced eye strain
- **DarkGray**: Balanced dark theme, professional appearance

## Office2007 Color Schemes

When using the Office2007 style, you can choose from four different color schemes using the `Office2007ColorScheme` property.

### Available Schemes

| Scheme | Description |
|--------|-------------|
| `Blue` | Blue color scheme (classic Office look) |
| `Silver` | Silver/gray color scheme |
| `Black` | Black color scheme |
| `Managed` | Custom managed colors |

### Blue Scheme

The classic Office 2007 blue color scheme.

**C#:**
```csharp
this.radioButtonAdv1.Style = Syncfusion.Windows.Forms.Tools.RadioButtonAdvStyle.Office2007;
this.radioButtonAdv1.Office2007ColorScheme = Syncfusion.Windows.Forms.Office2007Theme.Blue;
this.radioButtonAdv1.Text = "Blue Scheme";
```

**VB.NET:**
```vb
Me.radioButtonAdv1.Style = Syncfusion.Windows.Forms.Tools.RadioButtonAdvStyle.Office2007
Me.radioButtonAdv1.Office2007ColorScheme = Syncfusion.Windows.Forms.Office2007Theme.Blue
Me.radioButtonAdv1.Text = "Blue Scheme"
```

### Silver Scheme

A professional silver/gray color scheme.

**C#:**
```csharp
this.radioButtonAdv2.Style = Syncfusion.Windows.Forms.Tools.RadioButtonAdvStyle.Office2007;
this.radioButtonAdv2.Office2007ColorScheme = Syncfusion.Windows.Forms.Office2007Theme.Silver;
this.radioButtonAdv2.Text = "Silver Scheme";
```

**VB.NET:**
```vb
Me.radioButtonAdv2.Style = Syncfusion.Windows.Forms.Tools.RadioButtonAdvStyle.Office2007
Me.radioButtonAdv2.Office2007ColorScheme = Syncfusion.Windows.Forms.Office2007Theme.Silver
Me.radioButtonAdv2.Text = "Silver Scheme"
```

### Black Scheme

A dark, elegant black color scheme.

**C#:**
```csharp
this.radioButtonAdv3.Style = Syncfusion.Windows.Forms.Tools.RadioButtonAdvStyle.Office2007;
this.radioButtonAdv3.Office2007ColorScheme = Syncfusion.Windows.Forms.Office2007Theme.Black;
this.radioButtonAdv3.Text = "Black Scheme";
```

**VB.NET:**
```vb
Me.radioButtonAdv3.Style = Syncfusion.Windows.Forms.Tools.RadioButtonAdvStyle.Office2007
Me.radioButtonAdv3.Office2007ColorScheme = Syncfusion.Windows.Forms.Office2007Theme.Black
Me.radioButtonAdv3.Text = "Black Scheme"
```

### Managed Colors

The Managed color scheme allows you to apply custom colors to Office2007-styled controls using the `Office2007Colors.ApplyManagedColors` method.

**C#:**
```csharp
using Syncfusion.Windows.Forms;

// Set to Office2007 style with Managed colors
this.radioButtonAdv1.Style = Syncfusion.Windows.Forms.Tools.RadioButtonAdvStyle.Office2007;
this.radioButtonAdv1.Office2007ColorScheme = Syncfusion.Windows.Forms.Office2007Theme.Managed;

// Apply custom color (affects all Office2007 Managed controls in the form)
Office2007Colors.ApplyManagedColors(this, Color.Red);
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms

' Set to Office2007 style with Managed colors
Me.radioButtonAdv1.Style = Syncfusion.Windows.Forms.Tools.RadioButtonAdvStyle.Office2007
Me.radioButtonAdv1.Office2007ColorScheme = Syncfusion.Windows.Forms.Office2007Theme.Managed

' Apply custom color (affects all Office2007 Managed controls in the form)
Office2007Colors.ApplyManagedColors(Me, Color.Red)
```

#### Advanced Managed Colors Example

**C#:**
```csharp
// Apply different managed colors to multiple controls
this.radioButtonAdv1.Style = RadioButtonAdvStyle.Office2007;
this.radioButtonAdv1.Office2007ColorScheme = Office2007Theme.Managed;
this.radioButtonAdv1.Text = "Red Theme";

this.radioButtonAdv2.Style = RadioButtonAdvStyle.Office2007;
this.radioButtonAdv2.Office2007ColorScheme = Office2007Theme.Managed;
this.radioButtonAdv2.Text = "Green Theme";

// Apply custom colors
Office2007Colors.ApplyManagedColors(this, Color.Crimson);

// To apply different colors to different forms:
// Create controls on Form2 and use:
// Office2007Colors.ApplyManagedColors(form2Instance, Color.Green);
```

## Choosing the Right Theme

### Decision Guide

| Scenario | Recommended Style |
|----------|------------------|
| Modern touch applications | Metro |
| Business/Enterprise apps | Office2016Colorful or Office2016White |
| Dark mode applications | Office2016Black or Office2016DarkGray |
| Classic Office look | Office2007 with Blue scheme |
| Custom branded apps | Office2007 with Managed colors |
| Maximum compatibility | Default |

## Complete Examples

### Example 1: Theme Selection Application

A complete application that lets users switch between different themes.

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace ThemeDemo
{
    public partial class ThemeForm : Form
    {
        private RadioButtonAdv radioOption1;
        private RadioButtonAdv radioOption2;
        private RadioButtonAdv radioOption3;
        private ComboBox cmbThemes;
        private Label lblTheme;

        public ThemeForm()
        {
            InitializeComponent();
            InitializeUI();
        }

        private void InitializeUI()
        {
            this.Text = "RadioButtonAdv Theme Demo";
            this.Size = new Size(400, 300);
            this.BackColor = Color.White;

            // Theme selector
            lblTheme = new Label();
            lblTheme.Text = "Select Theme:";
            lblTheme.Location = new Point(20, 20);
            lblTheme.Size = new Size(100, 20);
            this.Controls.Add(lblTheme);

            cmbThemes = new ComboBox();
            cmbThemes.Location = new Point(130, 18);
            cmbThemes.Size = new Size(200, 25);
            cmbThemes.DropDownStyle = ComboBoxStyle.DropDownList;
            cmbThemes.Items.AddRange(new string[] {
                "Default",
                "Office2007",
                "Metro",
                "Office2016Colorful",
                "Office2016White",
                "Office2016Black",
                "Office2016DarkGray"
            });
            cmbThemes.SelectedIndex = 0;
            cmbThemes.SelectedIndexChanged += CmbThemes_SelectedIndexChanged;
            this.Controls.Add(cmbThemes);

            // Radio buttons
            radioOption1 = CreateRadioButton("Option 1", new Point(30, 70));
            radioOption2 = CreateRadioButton("Option 2", new Point(30, 110));
            radioOption3 = CreateRadioButton("Option 3", new Point(30, 150));

            radioOption1.Checked = true;
        }

        private RadioButtonAdv CreateRadioButton(string text, Point location)
        {
            var radio = new RadioButtonAdv();
            radio.Text = text;
            radio.Location = location;
            radio.Size = new Size(300, 30);
            this.Controls.Add(radio);
            return radio;
        }

        private void CmbThemes_SelectedIndexChanged(object sender, EventArgs e)
        {
            RadioButtonAdvStyle style = RadioButtonAdvStyle.Default;

            switch (cmbThemes.SelectedItem.ToString())
            {
                case "Default":
                    style = RadioButtonAdvStyle.Default;
                    break;
                case "Office2007":
                    style = RadioButtonAdvStyle.Office2007;
                    break;
                case "Metro":
                    style = RadioButtonAdvStyle.Metro;
                    break;
                case "Office2016Colorful":
                    style = RadioButtonAdvStyle.Office2016Colorful;
                    break;
                case "Office2016White":
                    style = RadioButtonAdvStyle.Office2016White;
                    break;
                case "Office2016Black":
                    style = RadioButtonAdvStyle.Office2016Black;
                    this.BackColor = Color.FromArgb(45, 45, 48);
                    break;
                case "Office2016DarkGray":
                    style = RadioButtonAdvStyle.Office2016DarkGray;
                    this.BackColor = Color.FromArgb(62, 62, 64);
                    break;
            }

            ApplyStyle(style);
        }

        private void ApplyStyle(RadioButtonAdvStyle style)
        {
            radioOption1.Style = style;
            radioOption2.Style = style;
            radioOption3.Style = style;

            // Adjust colors for dark themes
            if (style == RadioButtonAdvStyle.Office2016Black ||
                style == RadioButtonAdvStyle.Office2016DarkGray)
            {
                radioOption1.ForeColor = Color.White;
                radioOption2.ForeColor = Color.White;
                radioOption3.ForeColor = Color.White;
            }
            else
            {
                radioOption1.ForeColor = Color.Black;
                radioOption2.ForeColor = Color.Black;
                radioOption3.ForeColor = Color.Black;
                this.BackColor = Color.White;
            }
        }
    }
}
```

### Example 2: Office2007 Color Schemes

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace Office2007Demo
{
    public partial class Office2007Form : Form
    {
        private GroupBox grpBlue, grpSilver, grpBlack, grpManaged;

        public Office2007Form()
        {
            InitializeComponent();
            InitializeOffice2007Schemes();
        }

        private void InitializeOffice2007Schemes()
        {
            this.Text = "Office 2007 Color Schemes";
            this.Size = new Size(600, 400);

            // Blue scheme group
            grpBlue = CreateSchemeGroup("Blue Scheme", new Point(20, 20), 
                                       Office2007Theme.Blue);

            // Silver scheme group
            grpSilver = CreateSchemeGroup("Silver Scheme", new Point(320, 20), 
                                         Office2007Theme.Silver);

            // Black scheme group
            grpBlack = CreateSchemeGroup("Black Scheme", new Point(20, 180), 
                                        Office2007Theme.Black);

            // Managed colors group
            grpManaged = CreateManagedGroup("Managed Colors", new Point(320, 180));
        }

        private GroupBox CreateSchemeGroup(string title, Point location, 
                                          Office2007Theme scheme)
        {
            var group = new GroupBox();
            group.Text = title;
            group.Location = location;
            group.Size = new Size(250, 140);

            for (int i = 0; i < 3; i++)
            {
                var radio = new RadioButtonAdv();
                radio.Text = $"Option {i + 1}";
                radio.Location = new Point(20, 30 + (i * 35));
                radio.Size = new Size(200, 25);
                radio.Style = RadioButtonAdvStyle.Office2007;
                radio.Office2007ColorScheme = scheme;
                
                if (i == 0) radio.Checked = true;
                
                group.Controls.Add(radio);
            }

            this.Controls.Add(group);
            return group;
        }

        private GroupBox CreateManagedGroup(string title, Point location)
        {
            var group = new GroupBox();
            group.Text = title;
            group.Location = location;
            group.Size = new Size(250, 140);

            var radio1 = new RadioButtonAdv();
            radio1.Text = "Red Theme";
            radio1.Location = new Point(20, 30);
            radio1.Size = new Size(200, 25);
            radio1.Style = RadioButtonAdvStyle.Office2007;
            radio1.Office2007ColorScheme = Office2007Theme.Managed;
            radio1.Checked = true;
            group.Controls.Add(radio1);

            var btnApplyRed = new Button();
            btnApplyRed.Text = "Apply Red";
            btnApplyRed.Location = new Point(20, 70);
            btnApplyRed.Size = new Size(100, 30);
            btnApplyRed.Click += (s, e) => 
                Office2007Colors.ApplyManagedColors(this, Color.Crimson);
            group.Controls.Add(btnApplyRed);

            var btnApplyGreen = new Button();
            btnApplyGreen.Text = "Apply Green";
            btnApplyGreen.Location = new Point(130, 70);
            btnApplyGreen.Size = new Size(100, 30);
            btnApplyGreen.Click += (s, e) => 
                Office2007Colors.ApplyManagedColors(this, Color.Green);
            group.Controls.Add(btnApplyGreen);

            this.Controls.Add(group);
            return group;
        }
    }
}
```

## Best Practices

### Theme Consistency

1. **Use the same theme throughout your application** for a consistent user experience
2. **Consider your target audience** when selecting themes (corporate users may prefer Office styles)
3. **Test themes with different Windows themes** to ensure compatibility

### Dark Theme Considerations

When using dark themes (Office2016Black, Office2016DarkGray):

```csharp
// Adjust text colors for readability
if (radioButton.Style == RadioButtonAdvStyle.Office2016Black)
{
    radioButton.ForeColor = Color.White;
    this.BackColor = Color.FromArgb(45, 45, 48);
}
```

### Performance

- Theme changes are lightweight and can be done at runtime
- Use managed colors sparingly as they affect all controls in the form
- Cache theme settings if switching frequently

### Accessibility

- Ensure sufficient contrast between text and background
- Test with high contrast Windows themes
- Consider colorblind-friendly options for custom managed colors
