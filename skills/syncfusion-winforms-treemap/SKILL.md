---
name: syncfusion-winforms-treemap
description: Implement Syncfusion Windows Forms TreeMap control for hierarchical data visualization with nested rectangles. Use this when working with TreeMap controls, hierarchical data display, space-efficient charts, or color-coded visualizations. Covers TreeMap configuration, layout algorithms, color mapping, and customization for stock market visualizations and data categorization with rectangular layouts.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Windows Forms TreeMap

Guide for implementing the Syncfusion® Windows Forms TreeMap control for visualizing hierarchical data through nested, color-coded rectangles. TreeMaps efficiently display large datasets where the size and color of rectangles represent quantitative values.

## When to Use This Skill

Use this skill when you need to:
- **Visualize hierarchical data** with nested rectangles
- **Display large datasets** in space-efficient layouts
- **Show proportional relationships** through rectangle sizes
- **Color-code data values** with multiple mapping strategies
- **Implement TreeMap layouts** (Squarified, SliceAndDice variations)
- **Create interactive visualizations** with tooltips and legends
- **Customize TreeMap appearance** with headers, labels, and custom rendering
- **Analyze categorical data** like stock portfolios, disk usage, or sales by region

## TreeMap Overview

Tree maps are ideal for visualizing large amounts of data where space is split into rectangles sized and colored based on quantitative variables. The levels in the hierarchy are visualized as rectangles containing other rectangles.

**Key concepts:**
- **Rectangles represent data items** - Size based on weight value
- **Hierarchical nesting** - Parent rectangles contain child rectangles
- **Color coding** - Colors represent data categories or value ranges
- **Space efficiency** - Entire visualization area is utilized
- **Multiple levels** - Support for multi-level hierarchies

### Common Use Cases

TreeMaps are used to represent large or complex data sets in various applications:

1. **Stock Market Analysis** - Rectangle size represents stock weight in index, color shows gain/loss range
2. **Internet Usage Categories** - Visualize usage across retail, social networks, search, etc.
3. **News Aggregation** - Colors represent sections (business, politics), size represents story frequency
4. **Weather Analysis** - Opacity represents humidity, size represents geographic area
5. **Disk Space Usage** - Visualize file and folder sizes in storage
6. **Sales by Region/Product** - Show sales distribution across categories
7. **Portfolio Management** - Display asset allocation and performance

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly deployment and control dependencies
- NuGet package installation
- Creating TreeMap control instance
- Adding control to Windows Forms application
- Basic namespace and assembly references
- First working TreeMap example

### Overview and Visualization Concepts
📄 **Read:** [references/overview-and-use-cases.md](references/overview-and-use-cases.md)
- What TreeMaps are and how they work
- Hierarchical information visualization
- Clustered rectangles and data representation
- Real-world use case scenarios
- When to choose TreeMap over other visualizations

### Core Configuration - Weight and Color Paths
📄 **Read:** [references/weight-and-color-paths.md](references/weight-and-color-paths.md)
- WeightValuePath property for sizing rectangles
- ColorValuePath property for color mapping
- Data binding path requirements
- Field specifications in data models
- Configuration examples

### Data Binding and Levels
📄 **Read:** [references/items-source-and-levels.md](references/items-source-and-levels.md)
- ItemsSource setup with flat data collections
- Creating data view models
- TreeMapFlatLevel configuration
- GroupPath for hierarchical grouping
- GroupGap for visual separation
- Multi-level TreeMap structure
- Complete data binding examples

### Layout Modes
📄 **Read:** [references/layout-modes.md](references/layout-modes.md)
- ItemsLayoutMode property overview
- Squarified layout algorithm
- SliceAndDiceAuto layout
- SliceAndDiceHorizontal layout
- SliceAndDiceVertical layout
- Choosing the right layout for your data
- Visual comparisons

### Color Mapping Strategies
📄 **Read:** [references/color-mapping.md](references/color-mapping.md)
- Four color mapping types overview
- UniColorMapping for single color application
- RangeBrushColorMapping for value ranges
- DesaturationColorMapping for opacity-based coloring
- PaletteColorMapping for color collections
- LeafColorMapping configuration
- Complete examples for each type

### Headers and Labels Customization
📄 **Read:** [references/headers-and-labels.md](references/headers-and-labels.md)
- Label font and color customization
- LabelBrush and LabelBackgroundBrush
- Label border properties
- HeaderBrush configuration
- ShowLabels property for visibility
- HeaderHeight for level headers
- Complete styling examples

### Legend Support
📄 **Read:** [references/legend-support.md](references/legend-support.md)
- TreeMap legend for color value demonstration
- LegendPosition configuration (Left, Right, Top, Bottom)
- LegendType for icon shapes
- LegendGap for spacing
- Integration with RangeBrushColorMapping
- LegendLabel in RangeBrush items

