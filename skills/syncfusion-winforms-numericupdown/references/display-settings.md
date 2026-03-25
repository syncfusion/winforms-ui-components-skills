# Display Settings for NumericUpDownExt

Complete guide to formatting and displaying numeric values in the NumericUpDownExt control.

## Overview

Display settings control how numeric values appear to users. These settings include decimal precision, thousands separators, and culture-specific formatting options. Proper display configuration enhances readability and user experience.

## DecimalPlaces Property

The `DecimalPlaces` property sets the number of decimal places to display in the control.

**Type:** `int`  
**Default:** `0`  
**Range:** `0` to `99`

### When to Use
- Currency inputs requiring cent precision (2 decimal places)
- Percentage values with decimal precision (1-2 decimal places)
- Scientific measurements requiring high precision (3+ decimal places)
- Integer-only inputs (0 decimal places)

### Basic DecimalPlaces Configuration

```csharp
using Syncfusion.Windows.Forms.Tools;

// Display 2 decimal places
numericUpDownExt1.DecimalPlaces = 2;
numericUpDownExt1.Value = new decimal(123.456M);
```

**Result:** Displays "123.46" (rounded to 2 decimal places).

### Integer Display (No Decimals)

```csharp
// Integer-only display
NumericUpDownExt quantityControl = new NumericUpDownExt();
quantityControl.DecimalPlaces = 0;
quantityControl.Minimum = new decimal(1);
quantityControl.Maximum = new decimal(999);
quantityControl.Value = new decimal(10);
```

**Result:** Displays whole numbers only, e.g., "10", "25", "100".

### Currency Display

```csharp
// Standard currency format (2 decimal places)
NumericUpDownExt priceControl = new NumericUpDownExt();
priceControl.DecimalPlaces = 2;
priceControl.Minimum = new decimal(0);
priceControl.Maximum = new decimal(999999.99M);
priceControl.Value = new decimal(49.99M);
priceControl.ThousandsSeparator = true;
```

**Result:** Displays "49.99" for currency values.

### High-Precision Scientific Values

```csharp
// Scientific measurement (4 decimal places)
NumericUpDownExt precisionControl = new NumericUpDownExt();
precisionControl.DecimalPlaces = 4;
precisionControl.Minimum = new decimal(0);
precisionControl.Maximum = new decimal(1000);
precisionControl.Value = new decimal(3.1416M);
precisionControl.Increment = new decimal(0.0001M);
```

**Result:** Displays "3.1416" with high precision.

### Percentage with Decimal

```csharp
// Percentage with one decimal place
NumericUpDownExt percentControl = new NumericUpDownExt();
percentControl.DecimalPlaces = 1;
percentControl.Minimum = new decimal(0);
percentControl.Maximum = new decimal(100);
percentControl.Value = new decimal(75.5M);
percentControl.Increment = new decimal(0.1M);
```

**Result:** Displays "75.5" for percentage values.

### Complete Price Input Example

```csharp
private void SetupPriceDisplay()
{
    NumericUpDownExt priceInput = new NumericUpDownExt();
    
    // Price formatting
    priceInput.DecimalPlaces = 2;
    priceInput.ThousandsSeparator = true;
    priceInput.Minimum = new decimal(0);
    priceInput.Maximum = new decimal(9999.99M);
    priceInput.Value = new decimal(1299.99M);
    priceInput.Increment = new decimal(0.01M);
    
    priceInput.Location = new Point(50, 30);
    priceInput.Size = new Size(150, 24);
    
    Label lblPrice = new Label();
    lblPrice.Location = new Point(50, 60);
    lblPrice.Size = new Size(200, 20);
    
    // Display formatted price
    priceInput.ValueChanged += (s, e) =>
    {
        lblPrice.Text = $"Price: ${priceInput.Value:F2}";
    };
    
    lblPrice.Text = $"Price: ${priceInput.Value:F2}";
    
    this.Controls.Add(priceInput);
    this.Controls.Add(lblPrice);
}
```

**Result:** Professional price input with formatted currency display.

### Rounding Behavior

```csharp
// Demonstrate rounding with DecimalPlaces
NumericUpDownExt roundingControl = new NumericUpDownExt();
roundingControl.DecimalPlaces = 2;

roundingControl.Value = new decimal(123.456M); // Rounds to 123.46
roundingControl.Value = new decimal(123.454M); // Rounds to 123.45
roundingControl.Value = new decimal(123.455M); // Rounds to 123.46 (banker's rounding)
```

**Result:** Values are rounded according to standard rounding rules.

