---
name: syncfusion-winforms-tooltip
description: Guide for implementing Syncfusion Windows Forms Tooltip (SfToolTip) component for displaying contextual information and hover text in Windows Forms applications. Use this skill when implementing tooltips, popup information, balloon tips, or informational popups on controls. Covers SfToolTip setup, multi-item tooltips, custom tooltip content, appearance customization, and tooltip images.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion Windows Forms Tooltips

The Syncfusion `SfToolTip` component provides rich tooltip functionality for Windows Forms applications, appearing automatically as a popup when the user hovers over a control. It supports multiple tooltip items, custom controls, images, and extensive appearance customization.

## When to Use This Skill

Use this skill when you need to:

- **Add tooltips** to Windows Forms controls with basic or advanced content
- **Display multi-item tooltips** with multiple lines of information or sections
- **Show balloon-style tooltips** with beaks pointing to controls
- **Customize tooltip appearance** with colors, borders, gradients, images
- **Host custom controls** inside tooltips for rich interactive content
- **Programmatically control** tooltip display timing and positioning
- **Implement contextual help** with hover-based information panels
- **Support RTL layouts** for internationalized applications
- **Create custom-drawn tooltips** with specialized rendering

## Component Overview

The `SfToolTip` component offers:

- **Multiple items:** Add multiple tooltip items with individual styling
- **Custom controls:** Host any user control in tooltip items
- **Rich content:** Text, images, gradients, and separators
- **Balloon style:** Display tooltips with directional beaks
- **Appearance:** Extensive customization (colors, fonts, borders, shadows)
- **Events:** Control display behavior and custom drawing
- **Programmatic control:** Show/hide tooltips on demand

**Comparison:** Choose `SfToolTip` over `SuperToolTip` when you need multi-item support, individual tooltip item customization, or custom control hosting. `SfToolTip` offers richer styling and flexibility.

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

When implementing basic tooltips:
- Assembly deployment and required dependencies
- Setting tooltips using simple text (designer and code)
- Setting tooltips using ToolTipInfo for structured content
- Programmatically showing and hiding tooltips
- Configuring tooltip delay times (InitialDelay, AutoPopDelay)
- Basic balloon style setup

### Tooltip Items and Content
📄 **Read:** [references/tooltip-items-and-content.md](references/tooltip-items-and-content.md)

When working with tooltip content structure:
- Creating and configuring ToolTipItem objects
- Adding multiple items to a single tooltip
- Configuring spacing and padding between items
- Adding images using Image or ImageList properties
- Setting image alignment (Left, Top, Right)
- Configuring image size and spacing
- Hosting custom user controls in tooltips

### Balloon Style and Beak
📄 **Read:** [references/balloon-style-and-beak.md](references/balloon-style-and-beak.md)

When implementing balloon-style tooltips:
- Enabling ToolTipStyle.Balloon
- Understanding balloon vs rectangle styles
- Customizing beak back color
- Positioning balloons relative to controls
- Use cases for balloon-style tooltips

### Appearance Customization
📄 **Read:** [references/appearance-customization.md](references/appearance-customization.md)

When customizing tooltip visual appearance:
- Border color and thickness
- Individual ToolTipItem styling (colors, fonts, alignment)
- Gradient background implementation
- Separator customization between items
- Shadow effects
- RTL (Right-to-Left) layout support

### Advanced Usage and Events
📄 **Read:** [references/advanced-usage.md](references/advanced-usage.md)

When implementing advanced tooltip features:
- Getting and setting tooltip text/info programmatically
- Using ToolTipShowing event for conditional display
- Customizing appearance per control dynamically
- Changing tooltip location before display
- Setting minimum and maximum widths
- Custom drawing with DrawToolTipItem event
- Canceling default rendering

## Quick Start

### Basic Tooltip with Text

```csharp
using Syncfusion.Windows.Forms;

// Add SfToolTip component to form
SfToolTip sfToolTip1 = new SfToolTip();

// Set simple text tooltip
sfToolTip1.SetToolTip(this.button1, "Click to submit the form");
```

### Tooltip with Multiple Items