### Tooltips and Advanced Customization
📄 **Read:** [references/tooltip-and-customization.md](references/tooltip-and-customization.md)
- Tooltip visibility and configuration
- HeaderToolTipInfo for group tooltips
- ItemToolTipInfo for leaf tooltips
- Custom tooltip content patterns
- LeafItemDrawing event for custom rendering
- Advanced leaf node customization
- Custom graphics and images in TreeMap

## Quick Start Example

### Step 1: Install NuGet Package

```powershell
Install-Package Syncfusion.TreeMap.WinForms
```

### Step 2: Create Data Model

```csharp
using System.Collections.ObjectModel;

public class PopulationDetail
{
    public string Continent { get; set; }
    public string Country { get; set; }
    public double Growth { get; set; }
    public double Population { get; set; }
}

public class PopulationViewModel
{
    public ObservableCollection<PopulationDetail> PopulationDetails { get; set; }
    
    public PopulationViewModel()
    {
        PopulationDetails = new ObservableCollection<PopulationDetail>();
        
        PopulationDetails.Add(new PopulationDetail 
        { 
            Continent = "Asia", 
            Country = "Indonesia", 
            Growth = 3, 
            Population = 237641326 
        });
        
        PopulationDetails.Add(new PopulationDetail 
        { 
            Continent = "Asia", 
            Country = "Russia", 
            Growth = 2, 
            Population = 152518015 
        });
        
        PopulationDetails.Add(new PopulationDetail 
        { 
            Continent = "North America", 
            Country = "United States", 
            Growth = 4, 
            Population = 315645000 
        });
        
        // Add more data...
    }
}
```

### Step 3: Configure TreeMap

```csharp
using Syncfusion.Windows.Forms.TreeMap;

public class MyForm : Form
{
    private TreeMap treeMap;
    
    public MyForm()
    {
        InitializeComponent();
        
        // Create TreeMap instance
        treeMap = new TreeMap();
        
        // Set data source
        PopulationViewModel data = new PopulationViewModel();
        treeMap.ItemsSource = data.PopulationDetails;
        
        // Configure paths
        treeMap.WeightValuePath = "Population";
        treeMap.ColorValuePath = "Growth";
        
        // Set layout mode
        treeMap.ItemsLayoutMode = ItemsLayoutModes.Squarified;
        
        // Add first level (Continent)
        TreeMapFlatLevel level1 = new TreeMapFlatLevel();
        level1.GroupPath = "Continent";
        level1.ShowLabels = true;
        treeMap.Levels.Add(level1);
        
        // Add second level (Country)
        TreeMapFlatLevel level2 = new TreeMapFlatLevel();
        level2.GroupPath = "Country";
        level2.ShowLabels = true;
        level2.HeaderHeight = 25;
        treeMap.Levels.Add(level2);
        
        // Add color mapping
        UniColorMapping colorMapping = new UniColorMapping();
        colorMapping.Color = Color.MediumSlateBlue;
        treeMap.LeafColorMapping = colorMapping;
        
        // Set size and add to form
        treeMap.Size = new Size(800, 600);
        treeMap.Location = new Point(10, 10);
        this.Controls.Add(treeMap);
    }
}
```

## Common Patterns

### Multi-Level Hierarchy Pattern

```csharp
// Create levels for nested grouping
TreeMapFlatLevel level1 = new TreeMapFlatLevel();
level1.GroupPath = "Category";
level1.ShowLabels = true;
treeMap.Levels.Add(level1);

TreeMapFlatLevel level2 = new TreeMapFlatLevel();
level2.GroupPath = "SubCategory";
level2.ShowLabels = true;
level2.HeaderHeight = 25;
treeMap.Levels.Add(level2);

TreeMapFlatLevel level3 = new TreeMapFlatLevel();
level3.GroupPath = "Item";
level3.ShowLabels = true;
level3.HeaderHeight = 20;
treeMap.Levels.Add(level3);
```

### Range-Based Color Mapping Pattern

```csharp
RangeBrushColorMapping rangeMapping = new RangeBrushColorMapping();

rangeMapping.Brushes.Add(new RangeBrush 
{ 
    Color = ColorTranslator.FromHtml("#77D8D8"), 
    From = 0, 
    To = 1, 
    LegendLabel = "Low" 
});

rangeMapping.Brushes.Add(new RangeBrush 
{ 
    Color = ColorTranslator.FromHtml("#FFAF51"), 
    From = 1, 
    To = 5, 
    LegendLabel = "Medium" 
});

rangeMapping.Brushes.Add(new RangeBrush 
{ 
    Color = ColorTranslator.FromHtml("#F3D240"), 
    From = 5, 
    To = 20, 
    LegendLabel = "High" 
});

treeMap.LeafColorMapping = rangeMapping;
```

### Interactive Tooltip Pattern

