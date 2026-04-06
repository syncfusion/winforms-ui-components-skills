# Multi-level Menus

Guide for creating hierarchical menu structures with submenus and nested navigation in ContextMenuStripEx.

## Overview

Multi-level menus (also called hierarchical or nested menus) allow you to organize related menu items in a tree structure. Parent menu items expand to reveal submenu items when clicked or hovered, enabling compact organization of large command sets.

**Common Use Cases:**
- File → New → Document/Project/File
- Format → Font → Size/Style/Color
- View → Toolbars → Standard/Formatting/Debug
- Window → Split → Horizontal/Vertical

## Key Concepts

**Parent Item:** A menu item that contains other menu items  
**Child Items (Submenus):** Menu items displayed when parent is clicked/hovered  
**DropDownItems:** The collection property that holds child items  
**Nesting Depth:** How many levels deep the hierarchy goes

## Creating Submenus

### The DropDownItems Property

The `DropDownItems` property of a ToolStripMenuItem contains its submenu items. This property is a collection that can hold any ToolStripItem type (MenuItem, TextBox, ComboBox, Separator).

**Key Points:**
- Any MenuItem can have DropDownItems (become a parent)
- DropDownItems is a collection, use `Add()` or `AddRange()`
- Parent items show an arrow indicator (►) automatically
- Submenus can contain any mix of item types

## Creating Multi-level Menus via Designer

### Step 1: Add Parent Menu Item

1. Click **"Type Here"** in the ContextMenuStripEx designer
2. Add a MenuItem (type text or select from dropdown)
3. The parent item is created

### Step 2: Add Child Items

**Method A: Type Here Interface**
1. After typing the parent item text, notice a **"Type Here"** appears to the right
2. Click this right-side **"Type Here"**
3. Add child menu items
4. Continue adding siblings or create deeper nesting

**Method B: DropDownItems Property**
1. Select the parent menu item in the designer
2. In Properties panel, locate **DropDownItems** property
3. Click the ellipsis (...) to open Items Collection Editor
4. Add child items using the Add dropdown
5. Configure each child's properties
6. Click OK

### Step 3: Create Deeper Nesting

To create multiple levels:
1. Select a child item (that should become a parent)
2. Use "Type Here" to its right or the DropDownItems property
3. Add grandchild items
4. Repeat for desired depth

## Creating Multi-level Menus via Code

### Basic Two-Level Menu

**C# Example:**
```csharp
using Syncfusion.Windows.Forms.Tools;
using System.Windows.Forms;

private void CreateTwoLevelMenu()
{
    var contextMenu = new ContextMenuStripEx();
    
    // Create parent menu item
    var fileMenu = new ToolStripMenuItem("File");
    
    // Create child menu items
    var newItem = new ToolStripMenuItem("New");
    var openItem = new ToolStripMenuItem("Open");
    var saveItem = new ToolStripMenuItem("Save");
    
    // Add children to parent
    fileMenu.DropDownItems.AddRange(new ToolStripItem[] {
        newItem,
        openItem,
        saveItem
    });
    
    // Add parent to context menu
    contextMenu.Items.Add(fileMenu);
    
    // Associate with control
    this.textBox1.ContextMenuStrip = contextMenu;
}
```

**VB.NET Example:**
```vb
Imports Syncfusion.Windows.Forms.Tools
Imports System.Windows.Forms

Private Sub CreateTwoLevelMenu()
    Dim contextMenu As New ContextMenuStripEx()
    
    ' Create parent menu item
    Dim fileMenu As New ToolStripMenuItem("File")
    
    ' Create child menu items
    Dim newItem As New ToolStripMenuItem("New")
    Dim openItem As New ToolStripMenuItem("Open")
    Dim saveItem As New ToolStripMenuItem("Save")
    
    ' Add children to parent
    fileMenu.DropDownItems.AddRange(New ToolStripItem() {
        newItem,
        openItem,
        saveItem
    })
    
    ' Add parent to context menu
    contextMenu.Items.Add(fileMenu)
    
    ' Associate with control
    Me.textBox1.ContextMenuStrip = contextMenu
End Sub
```

