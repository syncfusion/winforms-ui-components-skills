# Pivot Calculations and Editing

## Table of Contents
- [Overview](#overview)
- [Pivot Calculations](#pivot-calculations)
- [Expression Fields](#expression-fields)
- [Editing Values](#editing-values)
- [Updating Values](#updating-values)

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
    CalculationType = CalculationType.RunningTotalIn,
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
    Formula = "[Amount] - [Cost]",
    Format = "C",
    SummaryType = SummaryType.DoubleTotalSum
};

pivotGridControl1.PivotCalculations.Add(profit);
```

## Editing Values

Enable cell editing:

```csharp
// Allow editing
pivotGridControl1.EnableValueEditing = true;
```

## Updating Values

Programmatically update cell values:

```csharp
// Enabling updating
pivotGridControl1.EnableUpdating = true;

//Throttling update speed
pivotGridControl1.UpdateManager.ThrottleUpdateRate = 300;

// Refresh calculations
pivotGridControl1.TableControl.Refresh(true);
```
