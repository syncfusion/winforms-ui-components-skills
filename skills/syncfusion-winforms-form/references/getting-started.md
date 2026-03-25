# Getting Started with SfForm

This guide covers the essential steps to set up and use the Syncfusion Windows Forms `SfForm` control in your .NET desktop application.

## Assembly Deployment

Before using SfForm, add the following assembly references to your Windows Forms project:

**Required Assemblies:**
- `Syncfusion.Core.WinForms.dll`
- `Syncfusion.Shared.Base.dll`

**Installation Methods:**

### Via NuGet Package Manager
```powershell
Install-Package Syncfusion.Core.WinForms
Install-Package Syncfusion.Shared.Base
```

### Via Project References
1. Right-click on **References** in Solution Explorer
2. Select **Add Reference**
3. Browse to Syncfusion installation directory
4. Add the required DLLs

**Default Installation Path:**
```
C:\Program Files (x86)\Syncfusion\Essential Studio\<Version>\precompiledassemblies\<Framework Version>\
```

## Converting Standard Form to SfForm

### Step 1: Create Windows Forms Application

Create a new Windows Forms Application project in Visual Studio:
1. **File** → **New** → **Project**
2. Select **Windows Forms App (.NET Framework)** or **Windows Forms App**
3. Name your project and click **Create**

### Step 2: Add Assembly References

Add the required Syncfusion assemblies as described in the Assembly Deployment section above.

### Step 3: Add Namespace

Include the Syncfusion namespace in your form's code file:

**C#:**
```csharp
using Syncfusion.WinForms.Controls;
```

**VB.NET:**
```vb
Imports Syncfusion.WinForms.Controls
```

### Step 4: Change Base Class

Modify your form to inherit from `SfForm` instead of `Form`:

**C# (Form1.cs):**
```csharp
using System;
using System.Windows.Forms;
using Syncfusion.WinForms.Controls;

namespace MyWinFormsApp
{
    public partial class Form1 : SfForm  // Changed from Form to SfForm
    {
        public Form1()
        {
            InitializeComponent();
        }
    }
}
```

**VB.NET (Form1.vb):**
```vb
Imports System.Windows.Forms
Imports Syncfusion.WinForms.Controls

Public Class Form1
    Inherits SfForm  ' Changed from Form to SfForm
    
    Public Sub New()
        InitializeComponent()
    End Sub
End Class
```

### Step 5: Update Designer File (Important)

You also need to update the designer file to reflect the base class change:

**C# (Form1.Designer.cs):**
```csharp
partial class Form1
{
    // Change the type from Form to SfForm
    // Look for: private void InitializeComponent()
    // And ensure the class inheritance is correct
}
```

**Tip:** It's often easier to delete the form and create a new one with SfForm as the base class from the start.

## Basic Title Bar Customization

Once your form inherits from `SfForm`, you can customize the title bar appearance:

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Set title bar colors
    this.Style.TitleBar.BackColor = Color.Black;
    this.Style.TitleBar.ForeColor = Color.White;
    
    // Customize title bar buttons
    this.Style.TitleBar.CloseButtonForeColor = Color.White;
    this.Style.TitleBar.MinimizeButtonForeColor = Color.White;
    this.Style.TitleBar.MaximizeButtonForeColor = Color.White;
    
    // Set button hover colors
    this.Style.TitleBar.CloseButtonHoverBackColor = Color.DarkGray;
    this.Style.TitleBar.MinimizeButtonHoverBackColor = Color.DarkGray;
    this.Style.TitleBar.MaximizeButtonHoverBackColor = Color.DarkGray;
    
    // Set button pressed colors
    this.Style.TitleBar.CloseButtonPressedBackColor = Color.Gray;
    this.Style.TitleBar.MaximizeButtonPressedBackColor = Color.Gray;
    this.Style.TitleBar.MinimizeButtonPressedBackColor = Color.Gray;
}
```

**VB.NET:**
```vb
Public Sub New()
    InitializeComponent()
    
    ' Set title bar colors
    Me.Style.TitleBar.BackColor = Color.Black
    Me.Style.TitleBar.ForeColor = Color.White
    
    ' Customize title bar buttons
    Me.Style.TitleBar.CloseButtonForeColor = Color.White
    Me.Style.TitleBar.MinimizeButtonForeColor = Color.White
    Me.Style.TitleBar.MaximizeButtonForeColor = Color.White
    
    ' Set button hover colors
    Me.Style.TitleBar.CloseButtonHoverBackColor = Color.DarkGray
    Me.Style.TitleBar.MinimizeButtonHoverBackColor = Color.DarkGray
    Me.Style.TitleBar.MaximizeButtonHoverBackColor = Color.DarkGray
    
    ' Set button pressed colors
    Me.Style.TitleBar.CloseButtonPressedBackColor = Color.Gray
    Me.Style.TitleBar.MaximizeButtonPressedBackColor = Color.Gray
    Me.Style.TitleBar.MinimizeButtonPressedBackColor = Color.Gray
