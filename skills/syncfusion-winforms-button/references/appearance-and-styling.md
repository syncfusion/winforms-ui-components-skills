# Appearance & Styling in SfButton

## Table of Contents

- [Background Colors](#background-colors)
- [Gradient Backgrounds](#gradient-backgrounds)
- [Background Images](#background-images)
- [State-Based Customization](#state-based-customization)
- [Image States](#image-states)
- [Border Customization](#border-customization)
- [Focus Rectangle](#focus-rectangle)
- [Rounded Rectangle Buttons](#rounded-rectangle-buttons)
- [Animated GIF Images](#animated-gif-images)

---

## Background Colors

### Basic BackColor

Set a solid background color for the button:

```csharp
// Set gray background
sfButton1.BackColor = Color.Gray;

// Set light blue background
sfButton1.BackColor = Color.LightBlue;

// Set custom color
sfButton1.BackColor = Color.FromArgb(100, 150, 200);
```

### Foreground (Text) Color

Control text color separately from background:

```csharp
// Set white text on dark background
sfButton1.BackColor = Color.DarkBlue;
sfButton1.ForeColor = Color.White;

// Set dark text on light background
sfButton1.BackColor = Color.LightGray;
sfButton1.ForeColor = Color.Black;
```

---

## Gradient Backgrounds

### Using GradientBrush

Create gradient backgrounds for professional appearance:

```csharp
// Linear gradient from Green to Yellow
sfButton1.Style.GradientBrush = new BrushInfo(
    GradientStyle.ForwardDiagonal,
    Color.Green,
    Color.Yellow
);
```

### Gradient Styles

```csharp
// Horizontal gradient (left to right)
sfButton1.Style.GradientBrush = new BrushInfo(
    GradientStyle.Horizontal,
    Color.Blue,
    Color.Cyan
);

// Vertical gradient (top to bottom)
sfButton1.Style.GradientBrush = new BrushInfo(
    GradientStyle.Vertical,
    Color.Red,
    Color.Yellow
);

// Forward diagonal gradient
sfButton1.Style.GradientBrush = new BrushInfo(
    GradientStyle.ForwardDiagonal,
    Color.DarkBlue,
    Color.LightBlue
);

// Backward diagonal gradient
sfButton1.Style.GradientBrush = new BrushInfo(
    GradientStyle.BackwardDiagonal,
    Color.Purple,
    Color.Pink
);

// Path gradient (center radiates outward)
sfButton1.Style.GradientBrush = new BrushInfo(
    GradientStyle.PathGradient,
    Color.White,
    Color.Gray
);
```

### Complete Gradient Example

```csharp
// Create a professional blue gradient button
sfButton1.Text = "Gradient Button";
sfButton1.Style.ForeColor = Color.White;
sfButton1.Style.GradientBrush = new BrushInfo(
    GradientStyle.Vertical,
    Color.DodgerBlue,
    Color.LightBlue
);
sfButton1.Size = new Size(150, 50);
```

---

## Background Images

### Setting Background Image

Fill the button background with an image:

```csharp
// Initialize the background image
this.sfButton1.BackgroundImage = Image.FromFile(@"path\to\background.png");

// Set the layout style
this.sfButton1.BackgroundImageLayout = ImageLayout.Center;
```

### ImageLayout Options

Control how the background image is displayed:

```csharp
// Stretch image to fill entire button
sfButton1.BackgroundImageLayout = ImageLayout.Stretch;

// Tile image to fill button
sfButton1.BackgroundImageLayout = ImageLayout.Tile;

// Center image in button (may not cover entire button)
sfButton1.BackgroundImageLayout = ImageLayout.Center;

// Display image at original size (may be clipped)
sfButton1.BackgroundImageLayout = ImageLayout.None;

// Zoom to fill button (maintains aspect ratio)
sfButton1.BackgroundImageLayout = ImageLayout.Zoom;
```

### Complete Example

```csharp
sfButton1.Text = "Custom Background";
sfButton1.BackgroundImage = Image.FromFile(@"bg.jpg");
sfButton1.BackgroundImageLayout = ImageLayout.Stretch;
sfButton1.ForeColor = Color.White;  // White text for contrast
sfButton1.Size = new Size(200, 100);
```

---

## State-Based Customization

### Overview

SfButton allows customizing appearance for different button states:
- **Normal** - Default state
- **Hover** - Mouse over button
- **Pressed** - Mouse button held down
- **Focused** - Keyboard focus
- **Disabled** - Button disabled

### BackColor and ForeColor by State

```csharp
// Normal state (default)
sfButton1.Style.BackColor = Color.LightGray;
sfButton1.Style.ForeColor = Color.Black;

// Hover state (mouse over)
sfButton1.Style.HoverBackColor = Color.Gray;
sfButton1.Style.HoverForeColor = Color.White;

// Pressed state (mouse clicked)
sfButton1.Style.PressedBackColor = Color.DarkGray;
sfButton1.Style.PressedForeColor = Color.White;

// Focused state (keyboard focus)
sfButton1.Style.FocusedBackColor = Color.LightGray;
sfButton1.Style.FocusedForeColor = Color.Black;

// Disabled state (button.Enabled = false)
sfButton1.Style.DisabledBackColor = Color.White;
sfButton1.Style.DisabledForeColor = Color.Gray;
```

### Complete State Example

```csharp
// Create button with all states customized
sfButton1.Text = "Interactive Button";
sfButton1.Size = new Size(150, 50);

// Normal state (blue)
sfButton1.Style.BackColor = Color.CornflowerBlue;
sfButton1.Style.ForeColor = Color.White;

// Hover state (darker blue)
sfButton1.Style.HoverBackColor = Color.RoyalBlue;
sfButton1.Style.HoverForeColor = Color.White;

// Pressed state (darkest blue)
sfButton1.Style.PressedBackColor = Color.MidnightBlue;
sfButton1.Style.PressedForeColor = Color.Yellow;

// Focused state (bright blue)
sfButton1.Style.FocusedBackColor = Color.DodgerBlue;
sfButton1.Style.FocusedForeColor = Color.White;

// Disabled state (grayed out)
sfButton1.Style.DisabledBackColor = Color.LightGray;
sfButton1.Style.DisabledForeColor = Color.DarkGray;
```

---

## Image States

### Changing Images by State

Display different images for each button state:

```csharp
// Normal state image
sfButton1.Style.Image = Image.FromFile(@"normal.png");

// Hover state image
sfButton1.Style.HoverImage = Image.FromFile(@"hover.png");

// Pressed state image
sfButton1.Style.PressedImage = Image.FromFile(@"pressed.png");

// Focused state image
sfButton1.Style.FocusedImage = Image.FromFile(@"focused.png");

// Disabled state image
sfButton1.Style.DisabledImage = Image.FromFile(@"disabled.png");
```

### Image State Example

```csharp
sfButton1.Text = "Save";
sfButton1.TextImageRelation = TextImageRelation.ImageBeforeText;
sfButton1.ImageSize = new Size(24, 24);

// Different icon for each state
sfButton1.Style.Image = Image.FromFile(@"save-normal.png");
sfButton1.Style.HoverImage = Image.FromFile(@"save-hover.png");
sfButton1.Style.PressedImage = Image.FromFile(@"save-pressed.png");
```

### Important Note

When using animated GIF images, only set the `Image` property (normal state). Animated images don't work in other states.

---

## Border Customization

### Border by State

Customize button borders for each state:

```csharp
// Normal state border (solid 2px blue)
sfButton1.Style.Border = new Pen(Color.Blue, 2);

// Hover state border
sfButton1.Style.HoverBorder = new Pen(Color.DarkBlue, 2);

// Pressed state border
sfButton1.Style.PressedBorder = new Pen(Color.Navy, 3);

// Focused state border
sfButton1.Style.FocusedBorder = new Pen(Color.LightBlue, 2);

// Disabled state border
sfButton1.Style.DisabledBorder = new Pen(Color.LightGray, 1);
```

### Creating Pen Objects

```csharp
// Solid pen with color and width
Pen bluePen = new Pen(Color.Blue, 2);

// Dashed pen
Pen dashedPen = new Pen(Color.Red, 1) { DashStyle = System.Drawing.Drawing2D.DashStyle.Dash };

// Dotted pen
Pen dottedPen = new Pen(Color.Green, 1) { DashStyle = System.Drawing.Drawing2D.DashStyle.Dot };

// Apply to button
sfButton1.Style.Border = bluePen;
sfButton1.Style.HoverBorder = dashedPen;
```

### No Border

Remove borders entirely:

```csharp
// Remove all borders
sfButton1.Style.Border = null;
sfButton1.Style.HoverBorder = null;
sfButton1.Style.FocusedBorder = null;
sfButton1.Style.PressedBorder = null;
sfButton1.Style.DisabledBorder = null;
```

---

## Focus Rectangle

### Showing Focus Rectangle

Show a dotted rectangular frame inside the button when it has keyboard focus:

```csharp
// Enable the focus rectangle
sfButton1.FocusRectangleVisible = true;

// Disable the focus rectangle (default)
sfButton1.FocusRectangleVisible = false;
```

### When Focus Rectangle Appears

The dotted frame appears when:
- Tab key navigates to the button
- Button is clicked and receives focus
- Focus rectangle is enabled

### Example

```csharp
sfButton1.Text = "Focused Button";
sfButton1.FocusRectangleVisible = true;  // Show focus indicator
sfButton1.Size = new Size(120, 40);
```

---

## Rounded Rectangle Buttons

### Implementation Overview

Create buttons with rounded corners using the Paint event:

1. Set button region to rounded rectangle
2. Draw border with rounded corners
3. Handle paint events for state-based drawing

### Complete Rounded Rectangle Implementation

```csharp
private bool isHovered = false;
private bool isPressed = false;

public Form1()
{
    InitializeComponent();
    
    sfButton1 = new SfButton();
    sfButton1.Text = "Rounded Button";
    sfButton1.Paint += SfButton1_Paint;
    sfButton1.MouseDown += (s, e) => { isPressed = true; sfButton1.Invalidate(); };
    sfButton1.MouseUp += (s, e) => { isPressed = false; sfButton1.Invalidate(); };
    sfButton1.MouseEnter += (s, e) => { isHovered = true; sfButton1.Invalidate(); };
    sfButton1.MouseLeave += (s, e) => { isHovered = false; sfButton1.Invalidate(); };
    
    this.Controls.Add(sfButton1);
}

private void SfButton1_Paint(object sender, PaintEventArgs e)
{
    // Set corner radius (must be less than 10)
    int radius = 5;
    e.Graphics.SmoothingMode = System.Drawing.Drawing2D.SmoothingMode.AntiAlias;
    
    // Calculate rectangle for rounded shape
    Rectangle rect = new Rectangle(
        this.sfButton1.ClientRectangle.X + 1,
        this.sfButton1.ClientRectangle.Y + 1,
        this.sfButton1.ClientRectangle.Width - 2,
        this.sfButton1.ClientRectangle.Height - 2
    );
    
    // Set button region to rounded rectangle
    sfButton1.Region = new Region(GetRoundedRect(rect, radius));
    
    // Adjust rectangle for border
    rect = new Rectangle(rect.X + 1, rect.Y + 1, rect.Width - 2, rect.Height - 2);

    // Determine border color based on state
    Pen borderPen;
    if (!sfButton1.Enabled)
        borderPen = new Pen(sfButton1.Style.DisabledBorder?.Color ?? Color.LightGray);
    else if (isPressed)
        borderPen = new Pen(sfButton1.Style.PressedBorder?.Color ?? Color.DarkBlue);
    else if (isHovered)
        borderPen = new Pen(sfButton1.Style.HoverBorder?.Color ?? Color.Blue);
    else
        borderPen = new Pen(sfButton1.Style.Border?.Color ?? Color.Gray);

    // Draw the border
    e.Graphics.DrawPath(borderPen, GetRoundedRect(rect, radius));
}

private GraphicsPath GetRoundedRect(Rectangle rect, int radius)
{
    var graphicsPath = new GraphicsPath();

    // Top-left corner
    graphicsPath.AddArc(rect.X, rect.Y, radius * 2, radius * 2, 180, 90);

    // Top edge
    graphicsPath.AddLine(rect.X + radius, rect.Y, rect.Right - radius, rect.Y);

    // Top-right corner
    graphicsPath.AddArc(rect.Right - radius * 2, rect.Y, radius * 2, radius * 2, 270, 90);

    // Right edge
    graphicsPath.AddLine(rect.Right, rect.Y + radius, rect.Right, rect.Bottom - radius);

    // Bottom-right corner
    graphicsPath.AddArc(rect.Right - radius * 2, rect.Bottom - radius * 2, 
        radius * 2, radius * 2, 0, 90);

    // Bottom edge
    graphicsPath.AddLine(rect.Right - radius, rect.Bottom, rect.X + radius, rect.Bottom);

    // Bottom-left corner
    graphicsPath.AddArc(rect.X, rect.Bottom - radius * 2, 
        radius * 2, radius * 2, 90, 90);

    // Left edge
    graphicsPath.AddLine(rect.X, rect.Bottom - radius, rect.X, rect.Y + radius);

    graphicsPath.CloseFigure();
    return graphicsPath;
}
```

### Important Note

When using Paint event for rounded rectangle customization:
- Border customization properties (Border, HoverBorder, etc.) don't work
- State-based drawing is handled within the Paint method
- Provide visual feedback through color changes

---

## Animated GIF Images

### Displaying Animated GIFs

Show animated GIF images in buttons:

```csharp
// Enable image animation
sfButton1.AllowImageAnimation = true;

// Set the animated GIF as the button image
sfButton1.Style.Image = Image.FromFile(@"path\to\animation.gif");

// Optionally set text
sfButton1.Text = "Processing...";
```

### Complete Example

```csharp
sfButton1.Text = "Loading";
sfButton1.AllowImageAnimation = true;
sfButton1.Style.Image = Image.FromFile(@"spinner.gif");
sfButton1.ImageSize = new Size(32, 32);
sfButton1.TextImageRelation = TextImageRelation.ImageBeforeText;
```

### Critical Constraint

**Important:** Animated images only work in the normal state:
- Set GIF using `Style.Image` property
- Do NOT set GIF in `HoverImage`, `PressedImage`, or `FocusedImage`
- Animation won't display in other states

### Valid Animation Setup

```csharp
// ✓ CORRECT: Only in normal state
sfButton1.AllowImageAnimation = true;
sfButton1.Style.Image = Image.FromFile(@"animate.gif");

// Use static images for other states
sfButton1.Style.HoverImage = Image.FromFile(@"static-hover.png");
sfButton1.Style.PressedImage = Image.FromFile(@"static-pressed.png");
```

### Invalid Animation Setup

```csharp
// ✗ INCORRECT: Won't animate
sfButton1.AllowImageAnimation = true;
sfButton1.Style.HoverImage = Image.FromFile(@"animate.gif");  // No animation in hover state
```

---

## Complete Styling Example

```csharp
private void SetupStyledButton()
{
    sfButton1.Text = "Professional Button";
    sfButton1.Size = new Size(180, 50);
    
    // Normal state: Blue gradient with white text
    sfButton1.Style.BackColor = Color.CornflowerBlue;
    sfButton1.Style.ForeColor = Color.White;
    sfButton1.Style.Border = new Pen(Color.DarkBlue, 2);
    
    // Hover state: Darker gradient, subtle color shift
    sfButton1.Style.HoverBackColor = Color.RoyalBlue;
    sfButton1.Style.HoverForeColor = Color.White;
    sfButton1.Style.HoverBorder = new Pen(Color.MidnightBlue, 2);
    
    // Pressed state: Almost black, yellow accent text
    sfButton1.Style.PressedBackColor = Color.MidnightBlue;
    sfButton1.Style.PressedForeColor = Color.Yellow;
    sfButton1.Style.PressedBorder = new Pen(Color.Navy, 3);
    
    // Focused state: Same as normal but slightly lighter
    sfButton1.Style.FocusedBackColor = Color.DodgerBlue;
    sfButton1.Style.FocusedForeColor = Color.White;
    sfButton1.Style.FocusedBorder = new Pen(Color.Blue, 2);
    
    // Disabled state: Grayed out
    sfButton1.Style.DisabledBackColor = Color.LightGray;
    sfButton1.Style.DisabledForeColor = Color.DarkGray;
    sfButton1.Style.DisabledBorder = new Pen(Color.Gray, 1);
    
    // Show focus rectangle
    sfButton1.FocusRectangleVisible = true;
}
```
