# Getting Started with TreeViewAdv

This guide covers the essential steps to create and configure a TreeViewAdv control in Windows Forms applications, including installation, adding the control via Designer or code, and basic node customization.

## Installation and Package Setup

### NuGet Package Installation

The recommended approach is to install via NuGet Package Manager:

```powershell
Install-Package Syncfusion.Tools.WinForms
```

Or use the NuGet Package Manager UI in Visual Studio:
1. Right-click project → Manage NuGet Packages
2. Search for "Syncfusion.Tools.WinForms"
3. Click Install

### Assembly References

If adding references manually, include:
- `Syncfusion.Tools.Windows.dll` - Contains TreeViewAdv control
- `Syncfusion.Shared.Base.dll` - Core shared functionality

## Creating a Windows Forms Application with TreeViewAdv

### Step 1: Create Project

1. Open Visual Studio
2. Create new Windows Forms App (.NET Framework or .NET 5+)
3. Name your project (e.g., "TreeViewDemo")

### Step 2: Register Syncfusion License

Add license registration in `Program.cs` before application runs:

```csharp
using System;
using System.Windows.Forms;

namespace TreeViewDemo
{
    static class Program
    {
        [STAThread]
        static void Main()
        {
            // Register Syncfusion license
            Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
            
            Application.EnableVisualStyles();
            Application.SetCompatibleTextRenderingDefault(false);
            Application.Run(new Form1());
        }
    }
}
```

## Adding Control via Designer

### Using Toolbox Drag-Drop

1. Open form in Designer view
2. Locate "Syncfusion Controls" section in Toolbox
3. Find **TreeViewAdv** control
4. Drag and drop onto form

The required assembly references are added automatically.

![TreeViewAdv in Designer]

### Using Smart Tag

After adding control to form, click the smart tag (arrow icon) on TreeViewAdv to access:
- **Edit Node Collection** - Opens NodeCollection Editor
- **Choose Style** - Apply visual themes
- **Common Properties** - Quick property settings

### NodeCollection Editor

To add nodes at design time:

1. Select TreeViewAdv control on form
2. Click smart tag → **Edit Node Collection**
3. Or right-click control → **Nodes Editor**
4. Or open from Properties window → **Nodes** property → click ellipsis (...)

**NodeCollection Editor Options:**
- **Add Node** - Adds sibling node to selected node (or root node if none selected)
- **Add Child** - Adds child node under selected node
- **Remove** - Deletes selected node
- **Drag-Drop** - Move nodes to different parents by dragging

**Node Properties in Editor:**
- `Text` - Display text for the node
- `Name` - Identifier for the node
- `Tag` - Custom data object
- `ShowCheckBox` - Display checkbox
- `ShowPlusMinus` - Display expand/collapse button

### Example: Adding Nodes via Designer

1. Click "Add Node" → Creates "Node0"
2. Change `Text` property to "Parent 1"
3. With "Parent 1" selected, click "Add Child" → Creates child
4. Change child `Text` to "Child 1"
5. Click "Add Child" again for another child
6. Click "OK" to save

## Adding Control Manually in Code

### C# Implementation

```csharp
using Syncfusion.Windows.Forms.Tools;
using System;
using System.Drawing;
using System.Windows.Forms;

namespace WindowsFormsApp
{
    public partial class Form1 : Form
    {
        private TreeViewAdv treeViewAdv1;
        
        public Form1()
        {
            InitializeComponent();
            InitializeTreeView();
        }
        
        private void InitializeTreeView()
        {
            // Create TreeViewAdv instance
            treeViewAdv1 = new TreeViewAdv();
            
            // Set location and size
            treeViewAdv1.Location = new Point(85, 108);
            treeViewAdv1.Size = new Size(240, 150);
            treeViewAdv1.Name = "treeViewAdv1";
            
            // Add to form controls
            this.Controls.Add(treeViewAdv1);
        }
    }
}
```

### VB.NET Implementation

```vb
Imports Syncfusion.Windows.Forms.Tools
Imports System.Drawing
Imports System.Windows.Forms

Namespace WindowsFormsApp
    Public Partial Class Form1
        Inherits Form
        
        Private treeViewAdv1 As TreeViewAdv
        
        Public Sub New()
            InitializeComponent()
            InitializeTreeView()
        End Sub
        
        Private Sub InitializeTreeView()
            ' Create TreeViewAdv instance
            treeViewAdv1 = New TreeViewAdv()
            
            ' Set location and size
            treeViewAdv1.Location = New Point(85, 108)
            treeViewAdv1.Size = New Size(240, 150)
            treeViewAdv1.Name = "treeViewAdv1"
            
            ' Add to form controls
            Me.Controls.Add(treeViewAdv1)
        End Sub
    End Class
End Namespace
```

