# Grouping BarItems in PopupMenu

Grouping adds visual separators between related menu items, improving organization and readability. PopupMenu provides multiple methods for creating and managing separator lines between BarItems.

## Overview

Separators visually divide menu items into logical groups. They appear as horizontal lines between items, helping users quickly identify related commands.

**Grouping Methods:**
1. **SeparatorIndices** - Collection-based approach using item indices
2. **BeginGroupAt()** - Method to add separator before specific item
3. **RemoveGroupAt()** - Method to remove separator before specific item
4. **IsGroupBeginning()** - Method to check if item starts a group

## When to Use Grouping

Use separators to:
- Divide menu items by functionality (Edit operations vs. Navigation)
- Separate destructive actions (Delete, Remove) from safe operations
- Group related commands (Font, Size, Style in one group)
- Indicate menu sections (Recent Files, All Files)
- Improve visual hierarchy and scanability

## Using SeparatorIndices Property

The `SeparatorIndices` collection specifies indices where separators appear. Separators are drawn **after** the item at the specified index.

### Basic Example

```csharp
ParentBarItem parentBarItem1 = new ParentBarItem();
parentBarItem1.SizeToFit = true;

// Add menu items
BarItem cut = new BarItem { Text = "Cut" };
BarItem copy = new BarItem { Text = "Copy" };
BarItem paste = new BarItem { Text = "Paste" };
BarItem selectAll = new BarItem { Text = "Select All" };

parentBarItem1.Items.AddRange(new BarItem[] { cut, copy, paste, selectAll });

// Add separator after index 2 (after Paste)
// Result: Cut, Copy, Paste | Select All
parentBarItem1.SeparatorIndices.Add(2);
```

### VB.NET Example

```vb
Dim parentBarItem1 As New ParentBarItem()
parentBarItem1.SizeToFit = True

' Add menu items
Dim cut As New BarItem With {.Text = "Cut"}
Dim copy As New BarItem With {.Text = "Copy"}
Dim paste As New BarItem With {.Text = "Paste"}
Dim selectAll As New BarItem With {.Text = "Select All"}

parentBarItem1.Items.AddRange(New BarItem() {cut, copy, paste, selectAll})

' Add separator after index 2 (after Paste)
parentBarItem1.SeparatorIndices.Add(2)
```

### Multiple Separators

```csharp
// Menu structure:
// New, Open, Save | Cut, Copy, Paste | Options, Exit

BarItem newItem = new BarItem { Text = "New" };
BarItem openItem = new BarItem { Text = "Open" };
BarItem saveItem = new BarItem { Text = "Save" };
BarItem cutItem = new BarItem { Text = "Cut" };
BarItem copyItem = new BarItem { Text = "Copy" };
BarItem pasteItem = new BarItem { Text = "Paste" };
BarItem optionsItem = new BarItem { Text = "Options" };
BarItem exitItem = new BarItem { Text = "Exit" };

parentBarItem1.Items.AddRange(new BarItem[] {
    newItem, openItem, saveItem,      // Indices 0-2
    cutItem, copyItem, pasteItem,      // Indices 3-5
    optionsItem, exitItem              // Indices 6-7
});

// Add separators after indices 2 and 5
parentBarItem1.SeparatorIndices.AddRange(new int[] { 2, 5 });
```

### Understanding Indices

```csharp
// Items array:
// Index 0: Cut
// Index 1: Copy
// Index 2: Paste
// Index 3: Select All

// SeparatorIndices.Add(1) → Cut, Copy | Paste, Select All
// SeparatorIndices.Add(2) → Cut, Copy, Paste | Select All
// SeparatorIndices.Add(0) → Cut | Copy, Paste, Select All
```

## Using BeginGroupAt() Method

`BeginGroupAt()` adds a separator **immediately before** the specified BarItem. This approach is more readable when working with item references rather than indices.

### Method Signature

```csharp
void BeginGroupAt(BarItem item)
```

### Basic Example

```csharp
BarItem cut = new BarItem { Text = "Cut" };
BarItem copy = new BarItem { Text = "Copy" };
BarItem paste = new BarItem { Text = "Paste" };
BarItem selectAll = new BarItem { Text = "Select All" };

parentBarItem1.Items.AddRange(new BarItem[] { cut, copy, paste, selectAll });

// Add separator before selectAll
// Result: Cut, Copy, Paste | Select All
parentBarItem1.BeginGroupAt(selectAll);
```

