# Selection and Editing

This guide covers node selection modes, active node management, mouse and keyboard selection, and label editing capabilities.

## Selection Modes

The `SelectionMode` property defines how many nodes can be selected at once.

### Single Selection

Only one node can be selected at a time:

```csharp
multiColumnTreeView1.SelectionMode = 
    Syncfusion.Windows.Forms.Tools.MultiColumnTreeView.TreeSelectionMode.Single;
```

### MultiSelectSameLevel

Multiple nodes can be selected, but only if they're at the same tree level:

```csharp
multiColumnTreeView1.SelectionMode = 
    Syncfusion.Windows.Forms.Tools.MultiColumnTreeView.TreeSelectionMode.MultiSelectSameLevel;
```

### MultiSelectAll

Multiple nodes can be selected regardless of level:

```csharp
multiColumnTreeView1.SelectionMode = 
    Syncfusion.Windows.Forms.Tools.MultiColumnTreeView.TreeSelectionMode.MultiSelectAll;
```

## Working with Selected Nodes

### SelectedNode Property

Gets or sets the currently selected node (useful for single selection):

```csharp
// Get selected node
TreeNodeAdv selected = multiColumnTreeView1.SelectedNode;
if (selected != null)
{
    MessageBox.Show($"Selected: {selected.Text}");
}

// Set selected node programmatically
multiColumnTreeView1.SelectedNode = multiColumnTreeView1.Nodes[0].Nodes[1];
```

### SelectedNodes Collection

Gets all selected nodes (useful for multi-selection):

```csharp
// Get all selected nodes
TreeNodeAdvCollection selectedNodes = multiColumnTreeView1.SelectedNodes;

foreach (TreeNodeAdv node in selectedNodes)
{
    Console.WriteLine($"Selected: {node.Text}");
}

// Check selection count
int count = multiColumnTreeView1.SelectedNodes.Count;
MessageBox.Show($"{count} nodes selected");
```

### ActiveNode Property

The active node is the node that currently has focus (highlighted with focus rectangle):

```csharp
// Get active node
TreeNodeAdv active = multiColumnTreeView1.ActiveNode;

// Set active node
multiColumnTreeView1.ActiveNode = multiColumnTreeView1.Nodes[4].Nodes[1];
```

**Difference between SelectedNode and ActiveNode:**
- **SelectedNode** - The node(s) user has selected (may be multiple)
- **ActiveNode** - The node that currently has keyboard focus (only one)

## Mouse-Based Selection

Enable drag selection to select multiple nodes by dragging the mouse:

```csharp
// Enable mouse-based selection
multiColumnTreeView1.AllowMouseBasedSelection = true;

// Must also enable multi-selection mode
multiColumnTreeView1.SelectionMode = 
    Syncfusion.Windows.Forms.Tools.MultiColumnTreeView.TreeSelectionMode.MultiSelectAll;
```

**Usage:**
1. Click and hold mouse button
2. Drag across nodes
3. All nodes in drag region are selected

## Keyboard Search

Enable quick node navigation by typing:

```csharp
// Enable keyboard search
multiColumnTreeView1.AllowKeyboardSearch = true;
```

**Usage:**
- Type any letter to jump to first node starting with that letter
- Keep typing same letter to cycle through nodes starting with that letter
- Works with node text in the first column

## Label Editing

Allow users to edit node labels inline:

```csharp
// Enable label editing
multiColumnTreeView1.LabelEdit = true;
```

**User Actions:**
- Click on a selected node to enter edit mode
- Press F2 to edit the active node
- Press Enter to commit changes
- Press Escape to cancel editing

### Programmatically Start Editing

```csharp
// Make node editable and start editing
TreeNodeAdv node = multiColumnTreeView1.Nodes[0];
multiColumnTreeView1.BeginEdit(node);
```

### Handling Edit Events

Subscribe to editing events for validation and custom logic:

