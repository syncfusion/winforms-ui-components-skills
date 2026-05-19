# Advanced Features

## Table of Contents
- [Find and Replace](#find-and-replace)
- [History Manager](#history-manager)
- [Save and Load XML](#save-and-load-xml)
- [Printing](#printing)
- [Context Menus](#context-menus)
- [Multi-Selection](#multi-selection)

## Find and Replace

Search functionality for locating nodes by text or value.

### Find All

```csharp
List<TreeNodeAdv> results = new List<TreeNodeAdv>();

void FindAll(TreeNodeAdvCollection nodes, string searchText)
{
    foreach (TreeNodeAdv node in nodes)
    {
        if (node.Text.Contains(searchText, StringComparison.OrdinalIgnoreCase))
        {
            results.Add(node);
        }
        
        if (node.Nodes.Count > 0)
        {
            FindAll(node.Nodes, searchText);
        }
    }
}

// Usage
FindAll(treeViewAdv1.Nodes, "search");
```

## History Manager

Undo/redo functionality for tree modifications.

### Enable History Manager

```csharp
// Enable history tracking
treeViewAdv1.HistoryEnabled = true;
```

### Undo/Redo Operations

```csharp
// Undo last action
if (treeViewAdv1.HistoryManager.CanUndo)
{
    treeViewAdv1.HistoryManager.Undo();
}

// Redo action
if (treeViewAdv1.HistoryManager.CanRedo)
{
    treeViewAdv1.HistoryManager.Redo();
}

// Resets history.
treeViewAdv1.HistoryManager.Reset();
```

### History Manager Properties

```csharp
// Check if can undo/redo
bool canUndo = treeViewAdv1.HistoryManager.CanUndo;
bool canRedo = treeViewAdv1.HistoryManager.CanRedo;
```

## Save and Load XML

Persist tree structure to XML file.

### Save to XML

```csharp
// Save entire tree
treeViewAdv1.SaveToXML("tree.xml");

// Save specific node and children
TreeNodeAdv node = treeViewAdv1.SelectedNode;
node.SaveToXML("subtree.xml");
```

### Load from XML

```csharp
// Load from XML file
treeViewAdv1.LoadXML("tree.xml");

// Load into specific node
TreeNodeAdv parentNode = treeViewAdv1.Nodes[0];
parentNode.LoadXML("subtree.xml");
```

### XML Format Example

```xml
<TreeNodes>
  <Node Text="Root">
    <Node Text="Child 1" />
    <Node Text="Child 2">
      <Node Text="Grandchild" />
    </Node>
  </Node>
</TreeNodes>
```

### Save/Load with Node Properties

```csharp
// Custom XML serialization with properties
private void SaveTreeWithProperties()
{
    XmlDocument doc = new XmlDocument();
    XmlElement root = doc.CreateElement("TreeView");
    doc.AppendChild(root);
    
    foreach (TreeNodeAdv node in treeViewAdv1.Nodes)
    {
        SaveNodeToXml(node, root, doc);
    }
    
    doc.Save("tree_full.xml");
}

private void SaveNodeToXml(TreeNodeAdv node, XmlElement parent, XmlDocument doc)
{
    XmlElement element = doc.CreateElement("Node");
    element.SetAttribute("Text", node.Text);
    element.SetAttribute("Checked", node.Checked.ToString());
    element.SetAttribute("Tag", node.Tag?.ToString() ?? "");
    parent.AppendChild(element);
    
    foreach (TreeNodeAdv child in node.Nodes)
    {
        SaveNodeToXml(child, element, doc);
    }
}
```

## Printing

Print tree structure with customization.

### Basic Printing

```csharp
// Print preview
treeViewAdv1.PrintPreview();

// Direct print
treeViewAdv1.Print();
```
### Custom Print Dialog

```csharp
private void PrintTree()
{
    PrintDocument printDoc = new PrintDocument();
    printDoc.PrintPage += PrintDoc_PrintPage;
    
    PrintDialog printDialog = new PrintDialog();
    printDialog.Document = printDoc;
    
    if (printDialog.ShowDialog() == DialogResult.OK)
    {
        printDoc.Print();
    }
}

private void PrintDoc_PrintPage(object sender, PrintPageEventArgs e)
{
    // Custom print logic
    int y = 50;
    Font font = new Font("Arial", 10);
    
    foreach (TreeNodeAdv node in treeViewAdv1.Nodes)
    {
        PrintNode(node, e.Graphics, font, 50, ref y, 0);
    }
}

private void PrintNode(TreeNodeAdv node, Graphics g, Font font, int x, ref int y, int level)
{
    string indent = new string(' ', level * 4);
    g.DrawString(indent + node.Text, font, Brushes.Black, x, y);
    y += 20;
    
    foreach (TreeNodeAdv child in node.Nodes)
    {
        PrintNode(child, g, font, x, ref y, level + 1);
    }
}
```

## Context Menus

Associate context menus with nodes.

### Basic Context Menu

```csharp
// Create context menu
ContextMenuStrip contextMenu = new ContextMenuStrip();
contextMenu.Items.Add("Add Child", null, AddChild_Click);
contextMenu.Items.Add("Delete", null, Delete_Click);
contextMenu.Items.Add("Rename", null, Rename_Click);

// Assign to TreeViewAdv
treeViewAdv1.ContextMenuStrip = contextMenu;
```

### Context Menu Handlers

```csharp
private void AddChild_Click(object sender, EventArgs e)
{
    if (treeViewAdv1.SelectedNode != null)
    {
        TreeNodeAdv newNode = new TreeNodeAdv("New Item");
        treeViewAdv1.SelectedNode.Nodes.Add(newNode);
        treeViewAdv1.SelectedNode.Expand();
    }
}

private void Delete_Click(object sender, EventArgs e)
{
    if (treeViewAdv1.SelectedNode != null)
    {
        treeViewAdv1.SelectedNode.Remove();
    }
}

private void Rename_Click(object sender, EventArgs e)
{
    if (treeViewAdv1.SelectedNode != null)
    {
        treeViewAdv1.LabelEdit = true;
        treeViewAdv1.BeginEdit();
    }
}
```

## Multi-Selection

Select multiple nodes with CTRL and SHIFT keys.

### Enable Multi-Selection

```csharp
// Enable multi-select modes
treeViewAdv1.SelectionMode = TreeSelectionMode.MultiSelectAll;

// Available modes:
// - SingleSelection (default)
// - MultiSelectSameLevel
// - MultiSelectAll
```

### Get Selected Nodes

```csharp
// Get all selected nodes
TreeNodeAdv[] selectedNodes = treeViewAdv1.SelectedNodes.ToArray();

// Process selected nodes
foreach (TreeNodeAdv node in selectedNodes)
{
    Console.WriteLine(node.Text);
}
```

### Selection Events

```csharp
treeViewAdv1.SelectionChanged += (sender, e) =>
{
    Console.WriteLine($"{treeViewAdv1.SelectedNodes.Length} nodes selected");
};
```

### Programmatic Selection

```csharp
// Select multiple nodes
treeViewAdv1.SelectedNodes = new TreeNodeAdv[] 
{ 
    node1, 
    node2, 
    node3 
};

// Add to selection
treeViewAdv1.AddToSelectedNodes(newNode);

// Remove from selection
treeViewAdv1.RemoveFromSelectedNodes(node);

// Clear selection
treeViewAdv1.ClearSelection();
```

## Complete Example

```csharp
public class AdvancedFeaturesExample : Form
{
    private TreeViewAdv treeViewAdv1;
    private ContextMenuStrip contextMenu;
    
    public AdvancedFeaturesExample()
    {
        InitializeTree();
        SetupContextMenu();
        SetupAdvancedFeatures();
    }
    
    private void InitializeTree()
    {
        treeViewAdv1 = new TreeViewAdv();
        treeViewAdv1.Size = new Size(400, 500);
        treeViewAdv1.SelectionMode = TreeSelectionMode.MultiSelectAll;
        this.Controls.Add(treeViewAdv1);
    }
    
    private void SetupContextMenu()
    {
        contextMenu = new ContextMenuStrip();
        contextMenu.Items.Add("Add Child", null, (s, e) => AddChild());
        contextMenu.Items.Add("Delete", null, (s, e) => DeleteNode());
        contextMenu.Items.Add("Find...", null, (s, e) => ShowFindDialog());
        contextMenu.Items.Add(new ToolStripSeparator());
        contextMenu.Items.Add("Save to XML", null, (s, e) => SaveToXml());
        contextMenu.Items.Add("Print", null, (s, e) => PrintTree());
        
        treeViewAdv1.ContextMenuStrip = contextMenu;
    }
    
    private void SetupAdvancedFeatures()
    {
        // Enable history
        treeViewAdv1.HistoryEnabled = true;
        
        // Keyboard shortcuts
        this.KeyPreview = true;
        this.KeyDown += (s, e) =>
        {
            if (e.Control && e.KeyCode == Keys.Z)
                treeViewAdv1.HistoryManager.Undo();
            else if (e.Control && e.KeyCode == Keys.Y)
                treeViewAdv1.HistoryManager.Redo();
            else if (e.Control && e.KeyCode == Keys.F)
                ShowFindDialog();
            else if (e.Control && e.KeyCode == Keys.P)
                PrintTree();
        };
    }
    
    private void AddChild()
    {
        if (treeViewAdv1.SelectedNode != null)
        {
            TreeNodeAdv newNode = new TreeNodeAdv("New Item");
            treeViewAdv1.SelectedNode.Nodes.Add(newNode);
            treeViewAdv1.SelectedNode.Expand();
        }
    }
    
    private void DeleteNode()
    {
        foreach (TreeNodeAdv node in treeViewAdv1.SelectedNodes)
        {
            node.Remove();
        }
    }
    
    private void ShowFindDialog()
    {
        string search = Microsoft.VisualBasic.Interaction.InputBox(
            "Enter search text:", "Find Node", "");
        
        if (!string.IsNullOrEmpty(search))
        {
            TreeNodeAdv found = treeViewAdv1.Find(search, false);
            if (found != null)
            {
                treeViewAdv1.SelectedNode = found;
                found.EnsureVisible();
            }
            else
            {
                MessageBox.Show("Node not found");
            }
        }
    }
    
    private void SaveToXml()
    {
        using (SaveFileDialog dialog = new SaveFileDialog())
        {
            dialog.Filter = "XML Files|*.xml";
            if (dialog.ShowDialog() == DialogResult.OK)
            {
                treeViewAdv1.SaveToXML(dialog.FileName);
            }
        }
    }
    
    private void PrintTree()
    {
        treeViewAdv1.PrintPreview();
    }
}
```

## Troubleshooting

**Issue:** Find not working
- **Solution:** Ensure search text matches node Text property exactly (consider case sensitivity)

**Issue:** Undo/Redo not available
- **Solution:** Enable HistoryManager: `treeViewAdv1.HistoryEnabled = true`;

**Issue:** XML save/load fails
- **Solution:** Check file path permissions, ensure XML format is valid

**Issue:** Multi-selection not working
- **Solution:** Set `SelectionMode = TreeSelectionMode.MultiSelectAll`

**Issue:** Context menu not appearing
- **Solution:** Assign menu to `ContextMenuStrip` property, handle NodeMouseClick for node-specific menus