### VB.NET Example

```vb
Dim cut As New BarItem With {.Text = "Cut"}
Dim copy As New BarItem With {.Text = "Copy"}
Dim paste As New BarItem With {.Text = "Paste"}
Dim selectAll As New BarItem With {.Text = "Select All"}

parentBarItem1.Items.AddRange(New BarItem() {cut, copy, paste, selectAll})

' Add separator before selectAll
parentBarItem1.BeginGroupAt(selectAll)
```

### Multiple Groups with BeginGroupAt

```csharp
// File operations
BarItem newItem = new BarItem { Text = "New" };
BarItem openItem = new BarItem { Text = "Open" };
BarItem saveItem = new BarItem { Text = "Save" };

// Edit operations
BarItem cutItem = new BarItem { Text = "Cut" };
BarItem copyItem = new BarItem { Text = "Copy" };
BarItem pasteItem = new BarItem { Text = "Paste" };

// Application actions
BarItem optionsItem = new BarItem { Text = "Options" };
BarItem exitItem = new BarItem { Text = "Exit" };

parentBarItem1.Items.AddRange(new BarItem[] {
    newItem, openItem, saveItem,
    cutItem, copyItem, pasteItem,
    optionsItem, exitItem
});

// Add separators before cutItem and optionsItem
parentBarItem1.BeginGroupAt(cutItem);
parentBarItem1.BeginGroupAt(optionsItem);
```

## Using RemoveGroupAt() Method

`RemoveGroupAt()` removes the separator **immediately before** the specified BarItem.

### Method Signature

```csharp
void RemoveGroupAt(BarItem item)
```

### Example

```csharp
// Setup with separators
parentBarItem1.BeginGroupAt(cutItem);
parentBarItem1.BeginGroupAt(optionsItem);

// Later, remove separator before optionsItem
parentBarItem1.RemoveGroupAt(optionsItem);

// Or remove by index
parentBarItem1.SeparatorIndices.Remove(2);  // Remove separator at index 2
```

### VB.NET Example

```vb
' Setup with separators
parentBarItem1.BeginGroupAt(cutItem)
parentBarItem1.BeginGroupAt(optionsItem)

' Later, remove separator before optionsItem
parentBarItem1.RemoveGroupAt(optionsItem)
```

## Using IsGroupBeginning() Method

`IsGroupBeginning()` checks whether a separator exists **immediately before** the specified BarItem.

### Method Signature

```csharp
bool IsGroupBeginning(BarItem item)
```

### Example

```csharp
BarItem selectAll = new BarItem { Text = "Select All" };
parentBarItem1.Items.Add(selectAll);
parentBarItem1.BeginGroupAt(selectAll);

// Check if selectAll starts a group
if (parentBarItem1.IsGroupBeginning(selectAll))
{
    Console.WriteLine("Select All starts a new group");
}
else
{
    Console.WriteLine("Select All does not start a new group");
}
```

### VB.NET Example

```vb
Dim selectAll As New BarItem With {.Text = "Select All"}
parentBarItem1.Items.Add(selectAll)
parentBarItem1.BeginGroupAt(selectAll)

' Check if selectAll starts a group
If parentBarItem1.IsGroupBeginning(selectAll) Then
    Console.WriteLine("Select All starts a new group")
Else
    Console.WriteLine("Select All does not start a new group")
End If
```

## Common Patterns

### Standard Edit Menu Grouping

```csharp
// Group 1: Clipboard operations
BarItem cut = new BarItem { Text = "Cut", Shortcut = Shortcut.CtrlX };
BarItem copy = new BarItem { Text = "Copy", Shortcut = Shortcut.CtrlC };
BarItem paste = new BarItem { Text = "Paste", Shortcut = Shortcut.CtrlV };

// Group 2: Selection operations
BarItem selectAll = new BarItem { Text = "Select All", Shortcut = Shortcut.CtrlA };
BarItem find = new BarItem { Text = "Find...", Shortcut = Shortcut.CtrlF };

// Group 3: Advanced operations
BarItem replace = new BarItem { Text = "Replace...", Shortcut = Shortcut.CtrlH };

parentBarItem1.Items.AddRange(new BarItem[] {
    cut, copy, paste,
    selectAll, find,
    replace
});

// Add separators
parentBarItem1.BeginGroupAt(selectAll);
parentBarItem1.BeginGroupAt(replace);
```

