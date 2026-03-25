# Troubleshooting GridGroupingControl

Common issues, solutions, and best practices for GridGroupingControl implementation.

## Performance Issues

### Slow Performance with Large Datasets

**Symptoms:**
- Grid becomes sluggish with thousands of records
- UI freezes during data loading
- Slow scrolling or grouping operations

**Solutions:**

1. **Enable Data Virtualization (Automatic)**

GridGroupingControl automatically uses virtualization, but ensure you're not disabling it:

```csharp
// Virtualization is enabled by default
// Don't iterate through all records unnecessarily
```

2. **Optimize QueryCellStyleInfo Event**

This event fires for every visible cell. Keep it lightweight:

```csharp
// ❌ BAD - Complex operations for every cell
gridGroupingControl1.QueryCellStyleInfo += (s, e) =>
{
    // Don't do heavy database queries here
    // Don't perform complex calculations for every cell
};

// ✅ GOOD - Quick conditional checks only
gridGroupingControl1.QueryCellStyleInfo += (s, e) =>
{
    if (e.TableCellIdentity.TableCellType == GridTableCellType.RecordFieldCell)
    {
        if (e.TableCellIdentity.Column.Name == "Status" && e.Style.CellValue?.ToString() == "Critical")
        {
            e.Style.BackColor = Color.Red;
        }
    }
};
```

3. **Limit Initial Grouping Levels**

```csharp
// Start with 1-2 grouping levels
// Let users add more via GroupDropArea if needed
gridGroupingControl1.TableDescriptor.GroupedColumns.Add("Department");
// Avoid: Adding 4+ grouping levels initially
```

4. **Use BeginUpdate/EndUpdate for Bulk Operations**

```csharp
gridGroupingControl1.BeginUpdate();
try
{
    // Multiple operations
    gridGroupingControl1.TableDescriptor.GroupedColumns.Clear();
    gridGroupingControl1.TableDescriptor.GroupedColumns.Add("Category");
    gridGroupingControl1.TableDescriptor.SortedColumns.Add("Name");
}
finally
{
    gridGroupingControl1.EndUpdate();
}
```

### Memory Issues

**Symptoms:**
- High memory consumption
- OutOfMemoryException with large datasets

**Solutions:**

1. **Dispose Properly**

```csharp
protected override void Dispose(bool disposing)
{
    if (disposing)
    {
        if (gridGroupingControl1 != null)
        {
            gridGroupingControl1.Dispose();
        }
        if (components != null)
        {
            components.Dispose();
        }
    }
    base.Dispose(disposing);
}
```

2. **Avoid Keeping Unnecessary References**

```csharp
// ❌ BAD - Keeping references to all records
List<Record> allRecords = new List<Record>();
foreach (Record r in gridGroupingControl1.Table.Records)
{
    allRecords.Add(r); // Memory leak
}

// ✅ GOOD - Process and release
foreach (Record r in gridGroupingControl1.Table.Records)
{
    ProcessRecord(r);
    // No long-term reference kept
}
```

## Data Binding Issues

### Data Not Displaying

**Issue:** Grid is empty after setting DataSource

**Possible Causes & Solutions:**

1. **DataSource is null or empty**

```csharp
if (dataTable != null && dataTable.Rows.Count > 0)
{
    gridGroupingControl1.DataSource = dataTable;
}
else
{
    MessageBox.Show("No data to display");
}
```

2. **Wrong DataMember**

```csharp
// For DataSet with multiple tables
gridGroupingControl1.DataSource = dataSet;
gridGroupingControl1.DataMember = "Orders"; // Correct table name
```

3. **Columns Not Visible**

```csharp
// Check if columns are hidden
gridGroupingControl1.TableDescriptor.VisibleColumns.LoadDefault();
```

### Changes Not Reflecting

**Issue:** Data changes in DataSource don't appear in grid

**Solutions:**

1. **Use IBindingList Implementation**

```csharp
// ✅ Use BindingList<T> for automatic updates
BindingList<Employee> employees = new BindingList<Employee>();
gridGroupingControl1.DataSource = employees;

// Changes will reflect automatically
employees.Add(new Employee { Name = "John" });
```

2. **Refresh Grid Manually**

```csharp
// After modifying DataTable directly
dataTable.Rows.Add(1, "New Item");
gridGroupingControl1.TableControl.Refresh();
```

### Binding to Complex Objects

**Issue:** Properties of nested objects not displaying

**Solution:**

