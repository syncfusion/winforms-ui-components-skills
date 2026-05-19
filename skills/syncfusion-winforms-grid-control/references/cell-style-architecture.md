# Cell Style Architecture

## Table of Contents
- [Overview](#overview)
- [GridStyleInfo Object](#gridstyleinfo-object)
- [Key Properties](#key-properties)
  - [BackColor and TextColor](#backcolor-and-textcolor)
  - [Font](#font)
  - [CellValue and Text](#cellvalue-and-text)
  - [Borders](#borders)
  - [Interior (Gradients and Patterns)](#interior-gradients-and-patterns)
  - [Format](#format)
  - [Alignment](#alignment)
  - [CellTipText](#celltiptext)
- [Accessing and Modifying Styles](#accessing-and-modifying-styles)
  - [Through Designer](#through-designer)
  - [Through Code](#through-code)
  - [Using ChangeCells Method](#using-changecells-method)
- [GridRangeInfo](#gridrangeinfo)
- [BaseStyles](#basestyles)
  - [Standard BaseStyle](#standard-basestyle)
  - [Header BaseStyles](#header-basestyles)
  - [Custom BaseStyles](#custom-basestyles)
- [Best Practices](#best-practices)

## Overview

GridControl uses a cell-oriented architecture where each cell stores its own style information independently. This architecture is powered by the `GridStyleInfo` class, which encapsulates all appearance and behavior properties for a grid cell.

**Key Concept:** Every cell contains a `GridStyleInfo` object that stores:
- Visual properties (colors, fonts, borders)
- Content properties (cell value, cell type, format)
- Behavior properties (read-only, enabled, validation)

This architecture allows complete customization down to the individual cell level while maintaining efficient memory usage through style inheritance and base styles.

## GridStyleInfo Object

`GridStyleInfo` is the central class for all cell styling in GridControl. It provides comprehensive control over cell appearance and behavior.

### Accessing GridStyleInfo:

```csharp
// Direct access to cell's style
GridStyleInfo style = gridControl1[row, col];
GridStyleInfo style2 = gridControl1.Model[row, col];

// Both approaches access the same GridStyleInfo object
```

### Creating New Style Objects:

```csharp
// Create a new style
GridStyleInfo newStyle = new GridStyleInfo();
newStyle.BackColor = Color.LightBlue;
newStyle.TextColor = Color.DarkBlue;
newStyle.Font.Bold = true;

// Apply to cell
gridControl1.ChangeCells(GridRangeInfo.Cell(2, 3), newStyle);
```

## Key Properties

### BackColor and TextColor

Control cell background and text colors:

```csharp
// Set background color
gridControl1[2, 2].BackColor = Color.LightSkyBlue;
gridControl1[3, 3].BackColor = Color.LightCoral;
gridControl1[4, 4].BackColor = Color.LightPink;

// Set text color
gridControl1[2, 2].TextColor = Color.DarkBlue;
gridControl1[3, 3].TextColor = Color.DarkRed;

// Combine both
gridControl1[5, 5].BackColor = Color.Yellow;
gridControl1[5, 5].TextColor = Color.Red;
```

### Font

`GridFontInfo` class provides font customization. It wraps the standard `System.Drawing.Font` class with grid-specific enhancements.

```csharp
// Set font size
GridFontInfo fontSize = new GridFontInfo();
fontSize.Size = 11f;
gridControl1[2, 2].Font = fontSize;

// Font styles
gridControl1[2, 3].Font.Bold = true;
gridControl1[2, 3].Font.Italic = true;
gridControl1[3, 2].Font.Underline = true;
gridControl1[3, 3].Font.Strikeout = true;

// Font family
gridControl1[4, 2].Font.Facename = "Arial";
gridControl1[4, 2].Font.Size = 12f;

// Font orientation (rotation)
gridControl1[5, 2].Text = "Rotated Text";
gridControl1[5, 2].Font.Orientation = 45;  // 45-degree angle
```

**Orientation values:**
- 0 = Horizontal (default)
- 90 = Vertical (bottom to top)
- 180 = Upside down
- 270 = Vertical (top to bottom)
- Any angle 0-360

### CellValue and Text

Two closely related properties for setting cell content:

**CellValue** (object type) - Stores actual data
**Text** (string type) - String representation

```csharp
// Using Text (string)
gridControl1[2, 2].Text = "Essential Grid";

// Using CellValue (object)
gridControl1[3, 2].CellValue = "Essential Grid";
gridControl1[4, 2].CellValue = 12345;
gridControl1[5, 2].CellValue = DateTime.Now;
gridControl1[6, 2].CellValue = true;

// CellValue is more flexible for typed data
```

**When to use each:**
- Use `CellValue` for typed data (numbers, dates, booleans)
- Use `Text` for simple string assignments
- `CellValue` respects the Format property
- `Text` returns formatted string

### Borders

Configure cell borders with `GridBorder` class:

```csharp
// Set all borders
gridControl1[2, 2].Borders.All = new GridBorder(GridBorderStyle.Solid, Color.Red);

// Individual borders
gridControl1[3, 3].Borders.Top = new GridBorder(GridBorderStyle.Solid, Color.Blue, GridBorderWeight.Thick);
gridControl1[3, 3].Borders.Bottom = new GridBorder(GridBorderStyle.Solid, Color.Blue, GridBorderWeight.Thick);

// Border styles
gridControl1[4, 4].Borders.All = new GridBorder(GridBorderStyle.Dashed, Color.Green);
gridControl1[5, 5].Borders.All = new GridBorder(GridBorderStyle.Dotted, Color.Purple);

// No borders
gridControl1[6, 6].Borders.All = GridBorder.Empty;
```

**Border styles:**
- `Solid` - Standard solid line
- `Dashed` - Dashed line
- `Dotted` - Dotted line
- `DashDot` - Alternating dash and dot
- `DashDotDot` - Dash followed by two dots

**Border weights:**
- `Thin` - 1 pixel
- `Medium` - 2 pixels
- `Thick` - 3 pixels

### Interior (Gradients and Patterns)

Create gradient or pattern backgrounds with `BrushInfo`:

```csharp
// Gradient backgrounds
gridControl1[2, 2].Interior = new BrushInfo(GradientStyle.Horizontal, Color.Yellow, Color.Blue);
gridControl1[3, 2].Interior = new BrushInfo(GradientStyle.Vertical, Color.Red, Color.White);
gridControl1[4, 2].Interior = new BrushInfo(GradientStyle.ForwardDiagonal, Color.Green, Color.LightGreen);

// Pattern backgrounds
gridControl1[5, 2].Interior = new BrushInfo(PatternStyle.DashedHorizontal, Color.Black, Color.White);
gridControl1[6, 2].Interior = new BrushInfo(PatternStyle.DiagonalCross, Color.Navy, Color.LightBlue);
```

**Gradient styles:**
- `Horizontal` - Left to right
- `Vertical` - Top to bottom
- `ForwardDiagonal` - Top-left to bottom-right
- `BackwardDiagonal` - Top-right to bottom-left
- `PathRectangle` - Rectangle path
- `PathEllipse` - Ellipse path

**Pattern styles:**
- `DashedHorizontal` / `DashedVertical`
- `DiagonalCross`
- `Dots`
- And many more...

### Format

Apply number, date, and currency formatting:

```csharp
// Currency format
gridControl1[2, 2].CellValue = 31456;
gridControl1[2, 2].Format = "C";  // $31,456.00

// Number with decimals
gridControl1[3, 2].CellValue = 123.456;
gridControl1[3, 2].Format = "N2";  // 123.46

// Percentage
gridControl1[4, 2].CellValue = 0.75;
gridControl1[4, 2].Format = "P";  // 75.00%

// Date format
gridControl1[5, 2].CellValue = DateTime.Now;
gridControl1[5, 2].Format = "d";  // Short date

// Custom format
gridControl1[6, 2].CellValue = 1234567;
gridControl1[6, 2].Format = "###-###-####";  // 123-456-7
```

**Common format specifiers:**
- `C` - Currency
- `N` - Number
- `P` - Percent
- `d` - Short date
- `D` - Long date
- `t` - Short time
- `T` - Long time
- Custom patterns

### Alignment

Control text alignment within cells:

```csharp
// Horizontal alignment
gridControl1[2, 2].HorizontalAlignment = GridHorizontalAlignment.Left;
gridControl1[3, 2].HorizontalAlignment = GridHorizontalAlignment.Center;
gridControl1[4, 2].HorizontalAlignment = GridHorizontalAlignment.Right;

// Vertical alignment
gridControl1[2, 3].VerticalAlignment = GridVerticalAlignment.Top;
gridControl1[3, 3].VerticalAlignment = GridVerticalAlignment.Middle;
gridControl1[4, 3].VerticalAlignment = GridVerticalAlignment.Bottom;

// Combined
gridControl1[5, 5].HorizontalAlignment = GridHorizontalAlignment.Center;
gridControl1[5, 5].VerticalAlignment = GridVerticalAlignment.Middle;
```

### CellTipText

Add tooltips to cells:

```csharp
// Simple tooltip
gridControl1[2, 2].CellTipText = "This is a tooltip";

// Dynamic tooltips
for (int row = 1; row <= 10; row++)
{
    gridControl1[row, 1].CellTipText = $"Row {row} information";
}

// Detailed tooltip
gridControl1[5, 5].CellTipText = "Click to edit\nPress F2 for full edit mode\nEsc to cancel";
```

## Accessing and Modifying Styles

### Through Designer

Visual Studio designer provides two editing modes:

**1. Grid Properties Tab:**
- Applies to entire grid
- Changes default styles
- Affects all cells unless overridden

**2. Selected Range Tab:**
- Select cells in designer (click Edit button)
- Modify properties in property grid
- Changes apply only to selected cells

**Steps:**
1. Select GridControl in designer
2. Click "Edit" in smart tag or properties
3. Select cells to modify
4. Use Property Grid to change styles
5. Switch between "Grid Properties" and "Selected Range" tabs

### Through Code

Direct style modification:

```csharp
// Modify single cell
gridControl1[2, 2].BackColor = Color.Yellow;
gridControl1[2, 2].TextColor = Color.Red;
gridControl1[2, 2].Font.Bold = true;

// Multiple properties
gridControl1[3, 3].BackColor = Color.LightBlue;
gridControl1[3, 3].TextColor = Color.DarkBlue;
gridControl1[3, 3].CellValue = "Styled Cell";
gridControl1[3, 3].HorizontalAlignment = GridHorizontalAlignment.Center;
```

### Using ChangeCells Method

Apply styles to ranges efficiently:

```csharp
// Create style
GridStyleInfo style = new GridStyleInfo();
style.BackColor = Color.DarkGreen;
style.TextColor = Color.White;
style.Font.Facename = "Verdana";
style.Font.Bold = true;
style.Font.Size = 9f;

// Apply to range
gridControl1.ChangeCells(GridRangeInfo.Cells(2, 2, 4, 2), style);

// Apply to row
gridControl1.ChangeCells(GridRangeInfo.Row(5), style);

// Apply to column
gridControl1.ChangeCells(GridRangeInfo.Col(3), style);
```

## GridRangeInfo

`GridRangeInfo` defines cell ranges for batch operations.

### Creating Ranges:

```csharp
// Single cell
GridRangeInfo cellRange = GridRangeInfo.Cell(2, 3);

// Rectangle range (top-left to bottom-right)
GridRangeInfo rectRange = GridRangeInfo.Cells(2, 2, 5, 5);

// Entire row
GridRangeInfo rowRange = GridRangeInfo.Row(3);

// Entire column
GridRangeInfo colRange = GridRangeInfo.Col(4);

// Multiple rows
GridRangeInfo rowsRange = GridRangeInfo.Rows(2, 5);  // Rows 2-5

// Multiple columns
GridRangeInfo colsRange = GridRangeInfo.Cols(2, 5);  // Cols 2-5

// Entire table
GridRangeInfo tableRange = GridRangeInfo.Table();
```

### Using with ChangeCells:

```csharp
GridStyleInfo headerStyle = new GridStyleInfo
{
    BackColor = Color.Navy,
    TextColor = Color.White,
    Font = { Bold = true }
};

// Style header row
gridControl1.ChangeCells(GridRangeInfo.Row(1), headerStyle);

// Style data range
GridStyleInfo dataStyle = new GridStyleInfo { BackColor = Color.WhiteSmoke };
gridControl1.ChangeCells(GridRangeInfo.Cells(2, 1, 10, 5), dataStyle);
```

## BaseStyles

BaseStyles are reusable style templates that can be applied to multiple cells.

### Standard BaseStyle

Affects all data cells (excludes headers):

```csharp
gridControl1.BaseStylesMap["Standard"].StyleInfo.BackColor = Color.Aqua;
gridControl1.BaseStylesMap["Standard"].StyleInfo.Font.Size = 10f;
```

### Header BaseStyles

**All Headers:**
```csharp
gridControl1.BaseStylesMap["Header"].StyleInfo.TextColor = Color.Red;
gridControl1.BaseStylesMap["Header"].StyleInfo.Font.Bold = true;
```

**Column Headers Only:**
```csharp
gridControl1.BaseStylesMap["Column Header"].StyleInfo.BackColor = Color.SteelBlue;
gridControl1.BaseStylesMap["Column Header"].StyleInfo.TextColor = Color.White;
```

**Row Headers Only:**
```csharp
gridControl1.BaseStylesMap["Row Header"].StyleInfo.BackColor = Color.LightGray;
gridControl1.BaseStylesMap["Row Header"].StyleInfo.TextColor = Color.Black;
```

### Custom BaseStyles

Create and apply custom reusable styles:

```csharp
// Create custom base style
GridBaseStyle highlightStyle = new GridBaseStyle("Highlight", false);
highlightStyle.StyleInfo.BackColor = Color.Yellow;
highlightStyle.StyleInfo.TextColor = Color.Red;
highlightStyle.StyleInfo.Font.Bold = true;

// Add to grid
gridControl1.BaseStylesMap.Add(highlightStyle);

// Apply to cells
gridControl1[2, 2].BaseStyle = "Highlight";
gridControl1[3, 3].BaseStyle = "Highlight";
gridControl1[4, 4].BaseStyle = "Highlight";

// Create error style
GridBaseStyle errorStyle = new GridBaseStyle("Error", false);
errorStyle.StyleInfo.BackColor = Color.LightPink;
errorStyle.StyleInfo.TextColor = Color.DarkRed;
errorStyle.StyleInfo.Borders.All = new GridBorder(GridBorderStyle.Solid, Color.Red, GridBorderWeight.Thick);

gridControl1.BaseStylesMap.Add(errorStyle);

// Apply conditionally
if (IsInvalidData(row, col))
{
    gridControl1[row, col].BaseStyle = "Error";
}
```

## Best Practices

### Performance

**Use BeginUpdate/EndUpdate for batch styling:**
```csharp
gridControl1.BeginUpdate();

for (int row = 1; row <= 100; row++)
{
    gridControl1[row, 1].BackColor = row % 2 == 0 ? Color.White : Color.WhiteSmoke;
}

gridControl1.EndUpdate();
```

**Use BaseStyles for repeated patterns:**
```csharp
// Instead of setting style for each cell
// Create a base style and reference it
GridBaseStyle alternateRow = new GridBaseStyle("AlternateRow", false);
alternateRow.StyleInfo.BackColor = Color.WhiteSmoke;
gridControl1.BaseStylesMap.Add(alternateRow);

for (int row = 1; row <= 100; row += 2)
{
    gridControl1[row, GridRangeInfo.Table().Left].BaseStyle = "AlternateRow";
}
```

### Maintainability

**Centralize style definitions:**
```csharp
private void InitializeStyles()
{
    // Define all styles in one place
    headerStyle = new GridStyleInfo
    {
        BackColor = Color.Navy,
        TextColor = Color.White,
        Font = { Bold = true, Size = 10f }
    };
    
    dataStyle = new GridStyleInfo
    {
        BackColor = Color.White,
        Font = { Size = 9f }
    };
    
    errorStyle = new GridStyleInfo
    {
        BackColor = Color.LightPink,
        TextColor = Color.DarkRed
    };
}

private void ApplyStyles()
{
    gridControl1.ChangeCells(GridRangeInfo.Row(1), headerStyle);
    // ... apply other styles
}
```

### Conditional Formatting

```csharp
// Apply styles based on cell values
private void ApplyConditionalFormatting()
{
    for (int row = 2; row <= gridControl1.RowCount; row++)
    {
        var value = Convert.ToDouble(gridControl1[row, 3].CellValue);
        
        if (value < 0)
        {
            gridControl1[row, 3].TextColor = Color.Red;
            gridControl1[row, 3].Font.Bold = true;
        }
        else if (value > 1000)
        {
            gridControl1[row, 3].BackColor = Color.LightGreen;
        }
    }
}
```

### Style Inheritance

Understand the style priority order:
1. Cell-level styles (highest priority)
2. BaseStyle reference
3. Column/Row styles
4. Default grid style (lowest priority)

```csharp
// Set default
gridControl1.TableStyle.BackColor = Color.White;

// Override for specific cells
gridControl1[2, 2].BackColor = Color.Yellow;  // This wins
```

## Troubleshooting

### Styles Not Applying
- Check if cell has a BaseStyle that overrides your changes
- Verify BeginUpdate has matching EndUpdate
- Use Refresh() after style changes if needed

### Performance Issues
- Use QueryCellInfo event for large grids
- Implement BaseStyles instead of per-cell styling
- Use BeginUpdate/EndUpdate for batch operations

### Styles Disappearing
- Styles may be overridden by BaseStyle
- Check for code that resets styles
- Verify grid isn't being recreated

## Next Steps

After mastering cell styling:
1. Explore cell types for different data inputs
2. Implement conditional formatting
3. Create custom cell renderers
4. Learn about themes and visual styles
5. Optimize performance with virtual mode
