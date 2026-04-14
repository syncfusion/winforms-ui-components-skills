---
name: syncfusion-winforms-xptoolbar
description: Implement and configure Syncfusion XPToolBar control for creating professional Visual Studio-style toolbars in Windows Forms applications. Use when you need customizable toolbar layouts with various item types (BarItem, ParentBarItem, DropDownBarItem, ComboBoxBarItem), dockable positioning, chevron overflow buttons, and Office themes. Covers toolbar structure, bar item management, docking positions, appearance customization with Office2007/2016 themes, and RTL support for creating feature-rich application toolbars and menu bars.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WinForms XPToolBar

The **XPToolBar** control is a Microsoft Visual Studio-inspired standalone toolbar that provides convenient access to frequently used commands and options. It supports various item types, flexible positioning, overflow handling, and professional themes.

## When to Use This Skill

Use this skill when the user needs to:

- **Application Toolbars:** Standard toolbar with buttons, dropdowns, and combo boxes
- **Visual Studio-Style Menus:** VS-inspired menu bars with File, Edit, View options
- **Quick Access Toolbars:** Frequently used commands in a compact toolbar
- **Dockable Toolbars:** Toolbars that can dock to any side (Top, Bottom, Left, Right)
- **Multiple Toolbar Levels:** Several toolbars stacked consecutively
- **Dropdown Menus:** DropDownBarItem and ParentBarItem with submenus
- **Toolbar Controls:** ComboBoxBarItem, TextBoxBarItem, ListBarItem in toolbars
- **Overflow Management:** Chevron button for items that don't fit
- **Themed Toolbars:** Office2007, Office2016, Metro themes
- **RTL Applications:** Right-to-Left toolbar layouts

## Component Overview

**XPToolBar** (`Syncfusion.Windows.Forms.Tools.XPMenus.XPToolBar`) is a versatile toolbar control supporting:

- **8 BarItem Types:**
  - **BarItem:** Individual menu/toolbar button
  - **ParentBarItem:** Submenu with child items
  - **DropDownBarItem:** Dropdown list selection
  - **ComboBoxBarItem:** Editable combo box
  - **ListBarItem:** List selection control
  - **StaticBarItem:** Non-clickable label/separator
  - **ToolbarListBarItem:** Toolbar list display
  - **TextBoxBarItem:** Text input box
  - **Separator:** Visual divider between items
- **Flexible Docking:** Top, Bottom, Left, Right positioning
- **Multiple Toolbars:** Add consecutive toolbar levels
- **Chevron/Overflow Button:** Shows hidden items when toolbar is too narrow
- **Rich Themes:** Office2007, Office2016 (Colorful/White/DarkGray/Black), Metro
- **Customization:** Text, colors, fonts, images, tooltips per item
- **RTL Support:** Right-to-Left layout for international apps
- **Localization:** Multi-language support with CultureInfo
- **Keyboard Shortcuts:** Assign keyboard shortcuts to BarItems

**Key Namespace:** `Syncfusion.Windows.Forms.Tools.XPMenus`

**Assembly:** `Syncfusion.Tools.Windows.dll` (and dependencies)

---

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

**When to read:** User needs to install, configure, or create their first XPToolBar.

**Topics covered:**
- Assembly deployment and NuGet package installation
- Adding XPToolBar via designer (toolbox, smart tags)
- Adding XPToolBar via code
- Positioning toolbar with Panel/container controls
- BarItem Collection Editor for adding items
- Bar.Items collection management
- Basic toolbar setup and initialization

### BarItem Types and Collections
📄 **Read:** [references/baritem-types.md](references/baritem-types.md)

**When to read:** User wants to add buttons, dropdowns, combo boxes, or understand different item types.

**Topics covered:**
- BarItem (individual button/menu item)
- ParentBarItem (submenu with child items)
- DropDownBarItem (dropdown selection list)
- ComboBoxBarItem (editable combo box control)
- ListBarItem (list selection)
- StaticBarItem (non-clickable labels)
- ToolbarListBarItem (toolbar item lists)
- TextBoxBarItem (text input in toolbar)
- Separator (visual divider)
- Adding items programmatically via Items collection
- Item properties (Text, Image, Tooltip, Enabled)

