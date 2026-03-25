# Clipboard Operations

## Table of Contents
- [Overview](#overview)
- [Copy Operations](#copy-operations)
- [Paste Operations](#paste-operations)
- [Cut Operations](#cut-operations)
- [Clipboard Events](#clipboard-events)
- [Common Scenarios](#common-scenarios)
- [Best Practices](#best-practices)

## Overview

GridGroupingControl provides built-in clipboard support for Copy, Cut, and Paste operations. Operations work with selected cells, ranges, and complete records with both keyboard shortcuts and programmatic APIs.

### Key Components

- **TableModel.CutPaste** - Clipboard operations controller
- **Copy()** - Copy selection to clipboard
- **Paste()** - Paste clipboard content to grid
- **Cut()** - Cut selection to clipboard
- **Clipboard Events** - Validate and customize operations

## Copy Operations

### Default Copy (Ctrl+C)

```csharp
// User presses Ctrl+C
// Copies selected cells/ranges to clipboard
// Format: Tab-separated values (TSV)
```

### Programmatic Copy

```csharp
// Copy current selection to clipboard
gridGroupingControl1.TableModel.CutPaste.Copy();
```

### Copy Specific Range

```csharp
// Copy specific cell range
GridRangeInfo range = GridRangeInfo.Cells(3, 3, 5, 5);  // Rows 3-5, Cols 3-5
gridGroupingControl1.TableModel.CutPaste.CopyRange(range);
```

### Copy Formatted Text

```csharp
// Copy with formatting preserved
GridCurrentCell cc = gridGroupingControl1.TableControl.CurrentCell;
GridRangeInfoList rangeList = new GridRangeInfoList();

// Add ranges to copy
rangeList.Add(GridRangeInfo.Cells(3, 3, 4, 4));
rangeList.Add(GridRangeInfo.Cell(cc.RowIndex, cc.ColIndex));

// Copy formatted text
gridGroupingControl1.TableModel.CutPaste.CopyTextToClipboard(rangeList);
```

### Copy Current Cell Only

```csharp
// When current cell is in edit mode, only cell text is copied
// To explicitly copy current cell:
GridCurrentCell cc = gridGroupingControl1.TableControl.CurrentCell;
string cellText = cc.Renderer.ControlText;
Clipboard.SetText(cellText);
```

### Copy Group Records

```csharp
// Copy all records from current group
GridCurrentCell cc = gridGroupingControl1.TableControl.CurrentCell;
GridTableCellStyleInfo style = gridGroupingControl1.TableControl.GetTableViewStyleInfo(cc.RowIndex, cc.ColIndex);
Element el = style.TableCellIdentity.DisplayElement;

string groupData = CopyGroup(el.ParentGroup);
Clipboard.SetText(groupData);

string CopyGroup(Group g)
{
    StringBuilder sb = new StringBuilder();
    
    if (g.Records != null && g.Records.Count > 0)
    {
        foreach (Record r in g.Records)
        {
            foreach (FieldDescriptor descriptor in gridGroupingControl1.TableDescriptor.Fields)
            {
                sb.Append(r.GetValue(descriptor).ToString());
                sb.Append("\t");
            }
            sb.AppendLine();
        }
    }
    
    return sb.ToString();
}
```

## Paste Operations

### Default Paste (Ctrl+V)

```csharp
// User presses Ctrl+V
// Pastes clipboard content starting at current cell
// Overwrites existing cell values
```

### Programmatic Paste

```csharp
// Paste clipboard content at current cell
gridGroupingControl1.TableModel.CutPaste.Paste();
```

### Paste at Specific Location

```csharp
// Move current cell to target location
int targetRow = 5;
int targetCol = 2;
gridGroupingControl1.TableControl.CurrentCell.MoveTo(targetRow, targetCol);

// Paste
gridGroupingControl1.TableModel.CutPaste.Paste();
```

### Paste Record

```csharp
// Copy record to clipboard
int sourceRecordIndex = 3;
StringBuilder sb = new StringBuilder();

foreach (FieldDescriptor descriptor in gridGroupingControl1.TableDescriptor.Fields)
{
    sb.Append(gridGroupingControl1.Table.Records[sourceRecordIndex].GetValue(descriptor).ToString());
    sb.Append("\t");
}

Clipboard.SetText(sb.ToString());

// Paste to target record
int targetRowIndex = gridGroupingControl1.Table.CurrentRecord.GetRowIndex();
int firstColIndex = gridGroupingControl1.TableDescriptor.FieldToColIndex(0);

// Position at first column of target record
gridGroupingControl1.TableControl.CurrentCell.MoveTo(targetRowIndex, firstColIndex);

// Paste
gridGroupingControl1.TableModel.CutPaste.Paste();
```

### Paste with Validation

```csharp
// Validate clipboard content before pasting
if (Clipboard.ContainsText())
{
    string clipboardText = Clipboard.GetText();
    
    // Validate format
    if (IsValidData(clipboardText))
    {
        gridGroupingControl1.TableModel.CutPaste.Paste();
    }
    else
    {
        MessageBox.Show("Invalid clipboard data format");
    }
}

bool IsValidData(string data)
{
    // Check if data matches expected format
    // Return true if valid, false otherwise
    return !string.IsNullOrWhiteSpace(data);
}
```

## Cut Operations

### Default Cut (Ctrl+X)

```csharp
// User presses Ctrl+X
// Copies selection to clipboard
// Clears selected cells
```

### Programmatic Cut

```csharp
// Cut selected cells
gridGroupingControl1.TableModel.CutPaste.Cut();
```

### Cut Specific Range

```csharp
// Cut specific range
GridRangeInfo range = GridRangeInfo.Cells(3, 3, 4, 4);
gridGroupingControl1.TableModel.CutPaste.CutRange(range, true);  // true = clear cells
```

### Cut Without Clearing

```csharp
// Copy to clipboard but don't clear cells
GridRangeInfo range = GridRangeInfo.Cells(3, 3, 4, 4);
gridGroupingControl1.TableModel.CutPaste.CutRange(range, false);  // false = keep cells
```

## Clipboard Events

Control and customize clipboard operations using events.

### ClipboardCanCopy

Determine if copy operation is allowed:

```csharp
gridGroupingControl1.TableModel.ClipboardCanCopy += TableModel_ClipboardCanCopy;

void TableModel_ClipboardCanCopy(object sender, GridCutPasteEventArgs e)
{
    // Restrict copying sensitive columns
    GridRangeInfoList ranges = e.RangeList;
    foreach (GridRangeInfo range in ranges)
    {
        for (int col = range.Left; col <= range.Right; col++)
        {
            string columnName = gridGroupingControl1.TableControl.Model[1, col].CellIdentity.Column?.Name;
            if (columnName == "Password" || columnName == "SSN")
            {
                e.Cancel = true;
                MessageBox.Show("Cannot copy sensitive data");
                return;
            }
        }
    }
}
```

### ClipboardCopy

Customize copy operation:

```csharp
gridGroupingControl1.TableModel.ClipboardCopy += TableModel_ClipboardCopy;

void TableModel_ClipboardCopy(object sender, GridCutPasteEventArgs e)
{
    // Custom copy logic
    // e.Handled = true prevents default copy behavior
    
    // Example: Add header row
    StringBuilder sb = new StringBuilder();
    
    // Add headers
    foreach (FieldDescriptor field in gridGroupingControl1.TableDescriptor.Fields)
    {
        sb.Append(field.Name);
        sb.Append("\t");
    }
    sb.AppendLine();
    
    // Let default copy add data
    e.Handled = false;  // Continue with default copy
}
```

### ClipboardCanPaste

Validate before pasting:

```csharp
gridGroupingControl1.TableModel.ClipboardCanPaste += TableModel_ClipboardCanPaste;

void TableModel_ClipboardCanPaste(object sender, GridCutPasteEventArgs e)
{
    if (!Clipboard.ContainsText())
    {
        e.Cancel = true;
        return;
    }
    
    // Check if target cells are read-only
    GridCurrentCell cc = gridGroupingControl1.TableControl.CurrentCell;
    GridStyleInfo style = gridGroupingControl1.TableControl.Model[cc.RowIndex, cc.ColIndex];
    
    if (style.ReadOnly)
    {
        e.Cancel = true;
        MessageBox.Show("Cannot paste into read-only cells");
    }
}
```

### ClipboardPaste

Customize paste operation:

```csharp
gridGroupingControl1.TableModel.ClipboardPaste += TableModel_ClipboardPaste;

void TableModel_ClipboardPaste(object sender, GridCutPasteEventArgs e)
{
    // Validate data before pasting
    string clipboardText = Clipboard.GetText();
    
    if (!ValidateClipboardData(clipboardText))
    {
        e.Handled = true;  // Cancel paste
        MessageBox.Show("Invalid data format");
        return;
    }
    
    // Log paste operation
    LogOperation("Paste", clipboardText.Length);
}
```

### ClipboardPasted

Post-paste processing:

```csharp
gridGroupingControl1.TableModel.ClipboardPasted += TableModel_ClipboardPasted;

void TableModel_ClipboardPasted(object sender, GridCutPasteEventArgs e)
{
    // Refresh calculations after paste
    gridGroupingControl1.TableControl.Refresh();
    
    // Update status
    MessageBox.Show("Data pasted successfully");
}
```

### ClipboardCanCut

Control cut operation:

```csharp
gridGroupingControl1.TableModel.ClipboardCanCut += TableModel_ClipboardCanCut;

void TableModel_ClipboardCanCut(object sender, GridCutPasteEventArgs e)
{
    // Prevent cutting from read-only cells
    GridCurrentCell cc = gridGroupingControl1.TableControl.CurrentCell;
    
    if (gridGroupingControl1.TableModel.ReadOnly)
    {
        e.Cancel = true;
        MessageBox.Show("Grid is read-only");
    }
}
```

### ClipboardCut

Customize cut operation:

```csharp
gridGroupingControl1.TableModel.ClipboardCut += TableModel_ClipboardCut;

void TableModel_ClipboardCut(object sender, GridCutPasteEventArgs e)
{
    // Log cut operation
    LogOperation("Cut", e.RangeList.Count);
    
    // Default cut behavior continues
}
```

### Ignore Current Cell in Copy

```csharp
gridGroupingControl1.TableModel.ClipboardCanCopy += (s, e) =>
{
    // Exclude current cell from copy
    e.IgnoreCurrentCell = true;
};
```

## Common Scenarios

### Scenario 1: Copy with Headers

```csharp
// Copy selection with column headers
void CopyWithHeaders()
{
    GridRangeInfoList selectedRanges = gridGroupingControl1.TableControl.Selections.GetSelectedRanges();
    
    if (selectedRanges.Count == 0)
    {
        MessageBox.Show("No selection");
        return;
    }
    
    StringBuilder sb = new StringBuilder();
    GridRangeInfo range = selectedRanges[0];
    
    // Add headers
    for (int col = range.Left; col <= range.Right; col++)
    {
        string columnName = gridGroupingControl1.TableControl.Model[1, col].CellIdentity.Column?.Name ?? "";
        sb.Append(columnName);
        if (col < range.Right) sb.Append("\t");
    }
    sb.AppendLine();
    
    // Add data
    for (int row = range.Top; row <= range.Bottom; row++)
    {
        for (int col = range.Left; col <= range.Right; col++)
        {
            string cellValue = gridGroupingControl1.TableControl.Model[row, col].FormattedText;
            sb.Append(cellValue);
            if (col < range.Right) sb.Append("\t");
        }
        sb.AppendLine();
    }
    
    Clipboard.SetText(sb.ToString());
    MessageBox.Show("Copied with headers");
}
```

### Scenario 2: Copy Entire Table

```csharp
// Copy all visible data to clipboard
void CopyEntireTable()
{
    StringBuilder sb = new StringBuilder();
    
    // Headers
    foreach (GridColumnDescriptor col in gridGroupingControl1.TableDescriptor.Columns)
    {
        if (gridGroupingControl1.TableDescriptor.VisibleColumns.Contains(col.Name))
        {
            sb.Append(col.HeaderText);
            sb.Append("\t");
        }
    }
    sb.AppendLine();
    
    // Records
    foreach (Record record in gridGroupingControl1.Table.Records)
    {
        foreach (GridColumnDescriptor col in gridGroupingControl1.TableDescriptor.Columns)
        {
            if (gridGroupingControl1.TableDescriptor.VisibleColumns.Contains(col.Name))
            {
                sb.Append(record.GetValue(col.MappingName)?.ToString() ?? "");
                sb.Append("\t");
            }
        }
        sb.AppendLine();
    }
    
    Clipboard.SetText(sb.ToString());
    MessageBox.Show($"Copied {gridGroupingControl1.Table.Records.Count} records");
}
```

### Scenario 3: Paste with Validation

```csharp
gridGroupingControl1.TableModel.ClipboardPaste += (s, e) =>
{
    string clipboardData = Clipboard.GetText();
    string[] lines = clipboardData.Split(new[] { "\r\n", "\n" }, StringSplitOptions.RemoveEmptyEntries);
    
    GridCurrentCell cc = gridGroupingControl1.TableControl.CurrentCell;
    int startRow = cc.RowIndex;
    int startCol = cc.ColIndex;
    
    // Validate each cell before pasting
    for (int i = 0; i < lines.Length; i++)
    {
        string[] cells = lines[i].Split('\t');
        
        for (int j = 0; j < cells.Length; j++)
        {
            string columnName = gridGroupingControl1.TableControl.Model[1, startCol + j].CellIdentity.Column?.Name;
            
            if (columnName == "Age" && !int.TryParse(cells[j], out _))
            {
                e.Handled = true;
                MessageBox.Show($"Invalid age value: {cells[j]}");
                return;
            }
            
            if (columnName == "Email" && !cells[j].Contains("@"))
            {
                e.Handled = true;
                MessageBox.Show($"Invalid email: {cells[j]}");
                return;
            }
        }
    }
};
```

### Scenario 4: Copy Selected Records Only

```csharp
// Copy only selected records (record-based selection)
void CopySelectedRecords()
{
    if (gridGroupingControl1.Table.SelectedRecords.Count == 0)
    {
        MessageBox.Show("No records selected");
        return;
    }
    
    StringBuilder sb = new StringBuilder();
    
    // Headers
    foreach (FieldDescriptor field in gridGroupingControl1.TableDescriptor.Fields)
    {
        sb.Append(field.Name);
        sb.Append("\t");
    }
    sb.AppendLine();
    
    // Selected records
    foreach (Record record in gridGroupingControl1.Table.SelectedRecords)
    {
        foreach (FieldDescriptor field in gridGroupingControl1.TableDescriptor.Fields)
        {
            sb.Append(record.GetValue(field.Name)?.ToString() ?? "");
            sb.Append("\t");
        }
        sb.AppendLine();
    }
    
    Clipboard.SetText(sb.ToString());
    MessageBox.Show($"Copied {gridGroupingControl1.Table.SelectedRecords.Count} selected records");
}
```

## Best Practices

### Data Validation

1. **Validate on Paste**: Use `ClipboardCanPaste` to check data format before allowing paste.

2. **Type Checking**: Verify pasted data matches column data types:
   ```csharp
   if (columnType == typeof(int) && !int.TryParse(value, out _))
   {
       // Reject paste
   }
   ```

3. **Range Checking**: Validate numeric values fall within acceptable ranges.

### Performance

1. **Large Copy Operations**: For large selections, show progress indicator:
   ```csharp
   void CopyLargeSelection()
   {
       Cursor = Cursors.WaitCursor;
       try
       {
           gridGroupingControl1.TableModel.CutPaste.Copy();
       }
       finally
       {
           Cursor = Cursors.Default;
       }
   }
   ```

2. **Batch Paste**: Suspend grid updates during large paste operations:
   ```csharp
   gridGroupingControl1.BeginUpdate();
   try
   {
       gridGroupingControl1.TableModel.CutPaste.Paste();
   }
   finally
   {
       gridGroupingControl1.EndUpdate();
   }
   ```

### User Experience

1. **Feedback**: Show messages for successful operations:
   ```csharp
   statusLabel.Text = "10 cells copied to clipboard";
   ```

2. **Keyboard Shortcuts**: Document Ctrl+C, Ctrl+V, Ctrl+X in user manual.

3. **Context Menu**: Add Copy/Paste to right-click menu:
   ```csharp
   ContextMenuStrip menu = new ContextMenuStrip();
   menu.Items.Add("Copy", null, (s, e) => gridGroupingControl1.TableModel.CutPaste.Copy());
   menu.Items.Add("Paste", null, (s, e) => gridGroupingControl1.TableModel.CutPaste.Paste());
   gridGroupingControl1.TableControl.ContextMenuStrip = menu;
   ```

### Security

1. **Sensitive Data**: Prevent copying sensitive columns (passwords, SSNs):
   ```csharp
   gridGroupingControl1.TableModel.ClipboardCanCopy += (s, e) =>
   {
       // Check if sensitive columns are in selection
       // Cancel if found
   };
   ```

2. **Read-Only Enforcement**: Respect read-only cells in paste operations.

3. **Audit Logging**: Log copy/paste operations for security audits.

### Error Handling

- Wrap clipboard operations in try-catch (clipboard can fail if locked by another app)
- Provide meaningful error messages to users
- Validate clipboard format matches expected data structure