```csharp
using Syncfusion.Windows.Forms;
using Syncfusion.WinForms.Controls;

SfToolTip sfToolTip1 = new SfToolTip();

// Create ToolTipInfo with multiple items
ToolTipInfo toolTipInfo = new ToolTipInfo();

ToolTipItem item1 = new ToolTipItem();
item1.Text = "User Information";
item1.Style.Font = new Font("Arial", 10f, FontStyle.Bold);

ToolTipItem item2 = new ToolTipItem();
item2.Text = "Name: John Doe\nEmail: john@example.com";

toolTipInfo.Items.AddRange(new ToolTipItem[] { item1, item2 });
sfToolTip1.SetToolTipInfo(this.button1, toolTipInfo);
```

### Balloon Style Tooltip with Image

```csharp
SfToolTip sfToolTip1 = new SfToolTip();

ToolTipInfo toolTipInfo = new ToolTipInfo();
toolTipInfo.ToolTipStyle = ToolTipStyle.Balloon;
toolTipInfo.BeakBackColor = Color.LightSkyBlue;

ToolTipItem toolTipItem = new ToolTipItem();
toolTipItem.Text = "David Carter\nPhone: +1 919.494.1974\nEmail: david@syncfusion.com";
toolTipItem.Image = Properties.Resources.UserImage;
toolTipItem.Style.ImageSize = new Size(80, 80);
toolTipItem.Style.ImageAlignment = ToolTipImageAlignment.Left;

toolTipInfo.Items.Add(toolTipItem);
sfToolTip1.SetToolTipInfo(this.button1, toolTipInfo);
```

## Common Patterns

### Pattern 1: Rich Formatted Tooltip with Gradient

```csharp
ToolTipInfo toolTipInfo = new ToolTipInfo();
toolTipInfo.BorderColor = Color.DarkGray;
toolTipInfo.BorderThickness = 2;

ToolTipItem toolTipItem = new ToolTipItem();
toolTipItem.Text = "Important Information";
toolTipItem.EnableGradientBackground = true;
toolTipItem.Style.GradientBrush = new BrushInfo(
    GradientStyle.Horizontal, 
    new Color[] { Color.LightBlue, Color.LightGreen }
);
toolTipItem.Style.Font = new Font("Arial", 10f, FontStyle.Bold);
toolTipItem.Style.ForeColor = Color.DarkBlue;

toolTipInfo.Items.Add(toolTipItem);
sfToolTip1.SetToolTipInfo(this.control, toolTipInfo);
```

### Pattern 2: Conditional Tooltip Display

```csharp
sfToolTip1.ToolTipShowing += (sender, e) =>
{
    // Customize tooltip based on control state
    if (e.Control is Button button && !button.Enabled)
    {
        e.Cancel = true; // Don't show tooltip for disabled controls
    }
    
    // Or modify appearance dynamically
    if (e.Control.Tag?.ToString() == "Warning")
    {
        e.ToolTipInfo.Items[0].Style.BackColor = Color.LightCoral;
        e.ToolTipInfo.BorderColor = Color.Red;
    }
};
```

### Pattern 3: Programmatic Tooltip Display

```csharp
// Show tooltip at specific location
Point location = new Point(300, 200);
sfToolTip1.Show("Processing complete!", location);

// Show tooltip at cursor position with duration
ToolTipInfo info = new ToolTipInfo();
ToolTipItem item = new ToolTipItem { Text = "Operation successful!" };
info.Items.Add(item);
sfToolTip1.Show(info);

// Hide after delay
Thread.Sleep(3000);
sfToolTip1.Hide();
```

### Pattern 4: Custom Control in Tooltip

```csharp
// Create custom control (e.g., PictureBox with animated GIF)
PictureBox pictureBox = new PictureBox();
pictureBox.Image = Image.FromFile("loading.gif");
pictureBox.SizeMode = PictureBoxSizeMode.CenterImage;
pictureBox.Size = new Size(200, 100);

ToolTipInfo toolTipInfo = new ToolTipInfo();
ToolTipItem toolTipItem = new ToolTipItem();
toolTipItem.Control = pictureBox;

toolTipInfo.Items.Add(toolTipItem);
sfToolTip1.SetToolTipInfo(this.button1, toolTipInfo);
```


