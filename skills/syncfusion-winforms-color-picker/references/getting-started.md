# Getting Started with ColorPickerUIAdv

Complete guide for setting up and implementing the ColorPickerUIAdv control in Windows Forms applications.

## Assembly Dependencies

The ColorPickerUIAdv control requires the following assembly references:

**Required Assemblies:**
- `Syncfusion.Grid.Base.dll` - Grid base functionality
- `Syncfusion.Grid.Windows.dll` - Windows grid components
- `Syncfusion.Shared.Base.dll` - Shared base functionality
- `Syncfusion.Shared.Windows.dll` - Windows-specific shared components
- `Syncfusion.Tools.Base.dll` - Tools base functionality
- `Syncfusion.Tools.Windows.dll` - Windows tools components (ColorPickerUIAdv included here)

### NuGet Package Installation

The easiest way to add ColorPickerUIAdv is via NuGet Package Manager:

**Package Manager Console:**
```powershell
Install-Package Syncfusion.Tools.Windows
```

**Visual Studio NuGet UI:**
1. Right-click project → **Manage NuGet Packages**
2. Search for **"Syncfusion.Tools.WinForms"**
3. Click **Install**
4. Accept license agreement

This automatically adds all required assembly references.

### Manual Assembly Reference

If not using NuGet, add references manually:

1. Right-click project → **Add Reference**
2. Browse to Syncfusion installation folder (typically `C:\Program Files (x86)\Syncfusion\Essential Studio\{Version}\Assemblies\`)
3. Select all six assemblies listed above
4. Click **OK**

## Adding ColorPickerUIAdv via Designer

The designer approach provides visual configuration and automatic assembly referencing.

### Steps:

1. **Create Windows Forms Project**
   - File → New → Project
   - Select **Windows Forms App (.NET Framework)**
   - Choose .NET Framework 4.5 or higher
   - Click **Create**

2. **Open Form in Designer**
   - Double-click Form1.cs in Solution Explorer
   - Designer view opens automatically

3. **Locate Control in Toolbox**
   - View → Toolbox (or Ctrl+Alt+X)
   - Scroll to **Syncfusion Controls** section
   - Find **ColorPickerUIAdv**

   **Note:** If not visible, right-click Toolbox → **Choose Items** → Browse to Syncfusion.Tools.Windows.dll

4. **Drag to Form**
   - Drag ColorPickerUIAdv from toolbox onto form
   - Position where needed
   - Assemblies are automatically added to References

5. **Configure Properties**
   - Select control in designer
   - Modify properties in Properties window (F4)
   - Set SelectedColor, Style, Size, etc.

### Designer Result

The designer generates initialization code in `Form1.Designer.cs`:

```csharp
private Syncfusion.Windows.Forms.Tools.ColorPickerUIAdv colorPickerUIAdv1;

private void InitializeComponent()
{
    this.colorPickerUIAdv1 = new Syncfusion.Windows.Forms.Tools.ColorPickerUIAdv();
    this.SuspendLayout();
    
    // colorPickerUIAdv1
    this.colorPickerUIAdv1.Location = new System.Drawing.Point(20, 20);
    this.colorPickerUIAdv1.Name = "colorPickerUIAdv1";
    this.colorPickerUIAdv1.Size = new System.Drawing.Size(200, 180);
    this.colorPickerUIAdv1.TabIndex = 0;
    
    // Form1
    this.Controls.Add(this.colorPickerUIAdv1);
    this.ResumeLayout(false);
}
```

## Adding ColorPickerUIAdv via Code

Programmatic approach provides dynamic control creation and runtime configuration.

### Steps:

1. **Create Windows Forms Project** (as above)

2. **Add Assembly References**
   - Right-click References → Add Reference
   - Add all six required assemblies
   - Click OK

3. **Import Namespace**

Add using directive at the top of your code file:

```csharp
using Syncfusion.Windows.Forms.Tools;
```

**Visual Basic:**
```vb
Imports Syncfusion.Windows.Forms.Tools
```

4. **Declare Field**

In your form class:

```csharp
public partial class Form1 : Form
{
    private ColorPickerUIAdv colorPickerUIAdv1;
    
    // ... rest of class
}
```

**Visual Basic:**
```vb
Public Partial Class Form1
    Inherits Form
    
    Private colorPickerUIAdv1 As ColorPickerUIAdv
    
    ' ... rest of class
End Class
```

5. **Instantiate and Configure**

In constructor or Form_Load:

```csharp
public Form1()
{
    InitializeComponent();
    
    // Create instance
    colorPickerUIAdv1 = new ColorPickerUIAdv();
    
    // Configure properties
    colorPickerUIAdv1.Size = new Size(200, 180);
    colorPickerUIAdv1.Location = new Point(20, 20);
    colorPickerUIAdv1.Name = "colorPickerUIAdv1";
    
    // Add to form
    this.Controls.Add(colorPickerUIAdv1);
}
```

**Visual Basic:**
```vb
Public Sub New()
    InitializeComponent()
    
    ' Create instance
    colorPickerUIAdv1 = New ColorPickerUIAdv()
    
    ' Configure properties
    colorPickerUIAdv1.Size = New Size(200, 180)
    colorPickerUIAdv1.Location = New Point(20, 20)
    colorPickerUIAdv1.Name = "colorPickerUIAdv1"
    
    ' Add to form
    Me.Controls.Add(colorPickerUIAdv1)
End Sub
```

## Basic Color Selection

The **SelectedColor** property controls which color is focused or selected.

### Setting Initial Color

```csharp
// Set selected color at initialization
colorPickerUIAdv1.SelectedColor = Color.Blue;

// Or at runtime
colorPickerUIAdv1.SelectedColor = Color.FromArgb(255, 100, 150, 200); // ARGB
colorPickerUIAdv1.SelectedColor = Color.FromArgb(100, 150, 200);      // RGB
```

**Visual Basic:**
```vb
' Set selected color
colorPickerUIAdv1.SelectedColor = Color.Blue

' Or with custom RGB
colorPickerUIAdv1.SelectedColor = Color.FromArgb(255, 100, 150, 200)
```

### Getting Selected Color

```csharp
// Get current selection
Color selectedColor = colorPickerUIAdv1.SelectedColor;

// Use the color
this.BackColor = selectedColor;
label1.ForeColor = selectedColor;
```

### Accessing SelectedItem

For more information about the selected item:

```csharp
// Get the selected color item object
ColorItem item = colorPickerUIAdv1.SelectedItem;

if (item != null)
{
    Color color = item.Color;
    // Additional item properties available
}
```

## Complete Minimal Example

### C# Example

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace ColorPickerDemo
{
    public partial class Form1 : Form
    {
        private ColorPickerUIAdv colorPickerUIAdv1;
        private Panel previewPanel;
        private Label lblInfo;
        
        public Form1()
        {
            InitializeComponent();
            InitializeColorPicker();
        }
        
        private void InitializeColorPicker()
        {
            // Create ColorPickerUIAdv
            colorPickerUIAdv1 = new ColorPickerUIAdv();
            colorPickerUIAdv1.Size = new Size(200, 180);
            colorPickerUIAdv1.Location = new Point(20, 20);
            colorPickerUIAdv1.SelectedColor = Color.White;
            
            // Create preview panel
            previewPanel = new Panel();
            previewPanel.Size = new Size(200, 100);
            previewPanel.Location = new Point(20, 210);
            previewPanel.BorderStyle = BorderStyle.FixedSingle;
            previewPanel.BackColor = Color.White;
            
            // Create info label
            lblInfo = new Label();
            lblInfo.Location = new Point(20, 320);
            lblInfo.Size = new Size(200, 20);
            lblInfo.Text = "Selected: White";
            
            // Handle color selection
            colorPickerUIAdv1.Picked += ColorPickerUIAdv1_Picked;
            
            // Add controls to form
            this.Controls.Add(colorPickerUIAdv1);
            this.Controls.Add(previewPanel);
            this.Controls.Add(lblInfo);
        }
        
        private void ColorPickerUIAdv1_Picked(object sender, 
                                               ColorPickerUIAdv.ColorPickedEventArgs e)
        {
            // Update preview panel
            previewPanel.BackColor = e.Color;
            
            // Update info label
            lblInfo.Text = $"Selected: {e.Color.Name}";
        }
    }
}
```

### Visual Basic Example

```vb
Imports System
Imports System.Drawing
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Namespace ColorPickerDemo
    Public Partial Class Form1
        Inherits Form
        
        Private colorPickerUIAdv1 As ColorPickerUIAdv
        Private previewPanel As Panel
        Private lblInfo As Label
        
        Public Sub New()
            InitializeComponent()
            InitializeColorPicker()
        End Sub
        
        Private Sub InitializeColorPicker()
            ' Create ColorPickerUIAdv
            colorPickerUIAdv1 = New ColorPickerUIAdv()
            colorPickerUIAdv1.Size = New Size(200, 180)
            colorPickerUIAdv1.Location = New Point(20, 20)
            colorPickerUIAdv1.SelectedColor = Color.White
            
            ' Create preview panel
            previewPanel = New Panel()
            previewPanel.Size = New Size(200, 100)
            previewPanel.Location = New Point(20, 210)
            previewPanel.BorderStyle = BorderStyle.FixedSingle
            previewPanel.BackColor = Color.White
            
            ' Create info label
            lblInfo = New Label()
            lblInfo.Location = New Point(20, 320)
            lblInfo.Size = New Size(200, 20)
            lblInfo.Text = "Selected: White"
            
            ' Handle color selection
            AddHandler colorPickerUIAdv1.Picked, AddressOf ColorPickerUIAdv1_Picked
            
            ' Add controls to form
            Me.Controls.Add(colorPickerUIAdv1)
            Me.Controls.Add(previewPanel)
            Me.Controls.Add(lblInfo)
        End Sub
        
        Private Sub ColorPickerUIAdv1_Picked(sender As Object, 
                                              e As ColorPickerUIAdv.ColorPickedEventArgs)
            ' Update preview panel
            previewPanel.BackColor = e.Color
            
            ' Update info label
            lblInfo.Text = $"Selected: {e.Color.Name}"
        End Sub
    End Class
End Namespace
```

## Next Steps

- **Color Organization:** Read [color-groups.md](color-groups.md) to learn about organizing colors into groups
- **Visual Styling:** See [appearance-styling.md](appearance-styling.md) for Office themes and custom styles
- **Event Handling:** Explore [events-selection.md](events-selection.md) for advanced event patterns
- **Layout Customization:** Check [customization.md](customization.md) for spacing, sizing, and headers

## Common Issues

### Toolbox Not Showing ColorPickerUIAdv

**Cause:** Assemblies not registered in Visual Studio toolbox  
**Solution:**
1. Right-click Toolbox → Choose Items
2. Click Browse
3. Navigate to Syncfusion.Tools.Windows.dll
4. Click OK
5. ColorPickerUIAdv should now appear

### "Could not load file or assembly" Error

**Cause:** Missing assembly references or version mismatch  
**Solution:**
1. Verify all six assemblies are referenced
2. Check assembly versions match
3. Ensure Copy Local = True for all references
4. Clean and rebuild solution

### Control Not Visible at Runtime

**Cause:** Not added to Controls collection or Size is too small  
**Solution:**
```csharp
// Ensure control is added
this.Controls.Add(colorPickerUIAdv1);

// Set appropriate size
colorPickerUIAdv1.Size = new Size(200, 180); // Minimum recommended
```

### Design-Time Error "Failed to create component"

**Cause:** Licensing or assembly loading issue  
**Solution:**
1. Verify Syncfusion license is registered
2. Clean obj and bin folders
3. Rebuild solution
4. Restart Visual Studio if needed
