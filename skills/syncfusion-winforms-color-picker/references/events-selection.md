# Events and Runtime Selection

Complete guide to handling color selection events and runtime color dialog in ColorPickerUIAdv.

## Overview

ColorPickerUIAdv provides two primary events for color interaction:
- **Picked** - Raised when a color is selected (clicked)
- **ItemSelection** - Raised when mouse hovers over a color item

Additionally, the control supports runtime color selection through a "More Colors" dialog.

## Picked Event

The `Picked` event fires when a user clicks and selects a color from the control.

### Event Signature

```csharp
public event ColorPickerUIAdv.ColorPickedEventHandler Picked;
```

### ColorPickedEventArgs

The event handler receives a `ColorPickedEventArgs` argument with:

**Properties:**
- `Color` - The selected `System.Drawing.Color` structure

### Basic Usage

```csharp
// Attach event handler
colorPickerUIAdv1.Picked += ColorPickerUIAdv1_Picked;

// Event handler method
private void ColorPickerUIAdv1_Picked(object sender, 
                                       ColorPickerUIAdv.ColorPickedEventArgs e)
{
    // Access selected color
    Color selectedColor = e.Color;
    
    // Apply to form background
    this.BackColor = selectedColor;
    
    // Display color information
    MessageBox.Show($"Selected: {e.Color.Name}");
}
```

### Common Picked Event Patterns

#### Pattern 1: Multiple Target Updates

```csharp
private void ColorPickerUIAdv1_Picked(object sender, 
                                       ColorPickerUIAdv.ColorPickedEventArgs e)
{
    // Update multiple controls
    panel1.BackColor = e.Color;
    label1.ForeColor = e.Color;
    button1.BackColor = e.Color;
    
    // Log selection
    Console.WriteLine($"Color selected: {e.Color.Name} " +
                      $"(R:{e.Color.R}, G:{e.Color.G}, B:{e.Color.B})");
}
```

#### Pattern 2: Conditional Actions

```csharp
private void ColorPickerUIAdv1_Picked(object sender, 
                                       ColorPickerUIAdv.ColorPickedEventArgs e)
{
    // Different actions based on color brightness
    if (e.Color.GetBrightness() > 0.5)
    {
        // Light color - use dark text
        textBox.ForeColor = Color.Black;
    }
    else
    {
        // Dark color - use light text
        textBox.ForeColor = Color.White;
    }
    
    // Apply selected color as background
    textBox.BackColor = e.Color;
}
```

## ItemSelection Event

The `ItemSelection` event fires when the mouse hovers over a color item (without clicking).

### Event Signature

```csharp
public event ColorPickerUIAdv.ColorPickedEventHandler ItemSelection;
```

### Use Cases

- Live preview of color before selection
- Display color information on hover
- Temporary highlighting effects
- Color name tooltips

### Basic Usage

```csharp
// Attach event handler
colorPickerUIAdv1.ItemSelection += ColorPickerUIAdv1_ItemSelection;

// Event handler method
private void ColorPickerUIAdv1_ItemSelection(object sender, 
                                              ColorPickerUIAdv.ColorPickedEventArgs e)
{
    // Show preview without committing
    previewPanel.BackColor = e.Color;
    
    // Display color name
    statusLabel.Text = $"Preview: {e.Color.Name}";
}
```

### ItemSelection Patterns

#### Live Preview with Revert Option

```csharp
private Color originalColor;
private bool isPreviewMode = false;

private void ColorPickerUIAdv1_ItemSelection(object sender, 
                                              ColorPickerUIAdv.ColorPickedEventArgs e)
{
    if (!isPreviewMode)
    {
        // Save original color on first hover
        originalColor = targetPanel.BackColor;
        isPreviewMode = true;
    }
    
    // Apply preview
    targetPanel.BackColor = e.Color;
}

private void ColorPickerUIAdv1_Picked(object sender, 
                                       ColorPickerUIAdv.ColorPickedEventArgs e)
{
    // Commit the selection
    targetPanel.BackColor = e.Color;
    isPreviewMode = false;
}

private void CancelButton_Click(object sender, EventArgs e)
{
    // Revert to original color
    if (isPreviewMode)
    {
        targetPanel.BackColor = originalColor;
        isPreviewMode = false;
    }
}
```

