# Getting Started with WizardControl

This guide covers installation, setup, and basic usage of the WizardControl in Windows Forms applications.

## When to Read This

Read this reference when:
- Setting up WizardControl for the first time
- Understanding assembly requirements and dependencies
- Adding the control via designer or programmatically
- Creating a basic multi-page wizard
- Troubleshooting setup and configuration issues

## Assembly Requirements

**Required Assemblies:**
- `Syncfusion.Tools.Windows.dll`
- `Syncfusion.Tools.Base.dll`
- `Syncfusion.Shared.Windows.dll`
- `Syncfusion.Shared.Base.dll`
- `Syncfusion.Grid.Windows.dll`
- `Syncfusion.Grid.Base.dll`

**Namespace:**
```csharp
using Syncfusion.Windows.Forms.Tools;
```

## NuGet Installation

```powershell
Install-Package Syncfusion.Tools.Windows
```

Or via NuGet Package Manager UI: search for `Syncfusion.Tools.Windows`.

## Designer-Based Setup

1. Open your form in designer view.
2. Locate **WizardControl** in the Syncfusion toolbox section.
3. Drag and drop onto the form — all 6 required assemblies are referenced automatically.
4. The control docks to fill the form with a default banner panel and navigation buttons.

Configure properties in the Property Grid: `Dock`, `Style`, `Size`.

## Programmatic Creation

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
        wizardControl1 = new WizardControl
        {
            Dock = DockStyle.Fill,
            Style = Syncfusion.Windows.Forms.Tools.WizardStyle.Metro
        };
        this.Controls.Add(wizardControl1);
    }
}
```

## Adding Wizard Pages

```csharp
private void CreateWizardPages()
{
    WizardControlPage page1 = new WizardControlPage
    {
        Title = "Welcome",
        Description = "First page of the wizard",
        BackVisible = false
    };

    WizardControlPage page2 = new WizardControlPage
    {
        Title = "Configuration",
        Description = "Enter your configuration settings"
    };

    WizardControlPage page3 = new WizardControlPage
    {
        Title = "Finished",
        Description = "Setup is complete",
        NextVisible = false,
        CancelVisible = false,
        FinishVisible = true
    };

    wizardControl1.WizardPages = new WizardControlPage[] { page1, page2, page3 };
}
```

## Configuring Banner Panel

```csharp
private void ConfigureBanner()
{
    PictureBox pictureBox = new PictureBox
    {
        Image = Image.FromFile("banner.png"),
        SizeMode = PictureBoxSizeMode.StretchImage,
        Size = new Size(100, 60)
    };

    wizardControl1.Banner = pictureBox;
    wizardControl1.AutoLayoutBanner = true;
    wizardControl1.AutoLayoutTitle = true;
    wizardControl1.AutoLayoutDescription = true;
}
```

## Complete Example: Software Installation Wizard

```csharp
public partial class InstallWizard : Form
{
    private WizardControl wizardControl1;

    public InstallWizard()
    {
        InitializeComponent();
        SetupWizard();
    }

    private void SetupWizard()
    {
        wizardControl1 = new WizardControl
        {
            Dock = DockStyle.Fill,
            Style = WizardStyle.Metro
        };

        var welcomePage = new WizardControlPage
        {
            Title = "Welcome to Setup Wizard",
            Description = "This wizard will guide you through the installation process",
            BackVisible = false
        };
        welcomePage.Controls.Add(new Label
        {
            Text = "Welcome to MyApp Setup\n\nClick Next to continue.",
            Location = new Point(20, 20), AutoSize = true,
            Font = new Font("Arial", 10)
        });

        var licensePage = new WizardControlPage
        {
            Title = "License Agreement",
            Description = "Please read and accept the license terms to continue",
            NextEnabled = false
        };
        var chkAccept = new CheckBox
        {
            Text = "I accept the license agreement",
            Location = new Point(20, 230), AutoSize = true
        };
        chkAccept.CheckedChanged += (s, e) => licensePage.NextEnabled = chkAccept.Checked;
        licensePage.Controls.Add(new RichTextBox
        {
            Text = "END USER LICENSE AGREEMENT\n\nPlease read this agreement carefully...",
            Location = new Point(20, 20), Size = new Size(500, 200), ReadOnly = true
        });
        licensePage.Controls.Add(chkAccept);

        var completePage = new WizardControlPage
        {
            Title = "Installation Complete",
            Description = "Setup has successfully installed MyApp",
            NextVisible = false, CancelVisible = false, FinishVisible = true
        };
        completePage.Controls.Add(new Label
        {
            Text = "Installation completed successfully!\n\nClick Finish to exit.",
            Location = new Point(20, 20), AutoSize = true
        });

        wizardControl1.WizardPages = new WizardControlPage[]
        {
            welcomePage, licensePage, completePage
        };

        this.Controls.Add(wizardControl1);
        this.Text = "MyApp Setup";
        this.Size = new Size(700, 500);
        this.StartPosition = FormStartPosition.CenterScreen;
    }
}
```

## Best Practices

- Set `Dock = DockStyle.Fill` to let the wizard fill the form.
- Configure first page with `BackVisible = false`.
- Configure last page with `NextVisible = false` and `FinishVisible = true`.
- Use NuGet for automatic dependency management.

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Control not in Toolbox | Install NuGet or right-click Toolbox > Choose Items > browse to `Syncfusion.Tools.Windows.dll` |
| Namespace not found | Add reference to `Syncfusion.Tools.Windows.dll`; add `using Syncfusion.Windows.Forms.Tools;` |
| Designer errors | Rebuild project; verify all 6 assemblies are referenced and versions match |
| Pages not displaying | Verify `WizardPages` array is set; check `Controls.Add(wizardControl1)` was called |
| Navigation buttons missing | Check per-page `BackVisible`/`NextVisible` properties; ensure control has sufficient height |
| Build errors (missing assemblies) | Verify all 6 assemblies referenced; set "Copy Local = True"; use NuGet |
| .NET Core collection editor title | Known issue (GitHub #14049) — does not affect functionality |

## Next Steps

- [wizard-pages.md](wizard-pages.md) — creating and managing pages
- [navigation-buttons.md](navigation-buttons.md) — button visibility and customization
- [page-validation-events.md](page-validation-events.md) — validation and events
- [banner-configuration.md](banner-configuration.md) — banner customization
- [appearance-customization.md](appearance-customization.md) — styling and themes