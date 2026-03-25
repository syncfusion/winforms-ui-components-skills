# Summaries in GridGroupingControl

## Overview

Summaries derive aggregate information from data (count, sum, average, min, max). GridGroupingControl displays summaries in dedicated rows at the bottom of tables and within group captions.

**Built-in summary types:**
- `Int32Aggregate`, `DoubleAggregate` - Count, Min, Max, Sum, Average
- `StringAggregate` - MaxLength, Count
- `Count` - Record count
- `DistinctCount` - Unique value count
- `Vector` - Values array
- `DoubleVector` - Statistical methods (Median, Quartiles)
- `Custom` - Custom summary implementations

## Adding Summaries Programmatically

### Basic Summary

```csharp
// Step 1: Create summary column descriptor
GridSummaryColumnDescriptor summaryColumn = new GridSummaryColumnDescriptor();
summaryColumn.DataMember = "UnitPrice";
summaryColumn.Format = "Total: {Sum:C}";
summaryColumn.Name = "PriceSum";
summaryColumn.SummaryType = SummaryType.DoubleAggregate;

// Step 2: Create summary row descriptor
GridSummaryRowDescriptor summaryRow = new GridSummaryRowDescriptor();
summaryRow.SummaryColumns.Add(summaryColumn);

// Step 3: Add to grid
gridGroupingControl1.TableDescriptor.SummaryRows.Add(summaryRow);
```

### Summary Column Descriptor Constructor

```csharp
// Shorthand constructor
GridSummaryColumnDescriptor summaryColumn = new GridSummaryColumnDescriptor(
    "PriceSum",                    // Name
    SummaryType.DoubleAggregate,   // Type
    "UnitPrice",                   // Data member
    "Total: {Sum:C}"               // Format
);
```

### Format Strings

Available format placeholders:
- `{Count}` - Number of records
- `{Sum}` - Sum of values
- `{Average}` or `{Average:#.00}` - Average value
- `{Min}`, `{Max}` - Minimum/maximum values
- `{MaxLength}` - Maximum string length (StringAggregate)

```csharp
// Examples
summaryColumn.Format = "{Count} items";
summaryColumn.Format = "Total: ${Sum:#,##0.00}";
summaryColumn.Format = "Avg: {Average:F2}";
summaryColumn.Format = "Range: {Min} - {Max}";
```

## Multi-Column Summaries

Display summaries for multiple columns in single row:

```csharp
// Create summary for Wins column
GridSummaryColumnDescriptor winsSum = new GridSummaryColumnDescriptor(
    "Wins", SummaryType.Int32Aggregate, "Wins", "{Sum}");

// Create summary for Losses column
GridSummaryColumnDescriptor lossesSum = new GridSummaryColumnDescriptor(
    "Losses", SummaryType.Int32Aggregate, "Losses", "{Sum}");

// Add both to same row
GridSummaryRowDescriptor summaryRow = new GridSummaryRowDescriptor();
summaryRow.SummaryColumns.AddRange(new GridSummaryColumnDescriptor[] { winsSum, lossesSum });

gridGroupingControl1.TableDescriptor.SummaryRows.Add(summaryRow);
```

## Multi-Row Summaries

Display multiple summary rows with different calculations:

```csharp
// Row 1: Count
GridSummaryColumnDescriptor countSummary = new GridSummaryColumnDescriptor(
    "FreightCount", SummaryType.DoubleAggregate, "Freight", "Total: {Count}");
GridSummaryRowDescriptor row1 = new GridSummaryRowDescriptor();
row1.SummaryColumns.Add(countSummary);

// Row 2: Average
GridSummaryColumnDescriptor avgSummary = new GridSummaryColumnDescriptor(
    "FreightAvg", SummaryType.DoubleAggregate, "Freight", "Avg: {Average:#.00}");
GridSummaryRowDescriptor row2 = new GridSummaryRowDescriptor();
row2.SummaryColumns.Add(avgSummary);

// Add both rows
gridGroupingControl1.TableDescriptor.SummaryRows.Add(row1);
gridGroupingControl1.TableDescriptor.SummaryRows.Add(row2);
```

## Nested Tables and Group Summaries

Add summaries to nested tables and groups:

### Nested Table Summaries

```csharp
// Add summary to child table
GridSummaryColumnDescriptor childSummary = new GridSummaryColumnDescriptor(
    "OrderTotal", SummaryType.DoubleAggregate, "Total", "{Sum:C}");

GridSummaryRowDescriptor childRow = new GridSummaryRowDescriptor("Sum", "$", childSummary);

// Access child table descriptor via relation
gridGroupingControl1.TableDescriptor.Relations[0]
    .ChildTableDescriptor.SummaryRows.Add(childRow);
```

