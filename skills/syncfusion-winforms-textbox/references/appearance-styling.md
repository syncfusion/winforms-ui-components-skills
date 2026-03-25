# Appearance and Styling

This guide covers visual appearance customization for the TextBoxExt control, including background colors, foreground colors, and built-in visual styles.

## Background Settings

The `BackColor` property sets the background color of the textbox.

### Basic Background Color

**C#:**
```csharp
using System.Drawing;

// Set background color
textBoxExt1.BackColor = Color.Moccasin;
```

**VB.NET:**
```vb
Imports System.Drawing

' Set background color
textBoxExt1.BackColor = Color.Moccasin
```

![Set the backcolor of WF TextBoxExt control](../../../../../docs/Appearance-Settings_images/Appearance-Settings_img1.png)

### Common Background Colors

**White (Standard):**
```csharp
textBoxExt1.BackColor = Color.White;
```

**Light Gray (Disabled appearance):**
```csharp
textBoxExt1.BackColor = Color.LightGray;
```

**Light Yellow (Highlight/Warning):**
```csharp
textBoxExt1.BackColor = Color.LightYellow;
```

**Light Blue (Information):**
```csharp
textBoxExt1.BackColor = Color.LightBlue;
```

**Light Green (Success):**
```csharp
textBoxExt1.BackColor = Color.LightGreen;
```

**Light Pink (Error):**
```csharp
textBoxExt1.BackColor = Color.FromArgb(255, 240, 240);
```

### Custom RGB Background

**C#:**
```csharp
// Custom RGB color
textBoxExt1.BackColor = Color.FromArgb(240, 248, 255); // Alice Blue

// From hex color
textBoxExt1.BackColor = ColorTranslator.FromHtml("#F0F8FF");
```

**VB.NET:**
```vb
' Custom RGB color
textBoxExt1.BackColor = Color.FromArgb(240, 248, 255) ' Alice Blue

' From hex color
textBoxExt1.BackColor = ColorTranslator.FromHtml("#F0F8FF")
```

### System Colors

Use system colors for consistent appearance with Windows theme:

```csharp
// System background colors
textBoxExt1.BackColor = SystemColors.Window; // Standard window background
textBoxExt1.BackColor = SystemColors.Control; // Control background
textBoxExt1.BackColor = SystemColors.Info; // Tooltip background (light yellow)
```

## Foreground Settings

The `ForeColor` property sets the text color.

### Basic Text Color

**C#:**
```csharp
using System.Drawing;

// Set text color
textBoxExt1.ForeColor = Color.LightSeaGreen;
```

**VB.NET:**
```vb
Imports System.Drawing

' Set text color
textBoxExt1.ForeColor = Color.LightSeaGreen
```

![Set the fore ground color of WF TextBoxExt control](../../../../../docs/Appearance-Settings_images/Appearance-Settings_img2.png)

### Common Text Colors

**Black (Default):**
```csharp
textBoxExt1.ForeColor = Color.Black;
```

**Dark Gray (Subtle):**
```csharp
textBoxExt1.ForeColor = Color.DarkGray;
```

**Gray (Placeholder text):**
```csharp
textBoxExt1.ForeColor = Color.Gray;
```

**Red (Error/Warning):**
```csharp
textBoxExt1.ForeColor = Color.Red;
```

**Blue (Links/Information):**
```csharp
textBoxExt1.ForeColor = Color.Blue;
```

**Green (Success):**
```csharp
textBoxExt1.ForeColor = Color.Green;
```

### System Text Colors

```csharp
// System text colors
textBoxExt1.ForeColor = SystemColors.WindowText; // Standard window text
textBoxExt1.ForeColor = SystemColors.ControlText; // Control text
textBoxExt1.ForeColor = SystemColors.GrayText; // Disabled text
```

## Visual Styles

The `Style` property applies predefined visual themes to the textbox.

### Available Styles

TextBoxExt supports eight built-in visual styles:

