# Node Customization

## Table of Contents
- [Overview](#overview)
- [ShowLines and ShowRootLines](#showlines-and-showrootlines)
- [Plus/Minus Signs](#plusminus-signs)
- [CheckBoxes](#checkboxes)
- [Option Buttons](#option-buttons)
- [Node Images](#node-images)
- [Control-Level vs Node-Level Properties](#control-level-vs-node-level-properties)

## Overview

TreeViewAdv provides extensive node customization capabilities at both control and individual node levels. This allows fine-grained control over visual appearance and behavior of tree nodes including connecting lines, expand/collapse indicators, checkboxes, option buttons, and images.

## ShowLines and ShowRootLines

### ShowLines Property

Controls visibility of connecting lines between child nodes within the tree hierarchy.

```csharp
// Show connecting lines for all nodes (default: true)
treeViewAdv1.ShowLines = true;

// Hide all connecting lines
treeViewAdv1.ShowLines = false;
```

**Effect:** When set to `false`, ALL connecting lines disappear from the entire control, including root lines.

### ShowRootLines Property

Controls visibility of connecting lines specifically between root-level nodes.

```csharp
// Show lines between root nodes (default: true)
treeViewAdv1.ShowRootLines = true;

// Hide lines between root nodes only
treeViewAdv1.ShowRootLines = false;
```

**Important:** `ShowRootLines` only affects root level connections. If `ShowLines` is `false`, `ShowRootLines` has no effect.

### Combination Examples

```csharp
// Scenario 1: Show all lines including root
treeViewAdv1.ShowLines = true;
treeViewAdv1.ShowRootLines = true;

// Scenario 2: Show child lines only, hide root lines
treeViewAdv1.ShowLines = true;
treeViewAdv1.ShowRootLines = false;

// Scenario 3: Hide all lines
treeViewAdv1.ShowLines = false;
// ShowRootLines doesn't matter when ShowLines is false
```

### VB.NET Example

```vb
' Configure line visibility
Me.treeViewAdv1.ShowLines = True
Me.treeViewAdv1.ShowRootLines = False
```

## Plus/Minus Signs

### Control-Level Configuration

The `ShowPlusMinus` property controls expand/collapse buttons for all parent nodes.

```csharp
// Show plus/minus for all parent nodes (default: true)
treeViewAdv1.ShowPlusMinus = true;

// Hide plus/minus for all nodes
treeViewAdv1.ShowPlusMinus = false;
```

### Node-Level Configuration

Override control-level setting for specific nodes using `TreeNodeAdv.ShowPlusMinus`.

```csharp
TreeNodeAdv specificNode = new TreeNodeAdv("Special Node");

// Show plus/minus even if control setting is false
specificNode.ShowPlusMinus = true;

// Hide plus/minus even if control setting is true
specificNode.ShowPlusMinus = false;
```

### ShowPlusOnExpand Property

Keep plus sign visible even when node is expanded. Useful for load-on-demand scenarios where users might want to reload children.

```csharp
// Enable load on demand (required for ShowPlusOnExpand)
treeViewAdv1.LoadOnDemand = true;

// Configure node to keep plus sign when expanded
TreeNodeAdv node = new TreeNodeAdv("Dynamic Node");
node.ShowPlusOnExpand = true;
treeViewAdv1.Nodes.Add(node);
```

**Use Case:** Indicating that more data can be loaded even after initial expansion.

### Complete Example

```csharp
private void ConfigurePlusMinusSigns()
{
    // Global setting
    treeViewAdv1.ShowPlusMinus = true;
    
    // Create nodes with mixed configurations
    TreeNodeAdv parent1 = new TreeNodeAdv("Always Show Plus/Minus");
    parent1.ShowPlusMinus = true;
    parent1.Nodes.Add(new TreeNodeAdv("Child 1"));
    
    TreeNodeAdv parent2 = new TreeNodeAdv("Hide Plus/Minus");
    parent2.ShowPlusMinus = false;
    parent2.Nodes.Add(new TreeNodeAdv("Child 2"));
    
    TreeNodeAdv parent3 = new TreeNodeAdv("Keep Plus When Expanded");
    parent3.ShowPlusOnExpand = true;
    parent3.Nodes.Add(new TreeNodeAdv("Child 3"));
    
    treeViewAdv1.LoadOnDemand = true; // Required for ShowPlusOnExpand
    treeViewAdv1.Nodes.AddRange(new[] { parent1, parent2, parent3 });
}
```

## CheckBoxes

### Control-Level Configuration

Display checkboxes for all nodes in the tree.

```csharp
// Show checkboxes for all nodes
treeViewAdv1.ShowCheckBoxes = true;

// Hide checkboxes for all nodes (default)
treeViewAdv1.ShowCheckBoxes = false;
```

### Node-Level Configuration

Control checkbox visibility for individual nodes.

```csharp
// Show checkbox for specific node
TreeNodeAdv checkableNode = new TreeNodeAdv("Checkable Item");
checkableNode.ShowCheckBox = true;

// Hide checkbox for specific node
TreeNodeAdv nonCheckableNode = new TreeNodeAdv("Non-Checkable Item");
nonCheckableNode.ShowCheckBox = false;
```

### Getting/Setting Checked State

```csharp
// Set node as checked
node.Checked = true;

// Check if node is checked
if (node.Checked)
{
    MessageBox.Show($"{node.Text} is checked");
}

// Get all checked nodes
foreach (TreeNodeAdv node in treeViewAdv1.Nodes)
{
    if (node.Checked)
    {
        // Process checked node
    }
}
```

### CheckedMember for Data Binding

Bind checkbox state to data source field.

```csharp
DataTable data = GetData();

treeViewAdv1.DataSource = data;
treeViewAdv1.DisplayMember = "Name";
treeViewAdv1.CheckedMember = "IsActive";  // Bind to boolean field
treeViewAdv1.ShowCheckBoxes = true;
```

### Mixed Configuration Example

```csharp
// Global setting: hide checkboxes
treeViewAdv1.ShowCheckBoxes = false;

// Show checkboxes only for specific nodes
TreeNodeAdv selectableNode = new TreeNodeAdv("Select This");
selectableNode.ShowCheckBox = true;
selectableNode.Checked = true;

TreeNodeAdv anotherSelectableNode = new TreeNodeAdv("Or This");
anotherSelectableNode.ShowCheckBox = true;

// This node won't have checkbox (inherits global setting)
TreeNodeAdv normalNode = new TreeNodeAdv("Normal Node");

treeViewAdv1.Nodes.AddRange(new[] { selectableNode, anotherSelectableNode, normalNode });
```

### Handling CheckStateChanged Event

```csharp
treeViewAdv1.NodeCheckStateChanged += (sender, e) =>
{
    TreeNodeAdv node = e.Node;
    MessageBox.Show($"{node.Text} is now {(node.Checked ? "checked" : "unchecked")}");
};
```

## Option Buttons

Option buttons (radio buttons) provide single-selection behavior within a group of sibling nodes.

### Control-Level Configuration

```csharp
// Show option buttons for all nodes
treeViewAdv1.ShowOptionButtons = true;

// Hide option buttons for all nodes (default)
treeViewAdv1.ShowOptionButtons = false;
```

### Node-Level Configuration

```csharp
TreeNodeAdv optionNode = new TreeNodeAdv("Option 1");
optionNode.ShowOptionButton = true;

// Set as selected
optionNode.Optioned = true;

// Check if optioned
if (optionNode.Optioned)
{
    MessageBox.Show("This option is selected");
}
```

### Customizing Option Button Colors

```csharp
TreeNodeAdv node = new TreeNodeAdv("Custom Option");
node.ShowOptionButton = true;
node.OptionButtonColor = Color.AliceBlue;           // Normal state color
node.SelectedOptionButtonColor = Color.Red;         // Selected state color
node.Optioned = true;
```

### Use Case: Single Selection Group

```csharp
private void CreateOptionButtonGroup()
{
    treeViewAdv1.ShowOptionButtons = false; // Global off
    
    TreeNodeAdv categoryNode = new TreeNodeAdv("Select Priority");
    
    // Create mutually exclusive options
    TreeNodeAdv highPriority = new TreeNodeAdv("High");
    highPriority.ShowOptionButton = true;
    highPriority.Optioned = true; // Default selection
    
    TreeNodeAdv mediumPriority = new TreeNodeAdv("Medium");
    mediumPriority.ShowOptionButton = true;
    
    TreeNodeAdv lowPriority = new TreeNodeAdv("Low");
    lowPriority.ShowOptionButton = true;
    
    categoryNode.Nodes.AddRange(new[] { highPriority, mediumPriority, lowPriority });
    treeViewAdv1.Nodes.Add(categoryNode);
}
```

**Note:** Option buttons within the same parent node typically behave as mutually exclusive - selecting one deselects others.

### VB.NET Example

```vb
' Configure option button
Dim node As TreeNodeAdv = New TreeNodeAdv("Option")
node.ShowOptionButton = True
node.SelectedOptionButtonColor = System.Drawing.Color.Red
node.OptionButtonColor = System.Drawing.Color.AliceBlue
node.Optioned = True
```

## Node Images

TreeViewAdv supports multiple image types for enhanced visual representation.

### Image Types

1. **LeftImageList** - Images displayed on left side of node text
2. **RightImageList** - Images displayed on right side of node text  
3. **StateImageList** - Images for expanded/collapsed states
4. **ExpandImageIndex/CollapseImageIndex** - Custom expand/collapse icons

### Setting Up ImageLists

```csharp
// Create ImageList
ImageList leftImages = new ImageList();
leftImages.ImageSize = new Size(16, 16);
leftImages.Images.Add("folder", Image.FromFile("folder.png"));
leftImages.Images.Add("file", Image.FromFile("file.png"));
leftImages.Images.Add("document", Image.FromFile("document.png"));

// Assign to TreeViewAdv
treeViewAdv1.LeftImageList = leftImages;
```

### Assigning Images to Nodes

```csharp
TreeNodeAdv folderNode = new TreeNodeAdv("Documents");
folderNode.LeftImageIndices = new int[] { 0 }; // Use "folder" image

TreeNodeAdv fileNode = new TreeNodeAdv("File.txt");
fileNode.LeftImageIndices = new int[] { 1 }; // Use "file" image
```

### Multiple Images Per Node

```csharp
// Node can display multiple images
TreeNodeAdv multiImageNode = new TreeNodeAdv("Important File");
multiImageNode.LeftImageIndices = new int[] { 1, 2 }; // Show both file and document icons
```

### Right-Side Images

```csharp
ImageList rightImages = new ImageList();
rightImages.Images.Add("lock", Image.FromFile("lock.png"));
rightImages.Images.Add("star", Image.FromFile("star.png"));

treeViewAdv1.RightImageList = rightImages;

TreeNodeAdv secureNode = new TreeNodeAdv("Secure File");
secureNode.RightImageIndices = new int[] { 0 }; // Show lock icon on right
```

### State Images for Expanded/Collapsed

```csharp
ImageList stateImages = new ImageList();
stateImages.Images.Add("expanded", Image.FromFile("folder-open.png"));
stateImages.Images.Add("collapsed", Image.FromFile("folder-closed.png"));

treeViewAdv1.StateImageList = stateImages;

TreeNodeAdv folderNode = new TreeNodeAdv("Project Files");
folderNode.ExpandImageIndex = 0;  // Show when expanded
folderNode.CollapseImageIndex = 1; // Show when collapsed
```

### Complete File Explorer Example

```csharp
private void SetupFileExplorerImages()
{
    // Setup left images
    ImageList leftImages = new ImageList();
    leftImages.Images.Add("folder", Properties.Resources.FolderIcon);
    leftImages.Images.Add("file", Properties.Resources.FileIcon);
    leftImages.Images.Add("image", Properties.Resources.ImageIcon);
    leftImages.Images.Add("doc", Properties.Resources.DocumentIcon);
    
    treeViewAdv1.LeftImageList = leftImages;
    
    // Create folder with children
    TreeNodeAdv documents = new TreeNodeAdv("Documents");
    documents.LeftImageIndices = new int[] { 0 }; // Folder icon
    
    TreeNodeAdv file1 = new TreeNodeAdv("Report.docx");
    file1.LeftImageIndices = new int[] { 3 }; // Document icon
    
    TreeNodeAdv file2 = new TreeNodeAdv("Photo.jpg");
    file2.LeftImageIndices = new int[] { 2 }; // Image icon
    
    documents.Nodes.AddRange(new[] { file1, file2 });
    treeViewAdv1.Nodes.Add(documents);
}
```

## Control-Level vs Node-Level Properties

### Property Override Hierarchy

When both control-level and node-level properties are set, node-level takes precedence.

**Example:**

```csharp
// Control level: Hide checkboxes globally
treeViewAdv1.ShowCheckBoxes = false;

// Node level: Show checkbox for specific node
TreeNodeAdv node = new TreeNodeAdv("Override Example");
node.ShowCheckBox = true;  // This overrides control setting

// Result: Only this node shows checkbox
```

### Properties That Support Both Levels

| Feature | Control Property | Node Property |
|---------|------------------|---------------|
| Plus/Minus | `ShowPlusMinus` | `TreeNodeAdv.ShowPlusMinus` |
| Checkboxes | `ShowCheckBoxes` | `TreeNodeAdv.ShowCheckBox` |
| Option Buttons | `ShowOptionButtons` | `TreeNodeAdv.ShowOptionButton` |

### Properties Only at Control Level

- `ShowLines` - No node-level override
- `ShowRootLines` - No node-level override
- `LoadOnDemand` - No node-level override

### Best Practices

**Use control-level when:**
- Consistent behavior across all nodes
- Default appearance for tree

**Use node-level when:**
- Specific nodes need different appearance
- Mixed behavior within single tree
- Business logic determines visibility per node

**Example: Mixed Configuration**

```csharp
private void ConfigureMixedTree()
{
    // Default: No checkboxes, no option buttons
    treeViewAdv1.ShowCheckBoxes = false;
    treeViewAdv1.ShowOptionButtons = false;
    treeViewAdv1.ShowPlusMinus = true;
    
    // Category with checkboxes
    TreeNodeAdv category1 = new TreeNodeAdv("Multi-Select Items");
    TreeNodeAdv item1 = new TreeNodeAdv("Item 1");
    item1.ShowCheckBox = true;
    TreeNodeAdv item2 = new TreeNodeAdv("Item 2");
    item2.ShowCheckBox = true;
    category1.Nodes.AddRange(new[] { item1, item2 });
    
    // Category with option buttons
    TreeNodeAdv category2 = new TreeNodeAdv("Single-Select Items");
    TreeNodeAdv option1 = new TreeNodeAdv("Option A");
    option1.ShowOptionButton = true;
    TreeNodeAdv option2 = new TreeNodeAdv("Option B");
    option2.ShowOptionButton = true;
    category2.Nodes.AddRange(new[] { option1, option2 });
    
    // Normal category (no special controls)
    TreeNodeAdv category3 = new TreeNodeAdv("Normal Items");
    category3.Nodes.Add(new TreeNodeAdv("Plain Item"));
    
    treeViewAdv1.Nodes.AddRange(new[] { category1, category2, category3 });
}
```

## Troubleshooting

**Issue:** Plus/minus not showing for parent nodes
- **Solution:** Verify `ShowPlusMinus = true`, ensure nodes have children, check node-level `ShowPlusMinus` isn't explicitly `false`

**Issue:** ShowPlusOnExpand not working
- **Solution:** Set `LoadOnDemand = true` at control level - this is required for ShowPlusOnExpand to function

**Issue:** Checkboxes not appearing for specific nodes
- **Solution:** Check node-level `ShowCheckBox` property - it might override control-level `ShowCheckBoxes`

**Issue:** ShowRootLines has no effect
- **Solution:** Verify `ShowLines = true` - when ShowLines is false, ShowRootLines is ignored

**Issue:** Images not displaying
- **Solution:** Verify ImageList is assigned to control, check image indices are valid (within ImageList bounds)

**Issue:** Option buttons not mutually exclusive
- **Solution:** Ensure option button nodes share same parent - mutual exclusivity works within sibling group
