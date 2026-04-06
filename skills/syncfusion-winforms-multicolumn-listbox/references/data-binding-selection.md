# Data Binding and Selection Modes

This guide covers data binding techniques and selection mode configuration for GridListControl.

## Data Binding Overview

Data binding in GridListControl allows you to populate the control with data from various sources. The control automatically generates columns based on the public properties of your data objects or the columns in your data table.

**Benefits of Data Binding:**
- Automatic UI updates when data changes
- No manual population code needed
- Type-safe data access
- Supports large datasets efficiently

## DataSource Property

The `DataSource` property is the primary mechanism for binding data to GridListControl.

```csharp
gridListControl1.DataSource = yourDataSource;
```

### Supported Data Source Types

- **ArrayList** - Dynamic array of objects
- **List<T>** - Generic list of typed objects
- **DataTable** - ADO.NET table
- **DataSet** - ADO.NET dataset (specify table)
- **BindingList<T>** - List with change notifications
- **Any IList implementation** - Custom collections

## Binding to ArrayList and Custom Objects

ArrayList binding is useful for simple scenarios and dynamic data.

### Example with Custom Objects

```csharp
using System.Collections;

// Create ArrayList
ArrayList array = new ArrayList();

// Add custom objects
array.Add(new MyClass(001, "John David"));
array.Add(new MyClass(002, "Tom"));
array.Add(new MyClass(003, "Bretney"));
array.Add(new MyClass(004, "Jessy"));
array.Add(new MyClass(005, "Bruch"));
array.Add(new MyClass(006, "Johny"));

// Bind to GridListControl
this.gridListControl1.DataSource = array;

// Custom class definition
public class MyClass
{
    public int ID { get; set; }
    public string Name { get; set; }
    
    public MyClass(int id, string name)
    {
        ID = id;
        Name = name;
    }
}
```

### VB.NET Example

```vb
Dim array As ArrayList = New ArrayList()
array.Add(New [MyClass](1, "John David"))
array.Add(New [MyClass](2, "Tom"))
array.Add(New [MyClass](3, "Bretney"))
array.Add(New [MyClass](4, "Jessy"))
array.Add(New [MyClass](5, "Bruch"))
array.Add(New [MyClass](6, "Johny"))

Me.gridListControl1.DataSource = array
```

**Important:** GridListControl displays public properties of bound objects as columns. Private fields are not displayed.

## Binding to Generic Lists

Generic lists provide type safety and better performance.

```csharp
using System.Collections.Generic;

// Create typed list
List<Employee> employees = new List<Employee>();

employees.Add(new Employee 
{ 
    EmployeeID = 1001, 
    Name = "Alice Smith", 
    Department = "Sales",
    Salary = 50000 
});

employees.Add(new Employee 
{ 
    EmployeeID = 1002, 
    Name = "Bob Johnson", 
    Department = "Engineering",
    Salary = 75000 
});

// Bind to GridListControl
gridListControl1.DataSource = employees;

// Employee class
public class Employee
{
    public int EmployeeID { get; set; }
    public string Name { get; set; }
    public string Department { get; set; }
    public decimal Salary { get; set; }
}
```

**Advantages:**
- Compile-time type checking
- IntelliSense support
- Better performance
- Easier to maintain

## Binding to DataTable

DataTable binding is ideal for database scenarios.

```csharp
using System.Data;
using System.Data.SqlClient;

// Create DataTable from database
DataTable dataTable = new DataTable();

using (SqlConnection conn = new SqlConnection(connectionString))
{
    SqlDataAdapter adapter = new SqlDataAdapter("SELECT * FROM Customers", conn);
    adapter.Fill(dataTable);
}

// Bind to GridListControl
gridListControl1.DataSource = dataTable;
```

**Features:**
- Direct database integration
- Automatic column generation from table schema
- Supports updates and change tracking
- Works with Entity Framework and LINQ

## Selection Modes

GridListControl supports three distinct selection modes that control how users can select rows.

### SelectionMode Property

```csharp
gridListControl1.SelectionMode = SelectionMode.One;
```

### Selection Mode Types

#### 1. SelectionMode.One

**Single row selection only.**

```csharp
this.gridListControl1.SelectionMode = SelectionMode.One;
```

**Behavior:**
- User can select only one row at a time
- Clicking a new row deselects the previous row
- No multi-selection possible

**When to use:**
- Master-detail views where only one master is shown
- Record selection for editing
- Simple lookup scenarios
- When only one choice is valid

**VB.NET:**
```vb
Me.gridListControl1.SelectionMode = SelectionMode.One
```

#### 2. SelectionMode.MultiSimple

**Multiple row selection with simple clicking.**

