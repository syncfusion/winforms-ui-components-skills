# Data Population

This guide covers all methods for populating data in GridControl, from simple cell-by-cell assignment to high-performance virtual mode with on-demand loading.

## Overview

GridControl is a cell-based control that stores its own data. Before populating, you must set the grid dimensions using `RowCount` and `ColCount` properties.

**Default dimensions:** 10 rows × 10 columns

**Three population methods:**
1. **Loop through cells** - Direct assignment, good for small grids
2. **PopulateValues method** - Batch population from arrays/data sources
3. **QueryCellInfo event** - Virtual mode for large datasets (best performance)

## Setting Grid Dimensions

Always define dimensions before populating:

```csharp
// Set grid size
gridControl1.RowCount = 100;
gridControl1.ColCount = 10;

// Access via Model for virtual grids
gridControl1.Model.RowCount = 1000000;
gridControl1.Model.ColCount = 20;
```

**Key points:**
- RowCount and ColCount define the grid's capacity
- Row 0 and Column 0 are headers
- Data cells start at [1, 1]
- Virtual mode doesn't allocate memory upfront

## Method 1: Loop Through Cells

Direct cell assignment using nested loops. Simple and intuitive for smaller grids.

### Basic Loop Pattern:

```csharp
// Set dimensions
gridControl1.RowCount = 15;
gridControl1.ColCount = 4;

// Loop through all cells
for (int row = 1; row <= gridControl1.RowCount; row++)
{
    for (int col = 1; col <= gridControl1.ColCount; col++)
    {
        gridControl1.Model[row, col].CellValue = string.Format("{0}/{1}", row, col);
    }
}
```

### VB.NET Version:

```vb
' Set dimensions
gridControl1.RowCount = 15
gridControl1.ColCount = 4

' Loop through all cells
For row As Integer = 1 To gridControl1.RowCount
    For col As Integer = 1 To gridControl1.ColCount
        gridControl1.Model(row, col).CellValue = String.Format("{0}/{1}", row, col)
    Next col
Next row
```

### Populating from Collections:

```csharp
// Sample data
var products = new List<Product>
{
    new Product { Name = "Widget", Quantity = 10, Price = 19.99 },
    new Product { Name = "Gadget", Quantity = 5, Price = 29.99 },
    new Product { Name = "Tool", Quantity = 15, Price = 9.99 }
};

// Set grid size
gridControl1.RowCount = products.Count + 1; // +1 for header
gridControl1.ColCount = 3;

// Add headers
gridControl1[1, 1].CellValue = "Product";
gridControl1[1, 2].CellValue = "Quantity";
gridControl1[1, 3].CellValue = "Price";

// Populate data
int rowIndex = 2;
foreach (var product in products)
{
    gridControl1[rowIndex, 1].CellValue = product.Name;
    gridControl1[rowIndex, 2].CellValue = product.Quantity;
    gridControl1[rowIndex, 3].CellValue = product.Price;
    rowIndex++;
}
```

### Performance Optimization:

Use `BeginUpdate()` and `EndUpdate()` for batch operations:

```csharp
gridControl1.BeginUpdate();

for (int row = 1; row <= gridControl1.RowCount; row++)
{
    for (int col = 1; col <= gridControl1.ColCount; col++)
    {
        gridControl1[row, col].CellValue = GetCellData(row, col);
    }
}

gridControl1.EndUpdate();
```

**When to use loops:**
- Small to medium grids (< 1,000 cells)
- Simple data structures
- Need to apply cell-specific formatting
- Data is already in memory

## Method 2: PopulateValues Method

Efficient batch population from arrays or data sources. Better performance than loops for medium-sized datasets.

### Using 2D Arrays:

```csharp
// Set dimensions
gridControl1.RowCount = 15;
gridControl1.ColCount = 4;

// Create data array
string[,] table = new string[gridControl1.RowCount, gridControl1.ColCount];

// Fill array
for (int row = 0; row < gridControl1.RowCount; row++)
{
    for (int col = 0; col < gridControl1.ColCount; col++)
    {
        table[row, col] = string.Format("R{0}C{1}", row + 1, col + 1);
    }
}

// Populate grid in one operation
gridControl1.PopulateValues(
    GridRangeInfo.Cells(1, 1, gridControl1.RowCount, gridControl1.ColCount),
    table
);
```

### VB.NET Version:

