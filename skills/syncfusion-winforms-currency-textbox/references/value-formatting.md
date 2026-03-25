# Value Formatting

## DecimalValue Property

### Programmatic Access to Numeric Value

The `DecimalValue` property provides access to the underlying decimal number without formatting:

```csharp
// Read the current value
decimal amount = currencyTextBox1.DecimalValue;

// Set the value
currencyTextBox1.DecimalValue = 1234.56m;

// The Text property shows formatted version
string display = currencyTextBox1.Text;  // Returns: "$1,234.56"
```

**When to use DecimalValue:**
- Reading for calculations and business logic
- Setting initial values or defaults
- Storing/loading from database
- Comparing values
- Performing arithmetic operations

### Value vs Text Distinction

```csharp
// Value 1234.56
currencyTextBox1.DecimalValue = 1234.56m;

// Different formatting displays same value
currencyTextBox1.CurrencySymbol = "$";
string text1 = currencyTextBox1.Text;  // "$1,234.56"

currencyTextBox1.CurrencySymbol = "€";
string text2 = currencyTextBox1.Text;  // "€1,234.56"

// Value unchanged
decimal value = currencyTextBox1.DecimalValue;  // Still 1234.56m
```

## Currency Symbol Customization

### Setting the Currency Symbol

```csharp
// US Dollar
currencyTextBox1.CurrencySymbol = "$";
currencyTextBox1.DecimalValue = 100m;
// Display: $100.00

// Euro
currencyTextBox1.CurrencySymbol = "€";
// Display: €100.00

// British Pound
currencyTextBox1.CurrencySymbol = "£";
// Display: £100.00

// Japanese Yen
currencyTextBox1.CurrencySymbol = "¥";
// Display: ¥100.00

// Indian Rupee
currencyTextBox1.CurrencySymbol = "₹";
// Display: ₹100.00

// Chinese Yuan
currencyTextBox1.CurrencySymbol = "¥";
// Display: ¥100.00

// Canadian Dollar
currencyTextBox1.CurrencySymbol = "C$";
// Display: C$100.00
```

### Multi-Character Symbols

```csharp
// Two-character symbols
currencyTextBox1.CurrencySymbol = "US$";
// Display: US$100.00

currencyTextBox1.CurrencySymbol = "HK$";
// Display: HK$100.00

// Currency codes
currencyTextBox1.CurrencySymbol = "USD";
// Display: USD100.00

currencyTextBox1.CurrencySymbol = "EUR";
// Display: EUR100.00
```

### Custom Symbols

```csharp
// Project-specific symbol
currencyTextBox1.CurrencySymbol = "PT";  // Project Tokens
// Display: PT100.00

// Credit/Debit symbols
currencyTextBox1.CurrencySymbol = "CR";  // Credit
currencyTextBox1.CurrencySymbol = "DR";  // Debit

// Bitcoin/Crypto
currencyTextBox1.CurrencySymbol = "₿";
// Display: ₿100.00

// Generic currency
currencyTextBox1.CurrencySymbol = "¤";
// Display: ¤100.00
```

## Number Formatting (Digits Before Decimal)

### CurrencyNumberDigits Property

Controls the maximum number of digits before the decimal point:

```csharp
// Standard: typically 10 digits
currencyTextBox1.CurrencyNumberDigits = 10;
// Allows values up to: 9,999,999,999.99

// Small amounts: 5 digits
currencyTextBox1.CurrencyNumberDigits = 5;
// Allows values up to: 99,999.99

// Large amounts: 15 digits
currencyTextBox1.CurrencyNumberDigits = 15;
// Allows values up to: 999,999,999,999,999.99
```

### Relationship with MaxValue/MinValue

```csharp
// Number digits affects display width, not value limits
currencyTextBox1.CurrencyNumberDigits = 8;  // Display up to 8 digits

// MaxValue/MinValue enforce actual limits
currencyTextBox1.MaxValue = 999999.99m;  // Hard limit on value
currencyTextBox1.MinValue = 0m;

// User cannot exceed MaxValue regardless of CurrencyNumberDigits
```

## Decimal Precision and Formatting

### CurrencyDecimalDigits Property

Number of decimal places displayed:

```csharp
// Two decimal places (standard for USD, EUR, etc.)
currencyTextBox1.CurrencyDecimalDigits = 2;
currencyTextBox1.DecimalValue = 100.5m;
// Display: $100.50

// Three decimal places (Kuwait Dinar, Bahraini Dinar)
currencyTextBox1.CurrencyDecimalDigits = 3;
// Display: $100.500

// No decimal places (Japanese Yen, no subunits)
currencyTextBox1.CurrencyDecimalDigits = 0;
// Display: $100

// Four decimal places (some commodities)
currencyTextBox1.CurrencyDecimalDigits = 4;
// Display: $100.5000
```