### Three-Level Menu (Nested Submenus)

**C# Example:**
```csharp
private void CreateThreeLevelMenu()
{
    var contextMenu = new ContextMenuStripEx();
    
    // Top-level item
    var newMenu = new ToolStripMenuItem("New");
    
    // Second-level items
    var newDocMenu = new ToolStripMenuItem("New Document");
    var newProjectItem = new ToolStripMenuItem("New Project");
    
    // Third-level items (grandchildren)
    var wordDocItem = new ToolStripMenuItem("Word Document");
    var pdfDocItem = new ToolStripMenuItem("PDF Document");
    var textDocItem = new ToolStripMenuItem("Text Document");
    
    // Build hierarchy: grandchildren → child
    newDocMenu.DropDownItems.AddRange(new ToolStripItem[] {
        wordDocItem,
        pdfDocItem,
        textDocItem
    });
    
    // Build hierarchy: children → parent
    newMenu.DropDownItems.AddRange(new ToolStripItem[] {
        newDocMenu,
        newProjectItem
    });
    
    // Add to context menu
    contextMenu.Items.Add(newMenu);
}
```

**VB.NET Example:**
```vb
Private Sub CreateThreeLevelMenu()
    Dim contextMenu As New ContextMenuStripEx()
    
    ' Top-level item
    Dim newMenu As New ToolStripMenuItem("New")
    
    ' Second-level items
    Dim newDocMenu As New ToolStripMenuItem("New Document")
    Dim newProjectItem As New ToolStripMenuItem("New Project")
    
    ' Third-level items (grandchildren)
    Dim wordDocItem As New ToolStripMenuItem("Word Document")
    Dim pdfDocItem As New ToolStripMenuItem("PDF Document")
    Dim textDocItem As New ToolStripMenuItem("Text Document")
    
    ' Build hierarchy: grandchildren → child
    newDocMenu.DropDownItems.AddRange(New ToolStripItem() {
        wordDocItem,
        pdfDocItem,
        textDocItem
    })
    
    ' Build hierarchy: children → parent
    newMenu.DropDownItems.AddRange(New ToolStripItem() {
        newDocMenu,
        newProjectItem
    })
    
    ' Add to context menu
    contextMenu.Items.Add(newMenu)
End Sub
```

## Complete Multi-level Example

This example shows a full context menu with multiple hierarchies and mixed item types:

**C# Example:**
```csharp
private void CreateCompleteHierarchy()
{
    var contextMenu = new ContextMenuStripEx();
    
    // File menu (with submenus)
    var fileMenu = new ToolStripMenuItem("File");
    
    var newMenu = new ToolStripMenuItem("New");
    var newDocItem = new ToolStripMenuItem("Document", null, (s, e) => CreateDocument());
    var newProjectItem = new ToolStripMenuItem("Project", null, (s, e) => CreateProject());
    var newFileItem = new ToolStripMenuItem("File", null, (s, e) => CreateFile());
    newMenu.DropDownItems.AddRange(new ToolStripItem[] { 
        newDocItem, newProjectItem, newFileItem 
    });
    
    var openItem = new ToolStripMenuItem("Open", null, (s, e) => OpenFile());
    var saveItem = new ToolStripMenuItem("Save", null, (s, e) => SaveFile());
    
    fileMenu.DropDownItems.AddRange(new ToolStripItem[] {
        newMenu,
        openItem,
        saveItem
    });
    
    // Edit menu (simple items)
    var editMenu = new ToolStripMenuItem("Edit");
    var cutItem = new ToolStripMenuItem("Cut", null, (s, e) => Cut());
    var copyItem = new ToolStripMenuItem("Copy", null, (s, e) => Copy());
    var pasteItem = new ToolStripMenuItem("Paste", null, (s, e) => Paste());
    
    editMenu.DropDownItems.AddRange(new ToolStripItem[] {
        cutItem, copyItem, pasteItem
    });
    
    // View menu (with mixed items and deep nesting)
    var viewMenu = new ToolStripMenuItem("View");
    
    var toolbarsMenu = new ToolStripMenuItem("Toolbars");
    var standardToolbar = new ToolStripMenuItem("Standard");
    standardToolbar.CheckOnClick = true;
    standardToolbar.Checked = true;
    
    var formattingToolbar = new ToolStripMenuItem("Formatting");
    formattingToolbar.CheckOnClick = true;
    
    var debugToolbar = new ToolStripMenuItem("Debug");
    debugToolbar.CheckOnClick = true;
    
    toolbarsMenu.DropDownItems.AddRange(new ToolStripItem[] {
        standardToolbar, formattingToolbar, debugToolbar
    });
    
    var zoomMenu = new ToolStripMenuItem("Zoom");
    var zoomCombo = new ToolStripComboBox();
    zoomCombo.Items.AddRange(new object[] { "50%", "75%", "100%", "125%", "150%", "200%" });
    zoomCombo.SelectedIndex = 2;  // 100%
    
    zoomMenu.DropDownItems.Add(zoomCombo);
    
    viewMenu.DropDownItems.AddRange(new ToolStripItem[] {
        toolbarsMenu,
        new ToolStripSeparator(),
        zoomMenu
    });
    
    // Add all top-level menus
    contextMenu.Items.AddRange(new ToolStripItem[] {
        fileMenu,
        editMenu,
        viewMenu
    });
    
    this.richTextBox1.ContextMenuStrip = contextMenu;
}
```

## Adding Event Handlers to Nested Items

Event handlers work the same for nested items as for top-level items:

**C# Example:**
```csharp
var parentItem = new ToolStripMenuItem("Format");

var fontMenu = new ToolStripMenuItem("Font");
var arial = new ToolStripMenuItem("Arial");
var timesNewRoman = new ToolStripMenuItem("Times New Roman");
var courier = new ToolStripMenuItem("Courier New");

// Attach handlers to nested items
arial.Click += (s, e) => SetFont("Arial");
timesNewRoman.Click += (s, e) => SetFont("Times New Roman");
courier.Click += (s, e) => SetFont("Courier New");

fontMenu.DropDownItems.AddRange(new ToolStripItem[] {
    arial, timesNewRoman, courier
});

parentItem.DropDownItems.Add(fontMenu);
```

## Dynamic Submenu Population

Populate submenus dynamically based on application state or user data:

**C# Example:**
```csharp
private void CreateDynamicMenu()
{
    var contextMenu = new ContextMenuStripEx();
    
    var recentFilesMenu = new ToolStripMenuItem("Recent Files");
    
    // Populate dynamically from list
    List<string> recentFiles = GetRecentFiles();  // Your method
    
    foreach (string filePath in recentFiles)
    {
        string fileName = System.IO.Path.GetFileName(filePath);
        var fileItem = new ToolStripMenuItem(fileName);
        fileItem.Tag = filePath;  // Store full path in Tag
        fileItem.Click += (s, e) => {
            var item = s as ToolStripMenuItem;
            string path = item.Tag as string;
            OpenFile(path);
        };
        
        recentFilesMenu.DropDownItems.Add(fileItem);
    }
    
    // Add "Clear Recent" option
    if (recentFiles.Count > 0)
    {
        recentFilesMenu.DropDownItems.Add(new ToolStripSeparator());
        var clearItem = new ToolStripMenuItem("Clear Recent Files");
        clearItem.Click += (s, e) => ClearRecentFiles();
        recentFilesMenu.DropDownItems.Add(clearItem);
    }
    
    contextMenu.Items.Add(recentFilesMenu);
}
```

## Submenu Opening Events

Respond when submenus open to update their contents or states:

**C# Example:**
```csharp
var fileMenu = new ToolStripMenuItem("File");
var recentMenu = new ToolStripMenuItem("Recent Files");

// Subscribe to DropDownOpening event
recentMenu.DropDownOpening += (s, e) => {
    var menu = s as ToolStripMenuItem;
    
    // Clear existing items
    menu.DropDownItems.Clear();
    
    // Repopulate with current data
    foreach (string file in GetRecentFiles())
    {
        menu.DropDownItems.Add(new ToolStripMenuItem(file));
    }
};

fileMenu.DropDownItems.Add(recentMenu);
```

## Best Practices

1. **Hierarchy Depth:** Keep to 2-3 levels maximum; 4-5 levels hurts usability
2. **Organization:** Group related items; limit 5-10 items per level (15 max)
3. **Performance:** Populate on DropDownOpening for large/dynamic submenus
4. **Accessibility:** Submenus work with arrow keys (→ expand, ← collapse)

## Common Patterns

### Pattern 1: File → New Menu

```csharp
var fileMenu = new ToolStripMenuItem("File");
var newMenu = new ToolStripMenuItem("New");

var newDoc = new ToolStripMenuItem("Document", null, (s, e) => CreateDocument());
var newProj = new ToolStripMenuItem("Project", null, (s, e) => CreateProject());

newMenu.DropDownItems.AddRange(new ToolStripItem[] { newDoc, newProj });
fileMenu.DropDownItems.Add(newMenu);
```

### Pattern 2: Format → Font → Properties

```csharp
var formatMenu = new ToolStripMenuItem("Format");
var fontMenu = new ToolStripMenuItem("Font");

var sizeMenu = new ToolStripMenuItem("Size");
foreach (int size in new[] { 8, 10, 12, 14, 16, 18, 20 })
{
    var sizeItem = new ToolStripMenuItem(size.ToString());
    sizeItem.Click += (s, e) => SetFontSize(size);
    sizeMenu.DropDownItems.Add(sizeItem);
}

var styleMenu = new ToolStripMenuItem("Style");
var boldItem = new ToolStripMenuItem("Bold");
boldItem.CheckOnClick = true;
var italicItem = new ToolStripMenuItem("Italic");
italicItem.CheckOnClick = true;
styleMenu.DropDownItems.AddRange(new ToolStripItem[] { boldItem, italicItem });

fontMenu.DropDownItems.AddRange(new ToolStripItem[] { sizeMenu, styleMenu });
formatMenu.DropDownItems.Add(fontMenu);
```

### Pattern 3: Cascading Filters

```csharp
var filterMenu = new ToolStripMenuItem("Filter");

var dateFilter = new ToolStripMenuItem("Date");
dateFilter.DropDownItems.AddRange(new ToolStripItem[] {
    new ToolStripMenuItem("Today", null, (s, e) => FilterByDate(FilterDate.Today)),
    new ToolStripMenuItem("This Week", null, (s, e) => FilterByDate(FilterDate.Week)),
    new ToolStripMenuItem("This Month", null, (s, e) => FilterByDate(FilterDate.Month))
});

var statusFilter = new ToolStripMenuItem("Status");
statusFilter.DropDownItems.AddRange(new ToolStripItem[] {
    new ToolStripMenuItem("Active", null, (s, e) => FilterByStatus("Active")),
    new ToolStripMenuItem("Completed", null, (s, e) => FilterByStatus("Completed")),
    new ToolStripMenuItem("Archived", null, (s, e) => FilterByStatus("Archived"))
});

filterMenu.DropDownItems.AddRange(new ToolStripItem[] { dateFilter, statusFilter });
```

## Troubleshooting

**Submenus not appearing:** Verify DropDownItems populated and parent enabled  
**Arrow (►) not showing:** Check items added to DropDownItems, not Items collection  
**Performance issues:** Use DropDownOpening to populate on-demand  
**Keyboard nav not working:** Ensure items enabled and no ShortcutKeys conflicts