```vb
' Set dimensions
gridControl1.RowCount = 15
gridControl1.ColCount = 4

' Create data array
Dim table(gridControl1.RowCount - 1, gridControl1.ColCount - 1) As String

' Fill array
For row As Integer = 0 To gridControl1.RowCount - 1
    For col As Integer = 0 To gridControl1.ColCount - 1
        table(row, col) = String.Format("R{0}C{1}", row + 1, col + 1)
    Next col
Next row

' Populate grid
gridControl1.PopulateValues( _
    GridRangeInfo.Cells(1, 1, gridControl1.RowCount, gridControl1.ColCount), _
    table)
```

### Populating Specific Ranges:

```csharp
// Populate only a portion of the grid
string[,] data = new string[5, 3]
{
    { "A1", "B1", "C1" },
    { "A2", "B2", "C2" },
    { "A3", "B3", "C3" },
    { "A4", "B4", "C4" },
    { "A5", "B5", "C5" }
};

// Populate starting at row 2, column 2
gridControl1.PopulateValues(
    GridRangeInfo.Cells(2, 2, 6, 4),
    data
);
```

### Using DataTable:

```csharp
// Create DataTable
DataTable dt = new DataTable();
dt.Columns.Add("Name", typeof(string));
dt.Columns.Add("Age", typeof(int));
dt.Columns.Add("City", typeof(string));

dt.Rows.Add("John", 30, "New York");
dt.Rows.Add("Jane", 25, "London");
dt.Rows.Add("Bob", 35, "Paris");

// Convert to array and populate
object[,] dataArray = new object[dt.Rows.Count, dt.Columns.Count];
for (int i = 0; i < dt.Rows.Count; i++)
{
    for (int j = 0; j < dt.Columns.Count; j++)
    {
        dataArray[i, j] = dt.Rows[i][j];
    }
}

gridControl1.RowCount = dt.Rows.Count + 1;
gridControl1.ColCount = dt.Columns.Count;

// Populate headers
for (int j = 0; j < dt.Columns.Count; j++)
{
    gridControl1[1, j + 1].CellValue = dt.Columns[j].ColumnName;
}

// Populate data
gridControl1.PopulateValues(
    GridRangeInfo.Cells(2, 1, dt.Rows.Count + 1, dt.Columns.Count),
    dataArray
);
```

**When to use PopulateValues:**
- Medium-sized datasets (1,000 - 10,000 cells)
- Data is in array or table format
- Need better performance than loops
- Batch operations on ranges

## Method 3: QueryCellInfo Event (Virtual Mode)

High-performance virtual mode that loads data on-demand. Best for large datasets.

### Basic Virtual Grid:

```csharp
// Set dimensions (no memory allocated)
gridControl1.Model.RowCount = 1000000;  // One million rows!
gridControl1.Model.ColCount = 20;

// Handle QueryCellInfo event
gridControl1.QueryCellInfo += GridControl1_QueryCellInfo;

private void GridControl1_QueryCellInfo(object sender, GridQueryCellInfoEventArgs e)
{
    // Only populate when grid needs to display the cell
    if (e.RowIndex > 0 && e.ColIndex > 0)
    {
        e.Style.CellValue = string.Format("Cell {0},{1}", e.RowIndex, e.ColIndex);
    }
}
```

### Loading from Database:

```csharp
private void GridControl1_QueryCellInfo(object sender, GridQueryCellInfoEventArgs e)
{
    if (e.RowIndex > 0 && e.ColIndex > 0)
    {
        // Load data from database on-demand
        var data = GetDataFromDatabase(e.RowIndex, e.ColIndex);
        e.Style.CellValue = data;
        
        // Apply conditional formatting
        if (e.RowIndex % 2 == 0)
        {
            e.Style.BackColor = Color.WhiteSmoke;
        }
    }
    else if (e.RowIndex == 0 && e.ColIndex > 0)
    {
        // Column headers
        e.Style.CellValue = $"Column {e.ColIndex}";
        e.Style.BackColor = Color.SteelBlue;
        e.Style.TextColor = Color.White;
        e.Style.Font.Bold = true;
    }
    else if (e.RowIndex > 0 && e.ColIndex == 0)
    {
        // Row headers
        e.Style.CellValue = e.RowIndex.ToString();
    }
}
```

### Virtual Grid with Caching:

```csharp
// Cache for frequently accessed data
private Dictionary<string, object> dataCache = new Dictionary<string, object>();

private void GridControl1_QueryCellInfo(object sender, GridQueryCellInfoEventArgs e)
{
    if (e.RowIndex > 0 && e.ColIndex > 0)
    {
        string key = $"{e.RowIndex}_{e.ColIndex}";
        
        // Check cache first
        if (!dataCache.ContainsKey(key))
        {
            // Load from data source if not cached
            dataCache[key] = LoadDataFromSource(e.RowIndex, e.ColIndex);
        }
        
        e.Style.CellValue = dataCache[key];
    }
}

// Clear cache when data changes
private void ClearDataCache()
{
    dataCache.Clear();
    gridControl1.Refresh();  // Force re-query
}
```

