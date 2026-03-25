# Validation and Constraints

## Setting Min/Max Boundaries

### MaxValue Property

Sets the maximum allowed currency amount:

```csharp
// Standard budget limit
currencyTextBox1.MaxValue = 10000m;

// Large transactions
currencyTextBox1.MaxValue = 999999999.99m;

// No practical limit (up to decimal.MaxValue)
currencyTextBox1.MaxValue = decimal.MaxValue;

// Single dollar
currencyTextBox1.MaxValue = 1m;
```

### MinValue Property

Sets the minimum allowed currency amount:

```csharp
// Cannot be negative
currencyTextBox1.MinValue = 0m;

// Allow negative values (losses, refunds)
currencyTextBox1.MinValue = decimal.MinValue;

// Minimum purchase
currencyTextBox1.MinValue = 0.01m;  // Minimum one penny

// Range with minimum
currencyTextBox1.MinValue = -5000m;  // Allow up to $5000 loss
```

### Valid Range Example

```csharp
// Set boundaries
currencyTextBox1.MinValue = 10m;      // Minimum $10
currencyTextBox1.MaxValue = 5000m;    // Maximum $5,000

// Valid entries
currencyTextBox1.DecimalValue = 100m;    // ✓ Accepted
currencyTextBox1.DecimalValue = 5000m;   // ✓ Accepted
currencyTextBox1.DecimalValue = 10m;     // ✓ Accepted

// Invalid entries (rejected, ValidationError fires)
currencyTextBox1.DecimalValue = 5m;      // ✗ Below minimum
currencyTextBox1.DecimalValue = 5001m;   // ✗ Exceeds maximum
```

## EnforceMinMaxDuringValidating Property

### When Enforcement Happens

Controls whether min/max limits are enforced during text input validation:

```csharp
// Enforce during entry (recommended)
currencyTextBox1.EnforceMinMaxDuringValidating = true;
// User cannot type values exceeding MaxValue or MinValue

// No enforcement during entry (only programmatic)
currencyTextBox1.EnforceMinMaxDuringValidating = false;
// ValidationError may not fire, but DecimalValue respects limits
```

### Enforcement Examples

```csharp
// With enforcement enabled
currencyTextBox1.EnforceMinMaxDuringValidating = true;
currencyTextBox1.MaxValue = 1000m;
currencyTextBox1.MinValue = 0m;

// User tries to type: 5000
// Type 5: Allowed ✓
// Type 0: Allowed ✓ (now showing 50)
// Type 0: Allowed ✓ (now showing 500)
// Type 0: Rejected ✗ (would exceed 1000)
// Result: User can only enter up to 1000
```

```csharp
// Without enforcement
currencyTextBox1.EnforceMinMaxDuringValidating = false;
currencyTextBox1.MaxValue = 1000m;

// User can type: 50000
// Later, accessing DecimalValue may be clamped to MaxValue
// But during entry, no immediate rejection
```

### Recommended Configuration

```csharp
// Standard approach - enforce during validation
currencyTextBox1.EnforceMinMaxDuringValidating = true;
currencyTextBox1.MinValue = 0m;
currencyTextBox1.MaxValue = 100000m;

// User gets immediate feedback on invalid amounts
// Cannot enter values outside the range
```

## Negative Input Behavior

### NegativeInputPendingOnSelectAll Property

Controls behavior when user has selected all text and presses the negative key:

```csharp
// Replace selected value with negative input
currencyTextBox1.NegativeInputPendingOnSelectAll = true;

// Negate the current value
currencyTextBox1.NegativeInputPendingOnSelectAll = false;
```

### Pending Mode (true): Replacement

When enabled, pressing negative key with all text selected starts a new negative entry:

```csharp
currencyTextBox1.NegativeInputPendingOnSelectAll = true;
currencyTextBox1.DecimalValue = 100m;
// Display: $100.00

// User selects all (Ctrl+A)
// User presses minus (-)
// User types: 50
// Result: -$50.00 (old value replaced)
```

**Advantage:** Clean replacement of value with negative

### Non-Pending Mode (false): Negation

When disabled, pressing negative key negates the current selected value:

```csharp
currencyTextBox1.NegativeInputPendingOnSelectAll = false;
currencyTextBox1.DecimalValue = 100m;
// Display: $100.00

// User selects all (Ctrl+A)
// User presses minus (-)
// Result: -$100.00 (current value negated)

// If user continues typing after negation, it replaces the value
// User types: 5
// Result: -$5.00 (after negation, new number replaces)
```