### Docking and Layout
📄 **Read:** [references/docking-and-layout.md](references/docking-and-layout.md)

**When to read:** User needs to position toolbar or add multiple toolbar levels.

**Topics covered:**
- Docking positions (Top, Bottom, Left, Right)
- Dock property configuration
- Multiple consecutive toolbars
- Adding toolbar levels
- Container requirements (Panel, GroupBox, etc.)
- Toolbar positioning strategies
- Layout best practices for different scenarios

### Chevron and Overflow
📄 **Read:** [references/chevron-and-overflow.md](references/chevron-and-overflow.md)

**When to read:** User encounters overflow issues or wants to manage hidden toolbar items.

**Topics covered:**
- Chevron/Overflow button functionality
- Viewing items that don't fit in toolbar width
- Overflow behavior and triggers
- Configuring chevron appearance
- User interaction with overflow menu
- Managing toolbar items for different window sizes

### Appearance Customization
📄 **Read:** [references/appearance-customization.md](references/appearance-customization.md)

**When to read:** User wants to customize colors, fonts, images, or tooltips.

**Topics covered:**
- Text customization (Text property)
- Background color (BackColor property)
- Foreground color (ForeColor property)
- Font configuration per item or toolbar
- Image assignment to BarItems
- Tooltip configuration (ToolTip property per item)
- Size and spacing adjustments
- Per-item appearance customization

### Themes and Styling
📄 **Read:** [references/themes-and-styling.md](references/themes-and-styling.md)

**When to read:** User wants Office themes, RTL support, or localization.

**Topics covered:**
- Visual styles: Office2007 (Blue, Silver, Black)
- Office2016 themes (Colorful, White, DarkGray, Black)
- Metro theme
- Style property configuration
- RTL (Right-to-Left) support for Arabic/Hebrew
- Localization with CultureInfo
- Keyboard shortcuts (Ctrl+X, Alt+F, etc.)
- Trigger BarItems for custom actions

---

## Quick Start Example

### Basic Toolbar with Menu Items

```csharp
using Syncfusion.Windows.Forms.Tools.XPMenus;
using System.Windows.Forms;

// Create panel to host toolbar
Panel panel1 = new Panel();
panel1.Dock = DockStyle.Top;
panel1.Height = 50;

// Create XPToolBar
XPToolBar xpToolBar1 = new XPToolBar();

// Create menu items
BarItem fileItem = new BarItem();
fileItem.Text = "File";

ParentBarItem editItem = new ParentBarItem();
editItem.Text = "Edit";

// Add submenu items to Edit
BarItem cutItem = new BarItem { Text = "Cut" };
BarItem copyItem = new BarItem { Text = "Copy" };
BarItem pasteItem = new BarItem { Text = "Paste" };
editItem.Items.AddRange(new BarItem[] { cutItem, copyItem, pasteItem });

DropDownBarItem viewItem = new DropDownBarItem();
viewItem.Text = "View";

// Add items to toolbar
xpToolBar1.Bar.Items.AddRange(new BarItem[] { 
    fileItem, 
    editItem, 
    viewItem 
});

// Add toolbar to panel
panel1.Controls.Add(xpToolBar1);

// Add panel to form
this.Controls.Add(panel1);
```

### Toolbar with Office2016 Theme

```csharp
// Apply Office2016 Colorful theme
xpToolBar1.Style = VisualStyle.Office2016Colorful;
```

---

## Common Patterns

### File Menu with Submenu Items

```csharp
// Create File menu with submenu
ParentBarItem fileMenu = new ParentBarItem();
fileMenu.Text = "File";

// Add submenu items
BarItem newItem = new BarItem { Text = "New", Image = Properties.Resources.New };
BarItem openItem = new BarItem { Text = "Open", Image = Properties.Resources.Open };
BarItem saveItem = new BarItem { Text = "Save", Image = Properties.Resources.Save };
BarItem separator = new BarItem { BarItemType = BarItemType.Separator };
BarItem exitItem = new BarItem { Text = "Exit" };

fileMenu.Items.AddRange(new BarItem[] { 
    newItem, 
    openItem, 
    saveItem, 
    separator, 
    exitItem 
});

// Handle click events
newItem.Click += (s, e) => CreateNewDocument();
openItem.Click += (s, e) => OpenDocument();
saveItem.Click += (s, e) => SaveDocument();
exitItem.Click += (s, e) => Application.Exit();

xpToolBar1.Bar.Items.Add(fileMenu);
```

