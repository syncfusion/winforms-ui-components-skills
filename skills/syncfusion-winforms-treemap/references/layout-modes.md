# TreeMap Layout Modes

## Table of Contents
- [ItemsLayoutMode Overview](#itemslayoutmode-overview)
- [Squarified Layout](#squarified-layout)
- [SliceAndDiceAuto Layout](#sliceanddiceauto-layout)
- [SliceAndDiceHorizontal Layout](#sliceanddice horizontal-layout)
- [SliceAndDiceVertical Layout](#sliceanddicevertical-layout)
- [Choosing the Right Layout](#choosing-the-right-layout)
- [Layout Comparison](#layout-comparison)

## ItemsLayoutMode Overview

The `ItemsLayoutMode` property of the TreeMap control specifies the algorithm used to arrange rectangles. This layout mode is applied to all levels in the TreeMap hierarchy.

### Available Layout Modes

The TreeMap control supports four different layout algorithms:

| Layout Mode | Description |
|-------------|-------------|
| `Squarified` | Balances rectangle aspect ratios for readability |
| `SliceAndDiceAuto` | Alternates horizontal/vertical splits by level |
| `SliceAndDiceHorizontal` | All rectangles split horizontally |
| `SliceAndDiceVertical` | All rectangles split vertically |

### Setting Layout Mode

```csharp
treeMap1.ItemsLayoutMode = ItemsLayoutModes.Squarified;
// or
treeMap1.ItemsLayoutMode = ItemsLayoutModes.SliceAndDiceAuto;
// or
treeMap1.ItemsLayoutMode = ItemsLayoutModes.SliceAndDiceHorizontal;
// or
treeMap1.ItemsLayoutMode = ItemsLayoutModes.SliceAndDiceVertical;
```

## Squarified Layout

The Squarified algorithm attempts to create rectangles with aspect ratios as close to square as possible. This produces the most visually balanced and readable layout.

### Characteristics

- **Balanced aspect ratios:** Rectangles tend toward square shapes
- **Better readability:** Labels and content easier to read
- **Optimal space usage:** Efficient use of available area
- **Variable orientation:** Rectangles oriented as needed for best fit

### Configuration

```csharp
TreeMap treeMap1 = new TreeMap();
PopulationViewModel data = new PopulationViewModel();

treeMap1.ItemsSource = data.PopulationDetails;
treeMap1.WeightValuePath = "Population";
treeMap1.ColorValuePath = "Growth";

// Set Squarified layout
treeMap1.ItemsLayoutMode = ItemsLayoutModes.Squarified;

TreeMapFlatLevel level1 = new TreeMapFlatLevel();
level1.GroupPath = "Continent";
level1.ShowLabels = true;
treeMap1.Levels.Add(level1);

TreeMapFlatLevel level2 = new TreeMapFlatLevel();
level2.GroupPath = "Country";
level2.ShowLabels = true;
level2.HeaderHeight = 25;
treeMap1.Levels.Add(level2);
```

### When to Use Squarified

**Best for:**
- General-purpose TreeMap visualizations
- Data with wide range of size variations
- When readability is priority
- Desktop applications with flexible layouts
- Presentations and reports

**Advantages:**
- Most readable layout
- Works well with varying data sizes
- Labels fit better in square-ish rectangles
- Professional appearance

**Disadvantages:**
- Layout changes more dramatically when data updates
- Harder to track specific item positions
- Not ideal for strict hierarchical clarity

## SliceAndDiceAuto Layout

The SliceAndDiceAuto algorithm alternates between horizontal and vertical slicing at each hierarchical level. First level slices one direction, second level slices the other direction, and so on.

### Characteristics

- **Alternating directions:** Changes slice direction per level
- **Clear level distinction:** Easy to see hierarchical levels
- **Predictable structure:** Consistent pattern throughout
- **Good for multi-level hierarchies:** Level changes are obvious

### Configuration

```csharp
TreeMap treeMap1 = new TreeMap();
PopulationViewModel data = new PopulationViewModel();

treeMap1.ItemsSource = data.PopulationDetails;
treeMap1.WeightValuePath = "Population";
treeMap1.ColorValuePath = "Growth";

// Set SliceAndDiceAuto layout
treeMap1.ItemsLayoutMode = ItemsLayoutModes.SliceAndDiceAuto;

TreeMapFlatLevel level1 = new TreeMapFlatLevel();
level1.GroupPath = "Continent";
level1.ShowLabels = true;
treeMap1.Levels.Add(level1);

TreeMapFlatLevel level2 = new TreeMapFlatLevel();
level2.GroupPath = "Country";
level2.ShowLabels = true;
level2.HeaderHeight = 25;
treeMap1.Levels.Add(level2);
```

### Alternation Pattern

**Two-level example:**
- Level 1 (Continent): Sliced horizontally
- Level 2 (Country): Sliced vertically within each continent

**Three-level example:**
- Level 1: Horizontal
- Level 2: Vertical
- Level 3: Horizontal

### When to Use SliceAndDiceAuto

**Best for:**
- Multi-level hierarchies (3+ levels)
- When level distinction is important
- Square or slightly rectangular display areas
- Organizational charts
- File system visualizations

**Advantages:**
- Clear visual hierarchy
- Easy to understand structure
- Stable layout (minimal change on updates)
- Good for drill-down scenarios

**Disadvantages:**
- Can produce very thin rectangles
- Less optimal aspect ratios than Squarified
- May waste space with certain data distributions

## SliceAndDiceHorizontal Layout

The SliceAndDiceHorizontal algorithm slices all rectangles horizontally, creating rows of rectangles at all levels.

### Characteristics

- **All horizontal splits:** Rectangles arranged in rows
- **Wide rectangles:** Tendency toward landscape orientation
- **Consistent direction:** Same slicing throughout
- **Scanline pattern:** Easy to scan top to bottom

### Configuration

```csharp
TreeMap treeMap1 = new TreeMap();
PopulationViewModel data = new PopulationViewModel();

treeMap1.ItemsSource = data.PopulationDetails;
treeMap1.WeightValuePath = "Population";
treeMap1.ColorValuePath = "Growth";

// Set SliceAndDiceHorizontal layout
treeMap1.ItemsLayoutMode = ItemsLayoutModes.SliceAndDiceHorizontal;

TreeMapFlatLevel level1 = new TreeMapFlatLevel();
level1.GroupPath = "Continent";
level1.ShowLabels = true;
treeMap1.Levels.Add(level1);

TreeMapFlatLevel level2 = new TreeMapFlatLevel();
level2.GroupPath = "Country";
level2.ShowLabels = true;
level2.HeaderHeight = 25;
treeMap1.Levels.Add(level2);
```

### When to Use SliceAndDiceHorizontal

**Best for:**
- Wide display areas (landscape orientation)
- Widescreen monitors or dashboards
- When horizontal scanning is natural
- Timeline-like visualizations
- Gantt chart alternatives

**Advantages:**
- Works well on wide screens
- Natural left-to-right reading
- Good for horizontal labels
- Stable positions for items

**Disadvantages:**
- Can create very short, wide rectangles
- Poor use of space on tall displays
- Labels may be truncated on narrow rectangles

## SliceAndDiceVertical Layout

The SliceAndDiceVertical algorithm slices all rectangles vertically, creating columns of rectangles at all levels.

### Characteristics

- **All vertical splits:** Rectangles arranged in columns
- **Tall rectangles:** Tendency toward portrait orientation
- **Consistent direction:** Same slicing throughout
- **Column pattern:** Easy to scan left to right

### Configuration

```csharp
TreeMap treeMap1 = new TreeMap();
PopulationViewModel data = new PopulationViewModel();

treeMap1.ItemsSource = data.PopulationDetails;
treeMap1.WeightValuePath = "Population";
treeMap1.ColorValuePath = "Growth";

// Set SliceAndDiceVertical layout
treeMap1.ItemsLayoutMode = ItemsLayoutModes.SliceAndDiceVertical;

TreeMapFlatLevel level1 = new TreeMapFlatLevel();
level1.GroupPath = "Continent";
level1.ShowLabels = true;
treeMap1.Levels.Add(level1);

TreeMapFlatLevel level2 = new TreeMapFlatLevel();
level2.GroupPath = "Country";
level2.ShowLabels = true;
level2.HeaderHeight = 25;
treeMap1.Levels.Add(level2);
```

### When to Use SliceAndDiceVertical

**Best for:**
- Tall display areas (portrait orientation)
- Mobile devices in portrait mode
- Sidebar widgets
- Vertical navigation patterns
- When vertical scanning is preferred

**Advantages:**
- Works well on tall screens
- Good for vertical labels
- Natural top-to-bottom reading in some contexts
- Stable positions for items

**Disadvantages:**
- Can create very narrow, tall rectangles
- Poor use of space on wide displays
- Labels may not fit in narrow rectangles

## Choosing the Right Layout

### Decision Matrix

| Scenario | Recommended Layout | Why |
|----------|-------------------|-----|
| General visualization | Squarified | Best readability and balance |
| Multi-level hierarchy | SliceAndDiceAuto | Clear level separation |
| Widescreen dashboard | SliceAndDiceHorizontal | Optimizes horizontal space |
| Mobile portrait | SliceAndDiceVertical | Fits tall screens |
| Variable data sizes | Squarified | Handles range of sizes well |
| Fixed hierarchies | SliceAndDiceAuto | Predictable structure |
| Space efficiency | Squarified | Optimal area usage |
| Position stability | SliceAndDice* | Less layout change on updates |

### Display Area Considerations

**Square or slightly rectangular area:**
- First choice: Squarified
- Alternative: SliceAndDiceAuto

**Wide area (width > 2× height):**
- First choice: SliceAndDiceHorizontal
- Alternative: Squarified

**Tall area (height > 2× width):**
- First choice: SliceAndDiceVertical
- Alternative: Squarified

### Data Characteristics

**Wide range of sizes:**
- Use Squarified for better handling of extremes

**Similar sizes:**
- Any layout works, choose based on display shape

**Many levels (3+):**
- Use SliceAndDiceAuto for hierarchical clarity

**Few items per level:**
- Any layout works well

**Many items per level:**
- Squarified provides better packing

## Layout Comparison

### Visual Comparison

Given the same data, different layouts produce different arrangements:

**Data: 5 items with weights [50, 30, 10, 7, 3]**

**Squarified:**
```
┌─────────────────┬──────┐
│                 │      │
│       A         │  B   │
│     (50)        │ (30) │
│                 │      │
├────────┬────┬───┴──────┤
│   C    │ D  │    E     │
│  (10)  │(7) │   (3)    │
└────────┴────┴──────────┘
```
- Most balanced rectangles
- Better aspect ratios

**SliceAndDiceHorizontal:**
```
┌──────────────────────────┐
│            A (50)        │
├──────────────────────────┤
│         B (30)           │
├──────────────────────────┤
│   C (10)                 │
├──────────────────────────┤
│ D (7)                    │
├──────────────────────────┤
│ E (3)                    │
└──────────────────────────┘
```
- All horizontal rows
- Can create thin rectangles

**SliceAndDiceVertical:**
```
┌─────┬────┬───┬──┬─┐
│     │    │   │  │ │
│     │    │   │  │ │
│  A  │ B  │ C │D │E│
│(50) │(30)│(10)(7)(3)
│     │    │   │  │ │
│     │    │   │  │ │
│     │    │   │  │ │
└─────┴────┴───┴──┴─┘
```
- All vertical columns
- Can create narrow rectangles

### Performance Considerations

**Layout calculation time:**
- Squarified: Medium (more complex algorithm)
- SliceAndDice*: Fast (simpler calculations)

**Recommendation:** Performance difference is negligible for typical data sizes (<10,000 items). Choose based on visual needs, not performance.

## Common Patterns

### Responsive Layout Pattern

```csharp
// Adjust layout based on container size
private void UpdateLayout()
{
    double aspectRatio = (double)treeMap1.Width / treeMap1.Height;
    
    if (aspectRatio > 2.0)
    {
        // Wide layout
        treeMap1.ItemsLayoutMode = ItemsLayoutModes.SliceAndDiceHorizontal;
    }
    else if (aspectRatio < 0.5)
    {
        // Tall layout
        treeMap1.ItemsLayoutMode = ItemsLayoutModes.SliceAndDiceVertical;
    }
    else
    {
        // Balanced layout
        treeMap1.ItemsLayoutMode = ItemsLayoutModes.Squarified;
    }
}

// Call on resize
protected override void OnResize(EventArgs e)
{
    base.OnResize(e);
    UpdateLayout();
}
```

### User Selection Pattern

```csharp
// Allow user to choose layout
private void layoutComboBox_SelectedIndexChanged(object sender, EventArgs e)
{
    switch (layoutComboBox.SelectedIndex)
    {
        case 0:
            treeMap1.ItemsLayoutMode = ItemsLayoutModes.Squarified;
            break;
        case 1:
            treeMap1.ItemsLayoutMode = ItemsLayoutModes.SliceAndDiceAuto;
            break;
        case 2:
            treeMap1.ItemsLayoutMode = ItemsLayoutModes.SliceAndDiceHorizontal;
            break;
        case 3:
            treeMap1.ItemsLayoutMode = ItemsLayoutModes.SliceAndDiceVertical;
            break;
    }
}
```

## Troubleshooting

### Issue: Rectangles Too Thin

**Symptom:** Some rectangles are unreadable stripes

**Solutions:**
1. Try Squarified layout for better aspect ratios
2. Filter out very small items from data
3. Adjust display area dimensions
4. Reduce number of levels

### Issue: Layout Looks Jumbled

**Symptom:** No clear structure or pattern

**Solution:** Try SliceAndDiceAuto for clearer hierarchical structure

### Issue: Wasted Space

**Symptom:** Large empty areas or poor space utilization

**Solution:** Use Squarified layout for optimal space filling

---

**Recommendation:** Start with Squarified layout for most applications. Switch to SliceAndDice variants if you need more predictable structure or your display area is extremely wide or tall.