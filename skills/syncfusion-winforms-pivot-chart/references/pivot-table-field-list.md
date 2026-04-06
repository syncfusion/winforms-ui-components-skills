# Pivot Table Field List

This guide covers the built-in Pivot Table Field List, which provides an Excel-like interface for configuring pivot fields.

## Overview

The Pivot Table Field List is a schema designer similar to Microsoft Excel's PivotTable Field List. It allows users to interactively select and arrange fields without code.

## Enabling Field List

```csharp
// Show field list
pivotChart1.ShowPivotTableFieldList = true;
```

## Features

- Drag-and-drop field arrangement
- Field selection from data source
- Filter configuration per field
- Calculation setup
- Excel-like user experience

## Configuration

```csharp
private void ConfigureFieldList()
{
    // Enable field list
    pivotChart1.ShowPivotTableFieldList = true;
}
```

## User Workflow

1. **Open Field List:** Click field list button or menu
2. **Select Fields:** Check boxes to add to chart
3. **Drag to Areas:** Axis, Legend, or Calculations
4. **Apply Filters:** Click filter icons for specific fields
5. **Update Chart:** Changes apply immediately

## Best Practices

1. **Enable for Interactive Dashboards:** Great for user-driven analysis
2. **Disable for Fixed Reports:** Lock down field arrangement
3. **Provide Guidance:** Document available fields and their meanings
4. **Test Field Combinations:** Ensure all arrangements work correctly