| Style | Description | When to Use |
|-------|-------------|-------------|
| `Office2016Colorful` | Colorful Office 2016 theme | Modern Office applications |
| `Office2016White` | White Office 2016 theme | Clean, minimal design |
| `Office2016Black` | Black Office 2016 theme | Dark mode applications |
| `Office2016DarkGray` | Dark gray Office 2016 theme | Reduced contrast dark mode |
| `Office2019Colorful` | Office 2019 colorful theme | Latest Office styling |
| `Metro` | Flat modern design | Windows 8/10/11 style apps |
| `Office2007` | Classic Office 2007 Ribbon | Traditional Office look |
| `Office2010` | Office 2010 style | Refined Office appearance |
| `Default` | Standard Windows Forms | No special styling |

### Applying Styles

**C#:**
```csharp
using Syncfusion.Windows.Forms.Tools;

// Apply Office2016 Colorful theme
textBoxExt1.Style = TextBoxExt.theme.Office2016Colorful;
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms.Tools

' Apply Office2016 Colorful theme
textBoxExt1.Style = TextBoxExt.theme.Office2016Colorful
```

![Set the visual style to WF TextBoxExt control](../../../../../docs/Appearance-Settings_images/Appearance-Settings_img3.png)

## Style-by-Style Examples

### Office2016Colorful

Modern, vibrant Office 2016 appearance with accent colors.

**C#:**
```csharp
textBoxExt1.Style = TextBoxExt.theme.Office2016Colorful;
textBoxExt1.Text = "Office2016Colorful Style";
```

**VB.NET:**
```vb
textBoxExt1.Style = TextBoxExt.theme.Office2016Colorful
textBoxExt1.Text = "Office2016Colorful Style"
```

**Characteristics:**
- Colorful accents
- Modern flat design
- Clear borders

### Office2016White

Clean white theme with subtle borders.

**C#:**
```csharp
textBoxExt1.Style = TextBoxExt.theme.Office2016White;
textBoxExt1.Text = "Office2016White Style";
```

**VB.NET:**
```vb
textBoxExt1.Style = TextBoxExt.theme.Office2016White
textBoxExt1.Text = "Office2016White Style"
```

**Characteristics:**
- White background
- Light gray borders
- Minimal design

### Office2016Black

Dark theme for dark mode applications.

**C#:**
```csharp
textBoxExt1.Style = TextBoxExt.theme.Office2016Black;
textBoxExt1.Text = "Office2016Black Style";
```

**VB.NET:**
```vb
textBoxExt1.Style = TextBoxExt.theme.Office2016Black
textBoxExt1.Text = "Office2016Black Style"
```

**Characteristics:**
- Dark background
- Light text
- High contrast

### Office2016DarkGray

Reduced contrast dark theme.

**C#:**
```csharp
textBoxExt1.Style = TextBoxExt.theme.Office2016DarkGray;
textBoxExt1.Text = "Office2016DarkGray Style";
```

**VB.NET:**
```vb
textBoxExt1.Style = TextBoxExt.theme.Office2016DarkGray
textBoxExt1.Text = "Office2016DarkGray Style"
```

**Characteristics:**
- Dark gray background
- Softer contrast than Black theme
- Easier on eyes for extended use

### Office2019Colorful

Latest Office styling with refined colors.

**C#:**
```csharp
textBoxExt1.Style = TextBoxExt.theme.Office2019Colorful;
textBoxExt1.Text = "Office2019Colorful Style";
```

**VB.NET:**
```vb
textBoxExt1.Style = TextBoxExt.theme.Office2019Colorful
textBoxExt1.Text = "Office2019Colorful Style"
```

**Characteristics:**
- Modern Office 2019 design
- Refined accent colors
- Contemporary appearance

### Metro

Flat, modern Windows design.

**C#:**
```csharp
textBoxExt1.Style = TextBoxExt.theme.Metro;
textBoxExt1.Text = "Metro Style";
```

**VB.NET:**
```vb
textBoxExt1.Style = TextBoxExt.theme.Metro
textBoxExt1.Text = "Metro Style"
```

**Characteristics:**
- Completely flat design
- No gradients or shadows
- Windows 8/10/11 aesthetic

### Office2007

Classic Office 2007 Ribbon style.

**C#:**
```csharp
textBoxExt1.Style = TextBoxExt.theme.Office2007;
textBoxExt1.Text = "Office2007 Style";
```

**VB.NET:**
```vb
textBoxExt1.Style = TextBoxExt.theme.Office2007
textBoxExt1.Text = "Office2007 Style"
```

