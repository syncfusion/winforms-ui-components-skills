# Color Groups in ColorUIControl

This guide covers the four built-in color groups, controlling their visibility, customizing color palettes, and working with user-defined colors.

## Table of Contents
- [Overview](#overview)
- [The Four Color Groups](#the-four-color-groups)
- [Controlling Visible Groups](#controlling-visible-groups)
- [Working with ColorGroups Property](#working-with-colorgroups-property)
- [Adding User Groups](#adding-user-groups)
- [Selecting Active Color Group](#selecting-active-color-group)
- [Customizing User Color Cells](#customizing-user-color-cells)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

ColorUIControl organizes colors into four predefined groups, each displayed as a separate tab in the control. You can control which groups are visible, set which group is active by default, and customize the colors within certain groups.

**Key Concepts:**
- **ColorGroups** property controls which tabs are visible (flags enum)
- **SelectedColorGroup** property controls which tab is active
- **UserColors** and **UserCustomColors** collections allow programmatic customization
- Each group serves a different purpose (system integration, web colors, custom palettes, user-defined)

## The Four Color Groups

### 1. SystemColors

Contains colors defined in the .NET `SystemColors` class. These are the colors Windows uses for UI elements like window backgrounds, text, borders, etc.

**Examples:**
- ActiveBorder, ActiveCaption
- Control, ControlText
- Window, WindowText
- Highlight, HighlightText
- ButtonFace, ButtonShadow

**When to Use:**
- Integrating with Windows UI themes
- Ensuring consistency with system appearance
- Accessibility and high-contrast themes

### 2. StandardColors

A palette of basic, web-safe colors commonly used across applications. This is typically the most used group for general color selection.

**Examples:**
- Basic colors: Red, Green, Blue, Yellow, Orange
- Shades: LightBlue, DarkGreen, PaleYellow
- Neutrals: Black, White, Gray shades

**When to Use:**
- General purpose color picking
- Web design and HTML colors
- Familiar color palette for users
- Default group for most applications

### 3. CustomColors

A customizable color palette that can be programmatically populated. By default, it provides a standard palette, but you can replace it with your own colors.

**When to Use:**
- Brand-specific color schemes
- Theme-based applications
- Predefined color palettes for specific domains
- Corporate color guidelines

### 4. UserColors

Provides different shades and variations of user-defined colors. This group displays two color collections: `UserColors` and `UserCustomColors`, both of which can be programmatically customized.

**When to Use:**
- Recently used colors
- Favorite colors
- Dynamically generated color palettes
- Application-specific color schemes

## Controlling Visible Groups

Use the `ColorGroups` property to control which color group tabs are displayed. This is a flags enumeration, so you can combine multiple groups using the bitwise OR operator (`|`).

### Showing All Groups (Default)

**C#:**
```csharp
// Show all four groups
this.colorUIControl1.ColorGroups = 
    ColorUIGroups.SystemColors | 
    ColorUIGroups.StandardColors | 
    ColorUIGroups.CustomColors | 
    ColorUIGroups.UserColors;
```

**VB.NET:**
```vb
' Show all four groups
Me.colorUIControl1.ColorGroups = _
    ColorUIGroups.SystemColors Or _
    ColorUIGroups.StandardColors Or _
    ColorUIGroups.CustomColors Or _
    ColorUIGroups.UserColors
```

### Showing Specific Groups Only

**C#:**
```csharp
// Show only Standard and Custom colors
this.colorUIControl1.ColorGroups = 
    ColorUIGroups.StandardColors | ColorUIGroups.CustomColors;

// Show only Standard colors (simplest picker)
this.colorUIControl1.ColorGroups = ColorUIGroups.StandardColors;

// Show System and Standard
this.colorUIControl1.ColorGroups = 
    ColorUIGroups.SystemColors | ColorUIGroups.StandardColors;
```

**VB.NET:**
```vb
' Show only Standard and Custom colors
Me.colorUIControl1.ColorGroups = _
    ColorUIGroups.StandardColors Or ColorUIGroups.CustomColors

' Show only Standard colors (simplest picker)
Me.colorUIControl1.ColorGroups = ColorUIGroups.StandardColors

' Show System and Standard
Me.colorUIControl1.ColorGroups = _
    ColorUIGroups.SystemColors Or ColorUIGroups.StandardColors
```

## Working with ColorGroups Property

### ColorUIGroups Enumeration

```csharp
[Flags]
public enum ColorUIGroups
{
    None = 0,
    SystemColors = 1,
    StandardColors = 2,
    CustomColors = 4,
    UserColors = 8
}
```

### Checking if a Group is Visible

**C#:**
```csharp
// Check if UserColors group is included
bool hasUserColors = (colorUIControl1.ColorGroups & ColorUIGroups.UserColors) 
    == ColorUIGroups.UserColors;

if (hasUserColors)
{
    // UserColors group is visible
}
```

### Adding a Group to Existing Configuration

**C#:**
```csharp
// Add UserColors to existing groups
colorUIControl1.ColorGroups |= ColorUIGroups.UserColors;
```

### Removing a Group from Configuration

**C#:**
```csharp
// Remove CustomColors from existing groups
colorUIControl1.ColorGroups &= ~ColorUIGroups.CustomColors;
```

## Adding User Groups

To display the UserColors group, include it in the `ColorGroups` property. By default, the UserColors palette uses the CustomColors palette.

**C#:**
```csharp
// Include UserColors along with other groups
this.colorUIControl1.ColorGroups = 
    ColorUIGroups.CustomColors | 
    ColorUIGroups.StandardColors | 
    ColorUIGroups.UserColors;
```

**VB.NET:**
```vb
' Include UserColors along with other groups
Me.colorUIControl1.ColorGroups = _
    ColorUIGroups.CustomColors Or _
    ColorUIGroups.StandardColors Or _
    ColorUIGroups.UserColors
```

**Important Note:** The UserColors group must be explicitly included in `ColorGroups` for custom user color settings to take effect.

## Selecting Active Color Group

Use `SelectedColorGroup` to programmatically set which tab is active when the control loads or at runtime.

### Setting at Design Time or Initialization

**C#:**
```csharp
// Set StandardColors as the active tab
this.colorUIControl1.SelectedColorGroup = 
    ColorUISelectedGroup.StandardColors;

// Set UserColors as active
this.colorUIControl1.SelectedColorGroup = 
    ColorUISelectedGroup.UserColors;

// Set to None (no tab selected)
this.colorUIControl1.SelectedColorGroup = 
    ColorUISelectedGroup.None;
```

**VB.NET:**
```vb
' Set StandardColors as the active tab
Me.colorUIControl1.SelectedColorGroup = _
    ColorUISelectedGroup.StandardColors

' Set UserColors as active
Me.colorUIControl1.SelectedColorGroup = _
    ColorUISelectedGroup.UserColors

' Set to None (no tab selected)
Me.colorUIControl1.SelectedColorGroup = _
    ColorUISelectedGroup.None
```

### Available SelectedColorGroup Options

```csharp
public enum ColorUISelectedGroup
{
    None = 0,           // No group selected
    SystemColors = 1,   // System colors tab
    StandardColors = 2, // Standard colors tab
    CustomColors = 3,   // Custom colors tab
    UserColors = 4      // User colors tab
}
```

### Changing Group Programmatically

**C#:**
```csharp
private void btnShowStandard_Click(object sender, EventArgs e)
{
    // Switch to Standard colors tab
    colorUIControl1.SelectedColorGroup = ColorUISelectedGroup.StandardColors;
}

private void btnShowCustom_Click(object sender, EventArgs e)
{
    // Switch to Custom colors tab
    colorUIControl1.SelectedColorGroup = ColorUISelectedGroup.CustomColors;
}
```

### Getting Current Active Group

**C#:**
```csharp
// Check which group is currently active
ColorUISelectedGroup currentGroup = colorUIControl1.SelectedColorGroup;

switch (currentGroup)
{
    case ColorUISelectedGroup.SystemColors:
        MessageBox.Show("System colors are selected");
        break;
    case ColorUISelectedGroup.StandardColors:
        MessageBox.Show("Standard colors are selected");
        break;
    case ColorUISelectedGroup.CustomColors:
        MessageBox.Show("Custom colors are selected");
        break;
    case ColorUISelectedGroup.UserColors:
        MessageBox.Show("User colors are selected");
        break;
    case ColorUISelectedGroup.None:
        MessageBox.Show("No group selected");
        break;
}
```

## Customizing User Color Cells

The UserColors group provides two collections that can be programmatically customized: `UserColors` and `UserCustomColors`.

### UserColors Collection

The main color collection in the UserColors group.

**C#:**
```csharp
// Customize UserColors with a blue gradient
for (int i = 0; i < this.colorUIControl1.UserColors.Count; i++)
{
    this.colorUIControl1.UserColors[i] = Color.FromArgb(0, 0, i * 5);
}

// Set UserColors group as active
this.colorUIControl1.SelectedColorGroup = ColorUISelectedGroup.UserColors;
```

**VB.NET:**
```vb
' Customize UserColors with a blue gradient
Dim i As Integer
For i = 0 To Me.colorUIControl1.UserColors.Count - 1
    Me.colorUIControl1.UserColors(i) = Color.FromArgb(0, 0, i * 5)
Next

' Set UserColors group as active
Me.colorUIControl1.SelectedColorGroup = ColorUISelectedGroup.UserColors
```

### UserCustomColors Collection

An additional collection for extended custom colors in the UserColors group.

**C#:**
```csharp
// Customize UserCustomColors with a red gradient
for (int i = 0; i < this.colorUIControl1.UserCustomColors.Count; i++)
{
    this.colorUIControl1.UserCustomColors[i] = Color.FromArgb(i * 15, 0, 0);
}
```

**VB.NET:**
```vb
' Customize UserCustomColors with a red gradient
Dim i As Integer
For i = 0 To Me.colorUIControl1.UserCustomColors.Count - 1
    Me.colorUIControl1.UserCustomColors(i) = Color.FromArgb(i * 15, 0, 0)
Next
```

### Enabling Stretching for User Color Cells

Use `UserColorsStretchOnResize` to allow the UserColors panel to resize with the control.

**C#:**
```csharp
// Enable stretching for better visual layout
this.colorUIControl1.UserColorsStretchOnResize = true;
```

**VB.NET:**
```vb
' Enable stretching for better visual layout
Me.colorUIControl1.UserColorsStretchOnResize = True
```

### Complete User Color Customization Example

**C#:**
```csharp
private void SetupUserColors()
{
    // Make sure UserColors group is visible
    colorUIControl1.ColorGroups = 
        ColorUIGroups.StandardColors | 
        ColorUIGroups.CustomColors | 
        ColorUIGroups.UserColors;
    
    // Customize UserColors with gradient (blue shades)
    for (int i = 0; i < colorUIControl1.UserColors.Count; i++)
    {
        int blueValue = Math.Min(255, i * 5);
        colorUIControl1.UserColors[i] = Color.FromArgb(0, 0, blueValue);
    }
    
    // Customize UserCustomColors with gradient (red shades)
    for (int i = 0; i < colorUIControl1.UserCustomColors.Count; i++)
    {
        int redValue = Math.Min(255, i * 15);
        colorUIControl1.UserCustomColors[i] = Color.FromArgb(redValue, 0, 0);
    }
    
    // Enable stretching
    colorUIControl1.UserColorsStretchOnResize = true;
    
    // Set as active group
    colorUIControl1.SelectedColorGroup = ColorUISelectedGroup.UserColors;
}
```

## Complete Examples

### Example 1: Simplified Color Picker (Standard Colors Only)

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;

public class SimplifiedColorPicker : Form
{
    private ColorUIControl colorUIControl1;
    
    public SimplifiedColorPicker()
    {
        InitializeComponent();
    }
    
    private void InitializeComponent()
    {
        colorUIControl1 = new ColorUIControl();
        
        // Show only Standard colors for simplicity
        colorUIControl1.ColorGroups = ColorUIGroups.StandardColors;
        
        // Set as active group
        colorUIControl1.SelectedColorGroup = ColorUISelectedGroup.StandardColors;
        
        // Configure appearance
        colorUIControl1.Size = new Size(210, 180);
        colorUIControl1.Location = new Point(20, 20);
        
        // Add to form
        this.Controls.Add(colorUIControl1);
        this.Text = "Simple Color Picker";
        this.Size = new Size(270, 250);
    }
}
```

### Example 2: Professional Color Picker (Standard and Custom)

**C#:**
```csharp
private void SetupProfessionalColorPicker()
{
    // Show Standard and Custom colors
    colorUIControl1.ColorGroups = 
        ColorUIGroups.StandardColors | 
        ColorUIGroups.CustomColors;
    
    // Start with Standard colors active
    colorUIControl1.SelectedColorGroup = ColorUISelectedGroup.StandardColors;
    
    // Enable stretching for custom colors
    colorUIControl1.CustomColorsStretchOnResize = true;
    
    // Set initial color
    colorUIControl1.SelectedColor = Color.SteelBlue;
}
```

### Example 3: Theme-Based Color Picker with User Colors

**C#:**
```csharp
private void SetupThemeColorPicker()
{
    // Include all groups except System colors
    colorUIControl1.ColorGroups = 
        ColorUIGroups.StandardColors | 
        ColorUIGroups.CustomColors | 
        ColorUIGroups.UserColors;
    
    // Populate UserColors with brand theme colors
    Color[] brandColors = new Color[]
    {
        Color.FromArgb(0, 120, 212),   // Primary Blue
        Color.FromArgb(0, 99, 177),    // Dark Blue
        Color.FromArgb(106, 185, 255), // Light Blue
        Color.FromArgb(255, 140, 0),   // Accent Orange
        Color.FromArgb(232, 17, 35),   // Alert Red
        Color.FromArgb(16, 124, 16),   // Success Green
    };
    
    for (int i = 0; i < brandColors.Length && i < colorUIControl1.UserColors.Count; i++)
    {
        colorUIControl1.UserColors[i] = brandColors[i];
    }
    
    // Set UserColors as default
    colorUIControl1.SelectedColorGroup = ColorUISelectedGroup.UserColors;
    colorUIControl1.UserColorsStretchOnResize = true;
}
```

### Example 4: Recently Used Colors Implementation

**C#:**
```csharp
private List<Color> recentColors = new List<Color>();
private const int MaxRecentColors = 20;

private void AddToRecentColors(Color color)
{
    // Remove if already exists
    recentColors.Remove(color);
    
    // Add to beginning
    recentColors.Insert(0, color);
    
    // Limit to max count
    if (recentColors.Count > MaxRecentColors)
    {
        recentColors.RemoveAt(recentColors.Count - 1);
    }
    
    // Update UserColors collection
    UpdateUserColorsWithRecent();
}

private void UpdateUserColorsWithRecent()
{
    for (int i = 0; i < colorUIControl1.UserColors.Count; i++)
    {
        if (i < recentColors.Count)
        {
            colorUIControl1.UserColors[i] = recentColors[i];
        }
        else
        {
            colorUIControl1.UserColors[i] = Color.White; // Empty slot
        }
    }
}

private void colorUIControl1_ColorSelected(object sender, EventArgs e)
{
    Color selected = colorUIControl1.SelectedColor;
    AddToRecentColors(selected);
    
    // Apply color to your application
    // ...
}
```

## Best Practices

1. **Choose Appropriate Groups for Your Use Case**
   - Simple applications: StandardColors only
   - System integration: SystemColors + StandardColors
   - Branded apps: StandardColors + UserColors with brand colors
   - Advanced apps: All groups with custom UserColors

2. **Set SelectedColorGroup to Match ColorGroups**
   ```csharp
   // If only showing StandardColors, set it as active
   colorUIControl1.ColorGroups = ColorUIGroups.StandardColors;
   colorUIControl1.SelectedColorGroup = ColorUISelectedGroup.StandardColors;
   ```

3. **Enable Stretching for Better UX**
   ```csharp
   colorUIControl1.CustomColorsStretchOnResize = true;
   colorUIControl1.UserColorsStretchOnResize = true;
   ```

4. **Always Check UserColors is Visible Before Customizing**
   ```csharp
   // Include UserColors in ColorGroups before customizing
   colorUIControl1.ColorGroups |= ColorUIGroups.UserColors;
   // Then customize UserColors collection
   ```

5. **Validate Collection Bounds**
   ```csharp
   for (int i = 0; i < colorUIControl1.UserColors.Count; i++)
   {
       // Safe: respects collection bounds
       colorUIControl1.UserColors[i] = myColors[i];
   }
   ```

## Troubleshooting

### Issue: UserColors Not Appearing

**Solution:** Ensure UserColors is included in ColorGroups:
```csharp
colorUIControl1.ColorGroups = 
    ColorUIGroups.StandardColors | 
    ColorUIGroups.UserColors; // Must include UserColors
```

### Issue: Custom User Colors Not Showing

**Solution:** Set SelectedColorGroup after customizing:
```csharp
// Customize first
for (int i = 0; i < colorUIControl1.UserColors.Count; i++)
{
    colorUIControl1.UserColors[i] = Color.FromArgb(0, 0, i * 5);
}
// Then select the group
colorUIControl1.SelectedColorGroup = ColorUISelectedGroup.UserColors;
```

### Issue: Color Group Tabs Not Visible

**Solution:** Check that ColorGroups property includes the desired groups:
```csharp
// Make sure to use bitwise OR to combine groups
colorUIControl1.ColorGroups = 
    ColorUIGroups.StandardColors | ColorUIGroups.CustomColors;
```

### Issue: Selected Color Not in Active Group

**Problem:** Setting a color that doesn't exist in the active group causes confusion.

**Solution:** Match the group to the color:
```csharp
// Set a system color and show System group
colorUIControl1.SelectedColor = SystemColors.Control;
colorUIControl1.SelectedColorGroup = ColorUISelectedGroup.SystemColors;
```

## Next Steps

- [Appearance Customization](appearance-customization.md) - Customize borders, tabs, and visual styling
- [Events](events.md) - Handle color selection events
- [Popup Integration](popup-integration.md) - Use ColorUIControl in popup menus
