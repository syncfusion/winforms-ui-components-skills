# Data Binding in GridGroupingControl

## Table of Contents
- [Overview](#overview)
- [Data Binding Using ADO.NET](#data-binding-using-adonet)
- [Binding to XML Data](#binding-to-xml-data)
- [Binding to Custom Collections](#binding-to-custom-collections)
- [Strongly Typed Collections](#strongly-typed-collections)
- [Generic Collections](#generic-collections)
- [Dynamic Object Binding](#dynamic-object-binding)
- [Unbound Mode](#unbound-mode)
- [Binding at Design Time](#binding-at-design-time)
- [Binding at Runtime](#binding-at-runtime)

## Overview

GridGroupingControl supports a wide variety of data sources for flexible data binding. The control can bind to:

- **ADO.NET Objects:** DataTable, DataSet, DataView
- **XML Data:** Using DataSet.ReadXml/WriteXml
- **Custom Collections:** IList, IBindingList, ITypedList, IListSource
- **Strongly Typed Collections:** CollectionBase-derived classes
- **Generic Collections:** List<T>, BindingList<T>, ObservableCollection<T>
- **Dynamic Objects:** ExpandoObject, DynamicObject (.NET 4.0+)
- **Unbound Mode:** Custom columns with QueryValue/SaveValue events

The data source can contain multiple nested tables, which GridGroupingControl will display hierarchically with master-detail relationships.

## Data Binding Using ADO.NET

ADO.NET is the standard data access technology for .NET applications. GridGroupingControl seamlessly integrates with ADO.NET objects.

### Binding to DataTable

The simplest approach is binding directly to a DataTable:

```csharp
using System.Data;
using Syncfusion.Windows.Forms.Grid.Grouping;

// Create DataTable
DataTable dataTable = new DataTable("Employees");
dataTable.Columns.Add("EmployeeID", typeof(int));
dataTable.Columns.Add("Name", typeof(string));
dataTable.Columns.Add("Department", typeof(string));
dataTable.Columns.Add("Salary", typeof(decimal));

// Add data
dataTable.Rows.Add(1, "John Smith", "Sales", 50000);
dataTable.Rows.Add(2, "Sarah Johnson", "Marketing", 55000);
dataTable.Rows.Add(3, "Mike Wilson", "IT", 60000);

// Bind to grid
gridGroupingControl1.DataSource = dataTable;
```

### Binding to DataSet

For multiple related tables, use a DataSet:

```csharp
// Create DataSet
DataSet dataSet = new DataSet();

// Create parent table
DataTable customers = new DataTable("Customers");
customers.Columns.Add("CustomerID", typeof(int));
customers.Columns.Add("CustomerName", typeof(string));
customers.Columns.Add("Country", typeof(string));
dataSet.Tables.Add(customers);

// Create child table
DataTable orders = new DataTable("Orders");
orders.Columns.Add("OrderID", typeof(int));
orders.Columns.Add("CustomerID", typeof(int));
orders.Columns.Add("OrderDate", typeof(DateTime));
orders.Columns.Add("Amount", typeof(decimal));
dataSet.Tables.Add(orders);

// Define relationship
DataRelation relation = new DataRelation("CustomerOrders",
    customers.Columns["CustomerID"],
    orders.Columns["CustomerID"]);
dataSet.Relations.Add(relation);

// Add sample data
customers.Rows.Add(1, "Acme Corp", "USA");
customers.Rows.Add(2, "Global Ltd", "UK");

orders.Rows.Add(101, 1, DateTime.Now, 1500.00);
orders.Rows.Add(102, 1, DateTime.Now.AddDays(-5), 2300.00);
orders.Rows.Add(103, 2, DateTime.Now.AddDays(-2), 1800.00);

// Bind to grid
gridGroupingControl1.DataSource = dataSet;
gridGroupingControl1.DataMember = "Customers";
```

The grid automatically detects the relationship and displays nested tables.

### Binding to Database

Connect to SQL Server or other databases using ADO.NET:

```csharp
using System.Data.SqlClient;

// Create connection
string connectionString = "Server=myServerAddress;Database=myDataBase;User Id=myUsername;Password=myPassword;";
SqlConnection connection = new SqlConnection(connectionString);

// Create command
SqlCommand command = new SqlCommand("SELECT * FROM Employees", connection);

// Create data adapter
SqlDataAdapter adapter = new SqlDataAdapter(command);

// Fill DataSet
DataSet dataSet = new DataSet();
adapter.Fill(dataSet, "Employees");

// Bind to grid
gridGroupingControl1.DataSource = dataSet;
gridGroupingControl1.DataMember = "Employees";
```

### Binding to Access Database (MDB)

```csharp
using System.Data.OleDb;

// Create connection
string connectionString = "Provider=Microsoft.Jet.OLEDB.4.0;Data Source=C:\\Data\\NWIND.MDB";
OleDbConnection connection = new OleDbConnection(connectionString);

// Create data adapter
OleDbDataAdapter adapter = new OleDbDataAdapter("SELECT * FROM Customers", connection);

// Fill DataSet
DataSet dataSet = new DataSet();
adapter.Fill(dataSet);

// Bind to grid
gridGroupingControl1.DataSource = dataSet.Tables[0];
```

## Binding to XML Data

GridGroupingControl can bind to XML data using DataSet's XML capabilities:

### Reading XML File

```csharp
// Create DataSet
DataSet xmlData = new DataSet();

// Read XML file
xmlData.ReadXml("C:\\Data\\Customers.xml");

// Bind to grid
gridGroupingControl1.DataSource = xmlData;
gridGroupingControl1.DataMember = xmlData.Tables[0].TableName;
```

### XML with Schema

```csharp
DataSet dataSet = new DataSet();

// Read schema first
dataSet.ReadXmlSchema("C:\\Data\\Schema.xsd");

// Then read data
dataSet.ReadXml("C:\\Data\\Data.xml");

// Bind to grid
gridGroupingControl1.DataSource = dataSet.Tables[0];
```

### Writing Changes Back to XML

```csharp
// Get DataSet from grid's data source
DataSet dataSet = (DataSet)gridGroupingControl1.DataSource;

// Write XML
dataSet.WriteXml("C:\\Data\\ModifiedData.xml");

// Write schema separately if needed
dataSet.WriteXmlSchema("C:\\Data\\Schema.xsd");
```

## Binding to Custom Collections

Custom collections provide flexibility for business object binding.

### IList Interface

Basic indexed collection:

```csharp
public class Employee
{
    public int ID { get; set; }
    public string Name { get; set; }
    public string Department { get; set; }
    public decimal Salary { get; set; }
}

// Using ArrayList (implements IList)
ArrayList employees = new ArrayList();
employees.Add(new Employee { ID = 1, Name = "John", Department = "Sales", Salary = 50000 });
employees.Add(new Employee { ID = 2, Name = "Sarah", Department = "Marketing", Salary = 55000 });

// Bind to grid
gridGroupingControl1.DataSource = employees;
```

### IBindingList Interface

For automatic change notifications:

```csharp
using System.ComponentModel;

public class Employee : INotifyPropertyChanged
{
    private string name;
    
    public int ID { get; set; }
    
    public string Name
    {
        get { return name; }
        set
        {
            if (name != value)
            {
                name = value;
                OnPropertyChanged("Name");
            }
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}

// Use BindingList for change notifications
BindingList<Employee> employees = new BindingList<Employee>();
employees.Add(new Employee { ID = 1, Name = "John" });

gridGroupingControl1.DataSource = employees;

// Changes will automatically reflect in grid
employees[0].Name = "John Smith"; // Grid updates automatically
employees.Add(new Employee { ID = 2, Name = "Sarah" }); // Grid adds new row
```

### ITypedList Interface

For custom property descriptors:

```csharp
using System.ComponentModel;

public class CustomList : ArrayList, ITypedList
{
    public PropertyDescriptorCollection GetItemProperties(PropertyDescriptor[] listAccessors)
    {
        // Return properties of item type
        return TypeDescriptor.GetProperties(typeof(Employee));
    }
    
    public string GetListName(PropertyDescriptor[] listAccessors)
    {
        return "Employees";
    }
}

CustomList employees = new CustomList();
employees.Add(new Employee { ID = 1, Name = "John" });
gridGroupingControl1.DataSource = employees;
```

## Strongly Typed Collections

Create type-safe collections using CollectionBase:

```csharp
using System.Collections;

// Define item class
public class Product
{
    public int ProductID { get; set; }
    public string ProductName { get; set; }
    public decimal UnitPrice { get; set; }
}

// Create strongly typed collection
public class ProductCollection : CollectionBase
{
    // Default indexer
    public Product this[int index]
    {
        get { return (Product)List[index]; }
        set { List[index] = value; }
    }
    
    // Add method
    public int Add(Product product)
    {
        return List.Add(product);
    }
    
    // Contains method
    public bool Contains(Product product)
    {
        return List.Contains(product);
    }
    
    // Remove method
    public void Remove(Product product)
    {
        List.Remove(product);
    }
}

// Usage
ProductCollection products = new ProductCollection();
products.Add(new Product { ProductID = 1, ProductName = "Chai", UnitPrice = 18.00m });
products.Add(new Product { ProductID = 2, ProductName = "Chang", UnitPrice = 19.00m });

gridGroupingControl1.DataSource = products;
```

## Generic Collections

Modern approach using generics (.NET 2.0+):

### List<T>

Basic generic list:

```csharp
List<Employee> employees = new List<Employee>
{
    new Employee { ID = 1, Name = "John", Department = "Sales" },
    new Employee { ID = 2, Name = "Sarah", Department = "Marketing" }
};

gridGroupingControl1.DataSource = employees;
```

### BindingList<T>

With change notifications:

```csharp
public class Employee : INotifyPropertyChanged
{
    private string name;
    
    public int ID { get; set; }
    
    public string Name
    {
        get { return name; }
        set
        {
            if (name != value)
            {
                name = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(nameof(Name)));
            }
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
}

// Use BindingList for automatic UI updates
BindingList<Employee> employees = new BindingList<Employee>
{
    new Employee { ID = 1, Name = "John" },
    new Employee { ID = 2, Name = "Sarah" }
};

gridGroupingControl1.DataSource = employees;

// Grid updates automatically when collection or items change
employees.Add(new Employee { ID = 3, Name = "Mike" });
employees[0].Name = "John Smith";
```

### ObservableCollection<T>

WPF-style collection with notifications:

```csharp
using System.Collections.ObjectModel;

ObservableCollection<Employee> employees = new ObservableCollection<Employee>
{
    new Employee { ID = 1, Name = "John" },
    new Employee { ID = 2, Name = "Sarah" }
};

gridGroupingControl1.DataSource = employees;
```

## Dynamic Object Binding

Bind to dynamic objects (.NET 4.0+):

```csharp
using System.Dynamic;

// Enable dynamic data support
gridGroupingControl1.IsDynamicData = true;

// Create dynamic objects
List<dynamic> employees = new List<dynamic>();

dynamic employee1 = new ExpandoObject();
employee1.ID = 1;
employee1.Name = "John Smith";
employee1.Department = "Sales";
employee1.Salary = 50000;
employees.Add(employee1);

dynamic employee2 = new ExpandoObject();
employee2.ID = 2;
employee2.Name = "Sarah Johnson";
employee2.Department = "Marketing";
employee2.Salary = 55000;
employees.Add(employee2);

// Bind to grid
gridGroupingControl1.DataSource = employees;
```

### Custom DynamicObject

```csharp
using System.Dynamic;
using System.Collections.Generic;

public class DynamicDictionary : DynamicObject, IDictionary<string, object>
{
    private Dictionary<string, object> dictionary = new Dictionary<string, object>();
    
    public override bool TryGetMember(GetMemberBinder binder, out object result)
    {
        return dictionary.TryGetValue(binder.Name, out result);
    }
    
    public override bool TrySetMember(SetMemberBinder binder, object value)
    {
        dictionary[binder.Name] = value;
        return true;
    }
    
    // Implement IDictionary<string, object> members
    public void Add(string key, object value) => dictionary.Add(key, value);
    public bool ContainsKey(string key) => dictionary.ContainsKey(key);
    public object this[string key]
    {
        get => dictionary[key];
        set => dictionary[key] = value;
    }
    // ... other IDictionary members
}

// Usage
gridGroupingControl1.IsDynamicData = true;
List<DynamicDictionary> data = new List<DynamicDictionary>();

dynamic item = new DynamicDictionary();
item.Name = "Product A";
item.Price = 19.99;
data.Add((DynamicDictionary)item);

gridGroupingControl1.DataSource = data;
```

## Unbound Mode

Add custom columns not backed by data source:

### Adding Unbound Columns

```csharp
// Create unbound field descriptor
FieldDescriptor unboundField = new FieldDescriptor("Notes","", false, "");
unboundField.ReadOnly = false;
gridGroupingControl1.TableDescriptor.UnboundFields.Add(unboundField);

// Configure column appearance
gridGroupingControl1.TableDescriptor.Columns["Notes"].HeaderText = "Notes";
gridGroupingControl1.TableDescriptor.Columns["Notes"].Width = 200;
```

### Providing Unbound Values

Use QueryValue and SaveValue events:

```csharp
// Store unbound values
private Dictionary<int, string> notesData = new Dictionary<int, string>();

// Initialize event handlers
gridGroupingControl1.QueryValue += GridGroupingControl1_QueryValue;
gridGroupingControl1.SaveValue += GridGroupingControl1_SaveValue;

private void GridGroupingControl1_QueryValue(object sender, FieldValueEventArgs e)
{
    if (e.Field.Name == "Notes")
    {
        // Get record key
        int id = (int)e.Record.GetValue("EmployeeID");
        
        // Provide value
        if (notesData.ContainsKey(id))
        {
            e.Value = notesData[id];
        }
    }
}

private void GridGroupingControl1_SaveValue(object sender, FieldValueEventArgs e)
{
    if (e.Field.Name == "Notes")
    {
        // Get record key
        int id = (int)e.Record.GetValue("EmployeeID");
        
        // Save value
        notesData[id] = e.Value?.ToString();
    }
}
```

### Unbound CheckBox Column

```csharp
// Create unbound field
FieldDescriptor checkBoxField = new FieldDescriptor("Selected"," ", false,"");
gridGroupingControl1.TableDescriptor.UnboundFields.Add(checkBoxField);

// Configure as CheckBox
gridGroupingControl1.TableDescriptor.Columns["Selected"].Appearance.AnyRecordFieldCell.CellType = "CheckBox";
gridGroupingControl1.TableDescriptor.Columns["Selected"].Appearance.AnyRecordFieldCell.CheckBoxOptions.CheckedValue = "True";
gridGroupingControl1.TableDescriptor.Columns["Selected"].Appearance.AnyRecordFieldCell.CheckBoxOptions.UncheckedValue = "False";
gridGroupingControl1.TableDescriptor.Columns["Selected"].Appearance.AnyRecordFieldCell.HorizontalAlignment = GridHorizontalAlignment.Center;

// Store selections
private HashSet<int> selectedRows = new HashSet<int>();

gridGroupingControl1.QueryValue += (s, e) =>
{
    if (e.Field.Name == "Selected")
    {
        int id = (int)e.Record.GetValue("EmployeeID");
        e.Value = selectedRows.Contains(id);
    }
};

gridGroupingControl1.SaveValue += (s, e) =>
{
    if (e.Field.Name == "Selected")
    {
        int id = (int)e.Record.GetValue("EmployeeID");
        bool isSelected = (bool)e.Value;
        
        if (isSelected)
            selectedRows.Add(id);
        else
            selectedRows.Remove(id);
    }
};
```

## Binding at Design Time

### Using Visual Studio Designer

1. **Drag GridGroupingControl** from Toolbox to Form
2. **Click Smart Tag** (small arrow on control)
3. **Select "Choose DataSource"** → "Add Project Data Source"
4. **Choose Data Source Type:** Database
5. **Choose Database Model:** Dataset
6. **Choose Data Connection:** New Connection or existing
7. **Select Database Objects:** Tables, Views, or Stored Procedures
8. **Click Finish**

The Designer generates:
- DataSet class
- TableAdapter for data access
- Binding code in Form.Designer.cs

### Binding to Existing DataSet

In Properties window:
1. Select **DataSource** property
2. Choose **Project Data Sources** → Your DataSet
3. Set **DataMember** to specific table

## Binding at Runtime

### Complete Example

```csharp
using System;
using System.Data;
using System.Data.SqlClient;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Grid.Grouping;

public class DataBindingForm : Form
{
    private GridGroupingControl gridGroupingControl1;
    
    public DataBindingForm()
    {
        InitializeComponent();
        BindGridToDatabase();
    }
    
    private void InitializeComponent()
    {
        this.gridGroupingControl1 = new GridGroupingControl();
        this.gridGroupingControl1.Dock = DockStyle.Fill;
        this.Controls.Add(this.gridGroupingControl1);
        
        this.Text = "Data Binding Example";
        this.Size = new System.Drawing.Size(800, 600);
    }
    
    private void BindGridToDatabase()
    {
        try
        {
            // Create connection
            string connectionString = "Your_Connection_String_Here";
            using (SqlConnection connection = new SqlConnection(connectionString))
            {
                // Create command
                SqlCommand command = new SqlCommand("SELECT * FROM Employees", connection);
                
                // Create adapter
                SqlDataAdapter adapter = new SqlDataAdapter(command);
                
                // Fill DataSet
                DataSet dataSet = new DataSet();
                adapter.Fill(dataSet, "Employees");
                
                // Bind to grid
                gridGroupingControl1.DataSource = dataSet;
                gridGroupingControl1.DataMember = "Employees";
                
                // Configure display
                gridGroupingControl1.ShowGroupDropArea = true;
                gridGroupingControl1.TableOptions.AllowSortColumns = true;
            }
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Error binding data: {ex.Message}", "Error",
                MessageBoxButtons.OK, MessageBoxIcon.Error);
        }
    }
}
```

## Best Practices

1. **Use IBindingList** for automatic change notifications
2. **Implement INotifyPropertyChanged** on business objects
3. **Use BindingList<T>** or ObservableCollection<T> for collections
4. **Dispose connections** properly after use
5. **Handle exceptions** during data binding
6. **Use BeginUpdate/EndUpdate** for multiple property changes
7. **Verify data exists** before binding
8. **Keep UI responsive** for large datasets (use async loading if needed)

## Common Issues

### Data Not Appearing

```csharp
// Verify data source has records
if (dataTable.Rows.Count == 0)
{
    MessageBox.Show("No data to display");
    return;
}

gridGroupingControl1.DataSource = dataTable;
```

### Changes Not Reflecting

```csharp
// Use IBindingList implementation
BindingList<Employee> employees = new BindingList<Employee>();
gridGroupingControl1.DataSource = employees;

// Or refresh manually
gridGroupingControl1.TableControl.Refresh();
```

### Binding to Primitive Types

```csharp
// Wrap primitive types in a class
public class Item
{
    public int Value { get; set; }
}

List<Item> items = new List<Item> { new Item { Value = 1 }, new Item { Value = 2 } };
gridGroupingControl1.DataSource = items;
```
