# Headers and Labels Customization

Customize the appearance of labels and headers in TreeMap to improve readability and match your application's visual design.

## Label Customization Properties

Labels and headers in a TreeMap can be customized using the following properties available in the TreeMap control:

| Property | Type | Description |
|----------|------|-------------|
| `LabelFont` | Font | Font style, size, and family for labels |
| `LabelBrush` | Brush | Text color for labels |
| `LabelBackgroundBrush` | Brush | Background color behind labels |
| `LabelBorderBrush` | Brush | Border color around labels |
| `LabelBorderThickness` | int | Border width in pixels |
| `HeaderBrush` | Brush | Background color for level headers |

## ShowLabels Property

Control label visibility at each hierarchical level using the `ShowLabels` property on `TreeMapFlatLevel`:

```csharp
TreeMapFlatLevel level1 = new TreeMapFlatLevel();
level1.GroupPath = "Continent";
level1.ShowLabels = true;  // Show labels for this level
treeMap1.Levels.Add(level1);

TreeMapFlatLevel level2 = new TreeMapFlatLevel();
level2.GroupPath = "Country";
level2.ShowLabels = true;  // Show labels for this level
level2.HeaderHeight = 25;
treeMap1.Levels.Add(level2);
```

**Default:** `ShowLabels = false` (labels hidden)

## Label Font Customization

### Setting Label Font

```csharp
// Set font family, size, and style
treeMap1.LabelFont = new Font("Segoe UI Semibold", 13F, FontStyle.Bold, GraphicsUnit.Point);

// Alternative: Use system font
treeMap1.LabelFont = new Font("Arial", 12F);

// With multiple font styles
treeMap1.LabelFont = new Font("Verdana", 11F, FontStyle.Bold | FontStyle.Italic);
```

### Font Selection Guidelines

**Readability:**
- Use sans-serif fonts (Segoe UI, Arial, Verdana) for screen display
- Minimum 9pt font size for readability
- 11-13pt recommended for most TreeMaps

**Font styles:**
- `FontStyle.Regular` - Standard weight
- `FontStyle.Bold` - Emphasized labels
- `FontStyle.Italic` - Subtle emphasis
- `FontStyle.Bold | FontStyle.Italic` - Combined styles

## Label Color Customization

### Label Text Color (LabelBrush)

```csharp
// Solid color
treeMap1.LabelBrush = new SolidBrush(Color.White);

// Dark labels for light backgrounds
treeMap1.LabelBrush = new SolidBrush(Color.Black);

// Custom RGB color
treeMap1.LabelBrush = new SolidBrush(Color.FromArgb(50, 50, 50));

// Hex color
treeMap1.LabelBrush = new SolidBrush(ColorTranslator.FromHtml("#333333"));
```

**Contrast considerations:**
- Light labels (white/light gray) on dark rectangles
- Dark labels (black/dark gray) on light rectangles
- Ensure sufficient contrast ratio for accessibility (4.5:1 minimum)

## Label Background Customization

### LabelBackgroundBrush

```csharp
// Solid background
treeMap1.LabelBackgroundBrush = new SolidBrush(Color.LightSkyBlue);

// Semi-transparent background
treeMap1.LabelBackgroundBrush = new SolidBrush(Color.FromArgb(180, Color.White));

// Match a specific color scheme
treeMap1.LabelBackgroundBrush = new SolidBrush(ColorTranslator.FromHtml("#F0F0F0"));
```

**Use cases:**
- Improve label readability on varied rectangle colors
- Create consistent label appearance
- Add visual hierarchy

## Label Border Customization

### Border Color and Thickness

```csharp
// Border color
treeMap1.LabelBorderBrush = new SolidBrush(Color.White);

// Border thickness (pixels)
treeMap1.LabelBorderThickness = 5;
```

**Visual effects:**
- `Thickness 0`: No border (default)
- `Thickness 1-2`: Subtle outline
- `Thickness 3-5`: Prominent border
- `Thickness 5+`: Heavy emphasis

### Border Patterns

