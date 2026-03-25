# Advanced Features

## Clipboard Support

### ClipMode Property

Controls how currency data is copied to and pasted from the clipboard:

```csharp
// Include formatting characters (default)
currencyTextBox1.ClipMode = Syncfusion.Windows.Forms.Tools.CurrencyClipModes.IncludeFormatting;

// Exclude formatting characters
currencyTextBox1.ClipMode = Syncfusion.Windows.Forms.Tools.CurrencyClipModes.ExcludeFormatting;
```

### IncludeFormatting Mode

When enabled, copying includes formatting characters (currency symbol, separators):

```csharp
currencyTextBox1.ClipMode = Syncfusion.Windows.Forms.Tools.CurrencyClipModes.IncludeFormatting;
currencyTextBox1.CurrencySymbol = "$";
currencyTextBox1.DecimalValue = 1234.56m;

// User copies the displayed text: $1,234.56
// Clipboard contains: "$1,234.56"
```

**Use case:** When you want the formatted display copied for documents or emails

### ExcludeFormatting Mode

When enabled, copying excludes formatting characters (only numeric value):

```csharp
currencyTextBox1.ClipMode = Syncfusion.Windows.Forms.Tools.CurrencyClipModes.ExcludeFormatting;
currencyTextBox1.CurrencySymbol = "$";
currencyTextBox1.DecimalValue = 1234.56m;

// User copies the displayed text: $1,234.56
// Clipboard contains: "1234.56" (no currency symbol or commas)
```

**Use case:** When pasting into databases or other numeric-only fields

### Clipboard Copy Example

```csharp
// Setup for copying numeric values only
currencyTextBox1.ClipMode = Syncfusion.Windows.Forms.Tools.CurrencyClipModes.ExcludeFormatting;

// User displays: $1,234.56
// When copied, clipboard has: 1234.56

// This can be pasted into:
// - Database numeric field
// - Excel cell (interpreted as number)
// - Another numeric control
```

### Clipboard Paste Handling

When user pastes, the control automatically formats the input:

```csharp
currencyTextBox1.ClipMode = Syncfusion.Windows.Forms.Tools.CurrencyClipModes.ExcludeFormatting;

// User pastes: "1234.56"
// Control displays: $1,234.56 (automatically formatted)

// Pasting with existing format
// User pastes: "$1,234.56"
// Control validates and extracts: 1234.56
// Displays: $1,234.56 (reformatted consistently)
```

### Complete Clipboard Configuration

```csharp
// For database applications (exclude formatting)
currencyTextBox1.ClipMode = Syncfusion.Windows.Forms.Tools.CurrencyClipModes.ExcludeFormatting;
currencyTextBox1.CurrencySymbol = "$";
currencyTextBox1.CurrencyDecimalDigits = 2;

// Copy preserves: numeric value only (1234.56)
// Paste accepts: numeric or formatted input
// Maintains: consistent internal representation

// For user-facing reports (include formatting)
currencyTextBox1.ClipMode = Syncfusion.Windows.Forms.Tools.CurrencyClipModes.IncludeFormatting;

// Copy preserves: full display ($1,234.56)
// Paste accepts: numeric or formatted input
// Shows: formatted display
```

## Overflow Indicator

Display a visual indicator when currency value exceeds the visible boundaries of the control.

### ShowOverflowIndicator Property

Enable/disable the overflow indicator display:

```csharp
// Enable overflow indicator
currencyTextBox1.ShowOverflowIndicator = true;

// Disable overflow indicator
currencyTextBox1.ShowOverflowIndicator = false;
```

### When Overflow Occurs

The indicator appears when:
- The formatted value is too long to display in the available control width
- The text would be truncated without the indicator

```csharp
currencyTextBox1.Width = 100;  // Small control
currencyTextBox1.DecimalValue = 999999999.99m;
// Display is truncated, indicator shows the value doesn't fit
```

### Overflow Indicator Display

```csharp
currencyTextBox1.ShowOverflowIndicator = true;
currencyTextBox1.Width = 120;
currencyTextBox1.CurrencyDecimalDigits = 2;

// If value is: $12,345,678.90
// And control can only show: $12,345...
// Indicator: Small symbol/icon appears (usually "►" or similar)
```

## Overflow Indicator Tooltip

### ShowOverflowIndicatorToolTip Property

Enable/disable tooltip when hovering over the overflow indicator:

```csharp
// Enable tooltip on overflow indicator
currencyTextBox1.ShowOverflowIndicatorToolTip = true;

// Disable tooltip
currencyTextBox1.ShowOverflowIndicatorToolTip = false;
```

### OverflowIndicatorToolTipText Property

Set custom text for the tooltip:

```csharp
currencyTextBox1.OverflowIndicatorToolTipText = "Full value is: ";
currencyTextBox1.ShowOverflowIndicator = true;
currencyTextBox1.ShowOverflowIndicatorToolTip = true;

// When user hovers over indicator
// Tooltip shows: "Full value is: $12,345,678.90"
```

### Overflow Tooltip Examples

```csharp
// Generic overflow message
currencyTextBox1.OverflowIndicatorToolTipText = "Overflow";

// Descriptive message
currencyTextBox1.OverflowIndicatorToolTipText = "Value too large to display";

// Custom context message
currencyTextBox1.OverflowIndicatorToolTipText = "Total amount: ";

// Full value indication
currencyTextBox1.OverflowIndicatorToolTipText = "Complete value: ";
```

## Complete Advanced Features Example

### Large Number Handling with Overflow

