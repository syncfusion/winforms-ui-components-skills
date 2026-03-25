# Data Markers

## Table of Contents
- [Overview](#overview)
- [Marker Shapes](#marker-shapes)
- [Marker Customization](#marker-customization)
- [Marker Images](#marker-images)
- [Data Labels](#data-labels)

## Overview

Data markers are used to visually indicate individual data points on the Smith Chart. They help users identify specific values and make the chart more readable. Markers can be shapes (circle, rectangle, diamond, etc.) or custom images.

## Marker Shapes

Enable markers by setting the `MarkerVisible` property to `true`. By default, markers are displayed as circles.

### Enabling Markers

**C# Example:**
```csharp
LineSeries series = new LineSeries();
series.MarkerVisible = true;
```

**VB.NET Example:** 
```vb
Dim series As New LineSeries()
series.MarkerVisible = True
```

### Available Marker Types

The `MarkerType` property provides different shapes:

| MarkerType | Description |
|------------|-------------|
| `Circle` | Circular marker (default) |
| `Rectangle` | Square/rectangular marker |
| `Diamond` | Diamond-shaped marker |
| `Triangle` | Triangular marker |
| `Cross` | Cross (+) marker |
| `Plus` | Plus marker |
| `Pentagon` | Five-sided marker |
| `Image` | Custom image marker |

### Setting Marker Type

**C# Example:**
```csharp
LineSeries series = new LineSeries();
series.MarkerVisible = true;
series.MarkerType = MarkerType.Diamond;
```

**VB.NET Example:**
```vb
Dim series As New LineSeries()
series.MarkerVisible = True
series.MarkerType = MarkerType.Diamond
```

## Marker Customization

Customize marker appearance using the following properties:

### Marker Properties

| Property | Type | Description |
|----------|------|-------------|
| `MarkerVisible` | bool | Enable/disable marker display |
| `MarkerType` | MarkerType | Shape of the marker |
| `MarkerBackColor` | Color | Fill color of the marker |
| `MarkerBorderColor` | Color | Border/outline color of the marker |
| `MarkerHeight` | float | Height of the marker in pixels |   
| `MarkerWidth` | float | Width of the marker in pixels |

### Comprehensive Customization Example

**C# Example:**
```csharp
LineSeries series = new LineSeries();
series.MarkerVisible = true;
series.MarkerType = MarkerType.Rectangle;
series.MarkerHeight = 8;
series.MarkerWidth = 8;
series.MarkerBorderColor = Color.Black;
series.MarkerBackColor = Color.Yellow;
sfSmithChart1.Series.Add(series);
```

**VB.NET Example:**
```vb
Dim series As New LineSeries()
series.MarkerVisible = True
series.MarkerType = MarkerType.Rectangle
series.MarkerHeight = 8
series.MarkerWidth = 8
series.MarkerBorderColor = Color.Black
series.MarkerBackColor = Color.Yellow
sfSmithChart1.Series.Add(series)
```

This creates yellow rectangular markers with black borders, 8x8 pixels in size.

## Marker Images

Use custom images as markers to create unique visualizations or highlight specific data points.

### Setting Up Image Markers

1. Add your image to the project's Resources
2. Set `MarkerType` to `Image`
3. Assign the image to the `MarkerImage` property
4. Configure marker size with `MarkerHeight` and `MarkerWidth`

**C# Example:**
```csharp
series.MarkerType = MarkerType.Image;
series.MarkerWidth = 20;
series.MarkerHeight = 20;
series.MarkerImage = Properties.Resources.Marker;
```

**VB.NET Example:**
```vb
series.MarkerType = MarkerType.Image
series.MarkerWidth = 20
series.MarkerHeight = 20
series.MarkerImage = My.Resources.Marker
```

### Adding Image to Resources

1. Right-click on your project in Solution Explorer
2. Select **Properties**
3. Go to the **Resources** tab
4. Click **Add Resource** → **Add Existing File**
5. Select your image file (PNG, JPG, etc.)
6. Reference it using `Properties.Resources.YourImageName`

### When to Use Image Markers

- **Branding:** Use company logos or product icons
- **Special Points:** Highlight critical measurements with unique icons
- **Categorization:** Different images for different data categories
- **Visual Appeal:** Create more engaging visualizations

## Data Labels

Data labels display the actual values of data points directly on the chart. They provide additional information and improve readability.

### Enabling Data Labels

**C# Example:**
```csharp
series.DataLabel.Visible = true;
```

**VB.NET Example:**
```vb
series.DataLabel.Visible = True
```

### Smart Label Alignment

Data labels are automatically positioned to avoid overlapping with other labels and chart elements. The Smith Chart uses intelligent algorithms to:

1. **Position labels optimally** - Labels appear at the top of data points by default
2. **Detect collisions** - Automatically identifies when labels would overlap
3. **Auto-adjust placement** - Moves labels to alternate positions when collisions occur
4. **Maintain readability** - Ensures all labels remain readable

### Connector Lines

When data labels are automatically repositioned due to collisions, connector lines are drawn from the label to its corresponding data point. This ensures users can always identify which label belongs to which point.

**Features:**
- Automatically generated when labels are moved
- Connect the label to its data point
- Maintain visual clarity
- No configuration required

### Data Label Format

The format of data labels can be customized, though the default format typically shows the resistance/conductance and reactance/susceptance values.

### Smart Alignment Example

When multiple data points are close together:

```csharp
LineSeries series = new LineSeries();
series.MarkerVisible = true;
series.DataLabel.Visible = true;

// Add points that are close together
series.Points.Add(1.0, 0.5);
series.Points.Add(1.1, 0.52);
series.Points.Add(1.2, 0.54);
series.Points.Add(1.15, 0.51);

sfSmithChart.Series.Add(series);
```

The chart will automatically:
- Detect that these labels would overlap
- Reposition them to avoid collision
- Draw connector lines to maintain point-label association

## Common Patterns

### Pattern 1: Basic Markers with Custom Colors

```csharp
LineSeries series = new LineSeries();
series.MarkerVisible = true;
series.MarkerType = MarkerType.Circle;
series.MarkerBackColor = Color.Red;
series.MarkerBorderColor = Color.DarkRed;
series.MarkerHeight = 6;
series.MarkerWidth = 6;
```

### Pattern 2: Different Markers for Multiple Series

```csharp
// Series 1 - Circles
LineSeries series1 = new LineSeries();
series1.MarkerVisible = true;
series1.MarkerType = MarkerType.Circle;
series1.MarkerBackColor = Color.Blue;
sfSmithChart.Series.Add(series1);

// Series 2 - Diamonds
LineSeries series2 = new LineSeries();
series2.MarkerVisible = true;
series2.MarkerType = MarkerType.Diamond;
series2.MarkerBackColor = Color.Red;
sfSmithChart.Series.Add(series2);
```

### Pattern 3: Markers with Data Labels

```csharp
LineSeries series = new LineSeries();
series.MarkerVisible = true;
series.MarkerType = MarkerType.Rectangle;
series.MarkerBackColor = Color.Green;
series.DataLabel.Visible = true;
```

### Pattern 4: Image Markers for Special Points

```csharp
LineSeries series = new LineSeries();
series.MarkerVisible = true;
series.MarkerType = MarkerType.Image;
series.MarkerImage = Properties.Resources.CustomIcon;
series.MarkerWidth = 16;
series.MarkerHeight = 16;
```

## Best Practices

1. **Marker Size:** Keep markers between 6-12 pixels for optimal visibility without cluttering

2. **Contrast:** Use marker colors that contrast with both the line color and chart background

3. **Border Colors:** Add borders to markers (MarkerBorderColor) for better definition

4. **Consistency:** Use consistent marker styles within the same chart for professional appearance

5. **Image Size:** When using image markers, keep dimensions small (16x16 or 20x20) to avoid obscuring data

6. **Data Labels:** Use data labels sparingly on charts with many points to avoid clutter; rely on tooltips instead

7. **Multiple Series:** Use different marker shapes for different series to aid visual distinction

## Troubleshooting

### Markers Not Appearing

- Ensure `MarkerVisible = true` is set
- Check that `MarkerHeight` and `MarkerWidth` are greater than 0
- Verify marker color is not the same as background color

### Image Markers Not Showing

- Confirm the image is added to project Resources
- Verify `MarkerType = MarkerType.Image` is set
- Check that `MarkerImage` property references the correct resource
- Ensure `MarkerWidth` and `MarkerHeight` are set to appropriate values

### Data Labels Overlapping

- The smart alignment system should handle this automatically
- If issues persist, reduce the number of data points or increase chart size
- Consider using tooltips instead of always-visible data labels for dense data
