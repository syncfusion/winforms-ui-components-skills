---
name: syncfusion-winforms-gradientpanelext
description: Implement Syncfusion WinForms GradientPanelExt for styled container panels with gradient backgrounds, rounded corners, and border primitives. Use this when building panels with gradient effects, rounded corner containers, collapsible sections, or panels with border elements (text, images, buttons) in WinForms.
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
---

# Implementing GradientPanelExt

An enhanced panel control for Windows Forms that provides gradient backgrounds, rounded corners, border primitives, and collapse/expand functionality for creating sophisticated container panels.

## When to Use This Skill

Use this skill when you need to:
- **Gradient Container Panels**: Create panels with gradient backgrounds that contain other controls
- **Rounded Corner Panels**: Design panels with customizable corner radius for modern UI
- **Border Primitives**: Add text, images, buttons, or controls to panel borders (unique feature)
- **Collapsible Sections**: Implement expandable/collapsible panel sections with animation
- **Styled Containers**: Build visually appealing container panels with custom backgrounds
- **Login/Dialog Panels**: Create attractive login forms or dialog boxes with gradients
- **Dashboard Sections**: Design dashboard panels with titles and controls in borders

## Component Overview

The **GradientPanelExt** is an enhanced version of the standard Panel control that provides:
- Multiple gradient styles (Horizontal, Vertical, Diagonal, PathRectangle, PathEllipse)
- Rounded corners with configurable radius
- **Primitives system** - host text, images, collapse controls, and .NET controls in panel borders
- Pattern and solid background options
- Collapse/expand animation support
- Multi-color gradient support
- Acts as container for other WinForms controls

**Key Capabilities:**
- Gradient background using BrushInfo class
- CornerRadius for rounded rectangle appearance
- Primitives collection (CollapsePrimitive, ImagePrimitive, TextPrimitive, HostPrimitive)
- BorderGap for spacing control
- Animated collapse/expand with speed and delay settings
- Background image support
- Hosts child controls like standard Panel

**Unique Feature - Primitives:**
GradientPanelExt's most powerful feature is the ability to add "primitives" to panel borders - text labels, images, collapse buttons, or even entire .NET controls positioned in the Top, Bottom, Left, or Right borders.

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

**When to read:** Starting new implementation, setting up dependencies, first-time usage

**Topics covered:**
- Assembly deployment (Syncfusion.Shared.Base)
- NuGet package installation
- Adding via Visual Studio designer (toolbox drag-drop)
- Adding via code (manual instantiation)
- Basic panel initialization and configuration
- Adding child controls to the panel
- PrimitiveCollection Editor introduction
- Simple gradient panel examples
- Namespace requirements (Syncfusion.Windows.Forms.Tools)

---

### Background Styling
📄 **Read:** [references/background-styling.md](references/background-styling.md)

**When to read:** Configuring gradients, setting background colors, customizing panel appearance

**Topics covered:**
- BackgroundColor property and BrushInfo class
- Background styles (Solid, Pattern, Gradient, None)
- Gradient styles (Horizontal, Vertical, ForwardDiagonal, BackwardDiagonal, PathRectangle, PathEllipse)
- BackColor and ForeColor properties
- Multi-color gradients using GradientColors array
- Pattern backgrounds with PatternStyle
- BackgroundImage and BackgroundImageLayout
- Creating complex color transitions
- Complete styling examples with code

---

### Border and Corner Settings
📄 **Read:** [references/border-corner-settings.md](references/border-corner-settings.md)

**When to read:** Customizing corners, adjusting border appearance, creating rounded panels

**Topics covered:**
- CornerRadius property for rounded corners
- Removing rounded corners (CornerRadius = 0)
- BorderGap property for margin spacing
- Sharp vs rounded corner design
- Coordinating corners with gradients
- Complete examples with different radius values
- Best practices for modern vs classic design

---

### Primitives
📄 **Read:** [references/primitives.md](references/primitives.md)

**When to read:** Adding border elements, implementing collapse functionality, hosting controls in borders

**Topics covered:**
- Primitives overview and PrimitiveCollection Editor
- CollapsePrimitive (expand/collapse with images)
- ImagePrimitive (images in borders)
- TextPrimitive (text labels in borders)
- HostPrimitive (hosting .NET controls in borders)
- Alignment property (Top, Bottom, Left, Right)
- Position property for pixel placement
- Size property for primitive dimensions
- Complete examples with all primitive types
- Login panel with primitives example

---