```csharp
// Enable tooltips
treeMap.IsToolTipVisible = true;

// Configure header tooltip
ToolTipInfo headerToolTip = new ToolTipInfo();
headerToolTip.ToolTipHeaderPattern = "<b>{Label}</b>";
headerToolTip.ToolTipContentPattern = "Growth: {Growth}%";
treeMap.HeaderToolTipInfo = headerToolTip;

// Configure item tooltip
ToolTipInfo itemToolTip = new ToolTipInfo();
itemToolTip.ToolTipHeaderPattern = "<b>{Country}</b>";
itemToolTip.ToolTipContentPattern = "Growth: {Growth}%\nPopulation: {Population}";
treeMap.ItemToolTipInfo = itemToolTip;
```

## Key Properties Reference

### Essential Properties

| Property | Type | Description |
|----------|------|-------------|
| `ItemsSource` | object | Data collection to visualize |
| `WeightValuePath` | string | Path to field determining rectangle size |
| `ColorValuePath` | string | Path to field for color mapping |
| `ItemsLayoutMode` | ItemsLayoutModes | Layout algorithm selection |
| `Levels` | TreeMapLevelCollection | Collection of hierarchical levels |
| `LeafColorMapping` | TreeMapColorMapping | Color mapping strategy |

### Customization Properties

| Property | Type | Description |
|----------|------|-------------|
| `LabelFont` | Font | Font for leaf node labels |
| `LabelBrush` | Brush | Label text color |
| `LabelBackgroundBrush` | Brush | Label background color |
| `LabelBorderBrush` | Brush | Label border color |
| `LabelBorderThickness` | int | Label border width |
| `HeaderBrush` | Brush | Header background color |

### Legend Properties

| Property | Type | Description |
|----------|------|-------------|
| `LegendPosition` | LegendPositions | Legend placement (Top/Bottom/Left/Right) |
| `LegendType` | LegendTypes | Legend icon shape |
| `LegendGap` | int | Spacing between legend items |

### Tooltip Properties

| Property | Type | Description |
|----------|------|-------------|
| `IsToolTipVisible` | bool | Enable/disable tooltips |
| `HeaderToolTipInfo` | ToolTipInfo | Tooltip configuration for headers |
| `ItemToolTipInfo` | ToolTipInfo | Tooltip configuration for leaf items |

## Decision Guide

### When to Use Each Layout Mode

**Squarified:**
- Best for balanced aspect ratios
- More readable rectangles
- General-purpose visualization
- **Use when:** Data proportions vary widely

**SliceAndDiceAuto:**
- Alternates horizontal/vertical splits by level
- Good for hierarchical clarity
- **Use when:** Showing clear level separation

**SliceAndDiceHorizontal:**
- All rectangles split horizontally
- **Use when:** Wide display area available

**SliceAndDiceVertical:**
- All rectangles split vertically
- **Use when:** Tall display area available

### Choosing Color Mapping

**UniColorMapping:**
- Single color for all items
- **Use when:** Color doesn't represent data, only grouping

**RangeBrushColorMapping:**
- Different colors for value ranges
- **Use when:** Categorizing data into discrete ranges (low/medium/high)

**DesaturationColorMapping:**
- Single base color with varying opacity
- **Use when:** Showing continuous gradient of values

**PaletteColorMapping:**
- Cycle through color collection
- **Use when:** Distinguishing categories without value meaning

## Edge Cases and Troubleshooting

### Missing Data in Visualization
- **Cause:** WeightValuePath or ColorValuePath field missing in data model
- **Solution:** Ensure all data objects have required properties

### Labels Not Showing
- **Cause:** ShowLabels property not set on TreeMapFlatLevel
- **Solution:** Set `level.ShowLabels = true` for each level

### Colors Not Applying
- **Cause:** LeafColorMapping not configured
- **Solution:** Create and assign color mapping to `TreeMap.LeafColorMapping`

### Assembly Reference Error
- **Cause:** Missing Syncfusion.TreeMap.Windows assembly
- **Solution:** Install `Syncfusion.TreeMap.WinForms` NuGet package

### Levels Not Grouping Correctly
- **Cause:** GroupPath doesn't match data property name
- **Solution:** Verify GroupPath string matches exact property name (case-sensitive)

## Best Practices

1. **Use ObservableCollection** for dynamic data that may change
2. **Set appropriate HeaderHeight** for multi-level TreeMaps (20-30 pixels)
3. **Choose color mapping** based on data meaning (ranges vs. gradient)
4. **Enable tooltips** for interactive exploration of data
5. **Use GroupGap** to visually separate levels (5-10 pixels)
6. **Test with realistic data sizes** to ensure layout works well
7. **Provide legends** when using RangeBrushColorMapping
8. **Consider layout mode** based on display dimensions

---

**Assembly:** `Syncfusion.TreeMap.Windows`  
**Namespace:** `Syncfusion.Windows.Forms.TreeMap`  
**NuGet Package:** `Syncfusion.TreeMap.WinForms`