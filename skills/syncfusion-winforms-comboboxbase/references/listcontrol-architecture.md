# ListControl Architecture

This guide explains the pluggable ListControl architecture of ComboBoxBase and how to create custom ListControl-derived controls.

## Table of Contents
- [Overview](#overview)
- [ListControl Property](#listcontrol-property)
- [Creating Custom ListControl Controls](#creating-custom-listcontrol-controls)
- [Required Properties and Methods](#required-properties-and-methods)
- [Collection Classes](#collection-classes)
- [QuickSelection Capability](#quickselection-capability)
- [GridListControl Integration](#gridlistcontrol-integration)

## Overview

The ComboBoxBase architecture separates the edit portion (TextBox) from the drop-down list portion. This separation enables a powerful pluggable architecture where you can use any ListControl-derived class in the dropdown.

**Key Benefits:**
- Flexibility: Use any ListControl-derived control
- Customization: Create controls tailored to specific needs
- Reusability: Custom controls work across multiple ComboBoxBase instances
- Extensibility: Leverage existing ListControl implementations

**Built-in Compatible Controls:**
- `ListBox` - Standard list selection
- `CheckedListBox` - Multi-select with checkboxes
- `GridListControl` - Multi-column grid dropdown (from Essential Grid)

## ListControl Property

The `ListControl` property is the bridge between ComboBoxBase and the dropdown list.

### Setting ListControl

**C#:**
```csharp
ListBox listBox1 = new ListBox();
comboBoxBase1.ListControl = listBox1;
```

**VB.NET:**
```vb
Dim listBox1 As New ListBox()
comboBoxBase1.ListControl = listBox1
```

### Using Different ListControl Types

**Example 1: Standard ListBox**
```csharp
ListBox standardList = new ListBox();
standardList.Items.AddRange(new object[] {
    "Option A", "Option B", "Option C"
});
comboBoxBase1.ListControl = standardList;
```

**Example 2: CheckedListBox**
```csharp
CheckedListBox checkedList = new CheckedListBox();
checkedList.Items.AddRange(new object[] {
    "Feature 1", "Feature 2", "Feature 3"
});
comboBoxBase1.ListControl = checkedList;
```

**Example 3: Custom ListControl**
```csharp
MyCustomListControl customList = new MyCustomListControl();
customList.LoadData();
comboBoxBase1.ListControl = customList;
```

### Swapping ListControls at Runtime

You can dynamically change the ListControl:

```csharp
private void SwitchToCheckedList()
{
    CheckedListBox checkedList = new CheckedListBox();
    checkedList.Items.AddRange(listBox1.Items.Cast<object>().ToArray());
    
    comboBoxBase1.ListControl = checkedList;
}

private void SwitchToStandardList()
{
    ListBox standardList = new ListBox();
    // Copy items from current ListControl
    standardList.Items.AddRange(
        ((ListBox)comboBoxBase1.ListControl).Items.Cast<object>().ToArray()
    );
    
    comboBoxBase1.ListControl = standardList;
}
```

## Creating Custom ListControl Controls

When built-in controls don't meet your needs, create custom ListControl-derived controls.

### Basic Custom ListControl

**C# Example:**
```csharp
using System;
using System.Windows.Forms;
using System.Drawing;

public class CustomListControl : ListBox
{
    public CustomListControl()
    {
        // Custom initialization
        this.DrawMode = DrawMode.OwnerDrawFixed;
        this.ItemHeight = 30;
    }
    
    // Required: Items property (inherited from ListBox)
    // Already available through base class
    
    // Optional: IndexFromPoint for QuickSelection
    public int IndexFromPoint(Point pt)
    {
        return this.IndexFromPoint(pt.X, pt.Y);
    }
    
    // Optional: TopIndex for QuickSelection
    // Already available through base ListBox.TopIndex property
    
    // Custom drawing
    protected override void OnDrawItem(DrawItemEventArgs e)
    {
        if (e.Index < 0) return;
        
        e.DrawBackground();
        
        // Custom drawing logic
        using (Brush brush = new SolidBrush(e.ForeColor))
        {
            string text = this.Items[e.Index].ToString();
            e.Graphics.DrawString(text, e.Font, brush, e.Bounds);
        }
        
        e.DrawFocusRectangle();
    }
}

// Usage
CustomListControl customList = new CustomListControl();
customList.Items.Add("Custom Item 1");
customList.Items.Add("Custom Item 2");
comboBoxBase1.ListControl = customList;
```

**VB.NET Example:**
```vb
Imports System.Windows.Forms
Imports System.Drawing

Public Class CustomListControl
    Inherits ListBox
    
    Public Sub New()
        ' Custom initialization
        Me.DrawMode = DrawMode.OwnerDrawFixed
        Me.ItemHeight = 30
    End Sub
    
    ' Required: Items property (inherited from ListBox)
    ' Already available through base class
    
    ' Optional: IndexFromPoint for QuickSelection
    Public Function IndexFromPoint(pt As Point) As Integer
        Return Me.IndexFromPoint(pt.X, pt.Y)
    End Function
    
    ' Optional: TopIndex for QuickSelection
    ' Already available through base ListBox.TopIndex property
    
    ' Custom drawing
    Protected Overrides Sub OnDrawItem(e As DrawItemEventArgs)
        If e.Index < 0 Then Return
        
        e.DrawBackground()
        
        ' Custom drawing logic
        Using brush As New SolidBrush(e.ForeColor)
            Dim text As String = Me.Items(e.Index).ToString()
            e.Graphics.DrawString(text, e.Font, brush, e.Bounds)
        End Using
        
        e.DrawFocusRectangle()
    End Sub
End Class

' Usage
Dim customList As New CustomListControl()
customList.Items.Add("Custom Item 1")
customList.Items.Add("Custom Item 2")
comboBoxBase1.ListControl = customList
```

### Advanced Custom ListControl with Icons

```csharp
public class IconListControl : ListBox
{
    private ImageList imageList;
    
    public IconListControl()
    {
        this.DrawMode = DrawMode.OwnerDrawFixed;
        this.ItemHeight = 20;
        
        // Create image list
        imageList = new ImageList();
        imageList.ImageSize = new Size(16, 16);
    }
    
    public ImageList ImageList
    {
        get { return imageList; }
        set { imageList = value; }
    }
    
    protected override void OnDrawItem(DrawItemEventArgs e)
    {
        if (e.Index < 0) return;
        
        e.DrawBackground();
        
        // Draw icon
        if (imageList != null && e.Index < imageList.Images.Count)
        {
            Image icon = imageList.Images[e.Index];
            e.Graphics.DrawImage(icon, e.Bounds.Left + 2, e.Bounds.Top + 2, 16, 16);
        }
        
        // Draw text
        using (Brush brush = new SolidBrush(e.ForeColor))
        {
            Rectangle textBounds = new Rectangle(
                e.Bounds.Left + 22, 
                e.Bounds.Top, 
                e.Bounds.Width - 22, 
                e.Bounds.Height
            );
            
            string text = this.Items[e.Index].ToString();
            e.Graphics.DrawString(text, e.Font, brush, textBounds);
        }
        
        e.DrawFocusRectangle();
    }
}

// Usage
IconListControl iconList = new IconListControl();
iconList.ImageList.Images.Add(Properties.Resources.Icon1);
iconList.ImageList.Images.Add(Properties.Resources.Icon2);
iconList.Items.Add("Option with Icon 1");
iconList.Items.Add("Option with Icon 2");

comboBoxBase1.ListControl = iconList;
```

## Required Properties and Methods

When creating custom ListControl-derived controls, implement these members to enable full ComboBoxBase functionality.

### Essential: Items Property

**Required** - Provides access to the list items.

```csharp
public ObjectCollection Items { get; }
```

This is typically inherited from `ListBox.Items` when deriving from ListBox.

### Optional: IndexFromPoint Method

**Recommended** - Enables QuickSelection capability.

```csharp
public int IndexFromPoint(Point pt)
{
    // Return the index of the item at the specified point
    // Return -1 if no item at that point
}
```

**Why needed:** Allows users to click dropdown button and select items by dragging without releasing mouse.

### Optional: TopIndex Property

**Recommended** - Enables proper scrolling during QuickSelection.

```csharp
public int TopIndex { get; set; }
```

**Why needed:** Allows ComboBoxBase to scroll the list during QuickSelection.

### Example Implementation

```csharp
public class MinimalListControl : Control
{
    private List<object> items = new List<object>();
    private int selectedIndex = -1;
    private int topIndex = 0;
    
    // Required
    public List<object> Items
    {
        get { return items; }
    }
    
    // Optional but recommended
    public int IndexFromPoint(Point pt)
    {
        int itemHeight = 20; // Your item height
        int index = (pt.Y / itemHeight) + topIndex;
        
        if (index >= 0 && index < items.Count)
            return index;
        
        return -1;
    }
    
    // Optional but recommended
    public int TopIndex
    {
        get { return topIndex; }
        set
        {
            topIndex = Math.Max(0, Math.Min(value, items.Count - 1));
            this.Invalidate();
        }
    }
    
    // Your custom rendering and interaction logic
}
```

## Collection Classes

ListBox provides three collection classes for managing items and selections.

### ListBox.ObjectCollection

Contains all items in the ListBox.

**Access:**
```csharp
ListBox.ObjectCollection items = listBox1.Items;
```

**Common Operations:**
```csharp
// Add items
listBox1.Items.Add("Item 1");
listBox1.Items.AddRange(new object[] { "Item 2", "Item 3" });
listBox1.Items.Insert(0, "First Item");

// Remove items
listBox1.Items.Remove("Item 1");
listBox1.Items.RemoveAt(0);
listBox1.Items.Clear();

// Query
int count = listBox1.Items.Count;
object item = listBox1.Items[0];
bool contains = listBox1.Items.Contains("Item 1");
int index = listBox1.Items.IndexOf("Item 1");

// Iterate
foreach (object item in listBox1.Items)
{
    Console.WriteLine(item.ToString());
}
```

### ListBox.SelectedObjectCollection

Contains the selected items (subset of Items collection).

**Access:**
```csharp
ListBox.SelectedObjectCollection selected = listBox1.SelectedItems;
```

**Common Operations:**
```csharp
// Get selected items
int selectedCount = listBox1.SelectedItems.Count;

if (listBox1.SelectedItems.Count > 0)
{
    object firstSelected = listBox1.SelectedItems[0];
}

// Iterate selected items
foreach (object item in listBox1.SelectedItems)
{
    Console.WriteLine($"Selected: {item}");
}

// Check if specific item is selected
bool isSelected = listBox1.SelectedItems.Contains("Item 1");
```

### ListBox.SelectedIndexCollection

Contains the indexes of selected items.

**Access:**
```csharp
ListBox.SelectedIndexCollection indexes = listBox1.SelectedIndices;
```

**Common Operations:**
```csharp
// Get selected indexes
int selectedCount = listBox1.SelectedIndices.Count;

if (listBox1.SelectedIndices.Count > 0)
{
    int firstIndex = listBox1.SelectedIndices[0];
    object firstItem = listBox1.Items[firstIndex];
}

// Iterate selected indexes
foreach (int index in listBox1.SelectedIndices)
{
    Console.WriteLine($"Index {index}: {listBox1.Items[index]}");
}
```

### Working with Collections

**Example: Multi-Selection Processing**
```csharp
private void ProcessSelectedItems()
{
    // Enable multi-selection
    listBox1.SelectionMode = SelectionMode.MultiExtended;
    
    // Process using SelectedItems
    List<string> selectedValues = new List<string>();
    foreach (object item in listBox1.SelectedItems)
    {
        selectedValues.Add(item.ToString());
    }
    
    // OR process using SelectedIndices
    List<int> selectedIndexes = new List<int>();
    foreach (int index in listBox1.SelectedIndices)
    {
        selectedIndexes.Add(index);
        Console.WriteLine($"Item at {index}: {listBox1.Items[index]}");
    }
    
    MessageBox.Show($"Selected {selectedValues.Count} items");
}
```

## QuickSelection Capability

QuickSelection allows users to open dropdown and select items without releasing the mouse - a smooth, quick interaction.

### Enabling QuickSelection

Implement `IndexFromPoint` and `TopIndex` in your custom ListControl:

```csharp
public class QuickSelectListControl : ListBox
{
    // Already has TopIndex from base class
    
    // Implement IndexFromPoint
    public new int IndexFromPoint(Point pt)
    {
        return base.IndexFromPoint(pt.X, pt.Y);
    }
}
```

### How QuickSelection Works

1. User clicks dropdown button
2. Dropdown opens
3. User drags mouse over items **without releasing**
4. Items highlight as mouse passes over
5. User releases mouse on desired item
6. Item is selected and dropdown closes

### Testing QuickSelection

```csharp
// Create test form
private void TestQuickSelection()
{
    QuickSelectListControl quickList = new QuickSelectListControl();
    quickList.Items.AddRange(new object[] {
        "Quick Item 1",
        "Quick Item 2",
        "Quick Item 3",
        "Quick Item 4",
        "Quick Item 5"
    });
    
    comboBoxBase1.ListControl = quickList;
    
    // Test: Click dropdown button and drag over items without releasing
}
```

## GridListControl Integration

Essential Grid provides `GridListControl` for multi-column dropdowns.

### Basic GridListControl Usage

```csharp
using Syncfusion.Windows.Forms.Grid;

// Create GridListControl
GridListControl gridList = new GridListControl();

// Configure grid
gridList.Model.RowCount = 10;
gridList.Model.ColCount = 3;

// Set headers
gridList[0, 1].Text = "ID";
gridList[0, 2].Text = "Name";
gridList[0, 3].Text = "Department";

// Add data
for (int i = 1; i <= 10; i++)
{
    gridList[i, 1].Text = i.ToString();
    gridList[i, 2].Text = $"Employee {i}";
    gridList[i, 3].Text = $"Dept {(i % 3) + 1}";
}

// Set display member (column to show in textbox)
gridList.DisplayMember = "Name"; // Column 2

// Connect to ComboBoxBase
comboBoxBase1.ListControl = gridList;
```

### Multi-Column Dropdown Benefits

- Display related data (ID, Name, Description)
- Provide context for selection
- Support complex data structures
- Professional appearance

## Best Practices

**ListControl Selection:**
- Use `ListBox` for simple single-select scenarios
- Use `CheckedListBox` for multi-select needs
- Use `GridListControl` for multi-column data
- Create custom controls only when built-in controls are insufficient

**Custom ListControl Development:**
- Always implement `Items` property
- Implement `IndexFromPoint` and `TopIndex` for better UX
- Inherit from `ListBox` when possible (easier than starting from Control)
- Handle owner drawing carefully to avoid performance issues

**Collection Management:**
- Use `BeginUpdate()` and `EndUpdate()` when adding many items
- Clear `DataSource` before manual `Items` manipulation
- Use `SelectedItems` for multi-select processing

## Next Steps

- **Event Handling:** Read [event-handling.md](event-handling.md) for selection events
- **Advanced Scenarios:** Read [advanced-scenarios.md](advanced-scenarios.md) for CheckedListBox and PopupControlContainer
- **Getting Started:** Return to [getting-started.md](getting-started.md) for basic setup
