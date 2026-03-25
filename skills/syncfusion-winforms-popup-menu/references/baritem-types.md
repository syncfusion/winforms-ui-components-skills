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
BarItem saveItem = new BarItem();
saveItem.Text = "Save";
saveItem.SizeToFit = true;
saveItem.Image = new ImageExt(System.Drawing.Image.FromFile(@"icons\save.png"));
saveItem.Shortcut = System.Windows.Forms.Shortcut.CtrlS;
saveItem.Click += SaveItem_Click;

parentBarItem1.Items.Add(saveItem);

private void SaveItem_Click(object sender, EventArgs e)
{
    // Save logic here
    SaveDocument();
}
```

### VB.NET Implementation

```vb
Dim saveItem As New BarItem()
saveItem.Text = "Save"
saveItem.SizeToFit = True
saveItem.Image = New ImageExt(System.Drawing.Image.FromFile("icons\save.png"))
saveItem.Shortcut = System.Windows.Forms.Shortcut.CtrlS
AddHandler saveItem.Click, AddressOf SaveItem_Click

parentBarItem1.Items.Add(saveItem)

Private Sub SaveItem_Click(sender As Object, e As EventArgs)
    ' Save logic here
    SaveDocument()
End Sub
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
// Create parent menu item
ParentBarItem fileMenu = new ParentBarItem();
fileMenu.Text = "File";
fileMenu.SizeToFit = true;
fileMenu.MetroColor = System.Drawing.Color.LightSkyBlue;

// Create submenu
ParentBarItem newSubmenu = new ParentBarItem();
newSubmenu.Text = "New";
newSubmenu.SizeToFit = true;
newSubmenu.MetroColor = System.Drawing.Color.LightSkyBlue;

// Create child items
BarItem newProject = new BarItem();
newProject.Text = "New Project...";
newProject.SizeToFit = true;
newProject.Click += NewProject_Click;

BarItem newFile = new BarItem();
newFile.Text = "New File...";
newFile.SizeToFit = true;
newFile.Click += NewFile_Click;

// Build hierarchy
newSubmenu.Items.AddRange(new BarItem[] { newProject, newFile });
fileMenu.Items.Add(newSubmenu);
parentBarItem1.Items.Add(fileMenu);
```

### VB.NET Implementation

```vb
' Create parent menu item
Dim fileMenu As New ParentBarItem()
fileMenu.Text = "File"
fileMenu.SizeToFit = True
fileMenu.MetroColor = System.Drawing.Color.LightSkyBlue

' Create submenu
Dim newSubmenu As New ParentBarItem()
newSubmenu.Text = "New"
newSubmenu.SizeToFit = True
newSubmenu.MetroColor = System.Drawing.Color.LightSkyBlue

' Create child items
Dim newProject As New BarItem()
newProject.Text = "New Project..."
newProject.SizeToFit = True
AddHandler newProject.Click, AddressOf NewProject_Click

Dim newFile As New BarItem()
newFile.Text = "New File..."
newFile.SizeToFit = True
AddHandler newFile.Click, AddressOf NewFile_Click

' Build hierarchy
newSubmenu.Items.AddRange(New BarItem() {newProject, newFile})
fileMenu.Items.Add(newSubmenu)
parentBarItem1.Items.Add(fileMenu)
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
// Create PopupControlContainer
PopupControlContainer popupContainer = new PopupControlContainer();
popupContainer.Size = new System.Drawing.Size(220, 250);

// Add ColorPicker to container
ColorPickerUIAdv colorPicker = new ColorPickerUIAdv();
colorPicker.BeforeTouchSize = new System.Drawing.Size(13, 13);
colorPicker.ButtonsHeight = 25;
colorPicker.ColorItemSize = new System.Drawing.Size(17, 17);
colorPicker.Location = new System.Drawing.Point(3, 3);
colorPicker.Size = new System.Drawing.Size(212, 237);
colorPicker.Picked += ColorPicker_Picked;

popupContainer.Controls.Add(colorPicker);

// Create DropDownBarItem
DropDownBarItem colorDropdown = new DropDownBarItem();
colorDropdown.Text = "Text Color";
colorDropdown.SizeToFit = true;
colorDropdown.PopupControlContainer = popupContainer;

parentBarItem1.Items.Add(colorDropdown);

private void ColorPicker_Picked(object sender, ColorPickerUIAdv.ColorPickedEventArgs args)
{
    // Apply selected color
    richTextBox1.SelectionColor = args.Color;
}
```

### VB.NET Implementation

```vb
' Create PopupControlContainer
Dim popupContainer As New PopupControlContainer()
popupContainer.Size = New System.Drawing.Size(220, 250)

