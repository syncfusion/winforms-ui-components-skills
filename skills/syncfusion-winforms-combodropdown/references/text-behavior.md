# Text Behavior and Input Control

This guide covers properties that control text input behavior in the ComboDropDown's edit portion.

## Edit Portion Overview

The ComboDropDown's edit portion is the text box where users can type or see selected values. Several properties control how text input behaves:

- **CharacterCasing** - Convert input to uppercase, lowercase, or leave as-is
- **NumberOnly** - Restrict input to numeric characters only
- **ReadOnly** - Prevent user from typing, only allow selection from dropdown
- **CaseSensitiveAutocomplete** - Control case sensitivity in autocomplete
- **MatchFirstCharacterOnly** - Limit autocomplete to first character matching
- **Banner Text** - Display placeholder text when empty

## CharacterCasing Property

Controls automatic case conversion for typed characters.

### Values

| Value | Description | Use Case |
|-------|-------------|----------|
| `Normal` | No case conversion (default) | General text input |
| `Upper` | Converts all characters to uppercase | Product codes, state abbreviations, IDs |
| `Lower` | Converts all characters to lowercase | Email addresses, usernames |

### Example

```csharp
// Convert input to uppercase (e.g., state codes: "CA", "NY", "TX")
this.comboDropDown1.CharacterCasing = CharacterCasing.Upper;

// Convert input to lowercase (e.g., email domains)
this.comboDropDown1.CharacterCasing = CharacterCasing.Lower;

// No conversion (default)
this.comboDropDown1.CharacterCasing = CharacterCasing.Normal;
```

```vb
' Convert input to uppercase
Me.comboDropDown1.CharacterCasing = CharacterCasing.Upper

' Convert input to lowercase
Me.comboDropDown1.CharacterCasing = CharacterCasing.Lower

' No conversion (default)
Me.comboDropDown1.CharacterCasing = CharacterCasing.Normal
```

### Use Case: State Code Selector

```csharp
// Setup for US state abbreviations (always uppercase)
comboDropDown1.CharacterCasing = CharacterCasing.Upper;
comboDropDown1.Text = "CA"; // User types "ca", displays as "CA"

// TreeView with state codes
TreeNode statesNode = new TreeNode("States");
statesNode.Nodes.Add("CA - California");
statesNode.Nodes.Add("NY - New York");
statesNode.Nodes.Add("TX - Texas");
treeView1.Nodes.Add(statesNode);
```

## NumberOnly Property

Restricts input to numeric characters only (0-9). Non-numeric keys are ignored.

### Property

```csharp
public bool NumberOnly { get; set; }
```

**Default:** `false`

### Example

```csharp
// Allow only numeric input
this.comboDropDown1.NumberOnly = true;
```

```vb
' Allow only numeric input
Me.comboDropDown1.NumberOnly = True
```

### Use Case: Quantity Selector

```csharp
// Setup for quantity selection (numbers only)
comboDropDown1.NumberOnly = true;
comboDropDown1.Text = "1";

// ListBox with quantity options
ListBox quantityList = new ListBox();
for (int i = 1; i <= 100; i++)
{
    quantityList.Items.Add(i.ToString());
}
comboDropDown1.PopupControl = quantityList;
```

### Behavior Notes

- Only digits 0-9 are accepted
- Decimal point, negative sign, and other characters are blocked
- Paste operations are filtered to numeric characters only
- For decimal/currency input, use a MaskedEditBox or custom validation instead

## ReadOnly Property

Prevents users from typing in the edit portion. Selection can only be made from the dropdown.

### Property

```csharp
public bool ReadOnly { get; set; }
```

**Default:** `false`

### Example

```csharp
// Make edit portion read-only
this.comboDropDown1.ReadOnly = true;
```

```vb
' Make edit portion read-only
Me.comboDropDown1.ReadOnly = True
```

### When to Use

1. **Enforce selection from list** - Prevent free-form text entry, only allow predefined options
2. **Display-only combo** - Show selected value but don't allow editing
3. **Prevent invalid input** - Ensure users can only select valid options from dropdown
4. **Validation simplification** - No need to validate typed text if users can't type

### Use Case: Category Selection (No Free Text)

```csharp
// Setup for strict category selection (no custom entries)
comboDropDown1.ReadOnly = true;
comboDropDown1.Text = "Select category...";

// TreeView with predefined categories
TreeNode categories = new TreeNode("Categories");
categories.Nodes.Add("Hardware");
categories.Nodes.Add("Software");
categories.Nodes.Add("Services");
treeView1.Nodes.Add(categories);

comboDropDown1.PopupControl = treeView1;
```

### Visual Indicator

When ReadOnly is true:
- Text appears grayed out or with system read-only appearance
- Cursor changes to arrow (not I-beam) when hovering over text
- Text can be selected/copied but not modified
- Dropdown button remains fully functional

## CaseSensitiveAutocomplete Property

Controls whether autocomplete matching is case-sensitive.

### Property

```csharp
public bool CaseSensitiveAutocomplete { get; set; }
```

**Default:** `false` (case-insensitive)

### Example

```csharp
// Case-insensitive autocomplete (default)
this.comboDropDown1.CaseSensitiveAutocomplete = false;
// Typing "cal" matches "California", "CALIFORNIA", "CaLiFoRnIa"

// Case-sensitive autocomplete
this.comboDropDown1.CaseSensitiveAutocomplete = true;
// Typing "Cal" matches "California" but NOT "california"
```

```vb
' Case-insensitive autocomplete
Me.comboDropDown1.CaseSensitiveAutocomplete = False

' Case-sensitive autocomplete
Me.comboDropDown1.CaseSensitiveAutocomplete = True
```

### Use Cases

