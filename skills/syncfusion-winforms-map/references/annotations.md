# Annotations in WinForms Maps

## Overview

Annotations are custom markers or labels added to specific geographical locations on a map. They provide additional context, highlight important locations, or display custom information at precise latitude/longitude coordinates.

**Use Cases:**
- Mark capital cities or major landmarks
- Display location names or labels
- Add custom icons or markers
- Highlight points of interest
- Show event locations

**Key Features:**
- Latitude/Longitude positioning
- Custom text labels
- Custom drawing support
- Color customization
- Multiple annotations per map

## Annotation Properties

The `Annotation` class provides properties for positioning and styling annotations.

### Core Properties

| Property | Type | Description |
|----------|------|-------------|
| `AnnotationLabel` | string | Text message displayed at the annotation |
| `AnnotationStroke` | Brush | Color/brush for the annotation |
| `Latitude` | double | Latitude coordinate for positioning |
| `Longitude` | double | Longitude coordinate for positioning |

### Annotation Class Structure

```csharp
public class Annotation
{
    public string AnnotationLabel { get; set; }
    public Brush AnnotationStroke { get; set; }
    public double Latitude { get; set; }
    public double Longitude { get; set; }
}
```

## Adding Annotations

### Basic Annotation Example

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using System.Collections.ObjectModel;
using Syncfusion.Windows.Forms.Maps;

private void AddBasicAnnotations()
{
    ShapeFileLayer shapeLayer = new ShapeFileLayer();
    shapeLayer.Uri = "world1.shp";
    
    // Styling
    shapeLayer.ShapeSetting.ShapeFill = "#E5E5E5";
    shapeLayer.ShapeSetting.ShapeStrokeThickness = 1.5;
    shapeLayer.ShapeSetting.ShapeStroke = "#C1C1C1";

    // Add annotations
    shapeLayer.Annotations.Add(new Annotation() 
    { 
        AnnotationLabel = "North America", 
        Latitude = 40.4230, 
        Longitude = -112.7372,
        AnnotationStroke = new SolidBrush(Color.OrangeRed) 
    });

    shapeLayer.Annotations.Add(new Annotation() 
    { 
        AnnotationLabel = "Africa", 
        Latitude = 9.1021, 
        Longitude = 18.2812, 
        AnnotationStroke = new SolidBrush(Color.OrangeRed) 
    });

    shapeLayer.Annotations.Add(new Annotation() 
    { 
        AnnotationLabel = "Europe", 
        Latitude = 53.0000, 
        Longitude = 9.0000, 
        AnnotationStroke = new SolidBrush(Color.OrangeRed) 
    });

    mapsControl.Layers.Add(shapeLayer);
}
```

### Complete Annotation Example

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using System.Collections.ObjectModel;
using Syncfusion.Windows.Forms.Maps;

namespace AnnotationsExample
{
    public partial class Form1 : Form
    {
        private Maps mapsControl1;

        private void Form1_Load(object sender, EventArgs e)
        {
            // Map configuration
            this.MetroColor = Color.White;
            this.mapsControl1 = new Maps();
            this.mapsControl1.Dock = DockStyle.Fill;
            this.mapsControl1.Margin = new Padding(0, 0, 4, 0);
            this.mapsControl1.MapBackgroundBrush = new SolidBrush(Color.White);
            this.mapsControl1.MapItemsShape = MapItemShapes.None;

            MapViewModel model = new MapViewModel();

            // Shape layer
            ShapeFileLayer shapeLayer = new ShapeFileLayer();
            shapeLayer.Uri = "world1.shp";
            shapeLayer.ItemSource = model.Countries;
            shapeLayer.ShapeSetting.ShapeFill = "#E5E5E5";
            shapeLayer.ShapeSetting.ShapeStrokeThickness = 1.5;
            shapeLayer.ShapeSetting.ShapeStroke = "#C1C1C1";

            // Add multiple annotations
            shapeLayer.Annotations.Add(new Annotation() 
            { 
                AnnotationLabel = "North America", 
                Latitude = 40.4230, 
                Longitude = -112.7372,
                AnnotationStroke = new SolidBrush(Color.OrangeRed) 
            });

            shapeLayer.Annotations.Add(new Annotation() 
            { 
                AnnotationLabel = "Africa", 
                Latitude = 9.1021, 
                Longitude = 18.2812, 
                AnnotationStroke = new SolidBrush(Color.OrangeRed) 
            });

            shapeLayer.Annotations.Add(new Annotation() 
            { 
                AnnotationLabel = "Europe", 
                Latitude = 53.0000, 
                Longitude = 9.0000, 
                AnnotationStroke = new SolidBrush(Color.OrangeRed) 
            });

            shapeLayer.Annotations.Add(new Annotation() 
            { 
                AnnotationLabel = "South America", 
                Latitude = -19.6048, 
                Longitude = -73.0625, 
                AnnotationStroke = new SolidBrush(Color.OrangeRed) 
            });

            shapeLayer.Annotations.Add(new Annotation() 
            { 
                AnnotationLabel = "Asia", 
                Latitude = 49.8380, 
                Longitude = 105.8203, 
                AnnotationStroke = new SolidBrush(Color.OrangeRed) 
            });

            shapeLayer.Annotations.Add(new Annotation() 
            { 
                AnnotationLabel = "Oceania", 
                Latitude = -20.3456, 
                Longitude = 120.4346, 
                AnnotationStroke = new SolidBrush(Color.OrangeRed) 
            });

            this.mapsControl1.Layers.Add(shapeLayer);
            this.Controls.Add(this.mapsControl1);
        }
    }
}
```

