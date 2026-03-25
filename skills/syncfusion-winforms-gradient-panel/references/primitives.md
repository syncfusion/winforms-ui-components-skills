# Primitives

Comprehensive guide to using primitives - the unique feature of GradientPanelExt that allows hosting text, images, collapse controls, and even .NET controls in panel borders.

## Table of Contents
- [Primitives Overview](#primitives-overview)
- [CollapsePrimitive](#collapseprimitive)
- [ImagePrimitive](#imageprimitive)
- [TextPrimitive](#textprimitive)
- [HostPrimitive](#hostprimitive)
- [Common Properties](#common-properties)
- [PrimitiveCollection Editor](#primitivecollection-editor)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

---

## Primitives Overview

**Primitives** are UI elements that can be placed in the borders (Top, Bottom, Left, Right) of a GradientPanelExt. They don't occupy space inside the panel - they're positioned in the border areas.

### Types of Primitives

| Primitive | Description | Use Case |
|-----------|-------------|----------|
| **CollapsePrimitive** | Expand/collapse button with images | Collapsible sections |
| **ImagePrimitive** | Static image in border | Logos, icons, decorations |
| **TextPrimitive** | Text label in border | Titles, captions, buttons |
| **HostPrimitive** | Hosts any .NET control | Buttons, progress bars, custom controls |

### Key Concept

Primitives are **border-hosted** - they sit in the panel's edge areas (top, bottom, left, right margins), not in the main content area where child controls go.

---

## CollapsePrimitive

Provides expand/collapse functionality with clickable images.

**Namespace:** `Syncfusion.Windows.Forms.Tools.CollapsePrimitive`

### Key Properties

| Property | Type | Description |
|----------|------|-------------|
| **CollapseImage** | Image | Image shown when panel is expanded (click to collapse) |
| **ExpandImage** | Image | Image shown when panel is collapsed (click to expand) |
| **Alignment** | Alignment enum | Border position (Top, Bottom, Left, Right) |
| **Position** | int | Pixel offset along the border |
| **Size** | Size | Width and height of the image button |

### Basic Collapse Primitive

**C# Example:**
```csharp
// Create collapse primitive
CollapsePrimitive collapseButton = new CollapsePrimitive
{
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top,
    Position = 350,  // 350 pixels from left on top border
    Size = new Size(30, 30),
    BackColor = Color.Transparent
};

// Set images (from resources or file)
collapseButton.CollapseImage = Properties.Resources.CollapseIcon;  // Down arrow
collapseButton.ExpandImage = Properties.Resources.ExpandIcon;      // Up arrow

// Add to panel
gradientPanel.Primitives.Add(collapseButton);
```

**VB.NET Example:**
```vb
' Create collapse primitive
Dim collapseButton As New CollapsePrimitive With {
    .Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top,
    .Position = 350,
    .Size = New Size(30, 30),
    .BackColor = Color.Transparent
}

' Set images
collapseButton.CollapseImage = My.Resources.CollapseIcon
collapseButton.ExpandImage = My.Resources.ExpandIcon

' Add to panel
gradientPanel.Primitives.Add(collapseButton)
```

### Behavior

- **Click**: Toggles between collapsed and expanded states
- **Collapsed**: Panel height reduces, content hidden
- **Expanded**: Panel returns to full height
- **Image**: Automatically switches between CollapseImage and ExpandImage

---

## ImagePrimitive

Displays static images in panel borders.

**Namespace:** `Syncfusion.Windows.Forms.Tools.ImagePrimitive`

### Key Properties

| Property | Type | Description |
|----------|------|-------------|
| **Image** | Image | The image to display |
| **Alignment** | Alignment enum | Border position |
| **Position** | int | Pixel offset along border |
| **Size** | Size | Image dimensions |
| **PrimitiveBorderStyle** | PrimitiveBorderStyle | Border style for the image |

### Basic Image Primitive

**C# Example:**
```csharp
// Create image primitive
ImagePrimitive logoPrimitive = new ImagePrimitive
{
    Image = Properties.Resources.CompanyLogo,
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top,
    Position = 10,  // 10 pixels from left
    Size = new Size(40, 40),
    PrimitiveBorderStyle = PrimitiveBorderStyle.None
};

gradientPanel.Primitives.Add(logoPrimitive);
```

**VB.NET Example:**
```vb
Dim logoPrimitive As New ImagePrimitive With {
    .Image = My.Resources.CompanyLogo,
    .Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top,
    .Position = 10,
    .Size = New Size(40, 40),
    .PrimitiveBorderStyle = PrimitiveBorderStyle.None
}

gradientPanel.Primitives.Add(logoPrimitive)
```

### Multiple Images in Borders

**C# Example:**
```csharp
// Top-left icon
ImagePrimitive icon1 = new ImagePrimitive
{
    Image = Properties.Resources.Icon1,
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top,
    Position = 5,
    Size = new Size(20, 20)
};

// Top-right icon
ImagePrimitive icon2 = new ImagePrimitive
{
    Image = Properties.Resources.Icon2,
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top,
    Position = 350,
    Size = new Size(20, 20)
};

// Bottom-left icon
ImagePrimitive icon3 = new ImagePrimitive
{
    Image = Properties.Resources.Icon3,
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Bottom,
    Position = 5,
    Size = new Size(20, 20)
};

gradientPanel.Primitives.AddRange(new Primitive[] { icon1, icon2, icon3 });
```

---

## TextPrimitive

Displays text labels in panel borders. Can be styled like buttons.

**Namespace:** `Syncfusion.Windows.Forms.Tools.TextPrimitive`

### Key Properties

| Property | Type | Description |
|----------|------|-------------|
| **Text** | string | Text to display |
| **TextFont** | Font | Font for the text |
| **TextColor** | Color | Text color |
| **BackColor** | Color | Background color of primitive |
| **Alignment** | Alignment enum | Border position |
| **Position** | int | Pixel offset |
| **Size** | Size | Primitive dimensions |
| **BorderColor** | Color | Border color around primitive |

### Basic Text Primitive

**C# Example:**
```csharp
// Create title primitive
TextPrimitive titlePrimitive = new TextPrimitive
{
    Text = "User Settings",
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top,
    TextColor = Color.White,
    TextFont = new Font("Segoe UI", 14, FontStyle.Bold),
    Size = new Size(150, 30),
    BackColor = Color.Transparent,
    BorderColor = Color.Transparent
};

gradientPanel.Primitives.Add(titlePrimitive);
```

**VB.NET Example:**
```vb
Dim titlePrimitive As New TextPrimitive With {
    .Text = "User Settings",
    .Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top,
    .TextColor = Color.White,
    .TextFont = New Font("Segoe UI", 14, FontStyle.Bold),
    .Size = New Size(150, 30),
    .BackColor = Color.Transparent,
    .BorderColor = Color.Transparent
}

gradientPanel.Primitives.Add(titlePrimitive)
```

### Button-Style Text Primitives

**C# Example:**
```csharp
// OK button primitive
TextPrimitive okButton = new TextPrimitive
{
    Text = "OK",
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Bottom,
    Position = 100,
    Size = new Size(80, 30),
    BackColor = Color.LightGreen,
    TextColor = Color.DarkGreen,
    TextFont = new Font("Arial", 10, FontStyle.Bold),
    BorderColor = Color.DarkGreen
};

// Cancel button primitive
TextPrimitive cancelButton = new TextPrimitive
{
    Text = "Cancel",
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Bottom,
    Position = 200,
    Size = new Size(80, 30),
    BackColor = Color.LightCoral,
    TextColor = Color.DarkRed,
    TextFont = new Font("Arial", 10, FontStyle.Bold),
    BorderColor = Color.DarkRed
};

gradientPanel.Primitives.AddRange(new Primitive[] { okButton, cancelButton });
```

**Note:** TextPrimitives can be clicked and you can handle their click events (see Events documentation).

---

## HostPrimitive

Hosts any .NET Windows Forms control in panel borders.

**Namespace:** `Syncfusion.Windows.Forms.Tools.HostPrimitive`

### Key Properties

| Property | Type | Description |
|----------|------|-------------|
| **HostControl** | Control | The control to host |
| **Alignment** | Alignment enum | Border position |
| **Position** | int | Pixel offset |
| **Size** | Size | Primitive dimensions |
| **BackColor** | Color | Background color |

### Hosting a Button

**C# Example:**
```csharp
// Create button control
Button settingsButton = new Button
{
    Text = "Settings",
    FlatStyle = FlatStyle.Flat,
    BackColor = Color.White,
    ForeColor = Color.DarkBlue
};

// Create host primitive
HostPrimitive buttonHost = new HostPrimitive
{
    HostControl = settingsButton,
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top,
    Position = 300,
    Size = new Size(80, 25),
    BackColor = Color.Transparent
};

gradientPanel.Primitives.Add(buttonHost);
```

**VB.NET Example:**
```vb
' Create button control
Dim settingsButton As New Button With {
    .Text = "Settings",
    .FlatStyle = FlatStyle.Flat,
    .BackColor = Color.White,
    .ForeColor = Color.DarkBlue
}

' Create host primitive
Dim buttonHost As New HostPrimitive With {
    .HostControl = settingsButton,
    .Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top,
    .Position = 300,
    .Size = New Size(80, 25),
    .BackColor = Color.Transparent
}

gradientPanel.Primitives.Add(buttonHost)
```

### Hosting a ProgressBar

**C# Example:**
```csharp
// Create progress bar
ProgressBar progressBar = new ProgressBar
{
    Value = 65,
    Style = ProgressBarStyle.Continuous
};

// Host in bottom border
HostPrimitive progressHost = new HostPrimitive
{
    HostControl = progressBar,
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Bottom,
    Position = 50,
    Size = new Size(300, 20),
    BackColor = Color.Transparent
};

gradientPanel.Primitives.Add(progressHost);
```

### Hosting Custom Controls

**C# Example:**
```csharp
// Any custom control
MyCustomControl customControl = new MyCustomControl();

HostPrimitive customHost = new HostPrimitive
{
    HostControl = customControl,
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Right,
    Position = 50,
    Size = new Size(100, 100)
};

gradientPanel.Primitives.Add(customHost);
```

**You can host:** Button, TextBox, ComboBox, CheckBox, ProgressBar, TrackBar, custom controls, even other Syncfusion controls.

---

## Common Properties

Properties shared by all primitive types:

### Alignment Property

Specifies which border edge the primitive is placed on.

**Type:** `Syncfusion.Windows.Forms.Tools.Alignment` enum

**Values:**
- **Top**: Top border
- **Bottom**: Bottom border
- **Left**: Left border
- **Right**: Right border

**C# Example:**
```csharp
primitive.Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top;
primitive.Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Bottom;
primitive.Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Left;
primitive.Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Right;
```

---

### Position Property

Pixel offset from the starting point of the border.

**Type:** `int` (pixels)

**Behavior:**
- **Top/Bottom borders**: Position from left edge
- **Left/Right borders**: Position from top edge

**C# Example:**
```csharp
// On top border, 100 pixels from left
primitive.Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top;
primitive.Position = 100;

// On left border, 50 pixels from top
primitive.Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Left;
primitive.Position = 50;
```

---

### Size Property

Width and height of the primitive.

**Type:** `System.Drawing.Size`

**C# Example:**
```csharp
primitive.Size = new Size(100, 30);  // 100px wide, 30px tall
```

**Guidelines:**
- TextPrimitive: Width for text, height ~20-30px
- ImagePrimitive: Match image dimensions or scale
- HostPrimitive: Match hosted control size
- CollapsePrimitive: Square size ~25-40px

---

### BackColor Property

Background color of the primitive.

**Type:** `System.Drawing.Color`

**C# Example:**
```csharp
primitive.BackColor = Color.Transparent;  // Blend with panel
primitive.BackColor = Color.White;         // Solid background
```

---

## PrimitiveCollection Editor

Visual designer tool for adding and configuring primitives.

### Opening the Editor

1. Select GradientPanelExt in Designer
2. Find **Primitives** property in Properties window
3. Click **[...]** button

### Using the Editor

**Left Panel:** Shows list of added primitives  
**Dropdown:** Select primitive type (CollapsePrimitive, ImagePrimitive, TextPrimitive, HostPrimitive)  
**Add Button:** Adds selected type to collection  
**Remove Button:** Removes selected primitive  
**Right Panel:** Properties for selected primitive

**Steps:**
1. Select type from dropdown
2. Click **Add**
3. Configure properties in right panel:
   - Set Alignment, Position, Size
   - Set type-specific properties (Text, Image, HostControl, etc.)
4. Click **OK**

---

## Complete Examples

### Example 1: Login Panel with Primitives

```csharp
// Create gradient panel
GradientPanelExt loginPanel = new GradientPanelExt
{
    Size = new Size(400, 200),
    Location = new Point(50, 50),
    CornerRadius = 12
};

// Dark gradient
loginPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.FromArgb(40, 40, 40),
    Color.FromArgb(80, 80, 80)
);

// Title primitive at top
TextPrimitive title = new TextPrimitive
{
    Text = "User Login",
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top,
    TextColor = Color.White,
    TextFont = new Font("Segoe UI", 16, FontStyle.Bold),
    Size = new Size(120, 35),
    BackColor = Color.Transparent
};

// OK button primitive at bottom
TextPrimitive okButton = new TextPrimitive
{
    Text = "OK",
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Bottom,
    Position = 120,
    Size = new Size(70, 28),
    BackColor = Color.LightGreen,
    TextColor = Color.DarkGreen,
    TextFont = new Font("Arial", 10, FontStyle.Bold)
};

// Cancel button primitive at bottom
TextPrimitive cancelButton = new TextPrimitive
{
    Text = "Cancel",
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Bottom,
    Position = 210,
    Size = new Size(70, 28),
    BackColor = Color.LightCoral,
    TextColor = Color.DarkRed,
    TextFont = new Font("Arial", 10, FontStyle.Bold)
};

// Add primitives
loginPanel.Primitives.AddRange(new Primitive[] { title, okButton, cancelButton });

// Add username/password controls inside panel
Label lblUser = new Label
{
    Text = "Username:",
    Location = new Point(30, 60),
    ForeColor = Color.White,
    BackColor = Color.Transparent,
    AutoSize = true
};

TextBox txtUser = new TextBox
{
    Location = new Point(120, 58),
    Size = new Size(220, 20)
};

Label lblPass = new Label
{
    Text = "Password:",
    Location = new Point(30, 100),
    ForeColor = Color.White,
    BackColor = Color.Transparent,
    AutoSize = true
};

TextBox txtPass = new TextBox
{
    Location = new Point(120, 98),
    Size = new Size(220, 20),
    PasswordChar = '*'
};

loginPanel.Controls.AddRange(new Control[] { lblUser, txtUser, lblPass, txtPass });

this.Controls.Add(loginPanel);
```

---

### Example 2: Dashboard Panel with All Primitive Types

```csharp
GradientPanelExt dashboardPanel = new GradientPanelExt
{
    Size = new Size(500, 300),
    CornerRadius = 10
};

dashboardPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.WhiteSmoke,
    Color.LightGray
);

// 1. ImagePrimitive: Logo at top-left
ImagePrimitive logo = new ImagePrimitive
{
    Image = Properties.Resources.Logo,
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top,
    Position = 10,
    Size = new Size(32, 32),
    PrimitiveBorderStyle = PrimitiveBorderStyle.None
};

// 2. TextPrimitive: Title at top-center
TextPrimitive title = new TextPrimitive
{
    Text = "Sales Dashboard",
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top,
    Position = 180,
    Size = new Size(150, 32),
    TextFont = new Font("Arial", 14, FontStyle.Bold),
    TextColor = Color.DarkBlue,
    BackColor = Color.Transparent
};

// 3. CollapsePrimitive: Collapse button at top-right
CollapsePrimitive collapseBtn = new CollapsePrimitive
{
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top,
    Position = 450,
    Size = new Size(30, 30),
    CollapseImage = Properties.Resources.CollapseIcon,
    ExpandImage = Properties.Resources.ExpandIcon,
    BackColor = Color.Transparent
};

// 4. HostPrimitive: Progress bar at bottom
ProgressBar progress = new ProgressBar { Value = 70 };
HostPrimitive progressHost = new HostPrimitive
{
    HostControl = progress,
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Bottom,
    Position = 100,
    Size = new Size(300, 20),
    BackColor = Color.Transparent
};

dashboardPanel.Primitives.AddRange(new Primitive[] { 
    logo, 
    title, 
    collapseBtn, 
    progressHost 
});

this.Controls.Add(dashboardPanel);
```

---

## Best Practices

### 1. Size Primitives Appropriately

```csharp
// Text primitives: Height ~25-35px for readability
textPrimitive.Size = new Size(100, 30);

// Image primitives: Match image size or slightly larger
imagePrimitive.Size = new Size(40, 40);

// Host primitives: Match hosted control size
hostPrimitive.Size = new Size(hostedControl.Width, hostedControl.Height);
```

### 2. Position for Visual Balance

```csharp
// Symmetrical positioning
leftPrimitive.Position = 10;
rightPrimitive.Position = panelWidth - primitiveWidth - 10;

// Centered positioning
int centerPosition = (panelWidth - primitiveWidth) / 2;
centerPrimitive.Position = centerPosition;
```

### 3. Use Transparent Backgrounds

```csharp
// Let panel gradient show through
primitive.BackColor = Color.Transparent;
```

### 4. Coordinate Colors with Panel

```csharp
// Panel has dark gradient
panel.BackgroundColor = new BrushInfo(GradientStyle.Horizontal, Color.Navy, Color.Blue);

// Use light text
textPrimitive.TextColor = Color.White;
```

---

## Troubleshooting

### Primitive Not Visible

**Check:**
1. Size > 0 (both width and height)
2. Position within panel bounds
3. Alignment is set correctly
4. For ImagePrimitive: Image property is set
5. For TextPrimitive: Text property is not empty
6. For HostPrimitive: HostControl is not null

```csharp
// Debug primitive
Debug.WriteLine($"Size: {primitive.Size}");
Debug.WriteLine($"Position: {primitive.Position}");
Debug.WriteLine($"Alignment: {primitive.Alignment}");
```

### Collapse Primitive Not Working

**Check:**
- Both CollapseImage and ExpandImage are set
- Images are loaded correctly (not null)
- Size is adequate (minimum 20x20)

### Host Primitive Control Not Interactive

**Solution:** Ensure hosted control is properly initialized and enabled

```csharp
Button btn = new Button { Text = "Click Me", Enabled = true };
hostPrimitive.HostControl = btn;
```

### Primitives Overlapping

**Solution:** Adjust Position values to create spacing

```csharp
primitive1.Position = 10;
primitive2.Position = primitive1.Position + primitive1.Size.Width + 10;  // 10px gap
```

---

## Related Topics

- **Getting Started**: Basic setup → [getting-started.md](getting-started.md)
- **Collapse Animation**: Animation settings → [collapse-expand-animation.md](collapse-expand-animation.md)
- **Events**: Handling clicks → [scroll-settings-events.md](scroll-settings-events.md)
