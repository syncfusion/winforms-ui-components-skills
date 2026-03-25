# Weight and Color Paths Configuration

Configuring the WeightValuePath and ColorValuePath properties to control rectangle sizing and coloring in TreeMaps.

## WeightValuePath

The `WeightValuePath` property is a path to a field on the source object that serves as the "weight" of the object. This weight determines the size of the rectangle representing that data item.

### Purpose

- **Rectangle Sizing:** Controls how large each rectangle appears
- **Proportional Representation:** Larger weight values = larger rectangles
- **Data Magnitude:** Visually represents quantitative data through size

### Configuration

Set the WeightValuePath to the name of a numeric property in your data model:

```csharp
TreeMap treeMap1 = new TreeMap();
treeMap1.ItemsSource = data.PopulationDetails;
treeMap1.WeightValuePath = "Population";
```

### Example with Data Model

```csharp
public class PopulationDetail
{
    public string Country { get; set; }
    public double Population { get; set; }  // This property used for WeightValuePath
    public double Growth { get; set; }
}

// Configuration
treeMap1.WeightValuePath = "Population";  // Property name as string
```

**Result:** Countries with larger populations will have larger rectangles in the TreeMap.

### Weight Value Requirements

**Data Type:**
- Must be a numeric type (int, double, decimal, long, float)
- Cannot be null for items to be displayed
- Should be positive values (negative values may cause layout issues)

**Property Availability:**
- The specified field must be available in every object in the data collection
- Property name is case-sensitive
- Must be a public property

### Example: Sales Data

```csharp
public class SalesData
{
    public string Product { get; set; }
    public decimal Revenue { get; set; }     // Use this for weight
    public int Units { get; set; }           // Or use this for weight
    public double MarginPercent { get; set; }
}

// Option 1: Size by revenue
treeMap1.WeightValuePath = "Revenue";

// Option 2: Size by units sold
treeMap1.WeightValuePath = "Units";
```

### Multiple Levels and Weight

When using multiple hierarchical levels, the weight calculation works as follows:

```csharp
// Data with hierarchy
public class RegionalSales
{
    public string Region { get; set; }
    public string Country { get; set; }
    public double Sales { get; set; }
}

// Configuration
treeMap1.WeightValuePath = "Sales";

TreeMapFlatLevel level1 = new TreeMapFlatLevel();
level1.GroupPath = "Region";
treeMap1.Levels.Add(level1);

TreeMapFlatLevel level2 = new TreeMapFlatLevel();
level2.GroupPath = "Country";
treeMap1.Levels.Add(level2);
```

**Behavior:**
- Each country's rectangle size = its Sales value
- Each region's rectangle size = sum of all countries' Sales in that region
- Parent rectangles automatically sized to fit all children

## ColorValuePath

The `ColorValuePath` property is a path to a field on the source object that serves as the "color" of the object. This value is used by color mapping strategies to determine rectangle colors.

### Purpose

- **Color Assignment:** Determines what value drives color selection
- **Visual Encoding:** Use color to represent a second dimension of data
- **Category Distinction:** Differentiate items by color

### Configuration

Set the ColorValuePath to the name of a property in your data model:

```csharp
TreeMap treeMap1 = new TreeMap();
treeMap1.ItemsSource = data.PopulationDetails;
treeMap1.ColorValuePath = "Growth";
```

### Example with Data Model

```csharp
public class PopulationDetail
{
    public string Country { get; set; }
    public double Population { get; set; }
    public double Growth { get; set; }  // This property used for ColorValuePath
}

// Configuration
treeMap1.ColorValuePath = "Growth";  // Property name as string
```

**Result:** Color mapping strategy will use the Growth value to determine each rectangle's color.

### Color Value Requirements

**Data Type:**
- Can be numeric (int, double, decimal) for range-based coloring
- Can be string for categorical coloring
- Should match the color mapping strategy being used

**Property Availability:**
- The specified field must be available in every object
- Property name is case-sensitive
- Must be a public property

### Usage with Color Mapping

The ColorValuePath value is used by the `LeafColorMapping` property:

```csharp
// Set color value path
treeMap1.ColorValuePath = "Growth";

// Create range-based color mapping
RangeBrushColorMapping rangeMapping = new RangeBrushColorMapping();
rangeMapping.Brushes.Add(new RangeBrush 
{ 
    Color = Color.Red, 
    From = 0, 
    To = 1, 
    LegendLabel = "0-1% Growth" 
});
rangeMapping.Brushes.Add(new RangeBrush 
{ 
    Color = Color.Yellow, 
    From = 1, 
    To = 3, 
    LegendLabel = "1-3% Growth" 
});
rangeMapping.Brushes.Add(new RangeBrush 
{ 
    Color = Color.Green, 
    From = 3, 
    To = 10, 
    LegendLabel = "3-10% Growth" 
});

treeMap1.LeafColorMapping = rangeMapping;
```

**Result:** Rectangles colored based on Growth value ranges.

## Combining Weight and Color Paths

Use both paths together to create informative visualizations:

