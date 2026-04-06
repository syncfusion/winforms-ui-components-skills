# Events and Advanced Patterns in SfNumericTextBox

## Table of Contents
- [ValueChanged Event](#valuechanged-event)
- [ValueChangedEventArgs](#valuechangedeventargs)
- [Event Subscription Patterns](#event-subscription-patterns)
- [Advanced Scenarios](#advanced-scenarios)
- [Performance Considerations](#performance-considerations)
- [Common Patterns](#common-patterns)

## ValueChanged Event

### Purpose

The `ValueChanged` event fires when the Value property changes, allowing you to respond to value updates with custom logic.

### Basic Usage

```csharp
// Subscribe to the event
this.numericTextBox.ValueChanged += NumericTextBox_ValueChanged;

// Event handler
private void NumericTextBox_ValueChanged(object sender, 
    Syncfusion.WinForms.Input.Events.ValueChangedEventArgs e)
{
    double? newValue = e.NewValue;
    double? oldValue = e.OldValue;
    
    // Your logic here
}
```

### When Does ValueChanged Fire?

The event fires when:
1. **Programmatically**: `Value` property is set in code
2. **User Input**: User enters a value (timing depends on ValueChangeMode)
3. **Validation Correction**: Invalid value is corrected on LostFocus
4. **Clearing**: Value is set to null

### When It Does NOT Fire

- When other properties are changed
- When text is entered but Value hasn't changed yet (depends on ValueChangeMode)
- When control is disabled

## ValueChangedEventArgs

### Properties

#### NewValue

```csharp
double? newValue = e.NewValue;
```

**Type:** `double?` (nullable double)
**Contains:** The new value after change

#### OldValue

```csharp
double? oldValue = e.OldValue;
```

**Type:** `double?` (nullable double)
**Contains:** The previous value before change

### Usage Examples

```csharp
private void NumericTextBox_ValueChanged(object sender, ValueChangedEventArgs e)
{
    // Calculate delta (change amount)
    double? delta = null;
    if (e.OldValue.HasValue && e.NewValue.HasValue)
    {
        delta = e.NewValue.Value - e.OldValue.Value;
        MessageBox.Show($"Changed by: {delta}");
    }
    
    // Handle null transitions
    if (e.OldValue.HasValue && !e.NewValue.HasValue)
    {
        MessageBox.Show("Value cleared");
    }
    
    if (!e.OldValue.HasValue && e.NewValue.HasValue)
    {
        MessageBox.Show($"Value set to: {e.NewValue}");
    }
}
```

## Event Subscription Patterns

### Pattern 1: Named Event Handler

```csharp
// In form initialization
public Form1()
{
    InitializeComponent();
    this.numericTextBox.ValueChanged += NumericTextBox_ValueChanged;
}

// Event handler method
private void NumericTextBox_ValueChanged(object sender, ValueChangedEventArgs e)
{
    Debug.WriteLine($"Value changed from {e.OldValue} to {e.NewValue}");
}
```

**Advantages:**
- Clear, reusable handler
- Easy to debug
- Can be shared with multiple controls

**Use When:**
- Handler is complex
- Used by multiple controls
- Need to maintain readability

### Pattern 2: Lambda Expression

```csharp
this.numericTextBox.ValueChanged += (sender, e) =>
{
    Debug.WriteLine($"New value: {e.NewValue}");
};
```

**Use When:** Simple one-liner operations or logic specific to one control.

### Pattern 3: Method Group

```csharp
this.numericTextBox.ValueChanged += OnAnyValueChanged;

private void OnAnyValueChanged(object sender, ValueChangedEventArgs e)
{
    // Handler can be shared with other controls
}
```

## Advanced Scenarios

### Scenario 1: Cascading Value Updates

```csharp
// When quantity changes, update total automatically
private void QuantityTextBox_ValueChanged(object sender, ValueChangedEventArgs e)
{
    if (e.NewValue.HasValue && pricePerUnit > 0)
    {
        double total = e.NewValue.Value * pricePerUnit;
        totalTextBox.Value = total;
    }
}
```

### Scenario 2: Conditional Formatting

```csharp
// Apply styles based on value ranges
private void Amount_ValueChanged(object sender, ValueChangedEventArgs e)
{
    var textBox = (SfNumericTextBox)sender;
    
    if (!e.NewValue.HasValue)
        textBox.Style.PositiveForeColor = Color.Gray;
    else if (e.NewValue.Value > 1000)
        textBox.Style.PositiveForeColor = Color.Green;
    else if (e.NewValue.Value > 100)
        textBox.Style.PositiveForeColor = Color.Orange;
    else
        textBox.Style.PositiveForeColor = Color.Red;
}
```

### Scenario 3
```csharp
// Log all value changes for audit purposes
private void AuditedField_ValueChanged(object sender, ValueChangedEventArgs e)
{
    var textBox = (SfNumericTextBox)sender;
    
    string logEntry = $"[{DateTime.Now:HH:mm:ss}] {textBox.Name} " +
                      $"changed from {e.OldValue} to {e.NewValue}";
    
    File.AppendAllText("audit_log.txt", logEntry + Environment.NewLine);
}
```

### Scenario 5: Update Total Across Multiple Fields

```csharp
// Form with quantity and price, updates total
private void QuantityTextBox_ValueChanged(object sender, ValueChangedEventArgs e)
{
    UpdateTotal();
}

private void PriceTextBox_ValueChanged(object sender, ValueChangedEventArgs e)
{
    UpdateTotal();
}

private void UpdateTotal()
{
    double? qty = quantityTextBox.Value;
    double? price = priceTextBox.Value;
    
    if (qty.HasValue && price.HasValue)
    {
        totalTextBox.Value = qty.Value * price.Value;
    }
}
```

## Performance Considerations

### Avoid Heavy Operations in ValueChanged

private void AuditedField_ValueChanged(object sender, ValueChangedEventArgs e)
{
    string logEntry = $"[{DateTime.Now:HH:mm:ss}] " +
                      $"changed from {e.OldValue} to {e.NewValue}";    // Heavy computation
        Math.Sqrt(i);
    }
}
```

**✅ PREFERRED - Fast:**
```csharp
private void ValueChanged_FastHandler(object sender, ValueChangedEventArgs e)
{
    // Simple, fast operation
    if (e.NewValue.HasValue)
    {
        resultLabel.Text = (e.NewValue.Value * 1.1).ToString("F2");
    }
}
```

### Event Timing with ValueChangeMode

```csharp
// KeyPress mode: Event fires frequently
this.numericTextBox.ValueChangeMode = ValueChangeMode.KeyPress;
this.numericTextBox.ValueChanged += (s, e) =>
{
    // Called on EVERY keystroke - keep it fast!
    simpleLabel.Text = e.NewValue?.ToString() ?? "N/A";
};

// LostFocus mode: Event fires once when done
this.numericTextBox.ValueChangeMode = ValueChangeMode.LostFocus;
this.numericTextBox.ValueChanged += (s, e) =>
{
    // Called only when user leaves field - can be slower
    SaveToDatabase(e.NewValue);
};
```

### Unsubscribe When Needed

```csharp
// Good practice: Unsubscribe when done
public void InitializeControl()
{
    this.numericTextBox.ValueChanged += NumericTextBox_ValueChanged;
public void CleanupControl()
{
    this.numericTextBox.ValueChanged -= NumericTextBox_ValueChanged;
}

public override void Dispose(bool disposing)
{
    if (disposing)
        CleanupControl();
## Common Patterns

### Pattern: Real-Time Calculation

```csharp
private void SetupCalculator()
{
    // Input: Quantity
    quantityBox.ValueChangeMode = ValueChangeMode.KeyPress;
    quantityBox.ValueChanged += (s, e) => UpdateTotal();
    
    // Input: Unit Price
    priceBox.ValueChangeMode = ValueChangeMode.KeyPress;
    priceBox.ValueChanged += (s, e) => UpdateTotal();
}

private void UpdateTotal()
{
    double? qty = quantityBox.Value;
    quantityBox.ValueChanged += (s, e) => UpdateTotal();
    priceBox.ValueChanged += (s, e) => UpdateTotal();
}

private void UpdateTotal()
{
    if (quantityBox.Value.HasValue && priceBox.Value.HasValue)
        totalBox.Value = quantityBox.Value.Value * priceBox.Value.Value;
    else
        totalBox.Value = nullalaryBox.ValueChanged += UpdateFormStatus;
}

private void UpdateFormStatus(object sender, ValueChangedEventArgs e)
{
    bool isComplete = 
        !string.IsNullOrEmpty(nameBox.Text) &&
        ageBox.Value.HasValue &&
        salaryBox.Value.HasValue;
    
    submitButton.Enabled = isComplete;
}
```

### Pattern: Change Tracking

```csharp
private double? lastSavedValue;

private void ValueBox_ValueChanged(object sender, ValueChangedEventArgs e)
{
    bool hasChanged = e.NewValue != lastSavedValue;
    
    if (hasChanged)
    {
        saveButton.Enabled = true;
        saveButton.Text = "Save*";  // Indicate unsaved
    }
}

private void SaveButton_Click(object sender, EventArgs e)
{
    // Save logic
    lastSavedValue = valueBox.Value;
    saveButton.Enabled = false;
    saveButton.Text = "Save";
}
```

### Pattern: Conditional Visibility

```csharp
private void AmountBox_ValueChanged(object sender, ValueChangedEventArgs e)
{
    nameBox.ValueChanged += UpdateFormStatus;
    ageBox.ValueChanged += UpdateFormStatus;
    salaryBox.ValueChanged += UpdateFormStatus;
}

private void UpdateFormStatus(object sender, ValueChangedEventArgs e)
{
    submitButton.Enabled = !string.IsNullOrEmpty(nameBox.Text) &&
                           ageBox.Value.HasValue && salaryBox.Value.HasValue;
}
```

### Pattern: Change Tracking

```csharp
private double? lastSavedValue;

private void ValueBox_ValueChanged(object sender, ValueChangedEventArgs e)
{
    if (e.NewValue != lastSavedValue)
    {
        saveButton.Enabled = true;
        saveButton.Text = "Save*"
            totalBox.Value = null;
            validationLabel.Text = "Missing quantity or price";
            validationLabel.ForeColor = Color.Red;
            return;
        }

        double subtotal = qty.Value * price.Value;
        double discountAmount = discount.HasValue ? 
            (subtotal * discount.Value / 100) : 0;
        double total = subtotal - discountAmount;

        totalBox.Value = total;
        validationLabel.Text = "Valid";
        validationLabel.ForeColor = Color.Green;
    }
}
```

## Important Notes

- **Event Order**: Events fire in subscription order
- **Multiple Values Changing**: If you change multiple values in the event handler, each triggers its own event (be careful of infinite loops)
- **Null Values**: Always check `.HasValue` before using `.Value`
- **Unsubscribe**: Unsubscribe when controls are disposed to prevent memory leaks
- **Performance**: Keep event handlers fast; defer heavy operations to background tasks