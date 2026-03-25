# Advanced Scenarios

This guide covers advanced ComboBoxBase implementation scenarios including CheckedListBox integration, PopupControlContainer nesting, and complex dropdown patterns.

## Table of Contents
- [CheckedListBox Integration](#checkedlistbox-integration)
- [Custom PopupControlContainer](#custom-popupcontrolcontainer)
- [Nesting in PopupControlContainer](#nesting-in-popupcontrolcontainer)
- [Multi-Column Dropdowns](#multi-column-dropdowns)
- [Complex Selection Patterns](#complex-selection-patterns)

## CheckedListBox Integration

CheckedListBox enables multi-select combo boxes with checkboxes, ideal for feature selection, filters, and tag inputs.

### Basic CheckedListBox Setup

**C# Example:**
```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class CheckedComboBoxExample
{
    private ComboBoxBase comboBoxBase1;
    private CheckedListBox checkedListBox1;
    
    public void CreateCheckedComboBox()
    {
        // Create controls
        comboBoxBase1 = new ComboBoxBase();
        checkedListBox1 = new CheckedListBox();
        
        // Configure ComboBoxBase
        comboBoxBase1.Size = new System.Drawing.Size(300, 25);
        comboBoxBase1.Location = new System.Drawing.Point(20, 20);
        
        // Populate CheckedListBox
        checkedListBox1.Items.AddRange(new object[] {
            "Email Notifications",
            "SMS Notifications",
            "Push Notifications",
            "Weekly Summary",
            "Monthly Report"
        });
        
        // Connect to ComboBoxBase
        comboBoxBase1.ListControl = checkedListBox1;
        
        // Prevent dropdown from closing on click
        comboBoxBase1.DropDownCloseOnClick += ComboBoxBase1_DropDownCloseOnClick;
        
        // Update text when items are checked/unchecked
        checkedListBox1.ItemCheck += CheckedListBox1_ItemCheck;
    }
    
    private void ComboBoxBase1_DropDownCloseOnClick(object sender, MouseClickCancelEventArgs e)
    {
        // Keep dropdown open while user checks items
        e.Cancel = true;
    }
    
    private void CheckedListBox1_ItemCheck(object sender, ItemCheckEventArgs e)
    {
        // Use BeginInvoke because ItemCheck fires before check state changes
        comboBoxBase1.BeginInvoke(new Action(() =>
        {
            UpdateTextFromCheckedItems();
        }));
    }
    
    private void UpdateTextFromCheckedItems()
    {
        List<string> checkedItems = new List<string>();
        
        foreach (object item in checkedListBox1.CheckedItems)
        {
            checkedItems.Add(item.ToString());
        }
        
        if (checkedItems.Count == 0)
        {
            comboBoxBase1.TextBox.Text = "Select options...";
        }
        else if (checkedItems.Count == 1)
        {
            comboBoxBase1.TextBox.Text = checkedItems[0];
        }
        else
        {
            comboBoxBase1.TextBox.Text = $"{checkedItems.Count} items selected";
            // Or show all: string.Join(", ", checkedItems)
        }
    }
}
```

**VB.NET Example:**
```vb
Imports System
Imports System.Collections.Generic
Imports System.Linq
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Public Class CheckedComboBoxExample
    Private comboBoxBase1 As ComboBoxBase
    Private checkedListBox1 As CheckedListBox
    
    Public Sub CreateCheckedComboBox()
        ' Create controls
        comboBoxBase1 = New ComboBoxBase()
        checkedListBox1 = New CheckedListBox()
        
        ' Configure ComboBoxBase
        comboBoxBase1.Size = New System.Drawing.Size(300, 25)
        comboBoxBase1.Location = New System.Drawing.Point(20, 20)
        
        ' Populate CheckedListBox
        checkedListBox1.Items.AddRange(New Object() {
            "Email Notifications",
            "SMS Notifications",
            "Push Notifications",
            "Weekly Summary",
            "Monthly Report"
        })
        
        ' Connect to ComboBoxBase
        comboBoxBase1.ListControl = checkedListBox1
        
        ' Prevent dropdown from closing on click
        AddHandler comboBoxBase1.DropDownCloseOnClick, AddressOf ComboBoxBase1_DropDownCloseOnClick
        
        ' Update text when items are checked/unchecked
        AddHandler checkedListBox1.ItemCheck, AddressOf CheckedListBox1_ItemCheck
    End Sub
    
    Private Sub ComboBoxBase1_DropDownCloseOnClick(sender As Object, e As MouseClickCancelEventArgs)
        ' Keep dropdown open while user checks items
        e.Cancel = True
    End Sub
    
    Private Sub CheckedListBox1_ItemCheck(sender As Object, e As ItemCheckEventArgs)
        ' Use BeginInvoke because ItemCheck fires before check state changes
        comboBoxBase1.BeginInvoke(New Action(Sub()
            UpdateTextFromCheckedItems()
        End Sub))
    End Sub
    
    Private Sub UpdateTextFromCheckedItems()
        Dim checkedItems As New List(Of String)()
        
        For Each item As Object In checkedListBox1.CheckedItems
            checkedItems.Add(item.ToString())
        Next
        
        If checkedItems.Count = 0 Then
            comboBoxBase1.TextBox.Text = "Select options..."
        ElseIf checkedItems.Count = 1 Then
            comboBoxBase1.TextBox.Text = checkedItems(0)
        Else
            comboBoxBase1.TextBox.Text = $"{checkedItems.Count} items selected"
            ' Or show all: String.Join(", ", checkedItems)
        End If
    End Sub
End Class
```

### CheckedListBox with Done Button

Add a "Done" button to explicitly close dropdown:

**C#:**
```csharp
public void CreateCheckedComboBoxWithDoneButton()
{
    // Create controls
    comboBoxBase1 = new ComboBoxBase();
    checkedListBox1 = new CheckedListBox();
    Button btnDone = new Button { Text = "Done", Dock = DockStyle.Bottom, Height = 30 };
    Panel container = new Panel { Height = 200 };
    
    // Setup CheckedListBox
    checkedListBox1.Dock = DockStyle.Fill;
    checkedListBox1.Items.AddRange(new object[] {
        "Option A", "Option B", "Option C", "Option D", "Option E"
    });
    
    // Add CheckedListBox and button to container
    container.Controls.Add(checkedListBox1);
    container.Controls.Add(btnDone);
    
    // Note: Using Panel requires custom IListControl implementation
    // For simplicity, handle manually:
    comboBoxBase1.PopupContainer.Size = new System.Drawing.Size(300, 200);
    comboBoxBase1.PopupContainer.Controls.Add(container);
    
    // Prevent auto-close
    comboBoxBase1.DropDownCloseOnClick += (s, e) => e.Cancel = true;
    
    // Handle Done button
    btnDone.Click += (s, e) =>
    {
        UpdateTextFromCheckedItems();
        comboBoxBase1.PopupContainer.Hide();
    };
    
    // Update on check
    checkedListBox1.ItemCheck += (s, e) =>
    {
        comboBoxBase1.BeginInvoke(new Action(UpdateTextFromCheckedItems));
    };
}
```

### CheckedListBox with Select All/None

**C#:**
```csharp
public void CreateCheckedComboBoxWithSelectAll()
{
    comboBoxBase1 = new ComboBoxBase();
    checkedListBox1 = new CheckedListBox();
    Panel buttonPanel = new Panel { Dock = DockStyle.Bottom, Height = 30 };
    Button btnSelectAll = new Button { Text = "All", Dock = DockStyle.Left, Width = 60 };
    Button btnSelectNone = new Button { Text = "None", Dock = DockStyle.Left, Width = 60 };
    Button btnDone = new Button { Text = "Done", Dock = DockStyle.Right, Width = 60 };
    
    // Add buttons to panel
    buttonPanel.Controls.Add(btnDone);
    buttonPanel.Controls.Add(btnSelectNone);
    buttonPanel.Controls.Add(btnSelectAll);
    
    // Setup CheckedListBox
    checkedListBox1.Dock = DockStyle.Fill;
    checkedListBox1.Items.AddRange(new object[] {
        "Item 1", "Item 2", "Item 3", "Item 4", "Item 5"
    });
    
    // Create container
    Panel container = new Panel { Height = 200 };
    container.Controls.Add(checkedListBox1);
    container.Controls.Add(buttonPanel);
    
    comboBoxBase1.PopupContainer.Controls.Add(container);
    comboBoxBase1.PopupContainer.Size = new System.Drawing.Size(250, 200);
    
    // Event handlers
    comboBoxBase1.DropDownCloseOnClick += (s, e) => e.Cancel = true;
    
    btnSelectAll.Click += (s, e) =>
    {
        for (int i = 0; i < checkedListBox1.Items.Count; i++)
            checkedListBox1.SetItemChecked(i, true);
    };
    
    btnSelectNone.Click += (s, e) =>
    {
        for (int i = 0; i < checkedListBox1.Items.Count; i++)
            checkedListBox1.SetItemChecked(i, false);
    };
    
    btnDone.Click += (s, e) =>
    {
        UpdateTextFromCheckedItems();
        comboBoxBase1.PopupContainer.Hide();
    };
}
```

## Custom PopupControlContainer

When nesting ComboBoxBase inside a PopupControlContainer, create a custom derived class to manage focus properly.

### Why Custom PopupControlContainer Needed

**Problem:** When ComboBoxBase is inside a standard PopupControlContainer, the container loses focus and closes prematurely when ComboBoxBase's dropdown opens.

**Solution:** Override `OnPopup` method to set focus to the derived control, ensuring it doesn't lose focus.

### Creating Custom PopupControlContainer

**C#:**
```csharp
using Syncfusion.Windows.Forms;
using System;
using System.ComponentModel;

public class CustomPopupControlContainer : PopupControlContainer
{
    public CustomPopupControlContainer()
    {
    }
    
    public CustomPopupControlContainer(IContainer container) : this()
    {
        container.Add(this);
    }
    
    protected override void OnPopup(EventArgs args)
    {
        base.OnPopup(args);
        
        // Set focus to derived control to prevent premature closing
        this.Focus();
    }
}
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms
Imports System
Imports System.ComponentModel

Public Class CustomPopupControlContainer
    Inherits PopupControlContainer
    
    Public Sub New()
    End Sub
    
    Public Sub New(container As IContainer)
        Me.New()
        container.Add(Me)
    End Sub
    
    Protected Overrides Sub OnPopup(args As EventArgs)
        MyBase.OnPopup(args)
        
        ' Set focus to derived control to prevent premature closing
        Me.Focus()
    End Sub
End Class
```

## Nesting in PopupControlContainer

When ComboBoxBase is nested inside a PopupControlContainer, setup parent-child relationships properly.

### Setup Parent-Child Relationship

**C#:**
```csharp
private CustomPopupControlContainer popupControlContainer1;
private ComboBoxBase comboBoxBase1;
private ListBox listBox1;

public void SetupNestedComboBox()
{
    // Create custom PopupControlContainer
    popupControlContainer1 = new CustomPopupControlContainer();
    
    // Create ComboBoxBase and ListBox
    comboBoxBase1 = new ComboBoxBase();
    listBox1 = new ListBox();
    
    // Setup ComboBoxBase
    listBox1.Items.AddRange(new object[] { "Nested 1", "Nested 2", "Nested 3" });
    comboBoxBase1.ListControl = listBox1;
    
    // Add ComboBoxBase to PopupControlContainer
    popupControlContainer1.Controls.Add(comboBoxBase1);
    
    // Setup parent-child relationship in DropDown event
    comboBoxBase1.DropDown += ComboBoxBase1_DropDown;
}

private void ComboBoxBase1_DropDown(object sender, EventArgs e)
{
    // Specify parent-child relationship
    // This prevents PopupControlContainer from closing when ComboBoxBase dropdown opens
    
    comboBoxBase1.PopupContainer.PopupParent = popupControlContainer1;
    popupControlContainer1.CurrentPopupChild = comboBoxBase1.PopupContainer;
}
```

**VB.NET:**
```vb
Private popupControlContainer1 As CustomPopupControlContainer
Private comboBoxBase1 As ComboBoxBase
Private listBox1 As ListBox

Public Sub SetupNestedComboBox()
    ' Create custom PopupControlContainer
    popupControlContainer1 = New CustomPopupControlContainer()
    
    ' Create ComboBoxBase and ListBox
    comboBoxBase1 = New ComboBoxBase()
    listBox1 = New ListBox()
    
    ' Setup ComboBoxBase
    listBox1.Items.AddRange(New Object() { "Nested 1", "Nested 2", "Nested 3" })
    comboBoxBase1.ListControl = listBox1
    
    ' Add ComboBoxBase to PopupControlContainer
    popupControlContainer1.Controls.Add(comboBoxBase1)
    
    ' Setup parent-child relationship in DropDown event
    AddHandler comboBoxBase1.DropDown, AddressOf ComboBoxBase1_DropDown
End Sub

Private Sub ComboBoxBase1_DropDown(sender As Object, e As EventArgs)
    ' Specify parent-child relationship
    ' This prevents PopupControlContainer from closing when ComboBoxBase dropdown opens
    
    comboBoxBase1.PopupContainer.PopupParent = popupControlContainer1
    popupControlContainer1.CurrentPopupChild = comboBoxBase1.PopupContainer
End Sub
```

### Complete Nesting Example

```csharp
public class NestedComboBoxExample : Form
{
    private CustomPopupControlContainer mainPopup;
    private ComboBoxBase comboBoxBase1;
    private ListBox listBox1;
    private Button triggerButton;
    
    public NestedComboBoxExample()
    {
        SetupControls();
    }
    
    private void SetupControls()
    {
        // Create trigger button
        triggerButton = new Button
        {
            Text = "Show Popup",
            Location = new System.Drawing.Point(20, 20),
            Size = new System.Drawing.Size(120, 30)
        };
        triggerButton.Click += TriggerButton_Click;
        
        // Create custom popup
        mainPopup = new CustomPopupControlContainer
        {
            Size = new System.Drawing.Size(300, 150)
        };
        
        // Create ComboBoxBase
        comboBoxBase1 = new ComboBoxBase
        {
            Location = new System.Drawing.Point(10, 10),
            Size = new System.Drawing.Size(250, 25)
        };
        
        // Create ListBox
        listBox1 = new ListBox();
        listBox1.Items.AddRange(new object[] {
            "Nested Option 1",
            "Nested Option 2",
            "Nested Option 3"
        });
        
        // Connect
        comboBoxBase1.ListControl = listBox1;
        
        // Setup relationship
        comboBoxBase1.DropDown += (s, e) =>
        {
            comboBoxBase1.PopupContainer.PopupParent = mainPopup;
            mainPopup.CurrentPopupChild = comboBoxBase1.PopupContainer;
        };
        
        // Add to popup
        mainPopup.Controls.Add(comboBoxBase1);
        mainPopup.Controls.Add(listBox1);
        
        // Add to form
        this.Controls.Add(triggerButton);
        this.Controls.Add(mainPopup);
        this.Controls.Add(listBox1);
    }
    
    private void TriggerButton_Click(object sender, EventArgs e)
    {
        mainPopup.Show(triggerButton, System.Drawing.Point.Empty);
    }
}
```

## Multi-Column Dropdowns

Use GridListControl for multi-column data display in dropdowns.

### GridListControl Setup

**C#:**
```csharp
using Syncfusion.Windows.Forms.Grid;

public void CreateMultiColumnComboBox()
{
    comboBoxBase1 = new ComboBoxBase();
    GridListControl gridList = new GridListControl();
    
    // Configure grid
    gridList.Model.RowCount = 10;
    gridList.Model.ColCount = 4;
    
    // Set headers
    gridList[0, 1].Text = "ID";
    gridList[0, 2].Text = "Name";
    gridList[0, 3].Text = "Department";
    gridList[0, 4].Text = "Email";
    
    // Style headers
    for (int col = 1; col <= 4; col++)
    {
        gridList[0, col].CellType = GridCellTypeName.ColumnHeader;
    }
    
    // Add sample data
    for (int row = 1; row <= 10; row++)
    {
        gridList[row, 1].Text = (100 + row).ToString();
        gridList[row, 2].Text = $"Employee {row}";
        gridList[row, 3].Text = $"Dept {(row % 3) + 1}";
        gridList[row, 4].Text = $"emp{row}@company.com";
    }
    
    // Set display column (shown in textbox when selected)
    gridList.DisplayMember = "Text"; // Column 2 (Name)
    
    // Connect to ComboBoxBase
    comboBoxBase1.ListControl = gridList;
    comboBoxBase1.Size = new System.Drawing.Size(400, 25);
}
```

### GridListControl with Data Binding

```csharp
public class Employee
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Department { get; set; }
    public string Email { get; set; }
}

public void CreateDataBoundMultiColumnComboBox()
{
    // Create data
    List<Employee> employees = new List<Employee>
    {
        new Employee { Id = 101, Name = "John Doe", Department = "IT", Email = "john@company.com" },
        new Employee { Id = 102, Name = "Jane Smith", Department = "HR", Email = "jane@company.com" },
        new Employee { Id = 103, Name = "Bob Johnson", Department = "Sales", Email = "bob@company.com" }
    };
    
    // Create GridListControl
    GridListControl gridList = new GridListControl();
    
    // Bind data
    gridList.DataSource = employees;
    gridList.DisplayMember = "Name";
    gridList.ValueMember = "Id";
    
    // Connect to ComboBoxBase
    comboBoxBase1.ListControl = gridList;
}
```

## Complex Selection Patterns

### Dependent ComboBoxes

Create cascading dropdowns where one ComboBoxBase selection affects another:

```csharp
private ComboBoxBase cboCategory;
private ComboBoxBase cboSubCategory;
private ListBox listCategory;
private ListBox listSubCategory;

public void CreateDependentComboBoxes()
{
    // Category ComboBox
    cboCategory = new ComboBoxBase();
    listCategory = new ListBox();
    listCategory.Items.AddRange(new object[] { "Electronics", "Clothing", "Books" });
    cboCategory.ListControl = listCategory;
    
    // SubCategory ComboBox
    cboSubCategory = new ComboBoxBase();
    listSubCategory = new ListBox();
    cboSubCategory.ListControl = listSubCategory;
    
    // Handle category selection change
    listCategory.SelectedIndexChanged += (s, e) =>
    {
        UpdateSubCategories(listCategory.SelectedItem?.ToString());
    };
}

private void UpdateSubCategories(string category)
{
    listSubCategory.Items.Clear();
    
    switch (category)
    {
        case "Electronics":
            listSubCategory.Items.AddRange(new object[] { "Computers", "Phones", "Tablets" });
            break;
        case "Clothing":
            listSubCategory.Items.AddRange(new object[] { "Shirts", "Pants", "Shoes" });
            break;
        case "Books":
            listSubCategory.Items.AddRange(new object[] { "Fiction", "Non-Fiction", "Textbooks" });
            break;
    }
}
```

### Search-Enabled ComboBox

Add search capability to ComboBoxBase:

```csharp
public void CreateSearchableComboBox()
{
    comboBoxBase1 = new ComboBoxBase();
    listBox1 = new ListBox();
    
    // Full data list
    List<string> allItems = new List<string>
    {
        "Apple", "Apricot", "Banana", "Blueberry",
        "Cherry", "Date", "Elderberry", "Fig"
    };
    
    listBox1.Items.AddRange(allItems.ToArray());
    comboBoxBase1.ListControl = listBox1;
    
    // Handle text change for search
    comboBoxBase1.TextBox.TextChanged += (s, e) =>
    {
        string searchText = comboBoxBase1.TextBox.Text.ToLower();
        
        if (string.IsNullOrWhiteSpace(searchText))
        {
            // Show all items
            listBox1.Items.Clear();
            listBox1.Items.AddRange(allItems.ToArray());
        }
        else
        {
            // Filter items
            var filtered = allItems.Where(item => 
                item.ToLower().Contains(searchText)).ToArray();
            
            listBox1.Items.Clear();
            listBox1.Items.AddRange(filtered);
        }
    };
}
```

## Best Practices

**CheckedListBox Integration:**
- Always cancel `DropDownCloseOnClick` for multi-select scenarios
- Use `BeginInvoke` when updating text in `ItemCheck` handler
- Provide "Done" button for explicit close (better UX)
- Show count ("3 items selected") for many selections

**PopupControlContainer Nesting:**
- Always use custom PopupControlContainer with `OnPopup` override
- Setup `PopupParent` and `CurrentPopupChild` in `DropDown` event
- Test thoroughly - nesting can be complex

**Multi-Column Dropdowns:**
- Use GridListControl for structured data
- Set appropriate column widths
- Style headers for clarity
- Consider data binding for large datasets

**Performance:**
- Limit items in dropdowns (use search/filter for large lists)
- Use virtual mode for grids with many rows
- Avoid complex operations in frequently-fired events

## Next Steps

- **Event Handling:** Read [event-handling.md](event-handling.md) for detailed event documentation
- **ListControl Architecture:** Read [listcontrol-architecture.md](listcontrol-architecture.md) for custom controls
- **Getting Started:** Return to [getting-started.md](getting-started.md) for basic setup
