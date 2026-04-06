# Data Binding in Grouping Engine

This guide covers how to bind data to the Syncfusion Grouping Engine, including creating custom data objects, building IList datasources, and accessing bound data.

## Overview

Essential Grouping works with any IList datasource whose items have public properties. The datasource can be:
- `ArrayList` of custom objects
- `List<T>` generic collections
- `BindingList<T>` for two-way binding
- Any collection implementing `IList` interface

## Creating Custom Data Objects

Your data objects must have **public properties** for the Grouping Engine to access them. The engine uses reflection to read property values.

### Basic Custom Object

```csharp
public class MyObject
{
    private string aValue;
    private string bValue;
    private string cValue;
    private string dValue;

    public MyObject(int i)
    {
        aValue = string.Format("a{0}", i);
        bValue = string.Format("{0}", i);      // Numeric string
        cValue = string.Format("c{0}", i % 3); // Modulo for grouping
        dValue = string.Format("d{0}", i % 2); // Binary grouping
    }

    public string A
    {
        get { return aValue; }
        set { aValue = value; }
    }

    public string B
    {
        get { return bValue; }
        set { bValue = value; }
    }

    public string C
    {
        get { return cValue; }
        set { cValue = value; }
    }

    public string D
    {
        get { return dValue; }
        set { dValue = value; }
    }

    public override string ToString()
    {
        return A + "\t" + B + "\t" + C + "\t" + D;
    }
}
```

**VB.NET Version:**

```vb
Public Class MyObject
    Private aValue As String
    Private bValue As String
    Private cValue As String
    Private dValue As String

    Public Sub New(ByVal i As Integer)
        aValue = String.Format("a{0}", i)
        bValue = String.Format("{0}", i)
        cValue = String.Format("c{0}", i Mod 3)
        dValue = String.Format("d{0}", i Mod 2)
    End Sub

    Public Property A() As String
        Get
            Return aValue
        End Get
        Set(ByVal Value As String)
            aValue = Value
        End Set
    End Property

    Public Property B() As String
        Get
            Return bValue
        End Get
        Set(ByVal Value As String)
            bValue = Value
        End Set
    End Property

    Public Property C() As String
        Get
            Return cValue
        End Get
        Set(ByVal Value As String)
            cValue = Value
        End Set
    End Property

    Public Property D() As String
        Get
            Return dValue
        End Get
        Set(ByVal Value As String)
            dValue = Value
        End Set
    End Property

    Public Overrides Function ToString() As String
        Return A + ControlChars.Tab + B + ControlChars.Tab + C + ControlChars.Tab + D
    End Function
End Class
```

### Key Requirements for Data Objects

1. **Public properties only**: Private fields are not accessible to Grouping Engine
2. **Get accessors required**: Engine needs to read property values
3. **Set accessors optional**: Only needed if you modify data through the engine
4. **Property types**: Support string, int, decimal, DateTime, bool, and custom types
5. **ToString() override**: Helpful for debugging and console output

## Building ArrayList Datasources

### Creating and Populating ArrayList

```csharp
using System;
using System.Collections;

// Create ArrayList
ArrayList list = new ArrayList();

// Create random data for testing
Random r = new Random();

for (int i = 0; i < 10; i++)
{
    list.Add(new MyObject(r.Next(5)));
    Console.WriteLine(list[i]); // Display created object
}
```

**VB.NET Version:**

```vb
Imports System.Collections

' Create ArrayList
Dim list As New ArrayList()
Dim r As New Random()
Dim i As Integer

For i = 0 To 9
    list.Add(New MyObject(r.Next(5)))
    Console.WriteLine(list(i))
Next i
```

### Real-World Example: Sales Records

```csharp
public class SalesRecord
{
    public string ProductName { get; set; }
    public string Category { get; set; }
    public int Quantity { get; set; }
    public decimal Price { get; set; }
    public DateTime OrderDate { get; set; }
    
    public SalesRecord(string product, string category, int qty, decimal price, DateTime date)
    {
        ProductName = product;
        Category = category;
        Quantity = qty;
        Price = price;
        OrderDate = date;
    }
}

// Build datasource
ArrayList salesData = new ArrayList();
salesData.Add(new SalesRecord("Laptop", "Electronics", 5, 1200.00m, DateTime.Parse("2024-01-15")));
salesData.Add(new SalesRecord("Mouse", "Electronics", 25, 15.00m, DateTime.Parse("2024-01-16")));
salesData.Add(new SalesRecord("Desk", "Furniture", 10, 350.00m, DateTime.Parse("2024-01-17")));
salesData.Add(new SalesRecord("Chair", "Furniture", 20, 150.00m, DateTime.Parse("2024-01-18")));
```

## Setting Datasource in Grouping Engine

### Basic Setup

```csharp
using Syncfusion.Grouping;

// Create Grouping Engine object
Engine groupingEngine = new Engine();

// Set datasource (IList object)
groupingEngine.SetSourceList(list);
```

**VB.NET Version:**

```vb
Imports Syncfusion.Grouping

' Create Grouping Engine object
Dim groupingEngine As New Engine()

' Set datasource
groupingEngine.SetSourceList(list)
```

