# Getting Started with Integer TextBox

## Installation

### Assembly References

The IntegerTextBox control requires the **Syncfusion.Shared.Base** assembly to be referenced in your project.

**NuGet Installation:**

You can install the required NuGet package via the Package Manager:

```
Install-Package Syncfusion.Shared.Base
```

For more details, see: [How to install NuGet packages](https://help.syncfusion.com/windowsforms/installation/install-nuget-packages)

**Manual Assembly Reference:**

1. Right-click on your project in Visual Studio
2. Select **Add Reference**
3. Navigate to the Syncfusion installation folder
4. Add **Syncfusion.Shared.Base.dll**

---

## Create the Control

### Method 1: Using the Designer

1. Open your Windows Forms designer
2. Locate the **IntegerTextBox** in the Toolbox
3. Drag it onto your form
4. The **Syncfusion.Shared.Base** assembly reference is added automatically

### Method 2: Add Control Manually in Code

Create the control programmatically by adding the following code:

```csharp
using Syncfusion.Windows.Forms.Tools;

// In your Form_Load or constructor:
IntegerTextBox integerTextBox1 = new IntegerTextBox();
this.Controls.Add(integerTextBox1);

// Set basic properties
integerTextBox1.Name = "integerTextBox1";
integerTextBox1.Location = new System.Drawing.Point(10, 10);
integerTextBox1.Size = new System.Drawing.Size(200, 24);
```

---

## Set Value Constraints

### Maximum and Minimum Values

Control the range of valid integers using MaxValue and MinValue:

```csharp
// Set a range from 0 to 100
this.integerTextBox1.MaxValue = 100;
this.integerTextBox1.MinValue = 0;
```

**Range Examples:**

```csharp
// Positive integers only (0 to 1 million)
integerTextBox1.MinValue = 0;
integerTextBox1.MaxValue = 1000000;

// Negative and positive integers (-1000 to 1000)
integerTextBox1.MinValue = -1000;
integerTextBox1.MaxValue = 1000;

// Large range (64-bit integers)
integerTextBox1.MinValue = -9223372036854775808;
integerTextBox1.MaxValue = 9223372036854775807;

// Single digit (0-9)
integerTextBox1.MinValue = 0;
integerTextBox1.MaxValue = 9;
```

When a user tries to enter a value outside this range, the control either rejects the input or clamps it to the nearest valid boundary.

---

## Set Initial Values

### Using IntegerValue

Set the initial value directly:

```csharp
// Set to zero
integerTextBox1.IntegerValue = 0;

// Set to 500
integerTextBox1.IntegerValue = 500;

// Set to negative
integerTextBox1.IntegerValue = -250;
```

### Using BindableValue

For nullable scenarios, use BindableValue:

```csharp
// Set to null (empty)
integerTextBox1.BindableValue = null;

// Set to a value
integerTextBox1.BindableValue = 42;

// Check if null
if (integerTextBox1.BindableValue == null)
{
    MessageBox.Show("No value entered");
}
```

---

## Basic Configuration Example

Here's a complete example setting up an integer textbox for age input:

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class AgeForm : Form
{
    private IntegerTextBox ageTextBox;

    public AgeForm()
    {
        InitializeComponent();
        SetupAgeTextBox();
    }

    private void SetupAgeTextBox()
    {
        // Create the control
        ageTextBox = new IntegerTextBox();
        ageTextBox.Location = new System.Drawing.Point(10, 10);
        ageTextBox.Size = new System.Drawing.Size(150, 24);
        
        // Set constraints (age 0-150)
        ageTextBox.MinValue = 0;
        ageTextBox.MaxValue = 150;
        ageTextBox.IntegerValue = 0;
        
        this.Controls.Add(ageTextBox);
        
        // Add event handler
        ageTextBox.IntegerValueChanged += (sender, e) =>
        {
            Console.WriteLine($"Age: {ageTextBox.IntegerValue}");
        };
    }
}
```

---

## Key Takeaways

- Reference the **Syncfusion.Shared.Base** assembly
- Include the **Syncfusion.Windows.Forms.Tools** namespace
- Use MaxValue/MinValue to define valid ranges
- Use IntegerValue for standard access or BindableValue for nullable scenarios
- Always set both min and max for predictable behavior
