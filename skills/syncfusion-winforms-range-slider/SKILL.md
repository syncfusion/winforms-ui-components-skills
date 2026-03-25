---
name: syncfusion-winforms-range-slider
description: Complete guide for implementing Syncfusion RangeSlider control in Windows Forms for dual-thumb value selection. Use when creating range selection interfaces with SliderMin/SliderMax configuration, ChannelColor customization, or RangeColor styling. Covers range bound configuration, slider appearance customization, event handling, and building range selection UI components.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion Windows Forms Range Slider

Complete guide for implementing the RangeSlider control in Windows Forms applications. The RangeSlider is a flexible UI component that enables value-range selection with dual thumb controls, allowing users to select from a range of values by moving thumbs along a track.

## When to Use This Skill

Use this skill when you need to:
- **Create range selection UI** with dual thumbs for minimum and maximum values
- **Configure range bounds** with minimum and maximum properties
- **Customize visual appearance** with colors, sizes, and tick displays
- **Set initial slider positions** using SliderMin and SliderMax properties
- **Handle range selection events** for user interactions
- **Support different orientations** (horizontal or vertical layouts)
- **Add interactive range controls** to Windows Forms applications

## Component Overview

The RangeSlider control features:
- **Dual thumb controls** for independent minimum/maximum selection
- **Configurable range bounds** from Minimum to Maximum properties
- **Color customization** for channel, range, and thumb elements
- **Tick display** with configurable frequency
- **Horizontal and vertical orientations** for flexible layout
- **Event handling** for scroll and value change interactions
- **Visual highlighting** of selected range between thumbs

## Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly deployment and NuGet installation
- Adding via designer
- Adding via code (C# and VB.NET examples)
- Basic configuration with minimum/maximum values

### Value Configuration
📄 **Read:** [references/value-configuration.md](references/value-configuration.md)
- Setting SliderMin and SliderMax positions
- Configuring Minimum and Maximum bounds
- Understanding Range property
- Practical value setup examples

### Interactive Features
📄 **Read:** [references/interactive-features.md](references/interactive-features.md)
- Channel color and height customization
- Range color highlighting
- Thumb color configuration
- Slider size adjustment
- Tick display and frequency control
- Event handling (Scroll and ValueChanged events)

### Layout and Orientation
📄 **Read:** [references/layout-orientation.md](references/layout-orientation.md)
- Horizontal vs vertical orientation
- Right-to-left (RTL) support
- Orientation property usage
- Layout considerations and best practices

## Quick Start Example

Here's a minimal example to get started with RangeSlider:

```csharp
using Syncfusion.Windows.Forms.Tools;
using System.Windows.Forms;

public partial class Form1 : Form
{
    public Form1()
    {
        InitializeComponent();
        
        // Create RangeSlider instance
        RangeSlider rangeSlider = new RangeSlider();
        
        // Configure range bounds
        rangeSlider.Minimum = 0;
        rangeSlider.Maximum = 100;
        
        // Set initial thumb positions
        rangeSlider.SliderMin = 20;
        rangeSlider.SliderMax = 80;
        
        // Show value labels
        rangeSlider.ShowLabels = true;
        
        // Display ticks
        rangeSlider.ShowTicks = true;
        rangeSlider.TickFrequency = 10;
        
        // Add to form
        this.Controls.Add(rangeSlider);
    }
}
```

## Common Patterns

### Pattern 1: Price Range Filter
Create a price range selector with custom formatting:

```csharp
rangeSlider.Minimum = 0;
rangeSlider.Maximum = 1000;
rangeSlider.SliderMin = 100;
rangeSlider.SliderMax = 900;

rangeSlider.ValueChanged += (s, e) =>
{
    decimal minPrice = rangeSlider.SliderMin;
    decimal maxPrice = rangeSlider.SliderMax;
    // Update product list based on price range
};
```

### Pattern 2: Custom Visual Styling
Customize colors for visual feedback:

```csharp
// Set distinct colors for visual hierarchy
rangeSlider.ChannelColor = Color.LightGray;      // Background track
rangeSlider.RangeColor = Color.DarkGreen;        // Selected range
rangeSlider.ThumbColor = Color.Green;            // Thumb controls

// Adjust sizing
rangeSlider.ChannelHeight = 6;
rangeSlider.SliderSize = new Size(12, 18);
```

### Pattern 3: Vertical Layout
Create vertical range sliders for specific layouts:

```csharp
rangeSlider.Orientation = Orientation.Vertical;
rangeSlider.Height = 300;  // Sufficient height for vertical orientation
rangeSlider.Width = 50;    // Narrower for vertical
```

### Pattern 4: Event-Driven Updates
Handle user interactions with events:

```csharp
rangeSlider.Scroll += (s, e) =>
{
    // Called while user is dragging
    UpdatePreview();
};

rangeSlider.ValueChanged += (s, e) =>
{
    // Called after value change completes
    UpdateData();
};
```

## Key Properties Reference

| Property | Type | Purpose |
|----------|------|---------|
| `Minimum` | int | Defines the lower bound of range |
| `Maximum` | int | Defines the upper bound of range |
| `SliderMin` | int | Position of left/top thumb (current minimum) |
| `SliderMax` | int | Position of right/bottom thumb (current maximum) |
| `Range` | range structure | Gets current selected range |
| `ChannelColor` | Color | Background track color |
| `RangeColor` | Color | Highlighted selected range color |
| `ThumbColor` | Color | Thumb control color |
| `ChannelHeight` | int | Height of track in pixels |
| `SliderSize` | Size | Dimensions of thumb controls |
| `ShowTicks` | bool | Display tick marks |
| `TickFrequency` | int | Interval between ticks |
| `Orientation` | Orientation | Horizontal or Vertical layout |
| `ShowLabels` | bool | Display value labels |
| `RightToLeft` | RightToLeft | RTL layout support |

## Design Patterns

### Responsive Range Selection
Create controls that adapt to user needs:

```csharp
public void ConfigureRangeSlider(int dataMin, int dataMax, int defaultMin, int defaultMax)
{
    rangeSlider.Minimum = dataMin;
    rangeSlider.Maximum = dataMax;
    rangeSlider.SliderMin = defaultMin;
    rangeSlider.SliderMax = defaultMax;
    
    // Calculate appropriate tick frequency
    int range = dataMax - dataMin;
    rangeSlider.TickFrequency = range > 1000 ? 100 : range > 100 ? 10 : 1;
}
```

### Validation Pattern
Ensure valid range selections:

```csharp
rangeSlider.ValueChanged += (s, e) =>
{
    if (rangeSlider.SliderMin > rangeSlider.SliderMax)
    {
        // Swap if needed
        int temp = rangeSlider.SliderMin;
        rangeSlider.SliderMin = rangeSlider.SliderMax;
        rangeSlider.SliderMax = temp;
    }
};
```

## Common Use Cases

### Price Range Filtering
Enable users to filter products by selecting a price range, commonly used in e-commerce applications.

### Date Range Selection
Create date range pickers by mapping slider values to date ranges for filtering historical data.

### Parameter Calibration
Allow configuration of minimum and maximum parameters in scientific or industrial applications.

### Volume Control
Use range sliders for audio applications to set volume range or frequency bands.

### Data Analysis
Filter large datasets by selecting value ranges for visualization and analysis.

## Best Practices

1. **Set clear bounds** - Always define Minimum and Maximum before adding to form
2. **Provide visual feedback** - Use colors to indicate selected range clearly
3. **Display ticks and labels** - Help users understand scale and current values
4. **Handle both events** - Use Scroll for preview, ValueChanged for final updates
5. **Responsive design** - Adjust TickFrequency based on range size
6. **Consider orientation** - Choose horizontal for compact layouts, vertical for specialized UIs

---

**Next Steps:** Read the appropriate reference guide from the Navigation Guide above based on your specific implementation needs.
