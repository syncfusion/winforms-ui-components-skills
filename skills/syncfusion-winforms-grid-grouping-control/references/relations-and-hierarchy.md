# Relations and Hierarchy

## Table of Contents
- [Overview](#overview)
- [Relation Types](#relation-types)
- [Related Master Details](#related-master-details)
- [Foreign Key Reference](#foreign-key-reference)
- [ForeignKey KeyWords](#foreignkey-keywords)
- [Uniform Child List](#uniform-child-list)
- [ListItem Reference](#listitem-reference)
- [Common Scenarios](#common-scenarios)
- [Best Practices](#best-practices)

## Overview

GridGroupingControl supports hierarchical data display through master-detail relationships. Multiple tables can be connected via relations to create nested, expandable data views. The grid automatically detects DataRelations in DataSets and provides RecordPlusMinus expand/collapse buttons for navigation.

### Key Components

- **RelationDescriptor** - Defines relationship between parent and child tables
- **TableDescriptor.Relations** - Collection of relations for a table
- **SourceListSet** - Registry for data sources used in relations
- **RecordPlusMinus** - Expand/collapse button for nested records
- **RelationKind** - Type of relationship (1:1, 1:n, n:1, n:n)

## Relation Types

GridGroupingControl supports five relation types:

| Relation Type | Pattern | Description |
|--------------|---------|-------------|
| RelatedMasterDetails | 1:n | Parent has multiple child records matching key columns |
| ForeignKeyReference | n:1 | Parent looks up single record in related table by ID |
| ForeignKeyKeyWords | n:1 | Lookup relation displaying multiple related fields |
| UniformChildList | 1:n | Parent has list property containing child objects |
| ListItemReference | n:1 | Parent item references single object in list |

## Related Master Details

Master-details relation where parent and child tables share matching key columns. Most common hierarchical pattern.

### Automatic Detection from DataSet

```csharp
// Create DataSet with DataRelations
DataSet dataSet = new DataSet();
DataTable parentTable = GetParentTable();
DataTable childTable = GetChildTable();
dataSet.Tables.Add(parentTable);
dataSet.Tables.Add(childTable);

// Define DataRelation
DataRelation relation = new DataRelation("ParentChild",
    parentTable.Columns["ParentID"],
    childTable.Columns["ParentID"]);
dataSet.Relations.Add(relation);

// Grid automatically creates RelationDescriptor
gridGroupingControl1.DataSource = dataSet;
gridGroupingControl1.DataMember = "ParentTable";
```

### Manual Configuration

Set up master-detail without DataSet:

```csharp
// Step 1: Create data tables
DataTable parentTable = GetParentTable();
DataTable childTable = GetChildTable();
DataTable grandChildTable = GetGrandChildTable();

// Step 2: Create parent-to-child relation
GridRelationDescriptor parentToChild = new GridRelationDescriptor();
parentToChild.ChildTableName = "MyChildTable";  // Matches SourceListSet name
parentToChild.RelationKind = RelationKind.RelatedMasterDetails;
parentToChild.RelationKeys.Add("parentID", "ParentID");  // Parent key, Child key

// Step 3: Add relation to parent table
gridGroupingControl1.TableDescriptor.Relations.Add(parentToChild);

// Step 4: Create child-to-grandchild relation
GridRelationDescriptor childToGrandChild = new GridRelationDescriptor();
childToGrandChild.ChildTableName = "MyGrandChildTable";
childToGrandChild.RelationKind = RelationKind.RelatedMasterDetails;
childToGrandChild.RelationKeys.Add("childID", "ChildID");

// Step 5: Add relation to child table descriptor
parentToChild.ChildTableDescriptor.Relations.Add(childToGrandChild);

// Step 6: Register data sources
gridGroupingControl1.Engine.SourceListSet.Add("MyParentTable", parentTable);
gridGroupingControl1.Engine.SourceListSet.Add("MyChildTable", childTable);
gridGroupingControl1.Engine.SourceListSet.Add("MyGrandChildTable", grandChildTable);

// Step 7: Bind grid
gridGroupingControl1.DataSource = parentTable;
```

### Multi-Level Hierarchy

```csharp
// 3-level hierarchy: Orders → OrderDetails → Products
DataTable orders = GetOrders();
DataTable orderDetails = GetOrderDetails();
DataTable products = GetProducts();

// Orders to OrderDetails (1:n)
GridRelationDescriptor ordersToDetails = new GridRelationDescriptor();
ordersToDetails.ChildTableName = "OrderDetails";
ordersToDetails.RelationKind = RelationKind.RelatedMasterDetails;
ordersToDetails.RelationKeys.Add("OrderID", "OrderID");
gridGroupingControl1.TableDescriptor.Relations.Add(ordersToDetails);

// OrderDetails to Products (n:1 lookup or 1:n if showing product variants)
GridRelationDescriptor detailsToProducts = new GridRelationDescriptor();
detailsToProducts.ChildTableName = "Products";
detailsToProducts.RelationKind = RelationKind.RelatedMasterDetails;
detailsToProducts.RelationKeys.Add("ProductID", "ProductID");
ordersToDetails.ChildTableDescriptor.Relations.Add(detailsToProducts);

// Register sources
gridGroupingControl1.Engine.SourceListSet.Add("Orders", orders);
gridGroupingControl1.Engine.SourceListSet.Add("OrderDetails", orderDetails);
gridGroupingControl1.Engine.SourceListSet.Add("Products", products);

gridGroupingControl1.DataSource = orders;
```

## Foreign Key Reference

Lookup relation (n:1) where parent table has ID column that references records in a foreign table. Parent columns can display fields from foreign table using dot notation.

### Create Foreign Key Lookup

```csharp
// Parent table has StateCode column (e.g., "CA", "TX")
// USStates collection provides lookup data
USStatesCollection usStates = USStatesCollection.CreateDefaultCollection();
gridGroupingControl1.Engine.SourceListSet.Add("USStates", usStates);

// Define foreign key relation
GridRelationDescriptor foreignKeyRelation = new GridRelationDescriptor();
foreignKeyRelation.Name = "StateLookup";
foreignKeyRelation.RelationKind = RelationKind.ForeignKeyReference;

// Map columns: parent StateCode → child Key
foreignKeyRelation.RelationKeys.Add("StateCode", "Key");
foreignKeyRelation.ChildTableName = "USStates";

// Add to parent table
gridGroupingControl1.TableDescriptor.Relations.Add(foreignKeyRelation);

// Display state name from foreign table using dot notation
GridColumnDescriptor stateNameCol = new GridColumnDescriptor("StateLookup.Name");
stateNameCol.HeaderText = "State Name";
stateNameCol.Width = 150;
gridGroupingControl1.TableDescriptor.Columns.Add(stateNameCol);
```

### USStates Collection Example

```csharp
[Serializable]
public class USStatesCollection : ArrayList
{
    public new USState this[int index]
    {
        get { return (USState)base[index]; }
        set { base[index] = value; }
    }

    public static USStatesCollection CreateDefaultCollection()
    {
        USStatesCollection states = new USStatesCollection();
        states.Add(new USState("CA", "California"));
        states.Add(new USState("TX", "Texas"));
        states.Add(new USState("NY", "New York"));
        states.Add(new USState("FL", "Florida"));
        // ... more states
        return states;
    }

    public override bool IsReadOnly => true;
    public override bool IsFixedSize => true;
}

[Serializable]
public class USState
{
    public USState(string key, string name)
    {
        Key = key;
        Name = name;
    }

    [Browsable(true)]
    public string Key { get; set; }

    [Browsable(true)]
    public string Name { get; set; }

    public override string ToString() => $"{Name} ({Key})";
}
```

### Display Multiple Foreign Fields

```csharp
// Display employee name from Employees table
gridGroupingControl1.TableDescriptor.Columns.Add("EmployeeLookup.FirstName");
gridGroupingControl1.TableDescriptor.Columns.Add("EmployeeLookup.LastName");
gridGroupingControl1.TableDescriptor.Columns.Add("EmployeeLookup.Title");

// Access in code
Record record = gridGroupingControl1.Table.Records[0];
string employeeName = record.GetValue("EmployeeLookup.FirstName").ToString();
```

## ForeignKey KeyWords

Similar to ForeignKeyReference but allows specifying which child table fields to display using KeyWords property.

```csharp
GridRelationDescriptor foreignKeyKeyWords = new GridRelationDescriptor();
foreignKeyKeyWords.RelationKind = RelationKind.ForeignKeyKeyWords;
foreignKeyKeyWords.ChildTableName = "Products";
foreignKeyKeyWords.RelationKeys.Add("ProductID", "ProductID");

// Specify fields to display from child table
foreignKeyKeyWords.ChildTableDescriptor.Fields.Add("ProductName");
foreignKeyKeyWords.ChildTableDescriptor.Fields.Add("UnitPrice");
foreignKeyKeyWords.ChildTableDescriptor.Fields.Add("Category");

gridGroupingControl1.TableDescriptor.Relations.Add(foreignKeyKeyWords);
```

## Uniform Child List

Use when parent objects contain a list property with child objects (object model relations).

### Collection-Based Relation

```csharp
// Parent class with child collection
public class Order
{
    public int OrderID { get; set; }
    public string CustomerName { get; set; }
    public List<OrderDetail> OrderDetails { get; set; }  // Child list
}

public class OrderDetail
{
    public int ProductID { get; set; }
    public string ProductName { get; set; }
    public int Quantity { get; set; }
    public decimal UnitPrice { get; set; }
}

// Bind to grid
List<Order> orders = GetOrders();
gridGroupingControl1.DataSource = orders;

// Grid automatically detects OrderDetails property and creates UniformChildList relation
// Or define explicitly:
GridRelationDescriptor childListRelation = new GridRelationDescriptor();
childListRelation.RelationKind = RelationKind.UniformChildList;
childListRelation.ChildTableName = "OrderDetails";  // Property name
childListRelation.MappingName = "OrderDetails";
gridGroupingControl1.TableDescriptor.Relations.Add(childListRelation);
```

### Caching Child Lists

```csharp
// Enable caching for better performance with large datasets
GridRelationDescriptor relation = new GridRelationDescriptor();
relation.RelationKind = RelationKind.UniformChildList;
relation.ChildTableName = "OrderDetails";
relation.AllowCacheChildList = true;  // Cache child lists
gridGroupingControl1.TableDescriptor.Relations.Add(relation);
```

## ListItem Reference

Inverse of UniformChildList. Child items reference parent object.

```csharp
// Child class references parent
public class OrderDetail
{
    public int OrderDetailID { get; set; }
    public Order ParentOrder { get; set; }  // Parent reference
    public string ProductName { get; set; }
}

// Define relation
GridRelationDescriptor listItemRef = new GridRelationDescriptor();
listItemRef.RelationKind = RelationKind.ListItemReference;
listItemRef.ChildTableName = "ParentOrder";  // Property name
gridGroupingControl1.TableDescriptor.Relations.Add(listItemRef);
```

## Common Scenarios

### Scenario 1: Customer-Orders-OrderDetails (3-Level)

```csharp
// Northwind-style hierarchy
DataSet northwind = new DataSet();
DataTable customers = GetCustomers();
DataTable orders = GetOrders();
DataTable orderDetails = GetOrderDetails();

northwind.Tables.AddRange(new[] { customers, orders, orderDetails });

// Define relations
northwind.Relations.Add("CustomerOrders",
    customers.Columns["CustomerID"],
    orders.Columns["CustomerID"]);

northwind.Relations.Add("OrderDetails",
    orders.Columns["OrderID"],
    orderDetails.Columns["OrderID"]);

// Bind to grid
gridGroupingControl1.DataSource = northwind;
gridGroupingControl1.DataMember = "Customers";

// Grid shows: Customers → Orders → OrderDetails
// Each level expandable with RecordPlusMinus buttons
```

### Scenario 2: Employee Hierarchy (Self-Referencing)

```csharp
// Employee table with ManagerID referencing EmployeeID
DataTable employees = GetEmployees();

// Create self-referencing relation
DataRelation managerRelation = new DataRelation("ManagerSubordinates",
    employees.Columns["EmployeeID"],
    employees.Columns["ManagerID"]);

DataSet ds = new DataSet();
ds.Tables.Add(employees);
ds.Relations.Add(managerRelation);

gridGroupingControl1.DataSource = ds;
gridGroupingControl1.DataMember = "Employees";

// Shows org chart: Manager → Subordinates → Their Subordinates
```

### Scenario 3: Mixed Relations (Master-Detail + Foreign Key)

```csharp
// Orders with order details AND customer lookup
DataTable customers = GetCustomers();
DataTable orders = GetOrders();
DataTable orderDetails = GetOrderDetails();

// Register all sources
gridGroupingControl1.Engine.SourceListSet.Add("Customers", customers);
gridGroupingControl1.Engine.SourceListSet.Add("Orders", orders);
gridGroupingControl1.Engine.SourceListSet.Add("OrderDetails", orderDetails);

// 1. Foreign key: Orders → Customers (lookup)
GridRelationDescriptor customerLookup = new GridRelationDescriptor();
customerLookup.Name = "CustomerInfo";
customerLookup.RelationKind = RelationKind.ForeignKeyReference;
customerLookup.ChildTableName = "Customers";
customerLookup.RelationKeys.Add("CustomerID", "CustomerID");
gridGroupingControl1.TableDescriptor.Relations.Add(customerLookup);

// Add customer name column
gridGroupingControl1.TableDescriptor.Columns.Add("CustomerInfo.CompanyName");
gridGroupingControl1.TableDescriptor.Columns.Add("CustomerInfo.ContactName");

// 2. Master-detail: Orders → OrderDetails (nested)
GridRelationDescriptor orderDetailsRelation = new GridRelationDescriptor();
orderDetailsRelation.ChildTableName = "OrderDetails";
orderDetailsRelation.RelationKind = RelationKind.RelatedMasterDetails;
orderDetailsRelation.RelationKeys.Add("OrderID", "OrderID");
gridGroupingControl1.TableDescriptor.Relations.Add(orderDetailsRelation);

gridGroupingControl1.DataSource = orders;
```

### Scenario 4: Control Nested Table Appearance

```csharp
// Customize child table display
GridRelationDescriptor relation = gridGroupingControl1.TableDescriptor.Relations[0];
GridTableDescriptor childTable = relation.ChildTableDescriptor;

// Hide columns in child table
childTable.VisibleColumns.Remove("ParentID");  // Foreign key not needed
childTable.VisibleColumns.Remove("InternalID");

// Set child table appearance
childTable.Appearance.AnyRecordFieldCell.BackColor = Color.LightYellow;
childTable.Appearance.AlternateRecordFieldCell.BackColor = Color.LemonChiffon;

// Child table grouping
childTable.GroupedColumns.Add("Category");

// Child table summaries
GridSummaryColumnDescriptor summary = new GridSummaryColumnDescriptor("Total", SummaryType.DoubleAggregate, "Amount", "{Sum}");
summary.Appearance.AnySummaryCell.Format = "C2";
childTable.SummaryRows.Add(new GridSummaryRowDescriptor("TotalRow", "Total", summary));
```

## Best Practices

### Relation Configuration

1. **Name Relations Clearly**: Use descriptive names for relations:
   ```csharp
   relation.Name = "CustomerOrders";  // Good
   relation.Name = "Relation1";        // Bad
   ```

2. **Register Before Binding**: Add all data sources to SourceListSet before binding:
   ```csharp
   // Register ALL tables first
   gridGroupingControl1.Engine.SourceListSet.Add("Parent", parentTable);
   gridGroupingControl1.Engine.SourceListSet.Add("Child", childTable);
   
   // Then configure relations
   // Then bind grid
   ```

3. **Match ChildTableName**: ChildTableName must exactly match SourceListSet registration name.

### Performance

1. **Limit Nesting Depth**: 3-4 levels maximum. Deeper nesting impacts performance and UX.

2. **Cache Child Lists**: Enable for UniformChildList with large datasets:
   ```csharp
   relation.AllowCacheChildList = true;
   ```

3. **Lazy Loading**: Load child records only when expanded (use events to populate on demand).

4. **Filter Early**: Apply filters at data source level before binding, not in grid.

### Foreign Key Lookups

1. **Use for Display, Not Storage**: Foreign key columns display data but don't change underlying parent table.

2. **Read-Only Foreign Fields**: Mark foreign key display columns as read-only:
   ```csharp
   childTable.Columns["CustomerLookup.CompanyName"].Appearance.AnyRecordFieldCell.ReadOnly = true;
   ```

3. **Dropdown for Editing**: Use ComboBox cell type for editing foreign key IDs:
   ```csharp
   gridGroupingControl1.TableDescriptor.Columns["CustomerID"].Appearance.AnyRecordFieldCell.CellType = "ComboBox";
   gridGroupingControl1.TableDescriptor.Columns["CustomerID"].Appearance.AnyRecordFieldCell.DataSource = customers;
   gridGroupingControl1.TableDescriptor.Columns["CustomerID"].Appearance.AnyRecordFieldCell.DisplayMember = "CompanyName";
   gridGroupingControl1.TableDescriptor.Columns["CustomerID"].Appearance.AnyRecordFieldCell.ValueMember = "CustomerID";
   ```

### User Experience

1. **Default Expand State**: Collapse by default for performance:
   ```csharp
   gridGroupingControl1.TableDescriptor.TopLevelGroupOptions.ShowCaptionPlusMinus = true;
   gridGroupingControl1.TableDescriptor.ChildGroupOptions.ShowCaptionPlusMinus = true;
   // Records start collapsed
   ```

2. **Expand Programmatically**: Expand specific records on load:
   ```csharp
   Record firstRecord = gridGroupingControl1.Table.Records[0];
   firstRecord.IsExpanded = true;
   ```

3. **Visual Hierarchy**: Use indentation and colors to show hierarchy levels:
   ```csharp
   childTable.Appearance.AnyRecordFieldCell.BackColor = Color.LightBlue;
   grandChildTable.Appearance.AnyRecordFieldCell.BackColor = Color.LightGreen;
   ```

4. **Tooltips**: Add tooltips to RecordPlusMinus buttons explaining what expands.

### Debugging

- Verify SourceListSet registration: Check `Engine.SourceListSet` contains all referenced table names
- Check RelationKeys: Parent and child column names must match exactly (case-sensitive)
- Test with small dataset first: Confirm relations work before loading large data
- Use `TableDescriptor.Relations.Count` to verify relations are created
