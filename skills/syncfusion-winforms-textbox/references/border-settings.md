# Border Settings

This guide covers border customization options for the TextBoxExt control, including border styles, colors, 3D effects, and side configurations.

## Overview

The TextBoxExt control provides extensive border customization through four main properties:

- **BorderStyle** - Overall border type (Fixed3D, FixedSingle, None)
- **Border3DStyle** - 3D visual effect (Raised, Sunken, Etched, etc.)
- **BorderColor** - Custom border color
- **BorderSides** - Which sides display the border (Top, Bottom, Left, Right, All)

These properties allow you to create visually distinct textboxes that match your application's design.

## BorderStyle Property

The `BorderStyle` property sets the overall border type.

### BorderStyle Options

| Value | Description | Appearance |
|-------|-------------|------------|
| `BorderStyle.None` | No border | Flat, borderless |
| `BorderStyle.FixedSingle` | Simple single-line border | Thin line border |
| `BorderStyle.Fixed3D` | 3D border effect | Raised/sunken appearance |

### None (Borderless)

**C#:**
```csharp
using System.Windows.Forms;

// Remove border completely
textBoxExt1.BorderStyle = BorderStyle.None;
```

**VB.NET:**
```vb
Imports System.Windows.Forms

' Remove border completely
textBoxExt1.BorderStyle = BorderStyle.None
```

**Use Case:** Embedded textboxes in panels or containers where borders are unnecessary.

### FixedSingle (Flat Border)

**C#:**
```csharp
// Apply flat single-line border
textBoxExt1.BorderStyle = BorderStyle.FixedSingle;
```

**VB.NET:**
```vb
' Apply flat single-line border
textBoxExt1.BorderStyle = BorderStyle.FixedSingle
```

**Use Case:** Modern, flat design interfaces (Windows 10/11 style).

### Fixed3D (Three-Dimensional Border)

**C#:**
```csharp
// Apply 3D border
textBoxExt1.BorderStyle = BorderStyle.Fixed3D;
```

**VB.NET:**
```vb
' Apply 3D border
textBoxExt1.BorderStyle = BorderStyle.Fixed3D
```

**Use Case:** Traditional Windows forms with raised or sunken appearance.

## Border3DStyle Property

When `BorderStyle` is set to `Fixed3D`, the `Border3DStyle` property controls the 3D visual effect.

### Border3DStyle Options

| Value | Description | Visual Effect |
|-------|-------------|---------------|
| `Border3DStyle.Raised` | Border appears raised above surface | Convex appearance |
| `Border3DStyle.Sunken` | Border appears sunken into surface | Concave appearance (default) |
| `Border3DStyle.Etched` | Etched line effect | Carved appearance |
| `Border3DStyle.Bump` | Raised bump effect | Subtle convex ridge |
| `Border3DStyle.Flat` | Flat 3D border | No depth effect |
| `Border3DStyle.RaisedOuter` | Raised outer edge only | Partial raise |
| `Border3DStyle.RaisedInner` | Raised inner edge only | Inner highlight |
| `Border3DStyle.SunkenOuter` | Sunken outer edge only | Partial depression |
| `Border3DStyle.SunkenInner` | Sunken inner edge only | Inner shadow |
| `Border3DStyle.Adjust` | Adjusts rectangle for border | Layout adjustment |

### Raised Border

**C#:**
```csharp
using System.Windows.Forms;

// Create raised 3D border
textBoxExt1.BorderStyle = BorderStyle.Fixed3D;
textBoxExt1.Border3DStyle = Border3DStyle.Raised;
```

**VB.NET:**
```vb
Imports System.Windows.Forms

' Create raised 3D border
textBoxExt1.BorderStyle = BorderStyle.Fixed3D
textBoxExt1.Border3DStyle = Border3DStyle.Raised
```

**Result:** Textbox appears elevated from the form surface.

### Sunken Border (Default)

**C#:**
```csharp
// Create sunken 3D border (default for Fixed3D)
textBoxExt1.BorderStyle = BorderStyle.Fixed3D;
textBoxExt1.Border3DStyle = Border3DStyle.Sunken;
```

