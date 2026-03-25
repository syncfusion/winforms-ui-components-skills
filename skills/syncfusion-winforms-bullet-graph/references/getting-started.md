# Getting Started with Windows Forms Bullet Graph

## Table of Contents
- [Overview](#overview)
- [Assembly Deployment](#assembly-deployment)
- [NuGet Package Installation](#nuget-package-installation)
- [Creating Bullet Graph Programmatically](#creating-bullet-graph-programmatically)
- [Using Syncfusion Reference Manager](#using-syncfusion-reference-manager)
- [First Working Example](#first-working-example)
- [Next Steps](#next-steps)

## Overview

The Bullet Graph is a variation of the bar graph that serves as a replacement for dashboard gauges and meters. It provides a compact, information-dense visualization perfect for dashboard environments where screen real-estate is limited. The control features a single primary measure (featured measure) compared to one or more other measures (comparative measure) within the context of qualitative performance ranges.

**Key Components:**
- **Featured Measure**: Current year-to-date revenue (or any primary metric)
- **Comparative Measure**: Target or benchmark value
- **Qualitative Ranges**: Performance bands (poor, satisfactory, good)
- **Quantitative Scale**: Ticks and labels showing numeric values
- **Caption**: Label describing the metric

**Use Cases:**
- Dashboard environments requiring efficient use of space
- Displaying large amounts of data compactly
- Revenue analysis and expense tracking
- KPI visualization and performance monitoring

## Assembly Deployment

To use the Bullet Graph control in your Windows Forms application, you need to reference the required assemblies.

**Required Assembly:**
- `Syncfusion.BulletGraph.Windows.dll`

**Common Shared Assembly (often required):**
- `Syncfusion.Shared.Windows.dll`

**Assembly Location:**
After installing Syncfusion Essential Studio, assemblies are typically located at:
```
C:\Program Files (x86)\Syncfusion\Essential Studio\<version>\precompiledassemblies\<.NET version>\
```

**To Add Assembly Reference Manually:**
1. Right-click **References** in Solution Explorer
2. Select **Add Reference...**
3. Browse to the assembly location
4. Select `Syncfusion.BulletGraph.Windows.dll`
5. Click **OK**

## NuGet Package Installation

The recommended approach is to use NuGet packages for easier dependency management and updates.

**Package Name:**
```
Syncfusion.BulletGraph.WinForms
```

**Installation via Package Manager Console:**
```powershell
Install-Package Syncfusion.BulletGraph.WinForms
```

**Installation via NuGet Package Manager UI:**
1. Right-click project in Solution Explorer
2. Select **Manage NuGet Packages...**
3. Search for `Syncfusion.BulletGraph.WinForms`
4. Click **Install**

**Package Dependencies:**
The NuGet package automatically handles dependencies, including:
- Syncfusion.Licensing
- Syncfusion.Shared.Windows

For more details on NuGet package installation, see:
[Syncfusion NuGet Installation Guide](https://help.syncfusion.com/windowsforms/installation/install-nuget-packages)

## Creating Bullet Graph Programmatically

### Required Namespace

Add the following namespace to your code file:

```csharp
using Syncfusion.Windows.Forms.BulletGraph;
```

### Basic Creation Steps

**Step 1: Create a Windows Forms Application**

Create a new Windows Forms project in Visual Studio:
- File → New → Project
- Select **Windows Forms App (.NET Framework)**
- Choose appropriate .NET Framework version (.NET 8 or higher)
- Click **Create**

**Step 2: Add Assembly References**

Add reference to `Syncfusion.BulletGraph.Windows.dll` as described in the Assembly Deployment section.

**Step 3: Add Bullet Graph in Code**

In your Form class (e.g., `Form1.cs`), add the following code:

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.BulletGraph;

namespace BulletGraphDemo
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
            CreateBulletGraph();
        }

        private void CreateBulletGraph()
        {
            // Create Bullet Graph instance
            BulletGraph bullet = new BulletGraph();
            
            // Set layout
            bullet.Dock = DockStyle.Fill;
            
            // Set orientation and flow
            bullet.FlowDirection = BulletGraphFlowDirection.Forward;
            bullet.Orientation = Orientation.Horizontal;
            
            // Set measure values
            bullet.FeaturedMeasure = 4.5;
            bullet.ComparativeMeasure = 7;
            
            // Configure scale
            bullet.LabelFontSize = 10;
            bullet.LabelStroke = Color.Black;
            bullet.MajorTickStroke = Color.Black;
            bullet.Minimum = 0;
            bullet.Maximum = 10;
            bullet.Interval = 2;
            bullet.MinorTicksPerInterval = 3;
            
            // Add qualitative ranges
            bullet.QualitativeRanges.Add(new QualitativeRange() 
            { 
                RangeEnd = 4, 
                RangeCaption = "Bad", 
                RangeStroke = Color.Red 
            });
            
            bullet.QualitativeRanges.Add(new QualitativeRange() 
            { 
                RangeEnd = 7, 
                RangeCaption = "Satisfactory", 
                RangeStroke = Color.Yellow 
            });
            
            bullet.QualitativeRanges.Add(new QualitativeRange() 
            { 
                RangeEnd = 10, 
                RangeCaption = "Good", 
                RangeStroke = Color.Green 
            });
            
            // Add to form
            this.Controls.Add(bullet);
        }
    }
}
```

**Step 4: Run the Application**

Press F5 or click the Start button to run the application. You should see a bullet graph with three colored ranges (red, yellow, green), a featured measure bar at 4.5, and a comparative measure line at 7.

## Using Syncfusion Reference Manager

Syncfusion provides a Visual Studio extension that simplifies adding controls to your project.

**Prerequisites:**
- Visual Studio 2022 or later
- Syncfusion Essential Studio version 11.3.0.30 or later
- Syncfusion VS Extensions installed

### Steps to Use Reference Manager

**Step 1: Create Windows Forms Application**

Create a new Windows Forms project in Visual Studio.

**Step 2: Open Syncfusion Reference Manager**

Right-click on your project in Solution Explorer and select **Syncfusion Reference Manager**.

**Step 3: Search for Bullet Graph**

In the Syncfusion Reference Manager wizard:
1. Use the search box to search for "Bullet Graph"
2. Select the **Bullet Graph** control from the results
3. Click **Done** to add the control

**Step 4: Assemblies Added Automatically**

The Reference Manager automatically adds the required assemblies to your project:
- `Syncfusion.BulletGraph.Windows`
- `Syncfusion.Shared.Windows` (if needed)

**Step 5: Add Bullet Graph in Code**

Add the following code to your form:

```csharp
using Syncfusion.Windows.Forms.BulletGraph;

public partial class Form1 : Form
{
    public Form1()
    {
        InitializeComponent();
        
        BulletGraph bullet = new BulletGraph();
        bullet.Dock = DockStyle.Fill;
        bullet.FlowDirection = BulletGraphFlowDirection.Forward;
        bullet.Orientation = Orientation.Horizontal;
        bullet.FeaturedMeasure = 4.5;
        bullet.ComparativeMeasure = 7;
        bullet.LabelFontSize = 10;
        bullet.LabelStroke = Color.Black;
        bullet.MajorTickStroke = Color.Black;
        bullet.Minimum = 0;
        bullet.Maximum = 10;
        bullet.Interval = 2;
        bullet.MinorTicksPerInterval = 3;
        
        bullet.QualitativeRanges.Add(new QualitativeRange() 
            { RangeEnd = 4, RangeCaption = "Bad", RangeStroke = Color.Red });
        bullet.QualitativeRanges.Add(new QualitativeRange() 
            { RangeEnd = 7, RangeCaption = "Satisfactory", RangeStroke = Color.Yellow });
        bullet.QualitativeRanges.Add(new QualitativeRange() 
            { RangeEnd = 10, RangeCaption = "Good", RangeStroke = Color.Green });
        
        this.Controls.Add(bullet);
    }
}
```

**Step 6: Run the Application**

The application will display the bullet graph control.

## First Working Example

Here's a complete, minimal working example that demonstrates all essential features:

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.BulletGraph;

namespace MinimalBulletGraph
{
    public class Program
    {
        [STAThread]
        static void Main()
        {
            Application.EnableVisualStyles();
            Application.SetCompatibleTextRenderingDefault(false);
            Application.Run(new MainForm());
        }
    }
    
    public class MainForm : Form
    {
        public MainForm()
        {
            this.Text = "Bullet Graph Demo";
            this.Size = new Size(800, 200);
            
            // Create bullet graph
            BulletGraph bullet = new BulletGraph();
            bullet.Dock = DockStyle.Fill;
            
            // Basic configuration
            bullet.Orientation = Orientation.Horizontal;
            bullet.FlowDirection = BulletGraphFlowDirection.Forward;
            
            // Set values
            bullet.FeaturedMeasure = 65;     // Actual: 65%
            bullet.ComparativeMeasure = 80;  // Target: 80%
            
            // Configure scale
            bullet.Minimum = 0;
            bullet.Maximum = 100;
            bullet.Interval = 20;
            bullet.MinorTicksPerInterval = 4;
            
            // Add performance ranges
            bullet.QualitativeRanges.Add(new QualitativeRange() 
            { 
                RangeEnd = 40, 
                RangeStroke = Color.FromArgb(220, 220, 220) 
            });
            
            bullet.QualitativeRanges.Add(new QualitativeRange() 
            { 
                RangeEnd = 70, 
                RangeStroke = Color.FromArgb(180, 180, 180) 
            });
            
            bullet.QualitativeRanges.Add(new QualitativeRange() 
            { 
                RangeEnd = 100, 
                RangeStroke = Color.FromArgb(140, 140, 140) 
            });
            
            // Add caption
            bullet.Caption = "Performance Score\nPercentage";
            
            this.Controls.Add(bullet);
        }
    }
}
```

## Next Steps

Now that you have a working Bullet Graph, explore these additional features:

1. **Customize Measures**: Learn how to style the featured and comparative measures
   - Read the `measures.md` reference file

2. **Configure Ranges**: Understand qualitative range configuration in depth
   - Read the `qualitative-ranges.md` reference file

3. **Customize Scale**: Learn about ticks, labels, and scale customization
   - Read the `scale-and-ticks.md` and `scale-labels.md` reference files

4. **Layout Options**: Explore orientation, flow direction, and caption positioning
   - Read the `layout-and-orientation.md` reference file

## Common Gotchas

### Issue: Bullet Graph Not Visible

**Problem**: Control added but nothing displays.

**Solution**: Ensure you've set either `Dock` property or explicit `Size` and `Location`:
```csharp
// Option 1: Use Dock
bullet.Dock = DockStyle.Fill;

// Option 2: Set explicit size
bullet.Size = new Size(600, 100);
bullet.Location = new Point(10, 10);
```

### Issue: Ranges Not Showing

**Problem**: Qualitative ranges are configured but not visible.

**Solution**: Ensure `RangeEnd` values are within `Minimum` and `Maximum` scale range, and ranges are added in ascending order of `RangeEnd`.

### Issue: Assembly Not Found

**Problem**: Build error about missing Syncfusion assemblies.

**Solution**: 
- Verify assembly references are added correctly
- Check that the assembly version matches your installed Syncfusion version
- Use NuGet package installation for automatic dependency management

### Issue: Featured Measure Not Visible

**Problem**: The featured measure bar doesn't appear.

**Solution**: Ensure `FeaturedMeasure` value is within the `Minimum` and `Maximum` range:
```csharp
bullet.Minimum = 0;
bullet.Maximum = 10;
bullet.FeaturedMeasure = 5;  // Must be between 0 and 10
```