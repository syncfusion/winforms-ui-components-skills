# Grouping Bar in Pivot Chart

This guide covers the Grouping Bar feature, which provides interactive drag-and-drop functionality for rearranging pivot fields.

## Overview

The Grouping Bar provides an Excel-like interface for interactively manipulating pivot chart fields. Users can drag and drop fields between axis, legend, and calculation areas to dynamically restructure the chart visualization.

## Enabling Grouping Bar

```csharp
// Enable grouping bar
pivotChart1.AxisFieldSection.Visible = true;
pivotChart1.LegendFieldSection.Visible = true;
pivotChart1.ValueFieldSection.Visible = true;
pivotChart1.FilterFieldSection.Visible = true;
```

## Customizing grouping bar

```csharp
this.pivotChart1.AxisFieldSection.ItemBackColor = Color.Yellow;
this.pivotChart1.AxisFieldSection.ItemForeColor = Color.Black;
this.pivotChart1.AxisFieldSection.BackInterior = Color.SkyBlue;
```

## User Interactions

Users can:
- Drag fields to reorder hierarchy inside the Axis Field Section (fields from PivotAxis).
- Move fields between areas — Axis, Legend, Value and Filter sections.
- Customize fields visually using section-level button styles (background/foreground).
- Remove fields by dragging them out of the grouping bar.

## Best Practices

1. Enable grouping bar when users need flexible, Excel‑style pivot rearrangement.
2. Provide default axis arrangement (e.g., Year → Quarter → Month).
3. Disable grouping bar for fixed analytical layouts where changes must be restricted.
4. Test common drag‑drop and reorder scenarios to ensure chart refresh and recalculation behave consistently.