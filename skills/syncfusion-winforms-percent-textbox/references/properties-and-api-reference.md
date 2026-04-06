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

### PercentValue / DoubleValue

```csharp
// Percentage (0-100 range)
percentTextBox1.PercentValue = 50;              // Set 50%
double percent = percentTextBox1.PercentValue;  // Get 50

// Decimal/fractional (0-1 range)
percentTextBox1.DoubleValue = 0.5;              // Set 50%
double fraction = percentTextBox1.DoubleValue;  // Get 0.5
```

### Bindable Properties (nullable)

```csharp
// BindablePercentValue (nullable percentage)
percentTextBox1.BindablePercentValue = 50;    // Set 50%
percentTextBox1.BindablePercentValue = null;  // Set null
double? value1 = percentTextBox1.BindablePercentValue;

// BindableValue (nullable decimal)
percentTextBox1.BindableValue = 0.5;          // Set 50%
percentTextBox1.BindableValue = null;         // Set null
double? value2 = percentTextBox1.BindableValue;
```

### DefaultValue

```csharp
percentTextBox1.DefaultValue = 25;  // Initial value: 25%
```

---

## Constraint Properties

### MinValue / MaxValue

```csharp
percentTextBox1.MinValue = 0;      // Minimum: 0%
percentTextBox1.MaxValue = 100;    // Maximum: 100%
percentTextBox1.MinValue = -100;   // Allow negative: -100%
percentTextBox1.EnforceMinMaxDuringValidating = true;  // Enforce bounds (default: true)
```

**Methods:** `ResetMinValue()`, `ResetMaxValue()`

### Null Handling

```csharp
percentTextBox1.AllowNull = true;              // Allow empty/null (default: false)
percentTextBox1.NullString = "Not specified";  // Display text when null
percentTextBox1.NullFormat = "{0}";            // Format for null display
```

---

## Formatting Properties

### Formatting Properties

```csharp
// Decimal places
percentTextBox1.PercentDecimalDigits = 2;      // Display: 50.00%

// Separators
percentTextBox1.PercentDecimalSeparator = "."; // US: 50.50%
percentTextBox1.PercentGroupSeparator = ",";   // 1,234.50%

// Grouping pattern
percentTextBox1.PercentGroupSizes = new int[] { 3 };     // 1,234%
percentTextBox1.PercentGroupSizes = new int[] { 2, 3 };  // 12,34,567% (Indian)
```

---

## Behavior Properties

### NegativeInputPendingOnSelectAll

```csharp
percentTextBox1.NegativeInputPendingOnSelectAll = true;  // Allow Ctrl+A, "-" for negative (default: false)
```

---

## Text Properties

```csharp
// Formatted text (read-only)
string formatted = percentTextBox1.FormattedText;  // Returns: "50.00%"

// Unformatted numeric text (read-only)
string unformatted = percentTextBox1.ClipText;     // Returns: "50"

// Text property (use value properties instead)
percentTextBox1.PercentValue = 50;  // Recommended
// percentTextBox1.Text = "50%";    // Not recommended
```

---

## Essential Methods

```csharp
percentTextBox1.ResetMinValue();  // Reset MinValue to default
percentTextBox1.ResetMaxValue();  // Reset MaxValue to default
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

| Use Case | Code |
|----------|------|
| Set/get percentage | `percentTextBox1.PercentValue = 50;` |
| Set/get decimal | `percentTextBox1.DoubleValue = 0.5;` |
| Data binding | `control.DataBindings.Add("BindablePercentValue", ds, "Field");` |
| Limit to 0-100 | `MinValue = 0; MaxValue = 100; EnforceMinMaxDuringValidating = true;` |
| Allow negative | `MinValue = -100; NegativeInputPendingOnSelectAll = true;` |
| Allow null | `AllowNull = true; NullString = "Not set";` |
| Decimal places | `PercentDecimalDigits = 2;` |
| Regional format | `PercentDecimalSeparator = "."; PercentGroupSeparator = ",";` |
| Value changed event | `BindablePercentValueChanged += (s,e) => { };` |
| Get unformatted text | `string text = percentTextBox1.ClipText;` |
| Get formatted text | `string text = percentTextBox1.FormattedText;` |
| Validation errors | `ValidationError += (s,e) => { };` |

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