**VB.NET:**
```vb
' Create sunken 3D border (default for Fixed3D)
textBoxExt1.BorderStyle = BorderStyle.Fixed3D
textBoxExt1.Border3DStyle = Border3DStyle.Sunken
```

**Result:** Textbox appears recessed into the form surface (standard input field appearance).

### Etched Border

**C#:**
```csharp
// Create etched line effect
textBoxExt1.BorderStyle = BorderStyle.Fixed3D;
textBoxExt1.Border3DStyle = Border3DStyle.Etched;
```

**VB.NET:**
```vb
' Create etched line effect
textBoxExt1.BorderStyle = BorderStyle.Fixed3D
textBoxExt1.Border3DStyle = Border3DStyle.Etched
```

**Result:** Border looks carved into the surface.

## BorderColor Property

The `BorderColor` property sets a custom color for the border.

**Note:** `BorderColor` is most effective with `BorderStyle.FixedSingle`. For `Fixed3D` borders, the color effect may be limited.

### Basic Color Setting

**C#:**
```csharp
using System.Drawing;
using System.Windows.Forms;

// Set custom border color
textBoxExt1.BorderStyle = BorderStyle.FixedSingle;
textBoxExt1.BorderColor = Color.Orchid;
```

**VB.NET:**
```vb
Imports System.Drawing
Imports System.Windows.Forms

' Set custom border color
textBoxExt1.BorderStyle = BorderStyle.FixedSingle
textBoxExt1.BorderColor = Color.Orchid
```

![Change the border style and color in WF TextBoxExt](../../../../../docs/Border-Settings_images/Border-Settings_img1.png)

### Common Border Colors

**Red (Error/Required):**
```csharp
textBoxExt1.BorderStyle = BorderStyle.FixedSingle;
textBoxExt1.BorderColor = Color.Red;
```

**Blue (Focus/Active):**
```csharp
textBoxExt1.BorderStyle = BorderStyle.FixedSingle;
textBoxExt1.BorderColor = Color.FromArgb(0, 120, 215); // Windows blue
```

**Green (Success/Valid):**
```csharp
textBoxExt1.BorderStyle = BorderStyle.FixedSingle;
textBoxExt1.BorderColor = Color.Green;
```

**Gray (Disabled/Inactive):**
```csharp
textBoxExt1.BorderStyle = BorderStyle.FixedSingle;
textBoxExt1.BorderColor = Color.LightGray;
```

### Custom RGB Colors

**C#:**
```csharp
// Use RGB values
textBoxExt1.BorderColor = Color.FromArgb(255, 87, 34); // Deep Orange

// Use hex color (requires conversion)
textBoxExt1.BorderColor = ColorTranslator.FromHtml("#FF5722");
```

**VB.NET:**
```vb
' Use RGB values
textBoxExt1.BorderColor = Color.FromArgb(255, 87, 34) ' Deep Orange

' Use hex color (requires conversion)
textBoxExt1.BorderColor = ColorTranslator.FromHtml("#FF5722")
```

## BorderSides Property

The `BorderSides` property controls which sides of the textbox display a border.

### BorderSides Options

| Value | Description |
|-------|-------------|
| `Border3DSide.All` | All four sides (default) |
| `Border3DSide.Top` | Top side only |
| `Border3DSide.Bottom` | Bottom side only |
| `Border3DSide.Left` | Left side only |
| `Border3DSide.Right` | Right side only |

You can combine sides using the bitwise OR operator (`|`).

### All Sides (Default)

**C#:**
```csharp
using System.Windows.Forms;

// Border on all sides
textBoxExt1.BorderStyle = BorderStyle.FixedSingle;
textBoxExt1.BorderSides = Border3DSide.All;
```

**VB.NET:**
```vb
Imports System.Windows.Forms

' Border on all sides
textBoxExt1.BorderStyle = BorderStyle.FixedSingle
textBoxExt1.BorderSides = Border3DSide.All
```

### Single Side Border

**Bottom border only (underline effect):**
```csharp
textBoxExt1.BorderStyle = BorderStyle.FixedSingle;
textBoxExt1.BorderSides = Border3DSide.Bottom;
textBoxExt1.BorderColor = Color.Gray;
```

**Top border only:**
```csharp
textBoxExt1.BorderStyle = BorderStyle.FixedSingle;
textBoxExt1.BorderSides = Border3DSide.Top;
```

