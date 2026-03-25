---
name: syncfusion-winforms-popup-menu
description: Guide for implementing Syncfusion PopupMenu control for context menus in Windows Forms applications. Use this when creating right-click menus, multi-level menu hierarchies, or keyboard shortcuts. Covers PopupMenu implementation with BarItems, PopupMenusManager integration, and Bar Manager association for building professional context menu systems.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing PopupMenu in Windows Forms

This skill provides comprehensive guidance for implementing the Syncfusion PopupMenu control in Windows Forms applications. PopupMenu represents a context menu that appears on user interaction, supporting multiple BarItem types, multi-level hierarchies, keyboard navigation, and integration with Bar Manager.

## When to Use This Skill

Use this skill when the user:
- Needs to implement context menus or right-click menus in WinForms
- Wants to create popup menus with various item types (buttons, dropdowns, comboboxes, lists)
- Needs multi-level menu structures with nested submenus
- Wants keyboard shortcuts, mnemonics, or touch support for menus
- Needs to group menu items with separators
- Wants checked/unchecked states or disabled items in menus
- Needs to display images and tooltips in menu items
- Wants to integrate popup menus with Bar Manager or CommandBar
- Needs partial menus with frequently used items prioritized
- Is building context-sensitive menu systems for desktop applications

## Component Overview

**PopupMenu** is a context menu control that remains hidden by default and displays over any control when triggered (typically by right-click). It supports rich menu structures with seven types of BarItems, multi-level nesting, visual customization, and comprehensive keyboard/touch interaction.

**Key Features:**
- **7 BarItem Types:** BarItem, ParentBarItem, DropDownBarItem, ComboBoxBarItem, ListBarItem, StaticBarItem, TextBoxBarItem
- **Multi-Level Menus:** Nested submenu structures with unlimited depth
- **Grouping:** Separators between related menu items
- **States:** Checked/unchecked, enabled/disabled support
- **Visual Customization:** Images (normal, disabled, highlighted), tooltips
- **Keyboard Support:** Shortcuts, mnemonics, keyboard navigation
- **Touch Support:** Touch-enabled by default for modern devices
- **Partial Menus:** Prioritize frequently used items, hide others
- **Integration:** Bar Manager, CommandBar, PopupMenusManager
- **Events:** BeforePopup, Popup, Collapse, ParentBarItemChanged

**Core Components:**
- **PopupMenu:** Main control that hosts the menu structure
- **ParentBarItem:** Root container that holds all menu items
- **PopupMenusManager:** Associates popup menus with controls
- **BarItems:** Individual menu items (7 types available)

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly references and dependencies
- Adding PopupMenu through designer (drag-drop from toolbox)
- Adding PopupMenu through code (initialization and setup)
- PopupMenusManager configuration
- Associating popup menus with controls (RichTextBox, Button, etc.)
- Adding default ParentBarItem
- Creating basic BarItems with text and images
- NuGet package installation

### BarItem Types and Configuration
📄 **Read:** [references/baritem-types.md](references/baritem-types.md)
- **BarItem:** Standard menu item with text, image, events
- **ParentBarItem:** Container for submenu items (hierarchical menus)
- **DropDownBarItem:** Dropdown with custom PopupControlContainer (ColorPicker, etc.)
- **ComboBoxBarItem:** Editable combo box in menu
- **ListBarItem:** List of child items in menu
- **StaticBarItem:** Label/static text for adjacent items
- **TextBoxBarItem:** Text input field in menu
- Designer and code examples for each type
- When to use each BarItem type

### Multi-Level Menus and Grouping
📄 **Read:** [references/multilevel-menus.md](references/multilevel-menus.md)
- Creating nested menu structures
- Adding ParentBarItem as submenu containers
- Building hierarchical menus (File → New → New Project)
- Unlimited depth menu hierarchies
- Complex menu examples with multiple levels

📄 **Read:** [references/grouping-baritems.md](references/grouping-baritems.md)
- Adding separators between menu items
- SeparatorIndices property for grouping
- BeginGroupAt() method for programmatic grouping
- RemoveGroupAt() method to remove separators
- IsGroupBeginning() to check grouping state
- Visual organization patterns

### Visual Customization
📄 **Read:** [references/baritem-states.md](references/baritem-states.md)
- Checked/unchecked states (Checked property)
- Toggle behavior with Click events
- Enabled/disabled states (Enabled property)
- State management patterns
- OverlapCheckBoxImageBounds (checkbox/image overlap control)
- When to use checked states vs other indicators

📄 **Read:** [references/images-and-tooltips.md](references/images-and-tooltips.md)
- Adding images to BarItems (Image property)
- ImageExt class for image loading
- DisabledImage for disabled state
- HighlightedImage for hover/selection state
- Tooltip configuration (Tooltip property)
- ShowToolTipInPopUp to enable tooltips
- Icon guidelines and best practices

### Keyboard and Interaction
📄 **Read:** [references/keyboard-interaction.md](references/keyboard-interaction.md)
- Touch support (enabled by default)
- Keyboard shortcuts (Shortcut property, e.g., Ctrl+F)
- Custom shortcut text (ShortcutText property)
- Keyboard mnemonics (&Text, e.g., "&File")
- ShowMnemonicUnderlinesAlways for visible mnemonics
- Click event handling for menu item selection
- Triggering actions via keyboard vs mouse

