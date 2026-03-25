# User Interaction in WinForms Maps

## Table of Contents
- [Overview](#overview)
- [Tooltip Support](#tooltip-support)
- [Map Selection](#map-selection)
  - [Single Selection](#single-selection)
  - [Multiple Selection](#multiple-selection)
- [Zooming](#zooming)
  - [Zoom using ZoomFactor](#zoom-using-zoomfactor)
  - [Zoom using ZoomLevel](#zoom-using-zoomlevel)
- [Panning](#panning)
- [Events](#events)
- [Complete Interactive Examples](#complete-interactive-examples)

## Overview

The WinForms Maps control provides rich interaction capabilities that enable users to explore geographical data effectively. Key interaction features include:

- **Tooltips**: Display additional information on hover
- **Selection**: Single or multiple shape selection
- **Zooming**: Magnify specific map regions
- **Panning**: Navigate across the map
- **Events**: Respond to user actions

These features work together to create an intuitive, responsive map experience.

## Tooltip Support

Tooltips are hanging windows that appear when the mouse hovers over a shape, displaying additional information from the bound data object.

### Enabling Tooltips

```csharp
ShapeFileLayer shapeLayer = new ShapeFileLayer();
shapeLayer.Uri = "world1.shp";
shapeLayer.ItemSource = countries;
shapeLayer.ShapeIDPath = "NAME";
shapeLayer.ShapeIDTableField = "NAME";

// Enable tooltips
shapeLayer.ShowToolTip = true;

mapsControl.Layers.Add(shapeLayer);
```

### Tooltip Content

By default, tooltips display the value from the property specified in `ShapeValuePath`:

```csharp
shapeLayer.ShapeSetting.ShapeValuePath = "Population";
// Tooltip will show: Population value
```

### Tooltip Display Path

Control what information appears in tooltips using `ShapeDisplayValuePath`:

```csharp
shapeLayer.ShapeSetting.ShapeDisplayValuePath = "NAME";
// Tooltip will show: Country name
```

### Complete Tooltip Example

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Maps;

private void CreateMapWithTooltips()
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

    // Configure shape display
    shapeLayer.ShapeSetting.ShapeValuePath = "Population";
    shapeLayer.ShapeSetting.ShapeColorValuePath = "Population";
    shapeLayer.ShapeSetting.ShapeDisplayValuePath = "NAME";
    shapeLayer.ShapeSetting.TextForeground = "Black";
    
    // Styling
    shapeLayer.ShowMapItem = false;
    shapeLayer.ShapeSetting.ShapeFill = "#E5E5E5";
    shapeLayer.ShapeSetting.ShapeStrokeThickness = 1.5;
    shapeLayer.ShapeSetting.ShapeStroke = "Black";
    shapeLayer.ShapeSetting.FillSetting.AutoFillColors = false;

    // Enable tooltips
    shapeLayer.ShowToolTip = true;

    mapsControl1.Layers.Add(shapeLayer);
}
```

**Result:** When hovering over a country, a tooltip displays the country name.

## Map Selection

Map selection allows users to interactively select one or more shapes on the map. Selected shapes are visually differentiated by their fill color.

### Key Selection Properties

| Property | Type | Description |
|----------|------|-------------|
| `EnableSelection` | bool | Enable/disable shape selection |
| `SelectionMode` | SelectionModes | Single or Multiple selection |
| `SelectedShapeColor` | Color | Fill color for selected shapes |
| `SelectedMapShapes` | Collection | Currently selected shapes |

### Single Selection

Single selection mode allows only one shape to be selected at a time. Selecting a new shape deselects the previously selected shape.

```csharp
ShapeFileLayer shapeLayer = new ShapeFileLayer();
shapeLayer.Uri = "world1.shp";
shapeLayer.ShapeIDPath = "Country";
shapeLayer.ShapeIDTableField = "NAME";

// Enable single selection
shapeLayer.EnableSelection = true;
shapeLayer.SelectionMode = SelectionModes.Single;  // Default

// Customize selected shape appearance
shapeLayer.ShapeSetting.SelectedShapeColor = Color.Orange;

mapsControl.Layers.Add(shapeLayer);
```

**Behavior:**
- Click a shape to select it
- Click another shape to deselect the first and select the new one
- Click map background to deselect all

### Multiple Selection

Multiple selection mode allows selecting multiple shapes simultaneously by holding the Ctrl key.

```csharp
ShapeFileLayer shapeLayer = new ShapeFileLayer();
shapeLayer.Uri = "world1.shp";
shapeLayer.ShapeIDPath = "Country";
shapeLayer.ShapeIDTableField = "NAME";

// Enable multiple selection
shapeLayer.EnableSelection = true;
shapeLayer.SelectionMode = SelectionModes.Multiple;

// Style selected shapes
shapeLayer.ShapeSetting.SelectedShapeColor = Color.DeepSkyBlue;

shapeLayer.ShowMapItem = false;

mapsControl.Layers.Add(shapeLayer);
```

**Behavior:**
- Click a shape to select it
- Hold Ctrl and click additional shapes to add to selection
- Click selected shape while holding Ctrl to deselect it
- Release Ctrl to return to single selection behavior

### Complete Selection Example

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Maps;

private void Form1_Load(object sender, EventArgs e)
{
    mapsControl1.Dock = DockStyle.Fill;
    mapsControl1.Margin = new Padding(0, 0, 4, 0);
    mapsControl1.MapBackgroundBrush = new SolidBrush(Color.White);
    mapsControl1.MapItemsShape = MapItemShapes.None;

    ShapeFileLayer shapeLayer = new ShapeFileLayer();
    shapeLayer.ShapeSetting.FillSetting.AutoFillColors = false;
    shapeLayer.Uri = "world1.shp";
    shapeLayer.ShapeIDPath = "Country";
    shapeLayer.ShapeIDTableField = "NAME";
    
    // Enable multiple selection
    shapeLayer.EnableSelection = true;
    shapeLayer.SelectionMode = SelectionModes.Multiple;
    
    // Customize appearance
    shapeLayer.ShapeSetting.ShapeFill = "#E5E5E5";
    shapeLayer.ShapeSetting.ShapeStroke = "#C1C1C1";
    shapeLayer.ShapeSetting.ShapeStrokeThickness = 1.0;
    shapeLayer.ShapeSetting.SelectedShapeColor = Color.FromArgb(255, 255, 140, 0);
    
    shapeLayer.ShowMapItem = false;

    mapsControl1.Layers.Add(shapeLayer);
}
```

### Accessing Selected Shapes

```csharp
// Get selected shapes
var selectedShapes = mapsControl.SelectedMapShapes;

foreach (var shape in selectedShapes)
{
    // Process selected shape
    Console.WriteLine($"Selected: {shape}");
}
```

## Zooming

Zooming allows users to magnify specific regions of the map for detailed examination. WinForms Maps supports two zooming methods.

### Zoom using ZoomFactor

`ZoomFactor` directly controls the zoom level. Increasing the value zooms in; decreasing zooms out.

**Property:** `float ZoomFactor`

**Default:** `1.0` (no zoom)

**Range:** `0.1` to `10.0` (typical)

```csharp
mapsControl1.ZoomFactor = 1.5f;  // 150% zoom (50% magnification)
```

#### ZoomFactor Example

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Maps;

private void Form1_Load(object sender, EventArgs e)
{
    mapsControl1.MapBackgroundBrush = new SolidBrush(Color.White);
    mapsControl1.MapItemsShape = MapItemShapes.None;
    
    // Set zoom factor
    mapsControl1.ZoomFactor = 0.75f;  // Zoom out to 75%

    ShapeFileLayer shapeLayer = new ShapeFileLayer();
    shapeLayer.Uri = "world1.shp";
    shapeLayer.ItemSource = model.Countries;
    shapeLayer.ShapeIDPath = "NAME";
    shapeLayer.ShapeIDTableField = "NAME";

    mapsControl1.Layers.Add(shapeLayer);
}
```

### Zoom using ZoomLevel

`ZoomLevel` is a multiplier applied to the current ZoomFactor. When ZoomLevel changes, ZoomFactor is recalculated:

```
New ZoomFactor = Initial ZoomFactor × ZoomLevel
```

**Property:** `int ZoomLevel`

**Default:** `1` (no additional zoom)

```csharp
mapsControl1.ZoomFactor = 1.0f;  // Initial zoom
mapsControl1.ZoomLevel = 4;       // Final zoom = 1.0 × 4 = 4.0
```

#### ZoomLevel Example

```csharp
private void Form1_Load(object sender, EventArgs e)
{
    mapsControl1.MapBackgroundBrush = new SolidBrush(Color.White);
    mapsControl1.MapItemsShape = MapItemShapes.None;
    
    // Set initial zoom
    mapsControl1.ZoomFactor = 0.75f;
    
    // Apply zoom level multiplier
    mapsControl1.ZoomLevel = 4;
    // Effective zoom = 0.75 × 4 = 3.0

    ShapeFileLayer shapeLayer = new ShapeFileLayer();
    shapeLayer.Uri = "world1.shp";

    mapsControl1.Layers.Add(shapeLayer);
}
```

### Dynamic Zoom Controls

Add buttons or trackbar to control zoom:

```csharp
// Zoom In button
private void btnZoomIn_Click(object sender, EventArgs e)
{
    mapsControl1.ZoomFactor = Math.Min(mapsControl1.ZoomFactor * 1.2f, 10.0f);
}

// Zoom Out button
private void btnZoomOut_Click(object sender, EventArgs e)
{
    mapsControl1.ZoomFactor = Math.Max(mapsControl1.ZoomFactor / 1.2f, 0.1f);
}

// Reset Zoom button
private void btnResetZoom_Click(object sender, EventArgs e)
{
    mapsControl1.ZoomFactor = 1.0f;
    mapsControl1.ZoomLevel = 1;
}

// Trackbar for zoom
private void zoomTrackBar_ValueChanged(object sender, EventArgs e)
{
    mapsControl1.ZoomFactor = zoomTrackBar.Value / 10.0f;
}
```

## Panning

Panning allows users to navigate across the map by dragging. This is typically enabled by default when zoomed in.

**Behavior:**
- Click and hold on map
- Drag to move the visible area
- Release to stop panning

**Programmatic Panning:**
While there's no direct Pan method, you can achieve panning effects by adjusting the zoom center or reloading layers with offset coordinates.

## Events

### ShapeSelected Event

Triggered when one or more shapes are selected. Provides access to the selected shape's bound data.

```csharp
// Subscribe to event
mapsControl1.ShapeSelected += MapsControl1_ShapeSelected;

private void MapsControl1_ShapeSelected(object sender, ShapeSelectedEventArgs e)
{
    if (e.Data != null)
    {
        foreach (Country country in e.Data)
        {
            Console.WriteLine($"Selected: {country.NAME}");
            Console.WriteLine($"Population: {country.Population}");
        }
    }
}
```

### Event with UI Update Example

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Maps;

public partial class Form1 : Form
{
    private ListBox countryListBox;
    
    private void Form1_Load(object sender, EventArgs e)
    {
        // Create ListBox to display selections
        countryListBox = new ListBox();
        countryListBox.Dock = DockStyle.Right;
        countryListBox.Width = 200;
        this.Controls.Add(countryListBox);
        
        // Configure map
        ShapeFileLayer shapeLayer = new ShapeFileLayer();
        shapeLayer.Uri = "world1.shp";
        shapeLayer.EnableSelection = true;
        shapeLayer.SelectionMode = SelectionModes.Multiple;
        
        mapsControl1.Layers.Add(shapeLayer);
        
        // Subscribe to event
        mapsControl1.ShapeSelected += MapsControl1_ShapeSelected;
    }

    private void MapsControl1_ShapeSelected(object sender, ShapeSelectedEventArgs e)
    {
        countryListBox.Items.Clear();

        if (e.Data != null)
        {
            foreach (Country country in e.Data)
            {
                countryListBox.Items.Add($"{country.NAME} - {country.Population:N0}");
            }
        }
    }
}
```

## Complete Interactive Examples

### Example 1: Fully Interactive Map

```csharp
private void CreateFullyInteractiveMap()
{
    // Map configuration
    mapsControl1.Dock = DockStyle.Fill;
    mapsControl1.MapBackgroundBrush = new SolidBrush(Color.White);
    mapsControl1.MapItemsShape = MapItemShapes.None;
    mapsControl1.ZoomFactor = 1.0f;
    
    // Data
    MapViewModel model = new MapViewModel();
    
    // Layer configuration
    ShapeFileLayer shapeLayer = new ShapeFileLayer();
    shapeLayer.Uri = "world1.shp";
    shapeLayer.ItemSource = model.Countries;
    shapeLayer.ShapeIDPath = "NAME";
    shapeLayer.ShapeIDTableField = "NAME";
    
    // Appearance
    shapeLayer.ShapeSetting.ShapeValuePath = "Population";
    shapeLayer.ShapeSetting.ShapeDisplayValuePath = "NAME";
    shapeLayer.ShapeSetting.ShapeFill = "#E5E5E5";
    shapeLayer.ShapeSetting.ShapeStroke = "#C1C1C1";
    shapeLayer.ShapeSetting.ShapeStrokeThickness = 1.0;
    shapeLayer.ShapeSetting.SelectedShapeColor = Color.OrangeRed;
    
    // Enable all interactions
    shapeLayer.ShowToolTip = true;
    shapeLayer.EnableSelection = true;
    shapeLayer.SelectionMode = SelectionModes.Multiple;
    
    mapsControl1.Layers.Add(shapeLayer);
    
    // Event handlers
    mapsControl1.ShapeSelected += MapsControl1_ShapeSelected;
}
```

### Example 2: Map with Zoom Controls

```csharp
private void CreateMapWithZoomControls()
{
    // Create zoom control panel
    Panel zoomPanel = new Panel();
    zoomPanel.Dock = DockStyle.Top;
    zoomPanel.Height = 40;
    
    Button btnZoomIn = new Button() { Text = "+", Width = 40 };
    Button btnZoomOut = new Button() { Text = "-", Width = 40, Left = 45 };
    Button btnReset = new Button() { Text = "Reset", Width = 60, Left = 90 };
    
    btnZoomIn.Click += (s, e) => 
        mapsControl1.ZoomFactor = Math.Min(mapsControl1.ZoomFactor * 1.2f, 10.0f);
    
    btnZoomOut.Click += (s, e) => 
        mapsControl1.ZoomFactor = Math.Max(mapsControl1.ZoomFactor / 1.2f, 0.1f);
    
    btnReset.Click += (s, e) => mapsControl1.ZoomFactor = 1.0f;
    
    zoomPanel.Controls.Add(btnZoomIn);
    zoomPanel.Controls.Add(btnZoomOut);
    zoomPanel.Controls.Add(btnReset);
    
    this.Controls.Add(zoomPanel);
    
    // Configure map
    ShapeFileLayer shapeLayer = new ShapeFileLayer();
    shapeLayer.Uri = "world1.shp";
    mapsControl1.Layers.Add(shapeLayer);
}
```

## Best Practices

1. **Enable tooltips** for better data exploration
2. **Use appropriate selection modes** - Single for exclusive choices, Multiple for comparisons
3. **Style selected shapes distinctly** - Use contrasting colors
4. **Provide zoom controls** for better UX
5. **Handle ShapeSelected event** to respond to user actions
6. **Set reasonable zoom limits** to prevent over-zooming
7. **Combine features** - Tooltips + Selection + Zoom for rich interaction
8. **Clear visual feedback** - Ensure users know what's selected
9. **Test interaction flows** - Verify all combinations work smoothly

## Troubleshooting

**Issue: Tooltips not showing**
- Verify `ShowToolTip = true`
- Check that ShapeDisplayValuePath points to valid property
- Ensure data is bound correctly

**Issue: Selection not working**
- Set `EnableSelection = true`
- Verify shapes have bound data
- Check mouse events aren't blocked

**Issue: Zoom not responding**
- Ensure ZoomFactor is in valid range (0.1 - 10.0)
- Check that map has renderable content
- Verify no conflicting pan/zoom settings

**Issue: ShapeSelected event not firing**
- Confirm event handler is subscribed
- Verify EnableSelection is true
- Check that e.Data type matches your data model