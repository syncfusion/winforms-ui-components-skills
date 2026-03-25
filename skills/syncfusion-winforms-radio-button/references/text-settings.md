# Text Settings

## Overview

RadioButtonAdv provides advanced text rendering features including text shadows and text wrapping capabilities. These features enhance the visual appeal of radio buttons and improve readability for long text labels.

### Key Text Features

- **Text Shadows**: Add depth with customizable shadow effects
- **Shadow Customization**: Control shadow color and offset
- **Text Wrapping**: Automatically wrap long text across multiple lines

## Text Shadow Properties

Text shadows add a visual depth effect to the radio button text, making it stand out or creating a subtle 3D appearance.

### Available Properties

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `TextShadow` | bool | Enables or disables text shadow | False |
| `ShadowColor` | Color | Color of the text shadow | Gray |
| `ShadowOffset` | Point | Offset position of the shadow from the text | (0, 0) |
| `WrapText` | bool | Enables text wrapping for long text | False |

## Enabling Text Shadow

The `TextShadow` property controls whether a shadow effect is rendered behind the text.

### Basic Shadow Example

**C#:**
```csharp
// Enable text shadow
this.radioButtonAdv1.TextShadow = true;
this.radioButtonAdv1.Text = "Option with Shadow";
```

**VB.NET:**
```vb
' Enable text shadow
Me.radioButtonAdv1.TextShadow = True
Me.radioButtonAdv1.Text = "Option with Shadow"
```

## Shadow Color

The `ShadowColor` property specifies the color used for the text shadow.

### Shadow Color Examples

**C#:**
```csharp
// Gold shadow
this.radioButtonAdv1.TextShadow = true;
this.radioButtonAdv1.ShadowColor = System.Drawing.Color.Gold;
this.radioButtonAdv1.Text = "Gold Shadow";

// Dark gray shadow for subtle effect
this.radioButtonAdv2.TextShadow = true;
this.radioButtonAdv2.ShadowColor = System.Drawing.Color.DarkGray;
this.radioButtonAdv2.Text = "Subtle Shadow";

// Black shadow for high contrast
this.radioButtonAdv3.TextShadow = true;
this.radioButtonAdv3.ShadowColor = System.Drawing.Color.Black;
this.radioButtonAdv3.Text = "Strong Shadow";
```

**VB.NET:**
```vb
' Gold shadow
Me.radioButtonAdv1.TextShadow = True
Me.radioButtonAdv1.ShadowColor = System.Drawing.Color.Gold
Me.radioButtonAdv1.Text = "Gold Shadow"

' Dark gray shadow for subtle effect
Me.radioButtonAdv2.TextShadow = True
Me.radioButtonAdv2.ShadowColor = System.Drawing.Color.DarkGray
Me.radioButtonAdv2.Text = "Subtle Shadow"

' Black shadow for high contrast
Me.radioButtonAdv3.TextShadow = True
Me.radioButtonAdv3.ShadowColor = System.Drawing.Color.Black
Me.radioButtonAdv3.Text = "Strong Shadow"
```

## Shadow Offset

The `ShadowOffset` property controls the position of the shadow relative to the text. It accepts a `Point` structure where X represents horizontal offset and Y represents vertical offset.

### Understanding Offsets

- **Positive X**: Shadow moves to the right
- **Negative X**: Shadow moves to the left
- **Positive Y**: Shadow moves down
- **Negative Y**: Shadow moves up

### Shadow Offset Examples

