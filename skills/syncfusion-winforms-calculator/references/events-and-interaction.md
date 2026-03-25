# Events and Interaction

## ValueCalculated Event

The ValueCalculated event fires each time the calculator's value changes, including during digit entry, operations, and calculation completion. This is the primary event for tracking user interactions.

### Event Overview

```csharp
CalculatorControl calc = new CalculatorControl();
calc.ValueCalculated += Calculator_ValueCalculated;

private void Calculator_ValueCalculated(object sender, CalculatorValueCalculatedEventArgs args)
{
    // Handle value change
}
```

```vb
Dim calc As New CalculatorControl()
AddHandler calc.ValueCalculated, AddressOf Calculator_ValueCalculated

Private Sub Calculator_ValueCalculated(sender As Object, args As CalculatorValueCalculatedEventArgs)
    ' Handle value change
End Sub
```

### Event Arguments

The CalculatorValueCalculatedEventArgs provides:
- **LastAction** — The action that triggered the event (CalcActions enum value)
- **ErrorCondition** — Boolean indicating if an error occurred (e.g., division by zero)

### LastAction Property

LastAction identifies what operation caused the event to fire. Common values:

| LastAction | Triggered By |
|------------|-------------|
| CalcOperatorEquals | Pressing "=" or Enter |
| CalcDigitZero...CalcDigitNine | Pressing number buttons |
| CalcOperatorPlus | Pressing "+" |
| CalcOperatorMinus | Pressing "-" |
| CalcOperatorMultiply | Pressing "*" |
| CalcOperatorDivide | Pressing "/" |
| CalcSpecialBackspace | Pressing Backspace |
| CalcSpecialClear | Pressing C (ESC) |
| CalcMemoryStore | Pressing MS (CTRL+M) |
| CalcOperatorPercent | Pressing "%" |

## Using ValueCalculated for Final Results

Since ValueCalculated fires during every change, use LastAction to detect when users press "=":

```csharp
private void calc_ValueCalculated(object sender, CalculatorValueCalculatedEventArgs args)
{
    // Check for final calculation completion
    if (!args.ErrorCondition && args.LastAction == CalcActions.CalcOperatorEquals)
    {
        CalculatorControl calc = sender as CalculatorControl;
        double result = calc.DoubleValue;
        
        // Process final result
        MessageBox.Show($"Result: {result}");
        LogCalculation(result);
    }
}
```

```vb
Private Sub calc_ValueCalculated(sender As Object, args As CalculatorValueCalculatedEventArgs)
    ' Check for final calculation completion
    If Not args.ErrorCondition AndAlso args.LastAction = CalcActions.CalcOperatorEquals Then
        Dim calc As CalculatorControl = DirectCast(sender, CalculatorControl)
        Dim result As Double = calc.DoubleValue

        ' Process final result
        MessageBox.Show(String.Format("Result: {0}", result))
        LogCalculation(result)
    End If
End Sub
```

## Handling Errors

The ErrorCondition property indicates calculation errors:

```csharp
private void calc_ValueCalculated(object sender, CalculatorValueCalculatedEventArgs args)
{
    if (args.ErrorCondition)
    {
        // Handle error (e.g., division by zero)
        MessageBox.Show("Calculation error occurred");
    }
    else if (args.LastAction == CalcActions.CalcOperatorEquals)
    {
        // Process valid result
        CalculatorControl calc = sender as CalculatorControl;
        ProcessResult(calc.DoubleValue);
    }
}
```

```vb
Private Sub calc_ValueCalculated(sender As Object, args As CalculatorValueCalculatedEventArgs)
    If args.ErrorCondition Then
        ' Handle error (e.g., division by zero)
        MessageBox.Show("Calculation error occurred")
    ElseIf args.LastAction = CalcActions.CalcOperatorEquals Then
        ' Process valid result
        Dim calc As CalculatorControl = DirectCast(sender, CalculatorControl)
        ProcessResult(calc.DoubleValue)
    End If
End Sub
```

## Closing Event for PopupCalculator

The PopupCalculator control fires a Closing event when the popup closes, typically after "=" is pressed.

### Event Handler

```csharp
PopupCalculator popupCalc = new PopupCalculator();
popupCalc.Closing += PopupCalculator_Closing;

private void PopupCalculator_Closing(object sender, CalculatorClosingEventArgs args)
{
    // args.FinalValue contains the calculated result
    decimal finalValue = args.FinalValue;
}
```

```vb
Dim popupCalc As New PopupCalculator()
AddHandler popupCalc.Closing, AddressOf PopupCalculator_Closing

Private Sub PopupCalculator_Closing(sender As Object, args As CalculatorClosingEventArgs)
    ' args.FinalValue contains the calculated result
    Dim finalValue As Decimal = args.FinalValue
End Sub
```

