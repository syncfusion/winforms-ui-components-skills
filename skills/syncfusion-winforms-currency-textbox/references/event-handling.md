# Event Handling

## KeyDown Event

The `KeyDown` event fires when a key is pressed while the control has focus. Use this to implement keyboard shortcuts or custom input behavior.

### Basic KeyDown Handler

```csharp
private void currencyTextBox1_KeyDown(object sender, KeyEventArgs e)
{
    // Check which key was pressed
    if (e.KeyCode == Keys.Enter)
    {
        // Process the entered amount
        decimal amount = currencyTextBox1.DecimalValue;
        MessageBox.Show($"Amount entered: {amount:C2}");
        e.Handled = true;  // Prevent further processing
    }
}

// In Form_Load or designer:
currencyTextBox1.KeyDown += currencyTextBox1_KeyDown;
```

### Adding Keyboard Support for Large Numbers

Implement multiplier shortcuts (G, M, K) for entering large values:

```csharp
private void currencyTextBox1_KeyDown(object sender, KeyEventArgs e)
{
    decimal value = currencyTextBox1.DecimalValue;
    
    switch (e.KeyCode)
    {
        case Keys.G:  // Giga (billion)
            currencyTextBox1.DecimalValue = value * 1000000000m;
            e.Handled = true;
            break;
            
        case Keys.M:  // Mega (million)
            currencyTextBox1.DecimalValue = value * 1000000m;
            e.Handled = true;
            break;
            
        case Keys.K:  // Kilo (thousand)
            currencyTextBox1.DecimalValue = value * 1000m;
            e.Handled = true;
            break;
    }
}
```

### Usage Example: Multiplier Keys

```csharp
// User enters: 32
// User presses: K (Kilo key)
// Result: 32,000

// User enters: 5
// User presses: M (Mega key)
// Result: 5,000,000

// User enters: 2
// User presses: G (Giga key)
// Result: 2,000,000,000
```

### Modifier Key Handling

Combine keys with Ctrl, Shift, Alt:

```csharp
private void currencyTextBox1_KeyDown(object sender, KeyEventArgs e)
{
    // Ctrl+Z for undo-like functionality
    if (e.Control && e.KeyCode == Keys.Z)
    {
        currencyTextBox1.DecimalValue = 0m;
        e.Handled = true;
    }
    
    // Shift+C for currency
    if (e.Shift && e.KeyCode == Keys.C)
    {
        currencyTextBox1.CurrencySymbol = "€";
        e.Handled = true;
    }
}
```

## ValidationError Event

The `ValidationError` event fires when the user enters invalid currency data. Use this to provide feedback.

### ValidationError Event Handler

```csharp
private void currencyTextBox1_ValidationError(object sender, ValidationErrorArgs e)
{
    // Event properties:
    // e.ErrorMessage - Description of the error
    // e.InvalidText - What the user tried to enter
    // e.StartPosition - Where in the text the error occurred
    
    string message = $"Invalid input: {e.InvalidText} at position {e.StartPosition}";
    MessageBox.Show(message, "Currency Entry Error");
}

// In Form_Load:
currencyTextBox1.ValidationError += currencyTextBox1_ValidationError;
```

### Common Validation Errors

**Invalid characters:** User enters letters or special characters
```csharp
// User types: $100abc
// ValidationError fires: "abc" is invalid
// Only "$100" is kept
```

**Exceeds MaxValue:** User enters amount larger than allowed
```csharp
currencyTextBox1.MaxValue = 1000m;

// User tries to enter: 5000
// ValidationError fires: Value exceeds maximum
// Input is rejected
```

**Below MinValue:** User enters amount smaller than allowed
```csharp
currencyTextBox1.MinValue = 10m;

// User tries to enter: 5
// ValidationError fires: Value below minimum
// Input is rejected
```

## Error Validation with ErrorProvider

Use the `ErrorProvider` component to display error messages visually:

### Setup ErrorProvider with ValidationError

```csharp
// Add these to your form
private CurrencyTextBox currencyTextBox1;
private ErrorProvider errorProvider1;
private TextBox textBoxLog;

public Form1()
{
    InitializeComponent();
    
    // Initialize ErrorProvider
    errorProvider1 = new ErrorProvider();
    
    // Initialize logging TextBox for visibility
    textBoxLog = new TextBox();
    textBoxLog.Multiline = true;
    textBoxLog.Height = 100;
    textBoxLog.Location = new Point(10, 100);
    this.Controls.Add(textBoxLog);
    
    // Attach ValidationError handler
    currencyTextBox1.ValidationError += currencyTextBox1_ValidationError;
}

private void currencyTextBox1_ValidationError(object sender, ValidationErrorArgs e)
{
    // Extract error information
    string errorPosition = e.StartPosition.ToString();
    string eventMessage = String.Format(
        "Event: ValidationError | InvalidText: {0} | Position: {1}\r\n",
        e.InvalidText,
        errorPosition
    );
    
    // Log to TextBox
    textBoxLog.Text = textBoxLog.Text + eventMessage;
    
    // Display error icon next to control
    errorProvider1.SetError(
        (Control)sender,
        $"Invalid entry: {e.InvalidText} at position {e.StartPosition}"
    );
}
```

### Visual Feedback with ErrorProvider

