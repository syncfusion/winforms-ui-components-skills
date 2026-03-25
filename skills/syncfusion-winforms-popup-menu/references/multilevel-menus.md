# Multi-Level Menu Items in PopupMenu

Multi-level menu items create hierarchical menu structures with nested submenus. ParentBarItem serves as the container type that enables unlimited depth menu hierarchies.

## Overview

Multi-level menus organize related commands into hierarchical structures (e.g., File → New → New Project). Each level can contain any BarItem type, including more ParentBarItems for additional nesting.

**Key Concepts:**
- **Root ParentBarItem:** The main container assigned to PopupMenu.ParentBarItem
- **Submenu ParentBarItem:** ParentBarItems added as items to other ParentBarItems
- **Leaf Items:** End-point BarItems that execute actions (BarItem, DropDownBarItem, etc.)
- **Unlimited Depth:** No limit to nesting levels (practical limit: 3-4 levels for usability)

## When to Use Multi-Level Menus

Use hierarchical structures when:
- Organizing many related commands into categories
- Creating standard menu patterns (File, Edit, View, etc.)
- Grouping specialized tools under parent categories
- Building complex application menu systems
- Implementing Windows Explorer-style context menus

## Creating Two-Level Menus

### Example: File Menu with Submenus

```csharp
// Root parent (assigned to PopupMenu)
ParentBarItem rootParent = new ParentBarItem();
rootParent.MetroColor = System.Drawing.Color.LightSkyBlue;
rootParent.SizeToFit = true;

// Top-level menu item (File)
ParentBarItem fileMenu = new ParentBarItem();
fileMenu.Text = "File";
fileMenu.SizeToFit = true;
fileMenu.MetroColor = System.Drawing.Color.LightSkyBlue;

// File submenu items
BarItem newItem = new BarItem();
newItem.Text = "New...";
newItem.Shortcut = Shortcut.CtrlN;
newItem.Click += NewItem_Click;

BarItem openItem = new BarItem();
openItem.Text = "Open...";
openItem.Shortcut = Shortcut.CtrlO;
openItem.Click += OpenItem_Click;

BarItem saveItem = new BarItem();
saveItem.Text = "Save";
saveItem.Shortcut = Shortcut.CtrlS;
saveItem.Click += SaveItem_Click;

// Build structure
fileMenu.Items.AddRange(new BarItem[] { newItem, openItem, saveItem });
rootParent.Items.Add(fileMenu);

// Assign to PopupMenu
popupMenu1.ParentBarItem = rootParent;
```

### VB.NET Example

```vb
' Root parent (assigned to PopupMenu)
Dim rootParent As New ParentBarItem()
rootParent.MetroColor = System.Drawing.Color.LightSkyBlue
rootParent.SizeToFit = True

' Top-level menu item (File)
Dim fileMenu As New ParentBarItem()
fileMenu.Text = "File"
fileMenu.SizeToFit = True
fileMenu.MetroColor = System.Drawing.Color.LightSkyBlue

' File submenu items
Dim newItem As New BarItem()
newItem.Text = "New..."
newItem.Shortcut = Shortcut.CtrlN
AddHandler newItem.Click, AddressOf NewItem_Click

Dim openItem As New BarItem()
openItem.Text = "Open..."
openItem.Shortcut = Shortcut.CtrlO
AddHandler openItem.Click, AddressOf OpenItem_Click

Dim saveItem As New BarItem()
saveItem.Text = "Save"
saveItem.Shortcut = Shortcut.CtrlS
AddHandler saveItem.Click, AddressOf SaveItem_Click

' Build structure
fileMenu.Items.AddRange(New BarItem() {newItem, openItem, saveItem})
rootParent.Items.Add(fileMenu)

' Assign to PopupMenu
popupMenu1.ParentBarItem = rootParent
```

## Creating Three-Level Menus

### Example: File → New → Project Types

