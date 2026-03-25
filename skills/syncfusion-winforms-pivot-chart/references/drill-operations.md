# Drill Operations in Pivot Chart

This guide covers drill-down and drill-up functionality for navigating through hierarchical data levels in the Pivot Chart.

## Table of Contents
- [Overview](#overview)
- [Enabling Drill Operations](#enabling-drill-operations)
- [How Drill Operations Work](#how-drill-operations-work)
- [User Interactions](#user-interactions)
- [Programmatic Drill Control](#programmatic-drill-control)
- [Best Practices](#best-practices)

## Overview

Drill operations allow users to navigate through hierarchical data levels interactively. Users can:
- **Drill Down:** Expand to see more detailed data at the next hierarchy level
- **Drill Up:** Collapse back to the previous aggregated level

This feature is essential for exploring multidimensional data like Product → Region → State or Year → Quarter → Month.

## Enabling Drill Operations

### Basic Configuration

```csharp
// Enable drill-down functionality
pivotChart1.AllowDrillDown = true;
```

```vb
' VB.NET - Enable drill-down
pivotChart1.AllowDrillDown = True
```

### Complete Setup Example

```csharp
using Syncfusion.Windows.Forms.PivotChart;
using Syncfusion.PivotAnalysis.Base;

private void ConfigureDrillDown()
{
    // Enable drill operations
    pivotChart1.AllowDrillDown = true;
    
    // Define hierarchical structure (3 levels)
    pivotChart1.PivotAxis.Clear();
    
    // Level 1: Product (top level)
    pivotChart1.PivotAxis.Add(new PivotItem 
    { 
        FieldMappingName = "Product", 
        TotalHeader = "All Products" 
    });
    
    // Level 2: Country (drill down from Product)
    pivotChart1.PivotAxis.Add(new PivotItem 
    { 
        FieldMappingName = "Country", 
        TotalHeader = "All Countries" 
    });
    
    // Level 3: State (drill down from Country)
    pivotChart1.PivotAxis.Add(new PivotItem 
    { 
        FieldMappingName = "State", 
        TotalHeader = "All States" 
    });
    
    // Bind data
    pivotChart1.ItemSource = GetSalesData();
    
    // Configure legend and calculations
    pivotChart1.PivotLegend.Add(new PivotItem { FieldMappingName = "Date", TotalHeader = "Total" });
    pivotChart1.PivotCalculations.Add(new PivotComputationInfo { FieldName = "Quantity", Format = "#,##0" });
}
```

## How Drill Operations Work

### Hierarchy Structure

The drill hierarchy is defined by the order of PivotItems in PivotAxis:

```csharp
// 4-level hierarchy example
pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "Category" });  // Level 1
pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "Product" });   // Level 2
pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "Region" });    // Level 3
pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "State" });     // Level 4
```

**Initial View:** Shows Level 1 (Category) aggregated  
**First Drill-Down:** Shows Level 2 (Products within selected Category)  
**Second Drill-Down:** Shows Level 3 (Regions within selected Product)  
**Third Drill-Down:** Shows Level 4 (States within selected Region)

### Expander Icons

When `AllowDrillDown = true`, the chart displays:
- **Plus (+) icons** - Indicate you can drill down to see more detail
- **Minus (-) icons** - Indicate you can drill up to see less detail

![Drill expanders](../../../../../docs/Pivot-Chart/Drill-UpDown-Level_images/Drill-UpDown-Level_img1.png)

## User Interactions

### Drill-Down Workflow

1. **Initial Display:** Chart shows top-level aggregation
2. **Click (+) expander** next to a category
3. **Chart updates** to show next level for that category only
4. **Repeat** to drill deeper into hierarchy

### Drill-Up Workflow

1. **From drilled state:** Chart shows detailed level
2. **Click (-) expander** next to category header
3. **Chart collapses** back to previous aggregation level
4. **Repeat** to return to top level

### Example User Journey

**Starting Point:**
```
Chart shows: [Bike] [Car] [Scooter]  ← All products aggregated
```

**User clicks (+) on "Bike":**
```
Chart shows: [Bike: USA] [Bike: Canada] [Bike: Mexico]  ← Countries for Bike
```

**User clicks (+) on "Bike: USA":**
```
Chart shows: [Bike: USA: CA] [Bike: USA: NY] [Bike: USA: TX]  ← States for USA
```

**User clicks (-) to drill up:**
```
Chart shows: [Bike: USA] [Bike: Canada] [Bike: Mexico]  ← Back to countries
```

## Programmatic Drill Control

While drill operations are primarily user-driven, you can control them programmatically:

### Checking Drill State

```csharp
// Check if drill-down is enabled
bool isDrillEnabled = pivotChart1.AllowDrillDown;

// Get current hierarchy level (custom tracking)
private int currentDrillLevel = 0;

private void TrackDrillLevel()
{
    // Implement custom logic to track which level user is viewing
    // This requires hooking into chart events or custom state management
}
```

### Resetting to Top Level

```csharp
// Reset to top-level view
private void ResetToTopLevel()
{
    // Refresh chart to reset drill state
    pivotChart1.Refresh();
}
```

### Complete Multi-Level Example

```csharp
public partial class DrillDownDemo : Form
{
    private PivotChart pivotChart1;
    private Label lblCurrentLevel;
    
    public DrillDownDemo()
    {
        InitializeComponent();
        SetupDrillChart();
    }
    
    private void SetupDrillChart()
    {
        // Create UI elements
        lblCurrentLevel = new Label 
        { 
            Text = "Current Level: Top Level", 
            Dock = DockStyle.Top, 
            Height = 30 
        };
        this.Controls.Add(lblCurrentLevel);
        
        // Create pivot chart
        pivotChart1 = new PivotChart { Dock = DockStyle.Fill };
        
        // Enable drill-down
        pivotChart1.AllowDrillDown = true;
        
        // Configure 4-level hierarchy
        pivotChart1.PivotAxis.Add(new PivotItem 
        { 
            FieldMappingName = "Category", 
            TotalHeader = "All Categories",
            FieldHeader = "Category" 
        });
        
        pivotChart1.PivotAxis.Add(new PivotItem 
        { 
            FieldMappingName = "Product", 
            TotalHeader = "All Products",
            FieldHeader = "Product" 
        });
        
        pivotChart1.PivotAxis.Add(new PivotItem 
        { 
            FieldMappingName = "Region", 
            TotalHeader = "All Regions",
            FieldHeader = "Region" 
        });
        
        pivotChart1.PivotAxis.Add(new PivotItem 
        { 
            FieldMappingName = "State", 
            TotalHeader = "All States",
            FieldHeader = "State" 
        });
        
        // Bind data
        pivotChart1.ItemSource = GetHierarchicalData();
        
        // Configure legend and calculations
        pivotChart1.PivotLegend.Add(new PivotItem 
        { 
            FieldMappingName = "Quarter", 
            TotalHeader = "All Quarters" 
        });
        
        pivotChart1.PivotCalculations.Add(new PivotComputationInfo 
        { 
            FieldName = "Revenue", 
            Format = "C0",
            FieldHeader = "Total Revenue" 
        });
        
        this.Controls.Add(pivotChart1);
    }
    
    private List<SalesData> GetHierarchicalData()
    {
        return new List<SalesData>
        {
            new SalesData { Category = "Electronics", Product = "Laptop", Region = "North", State = "NY", Quarter = "Q1", Revenue = 50000 },
            new SalesData { Category = "Electronics", Product = "Laptop", Region = "North", State = "CA", Quarter = "Q1", Revenue = 65000 },
            new SalesData { Category = "Electronics", Product = "Laptop", Region = "South", State = "TX", Quarter = "Q1", Revenue = 45000 },
            new SalesData { Category = "Electronics", Product = "Tablet", Region = "North", State = "NY", Quarter = "Q1", Revenue = 30000 },
            new SalesData { Category = "Electronics", Product = "Tablet", Region = "South", State = "FL", Quarter = "Q1", Revenue = 28000 },
            new SalesData { Category = "Furniture", Product = "Desk", Region = "North", State = "NY", Quarter = "Q1", Revenue = 20000 },
            new SalesData { Category = "Furniture", Product = "Desk", Region = "South", State = "TX", Quarter = "Q1", Revenue = 18000 },
            new SalesData { Category = "Furniture", Product = "Chair", Region = "North", State = "CA", Quarter = "Q1", Revenue = 15000 },
            // Add more hierarchical data...
        };
    }
}

public class SalesData
{
    public string Category { get; set; }
    public string Product { get; set; }
    public string Region { get; set; }
    public string State { get; set; }
    public string Quarter { get; set; }
    public decimal Revenue { get; set; }
}
```

## Best Practices

### Hierarchy Design

1. **Logical Progression:** Order levels from general to specific
   ```csharp
   // Good: General → Specific
   Category → Product → Region → State
   
   // Avoid: Mixed levels
   Product → Category → State → Region  // Confusing order
   ```

2. **Limit Depth:** 3-4 levels optimal for usability
   ```csharp
   // Optimal
   Product → Country → State  // 3 levels
   
   // Too deep
   Category → Subcategory → Product → Model → Variant → SKU  // 6 levels - confusing
   ```

3. **Meaningful Aggregations:** Each level should provide value
   ```csharp
   // Good: Each level answers a question
   Year → Quarter → Month  // When did it happen?
   Region → Country → State  // Where did it happen?
   
   // Avoid: Redundant levels
   Product → ProductName → ProductID  // Redundant
   ```

### User Experience

1. **Provide Context:** Use descriptive TotalHeader values
   ```csharp
   new PivotItem 
   { 
       FieldMappingName = "Product", 
       TotalHeader = "All Products",      // Clear
       FieldHeader = "Product Name"       // Descriptive
   }
   ```

2. **Enable by Default:** If you have hierarchical data, enable drill-down
   ```csharp
   // If PivotAxis has 2+ items, enable drill-down
   if (pivotChart1.PivotAxis.Count >= 2)
   {
       pivotChart1.AllowDrillDown = true;
   }
   ```

3. **Test Drill Paths:** Verify all drill combinations work correctly
   ```csharp
   // Test each level
   // - Can I drill into each top-level item?
   // - Does data display correctly at each level?
   // - Can I drill back up successfully?
   ```

### Performance

1. **Optimize Data:** Ensure efficient data retrieval at each level
   ```csharp
   // Use indexed fields for hierarchy
   // Avoid expensive calculations in drill paths
   ```

2. **Consider Data Volume:** More levels = more navigation
   ```csharp
   // For large datasets, consider:
   // - Filtering options
   // - Search functionality
   // - Direct navigation to specific levels
   ```

## Common Use Cases

### Sales Analysis Hierarchy
```csharp
// Product → Region → State → City
pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "Product" });
pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "Region" });
pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "State" });
pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "City" });
```

### Time-Based Hierarchy
```csharp
// Year → Quarter → Month
pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "Year" });
pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "Quarter" });
pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "Month" });
```

### Organizational Hierarchy
```csharp
// Department → Team → Employee
pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "Department" });
pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "Team" });
pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "Employee" });
```

### Product Catalog Hierarchy
```csharp
// Category → Subcategory → Brand → Product
pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "Category" });
pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "Subcategory" });
pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "Brand" });
pivotChart1.PivotAxis.Add(new PivotItem { FieldMappingName = "Product" });
```

## Troubleshooting

### Issue: Drill-Down Not Working

**Problem:** Clicking expanders does nothing

**Solutions:**
1. Verify `AllowDrillDown = true`
2. Ensure PivotAxis has 2+ items
3. Check that data has hierarchical relationships
4. Verify FieldMappingName matches data properties

### Issue: No Expanders Visible

**Problem:** Chart shows data but no drill icons

**Causes & Solutions:**
- Only 1 item in PivotAxis → Add more hierarchy levels
- AllowDrillDown = false → Set to true
- Data doesn't support drilling → Verify hierarchical structure exists

### Issue: Unexpected Data After Drill

**Problem:** Drilled data seems incorrect

**Solutions:**
1. Verify data relationships are correct
2. Check FieldMappingName accuracy
3. Ensure data has proper parent-child relationships
4. Test with known sample data

## Next Steps

- Configure **Chart Types** for different visualizations at each drill level
- Add **Grouping Bar** for user-controlled field arrangement
- Implement **Export** to save drilled views
- Customize **Appearance** for better visual hierarchy indication
