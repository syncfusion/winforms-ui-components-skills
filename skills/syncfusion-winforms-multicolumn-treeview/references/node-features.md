# Node Features

Interactive node features including checkboxes, option buttons, tooltips, plus/minus symbols, and custom controls.

## CheckBoxes

```csharp
//Check box for all nodes in MultiColumnTreeView
this.multiColumnTreeView1.ShowCheckBoxes = true;

//Check box for particular nodes
this.multiColumnTreeView1.Nodes[0].ShowCheckBox = true;

this.multiColumnTreeView1.Nodes[0].Checked = true;
```

## Option Buttons (Radio Buttons)

```csharp
// Enable for all nodes
multiColumnTreeView1.ShowOptionButtons = true;

//Option button for all nodes in MultiColumnTreeView
this.multiColumnTreeView1.ShowOptionButtons = true;

//Option button for particular nodes
this.multiColumnTreeView1.Nodes[0].ShowOptionButton = true;

this.multiColumnTreeView1.Nodes[0].Optioned = true;
```

## ToolTips and HelpText

```csharp
// Node help text
TreeNodeAdv node = new TreeNodeAdv { Text = "India" };
node.HelpText = "Country in South Asia with capital New Delhi";

// SubItem help text
TreeNodeAdvSubItem subItem = new TreeNodeAdvSubItem { Text = "New Delhi" };
subItem.HelpText = "Capital city of India with population of 16.8 million";
node.SubItems.Add(subItem);
```

## Plus/Minus Symbols

```csharp
// Enable for all nodes
multiColumnTreeView1.ShowPlusMinus = true;

// Enable for specific node
node.ShowPlusMinus = true;

// Hide for leaf nodes
node.ShowPlusMinus = (node.Nodes.Count > 0);
```

## Root Lines and Connecting Lines

```csharp
// Show lines between root nodes
multiColumnTreeView1.ShowRootLines = true;

// Show connecting lines
multiColumnTreeView1.ShowLines = true;
multiColumnTreeView1.LineColor = Color.Gray;
```

## Multiline Text

```csharp
TreeNodeAdv node = new TreeNodeAdv
{
    Text = "This is a very long node text that spans\nmultiple lines when multiline is enabled",
    Multiline = true
};
```

## Task Selection Example

```csharp
void SetupTaskList()
{
    multiColumnTreeView1.ShowCheckBoxes = true;
    multiColumnTreeView1.InteractiveCheckBoxes = true;
    
    TreeNodeAdv projectNode = new TreeNodeAdv { Text = "Project Alpha" };
    TreeNodeAdv phase1 = new TreeNodeAdv { Text = "Phase 1" };
    phase1.Nodes.Add(new TreeNodeAdv { Text = "Task 1.1", Checked = true });
    phase1.Nodes.Add(new TreeNodeAdv { Text = "Task 1.2", Checked = false });
    projectNode.Nodes.Add(phase1);
    multiColumnTreeView1.Nodes.Add(projectNode);
}
```

## Settings with Option Buttons

```csharp
void CreateSettings()
{
    multiColumnTreeView1.ShowOptionButtons = true;
    
    TreeNodeAdv themeNode = new TreeNodeAdv { Text = "Theme" };
    themeNode.EnsureDefaultOptionedChild = true;
    themeNode.Nodes.Add(new TreeNodeAdv { Text = "Light", Optioned = true });
    themeNode.Nodes.Add(new TreeNodeAdv { Text = "Dark" });
    themeNode.Nodes.Add(new TreeNodeAdv { Text = "Auto" });
    
    multiColumnTreeView1.Nodes.Add(themeNode);
}

string GetSelectedOption(TreeNodeAdv parentNode)
{
    foreach (TreeNodeAdv child in parentNode.Nodes)
        if (child.Optioned) return child.Text;
    return null;
}
```

## Best Practices

- Use interactive checkboxes for hierarchical selection
- Use option buttons when only one selection needed
- Provide helpful HelpText for complex data
- Hide plus/minus on leaf nodes
- Use `EnsureDefaultOptionedChild` for option buttons
