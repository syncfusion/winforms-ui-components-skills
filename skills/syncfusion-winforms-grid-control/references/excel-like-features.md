# Excel-Like Features

## Table of Contents
- [Overview](#overview)
- [Excel-Like Selection Frame](#excel-like-selection-frame)
- [Excel-Like Current Cell](#excel-like-current-cell)
- [Selection Frame Styles](#selection-frame-styles)
- [Copy and Paste](#copy-and-paste)
- [Keyboard Navigation](#keyboard-navigation)
- [Other Excel Behaviors](#other-excel-behaviors)

## Overview

GridControl provides Excel-like features that make the grid behave similar to Microsoft Excel, including selection frames, current cell highlighting, keyboard navigation, and clipboard operations.

## Excel-Like Selection Frame

Enable Excel-style selection frames that highlight selected cells with a border.

### Basic Configuration:

```csharp
// Enable Excel-like selection frame
gridControl1.ExcelLikeSelectionFrame = true;

// Enable current cell highlighting
gridControl1.ExcelLikeCurrentCell = true;
```

### Visual Result:
- Selected cells show a thick border frame
- Current cell highlighted with special border
- Looks and feels like Excel selection

## Excel-Like Current Cell

Highlights the active cell with Excel-style appearance.

```csharp
// Enable current cell highlighting
gridControl1.ExcelLikeCurrentCell = true;

// Works best with selection frame
gridControl1.ExcelLikeSelectionFrame = true;
```

**Features:**
- Current cell has distinct appearance
- Shows which cell is active for editing
- Updates as user navigates
- Compatible with range selection

## Selection Frame Styles

Choose between different Excel selection frame styles.

### Excel 2016 Style (Default):

```csharp
gridControl1.ExcelLikeSelectionFrame = true;
gridControl1.ExcelLikeCurrentCell = true;
gridControl1.Model.Options.SelectionFrameOption = SelectionFrameOption.Excel2016;
```

**Appearance:**
- Modern flat design
- Clean borders
- Subtle current cell indicator

### Excel 2003 Style:

```csharp
gridControl1.ExcelLikeSelectionFrame = true;
gridControl1.ExcelLikeCurrentCell = true;
gridControl1.Model.Options.SelectionFrameOption = SelectionFrameOption.Excel2003;
```

**Appearance:**
- Classic Excel look
- Thicker borders
- Traditional styling

### Customizing Selection Colors:

```csharp
// Alpha blend for transparency
gridControl1.AlphaBlendSelectionColor = Color.FromArgb(128, Color.LightBlue);
```

## Copy and Paste

Built-in clipboard support for Excel-like copy/paste operations.

### Copy Selected Cells:

```csharp
// Copy selection to clipboard
gridControl1.Model.CutPaste.Copy();

// Or use Ctrl+C (works automatically)
```

### Paste from Clipboard:

```csharp
// Paste from clipboard
gridControl1.Model.CutPaste.Paste();

// Or use Ctrl+V (works automatically)
```

### Cut Operation:

```csharp
// Cut selection
gridControl1.Model.CutPaste.Cut();

// Or use Ctrl+X (works automatically)
```

### Custom Clipboard Handling:

```csharp
// Handle clipboard operations
gridControl1.ClipboardCanCopy += (s, e) =>
{
    // Allow/prevent copy
    e.Result = true;
};

gridControl1.ClipboardCanPaste += (s, e) =>
{
    // Validate before paste
    e.Result = ValidatePasteData(e.Text);
};

gridControl1.ClipboardPaste += (s, e) =>
{
    // Custom paste logic
    Console.WriteLine($"Pasted: {e.Text}");
};
```

## Keyboard Navigation

Excel-like keyboard shortcuts and navigation.

### Arrow Keys:

```
↑ Up Arrow    - Move to cell above
↓ Down Arrow  - Move to cell below
← Left Arrow  - Move to cell on left
→ Right Arrow - Move to cell on right
```

### Tab Navigation:

```
Tab         - Move to next cell (right)
Shift+Tab   - Move to previous cell (left)
```

### Enter Key:

```
Enter       - Move down one row
Shift+Enter - Move up one row
```

### Home and End:

```
Home       - Move to first column
End        - Move to last column
Ctrl+Home  - Move to first cell
Ctrl+End   - Move to last cell
```

### Page Navigation:

```
Page Up    - Scroll up one page
Page Down  - Scroll down one page
```

### Selection with Shift:

```
Shift+Arrow Keys  - Extend selection
Shift+Home        - Select to beginning of row
Shift+End         - Select to end of row
Ctrl+Shift+Home   - Select to beginning of grid
Ctrl+Shift+End    - Select to end of grid
```

### Configuring Navigation:

```csharp
// Configure Enter key behavior
gridControl1.WantTabKey = true;  // Handle Tab key
gridControl1.WantEnterKey = true; // Handle Enter key

// Navigate on Enter
gridControl1.ActivateCurrentCellBehavior = GridCellActivateAction.ClickOnCell;
```

## Other Excel Behaviors

### Freeze Panes:

```csharp
// Freeze at specific position
gridControl1.Model.Rows.FrozenCount = 2;  // Freeze first 2 rows
gridControl1.Model.Cols.FrozenCount = 1;  // Freeze first column
```

### Grid Lines:

```csharp
// Excel-like grid lines
gridControl1.GridLineColor = Color.LightGray;
```

### Column Resizing:

```csharp
// Allow column resizing like Excel
gridControl1.ResizeColsBehavior = GridResizeCellsBehavior.ResizeAll;
```

### Row Resizing:

```csharp
// Allow row resizing
gridControl1.ResizeRowsBehavior = GridResizeCellsBehavior.ResizeAll;
```

## Complete Excel-Like Setup

### Full Configuration:

```csharp
private void ConfigureExcelLikeGrid()
{
    // Selection
    gridControl1.ExcelLikeSelectionFrame = true;
    gridControl1.ExcelLikeCurrentCell = true;
    gridControl1.Model.Options.SelectionFrameOption = SelectionFrameOption.Excel2016;Excel2016;
    gridControl1.AllowSelection = GridSelectionFlags.Any;
    
    // Navigation
    gridControl1.WantTabKey = true;
    gridControl1.WantEnterKey = true;
    
    // Appearance
    gridControl1.GridLineColor = Color.LightGray;
    
    // Resizing
    gridControl1.ResizeColsBehavior = GridResizeCellsBehavior.ResizeAll;
    gridControl1.ResizeRowsBehavior = GridResizeCellsBehavior.ResizeAll;
    
    // Headers
    gridControl1.Properties.RowHeaders = true;
    gridControl1.Properties.ColHeaders = true;
    
    // Freeze panes
    gridControl1.Model.Rows.FrozenCount = 1;  // Freeze header row
}
```

## Best Practices

1. **Enable both selection frame and current cell** for best Excel-like experience
2. **Choose appropriate selection frame style** (2016 for modern, 2003 for classic)
3. **Test keyboard navigation** to ensure it works as expected
4. **Handle clipboard events** for custom validation
5. **Configure freeze panes** for better usability with large grids
6. **Enable resizing** for user flexibility

## Troubleshooting

### Selection frame not visible
- Ensure `ExcelLikeSelectionFrame` is true
- Check selection frame color settings
- Verify grid has focus

### Keyboard navigation not working
- Check `WantTabKey` and `WantEnterKey` properties
- Verify grid has focus
- Ensure no custom key handling is interfering

### Copy/paste not working
- Check if custom handlers are preventing default behavior
- Ensure grid has focus

## Next Steps

- Implement custom clipboard formats
- Add Excel-like formulas
- Configure conditional formatting
- Implement Excel import/export
- Add data validation like Excel