## Custom Annotation Drawing

For advanced scenarios, use the `AnnotationDrawing` event to render custom graphics at annotation positions.

### AnnotationDrawing Event

The `AnnotationDrawing` event fires for each annotation, providing a Graphics object and the screen coordinates where the annotation should be drawn.

**Event Handler Signature:**
```csharp
void AnnotationDrawing(object sender, AnnotationDrawingEventArgs e)
```

**AnnotationDrawingEventArgs Properties:**
- `Graphics`: GDI+ Graphics object for drawing
- `X`: Screen X coordinate
- `Y`: Screen Y coordinate
- `Annotation`: The annotation being drawn

### Custom Icon Markers

```csharp
using System;
using System.Drawing;
using Syncfusion.Windows.Forms.Maps;

private void Form1_Load(object sender, EventArgs e)
{
    ShapeFileLayer shapeLayer = new ShapeFileLayer();
    shapeLayer.Uri = "world1.shp";
    
    // Use Tile layout for accurate positioning
    shapeLayer.LayoutType = LayoutType.Tile;
    
    // Add location annotations
    shapeLayer.Annotations = new ObservableCollection<Annotation>()
    {
        new SyncfusionLocation() 
        { 
            Name = "USA", 
            Latitude = 38.8833, 
            Longitude = -77.0167 
        },
        new SyncfusionLocation() 
        { 
            Name = "Indonesia", 
            Latitude = -6.1750, 
            Longitude = 106.8283 
        },
        new SyncfusionLocation() 
        { 
            Name = "India", 
            Latitude = 28.6139, 
            Longitude = 77.2090 
        }
    };
    
    // Subscribe to drawing event
    mapsControl1.AnnotationDrawing += MapsControl1_AnnotationDrawing;
    mapsControl1.Layers.Add(shapeLayer);
}

void MapsControl1_AnnotationDrawing(object sender, AnnotationDrawingEventArgs e)
{
    // Load custom pin image
    Image image = Image.FromFile("pin.png");
    
    // Draw at annotation position, offset to center the pin
    e.Graphics.DrawImage(image, (float)(e.X - 10), (float)(e.Y - 25), 25, 30);
}

// Custom annotation class
public class SyncfusionLocation : Annotation
{
    public string Name { get; set; }
}
```

### Drawing Custom Shapes

```csharp
void MapsControl1_AnnotationDrawing(object sender, AnnotationDrawingEventArgs e)
{
    SyncfusionLocation location = e.Annotation as SyncfusionLocation;
    
    if (location != null)
    {
        // Draw circle marker
        float x = (float)e.X;
        float y = (float)e.Y;
        float radius = 8;
        
        // Fill circle
        e.Graphics.FillEllipse(
            new SolidBrush(Color.Red), 
            x - radius, 
            y - radius, 
            radius * 2, 
            radius * 2
        );
        
        // Draw border
        e.Graphics.DrawEllipse(
            new Pen(Color.DarkRed, 2), 
            x - radius, 
            y - radius, 
            radius * 2, 
            radius * 2
        );
        
        // Draw label
        Font font = new Font("Arial", 9, FontStyle.Bold);
        SizeF textSize = e.Graphics.MeasureString(location.Name, font);
        e.Graphics.DrawString(
            location.Name, 
            font, 
            Brushes.Black, 
            x - textSize.Width / 2, 
            y + radius + 2
        );
    }
}
```

### Drawing Images with Text

```csharp
void MapsControl1_AnnotationDrawing(object sender, AnnotationDrawingEventArgs e)
{
    SyncfusionLocation location = e.Annotation as SyncfusionLocation;
    
    if (location != null)
    {
        float x = (float)e.X;
        float y = (float)e.Y;
        
        // Draw marker icon
        Image markerImage = Image.FromFile(@"assets\marker.png");
        e.Graphics.DrawImage(markerImage, x - 12, y - 30, 24, 30);
        
        // Draw location name
        Font font = new Font("Segoe UI", 9, FontStyle.Bold);
        Brush textBrush = new SolidBrush(Color.FromArgb(200, 255, 255, 255));
        Brush shadowBrush = new SolidBrush(Color.FromArgb(150, 0, 0, 0));
        
        // Text shadow
        e.Graphics.DrawString(location.Name, font, shadowBrush, x - 20, y + 2);
        
        // Text
        e.Graphics.DrawString(location.Name, font, textBrush, x - 20, y);
    }
}
```

## Dynamic Annotations

