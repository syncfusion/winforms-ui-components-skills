# Appearance Customization in Windows Forms CommandBar

## Table of Contents
- [Button Customization](#button-customization)
- [Gripper and Cursor](#gripper-and-cursor)
- [Background Colors](#background-colors)
- [Text and Font](#text-and-font)

## Button Customization

### Close button

The close button appears when CommandBar is in float state. Hide it using `HideCloseButton`:

```csharp
commandBar1.HideCloseButton = true;
```

To check the current state:

```csharp
bool isCloseBtnHidden = commandBar1.GetCloseButtonState();
```

### Chevron button

The chevron button displays toolbar icons that don't fit in available space. Control visibility:

```csharp
// Hide chevron
commandBar1.HideChevron = true;

// Check if visible
bool isChevronVisible = commandBar1.IsChevronVisible;
```

Customize chevron color:

```csharp
commandBar1.ChevronColor = Color.Red;
commandBar1.ChevronColor = Color.FromArgb(255, 100, 100);
```

### Drop down button

Hide the drop down button that appears in both dock and float states:

```csharp
commandBar1.HideDropDownButton = true;
```

Check the drop down button state:

```csharp
bool isDropDownVisible = commandBar1.GetDropDownState();
```

### Combined button control

```csharp
// Create toolbar with custom button configuration
CommandBar toolbar = new CommandBar();
toolbar.Text = "Custom Toolbar";

// Hide unnecessary buttons
toolbar.HideCloseButton = false;      // Show close in float mode
toolbar.HideChevron = false;          // Show overflow items
toolbar.HideDropDownButton = true;    // Hide dropdown
toolbar.HideGripper = false;          // Show drag handle

commandBarController1.CommandBars.Add(toolbar);
```

## Gripper and Cursor

### Gripper visibility

The gripper allows users to drag and reposition the bar. Hide it when you want to prevent moving:

```csharp
// Hide gripper (no dragging)
this.commandBar1.HideGripper = true;

// Show gripper (allow dragging)
this.commandBar1.HideGripper = false;
```

### Cursor customization

Change the mouse cursor when hovering over CommandBar bounds:

```csharp
// Set cursor to hand pointer
this.commandBar1.Cursor = System.Windows.Forms.Cursors.Hand;

// Set cursor to move
this.commandBar1.Cursor = System.Windows.Forms.Cursors.SizeAll;

// Set cursor to custom
Cursor customCursor = new Cursor(@"path\to\custom\cursor.cur");
this.commandBar1.Cursor = customCursor;
```

Reset cursor to default:

```csharp
this.commandBar1.ResetCursor();
```

### Cursor with gripper

```csharp
// Show draggable toolbar
commandBar1.HideGripper = false;
commandBar1.Cursor = System.Windows.Forms.Cursors.Hand;
```

## Background Colors

### Docked bar background

Customize the background of the entire docked area managed by CommandBarController:

```csharp
// Set docked area background
this.commandBarController1.BackColor = Color.Green;

// Reset to default
this.commandBarController1.ResetBackColor();
```

This affects the background behind all docked CommandBars.

### Individual CommandBar background

Customize a specific CommandBar's background:

```csharp
// Set CommandBar background
this.commandBar1.BackColor = Color.LightBlue;

// Reset to default
this.commandBar1.ResetBackColor();
```

Background color change triggers `BackColorChanged` event.

### Gradient background

For gradient effects, use panel containers:

```csharp
Panel gradientPanel = new Panel();
gradientPanel.Dock = DockStyle.Fill;
gradientPanel.BackColor = Color.White;

// In Paint event, draw gradient
gradientPanel.Paint += (s, e) =>
{
    Brush brush = new LinearGradientBrush(
        gradientPanel.ClientRectangle,
        Color.Blue,
        Color.White,
        LinearGradientMode.Horizontal);
    e.Graphics.FillRectangle(brush, gradientPanel.ClientRectangle);
};

commandBar1.Controls.Add(gradientPanel);
```

### Themed background

```csharp
// Match application theme
commandBar1.BackColor = SystemColors.Control;

// Custom corporate colors
commandBar1.BackColor = Color.FromArgb(50, 100, 150);
commandBarController1.BackColor = Color.FromArgb(240, 240, 240);
```

## Text and Font

### Show/hide dock state text

Control whether text appears when bar is docked:

```csharp
// Hide text when docked
this.commandBar1.ShowDockModeText = false;

// Show text when docked
this.commandBar1.ShowDockModeText = true;
```

In float mode, the text appears in the window caption regardless of this setting.

### Customize font

Change the font of all text in the CommandBar:

```csharp
// Set specific font
this.commandBar1.Font = new Font("AgencyFB", 12.0F, FontStyle.Bold);

// Or use system font
this.commandBar1.Font = new Font("Segoe UI", 10.0F);

// Reset to default
this.commandBar1.ResetFont();
```

### Font with text example

```csharp
CommandBar bar = new CommandBar();
bar.Text = "Formatting";
bar.Font = new Font("Arial", 11.0F, FontStyle.Bold);
bar.ShowDockModeText = true;
bar.BackColor = Color.WhiteSmoke;

commandBarController1.CommandBars.Add(bar);
```

### Complete appearance example

```csharp
// Create fully customized toolbar
CommandBar customBar = new CommandBar();
customBar.Text = "My Custom Toolbar";

// Appearance
customBar.BackColor = Color.LightCyan;
customBar.Font = new Font("Tahoma", 11.0F);
customBar.ShowDockModeText = true;

// Buttons
customBar.HideGripper = false;
customBar.HideChevron = false;
customBar.HideCloseButton = false;
customBar.HideDropDownButton = false;
customBar.ChevronColor = Color.DarkBlue;

// Interaction
customBar.Cursor = System.Windows.Forms.Cursors.Hand;

commandBarController1.CommandBars.Add(customBar);
```

## Troubleshooting

**Issue: Color changes not appearing**

Solution: Ensure the CommandBar is added to controller before changing properties:

```csharp
commandBar1 = new CommandBar();
commandBarController1.CommandBars.Add(commandBar1);  // Add first
commandBar1.BackColor = Color.Blue;                 // Then customize
```

**Issue: Font changes not reflected**

Solution: Set font before or after adding controls, not during addition:

```csharp
// Good: Set font before controls
commandBar1.Font = new Font("Arial", 12.0F);
commandBar1.Controls.Add(textBox1);

// Avoid: Changing font with many controls
```

**Issue: Button hiding not working**

Solution: Ensure properties are set before button interactions:

```csharp
// Set early in initialization
commandBar1.HideCloseButton = true;
commandBar1.HideChevron = false;
commandBar1.DockState = CommandBarDockState.Top;
```
