# Value Management in PercentTextBox

## Table of Contents
- [Value Types Overview](#value-types-overview)
- [PercentValue Property](#percentvalue-property)
- [DoubleValue Property](#doublevalue-property)
- [BindableValue Property](#bindablevalue-property)
- [BindablePercentValue Property](#bindablepercentvalue-property)
- [DefaultValue Property](#defaultvalue-property)
- [Choosing the Right Value Type](#choosing-the-right-value-type)
- [Value Conversion Examples](#value-conversion-examples)

## Value Types Overview

PercentTextBox provides four different ways to get/set values, each suited for different scenarios:

| Property | Range | Use Case | Data Type |
|----------|-------|----------|-----------|
| `PercentValue` | 0-100+ | Direct percentage | double |
| `DoubleValue` | 0-1+ | Decimal representation | double |
| `BindableValue` | 0-1+ | Nullable decimal binding | double? |
| `BindablePercentValue` | 0-100+ | Nullable percent binding | double? |

## PercentValue Property

**Purpose:** Get/set the value as a percentage (0-100).

### Setting PercentValue

```csharp
// Set to 50%
percentTextBox1.PercentValue = 50;

// Set to 25.5%
percentTextBox1.PercentValue = 25.5;
```

### Getting PercentValue

```csharp
double currentPercent = percentTextBox1.PercentValue;
Console.WriteLine($"Current: {currentPercent}%");
```

### Example: Processing Percentages

```csharp
// Calculate discount
double originalPrice = 100.00;
double discountPercent = percentTextBox1.PercentValue;
double discountAmount = originalPrice * (discountPercent / 100);
double finalPrice = originalPrice - discountAmount;

Console.WriteLine($"Discount: {discountPercent}% = ${discountAmount}");
Console.WriteLine($"Final Price: ${finalPrice}");
```

## DoubleValue Property

**Purpose:** Get/set the value as a decimal (0-1), where 1 represents 100%.

### Setting DoubleValue

```csharp
// Set to 50% (0.5 in decimal form)
percentTextBox1.DoubleValue = 0.5;

// Set to 25% (0.25 in decimal form)
percentTextBox1.DoubleValue = 0.25;
```

### Getting DoubleValue

```csharp
double decimalValue = percentTextBox1.DoubleValue;
Console.WriteLine($"Decimal form: {decimalValue}");
// Output: Decimal form: 0.5
```

### Example: Decimal Calculations

```csharp
// Use decimal form for mathematical calculations
decimal originalAmount = 1000m;
double percentFraction = percentTextBox1.DoubleValue;  // e.g., 0.20
decimal result = (decimal)(originalAmount * percentFraction);

Console.WriteLine($"Amount: ${originalAmount}");
Console.WriteLine($"At {percentFraction * 100}%: ${result}");
```

## BindableValue Property

**Purpose:** Get/set the value as a nullable decimal (0-1) for data binding scenarios.

### Setting BindableValue

```csharp
// Set to 30% (0.3 in decimal form)
percentTextBox1.BindableValue = 0.3;

// Set to null (empty control)
percentTextBox1.BindableValue = null;
```

### Getting BindableValue

```csharp
double? bindableValue = percentTextBox1.BindableValue;

if (bindableValue.HasValue)
{
    Console.WriteLine($"Value: {bindableValue.Value}");
}
else
{
    Console.WriteLine("No value set");
}
```

### Example: Data Binding

```csharp
public class Model
{
    public double? TaxRate { get; set; }  // 0-1 format
}

var model = new Model { TaxRate = 0.08 };
percentTextBox1.DataBindings.Add("BindableValue", model, "TaxRate");

// User changes value in control → model updates
// Model value changes → control updates
```

## BindablePercentValue Property

**Purpose:** Get/set the value as a nullable percentage (0-100) for data binding scenarios.

### Setting BindablePercentValue

```csharp
// Set to 30%
percentTextBox1.BindablePercentValue = 30;

// Set to null (empty control)
percentTextBox1.BindablePercentValue = null;
```

### Getting BindablePercentValue

```csharp
double? percentValue = percentTextBox1.BindablePercentValue;

if (percentValue.HasValue)
{
    Console.WriteLine($"Percentage: {percentValue.Value}%");
}
else
{
    Console.WriteLine("Control is empty");
}
```

### Example: Data Binding

```csharp
public class SalesData
{
    public double? CommissionPercent { get; set; }  // 0-100 format
}

var data = new SalesData { CommissionPercent = 15.5 };
percentTextBox1.DataBindings.Add("BindablePercentValue", data, "CommissionPercent");

// Two-way binding:
// - User enters 20% → CommissionPercent = 20
// - CommissionPercent = 25 → Control displays 25%
```

### Example: Handling Null Values with Events

```csharp
percentTextBox1.AllowNull = true;
percentTextBox1.BindablePercentValueChanged += (sender, e) =>
{
    if (percentTextBox1.BindablePercentValue.HasValue)
    {
        Console.WriteLine($"Value: {percentTextBox1.BindablePercentValue}%");
    }
    else
    {
        Console.WriteLine("User cleared the value");
    }
};
```

## DefaultValue Property

**Purpose:** Specify a default value when the control is reset or initialized.

### Setting DefaultValue

```csharp
// Default to 0%
percentTextBox1.DefaultValue = 0;

// Default to 50%
percentTextBox1.DefaultValue = 50;
```

### Using DefaultValue

```csharp
// The control displays DefaultValue when first created
var box = new PercentTextBox();
box.DefaultValue = 25;  // Will display 25% initially

// Programmatically reset to default
// (Use Binding.Reset() if using data binding)
```

## Choosing the Right Value Type

### Use PercentValue When:
- Working with percentages in UI code
- Displaying/receiving 0-100 values
- Simple percentage calculations
- Form fields expecting percentages

```csharp
// Discount form
double discount = percentTextBox1.PercentValue;  // e.g., 15
```

### Use DoubleValue When:
- Converting for mathematical calculations
- Working with fractional rates (0-1 range)
- Passing to functions expecting decimal form

```csharp
// Tax calculation
double taxRate = percentTextBox1.DoubleValue;  // e.g., 0.08
double tax = amount * taxRate;
```

### Use BindablePercentValue When:
- Binding to data sources
- Working with nullable values
- Supporting null/empty states
- Two-way data binding scenarios

```csharp
// Bind to model
control.DataBindings.Add("BindablePercentValue", model, "CommissionRate");
```

### Use BindableValue When:
- Binding to decimal-format data (0-1)
- Supporting nullable values
- Two-way data binding with fractional rates

```csharp
// Bind to tax rate data
control.DataBindings.Add("BindableValue", data, "TaxRate");
```

## Value Conversion Examples

### Convert Percent to Decimal

```csharp
// If you have a percent value and need decimal
double percentValue = percentTextBox1.PercentValue;  // e.g., 25
double decimalValue = percentValue / 100;  // 0.25

// Or directly use DoubleValue
double directDecimal = percentTextBox1.DoubleValue;  // 0.25
```

### Convert Decimal to Percent

```csharp
// If you have a decimal value and need percent
double decimalValue = 0.75;
double percentValue = decimalValue * 100;  // 75

// Or set DoubleValue and read PercentValue
percentTextBox1.DoubleValue = 0.75;
double retrievedPercent = percentTextBox1.PercentValue;  // 75
```

### Working with Both Types

```csharp
// Set using percent
percentTextBox1.PercentValue = 40;

// Read as decimal
double asDecimal = percentTextBox1.DoubleValue;  // 0.4

// Both refer to the same internal value
double asPercent = percentTextBox1.PercentValue;  // 40
```

## Summary Table

```csharp
// Same value, different access patterns:
percentTextBox1.PercentValue = 50;

// All of these return the same value:
double p = percentTextBox1.PercentValue;              // 50
double d = percentTextBox1.DoubleValue;               // 0.5
double? bp = percentTextBox1.BindablePercentValue;    // 50
double? bd = percentTextBox1.BindableValue;           // 0.5
```

---

**Next:** Learn about validation and constraints in [constraints-and-validation.md](constraints-and-validation.md)