## Adding Nodes Programmatically

### Simple Node Creation

```csharp
// Create individual nodes
TreeNodeAdv node1 = new TreeNodeAdv("Parent 1");
TreeNodeAdv node2 = new TreeNodeAdv("Parent 2");

// Add to TreeViewAdv
treeViewAdv1.Nodes.Add(node1);
treeViewAdv1.Nodes.Add(node2);
```

### Creating Parent-Child Hierarchy

```csharp
// Create child nodes
TreeNodeAdv child1 = new TreeNodeAdv("Child1");
TreeNodeAdv child2 = new TreeNodeAdv("Child2");

// Create parent with children in constructor
TreeNodeAdv parent = new TreeNodeAdv("Parent", new TreeNodeAdv[] { child1, child2 });

// Add parent to tree (children added automatically)
treeViewAdv1.Nodes.Add(parent);
```

### Complete Example with Multiple Levels

```csharp
private void InitializeTreeView()
{
    // Initialize TreeViewAdv
    treeViewAdv1 = new TreeViewAdv();
    treeViewAdv1.Location = new Point(202, 75);
    treeViewAdv1.Size = new Size(377, 250);
    treeViewAdv1.Name = "treeView1";
    
    // Create node hierarchy
    TreeNodeAdv child1 = new TreeNodeAdv("Node1");
    child1.Name = "Node1";
    child1.Text = "Child1";
    
    TreeNodeAdv parent1 = new TreeNodeAdv("Node0", new TreeNodeAdv[] { child1 });
    parent1.Name = "Node0";
    parent1.Text = "Parent";
    
    TreeNodeAdv child2 = new TreeNodeAdv("Node3");
    child2.Name = "Node3";
    child2.Text = "Child1";
    
    TreeNodeAdv parent2 = new TreeNodeAdv("Node2", new TreeNodeAdv[] { child2 });
    parent2.Name = "Node2";
    parent2.Text = "Parent1";
    
    TreeNodeAdv child3 = new TreeNodeAdv("Node5");
    child3.Name = "Node5";
    child3.Text = "Child1";
    
    TreeNodeAdv parent3 = new TreeNodeAdv("Node4", new TreeNodeAdv[] { child3 });
    parent3.Name = "Node4";
    parent3.Text = "Parent2";
    
    // Add all root nodes to TreeViewAdv
    treeViewAdv1.Nodes.AddRange(new TreeNodeAdv[] { parent1, parent2, parent3 });
    
    // Add to form
    this.Controls.Add(treeViewAdv1);
}
```

### VB.NET Example

```vb
Private Sub InitializeTreeView()
    ' Initialize TreeViewAdv
    treeViewAdv1 = New TreeViewAdv()
    treeViewAdv1.Location = New Point(202, 75)
    treeViewAdv1.Size = New Size(377, 250)
    treeViewAdv1.Name = "treeView1"
    
    ' Create node hierarchy
    Dim child1 As TreeNodeAdv = New TreeNodeAdv("Node1")
    child1.Name = "Node1"
    child1.Text = "Child1"
    
    Dim parent1 As TreeNodeAdv = New TreeNodeAdv("Node0", New TreeNodeAdv() {child1})
    parent1.Name = "Node0"
    parent1.Text = "Parent"
    
    Dim child2 As TreeNodeAdv = New TreeNodeAdv("Node3")
    child2.Name = "Node3"
    child2.Text = "Child1"
    
    Dim parent2 As TreeNodeAdv = New TreeNodeAdv("Node2", New TreeNodeAdv() {child2})
    parent2.Name = "Node2"
    parent2.Text = "Parent1"
    
    ' Add all root nodes to TreeViewAdv
    treeViewAdv1.Nodes.AddRange(New TreeNodeAdv() {parent1, parent2})
    
    ' Add to form
    Me.Controls.Add(treeViewAdv1)
End Sub
```

## Basic Node Customization

### Showing/Hiding Connecting Lines

**ShowLines Property** - Controls connecting lines between child nodes:

```csharp
// Show connecting lines (default: true)
treeViewAdv1.ShowLines = true;

// Hide connecting lines
treeViewAdv1.ShowLines = false;
```

**ShowRootLines Property** - Controls lines between root nodes:

```csharp
// Show lines between root nodes (default: true)
treeViewAdv1.ShowRootLines = true;

// Hide root lines
treeViewAdv1.ShowRootLines = false;
```

**Note:** If `ShowLines` is false, connecting lines are hidden for the entire control regardless of `ShowRootLines`.

### Plus/Minus Signs

**Control-Level Configuration:**