```csharp
// Use dot notation for nested properties
gridGroupingControl1.TableDescriptor.Columns["Address.City"].HeaderText = "City";
gridGroupingControl1.TableDescriptor.Columns["Address.Country"].HeaderText = "Country";
```

## Grouping Issues

### GroupDropArea Not Showing

**Issue:** GroupDropArea is not visible

**Solutions:**

```csharp
// Ensure ShowGroupDropArea is true
gridGroupingControl1.ShowGroupDropArea = true;

// Check if form has enough height
gridGroupingControl1.MinimumSize = new Size(400, 200);
```

### Groups Not Expanding

**Issue:** Clicking on group doesn't expand it

**Solutions:**

1. **Check AllowExpandGroups**

```csharp
gridGroupingControl1.TableOptions.AllowExpandGroups = true;
```

2. **Expand Programmatically**

```csharp
// Expand all groups
gridGroupingControl1.Table.ExpandAllGroups();

// Expand specific group
gridGroupingControl1.Table.TopLevelGroup.Groups[0].IsExpanded = true;
```

### Custom Grouping Not Working

**Issue:** Custom IGroupByColumnCategorizer not being used

**Solution:**

```csharp
// Ensure you're adding to GroupedColumns, not SortedColumns
SortColumnDescriptor descriptor = new SortColumnDescriptor("Age");
descriptor.Categorizer = new CustomAgeCategorizer();
gridGroupingControl1.TableDescriptor.GroupedColumns.Add(descriptor);
```

## Filtering Issues

### Filter Bar Not Showing

**Issue:** Filter bar is not visible

**Solutions:**

```csharp
// Enable filter bar
gridGroupingControl1.TopLevelGroupOptions.ShowFilterBar = true;

// Enable filtering for columns
foreach (GridColumnDescriptor column in gridGroupingControl1.TableDescriptor.Columns)
{
    column.AllowFilter = true;
}
```

### Excel Filter Not Working

**Issue:** GridExcelFilter not displaying

**Solutions:**

1. **Check Assembly Reference**

```csharp
// Ensure Syncfusion.GridHelperClasses.Windows.dll is referenced
using Syncfusion.GridHelperClasses;
```

2. **Wire Grid Correctly**

```csharp
GridExcelFilter excelFilter = new GridExcelFilter();
excelFilter.WireGrid(gridGroupingControl1);

// Not UnwireGrid unless intentionally removing
```

### Filter Not Applying

**Issue:** Setting RecordFilters doesn't filter data

**Solution:**

```csharp
// Ensure proper filter syntax
FilterCondition condition = new FilterCondition(FilterCompareOperator.Equals, "Active");
RecordFilterDescriptor filter = new RecordFilterDescriptor("Status", condition);
gridGroupingControl1.TableDescriptor.RecordFilters.Add(filter);

// For expressions, use correct syntax
RecordFilterDescriptor filter2 = new RecordFilterDescriptor("Age");
filter2.Expression = "[Age] > 25 AND [Age] < 65";
gridGroupingControl1.TableDescriptor.RecordFilters.Add(filter2);
```

## Sorting Issues

### Sorting Not Working

**Issue:** Clicking column header doesn't sort

**Solutions:**

```csharp
// Enable sorting
gridGroupingControl1.TableOptions.AllowSortColumns = true;

// Check if specific column allows sorting
gridGroupingControl1.TableDescriptor.Columns["Name"].AllowSort = true;
```

### Custom Sort Not Applied

**Issue:** Custom IComparer not being used

**Solution:**

```csharp
// Add to SortedColumns with custom comparer
SortColumnDescriptor sortDesc = new SortColumnDescriptor("CustomField");
sortDesc.Comparer = new CustomComparer();
gridGroupingControl1.TableDescriptor.SortedColumns.Add(sortDesc);
```

## UI Rendering Issues

### Columns Too Narrow/Wide

**Issue:** Column widths not appropriate

**Solutions:**

```csharp
// Auto-fit columns to content
gridGroupingControl1.TableModel.ColWidths.ResizeToFit(
    GridRangeInfo.Table(), 
    GridResizeToFitOptions.IncludeHeaders);

// Set specific column width
gridGroupingControl1.TableDescriptor.Columns["Description"].Width = 200;

// Allow user resizing
gridGroupingControl1.TableOptions.AllowResizeColumns = true;
```

### Appearance Not Updating

**Issue:** Style changes not reflecting

**Solutions:**

