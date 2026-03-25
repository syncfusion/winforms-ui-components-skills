# Appearance and Formatting in GridGroupingControl

## Table of Contents
- [Overview](#overview)
- [Table Level Appearance](#table-level-appearance)
- [Group Level Appearance](#group-level-appearance)
- [Column-Based Formatting](#column-based-formatting)
- [BaseStyles](#basestyles)
- [Conditional Formatting](#conditional-formatting)
- [Dynamic Formatting](#dynamic-formatting)
- [Common Scenarios](#common-scenarios)

## Overview

GridGroupingControl appearance is controlled through the `Appearance` property, which contains `GridTableCellAppearance` properties for different grid elements. Each appearance property holds `GridStyleInfo` settings for back color, font, cell type, and more.

**Key appearance properties:**
- `AnyCell` - Applies to all cells
- `AnyRecordFieldCell` - All record cells
- `AlternateRecordFieldCell` - Alternate row cells
- `ColumnHeaderCell` - Column headers
- `GroupCaptionCell` - Group captions
- And 20+ more specific cell types

## Basic Formatting

### Apply to All Cells

```csharp
// Light green background for all cells
gridGroupingControl1.Appearance.AnyCell.BackColor = Color.LightGreen;
```

### Apply to Record Cells

```csharp
// Light pink background for all records
gridGroupingControl1.Appearance.AnyRecordFieldCell.BackColor = Color.LightPink;
```

### Alternate Row Colors

```csharp
// Light salmon for alternate rows
gridGroupingControl1.Appearance.AlternateRecordFieldCell.BackColor = Color.LightSalmon;
```

## Table Level Appearance

### Parent Table Customization

```csharp
// Column headers with gradient
gridGroupingControl1.Appearance.ColumnHeaderCell.Interior = 
    new BrushInfo(GradientStyle.Vertical, 
        Color.FromArgb(214, 220, 232), 
        Color.FromArgb(106, 111, 151));
gridGroupingControl1.Appearance.ColumnHeaderCell.TextColor = Color.White;

// Record field cells
gridGroupingControl1.Appearance.RecordFieldCell.Interior = 
    new BrushInfo(Color.Lavender);

// Row headers
gridGroupingControl1.Appearance.RowHeaderCell.Interior = 
    new BrushInfo(GradientStyle.Horizontal, 
        SystemColors.Window, 
        Color.FromArgb(206, 213, 231));
gridGroupingControl1.Appearance.RowHeaderCell.Themed = false;

// Top-left corner cell
gridGroupingControl1.Appearance.TopLeftHeaderCell.Interior = 
    new BrushInfo(GradientStyle.PathRectangle, 
        SystemColors.Window, 
        Color.FromArgb(255, 228, 221));
```

### Child Table Customization

```csharp
// Get child table descriptor
GridTableDescriptor childTable = gridGroupingControl1.GetTableDescriptor("Orders");

// Record cells with alternating colors
childTable.Appearance.AnyRecordFieldCell.BackColor = Color.FromArgb(223, 247, 252);
childTable.Appearance.AlternateRecordFieldCell.BackColor = Color.FromArgb(255, 229, 201);

// Column headers
childTable.Appearance.ColumnHeaderCell.Interior = 
    new BrushInfo(GradientStyle.Vertical, 
        Color.FromArgb(203, 201, 202), 
        Color.FromArgb(253, 247, 215));
childTable.Appearance.ColumnHeaderCell.TextColor = Color.Black;

// Group captions
childTable.Appearance.GroupCaptionCell.Interior = 
    new BrushInfo(Color.FromArgb(255, 238, 220));
childTable.Appearance.GroupCaptionCell.Borders.Bottom = 
    new GridBorder(GridBorderStyle.Solid, 
        Color.FromArgb(242, 158, 32), 
        GridBorderWeight.Medium);
```

## Group Level Appearance

Customize group elements:

```csharp
// All group cells (black background)
gridGroupingControl1.Appearance.AnyGroupCell.Interior = new BrushInfo(Color.Black);
gridGroupingControl1.Appearance.AnyGroupCell.Themed = false;

// Group caption borders
gridGroupingControl1.Appearance.GroupCaptionCell.Borders.Bottom = 
    new GridBorder(GridBorderStyle.Solid, Color.FromArgb(157, 179, 200));

// Group caption row header
gridGroupingControl1.Appearance.GroupCaptionRowHeaderCell.Interior = 
    new BrushInfo(GradientStyle.BackwardDiagonal, 
        SystemColors.Window, 
        Color.DarkOrange);
```

**Group-specific properties:**
- `GroupCaptionCell` - Caption bar of group
- `GroupCaptionPlusMinusCell` - Expand/collapse icon
- `GroupHeaderSectionCell` - Group header section
- `GroupFooterSectionCell` - Group footer section
- `GroupIndentCell` - Indentation cells
- `GroupPreviewCell` - Preview row cells

## Column-Based Formatting

### Through Designer

1. Select `TableDescriptor.Columns` in Properties window
2. Opens `GridColumnDescriptor` collection editor
3. Set `Appearance` properties for desired column

### Through Code

```csharp
// Method 1: Modify existing column
gridGroupingControl1.TableDescriptor.Columns["EmployeeID"]
    .Appearance.AnyRecordFieldCell.Interior = 
    new BrushInfo(Color.FromArgb(237, 240, 246));

// Method 2: Create descriptors
GridColumnDescriptor desc1 = new GridColumnDescriptor();
desc1.MappingName = "EmployeeID";
desc1.Appearance.AnyRecordFieldCell.Interior = 
    new BrushInfo(Color.FromArgb(237, 240, 246));

GridColumnDescriptor desc2 = new GridColumnDescriptor();
desc2.MappingName = "LastName";
desc2.Appearance.AnyRecordFieldCell.Interior = 
    new BrushInfo(Color.FromArgb(218, 229, 245));
desc2.Appearance.AnyRecordFieldCell.Font.Bold = true;

GridColumnDescriptor desc3 = new GridColumnDescriptor();
desc3.MappingName = "FirstName";
desc3.Appearance.AnyRecordFieldCell.Interior = 
    new BrushInfo(Color.FromArgb(102, 110, 152));
desc3.Appearance.AnyRecordFieldCell.TextColor = Color.White;
```

### Header Images

```csharp
// Add image to column header
gridGroupingControl1.TableDescriptor.Columns["Title"].HeaderImage = 
    Image.FromFile(@"Images\Contact.PNG");

// Align header image
gridGroupingControl1.TableDescriptor.Columns["Title"].HeaderImageAlignment = 
    HeaderImageAlignment.Right;
```

## BaseStyles

Reusable style templates for consistent formatting.

### Through Designer

1. Access `BaseStyles` in property editor
2. Opens `GridTableStyle Collection Editor`
3. Configure `StyleInfo` properties
4. Assign to cells via `Appearance.*.BaseStyle` property

### Through Code

```csharp
// Create base style
GridTableBaseStyle style1 = new GridTableBaseStyle("CustomStyle");
style1.Name = "CustomStyle";
style1.StyleInfo.Font.Facename = "Verdana";
style1.StyleInfo.Font.Bold = true;
style1.StyleInfo.BackColor = Color.LightBlue;
style1.StyleInfo.TextColor = Color.DarkBlue;

// Register style
gridGroupingControl1.BaseStyles.Add(style1);

// Apply to cells
gridGroupingControl1.Appearance.AlternateRecordFieldCell.BaseStyle = "CustomStyle";

// Can apply to specific columns
gridGroupingControl1.TableDescriptor.Columns["ProductName"]
    .Appearance.AnyRecordFieldCell.BaseStyle = "CustomStyle";
```

## Conditional Formatting

Apply styles based on data conditions using expressions or filters.

### Using Expression

```csharp
// Format 1: CustomerID starts with 'A'
GridConditionalFormatDescriptor format1 = new GridConditionalFormatDescriptor();
format1.Appearance.AnyRecordFieldCell.Interior = 
    new BrushInfo(Color.FromArgb(255, 191, 52));
format1.Appearance.AnyRecordFieldCell.TextColor = Color.White;
format1.Expression = "[CustomerID] LIKE 'A*'";
format1.Name = "ConditionalFormat1";

// Format 2: ContactTitle = 'Sales Representative'
GridConditionalFormatDescriptor format2 = new GridConditionalFormatDescriptor();
format2.Appearance.AnyRecordFieldCell.Font.Bold = true;
format2.Appearance.AnyRecordFieldCell.Interior = 
    new BrushInfo(Color.FromArgb(102, 110, 152));
format2.Appearance.AnyRecordFieldCell.TextColor = Color.White;
format2.Expression = "[ContactTitle] LIKE 'Sales Representative'";
format2.Name = "ConditionalFormat2";

// Add to grid
gridGroupingControl1.TableDescriptor.ConditionalFormats.Add(format1);
gridGroupingControl1.TableDescriptor.ConditionalFormats.Add(format2);
```

### Using Record Filters

```csharp
GridConditionalFormatDescriptor format = new GridConditionalFormatDescriptor();

// Define filter
FilterCondition condition = new FilterCondition(FilterCompareOperator.GreaterThan, 1000);
RecordFilterDescriptor filter = new RecordFilterDescriptor("UnitPrice", condition);
format.RecordFilters.Add(filter);

// Define style
format.Appearance.AnyRecordFieldCell.BackColor = Color.LightCoral;
format.Appearance.AnyRecordFieldCell.Font.Bold = true;

gridGroupingControl1.TableDescriptor.ConditionalFormats.Add(format);
```

## Dynamic Formatting

Apply styles at runtime using `QueryCellStyleInfo` event:

```csharp
gridGroupingControl1.QueryCellStyleInfo += (s, e) =>
{
    // Format based on cell type
    if (e.TableCellIdentity.TableCellType == GridTableCellType.RecordFieldCell)
    {
        if (e.TableCellIdentity.ColIndex % 2 == 0)
        {
            e.Style.BackColor = Color.FromArgb(255, 187, 111);
            e.Style.Font.FontStyle = FontStyle.Bold;
        }
        else
        {
            e.Style.TextColor = Color.White;
            e.Style.BackColor = Color.FromArgb(55, 91, 114);
        }
    }
    else if (e.TableCellIdentity.TableCellType == GridTableCellType.AlternateRecordFieldCell)
    {
        if (e.TableCellIdentity.ColIndex % 2 == 0)
        {
            e.Style.Font.FontStyle = FontStyle.Underline;
            e.Style.BackColor = Color.FromArgb(0, 21, 84);
            e.Style.TextColor = Color.White;
        }
        else
        {
            e.Style.BackColor = Color.FromArgb(255, 188, 112);
            e.Style.Font.FontStyle = FontStyle.Italic;
        }
    }
};
```

### Format Based on Cell Value

```csharp
gridGroupingControl1.QueryCellStyleInfo += (s, e) =>
{
    if (e.TableCellIdentity.TableCellType == GridTableCellType.RecordFieldCell &&
        e.TableCellIdentity.Column != null &&
        e.TableCellIdentity.Column.Name == "UnitPrice")
    {
        decimal price;
        if (decimal.TryParse(e.Style.Text, out price))
        {
            if (price > 100)
            {
                e.Style.BackColor = Color.LightCoral;
                e.Style.Font.Bold = true;
            }
            else if (price < 10)
            {
                e.Style.BackColor = Color.LightGreen;
            }
        }
    }
};
```

## Table Options

Control grid layout and behavior:

```csharp
// Caption and preview
gridGroupingControl1.TopLevelGroupOptions.ShowCaption = true;
gridGroupingControl1.TableOptions.RecordPreviewRowHeight = 55;

// Headers and selection
gridGroupingControl1.TableOptions.ShowRowHeader = false;
gridGroupingControl1.TableOptions.SelectionBackColor = Color.FromArgb(255, 128, 128);
gridGroupingControl1.TableOptions.SelectionTextColor = Color.Maroon;
gridGroupingControl1.TableOptions.ListBoxSelectionMode = SelectionMode.MultiExtended;

// Sizing
gridGroupingControl1.TableOptions.DefaultColumnWidth = 65;
gridGroupingControl1.TableOptions.CaptionRowHeight = 22;
```

## FormatCell Dialog

Excel-like format dialog:

```csharp
using Syncfusion.GridHelperClasses;

// Show format dialog
GroupingGridFormatCellDialog dialog = 
    new GroupingGridFormatCellDialog(gridGroupingControl1);
dialog.ShowDialog();
```

Dialog allows formatting:
- Font (face, size, style)
- Number format
- Background colors
- Cell alignment

## Common Scenarios

### Scenario 1: Zebra Striping

```csharp
gridGroupingControl1.Appearance.AnyRecordFieldCell.BackColor = Color.White;
gridGroupingControl1.Appearance.AlternateRecordFieldCell.BackColor = 
    Color.FromArgb(240, 240, 240);
```

### Scenario 2: Excel-Like Theme

```csharp
// Headers
gridGroupingControl1.Appearance.ColumnHeaderCell.BackColor = Color.FromArgb(217, 217, 217);
gridGroupingControl1.Appearance.ColumnHeaderCell.Font.Bold = true;
gridGroupingControl1.Appearance.ColumnHeaderCell.Borders.All = 
    new GridBorder(GridBorderStyle.Solid, Color.FromArgb(189, 189, 189));

// Records
gridGroupingControl1.Appearance.AnyRecordFieldCell.Borders.All = 
    new GridBorder(GridBorderStyle.Solid, Color.FromArgb(217, 217, 217));

// Gridlines
gridGroupingControl1.TableControl.Properties.GridLineColor = Color.FromArgb(217, 217, 217);
```

### Scenario 3: Heat Map (Value-Based Colors)

```csharp
gridGroupingControl1.QueryCellStyleInfo += (s, e) =>
{
    if (e.TableCellIdentity.Column?.Name == "Sales")
    {
        double sales;
        if (double.TryParse(e.Style.Text, out sales))
        {
            // Color gradient: green (low) to red (high)
            int intensity = (int)((sales / 10000) * 255);
            intensity = Math.Min(255, Math.Max(0, intensity));
            
            e.Style.BackColor = Color.FromArgb(255, 255 - intensity, 255 - intensity);
            e.Style.TextColor = intensity > 128 ? Color.White : Color.Black;
        }
    }
};
```

### Scenario 4: Highlight Current Row

```csharp
private int currentRow = -1;

gridGroupingControl1.TableControl.CurrentCellMoved += (s, e) =>
{
    currentRow = gridGroupingControl1.TableControl.CurrentCell.RowIndex;
    gridGroupingControl1.TableControl.Refresh();
};

gridGroupingControl1.QueryCellStyleInfo += (s, e) =>
{
    if (e.TableCellIdentity.RowIndex == currentRow && 
        e.TableCellIdentity.TableCellType == GridTableCellType.RecordFieldCell)
    {
        e.Style.BackColor = Color.LightYellow;
    }
};
```

### Scenario 5: Icon-Based Status

```csharp
gridGroupingControl1.QueryCellStyleInfo += (s, e) =>
{
    if (e.TableCellIdentity.Column?.Name == "Status")
    {
        string status = e.Style.Text;
        
        switch (status)
        {
            case "Active":
                e.Style.BackColor = Color.LightGreen;
                e.Style.Text = "✓ " + status;
                break;
            case "Pending":
                e.Style.BackColor = Color.LightYellow;
                e.Style.Text = "⌛ " + status;
                break;
            case "Inactive":
                e.Style.BackColor = Color.LightCoral;
                e.Style.Text = "✗ " + status;
                break;
        }
    }
};
```

### Scenario 6: Gradient Column Headers

```csharp
for (int i = 0; i < gridGroupingControl1.TableDescriptor.Columns.Count; i++)
{
    int hue = (i * 30) % 360; // Rotate through color wheel
    Color startColor = ColorFromHSL(hue, 0.7, 0.8);
    Color endColor = ColorFromHSL(hue, 0.7, 0.6);
    
    gridGroupingControl1.TableDescriptor.Columns[i]
        .Appearance.ColumnHeaderCell.Interior = 
        new BrushInfo(GradientStyle.Vertical, startColor, endColor);
}

// Helper method
private Color ColorFromHSL(double h, double s, double l)
{
    // HSL to RGB conversion (implementation omitted for brevity)
}
```

## Best Practices

1. **Use BaseStyles for reusable themes:** Define once, apply many times
2. **QueryCellStyleInfo for dynamic formatting:** Value-based styling, runtime conditions
3. **Conditional formatting for business rules:** Static conditions known at design time
4. **Disable themes if needed:** Set `ThemesEnabled = false` for custom header styles
5. **Performance:** Minimize QueryCellStyleInfo logic, cache complex calculations
6. **Gradient usage:** Use sparingly for headers/important cells only
7. **Color contrast:** Ensure text is readable on background colors

## Common Issues

### Styles Not Applying to Headers

```csharp
// Disable themes to allow custom header styles
gridGroupingControl1.ThemesEnabled = false;
```

### QueryCellStyleInfo Performance

```csharp
// Cache expensive operations
private Dictionary<int, Color> colorCache = new Dictionary<int, Color>();

gridGroupingControl1.QueryCellStyleInfo += (s, e) =>
{
    int key = e.TableCellIdentity.RowIndex * 1000 + e.TableCellIdentity.ColIndex;
    
    if (!colorCache.ContainsKey(key))
    {
        // Expensive calculation
        colorCache[key] = CalculateColor(e.Style.Text);
    }
    
    e.Style.BackColor = colorCache[key];
};
```

### Conditional Format Not Working

```csharp
// Verify expression syntax
string expression = "[FieldName] > 100"; // Correct
// string expression = "FieldName > 100"; // Missing brackets - won't work

GridConditionalFormatDescriptor format = new GridConditionalFormatDescriptor();
format.Expression = expression;
```
