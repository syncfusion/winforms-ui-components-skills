# Appearance Customization

## Table of Contents
- [Overview](#overview)
- [Border Styles](#border-styles)
- [FlatStyle Property](#flatstyle-property)
- [Border Customization](#border-customization)
- [Edit Portion Appearance](#edit-portion-appearance)
- [Dropdown Button](#dropdown-button)
- [Complete Styling Examples](#complete-styling-examples)

## Overview

ComboDropDown provides extensive appearance customization through border styles, flat style modes, and color properties. These settings control the visual presentation independent of theme settings (covered in themes-and-styles.md).

**Key Properties:**
- **Border3DStyle** - 3D border appearance (10 styles available)
- **BorderSides** - Which sides of the control have borders
- **FlatStyle** - Overall control appearance (Flat, System, Standard)
- **FlatBorderColor** - Border color when using Flat style
- **Font, ForeColor, BackColor** - Text box appearance

**Important:** To use Border3DStyle settings, FlatStyle must be set to `Standard`.

## Border Styles

### Border3DStyle Property

Sets the 3D border style for the control. This property provides 10 different border appearances.

**Type:** `System.Windows.Forms.Border3DStyle`

**Prerequisite:** `FlatStyle = FlatStyle.Standard`

### Available Border Styles

| Value | Description | Visual Effect |
|-------|-------------|---------------|
| `RaisedOuter` | Outer edge raised | Outer border appears elevated |
| `RaisedInner` | Inner edge raised | Inner border appears elevated |
| `SunkenOuter` | Outer edge sunken | Outer border appears depressed |
| `SunkenInner` | Inner edge sunken | Inner border appears depressed |
| `Raised` | Raised on both edges | Full raised appearance (default) |
| `Sunken` | Sunken on both edges | Full sunken/inset appearance |
| `Etched` | Etched into surface | Engraved look |
| `Flat` | Flat border | No 3D effect |
| `Adjust` | Adjust border | System-adjusted border |
| `Bump` | Bumped appearance | Protruding look |

### Example

```csharp
// Set to Standard mode first (required)
this.comboDropDown1.FlatStyle = FlatStyle.Standard;

// Apply 3D border style
this.comboDropDown1.Border3DStyle = Border3DStyle.RaisedInner;
```

```vb
' Set to Standard mode first (required)
Me.comboDropDown1.FlatStyle = FlatStyle.Standard

' Apply 3D border style
Me.comboDropDown1.Border3DStyle = Border3DStyle.RaisedInner
```

### BorderSides Property

Specifies which sides of the control should have borders.

**Type:** `System.Windows.Forms.Border3DSide` (flags enumeration)

### Available Border Sides

| Value | Description |
|-------|-------------|
| `Left` | Left side only |
| `Right` | Right side only |
| `Top` | Top side only |
| `Bottom` | Bottom side only |
| `All` | All four sides (default) |

### Example

```csharp
// Border on all sides (default)
this.comboDropDown1.BorderSides = Border3DSide.All;

// Border on left and right only
this.comboDropDown1.BorderSides = Border3DSide.Left | Border3DSide.Right;

// Border on top only
this.comboDropDown1.BorderSides = Border3DSide.Top;
```

```vb
' Border on all sides (default)
Me.comboDropDown1.BorderSides = Border3DSide.All

' Border on left and right only
Me.comboDropDown1.BorderSides = Border3DSide.Left Or Border3DSide.Right

' Border on top only
Me.comboDropDown1.BorderSides = Border3DSide.Top
```

### Complete Border Example

```csharp
// Standard 3D appearance with raised inner border
this.comboDropDown1.FlatStyle = FlatStyle.Standard;
this.comboDropDown1.Border3DStyle = Border3DStyle.RaisedInner;
this.comboDropDown1.BorderSides = Border3DSide.All;
```

```vb
' Standard 3D appearance with raised inner border
Me.comboDropDown1.FlatStyle = FlatStyle.Standard
Me.comboDropDown1.Border3DStyle = Border3DStyle.RaisedInner
Me.comboDropDown1.BorderSides = Border3DSide.All
```

## FlatStyle Property

Controls the overall appearance mode of the ComboDropDown control and its dropdown button.

**Type:** `System.Windows.Forms.FlatStyle`

### Available Flat Styles

| Value | Description | Button Appearance | When to Use |
|-------|-------------|-------------------|-------------|
| `Flat` | Flat appearance | Button appears flat, no 3D effect | Modern, minimalist UI |
| `System` | System-defined | OS default appearance | Consistent with OS theme |
| `Standard` | Standard 3D | Button appears three-dimensional | Classic Windows look, required for Border3DStyle |

### Example

```csharp
// Flat appearance (modern)
this.comboDropDown1.FlatStyle = FlatStyle.Flat;

// System appearance (OS default)
this.comboDropDown1.FlatStyle = FlatStyle.System;

// Standard 3D appearance (classic)
this.comboDropDown1.FlatStyle = FlatStyle.Standard;
```

```vb
' Flat appearance (modern)
Me.comboDropDown1.FlatStyle = FlatStyle.Flat

' System appearance (OS default)
Me.comboDropDown1.FlatStyle = FlatStyle.System

' Standard 3D appearance (classic)
Me.comboDropDown1.FlatStyle = FlatStyle.Standard
```

### Visual Comparison

**Flat:**
- No 3D borders or shadows
- Clean, modern look
- Dropdown button is flat
- Border color controlled by FlatBorderColor

**System:**
- Appearance determined by Windows theme
- Varies by OS version and user settings
- Consistent with other system controls

**Standard:**
- Classic Windows Forms 3D appearance
- Raised/sunken effects
- Required for Border3DStyle customization
- Dropdown button has 3D appearance

## Border Customization

### FlatBorderColor Property

Specifies the border color when `FlatStyle` is set to `Flat`.

**Type:** `System.Drawing.Color`

**Applies to:** `FlatStyle.Flat` only

### Example

```csharp
// Flat style with custom border color
this.comboDropDown1.FlatStyle = FlatStyle.Flat;
this.comboDropDown1.FlatBorderColor = Color.Gray;

// Flat style with accent color border
this.comboDropDown1.FlatStyle = FlatStyle.Flat;
this.comboDropDown1.FlatBorderColor = Color.DodgerBlue;
```

```vb
' Flat style with custom border color
Me.comboDropDown1.FlatStyle = FlatStyle.Flat
Me.comboDropDown1.FlatBorderColor = Color.Gray

' Flat style with accent color border
Me.comboDropDown1.FlatStyle = FlatStyle.Flat
Me.comboDropDown1.FlatBorderColor = Color.DodgerBlue
```

### Custom Border Color Example

```csharp
// Modern flat appearance with custom border
this.comboDropDown1.FlatStyle = FlatStyle.Flat;
this.comboDropDown1.FlatBorderColor = Color.FromArgb(200, 200, 200); // Light gray
this.comboDropDown1.BackColor = Color.White;
this.comboDropDown1.ForeColor = Color.Black;
```

## Edit Portion Appearance

Customize the text box portion's visual appearance using standard control properties.

### Font Property

```csharp
// Custom font
this.comboDropDown1.Font = new Font("Segoe UI", 10F, FontStyle.Regular);

// Bold font
this.comboDropDown1.Font = new Font("Arial", 9F, FontStyle.Bold);
```

### ForeColor Property

```csharp
// Text color
this.comboDropDown1.ForeColor = Color.Black;

// Custom text color
this.comboDropDown1.ForeColor = Color.DarkBlue;
```

### BackColor Property

```csharp
// Background color
this.comboDropDown1.BackColor = Color.White;

// Light background
this.comboDropDown1.BackColor = Color.FromArgb(245, 245, 245);
```

### Complete Edit Portion Example

```csharp
// Professional text box appearance
this.comboDropDown1.Font = new Font("Segoe UI", 10F);
this.comboDropDown1.ForeColor = Color.FromArgb(60, 60, 60); // Dark gray text
this.comboDropDown1.BackColor = Color.White;
this.comboDropDown1.FlatStyle = FlatStyle.Flat;
this.comboDropDown1.FlatBorderColor = Color.FromArgb(200, 200, 200);
```

```vb
' Professional text box appearance
Me.comboDropDown1.Font = New Font("Segoe UI", 10F)
Me.comboDropDown1.ForeColor = Color.FromArgb(60, 60, 60) ' Dark gray text
Me.comboDropDown1.BackColor = Color.White
Me.comboDropDown1.FlatStyle = FlatStyle.Flat
Me.comboDropDown1.FlatBorderColor = Color.FromArgb(200, 200, 200)
```

## Dropdown Button

The dropdown button appearance is controlled by the `FlatStyle` property:

### Button Appearance by FlatStyle

**Flat:**
```csharp
comboDropDown1.FlatStyle = FlatStyle.Flat;
// Button: Flat with no 3D effect, minimal visual separation
```

**Standard:**
```csharp
comboDropDown1.FlatStyle = FlatStyle.Standard;
// Button: 3D raised button, classic Windows appearance
```

**System:**
```csharp
comboDropDown1.FlatStyle = FlatStyle.System;
// Button: OS-themed button, varies by Windows version
```

The dropdown button inherits colors from the control but cannot be customized independently beyond the FlatStyle setting.

## Complete Styling Examples

### Example 1: Modern Flat Design

```csharp
// Clean, modern flat appearance
this.comboDropDown1.FlatStyle = FlatStyle.Flat;
this.comboDropDown1.FlatBorderColor = Color.FromArgb(220, 220, 220);
this.comboDropDown1.BackColor = Color.White;
this.comboDropDown1.ForeColor = Color.FromArgb(50, 50, 50);
this.comboDropDown1.Font = new Font("Segoe UI", 10F);
```

### Example 2: Classic 3D Appearance

```csharp
// Traditional Windows Forms 3D look
this.comboDropDown1.FlatStyle = FlatStyle.Standard;
this.comboDropDown1.Border3DStyle = Border3DStyle.Sunken;
this.comboDropDown1.BorderSides = Border3DSide.All;
this.comboDropDown1.BackColor = SystemColors.Window;
this.comboDropDown1.ForeColor = SystemColors.WindowText;
```

### Example 3: Accented Border

```csharp
// Flat with accent color border
this.comboDropDown1.FlatStyle = FlatStyle.Flat;
this.comboDropDown1.FlatBorderColor = Color.DodgerBlue;
this.comboDropDown1.BackColor = Color.White;
this.comboDropDown1.ForeColor = Color.Black;
this.comboDropDown1.Font = new Font("Segoe UI", 9.75F);
```

### Example 4: High Contrast

```csharp
// High contrast for accessibility
this.comboDropDown1.FlatStyle = FlatStyle.Flat;
this.comboDropDown1.FlatBorderColor = Color.Black;
this.comboDropDown1.BackColor = Color.White;
this.comboDropDown1.ForeColor = Color.Black;
this.comboDropDown1.Font = new Font("Arial", 11F, FontStyle.Bold);
```

### Example 5: Dark Theme

```csharp
// Dark theme appearance
this.comboDropDown1.FlatStyle = FlatStyle.Flat;
this.comboDropDown1.FlatBorderColor = Color.FromArgb(80, 80, 80);
this.comboDropDown1.BackColor = Color.FromArgb(45, 45, 45);
this.comboDropDown1.ForeColor = Color.White;
this.comboDropDown1.Font = new Font("Consolas", 9.75F);
```

## Style Property Dependency

**Important:** ComboDropDown also has a `Style` property (covered in themes-and-styles.md) that applies visual themes like Office2016, Metro, etc.

**Interaction with Appearance Properties:**

When `Style` is set to `Default`:
- Border3DStyle, FlatStyle, and FlatBorderColor take full effect
- Complete control over custom appearance

When `Style` is set to a theme (Office2016Colorful, Metro, etc.):
- Theme appearance overrides some custom settings
- FlatStyle and Border3DStyle may be ignored
- Theme colors take precedence

**Best Practice:** Use either custom appearance OR theme styling, not both simultaneously.

```csharp
// Option 1: Custom appearance (set Style to Default)
this.comboDropDown1.Style = Syncfusion.Windows.Forms.VisualStyle.Default;
this.comboDropDown1.FlatStyle = FlatStyle.Flat;
this.comboDropDown1.FlatBorderColor = Color.Gray;

// Option 2: Theme appearance (custom settings ignored)
this.comboDropDown1.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016Colorful;
// FlatStyle and Border3DStyle settings won't apply
```

## Best Practices

1. **Set Style to Default for custom appearance** - Avoid conflicts between themes and custom styling
2. **Use Flat for modern UI** - Flat style fits contemporary design trends
3. **Standard for Border3DStyle** - Only use Standard FlatStyle when customizing 3D borders
4. **Consistent colors** - Match BackColor, ForeColor with your application's color scheme
5. **Test with different DPI settings** - Ensure borders and fonts scale properly
6. **Accessibility** - Maintain sufficient contrast between text and background colors
