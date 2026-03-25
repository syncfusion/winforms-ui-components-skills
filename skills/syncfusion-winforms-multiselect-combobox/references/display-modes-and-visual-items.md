# Display Modes and Visual Item Configuration

## Table of Contents
- [Display Modes](#display-modes)
  - [VisualItem Mode](#visualitem-mode)
  - [Delimiter Mode](#delimiter-mode)
  - [Normal Mode](#normal-mode)
- [AutoSizeMode](#autosizemode)
- [VisualItemInputMode](#visualiteminputmode)
- [Delimiter Character](#delimiter-character)

---

## Display Modes

The `DisplayMode` property controls how selected items appear in the text area. Choose the mode that best fits your UI requirements.

### VisualItem Mode

Each selected item appears as a **tag chip** with a close (remove) button. This is the most visually rich mode and works well when users need to see and individually remove selections.

```csharp
this.MultiSelectionComboBox1.DisplayMode = DisplayMode.VisualItem;
```

```vb
Me.MultiSelectionComboBox1.DisplayMode = DisplayMode.VisualItem
```

> The tag chips automatically adapt to the control's `AutoSizeMode` setting — the control can grow vertically or horizontally as more items are selected.

### Delimiter Mode

Selected items are shown in the text area as plain text, each **separated by a delimiter character** (default: comma). This is compact and works well in read-heavy layouts.

```csharp
this.MultiSelectionComboBox1.DisplayMode = DisplayMode.DelimiterMode;
```

```vb
Me.MultiSelectionComboBox1.DisplayMode = DisplayMode.DelimiterMode
```

See [Delimiter Character](#delimiter-character) below to customize the separator.

### Normal Mode

Only **one value** can be selected and displayed at a time — behaves like a standard ComboBox. Use this when multi-selection isn't required but you still want the auto-suggestion capability.

```csharp
this.MultiSelectionComboBox1.DisplayMode = DisplayMode.NormalMode;
```

```vb
Me.MultiSelectionComboBox1.DisplayMode = DisplayMode.NormalMode
```

---

## AutoSizeMode

`AutoSizeMode` determines how the control resizes as visual item tags are added. Only relevant when `DisplayMode` is `VisualItem`.

### Vertical (default for tag mode)

The control grows **taller** as more tags are added, wrapping onto new lines.

```csharp
this.MultiSelectionComboBox1.AutoSizeMode = AutoSizeModes.Vertical;
```

```vb
Me.MultiSelectionComboBox1.AutoSizeMode = AutoSizeModes.Vertical
```

### Horizontal

The control grows **wider** as more tags are added, staying on one line.

```csharp
this.MultiSelectionComboBox1.AutoSizeMode = AutoSizeModes.Horizontal;
```

```vb
Me.MultiSelectionComboBox1.AutoSizeMode = AutoSizeModes.Horizontal
```

### None

The control maintains a **fixed size**. Once the visible area fills up, a scrollbar appears so users can navigate through the selected tags.

```csharp
this.MultiSelectionComboBox1.AutoSizeMode = AutoSizeMode.None;
```

```vb
Me.MultiSelectionComboBox1.AutoSizeMode = AutoSizeMode.None
```

---

## VisualItemInputMode

`VisualItemInputMode` controls what text is stored inside each visual item tag when an item is selected from the dropdown. This matters when `DisplayMember` and `ValueMember` differ.

| Mode | Tag Text Source |
|---|---|
| `DisplayMemberMode` | Uses the `DisplayMember` property value (human-readable label) |
| `ValueMemberMode` | Uses the `ValueMember` property value (underlying key/ID) |
| `VisualItemMode` | Custom text — set via end-user requirements |

```csharp
// Show the display label inside the tag chip
this.multiSelectionComboBox1.VisualItemInputMode =
    Syncfusion.Windows.Forms.Tools.VisualItemInputMode.DisplayMemberMode;

// Show the value/key inside the tag chip
this.multiSelectionComboBox1.VisualItemInputMode =
    Syncfusion.Windows.Forms.Tools.VisualItemInputMode.ValueMemberMode;

// Use custom text for each tag chip
this.multiSelectionComboBox1.VisualItemInputMode =
    Syncfusion.Windows.Forms.Tools.VisualItemInputMode.VisualItemMode;
```

```vb
' Show the display label inside the tag chip
Me.multiSelectionComboBox1.VisualItemInputMode =
    Syncfusion.Windows.Forms.Tools.VisualItemInputMode.DisplayMemberMode

' Show the value/key inside the tag chip
Me.multiSelectionComboBox1.VisualItemInputMode =
    Syncfusion.Windows.Forms.Tools.VisualItemInputMode.ValueMemberMode

' Use custom text for each tag chip
Me.multiSelectionComboBox1.VisualItemInputMode =
    Syncfusion.Windows.Forms.Tools.VisualItemInputMode.VisualItemMode
```

---

## Delimiter Character

When `DisplayMode` is `DelimiterMode`, the `DelimiterChar` property sets the separator between displayed values. The delimiter must be a **single special character**.

```csharp
// Use semicolon as separator (default is comma ",")
this.MultiSelectionComboBox1.DelimiterChar = ";";
```

```vb
Me.MultiSelectionComboBox1.DelimiterChar = ";"
```

**Valid examples:** `","` `";"` `"|"` `"."`  
**Not valid:** multi-character strings or alphanumeric characters.
