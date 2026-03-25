# Value Settings for NumericUpDownExt

Comprehensive guide to configuring and managing numeric values in the NumericUpDownExt control.

## Table of Contents

- [Overview](#overview)
- [Value Property](#value-property)
- [Minimum Property](#minimum-property)
- [Maximum Property](#maximum-property)
- [Increment Property](#increment-property)
- [Hexadecimal Mode](#hexadecimal-mode)
- [HexValue Property](#hexvalue-property)
- [UpButton and DownButton Methods](#upbutton-and-downbutton-methods)
- [Constraint Scenarios](#constraint-scenarios)
- [Validation Patterns](#validation-patterns)
- [Common Use Cases](#common-use-cases)

## Overview

Value settings control the numeric behavior of the NumericUpDownExt control. These properties define the current value, acceptable range, increment step, and display format (decimal or hexadecimal).

## Value Property

The `Value` property gets or sets the current numeric value displayed in the control.

**Type:** `decimal`  
**Default:** `0`

### When to Use
- Setting the initial value when the control loads
- Reading the user's selected value
- Programmatically changing the displayed value
- Binding to data sources

### Basic Usage

```csharp
using Syncfusion.Windows.Forms.Tools;

// Set initial value
numericUpDownExt1.Value = new decimal(50);

// Read current value
decimal currentValue = numericUpDownExt1.Value;
Console.WriteLine($"Current value: {currentValue}");
```

**Result:** Control displays 50 as the initial value.

### Setting Value with Constraints

```csharp
// Configure range first
numericUpDownExt1.Minimum = new decimal(0);
numericUpDownExt1.Maximum = new decimal(100);

// Set value within range
numericUpDownExt1.Value = new decimal(75);

// Attempting to set value outside range will be clamped
numericUpDownExt1.Value = new decimal(150); // Will be set to 100 (Maximum)
numericUpDownExt1.Value = new decimal(-10); // Will be set to 0 (Minimum)
```

**Result:** Values are automatically constrained to the defined range.

### Decimal Value Example

```csharp
// Working with decimal values
numericUpDownExt1.DecimalPlaces = 2;
numericUpDownExt1.Value = new decimal(123.45);
```

**Result:** Displays "123.45" with two decimal places.

### Complete Price Input Example

```csharp
private void SetupPriceInput()
{
    NumericUpDownExt priceControl = new NumericUpDownExt();
    
    // Configure for currency
    priceControl.Minimum = new decimal(0);
    priceControl.Maximum = new decimal(999999.99M);
    priceControl.DecimalPlaces = 2;
    priceControl.ThousandsSeparator = true;
    
    // Set initial price
    priceControl.Value = new decimal(99.99M);
    
    // Handle value changes
    priceControl.ValueChanged += (s, e) => 
    {
        decimal price = priceControl.Value;
        decimal tax = price * 0.08M; // 8% tax
        decimal total = price + tax;
        Console.WriteLine($"Price: ${price:F2}, Tax: ${tax:F2}, Total: ${total:F2}");
    };
    
    this.Controls.Add(priceControl);
}
```

**Result:** A price input control with currency formatting that calculates tax on value changes.

## Minimum Property

The `Minimum` property sets the lower bound for allowable values.

**Type:** `decimal`  
**Default:** `0`

### When to Use
- Preventing negative values in quantity controls
- Enforcing business rules (e.g., minimum order quantity)
- Setting lower limits for measurements
- Constraining user input to valid ranges

### Basic Minimum Configuration

```csharp
// Set minimum value
numericUpDownExt1.Minimum = new decimal(10);
numericUpDownExt1.Maximum = new decimal(100);
numericUpDownExt1.Value = new decimal(50);
```

**Result:** User cannot decrease value below 10.

### Zero and Negative Minimums

```csharp
// Allow negative values (temperature example)
NumericUpDownExt temperatureControl = new NumericUpDownExt();
temperatureControl.Minimum = new decimal(-50);
temperatureControl.Maximum = new decimal(50);
temperatureControl.Value = new decimal(20);
temperatureControl.DecimalPlaces = 1;
```

**Result:** Control accepts negative values from -50 to 50.

### Minimum with Decimal Values

```csharp
// Minimum for percentage (0.1% minimum)
numericUpDownExt1.Minimum = new decimal(0.1M);
numericUpDownExt1.Maximum = new decimal(100);
numericUpDownExt1.DecimalPlaces = 1;
numericUpDownExt1.Increment = new decimal(0.1M);
```

**Result:** Minimum value is 0.1, preventing zero or negative percentages.

### Business Rule Example

```csharp
private void SetupMinimumOrderQuantity()
{
    NumericUpDownExt orderQty = new NumericUpDownExt();
    
    // Business rule: Minimum order is 5 units
    orderQty.Minimum = new decimal(5);
    orderQty.Maximum = new decimal(1000);
    orderQty.Value = new decimal(5);
    orderQty.Increment = new decimal(1);
    
    // Display warning when at minimum
    orderQty.ValueChanged += (s, e) =>
    {
        if (orderQty.Value == orderQty.Minimum)
        {
            Console.WriteLine("Warning: Minimum order quantity reached");
        }
    };
    
    this.Controls.Add(orderQty);
}
```

**Result:** Enforces minimum order quantity of 5 units with warning.

## Maximum Property

The `Maximum` property sets the upper bound for allowable values.

**Type:** `decimal`  
**Default:** `100`

### When to Use
- Limiting quantities to available stock
- Enforcing capacity constraints
- Setting upper limits for percentages or rates
- Preventing unrealistic values

### Basic Maximum Configuration

```csharp
// Set maximum value
numericUpDownExt1.Minimum = new decimal(0);
numericUpDownExt1.Maximum = new decimal(500);
numericUpDownExt1.Value = new decimal(100);
```

**Result:** User cannot increase value above 500.

### Large Maximum Values

```csharp
// Very large maximum (population counter)
NumericUpDownExt populationControl = new NumericUpDownExt();
populationControl.Minimum = new decimal(0);
populationControl.Maximum = new decimal(999999999);
populationControl.ThousandsSeparator = true;
populationControl.Value = new decimal(1000000);
```

**Result:** Supports very large numbers with thousands separator for readability.

### Percentage Maximum

```csharp
// Percentage control (0-100%)
numericUpDownExt1.Minimum = new decimal(0);
numericUpDownExt1.Maximum = new decimal(100);
numericUpDownExt1.DecimalPlaces = 1;
numericUpDownExt1.Value = new decimal(50.0M);
```

**Result:** Perfect for percentage inputs, capped at 100%.

### Dynamic Maximum Example

```csharp
private NumericUpDownExt qtyControl;
private int availableStock = 25;

private void SetupDynamicMaximum()
{
    qtyControl = new NumericUpDownExt();
    
    // Set maximum based on available stock
    qtyControl.Minimum = new decimal(1);
    qtyControl.Maximum = new decimal(availableStock);
    qtyControl.Value = new decimal(1);
    
    // Update maximum when stock changes
    UpdateStockLevel(30); // New stock arrived
    
    this.Controls.Add(qtyControl);
}

private void UpdateStockLevel(int newStock)
{
    availableStock = newStock;
    qtyControl.Maximum = new decimal(availableStock);
    
    // Ensure current value doesn't exceed new maximum
    if (qtyControl.Value > qtyControl.Maximum)
    {
        qtyControl.Value = qtyControl.Maximum;
    }
    
    Console.WriteLine($"Stock updated. Available: {availableStock}");
}
```

**Result:** Maximum value adjusts dynamically based on available inventory.

## Increment Property

The `Increment` property defines how much the value changes when users click the up/down buttons or press arrow keys.

**Type:** `decimal`  
**Default:** `1`

### When to Use
- Setting step sizes for specific measurement units
- Creating fast navigation for large ranges
- Fine-tuning precision for decimal inputs
- Providing intuitive increment values (5, 10, 25, etc.)

### Basic Increment

```csharp
// Increment by 5
numericUpDownExt1.Minimum = new decimal(0);
numericUpDownExt1.Maximum = new decimal(100);
numericUpDownExt1.Increment = new decimal(5);
numericUpDownExt1.Value = new decimal(0);
```

**Result:** Each button click changes value by 5 (0, 5, 10, 15, 20...).

### Decimal Increment

```csharp
// Fine decimal increments (0.25 steps)
numericUpDownExt1.Minimum = new decimal(0);
numericUpDownExt1.Maximum = new decimal(10);
numericUpDownExt1.Increment = new decimal(0.25M);
numericUpDownExt1.DecimalPlaces = 2;
numericUpDownExt1.Value = new decimal(0);
```

**Result:** Value changes by 0.25 each click (0.00, 0.25, 0.50, 0.75, 1.00...).

### Large Increment for Big Ranges

```csharp
// Large increments for year selection
NumericUpDownExt yearControl = new NumericUpDownExt();
yearControl.Minimum = new decimal(1900);
yearControl.Maximum = new decimal(2100);
yearControl.Increment = new decimal(1);
yearControl.DecimalPlaces = 0;
yearControl.Value = new decimal(2024);
```

**Result:** Convenient year selection with single-year increments.

### Currency Increment Example

```csharp
private void SetupCurrencyIncrement()
{
    NumericUpDownExt priceControl = new NumericUpDownExt();
    
    // Price increments by $0.99
    priceControl.Minimum = new decimal(0);
    priceControl.Maximum = new decimal(999.99M);
    priceControl.Increment = new decimal(0.99M);
    priceControl.DecimalPlaces = 2;
    priceControl.Value = new decimal(9.99M);
    
    // Common pricing: 9.99, 10.98, 11.97, etc.
    this.Controls.Add(priceControl);
}
```

**Result:** Creates pricing increments common in retail (0.99 steps).

### Multiple Increment Levels

```csharp
private NumericUpDownExt volumeControl;

private void SetupVolumeControl()
{
    volumeControl = new NumericUpDownExt();
    volumeControl.Minimum = new decimal(0);
    volumeControl.Maximum = new decimal(100);
    volumeControl.Value = new decimal(50);
    
    // Default increment
    volumeControl.Increment = new decimal(1);
    
    // Change increment based on modifier keys
    volumeControl.KeyDown += VolumeControl_KeyDown;
    
    this.Controls.Add(volumeControl);
}

private void VolumeControl_KeyDown(object sender, KeyEventArgs e)
{
    // Hold Shift for larger increments
    if (e.Shift)
    {
        volumeControl.Increment = new decimal(10);
    }
    else
    {
        volumeControl.Increment = new decimal(1);
    }
}
```

**Result:** Users can change increment size by holding Shift key.

## Hexadecimal Mode

The `Hexadecimal` property displays the value in hexadecimal format instead of decimal.

**Type:** `bool`  
**Default:** `false`

### When to Use
- Color value inputs (RGB components)
- Memory address inputs
- Binary/hex data entry
- Low-level programming tools
- Hardware configuration utilities

### Basic Hexadecimal Display

```csharp
// Display value in hexadecimal
numericUpDownExt1.Minimum = new decimal(0);
numericUpDownExt1.Maximum = new decimal(255);
numericUpDownExt1.Value = new decimal(128);
numericUpDownExt1.Hexadecimal = true;
```

**Result:** Displays "80" instead of "128" (0x80 in hex).

### RGB Color Component Example

```csharp
private void SetupColorComponentPickers()
{
    // Red component (0-255)
    NumericUpDownExt redComponent = new NumericUpDownExt();
    redComponent.Minimum = new decimal(0);
    redComponent.Maximum = new decimal(255);
    redComponent.Value = new decimal(255);
    redComponent.Hexadecimal = true;
    redComponent.Location = new Point(50, 30);
    
    // Green component (0-255)
    NumericUpDownExt greenComponent = new NumericUpDownExt();
    greenComponent.Minimum = new decimal(0);
    greenComponent.Maximum = new decimal(255);
    greenComponent.Value = new decimal(128);
    greenComponent.Hexadecimal = true;
    greenComponent.Location = new Point(50, 60);
    
    // Blue component (0-255)
    NumericUpDownExt blueComponent = new NumericUpDownExt();
    blueComponent.Minimum = new decimal(0);
    blueComponent.Maximum = new decimal(255);
    blueComponent.Value = new decimal(0);
    blueComponent.Hexadecimal = true;
    blueComponent.Location = new Point(50, 90);
    
    // ValueChanged handler to display color
    EventHandler updateColor = (s, e) =>
    {
        int r = (int)redComponent.Value;
        int g = (int)greenComponent.Value;
        int b = (int)blueComponent.Value;
        
        Console.WriteLine($"RGB: #{r:X2}{g:X2}{b:X2}");
        Console.WriteLine($"Decimal: R={r}, G={g}, B={b}");
    };
    
    redComponent.ValueChanged += updateColor;
    greenComponent.ValueChanged += updateColor;
    blueComponent.ValueChanged += updateColor;
    
    this.Controls.Add(redComponent);
    this.Controls.Add(greenComponent);
    this.Controls.Add(blueComponent);
}
```

**Result:** Three hex input controls for RGB color values with live preview.

### Memory Address Input

```csharp
// Memory address selector
NumericUpDownExt addressControl = new NumericUpDownExt();
addressControl.Minimum = new decimal(0);
addressControl.Maximum = new decimal(65535); // 0xFFFF
addressControl.Value = new decimal(4096); // 0x1000
addressControl.Hexadecimal = true;
addressControl.Increment = new decimal(16); // Increment by 0x10
```

**Result:** Displays memory addresses in hexadecimal (e.g., "1000" for 4096).

## HexValue Property

The `HexValue` property gets the current value as a hexadecimal string (read-only).

**Type:** `string` (read-only)

### When to Use
- Retrieving hex representation for display
- Logging or debugging output
- Copying hex values to clipboard
- Generating hex strings for API calls

### Reading HexValue

```csharp
numericUpDownExt1.Value = new decimal(255);
numericUpDownExt1.Hexadecimal = true;

// Get hex representation
string hexValue = numericUpDownExt1.HexValue;
Console.WriteLine($"Hex Value: 0x{hexValue}"); // Output: "0xFF"
```

**Result:** Retrieves the hexadecimal string representation.

### Hex Value Display Example

```csharp
private void SetupHexDisplay()
{
    NumericUpDownExt hexInput = new NumericUpDownExt();
    hexInput.Minimum = new decimal(0);
    hexInput.Maximum = new decimal(4095);
    hexInput.Value = new decimal(256);
    hexInput.Hexadecimal = true;
    hexInput.Location = new Point(50, 30);
    
    Label hexLabel = new Label();
    hexLabel.Location = new Point(50, 60);
    hexLabel.Size = new Size(200, 20);
    
    // Display both decimal and hex
    hexInput.ValueChanged += (s, e) =>
    {
        hexLabel.Text = $"Decimal: {hexInput.Value}, Hex: 0x{hexInput.HexValue}";
    };
    
    // Initial display
    hexLabel.Text = $"Decimal: {hexInput.Value}, Hex: 0x{hexInput.HexValue}";
    
    this.Controls.Add(hexInput);
    this.Controls.Add(hexLabel);
}
```

**Result:** Shows both decimal and hexadecimal representations simultaneously.

## UpButton and DownButton Methods

These methods programmatically increment or decrement the value by the Increment amount.

### When to Use
- Custom UI with separate buttons
- Keyboard shortcuts for increment/decrement
- Automated testing
- Programmatic value changes

### UpButton Method

```csharp
// Increment value programmatically
numericUpDownExt1.Value = new decimal(50);
numericUpDownExt1.Increment = new decimal(5);

numericUpDownExt1.UpButton(); // Value becomes 55
numericUpDownExt1.UpButton(); // Value becomes 60
```

**Result:** Value increases by the Increment amount each call.

### DownButton Method

```csharp
// Decrement value programmatically
numericUpDownExt1.Value = new decimal(50);
numericUpDownExt1.Increment = new decimal(5);

numericUpDownExt1.DownButton(); // Value becomes 45
numericUpDownExt1.DownButton(); // Value becomes 40
```

**Result:** Value decreases by the Increment amount each call.

### Custom Button Controls

```csharp
private void SetupCustomButtons()
{
    NumericUpDownExt valueControl = new NumericUpDownExt();
    valueControl.Minimum = new decimal(0);
    valueControl.Maximum = new decimal(100);
    valueControl.Value = new decimal(50);
    valueControl.Increment = new decimal(10);
    valueControl.Location = new Point(50, 50);
    
    // Custom Up button
    Button btnUp = new Button();
    btnUp.Text = "Increase (+10)";
    btnUp.Location = new Point(200, 50);
    btnUp.Size = new Size(100, 24);
    btnUp.Click += (s, e) => valueControl.UpButton();
    
    // Custom Down button
    Button btnDown = new Button();
    btnDown.Text = "Decrease (-10)";
    btnDown.Location = new Point(310, 50);
    btnDown.Size = new Size(100, 24);
    btnDown.Click += (s, e) => valueControl.DownButton();
    
    this.Controls.Add(valueControl);
    this.Controls.Add(btnUp);
    this.Controls.Add(btnDown);
}
```

**Result:** External buttons that control the NumericUpDownExt value.

## Constraint Scenarios

### Boundary Value Handling

```csharp
private void DemonstrateBoundaries()
{
    NumericUpDownExt control = new NumericUpDownExt();
    control.Minimum = new decimal(10);
    control.Maximum = new decimal(90);
    control.Value = new decimal(50);
    control.Increment = new decimal(5);
    
    // At maximum boundary
    control.Value = new decimal(87);
    control.UpButton(); // Value becomes 90 (Maximum)
    control.UpButton(); // Value stays at 90 (clamped)
    
    // At minimum boundary
    control.Value = new decimal(13);
    control.DownButton(); // Value becomes 10 (Minimum)
    control.DownButton(); // Value stays at 10 (clamped)
}
```

**Result:** Values are automatically clamped to Min/Max boundaries.

### Zero Crossing

```csharp
// Crossing zero boundary
NumericUpDownExt zeroControl = new NumericUpDownExt();
zeroControl.Minimum = new decimal(-100);
zeroControl.Maximum = new decimal(100);
zeroControl.Increment = new decimal(10);
zeroControl.Value = new decimal(-5);

zeroControl.UpButton(); // Value becomes 5 (crossed zero)
zeroControl.DownButton(); // Value becomes -5 (crossed zero again)
```

**Result:** Smooth transition across zero.

## Validation Patterns

### Range Validation

```csharp
private bool ValidateValueRange(NumericUpDownExt control, decimal min, decimal max)
{
    if (control.Value < min || control.Value > max)
    {
        MessageBox.Show(
            $"Value must be between {min} and {max}",
            "Validation Error",
            MessageBoxButtons.OK,
            MessageBoxIcon.Warning);
        
        // Reset to safe value
        control.Value = min;
        return false;
    }
    return true;
}
```

### Multiple Control Validation

```csharp
private void SetupMinMaxValidation()
{
    NumericUpDownExt minControl = new NumericUpDownExt();
    NumericUpDownExt maxControl = new NumericUpDownExt();
    
    minControl.Minimum = new decimal(0);
    minControl.Maximum = new decimal(1000);
    minControl.Value = new decimal(10);
    
    maxControl.Minimum = new decimal(0);
    maxControl.Maximum = new decimal(1000);
    maxControl.Value = new decimal(100);
    
    // Ensure min <= max
    minControl.ValueChanged += (s, e) =>
    {
        if (minControl.Value > maxControl.Value)
        {
            maxControl.Value = minControl.Value;
            Console.WriteLine("Max adjusted to match Min");
        }
    };
    
    maxControl.ValueChanged += (s, e) =>
    {
        if (maxControl.Value < minControl.Value)
        {
            minControl.Value = maxControl.Value;
            Console.WriteLine("Min adjusted to match Max");
        }
    };
}
```

**Result:** Maintains logical relationship between min and max values.

## Common Use Cases

### Age Input
```csharp
NumericUpDownExt ageControl = new NumericUpDownExt();
ageControl.Minimum = new decimal(0);
ageControl.Maximum = new decimal(120);
ageControl.Value = new decimal(18);
ageControl.DecimalPlaces = 0;
```

### Temperature Control
```csharp
NumericUpDownExt tempControl = new NumericUpDownExt();
tempControl.Minimum = new decimal(-40);
tempControl.Maximum = new decimal(120);
tempControl.DecimalPlaces = 1;
tempControl.Increment = new decimal(0.5M);
```

### Discount Percentage
```csharp
NumericUpDownExt discountControl = new NumericUpDownExt();
discountControl.Minimum = new decimal(0);
discountControl.Maximum = new decimal(75);
discountControl.DecimalPlaces = 0;
discountControl.Increment = new decimal(5);
discountControl.Value = new decimal(10);
```
