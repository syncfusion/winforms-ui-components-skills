# Appearance and Behavior Settings

## Table of Contents
- [Border Style Settings](#border-style-settings)
- [Color Settings for Value States](#color-settings-for-value-states)
- [Keyboard Support](#keyboard-support)
- [Overflow Indicator](#overflow-indicator)
- [Active When Disabled](#active-when-disabled)

## Border Style Settings

The DoubleTextBox supports 3D border styles and customizable border colors for visual distinction.

**Available Border Styles:**

Border styles can be applied through the `BorderStyle` property:

**C# Example:**

```csharp
// Flat border
doubleTextBox1.BorderStyle = BorderStyle.Fixed3D;

// Other common border styles
doubleTextBox1.BorderStyle = BorderStyle.FixedSingle;
```

**VB.NET Example:**

```vbnet
' Set border style
doubleTextBox1.BorderStyle = BorderStyle.Fixed3D
```

**Border Color Customization:**

Use inherited TextBox properties to customize appearance:

**C# Example:**

```csharp
// Set background color
doubleTextBox1.BackColor = Color.White;

// Set text color
doubleTextBox1.ForeColor = Color.Black;

// Set border color (depends on border style)
doubleTextBox1.BorderStyle = BorderStyle.Fixed3D;
```

## Color Settings for Value States

Apply different colors based on whether the value is positive, negative, or zero. This provides visual feedback to users:

**C# Example - Color by Value State:**

```csharp
private void ApplyValueStateColors(DoubleTextBox textBox)
{
    double value = textBox.DoubleValue;
    
    if (value > 0)
    {
        // Positive values: green text
        textBox.ForeColor = Color.Green;
    }
    else if (value < 0)
    {
        // Negative values: red text
        textBox.ForeColor = Color.Red;
    }
    else
    {
        // Zero: black text
        textBox.ForeColor = Color.Black;
    }
}

// Apply on value change
doubleTextBox1.DoubleValueChanged += (s, e) =>
{
    ApplyValueStateColors(doubleTextBox1);
};
```

**VB.NET Example:**

```vbnet
Private Sub ApplyValueStateColors(textBox As DoubleTextBox)
    Dim value As Double = textBox.DoubleValue
    
    If value > 0 Then
        ' Positive values: green text
        textBox.ForeColor = Color.Green
    ElseIf value < 0 Then
        ' Negative values: red text
        textBox.ForeColor = Color.Red
    Else
        ' Zero: black text
        textBox.ForeColor = Color.Black
    End If
End Sub

' Apply on value change
doubleTextBox1.DoubleValueChanged += Sub(s, e)
    ApplyValueStateColors(doubleTextBox1)
End Sub
```

**Background Colors:**

```csharp
// Highlight when value is out of normal range
if (doubleTextBox1.DoubleValue > 1000)
{
    doubleTextBox1.BackColor = Color.Yellow;
}
else
{
    doubleTextBox1.BackColor = Color.White;
}
```

## Keyboard Support

The DoubleTextBox inherits keyboard support from the TextBox control and supports standard keyboard operations:

**Standard Keyboard Operations:**
- Ctrl+A: Select all text
- Ctrl+C: Copy selected text
- Ctrl+X: Cut selected text
- Ctrl+V: Paste from clipboard
- Delete: Delete selected text
- Backspace: Delete character before cursor

**Custom Keyboard Handling:**

You can add custom keyboard event handlers:

**C# Example - Increment/Decrement with Arrow Keys:**

```csharp
private void doubleTextBox1_KeyDown(object sender, KeyEventArgs e)
{
    decimal currentValue = (decimal)doubleTextBox1.DoubleValue;
    decimal step = 0.1m;
    
    switch (e.KeyCode)
    {
        case Keys.Up:
            // Increment on Up arrow
            doubleTextBox1.DoubleValue = (double)(currentValue + step);
            e.Handled = true;
            break;
            
        case Keys.Down:
            // Decrement on Down arrow
            doubleTextBox1.DoubleValue = (double)(currentValue - step);
            e.Handled = true;
            break;
            
        case Keys.Multiply:
            // Double value on Multiply key
            doubleTextBox1.DoubleValue = doubleTextBox1.DoubleValue * 2;
            e.Handled = true;
            break;
            
        case Keys.Divide:
            // Halve value on Divide key
            doubleTextBox1.DoubleValue = doubleTextBox1.DoubleValue / 2;
            e.Handled = true;
            break;
    }
}

// Attach event handler
doubleTextBox1.KeyDown += doubleTextBox1_KeyDown;
```

**VB.NET Example:**

```vbnet
Private Sub doubleTextBox1_KeyDown(sender As Object, e As KeyEventArgs)
    Dim currentValue As Decimal = CDec(doubleTextBox1.DoubleValue)
    Dim [step] As Decimal = 0.1
    
    Select Case e.KeyCode
        Case Keys.Up
            ' Increment on Up arrow
            doubleTextBox1.DoubleValue = CDbl(currentValue + [step])
            e.Handled = True
            
        Case Keys.Down
            ' Decrement on Down arrow
            doubleTextBox1.DoubleValue = CDbl(currentValue - [step])
            e.Handled = True
            
        Case Keys.Multiply
            ' Double value on Multiply key
            doubleTextBox1.DoubleValue = doubleTextBox1.DoubleValue * 2
            e.Handled = True
            
        Case Keys.Divide
            ' Halve value on Divide key
            doubleTextBox1.DoubleValue = doubleTextBox1.DoubleValue / 2
            e.Handled = True
    End Select
End Sub

' Attach event handler
doubleTextBox1.KeyDown += AddressOf doubleTextBox1_KeyDown
```

## Overflow Indicator

The DoubleTextBox displays an overflow indicator when the numeric value exceeds the control's display width.

**How It Works:**
- When a value is too large to display completely, the control shows an overflow indicator
- The indicator visually alerts the user that the full value is not visible
- Hovering or selecting text reveals the complete value

**C# Example - Detecting Overflow:**

```csharp
// Set a large value in a small control
doubleTextBox1.Size = new Size(100, 20);
doubleTextBox1.DoubleValue = 123456789.123456;
// The overflow indicator will appear

// Monitor text changes to detect when overflow occurs
doubleTextBox1.TextChanged += (s, e) =>
{
    Graphics g = doubleTextBox1.CreateGraphics();
    SizeF textSize = g.MeasureString(doubleTextBox1.Text, doubleTextBox1.Font);
    
    if (textSize.Width > doubleTextBox1.ClientSize.Width - 4)
    {
        // Text is overflowing
        Console.WriteLine("Overflow detected");
    }
};
```

**Tip:** Make the DoubleTextBox wide enough to display expected values, or use the overflow indicator as a signal to increase control width.

## Active When Disabled

The DoubleTextBox can be set to remain interactive even when the control's `Enabled` property is set to `false`.

**C# Example:**

```csharp
// Make control appear disabled but still allow text selection
doubleTextBox1.Enabled = false;

// User can still select and copy text even though control appears disabled
// Users cannot edit the value
```

**VB.NET Example:**

```vbnet
' Make control appear disabled but still allow text selection
doubleTextBox1.Enabled = False

' User can still select and copy text
```

**Use Cases:**
- Display read-only values in a form
- Show calculated results that users can view but not edit
- Present historical data for reference

**Important Notes:**
- When `Enabled = false`, the control typically appears grayed out
- Users can still select and copy text from the control
- The `DoubleValueChanged` event does not fire when disabled
- Users cannot modify the value when disabled