## Complete Event Examples

### Example 1: Display Final Result

```csharp
public partial class MainForm : Form
{
    private Label resultLabel;
    private CalculatorControl calculator;

    public MainForm()
    {
        InitializeComponent();
        
        // Setup UI
        resultLabel = new Label();
        resultLabel.Location = new System.Drawing.Point(10, 260);
        resultLabel.Width = 300;
        this.Controls.Add(resultLabel);
        
        // Setup calculator
        calculator = new CalculatorControl();
        calculator.Size = new System.Drawing.Size(300, 250);
        calculator.ValueCalculated += Calculator_ValueCalculated;
        this.Controls.Add(calculator);
    }

    private void Calculator_ValueCalculated(object sender, CalculatorValueCalculatedEventArgs args)
    {
        if (!args.ErrorCondition && args.LastAction == CalcActions.CalcOperatorEquals)
        {
            resultLabel.Text = $"Result: {calculator.DoubleValue}";
        }
        else if (args.ErrorCondition)
        {
            resultLabel.Text = "Error in calculation";
        }
    }
}
```

### Example 2: Log All Calculator Operations

```csharp
public class CalculatorLogger
{
    private List<string> operations = new List<string>();
    private CalculatorControl calculator;

    public CalculatorLogger(CalculatorControl calc)
    {
        calculator = calc;
        calculator.ValueCalculated += Logger_ValueCalculated;
    }

    private void Logger_ValueCalculated(object sender, CalculatorValueCalculatedEventArgs args)
    {
        string action = args.LastAction.ToString();
        string value = calculator.DoubleValue.ToString();
        string errorStatus = args.ErrorCondition ? "ERROR" : "OK";
        
        operations.Add($"[{DateTime.Now:HH:mm:ss}] Action: {action}, Value: {value}, Status: {errorStatus}");
    }

    public void DisplayLog()
    {
        foreach (string operation in operations)
        {
            Console.WriteLine(operation);
        }
    }
}
```

### Example 3: Transfer Popup Result to TextBox

```csharp
public partial class MainForm : Form
{
    private TextBox inputBox;
    private Button calcButton;

    public MainForm()
    {
        InitializeComponent();
        
        // Setup TextBox
        inputBox = new TextBox();
        inputBox.Location = new System.Drawing.Point(10, 10);
        inputBox.Width = 200;
        this.Controls.Add(inputBox);
        
        // Setup Button
        calcButton = new Button();
        calcButton.Text = "Calculator";
        calcButton.Location = new System.Drawing.Point(220, 10);
        calcButton.Click += CalcButton_Click;
        this.Controls.Add(calcButton);
    }

    private void CalcButton_Click(object sender, EventArgs e)
    {
        PopupCalculator popupCalc = new PopupCalculator();
        popupCalc.ParentControl = calcButton;
        popupCalc.PopupCalculatorAlignment = CalculatorPopupAlignment.Right;
        popupCalc.Closing += (s, args) =>
        {
            inputBox.Text = args.FinalValue.ToString();
        };
        popupCalc.DisplayCalculator(System.Drawing.Point.Empty);
    }
}
```

### Example 4: Detect Specific Operations

```csharp
private void Calculator_ValueCalculated(object sender, CalculatorValueCalculatedEventArgs args)
{
    CalculatorControl calc = sender as CalculatorControl;
    
    switch (args.LastAction)
    {
        case CalcActions.CalcOperatorEquals:
            HandleCalculationComplete(calc.DoubleValue);
            break;
            
        case CalcActions.CalcMemoryStore:
            HandleMemoryStore();
            break;
            
        case CalcActions.CalcMemoryRecall:
            HandleMemoryRecall();
            break;
            
        case CalcActions.CalcOperatorPercent:
            HandlePercentage();
            break;
            
        case CalcActions.CalcSpecialClear:
            HandleClear();
            break;
    }
}

private void HandleCalculationComplete(double result)
{
    // Only process when user presses =
}

private void HandleMemoryStore()
{
    // React to memory store operation
}

private void HandleMemoryRecall()
{
    // React to memory recall operation
}

private void HandlePercentage()
{
    // React to percentage operation
}

private void HandleClear()
{
    // Reset UI or state when calculator is cleared
}
```

## Troubleshooting Events

**Issue:** ValueCalculated fires too frequently
- **Solution:** Use LastAction to filter for specific operations only

**Issue:** PopupCalculator Closing event doesn't fire
- **Solution:** Ensure user presses "=" to complete calculation; closing event fires on completion

**Issue:** ErrorCondition not set for invalid operations
- **Solution:** Error handling depends on operation type; not all invalid inputs set error flag

**Issue:** FinalValue in Closing event is incorrect
- **Solution:** Verify the user completed the calculation with "="; incomplete calculations may not return expected value
