# Rendering Modes

## Overview

The Smith Chart supports two rendering modes for visualizing different types of transmission line parameters:

1. **Impedance Mode** - Displays resistance and reactance (default)
2. **Admittance Mode** - Displays conductance and susceptance

Switch between modes using the `RenderingMode` property.

## Impedance Smith Chart

The impedance Smith Chart is the default mode and most commonly used in RF engineering.

### Composition

The impedance Smith Chart consists of two families of circles:
- **Normalized resistance circles** - Horizontal family
- **Normalized reactance curves** - Radial family

### Value Distribution

- **Positive reactance values** - Appear in the upper region of the chart (inductive)
- **Negative reactance values** - Appear in the lower region of the chart (capacitive)
- **Zero reactance** - Lies on the horizontal axis

### Setting Impedance Mode

**C# Example:**
```csharp
SfSmithChart chart = new SfSmithChart();
chart.RenderingMode = RenderingMode.Impedance;
```

**VB.NET Example:**
```vb
Dim chart As New SfSmithChart()
chart.RenderingMode = RenderingMode.Impedance
```

### Use Cases

Use impedance mode when:
- Analyzing antenna impedance characteristics
- Designing impedance matching networks
- Measuring S-parameters with network analyzers
- Working with series circuit elements
- Visualizing reflection coefficients

### Data Interpretation

In impedance mode:
- **ResistanceMember** property maps to normalized resistance (R/Z₀)
- **ReactanceMember** property maps to normalized reactance (X/Z₀)
- Center of chart represents perfect match (Z = Z₀)
- Right edge represents open circuit
- Left edge represents short circuit

## Admittance Smith Chart

The admittance Smith Chart displays conductance and susceptance values, providing an alternative view of transmission line parameters.

### Composition

The admittance Smith Chart consists of:
- **Normalized conductance circles** - Horizontal family
- **Normalized susceptance curves** - Radial family

### Value Distribution

- **Positive susceptance values** - Appear in the lower region of the chart (capacitive)
- **Negative susceptance values** - Appear in the upper region of the chart (inductive)
- **Zero susceptance** - Lies on the horizontal axis

**Note:** The position of positive/negative values is inverted compared to impedance mode.

### Setting Admittance Mode

**C# Example:**
```csharp
SfSmithChart chart = new SfSmithChart();
chart.RenderingMode = RenderingMode.Admittance;
```

**VB.NET Example:**
```vb
Dim chart As New SfSmithChart()
chart.RenderingMode = RenderingMode.Admittance
```

### Use Cases

Use admittance mode when:
- Working with parallel circuit elements
- Analyzing shunt components
- Designing stub matching networks
- Converting between impedance and admittance
- Simplifying parallel calculations

### Data Interpretation

In admittance mode:
- **ResistanceMember** property maps to normalized conductance (G/Y₀)
- **ReactanceMember** property maps to normalized susceptance (B/Y₀)
- Center represents perfect match (Y = Y₀)
- Right edge represents short circuit
- Left edge represents open circuit

## Switching Between Modes

You can dynamically change the rendering mode at runtime:

**C# Example:**
```csharp
private void ToggleMode()
{
    if (sfSmithChart.RenderingMode == RenderingMode.Impedance)
    {
        sfSmithChart.RenderingMode = RenderingMode.Admittance;
    }
    else
    {
        sfSmithChart.RenderingMode = RenderingMode.Impedance;
    }
}
```

**VB.NET Example:**
```vb
Private Sub ToggleMode()
    If sfSmithChart.RenderingMode = RenderingMode.Impedance Then
        sfSmithChart.RenderingMode = RenderingMode.Admittance
    Else
        sfSmithChart.RenderingMode = RenderingMode.Impedance
    End If
End Sub
```

### Dynamic Mode Switching with Button

**C# Example:**
```csharp
private void btnToggleMode_Click(object sender, EventArgs e)
{
    if (sfSmithChart.RenderingMode == RenderingMode.Impedance)
    {
        sfSmithChart.RenderingMode = RenderingMode.Admittance;
        sfSmithChart.Text = "Admittance Smith Chart";
        btnToggleMode.Text = "Switch to Impedance";
    }
    else
    {
        sfSmithChart.RenderingMode = RenderingMode.Impedance;
        sfSmithChart.Text = "Impedance Smith Chart";
        btnToggleMode.Text = "Switch to Admittance";
    }
}
```

