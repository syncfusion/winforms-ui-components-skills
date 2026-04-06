# AutoComplete Integration

Complete guide for integrating AutoComplete functionality with EditableList to provide intelligent text suggestions during inline editing.

## Overview

The AutoComplete component can be associated with EditableList's TextBox to provide:
- **Intelligent suggestions** as users type
- **Faster data entry** with predictive text
- **Reduced errors** through validated suggestions
- **Improved user experience** with familiar autocomplete behavior

AutoComplete integration transforms the editing experience from simple text input to an intelligent, suggestion-driven interface.

## Prerequisites

### Required Assembly

In addition to `Syncfusion.Shared.Base.dll` (for EditableList), you need:

- **Syncfusion.Shared.Base.dll** - Contains AutoComplete control

This is typically already referenced if you're using EditableList.

### Required Namespaces

```csharp
using Syncfusion.Windows.Forms.Tools;
using System.Collections.Generic;
using System.Windows.Forms;
```

## Basic AutoComplete Setup

### Step 1: Create AutoComplete Instance

Add AutoComplete control to your form (via designer or code).

**Via Designer:**
1. Drag AutoComplete from toolbox to form
2. It appears in the component tray (non-visual control)

**Via Code:**
```csharp
private AutoComplete autoComplete1;

public Form1()
{
    InitializeComponent();
    
    // Create AutoComplete instance
    this.autoComplete1 = new AutoComplete();
}
```

### Step 2: Associate with EditableList TextBox

The key step is connecting AutoComplete to EditableList's TextBox in the Form Load event:

```csharp
private void Form1_Load(object sender, EventArgs e)
{
    // Set AutoComplete data source
    autoComplete1.DataSource = editableList1.ListBox.Items;
    
    // Associate AutoComplete with EditableList's TextBox
    autoComplete1.SetAutoComplete(
        editableList1.TextBox,
        AutoCompleteModes.Both
    );
}
```

### Why Form_Load?

AutoComplete must be set up after:
- EditableList is fully initialized
- ListBox.Items collection is populated
- TextBox control is ready

Form_Load ensures all controls are initialized before AutoComplete association.

## AutoComplete Modes

The `AutoCompleteModes` parameter determines how suggestions are displayed.

### Available Modes

| Mode | Behavior | Use When |
|------|----------|----------|
| **None** | No autocomplete | Testing or temporarily disabling |
| **Suggest** | Dropdown suggestion list only | User wants to see all options |
| **Append** | Inline text completion only | Quick, familiar entries |
| **Both** | Dropdown + inline completion | Best user experience (recommended) |
| **AutoSuggest** | Enhanced suggestion behavior | Advanced scenarios |

### Mode Examples

```csharp
// Suggest Mode (Dropdown Only)
autoComplete1.SetAutoComplete(editableList1.TextBox, AutoCompleteModes.Suggest);

// Append Mode (Inline Only)
autoComplete1.SetAutoComplete(editableList1.TextBox, AutoCompleteModes.Append);

// Both Mode (Recommended)
autoComplete1.SetAutoComplete(editableList1.TextBox, AutoCompleteModes.Both);
```

## Data Source Configuration

AutoComplete can pull suggestions from various sources.

### Using ListBox Items

Use the existing list items as suggestions:

```csharp
// Use current list items
autoComplete1.DataSource = editableList1.ListBox.Items;
```

This is ideal when users should only enter values already in the list.

### Using Custom String Collection

Provide a predefined set of suggestions:

```csharp
private void SetupCustomSuggestions()
{
    // Create suggestion list
    List<string> suggestions = new List<string>
    {
        "Apple", "Apricot", "Avocado",
        "Banana", "Blackberry", "Blueberry",
        "Cherry", "Cranberry",
        "Date", "Dragonfruit"
    };
    
    // Set as AutoComplete data source
    autoComplete1.DataSource = suggestions;
    
    // Associate with TextBox
    autoComplete1.SetAutoComplete(
        editableList1.TextBox,
        AutoCompleteModes.Both
    );
}
```

