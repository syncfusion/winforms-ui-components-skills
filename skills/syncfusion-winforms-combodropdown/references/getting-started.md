# Getting Started with ComboDropDown

This guide covers the initial setup and basic configuration of the ComboDropDown control in Windows Forms applications.

## Installation and Assembly References

### Required Assembly

**Assembly:** `Syncfusion.Shared.Base.dll`

This assembly contains the `ComboDropDown` control and supporting classes. The NuGet package `Syncfusion.Shared.Base` delivers this assembly for manual referencing scenarios.

### NuGet Package Installation

Install via NuGet Package Manager:

```powershell
Install-Package Syncfusion.Shared.Base
```

**Package:** `Syncfusion.Shared.Base`  
**Namespace:** `Syncfusion.Windows.Forms.Tools`

### Supported Frameworks

- .NET Framework 4.5, 4.5.1, 4.6, 4.7, 4.8
- .NET 6.0, 7.0, 8.0 (Windows)

## Namespace Imports

Add the required namespace to your form or class:

```csharp
using Syncfusion.Windows.Forms.Tools;
```

```vb
Imports Syncfusion.Windows.Forms.Tools
```

## Designer-Based Setup

Follow these steps to set up ComboDropDown using the Visual Studio Designer:

### Step 1: Create New Project

Create a new Visual C# or VB.NET Windows Forms application in Visual Studio.

### Step 2: Add Controls to Form

1. Open the Toolbox (View → Toolbox or Ctrl+Alt+X)
2. Locate **ComboDropDown** in the Syncfusion Controls section
3. Drag and drop **ComboDropDown** onto the form
4. Drag and drop the control you want to host in the dropdown (e.g., **TreeView**)

### Step 3: Configure the TreeView (Example)

If using TreeView as the dropdown control:

1. Select the TreeView control
2. Click the smart tag or use the Properties window
3. Add nodes to the TreeView:
   - Right-click TreeView → Edit Nodes
   - Add root nodes and child nodes as needed
4. Set `HideSelection` property to `false`

**Why set HideSelection to false?**  
This ensures the selected TreeNode remains highlighted even when the TreeView loses focus, providing better visual feedback.

### Step 4: Associate Control with ComboDropDown

1. Select the ComboDropDown control
2. In the Properties window, find the `PopupControl` property
3. Click the dropdown and select your TreeView (or other control)

### Step 5: Wire Up Event Handlers

1. Select ComboDropDown
2. Click the Events button (lightning bolt) in Properties window
3. Double-click the `DropDown` event to create a handler
4. Select the TreeView control
5. Double-click the `DoubleClick` event to create a handler

**Result:** You now have a ComboDropDown with an associated control and event stubs ready for interaction logic.

## Code-Based Setup

### Basic Programmatic Creation

```csharp
using Syncfusion.Windows.Forms.Tools;

// Create ComboDropDown instance
private ComboDropDown comboDropDown1;

public Form1()
{
    InitializeComponent();
    
    // Initialize ComboDropDown
    this.comboDropDown1 = new ComboDropDown();
    this.comboDropDown1.Location = new Point(20, 20);
    this.comboDropDown1.Size = new Size(200, 25);
    this.comboDropDown1.Name = "comboDropDown1";
    
    // Add to form
    this.Controls.Add(this.comboDropDown1);
}
```

```vb
Imports Syncfusion.Windows.Forms.Tools

' Create ComboDropDown instance
Private comboDropDown1 As ComboDropDown

Public Sub New()
    InitializeComponent()
    
    ' Initialize ComboDropDown
    Me.comboDropDown1 = New ComboDropDown()
    Me.comboDropDown1.Location = New Point(20, 20)
    Me.comboDropDown1.Size = New Size(200, 25)
    Me.comboDropDown1.Name = "comboDropDown1"
    
    ' Add to form
    Me.Controls.Add(Me.comboDropDown1)
End Sub
```

### Complete TreeView Integration Example

```csharp
using Syncfusion.Windows.Forms.Tools;

private ComboDropDown comboDropDown1;
private TreeView treeView1;

public Form1()
{
    InitializeComponent();
    SetupComboDropDown();
}

private void SetupComboDropDown()
{
    // Create TreeView
    this.treeView1 = new TreeView();
    this.treeView1.HideSelection = false;
    
    // Add sample nodes
    TreeNode rootNode = new TreeNode("Categories");
    rootNode.Nodes.Add("Electronics");
    rootNode.Nodes.Add("Clothing");
    rootNode.Nodes.Add("Books");
    this.treeView1.Nodes.Add(rootNode);
    
    rootNode.Nodes[0].Nodes.Add("Laptops");
    rootNode.Nodes[0].Nodes.Add("Phones");
    
    // Create ComboDropDown
    this.comboDropDown1 = new ComboDropDown();
    this.comboDropDown1.Location = new Point(20, 20);
    this.comboDropDown1.Size = new Size(250, 25);
    
    // Associate TreeView with ComboDropDown
    this.comboDropDown1.PopupControl = this.treeView1;
    
    // Wire up events (see event-handling.md for implementation)
    this.comboDropDown1.DropDown += ComboDropDown1_DropDown;
    this.treeView1.DoubleClick += TreeView1_DoubleClick;
    
    // Add to form
    this.Controls.Add(this.comboDropDown1);
}

private void ComboDropDown1_DropDown(object sender, EventArgs e)
{
    // Sync text to TreeView selection (before dropdown shown)
    // Implementation in event-handling.md
}

private void TreeView1_DoubleClick(object sender, EventArgs e)
{
    // Sync TreeView selection to text (on double-click)
    // Implementation in event-handling.md
}
```

