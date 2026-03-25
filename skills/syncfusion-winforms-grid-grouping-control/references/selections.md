# Selections

## Table of Contents
- [Overview](#overview)
- [Model-Based Selection](#model-based-selection)
- [Record-Based Selection](#record-based-selection)
- [Selection Appearance](#selection-appearance)
- [Programmatic Selection](#programmatic-selection)
- [Common Scenarios](#common-scenarios)
- [Best Practices](#best-practices)

## Overview

GridGroupingControl provides two selection architectures: Model-Based (cell-range selection) and Record-Based (whole-record selection). Choose based on whether selections need to be aware of grouping, sorting, and nested tables.

### Key Components

- **AllowSelection** - Enables model-based (cell) selection
- **ListBoxSelectionMode** - Enables record-based selection
- **SelectedRecords** - Collection of selected records
- **SelectedRanges** - Collection of selected cell ranges
- **SelectionBackColor/TextColor** - Selection appearance

## Model-Based Selection

Cell-based selection inherited from GridControlBase. **Not aware** of grouping, nested tables, or record structure. Best for spreadsheet-like scenarios.

### Enable Model-Based Selection

```csharp
// Must disable record-based selection first
gridGroupingControl1.TableOptions.ListBoxSelectionMode = SelectionMode.None;

// Enable cell selection
gridGroupingControl1.TableOptions.AllowSelection = GridSelectionFlags.Any;
```

### Selection Flags

Combine flags to customize selection behavior:

```csharp
using Syncfusion.Windows.Forms.Grid;

// Individual cells
gridGroupingControl1.TableOptions.AllowSelection = GridSelectionFlags.Cell;

// Entire rows
gridGroupingControl1.TableOptions.AllowSelection = GridSelectionFlags.Row;

// Entire columns
gridGroupingControl1.TableOptions.AllowSelection = GridSelectionFlags.Column;

// Entire table
gridGroupingControl1.TableOptions.AllowSelection = GridSelectionFlags.Table;

// Multiple selections with Ctrl
gridGroupingControl1.TableOptions.AllowSelection = 
    GridSelectionFlags.Cell | 
    GridSelectionFlags.Multiple | 
    GridSelectionFlags.MixRangeType;

// Extend selection with Shift
gridGroupingControl1.TableOptions.AllowSelection = 
    GridSelectionFlags.Row | 
    GridSelectionFlags.Shift;

// Extend with Shift+Arrow keys
gridGroupingControl1.TableOptions.AllowSelection = 
    GridSelectionFlags.Cell | 
    GridSelectionFlags.Keyboard;

// Alpha blending for selection
gridGroupingControl1.TableOptions.AllowSelection = 
    GridSelectionFlags.Row | 
    GridSelectionFlags.AlphaBlend;

// Rows and columns simultaneously
gridGroupingControl1.TableOptions.AllowSelection = 
    GridSelectionFlags.Row | 
    GridSelectionFlags.Column | 
    GridSelectionFlags.Multiple;
```

### GridSelectionFlags Options

| Flag | Description |
|------|-------------|
| None | Disable selections |
| Cell | Select individual cells |
| Row | Select entire rows |
| Column | Select entire columns |
| Table | Select entire table |
| Shift | Extend selection with Shift+Click |
| Multiple | Select multiple ranges with Ctrl |
| MixRangeType | Mix different selection types (rows + cells) |
| Keyboard | Extend with Shift+Arrow keys |
| AlphaBlend | Use alpha blending for selection highlight |
| Any | Default: All flags enabled |

### Customize Alpha Blend Color

```csharp
// Enable alpha blending
gridGroupingControl1.TableOptions.AllowSelection = 
    GridSelectionFlags.AlphaBlend | GridSelectionFlags.Cell;

// Set custom alpha blend color
gridGroupingControl1.TableModel.Options.AlphaBlendSelectionColor = Color.Red;
```

## Record-Based Selection

Whole-record selection designed for GridGroupingControl. **Aware** of grouping, sorting, nested tables. Affects `Table.SelectedRecords` collection.

### Enable Record-Based Selection

```csharp
// ListBoxSelectionMode.One - Select single record
gridGroupingControl1.TableOptions.ListBoxSelectionMode = SelectionMode.One;

// ListBoxSelectionMode.MultiSimple - Multiple records, no Shift/Ctrl
gridGroupingControl1.TableOptions.ListBoxSelectionMode = SelectionMode.MultiSimple;

// ListBoxSelectionMode.MultiExtended - Multiple with Shift/Ctrl/Arrow keys
gridGroupingControl1.TableOptions.ListBoxSelectionMode = SelectionMode.MultiExtended;

// Disable record selection
gridGroupingControl1.TableOptions.ListBoxSelectionMode = SelectionMode.None;
```

### SelectionMode.One

Select only one record at a time:

```csharp
gridGroupingControl1.TableOptions.ListBoxSelectionMode = SelectionMode.One;

// Only one record selected at any time
// Clicking another record deselects previous
```

### SelectionMode.MultiSimple

Select multiple records individually without keyboard modifiers:

```csharp
gridGroupingControl1.TableOptions.ListBoxSelectionMode = SelectionMode.MultiSimple;

// Click records to toggle selection
// No Shift/Ctrl/Arrow key support
// Each click toggles selection state of clicked record
```

### SelectionMode.MultiExtended

Full keyboard and mouse support for multi-selection:

```csharp
gridGroupingControl1.TableOptions.ListBoxSelectionMode = SelectionMode.MultiExtended;

// Click: Select single record
// Ctrl+Click: Add/remove record from selection
// Shift+Click: Select range from last selected to clicked
// Shift+Arrow: Extend selection with keyboard
```

### Keyboard Selection

With `SelectionMode.MultiExtended`:

- **Up/Down Arrow**: Navigate records
- **Shift+Up/Down**: Extend selection
- **Ctrl+Click**: Add individual records
- **Shift+Click**: Select range
- **Ctrl+A**: Select all records
- **Spacebar**: Toggle selection of current record

## Selection Appearance

### Appearance Options

Control how selections are displayed:

```csharp
// Apply custom back color and text color
gridGroupingControl1.TableOptions.ListBoxSelectionColorOptions = 
    GridListBoxSelectionColorOptions.ApplySelectionColor;
gridGroupingControl1.TableOptions.SelectionBackColor = Color.LightBlue;
gridGroupingControl1.TableOptions.SelectionTextColor = Color.DarkBlue;

// Alpha blending over selection
gridGroupingControl1.TableOptions.ListBoxSelectionColorOptions = 
    GridListBoxSelectionColorOptions.DrawAlphablend;

// Invert cell colors (BackColor ↔ TextColor)
gridGroupingControl1.TableOptions.ListBoxSelectionColorOptions = 
    GridListBoxSelectionColorOptions.InvertCells;

// No automatic coloring (handle manually in events)
gridGroupingControl1.TableOptions.ListBoxSelectionColorOptions = 
    GridListBoxSelectionColorOptions.None;
```

### GridListBoxSelectionColorOptions

| Option | Effect |
|--------|--------|
| ApplySelectionColor | Use SelectionBackColor and SelectionTextColor |
| DrawAlphablend | Draw semi-transparent overlay on selected rows |
| InvertCells | Swap BackColor and TextColor for selected cells |
| None | No automatic coloring (manual control via events) |

### Current Cell in Selection

Control current cell appearance within selected records:

```csharp
// Hide current cell border in selected row
gridGroupingControl1.TableOptions.ListBoxSelectionCurrentCellOptions = 
    GridListBoxSelectionCurrentCellOptions.HideCurrentCell;

// Show current cell with white background
gridGroupingControl1.TableOptions.ListBoxSelectionCurrentCellOptions = 
    GridListBoxSelectionCurrentCellOptions.WhiteCurrentCell;

// Show current cell with same selection color
gridGroupingControl1.TableOptions.ListBoxSelectionCurrentCellOptions = 
    GridListBoxSelectionCurrentCellOptions.None;

// Move current cell when extending selection with mouse
gridGroupingControl1.TableOptions.ListBoxSelectionCurrentCellOptions = 
    GridListBoxSelectionCurrentCellOptions.MoveCurrentCellWithMouse;
```

## Programmatic Selection

### Select Records

```csharp
// Select single record
Record record = gridGroupingControl1.Table.Records[2];
gridGroupingControl1.Table.SelectedRecords.Add(record);

// Select multiple records
foreach (Record r in gridGroupingControl1.Table.Records)
{
    if (r.GetValue("Status").ToString() == "Active")
    {
        gridGroupingControl1.Table.SelectedRecords.Add(r);
    }
}

// Select all records
foreach (Record r in gridGroupingControl1.Table.Records)
{
    gridGroupingControl1.Table.SelectedRecords.Add(r);
}

// Clear selection
gridGroupingControl1.Table.SelectedRecords.Clear();
```

### Check Selection

```csharp
// Check if record is selected
Record record = gridGroupingControl1.Table.Records[0];
bool isSelected = gridGroupingControl1.Table.SelectedRecords.Contains(record);

// Get selected record count
int count = gridGroupingControl1.Table.SelectedRecords.Count;

// Iterate selected records
foreach (Record selectedRecord in gridGroupingControl1.Table.SelectedRecords)
{
    string customerName = selectedRecord.GetValue("CustomerName").ToString();
    Console.WriteLine($"Selected: {customerName}");
}
```

### Select Cell Ranges (Model-Based)

```csharp
// Select cell range
GridRangeInfo range = GridRangeInfo.Cells(3, 2, 7, 5);  // Top row, left col, bottom row, right col
gridGroupingControl1.TableControl.Selections.Add(range);

// Select entire row
GridRangeInfo rowRange = GridRangeInfo.Row(5);
gridGroupingControl1.TableControl.Selections.Add(rowRange);

// Select entire column
GridRangeInfo colRange = GridRangeInfo.Col(3);
gridGroupingControl1.TableControl.Selections.Add(colRange);

// Clear selections
gridGroupingControl1.TableControl.Selections.Clear();

// Get selected ranges
GridRangeInfoList selections = gridGroupingControl1.TableControl.Selections.GetSelectedRanges();
foreach (GridRangeInfo r in selections)
{
    Console.WriteLine($"Selected: {r}");
}
```

## Common Scenarios

### Scenario 1: Single-Select Listbox Style

```csharp
// Configure grid as single-select listbox
gridGroupingControl1.TableOptions.ListBoxSelectionMode = SelectionMode.One;
gridGroupingControl1.TableOptions.ListBoxSelectionColorOptions = 
    GridListBoxSelectionColorOptions.ApplySelectionColor;
gridGroupingControl1.TableOptions.SelectionBackColor = SystemColors.Highlight;
gridGroupingControl1.TableOptions.SelectionTextColor = SystemColors.HighlightText;

// Hide current cell border
gridGroupingControl1.TableOptions.ListBoxSelectionCurrentCellOptions = 
    GridListBoxSelectionCurrentCellOptions.HideCurrentCell;

// Access selected record
if (gridGroupingControl1.Table.SelectedRecords.Count > 0)
{
    Record selected = gridGroupingControl1.Table.SelectedRecords[0];
    string customerID = selected.GetValue("CustomerID").ToString();
}
```

### Scenario 2: Multi-Select with Checkboxes

```csharp
// Enable multi-selection
gridGroupingControl1.TableOptions.ListBoxSelectionMode = SelectionMode.MultiExtended;

// Custom selection appearance
gridGroupingControl1.TableOptions.ListBoxSelectionColorOptions = 
    GridListBoxSelectionColorOptions.ApplySelectionColor;
gridGroupingControl1.TableOptions.SelectionBackColor = Color.LightGreen;

// Add "Select All" button
void btnSelectAll_Click(object sender, EventArgs e)
{
    gridGroupingControl1.Table.SelectedRecords.Clear();
    foreach (Record r in gridGroupingControl1.Table.Records)
    {
        gridGroupingControl1.Table.SelectedRecords.Add(r);
    }
}

// Process selected records
void btnProcessSelected_Click(object sender, EventArgs e)
{
    if (gridGroupingControl1.Table.SelectedRecords.Count == 0)
    {
        MessageBox.Show("No records selected");
        return;
    }

    foreach (Record record in gridGroupingControl1.Table.SelectedRecords)
    {
        // Process each selected record
        int id = Convert.ToInt32(record.GetValue("ID"));
        ProcessRecord(id);
    }
}
```

### Scenario 3: Excel-Style Cell Selection

```csharp
// Configure Excel-style cell selection
gridGroupingControl1.TableOptions.ListBoxSelectionMode = SelectionMode.None;
gridGroupingControl1.TableOptions.AllowSelection = 
    GridSelectionFlags.Cell | 
    GridSelectionFlags.Multiple | 
    GridSelectionFlags.Shift | 
    GridSelectionFlags.Keyboard | 
    GridSelectionFlags.AlphaBlend;

gridGroupingControl1.TableModel.Options.AlphaBlendSelectionColor = Color.FromArgb(100, Color.Blue);

// Copy selected cells to clipboard
void btnCopy_Click(object sender, EventArgs e)
{
    gridGroupingControl1.TableModel.CutPaste.Copy();
}
```

### Scenario 4: Conditional Row Selection

```csharp
// Select rows matching condition
gridGroupingControl1.TableOptions.ListBoxSelectionMode = SelectionMode.MultiExtended;

void btnSelectActiveCustomers_Click(object sender, EventArgs e)
{
    gridGroupingControl1.Table.SelectedRecords.Clear();
    
    foreach (Record record in gridGroupingControl1.Table.Records)
    {
        string status = record.GetValue("Status").ToString();
        if (status == "Active")
        {
            gridGroupingControl1.Table.SelectedRecords.Add(record);
        }
    }
    
    MessageBox.Show($"{gridGroupingControl1.Table.SelectedRecords.Count} active customers selected");
}
```

### Scenario 5: Focused Selection (Highlight Row and Column)

```csharp
// Highlight entire row and column of current cell
gridGroupingControl1.TableOptions.ListBoxSelectionMode = SelectionMode.None;
gridGroupingControl1.TableOptions.AllowSelection = GridSelectionFlags.None;

gridGroupingControl1.TableControlCurrentCellActivated += (s, e) =>
{
    GridCurrentCell cc = e.TableControl.CurrentCell;
    
    // Clear previous selections
    e.TableControl.Selections.Clear();
    
    // Select current row
    GridRangeInfo row = GridRangeInfo.Row(cc.RowIndex);
    e.TableControl.Selections.Add(row);
    
    // Select current column
    GridRangeInfo col = GridRangeInfo.Col(cc.ColIndex);
    e.TableControl.Selections.Add(col);
    
    e.TableControl.Invalidate();
};
```

## Best Practices

### Choosing Selection Mode

1. **Use Record-Based When:**
   - Working with complete records (not individual cells)
   - Need selection to survive sorting/grouping
   - Building list-style interfaces (item selection)
   - Need `SelectedRecords` collection

2. **Use Model-Based When:**
   - Need cell-range selection (Excel-style)
   - Copying/pasting cell ranges
   - No grouping or nested tables
   - Spreadsheet-like behavior

3. **Never Mix**: Don't enable both modes simultaneously. Set one to `None`.

### Performance

1. **Limit Selection Count**: For large datasets, limit multi-selection or provide "Select All" warning.

2. **Batch Operations**: When modifying selected records, suspend updates:
   ```csharp
   gridGroupingControl1.BeginUpdate();
   foreach (Record r in gridGroupingControl1.Table.SelectedRecords)
   {
       r.SetValue("Status", "Processed");
   }
   gridGroupingControl1.EndUpdate();
   ```

3. **Clear Before Rebuild**: Clear selections before refreshing data source.

### User Experience

1. **Visual Feedback**: Use clear selection colors that contrast with cell colors:
   ```csharp
   gridGroupingControl1.TableOptions.SelectionBackColor = Color.DodgerBlue;
   gridGroupingControl1.TableOptions.SelectionTextColor = Color.White;
   ```

2. **Keyboard Support**: Enable keyboard selection for accessibility:
   ```csharp
   gridGroupingControl1.TableOptions.ListBoxSelectionMode = SelectionMode.MultiExtended;
   ```

3. **Selection Count**: Display selected record count in status bar:
   ```csharp
   gridGroupingControl1.Table.SelectedRecordsChanged += (s, e) =>
   {
       int count = gridGroupingControl1.Table.SelectedRecords.Count;
       statusLabel.Text = $"{count} record(s) selected";
   };
   ```

4. **Deselect**: Provide "Clear Selection" or click empty area to deselect:
   ```csharp
   void btnClearSelection_Click(object sender, EventArgs e)
   {
       gridGroupingControl1.Table.SelectedRecords.Clear();
   }
   ```

### Event Handling

- Use `SelectedRecordsChanged` to track record selection changes
- Use `SelectionChanging`/`SelectionChanged` for cell selection changes
- Validate selections before performing operations
- Disable UI controls when no selection exists
