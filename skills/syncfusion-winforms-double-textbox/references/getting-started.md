# Getting Started with DoubleTextBox

## Table of Contents
- [Assembly Deployment](#assembly-deployment)
- [Add Control Through Designer](#add-control-through-designer)
- [Add Control Manually in Code](#add-control-manually-in-code)
- [Setting Value Constraints](#setting-value-constraints)
- [Customizing Number Format](#customizing-number-format)

## Assembly Deployment

To use the DoubleTextBox control, add the **Syncfusion.Shared.Base** assembly reference to your project. This is the core assembly containing the DoubleTextBox control and its dependencies for Windows Forms applications.

## Add Control Through Designer

The easiest way to add DoubleTextBox to your form:

1. Open your Windows Forms designer
2. Locate the Syncfusion toolbox items
3. Drag **DoubleTextBox** from the toolbox onto your form
4. The Syncfusion.Shared.Base assembly reference is automatically added

The control will appear on your form ready for configuration in the Properties panel.

## Add Control Manually in Code

To add the control programmatically in C#:

```csharp
using Syncfusion.Windows.Forms.Tools;

// Create a new DoubleTextBox instance
DoubleTextBox doubleTextBox1 = new DoubleTextBox();

// Add it to the form's controls
this.Controls.Add(doubleTextBox1);

// Optional: Set location and size
doubleTextBox1.Location = new Point(10, 10);
doubleTextBox1.Size = new Size(200, 20);
```

Or in VB.NET:

```vbnet
Imports Syncfusion.Windows.Forms.Tools

' Create a new DoubleTextBox instance
Dim doubleTextBox1 As DoubleTextBox = New DoubleTextBox()

' Add it to the form's controls
Me.Controls.Add(doubleTextBox1)

' Optional: Set location and size
doubleTextBox1.Location = New Point(10, 10)
doubleTextBox1.Size = New Size(200, 20)
```

## Setting Value Constraints

The DoubleTextBox supports minimum and maximum value constraints. Users cannot enter values outside this range.

**C# Example:**

```csharp
// Set value range (0 to 100)
doubleTextBox1.MinValue = 0;
doubleTextBox1.MaxValue = 100;

// Set initial value
doubleTextBox1.DoubleValue = 50;
```

**VB.NET Example:**

```vbnet
' Set value range (0 to 100)
doubleTextBox1.MinValue = 0
doubleTextBox1.MaxValue = 100

' Set initial value
doubleTextBox1.DoubleValue = 50
```

**When User Exceeds Bounds:**
- If the user types a value greater than MaxValue, the control reverts to MaxValue
- If the user types a value less than MinValue, the control reverts to MinValue
- This provides automatic validation without requiring explicit error handling

## Customizing Number Format

Customize how numbers are displayed using formatting properties:

**C# Example:**

```csharp
doubleTextBox1.DoubleValue = 1234567.89;

// Display with 2 decimal places
doubleTextBox1.NumberDecimalDigits = 2;

// Use different decimal separator
doubleTextBox1.NumberDecimalSeparator = ",";

// Use thousands separator
doubleTextBox1.NumberGroupSeparator = ".";

// Group by 3 digits
doubleTextBox1.NumberGroupSizes = new int[] { 3 };

// Display pattern for negative numbers (0 = standard)
doubleTextBox1.NumberNegativePattern = 0;
```

Result: `1.234.567,89`

**VB.NET Example:**

```vbnet
doubleTextBox1.DoubleValue = 1234567.89

' Display with 2 decimal places
doubleTextBox1.NumberDecimalDigits = 2

' Use different decimal separator
doubleTextBox1.NumberDecimalSeparator = ","

' Use thousands separator
doubleTextBox1.NumberGroupSeparator = "."

' Group by 3 digits
doubleTextBox1.NumberGroupSizes = New Integer() {3}

' Display pattern for negative numbers
doubleTextBox1.NumberNegativePattern = 0
```

**Common Formatting Scenarios:**

| Scenario | Configuration |
|----------|---------------|
| US Format (1,234.56) | DecimalSeparator: ".", GroupSeparator: ",", Digits: 2 |
| European Format (1.234,56) | DecimalSeparator: ",", GroupSeparator: ".", Digits: 2 |
| Scientific Notation | Digits: 5+, no GroupSeparator |
| Currency | Digits: 2, match regional separators |

**Tip:** Set formatting properties when the form loads or in the designer's Properties panel rather than repeatedly changing them during runtime.
