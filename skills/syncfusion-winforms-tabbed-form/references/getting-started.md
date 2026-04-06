# Getting Started with Tabbed Form

This guide covers the initial setup and basic usage of Syncfusion Windows Forms Tabbed Form (SfTabbedForm).

## Assembly Deployment

Before using the SfTabbedForm control, you need to add the required assembly reference.

### Required Assembly
- **Syncfusion.Tools.Windows**
- **Syncfusion.Shared.Base**

### NuGet Package Installation

Install via NuGet Package Manager Console:

```powershell
Install-Package Syncfusion.Tools.Windows
```

Or through the NuGet Package Manager UI in Visual Studio:
1. Right-click on your project → Manage NuGet Packages
2. Search for "Syncfusion.Tools.Windows"
3. Install the package

### Manual Assembly Reference

If not using NuGet, add reference to:
- `Syncfusion.Tools.Windows.dll`
- `Syncfusion.Shared.Base.dll`


Refer to the [control dependencies documentation](https://help.syncfusion.com/windowsforms/control-dependencies#sftabbedform) for the complete list of dependencies.

## Converting Standard Form to SfTabbedForm

The default Windows Form can be converted to SfTabbedForm by following these steps:

### Step 1: Create Windows Forms Application

1. Create a new Windows Forms application in Visual Studio
2. Reference the `Syncfusion.Tools.Windows` assembly (as described above)

### Step 2: Import Namespace

Add the required namespace to your form's code file:

**C#:**
```csharp
using Syncfusion.Windows.Forms.Tools;
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms.Tools
```

### Step 3: Change Base Class

Change the base class of your form from `System.Windows.Forms.Form` to `SfTabbedForm`.

**C#:**
```csharp
public partial class Form1 : SfTabbedForm
{
    public Form1()
    {
        InitializeComponent();
    }
}
```

**VB.NET:**
```vb
Partial Public Class Form1
    Inherits SfTabbedForm
    Public Sub New()
        InitializeComponent()
    End Sub
End Class
```

### Important Notes

- The form designer will reflect the change after rebuilding the project
- If using the designer, you'll also need to update the designer code file to inherit from `SfTabbedForm`
- This change enables all tabbed form features on your form

## Loading TabbedFormControl to TabbedForm

The `TabbedFormControl` provides the tabbed user interface to the form. It must be added to the form to enable the tabbed UI.

### Basic Setup

**C#:**
```csharp
public partial class Form1 : SfTabbedForm
{
    public Form1()
    {
        InitializeComponent();
        
        // Create the tabbed form control
        SfTabbedFormControl tabbedFormControl = new SfTabbedFormControl();
        
        // Add to form's controls collection
        this.Controls.Add(tabbedFormControl);
        
        // Assign as the form's TabbedFormControl
        this.TabbedFormControl = tabbedFormControl;
    }
}
```

**VB.NET:**
```vb
Partial Public Class Form1
    Inherits SfTabbedForm
    Public Sub New()
        InitializeComponent()
        
        ' Create the tabbed form control
        Dim tabbedFormControl As New SfTabbedFormControl()
        
        ' Add to form's controls collection
        Me.Controls.Add(tabbedFormControl)
        
        ' Assign as the form's TabbedFormControl
        Me.TabbedFormControl = tabbedFormControl
    End Sub
End Class
```

## Adding Tabs to TabbedForm

To add tabs to the form, create instances of `TabPageAdv` and add them to the tabs collection.

### Creating and Adding Tabs

**C#:**
```csharp
public partial class Form1 : SfTabbedForm
{
    public Form1()
    {
        InitializeComponent();
        
        // Create tabs
        TabPageAdv tabPageAdv1 = new TabPageAdv();
        TabPageAdv tabPageAdv2 = new TabPageAdv();
        
        // Create tabbed form control
        SfTabbedFormControl tabbedFormControl = new SfTabbedFormControl();
        
        // Set tab text
        tabPageAdv1.Text = "Document1";
        tabPageAdv2.Text = "Document2";
        
        // Add tabs to the control
        tabbedFormControl.Tabs.Add(tabPageAdv1);
        tabbedFormControl.Tabs.Add(tabPageAdv2);
        
        // Add control to form
        this.Controls.Add(tabbedFormControl);
        this.TabbedFormControl = tabbedFormControl;
    }
}
```

**VB.NET:**
```vb
Partial Public Class Form1
    Inherits SfTabbedForm
    Public Sub New()
        InitializeComponent()
        
        ' Create tabs
        Dim tabPageAdv1 As New TabPageAdv()
        Dim tabPageAdv2 As New TabPageAdv()
        
        ' Create tabbed form control
        Dim tabbedFormControl As New SfTabbedFormControl()
        
        ' Set tab text
        tabPageAdv1.Text = "Document1"
        tabPageAdv2.Text = "Document2"
        
        ' Add tabs to the control
        tabbedFormControl.Tabs.Add(tabPageAdv1)
        tabbedFormControl.Tabs.Add(tabPageAdv2)
        
        ' Add control to form
        Me.Controls.Add(tabbedFormControl)
        Me.TabbedFormControl = tabbedFormControl
    End Sub
End Class
```

### Adding Controls to Tabs

Each tab is a container that can hold other controls:

**C#:**
```csharp
// Add a button to the first tab
Button button1 = new Button();
button1.Text = "Click Me";
button1.Location = new Point(10, 10);
tabPageAdv1.Controls.Add(button1);

// Add a label to the second tab
Label label1 = new Label();
label1.Text = "This is Document 2";
label1.Location = new Point(10, 10);
tabPageAdv2.Controls.Add(label1);
```

**Result:**  
The form will display with tabs extended into the title bar, showing "Document1" and "Document2" tabs.

## Show Tabs Below the Title Bar

By default, tabs are extended into the title bar. To display tabs below the title bar, disable the `ExtendTabsToTitleBar` property.

### Configuration

**C#:**
```csharp
public partial class Form1 : SfTabbedForm
{
    public Form1()
    {
        InitializeComponent();
        
        // Disable extending tabs to title bar
        this.ExtendTabsToTitleBar = false;
        
        // Setup tabbed control as usual
        SfTabbedFormControl tabbedFormControl = new SfTabbedFormControl();
        // ... add tabs
        this.Controls.Add(tabbedFormControl);
        this.TabbedFormControl = tabbedFormControl;
    }
}
```

**VB.NET:**
```vb
Partial Public Class Form1
    Inherits SfTabbedForm
    Public Sub New()
        InitializeComponent()
        
        ' Disable extending tabs to title bar
        Me.ExtendTabsToTitleBar = False
        
        ' Setup tabbed control as usual
        Dim tabbedFormControl As New SfTabbedFormControl()
        ' ... add tabs
        Me.Controls.Add(tabbedFormControl)
        Me.TabbedFormControl = tabbedFormControl
    End Sub
End Class
```

### Visual Difference

- **ExtendTabsToTitleBar = true** (default): Tabs appear integrated with the window title bar
- **ExtendTabsToTitleBar = false**: Tabs appear below the title bar as a separate row

## Complete Working Example

Here's a complete, ready-to-run example:

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace TabbedFormDemo
{
    public partial class Form1 : SfTabbedForm
    {
        private SfTabbedFormControl tabbedFormControl;
        
        public Form1()
        {
            InitializeComponent();
            InitializeTabbedForm();
        }
        
        private void InitializeTabbedForm()
        {
            // Create tabbed form control
            tabbedFormControl = new SfTabbedFormControl();
            
            // Create and configure tabs
            for (int i = 1; i <= 3; i++)
            {
                TabPageAdv tab = new TabPageAdv();
                tab.Text = $"Document {i}";
                
                // Add some content to the tab
                Label label = new Label();
                label.Text = $"Content for Document {i}";
                label.AutoSize = true;
                label.Location = new Point(20, 20);
                tab.Controls.Add(label);
                
                tabbedFormControl.Tabs.Add(tab);
            }
            
            // Add control to form
            this.Controls.Add(tabbedFormControl);
            this.TabbedFormControl = tabbedFormControl;
            
            // Optional: Configure appearance
            this.Text = "Tabbed Form Demo";
            this.Size = new Size(800, 600);
        }
    }
}
```

## Next Steps

Now that you have the basic setup complete, explore:

- **Tab Selection**: Control which tab is active programmatically
- **Tab Navigation**: Add navigation buttons for easier tab access
- **Drag and Drop**: Enable users to reorder tabs
- **Context Menu**: Add right-click functionality to tabs
- **Customization**: Style tabs and configure appearance

## Common Issues

### Issue: Tabs Not Appearing

**Solution**: Ensure you've:
1. Changed the form base class to `SfTabbedForm`
2. Created a `SfTabbedFormControl` instance
3. Added it to the form's Controls collection
4. Assigned it to `this.TabbedFormControl`

### Issue: Designer Shows Errors

**Solution**: 
1. Rebuild the project after changing the base class
2. Update both the code file and designer file
3. Close and reopen the designer

### Issue: Assembly Not Found

**Solution**: Verify the Syncfusion.Tools.Windows assembly is referenced and the version matches your Syncfusion installation.
