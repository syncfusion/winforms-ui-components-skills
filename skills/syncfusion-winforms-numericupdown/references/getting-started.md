# Getting Started with NumericUpDownExt

Complete guide to installing, configuring, and using the Syncfusion NumericUpDownExt control in WinForms applications.

## Overview

The **NumericUpDownExt** is an enhanced version of the standard WinForms NumericUpDown control that provides XP Themes support and additional features. It allows users to increment or decrement numeric values using up/down buttons or keyboard arrow keys.

## Assembly Dependencies

Before using the NumericUpDownExt control, you must add references to the following assemblies in your project:

- **Syncfusion.Grid.Base** - Base grid functionality
- **Syncfusion.Grid.Windows** - Windows-specific grid components
- **Syncfusion.Shared.Base** - Shared base utilities
- **Syncfusion.Shared.Windows** - Shared Windows Forms utilities
- **Syncfusion.Tools.Base** - Tools base library
- **Syncfusion.Tools.Windows** - Windows Forms tools (contains NumericUpDownExt)

These assemblies are automatically added when you add the control via the designer.

## NuGet Package Installation

You can install the NumericUpDownExt control via NuGet Package Manager:

1. Open the NuGet Package Manager in Visual Studio
2. Search for **Syncfusion.Tools.Windows**
3. Install the package

Alternatively, use the Package Manager Console:

```powershell
Install-Package Syncfusion.Tools.Windows
```

This package includes all the required dependencies.

## Creating NumericUpDownExt via Designer

The easiest way to add a NumericUpDownExt control is through the Visual Studio designer:

### Step 1: Create a New Project
Create a new Windows Forms application in Visual Studio.

### Step 2: Add Control from Toolbox
1. Open the form in the designer view
2. Locate **NumericUpDownExt** in the Syncfusion Controls toolbox section
3. Drag and drop the control onto your form

![NumericUpDownExt in Designer](../images/numericupdownext-designer.png)

The required assemblies will be automatically referenced in your project.

### Step 3: Configure Properties
Use the Properties window to configure the control:

- **Value**: Set the initial numeric value
- **Minimum**: Set the minimum allowed value
- **Maximum**: Set the maximum allowed value
- **Increment**: Set the step size for up/down buttons
- **DecimalPlaces**: Set number of decimal places to display

```csharp
// Designer-generated code (in Form1.Designer.cs)
this.numericUpDownExt1.Location = new System.Drawing.Point(50, 30);
this.numericUpDownExt1.Name = "numericUpDownExt1";
this.numericUpDownExt1.Size = new System.Drawing.Size(120, 20);
this.numericUpDownExt1.Value = new decimal(new int[] {0, 0, 0, 0});
this.numericUpDownExt1.Minimum = new decimal(new int[] {0, 0, 0, 0});
this.numericUpDownExt1.Maximum = new decimal(new int[] {100, 0, 0, 0});
```

## Creating NumericUpDownExt via Code

For programmatic control creation, follow these steps:

### Step 1: Add Assembly References
Manually add references to the six required assemblies listed above.

### Step 2: Include Namespace
Add the necessary using directive to your code file:

```csharp
using Syncfusion.Windows.Forms.Tools;
using System.Drawing;
using System.Windows.Forms;
```

### Step 3: Create and Configure Control
Create an instance of NumericUpDownExt and add it to your form:

```csharp
public partial class Form1 : Form
{
    private NumericUpDownExt numericUpDownExt1;

    public Form1()
    {
        InitializeComponent();
        InitializeNumericUpDown();
    }

    private void InitializeNumericUpDown()
    {
        // Create the control
        this.numericUpDownExt1 = new NumericUpDownExt();

        // Set location and size
        this.numericUpDownExt1.Location = new Point(70, 29);
        this.numericUpDownExt1.Size = new Size(120, 20);
        this.numericUpDownExt1.Name = "numericUpDownExt1";

        // Add to form's controls collection
        this.Controls.Add(this.numericUpDownExt1);
    }
}
```

**Result:** A basic NumericUpDownExt control is added to the form with default settings.

## Basic Configuration Example

