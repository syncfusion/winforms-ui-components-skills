# Node Features

This guide covers interactive node features including checkboxes, option buttons, tooltips, plus/minus symbols, custom controls, and primitives.

## CheckBoxes

CheckBoxes allow users to select multiple nodes independently. The control supports both simple and interactive checkboxes.

### Enabling CheckBoxes for All Nodes

```csharp
// Enable checkboxes for all nodes
multiColumnTreeView1.ShowCheckBoxes = true;
```

### Enabling CheckBoxes for Individual Nodes

```csharp
// Enable checkbox for specific node
TreeNodeAdv node = multiColumnTreeView1.Nodes[0];
node.ShowCheckBox = true;
node.Checked = true; // Set checked state
```

### Interactive CheckBoxes

Interactive checkboxes automatically update parent checkbox states based on child checkbox states:

```csharp
// Enable interactive checkboxes for all nodes
multiColumnTreeView1.InteractiveCheckBoxes = true;

// Or for individual nodes
TreeNodeAdv parentNode = multiColumnTreeView1.Nodes[0];
parentNode.InteractiveCheckBox = true;
```

**Interactive Behavior:**
- If all children are checked → Parent is checked
- If all children are unchecked → Parent is unchecked
- If some children are checked → Parent is in intermediate state

### Tristate CheckBox Settings

CheckBoxes support three states: Checked, Unchecked, and Indeterminate (intermediate).

**Properties:**
- `CheckState` - Gets or sets check state (Checked, Unchecked, Indeterminate)
- `CheckColor` - Color of the check mark
- `IntermediateCheckColor` - Color when in intermediate state
- `Checked` - Simple boolean checked property

```csharp
TreeNodeAdv node = new TreeNodeAdv { Text = "Parent Node" };
node.CheckState = CheckState.Indeterminate;
node.CheckColor = Color.Green;
node.IntermediateCheckColor = Color.Orange;
```

### Getting Checked Nodes

```csharp
// Get all checked nodes
TreeNodeAdvCollection checkedNodes = multiColumnTreeView1.CheckedNodes;

foreach (TreeNodeAdv node in checkedNodes)
{
    Console.WriteLine($"Checked: {node.Text}");
}
```

### Example: Task Selection with CheckBoxes

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

## Option Buttons (Radio Buttons)

Option buttons allow users to select only one node from a group, similar to radio buttons.

### Enabling Option Buttons

```csharp
// Enable option buttons for all nodes
multiColumnTreeView1.ShowOptionButtons = true;

// Or for individual nodes
TreeNodeAdv node = multiColumnTreeView1.Nodes[0];
node.ShowOptionButton = true;
node.Optioned = true; // Set as selected
```

### Option Button Properties

**TreeNodeAdv Properties:**
- `ShowOptionButton` - Show/hide option button for node
- `Optioned` - Gets or sets option button state
- `OptionButtonColor` - Background color of selected option button

```csharp
TreeNodeAdv node = new TreeNodeAdv { Text = "Option 1" };
node.ShowOptionButton = true;
node.Optioned = true;
node.OptionButtonColor = Color.LightBlue;
```

### Ensuring Default Selection

```csharp
// Ensure at least one child is always selected
TreeNodeAdv parentNode = new TreeNodeAdv { Text = "Payment Method" };
parentNode.EnsureDefaultOptionedChild = true;

parentNode.Nodes.Add(new TreeNodeAdv { Text = "Credit Card", Optioned = true });
parentNode.Nodes.Add(new TreeNodeAdv { Text = "PayPal" });
parentNode.Nodes.Add(new TreeNodeAdv { Text = "Bank Transfer" });
```

## ToolTips and HelpText

### ToolTips

ToolTips automatically appear when a node's text is partially visible:

```csharp
// ToolTips are enabled by default
// The full text appears when hovering over truncated text
TreeNodeAdv node = new TreeNodeAdv { Text = "Very Long Node Text That Gets Truncated" };
```

### HelpText

HelpText displays custom information when hovering over a node or subitem:

```csharp
// Add help text to node
TreeNodeAdv node = new TreeNodeAdv { Text = "India" };
node.HelpText = "Country in South Asia with capital New Delhi";

// Add help text to subitem
TreeNodeAdvSubItem subItem = new TreeNodeAdvSubItem { Text = "New Delhi" };
subItem.HelpText = "Capital city of India with population of 16.8 million";
node.SubItems.Add(subItem);
```

## Plus/Minus Symbols

Plus/minus symbols indicate expand/collapse state of nodes with children.

### Control-Level Configuration

```csharp
// Enable plus/minus for all nodes
multiColumnTreeView1.ShowPlusMinus = true;
```

### Node-Level Configuration

```csharp
// Enable for specific node
TreeNodeAdv node = multiColumnTreeView1.Nodes[0];
node.ShowPlusMinus = true;
```

### Hiding Plus/Minus

```csharp
// Hide plus/minus symbols
multiColumnTreeView1.ShowPlusMinus = false;

// Or hide for nodes without children
node.ShowPlusMinus = (node.Nodes.Count > 0);
```

## Root Lines and Connecting Lines

### Root Lines

Lines between root-level nodes:

```csharp
// Show lines between root nodes
multiColumnTreeView1.ShowRootLines = true;
```

### Connecting Lines

Lines connecting all nodes:

```csharp
// Show connecting lines
multiColumnTreeView1.ShowLines = true;

// Customize line color
multiColumnTreeView1.LineColor = Color.Gray;
```

**Note:** Setting `ShowLines = false` hides all connecting lines, including root lines.

## Multiline Text Support

Enable multiline text for nodes:

```csharp
TreeNodeAdv node = new TreeNodeAdv();
node.Text = "This is a very long node text that spans multiple lines\nwhen multiline is enabled";
node.Multiline = true;
```

## Custom Controls in Nodes

Nodes can host custom controls like combo boxes, date pickers, or buttons:

```csharp
TreeNodeAdv node = new TreeNodeAdv { Text = "Select Date" };

// Create custom control
DateTimePicker datePicker = new DateTimePicker();
datePicker.Width = 150;

// Add custom control to node (via designer or code)
// Note: Custom controls are typically added through the designer
// using the CustomControl property of TreeNodeAdv
```

## Primitives

Primitives define the visual elements and their order within a node. Available primitive types:

### Primitive Types

1. **LabelPrimitive** - Displays node text
2. **LeftImagePrimitive** - Shows left images
3. **RightImagePrimitive** - Shows right images  
4. **StateImagePrimitive** - Shows expand/collapse state images
5. **CheckBoxPrimitive** - Displays checkbox
6. **OptionButtonPrimitive** - Displays option button
7. **CustomControlPrimitive** - Displays custom control

### Configuring Primitives

Primitives are typically configured through the **Primitives Collection Editor** in the designer:

1. Select a node in the designer
2. Find the **Primitives** property
3. Open the Primitives Collection Editor
4. Add primitives and set their **Index** property to control order

### Primitive Order Example

By default, primitives appear in this order:
1. StateImagePrimitive (expand/collapse icon)
2. CheckBoxPrimitive / OptionButtonPrimitive
3. LeftImagePrimitive
4. LabelPrimitive
5. RightImagePrimitive

You can reorder by changing the Index property of each primitive.

### Code Example

```csharp
// Primitives are typically managed through designer
// But you can access them in code:
TreeNodeAdv node = multiColumnTreeView1.Nodes[0];

// Access primitives collection (read-only in most scenarios)
// Configuration typically done via designer
```

## Practical Examples

### Example 1: File Selection with CheckBoxes

