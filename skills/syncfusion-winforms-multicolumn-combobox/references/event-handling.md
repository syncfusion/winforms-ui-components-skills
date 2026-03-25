# Event Handling in MultiColumnComboBox

This guide covers selection events, event timing, and practical patterns for responding to user interactions with the MultiColumnComboBox control.

## Table of Contents
- [Overview](#overview)
- [Selection Events](#selection-events)
- [Event Timing and Order](#event-timing-and-order)
- [Accessing Selected Data](#accessing-selected-data)
- [Practical Examples](#practical-examples)
- [Event Patterns](#event-patterns)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

The MultiColumnComboBox provides three primary selection events inherited from ComboBoxAdv:

| Event | When Fired | Typical Use |
|-------|------------|-------------|
| `SelectionChangedCommitted` | When selection is committed (Enter, click, focus loss) | Update UI, save changes |
| `SelectedValueChanged` | When SelectedValue property changes | Track value changes |
| `SelectedIndexChanged` | When SelectedIndex property changes | React to any selection change |

## Selection Events

### SelectionChangedCommitted Event

Fires when the user commits a selection.

**Triggers:**
- User presses Enter key in text area
- User clicks an item in dropdown
- Control loses focus (DropDown mode only)
- Text property changed in code

**C#:**
```csharp
private void Form1_Load(object sender, EventArgs e)
{
    multiColumnComboBox1.SelectionChangedCommitted += MultiColumnComboBox1_SelectionChangedCommitted;
}

private void MultiColumnComboBox1_SelectionChangedCommitted(object sender, EventArgs e)
{
    ComboBoxBaseDataBound combo = sender as ComboBoxBaseDataBound;
    
    if (combo != null && combo.SelectedIndex != -1)
    {
        // Get selected value
        object selectedValue = combo.SelectedValue;
        string displayText = combo.Text;
        
        Console.WriteLine($"Selection committed: {displayText} (Value: {selectedValue})");
        
        // Perform action (update UI, save data, etc.)
        UpdateRelatedControls(selectedValue);
    }
}
```

**VB.NET:**
```vbnet
Private Sub Form1_Load(sender As Object, e As EventArgs)
    AddHandler multiColumnComboBox1.SelectionChangedCommitted, AddressOf MultiColumnComboBox1_SelectionChangedCommitted
End Sub

Private Sub MultiColumnComboBox1_SelectionChangedCommitted(sender As Object, e As EventArgs)
    Dim combo As ComboBoxBaseDataBound = TryCast(sender, ComboBoxBaseDataBound)
    
    If combo IsNot Nothing AndAlso combo.SelectedIndex <> -1 Then
        ' Get selected value
        Dim selectedValue As Object = combo.SelectedValue
        Dim displayText As String = combo.Text
        
        Console.WriteLine($"Selection committed: {displayText} (Value: {selectedValue})")
        
        ' Perform action (update UI, save data, etc.)
        UpdateRelatedControls(selectedValue)
    End If
End Sub
```

### SelectedValueChanged Event

Fires when the `SelectedValue` property changes.

**Triggers:**
- Arrow key navigation in dropdown
- Arrow key navigation in text area
- Programmatic value change

**C#:**
```csharp
private void Form1_Load(object sender, EventArgs e)
{
    multiColumnComboBox1.SelectedValueChanged += MultiColumnComboBox1_SelectedValueChanged;
}

private void MultiColumnComboBox1_SelectedValueChanged(object sender, EventArgs e)
{
    ComboBoxBaseDataBound combo = sender as ComboBoxBaseDataBound;
    
    if (combo != null && combo.SelectedIndex != -1)
    {
        // Access full row data
        DataRowView drv = combo.Items[combo.SelectedIndex] as DataRowView;
        
        if (drv != null)
        {
            // Access any column from the row
            string displayValue = drv[combo.DisplayMember].ToString();
            object valueColumn = drv[combo.ValueMember];
            
            Console.WriteLine($"Value changed: {displayValue}");
            
            // Update preview or related data
            DisplayPreview(drv);
        }
    }
}
```

**VB.NET:**
```vbnet
Private Sub Form1_Load(sender As Object, e As EventArgs)
    AddHandler multiColumnComboBox1.SelectedValueChanged, AddressOf MultiColumnComboBox1_SelectedValueChanged
End Sub

Private Sub MultiColumnComboBox1_SelectedValueChanged(sender As Object, e As EventArgs)
    Dim combo As ComboBoxBaseDataBound = TryCast(sender, ComboBoxBaseDataBound)
    
    If combo IsNot Nothing AndAlso combo.SelectedIndex <> -1 Then
        ' Access full row data
        Dim drv As DataRowView = TryCast(combo.Items(combo.SelectedIndex), DataRowView)
        
        If drv IsNot Nothing Then
            ' Access any column from the row
            Dim displayValue As String = drv(combo.DisplayMember).ToString()
            Dim valueColumn As Object = drv(combo.ValueMember)
            
            Console.WriteLine($"Value changed: {displayValue}")
            
            ' Update preview or related data
            DisplayPreview(drv)
        End If
    End If
End Sub
```

### SelectedIndexChanged Event

Fires when the `SelectedIndex` property changes.

**Triggers:**
- Keyboard navigation in dropdown
- Mouse hover (not selection, just highlight change)
- Programmatic index change

**C#:**
```csharp
private void Form1_Load(object sender, EventArgs e)
{
    multiColumnComboBox1.SelectedIndexChanged += MultiColumnComboBox1_SelectedIndexChanged;
}

private void MultiColumnComboBox1_SelectedIndexChanged(object sender, EventArgs e)
{
    ComboBoxBaseDataBound combo = sender as ComboBoxBaseDataBound;
    
    if (combo != null && combo.SelectedIndex != -1)
    {
        Console.WriteLine($"Index changed to: {combo.SelectedIndex}");
        
        // Access selected item
        DataRowView drv = combo.Items[combo.SelectedIndex] as DataRowView;
        if (drv != null)
        {
            // Display selected item details
            ShowItemDetails(drv);
        }
    }
}
```

## Event Timing and Order

Understanding when and in what order events fire is crucial for proper implementation.

### Event Order Table

The following table shows event occurrence for different user actions:

| Scenario | SelectionChangedCommitted | SelectedValueChanged | SelectedIndexChanged | Validating/Validated |
|----------|---------------------------|---------------------|---------------------|---------------------|
| **Text Area - Change Selection by Keys** | ✓ (1st) | ✓ (2nd) | ✓ (3rd) | ✗ |
| **Text Area - AutoComplete** | ✗ | ✗ | ✗ | ✗ |
| **Dropdown - Change Selection by Keys** | ✗ | ✓ (1st) | ✓ (2nd) | ✗ |
| **Dropdown - Mouse Hover** | ✗ | ✗ | ✗ | ✗ |
| **Dropdown Close - Enter Key** | ✓ (1st) | ✗ | ✗ | ✗ |
| **Dropdown Close - Escape Key** | ✗ | ✗ | ✗ | ✗ |
| **Dropdown Close - Click Item** | ✓ (1st) | ✓ (2nd) | ✓ (3rd) | ✗ |
| **Losing Focus** | ✓ (2nd)* | ✗ | ✗ | ✓ (1st) |
| **Text Property Changed in Code** | ✓ (1st) | ✗ | ✗ | ✗ |

**Note:** *Only in DropDown (editable) mode.

### Key Insights

**1. Most Common Pattern (Click Selection):**
- SelectionChangedCommitted → SelectedValueChanged → SelectedIndexChanged

**2. Keyboard Navigation in Dropdown:**
- SelectedValueChanged → SelectedIndexChanged (committed on Enter)

**3. Focus Loss (DropDown mode):**
- Validating → SelectionChangedCommitted

## Accessing Selected Data

### Get DataRowView from Selection

Access the complete row data, including all columns:

**C#:**
```csharp
private void GetSelectedRowData(object sender, EventArgs e)
{
    ComboBoxBaseDataBound combo = multiColumnComboBox1 as ComboBoxBaseDataBound;
    
    // Check if something is selected
    if (combo == null || combo.SelectedIndex == -1)
        return;
    
    // Get DataRowView
    DataRowView drv = combo.Items[combo.SelectedIndex] as DataRowView;
    
    if (drv != null)
    {
        // Access any column by name
        int id = Convert.ToInt32(drv["EmployeeID"]);
        string name = drv["Name"].ToString();
        string department = drv["Department"].ToString();
        string email = drv["Email"].ToString();
        
        // Access hidden columns too
        decimal salary = Convert.ToDecimal(drv["Salary"]);
        
        // Use the data
        Console.WriteLine($"Selected Employee: {name} (ID: {id})");
        Console.WriteLine($"Department: {department}, Email: {email}");
    }
}
```

**VB.NET:**
```vbnet
Private Sub GetSelectedRowData(sender As Object, e As EventArgs)
    Dim combo As ComboBoxBaseDataBound = TryCast(multiColumnComboBox1, ComboBoxBaseDataBound)
    
    ' Check if something is selected
    If combo Is Nothing OrElse combo.SelectedIndex = -1 Then Return
    
    ' Get DataRowView
    Dim drv As DataRowView = TryCast(combo.Items(combo.SelectedIndex), DataRowView)
    
    If drv IsNot Nothing Then
        ' Access any column by name
        Dim id As Integer = Convert.ToInt32(drv("EmployeeID"))
        Dim name As String = drv("Name").ToString()
        Dim department As String = drv("Department").ToString()
        Dim email As String = drv("Email").ToString()
        
        ' Access hidden columns too
        Dim salary As Decimal = Convert.ToDecimal(drv("Salary"))
        
        ' Use the data
        Console.WriteLine($"Selected Employee: {name} (ID: {id})")
        Console.WriteLine($"Department: {department}, Email: {email}")
    End If
End Sub
```

### Setting Text from Different Column

Set the text area to display a different column than DisplayMember:

**C#:**
```csharp
private void multiColumnComboBox1_SelectedValueChanged(object sender, EventArgs e)
{
    ComboBoxBaseDataBound combo = sender as ComboBoxBaseDataBound;
    
    if (combo != null && combo.SelectedIndex != -1)
    {
        DataRowView drv = combo.Items[combo.SelectedIndex] as DataRowView;
        
        if (drv != null)
        {
            // Display custom formatted text
            // Instead of just "John Smith", show "John Smith (Engineering)"
            string name = drv["Name"].ToString();
            string department = drv["Department"].ToString();
            
            combo.Text = $"{name} ({department})";
        }
    }
}
```

## Practical Examples

### Example 1: Load Related Data

Load related data based on selection:

**C#:**
```csharp
using System;
using System.Data;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class EmployeeDetailsForm : Form
{
    private MultiColumnComboBox employeeCombo;
    private TextBox nameTextBox;
    private TextBox departmentTextBox;
    private TextBox emailTextBox;
    private TextBox phoneTextBox;
    
    public EmployeeDetailsForm()
    {
        InitializeComponent();
        SetupForm();
    }
    
    private void SetupForm()
    {
        // Create combo
        employeeCombo = new MultiColumnComboBox
        {
            Location = new System.Drawing.Point(100, 20),
            Size = new System.Drawing.Size(300, 30),
            MultiColumn = true,
            ShowColumnHeader = true
        };
        
        // Create detail textboxes
        nameTextBox = CreateTextBox(100, 60);
        departmentTextBox = CreateTextBox(100, 100);
        emailTextBox = CreateTextBox(100, 140);
        phoneTextBox = CreateTextBox(100, 180);
        
        // Load employee data
        DataTable employees = LoadEmployees();
        employeeCombo.DataSource = employees;
        employeeCombo.DisplayMember = "Name";
        employeeCombo.ValueMember = "EmployeeID";
        
        // Attach event
        employeeCombo.SelectedValueChanged += EmployeeCombo_SelectedValueChanged;
        
        // Add labels
        AddLabel("Employee:", 20, 20);
        AddLabel("Name:", 20, 60);
        AddLabel("Department:", 20, 100);
        AddLabel("Email:", 20, 140);
        AddLabel("Phone:", 20, 180);
        
        // Add controls to form
        this.Controls.Add(employeeCombo);
        this.Controls.Add(nameTextBox);
        this.Controls.Add(departmentTextBox);
        this.Controls.Add(emailTextBox);
        this.Controls.Add(phoneTextBox);
    }
    
    private void EmployeeCombo_SelectedValueChanged(object sender, EventArgs e)
    {
        LoadEmployeeDetails();
    }
    
    private void LoadEmployeeDetails()
    {
        ComboBoxBaseDataBound combo = employeeCombo as ComboBoxBaseDataBound;
        
        if (combo == null || combo.SelectedIndex == -1)
        {
            ClearDetails();
            return;
        }
        
        DataRowView drv = combo.Items[combo.SelectedIndex] as DataRowView;
        
        if (drv != null)
        {
            // Populate detail fields
            nameTextBox.Text = drv["Name"].ToString();
            departmentTextBox.Text = drv["Department"].ToString();
            emailTextBox.Text = drv["Email"].ToString();
            phoneTextBox.Text = drv["Phone"].ToString();
        }
    }
    
    private void ClearDetails()
    {
        nameTextBox.Clear();
        departmentTextBox.Clear();
        emailTextBox.Clear();
        phoneTextBox.Clear();
    }
    
    private TextBox CreateTextBox(int x, int y)
    {
        return new TextBox
        {
            Location = new System.Drawing.Point(x, y),
            Size = new System.Drawing.Size(300, 25),
            ReadOnly = true
        };
    }
    
    private void AddLabel(string text, int x, int y)
    {
        Label label = new Label
        {
            Text = text,
            Location = new System.Drawing.Point(x, y + 3),
            AutoSize = true
        };
        this.Controls.Add(label);
    }
    
    private DataTable LoadEmployees()
    {
        DataTable dt = new DataTable();
        dt.Columns.Add("EmployeeID", typeof(int));
        dt.Columns.Add("Name", typeof(string));
        dt.Columns.Add("Department", typeof(string));
        dt.Columns.Add("Email", typeof(string));
        dt.Columns.Add("Phone", typeof(string));
        
        dt.Rows.Add(1001, "John Smith", "Engineering", "john.smith@company.com", "555-0101");
        dt.Rows.Add(1002, "Sarah Johnson", "Marketing", "sarah.j@company.com", "555-0102");
        dt.Rows.Add(1003, "Mike Brown", "Sales", "mike.b@company.com", "555-0103");
        
        return dt;
    }
}
```

### Example 2: Cascading Dropdowns

Implement dependent dropdowns:

**C#:**
```csharp
using System;
using System.Data;
using System.Linq;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class CascadingForm : Form
{
    private MultiColumnComboBox categoryCombo;
    private MultiColumnComboBox productCombo;
    private DataTable allProducts;
    
    public CascadingForm()
    {
        InitializeComponent();
        SetupCascadingCombos();
    }
    
    private void SetupCascadingCombos()
    {
        // Category combo
        categoryCombo = new MultiColumnComboBox
        {
            Location = new System.Drawing.Point(100, 20),
            Size = new System.Drawing.Size(200, 30)
        };
        
        // Product combo (depends on category)
        productCombo = new MultiColumnComboBox
        {
            Location = new System.Drawing.Point(100, 60),
            Size = new System.Drawing.Size(300, 30),
            MultiColumn = true,
            ShowColumnHeader = true,
            Enabled = false
        };
        
        // Load data
        LoadCategories();
        LoadAllProducts();
        
        // Attach event
        categoryCombo.SelectedValueChanged += CategoryCombo_SelectedValueChanged;
        
        this.Controls.Add(categoryCombo);
        this.Controls.Add(productCombo);
    }
    
    private void CategoryCombo_SelectedValueChanged(object sender, EventArgs e)
    {
        FilterProductsByCategory();
    }
    
    private void FilterProductsByCategory()
    {
        ComboBoxBaseDataBound combo = categoryCombo as ComboBoxBaseDataBound;
        
        if (combo == null || combo.SelectedIndex == -1)
        {
            productCombo.DataSource = null;
            productCombo.Enabled = false;
            return;
        }
        
        DataRowView drv = combo.Items[combo.SelectedIndex] as DataRowView;
        if (drv == null) return;
        
        string selectedCategory = drv["CategoryName"].ToString();
        
        // Filter products by selected category
        DataView filteredProducts = new DataView(allProducts);
        filteredProducts.RowFilter = $"Category = '{selectedCategory}'";
        
        // Bind filtered products
        productCombo.DataSource = filteredProducts;
        productCombo.DisplayMember = "ProductName";
        productCombo.ValueMember = "ProductID";
        productCombo.Enabled = true;
    }
    
    private void LoadCategories()
    {
        DataTable categories = new DataTable();
        categories.Columns.Add("CategoryID", typeof(int));
        categories.Columns.Add("CategoryName", typeof(string));
        
        categories.Rows.Add(1, "Electronics");
        categories.Rows.Add(2, "Accessories");
        categories.Rows.Add(3, "Furniture");
        
        categoryCombo.DataSource = categories;
        categoryCombo.DisplayMember = "CategoryName";
        categoryCombo.ValueMember = "CategoryID";
    }
    
    private void LoadAllProducts()
    {
        allProducts = new DataTable();
        allProducts.Columns.Add("ProductID", typeof(int));
        allProducts.Columns.Add("ProductName", typeof(string));
        allProducts.Columns.Add("Category", typeof(string));
        allProducts.Columns.Add("Price", typeof(decimal));
        
        allProducts.Rows.Add(101, "Laptop", "Electronics", 1299.99);
        allProducts.Rows.Add(102, "Monitor", "Electronics", 499.99);
        allProducts.Rows.Add(103, "Mouse", "Accessories", 29.99);
        allProducts.Rows.Add(104, "Keyboard", "Accessories", 79.99);
        allProducts.Rows.Add(105, "Desk", "Furniture", 399.99);
        allProducts.Rows.Add(106, "Chair", "Furniture", 299.99);
    }
}
```

### Example 3: Validation Before Selection

Validate before allowing selection change:

**C#:**
```csharp
using System;
using System.Data;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class ValidationForm : Form
{
    private MultiColumnComboBox accountCombo;
    private int lastValidIndex = -1;
    
    public ValidationForm()
    {
        InitializeComponent();
        SetupValidation();
    }
    
    private void SetupValidation()
    {
        accountCombo = new MultiColumnComboBox
        {
            Location = new System.Drawing.Point(20, 20),
            Size = new System.Drawing.Size(350, 30),
            MultiColumn = true,
            ShowColumnHeader = true
        };
        
        LoadAccounts();
        
        // Use SelectedIndexChanged for validation
        // (fires before commitment)
        accountCombo.SelectedIndexChanged += AccountCombo_SelectedIndexChanged;
        
        this.Controls.Add(accountCombo);
    }
    
    private void AccountCombo_SelectedIndexChanged(object sender, EventArgs e)
    {
        ComboBoxBaseDataBound combo = sender as ComboBoxBaseDataBound;
        
        if (combo == null || combo.SelectedIndex == -1)
            return;
        
        DataRowView drv = combo.Items[combo.SelectedIndex] as DataRowView;
        if (drv == null) return;
        
        // Check if account is active
        bool isActive = Convert.ToBoolean(drv["IsActive"]);
        
        if (!isActive)
        {
            // Show warning
            string accountName = drv["AccountName"].ToString();
            MessageBox.Show(
                $"Account '{accountName}' is inactive and cannot be selected.",
                "Inactive Account",
                MessageBoxButtons.OK,
                MessageBoxIcon.Warning
            );
            
            // Revert to last valid selection
            combo.SelectedIndex = lastValidIndex;
        }
        else
        {
            // Valid selection - remember it
            lastValidIndex = combo.SelectedIndex;
        }
    }
    
    private void LoadAccounts()
    {
        DataTable dt = new DataTable();
        dt.Columns.Add("AccountID", typeof(int));
        dt.Columns.Add("AccountName", typeof(string));
        dt.Columns.Add("Balance", typeof(decimal));
        dt.Columns.Add("IsActive", typeof(bool));
        
        dt.Rows.Add(1, "Checking Account", 5432.10, true);
        dt.Rows.Add(2, "Savings Account", 12500.50, true);
        dt.Rows.Add(3, "Old Account (Closed)", 0.00, false);
        dt.Rows.Add(4, "Investment Account", 25000.00, true);
        
        accountCombo.DataSource = dt;
        accountCombo.DisplayMember = "AccountName";
        accountCombo.ValueMember = "AccountID";
        
        // Hide IsActive column
        accountCombo.ListBox.Grid.Model.Cols.Hidden["IsActive"] = true;
        
        // Set initial valid selection
        accountCombo.SelectedIndex = 0;
        lastValidIndex = 0;
    }
}
```

## Event Patterns

### Pattern 1: Update Summary on Selection

**C#:**
```csharp
private Label summaryLabel;

private void ProductCombo_SelectedValueChanged(object sender, EventArgs e)
{
    UpdateProductSummary();
}

private void UpdateProductSummary()
{
    ComboBoxBaseDataBound combo = productCombo as ComboBoxBaseDataBound;
    
    if (combo == null || combo.SelectedIndex == -1)
    {
        summaryLabel.Text = "No product selected";
        return;
    }
    
    DataRowView drv = combo.Items[combo.SelectedIndex] as DataRowView;
    
    if (drv != null)
    {
        string name = drv["ProductName"].ToString();
        decimal price = Convert.ToDecimal(drv["Price"]);
        int stock = Convert.ToInt32(drv["Stock"]);
        
        summaryLabel.Text = $"{name} - ${price:N2} ({stock} in stock)";
    }
}
```

### Pattern 2: Enable/Disable Controls

**C#:**
```csharp
private Button saveButton;
private Button deleteButton;

private void RecordCombo_SelectedValueChanged(object sender, EventArgs e)
{
    bool hasSelection = recordCombo.SelectedIndex != -1;
    
    saveButton.Enabled = hasSelection;
    deleteButton.Enabled = hasSelection;
}
```

### Pattern 3: Load Image Based on Selection

**C#:**
```csharp
private PictureBox productImage;

private void ProductCombo_SelectedValueChanged(object sender, EventArgs e)
{
    LoadProductImage();
}

private void LoadProductImage()
{
    ComboBoxBaseDataBound combo = productCombo as ComboBoxBaseDataBound;
    
    if (combo == null || combo.SelectedIndex == -1)
    {
        productImage.Image = null;
        return;
    }
    
    DataRowView drv = combo.Items[combo.SelectedIndex] as DataRowView;
    
    if (drv != null)
    {
        string imagePath = drv["ImagePath"].ToString();
        
        if (System.IO.File.Exists(imagePath))
        {
            productImage.Image = Image.FromFile(imagePath);
        }
    }
}
```

## Best Practices

### 1. Always Check for Valid Selection

```csharp
if (combo == null || combo.SelectedIndex == -1)
    return; // No selection
```

### 2. Use Appropriate Event

- **SelectionChangedCommitted:** Final actions (save, update database)
- **SelectedValueChanged:** Live preview, enable/disable controls
- **SelectedIndexChanged:** Real-time updates, cascading dropdowns

### 3. Handle Null Values

```csharp
DataRowView drv = combo.Items[combo.SelectedIndex] as DataRowView;
if (drv != null)
{
    string value = drv["Column"]?.ToString() ?? "";
}
```

### 4. Detach Events When Not Needed

```csharp
// Prevent event during programmatic changes
combo.SelectedValueChanged -= Combo_SelectedValueChanged;
combo.SelectedIndex = 0; // Won't fire event
combo.SelectedValueChanged += Combo_SelectedValueChanged;
```

### 5. Use Try-Catch for Data Access

```csharp
try
{
    int id = Convert.ToInt32(drv["ID"]);
    decimal price = Convert.ToDecimal(drv["Price"]);
}
catch (Exception ex)
{
    MessageBox.Show($"Error accessing data: {ex.Message}");
}
```

## Troubleshooting

### Event Fires Multiple Times

**Issue:** Event handler executes repeatedly for single user action.

**Solutions:**
1. Check event isn't attached multiple times:
   ```csharp
   // Detach first to prevent duplicates
   combo.SelectedValueChanged -= Combo_SelectedValueChanged;
   combo.SelectedValueChanged += Combo_SelectedValueChanged;
   ```

2. Use flag to prevent re-entry:
   ```csharp
   private bool isUpdating = false;
   
   private void Combo_SelectedValueChanged(object sender, EventArgs e)
   {
       if (isUpdating) return;
       
       isUpdating = true;
       // Your code here
       isUpdating = false;
   }
   ```

### Event Doesn't Fire

**Issue:** Event handler never executes.

**Solutions:**
1. Verify event is attached:
   ```csharp
   combo.SelectedValueChanged += Combo_SelectedValueChanged;
   ```

2. Check selection is actually changing:
   ```csharp
   Console.WriteLine($"SelectedIndex: {combo.SelectedIndex}");
   ```

3. Ensure method signature matches:
   ```csharp
   // Correct signature
   private void Combo_SelectedValueChanged(object sender, EventArgs e)
   ```

### Cannot Access DataRowView

**Issue:** Cast to DataRowView returns null.

**Solutions:**
1. Verify DataSource is DataTable/DataView:
   ```csharp
   if (combo.DataSource is DataTable || combo.DataSource is DataView)
   {
       DataRowView drv = combo.Items[combo.SelectedIndex] as DataRowView;
   }
   ```

2. Check SelectedIndex is valid:
   ```csharp
   if (combo.SelectedIndex >= 0 && combo.SelectedIndex < combo.Items.Count)
   ```

### SelectedIndex is -1 When It Shouldn't Be

**Issue:** SelectedIndex shows -1 even though item was selected.

**Solutions:**
1. Check timing - event may fire during initialization
2. Verify DataSource is bound before selection
3. Use SelectionChangedCommitted instead of SelectedIndexChanged for committed selections