```csharp
this.gridListControl1.SelectionMode = SelectionMode.MultiSimple;
```

**Behavior:**
- Click any row to toggle its selection
- Multiple rows can be selected simultaneously
- No keyboard modifiers required
- Click again to deselect

**When to use:**
- Checklist-style interfaces
- Batch operations on multiple items
- Touch-friendly multi-selection
- Simple selection without keyboard shortcuts

**VB.NET:**
```vb
Me.gridListControl1.SelectionMode = SelectionMode.MultiSimple
```

#### 3. SelectionMode.MultiExtended

**Multiple row selection with keyboard modifiers.**

```csharp
this.gridListControl1.SelectionMode = SelectionMode.MultiExtended;
```

**Behavior:**
- **Click:** Select single row (deselects others)
- **CTRL + Click:** Toggle individual row selection
- **SHIFT + Click:** Select range from last selected to clicked row
- **CTRL + Arrow keys:** Navigate without changing selection
- **CTRL + Space:** Toggle selection at current position
- **SHIFT + Arrow keys:** Extend selection

**When to use:**
- Power user interfaces
- Windows Explorer-style selection
- Complex multi-selection scenarios
- Desktop applications with keyboard navigation

**VB.NET:**
```vb
Me.gridListControl1.SelectionMode = SelectionMode.MultiExtended
```

## Selection Mode Comparison

| Feature | One | MultiSimple | MultiExtended |
|---------|-----|-------------|---------------|
| Single selection | ✓ | ✓ | ✓ |
| Multiple selection | ✗ | ✓ | ✓ |
| Click to toggle | ✗ | ✓ | With CTRL |
| Range selection | ✗ | ✗ | With SHIFT |
| Keyboard navigation | ✓ | ✓ | ✓ Advanced |
| Touch-friendly | ✓ | ✓ | ✗ |
| User complexity | Simple | Simple | Moderate |

## Working with Selected Items

### Getting Selected Items

```csharp
// Get selected row index
int selectedIndex = gridListControl1.SelectedIndex;

// Get selected item (bound object)
object selectedItem = gridListControl1.SelectedItem;
```

## Data Binding Best Practices

### 1. Use Strongly-Typed Collections

Prefer `List<T>` over `ArrayList` for type safety:

```csharp
// Good
List<Customer> customers = new List<Customer>();

// Avoid
ArrayList customers = new ArrayList();
```

### 2. Validate Data Source Before Binding

```csharp
if (dataSource != null && dataSource.Count > 0)
{
    gridListControl1.DataSource = dataSource;
}
else
{
    MessageBox.Show("No data to display");
}
```

### 3. Refresh After Data Changes

```csharp
// After modifying bound data
gridListControl1.Refresh();

// Or rebind
gridListControl1.DataSource = null;
gridListControl1.DataSource = updatedData;
```

### 4. Use BindingList for Automatic Updates

```csharp
using System.ComponentModel;

BindingList<Customer> customers = new BindingList<Customer>();
gridListControl1.DataSource = customers;

// Changes automatically reflect in UI
customers.Add(new Customer { Name = "New Customer" });
```

## Selection Best Practices

### 1. Choose Appropriate Mode

- Use **One** for single-selection scenarios
- Use **MultiSimple** for touch devices or simple multi-select
- Use **MultiExtended** for desktop power users

### 2. Provide Visual Feedback

```csharp
// Highlight selected items with custom colors
// (See customization.md for styling details)
```

### 3. Handle Empty Selections

```csharp
if (gridListControl1.SelectedIndex == -1)
{
    MessageBox.Show("Please select an item first");
    return;
}
```

## Common Scenarios

### Scenario 1: Load Data from Database

```csharp
private void LoadCustomers()
{
    try
    {
        using (SqlConnection conn = new SqlConnection(connectionString))
        {
            SqlDataAdapter adapter = new SqlDataAdapter(
                "SELECT CustomerID, CompanyName, ContactName, City FROM Customers", 
                conn);
            
            DataTable dt = new DataTable();
            adapter.Fill(dt);
            
            gridListControl1.DataSource = dt;
            gridListControl1.SelectionMode = SelectionMode.One;
        }
    }
    catch (Exception ex)
    {
        MessageBox.Show($"Error loading data: {ex.Message}");
    }
}
```

## Troubleshooting

**No data appears after binding**
- Check if data source has items
- Verify MultiColumn is true for multi-column display
- Ensure properties are public in data class

**Cannot select multiple items**
- Set SelectionMode to MultiSimple or MultiExtended
- Check if control is enabled

**Keyboard shortcuts not working in MultiExtended**
- Verify control has focus
- Check if other controls are capturing keyboard input
- Ensure SelectionMode is MultiExtended, not MultiSimple
