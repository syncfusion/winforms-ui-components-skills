# AutoComplete Features

Comprehensive guide to implementing and configuring AutoComplete functionality in FontComboBox for fast font search and filtering.

## Overview

The AutoComplete feature enables users to quickly find fonts by typing characters. As the user types, FontComboBox suggests matching fonts, reducing the need to scroll through long lists.

**Key Benefits:**
- Fast font discovery by typing
- Reduces scroll time in long font lists
- Supports multiple completion modes
- Custom font filtering options
- Improves user experience

---

## Core AutoComplete Properties

### UseAutoComplete

Enables or disables the AutoComplete feature for FontComboBox.

**Property Type:** `bool`  
**Default Value:** `false`

**C# Example:**
```csharp
// Enable AutoComplete feature
fontComboBox.UseAutoComplete = true;

// Disable AutoComplete feature
fontComboBox.UseAutoComplete = false;
```

**VB.NET Example:**
```vb
' Enable AutoComplete feature
fontComboBox.UseAutoComplete = True

' Disable AutoComplete feature
fontComboBox.UseAutoComplete = False
```

**When Enabled:**
- FontComboBox automatically populates with all system fonts
- Font names display in their respective typefaces
- Type-ahead filtering activates
- Dropdown shows matching fonts as user types

---

## AutoCompleteMode Property

Controls how completion suggestions are displayed to the user.

**Property Type:** `AutoCompleteMode` (enum)  
**Default Value:** `AutoCompleteMode.None`

### Available Modes

| Mode | Behavior | Use Case |
|------|----------|----------|
| **None** | No auto-completion | Disable suggestions entirely |
| **Suggest** | Shows dropdown with matches | User sees options but must select |
| **Append** | Automatically completes text | Fast input, auto-fills first match |
| **SuggestAppend** | Shows dropdown + completes text | Best of both: suggestions + auto-fill |

### Mode Examples

#### Suggest Mode

Shows matching fonts in dropdown, user must manually select.

**C# Example:**
```csharp
fontComboBox.AutoCompleteMode = AutoCompleteMode.Suggest;
fontComboBox.AutoCompleteSource = AutoCompleteSource.ListItems;
```

**VB.NET Example:**
```vb
fontComboBox.AutoCompleteMode = AutoCompleteMode.Suggest
fontComboBox.AutoCompleteSource = AutoCompleteSource.ListItems
```

**User Experience:**
- User types "Ar"
- Dropdown shows: Arial, Arial Black, Arial Narrow
- User must click or arrow-key to select

---

#### Append Mode

Automatically completes with first matching font.

**C# Example:**
```csharp
fontComboBox.AutoCompleteMode = AutoCompleteMode.Append;
fontComboBox.AutoCompleteSource = AutoCompleteSource.ListItems;
```

**VB.NET Example:**
```vb
fontComboBox.AutoCompleteMode = AutoCompleteMode.Append
fontComboBox.AutoCompleteSource = AutoCompleteSource.ListItems
```

**User Experience:**
- User types "Ar"
- Text automatically becomes "Arial" (first match)
- User can continue typing to refine or accept current

---

#### SuggestAppend Mode (Recommended)

Combines dropdown suggestions with auto-completion.

**C# Example:**
```csharp
fontComboBox.AutoCompleteMode = AutoCompleteMode.SuggestAppend;
fontComboBox.AutoCompleteSource = AutoCompleteSource.ListItems;
```

**VB.NET Example:**
```vb
fontComboBox.AutoCompleteMode = AutoCompleteMode.SuggestAppend
fontComboBox.AutoCompleteSource = AutoCompleteSource.ListItems
```

**User Experience:**
- User types "Ar"
- Text becomes "Arial" (auto-completed)
- Dropdown shows: Arial, Arial Black, Arial Narrow
- User can select from dropdown or continue typing

**Why Recommended:**
- Fastest input method
- Visual confirmation via dropdown
- Flexible: supports both typing and selection

---

## AutoCompleteSource Property

Specifies where completion strings come from.

**Property Type:** `AutoCompleteSource` (enum)  
**Default Value:** `AutoCompleteSource.None`

### Available Sources

