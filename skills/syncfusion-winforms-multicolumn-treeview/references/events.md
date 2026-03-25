# Events

This guide covers the event system in MultiColumnTreeView, including node selection, editing, expand/collapse, and other interactions.

## Node Selection Events

Handle node selection changes:

### BeforeSelect

Occurs before a node is selected. Cancel to prevent selection:

```csharp
multiColumnTreeView1.BeforeSelect += (sender, e) =>
{
    // e.SelectedNodes - nodes about to be selected
    // e.Cancel = true to prevent selection
    
    foreach (TreeNodeAdv node in e.SelectedNodes)
    {
        // Prevent selecting disabled nodes
        if (!node.Enabled)
        {
            e.Cancel = true;
            MessageBox.Show("Cannot select disabled nodes");
            return;
        }
    }
};
```

### AfterSelect

Occurs after a node is selected:

```csharp
multiColumnTreeView1.AfterSelect += (sender, e) =>
{
    if (multiColumnTreeView1.SelectedNode != null)
    {
        string text = multiColumnTreeView1.SelectedNode.Text;
        UpdateDetailsPanel(text);
    }
};
```

### NodeHotTrackChanged

Occurs when mouse hovers over a node:

```csharp
multiColumnTreeView1.NodeHotTrackChanged += (sender, e) =>
{
    if (e.Node != null)
    {
        statusLabel.Text = $"Hovering: {e.Node.Text}";
    }
};
```

## Node Expand/Collapse Events

### BeforeExpand

Occurs before a node is expanded. Use for load on demand:

```csharp
multiColumnTreeView1.BeforeExpand += (sender, e) =>
{
    // e.Node - node being expanded
    // e.Cancel = true to prevent expansion
    
    if (!e.Node.ExpandedOnce)
    {
        LoadChildNodes(e.Node);
    }
};
```

### AfterExpand

Occurs after a node is expanded:

```csharp
multiColumnTreeView1.AfterExpand += (sender, e) =>
{
    Console.WriteLine($"Expanded: {e.Node.Text}");
};
```

### BeforeCollapse

Occurs before a node is collapsed:

```csharp
multiColumnTreeView1.BeforeCollapse += (sender, e) =>
{
    // Prevent collapsing of important nodes
    if (e.Node.Tag as string == "important")
    {
        e.Cancel = true;
    }
};
```

## Checkbox Events

### BeforeCheck

Occurs before a node's checkbox state changes:

```csharp
multiColumnTreeView1.BeforeCheck += (sender, e) =>
{
    // e.Node - node being checked/unchecked
    // e.NewCheckState - new state (Checked, Unchecked, Indeterminate)
    // e.Cancel = true to prevent change
    
    Console.WriteLine($"{e.Node.Text} changing to {e.NewCheckState}");
};
```

### AfterCheck

Occurs after checkbox state changes:

```csharp
multiColumnTreeView1.AfterCheck += (sender, e) =>
{
    UpdateSelectionCount();
};
```

### AfterInteractiveChecks

Occurs after interactive checkbox updates (parent-child synchronization):

```csharp
multiColumnTreeView1.AfterInteractiveChecks += (sender, e) =>
{
    Console.WriteLine($"Interactive check completed for: {e.Node.Text}");
};
```

## Node Editing Events

### BeforeEdit

Occurs before a node enters edit mode:

```csharp
multiColumnTreeView1.BeforeEdit += (sender, e) =>
{
    // Prevent editing of certain nodes
    if (e.Node.Level == 0)
    {
        e.Cancel = true;
        MessageBox.Show("Root nodes cannot be edited");
    }
};
```

### NodeEditorValidating

Validate new text during editing:

```csharp
multiColumnTreeView1.NodeEditorValidating += (sender, e) =>
{
    // e.Label - new text
    // e.ContinueEditing = true to keep in edit mode
    
    if (string.IsNullOrWhiteSpace(e.Label))
    {
        MessageBox.Show("Name cannot be empty");
        e.ContinueEditing = true;
    }
};
```

### NodeEditorValidated

Occurs after successful edit:

```csharp
multiColumnTreeView1.NodeEditorValidated += (sender, e) =>
{
    Console.WriteLine($"Node renamed to: {e.Label}");
    
    // Update data source
    if (e.Node.Tag != null)
    {
        UpdateDatabase(e.Node.Tag, e.Label);
    }
};
```

