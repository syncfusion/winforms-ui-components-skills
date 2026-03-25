# User Interactions

## Table of Contents
- [Tooltip](#tooltip)
- [Tooltip Format](#tooltip-format)
- [Tooltip Customization](#tooltip-customization)
- [TooltipOpening Event](#tooltipopening-event)

## Tooltip

Tooltips display information about data points when users hover over them with the mouse. This provides contextual information without cluttering the chart.

### Enabling Tooltips

Enable tooltips for a series using the `TooltipVisible` property:

**C# Example:**
```csharp
LineSeries series = new LineSeries();
series.TooltipVisible = true;
sfSmithChart1.Series.Add(series);
```

**VB.NET Example:**
```vb
Dim series As New LineSeries()
series.TooltipVisible = True
sfSmithChart1.Series.Add(series)
```

**When enabled:**
- Hover mouse over any data point
- Tooltip appears showing point values
- Tooltip follows mouse movement
- Tooltip hides when mouse moves away

## Tooltip Format

Customize the content displayed in tooltips using the `TooltipFormat` property.

### Format Placeholders

| Placeholder | Description |
|-------------|-------------|
| `{0}` | Resistance value (impedance) or conductance value (admittance) |
| `{1}` | Reactance value (impedance) or susceptance value (admittance) |
| `{2}` | Series name from the `LegendText` property |

### Default Format

By default, tooltips display only the resistance/conductance and reactance/susceptance values without labels.

### Custom Format Example

**C# Example:**
```csharp
series.TooltipFormat = "Resistance : {0}" + Environment.NewLine + 
                       "Reactance : {1}" + Environment.NewLine + 
                       "Series : {2}";
```

**VB.NET Example:**
```vb
series.TooltipFormat = "Resistance : {0}" & Environment.NewLine & 
                       "Reactance : {1}" & Environment.NewLine & 
                       "Series : {2}"
```

This displays:
```
Resistance : 1.5
Reactance : 0.5
Series : Transmission1
```

### Format Variations

**Simple with Units:**
```csharp
series.TooltipFormat = "R: {0} Ω" + Environment.NewLine + "X: {1} Ω";
```

**Descriptive Format:**
```csharp
series.TooltipFormat = "Impedance" + Environment.NewLine + 
                       "Real: {0}" + Environment.NewLine + 
                       "Imaginary: {1}";
```

**With Series Identification:**
```csharp
series.TooltipFormat = "{2}" + Environment.NewLine + 
                       "({0}, {1})";
```

## Tooltip Customization

Customize tooltip appearance using the `TooltipOptions` class.

### Customization Properties

| Property | Type | Description |
|----------|------|-------------|
| `BackColor` | Color | Background color of tooltip |
| `ForeColor` | Color | Text color |
| `BorderColor` | Color | Border color |
| `BorderWidth` | int | Border thickness in pixels |
| `Font` | Font | Font for tooltip text |
| `ShadowVisible` | bool | Show/hide tooltip shadow |

### Complete Customization Example

**C# Example:**
```csharp
sfSmithChart1.TooltipOptions.BackColor = Color.Green;
sfSmithChart1.TooltipOptions.BorderColor = Color.Black;
sfSmithChart1.TooltipOptions.ForeColor = Color.White;
sfSmithChart1.TooltipOptions.ShadowVisible = true;
sfSmithChart1.TooltipOptions.Font = new Font("Verdana", 10);
sfSmithChart1.TooltipOptions.BorderWidth = 1;
```

**VB.NET Example:**
```vb
sfSmithChart1.TooltipOptions.BackColor = Color.Green
sfSmithChart1.TooltipOptions.BorderColor = Color.Black
sfSmithChart1.TooltipOptions.ForeColor = Color.White
sfSmithChart1.TooltipOptions.ShadowVisible = True
sfSmithChart1.TooltipOptions.Font = New Font("Verdana", 10)
sfSmithChart1.TooltipOptions.BorderWidth = 1
```

This creates:
- Green background
- White text
- Black border (1 pixel)
- Verdana font (10pt)
- Drop shadow effect

### Style Variations

**Professional Style:**
```csharp
sfSmithChart1.TooltipOptions.BackColor = Color.White;
sfSmithChart1.TooltipOptions.ForeColor = Color.Black;
sfSmithChart1.TooltipOptions.BorderColor = Color.DarkGray;
sfSmithChart1.TooltipOptions.BorderWidth = 1;
sfSmithChart1.TooltipOptions.Font = new Font("Segoe UI", 9);
sfSmithChart1.TooltipOptions.ShadowVisible = true;
```

**High Contrast:**
```csharp
sfSmithChart1.TooltipOptions.BackColor = Color.Black;
sfSmithChart1.TooltipOptions.ForeColor = Color.Yellow;
sfSmithChart1.TooltipOptions.BorderColor = Color.Yellow;
sfSmithChart1.TooltipOptions.BorderWidth = 2;
sfSmithChart1.TooltipOptions.Font = new Font("Arial", 10, FontStyle.Bold);
sfSmithChart1.TooltipOptions.ShadowVisible = false;
```

**Subtle Style:**
```csharp
sfSmithChart1.TooltipOptions.BackColor = Color.FromArgb(240, 240, 240);
sfSmithChart1.TooltipOptions.ForeColor = Color.FromArgb(60, 60, 60);
sfSmithChart1.TooltipOptions.BorderColor = Color.FromArgb(200, 200, 200);
sfSmithChart1.TooltipOptions.BorderWidth = 1;
sfSmithChart1.TooltipOptions.Font = new Font("Calibri", 9);
sfSmithChart1.TooltipOptions.ShadowVisible = true;
```

## TooltipOpening Event

The `TooltipOpening` event fires before a tooltip is displayed, allowing dynamic customization based on the specific data point.

### Event Arguments

| Property | Type | Description |
|----------|------|-------------|
| `Data` | object | The data object for the point (cast to your data model) |
| `SeriesIndex` | double | Index of the series in the Series collection |
| `PointIndex` | double | Index of the point within its series |
| `Text` | string | Tooltip text (can be modified) |
| `Style` | TooltipOptions  | Tooltip appearance (can be modified) |

### Basic Event Usage

**C# Example:**
```csharp
// Hook the event
sfSmithChart1.TooltipOpening += SfSmithChart1_TooltipOpening;

private void SfSmithChart1_TooltipOpening(object sender, TooltipOpeningEventArgs e)
{
    var data = e.Data as TransmissionData;
    if (e.PointIndex == 1)
    {
        e.Text += Environment.NewLine + "Frequency: " + data.Frequency;
        e.Style.ForeColor = Color.Red;
        e.Style.BackColor = Color.Yellow;
    }
}
```

**VB.NET Example:**
```vb
' Hook the event
AddHandler sfSmithChart1.TooltipOpening, AddressOf SfSmithChart1_TooltipOpening

Private Sub SfSmithChart1_TooltipOpening(ByVal sender As Object, ByVal e As TooltipOpeningEventArgs)
    Dim data = TryCast(e.Data, TransmissionData)
    
    If e.PointIndex = 1 Then
        e.Text += Environment.NewLine & "Frequency: " & data.Frequency
        e.Style.ForeColor = Color.Red
        e.Style.BackColor = Color.Yellow
    End If
End Sub
```

### Advanced Event Scenarios

**Conditional Formatting:**
```csharp
private void SfSmithChart1_TooltipOpening(object sender, TooltipOpeningEventArgs e)
{
    var data = e.Data as TransmissionData;
    
    // Highlight out-of-range values
    if (data.Resistance > 10)
    {
        e.Style.BackColor = Color.Red;
        e.Style.ForeColor = Color.White;
        e.Text = "⚠ HIGH RESISTANCE" + Environment.NewLine + e.Text;
    }
}
```

**Series-Specific Styling:**
```csharp
private void SfSmithChart1_TooltipOpening(object sender, TooltipOpeningEventArgs e)
{
    if (e.SeriesIndex == 0)
    {
        // First series - blue theme
        e.Style.BackColor = Color.LightBlue;
        e.Style.ForeColor = Color.DarkBlue;
    }
    else if (e.SeriesIndex == 1)
    {
        // Second series - green theme
        e.Style.BackColor = Color.LightGreen;
        e.Style.ForeColor = Color.DarkGreen;
    }
}
```

**Adding Calculated Values:**
```csharp
private void SfSmithChart1_TooltipOpening(object sender, TooltipOpeningEventArgs e)
{
    var data = e.Data as TransmissionData;
    
    // Calculate magnitude
    double magnitude = Math.Sqrt(
        Math.Pow(data.Resistance, 2) + 
        Math.Pow(data.Reactance, 2)
    );
    
    e.Text += Environment.NewLine + 
              string.Format("Magnitude: {0:F2}", magnitude);
}
```

**Custom Data Display:**
```csharp
// Assuming your data model has additional properties
public class TransmissionData
{
    public double Resistance { get; set; }
    public double Reactance { get; set; }
    public double Frequency { get; set; }
    public string Measurement { get; set; }
}

private void SfSmithChart1_TooltipOpening(object sender, TooltipOpeningEventArgs e)
{
    var data = e.Data as TransmissionData;
    
    e.Text = $"Measurement: {data.Measurement}" + Environment.NewLine +
             $"Frequency: {data.Frequency} MHz" + Environment.NewLine +
             $"R: {data.Resistance:F2} Ω" + Environment.NewLine +
             $"X: {data.Reactance:F2} Ω";
}
```

## Common Patterns

### Pattern 1: Basic Tooltip Setup

```csharp
LineSeries series = new LineSeries();
series.TooltipVisible = true;
series.TooltipFormat = "R: {0}" + Environment.NewLine + "X: {1}";
sfSmithChart.Series.Add(series);
```

### Pattern 2: Styled Tooltips

```csharp
// Enable tooltips for series
series.TooltipVisible = true;

// Customize appearance
sfSmithChart.TooltipOptions.BackColor = Color.Navy;
sfSmithChart.TooltipOptions.ForeColor = Color.White;
sfSmithChart.TooltipOptions.BorderColor = Color.Cyan;
sfSmithChart.TooltipOptions.BorderWidth = 2;
sfSmithChart.TooltipOptions.Font = new Font("Courier New", 9);
sfSmithChart.TooltipOptions.ShadowVisible = true;
```

### Pattern 3: Dynamic Tooltips with Event

```csharp
series.TooltipVisible = true;
sfSmithChart.TooltipOpening += (sender, e) =>
{
    var data = e.Data as TransmissionData;
    e.Text = $"Point {e.PointIndex + 1}" + Environment.NewLine +
             $"Resistance: {data.Resistance:F3} Ω" + Environment.NewLine +
             $"Reactance: {data.Reactance:F3} Ω";
};
```

### Pattern 4: Color-Coded Tooltips

```csharp
sfSmithChart.TooltipOpening += (sender, e) =>
{
    var data = e.Data as TransmissionData;
    
    // Color code based on data values
    if (data.Resistance < 1)
        e.Style.BackColor = Color.LightGreen;
    else if (data.Resistance < 5)
        e.Style.BackColor = Color.Yellow;
    else
        e.Style.BackColor = Color.LightCoral;
};
```

## Best Practices

1. **Enable Selectively:** Only enable tooltips when users benefit from additional information

2. **Keep It Concise:** Display essential information only; too much text makes tooltips hard to read

3. **Use Meaningful Labels:** Include labels in `TooltipFormat` (e.g., "Resistance:" not just the value)

4. **Consistent Styling:** Match tooltip colors to your application theme

5. **Readable Fonts:** Use clear, legible fonts sized 9-11pt

6. **Shadow for Depth:** Enable `ShadowVisible` for better tooltip distinction from chart

7. **Event for Complexity:** Use `TooltipOpening` event for conditional formatting or calculated values

8. **Performance:** Avoid heavy calculations in `TooltipOpening` event handler as it fires frequently

## Troubleshooting

### Tooltips Not Appearing

- Verify `TooltipVisible = true` is set on the series
- Check that data points exist and are within chart bounds
- Ensure mouse events are not blocked by overlaying controls

### Tooltip Text Not Formatted

- Confirm `TooltipFormat` is set with valid placeholders ({0}, {1}, {2})
- Use `Environment.NewLine` for line breaks (not "\n")
- Verify series has `LegendText` set if using {2} placeholder

### Custom Styling Not Applied

- Check that `TooltipOptions` properties are set on the chart object (not series)
- Verify colors have sufficient contrast for visibility
- Ensure `Font` object is properly created

### Event Not Firing

- Confirm event handler is hooked: `sfSmithChart.TooltipOpening += Handler;`
- Verify tooltips are enabled on at least one series
- Check that handler method signature matches `TooltipOpeningEventArgs`
