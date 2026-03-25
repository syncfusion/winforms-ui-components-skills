# Selection

This guide covers selection functionality in GridControl, including range selection, row selection, programmatic selection, and selection events.

## Overview

GridControl provides two types of selection:
1. **Range Selection** - Select cells, rows, columns, or ranges
2. **Record Selection** - Select entire rows (ListBox-style)

## Range Selection

Range selection allows selecting individual cells, ranges, rows, or columns.

### Enabling Range Selection:

```csharp
// Allow any selection type
gridControl1.AllowSelection = GridSelectionFlags.Any;

// Only cell selections
gridControl1.AllowSelection = GridSelectionFlags.Cell;

// Only row selections
gridControl1.AllowSelection = GridSelectionFlags.Row;

// Only column selections
gridControl1.AllowSelection = GridSelectionFlags.Column;

// Disable selection
gridControl1.AllowSelection = GridSelectionFlags.None;
```

### GridSelectionFlags Options:

- `None` - No selection allowed
- `Cell` - Single cell selection
- `Row` - Entire row selection
- `Column` - Entire column selection
- `Table` - Entire table selection
- `Multiple` - Allow multiple selections
- `Shift` - Allow Shift+Click selection
- `Keyboard` - Allow keyboard selection
- `Any` - Allow all selection types (most common)

## Row Selection (ListBox Mode)

Row selection allows selecting entire rows with ListBox-style behavior.

### Enabling Row Selection:

```csharp
// Single row selection
gridControl1.ListBoxSelectionMode = SelectionMode.One;

// Multiple rows with Ctrl/Shift
gridControl1.ListBoxSelectionMode = SelectionMode.MultiExtended;

// Multiple rows with simple click
gridControl1.ListBoxSelectionMode = SelectionMode.MultiSimple;

// Disable row selection (default)
gridControl1.ListBoxSelectionMode = SelectionMode.None;
```

**SelectionMode values:**
- `None` - No row selection (default)
- `One` - Single row only
- `MultiSimple` - Multiple rows, click to toggle
- `MultiExtended` - Multiple rows with Ctrl/Shift keys

### Example:

```csharp
// Enable multi-row selection
gridControl1.ListBoxSelectionMode = SelectionMode.MultiExtended;

// Users can now:
// - Click to select one row
// - Ctrl+Click to select multiple rows
// - Shift+Click to select range of rows
```

## Programmatic Selection

### Adding Selections:

```csharp
// Select a single cell
gridControl1.Selections.Add(GridRangeInfo.Cell(2, 3));

// Select a range of cells
gridControl1.Selections.Add(GridRangeInfo.Cells(2, 2, 5, 4));

// Select entire row
gridControl1.Selections.Add(GridRangeInfo.Row(3));

// Select entire column
gridControl1.Selections.Add(GridRangeInfo.Col(4));

// Select multiple rows
gridControl1.Selections.Add(GridRangeInfo.Rows(2, 5));

// Select multiple columns
gridControl1.Selections.Add(GridRangeInfo.Cols(2, 4));
```

### Multiple Selections:

```csharp
// Add multiple selections
gridControl1.Selections.Add(GridRangeInfo.Cells(3, 3, 3, 4));
gridControl1.Selections.Add(GridRangeInfo.Cols(1, 1));
gridControl1.Selections.Add(GridRangeInfo.Row(5));

// Select non-contiguous ranges
gridControl1.Selections.Add(GridRangeInfo.Cells(2, 2, 4, 4));
gridControl1.Selections.Add(GridRangeInfo.Cells(7, 7, 9, 9));
```

### Clearing Selections:

```csharp
// Clear all selections
gridControl1.Selections.Clear();

// Remove specific selection
gridControl1.Selections.Remove(GridRangeInfo.Row(3));
```

### Getting Selected Ranges:

```csharp
// Get all selected ranges
GridRangeInfoList selectedRanges = gridControl1.Selections.GetSelectedRanges(false, false);

foreach (GridRangeInfo range in selectedRanges)
{
    Console.WriteLine($"Selected: {range}");
    Console.WriteLine($"  Top: {range.Top}, Left: {range.Left}");
    Console.WriteLine($"  Bottom: {range.Bottom}, Right: {range.Right}");
}

// Get selected rows
GridRangeInfoList selectedRows = gridControl1.Selections.GetSelectedRows(false, false);

// Get selected columns
GridRangeInfoList selectedCols = gridControl1.Selections.GetSelectedCols(false, false);
```