**C#:**
```csharp
// Shadow to bottom-right (classic drop shadow)
this.radioButtonAdv1.TextShadow = true;
this.radioButtonAdv1.ShadowColor = System.Drawing.Color.Gray;
this.radioButtonAdv1.ShadowOffset = new System.Drawing.Point(2, 2);
this.radioButtonAdv1.Text = "Bottom-Right Shadow";

// Large offset for dramatic effect
this.radioButtonAdv2.TextShadow = true;
this.radioButtonAdv2.ShadowColor = System.Drawing.Color.Gold;
this.radioButtonAdv2.ShadowOffset = new System.Drawing.Point(8, 8);
this.radioButtonAdv2.Text = "Dramatic Shadow";

// Shadow to top-left
this.radioButtonAdv3.TextShadow = true;
this.radioButtonAdv3.ShadowColor = System.Drawing.Color.DarkBlue;
this.radioButtonAdv3.ShadowOffset = new System.Drawing.Point(-3, -3);
this.radioButtonAdv3.Text = "Top-Left Shadow";

// Horizontal shadow only
this.radioButtonAdv4.TextShadow = true;
this.radioButtonAdv4.ShadowColor = System.Drawing.Color.Red;
this.radioButtonAdv4.ShadowOffset = new System.Drawing.Point(5, 0);
this.radioButtonAdv4.Text = "Horizontal Shadow";
```

**VB.NET:**
```vb
' Shadow to bottom-right (classic drop shadow)
Me.radioButtonAdv1.TextShadow = True
Me.radioButtonAdv1.ShadowColor = System.Drawing.Color.Gray
Me.radioButtonAdv1.ShadowOffset = New System.Drawing.Point(2, 2)
Me.radioButtonAdv1.Text = "Bottom-Right Shadow"

' Large offset for dramatic effect
Me.radioButtonAdv2.TextShadow = True
Me.radioButtonAdv2.ShadowColor = System.Drawing.Color.Gold
Me.radioButtonAdv2.ShadowOffset = New System.Drawing.Point(8, 8)
Me.radioButtonAdv2.Text = "Dramatic Shadow"

' Shadow to top-left
Me.radioButtonAdv3.TextShadow = True
Me.radioButtonAdv3.ShadowColor = System.Drawing.Color.DarkBlue
Me.radioButtonAdv3.ShadowOffset = New System.Drawing.Point(-3, -3)
Me.radioButtonAdv3.Text = "Top-Left Shadow"

' Horizontal shadow only
Me.radioButtonAdv4.TextShadow = True
Me.radioButtonAdv4.ShadowColor = System.Drawing.Color.Red
Me.radioButtonAdv4.ShadowOffset = New System.Drawing.Point(5, 0)
Me.radioButtonAdv4.Text = "Horizontal Shadow"
```

## Text Wrapping

The `WrapText` property enables automatic text wrapping when the text is longer than the control's width.

### Basic Text Wrapping

**C#:**
```csharp
// Enable text wrapping
this.radioButtonAdv1.WrapText = true;
this.radioButtonAdv1.Text = "This is a very long text that will wrap to multiple lines when the control width is exceeded";
this.radioButtonAdv1.Size = new System.Drawing.Size(200, 60);
```

**VB.NET:**
```vb
' Enable text wrapping
Me.radioButtonAdv1.WrapText = True
Me.radioButtonAdv1.Text = "This is a very long text that will wrap to multiple lines when the control width is exceeded"
Me.radioButtonAdv1.Size = New System.Drawing.Size(200, 60)
```

### Text Wrapping with AutoHeight

Combine text wrapping with `AutoHeight` for automatic sizing:

**C#:**
```csharp
this.radioButtonAdv1.WrapText = true;
this.radioButtonAdv1.AutoHeight = true;
this.radioButtonAdv1.Text = "Long text that automatically adjusts the control height";
this.radioButtonAdv1.Width = 200;
```

**VB.NET:**
```vb
Me.radioButtonAdv1.WrapText = True
Me.radioButtonAdv1.AutoHeight = True
Me.radioButtonAdv1.Text = "Long text that automatically adjusts the control height"
Me.radioButtonAdv1.Width = 200
```

## Complete Examples

### Example 1: Text Shadow Showcase

