# Appearance and Theming in Windows Forms AutoLabel

This section explains how to customize the appearance of AutoLabel controls and apply themes.

## Size Settings

### AutoSize Property

The AutoLabel control can be resized automatically based on the font size using the `AutoSize` property.

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `AutoSize` | bool | false | Enables automatic resizing based on the font size |

> **Note**: AutoSize is valid only for label controls that do not wrap text.

```csharp
this.autoLabel1.AutoSize = true;
```

```vb
Me.autoLabel1.AutoSize = True
```

When `AutoSize` is enabled, the label will automatically adjust its dimensions to fit the text content based on the font size.

## Color Customization

### BackColor Property

Sets the background color of the AutoLabel.

```csharp
this.autoLabel1.BackColor = System.Drawing.Color.LightBlue;
```

```vb
Me.autoLabel1.BackColor = System.Drawing.Color.LightBlue
```

### ForeColor Property

Sets the text color of the AutoLabel.

```csharp
this.autoLabel1.ForeColor = System.Drawing.Color.DarkBlue;
```

```vb
Me.autoLabel1.ForeColor = System.Drawing.Color.DarkBlue
```

### Combined Example

```csharp
this.autoLabel1.BackColor = System.Drawing.Color.DarkGray;
this.autoLabel1.ForeColor = System.Drawing.Color.White;
```

## Font Customization

The Font property allows you to customize the text appearance including font family, size, and style.

```csharp
this.autoLabel1.Font = new System.Drawing.Font("Segoe UI", 10F, System.Drawing.FontStyle.Bold);
```

```vb
Me.autoLabel1.Font = New System.Drawing.Font("Segoe UI", 10F, System.Drawing.FontStyle.Bold)
```

### Font Style Options

```csharp
// Regular font
this.autoLabel1.Font = new Font("Arial", 9F, FontStyle.Regular);

// Bold font
this.autoLabel2.Font = new Font("Arial", 9F, FontStyle.Bold);

// Italic font
this.autoLabel3.Font = new Font("Arial", 9F, FontStyle.Italic);

// Bold and Italic
this.autoLabel4.Font = new Font("Arial", 9F, FontStyle.Bold | FontStyle.Italic);
```

## Text Alignment

The `TextAlign` property controls how text is aligned within the label bounds.

```csharp
this.autoLabel1.TextAlign = System.Drawing.ContentAlignment.MiddleCenter;
```

```vb
Me.autoLabel1.TextAlign = System.Drawing.ContentAlignment.MiddleCenter
```

### Alignment Options

| Alignment | Description |
|-----------|-------------|
| `TopLeft` | Text aligned to top-left corner |
| `TopCenter` | Text centered at top |
| `TopRight` | Text aligned to top-right corner |
| `MiddleLeft` | Text vertically centered, aligned left |
| `MiddleCenter` | Text centered both horizontally and vertically |
| `MiddleRight` | Text vertically centered, aligned right |
| `BottomLeft` | Text aligned to bottom-left corner |
| `BottomCenter` | Text centered at bottom |
| `BottomRight` | Text aligned to bottom-right corner |

```csharp
// Center-aligned label
this.autoLabel1.TextAlign = ContentAlignment.MiddleCenter;

// Left-aligned label (common for form labels)
this.autoLabel2.TextAlign = ContentAlignment.MiddleLeft;
```

## Theming with SkinManager

You can apply professional Office 2016 themes to AutoLabel controls using the SkinManager.

### Supported Themes

The AutoLabel control supports the following Office 2016 visual themes:
- Office2016Colorful
- Office2016Black
- Office2016DarkGray
- Office2016White

### Applying Themes

```csharp
using Syncfusion.Windows.Forms;

// Apply Office 2016 Colorful theme
SkinManager.SetVisualStyle(this.autoLabel1, VisualTheme.Office2016Colorful);
```

```vb
Imports Syncfusion.Windows.Forms

' Apply Office 2016 Colorful theme
SkinManager.SetVisualStyle(Me.autoLabel1, VisualTheme.Office2016Colorful)
```

### Theme Examples

#### Office2016Colorful

```csharp
SkinManager.SetVisualStyle(this.autoLabel1, VisualTheme.Office2016Colorful);
```

#### Office2016Black

```csharp
SkinManager.SetVisualStyle(this.autoLabel1, VisualTheme.Office2016Black);
```

#### Office2016DarkGray

```csharp
SkinManager.SetVisualStyle(this.autoLabel1, VisualTheme.Office2016DarkGray);
```

#### Office2016White

```csharp
SkinManager.SetVisualStyle(this.autoLabel1, VisualTheme.Office2016White);
```

### Applying Theme to All Labels

```csharp
private void ApplyThemeToAllLabels(VisualTheme theme)
{
    foreach (Control control in this.Controls)
    {
        if (control is AutoLabel)
        {
            SkinManager.SetVisualStyle((AutoLabel)control, theme);
        }
    }
}

// Usage
ApplyThemeToAllLabels(VisualTheme.Office2016Colorful);
```

