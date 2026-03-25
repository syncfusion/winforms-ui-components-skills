# Layout Customization in TileLayout

## Table of Contents
- [Overview](#overview)
- [Alignment](#alignment)
- [Horizontal Margins](#horizontal-margins)
  - [HorzNearMargin (Left Margin)](#horznearmargin-left-margin)
  - [HorzFarMargin (Right Margin)](#horzfarmargin-right-margin)
- [Vertical Margins](#vertical-margins)
  - [TopMargin](#topmargin)
  - [BottomMargin](#bottommargin)
- [ReverseRows](#reverserows)
- [Complete Layout Example](#complete-layout-example)
- [Common Layout Patterns](#common-layout-patterns)

## Overview

TileLayout provides extensive layout customization through the **MainLayout** property. This property is a `FlowLayout` manager that controls:

- **Alignment:** Horizontal positioning of tile groups (Near, Center, Far)
- **Margins:** Spacing between client rectangle and layout rectangle
- **Flow Direction:** Normal or reversed row ordering

All layout customization is done through `tileLayout.MainLayout` properties.

## Alignment

The **Alignment** property controls where tile groups are positioned horizontally within the TileLayout container.

**Property:** `tileLayout1.MainLayout.Alignment`  
**Type:** `Syncfusion.Windows.Forms.Tools.FlowAlignment`  
**Values:** `Near`, `Center`, `Far`

### Near Alignment (Left)

Positions tile groups at the left side of the container:

```csharp
// Align tile groups to the left
tileLayout1.MainLayout.Alignment = Syncfusion.Windows.Forms.Tools.FlowAlignment.Near;
```

```vb
' Align tile groups to the left
tileLayout1.MainLayout.Alignment = Syncfusion.Windows.Forms.Tools.FlowAlignment.Near
```

![Near Alignment](images/NearAlignment.png)

**When to use:** Standard left-aligned layouts, default appearance for most applications.

### Center Alignment

Centers tile groups horizontally within the container:

```csharp
// Center tile groups horizontally
tileLayout1.MainLayout.Alignment = Syncfusion.Windows.Forms.Tools.FlowAlignment.Center;
```

```vb
' Center tile groups horizontally
tileLayout1.MainLayout.Alignment = Syncfusion.Windows.Forms.Tools.FlowAlignment.Center
```

![Center Alignment](images/CenterAlignment.png)

**When to use:** Balanced, symmetrical layouts; dashboards; presentation-style interfaces.

### Far Alignment (Right)

Positions tile groups at the right side of the container:

```csharp
// Align tile groups to the right
tileLayout1.MainLayout.Alignment = Syncfusion.Windows.Forms.Tools.FlowAlignment.Far;
```

```vb
' Align tile groups to the right
tileLayout1.MainLayout.Alignment = Syncfusion.Windows.Forms.Tools.FlowAlignment.Far
```

![Far Alignment](images/FarAlignment.png)

**When to use:** Right-to-left (RTL) languages; specialized UI designs; side-panel layouts.

## Horizontal Margins

Horizontal margins control the left and right spacing between the TileLayout's client area and the layout rectangle where tiles are rendered.

### HorzNearMargin (Left Margin)

**HorzNearMargin** sets the left margin in pixels.

**Property:** `tileLayout1.MainLayout.HorzNearMargin`  
**Type:** `int`  
**Default:** 0

```csharp
// Set 100-pixel left margin
tileLayout1.MainLayout.HorzNearMargin = 100;
```

```vb
' Set 100-pixel left margin
tileLayout1.MainLayout.HorzNearMargin = 100
```

![Horizontal Near Margin](images/HorNearMargin.png)

**When to use:**
- Add breathing room on the left side
- Align with other UI elements that have left padding
- Create visual separation from window edge

**Example - Consistent left padding:**
```csharp
// Match left margin with other form controls
tileLayout1.MainLayout.HorzNearMargin = 20;
```

### HorzFarMargin (Right Margin)

**HorzFarMargin** sets the right margin in pixels.

**Property:** `tileLayout1.MainLayout.HorzFarMargin`  
**Type:** `int`  
**Default:** 0

```csharp
// Set 100-pixel right margin
tileLayout1.MainLayout.HorzFarMargin = 100;
```

```vb
' Set 100-pixel right margin
tileLayout1.MainLayout.HorzFarMargin = 100
```

![Horizontal Far Margin](images/HorFarMargin.png)

**When to use:**
- Prevent tiles from touching right window edge
- Balance with left margin for centered appearance
- Accommodate scrollbar when present

**Example - Symmetrical horizontal margins:**
```csharp
// Equal left and right margins
tileLayout1.MainLayout.HorzNearMargin = 50;
tileLayout1.MainLayout.HorzFarMargin = 50;
```

## Vertical Margins

Vertical margins control the top and bottom spacing between the TileLayout's client area and the layout rectangle.

### TopMargin

**TopMargin** sets the top margin in pixels.

**Property:** `tileLayout1.MainLayout.TopMargin`  
**Type:** `int`  
**Default:** 0

```csharp
// Set 20-pixel top margin
tileLayout1.MainLayout.TopMargin = 20;
```

```vb
' Set 20-pixel top margin
tileLayout1.MainLayout.TopMargin = 20
```

![Top Margin](images/TopMargin.png)

**When to use:**
- Space below title bars or headers
- Separate from toolbar controls above
- Add visual breathing room at top

**Example - Header spacing:**
```csharp
// Add space for custom header
tileLayout1.MainLayout.TopMargin = 60;
```

### BottomMargin

**BottomMargin** sets the bottom margin in pixels.

**Property:** `tileLayout1.MainLayout.BottomMargin`  
**Type:** `int`  
**Default:** 0

```csharp
// Set 100-pixel bottom margin
tileLayout1.MainLayout.BottomMargin = 100;
```

```vb
' Set 100-pixel bottom margin
tileLayout1.MainLayout.BottomMargin = 100
```

![Bottom Margin](images/BottomMargin.png)

**When to use:**
- Space above status bars or footers
- Prevent tiles from touching bottom edge
- Add scrolling buffer at bottom

**Example - Status bar clearance:**
```csharp
// Space for status bar (30px height + 10px padding)
tileLayout1.MainLayout.BottomMargin = 40;
```

## ReverseRows

The **ReverseRows** property reverses the order in which rows are laid out.

**Property:** `tileLayout1.MainLayout.ReverseRows`  
**Type:** `bool`  
**Default:** `false`

**Behavior:**
- If **LayoutMode = Horizontal**: Lays out rows from **top to bottom** (reversed from default)
- If **LayoutMode = Vertical**: Lays out rows from **left to right** (reversed from default)

```csharp
// Reverse row layout direction
tileLayout1.MainLayout.ReverseRows = true;
```

```vb
' Reverse row layout direction
tileLayout1.MainLayout.ReverseRows = True
```

![Reverse Rows](images/ReverseRows.png)

**When to use:**
- Right-to-left (RTL) language support
- Specialized UI flow requirements
- Bottom-up or right-to-left tile ordering

**Example - RTL layout:**
```csharp
// Configure for right-to-left languages
tileLayout1.MainLayout.Alignment = FlowAlignment.Far;
tileLayout1.MainLayout.ReverseRows = true;
```

## Complete Layout Example

Here's a comprehensive example using all layout customization options:

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class CustomLayoutExample : Form
{
    private TileLayout tileLayout1;
    
    public CustomLayoutExample()
    {
        SetupCustomLayout();
    }
    
    private void SetupCustomLayout()
    {
        // Create TileLayout
        tileLayout1 = new TileLayout();
        tileLayout1.Dock = DockStyle.Fill;
        tileLayout1.ShowGroupTitle = true;
        
        // Configure centered layout with margins
        tileLayout1.MainLayout.Alignment = FlowAlignment.Center;
        tileLayout1.MainLayout.HorzNearMargin = 40;
        tileLayout1.MainLayout.HorzFarMargin = 40;
        tileLayout1.MainLayout.TopMargin = 30;
        tileLayout1.MainLayout.BottomMargin = 30;
        tileLayout1.MainLayout.ReverseRows = false;
        
        // Set background color
        tileLayout1.IgnoreThemeBackground = true;
        tileLayout1.BackColor = Color.FromArgb(240, 240, 240);
        
        // Create groups with tiles
        CreateTileGroups();
        
        // Add to form
        this.Controls.Add(tileLayout1);
        this.Text = "Custom Layout Example";
        this.Size = new Size(1000, 700);
        this.BackColor = Color.White;
    }
    
    private void CreateTileGroups()
    {
        // Create "Dashboard" group
        LayoutGroup dashboardGroup = new LayoutGroup();
        dashboardGroup.Text = "Dashboard";
        dashboardGroup.BackColor = Color.FromArgb(0, 120, 215);
        
        for (int i = 1; i <= 6; i++)
        {
            ImageStreamer tile = new ImageStreamer();
            tile.Images.Add(CreatePlaceholderImage($"Tile {i}"));
            tile.InternalBackColor = Color.White;
            dashboardGroup.Controls.Add(tile);
        }
        
        // Create "Tools" group
        LayoutGroup toolsGroup = new LayoutGroup();
        toolsGroup.Text = "Tools";
        toolsGroup.BackColor = Color.FromArgb(16, 124, 16);
        
        for (int i = 1; i <= 4; i++)
        {
            ImageStreamer tile = new ImageStreamer();
            tile.Images.Add(CreatePlaceholderImage($"Tool {i}"));
            tile.InternalBackColor = Color.White;
            toolsGroup.Controls.Add(tile);
        }
        
        tileLayout1.Controls.Add(dashboardGroup);
        tileLayout1.Controls.Add(toolsGroup);
    }
    
    private Image CreatePlaceholderImage(string text)
    {
        Bitmap bmp = new Bitmap(150, 150);
        using (Graphics g = Graphics.FromImage(bmp))
        {
            g.FillRectangle(Brushes.LightGray, 0, 0, 150, 150);
            g.DrawString(text, new Font("Segoe UI", 12), Brushes.Black, 
                new PointF(10, 60));
        }
        return bmp;
    }
}
```

**Result:** A centered tile layout with 40-pixel horizontal margins, 30-pixel vertical margins, and two tile groups.

## Common Layout Patterns

### Full-Width Layout (No Margins)

```csharp
// Tiles extend to window edges
tileLayout1.MainLayout.Alignment = FlowAlignment.Near;
tileLayout1.MainLayout.HorzNearMargin = 0;
tileLayout1.MainLayout.HorzFarMargin = 0;
tileLayout1.MainLayout.TopMargin = 0;
tileLayout1.MainLayout.BottomMargin = 0;
```

**Use case:** Maximize screen space; immersive full-screen experiences.

### Balanced Centered Layout

```csharp
// Centered with equal margins on all sides
tileLayout1.MainLayout.Alignment = FlowAlignment.Center;
tileLayout1.MainLayout.HorzNearMargin = 50;
tileLayout1.MainLayout.HorzFarMargin = 50;
tileLayout1.MainLayout.TopMargin = 40;
tileLayout1.MainLayout.BottomMargin = 40;
```

**Use case:** Dashboard applications; presentation-style UIs; modern aesthetics.

### Left-Aligned with Padding

```csharp
// Left-aligned with consistent padding
tileLayout1.MainLayout.Alignment = FlowAlignment.Near;
tileLayout1.MainLayout.HorzNearMargin = 20;
tileLayout1.MainLayout.HorzFarMargin = 20;
tileLayout1.MainLayout.TopMargin = 20;
tileLayout1.MainLayout.BottomMargin = 20;
```

**Use case:** Standard application layout; breathing room around content.

### Custom Header/Footer Space

```csharp
// Extra top/bottom margins for header/footer
tileLayout1.MainLayout.TopMargin = 80;      // Header space
tileLayout1.MainLayout.BottomMargin = 50;   // Footer/status bar space
tileLayout1.MainLayout.HorzNearMargin = 20;
tileLayout1.MainLayout.HorzFarMargin = 20;
```

**Use case:** Applications with custom headers, toolbars, status bars.

### Responsive Margin Adjustment

Adjust margins based on form size:

```csharp
// In form Resize event
private void Form1_Resize(object sender, EventArgs e)
{
    int margin = Math.Max(20, (this.ClientSize.Width - 1200) / 2);
    tileLayout1.MainLayout.HorzNearMargin = margin;
    tileLayout1.MainLayout.HorzFarMargin = margin;
}
```

**Use case:** Adaptive layouts; centered content with max-width constraint.

## Layout Best Practices

1. **Consistent Margins:** Use equal left/right margins for balanced appearance
2. **Adequate Spacing:** Minimum 10-20 pixels prevents tiles from touching edges
3. **Consider Scrollbars:** Add extra right margin if scrollbar may appear
4. **Theme-Aware:** Set IgnoreThemeBackground=true before applying custom BackColor
5. **Test Resizing:** Verify layout works at different form sizes
6. **Accessibility:** Ensure adequate margins for touch targets (min 40-50px for touch)

## Troubleshooting

**Issue:** Tiles appear cut off at edges
- **Solution:** Increase HorzNearMargin/HorzFarMargin or TopMargin/BottomMargin

**Issue:** Layout not centered despite Alignment = Center
- **Solution:** Ensure form size is large enough; reduce tile group count if necessary

**Issue:** Margins not applying
- **Solution:** Check MainLayout property is not null; set margins after TileLayout initialization

**Issue:** Layout changes not visible
- **Solution:** Call `tileLayout1.Refresh()` after changing margin properties

## Summary

Layout customization in TileLayout provides precise control over tile positioning through:

- **Alignment:** Near (left), Center, Far (right)
- **Margins:** HorzNearMargin, HorzFarMargin, TopMargin, BottomMargin
- **Flow:** ReverseRows for alternate layout directions

These properties enable you to create professional, well-spaced tile layouts that match your application's design requirements.
