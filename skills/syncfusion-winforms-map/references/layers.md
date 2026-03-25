# Layers in WinForms Maps

## Table of Contents
- [Overview](#overview)
- [ShapeFileLayer](#shapefilelayer)
- [MultiLayer Support](#multilayer-support)
- [LayoutType](#layouttype)
- [Data Binding](#data-binding)
- [Shape ID Mapping](#shape-id-mapping)
- [Advanced Scenarios](#advanced-scenarios)

## Overview

Maps are maintained through layers, and a map can accommodate one or more layers. Layers provide the foundation for rendering geographical shapes and binding data to those shapes.

**Key Concepts:**
- **ShapeFileLayer**: Primary layer that loads and renders shape files
- **SubShapeFileLayer**: Additional layers for highlighting or overlaying regions
- **Layer Collection**: Maps can contain multiple independent layers
- **Layer Hierarchy**: Main layer → SubShapeFileLayers → Shapes

## ShapeFileLayer

The ShapeFileLayer is the core layer of the map that loads ESRI shape files and renders geographical shapes.

### Basic ShapeFileLayer Configuration

```csharp
ShapeFileLayer shapeLayer = new ShapeFileLayer();

// Required: Shape file reference
shapeLayer.Uri = "world1.shp";

// Add to Maps control
mapsControl.Layers.Add(shapeLayer);
```

### Key ShapeFileLayer Properties

| Property | Type | Description |
|----------|------|-------------|
| `Uri` | string | Path to shape file (.shp) |
| `ItemSource` | object | Data source for binding |
| `ShapeIDPath` | string | Property name in data model |
| `ShapeIDTableField` | string | Column name in .dbf file |
| `LayoutType` | LayoutType | Default or Tile projection |
| `SubShapeFileLayers` | Collection | Additional overlay layers |
| `ShapeSetting` | ShapeSetting | Appearance and behavior settings |

## MultiLayer Support

MultiLayer support allows loading multiple shape files in a single container, enabling maps to display more information by combining different geographical data sets.

### Why Use Multiple Layers?

**Use Cases:**
- Highlight specific regions on a base map
- Overlay different data types (boundaries, routes, points)
- Show before/after comparisons
- Display hierarchical geographical data
- Combine global and regional maps

### Adding Multiple Layers with SubShapeFileLayer

SubShapeFileLayer is a collection that contains additional shape layers rendered on top of the main ShapeFileLayer.

**Important:** SubShapeFileLayers must be added to the main ShapeFileLayer, not directly to Maps.Layers.

```csharp
// Create main world layer
ShapeFileLayer worldLayer = new ShapeFileLayer();
worldLayer.Uri = "world1.shp";
worldLayer.ShapeSetting.ShapeFill = "#E5E5E5";
worldLayer.ShapeSetting.ShapeStroke = "#C1C1C1";
worldLayer.ShapeSetting.ShapeStrokeThickness = 0.5;

// Create sublayer for Africa
SubShapeFileLayer africaLayer = new SubShapeFileLayer();
africaLayer.Uri = "Africa.shp";
africaLayer.ShapeSetting.ShapeFill = "#8DCEFF";
africaLayer.ShapeSetting.ShapeStroke = "#2F8CEA";
africaLayer.ShapeSetting.ShapeStrokeThickness = 0.5;

// Create sublayer for Australia
SubShapeFileLayer australiaLayer = new SubShapeFileLayer();
australiaLayer.Uri = "australia.shp";
australiaLayer.ShapeSetting.ShapeFill = "#FFA07A";
australiaLayer.ShapeSetting.ShapeStroke = "#FF6347";
australiaLayer.ShapeSetting.ShapeStrokeThickness = 0.5;

// Add sublayers to main layer
worldLayer.SubShapeFileLayers.Add(africaLayer);
worldLayer.SubShapeFileLayers.Add(australiaLayer);

// Add main layer to map
mapsControl.Layers.Add(worldLayer);
```

**Result:**
- Base world map in gray
- Africa highlighted in blue
- Australia highlighted in orange

### Complete MultiLayer Example

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Maps;

namespace MultiLayerExample
{
    public partial class Form1 : Form
    {
        private Maps mapsControl1;

        private void Form1_Load(object sender, EventArgs e)
        {
            // Initialize Maps control
            mapsControl1 = new Maps();
            mapsControl1.Name = "mapsControl1";
            mapsControl1.Size = new Size(880, 585);
            mapsControl1.Dock = DockStyle.Fill;
            mapsControl1.Margin = new Padding(0, 0, 4, 0);
            mapsControl1.MapBackgroundBrush = new SolidBrush(Color.White);
            mapsControl1.MapItemsShape = MapItemShapes.None;

            // Main layer - World map
            ShapeFileLayer shapeLayer = new ShapeFileLayer();
            shapeLayer.Uri = "world1.shp";
            shapeLayer.ShapeSetting.ShapeFill = "#E5E5E5";
            shapeLayer.ShapeSetting.ShapeStroke = "#C1C1C1";
            shapeLayer.ShapeSetting.ShapeStrokeThickness = 0.5;

            // Sublayer 1 - Highlighted region
            SubShapeFileLayer layer1 = new SubShapeFileLayer();
            layer1.Uri = "Africa.shp";
            layer1.ShapeSetting.ShapeFill = "#8DCEFF";
            layer1.ShapeSetting.ShapeStrokeThickness = 0.5;
            layer1.ShapeSetting.ShapeStroke = "#2F8CEA";

            // Sublayer 2 - Another highlighted region
            SubShapeFileLayer layer2 = new SubShapeFileLayer();
            layer2.Uri = "australia.shp";
            layer2.ShapeSetting.ShapeFill = "#8DCEFF";
            layer2.ShapeSetting.ShapeStrokeThickness = 0.5;
            layer2.ShapeSetting.ShapeStroke = "#2F8CEA";

            // Add sublayers
            shapeLayer.SubShapeFileLayers.Add(layer1);
            shapeLayer.SubShapeFileLayers.Add(layer2);

            // Add main layer to map
            mapsControl1.Layers.Add(shapeLayer);
            
            // Add map to form
            this.Controls.Add(mapsControl1);
        }
    }
}
```

## LayoutType

LayoutType defines how the map is projected and rendered. This affects how shapes appear and how coordinates are interpreted.

### LayoutType Options

**1. Default Layout**

Maps are rendered based on the coordinate points in the shape file without manipulation of the map scale.

```csharp
shapeLayer.LayoutType = LayoutType.Default;
```

**Characteristics:**
- Direct rendering of shape file coordinates
- No projection transformation
- Suitable for most standard map visualizations
- Faster rendering

**2. Tile Layout**

Map scale is maintained consistently in every direction around a point, providing accurate representation without distortion for small areas.

```csharp
shapeLayer.LayoutType = LayoutType.Tile;
```

**Characteristics:**
- Maintains uniform scale
- Accurate for small geographical areas
- Similar to web map tile systems
- Useful for precise location mapping

### Tile Layout with Annotations Example

```csharp
private void Form1_Load(object sender, EventArgs e)
{
    ShapeFileLayer shapeLayer = new ShapeFileLayer();
    shapeLayer.Uri = "world1.shp";
    
    // Set Tile layout for accurate positioning
    shapeLayer.LayoutType = LayoutType.Tile;
    
    // Add location annotations
    shapeLayer.Annotations = new ObservableCollection<Annotation>()
    {
        new LocationMarker() 
        { 
            Name = "USA", 
            Latitude = 38.8833, 
            Longitude = -77.0167 
        },
        new LocationMarker() 
        { 
            Name = "Indonesia", 
            Latitude = -6.1750, 
            Longitude = 106.8283 
        }
    };
    
    // Handle annotation drawing
    mapsControl1.AnnotationDrawing += MapsControl1_AnnotationDrawing;
    mapsControl1.Layers.Add(shapeLayer);
}

void MapsControl1_AnnotationDrawing(object sender, AnnotationDrawingEventArgs e)
{
    // Draw custom marker at annotation position
    Image image = Image.FromFile("pin.png");
    e.Graphics.DrawImage(image, (float)(e.X - 10), (float)(e.Y - 25), 25, 30);
}

public class LocationMarker : Annotation
{
    public string Name { get; set; }
}
```

## Data Binding

### ItemSource Property

The `ItemSource` property accepts collection values for data binding. This allows you to bind business objects to map shapes.

```csharp
// Prepare data model
MapViewModel model = new MapViewModel();

// Configure layer with data binding
ShapeFileLayer shapeLayer = new ShapeFileLayer();
shapeLayer.Uri = "world1.shp";
shapeLayer.ItemSource = model.Countries;  // Bind data collection

mapsControl.Layers.Add(shapeLayer);
```

### Data Model Example

```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;

public class MapViewModel
{
    public ObservableCollection<Country> Countries { get; set; }

    public MapViewModel()
    {
        Countries = new ObservableCollection<Country>();
        Countries = GetCountriesAndPopulation();
    }

    private ObservableCollection<Country> GetCountriesAndPopulation()
    {
        ObservableCollection<Country> countries = new ObservableCollection<Country>();
        
        countries.Add(new Country() { NAME = "India", Population = 1210193422 });
        countries.Add(new Country() { NAME = "Australia", Population = 22789701 });
        countries.Add(new Country() { NAME = "Mexico", Population = 112336538 });
        countries.Add(new Country() { NAME = "Russia", Population = 143228300 });
        countries.Add(new Country() { NAME = "Canada", Population = 34955100 });
        countries.Add(new Country() { NAME = "Brazil", Population = 193946886 });
        countries.Add(new Country() { NAME = "Algeria", Population = 37100000 });
        
        return countries;
    }
}

public class Country : INotifyPropertyChanged
{
    public string NAME { get; set; }
    
    private double population;
    public double Population
    {
        get { return population; }
        set
        {
            population = value;
            OnPropertyChanged(new PropertyChangedEventArgs("Population"));
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;

    public void OnPropertyChanged(PropertyChangedEventArgs e)
    {
        if (PropertyChanged != null)
        {
            PropertyChanged(this, e);
        }
    }
}
```

## Shape ID Mapping

Shape ID mapping connects data from your ItemSource to shapes in the map by matching identifiers.

### ShapeIDPath

`ShapeIDPath` is a string property that refers to the property name in your data model (ItemSource) that contains the shape identifier.

```csharp
shapeLayer.ItemSource = model.Countries;
shapeLayer.ShapeIDPath = "NAME";  // Property in Country class
```

**Requirements:**
- Must match a property name in ItemSource objects
- Case-sensitive
- Property value should match corresponding ShapeIDTableField value

### ShapeIDTableField

`ShapeIDTableField` refers to the column name in the .dbf file that contains shape identifiers.

```csharp
shapeLayer.ShapeIDTableField = "NAME";  // Column in world1.dbf
```

**Requirements:**
- Must match a column name in the .dbf file
- Case-sensitive
- Column values should match corresponding ShapeIDPath values

### Complete Mapping Example

```csharp
private void Form1_Load(object sender, EventArgs e)
{
    // Prepare data
    MapViewModel model = new MapViewModel();

    // Configure layer
    ShapeFileLayer shapeLayer = new ShapeFileLayer();
    shapeLayer.Uri = "world1.shp";
    
    // Bind data source
    shapeLayer.ItemSource = model.Countries;
    
    // Map data property to shape identifier
    shapeLayer.ShapeIDPath = "NAME";              // Property in Country class
    shapeLayer.ShapeIDTableField = "NAME";        // Column in world1.dbf file
    
    // The map will now match:
    // Country.NAME (data) ↔ world1.dbf.NAME (shape)
    
    mapsControl.Layers.Add(shapeLayer);
}
```

**How Matching Works:**

1. Map reads shape file and loads .dbf data
2. For each shape, map reads ShapeIDTableField column value
3. Map searches ItemSource for object where ShapeIDPath property matches
4. If match found, data object is bound to that shape
5. Shape can now access all properties of bound object

**Example Match:**
```
Shape in world1.shp → world1.dbf.NAME = "India"
                     ↓ (Match)
Country object → Country.NAME = "India"
                     ↓
Shape bound to Country object with Population = 1210193422
```

## Advanced Scenarios

### Scenario 1: Multiple Independent Layers

Add completely separate layers (not sublayers) to show different data sets:

```csharp
// Layer 1: World map with population data
ShapeFileLayer worldLayer = new ShapeFileLayer();
worldLayer.Uri = "world1.shp";
worldLayer.ItemSource = populationData;
mapsControl.Layers.Add(worldLayer);

// Layer 2: Major cities
ShapeFileLayer citiesLayer = new ShapeFileLayer();
citiesLayer.Uri = "cities.shp";
citiesLayer.ItemSource = cityData;
mapsControl.Layers.Add(citiesLayer);
```

### Scenario 2: Conditional Layer Visibility

Show/hide layers based on zoom level or user selection:

```csharp
// Toggle layer visibility
shapeLayer.Visible = shouldShowLayer;

// Or remove/add layers dynamically
if (zoomLevel > 5)
{
    mapsControl.Layers.Add(detailLayer);
}
else
{
    mapsControl.Layers.Remove(detailLayer);
}
```

### Scenario 3: Layer-Specific Styling

Apply different styles to different layers:

```csharp
// Base layer: Subtle colors
baseLayer.ShapeSetting.ShapeFill = "#F0F0F0";
baseLayer.ShapeSetting.ShapeStroke = "#D0D0D0";

// Highlight layer: Vibrant colors
highlightLayer.ShapeSetting.ShapeFill = "#FF6B6B";
highlightLayer.ShapeSetting.ShapeStroke = "#C92A2A";
highlightLayer.ShapeSetting.ShapeStrokeThickness = 2.0;
```

## Best Practices

1. **Use SubShapeFileLayers for highlights** - Don't duplicate main layer data
2. **Match coordinate systems** - All layers should use same projection
3. **Order matters** - Layers render in order added (bottom to top)
4. **Optimize shape files** - Use appropriate detail level for zoom range
5. **Clear binding paths** - Use descriptive property names that match .dbf columns
6. **Test with sample data** - Verify ShapeID mapping with small data set first
7. **Use LayoutType.Tile** - When precise positioning is critical