# Number and Format Settings

## Table of Contents
- [Number Formatting Properties](#number-formatting-properties)
- [Configuring Value Range](#configuring-value-range)
- [Banner Text Support](#banner-text-support)
- [Hiding Trailing Zeros](#hiding-trailing-zeros)
- [Complete Configuration Examples](#complete-configuration-examples)

## Number Formatting Properties

The DoubleTextBox provides multiple properties to control how numeric values are displayed:

### Core Formatting Properties

| Property | Type | Purpose |
|----------|------|---------|
| `DoubleValue` | double | Gets or sets the numeric value |
| `NumberDecimalDigits` | int | Number of decimal places to display |
| `NumberDecimalSeparator` | string | Character used to separate whole and fractional parts |
| `NumberGroupSeparator` | string | Character used to separate groups (thousands) |
| `NumberGroupSizes` | int[] | Array defining the size of each digit group |
| `NumberNegativePattern` | int | Index indicating format pattern for negative numbers |

### Decimal Digits Configuration

Controls the number of decimal places displayed and entered:

**C# Example:**

```csharp
// Display 2 decimal places (for currency)
doubleTextBox1.NumberDecimalDigits = 2;

// Display 5 decimal places (for scientific data)
doubleTextBox1.NumberDecimalDigits = 5;

// No decimal places (integer display)
doubleTextBox1.NumberDecimalDigits = 0;
```

**VB.NET Example:**

```vbnet
' Display 2 decimal places
doubleTextBox1.NumberDecimalDigits = 2

' Display 5 decimal places
doubleTextBox1.NumberDecimalDigits = 5

' No decimal places
doubleTextBox1.NumberDecimalDigits = 0
```

### Separator Configuration

**C# Example:**

```csharp
// US format: 1,234.56
doubleTextBox1.NumberDecimalSeparator = ".";
doubleTextBox1.NumberGroupSeparator = ",";

// European format: 1.234,56
doubleTextBox1.NumberDecimalSeparator = ",";
doubleTextBox1.NumberGroupSeparator = ".";

// No separators: 1234.56
doubleTextBox1.NumberGroupSeparator = "";
doubleTextBox1.NumberDecimalSeparator = ".";
```

**VB.NET Example:**

```vbnet
' US format: 1,234.56
doubleTextBox1.NumberDecimalSeparator = "."
doubleTextBox1.NumberGroupSeparator = ","

' European format: 1.234,56
doubleTextBox1.NumberDecimalSeparator = ","
doubleTextBox1.NumberGroupSeparator = "."

' No separators: 1234.56
doubleTextBox1.NumberGroupSeparator = ""
doubleTextBox1.NumberDecimalSeparator = "."
```

### Group Sizes

Defines how digits are grouped for the thousands separator:

**C# Example:**

```csharp
// Standard 3-digit grouping: 1,234,567
doubleTextBox1.NumberGroupSizes = new int[] { 3 };

// Indian format (3-2-2): 12,34,567
doubleTextBox1.NumberGroupSizes = new int[] { 3, 2 };

// Custom grouping: 1,234,567 with different patterns
doubleTextBox1.NumberGroupSizes = new int[] { 3 };
```

## Configuring Value Range

Enforce minimum and maximum constraints on user input:

**C# Example:**

```csharp
// Set constraints
doubleTextBox1.MinValue = 0.0;
doubleTextBox1.MaxValue = 100.0;

// Set initial value
doubleTextBox1.DoubleValue = 50.0;
```

**VB.NET Example:**

```vbnet
' Set constraints
doubleTextBox1.MinValue = 0.0
doubleTextBox1.MaxValue = 100.0

' Set initial value
doubleTextBox1.DoubleValue = 50.0
```

**Important Notes:**
- Values outside the range are automatically corrected to the nearest boundary
- Validation happens when user loses focus or presses Enter
- MinValue can be negative
- MaxValue must be greater than MinValue

## Banner Text Support

Display placeholder text when the DoubleTextBox is empty:

**Setup Requirements:**

```csharp
// Enable banner text
doubleTextBox1.AllowNull = true;
doubleTextBox1.NullString = "";
doubleTextBox1.Text = "";
```

Or in VB.NET:

```vbnet
' Enable banner text
doubleTextBox1.AllowNull = True
doubleTextBox1.NullString = ""
doubleTextBox1.Text = ""
```

With this configuration, you can use a BannerTextProvider component to display hint text.

## Hiding Trailing Zeros

The HideTrailingZeros property controls whether trailing zeros appear in the display:

**C# Example - Show Trailing Zeros (Default):**

```csharp
doubleTextBox1.NumberDecimalDigits = 3;
doubleTextBox1.HideTrailingZeros = false;
doubleTextBox1.DoubleValue = 10.5;
// Displays: 10.500
```

**C# Example - Hide Trailing Zeros:**

```csharp
doubleTextBox1.NumberDecimalDigits = 3;
doubleTextBox1.HideTrailingZeros = true;
doubleTextBox1.DoubleValue = 10.5;
// Displays: 10.5
```

**VB.NET Example:**

```vbnet
doubleTextBox1.NumberDecimalDigits = 3
doubleTextBox1.HideTrailingZeros = False
doubleTextBox1.DoubleValue = 10.5
' Displays: 10.500

doubleTextBox1.HideTrailingZeros = True
' Displays: 10.5
```

**When to Use:**
- Set to `true` for cleaner display of user-entered values
- Set to `false` for consistent formatting (e.g., currency always shows cents)

## Complete Configuration Examples

### Example 1: US Currency Input

**C# Code:**

```csharp
DoubleTextBox currencyBox = new DoubleTextBox();
currencyBox.MinValue = 0;
currencyBox.MaxValue = 999999.99;
currencyBox.NumberDecimalDigits = 2;
currencyBox.NumberDecimalSeparator = ".";
currencyBox.NumberGroupSeparator = ",";
currencyBox.NumberGroupSizes = new int[] { 3 };
currencyBox.HideTrailingZeros = false;
currencyBox.DoubleValue = 1234.56;
// Displays: 1,234.56
```

### Example 2: Scientific Data Entry

**C# Code:**

```csharp
DoubleTextBox scientificBox = new DoubleTextBox();
scientificBox.MinValue = -1000000;
scientificBox.MaxValue = 1000000;
scientificBox.NumberDecimalDigits = 5;
scientificBox.NumberDecimalSeparator = ".";
scientificBox.NumberGroupSeparator = "";
scientificBox.HideTrailingZeros = true;
scientificBox.DoubleValue = 123.456789;
// Displays: 123.45679
```

### Example 3: Percentage Input (0-100%)

**C# Code:**

```csharp
DoubleTextBox percentageBox = new DoubleTextBox();
percentageBox.MinValue = 0;
percentageBox.MaxValue = 100;
percentageBox.NumberDecimalDigits = 2;
percentageBox.NumberGroupSeparator = "";
percentageBox.DoubleValue = 75.5;
// Displays: 75.50
```

### Example 4: European Format

**C# Code:**

```csharp
DoubleTextBox europeanBox = new DoubleTextBox();
europeanBox.MinValue = 0;
europeanBox.MaxValue = 9999999.99;
europeanBox.NumberDecimalDigits = 2;
europeanBox.NumberDecimalSeparator = ",";
europeanBox.NumberGroupSeparator = ".";
europeanBox.NumberGroupSizes = new int[] { 3 };
europeanBox.DoubleValue = 1234567.89;
// Displays: 1.234.567,89
```
