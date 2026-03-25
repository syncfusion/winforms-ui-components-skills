# Getting Started with SfNumericTextBox

## Assembly Deployment

### Required Assemblies

To use SfNumericTextBox in your application, add these assembly references to your project:

- **Syncfusion.Core.WinForms**
- **Syncfusion.SfInput.WinForms**
- **Syncfusion.Shared.Base**

### NuGet Package Installation

The easiest way to install Syncfusion WinForms packages:

1. Open Package Manager Console in Visual Studio
2. Run: `Install-Package Syncfusion.SfInput.WinForms`
3. Dependencies will be automatically added
4. Alternatively, use NuGet Package Manager UI by searching "Syncfusion WinForms"

## Adding SfNumericTextBox via Designer

### Steps

1. Create a new Windows Forms application in Visual Studio
2. Open the Toolbox (View → Toolbox)
3. Search for or locate "SfNumericTextBox" in the Syncfusion controls section
4. Drag and drop the control onto your form
5. Visual Studio automatically adds required assembly references
6. The control appears on your form ready for configuration

### Designer Configuration

In the Properties panel, you can immediately set:
- **Size**: Set width and height
- **Location**: Position on the form
- **Value**: Initial numeric value
- **Name**: Control identifier for code

## Adding SfNumericTextBox via Code

### Namespace Import

```csharp
using Syncfusion.WinForms.Input;
```

### Creating the Control

```csharp
// Create instance
private Syncfusion.WinForms.Input.SfNumericTextBox numericTextBox = 
    new Syncfusion.WinForms.Input.SfNumericTextBox();

// In your form's constructor or Load event:
this.numericTextBox.Size = new System.Drawing.Size(150, 20);
this.numericTextBox.Location = new System.Drawing.Point(10, 10);
this.Controls.Add(this.numericTextBox);
```

### VB.NET Example

```vb
Imports Syncfusion.WinForms.Input

' Create instance
Private numericTextBox As Syncfusion.WinForms.Input.SfNumericTextBox = 
    New Syncfusion.WinForms.Input.SfNumericTextBox()

' In your form's Load event:
Me.numericTextBox.Size = New System.Drawing.Size(150, 20)
Me.numericTextBox.Location = New System.Drawing.Point(10, 10)
Me.Controls.Add(Me.numericTextBox)
```

## Setting the Value

### Basic Value Assignment

```csharp
// Set numeric value
this.numericTextBox.Value = 123.45;
```

### About the Value Property

- **Type**: `double?` (nullable double)
- **Default**: `null`
- The Value property holds the actual numeric value
- The Text property displays the formatted value based on FormatMode and culture
- Full precision is always maintained in the Value property regardless of display format

### Example: Precision Preservation

```csharp
// Even with limited decimal display, full value is preserved
this.numericTextBox.Value = 123.456789;  // Full precision stored

// Display might show: 123.46 (based on format)
// But internally: 123.456789 (full precision)

// When calculating with the value, you get the exact number
double exactValue = this.numericTextBox.Value.Value;  // 123.456789
```

## Basic Format Modes

### Format Mode Property

The `FormatMode` property determines how the value is displayed and accepted:

```csharp
// Numeric format (default)
this.numericTextBox.FormatMode = Syncfusion.WinForms.Input.Enums.FormatMode.Numeric;

// Currency format
this.numericTextBox.FormatMode = Syncfusion.WinForms.Input.Enums.FormatMode.Currency;

// Percent format
this.numericTextBox.FormatMode = Syncfusion.WinForms.Input.Enums.FormatMode.Percent;
```

### Quick Reference

| Mode | Example Display | Use Case |
|------|-----------------|----------|
| **Numeric** | 1234.56 | General numbers, quantities |
| **Currency** | $1,234.56 | Money, prices, amounts |
| **Percent** | 45.50% | Percentages, rates, ratios |

For detailed format mode information, see [format-modes.md](format-modes.md).

## Watermark Text

### Basic Watermark

Watermark text displays as a hint when the value is null:

```csharp
// Enable null values
this.numericTextBox.AllowNull = true;

// Set watermark text
this.numericTextBox.WatermarkText = "Enter your age";
```

### When It Shows

- Watermark appears when `Value == null`
- Watermark disappears when user enters a value
- Watermark reappears if value is cleared back to null
- Watermark is not shown if control has focus

## Minimum and Maximum Values

### Setting Value Range

```csharp
// Define valid range
this.numericTextBox.MinValue = 10;
this.numericTextBox.MaxValue = 150;
```

### Behavior

- **During Input**: If ValidationMode is set to KeyPress, invalid values are rejected
- **On LostFocus**: If entered value exceeds range, it's reset based on LostFocusValidation setting
- **Programmatic**: You can set Value outside range, but it violates constraints

### Practical Example: Age Input

```csharp
// Restrict age to realistic range
this.numericTextBox.MinValue = 0;
this.numericTextBox.MaxValue = 120;
this.numericTextBox.WatermarkText = "Age (0-120)";
this.numericTextBox.AllowNull = true;
```

## Complete Getting Started Example

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.WinForms.Input;
using Syncfusion.WinForms.Input.Enums;

public class Form1 : Form
{
    private SfNumericTextBox numericTextBox;
    private Label label;

    public Form1()
    {
        // Create label
        label = new Label();
        label.Text = "Enter a number:";
        label.Location = new System.Drawing.Point(10, 10);
        label.AutoSize = true;
        this.Controls.Add(label);

        // Create numeric textbox
        numericTextBox = new SfNumericTextBox();
        numericTextBox.Location = new System.Drawing.Point(10, 30);
        numericTextBox.Size = new System.Drawing.Size(150, 25);
        numericTextBox.Value = 100;
        numericTextBox.MinValue = 0;
        numericTextBox.MaxValue = 1000;
        numericTextBox.FormatMode = FormatMode.Numeric;
        numericTextBox.WatermarkText = "Enter value (0-1000)";
        numericTextBox.AllowNull = true;
        
        this.Controls.Add(numericTextBox);

        // Form settings
        this.Text = "SfNumericTextBox Demo";
        this.Size = new System.Drawing.Size(300, 150);
        this.StartPosition = FormStartPosition.CenterScreen;
    }

    [STAThread]
    static void Main()
    {
        Application.EnableVisualStyles();
        Application.Run(new Form1());
    }
}
```

## Next Steps

- For detailed formatting options, see [format-modes.md](format-modes.md)
- For prefix, suffix, and hiding zeros, see [formatting-options.md](formatting-options.md)
- For validation configuration, see [validation-and-ranges.md](validation-and-ranges.md)
- For color and border customization, see [appearance-and-styling.md](appearance-and-styling.md)
