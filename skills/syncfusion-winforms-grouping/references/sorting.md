# Sorting Data in Grouping Engine

## Table of Contents
- [Overview](#overview)
- [Basic Sorting](#basic-sorting)
- [Sort Direction](#sort-direction)
- [Custom Sorting with IComparer](#custom-sorting-with-icomparer)
- [Multi-Column Sorting](#multi-column-sorting)
- [SortColumnDescriptor Class](#sortcolumndescriptor-class)
- [Common Sorting Patterns](#common-sorting-patterns)

## Overview

The Grouping Engine provides flexible sorting capabilities through the `TableDescriptor.SortedColumns` collection. You can sort by one or multiple properties with standard or custom comparison logic.

Sorting data helps organize records in a meaningful order, such as alphabetically by name, numerically by value, or chronologically by date.

## Basic Sorting

To sort data by a property, add the property name to the `SortedColumns` collection:

### Simple Sort Example

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

// Display data before sorting
foreach (Record rec in groupingEngine.Table.Records)
{
    MyObject obj = rec.GetData() as MyObject;
    if (obj != null)
    {
        Console.WriteLine(obj);
    }
}

Console.ReadLine(); // Pause

// Sort by property A
groupingEngine.TableDescriptor.SortedColumns.Add("A");

// Display data after sorting
foreach (Record rec in groupingEngine.Table.Records)
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
' Sort column A
groupingEngine.TableDescriptor.SortedColumns.Add("A")

' Display the data after sorting
Dim rec As Record

For Each rec In groupingEngine.Table.Records
    Dim obj As MyObject = CType(rec.GetData(), MyObject)
    
    If Not (obj Is Nothing) Then
        Console.WriteLine(obj)
    End If
Next rec

Console.ReadLine()
```

**Result:** Data is sorted alphabetically by property A in ascending order (default).

## Sort Direction

Control whether sorting is ascending or descending using `ListSortDirection`:

### Ascending vs Descending

```csharp
using System.ComponentModel;

// Ascending sort (default)
groupingEngine.TableDescriptor.SortedColumns.Add("Price", ListSortDirection.Ascending);

// Descending sort
groupingEngine.TableDescriptor.SortedColumns.Add("Price", ListSortDirection.Descending);
```

### Full Example with Direction

```csharp
using System.ComponentModel;

// Create sales records
ArrayList salesData = new ArrayList();
salesData.Add(new SalesRecord("Product A", 150.00m));
salesData.Add(new SalesRecord("Product B", 75.00m));
salesData.Add(new SalesRecord("Product C", 200.00m));
salesData.Add(new SalesRecord("Product D", 50.00m));

Engine engine = new Engine();
engine.SetSourceList(salesData);

// Sort by price descending (highest first)
engine.TableDescriptor.SortedColumns.Add("Price", ListSortDirection.Descending);

// Display sorted results
foreach (Record rec in engine.Table.Records)
{
    SalesRecord sale = rec.GetData() as SalesRecord;
    Console.WriteLine($"{sale.ProductName}: ${sale.Price}");
}
// Output: Product C: $200, Product A: $150, Product B: $75, Product D: $50
```

## Custom Sorting with IComparer

For specialized sorting logic, implement the `IComparer` interface and assign it to a column's `Comparer` property.

### Implementing IComparer

```csharp
using System.Collections;

public class AComparer : IComparer
{
    // Custom comparison logic
    public int Compare(object x, object y)
    {
        if (x == null && y == null)
            return 0;
        else if (x == null)
            return -1;
        else if (y == null)
            return 1;
        else
        {
            try
            {
                // Ignore leading character for numerical sorting
                // Example: "a10" vs "a2" → extracts 10 vs 2
                int value1 = int.Parse(x.ToString().Substring(1));
                int value2 = int.Parse(y.ToString().Substring(1));
                
                // Descending order (larger first)
                return value2 - value1;
            }
            catch
            {
                throw new ArgumentException("Value must be in format 'aX' where X is a number");
            }
        }
    }
}
```

**VB.NET Version:**

```vb
Public Class AComparer
    Implements IComparer
    
    Public Function Compare(ByVal x As Object, ByVal y As Object) As Integer _
        Implements IComparer.Compare
        
        If x Is Nothing And y Is Nothing Then
            Return 0
        ElseIf x Is Nothing Then
            Return -1
        ElseIf y Is Nothing Then
            Return 1
        Else
            Dim value1 As Integer = 0
            Dim value2 As Integer = 0
            
            Try
                ' Ignore leading character for numerical sorting
                value1 = Integer.Parse(x.ToString().Substring(1))
                value2 = Integer.Parse(y.ToString().Substring(1))
                Return value2 - value1
            Catch
                Throw New ArgumentException("Value must be in format 'aX'")
            End Try
        End If
    End Function
End Class
```

### Using Custom Comparer

```csharp
// Create data with values like a1, a2, a10, a20
ArrayList list = new ArrayList();
Random r = new Random();

for (int i = 0; i < 10; i++)
{
    list.Add(new MyObject(r.Next(20)));
}

Engine groupingEngine = new Engine();
groupingEngine.SetSourceList(list);

// Apply custom sort
AComparer comparer = new AComparer();
groupingEngine.TableDescriptor.SortedColumns.Add("A");
groupingEngine.TableDescriptor.SortedColumns["A"].Comparer = comparer;

// Display sorted data
foreach (Record rec in groupingEngine.Table.Records)
{
    MyObject obj = rec.GetData() as MyObject;
    if (obj != null)
    {
        Console.WriteLine(obj);
    }
}
```

**Why Custom Sorting?**

Without custom comparer:
- String sort: a1, a10, a11, a2, a20, a3 (alphabetical)

With custom comparer:
- Numerical sort: a20, a11, a10, a3, a2, a1 (by numeric value)

### Real-World Custom Comparer Examples

**Case-Insensitive Sort:**

```csharp
public class CaseInsensitiveComparer : IComparer
{
    public int Compare(object x, object y)
    {
        string str1 = x?.ToString() ?? "";
        string str2 = y?.ToString() ?? "";
        
        return string.Compare(str1, str2, StringComparison.OrdinalIgnoreCase);
    }
}
```

**Custom Date Sort (Null Dates Last):**

```csharp
public class DateComparer : IComparer
{
    public int Compare(object x, object y)
    {
        DateTime? date1 = x as DateTime?;
        DateTime? date2 = y as DateTime?;
        
        if (!date1.HasValue && !date2.HasValue)
            return 0;
        if (!date1.HasValue)
            return 1;  // Null dates go last
        if (!date2.HasValue)
            return -1;
        
        return DateTime.Compare(date1.Value, date2.Value);
    }
}
```

**Priority-Based Sort:**

```csharp
public class PriorityComparer : IComparer
{
    private Dictionary<string, int> priorities = new Dictionary<string, int>
    {
        { "Urgent", 1 },
        { "High", 2 },
        { "Medium", 3 },
        { "Low", 4 }
    };
    
    public int Compare(object x, object y)
    {
        string priority1 = x?.ToString() ?? "Low";
        string priority2 = y?.ToString() ?? "Low";
        
        int value1 = priorities.ContainsKey(priority1) ? priorities[priority1] : 999;
        int value2 = priorities.ContainsKey(priority2) ? priorities[priority2] : 999;
        
        return value1.CompareTo(value2);
    }
}
```

## Multi-Column Sorting

Sort by multiple columns by adding them in order of priority:

```csharp
// Primary sort by Category, secondary sort by Price
engine.TableDescriptor.SortedColumns.Add("Category", ListSortDirection.Ascending);
engine.TableDescriptor.SortedColumns.Add("Price", ListSortDirection.Descending);
```

The first column added has highest priority. Within each category, items are sorted by price descending.

### Multi-Column Example

```csharp
public class Employee
{
    public string Department { get; set; }
    public string Name { get; set; }
    public int Salary { get; set; }
}

// Create employee data
ArrayList employees = new ArrayList();
employees.Add(new Employee { Department = "Sales", Name = "John", Salary = 60000 });
employees.Add(new Employee { Department = "IT", Name = "Alice", Salary = 75000 });
employees.Add(new Employee { Department = "Sales", Name = "Bob", Salary = 65000 });
employees.Add(new Employee { Department = "IT", Name = "Charlie", Salary = 70000 });

Engine engine = new Engine();
engine.SetSourceList(employees);

// Sort by Department (ascending), then Salary (descending)
engine.TableDescriptor.SortedColumns.Add("Department", ListSortDirection.Ascending);
engine.TableDescriptor.SortedColumns.Add("Salary", ListSortDirection.Descending);

// Results:
// IT - Alice (75000)
// IT - Charlie (70000)
// Sales - Bob (65000)
// Sales - John (60000)
```

## SortColumnDescriptor Class

The `SortedColumns.Add()` method has three overloads:

### Overload 1: Simple (Property Name Only)

```csharp
public int Add(string propertyName);

// Usage
engine.TableDescriptor.SortedColumns.Add("ProductName");
// Defaults to ascending order
```

### Overload 2: With Direction

```csharp
public int Add(string propertyName, ListSortDirection direction);

// Usage
engine.TableDescriptor.SortedColumns.Add("Price", ListSortDirection.Descending);
```

### Overload 3: With SortColumnDescriptor

```csharp
public int Add(SortColumnDescriptor sdc);

// Usage
SortColumnDescriptor sortDesc = new SortColumnDescriptor("ProductName");
sortDesc.SortDirection = ListSortDirection.Ascending;
engine.TableDescriptor.SortedColumns.Add(sortDesc);
```

### Accessing and Modifying Sort Descriptors

```csharp
// Add a sort column
engine.TableDescriptor.SortedColumns.Add("Price");

// Access the descriptor
SortColumnDescriptor priceSort = engine.TableDescriptor.SortedColumns["Price"];

// Modify properties
priceSort.SortDirection = ListSortDirection.Descending;

// Remove a sort column
engine.TableDescriptor.SortedColumns.Remove("Price");

// Clear all sorting
engine.TableDescriptor.SortedColumns.Clear();
```

## Common Sorting Patterns

### Pattern 1: Sort Before Grouping

```csharp
// Sort first (secondary criteria)
engine.TableDescriptor.SortedColumns.Add("Price", ListSortDirection.Descending);

// Then group (primary criteria)
engine.TableDescriptor.GroupedColumns.Add("Category");

// Result: Categories are grouped, and within each category, items sorted by price
```

### Pattern 2: Dynamic Sort Direction Toggle

```csharp
public void ToggleSortDirection(Engine engine, string columnName)
{
    if (engine.TableDescriptor.SortedColumns.Contains(columnName))
    {
        SortColumnDescriptor sortDesc = engine.TableDescriptor.SortedColumns[columnName];
        
        // Toggle direction
        sortDesc.SortDirection = sortDesc.SortDirection == ListSortDirection.Ascending
            ? ListSortDirection.Descending
            : ListSortDirection.Ascending;
    }
    else
    {
        // Add if not already sorted
        engine.TableDescriptor.SortedColumns.Add(columnName, ListSortDirection.Ascending);
    }
}
```

### Pattern 3: Conditional Sorting

```csharp
public void ApplySorting(Engine engine, string sortMode)
{
    engine.TableDescriptor.SortedColumns.Clear();
    
    switch (sortMode)
    {
        case "NameAZ":
            engine.TableDescriptor.SortedColumns.Add("Name", ListSortDirection.Ascending);
            break;
        
        case "NameZA":
            engine.TableDescriptor.SortedColumns.Add("Name", ListSortDirection.Descending);
            break;
        
        case "PriceHighLow":
            engine.TableDescriptor.SortedColumns.Add("Price", ListSortDirection.Descending);
            break;
        
        case "PriceLowHigh":
            engine.TableDescriptor.SortedColumns.Add("Price", ListSortDirection.Ascending);
            break;
        
        case "RecentFirst":
            engine.TableDescriptor.SortedColumns.Add("Date", ListSortDirection.Descending);
            break;
    }
}
```

### Pattern 4: Stable Multi-Level Sorting

```csharp
// Sort by multiple criteria with clear priority
engine.TableDescriptor.SortedColumns.Clear();

// Level 1: Status (custom priority)
SortColumnDescriptor statusSort = new SortColumnDescriptor("Status");
statusSort.Comparer = new PriorityComparer();
engine.TableDescriptor.SortedColumns.Add(statusSort);

// Level 2: Due date (ascending)
engine.TableDescriptor.SortedColumns.Add("DueDate", ListSortDirection.Ascending);

// Level 3: Title (alphabetical)
engine.TableDescriptor.SortedColumns.Add("Title", ListSortDirection.Ascending);
```

## Numerical vs String Sorting

### String Sorting

String properties sort alphabetically:
```
"1", "10", "100", "2", "20", "3"
```

This is usually not desired for numeric data stored as strings.

### Solution 1: Use Numeric Types

```csharp
public class Product
{
    public int Quantity { get; set; }  // Use int, not string
    public decimal Price { get; set; }  // Use decimal, not string
}

// Sorting works correctly with numeric types
engine.TableDescriptor.SortedColumns.Add("Quantity");
// Result: 1, 2, 3, 10, 20, 100
```

### Solution 2: Custom Comparer for String Numbers

```csharp
public class NumericStringComparer : IComparer
{
    public int Compare(object x, object y)
    {
        if (int.TryParse(x?.ToString(), out int val1) &&
            int.TryParse(y?.ToString(), out int val2))
        {
            return val1.CompareTo(val2);
        }
        
        // Fallback to string comparison
        return string.Compare(x?.ToString(), y?.ToString());
    }
}

// Apply to sort column
engine.TableDescriptor.SortedColumns.Add("Quantity");
engine.TableDescriptor.SortedColumns["Quantity"].Comparer = new NumericStringComparer();
```

## Best Practices

1. **Choose appropriate data types**: Use int/decimal for numbers, DateTime for dates
2. **Clear before resorting**: Call `SortedColumns.Clear()` when changing sort criteria
3. **Null handling in comparers**: Always check for null values in custom comparers
4. **Sort before display**: Apply sorting before iterating through records for display
5. **Combine with grouping**: Use sorting within groups for organized hierarchies
6. **Test edge cases**: Verify sorting works with empty values, duplicates, special characters
7. **Performance consideration**: Multiple sort columns and custom comparers impact performance

## Common Issues

### Issue: Unexpected Sort Order

**Symptom:** Data not sorting as expected

**Causes:**
- String sorting on numeric data
- Case-sensitive comparison
- Null values not handled

**Solutions:**
- Use numeric types for numbers
- Implement case-insensitive custom comparer
- Handle nulls in comparer logic

### Issue: Sort Not Applied

**Symptom:** Data appears unsorted

**Causes:**
- Property name misspelled
- Sort added after data display
- Grouping overriding sort

**Solutions:**
- Verify property name matches exactly (case-sensitive)
- Add sort before accessing `Table.Records`
- Add sort columns before or after grouped columns based on desired behavior

## Next Steps

1. Review [grouping-data.md](grouping-data.md) to combine sorting with grouping
2. See [filtering.md](filtering.md) to filter data before sorting
3. Explore [expressions-and-summaries.md](expressions-and-summaries.md) for calculated sort keys