End Sub
```

**Result:** The title bar will have a black background with white text and buttons, with visual feedback on hover and press.

## Basic Border Customization

Customize the form border for both active and inactive states:

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Active border (when form has focus)
    this.Style.Border = new Pen(Color.Black, 5);
    
    // Inactive border (when form loses focus)
    this.Style.InactiveBorder = new Pen(Color.Gray, 5);
}
```

**VB.NET:**
```vb
Public Sub New()
    InitializeComponent()
    
    ' Active border (when form has focus)
    Me.Style.Border = New Pen(Color.Black, 5)
    
    ' Inactive border (when form loses focus)
    Me.Style.InactiveBorder = New Pen(Color.Gray, 5)
End Sub
```

**Parameters:**
- **Color:** Border color
- **Width:** Border thickness in pixels

## Loading User Control to Title Bar

One of the most powerful features of SfForm is the ability to load any user control into the title bar.

### Example: Search Box in Title Bar

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Create a panel to hold controls
    FlowLayoutPanel searchPanel = new FlowLayoutPanel();
    searchPanel.FlowDirection = FlowDirection.LeftToRight;
    
    // Create search label
    Label searchingLabel = new Label();
    searchingLabel.Text = "Search:";
    searchingLabel.ForeColor = Color.White;
    searchingLabel.AutoSize = true;
    searchingLabel.Padding = new Padding(5, 5, 0, 0);
    
    // Create search textbox
    TextBox searchBox = new TextBox();
    searchBox.Width = 200;
    
    // Add controls to panel
    searchPanel.Controls.Add(searchingLabel);
    searchPanel.Controls.Add(searchBox);
    
    // Set panel size to fit in title bar
    searchPanel.Size = new Size(280, 25);
    
    // Load the panel to title bar
    this.TitleBarTextControl = searchPanel;
    
    // Style the title bar
    this.Style.TitleBar.BackColor = Color.FromArgb(0, 122, 204);
    this.Style.TitleBar.Height = 35;
}
```

**VB.NET:**
```vb
Public Sub New()
    InitializeComponent()
    
    ' Create a panel to hold controls
    Dim searchPanel As New FlowLayoutPanel()
    searchPanel.FlowDirection = FlowDirection.LeftToRight
    
    ' Create search label
    Dim searchingLabel As New Label()
    searchingLabel.Text = "Search:"
    searchingLabel.ForeColor = Color.White
    searchingLabel.AutoSize = True
    searchingLabel.Padding = New Padding(5, 5, 0, 0)
    
    ' Create search textbox
    Dim searchBox As New TextBox()
    searchBox.Width = 200
    
    ' Add controls to panel
    searchPanel.Controls.Add(searchingLabel)
    searchPanel.Controls.Add(searchBox)
    
    ' Set panel size to fit in title bar
    searchPanel.Size = New Size(280, 25)
    
    ' Load the panel to title bar
    Me.TitleBarTextControl = searchPanel
    
    ' Style the title bar
    Me.Style.TitleBar.BackColor = Color.FromArgb(0, 122, 204)
    Me.Style.TitleBar.Height = 35
End Sub
```

**Important Notes:**
- Size the user control appropriately to fit within the title bar height
- Consider the space needed for window buttons (close, minimize, maximize)
- The custom control replaces the form's text in the title bar
- Use `FlowLayoutPanel` or `TableLayoutPanel` for complex layouts

## Complete Getting Started Example

Here's a complete example that demonstrates basic SfForm setup with customization:

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.WinForms.Controls;

namespace SfFormGettingStarted
{
    public partial class Form1 : SfForm
    {
        public Form1()
        {
            InitializeComponent();
            
            // Configure form
            this.Text = "SfForm Getting Started";
            this.Size = new Size(800, 600);
            
            // Title bar customization
            this.Style.TitleBar.BackColor = Color.FromArgb(0, 120, 215);
            this.Style.TitleBar.ForeColor = Color.White;
            this.Style.TitleBar.Height = 35;
            this.Style.TitleBar.TextHorizontalAlignment = HorizontalAlignment.Left;
            
            // Button customization
            this.Style.TitleBar.CloseButtonForeColor = Color.White;
            this.Style.TitleBar.MinimizeButtonForeColor = Color.White;
            this.Style.TitleBar.MaximizeButtonForeColor = Color.White;
            this.Style.TitleBar.CloseButtonHoverBackColor = Color.FromArgb(232, 17, 35);
            this.Style.TitleBar.MinimizeButtonHoverBackColor = Color.FromArgb(0, 100, 180);
            this.Style.TitleBar.MaximizeButtonHoverBackColor = Color.FromArgb(0, 100, 180);
            
            // Border customization
            this.Style.Border = new Pen(Color.FromArgb(0, 120, 215), 2);
            this.Style.InactiveBorder = new Pen(Color.Gray, 2);
            
            // Shadow
            this.Style.ShadowOpacity = 150;
            this.Style.InactiveShadowOpacity = 80;
            
            // Client area
            this.Style.BackColor = Color.White;
        }
    }
}
```

