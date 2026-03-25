# Grouping in GridGroupingControl

## Table of Contents
- [Overview](#overview)
- [Adding Data Groups](#adding-data-groups)
- [Removing Data Groups](#removing-data-groups)
- [GroupDropArea](#groupdroparea)
- [Hierarchical GroupDropArea](#hierarchical-groupdroparea)
- [Multi-Level Grouping](#multi-level-grouping)
- [Common Scenarios](#common-scenarios)

## Overview

GridGroupingControl organizes data into hierarchical structures based on matching field values. When you group by one or more columns, records with the same values collapse into expandable groups with summary information.

**Key concepts:**
- **GroupDropArea:** Visual panel at top where users drag column headers to group
- **GroupedColumns:** Collection defining which fields are grouped and their sort order
- **Nested Groups:** Multiple grouping levels create parent-child group hierarchies
- **Group Summaries:** Automatic calculations (Count, Sum, Average) for each group

### Basic Architecture

```csharp
// Enable grouping interface
gridGroupingControl1.ShowGroupDropArea = true;

// Group by single column
gridGroupingControl1.TableDescriptor.GroupedColumns.Add("Department");

// Result: Records grouped by Department values
// - Sales
//   - Record 1
//   - Record 2
// - Marketing
//   - Record 3
```

## Adding Data Groups

### Through Designer

1. Open **TableDescriptor** in Properties window
2. Expand **GroupedColumns** property
3. Click **...** to open `SortColumnDescriptorCollection` Editor
4. Click **Add** button
5. Set **Name** (field to group) and **SortDirection**

![GroupedColumns Designer](Grouping_images/Grouping_img2.jpeg)

### Programmatically

Add column names to `GroupedColumns` collection:

```csharp
// Simple group - ascending order
gridGroupingControl1.TableDescriptor.GroupedColumns.Add("Title");
```

**Specify sort direction:**

```csharp
using System.ComponentModel;

// Group by Title, descending order
gridGroupingControl1.TableDescriptor.GroupedColumns.Add("Title", 
    ListSortDirection.Descending);
```

### Grouping Nested Tables

Group child tables by accessing their descriptors:

```csharp
// Group child table named "Orders"
gridGroupingControl1.TableDescriptor.Relations[0]
    .ChildTableDescriptor.GroupedColumns.Add("UnitPrice", 
        ListSortDirection.Descending);
```

**Example with master-detail:**

```csharp
// Parent: Categories table
gridGroupingControl1.TableDescriptor.GroupedColumns.Add("CategoryName");

// Child: Products table grouped by Supplier
gridGroupingControl1.TableDescriptor.Relations["CategoryProducts"]
    .ChildTableDescriptor.GroupedColumns.Add("SupplierName");
```

## Removing Data Groups

### Remove Specific Group

```csharp
// By column name
gridGroupingControl1.TableDescriptor.GroupedColumns.Remove("Title");

// By index (removes first group)
gridGroupingControl1.TableDescriptor.GroupedColumns.RemoveAt(0);
```

### Clear All Groups

```csharp
// Ungroup all columns
gridGroupingControl1.TableDescriptor.GroupedColumns.Clear();
```

### Prevent Grouping by Column

Disable grouping for specific columns:

```csharp
// User cannot drag "CompanyName" to GroupDropArea
gridGroupingControl1.TableDescriptor.Columns["CompanyName"].AllowGroupByColumn = false;
```

### Prevent Grouping Using Event

Handle `TableControlQueryAllowGroupByColumn` for conditional prevention:

```csharp
gridGroupingControl1.TableControlQueryAllowGroupByColumn += 
    (s, e) =>
    {
        if (e.Column == "Title" || e.Column == "ID")
        {
            e.AllowGroupByColumn = false;
        }
    };
```

## GroupDropArea

Interactive panel where users drag column headers to group data.

### Enabling GroupDropArea

```csharp
// Show drop area for top-level table
gridGroupingControl1.ShowGroupDropArea = true;
```

![GroupDropArea](Grouping_images/Grouping_img1.jpeg)

### Adding GroupDropArea for Nested Tables

Manually add drop areas for child tables:

```csharp
// Get table references
Table categoriesTable = gridGroupingControl1.Engine.Table;
Table productsTable = categoriesTable.RelatedTables["Products"];
Table orderDetailsTable = productsTable.RelatedTables["OrdersDetails"];

// Add drop areas
gridGroupingControl1.AddGroupDropArea((GridTable)productsTable);
gridGroupingControl1.AddGroupDropArea((GridTable)orderDetailsTable);
```

### Customizing GroupDropArea

**Change splitter color:**

```csharp
gridGroupingControl1.Splitter.BackColor = Color.Green;
```

**Change drop area background:**

```csharp
gridGroupingControl1.GroupDropPanel.BackColor = Color.LightBlue;
```

**Customize individual drop areas:**

```csharp
foreach (Control ctrl in gridGroupingControl1.GroupDropPanel.Controls)
{
    GridGroupDropArea dropArea = ctrl as GridGroupDropArea;
    
    if (dropArea != null)
    {
        string tableName = dropArea.Model.Table.TableDescriptor.Name;
        
        switch (tableName)
        {
            case "Customers":
                dropArea.BackColor = Color.DarkOliveGreen;
                dropArea.PrepareViewStyleInfo += (s, e) =>
                {
                    if (e.ColIndex == 2 && e.RowIndex == 2)
                    {
                        e.Style.Text = "Customer Groups";
                        e.Style.Font.Bold = true;
                        e.Style.TextColor = Color.Yellow;
                    }
                    else if (e.Style.Text.StartsWith("Drag a"))
                    {
                        e.Style.Text = "Drag column headers here";
                        e.Style.Font.Italic = true;
                    }
                };
                break;
                
            case "Orders":
                dropArea.BackColor = Color.DarkSlateBlue;
                break;
        }
    }
}
```

### GroupDropArea Position

Change alignment using `GroupDropAreaAlignment`:

```csharp
// Top (default)
gridGroupingControl1.GroupDropAreaAlignment = GridGroupDropAreaAlignment.Top;

// Left side
gridGroupingControl1.GroupDropAreaAlignment = GridGroupDropAreaAlignment.Left;

// Bottom
gridGroupingControl1.GroupDropAreaAlignment = GridGroupDropAreaAlignment.Bottom;

// Right side
gridGroupingControl1.GroupDropAreaAlignment = GridGroupDropAreaAlignment.Right;
```

## Hierarchical GroupDropArea

Display grouped columns in hierarchical tree structure:

```csharp
// Enable hierarchical display
gridGroupingControl1.HierarchicalGroupDropArea = true;
```

![Hierarchical GroupDropArea](Grouping_images/Grouping_img14.jpeg)

### Additional Features

```csharp
GridGroupDropArea dropArea = gridGroupingControl1.GridGroupDropArea;

// Allow removing columns by clicking X button
dropArea.AllowRemove = true;

// Position tree lines (Top or Bottom)
dropArea.TreeLinePlacement = TreeLinePlacement.Bottom;

// Auto-resize to accommodate hierarchy
dropArea.DynamicResizing = true;

// Customize tree line color
dropArea.TreeLineColor = Color.Red;
```

## Multi-Level Grouping

Create nested groups by adding multiple columns:

```csharp
// Group by Country, then City, then Company
gridGroupingControl1.TableDescriptor.GroupedColumns.Add("Country");
gridGroupingControl1.TableDescriptor.GroupedColumns.Add("City");
gridGroupingControl1.TableDescriptor.GroupedColumns.Add("CompanyName");
```

**Result structure:**
```
- USA (Country Group)
  - New York (City Group)
    - ABC Corp (Company Group)
      - Record 1
      - Record 2
    - XYZ Inc (Company Group)
      - Record 3
  - Los Angeles (City Group)
    - Global Ltd (Company Group)
      - Record 4
```

### Mixing Sort Directions

```csharp
gridGroupingControl1.TableDescriptor.GroupedColumns.Add("Category", 
    ListSortDirection.Ascending);
    
gridGroupingControl1.TableDescriptor.GroupedColumns.Add("UnitPrice", 
    ListSortDirection.Descending);
```

## Common Scenarios

### Scenario 1: Group with Custom Sort Order

**Need:** Group by Status but sort groups in custom order (Pending, InProgress, Completed)

```csharp
// Option A: Use numeric backing field
// Assign: Pending=1, InProgress=2, Completed=3
gridGroupingControl1.TableDescriptor.GroupedColumns.Add("StatusOrder");

// Option B: Custom comparer
public class StatusComparer : IComparer
{
    private Dictionary<string, int> order = new Dictionary<string, int>
    {
        { "Pending", 1 },
        { "InProgress", 2 },
        { "Completed", 3 }
    };
    
    public int Compare(object x, object y)
    {
        Group g1 = x as Group;
        Group g2 = y as Group;
        
        string status1 = g1.Info.ToString();
        string status2 = g2.Info.ToString();
        
        return order[status1].CompareTo(order[status2]);
    }
}

// Apply comparer
SortColumnDescriptor descriptor = new SortColumnDescriptor("Status");
descriptor.Comparer = new StatusComparer();
gridGroupingControl1.TableDescriptor.GroupedColumns.Add(descriptor);
```

### Scenario 2: Programmatic Group Expansion

**Need:** Expand specific groups, collapse others

```csharp
// Expand all groups
gridGroupingControl1.ExpandAllGroups();

// Collapse all groups
gridGroupingControl1.CollapseAllGroups();

// Expand to specific level (0-based)
gridGroupingControl1.ExpandAllGroups();
gridGroupingControl1.Table.CollapseAllGroups();
foreach (Group g in gridGroupingControl1.Table.TopLevelGroup.Groups)
{
    if (g.Info.ToString() == "Sales") // Expand only "Sales" group
        g.IsExpanded = true;
}

// Expand nested child groups
foreach (Group parentGroup in gridGroupingControl1.Table.TopLevelGroup.Groups)
{
    if (parentGroup.Info.ToString() == "USA")
    {
        parentGroup.IsExpanded = true;
        
        foreach (Group childGroup in parentGroup.Groups)
        {
            if (childGroup.Info.ToString() == "New York")
                childGroup.IsExpanded = true;
        }
    }
}
```

### Scenario 3: Group by Date Ranges

**Need:** Group dates into Year/Month/Day hierarchy

```csharp
// Option A: Add expression fields
ExpressionFieldDescriptor yearField = new ExpressionFieldDescriptor("Year", "Year([OrderDate])", "System.Int32");
ExpressionFieldDescriptor monthField = new ExpressionFieldDescriptor("Month", "Month([OrderDate])", "System.Int32");
gridGroupingControl1.TableDescriptor.ExpressionFields.Add(yearField);
gridGroupingControl1.TableDescriptor.ExpressionFields.Add(monthField);

gridGroupingControl1.TableDescriptor.GroupedColumns.Add("Year");
gridGroupingControl1.TableDescriptor.GroupedColumns.Add("Month");

// Option B: Custom grouping intervals (daily, weekly, monthly)
gridGroupingControl1.TableDescriptor.GroupedColumns.Add("OrderDate");

// Access group options for date intervals (requires custom GroupCategorizer)
```

### Scenario 4: Prevent Specific Grouping Combinations

**Need:** Allow grouping by Category OR Supplier, but not both

```csharp
gridGroupingControl1.TableDescriptor.GroupedColumns.Changing += (s, e) =>
{
    if (e.Action == ListPropertyChangedType.Add)
    {
        SortColumnDescriptor newGroup = e.Item as SortColumnDescriptor;
        
        if (newGroup.Name == "Category" && 
            gridGroupingControl1.TableDescriptor.GroupedColumns.Contains("Supplier"))
        {
            MessageBox.Show("Cannot group by Category when grouped by Supplier");
            e.Cancel = true;
        }
        else if (newGroup.Name == "Supplier" && 
                 gridGroupingControl1.TableDescriptor.GroupedColumns.Contains("Category"))
        {
            MessageBox.Show("Cannot group by Supplier when grouped by Category");
            e.Cancel = true;
        }
    }
};
```

### Scenario 5: Custom Group Captions

**Need:** Show "Department: Sales (15 employees)" instead of "Sales: 15"

```csharp
gridGroupingControl1.QueryCellStyleInfo += (s, e) =>
{
    Element el = e.TableCellIdentity.DisplayElement;
    
    if (el is CaptionSection)
    {
        CaptionSection caption = el as CaptionSection;
        Group group = caption.ParentGroup;
        
        if (group.GroupLevel == 1) // First grouping level
        {
            string categoryName = group.Info.ToString();
            int count = group.GetFilteredRecordCount();
            e.Style.Text = $"Department: {categoryName} ({count} employees)";
        }
    }
};
```

### Scenario 6: Dynamic Grouping Based on Selection

**Need:** Group by user-selected columns from CheckBoxList

```csharp
private void ApplySelectedGrouping()
{
    gridGroupingControl1.TableDescriptor.GroupedColumns.Clear();
    
    // Assume checkedListBoxColumns contains selected column names
    foreach (string columnName in checkedListBoxColumns.CheckedItems)
    {
        gridGroupingControl1.TableDescriptor.GroupedColumns.Add(columnName);
    }
    
    gridGroupingControl1.Table.Refresh();
}
```

## Events

Key events for grouping operations:

```csharp
// Before group is added/removed/modified
gridGroupingControl1.TableDescriptor.GroupedColumns.Changing += (s, e) =>
{
    if (e.Action == ListPropertyChangedType.Add)
    {
        Console.WriteLine($"Adding group: {((SortColumnDescriptor)e.Item).Name}");
        // e.Cancel = true; // Prevent grouping
    }
};

// After group is added/removed/modified
gridGroupingControl1.TableDescriptor.GroupedColumns.Changed += (s, e) =>
{
    if (e.Action == ListPropertyChangedType.Add)
    {
        Console.WriteLine("Group added successfully");
        // Update UI, refresh summaries, etc.
    }
};

// Before records in a group are sorted
gridGroupingControl1.SortingItemsInGroup += (s, e) =>
{
    Group group = e.Group;
    Console.WriteLine($"Sorting group: {group.Info}");
};

// After records in a group are sorted
gridGroupingControl1.SortedItemsInGroup += (s, e) =>
{
    Group group = e.Group;
    Console.WriteLine($"Group sorted: {group.Info}");
};

// User attempts to drag column to GroupDropArea
gridGroupingControl1.TableControlQueryAllowGroupByColumn += (s, e) =>
{
    if (e.Column == "ID")
    {
        e.AllowGroupByColumn = false;
        MessageBox.Show("Cannot group by ID column");
    }
};
```

## Best Practices

1. **Limit grouping levels:** 3-4 levels maximum for usability
2. **Sort grouped columns:** Specify ListSortDirection for predictable order
3. **Disable grouping for key columns:** Prevent grouping by ID, unique keys
4. **Use summaries:** Show Count, Sum, Average in group captions
5. **Handle empty groups:** Configure CaptionSection appearance for null values
6. **Performance:** Use BeginUpdate/EndUpdate when adding multiple groups
7. **Persist state:** Save/restore GroupedColumns order for user sessions

## Common Issues

### Groups Not Appearing

```csharp
// Verify ShowGroupDropArea is enabled
gridGroupingControl1.ShowGroupDropArea = true;

// Check column exists
if (!gridGroupingControl1.TableDescriptor.Columns.Contains("CategoryName"))
{
    MessageBox.Show("Column not found");
    return;
}

// Verify data exists
if (gridGroupingControl1.Table.Records.Count == 0)
{
    MessageBox.Show("No records to group");
}
```

### Nested Table GroupDropArea Not Showing

```csharp
// Must explicitly add for child tables
Table childTable = gridGroupingControl1.Engine.Table.RelatedTables["Orders"];
if (childTable != null)
{
    gridGroupingControl1.AddGroupDropArea((GridTable)childTable);
}
```

### Group Captions Showing Wrong Values

```csharp
// Ensure field mapping is correct
ExpressionFieldDescriptor expr = new ExpressionFieldDescriptor(
    "DisplayName",  // Name used in grouping
    "[FirstName] + ' ' + [LastName]",  // Expression
    "System.String"  // Type
);
gridGroupingControl1.TableDescriptor.ExpressionFields.Add(expr);
gridGroupingControl1.TableDescriptor.GroupedColumns.Add("DisplayName");
```