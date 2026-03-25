# Events & Interactions

## Table of Contents
- [BindableValueChanged Event](#bindablevaluechanged-event)
- [IntegerValueChanged Event](#integervaluechanged-event)
- [ValidationError Event](#validationerror-event)
- [Other Events](#other-events)
- [Event Patterns](#event-patterns)

---

## BindableValueChanged Event

Triggered when the **BindableValue** property changes. This includes null assignments.

### Purpose

Use this event when you need to respond to value changes that may include null values. BindableValue is a wrapper that allows null-safe operations.

### Event Handler

```csharp
private void integerTextBox1_BindableValueChanged(object sender, EventArgs e)
{
    Console.WriteLine("BindableValueChanged event is raised");
}
```

### Wiring the Event

```csharp
integerTextBox1.BindableValueChanged += integerTextBox1_BindableValueChanged;

// Or using lambda:
integerTextBox1.BindableValueChanged += (sender, e) =>
{
    Console.WriteLine("Value changed");
};
```

### Complete Example

```csharp
private void SetupNullableValueTracking()
{
    integerTextBox1.BindableValueChanged += (sender, e) =>
    {
        if (integerTextBox1.BindableValue == null)
        {
            statusLabel.Text = "Value is empty";
            summaryLabel.Text = "No data";
        }
        else
        {
            int value = (int)integerTextBox1.BindableValue;
            statusLabel.Text = $"Current: {value}";
            summaryLabel.Text = $"Entered value: {value}";
        }
    };
}
```

---

## IntegerValueChanged Event

Triggered when the **IntegerValue** property changes. This represents the actual integer value only (never null).

### Purpose

Use this event for numeric calculations, validations, and business logic tied to integer values.

### Event Handler

```csharp
private void integerTextBox1_IntegerValueChanged(object sender, EventArgs e)
{
    Console.WriteLine("IntegerValueChanged event is raised");
}
```

### Wiring the Event

```csharp
integerTextBox1.IntegerValueChanged += integerTextBox1_IntegerValueChanged;

// Or using lambda:
integerTextBox1.IntegerValueChanged += (sender, e) =>
{
    int newValue = integerTextBox1.IntegerValue;
    Console.WriteLine($"New integer value: {newValue}");
};
```

### Complete Example with Calculations

```csharp
private void SetupCalculations()
{
    quantityTextBox.IntegerValueChanged += (sender, e) =>
    {
        int quantity = quantityTextBox.IntegerValue;
        int unitPrice = 100;
        int total = quantity * unitPrice;
        
        totalTextBox.IntegerValue = total;
        Console.WriteLine($"Quantity: {quantity}, Total: {total}");
    };
}
```

---

## ValidationError Event

Triggered when the user enters an invalid value (outside min/max range or invalid format).

### Purpose

Use this event to:
- Alert users about invalid input
- Log validation failures
- Provide specific error messages
- Implement custom validation logic

### Event Handler

```csharp
private void integerTextBox1_ValidationError(object sender, EventArgs e)
{
    Console.WriteLine("ValidationError event is raised");
}
```

### Wiring the Event

```csharp
integerTextBox1.ValidationError += integerTextBox1_ValidationError;

// Or using lambda:
integerTextBox1.ValidationError += (sender, e) =>
{
    MessageBox.Show("Invalid input! Please enter a valid integer.");
};
```

### Complete Example with User Feedback

```csharp
private void SetupValidationFeedback()
{
    ageTextBox.MinValue = 0;
    ageTextBox.MaxValue = 150;
    
    ageTextBox.ValidationError += (sender, e) =>
    {
        errorLabel.Text = "Please enter an age between 0 and 150";
        errorLabel.ForeColor = System.Drawing.Color.Red;
        
        // Revert to previous valid value
        ageTextBox.IntegerValue = 0;
    };
}
```

### Validation with Logging

```csharp
private void SetupValidationLogging()
{
    priceTextBox.MinValue = 0;
    priceTextBox.MaxValue = 999999;
    
    priceTextBox.ValidationError += (sender, e) =>
    {
        string errorMsg = $"Invalid price entry. Min: {priceTextBox.MinValue}, Max: {priceTextBox.MaxValue}";
        LogError(errorMsg);
        Console.WriteLine(errorMsg);
    };
}

private void LogError(string message)
{
    // Write to log file or event viewer
    System.IO.File.AppendAllText("errors.log", 
        $"{DateTime.Now}: {message}{Environment.NewLine}");
}
```

---

## Other Events

### FormattedTextChanged

Triggered when the **FormattedText** property changes (display text with formatting applied).

```csharp
integerTextBox1.FormattedTextChanged += (sender, e) =>
{
    Console.WriteLine($"Formatted text: {integerTextBox1.FormattedText}");
};
```

### ClipTextChanged

Triggered when the **ClipText** property changes (display text without formatting).

```csharp
integerTextBox1.ClipTextChanged += (sender, e) =>
{
    // ClipText is the raw numeric value as a string
    string rawText = integerTextBox1.ClipText;
    Console.WriteLine($"Raw value: {rawText}");
};
```

### SetNull

Triggered when the value is explicitly set to null (via BindableValue).

```csharp
integerTextBox1.SetNull += (sender, e) =>
{
    Console.WriteLine("Value cleared to null");
};
```

---

## Event Patterns

### Pattern 1: Live Validation Display

Show validation messages as the user types:

```csharp
private void SetupLiveValidation()
{
    amountTextBox.MinValue = 0;
    amountTextBox.MaxValue = 100000;
    
    // On valid change
    amountTextBox.IntegerValueChanged += (sender, e) =>
    {
        int amount = amountTextBox.IntegerValue;
        validationLabel.Text = $"✓ Valid amount: {amount}";
        validationLabel.ForeColor = System.Drawing.Color.Green;
    };
    
    // On invalid input
    amountTextBox.ValidationError += (sender, e) =>
    {
        validationLabel.Text = "✗ Invalid amount (must be 0-100,000)";
        validationLabel.ForeColor = System.Drawing.Color.Red;
    };
}
```

### Pattern 2: Cascading Updates

Update related controls when value changes:

```csharp
private void SetupCascadingUpdates()
{
    // When height changes, update area
    heightTextBox.IntegerValueChanged += (sender, e) =>
    {
        int height = heightTextBox.IntegerValue;
        int width = widthTextBox.IntegerValue;
        int area = height * width;
        areaTextBox.IntegerValue = area;
    };
    
    // When width changes, update area
    widthTextBox.IntegerValueChanged += (sender, e) =>
    {
        int height = heightTextBox.IntegerValue;
        int width = widthTextBox.IntegerValue;
        int area = height * width;
        areaTextBox.IntegerValue = area;
    };
}
```

### Pattern 3: Nullable with Default

Use BindableValueChanged to apply defaults:

```csharp
private void SetupNullableWithDefault()
{
    quantityTextBox.BindableValue = null;
    
    quantityTextBox.BindableValueChanged += (sender, e) =>
    {
        if (quantityTextBox.BindableValue == null)
        {
            // Apply default
            quantityTextBox.IntegerValue = 1;
        }
        else
        {
            int qty = (int)quantityTextBox.BindableValue;
            CalculateOrderTotal(qty);
        }
    };
}
```

### Pattern 4: Status Tracking

Track input state and provide feedback:

```csharp
private void SetupStatusTracking()
{
    scoreTextBox.MinValue = 0;
    scoreTextBox.MaxValue = 100;
    
    scoreTextBox.IntegerValueChanged += (sender, e) =>
    {
        int score = scoreTextBox.IntegerValue;
        
        if (score >= 90)
            statusLabel.Text = "Excellent";
        else if (score >= 75)
            statusLabel.Text = "Good";
        else if (score >= 60)
            statusLabel.Text = "Satisfactory";
        else
            statusLabel.Text = "Needs Improvement";
    };
    
    scoreTextBox.ValidationError += (sender, e) =>
    {
        statusLabel.Text = "Invalid Score";
        statusLabel.ForeColor = System.Drawing.Color.Red;
    };
}
```

### Pattern 5: Range-Based Warnings

Warn when values approach limits:

```csharp
private void SetupRangeWarnings()
{
    budgetTextBox.MinValue = 0;
    budgetTextBox.MaxValue = 100000;
    
    budgetTextBox.IntegerValueChanged += (sender, e) =>
    {
        int budget = budgetTextBox.IntegerValue;
        int maxBudget = 100000;
        double percentUsed = (double)budget / maxBudget * 100;
        
        if (percentUsed > 90)
        {
            warningLabel.Text = "⚠ Budget at 90%+";
            warningLabel.ForeColor = System.Drawing.Color.OrangeRed;
        }
        else if (percentUsed > 75)
        {
            warningLabel.Text = "⚠ Budget at 75%+";
            warningLabel.ForeColor = System.Drawing.Color.Orange;
        }
        else
        {
            warningLabel.Text = "✓ Within limits";
            warningLabel.ForeColor = System.Drawing.Color.Green;
        }
    };
}
```

---

## Complete Event Setup Example

Here's a production-ready event setup:

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class InventoryForm : Form
{
    private IntegerTextBox quantityTextBox;
    private Label statusLabel;
    
    public InventoryForm()
    {
        InitializeComponent();
        SetupQuantityTextBox();
    }
    
    private void SetupQuantityTextBox()
    {
        quantityTextBox = new IntegerTextBox();
        quantityTextBox.MinValue = 1;
        quantityTextBox.MaxValue = 1000;
        
        // Valid input
        quantityTextBox.IntegerValueChanged += (sender, e) =>
        {
            int quantity = quantityTextBox.IntegerValue;
            statusLabel.Text = $"Stock: {quantity} units";
            statusLabel.ForeColor = System.Drawing.Color.Green;
        };
        
        // Invalid input
        quantityTextBox.ValidationError += (sender, e) =>
        {
            statusLabel.Text = "Invalid quantity (1-1000)";
            statusLabel.ForeColor = System.Drawing.Color.Red;
        };
        
        // Null handling
        quantityTextBox.SetNull += (sender, e) =>
        {
            statusLabel.Text = "Quantity cleared";
            statusLabel.ForeColor = System.Drawing.Color.Gray;
        };
    }
}
```

---

## Key Takeaways

- **BindableValueChanged** - For nullable value tracking
- **IntegerValueChanged** - For numeric calculations and business logic
- **ValidationError** - For invalid input feedback
- **FormattedTextChanged / ClipTextChanged** - For display-level tracking
- **SetNull** - For null assignment detection
- Combine events for comprehensive form feedback
- Use lambdas for inline event logic
- Always validate before performing calculations
