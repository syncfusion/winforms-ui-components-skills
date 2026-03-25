# Behavior Settings for NumericUpDownExt

Comprehensive guide to configuring interaction behaviors and user input handling in the NumericUpDownExt control.

## Overview

Behavior settings control how users interact with the NumericUpDownExt control. These settings include keyboard input, read-only mode, focus behavior, and event handling. Proper behavior configuration ensures intuitive and efficient user interactions.

## InterceptArrowKeys Property

The `InterceptArrowKeys` property determines whether arrow keys can be used to change the value.

**Type:** `bool`  
**Default:** `true`

### When to Use
- Enable for quick keyboard value adjustment
- Disable when arrow keys should navigate between controls
- Enable for data entry workflows
- Disable in forms with complex keyboard navigation

### Basic InterceptArrowKeys Usage

```csharp
using Syncfusion.Windows.Forms.Tools;

// Enable arrow key interception (default)
numericUpDownExt1.InterceptArrowKeys = true;

// User can press Up Arrow to increment, Down Arrow to decrement
numericUpDownExt1.Minimum = new decimal(0);
numericUpDownExt1.Maximum = new decimal(100);
numericUpDownExt1.Increment = new decimal(1);
numericUpDownExt1.Value = new decimal(50);
```

**Result:** Up/Down arrow keys change the value by the Increment amount.

### Disabling Arrow Keys

```csharp
// Disable arrow key interception
numericUpDownExt1.InterceptArrowKeys = false;
```

**Result:** Arrow keys do not change the value; they can be used for form navigation.

### Quick Entry Example

```csharp
private void SetupQuickEntry()
{
    NumericUpDownExt quantityControl = new NumericUpDownExt();
    
    // Enable keyboard shortcuts
    quantityControl.InterceptArrowKeys = true;
    quantityControl.Minimum = new decimal(1);
    quantityControl.Maximum = new decimal(999);
    quantityControl.Value = new decimal(1);
    quantityControl.Increment = new decimal(1);
    quantityControl.Location = new Point(50, 50);
    
    Label instructions = new Label();
    instructions.Text = "Use Up/Down arrows to adjust quantity";
    instructions.Location = new Point(50, 80);
    instructions.AutoSize = true;
    
    this.Controls.Add(quantityControl);
    this.Controls.Add(instructions);
}
```

**Result:** Users can quickly adjust values using arrow keys.

### Form Navigation Priority

```csharp
private void SetupFormNavigation()
{
    // Multiple controls where arrow keys should navigate, not modify values
    NumericUpDownExt control1 = new NumericUpDownExt();
    control1.InterceptArrowKeys = false;
    control1.Location = new Point(50, 30);
    control1.TabIndex = 0;
    
    NumericUpDownExt control2 = new NumericUpDownExt();
    control2.InterceptArrowKeys = false;
    control2.Location = new Point(50, 60);
    control2.TabIndex = 1;
    
    NumericUpDownExt control3 = new NumericUpDownExt();
    control3.InterceptArrowKeys = false;
    control3.Location = new Point(50, 90);
    control3.TabIndex = 2;
    
    // Arrow keys navigate between controls instead of changing values
    this.Controls.Add(control1);
    this.Controls.Add(control2);
    this.Controls.Add(control3);
}
```

**Result:** Arrow keys navigate between controls rather than modifying values.

### Conditional Arrow Key Handling

```csharp
private void SetupConditionalArrowKeys()
{
    NumericUpDownExt smartControl = new NumericUpDownExt();
    smartControl.Location = new Point(50, 50);
    smartControl.Minimum = new decimal(0);
    smartControl.Maximum = new decimal(100);
    smartControl.Value = new decimal(50);
    
    CheckBox chkEnableArrows = new CheckBox();
    chkEnableArrows.Text = "Enable Arrow Keys";
    chkEnableArrows.Location = new Point(50, 80);
    chkEnableArrows.Checked = true;
    
    chkEnableArrows.CheckedChanged += (s, e) =>
    {
        smartControl.InterceptArrowKeys = chkEnableArrows.Checked;
    };
    
    this.Controls.Add(smartControl);
    this.Controls.Add(chkEnableArrows);
}
```

