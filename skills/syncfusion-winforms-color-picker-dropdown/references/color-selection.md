# Color Selection & Groups

## Table of Contents
- [Overview](#overview)
- [SelectedColor Property](#selectedcolor-property)
- [Color Groups](#color-groups)
- [SelectedColorGroup Property](#selectedcolorgroup-property)
- [Setting Initial Color](#setting-initial-color)
- [Getting Selected Color](#getting-selected-color)
- [Color Change Events](#color-change-events)
- [Programmatic Color Updates](#programmatic-color-updates)

## Overview

The ColorPickerButton allows users to select colors from multiple predefined groups. The component provides two primary properties for color management:

1. **SelectedColor** - The currently selected color
2. **SelectedColorGroup** - Which color group tab is focused

## SelectedColor Property

### Purpose

Gets or sets the currently selected color in the ColorPickerButton.

### Type

`System.Drawing.Color`

### Usage

```csharp
// Get the selected color
System.Drawing.Color myColor = colorPickerButton1.SelectedColor;

// Set the selected color programmatically
colorPickerButton1.SelectedColor = System.Drawing.Color.Red;
colorPickerButton1.SelectedColor = System.Drawing.Color.FromArgb(255, 128, 0); // Orange
colorPickerButton1.SelectedColor = System.Drawing.ColorTranslator.FromHtml("#FF8000");
```

### Default Value

`System.Drawing.Color.Empty` (no color selected)

### Common Colors

```csharp
// Named colors
colorPickerButton1.SelectedColor = System.Drawing.Color.Blue;
colorPickerButton1.SelectedColor = System.Drawing.Color.Green;
colorPickerButton1.SelectedColor = System.Drawing.Color.Yellow;
colorPickerButton1.SelectedColor = System.Drawing.Color.OrangeRed;

// RGB colors
colorPickerButton1.SelectedColor = System.Drawing.Color.FromArgb(255, 0, 0);    // Red
colorPickerButton1.SelectedColor = System.Drawing.Color.FromArgb(0, 255, 0);    // Green
colorPickerButton1.SelectedColor = System.Drawing.Color.FromArgb(0, 0, 255);    // Blue
colorPickerButton1.SelectedColor = System.Drawing.Color.FromArgb(128, 128, 128); // Gray

// Hex colors
colorPickerButton1.SelectedColor = System.Drawing.ColorTranslator.FromHtml("#FF0000");
colorPickerButton1.SelectedColor = System.Drawing.ColorTranslator.FromHtml("#00FF00");
```

## Color Groups

ColorPickerButton displays colors organized in groups. Users can switch between groups by clicking tabs in the dropdown.

### Available Groups

| Group | Name | Description |
|-------|------|-------------|
| **StandardColors** | Standard | Web-safe colors and common colors |
| **SystemColors** | System | Operating system theme colors |
| **CustomColors** | Custom | Pre-configured custom color palette |
| **UserColors** | User | Colors saved by the user during previous selections |
| **None** | (None) | No group focused (default) |

### Standard Colors Example

Standard colors typically include:
- Primary colors (Red, Green, Blue, Yellow, Cyan, Magenta, Black, White)
- Web-safe palette colors
- Common UI colors

### System Colors Example

System colors reflect the current Windows theme:
- Window background
- Window text
- Button face
- Button text
- Highlight color
- Shadow color

## SelectedColorGroup Property

### Purpose

Gets or sets which color group tab is focused when the dropdown opens.

### Type

`Syncfusion.Windows.Forms.ColorUISelectedGroup` (enumeration)

### Values

```csharp
Syncfusion.Windows.Forms.ColorUISelectedGroup.StandardColors
Syncfusion.Windows.Forms.ColorUISelectedGroup.SystemColors
Syncfusion.Windows.Forms.ColorUISelectedGroup.CustomColors
Syncfusion.Windows.Forms.ColorUISelectedGroup.UserColors
Syncfusion.Windows.Forms.ColorUISelectedGroup.None
```

### Usage

```csharp
// Focus Standard Colors when dropdown opens
colorPickerButton1.SelectedColorGroup = 
    Syncfusion.Windows.Forms.ColorUISelectedGroup.StandardColors;

// Focus System Colors
colorPickerButton1.SelectedColorGroup = 
    Syncfusion.Windows.Forms.ColorUISelectedGroup.SystemColors;

// Focus Custom Colors
colorPickerButton1.SelectedColorGroup = 
    Syncfusion.Windows.Forms.ColorUISelectedGroup.CustomColors;

// Focus User Colors
colorPickerButton1.SelectedColorGroup = 
    Syncfusion.Windows.Forms.ColorUISelectedGroup.UserColors;

// No group focused (default)
colorPickerButton1.SelectedColorGroup = 
    Syncfusion.Windows.Forms.ColorUISelectedGroup.None;
```

## Setting Initial Color

### Set Color and Group on Form Load

```csharp
public partial class Form1 : Form
{
    public Form1()
    {
        InitializeComponent();
    }

    private void Form1_Load(object sender, EventArgs e)
    {
        // Set initial color
        colorPickerButton1.SelectedColor = System.Drawing.Color.OrangeRed;
        
        // Set initial group
        colorPickerButton1.SelectedColorGroup = 
            Syncfusion.Windows.Forms.ColorUISelectedGroup.StandardColors;
    }
}
```

### Set Color in Constructor

```csharp
public partial class ColorPickerForm : Form
{
    private ColorPickerButton colorPicker;

    public ColorPickerForm()
    {
        InitializeComponent();
        
        colorPicker = new ColorPickerButton();
        colorPicker.SelectedColor = System.Drawing.Color.Blue;
        colorPicker.SelectedColorGroup = 
            Syncfusion.Windows.Forms.ColorUISelectedGroup.StandardColors;
        
        this.Controls.Add(colorPicker);
    }
}
```

## Getting Selected Color

### Access Selected Color at Runtime

```csharp
// Get the color in a button click handler
private void GetColorButton_Click(object sender, EventArgs e)
{
    System.Drawing.Color selected = colorPickerButton1.SelectedColor;
    
    // Use the color
    MessageBox.Show($"Selected: {selected.Name}");
    this.BackColor = selected;
}
```

### Extract Color Components

```csharp
System.Drawing.Color color = colorPickerButton1.SelectedColor;

// Get color components
int red = color.R;      // 0-255
int green = color.G;    // 0-255
int blue = color.B;     // 0-255
int alpha = color.A;    // 0-255 (transparency)

// Get color name
string colorName = color.Name;

// Convert to hex
string hexColor = "#" + color.R.ToString("X2") + 
                        color.G.ToString("X2") + 
                        color.B.ToString("X2");

Console.WriteLine($"RGB: ({red}, {green}, {blue})");
Console.WriteLine($"Hex: {hexColor}");
```

### Store Selected Color

```csharp
public class UserPreferences
{
    public System.Drawing.Color ThemeColor { get; set; }
}

// Save user's color selection
UserPreferences prefs = new UserPreferences();
prefs.ThemeColor = colorPickerButton1.SelectedColor;

// Restore on next session
colorPickerButton1.SelectedColor = prefs.ThemeColor;
```

## Color Change Events

### Handle Color Selection

The ColorPickerButton inherits from System.Windows.Forms.Button, so it supports standard button events:

```csharp
// Handle button click (opens color picker)
colorPickerButton1.Click += ColorPickerButton1_Click;

private void ColorPickerButton1_Click(object sender, EventArgs e)
{
    MessageBox.Show("Color picker opened");
}
```

### Detect When Color Changes

Since ColorPickerButton is a Button control, check the SelectedColor property after user interaction:

```csharp
private System.Drawing.Color lastColor = System.Drawing.Color.Empty;

private void CheckColorChanged()
{
    if (colorPickerButton1.SelectedColor != lastColor)
    {
        lastColor = colorPickerButton1.SelectedColor;
        OnColorChanged();
    }
}

private void OnColorChanged()
{
    // React to color change
    this.BackColor = colorPickerButton1.SelectedColor;
}
```

## Programmatic Color Updates

### Update Colors Based on Logic

```csharp
// Set color based on value thresholds
private void SetColorByValue(int value)
{
    if (value < 0)
        colorPickerButton1.SelectedColor = System.Drawing.Color.Red;
    else if (value > 0)
        colorPickerButton1.SelectedColor = System.Drawing.Color.Green;
    else
        colorPickerButton1.SelectedColor = System.Drawing.Color.Yellow;
}

// Example: Status indicator
private void UpdateStatusColor(string status)
{
    switch(status)
    {
        case "Error":
            colorPickerButton1.SelectedColor = System.Drawing.Color.Red;
            break;
        case "Warning":
            colorPickerButton1.SelectedColor = System.Drawing.Color.Orange;
            break;
        case "Success":
            colorPickerButton1.SelectedColor = System.Drawing.Color.Green;
            break;
    }
}
```

### Color Cycling

```csharp
// Cycle through predefined colors
private int colorIndex = 0;
private System.Drawing.Color[] colors = new[]
{
    System.Drawing.Color.Red,
    System.Drawing.Color.Green,
    System.Drawing.Color.Blue,
    System.Drawing.Color.Yellow
};

private void CycleColor()
{
    colorPickerButton1.SelectedColor = colors[colorIndex];
    colorIndex = (colorIndex + 1) % colors.Length;
}
```

## Common Scenarios

### Theme Color Selector

```csharp
// Let user pick theme color from standard palette
colorPickerButton1.SelectedColorGroup = 
    Syncfusion.Windows.Forms.ColorUISelectedGroup.StandardColors;
colorPickerButton1.SelectedColor = System.Drawing.Color.Blue;

// Apply to UI
private void ApplyTheme()
{
    this.BackColor = colorPickerButton1.SelectedColor;
}
```

### Custom Color Palette

```csharp
// Guide users to custom colors for brand colors
colorPickerButton1.SelectedColorGroup = 
    Syncfusion.Windows.Forms.ColorUISelectedGroup.CustomColors;
```

## Edge Cases

### No Color Selected

```csharp
// Check if a color has been selected
if (colorPickerButton1.SelectedColor == System.Drawing.Color.Empty)
{
    MessageBox.Show("Please select a color first");
    return;
}
```

### Preserve Selection

```csharp
// Save current color
System.Drawing.Color previousColor = colorPickerButton1.SelectedColor;

// ... do something ...

// Restore if needed
colorPickerButton1.SelectedColor = previousColor;
```