```csharp
public class StockData
{
    public string Sector { get; set; }
    public string Symbol { get; set; }
    public decimal MarketCap { get; set; }      // For WeightValuePath
    public double PerformancePercent { get; set; } // For ColorValuePath
}

// Configure TreeMap
treeMap1.ItemsSource = stockData;
treeMap1.WeightValuePath = "MarketCap";        // Size by market cap
treeMap1.ColorValuePath = "PerformancePercent"; // Color by performance

// Result: Large rectangles = large market cap
//         Green = positive performance
//         Red = negative performance
```

## Path Specification Best Practices

### Property Naming

**Use clear, descriptive property names:**

```csharp
// Good
public double Revenue { get; set; }
treeMap1.WeightValuePath = "Revenue";

// Avoid
public double Val1 { get; set; }
treeMap1.WeightValuePath = "Val1";  // Not clear what this represents
```

### Case Sensitivity

Property names must match exactly (case-sensitive):

```csharp
public class DataModel
{
    public double Population { get; set; }  // Note: Capital 'P'
}

// Correct
treeMap1.WeightValuePath = "Population";  // Matches exactly

// Incorrect - will not work
treeMap1.WeightValuePath = "population";  // Lowercase won't match
```

### Nested Properties

For flat data collections, use direct property names. Nested paths are not supported:

```csharp
// Supported
public class DataItem
{
    public double Value { get; set; }
}
treeMap1.WeightValuePath = "Value";  // Works

// Not supported
public class DataItem
{
    public Details DetailInfo { get; set; }
}
treeMap1.WeightValuePath = "DetailInfo.Value";  // Won't work - flatten data instead
```

## Common Issues and Solutions

### Issue: Rectangles Not Sizing Correctly

**Symptom:** All rectangles appear same size or layout is incorrect

**Causes and Solutions:**
1. **Wrong property name:** Verify WeightValuePath matches property exactly (case-sensitive)
2. **Non-numeric property:** Ensure property is numeric type (int, double, decimal)
3. **Null values:** Check that all items have non-null weight values
4. **Zero values:** Verify weight values are greater than zero

```csharp
// Debug: Check your data
foreach (var item in data.PopulationDetails)
{
    Debug.WriteLine($"{item.Country}: {item.Population}");
}
```

### Issue: Colors Not Applying

**Symptom:** All rectangles are the same color

**Causes and Solutions:**
1. **ColorValuePath not set:** Ensure property is specified
2. **LeafColorMapping not configured:** Color mapping must be set
3. **Wrong property name:** Verify ColorValuePath matches property exactly

```csharp
// Ensure both are set
treeMap1.ColorValuePath = "Growth";
treeMap1.LeafColorMapping = colorMapping;  // Don't forget this!
```

### Issue: Property Not Found Exception

**Symptom:** Runtime error about missing property

**Solution:** Verify property exists in every data object and is public:

```csharp
// Correct - public property
public class DataModel
{
    public double Weight { get; set; }  // Accessible
}

// Incorrect - private field
public class DataModel
{
    private double weight;  // Not accessible via path
}
```

## Complete Working Example

```csharp
using System.Collections.ObjectModel;
using Syncfusion.Windows.Forms.TreeMap;

public class PortfolioItem
{
    public string AssetClass { get; set; }
    public string Symbol { get; set; }
    public decimal InvestmentAmount { get; set; }  // For WeightValuePath
    public double ReturnPercent { get; set; }      // For ColorValuePath
}

public class PortfolioViewModel
{
    public ObservableCollection<PortfolioItem> Portfolio { get; set; }
    
    public PortfolioViewModel()
    {
        Portfolio = new ObservableCollection<PortfolioItem>
        {
            new PortfolioItem 
            { 
                AssetClass = "Stocks", 
                Symbol = "AAPL", 
                InvestmentAmount = 50000, 
                ReturnPercent = 15.5 
            },
            new PortfolioItem 
            { 
                AssetClass = "Stocks", 
                Symbol = "MSFT", 
                InvestmentAmount = 30000, 
                ReturnPercent = 12.3 
            },
            new PortfolioItem 
            { 
                AssetClass = "Bonds", 
                Symbol = "GOVT", 
                InvestmentAmount = 20000, 
                ReturnPercent = 3.5 
            }
        };
    }
}

// TreeMap Configuration
TreeMap treeMap = new TreeMap();
PortfolioViewModel data = new PortfolioViewModel();

treeMap.ItemsSource = data.Portfolio;
treeMap.WeightValuePath = "InvestmentAmount";  // Size by investment
treeMap.ColorValuePath = "ReturnPercent";       // Color by return

// Add levels
TreeMapFlatLevel level1 = new TreeMapFlatLevel();
level1.GroupPath = "AssetClass";
level1.ShowLabels = true;
treeMap.Levels.Add(level1);

TreeMapFlatLevel level2 = new TreeMapFlatLevel();
level2.GroupPath = "Symbol";
level2.ShowLabels = true;
treeMap.Levels.Add(level2);
```

---

**Important Note:** The specified fields must be available in every subclass (object) defined in the data collection. Missing or inaccessible properties will cause runtime errors or unexpected behavior.