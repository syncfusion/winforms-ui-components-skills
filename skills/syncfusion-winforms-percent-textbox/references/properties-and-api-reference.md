# Properties and API Reference for PercentTextBox

## Table of Contents
- [Value Properties](#value-properties)
- [Constraint Properties](#constraint-properties)
- [Formatting Properties](#formatting-properties)
- [Behavior Properties](#behavior-properties)
- [Text Properties](#text-properties)
- [Essential Methods](#essential-methods)
- [Quick Lookup by Use Case](#quick-lookup-by-use-case)

## Value Properties

### PercentValue

**Type:** `double`

Gets or sets the percentage value (0-100 range).

```csharp
// Set value
percentTextBox1.PercentValue = 50;  // 50%

// Get value
double value = percentTextBox1.PercentValue;  // Returns 50
```

**Use When:** Working directly with percentages, receiving 0-100 values

---

### DoubleValue

**Type:** `double`

Gets or sets the decimal/fractional value (0-1 range).

```csharp
// Set value
percentTextBox1.DoubleValue = 0.5;  // Represents 50%

// Get value
double value = percentTextBox1.DoubleValue;  // Returns 0.5
```

**Use When:** Converting to/from decimal format, performing calculations

---

### BindablePercentValue

**Type:** `double?` (nullable)

Gets or sets the percentage value for data binding. Allows null.

```csharp
// Set value
percentTextBox1.BindablePercentValue = 50;    // 50%
percentTextBox1.BindablePercentValue = null;  // Empty/null

// Get value
double? value = percentTextBox1.BindablePercentValue;
if (value.HasValue)
    Console.WriteLine(value.Value);
```

**Use When:** Data binding, supporting null values, nullable fields

---

### BindableValue

**Type:** `double?` (nullable)

Gets or sets the decimal value for data binding. Allows null (0-1 range).

```csharp
// Set value
percentTextBox1.BindableValue = 0.5;    // Represents 50%
percentTextBox1.BindableValue = null;   // Empty/null

// Get value
double? value = percentTextBox1.BindableValue;
if (value.HasValue)
    Console.WriteLine(value.Value);
```

**Use When:** Data binding with decimal/fractional rates, nullable fields

---

### DefaultValue

**Type:** `double`

Gets or sets the default value used for initialization.

```csharp
percentTextBox1.DefaultValue = 25;
// When control initializes, displays 25%
```

**Use When:** Providing initial default values for new controls

---

## Constraint Properties

### MinValue

**Type:** `double`

Gets or sets the minimum allowed percentage value.

```csharp
percentTextBox1.MinValue = 0;    // No negative percentages
percentTextBox1.MinValue = -100; // Allow down to -100%
```

**Related:** `EnforceMinMaxDuringValidating`, `ResetMinValue()`

---

### MaxValue

**Type:** `double`

Gets or sets the maximum allowed percentage value.

```csharp
percentTextBox1.MaxValue = 100;   // Maximum 100%
percentTextBox1.MaxValue = 1000;  // Allow up to 1000%
```

**Related:** `EnforceMinMaxDuringValidating`, `ResetMaxValue()`

---

### EnforceMinMaxDuringValidating

**Type:** `bool`

Controls whether min/max bounds are enforced during validation.

```csharp
percentTextBox1.EnforceMinMaxDuringValidating = true;   // Enforce bounds
percentTextBox1.EnforceMinMaxDuringValidating = false;  // Don't enforce
```

**Default:** `true`

**Behavior:**
- When `true`: User input outside min/max is rejected
- When `false`: User can enter any value

---

### AllowNull

**Type:** `bool`

Controls whether null/empty values are allowed.

```csharp
percentTextBox1.AllowNull = true;   // Allow empty
percentTextBox1.AllowNull = false;  // Require value
```

**Default:** `false`

**Related:** `NullString`, `NullFormat`

---

### NullString

**Type:** `string`

Gets or sets the text displayed when the control is null/empty.

```csharp
percentTextBox1.AllowNull = true;
percentTextBox1.NullString = "Not specified";
// When empty, displays "Not specified"
```

**Related:** `AllowNull`

---

### NullFormat

**Type:** `string`

Gets or sets the format string for null display.

```csharp
percentTextBox1.NullFormat = "{0}";
// Format string for null value display
```

---

## Formatting Properties

### PercentDecimalDigits

**Type:** `int`

Gets or sets the number of decimal places displayed.

```csharp
percentTextBox1.PercentDecimalDigits = 0;  // 50% (no decimals)
percentTextBox1.PercentDecimalDigits = 2;  // 50.00% (two decimals)
percentTextBox1.PercentDecimalDigits = 4;  // 50.0000% (four decimals)
```

---

### PercentDecimalSeparator

**Type:** `string`

Gets or sets the decimal separator character.

```csharp
percentTextBox1.PercentDecimalSeparator = ".";  // US: 50.50%
percentTextBox1.PercentDecimalSeparator = ",";  // Europe: 50,50%
```

---

### PercentGroupSeparator

**Type:** `string`

Gets or sets the thousands group separator.

```csharp
percentTextBox1.PercentGroupSeparator = ",";   // 1,234.50%
percentTextBox1.PercentGroupSeparator = ".";   // 1.234,50%
percentTextBox1.PercentGroupSeparator = " ";   // 1 234.50%
percentTextBox1.PercentGroupSeparator = "";    // 1234.50%
```

---

### PercentGroupSizes

**Type:** `int[]`

Gets or sets the digit grouping pattern.

```csharp
percentTextBox1.PercentGroupSizes = new int[] { 3 };        // 1,234%
percentTextBox1.PercentGroupSizes = new int[] { 2, 3 };     // 12,34,567% (Indian style)
percentTextBox1.PercentGroupSizes = new int[] { };          // No grouping
```

---

## Behavior Properties

### NegativeInputPendingOnSelectAll

**Type:** `bool`

Controls whether negative values can be entered via select-all + minus.

```csharp
percentTextBox1.NegativeInputPendingOnSelectAll = true;
// User can: Ctrl+A, type "-", enter negative value
```

**Default:** `false`

---

## Text Properties

### FormattedText

**Type:** `string` (read-only)

Gets the formatted text display including percent symbol and formatting.

```csharp
string formatted = percentTextBox1.FormattedText;
// Returns: "50.00%" (with formatting)
```

---

### ClipText

**Type:** `string` (read-only)

Gets the unformatted numeric text without percent symbol or separators.

```csharp
string unformatted = percentTextBox1.ClipText;
// Returns: "50" (without formatting)
```

---

### Text

**Type:** `string`

Gets or sets the control's text (use with caution).

```csharp
// Get text
string text = percentTextBox1.Text;

// Setting via properties is preferred over Text
percentTextBox1.PercentValue = 50;  // Recommended
// percentTextBox1.Text = "50%";    // Not recommended
```

---

## Essential Methods

### ResetMinValue()

Resets the `MinValue` property to its default value.

```csharp
percentTextBox1.MinValue = 10;
percentTextBox1.ResetMinValue();  // MinValue returns to default
```

---

### ResetMaxValue()

Resets the `MaxValue` property to its default value.

```csharp
percentTextBox1.MaxValue = 50;
percentTextBox1.ResetMaxValue();  // MaxValue returns to default
```

---

## Events Reference

| Event | Fires When | Use For |
|-------|-----------|---------|
| `BindablePercentValueChanged` | BindablePercentValue changes | Percent value binding/changes |
| `BindableValueChanged` | BindableValue changes | Decimal value binding/changes |
| `DoubleValueChanged` | DoubleValue changes | Decimal calculations |
| `FormattedTextChanged` | Formatted text changes | Display updates |
| `ValidationError` | Input validation fails | Error handling |
| `ClipTextChanged` | Unformatted text changes | Text-based processing |
| `SetNull` | Null state is set | Null handling logic |

---

## Quick Lookup by Use Case

### "I want to set/get a percentage (e.g., 50%)"

```csharp
// Set
percentTextBox1.PercentValue = 50;

// Get
double percent = percentTextBox1.PercentValue;
```

**Reference:** PercentValue

---

### "I want to set/get a decimal fraction (e.g., 0.5 for 50%)"

```csharp
// Set
percentTextBox1.DoubleValue = 0.5;

// Get
double fraction = percentTextBox1.DoubleValue;
```

**Reference:** DoubleValue

---

### "I want to bind to a data source"

```csharp
// For percent binding
control.DataBindings.Add("BindablePercentValue", datasource, "PercentField");

// For decimal binding
control.DataBindings.Add("BindableValue", datasource, "DecimalField");
```

**Reference:** BindablePercentValue, BindableValue

---

### "I want to limit values to 0-100"

```csharp
percentTextBox1.MinValue = 0;
percentTextBox1.MaxValue = 100;
percentTextBox1.EnforceMinMaxDuringValidating = true;
```

**Reference:** MinValue, MaxValue, EnforceMinMaxDuringValidating

---

### "I want to allow negative values"

```csharp
percentTextBox1.MinValue = -100;
percentTextBox1.MaxValue = 100;
percentTextBox1.NegativeInputPendingOnSelectAll = true;
```

**Reference:** NegativeInputPendingOnSelectAll

---

### "I want to allow empty/null values"

```csharp
percentTextBox1.AllowNull = true;
percentTextBox1.NullString = "Not set";
var value = percentTextBox1.BindablePercentValue;  // Could be null
```

**Reference:** AllowNull, NullString, BindablePercentValue

---

### "I want 2 decimal places displayed (e.g., 50.25%)"

```csharp
percentTextBox1.PercentDecimalDigits = 2;
```

**Reference:** PercentDecimalDigits

---

### "I want to format for a specific region"

```csharp
// US Style
percentTextBox1.PercentDecimalSeparator = ".";
percentTextBox1.PercentGroupSeparator = ",";

// European Style
percentTextBox1.PercentDecimalSeparator = ",";
percentTextBox1.PercentGroupSeparator = ".";
```

**Reference:** PercentDecimalSeparator, PercentGroupSeparator

---

### "I want to detect when the value changes"

```csharp
percentTextBox1.BindablePercentValueChanged += (s, e) =>
{
    Console.WriteLine($"Value: {percentTextBox1.BindablePercentValue}");
};
```

**Reference:** BindablePercentValueChanged event

---

### "I want to export the numeric value without formatting"

```csharp
string numericOnly = percentTextBox1.ClipText;
// e.g., "50" instead of "50.00%"
```

**Reference:** ClipText

---

### "I want to get the formatted display string"

```csharp
string formatted = percentTextBox1.FormattedText;
// e.g., "50.00%"
```

**Reference:** FormattedText

---

### "I want to handle validation errors"

```csharp
percentTextBox1.ValidationError += (s, e) =>
{
    Console.WriteLine($"Error: {e.ErrorMessage}");
};
```

**Reference:** ValidationError event

---

## Property Summary Table

| Property | Type | Default | Use Case |
|----------|------|---------|----------|
| PercentValue | double | 0 | Direct percentage access |
| DoubleValue | double | 0 | Decimal fraction access |
| BindablePercentValue | double? | null | Nullable percent binding |
| BindableValue | double? | null | Nullable decimal binding |
| DefaultValue | double | 0 | Initial default value |
| MinValue | double | -1e308 | Minimum bound |
| MaxValue | double | 1e308 | Maximum bound |
| EnforceMinMaxDuringValidating | bool | true | Enforce bounds |
| AllowNull | bool | false | Allow empty values |
| NullString | string | "" | Display text when null |
| PercentDecimalDigits | int | 2 | Decimal places |
| PercentDecimalSeparator | string | "." | Decimal char |
| PercentGroupSeparator | string | "," | Thousands char |
| NegativeInputPendingOnSelectAll | bool | false | Allow negative input |

---

**For more detailed examples, see:** [events-and-data-binding.md](events-and-data-binding.md), [value-management.md](value-management.md), [formatting-and-display.md](formatting-and-display.md)
