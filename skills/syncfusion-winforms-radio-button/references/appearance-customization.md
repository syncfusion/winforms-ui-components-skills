# Appearance Customization

## Table of Contents

- [Overview](#overview)
- [Gradient Backgrounds](#gradient-backgrounds)
  - [Background Styles](#background-styles)
  - [Gradient Colors](#gradient-colors)
  - [Complete Gradient Examples](#complete-gradient-examples)
- [Border Customization](#border-customization)
  - [Border Styles](#border-styles)
  - [2D Borders](#2d-borders)
  - [3D Borders](#3d-borders)
  - [Border Colors](#border-colors)
- [Alignment Settings](#alignment-settings)
  - [Text Alignment](#text-alignment)
  - [RadioButton Alignment](#radiobutton-alignment)
- [Image Settings](#image-settings)
  - [Image-Based Radio Buttons](#image-based-radio-buttons)
  - [State-Specific Images](#state-specific-images)
  - [Mouse-Over Images](#mouse-over-images)
- [Complete Customization Examples](#complete-customization-examples)
- [Design Patterns](#design-patterns)
- [Best Practices](#best-practices)

## Overview

RadioButtonAdv provides extensive appearance customization options that allow you to create visually stunning radio buttons that match your application's design. This includes gradient backgrounds, custom borders, flexible alignment, and image-based rendering.

### Customization Categories

- **Backgrounds**: Gradient fills with horizontal or vertical orientation
- **Borders**: 2D and 3D border styles with custom colors
- **Alignment**: Independent text and radio button positioning
- **Images**: Custom images for different states and interactions

## Gradient Backgrounds

RadioButtonAdv supports gradient backgrounds that can enhance the visual appeal of your controls.

### Background Styles

The `BackgroundStyle` property determines how the background is rendered.

| Property | Type | Description | Options |
|----------|------|-------------|---------|
| `BackgroundStyle` | CheckBoxAdvBackStyle | Background rendering mode | Default, HorizontalGradient, VerticalGradient |
| `GradientStart` | Color | Starting color of gradient | Any Color |
| `GradientEnd` | Color | Ending color of gradient | Any Color |

### Horizontal Gradient

**C#:**
```csharp
this.radioButtonAdv1.BackgroundStyle = Syncfusion.Windows.Forms.Tools.CheckBoxAdvBackStyle.HorizontalGradient;
this.radioButtonAdv1.GradientStart = System.Drawing.Color.LightSkyBlue;
this.radioButtonAdv1.GradientEnd = System.Drawing.Color.DarkBlue;
this.radioButtonAdv1.Text = "Horizontal Gradient";
```

**VB.NET:**
```vb
Me.radioButtonAdv1.BackgroundStyle = Syncfusion.Windows.Forms.Tools.CheckBoxAdvBackStyle.HorizontalGradient
Me.radioButtonAdv1.GradientStart = System.Drawing.Color.LightSkyBlue
Me.radioButtonAdv1.GradientEnd = System.Drawing.Color.DarkBlue
Me.radioButtonAdv1.Text = "Horizontal Gradient"
```

### Vertical Gradient

**C#:**
```csharp
this.radioButtonAdv2.BackgroundStyle = Syncfusion.Windows.Forms.Tools.CheckBoxAdvBackStyle.VerticalGradient;
this.radioButtonAdv2.GradientStart = System.Drawing.Color.LightCoral;
this.radioButtonAdv2.GradientEnd = System.Drawing.Color.DarkRed;
this.radioButtonAdv2.Text = "Vertical Gradient";
this.radioButtonAdv2.ForeColor = System.Drawing.Color.White;
```

**VB.NET:**
```vb
Me.radioButtonAdv2.BackgroundStyle = Syncfusion.Windows.Forms.Tools.CheckBoxAdvBackStyle.VerticalGradient
Me.radioButtonAdv2.GradientStart = System.Drawing.Color.LightCoral
Me.radioButtonAdv2.GradientEnd = System.Drawing.Color.DarkRed
Me.radioButtonAdv2.Text = "Vertical Gradient"
Me.radioButtonAdv2.ForeColor = System.Drawing.Color.White
```

### Complete Gradient Examples

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace GradientDemo
{
    public partial class GradientForm : Form
    {
        public GradientForm()
        {
            InitializeComponent();
            CreateGradientOptions();
        }

        private void CreateGradientOptions()
        {
            this.Text = "Gradient Background Examples";
            this.Size = new Size(500, 450);
            this.BackColor = Color.White;

            // Cool blue gradient
            var radio1 = new RadioButtonAdv();
            radio1.Text = "Ocean Theme";
            radio1.Location = new Point(30, 30);
            radio1.Size = new Size(400, 40);
            radio1.BackgroundStyle = CheckBoxAdvBackStyle.HorizontalGradient;
            radio1.GradientStart = Color.LightCyan;
            radio1.GradientEnd = Color.DarkCyan;
            radio1.ForeColor = Color.DarkSlateGray;
            radio1.Font = new Font("Segoe UI", 11F, FontStyle.Bold);
            this.Controls.Add(radio1);

            // Warm sunset gradient
            var radio2 = new RadioButtonAdv();
            radio2.Text = "Sunset Theme";
            radio2.Location = new Point(30, 90);
            radio2.Size = new Size(400, 40);
            radio2.BackgroundStyle = CheckBoxAdvBackStyle.VerticalGradient;
            radio2.GradientStart = Color.Gold;
            radio2.GradientEnd = Color.DarkOrange;
            radio2.ForeColor = Color.DarkRed;
            radio2.Font = new Font("Segoe UI", 11F, FontStyle.Bold);
            this.Controls.Add(radio2);

            // Professional gray gradient
            var radio3 = new RadioButtonAdv();
            radio3.Text = "Professional Theme";
            radio3.Location = new Point(30, 150);
            radio3.Size = new Size(400, 40);
            radio3.BackgroundStyle = CheckBoxAdvBackStyle.HorizontalGradient;
            radio3.GradientStart = Color.WhiteSmoke;
            radio3.GradientEnd = Color.LightGray;
            radio3.ForeColor = Color.Black;
            radio3.Font = new Font("Segoe UI", 11F);
            this.Controls.Add(radio3);

            // Nature green gradient
            var radio4 = new RadioButtonAdv();
            radio4.Text = "Nature Theme";
            radio4.Location = new Point(30, 210);
            radio4.Size = new Size(400, 40);
            radio4.BackgroundStyle = CheckBoxAdvBackStyle.VerticalGradient;
            radio4.GradientStart = Color.LightGreen;
            radio4.GradientEnd = Color.ForestGreen;
            radio4.ForeColor = Color.White;
            radio4.Font = new Font("Segoe UI", 11F, FontStyle.Bold);
            this.Controls.Add(radio4);

            // Royal purple gradient
            var radio5 = new RadioButtonAdv();
            radio5.Text = "Royal Theme";
            radio5.Location = new Point(30, 270);
            radio5.Size = new Size(400, 40);
            radio5.BackgroundStyle = CheckBoxAdvBackStyle.HorizontalGradient;
            radio5.GradientStart = Color.Plum;
            radio5.GradientEnd = Color.DarkMagenta;
            radio5.ForeColor = Color.White;
            radio5.Font = new Font("Segoe UI", 11F, FontStyle.Bold);
            this.Controls.Add(radio5);

            // No gradient (reference)
            var radio6 = new RadioButtonAdv();
            radio6.Text = "Standard (No Gradient)";
            radio6.Location = new Point(30, 330);
            radio6.Size = new Size(400, 40);
            radio6.BackgroundStyle = CheckBoxAdvBackStyle.Default;
            radio6.BackColor = Color.WhiteSmoke;
            radio6.Font = new Font("Segoe UI", 11F);
            this.Controls.Add(radio6);
        }
    }
}
```

**Important Notes:**
- Gradient backgrounds cannot be applied when `BackgroundStyle` is set to `Default`
- Background images cannot be displayed simultaneously with gradient settings
- Gradients work well with both light and dark themes

## Border Customization

RadioButtonAdv provides extensive border customization options including 2D and 3D styles.

### Border Styles

| Property | Type | Description |
|----------|------|-------------|
| `BorderStyle` | BorderStyle | Main border style (None, FixedSingle, Fixed3D) |
| `BorderColor` | Color | Color for 2D borders |
| `BorderSingle` | ButtonBorderStyle | Style for FixedSingle borders |
| `Border3DStyle` | Border3DStyle | Style for Fixed3D borders |
| `HotBorderColor` | Color | Border color on mouse hover (FixedSingle only) |

### 2D Borders

2D borders use the `FixedSingle` border style with customizable colors and line styles.

**C#:**
```csharp
// Solid 2D border
this.radioButtonAdv1.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle;
this.radioButtonAdv1.BorderColor = System.Drawing.Color.Navy;
this.radioButtonAdv1.BorderSingle = System.Windows.Forms.ButtonBorderStyle.Solid;
this.radioButtonAdv1.Text = "Solid Border";

// Dotted 2D border
this.radioButtonAdv2.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle;
this.radioButtonAdv2.BorderColor = System.Drawing.Color.Red;
this.radioButtonAdv2.BorderSingle = System.Windows.Forms.ButtonBorderStyle.Dotted;
this.radioButtonAdv2.Text = "Dotted Border";

// Dashed 2D border
this.radioButtonAdv3.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle;
this.radioButtonAdv3.BorderColor = System.Drawing.Color.Green;
this.radioButtonAdv3.BorderSingle = System.Windows.Forms.ButtonBorderStyle.Dashed;
this.radioButtonAdv3.Text = "Dashed Border";

// Hot border color (changes on hover)
this.radioButtonAdv4.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle;
this.radioButtonAdv4.BorderColor = System.Drawing.Color.Gray;
this.radioButtonAdv4.HotBorderColor = System.Drawing.Color.DarkOrange;
this.radioButtonAdv4.Text = "Hover Effect Border";
```

**VB.NET:**
```vb
' Solid 2D border
Me.radioButtonAdv1.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle
Me.radioButtonAdv1.BorderColor = System.Drawing.Color.Navy
Me.radioButtonAdv1.BorderSingle = System.Windows.Forms.ButtonBorderStyle.Solid
Me.radioButtonAdv1.Text = "Solid Border"

' Dotted 2D border
Me.radioButtonAdv2.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle
Me.radioButtonAdv2.BorderColor = System.Drawing.Color.Red
Me.radioButtonAdv2.BorderSingle = System.Windows.Forms.ButtonBorderStyle.Dotted
Me.radioButtonAdv2.Text = "Dotted Border"

' Dashed 2D border
Me.radioButtonAdv3.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle
Me.radioButtonAdv3.BorderColor = System.Drawing.Color.Green
Me.radioButtonAdv3.BorderSingle = System.Windows.Forms.ButtonBorderStyle.Dashed
Me.radioButtonAdv3.Text = "Dashed Border"

' Hot border color (changes on hover)
Me.radioButtonAdv4.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle
Me.radioButtonAdv4.BorderColor = System.Drawing.Color.Gray
Me.radioButtonAdv4.HotBorderColor = System.Drawing.Color.DarkOrange
Me.radioButtonAdv4.Text = "Hover Effect Border"
```

### 3D Borders

3D borders provide depth and dimension to the control.

**C#:**
```csharp
// Raised 3D border
this.radioButtonAdv1.BorderStyle = System.Windows.Forms.BorderStyle.Fixed3D;
this.radioButtonAdv1.Border3DStyle = System.Windows.Forms.Border3DStyle.Raised;
this.radioButtonAdv1.Text = "Raised 3D Border";

// Sunken 3D border
this.radioButtonAdv2.BorderStyle = System.Windows.Forms.BorderStyle.Fixed3D;
this.radioButtonAdv2.Border3DStyle = System.Windows.Forms.Border3DStyle.Sunken;
this.radioButtonAdv2.Text = "Sunken 3D Border";

// Etched 3D border
this.radioButtonAdv3.BorderStyle = System.Windows.Forms.BorderStyle.Fixed3D;
this.radioButtonAdv3.Border3DStyle = System.Windows.Forms.Border3DStyle.Etched;
this.radioButtonAdv3.Text = "Etched 3D Border";

// Bump 3D border
this.radioButtonAdv4.BorderStyle = System.Windows.Forms.BorderStyle.Fixed3D;
this.radioButtonAdv4.Border3DStyle = System.Windows.Forms.Border3DStyle.Bump;
this.radioButtonAdv4.Text = "Bump 3D Border";
```

**VB.NET:**
```vb
' Raised 3D border
Me.radioButtonAdv1.BorderStyle = System.Windows.Forms.BorderStyle.Fixed3D
Me.radioButtonAdv1.Border3DStyle = System.Windows.Forms.Border3DStyle.Raised
Me.radioButtonAdv1.Text = "Raised 3D Border"

' Sunken 3D border
Me.radioButtonAdv2.BorderStyle = System.Windows.Forms.BorderStyle.Fixed3D
Me.radioButtonAdv2.Border3DStyle = System.Windows.Forms.Border3DStyle.Sunken
Me.radioButtonAdv2.Text = "Sunken 3D Border"

' Etched 3D border
Me.radioButtonAdv3.BorderStyle = System.Windows.Forms.BorderStyle.Fixed3D
Me.radioButtonAdv3.Border3DStyle = System.Windows.Forms.Border3DStyle.Etched
Me.radioButtonAdv3.Text = "Etched 3D Border"

' Bump 3D border
Me.radioButtonAdv4.BorderStyle = System.Windows.Forms.BorderStyle.Fixed3D
Me.radioButtonAdv4.Border3DStyle = System.Windows.Forms.Border3DStyle.Bump
Me.radioButtonAdv4.Text = "Bump 3D Border"
```

## Alignment Settings

RadioButtonAdv allows independent alignment of both the text and the radio button itself.

### Text Alignment

The `TextContentAlignment` property controls where the text appears within the control.

**C#:**
```csharp
// Middle Left (default)
this.radioButtonAdv1.TextContentAlignment = System.Drawing.ContentAlignment.MiddleLeft;
this.radioButtonAdv1.Text = "Middle Left";

// Middle Center
this.radioButtonAdv2.TextContentAlignment = System.Drawing.ContentAlignment.MiddleCenter;
this.radioButtonAdv2.Text = "Middle Center";

// Middle Right
this.radioButtonAdv3.TextContentAlignment = System.Drawing.ContentAlignment.MiddleRight;
this.radioButtonAdv3.Text = "Middle Right";

// Top Left
this.radioButtonAdv4.TextContentAlignment = System.Drawing.ContentAlignment.TopLeft;
this.radioButtonAdv4.Text = "Top Left";

// Bottom Right
this.radioButtonAdv5.TextContentAlignment = System.Drawing.ContentAlignment.BottomRight;
this.radioButtonAdv5.Text = "Bottom Right";
```

**Note:** For optimal results with `TextContentAlignment`, set `WrapText = false`.

### RadioButton Alignment

The `CheckAlign` property controls where the radio button circle appears.

**C#:**
```csharp
// Radio button on left (default)
this.radioButtonAdv1.CheckAlign = System.Drawing.ContentAlignment.MiddleLeft;
this.radioButtonAdv1.Text = "Button on Left";

// Radio button on right
this.radioButtonAdv2.CheckAlign = System.Drawing.ContentAlignment.MiddleRight;
this.radioButtonAdv2.Text = "Button on Right";

// Radio button centered
this.radioButtonAdv3.CheckAlign = System.Drawing.ContentAlignment.MiddleCenter;
this.radioButtonAdv3.Text = "Button Centered";
this.radioButtonAdv3.TextContentAlignment = System.Drawing.ContentAlignment.BottomCenter;

// Radio button on top
this.radioButtonAdv4.CheckAlign = System.Drawing.ContentAlignment.TopCenter;
this.radioButtonAdv4.Text = "Button on Top";
this.radioButtonAdv4.TextContentAlignment = System.Drawing.ContentAlignment.BottomCenter;
```

**VB.NET:**
```vb
' Radio button on left (default)
Me.radioButtonAdv1.CheckAlign = System.Drawing.ContentAlignment.MiddleLeft
Me.radioButtonAdv1.Text = "Button on Left"

' Radio button on right
Me.radioButtonAdv2.CheckAlign = System.Drawing.ContentAlignment.MiddleRight
Me.radioButtonAdv2.Text = "Button on Right"

' Radio button centered
Me.radioButtonAdv3.CheckAlign = System.Drawing.ContentAlignment.MiddleCenter
Me.radioButtonAdv3.Text = "Button Centered"
Me.radioButtonAdv3.TextContentAlignment = System.Drawing.ContentAlignment.BottomCenter

' Radio button on top
Me.radioButtonAdv4.CheckAlign = System.Drawing.ContentAlignment.TopCenter
Me.radioButtonAdv4.Text = "Button on Top"
Me.radioButtonAdv4.TextContentAlignment = System.Drawing.ContentAlignment.BottomCenter
```

## Image Settings

RadioButtonAdv supports custom images for the radio button, providing complete visual control.

### Image-Based Radio Buttons

To use images instead of the standard radio button:

**C#:**
```csharp
// Enable image-based rendering
this.radioButtonAdv1.ImageCheckBox = true;
this.radioButtonAdv1.ImageCheckBoxSize = new System.Drawing.Size(20, 20);

// Set images for different states
this.radioButtonAdv1.CheckedImage = Image.FromFile("checked.png");
this.radioButtonAdv1.UncheckedImage = Image.FromFile("unchecked.png");
this.radioButtonAdv1.DisabledImage = Image.FromFile("disabled.png");

// Optional: Stretch images to fit
this.radioButtonAdv1.StretchImage = false;
```

**VB.NET:**
```vb
' Enable image-based rendering
Me.radioButtonAdv1.ImageCheckBox = True
Me.radioButtonAdv1.ImageCheckBoxSize = New System.Drawing.Size(20, 20)

' Set images for different states
Me.radioButtonAdv1.CheckedImage = Image.FromFile("checked.png")
Me.radioButtonAdv1.UncheckedImage = Image.FromFile("unchecked.png")
Me.radioButtonAdv1.DisabledImage = Image.FromFile("disabled.png")

' Optional: Stretch images to fit
Me.radioButtonAdv1.StretchImage = False
```

### State-Specific Images

| Property | Description |
|----------|-------------|
| `ImageCheckBox` | Enables image-based radio button (must be `true`) |
| `ImageCheckBoxSize` | Size of the image checkbox |
| `CheckedImage` | Image when checked (not hovering) |
| `UncheckedImage` | Image when unchecked (not hovering) |
| `DisabledImage` | Image when control is disabled |
| `StretchImage` | Whether to stretch images to fit |

### Mouse-Over Images

**C#:**
```csharp
// Images for mouse hover states
this.radioButtonAdv1.ImageCheckBox = true;
this.radioButtonAdv1.ImageCheckBoxSize = new Size(20, 20);

// Normal state images
this.radioButtonAdv1.CheckedImage = Image.FromFile("checked.png");
this.radioButtonAdv1.UncheckedImage = Image.FromFile("unchecked.png");

// Mouse hover images
this.radioButtonAdv1.MouseOverCheckedImage = Image.FromFile("checked_hover.png");
this.radioButtonAdv1.MouseOverUncheckedImage = Image.FromFile("unchecked_hover.png");
```

**VB.NET:**
```vb
' Images for mouse hover states
Me.radioButtonAdv1.ImageCheckBox = True
Me.radioButtonAdv1.ImageCheckBoxSize = New Size(20, 20)

' Normal state images
Me.radioButtonAdv1.CheckedImage = Image.FromFile("checked.png")
Me.radioButtonAdv1.UncheckedImage = Image.FromFile("unchecked.png")

' Mouse hover images
Me.radioButtonAdv1.MouseOverCheckedImage = Image.FromFile("checked_hover.png")
Me.radioButtonAdv1.MouseOverUncheckedImage = Image.FromFile("unchecked_hover.png")
```

## Complete Customization Examples

### Example 1: Professional Form with Custom Styling

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace ProfessionalStyling
{
    public partial class StyledForm : Form
    {
        public StyledForm()
        {
            InitializeComponent();
            CreateProfessionalOptions();
        }

        private void CreateProfessionalOptions()
        {
            this.Text = "Professional Subscription Options";
            this.Size = new Size(550, 500);
            this.BackColor = Color.FromArgb(245, 245, 245);

            var titleLabel = new Label();
            titleLabel.Text = "Choose Your Plan:";
            titleLabel.Location = new Point(30, 20);
            titleLabel.Size = new Size(400, 30);
            titleLabel.Font = new Font("Segoe UI", 16F, FontStyle.Bold);
            titleLabel.ForeColor = Color.FromArgb(51, 51, 51);
            this.Controls.Add(titleLabel);

            // Basic plan
            var radioBasic = CreateStyledRadio(
                "Basic Plan - $9.99/month",
                "Perfect for individuals",
                new Point(30, 70),
                Color.LightSkyBlue,
                Color.SteelBlue
            );
            this.Controls.Add(radioBasic);

            // Professional plan
            var radioPro = CreateStyledRadio(
                "Professional Plan - $19.99/month",
                "Ideal for small teams",
                new Point(30, 180),
                Color.LightGreen,
                Color.ForestGreen
            );
            this.Controls.Add(radioPro);

            // Enterprise plan
            var radioEnterprise = CreateStyledRadio(
                "Enterprise Plan - $49.99/month",
                "Complete solution for organizations",
                new Point(30, 290),
                Color.Gold,
                Color.DarkGoldenrod
            );
            this.Controls.Add(radioEnterprise);
        }

        private RadioButtonAdv CreateStyledRadio(string title, string description, 
                                                 Point location, Color gradStart, Color gradEnd)
        {
            var panel = new Panel();
            panel.Location = location;
            panel.Size = new Size(480, 90);
            panel.BackColor = Color.White;
            panel.BorderStyle = BorderStyle.FixedSingle;

            var radio = new RadioButtonAdv();
            radio.Text = title;
            radio.Location = new Point(15, 15);
            radio.Size = new Size(450, 30);
            radio.Font = new Font("Segoe UI", 12F, FontStyle.Bold);
            radio.BackgroundStyle = CheckBoxAdvBackStyle.HorizontalGradient;
            radio.GradientStart = gradStart;
            radio.GradientEnd = gradEnd;
            radio.ForeColor = Color.DarkSlateGray;
            radio.Style = RadioButtonAdvStyle.Office2016Colorful;

            var descLabel = new Label();
            descLabel.Text = description;
            descLabel.Location = new Point(40, 50);
            descLabel.Size = new Size(420, 25);
            descLabel.Font = new Font("Segoe UI", 10F);
            descLabel.ForeColor = Color.Gray;

            panel.Controls.Add(radio);
            panel.Controls.Add(descLabel);
            this.Controls.Add(panel);

            return radio;
        }
    }
}
```

### Example 2: Custom Border Showcase

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace BorderShowcase
{
    public partial class BorderForm : Form
    {
        public BorderForm()
        {
            InitializeComponent();
            ShowcaseBorders();
        }

        private void ShowcaseBorders()
        {
            this.Text = "Border Customization Examples";
            this.Size = new Size(600, 500);
            this.BackColor = Color.White;

            int yPos = 30;

            // No border
            var radio1 = new RadioButtonAdv();
            radio1.Text = "No Border";
            radio1.Location = new Point(30, yPos);
            radio1.Size = new Size(250, 40);
            radio1.BorderStyle = BorderStyle.None;
            radio1.BackColor = Color.WhiteSmoke;
            this.Controls.Add(radio1);

            yPos += 60;

            // Solid border with hover effect
            var radio2 = new RadioButtonAdv();
            radio2.Text = "Solid Border with Hover Effect";
            radio2.Location = new Point(30, yPos);
            radio2.Size = new Size(250, 40);
            radio2.BorderStyle = BorderStyle.FixedSingle;
            radio2.BorderColor = Color.Navy;
            radio2.BorderSingle = ButtonBorderStyle.Solid;
            radio2.HotBorderColor = Color.Red;
            this.Controls.Add(radio2);

            yPos += 60;

            // Dotted border
            var radio3 = new RadioButtonAdv();
            radio3.Text = "Dotted Border";
            radio3.Location = new Point(30, yPos);
            radio3.Size = new Size(250, 40);
            radio3.BorderStyle = BorderStyle.FixedSingle;
            radio3.BorderColor = Color.Green;
            radio3.BorderSingle = ButtonBorderStyle.Dotted;
            this.Controls.Add(radio3);

            yPos += 60;

            // Raised 3D border
            var radio4 = new RadioButtonAdv();
            radio4.Text = "Raised 3D Border";
            radio4.Location = new Point(30, yPos);
            radio4.Size = new Size(250, 40);
            radio4.BorderStyle = BorderStyle.Fixed3D;
            radio4.Border3DStyle = Border3DStyle.Raised;
            radio4.BackColor = Color.LightGray;
            this.Controls.Add(radio4);

            yPos += 60;

            // Etched 3D border
            var radio5 = new RadioButtonAdv();
            radio5.Text = "Etched 3D Border";
            radio5.Location = new Point(30, yPos);
            radio5.Size = new Size(250, 40);
            radio5.BorderStyle = BorderStyle.Fixed3D;
            radio5.Border3DStyle = Border3DStyle.Etched;
            radio5.BackColor = Color.WhiteSmoke;
            this.Controls.Add(radio5);

            yPos += 60;

            // Combined: Border + Gradient
            var radio6 = new RadioButtonAdv();
            radio6.Text = "Border with Gradient Background";
            radio6.Location = new Point(30, yPos);
            radio6.Size = new Size(250, 40);
            radio6.BorderStyle = BorderStyle.FixedSingle;
            radio6.BorderColor = Color.DarkBlue;
            radio6.BackgroundStyle = CheckBoxAdvBackStyle.HorizontalGradient;
            radio6.GradientStart = Color.LightBlue;
            radio6.GradientEnd = Color.SkyBlue;
            this.Controls.Add(radio6);
        }
    }
}
```

## Design Patterns

### Pattern 1: Card-Style Options

Create visually distinct option cards:

**C#:**
```csharp
private RadioButtonAdv CreateCardOption(string title, string price, Point location)
{
    var radio = new RadioButtonAdv();
    radio.Text = $"{title}\n{price}";
    radio.Location = location;
    radio.Size = new Size(200, 80);
    radio.BackgroundStyle = CheckBoxAdvBackStyle.VerticalGradient;
    radio.GradientStart = Color.White;
    radio.GradientEnd = Color.LightGray;
    radio.BorderStyle = BorderStyle.FixedSingle;
    radio.BorderColor = Color.DarkGray;
    radio.HotBorderColor = Color.DodgerBlue;
    radio.Font = new Font("Segoe UI", 10F);
    radio.TextContentAlignment = ContentAlignment.MiddleCenter;
    radio.CheckAlign = ContentAlignment.TopLeft;
    radio.WrapText = true;
    return radio;
}
```

### Pattern 2: Icon-Based Selection

Use images for intuitive selection:

**C#:**
```csharp
private RadioButtonAdv CreateIconOption(string text, Image checkedIcon, Image uncheckedIcon)
{
    var radio = new RadioButtonAdv();
    radio.Text = text;
    radio.ImageCheckBox = true;
    radio.ImageCheckBoxSize = new Size(32, 32);
    radio.CheckedImage = checkedIcon;
    radio.UncheckedImage = uncheckedIcon;
    radio.Size = new Size(200, 50);
    radio.Font = new Font("Segoe UI", 11F);
    return radio;
}
```

### Pattern 3: Highlighted Selection

Create emphasis through gradients and borders:

**C#:**
```csharp
private void HighlightSelectedOption(RadioButtonAdv selected)
{
    foreach (Control control in this.Controls)
    {
        if (control is RadioButtonAdv radio)
        {
            if (radio == selected && radio.Checked)
            {
                // Highlight selected
                radio.BackgroundStyle = CheckBoxAdvBackStyle.HorizontalGradient;
                radio.GradientStart = Color.LightGreen;
                radio.GradientEnd = Color.MediumSeaGreen;
                radio.BorderStyle = BorderStyle.FixedSingle;
                radio.BorderColor = Color.DarkGreen;
            }
            else
            {
                // Reset others
                radio.BackgroundStyle = CheckBoxAdvBackStyle.Default;
                radio.BackColor = Color.White;
                radio.BorderStyle = BorderStyle.FixedSingle;
                radio.BorderColor = Color.LightGray;
            }
        }
    }
}
```

## Best Practices

### Visual Hierarchy

1. **Use gradients sparingly** - Too many gradients can overwhelm users
2. **Maintain consistency** - Use similar styling across related options
3. **Provide visual feedback** - Use hover effects and borders to indicate interactivity

### Accessibility

1. **Ensure sufficient contrast** between text and background
2. **Use borders to define boundaries** when using custom backgrounds
3. **Test with screen readers** when using image-based radio buttons
4. **Provide text alternatives** for image-only controls

### Performance

1. **Optimize image sizes** - Use appropriately sized images for `ImageCheckBox`
2. **Cache images** - Load images once and reuse
3. **Limit gradient complexity** - Simple gradients perform better

### Color Guidelines

| Use Case | Recommended Colors |
|----------|-------------------|
| Professional | Blues, grays, subtle gradients |
| Warning/Important | Reds, oranges |
| Success/Positive | Greens |
| Information | Blues, cyans |
| Neutral | Grays, whites |

### Border Usage

- **2D Borders**: Modern, flat designs
- **3D Borders**: Traditional, skeuomorphic designs
- **No Borders**: Minimalist designs (ensure other visual cues exist)

### Common Mistakes to Avoid

```csharp
// DON'T: Gradient without setting BackgroundStyle
radio.GradientStart = Color.Red;
radio.GradientEnd = Color.Blue;
// Must set: radio.BackgroundStyle = CheckBoxAdvBackStyle.HorizontalGradient;

// DON'T: Images without enabling ImageCheckBox
radio.CheckedImage = myImage;
// Must set: radio.ImageCheckBox = true;

// DON'T: HotBorderColor without FixedSingle
radio.HotBorderColor = Color.Red;
// Must set: radio.BorderStyle = BorderStyle.FixedSingle;
```

### Troubleshooting

**Gradient Not Showing:**
- Verify `BackgroundStyle` is not `Default`
- Check that `GradientStart` and `GradientEnd` are different colors
- Ensure no background image is set

**Border Not Visible:**
- Confirm `BorderStyle` is set to `FixedSingle` or `Fixed3D`
- Check border color contrasts with background
- Verify control size is sufficient to show border

**Images Not Appearing:**
- Ensure `ImageCheckBox = true`
- Verify image files exist and are accessible
- Check `ImageCheckBoxSize` is appropriate
- Confirm images are loaded correctly (not null)
