# Menu Item Types

## Table of Contents
- [Overview](#overview)
- [BarItem - Basic Items](#baritem---basic-items)
- [ParentBarItem - Container Items](#parentbaritem---container-items)
- [DropDownBarItem - Custom Popups](#dropdownbaritem---custom-popups)
- [ComboBoxBarItem - Selection Lists](#comboboxbaritem---selection-lists)
- [StaticBarItem - Label Items](#staticbaritem---label-items)
- [TextBoxBarItem - Text Input](#textboxbaritem---text-input)
- [ToolBarListBarItem - Customization](#toolbarlistbaritem---customization)
- [Common Properties](#common-properties)

---

## Overview

MainFrameBarManager provides a rich set of built-in BarItem types to create various menu and toolbar elements. Each type serves a specific purpose in the user interface, from simple clickable items to complex interactive controls.

## BarItem - Basic Items

The fundamental item type representing a clickable menu or toolbar button.

### Basic Usage

```csharp
BarItem newItem = new BarItem();
newItem.Text = "&New";
newItem.Category = "File";

// Add event handler
newItem.ItemClick += (sender, args) =>
{
    CreateNewDocument();
};

mainFrameBarManager1.Items.Add(newItem);
fileBar.Items.Add(newItem);
```

### Properties

| Property | Type | Description |
|----------|------|-------------|
| **Text** | string | Display text; & denotes mnemonic |
| **Shortcut** | Shortcut | Keyboard shortcut (Ctrl+N, etc.) |
| **Checked** | bool | Whether item is checked (shows checkmark) |
| **Enabled** | bool | Whether item is clickable |
| **Category** | string | For organizing in customization UI |

### Events

```csharp
newItem.ItemClick += (sender, args) => { };  // Fired when clicked
newItem.ItemSelected += (sender, args) => { }; // Fired when highlighted
```

### Example: Disabled Item

```csharp
BarItem pasteItem = new BarItem() 
{ 
    Text = "&Paste", 
    Enabled = false  // Grayed out
};
```

## ParentBarItem - Container Items

Container for sub-menu items, enabling hierarchical menu structures.

### Basic Usage

```csharp
// Create parent
ParentBarItem editMenu = new ParentBarItem();
editMenu.Text = "&Edit";

// Create children
BarItem cutItem = new BarItem() { Text = "Cu&t" };
BarItem copyItem = new BarItem() { Text = "&Copy" };
BarItem pasteItem = new BarItem() { Text = "&Paste" };

// Add children to parent
editMenu.Items.AddRange(new BarItem[] { cutItem, copyItem, pasteItem });

// Add all to manager
mainFrameBarManager1.Items.AddRange(new BarItem[] 
{ 
    editMenu, cutItem, copyItem, pasteItem 
});

// Add parent to bar
editBar.Items.Add(editMenu);
```

### Nested Sub-Menus

```csharp
// Create main menu
ParentBarItem viewMenu = new ParentBarItem() { Text = "&View" };

// Create submenu
ParentBarItem toolbarsMenu = new ParentBarItem() { Text = "&Toolbars" };

// Create sub-items
BarItem standardToolbar = new BarItem() { Text = "Standard Toolbar", Checked = true };
BarItem formatToolbar = new BarItem() { Text = "Format Toolbar" };

// Build hierarchy
toolbarsMenu.Items.AddRange(new BarItem[] { standardToolbar, formatToolbar });
viewMenu.Items.Add(toolbarsMenu);

mainFrameBarManager1.Items.AddRange(new BarItem[] 
{ 
    viewMenu, toolbarsMenu, standardToolbar, formatToolbar 
});
```

### Properties

| Property | Type | Description |
|----------|------|-------------|
| **Text** | string | Menu text |
| **Items** | BarItemCollection | Child items |
| **Checked** | bool | Show checkmark |
| **Enabled** | bool | Enable/disable entire submenu |
| **Category** | string | For organizing in customization UI |

## DropDownBarItem - Custom Popups

Displays a custom control in a popup when clicked, useful for color pickers, calendars, or custom panels.

### Basic Usage

```csharp
// Create dropdown item
DropDownBarItem colorItem = new DropDownBarItem();
colorItem.Text = "&Color";

// Create popup container
PopupControlContainer popupContainer = new PopupControlContainer();

// Add control to popup
ColorPickerUIAdv colorPicker = new ColorPickerUIAdv();
popupContainer.Controls.Add(colorPicker);
this.Controls.Add(popupContainer);

// Assign to dropdown item
colorItem.PopupControlContainer = popupContainer;

mainFrameBarManager1.Items.Add(colorItem);
mainFrameBarManager1.Bars[0].Items.Add(colorItem);
```

### With Event Handling

```csharp
// Handle color selection
colorPicker.SelectedColorChanged += (sender, args) =>
{
    MessageBox.Show($"Selected color: {colorPicker.SelectedColor}");
};

// Handle popup opening/closing
colorItem.PopupItemShow += (sender, args) => 
{ 
    // Popup about to show
};

colorItem.PopupItemHide += (sender, args) => 
{ 
    // Popup about to hide
};
```

### Properties

| Property | Type | Description |
|----------|------|-------------|
| **Text** | string | Display text |
| **PopupControlContainer** | PopupControlContainer | Container with custom control |
| **AutoClose** | bool | Close popup after selection |
| **Enabled** | bool | Enable/disable item |

## ComboBoxBarItem - Selection Lists

Combo box in menu/toolbar for selecting from predefined options.

### Basic Usage

```csharp
// Create combo item
ComboBoxBarItem fontSizeItem = new ComboBoxBarItem();
fontSizeItem.Text = "Font Size:";

// Add options to ChoiceList
fontSizeItem.ChoiceList.AddRange(new string[] 
{ 
    "8pt", "10pt", "12pt", "14pt", "16pt", "18pt", "20pt", "24pt", "28pt", "32pt" 
});

// Set default selection
fontSizeItem.SelectedIndex = 2;  // 12pt

mainFrameBarManager1.Items.Add(fontSizeItem);
mainFrameBarManager1.Bars[0].Items.Add(fontSizeItem);
```

### Event Handling

```csharp
// Handle selection change
fontSizeItem.SelectedIndexChanged += (sender, args) =>
{
    string selectedSize = fontSizeItem.ChoiceList[fontSizeItem.SelectedIndex];
    ApplyFontSize(selectedSize);
};
```

### Designer Setup

1. Open Customize dialog
2. Drag ComboBoxBarItem to toolbar
3. Select item, find **ChoiceList** property
4. Click ellipsis to open String Collection Editor
5. Add items (one per line)

### Properties

| Property | Type | Description |
|----------|------|-------------|
| **ChoiceList** | StringCollection | Available options |
| **SelectedIndex** | int | Currently selected index (-1 if none) |
| **Text** | string | Display label |
| **TextBoxValue** | string | Current text in combo |
| **DropDownStyle** | ComboBoxStyle | Dropdown vs. simple combo |
| **Enabled** | bool | Enable/disable item |

## StaticBarItem - Label Items

Display static text or visual separators without interactivity.

### Basic Usage

```csharp
// Label item
StaticBarItem statusLabel = new StaticBarItem();
statusLabel.Text = "Ready";

mainFrameBarManager1.Items.Add(statusLabel);
mainFrameBarManager1.Bars[0].Items.Add(statusLabel);
```

### Separator

```csharp
// Visual separator
StaticBarItem separator = new StaticBarItem();
separator.Text = "-";

fileMenu.Items.Add(separator);
mainFrameBarManager1.Items.Add(separator);
```

### Grouped Items with Separator

```csharp
// New, Open, Save - grouped together
fileMenu.Items.AddRange(new BarItem[] { newItem, openItem, saveItem });

// Separator
fileMenu.Items.Add(new StaticBarItem() { Text = "-" });

// Exit - separate group
fileMenu.Items.Add(exitItem);
```

### Properties

| Property | Type | Description |
|----------|------|-------------|
| **Text** | string | Display text ("-" creates separator) |
| **Category** | string | For organizing in customization UI |

## TextBoxBarItem - Text Input

Text input field in menu or toolbar for user input.

### Basic Usage

```csharp
// Create text box item
TextBoxBarItem searchItem = new TextBoxBarItem();
searchItem.Text = "Search:";
searchItem.TextBoxValue = "";

mainFrameBarManager1.Items.Add(searchItem);
mainFrameBarManager1.Bars[0].Items.Add(searchItem);
```

### Event Handling

```csharp
// Handle text changes
searchItem.TextBoxValueChanged += (sender, args) =>
{
    string searchText = searchItem.TextBoxValue;
    PerformSearch(searchText);
};

// Handle Enter key
searchItem.TextBoxKeyDown += (sender, args) =>
{
    if (args.KeyCode == Keys.Return)
    {
        ExecuteSearch(searchItem.TextBoxValue);
        args.Handled = true;
    }
};
```

### Designer Setup

1. Drag TextBoxBarItem to toolbar
2. Set **TextBoxValue** property to default text
3. Set width for input field
4. Configure TextBoxKeyDown event if needed

### Properties

| Property | Type | Description |
|----------|------|-------------|
| **TextBoxValue** | string | Current text |
| **Text** | string | Display label |
| **TextBoxWidth** | int | Width of text box |
| **Enabled** | bool | Enable/disable input |

## ToolBarListBarItem - Customization

Displays toolbar customization options in menu, allowing end-users to show/hide specific toolbars.

### Basic Usage

```csharp
// Create toolbar list item
ToolBarListBarItem toolbarListItem = new ToolBarListBarItem();
toolbarListItem.BarName = "ToolbarsMenu";
toolbarListItem.Text = "&Toolbars";

mainFrameBarManager1.Items.Add(toolbarListItem);
mainFrameBarManager1.Bars[0].Items.Add(toolbarListItem);
```

### Properties

| Property | Type | Description |
|----------|------|-------------|
| **BarName** | string | Identifier for toolbar list |
| **Text** | string | Display text |
| **Category** | string | For organizing in customization UI |

When clicked, displays all available toolbars as checkable sub-menu items. Users can toggle toolbar visibility.

---

## Common Properties

All BarItem types inherit common properties:

| Property | Type | Description |
|----------|------|-------------|
| **Text** | string | Display text; & for mnemonics |
| **Category** | string | For organizing in customization UI |
| **Enabled** | bool | Whether item is interactive |
| **Tooltip** | SuperToolTip | Associated tooltip |
| **ShowTooltip** | bool | Display tooltip on hover |
| **ShowMnemonicUnderlinesAlways** | bool | Always show mnemonic underlines |
| **MergeOrder** | int | For MDI menu merging |
| **MergeType** | MergeType | How to merge in MDI scenarios |

---

## Selection Guide

| Use Case | Item Type |
|----------|-----------|
| Basic menu action | **BarItem** |
| Sub-menu with children | **ParentBarItem** |
| Color picker menu | **DropDownBarItem** + custom control |
| Font size selection | **ComboBoxBarItem** |
| Menu separator | **StaticBarItem** with "-" |
| Search input | **TextBoxBarItem** |
| Toolbar visibility menu | **ToolBarListBarItem** |
| Static text/label | **StaticBarItem** |

Choose the item type that best matches your functional requirement.
