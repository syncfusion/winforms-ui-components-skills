# Advanced Features

## Table of Contents
- [Overview](#overview)
- [Find and Replace](#find-and-replace)
- [Tooltips](#tooltips)
- [Zooming](#zooming)
- [Covered Ranges](#covered-ranges)
- [Common Scenarios](#common-scenarios)
- [Best Practices](#best-practices)

## Overview

GridGroupingControl provides advanced features for enhanced user experience and specialized scenarios:

- **Find and Replace**: Excel-like search and replace functionality with dialog or programmatic API
- **Tooltips**: Cell-level tooltips for additional information on hover
- **Zooming**: Grid-level and cell-level magnification for better visualization
- **Covered Ranges**: Merge cells for custom layouts and headers

**Assembly requirement**: Some features require `Syncfusion.GridHelperClasses.Windows.dll` assembly reference.

## Find and Replace

### Requirements

Add assembly reference:
```csharp
// Reference: Syncfusion.GridHelperClasses.Windows.dll
using Syncfusion.Windows.Forms.Grid;
```

### Using Find/Replace Dialog

Show Excel-like find/replace dialog:

```csharp
using Syncfusion.Windows.Forms.Grid;

// Create dialog sink
GridFindReplaceDialogSink findReplaceSink = 
    new GridFindReplaceDialogSink(gridGroupingControl1.TableControl);

// Show dialog
GridFindReplaceDialog findReplaceDialog = GridFindReplaceDialog.Instance;
findReplaceDialog.ActiveSink = findReplaceSink;
findReplaceDialog.ShowDialog();
```

Dialog features:
- Find Next / Find All
- Replace / Replace All
- Match case
- Match whole cell
- Search direction (up/down)
- Search scope (column, selection, whole table)

### Keyboard Shortcut (Ctrl+F)

Activate find dialog with keyboard shortcut:

```csharp
gridGroupingControl1.TableControlCurrentCellKeyDown += 
    TableControlCurrentCellKeyDown;

void TableControlCurrentCellKeyDown(object sender, KeyEventArgs e)
{
    if (e.Control && e.KeyCode == Keys.F)
    {
        // Show Find/Replace dialog
        GridFindReplaceDialogSink findReplaceSink = 
            new GridFindReplaceDialogSink(gridGroupingControl1.TableControl);
        GridFindReplaceDialog findReplaceDialog = GridFindReplaceDialog.Instance;
        findReplaceDialog.ActiveSink = findReplaceSink;
        findReplaceDialog.ShowDialog();
        
        e.Handled = true;
    }
}
```

### Find Options

Control search behavior with `GridFindTextOptions` enumeration:

```csharp
using Syncfusion.Windows.Forms.Grid;

GridFindTextOptions options = GridFindTextOptions.WholeTable | 
                               GridFindTextOptions.MatchCase;
```

Available options:
- **MatchCase**: Case-sensitive search
- **MatchWholeCell**: Match entire cell content
- **SearchUp**: Search bottom-up instead of top-down
- **ColumnOnly**: Search current column only
- **SelectionOnly**: Search selected range only
- **WholeTable**: Search entire table

### Programmatic Find

Find text without dialog:

```csharp
using Syncfusion.Windows.Forms.Grid;

// Create find/replace sink
GridFindReplaceDialogSink frDialog = 
    new GridFindReplaceDialogSink(gridGroupingControl1.TableControl);

// Set search options
GridFindTextOptions options = GridFindTextOptions.WholeTable;
object locInfo = GridRangeInfo.Table();

// Create event args
GridFindReplaceEventArgs frEvents = 
    new GridFindReplaceEventArgs("SearchText", "", options, locInfo);

// Find next match
GridRangeInfo result = (GridRangeInfo)frDialog.Find(frEvents);

if (result != null)
{
    Console.WriteLine($"Found at: {result}");
}
else
{
    MessageBox.Show("No matches found");
}
```

### Find All

Find all occurrences at once:

```csharp
private bool findAll = false;

void FindAll_Click(object sender, EventArgs e)
{
    findAll = true;
    
    GridFindReplaceDialogSink frDialog = 
        new GridFindReplaceDialogSink(gridGroupingControl1.TableControl);
    
    GridFindTextOptions options = GridFindTextOptions.WholeTable;
    object locInfo = GridRangeInfo.Table();
    
    GridFindReplaceEventArgs frEvents = 
        new GridFindReplaceEventArgs(txtSearch.Text, "", options, locInfo);
    
    // Find all matches
    if (frDialog.Find(frEvents) == null)
    {
        MessageBox.Show($"No matches found for: {txtSearch.Text}");
    }
    
    gridGroupingControl1.Refresh();
}
```

### Programmatic Replace

Replace text programmatically:

```csharp
if (!string.IsNullOrEmpty(txtSearch.Text) && !string.IsNullOrEmpty(txtReplace.Text))
{
    GridFindReplaceDialogSink frDialog = 
        new GridFindReplaceDialogSink(gridGroupingControl1.TableControl);
    
    GridFindTextOptions options = GridFindTextOptions.WholeTable;
    object locInfo = GridRangeInfo.Table();
    
    GridFindReplaceEventArgs frEvents = 
        new GridFindReplaceEventArgs(txtSearch.Text, txtReplace.Text, options, locInfo);
    
    // Replace current match
    frDialog.Replace(frEvents);
    
    // Or replace all matches
    // frDialog.ReplaceAll(frEvents);
}
```

## Tooltips

### Adding Cell Tooltips

Show tooltip on mouse hover using `QueryCellStyleInfo` event:

```csharp
gridGroupingControl1.QueryCellStyleInfo += GridGroupingControl1_QueryCellStyleInfo;

void GridGroupingControl1_QueryCellStyleInfo(object sender, 
    GridTableCellStyleInfoEventArgs e)
{
    if (e.Style.TableCellIdentity.Column != null && 
        e.Style.TableCellIdentity.Column.Name == "FirstName" &&
        e.TableCellIdentity.DisplayElement != null && 
        e.TableCellIdentity.DisplayElement.Kind == DisplayElementKind.Record)
    {
        // Set tooltip text
        e.Style.CellTipText = $"{e.TableCellIdentity.Column.Name} is {e.Style.Text}";
    }
}
```

### Column-Level Tooltips

Apply tooltip to entire column:

```csharp
// Tooltip for all cells in City column
gridGroupingControl1.TableDescriptor.Columns["City"]
    .Appearance.AnyRecordFieldCell.CellTipText = "City";
```

### Nested Table Tooltips

Add tooltips to nested table cells:

```csharp
// Get nested table descriptor
var nestedDescriptor = gridGroupingControl1.GetTableDescriptor("Orders");

// Set tooltip for nested column
nestedDescriptor.Columns["Freight"].Appearance.AnyRecordFieldCell.CellTipText = 
    "Freight cost in USD";
```

### Removing Tooltips

Reset tooltip to remove it:

```csharp
// Remove tooltip from specific column
gridGroupingControl1.TableDescriptor.Columns["ColumnName"]
    .Appearance.AnyRecordFieldCell.ResetCellTipText();
```

### Disabling Tooltips

Disable tooltips for entire grid:

```csharp
// Disable all tooltips
gridGroupingControl1.TableControl.CellToolTip.Active = false;
```

### Tooltip Delay Settings

Control tooltip display timing:

```csharp
using System.Windows.Forms;

// Time tooltip remains visible (milliseconds)
gridGroupingControl1.TableControl.CellToolTip.AutoPopDelay = 5000;

// Delay before tooltip appears
gridGroupingControl1.TableControl.CellToolTip.InitialDelay = 1000;

// Delay when moving to another cell with tooltip
gridGroupingControl1.TableControl.CellToolTip.ReshowDelay = 500;

// Or set all delays at once
gridGroupingControl1.TableControl.CellToolTip.AutomaticDelay = 500;
// Sets: AutoPopDelay = 5000, InitialDelay = 500, ReshowDelay = 100
```

**AutomaticDelay calculation:**
- AutoPopDelay = 10 × AutomaticDelay
- InitialDelay = AutomaticDelay
- ReshowDelay = AutomaticDelay ÷ 5

## Zooming

### Requirements

```csharp
// Reference: Syncfusion.GridHelperClasses.Windows.dll
using Syncfusion.GridHelperClasses;
```

### Grid-Level Zooming

Zoom entire grid by percentage:

```csharp
// Initialize zooming
ZoomGroupingGrid zoom = new ZoomGroupingGrid(gridGroupingControl1);

// Zoom to 150% (range: 0-400)
zoom.zoomGrid("150");
```

Result: All cells, text, and borders scaled to 150%.

### Cell-Level Zooming

Zoom individual cells on click:

```csharp
// Initialize zooming
ZoomGroupingGrid zoom = new ZoomGroupingGrid(gridGroupingControl1);

// Enable cell zoom on click
ZoomGroupingGrid.zoomCell = true;
```

User clicks cell → Magnified view appears near cursor → Click again to dismiss.

### Zoom Modes

#### Ellipse Mode

```csharp
ZoomGroupingGrid zoom = new ZoomGroupingGrid(gridGroupingControl1);

// Ellipse-shaped zoom window
zoom.ZoomImageMode = ZoomGroupingGrid.ImageMode.Ellipse;

// Set zoom window size
zoom.ZoomSize = new Size(250, 400); // Width, Height in pixels
```

#### Rectangle Mode

```csharp
// Rectangle-shaped zoom window
zoom.ZoomImageMode = ZoomGroupingGrid.ImageMode.Rectangle;
zoom.ZoomSize = new Size(250, 400);
```

### Zoom Appearance

#### Border Color

```csharp
// Set zoom border color
zoom.ZoomBorderColor = Color.ForestGreen;
```

#### Border Size

```csharp
// Set border thickness (1-50)
zoom.ZoomBorderSize = 10;
```

#### Zoom Factor

```csharp
// Set magnification factor (up to 4.0)
zoom.ZoomFactor = 2.5f; // 250% magnification
```

Default zoom factor: 1.5 (150%)

## Covered Ranges

Merge adjacent cells into single cell display.

### Adding Covered Ranges

```csharp
using Syncfusion.Windows.Forms.Grid;

// Merge cells from (top=5, left=3) to (bottom=7, right=4)
gridGroupingControl1.TableModel.CoveredRanges.Add(
    GridRangeInfo.Cells(5, 3, 7, 4));
```

**GridRangeInfo.Cells parameters:**
```csharp
GridRangeInfo.Cells(top, left, bottom, right)
```

### Dynamic Covered Ranges (Event-Based)

Add covered ranges conditionally using `QueryCoveredRange` event:

```csharp
gridGroupingControl1.QueryCoveredRange += GridGroupingControl1_QueryCoveredRange;

void GridGroupingControl1_QueryCoveredRange(object sender, 
    GridTableQueryCoveredRangeEventArgs e)
{
    int recordIndex = 3;
    
    if (e.Table.Records[recordIndex].GetValue("Description").ToString() == "Cheeses")
    {
        if (e.RowIndex > 4 && e.RowIndex < 7 && 
            e.ColIndex > 1 && e.ColIndex < 3)
        {
            // Merge 2x2 range
            e.Range = GridRangeInfo.Cells(e.RowIndex, e.ColIndex, 
                e.RowIndex + 1, e.ColIndex + 1);
            e.Handled = true;
        }
    }
}
```

### Finding Covered Ranges

#### Find Method

```csharp
GridRangeInfo range;
bool isCovered = gridGroupingControl1.TableModel.CoveredRanges.Find(
    5, 3, out range);

if (isCovered)
{
    Console.WriteLine($"Cell [5,3] is part of covered range: {range}");
}
else
{
    Console.WriteLine("Cell [5,3] is not covered");
}
```

#### FindRange Method

```csharp
GridRangeInfo range = gridGroupingControl1.TableModel.CoveredRanges.FindRange(5, 3);

if (range != null)
{
    Console.WriteLine($"Covered range: {range}");
}
```

#### GetSpannedRangeInfo Method

Alternative method for finding covered ranges:

```csharp
GridRangeInfo range;
gridGroupingControl1.TableModel.GetSpannedRangeInfo(5, 3, out range);
```

### Removing Covered Ranges

#### Remove Specific Range

```csharp
// Remove specific covered range
gridGroupingControl1.TableModel.CoveredRanges.Remove(
    GridRangeInfo.Cells(5, 3, 7, 4));
```

#### Clear All

```csharp
// Remove all covered ranges
gridGroupingControl1.TableModel.CoveredRanges.Clear();
```

### Important Notes

**Sorting impact**: Covered ranges don't move with records during sorting. They remain at original cell positions.

**Content display**: Top-left cell content displays across merged area. Other cells in range are hidden.

## Common Scenarios

### Scenario 1: Excel-Like Find with Highlighting

```csharp
private List<GridRangeInfo> highlightedCells = new List<GridRangeInfo>();

void FindAllWithHighlight()
{
    // Clear previous highlights
    highlightedCells.Clear();
    
    GridFindReplaceDialogSink frDialog = 
        new GridFindReplaceDialogSink(gridGroupingControl1.TableControl);
    
    GridFindTextOptions options = GridFindTextOptions.WholeTable;
    object locInfo = GridRangeInfo.Table();
    
    GridFindReplaceEventArgs frEvents = 
        new GridFindReplaceEventArgs(txtSearch.Text, "", options, locInfo);
    
    // Find all matches
    GridRangeInfo result;
    while ((result = (GridRangeInfo)frDialog.Find(frEvents)) != null)
    {
        highlightedCells.Add(result);
    }
    
    // Highlight found cells
    gridGroupingControl1.QueryCellStyleInfo += HighlightFoundCells;
    gridGroupingControl1.TableControl.Invalidate();
    
    MessageBox.Show($"Found {highlightedCells.Count} matches");
}

void HighlightFoundCells(object sender, GridTableCellStyleInfoEventArgs e)
{
    GridRangeInfo cellRange = GridRangeInfo.Cell(e.TableCellIdentity.RowIndex, 
        e.TableCellIdentity.ColIndex);
    
    if (highlightedCells.Contains(cellRange))
    {
        e.Style.BackColor = Color.Yellow;
        e.Style.TextColor = Color.Black;
    }
}
```

### Scenario 2: Context-Sensitive Tooltips

```csharp
gridGroupingControl1.QueryCellStyleInfo += GridGroupingControl1_QueryCellStyleInfo;

void GridGroupingControl1_QueryCellStyleInfo(object sender, 
    GridTableCellStyleInfoEventArgs e)
{
    var column = e.Style.TableCellIdentity.Column;
    
    if (column != null && e.TableCellIdentity.DisplayElement?.Kind == DisplayElementKind.Record)
    {
        string columnName = column.Name;
        string cellValue = e.Style.Text;
        
        switch (columnName)
        {
            case "Status":
                // Detailed status explanation
                e.Style.CellTipText = GetStatusDescription(cellValue);
                break;
                
            case "Amount":
                // Show formatted currency with details
                if (decimal.TryParse(cellValue, out decimal amount))
                {
                    e.Style.CellTipText = 
                        $"Amount: {amount:C}\nTax: {amount * 0.08m:C}\nTotal: {amount * 1.08m:C}";
                }
                break;
                
            case "LastModified":
                // Show relative time
                if (DateTime.TryParse(cellValue, out DateTime date))
                {
                    TimeSpan elapsed = DateTime.Now - date;
                    e.Style.CellTipText = 
                        $"Modified: {date:g}\n({FormatTimeSpan(elapsed)} ago)";
                }
                break;
        }
    }
}
```

### Scenario 3: Zoomed Detail View on Hover

```csharp
private ZoomGroupingGrid zoom;

public MyForm()
{
    InitializeComponent();
    
    // Initialize zoom
    zoom = new ZoomGroupingGrid(gridGroupingControl1);
    zoom.ZoomImageMode = ZoomGroupingGrid.ImageMode.Ellipse;
    zoom.ZoomSize = new Size(300, 300);
    zoom.ZoomFactor = 2.0f;
    zoom.ZoomBorderColor = Color.DodgerBlue;
    zoom.ZoomBorderSize = 3;
    
    // Enable cell zoom
    ZoomGroupingGrid.zoomCell = true;
}

// User clicks cell → Magnified ellipse view appears
// Useful for grids with small text or images
```

### Scenario 4: Merged Header Rows

```csharp
// Create custom header with merged cells
gridGroupingControl1.TableControl.Model.RowCount += 2; // Add header rows

// Merge cells for "Customer Information" spanning columns 1-3
gridGroupingControl1.TableModel.CoveredRanges.Add(
    GridRangeInfo.Cells(1, 1, 1, 3));

// Merge cells for "Order Details" spanning columns 4-6
gridGroupingControl1.TableModel.CoveredRanges.Add(
    GridRangeInfo.Cells(1, 4, 1, 6));

// Set header text
gridGroupingControl1.QueryCellStyleInfo += (s, e) => {
    if (e.TableCellIdentity.RowIndex == 1)
    {
        if (e.TableCellIdentity.ColIndex == 1)
        {
            e.Style.Text = "Customer Information";
            e.Style.Font.Bold = true;
            e.Style.HorizontalAlignment = GridHorizontalAlignment.Center;
        }
        else if (e.TableCellIdentity.ColIndex == 4)
        {
            e.Style.Text = "Order Details";
            e.Style.Font.Bold = true;
            e.Style.HorizontalAlignment = GridHorizontalAlignment.Center;
        }
    }
};
```

### Scenario 5: Replace with Confirmation

```csharp
void ReplaceWithConfirmation()
{
    GridFindReplaceDialogSink frDialog = 
        new GridFindReplaceDialogSink(gridGroupingControl1.TableControl);
    
    GridFindTextOptions options = GridFindTextOptions.WholeTable;
    object locInfo = GridRangeInfo.Table();
    
    GridFindReplaceEventArgs frEvents = 
        new GridFindReplaceEventArgs(txtSearch.Text, "", options, locInfo);
    
    int replacedCount = 0;
    GridRangeInfo result;
    
    while ((result = (GridRangeInfo)frDialog.Find(frEvents)) != null)
    {
        // Highlight found cell
        gridGroupingControl1.TableControl.ScrollCellInView(result.Top, result.Left);
        
        // Confirm replacement
        DialogResult confirm = MessageBox.Show(
            $"Replace '{txtSearch.Text}' with '{txtReplace.Text}'?",
            "Confirm Replace",
            MessageBoxButtons.YesNoCancel);
        
        if (confirm == DialogResult.Yes)
        {
            frEvents.ReplaceString = txtReplace.Text;
            frDialog.Replace(frEvents);
            replacedCount++;
        }
        else if (confirm == DialogResult.Cancel)
        {
            break;
        }
        // No = skip this match, continue to next
    }
    
    MessageBox.Show($"Replaced {replacedCount} occurrence(s)");
}
```

## Best Practices

### Find and Replace

1. **Provide keyboard shortcuts**: Ctrl+F for Find, Ctrl+H for Replace (Excel convention)

2. **Validate search text**: Don't allow empty search strings

3. **Show match count**: Inform user how many matches found

4. **Highlight current match**: Scroll found cell into view and select it

5. **Case sensitivity**: Default to case-insensitive, provide checkbox for case-sensitive

### Tooltips

1. **Keep tooltips concise**: 1-3 lines maximum for readability

2. **Avoid redundant tooltips**: Don't show tooltip with same text as cell content

3. **Use for truncated text**: Show full content in tooltip when cell text is truncated

4. **Context-sensitive**: Provide additional information not visible in cell (formulas, metadata, help text)

5. **Adjust delays**: Shorter delays (500ms) for frequently accessed tooltips

### Zooming

1. **Provide zoom controls**: Slider or +/- buttons for grid-level zoom

2. **Persist zoom level**: Save user's preferred zoom level in settings

3. **Document cell zoom**: Inform users that clicking cell magnifies it

4. **Limit zoom factor**: Don't exceed 4.0 (400%) to avoid pixelation

5. **Match zoom mode to data**: Ellipse for images/icons, Rectangle for text-heavy cells

### Covered Ranges

1. **Avoid with sorting**: Don't use covered ranges in sortable tables (ranges don't move with data)

2. **Document behavior**: Explain that merged cells remain at fixed positions

3. **Use for static layouts**: Headers, summary rows, custom report layouts

4. **Center content**: Horizontally center text in merged cells for better appearance:
   ```csharp
   style.HorizontalAlignment = GridHorizontalAlignment.Center;
   ```

5. **Prevent editing**: Make covered range cells read-only to avoid confusion:
   ```csharp
   style.ReadOnly = true;
   ```
