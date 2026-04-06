# Pivot Calculations and Editing

## Table of Contents
- [Overview](#overview)
- [Pivot Calculations](#pivot-calculations)
- [Expression Fields](#expression-fields)
- [Editing Values](#editing-values)
- [Updating Values](#updating-values)
- [Validation](#validation)

## Overview

Pivot calculations allow advanced aggregations like running totals, percentages, and custom formulas. Editing features enable users to modify cell values and update source data.

## Pivot Calculations

Advanced calculation types:

```csharp
using Syncfusion.PivotAnalysis.Base;

// Running total
pivotGridControl1.PivotCalculations.Add(new PivotComputationInfo
{
    FieldName = "Amount",
    CalculationType = CalculationType.RunningTotal,
    Format = "C"
});

// Percentage of total
pivotGridControl1.PivotCalculations.Add(new PivotComputationInfo
{
    FieldName = "Amount",
    CalculationType = CalculationType.PercentageOfGrandTotal,
    Format = "P"
});

// Difference from previous
pivotGridControl1.PivotCalculations.Add(new PivotComputationInfo
{
    FieldName = "Amount",
    CalculationType = CalculationType.DifferenceFrom,
    Format = "C"
});
```

## Expression Fields

Create calculated fields using expressions:

```csharp
// Add expression field
PivotComputationInfo profit = new PivotComputationInfo
{
    FieldName = "Profit",
    Expression = "[Amount] - [Cost]",
    Format = "C",
    SummaryType = SummaryType.DoubleTotalSum
};

pivotGridControl1.PivotCalculations.Add(profit);
```

## Editing Values

Enable cell editing:

```csharp
// Allow editing
pivotGridControl1.AllowEditing = true;

// Handle value changes
pivotGridControl1.TableControl.CellValueChanged += (s, e) =>
{
    var newValue = e.NewValue;
    var oldValue = e.OldValue;
    Console.WriteLine($"Changed: {oldValue} → {newValue}");
};
```

## Updating Values

Programmatically update cell values:

```csharp
// Update specific cell
pivotGridControl1.TableControl.SetCellValue(rowIndex, colIndex, newValue);

// Refresh calculations
pivotGridControl1.TableControl.Refresh(true);
```

## Validation

Add validation rules:

```csharp
pivotGridControl1.TableControl.CellValidating += (s, e) =>
{
    if (double.TryParse(e.NewValue.ToString(), out double value))
    {
        if (value < 0)
        {
            e.Cancel = true;  // Reject negative values
            MessageBox.Show("Value must be positive");
        }
    }
};
```
