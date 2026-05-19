# Filtering in GridGroupingControl

## Table of Contents
- [Overview](#overview)
- [Setting Up Filter Bar](#setting-up-filter-bar)
- [Record Filters (Programmatic)](#record-filters-programmatic)
- [Dynamic Filter](#dynamic-filter)
- [Excel-Like Filter](#excel-like-filter)
- [Common Scenarios](#common-scenarios)

## Overview

GridGroupingControl provides three types of filtering:

1. **Record Filters:** Programmatic filtering with expressions and conditions
2. **Dynamic Filter:** Interactive filter with real-time results as you type
3. **Excel-Like Filter:** Dialog-based filtering similar to Microsoft Excel

All filters require enabling `AllowFilter` property on columns and filter bar visibility.

## Setting Up Filter Bar

### Enable Filter Bar

```csharp
// Show filter bar for top-level table
gridGroupingControl1.TopLevelGroupOptions.ShowFilterBar = true;

// Enable filtering for all columns
foreach (GridColumnDescriptor column in gridGroupingControl1.TableDescriptor.Columns)
{
    column.AllowFilter = true;
}
```

### Filter Bar for Nested Tables and Groups

```csharp
// Filter bar for nested tables
gridGroupingControl1.NestedTableGroupOptions.ShowFilterBar = true;

// Filter bar for groups
gridGroupingControl1.ChildGroupOptions.ShowFilterBar = true;
```

### Remove Custom and Empty Options

```csharp
// Hide "Custom" and "Empty" from filter dropdown
gridGroupingControl1.TableDescriptor.Columns["ProductName"].FilterRowOptions.AllowCustomFilter = false;
gridGroupingControl1.TableDescriptor.Columns["ProductName"].FilterRowOptions.AllowEmptyFilter = false;
```

## Record Filters (Programmatic)

Programmatic filtering using `RecordFilterDescriptor` with flexible conditions.

### Basic Record Filter

```csharp
// Filter: Country = "USA"
FilterCondition condition = new FilterCondition(FilterCompareOperator.Equals, "USA");
RecordFilterDescriptor filter = new RecordFilterDescriptor("Country", condition);
gridGroupingControl1.TableDescriptor.RecordFilters.Add(filter);
```

### Filter Compare Operators

| Operator | Description |
|----------|-------------|
| `Equals` | Value is equal |
| `NotEquals` | Value is not equal |
| `LessThan` | Left value < right value |
| `LessThanOrEqualTo` | Left value ≤ right value |
| `GreaterThan` | Left value > right value |
| `GreaterThanOrEqualTo` | Left value ≥ right value |
| `Like` | String matches pattern with wildcards (*, ?, [list]) |
| `Match` | String matches regex pattern |
| `Custom` | Custom ICustomFilter implementation |

### Filter by Expression

```csharp
// Filter: SupplierID <= 10
RecordFilterDescriptor filter = new RecordFilterDescriptor("SupplierID");
filter.Expression = "[SupplierID] <= 10";
gridGroupingControl1.TableDescriptor.RecordFilters.Add(filter);
```

**Common expressions:**
```csharp
// Numeric range
filter.Expression = "[UnitPrice] >= 10 AND [UnitPrice] <= 50";

// Date range
filter.Expression = "[OrderDate] >= #1/1/2020# AND [OrderDate] < #1/1/2021#";

// String contains
filter.Expression = "[ProductName] LIKE '*Chai*'";

// Computed field
filter.Expression = "[Quantity] * [UnitPrice] > 1000";
```

### Multiple Filter Conditions

```csharp
// OR condition: Country = "USA" OR Country = "UK"
RecordFilterDescriptor filter1 = new RecordFilterDescriptor("Country", 
    new FilterCondition(FilterCompareOperator.Equals, "USA"));
    
RecordFilterDescriptor filter2 = new RecordFilterDescriptor("Country", 
    new FilterCondition(FilterCompareOperator.Equals, "UK"));
filter2.LogicalOperator = FilterLogicalOperator.Or;

gridGroupingControl1.TableDescriptor.RecordFilters.Add(filter1);
gridGroupingControl1.TableDescriptor.RecordFilters.Add(filter2);
```

### Special Characters in Filter Values

Escape special characters in filter patterns:

```csharp
private string EscapeFilterPattern(string pattern)
{
    pattern = pattern.Replace("[", "[[]");
    pattern = pattern.Replace("#", "[#]");
    pattern = pattern.Replace("*", "[*]");
    pattern = pattern.Replace("?", "[?]");
    return pattern;
}

// Usage
string searchTerm = "Product[A]";
string filter = $"[ProductName] LIKE '{EscapeFilterPattern(searchTerm)}'";
RecordFilterDescriptor descriptor = new RecordFilterDescriptor(filter);
gridGroupingControl1.TableDescriptor.RecordFilters.Add(descriptor);
```

### Filter Nested Tables

```csharp
// Filter child table named "Products" where UnitPrice > 10
FilterCondition condition = new FilterCondition(FilterCompareOperator.GreaterThan, 10);
RecordFilterDescriptor filter = new RecordFilterDescriptor("UnitPrice", condition);

// Get child table descriptor and add filter
gridGroupingControl1.GetTableDescriptor("Products").RecordFilters.Add(filter);
```

### Remove and Clear Filters

```csharp
// Remove specific filter by column name
gridGroupingControl1.TableDescriptor.RecordFilters.Remove("ProductName");

// Remove by index
gridGroupingControl1.TableDescriptor.RecordFilters.RemoveAt(0);

// Clear all filters
gridGroupingControl1.TableDescriptor.RecordFilters.Clear();
```

### Record Filter Events

```csharp
gridGroupingControl1.TableDescriptor.RecordFilters.Changing += (s, e) =>
{
    if (e.Action == ListPropertyChangedType.Add)
    {
        Console.WriteLine("Adding record filter");
    }
};

gridGroupingControl1.TableDescriptor.RecordFilters.Changed += (s, e) =>
{
    if (e.Action == ListPropertyChangedType.Add)
    {
        Console.WriteLine("Record filter added");
    }
};
```

## Dynamic Filter

Real-time filtering that updates results as you type. Requires `Syncfusion.GridHelperClasses.Windows.dll`.

### Enable Dynamic Filter

```csharp
using Syncfusion.GridHelperClasses;

GridDynamicFilter dynamicFilter = new GridDynamicFilter();
dynamicFilter.WireGrid(gridGroupingControl1);
```

### Set Filtering Delay

```csharp
GridDynamicFilter filter = new GridDynamicFilter();

// Delay filtering by 300 milliseconds
filter.FilterDelay = 300;

filter.WireGrid(gridGroupingControl1);
```

### Apply Filter Only on Lost Focus

```csharp
GridDynamicFilter filter = new GridDynamicFilter();

// Filter only when cell loses focus (not on every keystroke)
filter.ApplyFilterOnlyOnCellLostFocus = true;

filter.WireGrid(gridGroupingControl1);
```

### Filter by Display Text

Filter combo box columns by display member instead of value member:

```csharp
// Filter ComboBox by display text
gridGroupingControl1.TableDescriptor.Columns["Category"]
    .FilterRowOptions.FilterMode = FilterMode.DisplayText;

GridDynamicFilter filter = new GridDynamicFilter();
filter.WireGrid(gridGroupingControl1);
```

### Filter by Formatted Text

```csharp
// Filter currency column by formatted text (e.g., "$1,500.00")
gridGroupingControl1.TableDescriptor.Columns["UnitPrice"]
    .FilterRowOptions.FilterMode = FilterMode.DisplayText;
```

### Wire Filter to Individual Columns

```csharp
GridDynamicFilter filter = new GridDynamicFilter();

// Enable individual column wiring
filter.AllowIndividualColumnWiring = true;

// Apply to specific columns
gridGroupingControl1.TableDescriptor.Columns["ProductName"]
    .Appearance.FilterBarCell.CellType = "DynamicFilterCell";

filter.WireGrid(gridGroupingControl1);
```

### Dynamic Filter Events

```csharp
dynamicFilter.ShowingCustomFilterDialog += (s, e) =>
{
    // Customize filter dialog before it shows
    // e.Control is the dialog control
};
```

## Excel-Like Filter

Dialog-based filtering similar to Microsoft Excel. Requires `Syncfusion.GridHelperClasses.Windows.dll`.

### Enable Excel Filter (Optimized)

```csharp
using Syncfusion.GridHelperClasses;

GridExcelFilter excelFilter = new GridExcelFilter();
excelFilter.WireGrid(gridGroupingControl1);
```

### Enable Number Filter

```csharp
GridExcelFilter excelFilter = new GridExcelFilter();
excelFilter.EnableNumberFilter = true;
excelFilter.WireGrid(gridGroupingControl1);
```

Number filter options:
- Equals, Does Not Equal
- Greater Than, Greater Than or Equal To
- Less Than, Less Than or Equal To
- Between, Custom

### Enable Date Filter

```csharp
GridExcelFilter excelFilter = new GridExcelFilter();
excelFilter.EnableDateFilter = true;
excelFilter.WireGrid(gridGroupingControl1);
```

Date filter options:
- Equals, Before, After, Between
- Today, Yesterday, Tomorrow
- This Week, Last Week, Next Week
- This Month, Last Month, Next Month
- This Year, Last Year, Next Year
- Custom date ranges

### Filter by Color

```csharp
GridExcelFilter excelFilter = new GridExcelFilter();
excelFilter.AllowFilterByColor = true;
excelFilter.WireGrid(gridGroupingControl1);
```

Filters by:
- Cell Background Color
- Cell Text Color

### Show Filter Icon on Hover

```csharp
GridExcelFilter filter = new GridExcelFilter();
filter.WireGrid(gridGroupingControl1);

// Show filter icon only on column header hover
GridExcelFilter.EnableFilteredColumnIcon = true;
```

### Remove Search Box

```csharp
GridExcelFilter filter = new GridExcelFilter();

// Hide search textbox in filter dialog
filter.AllowSearch = false;

filter.WireGrid(gridGroupingControl1);
```

### Enable Filter Dialog Resizing

```csharp
GridExcelFilter filter = new GridExcelFilter();

// Allow resizing filter popup
filter.AllowResize = true;

filter.WireGrid(gridGroupingControl1);
```

### Wire Excel Filter to Individual Columns

```csharp
GridExcelFilter filter = new GridExcelFilter();

// Enable individual column wiring
filter.AllowIndividualColumnWiring = true;

// Apply to specific column
gridGroupingControl1.TableDescriptor.Columns["Category"]
    .Appearance.ColumnHeaderCell.CellType = "GridExcelFilterCell";

filter.WireGrid(gridGroupingControl1);
```

### Customize Filter Icons

Override cell renderer for custom filter icons:

```csharp
public class GridExcelFilterCellRendererCustom : GridExcelFilterCellRenderer
{
    public GridExcelFilterCellRendererCustom(GridControlBase grid, 
        GridCellModelBase cellModel, GridExcelFilter excelFilter)
        : base(grid, cellModel, excelFilter)
    {
        // Set custom icons
        base.FilterIconSize = new Size(24, 24);
        base.FilterIcon = new Bitmap("CustomFilter.png");
        base.FilteredIcon = new Bitmap("CustomFiltered.png");
    }
}

// Register custom renderer
gridGroupingControl1.TableControl.CellRenderers["ColumnHeaderCell"] = 
    new GridExcelFilterCellRendererCustom(
        gridGroupingControl1.TableControl,
        new GridExcelFilterCellModel(gridGroupingControl1.TableModel, excelFilter),
        excelFilter);
```

## Common Scenarios

### Scenario 1: Filter by Multiple Values (OR)

```csharp
// Filter: Country = "USA" OR Country = "UK" OR Country = "Germany"
string[] countries = { "USA", "UK", "Germany" };
string expression = string.Join(" OR ", 
    countries.Select(c => $"[Country] = '{c}'"));

RecordFilterDescriptor filter = new RecordFilterDescriptor("Country");
filter.Expression = expression;
gridGroupingControl1.TableDescriptor.RecordFilters.Add(filter);
```

### Scenario 2: Filter by Date Range

```csharp
// Filter orders from last 30 days
DateTime startDate = DateTime.Now.AddDays(-30);
DateTime endDate = DateTime.Now;

string expression = $"[OrderDate] >= #{startDate:M/d/yyyy}# AND " +
                    $"[OrderDate] <= #{endDate:M/d/yyyy}#";

RecordFilterDescriptor filter = new RecordFilterDescriptor("OrderDate");
filter.Expression = expression;
gridGroupingControl1.TableDescriptor.RecordFilters.Add(filter);
```

### Scenario 3: Filter Null or Empty Values

```csharp
// Show only records with non-empty ProductName
FilterCondition condition = new FilterCondition(FilterCompareOperator.NotEquals, null);
RecordFilterDescriptor filter = new RecordFilterDescriptor("ProductName", condition);
gridGroupingControl1.TableDescriptor.RecordFilters.Add(filter);

// Or using expression
filter.Expression = "[ProductName] IS NOT NULL AND [ProductName] <> ''";
```

### Scenario 4: Filter by Computed Value

```csharp
// Filter where Total (Quantity * UnitPrice) > 1000
RecordFilterDescriptor filter = new RecordFilterDescriptor();
filter.Expression = "[Quantity] * [UnitPrice] > 1000";
gridGroupingControl1.TableDescriptor.RecordFilters.Add(filter);
```

### Scenario 5: Case-Insensitive String Filter

```csharp
// Filter ProductName contains "chai" (case-insensitive)
RecordFilterDescriptor filter = new RecordFilterDescriptor("ProductName");
filter.Expression = "LOWER([ProductName]) LIKE '*chai*'";
gridGroupingControl1.TableDescriptor.RecordFilters.Add(filter);
```

### Scenario 6: Combine Record and Dynamic Filters

```csharp
// Apply programmatic filter first
FilterCondition condition = new FilterCondition(FilterCompareOperator.GreaterThan, 10);
RecordFilterDescriptor recordFilter = new RecordFilterDescriptor("UnitPrice", condition);
gridGroupingControl1.TableDescriptor.RecordFilters.Add(recordFilter);

// Then enable dynamic filter for additional user filtering
GridDynamicFilter dynamicFilter = new GridDynamicFilter();
dynamicFilter.WireGrid(gridGroupingControl1);
```

### Scenario 7: Clear All Filters

```csharp
private void ClearAllFilters()
{
    // Clear record filters
    gridGroupingControl1.TableDescriptor.RecordFilters.Clear();
    
    // Clear nested table filters
    foreach (GridRelationDescriptor relation in gridGroupingControl1.TableDescriptor.Relations)
    {
        relation.ChildTableDescriptor.RecordFilters.Clear();
    }
    
    // Refresh grid
    gridGroupingControl1.TableControl.Refresh();
}
```

## Best Practices

1. **Choose appropriate filter type:**
   - **Record Filters:** Complex conditions, server-side filtering, persistent filters
   - **Dynamic Filter:** Interactive user filtering with real-time feedback
   - **Excel Filter:** Familiar Excel-like experience for end users

2. **Performance optimization:**
   - Use `BeginUpdate()`/`EndUpdate()` when adding multiple filters
   - Enable `FilterDelay` for Dynamic Filter to reduce filter operations
   - Consider indexed fields for large datasets

3. **User experience:**
   - Show filter bar by default for filterable grids
   - Provide "Clear Filters" button for easy reset
   - Use `ApplyFilterOnlyOnCellLostFocus` for performance-sensitive scenarios

4. **Expressions:**
   - Test complex expressions with sample data
   - Escape special characters in user input
   - Use typed values (dates with #, strings with ')

## Common Issues

### Filter Not Working

```csharp
// Verify AllowFilter is enabled
gridGroupingControl1.TableDescriptor.Columns["ColumnName"].AllowFilter = true;

// Verify filter bar is visible
gridGroupingControl1.TopLevelGroupOptions.ShowFilterBar = true;

// Check data type matches filter value
// (e.g., numeric column needs numeric value, not string)
```

### Dynamic Filter Not Appearing

```csharp
// Ensure Syncfusion.GridHelperClasses.Windows.dll is referenced
// Verify WireGrid is called AFTER data binding
gridGroupingControl1.DataSource = dataTable;
dynamicFilter.WireGrid(gridGroupingControl1);
```

### Special Characters Breaking Filter

```csharp
// Always escape special characters: [ ] # * ?
string userInput = "Product[A]*";
string escaped = EscapeFilterPattern(userInput);
filter.Expression = $"[ProductName] LIKE '{escaped}'";
```