**Subtle separation:**
```csharp
treeMap1.LabelBorderBrush = new SolidBrush(Color.FromArgb(50, Color.Black));
treeMap1.LabelBorderThickness = 1;
```

**Strong definition:**
```csharp
treeMap1.LabelBorderBrush = new SolidBrush(Color.White);
treeMap1.LabelBorderThickness = 3;
```

## Header Customization

### HeaderBrush

Headers appear at each hierarchical level when `HeaderHeight` is set. Customize the header background:

```csharp
// Set header height on level
TreeMapFlatLevel level2 = new TreeMapFlatLevel();
level2.GroupPath = "Country";
level2.ShowLabels = true;
level2.HeaderHeight = 25;  // Header 25 pixels tall
treeMap1.Levels.Add(level2);

// Customize header appearance
treeMap1.HeaderBrush = new SolidBrush(Color.Blue);
```

### HeaderHeight Guidelines

| Height | Use Case |
|--------|----------|
| 0 | No header (default) |
| 15-20 px | Minimal headers for tight layouts |
| 25-30 px | Standard headers, readable labels |
| 35-40 px | Large headers for emphasis |

**Recommendation:** Use 25px for most applications

## Complete Styling Example

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

// Customize headers
treeMap1.HeaderBrush = new SolidBrush(Color.Blue);

// Customize labels
treeMap1.LabelBorderBrush = new SolidBrush(Color.White);
treeMap1.LabelBorderThickness = 5;
treeMap1.LabelBackgroundBrush = new SolidBrush(Color.LightSkyBlue);
treeMap1.LabelBrush = new SolidBrush(Color.White);
treeMap1.LabelFont = new Font("Segoe UI Semibold", 13F, FontStyle.Bold, GraphicsUnit.Point);

// Add color mapping
DesaturationColorMapping desaturationColorMapping = new DesaturationColorMapping();
desaturationColorMapping.Color = Color.OrangeRed;
desaturationColorMapping.From = 220;
desaturationColorMapping.To = 0;
desaturationColorMapping.RangeMinimum = 0;
desaturationColorMapping.RangeMaximum = 80000;
treeMap1.LeafColorMapping = desaturationColorMapping;

// Add to form
treeMap1.Size = new Size(800, 600);
treeMap1.Location = new Point(10, 10);
this.Controls.Add(treeMap1);
```

## Common Styling Patterns

### Modern Minimal Style

```csharp
// Clean, minimal appearance
treeMap1.LabelFont = new Font("Segoe UI", 11F);
treeMap1.LabelBrush = new SolidBrush(Color.FromArgb(60, 60, 60));
treeMap1.LabelBackgroundBrush = new SolidBrush(Color.Transparent);
treeMap1.LabelBorderBrush = new SolidBrush(Color.Transparent);
treeMap1.LabelBorderThickness = 0;
treeMap1.HeaderBrush = new SolidBrush(Color.FromArgb(240, 240, 240));
```

### High Contrast Style

```csharp
// Maximum readability
treeMap1.LabelFont = new Font("Arial", 12F, FontStyle.Bold);
treeMap1.LabelBrush = new SolidBrush(Color.White);
treeMap1.LabelBackgroundBrush = new SolidBrush(Color.Black);
treeMap1.LabelBorderBrush = new SolidBrush(Color.Yellow);
treeMap1.LabelBorderThickness = 2;
treeMap1.HeaderBrush = new SolidBrush(Color.DarkBlue);
```

### Dashboard Style

```csharp
// Professional dashboard appearance
treeMap1.LabelFont = new Font("Segoe UI", 10F, FontStyle.Bold);
treeMap1.LabelBrush = new SolidBrush(Color.White);
treeMap1.LabelBackgroundBrush = new SolidBrush(Color.FromArgb(100, Color.Black));
treeMap1.LabelBorderBrush = new SolidBrush(Color.FromArgb(200, Color.White));
treeMap1.LabelBorderThickness = 1;
treeMap1.HeaderBrush = new SolidBrush(Color.FromArgb(50, 50, 50));
```

### Colorful Style

```csharp
// Vibrant, eye-catching
treeMap1.LabelFont = new Font("Verdana", 11F, FontStyle.Bold);
treeMap1.LabelBrush = new SolidBrush(Color.White);
treeMap1.LabelBackgroundBrush = new SolidBrush(Color.FromArgb(180, Color.DeepPink));
treeMap1.LabelBorderBrush = new SolidBrush(Color.White);
treeMap1.LabelBorderThickness = 3;
treeMap1.HeaderBrush = new SolidBrush(Color.Purple);
```

## Dynamic Label Styling

### Responsive Font Sizing

```csharp
// Adjust font based on TreeMap size
private void UpdateLabelFont()
{
    float fontSize = Math.Max(8, treeMap1.Width / 80);
    treeMap1.LabelFont = new Font("Segoe UI", fontSize, FontStyle.Regular);
}

