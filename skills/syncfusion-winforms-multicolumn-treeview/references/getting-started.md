# Getting Started with MultiColumnTreeView

This guide covers installation, basic setup, and creating your first MultiColumnTreeView with columns, nodes, and subitems.

## Assembly Deployment

The MultiColumnTreeView control requires the following assemblies:

**Required Assemblies:**
- `Syncfusion.Grid.Base.dll`
- `Syncfusion.Grid.Windows.dll`
- `Syncfusion.Shared.Base.dll`
- `Syncfusion.Shared.Windows.dll`
- `Syncfusion.Tools.Base.dll`
- `Syncfusion.Tools.Windows.dll`

**Installation via NuGet:**

```powershell
Install-Package Syncfusion.Tools.Windows
```

This command installs the package with all dependencies. You can also use the NuGet Package Manager UI in Visual Studio.

**Manual Installation:**

If using Syncfusion Control Panel, install Essential Studio for Windows Forms. The assemblies will be added automatically when you drag the control from the toolbox.

## Adding MultiColumnTreeView Through Designer

The easiest way to add a MultiColumnTreeView is via the Visual Studio designer:

**Step 1: Drag and Drop**

1. Open your form in the Visual Studio Designer
2. Locate **MultiColumnTreeView** in the Toolbox (under Syncfusion Controls)
3. Drag it onto your form
4. The required assemblies will be added automatically to your project

![Drag from toolbox](The control appears in the Toolbox after installing Syncfusion)

**Step 2: Configure Properties**

Set desired properties through the Properties window:
- **Size and Location**: Position the control on your form
- **Dock**: Set to Fill for full form coverage
- **Name**: Give it a meaningful name (e.g., `employeeTreeView`)

The control is now ready to configure with columns and data.

## Adding MultiColumnTreeView Through Code

For more control or dynamic scenarios, add the control programmatically:

**Step 1: Create a New Windows Forms Project**

Create a C# or VB.NET Windows Forms application in Visual Studio.

**Step 2: Add Assembly References**

Add references to the required Syncfusion assemblies (listed above).

**Step 3: Import Namespaces**

In C#:
```csharp
using Syncfusion.Windows.Forms.Tools.MultiColumnTreeView;
```

In VB.NET:
```vb
Imports Syncfusion.Windows.Forms.Tools.MultiColumnTreeView
```

**Step 4: Create and Add the Control**

In C#:
```csharp
public partial class Form1 : Form
{
    private MultiColumnTreeView multiColumnTreeView1;
    
    public Form1()
    {
        InitializeComponent();
        
        // Create control instance
        this.multiColumnTreeView1 = new MultiColumnTreeView();
        this.multiColumnTreeView1.Location = new System.Drawing.Point(10, 10);
        this.multiColumnTreeView1.Size = new System.Drawing.Size(600, 400);
        
        // Add to form
        this.Controls.Add(this.multiColumnTreeView1);
    }
}
```

In VB.NET:
```vb
Public Class Form1
    Private multiColumnTreeView1 As MultiColumnTreeView
    
    Public Sub New()
        InitializeComponent()
        
        ' Create control instance
        Me.multiColumnTreeView1 = New MultiColumnTreeView()
        Me.multiColumnTreeView1.Location = New System.Drawing.Point(10, 10)
        Me.multiColumnTreeView1.Size = New System.Drawing.Size(600, 400)
        
        ' Add to form
        Me.Controls.Add(Me.multiColumnTreeView1)
    End Sub
End Class
```

## Adding Columns

Columns define the structure of your MultiColumnTreeView. The first column displays the tree hierarchy, and additional columns display related data.

**Creating Columns in Code:**

In C#:
```csharp
// Create column instances
TreeColumnAdv countryColumn = new TreeColumnAdv();
TreeColumnAdv capitalColumn = new TreeColumnAdv();

// Configure column properties
countryColumn.Text = "Country";
countryColumn.Width = 150;

capitalColumn.Text = "Capital";
capitalColumn.Width = 150;

// Add columns to the control
multiColumnTreeView1.Columns.AddRange(
    new TreeColumnAdv[] { countryColumn, capitalColumn });
```

In VB.NET:
```vb
' Create column instances
Dim countryColumn As New TreeColumnAdv()
Dim capitalColumn As New TreeColumnAdv()

' Configure column properties
countryColumn.Text = "Country"
countryColumn.Width = 150

capitalColumn.Text = "Capital"
capitalColumn.Width = 150

' Add columns to the control
multiColumnTreeView1.Columns.AddRange(
    New TreeColumnAdv() { countryColumn, capitalColumn })
```

