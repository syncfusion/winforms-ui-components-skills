# Getting Started with MaskedEditBox

This guide covers installation and basic setup of the MaskedEditBox control in a WinForms application.

## Prerequisites

- Visual Studio 2019 or later
- .NET Framework 4.6.2 or later
- Syncfusion WinForms NuGet packages

## Assembly Requirements

To use MaskedEditBox, add the following assembly reference to your project:

- **Syncfusion.Shared.Base** (core library for MaskedEditBox)

### Installing via NuGet

1. Open **Package Manager Console** in Visual Studio
2. Run the following command:

```powershell
Install-Package Syncfusion.Shared.Base
```

This automatically includes all required dependencies including `Syncfusion.Shared.Base`.

### Manual Assembly Reference

1. In Visual Studio, right-click on **References** in Solution Explorer
2. Select **Add Reference**
3. Browse to the Syncfusion installation folder (typically `C:\Program Files (x86)\Syncfusion\Essential Studio\Windows\Assemblies`)
4. Select `Syncfusion.Shared.Base.dll`
5. Click **Add**

## Adding MaskedEditBox via Designer

**Step 1: Open Toolbox**
- In Visual Studio, open the **Toolbox** panel (View → Toolbox)

**Step 2: Locate MaskedEditBox**
- Expand **Syncfusion Components** or search for "MaskedEditBox"
- If not visible, ensure the assembly is referenced in your project

**Step 3: Drag to Form**
- Drag **MaskedEditBox** from the Toolbox onto your Form in Design View
- The dependent assembly will be automatically added

**Step 4: Configure Properties**
- In the Properties panel, set the `Mask` property:
  - Click on the `Mask` property
  - Enter your desired mask pattern (e.g., `(###) ###-####` for phone)

**Step 5: Optional - Set Initial Text**
- Set the `Text` property to show a placeholder or initial value

## Adding MaskedEditBox via Code

**Step 1: Add Namespace**

```csharp
using Syncfusion.Windows.Forms.Tools;
```

**Step 2: Create Instance**

```csharp
MaskedEditBox maskedEditBox = new MaskedEditBox();
```

**Step 3: Configure Properties**

```csharp
maskedEditBox.Mask = "(###) ###-####";  // US phone format
maskedEditBox.Location = new Point(10, 10);
maskedEditBox.Size = new Size(200, 25);
maskedEditBox.Text = "";
```

**Step 4: Add to Form**

```csharp
this.Controls.Add(maskedEditBox);
```

## Complete Example

Here's a simple Windows Forms application with a MaskedEditBox for phone number input:

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class MainForm : Form
{
    private MaskedEditBox phoneInput;
    private Button submitButton;
    private Label resultLabel;

    public MainForm()
    {
        // Create MaskedEditBox for phone number
        phoneInput = new MaskedEditBox();
        phoneInput.Mask = "(###) ###-####";
        phoneInput.Location = new Point(10, 10);
        phoneInput.Size = new Size(200, 25);
        phoneInput.Text = "";

        // Create submit button
        submitButton = new Button();
        submitButton.Text = "Submit";
        submitButton.Location = new Point(10, 45);
        submitButton.Click += SubmitButton_Click;

        // Create result label
        resultLabel = new Label();
        resultLabel.Location = new Point(10, 80);
        resultLabel.Size = new Size(300, 50);

        // Add controls to form
        this.Controls.Add(phoneInput);
        this.Controls.Add(submitButton);
        this.Controls.Add(resultLabel);

        this.Text = "MaskedEditBox Example";
        this.Size = new Size(400, 200);
    }

    private void SubmitButton_Click(object sender, EventArgs e)
    {
        // Get value without mask characters
        string phoneValue = phoneInput.Value;
        resultLabel.Text = $"Phone: {phoneInput.Text}\nValue (no mask): {phoneValue}";
    }

    [STAThread]
    static void Main()
    {
        Application.EnableVisualStyles();
        Application.Run(new MainForm());
    }
}
```

## Key Points

- **Mask property:** Always set this before adding data; without it, MaskedEditBox behaves like a standard TextBox
- **Text vs Value:** 
  - `Text` includes mask literals: `(123) 456-7890`
  - `Value` contains only input characters: `1234567890`
- **Namespace:** Always include `using Syncfusion.Windows.Forms.Tools;` at the top of your class
- **Designer vs Code:** Both approaches are equivalent; choose based on your workflow preference

## Troubleshooting

**MaskedEditBox not appearing in Toolbox:**
- Ensure Syncfusion.Shared.Base assembly is referenced
- Rebuild the solution
- Close and reopen Visual Studio if needed

**InvalidOperationException when setting Mask:**
- Ensure the control is created before setting properties
- Check that the mask string uses valid mask characters (#, &, $, etc.)

**NuGet package installation fails:**
- Ensure you have internet connection
- Check NuGet package source is configured (Tools → Options → NuGet Package Manager)
- Update NuGet Package Manager to the latest version
