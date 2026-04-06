# Populating and Editing the List

Complete guide for populating EditableList with data and enabling inline editing functionality for runtime item management.

## Overview

EditableList supports two primary methods for populating items:
1. **DataSource Binding** - Connect to collections, databases, or data objects
2. **Manual Population** - Add items via property editor or code

Additionally, the control provides powerful inline editing capabilities where users can modify items directly within the list interface.

## Populating via DataSource

DataSource binding is ideal for dynamic data that may change or needs to be synchronized with backend systems.

### Binding to a List Collection

```csharp
using System.Collections.Generic;
using Syncfusion.Windows.Forms.Tools;

private void PopulateFromList()
{
    // Create and bind a data source
    List<string> dataSource = new List<string> { "Item 1", "Item 2", "Item 3", "Item 4", "Item 5" };
    this.editableList1.ListBox.DataSource = dataSource;
    
    // Or bind to an array
    string[] items = new string[] { "Monday", "Tuesday", "Wednesday", "Thursday", "Friday" };
    this.editableList1.ListBox.DataSource = items;
}
```

### Binding to Custom Objects

When binding to complex objects, specify the display member:

```csharp
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
}

private void PopulateFromObjects()
{
    List<Product> products = new List<Product>
    {
        new Product { Id = 1, Name = "Laptop", Price = 999.99m },
        new Product { Id = 2, Name = "Mouse", Price = 29.99m },
        new Product { Id = 3, Name = "Keyboard", Price = 79.99m }
    };
    
    this.editableList1.ListBox.DataSource = products;
    this.editableList1.ListBox.DisplayMember = "Name";
    this.editableList1.ListBox.ValueMember = "Id";
}
```

### Binding to Database Results

```csharp
using System.Data;
using System.Data.SqlClient;

private void PopulateFromDatabase()
{
    string connectionString = "your_connection_string_here";
    string query = "SELECT CategoryName FROM Categories";
    
    using (SqlConnection conn = new SqlConnection(connectionString))
    {
        SqlDataAdapter adapter = new SqlDataAdapter(query, conn);
        DataTable dataTable = new DataTable();
        adapter.Fill(dataTable);
        
        this.editableList1.ListBox.DataSource = dataTable;
        this.editableList1.ListBox.DisplayMember = "CategoryName";
    }
}
```

### Refreshing DataSource

When the underlying data changes, refresh the display:

```csharp
// Method 1: Reset DataSource
var currentDataSource = this.editableList1.ListBox.DataSource;
this.editableList1.ListBox.DataSource = null;
this.editableList1.ListBox.DataSource = currentDataSource;

// Method 2: Use BindingList for automatic updates
using System.ComponentModel;

BindingList<string> bindingList = new BindingList<string>();
bindingList.Add("Item 1");
bindingList.Add("Item 2");
this.editableList1.ListBox.DataSource = bindingList;

// Now add/remove items - list updates automatically
bindingList.Add("Item 3"); // Automatically reflected in UI
```

## Manual Population via Property Editor

The property editor approach is best for static lists defined at design time.

### Step-by-Step Process

1. Select EditableList control in Visual Studio designer
2. Open Properties Window (F4)
3. Expand ListBox → Items property and click ellipsis (...)
4. Enter items (one per line) in String Collection Editor
5. Click OK to save

Visual Studio generates code in the designer file:

```csharp
this.editableList1.ListBox.Items.AddRange(new object[] {
    "Apple", "Banana", "Cherry", "Date", "Elderberry"
});
```

## Manual Population via Code

Add items programmatically for dynamic scenarios.

### Adding Items Programmatically

```csharp
// Add individual items
this.editableList1.ListBox.Items.Add("Item 1");
this.editableList1.ListBox.Items.Insert(0, "First Item"); // Insert at beginning

// Add multiple items at once
this.editableList1.ListBox.Items.AddRange(new object[] { "Red", "Green", "Blue", "Yellow" });

// Dynamic population from user input
private void btnAddItem_Click(object sender, EventArgs e)
{
    string newItem = txtNewItem.Text.Trim();
    if (!string.IsNullOrEmpty(newItem))
    {
        this.editableList1.ListBox.Items.Add(newItem);
        txtNewItem.Clear();
    }
}
```

