# Cell Selection and Conditional Formatting

## Table of Contents
- [Overview](#overview)
- [Cell Selection](#cell-selection)
- [Conditional Formatting](#conditional-formatting)
- [Custom Cell Styles](#custom-cell-styles)

## Overview

Control how users select cells and apply dynamic formatting based on cell values to highlight important data patterns.

## Cell Selection

Configure selection modes:

```csharp
using Syncfusion.Windows.Forms.PivotAnalysis;

// Enable selection
pivotGridControl1.AllowSelection = true;

// Set selection mode
pivotGridControl1.TableControl.SelectionMode = GridSelectionMode.Cell;  // Single cell
// Or
pivotGridControl1.TableControl.SelectionMode = GridSelectionMode.Range; // Range selection
```

### Selection Events

```csharp
// Handle selection changes
pivotGridControl1.TableControl.SelectionChanged += (s, e) =>
{
    var selectedRanges = pivotGridControl1.TableControl.SelectedRanges;
    Console.WriteLine($"Selected {selectedRanges.Count} ranges");
};
```

### Programmatic Selection

```csharp
// Select specific cell
pivotGridControl1.TableControl.Selections.Add(GridRangeInfo.Cell(5, 3));

// Select range
pivotGridControl1.TableControl.Selections.Add(GridRangeInfo.Cells(2, 2, 10, 5));

// Clear selection
pivotGridControl1.TableControl.Selections.Clear();
```

## Conditional Formatting

Apply formatting rules based on cell values:

### Basic Conditional Format

```csharp
using Syncfusion.Windows.Forms.PivotAnalysis;

// Create style
PivotCellStyle redStyle = new PivotCellStyle();
redStyle.BackColor = Color.LightCoral;
redStyle.ForeColor = Color.White;
redStyle.Font = new Font("Arial", 10, FontStyle.Bold);

// Create condition
ConditionalFormat condition = new ConditionalFormat();
condition.Conditions.Add(new PivotCondition
{
    ConditionType = PivotConditionType.LessThan,
    PredicateValue = 10000
});
condition.ApplyStyleInfo = redStyle;

// Apply to grid
pivotGridControl1.TableControl.ConditionalFormats.Add(condition);
```

### Multiple Conditions

```csharp
// High values - Green
PivotCellStyle greenStyle = new PivotCellStyle();
greenStyle.BackColor = Color.LightGreen;

ConditionalFormat highCondition = new ConditionalFormat();
highCondition.Conditions.Add(new PivotCondition
{
    ConditionType = PivotConditionType.GreaterThan,
    PredicateValue = 50000
});
highCondition.ApplyStyleInfo = greenStyle;

// Low values - Red
ConditionalFormat lowCondition = new ConditionalFormat();
lowCondition.Conditions.Add(new PivotCondition
{
    ConditionType = PivotConditionType.LessThan,
    PredicateValue = 10000
});
lowCondition.ApplyStyleInfo = redStyle;

// Apply both
pivotGridControl1.TableControl.ConditionalFormats.Add(highCondition);
pivotGridControl1.TableControl.ConditionalFormats.Add(lowCondition);
```

### Condition Types

Available condition types:
- `GreaterThan`
- `LessThan`
- `Equals`
- `NotEquals`
- `GreaterThanOrEqual`
- `LessThanOrEqual`
- `Between`
- `NotBetween`

### Between Range Example

```csharp
ConditionalFormat rangeCondition = new ConditionalFormat();
rangeCondition.Conditions.Add(new PivotCondition
{
    ConditionType = PivotConditionType.Between,
    PredicateValue = 10000,
    PredicateValue2 = 50000
});
rangeCondition.ApplyStyleInfo = yellowStyle;

pivotGridControl1.TableControl.ConditionalFormats.Add(rangeCondition);
```

## Custom Cell Styles

Apply custom styling to specific cells:

```csharp
// Style specific cell
pivotGridControl1.TableControl.QueryCellStyle += (s, e) =>
{
    if (e.RowIndex > 0 && e.ColIndex > 0)
    {
        var cellValue = e.Style.CellValue;
        if (cellValue != null && double.TryParse(cellValue.ToString(), out double value))
        {
            if (value < 0)
            {
                e.Style.BackColor = Color.Pink;
                e.Style.ForeColor = Color.DarkRed;
            }
        }
    }
};
```

### Styling Headers

```csharp
pivotGridControl1.TableControl.QueryCellStyle += (s, e) =>
{
    // Style column headers
    if (e.RowIndex == 0 && e.ColIndex > 0)
    {
        e.Style.BackColor = Color.Navy;
        e.Style.ForeColor = Color.White;
        e.Style.Font.Bold = true;
    }
    
    // Style row headers
    if (e.ColIndex == 0 && e.RowIndex > 0)
    {
        e.Style.BackColor = Color.DarkBlue;
        e.Style.ForeColor = Color.White;
    }
};
```

## Complete Example

```csharp
public void SetupFormattingAndSelection()
{
    // Enable selection
    pivotGridControl1.AllowSelection = true;
    pivotGridControl1.TableControl.SelectionMode = GridSelectionMode.Range;
    
    // Define styles
    PivotCellStyle highStyle = new PivotCellStyle
    {
        BackColor = Color.LightGreen,
        ForeColor = Color.DarkGreen,
        Font = new Font("Arial", 9, FontStyle.Bold)
    };
    
    PivotCellStyle lowStyle = new PivotCellStyle
    {
        BackColor = Color.LightCoral,
        ForeColor = Color.DarkRed,
        Font = new Font("Arial", 9, FontStyle.Bold)
    };
    
    // High values condition
    ConditionalFormat highFormat = new ConditionalFormat();
    highFormat.Conditions.Add(new PivotCondition
    {
        ConditionType = PivotConditionType.GreaterThan,
        PredicateValue = 50000
    });
    highFormat.ApplyStyleInfo = highStyle;
    
    // Low values condition
    ConditionalFormat lowFormat = new ConditionalFormat();
    lowFormat.Conditions.Add(new PivotCondition
    {
        ConditionType = PivotConditionType.LessThan,
        PredicateValue = 10000
    });
    lowFormat.ApplyStyleInfo = lowStyle;
    
    // Apply formatting
    pivotGridControl1.TableControl.ConditionalFormats.Add(highFormat);
    pivotGridControl1.TableControl.ConditionalFormats.Add(lowFormat);
    
    // Handle selection events
    pivotGridControl1.TableControl.SelectionChanged += (s, e) =>
    {
        Console.WriteLine("Selection changed");
    };
}
```
