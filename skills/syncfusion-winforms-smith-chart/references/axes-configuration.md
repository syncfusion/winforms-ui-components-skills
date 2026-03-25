# Axes Configuration

## Table of Contents
- [Overview](#overview)
- [Horizontal Axis](#horizontal-axis)
- [Radial Axis](#radial-axis)
- [Common Axis Properties](#common-axis-properties)
- [Events](#events)

## Overview

The Smith Chart contains two axes for plotting transmission line parameters:

1. **Horizontal Axis** - Plots normalized resistance (impedance) or conductance (admittance)
2. **Radial Axis** - Plots normalized reactance (impedance) or susceptance (admittance)

Both axes are automatically initialized and can be customized without manual creation.

## Horizontal Axis

The horizontal axis represents:
- **Impedance Mode:** Normalized resistance values
- **Admittance Mode:** Normalized conductance values

### Major Gridlines

Major gridlines are the primary grid lines on the axis.

#### Visibility

**C# Example:**
```csharp
sfSmithChart1.HorizontalAxis.MajorGridlinesVisible = false;
```

**VB.NET Example:**
```vb
sfSmithChart1.HorizontalAxis.MajorGridlinesVisible = False
```

#### Styling

Customize major gridline appearance using style properties:

| Property | Type | Description |
|----------|------|-------------|
| `MajorGridlinesDashStyle` | DashStyle | Line pattern (Solid, Dash, Dot, etc.) |
| `MajorGridlinesColor` | Color | Line color |
| `MajorGridlinesWidth` | float | Line thickness in pixels |

**C# Example:**
```csharp
sfSmithChart1.HorizontalAxis.Style.MajorGridlinesDashStyle = System.Drawing.Drawing2D.DashStyle.Dash;
sfSmithChart1.HorizontalAxis.Style.MajorGridlinesColor = Color.Red;
sfSmithChart1.HorizontalAxis.Style.MajorGridlinesWidth = 1;
```

**VB.NET Example:**
```vb
sfSmithChart1.HorizontalAxis.Style.MajorGridlinesDashStyle = System.Drawing.Drawing2D.DashStyle.Dash
sfSmithChart1.HorizontalAxis.Style.MajorGridlinesColor = Color.Red
sfSmithChart1.HorizontalAxis.Style.MajorGridlinesWidth = 1
```

### Minor Gridlines

Minor gridlines provide additional grid divisions between major gridlines.

#### Visibility

By default, minor gridlines are not displayed. Enable them:

**C# Example:**
```csharp
sfSmithChart1.HorizontalAxis.MinorGridlinesVisible = true;
```

**VB.NET Example:**
```vb
sfSmithChart1.HorizontalAxis.MinorGridlinesVisible = True
```

#### Styling

| Property | Type | Description |
|----------|------|-------------|
| `MinorGridlinesDashStyle` | DashStyle | Line pattern |
| `MinorGridlinesColor` | Color | Line color |
| `MinorGridlinesWidth` | float | Line thickness |
| `MinorGridlinesCount` | int | Number of minor gridlines between major ones |

**C# Example:**
```csharp
sfSmithChart1.HorizontalAxis.MinorGridlinesVisible = true;
sfSmithChart1.HorizontalAxis.Style.MinorGridlinesDashStyle = System.Drawing.Drawing2D.DashStyle.Dash;
sfSmithChart1.HorizontalAxis.Style.MinorGridlinesColor = Color.Red;
sfSmithChart1.HorizontalAxis.Style.MinorGridlinesWidth = 1;
sfSmithChart1.HorizontalAxis.MinorGridlinesCount = 10;
```

**VB.NET Example:**
```vb
sfSmithChart1.HorizontalAxis.MinorGridlinesVisible = True
sfSmithChart1.HorizontalAxis.Style.MinorGridlinesDashStyle = System.Drawing.Drawing2D.DashStyle.Dash
sfSmithChart1.HorizontalAxis.Style.MinorGridlinesColor = Color.Red
sfSmithChart1.HorizontalAxis.Style.MinorGridlinesWidth = 1
sfSmithChart1.HorizontalAxis.MinorGridlinesCount = 10
```

### Axis Line

The axis line is the primary line of the axis itself.

#### Properties

| Property | Type | Description |
|----------|------|-------------|
| `AxisLineVisible` | bool | Show/hide axis line |
| `AxisLineDashStyle` | DashStyle | Line pattern |
| `AxisLineColor` | Color | Line color |

**C# Example:**
```csharp
sfSmithChart1.HorizontalAxis.AxisLineVisible = true;
sfSmithChart1.HorizontalAxis.Style.AxisLineColor = Color.Red;
sfSmithChart1.HorizontalAxis.Style.AxisLineDashStyle = DashStyle.Solid;
```

**VB.NET Example:**
```vb
sfSmithChart1.HorizontalAxis.AxisLineVisible = True
sfSmithChart1.HorizontalAxis.Style.AxisLineColor = Color.Red
sfSmithChart1.HorizontalAxis.Style.AxisLineDashStyle = DashStyle.Solid
```

### Label Placement

Position axis labels either inside or outside the chart plotting area.

**Values:**
- `LabelPlacement.Outside` (default)
- `LabelPlacement.Inside`

**C# Example:**
```csharp
sfSmithChart1.HorizontalAxis.LabelPlacement = LabelPlacement.Inside;
```

**VB.NET Example:**
```vb
sfSmithChart1.HorizontalAxis.LabelPlacement = LabelPlacement.Inside
```

### Label Intersect Action

Handle overlapping labels automatically.

**C# Example:**
```csharp
sfSmithChart1.HorizontalAxis.LabelIntersectAction = LabelIntersectActions.Hide;
``` 

**VB.NET Example:**
```vb
sfSmithChart1.HorizontalAxis.LabelIntersectAction = LabelIntersectActions.Hide
```

When set to `Hide`, overlapping labels are automatically hidden to maintain readability.

## Radial Axis

The radial axis represents:
- **Impedance Mode:** Normalized reactance values
- **Admittance Mode:** Normalized susceptance values

All properties available for the horizontal axis apply to the radial axis as well.

### Major Gridlines

#### Visibility

**C# Example:**
```csharp
sfSmithChart1.RadialAxis.MajorGridlinesVisible = false;
```

**VB.NET Example:**
```vb
sfSmithChart1.RadialAxis.MajorGridlinesVisible = False
```

#### Styling

**C# Example:**
```csharp
sfSmithChart1.RadialAxis.Style.MajorGridlinesDashStyle = System.Drawing.Drawing2D.DashStyle.Dash;
sfSmithChart1.RadialAxis.Style.MajorGridlinesColor = Color.Red;
sfSmithChart1.RadialAxis.Style.MajorGridlinesWidth = 1;
```

**VB.NET Example:**
```vb
sfSmithChart1.RadialAxis.Style.MajorGridlinesDashStyle = System.Drawing.Drawing2D.DashStyle.Dash
sfSmithChart1.RadialAxis.Style.MajorGridlinesColor = Color.Red
sfSmithChart1.RadialAxis.Style.MajorGridlinesWidth = 1
```

### Minor Gridlines

#### Visibility

**C# Example:**
```csharp
sfSmithChart1.RadialAxis.MinorGridlinesVisible = true;
```

**VB.NET Example:**
```vb
sfSmithChart1.RadialAxis.MinorGridlinesVisible = True
```

#### Styling

**C# Example:**
```csharp
sfSmithChart1.RadialAxis.MinorGridlinesVisible = true;
sfSmithChart1.RadialAxis.Style.MinorGridlinesColor = Color.PaleVioletRed;
sfSmithChart1.RadialAxis.Style.MinorGridlinesDashStyle = System.Drawing.Drawing2D.DashStyle.Dash;
sfSmithChart1.RadialAxis.Style.MinorGridlinesWidth = 1;
sfSmithChart1.RadialAxis.MinorGridlinesCount = 10;
```

**VB.NET Example:**
```vb
sfSmithChart1.RadialAxis.MinorGridlinesVisible = True
sfSmithChart1.RadialAxis.Style.MinorGridlinesColor = Color.PaleVioletRed
sfSmithChart1.RadialAxis.Style.MinorGridlinesDashStyle = System.Drawing.Drawing2D.DashStyle.Dash
sfSmithChart1.RadialAxis.Style.MinorGridlinesWidth = 1
sfSmithChart1.RadialAxis.MinorGridlinesCount = 10
```

### Axis Line

**C# Example:**
```csharp
sfSmithChart1.RadialAxis.AxisLineVisible = true;
sfSmithChart1.RadialAxis.Style.AxisLineColor = Color.Red;
sfSmithChart1.RadialAxis.Style.AxisLineDashStyle = DashStyle.Solid;
```

**VB.NET Example:**
```vb
sfSmithChart1.RadialAxis.AxisLineVisible = True
sfSmithChart1.RadialAxis.Style.AxisLineColor = Color.Red
sfSmithChart1.RadialAxis.Style.AxisLineDashStyle = DashStyle.Solid
```

### Label Placement

**C# Example:**
```csharp
sfSmithChart1.RadialAxis.LabelPlacement = LabelPlacement.Inside;
```

**VB.NET Example:**
```vb
sfSmithChart1.RadialAxis.LabelPlacement = LabelPlacement.Inside
```

### Label Intersect Action

**C# Example:**
```csharp
sfSmithChart1.RadialAxis.LabelIntersectAction = LabelIntersectActions.Hide;
```

**VB.NET Example:**
```vb
sfSmithChart1.RadialAxis.LabelIntersectAction = LabelIntersectActions.Hide
```

## Common Axis Properties

### Properties Summary

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `MajorGridlinesVisible` | bool | true | Show/hide major gridlines |
| `MinorGridlinesVisible` | bool | false | Show/hide minor gridlines |
| `MinorGridlinesCount` | int | 5 | Number of minor gridlines between major ones |
| `AxisLineVisible` | bool | true | Show/hide axis line |
| `LabelPlacement` | LabelPlacement | Outside | Label position (Inside/Outside) |
| `LabelIntersectAction` | LabelIntersectActions | Hide | How to handle overlapping labels |

### Style Properties

Access style properties through the `Style` property of each axis:

| Property | Type | Description |
|----------|------|-------------|
| `MajorGridlinesColor` | Color | Major gridline color |
| `MajorGridlinesWidth` | float | Major gridline thickness |
| `MajorGridlinesDashStyle` | DashStyle | Major gridline pattern |
| `MinorGridlinesColor` | Color | Minor gridline color |
| `MinorGridlinesWidth` | float | Minor gridline thickness |
| `MinorGridlinesDashStyle` | DashStyle | Minor gridline pattern |
| `AxisLineColor` | Color | Axis line color |
| `AxisLineDashStyle` | DashStyle | Axis line pattern |

## Events

### LabelCreated Event

The `LabelCreated` event is triggered when axis labels are created. Use it to customize label text dynamically.

**Event Arguments:**
- `Label.Text` - The label text to display
- Access to label properties for customization

**C# Example:**
```csharp
// Hook the event
sfSmithChart1.RadialAxis.LabelCreated += RadialAxis_LabelCreated;

private void RadialAxis_LabelCreated(object sender, EventArgs e)
{
    var axisLabel = e as ChartAxisLabelEventArgs;
    if (axisLabel.Label.Text == "1")
    {
        axisLabel.Label.Text = "One";
    }
}
```

**VB.NET Example:**
```vb
' Hook the event
AddHandler sfSmithChart1.RadialAxis.LabelCreated, AddressOf RadialAxis_LabelCreated

Private Sub RadialAxis_LabelCreated(ByVal sender As Object, ByVal e As EventArgs)
    Dim axisLabel = TryCast(e, ChartAxisLabelEventArgs)
    If axisLabel.Label.Text = "1" Then
        axisLabel.Label.Text = "One"
    End If
End Sub
```

**Use Cases:**
- Custom label formatting (e.g., adding units)
- Localization of label text
- Replacing numeric values with descriptive text
- Adding prefixes or suffixes to labels

## Common Patterns

### Pattern 1: Basic Grid Configuration

```csharp
// Show both major and minor gridlines
sfSmithChart1.HorizontalAxis.MajorGridlinesVisible = true;
sfSmithChart1.HorizontalAxis.MinorGridlinesVisible = true;
sfSmithChart1.RadialAxis.MajorGridlinesVisible = true;
sfSmithChart1.RadialAxis.MinorGridlinesVisible = true;
```

### Pattern 2: Subtle Minor Gridlines

```csharp
// Major gridlines - bold and solid
sfSmithChart1.HorizontalAxis.Style.MajorGridlinesWidth = 2;
sfSmithChart1.HorizontalAxis.Style.MajorGridlinesColor = Color.Gray;

// Minor gridlines - thin and dashed
sfSmithChart1.HorizontalAxis.MinorGridlinesVisible = true;
sfSmithChart1.HorizontalAxis.Style.MinorGridlinesWidth = 1;
sfSmithChart1.HorizontalAxis.Style.MinorGridlinesColor = Color.LightGray;
sfSmithChart1.HorizontalAxis.Style.MinorGridlinesDashStyle = System.Drawing.Drawing2D.DashStyle.Dash;
sfSmithChart1.HorizontalAxis.MinorGridlinesCount = 5;
```

### Pattern 3: Coordinated Axis Styling 

```csharp
// Apply same styling to both axes
void StyleAxis(ChartAxis axis, Color majorColor, Color minorColor)
{
    axis.MajorGridlinesVisible = true;
    axis.MinorGridlinesVisible = true;
    axis.Style.MajorGridlinesColor = majorColor;
    axis.Style.MinorGridlinesColor = minorColor;
    axis.Style.MajorGridlinesWidth = 2;
    axis.Style.MinorGridlinesWidth = 1;
    axis.MinorGridlinesCount = 10;
}

StyleAxis(sfSmithChart1.HorizontalAxis, Color.DarkBlue, Color.LightBlue);
StyleAxis(sfSmithChart1.RadialAxis, Color.DarkBlue, Color.LightBlue);
```

### Pattern 4: Custom Label Formatting

```csharp
sfSmithChart1.HorizontalAxis.LabelCreated += (sender, e) =>
{
    var args = e as ChartAxisLabelEventArgs;
    args.Label.Text = args.Label.Text + " Ω";  // Add ohm symbol
};
```

## Best Practices

1. **Consistency:** Apply similar styling to both axes for visual harmony

2. **Contrast:** Use sufficient contrast between gridlines and background

3. **Minor Gridlines:** Use sparingly; too many can clutter the chart

4. **Label Placement:** Use `Inside` placement when chart borders are constrained

5. **Grid Count:** Keep `MinorGridlinesCount` between 5-10 for optimal readability

6. **Performance:** Disable gridlines that aren't needed to improve rendering performance

7. **Color Choice:** Use light colors (e.g., LightGray) for gridlines to avoid overwhelming the data
