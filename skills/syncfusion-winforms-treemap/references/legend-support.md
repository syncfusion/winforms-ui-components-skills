# TreeMap Legend Support

The TreeMap legend provides a visual key to explain color meanings, particularly useful with RangeBrushColorMapping to show what each color range represents.

## Legend Overview

The TreeMap legend displays color indicators with labels, helping users understand what colors represent in the visualization. Legends are most appropriate for TreeMaps using RangeBrushColorMapping where colors have specific meanings.

### When to Use Legends

**Recommended:**
- RangeBrushColorMapping with multiple ranges
- When colors represent meaningful data (performance, risk, categories)
- Unfamiliar users who need color interpretation guidance
- Presentations and reports

**Optional or Not Needed:**
- UniColorMapping (single color, no meaning)
- DesaturationColorMapping (gradient is self-explanatory)
- PaletteColorMapping (colors arbitrary)
- Expert users familiar with color meanings

## Legend Configuration Properties

Configure legend appearance and positioning using TreeMap properties:

| Property | Type | Options | Description |
|----------|------|---------|-------------|
| `LegendPosition` | LegendPositions | Top, Bottom, Left, Right | Where legend appears |
| `LegendType` | LegendTypes | Rectangle, Circle, Ellipse, etc. | Icon shape |
| `LegendGap` | int | Pixels | Spacing between legend items |

## Legend Position

The `LegendPosition` property controls where the legend appears relative to the TreeMap.

### Setting Legend Position

```csharp
// Position at top
treeMap1.LegendSetting.LegendPosition = LegendPositions.Top;

// Position at bottom
treeMap1.LegendSetting.LegendPosition = LegendPositions.Bottom;

// Position on left
treeMap1.LegendSetting.LegendPosition = LegendPositions.Left;

// Position on right
treeMap1.LegendSetting.LegendPosition = LegendPositions.Right;
```

### Position Guidelines

| Position | Best For | Considerations |
|----------|----------|----------------|
| **Top** | Wide layouts, dashboards | Leaves maximum space for TreeMap |
| **Bottom** | Wide layouts, reports | Traditional placement, familiar |
| **Left** | Tall layouts, sidebars | Works with vertical displays |
| **Right** | Most applications | Common, intuitive placement |

**Recommendation:** Use `Right` position for most applications (conventional placement)

## Legend Type (Icon Shapes)

The `LegendType` property sets the shape of legend icons.

### Available Legend Types

```csharp
// Rectangle icons (default)
treeMap1.LegendSetting.LegendType = LegendTypes.Rectangle;

// Circle icons
treeMap1.LegendSetting.LegendType = LegendTypes.Circle;

// Ellipse icons
treeMap1.LegendSetting.LegendType = LegendTypes.Ellipse;
```

### Icon Shape Selection

**Rectangle:**
- Most common
- Matches TreeMap rectangle shapes
- Professional appearance

**Circle:**
- Softer appearance
- Good for dashboards
- Modern look

**Ellipse:**
- Unique styling
- Artistic designs
- Less common

**Recommendation:** Use `Rectangle` for consistency with TreeMap shape

## Legend Gap

The `LegendGap` property controls spacing between legend items.

### Setting Legend Gap

```csharp
// 100 pixels between legend items
treeMap1.LegendSetting.LegendGap = 100;

// Tight spacing (minimal gap)
treeMap1.LegendSetting.LegendGap = 20;

// Generous spacing
treeMap1.LegendSetting.LegendGap = 150;
```

### Gap Size Guidelines

| Gap Size | Effect | Use Case |
|----------|--------|----------|
| 10-30 px | Tight spacing | Many legend items, limited space |
| 50-80 px | Standard spacing | 3-5 legend items |
| 100-150 px | Generous spacing | Few items, plenty of space |
| 150+ px | Wide spacing | Emphasis, specific design needs |

**Recommendation:** Use 100px for most applications (balanced, readable)

## Complete Legend Example

```csharp
TreeMap treeMap1 = new TreeMap();
PopulationViewModel data = new PopulationViewModel();

treeMap1.ItemsSource = data.PopulationDetails;
treeMap1.WeightValuePath = "Population";
treeMap1.ColorValuePath = "Growth";
treeMap1.ItemsLayoutMode = ItemsLayoutModes.SliceAndDiceAuto;

// Configure legend appearance
treeMap1.LegendSetting.LegendPosition = LegendPositions.Top;
treeMap1.LegendSetting.LegendType = LegendTypes.Ellipse;
treeMap1.LegendSetting.LegendGap = 100;

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

// Create RangeBrushColorMapping with legend labels
RangeBrushColorMapping rangeBrushColorMapping = new RangeBrushColorMapping();

rangeBrushColorMapping.Brushes.Add(new RangeBrush
{
    Color = ColorTranslator.FromHtml("#77D8D8"),
    From = 0,
    To = 1,
    LegendLabel = "1% Growth"  // Label shown in legend
});

rangeBrushColorMapping.Brushes.Add(new RangeBrush
{
    Color = ColorTranslator.FromHtml("#AED960"),
    From = 0,
    To = 2,
    LegendLabel = "2% Growth"  // Label shown in legend
});

rangeBrushColorMapping.Brushes.Add(new RangeBrush
{
    Color = ColorTranslator.FromHtml("#FFAF51"),
    From = 0,
    To = 3,
    LegendLabel = "3% Growth"  // Label shown in legend
});

rangeBrushColorMapping.Brushes.Add(new RangeBrush
{
    Color = ColorTranslator.FromHtml("#F3D240"),
    From = 0,
    To = 20,
    LegendLabel = "20% Growth"  // Label shown in legend
});

treeMap1.LeafColorMapping = rangeBrushColorMapping;
```

