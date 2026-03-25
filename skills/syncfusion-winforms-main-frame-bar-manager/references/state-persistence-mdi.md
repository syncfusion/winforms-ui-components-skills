# State Persistence & MDI Integration

## Table of Contents
- [State Persistence Overview](#state-persistence-overview)
- [Enabling Serialization](#enabling-serialization)
- [MDI Concepts](#mdi-concepts)
- [Setting Up MDI Parent](#setting-up-mdi-parent)
- [MDI Child Forms](#mdi-child-forms)
- [Menu Merging](#menu-merging)
- [Merge Behavior Reference](#merge-behavior-reference)
- [Best Practices](#best-practices)

---

## State Persistence Overview

State persistence automatically saves and restores the state of menus, toolbars, and customizations between application sessions. Users can customize their toolbar layout, and those changes are preserved.

### Key Concepts

- **Toolbar Positions:** Docked location and size of toolbars
- **Menu Customization:** Added, removed, or reordered menu items
- **Visible/Hidden Items:** Which toolbars are visible
- **Application Preferences:** Custom menu configurations

---

## Enabling Serialization

### AutoLoadToolBarPositions

Automatically loads saved toolbar positions on application startup:

```csharp
mainFrameBarManager1.AutoLoadToolBarPositions = true;
```

**Default:** Enabled

**Effect:** Restores toolbars to positions from previous session

### AutoPersistCustomization

Automatically saves menu and toolbar customizations when users make changes:

```csharp
mainFrameBarManager1.AutoPersistCustomization = true;
```

**Default:** Enabled

**Effect:** User customizations automatically persist to disk

### Complete Setup

```csharp
// Initialize menu manager
MainFrameBarManager mainFrameBarManager1 = new MainFrameBarManager();
mainFrameBarManager1.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016Colorful;
mainFrameBarManager1.Form = this;

// Enable state persistence
mainFrameBarManager1.AutoLoadToolBarPositions = true;
mainFrameBarManager1.AutoPersistCustomization = true;
```

### Designer Configuration

In the Windows Forms designer:

1. Select MainFrameBarManager component
2. In Smart Tags menu, check:
   - ☑ **AutoLoadToolBarPositions**
   - ☑ **AutoPersistCustomization**
3. Properties automatically applied

### VB.NET Example

```vb
' Enable persistence
mainFrameBarManager1.AutoLoadToolBarPositions = True
mainFrameBarManager1.AutoPersistCustomization = True
```

---

## MDI Concepts

MDI (Multiple Document Interface) applications display multiple child windows within a parent window. The MainFrameBarManager framework provides automatic menu merging for MDI applications.

### MDI Pattern

- **MDI Parent:** Single main window (IsMdiContainer = true)
- **MDI Children:** Multiple document windows within parent
- **MainFrameBarManager:** Attached to parent form
- **ChildFrameBarManager:** Attached to each child form

### Menu Merging

When child windows are created, their menus automatically merge with the parent's menus:

```
PARENT: [File] [Edit] [View] [Window] [Help]
        New   Save    Zoom            About
        Open          Pan
```

When CHILD window is active:

```
MERGED: [File] [Edit] [View] [Window] [Help]
        New   Save    Zoom   Child1   About
        Open  Cut     Pan    Child2
        Save  Copy
```

The framework manages merging automatically.

---

## Setting Up MDI Parent

### Enable MDI Container

```csharp
public partial class MainForm : Form
{
    public MainForm()
    {
        InitializeComponent();
        
        // Enable MDI container
        this.IsMdiContainer = true;
    }
}
```

### Add MainFrameBarManager

```csharp
public MainForm()
{
    InitializeComponent();
    this.IsMdiContainer = true;
    
    // Create menu manager
    MainFrameBarManager mainFrameBarManager1 = new MainFrameBarManager();
    mainFrameBarManager1.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016Colorful;
    mainFrameBarManager1.Form = this;
    
    // Enable persistence
    mainFrameBarManager1.AutoLoadToolBarPositions = true;
    mainFrameBarManager1.AutoPersistCustomization = true;
    
    // Add File menu
    CreateFileMenu(mainFrameBarManager1);
}
```

### Create Parent Menus

```csharp
private void CreateFileMenu(MainFrameBarManager manager)
{
    Bar fileBar = new Bar() { BarName = "File", Caption = "File", Manager = manager };
    
    ParentBarItem fileMenu = new ParentBarItem() { Text = "&File" };
    
    BarItem newDocItem = new BarItem() { Text = "&New Document", Shortcut = Shortcut.CtrlN };
    BarItem openDocItem = new BarItem() { Text = "&Open", Shortcut = Shortcut.CtrlO };
    BarItem exitItem = new BarItem() { Text = "E&xit", Shortcut = Shortcut.AltF4 };
    
    fileMenu.Items.AddRange(new BarItem[] { newDocItem, openDocItem, exitItem });
    
    manager.Items.AddRange(new BarItem[] { fileMenu, newDocItem, openDocItem, exitItem });
    fileBar.Items.Add(fileMenu);
    manager.Bars.Add(fileBar);
    
    // Event handlers
    newDocItem.ItemClick += (s, e) => OpenNewChildForm();
    openDocItem.ItemClick += (s, e) => OpenExistingDocument();
    exitItem.ItemClick += (s, e) => this.Close();
}
```

---

## MDI Child Forms

### Create Child Form with Menu

```csharp
public partial class ChildForm : Form
{
    private ChildFrameBarManager childFrameBarManager1;
    
    public ChildForm()
    {
        InitializeComponent();
        
        // Create child menu manager
        childFrameBarManager1 = new ChildFrameBarManager();
        
        // Associate with parent MainFrameBarManager
        // (set parent reference if needed)
        
        CreateChildMenus();
    }
    
    private void CreateChildMenus()
    {
        // Create Edit menu specific to this child
        Bar editBar = new Bar() 
        { 
            BarName = "Edit", 
            Caption = "Edit", 
            Manager = childFrameBarManager1 
        };
        
        ParentBarItem editMenu = new ParentBarItem() { Text = "&Edit" };
        
        BarItem cutItem = new BarItem() { Text = "Cu&t", Shortcut = Shortcut.CtrlX };
        BarItem copyItem = new BarItem() { Text = "&Copy", Shortcut = Shortcut.CtrlC };
        BarItem pasteItem = new BarItem() { Text = "&Paste", Shortcut = Shortcut.CtrlV };
        
        editMenu.Items.AddRange(new BarItem[] { cutItem, copyItem, pasteItem });
        
        childFrameBarManager1.Items.AddRange(new BarItem[] { editMenu, cutItem, copyItem, pasteItem });
        editBar.Items.Add(editMenu);
        childFrameBarManager1.Bars.Add(editBar);
    }
}
```

### Opening Child Forms

```csharp
private void OpenNewChildForm()
{
    ChildForm childForm = new ChildForm();
    childForm.MdiParent = this;  // Important: Set parent
    childForm.Text = "Document " + DateTime.Now.Ticks;
    childForm.Show();
}
```

---

## Menu Merging

### Auto Merging (Default)

The framework automatically merges menus when child windows are created:

```csharp
// Auto merging happens automatically - no code needed
// When child is created: childForm.MdiParent = this;
// Menus merge automatically
// When child is closed: Menus revert
```

**Benefits:**
- Child menus appear when child is active
- Seamless switching between children
- Automatic cleanup when child closes

### Explicit Merging

For better performance or VS.NET-like behavior, explicitly register child types:

```csharp
public MainForm()
{
    InitializeComponent();
    this.IsMdiContainer = true;
    
    // Create menu manager...
    MainFrameBarManager mainFrameBarManager1 = new MainFrameBarManager();
    
    // Register child types for explicit merging
    mainFrameBarManager1.RegisterMdiChildTypes(new Type[] 
    { 
        typeof(TextEditorForm), 
        typeof(ImageEditorForm) 
    });
}
```

**Requirements:**
- Child form types must have public default constructor
- Framework creates dummy instances to extract menus
- Merged state visible in customization UI immediately

**Benefits:**
- Better performance (menus pre-merged)
- Consistent with Visual Studio behavior
- Predictable merge state

---

## Merge Behavior Reference

Menu merging in MDI scenarios is controlled by the **MergeType** property on BarItems.

### MergeType Values

| Value | Behavior |
|-------|----------|
| **Add** | Item added to parent menu without merging |
| **MergeItems** | Sub-menus merged; items with same text combine |
| **Replace** | Child item replaces parent item |
| **Remove** | Item hidden when merging occurs |

### Merge Behavior Matrix

When parent has MergeType=X and child has MergeType=Y:

| Parent | Child | Result |
|--------|-------|--------|
| Add | Add | No merging |
| Add | MergeItems | No merging |
| Add | Replace | No merging |
| Add | Remove | Parent stays, child hidden |
| MergeItems | Add | No merging |
| MergeItems | MergeItems | Sub-menus merge if both are parents |
| MergeItems | Replace | Child replaces parent |
| MergeItems | Remove | Both hidden |
| Replace | Add | No merging |
| Replace | MergeItems | Parent replaces child |
| Replace | Replace | Child replaces parent |
| Replace | Remove | Parent stays, child hidden |
| Remove | Add | Child replaces parent |
| Remove | MergeItems | Child replaces parent |
| Remove | Replace | Child replaces parent |
| Remove | Remove | Both hidden |

### Setting Merge Type

```csharp
// In parent form
BarItem editMenu = new BarItem() { Text = "&Edit" };
editMenu.MergeType = MergeType.MergeItems;  // Merge with child items

// In child form
BarItem editMenu = new BarItem() { Text = "&Edit" };
editMenu.MergeType = MergeType.MergeItems;  // Merge with parent items
```

### Merge Order

Control merge position with **MergeOrder** property:

```csharp
// Child menu item position in merged menu
BarItem childItem = new BarItem() { Text = "Child Item" };
childItem.MergeOrder = 2;  // Appears after items with MergeOrder < 2

// Parent items
BarItem parentItem1 = new BarItem() { Text = "Parent Item 1", MergeOrder = 1 };
BarItem parentItem2 = new BarItem() { Text = "Parent Item 2", MergeOrder = 3 };

// Merged order: Parent 1, Child, Parent 2
```

---

## Best Practices

### 1. Use Auto Merging for Simple Cases

```csharp
// Simple: Just set MdiParent
ChildForm child = new ChildForm();
child.MdiParent = this;
child.Show();
// Merging handled automatically
```

### 2. Use Explicit Merging for Complex Applications

```csharp
// Explicit: Pre-register child types
mainFrameBarManager1.RegisterMdiChildTypes(new Type[] 
{ 
    typeof(TextEditorForm), 
    typeof(ImageEditorForm),
    typeof(SettingsForm)
});
```

### 3. Consistent Merge Types

```csharp
// Parent: MergeItems for sub-menu merging
BarItem editMenu = new BarItem() { Text = "&Edit" };
editMenu.MergeType = MergeType.MergeItems;

// Child: MergeItems to merge into parent
BarItem childEditMenu = new BarItem() { Text = "&Edit" };
childEditMenu.MergeType = MergeType.MergeItems;
```

### 4. Test Menu Merging

Verify menu merging works correctly:
1. Open multiple child windows
2. Switch between windows
3. Verify menus update
4. Close window and verify revert

### 5. Enable Persistence

```csharp
// Always enable for better UX
mainFrameBarManager1.AutoLoadToolBarPositions = true;
mainFrameBarManager1.AutoPersistCustomization = true;
```

### 6. Document Merge Strategy

Add comments explaining merge behavior:

```csharp
// Parent Edit menu merges with child items
// Child Form must have Edit menu with MergeType.MergeItems
// Merged result shows parent + child items
```

### 7. Handle Window List

Use Window menu to show open documents:

```csharp
BarItem windowMenu = new BarItem() { Text = "&Window" };
windowMenu.MergeType = MergeType.Add;  // Shows all open windows

// Framework automatically manages this
```

---

## Complete MDI Example

```csharp
// Main form setup
public partial class MainForm : Form
{
    public MainForm()
    {
        InitializeComponent();
        this.IsMdiContainer = true;
        
        // Setup menu
        MainFrameBarManager mgr = new MainFrameBarManager();
        mgr.Form = this;
        mgr.AutoLoadToolBarPositions = true;
        mgr.AutoPersistCustomization = true;
        
        // Register child types
        mgr.RegisterMdiChildTypes(new Type[] { typeof(ChildForm) });
        
        // Create menus
        CreateFileMenu(mgr);
        CreateWindowMenu(mgr);
    }
    
    private void NewChildForm()
    {
        ChildForm child = new ChildForm();
        child.MdiParent = this;
        child.Show();
    }
}

// Child form setup
public partial class ChildForm : Form
{
    public ChildForm()
    {
        InitializeComponent();
        
        // Setup child menu
        ChildFrameBarManager mgr = new ChildFrameBarManager();
        CreateEditMenu(mgr);
    }
}
```

This pattern provides:
- Automatic menu merging
- State persistence
- Clean separation of parent/child menus
- Professional MDI behavior
