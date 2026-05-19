# Drag and Drop

This guide covers drag and drop functionality in GridControl, including column repositioning, row reordering, and touch support.

## Overview

GridControl supports drag and drop operations for:
- Column repositioning
- Row reordering
- Cell content drag
- Touch-enabled drag operations

## Column Drag and Drop

Enable users to reorder columns by dragging column headers.

### Enabling Column Drag:

```csharp
// Enable column dragging
gridControl1.AllowDragSelectedCols = true;
```

### How It Works:
1. User clicks column header
2. Drags to new position
3. Column moves to dropped location
4. Grid updates layout automatically

### Example:

```csharp
private void InitializeGrid()
{
    gridControl1.AllowDragSelectedCols = true;
    
    // Populate grid
    for (int col = 1; col <= 5; col++)
    {
        gridControl1[0, col].CellValue = $"Column {col}";
    }
}
```

## Row Drag and Drop

Enable users to reorder rows by dragging row headers.

### Enabling Row Drag:

```csharp
// Enable row dragging
gridControl1.AllowDragSelectedRows = true;
```

### Example:

```csharp
private void InitializeGrid()
{
    gridControl1.AllowDragSelectedRows = true;
    
    // Populate grid
    for (int row = 1; row <= 10; row++)
    {
        gridControl1[row, 0].CellValue = $"Row {row}";
        gridControl1[row, 1].CellValue = $"Data {row}";
    }
}
```

## Drag and Drop Events
To change the effects of the contents while dropping, Effect property can be used.

```csharp
this.gridControl2.DragOver += new DragEventHandler(gridControl2_DragOver);

void gridControl2_DragOver(object sender, DragEventArgs e)
{
    // Contents that are been dragged over gridControl2 will be copied.
    e.Effect = DragDropEffects.Copy;
}
```

### Preventing drag and drop

The contents alone can be prevented while dropping by setting the Effect property as None.

```csharp
this.gridControl2.DragOver += new DragEventHandler(gridControl2_DragOver);

void gridControl2_DragOver(object sender, DragEventArgs e)
{
    // Contents that are been dragged over gridControl2 will not be moved or copied.
    e.Effect = DragDropEffects.None;
}
```

### Blocking the Data before Dragging

Fired when rows are being moved. Can be canceled.

```csharp
this.gridControl1.QueryCanOleDragRange += new GridQueryCanOleDragRangeEventHandler(gridControl1_QueryCanOleDragRange);

void gridControl1_QueryCanOleDragRange(object sender, GridQueryCanOleDragRangeEventArgs e)
{
    // Drag Drop process will be canceled.
    e.Cancel = true;
}
```

### Dragging and dropping for merged cells

Preserve merged cells during drag

```csharp
void gridControl1_QueryCanOleDragRange(object sender, GridQueryCanOleDragRangeEventArgs e)
{
    if (e.Range.Left != e.Range.Right || e.Range.Top!=e.Range.Bottom)
    this.gridControl1.Selections.Add(e.Range);
}
```
### Retaining the Dragged Contents

Use QueryDragDropMoveClearCells event, cancel clearing source grid data during dragdrop

```csharp
this.gridControl1.Model.QueryDragDropMoveClearCells += new CancelEventHandler(Model_QueryDragDropMoveClearCells);

void Model_QueryDragDropMoveClearCells(object sender, CancelEventArgs e)
{
    // Cancels the Clearing of Cells in the Grid.
    e.Cancel = true;
}
```
## Touch Support

GridControl fully supports touch gestures for drag and drop operations on touch-enabled devices.

### Enabling Touch:

```csharp
// Touch support is automatically enabled
// No additional configuration required

// Ensure drag and drop is enabled
gridControl1.AllowDragSelectedCols = true;
gridControl1.AllowDragSelectedRows = true;
```

### Touch Gestures:
- **Tap and hold** - Start drag operation
- **Drag** - Move column/row
- **Release** - Drop at new location

## Restricting Drag Operations

### Prevent Specific Columns from Moving:

```csharp
gridControl1.ColsMoving += (sender, e) =>
{
    // Don't allow moving columns 1 and 2
    if (e.From == 1 || e.From == 2)
    {
        e.Cancel = true;
    }
};
```

### Prevent Dragging to Specific Locations:

```csharp
gridControl1.ColsMoving += (sender, e) =>
{
    // Don't allow dropping before column 3
    if (e.To < 3)
    {
        MessageBox.Show("Cannot move before column 3");
        e.Cancel = true;
    }
};
```

### Conditional Row Movement:

```csharp
gridControl1.RowsMoving += (sender, e) =>
{
    // Only allow moving within same section
    int sectionStart = GetSectionStart(e.From);
    int sectionEnd = GetSectionEnd(e.From);
    
    if (e.To < sectionStart || e.To > sectionEnd)
    {
        MessageBox.Show("Cannot move row outside its section");
        e.Cancel = true;
    }
};
```

## Visual Feedback

### Drag Markers:

```csharp
// Enable visual drag markers
gridControl1.DragDropCols = true;  // Column drag marker
gridControl1.DragDropRows = true;  // Row drag marker

// Customize drag appearance
gridControl1.Properties.GridLineColor = Color.Blue;  // Marker color
```

### Custom Drag Cursor:

```csharp
gridControl1.ColsMoving += (sender, e) =>
{
    // Change cursor during drag
    Cursor.Current = Cursors.Hand;
};

gridControl1.ColsMoved += (sender, e) =>
{
    // Restore cursor
    Cursor.Current = Cursors.Default;
};
```

## Complete Example

### Full Drag and Drop Configuration:

```csharp
public class DragDropGrid : Form
{
    private GridControl gridControl1;
    
    public DragDropGrid()
    {
        InitializeComponent();
        SetupGrid();
        SetupDragDrop();
    }
    
    private void SetupGrid()
    {
        gridControl1 = new GridControl();
        gridControl1.Dock = DockStyle.Fill;
        gridControl1.RowCount = 20;
        gridControl1.ColCount = 5;
        
        // Populate headers
        for (int col = 1; col <= 5; col++)
        {
            gridControl1[0, col].CellValue = $"Column {col}";
        }
        
        // Populate data
        for (int row = 1; row <= 20; row++)
        {
            for (int col = 1; col <= 5; col++)
            {
                gridControl1[row, col].CellValue = $"R{row}C{col}";
            }
        }
        
        this.Controls.Add(gridControl1);
    }
    
    private void SetupDragDrop()
    {
        // Enable drag and drop
        gridControl1.AllowDragSelectedCols = true;
        gridControl1.AllowDragSelectedRows = true;
    }
}
```

## Data Persistence

### Saving Column Order:

```csharp
private List<int> GetColumnOrder()
{
    List<int> order = new List<int>();
    for (int col = 1; col <= gridControl1.ColCount; col++)
    {
        order.Add(col);
    }
    return order;
}

private void SaveColumnOrder()
{
    var order = GetColumnOrder();
    // Save to settings or database
    Properties.Settings.Default.ColumnOrder = string.Join(",", order);
    Properties.Settings.Default.Save();
}

gridControl1.ColsMoved += (sender, e) =>
{
    SaveColumnOrder();
};
```

## Best Practices

1. **Enable visual markers** for better user feedback
2. **Validate moves** in Moving events before they complete
3. **Update data sources** after successful moves
4. **Save user preferences** for column/row order
5. **Provide clear feedback** for invalid operations
6. **Test on touch devices** if targeting tablets/touch screens
7. **Handle undo/redo** for complex scenarios

## Common Scenarios

### Reorderable Data Grid:

```csharp
// Allow users to customize column order
gridControl1.AllowDragSelectedCols = true;

// Save preference on move
gridControl1.ColsMoved += (s, e) => SaveUserPreferences();
```

### Priority List:

```csharp
// Allow row reordering for priority
gridControl1.AllowDragSelectedRows = true;
gridControl1.RowsMoved += (s, e) => UpdatePriorities();
```

## Troubleshooting

### Drag not working
- Verify `AllowDragSelectedCols` or `AllowDragSelectedRows` is true
- Check if events are canceling the operation
- Ensure grid has focus

### Visual feedback missing
- Check grid theme settings
- Verify grid is visible and not covered

### Touch drag not responsive
- Test on actual touch device
- Check Windows touch settings
- Verify no gesture conflicts

## Next Steps

- Implement custom drag cursors
- Add undo/redo for drag operations
- Persist user's column order
- Create drag-and-drop between grids
- Add drag preview images
