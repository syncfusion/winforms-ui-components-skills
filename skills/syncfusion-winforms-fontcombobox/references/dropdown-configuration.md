# DropDown Configuration

Comprehensive guide to customizing FontComboBox dropdown appearance, behavior, and item display for optimal user experience.

## Table of Contents
- [DropDown Styles](#dropdown-styles)
- [DropDown Sizing](#dropdown-sizing)
- [Item Display Configuration](#item-display-configuration)
- [Symbol Font Preview](#symbol-font-preview)
- [Complete Configuration Examples](#complete-configuration-examples)

---

## DropDown Styles

Control how users interact with the FontComboBox using the DropDownStyle property.

### DropDownStyle Property

**Property Type:** `ComboBoxStyle` (enum)  
**Default Value:** `ComboBoxStyle.DropDown`

### Available Styles

| Style | Edit Text | Dropdown | Use Case |
|-------|-----------|----------|----------|
| **DropDown** | ✅ Editable | Click arrow | Type font name or select from list |
| **DropDownList** | ❌ Read-only | Click arrow | Selection only, no typing |
| **Simple** | ✅ Editable | Always visible | Browse while typing |

---

### DropDown Style (Default)

Users can type font names directly or select from dropdown.

**C# Example:**
```csharp
fontComboBox.DropDownStyle = ComboBoxStyle.DropDown;
```

**VB.NET Example:**
```vb
fontComboBox.DropDownStyle = ComboBoxStyle.DropDown
```

**Behavior:**
- Text box is editable
- User can type font name
- Arrow button shows/hides dropdown
- AutoComplete works with typing
- Invalid font names allowed (may cause errors)

**Best For:**
- Advanced users who know font names
- Combined with AutoComplete
- Quick font entry by typing

---

### DropDownList Style (Recommended)

Selection-only mode, prevents invalid font names.

**C# Example:**
```csharp
fontComboBox.DropDownStyle = ComboBoxStyle.DropDownList;
```

**VB.NET Example:**
```vb
fontComboBox.DropDownStyle = ComboBoxStyle.DropDownList
```

**Behavior:**
- Text box is read-only
- User must select from dropdown
- Arrow button or click anywhere opens dropdown
- Prevents invalid font names
- Keyboard navigation works (arrow keys)

**Best For:**
- Ensuring valid font selection
- Preventing user input errors
- Most UI scenarios (recommended)

---

### Simple Style

List is always visible, no dropdown button.

**C# Example:**
```csharp
fontComboBox.DropDownStyle = ComboBoxStyle.Simple;
fontComboBox.Height = 150; // Must set height to show list
```

**VB.NET Example:**
```vb
fontComboBox.DropDownStyle = ComboBoxStyle.Simple
fontComboBox.Height = 150 ' Must set height to show list
```

**Behavior:**
- List portion always visible
- No dropdown arrow button
- Text box is editable
- Control height includes list area

**Best For:**
- Dedicated font selection areas
- When screen space allows permanent list
- Rare use case for FontComboBox

---

## DropDown Sizing

Control the dimensions of the dropdown list for optimal display.

### DropDownHeight Property

Sets the height of the dropdown list in pixels.

**Property Type:** `int`  
**Default Value:** Auto-calculated based on items

**C# Example:**
```csharp
// Set dropdown height to 200 pixels
fontComboBox.DropDownHeight = 200;

// Set to specific item count (ItemHeight * count)
fontComboBox.DropDownHeight = fontComboBox.ItemHeight * 10; // Show 10 items
```

**VB.NET Example:**
```vb
' Set dropdown height to 200 pixels
fontComboBox.DropDownHeight = 200

' Set to specific item count (ItemHeight * count)
fontComboBox.DropDownHeight = fontComboBox.ItemHeight * 10 ' Show 10 items
```

**Tips:**
- Larger height = more visible fonts without scrolling
- Smaller height = compact UI, requires scrolling
- Balance between visibility and screen space

---

### DropDownWidth Property

Sets the width of the dropdown list in pixels.

**Property Type:** `int`  
**Default Value:** Same as control width

**C# Example:**
```csharp
// Set dropdown wider than control for long font names
fontComboBox.Size = new Size(150, 25);
fontComboBox.DropDownWidth = 250; // Dropdown is wider

// Match control width
fontComboBox.DropDownWidth = fontComboBox.Width;
```

**VB.NET Example:**
```vb
' Set dropdown wider than control for long font names
fontComboBox.Size = New Size(150, 25)
fontComboBox.DropDownWidth = 250 ' Dropdown is wider

' Match control width
fontComboBox.DropDownWidth = fontComboBox.Width
```

**Use Cases:**
- **Wider dropdown**: Show full font names when control is narrow
- **Same width**: Consistent visual alignment
- **Auto-adjust**: Calculate based on longest font name

---

### MaxDropDownItems Property

Limits the maximum number of visible items in dropdown.

**Property Type:** `int`  
**Default Value:** 8

**C# Example:**
```csharp
// Show maximum 15 items at once
fontComboBox.MaxDropDownItems = 15;

// Show fewer items (compact dropdown)
fontComboBox.MaxDropDownItems = 5;
```

**VB.NET Example:**
```vb
' Show maximum 15 items at once
fontComboBox.MaxDropDownItems = 15

' Show fewer items (compact dropdown)
fontComboBox.MaxDropDownItems = 5
```

**Behavior:**
- Controls dropdown height automatically
- Scroll bar appears if more items than max
- Does not filter items, only affects display

**Note:** DropDownHeight takes precedence if explicitly set.

---

## Item Display Configuration

Customize how individual font items appear in the dropdown.

### ItemHeight Property

Sets the height of each font item in pixels.

**Property Type:** `int`  
**Default Value:** 13 (Windows default)

**C# Example:**
```csharp
// Taller items for better font preview
fontComboBox.ItemHeight = 24;

// Compact items
fontComboBox.ItemHeight = 16;
```

**VB.NET Example:**
```vb
' Taller items for better font preview
fontComboBox.ItemHeight = 24

' Compact items
fontComboBox.ItemHeight = 16
```

**Recommendations:**
- **18-24px**: Good balance for font preview visibility
- **16px**: Compact, fits more items
- **28-32px**: Large preview for design applications

---

### Sorted Property

Enables alphabetical sorting of fonts in dropdown.

**Property Type:** `bool`  
**Default Value:** `false`

**C# Example:**
```csharp
// Enable alphabetical sorting
fontComboBox.Sorted = true;

// Keep original order (system order)
fontComboBox.Sorted = false;
```

**VB.NET Example:**
```vb
' Enable alphabetical sorting
fontComboBox.Sorted = True

' Keep original order (system order)
fontComboBox.Sorted = False
```

**Why Enable:**
- Easier to find fonts by name
- Consistent ordering across systems
- Improves user experience

**Why Disable:**
- Custom ordering (recently used fonts first)
- Programmatic item insertion order matters

---

## Symbol Font Preview

Control how symbol fonts are displayed in the dropdown.

### ShowSymbolFontPreview Property

Renders symbol fonts with their actual glyphs instead of generic text.

**Property Type:** `bool`  
**Default Value:** `false`

**C# Example:**
```csharp
// Show symbol fonts in their actual typeface
fontComboBox.ShowSymbolFontPreview = true;

// Show symbol fonts as regular text
fontComboBox.ShowSymbolFontPreview = false;
```

**VB.NET Example:**
```vb
' Show symbol fonts in their actual typeface
fontComboBox.ShowSymbolFontPreview = True

' Show symbol fonts as regular text
fontComboBox.ShowSymbolFontPreview = False
```

**Symbol Fonts:**
- Wingdings
- Webdings
- Symbol
- Marlett
- MS Gothic
- Font Awesome (if installed)

**When Enabled:**
- Symbol fonts display with their glyphs
- Visual preview of symbols
- Easier to identify icon fonts

**When Disabled:**
- Symbol fonts show as regular text (often unreadable)
- Faster rendering
- Consistent text display

**Recommendation:** Enable for design tools, disable for text editors.

---

## Complete Configuration Examples

### Example 1: Compact Font Picker

Space-efficient configuration for toolbars or side panels.

```csharp
FontComboBox fontComboBox = new FontComboBox
{
    Size = new Size(150, 23),
    DropDownStyle = ComboBoxStyle.DropDownList,
    DropDownHeight = 150,
    MaxDropDownItems = 8,
    ItemHeight = 18,
    Sorted = true,
    UseAutoComplete = true
};
```

**Characteristics:**
- Narrow control (150px)
- Selection-only (no typing)
- Compact items (18px height)
- Maximum 8 visible items
- Alphabetically sorted

---

### Example 2: Expanded Font Preview

Large preview for design applications.

```csharp
FontComboBox fontComboBox = new FontComboBox
{
    Size = new Size(300, 28),
    DropDownStyle = ComboBoxStyle.DropDownList,
    DropDownHeight = 320,
    DropDownWidth = 350,
    ItemHeight = 28,
    Sorted = true,
    ShowSymbolFontPreview = true,
    UseAutoComplete = true
};
```

**Characteristics:**
- Wide control (300px)
- Even wider dropdown (350px)
- Tall items (28px) for clear preview
- Large dropdown (320px height)
- Symbol fonts displayed with glyphs

---

### Example 3: Quick Search Font Selector

Type-to-search with suggestions.

```csharp
FontComboBox fontComboBox = new FontComboBox
{
    Size = new Size(200, 25),
    DropDownStyle = ComboBoxStyle.DropDown, // Allow typing
    DropDownHeight = 200,
    MaxDropDownItems = 10,
    ItemHeight = 20,
    Sorted = true,
    UseAutoComplete = true,
    AutoCompleteMode = AutoCompleteMode.SuggestAppend,
    AutoCompleteSource = AutoCompleteSource.ListItems
};
```

**Characteristics:**
- Editable text (can type font names)
- AutoComplete with suggestions
- Medium size dropdown
- Sorted for easier search

---

### Example 4: Custom Width Dropdown

Narrow control with wide dropdown for long font names.

```csharp
FontComboBox fontComboBox = new FontComboBox
{
    Size = new Size(120, 23),           // Narrow control
    DropDownWidth = 280,                 // Wide dropdown
    DropDownStyle = ComboBoxStyle.DropDownList,
    DropDownHeight = 180,
    ItemHeight = 20,
    Sorted = true,
    UseAutoComplete = true
};
```

**Use Case:**
- Limited horizontal space (toolbar)
- Font names need more space to display fully
- Dropdown expands beyond control boundaries

---

### Example 5: Dynamic Height Based on Item Count

Calculate dropdown height based on visible items.

```csharp
FontComboBox fontComboBox = new FontComboBox
{
    Size = new Size(200, 25),
    DropDownStyle = ComboBoxStyle.DropDownList,
    ItemHeight = 22,
    Sorted = true,
    UseAutoComplete = true
};

// Set height to show exactly 12 items
int visibleItems = 12;
fontComboBox.DropDownHeight = fontComboBox.ItemHeight * visibleItems;
```

**Benefits:**
- Precise control over visible items
- Consistent spacing
- Adapts to different ItemHeight settings

---

## Configuration Best Practices

### 1. Use DropDownList Style for Safety

```csharp
fontComboBox.DropDownStyle = ComboBoxStyle.DropDownList;
```

**Why:** Prevents invalid font names, ensures type-safe selections.

### 2. Enable Sorting for Better UX

```csharp
fontComboBox.Sorted = true;
```

**Why:** Alphabetical order is intuitive and predictable.

### 3. Adjust ItemHeight for Readability

```csharp
fontComboBox.ItemHeight = 22; // Slightly larger than default
```

**Why:** Larger items improve font preview visibility.

### 4. Set Appropriate DropDownHeight

```csharp
fontComboBox.DropDownHeight = 250; // Show ~10-12 fonts
```

**Why:** Balance between visibility and screen space usage.

### 5. Enable ShowSymbolFontPreview in Design Tools

```csharp
fontComboBox.ShowSymbolFontPreview = true;
```

**Why:** Visual preview of icon/symbol fonts improves selection accuracy.

---

## Performance Considerations

### Large Font Lists (500+ fonts)

**Optimize:**
```csharp
// Use CustomSource with filtered list
fontComboBox.AutoCompleteCustomSource.AddRange(commonFonts);
fontComboBox.AutoCompleteSource = AutoCompleteSource.CustomSource;
```

**Why:** Reduces rendering overhead and improves responsiveness.

### Symbol Font Preview Performance

**Balance:**
```csharp
// Enable only if needed
fontComboBox.ShowSymbolFontPreview = userNeedsSymbolPreview;
```

**Why:** Rendering symbol glyphs is more expensive than text.

---

## Troubleshooting

### Dropdown Too Small

**Solution:**
```csharp
fontComboBox.DropDownHeight = 300;
fontComboBox.MaxDropDownItems = 15;
```

### Font Names Cut Off

**Solution:**
```csharp
fontComboBox.DropDownWidth = fontComboBox.Width + 100; // Add 100px
```

### Items Too Cramped

**Solution:**
```csharp
fontComboBox.ItemHeight = 24; // Increase from default
```

### Dropdown Opens Upward (off-screen)

**Automatic:** ComboBox automatically adjusts direction based on screen space.

**Manual Override:** Not supported; handled by Windows Forms automatically.

---

## Related Topics

- **AutoComplete Features**: Enable fast font search → [autocomplete.md](autocomplete.md)
- **Selection and Events**: Handle dropdown selection → [selection-and-events.md](selection-and-events.md)
- **Visual Styles**: Apply themes to dropdown → [visual-styles.md](visual-styles.md)