## Comparison: Impedance vs Admittance

| Aspect | Impedance Mode | Admittance Mode |
|--------|----------------|-----------------|
| Horizontal Axis | Resistance (R) | Conductance (G) |
| Radial Axis | Reactance (X) | Susceptance (B) |
| Positive Radial Values | Upper region (inductive) | Lower region (capacitive) |
| Negative Radial Values | Lower region (capacitive) | Upper region (inductive) |
| Center Point | Z = Z₀ | Y = Y₀ |
| Right Edge | Open circuit | Short circuit |
| Left Edge | Short circuit | Open circuit |
| Best For | Series elements | Parallel elements |

## Common Patterns

### Pattern 1: Default Impedance Setup

```csharp
SfSmithChart chart = new SfSmithChart();
chart.RenderingMode = RenderingMode.Impedance;  // Default, but explicit
chart.Text = "Impedance Analysis";

LineSeries series = new LineSeries();
series.DataSource = transmissionData;
series.ResistanceMember = "Resistance";  // R values
series.ReactanceMember = "Reactance";    // X values
chart.Series.Add(series);
```

### Pattern 2: Admittance Mode Setup

```csharp
SfSmithChart chart = new SfSmithChart();
chart.RenderingMode = RenderingMode.Admittance;
chart.Text = "Admittance Analysis";

LineSeries series = new LineSeries();
series.DataSource = admittanceData;
series.ResistanceMember = "Conductance";  // G values
series.ReactanceMember = "Susceptance";   // B values
chart.Series.Add(series);
```

### Pattern 3: Mode Toggle with Radio Buttons

```csharp
private void rbImpedance_CheckedChanged(object sender, EventArgs e)
{
    if (rbImpedance.Checked)
    {
        sfSmithChart.RenderingMode = RenderingMode.Impedance;
        sfSmithChart.Text = "Impedance Smith Chart";
    }
}

private void rbAdmittance_CheckedChanged(object sender, EventArgs e)
{
    if (rbAdmittance.Checked)
    {
        sfSmithChart.RenderingMode = RenderingMode.Admittance;
        sfSmithChart.Text = "Admittance Smith Chart";
    }
}
```

### Pattern 4: Mode-Aware Data Display

```csharp
private string GetAxisLabel()
{
    if (sfSmithChart.RenderingMode == RenderingMode.Impedance)
    {
        return "Resistance + j Reactance";
    }
    else
    {
        return "Conductance + j Susceptance";
    }
}

private void UpdateChartTitle()
{
    sfSmithChart.Text = GetAxisLabel() + " - Smith Chart";
}
```

## Best Practices

1. **Choose Mode Based on Circuit:**
   - Use impedance for series circuits and matching networks
   - Use admittance for parallel circuits and stub matching

2. **Clear Labeling:** Update chart title when switching modes to indicate current mode

3. **Data Model:** Use descriptive property names that reflect the mode:
   ```csharp
   // For impedance
   public class ImpedanceData
   {
       public double Resistance { get; set; }
       public double Reactance { get; set; }
   }
   
   // For admittance
   public class AdmittanceData
   {
       public double Conductance { get; set; }
       public double Susceptance { get; set; }
   }
   ```

4. **Default Mode:** Keep impedance as default unless application is specifically focused on admittance

5. **User Guidance:** Provide tooltips or help text explaining the difference between modes

6. **Conversion:** If allowing mode switching, ensure data is properly converted between impedance and admittance

## Mathematical Relationship

The relationship between impedance and admittance:

```
Z = R + jX  (Impedance)
Y = G + jB  (Admittance)

Y = 1/Z
G = R / (R² + X²)
B = -X / (R² + X²)
```

If implementing mode conversion in your application, use these formulas to transform data between modes.

## Troubleshooting

### Data Appears Inverted

- Check that you're using the correct rendering mode for your data type
- Verify that positive/negative values are in expected regions
- Remember that positive reactance (upper) maps to positive susceptance (lower)

### Mode Switch Doesn't Update Display

- Ensure the chart redraws after mode change (it should be automatic)
- Verify data binding is correct for both modes
- Check that property names in ResistanceMember/ReactanceMember are valid

### Incorrect Circle Orientation

- Confirm RenderingMode is set before adding series
- Verify you're interpreting the correct axes (horizontal vs radial)
- Check documentation images to compare expected layout
