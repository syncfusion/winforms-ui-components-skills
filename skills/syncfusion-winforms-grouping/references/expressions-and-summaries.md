# Expressions and Summaries in Grouping Engine

## Table of Contents
- [Overview](#overview)
- [Expression Fields](#expression-fields)
- [Creating Expression Fields](#creating-expression-fields)
- [Algebra and Operators](#algebra-and-operators)
- [Using Expressions in Filters](#using-expressions-in-filters)
- [Summary Descriptors](#summary-descriptors)
- [Summary Types](#summary-types)
- [Retrieving Summary Values](#retrieving-summary-values)
- [Combining Expressions and Summaries](#combining-expressions-and-summaries)
- [Custom Functions](#custom-functions)

## Overview

The Grouping Engine supports two powerful data manipulation features:

1. **Expression Fields**: Add calculated properties to data using algebraic expressions
2. **Summaries**: Compute aggregate values (sum, average, min, max, count) per group

These features enable dynamic data analysis without modifying source objects.

## Expression Fields

Expression fields are calculated properties that don't exist in the original data. They're computed on-the-fly using algebraic expressions based on existing properties.

### Use Cases for Expressions
- Calculate totals: `[Quantity] * [Price]`
- Compute percentages: `([Sold] / [Total]) * 100`
- Apply formulas: `2.1 * [Value] + 3.2`
- Derive values: `[FirstName] + ' ' + [LastName]` (with limitations)

## Creating Expression Fields

Add expression fields to `TableDescriptor.ExpressionFields` collection:

### Basic Expression Example

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

// Display original data
foreach (Record rec in groupingEngine.Table.FilteredRecords)
{
    MyObject obj = rec.GetData() as MyObject;
    if (obj != null)
    {
        Console.WriteLine(obj);
    }
}

Console.ReadLine(); // Pause

// Add expression field: 2.1 * B + 3.2
ExpressionFieldDescriptor expressionFieldDescriptor = new ExpressionFieldDescriptor(
    "MultipleOfB",           // Expression name
    "2.1 * [B] + 3.2"       // Expression formula
);
groupingEngine.TableDescriptor.ExpressionFields.Add(expressionFieldDescriptor);

// Display expression values
foreach (Record rec in groupingEngine.Table.FilteredRecords)
{
    Console.WriteLine(rec.GetValue("MultipleOfB"));
}

Console.ReadLine(); // Pause
```

**VB.NET Version:**

```vb
' Add an expression property
Dim expressionFieldDescriptor As New ExpressionFieldDescriptor("MultipleOfB", "2.1 * [B] + 3.2")
groupingEngine.TableDescriptor.ExpressionFields.Add(expressionFieldDescriptor)

' Display the data after adding the field
Dim rec As Record

For Each rec In groupingEngine.Table.FilteredRecords
    Console.WriteLine(rec.GetValue("MultipleOfB"))
Next rec

Console.ReadLine()
```

### Real-World Expression Examples

**Calculate Total Value:**

```csharp
ExpressionFieldDescriptor totalValue = new ExpressionFieldDescriptor(
    "TotalValue",
    "[Quantity] * [UnitPrice]"
);
engine.TableDescriptor.ExpressionFields.Add(totalValue);

// Retrieve calculated value
foreach (Record rec in engine.Table.Records)
{
    decimal total = Convert.ToDecimal(rec.GetValue("TotalValue"));
    Console.WriteLine($"Total: ${total:F2}");
}
```

**Calculate Discount Amount:**

```csharp
ExpressionFieldDescriptor discountAmount = new ExpressionFieldDescriptor(
    "DiscountAmount",
    "[Price] * [DiscountPercent] / 100"
);
engine.TableDescriptor.ExpressionFields.Add(discountAmount);
```

**Calculate Final Price:**

```csharp
ExpressionFieldDescriptor finalPrice = new ExpressionFieldDescriptor(
    "FinalPrice",
    "[Price] - ([Price] * [DiscountPercent] / 100)"
);
engine.TableDescriptor.ExpressionFields.Add(finalPrice);
```

**Calculate Profit Margin:**

```csharp
ExpressionFieldDescriptor profitMargin = new ExpressionFieldDescriptor(
    "ProfitMargin",
    "(([SellingPrice] - [Cost]) / [SellingPrice]) * 100"
);
engine.TableDescriptor.ExpressionFields.Add(profitMargin);
```

## Algebra and Operators

### Supported Operators

Operators are evaluated in precedence order:

**Level 1 (Highest):** `*`, `/` - Multiplication, Division  
**Level 2:** `+`, `-` - Addition, Subtraction  
**Level 3:** `<`, `>`, `=`, `<=`, `>=`, `<>` - Comparison  
**Level 4:** `MATCH`, `LIKE`, `IN`, `BETWEEN` - String/Range operators  
**Level 5 (Lowest):** `OR`, `AND`, `NOT` - Logical operators

### Arithmetic Operators

```csharp
// Multiplication and division
"[Quantity] * [Price]"
"[TotalCost] / [ItemCount]"

// Addition and subtraction
"[BasePrice] + [Tax]"
"[Revenue] - [Cost]"

// Combined operations (respects precedence)
"[BasePrice] * [Quantity] + [ShippingCost]"
// Equivalent to: ([BasePrice] * [Quantity]) + [ShippingCost]

// Override precedence with parentheses
"([BasePrice] + [ShippingCost]) * [Quantity]"
```

### Comparison Operators

Comparison operators return `1` for true, `0` for false:

```csharp
// Less than
"[StockLevel] < [ReorderLevel]"  // Returns 1 if below reorder level, 0 otherwise

// Greater than or equal
"[SalesAmount] >= 1000"

// Equal
"[Status] = 'Active'"

// Not equal
"[Category] <> 'Discontinued'"
```

### Logical Operators

Logical operators also return `1` (true) or `0` (false):

```csharp
// AND operator
"[InStock] = 1 AND [Price] < 100"

// OR operator
"[Priority] = 'High' OR [Amount] > 10000"

// NOT operator
"NOT ([Status] = 'Cancelled')"
```

### Expression Components

**Property Names:**
- Must be enclosed in square brackets: `[PropertyName]`
- Case-sensitive
- Can reference any public property

**Numerical Constants:**
- Integer: `10`, `100`, `1000`
- Decimal: `3.14`, `0.5`, `99.99`
- Negative: `-5`, `-10.5`

**String Literals:**
- Enclose in single quotes: `'Active'`, `'Electronics'`
- Used with comparison and string operators

## Using Expressions in Filters

Expression fields can be used in filter conditions:

### Filter by Calculated Value

```csharp
// Create expression for total value
ExpressionFieldDescriptor totalExpr = new ExpressionFieldDescriptor(
    "TotalValue",
    "[Quantity] * [Price]"
);
engine.TableDescriptor.ExpressionFields.Add(totalExpr);

// Filter for high-value orders (total > $1000)
RecordFilterDescriptor highValueFilter = new RecordFilterDescriptor(
    "[TotalValue] > 1000"
);
engine.TableDescriptor.RecordFilters.Add(highValueFilter);

// Display filtered results
foreach (Record rec in engine.Table.FilteredRecords)
{
    decimal total = Convert.ToDecimal(rec.GetValue("TotalValue"));
    Console.WriteLine($"High-value order: ${total:F2}");
}
```

### Complex Expression Filters

```csharp
// Calculate profit margin
ExpressionFieldDescriptor marginExpr = new ExpressionFieldDescriptor(
    "Margin",
    "(([Price] - [Cost]) / [Price]) * 100"
);
engine.TableDescriptor.ExpressionFields.Add(marginExpr);

// Filter for low-margin products (<20%)
RecordFilterDescriptor lowMarginFilter = new RecordFilterDescriptor(
    "[Margin] < 20"
);
engine.TableDescriptor.RecordFilters.Add(lowMarginFilter);
```

## Summary Descriptors

Summaries compute aggregate calculations for groups of records.

### Creating Summary Descriptors

```csharp
// Create summary for property B
SummaryDescriptor sdBInt32Agg = new SummaryDescriptor(
    "BInt32Agg",              // Summary name
    "B",                       // Property to summarize
    SummaryType.Int32Aggregate // Summary type
);

// Add to Summaries collection
groupingEngine.TableDescriptor.Summaries.Add(sdBInt32Agg);
```

**VB.NET Version:**

```vb
' Create a summary that computes the Int32Aggregate calculations on property B
Dim sdBInt32Agg As New SummaryDescriptor("BInt32Agg", "B", SummaryType.Int32Aggregate)

' Add this summary to the Summaries collection
groupingEngine.TableDescriptor.Summaries.Add(sdBInt32Agg)
```

### Summary Constructor Overloads

```csharp
// Overload 1: Name, Property, Type
SummaryDescriptor summary1 = new SummaryDescriptor(
    "TotalSales",
    "SalesAmount",
    SummaryType.DoubleAggregate
);

// Overload 2: Name, Expression, Type
SummaryDescriptor summary2 = new SummaryDescriptor(
    "TotalRevenue",
    "[Quantity] * [Price]",  // Can summarize expressions!
    SummaryType.DoubleAggregate
);
```

## Summary Types

Choose the appropriate summary type based on the property data type:

### Int32Aggregate

For integer properties:

```csharp
SummaryDescriptor intSummary = new SummaryDescriptor(
    "QuantityStats",
    "Quantity",
    SummaryType.Int32Aggregate
);
engine.TableDescriptor.Summaries.Add(intSummary);
```

**Available values:**
- `Sum`: Total of all values
- `Average`: Mean value
- `Minimum`: Lowest value
- `Maximum`: Highest value
- `Count`: Number of records

### DoubleAggregate

For decimal/floating-point properties:

```csharp
SummaryDescriptor doubleSummary = new SummaryDescriptor(
    "PriceStats",
    "Price",
    SummaryType.DoubleAggregate
);
engine.TableDescriptor.Summaries.Add(doubleSummary);
```

**Available values:**
- `Sum`: Total of all values
- `Average`: Mean value
- `Minimum`: Lowest value
- `Maximum`: Highest value
- `Count`: Number of records

### BooleanAggregate

For boolean properties:

```csharp
SummaryDescriptor boolSummary = new SummaryDescriptor(
    "ActiveCount",
    "IsActive",
    SummaryType.BooleanAggregate
);
engine.TableDescriptor.Summaries.Add(boolSummary);
```

**Available values:**
- `TrueCount`: Number of true values
- `FalseCount`: Number of false values
- `Count`: Total count

### Count

Simple count of records:

```csharp
SummaryDescriptor countSummary = new SummaryDescriptor(
    "RecordCount",
    "AnyProperty",  // Property doesn't matter for Count
    SummaryType.Count
);
engine.TableDescriptor.Summaries.Add(countSummary);
```

## Retrieving Summary Values

Access summary values using `Group.GetSummary()`:

### Basic Summary Retrieval

```csharp
using ISummary = Syncfusion.Collections.BinaryTree.ITreeTableSummary;

// Get summary for entire table (TopLevelGroup)
ISummary groupSummary = groupingEngine.Table.TopLevelGroup.GetSummary("BInt32Agg");
Int32AggregateSummary int32Summary = (Int32AggregateSummary)groupSummary;

Console.WriteLine($"Whole table - Sum: {int32Summary.Sum}, Avg: {int32Summary.Average}, Max: {int32Summary.Maximum}");

// Get summary for specific group
Group c1Group = groupingEngine.Table.TopLevelGroup.Groups["c1"];
groupSummary = c1Group.GetSummary("BInt32Agg");
int32Summary = (Int32AggregateSummary)groupSummary;

Console.WriteLine($"c1-group - Sum: {int32Summary.Sum}, Avg: {int32Summary.Average}, Max: {int32Summary.Maximum}");
```

**VB.NET Version:**

```vb
' Get Summary value for the whole table
Dim groupSummary As Syncfusion.Collections.BinaryTree.ITreeTableSummary = groupingEngine.Table.TopLevelGroup.GetSummary("BInt32Agg")
Dim int32Summary As Int32AggregateSummary = CType(groupSummary, Int32AggregateSummary)
Console.WriteLine("whole table {0}, {1}, {2}", int32Summary.Sum, int32Summary.Average, int32Summary.Maximum)

' Value for "c1" group
groupSummary = groupingEngine.Table.TopLevelGroup.Groups("c1").GetSummary("BInt32Agg")
int32Summary = CType(groupSummary, Int32AggregateSummary)
Console.WriteLine("c1-group {0}, {1}, {2}", int32Summary.Sum, int32Summary.Average, int32Summary.Maximum)
```

### Int32AggregateSummary Properties

```csharp
Int32AggregateSummary summary = (Int32AggregateSummary)group.GetSummary("SummaryName");

int sum = summary.Sum;          // Total
double avg = summary.Average;   // Mean
int min = summary.Minimum;      // Lowest value
int max = summary.Maximum;      // Highest value
int count = summary.Count;      // Number of values
```

### DoubleAggregateSummary Properties

```csharp
DoubleAggregateSummary summary = (DoubleAggregateSummary)group.GetSummary("SummaryName");

double sum = summary.Sum;       // Total
double avg = summary.Average;   // Mean
double min = summary.Minimum;   // Lowest value
double max = summary.Maximum;   // Highest value
int count = summary.Count;      // Number of values
```

### Real-World Summary Example

```csharp
// Group sales by region
engine.TableDescriptor.GroupedColumns.Add("Region");

// Add sales summary
SummaryDescriptor salesSummary = new SummaryDescriptor(
    "SalesStats",
    "SalesAmount",
    SummaryType.DoubleAggregate
);
engine.TableDescriptor.Summaries.Add(salesSummary);

// Display summary for each region
foreach (Group regionGroup in engine.Table.TopLevelGroup.Groups)
{
    DoubleAggregateSummary stats = (DoubleAggregateSummary)regionGroup.GetSummary("SalesStats");
    
    Console.WriteLine($"\nRegion: {regionGroup.Name}");
    Console.WriteLine($"  Total Sales: ${stats.Sum:F2}");
    Console.WriteLine($"  Average: ${stats.Average:F2}");
    Console.WriteLine($"  Highest: ${stats.Maximum:F2}");
    Console.WriteLine($"  Count: {stats.Count} orders");
}
```

## Combining Expressions and Summaries

Use expressions to create calculated values, then summarize them:

### Example: Revenue Analysis

```csharp
// Step 1: Create expression for order value
ExpressionFieldDescriptor orderValue = new ExpressionFieldDescriptor(
    "OrderValue",
    "[Quantity] * [UnitPrice]"
);
engine.TableDescriptor.ExpressionFields.Add(orderValue);

// Step 2: Group by customer
engine.TableDescriptor.GroupedColumns.Add("CustomerName");

// Step 3: Summarize the expression
SummaryDescriptor revenueSummary = new SummaryDescriptor(
    "RevenueStats",
    "OrderValue",  // Summarize the expression field!
    SummaryType.DoubleAggregate
);
engine.TableDescriptor.Summaries.Add(revenueSummary);

// Step 4: Display results
foreach (Group customerGroup in engine.Table.TopLevelGroup.Groups)
{
    DoubleAggregateSummary stats = (DoubleAggregateSummary)customerGroup.GetSummary("RevenueStats");
    
    Console.WriteLine($"{customerGroup.Name}: Total Revenue = ${stats.Sum:F2}, Avg Order = ${stats.Average:F2}");
}
```

### Example: Performance Metrics

```csharp
// Calculate conversion rate expression
ExpressionFieldDescriptor conversionRate = new ExpressionFieldDescriptor(
    "ConversionRate",
    "([Conversions] / [Visits]) * 100"
);
engine.TableDescriptor.ExpressionFields.Add(conversionRate);

// Group by campaign
engine.TableDescriptor.GroupedColumns.Add("Campaign");

// Summarize visits and conversions
engine.TableDescriptor.Summaries.Add(
    new SummaryDescriptor("VisitStats", "Visits", SummaryType.Int32Aggregate)
);
engine.TableDescriptor.Summaries.Add(
    new SummaryDescriptor("ConversionStats", "Conversions", SummaryType.Int32Aggregate)
);

// Calculate overall conversion rate per campaign
foreach (Group campaign in engine.Table.TopLevelGroup.Groups)
{
    Int32AggregateSummary visits = (Int32AggregateSummary)campaign.GetSummary("VisitStats");
    Int32AggregateSummary conversions = (Int32AggregateSummary)campaign.GetSummary("ConversionStats");
    
    double convRate = (double)conversions.Sum / visits.Sum * 100;
    
    Console.WriteLine($"{campaign.Name}: {visits.Sum} visits, {conversions.Sum} conversions ({convRate:F1}%)");
}
```

## Custom Functions

### Limitations

Custom functions have the following restrictions:

1. **Cannot be used in algebraic expressions**: Functions must stand alone
2. **Simple expression only**: Function call with arguments, no operators
3. **Must be registered**: Add to the engine's function library
4. **Argument list**: Can have any number of arguments separated by delimiters

### Custom Function Example

```csharp
// Custom function signature (simplified)
public class CustomFunction
{
    public object Evaluate(params object[] arguments)
    {
        // Custom logic here
        return result;
    }
}

// Register function (pseudo-code, actual implementation varies)
// engine.RegisterFunction("MyFunction", new CustomFunction());

// Use in expression (stand-alone only)
// "[MyFunction(arg1, arg2)]"  // ✓ Valid
// "[MyFunction(arg1)] + 10"   // ✗ Invalid - cannot combine with operators
```

**Note:** Custom functions are an advanced feature with limited documentation. For most scenarios, use built-in algebraic expressions.

## Best Practices

1. **Choose appropriate summary types**: Match type to property data type (Int32, Double, Boolean)
2. **Name expressions clearly**: Use descriptive names like "TotalValue", "ProfitMargin"
3. **Test expressions**: Validate expression syntax with sample data
4. **Cache summary results**: Store retrieved summary values if used multiple times
5. **Use parentheses**: Clarify precedence in complex expressions
6. **Expression performance**: Complex expressions impact performance; keep them simple
7. **Null handling**: Expressions with null values may produce unexpected results
8. **Type safety**: Cast summary results to appropriate types (Int32AggregateSummary, etc.)

## Common Issues

### Issue: Expression Returns Wrong Values

**Symptom:** Calculated values incorrect

**Causes:**
- Operator precedence misunderstood
- Property name typo or wrong case
- Type mismatch (string vs number)

**Solutions:**
- Use parentheses to control precedence
- Verify property names exactly
- Ensure properties contain numeric data for arithmetic

### Issue: Summary Type Mismatch

**Symptom:** Invalid cast exception when retrieving summary

**Cause:** Using wrong summary type for property data type

**Solution:**
```csharp
// ✓ Correct: Int property → Int32Aggregate
new SummaryDescriptor("QtySum", "Quantity", SummaryType.Int32Aggregate);

// ✗ Wrong: Decimal property → Int32Aggregate
new SummaryDescriptor("PriceSum", "Price", SummaryType.Int32Aggregate);  // Should be DoubleAggregate
```

### Issue: Expression Not Found in Filter

**Symptom:** Filter using expression field fails

**Cause:** Expression field not added before filter

**Solution:** Always add expression fields before creating filters that reference them:

```csharp
// ✓ Correct order
engine.TableDescriptor.ExpressionFields.Add(expressionField);
engine.TableDescriptor.RecordFilters.Add(filterUsingExpression);

// ✗ Wrong order
engine.TableDescriptor.RecordFilters.Add(filterUsingExpression);  // Expression doesn't exist yet!
engine.TableDescriptor.ExpressionFields.Add(expressionField);
```

## Next Steps

1. Review [filtering.md](filtering.md) to use expressions in filter conditions
2. See [grouping-data.md](grouping-data.md) to understand group-level summaries
3. Explore [data-binding.md](data-binding.md) for property types suitable for expressions
