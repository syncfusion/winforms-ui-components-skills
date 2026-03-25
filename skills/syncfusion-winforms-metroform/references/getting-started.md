# Getting Started with MetroForm

This guide covers the complete setup process for implementing MetroForm in your Windows Forms application, from installation to creating your first Metro-styled form.

## Installation and Assembly Deployment

### NuGet Package Installation

The recommended way to add MetroForm to your project is via NuGet Package Manager.

**Using Package Manager Console:**
```bash
Install-Package Syncfusion.Shared.Base
```

**Using NuGet Package Manager UI:**
1. Right-click your project in Solution Explorer
2. Select "Manage NuGet Packages"
3. Search for "Syncfusion.Shared.Base"
4. Click "Install"

**More details:** [How to install NuGet packages](https://help.syncfusion.com/windowsforms/installation/install-nuget-packages)

### Required Assembly References

After installing the NuGet package, ensure your project references:
- `Syncfusion.Shared.Base.dll`

This assembly contains the core MetroForm functionality.

**To verify assembly references:**
1. Right-click "References" in Solution Explorer
2. Select "Add Reference"
3. Browse to the Syncfusion installation directory
4. Locate and add `Syncfusion.Shared.Base.dll`

## Creating Your First MetroForm Application

### Step 1: Create the Project

1. Open Visual Studio
2. Create a new **Windows Forms App (.NET Framework)** or **Windows Forms App (.NET)**
3. Choose your target framework (.NET Framework 4.0+ or .NET Core 3.0+)
4. Name your project (e.g., "MyMetroApp")
5. Click "Create"

### Step 2: Add the Namespace

Open your form's code file (typically `Form1.cs` or `MainForm.cs`) and add the Syncfusion namespace at the top:

**C#:**
```csharp
using Syncfusion.Windows.Forms;
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms
```

### Step 3: Change Form Inheritance

Change your form class to inherit from `MetroForm` instead of the default `Form`.

**C#:**
```csharp
namespace MyMetroApp
{
    // Change from: public partial class Form1 : Form
    public partial class Form1 : MetroForm
    {
        public Form1()
        {
            InitializeComponent();
        }
    }
}
```

**VB.NET:**
```vb
Namespace MyMetroApp
    ' Change from: Public Class Form1
    Public Partial Class Form1
        Inherits MetroForm
        
        Public Sub New()
            InitializeComponent()
        End Sub
    End Class
End Namespace
```

### Step 4: Run the Application

Build and run your application. Your form will now display with the Metro UI style:

![Metro Form Appearance](../images/MetroForm_Basic.png)

## Basic Configuration

Once your form inherits from MetroForm, you can configure its appearance in the constructor:

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Set form title
    this.Text = "My Metro Application";
    
    // Configure caption bar
    this.CaptionBarHeight = 40;
    this.CaptionBarColor = Color.FromArgb(17, 158, 218);
    this.CaptionForeColor = Color.White;
    
    // Configure border
    this.BorderColor = Color.FromArgb(17, 158, 218);
    this.BorderThickness = 2;
}
```

**VB.NET:**
```vb
Public Sub New()
    InitializeComponent()
    
    ' Set form title
    Me.Text = "My Metro Application"
    
    ' Configure caption bar
    Me.CaptionBarHeight = 40
    Me.CaptionBarColor = Color.FromArgb(17, 158, 218)
    Me.CaptionForeColor = Color.White
    
    ' Configure border
    Me.BorderColor = Color.FromArgb(17, 158, 218)
    Me.BorderThickness = 2
End Sub
```

## Adding Caption Labels (Basic)

Caption labels allow you to display text in the caption bar alongside the form title.

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Create caption label
    CaptionLabel captionLabel = new CaptionLabel();
    captionLabel.Text = "Dashboard";
    captionLabel.Font = new Font("Microsoft Sans Serif", 10F, FontStyle.Regular);
    captionLabel.ForeColor = Color.White;
    captionLabel.Size = new Size(400, 24);
    captionLabel.Name = "CaptionLabel1";
    
    // Add to caption bar
    this.CaptionLabels.Add(captionLabel);
}
```

**VB.NET:**
```vb
Public Sub New()
    InitializeComponent()
    
    ' Create caption label
    Dim captionLabel As New CaptionLabel()
    captionLabel.Text = "Dashboard"
    captionLabel.Font = New Font("Microsoft Sans Serif", 10F, FontStyle.Regular)
    captionLabel.ForeColor = Color.White
    captionLabel.Size = New Size(400, 24)
    captionLabel.Name = "CaptionLabel1"
    
    ' Add to caption bar
    Me.CaptionLabels.Add(captionLabel)
End Sub
```

## Adding Caption Images (Basic)

