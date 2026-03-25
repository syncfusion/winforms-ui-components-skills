# Display and Value Handling

## Display TextBox

The Calculator control features a display area at the top that shows all digits and calculation results in real-time. This section covers display management and value handling.

### Show or Hide Display Area

Control whether the display area is visible using the ShowDisplayArea property:

```csharp
// Show display (default)
this.calculatorControl1.ShowDisplayArea = true;

// Hide display area
this.calculatorControl1.ShowDisplayArea = false;
```

```vb
' Show display (default)
Me.calculatorControl1.ShowDisplayArea = True

' Hide display area
Me.calculatorControl1.ShowDisplayArea = False
```

### Display Text Alignment

Align the displayed text within the display area:

```csharp
this.calculatorControl1.DisplayTextAlign = System.Windows.Forms.HorizontalAlignment.Left;
```

```vb
Me.calculatorControl1.DisplayTextAlign = System.Windows.Forms.HorizontalAlignment.Left
```

**Options:**
- `Left` — Text aligned to left edge of display
- `Center` — Text centered in display (uncommon)
- `Right` — Text aligned to right edge (typical calculator display)

### Display Font

Customize the font of the display text:

```csharp
this.calculatorControl1.Font = new System.Drawing.Font("Verdana", 8.25F, System.Drawing.FontStyle.Bold);
```

```vb
Me.calculatorControl1.Font = New System.Drawing.Font("Verdana", 8.25F, System.Drawing.FontStyle.Bold)
```

## Value Handling

The Calculator control manages numeric values internally and provides multiple ways to access and set values.

### Working with Double Values

The DoubleValue property returns the calculator's value as a System.Double:

```csharp
// Get the current double value
double result = this.calculatorControl1.DoubleValue;

// Set the initial value
this.calculatorControl1.DoubleValue = 42.5;
```

```vb
' Get the current double value
Dim result As Double = Me.calculatorControl1.DoubleValue

' Set the initial value
Me.calculatorControl1.DoubleValue = 42.5
```

### Working with Value Object

The Value property returns a CalculatorValue object which provides more control:

```csharp
// Get the value object
CalculatorValue calcValue = this.calculatorControl1.Value;

// Access string representation
string valueString = calcValue.ToString();

// Access double representation
double valueDouble = calcValue.ToDouble();
```

```vb
' Get the value object
Dim calcValue As CalculatorValue = Me.calculatorControl1.Value

' Access string representation
Dim valueString As String = calcValue.ToString()

' Access double representation
Dim valueDouble As Double = calcValue.ToDouble()
```

## Culture and Number Formatting

The Calculator respects culture settings for number formatting, including decimal separators and group separators.

### Set Culture

Configure number formatting based on culture/locale:

```csharp
// Set to United States culture
this.calculatorControl1.Culture = new System.Globalization.CultureInfo("en-US");

// Set to German culture (comma as decimal separator)
this.calculatorControl1.Culture = new System.Globalization.CultureInfo("de-DE");

// Set to French culture
this.calculatorControl1.Culture = new System.Globalization.CultureInfo("fr-FR");
```

```vb
' Set to United States culture
Me.calculatorControl1.Culture = New System.Globalization.CultureInfo("en-US")

' Set to German culture (comma as decimal separator)
Me.calculatorControl1.Culture = New System.Globalization.CultureInfo("de-DE")

' Set to French culture
Me.calculatorControl1.Culture = New System.Globalization.CultureInfo("fr-FR")
```

### Culture-Related Properties

**RepeatAssignAction:** Allow repeating the last operation by pressing "=" multiple times

```csharp
this.calculatorControl1.RepeatAssignAction = true;
```

```vb
Me.calculatorControl1.RepeatAssignAction = True
```

**UseUserOverride:** Respect user's regional settings for number formatting

```csharp
this.calculatorControl1.UseUserOverride = true;
```

```vb
Me.calculatorControl1.UseUserOverride = True
```

## Practical Examples

### Example 1: Initialize Calculator with Starting Value

```csharp
CalculatorControl calc = new CalculatorControl();
calc.DoubleValue = 100;  // Start with 100
calc.Culture = new System.Globalization.CultureInfo("en-US");
this.Controls.Add(calc);
```

### Example 2: Display Calculator Value in TextBox

```csharp
TextBox resultTextBox = new TextBox();
CalculatorControl calc = new CalculatorControl();

calc.ValueCalculated += (sender, args) =>
{
    if (!args.ErrorCondition)
    {
        resultTextBox.Text = calc.Value.ToString();
    }
};

this.Controls.Add(calc);
this.Controls.Add(resultTextBox);
```

### Example 3: German Locale with Custom Display

```csharp
CalculatorControl calc = new CalculatorControl();
calc.Culture = new System.Globalization.CultureInfo("de-DE");
calc.DisplayTextAlign = System.Windows.Forms.HorizontalAlignment.Right;
calc.Font = new System.Drawing.Font("Arial", 10, System.Drawing.FontStyle.Bold);
calc.UseUserOverride = true;
this.Controls.Add(calc);
```

### Example 4: Hide Display but Handle Values Programmatically

```csharp
CalculatorControl calc = new CalculatorControl();
calc.ShowDisplayArea = false;  // Hide visual display
calc.Size = new System.Drawing.Size(200, 200);

// Store values in external display
Label valueDisplay = new Label();
valueDisplay.Location = new System.Drawing.Point(0, 0);

calc.ValueCalculated += (sender, args) =>
{
    valueDisplay.Text = calc.DoubleValue.ToString("F2");
};

this.Controls.Add(calc);
this.Controls.Add(valueDisplay);
```

## Troubleshooting

**Issue:** Display shows unexpected decimal separators
- **Solution:** Check Culture property and UseUserOverride setting for your target locale

**Issue:** DoubleValue returns 0 or unexpected value
- **Solution:** Verify the user has completed an operation (pressed "=") or set DoubleValue explicitly before reading

**Issue:** Repeated "=" operations not working
- **Solution:** Enable RepeatAssignAction property to support repeat operations