**Key Points:**
- Always add columns before adding nodes
- The first column displays the tree hierarchy
- Additional columns require subitems in nodes
- Column width is in pixels

## Adding Nodes

Nodes represent items in the tree. Each node can have child nodes to create hierarchy.

**Adding Root-Level Nodes:**

In C#:
```csharp
// Create parent nodes
TreeNodeAdv asiaNode = new TreeNodeAdv();
asiaNode.Text = "Asia";

TreeNodeAdv europeNode = new TreeNodeAdv();
europeNode.Text = "Europe";

TreeNodeAdv northAmericaNode = new TreeNodeAdv();
northAmericaNode.Text = "North America";

// Add to control
multiColumnTreeView1.Nodes.AddRange(
    new TreeNodeAdv[] { asiaNode, europeNode, northAmericaNode });
```

In VB.NET:
```vb
' Create parent nodes
Dim asiaNode As New TreeNodeAdv()
asiaNode.Text = "Asia"

Dim europeNode As New TreeNodeAdv()
europeNode.Text = "Europe"

Dim northAmericaNode As New TreeNodeAdv()
northAmericaNode.Text = "North America"

' Add to control
multiColumnTreeView1.Nodes.AddRange(
    New TreeNodeAdv() { asiaNode, europeNode, northAmericaNode })
```

## Adding Child Nodes

Child nodes create the hierarchical structure:

In C#:
```csharp
// Create child nodes for Asia
TreeNodeAdv indiaNode = new TreeNodeAdv();
indiaNode.Text = "India";

TreeNodeAdv chinaNode = new TreeNodeAdv();
chinaNode.Text = "China";

// Add children to parent
asiaNode.Nodes.AddRange(new TreeNodeAdv[] { indiaNode, chinaNode });

// Create child nodes for Europe
TreeNodeAdv ukNode = new TreeNodeAdv();
ukNode.Text = "United Kingdom";

TreeNodeAdv franceNode = new TreeNodeAdv();
franceNode.Text = "France";

europeNode.Nodes.AddRange(new TreeNodeAdv[] { ukNode, franceNode });
```

In VB.NET:
```vb
' Create child nodes for Asia
Dim indiaNode As New TreeNodeAdv()
indiaNode.Text = "India"

Dim chinaNode As New TreeNodeAdv()
chinaNode.Text = "China"

' Add children to parent
asiaNode.Nodes.AddRange(New TreeNodeAdv() { indiaNode, chinaNode })

' Create child nodes for Europe
Dim ukNode As New TreeNodeAdv()
ukNode.Text = "United Kingdom"

Dim franceNode As New TreeNodeAdv()
franceNode.Text = "France"

europeNode.Nodes.AddRange(New TreeNodeAdv() { ukNode, franceNode })
```

## Adding SubItems

SubItems populate the additional columns for each node:

In C#:
```csharp
// Create subitem for capital column
TreeNodeAdvSubItem delhiSubItem = new TreeNodeAdvSubItem();
delhiSubItem.Text = "New Delhi";

TreeNodeAdvSubItem beijingSubItem = new TreeNodeAdvSubItem();
beijingSubItem.Text = "Beijing";

TreeNodeAdvSubItem londonSubItem = new TreeNodeAdvSubItem();
londonSubItem.Text = "London";

TreeNodeAdvSubItem parisSubItem = new TreeNodeAdvSubItem();
parisSubItem.Text = "Paris";

// Add subitems to nodes
indiaNode.SubItems.Add(delhiSubItem);
chinaNode.SubItems.Add(beijingSubItem);
ukNode.SubItems.Add(londonSubItem);
franceNode.SubItems.Add(parisSubItem);
```

In VB.NET:
```vb
' Create subitem for capital column
Dim delhiSubItem As New TreeNodeAdvSubItem()
delhiSubItem.Text = "New Delhi"

Dim beijingSubItem As New TreeNodeAdvSubItem()
beijingSubItem.Text = "Beijing"

Dim londonSubItem As New TreeNodeAdvSubItem()
londonSubItem.Text = "London"

Dim parisSubItem As New TreeNodeAdvSubItem()
parisSubItem.Text = "Paris"

' Add subitems to nodes
indiaNode.SubItems.Add(delhiSubItem)
chinaNode.SubItems.Add(beijingSubItem)
ukNode.SubItems.Add(londonSubItem)
franceNode.SubItems.Add(parisSubItem)
```

**Important Notes:**
- The number of subitems should match (columns count - 1)
- SubItems are added in column order (excluding the first column)
- Parent nodes can have subitems too

## Complete Working Example

Here's a complete example bringing it all together:

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools.MultiColumnTreeView;