### NodeEditorValidateString

Occurs on each TextChanged event in the editor:

```csharp
multiColumnTreeView1.NodeEditorValidateString += (sender, e) =>
{
    // Real-time validation
    if (e.Label.Length > 50)
    {
        MessageBox.Show("Maximum 50 characters");
        e.ContinueEditing = true;
    }
};
```

## Node Painting Events

Custom draw nodes:

### BeforeNodePaint

Draw custom content before node rendering:

```csharp
multiColumnTreeView1.OwnerDrawNodes = true;

multiColumnTreeView1.BeforeNodePaint += (sender, e) =>
{
    // e.Node - node being painted
    // e.Graphics - graphics object
    // Custom drawing here
};
```

### AfterNodePaint

Draw custom content after node rendering:

```csharp
multiColumnTreeView1.AfterNodePaint += (sender, e) =>
{
    // Add custom overlay or highlight
    if (e.Node.Tag as string == "highlight")
    {
        e.Graphics.DrawRectangle(Pens.Red, e.Bounds);
    }
};
```

## Column Events

### ColumnClick

Occurs when a column header is clicked:

```csharp
multiColumnTreeView1.ColumnClick += (sender, e) =>
{
    // e.Column - clicked column
    SortByColumn(e.Column);
};
```

### ColumnDoubleClick

Occurs when a column header is double-clicked:

```csharp
multiColumnTreeView1.ColumnDoubleClick += (sender, e) =>
{
    // Auto-size column
    e.Column.Width = CalculateOptimalWidth(e.Column);
};
```

### ColumnMouseDown / ColumnMouseUp

Handle mouse events on column headers:

```csharp
multiColumnTreeView1.ColumnMouseDown += (sender, e) =>
{
    if (e.Button == MouseButtons.Right)
    {
        ShowColumnContextMenu(e.Column);
    }
};
```

## Mouse Events

### MouseDown / MouseUp / MouseMove

Standard mouse events:

```csharp
multiColumnTreeView1.MouseDown += (sender, e) =>
{
    if (e.Button == MouseButtons.Right)
    {
        TreeNodeAdv node = multiColumnTreeView1.GetNodeAtPoint(e.Location);
        if (node != null)
        {
            ShowNodeContextMenu(node, e.Location);
        }
    }
};
```

## Practical Examples

### Example 1: Confirmation on Delete

```csharp
private void SetupDeleteConfirmation()
{
    multiColumnTreeView1.KeyDown += (sender, e) =>
    {
        if (e.KeyCode == Keys.Delete && multiColumnTreeView1.SelectedNode != null)
        {
            var result = MessageBox.Show(
                $"Delete '{multiColumnTreeView1.SelectedNode.Text}'?",
                "Confirm Delete",
                MessageBoxButtons.YesNo);
            
            if (result == DialogResult.Yes)
            {
                DeleteSelectedNode();
            }
        }
    };
}
```

### Example 2: Audit Trail

```csharp
private void SetupAuditTrail()
{
    multiColumnTreeView1.AfterCheck += (sender, e) =>
    {
        LogAudit($"Checkbox changed: {e.Node.Text} = {e.Node.Checked}");
    };
    
    multiColumnTreeView1.NodeEditorValidated += (sender, e) =>
    {
        LogAudit($"Node renamed: {e.Node.Text} → {e.Label}");
    };
    
    multiColumnTreeView1.BeforeExpand += (sender, e) =>
    {
        LogAudit($"Node expanded: {e.Node.Text}");
    };
}

private void LogAudit(string message)
{
    string entry = $"[{DateTime.Now:yyyy-MM-dd HH:mm:ss}] {message}";
    auditLog.AppendText(entry + Environment.NewLine);
}
```

### Example 3: Context Menu

```csharp
private void SetupContextMenu()
{
    ContextMenuStrip menu = new ContextMenuStrip();
    
    menu.Opening += (s, e) =>
    {
        menu.Items.Clear();
        
        if (multiColumnTreeView1.SelectedNode != null)
        {
            menu.Items.Add("Rename", null, (_, __) => 
                multiColumnTreeView1.SelectedNode.BeginEdit());
            menu.Items.Add("Delete", null, (_, __) => 
                DeleteSelectedNode());
            menu.Items.Add("-"); // Separator
            menu.Items.Add("Expand All", null, (_, __) => 
                multiColumnTreeView1.SelectedNode.ExpandAll());
            menu.Items.Add("Collapse All", null, (_, __) => 
                multiColumnTreeView1.SelectedNode.CollapseAll());
        }
    };
    
    multiColumnTreeView1.ContextMenuStrip = menu;
}
```