## Key Properties

### SfToolTip Properties

| Property | Type | Description |
|----------|------|-------------|
| `InitialDelay` | int | Delay before tooltip appears (milliseconds, default: 0) |
| `AutoPopDelay` | int | Duration tooltip remains visible (milliseconds, default: 5000) |
| `ShadowVisible` | bool | Enable shadow effect on tooltip |

### ToolTipInfo Properties

| Property | Type | Description |
|----------|------|-------------|
| `Items` | ToolTipItemCollection | Collection of tooltip items to display |
| `ToolTipStyle` | ToolTipStyle | Rectangle (default) or Balloon style |
| `BeakBackColor` | Color | Background color of balloon beak |
| `BorderColor` | Color | Border color of tooltip |
| `BorderThickness` | int | Border thickness in pixels |
| `MinWidth` | int | Minimum tooltip width |
| `MaxWidth` | int | Maximum tooltip width |
| `RightToLeft` | RightToLeft | RTL layout support |

### ToolTipItem Properties

| Property | Type | Description |
|----------|------|-------------|
| `Text` | string | Tooltip item text content |
| `Image` | Image | Image to display in item |
| `ImageList` | ImageList | ImageList for multiple images |
| `ImageIndex` | int | Index when using ImageList |
| `Control` | Control | Custom control to host in item |
| `EnableSeparator` | bool | Show separator line after item |
| `EnableGradientBackground` | bool | Enable gradient background |
| `Padding` | Padding | Spacing around item content |
| `Style` | ToolTipStyleInfo | Comprehensive styling options |

### ToolTipStyleInfo (Style) Properties

| Property | Type | Description |
|----------|------|-------------|
| `BackColor` | Color | Background color |
| `ForeColor` | Color | Text color |
| `Font` | Font | Text font |
| `TextAlignment` | ContentAlignment | Text alignment (MiddleLeft, etc.) |
| `ImageAlignment` | ToolTipImageAlignment | Image position (Left, Top, Right) |
| `ImageSize` | Size | Fixed image dimensions |
| `ImageToTextOffset` | int | Spacing between image and text |
| `GradientBrush` | BrushInfo | Gradient configuration |
| `SeparatorColor` | Color | Separator line color |
| `SeparatorStyle` | DashStyle | Separator line style |

## Common Use Cases

1. **Form Field Help:** Add descriptive tooltips to form controls explaining expected input
2. **Button Actions:** Display action descriptions for toolbar buttons and icons
3. **Status Information:** Show detailed status for status bar items or indicators
4. **User Profiles:** Display rich profile cards with images on hover
5. **Data Validation:** Show validation rules or error details on validation controls
6. **Dashboard Widgets:** Provide detailed metrics in chart or widget tooltips
7. **Disabled Controls:** Explain why a control is disabled and when it becomes available
8. **Progress Indicators:** Show detailed progress information on hover
9. **Icon Tray Applications:** Display application status in system tray tooltips
10. **Multi-language Support:** Show translations or additional context with RTL support

## Events

| Event | Description | Use Case |
|-------|-------------|----------|
| `ToolTipShowing` | Fires before tooltip displays | Customize appearance per control, cancel display conditionally, adjust location |
| `DrawToolTipItem` | Fires during tooltip rendering | Implement custom drawing, override default appearance |

## Assembly Dependencies

**Required Assembly:**
- `Syncfusion.Core.WinForms.dll` and `Syncfusion.Shared.Base`

**NuGet Package:**
- `Syncfusion.Core.WinForms`

## Best Practices

1. **Keep tooltips concise:** Users read tooltips quickly; provide essential information only
2. **Use balloon style sparingly:** Balloon tooltips draw more attention; use for important hints
3. **Avoid redundancy:** Don't repeat control labels verbatim in tooltips
4. **Consider timing:** Adjust `InitialDelay` and `AutoPopDelay` based on content complexity
5. **Accessibility:** Ensure tooltip text is readable with sufficient color contrast
6. **Performance:** Avoid complex custom controls in tooltips for frequently-hovered controls
7. **Test RTL layouts:** Verify tooltip appearance in right-to-left language configurations
