# Getting Started with WizardControl

This guide covers the installation, setup, and basic usage of the WizardControl in Windows Forms applications.

## When to Read This

Read this reference when:
- Setting up WizardControl for the first time
- Understanding assembly requirements and dependencies
- Adding the control via designer or programmatically
- Creating a basic multi-page wizard
- Configuring WizardContainer and WizardControlPages
- Troubleshooting setup and configuration issues

## Assembly Requirements

The WizardControl requires the following assemblies:

**Required Assemblies:**
- `Syncfusion.Tools.Windows.dll` - Contains the WizardControl
- `Syncfusion.Tools.Base.dll` - Base functionality for Tools controls
- `Syncfusion.Shared.Windows.dll` - Shared Windows Forms components
- `Syncfusion.Shared.Base.dll` - Base shared functionality
- `Syncfusion.Grid.Windows.dll` - Grid support components
- `Syncfusion.Grid.Base.dll` - Grid base functionality

**Namespace:**
```csharp
using Syncfusion.Windows.Forms.Tools;
```

```vbnet
Imports Syncfusion.Windows.Forms.Tools
```

## Installation Methods

### NuGet Installation

Install the WizardControl via NuGet Package Manager:

**Package Manager Console:**
```powershell
Install-Package Syncfusion.Tools.Windows
```

**NuGet Package Manager UI:**
1. Right-click your project → Manage NuGet Packages
2. Search for "Syncfusion.Tools.Windows"
3. Select the package and click Install
4. Accept license agreements

### Manual Assembly Reference

If not using NuGet, add assembly references manually:

1. Right-click project → Add Reference → Browse
2. Navigate to Syncfusion installation folder:
   - `C:/Program Files (x86)/Syncfusion/Essential Studio/Windows/{version}/precompiledassemblies/{.NET version}/`
3. Select all 6 required assemblies listed above
4. Click OK

