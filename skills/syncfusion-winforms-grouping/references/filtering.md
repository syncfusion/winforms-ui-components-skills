# Filtering Records in Grouping Engine

## Table of Contents
- [Overview](#overview)
- [Creating Record Filters](#creating-record-filters)
- [Filter Expression Syntax](#filter-expression-syntax)
- [Comparison Operators](#comparison-operators)
- [String Operators](#string-operators)
- [Logical Operators](#logical-operators)
- [Accessing Filtered Data](#accessing-filtered-data)
- [Complex Filter Examples](#complex-filter-examples)
- [Managing Filters](#managing-filters)

## Overview

Filtering allows you to view only records that match specific criteria. The Grouping Engine uses the `RecordFilters` collection in `TableDescriptor` to define filter conditions expressed as algebraic and logical expressions.

Filters are useful when you need to:
- Display only high-value transactions
- Show active items only
- Find records within a date range
- Match text patterns
- Apply business rules to data visibility

## Creating Record Filters

Add filters using `RecordFilterDescriptor` objects:

### Basic Filter Example

```csharp
// Create datasource and engine
ArrayList list = new ArrayList();
Random r = new Random();

for (int i = 0; i < 10; i++)
{
    list.Add(new MyObject(r.Next(5)));
}

Engine groupingEngine = new Engine();
groupingEngine.SetSourceList(list);

// Display data before filtering
foreach (Record rec in groupingEngine.Table.FilteredRecords)
{
    MyObject obj = rec.GetData() as MyObject;
    if (obj != null)
    {
        Console.WriteLine(obj);
    }
}

Console.ReadLine(); // Pause

// Apply filter: Show only records where D = "d1"
RecordFilterDescriptor recordFilterDescriptor = new RecordFilterDescriptor("[D] LIKE 'd1'");
groupingEngine.TableDescriptor.RecordFilters.Add(recordFilterDescriptor);

// Display filtered data
foreach (Record rec in groupingEngine.Table.FilteredRecords)
{
    MyObject obj = rec.GetData() as MyObject;
    if (obj != null)
    {
        Console.WriteLine(obj);
    }
}

Console.ReadLine(); // Pause
```

**VB.NET Version:**

```vb
' Filter on [D] = d1
Dim recordFilterDescriptor As New RecordFilterDescriptor("[D] LIKE 'd1'")
groupingEngine.TableDescriptor.RecordFilters.Add(recordFilterDescriptor)

' Display the data after filtering
Dim rec As Record

For Each rec In groupingEngine.Table.FilteredRecords
    Dim obj As MyObject = CType(rec.GetData(), MyObject)
    
    If Not (obj Is Nothing) Then
        Console.WriteLine(obj)
    End If
Next rec

Console.ReadLine()
```

**Important:** Property names must be enclosed in square brackets: `[PropertyName]`

## Filter Expression Syntax

### Basic Syntax Rules

1. **Property names**: Enclose in square brackets `[PropertyName]`
2. **String literals**: Enclose in single quotes `'value'`
3. **Numbers**: Use directly without quotes `100`, `3.14`
4. **Operators**: Use standard comparison and logical operators
5. **Spaces**: Significant in some operators (especially IN)

### Expression Components

```
[PropertyName] operator value
```

Examples:
```
[Price] > 100
[Status] LIKE 'Active'
[Quantity] = 0
[Category] IN {Electronics,Furniture}
```

## Comparison Operators

Use comparison operators for numeric and string comparisons:

| Operator | Description | Example |
|----------|-------------|---------|
| `=` | Equal to | `[Price] = 100` |
| `<>` | Not equal to | `[Status] <> 'Cancelled'` |
| `>` | Greater than | `[Quantity] > 10` |
| `<` | Less than | `[Quantity] < 5` |
| `>=` | Greater than or equal | `[Price] >= 50.00` |
| `<=` | Less than or equal | `[Price] <= 200.00` |

### Numeric Comparison Examples

```csharp
// Filter for high-value items
RecordFilterDescriptor highValue = new RecordFilterDescriptor("[Price] > 1000");
engine.TableDescriptor.RecordFilters.Add(highValue);

// Filter for zero stock
RecordFilterDescriptor outOfStock = new RecordFilterDescriptor("[StockLevel] = 0");
engine.TableDescriptor.RecordFilters.Add(outOfStock);

// Filter for quantity range
RecordFilterDescriptor qtyRange = new RecordFilterDescriptor("[Quantity] >= 10 AND [Quantity] <= 100");
engine.TableDescriptor.RecordFilters.Add(qtyRange);
```

**Important:** Use `=` operator for numeric properties, not `LIKE`:

```csharp
// ✓ Correct for numeric property B
"[B] = 2"

// ✗ Wrong - LIKE is for strings
"[B] LIKE 2"
```

## String Operators

Special operators for string matching:

### LIKE Operator

Checks if field **starts exactly** as specified. Supports wildcards.

**Exact Match:**
```csharp
// Matches records where CompanyName is exactly "RTR"
"[CompanyName] LIKE 'RTR'"
```

**Wildcard Patterns:**

```csharp
// Starts with "RTR"
"[CompanyName] LIKE 'RTR*'"

// Ends with "RTR"
"[CompanyName] LIKE '*RTR'"

// Contains "RTR" anywhere
"[CompanyName] LIKE '*RTR*'"
```

**Example:**
```csharp
// Filter for products starting with "Pro"
RecordFilterDescriptor productFilter = new RecordFilterDescriptor("[ProductName] LIKE 'Pro*'");
engine.TableDescriptor.RecordFilters.Add(productFilter);
```

### MATCH Operator

Returns true if right-hand argument appears **anywhere** in the left-hand argument.

```csharp
// Matches any CompanyName containing "RTR"
"[CompanyName] MATCH 'RTR'"

// Examples that match:
// "RTR Industries"
// "MyRTRCompany"
// "CompanyRTR"
```

**Example:**
```csharp
// Find all records with "urgent" in description
RecordFilterDescriptor urgentFilter = new RecordFilterDescriptor("[Description] MATCH 'urgent'");
engine.TableDescriptor.RecordFilters.Add(urgentFilter);
```

### IN Operator

Checks if field value is one of the specified values.

**Syntax:** `[Property] IN {value1,value2,value3}`

**Important:** Spaces are significant! `{RTR,MAS}` ≠ `{RTR, MAS}`

```csharp
// Numeric values
"[Code] IN {1,10,21}"

// String values (no quotes needed inside braces)
"[CompanyName] IN {RTR,MAS,ABC}"

// Multiple categories
"[Category] IN {Electronics,Furniture,Clothing}"
```

**Example:**
```csharp
// Filter for specific statuses
RecordFilterDescriptor statusFilter = new RecordFilterDescriptor(
    "[Status] IN {Pending,Processing,Shipped}"
);
engine.TableDescriptor.RecordFilters.Add(statusFilter);
```

### BETWEEN Operator

Checks if value is within a range (typically for dates).

**Syntax:** `[Property] BETWEEN {startValue, endValue}`

```csharp
// Date range
"[OrderDate] BETWEEN {1/1/2024, 12/31/2024}"

// Start date is >= first value
// End date is < second value (exclusive)
```

**Special Tokens:**
- `TODAY`: Current date
- Empty first argument: `DateTime.MinValue`
- Empty second argument: `DateTime.MaxValue`

```csharp
// From specific date to today
"[OrderDate] BETWEEN {6/1/2024, TODAY}"

// All dates up to specific date
"[OrderDate] BETWEEN {, 12/31/2024}"

// All dates from specific date forward
"[OrderDate] BETWEEN {1/1/2024, }"
```

**Example:**
```csharp
// Filter for last quarter
RecordFilterDescriptor quarterFilter = new RecordFilterDescriptor(
    "[OrderDate] BETWEEN {10/1/2024, 12/31/2024}"
);
engine.TableDescriptor.RecordFilters.Add(quarterFilter);
```

## Logical Operators

Combine multiple conditions with logical operators:

| Operator | Description | Example |
|----------|-------------|---------|
| `AND` | Both conditions must be true | `[Price] > 100 AND [Stock] > 0` |
| `OR` | Either condition must be true | `[Status] = 'Urgent' OR [Priority] = 1` |
| `NOT` | Negates condition | `NOT ([Status] = 'Cancelled')` |

**Precedence:** `NOT` > `AND` > `OR`

### AND Operator

Both conditions must be true:

```csharp
// High-value AND in-stock items
"[Price] > 500 AND [StockLevel] > 0"

// Active AND recent
"[IsActive] = 1 AND [CreatedDate] > '1/1/2024'"
```

**Example:**
```csharp
// Filter for available premium products
RecordFilterDescriptor premiumAvailable = new RecordFilterDescriptor(
    "[Category] = 'Premium' AND [InStock] = 1"
);
engine.TableDescriptor.RecordFilters.Add(premiumAvailable);
```

### OR Operator

Either condition can be true:

```csharp
// Property D equals "d1" OR Property B equals 2
"[D] LIKE 'd1' OR [B] = 2"

// Multiple status options
"[Status] = 'Pending' OR [Status] = 'Processing'"
```

**Example:**
```csharp
// Filter for priority items (urgent or high-value)
RecordFilterDescriptor priorityFilter = new RecordFilterDescriptor(
    "[Status] = 'Urgent' OR [Price] > 10000"
);
engine.TableDescriptor.RecordFilters.Add(priorityFilter);
```

### Complex Logical Expressions

```csharp
// Combine multiple operators
"([Category] = 'Electronics' OR [Category] = 'Computers') AND [Price] < 1000"

// Using NOT
"[Status] <> 'Cancelled' AND NOT ([Quantity] = 0)"

// Multiple conditions
"[Region] IN {North,East} AND ([Sales] > 100000 OR [GrowthRate] > 0.15)"
```

## Accessing Filtered Data

### Using FilteredRecords Collection

**Always use `Table.FilteredRecords`** when filters are applied, not `Table.Records`.

```csharp
// ✓ Correct - respects filters
foreach (Record rec in engine.Table.FilteredRecords)
{
    MyObject obj = rec.GetData() as MyObject;
    // Process filtered records
}

// ✗ Wrong - ignores filters
foreach (Record rec in engine.Table.Records)
{
    // This shows ALL records, not filtered ones
}
```

### Checking Filter Results

```csharp
// Count before filtering
int totalRecords = engine.Table.Records.Count;

// Apply filter
RecordFilterDescriptor filter = new RecordFilterDescriptor("[Status] = 'Active'");
engine.TableDescriptor.RecordFilters.Add(filter);

// Count after filtering
int filteredRecords = engine.Table.FilteredRecords.Count;

Console.WriteLine($"Showing {filteredRecords} of {totalRecords} records");
```

## Complex Filter Examples

### Example 1: Multi-Condition Product Filter

```csharp
// Show electronics OR furniture, priced between $50-$500, in stock
RecordFilterDescriptor complexFilter = new RecordFilterDescriptor(
    "([Category] = 'Electronics' OR [Category] = 'Furniture') " +
    "AND [Price] >= 50 AND [Price] <= 500 " +
    "AND [StockLevel] > 0"
);
engine.TableDescriptor.RecordFilters.Add(complexFilter);
```

### Example 2: Sales Analysis Filter

```csharp
// High-value sales in specific regions during Q1
RecordFilterDescriptor salesFilter = new RecordFilterDescriptor(
    "[Region] IN {North,East,West} " +
    "AND [SalesAmount] > 5000 " +
    "AND [OrderDate] BETWEEN {1/1/2024, 3/31/2024}"
);
engine.TableDescriptor.RecordFilters.Add(salesFilter);
```

### Example 3: Customer Segmentation

```csharp
// Premium customers: high lifetime value OR frequent buyers
RecordFilterDescriptor premiumCustomers = new RecordFilterDescriptor(
    "([LifetimeValue] > 10000 OR [OrderCount] > 50) " +
    "AND [Status] = 'Active' " +
    "AND NOT ([RiskLevel] = 'High')"
);
engine.TableDescriptor.RecordFilters.Add(premiumCustomers);
```

### Example 4: Inventory Alert Filter

```csharp
// Low stock items that need reordering
RecordFilterDescriptor reorderAlert = new RecordFilterDescriptor(
    "[StockLevel] < [ReorderLevel] " +
    "AND [IsActive] = 1 " +
    "AND [Supplier] MATCH 'Approved'"
);
engine.TableDescriptor.RecordFilters.Add(reorderAlert);
```

### Example 5: Text Search Filter

```csharp
// Search across multiple text fields
string searchTerm = "laptop";
RecordFilterDescriptor searchFilter = new RecordFilterDescriptor(
    $"[ProductName] MATCH '{searchTerm}' " +
    $"OR [Description] MATCH '{searchTerm}' " +
    $"OR [Category] MATCH '{searchTerm}'"
);
engine.TableDescriptor.RecordFilters.Add(searchFilter);
```

## Managing Filters

### Adding Multiple Filters

```csharp
// All filters are ANDed together
RecordFilterDescriptor filter1 = new RecordFilterDescriptor("[Price] > 100");
RecordFilterDescriptor filter2 = new RecordFilterDescriptor("[Category] = 'Electronics'");

engine.TableDescriptor.RecordFilters.Add(filter1);
engine.TableDescriptor.RecordFilters.Add(filter2);

// Equivalent to: [Price] > 100 AND [Category] = 'Electronics'
```

### Removing Filters

```csharp
// Remove specific filter
engine.TableDescriptor.RecordFilters.Remove(filter1);

// Remove all filters
engine.TableDescriptor.RecordFilters.Clear();
```

### Updating Filters

```csharp
// Clear existing filters
engine.TableDescriptor.RecordFilters.Clear();

// Add new filter
RecordFilterDescriptor newFilter = new RecordFilterDescriptor("[Status] = 'Active'");
engine.TableDescriptor.RecordFilters.Add(newFilter);

// FilteredRecords collection automatically updates
```

### Conditional Filtering

```csharp
public void ApplyFilter(Engine engine, string filterType, object filterValue)
{
    engine.TableDescriptor.RecordFilters.Clear();
    
    RecordFilterDescriptor filter = null;
    
    switch (filterType)
    {
        case "PriceRange":
            filter = new RecordFilterDescriptor($"[Price] >= {filterValue}");
            break;
        
        case "Category":
            filter = new RecordFilterDescriptor($"[Category] = '{filterValue}'");
            break;
        
        case "InStock":
            filter = new RecordFilterDescriptor("[StockLevel] > 0");
            break;
        
        case "Active":
            filter = new RecordFilterDescriptor("[Status] = 'Active'");
            break;
    }
    
    if (filter != null)
    {
        engine.TableDescriptor.RecordFilters.Add(filter);
    }
}
```

## Best Practices

1. **Use appropriate operators**: Numeric properties use `=`, strings use `LIKE` or `MATCH`
2. **Mind the spaces**: Spaces in IN operator lists are significant
3. **Property name casing**: Property names are case-sensitive
4. **Use FilteredRecords**: Always iterate through `FilteredRecords` when filters are active
5. **Clear before reapplying**: Call `RecordFilters.Clear()` when changing filter criteria
6. **Test expressions**: Validate filter expressions during development
7. **Escape special characters**: Be careful with single quotes in string literals
8. **Combine with grouping**: Apply filters before grouping to filter grouped results

## Common Issues

### Issue: Filter Not Working

**Symptom:** All records still displayed

**Causes:**
- Using `Table.Records` instead of `Table.FilteredRecords`
- Property name misspelled or wrong case
- Wrong operator for data type

**Solutions:**
- Use `Table.FilteredRecords` collection
- Verify property name matches exactly
- Use `=` for numbers, `LIKE`/`MATCH` for strings

### Issue: Expression Syntax Error

**Symptom:** Exception when adding filter

**Causes:**
- Property name not in brackets
- String literal not in single quotes
- Invalid operator
- Unbalanced parentheses

**Solutions:**
- Enclose property names: `[PropertyName]`
- Enclose string literals: `'value'`
- Check operator spelling: `LIKE`, `MATCH`, `IN`, `BETWEEN`
- Balance all parentheses in complex expressions

### Issue: IN Operator Not Matching

**Symptom:** IN operator returns no results

**Cause:** Extra spaces in value list

**Solution:**
```csharp
// ✓ Correct (no spaces)
"[Status] IN {Active,Pending,Processing}"

// ✗ Wrong (spaces after commas)
"[Status] IN {Active, Pending, Processing}"
```

### Issue: Date Filter Not Working

**Symptom:** BETWEEN filter returns wrong records

**Causes:**
- Date format not recognized
- Time component included

**Solutions:**
- Use consistent date format: `MM/DD/YYYY`
- Use `TODAY` token for current date
- Remember end date is exclusive

## Next Steps

1. Review [expressions-and-summaries.md](expressions-and-summaries.md) to use calculated fields in filters
2. See [grouping-data.md](grouping-data.md) to apply filters before grouping
3. Explore [sorting.md](sorting.md) to sort filtered results
