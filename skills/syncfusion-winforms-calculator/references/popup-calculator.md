# Popup Calculator

## Overview

The PopupCalculator class allows you to embed a Calculator control as a popup window attached to buttons or other controls. This is useful when you want calculator functionality without dedicating permanent space on your form, such as in data entry scenarios where users occasionally need to calculate values.

## Creating a PopupCalculator Instance

PopupCalculator must be created programmatically (unlike CalculatorControl, which can be added via Designer).

### Basic Creation

```csharp
PopupCalculator popupCalculator = new Syncfusion.Windows.Forms.Tools.PopupCalculator();
```

```vb
Dim popupCalculator As New Syncfusion.Windows.Forms.Tools.PopupCalculator()
```

## ParentControl Property

The PopupCalculator must be associated with a parent control (usually a Button) that acts as the anchor point for the popup.

```csharp
popupCalculator.ParentControl = this.button1;
```

```vb
popupCalculator.ParentControl = Me.button1
```

**Why ParentControl matters:**
- Positions the popup relative to the button
- Provides visual relationship between button and calculator
- Popup may close when focus leaves the parent control

## PopupCalculatorAlignment

Control where the popup appears relative to the parent control using PopupCalculatorAlignment enum:

```csharp
popupCalculator.PopupCalculatorAlignment = Syncfusion.Windows.Forms.Tools.CalculatorPopupAlignment.Right;
```

```vb
popupCalculator.PopupCalculatorAlignment = Syncfusion.Windows.Forms.Tools.CalculatorPopupAlignment.Right
```

**Available Alignments:**
- **Right** — Pop up to the right of the parent control
- **Left** — Pop up to the left of the parent control
- **Top** — Pop up above the parent control
- **Bottom** — Pop up below the parent control

### Choosing Alignment

```
If parent button is on left side of form → Use Right alignment
If parent button is on right side of form → Use Left alignment
If parent button is near top of form → Use Bottom alignment
If parent button is near bottom of form → Use Top alignment
```

## DisplayCalculator Method

Display the popup at a specified location (or empty Point for automatic positioning):

```csharp
popupCalculator.DisplayCalculator(System.Drawing.Point.Empty);
```

```vb
popupCalculator.DisplayCalculator(System.Drawing.Point.Empty)
```

**Parameters:**
- `Point.Empty` — Automatic positioning based on alignment
- `new Point(x, y)` — Manual positioning (not typically needed)

## Popup Size

Set the size of the popup calculator:

```csharp
popupCalculator.Size = new System.Drawing.Size(320, 280);
```

```vb
popupCalculator.Size = New System.Drawing.Size(320, 280)
```

Or match the size of a regular CalculatorControl:

```csharp
popupCalculator.Size = this.calculatorControl1.Size;
```

```vb
popupCalculator.Size = Me.calculatorControl1.Size
```

## Closing Event

Handle the Closing event to process the final calculated value when the popup closes:

```csharp
popupCalculator.Closing += PopupCalculator_Closing;

private void PopupCalculator_Closing(object sender, CalculatorClosingEventArgs args)
{
    decimal finalValue = args.FinalValue;
    // Process the result
}
```

```vb
AddHandler popupCalculator.Closing, AddressOf PopupCalculator_Closing

Private Sub PopupCalculator_Closing(sender As Object, args As CalculatorClosingEventArgs)
    Dim finalValue As Decimal = args.FinalValue
    ' Process the result
End Sub
```

### CalculatorClosingEventArgs

The event arguments include:
- **FinalValue** — The calculated result (as Decimal)

The Closing event fires when the calculator closes after "=" is pressed.

## Complete Popup Calculator Example

### Basic Implementation

```csharp
public partial class DataEntryForm : Form
{
    private TextBox amountTextBox;
    private Button calculateButton;

    public DataEntryForm()
    {
        InitializeComponent();
        
        // Create text box for result
        amountTextBox = new TextBox();
        amountTextBox.Location = new System.Drawing.Point(10, 10);
        amountTextBox.Width = 150;
        this.Controls.Add(amountTextBox);
        
        // Create button to open calculator
        calculateButton = new Button();
        calculateButton.Text = "Calculate";
        calculateButton.Location = new System.Drawing.Point(170, 10);
        calculateButton.Click += CalculateButton_Click;
        this.Controls.Add(calculateButton);
    }

    private void CalculateButton_Click(object sender, EventArgs e)
    {
        // Create popup calculator
        PopupCalculator popupCalculator = new PopupCalculator();
        popupCalculator.ParentControl = calculateButton;
        popupCalculator.PopupCalculatorAlignment = CalculatorPopupAlignment.Bottom;
        popupCalculator.Size = new System.Drawing.Size(300, 250);
        
        // Handle when calculation is complete
        popupCalculator.Closing += (s, args) =>
        {
            amountTextBox.Text = args.FinalValue.ToString();
        };
        
        // Display the popup
        popupCalculator.DisplayCalculator(System.Drawing.Point.Empty);
    }
}
```