- **Case-insensitive (default):** User-friendly for general data entry
- **Case-sensitive:** Exact matching for codes, IDs, or case-significant data

## MatchFirstCharacterOnly Property

Limits autocomplete to only match the first character of items.

### Property

```csharp
public bool MatchFirstCharacterOnly { get; set; }
```

**Default:** `false` (matches anywhere in text)

### Example

```csharp
// Match anywhere in text (default)
this.comboDropDown1.MatchFirstCharacterOnly = false;
// Typing "for" matches "California" (contains "for")

// Match only first character
this.comboDropDown1.MatchFirstCharacterOnly = true;
// Typing "C" matches "California", but "a" does NOT match
```

```vb
' Match anywhere in text
Me.comboDropDown1.MatchFirstCharacterOnly = False

' Match only first character
Me.comboDropDown1.MatchFirstCharacterOnly = True
```

### Use Cases

- **MatchFirstCharacterOnly = false:** Search-style input, find items by any substring
- **MatchFirstCharacterOnly = true:** Traditional combo box behavior, alphabetical navigation

## Banner Text Support

Display placeholder text when the ComboDropDown is empty, providing hints to users.

### Using BannerTextProvider Component

ComboDropDown supports banner text through the **BannerTextProvider** component:

```csharp
using Syncfusion.Windows.Forms;

// Create BannerTextProvider component
BannerTextProvider bannerTextProvider1 = new BannerTextProvider(this.components);

// Set banner text for ComboDropDown
bannerTextProvider1.SetBannerText(this.comboDropDown1, "Select a category...");
```

```vb
Imports Syncfusion.Windows.Forms

' Create BannerTextProvider component
Dim bannerTextProvider1 As New BannerTextProvider(Me.components)

' Set banner text for ComboDropDown
bannerTextProvider1.SetBannerText(Me.comboDropDown1, "Select a category...")
```

### Complete Banner Text Example

```csharp
public class BannerTextForm : Form
{
    private ComboDropDown comboDropDown1;
    private TreeView treeView1;
    private BannerTextProvider bannerTextProvider1;
    
    public BannerTextForm()
    {
        InitializeComponent();
        SetupBannerText();
    }
    
    private void SetupBannerText()
    {
        // Create ComboDropDown
        this.comboDropDown1 = new ComboDropDown();
        this.comboDropDown1.Location = new Point(20, 20);
        this.comboDropDown1.Size = new Size(250, 25);
        
        // Create TreeView
        this.treeView1 = new TreeView();
        this.treeView1.Nodes.Add("Option 1");
        this.treeView1.Nodes.Add("Option 2");
        this.comboDropDown1.PopupControl = this.treeView1;
        
        // Create BannerTextProvider
        this.bannerTextProvider1 = new BannerTextProvider(this.components);
        
        // Set banner text (shows when Text is empty)
        this.bannerTextProvider1.SetBannerText(
            this.comboDropDown1, 
            "Please select an option..."
        );
        
        // Add to form
        this.Controls.Add(this.comboDropDown1);
    }
}
```

### Banner Text Behavior

- Banner text appears in gray italics when the control is empty
- Disappears when user types or selects from dropdown
- Reappears if text is cleared
- Not included in the actual `Text` property value
- Provides visual guidance without affecting data

## Combined Configuration Examples

### Example 1: Product Code Entry (Uppercase, Numbers)

```csharp
// Product codes: uppercase letters and numbers only
comboDropDown1.CharacterCasing = CharacterCasing.Upper;
comboDropDown1.Text = "";

BannerTextProvider banner = new BannerTextProvider(this.components);
banner.SetBannerText(comboDropDown1, "Enter product code (e.g., PROD-12345)");
```

### Example 2: Strict Category Selection (Read-Only)

```csharp
// No typing allowed, must select from tree
comboDropDown1.ReadOnly = true;

BannerTextProvider banner = new BannerTextProvider(this.components);
banner.SetBannerText(comboDropDown1, "Click to select category...");
```

### Example 3: Quantity Input (Numbers Only)

```csharp
// Numeric input only
comboDropDown1.NumberOnly = true;
comboDropDown1.Text = "1";

BannerTextProvider banner = new BannerTextProvider(this.components);
banner.SetBannerText(comboDropDown1, "Enter quantity (1-999)");
```

### Example 4: Search-Style Autocomplete

```csharp
// Case-insensitive, match anywhere
comboDropDown1.CaseSensitiveAutocomplete = false;
comboDropDown1.MatchFirstCharacterOnly = false;
comboDropDown1.CharacterCasing = CharacterCasing.Normal;

BannerTextProvider banner = new BannerTextProvider(this.components);
banner.SetBannerText(comboDropDown1, "Type to search...");
```

## Property Compatibility

| Property | Compatible With | Notes |
|----------|----------------|-------|
| `CharacterCasing` | All scenarios | Works with ReadOnly=false |
| `NumberOnly` | ReadOnly=false | No effect when ReadOnly=true |
| `ReadOnly` | All scenarios | Disables typing but allows dropdown selection |
| `CaseSensitiveAutocomplete` | ReadOnly=false | Affects typing behavior |
| `MatchFirstCharacterOnly` | ReadOnly=false | Affects typing behavior |
| `Banner Text` | Text="" | Only visible when Text is empty |

## Best Practices

1. **Use ReadOnly for strict selection** - When users must choose from predefined options
2. **Combine CharacterCasing with validation** - Enforce format standards (e.g., state codes always uppercase)
3. **NumberOnly for quantities** - But consider MaskedEditBox for decimals/currency
4. **Banner text improves UX** - Provide clear instructions or examples
5. **Test autocomplete settings** - Ensure MatchFirstCharacterOnly and CaseSensitiveAutocomplete fit your data