### Advanced Integration
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- Partial menus (UsePartialMenus property)
- IsRecentlyUsedItem for usage prioritization
- Bar Manager integration for application-wide menus
- CommandBar association (PopupMenu as dropdown)
- CommandBarController setup
- Events: BeforePopup, Popup, Collapse, ParentBarItemChanged
- Event-driven menu customization

📄 **Read:** [references/troubleshooting.md](references/troubleshooting.md)
- Common setup issues
- Designer vs code differences
- Assembly reference problems
- PopupMenusManager configuration issues
- BarItem visibility problems
- Event handling troubleshooting

## Quick Start Example

### Basic PopupMenu with BarItems

```csharp
using Syncfusion.Windows.Forms.Tools.XPMenus;
using System.Windows.Forms;

public partial class Form1 : Form
{
    private PopupMenu popupMenu1;
    private ParentBarItem parentBarItem1;
    private BarItem barItem1;
    private BarItem barItem2;
    private BarItem barItem3;
    private PopupMenusManager popupMenusManager1;
    private RichTextBox richTextBox1;

    public Form1()
    {
        InitializeComponent();
        
        // Initialize components
        this.popupMenu1 = new PopupMenu(this.components);
        this.parentBarItem1 = new ParentBarItem();
        this.barItem1 = new BarItem();
        this.barItem2 = new BarItem();
        this.barItem3 = new BarItem();
        this.richTextBox1 = new RichTextBox();
        this.popupMenusManager1 = new PopupMenusManager(this.components);
        
        // Configure popup menu
        this.popupMenu1.ParentBarItem = this.parentBarItem1;
        
        // Configure BarItems
        this.barItem1.Text = "Cut";
        this.barItem1.Shortcut = Shortcut.CtrlX;
        this.barItem1.Click += BarItem1_Click;
        
        this.barItem2.Text = "Copy";
        this.barItem2.Shortcut = Shortcut.CtrlC;
        this.barItem2.Click += BarItem2_Click;
        
        this.barItem3.Text = "Paste";
        this.barItem3.Shortcut = Shortcut.CtrlV;
        this.barItem3.Click += BarItem3_Click;
        
        // Add items to parent
        this.parentBarItem1.Items.AddRange(new BarItem[] { 
            this.barItem1, this.barItem2, this.barItem3 
        });
        this.parentBarItem1.SizeToFit = true;
        
        // Associate with control
        this.richTextBox1.Size = new System.Drawing.Size(300, 200);
        this.popupMenusManager1.SetXPContextMenu(this.richTextBox1, this.popupMenu1);
        
        // Add to form
        this.Controls.Add(this.richTextBox1);
    }
    
    private void BarItem1_Click(object sender, EventArgs e)
    {
        if (richTextBox1.SelectionLength > 0)
        {
            richTextBox1.Cut();
        }
    }
    
    private void BarItem2_Click(object sender, EventArgs e)
    {
        if (richTextBox1.SelectionLength > 0)
        {
            richTextBox1.Copy();
        }
    }
    
    private void BarItem3_Click(object sender, EventArgs e)
    {
        richTextBox1.Paste();
    }
}
```

## Common Patterns

### Pattern 1: Multi-Level Menu Structure

```csharp
// Create hierarchy: File → New → New Project, New Website
ParentBarItem fileMenu = new ParentBarItem();
fileMenu.Text = "File";

ParentBarItem newSubmenu = new ParentBarItem();
newSubmenu.Text = "New";

BarItem newProject = new BarItem();
newProject.Text = "New Project...";
newProject.Click += NewProject_Click;

BarItem newWebsite = new BarItem();
newWebsite.Text = "New Website...";
newWebsite.Click += NewWebsite_Click;

// Build hierarchy
newSubmenu.Items.AddRange(new BarItem[] { newProject, newWebsite });
fileMenu.Items.Add(newSubmenu);
parentBarItem1.Items.Add(fileMenu);
```

### Pattern 2: Grouping with Separators

```csharp
// Create menu items
BarItem cut = new BarItem { Text = "Cut" };
BarItem copy = new BarItem { Text = "Copy" };
BarItem paste = new BarItem { Text = "Paste" };
BarItem selectAll = new BarItem { Text = "Select All" };

// Add items
parentBarItem1.Items.AddRange(new BarItem[] { cut, copy, paste, selectAll });

// Add separator after index 2 (after Paste)
parentBarItem1.SeparatorIndices.Add(2);

// Alternative: Use method
parentBarItem1.BeginGroupAt(selectAll);
```

### Pattern 3: Checked Menu Items

```csharp
BarItem wordWrap = new BarItem();
wordWrap.Text = "Word Wrap";
wordWrap.Checked = true;  // Initial state
wordWrap.Click += (s, e) => 
{
    wordWrap.Checked = !wordWrap.Checked;  // Toggle
    richTextBox1.WordWrap = wordWrap.Checked;  // Apply setting
};
```

