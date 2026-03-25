# Getting Started with Windows Forms TreeMap

This guide covers the initial setup and basic implementation of the Syncfusion Windows Forms TreeMap control.

## Assembly Deployment

Before using the TreeMap control, you need to add the required assembly references to your project.

### Required Assemblies

The TreeMap control requires the following assembly:
- **Syncfusion.TreeMap.Windows.dll**

### Control Dependencies

Refer to the [control dependencies](https://help.syncfusion.com/windowsforms/control-dependencies#treemap) section to get the complete list of assemblies or NuGet packages needed to use the TreeMap control in your application.

## NuGet Package Installation

The recommended way to add the TreeMap control is through NuGet Package Manager.

### Installation Steps

1. Open your Windows Forms project in Visual Studio
2. Go to **Tools → NuGet Package Manager → Manage NuGet Packages for Solution**
3. Search for **"Syncfusion.TreeMap.WinForms"**
4. Select the package and click **Install**

### Using Package Manager Console

Alternatively, you can use the Package Manager Console:

```powershell
Install-Package Syncfusion.TreeMap.WinForms
```

**More details:** [How to install NuGet packages](https://help.syncfusion.com/windowsforms/installation/install-nuget-packages)

## Creating TreeMap Control

### Assembly and Namespace

The TreeMap control is available in the following assembly and namespace:

**Assembly:** `Syncfusion.TreeMap.Windows`  
**Namespace:** `Syncfusion.Windows.Forms.TreeMap`

### Adding Using Statement

Add the namespace reference at the top of your form class:

```csharp
using Syncfusion.Windows.Forms.TreeMap;
```

### Creating TreeMap Instance

Create a TreeMap control instance in your form:

```csharp
TreeMap treeMap1 = new TreeMap();
```

### Adding to Form

Add the TreeMap control to your form's Controls collection:

```csharp
public partial class Form1 : Form
{
    private TreeMap treeMap1;
    
    public Form1()
    {
        InitializeComponent();
        
        // Create TreeMap instance
        treeMap1 = new TreeMap();
        
        // Set size and location
        treeMap1.Size = new Size(800, 600);
        treeMap1.Location = new Point(10, 10);
        
        // Add to form
        this.Controls.Add(treeMap1);
    }
}
```

## Basic TreeMap Implementation

### Step 1: Define Data Model

Create a data model class with properties for your hierarchical data:

```csharp
public class PopulationDetail
{
    public string Continent { get; set; }
    public string Country { get; set; }
    public double Growth { get; set; }
    public double Population { get; set; }
    public string StrPopulation { get; set; }
}
```

### Step 2: Create View Model

Create a view model to hold your data collection:

```csharp
using System.Collections.ObjectModel;

public class PopulationViewModel
{
    public ObservableCollection<PopulationDetail> PopulationDetails { get; set; }
    
    public PopulationViewModel()
    {
        this.PopulationDetails = new ObservableCollection<PopulationDetail>();
        
        // Add sample data
        PopulationDetails.Add(new PopulationDetail 
        { 
            Continent = "Asia", 
            Country = "Indonesia", 
            Growth = 3, 
            Population = 237641326, 
            StrPopulation = "237.6 M" 
        });
        
        PopulationDetails.Add(new PopulationDetail 
        { 
            Continent = "Asia", 
            Country = "Russia", 
            Growth = 2, 
            Population = 152518015, 
            StrPopulation = "152.5 M" 
        });
        
        PopulationDetails.Add(new PopulationDetail 
        { 
            Continent = "North America", 
            Country = "United States", 
            Growth = 4, 
            Population = 315645000, 
            StrPopulation = "315.6 M" 
        });
        
        PopulationDetails.Add(new PopulationDetail 
        { 
            Continent = "Europe", 
            Country = "Germany", 
            Growth = 1, 
            Population = 81993000, 
            StrPopulation = "82 M" 
        });
    }
}
```

### Step 3: Configure TreeMap Control

Set up the TreeMap with basic configuration:

```csharp
public partial class Form1 : Form
{
    private TreeMap treeMap1;
    
    public Form1()
    {
        InitializeComponent();
        
        // Create TreeMap instance
        treeMap1 = new TreeMap();
        
        // Create data source
        PopulationViewModel data = new PopulationViewModel();
        
        // Set data source
        treeMap1.ItemsSource = data.PopulationDetails;
        
        // Configure weight and color paths
        treeMap1.WeightValuePath = "Population";
        treeMap1.ColorValuePath = "Growth";
        
        // Set layout mode
        treeMap1.ItemsLayoutMode = ItemsLayoutModes.Squarified;
        
        // Create first level (Continent)
        TreeMapFlatLevel level1 = new TreeMapFlatLevel();
        level1.GroupPath = "Continent";
        level1.ShowLabels = true;
        treeMap1.Levels.Add(level1);
        
        // Create second level (Country)
        TreeMapFlatLevel level2 = new TreeMapFlatLevel();
        level2.GroupPath = "Country";
        level2.ShowLabels = true;
        level2.HeaderHeight = 25;
        treeMap1.Levels.Add(level2);
        
        // Set size and location
        treeMap1.Size = new Size(800, 600);
        treeMap1.Location = new Point(10, 10);
        
        // Add to form
        this.Controls.Add(treeMap1);
    }
}
```

## First Render and Initialization

When you run the application, the TreeMap will:

1. **Load the data** from ItemsSource
2. **Calculate rectangle sizes** based on WeightValuePath (Population)
3. **Apply colors** based on ColorValuePath (Growth)
4. **Group data** by levels (Continent, then Country)
5. **Render nested rectangles** using the Squarified layout algorithm
6. **Display labels** on each rectangle showing the grouped values

## Minimal Working Example

Here's a complete minimal example to get started:

```csharp
using System;
using System.Collections.ObjectModel;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.TreeMap;

namespace TreeMapDemo
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
            
            // Create TreeMap
            TreeMap treeMap = new TreeMap();
            
            // Create simple data
            var data = new ObservableCollection<DataItem>
            {
                new DataItem { Category = "A", Value = 100 },
                new DataItem { Category = "B", Value = 200 },
                new DataItem { Category = "C", Value = 150 }
            };
            
            // Configure TreeMap
            treeMap.ItemsSource = data;
            treeMap.WeightValuePath = "Value";
            treeMap.ColorValuePath = "Value";
            treeMap.ItemsLayoutMode = ItemsLayoutModes.Squarified;
            
            // Add level
            TreeMapFlatLevel level = new TreeMapFlatLevel();
            level.GroupPath = "Category";
            level.ShowLabels = true;
            treeMap.Levels.Add(level);
            
            // Set size and add to form
            treeMap.Dock = DockStyle.Fill;
            this.Controls.Add(treeMap);
        }
    }
    
    public class DataItem
    {
        public string Category { get; set; }
        public double Value { get; set; }
    }
}
```

## Key Initialization Properties

When setting up a TreeMap, these properties are essential:

| Property | Required | Purpose |
|----------|----------|---------|
| `ItemsSource` | Yes | Data collection to visualize |
| `WeightValuePath` | Yes | Property name for rectangle sizing |
| `Levels` | Yes | At least one level for grouping |
| `ItemsLayoutMode` | Recommended | Layout algorithm (default: Squarified) |
| `ColorValuePath` | Optional | Property name for color mapping |

## Common Setup Issues

### TreeMap Not Displaying
**Problem:** TreeMap control is blank or not visible  
**Solutions:**
- Ensure `ItemsSource` is set with valid data
- Verify `WeightValuePath` matches a property in your data model
- Check that at least one `TreeMapFlatLevel` is added to `Levels` collection
- Confirm control `Size` is set appropriately

### Labels Not Showing
**Problem:** Rectangle labels are not visible  
**Solution:** Set `ShowLabels = true` on each `TreeMapFlatLevel`

### Assembly Not Found
**Problem:** Compiler error about missing Syncfusion.TreeMap namespace  
**Solution:** Install `Syncfusion.TreeMap.WinForms` NuGet package

### Data Not Grouping
**Problem:** All items appear at same level  
**Solution:** Verify `GroupPath` property matches exact property name in data model (case-sensitive)

## Next Steps

After basic setup:
- Configure **layout modes** for different rectangle arrangements
- Add **color mapping** to visualize data values with colors
- Customize **headers and labels** for better readability
- Implement **tooltips** for interactive data exploration
- Add **legends** to explain color meanings

---

**Quick Reference:**  
- Assembly: `Syncfusion.TreeMap.Windows`
- Namespace: `Syncfusion.Windows.Forms.TreeMap`
- NuGet: `Syncfusion.TreeMap.WinForms`
- Dependencies: See [control dependencies](https://help.syncfusion.com/windowsforms/control-dependencies#treemap)