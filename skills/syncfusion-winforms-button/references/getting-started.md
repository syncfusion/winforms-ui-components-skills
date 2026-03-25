# Getting Started with SfButton

This guide walks you through the initial setup and basic usage of the Syncfusion Windows Forms SfButton control.

## Assembly Dependencies

### Required Assemblies

To use SfButton in your Windows Forms application, you need the following assemblies:

- **Syncfusion.Core.WinForms** - Main SfButton control and core functionality
- **Syncfusion.Shared.Base** - Shared base library

### NuGet Installation

The easiest way to add SfButton is through NuGet Package Manager:

```
Install-Package Syncfusion.Core.WinForms
```

Or search for "Syncfusion.Core.WinForms" in NuGet Package Manager UI.

### Reference Assemblies in Visual Studio

If using manual assembly references:

1. In Solution Explorer, right-click **References**
2. Select **Add Reference**
3. Browse to your Syncfusion installation directory
4. Select `Syncfusion.Core.WinForms.dll` and `Syncfusion.Shared.Base.dll`
5. Click **Add**

## Adding SfButton to Your Form

### Method 1: Using Designer

The easiest method for new buttons:

1. Open your Windows Forms Designer
2. Open the **Toolbox**
3. Search for "SfButton" in the controls list
4. Drag SfButton onto your form
5. Resize and position as needed
6. Properties panel appears automatically for customization

### Method 2: Programmatically in Code

For dynamic button creation or more control:

```csharp
using Syncfusion.WinForms.Buttons;
using System.Drawing;
using System.Windows.Forms;

public partial class Form1 : Form
{
    public Form1()
    {
        InitializeComponent();
    }

    private void Form1_Load(object sender, EventArgs e)
    {
        // Create a new SfButton instance
        SfButton sfButton1 = new SfButton();

        // Configure button properties
        sfButton1.Font = new Font("Segoe UI Semibold", 9F);
        sfButton1.Location = new Point(60, 62);
        sfButton1.Name = "sfButton1";
        sfButton1.Size = new Size(96, 28);
        sfButton1.Text = "Click Me";

        // Add the button to the form
        this.Controls.Add(sfButton1);
    }
}
```

### Key Initialization Properties

When creating SfButton programmatically, set these common properties:

| Property | Type | Example | Description |
|----------|------|---------|-------------|
| `Text` | string | `"Submit"` | Button display text |
| `Location` | Point | `new Point(60, 62)` | Position on form |
| `Size` | Size | `new Size(96, 28)` | Width and height |
| `Font` | Font | `new Font("Segoe UI", 9F)` | Text font |
| `BackColor` | Color | `Color.LightBlue` | Background color |
| `ForeColor` | Color | `Color.White` | Text color |

## Handling Button Click Events

### Event Handler in Designer

1. Select the SfButton in Designer
2. In Properties panel, click **Events** (lightning bolt icon)
3. Double-click the **Click** event
4. Visual Studio creates the event handler
5. Add your code in the generated method:

```csharp
private void sfButton1_Click(object sender, EventArgs e)
{
    MessageBox.Show("Button was clicked!");
}
```

### Programmatic Event Attachment

Add click event handler in code:

```csharp
// Attach the click event
sfButton1.Click += SfButton1_Click;

// Define the event handler
private void SfButton1_Click(object sender, EventArgs e)
{
    MessageBox.Show("SfButton was clicked");
    // Your button logic here
}
```

### Lambda Expression (Modern C# Style)

```csharp
sfButton1.Click += (sender, e) => 
{
    MessageBox.Show("Button clicked!");
};
```

## Complete Getting Started Example

Here's a complete minimal Windows Forms application with SfButton:

```csharp
using Syncfusion.WinForms.Buttons;
using System;
using System.Drawing;
using System.Windows.Forms;

public partial class Form1 : Form
{
    private SfButton sfButton1;

    public Form1()
    {
        InitializeComponent();
        
        // Configure form
        this.Text = "SfButton Getting Started";
        this.Size = new Size(400, 300);
        this.StartPosition = FormStartPosition.CenterScreen;
        
        // Create and configure button
        sfButton1 = new SfButton();
        sfButton1.Text = "Click Me";
        sfButton1.Font = new Font("Segoe UI", 10F);
        sfButton1.Location = new Point(150, 130);
        sfButton1.Size = new Size(100, 40);
        sfButton1.BackColor = Color.LightBlue;
        
        // Add click event
        sfButton1.Click += SfButton1_Click;
        
        // Add to form
        this.Controls.Add(sfButton1);
    }

    private void SfButton1_Click(object sender, EventArgs e)
    {
        MessageBox.Show("Button clicked successfully!", "SfButton Demo");
    }

    [STAThread]
    static void Main()
    {
        Application.EnableVisualStyles();
        Application.SetCompatibleTextRenderingDefault(false);
        Application.Run(new Form1());
    }
}
```

## Common Setup Issues

### Issue: SfButton Not Appearing in Toolbox

**Solution:**
- Ensure Syncfusion NuGet package is installed
- Rebuild the solution (Build > Rebuild Solution)
- Close and reopen Visual Studio
- Check that correct target framework is used (.NET Framework 4.x or higher)

### Issue: Namespace Not Found

**Solution:**
Add using statement at top of file:

```csharp
using Syncfusion.WinForms.Buttons;
```

### Issue: Assembly Not Found at Runtime

**Solution:**
- Verify NuGet package installation
- Check References in project properties
- Ensure assemblies are in the bin output folder
- Rebuild entire solution

## Next Steps

After basic setup:

1. ✅ Add images and icons to buttons (see Button Types & Content)
2. ✅ Customize appearance for different button states (see Appearance & Styling)
3. ✅ Apply themes for professional look (see Themes & Customization)
4. ✅ Add tooltips and form interactions (see Events & Interactions)

## Important Notes

- Always include `using Syncfusion.WinForms.Buttons;` for SfButton
- Set `Size` property unless using `AutoSize = true`
- Position buttons relative to form size for responsive design
- Test click events with MessageBox before implementing actual logic
- Use Designer for visual layout, code for dynamic creation
