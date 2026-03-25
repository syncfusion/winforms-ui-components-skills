# Data Binding

This guide covers loading and saving MultiColumnTreeView data from XML files, including data mapping and import/export operations.

## Overview

MultiColumnTreeView does not have direct data binding to databases or collections, but provides XML-based data persistence. You can load tree structures from XML files and save them back to XML format.

## Loading from XML

Load tree data from an XML file:

```csharp
private void LoadTreeViewFromXML()
{
    XmlDocument xDoc = new XmlDocument();
    xDoc.Load("TreeView.xml");
    
    // Clear existing nodes
    multiColumnTreeView1.Nodes.Clear();
    
    // Add root node from XML document element
    multiColumnTreeView1.Nodes.Add(new TreeNodeAdv(xDoc.DocumentElement.Name));
    
    TreeNodeAdv rootNode = (TreeNodeAdv)multiColumnTreeView1.Nodes[0];
    
    // Load child nodes recursively
    LoadFromXML(xDoc.DocumentElement, rootNode);
    
    // Expand all nodes to show structure
    multiColumnTreeView1.ExpandAll();
}

private void LoadFromXML(XmlNode xmlNode, TreeNodeAdv treeNode)
{
    if (xmlNode.HasChildNodes)
    {
        XmlNodeList xNodeList = xmlNode.ChildNodes;
        
        for (int i = 0; i < xNodeList.Count; i++)
        {
            XmlNode xNode = xmlNode.ChildNodes[i];
            
            // Create tree node from XML node
            treeNode.Nodes.Add(new TreeNodeAdv(xNode.Name));
            TreeNodeAdv childTreeNode = treeNode.Nodes[i];
            
            // Recursively load children
            LoadFromXML(xNode, childTreeNode);
        }
    }
    else
    {
        // Leaf node - set text to XML content
        treeNode.Text = xmlNode.OuterXml.Trim();
    }
}
```

**VB.NET Version:**

```vb
Private Sub LoadTreeViewFromXML()
    Dim xDoc As XmlDocument = New XmlDocument()
    xDoc.Load("TreeView.xml")
    
    multiColumnTreeView1.Nodes.Clear()
    multiColumnTreeView1.Nodes.Add(New TreeNodeAdv(xDoc.DocumentElement.Name))
    
    Dim rootNode As TreeNodeAdv = CType(multiColumnTreeView1.Nodes(0), TreeNodeAdv)
    LoadFromXML(xDoc.DocumentElement, rootNode)
    multiColumnTreeView1.ExpandAll()
End Sub

Private Sub LoadFromXML(ByVal xmlNode As XmlNode, ByVal treeNode As TreeNodeAdv)
    If xmlNode.HasChildNodes Then
        Dim xNodeList As XmlNodeList = xmlNode.ChildNodes
        
        For i As Integer = 0 To xNodeList.Count - 1
            Dim xNode As XmlNode = xmlNode.ChildNodes(i)
            treeNode.Nodes.Add(New TreeNodeAdv(xNode.Name))
            Dim childTreeNode As TreeNodeAdv = treeNode.Nodes(i)
            LoadFromXML(xNode, childTreeNode)
        Next
    Else
        treeNode.Text = xmlNode.OuterXml.Trim()
    End If
End Sub
```

## Saving to XML

Export tree structure to an XML file:

```csharp
private XmlTextWriter xmlTextWriter;

public void ExportToXML(MultiColumnTreeView tree, string filename)
{
    xmlTextWriter = new XmlTextWriter(filename, System.Text.Encoding.UTF8);
    xmlTextWriter.WriteStartDocument();
    
    // Write root element
    xmlTextWriter.WriteStartElement(multiColumnTreeView1.Nodes[0].Text);
    
    // Save all nodes
    foreach (TreeNodeAdv node in tree.Nodes)
    {
        SaveToXML(node.Nodes);
    }
    
    xmlTextWriter.WriteEndElement();
    xmlTextWriter.Close();
}

private void SaveToXML(TreeNodeAdvCollection nodes)
{
    foreach (TreeNodeAdv node in nodes)
    {
        if (node.Nodes.Count > 0)
        {
            // Node with children - create element
            xmlTextWriter.WriteStartElement(node.Text);
            SaveToXML(node.Nodes); // Recursively save children
            xmlTextWriter.WriteEndElement();
        }
        else
        {
            // Leaf node - write string
            xmlTextWriter.WriteString(node.Text);
        }
    }
}
```

**VB.NET Version:**