## Complete Customization Example

Here's a comprehensive example showing various appearance customizations:

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace AutoLabelAppearance
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
            
            TextBox textBox1 = new TextBox();
            textBox1.Location = new Point(200, 50);
            textBox1.Size = new Size(200, 20);
            
            // Create styled AutoLabel
            AutoLabel styledLabel = new AutoLabel();
            styledLabel.Text = "Styled Label:";
            styledLabel.LabeledControl = textBox1;
            styledLabel.Position = AutoLabelPosition.Left;
            styledLabel.Gap = 10;
            
            // Customize appearance
            styledLabel.AutoSize = true;
            styledLabel.BackColor = Color.LightSteelBlue;
            styledLabel.ForeColor = Color.DarkBlue;
            styledLabel.Font = new Font("Segoe UI", 10F, FontStyle.Bold);
            styledLabel.TextAlign = ContentAlignment.MiddleRight;
            
            // Apply Office 2016 theme
            SkinManager.SetVisualStyle(styledLabel, VisualTheme.Office2016Colorful);
            
            this.Controls.Add(textBox1);
            this.Controls.Add(styledLabel);
        }
    }
}
```

## Best Practices

### 1. Consistent Styling

Use consistent colors, fonts, and sizes across all labels in your form:

```csharp
// Define standard styling
Color labelBackColor = Color.LightGray;
Color labelForeColor = Color.Black;
Font labelFont = new Font("Segoe UI", 9F, FontStyle.Regular);

// Apply to all labels
label1.BackColor = labelBackColor;
label1.ForeColor = labelForeColor;
label1.Font = labelFont;

label2.BackColor = labelBackColor;
label2.ForeColor = labelForeColor;
label2.Font = labelFont;
```

### 2. Use AutoSize for Dynamic Content

Enable AutoSize when label text length varies:

```csharp
autoLabel1.AutoSize = true;
autoLabel1.Text = "Variable Length Text Here";
```

### 3. Theme Consistency

Apply the same theme to all Syncfusion controls in your application:

```csharp
private void ApplyApplicationTheme(Form form, VisualTheme theme)
{
    foreach (Control control in form.Controls)
    {
        if (control is AutoLabel)
        {
            SkinManager.SetVisualStyle((AutoLabel)control, theme);
        }
        // Add other Syncfusion control types as needed
    }
}
```

### 4. Contrast for Readability

Ensure sufficient contrast between BackColor and ForeColor:

```csharp
// Good contrast
autoLabel1.BackColor = Color.DarkBlue;
autoLabel1.ForeColor = Color.White;

// Poor contrast (avoid)
// autoLabel1.BackColor = Color.LightGray;
// autoLabel1.ForeColor = Color.White;
```

### 5. Font Size for Accessibility

Use readable font sizes (9pt or larger) for better accessibility:

```csharp
// Good: Readable size
autoLabel1.Font = new Font("Segoe UI", 9F);

// Better: Larger for accessibility
autoLabel1.Font = new Font("Segoe UI", 10F);
```

## Common Patterns

### Pattern 1: Professional Form Labels

```csharp
AutoLabel label = new AutoLabel();
label.Font = new Font("Segoe UI", 9F, FontStyle.Regular);
label.ForeColor = Color.FromArgb(64, 64, 64);  // Dark gray
label.AutoSize = true;
label.TextAlign = ContentAlignment.MiddleLeft;
```

### Pattern 2: Bold Required Field Labels

```csharp
AutoLabel requiredLabel = new AutoLabel();
requiredLabel.Text = "Username: *";
requiredLabel.Font = new Font("Segoe UI", 9F, FontStyle.Bold);
requiredLabel.ForeColor = Color.DarkRed;  // Indicate required
```

### Pattern 3: Themed Form

```csharp
// Apply consistent Office 2016 theme to entire form
private void SetupThemedForm()
{
    VisualTheme theme = VisualTheme.Office2016Colorful;
    
    foreach (Control ctrl in this.Controls)
    {
        if (ctrl is AutoLabel)
        {
            SkinManager.SetVisualStyle((AutoLabel)ctrl, theme);
        }
    }
}
```

## Troubleshooting

**Issue**: AutoSize not working
- Verify AutoSize is set to true
- Check that text doesn't wrap (AutoSize doesn't work with wrapped text)
- Ensure font is set before enabling AutoSize

**Issue**: Theme not applied
- Ensure you've referenced Syncfusion.Windows.Forms assembly
- Verify the theme value is one of the four supported Office 2016 variants
- Apply theme after adding control to form

**Issue**: Colors not showing
- Check if a theme is overriding custom colors
- Apply custom colors after setting the theme
- Verify parent form BackColor allows label BackColor to show

**Issue**: Text not aligned properly
- Ensure AutoSize is false if using TextAlign
- Check label size is sufficient for text with desired alignment
- Verify TextAlign value is appropriate for the layout
