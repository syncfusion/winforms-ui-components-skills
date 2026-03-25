# Appearance Customization for NumericUpDownExt

Comprehensive guide to customizing the visual appearance of the NumericUpDownExt control.

## Table of Contents

- [Overview](#overview)
- [BackColor Property](#backcolor-property)
- [ForeColor Property](#forecolor-property)
- [NegativeForeColor Property](#negativeforecolor-property)
- [BorderStyle Property](#borderstyle-property)
- [Border3DStyle Property](#border3dstyle-property)
- [BorderColor Property](#bordercolor-property)
- [BorderSides Property](#bordersides-property)
- [ThemedBorder Property](#themedborder-property)
- [TextAlign Property](#textalign-property)
- [UpDownAlign Property](#updownalign-property)
- [MaximumSize and MinimumSize Properties](#maximumsize-and-minimumsize-properties)
- [Complete Custom Styling Examples](#complete-custom-styling-examples)
- [Themed Appearance Patterns](#themed-appearance-patterns)

## Overview

Appearance customization allows you to style the NumericUpDownExt control to match your application's design. You can customize colors, borders, alignment, and layout to create professional-looking interfaces.

## BackColor Property

The `BackColor` property sets the background color of the control.

**Type:** `System.Drawing.Color`  
**Default:** `System.Drawing.SystemColors.Window`

### When to Use
- Matching application color schemes
- Highlighting required fields
- Indicating disabled or read-only states
- Creating visual distinction between control types

### Basic BackColor Usage

```csharp
using System.Drawing;
using Syncfusion.Windows.Forms.Tools;

// Set background color
numericUpDownExt1.BackColor = Color.Aquamarine;
```

**Result:** Control displays with an aquamarine background.

### Common Color Examples

```csharp
// Light yellow for emphasis
numericUpDownExt1.BackColor = Color.LightYellow;

// Light gray for read-only appearance
numericUpDownExt1.BackColor = Color.LightGray;
numericUpDownExt1.ReadOnly = true;

// White for standard input
numericUpDownExt1.BackColor = Color.White;

// Subtle color for required fields
numericUpDownExt1.BackColor = Color.FromArgb(255, 255, 240); // Light cream
```

**Result:** Different background colors for different control states.

### Dynamic BackColor Based on Value

```csharp
private void SetupDynamicBackground()
{
    NumericUpDownExt temperatureControl = new NumericUpDownExt();
    temperatureControl.Minimum = new decimal(-50);
    temperatureControl.Maximum = new decimal(120);
    temperatureControl.Value = new decimal(72);
    temperatureControl.Location = new Point(50, 50);
    
    // Change background based on value
    temperatureControl.ValueChanged += (s, e) =>
    {
        decimal temp = temperatureControl.Value;
        
        if (temp < 32)
        {
            temperatureControl.BackColor = Color.LightBlue; // Cold
        }
        else if (temp > 90)
        {
            temperatureControl.BackColor = Color.LightCoral; // Hot
        }
        else
        {
            temperatureControl.BackColor = Color.LightGreen; // Comfortable
        }
    };
    
    this.Controls.Add(temperatureControl);
}
```

**Result:** Background color changes dynamically based on temperature value.

### Validation Indicator Example

```csharp
private void SetupValidationColors()
{
    NumericUpDownExt quantityInput = new NumericUpDownExt();
    quantityInput.Minimum = new decimal(1);
    quantityInput.Maximum = new decimal(100);
    quantityInput.Value = new decimal(1);
    quantityInput.Location = new Point(50, 50);
    
    int availableStock = 50;
    
    quantityInput.ValueChanged += (s, e) =>
    {
        if (quantityInput.Value > availableStock)
        {
            // Invalid - exceeds stock
            quantityInput.BackColor = Color.LightPink;
        }
        else if (quantityInput.Value > availableStock * 0.8M)
        {
            // Warning - approaching limit
            quantityInput.BackColor = Color.LightYellow;
        }
        else
        {
            // Valid
            quantityInput.BackColor = Color.White;
        }
    };
    
    this.Controls.Add(quantityInput);
}
```

**Result:** Visual feedback showing stock availability status.

## ForeColor Property

The `ForeColor` property sets the text color of the control.

**Type:** `System.Drawing.Color`  
**Default:** `System.Drawing.SystemColors.WindowText`

### When to Use
- Enhancing readability with high contrast
- Matching brand colors
- Indicating control states
- Creating themed interfaces

### Basic ForeColor Usage

```csharp
// Set text color
numericUpDownExt1.ForeColor = Color.DodgerBlue;
```

**Result:** Text displays in dodger blue color.

### High Contrast Example

```csharp
// High contrast for visibility
numericUpDownExt1.BackColor = Color.Black;
numericUpDownExt1.ForeColor = Color.White;
```

**Result:** White text on black background for maximum contrast.

### Custom Theme Colors

```csharp
private void ApplyCustomTheme()
{
    NumericUpDownExt themedControl = new NumericUpDownExt();
    
    // Dark theme
    themedControl.BackColor = Color.FromArgb(45, 45, 48);
    themedControl.ForeColor = Color.FromArgb(241, 241, 241);
    themedControl.BorderStyle = BorderStyle.FixedSingle;
    themedControl.BorderColor = Color.FromArgb(63, 63, 70);
    
    themedControl.Location = new Point(50, 50);
    themedControl.Size = new Size(120, 24);
    
    this.Controls.Add(themedControl);
}
```

**Result:** Professional dark theme appearance.

## NegativeForeColor Property

Custom property for displaying negative values in a different color. This requires deriving the control.

### When to Use
- Financial applications showing losses
- Temperature below zero
- Debt or deficit indicators
- Any scenario where negative values need emphasis

### Implementing NegativeForeColor

```csharp
// Custom derived class with negative color support
public class NumericUpDownExtDerived : NumericUpDownExt
{
    private IntegerTextBox itb = new IntegerTextBox();
    
    public NumericUpDownExtDerived()
    {
        // Find the internal TextBox and replace with IntegerTextBox
        foreach (Control c in Controls)
        {
            if (c is TextBox)
            {
                itb.Location = c.Location;
                itb.Size = c.Size;
                itb.Dock = c.Dock;
                itb.Anchor = c.Anchor;
                Controls.Add(itb);
                itb.BringToFront();
                itb.TextChanged += (s, e) => Value = itb.IntegerValue;
            }
        }
    }
    
    public Color NegativeColor
    {
        get { return itb.NegativeColor; }
        set { itb.NegativeColor = value; }
    }
    
    protected override void OnValueChanged(EventArgs e)
    {
        itb.Text = Value.ToString();
        base.OnValueChanged(e);
    }
}
```

### Using NegativeForeColor

```csharp
private void SetupNegativeColorDisplay()
{
    NumericUpDownExtDerived accountBalance = new NumericUpDownExtDerived();
    
    accountBalance.Location = new Point(100, 50);
    accountBalance.Size = new Size(150, 24);
    accountBalance.Minimum = new decimal(-10000);
    accountBalance.Maximum = new decimal(10000);
    accountBalance.DecimalPlaces = 2;
    accountBalance.Value = new decimal(-250.50M);
    
    // Set negative color to red
    accountBalance.NegativeColor = Color.Red;
    accountBalance.ForeColor = Color.Green; // Positive values in green
    
    this.Controls.Add(accountBalance);
}
```

**Result:** Negative values display in red, positive in green.

## BorderStyle Property

The `BorderStyle` property sets the border appearance of the control.

**Type:** `System.Windows.Forms.BorderStyle`  
**Default:** `BorderStyle.Fixed3D`  
**Options:** `FixedSingle`, `Fixed3D`, `None`

### When to Use
- Creating flat, modern designs (FixedSingle)
- Classic 3D appearance (Fixed3D)
- Borderless integration (None)
- Matching form design patterns

### BorderStyle Options

```csharp
// Fixed Single - flat 1-pixel border
numericUpDownExt1.BorderStyle = BorderStyle.FixedSingle;

// Fixed 3D - raised 3D border (default)
numericUpDownExt2.BorderStyle = BorderStyle.Fixed3D;

// None - no border
numericUpDownExt3.BorderStyle = BorderStyle.None;
```

**Result:** Three different border styles shown.

### Modern Flat Design

```csharp
private void CreateFlatDesign()
{
    NumericUpDownExt flatControl = new NumericUpDownExt();
    
    // Flat modern appearance
    flatControl.BorderStyle = BorderStyle.FixedSingle;
    flatControl.BackColor = Color.White;
    flatControl.ForeColor = Color.FromArgb(64, 64, 64);
    flatControl.Location = new Point(50, 50);
    flatControl.Size = new Size(120, 24);
    
    this.Controls.Add(flatControl);
}
```

**Result:** Clean, flat modern appearance.

### Borderless Integration

```csharp
// Seamless integration with panel
Panel panel = new Panel();
panel.BackColor = Color.LightGray;
panel.Location = new Point(50, 50);
panel.Size = new Size(200, 100);

NumericUpDownExt borderlessControl = new NumericUpDownExt();
borderlessControl.BorderStyle = BorderStyle.None;
borderlessControl.BackColor = Color.LightGray;
borderlessControl.Location = new Point(10, 10);

panel.Controls.Add(borderlessControl);
this.Controls.Add(panel);
```

**Result:** Control blends seamlessly with panel background.

## Border3DStyle Property

The `Border3DStyle` property sets the 3D border style when BorderStyle is Fixed3D.

**Type:** `System.Windows.Forms.Border3DStyle`  
**Default:** `Border3DStyle.Sunken`

### Available Styles

```csharp
// Various 3D border styles
numericUpDownExt1.BorderStyle = BorderStyle.Fixed3D;

// Sunken (default) - appears recessed
numericUpDownExt1.Border3DStyle = Border3DStyle.Sunken;

// Raised - appears elevated
numericUpDownExt1.Border3DStyle = Border3DStyle.Raised;

// Etched - etched appearance
numericUpDownExt1.Border3DStyle = Border3DStyle.Etched;

// Bump - bumped appearance
numericUpDownExt1.Border3DStyle = Border3DStyle.Bump;
```

**Result:** Different 3D border effects.

### Complete 3D Border Example

```csharp
private void Setup3DBorder()
{
    NumericUpDownExt control3D = new NumericUpDownExt();
    
    control3D.BorderStyle = BorderStyle.Fixed3D;
    control3D.Border3DStyle = Border3DStyle.Etched;
    control3D.BorderSides = Border3DSide.All;
    control3D.Location = new Point(50, 50);
    control3D.Size = new Size(120, 24);
    
    this.Controls.Add(control3D);
}
```

**Result:** Control with etched 3D border on all sides.

## BorderColor Property

The `BorderColor` property sets the color of a 2D border (when using FixedSingle style).

**Type:** `System.Drawing.Color`

### When to Use
- Custom border colors for branding
- Visual state indicators
- Highlighting focused controls
- Theme color coordination

### Basic BorderColor Usage

```csharp
// Set custom border color
numericUpDownExt1.BorderStyle = BorderStyle.FixedSingle;
numericUpDownExt1.BorderColor = Color.Crimson;
```

**Result:** Control with crimson border.

### Focus Indicator Example

```csharp
private void SetupFocusIndicator()
{
    NumericUpDownExt focusControl = new NumericUpDownExt();
    
    focusControl.BorderStyle = BorderStyle.FixedSingle;
    focusControl.BorderColor = Color.LightGray;
    focusControl.Location = new Point(50, 50);
    
    // Change border color on focus
    focusControl.Enter += (s, e) =>
    {
        focusControl.BorderColor = Color.DodgerBlue;
    };
    
    focusControl.Leave += (s, e) =>
    {
        focusControl.BorderColor = Color.LightGray;
    };
    
    this.Controls.Add(focusControl);
}
```

**Result:** Border changes to blue when focused.

### Validation Border Colors

```csharp
private void SetupValidationBorders()
{
    NumericUpDownExt validatedControl = new NumericUpDownExt();
    
    validatedControl.BorderStyle = BorderStyle.FixedSingle;
    validatedControl.Minimum = new decimal(1);
    validatedControl.Maximum = new decimal(100);
    validatedControl.Value = new decimal(1);
    validatedControl.Location = new Point(50, 50);
    
    validatedControl.ValueChanged += (s, e) =>
    {
        if (validatedControl.Value < 10)
        {
            validatedControl.BorderColor = Color.Red; // Too low
        }
        else if (validatedControl.Value > 90)
        {
            validatedControl.BorderColor = Color.Orange; // Too high
        }
        else
        {
            validatedControl.BorderColor = Color.Green; // Valid range
        }
    };
    
    this.Controls.Add(validatedControl);
}
```

**Result:** Border color indicates value validity.

## BorderSides Property

The `BorderSides` property specifies which sides of the control have borders.

**Type:** `System.Windows.Forms.Border3DSide`  
**Options:** `Left`, `Top`, `Right`, `Bottom`, `Middle`, `All`

### Partial Border Example

```csharp
// Bottom border only
numericUpDownExt1.BorderStyle = BorderStyle.FixedSingle;
numericUpDownExt1.BorderSides = Border3DSide.Bottom;
numericUpDownExt1.BorderColor = Color.DodgerBlue;
```

**Result:** Only bottom border visible.

### Custom Border Patterns

```csharp
// Left and right borders only
numericUpDownExt1.BorderStyle = BorderStyle.Fixed3D;
numericUpDownExt1.BorderSides = Border3DSide.Left | Border3DSide.Right;
```

**Result:** Borders on left and right sides only.

## ThemedBorder Property

The `ThemedBorder` property applies themed borders that match Windows visual styles.

**Type:** `bool`  
**Default:** `false`

### When to Use
- Matching Windows XP/Vista/7 themes
- Professional, native appearance
- When ThemesEnabled is true

### Basic Themed Border

```csharp
// Enable themed border
numericUpDownExt1.ThemesEnabled = true;
numericUpDownExt1.ThemedBorder = true;
```

**Result:** Border matches current Windows theme.

### Complete Themed Example

```csharp
private void SetupThemedControl()
{
    NumericUpDownExt themedControl = new NumericUpDownExt();
    
    // Enable theming
    themedControl.ThemesEnabled = true;
    themedControl.ThemedBorder = true;
    themedControl.Location = new Point(50, 50);
    themedControl.Size = new Size(120, 24);
    
    this.Controls.Add(themedControl);
}
```

**Result:** Professional themed appearance matching OS.

## TextAlign Property

The `TextAlign` property sets the horizontal alignment of text within the control.

**Type:** `System.Windows.Forms.HorizontalAlignment`  
**Default:** `HorizontalAlignment.Left`  
**Options:** `Left`, `Right`, `Center`

### When to Use
- Right-align for currency/numbers (accounting style)
- Center-align for displayed values
- Left-align for standard entry

### TextAlign Examples

```csharp
// Left alignment (default)
numericUpDownExt1.TextAlign = HorizontalAlignment.Left;

// Right alignment (common for numbers)
numericUpDownExt2.TextAlign = HorizontalAlignment.Right;

// Center alignment
numericUpDownExt3.TextAlign = HorizontalAlignment.Center;
```

**Result:** Text aligned differently in each control.

### Accounting Style Example

```csharp
private void SetupAccountingStyle()
{
    NumericUpDownExt accountingControl = new NumericUpDownExt();
    
    // Accounting/financial style - right aligned
    accountingControl.TextAlign = HorizontalAlignment.Right;
    accountingControl.DecimalPlaces = 2;
    accountingControl.ThousandsSeparator = true;
    accountingControl.Value = new decimal(12345.67M);
    accountingControl.Location = new Point(50, 50);
    accountingControl.Size = new Size(150, 24);
    
    this.Controls.Add(accountingControl);
}
```

**Result:** Professional accounting-style number display.

## UpDownAlign Property

The `UpDownAlign` property sets the position of the up/down buttons.

**Type:** `System.Windows.Forms.LeftRightAlignment`  
**Default:** `LeftRightAlignment.Right`  
**Options:** `Left`, `Right`

### Basic UpDownAlign Usage

```csharp
// Buttons on left side
numericUpDownExt1.UpDownAlign = LeftRightAlignment.Left;

// Buttons on right side (default)
numericUpDownExt2.UpDownAlign = LeftRightAlignment.Right;
```

**Result:** Buttons positioned on specified side.

### Left-Aligned Buttons Example

```csharp
private void SetupLeftAlignedButtons()
{
    NumericUpDownExt leftButtonControl = new NumericUpDownExt();
    
    leftButtonControl.UpDownAlign = LeftRightAlignment.Left;
    leftButtonControl.TextAlign = HorizontalAlignment.Right;
    leftButtonControl.Location = new Point(50, 50);
    leftButtonControl.Size = new Size(120, 24);
    
    this.Controls.Add(leftButtonControl);
}
```

**Result:** Buttons on left, text right-aligned.

## MaximumSize and MinimumSize Properties

Control the size constraints of the control.

**Type:** `System.Drawing.Size`

### When to Use
- Enforcing consistent control sizes
- Preventing excessive resizing
- Responsive layouts with constraints

### Size Constraints Example

```csharp
// Set size constraints
numericUpDownExt1.MinimumSize = new Size(80, 24);
numericUpDownExt1.MaximumSize = new Size(200, 24);
numericUpDownExt1.Size = new Size(120, 24);
```

**Result:** Control size constrained to specified range.

### Fixed Size Example

```csharp
// Fixed size control
NumericUpDownExt fixedControl = new NumericUpDownExt();
fixedControl.MinimumSize = new Size(100, 24);
fixedControl.MaximumSize = new Size(100, 24);
fixedControl.Location = new Point(50, 50);
```

**Result:** Control cannot be resized.

## Complete Custom Styling Examples

### Professional Price Input

```csharp
private void CreateProfessionalPriceInput()
{
    NumericUpDownExt priceInput = new NumericUpDownExt();
    
    // Appearance
    priceInput.BorderStyle = BorderStyle.FixedSingle;
    priceInput.BorderColor = Color.FromArgb(171, 173, 179);
    priceInput.BackColor = Color.White;
    priceInput.ForeColor = Color.FromArgb(64, 64, 64);
    priceInput.TextAlign = HorizontalAlignment.Right;
    
    // Size
    priceInput.Size = new Size(150, 26);
    priceInput.Location = new Point(100, 50);
    
    // Value settings
    priceInput.DecimalPlaces = 2;
    priceInput.ThousandsSeparator = true;
    priceInput.Minimum = new decimal(0);
    priceInput.Maximum = new decimal(999999.99M);
    priceInput.Value = new decimal(99.99M);
    
    Label lblPrice = new Label();
    lblPrice.Text = "Price ($):";
    lblPrice.Location = new Point(30, 53);
    lblPrice.AutoSize = true;
    
    this.Controls.Add(lblPrice);
    this.Controls.Add(priceInput);
}
```

**Result:** Professional, modern price input control.

### Dark Theme Example

```csharp
private void CreateDarkThemeControl()
{
    NumericUpDownExt darkControl = new NumericUpDownExt();
    
    // Dark theme colors
    darkControl.BackColor = Color.FromArgb(30, 30, 30);
    darkControl.ForeColor = Color.FromArgb(220, 220, 220);
    darkControl.BorderStyle = BorderStyle.FixedSingle;
    darkControl.BorderColor = Color.FromArgb(60, 60, 60);
    darkControl.TextAlign = HorizontalAlignment.Center;
    
    darkControl.Location = new Point(50, 50);
    darkControl.Size = new Size(120, 26);
    darkControl.Value = new decimal(50);
    
    this.BackColor = Color.FromArgb(45, 45, 48);
    this.Controls.Add(darkControl);
}
```

**Result:** Complete dark theme appearance.

## Themed Appearance Patterns

### Office 2007 Style

```csharp
private void ApplyOffice2007Style()
{
    NumericUpDownExt office2007Control = new NumericUpDownExt();
    
    office2007Control.VisualStyle = Syncfusion.Windows.Forms.VisualStyle.Office2007;
    office2007Control.ColorScheme = Syncfusion.Windows.Forms.Office2007Theme.Blue;
    office2007Control.Location = new Point(50, 50);
    office2007Control.Size = new Size(120, 24);
    
    this.Controls.Add(office2007Control);
}
```

**Result:** Office 2007 Blue theme applied.

### Complete Appearance Configuration

```csharp
private void SetupCompleteAppearance()
{
    NumericUpDownExt completeControl = new NumericUpDownExt();
    
    // Border
    completeControl.BorderStyle = BorderStyle.FixedSingle;
    completeControl.BorderColor = Color.SteelBlue;
    
    // Colors
    completeControl.BackColor = Color.AliceBlue;
    completeControl.ForeColor = Color.DarkBlue;
    
    // Alignment
    completeControl.TextAlign = HorizontalAlignment.Right;
    completeControl.UpDownAlign = LeftRightAlignment.Right;
    
    // Size
    completeControl.Size = new Size(150, 26);
    completeControl.MinimumSize = new Size(100, 26);
    completeControl.MaximumSize = new Size(200, 26);
    completeControl.Location = new Point(50, 50);
    
    // Value
    completeControl.Value = new decimal(100);
    completeControl.DecimalPlaces = 2;
    completeControl.ThousandsSeparator = true;
    
    this.Controls.Add(completeControl);
}
```

**Result:** Fully customized control with all appearance options configured.