### File Menu with Recent Files

```csharp
BarItem newItem = new BarItem { Text = "New" };
BarItem openItem = new BarItem { Text = "Open" };
BarItem saveItem = new BarItem { Text = "Save" };

// Recent files section
StaticBarItem recentHeader = new StaticBarItem { Text = "Recent Files" };
BarItem recent1 = new BarItem { Text = "Document1.txt" };
BarItem recent2 = new BarItem { Text = "Report.docx" };

// Exit section
BarItem exitItem = new BarItem { Text = "Exit" };

parentBarItem1.Items.AddRange(new BarItem[] {
    newItem, openItem, saveItem,
    recentHeader, recent1, recent2,
    exitItem
});

parentBarItem1.BeginGroupAt(recentHeader);
parentBarItem1.BeginGroupAt(exitItem);
```

### Context Menu with Destructive Actions

```csharp
// Safe operations
BarItem openItem = new BarItem { Text = "Open" };
BarItem copyItem = new BarItem { Text = "Copy" };
BarItem renameItem = new BarItem { Text = "Rename" };

// Destructive operations (separated for safety)
BarItem deleteItem = new BarItem { Text = "Delete" };

// Properties (separated as utility action)
BarItem propertiesItem = new BarItem { Text = "Properties" };

parentBarItem1.Items.AddRange(new BarItem[] {
    openItem, copyItem, renameItem,
    deleteItem,
    propertiesItem
});

// Separate destructive and utility actions
parentBarItem1.BeginGroupAt(deleteItem);
parentBarItem1.BeginGroupAt(propertiesItem);
```

## Dynamic Grouping

### Adding Separators at Runtime

```csharp
private void AddDynamicGroup(ParentBarItem parent, BarItem[] items, bool addSeparatorBefore)
{
    if (addSeparatorBefore && parent.Items.Count > 0)
    {
        // Add separator before first new item
        parent.BeginGroupAt(items[0]);
    }
    
    parent.Items.AddRange(items);
}

// Usage
BarItem[] recentFileItems = GetRecentFileItems();
AddDynamicGroup(fileMenuParent, recentFileItems, addSeparatorBefore: true);
```

### Conditional Grouping

```csharp
private void SetupContextMenu(bool includeAdvanced)
{
    // Basic items
    BarItem cut = new BarItem { Text = "Cut" };
    BarItem copy = new BarItem { Text = "Copy" };
    BarItem paste = new BarItem { Text = "Paste" };
    
    parentBarItem1.Items.AddRange(new BarItem[] { cut, copy, paste });
    
    if (includeAdvanced)
    {
        // Advanced items
        BarItem advancedEdit = new BarItem { Text = "Advanced Edit..." };
        BarItem formatOptions = new BarItem { Text = "Format Options..." };
        
        // Add with separator
        parentBarItem1.BeginGroupAt(advancedEdit);
        parentBarItem1.Items.AddRange(new BarItem[] { advancedEdit, formatOptions });
    }
}
```

## Best Practices

### Logical Grouping
- Group by functionality, not alphabetically
- Keep related commands together
- Separate destructive actions from safe operations

### Separator Frequency
- **Good:** 2-4 groups per menu
- **Acceptable:** 5-6 groups for complex menus
- **Avoid:** Too many groups (creates visual clutter)

### Group Size
- **Ideal:** 3-5 items per group
- **Acceptable:** 2-7 items per group
- **Avoid:** Single-item groups (wastes space)

### Consistency
- Use consistent grouping patterns across similar menus
- Match Windows standard menu grouping where applicable
- Keep group order logical (frequently used first)

## Troubleshooting

**Issue: Separator doesn't appear**
- Verify index is valid (0 to Items.Count - 1)
- Check that BeginGroupAt() is called with valid BarItem reference
- Ensure item is actually in Items collection

**Issue: Separator in wrong position**
- SeparatorIndices: Separator appears AFTER the index
- BeginGroupAt(): Separator appears BEFORE the item
- Recount indices if items were added/removed

**Issue: Cannot remove separator**
- Use RemoveGroupAt() with correct item reference
- Or use SeparatorIndices.Remove(index) with correct index
- Check that separator actually exists before removing
