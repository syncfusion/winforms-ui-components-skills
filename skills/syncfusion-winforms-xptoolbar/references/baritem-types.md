# BarItem Types and Collections

## Table of Contents
- [Overview](#overview)
- [BarItem (Standard Button)](#baritem-standard-button)
- [ParentBarItem (Submenu)](#parentbaritem-submenu)
- [DropDownBarItem (Dropdown List)](#dropdownbaritem-dropdown-list)
- [ComboBoxBarItem (Editable ComboBox)](#comboboxbaritem-editable-combobox)
- [ListBarItem (List Selection)](#listbaritem-list-selection)
- [StaticBarItem (Label)](#staticbaritem-label)
- [TextBoxBarItem (Text Input)](#textboxbaritem-text-input)
- [ToolbarListBarItem (Toolbar List)](#toolbarlistbaritem-toolbar-list)
- [Separator](#separator)
- [Managing Items Collection](#managing-items-collection)

## Overview

The XPToolBar control supports eight different types of BarItems, each designed for specific scenarios. Understanding when to use each type is crucial for creating intuitive and functional toolbars. The available BarItem types are:

1. **BarItem** - Standard clickable buttons
2. **ParentBarItem** - Submenu containers with child items
3. **DropDownBarItem** - Dropdown popup with custom controls
4. **ComboBoxBarItem** - Editable combo box for input and selection
5. **ListBarItem** - List selection with child items
6. **StaticBarItem** - Non-clickable labels or status text
7. **TextBoxBarItem** - Text input field
8. **ToolbarListBarItem** - Toolbar list display

Additionally, **Separator** items can be used to visually divide groups of bar items.

## BarItem (Standard Button)

### Description

BarItem represents an individual clickable button in the toolbar or menu structure. It's the most commonly used BarItem type for standard menu items and toolbar buttons.

### Key Properties

- **Text** - Display text for the button
- **Image** - Icon image for the button
- **ToolTip** - Tooltip text shown on hover
- **Enabled** - Whether the button is enabled or disabled
- **Visible** - Whether the button is visible
- **SizeToFit** - Auto-size the button to fit its content

### Code Example with Click Event

```csharp
// Create a BarItem
BarItem saveItem = new BarItem();
saveItem.Text = "Save";
saveItem.ToolTip = "Save the current document";
saveItem.Image = Image.FromFile(@"..\..\Images\Save.png");
saveItem.SizeToFit = true;

// Handle Click event
saveItem.Click += (sender, e) => {
    // Perform save operation
    MessageBox.Show("Document saved successfully!");
};

// Add to toolbar
xpToolBar1.Bar.Items.Add(saveItem);
```

```vb
' Create a BarItem
Dim saveItem As New BarItem()
saveItem.Text = "Save"
saveItem.ToolTip = "Save the current document"
saveItem.Image = Image.FromFile("..\..\Images\Save.png")
saveItem.SizeToFit = True

' Handle Click event
AddHandler saveItem.Click, Sub(sender, e)
    ' Perform save operation
    MessageBox.Show("Document saved successfully!")
End Sub

' Add to toolbar
xpToolBar1.Bar.Items.Add(saveItem)
```

### When to Use

- Standard menu items (File, Edit, View, Help)
- Toolbar action buttons (New, Open, Save, Print)
- Command buttons that trigger immediate actions
- Any clickable element that doesn't require child items

## ParentBarItem (Submenu)

### Description

ParentBarItem represents a menu item that contains child items, creating a submenu structure. When clicked, it displays its child items in a dropdown menu.

### Items Collection Usage

ParentBarItem has an `Items` collection that holds its child BarItems. You can add any type of BarItem as a child, including nested ParentBarItems.

### Code Example with Submenu Structure

```csharp
// Create parent item
ParentBarItem fileMenu = new ParentBarItem();
fileMenu.Text = "File";
fileMenu.SizeToFit = true;

// Create child items
BarItem newItem = new BarItem();
newItem.Text = "New";
newItem.Image = Image.FromFile(@"..\..\Images\New.png");
newItem.Click += (s, e) => MessageBox.Show("New clicked");

BarItem openItem = new BarItem();
openItem.Text = "Open";
openItem.Image = Image.FromFile(@"..\..\Images\Open.png");
openItem.Click += (s, e) => MessageBox.Show("Open clicked");

BarItem saveItem = new BarItem();
saveItem.Text = "Save";
saveItem.Image = Image.FromFile(@"..\..\Images\Save.png");
saveItem.Click += (s, e) => MessageBox.Show("Save clicked");

// Add child items to parent
fileMenu.Items.AddRange(new BarItem[] { newItem, openItem, saveItem });

// Add to toolbar
xpToolBar1.Bar.Items.Add(fileMenu);
```

```vb
' Create parent item
Dim fileMenu As New ParentBarItem()
fileMenu.Text = "File"
fileMenu.SizeToFit = True

' Create child items
Dim newItem As New BarItem()
newItem.Text = "New"
newItem.Image = Image.FromFile("..\..\Images\New.png")
AddHandler newItem.Click, Sub(s, e) MessageBox.Show("New clicked")

Dim openItem As New BarItem()
openItem.Text = "Open"
openItem.Image = Image.FromFile("..\..\Images\Open.png")
AddHandler openItem.Click, Sub(s, e) MessageBox.Show("Open clicked")

Dim saveItem As New BarItem()
saveItem.Text = "Save"
saveItem.Image = Image.FromFile("..\..\Images\Save.png")
AddHandler saveItem.Click, Sub(s, e) MessageBox.Show("Save clicked")

' Add child items to parent
fileMenu.Items.AddRange(New BarItem() { newItem, openItem, saveItem })

' Add to toolbar
xpToolBar1.Bar.Items.Add(fileMenu)
```

### Nested Submenus Example

```csharp
// Create nested submenu structure
ParentBarItem editMenu = new ParentBarItem();
editMenu.Text = "Edit";

// Create "Find" submenu
ParentBarItem findMenu = new ParentBarItem();
findMenu.Text = "Find";

BarItem findItem = new BarItem();
findItem.Text = "Find...";
findItem.Click += (s, e) => MessageBox.Show("Find dialog");

BarItem findNextItem = new BarItem();
findNextItem.Text = "Find Next";
findNextItem.Click += (s, e) => MessageBox.Show("Find next");

// Add items to Find submenu
findMenu.Items.AddRange(new BarItem[] { findItem, findNextItem });

// Add Find submenu to Edit menu
editMenu.Items.Add(findMenu);

// Add other Edit menu items
BarItem cutItem = new BarItem();
cutItem.Text = "Cut";
editMenu.Items.Add(cutItem);

// Add to toolbar
xpToolBar1.Bar.Items.Add(editMenu);
```

```vb
' Create nested submenu structure
Dim editMenu As New ParentBarItem()
editMenu.Text = "Edit"

' Create "Find" submenu
Dim findMenu As New ParentBarItem()
findMenu.Text = "Find"

Dim findItem As New BarItem()
findItem.Text = "Find..."
AddHandler findItem.Click, Sub(s, e) MessageBox.Show("Find dialog")

Dim findNextItem As New BarItem()
findNextItem.Text = "Find Next"
AddHandler findNextItem.Click, Sub(s, e) MessageBox.Show("Find next")

' Add items to Find submenu
findMenu.Items.AddRange(New BarItem() { findItem, findNextItem })

' Add Find submenu to Edit menu
editMenu.Items.Add(findMenu)

' Add other Edit menu items
Dim cutItem As New BarItem()
cutItem.Text = "Cut"
editMenu.Items.Add(cutItem)

' Add to toolbar
xpToolBar1.Bar.Items.Add(editMenu)
```

### When to Use

- Menu items that contain multiple related actions (File, Edit, View menus)
- Grouping related commands under a single parent
- Creating hierarchical menu structures
- Any scenario requiring nested menu levels

## DropDownBarItem (Dropdown List)

### Description

DropDownBarItem displays a popup control container when clicked. Unlike ParentBarItem, it can host any Windows Forms control through a PopupControlContainer.

### ChoiceList Property

While DropDownBarItem primarily uses PopupControlContainer for custom controls, it provides a flexible way to present dropdown content.

### Code Example with PopupControlContainer

```csharp
// Create dropdown bar item
DropDownBarItem viewDropdown = new DropDownBarItem();
viewDropdown.Text = "View";
viewDropdown.SizeToFit = true;

// Create popup control container
PopupControlContainer popupContainer = new PopupControlContainer();
popupContainer.Size = new Size(150, 100);

// Add controls to popup
Button toolbarsButton = new Button();
toolbarsButton.Text = "Toolbars";
toolbarsButton.Width = 120;
toolbarsButton.Location = new Point(10, 10);
toolbarsButton.Click += (s, e) => {
    MessageBox.Show("Toolbars button clicked");
};

CheckBox statusBarCheck = new CheckBox();
statusBarCheck.Text = "Status Bar";
statusBarCheck.Location = new Point(10, 45);
statusBarCheck.CheckedChanged += (s, e) => {
    MessageBox.Show($"Status Bar: {statusBarCheck.Checked}");
};

popupContainer.Controls.Add(toolbarsButton);
popupContainer.Controls.Add(statusBarCheck);

// Associate popup with dropdown item
viewDropdown.PopupControlContainer = popupContainer;

// Add to toolbar
xpToolBar1.Bar.Items.Add(viewDropdown);
```

```vb
' Create dropdown bar item
Dim viewDropdown As New DropDownBarItem()
viewDropdown.Text = "View"
viewDropdown.SizeToFit = True

' Create popup control container
Dim popupContainer As New PopupControlContainer()
popupContainer.Size = New Size(150, 100)

' Add controls to popup
Dim toolbarsButton As New Button()
toolbarsButton.Text = "Toolbars"
toolbarsButton.Width = 120
toolbarsButton.Location = New Point(10, 10)
AddHandler toolbarsButton.Click, Sub(s, e)
    MessageBox.Show("Toolbars button clicked")
End Sub

Dim statusBarCheck As New CheckBox()
statusBarCheck.Text = "Status Bar"
statusBarCheck.Location = New Point(10, 45)
AddHandler statusBarCheck.CheckedChanged, Sub(s, e)
    MessageBox.Show($"Status Bar: {statusBarCheck.Checked}")
End Sub

popupContainer.Controls.Add(toolbarsButton)
popupContainer.Controls.Add(statusBarCheck)

' Associate popup with dropdown item
viewDropdown.PopupControlContainer = popupContainer

' Add to toolbar
xpToolBar1.Bar.Items.Add(viewDropdown)
```

### When to Use

- Displaying custom controls in a dropdown (color pickers, date pickers, etc.)
- Complex selection interfaces that require more than simple list items
- Scenarios where you need rich UI elements in a popup
- Custom popup menus with mixed control types

## ComboBoxBarItem (Editable ComboBox)

### Description

ComboBoxBarItem provides an editable combo box control directly in the toolbar. Users can either select from the dropdown list or type custom values.

### Key Properties

- **ChoiceList** - Collection of items in the dropdown
- **TextBoxValue** - Current text value (selected or typed)
- **SelectedIndex** - Index of selected item
- **MinWidth** - Minimum width of the combo box

### Code Example with Selection and Typing

```csharp
// Create combo box bar item
ComboBoxBarItem configCombo = new ComboBoxBarItem();
configCombo.MinWidth = 100;

// Add items to dropdown
configCombo.ChoiceList.AddRange(new string[] {
    "Debug",
    "Release",
    "Testing",
    "Production"
});

// Set default value
configCombo.TextBoxValue = "Debug";

// Handle selection change
configCombo.TextChanged += (sender, e) => {
    ComboBoxBarItem combo = sender as ComboBoxBarItem;
    MessageBox.Show($"Configuration changed to: {combo.TextBoxValue}");
};

// Handle selected index change
configCombo.SelectedIndexChanged += (sender, e) => {
    ComboBoxBarItem combo = sender as ComboBoxBarItem;
    if (combo.SelectedIndex >= 0)
    {
        MessageBox.Show($"Selected index: {combo.SelectedIndex}");
    }
};

// Add to toolbar
xpToolBar1.Bar.Items.Add(configCombo);
```

```vb
' Create combo box bar item
Dim configCombo As New ComboBoxBarItem()
configCombo.MinWidth = 100

' Add items to dropdown
configCombo.ChoiceList.AddRange(New String() {
    "Debug",
    "Release",
    "Testing",
    "Production"
})

' Set default value
configCombo.TextBoxValue = "Debug"

' Handle selection change
AddHandler configCombo.TextChanged, Sub(sender, e)
    Dim combo As ComboBoxBarItem = TryCast(sender, ComboBoxBarItem)
    MessageBox.Show($"Configuration changed to: {combo.TextBoxValue}")
End Sub

' Handle selected index change
AddHandler configCombo.SelectedIndexChanged, Sub(sender, e)
    Dim combo As ComboBoxBarItem = TryCast(sender, ComboBoxBarItem)
    If combo.SelectedIndex >= 0 Then
        MessageBox.Show($"Selected index: {combo.SelectedIndex}")
    End If
End Sub

' Add to toolbar
xpToolBar1.Bar.Items.Add(configCombo)
```

### When to Use

- Build configuration selection (Debug, Release)
- Font selection dropdowns
- Recent items lists (recent files, recent searches)
- Any scenario requiring both predefined choices and custom input
- Platform or target selection

## ListBarItem (List Selection)

### Description

ListBarItem provides a dynamic list of child items that can be displayed in a toolbar. It's similar to ParentBarItem but optimized for list-style presentations.

### Configuration and Usage

```csharp
// Create list bar item
ListBarItem fontList = new ListBarItem();
fontList.Text = "Font Style";
fontList.SizeToFit = true;

// Add child items
fontList.ChoiceList.AddRange(new string[] {
    "Bold",
    "Italic",
    "Underline",
    "Strikethrough"
});

// Handle item selection
fontList.Click += (sender, e) => {
    MessageBox.Show($"Font style selected");
};

// Add to toolbar
xpToolBar1.Bar.Items.Add(fontList);
```

```vb
' Create list bar item
Dim fontList As New ListBarItem()
fontList.Text = "Font Style"
fontList.SizeToFit = True

' Add child items
fontList.ChoiceList.AddRange(New String() {
    "Bold",
    "Italic",
    "Underline",
    "Strikethrough"
})

' Handle item selection
AddHandler fontList.Click, Sub(sender, e)
    MessageBox.Show($"Font style selected")
End Sub

' Add to toolbar
xpToolBar1.Bar.Items.Add(fontList)
```

### When to Use

- Displaying lists of options or choices
- Font style selections
- Text formatting options
- Quick selection lists where items are related
- Dynamic list presentations

## StaticBarItem (Label)

### Description

StaticBarItem represents a non-clickable label or status text in the toolbar. It's used for displaying information rather than interactive elements.

### Usage Scenarios

StaticBarItem is ideal for:
- Status text display
- Labels for adjacent controls
- Separating sections with text headers
- Displaying read-only information

### Code Example

```csharp
// Create static bar item for label
StaticBarItem fontLabel = new StaticBarItem();
fontLabel.Text = "Font:";
fontLabel.SizeToFit = true;

// Create combo box for font selection (to demonstrate label usage)
ComboBoxBarItem fontCombo = new ComboBoxBarItem();
fontCombo.ChoiceList.AddRange(new string[] {
    "Arial",
    "Times New Roman",
    "Courier New",
    "Segoe UI"
});
fontCombo.TextBoxValue = "Segoe UI";
fontCombo.MinWidth = 120;

// Add both to toolbar
xpToolBar1.Bar.Items.AddRange(new BarItem[] { fontLabel, fontCombo });
```

```vb
' Create static bar item for label
Dim fontLabel As New StaticBarItem()
fontLabel.Text = "Font:"
fontLabel.SizeToFit = True

' Create combo box for font selection (to demonstrate label usage)
Dim fontCombo As New ComboBoxBarItem()
fontCombo.ChoiceList.AddRange(New String() {
    "Arial",
    "Times New Roman",
    "Courier New",
    "Segoe UI"
})
fontCombo.TextBoxValue = "Segoe UI"
fontCombo.MinWidth = 120

' Add both to toolbar
xpToolBar1.Bar.Items.AddRange(New BarItem() { fontLabel, fontCombo })
```

### When to Use

- Labels for adjacent input controls
- Status information display
- Section headers in toolbars
- Read-only text that provides context
- Displaying current state (e.g., "Line 45, Col 12")

## TextBoxBarItem (Text Input)

### Description

TextBoxBarItem provides a text input field directly in the toolbar, allowing users to enter text values.

### Key Properties

- **TextBoxValue** - Current text value
- **MinWidth** / **MinimumSize** - Minimum size of the text box
- **MaxLength** - Maximum character length

### Code Example with Validation

```csharp
// Create text box bar item
TextBoxBarItem searchBox = new TextBoxBarItem();
searchBox.TextBoxValue = "Search...";
searchBox.MinWidth = 150;

// Handle text changed event
searchBox.TextChanged += (sender, e) => {
    TextBoxBarItem textBox = sender as TextBoxBarItem;
    Console.WriteLine($"Search text: {textBox.TextBoxValue}");
};

// Handle Enter key for search
searchBox.KeyDown += (sender, e) => {
    if (e.KeyCode == Keys.Enter)
    {
        TextBoxBarItem textBox = sender as TextBoxBarItem;
        string searchText = textBox.TextBoxValue;
        
        // Validate input
        if (string.IsNullOrWhiteSpace(searchText) || searchText == "Search...")
        {
            MessageBox.Show("Please enter a search term");
            return;
        }
        
        // Perform search
        MessageBox.Show($"Searching for: {searchText}");
    }
};

// Add to toolbar
xpToolBar1.Bar.Items.Add(searchBox);
```

```vb
' Create text box bar item
Dim searchBox As New TextBoxBarItem()
searchBox.TextBoxValue = "Search..."
searchBox.MinWidth = 150

' Handle text changed event
AddHandler searchBox.TextChanged, Sub(sender, e)
    Dim textBox As TextBoxBarItem = TryCast(sender, TextBoxBarItem)
    Console.WriteLine($"Search text: {textBox.TextBoxValue}")
End Sub

' Handle Enter key for search
AddHandler searchBox.KeyDown, Sub(sender, e)
    If e.KeyCode = Keys.Enter Then
        Dim textBox As TextBoxBarItem = TryCast(sender, TextBoxBarItem)
        Dim searchText As String = textBox.TextBoxValue
        
        ' Validate input
        If String.IsNullOrWhiteSpace(searchText) OrElse searchText = "Search..." Then
            MessageBox.Show("Please enter a search term")
            Return
        End If
        
        ' Perform search
        MessageBox.Show($"Searching for: {searchText}")
    End If
End Sub

' Add to toolbar
xpToolBar1.Bar.Items.Add(searchBox)
```

### When to Use

- Search boxes in toolbars
- Quick input fields for filters
- URL or path entry
- Command line input
- Any scenario requiring text input in the toolbar

## ToolbarListBarItem (Toolbar List)

### Description

ToolbarListBarItem is used to display a list of toolbars and their states. It's particularly useful for applications with multiple configurable toolbars.

### Configuration

```csharp
// Create toolbar list bar item
ToolbarListBarItem toolbarList = new ToolbarListBarItem();
toolbarList.Text = "Toolbars";
toolbarList.SizeToFit = true;

// This item automatically populates with available toolbars
// when used in an application with multiple XPToolBar controls

// Add to toolbar
xpToolBar1.Bar.Items.Add(toolbarList);
```

```vb
' Create toolbar list bar item
Dim toolbarList As New ToolbarListBarItem()
toolbarList.Text = "Toolbars"
toolbarList.SizeToFit = True

' This item automatically populates with available toolbars
' when used in an application with multiple XPToolBar controls

' Add to toolbar
xpToolBar1.Bar.Items.Add(toolbarList)
```

### When to Use

- View menu for showing/hiding toolbars
- Toolbar management interfaces
- Applications with multiple configurable toolbars
- Providing users with toolbar visibility controls

## Separator

### Description

Separators are visual dividers used to group related bar items and improve toolbar organization. They appear as vertical lines between items.

### Using SeparatorIndices

```csharp
// Add bar items to toolbar
xpToolBar1.Bar.Items.AddRange(new BarItem[] {
    fileItem,      // Index 0
    editItem,      // Index 1
    viewItem,      // Index 2
    projectItem,   // Index 3
    buildItem,     // Index 4
    helpItem       // Index 5
});

// Add separators after indices 1 and 3
// This creates: File | Edit | View | Project | Build | Help
xpToolBar1.SeparatorIndices.AddRange(new int[] { 1, 3 });
```

```vb
' Add bar items to toolbar
xpToolBar1.Bar.Items.AddRange(New BarItem() {
    fileItem,      ' Index 0
    editItem,      ' Index 1
    viewItem,      ' Index 2
    projectItem,   ' Index 3
    buildItem,     ' Index 4
    helpItem       ' Index 5
})

' Add separators after indices 1 and 3
' This creates: File | Edit | View | Project | Build | Help
xpToolBar1.SeparatorIndices.AddRange(New Integer() { 1, 3 })
```

### When to Use

- Visually grouping related menu items
- Separating different categories of actions
- Improving toolbar readability
- Creating logical sections in menus
- Any time you need visual separation between items

## Managing Items Collection

### Adding Items at Runtime

```csharp
// Add single item
BarItem newItem = new BarItem();
newItem.Text = "New Item";
xpToolBar1.Bar.Items.Add(newItem);

// Add multiple items at once
BarItem[] items = new BarItem[] {
    new BarItem() { Text = "Item 1" },
    new BarItem() { Text = "Item 2" },
    new BarItem() { Text = "Item 3" }
};
xpToolBar1.Bar.Items.AddRange(items);

// Insert item at specific position
BarItem insertItem = new BarItem() { Text = "Inserted" };
xpToolBar1.Bar.Items.Insert(2, insertItem);
```

```vb
' Add single item
Dim newItem As New BarItem()
newItem.Text = "New Item"
xpToolBar1.Bar.Items.Add(newItem)

' Add multiple items at once
Dim items() As BarItem = New BarItem() {
    New BarItem() With {.Text = "Item 1"},
    New BarItem() With {.Text = "Item 2"},
    New BarItem() With {.Text = "Item 3"}
}
xpToolBar1.Bar.Items.AddRange(items)

' Insert item at specific position
Dim insertItem As New BarItem() With {.Text = "Inserted"}
xpToolBar1.Bar.Items.Insert(2, insertItem)
```

### Removing Items

```csharp
// Remove specific item
xpToolBar1.Bar.Items.Remove(barItem1);

// Remove at index
xpToolBar1.Bar.Items.RemoveAt(0);

// Remove all items
xpToolBar1.Bar.Items.Clear();

// Remove items matching condition
for (int i = xpToolBar1.Bar.Items.Count - 1; i >= 0; i--)
{
    if (xpToolBar1.Bar.Items[i].Text.Contains("Old"))
    {
        xpToolBar1.Bar.Items.RemoveAt(i);
    }
}
```

```vb
' Remove specific item
xpToolBar1.Bar.Items.Remove(barItem1)

' Remove at index
xpToolBar1.Bar.Items.RemoveAt(0)

' Remove all items
xpToolBar1.Bar.Items.Clear()

' Remove items matching condition
For i As Integer = xpToolBar1.Bar.Items.Count - 1 To 0 Step -1
    If xpToolBar1.Bar.Items(i).Text.Contains("Old") Then
        xpToolBar1.Bar.Items.RemoveAt(i)
    End If
Next
```

### Accessing and Modifying Items

```csharp
// Access item by index
BarItem firstItem = xpToolBar1.Bar.Items[0];

// Find item by text
BarItem foundItem = xpToolBar1.Bar.Items
    .Cast<BarItem>()
    .FirstOrDefault(item => item.Text == "File");

// Modify item properties
if (foundItem != null)
{
    foundItem.Enabled = false;
    foundItem.ToolTip = "Disabled temporarily";
}

// Iterate through all items
foreach (BarItem item in xpToolBar1.Bar.Items)
{
    Console.WriteLine($"Item: {item.Text}");
}
```

```vb
' Access item by index
Dim firstItem As BarItem = xpToolBar1.Bar.Items(0)

' Find item by text
Dim foundItem As BarItem = xpToolBar1.Bar.Items.Cast(Of BarItem)().
    FirstOrDefault(Function(item) item.Text = "File")

' Modify item properties
If foundItem IsNot Nothing Then
    foundItem.Enabled = False
    foundItem.ToolTip = "Disabled temporarily"
End If

' Iterate through all items
For Each item As BarItem In xpToolBar1.Bar.Items
    Console.WriteLine($"Item: {item.Text}")
Next
```

By understanding these BarItem types and collection management techniques, you can create rich, functional toolbars that provide intuitive interfaces for your WinForms applications.
