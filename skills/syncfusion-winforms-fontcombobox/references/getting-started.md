# Getting Started with FontComboBox

Complete guide to setting up and initializing the Syncfusion WinForms FontComboBox control in your Windows Forms applications.

## Assembly Deployment

### Required Dependencies

The FontComboBox control requires specific Syncfusion assemblies to function properly.

**Required Assemblies:**
- `Syncfusion.Shared.Base.dll` - Core shared components
- `Syncfusion.Tools.Windows.dll` - Tools package containing FontComboBox

### NuGet Package Installation

Install the required package using NuGet Package Manager:

**Package Manager Console:**
```powershell
Install-Package Syncfusion.Tools.WinForms
```

**NuGet Package Manager UI:**
1. Right-click project → Manage NuGet Packages
2. Search for "Syncfusion.Tools.WinForms"
3. Click Install

**Package Reference (csproj):**
```xml
<PackageReference Include="Syncfusion.Tools.WinForms" Version="25.1.35" />
```

### Manual Assembly References

If not using NuGet, add manual references:

1. Right-click References → Add Reference
2. Browse to Syncfusion installation folder:
   - `C:\Program Files (x86)\Syncfusion\Essential Studio\<version>\precompiledassemblies\<.NET version>\`
3. Select required assemblies listed above

**More Information:**
- [How to Install NuGet Packages](https://help.syncfusion.com/windowsforms/installation/install-nuget-packages)
- [Control Dependencies](https://help.syncfusion.com/windowsforms/control-dependencies#fontcombobox)

---

## Creating Application with FontComboBox

### Prerequisites

- Visual Studio 2015 or later
- .NET Framework 4.0 or later
- Syncfusion WinForms components installed

### Creating the Project

1. Open Visual Studio
2. File → New → Project
3. Select "Windows Forms App (.NET Framework)"
4. Name your project (e.g., "FontComboBoxDemo")
5. Click Create

---

## Adding FontComboBox via Designer

The easiest way to add FontComboBox is through the Visual Studio designer.

### Steps

1. **Open Form Designer**: Double-click Form1.cs in Solution Explorer
2. **Open Toolbox**: View → Toolbox (or Ctrl+Alt+X)
3. **Locate Control**: Expand "Syncfusion Controls" section
4. **Drag and Drop**: Find "FontComboBox" and drag it onto the form

![FontComboBox in Toolbox](images/fontcombobox-toolbox.png)

**Benefits:**
- Automatic assembly references added
- Designer code generated automatically
- Visual positioning and sizing
- Property Grid access for configuration

### Designer-Generated Code

When you drag and drop, Visual Studio generates initialization code:

```csharp
// In Form1.Designer.cs
private Syncfusion.Windows.Forms.Tools.FontComboBox fontComboBox1;

private void InitializeComponent()
{
    this.fontComboBox1 = new Syncfusion.Windows.Forms.Tools.FontComboBox();
    this.SuspendLayout();
    
    // fontComboBox1
    this.fontComboBox1.Location = new System.Drawing.Point(12, 12);
    this.fontComboBox1.Name = "fontComboBox1";
    this.fontComboBox1.Size = new System.Drawing.Size(200, 21);
    this.fontComboBox1.TabIndex = 0;
    
    // Form1
    this.Controls.Add(this.fontComboBox1);
    this.ResumeLayout(false);
}
```

---

## Adding FontComboBox via Code

For programmatic control creation, add FontComboBox manually in code.

### Step 1: Add Using Directive

```csharp
using Syncfusion.Windows.Forms.Tools;
using System.Drawing;
```

### Step 2: Create and Configure Control

**C# Example:**
```csharp
public partial class Form1 : Form
{
    private FontComboBox fontComboBox;
    
    public Form1()
    {
        InitializeComponent();
        InitializeFontComboBox();
    }
    
    private void InitializeFontComboBox()
    {
        // Create FontComboBox instance
        fontComboBox = new FontComboBox();
        
        // Set size and position
        fontComboBox.Size = new Size(200, 25);
        fontComboBox.Location = new Point(20, 20);
        
        // Set name for reference
        fontComboBox.Name = "fontComboBox";
        fontComboBox.TabIndex = 0;
        
        // Add to form's controls collection
        this.Controls.Add(fontComboBox);
    }
}
```

**VB.NET Example:**
```vb
Imports Syncfusion.Windows.Forms.Tools
Imports System.Drawing

Public Class Form1
    Private fontComboBox As FontComboBox
    
    Public Sub New()
        InitializeComponent()
        InitializeFontComboBox()
    End Sub
    
    Private Sub InitializeFontComboBox()
        ' Create FontComboBox instance
        fontComboBox = New FontComboBox()
        
        ' Set size and position
        fontComboBox.Size = New Size(200, 25)
        fontComboBox.Location = New Point(20, 20)
        
        ' Set name for reference
        fontComboBox.Name = "fontComboBox"
        fontComboBox.TabIndex = 0
        
        ' Add to form's controls collection
        Me.Controls.Add(fontComboBox)
    End Sub