**Result:** Users can toggle arrow key behavior dynamically.

## ReadOnly Property

The `ReadOnly` property prevents users from manually typing values while still allowing button/arrow key changes.

**Type:** `bool`  
**Default:** `false`

### When to Use
- Display-only scenarios where values are set programmatically
- Preventing accidental manual edits
- Calculated fields that users can't modify
- Workflow states where editing is temporarily disabled

### Basic ReadOnly Usage

```csharp
// Make control read-only
numericUpDownExt1.ReadOnly = true;
numericUpDownExt1.Value = new decimal(100);
```

**Result:** Users cannot type in the text box but can still use up/down buttons.

### Read-Only with Visual Feedback

```csharp
private void SetupReadOnlyWithFeedback()
{
    NumericUpDownExt readOnlyControl = new NumericUpDownExt();
    
    readOnlyControl.ReadOnly = true;
    readOnlyControl.Value = new decimal(500);
    readOnlyControl.Location = new Point(50, 50);
    
    // Visual indicator for read-only state
    readOnlyControl.BackColor = Color.LightGray;
    readOnlyControl.ForeColor = Color.DarkGray;
    
    Label lblReadOnly = new Label();
    lblReadOnly.Text = "(Read-Only)";
    lblReadOnly.Location = new Point(170, 53);
    lblReadOnly.ForeColor = Color.Gray;
    lblReadOnly.AutoSize = true;
    
    this.Controls.Add(readOnlyControl);
    this.Controls.Add(lblReadOnly);
}
```

**Result:** Read-only control with clear visual indication of state.

### Calculated Field Example

```csharp
private void SetupCalculatedField()
{
    NumericUpDownExt priceControl = new NumericUpDownExt();
    priceControl.DecimalPlaces = 2;
    priceControl.Value = new decimal(100.00M);
    priceControl.Location = new Point(100, 30);
    
    NumericUpDownExt quantityControl = new NumericUpDownExt();
    quantityControl.DecimalPlaces = 0;
    quantityControl.Value = new decimal(5);
    quantityControl.Location = new Point(100, 60);
    
    NumericUpDownExt totalControl = new NumericUpDownExt();
    totalControl.DecimalPlaces = 2;
    totalControl.ReadOnly = true; // Calculated, can't edit
    totalControl.BackColor = Color.LightYellow;
    totalControl.Location = new Point(100, 90);
    
    // Calculate total
    EventHandler calculate = (s, e) =>
    {
        totalControl.Value = priceControl.Value * quantityControl.Value;
    };
    
    priceControl.ValueChanged += calculate;
    quantityControl.ValueChanged += calculate;
    
    // Initial calculation
    totalControl.Value = priceControl.Value * quantityControl.Value;
    
    // Labels
    Label lblPrice = new Label { Text = "Price:", Location = new Point(30, 33), AutoSize = true };
    Label lblQty = new Label { Text = "Quantity:", Location = new Point(30, 63), AutoSize = true };
    Label lblTotal = new Label { Text = "Total:", Location = new Point(30, 93), AutoSize = true };
    
    this.Controls.Add(lblPrice);
    this.Controls.Add(priceControl);
    this.Controls.Add(lblQty);
    this.Controls.Add(quantityControl);
    this.Controls.Add(lblTotal);
    this.Controls.Add(totalControl);
}
```

**Result:** Total field is read-only and automatically calculated from price and quantity.

### Dynamic Read-Only State