## Combining Picked and ItemSelection Events

Use both events together for enhanced user experience.

### Complete Preview and Select Example

```csharp
public partial class ColorPickerForm : Form
{
    private ColorPickerUIAdv colorPicker;
    private Panel previewPanel;
    private Panel appliedPanel;
    private Label statusLabel;
    private Color currentColor = Color.White;
    
    public ColorPickerForm()
    {
        InitializeComponent();
        SetupColorPicker();
    }
    
    private void SetupColorPicker()
    {
        // Create color picker
        colorPicker = new ColorPickerUIAdv
        {
            Location = new Point(20, 20),
            Size = new Size(220, 200)
        };
        
        // Create preview panel (shows hover)
        previewPanel = new Panel
        {
            Location = new Point(260, 20),
            Size = new Size(150, 80),
            BorderStyle = BorderStyle.FixedSingle,
            BackColor = Color.White
        };
        
        // Create applied panel (shows selection)
        appliedPanel = new Panel
        {
            Location = new Point(260, 120),
            Size = new Size(150, 80),
            BorderStyle = BorderStyle.FixedSingle,
            BackColor = currentColor
        };
        
        // Status label
        statusLabel = new Label
        {
            Location = new Point(20, 230),
            Size = new Size(400, 40),
            Text = "Hover to preview, click to select"
        };
        
        // Attach events
        colorPicker.ItemSelection += ColorPicker_ItemSelection;
        colorPicker.Picked += ColorPicker_Picked;
        
        // Add to form
        this.Controls.AddRange(new Control[] 
        { 
            colorPicker, previewPanel, appliedPanel, statusLabel 
        });
    }
    
    private void ColorPicker_ItemSelection(object sender, 
                                            ColorPickerUIAdv.ColorPickedEventArgs e)
    {
        // Update preview panel
        previewPanel.BackColor = e.Color;
        statusLabel.Text = $"Preview: {e.Color.Name} - " +
                          $"RGB({e.Color.R}, {e.Color.G}, {e.Color.B})";
    }
    
    private void ColorPicker_Picked(object sender, 
                                     ColorPickerUIAdv.ColorPickedEventArgs e)
    {
        // Update applied panel
        appliedPanel.BackColor = e.Color;
        currentColor = e.Color;
        statusLabel.Text = $"Selected: {e.Color.Name}";
        
        // Additional action
        OnColorChanged(e.Color);
    }
    
    private void OnColorChanged(Color newColor)
    {
        // Notify other components, save to settings, etc.
        this.BackColor = Color.FromArgb(
            Math.Min(255, newColor.R + 200),
            Math.Min(255, newColor.G + 200),
            Math.Min(255, newColor.B + 200)
        );
    }
}
```

## Runtime Color Selection

ColorPickerUIAdv provides a "More Colors" option that opens a color dialog at runtime.

### Automatic Color Button

The `AutomaticColor` property sets the default color selected when the user clicks the "Automatic" button.

```csharp
// Set automatic color (default: Black)
colorPickerUIAdv1.AutomaticColor = Color.Black;

// Or custom automatic color
colorPickerUIAdv1.AutomaticColor = Color.White;
colorPickerUIAdv1.AutomaticColor = Color.FromArgb(240, 240, 240);
```

### ButtonHeight Property

Controls the height of the automatic color button.

```csharp
// Default button height
colorPickerUIAdv1.ButtonHeight = 23;

// Larger button
colorPickerUIAdv1.ButtonHeight = 30;

// Smaller button
colorPickerUIAdv1.ButtonHeight = 20;
```

### More Colors Dialog

