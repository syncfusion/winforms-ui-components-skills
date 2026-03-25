# Events and Event Handling

This guide covers event handling in ColorUIControl, focusing on the ColorSelected event and its common usage patterns.

## Overview

ColorUIControl provides the `ColorSelected` event which is raised whenever the user selects a color from any of the color groups. This event is essential for responding to user interactions and integrating the color picker into your application's workflow.

**Key Concepts:**
- **ColorSelected** event fires when a color is clicked/selected
- Access selected color via `SelectedColor` property
- Common use: Closing popups, updating UI, applying colors
- Event provides sender and EventArgs parameters

## ColorSelected Event

The primary event for responding to color selection by the user.

### Event Signature

```csharp
public event EventHandler ColorSelected;
```

### Subscribing to the Event

**C#:**
```csharp
// Subscribe using += operator
this.colorUIControl1.ColorSelected += ColorUIControl1_ColorSelected;

// Event handler method
private void ColorUIControl1_ColorSelected(object sender, EventArgs e)
{
    // Handle color selection
    Color selectedColor = this.colorUIControl1.SelectedColor;
    MessageBox.Show($"Selected: {selectedColor.Name}");
}
```

**VB.NET:**
```vb
' Subscribe using AddHandler
AddHandler Me.colorUIControl1.ColorSelected, AddressOf ColorUIControl1_ColorSelected

' Event handler method
Private Sub ColorUIControl1_ColorSelected(sender As Object, e As EventArgs)
    ' Handle color selection
    Dim selectedColor As Color = Me.colorUIControl1.SelectedColor
    MessageBox.Show($"Selected: {selectedColor.Name}")
End Sub
```

### Accessing Selected Color

**C#:**
```csharp
private void ColorUIControl1_ColorSelected(object sender, EventArgs e)
{
    // Method 1: Via control reference
    Color color1 = this.colorUIControl1.SelectedColor;
    
    // Method 2: Via sender parameter (more flexible)
    ColorUIControl control = sender as ColorUIControl;
    if (control != null)
    {
        Color color2 = control.SelectedColor;
    }
}
```

### Getting Color Information

**C#:**
```csharp
private void ColorUIControl1_ColorSelected(object sender, EventArgs e)
{
    Color selected = colorUIControl1.SelectedColor;
    
    // Get color properties
    string name = selected.Name;
    int alpha = selected.A;
    int red = selected.R;
    int green = selected.G;
    int blue = selected.B;
    
    // Format as hex
    string hex = $"#{selected.R:X2}{selected.G:X2}{selected.B:X2}";
    
    // Display information
    MessageBox.Show(
        $"Color: {name}\n" +
        $"RGB: ({red}, {green}, {blue})\n" +
        $"Hex: {hex}\n" +
        $"Alpha: {alpha}"
    );
}
```

## Common Event Patterns

### Pattern 1: Update UI Element

Apply the selected color to another control or UI element.

**C#:**
```csharp
private Panel colorPreview;

private void InitializeComponents()
{
    // Create preview panel
    colorPreview = new Panel();
    colorPreview.Size = new Size(100, 50);
    colorPreview.BorderStyle = BorderStyle.FixedSingle;
    this.Controls.Add(colorPreview);
    
    // Subscribe to event
    colorUIControl1.ColorSelected += ColorUIControl1_ColorSelected;
}

private void ColorUIControl1_ColorSelected(object sender, EventArgs e)
{
    // Update preview panel background
    colorPreview.BackColor = colorUIControl1.SelectedColor;
}
```

### Pattern 2: Close Popup After Selection

Close a PopupControlContainer when a color is selected.

**C#:**
```csharp
private void ColorUIControl_ColorSelected(object sender, EventArgs e)
{
    // Cast sender to ColorUIControl
    ColorUIControl cuiControl = sender as ColorUIControl;
    
    if (cuiControl != null)
    {
        // Get parent PopupControlContainer
        PopupControlContainer pcc = cuiControl.Parent as PopupControlContainer;
        
        if (pcc != null)
        {
            // Close the popup
            pcc.HidePopup(PopupCloseType.Done);
        }
    }
}
```

**VB.NET:**
```vb
Private Sub ColorUIControl_ColorSelected(sender As Object, e As EventArgs)
    ' Cast sender to ColorUIControl
    Dim cuiControl As ColorUIControl = TryCast(sender, ColorUIControl)
    
    If cuiControl IsNot Nothing Then
        ' Get parent PopupControlContainer
        Dim pcc As PopupControlContainer = TryCast(cuiControl.Parent, PopupControlContainer)
        
        If pcc IsNot Nothing Then
            ' Close the popup
            pcc.HidePopup(PopupCloseType.Done)
        End If
    End If
End Sub
```

