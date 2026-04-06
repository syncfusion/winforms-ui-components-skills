# Events

Event system for node selection, editing, expand/collapse, checkboxes, and interactions.

## Node Selection Events

```csharp
// Before selection (cancelable)
multiColumnTreeView1.BeforeSelect += (sender, e) =>
{
    foreach (TreeNodeAdv node in e.SelectedNodes)
    {
        if (!node.Enabled)
        {
            e.Cancel = true;
            MessageBox.Show("Cannot select disabled nodes");
            return;
        }
    }
};

// After selection
multiColumnTreeView1.AfterSelect += (sender, e) =>
{
    if (multiColumnTreeView1.SelectedNode != null)
        UpdateDetailsPanel(multiColumnTreeView1.SelectedNode.Text);
};

// Hot track (hover)
multiColumnTreeView1.NodeHotTrackChanged += (sender, e) =>
{
    if (e.Node != null)
        statusLabel.Text = $"Hovering: {e.Node.Text}";
};
```

## Expand/Collapse Events

```csharp
// Before expand (load on demand)
multiColumnTreeView1.BeforeExpand += (sender, e) =>
{
    if (!e.Node.ExpandedOnce)
        LoadChildNodes(e.Node);
};

// After expand
multiColumnTreeView1.AfterExpand += (sender, e) =>
{
    Console.WriteLine($"Expanded: {e.Node.Text}");
};

// Before collapse (cancelable)
multiColumnTreeView1.BeforeCollapse += (sender, e) =>
{
    if (e.Node.Tag as string == "important")
        e.Cancel = true;
};
```

## Checkbox Events

```csharp
// Before check (cancelable)
multiColumnTreeView1.BeforeCheck += (sender, e) =>
{
    Console.WriteLine($"{e.Node.Text} changing to {e.NewCheckState}");
};

// After check
multiColumnTreeView1.AfterCheck += (sender, e) =>
{
    UpdateSelectionCount();
};

// After interactive checks (parent-child sync)
multiColumnTreeView1.AfterInteractiveChecks += (sender, e) =>
{
    Console.WriteLine($"Interactive check completed for: {e.Node.Text}");
};
```

## Node Editing Events

```csharp
// Before edit (cancelable)
multiColumnTreeView1.BeforeEdit += (sender, e) =>
{
    if (e.Node.Level == 0)
    {
        e.Cancel = true;
        MessageBox.Show("Root nodes cannot be edited");
    }
};

// Validate during editing
multiColumnTreeView1.NodeEditorValidating += (sender, e) =>
{
    if (string.IsNullOrWhiteSpace(e.Label))
    {
        MessageBox.Show("Name cannot be empty");
        e.ContinueEditing = true;
    }
};

// After successful edit
multiColumnTreeView1.NodeEditorValidated += (sender, e) =>
{
    Console.WriteLine($"Node renamed to: {e.Label}");
    if (e.Node.Tag != null)
        UpdateDatabase(e.Node.Tag, e.Label);
};

// Real-time validation
multiColumnTreeView1.NodeEditorValidateString += (sender, e) =>
{
    if (e.Label.Length > 50)
    {
        MessageBox.Show("Maximum 50 characters");
        e.ContinueEditing = true;
    }
};
```

## Custom Painting

```csharp
multiColumnTreeView1.OwnerDrawNodes = true;

// Before node paint
multiColumnTreeView1.BeforeNodePaint += (sender, e) =>
{
    // Custom drawing before node
};

// After node paint
multiColumnTreeView1.AfterNodePaint += (sender, e) =>
{
    if (e.Node.Tag as string == "highlight")
        e.Graphics.DrawRectangle(Pens.Red, e.Bounds);
};
```

## Column Events