When users click "More Colors", a standard Windows color dialog appears allowing:
- Custom color selection
- RGB/HSL editing
- Custom color definition
- Palette creation

The selected color from the dialog:
1. Triggers the `Picked` event
2. Updates `SelectedColor` property
3. Can be added to Recent Colors group

### Complete Runtime Selection Example

```csharp
private void SetupRuntimeSelection()
{
    // Configure automatic color
    colorPickerUIAdv1.AutomaticColor = Color.White;
    colorPickerUIAdv1.ButtonHeight = 28;
    
    // Handle selections (from both palette and dialog)
    colorPickerUIAdv1.Picked += (sender, e) =>
    {
        // Works for both palette selection and "More Colors" dialog
        this.BackColor = e.Color;
        
        // Log source
        if (colorPickerUIAdv1.RecentGroup.Items.Count > 0)
        {
            Console.WriteLine("Color selected from palette or dialog");
        }
    };
}
```

## Advanced Event Handling

### Event Handler Chaining

```csharp
private void SetupEventChain()
{
    // Multiple handlers for same event
    colorPickerUIAdv1.Picked += UpdateUI;
    colorPickerUIAdv1.Picked += SaveToSettings;
    colorPickerUIAdv1.Picked += LogColorChange;
}

private void UpdateUI(object sender, ColorPickerUIAdv.ColorPickedEventArgs e)
{
    panel1.BackColor = e.Color;
}

private void SaveToSettings(object sender, ColorPickerUIAdv.ColorPickedEventArgs e)
{
    Properties.Settings.Default.LastColor = e.Color;
    Properties.Settings.Default.Save();
}

private void LogColorChange(object sender, ColorPickerUIAdv.ColorPickedEventArgs e)
{
    string logEntry = $"{DateTime.Now}: Color changed to {e.Color.Name}";
    File.AppendAllText("color_log.txt", logEntry + Environment.NewLine);
}
```

### Event with Undo/Redo Support

```csharp
private Stack<Color> undoStack = new Stack<Color>();
private Stack<Color> redoStack = new Stack<Color>();

private void ColorPickerUIAdv1_Picked(object sender, 
                                       ColorPickerUIAdv.ColorPickedEventArgs e)
{
    // Save current state for undo
    undoStack.Push(currentColor);
    redoStack.Clear(); // Clear redo on new action
    
    // Apply new color
    currentColor = e.Color;
    ApplyColor(currentColor);
}

private void UndoButton_Click(object sender, EventArgs e)
{
    if (undoStack.Count > 0)
    {
        redoStack.Push(currentColor);
        currentColor = undoStack.Pop();
        ApplyColor(currentColor);
        colorPickerUIAdv1.SelectedColor = currentColor;
    }
}

private void RedoButton_Click(object sender, EventArgs e)
{
    if (redoStack.Count > 0)
    {
        undoStack.Push(currentColor);
        currentColor = redoStack.Pop();
        ApplyColor(currentColor);
        colorPickerUIAdv1.SelectedColor = currentColor;
    }
}

private void ApplyColor(Color color)
{
    targetPanel.BackColor = color;
}
```

## Best Practices

1. **Keep Handlers Lightweight:** Avoid heavy processing in event handlers
2. **Use Lambda for Simple Logic:** `colorPicker.Picked += (s, e) => panel.BackColor = e.Color;`
3. **Separate Concerns:** Different handlers for UI updates vs. data persistence
4. **Validate Colors:** Check color properties before applying (brightness, contrast)
5. **Debounce ItemSelection:** For expensive preview operations, limit update frequency
6. **Handle Errors:** Use try-catch blocks for color application operations

## Troubleshooting

- **Picked event not firing:** Ensure event handler is attached before user interaction
- **ItemSelection fires too frequently:** Implement debouncing in event handler
- **AutomaticColor not working:** Verify "Automatic" button is visible and property is set
- **Events firing with wrong color:** Use `e.Color`, not `SelectedColor` in ItemSelection