### Collapse and Expand Animation
📄 **Read:** [references/collapse-expand-animation.md](references/collapse-expand-animation.md)

**When to read:** Implementing animated collapse, configuring animation behavior

**Topics covered:**
- Animated property (enable/disable animation)
- AnimationDelay property for timing
- AnimationSpeed property for speed control
- Integration with CollapsePrimitive
- Smooth vs instant collapse behavior
- Complete animation examples
- Performance considerations

---

### Scroll Settings and Events
📄 **Read:** [references/scroll-settings-events.md](references/scroll-settings-events.md)

**When to read:** Handling scrollable content, responding to events, custom event handling

**Topics covered:**
- AutoScroll property for scrollable panels
- Scroll bar configuration
- GradientPanelExt-specific events
- Collapse/expand event handling
- Primitive click events
- Paint events for custom drawing
- Complete event handler examples
- Memory management and cleanup

---

## Quick Start

### Basic Gradient Panel Setup

```csharp
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Drawing;

// Create GradientPanelExt
GradientPanelExt gradientPanel = new GradientPanelExt();
gradientPanel.Size = new Size(400, 200);
gradientPanel.Location = new Point(20, 20);

// Set gradient background (horizontal blue gradient)
gradientPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.DarkBlue,
    Color.LightSkyBlue
);

// Rounded corners
gradientPanel.CornerRadius = 10;

// Add child controls
Label label = new Label
{
    Text = "Welcome",
    ForeColor = Color.White,
    BackColor = Color.Transparent,
    Location = new Point(20, 20),
    AutoSize = true
};
gradientPanel.Controls.Add(label);

// Add to form
this.Controls.Add(gradientPanel);
```

### With Primitives in Borders

```csharp
// Add title text primitive at top border
TextPrimitive titlePrimitive = new TextPrimitive
{
    Text = "User Login",
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top,
    TextColor = Color.White,
    TextFont = new Font("Arial", 12, FontStyle.Bold),
    Size = new Size(100, 25),
    BackColor = Color.Transparent
};

// Add button primitives at bottom border
TextPrimitive okButton = new TextPrimitive
{
    Text = "OK",
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Bottom,
    Position = 100,
    Size = new Size(60, 25),
    BackColor = Color.White,
    TextColor = Color.Black
};

TextPrimitive cancelButton = new TextPrimitive
{
    Text = "Cancel",
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Bottom,
    Position = 180,
    Size = new Size(60, 25),
    BackColor = Color.White,
    TextColor = Color.Black
};

// Add primitives to panel
gradientPanel.Primitives.AddRange(new Primitive[] { 
    titlePrimitive, 
    okButton, 
    cancelButton 
});
```

## Common Patterns

### Pattern 1: Login Panel with Gradient and Primitives
```csharp
// Create login panel
GradientPanelExt loginPanel = new GradientPanelExt
{
    Size = new Size(350, 180),
    Location = new Point(50, 50),
    CornerRadius = 12
};

// Dark gradient background
loginPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.FromArgb(40, 40, 40),
    Color.FromArgb(80, 80, 80)
);

// Title primitive at top
TextPrimitive title = new TextPrimitive
{
    Text = "Login",
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top,
    TextColor = Color.White,
    TextFont = new Font("Segoe UI", 14, FontStyle.Bold),
    Size = new Size(80, 30),
    BackColor = Color.Transparent
};
loginPanel.Primitives.Add(title);

// Add username and password textboxes inside panel
Label lblUsername = new Label
{
    Text = "Username:",
    Location = new Point(30, 40),
    ForeColor = Color.White,
    BackColor = Color.Transparent,
    AutoSize = true
};

TextBox txtUsername = new TextBox
{
    Location = new Point(110, 38),
    Size = new Size(180, 20)
};

loginPanel.Controls.Add(lblUsername);
loginPanel.Controls.Add(txtUsername);
```

### Pattern 2: Collapsible Dashboard Section
```csharp
// Create collapsible panel
GradientPanelExt dashboardPanel = new GradientPanelExt
{
    Size = new Size(400, 250),
    Location = new Point(20, 20),
    CornerRadius = 8,
    Animated = true,
    AnimationSpeed = 3
};

// Vertical gradient
dashboardPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.WhiteSmoke,
    Color.LightGray
);

// Section title
TextPrimitive sectionTitle = new TextPrimitive
{
    Text = "Sales Statistics",
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top,
    TextColor = Color.DarkBlue,
    TextFont = new Font("Arial", 12, FontStyle.Bold),
    Size = new Size(150, 25)
};

// Collapse primitive
CollapsePrimitive collapseButton = new CollapsePrimitive
{
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top,
    Position = 350,
    Size = new Size(30, 30),
    BackColor = Color.Transparent
};
// Set images in designer or load from resources

dashboardPanel.Primitives.AddRange(new Primitive[] { 
    sectionTitle, 
    collapseButton 
});
```