## Runtime Editing Workflow

EditableList enables users to edit items directly within the list interface through inline editing.

### How Inline Editing Works

Users can edit items by clicking an item to select it, then clicking again to enter edit mode. A TextBox appears for editing. Changes commit when focus moves elsewhere (Tab, Enter, or clicking away).

### Programmatic Edit Control

```csharp
// Get currently selected item
object selectedItem = this.editableList1.ListBox.SelectedItem;
int selectedIndex = this.editableList1.ListBox.SelectedIndex;

// Modify the selected item programmatically
if (selectedIndex >= 0)
{
    this.editableList1.ListBox.Items[selectedIndex] = "Modified Item";
}
```

### Handling Edit Events

```csharp
private void SetupEditHandlers()
{
    // Handle edit completion and validation
    this.editableList1.TextBox.Leave += (s, e) => {
        string editedText = this.editableList1.TextBox.Text;
        if (string.IsNullOrWhiteSpace(editedText))
        {
            MessageBox.Show("Item cannot be empty!", "Validation Error");
        }
    };
}
```

## Complete Example: Tag Management System

```csharp
using System;
using System.Collections.Generic;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class TagManagerForm : Form
{
    private List<string> tags = new List<string> { "Important", "Review", "Urgent" };
    
    public TagManagerForm()
    {
        InitializeComponent();
        this.editableList1.ListBox.DataSource = tags;
        this.editableList1.TextBox.Leave += EditBox_Leave;
    }
    
    private void BtnAdd_Click(object sender, EventArgs e)
    {
        string newTag = txtNewTag.Text.Trim();
        if (!string.IsNullOrEmpty(newTag) && !tags.Contains(newTag))
        {
            tags.Add(newTag);
            RefreshList();
            txtNewTag.Clear();
        }
    }
    
    private void BtnRemove_Click(object sender, EventArgs e)
    {
        if (this.editableList1.ListBox.SelectedItem != null)
        {
            tags.Remove(this.editableList1.ListBox.SelectedItem.ToString());
            RefreshList();
        }
    }
    
    private void EditBox_Leave(object sender, EventArgs e)
    {
        if (string.IsNullOrWhiteSpace(this.editableList1.TextBox.Text))
            MessageBox.Show("Tag cannot be empty!", "Validation Error");
    }
    
    private void RefreshList()
    {
        this.editableList1.ListBox.DataSource = null;
        this.editableList1.ListBox.DataSource = tags;
    }
}
```

## Best Practices

1. Use `BindingList<T>` instead of `List<T>` for automatic UI updates
2. Validate during edit using `TextBox.Leave` event
3. Always check if `SelectedItem` is null before accessing it
4. Set `DisplayMember` property when binding complex objects

## Common Scenarios

```csharp
// Allow only unique items
private void AddUniqueItem(string item)
{
    if (!this.editableList1.ListBox.Items.Contains(item))
        this.editableList1.ListBox.Items.Add(item);
}

// Save/Load from file
using System.IO;

private void SaveToFile(string filePath)
{
    List<string> items = this.editableList1.ListBox.Items.Cast<object>()
        .Select(i => i.ToString()).ToList();
    File.WriteAllLines(filePath, items);
}

private void LoadFromFile(string filePath)
{
    if (File.Exists(filePath))
    {
        this.editableList1.ListBox.Items.Clear();
        this.editableList1.ListBox.Items.AddRange(File.ReadAllLines(filePath));
    }
}
```

## Troubleshooting

**Issue:** Items don't update after editing  
**Solution:** Ensure focus changes properly. Check that `TextBox.Leave` event fires.

**Issue:** DataSource binding doesn't reflect changes  
**Solution:** Use `BindingList<T>` instead of `List<T>`, or manually refresh the DataSource.

**Issue:** Cannot edit items  
**Solution:** Verify that the item is selected first (single click), then clicked again to edit.

**Issue:** Edited text disappears  
**Solution:** For DataSource-bound lists, the edit may not persist. Handle the edit event and update the underlying data source.
