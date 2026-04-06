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

Available `GradientStyle` options:
- `Horizontal` - Left to right
- `Vertical` - Top to bottom
- `ForwardDiagonal` / `BackwardDiagonal` - Diagonal gradients
- `PathGradient` - Center radiates outward

```csharp
// Example: Vertical gradient button
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

Available options: `Stretch`, `Tile`, `Center`, `None`, `Zoom`

```csharp
sfButton1.Text = "Custom Background";
sfButton1.BackgroundImage = Image.FromFile(@"bg.jpg");
sfButton1.BackgroundImageLayout = ImageLayout.Stretch;
sfButton1.ForeColor = Color.White;
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
// Configure all button states
sfButton1.Text = "Interactive Button";
sfButton1.Size = new Size(150, 50);

// Normal state
sfButton1.Style.BackColor = Color.CornflowerBlue;
sfButton1.Style.ForeColor = Color.White;

// Hover state
sfButton1.Style.HoverBackColor = Color.RoyalBlue;
sfButton1.Style.HoverForeColor = Color.White;

// Pressed state
sfButton1.Style.PressedBackColor = Color.MidnightBlue;
sfButton1.Style.PressedForeColor = Color.Yellow;

// Focused state
sfButton1.Style.FocusedBackColor = Color.DodgerBlue;
sfButton1.Style.FocusedForeColor = Color.White;

// Disabled state
sfButton1.Style.DisabledBackColor = Color.LightGray;
sfButton1.Style.DisabledForeColor = Color.DarkGray;
```

---

## Image States

### Changing Images by State

```csharp
sfButton1.Text = "Save";
sfButton1.TextImageRelation = TextImageRelation.ImageBeforeText;
sfButton1.ImageSize = new Size(24, 24);

// Set different images for button states
sfButton1.Style.Image = Image.FromFile(@"normal.png");
sfButton1.Style.HoverImage = Image.FromFile(@"hover.png");
sfButton1.Style.PressedImage = Image.FromFile(@"pressed.png");
sfButton1.Style.FocusedImage = Image.FromFile(@"focused.png");
sfButton1.Style.DisabledImage = Image.FromFile(@"disabled.png");
```

**Note:** Animated GIF images only work in the normal state (`Image` property).

---

## Border Customization

### Border by State

```csharp
// Configure borders for different states
sfButton1.Style.Border = new Pen(Color.Blue, 2);
sfButton1.Style.HoverBorder = new Pen(Color.DarkBlue, 2);
sfButton1.Style.PressedBorder = new Pen(Color.Navy, 3);
sfButton1.Style.FocusedBorder = new Pen(Color.LightBlue, 2);
sfButton1.Style.DisabledBorder = new Pen(Color.LightGray, 1);

// Create custom pen styles (dashed, dotted)
Pen dashedPen = new Pen(Color.Red, 1) { 
    DashStyle = System.Drawing.Drawing2D.DashStyle.Dash 
};

// Remove borders by setting to null
sfButton1.Style.Border = null;
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
Display a dotted frame when the button has keyboard focus:

```csharp
sfButton1.Text = "Focused Button";
sfButton1.FocusRectangleVisible = true;  // Shows when Tab navigates or clickedton1.Invalidate(); };
    
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

```csharp
sfButton1.Text = "Loading";
sfButton1.AllowImageAnimation = true;
sfButton1.Style.Image = Image.FromFile(@"spinner.gif");
sfButton1.ImageSize = new Size(32, 32);
sfButton1.TextImageRelation = TextImageRelation.ImageBeforeText;
```

**Important:** Animated GIFs only work in the normal state (`Style.Image`). Use static images for `HoverImage`, `PressedImage`, etc.

---

## Complete Styling Example

```csharp
private void SetupStyledButton()
{
    sfButton1.Text = "Professional Button";
    sfButton1.Size = new Size(180, 50);
    sfButton1.FocusRectangleVisible = true;
    
    // Configure all states with colors and borders
    sfButton1.Style.BackColor = Color.CornflowerBlue;
    sfButton1.Style.ForeColor = Color.White;
    sfButton1.Style.Border = new Pen(Color.DarkBlue, 2);
    
    sfButton1.Style.HoverBackColor = Color.RoyalBlue;
    sfButton1.Style.HoverBorder = new Pen(Color.MidnightBlue, 2);
    
    sfButton1.Style.PressedBackColor = Color.MidnightBlue;
    sfButton1.Style.PressedForeColor = Color.Yellow;
    
    sfButton1.Style.DisabledBackColor = Color.LightGray;
    sfButton1.Style.DisabledForeColor = Color.DarkGray;
}
```