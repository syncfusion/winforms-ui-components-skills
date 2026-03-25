# Customization in WinForms Maps

## Table of Contents
- [Overview](#overview)
- [Map Background](#map-background)
- [Shape Customization](#shape-customization)
- [Color Mapping](#color-mapping)
  - [Equal Color Mapping](#equal-color-mapping)
  - [Range Color Mapping](#range-color-mapping)
- [Fill Settings](#fill-settings)
- [Shape Value Paths](#shape-value-paths)
- [Advanced Styling](#advanced-styling)

## Overview

The WinForms Maps control provides extensive customization options to create visually appealing and meaningful geographical visualizations. Customization covers:

- Map background and container styling
- Shape fill colors and borders
- Data-driven color mapping
- Value-based visualization
- Theme integration

## Map Background

### MapBackgroundBrush Property

Controls the background color of the entire map control.

```csharp
mapsControl1.MapBackgroundBrush = new SolidBrush(Color.White);
```

**Common Backgrounds:**

```csharp
// White background (clean, professional)
mapsControl1.MapBackgroundBrush = new SolidBrush(Color.White);

// Light blue (ocean effect)
mapsControl1.MapBackgroundBrush = new SolidBrush(Color.FromArgb(173, 216, 230));

// Dark theme
mapsControl1.MapBackgroundBrush = new SolidBrush(Color.FromArgb(30, 30, 30));

// Gradient background
LinearGradientBrush gradient = new LinearGradientBrush(
    new Rectangle(0, 0, mapsControl1.Width, mapsControl1.Height),
    Color.LightBlue,
    Color.White,
    LinearGradientMode.Vertical
);
mapsControl1.MapBackgroundBrush = gradient;
```

### MapItemsShape Property

Defines the shape type for map items (markers).

```csharp
mapsControl1.MapItemsShape = MapItemShapes.None;
```

**Available Shapes:**
- `MapItemShapes.None`: No predefined markers
- `MapItemShapes.Circle`: Circular markers
- `MapItemShapes.Diamond`: Diamond-shaped markers
- `MapItemShapes.Rectangle`: Rectangular markers
- `MapItemShapes.Triangle`: Triangular markers

## Shape Customization

### Basic Shape Properties

The `ShapeSetting` object on `ShapeFileLayer` controls shape appearance.

#### ShapeFill

Defines the fill color for all shapes in the layer.

```csharp
shapeLayer.ShapeSetting.ShapeFill = "#E5E5E5";  // Hex color
```

#### ShapeStroke

Defines the border color for shapes.

```csharp
shapeLayer.ShapeSetting.ShapeStroke = "#C1C1C1";  // Hex color
```

#### ShapeStrokeThickness

Sets the border width in pixels.

```csharp
shapeLayer.ShapeSetting.ShapeStrokeThickness = 1.5;  // 1.5 pixels
```

### Complete Shape Styling Example

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Maps;

private void CustomizeShapeAppearance()
{
    mapsControl1.Dock = DockStyle.Fill;
    mapsControl1.Margin = new Padding(0, 0, 4, 0);
    mapsControl1.MapBackgroundBrush = new SolidBrush(Color.White);
    mapsControl1.MapItemsShape = MapItemShapes.None;

    ShapeFileLayer shapeLayer = new ShapeFileLayer();
    shapeLayer.Uri = "world1.shp";
    
    // Shape styling
    shapeLayer.ShapeSetting.ShapeFill = "#E5E5E5";           // Light gray fill
    shapeLayer.ShapeSetting.ShapeStroke = "#C1C1C1";         // Medium gray border
    shapeLayer.ShapeSetting.ShapeStrokeThickness = 1.5;      // 1.5px border

    mapsControl1.Layers.Add(shapeLayer);
}
```

## Color Mapping

Color mapping allows automatic shape coloring based on underlying data values, providing visual data representation.

### Color Mapping Types

1. **Equal Color Mapping**: Exact value matches
2. **Range Color Mapping**: Value ranges

### AutoFillColors Property

Controls whether colors are assigned automatically or manually.

```csharp
shapeLayer.ShapeSetting.FillSetting.AutoFillColors = false;
// false = Use manual ColorMappings
// true = Automatic color generation
```

**Important:** Set `AutoFillColors = false` to use custom ColorMappings.

## Equal Color Mapping

Equal color mapping assigns specific colors to shapes based on exact value matches.

### Use Cases

- Political maps (party affiliation)
- Category-based visualization
- Binary classifications
- Named groups

### EqualColorMapping Class

```csharp
public class EqualColorMapping : ColorMapping
{
    public object Value { get; set; }  // Exact value to match
    public Color Color { get; set; }   // Color for this value
}
```

### Equal Mapping Example

```csharp
using System;
using System.Drawing;
using System.Collections.ObjectModel;
using Syncfusion.Windows.Forms.Maps;

private void CreateElectionMap()
{
    mapsControl1.Dock = DockStyle.Fill;
    mapsControl1.MapBackgroundBrush = new SolidBrush(Color.White);
    mapsControl1.MapItemsShape = MapItemShapes.None;

    ViewModel model = new ViewModel();

    ShapeFileLayer shapeLayer = new ShapeFileLayer();
    shapeLayer.Uri = "usa_state.shp";
    shapeLayer.ItemSource = model.Data;
    shapeLayer.ShapeIDPath = "State";
    shapeLayer.ShapeIDTableField = "STATE_NAME";
    
    // Value to color mapping
    shapeLayer.ShapeSetting.ShapeColorValuePath = "Candidate";
    shapeLayer.ShapeSetting.ShapeStrokeThickness = 1.5;
    
    // Disable auto-fill
    shapeLayer.ShapeSetting.FillSetting.AutoFillColors = false;
    
    // Define equal color mappings
    shapeLayer.ShapeSetting.FillSetting.ColorMappings = 
        new ObservableCollection<ColorMapping>();
    
    shapeLayer.ShapeSetting.FillSetting.ColorMappings.Add(
        new EqualColorMapping 
        { 
            Color = Color.FromArgb(216, 68, 68),  // Red
            Value = "Romney" 
        }
    );
    
    shapeLayer.ShapeSetting.FillSetting.ColorMappings.Add(
        new EqualColorMapping 
        { 
            Color = Color.FromArgb(49, 109, 181),  // Blue
            Value = "Obama" 
        }
    );

    mapsControl1.Layers.Add(shapeLayer);
}
```

**Result:** States are colored red or blue based on the "Candidate" field value.

## Range Color Mapping

Range color mapping assigns colors to shapes based on numeric value ranges, ideal for continuous data visualization.

### Use Cases

- Population density
- Temperature maps
- Income levels
- Sales territories
- Statistical data

### RangeColorMapping Class

```csharp
public class RangeColorMapping : ColorMapping
{
    public double From { get; set; }  // Range start (inclusive)
    public double To { get; set; }    // Range end (inclusive)
    public Color Color { get; set; }  // Color for this range
    public string Label { get; set; } // Optional label for legend
}
```

### Range Mapping Example

```csharp
using System;
using System.Drawing;
using System.Collections.ObjectModel;
using Syncfusion.Windows.Forms.Maps;

private void CreatePopulationMap()
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
    
    // Specify which property determines color
    shapeLayer.ShapeSetting.ShapeColorValuePath = "Population";
    
    // Base styling
    shapeLayer.ShapeSetting.ShapeFill = "#E5E5E5";
    shapeLayer.ShapeSetting.ShapeStrokeThickness = 1.5;
    shapeLayer.ShapeSetting.ShapeStroke = "#C1C1C1";
    
    // Disable auto-fill colors
    shapeLayer.ShapeSetting.FillSetting.AutoFillColors = false;
    
    // Define range color mappings
    shapeLayer.ShapeSetting.FillSetting.ColorMappings = 
        new ObservableCollection<ColorMapping>();
    
    shapeLayer.ShapeSetting.FillSetting.ColorMappings.Add(
        new RangeColorMapping 
        { 
            From = 750000000, 
            To = 1500000000, 
            Color = Color.FromArgb(42, 145, 207),  // Dark blue
            Label = "750M - 1.5B"
        }
    );
    
    shapeLayer.ShapeSetting.FillSetting.ColorMappings.Add(
        new RangeColorMapping 
        { 
            From = 0, 
            To = 750000000, 
            Color = Color.FromArgb(61, 159, 216),  // Light blue
            Label = "0 - 750M"
        }
    );
    
    shapeLayer.ShapeSetting.FillSetting.ColorMappings.Add(
        new RangeColorMapping 
        { 
            From = 0, 
            To = 0, 
            Color = Color.FromArgb(199, 233, 250),  // Very light blue
            Label = "No Data"
        }
    );

    mapsControl1.Layers.Add(shapeLayer);
}
```

### Multi-Range Color Gradient

Create color gradients for more detailed visualizations:

```csharp
shapeLayer.ShapeSetting.FillSetting.ColorMappings = 
    new ObservableCollection<ColorMapping>
    {
        // 5-tier color scheme
        new RangeColorMapping 
        { 
            From = 1000000000, 
            To = 2000000000, 
            Color = Color.FromArgb(8, 48, 107),      // Darkest
            Label = "1B+"
        },
        new RangeColorMapping 
        { 
            From = 500000000, 
            To = 1000000000, 
            Color = Color.FromArgb(33, 102, 172),
            Label = "500M - 1B"
        },
        new RangeColorMapping 
        { 
            From = 100000000, 
            To = 500000000, 
            Color = Color.FromArgb(67, 147, 195),
            Label = "100M - 500M"
        },
        new RangeColorMapping 
        { 
            From = 10000000, 
            To = 100000000, 
            Color = Color.FromArgb(146, 197, 222),
            Label = "10M - 100M"
        },
        new RangeColorMapping 
        { 
            From = 0, 
            To = 10000000, 
            Color = Color.FromArgb(209, 229, 240),   // Lightest
            Label = "< 10M"
        }
    };
```

## Fill Settings

The `FillSetting` object provides additional customization options.

### FillSetting Properties

| Property | Type | Description |
|----------|------|-------------|
| `AutoFillColors` | bool | Auto-generate colors vs manual mapping |
| `ColorMappings` | Collection | Equal or Range color mappings |

### Example: Toggle Auto vs Manual

```csharp
// Automatic colors (random/generated)
shapeLayer.ShapeSetting.FillSetting.AutoFillColors = true;

// Manual colors (using ColorMappings)
shapeLayer.ShapeSetting.FillSetting.AutoFillColors = false;
shapeLayer.ShapeSetting.FillSetting.ColorMappings = myColorMappings;
```

## Shape Value Paths

Value paths connect shape properties to data model properties.

### Key Value Path Properties

#### ShapeValuePath

Property used for general shape values (tooltips, calculations).

```csharp
shapeLayer.ShapeSetting.ShapeValuePath = "Population";
```

#### ShapeColorValuePath

Property used to determine shape color via ColorMappings.

```csharp
shapeLayer.ShapeSetting.ShapeColorValuePath = "Population";
```

#### ShapeDisplayValuePath

Property displayed in tooltips and labels.

```csharp
shapeLayer.ShapeSetting.ShapeDisplayValuePath = "NAME";
```

### Value Path Example

```csharp
// Data model
public class Country
{
    public string NAME { get; set; }
    public double Population { get; set; }
    public double GDP { get; set; }
}

// Configuration
shapeLayer.ShapeSetting.ShapeValuePath = "GDP";               // Use GDP for values
shapeLayer.ShapeSetting.ShapeColorValuePath = "Population";   // Color by population
shapeLayer.ShapeSetting.ShapeDisplayValuePath = "NAME";       // Show name in tooltips
```

## Advanced Styling

### Theme Integration

Apply consistent color schemes:

```csharp
// Define color palette
Color[] palette = new Color[]
{
    Color.FromArgb(227, 26, 28),
    Color.FromArgb(51, 160, 44),
    Color.FromArgb(31, 120, 180),
    Color.FromArgb(255, 127, 0),
    Color.FromArgb(106, 61, 154)
};

// Apply to mappings
for (int i = 0; i < dataCategories.Count; i++)
{
    shapeLayer.ShapeSetting.FillSetting.ColorMappings.Add(
        new EqualColorMapping
        {
            Value = dataCategories[i],
            Color = palette[i % palette.Length]
        }
    );
}
```

### Conditional Styling

Apply different styles based on data conditions:

```csharp
private void ApplyConditionalStyling(ShapeFileLayer layer, List<Country> countries)
{
    layer.ShapeSetting.FillSetting.ColorMappings.Clear();
    
    // Determine thresholds
    double averagePopulation = countries.Average(c => c.Population);
    
    layer.ShapeSetting.FillSetting.ColorMappings.Add(
        new RangeColorMapping
        {
            From = averagePopulation,
            To = double.MaxValue,
            Color = Color.DarkGreen,  // Above average
            Label = "Above Average"
        }
    );
    
    layer.ShapeSetting.FillSetting.ColorMappings.Add(
        new RangeColorMapping
        {
            From = 0,
            To = averagePopulation,
            Color = Color.LightGreen,  // Below average
            Label = "Below Average"
        }
    );
}
```

### Layered Styling

Combine multiple layers with different styles:

```csharp
// Base layer - Subtle styling
ShapeFileLayer baseLayer = new ShapeFileLayer();
baseLayer.Uri = "world.shp";
baseLayer.ShapeSetting.ShapeFill = "#F5F5F5";
baseLayer.ShapeSetting.ShapeStroke = "#E0E0E0";
baseLayer.ShapeSetting.ShapeStrokeThickness = 0.5;

// Highlight layer - Bold styling
SubShapeFileLayer highlightLayer = new SubShapeFileLayer();
highlightLayer.Uri = "selected_countries.shp";
highlightLayer.ShapeSetting.ShapeFill = "#FF6B6B";
highlightLayer.ShapeSetting.ShapeStroke = "#C92A2A";
highlightLayer.ShapeSetting.ShapeStrokeThickness = 2.0;

baseLayer.SubShapeFileLayers.Add(highlightLayer);
mapsControl.Layers.Add(baseLayer);
```

## Best Practices

1. **Set AutoFillColors = false** when using ColorMappings
2. **Cover full data range** with range mappings to avoid unmapped values
3. **Use meaningful colors** - intuitive associations (red = high, green = low)
4. **Maintain contrast** - Ensure shapes are distinguishable from background
5. **Consistent stroke thickness** - Uniform borders look more professional
6. **Test color blindness** - Use color-blind friendly palettes
7. **Label ranges clearly** - Add labels to ColorMappings for legends
8. **Sort ranges logically** - Low to high for intuitive understanding

## Troubleshooting

**Issue: Colors not applying**
- Verify `AutoFillColors = false`
- Check that `ShapeColorValuePath` matches a property in your data model
- Ensure ColorMappings collection is not empty

**Issue: Shapes show default color**
- Verify value falls within defined ranges
- Check that ShapeIDPath/ShapeIDTableField mapping is correct
- Ensure data is bound properly

**Issue: Borders not visible**
- Increase `ShapeStrokeThickness`
- Check `ShapeStroke` color contrasts with `ShapeFill`
- Verify shape file contains valid geometry

**Issue: All shapes same color**
- Verify `ShapeColorValuePath` points to varying data
- Check that ColorMappings cover the data value range
- Ensure AutoFillColors is false when using manual mappings