```vb
Private xmlTextWriter As XmlTextWriter

Public Sub ExportToXML(ByVal tree As MultiColumnTreeView, ByVal filename As String)
    xmlTextWriter = New XmlTextWriter(filename, System.Text.Encoding.UTF8)
    xmlTextWriter.WriteStartDocument()
    xmlTextWriter.WriteStartElement(multiColumnTreeView1.Nodes(0).Text)
    
    For Each node As TreeNodeAdv In tree.Nodes
        SaveToXML(node.Nodes)
    Next
    
    xmlTextWriter.WriteEndElement()
    xmlTextWriter.Close()
End Sub

Private Sub SaveToXML(ByVal nodes As TreeNodeAdvCollection)
    For Each node As TreeNodeAdv In nodes
        If node.Nodes.Count > 0 Then
            xmlTextWriter.WriteStartElement(node.Text)
            SaveToXML(node.Nodes)
            xmlTextWriter.WriteEndElement()
        Else
            xmlTextWriter.WriteString(node.Text)
        End If
    Next
End Sub
```

## Example XML Structure

The tree structure is saved as nested XML elements:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Company>
  <Departments>
    <Engineering>
      <Backend>John Doe</Backend>
      <Frontend>Jane Smith</Frontend>
    </Engineering>
    <Sales>
      <North>Bob Johnson</North>
      <South>Alice Williams</South>
    </Sales>
  </Departments>
</Company>
```

## Advanced XML Binding with Attributes

To include additional data (like subitems), extend the XML saving/loading:

```csharp
private void SaveToXMLWithAttributes(TreeNodeAdvCollection nodes)
{
    foreach (TreeNodeAdv node in nodes)
    {
        xmlTextWriter.WriteStartElement("Node");
        xmlTextWriter.WriteAttributeString("Text", node.Text);
        
        // Save subitems as attributes or child elements
        for (int i = 0; i < node.SubItems.Count; i++)
        {
            xmlTextWriter.WriteAttributeString($"SubItem{i}", node.SubItems[i].Text);
        }
        
        // Save node properties
        xmlTextWriter.WriteAttributeString("Checked", node.Checked.ToString());
        xmlTextWriter.WriteAttributeString("Expanded", node.Expanded.ToString());
        
        if (node.Nodes.Count > 0)
        {
            SaveToXMLWithAttributes(node.Nodes);
        }
        
        xmlTextWriter.WriteEndElement();
    }
}

private void LoadFromXMLWithAttributes(XmlNode xmlNode, TreeNodeAdv parentNode)
{
    foreach (XmlNode xNode in xmlNode.ChildNodes)
    {
        if (xNode.Name == "Node")
        {
            TreeNodeAdv treeNode = new TreeNodeAdv();
            
            // Load text
            if (xNode.Attributes["Text"] != null)
                treeNode.Text = xNode.Attributes["Text"].Value;
            
            // Load subitems
            int subItemIndex = 0;
            while (xNode.Attributes[$"SubItem{subItemIndex}"] != null)
            {
                TreeNodeAdvSubItem subItem = new TreeNodeAdvSubItem();
                subItem.Text = xNode.Attributes[$"SubItem{subItemIndex}"].Value;
                treeNode.SubItems.Add(subItem);
                subItemIndex++;
            }
            
            // Load properties
            if (xNode.Attributes["Checked"] != null)
                treeNode.Checked = bool.Parse(xNode.Attributes["Checked"].Value);
                
            if (xNode.Attributes["Expanded"] != null)
                treeNode.Expanded = bool.Parse(xNode.Attributes["Expanded"].Value);
            
            // Add to parent
            if (parentNode == null)
                multiColumnTreeView1.Nodes.Add(treeNode);
            else
                parentNode.Nodes.Add(treeNode);
            
            // Recursively load children
            LoadFromXMLWithAttributes(xNode, treeNode);
        }
    }
}
```

## Practical Examples

### Example 1: Complete Import/Export

```csharp
public class TreeDataManager
{
    private MultiColumnTreeView treeView;
    
    public TreeDataManager(MultiColumnTreeView tree)
    {
        this.treeView = tree;
    }
    
    public void Import(string filename)
    {
        try
        {
            XmlDocument doc = new XmlDocument();
            doc.Load(filename);
            
            treeView.BeginUpdate();
            try
            {
                treeView.Nodes.Clear();
                LoadTreeStructure(doc.DocumentElement, null);
            }
            finally
            {
                treeView.EndUpdate();
            }
            
            MessageBox.Show("Data imported successfully");
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Import failed: {ex.Message}");
        }
    }
    
    public void Export(string filename)
    {
        try
        {
            using (XmlTextWriter writer = new XmlTextWriter(filename, Encoding.UTF8))
            {
                writer.Formatting = Formatting.Indented;
                writer.WriteStartDocument();
                writer.WriteStartElement("TreeData");
                
                SaveTreeStructure(treeView.Nodes, writer);
                
                writer.WriteEndElement();
                writer.WriteEndDocument();
            }
            
            MessageBox.Show("Data exported successfully");
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Export failed: {ex.Message}");
        }
    }
    