```csharp
void CreateFileSelector()
{
    multiColumnTreeView1.ShowCheckBoxes = true;
    multiColumnTreeView1.InteractiveCheckBoxes = true;
    
    TreeNodeAdv documentsFolder = new TreeNodeAdv { Text = "Documents" };
    documentsFolder.Nodes.Add(new TreeNodeAdv { Text = "Report.docx" });
    documentsFolder.Nodes.Add(new TreeNodeAdv { Text = "Presentation.pptx" });
    
    TreeNodeAdv imagesFolder = new TreeNodeAdv { Text = "Images" };
    imagesFolder.Nodes.Add(new TreeNodeAdv { Text = "Photo1.jpg" });
    imagesFolder.Nodes.Add(new TreeNodeAdv { Text = "Photo2.jpg" });
    
    multiColumnTreeView1.Nodes.AddRange(new TreeNodeAdv[] 
    {
        documentsFolder,
        imagesFolder
    });
}

void GetSelectedFiles()
{
    List<string> selectedFiles = new List<string>();
    
    foreach (TreeNodeAdv node in multiColumnTreeView1.CheckedNodes)
    {
        // Only include leaf nodes (files, not folders)
        if (node.Nodes.Count == 0)
        {
            selectedFiles.Add(node.Text);
        }
    }
    
    MessageBox.Show($"Selected {selectedFiles.Count} files");
}
```

### Example 2: Settings with Option Buttons

```csharp
void CreateSettings()
{
    multiColumnTreeView1.ShowOptionButtons = true;
    
    // Theme selection
    TreeNodeAdv themeNode = new TreeNodeAdv { Text = "Theme" };
    themeNode.EnsureDefaultOptionedChild = true;
    themeNode.Nodes.Add(new TreeNodeAdv { Text = "Light", Optioned = true });
    themeNode.Nodes.Add(new TreeNodeAdv { Text = "Dark" });
    themeNode.Nodes.Add(new TreeNodeAdv { Text = "Auto" });
    
    // Language selection
    TreeNodeAdv languageNode = new TreeNodeAdv { Text = "Language" };
    languageNode.EnsureDefaultOptionedChild = true;
    languageNode.Nodes.Add(new TreeNodeAdv { Text = "English", Optioned = true });
    languageNode.Nodes.Add(new TreeNodeAdv { Text = "Spanish" });
    languageNode.Nodes.Add(new TreeNodeAdv { Text = "French" });
    
    multiColumnTreeView1.Nodes.AddRange(new TreeNodeAdv[]
    {
        themeNode,
        languageNode
    });
}

string GetSelectedOption(TreeNodeAdv parentNode)
{
    foreach (TreeNodeAdv child in parentNode.Nodes)
    {
        if (child.Optioned)
            return child.Text;
    }
    return null;
}
```

### Example 3: Detailed HelpText

```csharp
void CreateProductCatalog()
{
    TreeNodeAdv electronicsNode = new TreeNodeAdv { Text = "Electronics" };
    electronicsNode.HelpText = "Electronic devices and accessories";
    
    TreeNodeAdv laptopNode = new TreeNodeAdv { Text = "Laptop Pro 15" };
    laptopNode.HelpText = "15-inch professional laptop\nProcessor: Intel Core i7\nRAM: 16GB\nStorage: 512GB SSD";
    
    TreeNodeAdvSubItem priceSubItem = new TreeNodeAdvSubItem { Text = "$1,299" };
    priceSubItem.HelpText = "Base price. Discounts may apply for bulk orders.";
    laptopNode.SubItems.Add(priceSubItem);
    
    electronicsNode.Nodes.Add(laptopNode);
    multiColumnTreeView1.Nodes.Add(electronicsNode);
}
```

## Best Practices

1. **Use interactive checkboxes** for hierarchical selection to auto-update parent states
2. **Use option buttons** when only one selection is needed from a group
3. **Provide helpful HelpText** for complex data to aid user understanding
4. **Hide plus/minus** on leaf nodes to indicate no children
5. **Use consistent checkbox/option patterns** across similar node groups
6. **Test checkbox states** after programmatic changes to ensure correct display
7. **Handle checkbox events** to respond to user selection changes
8. **Use EnsureDefaultOptionedChild** for option buttons to ensure a selection

## Common Issues

**CheckBoxes not updating parent:**
- Ensure `InteractiveCheckBoxes` or node's `InteractiveCheckBox` is true

**Option buttons allow multiple selections:**
- Option buttons work within same parent - children of different parents can be selected independently

**HelpText not showing:**
- Verify HelpText property is set on node or subitem
- Check if text is not empty

**Plus/minus not showing:**
- Ensure node has children
- Verify `ShowPlusMinus` is true at control or node level
