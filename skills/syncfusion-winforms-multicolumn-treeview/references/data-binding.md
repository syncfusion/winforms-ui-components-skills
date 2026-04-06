# Data Binding

XML-based data persistence for MultiColumnTreeView. Load and save tree structures from/to XML files.

## Basic XML Loading

```csharp
private void LoadTreeViewFromXML()
{
    XmlDocument xDoc = new XmlDocument();
    xDoc.Load("TreeView.xml");
    multiColumnTreeView1.Nodes.Clear();
    multiColumnTreeView1.Nodes.Add(new TreeNodeAdv(xDoc.DocumentElement.Name));
    LoadFromXML(xDoc.DocumentElement, (TreeNodeAdv)multiColumnTreeView1.Nodes[0]);
    multiColumnTreeView1.ExpandAll();
}

private void LoadFromXML(XmlNode xmlNode, TreeNodeAdv treeNode)
{
    if (xmlNode.HasChildNodes)
    {
        for (int i = 0; i < xmlNode.ChildNodes.Count; i++)
        {
            XmlNode xNode = xmlNode.ChildNodes[i];
            treeNode.Nodes.Add(new TreeNodeAdv(xNode.Name));
            LoadFromXML(xNode, treeNode.Nodes[i]);
        }
    }
    else
    {
        treeNode.Text = xmlNode.OuterXml.Trim();
    }
}
```

## Basic XML Saving

```csharp
private XmlTextWriter xmlTextWriter;

public void ExportToXML(MultiColumnTreeView tree, string filename)
{
    xmlTextWriter = new XmlTextWriter(filename, System.Text.Encoding.UTF8);
    xmlTextWriter.WriteStartDocument();
    xmlTextWriter.WriteStartElement(multiColumnTreeView1.Nodes[0].Text);
    foreach (TreeNodeAdv node in tree.Nodes)
        SaveToXML(node.Nodes);
    xmlTextWriter.WriteEndElement();
    xmlTextWriter.Close();
}

private void SaveToXML(TreeNodeAdvCollection nodes)
{
    foreach (TreeNodeAdv node in nodes)
    {
        if (node.Nodes.Count > 0)
        {
            xmlTextWriter.WriteStartElement(node.Text);
            SaveToXML(node.Nodes);
            xmlTextWriter.WriteEndElement();
        }
        else
        {
            xmlTextWriter.WriteString(node.Text);
        }
    }
}
```

## XML with Attributes (SubItems and Properties)

```csharp
private void SaveToXMLWithAttributes(TreeNodeAdvCollection nodes)
{
    foreach (TreeNodeAdv node in nodes)
    {
        xmlTextWriter.WriteStartElement("Node");
        xmlTextWriter.WriteAttributeString("Text", node.Text);
        for (int i = 0; i < node.SubItems.Count; i++)
            xmlTextWriter.WriteAttributeString($"SubItem{i}", node.SubItems[i].Text);
        xmlTextWriter.WriteAttributeString("Checked", node.Checked.ToString());
        if (node.Nodes.Count > 0)
            SaveToXMLWithAttributes(node.Nodes);
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
            if (xNode.Attributes["Text"] != null)
                treeNode.Text = xNode.Attributes["Text"].Value;
            
            int subItemIndex = 0;
            while (xNode.Attributes[$"SubItem{subItemIndex}"] != null)
            {
                treeNode.SubItems.Add(new TreeNodeAdvSubItem 
                { 
                    Text = xNode.Attributes[$"SubItem{subItemIndex}"].Value 
                });
                subItemIndex++;
            }
            
            if (xNode.Attributes["Checked"] != null)
                treeNode.Checked = bool.Parse(xNode.Attributes["Checked"].Value);
            
            (parentNode == null ? multiColumnTreeView1.Nodes : parentNode.Nodes).Add(treeNode);
            LoadFromXMLWithAttributes(xNode, treeNode);
        }
    }
}
```

## Complete Import/Export Manager

```csharp
public class TreeDataManager
{
    private MultiColumnTreeView treeView;
    
    public TreeDataManager(MultiColumnTreeView tree) => this.treeView = tree;
    
    public void Import(string filename)
    {
        XmlDocument doc = new XmlDocument();
        doc.Load(filename);
        treeView.BeginUpdate();
        try
        {
            treeView.Nodes.Clear();
            LoadTreeStructure(doc.DocumentElement, null);
        }
        finally { treeView.EndUpdate(); }
    }
    
    public void Export(string filename)
    {
        using (XmlTextWriter writer = new XmlTextWriter(filename, Encoding.UTF8))
        {
            writer.Formatting = Formatting.Indented;
            writer.WriteStartDocument();
            writer.WriteStartElement("TreeData");
            SaveTreeStructure(treeView.Nodes, writer);
            writer.WriteEndElement();
        }
    }
    
    private void LoadTreeStructure(XmlNode xmlNode, TreeNodeAdv parentNode)
    {
        foreach (XmlNode child in xmlNode.ChildNodes)
        {
            if (child.NodeType == XmlNodeType.Element)
            {
                TreeNodeAdv node = new TreeNodeAdv { Text = child.Attributes["text"]?.Value ?? child.Name };
                (parentNode?.Nodes ?? treeView.Nodes).Add(node);
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
                SaveTreeStructure(node.Nodes, writer);
            writer.WriteEndElement();
        }
    }
}
```

## JSON Alternative

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
    var data = multiColumnTreeView1.Nodes.Cast<TreeNodeAdv>().Select(n => ConvertNodeToData(n)).ToList();
    File.WriteAllText(filename, JsonSerializer.Serialize(data, new JsonSerializerOptions { WriteIndented = true }));
}

public void ImportFromJSON(string filename)
{
    var data = JsonSerializer.Deserialize<List<TreeNodeData>>(File.ReadAllText(filename));
    multiColumnTreeView1.BeginUpdate();
    try
    {
        multiColumnTreeView1.Nodes.Clear();
        foreach (var nodeData in data)
            multiColumnTreeView1.Nodes.Add(ConvertDataToNode(nodeData));
    }
    finally { multiColumnTreeView1.EndUpdate(); }
}

private TreeNodeData ConvertNodeToData(TreeNodeAdv node)
{
    return new TreeNodeData
    {
        Text = node.Text,
        Checked = node.Checked,
        SubItems = node.SubItems.Cast<TreeNodeAdvSubItem>().Select(s => s.Text).ToList(),
        Children = node.Nodes.Cast<TreeNodeAdv>().Select(n => ConvertNodeToData(n)).ToList()
    };
}

private TreeNodeAdv ConvertDataToNode(TreeNodeData data)
{
    TreeNodeAdv node = new TreeNodeAdv { Text = data.Text, Checked = data.Checked };
    data.SubItems.ForEach(s => node.SubItems.Add(new TreeNodeAdvSubItem { Text = s }));
    data.Children.ForEach(c => node.Nodes.Add(ConvertDataToNode(c)));
    return node;
}
```

## Best Practices

- Use `BeginUpdate/EndUpdate` when loading many nodes
- Handle exceptions during file I/O operations
- Store metadata (checked state, expanded state) as attributes
- Consider JSON for modern applications (easier parsing)
- Test with large datasets to ensure performance
