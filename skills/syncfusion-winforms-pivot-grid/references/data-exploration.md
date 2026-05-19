# Data Exploration

## Table of Contents
- [Overview](#overview)
- [Hyperlink Cells](#hyperlink-cells)
- [Interactive Navigation](#interactive-navigation)
- [Tooltips](#tooltips)

## Overview

Data exploration features enable users to navigate through pivot data interactively, drill into details, and visualize patterns through hyperlinks and drill-through functionality.

## Hyperlink Cells

Hyperlink cells allow users to click on values to drill into underlying data or navigate to related information.

### Enabling Hyperlinks

```csharp
using Syncfusion.Windows.Forms.PivotAnalysis;

// Enable hyperlink cells
this.pivotGridControl1.TableModel.QueryCellInfo += TableModel_QueryCellInfo;

private void TableModel_QueryCellInfo(object sender, Syncfusion.Windows.Forms.Grid.GridQueryCellInfoEventArgs e)
{
    if (e.RowIndex > this.pivotGridControl1.PivotColumns.Count + (this.pivotGridControl1.PivotCalculations.Count > 1 ? 1 : 0) && e.ColIndex > this.pivotGridControl1.PivotRows.Count && e.Style.CellValue != null)
    {
        e.Style.CellType = "HyperlinkCell";
        e.Style.Tag = null;
    }
}
```

### Setting hyperlink information to cells

```csharp
// Subscribe to hyperlink click event
pivotGridControl1.TableModel_QueryCellInfo += TableModel_QueryCellInfo;

private void TableModel_QueryCellInfo(object sender, GridQueryCellInfoEventArgs e)
{
    if (e.ColIndex < this.pivotGridControl1.PivotRows.Count && e.Style.CellValue != null)
    {
        e.Style.CellType = "HyperlinkCell";
        if (e.Style.CellValue.ToString().Contains("Bike"))
            e.Style.Tag = "https://en.wikipedia.org/wiki/Types_of_motorcycles";
        else if (e.Style.CellValue.ToString().Contains("Car"))
            e.Style.Tag = "https://en.wikipedia.org/wiki/Car_classification";
    }
}
```

## Interactive Navigation

### Breadcrumb Navigation

```csharp
private Stack<PivotState> navigationHistory = new Stack<PivotState>();

// Save current state before drilling
private void SaveCurrentState()
{
    PivotState state = new PivotState
    {
        Rows = new List<PivotItem>(pivotGridControl1.PivotRows),
        Columns = new List<PivotItem>(pivotGridControl1.PivotColumns),
        Calculations = new List<PivotComputationInfo>(pivotGridControl1.PivotCalculations)
    };
    navigationHistory.Push(state);
}

// Navigate back
private void NavigateBack()
{
    if (navigationHistory.Count > 0)
    {
        PivotState previousState = navigationHistory.Pop();
        
        pivotGridControl1.PivotRows.Clear();
        pivotGridControl1.PivotColumns.Clear();
        pivotGridControl1.PivotCalculations.Clear();
        
        foreach (var row in previousState.Rows)
            pivotGridControl1.PivotRows.Add(row);
        foreach (var col in previousState.Columns)
            pivotGridControl1.PivotColumns.Add(col);
        foreach (var calc in previousState.Calculations)
            pivotGridControl1.PivotCalculations.Add(calc);
        
        pivotGridControl1.TableControl.Refresh(true);
    }
}

// Helper class for state management
private class PivotState
{
    public List<PivotItem> Rows { get; set; }
    public List<PivotItem> Columns { get; set; }
    public List<PivotComputationInfo> Calculations { get; set; }
}
```

### Contextual Drill-Down

```csharp
private void DrillDownOnCell(int rowIndex, int colIndex)
{
    // Save current state
    SaveCurrentState();
    
    // Get the row/column headers for this cell
    string rowHeader = pivotGridControl1.TableControl.GetCellValue(rowIndex, 0)?.ToString();
    string colHeader = pivotGridControl1.TableControl.GetCellValue(0, colIndex)?.ToString();
    
    // Add filter based on clicked cell
    if (!string.IsNullOrEmpty(rowHeader))
    {
        FilterExpression filter = new FilterExpression
        {
            DimensionName = "Product",
            Expression = $"Product = {rowHeader}"
        };
        pivotGridControl1.Filters.Add(filter);
    }
    
    // Refresh with new filter
    pivotGridControl1.TableControl.Refresh(true);
}
```

## Tooltips

Display additional information on hover:

### Basic Tooltips

```csharp
// Enable cell tooltips
this.pivotGridControl1.TableModel.QueryCellInfo += TableModel_QueryCellInfo;

// Customize tooltip content
private void TableModel_QueryCellInfo(object sender, GridQueryCellInfoEventArgs e)
{
    Syncfusion.PivotAnalysis.Base.PivotCellInfo info = pivotGridControl1.PivotEngine[e.RowIndex - 1, e.ColIndex - 1];
    if (info.CellType == Syncfusion.PivotAnalysis.Base.PivotCellType.ValueCell)
        e.Style.CellTipText = e.Style.Text;
}
```
## Best Practices

1. **Provide Visual Feedback** - Make clickable cells obvious with colors/underlines
2. **Show Record Counts** - Help users understand data density
3. **Optimize Large Datasets** - Limit drill-through results or use paging
4. **Save Navigation State** - Allow users to navigate back through drill paths
5. **Informative Tooltips** - Show summary statistics on hover
