# Nodes and Columns Management

This guide covers working with TreeNodeAdv (nodes), TreeColumnAdv (columns), and TreeNodeAdvSubItem (subitems) to build and manage hierarchical multi-column data structures.

## TreeColumnAdv - Column Configuration

Columns define the structure and headers of your MultiColumnTreeView. The first column shows the tree hierarchy with expand/collapse controls.

### Creating Columns

```csharp
// Create column instance
TreeColumnAdv column = new TreeColumnAdv();
column.Text = "Employee Name";  // Header text
column.Width = 200;              // Width in pixels
column.TextColor = Color.Black;  // Header text color

// Add to control
multiColumnTreeView1.Columns.Add(column);
```

### Column Properties

**Text Properties:**
- `Text` - Header text displayed in column
- `TextColor` - Color of header text
- `Font` - Font for header text

**Size Properties:**
- `Width` - Column width in pixels
- `MinWidth` - Minimum width when resizing

**Appearance Properties:**
- `Background` - Background brush for column header
- `BorderColor` - Border color for column
- `BorderStyle` - Border style (FixedSingle, Fixed3D, None)

### Multiple Columns Example

```csharp
// Create multiple columns for employee data
TreeColumnAdv nameColumn = new TreeColumnAdv 
{ 
    Text = "Name", 
    Width = 200 
};

TreeColumnAdv departmentColumn = new TreeColumnAdv 
{ 
    Text = "Department", 
    Width = 150 
};

TreeColumnAdv salaryColumn = new TreeColumnAdv 
{ 
    Text = "Salary", 
    Width = 100 
};

TreeColumnAdv locationColumn = new TreeColumnAdv 
{ 
    Text = "Location", 
    Width = 120 
};

// Add all columns at once
multiColumnTreeView1.Columns.AddRange(new TreeColumnAdv[] 
{
    nameColumn,
    departmentColumn,
    salaryColumn,
    locationColumn
});
```

### Auto-Sizing Columns

The control can automatically calculate column widths based on content:

```csharp
// Set auto-size mode
multiColumnTreeView1.AutoSizeMode = 
    Syncfusion.Windows.Forms.Tools.MultiColumnTreeView.AutoSizeMode.AllCellsExceptHeader;
```

**AutoSizeMode Options:**
- `None` - No auto-sizing (use specified widths)
- `AllCellsExceptHeader` - Calculate based on node content width

## TreeNodeAdv - Node Configuration

TreeNodeAdv represents each item in the tree. Nodes can have text, images, child nodes, and subitems for additional columns.

### Creating Nodes

```csharp
// Create node
TreeNodeAdv node = new TreeNodeAdv();
node.Text = "John Doe";
node.Tag = employeeObject;  // Store associated data

// Add to control
multiColumnTreeView1.Nodes.Add(node);
```

### Essential Node Properties

**Text and Data:**
- `Text` - Node text (displayed in first column)
- `Tag` - Associated data object
- `Font` - Text font
- `ForeColor` - Text color
- `Multiline` - Enable multiline text display

**Hierarchy:**
- `Nodes` - Child nodes collection
- `Parent` - Parent node reference
- `Level` - Depth level in tree (0 = root)

**State:**
- `Expanded` - Expand/collapse state
- `Enabled` - Enable/disable node
- `Visible` - Show/hide node

**Visual Elements:**
- `LeftImageIndices` - Image indices for left side
- `RightImageIndices` - Image indices for right side
- `LeftImagePadding` - Space around left images
- `RightImagePadding` - Space around right images

### Building Node Hierarchy

Create parent-child relationships:

```csharp
// Create parent
TreeNodeAdv managerNode = new TreeNodeAdv();
managerNode.Text = "Alice Smith (Manager)";

// Create children
TreeNodeAdv employee1 = new TreeNodeAdv();
employee1.Text = "Bob Johnson";

TreeNodeAdv employee2 = new TreeNodeAdv();
employee2.Text = "Carol Williams";

TreeNodeAdv employee3 = new TreeNodeAdv();
employee3.Text = "David Brown";

// Add children to parent
managerNode.Nodes.AddRange(new TreeNodeAdv[] 
{
    employee1,
    employee2,
    employee3
});

// Add parent to control
multiColumnTreeView1.Nodes.Add(managerNode);
```

### Nested Hierarchy (Multiple Levels)

```csharp
// Level 0 - Company
TreeNodeAdv companyNode = new TreeNodeAdv { Text = "Acme Corp" };

// Level 1 - Departments
TreeNodeAdv engineeringDept = new TreeNodeAdv { Text = "Engineering" };
TreeNodeAdv salesDept = new TreeNodeAdv { Text = "Sales" };

// Level 2 - Teams within Engineering
TreeNodeAdv backendTeam = new TreeNodeAdv { Text = "Backend Team" };
TreeNodeAdv frontendTeam = new TreeNodeAdv { Text = "Frontend Team" };

// Level 3 - Team members
TreeNodeAdv developer1 = new TreeNodeAdv { Text = "John Doe" };
TreeNodeAdv developer2 = new TreeNodeAdv { Text = "Jane Smith" };

// Build hierarchy
backendTeam.Nodes.AddRange(new TreeNodeAdv[] { developer1, developer2 });
engineeringDept.Nodes.AddRange(new TreeNodeAdv[] { backendTeam, frontendTeam });
companyNode.Nodes.AddRange(new TreeNodeAdv[] { engineeringDept, salesDept });

// Add to control
multiColumnTreeView1.Nodes.Add(companyNode);
```

