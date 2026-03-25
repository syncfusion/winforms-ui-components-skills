# Data Binding and Tree Map Levels

## Table of Contents
- [ItemsSource Overview](#itemssource-overview)
- [Creating Data Models](#creating-data-models)
- [TreeMapFlatLevel Configuration](#treemapflatlevel-configuration)
- [GroupPath for Hierarchical Grouping](#grouppath-for-hierarchical-grouping)
- [GroupGap for Visual Separation](#groupgap-for-visual-separation)
- [Multi-Level TreeMap Examples](#multi-level-treemap-examples)
- [Common Patterns](#common-patterns)

## ItemsSource Overview

The levels of the tree map control can be categorized into flat and hierarchical types, which are used to define the levels of a data collection. The `ItemsSource` property is the foundation for data binding in TreeMap.

### Flat Collection Requirement

The ItemsSource set to TreeMap control must be a **flat collection** of data. This means:
- Single-level list or collection (not nested objects)
- Each item contains all properties needed for grouping
- Hierarchy is created through grouping, not nested collections

**Supported collection types:**
- `ObservableCollection<T>`
- `List<T>`
- `BindingList<T>`
- Any `IEnumerable<T>` implementation

### Setting ItemsSource

```csharp
TreeMap treeMap1 = new TreeMap();
PopulationViewModel data = new PopulationViewModel();

// Set the data source
treeMap1.ItemsSource = data.PopulationDetails;

// Configure paths
treeMap1.WeightValuePath = "Population";
treeMap1.ColorValuePath = "Growth";
```

## Creating Data Models

### Basic Data Model Structure

Create a data model class with properties for your data:

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

**Property purposes:**
- `Continent`, `Country`: Used for GroupPath to create hierarchy
- `Population`: Used for WeightValuePath (rectangle sizing)
- `Growth`: Used for ColorValuePath (color mapping)
- `StrPopulation`: Display-friendly format for labels/tooltips

### Complete View Model Example

```csharp
using System.Collections.ObjectModel;

public class PopulationViewModel
{
    public ObservableCollection<PopulationDetail> PopulationDetails { get; set; }
    
    public PopulationViewModel()
    {
        this.PopulationDetails = new ObservableCollection<PopulationDetail>();
        
        // Asia continent data
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
            Continent = "Asia", 
            Country = "Malaysia", 
            Growth = 1, 
            Population = 29672000, 
            StrPopulation = "29.7 M" 
        });
        
        // North America continent data
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
            Continent = "North America", 
            Country = "Mexico", 
            Growth = 2, 
            Population = 112336538, 
            StrPopulation = "112.3 M" 
        });
        
        PopulationDetails.Add(new PopulationDetail 
        { 
            Continent = "North America", 
            Country = "Canada", 
            Growth = 1, 
            Population = 35056064, 
            StrPopulation = "35.1 M" 
        });
        
        // South America continent data
        PopulationDetails.Add(new PopulationDetail 
        { 
            Continent = "South America", 
            Country = "Colombia", 
            Growth = 1, 
            Population = 47000000, 
            StrPopulation = "47 M" 
        });
        
        PopulationDetails.Add(new PopulationDetail 
        { 
            Continent = "South America", 
            Country = "Brazil", 
            Growth = 3, 
            Population = 193946886, 
            StrPopulation = "193.9 M" 
        });
        
        // Africa continent data
        PopulationDetails.Add(new PopulationDetail 
        { 
            Continent = "Africa", 
            Country = "Nigeria", 
            Growth = 2, 
            Population = 170901000, 
            StrPopulation = "170.9 M" 
        });
        
        PopulationDetails.Add(new PopulationDetail 
        { 
            Continent = "Africa", 
            Country = "Egypt", 
            Growth = 1, 
            Population = 83661000, 
            StrPopulation = "83 M" 
        });
        
        // Europe continent data
        PopulationDetails.Add(new PopulationDetail 
        { 
            Continent = "Europe", 
            Country = "Germany", 
            Growth = 1, 
            Population = 81993000, 
            StrPopulation = "82 M" 
        });
        
        PopulationDetails.Add(new PopulationDetail 
        { 
            Continent = "Europe", 
            Country = "France", 
            Growth = 1, 
            Population = 65605000, 
            StrPopulation = "65.6 M" 
        });
        
        PopulationDetails.Add(new PopulationDetail 
        { 
            Continent = "Europe", 
            Country = "UK", 
            Growth = 1, 
            Population = 63181775, 
            StrPopulation = "63.2 M" 
        });
    }
}
```

## TreeMapFlatLevel Configuration

The `TreeMapFlatLevel` class defines each level in the hierarchical visualization. Add levels to the TreeMap's `Levels` collection.

### Creating a Level

```csharp
TreeMapFlatLevel level = new TreeMapFlatLevel();
level.GroupPath = "PropertyName";
level.ShowLabels = true;
level.HeaderHeight = 25;
level.GroupGap = 5;

treeMap1.Levels.Add(level);
```

### Level Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `GroupPath` | string | null | Property name for grouping data |
| `ShowLabels` | bool | false | Display labels on rectangles |
| `HeaderHeight` | int | 0 | Height of header area in pixels |
| `GroupGap` | int | 0 | Spacing between groups in pixels |

## GroupPath for Hierarchical Grouping

The `GroupPath` must be specified for every level of the tree map control. It is a path to a field on the source object that serves as the "Group" for the level specified.

### How GroupPath Works

Data is grouped in the tree map control based on the GroupPath. The TreeMap:
1. Reads the specified property from each data item
2. Groups items with the same property value
3. Creates a parent rectangle for each group
4. Nests child rectangles within parent

### Single Level Example

```csharp
TreeMap treeMap1 = new TreeMap();
PopulationViewModel data = new PopulationViewModel();

treeMap1.ItemsSource = data.PopulationDetails;
treeMap1.WeightValuePath = "Population";
treeMap1.ColorValuePath = "Growth";

// Single level - group by Continent only
TreeMapFlatLevel level1 = new TreeMapFlatLevel();
level1.GroupPath = "Continent";
level1.ShowLabels = true;
treeMap1.Levels.Add(level1);
```

**Result:** One rectangle per continent, sized by total population

### Two-Level Example

```csharp
TreeMap treeMap1 = new TreeMap();
PopulationViewModel data = new PopulationViewModel();

treeMap1.ItemsSource = data.PopulationDetails;
treeMap1.WeightValuePath = "Population";
treeMap1.ColorValuePath = "Growth";

// First level - group by Continent
TreeMapFlatLevel level1 = new TreeMapFlatLevel();
level1.GroupPath = "Continent";
level1.ShowLabels = true;
treeMap1.Levels.Add(level1);

// Second level - group by Country within each Continent
TreeMapFlatLevel level2 = new TreeMapFlatLevel();
level2.GroupPath = "Country";
level2.ShowLabels = true;
level2.HeaderHeight = 25;
treeMap1.Levels.Add(level2);
```

**Result:** 
- Top level: Continent rectangles (Asia, North America, Europe, etc.)
- Nested level: Country rectangles within each continent
- Headers displayed for country names

### GroupPath Requirements

**Must match property name exactly:**
```csharp
// Data model
public class DataItem
{
    public string Category { get; set; }  // Note exact spelling and case
}

// Correct
level.GroupPath = "Category";  // Matches exactly

// Incorrect - will not work
level.GroupPath = "category";   // Wrong case
level.GroupPath = "Categories"; // Wrong property
```

**If GroupPath is not specified:**
- Items are not grouped
- Data shown in order as specified in ItemsSource
- No hierarchical structure created

## GroupGap for Visual Separation

The `GroupGap` property can be specified to separate the items of every level. It is used to differentiate the levels in the tree map control visually.

### Setting GroupGap

```csharp
TreeMapFlatLevel level1 = new TreeMapFlatLevel();
level1.GroupPath = "Continent";
level1.ShowLabels = true;
level1.GroupGap = 5;  // 5 pixels between continent groups
treeMap1.Levels.Add(level1);

TreeMapFlatLevel level2 = new TreeMapFlatLevel();
level2.GroupPath = "Country";
level2.ShowLabels = true;
level2.HeaderHeight = 25;
level2.GroupGap = 3;  // 3 pixels between country rectangles
treeMap1.Levels.Add(level2);
```

### GroupGap Values

| Value | Effect | Use Case |
|-------|--------|----------|
| 0 | No gap (default) | Maximum space utilization |
| 2-5 | Subtle separation | Modern, clean look |
| 5-10 | Clear separation | Better level distinction |
| 10+ | Obvious gaps | High contrast needs |

**Recommendation:** Start with 5 pixels and adjust based on visual preference.

## Multi-Level TreeMap Examples

### Three-Level Hierarchy

```csharp
public class SalesData
{
    public string Region { get; set; }      // Level 1
    public string Country { get; set; }     // Level 2
    public string Product { get; set; }     // Level 3
    public decimal Sales { get; set; }
    public double Margin { get; set; }
}

// Configuration
treeMap1.ItemsSource = salesData;
treeMap1.WeightValuePath = "Sales";
treeMap1.ColorValuePath = "Margin";

// Level 1: Region
TreeMapFlatLevel level1 = new TreeMapFlatLevel();
level1.GroupPath = "Region";
level1.ShowLabels = true;
level1.GroupGap = 8;
treeMap1.Levels.Add(level1);

// Level 2: Country
TreeMapFlatLevel level2 = new TreeMapFlatLevel();
level2.GroupPath = "Country";
level2.ShowLabels = true;
level2.HeaderHeight = 30;
level2.GroupGap = 5;
treeMap1.Levels.Add(level2);

// Level 3: Product
TreeMapFlatLevel level3 = new TreeMapFlatLevel();
level3.GroupPath = "Product";
level3.ShowLabels = true;
level3.HeaderHeight = 20;
level3.GroupGap = 2;
treeMap1.Levels.Add(level3);
```

**Visual structure:**
```
[Americas Region]
  [USA]
    [Product A] [Product B] [Product C]
  [Canada]
    [Product A] [Product B]
    
[Europe Region]
  [Germany]
    [Product A] [Product B]
  [France]
    [Product A] [Product C]
```

### Department-Project-Task Hierarchy

```csharp
public class WorkItem
{
    public string Department { get; set; }
    public string Project { get; set; }
    public string Task { get; set; }
    public int Hours { get; set; }
    public int Priority { get; set; }
}

treeMap1.ItemsSource = workItems;
treeMap1.WeightValuePath = "Hours";
treeMap1.ColorValuePath = "Priority";

TreeMapFlatLevel deptLevel = new TreeMapFlatLevel();
deptLevel.GroupPath = "Department";
deptLevel.ShowLabels = true;
treeMap1.Levels.Add(deptLevel);

TreeMapFlatLevel projectLevel = new TreeMapFlatLevel();
projectLevel.GroupPath = "Project";
projectLevel.ShowLabels = true;
projectLevel.HeaderHeight = 25;
treeMap1.Levels.Add(projectLevel);

TreeMapFlatLevel taskLevel = new TreeMapFlatLevel();
taskLevel.GroupPath = "Task";
taskLevel.ShowLabels = true;
taskLevel.HeaderHeight = 20;
treeMap1.Levels.Add(taskLevel);
```

## Common Patterns

### Simple Categorization Pattern

```csharp
// Single level grouping
TreeMapFlatLevel level = new TreeMapFlatLevel();
level.GroupPath = "Category";
level.ShowLabels = true;
treeMap.Levels.Add(level);
```

**Use when:** Simple category visualization without sub-categories

### Parent-Child Pattern

```csharp
// Two levels: parent and child
TreeMapFlatLevel parentLevel = new TreeMapFlatLevel();
parentLevel.GroupPath = "ParentCategory";
parentLevel.ShowLabels = true;
treeMap.Levels.Add(parentLevel);

TreeMapFlatLevel childLevel = new TreeMapFlatLevel();
childLevel.GroupPath = "ChildCategory";
childLevel.ShowLabels = true;
childLevel.HeaderHeight = 25;
treeMap.Levels.Add(childLevel);
```

**Use when:** Two-tier hierarchy (e.g., Department → Employee)

### Deep Hierarchy Pattern

```csharp
// Multiple levels with decreasing header heights
int[] headerHeights = { 0, 30, 25, 20 };  // Top level no header

for (int i = 0; i < groupPaths.Length; i++)
{
    TreeMapFlatLevel level = new TreeMapFlatLevel();
    level.GroupPath = groupPaths[i];
    level.ShowLabels = true;
    level.HeaderHeight = headerHeights[i];
    level.GroupGap = 5 - i;  // Decreasing gaps
    treeMap.Levels.Add(level);
}
```

**Use when:** 3+ levels with consistent configuration

## Common Issues

### Issue: All Items at Same Level

**Symptom:** No hierarchical grouping visible

**Solution:** Verify GroupPath is set for each level:
```csharp
level.GroupPath = "PropertyName";  // Must be set!
```

### Issue: Empty Rectangles

**Symptom:** Rectangles appear but no data shown

**Solutions:**
1. Set `ShowLabels = true` to display labels
2. Verify WeightValuePath is set correctly
3. Check data has non-zero weight values

### Issue: Wrong Grouping

**Symptom:** Items grouped unexpectedly

**Solution:** Verify GroupPath matches property name exactly (case-sensitive):
```csharp
// If property is "Country" (capital C)
level.GroupPath = "Country";  // Correct
level.GroupPath = "country";  // Won't work
```

### Issue: Levels Not Nesting

**Symptom:** All levels appear flat

**Solution:** Ensure levels added in correct order (parent to child):
```csharp
// Correct order
treeMap.Levels.Add(continentLevel);  // Parent first
treeMap.Levels.Add(countryLevel);     // Child second

// Incorrect order will cause issues
```

---

**Key Takeaway:** TreeMap uses flat data collections with GroupPath properties to create hierarchical visualizations. Each level groups data by a specific property, creating nested rectangles automatically.