## Selection Events

### SelectionChanging Event:

Fired before selection changes. Can be canceled.

```csharp
gridControl1.SelectionChanging += GridControl1_SelectionChanging;

private void GridControl1_SelectionChanging(object sender, GridSelectionChangingEventArgs e)
{
    // Prevent selection in specific area
    if (e.Range.IntersectsWith(GridRangeInfo.Cells(1, 1, 5, 5)))
    {
        MessageBox.Show("Cannot select this area");
        e.Cancel = true;
        return;
    }
    
    Console.WriteLine($"Selection changing: {e.Range}");
}
```

### SelectionChanged Event:

Fired after selection changes.

```csharp
gridControl1.SelectionChanged += GridControl1_SelectionChanged;

private void GridControl1_SelectionChanged(object sender, GridSelectionChangedEventArgs e)
{
    GridRangeInfoList ranges = gridControl1.Selections.GetSelectedRanges(false, false);
    
    Console.WriteLine($"Selection changed. {ranges.Count} range(s) selected");
    
    foreach (GridRangeInfo range in ranges)
    {
        Console.WriteLine($"  Range: {range}");
    }
    
    // Update status bar
    UpdateStatusBar(ranges);
}

private void UpdateStatusBar(GridRangeInfoList ranges)
{
    if (ranges.Count == 0)
    {
        statusLabel.Text = "No selection";
    }
    else if (ranges.Count == 1)
    {
        GridRangeInfo range = ranges[0];
        int cellCount = (range.Bottom - range.Top + 1) * (range.Right - range.Left + 1);
        statusLabel.Text = $"Selected: {cellCount} cell(s)";
    }
    else
    {
        statusLabel.Text = $"Multiple selections: {ranges.Count} range(s)";
    }
}
```

## Selection Patterns

### Select All:

```csharp
private void SelectAll()
{
    gridControl1.Selections.Clear();
    gridControl1.Selections.Add(GridRangeInfo.Table());
}
```

### Select Row on Cell Click:

```csharp
gridControl1.CellClick += (sender, e) =>
{
    if (e.RowIndex > 0 && e.ColIndex > 0)
    {
        gridControl1.Selections.Clear();
        gridControl1.Selections.Add(GridRangeInfo.Row(e.RowIndex));
    }
};
```

### Select Column Header Click:

```csharp
gridControl1.CellClick += (sender, e) =>
{
    if (e.RowIndex == 0 && e.ColIndex > 0)
    {
        // Column header clicked
        gridControl1.Selections.Clear();
        gridControl1.Selections.Add(GridRangeInfo.Col(e.ColIndex));
    }
};
```

### Conditional Selection:

```csharp
private void SelectCellsWithValue(string value)
{
    gridControl1.Selections.Clear();
    
    for (int row = 1; row <= gridControl1.RowCount; row++)
    {
        for (int col = 1; col <= gridControl1.ColCount; col++)
        {
            if (gridControl1[row, col].Text == value)
            {
                gridControl1.Selections.Add(GridRangeInfo.Cell(row, col));
            }
        }
    }
}
```

### Select Rows Matching Criteria:

```csharp
private void SelectRowsWhere(Func<int, bool> predicate)
{
    gridControl1.Selections.Clear();
    
    for (int row = 1; row <= gridControl1.RowCount; row++)
    {
        if (predicate(row))
        {
            gridControl1.Selections.Add(GridRangeInfo.Row(row));
        }
    }
}

// Usage
SelectRowsWhere(row => 
{
    var value = Convert.ToDouble(gridControl1[row, 3].CellValue ?? 0);
    return value > 1000;
});
```

## Working with Selected Data

### Get Values from Selection:

```csharp
private List<object> GetSelectedValues()
{
    List<object> values = new List<object>();
    GridRangeInfoList ranges = gridControl1.Selections.GetSelectedRanges(false, false);
    
    foreach (GridRangeInfo range in ranges)
    {
        for (int row = range.Top; row <= range.Bottom; row++)
        {
            for (int col = range.Left; col <= range.Right; col++)
            {
                values.Add(gridControl1[row, col].CellValue);
            }
        }
    }
    
    return values;
}
```

