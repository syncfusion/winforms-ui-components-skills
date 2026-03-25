# Scrolling and Zooming

This guide covers scrolling and zooming functionality in GridControl, including frozen rows/columns, scroll events, and zoom configuration.

## Overview

GridControl provides comprehensive scrolling and zooming features:
- Horizontal and vertical scrolling
- Frozen rows and columns
- Smooth scrolling
- Zoom in/out functionality
- Scroll events for custom behavior

## Basic Scrolling

### Scroll Bar Configuration:

```csharp
// Scroll bar modes
gridControl1.HScrollBehavior = GridScrollbarMode.Shared;  // Horizontal
gridControl1.VScrollBehavior = GridScrollbarMode.Shared;  // Vertical
```

**Scroll Bar Modes:**
- `Shared` - Standard scroll bars (default)
- `Disabled` - No scroll bars
- `Hidden` - Scroll bars hidden but scrolling still works
- `AutoHidden` - Show only when needed

### Example:

```csharp
// Standard scrolling
gridControl1.HScrollBehavior = GridScrollbarMode.Shared;
gridControl1.VScrollBehavior = GridScrollbarMode.Shared;

// Hide horizontal scroll bar
gridControl1.HScrollBehavior = GridScrollbarMode.Disabled;

// Auto-hide scroll bars
gridControl1.HScrollBehavior = GridScrollbarMode.AutoHidden;
gridControl1.VScrollBehavior = GridScrollbarMode.AutoHidden;
```

## Frozen Rows and Columns

Keep specific rows and columns visible while scrolling.

### Freeze First Row (Header):

```csharp
// Freeze first row
gridControl1.FreezeFirstRow = true;

// Or specify number of rows to freeze
gridControl1.Model.Rows.FrozenCount = 1;
```

### Freeze First Column:

```csharp
// Freeze first column
gridControl1.FreezeFirstColumn = true;

// Or specify number of columns to freeze
gridControl1.Model.Cols.FrozenCount = 1;
```

### Freeze Multiple Rows/Columns:

```csharp
// Freeze first 3 rows
gridControl1.Model.Rows.FrozenCount = 3;

// Freeze first 2 columns
gridControl1.Model.Cols.FrozenCount = 2;
```

### Complete Freeze Example:

```csharp
private void SetupFrozenPanes()
{
    // Freeze header row and first column
    gridControl1.Model.Rows.FrozenCount = 1;
    gridControl1.Model.Cols.FrozenCount = 1;
    
    // Style frozen cells
    GridStyleInfo frozenStyle = new GridStyleInfo
    {
        BackColor = Color.LightGray,
        Font = { Bold = true }
    };
    
    // Apply to frozen row
    gridControl1.ChangeCells(GridRangeInfo.Row(1), frozenStyle);
    
    // Apply to frozen column
    gridControl1.ChangeCells(GridRangeInfo.Col(1), frozenStyle);
}
```

## Programmatic Scrolling

### Scroll to Specific Cell:

```csharp
// Scroll to make cell visible
gridControl1.ScrollCellInView(10, 5);

// Alternative
gridControl1.TopRowIndex = 10;
gridControl1.LeftColIndex = 5;
```

### Scroll to Top:

```csharp
// Scroll to beginning
gridControl1.TopRowIndex = 0;
gridControl1.LeftColIndex = 0;
```

### Scroll to Bottom:

```csharp
// Scroll to end
gridControl1.TopRowIndex = gridControl1.RowCount - 1;
gridControl1.LeftColIndex = gridControl1.ColCount - 1;
```

### Smooth Scrolling:

```csharp
// Enable smooth scrolling
gridControl1.Properties.ScrollingAnimationMode = GridScrollingAnimationMode.Smooth;
```

## Scroll Events

### VScrollPixelPosChanged Event:

Fired when vertical scroll position changes.

```csharp
gridControl1.VScrollPixelPosChanged += (sender, e) =>
{
    int scrollPos = gridControl1.VScrollBar.Value;
    Console.WriteLine($"Vertical scroll position: {scrollPos}");
    
    // Update status bar
    statusLabel.Text = $"Row: {gridControl1.TopRowIndex}";
};
```

### HScrollPixelPosChanged Event:

Fired when horizontal scroll position changes.