```csharp
// Invalidate and refresh
gridGroupingControl1.TableControl.Invalidate();
gridGroupingControl1.TableControl.Refresh();

// Or reset appearance
gridGroupingControl1.Appearance.ResetAnyRecordFieldCell();
```

### Text Cut Off in Cells

**Issue:** Text is truncated in cells

**Solutions:**

```csharp
// Enable word wrap
gridGroupingControl1.TableOptions.WrapText = true;

// Auto-fit row heights
gridGroupingControl1.TableModel.RowHeights.ResizeToFit(
    GridRangeInfo.Table(), 
    GridResizeToFitOptions.None);
```

## Event Issues

### Events Not Firing

**Issue:** Event handlers not being called

**Solutions:**

1. **Verify Event Subscription**

```csharp
// Ensure event is subscribed
gridGroupingControl1.QueryCellStyleInfo += GridGroupingControl1_QueryCellStyleInfo;

// Check method signature matches
private void GridGroupingControl1_QueryCellStyleInfo(object sender, GridTableCellStyleInfoEventArgs e)
{
    // Handler code
}
```

2. **Check Event Order**

```csharp
// Some events must be subscribed before data binding
gridGroupingControl1.QueryCellStyleInfo += Handler;
gridGroupingControl1.DataSource = data; // After event subscription
```

### Infinite Loop in Events

**Issue:** Application hangs in event handler

**Solution:**

```csharp
// ❌ BAD - Modifying grid state in QueryCellStyleInfo can cause infinite loop
gridGroupingControl1.QueryCellStyleInfo += (s, e) =>
{
    gridGroupingControl1.TableDescriptor.GroupedColumns.Clear(); // DON'T DO THIS
};

// ✅ GOOD - Only read and set style properties
gridGroupingControl1.QueryCellStyleInfo += (s, e) =>
{
    e.Style.BackColor = Color.LightGreen; // This is fine
};
```

## Export/Print Issues

### Export Fails

**Issue:** Export to Excel/PDF throws exception

**Solutions:**

```csharp
// Ensure required assemblies are referenced
// For Excel: Syncfusion.XlsIO.Base.dll
// For PDF: Syncfusion.Pdf.Base.dll

// Use try-catch for export operations
try
{
    GridExcelConverterControl excelConverter = new GridExcelConverterControl();
    excelConverter.ExcelExportingOptions = GridExcelExportingOptions.DefaultFormats;
    excelConverter.GroupingExcelExport(gridGroupingControl1, "output.xlsx");
}
catch (Exception ex)
{
    MessageBox.Show($"Export failed: {ex.Message}");
}
```

### Print Preview Blank

**Issue:** Print preview shows no content

**Solution:**

```csharp
// Ensure grid has data before printing
if (gridGroupingControl1.Table.Records.Count > 0)
{
    gridGroupingControl1.ShowPrintPreview();
}
```

## Common Exceptions

### NullReferenceException

**Common Causes:**

```csharp
// 1. Accessing TableControl before data binding
// ✅ FIX: Check for null
if (gridGroupingControl1.TableControl != null)
{
    gridGroupingControl1.TableControl.CurrentCell.MoveTo(5, 2);
}

// 2. Accessing columns before they exist
// ✅ FIX: Verify column exists
if (gridGroupingControl1.TableDescriptor.Columns.Contains("ProductName"))
{
    var column = gridGroupingControl1.TableDescriptor.Columns["ProductName"];
}
```

### InvalidOperationException

**Common Causes:**

```csharp
// 1. Modifying collection during iteration
// ❌ BAD
foreach (Record record in gridGroupingControl1.Table.Records)
{
    gridGroupingControl1.Table.Records.Remove(record); // Exception
}

// ✅ GOOD
var recordsToRemove = gridGroupingControl1.Table.Records.ToList();
foreach (Record record in recordsToRemove)
{
    gridGroupingControl1.Table.Records.Remove(record);
}
```

### ArgumentException

**Common Causes:**

```csharp
// 1. Invalid column name
// ✅ FIX: Verify column exists before using
string columnName = "NonExistentColumn";
if (gridGroupingControl1.TableDescriptor.Columns.Contains(columnName))
{
    gridGroupingControl1.TableDescriptor.GroupedColumns.Add(columnName);
}
```

## Best Practices

### Always Use BeginUpdate/EndUpdate

```csharp
// For multiple property changes
gridGroupingControl1.BeginUpdate();
try
{
    // Multiple operations
}
finally
{
    gridGroupingControl1.EndUpdate();
}
```

