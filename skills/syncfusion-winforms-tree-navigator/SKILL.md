---
name: syncfusion-winforms-tree-navigator
description: Implement and configure Syncfusion TreeNavigator control for Windows Forms - a unique navigation interface that expands tree structures in-place without taking extra space. Use when you need hierarchical navigation with breadcrumb modes, drill-down capabilities, or space-efficient collapsible navigation menus. Covers in-place tree expansion, back button navigation, expandable menu items, file explorer-style navigation, and replacing standard TreeView with collapsible alternatives.
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
---

# Implementing Tree Navigators (TreeNavigator)

A unique navigation control for Windows Forms that provides in-place tree expansion without consuming additional screen space. Items expand and collapse inline, allowing users to drill down through hierarchical structures by clicking items.

## When to Use This Skill

Use this skill when you need to:
- **Space-Efficient Navigation**: Display hierarchical navigation that expands in-place without growing the control size
- **Drill-Down Interface**: Allow users to navigate from parent to child items with back button navigation
- **File Explorer Style**: Create file/folder navigation similar to Windows Explorer breadcrumb navigation
- **Menu Hierarchies**: Build collapsible menu systems with parent-child relationships
- **Breadcrumb Navigation**: Implement extended mode with stacked headers showing navigation path
- **Settings Navigation**: Create category-based settings interfaces with hierarchical organization
- **Document Browser**: Navigate through document structures, sections, and subsections
- **TreeView Alternative**: Replace standard TreeView with a more compact, focused navigation experience

## Component Overview

The **TreeNavigator** control provides a unique interface for hierarchical navigation:
- Expands tree structures in-place without taking more screen space
- Items collapse automatically when navigating to child items
- Drill down to sub-items by clicking on parent items
- Navigate back to parent levels using back button
- Two navigation modes: Default (single back button) and Extended (breadcrumb-style headers)

**Key Capabilities:**
- `NavigationMode` property for selecting navigation style (Default or Extended)
- `Items` collection for managing TreeMenuItem objects
- `Header` configuration for customizing header appearance and behavior
- `Style` property for applying Office 2016/2019 visual themes
- `SelectedItem` property for programmatic selection
- Selection events for tracking and controlling navigation

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

**When to read:** Starting new implementation, setting up dependencies, first-time usage, basic TreeNavigator creation

**Topics covered:**
- Assembly deployment (Syncfusion.Tools.Windows, Syncfusion.Shared.Base)
- NuGet package installation
- Namespace requirements (Syncfusion.Windows.Forms.Tools)
- Adding TreeNavigator via designer (drag-and-drop from toolbox)
- Adding TreeNavigator programmatically in code
- Adding TreeMenuItem items to Items collection
- Header configuration (HeaderText property)
- Adding items through designer (Smart Tag editor)
- Complete setup example with code
- .NET Core workaround for child items

---

### Navigation Modes
📄 **Read:** [references/navigation-modes.md](references/navigation-modes.md)

**When to read:** Choosing navigation behavior, implementing back button navigation, breadcrumb-style interfaces

**Topics covered:**
- NavigationMode property overview
- Default Mode (single back button, selected item at top)
- Extended Mode (stacked breadcrumb headers, click any level)
- Navigation behavior differences between modes
- Use cases for each mode (simple vs complex hierarchies)
- Code examples for setting navigation mode
- Visual comparison of Default vs Extended modes

---

### Appearance and Styling
📄 **Read:** [references/appearance-styling.md](references/appearance-styling.md)

**When to read:** Customizing visual appearance, applying themes, styling headers, configuring borders and colors

**Topics covered:**
- Style property (Office2016Colorful/White/Black/DarkGray, Office2019Colorful, Default)
- Visual theme application and comparison
- Header customization (Height, HeaderBackColor, HeaderForeColor, HeaderText, TextBounds)
- ShowHeader property (hide/show header area)
- Border customization (BorderColor, BorderThickness)
- Item spacing (PadY property for gap between items)
- TreeMenuItem appearance (ItemBackColor, ItemHoverColor, SelectedColor, SelectedItemForeColor)
- Text alignment (TextAlign: Center, Left, Right)
- Complete styling examples with code

---

### Tree Menu Items
📄 **Read:** [references/tree-menu-items.md](references/tree-menu-items.md)

**When to read:** Managing items, adding/removing TreeMenuItems, building hierarchies, manipulating item collections