```vb
Imports Syncfusion.Windows.Forms.Tools

Private comboDropDown1 As ComboDropDown
Private treeView1 As TreeView

Public Sub New()
    InitializeComponent()
    SetupComboDropDown()
End Sub

Private Sub SetupComboDropDown()
    ' Create TreeView
    Me.treeView1 = New TreeView()
    Me.treeView1.HideSelection = False
    
    ' Add sample nodes
    Dim rootNode As New TreeNode("Categories")
    rootNode.Nodes.Add("Electronics")
    rootNode.Nodes.Add("Clothing")
    rootNode.Nodes.Add("Books")
    Me.treeView1.Nodes.Add(rootNode)
    
    rootNode.Nodes(0).Nodes.Add("Laptops")
    rootNode.Nodes(0).Nodes.Add("Phones")
    
    ' Create ComboDropDown
    Me.comboDropDown1 = New ComboDropDown()
    Me.comboDropDown1.Location = New Point(20, 20)
    Me.comboDropDown1.Size = New Size(250, 25)
    
    ' Associate TreeView with ComboDropDown
    Me.comboDropDown1.PopupControl = Me.treeView1
    
    ' Wire up events
    AddHandler Me.comboDropDown1.DropDown, AddressOf ComboDropDown1_DropDown
    AddHandler Me.treeView1.DoubleClick, AddressOf TreeView1_DoubleClick
    
    ' Add to form
    Me.Controls.Add(Me.comboDropDown1)
End Sub

Private Sub ComboDropDown1_DropDown(sender As Object, e As EventArgs)
    ' Sync text to TreeView selection (before dropdown shown)
End Sub

Private Sub TreeView1_DoubleClick(sender As Object, e As EventArgs)
    ' Sync TreeView selection to text (on double-click)
End Sub
```

## Basic Configuration

### Setting Initial Text

```csharp
// Set default text in edit portion
comboDropDown1.Text = "Select an item...";
```

### Configuring Popup Size

The popup size is determined by the hosted control's size:

```csharp
// Set TreeView size (affects popup size)
treeView1.Size = new Size(250, 200);
```

### Setting Dropdown Button Appearance

```csharp
// Flat appearance
comboDropDown1.FlatStyle = FlatStyle.Flat;
comboDropDown1.FlatBorderColor = Color.Gray;
```

## Troubleshooting Common Setup Issues

### Issue 1: Popup Not Showing

**Symptom:** Clicking dropdown button does nothing  
**Cause:** PopupControl property not set  
**Solution:** Ensure `comboDropDown1.PopupControl = yourControl;` is called

### Issue 2: Text Not Syncing

**Symptom:** Selecting items in dropdown doesn't update combo text  
**Cause:** Missing event handlers for interaction  
**Solution:** Implement DropDown and control-specific events (see event-handling.md)

### Issue 3: TreeView Selection Not Visible

**Symptom:** Can't see which TreeNode is selected  
**Cause:** HideSelection property is true (default)  
**Solution:** Set `treeView1.HideSelection = false;`

### Issue 4: Dropdown Closes Immediately

**Symptom:** Dropdown closes as soon as it opens  
**Cause:** Conflicting event handlers or validation errors  
**Solution:** Check event handler logic, ensure no exceptions thrown in DropDown event

### Issue 5: Control Not Visible in Designer

**Symptom:** ComboDropDown not in Toolbox  
**Cause:** Assembly not referenced or Toolbox not configured  
**Solution:** 
- Right-click Toolbox → Choose Items
- Browse to `Syncfusion.Shared.Base.dll` (or add the `Syncfusion.Shared.Base` NuGet package)
- Check ComboDropDown and click OK

## Next Steps

**For interaction logic:** See [event-handling.md](event-handling.md) for implementing data synchronization between the combo text and hosted control.

**For text input control:** See [text-behavior.md](text-behavior.md) for CharacterCasing, NumberOnly, ReadOnly, and other input options.

**For styling:** See [appearance-customization.md](appearance-customization.md) and [themes-and-styles.md](themes-and-styles.md) for visual customization.