## Legend Labels in RangeBrush

Legend items come from the `LegendLabel` property in each `RangeBrush`. Always set meaningful legend labels.

### Setting Legend Labels

```csharp
RangeBrushColorMapping rangeMapping = new RangeBrushColorMapping();

// Each RangeBrush has a LegendLabel
rangeMapping.Brushes.Add(new RangeBrush
{
    Color = Color.Red,
    From = -10,
    To = 0,
    LegendLabel = "Negative Growth"  // Shown in legend
});

rangeMapping.Brushes.Add(new RangeBrush
{
    Color = Color.Yellow,
    From = 0,
    To = 5,
    LegendLabel = "Moderate Growth"  // Shown in legend
});

rangeMapping.Brushes.Add(new RangeBrush
{
    Color = Color.Green,
    From = 5,
    To = 100,
    LegendLabel = "Strong Growth"  // Shown in legend
});

treeMap1.LeafColorMapping = rangeMapping;
```

### Legend Label Best Practices

**Be descriptive:**
```csharp
// Good
LegendLabel = "High Risk (>75%)"

// Less helpful
LegendLabel = "High"
```

**Include range information:**
```csharp
// Helpful
LegendLabel = "Sales: $0-$10K"

// Vague
LegendLabel = "Low Sales"
```

**Use consistent formatting:**
```csharp
// Consistent
LegendLabel = "0-25%"
LegendLabel = "25-50%"
LegendLabel = "50-75%"
LegendLabel = "75-100%"

// Inconsistent (avoid)
LegendLabel = "Less than 25"
LegendLabel = "25 to 50 percent"
LegendLabel = "Above 50"
```

## Common Legend Patterns

### Performance Indicators

```csharp
treeMap1.LegendSetting.LegendPosition = LegendPositions.Top;
treeMap1.LegendSetting.LegendType = LegendTypes.Rectangle;
treeMap1.LegendSetting.LegendGap = 80;

RangeBrushColorMapping rangeMapping = new RangeBrushColorMapping();

rangeMapping.Brushes.Add(new RangeBrush
{
    Color = Color.Red,
    From = 0,
    To = 50,
    LegendLabel = "Below Target (0-50%)"
});

rangeMapping.Brushes.Add(new RangeBrush
{
    Color = Color.Orange,
    From = 50,
    To = 75,
    LegendLabel = "Approaching Target (50-75%)"
});

rangeMapping.Brushes.Add(new RangeBrush
{
    Color = Color.Yellow,
    From = 75,
    To = 100,
    LegendLabel = "At Target (75-100%)"
});

rangeMapping.Brushes.Add(new RangeBrush
{
    Color = Color.Green,
    From = 100,
    To = 200,
    LegendLabel = "Above Target (100%+)"
});

treeMap1.LeafColorMapping = rangeMapping;
```

### Risk Levels

```csharp
treeMap1.LegendSetting.LegendPosition = LegendPositions.Right;
treeMap1.LegendSetting.LegendType = LegendTypes.Circle;
treeMap1.LegendSetting.LegendGap = 60;

RangeBrushColorMapping rangeMapping = new RangeBrushColorMapping();

rangeMapping.Brushes.Add(new RangeBrush
{
    Color = Color.Green,
    From = 0,
    To = 3,
    LegendLabel = "Low Risk (0-3)"
});

rangeMapping.Brushes.Add(new RangeBrush
{
    Color = Color.Yellow,
    From = 3,
    To = 6,
    LegendLabel = "Medium Risk (3-6)"
});

rangeMapping.Brushes.Add(new RangeBrush
{
    Color = Color.Orange,
    From = 6,
    To = 8,
    LegendLabel = "High Risk (6-8)"
});

rangeMapping.Brushes.Add(new RangeBrush
{
    Color = Color.Red,
    From = 8,
    To = 10,
    LegendLabel = "Critical Risk (8-10)"
});

treeMap1.LeafColorMapping = rangeMapping;
```