### Proper Error Handling

```csharp
try
{
    gridGroupingControl1.DataSource = GetDataFromDatabase();
}
catch (SqlException ex)
{
    MessageBox.Show($"Database error: {ex.Message}", "Error", 
        MessageBoxButtons.OK, MessageBoxIcon.Error);
    // Log error
}
catch (Exception ex)
{
    MessageBox.Show($"Unexpected error: {ex.Message}", "Error", 
        MessageBoxButtons.OK, MessageBoxIcon.Error);
}
```

### Initialize in Correct Order

```csharp
// Correct initialization order
1. Create control
2. Subscribe to events
3. Configure properties
4. Set DataSource
5. Apply grouping/sorting/filtering

public Form1()
{
    InitializeComponent();
    
    // 1. Create
    gridGroupingControl1 = new GridGroupingControl();
    
    // 2. Events
    gridGroupingControl1.QueryCellStyleInfo += Handler;
    
    // 3. Properties
    gridGroupingControl1.ShowGroupDropArea = true;
    
    // 4. Data
    gridGroupingControl1.DataSource = data;
    
    // 5. Grouping
    gridGroupingControl1.TableDescriptor.GroupedColumns.Add("Category");
}
```

### Check for Valid Data Before Operations

```csharp
// Always verify data exists
if (gridGroupingControl1.Table != null && 
    gridGroupingControl1.Table.Records.Count > 0)
{
    // Perform operations
}
```

## Debug Tips

### Enable Diagnostic Logging

```csharp
// Add trace output for debugging
gridGroupingControl1.TableDescriptor.RecordFilters.Changed += (s, e) =>
{
    System.Diagnostics.Debug.WriteLine($"Filter changed: {e.Action}");
};
```

### Inspect Grid State

```csharp
// Check current state
Debug.WriteLine($"Record Count: {gridGroupingControl1.Table.Records.Count}");
Debug.WriteLine($"Grouped Columns: {gridGroupingControl1.TableDescriptor.GroupedColumns.Count}");
Debug.WriteLine($"Sorted Columns: {gridGroupingControl1.TableDescriptor.SortedColumns.Count}");
Debug.WriteLine($"Filters: {gridGroupingControl1.TableDescriptor.RecordFilters.Count}");
```

### Use Immediate Window

```csharp
// In Visual Studio, use Immediate Window (Ctrl+Alt+I) during debugging:
? gridGroupingControl1.DataSource
? gridGroupingControl1.Table.Records.Count
? gridGroupingControl1.TableDescriptor.Columns["Name"].HeaderText
```

## Getting Help

### Resources

1. **Official Documentation:** https://help.syncfusion.com/windowsforms/gridgrouping/overview
2. **API Reference:** https://help.syncfusion.com/cr/windowsforms
3. **Knowledge Base:** https://support.syncfusion.com/kb/windowsforms/gridgrouping
4. **Forum:** https://www.syncfusion.com/forums/windowsforms
5. **Support:** https://www.syncfusion.com/support/directtrac

### Providing Information for Support

When requesting help, include:

1. **Syncfusion version** and **.NET Framework version**
2. **Complete exception** message and stack trace
3. **Minimal reproducible example** of the issue
4. **Steps to reproduce** the problem
5. **Expected vs actual** behavior

### Sample Template for Support Request

```
Syncfusion Version: 25.1.35
.NET Framework: 4.8
OS: Windows 11

Issue: GridGroupingControl not displaying grouped data

Steps to Reproduce:
1. Create GridGroupingControl
2. Bind to DataTable with 100 records
3. Add grouping: gridGroupingControl1.TableDescriptor.GroupedColumns.Add("Category");
4. Groups appear but no records are visible

Expected: Records should be visible under each group
Actual: Only group captions are visible

Code Sample:
[Include minimal code that demonstrates the issue]

Exception (if any):
[Include full exception message and stack trace]
```

## Common Pitfalls to Avoid

1. **Don't modify collections during iteration** - Use ToList() first
2. **Don't perform heavy operations in QueryCellStyleInfo** - Keep it fast
3. **Don't forget to call BeginUpdate/EndUpdate** - For bulk operations
4. **Don't access UI from non-UI threads** - Use Invoke/BeginInvoke
5. **Don't keep references to Records** - They can become invalid
6. **Don't ignore exceptions** - Always handle and log errors
7. **Don't assume column order** - Access by name, not index
8. **Don't mix Designer and code-based configuration** - Choose one approach
