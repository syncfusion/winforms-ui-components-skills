# UI Appearance Properties

## Overview

The ColorPickerButton control provides multiple properties to control its visual appearance and behavior in your Windows Forms application. This reference covers button sizing, positioning, styling, and interaction properties.

## Button Properties

### Text Property

Displays the label on the button face.

```csharp
// Set button label
colorPickerButton1.Text = "Select a Color";

// Update label
colorPickerButton1.Text = "Theme Color Picker";

// Empty label (icon only)
colorPickerButton1.Text = "";
```

### Size and Layout

#### Location Property

Sets the position of the button on the form.

```csharp
// Set position using Point
colorPickerButton1.Location = new System.Drawing.Point(20, 20);

// Set individual coordinates
colorPickerButton1.Left = 20;
colorPickerButton1.Top = 20;

// Get current position
int x = colorPickerButton1.Left;
int y = colorPickerButton1.Top;
```

#### Size Property

Sets the width and height of the button.

```csharp
// Set size using Size object
colorPickerButton1.Size = new System.Drawing.Size(150, 32);

// Set individual dimensions
colorPickerButton1.Width = 150;
colorPickerButton1.Height = 32;

// Get size
int width = colorPickerButton1.Width;
int height = colorPickerButton1.Height;
```

#### Common Size Patterns

```csharp
// Standard button (toolbar compatible)
colorPickerButton1.Size = new System.Drawing.Size(100, 28);

// Large button (touchscreen friendly)
colorPickerButton1.Size = new System.Drawing.Size(150, 40);

// Compact (32x32 for toolbar)
colorPickerButton1.Size = new System.Drawing.Size(32, 32);

// Wide input field
colorPickerButton1.Size = new System.Drawing.Size(250, 24);

// Full width
colorPickerButton1.Size = new System.Drawing.Size(300, 32);
```

### Dock and Anchor

#### Dock Property

Automatically resizes and positions the button to fill a form edge.

```csharp
// Fill entire form
colorPickerButton1.Dock = DockStyle.Fill;

// Top edge
colorPickerButton1.Dock = DockStyle.Top;

// Bottom edge
colorPickerButton1.Dock = DockStyle.Bottom;

// Left edge
colorPickerButton1.Dock = DockStyle.Left;

// Right edge
colorPickerButton1.Dock = DockStyle.Right;

// No docking (default)
colorPickerButton1.Dock = DockStyle.None;
```

#### Anchor Property

Sets which edges maintain fixed distance from form edges when resizing.

```csharp
// Anchor to top-left (default)
colorPickerButton1.Anchor = AnchorStyles.Top | AnchorStyles.Left;

// Anchor to top and stretch horizontally
colorPickerButton1.Anchor = AnchorStyles.Top | AnchorStyles.Left | AnchorStyles.Right;

// Anchor to all edges (stretch with form)
colorPickerButton1.Anchor = AnchorStyles.Top | AnchorStyles.Bottom | 
                             AnchorStyles.Left | AnchorStyles.Right;

// Anchor to right side
colorPickerButton1.Anchor = AnchorStyles.Top | AnchorStyles.Right;
```

## Appearance Properties

### Font Property

Customizes the button text font.

```csharp
// Set font
colorPickerButton1.Font = new System.Drawing.Font("Arial", 10);

// Set font with style
colorPickerButton1.Font = new System.Drawing.Font("Arial", 10, FontStyle.Bold);
colorPickerButton1.Font = new System.Drawing.Font("Arial", 10, FontStyle.Italic);

// Common fonts
colorPickerButton1.Font = new System.Drawing.Font("Segoe UI", 10);        // Modern
colorPickerButton1.Font = new System.Drawing.Font("Courier New", 10);    // Monospace
colorPickerButton1.Font = new System.Drawing.Font("Tahoma", 9);          // Compact
```

### ForeColor Property

Sets the button text color.

