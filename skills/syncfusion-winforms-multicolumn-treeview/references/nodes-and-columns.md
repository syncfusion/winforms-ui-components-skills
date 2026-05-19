# Nodes and Columns Management

Working with TreeNodeAdv (nodes), TreeColumnAdv (columns), and TreeNodeAdvSubItem (subitems) to build hierarchical multi-column data.

## Creating Columns

```csharp
TreeColumnAdv column = new TreeColumnAdv 
{ 
    Text = "Employee Name",  // Header text
    Width = 200,             // Width in pixels
    TextColor = Color.Black, // Header text color
    Font = new Font("Arial", 10)
};
multiColumnTreeView1.Columns.Add(column);

// Add multiple columns
multiColumnTreeView1.Columns.AddRange(new TreeColumnAdv[] 
{
    new TreeColumnAdv { Text = "Name", Width = 200 },
    new TreeColumnAdv { Text = "Department", Width = 150 },
    new TreeColumnAdv { Text = "Salary", Width = 100 }
});

// Auto-size columns
multiColumnTreeView1.AutoSizeMode = AutoSizeMode.AllCellsExceptHeader;
```

## Creating Nodes

```csharp
TreeNodeAdv node = new TreeNodeAdv
{
    Text = "John Doe",           // Node text (first column)
    Tag = employeeObject,        // Store associated data
    Font = new Font("Arial", 9),
    TextColor = Color.Black
};
multiColumnTreeView1.Nodes.Add(node);
```

## Creating Node Hierarchy

```csharp
TreeNodeAdv managerNode = new TreeNodeAdv { Text = "Alice Smith (Manager)" };
TreeNodeAdv employee1 = new TreeNodeAdv { Text = "Bob Johnson" };
TreeNodeAdv employee2 = new TreeNodeAdv { Text = "Carol Williams" };

managerNode.Nodes.AddRange(new TreeNodeAdv[] { employee1, employee2 });
multiColumnTreeView1.Nodes.Add(managerNode);
```

## Adding SubItems

```csharp
TreeNodeAdv employeeNode = new TreeNodeAdv { Text = "John Doe" };

// Add subitems for additional columns (column count - 1)
employeeNode.SubItems.Add(new TreeNodeAdvSubItem { Text = "Engineering" });
employeeNode.SubItems.Add(new TreeNodeAdvSubItem { Text = "$85,000" });
employeeNode.SubItems.Add(new TreeNodeAdvSubItem { Text = "New York" });

// Style subitem
TreeNodeAdvSubItem statusSubItem = new TreeNodeAdvSubItem
{
    Text = "Urgent",
    TextColor = Color.Red,
    Background = new BrushInfo(Color.LightYellow),
    Font = new Font("Arial", 9, FontStyle.Bold)
};
employeeNode.SubItems.Add(statusSubItem);
```

## Dynamic Node Management

```csharp
// Add node
multiColumnTreeView1.Nodes.Add(new TreeNodeAdv { Text = "New Item" });

// Add to parent
TreeNodeAdv parentNode = multiColumnTreeView1.Nodes[0];
parentNode.Nodes.Add(new TreeNodeAdv { Text = "Child Item" });

// Remove node
multiColumnTreeView1.Nodes.Remove(nodeToRemove);
multiColumnTreeView1.Nodes.RemoveAt(0);
multiColumnTreeView1.Nodes.Clear();

// Insert at position
multiColumnTreeView1.Nodes.Insert(0, new TreeNodeAdv { Text = "First Item" });
```

## Finding Nodes

```csharp
TreeNodeAdv FindNodeByText(TreeNodeAdvCollection nodes, string text)
{
    foreach (TreeNodeAdv node in nodes)
    {
        if (node.Text == text) return node;
        TreeNodeAdv found = FindNodeByText(node.Nodes, text);
        if (found != null) return found;
    }
    return null;
}

// Usage
TreeNodeAdv result = FindNodeByText(multiColumnTreeView1.Nodes, "John Doe");
```

## Traversing All Nodes

```csharp
void TraverseNodes(TreeNodeAdvCollection nodes, Action<TreeNodeAdv> action)
{
    foreach (TreeNodeAdv node in nodes)
    {
        action(node);
        if (node.Nodes.Count > 0)
            TraverseNodes(node.Nodes, action);
    }
}

// Usage
TraverseNodes(multiColumnTreeView1.Nodes, node => Console.WriteLine($"Level {node.Level}: {node.Text}"));
```

## Batch Operations

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

## File System Tree Example

```csharp
void BuildFileSystemTree(string path, TreeNodeAdv parentNode)
{
    try
    {
        foreach (string dir in Directory.GetDirectories(path))
        {
            DirectoryInfo dirInfo = new DirectoryInfo(dir);
            TreeNodeAdv dirNode = new TreeNodeAdv { Text = dirInfo.Name };
            dirNode.SubItems.Add(new TreeNodeAdvSubItem { Text = "Folder" });
            dirNode.SubItems.Add(new TreeNodeAdvSubItem { Text = dirInfo.CreationTime.ToShortDateString() });
            parentNode.Nodes.Add(dirNode);
            BuildFileSystemTree(dir, dirNode);
        }
        
        foreach (string file in Directory.GetFiles(path))
        {
            FileInfo fileInfo = new FileInfo(file);
            TreeNodeAdv fileNode = new TreeNodeAdv { Text = fileInfo.Name };
            fileNode.SubItems.Add(new TreeNodeAdvSubItem { Text = fileInfo.Extension });
            fileNode.SubItems.Add(new TreeNodeAdvSubItem { Text = fileInfo.CreationTime.ToShortDateString() });
            fileNode.SubItems.Add(new TreeNodeAdvSubItem { Text = $"{fileInfo.Length / 1024} KB" });
            parentNode.Nodes.Add(fileNode);
        }
    }
    catch (UnauthorizedAccessException) { }
}
```

## Best Practices

- Always add columns before nodes
- Match subitem count to (column count - 1)
- Use `BeginUpdate/EndUpdate` when adding many nodes
- Store data in `Tag` property for reference
- Test with large datasets for performance
