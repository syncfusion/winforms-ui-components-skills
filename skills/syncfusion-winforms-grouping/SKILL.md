---
name: syncfusion-winforms-grouping
description: Implements the Syncfusion Grouping Engine for data grouping, sorting, filtering, and summarization in Windows Forms applications. This skill provides comprehensive data manipulation capabilities for managing and displaying tabular data from IList sources. Use when working with Syncfusion.Grouping namespace, Engine class, TableDescriptor, or advanced data organization features in Windows Forms.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion Grouping Engine

Essential Grouping is a 100% native .NET data management framework that provides sorting, grouping, filtering, and summarization capabilities for tabular data without dependencies on any particular UI component. It is designed for use in Windows Forms applications.

## When to Use This Skill

Use this skill when you need to:

- **Sort data** by one or multiple properties with standard or custom comparison logic
- **Group data** hierarchically by properties to create nested groups
- **Filter records** based on complex expressions with algebraic and logical operators
- **Summarize data** with aggregate calculations (sum, average, min, max, count)
- **Add calculated fields** using expression-based properties
- **Manage tabular data** from any IList source (ArrayList, List<T>, BindingList, etc.)
- **Build data-driven applications** requiring flexible data organization
- **Display grouped/summarized information** without UI control dependencies

## Component Overview

The Grouping Engine uses balanced binary trees as the core data structure for efficient data operations. Key benefits include:

- **O(Log n) operations** for insert, remove, and move operations
- **Cached parent information** for quick position and summary lookups
- **Recursive grouping** supporting multiple levels of nested groups
- **Expression support** for calculated fields and filter conditions
- **Platform support** optimized for Windows Forms applications

## Documentation and Navigation Guide

### Getting Started and Installation
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation locations and deployment requirements
- Creating Windows Forms applications
- Required assembly references and dependencies
- Setting up Copy Local properties for deployment
- Deployment scenarios and GAC options
- DLL requirements for Windows Forms deployment

### Data Binding Fundamentals
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- Creating custom objects with public properties
- Building ArrayList and IList datasources
- Setting datasource with Engine.SetSourceList()
- Iterating through Table.Records collection
- Accessing record data with Record.GetData()
- Understanding the Engine and Table relationship

### Grouping Data
📄 **Read:** [references/grouping-data.md](references/grouping-data.md)
- Using TableDescriptor.GroupedColumns collection
- Accessing groups via Table.TopLevelGroup
- Recursive navigation through Groups and Records
- Retrieving specific groups by key value
- Understanding group hierarchy patterns
- Side effects of grouping (automatic sorting)
- Displaying records within groups

### Sorting Data
📄 **Read:** [references/sorting.md](references/sorting.md)
- Adding columns to TableDescriptor.SortedColumns
- Specifying sort direction (Ascending/Descending)
- Implementing custom sorting with IComparer interface
- Using SortColumnDescriptor.Comparer property
- Numerical vs string sorting strategies
- Multi-column sorting scenarios

### Filtering Records
📄 **Read:** [references/filtering.md](references/filtering.md)
- Creating RecordFilterDescriptor with expressions
- Using Table.FilteredRecords collection
- Filter expression syntax and operators
- Logical operators (AND, OR, NOT)
- Comparison operators (=, <, >, <=, >=, <>)
- String operators (LIKE, MATCH, IN, BETWEEN)
- Building complex multi-condition filters
- Clearing and updating filter collections

### Expressions and Summaries
📄 **Read:** [references/expressions-and-summaries.md](references/expressions-and-summaries.md)
- Creating ExpressionFieldDescriptor for calculated fields
- Algebraic expressions with property names
- Adding to TableDescriptor.ExpressionFields collection
- Retrieving expression values with Record.GetValue()
- Creating SummaryDescriptor for aggregations
- Summary types (Int32Aggregate, DoubleAggregate, etc.)
- Getting summary values per group with GetSummary()
- Algebra operators and evaluation precedence
- Custom function support and limitations

## Quick Start Example