**Advantage:** Quick negation without full replacement

### Configuration Examples

```csharp
// For profit/loss entry (replacement mode preferred)
currencyTextBox1.NegativeInputPendingOnSelectAll = true;
currencyTextBox1.MinValue = decimal.MinValue;
currencyTextBox1.MaxValue = decimal.MaxValue;

// User: 100 → Select all → - → 50 → Result: -50

// For quick negation (toggle mode)
currencyTextBox1.NegativeInputPendingOnSelectAll = false;

// User: 100 → Select all → - → Result: -100
```

## AllowNull Property

### Enabling Null/Empty Values

Allow the control to accept empty/null values:

```csharp
// Allow empty state
currencyTextBox1.AllowNull = true;

// Require a value (default)
currencyTextBox1.AllowNull = false;
```

### NullString Property

Define what text displays when the value is null:

```csharp
// Empty display
currencyTextBox1.NullString = "";

// Placeholder text
currencyTextBox1.NullString = "(empty)";

// Descriptive text
currencyTextBox1.NullString = "N/A";

// Special symbol
currencyTextBox1.NullString = "—";  // Em dash
```

### Optional Field Configuration

```csharp
// Optional amount field
currencyTextBox1.AllowNull = true;
currencyTextBox1.NullString = "";
currencyTextBox1.Text = "";

// Initial display: empty
// After user enters: $100.00
// If user clears: back to empty

// Check if empty
if (currencyTextBox1.Text == currencyTextBox1.NullString)
{
    amount = null;  // Field is intentionally empty
}
else
{
    amount = currencyTextBox1.DecimalValue;  // Field has value
}
```

### Null String Examples

```csharp
// Optional budget field
currencyTextBox1.AllowNull = true;
currencyTextBox1.NullString = "Not specified";

// Optional fee field
currencyTextBox1.AllowNull = true;
currencyTextBox1.NullString = "No fee";

// Optional override amount
currencyTextBox1.AllowNull = true;
currencyTextBox1.NullString = "Use default";
```

## Value Range Enforcement

### Comprehensive Validation Setup

```csharp
// Complete validation configuration
currencyTextBox1.MinValue = 0m;                      // No negative
currencyTextBox1.MaxValue = 999999999.99m;          // Maximum limit
currencyTextBox1.EnforceMinMaxDuringValidating = true;  // Enforce on entry
currencyTextBox1.AllowNull = false;                 // Require value

// User can only enter values 0 to 999,999,999.99
```

### Multi-Step Validation

```csharp
// Programmatic validation beyond simple min/max
private decimal ValidateCurrencyInput(CurrencyTextBox box)
{
    decimal value = box.DecimalValue;
    
    // Check range
    if (value < box.MinValue || value > box.MaxValue)
    {
        throw new ArgumentException("Value out of range");
    }
    
    // Check business rules
    if (value % 0.01m != 0)  // Only whole cents
    {
        throw new ArgumentException("Value must be in cents");
    }
    
    // Check precision
    int decimalPlaces = GetDecimalPlaces(value);
    if (decimalPlaces > box.CurrencyDecimalDigits)
    {
        throw new ArgumentException("Too many decimal places");
    }
    
    return value;
}

private int GetDecimalPlaces(decimal value)
{
    return BitConverter.GetBytes(decimal.GetBits(value)[3])[2];
}
```

### Validation on Form Submit

```csharp
private void SubmitButton_Click(object sender, EventArgs e)
{
    // Validate all currency fields
    if (!ValidateAllFields())
    {
        MessageBox.Show("Please correct the highlighted fields.");
        return;
    }
    
    // Proceed with valid data
    ProcessData();
}

private bool ValidateAllFields()
{
    bool isValid = true;
    
    // Amount field
    if (amountBox.DecimalValue < 0.01m || amountBox.DecimalValue > 999999.99m)
    {
        amountBox.BorderColor = System.Drawing.Color.Red;
        isValid = false;
    }
    else
    {
        amountBox.BorderColor = System.Drawing.Color.Gray;
    }
    
    // Optional fee field
    if (feeBox.AllowNull && feeBox.Text == feeBox.NullString)
    {
        // Null is allowed, no error
    }
    else if (feeBox.DecimalValue < 0 || feeBox.DecimalValue > 1000m)
    {
        feeBox.BorderColor = System.Drawing.Color.Red;
        isValid = false;
    }
    else
    {
        feeBox.BorderColor = System.Drawing.Color.Gray;
    }
    
    return isValid;
}
```