```csharp
private void SetupDynamicReadOnly()
{
    NumericUpDownExt editableControl = new NumericUpDownExt();
    editableControl.Value = new decimal(100);
    editableControl.Location = new Point(50, 50);
    
    CheckBox chkLock = new CheckBox();
    chkLock.Text = "Lock Value";
    chkLock.Location = new Point(50, 80);
    
    chkLock.CheckedChanged += (s, e) =>
    {
        editableControl.ReadOnly = chkLock.Checked;
        editableControl.BackColor = chkLock.Checked ? Color.LightGray : Color.White;
    };
    
    this.Controls.Add(editableControl);
    this.Controls.Add(chkLock);
}
```

**Result:** Users can toggle read-only state with checkbox.

## Key Support for Large Value Entry

Implement custom key handlers to support quick entry of large values using letter multipliers.

### When to Use
- Financial applications with large numbers
- Scientific calculators
- Data entry where thousands/millions are common
- Improving data entry efficiency

### Basic Multiplier Keys

```csharp
private void SetupMultiplierKeys()
{
    NumericUpDownExt largeValueControl = new NumericUpDownExt();
    
    largeValueControl.Minimum = new decimal(0);
    largeValueControl.Maximum = new decimal(999999999);
    largeValueControl.ThousandsSeparator = true;
    largeValueControl.DecimalPlaces = 0;
    largeValueControl.Value = new decimal(0);
    largeValueControl.Location = new Point(50, 50);
    
    // Handle key press for multipliers
    largeValueControl.KeyDown += (s, e) =>
    {
        decimal currentValue = largeValueControl.Value;
        
        switch (e.KeyCode)
        {
            case Keys.K: // Thousand
                largeValueControl.Value = currentValue * 1000;
                e.Handled = true;
                break;
                
            case Keys.M: // Million
                largeValueControl.Value = currentValue * 1000000;
                e.Handled = true;
                break;
                
            case Keys.G: // Billion
                largeValueControl.Value = currentValue * 1000000000;
                e.Handled = true;
                break;
        }
    };
    
    Label instructions = new Label();
    instructions.Text = "Enter number, then press K (thousand), M (million), or G (billion)";
    instructions.Location = new Point(50, 80);
    instructions.AutoSize = true;
    
    this.Controls.Add(largeValueControl);
    this.Controls.Add(instructions);
}
```

**Result:** Type "22" then press "K" to get "22,000" automatically.

### Complete Large Value Entry Example

```csharp
private void SetupAdvancedKeySupport()
{
    NumericUpDownExt budgetControl = new NumericUpDownExt();
    
    budgetControl.Minimum = new decimal(0);
    budgetControl.Maximum = decimal.MaxValue;
    budgetControl.ThousandsSeparator = true;
    budgetControl.DecimalPlaces = 2;
    budgetControl.Value = new decimal(0);
    budgetControl.Location = new Point(100, 50);
    budgetControl.Size = new Size(150, 24);
    
    Label lblBudget = new Label();
    lblBudget.Text = "Budget:";
    lblBudget.Location = new Point(30, 53);
    lblBudget.AutoSize = true;
    
    Label lblHelp = new Label();
    lblHelp.Text = "Shortcuts: K=x1,000  M=x1,000,000  G=x1,000,000,000";
    lblHelp.Location = new Point(30, 80);
    lblHelp.AutoSize = true;
    lblHelp.ForeColor = Color.Gray;
    
    budgetControl.KeyDown += (sender, e) =>
    {
        decimal value = budgetControl.Value;
        decimal multiplier = 1;
        bool shouldMultiply = false;
        
        switch (e.KeyCode)
        {
            case Keys.K:
                multiplier = 1000;
                shouldMultiply = true;
                break;
            case Keys.M:
                multiplier = 1000000;
                shouldMultiply = true;
                break;
            case Keys.G:
                multiplier = 1000000000;
                shouldMultiply = true;
                break;
        }
        
        if (shouldMultiply)
        {
            try
            {
                budgetControl.Value = value * multiplier;
                e.Handled = true;
                e.SuppressKeyPress = true;
            }
            catch (OverflowException)
            {
                MessageBox.Show("Value too large!", "Error", 
                    MessageBoxButtons.OK, MessageBoxIcon.Warning);
            }
        }
    };
    
    this.Controls.Add(lblBudget);
    this.Controls.Add(budgetControl);
    this.Controls.Add(lblHelp);
}
```