For more details, see [Control Dependencies](https://help.syncfusion.com/windowsforms/control-dependencies#wizardcontrol).

## Designer-Based Setup

The WizardControl provides full Windows Forms designer support with smart tags and collection editors.

### Adding via Toolbox

**Steps:**
1. Open your form in designer view
2. Locate the Syncfusion toolbox section
3. Find "WizardControl" control
4. Drag and drop onto your form
5. The control will dock to fill the form automatically

**What Happens:**
- All 6 required assemblies are automatically referenced
- A WizardControl instance is added to the form
- The control appears in designer with default appearance

**Visual Result:**
The control displays with a default banner panel and navigation buttons at the bottom.

### Designer Properties

Set these properties in the Property Grid:

| Property | Purpose | Default |
|----------|---------|---------|
| `Dock` | Fill entire form or custom docking | `Fill` |
| `Style` | Visual theme (Default, Metro, Office) | `Default` |
| `Size` | Control dimensions (if not docked) | Form size |

## Programmatic Creation

### Basic Implementation

Create and configure a WizardControl in code:

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class Form1 : Form
{
    private WizardControl wizardControl1;
    
    public Form1()
    {
        InitializeComponent();
        CreateWizardControl();
    }
    
    private void CreateWizardControl()
    {
        // Create instance
        this.wizardControl1 = new WizardControl();
        
        // Set docking to fill form
        this.wizardControl1.Dock = DockStyle.Fill;
        
        // Set visual style
        this.wizardControl1.Style = Syncfusion.Windows.Forms.Tools.Wizard.Style.Metro;
        
        // Add to form
        this.Controls.Add(this.wizardControl1);
    }
}
```

**VB.NET:**
```vbnet
Imports System
Imports System.Drawing
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Public Partial Class Form1
    Inherits Form
    
    Private wizardControl1 As WizardControl
    
    Public Sub New()
        InitializeComponent()
        CreateWizardControl()
    End Sub
    
    Private Sub CreateWizardControl()
        ' Create instance
        Me.wizardControl1 = New WizardControl()
        
        ' Set docking to fill form
        Me.wizardControl1.Dock = DockStyle.Fill
        
        ' Set visual style
        Me.wizardControl1.Style = Syncfusion.Windows.Forms.Tools.Wizard.Style.Metro
        
        ' Add to form
        Me.Controls.Add(Me.wizardControl1)
    End Sub
End Class
```

### Adding Wizard Pages

To create a functional wizard, add WizardControlPage instances to the WizardPages collection:

**C#:**
```csharp
private void CreateWizardPages()
{
    // Create page instances
    WizardControlPage page1 = new WizardControlPage();
    WizardControlPage page2 = new WizardControlPage();
    WizardControlPage page3 = new WizardControlPage();
    
    // Configure first page
    page1.Title = "Welcome";
    page1.Description = "First page of the wizard";
    page1.BackVisible = false;  // Hide Back button on first page
    
    // Configure second page
    page2.Title = "Configuration";
    page2.Description = "Enter your configuration settings";
    
    // Configure third page (final)
    page3.Title = "Finished";
    page3.Description = "Setup is complete";
    page3.NextVisible = false;    // Hide Next button on last page
    page3.CancelVisible = false;  // Hide Cancel button on last page
    page3.FinishVisible = true;   // Show Finish button
    
    // Add pages to wizard
    this.wizardControl1.WizardPages = new WizardControlPage[]
    {
        page1,
        page2,
        page3
    };
}
```

**VB.NET:**
```vbnet
Private Sub CreateWizardPages()
    ' Create page instances
    Dim page1 As New WizardControlPage()
    Dim page2 As New WizardControlPage()
    Dim page3 As New WizardControlPage()
    
    ' Configure first page
    page1.Title = "Welcome"
    page1.Description = "First page of the wizard"
    page1.BackVisible = False
    
    ' Configure second page
    page2.Title = "Configuration"
    page2.Description = "Enter your configuration settings"
    
    ' Configure third page (final)
    page3.Title = "Finished"
    page3.Description = "Setup is complete"
    page3.NextVisible = False
    page3.CancelVisible = False
    page3.FinishVisible = True
    
    ' Add pages to wizard
    Me.wizardControl1.WizardPages = New WizardControlPage() {
        page1,
        page2,
        page3
    }
End Sub
```

### Complete Example: Software Installation Wizard

Here's a complete example creating a basic installation wizard:

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class InstallWizard : Form
{
    private WizardControl wizardControl1;
    private WizardControlPage welcomePage;
    private WizardControlPage licensePage;
    private WizardControlPage destinationPage;
    private WizardControlPage installPage;
    private WizardControlPage completePage;
    
    public InstallWizard()
    {
        InitializeComponent();
        SetupWizard();
    }
    
    private void SetupWizard()
    {
        // Create wizard control
        wizardControl1 = new WizardControl
        {
            Dock = DockStyle.Fill,
            Style = Syncfusion.Windows.Forms.Tools.Wizard.Style.Metro
        };
        
        // Welcome page
        welcomePage = new WizardControlPage
        {
            Title = "Welcome to Setup Wizard",
            Description = "This wizard will guide you through the installation process",
            BackVisible = false
        };
        
        Label lblWelcome = new Label
        {
            Text = "Welcome to MyApp Setup\n\nClick Next to continue.",
            Location = new Point(20, 20),
            AutoSize = true,
            Font = new Font("Arial", 10)
        };
        welcomePage.Controls.Add(lblWelcome);
        
        // License page
        licensePage = new WizardControlPage
        {
            Title = "License Agreement",
            Description = "Please read and accept the license terms to continue"
        };
        
        RichTextBox txtLicense = new RichTextBox
        {
            Text = "END USER LICENSE AGREEMENT\n\n" +
                   "Please read this agreement carefully...",
            Location = new Point(20, 20),
            Size = new Size(500, 200),
            ReadOnly = true
        };
        
        CheckBox chkAccept = new CheckBox
        {
            Text = "I accept the license agreement",
            Location = new Point(20, 230),
            AutoSize = true
        };
        chkAccept.CheckedChanged += (s, e) =>
        {
            licensePage.NextEnabled = chkAccept.Checked;
        };
        
        licensePage.Controls.Add(txtLicense);
        licensePage.Controls.Add(chkAccept);
        licensePage.NextEnabled = false;  // Disabled until accepted
        
        // Destination page
        destinationPage = new WizardControlPage
        {
            Title = "Installation Location",
            Description = "Choose where to install the application"
        };
        
        Label lblPath = new Label
        {
            Text = "Install to:",
            Location = new Point(20, 20),
            AutoSize = true
        };
        
        TextBox txtPath = new TextBox
        {
            Text = @"C:\Program Files\MyApp",
            Location = new Point(20, 45),
            Size = new Size(400, 20)
        };
        
        Button btnBrowse = new Button
        {
            Text = "Browse...",
            Location = new Point(430, 43),
            Size = new Size(80, 25)
        };
        
        destinationPage.Controls.Add(lblPath);
        destinationPage.Controls.Add(txtPath);
        destinationPage.Controls.Add(btnBrowse);
        
        // Install page
        installPage = new WizardControlPage
        {
            Title = "Installing",
            Description = "Please wait while files are being copied",
            BackEnabled = false,
            NextEnabled = false,
            CancelEnabled = false
        };
        
        ProgressBar progressBar = new ProgressBar
        {
            Location = new Point(20, 60),
            Size = new Size(500, 25),
            Style = ProgressBarStyle.Marquee
        };
        
        Label lblStatus = new Label
        {
            Text = "Installing files...",
            Location = new Point(20, 20),
            AutoSize = true
        };
        
        installPage.Controls.Add(lblStatus);
        installPage.Controls.Add(progressBar);
        
        // Complete page
        completePage = new WizardControlPage
        {
            Title = "Installation Complete",
            Description = "Setup has successfully installed MyApp",
            NextVisible = false,
            CancelVisible = false,
            FinishVisible = true
        };
        
        Label lblComplete = new Label
        {
            Text = "Installation completed successfully!\n\n" +
                   "Click Finish to exit the wizard.",
            Location = new Point(20, 20),
            AutoSize = true,
            Font = new Font("Arial", 10)
        };
        completePage.Controls.Add(lblComplete);
        
        // Add all pages
        wizardControl1.WizardPages = new WizardControlPage[]
        {
            welcomePage,
            licensePage,
            destinationPage,
            installPage,
            completePage
        };
        
        // Add wizard to form
        this.Controls.Add(wizardControl1);
        this.Text = "MyApp Setup";
        this.Size = new Size(700, 500);
        this.StartPosition = FormStartPosition.CenterScreen;
    }
}
```

## Configuring Banner Panel

The wizard displays a banner at the top with title, description, and optional image:

**C#:**
```csharp
private void ConfigureBanner()
{
    // Create banner image
    PictureBox pictureBox = new PictureBox
    {
        Image = Image.FromFile("banner.png"),
        SizeMode = PictureBoxSizeMode.StretchImage,
        Size = new Size(100, 60)
    };
    
    // Set banner
    wizardControl1.Banner = pictureBox;
    
    // Enable auto-layout for banner elements
    wizardControl1.AutoLayoutBanner = true;
    wizardControl1.AutoLayoutTitle = true;
    wizardControl1.AutoLayoutDescription = true;
}
```

**VB.NET:**
```vbnet
Private Sub ConfigureBanner()
    ' Create banner image
    Dim pictureBox As New PictureBox With {
        .Image = Image.FromFile("banner.png"),
        .SizeMode = PictureBoxSizeMode.StretchImage,
        .Size = New Size(100, 60)
    }
    
    ' Set banner
    wizardControl1.Banner = pictureBox
    
    ' Enable auto-layout for banner elements
    wizardControl1.AutoLayoutBanner = True
    wizardControl1.AutoLayoutTitle = True
    wizardControl1.AutoLayoutDescription = True
End Sub
```

## Next Steps

After setting up the basic wizard:

1. **Configure Wizard Pages** → Read: [wizard-pages.md](wizard-pages.md)
   - Add and manage multiple pages
   - Set page titles and descriptions
   - Reorder pages and control flow

2. **Customize Navigation Buttons** → Read: [navigation-buttons.md](navigation-buttons.md)
   - Control button visibility per page
   - Add custom buttons
   - Style navigation buttons

3. **Implement Validation** → Read: [page-validation-events.md](page-validation-events.md)
   - Validate page data before navigation
   - Handle wizard events
   - Create custom page sequences

## Troubleshooting

### Control Not Visible in Toolbox

**Issue:** WizardControl doesn't appear in Visual Studio toolbox.

**Solutions:**
1. Verify Syncfusion.Tools.Windows is installed via NuGet
2. Check if assemblies are compatible with your .NET version
3. Right-click toolbox → Choose Items → Browse to assembly location
4. Restart Visual Studio after installation
5. Ensure the correct .NET Framework version is targeted

### Designer Shows Errors

**Issue:** Designer displays errors or fails to load.

**Solutions:**
1. Rebuild the project to refresh designer
2. Check that all 6 required assemblies are referenced
3. Verify assembly versions match across all Syncfusion references
4. Close and reopen the designer
5. Check for conflicting assembly versions in bin folder

### Namespace Not Found

**Issue:** `The type or namespace name 'Tools' does not exist in the namespace 'Syncfusion.Windows.Forms'`

**Solution:**
1. Add reference to `Syncfusion.Tools.Windows.dll`
2. Verify `using Syncfusion.Windows.Forms.Tools;` is present
3. Check that assembly version matches your Syncfusion license
4. Ensure all 6 required assemblies are referenced
5. Clean and rebuild solution

### Pages Not Displaying

**Issue:** Wizard control shows but pages don't display.

**Solution:**
1. Verify WizardPages array is set:
   ```csharp
   wizardControl1.WizardPages = new WizardControlPage[] { page1, page2 };
   ```
2. Check that at least one page exists in collection
3. Ensure pages have valid Title and Description properties
4. Verify control's Dock or Size property is set appropriately
5. Check that form's Controls.Add(wizardControl1) was called

### Navigation Buttons Missing

**Issue:** Back, Next, or other buttons don't appear.

**Solution:**
1. Check page-specific button visibility properties:
   ```csharp
   page.BackVisible = true;
   page.NextVisible = true;
   ```
2. Verify control has sufficient height to display buttons
3. Ensure no custom layout is hiding button area
4. Check GridBagLayout constraints if customized

### Build Errors About Missing Assemblies

**Issue:** Build fails with "Could not load file or assembly" errors.

**Solution:**
1. Verify all 6 required assemblies are referenced
2. Check that assembly versions match across all Syncfusion references
3. Ensure assemblies are set to "Copy Local = True"
4. Clean and rebuild solution
5. Use NuGet for automatic dependency management
6. Verify .NET Framework version compatibility

### .NET Core Collection Editor Title Issue

**Issue:** In .NET Core, collection editor shows "ControlProxy`1 CollectionEditor" instead of "WizardControlPage CollectionEditor".

**Note:** This is a known .NET Core issue ([GitHub #14049](https://github.com/dotnet/winforms/issues/14049)) and does not affect functionality. The collection editor works normally despite the title display.
