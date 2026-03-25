---
name: syncfusion-winforms-combodropdown
description: Guide for implementing Syncfusion ComboDropDown control in Windows Forms. Use this when you need a flexible combo box that hosts any control (TreeView, ListView, GridList, custom controls) in the dropdown area. Essential for custom dropdown UI requirements and complex dropdown content beyond standard ListBox functionality.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing ComboDropDown

## When to Use This Skill

Use this skill when the user needs to implement the Syncfusion **ComboDropDown** control in a Windows Forms application. This skill is specifically designed for scenarios where you need:

1. **Flexible dropdown hosting** - Host ANY control in the dropdown area (TreeView, ListView, GridList, DataGrid, custom controls), not limited to ListBox-derived controls
2. **Custom dropdown UI** - Create unique dropdown experiences with controls that provide richer data visualization than standard combo boxes
3. **Tree-based selection** - Implement hierarchical navigation with TreeView controls in the dropdown
4. **Multi-column selection** - Display data in grid or multi-column layouts for easier browsing
5. **Complex data interaction** - Scenarios requiring custom logic for transferring data between the edit portion and the dropdown control
6. **Custom popup behavior** - Full control over when and how the dropdown opens, closes, and responds to user interactions
7. **Advanced control integration** - Integrate specialized controls that provide features beyond standard list selection
8. **Developer-managed synchronization** - Scenarios where automatic data binding isn't suitable and you need manual control

**Key Differentiator:** Unlike ComboBoxBase (which only hosts ListBox-derived controls with built-in data binding), ComboDropDown can host ANY control but requires you to implement the interaction logic between the text box and the hosted control.

**When NOT to use:** If you only need a simple dropdown with ListBox, CheckedListBox, or GridListControl and want automatic selection handling, use ComboBoxBase instead.

## Component Overview

The **ComboDropDown** control is a lightweight, flexible combo box that provides:

- **Universal control hosting** - PopupControl property accepts any Windows Forms control
- **Separated architecture** - Independent edit portion (text box) and dropdown portion (any control)
- **Developer-managed interaction** - You control how selections transfer to text and vice versa
- **Event-driven synchronization** - DropDown event (before showing) and Popup event (after showing) for implementing interaction logic
- **Standard combo box appearance** - Looks like a regular combo box but with unlimited dropdown flexibility
- **Programmatic dropdown control** - Methods to show/hide popup with close type specification
- **Full customization** - Appearance, themes, text behavior, and border styling options

**Architecture:** ComboDropDown consists of two independent parts:
1. **Edit portion** - A text box where users type or see selected values
2. **Dropdown portion** - A popup container hosting any control you specify

You're responsible for connecting these parts through event handlers.

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

When user wants to set up ComboDropDown for the first time. Covers:
- Assembly references and NuGet package installation
- Namespace imports (Syncfusion.Windows.Forms.Tools)
- Designer-based setup (drag-drop, property configuration)
- Code-based setup (programmatic instantiation)
- Setting PopupControl property to associate a control
- Basic TreeView integration example
- Initial configuration and troubleshooting

### Event Handling and Interaction
📄 **Read:** [references/event-handling.md](references/event-handling.md)

When user needs to implement data interaction between the combo's text and the hosted control. Covers:
- DropDown event - syncing text to control selection before dropdown shows
- TreeView/ListView DoubleClick events - transferring selection to text
- Closing dropdown programmatically (PopupCloseType.Done)
- Popup event - setting focus on hosted control after dropdown appears
- FindNode recursive helper for TreeView navigation
- Complete TreeView interaction implementation
- GridList integration patterns
- SuppressDropDownEvent property
- Best practices for event-driven interaction

### Text Behavior and Input Control
📄 **Read:** [references/text-behavior.md](references/text-behavior.md)

When user wants to control text input behavior in the edit portion. Covers:
- CharacterCasing property (Upper, Lower, Normal)
- NumberOnly property (restricting to numeric input)
- ReadOnly property (preventing edits)
- CaseSensitiveAutocomplete property
- MatchFirstCharacterOnly property
- Banner text support (BannerTextProvider integration)
- Complete examples for each behavior