A form demonstrating various text shadow effects:

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace TextShadowDemo
{
    public partial class ShadowForm : Form
    {
        public ShadowForm()
        {
            InitializeComponent();
            InitializeTextShadows();
        }

        private void InitializeTextShadows()
        {
            this.Text = "Text Shadow Examples";
            this.Size = new Size(500, 400);
            this.BackColor = Color.White;

            // No shadow (reference)
            var radio1 = CreateRadioButton("No Shadow", new Point(30, 30));
            radio1.TextShadow = false;

            // Subtle shadow
            var radio2 = CreateRadioButton("Subtle Shadow", new Point(30, 70));
            radio2.TextShadow = true;
            radio2.ShadowColor = Color.FromArgb(128, Color.Gray);
            radio2.ShadowOffset = new Point(1, 1);

            // Classic drop shadow
            var radio3 = CreateRadioButton("Classic Drop Shadow", new Point(30, 110));
            radio3.TextShadow = true;
            radio3.ShadowColor = Color.DarkGray;
            radio3.ShadowOffset = new Point(3, 3);
            radio3.Font = new Font("Arial", 11F, FontStyle.Bold);

            // Colorful shadow
            var radio4 = CreateRadioButton("Colorful Shadow", new Point(30, 150));
            radio4.TextShadow = true;
            radio4.ShadowColor = Color.Gold;
            radio4.ShadowOffset = new Point(4, 4);
            radio4.ForeColor = Color.Navy;
            radio4.Font = new Font("Arial", 12F, FontStyle.Bold);

            // Dramatic shadow
            var radio5 = CreateRadioButton("Dramatic Shadow", new Point(30, 190));
            radio5.TextShadow = true;
            radio5.ShadowColor = Color.Black;
            radio5.ShadowOffset = new Point(8, 8);
            radio5.Font = new Font("Arial", 14F, FontStyle.Bold);
            radio5.ForeColor = Color.DarkRed;

            // Inset effect (top-left shadow)
            var radio6 = CreateRadioButton("Inset Effect", new Point(30, 240));
            radio6.TextShadow = true;
            radio6.ShadowColor = Color.White;
            radio6.ShadowOffset = new Point(-2, -2);
            radio6.BackColor = Color.LightGray;
            radio6.ForeColor = Color.DarkSlateGray;

            // Neon glow effect
            var radio7 = CreateRadioButton("Neon Glow", new Point(30, 280));
            radio7.TextShadow = true;
            radio7.ShadowColor = Color.Cyan;
            radio7.ShadowOffset = new Point(2, 2);
            radio7.BackColor = Color.Black;
            radio7.ForeColor = Color.White;
            radio7.Font = new Font("Arial", 12F, FontStyle.Bold);
        }

        private RadioButtonAdv CreateRadioButton(string text, Point location)
        {
            var radio = new RadioButtonAdv();
            radio.Text = text;
            radio.Location = location;
            radio.Size = new Size(400, 30);
            radio.Style = RadioButtonAdvStyle.Office2016Colorful;
            this.Controls.Add(radio);
            return radio;
        }
    }
}
```

### Example 2: Text Wrapping Demo

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace TextWrappingDemo
{
    public partial class WrappingForm : Form
    {
        private RadioButtonAdv radioShort;
        private RadioButtonAdv radioLongNoWrap;
        private RadioButtonAdv radioLongWrap;
        private RadioButtonAdv radioLongWrapAuto;

        public WrappingForm()
        {
            InitializeComponent();
            InitializeTextWrapping();
        }

        private void InitializeTextWrapping()
        {
            this.Text = "Text Wrapping Examples";
            this.Size = new Size(450, 450);
            this.BackColor = Color.WhiteSmoke;

            // Short text (no wrapping needed)
            radioShort = new RadioButtonAdv();
            radioShort.Text = "Short Option";
            radioShort.Location = new Point(30, 30);
            radioShort.Size = new Size(380, 30);
            radioShort.Style = RadioButtonAdvStyle.Office2016Colorful;
            this.Controls.Add(radioShort);

            // Long text without wrapping (truncated)
            radioLongNoWrap = new RadioButtonAdv();
            radioLongNoWrap.Text = "This is a very long option text that will be truncated if wrapping is not enabled";
            radioLongNoWrap.Location = new Point(30, 80);
            radioLongNoWrap.Size = new Size(250, 30);
            radioLongNoWrap.WrapText = false;
            radioLongNoWrap.Style = RadioButtonAdvStyle.Office2016Colorful;
            this.Controls.Add(radioLongNoWrap);

            // Label for above
            var label1 = new Label();
            label1.Text = "(No wrapping - text truncated)";
            label1.Location = new Point(290, 85);
            label1.Size = new Size(140, 20);
            label1.ForeColor = Color.Gray;
            label1.Font = new Font("Arial", 8F, FontStyle.Italic);
            this.Controls.Add(label1);

            // Long text with wrapping (fixed height)
            radioLongWrap = new RadioButtonAdv();
            radioLongWrap.Text = "This is a very long option text that will wrap to multiple lines when wrapping is enabled";
            radioLongWrap.Location = new Point(30, 150);
            radioLongWrap.Size = new Size(250, 60);
            radioLongWrap.WrapText = true;
            radioLongWrap.Style = RadioButtonAdvStyle.Office2016Colorful;
            this.Controls.Add(radioLongWrap);

            // Label for above
            var label2 = new Label();
            label2.Text = "(Wrapping enabled - fixed height)";
            label2.Location = new Point(290, 165);
            label2.Size = new Size(140, 20);
            label2.ForeColor = Color.Gray;
            label2.Font = new Font("Arial", 8F, FontStyle.Italic);
            this.Controls.Add(label2);

            // Long text with wrapping and auto-height
            radioLongWrapAuto = new RadioButtonAdv();
            radioLongWrapAuto.Text = "This is a very long option text that will wrap to multiple lines and automatically adjust the control height to fit all the text content";
            radioLongWrapAuto.Location = new Point(30, 240);
            radioLongWrapAuto.Width = 250;
            radioLongWrapAuto.WrapText = true;
            radioLongWrapAuto.AutoHeight = true;
            radioLongWrapAuto.Style = RadioButtonAdvStyle.Office2016Colorful;
            this.Controls.Add(radioLongWrapAuto);

            // Label for above
            var label3 = new Label();
            label3.Text = "(Wrapping + AutoHeight)";
            label3.Location = new Point(290, 250);
            label3.Size = new Size(140, 20);
            label3.ForeColor = Color.Gray;
            label3.Font = new Font("Arial", 8F, FontStyle.Italic);
            this.Controls.Add(label3);

            // Interactive example
            var btnChangeText = new Button();
            btnChangeText.Text = "Toggle Long Text";
            btnChangeText.Location = new Point(30, 350);
            btnChangeText.Size = new Size(150, 35);
            btnChangeText.Click += BtnChangeText_Click;
            this.Controls.Add(btnChangeText);
        }

        private bool isLongText = false;

        private void BtnChangeText_Click(object sender, EventArgs e)
        {
            isLongText = !isLongText;

            if (isLongText)
            {
                radioLongWrap.Text = "Even longer text: Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua";
                radioLongWrapAuto.Text = "Even longer text with auto-height: Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam";
            }
            else
            {
                radioLongWrap.Text = "This is a very long option text that will wrap to multiple lines when wrapping is enabled";
                radioLongWrapAuto.Text = "This is a very long option text that will wrap to multiple lines and automatically adjust the control height to fit all the text content";
            }
        }
    }
}
```