### Group Summaries

Summaries automatically calculate for each group:

```csharp
// Add summary
GridSummaryColumnDescriptor summary = new GridSummaryColumnDescriptor(
    "CategoryTotal", SummaryType.DoubleAggregate, "UnitPrice", "{Sum:C}");
GridSummaryRowDescriptor row = new GridSummaryRowDescriptor();
row.SummaryColumns.Add(summary);
gridGroupingControl1.TableDescriptor.SummaryRows.Add(row);

// Group by Category
gridGroupingControl1.TableDescriptor.GroupedColumns.Add("Category");

// Summary appears at bottom of each category group
```

## Summary in Caption

Display summaries in group caption rows instead of separate rows:

```csharp
// Step 1: Define summary
GridSummaryColumnDescriptor summaryColumn = new GridSummaryColumnDescriptor(
    "Sum", SummaryType.DoubleAggregate, "UnitPrice", "{Sum:C}");
GridSummaryRowDescriptor summaryRow = new GridSummaryRowDescriptor("Sum", "$", summaryColumn);
gridGroupingControl1.TableDescriptor.SummaryRows.Add(summaryRow);

// Step 2: Group table
gridGroupingControl1.ShowGroupDropArea = true;
gridGroupingControl1.TableDescriptor.GroupedColumns.Add("Category");

// Step 3: Enable caption summaries
gridGroupingControl1.ChildGroupOptions.ShowCaptionSummaryCells = true;
gridGroupingControl1.ChildGroupOptions.ShowSummaries = false;  // Hide separate summary rows

// Step 4: Specify which summary to show in caption
gridGroupingControl1.ChildGroupOptions.CaptionSummaryRow = "Sum";
gridGroupingControl1.ChildGroupOptions.CaptionText = "{RecordCount} Items";

// Step 5: Format caption cells
gridGroupingControl1.Appearance.GroupCaptionCell.BackColor = 
    gridGroupingControl1.Appearance.RecordFieldCell.BackColor;
gridGroupingControl1.Appearance.GroupCaptionCell.Borders.Top = 
    new GridBorder(GridBorderStyle.Standard);
gridGroupingControl1.Appearance.GroupCaptionCell.CellType = "Static";
```

## Sort by Summary

Sort groups based on summary values instead of field values:

```csharp
// Step 1: Define summary
GridSummaryColumnDescriptor summaryColumn = new GridSummaryColumnDescriptor(
    "FreightAverage", SummaryType.DoubleAggregate, "Freight", "{Average:###.00}");
GridSummaryRowDescriptor summaryRow = new GridSummaryRowDescriptor();
summaryRow.Name = "Caption";
summaryRow.SummaryColumns.Add(summaryColumn);
gridGroupingControl1.TableDescriptor.SummaryRows.Add(summaryRow);

// Step 2: Enable caption summaries
gridGroupingControl1.ChildGroupOptions.ShowCaptionSummaryCells = true;
gridGroupingControl1.ChildGroupOptions.CaptionSummaryRow = "Caption";
gridGroupingControl1.ChildGroupOptions.ShowSummaries = false;

// Step 3: Create sort descriptor with summary-based order
SortColumnDescriptor sortDescriptor = new SortColumnDescriptor("ShipCountry");

// Sort groups by average Freight value
sortDescriptor.SetGroupSummarySortOrder(
    summaryColumn.GetSummaryDescriptorName(), 
    "Average");

// Step 4: Group with custom sort
gridGroupingControl1.TableDescriptor.GroupedColumns.Clear();
gridGroupingControl1.TableDescriptor.GroupedColumns.Add(sortDescriptor);
```

## Update Summaries Immediately

By default, summaries update when leaving a record. Update immediately on cell change:

### Method 1: ForceImmediateSaveValue

```csharp
// Update summary when leaving cell (not record)
gridGroupingControl1.TableDescriptor.Fields["UnitPrice"].ForceImmediateSaveValue = true;
```

### Method 2: CurrentRecordContextChange Event

```csharp
gridGroupingControl1.CurrentRecordContextChange += (s, e) =>
{
    if (e.Action == CurrentRecordAction.CurrentFieldChanged)
    {
        // End editing and update summaries
        gridGroupingControl1.CurrencyManager.EndCurrentEdit();
        gridGroupingControl1.Table.InvalidateSummary();
    }
};
```

