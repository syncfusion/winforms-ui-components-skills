# ToolStripItem Types

## Table of Contents
- [Overview](#overview)
- [MenuItem](#menuitem)
- [TextBox](#textbox)
- [ComboBox](#combobox)
- [Separator](#separator)

## Overview

ContextMenuStripEx supports four different types of ToolStripItems, allowing you to create rich, functional context menus with mixed content types:

1. **MenuItem** - Standard selectable menu options
2. **TextBox** - Editable text input fields
3. **ComboBox** - Dropdown selection lists
4. **Separator** - Visual dividers between menu sections

This flexibility enables sophisticated menu designs beyond simple click actions, such as search interfaces, filter controls, and inline data entry.

## MenuItem

MenuItem represents a selectable option displayed in the context menu. It's the most common item type and supports the full range of menu features.

### Key Properties

| Property | Description |
|----------|-------------|
| `Text` | Display text for the menu item |
| `Checked` | Whether a check mark appears before the text. Only visible if ContextMenuStripEx.ShowCheckMargin is true |
| `CheckedState` | State of the item: Checked, Unchecked, or Indeterminate |
| `Enabled` | Whether the item is enabled (clickable) or disabled (grayed out) |
| `ShowShortcutKeys` | Whether to display the keyboard shortcut |
| `ShortcutKeys` | The keyboard shortcut key combination (e.g., Ctrl+C) |
| `ShortcutKeyDisplayString` | Custom text to display for the shortcut (overrides ShortcutKeys display) |
| `AutoToolTip` | When true, uses Text property as tooltip. When false, uses ToolTipText |
| `ToolTipText` | Tooltip text to display when hovering (requires AutoToolTip = false) |
| `DropDown` | The ToolStripDropDown to show when item is clicked |
| `DropDownItems` | Collection of submenu items (opens Items Collection Editor) |

### Adding MenuItem via Designer

1. Select the ContextMenuStripEx control in the component tray
2. Click **"Type Here"** in the context menu designer
3. Select **MenuItem** from the dropdown (or just start typing text)
4. Configure properties in the Properties panel:
   - Set **Text** under Appearance
   - Configure shortcuts under Behavior → ShortcutKeys
   - Set enabled state, checked state, etc.

### Adding MenuItem via Code

**C# Example:**
```csharp
using Syncfusion.Windows.Forms.Tools;
using System.Windows.Forms;

// Create context menu
ContextMenuStripEx contextMenu = new ContextMenuStripEx();

// Create menu items
ToolStripMenuItem cutItem = new ToolStripMenuItem();
cutItem.Text = "Cut";
cutItem.ShortcutKeys = Keys.Control | Keys.X;
cutItem.ShowShortcutKeys = true;
cutItem.Enabled = true;
cutItem.Click += CutItem_Click;

ToolStripMenuItem copyItem = new ToolStripMenuItem();
copyItem.Text = "Copy";
copyItem.ShortcutKeys = Keys.Control | Keys.C;
copyItem.ShowShortcutKeys = true;

ToolStripMenuItem pasteItem = new ToolStripMenuItem();
pasteItem.Text = "Paste";
pasteItem.ShortcutKeys = Keys.Control | Keys.V;
pasteItem.ShowShortcutKeys = true;

// Add items to context menu
contextMenu.Items.AddRange(new ToolStripItem[] {
    cutItem, copyItem, pasteItem
});

// Event handler
private void CutItem_Click(object sender, EventArgs e)
{
    // Perform cut operation
}
```

**VB.NET Example:**
```vb
Imports Syncfusion.Windows.Forms.Tools
Imports System.Windows.Forms

' Create context menu
Dim contextMenu As New ContextMenuStripEx()

' Create menu items
Dim cutItem As New ToolStripMenuItem()
cutItem.Text = "Cut"
cutItem.ShortcutKeys = Keys.Control Or Keys.X
cutItem.ShowShortcutKeys = True
cutItem.Enabled = True
AddHandler cutItem.Click, AddressOf CutItem_Click

Dim copyItem As New ToolStripMenuItem()
copyItem.Text = "Copy"
copyItem.ShortcutKeys = Keys.Control Or Keys.C
copyItem.ShowShortcutKeys = True

Dim pasteItem As New ToolStripMenuItem()
pasteItem.Text = "Paste"
pasteItem.ShortcutKeys = Keys.Control Or Keys.V
pasteItem.ShowShortcutKeys = True

' Add items to context menu
contextMenu.Items.AddRange(New ToolStripItem() {
    cutItem, copyItem, pasteItem
})

' Event handler
Private Sub CutItem_Click(sender As Object, e As EventArgs)
    ' Perform cut operation
End Sub
```

### MenuItem Usage Examples

**Example 1: Simple Menu with Events**
```csharp
var menuOpen = new ToolStripMenuItem("Open", null, (s, e) => OpenFile());
var menuSave = new ToolStripMenuItem("Save", null, (s, e) => SaveFile());
var menuExit = new ToolStripMenuItem("Exit", null, (s, e) => Application.Exit());

contextMenu.Items.AddRange(new ToolStripItem[] { menuOpen, menuSave, menuExit });
```

**Example 2: Menu with Shortcuts and Tooltips**
```csharp
var menuItem = new ToolStripMenuItem();
menuItem.Text = "Find";
menuItem.ShortcutKeys = Keys.Control | Keys.F;
menuItem.ShowShortcutKeys = true;
menuItem.AutoToolTip = false;
menuItem.ToolTipText = "Search for text in the document";
menuItem.Click += (s, e) => ShowFindDialog();
```

**Example 3: Menu with Custom Shortcut Display**
```csharp
var menuItem = new ToolStripMenuItem();
menuItem.Text = "Quick Save";
menuItem.ShortcutKeys = Keys.Control | Keys.S;
menuItem.ShortcutKeyDisplayString = "Ctrl+S (Quick)";  // Custom display
menuItem.ShowShortcutKeys = true;
```

## TextBox

TextBox items provide editable text input directly within the context menu. This is useful for search boxes, filter inputs, or quick data entry without opening separate dialogs.

### Key Properties

| Property | Description |
|----------|-------------|
| `Text` | The text content of the textbox |
| `BorderStyle` | Border appearance: FixedSingle or Fixed3D (default) |
| `Lines` | Opens String Collection Editor for multi-line text entry |
| `TextBoxTextAlign` | Alignment of text: Left, Center, or Right |
| `AcceptsReturn` | Whether return characters are accepted as input |
| `AcceptsTab` | Whether tab characters are accepted as input |
| `CharacterCasing` | Normal, Upper, or Lower case conversion |
| `HideSelection` | Whether selection is hidden when control loses focus |
| `MaxLength` | Maximum number of characters allowed |
| `ReadOnly` | Whether the text is read-only |

### Adding TextBox via Designer

1. Click **"Type Here"** in the context menu designer
2. From the dropdown, select **TextBox**
3. Configure properties in the Properties panel:
   - Set default **Text** under Appearance
   - Configure **MaxLength**, **ReadOnly** under Behavior
   - Adjust **BorderStyle**, **TextBoxTextAlign** under Appearance

### Adding TextBox via Code

**C# Example:**
```csharp
using Syncfusion.Windows.Forms.Tools;
using System.Windows.Forms;

ContextMenuStripEx contextMenu = new ContextMenuStripEx();

// Create TextBox item
ToolStripTextBox searchBox = new ToolStripTextBox();
searchBox.Text = "Enter search term...";
searchBox.BorderStyle = BorderStyle.FixedSingle;
searchBox.MaxLength = 100;
searchBox.Size = new System.Drawing.Size(150, 23);

// Add to context menu
contextMenu.Items.Add(searchBox);

// Access text value when needed
string searchText = searchBox.Text;
```

**VB.NET Example:**
```vb
Imports Syncfusion.Windows.Forms.Tools
Imports System.Windows.Forms

Dim contextMenu As New ContextMenuStripEx()

' Create TextBox item
Dim searchBox As New ToolStripTextBox()
searchBox.Text = "Enter search term..."
searchBox.BorderStyle = BorderStyle.FixedSingle
searchBox.MaxLength = 100
searchBox.Size = New System.Drawing.Size(150, 23)

' Add to context menu
contextMenu.Items.Add(searchBox)

' Access text value when needed
Dim searchText As String = searchBox.Text
```

### TextBox Usage Examples

**Example 1: Search Box in Context Menu**
```csharp
var searchLabel = new ToolStripMenuItem("Search:");
searchLabel.Enabled = false;  // Non-clickable label

var searchBox = new ToolStripTextBox();
searchBox.Text = "";
searchBox.MaxLength = 50;

var searchButton = new ToolStripMenuItem("Find");
searchButton.Click += (s, e) => PerformSearch(searchBox.Text);

contextMenu.Items.AddRange(new ToolStripItem[] {
    searchLabel, searchBox, searchButton
});
```

**Example 2: Multi-line Input**
```csharp
var notesBox = new ToolStripTextBox();
notesBox.Multiline = true;
notesBox.AcceptsReturn = true;
notesBox.Size = new System.Drawing.Size(200, 60);
notesBox.Text = "Enter notes here...";
```

**Example 3: Filtered Input**
```csharp
var numericInput = new ToolStripTextBox();
numericInput.MaxLength = 10;
numericInput.CharacterCasing = CharacterCasing.Upper;
numericInput.KeyPress += (s, e) => {
    // Only allow numbers
    if (!char.IsControl(e.KeyChar) && !char.IsDigit(e.KeyChar))
        e.Handled = true;
};
```

## ComboBox

ComboBox items provide dropdown selection lists within the context menu. They're ideal for filters, options, or category selection without leaving the menu context.

### Key Properties

| Property | Description |
|----------|-------------|
| `Text` | Currently selected or displayed text |
| `Items` | Opens String Collection Editor to add dropdown items |
| `SelectedIndex` | Index of the currently selected item |
| `SelectedItem` | The currently selected item object |
| `FlatStyle` | Display style: Flat, Popup, Standard, or System |
| `MaxDropDownItems` | Maximum number of items visible in dropdown |
| `MaxLength` | Maximum characters allowed in the editable portion |
| `DropDownHeight` | Height of the dropdown list |
| `DropDownWidth` | Width of the dropdown list |
| `IntegralHeight` | Whether to resize to avoid showing partial items |
| `Sorted` | Whether dropdown items should be sorted alphabetically |
| `AutoCompleteSource` | Source for autocomplete: FileSystem, AllSystemSources, AllUrl, CustomSource, etc. |
| `AutoCompleteMode` | Autocomplete behavior: Suggest, Append, or SuggestAppend |
| `AutoCompleteCustomSource` | Custom strings for autocomplete when AutoCompleteSource is CustomSource |

### Adding ComboBox via Designer

1. Click **"Type Here"** in the context menu designer
2. Select **ComboBox** from the dropdown
3. Configure properties in the Properties panel:
   - Add items via **Data → Items** (String Collection Editor)
   - Set default selection via **SelectedIndex**
   - Configure appearance with **FlatStyle**

### Adding ComboBox via Code

**C# Example:**
```csharp
using Syncfusion.Windows.Forms.Tools;
using System.Windows.Forms;

ContextMenuStripEx contextMenu = new ContextMenuStripEx();

// Create ComboBox item
ToolStripComboBox filterCombo = new ToolStripComboBox();
filterCombo.Text = "Filter by...";
filterCombo.Items.AddRange(new object[] {
    "All Items",
    "Active Items",
    "Completed Items",
    "Archived Items"
});
filterCombo.SelectedIndex = 0;  // Select first item
filterCombo.DropDownStyle = ComboBoxStyle.DropDownList;  // Read-only

// Handle selection change
filterCombo.SelectedIndexChanged += (s, e) => {
    string selected = filterCombo.SelectedItem.ToString();
    ApplyFilter(selected);
};

contextMenu.Items.Add(filterCombo);
```

**VB.NET Example:**
```vb
Imports Syncfusion.Windows.Forms.Tools
Imports System.Windows.Forms

Dim contextMenu As New ContextMenuStripEx()

' Create ComboBox item
Dim filterCombo As New ToolStripComboBox()
filterCombo.Text = "Filter by..."
filterCombo.Items.AddRange(New Object() {
    "All Items",
    "Active Items",
    "Completed Items",
    "Archived Items"
})
filterCombo.SelectedIndex = 0
filterCombo.DropDownStyle = ComboBoxStyle.DropDownList

' Handle selection change
AddHandler filterCombo.SelectedIndexChanged, Sub(s, e)
    Dim selected As String = filterCombo.SelectedItem.ToString()
    ApplyFilter(selected)
End Sub

contextMenu.Items.Add(filterCombo)
```

### ComboBox Usage Examples

**Example 1: Simple Filter Dropdown**
```csharp
var filterCombo = new ToolStripComboBox();
filterCombo.Items.AddRange(new object[] { "Today", "This Week", "This Month", "All" });
filterCombo.SelectedIndex = 0;
filterCombo.DropDownStyle = ComboBoxStyle.DropDownList;
```

**Example 2: Autocomplete ComboBox**
```csharp
var searchCombo = new ToolStripComboBox();
searchCombo.AutoCompleteMode = AutoCompleteMode.SuggestAppend;
searchCombo.AutoCompleteSource = AutoCompleteSource.ListItems;
searchCombo.Items.AddRange(new object[] {
    "Apple", "Banana", "Cherry", "Date", "Elderberry"
});
```

**Example 3: Sorted ComboBox with Custom Width**
```csharp
var sortedCombo = new ToolStripComboBox();
sortedCombo.Sorted = true;
sortedCombo.DropDownWidth = 200;
sortedCombo.Items.AddRange(new object[] { "Zebra", "Apple", "Mango", "Banana" });
// Items will appear sorted: Apple, Banana, Mango, Zebra
```

## Separator

Separator items provide visual division between menu sections. They help organize large menus into logical groups, improving usability and visual clarity.

### Key Properties

| Property | Description |
|----------|-------------|
| `BackColor` | Background color of the separator |
| `ForeColor` | Foreground color of the separator line |
| `AutoSize` | Whether the separator automatically sizes |
| `Visible` | Whether the separator is visible |

### Adding Separator via Designer

1. Click **"Type Here"** in the context menu designer
2. Select **Separator** from the dropdown
3. The separator appears as a horizontal line
4. Configure visibility and colors in Properties panel if needed

### Adding Separator via Code

**C# Example:**
```csharp
using Syncfusion.Windows.Forms.Tools;
using System.Windows.Forms;

ContextMenuStripEx contextMenu = new ContextMenuStripEx();

// Create menu items
var cutItem = new ToolStripMenuItem("Cut");
var copyItem = new ToolStripMenuItem("Copy");
var pasteItem = new ToolStripMenuItem("Paste");

// Create separator
ToolStripSeparator separator1 = new ToolStripSeparator();

// More menu items
var selectAllItem = new ToolStripMenuItem("Select All");

// Add items with separator
contextMenu.Items.AddRange(new ToolStripItem[] {
    cutItem,
    copyItem,
    pasteItem,
    separator1,        // Visual divider
    selectAllItem
});
```

**VB.NET Example:**
```vb
Imports Syncfusion.Windows.Forms.Tools
Imports System.Windows.Forms

Dim contextMenu As New ContextMenuStripEx()

' Create menu items
Dim cutItem As New ToolStripMenuItem("Cut")
Dim copyItem As New ToolStripMenuItem("Copy")
Dim pasteItem As New ToolStripMenuItem("Paste")

' Create separator
Dim separator1 As New ToolStripSeparator()

' More menu items
Dim selectAllItem As New ToolStripMenuItem("Select All")

' Add items with separator
contextMenu.Items.AddRange(New ToolStripItem() {
    cutItem,
    copyItem,
    pasteItem,
    separator1,
    selectAllItem
})
```

### Separator Usage Examples

**Example 1: Multi-Section Menu**
```csharp
var menu = new ContextMenuStripEx();

// File operations
menu.Items.Add(new ToolStripMenuItem("New"));
menu.Items.Add(new ToolStripMenuItem("Open"));
menu.Items.Add(new ToolStripMenuItem("Save"));
menu.Items.Add(new ToolStripSeparator());  // Section divider

// Edit operations
menu.Items.Add(new ToolStripMenuItem("Cut"));
menu.Items.Add(new ToolStripMenuItem("Copy"));
menu.Items.Add(new ToolStripMenuItem("Paste"));
menu.Items.Add(new ToolStripSeparator());  // Section divider

// Exit
menu.Items.Add(new ToolStripMenuItem("Exit"));
```

**Example 2: Styled Separator**
```csharp
var separator = new ToolStripSeparator();
separator.BackColor = System.Drawing.Color.LightGray;
separator.ForeColor = System.Drawing.Color.DarkGray;
```

## Mixing Item Types

One of the powerful features of ContextMenuStripEx is the ability to combine different item types in a single menu.

### Example: Complete Search and Filter Menu

```csharp
var contextMenu = new ContextMenuStripEx();

// Search section
var searchLabel = new ToolStripMenuItem("Search:");
searchLabel.Enabled = false;  // Acts as a label

var searchBox = new ToolStripTextBox();
searchBox.MaxLength = 50;
searchBox.Size = new System.Drawing.Size(150, 23);

var separator1 = new ToolStripSeparator();

// Filter section
var filterLabel = new ToolStripMenuItem("Filter:");
filterLabel.Enabled = false;

var filterCombo = new ToolStripComboBox();
filterCombo.Items.AddRange(new object[] { "All", "Active", "Completed" });
filterCombo.SelectedIndex = 0;

var separator2 = new ToolStripSeparator();

// Action buttons
var applyButton = new ToolStripMenuItem("Apply");
applyButton.Click += (s, e) => {
    string searchTerm = searchBox.Text;
    string filter = filterCombo.SelectedItem.ToString();
    ApplySearchAndFilter(searchTerm, filter);
};

var clearButton = new ToolStripMenuItem("Clear");
clearButton.Click += (s, e) => {
    searchBox.Text = "";
    filterCombo.SelectedIndex = 0;
};

// Assemble menu
contextMenu.Items.AddRange(new ToolStripItem[] {
    searchLabel,
    searchBox,
    separator1,
    filterLabel,
    filterCombo,
    separator2,
    applyButton,
    clearButton
});
```

## Best Practices

1. **Use MenuItem for actions:** Standard clickable operations should be MenuItems
2. **TextBox for quick input:** Use when you need brief text entry without opening dialogs
3. **ComboBox for selections:** Ideal for filters, categories, or predefined options
4. **Separator for organization:** Group related items with separators for clarity
5. **Limit input complexity:** Context menus should provide quick access; avoid complex input forms
6. **Provide labels:** For TextBox and ComboBox items, consider adding a disabled MenuItem as a label
7. **Handle events:** Subscribe to appropriate events (Click, TextChanged, SelectedIndexChanged)
8. **Set sensible defaults:** Pre-populate TextBox and ComboBox with reasonable default values

## Troubleshooting

**TextBox or ComboBox not accepting input:**
- Ensure the item is enabled
- Check that the parent menu allows interaction
- Verify no event handlers are blocking input

**ComboBox dropdown not showing:**
- Ensure Items collection is populated
- Check DropDownHeight and DropDownWidth are reasonable
- Verify menu is fully visible on screen

**Separator not visible:**
- Check Visible property is true
- Ensure menu has sufficient width
- Verify separator is between other items (not at start/end)