### Example 4: Drag and Drop

```csharp
private void SetupDragDrop()
{
    multiColumnTreeView1.AllowDrop = true;
    TreeNodeAdv draggedNode = null;
    
    multiColumnTreeView1.ItemDrag += (sender, e) =>
    {
        draggedNode = e.Item as TreeNodeAdv;
        if (draggedNode != null)
        {
            multiColumnTreeView1.DoDragDrop(draggedNode, DragDropEffects.Move);
        }
    };
    
    multiColumnTreeView1.DragOver += (sender, e) =>
    {
        TreeNodeAdv targetNode = multiColumnTreeView1.GetNodeAtPoint(
            multiColumnTreeView1.PointToClient(new Point(e.X, e.Y)));
        
        e.Effect = (targetNode != null && targetNode != draggedNode) ?
            DragDropEffects.Move : DragDropEffects.None;
    };
    
    multiColumnTreeView1.DragDrop += (sender, e) =>
    {
        Point targetPoint = multiColumnTreeView1.PointToClient(new Point(e.X, e.Y));
        TreeNodeAdv targetNode = multiColumnTreeView1.GetNodeAtPoint(targetPoint);
        
        if (targetNode != null && draggedNode != null && targetNode != draggedNode)
        {
            // Remove from old location
            if (draggedNode.Parent == null)
                multiColumnTreeView1.Nodes.Remove(draggedNode);
            else
                ((TreeNodeAdv)draggedNode.Parent).Nodes.Remove(draggedNode);
            
            // Add to new location
            targetNode.Nodes.Add(draggedNode);
            targetNode.Expand();
        }
    };
}
```

## Event Order

Typical event sequence for common operations:

**Node Selection:**
1. BeforeSelect
2. AfterSelect
3. NodeHotTrackChanged (if hovering)

**Checkbox Toggle:**
1. BeforeCheck
2. AfterCheck
3. AfterInteractiveChecks (if interactive)

**Node Editing:**
1. BeforeEdit
2. NodeEditorValidateString (multiple times during typing)
3. NodeEditorValidating
4. NodeEditorValidated

**Node Expansion:**
1. BeforeExpand
2. (Child nodes loaded if load on demand)
3. AfterExpand

## Best Practices

1. **Cancel unwanted actions** in Before* events
2. **Update UI** in After* events
3. **Validate input** in editor events
4. **Use BeginUpdate/EndUpdate** when modifying multiple nodes in event handlers
5. **Avoid heavy operations** in frequently-fired events (NodeHotTrackChanged, Paint)
6. **Check for null** before accessing event properties
7. **Unsubscribe events** when disposing forms to prevent memory leaks

## Common Patterns

### Pattern: Cascade Selection

```csharp
private void CascadeSelection()
{
    multiColumnTreeView1.AfterCheck += (sender, e) =>
    {
        multiColumnTreeView1.BeginUpdate();
        try
        {
            CheckAllChildren(e.Node, e.Node.Checked);
        }
        finally
        {
            multiColumnTreeView1.EndUpdate();
        }
    };
}

private void CheckAllChildren(TreeNodeAdv node, bool isChecked)
{
    foreach (TreeNodeAdv child in node.Nodes)
    {
        child.Checked = isChecked;
        CheckAllChildren(child, isChecked);
    }
}
```

### Pattern: Auto-save on Edit

```csharp
private void SetupAutoSave()
{
    multiColumnTreeView1.NodeEditorValidated += async (sender, e) =>
    {
        try
        {
            await SaveNodeToDatabase(e.Node, e.Label);
            statusLabel.Text = "Saved successfully";
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Save failed: {ex.Message}");
        }
    };
}
```

## Troubleshooting

**Events not firing:**
- Verify event subscription
- Check if operation is being canceled in Before* event
- Ensure control is properly initialized

**Multiple event firings:**
- Some events may fire multiple times (e.g., NodeEditorValidateString)
- Use flags to prevent recursive event handling
- Unsubscribe temporarily if needed

**Performance issues:**
- Avoid heavy processing in frequently-fired events
- Use BeginUpdate/EndUpdate when modifying multiple nodes
- Consider debouncing for text change events