**Characteristics:**
- Traditional Office appearance
- Subtle gradients
- Classic Ribbon design

### Office2010

Refined Office 2010 appearance.

**C#:**
```csharp
textBoxExt1.Style = TextBoxExt.theme.Office2010;
textBoxExt1.Text = "Office2010 Style";
```

**VB.NET:**
```vb
textBoxExt1.Style = TextBoxExt.theme.Office2010
textBoxExt1.Text = "Office2010 Style"
```

**Characteristics:**
- Cleaner than Office2007
- Refined colors
- Professional appearance

### Default

Standard Windows Forms appearance without special styling.

**C#:**
```csharp
textBoxExt1.Style = TextBoxExt.theme.Default;
textBoxExt1.Text = "Default Style";
```

**VB.NET:**
```vb
textBoxExt1.Style = TextBoxExt.theme.Default
textBoxExt1.Text = "Default Style"
```

**Characteristics:**
- Standard Windows Forms look
- System theme integration
- No special styling

## Combining Colors and Styles

You can combine visual styles with custom colors:

**Example 1: Metro Style with Custom Background**
```csharp
textBoxExt1.Style = TextBoxExt.theme.Metro;
textBoxExt1.BackColor = Color.FromArgb(240, 240, 240); // Light gray
textBoxExt1.ForeColor = Color.FromArgb(33, 33, 33); // Dark gray text
```

**Example 2: Office2016Colorful with Accent Background**
```csharp
textBoxExt1.Style = TextBoxExt.theme.Office2016Colorful;
textBoxExt1.BackColor = Color.FromArgb(255, 251, 240); // Cream
textBoxExt1.ForeColor = Color.DarkSlateGray;
```

**Example 3: Custom Dark Theme**
```csharp
textBoxExt1.Style = TextBoxExt.theme.Office2016Black;
textBoxExt1.BackColor = Color.FromArgb(30, 30, 30); // Very dark gray
textBoxExt1.ForeColor = Color.FromArgb(220, 220, 220); // Light gray text
textBoxExt1.BorderColor = Color.FromArgb(70, 70, 70); // Dark border
```

## Practical Examples

### Example 1: Error State Styling

```csharp
using Syncfusion.Windows.Forms.Tools;
using System.Drawing;

TextBoxExt errorBox = new TextBoxExt();
errorBox.Style = TextBoxExt.theme.Office2016Colorful;

// Error state
errorBox.BackColor = Color.FromArgb(255, 235, 235); // Light red
errorBox.ForeColor = Color.DarkRed;
errorBox.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle;
errorBox.BorderColor = Color.Red;
errorBox.Text = "Invalid input";
```

### Example 2: Success State Styling

```csharp
TextBoxExt successBox = new TextBoxExt();
successBox.Style = TextBoxExt.theme.Office2016Colorful;

// Success state
successBox.BackColor = Color.FromArgb(235, 255, 235); // Light green
successBox.ForeColor = Color.DarkGreen;
successBox.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle;
successBox.BorderColor = Color.Green;
successBox.Text = "Valid input";
```

### Example 3: Readonly Display Field

```csharp
TextBoxExt displayBox = new TextBoxExt();
displayBox.Style = TextBoxExt.theme.Metro;

// Readonly display
displayBox.ReadOnly = true;
displayBox.BackColor = Color.FromArgb(245, 245, 245); // Very light gray
displayBox.ForeColor = Color.Black;
displayBox.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle;
displayBox.BorderColor = Color.LightGray;
displayBox.Text = "Read-only information";
```

### Example 4: Highlighted Important Field

```csharp
TextBoxExt importantBox = new TextBoxExt();
importantBox.Style = TextBoxExt.theme.Office2016Colorful;

// Highlighted field
importantBox.BackColor = Color.LightYellow;
importantBox.ForeColor = Color.Black;
importantBox.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle;
importantBox.BorderColor = Color.Gold;
importantBox.Text = "Important: Required field";
```

### Example 5: Application-Wide Style Consistency

