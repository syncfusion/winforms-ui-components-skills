# BarItem Types in PopupMenu

## Table of Contents
- [Overview](#overview)
- [BarItem (Standard Menu Item)](#baritem-standard-menu-item)
- [ParentBarItem (Submenu Container)](#parentbaritem-submenu-container)
- [DropDownBarItem (Custom Dropdown)](#dropdownbaritem-custom-dropdown)
- [ComboBoxBarItem (Combo Box)](#comboboxbaritem-combo-box)
- [ListBarItem (List Selection)](#listbaritem-list-selection)
- [StaticBarItem (Label/Static Text)](#staticbaritem-labelstatic-text)
- [TextBoxBarItem (Text Input)](#textboxbaritem-text-input)
- [Type Selection Guide](#type-selection-guide)

## Overview

PopupMenu supports seven types of BarItems, each serving different interaction patterns. Understanding when to use each type is essential for creating effective menu systems.

**Available Types:**
1. **BarItem** - Standard clickable menu item
2. **ParentBarItem** - Container for submenu items (hierarchical menus)
3. **DropDownBarItem** - Dropdown with custom controls (ColorPicker, panels)
4. **ComboBoxBarItem** - Editable combo box for selection/input
5. **ListBarItem** - Fixed list of child items
6. **StaticBarItem** - Non-interactive label text
7. **TextBoxBarItem** - Text input field in menu

## BarItem (Standard Menu Item)

The most common menu item type for standard commands and actions.

### Characteristics
- Displays text and optional image
- Responds to Click events
- Supports keyboard shortcuts
- Can be checked/unchecked
- Can be enabled/disabled

### When to Use
- Standard menu commands (Save, Copy, Delete, etc.)
- Actions that execute immediately on click
- Menu items that trigger dialogs or windows
- Commands with keyboard shortcuts

### Designer Implementation

1. Open BarItem Collection Editor (ParentBarItem → Items)
2. Add → Select **BarItem**
3. Configure properties:
   - **Appearance → Text:** "Save"
   - **Appearance → Image:** Select icon file
   - **Data → Shortcut:** CtrlS

### Code Implementation

```csharp
BarItem saveItem = new BarItem {
    Text = "Save",
    SizeToFit = true,
    Image = new ImageExt(System.Drawing.Image.FromFile(@"icons\save.png")),
    Shortcut = Shortcut.CtrlS
};
saveItem.Click += (s, e) => SaveDocument();
parentBarItem1.Items.Add(saveItem);
```

## ParentBarItem (Submenu Container)

Acts as a parent control for sub-menu items, enabling hierarchical menu structures.

### Characteristics
- Holds child BarItems (of any type)
- Displays arrow indicator for submenu
- Can be nested for multiple levels
- Supports all BarItem properties (text, image, etc.)

### When to Use
- Creating submenus (File → New → New Project)
- Grouping related commands under categories
- Building hierarchical menu structures
- Organizing complex menu systems

### Designer Implementation

1. Add ParentBarItem to the main ParentBarItem
2. Select the newly added ParentBarItem
3. In Properties panel: **Misc → Items** → Open Collection Editor
4. Add child BarItems (can be any type, including more ParentBarItems)
5. Set **Appearance → Text** for the parent and children

### Code Implementation

```csharp
// Create parent with submenu
ParentBarItem fileMenu = new ParentBarItem { Text = "File", SizeToFit = true };
ParentBarItem newSubmenu = new ParentBarItem { Text = "New", SizeToFit = true };

// Add child items
newSubmenu.Items.AddRange(new BarItem[] {
    new BarItem { Text = "New Project...", SizeToFit = true },
    new BarItem { Text = "New File...", SizeToFit = true }
});

fileMenu.Items.Add(newSubmenu);
parentBarItem1.Items.Add(fileMenu);
```

## DropDownBarItem (Custom Dropdown)

Displays a custom popup with any WinForms controls via PopupControlContainer.

### Characteristics
- Shows custom controls in dropdown (ColorPicker, custom panels, etc.)
- Uses PopupControlContainer to host controls
- Supports any WinForms control or custom control
- Ideal for rich UI interactions beyond standard menus

### When to Use
- Color pickers in menu
- Custom date/time selectors
- Mini control panels
- Rich UI beyond simple lists
- Complex input scenarios

### Designer Implementation

1. Drag **PopupControlContainer** onto form (from Syncfusion toolbox)
2. Add controls to PopupControlContainer (e.g., ColorPickerUIAdv)
3. Add **DropDownBarItem** to menu via Collection Editor
4. Select DropDownBarItem in Properties panel
5. **Misc → PopupControlContainer:** Select the container from dropdown
6. Set **Appearance → Text** for the dropdown item

### Code Implementation

```csharp
// Create container with ColorPicker
PopupControlContainer popupContainer = new PopupControlContainer { Size = new Size(220, 250) };
ColorPickerUIAdv colorPicker = new ColorPickerUIAdv { Size = new Size(212, 237) };
colorPicker.Picked += (s, args) => richTextBox1.SelectionColor = args.Color;
popupContainer.Controls.Add(colorPicker);

// Create DropDownBarItem
DropDownBarItem colorDropdown = new DropDownBarItem {
    Text = "Text Color",
    SizeToFit = true,
    PopupControlContainer = popupContainer
};
parentBarItem1.Items.Add(colorDropdown);
```

## ComboBoxBarItem (Combo Box)

Provides combo box functionality within the menu, allowing selection from predefined list with optional editing.

### Characteristics
- Displays as editable combo box
- ChoiceList property for dropdown items
- TextBoxValue property for selected/entered value
- Supports keyboard input and dropdown selection

### When to Use
- Font selection menus
- Size/scale selection with custom values
- Predefined options with custom input capability
- Quick selection lists with editing

### Designer Implementation

1. Add **ComboBoxBarItem** via Collection Editor
2. Select ComboBoxBarItem in Properties panel
3. **Data → TextBoxValue:** Set default text (e.g., "Edit")
4. **Data → ChoiceList:** Click ellipsis → Add items in String Collection Editor

### Code Implementation

```csharp
ComboBoxBarItem fontCombo = new ComboBoxBarItem {
    SizeToFit = true,
    TextBoxValue = "Segoe UI"
};
fontCombo.ChoiceList.AddRange(new string[] { "Arial", "Calibri", "Segoe UI", "Times New Roman" });
fontCombo.Click += (s, e) => {
    var combo = s as ComboBoxBarItem;
    if (combo != null && richTextBox1.SelectionLength > 0)
        richTextBox1.SelectionFont = new Font(combo.TextBoxValue, richTextBox1.SelectionFont.Size);
};
parentBarItem1.Items.Add(fontCombo);
```

## ListBarItem (List Selection)

Displays a fixed list of child items for selection.

### Characteristics
- Shows list of items in menu
- ChildCaptions property for list items
- Fixed list (not editable like ComboBox)
- Compact display of multiple options

### When to Use
- Fixed selection lists
- Recent files/items
- Predefined options without editing
- Quick selection from known set

### Designer Implementation

1. Add **ListBarItem** via Collection Editor
2. Select ListBarItem in Properties panel
3. **Appearance → Text:** Set label (e.g., "Recent Files")
4. **Data → ChildCaptions:** Click ellipsis → Add items in String Collection Editor

### Code Implementation

```csharp
ListBarItem recentList = new ListBarItem { Text = "Recent Files", SizeToFit = true };
recentList.ChildCaptions.AddRange(new string[] { "Document1.txt", "Report.docx", "Data.xlsx" });
parentBarItem1.Items.Add(recentList);
```

## StaticBarItem (Label/Static Text)

Non-interactive label used as heading or separator text.

### Characteristics
- Displays text only (no click interaction)
- No hover effects
- Acts as label for adjacent items
- Visual grouping element

### When to Use
- Section headings in menus
- Labels for adjacent items
- Informational text
- Visual organization without interaction

### Designer Implementation

1. Add **StaticBarItem** via Collection Editor
2. Set **Appearance → Text** to your label text

### Code Implementation

```csharp
StaticBarItem header = new StaticBarItem { Text = "--- Recent Actions ---", SizeToFit = true };
parentBarItem1.Items.Add(header);
```

## TextBoxBarItem (Text Input)

Provides text input field within the menu.

### Characteristics
- Editable text box in menu
- TextBoxValue property for text content
- Value property (same as TextBoxValue)
- MinWidth property for sizing

### When to Use
- Search boxes in menus
- Quick text input
- Filter/find functionality
- Simple data entry in menu context

### Designer Implementation

1. Add **TextBoxBarItem** via Collection Editor
2. Select TextBoxBarItem in Properties panel
3. **Appearance → Text:** Set label (e.g., "Search:")
4. **Misc → TextBoxValue:** Set default text
5. **Layout → MinWidth:** Set width (e.g., 100)

### Code Implementation

```csharp
TextBoxBarItem searchBox = new TextBoxBarItem {
    Text = "Search:",
    TextBoxValue = "",
    MinWidth = 120,
    SizeToFit = true
};
searchBox.Click += (s, e) => {
    var textBox = s as TextBoxBarItem;
    if (textBox != null) PerformSearch(textBox.TextBoxValue);
};
parentBarItem1.Items.Add(searchBox);
```

## Type Selection Guide

### Decision Tree

**Need user action/command?**
→ Use **BarItem**

**Need submenu/hierarchy?**
→ Use **ParentBarItem**

**Need custom controls (ColorPicker, etc.)?**
→ Use **DropDownBarItem** with PopupControlContainer

**Need selection from list WITH editing?**
→ Use **ComboBoxBarItem**

**Need selection from fixed list WITHOUT editing?**
→ Use **ListBarItem**

**Need non-interactive text/label?**
→ Use **StaticBarItem**

**Need text input field?**
→ Use **TextBoxBarItem**

### Common Combinations

**Text Editor Context Menu:**
- BarItems: Cut, Copy, Paste
- ParentBarItem: Format (with font/style submenus)
- ComboBoxBarItem: Font selection
- DropDownBarItem: Color picker

**File Browser Context Menu:**
- BarItems: Open, Rename, Delete
- ParentBarItem: Send To (with destination submenus)
- ListBarItem: Recent Locations

**Application Menu Bar:**
- ParentBarItems: File, Edit, View, Help (each with BarItem children)
- StaticBarItems: Section headers within menus
- TextBoxBarItem: Quick search in menu
