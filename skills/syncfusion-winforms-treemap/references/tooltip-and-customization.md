# Tooltip Support and Advanced Customization

## Table of Contents
- [Tooltip Overview](#tooltip-overview)
- [Enabling Tooltips](#enabling-tooltips)
- [Header Tooltip Configuration](#header-tooltip-configuration)
- [Item Tooltip Configuration](#item-tooltip-configuration)
- [Tooltip Content Patterns](#tooltip-content-patterns)
- [Leaf Node Customization](#leaf-node-customization)
- [LeafItemDrawing Event](#leafitemdrawing-event)
- [Advanced Custom Rendering](#advanced-custom-rendering)

## Tooltip Overview

Tooltips show additional information when hovering over TreeMap elements. They provide detailed data that may not fit in the visualization itself, enhancing interactivity and user understanding.

### Tooltip Types

TreeMap supports two types of tooltips:

| Type | Target | Purpose |
|------|--------|---------|
| **Header Tooltips** | Group headers | Show information about grouped categories |
| **Item Tooltips** | Leaf nodes | Show detailed information about individual data items |

## Enabling Tooltips

### IsTootTipVisible Property

Control tooltip visibility using the `IsTootTipVisible` property:

```csharp
// Enable tooltips
treeMap1.IsTootTipVisible = true;

// Disable tooltips (default)
treeMap1.IsTootTipVisible = false;
```

**Default:** Tooltips are enabled (`true`)

## Header Tooltip Configuration

Header tooltips appear when hovering over group headers (when `HeaderHeight > 0` on TreeMapFlatLevel).

### HeaderToolTipInfo Property

```csharp
// Create tooltip configuration
ToolTipInfo headerToolTip = new ToolTipInfo();
headerToolTip.ToolTipHeaderPattern = "<b>{Label}</b>";
headerToolTip.ToolTipContentPattern = "Growth: {Growth}%";

// Assign to TreeMap
treeMap1.HeaderToolTipInfo = headerToolTip;
```

### ToolTipInfo Properties

| Property | Type | Description |
|----------|------|-------------|
| `ToolTipHeaderPattern` | string | HTML pattern for tooltip title |
| `ToolTipContentPattern` | string | HTML pattern for tooltip content |

## Item Tooltip Configuration

Item tooltips appear when hovering over leaf node rectangles.

### ItemToolTipInfo Property

```csharp
// Create tooltip configuration
ToolTipInfo itemToolTip = new ToolTipInfo();
itemToolTip.ToolTipHeaderPattern = "<b>{Country}</b>";
itemToolTip.ToolTipContentPattern = "Growth: {Growth}%\nPopulation: {StrPopulation}";

// Assign to TreeMap
treeMap1.ItemToolTipInfo = itemToolTip;
```

## Tooltip Content Patterns

Tooltip patterns use placeholders that are replaced with actual data values. Place property names in curly braces `{PropertyName}`.

### Property Placeholders

Reference any property from your data model:

```csharp
public class PopulationDetail
{
    public string Continent { get; set; }
    public string Country { get; set; }
    public double Growth { get; set; }
    public double Population { get; set; }
    public string StrPopulation { get; set; }
}

// Use properties in patterns
itemToolTip.ToolTipHeaderPattern = "<b>{Country}</b>";  // Uses Country property
itemToolTip.ToolTipContentPattern = "Growth: {Growth}%";  // Uses Growth property
```

### HTML Formatting

Tooltip patterns support basic HTML formatting:

| HTML Tag | Effect | Example |
|----------|--------|---------|
| `<b>` | Bold text | `<b>{Country}</b>` |
| `<i>` | Italic text | `<i>{Category}</i>` |
| `<br>` | Line break | `Line 1<br>Line 2` |
| `&nbsp;` | Space | `Value&nbsp;&nbsp;:&nbsp;{Value}` |

### Newline Characters

Use `\n` for line breaks in content:

```csharp
itemToolTip.ToolTipContentPattern = "Growth: {Growth}%\nPopulation: {StrPopulation}";
```

### Complete Tooltip Example

```csharp
TreeMap treeMap1 = new TreeMap();
PopulationViewModel data = new PopulationViewModel();

treeMap1.ItemsSource = data.PopulationDetails;
treeMap1.WeightValuePath = "Population";
treeMap1.ColorValuePath = "Growth";

// Enable tooltips
treeMap1.IsTootTipVisible = true;

// Configure header tooltips
ToolTipInfo headerToolTip = new ToolTipInfo();
headerToolTip.ToolTipHeaderPattern = "<b>{Label}</b>";
headerToolTip.ToolTipContentPattern = "Growth: {Growth}%";
treeMap1.HeaderToolTipInfo = headerToolTip;

// Configure item tooltips
ToolTipInfo itemToolTip = new ToolTipInfo();
itemToolTip.ToolTipHeaderPattern = "<b>{Country}</b>";
itemToolTip.ToolTipContentPattern = "Growth: {Growth}%\nPopulation: {StrPopulation}";
treeMap1.ItemToolTipInfo = itemToolTip;

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
```

## Common Tooltip Patterns

### Simple Data Display

```csharp
ToolTipInfo itemToolTip = new ToolTipInfo();
itemToolTip.ToolTipHeaderPattern = "{Name}";
itemToolTip.ToolTipContentPattern = "Value: {Value}";
treeMap1.ItemToolTipInfo = itemToolTip;
```

### Multi-Line Information

```csharp
ToolTipInfo itemToolTip = new ToolTipInfo();
itemToolTip.ToolTipHeaderPattern = "<b>{ProductName}</b>";
itemToolTip.ToolTipContentPattern = "Sales: ${Sales}\nUnits: {Units}\nGrowth: {Growth}%";
treeMap1.ItemToolTipInfo = itemToolTip;
```

### Formatted Values

```csharp
ToolTipInfo itemToolTip = new ToolTipInfo();
itemToolTip.ToolTipHeaderPattern = "<b>{Region}</b> - <i>{Country}</i>";
itemToolTip.ToolTipContentPattern = "Revenue: ${Revenue}\nMargin: {Margin}%\nRank: #{Rank}";
treeMap1.ItemToolTipInfo = itemToolTip;
```

### Aligned Tooltips

```csharp
ToolTipInfo itemToolTip = new ToolTipInfo();
itemToolTip.ToolTipHeaderPattern = "<b>{Item}</b>";
itemToolTip.ToolTipContentPattern = 
    "Property&nbsp;&nbsp;&nbsp;:&nbsp;{Property}\n" +
    "Value&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;:&nbsp;{Value}\n" +
    "Status&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;:&nbsp;{Status}";
treeMap1.ItemToolTipInfo = itemToolTip;
```

## Leaf Node Customization

For advanced scenarios beyond standard tooltips, customize leaf node rendering using the `LeafItemDrawing` event.

### When to Use Custom Rendering

**Use custom rendering when you need:**
- Custom graphics or images in rectangles
- Non-standard label positioning
- Unique visual effects
- Dynamic visual elements based on data
- Integration with external image resources

**Use standard properties when:**
- Standard labels and colors suffice
- Performance is critical (custom rendering is slower)
- Maintenance simplicity is important

## LeafItemDrawing Event

The `LeafItemDrawing` event fires for each leaf node, allowing complete control over how it's drawn.

### Event Handler Setup

```csharp
// Subscribe to event
treeMap1.LeafItemDrawing += TreeMap_LeafItemDrawing;

// Event handler
private void TreeMap_LeafItemDrawing(object sender, LeafItemDrawingEventArgs e)
{
    // Cancel default drawing
    e.Cancel = true;
    
    // Custom drawing code
    if (e.Graphics != null && e.Cancel)
    {
        // Draw custom rectangle
        e.Graphics.FillRectangle(new SolidBrush(e.Color), e.RectSize);
        
        // Draw custom border
        e.Graphics.DrawRectangle(
            new Pen(new SolidBrush(Color.White), 5), 
            e.RectSize
        );
        
        // Draw custom label
        e.Graphics.DrawString(
            e.Label, 
            new Font("Segoe UI", 12F), 
            new SolidBrush(Color.White), 
            e.RectSize.X, 
            e.RectSize.Y
        );
    }
}
```

### LeafItemDrawingEventArgs Properties

| Property | Type | Description |
|----------|------|-------------|
| `Cancel` | bool | Set `true` to override default drawing |
| `Graphics` | Graphics | Graphics object for drawing |
| `RectSize` | Rectangle | Rectangle bounds for the leaf node |
| `Color` | Color | Calculated color for the leaf node |
| `Label` | string | Label text for the leaf node |
| `Data` | object | Original data item object |

### Accessing Data in Event

Cast `e.Data` to your data model type to access all properties:

```csharp
private void TreeMap_LeafItemDrawing(object sender, LeafItemDrawingEventArgs e)
{
    e.Cancel = true;
    
    if (e.Graphics != null && e.Cancel)
    {
        // Cast to your data type
        var dataItem = e.Data as PopulationDetail;
        
        if (dataItem != null)
        {
            // Use any property from data model
            string text = $"{dataItem.Country}: {dataItem.StrPopulation}";
            
            // Draw custom text
            e.Graphics.DrawString(
                text,
                new Font("Arial", 10F),
                Brushes.Black,
                e.RectSize.Location
            );
        }
    }
}
```

## Advanced Custom Rendering

### Custom Rectangle with Images

```csharp
public class OlympicMedals
{
    public string Country { get; set; }
    public string GameName { get; set; }
    public double GoldMedals { get; set; }
    public double TotalMedals { get; set; }
    public Image GameImage { get; set; }
}

private void TreeMap_LeafItemDrawing(object sender, LeafItemDrawingEventArgs e)
{
    e.Cancel = true;
    
    if (e.Graphics != null && e.Cancel)
    {
        // Fill background
        e.Graphics.FillRectangle(new SolidBrush(e.Color), e.RectSize);
        
        // Draw border
        e.Graphics.DrawRectangle(
            new Pen(new SolidBrush(Color.White), 5), 
            e.RectSize
        );
        
        // Draw label at top
        e.Graphics.DrawString(
            e.Label, 
            new Font("Segoe UI", 12F), 
            new SolidBrush(Color.White), 
            e.RectSize.X, 
            e.RectSize.Y
        );
        
        // Draw image in center
        var dataItem = e.Data as OlympicMedals;
        if (dataItem?.GameImage != null)
        {
            Image image = dataItem.GameImage;
            int imgX = e.RectSize.X + (e.RectSize.Width / 2) - (image.Width / 2);
            int imgY = e.RectSize.Y + (e.RectSize.Height / 2) - (image.Height / 2);
            e.Graphics.DrawImage(image, new Point(imgX, imgY));
        }
    }
}
```

### Conditional Styling

```csharp
private void TreeMap_LeafItemDrawing(object sender, LeafItemDrawingEventArgs e)
{
    e.Cancel = true;
    
    if (e.Graphics != null && e.Cancel)
    {
        var dataItem = e.Data as SalesData;
        
        // Different styling based on data
        Color bgColor = e.Color;
        Color borderColor = Color.White;
        int borderWidth = 2;
        
        if (dataItem != null && dataItem.Sales > 100000)
        {
            // Special styling for high performers
            borderColor = Color.Gold;
            borderWidth = 5;
        }
        
        // Draw with conditional styling
        e.Graphics.FillRectangle(new SolidBrush(bgColor), e.RectSize);
        e.Graphics.DrawRectangle(
            new Pen(borderColor, borderWidth), 
            e.RectSize
        );
        
        // Draw label
        Font font = dataItem.Sales > 100000 
            ? new Font("Segoe UI", 14F, FontStyle.Bold) 
            : new Font("Segoe UI", 11F);
            
        e.Graphics.DrawString(e.Label, font, Brushes.White, e.RectSize.Location);
    }
}
```

### Multi-Element Layout

```csharp
private void TreeMap_LeafItemDrawing(object sender, LeafItemDrawingEventArgs e)
{
    e.Cancel = true;
    
    if (e.Graphics != null && e.Cancel)
    {
        var dataItem = e.Data as ProductData;
        
        // Fill background
        e.Graphics.FillRectangle(new SolidBrush(e.Color), e.RectSize);
        
        // Draw border
        e.Graphics.DrawRectangle(Pens.White, e.RectSize);
        
        // Layout positions
        int padding = 5;
        int x = e.RectSize.X + padding;
        int y = e.RectSize.Y + padding;
        
        // Draw product name (top)
        e.Graphics.DrawString(
            dataItem.ProductName,
            new Font("Segoe UI", 11F, FontStyle.Bold),
            Brushes.White,
            x, y
        );
        
        // Draw sales (middle)
        y += 20;
        e.Graphics.DrawString(
            $"Sales: ${dataItem.Sales:N0}",
            new Font("Segoe UI", 9F),
            Brushes.White,
            x, y
        );
        
        // Draw units (bottom)
        y += 15;
        e.Graphics.DrawString(
            $"Units: {dataItem.Units}",
            new Font("Segoe UI", 9F),
            Brushes.White,
            x, y
        );
    }
}
```

## Best Practices

### Tooltips

1. **Keep it concise:** Show only relevant information
2. **Use clear labels:** Prefix values with descriptive text
3. **Format appropriately:** Use units, currency symbols, percentages
4. **HTML sparingly:** Use bold for emphasis, avoid overformatting
5. **Test with real data:** Ensure tooltips fit and are readable

### Custom Rendering

1. **Check rectangle size:** Only draw if rectangle is large enough
2. **Handle null data:** Always check if e.Data cast succeeds
3. **Optimize performance:** Minimize complex graphics operations
4. **Cache resources:** Create fonts, pens, brushes once
5. **Test thoroughly:** Custom rendering can have edge cases

### Performance Considerations

**Tooltips:**
- Minimal performance impact
- Safe for large datasets
- Recommended for all interactive TreeMaps

**Custom Rendering:**
- Can impact performance with many items
- Test with realistic data sizes
- Consider disabling for very large datasets (1000+ items)
- Cache frequently used graphics objects

## Troubleshooting

### Tooltips Not Showing

**Problem:** Tooltips don't appear on hover

**Solutions:**
1. Set `IsTootTipVisible = true`
2. Verify `HeaderToolTipInfo` or `ItemToolTipInfo` is configured
3. Check that property names in patterns match data model exactly (case-sensitive)
4. Ensure data objects are not null

### Tooltip Shows "{PropertyName}"

**Problem:** Tooltip displays placeholder instead of value

**Solutions:**
1. Verify property name spelling matches data model exactly
2. Check property is public
3. Ensure property exists on data object

### Custom Drawing Not Working

**Problem:** Custom LeafItemDrawing code doesn't execute

**Solutions:**
1. Verify event is subscribed: `treeMap1.LeafItemDrawing += Handler;`
2. Set `e.Cancel = true` to override default drawing
3. Check `if (e.Graphics != null && e.Cancel)` before drawing

### Custom Drawing Performance Issues

**Problem:** TreeMap is slow with custom rendering

**Solutions:**
1. Cache Font, Pen, Brush objects (don't create in event handler)
2. Minimize Graphics operations
3. Check rectangle size before drawing complex elements
4. Consider disabling custom rendering for small rectangles

---

**Key Takeaway:** Tooltips provide essential interactivity with minimal setup. Use `LeafItemDrawing` event only when standard customization options (labels, colors, borders) are insufficient for your needs.