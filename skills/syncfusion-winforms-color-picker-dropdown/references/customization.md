# Customization Settings

## Table of Contents
- [Overview](#overview)
- [Dropdown Size](#dropdown-size)
- [Button Appearance](#button-appearance)
- [Color Display Modes](#color-display-modes)
- [Advanced Customization](#advanced-customization)
- [Common Customization Patterns](#common-customization-patterns)

## Overview

ColorPickerButton offers several properties to customize its appearance and behavior. The control integrates with the underlying ColorUIControl, allowing you to adjust the dropdown size, display selected colors on the button, and configure visual appearance.

## Dropdown Size

### ColorUISize Property

Controls the size of the dropdown ColorUIControl that appears when the button is clicked.

```csharp
// Set dropdown size
colorPickerButton1.ColorUISize = new System.Drawing.Size(250, 280);
```

### Default Size

The default ColorUIControl size is typically 250 x 280 pixels.

### Common Size Configurations

```csharp
// Compact dropdown
colorPickerButton1.ColorUISize = new System.Drawing.Size(200, 200);

// Standard size
colorPickerButton1.ColorUISize = new System.Drawing.Size(250, 280);

// Large dropdown for accessibility
colorPickerButton1.ColorUISize = new System.Drawing.Size(350, 350);

// Wide palette view
colorPickerButton1.ColorUISize = new System.Drawing.Size(400, 250);
```

### Size Considerations

- **Width:** Minimum 150 pixels (narrow), typical 250-300
- **Height:** Minimum 150 pixels, typical 250-350
- **Responsive:** Adjust based on available screen space
- **Accessibility:** Larger sizes improve usability

### Adjust Size in Constructor

```csharp
private void InitializeColorPicker()
{
    colorPickerButton1.ColorUISize = new System.Drawing.Size(300, 300);
    colorPickerButton1.Size = new System.Drawing.Size(150, 32);
    colorPickerButton1.Location = new System.Drawing.Point(10, 10);
}
```

## Button Appearance

### Display Selected Color as Background

The `SelectedAsBackcolor` property displays the selected color as the button's background.

```csharp
// Enable - button background shows selected color
colorPickerButton1.SelectedAsBackcolor = true;

// Disable - button uses standard appearance
colorPickerButton1.SelectedAsBackcolor = false;
```

### Display Selected Color as Text

The `SelectedAsText` property displays the selected color information as the button's text.

```csharp
// Enable - button text shows color value or name
colorPickerButton1.SelectedAsText = true;

// Disable - button shows default text
colorPickerButton1.SelectedAsText = false;
```

### Combined Display

```csharp
// Show color as both background and text
colorPickerButton1.SelectedAsBackcolor = true;
colorPickerButton1.SelectedAsText = true;
colorPickerButton1.SelectedColor = System.Drawing.Color.Blue;

// Result: Button has blue background with color text
```

## Color Display Modes

### Mode 1: Color Swatch (Background Only)

```csharp
colorPickerButton1.SelectedAsBackcolor = true;
colorPickerButton1.SelectedAsText = false;
colorPickerButton1.Text = "Theme";
colorPickerButton1.SelectedColor = System.Drawing.Color.OrangeRed;

// Result: Red button labeled "Theme"
```

### Mode 2: Color Code (Text Only)

```csharp
colorPickerButton1.SelectedAsBackcolor = false;
colorPickerButton1.SelectedAsText = true;
colorPickerButton1.SelectedColor = System.Drawing.Color.Green;

// Result: Button text shows "Green" or hex code
```

### Mode 3: Color Swatch with Label (Background + Text)

```csharp
colorPickerButton1.SelectedAsBackcolor = true;
colorPickerButton1.SelectedAsText = true;
colorPickerButton1.SelectedColor = System.Drawing.Color.Yellow;

// Result: Yellow button with color information displayed
```

### Mode 4: Standard Button

```csharp
colorPickerButton1.SelectedAsBackcolor = false;
colorPickerButton1.SelectedAsText = false;
colorPickerButton1.Text = "Select Color";

// Result: Regular button appearance; color hidden
```

## Advanced Customization

### Button Text Customization

```csharp
// Static label
colorPickerButton1.Text = "Pick Theme Color";

// Dynamic label based on selection
private void UpdateButtonText()
{
    if (colorPickerButton1.SelectedColor == System.Drawing.Color.Empty)
        colorPickerButton1.Text = "No Color";
    else
        colorPickerButton1.Text = colorPickerButton1.SelectedColor.Name;
}
```

### Size and Layout

```csharp
// Button dimensions
colorPickerButton1.Width = 150;
colorPickerButton1.Height = 35;

// Position
colorPickerButton1.Left = 10;
colorPickerButton1.Top = 10;

// Or use Point and Size
colorPickerButton1.Location = new System.Drawing.Point(10, 10);
colorPickerButton1.Size = new System.Drawing.Size(150, 35);

// Layout docking
colorPickerButton1.Dock = DockStyle.Top;
colorPickerButton1.Anchor = AnchorStyles.Top | AnchorStyles.Left;
```

### Font Customization

```csharp
// Customize button font
colorPickerButton1.Font = new System.Drawing.Font("Arial", 10, FontStyle.Bold);

// Customize text color (when not showing selected color)
colorPickerButton1.ForeColor = System.Drawing.Color.Black;
```

### Border and Styling

```csharp
// Button flat style
colorPickerButton1.FlatStyle = FlatStyle.Flat;

// Button 3D style
colorPickerButton1.FlatStyle = FlatStyle.Standard;

// Popup style (dropdown appearance)
colorPickerButton1.FlatStyle = FlatStyle.Popup;

// System style
colorPickerButton1.FlatStyle = FlatStyle.System;
```

## Common Customization Patterns

### Pattern 1: Theme Selector

```csharp
private void SetupThemeSelector()
{
    // Large color swatch
    colorPickerButton1.Size = new System.Drawing.Size(200, 50);
    colorPickerButton1.SelectedAsBackcolor = true;
    colorPickerButton1.SelectedAsText = false;
    colorPickerButton1.Text = "Theme";
    
    // Standard colors only
    colorPickerButton1.SelectedColorGroup = 
        Syncfusion.Windows.Forms.ColorUISelectedGroup.StandardColors;
    
    // Larger dropdown for better visibility
    colorPickerButton1.ColorUISize = new System.Drawing.Size(300, 300);
}
```

### Pattern 2: Compact Toolbar Button

```csharp
private void SetupToolbarButton()
{
    // Small button for toolbar
    colorPickerButton1.Size = new System.Drawing.Size(32, 32);
    colorPickerButton1.SelectedAsBackcolor = true;
    colorPickerButton1.SelectedAsText = false;
    colorPickerButton1.Text = "";
    
    // Compact dropdown
    colorPickerButton1.ColorUISize = new System.Drawing.Size(200, 200);
    
    // Flat appearance for toolbar
    colorPickerButton1.FlatStyle = FlatStyle.Flat;
}
```

### Pattern 3: Labeled Color Input

```csharp
private void SetupColorInput()
{
    // Button with label and color code
    colorPickerButton1.Size = new System.Drawing.Size(200, 28);
    colorPickerButton1.SelectedAsBackcolor = false;
    colorPickerButton1.SelectedAsText = true;
    
    // Default color
    colorPickerButton1.SelectedColor = System.Drawing.Color.White;
    
    // Standard and custom colors
    colorPickerButton1.SelectedColorGroup = 
        Syncfusion.Windows.Forms.ColorUISelectedGroup.StandardColors;
    
    // Standard size dropdown
    colorPickerButton1.ColorUISize = new System.Drawing.Size(250, 280);
}
```

### Pattern 4: Status Indicator

```csharp
private void SetupStatusIndicator()
{
    // Small color indicator with status text
    colorPickerButton1.Size = new System.Drawing.Size(120, 28);
    colorPickerButton1.SelectedAsBackcolor = true;
    colorPickerButton1.SelectedAsText = true;
    
    // Update colors for status
    UpdateStatusColor("Ready");
    
    // Dropdown for manual override
    colorPickerButton1.ColorUISize = new System.Drawing.Size(250, 280);
}

private void UpdateStatusColor(string status)
{
    switch(status)
    {
        case "Ready":
            colorPickerButton1.SelectedColor = System.Drawing.Color.Green;
            break;
        case "Busy":
            colorPickerButton1.SelectedColor = System.Drawing.Color.Orange;
            break;
        case "Error":
            colorPickerButton1.SelectedColor = System.Drawing.Color.Red;
            break;
    }
}
```

### Pattern 5: Developer Settings

```csharp
private void SetupDevSettings()
{
    // Large, detailed display for development
    colorPickerButton1.Size = new System.Drawing.Size(250, 40);
    colorPickerButton1.SelectedAsBackcolor = true;
    colorPickerButton1.SelectedAsText = true;
    
    // Show all color groups
    colorPickerButton1.SelectedColorGroup = 
        Syncfusion.Windows.Forms.ColorUISelectedGroup.StandardColors;
    
    // Large dropdown with all options
    colorPickerButton1.ColorUISize = new System.Drawing.Size(350, 350);
    
    // Default to custom colors
    colorPickerButton1.SelectedColor = System.Drawing.Color.Blue;
}
```

## Complete Customization Example

```csharp
public partial class ColorCustomizationForm : Form
{
    public ColorCustomizationForm()
    {
        InitializeComponent();
        CustomizeColorPicker();
    }

    private void CustomizeColorPicker()
    {
        // Position and size
        colorPickerButton1.Location = new System.Drawing.Point(20, 20);
        colorPickerButton1.Size = new System.Drawing.Size(180, 40);
        
        // Text
        colorPickerButton1.Text = "Select a Theme Color";
        
        // Appearance
        colorPickerButton1.SelectedAsBackcolor = true;
        colorPickerButton1.SelectedAsText = true;
        colorPickerButton1.FlatStyle = FlatStyle.Flat;
        
        // Font
        colorPickerButton1.Font = new System.Drawing.Font("Segoe UI", 10);
        
        // Default color
        colorPickerButton1.SelectedColor = System.Drawing.Color.Blue;
        
        // Color group
        colorPickerButton1.SelectedColorGroup = 
            Syncfusion.Windows.Forms.ColorUISelectedGroup.StandardColors;
        
        // Dropdown size
        colorPickerButton1.ColorUISize = new System.Drawing.Size(300, 320);
    }
}
```

## Performance Considerations

- **Rendering:** Color display modes impact button rendering performance
- **Size:** Larger ColorUISize may impact dropdown appearance
- **Accessibility:** Balance size and text for readability
- **Theme changes:** May require re-initialization on system theme changes

## Edge Cases

### Handle Empty Color

```csharp
if (colorPickerButton1.SelectedColor == System.Drawing.Color.Empty)
{
    colorPickerButton1.SelectedColor = System.Drawing.Color.White;
    colorPickerButton1.Text = "No color selected";
}
```

### Reset to Defaults

```csharp
private void ResetColorPicker()
{
    colorPickerButton1.SelectedColor = System.Drawing.Color.Empty;
    colorPickerButton1.SelectedAsBackcolor = false;
    colorPickerButton1.SelectedAsText = false;
    colorPickerButton1.ColorUISize = new System.Drawing.Size(250, 280);
    colorPickerButton1.Text = "Select Color";
}
```