// Call on resize
protected override void OnResize(EventArgs e)
{
    base.OnResize(e);
    UpdateLabelFont();
}
```

### Theme-Based Styling

```csharp
private void ApplyTheme(string theme)
{
    switch (theme.ToLower())
    {
        case "dark":
            treeMap1.BackColor = Color.FromArgb(30, 30, 30);
            treeMap1.LabelBrush = new SolidBrush(Color.White);
            treeMap1.HeaderBrush = new SolidBrush(Color.FromArgb(50, 50, 50));
            break;
            
        case "light":
            treeMap1.BackColor = Color.White;
            treeMap1.LabelBrush = new SolidBrush(Color.Black);
            treeMap1.HeaderBrush = new SolidBrush(Color.FromArgb(240, 240, 240));
            break;
            
        case "blue":
            treeMap1.BackColor = Color.AliceBlue;
            treeMap1.LabelBrush = new SolidBrush(Color.DarkBlue);
            treeMap1.HeaderBrush = new SolidBrush(Color.SteelBlue);
            break;
    }
}
```

## Best Practices

### Readability First

1. **Ensure contrast:** Labels must be readable against rectangle colors
2. **Size appropriately:** Font size should match rectangle size
3. **Avoid clutter:** Don't show labels if rectangles are too small
4. **Test with data:** Verify labels fit with real data

### Visual Hierarchy

1. **Differentiate levels:** Use HeaderHeight to distinguish hierarchical levels
2. **Consistent styling:** Keep label appearance consistent across levels
3. **Subtle emphasis:** Use borders and backgrounds sparingly

### Performance

1. **Use solid colors:** Gradient brushes can impact performance
2. **Reasonable border thickness:** Very thick borders (10+px) may affect rendering
3. **Cache fonts:** Create Font objects once, reuse across operations

### Accessibility

1. **High contrast:** Ensure minimum 4.5:1 contrast ratio
2. **Readable fonts:** Use sans-serif fonts at 11pt or larger
3. **No color-only indicators:** Don't rely solely on color; use labels

## Troubleshooting

### Labels Not Showing

**Problem:** Labels are invisible or not displayed

**Solutions:**
1. Set `ShowLabels = true` on TreeMapFlatLevel
2. Verify `LabelBrush` is not transparent
3. Check rectangle size is large enough for labels
4. Ensure font size is reasonable

### Labels Hard to Read

**Problem:** Labels are difficult to read against rectangle colors

**Solutions:**
1. Add `LabelBackgroundBrush` for consistent background
2. Use contrasting `LabelBrush` color (white on dark, dark on light)
3. Add `LabelBorderBrush` and set thickness for outline effect
4. Increase font size with `LabelFont`

### Headers Not Appearing

**Problem:** Headers are missing

**Solutions:**
1. Set `HeaderHeight > 0` on TreeMapFlatLevel
2. Verify `HeaderBrush` is not transparent
3. Ensure level has `GroupPath` configured

### Labels Truncated

**Problem:** Label text is cut off

**Solutions:**
1. Increase rectangle size (adjust data or layout)
2. Reduce font size
3. Use shorter labels or abbreviations
4. Consider showing labels only on larger rectangles

---

**Key Takeaway:** Effective label and header customization improves TreeMap readability. Prioritize contrast and appropriate sizing while maintaining visual consistency across hierarchical levels.