End Class
```

### Default Initialization Result

When initialized without additional configuration, FontComboBox displays an empty combo box. To populate it with fonts, enable AutoComplete or manually add items.

![Default FontComboBox](images/fontcombobox-default.png)

---

## Loading System Fonts

The FontComboBox control automatically loads system fonts when AutoComplete is enabled.

### Enable AutoComplete to Load Fonts

**C# Example:**
```csharp
fontComboBox.UseAutoComplete = true;
```

**VB.NET Example:**
```vb
fontComboBox.UseAutoComplete = True
```

This populates the dropdown with all system-installed fonts, displaying each font name in its respective typeface.

![FontComboBox with System Fonts](images/fontcombobox-loaded.png)

---

## Setting Default Selection

Select a default font programmatically using SelectedItem or SelectedIndex.

### Using SelectedItem (by Font Name)

**C# Example:**
```csharp
// Select Arial as default font
fontComboBox.Text = "Arial";

// Alternative: Segoe UI
fontComboBox.Text = "Segoe UI";
```

**VB.NET Example:**
```vb
' Select Arial as default font
fontComboBox.Text = "Arial"

' Alternative: Segoe UI
fontComboBox.Text = "Segoe UI"
```

### Using SelectedIndex (by Position)

**C# Example:**
```csharp
// Select first font in list
fontComboBox.SelectedIndex = 0;

// Select second font
fontComboBox.SelectedIndex = 1;
```

**VB.NET Example:**
```vb
' Select first font in list
fontComboBox.SelectedIndex = 0

' Select second font
fontComboBox.SelectedIndex = 1
```

![Selected Font](images/fontcombobox-selected.png)

---

## RTL (Right-to-Left) Support

FontComboBox supports RTL layout for right-to-left languages (Arabic, Hebrew, etc.).

### Enable RTL Layout

**C# Example:**
```csharp
fontComboBox.RightToLeft = RightToLeft.Yes;
```

**VB.NET Example:**
```vb
fontComboBox.RightToLeft = RightToLeft.Yes
```

### RTL Behavior

When RTL is enabled:
- Text alignment changes to right
- Dropdown button moves to left side
- Scroll bar appears on left
- Text flows right-to-left

![RTL FontComboBox](images/fontcombobox-rtl.png)

### Inherit RTL from Parent

```csharp
// Inherit RTL setting from parent form
fontComboBox.RightToLeft = RightToLeft.Inherit;

// Set RTL on entire form
this.RightToLeft = RightToLeft.Yes;
```

---

## Complete Example

Full working example combining all basic features:

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace FontComboBoxDemo
{
    public partial class Form1 : Form
    {
        private FontComboBox fontComboBox;
        private Label previewLabel;
        
        public Form1()
        {
            InitializeComponent();
            InitializeControls();
        }
        
        private void InitializeControls()
        {
            // Create FontComboBox
            fontComboBox = new FontComboBox
            {
                Location = new Point(20, 20),
                Size = new Size(250, 25),
                UseAutoComplete = true,
                SelectedItem = "Segoe UI"
            };
            
            // Create preview label
            previewLabel = new Label
            {
                Location = new Point(20, 60),
                Size = new Size(400, 40),
                Text = "The quick brown fox jumps over the lazy dog",
                Font = new Font("Segoe UI", 12)
            };
            
            // Handle font selection
            fontComboBox.SelectedIndexChanged += FontComboBox_SelectedIndexChanged;
            
            // Add controls to form
            this.Controls.Add(fontComboBox);
            this.Controls.Add(previewLabel);
        }
        
        private void FontComboBox_SelectedIndexChanged(object sender, EventArgs e)
        {
            if (fontComboBox.Text != null)
            {
                previewLabel.Font = new Font(
                    fontComboBox.Text.ToString(), 
                    12, 
                    FontStyle.Regular
                );
            }
        }
    }
}
```

---

## Sample Projects

**GitHub Sample:**
- [FontComboBox Getting Started Sample](https://github.com/SyncfusionExamples/GettingStarted-WF-FontComboBox)

**Included Samples:**
- Basic initialization
- Font selection with preview
- Event handling
- Designer and code-based setup

---

## Next Steps

- **AutoComplete Features**: Learn how to configure font search and filtering → [autocomplete.md](autocomplete.md)
- **DropDown Configuration**: Customize dropdown appearance and behavior → [dropdown-configuration.md](dropdown-configuration.md)
- **Selection and Events**: Handle font selection events → [selection-and-events.md](selection-and-events.md)
- **Visual Styles**: Apply themes and custom colors → [visual-styles.md](visual-styles.md)

---

## Troubleshooting

### FontComboBox Not in Toolbox

**Solution:**
1. Verify Syncfusion installation
2. Tools → Choose Toolbox Items
3. Browse to Syncfusion.Tools.Windows.dll
4. Check FontComboBox in list

### Assembly Reference Errors

**Solution:**
1. Verify NuGet package installed
2. Check assembly version compatibility
3. Clean and rebuild solution
4. Ensure correct .NET Framework version

### Fonts Not Loading

**Solution:**
1. Ensure `UseAutoComplete = true` is set
2. Check if fonts exist on system
3. Verify control is properly initialized
4. Try manual Items.Add() for testing