## ThousandsSeparator Property

The `ThousandsSeparator` property determines whether thousands separators (commas) are displayed.

**Type:** `bool`  
**Default:** `false`

### When to Use
- Large numeric values (thousands, millions)
- Financial data and currency
- Population or statistical figures
- Any value where readability is improved by grouping digits

### Basic ThousandsSeparator Usage

```csharp
// Enable thousands separator
numericUpDownExt1.ThousandsSeparator = true;
numericUpDownExt1.Value = new decimal(1234567);
```

**Result:** Displays "1,234,567" instead of "1234567".

### Currency with Thousands Separator

```csharp
// Large currency values
NumericUpDownExt salaryControl = new NumericUpDownExt();
salaryControl.Minimum = new decimal(0);
salaryControl.Maximum = new decimal(999999.99M);
salaryControl.DecimalPlaces = 2;
salaryControl.ThousandsSeparator = true;
salaryControl.Value = new decimal(75000.00M);
```

**Result:** Displays "75,000.00" for easy reading.

### Population Counter

```csharp
// Population/statistics display
NumericUpDownExt populationControl = new NumericUpDownExt();
populationControl.Minimum = new decimal(0);
populationControl.Maximum = new decimal(999999999);
populationControl.DecimalPlaces = 0;
populationControl.ThousandsSeparator = true;
populationControl.Value = new decimal(5000000);
```

**Result:** Displays "5,000,000" for better readability.

### Combining DecimalPlaces and ThousandsSeparator

```csharp
private void SetupFormattedDisplay()
{
    NumericUpDownExt financialControl = new NumericUpDownExt();
    
    // Complete formatting setup
    financialControl.DecimalPlaces = 2;
    financialControl.ThousandsSeparator = true;
    financialControl.Minimum = new decimal(0);
    financialControl.Maximum = new decimal(9999999.99M);
    financialControl.Value = new decimal(123456.78M);
    
    financialControl.Location = new Point(50, 30);
    
    this.Controls.Add(financialControl);
}
```

**Result:** Displays "123,456.78" with both formatting options.

### Without Thousands Separator (Comparison)

```csharp
// Disable thousands separator for compact display
numericUpDownExt1.ThousandsSeparator = false;
numericUpDownExt1.Value = new decimal(1234567);
```

**Result:** Displays "1234567" (more compact but less readable).

## Display Formatting Options

### Currency Formatting Example

```csharp
private void CreateCurrencyInput()
{
    NumericUpDownExt currencyInput = new NumericUpDownExt();
    
    // Currency formatting
    currencyInput.DecimalPlaces = 2;
    currencyInput.ThousandsSeparator = true;
    currencyInput.Minimum = new decimal(0);
    currencyInput.Maximum = new decimal(999999.99M);
    currencyInput.Value = new decimal(0);
    currencyInput.Increment = new decimal(0.01M);
    
    currencyInput.Location = new Point(100, 50);
    currencyInput.Size = new Size(150, 24);
    
    Label lblCurrency = new Label();
    lblCurrency.Text = "Amount ($):";
    lblCurrency.Location = new Point(20, 53);
    lblCurrency.AutoSize = true;
    
    this.Controls.Add(lblCurrency);
    this.Controls.Add(currencyInput);
}
```

**Result:** Professional currency input control with proper formatting.

### Percentage Formatting Example

```csharp
private void CreatePercentageInput()
{
    NumericUpDownExt percentInput = new NumericUpDownExt();
    
    // Percentage formatting (0-100% with 1 decimal)
    percentInput.DecimalPlaces = 1;
    percentInput.ThousandsSeparator = false; // Not needed for percentages
    percentInput.Minimum = new decimal(0);
    percentInput.Maximum = new decimal(100);
    percentInput.Value = new decimal(50.0M);
    percentInput.Increment = new decimal(0.1M);
    
    percentInput.Location = new Point(100, 50);
    percentInput.Size = new Size(120, 24);
    
    Label lblPercent = new Label();
    lblPercent.Text = "Discount (%):";
    lblPercent.Location = new Point(10, 53);
    lblPercent.AutoSize = true;
    
    Label lblResult = new Label();
    lblResult.Location = new Point(230, 53);
    lblResult.Size = new Size(100, 20);
    
    percentInput.ValueChanged += (s, e) =>
    {
        lblResult.Text = $"{percentInput.Value}%";
    };
    
    lblResult.Text = $"{percentInput.Value}%";
    
    this.Controls.Add(lblPercent);
    this.Controls.Add(percentInput);
    this.Controls.Add(lblResult);
}
```

