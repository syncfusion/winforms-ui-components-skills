# Constraints and Validation in PercentTextBox

## Table of Contents
- [Min/Max Value Constraints](#minmax-value-constraints)
- [Enforcing Constraints During Validation](#enforcing-constraints-during-validation)
- [Null Value Handling](#null-value-handling)
- [Validation Error Handling](#validation-error-handling)
- [Validation Patterns](#validation-patterns)
- [Edge Cases and Troubleshooting](#edge-cases-and-troubleshooting)

## Min/Max Value Constraints

### Setting Bounds

The `MinValue` and `MaxValue` properties define the acceptable range for the control.

```csharp
// Allow percentages from 0 to 100
percentTextBox1.MinValue = 0;
percentTextBox1.MaxValue = 100;

// Allow negative percentages (-50 to 50)
percentTextBox1.MinValue = -50;
percentTextBox1.MaxValue = 50;

// Allow wide range
percentTextBox1.MinValue = -999;
percentTextBox1.MaxValue = 999;
```

### Getting Min/Max Values

```csharp
double minBound = percentTextBox1.MinValue;
double maxBound = percentTextBox1.MaxValue;

Console.WriteLine($"Range: {minBound}% to {maxBound}%");
```

### Resetting to Defaults

```csharp
// Reset MinValue to default
percentTextBox1.ResetMinValue();

// Reset MaxValue to default
percentTextBox1.ResetMaxValue();

// After reset, MinValue and MaxValue become their default values
```

## Enforcing Constraints During Validation

### EnforceMinMaxDuringValidating Property

When enabled, the control enforces min/max bounds during validation.

```csharp
// Enable enforcement
percentTextBox1.EnforceMinMaxDuringValidating = true;
percentTextBox1.MinValue = 0;
percentTextBox1.MaxValue = 100;

// Now:
// - Input below 0 → rejected or clamped
// - Input above 100 → rejected or clamped
```

### Behavior with EnforceMinMaxDuringValidating

When `EnforceMinMaxDuringValidating = true`:
- User tries to enter 150% (above max 100)
- Validation rejects the input
- ValidationError event fires
- Control retains previous valid value

```csharp
percentTextBox1.EnforceMinMaxDuringValidating = true;
percentTextBox1.MaxValue = 100;

// User attempts to type 150
percentTextBox1.ValidationError += (sender, e) =>
{
    Console.WriteLine($"Invalid: {e.ErrorMessage}");
    Console.WriteLine($"Invalid text: {e.InvalidText}");
};
```

### Disabling Enforcement

```csharp
// Allow any value, but keep bounds defined
percentTextBox1.EnforceMinMaxDuringValidating = false;

// Now users can enter values outside min/max
// Useful for display-only or read-only scenarios
```

## Null Value Handling

### Allowing Null/Empty Values

By default, PercentTextBox requires a value. Enable null values when appropriate:

```csharp
percentTextBox1.AllowNull = true;
```

### Displaying Null State

When `AllowNull = true`, you can customize how the empty state appears:

```csharp
percentTextBox1.AllowNull = true;
percentTextBox1.NullString = "Not Set";  // Display text when empty
```

### Setting Null Format

Customize how null values are displayed:

```csharp
percentTextBox1.AllowNull = true;
percentTextBox1.NullString = "N/A";
percentTextBox1.NullFormat = "{0}";  // Format string for null display
```

### Programmatically Setting to Null

```csharp
// Using BindablePercentValue
percentTextBox1.BindablePercentValue = null;

// Check if value is null
if (percentTextBox1.BindablePercentValue.HasValue)
{
    Console.WriteLine($"Value: {percentTextBox1.BindablePercentValue}%");
}
else
{
    Console.WriteLine("Control is empty (null)");
}
```

### Example: Optional Discount Field

```csharp
// A discount field that's optional
percentTextBox1.AllowNull = true;
percentTextBox1.NullString = "No discount";
percentTextBox1.MinValue = 0;
percentTextBox1.MaxValue = 100;

// User can leave empty or enter a valid percentage
var discount = percentTextBox1.BindablePercentValue;

if (discount.HasValue)
{
    Console.WriteLine($"Apply discount: {discount}%");
}
else
{
    Console.WriteLine("No discount applied");
}
```

## Validation Error Handling

### The ValidationError Event

Fires when invalid input is detected.

```csharp
percentTextBox1.ValidationError += (sender, e) =>
{
    Console.WriteLine($"Error: {e.ErrorMessage}");
    Console.WriteLine($"Invalid text: {e.InvalidText}");
    Console.WriteLine($"Position: {e.StartPosition}");
};
```

### ValidationErrorArgs Members

| Member | Type | Purpose |
|--------|------|---------|
| `ErrorMessage` | string | Description of the validation error |
| `InvalidText` | string | The text that failed validation |
| `StartPosition` | int | Position of invalid input |

### Handling Specific Errors

```csharp
percentTextBox1.ValidationError += (sender, e) =>
{
    if (e.ErrorMessage.Contains("out of range"))
    {
        MessageBox.Show("Please enter a value between 0 and 100");
    }
    else if (e.ErrorMessage.Contains("invalid format"))
    {
        MessageBox.Show("Please enter a valid number");
    }
};
```

### Example: User-Friendly Error Display

```csharp
Label errorLabel = new Label();

percentTextBox1.ValidationError += (sender, e) =>
{
    errorLabel.Text = e.ErrorMessage;
    errorLabel.ForeColor = Color.Red;
    errorLabel.Visible = true;
};

// Clear error when user starts typing valid input
percentTextBox1.BindablePercentValueChanged += (sender, e) =>
{
    errorLabel.Visible = false;
};
```

## Validation Patterns

### Pattern 1: Standard 0-100% Range

```csharp
private void SetupPercentageField(PercentTextBox control)
{
    control.MinValue = 0;
    control.MaxValue = 100;
    control.EnforceMinMaxDuringValidating = true;
    control.PercentDecimalDigits = 2;
    control.AllowNull = false;
    control.DefaultValue = 0;
}

// Usage
SetupPercentageField(percentTextBox1);
```

### Pattern 2: Optional Field with Bounds

```csharp
private void SetupOptionalPercent(PercentTextBox control)
{
    control.AllowNull = true;
    control.NullString = "Not specified";
    control.MinValue = 0;
    control.MaxValue = 100;
    control.EnforceMinMaxDuringValidating = true;
}

// Usage
SetupOptionalPercent(discountBox);
```

### Pattern 3: Accept Negative Values

```csharp
private void SetupSignedPercent(PercentTextBox control)
{
    control.MinValue = -100;
    control.MaxValue = 100;
    control.EnforceMinMaxDuringValidating = true;
    control.NegativeInputPendingOnSelectAll = true;
}

// Usage: for gains/losses, increases/decreases
SetupSignedPercent(changeBox);
```

### Pattern 4: Wide Range with Precision

```csharp
private void SetupPrecisionPercent(PercentTextBox control)
{
    control.MinValue = 0;
    control.MaxValue = 1000;  // Allow up to 1000%
    control.EnforceMinMaxDuringValidating = true;
    control.PercentDecimalDigits = 3;  // Three decimal places
}

// Usage: for scientific or multiplier values
SetupPrecisionPercent(multiplierBox);
```

### Pattern 5: With Error Feedback

```csharp
private void SetupWithErrorHandling(PercentTextBox control, Label errorLabel)
{
    control.MinValue = 0;
    control.MaxValue = 100;
    control.EnforceMinMaxDuringValidating = true;

    control.ValidationError += (sender, e) =>
    {
        errorLabel.Text = $"⚠ {e.ErrorMessage}";
        errorLabel.ForeColor = Color.Red;
    };

    control.BindablePercentValueChanged += (sender, e) =>
    {
        errorLabel.Text = "✓";
        errorLabel.ForeColor = Color.Green;
    };
}

// Usage
Label status = new Label();
SetupWithErrorHandling(percentTextBox1, status);
```

## Edge Cases and Troubleshooting

### Issue: Value Appears to Be Ignored

**Problem:** Setting value doesn't display in control

```csharp
// This might not be visible if constraints reject it
percentTextBox1.MinValue = 0;
percentTextBox1.MaxValue = 100;
percentTextBox1.PercentValue = 150;  // Outside bounds - might be ignored
```

**Solution:** Ensure value is within bounds

```csharp
// Clamp value to valid range
private double ClampValue(double value, double min, double max)
{
    return Math.Max(min, Math.Min(max, value));
}

double safeValue = ClampValue(150, percentTextBox1.MinValue, percentTextBox1.MaxValue);
percentTextBox1.PercentValue = safeValue;
```

### Issue: Null Value Displays as 0

**Problem:** Control shows 0 instead of empty

```csharp
percentTextBox1.AllowNull = true;
percentTextBox1.PercentValue = 0;  // This sets a value, not null
percentTextBox1.NullString = "Empty";  // This won't display
```

**Solution:** Use BindablePercentValue for null

```csharp
percentTextBox1.AllowNull = true;
percentTextBox1.BindablePercentValue = null;  // Now displays NullString
percentTextBox1.NullString = "Empty";
```

### Issue: ValidationError Events Fire Too Often

**Problem:** ValidationError fires for every keystroke during editing

**Solution:** Only respond to actual validation failures

```csharp
percentTextBox1.ValidationError += (sender, e) =>
{
    // Only show error if it's a critical issue
    if (e.ErrorMessage.Contains("out of range"))
    {
        MessageBox.Show(e.ErrorMessage);
    }
    // Ignore minor formatting issues
};
```

---

**Next:** Learn formatting options in [formatting-and-display.md](formatting-and-display.md) or handle events in [events-and-data-binding.md](events-and-data-binding.md)
