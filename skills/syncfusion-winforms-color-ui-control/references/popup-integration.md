# Popup Menu Integration

This guide covers how to integrate ColorUIControl into popup menus and create dropdown color picker functionality using PopupMenu and PopupControlContainer.

## Table of Contents
- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Basic Popup Setup](#basic-popup-setup)
- [Step-by-Step Implementation](#step-by-step-implementation)
- [Designer-Based Approach](#designer-based-approach)
- [Code-Based Approach](#code-based-approach)
- [Showing and Hiding Popups](#showing-and-hiding-popups)
- [Complete Working Examples](#complete-working-examples)
- [ColorPickerButton Alternative](#colorpickerbutton-alternative)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

ColorUIControl can be embedded in popup menus to create dropdown color pickers, similar to Microsoft Office applications. This approach provides:

- **Space-efficient UI** - Color picker appears only when needed
- **Context-appropriate** - Shows near the triggering control
- **Familiar UX** - Matches standard dropdown behavior
- **Professional appearance** - Integrates seamlessly with toolbars and menus

**Required Syncfusion Components:**
- **ColorUIControl** - The color picker interface
- **PopupControlContainer** - Container that hosts the ColorUIControl
- **PopupMenu** - Menu system that displays the container
- **DropDownBarItem** - Menu item that triggers the popup

## Prerequisites

Ensure you have the required assemblies referenced:

- `Syncfusion.Shared.Base.dll`
- `Syncfusion.Tools.Windows.dll` (for PopupMenu)

**Namespaces:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;
```

## Basic Popup Setup

The basic structure requires three Syncfusion components working together:

```
Panel/Button (Trigger)
    ↓ MouseUp event
PopupMenu.Show()
    ↓ Contains
DropDownBarItem
    ↓ References
PopupControlContainer
    ↓ Hosts
ColorUIControl
    ↓ ColorSelected event
Close Popup & Apply Color
```

## Step-by-Step Implementation

**C#:**
```csharp
// 1. Create controls
private ColorUIControl colorUIControl1 = new ColorUIControl();
private PopupControlContainer popupContainer1 = new PopupControlContainer();
private PopupMenu popupMenu1 = new PopupMenu();
private Panel colorButton = new Panel();

// 2. Configure ColorUIControl and add to container
colorUIControl1.Size = new Size(210, 200);
colorUIControl1.ColorGroups = ColorUIGroups.StandardColors | ColorUIGroups.CustomColors;
popupContainer1.Controls.Add(colorUIControl1);

// 3. Configure PopupMenu
DropDownBarItem dropDownItem = new DropDownBarItem();
dropDownItem.PopupControlContainer = popupContainer1;
popupMenu1.ParentBarItem.Items.Add(dropDownItem);

// 4. Setup trigger button
colorButton.Size = new Size(100, 30);
colorButton.BackColor = Color.White;
colorButton.Cursor = Cursors.Hand;
colorButton.MouseUp += (sender, e) => 
    popupMenu1.Show(colorButton, new Point(e.X, e.Y));

// 5. Handle color selection
colorUIControl1.ColorSelected += (sender, e) =>
{
    colorButton.BackColor = colorUIControl1.SelectedColor;
    ((sender as ColorUIControl)?.Parent as PopupControlContainer)
        ?.HidePopup(PopupCloseType.Done);
};
```

## Designer-Based Approach

1. Add `ColorUIControl`, `PopupMenu`, `PopupControlContainer`, and `Panel` to your form
2. Place ColorUIControl inside PopupControlContainer (cut/paste in designer)
3. In PopupMenu properties, add a `DropDownBarItem` and set its `PopupControlContainer` property
4. Configure Panel: BorderStyle=FixedSingle, Cursor=Hand

**Event handlers:**
```csharp
private void panel1_MouseUp(object sender, MouseEventArgs e)
{
    this.popupMenu1.Show(this.panel1, new Point(e.X, e.Y));
}

private void colorUIControl1_ColorSelected(object sender, EventArgs e)
{
    panel1.BackColor = colorUIControl1.SelectedColor;
    ((sender as ColorUIControl)?.Parent as PopupControlContainer)
        ?.HidePopup(PopupCloseType.Done);
}
```

## Code-Based Approach

Complete programmatic implementation without using the designer.

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class PopupColorPickerForm : Form
{
    private ColorUIControl colorUIControl1;
    private PopupControlContainer popupContainer1;
    private PopupMenu popupMenu1;
    private Panel colorButton;
    private Label statusLabel;
    
    public PopupColorPickerForm()
    {
        InitializeComponents();
    }
    
    private void InitializeComponents()
    {
        // Create ColorUIControl
        colorUIControl1 = new ColorUIControl();
        colorUIControl1.Size = new Size(210, 200);
        colorUIControl1.ColorGroups = 
            ColorUIGroups.StandardColors | ColorUIGroups.CustomColors;
        colorUIControl1.SelectedColorGroup = ColorUISelectedGroup.StandardColors;
        colorUIControl1.BorderStyle = BorderStyle.None;
        colorUIControl1.ColorSelected += ColorUIControl1_ColorSelected;
        
        // Create PopupControlContainer
        popupContainer1 = new PopupControlContainer();
        popupContainer1.Controls.Add(colorUIControl1);
        
        // Create PopupMenu
        popupMenu1 = new PopupMenu();
        
        // Create and configure DropDownBarItem
        DropDownBarItem dropDownItem = new DropDownBarItem();
        dropDownItem.PopupControlContainer = popupContainer1;
        
        // Add DropDownBarItem to PopupMenu
        popupMenu1.ParentBarItem.Items.Add(dropDownItem);
        
        // Create color button (trigger)
        colorButton = new Panel();
        colorButton.Size = new Size(120, 35);
        colorButton.Location = new Point(20, 20);
        colorButton.BorderStyle = BorderStyle.FixedSingle;
        colorButton.BackColor = Color.White;
        colorButton.Cursor = Cursors.Hand;
        colorButton.MouseUp += ColorButton_MouseUp;
        
        // Create status label
        statusLabel = new Label();
        statusLabel.Text = "Click the box to select a color";
        statusLabel.Location = new Point(20, 60);
        statusLabel.AutoSize = true;
        
        // Add controls to form
        this.Controls.Add(colorButton);
        this.Controls.Add(statusLabel);
        
        // Form properties
        this.Text = "Popup Color Picker";
        this.Size = new Size(300, 150);
        this.StartPosition = FormStartPosition.CenterScreen;
    }
    
    private void ColorButton_MouseUp(object sender, MouseEventArgs e)
    {
        // Show popup below the button
        popupMenu1.Show(colorButton, new Point(0, colorButton.Height));
    }
    
    private void ColorUIControl1_ColorSelected(object sender, EventArgs e)
    {
        // Update button color
        colorButton.BackColor = colorUIControl1.SelectedColor;
        
        // Update status label
        Color selected = colorUIControl1.SelectedColor;
        statusLabel.Text = $"Selected: {selected.Name} (R:{selected.R}, G:{selected.G}, B:{selected.B})";
        
        // Close the popup
        ColorUIControl control = sender as ColorUIControl;
        PopupControlContainer container = control?.Parent as PopupControlContainer;
        container?.HidePopup(PopupCloseType.Done);
    }
}
```

## Showing and Hiding Popups

### Showing the Popup

Use the `Show` method of PopupMenu:

**C#:**
```csharp
// Show at specific coordinates relative to control
popupMenu1.Show(parentControl, new Point(x, y));

// Show below a button
popupMenu1.Show(button, new Point(0, button.Height));

// Show at mouse cursor position
popupMenu1.Show(button, new Point(e.X, e.Y));

// Show at screen coordinates
popupMenu1.Show(button, button.PointToClient(Cursor.Position));
```

### Hiding the Popup

Use the `HidePopup` method of PopupControlContainer:

**C#:**
```csharp
// Hide with "Done" status (user completed action)
popupContainer1.HidePopup(PopupCloseType.Done);

// Hide with "Canceled" status
popupContainer1.HidePopup(PopupCloseType.Canceled);

// Hide on deactivation
popupContainer1.HidePopup(PopupCloseType.Deactivate);
```

### Popup Positioning Strategies

**Below trigger control:**
```csharp
popupMenu1.Show(trigger, new Point(0, trigger.Height));
```

**Above trigger control:**
```csharp
popupMenu1.Show(trigger, new Point(0, -popupContainer1.Height));
```

**To the right of trigger:**
```csharp
popupMenu1.Show(trigger, new Point(trigger.Width, 0));
```

**Centered below trigger:**
```csharp
int centerX = (trigger.Width - popupContainer1.Width) / 2;
popupMenu1.Show(trigger, new Point(centerX, trigger.Height));
```

## Complete Working Examples

### Example 1: Toolbar Color Picker Button

**C#:**
```csharp
public class ToolbarColorPickerForm : Form
{
    private ToolStrip toolStrip1;
    private ToolStripButton colorButton;
    private Panel colorPreview;
    private ColorUIControl colorUIControl1;
    private PopupControlContainer popupContainer1;
    private PopupMenu popupMenu1;
    
    public ToolbarColorPickerForm()
    {
        InitializeToolbar();
        InitializePopupColorPicker();
    }
    
    private void InitializeToolbar()
    {
        // Create toolbar
        toolStrip1 = new ToolStrip();
        toolStrip1.Items.Add(new ToolStripButton("Bold", null));
        toolStrip1.Items.Add(new ToolStripButton("Italic", null));
        toolStrip1.Items.Add(new ToolStripSeparator());
        
        // Create color preview panel
        colorPreview = new Panel();
        colorPreview.Size = new Size(30, 20);
        colorPreview.BackColor = Color.Black;
        colorPreview.BorderStyle = BorderStyle.FixedSingle;
        colorPreview.Click += ColorPreview_Click;
        
        // Add to toolbar as host
        ToolStripControlHost host = new ToolStripControlHost(colorPreview);
        host.ToolTipText = "Text Color";
        toolStrip1.Items.Add(host);
        
        this.Controls.Add(toolStrip1);
    }
    
    private void InitializePopupColorPicker()
    {
        // Create ColorUIControl
        colorUIControl1 = new ColorUIControl();
        colorUIControl1.Size = new Size(210, 200);
        colorUIControl1.ColorGroups = ColorUIGroups.StandardColors;
        colorUIControl1.ColorSelected += ColorSelected;
        
        // Create container and menu
        popupContainer1 = new PopupControlContainer();
        popupContainer1.Controls.Add(colorUIControl1);
        
        popupMenu1 = new PopupMenu();
        DropDownBarItem item = new DropDownBarItem();
        item.PopupControlContainer = popupContainer1;
        popupMenu1.ParentBarItem.Items.Add(item);
    }
    
    private void ColorPreview_Click(object sender, EventArgs e)
    {
        // Show popup below the preview
        Point screenPoint = colorPreview.PointToScreen(new Point(0, colorPreview.Height));
        Point clientPoint = this.PointToClient(screenPoint);
        popupMenu1.Show(this, clientPoint);
    }
    
    private void ColorSelected(object sender, EventArgs e)
    {
        // Update preview
        colorPreview.BackColor = colorUIControl1.SelectedColor;
        
        // Close popup
        ((sender as ColorUIControl)?.Parent as PopupControlContainer)
            ?.HidePopup(PopupCloseType.Done);
    }
}
```

## ColorPickerButton Alternative

For simple dropdown color picker functionality, consider using **ColorPickerButton** instead of manually creating the popup infrastructure.

**C#:**
```csharp
using Syncfusion.Windows.Forms.Tools;

// ColorPickerButton provides built-in popup functionality
ColorPickerButton colorPickerButton1 = new ColorPickerButton();
colorPickerButton1.Size = new Size(100, 25);
colorPickerButton1.SelectedColor = Color.Black;
colorPickerButton1.ColorSelected += (s, e) =>
{
    Color selected = colorPickerButton1.SelectedColor;
    // Apply color
};
```

**When to use ColorPickerButton vs Manual Popup:**
- **Use ColorPickerButton**: Simple dropdown, standard behavior, minimal customization
- **Use Manual Popup**: Full control over appearance, positioning, and behavior

## Best Practices

### 1. Always Close Popup After Selection

```csharp
private void ColorSelected(object sender, EventArgs e)
{
    // Apply color first
    ApplyColor(colorUIControl1.SelectedColor);
    
    // Then close popup
    (sender as ColorUIControl)?.Parent as PopupControlContainer)
        ?.HidePopup(PopupCloseType.Done);
}
```

```csharp
// Remove border when using in popup for cleaner appearance
colorUIControl1.BorderStyle = BorderStyle.None;
```

## Troubleshooting

### Issue: Popup Not Appearing

**Solution:** Verify all components are properly connected:
```csharp
// Check connections
Debug.Assert(popupContainer1.Controls.Contains(colorUIControl1));
Debug.Assert(dropDownItem.PopupControlContainer == popupContainer1);
Debug.Assert(popupMenu1.ParentBarItem.Items.Contains(dropDownItem));
```

### Issue: Popup Appears at Wrong Location

**Solution:** Use correct coordinate system:
```csharp
// Coordinates are relative to parent control
popupMenu1.Show(parentControl, new Point(x, y));
```

### Issue: Popup Doesn't Close After Selection

**Solution:** Ensure ColorSelected event is properly handled:
```csharp
colorUIControl1.ColorSelected += (s, e) =>
{
    var container = (s as ColorUIControl)?.Parent as PopupControlContainer;
    container?.HidePopup(PopupCloseType.Done);
};
```

### Issue: Can't Access PopupControlContainer from Event

**Solution:** Check parent chain:
```csharp
ColorUIControl control = sender as ColorUIControl;
if (control?.Parent is PopupControlContainer container)
{
    container.HidePopup(PopupCloseType.Done);
}
```

### Issue: Multiple Popups Interfere

**Solution:** Use separate PopupMenu instances:
```csharp
// Create separate popup infrastructure for each trigger
private void CreateColorPicker(Panel trigger, Action<Color> onColorSelected)
{
    var colorUI = new ColorUIControl();
    var container = new PopupControlContainer();
    var menu = new PopupMenu();
    // ... setup
}
```

## Next Steps

- [Getting Started](getting-started.md) - Basic setup and initialization
- [Color Groups](color-groups.md) - Configure available colors
- [Events](events.md) - Handle ColorSelected event patterns
- [Appearance Customization](appearance-customization.md) - Visual styling options