### Using Database Results

```csharp
using System.Data;
using System.Data.SqlClient;

private void LoadSuggestionsFromDatabase()
{
    string connectionString = "your_connection_string";
    string query = "SELECT DISTINCT ProductName FROM Products ORDER BY ProductName";
    
    try
    {
        using (SqlConnection conn = new SqlConnection(connectionString))
        {
            conn.Open();
            SqlDataAdapter adapter = new SqlDataAdapter(query, conn);
            DataTable dataTable = new DataTable();
            adapter.Fill(dataTable);
            
            // Extract strings from DataTable
            List<string> suggestions = new List<string>();
            foreach (DataRow row in dataTable.Rows)
            {
                suggestions.Add(row["ProductName"].ToString());
            }
            
            // Set AutoComplete source
            autoComplete1.DataSource = suggestions;
            autoComplete1.SetAutoComplete(
                editableList1.TextBox,
                AutoCompleteModes.Both
            );
        }
    }
    catch (Exception ex)
    {
        MessageBox.Show($"Error loading suggestions: {ex.Message}");
    }
}
```

### Dynamic DataSource Updates

Update suggestions based on context:

```csharp
private void UpdateAutoCompleteSuggestions(List<string> newSuggestions)
{
    // Update data source
    autoComplete1.DataSource = newSuggestions;
    
    // Re-associate with TextBox (refresh binding)
    autoComplete1.SetAutoComplete(
        editableList1.TextBox,
        AutoCompleteModes.Both
    );
}

// Example: Update based on category selection
private void cmbCategory_SelectedIndexChanged(object sender, EventArgs e)
{
    string category = cmbCategory.SelectedItem.ToString();
    List<string> categorySuggestions = GetSuggestionsForCategory(category);
    UpdateAutoCompleteSuggestions(categorySuggestions);
}
```

## Complete Integration Example

Here's a comprehensive example demonstrating full AutoComplete integration:

```csharp
using System;
using System.Collections.Generic;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class AutoCompleteListForm : Form
{
    private EditableList editableList1;
    private AutoComplete autoComplete1;
    private List<string> productList;
    
    public AutoCompleteListForm()
    {
        InitializeComponent();
        InitializeControls();
    }
    
    private void InitializeControls()
    {
        // Setup form
        this.Text = "EditableList with AutoComplete";
        this.Size = new Size(450, 400);
        
        // Create EditableList
        this.editableList1 = new EditableList();
        this.editableList1.Location = new Point(20, 20);
        this.editableList1.Size = new Size(350, 300);
        this.editableList1.AutoScroll = true;
        this.Controls.Add(this.editableList1);
        
        // Create AutoComplete
        this.autoComplete1 = new AutoComplete();
        
        // Initialize product list
        productList = new List<string>
        {
            "Laptop", "Desktop", "Tablet", "Smartphone",
            "Mouse", "Keyboard", "Monitor", "Printer",
            "Scanner", "Webcam", "Headset", "Speaker",
            "Router", "Switch", "Cable", "Adapter"
        };
        
        // Populate EditableList
        foreach (string product in productList)
        {
            this.editableList1.ListBox.Items.Add(product);
        }
        
        // Setup AutoComplete in Form Load
        this.Load += AutoCompleteListForm_Load;
    }
    
    private void AutoCompleteListForm_Load(object sender, EventArgs e)
    {
        // Configure AutoComplete
        ConfigureAutoComplete();
    }
    
    private void ConfigureAutoComplete()
    {
        // Set data source (use product list, not limited to current items)
        autoComplete1.DataSource = productList;
        
        // Associate with EditableList TextBox
        autoComplete1.SetAutoComplete(
            editableList1.TextBox,
            AutoCompleteModes.Both  // Dropdown + inline completion
        );
        
        // Optional: Configure AutoComplete appearance
        autoComplete1.Style = Syncfusion.Windows.Forms.Tools.AutoCompleteStyle.Default;
    }
}
```