## Custom Summaries

Create custom summary calculations by implementing `ICustomSummary`:

```csharp
public class CustomMedianSummary : ICustomSummary
{
    private List<double> values = new List<double>();
    
    public void Combine(ICustomSummary other)
    {
        CustomMedianSummary otherSummary = other as CustomMedianSummary;
        if (otherSummary != null)
        {
            values.AddRange(otherSummary.values);
        }
    }
    
    public ICustomSummary CreateNew()
    {
        return new CustomMedianSummary();
    }
    
    public void Final()
    {
        // Sort for median calculation
        values.Sort();
    }
    
    public void Union(Record record, FieldDescriptor fieldDescriptor)
    {
        object value = record.GetValue(fieldDescriptor);
        if (value != null && value != DBNull.Value)
        {
            values.Add(Convert.ToDouble(value));
        }
    }
    
    public double Median
    {
        get
        {
            if (values.Count == 0) return 0;
            
            int mid = values.Count / 2;
            if (values.Count % 2 == 0)
                return (values[mid - 1] + values[mid]) / 2.0;
            else
                return values[mid];
        }
    }
}

// Usage
GridSummaryColumnDescriptor summaryColumn = new GridSummaryColumnDescriptor();
summaryColumn.DataMember = "UnitPrice";
summaryColumn.Format = "Median: {Median:C}";
summaryColumn.Name = "MedianPrice";
summaryColumn.SummaryType = SummaryType.Custom;

// Create custom summary instance
CustomMedianSummary customSummary = new CustomMedianSummary();
FilterCondition condition = new FilterCondition(customSummary);
summaryColumn.SetCustomSummary(condition);

GridSummaryRowDescriptor summaryRow = new GridSummaryRowDescriptor();
summaryRow.SummaryColumns.Add(summaryColumn);
gridGroupingControl1.TableDescriptor.SummaryRows.Add(summaryRow);
```

## Appearance Customization

### Summary Row Appearance

```csharp
GridSummaryRowDescriptor summaryRow = new GridSummaryRowDescriptor();

// Set background color
summaryRow.Appearance.AnyCell.BackColor = Color.LightYellow;

// Set text alignment
summaryRow.Appearance.AnyCell.HorizontalAlignment = GridHorizontalAlignment.Right;

// Set font
summaryRow.Appearance.AnyCell.Font.Bold = true;
summaryRow.Appearance.AnyCell.TextColor = Color.DarkBlue;

summaryRow.SummaryColumns.Add(summaryColumn);
gridGroupingControl1.TableDescriptor.SummaryRows.Add(summaryRow);
```

### Individual Summary Column Appearance

```csharp
GridSummaryColumnDescriptor summaryColumn = new GridSummaryColumnDescriptor(
    "Total", SummaryType.DoubleAggregate, "UnitPrice", "{Sum:C}");

// Set appearance for this summary column only
summaryColumn.Appearance.AnySummaryCell.BackColor = Color.LightGreen;
summaryColumn.Appearance.AnySummaryCell.Font.Bold = true;
summaryColumn.Appearance.AnySummaryCell.HorizontalAlignment = GridHorizontalAlignment.Right;
```

## Common Scenarios

### Scenario 1: Count, Sum, and Average in One Row

```csharp
GridSummaryColumnDescriptor[] columns = new[]
{
    new GridSummaryColumnDescriptor("Count", SummaryType.Int32Aggregate, "OrderID", "Count: {Count}"),
    new GridSummaryColumnDescriptor("Sum", SummaryType.DoubleAggregate, "Total", "Sum: {Sum:C}"),
    new GridSummaryColumnDescriptor("Avg", SummaryType.DoubleAggregate, "Total", "Avg: {Average:C2}")
};

GridSummaryRowDescriptor summaryRow = new GridSummaryRowDescriptor();
summaryRow.SummaryColumns.AddRange(columns);
gridGroupingControl1.TableDescriptor.SummaryRows.Add(summaryRow);
```

### Scenario 2: Conditional Summary Display

```csharp
gridGroupingControl1.QueryCellStyleInfo += (s, e) =>
{
    Element el = e.TableCellIdentity.DisplayElement;
    
    if (el is SummarySection)
    {
        SummarySection summary = el as SummarySection;
        
        // Get summary value
        double sumValue = summary.GetSummaryValue("Total", "Sum");
        
        // Highlight high totals
        if (sumValue > 10000)
        {
            e.Style.BackColor = Color.LightCoral;
            e.Style.Font.Bold = true;
        }
    }
};
```

