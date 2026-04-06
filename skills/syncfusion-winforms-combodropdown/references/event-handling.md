# Event Handling and Interaction

## Table of Contents
- [Overview](#overview)
- [Event Lifecycle](#event-lifecycle)
- [DropDown Event](#dropdown-event)
- [Control-Specific Events](#control-specific-events)
- [Popup Event](#popup-event)
- [Complete TreeView Integration](#complete-treeview-integration)
- [GridList Integration](#gridlist-integration)
- [Suppressing Events](#suppressing-events)
- [Best Practices](#best-practices)

## Overview

ComboDropDown requires you to implement the interaction logic between the edit portion (text box) and the dropdown control. This is done through event handlers that transfer data bidirectionally:

- **Text → Control:** Before dropdown shows, sync the control's selection to match the combo's text
- **Control → Text:** When user selects in the control, transfer selection to the combo's text and close dropdown

**Key Events:**
- `DropDown` - Fires before dropdown is shown (sync text → control selection)
- Control-specific events (e.g., `DoubleClick`, `SelectedIndexChanged`) - Transfer selection to text
- `Popup` - Fires after dropdown is shown (focus management)

## Event Lifecycle

```
User clicks dropdown button
    ↓
1. DropDown event fires (before dropdown shown)
   - Sync combo.Text → control selection
   - Select matching item in control
    ↓
2. Dropdown becomes visible
    ↓
3. Popup event fires (after dropdown shown)
   - Set focus on control
   - Enable mouse interaction
    ↓
User interacts with control (click, double-click, etc.)
    ↓
4. Control-specific event fires (DoubleClick, SelectedIndexChanged)
   - Transfer control selection → combo.Text
   - Close dropdown: PopupContainer.HidePopup(PopupCloseType.Done)
```

## DropDown Event

The `DropDown` event fires **before** the dropdown is shown. Use it to select the appropriate item in the hosted control based on the current text in the combo.

### Event Signature

```csharp
public event EventHandler DropDown;
```

### When to Use

- Sync the combo's text to the control's selection before showing the dropdown
- Ensure the correct item is pre-selected or highlighted
- Perform validation or preparation before display

### TreeView Example: Select Node Matching Text

```csharp
private void comboDropDown1_DropDown(object sender, EventArgs e)
{
    // Before the dropdown is shown, select a TreeNode based on the text in the combo
    if (!string.IsNullOrEmpty(this.comboDropDown1.Text))
    {
        TreeNode matchedNode = FindNode(this.treeView1.Nodes, this.comboDropDown1.Text);
        if (matchedNode != null)
        {
            this.treeView1.SelectedNode = matchedNode;
            // Optionally ensure node is visible
            matchedNode.EnsureVisible();
        }
    }
}

// Recursive helper to find node by text
private TreeNode FindNode(TreeNodeCollection nodes, string text)
{
    // First pass: check immediate children
    foreach (TreeNode child in nodes)
    {
        if (child.Text == text)
            return child;
    }
    
    // Second pass: recursively search child nodes
    foreach (TreeNode child in nodes)
    {
        TreeNode matched = FindNode(child.Nodes, text);
        if (matched != null)
            return matched;
    }
    
    return null;
}
```

```vb
Private Sub comboDropDown1_DropDown(sender As Object, e As EventArgs) Handles comboDropDown1.DropDown
    ' Before the dropdown is shown, select a TreeNode based on the text in the combo
    If Not String.IsNullOrEmpty(Me.comboDropDown1.Text) Then
        Dim matchedNode As TreeNode = FindNode(Me.treeView1.Nodes, Me.comboDropDown1.Text)
        If matchedNode IsNot Nothing Then
            Me.treeView1.SelectedNode = matchedNode
            matchedNode.EnsureVisible()
        End If
    End If
End Sub

' Recursive helper to find node by text
Private Function FindNode(nodes As TreeNodeCollection, text As String) As TreeNode
    ' First pass: check immediate children
    For Each child As TreeNode In nodes
        If child.Text = text Then
            Return child
        End If
    Next
    
    ' Second pass: recursively search child nodes
    For Each child As TreeNode In nodes
        Dim matched As TreeNode = FindNode(child.Nodes, text)
        If matched IsNot Nothing Then
            Return matched
        End If
    Next
    
    Return Nothing
End Function
```

### ListView Example: Select Item Matching Text

```csharp
private void comboDropDown1_DropDown(object sender, EventArgs e)
{
    if (!string.IsNullOrEmpty(this.comboDropDown1.Text))
    {
        // Find matching item in ListView
        ListViewItem[] items = this.listView1.Items.Find(this.comboDropDown1.Text, false);
        if (items.Length > 0)
        {
            this.listView1.SelectedItems.Clear();
            items[0].Selected = true;
            items[0].EnsureVisible();
        }
    }
}
```

## Control-Specific Events

After the dropdown is shown, you need to handle events from the hosted control to transfer the selection back to the combo's text and close the dropdown.

### TreeView: DoubleClick Event

Use `DoubleClick` to indicate final selection in a TreeView:

```csharp
private void treeView1_DoubleClick(object sender, EventArgs e)
{
    if (this.treeView1.SelectedNode != null)
    {
        // Set the ComboDropDown's text to be the TreeNode's text
        this.comboDropDown1.Text = this.treeView1.SelectedNode.Text;
    }
    else
    {
        this.comboDropDown1.Text = string.Empty;
    }
    
    // Close the ComboDropDown
    this.comboDropDown1.PopupContainer.HidePopup(PopupCloseType.Done);
}
```

```vb
Private Sub treeView1_DoubleClick(sender As Object, e As EventArgs) Handles treeView1.DoubleClick
    If Me.treeView1.SelectedNode IsNot Nothing Then
        ' Set the ComboDropDown's text to be the TreeNode's text
        Me.comboDropDown1.Text = Me.treeView1.SelectedNode.Text
    Else
        Me.comboDropDown1.Text = String.Empty
    End If
    
    ' Close the ComboDropDown
    Me.comboDropDown1.PopupContainer.HidePopup(PopupCloseType.Done)
End Sub
```

### ListView: ItemActivate Event

Use `ItemActivate` for ListView double-click/Enter key:

```csharp
private void listView1_ItemActivate(object sender, EventArgs e)
{
    if (this.listView1.SelectedItems.Count > 0)
    {
        this.comboDropDown1.Text = this.listView1.SelectedItems[0].Text;
        this.comboDropDown1.PopupContainer.HidePopup(PopupCloseType.Done);
    }
}
```

### ListBox: Click or SelectedIndexChanged

For ListBox, single-click can close the dropdown:

```csharp
private void listBox1_Click(object sender, EventArgs e)
{
    if (this.listBox1.SelectedIndex >= 0)
    {
        this.comboDropDown1.Text = this.listBox1.SelectedItem.ToString();
        this.comboDropDown1.PopupContainer.HidePopup(PopupCloseType.Done);
    }
}
```

### Closing the Dropdown

Use `PopupContainer.HidePopup()` method with `PopupCloseType` enumeration:

```csharp
// Close with Done type (normal closure)
this.comboDropDown1.PopupContainer.HidePopup(PopupCloseType.Done);
```

**PopupCloseType Values:**
- `PopupCloseType.Done` - Normal closure, selection confirmed
- `PopupCloseType.Canceled` - User canceled, no selection made
- `PopupCloseType.Deactivated` - Window deactivated

## Popup Event

The `Popup` event fires **after** the dropdown is shown. Use it to set focus on the hosted control so it responds to mouse movement and keyboard input immediately.

### Event Signature

```csharp
public event EventHandler Popup; // On PopupContainer
```

### When to Use

- Give focus to the hosted control after dropdown appears
- Enable immediate mouse interaction
- Initialize control state after display

### Focus Management Example

```csharp
public Form1()
{
    InitializeComponent();
    
    // Wire up Popup event on the PopupContainer
    this.comboDropDown1.PopupContainer.Popup += PopupContainer_Popup;
}

private void PopupContainer_Popup(object sender, EventArgs e)
{
    // TreeView takes focus after dropdown shown
    this.treeView1.Focus();
}
```

```vb
Public Sub New()
    InitializeComponent()
    
    ' Wire up Popup event on the PopupContainer
    AddHandler Me.comboDropDown1.PopupContainer.Popup, AddressOf PopupContainer_Popup
End Sub

Private Sub PopupContainer_Popup(sender As Object, e As EventArgs)
    ' TreeView takes focus after dropdown shown
    Me.treeView1.Focus()
End Sub
```

**Why set focus?**  
By default, the dropdown control doesn't have focus when shown, so it won't respond to mouse hover highlighting or keyboard navigation until clicked. Setting focus immediately improves user experience.

## Complete TreeView Integration

Here's a complete, production-ready example integrating all events:

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class CategorySelectorForm : Form
{
    private ComboDropDown comboDropDown1;
    private TreeView treeView1;
    
    public CategorySelectorForm()
    {
        InitializeComponent();
        SetupCategorySelector();
    }
    
    private void SetupCategorySelector()
    {
        // Create and configure TreeView
        this.treeView1 = new TreeView();
        this.treeView1.HideSelection = false;
        this.treeView1.Size = new Size(250, 200);
        
        // Add category hierarchy
        TreeNode products = new TreeNode("Products");
        products.Nodes.Add("Electronics");
        products.Nodes.Add("Clothing");
        products.Nodes.Add("Books");
        products.Nodes[0].Nodes.Add("Laptops");
        products.Nodes[0].Nodes.Add("Phones");
        products.Nodes[0].Nodes.Add("Tablets");
        this.treeView1.Nodes.Add(products);
        
        // Create and configure ComboDropDown
        this.comboDropDown1 = new ComboDropDown();
        this.comboDropDown1.Location = new Point(20, 20);
        this.comboDropDown1.Size = new Size(250, 25);
        this.comboDropDown1.Text = "Select a category...";
        this.comboDropDown1.PopupControl = this.treeView1;
        
        // Wire up events
        this.comboDropDown1.DropDown += ComboDropDown1_DropDown;
        this.treeView1.DoubleClick += TreeView1_DoubleClick;
        this.comboDropDown1.PopupContainer.Popup += PopupContainer_Popup;
        
        // Add to form
        this.Controls.Add(this.comboDropDown1);
    }
    
    private void ComboDropDown1_DropDown(object sender, EventArgs e)
    {
        // Before dropdown shown: sync text to TreeView selection
        if (!string.IsNullOrEmpty(this.comboDropDown1.Text))
        {
            TreeNode matchedNode = FindNode(this.treeView1.Nodes, this.comboDropDown1.Text);
            if (matchedNode != null)
            {
                this.treeView1.SelectedNode = matchedNode;
                matchedNode.EnsureVisible();
            }
        }
    }
    
    private void TreeView1_DoubleClick(object sender, EventArgs e)
    {
        // On double-click: transfer selection to text and close
        if (this.treeView1.SelectedNode != null)
        {
            this.comboDropDown1.Text = this.treeView1.SelectedNode.Text;
        }
        
        this.comboDropDown1.PopupContainer.HidePopup(PopupCloseType.Done);
    }
    
    private void PopupContainer_Popup(object sender, EventArgs e)
    {
        // After dropdown shown: give focus to TreeView
        this.treeView1.Focus();
    }
    
    private TreeNode FindNode(TreeNodeCollection nodes, string text)
    {
        // First pass: direct children
        foreach (TreeNode child in nodes)
        {
            if (child.Text == text)
                return child;
        }
        
        // Second pass: recursive search
        foreach (TreeNode child in nodes)
        {
            TreeNode matched = FindNode(child.Nodes, text);
            if (matched != null)
                return matched;
        }
        
        return null;
    }
}
```

## GridList Integration

For multi-column data selection using a grid-like control, use grid APIs to read the active cell or row rather than list-style `SelectedItem`/`SelectedIndex` properties. Grid controls expose a model and a current cell; read the model value for the desired column.

```csharp
// Example: use the current cell to read display text from the model (assumes first data column at index 1)
GridListControl gridList = new GridListControl();
gridList.Size = new Size(300, 200);

// ... configure grid columns and data ...

comboDropDown1.PopupControl = gridList;

// Use DoubleClick (or a current-cell event) to transfer the active cell value into the combo text
gridList.DoubleClick += (s, e) => {
    var cell = gridList.CurrentCell;
    if (cell != null)
    {
        int row = cell.RowIndex;
        // Read text from the model for row and column (adjust column index as needed)
        comboDropDown1.Text = gridList.Model[row, 1].Text;
        comboDropDown1.PopupContainer.HidePopup(PopupCloseType.Done);
    }
};
```

**Note:** GridListControl does not expose `SelectedItem` or `SelectedIndex` like ListBox. Use `CurrentCell` and the grid's model (`gridList.Model[row, col].Text`) to obtain cell values. See the Syncfusion Essential Grid documentation or online sample gallery for advanced integration examples.

## Suppressing Events

Use `SuppressDropDownEvent` to prevent the DropDown event from firing:

```csharp
// Temporarily suppress DropDown event
comboDropDown1.SuppressDropDownEvent = true;

// Perform operations that would normally trigger DropDown
comboDropDown1.Text = "New Value";

// Re-enable DropDown event
comboDropDown1.SuppressDropDownEvent = false;
```

**When to use:**
- Programmatically setting text without triggering sync logic
- Bulk updates where sync isn't needed
- Avoiding recursive event calls

## Best Practices

### 1. Always Implement Both Directions

Implement both text → control (DropDown) and control → text (control-specific event) for complete interaction:

```csharp
// Text → Control
comboDropDown1.DropDown += SyncTextToControl;

// Control → Text
hostedControl.SelectionEvent += SyncControlToText;
```

### 2. Null Check in DropDown Event

Always check if text is empty before searching:

```csharp
private void ComboDropDown1_DropDown(object sender, EventArgs e)
{
    if (string.IsNullOrEmpty(this.comboDropDown1.Text))
        return; // Don't search with empty text
    
    // Proceed with search
}
```

### 3. Use Appropriate Closure Event

Choose the right event based on control type:
- **TreeView:** `DoubleClick` (hierarchical, deliberate selection)
- **ListBox:** `Click` or `SelectedIndexChanged` (flat list, quick selection)
- **ListView:** `ItemActivate` (double-click or Enter key)
- **DataGrid:** `CellDoubleClick` (grid selection)

### 4. Set Focus for Better UX

Always set focus in Popup event for immediate mouse/keyboard interaction:

```csharp
comboDropDown1.PopupContainer.Popup += (s, e) => hostedControl.Focus();
```

### 5. Handle Edge Cases

```csharp
private void TreeView1_DoubleClick(object sender, EventArgs e)
{
    // Check if a node is actually selected
    if (this.treeView1.SelectedNode == null)
    {
        // User double-clicked empty area
        return;
    }
    
    // Transfer selection
    this.comboDropDown1.Text = this.treeView1.SelectedNode.Text;
    this.comboDropDown1.PopupContainer.HidePopup(PopupCloseType.Done);
}
```

### 6. Consider Validation

Add validation before closing dropdown:

```csharp
private void listBox1_Click(object sender, EventArgs e)
{
    if (this.listBox1.SelectedIndex >= 0)
    {
        string selectedText = this.listBox1.SelectedItem.ToString();
        
        // Validate selection
        if (IsValidSelection(selectedText))
        {
            this.comboDropDown1.Text = selectedText;
            this.comboDropDown1.PopupContainer.HidePopup(PopupCloseType.Done);
        }
        else
        {
            MessageBox.Show("Invalid selection");
        }
    }
}
```