```csharp
// Before edit starts
multiColumnTreeView1.BeforeEdit += (sender, e) =>
{
    // e.Node - node being edited
    // e.Cancel = true to prevent editing
    
    if (e.Node.Text == "Read-Only")
    {
        e.Cancel = true; // Prevent editing
        MessageBox.Show("This node cannot be edited");
    }
};

// Validate new label
multiColumnTreeView1.NodeEditorValidating += (sender, e) =>
{
    // e.Label - new label text
    // e.Node - node being edited
    // e.ContinueEditing - set to true to keep editing
    // e.Cancel - set to true to reject changes
    
    if (string.IsNullOrWhiteSpace(e.Label))
    {
        MessageBox.Show("Node text cannot be empty");
        e.ContinueEditing = true; // Keep in edit mode
    }
};

// After successful edit
multiColumnTreeView1.NodeEditorValidated += (sender, e) =>
{
    // e.Node - edited node
    // e.Label - new label
    
    Console.WriteLine($"Node renamed to: {e.Label}");
};

// Validate on each text change
multiColumnTreeView1.NodeEditorValidateString += (sender, e) =>
{
    // Called for each TextChanged event
    // e.Label - current text
    
    // Real-time validation
    if (e.Label.Length > 50)
    {
        MessageBox.Show("Text too long");
        e.ContinueEditing = true;
    }
};
```

## Practical Examples

### Example 1: Multi-Selection with Actions

```csharp
void SetupMultiSelection()
{
    multiColumnTreeView1.SelectionMode = 
        Syncfusion.Windows.Forms.Tools.MultiColumnTreeView.TreeSelectionMode.MultiSelectAll;
    multiColumnTreeView1.AllowMouseBasedSelection = true;
    
    // Add context menu for selected nodes
    ContextMenuStrip menu = new ContextMenuStrip();
    menu.Items.Add("Delete Selected").Click += (s, e) => DeleteSelected();
    menu.Items.Add("Export Selected").Click += (s, e) => ExportSelected();
    
    multiColumnTreeView1.ContextMenuStrip = menu;
}

void DeleteSelected()
{
    // Get list of selected nodes (copy to avoid collection modification issues)
    List<TreeNodeAdv> toDelete = new List<TreeNodeAdv>();
    foreach (TreeNodeAdv node in multiColumnTreeView1.SelectedNodes)
    {
        toDelete.Add(node);
    }
    
    // Delete nodes
    foreach (TreeNodeAdv node in toDelete)
    {
        if (node.Parent == null)
        {
            multiColumnTreeView1.Nodes.Remove(node);
        }
        else
        {
            ((TreeNodeAdv)node.Parent).Nodes.Remove(node);
        }
    }
}

void ExportSelected()
{
    StringBuilder sb = new StringBuilder();
    foreach (TreeNodeAdv node in multiColumnTreeView1.SelectedNodes)
    {
        sb.AppendLine(node.Text);
    }
    
    Clipboard.SetText(sb.ToString());
    MessageBox.Show($"Exported {multiColumnTreeView1.SelectedNodes.Count} nodes");
}
```

### Example 2: Keyboard Search with Highlighting

```csharp
void SetupKeyboardSearch()
{
    multiColumnTreeView1.AllowKeyboardSearch = true;
    
    // Highlight search results
    multiColumnTreeView1.NodeHotTrackChanged += (sender, e) =>
    {
        if (e.Node != null)
        {
            // Node found by keyboard search
            e.Node.TextColor = Color.Blue;
            e.Node.Font = new Font(e.Node.Font, FontStyle.Bold);
        }
    };
}
```

### Example 3: Validated Label Editing

```csharp
void SetupLabelEditing()
{
    multiColumnTreeView1.LabelEdit = true;
    
    // Prevent editing of certain nodes
    multiColumnTreeView1.BeforeEdit += (sender, e) =>
    {
        // Don't allow editing root nodes
        if (e.Node.Level == 0)
        {
            e.Cancel = true;
            MessageBox.Show("Root nodes cannot be renamed");
        }
    };
    
    // Validate new names
    multiColumnTreeView1.NodeEditorValidating += (sender, e) =>
    {
        // Check for duplicates
        if (NodeNameExists(e.Node.Parent as TreeNodeAdv, e.Label, e.Node))
        {
            MessageBox.Show("A node with this name already exists");
            e.ContinueEditing = true;
        }
        
        // Check for invalid characters
        if (e.Label.IndexOfAny(new char[] { '/', '\\', ':', '*', '?', '"', '<', '>', '|' }) >= 0)
        {
            MessageBox.Show("Name contains invalid characters");
            e.ContinueEditing = true;
        }
    };
    
    // Update after successful edit
    multiColumnTreeView1.NodeEditorValidated += (sender, e) =>
    {
        // Update associated data object
        if (e.Node.Tag != null)
        {
            dynamic dataObj = e.Node.Tag;
            dataObj.Name = e.Label;
        }
        
        // Log change
        LogNodeRename(e.Node, e.Label);
    };
}

bool NodeNameExists(TreeNodeAdv parent, string name, TreeNodeAdv exclude)
{
    TreeNodeAdvCollection nodes = parent != null ? parent.Nodes : multiColumnTreeView1.Nodes;
    
    foreach (TreeNodeAdv node in nodes)
    {
        if (node != exclude && node.Text.Equals(name, StringComparison.OrdinalIgnoreCase))
            return true;
    }
    
    return false;
}

void LogNodeRename(TreeNodeAdv node, string newName)
{
    string path = GetNodePath(node);
    Console.WriteLine($"Renamed: {path} -> {newName}");
}

string GetNodePath(TreeNodeAdv node)
{
    List<string> parts = new List<string>();
    TreeNodeAdv current = node;
    
    while (current != null)
    {
        parts.Insert(0, current.Text);
        current = current.Parent as TreeNodeAdv;
    }
    
    return string.Join(" > ", parts);
}
```