```csharp
using System;
using System.Collections;
using Syncfusion.Grouping;

// Define a custom data object
public class SalesRecord
{
    public string Product { get; set; }
    public string Category { get; set; }
    public int Quantity { get; set; }
    public decimal Price { get; set; }
    
    public SalesRecord(string product, string category, int qty, decimal price)
    {
        Product = product;
        Category = category;
        Quantity = qty;
        Price = price;
    }
}

// Create and populate datasource
ArrayList salesData = new ArrayList();
salesData.Add(new SalesRecord("Laptop", "Electronics", 5, 1200.00m));
salesData.Add(new SalesRecord("Mouse", "Electronics", 25, 15.00m));
salesData.Add(new SalesRecord("Desk", "Furniture", 10, 350.00m));
salesData.Add(new SalesRecord("Chair", "Furniture", 20, 150.00m));

// Create Grouping Engine and set datasource
Engine groupingEngine = new Engine();
groupingEngine.SetSourceList(salesData);

// Group by Category
groupingEngine.TableDescriptor.GroupedColumns.Add("Category");

// Add summary for Quantity
SummaryDescriptor quantitySummary = new SummaryDescriptor(
    "TotalQuantity", 
    "Quantity", 
    SummaryType.Int32Aggregate
);
groupingEngine.TableDescriptor.Summaries.Add(quantitySummary);

// Iterate through groups and display summaries
foreach (Group group in groupingEngine.Table.TopLevelGroup.Groups)
{
    Console.WriteLine($"Category: {group.Name}");
    
    // Get summary value
    var summary = group.GetSummary("TotalQuantity") as Int32AggregateSummary;
    Console.WriteLine($"Total Items: {summary.Sum}");
    
    // Display records in this group
    foreach (Record rec in group.Records)
    {
        SalesRecord sale = rec.GetData() as SalesRecord;
        Console.WriteLine($"  {sale.Product}: {sale.Quantity} @ ${sale.Price}");
    }
    Console.WriteLine();
}
```

## Common Patterns

### Pattern 1: Filter-Then-Group Workflow
```csharp
// 1. Create engine and set datasource
Engine engine = new Engine();
engine.SetSourceList(dataList);

// 2. Apply filter first
RecordFilterDescriptor filter = new RecordFilterDescriptor("[Price] > 100");
engine.TableDescriptor.RecordFilters.Add(filter);

// 3. Then group filtered results
engine.TableDescriptor.GroupedColumns.Add("Category");

// 4. Access filtered and grouped data
foreach (Group group in engine.Table.TopLevelGroup.Groups)
{
    // Process each group
}
```

### Pattern 2: Multi-Level Grouping
```csharp
// Group by multiple columns for nested hierarchy
engine.TableDescriptor.GroupedColumns.Add("Region");
engine.TableDescriptor.GroupedColumns.Add("Category");
engine.TableDescriptor.GroupedColumns.Add("SubCategory");

// Navigate nested groups recursively
void ProcessGroup(Group group, int level)
{
    if (group.Records != null && group.Records.Count > 0)
    {
        // Terminal group with records
        foreach (Record rec in group.Records)
        {
            // Process record
        }
    }
    else if (group.Groups != null && group.Groups.Count > 0)
    {
        // Parent group with sub-groups
        foreach (Group subGroup in group.Groups)
        {
            ProcessGroup(subGroup, level + 1);
        }
    }
}
```

### Pattern 3: Dynamic Expression Fields
```csharp
// Add calculated column for total value
ExpressionFieldDescriptor totalExpr = new ExpressionFieldDescriptor(
    "TotalValue", 
    "[Quantity] * [Price]"
);
engine.TableDescriptor.ExpressionFields.Add(totalExpr);

// Use expression in filter
RecordFilterDescriptor highValueFilter = new RecordFilterDescriptor(
    "[TotalValue] > 1000"
);
engine.TableDescriptor.RecordFilters.Add(highValueFilter);

// Retrieve expression values
foreach (Record rec in engine.Table.FilteredRecords)
{
    decimal total = (decimal)rec.GetValue("TotalValue");
    Console.WriteLine($"Total: ${total}");
}
```

### Pattern 4: Custom Sort with IComparer
```csharp
// Implement custom comparer
public class CustomComparer : IComparer
{
    public int Compare(object x, object y)
    {
        // Custom comparison logic
        string str1 = x.ToString();
        string str2 = y.ToString();
        
        // Example: Sort by length first, then alphabetically
        int lengthCompare = str1.Length.CompareTo(str2.Length);
        return lengthCompare != 0 ? lengthCompare : str1.CompareTo(str2);
    }
}

// Apply custom sort
engine.TableDescriptor.SortedColumns.Add("Product");
engine.TableDescriptor.SortedColumns["Product"].Comparer = new CustomComparer();
```

## Key Properties and Collections

### Engine Class
| Property | Description |
|----------|-------------|
| `Table` | Gets the Table object containing records and groups |
| `TableDescriptor` | Gets schema information and configuration |
| `SetSourceList(IList)` | Sets the datasource for the engine |

### TableDescriptor Properties
| Property | Description |
|----------|-------------|
| `GroupedColumns` | Collection of properties to group by |
| `SortedColumns` | Collection of properties to sort by |
| `RecordFilters` | Collection of filter conditions |
| `Summaries` | Collection of summary calculations |
| `ExpressionFields` | Collection of calculated fields |
| `Columns` | Collection of column descriptors (schema info) |

### Table Properties
| Property | Description |
|----------|-------------|
| `Records` | All records in the table |
| `FilteredRecords` | Records after applying filters |
| `TopLevelGroup` | Root group for hierarchical access |