### Complete Virtual Grid Example:

```csharp
public class VirtualGridExample : Form
{
    private GridControl gridControl1;
    private List<DataRow> dataSource;
    
    public VirtualGridExample()
    {
        InitializeComponent();
        InitializeVirtualGrid();
    }
    
    private void InitializeVirtualGrid()
    {
        gridControl1 = new GridControl();
        gridControl1.Dock = DockStyle.Fill;
        
        // Load data source
        dataSource = LoadLargeDataset();
        
        // Set virtual grid size
        gridControl1.Model.RowCount = dataSource.Count;
        gridControl1.Model.ColCount = 10;
        
        // Enable virtual mode
        gridControl1.QueryCellInfo += OnQueryCellInfo;
        
        this.Controls.Add(gridControl1);
    }
    
    private void OnQueryCellInfo(object sender, GridQueryCellInfoEventArgs e)
    {
        if (e.RowIndex > 0 && e.ColIndex > 0 && e.RowIndex <= dataSource.Count)
        {
            var row = dataSource[e.RowIndex - 1];
            e.Style.CellValue = row[e.ColIndex - 1];
        }
    }
    
    private List<DataRow> LoadLargeDataset()
    {
        // Simulate large dataset
        var data = new List<DataRow>();
        for (int i = 0; i < 100000; i++)
        {
            data.Add(new DataRow { /* ... */ });
        }
        return data;
    }
}
```

**When to use QueryCellInfo:**
- Large datasets (> 10,000 rows)
- Data from external sources (database, API)
- Memory constraints
- Need maximum performance
- On-demand data loading

## Hybrid Approach

Combine methods for optimal results:

```csharp
// Use QueryCellInfo for data cells
gridControl1.Model.RowCount = 100000;
gridControl1.Model.ColCount = 10;
gridControl1.QueryCellInfo += LoadDataCells;

// Use direct assignment for headers
for (int col = 1; col <= 10; col++)
{
    gridControl1[0, col].CellValue = $"Header {col}";
    gridControl1[0, col].BackColor = Color.Navy;
    gridControl1[0, col].TextColor = Color.White;
    gridControl1[0, col].Font.Bold = true;
}

private void LoadDataCells(object sender, GridQueryCellInfoEventArgs e)
{
    if (e.RowIndex > 0 && e.ColIndex > 0)
    {
        e.Style.CellValue = GetData(e.RowIndex, e.ColIndex);
    }
}
```

## Performance Comparison

| Method | Small (<1K cells) | Medium (1K-10K) | Large (>10K) | Memory Usage |
|--------|-------------------|-----------------|--------------|--------------|
| Loop | Good | Acceptable | Poor | High |
| PopulateValues | Good | Good | Poor | High |
| QueryCellInfo | Excellent | Excellent | Excellent | Low |

## Best Practices

### For All Methods:
- Set RowCount and ColCount before populating
- Use BeginUpdate/EndUpdate for batch operations
- Consider user experience (show progress for slow operations)

### For Loops:
- Limit to small datasets
- Use BeginUpdate/EndUpdate
- Cache repeated calculations

### For PopulateValues:
- Prepare data in arrays first
- Use appropriate data types
- Consider memory for large arrays

### For QueryCellInfo:
- Keep event handler fast (< 1ms per cell)
- Implement caching for frequently accessed data
- Load data asynchronously if needed
- Handle header rows separately

## Troubleshooting

### Cells Not Showing Data
- Verify RowCount and ColCount are set
- Check row/column indices (1-based for data)
- Ensure CellValue is set correctly

### Performance Issues
- Switch to QueryCellInfo for large datasets
- Use BeginUpdate/EndUpdate
- Avoid complex operations in loops

### QueryCellInfo Not Firing
- Verify Model.RowCount is set (not just RowCount)
- Check event is properly subscribed
- Ensure grid is visible and needs to render cells

### Memory Problems
- Use virtual mode (QueryCellInfo)
- Don't store all data in memory
- Implement data paging or caching

## Next Steps

After populating your grid:
1. Apply cell styling for better presentation
2. Configure cell types for data entry
3. Enable editing and validation
4. Add formulas for calculations
5. Implement selection and navigation features
