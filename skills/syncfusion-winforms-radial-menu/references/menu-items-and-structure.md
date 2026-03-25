# Menu Items and Hierarchical Structure

## Table of Contents
- [RadialMenuItem Overview](#radialmenuitem-overview)
- [Creating Menu Items](#creating-menu-items)
- [Item Properties](#item-properties)
- [CheckMode Property](#checkmode-property)
- [Grouping with Radio Buttons](#grouping-with-radio-buttons)
- [Building Hierarchical Menus](#building-hierarchical-menus)
- [Event Handling](#event-handling)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)
- [Common Issues and Solutions](#common-issues-and-solutions)

## RadialMenuItem Overview

`RadialMenuItem` is the fundamental building block of the RadialMenu control. Each menu item represents an action or submenu that users can interact with. Menu items support text, icons, checkboxes, radio buttons, and hierarchical nesting for complex menu structures.

**Key Capabilities:**
- Display text labels and icons
- Support checkbox and radio button modes
- Create nested submenu hierarchies
- Handle click events for user actions
- Group related items together
- Maintain checked/unchecked states

**When to Use RadialMenuItem:**
- Building context menus with multiple actions
- Creating format toolbars (bold, italic, underline)
- Implementing touch-friendly navigation
- Designing hierarchical menu systems
- Providing quick access to common commands

## Creating Menu Items

Menu items are created by instantiating the `RadialMenuItem` class and adding them to the RadialMenu's Items collection.

**Basic Item Creation:**

```csharp
using Syncfusion.Windows.Forms.Tools;

// Create a single menu item
RadialMenuItem editItem = new RadialMenuItem();
editItem.Text = "Edit";

// Add to RadialMenu
this.radialMenu1.Items.Add(editItem);
```

**Creating Multiple Items Efficiently:**

```csharp
private void CreateMenuItems()
{
    // Array of item names
    string[] itemNames = { "New", "Open", "Save", "Close", "Print", "Export" };

    // Create and add items in a loop
    foreach (string name in itemNames)
    {
        RadialMenuItem item = new RadialMenuItem();
        item.Text = name;
        item.Click += MenuItem_Click;  // Attach event handler
        this.radialMenu1.Items.Add(item);
    }
}

private void MenuItem_Click(object sender, EventArgs e)
{
    RadialMenuItem clickedItem = sender as RadialMenuItem;
    MessageBox.Show($"You clicked: {clickedItem.Text}");
}
```

**Result:**
Six menu items are created programmatically with consistent event handling.

## Item Properties

Each RadialMenuItem has several important properties for customization.

### Text Property

The `Text` property sets the label displayed on the menu item.

```csharp
RadialMenuItem item = new RadialMenuItem();
item.Text = "Save Document";  // Clear, descriptive label
```

**Best Practices for Text:**
- Keep text short (1-2 words)
- Use title case (e.g., "Save File" not "save file")
- Avoid abbreviations unless well-known
- Consider internationalization

### ImageIndex Property

The `ImageIndex` property links the menu item to an image in the RadialMenu's ImageList.

```csharp
// First, set up the ImageList
ImageListAdv imageList = new ImageListAdv(this.components);
imageList.Images.Add(Image.FromFile("icons/new.png"));      // Index 0
imageList.Images.Add(Image.FromFile("icons/open.png"));     // Index 1
imageList.Images.Add(Image.FromFile("icons/save.png"));     // Index 2

this.radialMenu1.ImageList = imageList;

// Create items with image indices
RadialMenuItem newItem = new RadialMenuItem();
newItem.Text = "New";
newItem.ImageIndex = 0;  // References first image

RadialMenuItem openItem = new RadialMenuItem();
openItem.Text = "Open";
openItem.ImageIndex = 1;  // References second image

RadialMenuItem saveItem = new RadialMenuItem();
saveItem.Text = "Save";
saveItem.ImageIndex = 2;  // References third image

this.radialMenu1.Items.Add(newItem);
this.radialMenu1.Items.Add(openItem);
this.radialMenu1.Items.Add(saveItem);

// Set display style to show both text and images
this.radialMenu1.DisplayStyle = DisplayStyle.ImageAboveText;
```

**Result:**
Menu items display with icons above text labels, providing visual cues for quick recognition.

### ImageSize Property

Individual items can have custom image sizes different from the default.

```csharp
// Set uniform size for all items
this.radialMenu1.MenuItemImageSize = new Size(24, 24);

// Override size for specific item (e.g., emphasize important action)
RadialMenuItem importantItem = new RadialMenuItem();
importantItem.Text = "Save";
importantItem.ImageIndex = 2;
importantItem.ImageSize = new Size(32, 32);  // Larger than default
```

**Result:**
The "Save" item has a larger icon, drawing user attention to this important action.

## CheckMode Property

The `CheckMode` property enables checkboxes or radio buttons on menu items, allowing users to toggle options or select from mutually exclusive choices.

**CheckMode Options:**
- **None** - No checkbox (default behavior)
- **Check** - Checkbox mode (multiple selections allowed)
- **Option** - Radio button mode (single selection in group)

### None Mode (Default)

Items without checkboxes are used for immediate actions.

```csharp
RadialMenuItem actionItem = new RadialMenuItem();
actionItem.Text = "Execute";
actionItem.CheckMode = CheckMode.None;  // Default, can omit
actionItem.Click += (s, e) =>
{
    // Perform action immediately
    PerformOperation();
};
```

**When to Use:**
- Commands that execute immediately (Save, Print, Export)
- Navigation items that open submenus
- Actions that don't maintain state

### Check Mode (Checkboxes)

Checkbox mode allows multiple items to be checked simultaneously.

```csharp
private void CreateTextFormattingMenu()
{
    // Create formatting option items
    RadialMenuItem boldItem = new RadialMenuItem();
    boldItem.Text = "Bold";
    boldItem.CheckMode = CheckMode.Check;  // Enable checkbox
    boldItem.Click += FormatToggle_Click;

    RadialMenuItem italicItem = new RadialMenuItem();
    italicItem.Text = "Italic";
    italicItem.CheckMode = CheckMode.Check;
    italicItem.Click += FormatToggle_Click;

    RadialMenuItem underlineItem = new RadialMenuItem();
    underlineItem.Text = "Underline";
    underlineItem.CheckMode = CheckMode.Check;
    underlineItem.Click += FormatToggle_Click;

    // Add to menu
    this.radialMenu1.Items.Add(boldItem);
    this.radialMenu1.Items.Add(italicItem);
    this.radialMenu1.Items.Add(underlineItem);
}

private void FormatToggle_Click(object sender, EventArgs e)
{
    RadialMenuItem item = sender as RadialMenuItem;
    
    // Update text formatting based on checked state
    if (item.Text == "Bold")
        ApplyBoldFormatting(item.Checked);
    else if (item.Text == "Italic")
        ApplyItalicFormatting(item.Checked);
    else if (item.Text == "Underline")
        ApplyUnderlineFormatting(item.Checked);
}

private void ApplyBoldFormatting(bool isChecked)
{
    if (richTextBox1.SelectionLength > 0)
    {
        Font currentFont = richTextBox1.SelectionFont;
        FontStyle newStyle = isChecked 
            ? currentFont.Style | FontStyle.Bold 
            : currentFont.Style & ~FontStyle.Bold;
        richTextBox1.SelectionFont = new Font(currentFont, newStyle);
    }
}
```

**Result:**
Users can enable multiple text formatting options simultaneously (e.g., bold + italic).

**When to Use Check Mode:**
- Text formatting options (bold, italic, underline)
- View options (show grid, show rulers, show toolbar)
- Feature toggles (auto-save, spell check, word wrap)
- Filter selections (show all, show active, show completed)

## Grouping with Radio Buttons

Radio button mode (CheckMode.Option) combined with the `GroupName` property creates mutually exclusive option groups where only one item can be selected at a time.

### Creating Radio Button Groups

```csharp
private void CreateAlignmentMenu()
{
    // Create alignment option items
    RadialMenuItem leftAlign = new RadialMenuItem();
    leftAlign.Text = "Left";
    leftAlign.CheckMode = CheckMode.Option;  // Radio button mode
    leftAlign.GroupName = "alignment";       // Group identifier
    leftAlign.Checked = true;                // Default selection
    leftAlign.Click += Alignment_Click;

    RadialMenuItem centerAlign = new RadialMenuItem();
    centerAlign.Text = "Center";
    centerAlign.CheckMode = CheckMode.Option;
    centerAlign.GroupName = "alignment";     // Same group
    centerAlign.Click += Alignment_Click;

    RadialMenuItem rightAlign = new RadialMenuItem();
    rightAlign.Text = "Right";
    rightAlign.CheckMode = CheckMode.Option;
    rightAlign.GroupName = "alignment";      // Same group
    rightAlign.Click += Alignment_Click;

    RadialMenuItem justifyAlign = new RadialMenuItem();
    justifyAlign.Text = "Justify";
    justifyAlign.CheckMode = CheckMode.Option;
    justifyAlign.GroupName = "alignment";    // Same group
    justifyAlign.Click += Alignment_Click;

    // Add to menu
    this.radialMenu1.Items.Add(leftAlign);
    this.radialMenu1.Items.Add(centerAlign);
    this.radialMenu1.Items.Add(rightAlign);
    this.radialMenu1.Items.Add(justifyAlign);
}

private void Alignment_Click(object sender, EventArgs e)
{
    RadialMenuItem selectedItem = sender as RadialMenuItem;
    
    // Apply alignment based on selected option
    switch (selectedItem.Text)
    {
        case "Left":
            richTextBox1.SelectionAlignment = HorizontalAlignment.Left;
            break;
        case "Center":
            richTextBox1.SelectionAlignment = HorizontalAlignment.Center;
            break;
        case "Right":
            richTextBox1.SelectionAlignment = HorizontalAlignment.Right;
            break;
        case "Justify":
            // Apply justify formatting
            break;
    }
}
```

**Result:**
Only one alignment option can be selected at a time. Selecting a new option automatically deselects the previous one.

### Multiple Radio Button Groups

You can have multiple independent radio button groups in the same menu level by using different GroupName values.

```csharp
private void CreateMultipleGroupsMenu()
{
    // Text Alignment Group
    RadialMenuItem leftAlign = new RadialMenuItem();
    leftAlign.Text = "Left";
    leftAlign.CheckMode = CheckMode.Option;
    leftAlign.GroupName = "alignment";
    leftAlign.Checked = true;

    RadialMenuItem centerAlign = new RadialMenuItem();
    centerAlign.Text = "Center";
    centerAlign.CheckMode = CheckMode.Option;
    centerAlign.GroupName = "alignment";

    // Text Case Group (different group name)
    RadialMenuItem uppercase = new RadialMenuItem();
    uppercase.Text = "UPPERCASE";
    uppercase.CheckMode = CheckMode.Option;
    uppercase.GroupName = "textCase";  // Different group
    uppercase.Checked = true;

    RadialMenuItem lowercase = new RadialMenuItem();
    lowercase.Text = "lowercase";
    lowercase.CheckMode = CheckMode.Option;
    lowercase.GroupName = "textCase";  // Different group

    RadialMenuItem titleCase = new RadialMenuItem();
    titleCase.Text = "Title Case";
    titleCase.CheckMode = CheckMode.Option;
    titleCase.GroupName = "textCase";  // Different group

    // Add all items
    this.radialMenu1.Items.Add(leftAlign);
    this.radialMenu1.Items.Add(centerAlign);
    this.radialMenu1.Items.Add(uppercase);
    this.radialMenu1.Items.Add(lowercase);
    this.radialMenu1.Items.Add(titleCase);
}
```

**Result:**
Users can select one alignment option AND one text case option independently.

**When to Use Radio Button Groups:**
- Alignment options (left, center, right, justify)
- View modes (day, week, month)
- Sort orders (ascending, descending)
- Display sizes (small, medium, large)
- File types for export (PDF, Word, Excel)

## Building Hierarchical Menus

RadialMenu supports nested submenus, allowing you to create complex hierarchical navigation structures.

### Creating Submenu Items

Submenu items are created by adding RadialMenuItem instances to the parent item's Items collection.

**Basic Submenu Example:**

```csharp
private void CreateFileMenu()
{
    // Create parent item
    RadialMenuItem fileItem = new RadialMenuItem();
    fileItem.Text = "File";
    fileItem.ImageIndex = 0;

    // Create child items (submenu)
    RadialMenuItem newItem = new RadialMenuItem();
    newItem.Text = "New";
    newItem.Click += (s, e) => CreateNewDocument();

    RadialMenuItem openItem = new RadialMenuItem();
    openItem.Text = "Open";
    openItem.Click += (s, e) => OpenDocument();

    RadialMenuItem saveItem = new RadialMenuItem();
    saveItem.Text = "Save";
    saveItem.Click += (s, e) => SaveDocument();

    RadialMenuItem closeItem = new RadialMenuItem();
    closeItem.Text = "Close";
    closeItem.Click += (s, e) => CloseDocument();

    // Add children to parent's Items collection
    fileItem.Items.Add(newItem);
    fileItem.Items.Add(openItem);
    fileItem.Items.Add(saveItem);
    fileItem.Items.Add(closeItem);

    // Add parent to RadialMenu
    this.radialMenu1.Items.Add(fileItem);
}
```

**Result:**
Clicking "File" drills down to show a submenu with New, Open, Save, and Close options.

### Multi-Level Hierarchies

You can create multiple levels of nested menus.

```csharp
private void CreateEditMenuWithSubmenus()
{
    // Level 1: Main menu item
    RadialMenuItem editItem = new RadialMenuItem();
    editItem.Text = "Edit";

    // Level 2: Edit submenu items
    RadialMenuItem cutItem = new RadialMenuItem();
    cutItem.Text = "Cut";
    cutItem.Click += (s, e) => PerformCut();

    RadialMenuItem copyItem = new RadialMenuItem();
    copyItem.Text = "Copy";
    copyItem.Click += (s, e) => PerformCopy();

    RadialMenuItem pasteItem = new RadialMenuItem();
    pasteItem.Text = "Paste";

    // Level 3: Paste options submenu
    RadialMenuItem pasteTextItem = new RadialMenuItem();
    pasteTextItem.Text = "Paste Text";
    pasteTextItem.Click += (s, e) => PasteAsText();

    RadialMenuItem pasteFormattedItem = new RadialMenuItem();
    pasteFormattedItem.Text = "Paste Formatted";
    pasteFormattedItem.Click += (s, e) => PasteAsFormatted();

    RadialMenuItem pasteSpecialItem = new RadialMenuItem();
    pasteSpecialItem.Text = "Paste Special";
    pasteSpecialItem.Click += (s, e) => ShowPasteSpecialDialog();

    // Build Level 3 hierarchy
    pasteItem.Items.Add(pasteTextItem);
    pasteItem.Items.Add(pasteFormattedItem);
    pasteItem.Items.Add(pasteSpecialItem);

    // Build Level 2 hierarchy
    editItem.Items.Add(cutItem);
    editItem.Items.Add(copyItem);
    editItem.Items.Add(pasteItem);  // Has its own submenu

    // Add to main menu
    this.radialMenu1.Items.Add(editItem);
}
```

**Result:**
- Click "Edit" → shows Cut, Copy, Paste
- Click "Paste" → shows Paste Text, Paste Formatted, Paste Special

## Event Handling

Menu items expose a `Click` event that fires when the user selects an item.

### Basic Click Event

```csharp
RadialMenuItem item = new RadialMenuItem();
item.Text = "Action";
item.Click += Item_Click;

private void Item_Click(object sender, EventArgs e)
{
    RadialMenuItem clickedItem = sender as RadialMenuItem;
    MessageBox.Show($"Clicked: {clickedItem.Text}");
}
```

### Lambda Expression Event Handlers

```csharp
RadialMenuItem saveItem = new RadialMenuItem();
saveItem.Text = "Save";
saveItem.Click += (s, e) =>
{
    SaveDocument();
    statusLabel.Text = "Document saved successfully";
};
```

### Shared Event Handler for Multiple Items

```csharp
private void CreateMenuWithSharedHandler()
{
    string[] actions = { "Cut", "Copy", "Paste", "Delete" };
    
    foreach (string action in actions)
    {
        RadialMenuItem item = new RadialMenuItem();
        item.Text = action;
        item.Click += EditAction_Click;  // Same handler for all
        this.radialMenu1.Items.Add(item);
    }
}

private void EditAction_Click(object sender, EventArgs e)
{
    RadialMenuItem item = sender as RadialMenuItem;
    
    // Route to specific method based on item text
    switch (item.Text)
    {
        case "Cut":
            if (richTextBox1.SelectionLength > 0)
                richTextBox1.Cut();
            break;
        case "Copy":
            if (richTextBox1.SelectionLength > 0)
                richTextBox1.Copy();
            break;
        case "Paste":
            if (Clipboard.ContainsText())
                richTextBox1.Paste();
            break;
        case "Delete":
            if (richTextBox1.SelectionLength > 0)
                richTextBox1.SelectedText = "";
            break;
    }
}
```

### Accessing Checked State in Events

```csharp
RadialMenuItem wordWrapItem = new RadialMenuItem();
wordWrapItem.Text = "Word Wrap";
wordWrapItem.CheckMode = CheckMode.Check;
wordWrapItem.Click += (s, e) =>
{
    RadialMenuItem item = s as RadialMenuItem;
    
    // Toggle based on checked state
    richTextBox1.WordWrap = item.Checked;
    
    // Update UI based on state
    statusLabel.Text = item.Checked 
        ? "Word wrap enabled" 
        : "Word wrap disabled";
};
```

## Complete Examples

### Example 1: Text Editor Context Menu

```csharp
public class TextEditorForm : Form
{
    private RichTextBox editor;
    private RadialMenu contextMenu;

    private void InitializeTextEditorMenu()
    {
        this.contextMenu = new RadialMenu();
        this.contextMenu.Style = RadialMenuStyle.Office2016Colorful;
        this.contextMenu.Size = new Size(300, 300);
        this.contextMenu.Visible = true;
        this.contextMenu.MenuVisibility = false;  // Hidden by default

        // Basic editing commands
        AddMenuItem("Cut", CutText);
        AddMenuItem("Copy", CopyText);
        AddMenuItem("Paste", PasteText);

        // Formatting parent item with submenu
        RadialMenuItem formatItem = new RadialMenuItem();
        formatItem.Text = "Format";
        
        // Bold, Italic, Underline as checkboxes
        AddFormatOption(formatItem, "Bold", FontStyle.Bold);
        AddFormatOption(formatItem, "Italic", FontStyle.Italic);
        AddFormatOption(formatItem, "Underline", FontStyle.Underline);
        
        this.contextMenu.Items.Add(formatItem);

        // Show menu on right-click
        this.editor.MouseUp += (s, e) =>
        {
            if (e.Button == MouseButtons.Right)
            {
                this.contextMenu.Location = editor.PointToScreen(e.Location);
                this.contextMenu.MenuVisibility = true;
            }
        };

        this.Controls.Add(this.contextMenu);
    }

    private void AddMenuItem(string text, Action action)
    {
        RadialMenuItem item = new RadialMenuItem();
        item.Text = text;
        item.Click += (s, e) => action();
        this.contextMenu.Items.Add(item);
    }

    private void AddFormatOption(RadialMenuItem parent, string text, FontStyle style)
    {
        RadialMenuItem item = new RadialMenuItem();
        item.Text = text;
        item.CheckMode = CheckMode.Check;
        item.Click += (s, e) => ToggleFormatting(style, item.Checked);
        parent.Items.Add(item);
    }

    private void CutText() => editor.Cut();
    private void CopyText() => editor.Copy();
    private void PasteText() => editor.Paste();

    private void ToggleFormatting(FontStyle style, bool apply)
    {
        if (editor.SelectionLength == 0) return;

        Font currentFont = editor.SelectionFont ?? editor.Font;
        FontStyle newStyle = apply 
            ? currentFont.Style | style 
            : currentFont.Style & ~style;
        
        editor.SelectionFont = new Font(currentFont, newStyle);
    }
}
```

### Example 2: Graphics Editor Tool Menu

```csharp
private void CreateGraphicsToolMenu()
{
    this.radialMenu1.WedgeCount = 6;
    this.radialMenu1.Style = RadialMenuStyle.Office2016Colorful;

    // Tools parent item
    RadialMenuItem toolsItem = new RadialMenuItem();
    toolsItem.Text = "Tools";
    
    // Tool selection (radio buttons)
    string[] tools = { "Select", "Pen", "Brush", "Eraser", "Fill", "Text" };
    foreach (string tool in tools)
    {
        RadialMenuItem toolItem = new RadialMenuItem();
        toolItem.Text = tool;
        toolItem.CheckMode = CheckMode.Option;
        toolItem.GroupName = "tools";
        toolItem.Checked = (tool == "Select");  // Default tool
        toolItem.Click += Tool_Selected;
        toolsItem.Items.Add(toolItem);
    }

    // Brush size parent item
    RadialMenuItem sizeItem = new RadialMenuItem();
    sizeItem.Text = "Size";
    
    // Size options (radio buttons)
    int[] sizes = { 1, 3, 5, 10, 20 };
    foreach (int size in sizes)
    {
        RadialMenuItem sizeOption = new RadialMenuItem();
        sizeOption.Text = $"{size}px";
        sizeOption.CheckMode = CheckMode.Option;
        sizeOption.GroupName = "brushSize";
        sizeOption.Checked = (size == 3);  // Default size
        sizeOption.Tag = size;  // Store actual size value
        sizeOption.Click += Size_Selected;
        sizeItem.Items.Add(sizeOption);
    }

    this.radialMenu1.Items.Add(toolsItem);
    this.radialMenu1.Items.Add(sizeItem);
}

private void Tool_Selected(object sender, EventArgs e)
{
    RadialMenuItem item = sender as RadialMenuItem;
    currentTool = item.Text;
    statusLabel.Text = $"Current tool: {currentTool}";
}

private void Size_Selected(object sender, EventArgs e)
{
    RadialMenuItem item = sender as RadialMenuItem;
    brushSize = (int)item.Tag;
    statusLabel.Text = $"Brush size: {brushSize}px";
}
```

## Best Practices

**1. Keep Menu Depth Manageable**
- Limit to 2-3 levels of nesting
- Too many levels can confuse users
- Consider splitting into multiple menus if structure is too deep

**2. Group Related Items**
- Use GroupName for mutually exclusive options
- Place related commands in submenus
- Use separators (spacing) between different functional groups

**3. Provide Visual Feedback**
- Use CheckMode for toggleable options
- Show current state with Checked property
- Update UI immediately after selection

**4. Optimize Item Count**
- Set WedgeCount appropriately (4-8 items per level is ideal)
- Too many items create cluttered UI
- Use submenus to organize additional options

**5. Handle Events Consistently**
- Always check for null when casting sender
- Validate item state before taking action
- Provide user feedback (status messages, visual changes)

## Common Issues and Solutions

**Issue: Items don't respond to clicks**
```csharp
// Problem: Event handler not attached
RadialMenuItem item = new RadialMenuItem();
item.Text = "Action";
// Missing: item.Click += Handler;

// Solution: Always attach event handlers
item.Click += Item_Click;
```

**Issue: Radio buttons don't work as expected**
```csharp
// Problem: Forgot to set GroupName
item1.CheckMode = CheckMode.Option;  // Not enough
item2.CheckMode = CheckMode.Option;

// Solution: Set same GroupName for related items
item1.CheckMode = CheckMode.Option;
item1.GroupName = "group1";
item2.CheckMode = CheckMode.Option;
item2.GroupName = "group1";
```

**Issue: Submenu items not appearing**
```csharp
// Problem: Adding to wrong collection
RadialMenuItem parent = new RadialMenuItem();
RadialMenuItem child = new RadialMenuItem();
this.radialMenu1.Items.Add(child);  // Wrong! Added to menu, not parent

// Solution: Add to parent's Items collection
parent.Items.Add(child);
this.radialMenu1.Items.Add(parent);
```

**Issue: Too many items, some are hidden**
```csharp
// Problem: WedgeCount too small
this.radialMenu1.Items.Count = 10;
this.radialMenu1.WedgeCount = 6;  // Only 6 visible

// Solution: Increase WedgeCount or use submenus
this.radialMenu1.WedgeCount = 10;  // Show all items
// OR organize into submenus to reduce items per level
```

**Issue: Cannot detect checked state changes**
```csharp
// Problem: Not checking Checked property in event handler
private void Item_Click(object sender, EventArgs e)
{
    RadialMenuItem item = sender as RadialMenuItem;
    // Performing action without checking state
    DoSomething();
}

// Solution: Check Checked property for toggle behavior
private void Item_Click(object sender, EventArgs e)
{
    RadialMenuItem item = sender as RadialMenuItem;
    if (item.Checked)
        EnableFeature();
    else
        DisableFeature();
}
```