**Left border only (accent bar):**
```csharp
textBoxExt1.BorderStyle = BorderStyle.FixedSingle;
textBoxExt1.BorderSides = Border3DSide.Left;
textBoxExt1.BorderColor = Color.Blue;
```

### Combined Sides

**Top and bottom (horizontal lines):**
```csharp
textBoxExt1.BorderStyle = BorderStyle.FixedSingle;
textBoxExt1.BorderSides = Border3DSide.Top | Border3DSide.Bottom;
```

**Left and right (vertical lines):**
```csharp
textBoxExt1.BorderStyle = BorderStyle.FixedSingle;
textBoxExt1.BorderSides = Border3DSide.Left | Border3DSide.Right;
```

**Three sides (no top):**
```csharp
textBoxExt1.BorderStyle = BorderStyle.FixedSingle;
textBoxExt1.BorderSides = Border3DSide.Left | Border3DSide.Right | Border3DSide.Bottom;
```

## Complete Border Configuration Examples

### Example 1: Modern Flat Border with Custom Color

```csharp
using Syncfusion.Windows.Forms.Tools;
using System.Drawing;
using System.Windows.Forms;

TextBoxExt modernBox = new TextBoxExt();
modernBox.Location = new Point(50, 50);
modernBox.Size = new Size(300, 25);
modernBox.Text = "Modern styled textbox";

// Flat border with custom color
modernBox.BorderStyle = BorderStyle.FixedSingle;
modernBox.BorderColor = Color.FromArgb(0, 120, 215); // Windows 10 blue
modernBox.BorderSides = Border3DSide.All;

// Styling
modernBox.BackColor = Color.White;
modernBox.ForeColor = Color.Black;

this.Controls.Add(modernBox);
```

### Example 2: Underline-Style Border (Material Design)

```csharp
TextBoxExt underlineBox = new TextBoxExt();
underlineBox.Location = new Point(50, 100);
underlineBox.Size = new Size(300, 25);
underlineBox.Text = "";

// Only bottom border (underline effect)
underlineBox.BorderStyle = BorderStyle.FixedSingle;
underlineBox.BorderSides = Border3DSide.Bottom;
underlineBox.BorderColor = Color.Gray;

// Remove background border appearance
underlineBox.BackColor = SystemColors.Control;

this.Controls.Add(underlineBox);
```

### Example 3: Validation Border (Error State)

```csharp
TextBoxExt validationBox = new TextBoxExt();
validationBox.Location = new Point(50, 150);
validationBox.Size = new Size(300, 25);

// Validation method
void ValidateInput(string input)
{
    if (string.IsNullOrWhiteSpace(input))
    {
        // Error: Red border
        validationBox.BorderStyle = BorderStyle.FixedSingle;
        validationBox.BorderColor = Color.Red;
        validationBox.BackColor = Color.FromArgb(255, 240, 240); // Light red
    }
    else
    {
        // Success: Green border
        validationBox.BorderStyle = BorderStyle.FixedSingle;
        validationBox.BorderColor = Color.Green;
        validationBox.BackColor = Color.White;
    }
}

validationBox.TextChanged += (s, e) => ValidateInput(validationBox.Text);

this.Controls.Add(validationBox);
```

### Example 4: Raised 3D Border with Color Accent

```csharp
TextBoxExt raised3DBox = new TextBoxExt();
raised3DBox.Location = new Point(50, 200);
raised3DBox.Size = new Size(300, 25);
raised3DBox.Text = "3D Raised textbox";

// Raised 3D border
raised3DBox.BorderStyle = BorderStyle.Fixed3D;
raised3DBox.Border3DStyle = Border3DStyle.Raised;
raised3DBox.BorderSides = Border3DSide.All;

// Note: BorderColor has limited effect with Fixed3D
raised3DBox.BackColor = Color.WhiteSmoke;

this.Controls.Add(raised3DBox);
```

### Example 5: Accent Bar on Left Side