**Result:** Percentage input with live display of formatted value.

### Measurement Input Example

```csharp
private void CreateMeasurementInput()
{
    // Distance measurement (meters with 3 decimal places)
    NumericUpDownExt distanceInput = new NumericUpDownExt();
    
    distanceInput.DecimalPlaces = 3;
    distanceInput.ThousandsSeparator = true;
    distanceInput.Minimum = new decimal(0);
    distanceInput.Maximum = new decimal(99999.999M);
    distanceInput.Value = new decimal(1000.500M);
    distanceInput.Increment = new decimal(0.001M);
    
    distanceInput.Location = new Point(100, 50);
    distanceInput.Size = new Size(150, 24);
    
    Label lblDistance = new Label();
    lblDistance.Text = "Distance (m):";
    lblDistance.Location = new Point(10, 53);
    lblDistance.AutoSize = true;
    
    this.Controls.Add(lblDistance);
    this.Controls.Add(distanceInput);
}
```

**Result:** Precise measurement input with 3 decimal places.

## Scientific Notation Considerations

While NumericUpDownExt doesn't natively support scientific notation display, you can handle very large or small numbers:

### Very Large Numbers

```csharp
// Handle large numbers with thousands separator
NumericUpDownExt largeNumberControl = new NumericUpDownExt();
largeNumberControl.Minimum = new decimal(0);
largeNumberControl.Maximum = decimal.MaxValue;
largeNumberControl.ThousandsSeparator = true;
largeNumberControl.DecimalPlaces = 0;
largeNumberControl.Value = new decimal(123456789);

// Display in label with scientific notation if needed
Label scientificLabel = new Label();
scientificLabel.Location = new Point(50, 90);
scientificLabel.Size = new Size(200, 20);

largeNumberControl.ValueChanged += (s, e) =>
{
    decimal value = largeNumberControl.Value;
    if (value >= 1000000)
    {
        scientificLabel.Text = $"Scientific: {value:E2}";
    }
    else
    {
        scientificLabel.Text = $"Standard: {value}";
    }
};
```

**Result:** Large numbers display with thousands separator, with scientific notation option in label.

### Very Small Decimal Numbers

```csharp
// Very small decimal values (precision measurements)
NumericUpDownExt microControl = new NumericUpDownExt();
microControl.Minimum = new decimal(0);
microControl.Maximum = new decimal(1);
microControl.DecimalPlaces = 6; // Microseconds, etc.
microControl.Value = new decimal(0.000001M);
microControl.Increment = new decimal(0.000001M);
```

**Result:** Displays very small values with high precision (0.000001).

## Culture-Specific Formatting Notes

NumericUpDownExt respects system culture settings for number formatting.

### Understanding Culture Impact

```csharp
// Different cultures use different separators
// US: 1,234.56 (comma for thousands, period for decimal)
// Europe: 1.234,56 (period for thousands, comma for decimal)

NumericUpDownExt cultureControl = new NumericUpDownExt();
cultureControl.DecimalPlaces = 2;
cultureControl.ThousandsSeparator = true;
cultureControl.Value = new decimal(1234.56M);

// The display will automatically use the system's culture format
```

**Result:** Display format adapts to the system's regional settings.

### Testing with Different Cultures

```csharp
private void DemonstrateCultureFormatting()
{
    NumericUpDownExt control = new NumericUpDownExt();
    control.DecimalPlaces = 2;
    control.ThousandsSeparator = true;
    control.Value = new decimal(12345.67M);
    
    // Display how it appears in different cultures
    Label infoLabel = new Label();
    infoLabel.Location = new Point(50, 90);
    infoLabel.Size = new Size(300, 40);
    infoLabel.Text = $"Current Culture: {System.Globalization.CultureInfo.CurrentCulture.Name}\n" +
                     $"Formatted Value: {control.Value.ToString("N2")}";
    
    this.Controls.Add(control);
    this.Controls.Add(infoLabel);
}
```

**Result:** Displays current culture and how the number is formatted.

### Forcing Specific Culture Format