Here's a complete example showing common initial setup:

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace NumericUpDownDemo
{
    public partial class Form1 : Form
    {
        private NumericUpDownExt numericUpDownExt1;

        public Form1()
        {
            InitializeComponent();
            SetupNumericControl();
        }

        private void SetupNumericControl()
        {
            // Create control
            this.numericUpDownExt1 = new NumericUpDownExt();
            
            // Position and size
            this.numericUpDownExt1.Location = new Point(50, 50);
            this.numericUpDownExt1.Size = new Size(150, 24);
            this.numericUpDownExt1.Name = "numericUpDownExt1";
            
            // Value constraints
            this.numericUpDownExt1.Minimum = new decimal(0);
            this.numericUpDownExt1.Maximum = new decimal(1000);
            this.numericUpDownExt1.Value = new decimal(50);
            this.numericUpDownExt1.Increment = new decimal(5);
            
            // Display settings
            this.numericUpDownExt1.DecimalPlaces = 2;
            this.numericUpDownExt1.ThousandsSeparator = true;
            
            // Behavior
            this.numericUpDownExt1.InterceptArrowKeys = true;
            
            // Add to form
            this.Controls.Add(this.numericUpDownExt1);
        }
    }
}
```

**Result:** A NumericUpDownExt control displaying values from 0 to 1000 with 2 decimal places, incrementing by 5, with thousands separator enabled.

## Namespace Reference

The NumericUpDownExt control is located in the following namespace:

```csharp
using Syncfusion.Windows.Forms.Tools;
```

**Fully Qualified Type Name:**
```
Syncfusion.Windows.Forms.Tools.NumericUpDownExt
```

## Setting Up Value Constraints

Configure the range and increment behavior:

```csharp
// Set minimum value (prevents users from going below this)
numericUpDownExt1.Minimum = new decimal(10);

// Set maximum value (prevents users from exceeding this)
numericUpDownExt1.Maximum = new decimal(500);

// Set current value
numericUpDownExt1.Value = new decimal(100);

// Set increment step (amount changed per button click)
numericUpDownExt1.Increment = new decimal(10);
```

**Result:** Control accepts values between 10 and 500, starts at 100, and changes by 10 per click.

## Adding to Windows Forms

### Adding to Form Constructor
```csharp
public Form1()
{
    InitializeComponent();
    
    NumericUpDownExt priceControl = new NumericUpDownExt();
    priceControl.Location = new Point(100, 100);
    priceControl.Size = new Size(120, 24);
    priceControl.Minimum = new decimal(0);
    priceControl.Maximum = new decimal(10000);
    priceControl.DecimalPlaces = 2;
    
    this.Controls.Add(priceControl);
}
```

### Adding to Panel or Container
```csharp
// Add to a panel instead of directly to form
Panel panel1 = new Panel();
panel1.Location = new Point(20, 20);
panel1.Size = new Size(300, 200);

NumericUpDownExt quantityControl = new NumericUpDownExt();
quantityControl.Location = new Point(10, 10);
quantityControl.Size = new Size(100, 24);
quantityControl.Minimum = new decimal(1);
quantityControl.Maximum = new decimal(999);

panel1.Controls.Add(quantityControl);
this.Controls.Add(panel1);
```

**Result:** Control is contained within a panel for organized layout.

## Complete Minimal Working Example

Here's a fully functional example you can run immediately:

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace MinimalNumericUpDownExample
{
    public class MainForm : Form
    {
        private NumericUpDownExt numericUpDownExt1;
        private Label lblValue;
        private Button btnGetValue;

        public MainForm()
        {
            InitializeControls();
        }

        private void InitializeControls()
        {
            // Form setup
            this.Text = "NumericUpDownExt Demo";
            this.Size = new Size(400, 200);

            // Create NumericUpDownExt
            this.numericUpDownExt1 = new NumericUpDownExt();
            this.numericUpDownExt1.Location = new Point(50, 30);
            this.numericUpDownExt1.Size = new Size(150, 24);
            this.numericUpDownExt1.Minimum = new decimal(0);
            this.numericUpDownExt1.Maximum = new decimal(100);
            this.numericUpDownExt1.Value = new decimal(50);
            this.numericUpDownExt1.Increment = new decimal(1);
            this.numericUpDownExt1.DecimalPlaces = 0;

            // Create label
            this.lblValue = new Label();
            this.lblValue.Location = new Point(50, 70);
            this.lblValue.Size = new Size(300, 20);
            this.lblValue.Text = "Current Value: 50";

            // Create button
            this.btnGetValue = new Button();
            this.btnGetValue.Location = new Point(50, 100);
            this.btnGetValue.Size = new Size(150, 30);
            this.btnGetValue.Text = "Get Current Value";
            this.btnGetValue.Click += BtnGetValue_Click;

            // Wire up ValueChanged event
            this.numericUpDownExt1.ValueChanged += NumericUpDownExt1_ValueChanged;

            // Add controls to form
            this.Controls.Add(this.numericUpDownExt1);
            this.Controls.Add(this.lblValue);
            this.Controls.Add(this.btnGetValue);
        }

        private void NumericUpDownExt1_ValueChanged(object sender, EventArgs e)
        {
            lblValue.Text = $"Current Value: {numericUpDownExt1.Value}";
        }

        private void BtnGetValue_Click(object sender, EventArgs e)
        {
            MessageBox.Show(
                $"The current value is: {numericUpDownExt1.Value}",
                "Value Display",
                MessageBoxButtons.OK,
                MessageBoxIcon.Information);
        }

        [STAThread]
        static void Main()
        {
            Application.EnableVisualStyles();
            Application.SetCompatibleTextRenderingDefault(false);
            Application.Run(new MainForm());
        }
    }
}
```