### Apply Formatting to Selection:

```csharp
private void FormatSelection(Color backColor, Color textColor)
{
    GridRangeInfoList ranges = gridControl1.Selections.GetSelectedRanges(false, false);
    
    GridStyleInfo style = new GridStyleInfo();
    style.BackColor = backColor;
    style.TextColor = textColor;
    
    foreach (GridRangeInfo range in ranges)
    {
        gridControl1.ChangeCells(range, style);
    }
}

// Usage
FormatSelection(Color.Yellow, Color.Red);
```

### Delete Selected Cells:

```csharp
private void DeleteSelectedCells()
{
    GridRangeInfoList ranges = gridControl1.Selections.GetSelectedRanges(false, false);
    
    foreach (GridRangeInfo range in ranges)
    {
        for (int row = range.Top; row <= range.Bottom; row++)
        {
            for (int col = range.Left; col <= range.Right; col++)
            {
                gridControl1[row, col].CellValue = string.Empty;
            }
        }
    }
}
```

### Copy Selection to Clipboard:

```csharp
private void CopySelectionToClipboard()
{
    GridRangeInfoList ranges = gridControl1.Selections.GetSelectedRanges(false, false);
    
    if (ranges.Count > 0)
    {
        // Use built-in clipboard support
        gridControl1.Model.CutPaste.Copy();
    }
}
```

## Selection Appearance

### Customize Selection Colors:

```csharp
// Selection frame color
gridControl1.Properties.Excel2003SelectionColor = Color.Blue;

// Alpha blend (transparency)
gridControl1.AlphaBlendSelectionColor = Color.FromArgb(128, Color.LightBlue);
gridControl1.Properties.ExcelLikeSelectionFrame = true;
```

### Excel-Like Selection Frame:

```csharp
// Enable Excel-style selection
gridControl1.ExcelLikeSelectionFrame = true;
gridControl1.ExcelLikeCurrentCell = true;

// Selection frame style
gridControl1.Properties.SelectionFrameOption = SelectionFrameOption.Excel2016;
// or
gridControl1.Properties.SelectionFrameOption = SelectionFrameOption.Excel2003;
```

## Best Practices

1. **Clear selections** before adding new ones for single-select behavior
2. **Use events** to respond to selection changes
3. **Validate selections** before performing operations
4. **Provide visual feedback** for selected items
5. **Handle empty selections** gracefully
6. **Use appropriate selection mode** for your use case
7. **Test with keyboard navigation** (arrows, Tab, Shift)

## Common Scenarios

### Context Menu on Selection:

```csharp
private void ShowContextMenuForSelection()
{
    ContextMenuStrip menu = new ContextMenuStrip();
    menu.Items.Add("Copy", null, (s, e) => CopySelectionToClipboard());
    menu.Items.Add("Delete", null, (s, e) => DeleteSelectedCells());
    menu.Items.Add("Format...", null, (s, e) => ShowFormatDialog());
    
    gridControl1.ContextMenuStrip = menu;
}
```

### Multi-Row Operations:

```csharp
private void DeleteSelectedRows()
{
    GridRangeInfoList selectedRows = gridControl1.Selections.GetSelectedRows(false, false);
    
    // Delete from bottom to top to maintain indices
    foreach (GridRangeInfo range in selectedRows.Reverse())
    {
        for (int row = range.Bottom; row >= range.Top; row--)
        {
            gridControl1.Rows.RemoveRange(row, row);
        }
    }
}
```

## Troubleshooting

### Selection not visible
- Check `ExcelLikeSelectionFrame` property
- Verify selection colors are set
- Ensure grid has focus

### Cannot select cells
- Check `AllowSelection` property
- Verify `ReadOnly` isn't preventing selection
- Check if custom event handler is canceling selection

### Multiple selections not working
- Ensure `GridSelectionFlags.Multiple` is set
- Check `ListBoxSelectionMode` for row selection
- Verify users are using Ctrl/Shift keys

## Next Steps

- Implement custom selection behavior
- Add keyboard shortcuts for selection
- Create selection-based operations
- Integrate with copy/paste functionality