| Source | Description | Use Case |
|--------|-------------|----------|
| **ListItems** | Uses items in FontComboBox.Items collection | Standard font list (system fonts) |
| **CustomSource** | Uses AutoCompleteCustomSource collection | Filtered/curated font list |
| **None** | No source, disables AutoComplete | Disable temporarily |

### ListItems Source

Uses all fonts currently in the Items collection.

**C# Example:**
```csharp
fontComboBox.UseAutoComplete = true;
fontComboBox.AutoCompleteMode = AutoCompleteMode.SuggestAppend;
fontComboBox.AutoCompleteSource = AutoCompleteSource.ListItems;
```

**VB.NET Example:**
```vb
fontComboBox.UseAutoComplete = True
fontComboBox.AutoCompleteMode = AutoCompleteMode.SuggestAppend
fontComboBox.AutoCompleteSource = AutoCompleteSource.ListItems
```

**Behavior:**
- All system fonts available for completion
- Matches against loaded font names
- Ideal for full font catalog

---

### CustomSource

Provides a curated list of fonts for completion.

**C# Example:**
```csharp
// Create custom font list
fontComboBox.AutoCompleteCustomSource.AddRange(new string[]
{
    "Arial",
    "Calibri",
    "Cambria",
    "Candara",
    "Segoe UI",
    "Times New Roman",
    "Verdana"
});

// Configure AutoComplete with custom source
fontComboBox.AutoCompleteMode = AutoCompleteMode.SuggestAppend;
fontComboBox.AutoCompleteSource = AutoCompleteSource.CustomSource;
```

**VB.NET Example:**
```vb
' Create custom font list
fontComboBox.AutoCompleteCustomSource.AddRange(New String() _
{
    "Arial",
    "Calibri",
    "Cambria",
    "Candara",
    "Segoe UI",
    "Times New Roman",
    "Verdana"
})

' Configure AutoComplete with custom source
fontComboBox.AutoCompleteMode = AutoCompleteMode.SuggestAppend
fontComboBox.AutoCompleteSource = AutoCompleteSource.CustomSource
```

**Use Cases:**
- Limit fonts to commonly used options
- Branding (specific corporate fonts)
- Performance (reduce large font lists)
- User preferences (favorite fonts)

---

## AutoCompleteCustomSource Property

Collection of strings used when AutoCompleteSource is set to CustomSource.

**Property Type:** `AutoCompleteStringCollection`  
**Default Value:** Empty collection

### Adding Custom Fonts

**C# Example:**
```csharp
// Method 1: AddRange for multiple fonts
fontComboBox.AutoCompleteCustomSource.AddRange(new string[]
{
    "Arial", "Calibri", "Georgia", "Tahoma", "Verdana"
});

// Method 2: Add individual fonts
fontComboBox.AutoCompleteCustomSource.Add("Courier New");
fontComboBox.AutoCompleteCustomSource.Add("Comic Sans MS");

// Method 3: From array
string[] commonFonts = { "Arial", "Calibri", "Times New Roman" };
fontComboBox.AutoCompleteCustomSource.AddRange(commonFonts);
```

**VB.NET Example:**
```vb
' Method 1: AddRange for multiple fonts
fontComboBox.AutoCompleteCustomSource.AddRange(New String() _
{
    "Arial", "Calibri", "Georgia", "Tahoma", "Verdana"
})

' Method 2: Add individual fonts
fontComboBox.AutoCompleteCustomSource.Add("Courier New")
fontComboBox.AutoCompleteCustomSource.Add("Comic Sans MS")

' Method 3: From array
Dim commonFonts() As String = {"Arial", "Calibri", "Times New Roman"}
fontComboBox.AutoCompleteCustomSource.AddRange(commonFonts)
```

### Managing Custom Source

```csharp
// Clear all custom fonts
fontComboBox.AutoCompleteCustomSource.Clear();

// Remove specific font
fontComboBox.AutoCompleteCustomSource.Remove("Comic Sans MS");

// Check if font exists
bool hasArial = fontComboBox.AutoCompleteCustomSource.Contains("Arial");

// Get count of custom fonts
int count = fontComboBox.AutoCompleteCustomSource.Count;
```

---

## Complete Configuration Examples

### Example 1: Standard System Font Picker

Full system font list with suggest and append behavior.

