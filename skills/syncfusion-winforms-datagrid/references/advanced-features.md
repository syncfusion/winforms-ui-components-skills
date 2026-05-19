# Advanced Features in WinForms DataGrid

Comprehensive guide for advanced DataGrid features including Master-Details View (hierarchical data), drag-and-drop operations, localization, and serialization/deserialization in the Syncfusion WinForms DataGrid (SfDataGrid).

## Table of Contents
- [Overview](#overview)
- [Master-Details View](#master-details-view)
- [Drag and Drop](#drag-and-drop)
- [Localization](#localization)
- [Serialization and Deserialization](#serialization-and-deserialization)
- [Edge Cases and Troubleshooting](#edge-cases-and-troubleshooting)

## Overview

SfDataGrid provides advanced features for enterprise-level applications:
- **Master-Details View:** Display hierarchical data with nested grids
- **Drag and Drop:** Rearrange columns and rows via drag-and-drop
- **Localization:** Translate grid text for different cultures
- **Serialization:** Save and restore grid settings (columns, sorting, filtering, grouping)

## Master-Details View

Display hierarchical data in nested tables with unlimited nesting levels.

### Creating Hierarchical Data Source

Define data model with `IEnumerable` property for relations:

```csharp
public class OrderInfo : INotifyPropertyChanged
{
    private int _OrderID;
    private string _CustomerID;
    private List<OrderDetails> orderDetails;

    public int OrderID
    {
        get { return this._OrderID; }
        set
        {
            this._OrderID = value;
            RaisePropertyChanged("OrderID");
        }
    }

    public string CustomerID
    {
        get { return this._CustomerID; }
        set
        {
            this._CustomerID = value;
            RaisePropertyChanged("CustomerID");
        }
    }

    // Relation property for Master-Details view
    public List<OrderDetails> OrderDetails
    {
        get { return this.orderDetails; }
        set
        {
            this.orderDetails = value;
            RaisePropertyChanged("OrderDetails");
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;

    private void RaisePropertyChanged(string name)
    {
        if (PropertyChanged != null)
            PropertyChanged(this, new PropertyChangedEventArgs(name));
    }
}

public class OrderDetails : INotifyPropertyChanged
{
    private int _OrderID;
    private int _ProductID;
    private decimal _UnitPrice;
    private Int16 _Quantity;

    public int OrderID
    {
        get { return this._OrderID; }
        set
        {
            this._OrderID = value;
            RaisePropertyChanged("OrderID");
        }
    }

    public int ProductID
    {
        get { return this._ProductID; }
        set
        {
            this._ProductID = value;
            RaisePropertyChanged("ProductID");
        }
    }

    public decimal UnitPrice
    {
        get { return this._UnitPrice; }
        set
        {
            this._UnitPrice = value;
            RaisePropertyChanged("UnitPrice");
        }
    }

    public Int16 Quantity
    {
        get { return this._Quantity; }
        set
        {
            this._Quantity = value;
            RaisePropertyChanged("Quantity");
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;

    private void RaisePropertyChanged(string name)
    {
        if (PropertyChanged != null)
            PropertyChanged(this, new PropertyChangedEventArgs(name));
    }
}
```

### Auto-Generating Relations

Enable automatic relation generation:

```csharp
sfDataGrid.AutoGenerateRelations = true;
sfDataGrid.DataSource = orderInfoCollection;
```

### Manually Defining Relations

Define relations explicitly:

```csharp
sfDataGrid.AutoGenerateRelations = false;

var gridViewDefinition = new GridViewDefinition();
gridViewDefinition.RelationalColumn = "OrderDetails";
gridViewDefinition.DataGrid = new SfDataGrid() { Name = "FirstLevelNestedGrid", AutoGenerateColumns = true };

sfDataGrid.DetailsViewDefinitions.Add(gridViewDefinition);
```

### Accessing Details View DataGrid

Access nested grids:

```csharp
// Get DetailsViewDataGrid for first record
var detailsViewDataGrid = sfDataGrid.GetDetailsViewGrid(0);

// Get DetailsViewDataGrid for specific row index
var detailsGrid = sfDataGrid.GetDetailsViewGrid(5);
```

### Expanding and Collapsing

#### Programmatically Expand/Collapse

```csharp
// Expand specific record
sfDataGrid.ExpandDetailsViewAt(2);

// Collapse specific record
sfDataGrid.CollapseDetailsViewAt(2);

// Expand all
sfDataGrid.ExpandAllDetailsView();

// Collapse all
sfDataGrid.CollapseAllDetailsView();
```

#### Handle Expand/Collapse Events

```csharp
sfDataGrid.DetailsViewExpanding += SfDataGrid_DetailsViewExpanding;
sfDataGrid.DetailsViewExpanded += SfDataGrid_DetailsViewExpanded;
sfDataGrid.DetailsViewCollapsing += SfDataGrid_DetailsViewCollapsing;
sfDataGrid.DetailsViewCollapsed += SfDataGrid_DetailsViewCollapsed;

void SfDataGrid_DetailsViewExpanding(object sender, DetailsViewExpandingEventArgs e)
{
    if (e.Record == null)
        e.Cancel = true;
}

void SfDataGrid_DetailsViewExpanded(object sender, DetailsViewExpandedEventArgs e)
{
    // Perform action after expansion
}
void SfDataGrid_DetailsViewCollapsing(object sender, DetailsViewCollapsingEventArgs e)
{

}
void SfDataGrid_DetailsViewCollapsed(object sender, DetailsViewCollapsedEventArgs e)
{
    // Perform action after collapse   
}
```

### Customizing Details View Appearance

```csharp
// Set maximum size for nested grid
firstLevelSourceDataGrid.DataGrid.MaximumSize = new Size(0, 300);

// Customize nested grid
firstLevelSourceDataGrid.DataGrid.AllowSorting = true;
firstLevelSourceDataGrid.DataGrid.AllowFiltering = true;
firstLevelSourceDataGrid.DataGrid.AllowGrouping = false;
```

## Drag and Drop

### Column Drag and Drop

Enable column reordering:

```csharp
sfDataGrid.AllowDraggingColumns = true;
```

Disable for specific column:

```csharp
sfDataGrid.Columns[0].AllowDragging = false;
```

#### Cancel Column Dragging

```csharp
sfDataGrid.ColumnDragging += SfDataGrid_ColumnDragging;

void SfDataGrid_ColumnDragging(object sender, ColumnDraggingEventArgs e)
{
    // Cancel dragging start
    if (e.Reason == ColumnDraggingAction.DragStarting)
    {
        var column = sfDataGrid.Columns[e.From];
        if (column.MappingName == "OrderID")
        {
            e.Cancel = true;
        }
    }

    // Cancel drop at specific position
    if (e.Reason == ColumnDraggingAction.Dropping)
    {
        var targetColumn = sfDataGrid.Columns[e.To];
        if (targetColumn.MappingName == "ProductName")
        {
            e.Cancel = true;
        }
    }
}
```

#### Disable Drag Between Frozen and Non-Frozen Columns

```csharp
sfDataGrid.ColumnDragging += SfDataGrid_ColumnDragging;

void SfDataGrid_ColumnDragging(object sender, ColumnDraggingEventArgs e)
{
    if (e.Reason == ColumnDraggingAction.Dropping)
    {
        var frozenColIndex = this.sfDataGrid.FrozenColumnCount + 
            this.sfDataGrid.TableControl.ResolveToStartColumnIndex();
        
        if (e.From < frozenColIndex && e.To > frozenColIndex - 1)
            e.Cancel = true;
        
        if (e.From > frozenColIndex && e.To < frozenColIndex || 
            (e.From == frozenColIndex && e.To < frozenColIndex))
            e.Cancel = true;
    }
}
```

#### Custom Column Drag-Drop Controller

```csharp
sfDataGrid.ColumnDragDropController = new CustomDragAndDropController(
    sfDataGrid.TableControl, sfDataGrid.GroupPanel);

public class CustomDragAndDropController : ColumnDragDropController
{
    public CustomDragAndDropController(TableControl tableControl, GroupPanel groupPanel)
        : base(tableControl, groupPanel)
    {
    }

    protected override bool CanShowPopup(GridColumn column)
    {
        if (column.MappingName == "UnitPrice")
            return false;
        return base.CanShowPopup(column);
    }

    protected override void PopupDroppedOnHeaderRow(int oldIndex, int newIndex)
    {
        if (newIndex == 0)
            return;
        base.PopupDroppedOnHeaderRow(oldIndex, newIndex);
    }

    protected override void PopupDroppedOnGroupDropArea(GridColumn draggingColumn, MouseEventArgs e)
    {
        if (draggingColumn.MappingName == "OrderID")
            return;
        base.PopupDroppedOnGroupDropArea(draggingColumn, e);
    }
}
```

### Row Drag and Drop

Enable row dragging:

```csharp
sfDataGrid.AllowDraggingRows = true;
sfDataGrid.AllowDrop = true;
```

#### Row Drag Events

```csharp
sfDataGrid.RowDragDropController.DragStart += RowDragDropController_DragStart;
sfDataGrid.RowDragDropController.DragOver += RowDragDropController_DragOver;
sfDataGrid.RowDragDropController.Drop += RowDragDropController_Drop;
sfDataGrid.RowDragDropController.DragLeave += RowDragDropController_DragLeave;
sfDataGrid.RowDragDropController.Dropped += RowDragDropController_Dropped;

 
void RowDragDropController_DragStart(object sender, GridRowDragStartEventArgs e)
{
    foreach (var item in e.DraggingRecords)
    {
        if (item is OrderInfo order && order.OrderID < 10)
        {
            e.Cancel = true;
            break;
        }
    }
}

void RowDragDropController_DragOver(object sender, GridRowDragOverEventArgs e)
{
    // Provide visual feedback during drag
    e.ShowDragUI = true;
}

void RowDragDropController_Drop(object sender, GridRowDropEventArgs e)
{
    if (e.TargetRecord is OrderInfo o && o.OrderID < 10)
        e.Handled = true;
}

void RowDragDropController_Dropped(object sender, GridRowDroppedEventArgs e)
{
    if (e.IsFromOutsideSource)
    {
        var list = sfDataGrid1.DataSource as IList<OrderInfo>;
        if (list == null) return;

        foreach (var item in e.DraggingRecords.OfType<OrderInfo>())
        {
            list.Add(item); // add new records
        }
    }
}
void RowDragDropController_DragLeave(object sender, GridRowDragLeaveEventArgs e)
{
    // Check if drag is from outside
    if (e.IsFromOutsideSource)
    {
        Console.WriteLine("External drag left");
    }
    else
    {
        Console.WriteLine("Internal drag left");
    }
}

```

#### Drag Rows to External Controls

```csharp
// Enable drag from DataGrid
sfDataGrid.AllowDraggingRows = true;

// Handle DragEnter in target control (e.g., ListView)
listView.AllowDrop = true;
listView.DragEnter += ListView_DragEnter;
listView.DragDrop += ListView_DragDrop;

void ListView_DragEnter(object sender, DragEventArgs e)
{
    if (e.Data.GetDataPresent(typeof(ObservableCollection<object>)))
        e.Effect = DragDropEffects.Copy;
}

void ListView_DragDrop(object sender, DragEventArgs e)
{
    var draggedRecords = e.Data.GetData(typeof(ObservableCollection<object>)) 
        as ObservableCollection<object>;
    
    foreach (var record in draggedRecords)
    {
        // Add to ListView
        listView.Items.Add(record.ToString());
    }
}
```

## Localization

Translate grid text for different cultures.

### Setup Localization

**Step 1:** Add resource file to Resources folder

Create `Syncfusion.SfDataGrid.WinForms.<culture>.resx` (e.g., `Syncfusion.SfDataGrid.WinForms.de-DE.resx`)

**Step 2:** Add Name/Value pairs for localized strings:

| Name | Default (English) | German (de-DE) |
|------|-------------------|----------------|
| AddNewRow | Add new row | Neue Zeile hinzufügen |
| True | True | Wahr |
| False | False | Falsch |
| SortAscending | Sort Ascending | Aufsteigend sortieren |
| SortDescending | Sort Descending | Absteigend sortieren |
| ClearSorting | Clear Sorting | Sortierung löschen |
| ClearFiltering | Clear Filtering | Filter löschen |
| FilterEditor | Filter Editor | Filter-Editor |
| OK | OK | OK |
| Cancel | Cancel | Abbrechen |

**Step 3:** Set current culture before `InitializeComponent`:

```csharp
public Form1()
{
    System.Threading.Thread.CurrentThread.CurrentCulture = 
        new System.Globalization.CultureInfo("de-DE");
    System.Threading.Thread.CurrentThread.CurrentUICulture = 
        new System.Globalization.CultureInfo("de-DE");
    
    InitializeComponent();
}
```

```vb
Public Sub New()
    System.Threading.Thread.CurrentThread.CurrentCulture = _
        New System.Globalization.CultureInfo("de-DE")
    System.Threading.Thread.CurrentThread.CurrentUICulture = _
        New System.Globalization.CultureInfo("de-DE")
    
    InitializeComponent()
End Sub
```

### Editing Default Resource File

To modify default English text without creating culture-specific file:

1. Add `Syncfusion.SfDataGrid.WinForms.resx` to Resources folder
2. Modify Name/Value pairs
3. Run application (no culture setting needed)

### Custom Assembly/Namespace for Resources

```csharp
public Form1()
{
    System.Threading.Thread.CurrentThread.CurrentCulture = 
        new System.Globalization.CultureInfo("de-DE");
    System.Threading.Thread.CurrentThread.CurrentUICulture = 
        new System.Globalization.CultureInfo("de-DE");

    // Set custom assembly and namespace for localization
    SR.SetResources(typeof(CustomSfDataGrid).Assembly, "SfDataGridExt");
    
    InitializeComponent();
}
```

## Serialization and Deserialization

Save and restore grid settings (columns, sorting, filtering, grouping) to XML.

### Basic Serialization

```csharp
using (var file = File.Create("DataGrid.xml"))
{
    this.sfDataGrid.Serialize(file);
}
```

```vb
Using file = File.Create("DataGrid.xml")
    Me.sfDataGrid.Serialize(file)
End Using
```

### Serialize as Stream

```csharp
FileStream stream = new FileStream("DataGrid", FileMode.Create);
this.sfDataGrid.Serialize(stream);
stream.Close();
```

### Basic Deserialization

```csharp
using (var file = File.Open("DataGrid.xml", FileMode.Open))
{
    this.sfDataGrid.Deserialize(file);
}
```

```vb
Using file = File.Open("DataGrid.xml", FileMode.Open)
    Me.sfDataGrid.Deserialize(file)
End Using
```

### Serialization Options

Customize what gets serialized:

```csharp
using (var file = File.Create("DataGrid.xml"))
{
    SerializationOptions options = new SerializationOptions();
    options.SerializeSorting = true;       // Serialize sorting
    options.SerializeGrouping = true;      // Serialize grouping
    options.SerializeFiltering = true;     // Serialize filtering
    options.SerializeColumns = true;       // Serialize columns
    options.SerializeStackedHeaders = true; // Serialize stacked headers
    options.SerializeCaptionSummaries = true;  // Serialize caption summaries
    options.SerializeGroupSummaries = true;    // Serialize group summaries
    options.SerializeTableSummaries = true;    // Serialize table summaries
    options.SerializeUnboundRows = true;      // Serialize unbound rows
    
    this.sfDataGrid.Serialize(file, options);
}
```

### Selective Serialization Examples

#### Serialize Columns Only

```csharp
using (var file = File.Create("Columns.xml"))
{
    SerializationOptions options = new SerializationOptions();
    options.SerializeSorting = false;
    options.SerializeGrouping = false;
    options.SerializeFiltering = false;
    options.SerializeColumns = true; // Only columns
    
    this.sfDataGrid.Serialize(file, options);
}
```

#### Serialize Sorting and Filtering

```csharp
using (var file = File.Create("SortFilter.xml"))
{
    SerializationOptions options = new SerializationOptions();
    options.SerializeSorting = true;
    options.SerializeFiltering = true;
    options.SerializeGrouping = false;
    options.SerializeColumns = false;
    
    this.sfDataGrid.Serialize(file, options);
}
```

### Deserialization Options

Customize what gets deserialized:

```csharp
using (var file = File.Open("DataGrid.xml", FileMode.Open))
{
    DeserializationOptions options = new DeserializationOptions();
    options.DeserializeSorting = true;
    options.DeserializeGrouping = true;
    options.DeserializeFiltering = true;
    options.DeserializeColumns = true;
    
    this.sfDataGrid.Deserialize(file, options);
}
```

## Edge Cases and Troubleshooting

### Issue: Details view not expanding

**Cause:** `AutoGenerateRelations = false` without manual relation definition

**Solution:** Either enable auto-generation or define relations manually:

```csharp
// Option 1: Auto-generate
sfDataGrid.AutoGenerateRelations = true;

// Option 2: Manual definition
sfDataGrid.AutoGenerateRelations = false;
var firstLevelSourceDataGrid = new GridViewDefinition();
firstLevelSourceDataGrid.RelationalColumn = "OrderDetails";
sfDataGrid.DetailsViewDefinition.Add(firstLevelSourceDataGrid);
```

### Issue: Column drag not working

**Cause:** `AllowDraggingColumns` is false or column has `AllowDragging = false`

**Solution:** Enable dragging:

```csharp
sfDataGrid.AllowDraggingColumns = true;
sfDataGrid.Columns["ColumnName"].AllowDragging = true;
```

### Issue: Row drag causes exception

**Cause:** DataSource doesn't support add/remove operations (e.g., Array)

**Solution:** Use ObservableCollection or List:

```csharp
// ❌ Wrong: Array doesn't support add/remove
//OrderInfo[] array = new OrderInfo[10];

// ✓ Correct: ObservableCollection supports add/remove
ObservableCollection<OrderInfo> list = new ObservableCollection<OrderInfo>();
sfDataGrid.DataSource = list;
```

### Issue: Localization not applied

**Cause:** Resource file not named correctly or culture not set

**Solution:** Verify resource file naming and culture:

```csharp
// Resource file MUST be named: Syncfusion.SfDataGrid.WinForms.<culture>.resx
// Example: Syncfusion.SfDataGrid.WinForms.de-DE.resx

// Set culture BEFORE InitializeComponent
System.Threading.Thread.CurrentThread.CurrentCulture = 
    new System.Globalization.CultureInfo("de-DE");
System.Threading.Thread.CurrentThread.CurrentUICulture = 
    new System.Globalization.CultureInfo("de-DE");

InitializeComponent();
```

### Issue: Serialization doesn't restore all settings

**Cause:** SerializationOptions properties set to false

**Solution:** Enable required options:

```csharp
SerializationOptions options = new SerializationOptions();
options.SerializeSorting = true;
options.SerializeGrouping = true;
options.SerializeFiltering = true;
options.SerializeColumns = true;

sfDataGrid.Serialize(file, options);
```

### Issue: Master-Details view performance degradation

**Cause:** Too many nested levels or large child collections

**Solution:** Limit nested levels and use lazy loading:

```csharp
// Limit expansion depth
int maxLevel = 2;

firstLevelSourceDataGrid.DetailsViewExpanding += (sender, e) =>
{
    if (e.Record is OrderInfo parent)
    {
        // Example: block deeper levels
        if (parent.ChildOrders != null && parent.ChildOrders.Count > 0)
        {
            if (parent.Level >= maxLevel)   // YOU must maintain Level in model
            {
                e.Cancel = true;
            }
        }
    }
};

```

### Issue: Drag-drop between frozen and non-frozen columns

**Behavior:** By default, columns can be dragged between regions

**Solution:** Prevent using ColumnDragging event (see earlier example)

### Issue: Localized text cut off

**Cause:** Translated text longer than English

**Solution:** Adjust column widths or use auto-sizing:

```csharp
sfDataGrid.AutoSizeColumnsMode = AutoSizeColumnsMode.AllCells;
```
