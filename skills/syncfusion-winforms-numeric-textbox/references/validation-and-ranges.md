# Validation and Value Ranges in SfNumericTextBox

## Table of Contents
- [Overview](#overview)
- [MinValue and MaxValue](#minvalue-and-maxvalue)
- [ValidationMode](#validationmode)
- [ValueChangeMode](#valuechangemode)
- [LostFocusValidation](#lostfocusvalidation)
- [Validation Flow](#validation-flow)
- [Practical Examples](#practical-examples)

## Overview

SfNumericTextBox provides comprehensive validation and value constraint mechanisms:

- **Value Ranges**: MinValue and MaxValue enforce valid range
- **Validation Timing**: KeyPress (immediate) or LostFocus (deferred)
- **Value Update Timing**: When the Value property actually changes
- **Error Recovery**: How invalid values are reset when validation fails

These properties work together to create flexible validation behavior suitable for different scenarios.

## MinValue and MaxValue

### Purpose

Define the acceptable range for numeric input:

```csharp
this.numericTextBox.MinValue = 10;
this.numericTextBox.MaxValue = 100;
```

### Characteristics

- Both are optional (can be null or not set)
- Applied during validation based on ValidationMode
- Only affect user input and loss-of-focus behavior
- Do NOT prevent programmatic Value assignment
- Specify double? type (nullable double)

### Behavior by ValidationMode

#### When ValidationMode = KeyPress

- Value is validated on every keystroke
- Invalid values cannot be entered
- User is prevented from typing values outside range

```csharp
this.numericTextBox.MinValue = 0;
this.numericTextBox.MaxValue = 100;
this.numericTextBox.ValidationMode = ValidationMode.KeyPress;

// User tries to type "150" - stops at "100"
// User tries to type "-5" - rejected
```

#### When ValidationMode = LostFocus

- Validation occurs only when control loses focus
- User can type any value while editing
- Invalid values are corrected on LostFocus based on LostFocusValidation setting

```csharp
this.numericTextBox.MinValue = 0;
this.numericTextBox.MaxValue = 100;
this.numericTextBox.ValidationMode = ValidationMode.LostFocus;

// User types "150" - allowed while editing
// On LostFocus - corrected based on LostFocusValidation
```

### Practical Examples

#### Age Range (0-150)

```csharp
this.numericTextBox.MinValue = 0;
this.numericTextBox.MaxValue = 150;
this.numericTextBox.WatermarkText = "Age (0-150)";
```

#### Percentage (0-100)

```csharp
this.numericTextBox.MinValue = 0;
this.numericTextBox.MaxValue = 100;
this.numericTextBox.Suffix = "%";
```

#### Price Range ($10-$1000)

```csharp
this.numericTextBox.FormatMode = FormatMode.Currency;
this.numericTextBox.MinValue = 10;
this.numericTextBox.MaxValue = 1000;
```

#### Temperature Range

```csharp
this.numericTextBox.MinValue = -50;     // Celsius
this.numericTextBox.MaxValue = 50;
this.numericTextBox.Suffix = "°C";
```

## ValidationMode

### Property

```csharp
this.numericTextBox.ValidationMode = Syncfusion.WinForms.Input.Enums.ValidationMode.KeyPress;
// or
this.numericTextBox.ValidationMode = Syncfusion.WinForms.Input.Enums.ValidationMode.LostFocus;
```

### Options

#### KeyPress Mode

**Validates on every keystroke**

```csharp
this.numericTextBox.ValidationMode = ValidationMode.KeyPress;
```

**Behavior:**
- Min/Max constraints are applied immediately during input
- Decimal mask is maintained while typing
- User cannot enter values outside the range
- Provides immediate feedback
- More restrictive, prevents invalid input early

**Use When:**
- You want to prevent invalid input completely
- Real-time feedback is important
- Strict validation is required
- Input field has limited characters

**Example:**
```csharp
this.numericTextBox.MinValue = 0;
this.numericTextBox.MaxValue = 9;      // Single digit
this.numericTextBox.ValidationMode = ValidationMode.KeyPress;
// User can only type 0-9, nothing else
```

#### LostFocus Mode (Default)

**Validates when control loses focus**

```csharp
this.numericTextBox.ValidationMode = ValidationMode.LostFocus;
```

**Behavior:**
- User can type any value while editing
- Validation occurs when Tab/Click away from control
- Invalid values are corrected based on LostFocusValidation
- Allows more flexibility during input
- Correction happens automatically

**Use When:**
- You want user flexibility during input
- Values will be corrected automatically
- Intermediate invalid states are acceptable
- Better for free-form number entry

**Example:**
```csharp
this.numericTextBox.MinValue = 100;
this.numericTextBox.MaxValue = 999;
this.numericTextBox.ValidationMode = ValidationMode.LostFocus;
// User can type "50" but it becomes "100" on LostFocus
```

## ValueChangeMode

### Property

```csharp
this.numericTextBox.ValueChangeMode = Syncfusion.WinForms.Input.Enums.ValueChangeMode.KeyPress;
// or
this.numericTextBox.ValueChangeMode = Syncfusion.WinForms.Input.Enums.ValueChangeMode.LostFocus;
```

### Purpose

Controls **when the Value property is updated**, independent of when validation occurs.

### Options

#### KeyPress Mode

```csharp
this.numericTextBox.ValueChangeMode = ValueChangeMode.KeyPress;
```

**Behavior:**
- Value property updates on every keystroke
- Updates only if validation passes
- Useful for real-time value tracking
- Event handlers fire frequently

**Use Case:**
```csharp
// Update calculations in real-time as user types
this.numericTextBox.ValueChangeMode = ValueChangeMode.KeyPress;
this.numericTextBox.ValueChanged += (s, e) => 
{
    // This fires on every keystroke
    double newPrice = e.NewValue.HasValue ? e.NewValue.Value * 1.1 : 0; // Add 10%
    totalLabel.Text = newPrice.ToString();
};
```

#### LostFocus Mode (Default)

```csharp
this.numericTextBox.ValueChangeMode = ValueChangeMode.LostFocus;
```

**Behavior:**
- Value property updates only when control loses focus
- Waits until editing is complete
- Fewer event notifications
- Good for batch updates

**Use Case:**
```csharp
// Update value only when user is done editing
this.numericTextBox.ValueChangeMode = ValueChangeMode.LostFocus;
this.numericTextBox.ValueChanged += (s, e) => 
{
    // This fires only after user leaves the field
    SaveToDatabase(e.NewValue);
};
```

### Key Differences: ValidationMode vs ValueChangeMode

| Aspect | ValidationMode | ValueChangeMode |
|--------|---|---|
| **Controls** | When validation happens | When Value updates |
| **KeyPress** | Prevents invalid input | Updates Value on keystroke |
| **LostFocus** | Validates on blur | Updates Value on blur |
| **Independent** | Can be set independently | Different from ValidationMode |

### Combined Behavior

```csharp
// SCENARIO 1: Real-time everything
this.numericTextBox.ValidationMode = ValidationMode.KeyPress;
this.numericTextBox.ValueChangeMode = ValueChangeMode.KeyPress;
// Validates immediately AND updates Value immediately

// SCENARIO 2: Deferred everything
this.numericTextBox.ValidationMode = ValidationMode.LostFocus;
this.numericTextBox.ValueChangeMode = ValueChangeMode.LostFocus;
// Validates on blur AND updates Value on blur

// SCENARIO 3: Mixed (validate strict, update deferred)
this.numericTextBox.ValidationMode = ValidationMode.KeyPress;
this.numericTextBox.ValueChangeMode = ValueChangeMode.LostFocus;
// Prevents invalid input, but Value only updates on blur
```

## LostFocusValidation

### Purpose

When ValidationMode is LostFocus and user enters an invalid value, this property determines how to recover.

### Property

```csharp
this.numericTextBox.LostFocusValidation = Syncfusion.WinForms.Input.Enums.ValidationResetOption.Reset;
```

### Options

#### Reset Option

```csharp
this.numericTextBox.LostFocusValidation = ValidationResetOption.Reset;
```

**Behavior:**
- Invalid value reverts to the last valid value
- Useful for "undo" semantics
- Preserves user's previous valid entry
- Most user-friendly in many cases

**Example:**
```csharp
this.numericTextBox.MinValue = 0;
this.numericTextBox.MaxValue = 100;
this.numericTextBox.LostFocusValidation = ValidationResetOption.Reset;
this.numericTextBox.Value = 50;  // Last valid value

// User types "150" (invalid)
// On LostFocus -> Value reverts to 50
```

#### MaxValue Option

```csharp
this.numericTextBox.LostFocusValidation = ValidationResetOption.MaxValue;
```

**Behavior:**
- Invalid value is reset to MaxValue
- Clamps value to maximum
- Useful for "cap at maximum" semantics
- More aggressive correction

**Example:**
```csharp
this.numericTextBox.MinValue = 0;
this.numericTextBox.MaxValue = 100;
this.numericTextBox.LostFocusValidation = ValidationResetOption.MaxValue;
this.numericTextBox.Value = 50;

// User types "150" (invalid)
// On LostFocus -> Value becomes 100
```

#### MinValue Option

```csharp
this.numericTextBox.LostFocusValidation = ValidationResetOption.MinValue;
```

**Behavior:**
- Invalid value is reset to MinValue
- Clamps value to minimum
- Useful for "default to minimum" semantics
- Ensures minimum acceptable value

**Example:**
```csharp
this.numericTextBox.MinValue = 1;
this.numericTextBox.MaxValue = 1000;
this.numericTextBox.LostFocusValidation = ValidationResetOption.MinValue;

// User tries to enter 0 (below minimum)
// On LostFocus -> Value becomes 1
```

### Choosing the Right Option

| Scenario | Option | Reason |
|----------|--------|--------|
| **User error correction** | Reset | Restore user's last valid entry |
| **Price capped at max** | MaxValue | Cannot exceed budget limit |
| **Minimum required** | MinValue | Ensure minimum quantity |
| **Default to safe value** | Reset or MaxValue | Prevents edge cases |

## Validation Flow

### Typical Validation Flow (LostFocus Mode)

```
User Input → [Control has focus]
     ↓
Control loses focus (Tab/Click)
     ↓
ValidationMode = LostFocus: Validation occurs
     ↓
IF valid:
  - Value updates (if ValueChangeMode = LostFocus)
  - ValueChanged event fires
  - Success
ELSE (invalid):
  - LostFocusValidation applies
  - Value is corrected to Reset/MaxValue/MinValue
  - ValueChanged event fires with corrected value
```

### KeyPress Mode Flow

```
User types character
     ↓
ValidationMode = KeyPress: Validate immediately
     ↓
IF valid AND passes MinValue/MaxValue:
  - Character is accepted
  - Value updates (if ValueChangeMode = KeyPress)
  - ValueChanged event fires
ELSE:
  - Character is rejected (not entered)
  - No value change
```

## Practical Examples

### Example 1: Strict Age Validation

```csharp
// User cannot enter invalid age values at all
this.numericTextBox.MinValue = 0;
this.numericTextBox.MaxValue = 150;
this.numericTextBox.ValidationMode = ValidationMode.KeyPress;  // Strict
this.numericTextBox.ValueChangeMode = ValueChangeMode.KeyPress;
this.numericTextBox.WatermarkText = "Age (0-150)";

// User sees immediate feedback
// Can only type valid values
```

### Example 2: Flexible Currency Input

```csharp
// User has flexibility, errors corrected automatically
this.numericTextBox.FormatMode = FormatMode.Currency;
this.numericTextBox.MinValue = 0;
this.numericTextBox.MaxValue = 99999.99;
this.numericTextBox.ValidationMode = ValidationMode.LostFocus;    // Flexible
this.numericTextBox.ValueChangeMode = ValueChangeMode.LostFocus;
this.numericTextBox.LostFocusValidation = ValidationResetOption.Reset;

// User can type freely
// Invalid values corrected on LostFocus
```

### Example 3: Quantity with Minimum Enforcement

```csharp
// Quantity must be at least 1
this.numericTextBox.MinValue = 1;
this.numericTextBox.MaxValue = 999;
this.numericTextBox.ValidationMode = ValidationMode.LostFocus;
this.numericTextBox.LostFocusValidation = ValidationResetOption.MinValue;
this.numericTextBox.WatermarkText = "Qty (min 1)";

// If user enters 0, it becomes 1
// Ensures minimum quantity is always maintained
```

### Example 4: Real-time Percentage Validation

```csharp
// Percentage 0-100 with real-time validation
this.numericTextBox.MinValue = 0;
this.numericTextBox.MaxValue = 100;
this.numericTextBox.ValidationMode = ValidationMode.KeyPress;
this.numericTextBox.ValueChangeMode = ValueChangeMode.KeyPress;
this.numericTextBox.Suffix = "%";

this.numericTextBox.ValueChanged += (sender, e) =>
{
    // Update progress bar in real-time
    progressBar.Value = (int)e.NewValue.GetValueOrDefault(0);
};
```

### Example 5: Temperature Range with Correction

```csharp
// Temperature -50 to 50 Celsius, default to 0
this.numericTextBox.MinValue = -50;
this.numericTextBox.MaxValue = 50;
this.numericTextBox.ValidationMode = ValidationMode.LostFocus;
this.numericTextBox.LostFocusValidation = ValidationResetOption.Reset;  // Or MinValue
this.numericTextBox.Suffix = "°C";
this.numericTextBox.Value = 20;

// User can type any value while focused
// Invalid values corrected on blur
```

## Important Notes

- **Programmatic Assignment**: Setting Value property directly bypasses validation
- **Full Precision**: Validation doesn't affect the precision stored in Value
- **Default Behavior**: Default ValidationMode is LostFocus, default ValueChangeMode is LostFocus
- **Performance**: KeyPress mode with complex calculations can impact responsiveness
- **User Experience**: Choose modes based on use case (strict control vs flexible input)