**When to use:** Standard application menu with File operations.

### Toolbar with ComboBox and TextBox

```csharp
// Font selector combo box
ComboBoxBarItem fontCombo = new ComboBoxBarItem();
fontCombo.Text = "Font:";
fontCombo.ChoiceList.AddRange(new string[] { "Arial", "Calibri", "Times New Roman" });
fontCombo.SelectedIndex = 0;

// Font size text box
TextBoxBarItem fontSizeTextBox = new TextBoxBarItem();
fontSizeTextBox.Text = "Size:";
fontSizeTextBox.TextBoxValue = "12";
fontSizeTextBox.MinimumSize = new Size(50, 20);

// Add to toolbar
xpToolBar1.Bar.Items.AddRange(new BarItem[] { 
    fontCombo, 
    fontSizeTextBox 
});

// Handle changes
fontCombo.SelectedIndexChanged += (s, e) => {
    string selectedFont = fontCombo.ChoiceList[fontCombo.SelectedIndex];
    ApplyFont(selectedFont);
};
```

**When to use:** Text editor or formatting toolbars.

### Docked Toolbar (Top)

```csharp
// Create toolbar with top docking
XPToolBar toolbar = new XPToolBar();
toolbar.Dock = DockStyle.Top;

// Add items
toolbar.Bar.Items.AddRange(new BarItem[] { 
    new BarItem { Text = "Home" },
    new BarItem { Text = "Insert" },
    new BarItem { Text = "View" }
});

// Add directly to form (no panel needed for simple top dock)
this.Controls.Add(toolbar);
```

**When to use:** Simple top-docked toolbar without complex layout.

### Multiple Toolbar Levels

```csharp
// First toolbar level (Menu bar)
Panel menuPanel = new Panel { Dock = DockStyle.Top, Height = 30 };
XPToolBar menuToolbar = new XPToolBar();
menuToolbar.Bar.Items.AddRange(new BarItem[] { 
    new ParentBarItem { Text = "File" },
    new ParentBarItem { Text = "Edit" },
    new ParentBarItem { Text = "View" }
});
menuPanel.Controls.Add(menuToolbar);

// Second toolbar level (Quick access)
Panel quickPanel = new Panel { Dock = DockStyle.Top, Height = 40 };
XPToolBar quickToolbar = new XPToolBar();
quickToolbar.Bar.Items.AddRange(new BarItem[] { 
    new BarItem { Text = "New", Image = Properties.Resources.New },
    new BarItem { Text = "Save", Image = Properties.Resources.Save },
    new BarItem { Text = "Undo", Image = Properties.Resources.Undo }
});
quickPanel.Controls.Add(quickToolbar);

// Add to form (order matters - add quick toolbar first for correct stacking)
this.Controls.Add(quickPanel);
this.Controls.Add(menuPanel);
```

**When to use:** Complex applications with menu bar and quick access toolbar.

---

## Key Properties

### Core Properties

| Property | Type | Description | When to Use |
|----------|------|-------------|-------------|
| **Bar.Items** | `BarItemCollection` | Collection of toolbar items | Add/manage toolbar buttons and controls |
| **Dock** | `DockStyle` | Docking position | Position toolbar (Top/Bottom/Left/Right) |
| **Style** | `VisualStyle` | Visual theme style | Apply Office2007/2016/Metro themes |

### BarItem Properties

| Property | Type | Description | When to Use |
|----------|------|-------------|-------------|
| **Text** | `string` | Item text/caption | Set button or menu label |
| **Image** | `Image` | Item icon | Add visual icon to item |
| **ToolTip** | `string` | Tooltip text | Provide hover information |
| **Enabled** | `bool` | Enable/disable item | Control item interactivity |
| **Visible** | `bool` | Show/hide item | Toggle item visibility |

### BarItem Types Summary