### Appearance Customization
📄 **Read:** [references/appearance-customization.md](references/appearance-customization.md)

When user needs to customize the control's visual appearance and borders. Covers:
- Border3DStyle property (10 3D border styles)
- BorderSides property (specifying which sides have borders)
- FlatStyle property (Flat, System, Standard modes)
- FlatBorderColor property
- Edit portion appearance (Font, ForeColor, BackColor)
- Dropdown button customization
- Style property dependency
- Complete styling examples

### Themes and Styling
📄 **Read:** [references/themes-and-styles.md](references/themes-and-styles.md)

When user wants to apply visual themes or Office-style appearances. Covers:
- Style property (Office2016 variants, Metro, Office2010/2007/2003, VS2005, Default)
- Office2007ColorTheme property (Blue, Silver, Black schemes)
- Custom color support (Managed theme with ApplyManagedColors)
- IgnoreThemeBackground property
- Theme vs appearance property interaction
- Visual comparisons and complete theming examples

## Quick Start Example

### Designer Setup with TreeView

```csharp
// 1. Drag ComboDropDown and TreeView onto form
// 2. Add nodes to TreeView (in designer or code)
// 3. Set TreeView.HideSelection = false
// 4. Set ComboDropDown.PopupControl = treeView1

// Event handlers for interaction
private void comboDropDown1_DropDown(object sender, EventArgs e)
{
    // Before dropdown shown: select TreeNode matching combo text
    if (!string.IsNullOrEmpty(this.comboDropDown1.Text))
    {
        TreeNode matchedNode = FindNode(this.treeView1.Nodes, this.comboDropDown1.Text);
        this.treeView1.SelectedNode = matchedNode;
    }
}

private void treeView1_DoubleClick(object sender, EventArgs e)
{
    // Transfer selected node text to combo
    if (this.treeView1.SelectedNode != null)
    {
        this.comboDropDown1.Text = this.treeView1.SelectedNode.Text;
    }
    
    // Close the dropdown
    this.comboDropDown1.PopupContainer.HidePopup(PopupCloseType.Done);
}

private TreeNode FindNode(TreeNodeCollection nodes, string text)
{
    // Recursively find matching node
    foreach (TreeNode child in nodes)
    {
        if (child.Text == text) return child;
    }
    foreach (TreeNode child in nodes)
    {
        TreeNode matched = FindNode(child.Nodes, text);
        if (matched != null) return matched;
    }
    return null;
}
```

### Code-Based Setup

```csharp
using Syncfusion.Windows.Forms.Tools;

// Create ComboDropDown
ComboDropDown comboDropDown1 = new ComboDropDown();
comboDropDown1.Location = new Point(20, 20);
comboDropDown1.Size = new Size(200, 25);

// Create and configure TreeView
TreeView treeView1 = new TreeView();
treeView1.HideSelection = false;
treeView1.Nodes.Add("Root");
treeView1.Nodes[0].Nodes.Add("Child 1");
treeView1.Nodes[0].Nodes.Add("Child 2");

// Associate TreeView with ComboDropDown
comboDropDown1.PopupControl = treeView1;

// Wire up events
comboDropDown1.DropDown += comboDropDown1_DropDown;
treeView1.DoubleClick += treeView1_DoubleClick;

// Add to form
this.Controls.Add(comboDropDown1);
```

## Common Patterns

### Pattern 1: TreeView Hierarchical Selection

```csharp
// Setup
comboDropDown1.PopupControl = treeView1;
treeView1.HideSelection = false;

// Sync text → TreeView selection (before dropdown)
comboDropDown1.DropDown += (s, e) => {
    var node = FindNode(treeView1.Nodes, comboDropDown1.Text);
    treeView1.SelectedNode = node;
};

// Sync TreeView selection → text (on double-click)
treeView1.DoubleClick += (s, e) => {
    if (treeView1.SelectedNode != null)
    {
        comboDropDown1.Text = treeView1.SelectedNode.Text;
        comboDropDown1.PopupContainer.HidePopup(PopupCloseType.Done);
    }
};
```

### Pattern 2: Multi-Column Grid Selection