### Group Class
| Property | Description |
|----------|-------------|
| `Groups` | Child groups (for parent groups) |
| `Records` | Records in this group (for terminal groups) |
| `Name` | Group key/name |
| `GetSummary(string)` | Retrieves summary value by name |

### Record Class
| Method | Description |
|--------|-------------|
| `GetData()` | Returns the underlying data object |
| `GetValue(string)` | Gets property or expression value by name |

## Common Use Cases

### Use Case 1: Sales Report by Region and Product
```csharp
// Group sales by region, then product category
engine.TableDescriptor.GroupedColumns.Add("Region");
engine.TableDescriptor.GroupedColumns.Add("ProductCategory");

// Add sales summaries
engine.TableDescriptor.Summaries.Add(
    new SummaryDescriptor("TotalSales", "SalesAmount", SummaryType.DoubleAggregate)
);
engine.TableDescriptor.Summaries.Add(
    new SummaryDescriptor("OrderCount", "OrderID", SummaryType.Count)
);
```

### Use Case 2: Inventory Filtering for Low Stock
```csharp
// Filter for items below reorder level
RecordFilterDescriptor lowStock = new RecordFilterDescriptor(
    "[StockLevel] < [ReorderLevel] AND [Active] = 1"
);
engine.TableDescriptor.RecordFilters.Add(lowStock);

// Sort by urgency (stock level ascending)
engine.TableDescriptor.SortedColumns.Add("StockLevel", ListSortDirection.Ascending);
```

### Use Case 3: Customer Analysis with Calculated Fields
```csharp
// Add expression for customer value
ExpressionFieldDescriptor customerValue = new ExpressionFieldDescriptor(
    "CustomerValue",
    "[TotalOrders] * [AverageOrderAmount]"
);
engine.TableDescriptor.ExpressionFields.Add(customerValue);

// Filter for high-value customers
RecordFilterDescriptor premiumCustomers = new RecordFilterDescriptor(
    "[CustomerValue] > 10000"
);
engine.TableDescriptor.RecordFilters.Add(premiumCustomers);

// Group by customer tier
engine.TableDescriptor.GroupedColumns.Add("Tier");
```

### Use Case 4: Time-Based Data Analysis
```csharp
// Filter for date range using BETWEEN operator
RecordFilterDescriptor dateRange = new RecordFilterDescriptor(
    "[OrderDate] BETWEEN {1/1/2024, 12/31/2024}"
);
engine.TableDescriptor.RecordFilters.Add(dateRange);

// Group by month (requires calculated field)
ExpressionFieldDescriptor monthField = new ExpressionFieldDescriptor(
    "OrderMonth",
    "Month([OrderDate])"
);
engine.TableDescriptor.ExpressionFields.Add(monthField);
engine.TableDescriptor.GroupedColumns.Add("OrderMonth");
```

## Edge Cases and Troubleshooting

### Empty Groups
Groups may be empty after filtering. Always check `Records.Count` before iterating.

### Type Mismatches in Expressions
Property values must match the operation type:
- Use `=` for numeric comparisons
- Use `LIKE` or `MATCH` for string comparisons
- Use appropriate summary types (Int32Aggregate vs DoubleAggregate)

### Recursive Navigation
When navigating groups recursively, check whether `group.Groups` or `group.Records` is populated. Only one will have items at each level.

### Filter Expression Syntax
- Property names must be enclosed in square brackets: `[PropertyName]`
- String literals must be in single quotes: `'value'`
- Spaces in IN operator lists are significant: `{value1,value2}` not `{value1, value2}`

### Memory Management
The Grouping Engine maintains internal data structures. Call `Dispose()` when done to release resources.

## Best Practices

1. **Set datasource before configuration**: Call `SetSourceList()` before adding groups, sorts, or filters
2. **Use FilteredRecords for filtered data**: Access `Table.FilteredRecords` instead of `Table.Records` when filters are active
3. **Choose appropriate summary types**: Match summary type to property data type (Int32, Double, Boolean, etc.)
4. **Optimize expressions**: Complex expressions impact performance; keep them simple when possible
5. **Handle null values**: Check for null when accessing record data with `GetData()`
6. **Dispose properly**: Dispose of Engine objects when no longer needed
7. **Test filter expressions**: Validate filter syntax before deployment to avoid runtime errors

## Platform-Specific Notes

### Windows Forms
- Grouping Engine works standalone or with GridGroupingControl
- No UI dependencies required
- Optimized for desktop application scenarios

## Next Steps

1. Read [getting-started.md](references/getting-started.md) for installation and setup
2. Review [data-binding.md](references/data-binding.md) to understand datasource requirements
3. Explore specific features in respective reference files based on your needs
4. Review code examples in each reference for implementation details
