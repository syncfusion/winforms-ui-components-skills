# Getting Started with ColorUIControl

This guide covers installation, setup, and basic usage of the Syncfusion ColorUIControl in Windows Forms applications.

## Table of Contents
- [Overview](#overview)
- [Assembly Dependencies](#assembly-dependencies)
- [Installation Methods](#installation-methods)
- [Adding ColorUI via Designer](#adding-colorui-via-designer)
- [Adding ColorUI via Code](#adding-colorui-via-code)
- [Setting Initial Color and Group](#setting-initial-color-and-group)
- [Reset Methods](#reset-methods)
- [Complete Examples](#complete-examples)
- [Troubleshooting](#troubleshooting)

## Overview

The ColorUIControl provides a Visual Studio-style color picker interface for Windows Forms applications. Unlike the standard ColorDialog which appears as a modal window, ColorUIControl can be embedded directly into your application's layout, providing an inline color selection experience.

**Key Setup Requirements:**
- .NET Framework application (Windows Forms)
- Syncfusion.Shared.Base assembly reference
- Basic understanding of Windows Forms controls

## Assembly Dependencies

To use ColorUIControl, you need to reference the following assembly:

**Required Assembly:**
- `Syncfusion.Shared.Base.dll`

### NuGet Package Installation

The recommended approach is to install via NuGet Package Manager:

1. Open your Windows Forms project in Visual Studio
2. Right-click on the project → **Manage NuGet Packages**
3. Search for `Syncfusion.Windows.Forms` or `Syncfusion.Shared.WinForms`
4. Install the package

**Package Manager Console:**
```powershell
Install-Package Syncfusion.Shared.Base
```

### Manual Assembly Reference

If not using NuGet:

1. Right-click project → **Add Reference**
2. Browse to Syncfusion installation folder:
   - Default: `C:\Program Files (x86)\Syncfusion\Essential Studio\<version>\precompiledassemblies\<.NET version>\`
3. Select `Syncfusion.Shared.Base.dll`
4. Click **Add**

## Installation Methods

There are two primary methods to add ColorUIControl to your Windows Forms application:

1. **Designer Method** - Drag and drop from toolbox
2. **Code Method** - Programmatic creation

Both methods are valid; choose based on your development workflow.

## Adding ColorUI via Designer

The designer method is the quickest way to add ColorUIControl to a form.

### Steps:

1. **Create a Windows Forms Application**
   - File → New → Project → Windows Forms App (.NET Framework)

2. **Open Form in Designer**
   - Open `Form1.cs` in design view

3. **Locate ColorUIControl in Toolbox**
   - Open the Toolbox (View → Toolbox or Ctrl+Alt+X)
   - Find "Syncfusion Controls" section
   - Locate `ColorUIControl`

4. **Drag and Drop**
   - Drag `ColorUIControl` from toolbox to your form
   - Position and resize as needed

5. **Automatic Assembly Reference**
   - The `Syncfusion.Shared.Base` assembly is added automatically
   - The designer generates initialization code in `Form1.Designer.cs`

### Designer-Generated Code

After adding via designer, you'll see code like this in `Form1.Designer.cs`:

```csharp
private Syncfusion.Windows.Forms.ColorUIControl colorUIControl1;

private void InitializeComponent()
{
    this.colorUIControl1 = new Syncfusion.Windows.Forms.ColorUIControl();
    this.SuspendLayout();
    
    // colorUIControl1
    this.colorUIControl1.Location = new System.Drawing.Point(12, 12);
    this.colorUIControl1.Name = "colorUIControl1";
    this.colorUIControl1.Size = new System.Drawing.Size(210, 200);
    this.colorUIControl1.TabIndex = 0;
    
    // Form1
    this.Controls.Add(this.colorUIControl1);
    this.ResumeLayout(false);
}
```

## Adding ColorUI via Code

The code method provides more control and is useful for dynamic UI generation.

### Step 1: Add Assembly Reference

Manually add the `Syncfusion.Shared.Base` assembly as described in [Assembly Dependencies](#assembly-dependencies).

### Step 2: Import Namespace

Add the namespace at the top of your code file:

**C#:**
```csharp
using Syncfusion.Windows.Forms;
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms
```

### Step 3: Declare the Control

**C#:**
```csharp
private Syncfusion.Windows.Forms.ColorUIControl colorUIControl1;
```

**VB.NET:**
```vb
Private colorUIControl1 As Syncfusion.Windows.Forms.ColorUIControl
```

### Step 4: Initialize and Add to Form

**C#:**
```csharp
// Initialize the control
this.colorUIControl1 = new Syncfusion.Windows.Forms.ColorUIControl();

// Set size
this.colorUIControl1.Size = new System.Drawing.Size(210, 200);

// Set location
this.colorUIControl1.Location = new System.Drawing.Point(20, 20);

// Add to form
this.Controls.Add(this.colorUIControl1);
```

**VB.NET:**
```vb
' Initialize the control
Me.colorUIControl1 = New Syncfusion.Windows.Forms.ColorUIControl()

' Set size
Me.colorUIControl1.Size = New System.Drawing.Size(210, 200)

' Set location
Me.colorUIControl1.Location = New System.Drawing.Point(20, 20)

' Add to form
Me.Controls.Add(Me.colorUIControl1)
```

### Complete Code Method Example

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;

namespace ColorPickerApp
{
    public partial class Form1 : Form
    {
        private ColorUIControl colorUIControl1;
        
        public Form1()
        {
            InitializeComponent();
            InitializeColorUI();
        }
        
        private void InitializeColorUI()
        {
            // Create instance
            this.colorUIControl1 = new ColorUIControl();
            
            // Configure properties
            this.colorUIControl1.Size = new Size(210, 200);
            this.colorUIControl1.Location = new Point(20, 20);
            this.colorUIControl1.Name = "colorUIControl1";
            
            // Add to form
            this.Controls.Add(this.colorUIControl1);
        }
    }
}
```

**VB.NET:**
```vb
Imports System.Drawing
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms

Public Class Form1
    Private colorUIControl1 As ColorUIControl
    
    Public Sub New()
        InitializeComponent()
        InitializeColorUI()
    End Sub
    
    Private Sub InitializeColorUI()
        ' Create instance
        Me.colorUIControl1 = New ColorUIControl()
        
        ' Configure properties
        Me.colorUIControl1.Size = New Size(210, 200)
        Me.colorUIControl1.Location = New Point(20, 20)
        Me.colorUIControl1.Name = "colorUIControl1"
        
        ' Add to form
        Me.Controls.Add(Me.colorUIControl1)
    End Sub
End Class
```

## Setting Initial Color and Group

After creating the ColorUIControl, you can set the initial color and active color group.

### SelectedColor Property

Sets the initially selected color in the palette.

**C#:**
```csharp
// Set initial color to OrangeRed
this.colorUIControl1.SelectedColor = System.Drawing.Color.OrangeRed;

// Set to custom RGB color
this.colorUIControl1.SelectedColor = Color.FromArgb(255, 128, 64);

// Set to named color
this.colorUIControl1.SelectedColor = Color.DarkBlue;
```

**VB.NET:**
```vb
' Set initial color to OrangeRed
Me.colorUIControl1.SelectedColor = System.Drawing.Color.OrangeRed

' Set to custom RGB color
Me.colorUIControl1.SelectedColor = Color.FromArgb(255, 128, 64)

' Set to named color
Me.colorUIControl1.SelectedColor = Color.DarkBlue
```

### SelectedColorGroup Property

Sets which color group tab is initially displayed.

**Available Options:**
- `ColorUISelectedGroup.SystemColors` - System colors (from SystemColors class)
- `ColorUISelectedGroup.StandardColors` - Standard basic colors
- `ColorUISelectedGroup.CustomColors` - Customizable color palette
- `ColorUISelectedGroup.UserColors` - User-defined colors
- `ColorUISelectedGroup.None` - No group selected (default)

**C#:**
```csharp
// Show StandardColors tab by default
this.colorUIControl1.SelectedColorGroup = 
    Syncfusion.Windows.Forms.ColorUISelectedGroup.StandardColors;

// Show CustomColors tab
this.colorUIControl1.SelectedColorGroup = 
    ColorUISelectedGroup.CustomColors;
```

**VB.NET:**
```vb
' Show StandardColors tab by default
Me.colorUIControl1.SelectedColorGroup = _
    Syncfusion.Windows.Forms.ColorUISelectedGroup.StandardColors

' Show CustomColors tab
Me.colorUIControl1.SelectedColorGroup = _
    ColorUISelectedGroup.CustomColors
```

### Setting Both Color and Group

**C#:**
```csharp
// Set color and ensure the correct group is displayed
this.colorUIControl1.SelectedColor = Color.OrangeRed;
this.colorUIControl1.SelectedColorGroup = ColorUISelectedGroup.StandardColors;
```

**VB.NET:**
```vb
' Set color and ensure the correct group is displayed
Me.colorUIControl1.SelectedColor = Color.OrangeRed
Me.colorUIControl1.SelectedColorGroup = ColorUISelectedGroup.StandardColors
```

## Reset Methods

ColorUIControl provides reset methods to restore default values.

### ResetSelectedColor()

Resets the selected color to the default (typically Color.Empty or the first color in the palette).

**C#:**
```csharp
// Reset to default color
this.colorUIControl1.ResetSelectedColor();
```

**VB.NET:**
```vb
' Reset to default color
Me.colorUIControl1.ResetSelectedColor()
```

### ResetSelectedColorGroup()

Resets the selected color group to None (no tab selected).

**C#:**
```csharp
// Reset to no group selected
this.colorUIControl1.ResetSelectedColorGroup();
```

**VB.NET:**
```vb
' Reset to no group selected
Me.colorUIControl1.ResetSelectedColorGroup()
```

### Using Reset in Clear Button

**C#:**
```csharp
private void btnClear_Click(object sender, EventArgs e)
{
    // Clear both color and group selection
    colorUIControl1.ResetSelectedColor();
    colorUIControl1.ResetSelectedColorGroup();
    
    MessageBox.Show("Color selection cleared");
}
```

## Complete Examples

### Example 1: Basic Setup with Event Handler

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;

public class ColorPickerForm : Form
{
    private ColorUIControl colorUIControl1;
    private Panel previewPanel;
    
    public ColorPickerForm()
    {
        InitializeComponents();
    }
    
    private void InitializeComponents()
    {
        // Create ColorUIControl
        this.colorUIControl1 = new ColorUIControl();
        this.colorUIControl1.Size = new Size(210, 200);
        this.colorUIControl1.Location = new Point(20, 20);
        this.colorUIControl1.SelectedColor = Color.LightBlue;
        this.colorUIControl1.SelectedColorGroup = ColorUISelectedGroup.StandardColors;
        this.colorUIControl1.ColorSelected += ColorUIControl1_ColorSelected;
        
        // Create preview panel
        this.previewPanel = new Panel();
        this.previewPanel.Size = new Size(210, 50);
        this.previewPanel.Location = new Point(20, 230);
        this.previewPanel.BorderStyle = BorderStyle.FixedSingle;
        this.previewPanel.BackColor = Color.LightBlue;
        
        // Add controls to form
        this.Controls.Add(this.colorUIControl1);
        this.Controls.Add(this.previewPanel);
        
        // Form settings
        this.Text = "Color Picker Example";
        this.Size = new Size(270, 330);
    }
    
    private void ColorUIControl1_ColorSelected(object sender, EventArgs e)
    {
        // Update preview panel with selected color
        this.previewPanel.BackColor = this.colorUIControl1.SelectedColor;
    }
}
```

### Example 2: Designer-Based Setup

When using the designer, add the event handler manually:

1. Select `colorUIControl1` in the designer
2. Open Properties window (F4)
3. Click Events button (lightning icon)
4. Double-click `ColorSelected` event
5. Add your event handler code

```csharp
private void colorUIControl1_ColorSelected(object sender, EventArgs e)
{
    // Get selected color
    Color selectedColor = colorUIControl1.SelectedColor;
    
    // Update UI or apply color
    this.BackColor = selectedColor;
}
```

## Troubleshooting

### Issue: ColorUIControl Not in Toolbox

**Solution:**
1. Verify Syncfusion installation is complete
2. Right-click Toolbox → "Choose Items"
3. Browse to `Syncfusion.Shared.Base.dll`
4. Check `ColorUIControl` and click OK

### Issue: "Type or namespace 'Syncfusion' could not be found"

**Solution:**
- Verify assembly reference: Right-click project → Add Reference → Browse to Syncfusion.Shared.Base.dll
- Check that `using Syncfusion.Windows.Forms;` is added
- Ensure the correct .NET Framework version is targeted

### Issue: Control Appears but is Empty/Blank

**Solution:**
- Verify `Size` property is set (minimum 200x180 recommended)
- Check that `ColorGroups` property includes at least one group
- Ensure the control is visible (`Visible = true`)

### Issue: Selected Color Not Highlighted

**Solution:**
- Set `SelectedColorGroup` to match the group containing the color
- Verify the color exists in the specified group
- Check that `SelectedColor` is set after the control is initialized

### Issue: Cannot Get Selected Color

**Solution:**
```csharp
// Always check if control is initialized
if (colorUIControl1 != null)
{
    Color currentColor = colorUIControl1.SelectedColor;
    // Use the color
}
```

### Common Setup Mistakes

1. **Forgetting to add to Controls collection** - Control won't appear
   ```csharp
   this.Controls.Add(colorUIControl1); // Don't forget this!
   ```

2. **Setting size too small** - Control will be clipped
   ```csharp
   // Minimum recommended size
   colorUIControl1.Size = new Size(210, 200);
   ```

3. **Not setting SelectedColorGroup** - No tab will be active by default
   ```csharp
   // Always set an initial group
   colorUIControl1.SelectedColorGroup = ColorUISelectedGroup.StandardColors;
   ```

4. **Missing namespace import** - Compiler errors
   ```csharp
   using Syncfusion.Windows.Forms; // Required!
   ```

## Next Steps

Now that ColorUIControl is set up, explore:
- [Color Groups](color-groups.md) - Learn about System, Standard, Custom, and User color groups
- [Appearance Customization](appearance-customization.md) - Customize borders, tabs, and visual appearance
- [Events](events.md) - Handle color selection events
- [Popup Integration](popup-integration.md) - Use ColorUIControl in popup menus and dropdowns
