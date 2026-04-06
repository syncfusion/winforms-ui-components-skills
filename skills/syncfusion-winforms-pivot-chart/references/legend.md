# Legend Configuration

This guide covers legend configuration for identifying series in the Pivot Chart.

## Overview

The legend provides color-coded identification of chart series, helping users distinguish between different data categories.

## PivotLegend Property

```csharp
using Syncfusion.PivotAnalysis.Base;

// Add single legend field
pivotChart1.PivotLegend.Add(new PivotItem 
{ 
    FieldMappingName = "Quarter",
    TotalHeader = "All Quarters"
});
```

## Multiple Legend Fields

```csharp
// Nested legend: Year → Quarter
pivotChart1.PivotLegend.Clear();

pivotChart1.PivotLegend.Add(new PivotItem { FieldMappingName = "Year", TotalHeader = "All Years" });
pivotChart1.PivotLegend.Add(new PivotItem { FieldMappingName = "Quarter", TotalHeader = "All Quarters" });
```

## Legend Customization

```csharp
// Configure legend appearance
pivotChart1.ChartControl.Legend.Visible = true;
pivotChart1.ChartControl.Legend.Position = ChartDock.Right;
pivotChart1.ChartControl.Legend.ItemsSize = new Size(150, 20);
```

## Common Patterns

### Time-Based Legend
```csharp
// Quarters across years
pivotChart1.PivotLegend.Add(new PivotItem { FieldMappingName = "Quarter" });
```

### Category Legend
```csharp
// Product categories
pivotChart1.PivotLegend.Add(new PivotItem { FieldMappingName = "Category" });
```

## Best Practices

1. Use legend for secondary categorization
2. Limit to 5-7 series for readability
3. Use consistent color schemes
4. Position legend for optimal space usage