    private void LoadTreeStructure(XmlNode xmlNode, TreeNodeAdv parentNode)
    {
        foreach (XmlNode child in xmlNode.ChildNodes)
        {
            if (child.NodeType == XmlNodeType.Element)
            {
                TreeNodeAdv node = new TreeNodeAdv();
                node.Text = child.Attributes["text"]?.Value ?? child.Name;
                
                if (parentNode == null)
                    treeView.Nodes.Add(node);
                else
                    parentNode.Nodes.Add(node);
                
                LoadTreeStructure(child, node);
            }
        }
    }
    
    private void SaveTreeStructure(TreeNodeAdvCollection nodes, XmlTextWriter writer)
    {
        foreach (TreeNodeAdv node in nodes)
        {
            writer.WriteStartElement("Node");
            writer.WriteAttributeString("text", node.Text);
            
            if (node.Nodes.Count > 0)
            {
                SaveTreeStructure(node.Nodes, writer);
            }
            
            writer.WriteEndElement();
        }
    }
}

// Usage
TreeDataManager manager = new TreeDataManager(multiColumnTreeView1);
manager.Export("tree_data.xml");
manager.Import("tree_data.xml");
```

### Example 2: JSON Alternative

For modern applications, you might prefer JSON over XML:

```csharp
using System.Text.Json;

public class TreeNodeData
{
    public string Text { get; set; }
    public List<string> SubItems { get; set; } = new List<string>();
    public bool Checked { get; set; }
    public List<TreeNodeData> Children { get; set; } = new List<TreeNodeData>();
}

public void ExportToJSON(string filename)
{
    List<TreeNodeData> data = new List<TreeNodeData>();
    
    foreach (TreeNodeAdv node in multiColumnTreeView1.Nodes)
    {
        data.Add(ConvertNodeToData(node));
    }
    
    string json = JsonSerializer.Serialize(data, new JsonSerializerOptions 
    { 
        WriteIndented = true 
    });
    
    File.WriteAllText(filename, json);
}

public void ImportFromJSON(string filename)
{
    string json = File.ReadAllText(filename);
    List<TreeNodeData> data = JsonSerializer.Deserialize<List<TreeNodeData>>(json);
    
    multiColumnTreeView1.BeginUpdate();
    try
    {
        multiColumnTreeView1.Nodes.Clear();
        
        foreach (TreeNodeData nodeData in data)
        {
            multiColumnTreeView1.Nodes.Add(ConvertDataToNode(nodeData));
        }
    }
    finally
    {
        multiColumnTreeView1.EndUpdate();
    }
}

private TreeNodeData ConvertNodeToData(TreeNodeAdv node)
{
    TreeNodeData data = new TreeNodeData
    {
        Text = node.Text,
        Checked = node.Checked
    };
    
    foreach (TreeNodeAdvSubItem subItem in node.SubItems)
    {
        data.SubItems.Add(subItem.Text);
    }
    
    foreach (TreeNodeAdv child in node.Nodes)
    {
        data.Children.Add(ConvertNodeToData(child));
    }
    
    return data;
}

private TreeNodeAdv ConvertDataToNode(TreeNodeData data)
{
    TreeNodeAdv node = new TreeNodeAdv { Text = data.Text, Checked = data.Checked };
    
    foreach (string subItemText in data.SubItems)
    {
        node.SubItems.Add(new TreeNodeAdvSubItem { Text = subItemText });
    }
    
    foreach (TreeNodeData childData in data.Children)
    {
        node.Nodes.Add(ConvertDataToNode(childData));
    }
    
    return node;
}
```

## Best Practices

1. **Use BeginUpdate/EndUpdate** when loading many nodes for better performance
2. **Handle exceptions** during file I/O operations
3. **Validate XML** structure before loading to prevent errors
4. **Store metadata** (checked state, expanded state) as attributes for complete state persistence
5. **Use formatted/indented** XML for human readability
6. **Consider JSON** for modern applications (easier parsing, better tooling support)
7. **Back up data** before overwriting export files
8. **Test with large datasets** to ensure performance is acceptable

## Common Issues

**XML parsing errors:**
- Ensure XML is well-formed
- Check for special characters that need escaping in node text

**Missing nodes after import:**
- Verify recursive loading covers all child levels
- Check if root node handling is correct

**Subitems not loading:**
- Confirm column count matches before loading
- Ensure subitems are being read from XML

**Performance issues with large XML files:**
- Use BeginUpdate/EndUpdate
- Consider lazy loading for very large trees
- Show progress indicator for long operations
