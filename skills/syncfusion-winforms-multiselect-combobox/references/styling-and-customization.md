# Styling and Customization

## Table of Contents
- [Visual Styles](#visual-styles)
- [Drop-Down Styling](#drop-down-styling)
  - [Checkbox Selection](#checkbox-selection)
  - [Group Header Colors](#group-header-colors)
  - [Dimensions](#dimensions)
- [Visual Item Tag Styling](#visual-item-tag-styling)
- [Grouping](#grouping)
- [Auto-Suggestion Modes](#auto-suggestion-modes)

---

## Visual Styles

The `Style` property applies a complete Office-style visual theme to the control. The available themes are:

| Style Value | Appearance |
|---|---|
| `Default` | Windows system default |
| `Office2016Colorful` | Office 2016 Colorful (blue accent) |
| `Office2016White` | Office 2016 White |
| `Office2016Black` | Office 2016 Black (dark) |
| `Office2016DarkGray` | Office 2016 Dark Gray |

```csharp
this.multiSelectionComboBox1.Style =
    Syncfusion.Windows.Forms.Tools.MultiSelectionComboBoxStyle.Office2016Colorful;
```

```vb
Me.multiSelectionComboBox1.Style =
    Syncfusion.Windows.Forms.Tools.MultiSelectionComboBoxStyle.Office2016Colorful
```

---

## Drop-Down Styling

### Checkbox Selection

Enable checkboxes next to each item in the dropdown list to make multi-selection more discoverable:

```csharp
this.MultiSelectionComboBox1.ShowCheckBox = true;
```

```vb
Me.MultiSelectionComboBox1.ShowCheckBox = True
```

### Group Header Colors

When grouping is enabled (`ShowGroups = true`), the group header appearance can be customized:

```csharp
// Group header background
this.MultiSelectionComboBox1.GroupHeaderBackColor = Color.LightSteelBlue;

// Group header text color
this.MultiSelectionComboBox1.GroupHeaderForeColor = Color.DarkBlue;

// Separator line below the group header
this.MultiSelectionComboBox1.GroupHeaderSeperatorColor = Color.SteelBlue;
```

```vb
Me.MultiSelectionComboBox1.GroupHeaderBackColor = Color.LightSteelBlue
Me.MultiSelectionComboBox1.GroupHeaderForeColor = Color.DarkBlue
Me.MultiSelectionComboBox1.GroupHeaderSeperatorColor = Color.SteelBlue
```

### Dimensions

Control the sizes of the dropdown window and its items independently:

```csharp
// Height of each item row in the dropdown
this.MultiSelectionComboBox1.ItemsHeight = 25;

// Maximum visible items before scrolling
this.MultiSelectionComboBox1.MaxDropDownItems = 5;

// Dropdown window height (pixels)
this.MultiSelectionComboBox1.DropDownHeight = 150;

// Dropdown window width (pixels)
this.MultiSelectionComboBox1.DropDownWidth = 200;
```

```vb
Me.MultiSelectionComboBox1.ItemsHeight = 25
Me.MultiSelectionComboBox1.MaxDropDownItems = 5
Me.MultiSelectionComboBox1.DropDownHeight = 150
Me.MultiSelectionComboBox1.DropDownWidth = 200
```

---

## Visual Item Tag Styling

When `DisplayMode` is `VisualItem`, each selected item is shown as a tag chip. Customize the tag appearance with these properties:

```csharp
// Tag chip background color
this.MultiSelectionComboBox1.VisualItemBackColor = Color.AliceBlue;

// Tag chip text color
this.MultiSelectionComboBox1.VisualItemForeColor = Color.DarkSlateBlue;

// Tag chip background when selected/hovered
this.MultiSelectionComboBox1.VisualItemSelectionColor = Color.SteelBlue;

// Tag chip border color
this.MultiSelectionComboBox1.VisualItemBorderColor = Color.SlateBlue;
```

```vb
Me.MultiSelectionComboBox1.VisualItemBackColor = Color.AliceBlue
Me.MultiSelectionComboBox1.VisualItemForeColor = Color.DarkSlateBlue
Me.MultiSelectionComboBox1.VisualItemSelectionColor = Color.SteelBlue
Me.MultiSelectionComboBox1.VisualItemBorderColor = Color.SlateBlue
```

---

## Grouping

MultiSelectionComboBox can group dropdown items by their **initial character**. This helps users navigate long lists.

### Enable Grouping

```csharp
this.MultiSelectionComboBox1.ShowGroups = true;
```

```vb
Me.MultiSelectionComboBox1.ShowGroups = True
```

### Disable Grouping at Runtime

Grouping can be toggled at any point — for example, in response to a checkbox or button click:

```csharp
this.MultiSelectionComboBox1.ShowGroups = false;
```

```vb
Me.MultiSelectionComboBox1.ShowGroups = False
```

---

## Auto-Suggestion Modes

`AutoSuggestMode` controls how the dropdown filters items as the user types.

### Begin — Match from Start

Lists items whose text **starts with** the typed characters. Fastest for alphabetically ordered lists.

```csharp
this.MultiSelectionComboBox1.AutoSuggestMode = AutoSuggestMode.Begin;
```

```vb
Me.MultiSelectionComboBox1.AutoSuggestMode = AutoSuggestMode.Begin
```

### Match — Match Anywhere

Lists **all items** that contain the typed characters anywhere in their text. More flexible for large or unsorted lists.

```csharp
this.MultiSelectionComboBox1.AutoSuggestMode = AutoSuggestMode.Match;
```

```vb
Me.MultiSelectionComboBox1.AutoSuggestMode = AutoSuggestMode.Match
```

### Disabled — No Auto-Suggestion

Turns off auto-suggestion completely. Users must scroll the dropdown to find items.

```csharp
this.MultiSelectionComboBox1.AutoCompleteMatchMode = AutoCompleteMatchMode.Disabled;
```

```vb
Me.MultiSelectionComboBox1.AutoCompleteMatchMode = AutoCompleteMatchMode.Disabled
```
