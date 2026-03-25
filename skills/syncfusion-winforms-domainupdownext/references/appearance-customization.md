# Appearance and Customization

## Table of Contents
- [Border Styles](#border-styles)
- [Border Properties](#border-properties)
- [Colors and Background](#colors-and-background)
- [Text Styling](#text-styling)
- [Complete Customization Examples](#complete-customization-examples)

## Border Styles

The BorderStyle property controls the border appearance of the DomainUpdownExt control.

### Available BorderStyles

**No Border:**
```csharp
domainUpDownExt1.BorderStyle = BorderStyle.None;
```
- Minimal appearance
- Use for embedded layouts

**Fixed Single Border:**
```csharp
domainUpDownExt1.BorderStyle = BorderStyle.FixedSingle;
```
- Standard, clean look
- Recommended for most applications
- Single-pixel border

**Fixed 3D Border:**
```csharp
domainUpDownExt1.BorderStyle = BorderStyle.Fixed3D;
```
- Raised/embossed effect
- Provides visual depth
- Classic Windows appearance

## Border Properties

### BorderColor

Set the color of the border:

```csharp
domainUpDownExt1.BorderStyle = BorderStyle.FixedSingle;
domainUpDownExt1.BorderColor = System.Drawing.Color.DodgerBlue;
```

### Common BorderColor Examples

```csharp
// Professional colors
domainUpDownExt1.BorderColor = Color.Navy;          // Dark blue
domainUpDownExt1.BorderColor = Color.Gray;          // Neutral gray
domainUpDownExt1.BorderColor = Color.DarkGray;      // Darker shade

// Status colors
domainUpDownExt1.BorderColor = Color.Green;         // Valid/success
domainUpDownExt1.BorderColor = Color.Red;           // Error/invalid
domainUpDownExt1.BorderColor = Color.Orange;        // Warning

// Custom color
domainUpDownExt1.BorderColor = Color.FromArgb(70, 130, 180); // Steel blue
```

### Border3DStyle

Configure the 3D effect appearance:

```csharp
domainUpDownExt1.BorderStyle = BorderStyle.Fixed3D;
domainUpDownExt1.Border3DStyle = Border3DStyle.Bump;    // Raised appearance
domainUpDownExt1.Border3DStyle = Border3DStyle.Etched;  // Recessed appearance
domainUpDownExt1.Border3DStyle = Border3DStyle.Flat;    // Flat appearance
domainUpDownExt1.Border3DStyle = Border3DStyle.Raised;  // Elevated effect
domainUpDownExt1.Border3DStyle = Border3DStyle.Sunken;  // Sunken effect
```

### BorderSides

Control which sides display the border:

```csharp
// All sides (default)
domainUpDownExt1.BorderSides = Border3DSide.All;

// Only right side
domainUpDownExt1.BorderSides = Border3DSide.Right;

// Top and bottom
domainUpDownExt1.BorderSides = Border3DSide.Top | Border3DSide.Bottom;

// Left and bottom
domainUpDownExt1.BorderSides = Border3DSide.Left | Border3DSide.Bottom;
```

## Colors and Background

### BackColor

Set the background color of the text area:

```csharp
// Standard colors
domainUpDownExt1.BackColor = Color.White;           // Default
domainUpDownExt1.BackColor = Color.LightGray;       // Subtle background
domainUpDownExt1.BackColor = Color.LightBlue;       // Themed background

// Custom background
domainUpDownExt1.BackColor = Color.FromArgb(240, 248, 255); // Alice blue
```

### ForeColor

Set the text color:

```csharp
domainUpDownExt1.ForeColor = Color.Black;           // Default
domainUpDownExt1.ForeColor = Color.DarkBlue;        // Themed text
domainUpDownExt1.ForeColor = Color.White;           // Light text on dark background
```

### Complete Color Scheme Example

```csharp
public void ApplyProfessionalTheme()
{
    domainUpDownExt1.BackColor = Color.White;
    domainUpDownExt1.ForeColor = Color.Black;
    domainUpDownExt1.BorderStyle = BorderStyle.FixedSingle;
    domainUpDownExt1.BorderColor = Color.FromArgb(200, 200, 200);
}

public void ApplyDarkTheme()
{
    domainUpDownExt1.BackColor = Color.FromArgb(45, 45, 45);
    domainUpDownExt1.ForeColor = Color.White;
    domainUpDownExt1.BorderStyle = BorderStyle.FixedSingle;
    domainUpDownExt1.BorderColor = Color.FromArgb(100, 100, 100);
}

public void ApplyHighContrastTheme()
{
    domainUpDownExt1.BackColor = Color.Yellow;
    domainUpDownExt1.ForeColor = Color.Black;
    domainUpDownExt1.BorderStyle = BorderStyle.Fixed3D;
    domainUpDownExt1.BorderColor = Color.Black;
    domainUpDownExt1.BorderSides = Border3DSide.All;
}
```

## Text Styling

### Font Configuration

```csharp
// Change font
domainUpDownExt1.Font = new Font("Arial", 10, FontStyle.Regular);

// Bold text
domainUpDownExt1.Font = new Font("Arial", 10, FontStyle.Bold);

// Italic text
domainUpDownExt1.Font = new Font("Arial", 10, FontStyle.Italic);

// Combine styles
domainUpDownExt1.Font = new Font("Arial", 10, FontStyle.Bold | FontStyle.Italic);
```

### Text Alignment

```csharp
domainUpDownExt1.TextAlign = HorizontalAlignment.Left;    // Default
domainUpDownExt1.TextAlign = HorizontalAlignment.Center;  // Centered
domainUpDownExt1.TextAlign = HorizontalAlignment.Right;   // Right-aligned
```

## Complete Customization Examples

### Example 1: Professional Data Entry Form

```csharp
private void SetupProfessionalAppearance()
{
    domainUpDownExt1.BorderStyle = BorderStyle.FixedSingle;
    domainUpDownExt1.BorderColor = Color.FromArgb(180, 180, 180);
    domainUpDownExt1.BackColor = Color.White;
    domainUpDownExt1.ForeColor = Color.Black;
    domainUpDownExt1.Font = new Font("Segoe UI", 9, FontStyle.Regular);
    domainUpDownExt1.TextAlign = HorizontalAlignment.Left;
    domainUpDownExt1.Height = 24;
}
```

### Example 2: Status Indicator Control

```csharp
private void SetupStatusControl()
{
    domainUpDownExt1.Items.Add("Normal");
    domainUpDownExt1.Items.Add("Warning");
    domainUpDownExt1.Items.Add("Error");
    
    domainUpDownExt1.BorderStyle = BorderStyle.FixedSingle;
    domainUpDownExt1.BackColor = Color.FromArgb(240, 240, 240);
    domainUpDownExt1.Font = new Font("Arial", 10, FontStyle.Bold);
    domainUpDownExt1.TextAlign = HorizontalAlignment.Center;
}
```

### Example 3: Disabled State Appearance

```csharp
private void SetupDisabledAppearance()
{
    domainUpDownExt1.Enabled = false;
    domainUpDownExt1.BackColor = Color.FromArgb(240, 240, 240);  // Light gray
    domainUpDownExt1.ForeColor = Color.FromArgb(128, 128, 128);  // Muted text
    domainUpDownExt1.BorderColor = Color.FromArgb(200, 200, 200);
}
```

### Example 4: High Visibility Design

```csharp
private void SetupHighVisibilityAppearance()
{
    domainUpDownExt1.BackColor = Color.Yellow;
    domainUpDownExt1.ForeColor = Color.Black;
    domainUpDownExt1.BorderStyle = BorderStyle.Fixed3D;
    domainUpDownExt1.Border3DStyle = Border3DStyle.Raised;
    domainUpDownExt1.BorderSides = Border3DSide.All;
    domainUpDownExt1.Font = new Font("Arial", 12, FontStyle.Bold);
}
```

### Example 5: Themed Application Control

```csharp
public class ThemedDomainUpDown
{
    public static void ApplyTheme(DomainUpDownExt control, string theme)
    {
        switch (theme)
        {
            case "Light":
                control.BackColor = Color.White;
                control.ForeColor = Color.Black;
                control.BorderColor = Color.LightGray;
                break;
                
            case "Dark":
                control.BackColor = Color.FromArgb(30, 30, 30);
                control.ForeColor = Color.White;
                control.BorderColor = Color.FromArgb(80, 80, 80);
                break;
                
            case "Ocean":
                control.BackColor = Color.FromArgb(230, 240, 250);
                control.ForeColor = Color.FromArgb(0, 51, 102);
                control.BorderColor = Color.FromArgb(0, 102, 204);
                break;
                
            case "Forest":
                control.BackColor = Color.FromArgb(240, 248, 245);
                control.ForeColor = Color.FromArgb(34, 102, 68);
                control.BorderColor = Color.FromArgb(0, 102, 51);
                break;
        }
        
        // Common settings
        control.BorderStyle = BorderStyle.FixedSingle;
        control.Font = new Font("Segoe UI", 9, FontStyle.Regular);
    }
}
```

## Practical Scenarios

### Scenario 1: Form Validation Styling

```csharp
public void HighlightInvalidField()
{
    domainUpDownExt1.BackColor = Color.FromArgb(255, 220, 220);  // Light red
    domainUpDownExt1.BorderColor = Color.Red;
    domainUpDownExt1.BorderStyle = BorderStyle.FixedSingle;
}

public void HighlightValidField()
{
    domainUpDownExt1.BackColor = Color.White;
    domainUpDownExt1.BorderColor = Color.Green;
    domainUpDownExt1.BorderStyle = BorderStyle.FixedSingle;
}
```

### Scenario 2: Accessibility - High Contrast Mode

```csharp
public void ConfigureAccessibleAppearance()
{
    domainUpDownExt1.BackColor = Color.Black;
    domainUpDownExt1.ForeColor = Color.Yellow;
    domainUpDownExt1.BorderStyle = BorderStyle.Fixed3D;
    domainUpDownExt1.BorderSides = Border3DSide.All;
    domainUpDownExt1.Font = new Font("Arial", 12, FontStyle.Bold);
}
```