public class GettingStartedForm : Form
{
    private MultiColumnTreeView multiColumnTreeView1;
    
    public GettingStartedForm()
    {
        InitializeForm();
        CreateColumns();
        PopulateData();
    }
    
    private void InitializeForm()
    {
        this.Text = "MultiColumnTreeView - Getting Started";
        this.Size = new Size(700, 500);
        
        // Create and configure control
        multiColumnTreeView1 = new MultiColumnTreeView();
        multiColumnTreeView1.Dock = DockStyle.Fill;
        multiColumnTreeView1.ShowLines = true;
        multiColumnTreeView1.ShowRootLines = true;
        
        this.Controls.Add(multiColumnTreeView1);
    }
    
    private void CreateColumns()
    {
        // Country column (shows tree hierarchy)
        TreeColumnAdv countryColumn = new TreeColumnAdv();
        countryColumn.Text = "Country/Region";
        countryColumn.Width = 200;
        
        // Capital column
        TreeColumnAdv capitalColumn = new TreeColumnAdv();
        capitalColumn.Text = "Capital";
        capitalColumn.Width = 150;
        
        // Population column
        TreeColumnAdv populationColumn = new TreeColumnAdv();
        populationColumn.Text = "Population (M)";
        populationColumn.Width = 120;
        
        multiColumnTreeView1.Columns.AddRange(
            new TreeColumnAdv[] { countryColumn, capitalColumn, populationColumn });
    }
    
    private void PopulateData()
    {
        // Create continents (parent nodes)
        TreeNodeAdv asiaNode = new TreeNodeAdv { Text = "Asia" };
        TreeNodeAdv europeNode = new TreeNodeAdv { Text = "Europe" };
        
        // Create countries with data
        TreeNodeAdv indiaNode = new TreeNodeAdv { Text = "India" };
        indiaNode.SubItems.Add(new TreeNodeAdvSubItem { Text = "New Delhi" });
        indiaNode.SubItems.Add(new TreeNodeAdvSubItem { Text = "1,380" });
        
        TreeNodeAdv chinaNode = new TreeNodeAdv { Text = "China" };
        chinaNode.SubItems.Add(new TreeNodeAdvSubItem { Text = "Beijing" });
        chinaNode.SubItems.Add(new TreeNodeAdvSubItem { Text = "1,440" });
        
        TreeNodeAdv ukNode = new TreeNodeAdv { Text = "United Kingdom" };
        ukNode.SubItems.Add(new TreeNodeAdvSubItem { Text = "London" });
        ukNode.SubItems.Add(new TreeNodeAdvSubItem { Text = "67" });
        
        TreeNodeAdv franceNode = new TreeNodeAdv { Text = "France" };
        franceNode.SubItems.Add(new TreeNodeAdvSubItem { Text = "Paris" });
        franceNode.SubItems.Add(new TreeNodeAdvSubItem { Text = "65" });
        
        // Build hierarchy
        asiaNode.Nodes.AddRange(new TreeNodeAdv[] { indiaNode, chinaNode });
        europeNode.Nodes.AddRange(new TreeNodeAdv[] { ukNode, franceNode });
        
        // Add to control
        multiColumnTreeView1.Nodes.AddRange(
            new TreeNodeAdv[] { asiaNode, europeNode });
    }
    
    [STAThread]
    static void Main()
    {
        Application.EnableVisualStyles();
        Application.SetCompatibleTextRenderingDefault(false);
        Application.Run(new GettingStartedForm());
    }
}
```

This example creates a three-column tree showing continents, countries, capitals, and populations.

## Common Gotchas

**Issue: SubItems don't appear**
- **Cause:** Columns not added before nodes, or subitem count doesn't match column count
- **Solution:** Always add columns first, ensure subitems count = columns count - 1

**Issue: Nodes all at root level**
- **Cause:** Adding child nodes to control instead of parent node
- **Solution:** Use `parentNode.Nodes.Add(childNode)` not `control.Nodes.Add(childNode)`

**Issue: Assembly not found errors**
- **Cause:** Missing assembly references
- **Solution:** Install via NuGet or add all six required assemblies manually

## Next Steps

Now that you have a basic MultiColumnTreeView running:
- **Customize appearance** - See [appearance.md](appearance.md) for styling options
- **Add interactivity** - See [node-features.md](node-features.md) for checkboxes and option buttons
- **Handle events** - See [events.md](events.md) for responding to user actions
- **Optimize performance** - See [performance.md](performance.md) for large datasets
- **Implement sorting** - See [sorting-and-filtering.md](sorting-and-filtering.md) for data organization
