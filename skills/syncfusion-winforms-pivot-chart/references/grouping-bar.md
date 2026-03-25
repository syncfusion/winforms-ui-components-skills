# Grouping Bar in Pivot Chart

This guide covers the Grouping Bar feature, which provides interactive drag-and-drop functionality for rearranging pivot fields.

## Overview

The Grouping Bar provides an Excel-like interface for interactively manipulating pivot chart fields. Users can drag and drop fields between axis, legend, and calculation areas to dynamically restructure the chart visualization.

## Enabling Grouping Bar

```csharp
// Enable grouping bar
pivotChart1.ShowGroupingBar = true;
```

## Features

- Drag fields between Axis, Legend, and Calculations
- Reorder fields within each area
- Remove fields by dragging out
- Filter values for specific fields
- Sort field values

## Configuration

```csharp
private void ConfigureGroupingBar()
{
    pivotChart1.ShowGroupingBar = true;
    pivotChart1.GroupingBarSettings.AllowDragAndDrop = true;
    pivotChart1.GroupingBarSettings.ShowFieldButtons = true;
}
```

## User Interactions

Users can:
1. **Drag fields** to reorder hierarchy
2. **Move between areas** (Axis ↔ Legend)
3. **Apply filters** via field buttons
4. **Remove fields** by dragging off the bar

## Best Practices

1. Enable for power users who need flexibility
2. Provide initial sensible field arrangement
3. Consider disabling for fixed report layouts
4. Test common user rearrangement scenarios