### Pattern 3: Panel with Image and Host Primitives
```csharp
// Create styled panel
GradientPanelExt styledPanel = new GradientPanelExt
{
    Size = new Size(450, 300),
    CornerRadius = 10
};

// PathEllipse gradient with multiple colors
styledPanel.BackgroundColor = new BrushInfo(
    GradientStyle.PathEllipse,
    new Color[]
    {
        Color.LightBlue,
        Color.LightCyan,
        Color.PaleTurquoise
    }
);

// Image primitive at top-left
ImagePrimitive logo = new ImagePrimitive
{
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top,
    Position = 10,
    Size = new Size(40, 40)
    // Set Image property from resources
};

// Host a ProgressBar at bottom
ProgressBar progressBar = new ProgressBar
{
    Value = 50,
    Style = ProgressBarStyle.Continuous
};

HostPrimitive progressHost = new HostPrimitive
{
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Bottom,
    Position = 100,
    Size = new Size(250, 20),
    HostControl = progressBar,
    BackColor = Color.Transparent
};

styledPanel.Primitives.AddRange(new Primitive[] { 
    logo, 
    progressHost 
});
```

## Key Properties

### Background Properties
- **BackgroundColor**: Gets/sets background using BrushInfo (style, colors, gradient)
- **BackgroundColor.Style**: Brush style (Solid, Pattern, Gradient, None)
- **BackgroundColor.GradientStyle**: Gradient direction (Horizontal, Vertical, ForwardDiagonal, BackwardDiagonal, PathRectangle, PathEllipse)
- **BackgroundColor.BackColor**: Starting color for gradient
- **BackgroundColor.ForeColor**: Ending color for gradient
- **BackgroundColor.PatternStyle**: Pattern for Pattern brush style
- **BackgroundColor.GradientColors**: Array for multi-color gradients
- **BackgroundImage**: Background image for the panel
- **BackgroundImageLayout**: Layout style for background image

### Border and Corner Properties
- **CornerRadius**: Radius for rounded corners (0 = sharp corners)
- **BorderGap**: Spacing between border and margins

### Primitive Properties
- **Primitives**: Collection of primitives (accessed via PrimitiveCollection Editor)
- **Primitive.Alignment**: Border position (Top, Bottom, Left, Right)
- **Primitive.Position**: Pixel offset along the border
- **Primitive.Size**: Width and height of primitive
- **Primitive.BackColor**: Background color for primitive

### Animation Properties
- **Animated**: Enable/disable collapse animation
- **AnimationDelay**: Delay in milliseconds for animation
- **AnimationSpeed**: Speed factor for animation

### Container Properties (Inherited from Panel)
- **Controls**: Collection of child controls hosted in the panel
- **AutoScroll**: Enable scrollbars for overflow content
- **Padding**: Internal padding for child controls

## Common Use Cases

### Login/Authentication Panels
Create visually appealing login forms with gradient backgrounds, titles in borders, and button primitives.

### Dashboard Sections
Build dashboard panels with section titles in top border and collapsible functionality.

### Settings Panels
Design settings containers with category titles and organized control groups.

### Information Boxes
Create styled information panels with icons in borders and rich content inside.

### Wizard Steps
Implement wizard step panels with step numbers or icons in borders.

### Grouped Controls
Organize related form controls in visually distinct gradient panels.

### Collapsible Sidebars
Create expandable sidebar panels with collapse primitives for space management.

## Notes

- GradientPanelExt derives from Panel, so all standard Panel functionality is available
- The Primitives feature is unique to GradientPanelExt and extremely powerful
- Primitives are positioned in borders, not in the main panel area
- CornerRadius provides rounded corners by default (set to 0 for sharp corners)
- Child controls are added to the Controls collection like a normal Panel
- CollapsePrimitive requires collapse and expand images to be set
- HostPrimitive can host any .NET control, even custom controls
- Animation properties only affect collapse/expand when CollapsePrimitive is used
- BackgroundColor and Primitives can use same or coordinated gradients
- Performance is good for typical usage; avoid excessive primitives
