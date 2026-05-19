# Cell Selection and Conditional Formatting

## Table of Contents
- [Overview](#overview)
- [Cell Selection](#cell-selection)
- [Conditional Formatting](#conditional-formatting)

## Overview

Control how users select cells and apply dynamic formatting based on cell values to highlight important data patterns.

## Cell Selection

Configure selection modes:

```csharp
using Syncfusion.Windows.Forms.PivotAnalysis;

// Enable selection
pivotGridControl1.TableControl.AllowSelection = true;
```

### Selection Events

```csharp
// Handle selection changes
pivotGridControl1.TableControl.SelectionChanged += (s, e) =>
{
    var selectedRanges = pivotGridControl1.TableControl.Selections;
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

NewRuleConditionalFormat newRule1 = new NewRuleConditionalFormat();
newRule1.RuleType = RuleType.FormatOnlyCellsThatContain;
newRule1.SummaryElement = "Quantity";

// Create style
PivotGridNewRuleConditionalFormat newRuleFormat1 = new PivotGridNewRuleConditionalFormat();
newRuleFormat1.NewRuleCollections.Add(newRule1);
newRuleFormat1.PivotCellStyle.BackColor = Color.Red;
newRuleFormat1.PivotCellStyle.TextColor = Color.White;
pivotGridControl1.TableControl.NewRuleConditionalFormat.Add(newRuleFormat1);

// Create condition
ConditionalFormat condition1 = new ConditionalFormat();
condition1.PredicateType = PredicateType.And;
condition1.ConditionType = PivotGridDataConditionType.Between;
condition1.StartValue = 30;
condition1.EndValue = 60;
newRule1.Conditions.Add(condition1);
```

### Multiple Conditions

```csharp
// High values - Green
PivotGridNewRuleConditionalFormat newRuleFormat1 = new PivotGridNewRuleConditionalFormat();
newRuleFormat1.PivotCellStyle.BackColor = Color.LightGreen;

ConditionalFormat highCondition = new ConditionalFormat();
highCondition.ConditionType = PivotGridDataConditionType.GreaterThan;
newRuleFormat1.PivotCellStyle.BackColor = Color.Green;

// Low values - Red
ConditionalFormat lowCondition = new ConditionalFormat();
lowCondition.ConditionType = PivotGridDataConditionType.LessThan;
newRuleFormat1.PivotCellStyle.BackColor = Color.Red;

// Apply both
pivotGridControl1.TableControl.NewRuleConditionalFormat.Add(newRuleFormat1);
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

### Between Range Example

```csharp
ConditionalFormat condition1 = new ConditionalFormat();
condition1.PredicateType = PredicateType.And;
condition1.ConditionType = PivotGridDataConditionType.Between;
condition1.StartValue = 30;
condition1.EndValue = 60;
newRule1.Conditions.Add(condition1);
```

## Complete Example

```csharp
public void SetupFormattingAndSelection()
{
    // Enable selection
    pivotGridControl1.TableControl.AllowSelection = true;
    
    // Define styles
    NewRuleConditionalFormat newRule1 = new NewRuleConditionalFormat();
    newRule1.RuleType = RuleType.FormatOnlyCellsThatContain;
    newRule1.SummaryElement = "Quantity";

    // Define styles
    PivotGridNewRuleConditionalFormat highStyle = new PivotGridNewRuleConditionalFormat();
    highStyle.NewRuleCollections.Add(newRule1);
    highStyle.PivotCellStyle.BackColor = Color.LightGreen;
    highStyle.PivotCellStyle.TextColor = Color.DarkGreen;
    pivotGridControl1.TableControl.NewRuleConditionalFormat.Add(highStyle);

    PivotGridNewRuleConditionalFormat lowStyle = new PivotGridNewRuleConditionalFormat();
    lowStyle.NewRuleCollections.Add(newRule1);
    lowStyle.PivotCellStyle.BackColor = Color.LightCoral;
    lowStyle.PivotCellStyle.TextColor = Color.DarkRed;
    pivotGridControl1.TableControl.NewRuleConditionalFormat.Add(lowStyle);

    // High values condition
    ConditionalFormat highFormat = new ConditionalFormat();
    highFormat.ConditionType = PivotGridDataConditionType.GreaterThan;
    highStyle.PivotCellStyle.BackColor = Color.Green;
    pivotGridControl1.TableControl.NewRuleConditionalFormat.Add(highStyle);

    // Low values condition
    ConditionalFormat lowFormat = new ConditionalFormat();
    lowFormat.ConditionType = PivotGridDataConditionType.LessThan;
    lowStyle.PivotCellStyle.BackColor = Color.Red;

    // Apply formatting
    pivotGridControl1.TableControl.NewRuleConditionalFormat.Add(highStyle);
    pivotGridControl1.TableControl.NewRuleConditionalFormat.Add(lowStyle);
        
    // Handle selection events
    pivotGridControl1.TableControl.SelectionChanged += (s, e) =>
    {
        Console.WriteLine("Selection changed");
    };
}
```
