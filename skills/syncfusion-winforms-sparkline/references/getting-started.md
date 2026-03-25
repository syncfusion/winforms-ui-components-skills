# Getting Started with Windows Forms Sparkline

This guide covers assembly deployment, adding the Sparkline control to your Windows Forms application, and basic configuration including data binding and marker setup.

## Assembly Deployment

### NuGet Package Installation

The Sparkline control is available via NuGet package for easy installation.

**To install the NuGet package:**

1. Right-click your Windows Forms project in Solution Explorer
2. Select "Manage NuGet Packages"
3. Search for "Syncfusion.Windows.Forms.Chart"
4. Install the package

**Package Details:**
- Package name: `Syncfusion.Windows.Forms.Chart`
- Contains: Sparkline control and dependencies

**Alternative: Manual Assembly Reference**

If not using NuGet, add these assembly references manually:
- `Syncfusion.Chart.Windows.dll`
- `Syncfusion.Shared.Base.dll`

**Installation Documentation:**
For detailed NuGet package installation instructions, refer to:
- [How to install NuGet packages in Windows Forms](https://help.syncfusion.com/windowsforms/installation/install-nuget-packages)
- [Control Dependencies](https://help.syncfusion.com/windowsforms/control-dependencies#sparkline)

## Adding Sparkline to Form

### Using the Visual Studio Designer

The easiest way to add a Sparkline control is through the Visual Studio designer.

**Step-by-step process:**

1. **Open your form in designer view**
   - Double-click your form file (.cs or .vb) in Solution Explorer
   - Switch to Design view if not already there

2. **Access the Syncfusion toolbox**
   - The Syncfusion installation automatically adds controls to your VS.NET toolbox
   - If controls are not visible, verify toolbox integration during installation
   - Look for "Syncfusion Controls" section in the toolbox

3. **Add the Sparkline control**
   - Locate the "SparkLine" control in the toolbox
   - Drag the control from the toolbox
   - Drop it onto your form
   - The control appears with default appearance

4. **Position and size the control**
   - Use the designer to resize the sparkline
   - Position it where needed on your form
   - Typical sizes: 100-200px width, 30-50px height for compact visualization

**Result:** The sparkline renders with default settings (empty line chart with white background).

### Programmatic Creation

You can also create sparklines programmatically:

```csharp
using Syncfusion.Windows.Forms.Chart;

// Create new sparkline instance
SparkLine sparkLine1 = new SparkLine();

// Set size and location
sparkLine1.Location = new Point(20, 20);
sparkLine1.Size = new Size(150, 40);

// Add to form
this.Controls.Add(sparkLine1);
```

## Basic Data Binding

Once the sparkline is added to your form, bind it to data to display values.

### Simple Array Binding

The most straightforward way to bind data is using a double array:

```csharp
// Set sparkline data source
this.sparkLine1.Source = new double[] { 30, -20, 80, 20, 40, -50, -30, 70, -40, 50 };

// Set sparkline type (optional - default is Line)
this.sparkLine1.Type = SparkLineType.Line;
```

**Data Interpretation:**
- Positive values: Rendered above baseline
- Negative values: Rendered below baseline
- Values are scaled automatically to fit sparkline bounds

### Supported Data Sources

The Sparkline control supports multiple data source types:

**1. Double Array** (shown above)
```csharp
double[] data = { 10, 20, 15, 30, 25 };
sparkLine1.Source = data;
```

**2. DataTable**
```csharp
DataTable dt = new DataTable();
dt.Columns.Add("Value", typeof(double));
dt.Rows.Add(30);
dt.Rows.Add(-20);
dt.Rows.Add(80);
sparkLine1.Source = dt;
```

**3. IEnumerable, ICollection, IList**
```csharp
List<double> values = new List<double> { 30, -20, 80, 20, 40 };
sparkLine1.Source = values;
```

## Customizing Sparkline Appearance

Customize the basic appearance of your sparkline with styling properties.

### Line Color Customization

For line-type sparklines, customize the line color:

```csharp
// C# example
this.sparkLine1.LineStyle.LineColor = System.Drawing.Color.Maroon;
```

```vb
' VB.NET example
Me.sparkLine1.LineStyle.LineColor = System.Drawing.Color.Maroon
```

**Result:** The sparkline line renders in maroon color instead of default.

## Adding Markers to Sparkline

Markers are visual indicators that highlight specific data points in the sparkline graph. They work with all sparkline types.

### Enabling Basic Markers

To show markers at every data point (line sparkline):

```csharp
// C# example
this.sparkLine1.Markers.ShowMarker = true;
```

```vb
' VB.NET example
Me.sparkLine1.Markers.ShowMarker = true
```

**Result:** Small circular markers appear at each data point location.

## Highlighting High and Low Values

A powerful feature is automatically highlighting the highest and lowest values in your data.

### Enabling High/Low Point Markers

```csharp
// C# example - Show high and low points
this.sparkLine1.Markers.ShowHighPoint = true;
this.sparkLine1.Markers.ShowLowPoint = true;
```

```vb
' VB.NET example - Show high and low points
Me.sparkLine1.Markers.ShowHighPoint = True
Me.sparkLine1.Markers.ShowLowPoint = True
```

### Customizing High/Low Point Colors

Make high and low points visually distinct with custom colors:

```csharp
// C# example - Customize marker colors
this.sparkLine1.Markers.ShowHighPoint = true;
this.sparkLine1.Markers.ShowLowPoint = true;
this.sparkLine1.Markers.ShowStartPoint = true;
this.sparkLine1.Markers.ShowEndPoint = true;
this.sparkLine1.Markers.ShowNegativePoint = true;

// Set colors for each marker type
this.sparkLine1.Markers.HighPointColor = new BrushInfo(Color.Blue);
this.sparkLine1.Markers.LowPointColor = new BrushInfo(Color.Green);
this.sparkLine1.Markers.StartPointColor = new BrushInfo(Color.Maroon);
this.sparkLine1.Markers.EndPointColor = new BrushInfo(Color.Purple);
this.sparkLine1.Markers.NegativePointColor = new BrushInfo(Color.Red);
```

```vb
' VB.NET example - Customize marker colors
Me.sparkLine1.Markers.ShowHighPoint = True
Me.sparkLine1.Markers.ShowLowPoint = True
Me.sparkLine1.Markers.ShowStartPoint = True
Me.sparkLine1.Markers.ShowEndPoint = True
Me.sparkLine1.Markers.ShowNegativePoint = True

' Set colors for each marker type
Me.sparkLine1.Markers.HighPointColor = New BrushInfo(Color.Blue)
Me.sparkLine1.Markers.LowPointColor = New BrushInfo(Color.Green)
Me.sparkLine1.Markers.StartPointColor = New BrushInfo(Color.Maroon)
Me.sparkLine1.Markers.EndPointColor = New BrushInfo(Color.Purple)
Me.sparkLine1.Markers.NegativePointColor = New BrushInfo(Color.Red)
```

**Marker Types Explained:**
- **HighPoint**: Highest value in dataset (Blue)
- **LowPoint**: Lowest value in dataset (Green)
- **StartPoint**: First data point (Maroon)
- **EndPoint**: Last data point (Purple)
- **NegativePoint**: All negative values (Red)

**Result:** Your sparkline now visually emphasizes key data points with distinct colors, making trends and outliers immediately obvious.

## Complete Getting Started Example

Here's a complete example combining all the basics:

```csharp
using System.Drawing;
using Syncfusion.Windows.Forms.Chart;

namespace SparklineDemo
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
            SetupSparkline();
        }

        private void SetupSparkline()
        {
            // Create sparkline
            SparkLine sparkLine1 = new SparkLine();
            
            // Position and size
            sparkLine1.Location = new Point(50, 50);
            sparkLine1.Size = new Size(200, 50);
            
            // Bind data
            sparkLine1.Source = new double[] { 30, -20, 80, 20, 40, -50, -30, 70, -40, 50 };
            sparkLine1.Type = SparkLineType.Line;
            
            // Customize appearance
            sparkLine1.LineStyle.LineColor = Color.DarkBlue;
            
            // Enable and customize markers
            sparkLine1.Markers.ShowHighPoint = true;
            sparkLine1.Markers.ShowLowPoint = true;
            sparkLine1.Markers.HighPointColor = new BrushInfo(Color.Green);
            sparkLine1.Markers.LowPointColor = new BrushInfo(Color.Red);
            
            // Add to form
            this.Controls.Add(sparkLine1);
        }
    }
}
```

## Next Steps

After completing this getting started guide, explore:
- **Sparkline Types**: Learn about Line, Column, and WinLoss sparkline types
- **Marker Customization**: Deep dive into all marker options and configurations
- **Appearance Customization**: Advanced styling for LineStyle, ColumnStyle, and BackInterior

## Complete Sample

A complete getting started sample is available at:
[SparklineGettingStarted Sample](https://www.syncfusion.com/downloads/support/directtrac/general/ze/SparklineGettingStarted-1907776967)
