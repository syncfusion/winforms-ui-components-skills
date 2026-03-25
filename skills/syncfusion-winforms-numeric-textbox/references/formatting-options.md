# Formatting Options in SfNumericTextBox

## Table of Contents
- [Hiding Trailing Zeros](#hiding-trailing-zeros)
- [Prefix and Suffix](#prefix-and-suffix)
- [Watermark Text](#watermark-text)
- [Combining Options](#combining-options)
- [Examples](#examples)

## Hiding Trailing Zeros

### What are Trailing Zeros?

Trailing zeros are zeros that appear at the end of the decimal part:
- `100.50` has one trailing zero
- `100.10` has one trailing zero
- `100.00` has two trailing zeros
- `100.5` has no trailing zeros

### HideTrailingZeros Property

```csharp
// Hide trailing zeros
this.numericTextBox.HideTrailingZeros = true;
```

### Behavior

When `HideTrailingZeros = true`:

| Value | Display with HideTrailingZeros=false | Display with HideTrailingZeros=true |
|-------|--------------------------------------|-------------------------------------|
| 100.00 | 100.00 | 100 |
| 100.50 | 100.50 | 100.5 |
| 100.05 | 100.05 | 100.05 |
| 10.1 | 10.10 | 10.1 |

### Practical Example: Price Display

```csharp
// Price display - hide unnecessary zeros
this.numericTextBox.FormatMode = FormatMode.Currency;
this.numericTextBox.HideTrailingZeros = true;

this.numericTextBox.Value = 10.00;
// Displays: $10 (instead of $10.00)

this.numericTextBox.Value = 19.50;
// Displays: $19.5 (instead of $19.50)

this.numericTextBox.Value = 19.99;
// Displays: $19.99 (no trailing zeros to hide)
```

### Use Cases

- **Cleaner Display**: Reduces visual clutter
- **User-Friendly**: More natural appearance for whole numbers
- **E-commerce**: Common for price displays
- **Forms**: Improves readability in data entry

### Note

- Only removes unnecessary zeros, not significant decimal places
- If value is 100.05, displays as `100.05` even with HideTrailingZeros=true
- The actual Value property is unaffected; formatting only affects display

## Prefix and Suffix

### Overview

Prefix and Suffix add custom text before and after the numeric value:

```csharp
this.numericTextBox.Prefix = "Your text here";
this.numericTextBox.Suffix = "Your text here";
```

### Prefix Property

Adds text at the beginning of the value:

```csharp
this.numericTextBox.Prefix = "Total: ";
this.numericTextBox.Value = 500;
// Displays: Total: 500
```

### Suffix Property

Adds text at the end of the value:

```csharp
this.numericTextBox.Suffix = " kg";
this.numericTextBox.Value = 75.5;
// Displays: 75.5 kg
```

### Combining Prefix and Suffix

```csharp
this.numericTextBox.Prefix = "$";
this.numericTextBox.Suffix = " USD";
this.numericTextBox.Value = 100;
// Displays: $100 USD

// Alternative with currency mode
this.numericTextBox.FormatMode = FormatMode.Currency;
this.numericTextBox.Suffix = " per unit";
this.numericTextBox.Value = 19.99;
// Displays: $19.99 per unit
```

### Unit Examples

| Unit | Suffix | Example | Display |
|------|--------|---------|---------|
| Length | " cm" | 25.5 | 25.5 cm |
| Weight | " kg" | 75 | 75 kg |
| Distance | " miles" | 10.25 | 10.25 miles |
| Speed | " km/h" | 60 | 60 km/h |
| Temperature | "°C" | 23 | 23°C |
| Percent | "%" | 85 | 85% |

### Prefix Examples

| Context | Prefix | Example | Display |
|---------|--------|---------|---------|
| Label | "Price: " | 19.99 | Price: 19.99 |
| Currency | "$ " | 100 | $ 100 |
| Counter | "Count: " | 42 | Count: 42 |
| Delta | "Δ " | 5.2 | Δ 5.2 |

### Practical Examples

#### Distance Measurement

```csharp
this.numericTextBox.Suffix = " miles";
this.numericTextBox.HideTrailingZeros = true;
this.numericTextBox.Value = 10.5;
// Displays: 10.5 miles
```

#### Cost with Units

```csharp
this.numericTextBox.Prefix = "Price: ";
this.numericTextBox.Suffix = " per unit";
this.numericTextBox.FormatMode = FormatMode.Currency;
this.numericTextBox.Value = 29.99;
// Displays: Price: $29.99 per unit
```

#### Measurement

```csharp
this.numericTextBox.Prefix = "Height: ";
this.numericTextBox.Suffix = " inches";
this.numericTextBox.Value = 72;
// Displays: Height: 72 inches
```

## Watermark Text

### What is Watermark?

Watermark is placeholder text shown when the control's value is null, providing guidance to users:

```csharp
this.numericTextBox.WatermarkText = "Enter value here";
this.numericTextBox.AllowNull = true;
```

### Requirements

To use watermark:

1. Set `AllowNull = true` to allow null values
2. Set `WatermarkText` property
3. Leave `Value = null` (or don't set it)

```csharp
// Enable watermark
this.numericTextBox.AllowNull = true;
this.numericTextBox.WatermarkText = "Enter age";
this.numericTextBox.Value = null;  // Explicitly set to null
```

### When Watermark Appears

- When `Value == null` AND
- Control does NOT have focus
- As soon as focus is lost

### When Watermark Disappears

- When user enters a value
- When control receives focus
- When Value is set programmatically

### WatermarkForeColor

Customize the color of watermark text:

```csharp
this.numericTextBox.Style.WatermarkForeColor = Color.Gray;
this.numericTextBox.WatermarkText = "Enter value";
```

### Practical Examples

#### Age Input

```csharp
this.numericTextBox.AllowNull = true;
this.numericTextBox.WatermarkText = "Age (0-120)";
this.numericTextBox.MinValue = 0;
this.numericTextBox.MaxValue = 120;
```

#### Quantity Input

```csharp
this.numericTextBox.AllowNull = true;
this.numericTextBox.WatermarkText = "Qty";
this.numericTextBox.MinValue = 1;
this.numericTextBox.MaxValue = 1000;
```

#### Price Input

```csharp
this.numericTextBox.FormatMode = FormatMode.Currency;
this.numericTextBox.AllowNull = true;
this.numericTextBox.WatermarkText = "Enter amount";
```

#### Percentage Input

```csharp
this.numericTextBox.FormatMode = FormatMode.Percent;
this.numericTextBox.AllowNull = true;
this.numericTextBox.WatermarkText = "Enter percentage";
this.numericTextBox.MinValue = 0;
this.numericTextBox.MaxValue = 1;
```

### Watermark with Units

```csharp
// Watermark guides user with expected units
this.numericTextBox.AllowNull = true;
this.numericTextBox.Suffix = " kg";
this.numericTextBox.WatermarkText = "Weight in kg";
```

## Combining Options

### Using Multiple Options Together

You can combine prefix, suffix, hiding zeros, and watermark:

```csharp
// Rich formatting example
this.numericTextBox.FormatMode = FormatMode.Numeric;
this.numericTextBox.Prefix = "Weight: ";
this.numericTextBox.Suffix = " lbs";
this.numericTextBox.HideTrailingZeros = true;
this.numericTextBox.AllowNull = true;
this.numericTextBox.WatermarkText = "Enter weight in lbs";
this.numericTextBox.MinValue = 0;
this.numericTextBox.MaxValue = 500;

// Display when value is set: Weight: 150 lbs
// Display when value is null: Weight: [Enter weight in lbs]
```

### Option Priority

When multiple formatting options interact:

1. **Value** is stored with full precision
2. **FormatMode** applies (Numeric/Currency/Percent)
3. **NumberFormatInfo** customizes the format
4. **Prefix** appears first
5. **The formatted number** appears
6. **Suffix** appears last
7. **Watermark** overrides everything if Value is null

Example order:

```csharp
Prefix + [FormatMode(Value)] + Suffix
"$" + "1,234.56" + "/month" = "$1,234.56/month"
```

## Examples

### Example 1: Currency Input with Range

```csharp
// Financial input with clear guidance
this.numericTextBox.FormatMode = FormatMode.Currency;
this.numericTextBox.MinValue = 0;
this.numericTextBox.MaxValue = 100000;
this.numericTextBox.AllowNull = true;
this.numericTextBox.WatermarkText = "Enter amount (0-100000)";
this.numericTextBox.HideTrailingZeros = false;  // Keep .00
```

### Example 2: Product Measurement

```csharp
// Product dimension input
this.numericTextBox.Suffix = " cm";
this.numericTextBox.HideTrailingZeros = true;
this.numericTextBox.MinValue = 0.1;
this.numericTextBox.MaxValue = 500;
this.numericTextBox.AllowNull = true;
this.numericTextBox.WatermarkText = "Length in cm";
this.numericTextBox.Value = null;
// Display when null: [Length in cm]
// Display when value 10: 10 cm
```

### Example 3: Tax/Interest Rate

```csharp
// Percentage display 0-100%
this.numericTextBox.Suffix = "%";
this.numericTextBox.HideTrailingZeros = true;
this.numericTextBox.MinValue = 0;
this.numericTextBox.MaxValue = 100;
this.numericTextBox.AllowNull = true;
this.numericTextBox.WatermarkText = "Interest rate %";
this.numericTextBox.Value = null;
// Display: [Interest rate %]
```

### Example 4: Discount Amount

```csharp
// Discount in currency
this.numericTextBox.FormatMode = FormatMode.Currency;
this.numericTextBox.Prefix = "Discount: ";
this.numericTextBox.MinValue = 0;
this.numericTextBox.MaxValue = 500;
this.numericTextBox.AllowNull = true;
this.numericTextBox.WatermarkText = "Optional discount";
this.numericTextBox.Value = null;
// Display: Discount: [Optional discount]
// Display: Discount: $50.00 (when value set)
```

### Example 5: Quantity with Complex Format

```csharp
// Quantity with all options
this.numericTextBox.Prefix = "Qty: ";
this.numericTextBox.Suffix = " units";
this.numericTextBox.HideTrailingZeros = true;
this.numericTextBox.MinValue = 1;
this.numericTextBox.MaxValue = 999;
this.numericTextBox.Value = 5;
// Display: Qty: 5 units
```

## Important Notes

- **Watermark vs Prefix/Suffix**: When Value is null, watermark is shown (not the prefix/suffix)
- **Suffix with Currency**: If using FormatMode.Currency, the currency symbol comes from that, not suffix
- **Performance**: Formatting is applied on display; no impact on calculations
- **AllowNull**: Must be true to show watermark text