```csharp
// Named colors
colorPickerButton1.ForeColor = System.Drawing.Color.Black;
colorPickerButton1.ForeColor = System.Drawing.Color.White;
colorPickerButton1.ForeColor = System.Drawing.Color.DarkBlue;

// RGB colors
colorPickerButton1.ForeColor = System.Drawing.Color.FromArgb(64, 64, 64);

// Hex colors
colorPickerButton1.ForeColor = System.Drawing.ColorTranslator.FromHtml("#333333");
```

### BackColor Property

Sets the button background color (when not showing selected color).

```csharp
// System default
colorPickerButton1.BackColor = System.Drawing.SystemColors.Control;

// Custom color
colorPickerButton1.BackColor = System.Drawing.Color.LightGray;

// When SelectedAsBackcolor is true, this is overridden
colorPickerButton1.SelectedAsBackcolor = true;
// Now shows the SelectedColor instead
```

### FlatStyle Property

Controls the button's 3D appearance.

```csharp
// Flat style (modern, no 3D effect)
colorPickerButton1.FlatStyle = FlatStyle.Flat;

// Standard 3D style
colorPickerButton1.FlatStyle = FlatStyle.Standard;

// Popup style (looks raised until clicked)
colorPickerButton1.FlatStyle = FlatStyle.Popup;

// System style (uses OS theme)
colorPickerButton1.FlatStyle = FlatStyle.System;
```

### Image Property

Displays an image on the button.

```csharp
// Load image from file
colorPickerButton1.Image = System.Drawing.Image.FromFile("icon.png");

// From resource
colorPickerButton1.Image = Resources.ColorPickerIcon;

// Clear image
colorPickerButton1.Image = null;
```

### ImageAlign Property

Positions the image on the button.

```csharp
colorPickerButton1.ImageAlign = ContentAlignment.MiddleLeft;
colorPickerButton1.ImageAlign = ContentAlignment.MiddleCenter;
colorPickerButton1.ImageAlign = ContentAlignment.MiddleRight;
colorPickerButton1.ImageAlign = ContentAlignment.TopCenter;
colorPickerButton1.ImageAlign = ContentAlignment.BottomCenter;
```

### TextAlign Property

Positions the button text.

```csharp
colorPickerButton1.TextAlign = ContentAlignment.MiddleLeft;
colorPickerButton1.TextAlign = ContentAlignment.MiddleCenter;
colorPickerButton1.TextAlign = ContentAlignment.MiddleRight;
colorPickerButton1.TextAlign = ContentAlignment.TopCenter;
colorPickerButton1.TextAlign = ContentAlignment.BottomCenter;
```

## Visibility and State Properties

### Visible Property

Shows or hides the button.

```csharp
// Show button
colorPickerButton1.Visible = true;

// Hide button
colorPickerButton1.Visible = false;
```

### Enabled Property

Enables or disables user interaction.

```csharp
// Enable (default)
colorPickerButton1.Enabled = true;

// Disable (grayed out)
colorPickerButton1.Enabled = false;

// Conditional enable
if (userHasPermission)
    colorPickerButton1.Enabled = true;
else
    colorPickerButton1.Enabled = false;
```

### Name Property

Identifies the control programmatically.

```csharp
// Set name (usually in designer)
colorPickerButton1.Name = "colorPickerButton1";

// Use in code
if (colorPickerButton1.Name == "colorPickerButton1")
{
    // Do something
}
```

### Tag Property

Stores custom data associated with the button.

```csharp
// Store user data
colorPickerButton1.Tag = "ThemeColor";
colorPickerButton1.Tag = new { Category = "UI", Purpose = "Theme" };

// Retrieve data
string purpose = colorPickerButton1.Tag?.ToString();
```

## Event Properties

### Click Event

Fires when the button is clicked.

```csharp
// Subscribe to click event
colorPickerButton1.Click += ColorPickerButton1_Click;

// Handle click
private void ColorPickerButton1_Click(object sender, EventArgs e)
{
    MessageBox.Show("Color picker opened");
    System.Drawing.Color selected = colorPickerButton1.SelectedColor;
}
```

### MouseEnter Event

Fires when mouse enters the button area.

