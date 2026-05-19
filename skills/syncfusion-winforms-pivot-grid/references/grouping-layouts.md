# Grouping Bar and Layouts

## Table of Contents
- [Overview](#overview)
- [Grouping Bar](#grouping-bar)
- [Layout Types](#layout-types)

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

**Available Layouts:**
- `Normal` - Standard pivot layout with subtotals
- `Flat Layout` - Flat table layout