### Pattern 3: Apply Color to Property

Update a property or object with the selected color.

**C#:**
```csharp
private DocumentSettings settings;

private void ColorUIControl1_ColorSelected(object sender, EventArgs e)
{
    // Apply to application settings
    settings.BackgroundColor = colorUIControl1.SelectedColor;
    
    // Notify of change
    OnSettingsChanged();
}
```

### Pattern 4: Validate and Apply

Validate color selection before applying.

**C#:**
```csharp
private void ColorUIControl1_ColorSelected(object sender, EventArgs e)
{
    Color selected = colorUIControl1.SelectedColor;
    
    // Validate color (e.g., not too dark)
    if (selected.GetBrightness() < 0.3f)
    {
        MessageBox.Show("Please select a lighter color", "Color Too Dark",
            MessageBoxButtons.OK, MessageBoxIcon.Warning);
        return;
    }
    
    // Apply validated color
    ApplyColor(selected);
}

private void ApplyColor(Color color)
{
    // Apply to UI elements
    this.BackColor = color;
}
```

### Pattern 5: Update Multiple Elements

Apply color to multiple UI elements simultaneously.

**C#:**
```csharp
private void ColorUIControl1_ColorSelected(object sender, EventArgs e)
{
    Color selected = colorUIControl1.SelectedColor;
    
    // Update multiple elements
    headerPanel.BackColor = selected;
    footerPanel.BackColor = selected;
    statusStrip.BackColor = selected;
    
    // Update text to contrasting color
    Color textColor = GetContrastingColor(selected);
    headerPanel.ForeColor = textColor;
    footerPanel.ForeColor = textColor;
}

private Color GetContrastingColor(Color background)
{
    // Simple brightness-based contrast
    return background.GetBrightness() > 0.5 ? Color.Black : Color.White;
}
```

### Pattern 6: Save to Recent Colors

Track recently selected colors for later use.

**C#:**
```csharp
private List<Color> recentColors = new List<Color>();
private const int MaxRecentColors = 10;

private void ColorUIControl1_ColorSelected(object sender, EventArgs e)
{
    Color selected = colorUIControl1.SelectedColor;
    
    // Add to recent colors
    AddToRecentColors(selected);
    
    // Apply color
    ApplySelectedColor(selected);
}

private void AddToRecentColors(Color color)
{
    // Remove if already exists
    if (recentColors.Contains(color))
    {
        recentColors.Remove(color);
    }
    
    // Add to beginning
    recentColors.Insert(0, color);
    
    // Limit list size
    if (recentColors.Count > MaxRecentColors)
    {
        recentColors.RemoveAt(recentColors.Count - 1);
    }
    
    // Update UserColors panel
    UpdateUserColorsPanel();
}

private void UpdateUserColorsPanel()
{
    for (int i = 0; i < colorUIControl1.UserColors.Count && i < recentColors.Count; i++)
    {
        colorUIControl1.UserColors[i] = recentColors[i];
    }
}
```

## Working with PopupControlContainer

When ColorUIControl is hosted in a PopupControlContainer (for dropdown functionality), handling the ColorSelected event properly is crucial for good UX.

### Complete Popup Integration Example

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;

public class ColorPickerPopupForm : Form
{
    private PopupMenu popupMenu1;
    private PopupControlContainer popupContainer1;
    private ColorUIControl colorUIControl1;
    private Panel colorButton;
    private Label colorLabel;
    
    public ColorPickerPopupForm()
    {
        InitializeComponents();
    }
    
    private void InitializeComponents()
    {
        // Create color display button
        colorButton = new Panel();
        colorButton.Size = new Size(100, 30);
        colorButton.Location = new Point(20, 20);
        colorButton.BorderStyle = BorderStyle.FixedSingle;
        colorButton.BackColor = Color.White;
        colorButton.Cursor = Cursors.Hand;
        colorButton.MouseUp += ColorButton_MouseUp;
        
        // Create label
        colorLabel = new Label();
        colorLabel.Text = "Click to select color";
        colorLabel.Location = new Point(20, 55);
        colorLabel.AutoSize = true;
        
        // Create ColorUIControl
        colorUIControl1 = new ColorUIControl();
        colorUIControl1.Size = new Size(210, 200);
        colorUIControl1.ColorGroups = 
            ColorUIGroups.StandardColors | ColorUIGroups.CustomColors;
        colorUIControl1.SelectedColorGroup = ColorUISelectedGroup.StandardColors;
        colorUIControl1.ColorSelected += ColorUIControl1_ColorSelected;
        
        // Create PopupControlContainer
        popupContainer1 = new PopupControlContainer();
        popupContainer1.Controls.Add(colorUIControl1);
        
        // Create PopupMenu
        popupMenu1 = new PopupMenu();
        DropDownBarItem dropDownItem = new DropDownBarItem();
        dropDownItem.PopupControlContainer = popupContainer1;
        popupMenu1.ParentBarItem.Items.Add(dropDownItem);
        
        // Add to form
        this.Controls.Add(colorButton);
        this.Controls.Add(colorLabel);
        
        this.Text = "Color Picker Popup";
        this.Size = new Size(250, 150);
    }
    