```csharp
gridControl1.HScrollPixelPosChanged += (sender, e) =>
{
    int scrollPos = gridControl1.HScrollBar.Value;
    Console.WriteLine($"Horizontal scroll position: {scrollPos}");
    
    // Update status bar
    statusLabel.Text = $"Column: {gridControl1.LeftColIndex}";
};
```

### Scroll Position Tracking:

```csharp
private void TrackScrollPosition()
{
    gridControl1.VScrollPixelPosChanged += (s, e) =>
    {
        UpdateScrollIndicator();
    };
    
    gridControl1.HScrollPixelPosChanged += (s, e) =>
    {
        UpdateScrollIndicator();
    };
}

private void UpdateScrollIndicator()
{
    int currentRow = gridControl1.TopRowIndex;
    int totalRows = gridControl1.RowCount;
    int percentScrolled = (int)((double)currentRow / totalRows * 100);
    
    statusLabel.Text = $"Position: Row {currentRow}/{totalRows} ({percentScrolled}%)";
}
```

## Zooming

Control the zoom level of the grid for better visibility.

### Enable Zoom:

```csharp
// Set zoom level (default is 100)
gridControl1.ZoomSize = 150;  // 150% zoom

// Zoom in
gridControl1.ZoomSize = 125;

// Zoom out
gridControl1.ZoomSize = 75;

// Reset to normal
gridControl1.ZoomSize = 100;
```

### Zoom with Mouse Wheel:

```csharp
// Enable zoom with Ctrl+MouseWheel
gridControl1.MouseWheel += GridControl1_MouseWheel;

private void GridControl1_MouseWheel(object sender, MouseEventArgs e)
{
    if (ModifierKeys == Keys.Control)
    {
        if (e.Delta > 0)
        {
            // Zoom in
            gridControl1.ZoomSize = Math.Min(gridControl1.ZoomSize + 10, 200);
        }
        else
        {
            // Zoom out
            gridControl1.ZoomSize = Math.Max(gridControl1.ZoomSize - 10, 50);
        }
        
        ((HandledMouseEventArgs)e).Handled = true;
    }
}
```

### Zoom Toolbar:

```csharp
private void CreateZoomToolbar()
{
    ToolStrip toolbar = new ToolStrip();
    
    // Zoom in button
    ToolStripButton zoomInBtn = new ToolStripButton("+");
    zoomInBtn.Click += (s, e) => gridControl1.ZoomSize += 10;
    toolbar.Items.Add(zoomInBtn);
    
    // Zoom out button
    ToolStripButton zoomOutBtn = new ToolStripButton("-");
    zoomOutBtn.Click += (s, e) => gridControl1.ZoomSize = Math.Max(50, gridControl1.ZoomSize - 10);
    toolbar.Items.Add(zoomOutBtn);
    
    // Zoom reset button
    ToolStripButton resetBtn = new ToolStripButton("100%");
    resetBtn.Click += (s, e) => gridControl1.ZoomSize = 100;
    toolbar.Items.Add(resetBtn);
    
    // Zoom label
    ToolStripLabel zoomLabel = new ToolStripLabel("100%");
    toolbar.Items.Add(zoomLabel);
    
    // Update label on zoom
    gridControl1.ZoomChanged += (s, e) =>
    {
        zoomLabel.Text = $"{gridControl1.ZoomSize}%";
    };
    
    this.Controls.Add(toolbar);
}
```

## Performance Optimization

### Virtual Scrolling:

Use virtual mode for large datasets:

```csharp
// Set large row count without memory allocation
gridControl1.Model.RowCount = 1000000;
gridControl1.Model.ColCount = 50;

// Load data on-demand
gridControl1.QueryCellInfo += (sender, e) =>
{
    if (e.RowIndex > 0 && e.ColIndex > 0)
    {
        // Load only visible cells
        e.Style.CellValue = GetData(e.RowIndex, e.ColIndex);
    }
};
```

### Disable Animations for Performance:

```csharp
// Disable scrolling animations for better performance
gridControl1.Properties.ScrollingAnimationMode = GridScrollingAnimationMode.Disabled;
```

### BeginUpdate/EndUpdate:

```csharp
// Suspend updates during batch operations
gridControl1.BeginUpdate();

// Perform operations...
PopulateGrid();

// Resume updates
gridControl1.EndUpdate();
```

## Custom Scroll Behavior

