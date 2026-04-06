# Sorting in Pivot Chart

This guide covers sorting capabilities for pivot data in the chart.

## Overview

Sorting allows ordering of pivot data in ascending or descending order based on field values or calculation results.

## Sort by pivot axis
The PivotAxis can be sorted by adding PivotSortDescriptor to the SortedAxis collection and ListSortDirection to specify the sorting order.

```csharp
this.pivotChart1.SortedAxis.Add(new PivotSortDescriptor("Country", ListSortDirection.Descending));
```

## Sort by pivot legends
The PivotLegends can be sorted by adding PivotSortDescriptor to the SortedLegends collection and ListSortDirection to specify the sorting order.


```csharp
this.pivotChart1.SortedLegends.Add(new PivotSortDescriptor("Date", ListSortDirection.Descending));
```

## Sort Options

```csharp
public enum ListSortDirection
{
    Ascending,   // A to Z, 0 to 9
    Descending   // Z to A, 9 to 0
}
```

## Best Practices

1. Sort by most meaningful field first
2. Use calculation-based sorting for rankings
3. Consider user expectations (alphabetical vs value-based)
4. Test sort behavior with real data