### Example 3: Combined Text Effects

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace CombinedTextEffects
{
    public partial class EffectsForm : Form
    {
        public EffectsForm()
        {
            InitializeComponent();
            CreateStyledOptions();
        }

        private void CreateStyledOptions()
        {
            this.Text = "Combined Text Effects";
            this.Size = new Size(450, 350);
            this.BackColor = Color.FromArgb(240, 240, 240);

            // Professional option with subtle shadow
            var radioProfessional = new RadioButtonAdv();
            radioProfessional.Text = "Professional Package";
            radioProfessional.Location = new Point(30, 30);
            radioProfessional.Size = new Size(380, 30);
            radioProfessional.Style = RadioButtonAdvStyle.Office2016White;
            radioProfessional.Font = new Font("Segoe UI", 11F);
            radioProfessional.TextShadow = true;
            radioProfessional.ShadowColor = Color.FromArgb(100, Color.Gray);
            radioProfessional.ShadowOffset = new Point(1, 1);
            this.Controls.Add(radioProfessional);

            // Premium option with bold text and shadow
            var radioPremium = new RadioButtonAdv();
            radioPremium.Text = "Premium Package - Enhanced Features";
            radioPremium.Location = new Point(30, 80);
            radioPremium.Size = new Size(380, 35);
            radioPremium.Style = RadioButtonAdvStyle.Office2016Colorful;
            radioPremium.Font = new Font("Segoe UI", 12F, FontStyle.Bold);
            radioPremium.ForeColor = Color.DarkGoldenrod;
            radioPremium.TextShadow = true;
            radioPremium.ShadowColor = Color.Gold;
            radioPremium.ShadowOffset = new Point(2, 2);
            this.Controls.Add(radioPremium);

            // Wrapped text with shadow
            var radioEnterprise = new RadioButtonAdv();
            radioEnterprise.Text = "Enterprise Package - Complete solution with all features included, 24/7 support, and priority updates";
            radioEnterprise.Location = new Point(30, 140);
            radioEnterprise.Size = new Size(380, 70);
            radioEnterprise.Style = RadioButtonAdvStyle.Office2016Colorful;
            radioEnterprise.Font = new Font("Segoe UI", 10F);
            radioEnterprise.ForeColor = Color.DarkBlue;
            radioEnterprise.WrapText = true;
            radioEnterprise.TextShadow = true;
            radioEnterprise.ShadowColor = Color.LightBlue;
            radioEnterprise.ShadowOffset = new Point(2, 2);
            this.Controls.Add(radioEnterprise);
        }
    }
}
```

## Text Effects Best Practices

### Shadow Guidelines

1. **Subtle is Better**: Use small offsets (1-3 pixels) for professional appearance
2. **Contrast Matters**: Ensure shadow color contrasts appropriately with text color
3. **Background Consideration**: Adjust shadow color based on background
4. **Performance**: Text shadows have minimal performance impact

### Recommended Shadow Combinations

| Background | Text Color | Shadow Color | Offset |
|------------|-----------|--------------|--------|
| White | Black | Light Gray | (1, 1) |
| Light Gray | Dark Blue | White | (2, 2) |
| Black | White | Dark Gray | (2, 2) |
| Colorful | White | Black (50% opacity) | (2, 2) |

### Text Wrapping Guidelines

1. **Set Adequate Height**: Ensure control height accommodates wrapped text
2. **Use AutoHeight**: Combine with `AutoHeight` for dynamic sizing
3. **Alignment Consideration**: Text wrapping works best with `WrapText = true` and `TextContentAlignment = MiddleLeft`
4. **Font Size**: Larger fonts require more height when wrapping

### Common Pitfalls

**Avoid these combinations:**
```csharp
// DON'T: Text wrapping with TextContentAlignment other than Left alignments
radioButton.WrapText = true;
radioButton.TextContentAlignment = ContentAlignment.MiddleCenter; // Can cause issues
```

**Use instead:**
```csharp
// DO: Text wrapping with left alignment
radioButton.WrapText = true;
radioButton.TextContentAlignment = ContentAlignment.MiddleLeft;
```

## Troubleshooting

### Shadow Not Visible

If the text shadow isn't visible:
1. Verify `TextShadow = true`
2. Check that `ShadowColor` contrasts with `ForeColor`
3. Ensure `ShadowOffset` is not (0, 0)
4. Verify the control's background allows shadow visibility

### Text Truncation

If text is being truncated:
1. Enable `WrapText = true`
2. Increase control height
3. Consider using `AutoHeight = true`
4. Check control width is sufficient

### Wrapped Text Overlaps

If wrapped text overlaps or looks incorrect:
1. Ensure adequate control height
2. Use `AutoHeight` for automatic sizing
3. Verify font size is appropriate for control size
