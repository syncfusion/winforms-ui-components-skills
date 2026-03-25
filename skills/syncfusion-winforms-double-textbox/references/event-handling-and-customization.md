# Event Handling and Customization

## Table of Contents
- [DoubleValueChanged Event](#doublevaluechanged-event)
- [Keyboard Event Handling](#keyboard-event-handling)
- [Overriding Control Behavior](#overriding-control-behavior)
- [Custom HandleSubtractKey Implementation](#custom-handlesubtractkey-implementation)
- [Advanced Customization Patterns](#advanced-customization-patterns)

## DoubleValueChanged Event

The `DoubleValueChanged` event fires when the numeric value in the DoubleTextBox changes, either programmatically or through user input.

**C# Example - Basic Event Handler:**

```csharp
private void doubleTextBox1_DoubleValueChanged(object sender, EventArgs e)
{
    double newValue = doubleTextBox1.DoubleValue;
    MessageBox.Show($"New value: {newValue}");
}

// Attach the event handler
doubleTextBox1.DoubleValueChanged += doubleTextBox1_DoubleValueChanged;
```

**VB.NET Example:**

```vbnet
Private Sub doubleTextBox1_DoubleValueChanged(sender As Object, e As EventArgs)
    Dim newValue As Double = doubleTextBox1.DoubleValue
    MessageBox.Show($"New value: {newValue}")
End Sub

' Attach the event handler
doubleTextBox1.DoubleValueChanged += AddressOf doubleTextBox1_DoubleValueChanged
```

**C# Example - Using Lambda Expression:**

```csharp
doubleTextBox1.DoubleValueChanged += (s, e) =>
{
    // Update a label with the current value
    label1.Text = $"Current value: {doubleTextBox1.DoubleValue}";
    
    // Perform validation
    if (doubleTextBox1.DoubleValue > 100)
    {
        label1.ForeColor = Color.Red;
    }
    else
    {
        label1.ForeColor = Color.Green;
    }
};
```

**When Event Fires:**
- User types a value and presses Enter
- User loses focus from the control
- Value is set programmatically via `DoubleValue` property
- Value is pasted from clipboard

## Keyboard Event Handling

Handle keyboard events to implement custom shortcuts and input behavior.

**C# Example - Increment/Decrement with Up/Down Keys:**

```csharp
private void doubleTextBox1_KeyDown(object sender, KeyEventArgs e)
{
    decimal currentValue = (decimal)doubleTextBox1.DoubleValue;
    decimal increment = 1;
    
    switch (e.KeyCode)
    {
        // Up arrow: increment by 1
        case Keys.Up:
            doubleTextBox1.DoubleValue = (double)(currentValue + increment);
            e.Handled = true; // Suppress default behavior
            break;
            
        // Down arrow: decrement by 1
        case Keys.Down:
            doubleTextBox1.DoubleValue = (double)(currentValue - increment);
            e.Handled = true;
            break;
            
        // Shift+Up: increment by 10
        case Keys.Up when e.Shift:
            doubleTextBox1.DoubleValue = (double)(currentValue + (increment * 10));
            e.Handled = true;
            break;
    }
}

// Attach the event handler
doubleTextBox1.KeyDown += doubleTextBox1_KeyDown;
```

**VB.NET Example:**

```vbnet
Private Sub doubleTextBox1_KeyDown(sender As Object, e As KeyEventArgs)
    Dim currentValue As Decimal = CDec(doubleTextBox1.DoubleValue)
    Dim increment As Decimal = 1
    
    Select Case e.KeyCode
        Case Keys.Up
            ' Increment on Up arrow
            doubleTextBox1.DoubleValue = CDbl(currentValue + increment)
            e.Handled = True
            
        Case Keys.Down
            ' Decrement on Down arrow
            doubleTextBox1.DoubleValue = CDbl(currentValue - increment)
            e.Handled = True
    End Select
End Sub

' Attach event handler
doubleTextBox1.KeyDown += AddressOf doubleTextBox1_KeyDown
```

**Key Combinations:**
- Use `e.Shift`, `e.Control`, and `e.Alt` to detect modifier keys
- Set `e.Handled = true` to prevent the default keystroke behavior
- Combine with `case` or `Select Case` for custom shortcut logic

## Overriding Control Behavior

Create a derived class to override control methods and customize behavior at the source level.

**C# Example - Custom DoubleTextBox Class:**

```csharp
public class CustomDoubleTextBox : Syncfusion.Windows.Forms.Tools.DoubleTextBox
{
    public CustomDoubleTextBox() : base()
    {
    }
    
    // Override to customize value changed behavior
    protected override void OnDoubleValueChanged(EventArgs e)
    {
        // Add custom logic before firing the event
        if (this.DoubleValue < 0)
        {
            this.ForeColor = Color.Red;
        }
        else
        {
            this.ForeColor = Color.Black;
        }
        
        // Call base implementation to fire the event
        base.OnDoubleValueChanged(e);
    }
    
    // Override to validate input
    protected override void OnValidating(CancelEventArgs e)
    {
        // Custom validation logic
        if (this.DoubleValue > 1000)
        {
            e.Cancel = true;
            this.BackColor = Color.Yellow;
        }
        else
        {
            e.Cancel = false;
            this.BackColor = Color.White;
        }
        
        base.OnValidating(e);
    }
}

// Usage
var customBox = new CustomDoubleTextBox();
this.Controls.Add(customBox);
```

## Custom HandleSubtractKey Implementation

Override the `HandleSubtractKey()` method to customize how the control handles negative sign input.

**C# Example - Clear Text on Negative Sign:**

```csharp
public class DoubleTextBoxAdv : Syncfusion.Windows.Forms.Tools.DoubleTextBox
{
    public DoubleTextBoxAdv() : base() { }
    
    private bool deleteOnNegative = false;
    
    public bool DeleteOnNegative
    {
        get { return deleteOnNegative; }
        set { deleteOnNegative = value; }
    }
    
    // Overrides the behavior of SubtractKey so that the text is 
    // cleared when the NegativeSign is pressed
    protected override Syncfusion.Windows.Forms.Tools.NumberModifyState HandleSubtractKey()
    {
        if (deleteOnNegative == true)
        {
            if (this.NegativeSign == "-" && this.Text.StartsWith("-"))
            {
                this.Clear();
                return Syncfusion.Windows.Forms.Tools.NumberModifyState.Processed;
            }
        }
        return base.HandleSubtractKey();
    }
}

// Usage
var advancedBox = new DoubleTextBoxAdv();
advancedBox.DeleteOnNegative = true;
this.Controls.Add(advancedBox);
```

**VB.NET Example:**

```vbnet
Public Class DoubleTextBoxAdv
    Inherits Syncfusion.Windows.Forms.Tools.DoubleTextBox
    
    Public Sub New()
        MyBase.New()
    End Sub
    
    Private m_deleteOnNegative As Boolean = False
    
    Public Property DeleteOnNegative() As Boolean
        Get
            Return m_deleteOnNegative
        End Get
        Set(value As Boolean)
            m_deleteOnNegative = value
        End Set
    End Property
    
    ' Overrides the behavior of SubtractKey
    Protected Overrides Function HandleSubtractKey() As Syncfusion.Windows.Forms.Tools.NumberModifyState
        If m_deleteOnNegative = True Then
            If Me.NegativeSign = "-" AndAlso Me.Text.StartsWith("-") Then
                Me.Clear()
                Return Syncfusion.Windows.Forms.Tools.NumberModifyState.Processed
            End If
        End If
        Return MyBase.HandleSubtractKey()
    End Function
End Class

' Usage
Dim advancedBox As New DoubleTextBoxAdv()
advancedBox.DeleteOnNegative = True
Me.Controls.Add(advancedBox)
```

## Advanced Customization Patterns

### Pattern 1: Linked Textboxes with Auto-Calculate

```csharp
// Create a custom control that automatically calculates total
public class CalculatedDoubleTextBox : Syncfusion.Windows.Forms.Tools.DoubleTextBox
{
    private DoubleTextBox linkedBox;
    
    public void LinkTo(DoubleTextBox other)
    {
        linkedBox = other;
        this.DoubleValueChanged += (s, e) => RecalculateTotal();
        linkedBox.DoubleValueChanged += (s, e) => RecalculateTotal();
    }
    
    private void RecalculateTotal()
    {
        if (linkedBox != null)
        {
            // Example: calculate sum
            double total = this.DoubleValue + linkedBox.DoubleValue;
            // Update UI or perform action with total
        }
    }
}
```

### Pattern 2: Range Validation with Visual Feedback

```csharp
public class ValidatedDoubleTextBox : Syncfusion.Windows.Forms.Tools.DoubleTextBox
{
    public double OptimalMin { get; set; } = 0;
    public double OptimalMax { get; set; } = 100;
    
    public ValidatedDoubleTextBox() : base()
    {
        this.DoubleValueChanged += (s, e) => ValidateAndHighlight();
    }
    
    private void ValidateAndHighlight()
    {
        if (this.DoubleValue < OptimalMin || this.DoubleValue > OptimalMax)
        {
            this.BackColor = Color.LightCoral;
        }
        else if (this.DoubleValue > OptimalMax * 0.8)
        {
            this.BackColor = Color.LightYellow;
        }
        else
        {
            this.BackColor = Color.LightGreen;
        }
    }
}
```

### Pattern 3: Unit-Aware Input

```csharp
public class UnitAwareDoubleTextBox : Syncfusion.Windows.Forms.Tools.DoubleTextBox
{
    private string unitLabel = "";
    
    public string Unit
    {
        get { return unitLabel; }
        set 
        { 
            unitLabel = value;
            this.Text = $"{this.DoubleValue} {unitLabel}";
        }
    }
    
    protected override void OnDoubleValueChanged(EventArgs e)
    {
        base.OnDoubleValueChanged(e);
        // Auto-append unit label
        if (!string.IsNullOrEmpty(unitLabel))
        {
            // Format display with unit
        }
    }
}
```
