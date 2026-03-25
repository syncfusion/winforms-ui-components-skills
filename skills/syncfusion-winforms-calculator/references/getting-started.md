# Getting Started with Calculator Control

## Assembly Dependencies

The Calculator control requires several Syncfusion assemblies to function. When adding the control via Visual Studio Designer, these are automatically added to your project.

**Required Assemblies:**
- Syncfusion.Tools.Base
- Syncfusion.Tools.Windows
- Syncfusion.Shared.Base
- Syncfusion.Shared.Windows
- Syncfusion.Grid.Base
- Syncfusion.Grid.Windows

**Installation via NuGet:**
```
Install-Package Syncfusion.Tools.Windows
```

Refer to the [NuGet installation guide](https://help.syncfusion.com/windowsforms/installation/install-nuget-packages) for detailed steps.

## Create a Windows Forms Project

1. Open Visual Studio
2. Create a new **Windows Forms Application** project
3. Select .NET Framework version (typically 4.5 or later)

## Add Calculator Control

### Method 1: Designer (Recommended for Quick Setup)

1. Open your Form in Visual Studio Designer
2. Locate the **Toolbox**
3. Search for or scroll to **Calculator**
4. Drag the Calculator control onto your form
5. Assemblies are automatically added to your project
6. Resize the control as needed

### Method 2: Manual Code Addition

**Step 1: Add Assembly References**
- Right-click **References** in Solution Explorer
- Select **Add Reference**
- Browse to Syncfusion assemblies location
- Add all 6 assemblies listed above

**Step 2: Include Namespace**

```csharp
using Syncfusion.Windows.Forms.Tools;
```

```vb
Imports Syncfusion.Windows.Forms.Tools
```

**Step 3: Create and Add Control**

```csharp
CalculatorControl calculatorControl = new CalculatorControl();
calculatorControl.Size = new System.Drawing.Size(300, 250);
this.Controls.Add(calculatorControl);
```

```vb
Dim calculatorControl As CalculatorControl = New CalculatorControl
calculatorControl.Size = New System.Drawing.Size(300, 250)
Me.Controls.Add(calculatorControl)
```

## Basic Initialization

The Calculator control initializes with sensible defaults:
- **Layout:** Windows Standard mode (standard calculator appearance)
- **Display:** Shows calculation display area
- **Size:** Adjust to fit your form layout
- **Keyboard:** Full keyboard support enabled by default

## First Calculator Example

```csharp
public partial class MainForm : Form
{
    public MainForm()
    {
        InitializeComponent();
        SetupCalculator();
    }

    private void SetupCalculator()
    {
        // Create calculator
        CalculatorControl calc = new CalculatorControl();
        calc.Size = new System.Drawing.Size(320, 280);
        calc.Location = new System.Drawing.Point(10, 10);
        
        // Add event handler
        calc.ValueCalculated += Calculator_ValueCalculated;
        
        // Add to form
        this.Controls.Add(calc);
    }

    private void Calculator_ValueCalculated(object sender, CalculatorValueCalculatedEventArgs args)
    {
        if (!args.ErrorCondition)
        {
            // Process calculation
            CalculatorControl calc = sender as CalculatorControl;
            this.Text = $"Current Value: {calc.DoubleValue}";
        }
    }
}
```

```vb
Public Class MainForm
    Inherits Form

    Public Sub New()
        InitializeComponent()
        SetupCalculator()
    End Sub

    Private Sub SetupCalculator()
        ' Create calculator
        Dim calc As New CalculatorControl()
        calc.Size = New System.Drawing.Size(320, 280)
        calc.Location = New System.Drawing.Point(10, 10)

        ' Add event handler
        AddHandler calc.ValueCalculated, AddressOf Calculator_ValueCalculated

        ' Add to form
        Me.Controls.Add(calc)
    End Sub

    Private Sub Calculator_ValueCalculated(sender As Object, args As CalculatorValueCalculatedEventArgs)
        If Not args.ErrorCondition Then
            ' Process calculation
            Dim calc As CalculatorControl = DirectCast(sender, CalculatorControl)
            Me.Text = String.Format("Current Value: {0}", calc.DoubleValue)
        End If
    End Sub
End Class
```

## Verify Setup

To verify your calculator is working:
1. Run your application (F5)
2. Click numbers on the calculator or use keyboard (e.g., type "5 + 3 =")
3. Calculator should display the result
4. No assemblies missing error indicates successful setup
