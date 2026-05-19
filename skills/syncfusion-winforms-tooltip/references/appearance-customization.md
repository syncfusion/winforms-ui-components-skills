# Appearance Customization

## Table of Contents
- [Tooltip Control Appearance](#tooltip-control-appearance)
- [ToolTipItem Styling](#tooltipitem-styling)
- [Gradient Backgrounds](#gradient-backgrounds)
- [Separator Customization](#separator-customization)
- [Shadow Effects](#shadow-effects)
- [Right-to-Left Layout](#right-to-left-layout)

This guide covers comprehensive appearance customization for `SfToolTip`, including borders, colors, gradients, separators, shadows.

## Tooltip Control Appearance

Customize the overall tooltip container appearance using `ToolTipInfo` properties.

### Border Customization

Control border color and thickness for the entire tooltip.

#### BorderColor Property

```csharp
ToolTipInfo toolTipInfo1 = new ToolTipInfo();
toolTipInfo1.BorderColor = Color.Gray;

ToolTipItem toolTipItem1 = new ToolTipItem();
toolTipItem1.Text = "The ToolTip information of the Button control.";

toolTipInfo1.Items.AddRange(new ToolTipItem[] { toolTipItem1 });
sfToolTip1.SetToolTipInfo(this.button1, toolTipInfo1);
```

#### BorderThickness Property

```csharp
ToolTipInfo toolTipInfo1 = new ToolTipInfo();
toolTipInfo1.BorderColor = Color.Gray;
toolTipInfo1.BorderThickness = 5;

ToolTipItem toolTipItem1 = new ToolTipItem();
toolTipItem1.Text = "The ToolTip information of the Button control.";

toolTipInfo1.Items.AddRange(new ToolTipItem[] { toolTipItem1 });
sfToolTip1.SetToolTipInfo(this.button1, toolTipInfo1);
```

**Range:** Typically 1-5 pixels; higher values create bold borders.

**Example - Subtle Border:**
```csharp
toolTipInfo1.BorderColor = Color.LightGray;
toolTipInfo1.BorderThickness = 1;
```

**Example - Prominent Border:**
```csharp
toolTipInfo1.BorderColor = Color.DarkBlue;
toolTipInfo1.BorderThickness = 3;
```

## ToolTipItem Styling

Customize individual tooltip items using the `ToolTipStyleInfo` (accessed via `Style` property).

### Basic Styling Properties

```csharp
ToolTipInfo toolTipInfo1 = new ToolTipInfo();

ToolTipItem toolTipItem1 = new ToolTipItem();
toolTipItem1.Text = "The ToolTip information of the Button control.";

// Apply styling
toolTipItem1.Style.BackColor = Color.LightSkyBlue;
toolTipItem1.Style.ForeColor = Color.Black;
toolTipItem1.Style.TextAlignment = ContentAlignment.MiddleCenter;
toolTipItem1.Style.Font = new Font("Arial", 10.5f, FontStyle.Bold);

toolTipInfo1.Items.AddRange(new ToolTipItem[] { toolTipItem1 });
sfToolTip1.SetToolTipInfo(this.button1, toolTipInfo1);
```

### Available Style Properties

| Property | Type | Description |
|----------|------|-------------|
| `BackColor` | Color | Background color of the item |
| `ForeColor` | Color | Text color |
| `Font` | Font | Text font (family, size, style) |
| `TextAlignment` | ContentAlignment | Text position within item |

### Text Alignment Options

```csharp
// Available ContentAlignment values:
toolTipItem1.Style.TextAlignment = ContentAlignment.TopLeft;
toolTipItem1.Style.TextAlignment = ContentAlignment.TopCenter;
toolTipItem1.Style.TextAlignment = ContentAlignment.TopRight;
toolTipItem1.Style.TextAlignment = ContentAlignment.MiddleLeft;
toolTipItem1.Style.TextAlignment = ContentAlignment.MiddleCenter;
toolTipItem1.Style.TextAlignment = ContentAlignment.MiddleRight;
toolTipItem1.Style.TextAlignment = ContentAlignment.BottomLeft;
toolTipItem1.Style.TextAlignment = ContentAlignment.BottomCenter;
toolTipItem1.Style.TextAlignment = ContentAlignment.BottomRight;
```

**Common Choices:**
- `MiddleLeft` - For text with left-aligned images
- `MiddleCenter` - For centered, standalone text
- `TopLeft` - For long paragraphs

### Multi-Item Styling Example

```csharp
ToolTipInfo toolTipInfo1 = new ToolTipInfo();

// Header item
ToolTipItem header = new ToolTipItem();
header.Text = "System Status";
header.Style.BackColor = Color.Navy;
header.Style.ForeColor = Color.White;
header.Style.Font = new Font("Arial", 11f, FontStyle.Bold);
header.Style.TextAlignment = ContentAlignment.MiddleCenter;

// Content item
ToolTipItem content = new ToolTipItem();
content.Text = "All systems operational";
content.Style.BackColor = Color.LightGreen;
content.Style.ForeColor = Color.DarkGreen;
content.Style.Font = new Font("Arial", 9f);
content.Style.TextAlignment = ContentAlignment.MiddleLeft;

toolTipInfo1.Items.AddRange(new ToolTipItem[] { header, content });
sfToolTip1.SetToolTipInfo(this.statusIndicator, toolTipInfo1);
```

## Gradient Backgrounds

Create visually appealing gradient backgrounds for tooltip items.

### Enabling Gradient Background

Gradients require two steps:
1. Set `EnableGradientBackground = true`
2. Configure `GradientBrush` property

```csharp
ToolTipItem toolTipItem1 = new ToolTipItem();
toolTipItem1.Text = "The ToolTip information of the Button control.";

// Enable gradient
toolTipItem1.EnableGradientBackground = true;

// Configure gradient
toolTipItem1.Style.GradientBrush = new BrushInfo(
    GradientStyle.ForwardDiagonal,
    new Color[] { Color.LightSkyBlue, Color.LightGreen, Color.Orange }
);

ToolTipInfo toolTipInfo1 = new ToolTipInfo();
toolTipInfo1.Items.AddRange(new ToolTipItem[] { toolTipItem1 });
sfToolTip1.SetToolTipInfo(this.button2, toolTipInfo1);
```

**Important:** `GradientBrush` is ignored if `EnableGradientBackground` is `false`.

### Gradient Styles

Available gradient directions:

```csharp
using Syncfusion.WinForms.Core;
using Syncfusion.WinForms.Core.Enums;

// Horizontal gradient (left to right)
toolTipItem1.Style.GradientBrush = new BrushInfo(
    GradientStyle.Horizontal,
    new Color[] { Color.Blue, Color.Green }
);

// Vertical gradient (top to bottom)
toolTipItem1.Style.GradientBrush = new BrushInfo(
    GradientStyle.Vertical,
    new Color[] { Color.Blue, Color.Green }
);

// Diagonal gradients
toolTipItem1.Style.GradientBrush = new BrushInfo(
    GradientStyle.ForwardDiagonal,
    new Color[] { Color.Blue, Color.Green }
);

toolTipItem1.Style.GradientBrush = new BrushInfo(
    GradientStyle.BackwardDiagonal,
    new Color[] { Color.Blue, Color.Green }
);
```

### Multi-Color Gradients

Use more than two colors for complex gradients:

```csharp
toolTipItem1.Style.GradientBrush = new BrushInfo(
    GradientStyle.Horizontal,
    new Color[] { 
        Color.Red, 
        Color.Yellow, 
        Color.Green, 
        Color.Blue 
    }
);
```

**Use Case:** Create rainbow effects or multi-stage progress indicators.

### Gradient Best Practices

```csharp
// Subtle gradient for professional appearance
toolTipItem1.EnableGradientBackground = true;
toolTipItem1.Style.GradientBrush = new BrushInfo(
    GradientStyle.Vertical,
    new Color[] { Color.White, Color.FromArgb(240, 240, 240) }
);
toolTipItem1.Style.ForeColor = Color.Black;

// Bold gradient for attention
toolTipItem1.EnableGradientBackground = true;
toolTipItem1.Style.GradientBrush = new BrushInfo(
    GradientStyle.Horizontal,
    new Color[] { Color.DarkOrange, Color.OrangeRed }
);
toolTipItem1.Style.ForeColor = Color.White;
toolTipItem1.Style.Font = new Font("Arial", 10f, FontStyle.Bold);
```

**Tip:** Ensure text `ForeColor` contrasts well with gradient colors.

## Separator Customization

Add visual separators between tooltip items to organize content.

### Enabling Separators

Set `EnableSeparator = true` on a `ToolTipItem` to draw a separator line after it.

```csharp
ToolTipItem toolTipItem1 = new ToolTipItem();
toolTipItem1.Text = "ToolTipItem1 Text";
toolTipItem1.EnableSeparator = true;

ToolTipItem toolTipItem2 = new ToolTipItem();
toolTipItem2.Text = "ToolTipItem2 Text";

ToolTipInfo toolTipInfo1 = new ToolTipInfo();
toolTipInfo1.Items.AddRange(new ToolTipItem[] { toolTipItem1, toolTipItem2 });
sfToolTip1.SetToolTipInfo(this.button2, toolTipInfo1);
```

**Important:** Separators cannot be drawn after the last item in the collection.

### Separator Color

```csharp
toolTipItem1.EnableSeparator = true;
toolTipItem1.Style.SeparatorColor = Color.Gray;
```

### Separator Style

Control the line style using `SeparatorStyle` property.

```csharp
using System.Drawing.Drawing2D;

toolTipItem1.EnableSeparator = true;
toolTipItem1.Style.SeparatorColor = Color.Gray;
toolTipItem1.Style.SeparatorStyle = DashStyle.DashDot;
```

**Available DashStyle Values:**
- `DashStyle.Solid` - Continuous line (default)
- `DashStyle.Dash` - Dashed line
- `DashStyle.Dot` - Dotted line
- `DashStyle.DashDot` - Alternating dashes and dots
- `DashStyle.DashDotDot` - Dash followed by two dots
- `DashStyle.Custom` - Allows defining a custom dash pattern (requires custom rendering)

> Note:
> `DashStyle.Custom` requires custom rendering logic. By default, it behaves similar to a solid line as custom dash patterns are not supported directly in SfToolTip.

### Complete Separator Example

```csharp
ToolTipInfo toolTipInfo1 = new ToolTipInfo();

// Item 1 with separator
ToolTipItem toolTipItem1 = new ToolTipItem();
toolTipItem1.Text = "Section 1: User Details";
toolTipItem1.EnableSeparator = true;
toolTipItem1.Style.SeparatorColor = Color.DarkGray;
toolTipItem1.Style.SeparatorStyle = DashStyle.Solid;
toolTipItem1.Style.Font = new Font("Arial", 9f, FontStyle.Bold);

// Item 2 with separator
ToolTipItem toolTipItem2 = new ToolTipItem();
toolTipItem2.Text = "Name: John Doe\nEmail: john@example.com";
toolTipItem2.EnableSeparator = true;
toolTipItem2.Style.SeparatorColor = Color.LightGray;
toolTipItem2.Style.SeparatorStyle = DashStyle.Dot;

// Item 3 (no separator - last item)
ToolTipItem toolTipItem3 = new ToolTipItem();
toolTipItem3.Text = "Status: Active";
toolTipItem3.Style.ForeColor = Color.Green;

toolTipInfo1.Items.AddRange(new ToolTipItem[] { toolTipItem1, toolTipItem2, toolTipItem3 });
sfToolTip1.SetToolTipInfo(this.userButton, toolTipInfo1);
```

## Shadow Effects

Add depth to tooltips with shadow effects.

### Enabling Shadow

Set `ShadowVisible = true` at the `SfToolTip` component level (applies to all tooltips).

```csharp
SfToolTip sfToolTip1 = new SfToolTip();
sfToolTip1.ShadowVisible = true;
```

**Result:** Tooltips display with a subtle shadow for depth perception.

**Use Case:** Enhance visual hierarchy and make tooltips appear to "float" above the interface.

**Example with Shadow:**
```csharp
SfToolTip sfToolTip1 = new SfToolTip();
sfToolTip1.ShadowVisible = true;

ToolTipInfo toolTipInfo = new ToolTipInfo();
toolTipInfo.BorderColor = Color.DarkGray;
toolTipInfo.BorderThickness = 1;

ToolTipItem item = new ToolTipItem();
item.Text = "Tooltip with shadow effect";
item.Style.BackColor = Color.White;

toolTipInfo.Items.Add(item);
sfToolTip1.SetToolTipInfo(this.button1, toolTipInfo);
```

## Right-to-Left Layout

Support right-to-left languages with RTL layout configuration.

### Enabling RTL

Set the `RightToLeft` property on `ToolTipInfo`.

```csharp
ToolTipInfo toolTipInfo1 = new ToolTipInfo();
toolTipInfo1.RightToLeft = RightToLeft.Yes;

ToolTipItem toolTipItem1 = new ToolTipItem();
toolTipItem1.Text = "David Carter\r\nPhone : +1 919.494.1974\r\nEmail : david@syncfusion.com";
toolTipItem1.Style.TextAlignment = ContentAlignment.MiddleLeft;
toolTipItem1.Image = global::GettingStarted.Properties.Resources.MORGK;
toolTipItem1.Style.ImageSize = new Size(100, 100);

toolTipInfo1.Items.AddRange(new ToolTipItem[] { toolTipItem1 });
sfToolTip1.SetToolTipInfo(this.button2, toolTipInfo1);
```

**Effect:**
- Text flows right to left
- Images appear on the right side (when using left alignment)
- Layout mirrors for RTL languages (Arabic, Hebrew, etc.)

**Values:**
- `RightToLeft.Yes` - Enable RTL layout
- `RightToLeft.No` - Standard LTR layout (default)
- `RightToLeft.Inherit` - Inherit from parent control

### RTL with Image Alignment

```csharp
toolTipInfo1.RightToLeft = RightToLeft.Yes;

toolTipItem1.Style.ImageAlignment = ToolTipImageAlignment.Left;
// In RTL mode, "Left" actually positions image on the right
```

**Note:** ImageAlignment values are relative to LTR orientation and are automatically mirrored in RTL mode.

## Customizing Appearance Based on Control

Dynamically adjust tooltip appearance before display using the `ToolTipShowing` event.

```csharp
this.sfToolTip1.ToolTipShowing += SfToolTip1_ToolTipShowing;

private void SfToolTip1_ToolTipShowing(object sender, ToolTipShowingEventArgs e)
{
    if (e.Control is Button)
    {
        e.ToolTipInfo.Items[0].Style.BackColor = Color.LightSkyBlue;
        e.ToolTipInfo.Items[0].Style.ForeColor = Color.Black;
    }
}
```

**Use Case:** Apply different styles based on control type, state, or tags.

**Advanced Example:**
```csharp
private void SfToolTip1_ToolTipShowing(object sender, ToolTipShowingEventArgs e)
{
    // Style based on control state
    if (e.Control is Button button)
    {
        if (!button.Enabled)
        {
            e.ToolTipInfo.Items[0].Style.BackColor = Color.LightGray;
            e.ToolTipInfo.Items[0].Style.ForeColor = Color.DarkGray;
        }
        else if (button.Tag?.ToString() == "Primary")
        {
            e.ToolTipInfo.Items[0].Style.BackColor = Color.LightBlue;
            e.ToolTipInfo.Items[0].Style.ForeColor = Color.DarkBlue;
        }
    }
}
```

## Summary

This guide covered:
- **Border customization:** BorderColor and BorderThickness
- **Item styling:** BackColor, ForeColor, Font, TextAlignment
- **Gradients:** EnableGradientBackground and GradientBrush
- **Separators:** EnableSeparator, SeparatorColor, SeparatorStyle
- **Shadows:** ShadowVisible property
- **RTL support:** RightToLeft property
- **Dynamic styling:** ToolTipShowing event

**Best Practices:**
1. Apply custom styling sparingly for emphasis
2. Ensure text contrast with background colors
3. Test RTL layouts with actual language content
4. Use separators to organize multi-item tooltips
5. Enable shadows for depth in modern interfaces
6. Consider accessibility when choosing colors

**Next Steps:**
- Explore advanced features in [advanced-usage.md](advanced-usage.md)
- Review getting started guide in [getting-started.md](getting-started.md)