### Adding Annotations Programmatically

```csharp
private void AddAnnotationAtCoordinate(double latitude, double longitude, string label)
{
    ShapeFileLayer layer = mapsControl.Layers[0] as ShapeFileLayer;
    
    if (layer != null)
    {
        layer.Annotations.Add(new Annotation()
        {
            Latitude = latitude,
            Longitude = longitude,
            AnnotationLabel = label,
            AnnotationStroke = new SolidBrush(Color.Blue)
        });
        
        // Refresh map
        mapsControl.Invalidate();
    }
}
```

### Removing Annotations

```csharp
private void ClearAllAnnotations()
{
    ShapeFileLayer layer = mapsControl.Layers[0] as ShapeFileLayer;
    
    if (layer != null)
    {
        layer.Annotations.Clear();
        mapsControl.Invalidate();
    }
}
```

### Updating Annotations

```csharp
private void UpdateAnnotation(int index, string newLabel)
{
    ShapeFileLayer layer = mapsControl.Layers[0] as ShapeFileLayer;
    
    if (layer != null && index < layer.Annotations.Count)
    {
        layer.Annotations[index].AnnotationLabel = newLabel;
        mapsControl.Invalidate();
    }
}
```

## Advanced Scenarios

### Annotation from Data Binding

Create annotations from a data collection:

```csharp
public class City
{
    public string Name { get; set; }
    public double Latitude { get; set; }
    public double Longitude { get; set; }
    public int Population { get; set; }
}

private void LoadCitiesAsAnnotations(List<City> cities)
{
    ShapeFileLayer layer = new ShapeFileLayer();
    layer.Uri = "world1.shp";
    layer.LayoutType = LayoutType.Tile;
    
    // Convert cities to annotations
    layer.Annotations = new ObservableCollection<Annotation>();
    
    foreach (var city in cities)
    {
        layer.Annotations.Add(new CityAnnotation()
        {
            Name = city.Name,
            Latitude = city.Latitude,
            Longitude = city.Longitude,
            Population = city.Population,
            AnnotationStroke = new SolidBrush(Color.Red)
        });
    }
    
    mapsControl.AnnotationDrawing += DrawCityAnnotation;
    mapsControl.Layers.Add(layer);
}

public class CityAnnotation : Annotation
{
    public string Name { get; set; }
    public int Population { get; set; }
}

void DrawCityAnnotation(object sender, AnnotationDrawingEventArgs e)
{
    CityAnnotation city = e.Annotation as CityAnnotation;
    
    if (city != null)
    {
        // Scale marker size by population
        float size = Math.Min(20, Math.Max(5, city.Population / 100000f));
        
        e.Graphics.FillEllipse(
            Brushes.Red,
            (float)e.X - size / 2,
            (float)e.Y - size / 2,
            size,
            size
        );
    }
}
```

### Interactive Annotations

Handle click events on annotations:

```csharp
// Note: WinForms Maps doesn't have built-in annotation click events
// Implement custom hit testing

private void mapsControl1_MouseClick(object sender, MouseEventArgs e)
{
    ShapeFileLayer layer = mapsControl.Layers[0] as ShapeFileLayer;
    if (layer != null)
    {
        // Check if click is near any annotation
        foreach (var annotation in layer.Annotations)
        {
            // Convert lat/long to screen coordinates (simplified)
            Point screenPos = ConvertLatLongToScreen(annotation.Latitude, annotation.Longitude);
            
            double distance = Math.Sqrt(
                Math.Pow(e.X - screenPos.X, 2) + 
                Math.Pow(e.Y - screenPos.Y, 2)
            );
            
            if (distance < 15) // Within 15 pixels
            {
                MessageBox.Show($"Clicked: {annotation.AnnotationLabel}");
                break;
            }
        }
    }
}
```

## Best Practices

1. **Use LayoutType.Tile** for precise annotation positioning
2. **Keep labels concise** - Short, readable text
3. **Consistent styling** - Use same color scheme for related annotations
4. **Avoid overcrowding** - Don't place too many annotations close together
5. **Custom drawing for complex markers** - Use AnnotationDrawing event for icons
6. **Performance** - Limit number of annotations for large datasets
7. **Z-order** - Annotations draw after shapes; control overlap with drawing order
8. **Coordinate accuracy** - Verify latitude/longitude values are correct

## Troubleshooting

**Issue: Annotations not appearing**
- Verify LayoutType is set (prefer Tile for annotations)
- Check latitude/longitude values are within valid ranges
- Ensure Annotations collection is added to layer before adding layer to map

**Issue: Annotations in wrong positions**
- Use LayoutType.Tile for accurate positioning
- Verify coordinate system matches shape file projection
- Check latitude/longitude order (latitude first, longitude second)

**Issue: Custom drawing not working**
- Ensure AnnotationDrawing event is subscribed before adding layer
- Verify image paths are correct for custom icons
- Check Graphics object is used correctly

**Issue: Annotation labels overlap**
- Reduce number of annotations
- Filter annotations based on zoom level
- Use custom drawing to position labels intelligently