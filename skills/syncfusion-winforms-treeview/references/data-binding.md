# Data Binding

## Table of Contents
- [Overview](#overview)
- [Binding Types](#binding-types)
- [Binding to Self-Referencing Data](#binding-to-self-referencing-data)
- [Binding to Data Relations](#binding-to-data-relations)
- [Binding to Object-Relational Data](#binding-to-object-relational-data)
- [Binding Through Designer](#binding-through-designer)
- [Key Properties Reference](#key-properties-reference)
- [Troubleshooting](#troubleshooting)

## Overview

TreeViewAdv supports comprehensive data binding to various data sources including DataTable, DataSet, custom objects, and collections. This enables automatic tree structure generation from hierarchical data without manual node creation.

**Supported Data Sources:**
- DataTable (single or multiple tables)
- DataSet with DataRelations
- Custom object collections
- IList implementations
- Self-referencing data structures
- Microsoft Access databases

## Binding Types

TreeViewAdv supports three data binding approaches:

1. **Self-Referencing Data** - Single table with parent-child relationships via ID columns
2. **Data Relations** - Multiple related tables using DataRelation objects
3. **Object-Relational Data** - Custom classes with nested collection properties

## Binding to Self-Referencing Data

### When to Use

Use self-referencing binding when:
- Working with a single table containing parent-child references
- Data has ID and ParentID columns
- Building organization charts, category trees, or file systems
- Database uses self-referential foreign keys

### How It Works

Self-referencing binding creates tree hierarchy by matching values in two columns:
- **ChildMember** - The unique identifier column (e.g., "ID", "EmployeeID")
- **ParentMember** - The parent reference column (e.g., "ParentID", "ManagerID")

Records where ParentMember matches SelfRelationRootValue are treated as root nodes.

### Key Properties

| Property | Description | Example |
|----------|-------------|---------|
| `DataSource` | Data source object | `DataTable`, `List<T>` |
| `DisplayMember` | Field for node text | `"Name"`, `"EmployeeName"` |
| `ValueMember` | Field for node value | `"ID"`, `"EmployeeID"` |
| `ParentMember` | Parent ID field | `"ParentID"`, `"ManagerID"` |
| `ChildMember` | Child ID field | `"ID"`, `"EmployeeID"` |
| `SelfRelationRootValue` | Value indicating root nodes | `DBNull.Value`, `""`, `0` |
| `CheckedMember` | Field for checkbox state | `"IsActive"`, `"Checked"` |

### Example: Employee Hierarchy

**Data Structure:**

| EmployeeID | EmployeeName | ManagerID | Position |
|------------|--------------|-----------|----------|
| 1 | John CEO | NULL | CEO |
| 2 | Jane VP | 1 | VP |
| 3 | Bob Manager | 2 | Manager |
| 4 | Alice Dev | 3 | Developer |

**Binding Code:**

```csharp
// Assume dataTable contains employee data
DataTable employeeTable = GetEmployeeData();

// Configure TreeViewAdv for self-referencing binding
treeViewAdv1.DataSource = employeeTable;
treeViewAdv1.DisplayMember = "EmployeeName";    // What user sees
treeViewAdv1.ValueMember = "EmployeeID";        // Internal value
treeViewAdv1.ParentMember = "ManagerID";        // Parent reference
treeViewAdv1.ChildMember = "EmployeeID";        // Unique identifier
treeViewAdv1.SelfRelationRootValue = DBNull.Value; // Root nodes have null manager
```

**Result:**
```
John CEO
└── Jane VP
    └── Bob Manager
        └── Alice Dev
```

### Example: Category Tree with Checkbox

```csharp
DataTable categoryTable = new DataTable();
categoryTable.Columns.Add("CategoryID", typeof(int));
categoryTable.Columns.Add("CategoryName", typeof(string));
categoryTable.Columns.Add("ParentCategoryID", typeof(int));
categoryTable.Columns.Add("IsActive", typeof(bool));

// Root categories
categoryTable.Rows.Add(1, "Electronics", DBNull.Value, true);
categoryTable.Rows.Add(2, "Clothing", DBNull.Value, false);

// Sub-categories
categoryTable.Rows.Add(3, "Computers", 1, true);
categoryTable.Rows.Add(4, "Phones", 1, true);
categoryTable.Rows.Add(5, "Men's Wear", 2, false);

// Bind with checkbox support
treeViewAdv1.DataSource = categoryTable;
treeViewAdv1.DisplayMember = "CategoryName";
treeViewAdv1.ValueMember = "CategoryID";
treeViewAdv1.ParentMember = "ParentCategoryID";
treeViewAdv1.ChildMember = "CategoryID";
treeViewAdv1.CheckedMember = "IsActive";           // Bind checkbox state
treeViewAdv1.SelfRelationRootValue = DBNull.Value;
treeViewAdv1.ShowCheckBoxes = true;
```

### VB.NET Example

```vb
' Configure self-referencing binding
treeViewAdv1.DataSource = employeeTable
treeViewAdv1.DisplayMember = "EmployeeName"
treeViewAdv1.ValueMember = "EmployeeID"
treeViewAdv1.ParentMember = "ManagerID"
treeViewAdv1.ChildMember = "EmployeeID"
treeViewAdv1.SelfRelationRootValue = DBNull.Value
```

### Root Node Identification

**Critical:** Root nodes MUST have ParentMember value matching SelfRelationRootValue.

**Common SelfRelationRootValue patterns:**

```csharp
// NULL parent for root nodes
treeViewAdv1.SelfRelationRootValue = DBNull.Value;

// Empty string for root nodes
treeViewAdv1.SelfRelationRootValue = "";

// Zero for root nodes
treeViewAdv1.SelfRelationRootValue = 0;

// Negative value for root nodes
treeViewAdv1.SelfRelationRootValue = -1;
```

### Important Notes

1. **DisplayMember, ParentMember, and ChildMember are MANDATORY** - TreeViewAdv won't populate without them
2. **Root nodes must match SelfRelationRootValue** - Records without matching parent won't appear
3. **Orphan nodes are hidden** - If ParentMember doesn't match any ChildMember and isn't root value, node won't display
4. **Case-sensitive field names** - Ensure exact column name matches

## Binding to Data Relations

### When to Use

Use data relations binding when:
- Working with multiple related tables
- Each level comes from different table
- Database uses foreign key relationships between tables
- Need different display fields at each level

### How It Works

DataRelations define parent-child relationships between multiple DataTable objects. Each DataRelation specifies:
- Child data source (DataTable)
- Display and value members for child level
- Parent-child column relationships

### Key Components

| Component | Description |
|-----------|-------------|
| `TreeViewAdv.DataRelations` | Collection of DataRelation objects |
| `Syncfusion.Windows.Forms.Tools.DataRelation` | Defines one parent-child table relationship |

### DataRelation Constructor Parameters

```csharp
DataRelation(
    DataTable childDataSource,      // Child table
    string childDataMember,         // Child table name
    string childDisplayMember,      // Child node text field
    string parentKeyField,          // Parent table foreign key
    string childKeyField,           // Child table key field
    string childValueMember,        // Child node value field
    string childCheckedMember       // Child checkbox field (optional)
)
```

### Example: Multi-Level Folder Structure

**Data Structure:**

**Table_1 (Folders):**
| FolderName | ParentFolder | ChildFolder |
|------------|--------------|-------------|
| Root | NULL | 1 |
| Documents | 1 | 2 |

**Table_2 (SubFolders):**
| SubFolderName1 | ChildFolder | SubFolderChild1 |
|----------------|-------------|-----------------|
| Personal | 2 | 3 |
| Work | 2 | 4 |

**Table_3 (Sub-SubFolders):**
| SubFolderName2 | SubFolderChild1 | SubFolderChild2 |
|----------------|-----------------|-----------------|
| Projects | 4 | 5 |
| Reports | 4 | 6 |

**Table_4 (Files):**
| SubFolderName3 | SubFolderChild2 | SubFolderChild3 |
|----------------|-----------------|-----------------|
| File1.doc | 5 | 7 |
| File2.xlsx | 6 | 8 |

**Binding Code:**

```csharp
using Syncfusion.Windows.Forms.Tools;

// Create DataRelation objects for each level
DataRelation childRelation1 = new DataRelation(
    Table_2,               // Child table
    "Table_2",            // Child table name
    "SubFolderName1",     // Display field for child level
    "ChildFolder",        // Parent table key
    "SubFolderChild1",    // Child table key
    "SubFolderName1",     // Value field
    "Checked"             // Checkbox field (optional)
);

DataRelation childRelation2 = new DataRelation(
    Table_3,
    "SubFolderName2",
    "SubFolderChild1",
    "SubFolderChild2"
);

DataRelation childRelation3 = new DataRelation(
    Table_4,
    "Table_4",
    "SubFolderName3",
    "SubFolderChild2",
    "SubFolderChild3",
    "SubFolderName3",
    "Checked"
);

// Clear existing relations
treeViewAdv1.DataRelations.Clear();

// Configure root level
treeViewAdv1.DisplayMember = "FolderName";
treeViewAdv1.ParentMember = "ParentFolder";
treeViewAdv1.ChildMember = "ChildFolder";

// Add relations
treeViewAdv1.DataRelations.Add(childRelation1);
treeViewAdv1.DataRelations.Add(childRelation2);
treeViewAdv1.DataRelations.Add(childRelation3);

// Set root data source
treeViewAdv1.DataSource = Table_1;
```

### VB.NET Example

```vb
Dim childRelation1 As Syncfusion.Windows.Forms.Tools.DataRelation
Dim childRelation2 As Syncfusion.Windows.Forms.Tools.DataRelation

childRelation1 = New DataRelation(Table_2, "SubFolder1", "SubFolderName1", "FolderChild", "SubFolderChild1", "SubFolderName1", "Checked")
childRelation2 = New DataRelation(Table_3, "SubFolderName2", "SubFolderChild1", "SubFolderChild2")

treeViewAdv1.DataRelations.Clear()
treeViewAdv1.DisplayMember = "FolderName"
treeViewAdv1.ParentMember = "ParentFolder"
treeViewAdv1.ChildMember = "ChildFolder"
treeViewAdv1.DataRelations.Add(childRelation1)
treeViewAdv1.DataRelations.Add(childRelation2)
treeViewAdv1.DataSource = Table_1
```

### Adding Levels Dynamically

```csharp
// To add a new level at runtime
DataTable newLevelTable = CreateNewLevelData();

DataRelation newRelation = new DataRelation(
    newLevelTable,
    "NewLevelName",
    "LastLevelKey",
    "NewLevelKey"
);

treeViewAdv1.DataRelations.Add(newRelation);
treeViewAdv1.DataSource = treeViewAdv1.DataSource; // Refresh
```

### Important Notes

1. **Mandatory Properties:** DisplayMember, ParentMember, ChildMember must be set for root level
2. **DataRelation Order:** Add relations in hierarchical order (level 1, level 2, level 3, etc.)
3. **Key Matching:** Parent and child key fields must match for proper relationships
4. **Dynamic Levels:** Create new DataRelation instance and add to DataRelations collection

## Binding to Object-Relational Data

### When to Use

Use object-relational binding when:
- Working with custom class hierarchies
- Classes have collection properties referencing child classes
- Object-oriented data model with nested objects
- LINQ to Objects or Entity Framework results

### How It Works

Object-relational binding traverses class properties to build tree hierarchy. Classes contain collection properties that reference other classes, creating natural parent-child relationships.

**Key Difference:** No ParentMember property needed - relationships defined by class structure.

### Key Properties

| Property | Description | Format |
|----------|-------------|--------|
| `DisplayMember` | Property names for node text at each level | `"Prop1\\Prop2\\Prop3"` |
| `ChildMember` | Class names defining hierarchy order | `"Class1\\Class2\\Class3"` |

### Example: Geographic Hierarchy

**Class Structure:**

```csharp
// Root level class
public class Continent
{
    public string ContinentName { get; set; }
    public int ContinentID { get; set; }
    public List<Country> Country_List { get; set; }  // Collection of child objects
}

// Second level class
public class Country
{
    public string CountryName { get; set; }
    public int CountryID { get; set; }
    public List<State> State_List { get; set; }  // Collection of grandchild objects
}

// Third level class
public class State
{
    public string StateName { get; set; }
    public int StateID { get; set; }
    public string Capital { get; set; }
}
```

**Creating Data:**

```csharp
// Create sample data
List<Continent> continents = new List<Continent>
{
    new Continent
    {
        ContinentName = "North America",
        ContinentID = 1,
        Country_List = new List<Country>
        {
            new Country
            {
                CountryName = "United States",
                CountryID = 1,
                State_List = new List<State>
                {
                    new State { StateName = "California", StateID = 1, Capital = "Sacramento" },
                    new State { StateName = "Texas", StateID = 2, Capital = "Austin" },
                    new State { StateName = "New York", StateID = 3, Capital = "Albany" }
                }
            },
            new Country
            {
                CountryName = "Canada",
                CountryID = 2,
                State_List = new List<State>
                {
                    new State { StateName = "Ontario", StateID = 4, Capital = "Toronto" },
                    new State { StateName = "Quebec", StateID = 5, Capital = "Quebec City" }
                }
            }
        }
    },
    new Continent
    {
        ContinentName = "Europe",
        ContinentID = 2,
        Country_List = new List<Country>
        {
            new Country
            {
                CountryName = "Germany",
                CountryID = 3,
                State_List = new List<State>
                {
                    new State { StateName = "Bavaria", StateID = 6, Capital = "Munich" }
                }
            }
        }
    }
};
```

**Binding Code:**

```csharp
// Set data source
treeViewAdv1.DataSource = continents;

// Define display members for each level (separated by \\)
treeViewAdv1.DisplayMember = "ContinentName\\CountryName\\StateName";

// Define class hierarchy order (separated by \\)
treeViewAdv1.ChildMember = "Continent\\Country\\State";

// Optional: Set value members
treeViewAdv1.ValueMember = "ContinentID\\CountryID\\StateID";
```

**Result:**
```
North America
├── United States
│   ├── California
│   ├── Texas
│   └── New York
└── Canada
    ├── Ontario
    └── Quebec
Europe
└── Germany
    └── Bavaria
```

### Understanding DisplayMember and ChildMember Syntax

**DisplayMember Format:** `"Level1Property\\Level2Property\\Level3Property"`
- Specifies which property to display at each tree level
- Order matches tree hierarchy (root → leaf)

**ChildMember Format:** `"Level1Class\\Level2Class\\Level3Class"`
- Defines class type at each level
- Order determines nesting structure

**Example Mapping:**
```
Level 1: Continent class → Display ContinentName
Level 2: Country class → Display CountryName  
Level 3: State class → Display StateName
```

### VB.NET Example

```vb
' Set data source and configure binding
treeViewAdv1.DataSource = continents
treeViewAdv1.DisplayMember = "ContinentName\\CountryName\\StateName"
treeViewAdv1.ChildMember = "Continent\\Country\\State"
```

### Important Notes

1. **No ParentMember Required** - Relationships defined by collection properties
2. **Property Names Must Match** - DisplayMember property names must exist in classes
3. **Collection Properties** - Each parent class needs collection property of child type
4. **Level Separator** - Use double backslash `\\` to separate levels

## Binding Through Designer

### Microsoft Access Database Setup

TreeViewAdv supports visual data binding through Visual Studio Designer for Microsoft Access databases.

**Steps to Import Database:**

1. Open Visual Studio → **View** menu → **Other Windows** → **Data Sources**
2. In Data Sources window, click **Add New Data Source**
3. Select **Database** → **Next**
4. Click **New Connection**
5. Click **Change** → Select **.NET Framework Data Provider for OLE DB**
6. In OLE DB Provider, select **Microsoft Office 12.0 Access Database Engine OLE DB Provider**
7. In **Server or file name**, browse to .accdb file → **OK**
8. **Next** → **Next** on connection string page
9. Expand **Tables** node → Select desired tables → **Finish**

### Binding DataSource Property via Designer

**Configure at design-time:**

1. Select TreeViewAdv control on form
2. Open Properties window
3. Find **DataSource** property
4. Click dropdown → Select data source from list
5. Choose specific table from bound dataset

### Setting DisplayMember via Designer

1. After setting DataSource
2. Find **DisplayMember** property in Properties window
3. Click dropdown → Select field from list
4. This field will be displayed as node text

### Setting ValueMember and ParentMember

1. **ValueMember:** Select field for node internal value
2. **ParentMember:** Select parent ID field for self-referencing
3. **ChildMember:** Select child ID field for self-referencing

### Creating DataRelations via Designer

**For multi-table hierarchies:**

1. Select TreeViewAdv → Open Properties window
2. Find **DataRelations** property → Click ellipsis (...)
3. **DataRelations Collection Editor** opens
4. Click **Add** to create new relation
5. Configure relation properties:
   - **ChildDataSource:** Select child table
   - **ChildDisplayMember:** Select display field
   - **ParentKeyField:** Select parent table key
   - **ChildKeyField:** Select child table key
6. Click **Add** for additional levels
7. Click **OK** to save

## Key Properties Reference

### Essential Properties

```csharp
// Data source configuration
DataSource          // Object containing data (DataTable, List, etc.)
DataMember          // Table name when using DataSet

// Self-referencing binding
DisplayMember       // Field for node text
ValueMember         // Field for node value
ParentMember        // Parent ID field
ChildMember         // Child ID field
SelfRelationRootValue  // Value indicating root nodes

// Checkbox binding
CheckedMember       // Field for checkbox state

// Multi-table binding
DataRelations       // Collection of DataRelation objects
```

### Property Usage Matrix

| Binding Type | DataSource | DisplayMember | ParentMember | ChildMember | DataRelations |
|--------------|------------|---------------|--------------|-------------|---------------|
| Self-Referencing | Required | Required | Required | Required | Not used |
| Data Relations | Required | Required | Required | Required | Required |
| Object-Relational | Required | Required | Not used | Required | Not used |

## Troubleshooting

**Issue:** TreeView is empty after binding
- **Solution:** Verify DisplayMember, ParentMember, ChildMember match exact column/property names (case-sensitive)
- **Solution:** Check SelfRelationRootValue matches root node pattern in data
- **Solution:** Ensure DataSource is not null and contains data

**Issue:** Root nodes not appearing
- **Solution:** For self-referencing, verify root records have ParentMember value matching SelfRelationRootValue exactly
- **Solution:** Check for DBNull.Value vs null vs empty string mismatch

**Issue:** Some nodes missing (orphan nodes)
- **Solution:** Verify all ParentMember values either match a ChildMember value OR match SelfRelationRootValue
- **Solution:** Check data integrity - every non-root record must have valid parent reference

**Issue:** Wrong hierarchy structure
- **Solution:** Verify ParentMember and ChildMember point to correct columns
- **Solution:** For data relations, check key field mappings in DataRelation objects

**Issue:** Changes to data source not reflecting in tree
- **Solution:** Call `treeViewAdv1.DataSource = treeViewAdv1.DataSource;` to refresh
- **Solution:** For DataTable, call `dataTable.AcceptChanges()` after modifications

**Issue:** Performance slow with large datasets
- **Solution:** Enable virtualization: `treeViewAdv1.EnableVirtualization = true;`
- **Solution:** Use LoadOnDemand for lazy loading child nodes
- **Solution:** Consider filtering data source before binding

**Issue:** Designer binding not working
- **Solution:** Rebuild project, restart Visual Studio
- **Solution:** Check data source connection string in app.config
- **Solution:** Verify database file exists at specified path
