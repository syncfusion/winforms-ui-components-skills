# Appearance and Styling in SfNumericTextBox

## Table of Contents
- [Value-Based Colors](#value-based-colors)
- [Border Customization](#border-customization)
- [Background and Font](#background-and-font)
- [Watermark Styling](#watermark-styling)
- [Visual States](#visual-states)
- [Practical Examples](#practical-examples)

## Value-Based Colors

### Purpose

SfNumericTextBox can display different text colors based on whether the value is positive, negative, or zero, providing visual feedback at a glance.

### Color Properties

#### PositiveForeColor

```csharp
this.numericTextBox.Style.PositiveForeColor = Color.Green;
```

**Applied when:**
- Value > 0
- Example: profit, income, credit

#### NegativeForeColor

```csharp
this.numericTextBox.Style.NegativeForeColor = Color.Red;
```

**Applied when:**
- Value < 0
- Example: loss, debt, expense

#### ZeroForeColor

```csharp
this.numericTextBox.Style.ZeroForeColor = Color.Gray;
```

**Applied when:**
- Value == 0
- Example: break-even, no activity

### Combined Setup

```csharp
// Professional financial-style coloring
this.numericTextBox.Style.PositiveForeColor = Color.Green;      // Profit
this.numericTextBox.Style.NegativeForeColor = Color.Red;        // Loss
this.numericTextBox.Style.ZeroForeColor = Color.DarkGray;       // Break-even
```

### Using System Colors

```csharp
// Using predefined system colors
this.numericTextBox.Style.PositiveForeColor = Color.DarkGreen;
this.numericTextBox.Style.NegativeForeColor = Color.DarkRed;
this.numericTextBox.Style.ZeroForeColor = SystemColors.ControlText;
```

### Using Custom Colors

```csharp
// Using hex color codes
this.numericTextBox.Style.PositiveForeColor = ColorTranslator.FromHtml("#00AA00");  // Green
this.numericTextBox.Style.NegativeForeColor = ColorTranslator.FromHtml("#FF0000");  // Red
this.numericTextBox.Style.ZeroForeColor = ColorTranslator.FromHtml("#666666");      // Gray
```

### Practical Examples

#### Financial Application

```csharp
// Dashboard showing profit/loss
this.numericTextBox.Style.PositiveForeColor = Color.Green;       // Green for profit
this.numericTextBox.Style.NegativeForeColor = Color.Red;         // Red for loss
this.numericTextBox.Style.ZeroForeColor = Color.Black;           // Black for neutral

this.numericTextBox.Value = 2500;    // Displays in green
this.numericTextBox.Value = -500;    // Displays in red
this.numericTextBox.Value = 0;       // Displays in black
```

#### Temperature Display

```csharp
// Temperature indicator with semantic coloring
this.numericTextBox.Style.PositiveForeColor = Color.Red;         // Hot
this.numericTextBox.Style.NegativeForeColor = Color.Blue;        // Cold
this.numericTextBox.Style.ZeroForeColor = Color.Black;           // Freezing
this.numericTextBox.Suffix = "°C";
```

#### Inventory Tracker

```csharp
// Stock level indicator
this.numericTextBox.Style.PositiveForeColor = Color.Green;       // Adequate stock
this.numericTextBox.Style.NegativeForeColor = Color.Red;         // Negative (shouldn't happen)
this.numericTextBox.Style.ZeroForeColor = Color.Orange;          // Out of stock
```

## Border Customization

### BorderColor Property

```csharp
this.numericTextBox.Style.BorderColor = Color.Black;
```

**Applied:**
- Normal state when control is not focused
- Only visible if BorderStyle is set to FixedSingle

### FocusBorderColor Property

```csharp
this.numericTextBox.Style.FocusBorderColor = SystemColors.MenuHighlight;
```

**Applied:**
- When control has keyboard focus
- Indicates that control is active and ready for input
- Better UX by showing visual focus feedback

### HoverBorderColor Property

```csharp
this.numericTextBox.Style.HoverBorderColor = ColorTranslator.FromHtml("#e5c365");
```

**Applied:**
- When mouse hovers over the control
- Without focus
- Indicates interactivity

### DisabledBorderColor Property

```csharp
this.numericTextBox.Style.DisabledBorderColor = Color.LightGray;
```

**Applied:**
- When control is disabled (Enabled = false)
- Indicates control is not interactive

### BorderStyle Requirement

Border colors only apply when BorderStyle is set to FixedSingle:

```csharp
this.numericTextBox.BorderStyle = BorderStyle.FixedSingle;

// Then set colors
this.numericTextBox.Style.BorderColor = Color.Gray;
this.numericTextBox.Style.FocusBorderColor = Color.Blue;
this.numericTextBox.Style.HoverBorderColor = Color.LightBlue;
```

### Complete Border Setup

```csharp
// Professional border styling
this.numericTextBox.BorderStyle = BorderStyle.FixedSingle;
this.numericTextBox.Style.BorderColor = ColorTranslator.FromHtml("#ababab");        // Normal
this.numericTextBox.Style.FocusBorderColor = SystemColors.MenuHighlight;             // Focus
this.numericTextBox.Style.HoverBorderColor = ColorTranslator.FromHtml("#e5c365");   // Hover
this.numericTextBox.Style.DisabledBorderColor = Color.LightGray;                     // Disabled
```

### Visual State Progression

```
Disabled State:     DisabledBorderColor (light gray)
Normal State:       BorderColor (dark gray)
Mouse Hover:        HoverBorderColor (light gold)
Focus State:        FocusBorderColor (blue)
```

## Background and Font

### BackColor

```csharp
this.numericTextBox.BackColor = Color.White;
```

### ForeColor (General)

```csharp
// Note: Value-based colors (Positive/Negative/Zero) override this
this.numericTextBox.ForeColor = Color.Black;
```

### Font Customization

```csharp
// Change font and size
this.numericTextBox.Font = new Font("Arial", 12, FontStyle.Regular);

// Bold text
this.numericTextBox.Font = new Font(this.numericTextBox.Font, FontStyle.Bold);

// Italic text
this.numericTextBox.Font = new Font(this.numericTextBox.Font, FontStyle.Italic);
```

### Practical Examples

#### Clear and Professional

```csharp
this.numericTextBox.BackColor = Color.White;
this.numericTextBox.ForeColor = Color.Black;
this.numericTextBox.Font = new Font("Arial", 11);
```

#### Emphasized Input

```csharp
this.numericTextBox.Font = new Font("Arial", 14, FontStyle.Bold);
this.numericTextBox.BackColor = Color.LemonChiffon;  // Highlight
```

#### Dark Theme

```csharp
this.numericTextBox.BackColor = Color.FromArgb(45, 45, 48);
this.numericTextBox.ForeColor = Color.White;
this.numericTextBox.Font = new Font("Segoe UI", 11);
```

## Watermark Styling

### WatermarkForeColor

```csharp
this.numericTextBox.Style.WatermarkForeColor = Color.Gray;
```

**Styling the placeholder text:**

```csharp
// Subtle gray watermark
this.numericTextBox.Style.WatermarkForeColor = Color.LightGray;
this.numericTextBox.WatermarkText = "Enter value";

// Darker watermark (more visible)
this.numericTextBox.Style.WatermarkForeColor = Color.DarkGray;
```

### Watermark Visibility

The watermark appears when:
1. Value is null
2. Control doesn't have focus
3. WatermarkText is set
4. AllowNull is true

### Practical Examples

#### Standard Watermark

```csharp
this.numericTextBox.AllowNull = true;
this.numericTextBox.WatermarkText = "Enter amount";
this.numericTextBox.Style.WatermarkForeColor = Color.Gray;
```

#### Subtle Watermark (Low Contrast)

```csharp
this.numericTextBox.Style.WatermarkForeColor = Color.LightGray;
this.numericTextBox.WatermarkText = "Optional";
```

#### Emphasized Watermark (High Contrast)

```csharp
this.numericTextBox.Style.WatermarkForeColor = Color.Blue;
this.numericTextBox.WatermarkText = "REQUIRED - Enter age";
```

## Visual States

### Complete Styling for All States

```csharp
private void StyleNumericTextBox(SfNumericTextBox textBox)
{
    // Border setup
    textBox.BorderStyle = BorderStyle.FixedSingle;
    
    // Border colors
    textBox.Style.BorderColor = ColorTranslator.FromHtml("#cccccc");
    textBox.Style.FocusBorderColor = SystemColors.MenuHighlight;
    textBox.Style.HoverBorderColor = ColorTranslator.FromHtml("#aaaaaa");
    textBox.Style.DisabledBorderColor = Color.LightGray;
    
    // Value colors
    textBox.Style.PositiveForeColor = Color.Green;
    textBox.Style.NegativeForeColor = Color.Red;
    textBox.Style.ZeroForeColor = Color.Gray;
    
    // Watermark color
    textBox.Style.WatermarkForeColor = Color.LightGray;
    
    // Background and font
    textBox.BackColor = Color.White;
    textBox.ForeColor = Color.Black;
    textBox.Font = new Font("Arial", 11);
}
```

### State Visualization

```
┌─────────────────────────────────────────┐
│ Disabled State                          │
│ [_____________]  (light gray border)    │
│                                         │
├─────────────────────────────────────────┤
│ Normal State                            │
│ [150            ] (gray border)         │
│                                         │
├─────────────────────────────────────────┤
│ Hover State                             │
│ [150            ] (light gold border)   │
│                                         │
├─────────────────────────────────────────┤
│ Focus State                             │
│ [150            ] (blue border)         │
│ ▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔ (cursor blinking)   │
│                                         │
├─────────────────────────────────────────┤
│ Null Value (Watermark)                  │
│ [Enter value    ] (light gray text)     │
│                                         │
├─────────────────────────────────────────┤
│ Positive Value                          │
│ [500            ] (green text)          │
│                                         │
├─────────────────────────────────────────┤
│ Negative Value                          │
│ [-250           ] (red text)            │
│                                         │
├─────────────────────────────────────────┤
│ Zero Value                              │
│ [0              ] (gray text)           │
└─────────────────────────────────────────┘
```

## Practical Examples

### Example 1: Financial Dashboard

```csharp
// Professional financial input styling
this.numericTextBox.BorderStyle = BorderStyle.FixedSingle;
this.numericTextBox.Style.BorderColor = ColorTranslator.FromHtml("#d0d0d0");
this.numericTextBox.Style.FocusBorderColor = Color.RoyalBlue;
this.numericTextBox.Style.PositiveForeColor = Color.DarkGreen;   // Income
this.numericTextBox.Style.NegativeForeColor = Color.DarkRed;     // Expense
this.numericTextBox.Style.ZeroForeColor = Color.DarkGray;        // Balanced
this.numericTextBox.BackColor = Color.White;
this.numericTextBox.Font = new Font("Calibri", 11);
```

### Example 2: Modern Dark Theme

```csharp
// Dark mode styling
this.numericTextBox.BorderStyle = BorderStyle.FixedSingle;
this.numericTextBox.BackColor = Color.FromArgb(45, 45, 48);
this.numericTextBox.ForeColor = Color.White;
this.numericTextBox.Style.BorderColor = Color.FromArgb(70, 70, 70);
this.numericTextBox.Style.FocusBorderColor = Color.FromArgb(100, 150, 200);
this.numericTextBox.Style.WatermarkForeColor = Color.FromArgb(120, 120, 120);
this.numericTextBox.Font = new Font("Segoe UI", 11);
```

### Example 3: Inventory Application

```csharp
// Color-coded inventory levels
this.numericTextBox.BorderStyle = BorderStyle.FixedSingle;
this.numericTextBox.Style.BorderColor = Color.Gray;
this.numericTextBox.Style.FocusBorderColor = Color.Blue;
this.numericTextBox.Style.PositiveForeColor = Color.Green;       // In stock
this.numericTextBox.Style.ZeroForeColor = Color.Orange;          // Out of stock
this.numericTextBox.Style.NegativeForeColor = Color.Red;         // Data error
this.numericTextBox.BackColor = Color.White;
```

### Example 4: Age Input with Emphasis

```csharp
// User-friendly age input
this.numericTextBox.BorderStyle = BorderStyle.FixedSingle;
this.numericTextBox.Font = new Font("Arial", 12, FontStyle.Bold);
this.numericTextBox.Style.BorderColor = Color.LightGray;
this.numericTextBox.Style.FocusBorderColor = Color.Green;
this.numericTextBox.Style.WatermarkForeColor = Color.LightGray;
this.numericTextBox.WatermarkText = "Age (0-120)";
this.numericTextBox.BackColor = Color.White;
```

### Example 5: Temperature Monitor

```csharp
// Temperature display with semantic coloring
this.numericTextBox.BorderStyle = BorderStyle.FixedSingle;
this.numericTextBox.Style.BorderColor = Color.LightGray;
this.numericTextBox.Style.FocusBorderColor = Color.DarkCyan;
this.numericTextBox.Style.PositiveForeColor = Color.Red;         // Hot
this.numericTextBox.Style.NegativeForeColor = Color.Blue;        // Cold
this.numericTextBox.Style.ZeroForeColor = Color.Black;           // Freezing
this.numericTextBox.Suffix = "°C";
this.numericTextBox.Font = new Font("Courier New", 11, FontStyle.Bold);
```

## Important Notes

- **Value-Based Colors Override**: PositiveForeColor/NegativeForeColor/ZeroForeColor override the general ForeColor
- **BorderStyle Required**: Border colors only visible if BorderStyle is FixedSingle
- **Performance**: Theme changes are applied immediately; no performance impact
- **Accessibility**: Ensure sufficient color contrast for readability
- **Watermark Visibility**: Only visible when Value is null and control lacks focus
