# Getting Started with Windows Forms AutoLabel

This section briefly describes how to create a new Windows Forms project in Visual Studio, and add the AutoLabel control with its basic functionalities.

## Assembly deployment

Refer to the [control dependencies](https://help.syncfusion.com/windowsforms/control-dependencies#autolabel) section to get the list of assemblies or NuGet package details that need to be added as reference to use the control in any application.

Refer to this [documentation](https://help.syncfusion.com/windowsforms/installation/install-nuget-packages) to find more details on how to install NuGet packages in a Windows Forms application.

## Creating Application with AutoLabel

In this walk through, users will create WinForms application that contains AutoLabel control.

### Creating the Project

Create new Windows Forms Project in Visual Studio to display AutoLabel Control.

### Adding the AutoLabel control via designer

1. Create a new Windows Forms application in Visual Studio.

2. The AutoLabel control can be added to an application by dragging it from the toolbox to the design view. The following dependent assemblies will be added automatically.

    * Syncfusion.Shared.Base

3. Set the desired properties for the AutoLabel control using the **Properties** dialog.

### Adding the AutoLabel control in Code

In order to add control manually, do the below steps:

1. Create a C# or VB application via Visual Studio.

2. Add the following assembly reference to the project:

    * Syncfusion.Shared.Base

3. Include the required namespace.

```csharp
using Syncfusion.Windows.Forms.Tools;
```

```vb
Imports Syncfusion.Windows.Forms.Tools
```

4. Create an instance of the AutoLabel control. Set the following properties and add it to the form.

```csharp
//Initialization
private Syncfusion.Windows.Forms.Tools.AutoLabel autoLabel1;
this.autoLabel1 = new Syncfusion.Windows.Forms.Tools.AutoLabel();

//Set the properties
this.autoLabel1.Text = "autoLabel1";
this.autoLabel1.BackColor = System.Drawing.Color.DarkGray;
this.autoLabel1.ForeColor = System.Drawing.Color.DarkBlue;
this.autoLabel1.Font = new System.Drawing.Font("Microsoft Sans Serif", 8.25F, System.Drawing.FontStyle.Bold, System.Drawing.GraphicsUnit.Point, ((byte)(0)));
this.autoLabel1.TextAlign = System.Drawing.ContentAlignment.MiddleCenter;

// Add the AutoLabel control to the form.
this.Controls.Add(this.autoLabel1);
```

```vb
' Initialization
Private autoLabel1 As Syncfusion.Windows.Forms.Tools.AutoLabel
Me.autoLabel1 = New Syncfusion.Windows.Forms.Tools.AutoLabel()

' Set the properties
Me.autoLabel1.Text = "autoLabel1"
Me.autoLabel1.BackColor = System.Drawing.Color.DarkGray
Me.autoLabel1.ForeColor = System.Drawing.Color.DarkBlue
Me.autoLabel1.Font = New System.Drawing.Font("Microsoft Sans Serif", 8.25F, System.Drawing.FontStyle.Bold, System.Drawing.GraphicsUnit.Point, CByte((0)))
Me.autoLabel1.TextAlign = System.Drawing.ContentAlignment.MiddleCenter

' Add the AutoLabel control to the form.
Me.Controls.Add(Me.autoLabel1)
```

## Labeling a control

1. Add one control to the form. For example, TextBoxExt.

2. Right-click on the AutoLabel control. Choose **Properties** and select `LabeledControl` property. Now, you can choose newly added **TextBoxExt** control.

```csharp
this.autoLabel1.LabeledControl = this.textBoxExt1;
```

```vb
Me.autoLabel1.LabeledControl = Me.textBoxExt1
```

Once paired, the AutoLabel will automatically track and reposition itself when the TextBoxExt control moves.

## Complete Example

Here's a complete working example of a form with AutoLabel:

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace AutoLabelGettingStarted
{
    public partial class Form1 : Form
    {
        private AutoLabel nameLabel;
        private AutoLabel emailLabel;
        private TextBoxExt nameTextBox;
        private TextBoxExt emailTextBox;
        
        public Form1()
        {
            InitializeComponent();
            
            // Create TextBox controls
            nameTextBox = new TextBoxExt();
            nameTextBox.Location = new Point(150, 50);
            nameTextBox.Size = new Size(200, 20);
            
            emailTextBox = new TextBoxExt();
            emailTextBox.Location = new Point(150, 100);
            emailTextBox.Size = new Size(200, 20);
            
            // Create AutoLabel for name
            nameLabel = new AutoLabel();
            nameLabel.Text = "Name:";
            nameLabel.LabeledControl = nameTextBox;
            nameLabel.Position = AutoLabelPosition.Left;
            nameLabel.Gap = 10;
            
            // Create AutoLabel for email
            emailLabel = new AutoLabel();
            emailLabel.Text = "Email:";
            emailLabel.LabeledControl = emailTextBox;
            emailLabel.Position = AutoLabelPosition.Left;
            emailLabel.Gap = 10;
            
            // Add controls to form
            this.Controls.Add(nameTextBox);
            this.Controls.Add(nameLabel);
            this.Controls.Add(emailTextBox);
            this.Controls.Add(emailLabel);
        }
    }
}
```

## Key Points

- **Always pair the label**: Set `LabeledControl` property to associate the label with a control
- **Add both controls**: Add both the AutoLabel and its paired control to the form
- **Position automatically**: The label will automatically position itself based on the `Position` property
- **Adjust spacing**: Use `Gap` property to control the distance between label and control

## Next Steps

- Learn about [positioning and spacing options](positioning-spacing.md)
- Explore [appearance and theming](appearance-theming.md)
- Understand [events and customization](events.md)
