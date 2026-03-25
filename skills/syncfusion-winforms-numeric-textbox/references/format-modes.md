# Format Modes in SfNumericTextBox

## Table of Contents
- [Overview](#overview)
- [Numeric Format Mode](#numeric-format-mode)
- [Currency Format Mode](#currency-format-mode)
- [Percent Format Mode](#percent-format-mode)
- [Culture-Specific Formatting](#culture-specific-formatting)
- [NumberFormatInfo Customization](#numberformatinfo-customization)
- [Practical Examples](#practical-examples)

## Overview

The `FormatMode` property determines how numeric values are displayed and interpreted. SfNumericTextBox supports three standard format modes:

1. **Numeric** - General number formatting
2. **Currency** - Money/financial formatting
3. **Percent** - Percentage formatting

By default, the FormatMode is set to **Numeric**.

## Numeric Format Mode

### Usage

```csharp
this.numericTextBox.FormatMode = Syncfusion.WinForms.Input.Enums.FormatMode.Numeric;
```

### Characteristics

- Default format mode
- Displays general numeric values
- Customizable decimal digits, separators, and grouping
- Supports negative numbers
- Culture-aware (uses system culture by default)

### Examples

```csharp
// Numeric mode with value
this.numericTextBox.Value = 1234.567;
// Displays as: 1234.567 (US culture)
// or: 1234,567 (European culture)

// With thousands grouping
this.numericTextBox.Value = 1000000;
// Displays as: 1,000,000 (US culture)
```

### Customizable Properties

- **NumberFormatInfo.NumberDecimalDigits** - Decimal places (default: 2)
- **NumberFormatInfo.NumberDecimalSeparator** - Decimal symbol (default: culture-specific)
- **NumberFormatInfo.NumberGroupSeparator** - Thousands separator (default: culture-specific)
- **NumberFormatInfo.NumberGroupSizes** - Grouping pattern

## Currency Format Mode

### Usage

```csharp
this.numericTextBox.FormatMode = Syncfusion.WinForms.Input.Enums.FormatMode.Currency;
```

### Characteristics

- Includes currency symbol ($ by default in US culture)
- Typically uses 2 decimal places
- Formats with thousands separators
- Culture-specific symbol and spacing
- Professional appearance for monetary values

### Examples

```csharp
// Currency mode with value
this.numericTextBox.Value = 1234.56;
// Displays as: $1,234.56 (US culture)
// or: €1.234,56 (European culture)

// Negative value
this.numericTextBox.Value = -500;
// Displays as: -$500.00
```

### Currency Symbol by Culture

Different cultures use different currency symbols and formats:

```csharp
using System.Globalization;

// US Dollar
var usCulture = new CultureInfo("en-US");
this.numericTextBox.NumberFormatInfo = usCulture.NumberFormat;
// Display: $1,234.56

// Euro (Germany)
var deCulture = new CultureInfo("de-DE");
this.numericTextBox.NumberFormatInfo = deCulture.NumberFormat;
// Display: 1.234,56 € (symbol on right)

// British Pound
var gbCulture = new CultureInfo("en-GB");
this.numericTextBox.NumberFormatInfo = gbCulture.NumberFormat;
// Display: £1,234.56
```

## Percent Format Mode

### Usage

```csharp
this.numericTextBox.FormatMode = Syncfusion.WinForms.Input.Enums.FormatMode.Percent;
```

### Characteristics

- Includes percent symbol (%)
- Typically multiplies display by 100 (0.5 displays as 50%)
- Useful for rates, ratios, and percentages
- Culture-aware symbol positioning
- 2 decimal places by default

### Examples

```csharp
// Percent mode
this.numericTextBox.Value = 0.45;
// Displays as: 45.00%

this.numericTextBox.Value = 0.5;
// Displays as: 50.00%

this.numericTextBox.Value = 1.0;
// Displays as: 100.00%
```

### Practical Use Cases

```csharp
// Interest rate display
this.numericTextBox.FormatMode = FormatMode.Percent;
this.numericTextBox.Value = 0.065;  // 6.5%
this.numericTextBox.MinValue = 0;
this.numericTextBox.MaxValue = 1;

// Completion percentage
this.numericTextBox.Value = 0.75;  // 75%

// Tax rate
this.numericTextBox.Value = 0.08;  // 8%
```

## Culture-Specific Formatting

### Default Culture

By default, SfNumericTextBox uses the application's current UI culture:

```csharp
// Uses CurrentUICulture automatically
this.numericTextBox.FormatMode = FormatMode.Numeric;
this.numericTextBox.Value = 1234.56;
```

### Setting Specific Culture

```csharp
using System.Globalization;

// Set to German culture
var germanCulture = new CultureInfo("de-DE");
this.numericTextBox.NumberFormatInfo = germanCulture.NumberFormat;
this.numericTextBox.Value = 1234.56;
// Displays: 1.234,56 (German format)

// Set to French culture
var frenchCulture = new CultureInfo("fr-FR");
this.numericTextBox.NumberFormatInfo = frenchCulture.NumberFormat;
this.numericTextBox.Value = 1234.56;
// Displays: 1 234,56 (French format with space as thousands separator)
```

### Common Culture Codes

| Culture | Code | Number Display | Currency |
|---------|------|-----------------|----------|
| US English | en-US | 1,234.56 | $1,234.56 |
| German | de-DE | 1.234,56 | 1.234,56 € |
| French | fr-FR | 1 234,56 | 1 234,56 € |
| Italian | it-IT | 1.234,56 | €1.234,56 |
| Spanish | es-ES | 1.234,56 | 1.234,56 € |
| British | en-GB | 1,234.56 | £1,234.56 |
| Japanese | ja-JP | 1,234.56 | ¥1,234 |

## NumberFormatInfo Customization

### What is NumberFormatInfo?

`NumberFormatInfo` is a .NET class that contains culture-specific formatting information. Instead of relying on culture, you can create custom NumberFormatInfo for complete control.

### Creating Custom NumberFormatInfo

```csharp
// Create custom number format
NumberFormatInfo customFormat = new NumberFormatInfo();
customFormat.NumberDecimalSeparator = "*";      // Use * instead of .
customFormat.NumberDecimalDigits = 4;            // 4 decimal places
customFormat.NumberGroupSeparator = "/";         // Use / instead of ,
customFormat.NumberGroupSizes = new int[] { 3 }; // Group by 3 digits

this.numericTextBox.NumberFormatInfo = customFormat;
this.numericTextBox.Value = 1234567.8901;
// Displays: 1234/567*8901
```

### Common Customizations

#### Custom Decimal Separator

```csharp
var format = new NumberFormatInfo();
format.NumberDecimalSeparator = ",";  // Use comma instead of period
format.NumberGroupSeparator = ".";    // Use period for thousands
this.numericTextBox.NumberFormatInfo = format;
this.numericTextBox.Value = 1234.56;
// Displays: 1.234,56
```

#### Custom Currency Symbol

```csharp
var format = new NumberFormatInfo();
format.CurrencySymbol = "CAD ";      // Canadian Dollar
format.CurrencyDecimalDigits = 2;
this.numericTextBox.FormatMode = FormatMode.Currency;
this.numericTextBox.NumberFormatInfo = format;
this.numericTextBox.Value = 100;
// Displays: CAD 100.00
```

#### Custom Percent Symbol

```csharp
var format = new NumberFormatInfo();
format.PercentSymbol = "pct";        // Use "pct" instead of %
this.numericTextBox.FormatMode = FormatMode.Percent;
this.numericTextBox.NumberFormatInfo = format;
this.numericTextBox.Value = 0.5;
// Displays: 50.00pct
```

#### Custom Negative Symbol

```csharp
var format = new NumberFormatInfo();
format.NegativeSign = "~";           // Use ~ instead of -
this.numericTextBox.NumberFormatInfo = format;
this.numericTextBox.Value = -100;
// Displays: ~100
```

### Complete Customization Example

```csharp
NumberFormatInfo customFormat = new NumberFormatInfo();

// Decimal and grouping
customFormat.NumberDecimalSeparator = ",";
customFormat.NumberDecimalDigits = 3;
customFormat.NumberGroupSeparator = "'";
customFormat.NumberGroupSizes = new int[] { 3 };

// Currency
customFormat.CurrencySymbol = "CHF";
customFormat.CurrencyDecimalSeparator = ".";
customFormat.CurrencyDecimalDigits = 2;
customFormat.CurrencyGroupSeparator = "'";

// Negative display
customFormat.NegativeSign = "−";  // Unicode minus

this.numericTextBox.NumberFormatInfo = customFormat;
```

## Practical Examples

### Example 1: US Currency Input

```csharp
// Price input in US dollars
this.numericTextBox.FormatMode = FormatMode.Currency;
this.numericTextBox.MinValue = 0;
this.numericTextBox.MaxValue = 99999.99;
this.numericTextBox.WatermarkText = "Enter price";
this.numericTextBox.Value = 0;
// Display: $0.00
```

### Example 2: European Currency with Custom Format

```csharp
// Price in Euros with custom formatting
var euroFormat = new CultureInfo("de-DE").NumberFormat;
this.numericTextBox.NumberFormatInfo = euroFormat;
this.numericTextBox.FormatMode = FormatMode.Currency;
this.numericTextBox.Value = 1234.56;
// Display: 1.234,56 €
```

### Example 3: Percentage Input (0-100)

```csharp
// User enters values 0-100, stored as decimals internally
// Display as percent
this.numericTextBox.FormatMode = FormatMode.Percent;
this.numericTextBox.MinValue = 0;
this.numericTextBox.MaxValue = 100;
this.numericTextBox.Value = 50;
// Display: 5000% (NOTE: multiplies by 100 for display)

// Alternative: Store as 0-1 range
this.numericTextBox.Value = 0.5;  // 50%
```

### Example 4: Scientific Notation with Custom Format

```csharp
// Large numbers with custom grouping
var format = new NumberFormatInfo();
format.NumberGroupSeparator = " ";  // Space as thousands separator
format.NumberDecimalSeparator = ".";
format.NumberDecimalDigits = 2;
this.numericTextBox.NumberFormatInfo = format;
this.numericTextBox.Value = 1234567.89;
// Display: 1 234 567.89
```

## Important Notes

- **Full Precision**: Regardless of display format, the Value property maintains full precision
- **Culture Independence**: Setting NumberFormatInfo overrides the current culture
- **Null Values**: If NumberFormatInfo is not set, the control uses CurrentUICulture
- **Performance**: Setting NumberFormatInfo multiple times may impact performance; set once during initialization