```csharp
// Root parent
ParentBarItem rootParent = new ParentBarItem();
rootParent.SizeToFit = true;
rootParent.MetroColor = System.Drawing.Color.LightSkyBlue;

// Level 1: File menu
ParentBarItem fileMenu = new ParentBarItem();
fileMenu.Text = "File";
fileMenu.SizeToFit = true;
fileMenu.MetroColor = System.Drawing.Color.LightSkyBlue;

// Level 2: New submenu
ParentBarItem newSubmenu = new ParentBarItem();
newSubmenu.Text = "New";
newSubmenu.SizeToFit = true;
newSubmenu.MetroColor = System.Drawing.Color.LightSkyBlue;

// Level 3: New project types (leaf items)
BarItem newProject = new BarItem();
newProject.Text = "New Project...";
newProject.Click += NewProject_Click;

BarItem newWebsite = new BarItem();
newWebsite.Text = "New Website...";
newWebsite.Click += NewWebsite_Click;

BarItem newFile = new BarItem();
newFile.Text = "New File...";
newFile.Click += NewFile_Click;

// Build hierarchy: Level 3 → Level 2
newSubmenu.Items.AddRange(new BarItem[] { newProject, newWebsite, newFile });

// Level 2 → Level 1
fileMenu.Items.Add(newSubmenu);

// Add other File menu items
BarItem openItem = new BarItem { Text = "Open...", Shortcut = Shortcut.CtrlO };
BarItem saveItem = new BarItem { Text = "Save", Shortcut = Shortcut.CtrlS };
fileMenu.Items.AddRange(new BarItem[] { openItem, saveItem });

// Level 1 → Root
rootParent.Items.Add(fileMenu);

// Assign to PopupMenu
popupMenu1.ParentBarItem = rootParent;
```

## Complex Multi-Level Example

Complete application menu with File, Edit, and Tools menus:

```csharp
// Initialize all components
ParentBarItem rootParent = new ParentBarItem();
rootParent.SizeToFit = true;

// === FILE MENU ===
ParentBarItem fileMenu = new ParentBarItem { Text = "File", SizeToFit = true };

// File → New submenu
ParentBarItem fileNewSubmenu = new ParentBarItem { Text = "New", SizeToFit = true };
fileNewSubmenu.Items.AddRange(new BarItem[] {
    new BarItem { Text = "New Project...", Click += NewProject_Click },
    new BarItem { Text = "New File...", Click += NewFile_Click }
});

fileMenu.Items.Add(fileNewSubmenu);
fileMenu.Items.Add(new BarItem { Text = "Open...", Shortcut = Shortcut.CtrlO });
fileMenu.Items.Add(new BarItem { Text = "Save", Shortcut = Shortcut.CtrlS });
fileMenu.Items.Add(new BarItem { Text = "Exit", Click += Exit_Click });

// === EDIT MENU ===
ParentBarItem editMenu = new ParentBarItem { Text = "Edit", SizeToFit = true };
editMenu.Items.AddRange(new BarItem[] {
    new BarItem { Text = "Cut", Shortcut = Shortcut.CtrlX },
    new BarItem { Text = "Copy", Shortcut = Shortcut.CtrlC },
    new BarItem { Text = "Paste", Shortcut = Shortcut.CtrlV }
});

// Edit → Find submenu
ParentBarItem findSubmenu = new ParentBarItem { Text = "Find", SizeToFit = true };
findSubmenu.Items.AddRange(new BarItem[] {
    new BarItem { Text = "Find...", Shortcut = Shortcut.CtrlF },
    new BarItem { Text = "Find Next", Shortcut = Shortcut.F3 },
    new BarItem { Text = "Replace...", Shortcut = Shortcut.CtrlH }
});

editMenu.Items.Add(findSubmenu);

// === TOOLS MENU ===
ParentBarItem toolsMenu = new ParentBarItem { Text = "Tools", SizeToFit = true };

// Tools → Options submenu
ParentBarItem optionsSubmenu = new ParentBarItem { Text = "Options", SizeToFit = true };
optionsSubmenu.Items.AddRange(new BarItem[] {
    new BarItem { Text = "General...", Click += OptionsGeneral_Click },
    new BarItem { Text = "Editor...", Click += OptionsEditor_Click },
    new BarItem { Text = "Advanced...", Click += OptionsAdvanced_Click }
});

toolsMenu.Items.Add(optionsSubmenu);
toolsMenu.Items.Add(new BarItem { Text = "Customize...", Click += Customize_Click });

// Build root menu
rootParent.Items.AddRange(new BarItem[] { fileMenu, editMenu, toolsMenu });

// Assign to PopupMenu
popupMenu1.ParentBarItem = rootParent;
```

## Designer Approach for Multi-Level Menus