```csharp
// Setup for handling large numbers
currencyTextBox1.CurrencySymbol = "$";
currencyTextBox1.CurrencyDecimalDigits = 2;
currencyTextBox1.CurrencyGroupSeparator = ",";
currencyTextBox1.CurrencyGroupSizes = new int[] { 3 };

// Small control to force overflow
currencyTextBox1.Width = 150;
currencyTextBox1.Font = new System.Drawing.Font("Arial", 10);

// Enable overflow feedback
currencyTextBox1.ShowOverflowIndicator = true;
currencyTextBox1.ShowOverflowIndicatorToolTip = true;
currencyTextBox1.OverflowIndicatorToolTipText = "Full amount: ";

// Large value displays with overflow indicator
currencyTextBox1.DecimalValue = 9876543210.99m;
// Display: $9,876,543... (truncated)
// Tooltip: "Full amount: $9,876,543,210.99"
```

### Database Integration with Clipboard

```csharp
// For seamless database clipboard operations
currencyTextBox1.ClipMode = Syncfusion.Windows.Forms.Tools.CurrencyClipModes.ExcludeFormatting;
currencyTextBox1.CurrencySymbol = "$";
currencyTextBox1.CurrencyDecimalDigits = 2;

// User copies from control: "1234.56" (no formatting)
// Can be pasted directly into database

// User pastes from database: "1234.56"
// Control formats automatically: $1,234.56
```

### Report-Friendly Configuration

```csharp
// For copying to reports/documents
currencyTextBox1.ClipMode = Syncfusion.Windows.Forms.Tools.CurrencyClipModes.IncludeFormatting;
currencyTextBox1.CurrencySymbol = "$";
currencyTextBox1.CurrencyDecimalDigits = 2;

// User copies: "$1,234.56" (with formatting)
// Pastes into report maintaining currency format
```

### Responsive UI with Overflow Handling

```csharp
private void Form_Resize(object sender, EventArgs e)
{
    // Adjust overflow indicator based on available width
    if (currencyTextBox1.Width < 100)
    {
        currencyTextBox1.ShowOverflowIndicator = true;
        currencyTextBox1.ShowOverflowIndicatorToolTip = true;
    }
    else
    {
        currencyTextBox1.ShowOverflowIndicator = false;
    }
}
```

## Data Preservation and Recovery

### Handling Large Decimal Values

```csharp
// CurrencyTextBox handles decimal precision up to decimal.MaxValue
decimal largeAmount = decimal.Parse("999999999999.99");
currencyTextBox1.DecimalValue = largeAmount;

// Retrieve exact value for storage
decimal retrievedValue = currencyTextBox1.DecimalValue;
// retrievedValue == largeAmount (no precision loss)
```

### Value Range Validation

```csharp
// Validate before operations
decimal enteredValue = currencyTextBox1.DecimalValue;

if (enteredValue > currencyTextBox1.MaxValue)
{
    MessageBox.Show("Value exceeds maximum allowed");
    return;
}

if (enteredValue < currencyTextBox1.MinValue)
{
    MessageBox.Show("Value below minimum required");
    return;
}

// Safe to proceed with value
ProcessAmount(enteredValue);
```

### Null/Empty Value Handling

```csharp
// Support for optional amounts
currencyTextBox1.AllowNull = true;
currencyTextBox1.NullString = "(No amount)";

// Check if empty
if (currencyTextBox1.AllowNull && currencyTextBox1.Text == currencyTextBox1.NullString)
{
    // Field is intentionally empty
    amount = null;
}
else
{
    // Field has a value
    amount = currencyTextBox1.DecimalValue;
}
```

## Performance Considerations

### Rapid Value Updates

```csharp
// Efficiently update control with new values
for (int i = 0; i < largeDataSet.Count; i++)
{
    // Direct assignment is efficient
    currencyTextBox1.DecimalValue = largeDataSet[i].Amount;
    
    // Display updates automatically
    System.Windows.Forms.Application.DoEvents();
}
```

### Format Changes

```csharp
// Changing format settings updates display immediately
currencyTextBox1.DecimalValue = 1234.5m;

// Change formatting
currencyTextBox1.CurrencyDecimalDigits = 3;
// Display updates: $1,234.500

currencyTextBox1.CurrencyGroupSeparator = " ";
// Display updates: $1 234.500

// Current value unchanged
decimal value = currencyTextBox1.DecimalValue;  // Still 1234.5m
```

## Edge Cases and Gotchas

### Very Small Values

```csharp
// Less than 1 dollar
currencyTextBox1.CurrencyDecimalDigits = 3;
currencyTextBox1.DecimalValue = 0.001m;
// Display: $0.001 (not truncated)

currencyTextBox1.CurrencyDecimalDigits = 2;
currencyTextBox1.DecimalValue = 0.005m;
// Display: $0.01 (rounded, not truncated)
```

### Negative Amounts with Patterns

```csharp
// Parentheses for negative (accounting format)
currencyTextBox1.CurrencyNegativePattern = 13;  // ($ 1)
currencyTextBox1.DecimalValue = -100m;
// Display: ($ 100.00)

// Verify value is still negative
if (currencyTextBox1.DecimalValue < 0)
{
    // Negative value confirmed, not a display artifact
}
```

### Multiple DecimalValue Assignments

```csharp
// Rapid assignments
currencyTextBox1.DecimalValue = 100m;
currencyTextBox1.DecimalValue = 200m;
currencyTextBox1.DecimalValue = 300m;

// Last assignment wins
decimal final = currencyTextBox1.DecimalValue;  // 300m
string display = currencyTextBox1.Text;  // "$300.00"
```