```csharp
colorPickerButton1.MouseEnter += ColorPickerButton1_MouseEnter;

private void ColorPickerButton1_MouseEnter(object sender, EventArgs e)
{
    // Show tooltip or highlight
}
```

### MouseLeave Event

Fires when mouse leaves the button area.

```csharp
colorPickerButton1.MouseLeave += ColorPickerButton1_MouseLeave;

private void ColorPickerButton1_MouseLeave(object sender, EventArgs e)
{
    // Hide tooltip or unhighlight
}
```

## Complete Example

```csharp
public partial class UIPropertiesForm : Form
{
    public UIPropertiesForm()
    {
        InitializeComponent();
        ConfigureColorPickerUI();
    }

    private void ConfigureColorPickerUI()
    {
        // Position and size
        colorPickerButton1.Location = new System.Drawing.Point(20, 20);
        colorPickerButton1.Size = new System.Drawing.Size(200, 40);
        
        // Text
        colorPickerButton1.Text = "Pick Theme Color";
        colorPickerButton1.TextAlign = ContentAlignment.MiddleCenter;
        
        // Font and colors
        colorPickerButton1.Font = new System.Drawing.Font("Segoe UI", 10, FontStyle.Bold);
        colorPickerButton1.ForeColor = System.Drawing.Color.White;
        
        // Appearance
        colorPickerButton1.FlatStyle = FlatStyle.Flat;
        colorPickerButton1.SelectedAsBackcolor = true;
        colorPickerButton1.SelectedAsText = false;
        
        // Initial color
        colorPickerButton1.SelectedColor = System.Drawing.Color.Blue;
        
        // Layout
        colorPickerButton1.Anchor = AnchorStyles.Top | AnchorStyles.Left | AnchorStyles.Right;
        
        // Events
        colorPickerButton1.Click += ColorPickerButton1_Click;
        colorPickerButton1.MouseEnter += ColorPickerButton1_MouseEnter;
    }

    private void ColorPickerButton1_Click(object sender, EventArgs e)
    {
        MessageBox.Show($"Selected: {colorPickerButton1.SelectedColor.Name}");
    }

    private void ColorPickerButton1_MouseEnter(object sender, EventArgs e)
    {
        colorPickerButton1.Cursor = Cursors.Hand;
    }
}
```

## Property Combinations

### Theme Selector Style

```csharp
colorPickerButton1.Size = new System.Drawing.Size(200, 50);
colorPickerButton1.SelectedAsBackcolor = true;
colorPickerButton1.FlatStyle = FlatStyle.Flat;
colorPickerButton1.Font = new System.Drawing.Font("Segoe UI", 12, FontStyle.Bold);
colorPickerButton1.TextAlign = ContentAlignment.MiddleCenter;
```

### Toolbar Button Style

```csharp
colorPickerButton1.Size = new System.Drawing.Size(32, 32);
colorPickerButton1.SelectedAsBackcolor = true;
colorPickerButton1.SelectedAsText = false;
colorPickerButton1.Text = "";
colorPickerButton1.FlatStyle = FlatStyle.Flat;
colorPickerButton1.ForeColor = System.Drawing.Color.DarkGray;
```

### Form Input Style

```csharp
colorPickerButton1.Size = new System.Drawing.Size(300, 28);
colorPickerButton1.SelectedAsBackcolor = false;
colorPickerButton1.SelectedAsText = true;
colorPickerButton1.FlatStyle = FlatStyle.Standard;
colorPickerButton1.Font = new System.Drawing.Font("Arial", 10);
colorPickerButton1.ForeColor = System.Drawing.Color.Black;
```

## Troubleshooting

### Button text not displaying

- Check `Text` property is not empty
- Verify `TextAlign` is set correctly
- Ensure `SelectedAsText` is not overriding text

### Button looks disabled

- Check `Enabled` property is true
- Verify `Visible` property is true
- Check `ForeColor` has sufficient contrast with `BackColor`

### Size not changing

- Verify `Dock` is set to `DockStyle.None`
- Check parent container layout
- Ensure no layout manager is overriding size