```csharp
// Host a GridList control for multi-column data
GridListControl gridList = new GridListControl();
// Configure grid columns and data...

comboDropDown1.PopupControl = gridList;

// Sync selection to combo text
gridList.SelectedIndexChanged += (s, e) => {
    if (gridList.SelectedIndex >= 0)
    {
        comboDropDown1.Text = gridList.SelectedItem.ToString();
    }
};
```

### Pattern 3: Focus Management for Mouse Interaction

```csharp
// Make dropdown control respond to mouse immediately
comboDropDown1.PopupContainer.Popup += (s, e) => {
    // Give focus to hosted control after dropdown shown
    treeView1.Focus();
};
```

## Key Properties

### PopupControl Setup

| Property | Type | Description | When to Use |
|----------|------|-------------|-------------|
| `PopupControl` | Control | The control displayed in dropdown | Set to any Windows Forms control you want to host |

### Text Behavior

| Property | Type | Description | When to Use |
|----------|------|-------------|-------------|
| `Text` | string | Text displayed in edit portion | Get/set the combo's current text value |
| `CharacterCasing` | CharacterCasing | Upper, Lower, or Normal | Force uppercase/lowercase input |
| `NumberOnly` | bool | Restricts input to numbers | Numeric-only scenarios |
| `ReadOnly` | bool | Prevents editing | Display-only with selection from dropdown |

### Dropdown Control

| Property | Type | Description | When to Use |
|----------|------|-------------|-------------|
| `PopupContainer` | PopupControlContainer | Container managing popup behavior | Access HidePopup() or Popup event |
| `SuppressDropDownEvent` | bool | Prevents DropDown event from firing | Skip sync logic in specific scenarios |

### Appearance

| Property | Type | Description | When to Use |
|----------|------|-------------|-------------|
| `Border3DStyle` | Border3DStyle | 3D border appearance (10 styles) | Customize border look when FlatStyle is Standard |
| `BorderSides` | Border3DSide | Which sides have borders | Custom border configurations |
| `FlatStyle` | FlatStyle | Flat, System, or Standard | Control overall appearance mode |
| `FlatBorderColor` | Color | Border color in Flat mode | Custom border colors |

### Theming

| Property | Type | Description | When to Use |
|----------|------|-------------|-------------|
| `Style` | VisualStyle | Office2016Colorful, Metro, Office2010, etc. | Apply modern visual themes |
| `Office2007ColorTheme` | Office2007Theme | Blue, Silver, Black schemes | Office 2007 style appearance |
| `IgnoreThemeBackground` | bool | Use BackColor instead of theme | Override theme background |

## Common Use Cases

### 1. File System Navigator with TreeView
**Scenario:** User needs to browse a hierarchical file/folder structure  
**Solution:** Host TreeView showing directory tree, sync selected path to combo text

### 2. Category Selection with Multi-Column Grid
**Scenario:** User needs to select from categorized data with multiple display columns  
**Solution:** Host GridListControl with category, name, and description columns

### 3. Date Picker with Calendar Control
**Scenario:** User needs a custom date picker with visual calendar  
**Solution:** Host MonthCalendar or custom calendar control, transfer selected date to text

### 4. Color Picker with Custom Panel
**Scenario:** User needs to select colors from a visual color palette  
**Solution:** Host custom panel with color swatches, transfer color name/hex to text

## ComboDropDown vs ComboBoxBase Decision Guide

Choose **ComboDropDown** when:
- ✅ Need to host TreeView, ListView, DataGrid, or any custom control
- ✅ Want full control over text ↔ control synchronization logic
- ✅ Require custom popup behavior beyond standard selection
- ✅ Building hierarchical navigation or complex data visualization
- ✅ Standard ListBox functionality is insufficient

Choose **ComboBoxBase** when:
- ❌ Only need ListBox, CheckedListBox, or GridListControl (simple lists)
- ❌ Want automatic data binding and selection handling
- ❌ Prefer built-in SelectionChangedCommitted events
- ❌ Don't need to manage interaction logic yourself
- ❌ Standard dropdown list functionality is sufficient

**Key Difference:** ComboDropDown = maximum flexibility + manual implementation; ComboBoxBase = ListBox-only + automatic handling
