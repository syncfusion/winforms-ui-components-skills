# Getting Started with RadioButtonAdv

## Overview

The **RadioButtonAdv** is an enhanced radio button control for Windows Forms applications that provides advanced styling, theming, and visual customization options beyond the standard .NET RadioButton control. It supports Office-style themes, gradient backgrounds, text effects, image-based radio buttons, and much more.

### Key Features

- **Advanced Theming**: Office 2007, Office 2016, Metro styles
- **Gradient Backgrounds**: Horizontal and vertical gradient support
- **Text Effects**: Text shadows with customizable colors and offsets
- **Custom Images**: Use custom images for checked/unchecked states
- **Border Customization**: 2D and 3D border styles
- **State Management**: Associate integer/string values with radio button states
- **Event Support**: Comprehensive event handling for state changes

## Assembly Dependencies

To use the RadioButtonAdv control, you need to add references to the following assemblies:

- **Syncfusion.Grid.Base.dll**
- **Syncfusion.Grid.Windows.dll**
- **Syncfusion.Shared.Base.dll**
- **Syncfusion.Shared.Windows.dll**
- **Syncfusion.Tools.Base.dll**
- **Syncfusion.Tools.Windows.dll**

### Adding via NuGet

You can also install the required packages via NuGet Package Manager:

```powershell
Install-Package Syncfusion.Tools.Windows
```

## Creating RadioButtonAdv via Designer

Follow these steps to add a RadioButtonAdv control through the Visual Studio designer:

### Step 1: Create a New Project

1. Open Visual Studio
2. Create a new **Windows Forms App (.NET Framework)** project
3. Name your project (e.g., "RadioButtonAdvDemo")

### Step 2: Add Control from Toolbox

1. Open the **Toolbox** panel (View → Toolbox or Ctrl+Alt+X)
2. Locate **RadioButtonAdv** in the Syncfusion Controls section
3. Drag and drop the **RadioButtonAdv** control onto your form

When you add the control via the designer, Visual Studio automatically adds the required assembly references listed above.

![RadioButtonAdv in Toolbox](../images/radiobuttonadv-toolbox.png)

### Step 3: Configure Properties

1. Select the RadioButtonAdv control on the form
2. Open the **Properties** window (View → Properties Window or F4)
3. Set basic properties:
   - **Text**: "Option 1"
   - **Font**: Microsoft Sans Serif, 10pt
   - **ForeColor**: Navy
   - **BackColor**: White

## Creating RadioButtonAdv via Code

You can also create and configure RadioButtonAdv controls programmatically.

### Step 1: Create a Project

Create a new Windows Forms application project in Visual Studio.

### Step 2: Add Assembly References

Add references to the six required Syncfusion assemblies listed in the Assembly Dependencies section above.

### Step 3: Include Namespace

Add the following namespace at the top of your form's code file:

**C#:**
```csharp
using Syncfusion.Windows.Forms.Tools;
# Quick start — RadioButtonAdv (condensed)

This short guide shows the minimal steps to add a `RadioButtonAdv` to a WinForms form using C#.

## Prerequisites

- Syncfusion WinForms installed and referenced in the project.

## Minimal code example (C#)

```csharp
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class MainForm : Form
{
    public MainForm()
    {
        Text = "RadioButtonAdv Quick Start";
        Size = new Size(420, 200);

        var r1 = new RadioButtonAdv { Text = "Option A", Location = new Point(20, 20), Size = new Size(360, 30) };
        var r2 = new RadioButtonAdv { Text = "Option B", Location = new Point(20, 60), Size = new Size(360, 30) };
        Controls.Add(r1); Controls.Add(r2);
    }
}
```

## Notes

- Use the Designer for visual placement or code for dynamic creation.
- Common properties: `Checked`, `Text`, `BackgroundStyle`, `GradientStart`, `GradientEnd`, `BorderStyle`, `ImageCheckBox`.

## Next steps

- See `appearance-customization.md` for styling examples.
- See `events.md` for selection and change handling.

            this.radioOption3 = new RadioButtonAdv();
            this.radioOption3.Text = "Enterprise Option";
            this.radioOption3.Location = new Point(30, 100);
            this.radioOption3.Size = new Size(200, 25);
            this.radioOption3.Style = RadioButtonAdvStyle.Office2016Colorful;
            this.radioOption3.CheckedChanged += RadioButton_CheckedChanged;
            this.Controls.Add(this.radioOption3);

            // Submit button
            this.btnSubmit = new Button();
            this.btnSubmit.Text = "Submit";
            this.btnSubmit.Location = new Point(30, 140);
            this.btnSubmit.Size = new Size(100, 30);
            this.btnSubmit.Click += BtnSubmit_Click;
            this.Controls.Add(this.btnSubmit);

            // Form settings
            this.Text = "RadioButtonAdv Demo";
            this.Size = new Size(350, 230);
        }

        private void RadioButton_CheckedChanged(object sender, EventArgs e)
        {
            RadioButtonAdv radio = sender as RadioButtonAdv;
            if (radio != null && radio.Checked)
            {
                Console.WriteLine($"{radio.Text} selected");
            }
        }

        private void BtnSubmit_Click(object sender, EventArgs e)
        {
            string selectedOption = string.Empty;
            
            if (radioOption1.Checked)
                selectedOption = radioOption1.Text;
            else if (radioOption2.Checked)
                selectedOption = radioOption2.Text;
            else if (radioOption3.Checked)
                selectedOption = radioOption3.Text;

            MessageBox.Show($"You selected: {selectedOption}", 
                          "Selection Result", 
                          MessageBoxButtons.OK, 
                          MessageBoxIcon.Information);
        }
    }
}
```

