# Grouping Bar and Layouts

## Table of Contents
- [Overview](#overview)
- [Grouping Bar](#grouping-bar)
- [Layout Types](#layout-types)
- [Drill-Down Navigation](#drill-down-navigation)
- [Expand/Collapse Behaviors](#expandcollapse-behaviors)

## Overview

The Grouping Bar and Layout features provide interactive ways to organize and visualize pivot data through drag-and-drop field management and various display layouts.

## Grouping Bar

The Grouping Bar is a visual interface similar to Excel's pivot table interface, allowing users to dynamically rearrange pivot fields.

### Enabling Grouping Bar

```csharp
// Enable the grouping bar
pivotGridControl1.ShowGroupBar = true;
```

### Features

- Drag-and-drop field organization
- Visual representation of pivot structure
- Real-time grid updates
- Filter indicators
- Sort indicators

## Layout Types

Configure how data is displayed using different layout modes:

```csharp
// Set layout type
pivotGridControl1.PivotEngine.PivotLayoutFormat = PivotLayoutFormat.Normal;
```

**Available Layouts:**
- `Normal` - Standard pivot layout with subtotals
- `TopSummary` - Show subtotals at top of groups
- `NoSummary` - Hide subtotals, show only grand totals
- `Tabular` - Flat table layout

## Drill-Down Navigation

Users can expand/collapse hierarchical data:

```csharp
// Enable drill-down
pivotGridControl1.EnableDrillDown = true;

// Handle drill events
pivotGridControl1.HyperlinkCellClick += (s, e) =>
{
    Console.WriteLine($"Drill at Row: {e.RowIndex}, Col: {e.ColIndex}");
};
```

## Expand/Collapse Behaviors

Control default expansion state:

```csharp
// Collapse all groups initially
pivotGridControl1.ExpandAll = false;

// Or expand all
pivotGridControl1.ExpandAll = true;
```