```csharp
FontComboBox fontComboBox = new FontComboBox
{
    Location = new Point(20, 20),
    Size = new Size(250, 25),
    UseAutoComplete = true,
    AutoCompleteMode = AutoCompleteMode.SuggestAppend,
    AutoCompleteSource = AutoCompleteSource.ListItems,
    DropDownStyle = ComboBoxStyle.DropDown
};
```

---

### Example 2: Curated Font List

Limited to specific commonly-used fonts.

```csharp
FontComboBox fontComboBox = new FontComboBox
{
    Location = new Point(20, 20),
    Size = new Size(250, 25)
};

// Add curated font list
fontComboBox.AutoCompleteCustomSource.AddRange(new string[]
{
    "Arial",
    "Calibri",
    "Cambria",
    "Consolas",
    "Courier New",
    "Georgia",
    "Segoe UI",
    "Tahoma",
    "Times New Roman",
    "Trebuchet MS",
    "Verdana"
});

// Configure AutoComplete
fontComboBox.AutoCompleteMode = AutoCompleteMode.SuggestAppend;
fontComboBox.AutoCompleteSource = AutoCompleteSource.CustomSource;

// Prevent custom text entry
fontComboBox.DropDownStyle = ComboBoxStyle.DropDownList;
```

---

### Example 3: Dynamic Font Filtering

Load fonts based on user role or preferences.

```csharp
private void LoadFontsBasedOnUserRole(string userRole)
{
    fontComboBox.AutoCompleteCustomSource.Clear();
    
    if (userRole == "Designer")
    {
        // Designer fonts
        fontComboBox.AutoCompleteCustomSource.AddRange(new string[]
        {
            "Arial", "Helvetica", "Futura", "Garamond", "Gill Sans"
        });
    }
    else if (userRole == "Developer")
    {
        // Monospace fonts for code
        fontComboBox.AutoCompleteCustomSource.AddRange(new string[]
        {
            "Consolas", "Courier New", "Fira Code", "JetBrains Mono"
        });
    }
    else
    {
        // Default fonts
        fontComboBox.AutoCompleteCustomSource.AddRange(new string[]
        {
            "Arial", "Calibri", "Times New Roman", "Verdana"
        });
    }
    
    fontComboBox.AutoCompleteMode = AutoCompleteMode.SuggestAppend;
    fontComboBox.AutoCompleteSource = AutoCompleteSource.CustomSource;
}
```

---

## Best Practices

### 1. Always Use SuggestAppend for Best UX

```csharp
// Recommended configuration
fontComboBox.AutoCompleteMode = AutoCompleteMode.SuggestAppend;
```

**Why:** Provides both visual feedback (dropdown) and fast input (auto-completion).

### 2. Enable UseAutoComplete for System Fonts

```csharp
// Load system fonts automatically
fontComboBox.UseAutoComplete = true;
```

**Why:** Automatically populates with all available fonts without manual enumeration.

### 3. Use CustomSource for Curated Lists

```csharp
// Limit to commonly-used fonts
fontComboBox.AutoCompleteSource = AutoCompleteSource.CustomSource;
```

**Why:** Improves performance and user focus by reducing choices.

### 4. Combine with DropDownList Style

```csharp
// Prevent invalid font names
fontComboBox.DropDownStyle = ComboBoxStyle.DropDownList;
```

**Why:** Ensures users can only select valid fonts from the list.

---

## Troubleshooting

### AutoComplete Not Working

**Check:**
1. `UseAutoComplete = true` is set
2. `AutoCompleteMode` is not `None`
3. `AutoCompleteSource` is properly configured
4. `DropDownStyle` is set to `DropDown` (not `DropDownList`)

### Custom Fonts Not Appearing

**Solution:**
```csharp
// Ensure CustomSource is set AFTER adding items
fontComboBox.AutoCompleteCustomSource.AddRange(fonts);
fontComboBox.AutoCompleteSource = AutoCompleteSource.CustomSource; // Set after AddRange
```

### Slow Performance with Large Font Lists

**Solution:**
- Use CustomSource with filtered list
- Avoid ListItems source if 500+ fonts
- Consider lazy loading

---

## Related Topics

- **DropDown Configuration**: Customize dropdown behavior → [dropdown-configuration.md](dropdown-configuration.md)
- **Selection and Events**: Handle font selection → [selection-and-events.md](selection-and-events.md)