**Topics covered:**
- TreeMenuItem class overview and properties
- Items collection management (Add, Remove, Clear)
- Adding items programmatically (C# and VB.NET)
- Adding items via designer (Smart Tag collection editor)
- Parent-child item relationships and nesting
- Item properties (Text, ItemBackColor, ItemHoverColor, SelectedColor, SelectedItemForeColor)
- Building multi-level hierarchies
- Dynamic item manipulation at runtime
- Complete examples with nested items

---

### Selection and Events
📄 **Read:** [references/selection-events.md](references/selection-events.md)

**When to read:** Handling selection changes, responding to navigation, programmatically selecting items, preventing selection

**Topics covered:**
- SelectedItem property (get/set currently selected item)
- SelectionChanging event (fires before selection changes, cancellable)
- SelectionStateChangingEventArgs (NewValue, OldValue, Expanded, Cancel properties)
- SelectionChanged event (fires after selection changes)
- SelectionStateChangedEventArgs (SelectedItem, Expanded properties)
- Event handling examples with code
- Cancelling selection changes (validation scenarios)
- Touch scroll behavior (UseTouchScrollBehavior property)
- Complete event workflow examples

---

## Quick Start Example

### File Explorer-Style Navigation with Office2016 Theme

```csharp
using Syncfusion.Windows.Forms.Tools;

// Create TreeNavigator
TreeNavigator treeNavigator = new TreeNavigator();
treeNavigator.Size = new Size(250, 400);
treeNavigator.Header.HeaderText = "This PC";
treeNavigator.Style = TreeNavigatorStyle.Office2016Colorful;
treeNavigator.NavigationMode = NavigationMode.Extended;

// Add root items
TreeMenuItem desktop = new TreeMenuItem { Text = "Desktop" };
TreeMenuItem documents = new TreeMenuItem { Text = "Documents" };
TreeMenuItem downloads = new TreeMenuItem { Text = "Downloads" };

// Add items to TreeNavigator
treeNavigator.Items.Add(desktop);
treeNavigator.Items.Add(documents);
treeNavigator.Items.Add(downloads);

// Add child items to Documents
TreeMenuItem workFiles = new TreeMenuItem { Text = "Work Files" };
TreeMenuItem personalFiles = new TreeMenuItem { Text = "Personal Files" };
documents.Items.Add(workFiles);
documents.Items.Add(personalFiles);

this.Controls.Add(treeNavigator);
```

```vbnet
Imports Syncfusion.Windows.Forms.Tools

' Create TreeNavigator
Dim treeNavigator As New TreeNavigator()
treeNavigator.Size = New Size(250, 400)
treeNavigator.Header.HeaderText = "This PC"
treeNavigator.Style = TreeNavigatorStyle.Office2016Colorful
treeNavigator.NavigationMode = NavigationMode.Extended

' Add root items
Dim desktop As New TreeMenuItem With {.Text = "Desktop"}
Dim documents As New TreeMenuItem With {.Text = "Documents"}
Dim downloads As New TreeMenuItem With {.Text = "Downloads"}

' Add items to TreeNavigator
treeNavigator.Items.Add(desktop)
treeNavigator.Items.Add(documents)
treeNavigator.Items.Add(downloads)

' Add child items to Documents
Dim workFiles As New TreeMenuItem With {.Text = "Work Files"}
Dim personalFiles As New TreeMenuItem With {.Text = "Personal Files"}
documents.Items.Add(workFiles)
documents.Items.Add(personalFiles)

Me.Controls.Add(treeNavigator)
```

---

## Common Patterns

### Pattern 1: Settings Navigation with Custom Colors

```csharp
// Create TreeNavigator for settings
TreeNavigator settingsNav = new TreeNavigator();
settingsNav.Header.HeaderText = "Settings";
settingsNav.Style = TreeNavigatorStyle.Office2016White;
settingsNav.NavigationMode = NavigationMode.Default;

// Customize appearance
settingsNav.Header.HeaderBackColor = Color.FromArgb(0, 120, 215);
settingsNav.Header.HeaderForeColor = Color.White;
settingsNav.ItemBackColor = Color.WhiteSmoke;

// Add settings categories
TreeMenuItem general = new TreeMenuItem { Text = "General" };
TreeMenuItem appearance = new TreeMenuItem { Text = "Appearance" };
TreeMenuItem advanced = new TreeMenuItem { Text = "Advanced" };

settingsNav.Items.Add(general);
settingsNav.Items.Add(appearance);
settingsNav.Items.Add(advanced);

// Add sub-settings
general.Items.Add(new TreeMenuItem { Text = "Language" });
general.Items.Add(new TreeMenuItem { Text = "Startup" });
appearance.Items.Add(new TreeMenuItem { Text = "Theme" });
appearance.Items.Add(new TreeMenuItem { Text = "Colors" });
```

### Pattern 2: Breadcrumb Navigation with Selection Event

```csharp
// Create breadcrumb-style navigator
TreeNavigator breadcrumbNav = new TreeNavigator();
breadcrumbNav.NavigationMode = NavigationMode.Extended;
breadcrumbNav.Style = TreeNavigatorStyle.Office2016Colorful;

// Handle selection changes
breadcrumbNav.SelectionChanged += (sender, e) => {
    TreeMenuItem selected = e.SelectedItem;
    bool isExpanded = e.Expanded;
    
    // Update content panel based on selection
    MessageBox.Show($"Navigated to: {selected.Text}\nExpanded: {isExpanded}");
};

// Build hierarchy
TreeMenuItem root = new TreeMenuItem { Text = "Home" };
breadcrumbNav.Items.Add(root);
TreeMenuItem level1 = new TreeMenuItem { Text = "Category" };
TreeMenuItem level2 = new TreeMenuItem { Text = "Subcategory" };
root.Items.Add(level1);
level1.Items.Add(level2);
```

### Pattern 3: Compact Navigation Menu with Item Hover Colors

```csharp
// Create compact navigation
TreeNavigator compactNav = new TreeNavigator();
compactNav.ShowHeader = false; // Hide header for compact look
compactNav.Style = TreeNavigatorStyle.Office2016DarkGray;
compactNav.PadY = 5; // Reduce spacing between items

// Customize item colors
TreeMenuItem dashboard = new TreeMenuItem 
{ 
    Text = "Dashboard",
    ItemBackColor = Color.FromArgb(45, 45, 48),
    ItemHoverColor = Color.FromArgb(62, 62, 66),
    SelectedColor = Color.FromArgb(0, 122, 204)
};

TreeMenuItem reports = new TreeMenuItem 
{ 
    Text = "Reports",
    ItemBackColor = Color.FromArgb(45, 45, 48),
    ItemHoverColor = Color.FromArgb(62, 62, 66)
};

compactNav.Items.Add(dashboard);
compactNav.Items.Add(reports);

// Add report sub-items
reports.Items.Add(new TreeMenuItem { Text = "Sales" });
reports.Items.Add(new TreeMenuItem { Text = "Inventory" });
reports.Items.Add(new TreeMenuItem { Text = "Analytics" });
```

---

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| **Items** | Collection | Collection of TreeMenuItem objects that form the navigation hierarchy |
| **NavigationMode** | NavigationMode | Navigation style: Default (back button) or Extended (breadcrumb) |
| **Style** | TreeNavigatorStyle | Visual theme: Office2016Colorful/White/Black/DarkGray, Office2019Colorful, Default |
| **SelectedItem** | TreeMenuItem | Gets or sets the currently selected item in the current hierarchy level |
| **Header.HeaderText** | string | Text displayed in the header area |
| **Header.HeaderBackColor** | Color | Background color of the header |
| **Header.HeaderForeColor** | Color | Foreground (text) color of the header |
| **Header.Height** | int | Height of the header area in pixels |
| **ShowHeader** | bool | Shows or hides the header area (true = visible, false = hidden) |
| **BorderColor** | Color | Color of the control border |
| **BorderThickness** | int | Thickness of the control border in pixels |
| **PadY** | int | Vertical spacing (gap) between TreeMenuItem items |
| **ItemBackColor** | Color | Background color for all TreeMenuItem items (can be overridden per item) |
| **TextAlign** | TextAlignment | Text alignment for items: Center, Left, or Right |
| **UseTouchScrollBehavior** | bool | Enables touch-friendly scrolling with auto-hide scrollbar |

---

## Common Use Cases

1. **File/Folder Browser**: Navigate through folder hierarchies like Windows Explorer with back button or breadcrumb navigation
2. **Settings Interface**: Organize application settings into categories and subcategories with drill-down navigation
3. **Document Outline**: Navigate through document sections, chapters, and subsections
4. **Menu Navigation**: Create hierarchical menu systems for complex applications with space constraints
5. **Product Categories**: Browse product catalogs with category → subcategory → product navigation
6. **Help System**: Organize help topics into categories with easy navigation between topics and back to categories
7. **Data Dashboard**: Navigate between dashboard sections, reports, and detailed views
8. **Workflow Steps**: Guide users through multi-step workflows with hierarchical organization

---

## Additional Resources

- [TreeNavigator Control on Syncfusion Help](https://help.syncfusion.com/windowsforms/tree-navigator/overview)
- [Syncfusion.Windows.Forms.Tools Namespace](https://help.syncfusion.com/cr/windowsforms/Syncfusion.Windows.Forms.Tools.html)
- [TreeNavigator API Reference](https://help.syncfusion.com/cr/windowsforms/Syncfusion.Windows.Forms.Tools.TreeNavigator.html)
- [TreeMenuItem API Reference](https://help.syncfusion.com/cr/windowsforms/Syncfusion.Windows.Forms.Tools.TreeMenuItem.html)
