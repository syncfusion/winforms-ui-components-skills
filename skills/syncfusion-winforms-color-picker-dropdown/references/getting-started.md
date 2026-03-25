# Getting Started with ColorPickerButton

## Table of Contents
- [Assembly Deployment](#assembly-deployment)
- [Adding via Designer](#adding-via-designer)
- [Adding via Code](#adding-via-code)
- [Basic Setup](#basic-setup)
- [Testing the Component](#testing-the-component)

## Assembly Deployment

### NuGet Installation

To use the ColorPickerButton control, install the Syncfusion Windows Forms NuGet package:

```powershell
Install-Package Syncfusion.Tools.Windows
```

Or through the NuGet Package Manager in Visual Studio:

1. Right-click project → **Manage NuGet Packages**
2. Search for `Syncfusion.Windows.Forms`
3. Click **Install**

### Assembly References

The following assembly is required:
- **Syncfusion.Shared.Base.dll**

This assembly is automatically added when:
- Installing via NuGet
- Dragging ColorPickerButton from toolbox in Visual Studio designer

## Adding via Designer

### Step-by-Step Instructions

1. **Create a New Windows Forms Application**
   - Open Visual Studio
   - Create → New Project → Windows Forms App (.NET Framework or .NET Core)
   - Name the project (e.g., "ColorPickerDemo")

2. **Locate ColorPickerButton in Toolbox**
   - Open the toolbox (View → Toolbox or Ctrl+Alt+X)
   - Expand "Syncfusion Components"
   - Find "ColorPickerButton"

3. **Drag to Design Surface**
   - Drag ColorPickerButton from toolbox to the form designer
   - The required assembly (Syncfusion.Shared.Base) is automatically added

4. **Configure in Properties Panel**
   - Select the control on the designer
   - In the Properties panel:
     - Set `Name` to a meaningful identifier (e.g., "colorPickerButton1")
     - Set `Text` to "Select Color" or your preferred label
     - Set `Size` and `Location` as needed
     - Set `Dock` or `Anchor` for layout

5. **Run and Test**
   - Press F5 or click the Run button
   - Click the button to open the color picker

## Adding via Code

### Create an Instance Programmatically

```csharp
using Syncfusion.Windows.Forms;

public partial class Form1 : Form
{
    private Syncfusion.Windows.Forms.ColorPickerButton colorPickerButton1;

    public Form1()
    {
        InitializeComponent();
        
        // Create a new ColorPickerButton instance
        this.colorPickerButton1 = new Syncfusion.Windows.Forms.ColorPickerButton();
        
        // Set basic properties
        this.colorPickerButton1.Name = "colorPickerButton1";
        this.colorPickerButton1.Text = "Select a Color";
        this.colorPickerButton1.Location = new System.Drawing.Point(10, 10);
        this.colorPickerButton1.Size = new System.Drawing.Size(150, 30);
        
        // Add to form's control collection
        this.Controls.Add(this.colorPickerButton1);
    }
}
```

### Required Namespace

Always include the Syncfusion namespace:

```csharp
using Syncfusion.Windows.Forms;
```

## Basic Setup

### Minimal Configuration

```csharp
// Create instance
var colorPicker = new ColorPickerButton();

// Set essential properties
colorPicker.Text = "Pick Color";
colorPicker.Location = new Point(20, 20);
colorPicker.Size = new Size(120, 32);

// Add to form
this.Controls.Add(colorPicker);
```

### Set Default Color and Group

```csharp
// Set the initially selected color
colorPickerButton1.SelectedColor = System.Drawing.Color.OrangeRed;

// Set the initially focused color group
colorPickerButton1.SelectedColorGroup = 
    Syncfusion.Windows.Forms.ColorUISelectedGroup.StandardColors;
```

### Color Groups Available

- **StandardColors** - Standard web colors
- **SystemColors** - System colors
- **CustomColors** - Custom saved colors
- **UserColors** - User-defined colors
- **None** - No group selected (default)

### Display Color Information

```csharp
// Show selected color as button background
colorPickerButton1.SelectedAsBackcolor = true;

// Show selected color as button text
colorPickerButton1.SelectedAsText = true;
```

## Testing the Component

### Runtime Testing

1. **Run the Application**
   - Press F5 or Debug → Start Debugging
   - The form with ColorPickerButton appears

2. **Interact with the Control**
   - Click the ColorPickerButton
   - The color picker dropdown opens
   - Select a color from any group tab (Standard, System, Custom, User)
   - Click the color or OK button to confirm
   - Button updates with selected color

3. **Verify Selection**
   - Check that the button reflects the selected color
   - If `SelectedAsBackcolor` is true, background changes
   - If `SelectedAsText` is true, text updates

### Programmatic Testing

```csharp
// Access the selected color at runtime
System.Drawing.Color selectedColor = colorPickerButton1.SelectedColor;
Console.WriteLine($"Selected color: {selectedColor.Name}");

// Change color programmatically
colorPickerButton1.SelectedColor = System.Drawing.Color.Blue;
```

### Verification Checklist

- [ ] Control displays in designer without errors
- [ ] Clicking button opens the color picker dropdown
- [ ] Color selection works properly
- [ ] Selected color updates button appearance
- [ ] No compilation errors
- [ ] Control responds to property changes

## Troubleshooting

### "ColorPickerButton not found in toolbox"
- Verify NuGet package is installed: `Syncfusion.Windows.Forms`
- Rebuild the solution (Build → Rebuild Solution)
- Close and reopen Visual Studio if needed

### "Syncfusion.Windows.Forms namespace not found"
- Ensure NuGet package is installed
- Add `using Syncfusion.Windows.Forms;` at the top
- Verify project references the Syncfusion.Shared.Base assembly

### "Toolbox buttons won't register after installation"
- Close Visual Studio
- Delete the toolbox cache at `%LocalAppData%\Microsoft\VisualStudio\[version]\ComponentModelCache\`
- Restart Visual Studio

## Next Steps

- Set custom colors using [Color Selection & Groups](color-selection.md)
- Customize appearance with [Customization Settings](customization.md)
- Configure UI properties with [UI Appearance Properties](ui-properties.md)