1. Add PopupMenu and default ParentBarItem
2. Open ParentBarItem → Items in Properties panel
3. Add first-level ParentBarItem (e.g., "File")
4. Select the "File" ParentBarItem in the collection
5. In its Properties: **Misc → Items** → Open nested Collection Editor
6. Add second-level items (BarItem or ParentBarItem)
7. Repeat step 5-6 for additional nesting levels

## Best Practices

### Limit Nesting Depth
- **Recommended:** 2-3 levels maximum
- **Acceptable:** 4 levels for specialized scenarios
- **Avoid:** 5+ levels (poor usability, hard to navigate)

### Logical Grouping
```csharp
// Good: Logical hierarchy
File → New → New Project
File → New → New File
File → Open
File → Save

// Poor: Flat structure
New Project
New File
Open File
Save File
```

### Consistent Structure
- Use same patterns across menus (File, Edit, View all at same level)
- Keep similar command depths consistent
- Group related commands under common parents

### Visual Indicators
- ParentBarItems automatically show arrow indicator (►)
- No need to add custom indicators
- Arrow direction adjusts based on available space

### Performance Considerations
- Create menu structure once (in constructor or Form_Load)
- Avoid recreating menu hierarchy on every popup
- Use BeforePopup event for dynamic changes, not structure recreation

## Dynamic Multi-Level Menus

### Adding Items Dynamically

```csharp
private void AddRecentFilesMenu()
{
    ParentBarItem fileMenu = FindMenuItem("File") as ParentBarItem;
    if (fileMenu == null) return;
    
    // Create Recent Files submenu
    ParentBarItem recentMenu = new ParentBarItem();
    recentMenu.Text = "Recent Files";
    recentMenu.SizeToFit = true;
    
    // Add recent file items from list
    foreach (string file in GetRecentFiles())
    {
        BarItem fileItem = new BarItem();
        fileItem.Text = Path.GetFileName(file);
        fileItem.Tag = file;  // Store full path
        fileItem.Click += RecentFile_Click;
        recentMenu.Items.Add(fileItem);
    }
    
    fileMenu.Items.Add(recentMenu);
}

private void RecentFile_Click(object sender, EventArgs e)
{
    BarItem item = sender as BarItem;
    if (item != null && item.Tag is string filePath)
    {
        OpenFile(filePath);
    }
}
```

### Removing Submenus

```csharp
private void RemoveSubmenu(ParentBarItem parent, string submenuText)
{
    BarItem itemToRemove = parent.Items.Cast<BarItem>()
        .FirstOrDefault(item => item.Text == submenuText);
    
    if (itemToRemove != null)
    {
        parent.Items.Remove(itemToRemove);
    }
}
```

## Common Patterns

### Windows Explorer-Style Menu

```csharp
// Root context menu
rootParent.Items.AddRange(new BarItem[] {
    new BarItem { Text = "Open" },
    new BarItem { Text = "Edit" },
    // Send To submenu
    new ParentBarItem {
        Text = "Send To",
        Items = {
            new BarItem { Text = "Desktop (create shortcut)" },
            new BarItem { Text = "Mail Recipient" },
            new BarItem { Text = "Compressed folder" }
        }
    },
    new BarItem { Text = "Cut" },
    new BarItem { Text = "Copy" },
    new BarItem { Text = "Delete" },
    new BarItem { Text = "Properties" }
});
```

### Application Menu Bar Structure

```csharp
rootParent.Items.AddRange(new BarItem[] {
    CreateFileMenu(),
    CreateEditMenu(),
    CreateViewMenu(),
    CreateToolsMenu(),
    CreateHelpMenu()
});

private ParentBarItem CreateFileMenu()
{
    ParentBarItem menu = new ParentBarItem { Text = "File", SizeToFit = true };
    // Add file menu items...
    return menu;
}
```

## Troubleshooting

**Issue: Submenu doesn't appear**
- Verify ParentBarItem.Items contains child items
- Check that child items have Text property set
- Ensure SizeToFit = true on ParentBarItem

**Issue: Menu hierarchy appears flat**
- Confirm you're adding ParentBarItem (not BarItem) for submenus
- Verify ParentBarItem is added to parent's Items collection
- Check that ParentBarItem has children in its Items collection

**Issue: Arrow indicator missing**
- Arrow appears only if ParentBarItem has child items
- Check ParentBarItem.Items.Count > 0
- Verify items aren't null or empty

**Issue: Deep nesting is slow**
- Consider flattening menu structure
- Use BeforePopup for lazy loading of deep hierarchies
- Cache menu structures instead of recreating