**Result:** Full-featured large value entry with keyboard shortcuts and help text.

## User Interaction Patterns

### Double-Click to Reset

```csharp
private void SetupDoubleClickReset()
{
    NumericUpDownExt resetControl = new NumericUpDownExt();
    
    resetControl.Minimum = new decimal(0);
    resetControl.Maximum = new decimal(100);
    resetControl.Value = new decimal(50);
    decimal defaultValue = 50;
    
    resetControl.Location = new Point(50, 50);
    
    resetControl.DoubleClick += (s, e) =>
    {
        resetControl.Value = defaultValue;
        MessageBox.Show($"Value reset to default: {defaultValue}",
            "Reset", MessageBoxButtons.OK, MessageBoxIcon.Information);
    };
    
    Label lblHelp = new Label();
    lblHelp.Text = "Double-click to reset to default value";
    lblHelp.Location = new Point(50, 80);
    lblHelp.AutoSize = true;
    lblHelp.ForeColor = Color.Gray;
    
    this.Controls.Add(resetControl);
    this.Controls.Add(lblHelp);
}
```

**Result:** Users can quickly reset value by double-clicking.

### Mouse Wheel Support

```csharp
private void SetupMouseWheelControl()
{
    NumericUpDownExt wheelControl = new NumericUpDownExt();
    
    wheelControl.Minimum = new decimal(0);
    wheelControl.Maximum = new decimal(100);
    wheelControl.Value = new decimal(50);
    wheelControl.Increment = new decimal(5);
    wheelControl.Location = new Point(50, 50);
    
    // Mouse wheel automatically works with NumericUpDown controls
    // Additional custom behavior can be added:
    wheelControl.MouseWheel += (s, e) =>
    {
        // Optional: Show tooltip with new value
        ToolTip tooltip = new ToolTip();
        tooltip.Show($"Value: {wheelControl.Value}", 
            wheelControl, 0, -30, 1000);
    };
    
    Label lblHelp = new Label();
    lblHelp.Text = "Focus control and use mouse wheel to adjust";
    lblHelp.Location = new Point(50, 80);
    lblHelp.AutoSize = true;
    
    this.Controls.Add(wheelControl);
    this.Controls.Add(lblHelp);
}
```

**Result:** Mouse wheel changes value when control has focus.

## Focus Behavior

### Auto-Select on Focus

```csharp
private void SetupAutoSelect()
{
    NumericUpDownExt autoSelectControl = new NumericUpDownExt();
    
    autoSelectControl.Value = new decimal(12345);
    autoSelectControl.Location = new Point(50, 50);
    
    // Select all text when control receives focus
    autoSelectControl.Enter += (s, e) =>
    {
        autoSelectControl.Select(0, autoSelectControl.Text.Length);
    };
    
    Label lblHelp = new Label();
    lblHelp.Text = "Text automatically selected when focused";
    lblHelp.Location = new Point(50, 80);
    lblHelp.AutoSize = true;
    
    this.Controls.Add(autoSelectControl);
    this.Controls.Add(lblHelp);
}
```

**Result:** All text is selected when control receives focus for easy replacement.

### Focus Validation

```csharp
private void SetupFocusValidation()
{
    NumericUpDownExt validatedControl = new NumericUpDownExt();
    
    validatedControl.Minimum = new decimal(1);
    validatedControl.Maximum = new decimal(100);
    validatedControl.Value = new decimal(50);
    validatedControl.Location = new Point(50, 50);
    
    // Validate when leaving control
    validatedControl.Leave += (s, e) =>
    {
        if (validatedControl.Value < 10)
        {
            MessageBox.Show("Warning: Value is below recommended minimum of 10",
                "Validation", MessageBoxButtons.OK, MessageBoxIcon.Warning);
        }
    };
    
    this.Controls.Add(validatedControl);
}
```

