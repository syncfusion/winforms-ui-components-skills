# Grouping Data in Grouping Engine

## Table of Contents
- [Overview](#overview)
- [TableDescriptor and Schema Information](#tabledescriptor-and-schema-information)
- [Grouping a Table](#grouping-a-table)
- [Accessing Groups](#accessing-groups)
- [Recursive Group Navigation](#recursive-group-navigation)
- [Retrieving Specific Groups](#retrieving-specific-groups)
- [Group Hierarchy Patterns](#group-hierarchy-patterns)
- [Side Effects of Grouping](#side-effects-of-grouping)

## Overview

Grouping is a data analysis technique that organizes data into logical categories. For example, you might group sales details by month to analyze monthly trends, or group inventory by category to understand stock distribution.

The Grouping Engine provides powerful grouping capabilities through the `TableDescriptor.GroupedColumns` collection. Grouping is a **recursive process**, meaning data can be grouped multiple times, creating nested group hierarchies.

## TableDescriptor and Schema Information

The `Engine.TableDescriptor` property maintains schema information for the datasource. While `Engine.Table` holds the actual data, `TableDescriptor` defines how that data is organized and processed.

### Key TableDescriptor Collections

| Collection | Description |
|------------|-------------|
| `SortedColumns` | Properties to sort by |
| `GroupedColumns` | Properties to group by |
| `Summaries` | Aggregate calculations per group |
| `RecordFilters` | Filter conditions to apply |
| `Columns` | Column schema definitions |
| `ExpressionFields` | Calculated properties |

### Accessing Column Schema

```csharp
// Get column information
foreach (PropertyDescriptor pd in groupingEngine.TableDescriptor.Columns)
{
    Console.WriteLine($"Column: {pd.Name}, Type: {pd.PropertyType}");
}
```

The `Columns` collection corresponds to the public properties in your data objects. For a class with properties A, B, C, and D, you'll have four column descriptors.

## Grouping a Table

To group data by a property, add the property name to the `GroupedColumns` collection:

### Basic Grouping

```csharp
// Group on property C
groupingEngine.TableDescriptor.GroupedColumns.Add("C");

// Display the records after grouping
foreach (Record rec in groupingEngine.Table.Records)
{
    MyObject obj = rec.GetData() as MyObject;
    
    if (obj != null)
    {
        Console.WriteLine(obj);
    }
}
```

**VB.NET Version:**

```vb
' Group on property C
groupingEngine.TableDescriptor.GroupedColumns.Add("C")

' Display the records after grouping
Dim rec As Record

For Each rec In groupingEngine.Table.Records
    Dim obj As MyObject = CType(rec.GetData(), MyObject)
    
    If Not (obj Is Nothing) Then
        Console.WriteLine(obj)
    End If
Next rec
```

**Result:** Data is now grouped by property C values. Records with the same C value are grouped together.

### Multiple Grouping Levels

Create nested group hierarchies by adding multiple properties:

```csharp
// Create three-level hierarchy
groupingEngine.TableDescriptor.GroupedColumns.Add("Region");
groupingEngine.TableDescriptor.GroupedColumns.Add("Category");
groupingEngine.TableDescriptor.GroupedColumns.Add("SubCategory");
```

This creates a hierarchy:
```
TopLevelGroup
├── Region: "North"
│   ├── Category: "Electronics"
│   │   ├── SubCategory: "Computers"
│   │   └── SubCategory: "Phones"
│   └── Category: "Furniture"
└── Region: "South"
    └── Category: "Electronics"
```

## Accessing Groups

Groups are accessed through a recursive structure starting with `Table.TopLevelGroup`.

### The Group Object

The `Group` class has two key collections:

**Group.Groups** - Collection of child groups (for parent groups)
**Group.Records** - Collection of records (for terminal groups)

**Important:** At most one of these collections is populated:
- If `Groups` has items → This is a parent group with sub-groups
- If `Records` has items → This is a terminal group with actual data records

### Accessing TopLevelGroup

```csharp
// Get the root group
Group topLevelGroup = groupingEngine.Table.TopLevelGroup;

// Check what it contains
if (topLevelGroup.Groups != null && topLevelGroup.Groups.Count > 0)
{
    Console.WriteLine($"Contains {topLevelGroup.Groups.Count} child groups");
}

if (topLevelGroup.Records != null && topLevelGroup.Records.Count > 0)
{
    Console.WriteLine($"Contains {topLevelGroup.Records.Count} records");
}
```

## Recursive Group Navigation

Use recursion to navigate through all groups and records:

### Recursive Display Method

```csharp
private static void ShowRecordsUnderGroup(Group g)
{
    if (g.Records != null && g.Records.Count > 0)
    {
        // Terminal group with records
        foreach (Record rec in g.Records)
        {
            MyObject obj = rec.GetData() as MyObject;
            
            if (obj != null)
            {
                Console.WriteLine(obj);
            }
        }
        Console.WriteLine("--");
    }
    else if (g.Groups != null && g.Groups.Count > 0)
    {
        // Parent group with sub-groups
        foreach (Group g1 in g.Groups)
        {
            // Recursive call
            ShowRecordsUnderGroup(g1);
        }
    }
}

// Call with TopLevelGroup to display all records
ShowRecordsUnderGroup(groupingEngine.Table.TopLevelGroup);
```

**VB.NET Version:**

```vb
Private Sub ShowRecordsUnderGroup(ByVal g As Group)
    If Not (g.Records Is Nothing) And g.Records.Count > 0 Then
        ' Terminal group with records
        Dim rec As Record
        
        For Each rec In g.Records
            Dim obj As MyObject = CType(rec.GetData(), MyObject)
            
            If Not (obj Is Nothing) Then
                Console.WriteLine(obj)
            End If
        Next rec
        Console.WriteLine("--")
    Else
        If Not (g.Groups Is Nothing) And g.Groups.Count > 0 Then
            ' Parent group with sub-groups
            Dim g1 As Group
            
            For Each g1 In g.Groups
                ' Recursive call
                ShowRecordsUnderGroup(g1)
            Next g1
        End If
    End If
End Sub
```

### Advanced Recursive Pattern with Indentation

```csharp
private static void DisplayGroupHierarchy(Group group, int indentLevel)
{
    string indent = new string(' ', indentLevel * 2);
    
    if (group.Records != null && group.Records.Count > 0)
    {
        // Display records in this terminal group
        Console.WriteLine($"{indent}[{group.Name}] - {group.Records.Count} records");
        
        foreach (Record rec in group.Records)
        {
            MyObject obj = rec.GetData() as MyObject;
            if (obj != null)
            {
                Console.WriteLine($"{indent}  {obj}");
            }
        }
    }
    else if (group.Groups != null && group.Groups.Count > 0)
    {
        // Display parent group and recurse into children
        Console.WriteLine($"{indent}[Group: {group.Name}]");
        
        foreach (Group childGroup in group.Groups)
        {
            DisplayGroupHierarchy(childGroup, indentLevel + 1);
        }
    }
}

// Usage
DisplayGroupHierarchy(groupingEngine.Table.TopLevelGroup, 0);
```

## Retrieving Specific Groups

Access individual groups by key value using the Groups indexer:

### Getting Group by Key

```csharp
// Group by property C
groupingEngine.TableDescriptor.GroupedColumns.Add("C");

// Get the group associated with value "c1"
Group g = groupingEngine.Table.TopLevelGroup.Groups["c1"];

// Display records in this specific group
ShowRecordsUnderGroup(g);
```

**VB.NET Version:**

```vb
' Get the group associated with value "c1"
Dim g As Group = groupingEngine.Table.TopLevelGroup.Groups("c1")
ShowRecordsUnderGroup(g)
```

### Checking if Group Exists

```csharp
// Safe access with null check
if (groupingEngine.Table.TopLevelGroup.Groups.Contains("c1"))
{
    Group group = groupingEngine.Table.TopLevelGroup.Groups["c1"];
    Console.WriteLine($"Found group 'c1' with {group.Records.Count} records");
}
else
{
    Console.WriteLine("Group 'c1' does not exist");
}
```

### Accessing Nested Groups

```csharp
// Multi-level grouping
groupingEngine.TableDescriptor.GroupedColumns.Add("Category");
groupingEngine.TableDescriptor.GroupedColumns.Add("SubCategory");

// Access nested group: Category "Electronics" → SubCategory "Computers"
Group categoryGroup = groupingEngine.Table.TopLevelGroup.Groups["Electronics"];
if (categoryGroup != null && categoryGroup.Groups != null)
{
    Group subCategoryGroup = categoryGroup.Groups["Computers"];
    if (subCategoryGroup != null)
    {
        Console.WriteLine($"Found {subCategoryGroup.Records.Count} computer records");
    }
}
```

## Group Hierarchy Patterns

### Pattern 1: Count Records Per Group

```csharp
private static void CountRecordsPerGroup(Group group)
{
    if (group.Records != null && group.Records.Count > 0)
    {
        Console.WriteLine($"Group '{group.Name}': {group.Records.Count} records");
    }
    else if (group.Groups != null && group.Groups.Count > 0)
    {
        foreach (Group childGroup in group.Groups)
        {
            CountRecordsPerGroup(childGroup);
        }
    }
}

// Usage
groupingEngine.TableDescriptor.GroupedColumns.Add("Category");
CountRecordsPerGroup(groupingEngine.Table.TopLevelGroup);
```

### Pattern 2: Find Groups Matching Criteria

```csharp
private static List<Group> FindGroupsWithMinRecords(Group group, int minRecords)
{
    List<Group> result = new List<Group>();
    
    if (group.Records != null && group.Records.Count >= minRecords)
    {
        result.Add(group);
    }
    else if (group.Groups != null && group.Groups.Count > 0)
    {
        foreach (Group childGroup in group.Groups)
        {
            result.AddRange(FindGroupsWithMinRecords(childGroup, minRecords));
        }
    }
    
    return result;
}

// Find all groups with at least 5 records
List<Group> largeGroups = FindGroupsWithMinRecords(
    groupingEngine.Table.TopLevelGroup, 
    5
);
```

### Pattern 3: Build Group Path

```csharp
private static string GetGroupPath(Group group)
{
    List<string> path = new List<string>();
    Group current = group;
    
    while (current != null && current.Name != null)
    {
        path.Insert(0, current.Name);
        current = current.ParentGroup;
    }
    
    return string.Join(" → ", path);
}

// Usage: Display full path for each terminal group
private static void DisplayGroupPaths(Group group)
{
    if (group.Records != null && group.Records.Count > 0)
    {
        Console.WriteLine($"Path: {GetGroupPath(group)}");
    }
    else if (group.Groups != null)
    {
        foreach (Group childGroup in group.Groups)
        {
            DisplayGroupPaths(childGroup);
        }
    }
}
```

## Side Effects of Grouping

### Automatic Sorting

**Important:** When you group by a property, the data is automatically sorted by that property as a side effect.

```csharp
// Before grouping: data in random order
// After grouping by property C: data sorted by C values
groupingEngine.TableDescriptor.GroupedColumns.Add("C");

// Records are now sorted by C, even when accessing Table.Records
foreach (Record rec in groupingEngine.Table.Records)
{
    MyObject obj = rec.GetData() as MyObject;
    Console.WriteLine(obj); // Will display in sorted order by C
}
```

### Grouping Order Matters

The order in which you add grouped columns affects the hierarchy:

```csharp
// Option 1: Group by Category, then Region
groupingEngine.TableDescriptor.GroupedColumns.Add("Category");
groupingEngine.TableDescriptor.GroupedColumns.Add("Region");
// Hierarchy: Category → Region

// Option 2: Group by Region, then Category
groupingEngine.TableDescriptor.GroupedColumns.Clear();
groupingEngine.TableDescriptor.GroupedColumns.Add("Region");
groupingEngine.TableDescriptor.GroupedColumns.Add("Category");
// Hierarchy: Region → Category
```

These produce different group structures!

## Common Use Cases

### Use Case 1: Product Categories

```csharp
// Group products by category to analyze inventory
groupingEngine.TableDescriptor.GroupedColumns.Add("Category");

foreach (Group categoryGroup in groupingEngine.Table.TopLevelGroup.Groups)
{
    Console.WriteLine($"\nCategory: {categoryGroup.Name}");
    Console.WriteLine($"Products in this category: {categoryGroup.Records.Count}");
    
    foreach (Record rec in categoryGroup.Records)
    {
        Product product = rec.GetData() as Product;
        Console.WriteLine($"  - {product.Name}: ${product.Price}");
    }
}
```

### Use Case 2: Regional Sales Analysis

```csharp
// Multi-level: Region → Sales Rep → Month
groupingEngine.TableDescriptor.GroupedColumns.Add("Region");
groupingEngine.TableDescriptor.GroupedColumns.Add("SalesRep");
groupingEngine.TableDescriptor.GroupedColumns.Add("Month");

// Navigate to specific region and rep
Group northRegion = groupingEngine.Table.TopLevelGroup.Groups["North"];
if (northRegion != null)
{
    Group johnSmithSales = northRegion.Groups["John Smith"];
    if (johnSmithSales != null)
    {
        foreach (Group monthGroup in johnSmithSales.Groups)
        {
            Console.WriteLine($"Month {monthGroup.Name}: {monthGroup.Records.Count} sales");
        }
    }
}
```

### Use Case 3: Status-Based Workflow

```csharp
// Group orders by status
groupingEngine.TableDescriptor.GroupedColumns.Add("OrderStatus");

// Process each status group differently
foreach (Group statusGroup in groupingEngine.Table.TopLevelGroup.Groups)
{
    switch (statusGroup.Name)
    {
        case "Pending":
            Console.WriteLine($"{statusGroup.Records.Count} orders awaiting processing");
            break;
        case "Shipped":
            Console.WriteLine($"{statusGroup.Records.Count} orders in transit");
            break;
        case "Delivered":
            Console.WriteLine($"{statusGroup.Records.Count} orders completed");
            break;
    }
}
```

## Best Practices

1. **Check collection before iterating**: Always verify `Groups` or `Records` is not null and has items
2. **Use recursion for unknown depth**: Don't hard-code group levels; use recursive patterns
3. **Cache TopLevelGroup**: Store reference to avoid repeated property access
4. **Consider sort order**: Remember grouping automatically sorts data
5. **Name your groups meaningfully**: Use descriptive property names for clarity
6. **Handle empty groups**: Some groups may be empty after filtering

## Next Steps

1. Review [sorting.md](sorting.md) to control sort order within groups
2. See [expressions-and-summaries.md](expressions-and-summaries.md) to add aggregate calculations per group
3. Explore [filtering.md](filtering.md) to filter records before grouping