### Decimal Separator Character

```csharp
// Period (US/English)
currencyTextBox1.CurrencyDecimalSeparator = ".";
currencyTextBox1.DecimalValue = 1234.56m;
// Display: $1,234.56

// Comma (European)
currencyTextBox1.CurrencyDecimalSeparator = ",";
// Display: $1,234,56

// Custom separator
currencyTextBox1.CurrencyDecimalSeparator = "·";
// Display: $1,234·56

// Unicode separator
currencyTextBox1.CurrencyDecimalSeparator = "٫";  // Arabic decimal
```

## Group Separator (Thousands Separator)

### CurrencyGroupSeparator Property

Character that separates thousands:

```csharp
// Comma (US convention)
currencyTextBox1.CurrencyGroupSeparator = ",";
currencyTextBox1.CurrencyGroupSizes = new int[] { 3 };
currencyTextBox1.DecimalValue = 1234567.89m;
// Display: $1,234,567.89

// Period (European convention)
currencyTextBox1.CurrencyGroupSeparator = ".";
// Display: $1.234.567,89

// Space (French convention)
currencyTextBox1.CurrencyGroupSeparator = " ";
// Display: $1 234 567.89

// No separator
currencyTextBox1.CurrencyGroupSeparator = "";
// Display: $1234567.89

// Apostrophe (Swiss)
currencyTextBox1.CurrencyGroupSeparator = "'";
// Display: $1'234'567.89
```

## Currency Group Sizes (Grouping Pattern)

### Standard Grouping (3 Digits)

```csharp
// Most Western currencies: groups of 3
currencyTextBox1.CurrencyGroupSizes = new int[] { 3 };
currencyTextBox1.CurrencyGroupSeparator = ",";
currencyTextBox1.DecimalValue = 1234567.89m;
// Display: $1,234,567.89
```

### Indian Numbering System

```csharp
// Indian format: 3, then 2, then 2...
// 12,34,567.89
currencyTextBox1.CurrencyGroupSizes = new int[] { 3, 2 };
currencyTextBox1.CurrencyGroupSeparator = ",";
currencyTextBox1.CurrencyDecimalSeparator = ".";
currencyTextBox1.DecimalValue = 1234567.89m;
// Display: $12,34,567.89
```

### Chinese Grouping System

```csharp
// Chinese format: groups of 4 (wan, qian, etc.)
currencyTextBox1.CurrencyGroupSizes = new int[] { 4 };
currencyTextBox1.CurrencyGroupSeparator = ",";
currencyTextBox1.DecimalValue = 100000000m;
// Display: $1,0000,0000
```

### Custom Grouping

```csharp
// Groups of 2
currencyTextBox1.CurrencyGroupSizes = new int[] { 2 };
currencyTextBox1.DecimalValue = 1234567.89m;
// Display: $12,34,56,7.89

// Mixed grouping: 4, then 3
currencyTextBox1.CurrencyGroupSizes = new int[] { 4, 3 };
currencyTextBox1.DecimalValue = 1234567890m;
// Display: $1,234,567,890
```

## RemoveDecimalZeros Property

### Trailing Zero Removal

Remove trailing zeros from the decimal portion:

```csharp
// Keep trailing zeros (default)
currencyTextBox1.RemoveDecimalZeros = false;
currencyTextBox1.CurrencyDecimalDigits = 2;

currencyTextBox1.DecimalValue = 100m;
// Display: $100.00

currencyTextBox1.DecimalValue = 100.5m;
// Display: $100.50

currencyTextBox1.DecimalValue = 100.55m;
// Display: $100.55
```

```csharp
// Remove trailing zeros
currencyTextBox1.RemoveDecimalZeros = true;
currencyTextBox1.CurrencyDecimalDigits = 2;

currencyTextBox1.DecimalValue = 100m;
// Display: $100

currencyTextBox1.DecimalValue = 100.5m;
// Display: $100.5

currencyTextBox1.DecimalValue = 100.55m;
// Display: $100.55
```

### Use Cases for Removing Zeros

```csharp
// Cryptocurrency prices (variable precision)
currencyTextBox1.RemoveDecimalZeros = true;
currencyTextBox1.CurrencySymbol = "₿";
currencyTextBox1.CurrencyDecimalDigits = 8;

currencyTextBox1.DecimalValue = 0.00001234m;
// Display: ₿0.00001234

currencyTextBox1.DecimalValue = 0.1m;
// Display: ₿0.1 (not 0.10000000)
```

```csharp
// Stock prices (varies by type)
currencyTextBox1.RemoveDecimalZeros = true;
currencyTextBox1.CurrencyDecimalDigits = 2;

currencyTextBox1.DecimalValue = 123m;
// Display: $123 (not $123.00)

currencyTextBox1.DecimalValue = 123.5m;
// Display: $123.5 (not $123.50)
```