**VB.NET:**
```vb
Imports System.Drawing
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Public Class Form1
    Private radioOption1 As RadioButtonAdv
    Private radioOption2 As RadioButtonAdv
    Private radioOption3 As RadioButtonAdv
    Private lblTitle As Label
    Private btnSubmit As Button

    Public Sub New()
        InitializeComponent()
        InitializeRadioButtons()
    End Sub

    Private Sub InitializeRadioButtons()
        ' Title label
        Me.lblTitle = New Label()
        Me.lblTitle.Text = "Select Your Preference:"
        Me.lblTitle.Location = New Point(20, 10)
        Me.lblTitle.Size = New Size(300, 20)
        Me.lblTitle.Font = New Font("Segoe UI", 12.0F, FontStyle.Bold)
        Me.Controls.Add(Me.lblTitle)

        ' First radio button
        Me.radioOption1 = New RadioButtonAdv()
        Me.radioOption1.Text = "Standard Option"
        Me.radioOption1.Location = New Point(30, 40)
        Me.radioOption1.Size = New Size(200, 25)
        Me.radioOption1.Style = RadioButtonAdvStyle.Office2016Colorful
        Me.radioOption1.Checked = True
        AddHandler radioOption1.CheckedChanged, AddressOf RadioButton_CheckedChanged
        Me.Controls.Add(Me.radioOption1)

        ' Second radio button
        Me.radioOption2 = New RadioButtonAdv()
        Me.radioOption2.Text = "Premium Option"
        Me.radioOption2.Location = New Point(30, 70)
        Me.radioOption2.Size = New Size(200, 25)
        Me.radioOption2.Style = RadioButtonAdvStyle.Office2016Colorful
        AddHandler radioOption2.CheckedChanged, AddressOf RadioButton_CheckedChanged
        Me.Controls.Add(Me.radioOption2)

        ' Third radio button
        Me.radioOption3 = New RadioButtonAdv()
        Me.radioOption3.Text = "Enterprise Option"
        Me.radioOption3.Location = New Point(30, 100)
        Me.radioOption3.Size = New Size(200, 25)
        Me.radioOption3.Style = RadioButtonAdvStyle.Office2016Colorful
        AddHandler radioOption3.CheckedChanged, AddressOf RadioButton_CheckedChanged
        Me.Controls.Add(Me.radioOption3)

        ' Submit button
        Me.btnSubmit = New Button()
        Me.btnSubmit.Text = "Submit"
        Me.btnSubmit.Location = New Point(30, 140)
        Me.btnSubmit.Size = New Size(100, 30)
        AddHandler btnSubmit.Click, AddressOf BtnSubmit_Click
        Me.Controls.Add(Me.btnSubmit)

        ' Form settings
        Me.Text = "RadioButtonAdv Demo"
        Me.Size = New Size(350, 230)
    End Sub

    Private Sub RadioButton_CheckedChanged(sender As Object, e As EventArgs)
        Dim radio As RadioButtonAdv = TryCast(sender, RadioButtonAdv)
        If radio IsNot Nothing AndAlso radio.Checked Then
            Console.WriteLine($"{radio.Text} selected")
        End If
    End Sub

    Private Sub BtnSubmit_Click(sender As Object, e As EventArgs)
        Dim selectedOption As String = String.Empty

        If radioOption1.Checked Then
            selectedOption = radioOption1.Text
        ElseIf radioOption2.Checked Then
            selectedOption = radioOption2.Text
        ElseIf radioOption3.Checked Then
            selectedOption = radioOption3.Text
        End If

        MessageBox.Show($"You selected: {selectedOption}", _
                       "Selection Result", _
                       MessageBoxButtons.OK, _
                       MessageBoxIcon.Information)
    End Sub
End Class
```

## Common Properties

Here's a quick reference of commonly used properties:

| Property | Description | Default Value |
|----------|-------------|---------------|
| `Text` | Gets or sets the text displayed next to the radio button | Empty string |
| `Checked` | Gets or sets whether the radio button is checked | False |
| `Font` | Gets or sets the font of the text | System default |
| `ForeColor` | Gets or sets the foreground color of the text | System default |
| `BackColor` | Gets or sets the background color | System default |
| `Style` | Gets or sets the visual style (Default, Office2007, Metro, etc.) | Default |
| `ThemesEnabled` | Enables or disables Windows themes | False |
| `AutoHeight` | Automatically calculates control height | False |

## Next Steps

Now that you've created your first RadioButtonAdv control, explore these advanced features:

- **[Themes and Styles](themes-and-styles.md)**: Apply Office and Metro themes
- **[Text Settings](text-settings.md)**: Add shadows and text effects
- **[Appearance Customization](appearance-customization.md)**: Gradients, borders, and images
- **[Behavior Settings](behavior-settings.md)**: Configure control behavior
- **[Events](events.md)**: Handle user interactions

## Troubleshooting

### Control Not Visible in Toolbox

If RadioButtonAdv doesn't appear in the toolbox:
1. Right-click on the Toolbox and select "Choose Items"
2. Browse to the Syncfusion.Tools.Windows.dll assembly
3. Check the RadioButtonAdv control and click OK

### Assembly Reference Issues

If you get errors about missing assemblies:
1. Ensure all six required assemblies are referenced
2. Verify the assemblies are for the correct .NET Framework version
3. Check that the Syncfusion version is consistent across all assemblies

### Radio Buttons Not Mutually Exclusive

Radio buttons are only mutually exclusive when they share the same parent container. Ensure your RadioButtonAdv controls are:
- Added to the same form or panel
- Not separated by different GroupBox controls (unless that's intentional)
