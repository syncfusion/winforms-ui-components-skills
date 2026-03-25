# Legends in WinForms Maps

## Table of Contents
- [Overview](#overview)
- [Legend Visibility](#legend-visibility)
- [Legend Positioning](#legend-positioning)
- [Legend Header](#legend-header)
- [Legend Categories](#legend-categories)
- [Legend Shapes](#legend-shapes)
- [Complete Examples](#complete-examples)

## Overview

A legend is a key displayed on a map that contains swatches of symbols with descriptions. Legends interpret what the map displays through colors, shapes, or other identifiers based on underlying data.

**Purpose:**
- Explain color-coded regions
- Show value ranges
- Display bubble size meanings
- Provide data classification guide

**Key Features:**
- Customizable positioning
- Layer and bubble legend types
- Various icon shapes
- Headers and titles
- Size and style configuration

## Legend Visibility

Legends are displayed only when explicitly enabled using the `ShowLegend` property.

### Enabling Legend

```csharp
ShapeFileLayer shapeLayer = new ShapeFileLayer();
shapeLayer.Uri = "world1.shp";

// Enable legend
shapeLayer.LegendSetting = new LegendSettings()
{
    ShowLegend = true
};

mapsControl.Layers.Add(shapeLayer);
```

**Default:** `ShowLegend = false` (legend hidden)

## Legend Positioning

### Position Properties

| Property | Type | Description |
|----------|------|-------------|
| `Position` | LegendPosition | Standard position (TopLeft, TopRight, BottomLeft, BottomRight, Default) |
| `PositionX` | double | Horizontal margin from left edge |
| `PositionY` | double | Vertical margin from top edge |

### Standard Positions

```csharp
// Position legend in top-right corner
shapeLayer.LegendSetting = new LegendSettings()
{
    ShowLegend = true,
    Position = LegendPosition.TopRight
};
```

**Available Positions:**
- `LegendPosition.TopLeft`
- `LegendPosition.TopRight`
- `LegendPosition.TopCenter`
- `LegendPosition.BottomLeft`
- `LegendPosition.BottomRight`
- `LegendPosition.BottomCenter`
- `LegendPosition.MidLeft`
- `LegendPosition.MidRight`
- `LegendPosition.Center`
- `LegendPosition.Default` - Uses PositionX and PositionY

### Custom Positioning with Margins

When `Position = Default`, use PositionX and PositionY for precise positioning:

```csharp
shapeLayer.LegendSetting = new LegendSettings()
{
    ShowLegend = true,
    Position = LegendPosition.Default,
    PositionX = 30,   // 30 pixels from left
    PositionY = 180   // 180 pixels from top
};
```

**Note:** PositionX and PositionY are only respected when `Position = Default`.

## Legend Header

Add descriptive text above the legend using the `Title` property.

```csharp
shapeLayer.LegendSetting = new LegendSettings()
{
    ShowLegend = true,
    Title = "Population Density",
    PositionX = 30,
    PositionY = 180
};
```

**Title Properties:**
- String type
- Displayed above legend items
- Supports basic text (no formatting)

## Legend Categories

Legends are categorized into two types based on what they represent.

### LegendType Enumeration

```csharp
public enum LegendType
{
    Layers,   // Legend for shape layers
    Bubbles   // Legend for bubble visualization
}
```

### Layer Legends

Show color mappings for shape fills.

```csharp
shapeLayer.LegendSetting = new LegendSettings()
{
    ShowLegend = true,
    LegendType = LegendType.Layers
};

// Define shape color mappings
shapeLayer.ShapeSetting.FillSetting.ColorMappings = 
    new ObservableCollection<ColorMapping>
    {
        new RangeColorMapping 
        { 
            From = 0, 
            To = 750000000, 
            Color = Color.Aqua,
            Label = "0-750M"
        },
        new RangeColorMapping 
        { 
            From = 750000000, 
            To = 1500000000, 
            Color = Color.Coral,
            Label = "750M-1.5B"
        }
    };
```

### Bubble Legends

Show size ranges for bubbles.

```csharp
shapeLayer.LegendSetting = new LegendSettings()
{
    ShowLegend = true,
    LegendType = LegendType.Bubbles
};

// Configure bubbles
shapeLayer.BubbleSetting.MinSize = 25;
shapeLayer.BubbleSetting.MaxSize = 70;
shapeLayer.BubbleSetting.ValuePath = "Population";
```

**Bubble Legend Characteristics:**
- Always displayed as circles
- Size varies based on BubbleSetting.MinSize and MaxSize
- Size ratio determines legend bubble sizes
- Colors reflect bubble ColorMappings

## Legend Shapes

For layer-type legends, customize the icon shape displayed for each range.

### LegendIcons Enumeration

```csharp
public enum LegendIcons
{
    Ellipse,
    Rectangle
}
```

### Configuring Legend Icon

```csharp
shapeLayer.LegendSetting = new LegendSettings()
{
    ShowLegend = true,
    LegendType = LegendType.Layers,
    Icon = LegendIcons.Rectangle,  // Show rectangles
    Size = new Size(15, 15)              // Icon size
};
```

**Note:** Bubble legends always use circles regardless of LegendIcon setting.

### Size Property

Control the dimensions of legend icons:

```csharp
shapeLayer.LegendSetting = new LegendSettings()
{
    ShowLegend = true,
    Size = new Size(20, 20)  // Width, Height in pixels
};
```

## Complete Examples

### Example 1: Basic Layer Legend

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Maps;
using System.Collections.ObjectModel;

private void CreateMapWithLegend()
{
    mapsControl1.Dock = DockStyle.Fill;
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
    shapeLayer.ShapeSetting.ShapeDisplayValuePath = "NAME";
    shapeLayer.ShapeSetting.TextForeground = "Black";
    shapeLayer.ShowMapItem = false;
    shapeLayer.ShowToolTip = true;
    shapeLayer.ShapeSetting.ShapeFill = "#E5E5E5";
    shapeLayer.ShapeSetting.ShapeStrokeThickness = 1.5;
    shapeLayer.ShapeSetting.ShapeStroke = "Black";

    // Color mapping configuration
    shapeLayer.ShapeSetting.FillSetting.AutoFillColors = false;
    shapeLayer.ShapeSetting.FillSetting.ColorMappings = 
        new ObservableCollection<ColorMapping>
        {
            new RangeColorMapping 
            { 
                From = 750000000, 
                To = 1500000000, 
                Color = Color.Coral,
                Label = "750M - 1.5B"
            },
            new RangeColorMapping 
            { 
                From = 0, 
                To = 750000000, 
                Color = Color.Aqua,
                Label = "0 - 750M"
            },
            new RangeColorMapping 
            { 
                From = 0, 
                To = 0, 
                Color = Color.CadetBlue,
                Label = "No Data"
            }
        };

    // Legend configuration
    shapeLayer.LegendSetting = new LegendSettings()
    {
        ShowLegend = true,
        Size = new Size(15, 15),
        PositionX = 30,
        PositionY = 180,
        Title = "Population"
    };

    mapsControl1.Layers.Add(shapeLayer);
}
```

### Example 2: Bubble Legend with Custom Position

```csharp
private void CreateBubbleMapWithLegend()
{
    ShapeFileLayer shapeLayer = new ShapeFileLayer();
    shapeLayer.Uri = "world1.shp";
    shapeLayer.ItemSource = GetCountries();
    shapeLayer.ShapeIDPath = "NAME";
    shapeLayer.ShapeIDTableField = "NAME";

    // Bubble configuration
    shapeLayer.BubbleSetting.AutoFillColors = false;
    shapeLayer.BubbleSetting.MaxSize = 70;
    shapeLayer.BubbleSetting.MinSize = 25;
    shapeLayer.BubbleSetting.ValuePath = "Population";
    shapeLayer.BubbleSetting.ColorValuePath = "Population";

    // Bubble color mappings
    shapeLayer.BubbleSetting.ColorMappings = 
        new ObservableCollection<ColorMapping>
        {
            new RangeColorMapping 
            { 
                From = 1000000000, 
                To = 2000000000, 
                Color = Color.FromArgb(127, 32, 188, 238),
                Label = "> 1 Billion"
            },
            new RangeColorMapping 
            { 
                From = 100000000, 
                To = 1000000000, 
                Color = Color.FromArgb(127, 241, 178, 26),
                Label = "100M - 1B"
            },
            new RangeColorMapping 
            { 
                From = 0, 
                To = 100000000, 
                Color = Color.FromArgb(127, 235, 115, 124),
                Label = "< 100M"
            }
        };

    // Bubble legend configuration
    shapeLayer.LegendSetting = new LegendSettings()
    {
        ShowLegend = true,
        LegendType = LegendType.Bubbles,
        Position = LegendPosition.BottomRight,
        Title = "Population"
    };

    mapsControl1.Layers.Add(shapeLayer);
}
```

### Example 3: Multiple Legends (Not Directly Supported)

WinForms Maps supports one legend per layer. For multiple legends:

```csharp
// Layer 1 with its legend
ShapeFileLayer layer1 = new ShapeFileLayer();
layer1.Uri = "world1.shp";
layer1.LegendSetting = new LegendSettings()
{
    ShowLegend = true,
    Position = LegendPosition.TopLeft,
    Title = "Population"
};

// Layer 2 with its legend
ShapeFileLayer layer2 = new ShapeFileLayer();
layer2.Uri = "cities.shp";
layer2.LegendSetting = new LegendSettings()
{
    ShowLegend = true,
    Position = LegendPosition.TopRight,
    Title = "Cities"
};

// Add both layers - each displays its own legend
mapsControl1.Layers.Add(layer1);
mapsControl1.Layers.Add(layer2);
```

## Styling and Customization

### Legend Background and Border

Legend styling is typically controlled through the Maps control theme or custom rendering. Basic properties:

```csharp
shapeLayer.LegendSetting = new LegendSettings()
{
    ShowLegend = true,
    Size = new Size(20, 20),
    // Additional styling through theme or custom rendering
};
```

### Aligning Legend Items

Legend items are automatically arranged vertically. To control spacing:

```csharp
// Adjust icon size to change spacing
shapeLayer.LegendSetting.Size = new Size(15, 15);  // Compact
// vs
shapeLayer.LegendSetting.Size = new Size(25, 25);  // Spacious
```

## Best Practices

1. **Always set ShowLegend = true** - Legends are hidden by default
2. **Use descriptive titles** - Help users understand what the legend represents
3. **Match LegendType** - Use Layers for shape fills, Bubbles for bubble visualization
4. **Position carefully** - Avoid obscuring important map regions
5. **Keep it simple** - 3-7 legend items for readability
6. **Use meaningful labels** - Add Label property to ColorMapping for clarity
7. **Test with data** - Ensure all data ranges are represented in legend
8. **Consider zoom** - Position legend where it remains visible at all zoom levels

## Troubleshooting

**Issue: Legend not appearing**
- Verify `ShowLegend = true`
- Check that ColorMappings are defined
- Ensure layer has data bound

**Issue: Legend in wrong position**
- Set `Position = Default` to use PositionX/PositionY
- Standard positions may not work with custom layouts

**Issue: Legend colors don't match map**
- Verify `AutoFillColors = false` for manual ColorMappings
- Check that ColorMappings match ShapeSetting or BubbleSetting configuration

**Issue: Bubble legend shows wrong sizes**
- Check BubbleSetting.MinSize and MaxSize values
- Verify ValuePath points to correct property

## Integration with Other Features

### Legend + Tooltip

```csharp
shapeLayer.ShowToolTip = true;
shapeLayer.LegendSetting = new LegendSettings() { ShowLegend = true };
// Users can see category in legend, details in tooltip
```

### Legend + Selection

```csharp
shapeLayer.EnableSelection = true;
shapeLayer.LegendSetting = new LegendSettings() { ShowLegend = true };
// Legend helps users understand selected shape colors
```

### Legend + Zoom

Legends remain fixed during zoom operations, maintaining reference for users navigating the map.