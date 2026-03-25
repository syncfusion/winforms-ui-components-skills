# Color Mapping in TreeMap

## Table of Contents
- [Color Mapping Overview](#color-mapping-overview)
- [UniColorMapping](#unicolormapping)
- [RangeBrushColorMapping](#rangebrushcolormapping)
- [DesaturationColorMapping](#desaturationcolormapping)
- [PaletteColorMapping](#palettecolormapping)
- [Choosing Color Mapping Strategy](#choosing-color-mapping-strategy)
- [Color Mapping Best Practices](#color-mapping-best-practices)

## Color Mapping Overview

Color mapping determines how leaf node rectangles are colored in the TreeMap. The `ColorValuePath` property specifies which data field to use, and the `LeafColorMapping` property defines the color strategy.

### Color Mapping Types

TreeMap supports four different color mapping types:

| Type | Purpose | Best For |
|------|---------|----------|
| **UniColorMapping** | Single color for all items | Simple categorization, no color meaning |
| **RangeBrushColorMapping** | Different colors for value ranges | Discrete categories (low/medium/high) |
| **DesaturationColorMapping** | Single color with varying opacity | Continuous gradients, heat maps |
| **PaletteColorMapping** | Cycle through color collection | Distinguishing many categories |

### Basic Setup

All color mapping strategies follow this pattern:

```csharp
// 1. Set ColorValuePath to data property
treeMap1.ColorValuePath = "Growth";

// 2. Create color mapping instance
var colorMapping = new ColorMappingType();

// 3. Configure color mapping
// (type-specific configuration)

// 4. Assign to TreeMap
treeMap1.LeafColorMapping = colorMapping;
```

## UniColorMapping

UniColorMapping applies a single color to all leaf nodes in the TreeMap. Use when color doesn't represent data meaning.

### Configuration

```csharp
TreeMap treeMap1 = new TreeMap();
PopulationViewModel data = new PopulationViewModel();

treeMap1.ItemsSource = data.PopulationDetails;
treeMap1.WeightValuePath = "Population";
treeMap1.ColorValuePath = "Growth";
treeMap1.ItemsLayoutMode = ItemsLayoutModes.SliceAndDiceAuto;

// Configure levels
TreeMapFlatLevel level1 = new TreeMapFlatLevel();
level1.GroupPath = "Continent";
level1.ShowLabels = true;
treeMap1.Levels.Add(level1);

TreeMapFlatLevel level2 = new TreeMapFlatLevel();
level2.GroupPath = "Country";
level2.ShowLabels = true;
level2.HeaderHeight = 25;
treeMap1.Levels.Add(level2);

// Create UniColorMapping
UniColorMapping uniColorMapping = new UniColorMapping();
uniColorMapping.Color = Color.MediumSlateBlue;

// Apply color mapping
treeMap1.LeafColorMapping = uniColorMapping;
```

### Properties

| Property | Type | Description |
|----------|------|-------------|
| `Color` | Color | The single color applied to all leaf nodes |

### When to Use UniColorMapping

**Best for:**
- Simple visual grouping without color meaning
- Minimalist designs
- When other properties (size, labels) convey all information
- Consistent branding colors

**Example scenarios:**
- All products shown in company brand color
- Geographic regions without performance indicators
- Organizational chart with uniform coloring

### Color Selection Tips

```csharp
// System colors
uniColorMapping.Color = Color.DodgerBlue;

// RGB colors
uniColorMapping.Color = Color.FromArgb(100, 150, 200);

// Named colors
uniColorMapping.Color = Color.MediumSeaGreen;

// HTML hex colors
uniColorMapping.Color = ColorTranslator.FromHtml("#4A90E2");
```

## RangeBrushColorMapping

RangeBrushColorMapping assigns colors based on value ranges. Different color for each range creates discrete categories.

### Configuration

```csharp
TreeMap treeMap1 = new TreeMap();
PopulationViewModel data = new PopulationViewModel();

treeMap1.ItemsSource = data.PopulationDetails;
treeMap1.WeightValuePath = "Population";
treeMap1.ColorValuePath = "Growth";
treeMap1.ItemsLayoutMode = ItemsLayoutModes.SliceAndDiceAuto;

// Configure levels
TreeMapFlatLevel level1 = new TreeMapFlatLevel();
level1.GroupPath = "Continent";
level1.ShowLabels = true;
treeMap1.Levels.Add(level1);

TreeMapFlatLevel level2 = new TreeMapFlatLevel();
level2.GroupPath = "Country";
level2.ShowLabels = true;
level2.HeaderHeight = 25;
treeMap1.Levels.Add(level2);

// Create RangeBrushColorMapping
RangeBrushColorMapping rangeBrushColorMapping = new RangeBrushColorMapping();

// Add range brushes
rangeBrushColorMapping.Brushes.Add(new RangeBrush
{
    Color = ColorTranslator.FromHtml("#77D8D8"),
    From = 0,
    To = 1,
    LegendLabel = "1% Growth"
});

rangeBrushColorMapping.Brushes.Add(new RangeBrush
{
    Color = ColorTranslator.FromHtml("#AED960"),
    From = 0,
    To = 2,
    LegendLabel = "2% Growth"
});

rangeBrushColorMapping.Brushes.Add(new RangeBrush
{
    Color = ColorTranslator.FromHtml("#FFAF51"),
    From = 0,
    To = 3,
    LegendLabel = "3% Growth"
});

rangeBrushColorMapping.Brushes.Add(new RangeBrush
{
    Color = ColorTranslator.FromHtml("#F3D240"),
    From = 0,
    To = 20,
    LegendLabel = "20% Growth"
});

// Apply color mapping
treeMap1.LeafColorMapping = rangeBrushColorMapping;
```

### RangeBrush Properties

| Property | Type | Description |
|----------|------|-------------|
| `Color` | Color | Color for items in this range |
| `From` | double | Minimum value (inclusive) |
| `To` | double | Maximum value (inclusive) |
| `LegendLabel` | string | Label shown in legend |

### Range Matching Logic

**How values are matched:**
- Value compared against each RangeBrush
- First matching range determines color
- If `From <= value <= To`, range matches
- Order matters: define ranges from specific to general

### Overlapping Ranges Example

```csharp
RangeBrushColorMapping rangeMapping = new RangeBrushColorMapping();

// Define ranges with overlap
rangeMapping.Brushes.Add(new RangeBrush
{
    Color = Color.LightGreen,
    From = 0,
    To = 5,
    LegendLabel = "Low (0-5)"
});

rangeMapping.Brushes.Add(new RangeBrush
{
    Color = Color.Yellow,
    From = 5,
    To = 10,
    LegendLabel = "Medium (5-10)"
});

rangeMapping.Brushes.Add(new RangeBrush
{
    Color = Color.OrangeRed,
    From = 10,
    To = 100,
    LegendLabel = "High (10+)"
});

treeMap1.LeafColorMapping = rangeMapping;
```

### When to Use RangeBrushColorMapping

**Best for:**
- Discrete categories (low/medium/high)
- Performance indicators (bad/ok/good/excellent)
- Risk levels (safe/warning/danger)
- Any classification into distinct buckets

**Example scenarios:**
- Stock performance: loss/flat/gain/strong gain
- Sales targets: below/at/above target
- Temperature ranges: cold/cool/warm/hot
- Priority levels: low/medium/high/critical

### Common Range Patterns

**Three-tier pattern (bad/ok/good):**
```csharp
// Red (negative), Yellow (neutral), Green (positive)
rangeMapping.Brushes.Add(new RangeBrush
{
    Color = Color.Red,
    From = -100,
    To = 0,
    LegendLabel = "Negative"
});

rangeMapping.Brushes.Add(new RangeBrush
{
    Color = Color.Yellow,
    From = 0,
    To = 5,
    LegendLabel = "Neutral"
});

rangeMapping.Brushes.Add(new RangeBrush
{
    Color = Color.Green,
    From = 5,
    To = 100,
    LegendLabel = "Positive"
});
```

**Percentile pattern:**
```csharp
// Bottom 25%, Middle 50%, Top 25%
rangeMapping.Brushes.Add(new RangeBrush
{
    Color = Color.LightCoral,
    From = 0,
    To = 25,
    LegendLabel = "Bottom Quartile"
});

rangeMapping.Brushes.Add(new RangeBrush
{
    Color = Color.LightYellow,
    From = 25,
    To = 75,
    LegendLabel = "Middle Range"
});

rangeMapping.Brushes.Add(new RangeBrush
{
    Color = Color.LightGreen,
    From = 75,
    To = 100,
    LegendLabel = "Top Quartile"
});
```

## DesaturationColorMapping

DesaturationColorMapping uses a single base color with varying opacity to create a gradient effect. Higher values appear more saturated, lower values more transparent.

### Configuration

```csharp
TreeMap treeMap1 = new TreeMap();
PopulationViewModel data = new PopulationViewModel();

treeMap1.ItemsSource = data.PopulationDetails;
treeMap1.WeightValuePath = "Population";
treeMap1.ColorValuePath = "Growth";
treeMap1.ItemsLayoutMode = ItemsLayoutModes.SliceAndDiceAuto;

// Configure levels
TreeMapFlatLevel level1 = new TreeMapFlatLevel();
level1.GroupPath = "Continent";
level1.ShowLabels = true;
treeMap1.Levels.Add(level1);

TreeMapFlatLevel level2 = new TreeMapFlatLevel();
level2.GroupPath = "Country";
level2.ShowLabels = true;
level2.HeaderHeight = 25;
treeMap1.Levels.Add(level2);

// Create DesaturationColorMapping
DesaturationColorMapping desaturationColorMapping = new DesaturationColorMapping();

desaturationColorMapping.Color = Color.OrangeRed;  // Base color
desaturationColorMapping.From = 220;                // Most opaque alpha
desaturationColorMapping.To = 0;                    // Most transparent alpha
desaturationColorMapping.RangeMinimum = 0;          // Minimum data value
desaturationColorMapping.RangeMaximum = 80000;      // Maximum data value

// Apply color mapping
treeMap1.LeafColorMapping = desaturationColorMapping;
```

### Properties

| Property | Type | Description |
|----------|------|-------------|
| `Color` | Color | Base color applied to all items |
| `From` | int | Starting alpha value (0-255) for RangeMinimum |
| `To` | int | Ending alpha value (0-255) for RangeMaximum |
| `RangeMinimum` | double | Minimum expected data value |
| `RangeMaximum` | double | Maximum expected data value |

### How Opacity is Calculated

For each item:
1. Get value from ColorValuePath property
2. Calculate position in range: `position = (value - RangeMinimum) / (RangeMaximum - RangeMinimum)`
3. Interpolate alpha: `alpha = From + (To - From) * position`
4. Apply alpha to base Color

**Example:**
- RangeMinimum = 0, RangeMaximum = 100
- From = 220 (almost opaque), To = 0 (transparent)
- Item value = 50 (middle of range)
- Result alpha = 220 + (0 - 220) * 0.5 = 110 (semi-transparent)

### Desaturation Direction

**Higher value = more opaque (common):**
```csharp
desaturationColorMapping.From = 220;  // High opacity
desaturationColorMapping.To = 30;     // Low opacity
// Result: Higher values appear darker/stronger
```

**Higher value = more transparent (inverse):**
```csharp
desaturationColorMapping.From = 30;   // Low opacity
desaturationColorMapping.To = 220;    // High opacity
// Result: Lower values appear darker/stronger
```

### When to Use DesaturationColorMapping

**Best for:**
- Continuous gradients (not discrete categories)
- Heat map effects
- Density visualization
- When you want subtle color variation
- Single-dimension continuous data

**Example scenarios:**
- Population density (darker = more dense)
- Temperature maps (darker = hotter)
- Usage intensity (darker = more usage)
- Probability or confidence (darker = higher confidence)

### Color Selection for Desaturation

```csharp
// Heat map red (common)
desaturationColorMapping.Color = Color.OrangeRed;

// Cool blue gradient
desaturationColorMapping.Color = Color.DodgerBlue;

// Green intensity
desaturationColorMapping.Color = Color.ForestGreen;

// Purple density
desaturationColorMapping.Color = Color.MediumPurple;
```

## PaletteColorMapping

PaletteColorMapping cycles through a collection of colors, assigning each leaf node the next color in the list. Use to distinguish many categories visually.

### Configuration

```csharp
TreeMap treeMap1 = new TreeMap();
PopulationViewModel data = new PopulationViewModel();

treeMap1.ItemsSource = data.PopulationDetails;
treeMap1.WeightValuePath = "Population";
treeMap1.ColorValuePath = "Growth";
treeMap1.ItemsLayoutMode = ItemsLayoutModes.SliceAndDiceAuto;

// Configure levels
TreeMapFlatLevel level1 = new TreeMapFlatLevel();
level1.GroupPath = "Continent";
level1.ShowLabels = true;
treeMap1.Levels.Add(level1);

TreeMapFlatLevel level2 = new TreeMapFlatLevel();
level2.GroupPath = "Country";
level2.ShowLabels = true;
level2.HeaderHeight = 25;
treeMap1.Levels.Add(level2);

// Create PaletteColorMapping
PaletteColorMapping paletteColorMapping = new PaletteColorMapping();

paletteColorMapping.Colors = new List<Brush>()
{
    new SolidBrush(Color.MediumSeaGreen),
    new SolidBrush(Color.PaleVioletRed),
    new SolidBrush(Color.MediumSlateBlue)
};

// Apply color mapping
treeMap1.LeafColorMapping = paletteColorMapping;
```

### Properties

| Property | Type | Description |
|----------|------|-------------|
| `Colors` | List<Brush> | Collection of brushes to cycle through |

### Color Assignment Logic

- Colors assigned sequentially to leaf nodes
- When end of list reached, cycles back to start
- Order determined by data order in ItemsSource
- Grouped items get consecutive colors from palette

### When to Use PaletteColorMapping

**Best for:**
- Distinguishing many categories visually
- When ColorValuePath doesn't have meaningful numeric value
- Categorical data without inherent order
- Visual variety without semantic meaning

**Example scenarios:**
- Different products (colors arbitrary)
- Multiple departments (colors for distinction only)
- Various file types (colors help identify, not rank)
- User-defined categories

### Common Color Palettes

**Primary colors:**
```csharp
paletteColorMapping.Colors = new List<Brush>()
{
    new SolidBrush(Color.Red),
    new SolidBrush(Color.Blue),
    new SolidBrush(Color.Yellow),
    new SolidBrush(Color.Green)
};
```

**Pastel palette:**
```csharp
paletteColorMapping.Colors = new List<Brush>()
{
    new SolidBrush(ColorTranslator.FromHtml("#FFB3BA")),  // Pastel red
    new SolidBrush(ColorTranslator.FromHtml("#FFDFBA")),  // Pastel orange
    new SolidBrush(ColorTranslator.FromHtml("#FFFFBA")),  // Pastel yellow
    new SolidBrush(ColorTranslator.FromHtml("#BAFFC9")),  // Pastel green
    new SolidBrush(ColorTranslator.FromHtml("#BAE1FF"))   // Pastel blue
};
```

**Professional palette:**
```csharp
paletteColorMapping.Colors = new List<Brush>()
{
    new SolidBrush(ColorTranslator.FromHtml("#2E86AB")),  // Blue
    new SolidBrush(ColorTranslator.FromHtml("#A23B72")),  // Purple
    new SolidBrush(ColorTranslator.FromHtml("#F18F01")),  // Orange
    new SolidBrush(ColorTranslator.FromHtml("#C73E1D")),  // Red
    new SolidBrush(ColorTranslator.FromHtml("#6A994E"))   // Green
};
```

## Choosing Color Mapping Strategy

### Decision Flow

```
Do colors represent data meaning?
├─ No → Use UniColorMapping (single brand color)
│
└─ Yes → What type of data?
    ├─ Discrete categories (low/med/high) → RangeBrushColorMapping
    │
    ├─ Continuous gradient → DesaturationColorMapping
    │
    └─ Arbitrary distinction → PaletteColorMapping
```

### Strategy Comparison

| Mapping Type | Data Type | Color Count | Semantics | Legend |
|--------------|-----------|-------------|-----------|--------|
| UniColorMapping | Any | 1 | None | Not needed |
| RangeBrushColorMapping | Numeric ranges | 2-10 | Meaningful | Yes, recommended |
| DesaturationColorMapping | Continuous numeric | Infinite | Intensity | Optional |
| PaletteColorMapping | Categorical | 3-12 | Arbitrary | Optional |

## Color Mapping Best Practices

### Accessibility

**Use colorblind-safe palettes:**
```csharp
// Avoid red-green combinations
// Use blue-orange, purple-yellow instead

// Colorblind-friendly range mapping
rangeMapping.Brushes.Add(new RangeBrush
{
    Color = ColorTranslator.FromHtml("#0173B2"),  // Blue (safe)
    From = 0,
    To = 5,
    LegendLabel = "Low"
});

rangeMapping.Brushes.Add(new RangeBrush
{
    Color = ColorTranslator.FromHtml("#DE8F05"),  // Orange (safe)
    From = 5,
    To = 10,
    LegendLabel = "High"
});
```

### Contrast

**Ensure sufficient contrast for labels:**
- Dark colors need light labels
- Light colors need dark labels
- Consider label visibility when choosing colors

### Consistency

**Maintain color meaning across visualizations:**
- Red = negative/bad, Green = positive/good
- Keep color associations consistent in your application
- Document color meaning for users

### Legend

**Always provide legend for RangeBrushColorMapping:**
- Users need to understand color meanings
- Set meaningful LegendLabel values
- Position legend appropriately

### Performance

**Limit palette size:**
- 3-12 colors optimal for PaletteColorMapping
- Too many colors reduce distinguishability
- Consider grouping if you need more categories

### Testing

**Test with real data:**
- Verify range definitions match actual data
- Check RangeMinimum/RangeMaximum accuracy
- Ensure colors are distinguishable with real values

---

**Key Takeaway:** Choose color mapping based on data semantics. Use RangeBrushColorMapping for meaningful value ranges, DesaturationColorMapping for continuous gradients, PaletteColorMapping for categorical distinction, and UniColorMapping when color has no meaning.