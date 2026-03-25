# Text Field Customization

## Table of Contents
- [Text Display and Alignment](#text-display-and-alignment)
- [Multiline Support](#multiline-support)
- [Password Character Masking](#password-character-masking)
- [Banner Text for Empty State](#banner-text-for-empty-state)
- [Number and Decimal Formatting](#number-and-decimal-formatting)
- [Negative Value Handling](#negative-value-handling)
- [Null String Configuration](#null-string-configuration)

## Text Display and Alignment

### Default Text

The `Text` property contains the formatted display of the currency value:

```csharp
// Display shows: $25.00
currencyTextBox1.Text = "$25.00";

// Access the text (formatted)
string displayedText = currencyTextBox1.Text;  // Returns: "$25.00"

// Get the underlying value (unformatted)
decimal numericValue = currencyTextBox1.DecimalValue;  // Returns: 25.00m
```

**Important:** The Text property is read-only after initialization. Use `DecimalValue` to change the amount.

### Text Alignment

Position the text within the control using `TextAlign`:

```csharp
// Right-aligned (default for currency)
currencyTextBox1.TextAlign = System.Windows.Forms.HorizontalAlignment.Right;

// Left-aligned
currencyTextBox1.TextAlign = System.Windows.Forms.HorizontalAlignment.Left;

// Center-aligned
currencyTextBox1.TextAlign = System.Windows.Forms.HorizontalAlignment.Center;
```

**Recommendation:** Use `Right` alignment for numeric values (financial convention).

## Multiline Support

### Enabling Multiline Display

For very large numbers or multi-currency display:

```csharp
currencyTextBox1.Multiline = true;
currencyTextBox1.Height = 50;  // Increase height for multiple lines
```

### Configuring Multiline Behavior

```csharp
currencyTextBox1.Multiline = true;

// Enable word wrapping
currencyTextBox1.WordWrap = true;

// Show scrollbars
currencyTextBox1.ScrollBars = System.Windows.Forms.ScrollBars.Both;

// Or specific scrollbars
currencyTextBox1.ScrollBars = System.Windows.Forms.ScrollBars.Vertical;  // Vertical only
currencyTextBox1.ScrollBars = System.Windows.Forms.ScrollBars.Horizontal;  // Horizontal only
currencyTextBox1.ScrollBars = System.Windows.Forms.ScrollBars.None;  // No scrollbars
```

### Multiline Text Example

```csharp
currencyTextBox1.Multiline = true;
currencyTextBox1.Size = new System.Drawing.Size(300, 80);
currencyTextBox1.ScrollBars = System.Windows.Forms.ScrollBars.Vertical;

// Large number that wraps
currencyTextBox1.DecimalValue = 12456456456456456m;
// Display: $12,456,456,456,456,456.00 (formatted across multiple lines)
```

## Password Character Masking

Hide currency values for privacy (e.g., payment entry on shared screen):

### System Password Character

```csharp
// Use system default (usually •)
currencyTextBox1.UseSystemPasswordChar = true;
```

### Custom Password Character

```csharp
// Use asterisk
currencyTextBox1.PasswordChar = '*';
currencyTextBox1.UseSystemPasswordChar = false;

// Use any character
currencyTextBox1.PasswordChar = '◆';

// Use dot
currencyTextBox1.PasswordChar = '•';
```

### Complete Masking Example

```csharp
// For sensitive payment entry
currencyTextBox1.PasswordChar = '*';
currencyTextBox1.UseSystemPasswordChar = false;
currencyTextBox1.CurrencySymbol = "$";
currencyTextBox1.DecimalValue = 999.99m;

// Display: $****.** (all digits masked)
```

**Security Note:** PasswordChar masks display only. The actual DecimalValue is still programmatically accessible.

## Banner Text for Empty State

Display placeholder text when no value is entered:

### Setup Banner Text

```csharp
// Allow null/empty values
currencyTextBox1.AllowNull = true;

// Set display text when empty
currencyTextBox1.NullString = "";  // Empty string

// Initial state
currencyTextBox1.Text = "";

// User sees nothing until they start typing
```

### Banner Text with BannerTextProvider

```csharp
// Import the component
using Syncfusion.Windows.Forms.Tools;

// Configure control for banner support
currencyTextBox1.AllowNull = true;
currencyTextBox1.NullString = "";
currencyTextBox1.Text = "";

// In designer or code, add BannerTextProvider
BannerTextProvider bannerProvider = new BannerTextProvider();
bannerProvider.SetBannerText(currencyTextBox1, "Enter amount...");
```

### Banner Text Properties

```csharp
// These settings enable banner text capability
currencyTextBox1.AllowNull = true;           // Allow empty state
currencyTextBox1.NullString = "";            // Empty display
currencyTextBox1.Text = "";                  // Initial empty text
```

## Number and Decimal Formatting

### Decimal Digits (Precision)

Control how many digits appear after the decimal point:

```csharp
// Two decimal places (default for currency)
currencyTextBox1.CurrencyDecimalDigits = 2;
// Display: $123.45

// Three decimal places
currencyTextBox1.CurrencyDecimalDigits = 3;
// Display: $123.456

// No decimal places
currencyTextBox1.CurrencyDecimalDigits = 0;
// Display: $123
```

### Decimal Separator

Character used to separate whole and fractional parts:

```csharp
// Period (US convention)
currencyTextBox1.CurrencyDecimalSeparator = ".";
// Display: $123.45

// Comma (European convention)
currencyTextBox1.CurrencyDecimalSeparator = ",";
// Display: $123,45

// Other separators
currencyTextBox1.CurrencyDecimalSeparator = "·";
```

### Group Separator (Thousands Separator)

Character used to separate thousands:

```csharp
// Comma (US/English convention)
currencyTextBox1.CurrencyGroupSeparator = ",";
// Display: $1,234,567.89

// Period (European convention)
currencyTextBox1.CurrencyGroupSeparator = ".";
// Display: $1.234.567,89

// Space
currencyTextBox1.CurrencyGroupSeparator = " ";
// Display: $1 234 567.89

// No separator
currencyTextBox1.CurrencyGroupSeparator = "";
// Display: $1234567.89
```

### Group Sizes (Grouping Pattern)

How many digits between separators:

```csharp
// Standard: 3 digits per group (1,234,567)
currencyTextBox1.CurrencyGroupSizes = new int[] { 3 };

// Indian numbering: groups of 2 after first 3 (12,34,567)
currencyTextBox1.CurrencyGroupSizes = new int[] { 3, 2 };

// Different grouping
currencyTextBox1.CurrencyGroupSizes = new int[] { 4 };
// Display: $1234,5678,9012
```

### Remove Decimal Zeros

Strip trailing zeros from decimal portion:

```csharp
// Keep trailing zeros (default)
currencyTextBox1.RemoveDecimalZeros = false;
// Display: $100.00, $50.00, $25.50

// Remove trailing zeros
currencyTextBox1.RemoveDecimalZeros = true;
// Display: $100, $50, $25.5
```

### Complete Formatting Example

```csharp
// US format: $1,234.56
currencyTextBox1.CurrencySymbol = "$";
currencyTextBox1.CurrencyGroupSeparator = ",";
currencyTextBox1.CurrencyDecimalSeparator = ".";
currencyTextBox1.CurrencyGroupSizes = new int[] { 3 };
currencyTextBox1.CurrencyDecimalDigits = 2;
currencyTextBox1.DecimalValue = 1234.56m;
// Result: $1,234.56

// European format: €1.234,56
currencyTextBox1.CurrencySymbol = "€";
currencyTextBox1.CurrencyGroupSeparator = ".";
currencyTextBox1.CurrencyDecimalSeparator = ",";
currencyTextBox1.CurrencyGroupSizes = new int[] { 3 };
currencyTextBox1.CurrencyDecimalDigits = 2;
currencyTextBox1.DecimalValue = 1234.56m;
// Result: €1.234,56

// Indian format: ₹12,34,567.89
currencyTextBox1.CurrencySymbol = "₹";
currencyTextBox1.CurrencyGroupSeparator = ",";
currencyTextBox1.CurrencyDecimalSeparator = ".";
currencyTextBox1.CurrencyGroupSizes = new int[] { 3, 2 };
currencyTextBox1.CurrencyDecimalDigits = 2;
currencyTextBox1.DecimalValue = 1234567.89m;
// Result: ₹12,34,567.89
```

## Negative Value Handling

### Negative Sign Character

Replace the default minus sign with a custom character:

```csharp
// Default minus sign
currencyTextBox1.NegativeSign = "-";
// Display: -$100.00

// Parentheses (accounting format)
currencyTextBox1.NegativeSign = "(";  // Displayed as negative context
// Display: ($100.00)

// Custom character
currencyTextBox1.NegativeSign = "−";  // Unicode minus
```

### Negative Behavior with Selection

When user selects all text and presses negative key:

```csharp
// Immediate negation of current value
currencyTextBox1.NegativeInputPendingOnSelectAll = false;
// If value is 100 and user presses minus, result: -100

// Pending mode (replacement)
currencyTextBox1.NegativeInputPendingOnSelectAll = true;
// If value is 100, all text selected, user types minus then 50, result: -50
```

### Example: Profit/Loss Entry

```csharp
// Support both positive and negative
currencyTextBox1.MaxValue = decimal.MaxValue;
currencyTextBox1.MinValue = decimal.MinValue;
currencyTextBox1.NegativeSign = "-";
currencyTextBox1.NegativeInputPendingOnSelectAll = true;
currencyTextBox1.PositiveColor = System.Drawing.Color.Green;
currencyTextBox1.NegativeColor = System.Drawing.Color.Red;
currencyTextBox1.DecimalValue = 0m;
// User can enter positive profits or negative losses
```

## Null String Configuration

Display custom text when value is null/empty:

```csharp
// Allow null values
currencyTextBox1.AllowNull = true;

// Display "N/A" when null
currencyTextBox1.NullString = "N/A";

// Start empty
currencyTextBox1.Text = "";

// User sees: N/A (until they enter a value)
```

### Optional Amount with Placeholder

```csharp
currencyTextBox1.AllowNull = true;
currencyTextBox1.NullString = "(Optional)";
currencyTextBox1.Text = "";
currencyTextBox1.DecimalValue = 0m;

// When user leaves empty, displays: (Optional)
// When user enters amount, displays: $100.00
```

### Null String Example: Budget Application

```csharp
// For optional budget line items
currencyTextBox1.AllowNull = true;
currencyTextBox1.NullString = "—";  // Em dash for empty
currencyTextBox1.CurrencySymbol = "$";
currencyTextBox1.CurrencyDecimalDigits = 2;
currencyTextBox1.MinValue = 0m;

// Empty display: —
// After entering: $5,000.00
```

## Combining Multiple Customizations

Complete text field customization example:

```csharp
// Multi-currency format with all customizations
currencyTextBox1.CurrencySymbol = "€";
currencyTextBox1.CurrencyGroupSeparator = ".";
currencyTextBox1.CurrencyDecimalSeparator = ",";
currencyTextBox1.CurrencyGroupSizes = new int[] { 3 };
currencyTextBox1.CurrencyDecimalDigits = 2;
currencyTextBox1.TextAlign = System.Windows.Forms.HorizontalAlignment.Right;
currencyTextBox1.NegativeSign = "-";
currencyTextBox1.RemoveDecimalZeros = false;
currencyTextBox1.AllowNull = false;
currencyTextBox1.MaxValue = 999999999.99m;
currencyTextBox1.MinValue = -999999999.99m;
currencyTextBox1.DecimalValue = 0m;

// User enters: 1234.56
// Display: €1.234,56
```
