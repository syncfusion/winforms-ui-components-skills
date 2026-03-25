# Appearance Customization

## Table of Contents
- [Overview](#overview)
- [Colors](#colors)
- [Fonts](#fonts)
- [Sizing](#sizing)
- [Margins](#margins)
- [Shadow Effects](#shadow-effects)
- [Tooltips](#tooltips)
- [Render Modes](#render-modes)
- [Best Practices](#best-practices)

## Overview

ContextMenuStripEx provides extensive appearance customization options to match your application's design and branding. You can control colors, fonts, sizing, margins, shadows, tooltips, and rendering styles.

**Customizable Aspects:**
- Background and foreground colors
- Font family, size, and style
- Menu dimensions and AutoSize behavior
- Check and margin visibility
- Drop shadow effects
- Tooltip configuration
- Rendering style (Professional, System, Custom)

## Colors

### Background Color

The `BackColor` property sets the background color of the entire context menu.

**Property:** `BackColor` (Color)  
**Applies to:** ContextMenuStripEx

**Setting Background Color:**

**Via Designer:**
1. Select ContextMenuStripEx in component tray
2. In Properties panel → Appearance section
3. Click **BackColor** dropdown
4. Select from Web, System, or Custom colors

**Via Code:**
```csharp
using System.Drawing;

// Named colors
contextMenuStripEx.BackColor = Color.SkyBlue;
contextMenuStripEx.BackColor = Color.LightGray;
contextMenuStripEx.BackColor = Color.White;

// RGB colors
contextMenuStripEx.BackColor = Color.FromArgb(255, 240, 240);  // Light pink

// Hex colors
contextMenuStripEx.BackColor = ColorTranslator.FromHtml("#F0F0F0");
```

**VB.NET:**
```vb
Imports System.Drawing

' Named colors
contextMenuStripEx.BackColor = Color.SkyBlue
contextMenuStripEx.BackColor = Color.LightGray
contextMenuStripEx.BackColor = Color.White

' RGB colors
contextMenuStripEx.BackColor = Color.FromArgb(255, 240, 240)

' Hex colors
contextMenuStripEx.BackColor = ColorTranslator.FromHtml("#F0F0F0")
```

### Foreground Color

The `ForeColor` property sets the text color for all menu items.

**Property:** `ForeColor` (Color)  
**Applies to:** ContextMenuStripEx

**Setting Foreground Color:**
```csharp
// Set text color for all menu items
contextMenuStripEx.ForeColor = Color.Red;
contextMenuStripEx.ForeColor = Color.DarkBlue;
contextMenuStripEx.ForeColor = Color.FromArgb(50, 50, 50);  // Dark gray
```

**VB.NET:**
```vb
contextMenuStripEx.ForeColor = Color.Red
contextMenuStripEx.ForeColor = Color.DarkBlue
contextMenuStripEx.ForeColor = Color.FromArgb(50, 50, 50)
```

### Individual Item Colors

Override colors for specific menu items:

```csharp
var warningItem = new ToolStripMenuItem("Delete");
warningItem.ForeColor = Color.Red;  // Override menu's ForeColor
warningItem.BackColor = Color.LightYellow;

var normalItem = new ToolStripMenuItem("Copy");
// Uses menu's default colors

contextMenuStripEx.Items.AddRange(new ToolStripItem[] {
    normalItem, warningItem
});
```

### Complete Color Example

```csharp
private ContextMenuStripEx CreateStyledMenu()
{
    var contextMenu = new ContextMenuStripEx();
    
    // Menu colors
    contextMenu.BackColor = Color.FromArgb(45, 45, 48);  // Dark background
    contextMenu.ForeColor = Color.White;  // Light text
    
    // Create items
    var newItem = new ToolStripMenuItem("New");
    var openItem = new ToolStripMenuItem("Open");
    
    var deleteItem = new ToolStripMenuItem("Delete");
    deleteItem.ForeColor = Color.Red;  // Warning color
    
    contextMenu.Items.AddRange(new ToolStripItem[] {
        newItem, openItem, deleteItem
    });
    
    return contextMenu;
}
```

## Fonts

The `Font` property controls the font family, size, and style for all menu items.

**Property:** `Font` (Font)  
**Applies to:** ContextMenuStripEx

### Setting Fonts

**Via Designer:**
1. Select ContextMenuStripEx
2. In Properties panel → Appearance section
3. Click **Font** property ellipsis (...)
4. Choose font family, style, and size in Font dialog

**Via Code:**
```csharp
using System.Drawing;

// Font family and size
contextMenuStripEx.Font = new Font("Arial", 10F);
contextMenuStripEx.Font = new Font("Segoe UI", 9F);
contextMenuStripEx.Font = new Font("Courier New", 8F);

// Font with style
contextMenuStripEx.Font = new Font("Arial", 10F, FontStyle.Bold);
contextMenuStripEx.Font = new Font("Tahoma", 9F, FontStyle.Italic);
contextMenuStripEx.Font = new Font("Courier New", 9F, FontStyle.Strikeout);

// Multiple styles
contextMenuStripEx.Font = new Font("Arial", 10F, 
    FontStyle.Bold | FontStyle.Italic);
```

**VB.NET:**
```vb
Imports System.Drawing

' Font family and size
contextMenuStripEx.Font = New Font("Arial", 10F)
contextMenuStripEx.Font = New Font("Segoe UI", 9F)

' Font with style
contextMenuStripEx.Font = New Font("Arial", 10F, FontStyle.Bold)
contextMenuStripEx.Font = New Font("Tahoma", 9F, FontStyle.Italic)

' Multiple styles
contextMenuStripEx.Font = New Font("Arial", 10F, _
    FontStyle.Bold Or FontStyle.Italic)
```

### FontStyle Options

| Style | Description |
|-------|-------------|
| `FontStyle.Regular` | Normal weight |
| `FontStyle.Bold` | Bold weight |
| `FontStyle.Italic` | Italic slant |
| `FontStyle.Underline` | Underlined text |
| `FontStyle.Strikeout` | Strike-through line |

### Individual Item Fonts

Override fonts for specific items:

```csharp
var headerItem = new ToolStripMenuItem("-- Menu Header --");
headerItem.Font = new Font("Arial", 10F, FontStyle.Bold);
headerItem.Enabled = false;  // Non-clickable header

var normalItem = new ToolStripMenuItem("Normal Item");
// Uses menu's default font

contextMenuStripEx.Items.AddRange(new ToolStripItem[] {
    headerItem, normalItem
});
```

## Sizing

Control the dimensions of the context menu.

### Size Property

**Property:** `Size` (Size)  
**Default:** Auto-sized based on content

**Important:** Set `AutoSize = false` before manually setting Size.

**Setting Menu Size:**
```csharp
using System.Drawing;

// Disable AutoSize
contextMenuStripEx.AutoSize = false;

// Set specific size
contextMenuStripEx.Size = new Size(200, 250);  // Width x Height

// Or set dimensions separately
contextMenuStripEx.Width = 200;
contextMenuStripEx.Height = 250;
```

**VB.NET:**
```vb
Imports System.Drawing

' Disable AutoSize
contextMenuStripEx.AutoSize = False

' Set specific size
contextMenuStripEx.Size = New Size(200, 250)

' Or set dimensions separately
contextMenuStripEx.Width = 200
contextMenuStripEx.Height = 250
```

### AutoSize Behavior

| AutoSize Value | Behavior |
|----------------|----------|
| `true` (default) | Menu automatically sizes to fit content |
| `false` | Menu uses Size property dimensions |

**When to use AutoSize = false:**
- Fixed-width menu required for design consistency
- Prevent menu from growing too large
- Align multiple menus to same width
- Custom layout requirements

### Item Sizing

Individual items can also have custom sizes:

```csharp
// Disable auto-sizing
contextMenuStripEx.AutoSize = false;
contextMenuStripEx.Size = new Size(250, 300);

// TextBox with custom size
var searchBox = new ToolStripTextBox();
searchBox.Size = new Size(200, 25);

// ComboBox with custom size
var filterCombo = new ToolStripComboBox();
filterCombo.Size = new Size(150, 25);

contextMenuStripEx.Items.AddRange(new ToolStripItem[] {
    searchBox, filterCombo
});
```

## Margins

Margins control the space allocation for check marks and images in menu items.

### ShowCheckMargin

**Property:** `ShowCheckMargin` (bool)  
**Default:** `true`  
**Purpose:** Reserves space for check marks before menu item text

**Important:** Check marks only display if this property is `true`.

**Setting Check Margin:**
```csharp
// Enable check margin (required for check marks to display)
contextMenuStripEx.ShowCheckMargin = true;

// Disable check margin (saves space if not using checked items)
contextMenuStripEx.ShowCheckMargin = false;
```

**VB.NET:**
```vb
' Enable check margin
contextMenuStripEx.ShowCheckMargin = True

' Disable check margin
contextMenuStripEx.ShowCheckMargin = False
```

### Complete Margin Example

```csharp
private ContextMenuStripEx CreateMenuWithMargins()
{
    var contextMenu = new ContextMenuStripEx();
    
    // Enable check margin for checkable items
    contextMenu.ShowCheckMargin = true;
    
    // Create checkable items
    var option1 = new ToolStripMenuItem("Option 1");
    option1.CheckOnClick = true;
    option1.Checked = true;
    
    var option2 = new ToolStripMenuItem("Option 2");
    option2.CheckOnClick = true;
    
    contextMenu.Items.AddRange(new ToolStripItem[] {
        option1, option2
    });
    
    return contextMenu;
}
```

**Visual Effect:**
- `ShowCheckMargin = true`: Space reserved on left for check marks
- `ShowCheckMargin = false`: Items align flush left, no check mark space

## Shadow Effects

Add depth with drop shadows.

### DropShadowEnabled

**Property:** `DropShadowEnabled` (bool)  
**Default:** `true`  
**Purpose:** Displays a three-dimensional shadow behind the context menu

**Setting Drop Shadow:**
```csharp
// Enable drop shadow (default)
contextMenuStripEx.DropShadowEnabled = true;

// Disable drop shadow (flat appearance)
contextMenuStripEx.DropShadowEnabled = false;
```

**VB.NET:**
```vb
' Enable drop shadow
contextMenuStripEx.DropShadowEnabled = True

' Disable drop shadow
contextMenuStripEx.DropShadowEnabled = False
```

**When to disable:**
- Minimalist flat design aesthetic
- Performance on older hardware
- High contrast or accessibility modes
- Custom shadow implementation

## Tooltips

Tooltips provide helpful hints when users hover over menu items.

### Tooltip Properties

| Property | Applies To | Description |
|----------|------------|-------------|
| `ShowItemToolTips` | ContextMenuStripEx | Enable/disable tooltips for all items |
| `ToolTipText` | Individual items | Custom tooltip text for specific item |
| `AutoToolTip` | Individual items | Use Text property as tooltip if ToolTipText is empty |

### Enabling Tooltips

**Step 1: Enable for Menu**
```csharp
// Enable tooltips for the entire menu
contextMenuStripEx.ShowItemToolTips = true;
```

**Step 2: Set Tooltip Text for Items**
```csharp
// Custom tooltip text
toolStripMenuItem1.ToolTipText = "Used to create a new file";
toolStripTextBox1.ToolTipText = "Used to provide editable text";
toolStripComboBox1.ToolTipText = "Used to provide a collection of items";
```

**VB.NET:**
```vb
' Enable tooltips
contextMenuStripEx.ShowItemToolTips = True

' Set tooltip text
toolStripMenuItem1.ToolTipText = "Used to create a new file"
toolStripTextBox1.ToolTipText = "Used to provide editable text"
toolStripComboBox1.ToolTipText = "Used to provide a collection of items"
```

### AutoToolTip Behavior

**Property:** `AutoToolTip` (bool)  
**Default:** `false`

| Value | Behavior |
|-------|----------|
| `false` | Displays `ToolTipText` property value |
| `true` | Displays `Text` property if `ToolTipText` is empty |

**Example:**
```csharp
contextMenuStripEx.ShowItemToolTips = true;

// Explicit tooltip
var item1 = new ToolStripMenuItem("Save");
item1.ToolTipText = "Save the current document";
// Tooltip shows: "Save the current document"

// Auto tooltip
var item2 = new ToolStripMenuItem("Open");
item2.AutoToolTip = true;
// Tooltip shows: "Open" (uses Text property)

// Auto tooltip with explicit text (explicit takes precedence)
var item3 = new ToolStripMenuItem("Close");
item3.AutoToolTip = true;
item3.ToolTipText = "Close the active window";
// Tooltip shows: "Close the active window"
```

### Complete Tooltip Example

```csharp
private ContextMenuStripEx CreateMenuWithTooltips()
{
    var contextMenu = new ContextMenuStripEx();
    
    // Enable tooltips
    contextMenu.ShowItemToolTips = true;
    
    // Item with custom tooltip
    var newItem = new ToolStripMenuItem("New");
    newItem.ToolTipText = "Create a new document (Ctrl+N)";
    newItem.ShortcutKeys = Keys.Control | Keys.N;
    
    // Item with auto tooltip
    var openItem = new ToolStripMenuItem("Open");
    openItem.AutoToolTip = true;  // Uses "Open" as tooltip
    
    // TextBox with helpful tooltip
    var searchBox = new ToolStripTextBox();
    searchBox.ToolTipText = "Enter search terms and press Enter";
    
    contextMenu.Items.AddRange(new ToolStripItem[] {
        newItem, openItem, searchBox
    });
    
    return contextMenu;
}
```

## Render Modes

Render modes control the visual styling of the context menu.

### RenderMode Property

**Property:** `RenderMode` (ToolStripRenderMode enum)  
**Options:**
- `Professional` (default) - Modern, smooth styling
- `System` - Windows native appearance
- `Custom` - Use custom ToolStripRenderer

**Setting Render Mode:**
```csharp
// Professional mode (default - smooth, modern)
contextMenuStripEx.RenderMode = ToolStripRenderMode.Professional;

// System mode (native Windows appearance)
contextMenuStripEx.RenderMode = ToolStripRenderMode.System;

// Custom mode (requires custom renderer)
contextMenuStripEx.RenderMode = ToolStripRenderMode.Custom;
contextMenuStripEx.Renderer = new CustomToolStripRenderer();
```

**VB.NET:**
```vb
' Professional mode
contextMenuStripEx.RenderMode = ToolStripRenderMode.Professional

' System mode
contextMenuStripEx.RenderMode = ToolStripRenderMode.System

' Custom mode
contextMenuStripEx.RenderMode = ToolStripRenderMode.Custom
contextMenuStripEx.Renderer = New CustomToolStripRenderer()
```

### Render Mode Comparison

| Mode | Appearance | Use When |
|------|------------|----------|
| Professional | Modern, smooth gradients | Default for most applications |
| System | Native Windows style | Matching OS appearance exactly |
| Custom | Your implementation | Brand-specific styling needed |

### Custom Renderer Example

```csharp
// Create custom renderer
public class CustomToolStripRenderer : ToolStripProfessionalRenderer
{
    protected override void OnRenderMenuItemBackground(ToolStripItemRenderEventArgs e)
    {
        if (e.Item.Selected)
        {
            // Custom highlight color
            e.Graphics.FillRectangle(
                new SolidBrush(Color.FromArgb(51, 153, 255)),
                e.Item.ContentRectangle
            );
        }
        else
        {
            base.OnRenderMenuItemBackground(e);
        }
    }
}

// Apply custom renderer
contextMenuStripEx.RenderMode = ToolStripRenderMode.Custom;
contextMenuStripEx.Renderer = new CustomToolStripRenderer();
```

## Best Practices

### Color Best Practices

1. **Contrast:** Ensure sufficient contrast between background and text (WCAG 4.5:1 minimum)
2. **Consistency:** Use colors that match your application theme
3. **Accessibility:** Test in High Contrast mode
4. **Readability:** Dark text on light background or vice versa
5. **Highlighting:** Use distinct colors for warning/danger items

### Font Best Practices

1. **Readability:** Use clear, legible fonts (Segoe UI, Arial, Tahoma)
2. **Size:** 9-11pt for desktop applications
3. **Consistency:** Match font with application UI
4. **Touch:** Larger fonts (11-13pt) for touch interfaces
5. **Avoid:** Decorative or script fonts for menu items

### Sizing Best Practices

1. **AutoSize:** Keep enabled unless specific requirements dictate otherwise
2. **Minimum width:** Ensure menu is wide enough for longest item
3. **Touch targets:** 44x44 pixels minimum for touch devices
4. **Screen boundaries:** Menu should fit within screen bounds

### Tooltip Best Practices

1. **Be concise:** 1-2 sentences maximum
2. **Add value:** Provide information not obvious from item text
3. **Include shortcuts:** Mention keyboard shortcuts in tooltips
4. **Consistent style:** Use similar phrasing across all tooltips
5. **Test visibility:** Ensure tooltips appear and disappear correctly

### Performance Considerations

1. **Avoid:** Expensive operations in render events
2. **Custom renderers:** Cache brushes and pens, don't recreate each paint
3. **Large menus:** Consider virtualization or pagination
4. **Shadows:** Disable on lower-end hardware if needed

## Troubleshooting

**Colors not applying:**
- Check RenderMode (System mode may override colors)
- Verify no custom renderer is overriding
- Test with RenderMode = Professional

**Fonts not changing:**
- Ensure Font property is set on ContextMenuStripEx, not individual items
- Check that AutoSize allows font to display properly
- Verify font is installed on system

**Size not working:**
- Set AutoSize = false before setting Size
- Check that content isn't forcing size larger
- Verify no parent container is constraining size

**Tooltips not showing:**
- Enable ShowItemToolTips = true on ContextMenuStripEx
- Verify ToolTipText is set on items
- Check mouse hover duration is sufficient
- Ensure items are enabled

**Check marks not visible:**
- Set ShowCheckMargin = true on ContextMenuStripEx
- Verify Checked = true on menu items
- Check that menu width accommodates check mark space

**Shadows not appearing:**
- Verify DropShadowEnabled = true
- Check Windows display settings (shadows may be disabled OS-wide)
- Test on different hardware (some systems don't support shadows)
