# Data Exploration

## Table of Contents
- [Overview](#overview)
- [Hyperlink Cells](#hyperlink-cells)
- [Drill-Through Functionality](#drill-through-functionality)
- [Data Visualization](#data-visualization)
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
pivotGridControl1.TableControl.EnableHyperlinkCells = true;
```

### Handling Hyperlink Clicks

```csharp
// Subscribe to hyperlink click event
pivotGridControl1.HyperlinkCellClick += PivotGridControl1_HyperlinkCellClick;

private void PivotGridControl1_HyperlinkCellClick(object sender, 
    HyperlinkCellClickEventArgs e)
{
    // Get clicked cell information
    string cellValue = e.Text;
    int rowIndex = e.RowIndex;
    int colIndex = e.ColIndex;
    
    Console.WriteLine($"Clicked: {cellValue} at ({rowIndex}, {colIndex})");
    
    // Show drill-through data
    ShowDrillThroughData(rowIndex, colIndex);
    
    // Or cancel default behavior
    // e.Cancel = true;
}
```

## Drill-Through Functionality

Drill-through displays the underlying detail records that contribute to a summary cell value.

### Basic Drill-Through

```csharp
private void ShowDrillThroughData(int rowIndex, int colIndex)
{
    // Get the underlying data for the clicked cell
    var drillData = pivotGridControl1.InternalEngine.GetRawItemsFor(rowIndex, colIndex);
    
    if (drillData != null && drillData.Count > 0)
    {
        // Display in a data grid
        DataGridView detailGrid = new DataGridView
        {
            DataSource = drillData,
            Dock = DockStyle.Fill,
            ReadOnly = true,
            AllowUserToAddRows = false
        };
        
        // Show in new form
        Form detailForm = new Form
        {
            Text = $"Drill-Through Data ({drillData.Count} records)",
            Size = new Size(800, 600),
            StartPosition = FormStartPosition.CenterParent
        };
        detailForm.Controls.Add(detailGrid);
        detailForm.ShowDialog();
    }
    else
    {
        MessageBox.Show("No detail data available for this cell.");
    }
}
```

### Custom Drill-Through Dialog

```csharp
private void ShowCustomDrillThrough(object sender, HyperlinkCellClickEventArgs e)
{
    // Get raw data
    var rawData = pivotGridControl1.InternalEngine.GetRawItemsFor(e.RowIndex, e.ColIndex);
    
    if (rawData != null)
    {
        // Create custom view
        StringBuilder details = new StringBuilder();
        details.AppendLine($"Cell Value: {e.Text}");
        details.AppendLine($"Record Count: {rawData.Count}");
        details.AppendLine("\nDetail Records:");
        
        foreach (var item in rawData.Take(10))  // Show first 10
        {
            // Assuming ProductSales data model
            if (item is ProductSales sale)
            {
                details.AppendLine($"- {sale.Product}: {sale.Amount:C} ({sale.Quantity} units)");
            }
        }
        
        MessageBox.Show(details.ToString(), "Drill-Through Details", 
                       MessageBoxButtons.OK, MessageBoxIcon.Information);
    }
}
```

## Data Visualization

### Highlighting Patterns

```csharp
// Highlight cells based on drill-through data count
pivotGridControl1.TableControl.QueryCellStyle += (s, e) =>
{
    if (e.RowIndex > 0 && e.ColIndex > 0)
    {
        var rawData = pivotGridControl1.InternalEngine.GetRawItemsFor(
            e.RowIndex, e.ColIndex);
        
        if (rawData != null)
        {
            int count = rawData.Count;
            
            // Color intensity based on record count
            if (count > 100)
                e.Style.BackColor = Color.DarkGreen;
            else if (count > 50)
                e.Style.BackColor = Color.LightGreen;
            else if (count > 10)
                e.Style.BackColor = Color.LightYellow;
        }
    }
};
```

### Visual Indicators

```csharp
// Add visual indicators for clickable cells
pivotGridControl1.TableControl.QueryCellStyle += (s, e) =>
{
    if (pivotGridControl1.TableControl.EnableHyperlinkCells)
    {
        if (e.RowIndex > 0 && e.ColIndex > 0)
        {
            // Make value cells look like hyperlinks
            e.Style.ForeColor = Color.Blue;
            e.Style.Font.Underline = true;
            e.Style.CellTipText = "Click to view details";
        }
    }
};
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
pivotGridControl1.TableControl.ActivateCurrentCellBehavior = 
    GridCellActivateAction.SetCurrent;

// Customize tooltip content
pivotGridControl1.TableControl.QueryCellStyle += (s, e) =>
{
    if (e.RowIndex > 0 && e.ColIndex > 0)
    {
        var rawData = pivotGridControl1.InternalEngine.GetRawItemsFor(
            e.RowIndex, e.ColIndex);
        
        if (rawData != null)
        {
            string tooltip = $"Value: {e.Style.CellValue}\n" +
                           $"Record Count: {rawData.Count}\n" +
                           $"Click to view details";
            e.Style.CellTipText = tooltip;
        }
    }
};
```

### Rich Tooltips

```csharp
// Show statistical information in tooltip
pivotGridControl1.TableControl.QueryCellStyle += (s, e) =>
{
    if (e.RowIndex > 0 && e.ColIndex > 0)
    {
        var rawData = pivotGridControl1.InternalEngine.GetRawItemsFor(
            e.RowIndex, e.ColIndex);
        
        if (rawData != null && rawData.Count > 0)
        {
            // Calculate statistics
            var amounts = rawData.Cast<ProductSales>()
                                .Select(x => x.Amount)
                                .ToList();
            
            double sum = amounts.Sum();
            double avg = amounts.Average();
            double min = amounts.Min();
            double max = amounts.Max();
            
            string tooltip = $"Summary Statistics:\n" +
                           $"Total: {sum:C}\n" +
                           $"Average: {avg:C}\n" +
                           $"Min: {min:C}\n" +
                           $"Max: {max:C}\n" +
                           $"Count: {rawData.Count}";
            
            e.Style.CellTipText = tooltip;
        }
    }
};
```

## Complete Example

```csharp
public class DataExplorationForm : Form
{
    private PivotGridControl pivotGridControl1;
    private Stack<PivotState> navigationHistory = new Stack<PivotState>();
    
    public DataExplorationForm()
    {
        InitializeComponent();
        SetupDataExploration();
    }
    
    private void SetupDataExploration()
    {
        // Enable hyperlinks
        pivotGridControl1.TableControl.EnableHyperlinkCells = true;
        
        // Handle clicks
        pivotGridControl1.HyperlinkCellClick += OnHyperlinkCellClick;
        
        // Add tooltips
        pivotGridControl1.TableControl.QueryCellStyle += AddTooltips;
        
        // Style hyperlinks
        pivotGridControl1.TableControl.QueryCellStyle += StyleHyperlinks;
    }
    
    private void OnHyperlinkCellClick(object sender, HyperlinkCellClickEventArgs e)
    {
        // Get drill-through data
        var rawData = pivotGridControl1.InternalEngine.GetRawItemsFor(
            e.RowIndex, e.ColIndex);
        
        if (rawData != null && rawData.Count > 0)
        {
            // Show detail form
            ShowDrillThroughDialog(e.Text, rawData);
        }
        
        // Cancel default behavior
        e.Cancel = true;
    }
    
    private void AddTooltips(object sender, GridQueryCellStyleEventArgs e)
    {
        if (e.RowIndex > 0 && e.ColIndex > 0)
        {
            var rawData = pivotGridControl1.InternalEngine.GetRawItemsFor(
                e.RowIndex, e.ColIndex);
            
            if (rawData != null)
            {
                e.Style.CellTipText = $"Records: {rawData.Count}\nClick for details";
            }
        }
    }
    
    private void StyleHyperlinks(object sender, GridQueryCellStyleEventArgs e)
    {
        if (e.RowIndex > 0 && e.ColIndex > 0)
        {
            e.Style.ForeColor = Color.Blue;
            e.Style.Font.Underline = true;
        }
    }
    
    private void ShowDrillThroughDialog(string cellValue, IList rawData)
    {
        Form detailForm = new Form
        {
            Text = $"Details for {cellValue} ({rawData.Count} records)",
            Size = new Size(900, 600),
            StartPosition = FormStartPosition.CenterParent
        };
        
        DataGridView grid = new DataGridView
        {
            DataSource = rawData,
            Dock = DockStyle.Fill,
            ReadOnly = true,
            AllowUserToAddRows = false,
            AutoSizeColumnsMode = DataGridViewAutoSizeColumnsMode.AllCells
        };
        
        detailForm.Controls.Add(grid);
        detailForm.ShowDialog();
    }
}
```

## Best Practices

1. **Provide Visual Feedback** - Make clickable cells obvious with colors/underlines
2. **Show Record Counts** - Help users understand data density
3. **Optimize Large Datasets** - Limit drill-through results or use paging
4. **Save Navigation State** - Allow users to navigate back through drill paths
5. **Informative Tooltips** - Show summary statistics on hover