```csharp
// Show plus/minus for all parent nodes (default: true)
treeViewAdv1.ShowPlusMinus = true;

// Hide plus/minus for all nodes
treeViewAdv1.ShowPlusMinus = false;
```

**Node-Level Configuration:**

```csharp
// Override at node level
TreeNodeAdv node = new TreeNodeAdv("Custom Node");
node.ShowPlusMinus = true; // Show even if control setting is false
```

**ShowPlusOnExpand** - Keep plus sign even when expanded:

```csharp
// Required for ShowPlusOnExpand to work
treeViewAdv1.LoadOnDemand = true;

// Node keeps plus sign when expanded (useful for load-on-demand)
node.ShowPlusOnExpand = true;
```

### CheckBoxes

**Control-Level Configuration:**

```csharp
// Show checkboxes for all nodes
treeViewAdv1.ShowCheckBoxes = true;

// Hide checkboxes for all nodes (default)
treeViewAdv1.ShowCheckBoxes = false;
```

**Node-Level Configuration:**

```csharp
// Show checkbox for specific node
TreeNodeAdv node = new TreeNodeAdv("Checkable Node");
node.ShowCheckBox = true;
treeViewAdv1.Nodes.Add(node);

// Mixed configuration
treeViewAdv1.ShowCheckBoxes = false; // Hide globally
node1.ShowCheckBox = true;  // Show for specific node
node2.ShowCheckBox = false; // Hide for specific node
```

### Option Buttons (Radio Buttons)

**Control-Level Configuration:**

```csharp
// Show option buttons for all nodes
treeViewAdv1.ShowOptionButtons = true;

// Hide option buttons (default)
treeViewAdv1.ShowOptionButtons = false;
```

**Node-Level Configuration:**

```csharp
// Show option button for specific node
TreeNodeAdv node = new TreeNodeAdv("Radio Node");
node.ShowOptionButton = true;
treeViewAdv1.Nodes.Add(node);
```

**Use Case:** Option buttons are useful for single-selection scenarios within a group of sibling nodes.

## Assigning Active Node

The **ActiveNode** property holds the currently selected node:

```csharp
// Get currently selected node
TreeNodeAdv selectedNode = treeViewAdv1.ActiveNode;

if (selectedNode != null)
{
    MessageBox.Show($"Selected: {selectedNode.Text}");
}

// Set active node programmatically
treeViewAdv1.ActiveNode = specificNode;
```

**Default:** `ActiveNode` is `null` when no node is selected.

## Quick Configuration Example

Combine all basic customizations:

```csharp
private void SetupTreeView()
{
    // Create and configure TreeViewAdv
    treeViewAdv1 = new TreeViewAdv();
    treeViewAdv1.Location = new Point(20, 20);
    treeViewAdv1.Size = new Size(300, 400);
    
    // Visual configuration
    treeViewAdv1.ShowLines = true;
    treeViewAdv1.ShowRootLines = true;
    treeViewAdv1.ShowPlusMinus = true;
    treeViewAdv1.ShowCheckBoxes = true;
    
    // Add nodes
    TreeNodeAdv parent1 = new TreeNodeAdv("Documents");
    parent1.Nodes.Add(new TreeNodeAdv("File1.txt"));
    parent1.Nodes.Add(new TreeNodeAdv("File2.docx"));
    
    TreeNodeAdv parent2 = new TreeNodeAdv("Pictures");
    parent2.Nodes.Add(new TreeNodeAdv("Photo1.jpg"));
    parent2.Nodes.Add(new TreeNodeAdv("Photo2.png"));
    
    treeViewAdv1.Nodes.AddRange(new TreeNodeAdv[] { parent1, parent2 });
    
    // Add to form
    this.Controls.Add(treeViewAdv1);
}
```

## Troubleshooting

**Issue:** TreeViewAdv not appearing in Toolbox
- **Solution:** Rebuild solution, restart Visual Studio, verify NuGet package installation

**Issue:** Nodes not displaying
- **Solution:** Check that nodes were added to `treeViewAdv1.Nodes` collection, verify control is added to form's Controls

**Issue:** Plus/minus not showing for parent nodes
- **Solution:** Verify `ShowPlusMinus = true`, ensure nodes have children

**Issue:** Assembly reference errors
- **Solution:** Install Syncfusion.Tools.WinForms NuGet package, or manually reference Syncfusion.Tools.Windows.dll

**Issue:** License key errors
- **Solution:** Verify license registration in `Program.cs` Main method before `Application.Run()`

## Next Steps

- **Data Binding:** Learn to bind TreeViewAdv to data sources (DataTable, custom objects)
- **Styling:** Customize appearance with themes, colors, fonts, and borders
- **Drag-Drop:** Implement node reordering via drag-and-drop
- **Performance:** Optimize for large datasets with virtualization and load-on-demand