### Example 4: Selection Change Notification

```csharp
void SetupSelectionEvents()
{
    multiColumnTreeView1.BeforeSelect += (sender, e) =>
    {
        // Before selection changes
        // e.SelectedNodes - nodes about to be selected
        // e.Cancel = true to prevent selection change
        
        Console.WriteLine($"About to select {e.SelectedNodes.Count} nodes");
    };
    
    multiColumnTreeView1.AfterSelect += (sender, e) =>
    {
        // After selection changed
        UpdateStatusBar();
        LoadNodeDetails();
    };
}

void UpdateStatusBar()
{
    int count = multiColumnTreeView1.SelectedNodes.Count;
    statusLabel.Text = $"{count} node(s) selected";
}

void LoadNodeDetails()
{
    if (multiColumnTreeView1.SelectedNode != null)
    {
        TreeNodeAdv node = multiColumnTreeView1.SelectedNode;
        
        // Display node details in a panel
        detailsTextBox.Text = $"Text: {node.Text}\n" +
                              $"Level: {node.Level}\n" +
                              $"Children: {node.Nodes.Count}\n" +
                              $"Checked: {node.Checked}";
    }
}
```

## Selection Patterns

### Select All Nodes

```csharp
void SelectAllNodes()
{
    multiColumnTreeView1.BeginUpdate();
    try
    {
        SelectNodesRecursive(multiColumnTreeView1.Nodes);
    }
    finally
    {
        multiColumnTreeView1.EndUpdate();
    }
}

void SelectNodesRecursive(TreeNodeAdvCollection nodes)
{
    foreach (TreeNodeAdv node in nodes)
    {
        multiColumnTreeView1.SelectedNodes.Add(node);
        
        if (node.Nodes.Count > 0)
            SelectNodesRecursive(node.Nodes);
    }
}
```

### Clear Selection

```csharp
void ClearSelection()
{
    multiColumnTreeView1.SelectedNodes.Clear();
}
```

### Select Siblings

```csharp
void SelectSiblings(TreeNodeAdv node)
{
    TreeNodeAdvCollection siblings = node.Parent != null ? 
        ((TreeNodeAdv)node.Parent).Nodes : 
        multiColumnTreeView1.Nodes;
    
    multiColumnTreeView1.SelectedNodes.Clear();
    foreach (TreeNodeAdv sibling in siblings)
    {
        multiColumnTreeView1.SelectedNodes.Add(sibling);
    }
}
```

## Best Practices

1. **Choose appropriate selection mode** for your use case (single vs. multi)
2. **Handle BeforeSelect** to validate or prevent selections
3. **Use SelectedNodes** for multi-selection scenarios
4. **Enable keyboard search** for better navigation in large trees
5. **Validate edits** to prevent invalid data entry
6. **Use BeginUpdate/EndUpdate** when programmatically selecting many nodes
7. **Handle edit events** to sync with data sources
8. **Provide visual feedback** for selection and active node states

## Common Issues

**Selection not working:**
- Verify SelectionMode is not set to Single when expecting multi-select
- Check if any event handlers are canceling the selection

**Label editing not starting:**
- Ensure LabelEdit = true
- Node might be selected but user needs to click again or press F2

**Keyboard search not finding nodes:**
- Verify AllowKeyboardSearch = true
- Search only works on node text in first column

**Multiple selection across levels not working:**
- Check SelectionMode - use MultiSelectAll instead of MultiSelectSameLevel