**Result:** A complete application with NumericUpDownExt that displays the current value in a label and shows it in a message box when the button is clicked.

## Common Initialization Patterns

### Currency Input Control
```csharp
NumericUpDownExt priceInput = new NumericUpDownExt();
priceInput.Minimum = new decimal(0);
priceInput.Maximum = new decimal(999999.99M);
priceInput.DecimalPlaces = 2;
priceInput.ThousandsSeparator = true;
priceInput.Increment = new decimal(0.01M);
priceInput.Value = new decimal(0);
this.Controls.Add(priceInput);
```

### Percentage Control
```csharp
NumericUpDownExt percentInput = new NumericUpDownExt();
percentInput.Minimum = new decimal(0);
percentInput.Maximum = new decimal(100);
percentInput.DecimalPlaces = 1;
percentInput.Increment = new decimal(0.5M);
percentInput.Value = new decimal(50);
this.Controls.Add(percentInput);
```

### Quantity Control
```csharp
NumericUpDownExt quantityInput = new NumericUpDownExt();
quantityInput.Minimum = new decimal(1);
quantityInput.Maximum = new decimal(9999);
quantityInput.DecimalPlaces = 0;
quantityInput.Increment = new decimal(1);
quantityInput.Value = new decimal(1);
this.Controls.Add(quantityInput);
```

## Event Handling Setup

Handle the ValueChanged event to respond to value modifications:

```csharp
// Subscribe to event during initialization
numericUpDownExt1.ValueChanged += NumericUpDownExt1_ValueChanged;

// Event handler implementation
private void NumericUpDownExt1_ValueChanged(object sender, EventArgs e)
{
    NumericUpDownExt control = sender as NumericUpDownExt;
    Console.WriteLine($"Value changed to: {control.Value}");
    
    // Perform validation or other actions
    if (control.Value > 100)
    {
        // Handle values over 100
        Console.WriteLine("High value detected!");
    }
}
```

## Troubleshooting

### Control Not Appearing
**Issue:** Control is added but not visible on form.
**Solution:** Ensure Location and Size are set, and the control is added to the Controls collection.

```csharp
// Check these properties
numericUpDownExt1.Visible = true; // Must be true
numericUpDownExt1.Location = new Point(50, 50); // Valid location
numericUpDownExt1.Size = new Size(120, 24); // Appropriate size
this.Controls.Add(numericUpDownExt1); // Must be added to form
```

### Assembly Reference Errors
**Issue:** Type 'NumericUpDownExt' not found.
**Solution:** Ensure all six required assemblies are referenced and the using directive is present.

```csharp
// Add this using directive
using Syncfusion.Windows.Forms.Tools;

// Ensure these assemblies are referenced:
// - Syncfusion.Tools.Windows
// - Syncfusion.Shared.Windows
// - (and the other four listed above)
```

### Value Not Updating
**Issue:** Setting Value property doesn't update the display.
**Solution:** Ensure value is within Minimum and Maximum bounds.

```csharp
// Set constraints first
numericUpDownExt1.Minimum = new decimal(0);
numericUpDownExt1.Maximum = new decimal(1000);

// Then set value (must be within range)
numericUpDownExt1.Value = new decimal(500); // Valid
// numericUpDownExt1.Value = new decimal(2000); // Would be clamped to Maximum
```

## Next Steps

After setting up the basic control, explore these topics:

- **Value Settings**: Configure Min/Max constraints, Increment, and Hexadecimal mode
- **Display Settings**: Format numbers with decimal places and thousands separators
- **Appearance Customization**: Apply colors, borders, and alignment
- **Behavior Settings**: Configure keyboard input, read-only mode, and interactions
- **Themes and Styles**: Apply Office 2007/2016 visual styles and XP themes

## Key Properties Reference

| Property | Type | Description |
|----------|------|-------------|
| `Value` | decimal | Current numeric value displayed |
| `Minimum` | decimal | Minimum allowed value |
| `Maximum` | decimal | Maximum allowed value |
| `Increment` | decimal | Step size for up/down buttons |
| `DecimalPlaces` | int | Number of decimal places to display |
| `ThousandsSeparator` | bool | Whether to show thousands separator |
| `InterceptArrowKeys` | bool | Allow arrow keys to change value |
| `ReadOnly` | bool | Prevent manual text editing |
| `Hexadecimal` | bool | Display value in hexadecimal format |
