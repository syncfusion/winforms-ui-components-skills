# Bubbles in WinForms Maps

## Table of Contents
- [Overview](#overview)
- [BubbleSetting Properties](#bubblesetting-properties)
- [Adding Bubbles](#adding-bubbles)
- [Range Color Mapping](#range-color-mapping)
- [Customization Options](#customization-options)
- [Advanced Examples](#advanced-examples)

## Overview

Bubbles in the Maps control represent data values bound to geographical locations. They appear as circular markers scattered throughout map shapes, with their size and color representing underlying data values.

**Use Cases:**
- Population density visualization
- Sales volume by region
- Economic indicators
- Disease outbreak tracking
- Event frequency mapping

**Key Features:**
- Size-based value representation
- Color-based range mapping
- Automatic or manual color filling
- Customizable min/max sizes
- Stroke and fill styling

## BubbleSetting Properties

The `BubbleSetting` object on ShapeFileLayer configures all bubble behavior and appearance.

### Core Properties

| Property | Type | Description |
|----------|------|-------------|
| `AutoFillColors` | bool | Automatically generate bubble colors (true) or use manual color mappings (false) |
| `MaxSize` | double | Maximum diameter of largest bubble |
| `MinSize` | double | Minimum diameter of smallest bubble |
| `ValuePath` | string | Property name in ItemSource that determines bubble size |
| `ColorValuePath` | string | Property name in ItemSource used for color mapping |
| `ColorMappings` | ObservableCollection<ColorMapping> | Range-based color definitions |
| `Fill` | Brush | Fill color when AutoFillColors is true |
| `StrokeThickness` | double | Border thickness of bubbles |

### Property Relationships

**Size Calculation:**
```
Bubble Size = MinSize + (MaxSize - MinSize) * (Value / MaxValue)
```

**Color Selection:**
- `AutoFillColors = true`: Uses Fill property for all bubbles
- `AutoFillColors = false`: Uses ColorMappings based on ColorValuePath value

## Adding Bubbles

### Basic Bubble Configuration

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Maps;

private void ConfigureBasicBubbles()
{
    // Prepare data
    MapViewModel model = new MapViewModel();

    // Create shape layer
    ShapeFileLayer shapeLayer = new ShapeFileLayer();
    shapeLayer.Uri = "world1.shp";
    shapeLayer.ItemSource = model.Countries;
    shapeLayer.ShapeIDPath = "NAME";
    shapeLayer.ShapeIDTableField = "NAME";

    // Configure shape appearance
    shapeLayer.ShapeSetting.ShapeValuePath = "Population";
    shapeLayer.ShapeSetting.ShapeColorValuePath = "Population";
    shapeLayer.ShapeSetting.ShapeDisplayValuePath = "NAME";
    shapeLayer.ShapeSetting.ShapeFill = "#E5E5E5";
    shapeLayer.ShapeSetting.ShapeStrokeThickness = 1.5;
    shapeLayer.ShapeSetting.ShapeStroke = "#C1C1C1";

    // Configure bubbles
    shapeLayer.BubbleSetting.AutoFillColors = false;
    shapeLayer.BubbleSetting.MaxSize = 70;
    shapeLayer.BubbleSetting.MinSize = 25;
    shapeLayer.BubbleSetting.ValuePath = "Population";
    shapeLayer.BubbleSetting.ColorValuePath = "Population";

    mapsControl.Layers.Add(shapeLayer);
}
```

### Complete Bubble Example

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Maps;

namespace BubbleMapExample
{
    public partial class Form1 : Form
    {
        private Maps mapsControl1;

        private void Form1_Load(object sender, EventArgs e)
        {
            // Initialize Maps control
            mapsControl1 = new Maps();
            mapsControl1.Dock = DockStyle.Fill;
            mapsControl1.Margin = new Padding(0, 0, 4, 0);
            mapsControl1.MapBackgroundBrush = new SolidBrush(Color.White);
            mapsControl1.MapItemsShape = MapItemShapes.None;

            // Prepare data model
            MapViewModel model = new MapViewModel();

            // Create and configure shape layer
            ShapeFileLayer shapeLayer = new ShapeFileLayer();
            shapeLayer.Uri = "world1.shp";
            shapeLayer.ItemSource = model.Countries;
            shapeLayer.ShapeIDPath = "NAME";
            shapeLayer.ShapeIDTableField = "NAME";

            // Shape styling
            shapeLayer.ShapeSetting.ShapeValuePath = "Population";
            shapeLayer.ShapeSetting.ShapeColorValuePath = "Population";
            shapeLayer.ShapeSetting.ShapeDisplayValuePath = "NAME";
            shapeLayer.ShapeSetting.ShapeFill = "#E5E5E5";
            shapeLayer.ShapeSetting.ShapeStrokeThickness = 1.5;
            shapeLayer.ShapeSetting.ShapeStroke = "#C1C1C1";

            // Bubble configuration
            shapeLayer.BubbleSetting.AutoFillColors = false;
            shapeLayer.BubbleSetting.MaxSize = 70;
            shapeLayer.BubbleSetting.MinSize = 25;
            shapeLayer.BubbleSetting.ValuePath = "Population";
            shapeLayer.BubbleSetting.ColorValuePath = "Population";

            // Add layer
            mapsControl1.Layers.Add(shapeLayer);
            
            // Add to form
            this.Controls.Add(mapsControl1);
        }
    }
}
```

## Range Color Mapping

Range color mapping differentiates bubbles by assigning colors based on value ranges.

### RangeColorMapping Class

```csharp
public class RangeColorMapping : ColorMapping
{
    public double From { get; set; }  // Range start value
    public double To { get; set; }    // Range end value
    public Color Color { get; set; }  // Color for this range
}
```

### Defining Color Ranges

```csharp
// Create color mapping collection
shapeLayer.BubbleSetting.ColorMappings = 
    new System.Collections.ObjectModel.ObservableCollection<ColorMapping>();

// Add range mappings (sorted by value)
shapeLayer.BubbleSetting.ColorMappings.Add(new RangeColorMapping 
{ 
    From = 1000000000, 
    To = 1500000000, 
    Color = Color.FromArgb(127, 32, 188, 238)  // Blue
});

shapeLayer.BubbleSetting.ColorMappings.Add(new RangeColorMapping 
{ 
    From = 500000000, 
    To = 1000000000, 
    Color = Color.FromArgb(127, 167, 206, 56)  // Green
});

shapeLayer.BubbleSetting.ColorMappings.Add(new RangeColorMapping 
{ 
    From = 100000000, 
    To = 500000000, 
    Color = Color.FromArgb(127, 241, 178, 26)  // Orange
});

shapeLayer.BubbleSetting.ColorMappings.Add(new RangeColorMapping 
{ 
    From = 0, 
    To = 100000000, 
    Color = Color.FromArgb(127, 235, 115, 124)  // Red
});
```

**Important:** Set `AutoFillColors = false` to enable range color mapping.

### Complete Range Mapping Example

```csharp
private void Form1_Load(object sender, EventArgs e)
{
    mapsControl1.Dock = DockStyle.Fill;
    mapsControl1.Margin = new Padding(0, 0, 4, 0);
    mapsControl1.MapBackgroundBrush = new SolidBrush(Color.White);
    mapsControl1.MapItemsShape = MapItemShapes.None;

    MapViewModel model = new MapViewModel();

    ShapeFileLayer shapeLayer = new ShapeFileLayer();
    shapeLayer.Uri = "world1.shp";
    shapeLayer.ItemSource = model.Countries;
    shapeLayer.ShapeIDPath = "NAME";
    shapeLayer.ShapeIDTableField = "NAME";

    // Shape configuration
    shapeLayer.ShapeSetting.ShapeValuePath = "Population";
    shapeLayer.ShapeSetting.ShapeColorValuePath = "Population";
    shapeLayer.ShapeSetting.ShapeFill = "#E5E5E5";
    shapeLayer.ShapeSetting.ShapeStrokeThickness = 1.5;
    shapeLayer.ShapeSetting.ShapeStroke = "#C1C1C1";

    // Bubble configuration
    shapeLayer.BubbleSetting.AutoFillColors = false;
    shapeLayer.BubbleSetting.MaxSize = 70;
    shapeLayer.BubbleSetting.MinSize = 25;
    shapeLayer.BubbleSetting.ValuePath = "Population";
    shapeLayer.BubbleSetting.ColorValuePath = "Population";

    // Define color ranges
    shapeLayer.BubbleSetting.ColorMappings = 
        new System.Collections.ObjectModel.ObservableCollection<ColorMapping>();

    shapeLayer.BubbleSetting.ColorMappings.Add(new RangeColorMapping 
    { 
        From = 314623001, 
        To = 1347350000, 
        Color = Color.FromArgb(127, 32, 188, 238) 
    });

    shapeLayer.BubbleSetting.ColorMappings.Add(new RangeColorMapping 
    { 
        From = 143228301, 
        To = 314623001, 
        Color = Color.FromArgb(127, 167, 206, 56) 
    });

    shapeLayer.BubbleSetting.ColorMappings.Add(new RangeColorMapping 
    { 
        From = 82724090, 
        To = 143228301, 
        Color = Color.FromArgb(127, 241, 178, 26) 
    });

    shapeLayer.BubbleSetting.ColorMappings.Add(new RangeColorMapping 
    { 
        From = 22789702, 
        To = 50586757, 
        Color = Color.FromArgb(127, 29, 162, 73) 
    });

    shapeLayer.BubbleSetting.ColorMappings.Add(new RangeColorMapping 
    { 
        From = 0, 
        To = 22789702, 
        Color = Color.FromArgb(127, 235, 115, 124) 
    });

    shapeLayer.BubbleSetting.ColorMappings.Add(new RangeColorMapping 
    { 
        From = 50586757, 
        To = 82724090, 
        Color = Color.FromArgb(127, 237, 45, 149) 
    });

    mapsControl1.Layers.Add(shapeLayer);
}
```

### Handling Out-of-Range Values

**Behavior:**
- Values below the lowest `From`: Rendered in **Black**
- Values above the highest `To`: Rendered in **Black**
- Values within ranges: Use corresponding Color

**Best Practice:**
```csharp
// Cover entire data range
double minValue = countries.Min(c => c.Value);
double maxValue = countries.Max(c => c.Value);

// First range starts at or below minValue
new RangeColorMapping { From = minValue - 1, To = threshold1, Color = color1 }

// Last range ends at or above maxValue
new RangeColorMapping { From = thresholdN, To = maxValue + 1, Color = colorN }
```

## Customization Options

### Bubble Stroke Styling

```csharp
shapeLayer.BubbleSetting.StrokeThickness = 2.0;  // Thicker border
```

Note: Stroke color is typically derived from Fill or ColorMapping.

### Uniform Color Bubbles

```csharp
// Single color for all bubbles
shapeLayer.BubbleSetting.AutoFillColors = true;
shapeLayer.BubbleSetting.Fill = new SolidBrush(Color.FromArgb(150, 0, 120, 215));
```

### Dynamic Size Range

```csharp
// Adjust sizes based on zoom level or data range
double dataRange = maxValue - minValue;

if (dataRange < 1000)
{
    shapeLayer.BubbleSetting.MinSize = 10;
    shapeLayer.BubbleSetting.MaxSize = 30;
}
else
{
    shapeLayer.BubbleSetting.MinSize = 25;
    shapeLayer.BubbleSetting.MaxSize = 70;
}
```

### Semi-Transparent Bubbles

```csharp
// Use alpha channel for transparency
Color bubbleColor = Color.FromArgb(100, 255, 0, 0);  // 100 = ~39% opacity

new RangeColorMapping 
{ 
    From = 0, 
    To = 1000000, 
    Color = bubbleColor 
}
```

## Advanced Examples

### Example 1: Multi-Value Bubbles

Use size for one metric, color for another:

```csharp
shapeLayer.BubbleSetting.ValuePath = "Population";       // Size
shapeLayer.BubbleSetting.ColorValuePath = "GDPPerCapita"; // Color

// Color based on GDP, size based on population
```

### Example 2: Categorical Color Mapping

```csharp
// Define categories with distinct colors
shapeLayer.BubbleSetting.ColorMappings.Add(new RangeColorMapping 
{ 
    From = 0, 
    To = 1, 
    Color = Color.Red,
    Label = "Low"
});

shapeLayer.BubbleSetting.ColorMappings.Add(new RangeColorMapping 
{ 
    From = 1, 
    To = 2, 
    Color = Color.Yellow,
    Label = "Medium"
});

shapeLayer.BubbleSetting.ColorMappings.Add(new RangeColorMapping 
{ 
    From = 2, 
    To = 3, 
    Color = Color.Green,
    Label = "High"
});
```

### Example 3: Graduated Bubble Sizes

```csharp
// Create logarithmic or custom size scaling
public double CalculateBubbleSize(double value)
{
    double minSize = 20;
    double maxSize = 80;
    double minValue = 1000;
    double maxValue = 1000000000;
    
    // Logarithmic scaling
    double logValue = Math.Log10(value);
    double logMin = Math.Log10(minValue);
    double logMax = Math.Log10(maxValue);
    
    double normalizedValue = (logValue - logMin) / (logMax - logMin);
    return minSize + (maxSize - minSize) * normalizedValue;
}
```

Note: WinForms Maps uses linear scaling automatically. For custom scaling, consider preprocessing data values.

## Best Practices

1. **Set AutoFillColors = false** when using ColorMappings
2. **Cover full data range** with color mappings to avoid black bubbles
3. **Use appropriate size range** - too small = invisible, too large = overlapping
4. **Apply alpha channel** for transparency when bubbles overlap
5. **Sort color mappings** by From value for clarity
6. **Match ValuePath and ColorValuePath** to same property for consistent visualization
7. **Test with edge cases** - zero values, negative values, very large values
8. **Consider zoom levels** - adjust bubble sizes based on map zoom