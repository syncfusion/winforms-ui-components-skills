# Getting Started with ButtonEdit

## Assembly Deployment

To use ButtonEdit in your Windows Forms application, you need to add the required assembly references.

### NuGet Installation

1. Open Package Manager Console in Visual Studio
2. Run the following command:
```
Install-Package Syncfusion.Tools.Windows
```

Or use the NuGet Package Manager UI:
1. Right-click on project → Manage NuGet Packages
2. Search for "Syncfusion.Tools.Windows"
3. Click Install

The dependent assemblies will be added automatically to your project.

## Creating ButtonEdit Through Designer

This is the easiest way to add ButtonEdit to your form:

**Step 1:** Open your Windows Forms project in Visual Studio and create a new form.

**Step 2:** In the toolbox, find the ButtonEdit control under Syncfusion Tools. If not visible, ensure the Syncfusion NuGet package is installed.

**Step 3:** Drag and drop ButtonEdit from the toolbox onto your form designer window.

**Step 4:** The dependent assemblies will be added automatically.

**Step 5:** Configure properties in the Properties panel or using the Smart Tag.

## Creating ButtonEdit Through Code

For programmatic creation:

```csharp
using Syncfusion.Windows.Forms.Tools;

public Form1()
{
    InitializeComponent();
    
    ButtonEdit buttonEdit = new ButtonEdit();
    buttonEdit.Location = new System.Drawing.Point(145, 135);
    buttonEdit.Name = "buttonEdit1";
    buttonEdit.Text = "buttonEdit1";
    buttonEdit.Size = new System.Drawing.Size(200, 21);
    
    this.Controls.Add(buttonEdit);
}
```

### Visual Basic Example

```vb
Imports Syncfusion.Windows.Forms.Tools

Public Sub New()
    InitializeComponent()
    
    Dim buttonEdit As ButtonEdit = New ButtonEdit()
    buttonEdit.Location = New System.Drawing.Point(145, 135)
    buttonEdit.Name = "buttonEdit1"
    buttonEdit.Text = "buttonEdit1"
    buttonEdit.Size = New System.Drawing.Size(200, 21)
    
    Me.Controls.Add(buttonEdit)
End Sub
```

## Embedding Textbox

The ButtonEdit control has a TextBox property that holds the embedded textbox instance. By default, a TextBoxExt is embedded.

### Accessing the Embedded Textbox

```csharp
ButtonEdit buttonEdit = new ButtonEdit();

// Access the textbox
TextBoxExt textBox = buttonEdit.TextBox as TextBoxExt;
textBox.Text = "Enter value";
```

### Using a Custom Textbox

You can replace the default textbox with a specialized textbox:

```csharp
ButtonEdit buttonEdit = new ButtonEdit();

// Create and embed a PercentTextBox
PercentTextBox percentBox = new PercentTextBox();
buttonEdit.TextBox = percentBox;
buttonEdit.Controls.Add(percentBox);
```

Other specialized textboxes available:
- `IntegerTextBox` - for integer values
- `PercentTextBox` - for percentage values
- `CurrencyTextBox` - for currency values
- `TextBoxExt` - default extended textbox

## Adding Child Buttons

Child buttons are added through the Buttons collection.

### Through Designer

1. Select ButtonEdit on the form
2. In the Properties panel, find "Buttons" property
3. Click the "..." button to open ButtonEditChildButton Collection Editor
4. Click "Add" to create new child buttons
5. Configure each button's properties (Text, Image, ButtonAlign, etc.)

### Through Code

```csharp
ButtonEdit buttonEdit = new ButtonEdit();

// Create child buttons
ButtonEditChildButton button1 = new ButtonEditChildButton();
button1.Text = "Left";
button1.ButtonAlign = ButtonAlignment.Left;

ButtonEditChildButton button2 = new ButtonEditChildButton();
button2.Text = "Right";
button2.ButtonAlign = ButtonAlignment.Right;

// Add to collection
buttonEdit.Buttons.Add(button1);
buttonEdit.Buttons.Add(button2);
```

## Complete Example with Multiple Buttons

```csharp
public Form1()
{
    InitializeComponent();
    
    TextBoxExt textBoxExt = new TextBoxExt();
    ButtonEdit buttonEdit = new ButtonEdit();
    buttonEdit.Location = new System.Drawing.Point(50, 50);
    buttonEdit.Size = new System.Drawing.Size(200, 21);
    buttonEdit.Name = "buttonEdit1";
    
    // Embed textbox
    buttonEdit.TextBox = textBoxExt;
    buttonEdit.Controls.Add(textBoxExt);
    
    // Create left-aligned button
    ButtonEditChildButton leftButton = new ButtonEditChildButton();
    leftButton.Text = "L";
    leftButton.ButtonAlign = ButtonAlignment.Left;
    
    // Create center button
    ButtonEditChildButton centerButton = new ButtonEditChildButton();
    centerButton.Text = "C";
    centerButton.ButtonAlign = ButtonAlignment.Right;
    
    // Create right button
    ButtonEditChildButton rightButton = new ButtonEditChildButton();
    rightButton.Text = "R";
    rightButton.ButtonAlign = ButtonAlignment.Right;
    
    // Add buttons to collection
    buttonEdit.Buttons.Add(leftButton);
    buttonEdit.Buttons.Add(centerButton);
    buttonEdit.Buttons.Add(rightButton);
    
    this.Controls.Add(buttonEdit);
}
```

## ShowTextBox Property

By default, the embedded textbox is visible. You can hide it using the ShowTextBox property:

```csharp
buttonEdit.ShowTextBox = false;  // Hide textbox, show only buttons
buttonEdit.ShowTextBox = true;   // Show textbox with buttons (default)
```

## Textbox Selection

Control text selection in the embedded textbox:

```csharp
// Select text from position 3, length 5
buttonEdit.SelectionStart = 3;
buttonEdit.SelectionLength = 5;
```

## Next Steps

- **For styling:** See Appearance & Styling reference
- **For button customization:** See Child Button Customization reference
- **For interaction:** See Events & Interaction reference