    private void ColorButton_MouseUp(object sender, MouseEventArgs e)
    {
        // Show popup at button location
        popupMenu1.Show(colorButton, new Point(0, colorButton.Height));
    }
    
    private void ColorUIControl1_ColorSelected(object sender, EventArgs e)
    {
        // Update button color
        colorButton.BackColor = colorUIControl1.SelectedColor;
        
        // Update label
        colorLabel.Text = $"Selected: {colorUIControl1.SelectedColor.Name}";
        
        // Close popup
        ColorUIControl cuiControl = sender as ColorUIControl;
        PopupControlContainer pcc = cuiControl.Parent as PopupControlContainer;
        pcc?.HidePopup(PopupCloseType.Done);
    }
}
```

### Popup Close Types

When closing a PopupControlContainer, you can specify the close type:

```csharp
public enum PopupCloseType
{
    Done,      // User completed action (selected color)
    Canceled,  // User canceled (Escape key, clicked outside)
    Deactivate // Popup lost focus
}
```

**Example:**
```csharp
// Close with Done status (successful selection)
pcc.HidePopup(PopupCloseType.Done);

// Close with Canceled status (user pressed Escape)
pcc.HidePopup(PopupCloseType.Canceled);
```

## Unsubscribing from Events

Always unsubscribe from events when disposing controls or changing subscriptions.

**C#:**
```csharp
// Unsubscribe
this.colorUIControl1.ColorSelected -= ColorUIControl1_ColorSelected;

// In Dispose method
protected override void Dispose(bool disposing)
{
    if (disposing)
    {
        // Unsubscribe before disposal
        if (colorUIControl1 != null)
        {
            colorUIControl1.ColorSelected -= ColorUIControl1_ColorSelected;
        }
    }
    base.Dispose(disposing);
}
```

**VB.NET:**
```vb
' Unsubscribe
RemoveHandler Me.colorUIControl1.ColorSelected, AddressOf ColorUIControl1_ColorSelected
```

## Complete Examples

### Example 1: Text Color Picker

**C#:**
```csharp
private RichTextBox richTextBox1;
private ColorUIControl colorUIControl1;

private void InitializeTextColorPicker()
{
    // Create RichTextBox
    richTextBox1 = new RichTextBox();
    richTextBox1.Size = new Size(400, 200);
    richTextBox1.Location = new Point(20, 20);
    richTextBox1.Text = "Select text and choose a color";
    
    // Create ColorUIControl
    colorUIControl1 = new ColorUIControl();
    colorUIControl1.Size = new Size(210, 200);
    colorUIControl1.Location = new Point(20, 230);
    colorUIControl1.ColorGroups = ColorUIGroups.StandardColors;
    colorUIControl1.SelectedColorGroup = ColorUISelectedGroup.StandardColors;
    colorUIControl1.ColorSelected += ApplyTextColor;
    
    // Add to form
    this.Controls.Add(richTextBox1);
    this.Controls.Add(colorUIControl1);
}

private void ApplyTextColor(object sender, EventArgs e)
{
    Color selected = colorUIControl1.SelectedColor;
    
    if (richTextBox1.SelectionLength > 0)
    {
        // Apply to selected text
        richTextBox1.SelectionColor = selected;
    }
    else
    {
        // Apply to all text
        richTextBox1.ForeColor = selected;
    }
    
    // Refocus on text box
    richTextBox1.Focus();
}
```

### Example 2: Theme Selector

**C#:**
```csharp
private void ColorUIControl1_ColorSelected(object sender, EventArgs e)
{
    Color primaryColor = colorUIControl1.SelectedColor;
    
    // Apply theme based on selected color
    ApplyThemeColors(primaryColor);
}

private void ApplyThemeColors(Color primary)
{
    // Calculate complementary colors
    Color lighter = ControlPaint.Light(primary);
    Color darker = ControlPaint.Dark(primary);
    
    // Apply to form
    this.BackColor = lighter;
    
    // Apply to panels
    foreach (Control control in this.Controls)
    {
        if (control is Panel)
        {
            control.BackColor = primary;
            control.ForeColor = GetContrastingColor(primary);
        }
    }
    
    // Apply to menu
    menuStrip1.BackColor = darker;
    menuStrip1.ForeColor = Color.White;
}