## TreeNodeAdvSubItem - Additional Column Data

SubItems populate the additional columns for each node. Each node should have (column count - 1) subitems.

### Creating SubItems

```csharp
// Node for first column (Name)
TreeNodeAdv employeeNode = new TreeNodeAdv();
employeeNode.Text = "John Doe";

// SubItem for second column (Department)
TreeNodeAdvSubItem deptSubItem = new TreeNodeAdvSubItem();
deptSubItem.Text = "Engineering";

// SubItem for third column (Salary)
TreeNodeAdvSubItem salarySubItem = new TreeNodeAdvSubItem();
salarySubItem.Text = "$85,000";

// SubItem for fourth column (Location)
TreeNodeAdvSubItem locationSubItem = new TreeNodeAdvSubItem();
locationSubItem.Text = "New York";

// Add subitems in order
employeeNode.SubItems.Add(deptSubItem);
employeeNode.SubItems.Add(salarySubItem);
employeeNode.SubItems.Add(locationSubItem);
```

### SubItem Properties

**Text and Appearance:**
- `Text` - Content displayed in cell
- `ForeColor` - Text color
- `Font` - Text font
- `Background` - Cell background brush

**Visual Elements:**
- `LeftImage` - Image on left side
- `RightImage` - Image on right side
- `BorderColor` - Cell border color
- `BorderStyle` - Cell border style

**Additional:**
- `HelpText` - Tooltip text for cell
- `Tag` - Associated data object

### SubItem Styling Example

```csharp
TreeNodeAdv node = new TreeNodeAdv { Text = "High Priority Task" };

// Style department cell
TreeNodeAdvSubItem statusSubItem = new TreeNodeAdvSubItem();
statusSubItem.Text = "Urgent";
statusSubItem.ForeColor = Color.Red;
statusSubItem.Background = new Syncfusion.Drawing.BrushInfo(Color.LightYellow);

// Style due date cell
TreeNodeAdvSubItem dueDateSubItem = new TreeNodeAdvSubItem();
dueDateSubItem.Text = "2026-03-25";
dueDateSubItem.Font = new Font("Arial", 9, FontStyle.Bold);

node.SubItems.Add(statusSubItem);
node.SubItems.Add(dueDateSubItem);
```

## Dynamic Node Management

### Adding Nodes Dynamically

```csharp
// Add single node
TreeNodeAdv newNode = new TreeNodeAdv { Text = "New Item" };
multiColumnTreeView1.Nodes.Add(newNode);

// Add to specific parent
TreeNodeAdv parentNode = multiColumnTreeView1.Nodes[0];
parentNode.Nodes.Add(newNode);

// Insert at specific position
multiColumnTreeView1.Nodes.Insert(0, newNode); // Insert at beginning
```

### Removing Nodes

```csharp
// Remove specific node
TreeNodeAdv nodeToRemove = multiColumnTreeView1.Nodes[2];
multiColumnTreeView1.Nodes.Remove(nodeToRemove);

// Remove by index
multiColumnTreeView1.Nodes.RemoveAt(0);

// Remove all nodes
multiColumnTreeView1.Nodes.Clear();

// Remove children from parent
TreeNodeAdv parent = multiColumnTreeView1.Nodes[0];
parent.Nodes.Clear();
```

### Finding Nodes

```csharp
// Find by text
TreeNodeAdv FindNodeByText(TreeNodeAdvCollection nodes, string text)
{
    foreach (TreeNodeAdv node in nodes)
    {
        if (node.Text == text)
            return node;
        
        // Search children recursively
        TreeNodeAdv found = FindNodeByText(node.Nodes, text);
        if (found != null)
            return found;
    }
    return null;
}

// Usage
TreeNodeAdv result = FindNodeByText(multiColumnTreeView1.Nodes, "John Doe");
```

### Traversing All Nodes

```csharp
// Recursively visit all nodes
void TraverseNodes(TreeNodeAdvCollection nodes, Action<TreeNodeAdv> action)
{
    foreach (TreeNodeAdv node in nodes)
    {
        action(node);  // Process current node
        
        if (node.Nodes.Count > 0)
        {
            TraverseNodes(node.Nodes, action);  // Process children
        }
    }
}

// Usage - print all node texts
TraverseNodes(multiColumnTreeView1.Nodes, node => 
{
    Console.WriteLine($"Level {node.Level}: {node.Text}");
});
```

## Working with Node Collections

### Batch Operations with BeginUpdate/EndUpdate

For better performance when adding many nodes:

```csharp
multiColumnTreeView1.BeginUpdate();
try
{
    for (int i = 0; i < 1000; i++)
    {
        TreeNodeAdv node = new TreeNodeAdv { Text = $"Item {i}" };
        node.SubItems.Add(new TreeNodeAdvSubItem { Text = $"Value {i}" });
        multiColumnTreeView1.Nodes.Add(node);
    }
}
finally
{
    multiColumnTreeView1.EndUpdate();
}
```

### Counting Nodes

```csharp
// Count root nodes
int rootCount = multiColumnTreeView1.Nodes.Count;

// Count all nodes recursively
int CountAllNodes(TreeNodeAdvCollection nodes)
{
    int count = nodes.Count;
    foreach (TreeNodeAdv node in nodes)
    {
        count += CountAllNodes(node.Nodes);
    }
    return count;
}

int totalNodes = CountAllNodes(multiColumnTreeView1.Nodes);
```

## Practical Examples

### Example 1: Building a File System Tree

```csharp
void BuildFileSystemTree(string path, TreeNodeAdv parentNode)
{
    try
    {
        // Add directories
        foreach (string dir in Directory.GetDirectories(path))
        {
            DirectoryInfo dirInfo = new DirectoryInfo(dir);
            TreeNodeAdv dirNode = new TreeNodeAdv { Text = dirInfo.Name };
            
            // Add subitems for additional columns
            dirNode.SubItems.Add(new TreeNodeAdvSubItem { Text = "Folder" });
            dirNode.SubItems.Add(new TreeNodeAdvSubItem { Text = dirInfo.CreationTime.ToShortDateString() });
            dirNode.SubItems.Add(new TreeNodeAdvSubItem { Text = "" }); // Size (empty for folders)
            
            parentNode.Nodes.Add(dirNode);
            
            // Recursively add subdirectories
            BuildFileSystemTree(dir, dirNode);
        }
        
        // Add files
        foreach (string file in Directory.GetFiles(path))
        {
            FileInfo fileInfo = new FileInfo(file);
            TreeNodeAdv fileNode = new TreeNodeAdv { Text = fileInfo.Name };
            
            fileNode.SubItems.Add(new TreeNodeAdvSubItem { Text = fileInfo.Extension });
            fileNode.SubItems.Add(new TreeNodeAdvSubItem { Text = fileInfo.CreationTime.ToShortDateString() });
            fileNode.SubItems.Add(new TreeNodeAdvSubItem { Text = FormatFileSize(fileInfo.Length) });
            
            parentNode.Nodes.Add(fileNode);
        }
    }
    catch (UnauthorizedAccessException)
    {
        // Handle access denied
    }
}

string FormatFileSize(long bytes)
{
    string[] sizes = { "B", "KB", "MB", "GB", "TB" };
    double len = bytes;
    int order = 0;
    while (len >= 1024 && order < sizes.Length - 1)
    {
        order++;
        len = len / 1024;
    }
    return $"{len:0.##} {sizes[order]}";
}
```

### Example 2: Employee Organizational Chart

```csharp
class Employee
{
    public string Name { get; set; }
    public string Title { get; set; }
    public string Department { get; set; }
    public decimal Salary { get; set; }
    public List<Employee> DirectReports { get; set; } = new List<Employee>();
}

void BuildOrgChart(Employee employee, TreeNodeAdv parentNode)
{
    TreeNodeAdv empNode = new TreeNodeAdv();
    empNode.Text = employee.Name;
    empNode.Tag = employee;
    
    // Add employee details in additional columns
    empNode.SubItems.Add(new TreeNodeAdvSubItem { Text = employee.Title });
    empNode.SubItems.Add(new TreeNodeAdvSubItem { Text = employee.Department });
    empNode.SubItems.Add(new TreeNodeAdvSubItem { Text = employee.Salary.ToString("C") });
    
    if (parentNode == null)
    {
        multiColumnTreeView1.Nodes.Add(empNode);
    }
    else
    {
        parentNode.Nodes.Add(empNode);
    }
    
    // Recursively add direct reports
    foreach (Employee report in employee.DirectReports)
    {
        BuildOrgChart(report, empNode);
    }
}
```

## Best Practices

1. **Always add columns before nodes** - Columns define the structure
2. **Match subitem count to columns** - Use (column count - 1) subitems per node
3. **Use BeginUpdate/EndUpdate** - Essential for adding many nodes
4. **Store data in Tag property** - Keep reference to source objects
5. **Use meaningful node text** - First column should be primary identifier
6. **Handle exceptions** - When building from file system or database
7. **Dispose properly** - Clear nodes when form closes to release resources
8. **Test with large datasets** - Verify performance with realistic data volumes

## Common Issues

**Subitems not visible:**
- Verify column count matches subitem count + 1
- Ensure subitems added in correct order
- Check if columns were added before nodes

**Poor performance with many nodes:**
- Use BeginUpdate/EndUpdate
- Consider load on demand (see performance.md)
- Set SuspendExpandRecalculate = true

**Nodes not displaying hierarchy:**
- Verify nodes added to parent.Nodes, not control.Nodes
- Check if parent node is expanded
- Ensure ShowLines and ShowPlusMinus are true