### Snap to Row:

```csharp
gridControl1.VScrollPixelPosChanged += (sender, e) =>
{
    // Snap scroll to full rows
    int rowHeight = gridControl1.Model.RowHeights[1];
    int snapPosition = (gridControl1.VScrollBar.Value / rowHeight) * rowHeight;
    
    if (gridControl1.VScrollBar.Value != snapPosition)
    {
        gridControl1.VScrollBar.Value = snapPosition;
    }
};
```

### Infinite Scrolling:

```csharp
gridControl1.VScrollPixelPosChanged += (sender, e) =>
{
    // Load more data when scrolled near bottom
    int threshold = gridControl1.RowCount - 10;
    
    if (gridControl1.TopRowIndex + gridControl1.VisibleRows > threshold)
    {
        LoadMoreRows();
    }
};

private void LoadMoreRows()
{
    gridControl1.BeginUpdate();
    
    int currentRowCount = gridControl1.RowCount;
    gridControl1.RowCount += 50;  // Add 50 more rows
    
    // Populate new rows...
    
    gridControl1.EndUpdate();
}
```

## Complete Example

### Full Scrolling and Zooming Setup:

```csharp
public class ScrollZoomGrid : Form
{
    private GridControl gridControl1;
    private StatusStrip statusStrip;
    private ToolStripStatusLabel statusLabel;
    
    public ScrollZoomGrid()
    {
        SetupGrid();
        SetupFrozenPanes();
        SetupScrollTracking();
        SetupZoom();
        SetupStatusBar();
    }
    
    private void SetupGrid()
    {
        gridControl1 = new GridControl();
        gridControl1.Dock = DockStyle.Fill;
        gridControl1.RowCount = 100;
        gridControl1.ColCount = 20;
        
        // Populate grid...
        
        this.Controls.Add(gridControl1);
    }
    
    private void SetupFrozenPanes()
    {
        // Freeze header row and first column
        gridControl1.Model.Rows.FrozenCount = 1;
        gridControl1.Model.Cols.FrozenCount = 1;
    }
    
    private void SetupScrollTracking()
    {
        gridControl1.VScrollPixelPosChanged += (s, e) =>
        {
            UpdateStatus();
        };
    }
    
    private void SetupZoom()
    {
        gridControl1.MouseWheel += (s, e) =>
        {
            if (ModifierKeys == Keys.Control)
            {
                if (e.Delta > 0)
                    gridControl1.ZoomSize += 10;
                else
                    gridControl1.ZoomSize = Math.Max(50, gridControl1.ZoomSize - 10);
                    
                ((HandledMouseEventArgs)e).Handled = true;
                UpdateStatus();
            }
        };
    }
    
    private void SetupStatusBar()
    {
        statusStrip = new StatusStrip();
        statusLabel = new ToolStripStatusLabel();
        statusStrip.Items.Add(statusLabel);
        this.Controls.Add(statusStrip);
        UpdateStatus();
    }
    
    private void UpdateStatus()
    {
        statusLabel.Text = $"Row: {gridControl1.TopRowIndex} | Zoom: {gridControl1.ZoomSize}%";
    }
}
```

## Best Practices

1. **Freeze headers** for better navigation in large grids
2. **Use virtual mode** for datasets with thousands of rows
3. **Implement smooth scrolling** for better UX
4. **Track scroll position** for user feedback
5. **Enable zoom controls** for accessibility
6. **Test performance** with realistic data volumes
7. **Provide scroll indicators** for long lists

## Troubleshooting

### Scroll bars not appearing
- Check `HScrollBehavior` and `VScrollBehavior` settings
- Verify grid has more content than visible area
- Ensure `ScrollbarMode` is not `Disabled`

### Frozen panes not working
- Verify `FrozenCount` is set correctly
- Check if rows/columns exist
- Ensure not exceeding visible area

### Performance issues while scrolling
- Use virtual mode (QueryCellInfo)
- Disable scrolling animations
- Reduce cell complexity
- Implement data caching

### Zoom not working
- Verify `ZoomSize` property is set
- Check if zoom is supported in current view mode
- Test with different values

## Next Steps

- Implement custom scroll indicators
- Add keyboard shortcuts for scrolling
- Create scroll-to-search functionality
- Implement virtual scrolling with caching
- Add scroll position bookmarks