```csharp
using Syncfusion.Windows.Forms.Tools;

public class AppStyles
{
    // Define application theme
    public static TextBoxExt.theme CurrentTheme = TextBoxExt.theme.Office2016Colorful;
    
    // Apply standard style to textbox
    public static void ApplyStandardStyle(TextBoxExt textBox)
    {
        textBox.Style = CurrentTheme;
        textBox.BackColor = Color.White;
        textBox.ForeColor = Color.Black;
    }
    
    // Apply error style
    public static void ApplyErrorStyle(TextBoxExt textBox)
    {
        textBox.Style = CurrentTheme;
        textBox.BackColor = Color.FromArgb(255, 240, 240);
        textBox.ForeColor = Color.DarkRed;
        textBox.BorderColor = Color.Red;
    }
    
    // Apply success style
    public static void ApplySuccessStyle(TextBoxExt textBox)
    {
        textBox.Style = CurrentTheme;
        textBox.BackColor = Color.FromArgb(240, 255, 240);
        textBox.ForeColor = Color.DarkGreen;
        textBox.BorderColor = Color.Green;
    }
}

// Usage
AppStyles.ApplyStandardStyle(textBoxExt1);
```

## Theme Comparison Chart

| Feature | Office2016 Variants | Office2019 | Metro | Office2007/2010 | Default |
|---------|---------------------|------------|-------|-----------------|---------|
| Modern Design | ✓ | ✓ | ✓ | ✗ | ✗ |
| Flat Appearance | ✓ | ✓ | ✓ | ✗ | ✗ |
| Dark Mode Support | ✓ (Black/DarkGray) | ✗ | ✗ | ✗ | ✗ |
| Colorful Accents | ✓ (Colorful) | ✓ | ✗ | ✗ | ✗ |
| System Integration | ✗ | ✗ | ✗ | ✗ | ✓ |
| Best for | Modern Office apps | Latest Office | Windows 10/11 | Legacy apps | Generic apps |

## Best Practices

### Consistency

Apply the same style across all TextBoxExt controls in your application:

```csharp
// In form constructor or OnLoad
private void ApplyApplicationStyle()
{
    var theme = TextBoxExt.theme.Office2016Colorful;
    
    foreach (Control control in this.Controls)
    {
        if (control is TextBoxExt textBox)
        {
            textBox.Style = theme;
        }
    }
}
```

### Accessibility

Ensure sufficient contrast between text and background:

```csharp
// Good: High contrast
textBoxExt1.BackColor = Color.White;
textBoxExt1.ForeColor = Color.Black;

// Avoid: Low contrast
// textBoxExt1.BackColor = Color.LightGray;
// textBoxExt1.ForeColor = Color.DarkGray;
```

### Dark Mode

When implementing dark mode, use appropriate dark themes:

```csharp
public void ApplyDarkMode(bool isDark)
{
    if (isDark)
    {
        textBoxExt1.Style = TextBoxExt.theme.Office2016Black;
        textBoxExt1.BackColor = Color.FromArgb(45, 45, 48);
        textBoxExt1.ForeColor = Color.FromArgb(241, 241, 241);
    }
    else
    {
        textBoxExt1.Style = TextBoxExt.theme.Office2016Colorful;
        textBoxExt1.BackColor = Color.White;
        textBoxExt1.ForeColor = Color.Black;
    }
}
```

### User Preferences

Allow users to select their preferred theme:

```csharp
public void SetUserTheme(string themeName)
{
    switch (themeName)
    {
        case "Colorful":
            textBoxExt1.Style = TextBoxExt.theme.Office2016Colorful;
            break;
        case "White":
            textBoxExt1.Style = TextBoxExt.theme.Office2016White;
            break;
        case "Black":
            textBoxExt1.Style = TextBoxExt.theme.Office2016Black;
            break;
        case "Metro":
            textBoxExt1.Style = TextBoxExt.theme.Metro;
            break;
        default:
            textBoxExt1.Style = TextBoxExt.theme.Default;
            break;
    }
}
```

## Summary

TextBoxExt appearance customization provides:

- **BackColor** for background color customization
- **ForeColor** for text color control
- **Style** property with eight built-in themes
- Combination of styles and custom colors for unique looks
- Dark mode support with Office2016Black and Office2016DarkGray
- Modern themes (Metro, Office2016/2019) for contemporary applications
- Classic themes (Office2007/2010, Default) for traditional interfaces

These styling options enable you to create visually consistent, accessible, and attractive user interfaces that match your application's design language.