```csharp
private void FormatWithSpecificCulture()
{
    NumericUpDownExt control = new NumericUpDownExt();
    control.DecimalPlaces = 2;
    control.ThousandsSeparator = true;
    control.Value = new decimal(12345.67M);
    
    Label usFormatLabel = new Label();
    usFormatLabel.Location = new Point(50, 90);
    usFormatLabel.Size = new Size(200, 20);
    
    Label euFormatLabel = new Label();
    euFormatLabel.Location = new Point(50, 115);
    euFormatLabel.Size = new Size(200, 20);
    
    control.ValueChanged += (s, e) =>
    {
        // US format
        var usCulture = new System.Globalization.CultureInfo("en-US");
        usFormatLabel.Text = $"US Format: {control.Value.ToString("N2", usCulture)}";
        
        // European format (German)
        var euCulture = new System.Globalization.CultureInfo("de-DE");
        euFormatLabel.Text = $"EU Format: {control.Value.ToString("N2", euCulture)}";
    };
    
    // Initial display
    var usCulture = new System.Globalization.CultureInfo("en-US");
    usFormatLabel.Text = $"US Format: {control.Value.ToString("N2", usCulture)}";
    
    var euCulture = new System.Globalization.CultureInfo("de-DE");
    euFormatLabel.Text = $"EU Format: {control.Value.ToString("N2", euCulture)}";
    
    this.Controls.Add(control);
    this.Controls.Add(usFormatLabel);
    this.Controls.Add(euFormatLabel);
}
```

**Result:** Shows same value formatted in different cultural conventions.

## Complete Display Configuration Example

```csharp
private void SetupCompleteDisplayDemo()
{
    // Create multiple controls demonstrating different display settings
    
    // 1. Integer display
    NumericUpDownExt integerControl = new NumericUpDownExt();
    integerControl.Location = new Point(150, 30);
    integerControl.Size = new Size(120, 24);
    integerControl.DecimalPlaces = 0;
    integerControl.ThousandsSeparator = false;
    integerControl.Value = new decimal(42);
    
    Label lblInteger = new Label();
    lblInteger.Text = "Integer:";
    lblInteger.Location = new Point(50, 33);
    lblInteger.AutoSize = true;
    
    // 2. Currency display
    NumericUpDownExt currencyControl = new NumericUpDownExt();
    currencyControl.Location = new Point(150, 60);
    currencyControl.Size = new Size(120, 24);
    currencyControl.DecimalPlaces = 2;
    currencyControl.ThousandsSeparator = true;
    currencyControl.Value = new decimal(1234.56M);
    
    Label lblCurrency = new Label();
    lblCurrency.Text = "Currency:";
    lblCurrency.Location = new Point(50, 63);
    lblCurrency.AutoSize = true;
    
    // 3. Percentage display
    NumericUpDownExt percentControl = new NumericUpDownExt();
    percentControl.Location = new Point(150, 90);
    percentControl.Size = new Size(120, 24);
    percentControl.DecimalPlaces = 1;
    percentControl.ThousandsSeparator = false;
    percentControl.Minimum = new decimal(0);
    percentControl.Maximum = new decimal(100);
    percentControl.Value = new decimal(75.5M);
    
    Label lblPercent = new Label();
    lblPercent.Text = "Percentage:";
    lblPercent.Location = new Point(50, 93);
    lblPercent.AutoSize = true;
    
    // 4. Scientific precision
    NumericUpDownExt scientificControl = new NumericUpDownExt();
    scientificControl.Location = new Point(150, 120);
    scientificControl.Size = new Size(120, 24);
    scientificControl.DecimalPlaces = 4;
    scientificControl.ThousandsSeparator = false;
    scientificControl.Value = new decimal(3.1416M);
    
    Label lblScientific = new Label();
    lblScientific.Text = "Scientific:";
    lblScientific.Location = new Point(50, 123);
    lblScientific.AutoSize = true;
    
    // Add all controls
    this.Controls.Add(lblInteger);
    this.Controls.Add(integerControl);
    this.Controls.Add(lblCurrency);
    this.Controls.Add(currencyControl);
    this.Controls.Add(lblPercent);
    this.Controls.Add(percentControl);
    this.Controls.Add(lblScientific);
    this.Controls.Add(scientificControl);
}
```

**Result:** Comprehensive demonstration of different display formatting scenarios.

## Tips and Best Practices

### Choose Appropriate Decimal Places
- **Currency**: Use 2 decimal places
- **Percentages**: Use 0-2 decimal places depending on precision needs
- **Quantities**: Use 0 decimal places
- **Measurements**: Use 1-4 decimal places based on precision requirements

### Use Thousands Separator for Large Numbers
- Enable for values >= 10,000
- Always enable for financial/currency data
- Consider disabling for compact displays or short numbers

### Consider User Experience
- More decimal places = more precision but harder to read
- Thousands separators greatly improve readability of large numbers
- Match formatting to user expectations (currency looks like currency)

### Test with Different Cultures
- Verify display with different regional settings
- Consider international users
- Test with both US and European formats