**VB.NET:**
```vb
Imports System.Drawing
Imports System.Windows.Forms
Imports Syncfusion.WinForms.Controls

Public Class Form1
    Inherits SfForm
    
    Public Sub New()
        InitializeComponent()
        
        ' Configure form
        Me.Text = "SfForm Getting Started"
        Me.Size = New Size(800, 600)
        
        ' Title bar customization
        Me.Style.TitleBar.BackColor = Color.FromArgb(0, 120, 215)
        Me.Style.TitleBar.ForeColor = Color.White
        Me.Style.TitleBar.Height = 35
        Me.Style.TitleBar.TextHorizontalAlignment = HorizontalAlignment.Left
        
        ' Button customization
        Me.Style.TitleBar.CloseButtonForeColor = Color.White
        Me.Style.TitleBar.MinimizeButtonForeColor = Color.White
        Me.Style.TitleBar.MaximizeButtonForeColor = Color.White
        Me.Style.TitleBar.CloseButtonHoverBackColor = Color.FromArgb(232, 17, 35)
        Me.Style.TitleBar.MinimizeButtonHoverBackColor = Color.FromArgb(0, 100, 180)
        Me.Style.TitleBar.MaximizeButtonHoverBackColor = Color.FromArgb(0, 100, 180)
        
        ' Border customization
        Me.Style.Border = New Pen(Color.FromArgb(0, 120, 215), 2)
        Me.Style.InactiveBorder = New Pen(Color.Gray, 2)
        
        ' Shadow
        Me.Style.ShadowOpacity = 150
        Me.Style.InactiveShadowOpacity = 80
        
        ' Client area
        Me.Style.BackColor = Color.White
    End Sub
End Class
```

## License Registration

For production use, register your Syncfusion license key in the `Program.cs` file:

**C#:**
```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Licensing;

namespace SfFormGettingStarted
{
    static class Program
    {
        [STAThread]
        static void Main()
        {
            // Register Syncfusion license
            SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY_HERE");
            
            Application.EnableVisualStyles();
            Application.SetCompatibleTextRenderingDefault(false);
            Application.Run(new Form1());
        }
    }
}
```

**VB.NET:**
```vb
Imports System.Windows.Forms
Imports Syncfusion.Licensing

Module Program
    <STAThread>
    Sub Main()
        ' Register Syncfusion license
        SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY_HERE")
        
        Application.EnableVisualStyles()
        Application.SetCompatibleTextRenderingDefault(False)
        Application.Run(New Form1())
    End Sub
End Module
```

## Next Steps

Now that you have SfForm set up, explore these advanced features:

1. **Title Bar Customization** - Learn about text alignment, icon positioning, rich text, and button states
2. **Form Customization** - Explore icon alignment, shadow effects, and rounded corners
3. **MDI Applications** - Create parent-child form hierarchies
4. **Theming** - Apply professional built-in themes
5. **Localization** - Support multiple languages

## Troubleshooting

### Form doesn't show custom styling
- Ensure you're inheriting from `SfForm`, not `Form`
- Verify assemblies are referenced correctly
- Check that styling code runs after `InitializeComponent()`

### Visual Studio designer issues
- Rebuild the project after changing base class
- Close and reopen the designer
- If problems persist, manually edit the `.Designer.cs` file

### Missing assemblies
- Verify Syncfusion is installed correctly
- Check NuGet package versions match
- Ensure `.dll` files are in the correct framework folder

### Title bar custom control not visible
- Verify the control's size fits within title bar height
- Check `TitleBarTextControl` is assigned after control setup
- Ensure control's `Visible` property is `true`

## Quick Reference

### Essential Namespaces
```csharp
using Syncfusion.WinForms.Controls;
using Syncfusion.Licensing;  // For license registration
```

### Core Properties
- `Style.TitleBar.BackColor` - Title bar background
- `Style.TitleBar.ForeColor` - Title bar text color
- `Style.Border` - Active border pen
- `Style.InactiveBorder` - Inactive border pen
- `TitleBarTextControl` - Custom control in title bar

### Common Color Values
```csharp
Color.FromArgb(0, 120, 215)    // Windows blue
Color.FromArgb(232, 17, 35)    // Windows red
Color.FromArgb(46, 46, 46)     // Dark gray
Color.FromArgb(0, 122, 204)    // Accent blue
```