### Complete Setup Example

```csharp
using System;
using System.Collections;
using Syncfusion.Grouping;

namespace GroupingSample
{
    class Program
    {
        static void Main(string[] args)
        {
            // Create datasource
            ArrayList list = new ArrayList();
            Random r = new Random();

            for (int i = 0; i < 10; i++)
            {
                list.Add(new MyObject(r.Next(5)));
            }

            // Create and configure engine
            Engine groupingEngine = new Engine();
            groupingEngine.SetSourceList(list);

            Console.WriteLine("Data successfully bound to Grouping Engine");
            Console.ReadLine();
        }
    }
}
```

## Accessing Bound Data

### Iterating Through Records

The `Engine.Table.Records` collection provides access to all data records:

```csharp
// Access data through the Engine
foreach (Record rec in groupingEngine.Table.Records)
{
    MyObject obj = rec.GetData() as MyObject;
    
    if (obj != null)
    {
        Console.WriteLine(obj);
    }
}

// Pause for viewing
Console.ReadLine();
```

**VB.NET Version:**

```vb
' Access data through the Engine
Dim rec As Record

For Each rec In groupingEngine.Table.Records
    Dim obj As MyObject = CType(rec.GetData(), MyObject)
    
    If Not (obj Is Nothing) Then
        Console.WriteLine(obj)
    End If
Next rec

' Pause for viewing
Console.ReadLine()
```

### Understanding the Record Object

The `Record` class represents a single data item in the engine:

**Key Methods:**
- `GetData()`: Returns the underlying data object
- `GetValue(string propertyName)`: Gets a specific property value
- `SetValue(string propertyName, object value)`: Sets a property value

```csharp
foreach (Record rec in groupingEngine.Table.Records)
{
    // Method 1: Get entire object
    MyObject obj = rec.GetData() as MyObject;
    
    // Method 2: Get specific property values
    string aValue = rec.GetValue("A").ToString();
    string bValue = rec.GetValue("B").ToString();
    
    Console.WriteLine($"A={aValue}, B={bValue}");
}
```

## Engine and Table Relationship

Understanding the object hierarchy:

```
Engine (root object)
└── Table (data container)
    ├── Records (all records)
    ├── FilteredRecords (filtered records)
    └── TopLevelGroup (group hierarchy)
```

**Engine Properties:**
- `Engine.Table`: Main data container
- `Engine.TableDescriptor`: Schema and configuration

**Table Properties:**
- `Table.Records`: All records in the table
- `Table.FilteredRecords`: Records after applying filters
- `Table.TopLevelGroup`: Root group for hierarchical access

## Working with Different IList Types

### Generic List<T>

```csharp
using System.Collections.Generic;

List<SalesRecord> salesList = new List<SalesRecord>();
salesList.Add(new SalesRecord("Product1", "Category1", 10, 50.00m, DateTime.Now));

Engine engine = new Engine();
engine.SetSourceList(salesList); // Works with List<T>
```

### BindingList<T> for Two-Way Binding

```csharp
using System.ComponentModel;

BindingList<SalesRecord> bindingList = new BindingList<SalesRecord>();
bindingList.Add(new SalesRecord("Product1", "Category1", 10, 50.00m, DateTime.Now));

Engine engine = new Engine();
engine.SetSourceList(bindingList);

// Changes to bindingList automatically update the engine
```

### DataTable (via DataView)

```csharp
using System.Data;

DataTable dataTable = new DataTable();
dataTable.Columns.Add("Name", typeof(string));
dataTable.Columns.Add("Value", typeof(int));
dataTable.Rows.Add("Item1", 100);

Engine engine = new Engine();
engine.SetSourceList(dataTable.DefaultView); // Use DataView
```

## Best Practices

1. **Property design**: Use meaningful property names that describe the data
2. **Type selection**: Choose appropriate data types (int vs string for numbers affects sorting/filtering)
3. **Immutable IDs**: Include unique identifier properties for record tracking
4. **ToString() override**: Implement for debugging and logging purposes
5. **Null handling**: Check for null when casting with `GetData() as T`
6. **Collection choice**: Use List<T> for modern .NET, ArrayList for legacy compatibility
7. **Property count**: Keep reasonable number of properties (too many impacts performance)

## Common Issues

### Property Not Found

**Symptom:** Error when accessing property by name

**Cause:** Property is private, misspelled, or doesn't exist

**Solution:** Verify property is public and name matches exactly (case-sensitive)

### Cast Failures

**Symptom:** `GetData() as T` returns null

**Cause:** Wrong type cast or record contains different object type

**Solution:** Check datasource contains expected object types; use `is` operator to test

### Empty Records Collection

**Symptom:** `Table.Records.Count == 0` even though datasource has items

**Cause:** Datasource not set or set incorrectly

**Solution:** Verify `SetSourceList()` called with valid IList; check datasource isn't empty

## Next Steps

1. Review [grouping-data.md](grouping-data.md) to learn about grouping records by properties
2. Explore [sorting.md](sorting.md) for sorting capabilities
3. See [filtering.md](filtering.md) for filtering records with expressions
