# Text and Item Management

## Table of Contents
- [Items Collection](#items-collection)
- [Text Alignment](#text-alignment)
- [MaxLength Configuration](#maxlength-configuration)
- [Working with Selected Items](#working-with-selected-items)
- [Text Validation](#text-validation)

## Items Collection

The Items collection is the core of DomainUpdownExt functionality. It holds all the values users can navigate through.

### Basic Operations

**Add a single item:**
```csharp
domainUpDownExt1.Items.Add("New Item");
```

**Add multiple items:**
```csharp
domainUpDownExt1.Items.AddRange(new object[] { "Item 1", "Item 2", "Item 3" });
```

**Check if item exists:**
```csharp
bool contains = domainUpDownExt1.Items.Contains("Item 1");
```

**Get item by index:**
```csharp
object item = domainUpDownExt1.Items[0];
```

**Get total count:**
```csharp
int count = domainUpDownExt1.Items.Count;
```

**Insert at specific position:**
```csharp
domainUpDownExt1.Items.Insert(2, "Inserted Item");
```

### Clearing the Collection

Remove all items:

```csharp
domainUpDownExt1.Items.Clear();
```

### Removing Items

**Remove by value:**
```csharp
domainUpDownExt1.Items.Remove("Item to Remove");
```

**Remove by index:**
```csharp
domainUpDownExt1.Items.RemoveAt(0);
```

## Text Alignment

Control how text is displayed within the control using the TextAlign property.

### Alignment Options

```csharp
// Left alignment (default)
domainUpDownExt1.TextAlign = HorizontalAlignment.Left;

// Center alignment
domainUpDownExt1.TextAlign = HorizontalAlignment.Center;

// Right alignment
domainUpDownExt1.TextAlign = HorizontalAlignment.Right;
```

### Use Case: Right-to-Left Content

```csharp
// For RTL languages or numeric alignment
domainUpDownExt1.TextAlign = HorizontalAlignment.Right;
```

## MaxLength Configuration

Control the maximum number of characters that can be entered or displayed.

### Setting MaxLength

```csharp
// Allow up to 50 characters
domainUpDownExt1.MaxLength = 50;

// No limit (default value)
domainUpDownExt1.MaxLength = 0;
```

### Practical Example

```csharp
// For short codes
domainUpDownExt1.MaxLength = 5;
domainUpDownExt1.Items.Add("AB001");
domainUpDownExt1.Items.Add("CD002");
domainUpDownExt1.Items.Add("EF003");
```

## Working with Selected Items

### Getting Selected Item

```csharp
// Get selected index (-1 if nothing selected)
int selectedIndex = domainUpDownExt1.SelectedIndex;

// Get selected item value
object selectedValue = domainUpDownExt1.SelectedItem;

// Get displayed text
string displayedText = domainUpDownExt1.Text;
```

### Setting Selected Item

```csharp
// Select by index
domainUpDownExt1.SelectedIndex = 2;

// Select by value
domainUpDownExt1.SelectedItem = "Item 2";
```

### Handling Selection Changes

```csharp
private void Form_Load(object sender, EventArgs e)
{
    domainUpDownExt1.Items.Add("First");
    domainUpDownExt1.Items.Add("Second");
    domainUpDownExt1.Items.Add("Third");
    
    // Handle the SelectedItemChanged event
    domainUpDownExt1.SelectedIndexChanged += DomainUpDownExt1_SelectedIndexChanged;
}

private void DomainUpDownExt1_SelectedIndexChanged(object sender, EventArgs e)
{
    string selected = domainUpDownExt1.SelectedItem.ToString();
    MessageBox.Show("Selected: " + selected);
}
```

## Text Validation

### Validating User Input

```csharp
private void domainUpDownExt1_KeyDown(object sender, KeyEventArgs e)
{
    if (e.KeyCode == Keys.Enter)
    {
        // Validate before adding
        string userInput = domainUpDownExt1.Text;
        
        if (!string.IsNullOrWhiteSpace(userInput) && 
            !domainUpDownExt1.Items.Contains(userInput) &&
            userInput.Length <= domainUpDownExt1.MaxLength)
        {
            domainUpDownExt1.Items.Add(userInput);
            e.Handled = true;
        }
    }
}
```

### Common Validation Patterns

```csharp
// Check for duplicates
if (!domainUpDownExt1.Items.Contains(newValue))
{
    domainUpDownExt1.Items.Add(newValue);
}

// Validate length
if (newValue.Length <= domainUpDownExt1.MaxLength)
{
    domainUpDownExt1.Items.Add(newValue);
}

// Validate format (numeric only)
if (int.TryParse(newValue, out int result))
{
    domainUpDownExt1.Items.Add(newValue);
}
```

## Practical Scenarios

### Scenario 1: Priority Selection

```csharp
domainUpDownExt1.Items.Add("Low");
domainUpDownExt1.Items.Add("Medium");
domainUpDownExt1.Items.Add("High");
domainUpDownExt1.Items.Add("Critical");

domainUpDownExt1.SelectedIndex = 1; // Default to Medium
domainUpDownExt1.TextAlign = HorizontalAlignment.Center;
```

### Scenario 2: Numeric Range

```csharp
// Populate with values 1-100
for (int i = 1; i <= 100; i++)
{
    domainUpDownExt1.Items.Add(i.ToString());
}

domainUpDownExt1.MaxLength = 3;
domainUpDownExt1.TextAlign = HorizontalAlignment.Right;
```

### Scenario 3: Dynamic Item Management

```csharp
// Load items from configuration
var itemsFromConfig = GetItemsFromConfig();
foreach (var item in itemsFromConfig)
{
    if (!domainUpDownExt1.Items.Contains(item))
    {
        domainUpDownExt1.Items.Add(item);
    }
}
```