' Add ColorPicker to container
Dim colorPicker As New ColorPickerUIAdv()
colorPicker.BeforeTouchSize = New System.Drawing.Size(13, 13)
colorPicker.ButtonsHeight = 25
colorPicker.ColorItemSize = New System.Drawing.Size(17, 17)
colorPicker.Location = New System.Drawing.Point(3, 3)
colorPicker.Size = New System.Drawing.Size(212, 237)
AddHandler colorPicker.Picked, AddressOf ColorPicker_Picked

popupContainer.Controls.Add(colorPicker)

' Create DropDownBarItem
Dim colorDropdown As New DropDownBarItem()
colorDropdown.Text = "Text Color"
colorDropdown.SizeToFit = True
colorDropdown.PopupControlContainer = popupContainer

parentBarItem1.Items.Add(colorDropdown)

Private Sub ColorPicker_Picked(sender As Object, args As ColorPickerUIAdv.ColorPickedEventArgs)
    ' Apply selected color
    richTextBox1.SelectionColor = args.Color
End Sub
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
ComboBoxBarItem fontCombo = new ComboBoxBarItem();
fontCombo.SizeToFit = true;
fontCombo.TextBoxValue = "Segoe UI";
fontCombo.ChoiceList.AddRange(new string[] {
    "Arial",
    "Calibri",
    "Segoe UI",
    "Times New Roman",
    "Verdana"
});
fontCombo.Click += FontCombo_Click;

parentBarItem1.Items.Add(fontCombo);

private void FontCombo_Click(object sender, EventArgs e)
{
    ComboBoxBarItem combo = sender as ComboBoxBarItem;
    if (combo != null && richTextBox1.SelectionLength > 0)
    {
        richTextBox1.SelectionFont = new Font(combo.TextBoxValue, richTextBox1.SelectionFont.Size);
    }
}
```

### VB.NET Implementation

```vb
Dim fontCombo As New ComboBoxBarItem()
fontCombo.SizeToFit = True
fontCombo.TextBoxValue = "Segoe UI"
fontCombo.ChoiceList.AddRange(New String() { _
    "Arial", _
    "Calibri", _
    "Segoe UI", _
    "Times New Roman", _
    "Verdana" _
})
AddHandler fontCombo.Click, AddressOf FontCombo_Click

parentBarItem1.Items.Add(fontCombo)

Private Sub FontCombo_Click(sender As Object, e As EventArgs)
    Dim combo As ComboBoxBarItem = TryCast(sender, ComboBoxBarItem)
    If combo IsNot Nothing AndAlso richTextBox1.SelectionLength > 0 Then
        richTextBox1.SelectionFont = New Font(combo.TextBoxValue, richTextBox1.SelectionFont.Size)
    End If
End Sub
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
ListBarItem recentList = new ListBarItem();
recentList.Text = "Recent Files";
recentList.SizeToFit = true;
recentList.ChildCaptions.AddRange(new string[] {
    "Document1.txt",
    "Report.docx",
    "Data.xlsx"
});

parentBarItem1.Items.Add(recentList);
```

### VB.NET Implementation

```vb
Dim recentList As New ListBarItem()
recentList.Text = "Recent Files"
recentList.SizeToFit = True
recentList.ChildCaptions.AddRange(New String() { _
    "Document1.txt", _
    "Report.docx", _
    "Data.xlsx" _
})

parentBarItem1.Items.Add(recentList)
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
StaticBarItem header = new StaticBarItem();
header.Text = "--- Recent Actions ---";
header.SizeToFit = true;

parentBarItem1.Items.Add(header);
```

### VB.NET Implementation

```vb
Dim header As New StaticBarItem()
header.Text = "--- Recent Actions ---"
header.SizeToFit = True

parentBarItem1.Items.Add(header)
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
TextBoxBarItem searchBox = new TextBoxBarItem();
searchBox.Text = "Search:";
searchBox.TextBoxValue = "";
searchBox.Value = "";
searchBox.MinWidth = 120;
searchBox.SizeToFit = true;
searchBox.Click += SearchBox_Click;

parentBarItem1.Items.Add(searchBox);

private void SearchBox_Click(object sender, EventArgs e)
{
    TextBoxBarItem textBox = sender as TextBoxBarItem;
    if (textBox != null)
    {
        string searchTerm = textBox.TextBoxValue;
        // Perform search logic
        PerformSearch(searchTerm);
    }
}
```

### VB.NET Implementation

```vb
Dim searchBox As New TextBoxBarItem()
searchBox.Text = "Search:"
searchBox.TextBoxValue = ""
searchBox.Value = ""
searchBox.MinWidth = 120
searchBox.SizeToFit = True
AddHandler searchBox.Click, AddressOf SearchBox_Click

parentBarItem1.Items.Add(searchBox)

Private Sub SearchBox_Click(sender As Object, e As EventArgs)
    Dim textBox As TextBoxBarItem = TryCast(sender, TextBoxBarItem)
    If textBox IsNot Nothing Then
        Dim searchTerm As String = textBox.TextBoxValue
        ' Perform search logic
        PerformSearch(searchTerm)
    End If
End Sub
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