private Color GetContrastingColor(Color background)
{
    double brightness = (background.R * 299 + background.G * 587 + background.B * 114) / 1000;
    return brightness > 128 ? Color.Black : Color.White;
}
```

### Example 3: Drawing Application

**C#:**
```csharp
private PictureBox canvas;
private Color currentDrawColor = Color.Black;
private bool isDrawing = false;
private Point lastPoint;

private void InitializeDrawingApp()
{
    // Create canvas
    canvas = new PictureBox();
    canvas.Size = new Size(600, 400);
    canvas.Location = new Point(20, 20);
    canvas.BorderStyle = BorderStyle.FixedSingle;
    canvas.BackColor = Color.White;
    canvas.Image = new Bitmap(600, 400);
    canvas.MouseDown += Canvas_MouseDown;
    canvas.MouseMove += Canvas_MouseMove;
    canvas.MouseUp += Canvas_MouseUp;
    
    // Create color picker
    colorUIControl1 = new ColorUIControl();
    colorUIControl1.Size = new Size(210, 200);
    colorUIControl1.Location = new Point(20, 430);
    colorUIControl1.ColorSelected += ColorUIControl1_ColorSelected;
    
    this.Controls.Add(canvas);
    this.Controls.Add(colorUIControl1);
}

private void ColorUIControl1_ColorSelected(object sender, EventArgs e)
{
    // Update current drawing color
    currentDrawColor = colorUIControl1.SelectedColor;
}

private void Canvas_MouseDown(object sender, MouseEventArgs e)
{
    isDrawing = true;
    lastPoint = e.Location;
}

private void Canvas_MouseMove(object sender, MouseEventArgs e)
{
    if (isDrawing)
    {
        using (Graphics g = Graphics.FromImage(canvas.Image))
        using (Pen pen = new Pen(currentDrawColor, 2))
        {
            g.DrawLine(pen, lastPoint, e.Location);
        }
        canvas.Invalidate();
        lastPoint = e.Location;
    }
}

private void Canvas_MouseUp(object sender, MouseEventArgs e)
{
    isDrawing = false;
}
```

## Best Practices

1. **Always Access SelectedColor in the Event Handler**
   ```csharp
   // Correct: Color value is captured at selection time
   Color selected = colorUIControl1.SelectedColor;
   ```

2. **Close Popups Immediately After Selection**
   ```csharp
   // Good UX: Don't make users close popup manually
   pcc?.HidePopup(PopupCloseType.Done);
   ```

3. **Validate Color Selection When Necessary**
   ```csharp
   if (selected.GetBrightness() < threshold)
   {
       // Reject or warn
   }
   ```

4. **Unsubscribe in Dispose**
   ```csharp
   protected override void Dispose(bool disposing)
   {
       if (disposing && colorUIControl1 != null)
       {
           colorUIControl1.ColorSelected -= Handler;
       }
       base.Dispose(disposing);
   }
   ```

5. **Use Sender Parameter for Reusable Handlers**
   ```csharp
   private void SharedColorHandler(object sender, EventArgs e)
   {
       ColorUIControl control = sender as ColorUIControl;
       // Now works with multiple controls
   }
   ```

## Troubleshooting

### Issue: Event Not Firing

**Solution:** Verify event subscription:
```csharp
// Check that event is subscribed
colorUIControl1.ColorSelected += ColorUIControl1_ColorSelected;
```

### Issue: Cannot Access Parent Container

**Solution:** Ensure ColorUIControl is properly parented:
```csharp
// Add to PopupControlContainer first
popupContainer1.Controls.Add(colorUIControl1);
// Then access in event
PopupControlContainer pcc = colorUIControl1.Parent as PopupControlContainer;
```

### Issue: Selected Color is Empty

**Solution:** Check timing and initialization:
```csharp
private void ColorUIControl1_ColorSelected(object sender, EventArgs e)
{
    Color selected = colorUIControl1.SelectedColor;
    if (selected == Color.Empty || selected.IsEmpty)
    {
        // Handle empty color case
        return;
    }
    // Proceed with valid color
}
```

### Issue: Event Fires Multiple Times

**Solution:** Unsubscribe before resubscribing:
```csharp
// Prevent duplicate subscriptions
colorUIControl1.ColorSelected -= ColorUIControl1_ColorSelected;
colorUIControl1.ColorSelected += ColorUIControl1_ColorSelected;
```

## Next Steps

- [Getting Started](getting-started.md) - Setup and initialization
- [Color Groups](color-groups.md) - Configure color groups
- [Popup Integration](popup-integration.md) - Full popup implementation examples
