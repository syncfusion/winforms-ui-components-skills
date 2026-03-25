# Customization and Behavior

## Item Height

Control the pixel height of each row in the font list:

**C#:**
```csharp
this.fontListBox1.ItemHeight = 20;
```

**VB.NET:**
```vb
Me.fontListBox1.ItemHeight = 20
```

> Default value is `15`. Increase it to give more breathing room between font names, or to accommodate larger preview text if rendered via custom drawing.

---

## Sorting

Sort the font list alphabetically by setting `Sorted = true`:

**C#:**
```csharp
this.fontListBox1.Sorted = true;
```

**VB.NET:**
```vb
Me.fontListBox1.Sorted = True
```

> Default is `false` (system-enumeration order). Enabling `Sorted` makes it easier for users to locate a font by name.

---

## Auto-Complete (UseAutoComplete)

Allow users to type characters to jump to the matching font name in the list:

**C#:**
```csharp
this.fontListBox1.UseAutoComplete = true;
```

**VB.NET:**
```vb
Me.fontListBox1.UseAutoComplete = True
```

> When `UseAutoComplete = true`, typing `"Seg"` scrolls to and highlights fonts beginning with "Seg" (e.g., "Segoe UI"). This is especially useful when the list contains hundreds of installed fonts.

---

## Horizontal Scrollbar

By default the FontListBox only shows a vertical scrollbar. Enable a horizontal scrollbar to handle long font names that exceed the control width:

| Property | Description |
|---|---|
| `HorizontalScrollbar` | `true` = show horizontal scrollbar when items overflow the right edge |
| `HorizontalExtent` | Width in pixels of the scrollable content area (must be set for the scrollbar to appear) |

**C#:**
```csharp
this.fontListBox1.HorizontalScrollbar = true;
this.fontListBox1.HorizontalExtent    = 150;
```

**VB.NET:**
```vb
Me.fontListBox1.HorizontalScrollbar = True
Me.fontListBox1.HorizontalExtent    = 150
```

> Set `HorizontalExtent` to a value wider than the control's client width — otherwise the horizontal scrollbar appears but has no range to scroll.

---

## Always-Visible Scrollbars (ScrollAlwaysVisible)

Force the scrollbar(s) to remain visible even when the number of items fits within the control without scrolling:

**C#:**
```csharp
this.fontListBox1.ScrollAlwaysVisible = true;
```

**VB.NET:**
```vb
Me.fontListBox1.ScrollAlwaysVisible = True
```

> Useful when the FontListBox is placed alongside other controls of fixed size — a permanently visible scrollbar prevents layout shifts as the item count changes.

---

## Summary of Behavior Properties

| Property | Default | Description |
|---|---|---|
| `ItemHeight` | `15` | Pixel height per list row |
| `Sorted` | `false` | Alphabetical sort of font names |
| `UseAutoComplete` | `false` | Type-to-match navigation |
| `HorizontalScrollbar` | `false` | Show horizontal scrollbar |
| `HorizontalExtent` | `0` | Scrollable width when `HorizontalScrollbar` is true |
| `ScrollAlwaysVisible` | `false` | Keep scrollbars visible at all times |