**Result:** Validation occurs when user leaves the control.

## Tab Order Considerations

### Setting Tab Order

```csharp
private void SetupTabOrder()
{
    // Create form with multiple controls
    Label lblFirstName = new Label { Text = "First Name:", Location = new Point(30, 33), AutoSize = true };
    TextBox txtFirstName = new TextBox { Location = new Point(120, 30), TabIndex = 0 };
    
    Label lblAge = new Label { Text = "Age:", Location = new Point(30, 63), AutoSize = true };
    NumericUpDownExt numAge = new NumericUpDownExt 
    { 
        Location = new Point(120, 60), 
        TabIndex = 1,
        Minimum = new decimal(0),
        Maximum = new decimal(120)
    };
    
    Label lblSalary = new Label { Text = "Salary:", Location = new Point(30, 93), AutoSize = true };
    NumericUpDownExt numSalary = new NumericUpDownExt 
    { 
        Location = new Point(120, 90), 
        TabIndex = 2,
        DecimalPlaces = 2,
        ThousandsSeparator = true
    };
    
    Button btnSubmit = new Button { Text = "Submit", Location = new Point(120, 120), TabIndex = 3 };
    
    // Tab order: FirstName -> Age -> Salary -> Submit
    this.Controls.Add(lblFirstName);
    this.Controls.Add(txtFirstName);
    this.Controls.Add(lblAge);
    this.Controls.Add(numAge);
    this.Controls.Add(lblSalary);
    this.Controls.Add(numSalary);
    this.Controls.Add(btnSubmit);
}
```

**Result:** Proper tab navigation through form controls.

## Validation Integration

### Real-Time Validation

```csharp
private void SetupRealtimeValidation()
{
    NumericUpDownExt quantityControl = new NumericUpDownExt();
    quantityControl.Minimum = new decimal(1);
    quantityControl.Maximum = new decimal(100);
    quantityControl.Value = new decimal(1);
    quantityControl.Location = new Point(100, 50);
    
    Label lblValidation = new Label();
    lblValidation.Location = new Point(230, 53);
    lblValidation.Size = new Size(200, 20);
    
    int availableStock = 50;
    
    quantityControl.ValueChanged += (s, e) =>
    {
        if (quantityControl.Value > availableStock)
        {
            lblValidation.Text = "⚠ Exceeds available stock!";
            lblValidation.ForeColor = Color.Red;
        }
        else if (quantityControl.Value > availableStock * 0.9M)
        {
            lblValidation.Text = "⚠ Low stock warning";
            lblValidation.ForeColor = Color.Orange;
        }
        else
        {
            lblValidation.Text = "✓ Available";
            lblValidation.ForeColor = Color.Green;
        }
    };
    
    // Initial validation
    lblValidation.Text = "✓ Available";
    lblValidation.ForeColor = Color.Green;
    
    Label lblQuantity = new Label();
    lblQuantity.Text = "Quantity:";
    lblQuantity.Location = new Point(30, 53);
    lblQuantity.AutoSize = true;
    
    this.Controls.Add(lblQuantity);
    this.Controls.Add(quantityControl);
    this.Controls.Add(lblValidation);
}
```

**Result:** Real-time validation feedback as user changes value.

## Event Handling (ValueChanged)

### Basic ValueChanged Handling

```csharp
private void SetupValueChangedEvent()
{
    NumericUpDownExt control = new NumericUpDownExt();
    control.Location = new Point(50, 50);
    control.Value = new decimal(0);
    
    Label lblDisplay = new Label();
    lblDisplay.Location = new Point(50, 80);
    lblDisplay.Size = new Size(200, 20);
    
    // Handle value changes
    control.ValueChanged += (sender, e) =>
    {
        lblDisplay.Text = $"Current value: {control.Value}";
    };
    
    lblDisplay.Text = $"Current value: {control.Value}";
    
    this.Controls.Add(control);
    this.Controls.Add(lblDisplay);
}
```