### Simple Categories

```csharp
treeMap1.LegendSetting.LegendPosition = LegendPositions.Bottom;
treeMap1.LegendSetting.LegendType = LegendTypes.Rectangle;
treeMap1.LegendSetting.LegendGap = 100;

RangeBrushColorMapping rangeMapping = new RangeBrushColorMapping();

rangeMapping.Brushes.Add(new RangeBrush
{
    Color = ColorTranslator.FromHtml("#3498DB"),
    From = 1,
    To = 1,
    LegendLabel = "Category A"
});

rangeMapping.Brushes.Add(new RangeBrush
{
    Color = ColorTranslator.FromHtml("#E74C3C"),
    From = 2,
    To = 2,
    LegendLabel = "Category B"
});

rangeMapping.Brushes.Add(new RangeBrush
{
    Color = ColorTranslator.FromHtml("#2ECC71"),
    From = 3,
    To = 3,
    LegendLabel = "Category C"
});

treeMap1.LeafColorMapping = rangeMapping;
```

## Dynamic Legend Configuration

### Responsive Legend Position

```csharp
private void UpdateLegendPosition()
{
    double aspectRatio = (double)treeMap1.Width / treeMap1.Height;
    
    if (aspectRatio > 1.5)
    {
        // Wide layout - legend at top
        treeMap1.LegendSetting.LegendPosition = LegendPositions.Top;
    }
    else if (aspectRatio < 0.7)
    {
        // Tall layout - legend at bottom
        treeMap1.LegendSetting.LegendPosition = LegendPositions.Bottom;
    }
    else
    {
        // Balanced layout - legend on right
        treeMap1.LegendSetting.LegendPosition = LegendPositions.Right;
    }
}
```

### Adaptive Legend Gap

```csharp
private void UpdateLegendGap(int legendItemCount)
{
    // Adjust gap based on number of legend items
    if (legendItemCount <= 3)
    {
        treeMap1.LegendSetting.LegendGap = 150;  // Generous spacing for few items
    }
    else if (legendItemCount <= 5)
    {
        treeMap1.LegendSetting.LegendGap = 100;  // Standard spacing
    }
    else
    {
        treeMap1.LegendSetting.LegendGap = 50;   // Tight spacing for many items
    }
}
```

## Legend Visibility

### Showing/Hiding Legend

While there's no explicit `ShowLegend` property, legend appears automatically when using RangeBrushColorMapping with LegendLabel values.

**Legend appears when:**
- Using RangeBrushColorMapping
- At least one RangeBrush has a LegendLabel set
- Legend properties (Position, Type, Gap) are configured

**Legend does not appear when:**
- Using UniColorMapping
- Using DesaturationColorMapping
- Using PaletteColorMapping
- No LegendLabel values set in RangeBrush items

## Best Practices

### Label Clarity

1. **Be specific:** Include units and ranges in labels
2. **Consistent format:** Use same pattern for all labels
3. **Brevity:** Keep labels concise but meaningful
4. **Avoid jargon:** Use terms users understand

### Positioning

1. **Consider layout:** Place legend where it doesn't obscure data
2. **Match reading direction:** Right or top positions are intuitive
3. **Space efficiency:** Top/bottom for wide layouts, sides for square
4. **User familiarity:** Right position is most conventional

### Visual Design

1. **Match icon shape:** Rectangle matches TreeMap rectangles
2. **Appropriate spacing:** Don't crowd legend items
3. **Color consistency:** Ensure legend colors match TreeMap exactly
4. **Accessibility:** Ensure legend is readable with sufficient contrast

### Performance

1. **Reasonable item count:** 3-7 legend items optimal
2. **Avoid excessive gap:** Very large gaps waste space
3. **Simple shapes:** Rectangle/Circle render faster than complex shapes

## Troubleshooting

### Legend Not Appearing

**Problem:** Legend is not visible

**Solutions:**
1. Verify using RangeBrushColorMapping (other types don't show legends)
2. Ensure `LegendLabel` is set on RangeBrush items
3. Check that RangeBrush collection is not empty
4. Verify TreeMap size is large enough to display legend

### Legend Overlapping TreeMap

**Problem:** Legend covers part of TreeMap data

**Solution:** Legend should appear outside TreeMap area automatically. If overlapping occurs, try different `LegendPosition`

### Legend Items Too Close

**Problem:** Legend items are cramped

**Solution:** Increase `LegendGap` value (try 100-150 pixels)

### Legend Labels Truncated

**Problem:** Legend text is cut off

**Solutions:**
1. Shorten `LegendLabel` text
2. Increase TreeMap size
3. Change `LegendPosition` to allow more space
4. Use abbreviations in labels

---

**Key Takeaway:** Legends are essential for RangeBrushColorMapping to explain color meanings. Position legends appropriately, use clear labels, and maintain consistent formatting for best user experience.