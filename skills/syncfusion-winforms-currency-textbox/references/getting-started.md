# Getting Started with CurrencyTextBox

## Installation and Assembly Setup

### Required Assemblies

Add the following Syncfusion assembly to your project references:
- **Syncfusion.Shared.Base** - Contains core functionality

### NuGet Installation

For NuGet Package Manager Console:
```
Install-Package Syncfusion.Shared.Base
```

Or use the NuGet Package Manager UI to search for `Syncfusion.Shared.Base`.

### Manual Assembly Reference

1. Right-click your project in Solution Explorer
2. Select "Add Reference..."
3. Browse to Syncfusion installation folder: `Program Files\Syncfusion\Essential Studio\Windows\Assemblies`
4. Select `Syncfusion.Shared.Base.dll`
5. Click Add

## Creating a Basic Project

### Step 1: Create a New Windows Forms Project

1. Open Visual Studio
2. Create a new **Windows Forms App** project (.NET Framework or .NET Core)
3. Target Framework: .NET Framework 4.7.2 or higher (or .NET 6.0+)

### Step 2: Add Control via Designer

**Easiest method for beginners:**

1. Open your Form in Designer view
2. In the Toolbox, expand the "Syncfusion" section
3. Drag **CurrencyTextBox** onto the form
4. The Syncfusion.Shared.Base assembly is automatically added

**Result:** A control named `currencyTextBox1` is added to the form.

### Step 3: Add Control Manually in Code

**For programmatic control creation:**

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace CurrencyTextBoxDemo
{
    public partial class Form1 : Form
    {
        private CurrencyTextBox currencyTextBox1;
        
        public Form1()
        {
            InitializeComponent();
        }
        
        private void Form1_Load(object sender, EventArgs e)
        {
            // Create instance
            currencyTextBox1 = new CurrencyTextBox();
            
            // Set location and size
            currencyTextBox1.Location = new System.Drawing.Point(10, 10);
            currencyTextBox1.Size = new System.Drawing.Size(200, 25);
            
            // Add to form
            this.Controls.Add(currencyTextBox1);
        }
    }
}
```

## Basic Configuration

### Setting Maximum and Minimum Values

These properties enforce boundaries on the currency amount:

```csharp
currencyTextBox1.MaxValue = 10000;    // Cannot exceed $10,000
currencyTextBox1.MinValue = 0;        // Cannot go below $0
```

**Important:** If user enters a value outside these bounds:
- The value is rejected during validation
- ValidationError event is fired
- The input is not accepted

### Setting Currency Symbol

Customize the symbol displayed with the currency:

```csharp
// US Dollar
currencyTextBox1.CurrencySymbol = "$";

// Euro
currencyTextBox1.CurrencySymbol = "€";

// British Pound
currencyTextBox1.CurrencySymbol = "£";

// Japanese Yen
currencyTextBox1.CurrencySymbol = "¥";

// Custom symbol
currencyTextBox1.CurrencySymbol = "₹";
```

The symbol appears at the beginning of the value based on `CurrencyPositivePattern`.

### Setting Initial Value

Use `DecimalValue` to set the initial amount programmatically:

```csharp
// Set to $100.50
currencyTextBox1.DecimalValue = 100.50m;

// Set to $0.00
currencyTextBox1.DecimalValue = 0m;

// Set to $1,234.56
currencyTextBox1.DecimalValue = 1234.56m;
```

The display automatically formats based on decimal and group settings.

## Complete Basic Example

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace CurrencyTextBoxGettingStarted
{
    public partial class Form1 : Form
    {
        private CurrencyTextBox currencyTextBox1;
        private Label labelAmount;
        private Button btnSubmit;
        
        public Form1()
        {
            InitializeComponent();
            this.Text = "Currency TextBox Demo";
            this.Size = new System.Drawing.Size(400, 200);
        }
        
        private void Form1_Load(object sender, EventArgs e)
        {
            // Create label
            labelAmount = new Label();
            labelAmount.Text = "Enter Amount:";
            labelAmount.Location = new System.Drawing.Point(10, 10);
            labelAmount.Size = new System.Drawing.Size(100, 25);
            
            // Create CurrencyTextBox
            currencyTextBox1 = new CurrencyTextBox();
            currencyTextBox1.Location = new System.Drawing.Point(120, 10);
            currencyTextBox1.Size = new System.Drawing.Size(200, 25);
            
            // Configure currency
            currencyTextBox1.CurrencySymbol = "$";
            currencyTextBox1.CurrencyDecimalDigits = 2;
            currencyTextBox1.MaxValue = 999999.99m;
            currencyTextBox1.MinValue = 0m;
            currencyTextBox1.DecimalValue = 100.00m;
            
            // Create submit button
            btnSubmit = new Button();
            btnSubmit.Text = "Submit";
            btnSubmit.Location = new System.Drawing.Point(120, 50);
            btnSubmit.Size = new System.Drawing.Size(75, 25);
            btnSubmit.Click += BtnSubmit_Click;
            
            // Add controls to form
            this.Controls.Add(labelAmount);
            this.Controls.Add(currencyTextBox1);
            this.Controls.Add(btnSubmit);
        }
        
        private void BtnSubmit_Click(object sender, EventArgs e)
        {
            // Get the decimal value for processing
            decimal amount = currencyTextBox1.DecimalValue;
            MessageBox.Show($"You entered: {amount:C2}");
        }
    }
}
```

## Validation During Entry

The CurrencyTextBox automatically validates input as the user types:

**Keyboard input is rejected if:**
- Non-numeric characters are entered (except decimal separator and currency symbol)
- Input would exceed MaxValue
- Input would go below MinValue
- Format characters are invalid

**Result:** Invalid keystrokes are silently ignored, and the ValidationError event fires.

## Default Behavior

**Default values when control is created:**
- CurrencySymbol: "$"
- CurrencyDecimalDigits: 2
- CurrencyGroupSeparator: ","
- CurrencyDecimalSeparator: "."
- MaxValue: No limit (decimal.MaxValue)
- MinValue: No limit (decimal.MinValue)
- Initial value: $0.00

You should always configure these values to match your application requirements.

## Property Access Patterns

**Use DecimalValue for:**
- Reading value for calculations: `decimal amount = currencyTextBox1.DecimalValue;`
- Setting value programmatically: `currencyTextBox1.DecimalValue = 100.50m;`
- Storing in database: `db.Amount = currencyTextBox1.DecimalValue;`

**Use Text for:**
- Display purposes only
- Serialization with formatting
- Logging the formatted display
- Never for calculations (use DecimalValue instead)

## Common Issues

**Issue:** Control shows empty or "$0.00" on load
- **Solution:** Explicitly set `DecimalValue` in Form_Load after control creation

**Issue:** Cannot edit the value in the designer
- **Solution:** Use Properties panel to set DecimalValue after adding control to designer

**Issue:** Pasted values don't format correctly
- **Solution:** See "Clipboard Support" in Advanced Features reference