**Result:** Label updates whenever value changes.

### Complex Event Handling Example

```csharp
private void SetupComplexEventHandling()
{
    NumericUpDownExt priceControl = new NumericUpDownExt();
    NumericUpDownExt quantityControl = new NumericUpDownExt();
    NumericUpDownExt discountControl = new NumericUpDownExt();
    
    // Setup price control
    priceControl.DecimalPlaces = 2;
    priceControl.Minimum = new decimal(0);
    priceControl.Maximum = new decimal(9999.99M);
    priceControl.Value = new decimal(100.00M);
    priceControl.Location = new Point(100, 30);
    
    // Setup quantity control
    quantityControl.DecimalPlaces = 0;
    quantityControl.Minimum = new decimal(1);
    quantityControl.Maximum = new decimal(999);
    quantityControl.Value = new decimal(1);
    quantityControl.Location = new Point(100, 60);
    
    // Setup discount control
    discountControl.DecimalPlaces = 0;
    discountControl.Minimum = new decimal(0);
    discountControl.Maximum = new decimal(100);
    discountControl.Value = new decimal(0);
    discountControl.Location = new Point(100, 90);
    
    // Result labels
    Label lblSubtotal = new Label { Location = new Point(100, 120), Size = new Size(200, 20) };
    Label lblDiscount = new Label { Location = new Point(100, 140), Size = new Size(200, 20) };
    Label lblTotal = new Label { Location = new Point(100, 160), Size = new Size(200, 20), Font = new Font("Arial", 10, FontStyle.Bold) };
    
    // Calculation method
    void Calculate()
    {
        decimal subtotal = priceControl.Value * quantityControl.Value;
        decimal discountAmount = subtotal * (discountControl.Value / 100);
        decimal total = subtotal - discountAmount;
        
        lblSubtotal.Text = $"Subtotal: ${subtotal:F2}";
        lblDiscount.Text = $"Discount ({discountControl.Value}%): -${discountAmount:F2}";
        lblTotal.Text = $"Total: ${total:F2}";
    }
    
    // Wire up events
    priceControl.ValueChanged += (s, e) => Calculate();
    quantityControl.ValueChanged += (s, e) => Calculate();
    discountControl.ValueChanged += (s, e) => Calculate();
    
    // Initial calculation
    Calculate();
    
    // Add labels
    Label lblPrice = new Label { Text = "Price:", Location = new Point(30, 33), AutoSize = true };
    Label lblQty = new Label { Text = "Quantity:", Location = new Point(30, 63), AutoSize = true };
    Label lblDisc = new Label { Text = "Discount %:", Location = new Point(30, 93), AutoSize = true };
    
    // Add all controls
    this.Controls.Add(lblPrice);
    this.Controls.Add(priceControl);
    this.Controls.Add(lblQty);
    this.Controls.Add(quantityControl);
    this.Controls.Add(lblDisc);
    this.Controls.Add(discountControl);
    this.Controls.Add(lblSubtotal);
    this.Controls.Add(lblDiscount);
    this.Controls.Add(lblTotal);
}
```

**Result:** Interactive invoice calculator with live totals.

## Best Practices

### Behavior Configuration Checklist

1. **Enable InterceptArrowKeys** for data entry forms
2. **Disable InterceptArrowKeys** when arrow keys should navigate
3. **Set ReadOnly** for calculated or display-only fields
4. **Provide visual feedback** for read-only state
5. **Implement key multipliers** for large value entry
6. **Handle ValueChanged** for dependent calculations
7. **Set appropriate TabIndex** for logical form flow
8. **Add validation** on value changes or focus loss
9. **Consider auto-select** on focus for quick editing
10. **Test keyboard and mouse** interactions thoroughly
