# Events

Event system for node selection, editing, expand/collapse, checkboxes, and interactions.

## Node Selection Events

```csharp
this.multiColumnTreeView1.BeforeSelect += MultiColumnTreeView1_BeforeSelect;
this.multiColumnTreeView1.AfterSelect += MultiColumnTreeView1_AfterSelect;
this.multiColumnTreeView1.NodeHotTrackChanged += MultiColumnTreeView1_NodeHotTrackChanged;

// Before selection (cancelable)
private void MultiColumnTreeView1_BeforeSelect(object sender, Syncfusion.Windows.Forms.Tools.MultiColumnTreeView.TreeViewAdvCancelableSelectionEventArgs e)
{
    List<TreeNodeAdv> SelectedNodes = new List<TreeNodeAdv>();
    for (int i = 0; i < e.SelectedNodes.Count; i++)
    {
        TreeNodeAdv Node = e.SelectedNodes[i];
        SelectedNodes[i] = e.SelectedNodes[i];
    }
}

// After selection
private void MultiColumnTreeView1_AfterSelect(object sender, EventArgs e)
{
    MessageBox.Show("The Node is selected");
}

// Hot track (hover)
private void MultiColumnTreeView1_NodeHotTrackChanged(object sender, Syncfusion.Windows.Forms.Tools.MultiColumnTreeView.TreeViewAdvNodeEventArgs e)
{
    //Gets the node associated with this event
    TreeNodeAdv Node = e.Node;
}
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
this.multiColumnTreeView1.AfterInteractiveChecks += MultiColumnTreeView1_AfterInteractiveChecks;
this.multiColumnTreeView1.BeforeCheck += MultiColumnTreeView1_BeforeCheck;

// Before check (cancelable)
private void MultiColumnTreeView1_BeforeCheck(object sender, Syncfusion.Windows.Forms.Tools.MultiColumnTreeView.TreeNodeAdvBeforeCheckEventArgs e)
{
    //Gets the Node with this event
    TreeNodeAdv Node = e.Node;
    string CheckState = e.NewCheckState.ToString();
}

// After interactive checks (parent-child sync)
private void MultiColumnTreeView1_AfterInteractiveChecks(object sender, Syncfusion.Windows.Forms.Tools.MultiColumnTreeView.TreeNodeAdvEventArgs e)
{
    //Gets the Node with this event
    TreeNodeAdv Node = e.Node;
}
```

## Node Editing Events

```csharp
this.multiColumnTreeView1.BeforeEdit += MultiColumnTreeView1_BeforeEdit;

this.multiColumnTreeView1.NodeEditorValidated += MultiColumnTreeView1_NodeEditorValidated;

this.multiColumnTreeView1.NodeEditorValidating += MultiColumnTreeView1_NodeEditorValidating;

this.multiColumnTreeView1.NodeEditorValidateString += MultiColumnTreeView1_NodeEditorValidateString;

// Before edit (cancelable)
private void MultiColumnTreeView1_BeforeEdit(object sender, Syncfusion.Windows.Forms.Tools.MultiColumnTreeView.TreeNodeAdvBeforeEditEventArgs e)
{
    //The Node which is going to be edited
    TreeNodeAdv Node = e.Node;
}

// Validate during editing
private void MultiColumnTreeView1_NodeEditorValidating(object sender, Syncfusion.Windows.Forms.Tools.MultiColumnTreeView.TreeNodeAdvCancelableEditEventArgs e)
{
    e.ContinueEditing = true;
    string label = e.Label;
    TreeNodeAdv Node = e.Node;
};

// After successful edit
private void MultiColumnTreeView1_NodeEditorValidated(object sender, Syncfusion.Windows.Forms.Tools.MultiColumnTreeView.TreeNodeAdvEditEventArgs e)
{
    string label = e.Label;
    TreeNodeAdv Node = e.Node;
}

// Real-time validation
private void MultiColumnTreeView1_NodeEditorValidateString(object sender, Syncfusion.Windows.Forms.Tools.MultiColumnTreeView.TreeNodeAdvCancelableEditEventArgs e)
{
    e.ContinueEditing = true;
    string label = e.Label;
    TreeNodeAdv Node = e.Node;
}
```

## Custom Painting

```csharp
multiColumnTreeView1.OwnerDrawNodes = true;
this.multiColumnTreeView1.BeforeNodePaint += MultiColumnTreeView1_BeforeNodePaint;
this.multiColumnTreeView1.AfterNodePaint += MultiColumnTreeView1_AfterNodePaint;

// Before node paint
private void MultiColumnTreeView1_BeforeNodePaint(object sender, Syncfusion.Windows.Forms.Tools.MultiColumnTreeView.TreeNodeAdvPaintEventArgs e)
{
    //Node going to get painted
    TreeNodeAdv Node = e.Node;
}

// After node paint
private void MultiColumnTreeView1_AfterNodePaint(object sender, Syncfusion.Windows.Forms.Tools.MultiColumnTreeView.TreeNodeAdvPaintEventArgs e)
{
    //Node which got painted
    TreeNodeAdv Node = e.Node;
}
```

## Column Events

```csharp
this.multiColumnTreeView1.ColumnClick += MultiColumnTreeView1_ColumnClick;
this.multiColumnTreeView1.ColumnDoubleClick += MultiColumnTreeView1_ColumnDoubleClick;

// Column click
private void MultiColumnTreeView1_ColumnClick(object sender, TreeViewColumnSelectedChangedEventArgs e)
{
   MessageBox.show("The column selected is "+ e.Column.ToString());
}

// Column double-click
private void MultiColumnTreeView1_ColumnDoubleClick(object sender, TreeViewColumnSelectedChangedEventArgs e)
{
  MessageBox.show("The column selected is "+ e.Column.ToString());
}

// Column mouse events
multiColumnTreeView1.ColumnMouseDown += (sender, e) =>
{
    if (e.Button == MouseButtons.Right)
        ShowColumnContextMenu(e.Column);
};
```

## Mouse Events

```csharp
this.multiColumnTreeView1.ColumnMouseDown += MultiColumnTreeView1_ColumnMouseDown;
this.multiColumnTreeView1.ColumnMouseUp += MultiColumnTreeView1_ColumnMouseUp;

private void MultiColumnTreeView1_ColumnMouseDown(object sender, TreeColumnAdvMouseEventArgs e)
{
  MessageBox.show("The column selected is "+ e.Column.ToString()); 
}

private void MultiColumnTreeView1_ColumnMouseUp(object sender, TreeColumnAdvMouseEventArgs e)
{
  MessageBox.show("The column selected is "+ e.Column.ToString()); 
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
            menu.Items.Add("Rename", null, (_, __) => multiColumnTreeView1.BeginEdit());
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
