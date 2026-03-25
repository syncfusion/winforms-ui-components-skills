# Behavior and Input in PercentTextBox

## Table of Contents
- [Negative Input Handling](#negative-input-handling)
- [Text Manipulation](#text-manipulation)
- [ClipText Property](#cliptext-property)
- [User Input Processing](#user-input-processing)
- [Input Behavior Examples](#input-behavior-examples)

## Negative Input Handling

### NegativeInputPendingOnSelectAll Property

Controls whether negative values can be entered by selecting all text and typing a minus sign.

```csharp
// Enable negative input on select-all
percentTextBox1.NegativeInputPendingOnSelectAll = true;

// Now user can:
// 1. Select all (Ctrl+A)
// 2. Type minus sign (-)
// 3. Enter negative value
```

### Default Behavior

When `NegativeInputPendingOnSelectAll = false` (default):

```csharp
percentTextBox1.PercentValue = 50;
percentTextBox1.NegativeInputPendingOnSelectAll = false;

// User selects all and types "-"
// Result: Still 50% (negative input ignored)
```

### Enabled Behavior

When `NegativeInputPendingOnSelectAll = true`:

```csharp
percentTextBox1.PercentValue = 50;
percentTextBox1.NegativeInputPendingOnSelectAll = true;

// User selects all and types "-"
// Then types "25"
// Result: -25% (negative value accepted)
```

### Practical Example: Gain/Loss Percentage

```csharp
private void SetupGainLossField(PercentTextBox control)
{
    control.MinValue = -100;  // Allow up to -100% loss
    control.MaxValue = 100;   // Allow up to +100% gain
    control.NegativeInputPendingOnSelectAll = true;
    control.PercentDecimalDigits = 2;
}

// Usage
SetupGainLossField(gainLossBox);

// User can now:
// - Enter 25 for +25% gain
// - Enter -15 for 15% loss
```

### Workflow for Entering Negative

```csharp
// Enable this feature first
percentTextBox1.NegativeInputPendingOnSelectAll = true;
percentTextBox1.MinValue = -100;

// User interaction:
// Step 1: User presses Ctrl+A (select all)
// Step 2: User types "-"
// Step 3: User types "50"
// Result: -50 (if PercentValue was 50, becomes -50)
```

## Text Manipulation

### Accessing Text in Control

```csharp
// Get the raw text (formatted with percent symbol)
string displayText = percentTextBox1.Text;
// Example: "50%"

// Get unformatted text (just the number)
string clipText = percentTextBox1.ClipText;
// Example: "50"
```

### Clearing the Control

```csharp
// If AllowNull = true
percentTextBox1.BindablePercentValue = null;
// Control shows NullString

// If AllowNull = false
percentTextBox1.PercentValue = 0;  // Reset to zero
```

### Setting Text Programmatically

```csharp
// Set via PercentValue property (recommended)
percentTextBox1.PercentValue = 75;
// Control displays: "75%"

// Do NOT set via Text property directly
// percentTextBox1.Text = "75%";  // Not recommended - may cause formatting issues
```

## ClipText Property

### What is ClipText?

The `ClipText` property returns the text without formatting (no percent symbol or separators).

```csharp
percentTextBox1.PercentValue = 1234.56;
percentTextBox1.PercentDecimalDigits = 2;
percentTextBox1.PercentGroupSeparator = ",";

// Formatted text with percent symbol
string formatted = percentTextBox1.Text;
// Result: "1,234.56%"

// Unformatted numeric text
string unformatted = percentTextBox1.ClipText;
// Result: "1234.56"
```

### Use Cases for ClipText

```csharp
// Case 1: Export to CSV (numeric only)
string csvValue = percentTextBox1.ClipText;  // "50"
csvLine += csvValue;

// Case 2: Send to database
string dbValue = percentTextBox1.ClipText;  // "50"
database.SavePercent(dbValue);

// Case 3: Parse for calculation
if (double.TryParse(percentTextBox1.ClipText, out double value))
{
    double result = value * 2;
}
```

### ClipTextChanged Event

Fires when the unformatted text changes.

```csharp
percentTextBox1.ClipTextChanged += (sender, e) =>
{
    Console.WriteLine($"Unformatted text changed: {percentTextBox1.ClipText}");
};
```

## User Input Processing

### How User Input Flows

```
User Types → Validation → Formatting → Display
    ↓           ↓            ↓            ↓
   "50"     Valid?         "50%"      Shows "50%"
           (within bounds)
```

### Character-by-Character Input

PercentTextBox validates as the user types:

```csharp
percentTextBox1.MaxValue = 100;
percentTextBox1.PercentDecimalDigits = 2;

// User types: "1"
// Accepted, displays "1%"

// User types: "15"
// Accepted, displays "15%"

// User types: "150"
// Rejected (exceeds 100), stays at "15%"
```

### Input Rejection Rules

```csharp
percentTextBox1.MinValue = 0;
percentTextBox1.MaxValue = 100;
percentTextBox1.EnforceMinMaxDuringValidating = true;

// Valid inputs:
// 0, 50, 99.99, 100

// Invalid inputs:
// -1 (below min), 101 (above max), "abc" (not numeric)
```

### Example: Step-by-Step Input

```csharp
var control = new PercentTextBox();
control.PercentDecimalDigits = 2;
control.MinValue = 0;
control.MaxValue = 100;

// User action: Type "3"
// Internal state: PercentValue = 3, Display = "3.00%"

// User action: Type "5"
// Internal state: PercentValue = 35, Display = "35.00%"

// User action: Type "0"
// Internal state: PercentValue = 350, Display would be "350.00%"
// BUT: 350 > MaxValue (100), so rejected
// Result: Stays at 35, Display = "35.00%"
```

## Input Behavior Examples

### Example 1: Required Field (No Null)

```csharp
// Setup
var control = new PercentTextBox();
control.AllowNull = false;
control.DefaultValue = 0;
control.MinValue = 0;
control.MaxValue = 100;

// Behavior:
// - User cannot clear the field (always has a value)
// - Invalid inputs are rejected
// - Min/max bounds enforced
```

### Example 2: Optional Field with Null Support

```csharp
// Setup
var control = new PercentTextBox();
control.AllowNull = true;
control.NullString = "Not specified";
control.MinValue = 0;
control.MaxValue = 100;

// Behavior:
// - User can leave empty (shows "Not specified")
// - User can enter 0-100
// - BindablePercentValue returns null when empty
```

### Example 3: Accept Negative for Changes

```csharp
// Setup for percentage change (e.g., -15% for decrease, +20% for increase)
var control = new PercentTextBox();
control.MinValue = -100;
control.MaxValue = 500;
control.NegativeInputPendingOnSelectAll = true;
control.PercentDecimalDigits = 2;

// Behavior:
// - User can enter negative values for decreases
// - User can enter positive values for increases
// - Range allows realistic change percentages
```

### Example 4: High-Precision Input

```csharp
// Setup for scientific/technical percentages
var control = new PercentTextBox();
control.PercentDecimalDigits = 5;
control.MinValue = 0;
control.MaxValue = 100;
control.PercentGroupSeparator = "";  // No thousands separator

// Behavior:
// - Accepts 99.12345%
// - No thousands separator (cleaner for small values)
// - High precision maintained
```

### Example 5: Form with Multiple Related Fields

```csharp
public class AllocationForm
{
    private PercentTextBox categoryA, categoryB, categoryC;

    public AllocationForm()
    {
        // Each field accepts 0-100
        // Total should equal 100 (validate at form level)
        
        SetupField(categoryA, "Category A");
        SetupField(categoryB, "Category B");
        SetupField(categoryC, "Category C");
    }

    private void SetupField(PercentTextBox control, string label)
    {
        control.MinValue = 0;
        control.MaxValue = 100;
        control.PercentDecimalDigits = 1;
        
        control.BindablePercentValueChanged += (s, e) =>
        {
            ValidateTotalAllocation();
        };
    }

    private void ValidateTotalAllocation()
    {
        double total = (categoryA.PercentValue ?? 0) +
                      (categoryB.PercentValue ?? 0) +
                      (categoryC.PercentValue ?? 0);

        if (total != 100)
        {
            MessageBox.Show($"Total allocation: {total}% (should be 100%)");
        }
    }
}
```

---

**Next:** Learn event handling in [events-and-data-binding.md](events-and-data-binding.md) or see the API reference in [properties-and-api-reference.md](properties-and-api-reference.md)
