# Sorting in Pivot Chart

This guide covers sorting capabilities for pivot data in the chart.

## Overview

Sorting allows ordering of pivot data in ascending or descending order based on field values or calculation results.

## Basic Sorting

```csharp
// Sort by axis field
pivotChart1.PivotAxis[0].SortDirection = SortDirection.Ascending;
```

## Sort Options

```csharp
public enum SortDirection
{
    None,        // No sorting
    Ascending,   // A to Z, 0 to 9
    Descending   // Z to A, 9 to 0
}
```

## Sorting by Calculations

```csharp
// Sort categories by revenue (highest first)
pivotChart1.PivotAxis[0].SortDirection = SortDirection.Descending;
pivotChart1.PivotAxis[0].SortByCalculation = "Revenue";
```

## Multiple Field Sorting

```csharp
// Primary sort: Product ascending
pivotChart1.PivotAxis[0].SortDirection = SortDirection.Ascending;

// Secondary sort: Region descending
pivotChart1.PivotAxis[1].SortDirection = SortDirection.Descending;
```

## Custom Sort Logic

For advanced scenarios, implement custom comparer:

```csharp
// Custom sort order for months, quarters, etc.
private class CustomMonthSorter : IComparer<string>
{
    private string[] monthOrder = { "Jan", "Feb", "Mar", "Apr", "May", "Jun", 
                                    "Jul", "Aug", "Sep", "Oct", "Nov", "Dec" };
    
    public int Compare(string x, string y)
    {
        int indexX = Array.IndexOf(monthOrder, x);
        int indexY = Array.IndexOf(monthOrder, y);
        return indexX.CompareTo(indexY);
    }
}
```

## Best Practices

1. Sort by most meaningful field first
2. Use calculation-based sorting for rankings
3. Consider user expectations (alphabetical vs value-based)
4. Test sort behavior with real data
