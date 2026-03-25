# Data Formatting & Number Handling

## Table of Contents
- [Number Formatting](#number-formatting)
- [Number Group Separators](#number-group-separators)
- [Handling Negative Values](#handling-negative-values)
- [Leading Zeros](#leading-zeros)
- [Null Value Management](#null-value-management)
- [Read-Only States](#read-only-states)

---

## Number Formatting

The IntegerTextBox provides several approaches to display numbers in custom formats.

### Standard Number Display

By default, numbers display without formatting:

```csharp
integerTextBox1.IntegerValue = 1234567;
// Displays: 1234567
```

### Custom Format with Group Separators

Apply custom formatting using **NumberGroupSeparator** and **NumberGroupSizes**.

---

## Number Group Separators

### GroupSeparator Property

Specify a separator character to group digits:

```csharp
// Use comma separator
this.integerTextBox1.NumberGroupSeparator = ",";
integerTextBox1.IntegerValue = 1238761534122;
// Displays: 1,238,761,534,122
```

**Common Separators:**

```csharp
// US/UK format: comma separator
integerTextBox1.NumberGroupSeparator = ",";

// European format: period separator
integerTextBox1.NumberGroupSeparator = ".";

// Space separator
integerTextBox1.NumberGroupSeparator = " ";

// Underscore (custom)
integerTextBox1.NumberGroupSeparator = "_";
```

### GroupSizes Property

Define how many digits appear in each group using **NumberGroupSizes**:

```csharp
// Group by 3 digits (thousands)
this.integerTextBox1.NumberGroupSizes = new int[] { 3 };
integerTextBox1.IntegerValue = 1234567;
// Displays: 1,234,567
```

**Common Group Patterns:**

```csharp
// Indian format: 12,34,567 (groups of 2,3)
integerTextBox1.NumberGroupSizes = new int[] { 2, 3 };

// US format: 1,234,567 (groups of 3)
integerTextBox1.NumberGroupSizes = new int[] { 3 };

// Bank format: groups of 4
integerTextBox1.NumberGroupSizes = new int[] { 4 };

// Flexible: 5,3,3,... (first 5, then 3s)
integerTextBox1.NumberGroupSizes = new int[] { 5, 3 };
```

### Complete Formatting Examples

**Example 1: US Currency Format**

```csharp
integerTextBox1.NumberGroupSeparator = ",";
integerTextBox1.NumberGroupSizes = new int[] { 3 };
integerTextBox1.IntegerValue = 1000000;
// Displays: 1,000,000
```

**Example 2: Indian Numbering**

```csharp
integerTextBox1.NumberGroupSeparator = ",";
integerTextBox1.NumberGroupSizes = new int[] { 2, 3 };
integerTextBox1.IntegerValue = 1234567;
// Displays: 12,34,567
```

**Example 3: Custom Digit Grouping**

```csharp
integerTextBox1.NumberGroupSeparator = " ";
integerTextBox1.NumberGroupSizes = new int[] { 4 };
integerTextBox1.IntegerValue = 123456789;
// Displays: 1234 5678 9
```

---

## Handling Negative Values

Control how negative values are input and processed.

### DeleteSelectionOnNegative

When the user presses the minus key, decide whether to delete the current selection:

```csharp
// Delete selected text when minus key is pressed
this.integerTextBox1.DeleteSelectionOnNegative = true;

// Keep selected text and prepend minus sign
this.integerTextBox1.DeleteSelectionOnNegative = false;
```

**Use Cases:**

```csharp
// For finance apps where user might replace value with negative
integerTextBox1.DeleteSelectionOnNegative = true;

// For calculations where minus means "negate current value"
integerTextBox1.DeleteSelectionOnNegative = false;
```

### NegativeInputPendingOnSelectAll

When the user selects all and presses minus, decide the behavior:

```csharp
// Treat minus as negation intent (awaiting next key)
this.integerTextBox1.NegativeInputPendingOnSelectAll = true;

// Treat minus as error or ignore
this.integerTextBox1.NegativeInputPendingOnSelectAll = false;
```

**Example Scenario:**

```csharp
// Setup for flexible negative input
integerTextBox1.DeleteSelectionOnNegative = true;
integerTextBox1.NegativeInputPendingOnSelectAll = true;
integerTextBox1.MinValue = -100;
integerTextBox1.MaxValue = 100;
```

### Color Coding Negative Values

Visually distinguish negative numbers:

```csharp
integerTextBox1.PositiveColor = System.Drawing.Color.Green;
integerTextBox1.NegativeColor = System.Drawing.Color.Red;
```

---

## Leading Zeros

Allow numbers to be displayed with leading zeros.

### AllowLeadingZeros Property

Enable leading zeros:

```csharp
this.integerTextBox1.AllowLeadingZeros = true;
integerTextBox1.IntegerValue = 123;
// Displays as: 0123 (if field width supports it) or 000123
```

**Scenarios:**

```csharp
// Product IDs with leading zeros: 00001, 00002
productIdTextBox.AllowLeadingZeros = true;

// Standard numeric entry (no leading zeros)
standardNumberBox.AllowLeadingZeros = false;

// Invoice numbers with leading zeros
invoiceBox.AllowLeadingZeros = true;
```

**Important:** Leading zeros display depends on the display field width and formatting. The control may show them during editing.

---

## Null Value Management

Handle cases where no value has been entered.

### Using BindableValue

The **BindableValue** property allows null values (unlike IntegerValue which requires an integer):

```csharp
// Set to null (empty)
integerTextBox1.BindableValue = null;

// Set to an integer value
integerTextBox1.BindableValue = 42;

// Check for null
if (integerTextBox1.BindableValue == null)
{
    MessageBox.Show("No value entered");
}
else
{
    int val = (int)integerTextBox1.BindableValue;
}
```

### BindableValueChanged Event

Respond when BindableValue changes (including null assignments):

```csharp
integerTextBox1.BindableValueChanged += (sender, e) =>
{
    if (integerTextBox1.BindableValue == null)
        Console.WriteLine("Value cleared");
    else
        Console.WriteLine($"Value: {integerTextBox1.BindableValue}");
};
```

### SetNull Event

Triggered when the value is set to null:

```csharp
integerTextBox1.SetNull += (sender, e) =>
{
    Console.WriteLine("Control value cleared to null");
};
```

**Complete Null Handling Example:**

```csharp
private void SetupNullableInput()
{
    integerTextBox1.BindableValue = null;  // Start empty
    
    integerTextBox1.BindableValueChanged += (sender, e) =>
    {
        if (integerTextBox1.BindableValue == null)
            statusLabel.Text = "Empty";
        else
            statusLabel.Text = $"Value: {integerTextBox1.BindableValue}";
    };
    
    integerTextBox1.SetNull += (sender, e) =>
    {
        Console.WriteLine("Cleared by user");
    };
}
```

---

## Read-Only States

Display values without allowing user modification.

### Making the Control Read-Only

```csharp
// Prevent editing
this.integerTextBox1.ReadOnly = true;
this.integerTextBox1.IntegerValue = 500;
```

### Separate Color for Read-Only

Use **ReadOnlyBackColor** to distinguish the state:

```csharp
integerTextBox1.ReadOnly = true;
integerTextBox1.ReadOnlyBackColor = System.Drawing.Color.LightGray;
integerTextBox1.IntegerValue = 250;
```

**Practical Example: Summary Display**

```csharp
private void DisplayTotalReadOnly()
{
    totalTextBox.IntegerValue = CalculateTotal();
    totalTextBox.ReadOnly = true;
    totalTextBox.ReadOnlyBackColor = System.Drawing.Color.AliceBlue;
    totalTextBox.ForeColor = System.Drawing.Color.Navy;
}
```

### Toggling Read-Only State

```csharp
private void ToggleEditMode()
{
    if (integerTextBox1.ReadOnly)
    {
        integerTextBox1.ReadOnly = false;
        integerTextBox1.BackColor = System.Drawing.Color.White;
        editButton.Text = "Save";
    }
    else
    {
        integerTextBox1.ReadOnly = true;
        integerTextBox1.ReadOnlyBackColor = System.Drawing.Color.WhiteSmoke;
        editButton.Text = "Edit";
    }
}
```

---

## Complete Formatting Example

Here's a comprehensive setup combining multiple formatting features:

```csharp
private void ConfigureFinancialTextBox(IntegerTextBox textBox)
{
    // Constraints
    textBox.MinValue = -999999999;
    textBox.MaxValue = 999999999;
    
    // Formatting
    textBox.NumberGroupSeparator = ",";
    textBox.NumberGroupSizes = new int[] { 3 };
    
    // Negative handling
    textBox.DeleteSelectionOnNegative = true;
    textBox.NegativeInputPendingOnSelectAll = true;
    
    // Colors
    textBox.PositiveColor = System.Drawing.Color.DarkGreen;
    textBox.NegativeColor = System.Drawing.Color.Crimson;
    textBox.ZeroColor = System.Drawing.Color.Gray;
    
    // Nullable
    textBox.BindableValue = null;
    
    textBox.BindableValueChanged += (sender, e) =>
    {
        if (textBox.BindableValue == null)
            Console.WriteLine("Empty");
        else
            Console.WriteLine($"Amount: {textBox.BindableValue}");
    };
}
```

---

## Key Takeaways

- Use **NumberGroupSeparator** and **NumberGroupSizes** for custom formatting
- Control negative input behavior with **DeleteSelectionOnNegative** and **NegativeInputPendingOnSelectAll**
- Enable **AllowLeadingZeros** for IDs and codes
- Use **BindableValue** for nullable scenarios
- Handle **SetNull** event to respond to clearing
- Apply **ReadOnly** with **ReadOnlyBackColor** for display-only fields
