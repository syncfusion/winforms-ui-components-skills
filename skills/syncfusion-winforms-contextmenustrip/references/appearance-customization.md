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

Available styles: `Regular`, `Bold`, `Italic`, `Underline`, `Strikeout`

```csharp
// Individual item fonts
var headerItem = new ToolStripMenuItem("-- Menu Header --");
headerItem.Font = new Font("Arial", 10F, FontStyle.Bold);
headerItem.Enabled = false;  // Non-clickable header
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

- `true` (default): Menu automatically sizes to fit content
- `false`: Menu uses Size property dimensions

Use AutoSize = false for fixed-width menus or custom layouts.

```csharp
// Custom item sizing
contextMenuStripEx.AutoSize = false;
var searchBox = new ToolStripTextBox();
searchBox.Size = new Size(200, 25);
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

```csharp
// Enable check margin for checkable items
contextMenu.ShowCheckMargin = true;
var option1 = new ToolStripMenuItem("Option 1");
option1.CheckOnClick = true;
option1.Checked = true;
```

**Visual Effect:** ShowCheckMargin = true reserves space for check marks on left

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

### Enabling Tooltips

```csharp
// Enable tooltips
contextMenuStripEx.ShowItemToolTips = true;

// Set tooltip text
toolStripMenuItem1.ToolTipText = "Used to create a new file";

// Auto tooltip (uses Text property if ToolTipText empty)
var openItem = new ToolStripMenuItem("Open");
openItem.AutoToolTip = true;
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

**Modes:** Professional (modern, default), System (native Windows), Custom (your implementation)

```csharp
// Custom renderer example
public class CustomToolStripRenderer : ToolStripProfessionalRenderer
{
    protected override void OnRenderMenuItemBackground(ToolStripItemRenderEventArgs e)
    {
        if (e.Item.Selected)
            e.Graphics.FillRectangle(new SolidBrush(Color.FromArgb(51, 153, 255)), e.Item.ContentRectangle);
        else
            base.OnRenderMenuItemBackground(e);
    }
}

contextMenuStripEx.RenderMode = ToolStripRenderMode.Custom;
contextMenuStripEx.Renderer = new CustomToolStripRenderer();
```

## Best Practices

1. **Contrast:** Ensure sufficient contrast (WCAG 4.5:1 minimum) and test in High Contrast mode
2. **Fonts:** Use clear, legible fonts (Segoe UI, Arial) at 9-11pt for desktop
3. **AutoSize:** Keep enabled unless fixed-width required
4. **Tooltips:** Be concise (1-2 sentences) and include keyboard shortcuts
5. **Performance:** Cache brushes in custom renderers; disable shadows on low-end hardware

## Troubleshooting

**Colors not applying:** Check RenderMode (System mode may override); test with Professional  
**Fonts not changing:** Set Font on ContextMenuStripEx, not individual items  
**Size not working:** Set AutoSize = false before setting Size  
**Tooltips not showing:** Enable ShowItemToolTips = true and set ToolTipText on items  
**Check marks not visible:** Set ShowCheckMargin = true on ContextMenuStripEx  
**Shadows not appearing:** Verify DropShadowEnabled = true; check Windows display settings