- **BarItem** - Standard button/menu item
- **ParentBarItem** - Menu with submenu items (use `.Items` collection)
- **DropDownBarItem** - Dropdown list (use `.ChoiceList`)
- **ComboBoxBarItem** - Editable combo box (use `.Items` and `.SelectedIndex`)
- **ListBarItem** - List selection control
- **StaticBarItem** - Non-clickable label
- **TextBoxBarItem** - Text input (use `.TextBoxValue`)
- **ToolbarListBarItem** - Toolbar item list
- **Separator** - Visual divider (set `BarItemType = BarItemType.Separator`)

---

## Common Use Cases

### 1. Application Menu Bar
**Scenario:** Standard menu bar with File, Edit, View, Help.
- Use ParentBarItem for each top-level menu
- Add submenu BarItems to each ParentBarItem
- Add separators between item groups
- Handle Click events for actions
- Read: [baritem-types.md](references/baritem-types.md)

### 2. Text Editor Toolbar
**Scenario:** Formatting toolbar with font, size, bold, italic.
- ComboBoxBarItem for font selection
- TextBoxBarItem for font size
- BarItems with images for bold/italic/underline
- Use images for visual clarity
- Read: [baritem-types.md](references/baritem-types.md), [appearance-customization.md](references/appearance-customization.md)

### 3. Ribbon-Like Quick Access
**Scenario:** Quick access toolbar above main content.
- Top-docked toolbar with frequently used commands
- Use images without text (icon-only buttons)
- Enable tooltips for each item
- Apply Office2016 theme for modern look
- Read: [docking-and-layout.md](references/docking-and-layout.md), [themes-and-styling.md](references/themes-and-styling.md)

### 4. Multi-Level Toolbar System
**Scenario:** Menu bar + Quick access toolbar + Formatting toolbar.
- Create multiple Panel containers
- Add one XPToolBar per panel
- Stack panels with proper docking order
- Apply consistent theme across all toolbars
- Read: [docking-and-layout.md](references/docking-and-layout.md)

### 5. Localized Toolbar
**Scenario:** Toolbar for international application (Arabic/Hebrew).
- Enable RTL support
- Use CultureInfo for localization
- Load localized strings from resources
- Test with different languages
- Read: [themes-and-styling.md](references/themes-and-styling.md)

### 6. Dynamic Toolbar
**Scenario:** Add/remove toolbar items at runtime based on context.
- Use Bar.Items.Add/Remove methods
- Show/hide items with Visible property
- Enable/disable with Enabled property
- Refresh toolbar layout after changes
- Read: [baritem-types.md](references/baritem-types.md)

---

## Events

**Common BarItem events:**
- **Click** - Raised when item is clicked
- **SelectedIndexChanged** - For ComboBoxBarItem/DropDownBarItem selection changes

```csharp
barItem1.Click += (sender, e) => {
    // Handle button click
    MessageBox.Show("Item clicked!");
};

comboBoxBarItem1.SelectedIndexChanged += (sender, e) => {
    string selected = comboBoxBarItem1.ChoiceList[comboBoxBarItem1.SelectedIndex];
    // Handle selection change
};
```

---

## Best Practices

1. **Container Usage:** Always place XPToolBar in a Panel or container for proper positioning
2. **Theme Consistency:** Match toolbar Style to application theme
3. **Image Quality:** Use high-quality icons (16x16 or 24x24) for BarItem images
4. **Tooltips:** Always provide tooltips for icon-only buttons
5. **Overflow Management:** Keep toolbar item count reasonable to minimize overflow
6. **Event Handling:** Subscribe to Click events for all interactive BarItems
7. **Separators:** Use separators to visually group related items
8. **Keyboard Shortcuts:** Assign shortcuts for frequently used commands
9. **Dynamic Updates:** Use Enabled/Visible properties rather than removing items
10. **Docking Order:** Add panels in reverse order when stacking multiple toolbars

---

## See Also

- [Syncfusion XPToolBar Documentation](https://help.syncfusion.com/windowsforms/xptoolbar/overview)
- [XPToolBar API Reference](https://help.syncfusion.com/cr/windowsforms/Syncfusion.Windows.Forms.Tools.XPMenus.XPToolBar.html)
- [BarItem API Reference](https://help.syncfusion.com/cr/windowsforms/Syncfusion.Windows.Forms.Tools.XPMenus.BarItem.html)