```csharp
TextBoxExt accentBox = new TextBoxExt();
accentBox.Location = new Point(50, 250);
accentBox.Size = new Size(300, 25);
accentBox.Text = "Important field";

// Left accent bar
accentBox.BorderStyle = BorderStyle.FixedSingle;
accentBox.BorderSides = Border3DSide.Left;
accentBox.BorderColor = Color.OrangeRed;

// Full box border (separate control recommended in production)
// For complete look, you might wrap in a panel with full border

this.Controls.Add(accentBox);
```

### Example 6: Focus State Border Change

```csharp
TextBoxExt focusBox = new TextBoxExt();
focusBox.Location = new Point(50, 300);
focusBox.Size = new Size(300, 25);

// Default state
focusBox.BorderStyle = BorderStyle.FixedSingle;
focusBox.BorderColor = Color.LightGray;

// Focus state: Change to blue
focusBox.Enter += (s, e) => {
    focusBox.BorderColor = Color.FromArgb(0, 120, 215);
};

// Lost focus: Revert to gray
focusBox.Leave += (s, e) => {
    focusBox.BorderColor = Color.LightGray;
};

this.Controls.Add(focusBox);
```

## Combining Border Properties

Here's a comprehensive example combining all border properties:

```csharp
using Syncfusion.Windows.Forms.Tools;
using System.Drawing;
using System.Windows.Forms;

public void CreateCustomBorderedTextBox()
{
    TextBoxExt textBoxExt1 = new TextBoxExt();
    
    // Position and size
    textBoxExt1.Location = new Point(100, 100);
    textBoxExt1.Size = new Size(300, 25);
    textBoxExt1.Text = "Custom bordered textbox";
    
    // Border configuration
    textBoxExt1.BorderStyle = BorderStyle.FixedSingle;
    textBoxExt1.BorderColor = Color.Orchid;
    textBoxExt1.BorderSides = Border3DSide.All;
    
    // For 3D effect (alternative):
    // textBoxExt1.BorderStyle = BorderStyle.Fixed3D;
    // textBoxExt1.Border3DStyle = Border3DStyle.Raised;
    
    // Additional styling
    textBoxExt1.BackColor = Color.White;
    textBoxExt1.ForeColor = Color.Black;
    
    // Add to form
    this.Controls.Add(textBoxExt1);
}
```

## Best Practices

### Consistency

Use consistent border styles across your application:
```csharp
// Define border style constants
public class AppStyles
{
    public static void ApplyStandardBorder(TextBoxExt textBox)
    {
        textBox.BorderStyle = BorderStyle.FixedSingle;
        textBox.BorderColor = Color.FromArgb(171, 173, 179); // Gray
        textBox.BorderSides = Border3DSide.All;
    }
    
    public static void ApplyFocusBorder(TextBoxExt textBox)
    {
        textBox.BorderColor = Color.FromArgb(0, 120, 215); // Blue
    }
}
```

### Validation Feedback

Use border colors to indicate validation state:
```csharp
public enum ValidationState
{
    Normal,
    Valid,
    Invalid,
    Warning
}

public void SetValidationBorder(TextBoxExt textBox, ValidationState state)
{
    textBox.BorderStyle = BorderStyle.FixedSingle;
    
    switch (state)
    {
        case ValidationState.Normal:
            textBox.BorderColor = Color.LightGray;
            break;
        case ValidationState.Valid:
            textBox.BorderColor = Color.Green;
            break;
        case ValidationState.Invalid:
            textBox.BorderColor = Color.Red;
            break;
        case ValidationState.Warning:
            textBox.BorderColor = Color.Orange;
            break;
    }
}
```

### Performance

Avoid excessive border property changes in loops. Instead, set once:
```csharp
// Good: Set once
foreach (var textBox in textBoxes)
{
    textBox.BorderStyle = BorderStyle.FixedSingle;
    textBox.BorderColor = Color.Blue;
}

// Avoid: Repeated property access in tight loops
```

## Summary

Border customization in TextBoxExt provides:

- **BorderStyle** for overall border type (None, FixedSingle, Fixed3D)
- **Border3DStyle** for 3D visual effects (Raised, Sunken, Etched, etc.)
- **BorderColor** for custom colors (most effective with FixedSingle)
- **BorderSides** for selective border display (All, Top, Bottom, Left, Right, combinations)

These properties enable you to create visually distinctive textboxes that match your application's design language and provide clear visual feedback for user interactions.
