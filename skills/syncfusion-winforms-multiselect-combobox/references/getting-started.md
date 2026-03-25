# Getting Started with MultiSelectionComboBox

This reference covers assembly setup, adding the control to a form, and a minimal working configuration.

## Assembly References

Add the following assemblies (or install the NuGet package):

**NuGet:**
```powershell
Install-Package Syncfusion.Tools.Windows
```

**Manual references required:**
- `Syncfusion.Tools.Base`
- `Syncfusion.Tools.Windows`
- `Syncfusion.Shared.Base`
- `Syncfusion.Shared.Windows`

For the complete dependency list see the [control dependencies](https://help.syncfusion.com/windowsforms/control-dependencies#multiselectioncombobox) page.

## Namespace Imports

```csharp
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Windows.Forms;
```

```vb
Imports Syncfusion.Windows.Forms.Tools
Imports Syncfusion.Windows.Forms
```

## Adding via Designer

1. Create a new **Windows Forms Application** project in Visual Studio.
2. Open the **Toolbox**, locate `MultiSelectionComboBox` under the Syncfusion group.
3. Drag and drop the control onto the form. Assembly references are added automatically.
4. Use the **Smart Tag** or **Properties** window to configure common settings.

## Adding via Code-Behind

Create an instance, configure it, and add it to the form's `Controls` collection:

```csharp
MultiSelectionComboBox combo = new MultiSelectionComboBox();
combo.ButtonStyle = ButtonAppearance.Metro;
combo.Size = new System.Drawing.Size(217, 30);
combo.UseVisualStyle = true;
this.Controls.Add(combo);
```

```vb
Dim combo As MultiSelectionComboBox = New MultiSelectionComboBox()
combo.ButtonStyle = ButtonAppearance.Metro
combo.Size = New System.Drawing.Size(217, 30)
combo.UseVisualStyle = True
Me.Controls.Add(combo)
```

## Minimal Working Example

The snippet below creates a MultiSelectionComboBox in VisualItem mode with a short list of static items:

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Windows.Forms;

public class MainForm : Form
{
    public MainForm()
    {
        // Register license (replace with your key)
        Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");

        var combo = new MultiSelectionComboBox
        {
            ButtonStyle = ButtonAppearance.Metro,
            UseVisualStyle = true,
            Size = new System.Drawing.Size(300, 30),
            Location = new System.Drawing.Point(20, 20),
            DisplayMode = DisplayMode.VisualItem
        };

        combo.Items.AddRange(new object[] { "Apple", "Banana", "Cherry", "Date" });
        this.Controls.Add(combo);
    }
}
```

## Key Initialization Properties

| Property | Type | Purpose |
|---|---|---|
| `ButtonStyle` | `ButtonAppearance` | Appearance of the dropdown button (Metro, Office2007, etc.) |
| `UseVisualStyle` | `bool` | Enables the active Office visual style |
| `Size` | `Size` | Sets the control dimensions |
| `DisplayMode` | `DisplayMode` | How selected items appear (VisualItem, DelimiterMode, NormalMode) |

## Gotchas

- When `DataSource` is set, you **cannot** modify `Items` directly — use the data source to add/remove entries.
- Always register the Syncfusion license key before any control is created to avoid watermark banners in release builds.