## Constraint Enforcement Scenarios

### Scenario 1: Purchase Amount (Must be positive)

```csharp
// Purchase orders always positive
purchaseBox.MinValue = 0.01m;      // At least one cent
purchaseBox.MaxValue = 999999.99m; // Practical maximum
purchaseBox.EnforceMinMaxDuringValidating = true;
purchaseBox.AllowNull = false;     // Required field

// User can only enter: 0.01 to 999,999.99
```

### Scenario 2: Profit/Loss (Allow positive or negative)

```csharp
// Financial P&L entries can be positive or negative
profitBox.MinValue = decimal.MinValue;  // Allow any negative
profitBox.MaxValue = decimal.MaxValue;  // Allow any positive
profitBox.EnforceMinMaxDuringValidating = true;
profitBox.AllowNull = false;            // Must have value

// User can enter: Any value positive or negative
```

### Scenario 3: Percentage-Based Fee (0-100%)

```csharp
// Fee as percentage: 0 to 100
feePercentBox.MinValue = 0m;
feePercentBox.MaxValue = 100m;
feePercentBox.CurrencyDecimalDigits = 2;
feePercentBox.CurrencySymbol = "%";
feePercentBox.EnforceMinMaxDuringValidating = true;

// User can only enter: 0.00% to 100.00%
```

### Scenario 4: Account Balance (With overdraft limit)

```csharp
// Checking account with overdraft protection
balanceBox.MinValue = -500m;    // Allow $500 overdraft
balanceBox.MaxValue = 999999.99m; // Practical maximum
balanceBox.EnforceMinMaxDuringValidating = true;

// User can enter: -500.00 to 999,999.99
// -500 represents maximum allowed overdraft
```

### Scenario 5: Budget Line Item (Optional with range)

```csharp
// Optional budget lines with category limits
budgetBox.AllowNull = true;      // Can be empty
budgetBox.NullString = "—";      // Display as dash when empty
budgetBox.MinValue = 0m;         // If entered, must be positive
budgetBox.MaxValue = 50000m;     // Max per line item
budgetBox.EnforceMinMaxDuringValidating = true;

// User can: Leave empty (—) or enter 0.01 to 50,000.00
```

## Input Validation Flow

### Complete Validation Sequence

```csharp
// 1. Control-level validation (automatic)
// - EnforceMinMaxDuringValidating: Check bounds
// - Keyboard input validation: Reject invalid characters
// - ValidationError event: Fires on invalid input

// 2. Application-level validation (in code)
private bool IsValidCurrencyAmount(CurrencyTextBox box, 
    decimal minAllowed, decimal maxAllowed, bool allowNull)
{
    // Check if null
    if (box.AllowNull && box.Text == box.NullString)
    {
        return allowNull;  // Null OK if allowNull=true
    }
    
    // Check bounds
    decimal value = box.DecimalValue;
    if (value < minAllowed || value > maxAllowed)
    {
        return false;
    }
    
    // Check precision
    if (box.CurrencyDecimalDigits != GetDecimalPlaces(value))
    {
        // Optional: Enforce exact decimal places
    }
    
    return true;
}

// 3. Use in validation
private void currencyTextBox1_Leave(object sender, EventArgs e)
{
    if (!IsValidCurrencyAmount(currencyTextBox1, 0m, 10000m, false))
    {
        errorProvider1.SetError(currencyTextBox1, "Invalid amount");
        currencyTextBox1.Focus();
    }
    else
    {
        errorProvider1.SetError(currencyTextBox1, "");
    }
}
```

## Common Validation Patterns

### Required Positive Amount

```csharp
currencyTextBox1.AllowNull = false;
currencyTextBox1.MinValue = 0.01m;
currencyTextBox1.MaxValue = 999999.99m;
```

### Optional Non-Negative Amount

```csharp
currencyTextBox1.AllowNull = true;
currencyTextBox1.NullString = "";
currencyTextBox1.MinValue = 0m;
currencyTextBox1.MaxValue = 999999.99m;
```

### Required Amount with Range

```csharp
currencyTextBox1.AllowNull = false;
currencyTextBox1.MinValue = 10m;
currencyTextBox1.MaxValue = 5000m;
```

### Bidirectional (Debit/Credit)

```csharp
currencyTextBox1.AllowNull = false;
currencyTextBox1.MinValue = decimal.MinValue;
currencyTextBox1.MaxValue = decimal.MaxValue;
```
