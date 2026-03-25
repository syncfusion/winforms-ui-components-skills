# Pivot Axis Configuration

This guide covers detailed configuration of the PivotAxis property for defining hierarchical chart structures.

## Table of Contents
- [Overview](#overview)
- [PivotItem Properties](#pivotitem-properties)
- [Adding Axis Fields](#adding-axis-fields)
- [Hierarchical Configuration](#hierarchical-configuration)
- [Best Practices](#best-practices)

## Overview

The PivotAxis property defines the hierarchical structure for the chart's X-axis. Fields are displayed in the order they're added, with each level providing drill-down capability.

## PivotItem Properties

```csharp
public class PivotItem
{
    public string FieldMappingName { get; set; }  // Data property name
    public string TotalHeader { get; set; }        // Aggregate display text
    public string FieldHeader { get; set; }        // Field display name
}
```

| Property | Required | Description | Example |
|----------|----------|-------------|---------|
| FieldMappingName | Yes | Property name in data source | `"Product"` |
| TotalHeader | Yes | Text for aggregated rows | `"All Products"` |
| FieldHeader | No | Display name for field | `"Product Name"` |

## Adding Axis Fields

### Single Field

```csharp
using Syncfusion.PivotAnalysis.Base;

pivotChart1.PivotAxis.Add(new PivotItem 
{ 
    FieldMappingName = "Product",
    TotalHeader = "All Products"
});
```

### Multiple Fields (Hierarchy)

```csharp
// Define 3-level hierarchy
pivotChart1.PivotAxis.Clear();

// Level 1
pivotChart1.PivotAxis.Add(new PivotItem 
{ 
    FieldMappingName = "Category",
    TotalHeader = "All Categories",
    FieldHeader = "Product Category"
});

// Level 2
pivotChart1.PivotAxis.Add(new PivotItem 
{ 
    FieldMappingName = "Product",
    TotalHeader = "All Products",
    FieldHeader = "Product Name"
});

// Level 3
pivotChart1.PivotAxis.Add(new PivotItem 
{ 
    FieldMappingName = "Region",
    TotalHeader = "All Regions"
});
```

## Hierarchical Configuration

### Geographic Hierarchy

```csharp
// Country → State → City
pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "Country", TotalHeader = "All Countries" });
pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "State", TotalHeader = "All States" });
pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "City", TotalHeader = "All Cities" });
```

### Product Hierarchy

```csharp
// Category → Subcategory → Product
pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "Category", TotalHeader = "All Categories" });
pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "Subcategory", TotalHeader = "All Subcategories" });
pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "Product", TotalHeader = "All Products" });
```

### Time Hierarchy

```csharp
// Year → Quarter → Month
pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "Year", TotalHeader = "All Years" });
pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "Quarter", TotalHeader = "All Quarters" });
pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "Month", TotalHeader = "All Months" });
```

## Modifying Axis Configuration

### Adding Fields Dynamically

```csharp
private void AddAxisField(string fieldName, string header)
{
    pivotChart1.BeginUpdate();
    
    pivotChart1.PivotAxis.Add(new PivotItem 
    { 
        FieldMappingName = fieldName,
        TotalHeader = header
    });
    
    pivotChart1.EndUpdate();
}
```

### Removing Fields

```csharp
// Remove all fields
pivotChart1.PivotAxis.Clear();

// Remove specific field
var itemToRemove = pivotChart1.PivotAxis.FirstOrDefault(i => i.FieldMappingName == "Region");
if (itemToRemove != null)
{
    pivotChart1.PivotAxis.Remove(itemToRemove);
}
```

### Reordering Fields

```csharp
// Swap field positions
pivotChart1.BeginUpdate();

var items = pivotChart1.PivotAxis.ToList();
pivotChart1.PivotAxis.Clear();

// Add in new order
pivotChart1.PivotAxis.Add(items[1]); // Was second, now first
pivotChart1.PivotAxis.Add(items[0]); // Was first, now second

pivotChart1.EndUpdate();
```

## Best Practices

1. **Order Matters:** First item is top level
2. **Limit Depth:** 3-4 levels optimal
3. **Use Descriptive Headers:** "All Products" vs "Total"
4. **Match Property Names:** FieldMappingName is case-sensitive
5. **Test Hierarchy:** Verify logical drill-down flow
6. **BeginUpdate/EndUpdate:** Use for multiple changes

## Common Patterns

### Pattern 1: Build from User Selection

```csharp
private void BuildAxisFromSelection(List<string> selectedFields)
{
    pivotChart1.BeginUpdate();
    pivotChart1.PivotAxis.Clear();
    
    foreach (string field in selectedFields)
    {
        pivotChart1.PivotAxis.Add(new PivotItem 
        { 
            FieldMappingName = field,
            TotalHeader = $"All {field}s"
        });
    }
    
    pivotChart1.EndUpdate();
}
```

### Pattern 2: Conditional Hierarchy

```csharp
private void SetupAxisBasedOnData(string dataType)
{
    pivotChart1.PivotAxis.Clear();
    
    if (dataType == "Sales")
    {
        pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "Product", TotalHeader = "All Products" });
        pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "Region", TotalHeader = "All Regions" });
    }
    else if (dataType == "Time")
    {
        pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "Year", TotalHeader = "All Years" });
        pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "Quarter", TotalHeader = "All Quarters" });
    }
}
```

## Troubleshooting

### FieldMappingName Not Found
Ensure property name matches exactly (case-sensitive)

### No Drill-Down Available
Add 2+ items to PivotAxis and enable AllowDrillDown

### Incorrect Hierarchy Order
Check PivotItem add sequence - order defines hierarchy