### Scenario 3: Summary for Visible Records Only

```csharp
// After applying filter, summaries automatically calculate for visible records
FilterCondition condition = new FilterCondition(FilterCompareOperator.GreaterThan, 100);
RecordFilterDescriptor filter = new RecordFilterDescriptor("UnitPrice", condition);
gridGroupingControl1.TableDescriptor.RecordFilters.Add(filter);

// Summaries now show totals for filtered records only
```

### Scenario 4: Different Summaries per Group Level

```csharp
// Parent group: Show count
gridGroupingControl1.TableDescriptor.GroupedColumns.Add("Country");
GridSummaryColumnDescriptor countSummary = new GridSummaryColumnDescriptor(
    "CountryCount", SummaryType.Int32Aggregate, "OrderID", "{Count} Orders");
GridSummaryRowDescriptor row1 = new GridSummaryRowDescriptor();
row1.SummaryColumns.Add(countSummary);
gridGroupingControl1.TableDescriptor.SummaryRows.Add(row1);

// Child group: Show sum
gridGroupingControl1.TableDescriptor.GroupedColumns.Add("City");
GridSummaryColumnDescriptor sumSummary = new GridSummaryColumnDescriptor(
    "CitySum", SummaryType.DoubleAggregate, "Total", "{Sum:C}");
GridSummaryRowDescriptor row2 = new GridSummaryRowDescriptor();
row2.SummaryColumns.Add(sumSummary);
// Note: Use same SummaryRows collection - grid handles group-level display
```

### Scenario 5: Percentage Calculations

```csharp
// Add expression field for percentage
ExpressionFieldDescriptor percentField = new ExpressionFieldDescriptor(
    "SuccessRate",
    "([Wins] / ([Wins] + [Losses])) * 100",
    "System.Double");
gridGroupingControl1.TableDescriptor.ExpressionFields.Add(percentField);

// Add summary for percentage
GridSummaryColumnDescriptor percentSummary = new GridSummaryColumnDescriptor(
    "AvgSuccess", SummaryType.DoubleAggregate, "SuccessRate", "Avg: {Average:F1}%");
GridSummaryRowDescriptor row = new GridSummaryRowDescriptor();
row.SummaryColumns.Add(percentSummary);
gridGroupingControl1.TableDescriptor.SummaryRows.Add(row);
```

## Best Practices

1. **Name summaries:** Use descriptive names for `GridSummaryRowDescriptor.Name` (helpful for caption summaries)
2. **Format appropriately:** Use format strings ({Sum:C}, {Average:F2}) for readable output
3. **Limit summary rows:** 2-3 summary rows maximum for readability
4. **Use caption summaries:** For grouped data, prefer caption summaries over separate rows
5. **Update strategy:** Use `ForceImmediateSaveValue` for frequently-changing fields only
6. **Custom summaries:** Implement for complex calculations (median, mode, standard deviation)

## Common Issues

### Summary Not Appearing

```csharp
// Verify summary column data member matches field name
summaryColumn.DataMember = "UnitPrice";  // Must match exactly

// Verify summary type matches data type
// Use DoubleAggregate for decimal/float, Int32Aggregate for integers

// Check if summary row is added
if (!gridGroupingControl1.TableDescriptor.SummaryRows.Contains(summaryRow))
{
    gridGroupingControl1.TableDescriptor.SummaryRows.Add(summaryRow);
}
```

### Caption Summary Not Showing

```csharp
// Verify settings
gridGroupingControl1.ChildGroupOptions.ShowCaptionSummaryCells = true;
gridGroupingControl1.ChildGroupOptions.ShowSummaries = false;

// Verify summary row name matches CaptionSummaryRow
summaryRow.Name = "Sum";
gridGroupingControl1.ChildGroupOptions.CaptionSummaryRow = "Sum";  // Must match

// Verify table is grouped
if (gridGroupingControl1.TableDescriptor.GroupedColumns.Count == 0)
{
    gridGroupingControl1.TableDescriptor.GroupedColumns.Add("Category");
}
```

### Summary Value Is Zero

```csharp
// Check for null values in data
// Use filter or custom summary to handle nulls

// Verify data type matches summary type
// Example: Int32Aggregate won't work on decimal columns
summaryColumn.SummaryType = SummaryType.DoubleAggregate;  // Use for decimal
```