Caption images allow you to display icons or logos in the caption bar.

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Create caption image
    CaptionImage captionImage = new CaptionImage();
    captionImage.BackColor = Color.Transparent;
    captionImage.Image = Properties.Resources.AppIcon; // Load from resources
    captionImage.Location = new Point(30, 5);
    captionImage.Size = new Size(35, 35);
    captionImage.Name = "CaptionImage1";
    
    // Add to caption bar
    this.CaptionImages.Add(captionImage);
}
```

**VB.NET:**
```vb
Public Sub New()
    InitializeComponent()
    
    ' Create caption image
    Dim captionImage As New CaptionImage()
    captionImage.BackColor = Color.Transparent
    captionImage.Image = My.Resources.AppIcon ' Load from resources
    captionImage.Location = New Point(30, 5)
    captionImage.Size = New Size(35, 35)
    captionImage.Name = "CaptionImage1"
    
    ' Add to caption bar
    Me.CaptionImages.Add(captionImage)
End Sub
```

## Complete Example

Here's a complete example of a basic MetroForm setup with caption label and image:

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;

namespace MyMetroApp
{
    public partial class MainForm : MetroForm
    {
        public MainForm()
        {
            InitializeComponent();
            
            // Configure form
            this.Text = "My Metro Application";
            this.Size = new Size(800, 600);
            
            // Configure caption bar appearance
            this.CaptionBarHeight = 45;
            this.CaptionBarColor = Color.FromArgb(41, 128, 185);
            this.CaptionForeColor = Color.White;
            this.CaptionFont = new Font("Segoe UI", 10F, FontStyle.Regular);
            
            // Configure border
            this.BorderColor = Color.FromArgb(41, 128, 185);
            this.BorderThickness = 1;
            
            // Add caption image (logo)
            CaptionImage logo = new CaptionImage
            {
                Image = Properties.Resources.AppLogo,
                Location = new Point(10, 8),
                Size = new Size(30, 30),
                BackColor = Color.Transparent
            };
            this.CaptionImages.Add(logo);
            
            // Add caption label
            CaptionLabel titleLabel = new CaptionLabel
            {
                Text = "Welcome",
                Font = new Font("Segoe UI Semibold", 11F),
                ForeColor = Color.White,
                Size = new Size(200, 30),
                Location = new Point(50, 8)
            };
            this.CaptionLabels.Add(titleLabel);
        }
    }
}
```

**VB.NET:**
```vb
Imports System
Imports System.Drawing
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms

Namespace MyMetroApp
    Public Partial Class MainForm
        Inherits MetroForm
        
        Public Sub New()
            InitializeComponent()
            
            ' Configure form
            Me.Text = "My Metro Application"
            Me.Size = New Size(800, 600)
            
            ' Configure caption bar appearance
            Me.CaptionBarHeight = 45
            Me.CaptionBarColor = Color.FromArgb(41, 128, 185)
            Me.CaptionForeColor = Color.White
            Me.CaptionFont = New Font("Segoe UI", 10F, FontStyle.Regular)
            
            ' Configure border
            Me.BorderColor = Color.FromArgb(41, 128, 185)
            Me.BorderThickness = 1
            
            ' Add caption image (logo)
            Dim logo As New CaptionImage With {
                .Image = My.Resources.AppLogo,
                .Location = New Point(10, 8),
                .Size = New Size(30, 30),
                .BackColor = Color.Transparent
            }
            Me.CaptionImages.Add(logo)
            
            ' Add caption label
            Dim titleLabel As New CaptionLabel With {
                .Text = "Welcome",
                .Font = New Font("Segoe UI Semibold", 11F),
                .ForeColor = Color.White,
                .Size = New Size(200, 30),
                .Location = New Point(50, 8)
            }
            Me.CaptionLabels.Add(titleLabel)
        End Sub
    End Class
End Namespace
```

## Next Steps

Now that you have a basic MetroForm set up, explore these advanced features:

- **Caption Customization** - Learn advanced techniques for caption labels and images
- **Appearance and Styling** - Customize colors, borders, alignment, and more
- **Advanced Customization** - Implement custom painting, gradients, and event handling

## Troubleshooting

### MetroForm Style Not Applied

**Problem:** Form still looks like a standard Windows Form.

**Solutions:**
- Verify the class inherits from `MetroForm`, not `Form`
- Ensure `using Syncfusion.Windows.Forms;` is at the top of your file
- Check that `Syncfusion.Shared.Base.dll` is referenced in your project
- Clean and rebuild your solution

### Missing Syncfusion Namespace

**Problem:** `MetroForm` type not recognized or namespace not found.

**Solutions:**
- Install the `Syncfusion.Windows.Forms` NuGet package
- Add assembly reference to `Syncfusion.Shared.Base.dll`
- Verify the package installation completed successfully

### Designer Errors

**Problem:** Visual Studio designer shows errors after changing to MetroForm.

**Solutions:**
- Close and reopen the designer
- Clean and rebuild the solution
- Restart Visual Studio
- Ensure all Syncfusion assemblies are in the same location

### Form Title Not Visible

**Problem:** Form text not showing in caption bar.

**Solutions:**
- Set `this.Text = "Your Title";` property
- Verify `CaptionForeColor` contrasts with `CaptionBarColor`
- Check `CaptionBarHeight` is sufficient (minimum 30-35 pixels)
- Ensure `CaptionFont` size is appropriate