## Currency Positive and Negative Patterns

### CurrencyPositivePattern Property

Determines symbol and number positioning for positive values:

```csharp
// Pattern 0: $1
currencyTextBox1.CurrencyPositivePattern = 0;
currencyTextBox1.DecimalValue = 1m;
// Display: $1.00

// Pattern 1: 1$ (symbol after)
currencyTextBox1.CurrencyPositivePattern = 1;
// Display: 1.00$

// Pattern 2: $ 1 (space before)
currencyTextBox1.CurrencyPositivePattern = 2;
// Display: $ 1.00

// Pattern 3: 1 $ (space after)
currencyTextBox1.CurrencyPositivePattern = 3;
// Display: 1.00 $
```

### CurrencyNegativePattern Property

Determines symbol and number positioning for negative values:

```csharp
// Pattern 0: -$1
currencyTextBox1.CurrencyNegativePattern = 0;
currencyTextBox1.DecimalValue = -1m;
// Display: -$1.00

// Pattern 1: -1$ (symbol after)
currencyTextBox1.CurrencyNegativePattern = 1;
// Display: -1.00$

// Pattern 2: $-1 (minus after symbol)
currencyTextBox1.CurrencyNegativePattern = 2;
// Display: $-1.00

// Pattern 3: $1- (minus after number)
currencyTextBox1.CurrencyNegativePattern = 3;
// Display: $1.00-

// Pattern 4: -1 $ (space variations)
currencyTextBox1.CurrencyNegativePattern = 4;
// Display: -1.00 $

// Pattern 5: -$ 1
currencyTextBox1.CurrencyNegativePattern = 5;
// Display: -$ 1.00

// Pattern 8: 1 $- (number space symbol minus)
currencyTextBox1.CurrencyNegativePattern = 8;
// Display: 1.00 $-

// Pattern 9: $ 1- (symbol space number minus)
currencyTextBox1.CurrencyNegativePattern = 9;
// Display: $ 1.00-

// Pattern 10: $ -1
currencyTextBox1.CurrencyNegativePattern = 10;
// Display: $ -1.00

// Pattern 11: 1 -$ (number space minus symbol)
currencyTextBox1.CurrencyNegativePattern = 11;
// Display: 1.00 -$

// Pattern 12: 1- $ (number minus space symbol)
currencyTextBox1.CurrencyNegativePattern = 12;
// Display: 1.00- $

// Pattern 13: ($ 1) (parentheses)
currencyTextBox1.CurrencyNegativePattern = 13;
// Display: ($ 1.00)

// Pattern 14: (1 $)
currencyTextBox1.CurrencyNegativePattern = 14;
// Display: (1.00 $)

// Pattern 15: ($ -1)
currencyTextBox1.CurrencyNegativePattern = 15;
// Display: ($ -1.00)
```

## Complete Formatting Examples

### Example 1: US Dollar Format
```csharp
currencyTextBox1.CurrencySymbol = "$";
currencyTextBox1.CurrencyGroupSeparator = ",";
currencyTextBox1.CurrencyDecimalSeparator = ".";
currencyTextBox1.CurrencyGroupSizes = new int[] { 3 };
currencyTextBox1.CurrencyDecimalDigits = 2;
currencyTextBox1.CurrencyPositivePattern = 0;  // $1
currencyTextBox1.CurrencyNegativePattern = 0;  // -$1

currencyTextBox1.DecimalValue = 1234567.89m;
// Display: $1,234,567.89
```

### Example 2: Euro Format
```csharp
currencyTextBox1.CurrencySymbol = "€";
currencyTextBox1.CurrencyGroupSeparator = ".";
currencyTextBox1.CurrencyDecimalSeparator = ",";
currencyTextBox1.CurrencyGroupSizes = new int[] { 3 };
currencyTextBox1.CurrencyDecimalDigits = 2;
currencyTextBox1.CurrencyPositivePattern = 2;  // € 1
currencyTextBox1.CurrencyNegativePattern = 2;  // €-1

currencyTextBox1.DecimalValue = 1234567.89m;
// Display: €1.234.567,89
```

### Example 3: Indian Rupee Format
```csharp
currencyTextBox1.CurrencySymbol = "₹";
currencyTextBox1.CurrencyGroupSeparator = ",";
currencyTextBox1.CurrencyDecimalSeparator = ".";
currencyTextBox1.CurrencyGroupSizes = new int[] { 3, 2 };
currencyTextBox1.CurrencyDecimalDigits = 2;
currencyTextBox1.CurrencyPositivePattern = 0;  // ₹1
currencyTextBox1.CurrencyNegativePattern = 13; // (₹ 1)

currencyTextBox1.DecimalValue = 1234567.89m;
// Display: ₹12,34,567.89
```