### Pattern 4: Menu Items with Images

```csharp
BarItem saveItem = new BarItem();
saveItem.Text = "Save";
saveItem.Image = new ImageExt(Image.FromFile(@"icons\save.png"));
saveItem.DisabledImage = new ImageExt(Image.FromFile(@"icons\save_disabled.png"));
saveItem.HighlightedImage = new ImageExt(Image.FromFile(@"icons\save_highlight.png"));
saveItem.Shortcut = Shortcut.CtrlS;
saveItem.Tooltip = "Save the current document";
saveItem.ShowToolTipInPopUp = true;
```

### Pattern 5: Partial Menus (Frequently Used Items)

```csharp
// Enable partial menus
parentBarItem1.UsePartialMenus = true;

// Mark items as frequently used (visible by default)
cutItem.IsRecentlyUsedItem = true;
copyItem.IsRecentlyUsedItem = true;
pasteItem.IsRecentlyUsedItem = true;

// Mark items as less frequently used (hidden initially)
advancedItem.IsRecentlyUsedItem = false;
optionsItem.IsRecentlyUsedItem = false;
```

## Key Properties and Events

### PopupMenu Properties
- **ParentBarItem:** Root container holding all menu items (required)

### ParentBarItem Properties
- **Items:** Collection of BarItems in the menu
- **SizeToFit:** Auto-size menu items to fit content
- **SeparatorIndices:** Collection of indices where separators appear
- **UsePartialMenus:** Enable prioritized menu display
- **OverlapCheckBoxImageBounds:** Control checkbox/image overlap behavior

### BarItem Properties
- **Text:** Display text for the menu item
- **Image:** Icon displayed with the item
- **DisabledImage:** Icon when item is disabled
- **HighlightedImage:** Icon when item is highlighted/hovered
- **Tooltip:** Tooltip text displayed on hover
- **ShowToolTipInPopUp:** Enable tooltip display
- **Shortcut:** Keyboard shortcut (Ctrl+C, Alt+F, etc.)
- **ShortcutText:** Custom text for shortcut display
- **Checked:** Show checkmark before text
- **Enabled:** Enable/disable the menu item
- **IsRecentlyUsedItem:** Mark as frequently used (for partial menus)
- **ShowMnemonicUnderlinesAlways:** Always show mnemonic underlines
- **SizeToFit:** Auto-size item to fit content

### PopupMenusManager Methods
- **SetXPContextMenu(Control, PopupMenu):** Associate popup menu with a control

### ParentBarItem Methods
- **BeginGroupAt(BarItem):** Add separator before specified item
- **RemoveGroupAt(BarItem):** Remove separator before specified item
- **IsGroupBeginning(BarItem):** Check if item starts a group

### Events
- **BeforePopup:** Fired before menu is displayed (get mouse position, customize menu)
- **Popup:** Fired after menu is displayed
- **Collapse:** Fired when menu is closed
- **ParentBarItemChanged:** Fired when ParentBarItem properties change
- **Click (BarItem):** Fired when menu item is clicked

## Common Use Cases

### Use Case 1: Text Editor Context Menu
Create a context menu for RichTextBox with Cut, Copy, Paste, Select All, and formatting options.

### Use Case 2: Grid/DataGridView Context Menu
Add context menu to grid with options like Add Row, Delete Row, Edit Cell, Export Data, etc.

### Use Case 3: File/Folder Context Menu
Implement Windows Explorer-style context menu with Open, Copy, Delete, Properties, and nested submenus.

### Use Case 4: Form Control Context Menu
Add context menu to buttons, labels, or panels with control-specific actions.

### Use Case 5: Application-Wide Menu System
Integrate PopupMenu with Bar Manager for consistent menu experience across the application.

### Use Case 6: Dynamic Context Menus
Use BeforePopup event to customize menu items based on application state (enable/disable items, show/hide options).

### Use Case 7: Touch-Enabled Menus
Leverage built-in touch support for tablet and touch-screen applications.

## Tips and Best Practices

**BarItem Type Selection:**
- Use **BarItem** for standard menu commands
- Use **ParentBarItem** for submenus
- Use **DropDownBarItem** when you need custom controls (ColorPicker, custom panels)
- Use **ComboBoxBarItem** for selection with editing capability
- Use **ListBarItem** for fixed selection lists
- Use **StaticBarItem** for labels/headings
- Use **TextBoxBarItem** for text input in menus

**Performance:**
- Reuse PopupMenu instances across multiple controls when possible
- Use BeforePopup event for dynamic customization instead of recreating menus
- Avoid loading large images for menu items; use 16x16 or 24x24 icons

**Accessibility:**
- Always provide keyboard shortcuts for frequently used commands
- Use mnemonics (&Text) for keyboard navigation
- Set descriptive tooltip text for icon-only items
- Ensure disabled items are visually distinct

**Designer vs Code:**
- Designer is faster for static menu structures
- Code provides better control for dynamic menus
- Use Smart Tags in designer for quick setup
- Test both designer and code paths to ensure consistency

---

*For detailed implementation guidance, navigate to the reference files listed in the Documentation and Navigation Guide section.*