```csharp
// Column click
multiColumnTreeView1.ColumnClick += (sender, e) =>
{
    SortByColumn(e.Column);
};

// Column double-click
multiColumnTreeView1.ColumnDoubleClick += (sender, e) =>
{
    e.Column.Width = CalculateOptimalWidth(e.Column);
};

// Column mouse events
multiColumnTreeView1.ColumnMouseDown += (sender, e) =>
{
    if (e.Button == MouseButtons.Right)
        ShowColumnContextMenu(e.Column);
};
```

## Mouse Events

```csharp
multiColumnTreeView1.MouseDown += (sender, e) =>
{
    if (e.Button == MouseButtons.Right)
    {
        TreeNodeAdv node = multiColumnTreeView1.GetNodeAtPoint(e.Location);
        if (node != null)
            ShowNodeContextMenu(node, e.Location);
    }
};
```

## Confirmation on Delete

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
                DeleteSelectedNode();
        }
    };
}
```

## Audit Trail

```csharp
private void SetupAuditTrail()
{
    multiColumnTreeView1.AfterCheck += (sender, e) =>
        LogAudit($"Checkbox changed: {e.Node.Text} = {e.Node.Checked}");
    
    multiColumnTreeView1.NodeEditorValidated += (sender, e) =>
        LogAudit($"Node renamed: {e.Node.Text} → {e.Label}");
    
    multiColumnTreeView1.BeforeExpand += (sender, e) =>
        LogAudit($"Node expanded: {e.Node.Text}");
}

private void LogAudit(string message)
{
    string entry = $"[{DateTime.Now:yyyy-MM-dd HH:mm:ss}] {message}";
    auditLog.AppendText(entry + Environment.NewLine);
}
```

## Context Menu

```csharp
private void SetupContextMenu()
{
    ContextMenuStrip menu = new ContextMenuStrip();
    
    menu.Opening += (s, e) =>
    {
        menu.Items.Clear();
        if (multiColumnTreeView1.SelectedNode != null)
        {
            menu.Items.Add("Rename", null, (_, __) => multiColumnTreeView1.SelectedNode.BeginEdit());
            menu.Items.Add("Delete", null, (_, __) => DeleteSelectedNode());
            menu.Items.Add("-");
            menu.Items.Add("Expand All", null, (_, __) => multiColumnTreeView1.SelectedNode.ExpandAll());
            menu.Items.Add("Collapse All", null, (_, __) => multiColumnTreeView1.SelectedNode.CollapseAll());
        }
    };
    
    multiColumnTreeView1.ContextMenuStrip = menu;
}
```

## Drag and Drop

```csharp
private void SetupDragDrop()
{
    multiColumnTreeView1.AllowDrop = true;
    TreeNodeAdv draggedNode = null;
    
    multiColumnTreeView1.ItemDrag += (sender, e) =>
    {
        draggedNode = e.Item as TreeNodeAdv;
        if (draggedNode != null)
            multiColumnTreeView1.DoDragDrop(draggedNode, DragDropEffects.Move);
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
            if (draggedNode.Parent == null)
                multiColumnTreeView1.Nodes.Remove(draggedNode);
            else
                ((TreeNodeAdv)draggedNode.Parent).Nodes.Remove(draggedNode);
            
            targetNode.Nodes.Add(draggedNode);
            targetNode.Expand();
        }
    };
}
```

## Event Order

**Node Selection:** BeforeSelect → AfterSelect → NodeHotTrackChanged

**Checkbox Toggle:** BeforeCheck → AfterCheck → AfterInteractiveChecks

**Node Editing:** BeforeEdit → NodeEditorValidateString (multiple) → NodeEditorValidating → NodeEditorValidated

**Node Expansion:** BeforeExpand → (load children if needed) → AfterExpand

## Best Practices

- Cancel unwanted actions in Before* events
- Update UI in After* events
- Validate input in editor events
- Use `BeginUpdate/EndUpdate` when modifying multiple nodes in event handlers
- Avoid heavy operations in frequently-fired events
- Check for null before accessing event properties
- Unsubscribe events when disposing forms
