# Events and Data Binding in PercentTextBox

## Table of Contents
- [Value Change Events](#value-change-events)
- [BindablePercentValueChanged Event](#bindablepercentvaluechanged-event)
- [BindableValueChanged Event](#bindablevaluechanged-event)
- [DoubleValueChanged Event](#doublevaluechanged-event)
- [FormattedTextChanged Event](#formattedtextchanged-event)
- [Data Binding Patterns](#data-binding-patterns)
- [Event Handler Examples](#event-handler-examples)
- [Complete Binding Scenario](#complete-binding-scenario)

## Value Change Events

PercentTextBox provides multiple events for responding to value changes. Choose the right event for your use case:

| Event | Fires When | Use For |
|-------|-----------|---------|
| `BindablePercentValueChanged` | BindablePercentValue changes | Percentage data binding |
| `BindableValueChanged` | BindableValue changes | Decimal data binding |
| `DoubleValueChanged` | DoubleValue changes | Decimal calculations |
| `FormattedTextChanged` | Formatted display text changes | UI updates, display-related logic |

## BindablePercentValueChanged Event

### Purpose

Fires when the `BindablePercentValue` property changes (supports null values).

**Best for:** Data binding to percentage fields, handling nullable values

### Event Signature

```csharp
percentTextBox1.BindablePercentValueChanged += (sender, e) =>
{
    // Handle value change
};
```

### Basic Usage

```csharp
percentTextBox1.BindablePercentValueChanged += (sender, e) =>
{
    if (percentTextBox1.BindablePercentValue.HasValue)
    {
        double percent = percentTextBox1.BindablePercentValue.Value;
        Console.WriteLine($"New percent: {percent}%");
    }
    else
    {
        Console.WriteLine("Value cleared");
    }
};
```

### Example: Validation on Change

```csharp
percentTextBox1.MinValue = 0;
percentTextBox1.MaxValue = 100;

percentTextBox1.BindablePercentValueChanged += (sender, e) =>
{
    var newValue = percentTextBox1.BindablePercentValue;
    
    if (!newValue.HasValue)
    {
        statusLabel.Text = "Empty";
        return;
    }

    if (newValue.Value >= 80)
    {
        statusLabel.Text = "High (≥80%)";
        statusLabel.ForeColor = Color.Green;
    }
    else if (newValue.Value >= 50)
    {
        statusLabel.Text = "Medium (50-80%)";
        statusLabel.ForeColor = Color.Orange;
    }
    else
    {
        statusLabel.Text = "Low (<50%)";
        statusLabel.ForeColor = Color.Red;
    }
};
```

### Example: Update Related Field

```csharp
// When discount percentage changes, calculate new price
pricePercentBox.BindablePercentValueChanged += (sender, e) =>
{
    if (!pricePercentBox.BindablePercentValue.HasValue)
        return;

    double discountPercent = pricePercentBox.BindablePercentValue.Value;
    double originalPrice = 100.0;
    double discountedPrice = originalPrice * (1 - discountPercent / 100);
    
    resultLabel.Text = $"Price: ${discountedPrice:F2}";
};
```

## BindableValueChanged Event

### Purpose

Fires when the `BindableValue` property changes (decimal format 0-1, supports null).

**Best for:** Data binding to decimal/fractional rate fields

### Basic Usage

```csharp
percentTextBox1.BindableValueChanged += (sender, e) =>
{
    if (percentTextBox1.BindableValue.HasValue)
    {
        double decimalValue = percentTextBox1.BindableValue.Value;
        Console.WriteLine($"New decimal: {decimalValue}");
    }
};
```

### Example: Tax Rate Calculation

```csharp
taxRateBox.BindableValueChanged += (sender, e) =>
{
    if (!taxRateBox.BindableValue.HasValue)
        return;

    decimal amount = 100m;
    double taxRate = taxRateBox.BindableValue.Value;  // e.g., 0.08 for 8%
    decimal tax = (decimal)(amount * taxRate);
    
    Console.WriteLine($"Tax on ${amount}: ${tax:F2}");
};
```

## DoubleValueChanged Event

### Purpose

Fires when the `DoubleValue` property changes (decimal format 0-1, non-nullable).

**Best for:** Pure mathematical/calculation operations, strict decimal requirements

### Basic Usage

```csharp
percentTextBox1.DoubleValueChanged += (sender, e) =>
{
    double decimalValue = percentTextBox1.DoubleValue;
    Console.WriteLine($"Double value: {decimalValue}");
};
```

### Example: Multiplier Calculation

```csharp
growthBox.DoubleValueChanged += (sender, e) =>
{
    double multiplier = 1 + growthBox.DoubleValue;  // Convert to multiplier
    double baseAmount = 1000;
    double result = baseAmount * multiplier;
    
    resultLabel.Text = $"${result:F2}";
};
```

## FormattedTextChanged Event

### Purpose

Fires when the formatted display text (with percent symbol and separators) changes.

**Best for:** Display-related updates, UI synchronization

### Basic Usage

```csharp
percentTextBox1.FormattedTextChanged += (sender, e) =>
{
    string displayText = percentTextBox1.FormattedText;
    Console.WriteLine($"Formatted display: {displayText}");
};
```

### Example: Show Formatted Value in Label

```csharp
percentTextBox1.FormattedTextChanged += (sender, e) =>
{
    string formatted = percentTextBox1.FormattedText;
    valueDisplay.Text = $"Current: {formatted}";
};
```

## Data Binding Patterns

### Pattern 1: Basic Binding with BindablePercentValue

```csharp
public class SalesData
{
    public double? CommissionRate { get; set; }
}

// Setup
var data = new SalesData { CommissionRate = 15.5 };
percentTextBox1.DataBindings.Add(
    "BindablePercentValue",
    data,
    "CommissionRate"
);

// Result:
// - User edits in PercentTextBox → CommissionRate updates
// - Code changes CommissionRate → PercentTextBox updates
```

### Pattern 2: Binding with BindableValue (Decimal Format)

```csharp
public class TaxSettings
{
    public double? Rate { get; set; }  // 0-1 format
}

// Setup
var settings = new TaxSettings { Rate = 0.08 };
percentTextBox1.DataBindings.Add(
    "BindableValue",
    settings,
    "Rate"
);

// Result:
// - User enters 8% → Rate becomes 0.08
// - Rate becomes 0.12 → PercentTextBox shows 12%
```

### Pattern 3: Binding with Converter

For complex scenarios, use a binding source with a converter:

```csharp
public class PercentageConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value is double d)
            return d * 100;
        return 0;
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value is double d)
            return d / 100;
        return 0;
    }
}

// Note: IValueConverter is for binding scenarios needing transformation
```

### Pattern 4: Two-Way Data Binding with Validation

```csharp
public class Product
{
    private double? discountPercent;
    
    public double? DiscountPercent
    {
        get { return discountPercent; }
        set
        {
            if (value.HasValue && (value < 0 || value > 100))
                throw new ArgumentException("Discount must be 0-100%");
            discountPercent = value;
        }
    }
}

var product = new Product();
percentTextBox1.DataBindings.Add("BindablePercentValue", product, "DiscountPercent");
```

## Event Handler Examples

### Example 1: Synchronized Fields

```csharp
private void SetupSynchronizedFields()
{
    // When discount changes, update total
    discountBox.BindablePercentValueChanged += (sender, e) =>
    {
        UpdateTotalPrice();
    };

    taxBox.BindablePercentValueChanged += (sender, e) =>
    {
        UpdateTotalPrice();
    };
}

private void UpdateTotalPrice()
{
    decimal basePrice = 100m;
    double discount = discountBox.BindablePercentValue ?? 0;
    double tax = taxBox.BindablePercentValue ?? 0;
    
    decimal priceAfterDiscount = (decimal)(basePrice * (1 - discount / 100));
    decimal finalPrice = (decimal)(priceAfterDiscount * (1 + tax / 100));
    
    totalLabel.Text = $"${finalPrice:F2}";
}
```

### Example 2: Progress Indication

```csharp
percentTextBox1.BindablePercentValueChanged += (sender, e) =>
{
    if (percentTextBox1.BindablePercentValue.HasValue)
    {
        double percent = percentTextBox1.BindablePercentValue.Value;
        progressBar.Value = (int)Math.Min(percent, 100);
        progressLabel.Text = $"{percent:F1}% complete";
    }
};
```

### Example 3: Conditional UI Updates

```csharp
percentTextBox1.BindablePercentValueChanged += (sender, e) =>
{
    var value = percentTextBox1.BindablePercentValue;
    
    if (!value.HasValue)
    {
        statusPanel.BackColor = Color.LightGray;
        statusLabel.Text = "Not set";
    }
    else if (value >= 100)
    {
        statusPanel.BackColor = Color.LimeGreen;
        statusLabel.Text = "Complete";
    }
    else if (value >= 50)
    {
        statusPanel.BackColor = Color.Gold;
        statusLabel.Text = "In Progress";
    }
    else
    {
        statusPanel.BackColor = Color.IndianRed;
        statusLabel.Text = "Not Started";
    }
};
```

### Example 4: Data Persistence

```csharp
percentTextBox1.BindablePercentValueChanged += (sender, e) =>
{
    var value = percentTextBox1.BindablePercentValue;
    
    // Save to database
    using (var db = new AppContext())
    {
        var record = db.Records.Find(recordId);
        record.Percentage = value;
        db.SaveChanges();
    }
};
```

## Complete Binding Scenario

### Full Example: Discount Calculator Form

```csharp
public class DiscountCalculatorForm : Form
{
    private NumericUpDown originalPriceBox;
    private PercentTextBox discountBox;
    private Label originalLabel, discountLabel, finalLabel;

    public DiscountCalculatorForm()
    {
        InitializeComponents();
        WireUpEvents();
    }

    private void InitializeComponents()
    {
        // Original price
        originalPriceBox = new NumericUpDown
        {
            Minimum = 0,
            Maximum = 10000,
            DecimalPlaces = 2,
            Value = 100
        };

        // Discount percentage
        discountBox = new PercentTextBox
        {
            MinValue = 0,
            MaxValue = 100,
            PercentDecimalDigits = 2,
            PercentValue = 0
        };

        // Labels
        originalLabel = new Label { Text = "Original: $100.00" };
        discountLabel = new Label { Text = "Discount: 0.00%" };
        finalLabel = new Label { Text = "Final: $100.00" };

        this.Controls.AddRange(new Control[]
        {
            new Label { Text = "Price:" }, originalPriceBox,
            new Label { Text = "Discount:" }, discountBox,
            originalLabel, discountLabel, finalLabel
        });
    }

    private void WireUpEvents()
    {
        // Recalculate when price changes
        originalPriceBox.ValueChanged += (s, e) => UpdateDisplay();

        // Recalculate when discount changes
        discountBox.BindablePercentValueChanged += (s, e) => UpdateDisplay();
    }

    private void UpdateDisplay()
    {
        decimal original = originalPriceBox.Value;
        double? discount = discountBox.BindablePercentValue;

        originalLabel.Text = $"Original: ${original:F2}";
        
        if (discount.HasValue)
        {
            discountLabel.Text = $"Discount: {discount:F2}%";
            decimal discountAmount = (decimal)(original * discount.Value / 100);
            decimal final = original - discountAmount;
            finalLabel.Text = $"Final: ${final:F2}";
        }
        else
        {
            discountLabel.Text = "Discount: (not set)";
            finalLabel.Text = $"Final: ${original:F2}";
        }
    }
}
```

### Usage

```csharp
// Create and run the form
var form = new DiscountCalculatorForm();
Application.Run(form);

// User enters price, adjusts discount percentage, sees updated final price
```

---

**Next:** See the complete API reference in [properties-and-api-reference.md](properties-and-api-reference.md)