```vb
Public Class DataEntryForm
    Inherits Form

    Private amountTextBox As TextBox
    Private calculateButton As Button

    Public Sub New()
        InitializeComponent()

        ' Create text box for result
        amountTextBox = New TextBox()
        amountTextBox.Location = New System.Drawing.Point(10, 10)
        amountTextBox.Width = 150
        Me.Controls.Add(amountTextBox)

        ' Create button to open calculator
        calculateButton = New Button()
        calculateButton.Text = "Calculate"
        calculateButton.Location = New System.Drawing.Point(170, 10)
        AddHandler calculateButton.Click, AddressOf CalculateButton_Click
        Me.Controls.Add(calculateButton)
    End Sub

    Private Sub CalculateButton_Click(sender As Object, e As EventArgs)
        ' Create popup calculator
        Dim popupCalculator As New PopupCalculator()
        popupCalculator.ParentControl = calculateButton
        popupCalculator.PopupCalculatorAlignment = CalculatorPopupAlignment.Bottom
        popupCalculator.Size = New System.Drawing.Size(300, 250)

        ' Handle when calculation is complete
        AddHandler popupCalculator.Closing, Sub(s, args)
            amountTextBox.Text = args.FinalValue.ToString()
        End Sub

        ' Display the popup
        popupCalculator.DisplayCalculator(System.Drawing.Point.Empty)
    End Sub
End Class
```

### Advanced: Multiple Input Fields with Calculators

```csharp
public partial class SalesForm : Form
{
    private TextBox quantityBox;
    private TextBox unitPriceBox;
    private TextBox totalBox;

    public SalesForm()
    {
        InitializeComponent();
        
        // Create input fields
        quantityBox = CreateInputField(10, "Quantity:", out Button quantityCalcBtn);
        unitPriceBox = CreateInputField(50, "Unit Price:", out Button priceCalcBtn);
        totalBox = CreateInputField(90, "Total:", out Button totalCalcBtn);
        
        // Attach calculator buttons
        AttachCalculator(quantityCalcBtn, quantityBox);
        AttachCalculator(priceCalcBtn, unitPriceBox);
        AttachCalculator(totalCalcBtn, totalBox);
    }

    private TextBox CreateInputField(int top, string label, out Button calcBtn)
    {
        Label lbl = new Label();
        lbl.Text = label;
        lbl.Location = new System.Drawing.Point(10, top);
        lbl.Width = 100;
        this.Controls.Add(lbl);

        TextBox txt = new TextBox();
        txt.Location = new System.Drawing.Point(120, top);
        txt.Width = 100;
        this.Controls.Add(txt);

        calcBtn = new Button();
        calcBtn.Text = "Calc";
        calcBtn.Location = new System.Drawing.Point(230, top);
        calcBtn.Width = 50;
        this.Controls.Add(calcBtn);

        return txt;
    }

    private void AttachCalculator(Button button, TextBox targetBox)
    {
        button.Click += (sender, e) =>
        {
            PopupCalculator popup = new PopupCalculator();
            popup.ParentControl = button;
            popup.PopupCalculatorAlignment = CalculatorPopupAlignment.Right;
            popup.Size = new System.Drawing.Size(300, 250);
            popup.Closing += (s, args) =>
            {
                targetBox.Text = args.FinalValue.ToString("F2");
            };
            popup.DisplayCalculator(System.Drawing.Point.Empty);
        };
    }
}
```

## Practical Use Cases

### Use Case 1: Data Entry Form
Place calculator popups next to numeric input fields so users can calculate values before entry.

### Use Case 2: Quick Calculations
Attach a calculator to a toolbar button for quick ad-hoc calculations without cluttering the main form.

### Use Case 3: Multi-Step Calculations
Use multiple popup calculators in sequence, passing results from one to the next.

### Use Case 4: Financial Application
Provide popup calculators on invoice/quote forms for line-item calculations.

## Troubleshooting

**Issue:** Popup doesn't display
- **Solution:** Ensure ParentControl is set before calling DisplayCalculator

**Issue:** Popup closes immediately after appearing
- **Solution:** Verify user can interact; focus may be returning to parent

**Issue:** Closing event not firing
- **Solution:** Ensure user presses "=" to complete the calculation; event only fires on completion

**Issue:** FinalValue is incorrect or 0
- **Solution:** Check that a valid calculation was completed before closing; incomplete calculations return 0

**Issue:** PopupCalculatorAlignment not working
- **Solution:** Set alignment before calling DisplayCalculator
