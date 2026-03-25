---
name: syncfusion-winforms-multiselect-combobox
description: Guide for implementing the Syncfusion Windows Forms MultiSelectionComboBox control — a ComboBox with multi-item selection, auto-suggestion, and tag-style visual items. Use this skill when the user mentions MultiSelectionComboBox, WinForms multi-select combo, or Syncfusion.Windows.Forms.Tools.MultiSelectionComboBox, or needs a combo box that allows selecting multiple items in a Windows Forms application. Applies when configuring display modes, binding data sources, styling visual items, handling SelectedItemCollectionChanged, or setting AutoSuggestMode.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing MultiSelectionComboBox

The MultiSelectionComboBox is a WinForms ComboBox control that supports **multiple item selection** and **auto-suggestion**. Selected items can be displayed as removable tag chips (VisualItem mode), comma-separated text (Delimiter mode), or single-value (Normal mode). It supports data binding via `DataSource`/`DisplayMember`/`ValueMember`, Office-style visual themes, grouping, and a rich event model.

## When to Use This Skill

- Adding a multi-select dropdown to a Windows Forms application
- Displaying selected values as tag chips with close buttons
- Binding the combo box to a `DataTable`, `DataView`, or `ArrayList`
- Configuring auto-suggestion behavior (`AutoSuggestMode`)
- Grouping dropdown items by initial character
- Styling the drop-down list or visual item tags
- Handling selection change and visual item collection events

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly references and NuGet package setup
- Adding the control via Designer or code-behind
- Namespace imports (C# and VB.NET)
- Basic instantiation with `ButtonStyle`, `UseVisualStyle`, and `Size`
- Minimal working example

### Display Modes & Visual Item Input
📄 **Read:** [references/display-modes-and-visual-items.md](references/display-modes-and-visual-items.md)
- `DisplayMode.VisualItem` — tag chips with remove buttons
- `DisplayMode.DelimiterMode` — comma-separated text
- `DisplayMode.NormalMode` — single-value selection
- `AutoSizeMode`: Vertical, Horizontal, None
- `VisualItemInputMode`: DisplayMemberMode, ValueMemberMode, VisualItemMode
- Delimiter character customization

### Data Binding
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- Binding to `ArrayList`, `DataView`, and `DataTable`
- `DataSource`, `DisplayMember`, `ValueMember` properties
- Complete external data source binding example
- `DataSourceChanged` event handling

### Styling & Customization
📄 **Read:** [references/styling-and-customization.md](references/styling-and-customization.md)
- Visual styles: Default, Office2016Colorful, Office2016White, Office2016Black, Office2016DarkGray
- Drop-down styling: `ShowCheckBox`, group header colors, item/drop-down dimensions
- Visual item tag colors: BackColor, ForeColor, SelectionColor, BorderColor
- Grouping: `ShowGroups`, runtime enable/disable
- Auto-suggestion modes: `Begin`, `Match`, `Disabled`

### Events & Advanced Features
📄 **Read:** [references/events-and-advanced-features.md](references/events-and-advanced-features.md)
- `SelectedItemCollectionChanged` — when the selected items change
- `VisualItemCollectionChanged` — when visual item tags are added/removed
- `AutoSizeModeChanged`, `DataSourceChanged`, `DropDown` events
- Detecting VisualItemCollection modifications at runtime

## Quick Start

```csharp
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Windows.Forms;

// Create and configure the control
var combo = new MultiSelectionComboBox();
combo.ButtonStyle = ButtonAppearance.Metro;
combo.UseVisualStyle = true;
combo.Size = new System.Drawing.Size(250, 30);
combo.DisplayMode = DisplayMode.VisualItem;

// Add items manually
combo.Items.AddRange(new object[] { "Apple", "Banana", "Cherry", "Date", "Elderberry" });

this.Controls.Add(combo);
```

```vb
Imports Syncfusion.Windows.Forms.Tools
Imports Syncfusion.Windows.Forms

Dim combo As New MultiSelectionComboBox()
combo.ButtonStyle = ButtonAppearance.Metro
combo.UseVisualStyle = True
combo.Size = New System.Drawing.Size(250, 30)
combo.DisplayMode = DisplayMode.VisualItem
combo.Items.AddRange(New Object() {"Apple", "Banana", "Cherry"})
Me.Controls.Add(combo)
```

## Common Patterns

### Bind to a DataTable and display as tag chips

```csharp
DataTable dt = new DataTable();
dt.Columns.Add("ID");
dt.Columns.Add("Name");
dt.Rows.Add("1", "Alice");
dt.Rows.Add("2", "Bob");
dt.Rows.Add("3", "Carol");

combo.DataSource = dt;
combo.DisplayMember = "Name";
combo.ValueMember = "ID";
combo.DisplayMode = DisplayMode.VisualItem;
```

### React to selection changes

```csharp
combo.SelectedItemCollectionChanged += (sender, e) =>
{
    if (e.Action == Actions.Added)
        Console.WriteLine("Added: " + e.SelectedItems[0]);
    else
        Console.WriteLine("Removed: " + e.SelectedItems[0]);
};
```

### Apply Office 2016 Colorful theme

```csharp
combo.Style = MultiSelectionComboBoxStyle.Office2016Colorful;
```

## Key Properties

| Property | Purpose |
|---|---|
| `DisplayMode` | VisualItem / DelimiterMode / NormalMode |
| `AutoSuggestMode` | Begin / Match / Disabled |
| `AutoSizeMode` | Vertical / Horizontal / None |
| `ShowCheckBox` | Show checkboxes in the dropdown |
| `ShowGroups` | Group items by initial character |
| `DelimiterChar` | Separator character (Delimiter mode) |
| `DataSource` | Bind to ArrayList, DataView, DataTable |
| `DisplayMember` | Property name shown in the control |
| `ValueMember` | Property used as the actual value |
| `Style` | Visual theme (Office2016Colorful, etc.) |
| `VisualItemInputMode` | DisplayMemberMode / ValueMemberMode / VisualItemMode |
