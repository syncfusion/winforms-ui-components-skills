# Pivot Schema Designer

## Table of Contents
- [Overview](#overview)
- [Understanding the Pivot Schema Designer](#understanding-the-pivot-schema-designer)
- [Fields Section](#fields-section)
- [Layout Section](#layout-section)
- [Enabling the Pivot Table Field List](#enabling-the-pivot-table-field-list)
- [Setting Field Captions](#setting-field-captions)
- [Interactive Features](#interactive-features)
- [Programmatic Access](#programmatic-access)

## Overview

The Pivot Schema Designer, also known as the Pivot Table Field List, provides an Excel-like interface for configuring pivot grid structure. It allows users to dynamically add, rearrange, filter, and remove pivot fields through drag-and-drop interactions without writing code.

**Key Benefits:**
- Excel-like user experience
- Visual field organization
- Drag-and-drop field management
- Real-time pivot structure updates
- Support for duplicate fields with different captions

## Understanding the Pivot Schema Designer

The Pivot Schema Designer consists of two main sections:

### 1. Fields Section (Top)
- Displays all available fields from the data source
- Checkboxes to add/remove fields
- Drag source for adding fields to layout sections

### 2. Layout Section (Bottom)
- **Filters** - Apply report-level filtering
- **Columns** - Define column headers
- **Rows** - Define row headers
- **Values** - Define summary calculations

## Fields Section

The Fields Section contains the list of all pivot items available in the bound data source.

### Accessing Fields Collection

```csharp
using Syncfusion.Windows.Forms.PivotAnalysis;

// Access the pivot fields collection
var pivotFields = pivotGridControl1.PivotFields;

// Iterate through available fields
foreach (var field in pivotFields)
{
    Console.WriteLine($"Field: {field.FieldMappingName}");
}
```

### Adding Fields to Pivot Grid

**Interactive Method:**
1. Check the checkbox beside a field name
2. Field is automatically added to the Rows section by default
3. Drag field to other sections (Columns, Values, Filters) if needed

**Result:** Checking a field adds it to the pivot grid; unchecking removes it.

### Default Behavior

- By default, checked fields are added to the **Rows** section
- To add to other sections, drag from Fields to desired layout section
- Each field can appear multiple times with different captions

## Layout Section

The Layout Section is divided into four drop zones:

### Filters Section

**Purpose:** Apply report-level filters to the entire pivot grid

**Accessing Programmatically:**
```csharp
// Access filter list
var filterList = pivotGridControl1.TableControl.GridFilterList;

// Add field to filters
pivotGridControl1.Filters.Add(new PivotItem 
{ 
    FieldMappingName = "Country" 
});
```

**User Interaction:**
- Click the filter icon on a filter field
- Select specific values to include
- Filter applies across entire grid

### Columns Section

**Purpose:** Define fields that appear as column headers

**Accessing Programmatically:**
```csharp
// Access column list
var columnList = pivotGridControl1.TableControl.GridColumnList;

// Add columns programmatically
pivotGridControl1.PivotColumns.Add(new PivotItem 
{ 
    FieldMappingName = "Date", 
    TotalHeader = "Total" 
});
```

**Features:**
- Multiple fields create nested column headers
- Drag to reorder columns
- Remove by dragging out of section

### Rows Section

**Purpose:** Define fields that appear as row headers

**Accessing Programmatically:**
```csharp
// Access row list
var rowList = pivotGridControl1.TableControl.GridRowList;

// Add rows programmatically
pivotGridControl1.PivotRows.Add(new PivotItem 
{ 
    FieldMappingName = "Product", 
    TotalHeader = "Total" 
});
```

**Features:**
- Multiple fields create nested row headers
- Drag to reorder rows
- Hierarchical drill-down support

### Values Section

**Purpose:** Define summary calculations displayed in value cells

**Accessing Programmatically:**
```csharp
// Access value list
var valueList = pivotGridControl1.TableControl.GridValueList;

// Add calculations programmatically
pivotGridControl1.PivotCalculations.Add(new PivotComputationInfo 
{ 
    FieldName = "Amount", 
    Format = "C", 
    SummaryType = SummaryType.DoubleTotalSum 
});
```

**Features:**
- Multiple values display side-by-side
- Drag to reorder calculations
- Configure summary types (Sum, Average, Count, etc.)

## Enabling the Pivot Table Field List

By default, the Pivot Schema Designer is hidden. Enable it using the `ShowPivotTableFieldList` property:

### Basic Enablement

```csharp
using Syncfusion.Windows.Forms.PivotAnalysis;

// Show the Pivot Table Field List
pivotGridControl1.ShowPivotTableFieldList = true;
```

**VB.NET:**
```vb
' Show the Pivot Table Field List
pivotGridControl1.ShowPivotTableFieldList = True
```

### Complete Example with Schema Designer

```csharp
public partial class MainForm : Form
{
    private PivotGridControl pivotGridControl1;
    
    public MainForm()
    {
        InitializeComponent();
        SetupPivotGrid();
    }
    
    private void SetupPivotGrid()
    {
        pivotGridControl1 = new PivotGridControl(this.components);
        pivotGridControl1.Location = new Point(10, 10);
        pivotGridControl1.Size = new Size(800, 500);
        pivotGridControl1.Anchor = AnchorStyles.Top | AnchorStyles.Left | 
                                   AnchorStyles.Right | AnchorStyles.Bottom;
        
        // Bind data
        pivotGridControl1.ItemSource = ProductSales.GetSalesData();
        
        // Configure initial structure
        pivotGridControl1.PivotRows.Add(new PivotItem 
        { 
            FieldMappingName = "Product", 
            TotalHeader = "Total" 
        });
        pivotGridControl1.PivotColumns.Add(new PivotItem 
        { 
            FieldMappingName = "Country", 
            TotalHeader = "Total" 
        });
        pivotGridControl1.PivotCalculations.Add(new PivotComputationInfo 
        { 
            FieldName = "Amount", 
            Format = "C", 
            SummaryType = SummaryType.DoubleTotalSum 
        });
        
        // Enable Pivot Schema Designer
        pivotGridControl1.ShowPivotTableFieldList = true;
        
        this.Controls.Add(pivotGridControl1);
    }
}
```

**Result:** The Pivot Table Field List appears next to the grid, allowing users to reorganize fields visually.

## Setting Field Captions

Field captions allow using the same field multiple times with different display names. This is useful for:
- Showing the same field in different aggregations
- Creating calculated variations of a field
- Displaying duplicate fields with meaningful names

### Row and Column Captions

```csharp
using Syncfusion.PivotAnalysis.Base;

// Add same field twice with different captions
pivotGridControl1.PivotRows.Add(new PivotItem 
{ 
    FieldMappingName = "Product", 
    FieldCaption = "Product_1",  // Custom display name
    TotalHeader = "Total" 
});

pivotGridControl1.PivotRows.Add(new PivotItem 
{ 
    FieldMappingName = "Product", 
    FieldCaption = "Product_2",  // Different display name
    TotalHeader = "Total" 
});
```

**VB.NET:**
```vb
' Add same field with different captions
pivotGridControl1.PivotRows.Add(New PivotItem With _
{ 
    .FieldMappingName = "Product", _
    .FieldCaption = "Product_1", _
    .TotalHeader = "Total" 
})
```

### Calculation Captions

```csharp
// Add same calculation field with different formats/types
pivotGridControl1.PivotCalculations.Add(new PivotComputationInfo 
{ 
    FieldName = "Amount", 
    FieldCaption = "Amount($)",  // Currency format
    Format = "C", 
    SummaryType = SummaryType.DoubleTotalSum 
});

pivotGridControl1.PivotCalculations.Add(new PivotComputationInfo 
{ 
    FieldName = "Amount", 
    FieldCaption = "Amount(Units)",  // Integer format
    Format = "##", 
    SummaryType = SummaryType.IntTotalSum 
});
```

**Use Cases:**
- Display sales in both dollars and units
- Show quantities in different units (kg vs lbs)
- Calculate same field with different aggregations (Sum vs Average)

## Interactive Features

### Adding Fields

**Method 1: Checkbox Selection**
1. Check the checkbox next to a field name
2. Field appears in the default section (Rows)
3. Grid updates automatically

**Method 2: Drag and Drop**
1. Drag field from Fields Section
2. Drop onto desired layout section (Rows, Columns, Values, Filters)
3. Field appears in that section

### Reordering Fields

#### Within Same Section

Drag a field up or down within its current section to change order:

```plaintext
Before: Product → Date → Country
Action: Drag Date above Product
After: Date → Product → Country
```

**Effect:** Changes the nesting hierarchy in the pivot grid.

#### Between Sections

Drag a field from one layout section to another:

```plaintext
Action: Drag "Date" from Rows to Columns
Result: Date becomes a column header instead of row header
```

#### Using Context Menu

1. Click the down arrow icon on a field in any layout section
2. Context menu appears with options:
   - Move to Top
   - Move Up
   - Move Down
   - Move to Bottom
   - Remove Field
   - Field Settings

### Removing Fields

**Method 1: Uncheck Checkbox**
- Uncheck the field in the Fields Section
- Field is removed from all layout sections

**Method 2: Drag Out**
- Drag field out of layout section
- Release outside any drop zone
- Field is removed from that section

**Method 3: Context Menu**
- Right-click field or click down arrow
- Select "Remove Field"

### Cross-Component Drag-Drop

Fields can be dragged between the Pivot Schema Designer and the Grouping Bar (if enabled):

```csharp
// Enable both features for cross-dragging
pivotGridControl1.ShowPivotTableFieldList = true;
pivotGridControl1.ShowGroupBar = true;
```

**Drag Operations:**
- Schema Designer → Grouping Bar
- Grouping Bar → Schema Designer
- Between different sections in either component

## Programmatic Access

### Accessing Current Configuration

```csharp
// Get all rows
foreach (PivotItem row in pivotGridControl1.PivotRows)
{
    Console.WriteLine($"Row: {row.FieldMappingName}, Caption: {row.FieldCaption}");
}

// Get all columns
foreach (PivotItem col in pivotGridControl1.PivotColumns)
{
    Console.WriteLine($"Column: {col.FieldMappingName}");
}

// Get all calculations
foreach (PivotComputationInfo calc in pivotGridControl1.PivotCalculations)
{
    Console.WriteLine($"Calculation: {calc.FieldName}, " +
                     $"Type: {calc.SummaryType}, Format: {calc.Format}");
}

// Get all filters
foreach (PivotItem filter in pivotGridControl1.Filters)
{
    Console.WriteLine($"Filter: {filter.FieldMappingName}");
}
```

### Modifying Configuration Programmatically

```csharp
// Clear all pivot items
pivotGridControl1.PivotRows.Clear();
pivotGridControl1.PivotColumns.Clear();
pivotGridControl1.PivotCalculations.Clear();

// Add new configuration
pivotGridControl1.PivotRows.Add(new PivotItem 
{ 
    FieldMappingName = "Category", 
    TotalHeader = "Total" 
});

// Synchronize changes
pivotGridControl1.TableControl.Refresh(true);
```

### Responding to Schema Changes

```csharp
// Monitor when pivot structure changes
pivotGridControl1.PivotRows.CollectionChanged += (s, e) =>
{
    if (e.Action == System.Collections.Specialized.NotifyCollectionChangedAction.Add)
    {
        Console.WriteLine("New row added to pivot structure");
    }
};

pivotGridControl1.PivotColumns.CollectionChanged += (s, e) =>
{
    Console.WriteLine($"Column structure changed: {e.Action}");
};
```

## Best Practices

1. **Enable for End Users:**
   - Always enable Schema Designer for business users who need flexibility
   - Hide it for fixed-layout dashboards

2. **Initial Configuration:**
   - Set up a sensible default pivot structure before showing the Schema Designer
   - Users can then customize from a good starting point

3. **Field Captions:**
   - Use meaningful captions when duplicating fields
   - Indicate units or aggregation types in captions

4. **Combine with Grouping Bar:**
   - Enable both Schema Designer and Grouping Bar for maximum flexibility
   - Users can choose their preferred interaction method

5. **Save/Load Layouts:**
   - Allow users to save their custom pivot configurations
   - Restore saved layouts on application restart

## Troubleshooting

**Issue:** Schema Designer not visible after setting property
```csharp
// Solution: Ensure property is set AFTER data binding
pivotGridControl1.ItemSource = data;
pivotGridControl1.ShowPivotTableFieldList = true;  // Set after data binding
```

**Issue:** Fields not appearing in Fields Section
```csharp
// Solution: Verify data source is bound and contains data
if (pivotGridControl1.ItemSource != null && 
    pivotGridControl1.PivotFields.Count == 0)
{
    pivotGridControl1.TableControl.Refresh(true);  // Force refresh
}
```

**Issue:** Duplicate field captions conflict
```csharp
// Solution: Ensure each duplicated field has a unique FieldCaption
pivotGridControl1.PivotRows.Add(new PivotItem 
{ 
    FieldMappingName = "Product", 
    FieldCaption = "Product_Primary"  // Unique caption required
});
```
