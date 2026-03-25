# Getting Started with Smith Charts

## Table of Contents
- [Assembly Deployment](#assembly-deployment)
- [Creating Through Designer](#creating-through-designer)
- [Creating Through Code](#creating-through-code)
- [Populating Data](#populating-data)
- [Initialize the Smith Chart](#initialize-the-smith-chart)
- [Adding Header](#adding-header)
- [Adding Axes](#adding-axes)
- [Adding Series](#adding-series)
- [Adding Legends](#adding-legends)

## Assembly Deployment

To use the Smith Chart control in your application, you need to add the following assemblies as references:

### Required Assemblies

1. **Syncfusion.SfSmithChart.WinForms** - Main Smith Chart assembly
2. **Syncfusion.Core.WinForms** - Core Syncfusion functionality

### NuGet Package Installation

Install the required NuGet package using Package Manager Console:

```powershell
Install-Package Syncfusion.SfSmithChart.WinForms
```

Or use the NuGet Package Manager UI in Visual Studio to search for and install `Syncfusion.SfSmithChart.WinForms`.

## Creating Through Designer

The Smith Chart control can be added to your Windows Form through the Visual Studio designer.

### Steps

1. **Create a new Windows Form Application**
2. **Open the Toolbox** (View → Toolbox)
3. **Locate SfSmithChart** in the Syncfusion controls section
4. **Drag and drop** the control onto the form designer

When you drop the control, the required assemblies are automatically added to your project:
- Syncfusion.SfSmithChart.WinForms
- Syncfusion.Core.WinForms

### Designer Properties

After adding the control, you can configure it through the Properties window:

- Set appearance properties like `BackColor`
- Configure axis properties like `MinorGridlinesVisible`
- Set the chart title using the `Text` property
- Adjust layout with `Dock` or `Anchor` properties

**Example:** To show minor gridlines on the radial axis, select the control and in the Properties window, expand `RadialAxis` → set `MinorGridlinesVisible` to `True`.

## Creating Through Code

For programmatic control creation, follow these steps.

### Step 1: Add Assembly References

Manually add references to:
1. Syncfusion.SfSmithChart.WinForms
2. Syncfusion.Core.WinForms

### Step 2: Import Namespace

Add the required namespace at the top of your code file:

**C# Example:**
```csharp
using Syncfusion.WinForms.SmithChart;
```

**VB.NET Example:**
```vb
Imports Syncfusion.WinForms.SmithChart
```

### Step 3: Create Control Instance

Create an instance of `SfSmithChart` and add it to your form:

**C# Example:**
```csharp
SfSmithChart chart = new SfSmithChart();
this.Controls.Add(chart);
```

**VB.NET Example:**
```vb
Dim chart As New SfSmithChart()
Me.Controls.Add(chart)
```

## Populating Data

There are two ways to add data to the Smith Chart:

1. **By specifying DataSource** - Bind a data collection
2. **By directly adding points** - Add individual points to series

### Method 1: By Specifying DataSource

This is the recommended approach for binding to existing data collections.

#### Required Properties

| Property | Description |
|----------|-------------|
| `DataSource` | The data collection to bind (ObservableCollection, List, etc.) |
| `ResistanceMember` | Property name for resistance values (impedance) or conductance values (admittance) |
| `ReactanceMember` | Property name for reactance values (impedance) or susceptance values (admittance) |

#### Data Model

Create a class to represent transmission data:

**C# Example:**
```csharp
public class TransmissionData
{
    public double Resistance { get; set; }
    public double Reactance { get; set; }
}
```

**VB.NET Example:**
```vb
Public Class TransmissionData
    Public Property Resistance As Double
    Public Property Reactance As Double
End Class
```

#### Model Class with Data Collection

**C# Example:**
```csharp
public class SmithChartModel
{
    public SmithChartModel()
    {
        Trace1 = new ObservableCollection<TransmissionData>();
        Trace1.Add(new TransmissionData() { Resistance = 0, Reactance = 0.05 });
        Trace1.Add(new TransmissionData() { Resistance = 0.3, Reactance = 0.1 });
        Trace1.Add(new TransmissionData() { Resistance = 0.5, Reactance = 0.2 });
        Trace1.Add(new TransmissionData() { Resistance = 1.0, Reactance = 0.4 });
        Trace1.Add(new TransmissionData() { Resistance = 1.5, Reactance = 0.5 });
        Trace1.Add(new TransmissionData() { Resistance = 2.0, Reactance = 0.5 });
        Trace1.Add(new TransmissionData() { Resistance = 2.5, Reactance = 0.4 });
        Trace1.Add(new TransmissionData() { Resistance = 3.5, Reactance = 0.0 });
        Trace1.Add(new TransmissionData() { Resistance = 4.5, Reactance = -0.5 });
        Trace1.Add(new TransmissionData() { Resistance = 5, Reactance = -1.0 });
        Trace1.Add(new TransmissionData() { Resistance = 6, Reactance = -1.5 });
        Trace1.Add(new TransmissionData() { Resistance = 7, Reactance = -2.5 });
        Trace1.Add(new TransmissionData() { Resistance = 8, Reactance = -3.5 });
        Trace1.Add(new TransmissionData() { Resistance = 9, Reactance = -4.5 });
        Trace1.Add(new TransmissionData() { Resistance = 10, Reactance = -10 });
        Trace1.Add(new TransmissionData() { Resistance = 20, Reactance = -50 });
    }

    public ObservableCollection<TransmissionData> Trace1 { get; set; }
}
```

#### Binding to Series

**C# Example:**
```csharp
public partial class SmithChartSample
{
    public SmithChartSample()
    {
        SmithChartModel model = new SmithChartModel();
        sfSmithChart.BackColor = Color.White;
        
        LineSeries series = new LineSeries();
        series.MarkerVisible = true;
        series.TooltipVisible = true;
        series.LegendText = "Transmission";
        series.DataSource = model.Trace1;
        series.ResistanceMember = "Resistance";
        series.ReactanceMember = "Reactance";
        
        sfSmithChart1.Series.Add(series);
    }
}
```

**VB.NET Example:**
```vb
Public Partial Class SmithChartSample
    Public Sub New()
        Dim model As SmithChartModel = New SmithChartModel()
        sfSmithChart.BackColor = Color.White
        
        Dim series As LineSeries = New LineSeries()
        series.MarkerVisible = True
        series.TooltipVisible = True
        series.LegendText = "Transmission"
        series.DataSource = model.Trace1
        series.ResistanceMember = "Resistance"
        series.ReactanceMember = "Reactance"
        
        sfSmithChart1.Series.Add(series)
    End Sub
End Class
```

### Method 2: By Directly Adding Points

For dynamic scenarios or when you don't have a predefined data collection:

**C# Example:**
```csharp
LineSeries lineSeries = sfSmithChart.Series[0] as LineSeries;
Random random = new Random();
for (int i = 0; i < 100; i++)
{
    double val = random.Next(0, 5);
    double val1 = random.Next(-5, 5);
    lineSeries.Points.Add(val, val1);
}
```

**VB.NET Example:**
```vb
Dim lineSeries As LineSeries = TryCast(sfSmithChart.Series(0), LineSeries)
Dim random As Random = New Random()

For i As Integer = 0 To 99
    Dim val As Double = random.Next(0, 5)
    Dim val1 As Double = random.Next(-5, 5)
    lineSeries.Points.Add(val, val1)
Next
```

## Initialize the Smith Chart

**C# Example:**
```csharp
SfSmithChart chart = new SfSmithChart();
this.Controls.Add(chart);
```

**VB.NET Example:**
```vb
Dim chart As New SfSmithChart()
Me.Controls.Add(chart)
```

## Adding Header

Set the chart title using the `Text` property:

**C# Example:**
```csharp
chart.Text = "Impedance Transmission";
```

**VB.NET Example:**
```vb
chart.Text = "Impedance Transmission"
```

## Adding Axes

By default, both horizontal and radial axes are automatically added to the Smith Chart. You can customize them directly without manual initialization.

### Horizontal Axis

Represents resistance values (impedance mode) or conductance values (admittance mode).

### Radial Axis

Represents reactance values (impedance mode) or susceptance values (admittance mode).

### Customization Example

Enable minor gridlines on both axes:

**C# Example:**
```csharp
chart.HorizontalAxis.MinorGridlinesVisible = true;
chart.RadialAxis.MinorGridlinesVisible = true;
```

**VB.NET Example:**
```vb
chart.HorizontalAxis.MinorGridlinesVisible = True
chart.RadialAxis.MinorGridlinesVisible = True
```

## Adding Series

Create a `LineSeries` to plot data on the Smith Chart:

**C# Example:**
```csharp
LineSeries series = new LineSeries();
series.MarkerVisible = true;
series.DataSource = model.Trace1;
series.ResistanceMember = "Resistance";
series.ReactanceMember = "Reactance";
chart.Series.Add(series);
```

**VB.NET Example:**
```vb
Dim series As New LineSeries()
series.MarkerVisible = True
series.DataSource = model.Trace1
series.ResistanceMember = "Resistance"
series.ReactanceMember = "Reactance"
chart.Series.Add(series)
```

## Adding Legends

Enable the legend to display series information:

**C# Example:**
```csharp
chart.Legend.Visible = true;
```

**VB.NET Example:**
```vb
chart.Legend.Visible = True
```

Set the legend text for each series:

**C# Example:**
```csharp
series.LegendText = "Transmission1";
```

**VB.NET Example:**
```vb
series.LegendText = "Transmission1"
```

## Complete Example

**C# Example:**
```csharp
SfSmithChart chart = new SfSmithChart();
chart.Text = "Impedance Transmission";
chart.BackColor = Color.White;
chart.HorizontalAxis.MinorGridlinesVisible = true;
chart.RadialAxis.MinorGridlinesVisible = true;

LineSeries series = new LineSeries();
series.MarkerVisible = true;
series.LegendText = "Transmission1";
series.DataSource = model.Trace1;
series.ResistanceMember = "Resistance";
series.ReactanceMember = "Reactance";
chart.Series.Add(series);

chart.Legend.Visible = true;
chart.Dock = DockStyle.Fill;
this.Controls.Add(chart);
```

**VB.NET Example:**
```vb
Dim chart As New SfSmithChart()
chart.Text = "Impedance Transmission"
chart.BackColor = Color.White
chart.HorizontalAxis.MinorGridlinesVisible = True
chart.RadialAxis.MinorGridlinesVisible = True

Dim series As New LineSeries()
series.MarkerVisible = True
series.LegendText = "Transmission1"
series.DataSource = model.Trace1
series.ResistanceMember = "Resistance"
series.ReactanceMember = "Reactance"
chart.Series.Add(series)

chart.Legend.Visible = True
chart.Dock = DockStyle.Fill
Me.Controls.Add(chart)
```