## Advanced Scenarios

### Filtering Based on Context

```csharp
private void FilterSuggestionsByCategory()
{
    string selectedCategory = cmbCategory.SelectedItem?.ToString();
    
    List<string> filteredSuggestions;
    
    switch (selectedCategory)
    {
        case "Electronics":
            filteredSuggestions = new List<string> 
            { "Laptop", "Phone", "Tablet" };
            break;
        case "Accessories":
            filteredSuggestions = new List<string> 
            { "Mouse", "Keyboard", "Cable" };
            break;
        default:
            filteredSuggestions = new List<string>();
            break;
    }
    
    autoComplete1.DataSource = filteredSuggestions;
    autoComplete1.SetAutoComplete(editableList1.TextBox, AutoCompleteModes.Both);
}
```

### Combine List Items + Additional Suggestions

```csharp
private void SetupCombinedSuggestions()
{
    // Get current list items
    List<string> currentItems = new List<string>();
    foreach (var item in editableList1.ListBox.Items)
    {
        currentItems.Add(item.ToString());
    }
    
    // Add additional suggestions
    List<string> additionalSuggestions = new List<string>
    {
        "Suggestion 1", "Suggestion 2", "Suggestion 3"
    };
    
    // Combine both
    List<string> allSuggestions = new List<string>(currentItems);
    allSuggestions.AddRange(additionalSuggestions);
    
    // Remove duplicates
    allSuggestions = new List<string>(new HashSet<string>(allSuggestions));
    
    // Sort alphabetically
    allSuggestions.Sort();
    
    // Apply to AutoComplete
    autoComplete1.DataSource = allSuggestions;
    autoComplete1.SetAutoComplete(editableList1.TextBox, AutoCompleteModes.Both);
}
```

## AutoComplete Configuration Options

```csharp
// Visual styling
autoComplete1.Style = Syncfusion.Windows.Forms.Tools.AutoCompleteStyle.Default;
autoComplete1.Office2016ColorScheme = Syncfusion.Windows.Forms.Tools.Office2016Theme.Colorful;

// Behavior settings
autoComplete1.AutoSortList = true;  // Sort suggestions alphabetically
```

## Troubleshooting

**Issue:** AutoComplete not working  
**Solution:** Ensure `SetAutoComplete()` is called in `Form_Load` event, after EditableList is initialized.

**Issue:** Suggestions not appearing  
**Solution:** Verify `DataSource` is set and contains items. Check that `AutoCompleteModes` is not `None`.

**Issue:** Wrong suggestions displayed  
**Solution:** Check `DataSource` contents. Ensure it's updated if underlying data changes.

**Issue:** AutoComplete appears for wrong TextBox  
**Solution:** Verify you're passing `editableList1.TextBox` (not another TextBox) to `SetAutoComplete()`.

**Issue:** Dropdown position is off  
**Solution:** This can happen with dock/anchor settings. Test with fixed position first.

**Issue:** Performance slow with large DataSource  
**Solution:** Limit DataSource size or implement lazy loading. Consider filtering to most relevant suggestions.

## Best Practices

1. **Always use Form_Load:** Set up AutoComplete in Form_Load to ensure all controls are initialized
2. **Choose appropriate mode:** Use `Both` for best UX, `Suggest` for strict validation
3. **Keep DataSource reasonable:** 50-500 items is ideal; avoid thousands of suggestions
4. **Update dynamically:** Refresh DataSource when context changes
5. **Test user experience:** Verify suggestions appear quickly and match expectations
6. **Combine with validation:** Use `TextBox.Leave` event to validate final input
7. **Provide fallback:** Allow users to enter values not in suggestions if appropriate

## Next Steps

- Combine AutoComplete with data validation (TextBox.Leave event)
- Implement custom filtering logic
- Add visual styling to match your application theme
- Consider multi-field autocomplete scenarios
