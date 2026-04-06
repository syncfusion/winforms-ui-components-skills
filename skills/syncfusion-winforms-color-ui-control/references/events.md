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

### Accessing Selected Color

```csharp
private void ColorUIControl1_ColorSelected(object sender, EventArgs e)
{
    // Via control reference
    Color color = this.colorUIControl1.SelectedColor;
    
    // Get color properties
    string name = color.Name;
    string hex = $"#{color.R:X2}{color.G:X2}{color.B:X2}";
    int rgb = (color.R, color.G, color.B);
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

```csharp
private void ColorUIControl_ColorSelected(object sender, EventArgs e)
{
    ColorUIControl cuiControl = sender as ColorUIControl;
    PopupControlContainer pcc = cuiControl?.Parent as PopupControlContainer;
    pcc?.HidePopup(PopupCloseType.Done);
}ettingsChanged();
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

Apply color to Validate and Apply

```csharp
private void ColorUIControl1_ColorSelected(object sender, EventArgs e)
{
    Color selected = colorUIControl1.SelectedColor;
    
    // Validate color brightness
    if (selected.GetBrightness() < 0.3f)
    {
        MessageBox.Show("Please select a lighter color");
        return;
    }
    
    ApplyColor(selected);
}
```

### Pattern 4
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

## Working w4: Update Multiple Elements

```csharp
private void ColorUIControl1_ColorSelected(object sender, EventArgs e)
{
    Color selected = colorUIControl1.SelectedColor;
    
    // Update multiple elements
    headerPanel.BackColor = selected;
    footerPanel.BackColor = selected;
    
    // Update text to contrasting color
    Color textColor = selected.GetBrightness() > 0.5 ? Color.Black : Color.White;
    headerPanel.ForeColor = textColor;
    footerPanel.ForeColor = textColor;
}
```

### Pattern 5eate color display button
        colorButton = new Panel();
        colorButton.Size = new Size(100, 30);
        colorButton.Location = new Point(20, 20);
        colorButton.BorderStyle = BorderStyle.FixedSingle;
        colorButton.BackColor = Color.White;
        colorButton.Cursor = Cursors.Hand;
        colorButton.MouseUp += ColorButton_MouseUp;
        
        // Create label
        colo5: Save to Recent Colors

```csharp
private List<Color> recentColors = new List<Color>();

private void ColorUIControl1_ColorSelected(object sender, EventArgs e)
{
    Color selected = colorUIControl1.SelectedColor;
    
    // Remove if exists and add to front
    recentColors.Remove(selected);
    recentColors.Insert(0, selected);
    
    // Limit to 10 colors
    if (recentColors.Count > 10)
        recentColors.RemoveAt(10);   ColorUIControl cuiControl = sender as ColorUIControl;
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

### Popup Integration Example

```csharp
private PopupControlContainer popupContainer1;
private ColorUIControl colorUIControl1;
private Panel colorButton;

private void InitializeComponents()
{
    // Create color button
    colorButton = new Panel();
    colorButton.Size = new Size(100, 30);
    colorButton.BackColor = Color.White;
    colorButton.MouseUp += (s, e) => ShowColorPopup();
    
    // Create ColorUIControl
    colorUIControl1 = new ColorUIControl();
    colorUIControl1.ColorSelected += ColorUIControl1_ColorSelected;
    
    // Create PopupControlContainer
    popupContainer1 = new PopupControlContainer();
    popupContainer1.Controls.Add(colorUIControl1);
    
    this.Controls.Add(colorButton);
}

private void ColorUIControl1_ColorSelected(object sender, EventArgs e)
{
    colorButton.BackColor = colorUIControl1.SelectedColor;
    
    // Close popup
    PopupControlContainer pcc = (sender as ColorUIControl)?.Parent as PopupControlContainer;
    pcc?.HidePopup(PopupCloseType.Done);
}

## Real-World Examples

### Example 1: Text Editor Color

```csharp
private void ApplyTextColor(object sender, EventArgs e)
{
    Color selected = colorUIControl1.SelectedColor;
    
    if (richTextBox1.SelectionLength > 0)
        richTextBox1.SelectionColor = selected;
    else
        richTextBox1.ForeColor = selected;
    
    richTextBox1.Focus();
}
```

### Example 2: Drawing Application

```csharp
private Color currentDrawColor = Color.Black;

private void ColorUIControl1_ColorSelected(object sender, EventArgs e)
{
    currentDrawColor = colorUIControl1.SelectedColor;
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
    }Event Handler** - Capture color value at selection time
2. **Close Popups Immediately** - Use `pcc?.HidePopup(PopupCloseType.Done)` for better UX
3. **Validate When Necessary** - Check brightness or other criteria before applying
4. **Unsubscribe in Dispose** - Prevent memory leaks by unsubscribing from events
5. **Use Sender Parameter** - Makes handlers reusable across multiple controls**Event Not Firing** - Verify event subscription with `+=` operator

**Cannot Access Parent** - Ensure control is added to PopupControlContainer first

**Empty Color** - Check if `SelectedColor == Color.Empty` before use

**Multiple Fires** - Unsubscribe with `-=` before resubscribing to prevent duplicates