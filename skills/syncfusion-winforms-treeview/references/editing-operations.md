# Editing Operations

Dynamic editing operations allow runtime modification of tree structure including insert, delete, and rename operations with automatic data source synchronization.

## Overview

TreeViewAdv supports dynamic updates that reflect bidirectionally between the tree and bound data source. Changes made to tree nodes automatically update the underlying data, and vice versa.

## Insert Operations

### Adding Nodes Programmatically

```csharp
// Add node to tree
TreeNodeAdv newNode = new TreeNodeAdv("New Item");
treeViewAdv1.Nodes.Add(newNode);

// Add child node
parentNode.Nodes.Add(new TreeNodeAdv("Child Item"));
```

### Adding to Data-Bound Tree

When tree is data-bound, add records to data source:

```csharp
DataTable dt = (DataTable)treeViewAdv1.DataSource;

if (treeViewAdv1.SelectedItem != null)
{
    DataRow selectedRow = treeViewAdv1.SelectedItem as DataRow;
    string parentId = selectedRow[treeViewAdv1.ChildMember].ToString();
    
    // Add new row
    dt.Rows.Add("New Item", GenerateNewId(), parentId, "Value", false);
    
    // Commit changes
    dt.AcceptChanges();
}
```

## Delete Operations

### Removing Nodes

```csharp
// Remove selected node
if (treeViewAdv1.SelectedNode != null)
{
    treeViewAdv1.SelectedNode.Remove();
}
```

### Deleting from Data Source

```csharp
DataTable dt = (DataTable)treeViewAdv1.DataSource;
dt.AcceptChanges(); // Important: Accept changes first

if (treeViewAdv1.SelectedItem != null)
{
    DataRow row = treeViewAdv1.SelectedItem as DataRow;
    row.Delete();
}
```

## Rename/Update Operations

### Editing Node Text

```csharp
// Direct text update
treeViewAdv1.SelectedNode.Text = "Updated Text";
```

### Label Editing

Enable inline editing:

```csharp
// Enable label editing
treeViewAdv1.LabelEdit = true;

// Start editing programmatically
treeViewAdv1.SelectedNode.BeginEdit();

// Handle AfterEdit event
treeViewAdv1.AfterLabelEdit += (sender, e) =>
{
    if (e.Label != null && e.Label.Length > 0)
    {
        e.Node.Text = e.Label;
        // Update data source if bound
    }
    else
    {
        e.CancelEdit = true;
    }
};
```

## Data Source Synchronization

### Two-Way Binding Example

```csharp
// Setup: Bind TreeViewAdv and GridGroupingControl to same DataTable
DataTable sharedData = CreateDataTable();

treeViewAdv1.DataSource = sharedData;
treeViewAdv1.DisplayMember = "Name";
treeViewAdv1.ParentMember = "ParentID";
treeViewAdv1.ChildMember = "ID";

gridGroupingControl1.DataSource = sharedData;

// Changes in either control reflect in both
```

### Insert with Sync

```csharp
private void AddNodeToDataSource()
{
    DataTable dt = (DataTable)treeViewAdv1.DataSource;
    
    if (treeViewAdv1.SelectedItem != null)
    {
        DataRow selectedRow = treeViewAdv1.SelectedItem as DataRow;
        string parentId = selectedRow[treeViewAdv1.ChildMember].ToString();
        
        // Add new record
        dt.Rows.Add("New Item", GenerateId(), parentId, "New Value", false);
        dt.AcceptChanges();
        
        // Tree updates automatically
    }
}
```

### Delete with Sync

```csharp
private void DeleteNodeFromDataSource()
{
    DataTable dt = (DataTable)treeViewAdv1.DataSource;
    dt.AcceptChanges(); // Must call before delete
    
    if (treeViewAdv1.SelectedItem != null)
    {
        DataRow row = treeViewAdv1.SelectedItem as DataRow;
        row.Delete();
        // Tree updates automatically
    }
}
```

## AcceptChanges Pattern

**Critical:** Call `AcceptChanges()` before deletion to avoid errors:

```csharp
// Correct pattern
dataTable.AcceptChanges();
row.Delete();

// Incorrect - may cause errors
row.Delete();
dataTable.AcceptChanges();
```

## Complete Example

```csharp
public class TreeEditingExample : Form
{
    private TreeViewAdv treeViewAdv1;
    private DataTable dataTable;
    private Button btnAdd, btnDelete, btnRename;
    
    public TreeEditingExample()
    {
        InitializeComponents();
        SetupData();
    }
    
    private void SetupData()
    {
        dataTable = new DataTable();
        dataTable.Columns.Add("ID", typeof(int));
        dataTable.Columns.Add("Name", typeof(string));
        dataTable.Columns.Add("ParentID", typeof(object));
        
        dataTable.Rows.Add(1, "Root", DBNull.Value);
        dataTable.Rows.Add(2, "Child 1", 1);
        dataTable.Rows.Add(3, "Child 2", 1);
        
        treeViewAdv1.DataSource = dataTable;
        treeViewAdv1.DisplayMember = "Name";
        treeViewAdv1.ParentMember = "ParentID";
        treeViewAdv1.ChildMember = "ID";
        treeViewAdv1.SelfRelationRootValue = DBNull.Value;
    }
    
    private void btnAdd_Click(object sender, EventArgs e)
    {
        if (treeViewAdv1.SelectedItem != null)
        {
            DataRow parentRow = treeViewAdv1.SelectedItem as DataRow;
            int parentId = (int)parentRow["ID"];
            int newId = dataTable.Rows.Count + 1;
            
            dataTable.Rows.Add(newId, "New Node", parentId);
            dataTable.AcceptChanges();
        }
    }
    
    private void btnDelete_Click(object sender, EventArgs e)
    {
        dataTable.AcceptChanges();
        
        if (treeViewAdv1.SelectedItem != null)
        {
            DataRow row = treeViewAdv1.SelectedItem as DataRow;
            row.Delete();
        }
    }
    
    private void btnRename_Click(object sender, EventArgs e)
    {
        if (treeViewAdv1.SelectedNode != null)
        {
            treeViewAdv1.LabelEdit = true;
            treeViewAdv1.SelectedNode.BeginEdit();
        }
    }
}
```

## Troubleshooting

**Issue:** Changes not reflecting in tree
- **Solution:** Call `dataTable.AcceptChanges()` after modifications

**Issue:** Delete operation throws exception
- **Solution:** Call `AcceptChanges()` BEFORE calling `row.Delete()`

**Issue:** New nodes not appearing
- **Solution:** Verify parent ID matches existing node's child ID

**Issue:** Label edit not working
- **Solution:** Set `LabelEdit = true`, handle `BeforeLabelEdit` and `AfterLabelEdit` events