```csharp
private void currencyTextBox1_ValidationError(object sender, ValidationErrorArgs e)
{
    // Show error icon and tooltip
    Control ctrl = (Control)sender;
    errorProvider1.SetError(ctrl, $"Invalid input: {e.InvalidText}");
    
    // Optional: Change background color
    ctrl.BackColor = System.Drawing.Color.LightCoral;
}

// Clear error when valid entry is made
private void currencyTextBox1_Leave(object sender, EventArgs e)
{
    decimal value = currencyTextBox1.DecimalValue;
    if (value >= currencyTextBox1.MinValue && value <= currencyTextBox1.MaxValue)
    {
        errorProvider1.SetError(currencyTextBox1, "");
        currencyTextBox1.BackColor = System.Drawing.Color.White;
    }
}
```

## Event Handling Scenarios

### Scenario 1: Audit Logging

```csharp
private void currencyTextBox1_ValidationError(object sender, ValidationErrorArgs e)
{
    // Log invalid entries for audit
    string logEntry = $"[{DateTime.Now:yyyy-MM-dd HH:mm:ss}] " +
                     $"Invalid currency entry: {e.InvalidText} " +
                     $"at position {e.StartPosition}";
    
    System.IO.File.AppendAllText("currency_audit.log", logEntry + Environment.NewLine);
}
```

### Scenario 2: Custom Validation Message

```csharp
private void currencyTextBox1_ValidationError(object sender, ValidationErrorArgs e)
{
    string userFriendlyMessage = "";
    
    if (e.InvalidText.Contains("$"))
    {
        userFriendlyMessage = "Dollar signs are automatically added.";
    }
    else if (e.InvalidText.Any(char.IsLetter))
    {
        userFriendlyMessage = "Letters are not allowed in currency fields.";
    }
    else
    {
        userFriendlyMessage = $"Cannot enter: {e.InvalidText}";
    }
    
    MessageBox.Show(userFriendlyMessage, "Currency Entry Info");
}
```

### Scenario 3: Value Constraints Feedback

```csharp
private void currencyTextBox1_ValidationError(object sender, ValidationErrorArgs e)
{
    Control ctrl = (Control)sender;
    
    // Check if error is due to value exceeding bounds
    if (currencyTextBox1.DecimalValue > currencyTextBox1.MaxValue)
    {
        errorProvider1.SetError(ctrl, 
            $"Amount cannot exceed {currencyTextBox1.MaxValue:C2}");
    }
    else if (currencyTextBox1.DecimalValue < currencyTextBox1.MinValue)
    {
        errorProvider1.SetError(ctrl, 
            $"Amount must be at least {currencyTextBox1.MinValue:C2}");
    }
}
```

### Scenario 4: Keyboard Shortcuts with Validation

```csharp
private void currencyTextBox1_KeyDown(object sender, KeyEventArgs e)
{
    // Multiplier shortcuts with confirmation
    decimal multiplier = 1m;
    
    switch (e.KeyCode)
    {
        case Keys.G:
            multiplier = 1000000000m;  // Billion
            break;
        case Keys.M:
            multiplier = 1000000m;     // Million
            break;
        case Keys.K:
            multiplier = 1000m;        // Thousand
            break;
        default:
            return;
    }
    
    // Calculate new value
    decimal newValue = currencyTextBox1.DecimalValue * multiplier;
    
    // Validate before applying
    if (newValue > currencyTextBox1.MaxValue)
    {
        MessageBox.Show(
            $"Result ({newValue:C2}) exceeds maximum ({currencyTextBox1.MaxValue:C2})",
            "Cannot Apply Multiplier"
        );
        e.Handled = true;
    }
    else
    {
        currencyTextBox1.DecimalValue = newValue;
        e.Handled = true;
    }
}
```

## Best Practices

### Always Set e.Handled When Processing

```csharp
// Correct: Prevents default processing
if (e.KeyCode == Keys.Enter)
{
    ProcessAmount();
    e.Handled = true;  // ← Required
}

// Incomplete: May cause unexpected behavior
if (e.KeyCode == Keys.Enter)
{
    ProcessAmount();
    // e.Handled not set - default processing continues
}
```

### Validate Before Using DecimalValue

```csharp
private void currencyTextBox1_KeyDown(object sender, KeyEventArgs e)
{
    // Safe: Check valid range first
    decimal currentValue = currencyTextBox1.DecimalValue;
    
    if (currentValue >= currencyTextBox1.MinValue && 
        currentValue <= currencyTextBox1.MaxValue)
    {
        // Safe to proceed
        decimal newValue = currentValue * 1000m;
    }
}
```

### Catch All Format Errors

```csharp
private void currencyTextBox1_ValidationError(object sender, ValidationErrorArgs e)
{
    try
    {
        // Log error information
        LogValidationError(e.InvalidText, e.StartPosition);
        
        // Notify user
        errorProvider1.SetError((Control)sender, "Invalid currency format");
    }
    catch (Exception ex)
    {
        // Fallback error handling
        MessageBox.Show($"Error handling validation: {ex.Message}");
    }
}
```

### Clear Errors on Valid Input

```csharp
private void currencyTextBox1_TextChanged(object sender, EventArgs e)
{
    // Clear error when user corrects the input
    if (currencyTextBox1.DecimalValue > 0)
    {
        errorProvider1.SetError(currencyTextBox1, "");
        currencyTextBox1.BackColor = System.Drawing.Color.White;
    }
}
```
