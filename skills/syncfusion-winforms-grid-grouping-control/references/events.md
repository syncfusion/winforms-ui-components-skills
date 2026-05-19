# Events and Event Handling

## Table of Contents
- [Overview](#overview)
- [Cell Events](#cell-events)
- [CurrentCell Events](#currentcell-events)
- [Mouse Events](#mouse-events)
- [Table and Record Events](#table-and-record-events)
- [Common Scenarios](#common-scenarios)
- [Best Practices](#best-practices)

## Overview

GridGroupingControl exposes extensive events for intercepting user interactions, customizing behavior, and responding to data changes. Events are categorized by:

- **Cell events**: Click, double-click, drawing, button clicks
- **CurrentCell events**: Activation, deactivation, editing, validation
- **Mouse events**: Click, double-click, drag-drop, focus
- **Table/Record events**: Record added/removed, table structure changes

Use events when you need to:
- Customize cell appearance based on user interaction
- Validate data before committing
- Implement custom cell behaviors (tooltips, context menus)
- Track user navigation and selection
- Respond to data source changes

**Event subscription pattern:**
```csharp
// Subscribe to event
gridGroupingControl1.TableControl.CellClick += TableControl_CellClick;

// Event handler
void TableControl_CellClick(object sender, GridCellClickEventArgs e)
{
    // Handle event
}

// Unsubscribe (important for memory management)
gridGroupingControl1.TableControl.CellClick -= TableControl_CellClick;
```

## Cell Events

### CellClick

Fired when user clicks inside a cell:

```csharp
using Syncfusion.Windows.Forms.Grid;

gridGroupingControl1.TableControl.CellClick += TableControl_CellClick;

void TableControl_CellClick(object sender, GridCellClickEventArgs e)
{
    int rowIndex = e.RowIndex;
    int colIndex = e.ColIndex;
    MouseEventArgs mouse = e.MouseEventArgs;
    
    Console.WriteLine($"Clicked cell [{rowIndex}, {colIndex}] with {mouse.Button}");
    
    // Get cell value
    GridTableCellStyleInfo style = 
        gridGroupingControl1.TableControl.Model[rowIndex, colIndex];
    string cellValue = style.Text;
}
```

Use for:
- Custom cell actions (open detail dialog, launch calculator)
- Context-sensitive help
- Cell-level tracking/analytics

### CellDoubleClick

Fired when user double-clicks inside a cell:

```csharp
gridGroupingControl1.TableControl.CellDoubleClick += TableControl_CellDoubleClick;

void TableControl_CellDoubleClick(object sender, GridCellClickEventArgs e)
{
    // Notify the double click performed in a cell
    MessageBox.Show("Mouse Button is clicked Twice");
}
```

Use for:
- Launch detailed edit forms
- Expand/collapse nested content
- Drill-through to related data

### CellButtonClicked

Fired when user clicks a button element inside cell renderer:

```csharp
using Syncfusion.Windows.Forms.Grid;

gridGroupingControl1.TableControl.CellButtonClicked += TableControl_CellButtonClicked;

void TableControl_CellButtonClicked(object sender, GridCellButtonClickedEventArgs e)
{
    int rowIndex = e.RowIndex;
    int colIndex = e.ColIndex;
    
    // Cell has button (e.g., ComboBox dropdown, date picker calendar)
    Console.WriteLine($"Button clicked in [{rowIndex}, {colIndex}]");
}
```

Use for:
- Custom dropdown behavior
- Date picker customization
- Multi-select scenarios

### CellDrawn

Fired after grid finishes drawing each cell:

```csharp
gridGroupingControl1.TableControl.CellDrawn += TableControl_CellDrawn;

void TableControl_CellDrawn(object sender, GridDrawCellEventArgs e)
{
    // Post-processing after cell drawn
    if (e.RowIndex > 0 && e.ColIndex > 0)
    {
        // Draw custom overlay, border, icon
        Graphics g = e.Graphics;
        Rectangle bounds = e.Bounds;
        
        // Example: Draw red border for negative values
        string text = e.Style.Text;
        if (decimal.TryParse(text, out decimal value) && value < 0)
        {
            using (Pen pen = new Pen(Color.Red, 2))
            {
                g.DrawRectangle(pen, bounds);
            }
        }
    }
}
```

Use for:
- Custom cell decorations (icons, badges)
- Drawing custom graphics (sparklines, indicators)
- Post-processing visual effects

**Warning**: Avoid expensive operations (database calls, complex calculations) in CellDrawn—called frequently during scrolling.

### TableControlCellClick / TableControlCellDoubleClick / TableControlCellDrawn

Grid-level events that fire for all tables (including nested tables):

```csharp
// Grid-level cell click (fires for parent and child tables)
gridGroupingControl1.TableControl.CellClick += TableControlCellClick;

void TableControlCellClick(object sender, GridCellClickEventArgs e)
{
    // Works for main table and all nested tables
    Console.WriteLine($"Cell clicked: [{e.RowIndex}, {e.ColIndex}]");
}
```

Use when you need consistent behavior across all tables in a hierarchy.

## CurrentCell Events

### CurrentCellActivating

Fired before grid activates a cell as current cell (cancelable):

```csharp
using Syncfusion.Windows.Forms.Grid;

gridGroupingControl1.TableControl.CurrentCellActivating += 
    TableControl_CurrentCellActivating;

void TableControl_CurrentCellActivating(object sender, 
    GridCurrentCellActivatingEventArgs e)
{
    int rowIndex = e.RowIndex;
    int colIndex = e.ColIndex;
    
    // Prevent activating specific cells
    if (colIndex == 1) // First data column
    {
        e.Cancel = true; // Prevent activation
        MessageBox.Show("This column is read-only");
    }
}
```

Use for:
- Preventing cell activation based on conditions
- Implementing custom navigation rules
- Access control (disable cells for certain users)

### CurrentCellActivated

Fired after grid activates current cell:

```csharp
gridGroupingControl1.TableControl.CurrentCellActivated += 
    TableControl_CurrentCellActivated;

void TableControl_CurrentCellActivated(object sender, EventArgs e)
{
    var currentCell = gridGroupingControl1.TableControl.CurrentCell;
    int row = currentCell.RowIndex;
    int col = currentCell.ColIndex;
    
    // Update status bar with current position
    statusLabel.Text = $"Row: {row}, Column: {col}";
}
```

Use for:
- Updating status displays
- Loading related data when cell activated
- Synchronizing other UI elements

### CurrentCellChanged

Fired when user changes content of current cell:

```csharp
gridGroupingControl1.TableControl.CurrentCellChanged += 
    TableControl_CurrentCellChanged;

void TableControl_CurrentCellChanged(object sender, EventArgs e)
{
    var currentCell = gridGroupingControl1.TableControl.CurrentCell;
    string newValue = currentCell.Renderer.ControlText;
    
    Console.WriteLine($"Cell value changed to: {newValue}");
    
    // Trigger dependent calculations
    RecalculateTotals();
}
```

Use for:
- Real-time validation
- Dependent field updates
- Formula recalculation

### CurrentCellDeactivated

Fired after grid deactivates current cell:

```csharp
gridGroupingControl1.TableControl.CurrentCellDeactivated += 
    TableControl_CurrentCellDeactivated;

void TableControl_CurrentCellDeactivated(object sender, 
    GridCurrentCellDeactivatedEventArgs e)
{
    int row = e.RowIndex;
    int col = e.ColIndex;
    
    Console.WriteLine($"Cell [{row}, {col}] deactivated");
    
    // Save pending changes, clear temporary UI
}
```

Use for:
- Cleanup after cell edit
- Commit pending changes
- Reset temporary state

### CurrentCellStartEditing

Fired when current cell enters edit mode (cancelable):

```csharp
gridGroupingControl1.TableControl.CurrentCellStartEditing += 
    TableControl_CurrentCellStartEditing;

void TableControl_CurrentCellStartEditing(object sender, CancelEventArgs e)
{
    var currentCell = gridGroupingControl1.TableControl.CurrentCell;
    int colIndex = currentCell.ColIndex;
    
     // Get column using TableDescriptor
    var column = gridGroupingControl1.TableDescriptor.Columns[colIndex - 1];
    if (column.Name == "CalculatedTotal")
        {
         e.Cancel = true; // Prevent editing
        }
}
```

Use for:
- Preventing edit mode for calculated/read-only fields
- Conditional editing based on user permissions
- Pre-edit validation

### CurrentCellValidated

Fired after grid successfully validates current cell content:

```csharp
gridGroupingControl1.TableControl.CurrentCellValidated += 
    TableControl_CurrentCellValidated;

void TableControl_CurrentCellValidated(object sender, EventArgs e)
{
    var currentCell = gridGroupingControl1.TableControl.CurrentCell;
    string validatedValue = currentCell.Renderer.ControlText;
    
    Console.WriteLine($"Cell validated: {validatedValue}");
    
    // Proceed with post-validation logic
    UpdateDependentFields();
}
```

Use for:
- Post-validation processing
- Logging validated changes
- Triggering dependent updates

### CurrentCellControlGotFocus / CurrentCellControlLostFocus

Fired when in-place edit control gains/loses focus:

```csharp
gridGroupingControl1.TableControl.CurrentCellControlGotFocus += 
    TableControl_CurrentCellControlGotFocus;

void TableControl_CurrentCellControlGotFocus(object sender, ControlEventArgs e)
{
    Control editControl = e.Control;
    
    // Customize edit control
    if (editControl is TextBox textBox)
    {
        textBox.SelectAll(); // Select all text when focused
    }
}

gridGroupingControl1.TableControl.CurrentCellControlLostFocus += 
    TableControl_CurrentCellControlLostFocus;

void TableControl_CurrentCellControlLostFocus(object sender, ControlEventArgs e)
{
    // Commit changes, validate, etc.
    Console.WriteLine("Edit control lost focus");
}
```

Use for:
- Customizing edit control behavior (select all, autocomplete)
- Focus-based validation
- Auto-save on focus loss

## Mouse Events

### Click / DoubleClick

General control-level click events:

```csharp
gridGroupingControl1.TableControl.Click += TableControl_Click;

void TableControl_Click(object sender, EventArgs e)
{
    Console.WriteLine("Grid clicked");
}

gridGroupingControl1.TableControl.DoubleClick += TableControl_DoubleClick;

void TableControl_DoubleClick(object sender, EventArgs e)
{
    Console.WriteLine("Grid double-clicked");
}
```

Use for:
- General grid-level actions
- Context menu display
- Selection tracking

### DragDrop

Fired when drag-drop operation completes:

```csharp
using System.Windows.Forms;

gridGroupingControl1.TableControl.AllowDrop = true;
gridGroupingControl1.TableControl.DragDrop += TableControl_DragDrop;

void TableControl_DragDrop(object sender, DragEventArgs e)
{
    // Handle dropped data
    if (e.Data.GetDataPresent(DataFormats.Text))
    {
        string text = (string)e.Data.GetData(DataFormats.Text);
        
        // Determine drop location
        Point clientPoint = gridGroupingControl1.TableControl.PointToClient(
            new Point(e.X, e.Y));
        int rowIndex, colIndex;
        gridGroupingControl1.TableControl.PointToRowCol(
            clientPoint, out rowIndex, out colIndex);
        // Insert data at drop location
        InsertTextAt(rowIndex, colIndex, text);
    }
}
```

Use for:
- Drag-drop from external sources (Excel, file explorer)
- Reordering rows via drag-drop
- Copying data between grids

### GotFocus / LostFocus

Fired when grid control gains/loses focus:

```csharp
gridGroupingControl1.TableControl.GotFocus += TableControl_GotFocus;

void TableControl_GotFocus(object sender, EventArgs e)
{
    Console.WriteLine("Grid got focus");
    
    // Show grid-specific toolbar
    ShowGridToolbar();
}

gridGroupingControl1.TableControl.LostFocus += TableControl_LostFocus;

void TableControl_LostFocus(object sender, EventArgs e)
{
    Console.WriteLine("Grid lost focus");
    
    // Commit pending edits
    CommitCurrentEdit();
}
```

Use for:
- Showing/hiding context-sensitive UI
- Auto-saving when grid loses focus
- Keyboard shortcut management

## Table and Record Events

### RecordExpanding / RecordExpanded

Fired when expanding nested table or group:

```csharp
using Syncfusion.Grouping;

gridGroupingControl1.RecordExpanding += GridGroupingControl1_RecordExpanding;

void GridGroupingControl1_RecordExpanding(object sender, 
    RecordEventArgs e)
{
    Record record = e.Record;
    
    // Cancel expansion based on condition
    if (ShouldPreventExpansion(record))
    {
        e.Cancel = true;
    }
}

gridGroupingControl1.RecordExpanded += GridGroupingControl1_RecordExpanded;

void GridGroupingControl1_RecordExpanded(object sender, 
    RecordEventArgs e)
{
    // Load nested data on-demand
    LoadNestedData(e.Record);
}
```

Use for:
- Lazy-loading nested data
- Preventing expansion based on permissions
- Tracking user navigation patterns

### TableControlCurrentCellKeyDown / TableControlKeyDown

Keyboard events for custom key handling:

```csharp
gridGroupingControl1.TableControl.CurrentCellKeyDown += 
    TableControlCurrentCellKeyDown;

void TableControlCurrentCellKeyDown(object sender, KeyEventArgs e)
{
    // Custom keyboard shortcuts
    if (e.Control && e.KeyCode == Keys.F)
    {
        ShowFindDialog();
        e.Handled = true; // Prevent default handling
    }
}
```

Use for:
- Custom keyboard shortcuts (Ctrl+F, Ctrl+S, etc.)
- Custom navigation keys
- Preventing default key behaviors

### SourceListListChanged

Fired when underlying data source changes:

```csharp
gridGroupingControl1.SourceListListChanged += 
    GridGroupingControl1_SourceListListChanged;

void GridGroupingControl1_SourceListListChanged(object sender, 
    ListChangedEventArgs e)
{
    switch (e.ListChangedType)
    {
        case ListChangedType.ItemAdded:
            Console.WriteLine($"Item added at index {e.NewIndex}");
            break;
        case ListChangedType.ItemDeleted:
            Console.WriteLine($"Item deleted at index {e.NewIndex}");
            break;
        case ListChangedType.ItemChanged:
            Console.WriteLine($"Item changed at index {e.NewIndex}");
            break;
        case ListChangedType.Reset:
            Console.WriteLine("List reset");
            break;
    }
}
```

Use for:
- Tracking data changes for auditing
- Triggering external updates (save to database)
- Synchronizing with other data-bound controls

## Common Scenarios

### Scenario 1: Context Menu on Right-Click

```csharp
ContextMenuStrip contextMenu = new ContextMenuStrip();
contextMenu.Items.Add("Edit", null, EditMenuItem_Click);
contextMenu.Items.Add("Delete", null, DeleteMenuItem_Click);

gridGroupingControl1.TableControl.CellClick += TableControl_CellClick;

void TableControl_CellClick(object sender, GridCellClickEventArgs e)
{
    if (e.MouseEventArgs.Button == MouseButtons.Right)
    {
        // Show context menu
        var element = gridGroupingControl1.TableControl.GetTableCellAtPoint(
            e.MouseEventArgs.Location);
        
        if (element?.Record != null)
        {
            selectedRecord = element.Record;
            contextMenu.Show(gridGroupingControl1.TableControl, 
                e.MouseEventArgs.Location);
        }
    }
}

void EditMenuItem_Click(object sender, EventArgs e)
{
    // Edit selected record
    EditRecord(selectedRecord);
}

void DeleteMenuItem_Click(object sender, EventArgs e)
{
    // Delete selected record
    DeleteRecord(selectedRecord);
}
```

### Scenario 2: Custom Cell Tooltips

```csharp
ToolTip tooltip = new ToolTip();

gridGroupingControl1.TableControl.CellMouseHoverEnter += TableControl_CellMouseHoverEnter;
gridGroupingControl1.TableControl.CellMouseHoverLeave += TableControl_CellMouseHoverLeave;

void TableControl_CellMouseHoverEnter(object sender, GridCellMouseEventArgs e)
{
    var style = gridGroupingControl1.TableControl.Model[e.RowIndex, e.ColIndex];
    var column = style.TableCellIdentity.Column;
    
    if (column?.Name == "Status")
    {
        // Show detailed status tooltip
        string statusText = style.Text;
        string tooltipText = GetDetailedStatusDescription(statusText);
        
        Point position = gridGroupingControl1.TableControl.PointToClient(
            Cursor.Position);
        tooltip.Show(tooltipText, gridGroupingControl1.TableControl, 
            position, 3000);
    }
}

void TableControl_CellMouseHoverLeave(object sender, GridCellMouseEventArgs e)
{
    tooltip.Hide(gridGroupingControl1.TableControl);
}
```

### Scenario 3: Row-Level Validation on Edit

```csharp
gridGroupingControl1.TableControl.CurrentCellValidating += 
    TableControl_CurrentCellValidating;

void TableControl_CurrentCellValidating(object sender, CancelEventArgs e)
{
    var currentCell = gridGroupingControl1.TableControl.CurrentCell;
    var style = currentCell.Element as GridTableCellStyleInfo;
    
    if (style?.TableCellIdentity.Column?.Name == "Quantity")
    {
        string text = currentCell.Renderer.ControlText;
        
        // Validate quantity
        if (!int.TryParse(text, out int quantity) || quantity <= 0)
        {
            MessageBox.Show("Quantity must be a positive integer");
            e.Cancel = true; // Prevent cell deactivation
            return;
        }
        
        // Check inventory availability
        string productCode = GetCellValue(currentCell.RowIndex, "ProductCode");
        if (GetAvailableInventory(productCode) < quantity)
        {
            MessageBox.Show("Not enough inventory available");
            e.Cancel = true;
        }
    }
}
```

### Scenario 4: Auto-Save on Data Change

```csharp
private Timer autoSaveTimer;
private bool dataChanged = false;

public MyForm()
{
    InitializeComponent();
    
    // Setup auto-save timer
    autoSaveTimer = new Timer { Interval = 5000 }; // 5 seconds
    autoSaveTimer.Tick += AutoSaveTimer_Tick;
    autoSaveTimer.Start();
    
    // Track changes
    gridGroupingControl1.SourceListListChanged += 
        GridGroupingControl1_SourceListListChanged;
}

void GridGroupingControl1_SourceListListChanged(object sender, 
    ListChangedEventArgs e)
{
    if (e.ListChangedType == ListChangedType.ItemAdded ||
        e.ListChangedType == ListChangedType.ItemChanged ||
        e.ListChangedType == ListChangedType.ItemDeleted)
    {
        dataChanged = true;
    }
}

void AutoSaveTimer_Tick(object sender, EventArgs e)
{
    if (dataChanged)
    {
        SaveDataToDatabase();
        dataChanged = false;
        statusLabel.Text = $"Auto-saved at {DateTime.Now:HH:mm:ss}";
    }
}
```

### Scenario 5: Conditional Cell Background on Hover

```csharp
gridGroupingControl1.TableControl.CellMouseHoverEnter += 
    TableControl_CellMouseHoverEnter;
gridGroupingControl1.TableControl.CellMouseHoverLeave += 
    TableControl_CellMouseHoverLeave;

void TableControl_CellMouseHoverEnter(object sender, GridCellMouseEventArgs e)
{
    var style = gridGroupingControl1.TableControl.Model[e.RowIndex, e.ColIndex];
    
    // Highlight hovered cell
    style.BackColor = Color.LightYellow;
    gridGroupingControl1.TableControl.Invalidate(
        gridGroupingControl1.TableControl.RangeInfoToRectangle(
            GridRangeInfo.Cell(e.RowIndex, e.ColIndex)));
}

void TableControl_CellMouseHoverLeave(object sender, GridCellMouseEventArgs e)
{
    // Reset cell appearance
    var style = gridGroupingControl1.TableControl.Model[e.RowIndex, e.ColIndex];
    style.BackColor = SystemColors.Window;
    gridGroupingControl1.TableControl.Invalidate(
        gridGroupingControl1.TableControl.RangeInfoToRectangle(
            GridRangeInfo.Cell(e.RowIndex, e.ColIndex)));
}
```

## Best Practices

### Event Subscription Management

1. **Always unsubscribe** when disposing control:
   ```csharp
   protected override void OnFormClosing(FormClosingEventArgs e)
   {
       gridGroupingControl1.TableControl.CellClick -= TableControl_CellClick;
       gridGroupingControl1.Dispose();
       base.OnFormClosing(e);
   }
   ```

2. **Use weak event pattern** for long-lived subscribers

3. **Avoid memory leaks**: Unsubscribe before reassigning data source

### Performance

1. **Minimize expensive operations** in high-frequency events (CellDrawn, CurrentCellChanged)
2. **Batch updates**:
   ```csharp
   dataTable.BeginLoadData();
   // ... multiple changes
   dataTable.EndLoadData();
   ```

3. **Use e.Handled** to prevent default processing when custom handling is complete

4. **Debounce high-frequency events**:
   ```csharp
   private Timer debounceTimer = new Timer { Interval = 300 };
   
   void TableControl_CurrentCellChanged(object sender, EventArgs e)
   {
       debounceTimer.Stop();
       debounceTimer.Start();
   }
   
   void DebounceTimer_Tick(object sender, EventArgs e)
   {
       debounceTimer.Stop();
       // Perform expensive operation
   }
   ```

### Validation

1. **Use cancelable events** (CurrentCellActivating, CurrentCellValidating) for validation
2. **Provide clear error messages** when canceling operations
3. **Focus invalid cell** after validation failure:
   ```csharp
   currentCell.Activate();
   ```

### Error Handling

1. **Wrap event handlers** in try-catch:
   ```csharp
   void TableControl_CellClick(object sender, GridCellClickEventArgs e)
   {
       try
       {
           // Event logic
       }
       catch (Exception ex)
       {
           LogError(ex);
           MessageBox.Show("An error occurred");
       }
   }
   ```

2. **Log errors** for diagnostics without disrupting user workflow

### UI Responsiveness

1. **Use async/await** for long-running operations in event handlers:
   ```csharp
   async void TableControl_CellClick(object sender, GridCellClickEventArgs e)
   {
       await LoadDataAsync();
   }
   ```

2. **Show progress indicators** for slow operations

3. **Enable UI during processing**:
   ```csharp
   Application.DoEvents(); // Use sparingly
   ```
