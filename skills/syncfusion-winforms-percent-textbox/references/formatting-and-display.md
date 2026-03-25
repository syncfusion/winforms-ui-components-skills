# Formatting and Display in PercentTextBox

## Table of Contents
- [Decimal Digits Configuration](#decimal-digits-configuration)
- [Separator Customization](#separator-customization)
- [Group Sizes](#group-sizes)
- [Format Customization Examples](#format-customization-examples)
- [Common Formatting Patterns](#common-formatting-patterns)
- [Display Formatting Reference](#display-formatting-reference)

## Decimal Digits Configuration

### PercentDecimalDigits Property

Controls the number of decimal places displayed.

```csharp
// No decimal places (e.g., 50%)
percentTextBox1.PercentDecimalDigits = 0;

// Two decimal places (e.g., 50.00%)
percentTextBox1.PercentDecimalDigits = 2;

// Three decimal places (e.g., 50.123%)
percentTextBox1.PercentDecimalDigits = 3;

// Five decimal places (e.g., 50.12345%)
percentTextBox1.PercentDecimalDigits = 5;
```

### Effect on Value Display

```csharp
// With PercentDecimalDigits = 2
percentTextBox1.PercentValue = 50.666;
// Displays as: 50.67% (rounded)

// With PercentDecimalDigits = 0
percentTextBox1.PercentValue = 50.666;
// Displays as: 51% (rounded to nearest integer)

// With PercentDecimalDigits = 4
percentTextBox1.PercentValue = 50.666;
// Displays as: 50.6660% (padded with zeros)
```

### Practical Example: Price Discount

```csharp
// Discount should show 2 decimals (e.g., 15.50%)
private void SetupDiscountField(PercentTextBox control)
{
    control.PercentDecimalDigits = 2;
    control.MinValue = 0;
    control.MaxValue = 100;
}

// When user enters 15.5, displays as 15.50%
SetupDiscountField(discountBox);
```

## Separator Customization

### PercentDecimalSeparator

The character used to separate whole and decimal parts.

```csharp
// Use period (default in US)
percentTextBox1.PercentDecimalSeparator = ".";
// Result: 50.50%

// Use comma (default in many European regions)
percentTextBox1.PercentDecimalSeparator = ",";
// Result: 50,50%

// Use custom character
percentTextBox1.PercentDecimalSeparator = "·";
// Result: 50·50%
```

### PercentGroupSeparator

The character used to separate thousands groups.

```csharp
// Use comma for thousands (US style)
percentTextBox1.PercentGroupSeparator = ",";
// Result: 1,234.50%

// Use period for thousands (European style)
percentTextBox1.PercentGroupSeparator = ".";
// Result: 1.234,50%

// Use space for thousands
percentTextBox1.PercentGroupSeparator = " ";
// Result: 1 234,50%

// Use no separator
percentTextBox1.PercentGroupSeparator = "";
// Result: 1234.50%
```

## Group Sizes

### PercentGroupSizes Property

Defines how digits are grouped for the thousands separator.

```csharp
// Standard: group by 3 digits (1,000)
percentTextBox1.PercentGroupSizes = new int[] { 3 };
// Result: 1,234.56%

// Group by 2 digits
percentTextBox1.PercentGroupSizes = new int[] { 2 };
// Result: 12,34.56%

// Indian style: first group of 2, then groups of 3
percentTextBox1.PercentGroupSizes = new int[] { 2, 3 };
// Result: 12,34,567.89%

// Multiple groups with varying sizes
percentTextBox1.PercentGroupSizes = new int[] { 3, 2 };
// Result: groups of 3, then groups of 2
```

## Format Customization Examples

### Example 1: US Currency Style (15.50%)

```csharp
percentTextBox1.PercentDecimalDigits = 2;
percentTextBox1.PercentDecimalSeparator = ".";
percentTextBox1.PercentGroupSeparator = ",";
percentTextBox1.PercentGroupSizes = new int[] { 3 };

// Input: 1234.5
// Display: 1,234.50%
```

### Example 2: European Style (15,50%)

```csharp
percentTextBox1.PercentDecimalDigits = 2;
percentTextBox1.PercentDecimalSeparator = ",";
percentTextBox1.PercentGroupSeparator = ".";
percentTextBox1.PercentGroupSizes = new int[] { 3 };

// Input: 1234.5
// Display: 1.234,50%
```

### Example 3: Scientific Notation (4 decimal places)

```csharp
percentTextBox1.PercentDecimalDigits = 4;
percentTextBox1.PercentDecimalSeparator = ".";
percentTextBox1.PercentGroupSeparator = ",";
percentTextBox1.PercentGroupSizes = new int[] { 3 };

// Input: 99.99999
// Display: 100,0000%
```

### Example 4: No Thousands Separator, High Precision

```csharp
percentTextBox1.PercentDecimalDigits = 6;
percentTextBox1.PercentDecimalSeparator = ".";
percentTextBox1.PercentGroupSeparator = "";  // No separator
percentTextBox1.PercentGroupSizes = new int[] { };

// Input: 1234.123456
// Display: 1234.123456%
```

### Example 5: Indian Currency Format

```csharp
percentTextBox1.PercentDecimalDigits = 2;
percentTextBox1.PercentDecimalSeparator = ".";
percentTextBox1.PercentGroupSeparator = ",";
percentTextBox1.PercentGroupSizes = new int[] { 2, 3 };

// Input: 1234567.89
// Display: 12,34,567.89%
```

## Common Formatting Patterns

### Pattern 1: Simple Integer Percentage

Use when only whole percentages matter (e.g., completion %).

```csharp
private void FormatAsInteger(PercentTextBox control)
{
    control.PercentDecimalDigits = 0;
    control.PercentGroupSeparator = "";
}

// Usage: 50% (no decimals or thousands)
FormatAsInteger(progressBox);
```

### Pattern 2: Financial Percentage

Use for financial calculations requiring precision.

```csharp
private void FormatAsFinancial(PercentTextBox control)
{
    control.PercentDecimalDigits = 2;
    control.PercentDecimalSeparator = ".";
    control.PercentGroupSeparator = ",";
    control.PercentGroupSizes = new int[] { 3 };
}

// Usage: 15,234.56% (for large numbers)
FormatAsFinancial(interestRateBox);
```

### Pattern 3: Scientific/Technical Percentage

Use for technical values requiring high precision.

```csharp
private void FormatAsScientific(PercentTextBox control)
{
    control.PercentDecimalDigits = 5;
    control.PercentDecimalSeparator = ".";
    control.PercentGroupSeparator = "";
}

// Usage: 50.12345%
FormatAsScientific(calibrationBox);
```

### Pattern 4: Locale-Aware Formatting

Adapt formatting to user's locale.

```csharp
private void FormatByLocale(PercentTextBox control, CultureInfo culture)
{
    control.PercentDecimalDigits = 2;
    control.PercentDecimalSeparator = culture.NumberFormat.NumberDecimalSeparator;
    control.PercentGroupSeparator = culture.NumberFormat.NumberGroupSeparator;
    control.PercentGroupSizes = culture.NumberFormat.NumberGroupSizes;
}

// Usage
var enUS = new CultureInfo("en-US");
FormatByLocale(percentTextBox1, enUS);

var deDEU = new CultureInfo("de-DE");
FormatByLocale(percentTextBox2, deDEU);
```

### Pattern 5: Tax Rate Display

```csharp
private void FormatAsTaxRate(PercentTextBox control)
{
    control.PercentDecimalDigits = 2;
    control.MinValue = 0;
    control.MaxValue = 100;
    control.PercentGroupSeparator = ",";
}

// Usage: Display like "15.00%" or "8.75%"
FormatAsTaxRate(taxRateBox);
```

## Display Formatting Reference

### Quick Format Selection

```csharp
// Whole percentages only
control.PercentDecimalDigits = 0;

// One decimal place (e.g., 15.5%)
control.PercentDecimalDigits = 1;

// Two decimal places (most common)
control.PercentDecimalDigits = 2;

// Three decimal places (higher precision)
control.PercentDecimalDigits = 3;

// Four to six decimal places (scientific)
control.PercentDecimalDigits = 4;  // or 5, 6
```

### Separator Selection by Region

| Region | Decimal Sep | Group Sep | Example |
|--------|-------------|-----------|---------|
| US | . | , | 1,234.56% |
| Europe | , | . | 1.234,56% |
| India | . | , | 12,34,567.89% |
| Brazil | , | . | 1.234,56% |

### Complete Setup Templates

```csharp
// Template 1: Default US Format
percentTextBox1.PercentDecimalDigits = 2;
percentTextBox1.PercentDecimalSeparator = ".";
percentTextBox1.PercentGroupSeparator = ",";
percentTextBox1.PercentGroupSizes = new int[] { 3 };

// Template 2: European Format
percentTextBox1.PercentDecimalDigits = 2;
percentTextBox1.PercentDecimalSeparator = ",";
percentTextBox1.PercentGroupSeparator = ".";
percentTextBox1.PercentGroupSizes = new int[] { 3 };

// Template 3: No Formatting
percentTextBox1.PercentDecimalDigits = 0;
percentTextBox1.PercentGroupSeparator = "";

// Template 4: High Precision
percentTextBox1.PercentDecimalDigits = 4;
percentTextBox1.PercentDecimalSeparator = ".";
percentTextBox1.PercentGroupSeparator = ",";
percentTextBox1.PercentGroupSizes = new int[] { 3 };
```

## Practical Usage Example

```csharp
public class PercentBoxConfigurator
{
    public static void SetupForTaxRate(PercentTextBox control)
    {
        control.PercentDecimalDigits = 2;
        control.PercentDecimalSeparator = ".";
        control.PercentGroupSeparator = ",";
        control.PercentGroupSizes = new int[] { 3 };
        control.MinValue = 0;
        control.MaxValue = 100;
    }

    public static void SetupForDiscount(PercentTextBox control)
    {
        control.PercentDecimalDigits = 2;
        control.PercentGroupSeparator = "";  // No thousands for small numbers
        control.MinValue = 0;
        control.MaxValue = 100;
    }

    public static void SetupForGrowthRate(PercentTextBox control)
    {
        control.PercentDecimalDigits = 2;
        control.MinValue = -100;  // Allow negative for decline
        control.MaxValue = 10000;  // Allow large values
    }
}

// Usage
PercentBoxConfigurator.SetupForTaxRate(taxBox);
PercentBoxConfigurator.SetupForDiscount(discountBox);
PercentBoxConfigurator.SetupForGrowthRate(growthBox);
```

---

**Next:** Learn behavior settings in [behavior-and-input.md](behavior-and-input.md) or events in [events-and-data-binding.md](events-and-data-binding.md)
