# Customization and Layout

## Table of Contents
- [Overview](#overview)
- [Color Item Sizing](#color-item-sizing)
- [Spacing Between Items](#spacing-between-items)
- [Header Text Alignment](#header-text-alignment)
- [Font Customization](#font-customization)
- [Design-Time Color Editing](#design-time-color-editing)
- [Complete Customization Examples](#complete-customization-examples)

## Overview

ColorPickerUIAdv provides extensive customization options for layout, spacing, and appearance of color items and group headers.

**Customization Categories:**
- Color item dimensions
- Horizontal and vertical spacing
- Header text alignment
- Font styling for headers
- Design-time color editing

## Color Item Sizing

The `ColorItemSize` property controls the dimensions of individual color items (the color squares).

### ColorItemSize Property

```csharp
// Default size (13x13 pixels)
colorPickerUIAdv1.ColorItemSize = new Size(13, 13);

// Larger items (20x20 pixels)
colorPickerUIAdv1.ColorItemSize = new Size(20, 20);

// Extra large items (30x30 pixels)
colorPickerUIAdv1.ColorItemSize = new Size(30, 30);

// Rectangular items (different width/height)
colorPickerUIAdv1.ColorItemSize = new Size(25, 15);
```

**Visual Basic:**
```vb
' Default size
colorPickerUIAdv1.ColorItemSize = New Size(13, 13)

' Larger items
colorPickerUIAdv1.ColorItemSize = New Size(20, 20)
```

### Size Guidelines

| Size | Use Case | Appearance |
|------|----------|------------|
| **13x13** | Default, compact layouts | Standard Office-style |
| **15x15** | Slightly larger, better visibility | Comfortable |
| **20x20** | Touch-friendly, prominent | Large |
| **25x25+** | Presentation, demos | Extra large |

### Example: Touch-Friendly Size

```csharp
private void ConfigureTouchFriendlySize()
{
    // Larger items for touch interfaces
    colorPickerUIAdv1.ColorItemSize = new Size(28, 28);
    
    // Adjust control size accordingly
    colorPickerUIAdv1.Size = new Size(280, 250);
}
```

### Example: Compact Layout

```csharp
private void ConfigureCompactLayout()
{
    // Smaller items for space-constrained UIs
    colorPickerUIAdv1.ColorItemSize = new Size(10, 10);
    
    // Reduce spacing
    colorPickerUIAdv1.HorizontalItemsSpacing = 2;
    colorPickerUIAdv1.VerticalItemsSpacing = 2;
    
    // Compact control size
    colorPickerUIAdv1.Size = new Size(180, 150);
}
```

## Spacing Between Items

Control horizontal and vertical spacing between color items for visual organization.

### HorizontalItemsSpacing Property

Sets the horizontal distance between color items (default: 4 pixels).

```csharp
// Default spacing
colorPickerUIAdv1.HorizontalItemsSpacing = 4;

// Tighter spacing
colorPickerUIAdv1.HorizontalItemsSpacing = 2;

// Wider spacing
colorPickerUIAdv1.HorizontalItemsSpacing = 10;

// Extra wide spacing
colorPickerUIAdv1.HorizontalItemsSpacing = 15;

// No spacing
colorPickerUIAdv1.HorizontalItemsSpacing = 0;
```

### VerticalItemsSpacing Property

Sets the vertical distance between color item rows (default: 0 pixels).

```csharp
// Default spacing (no vertical gap)
colorPickerUIAdv1.VerticalItemsSpacing = 0;

// Add vertical breathing room
colorPickerUIAdv1.VerticalItemsSpacing = 5;

// Significant vertical separation
colorPickerUIAdv1.VerticalItemsSpacing = 10;

// Large vertical gaps
colorPickerUIAdv1.VerticalItemsSpacing = 15;
```

**Visual Basic:**
```vb
' Set horizontal spacing
colorPickerUIAdv1.HorizontalItemsSpacing = 10

' Set vertical spacing
colorPickerUIAdv1.VerticalItemsSpacing = 8
```

### Spacing Impact on Layout

```
HorizontalItemsSpacing = 4, VerticalItemsSpacing = 0:
■ ■ ■ ■ ■
■ ■ ■ ■ ■

HorizontalItemsSpacing = 10, VerticalItemsSpacing = 5:
■   ■   ■   ■   ■

■   ■   ■   ■   ■

HorizontalItemsSpacing = 2, VerticalItemsSpacing = 2:
■■■■■
■■■■■
```

### Example: Balanced Spacing

```csharp
private void ConfigureBalancedSpacing()
{
    // Comfortable spacing for both directions
    colorPickerUIAdv1.ColorItemSize = new Size(18, 18);
    colorPickerUIAdv1.HorizontalItemsSpacing = 6;
    colorPickerUIAdv1.VerticalItemsSpacing = 6;
}
```

### Example: Grid-Like Layout

```csharp
private void ConfigureGridLayout()
{
    // Uniform spacing creates grid appearance
    colorPickerUIAdv1.ColorItemSize = new Size(20, 20);
    colorPickerUIAdv1.HorizontalItemsSpacing = 8;
    colorPickerUIAdv1.VerticalItemsSpacing = 8;
}
```

### Example: Compact with Separation

```csharp
private void ConfigureCompactWithSeparation()
{
    // Tight horizontal, spaced vertical
    colorPickerUIAdv1.ColorItemSize = new Size(15, 15);
    colorPickerUIAdv1.HorizontalItemsSpacing = 3;
    colorPickerUIAdv1.VerticalItemsSpacing = 10;
}
```

## Header Text Alignment

The `TextAlign` property controls alignment of group header text.

### TextAlign Property

```csharp
// Left-aligned (default)
colorPickerUIAdv1.TextAlign = ContentAlignment.MiddleLeft;

// Center-aligned
colorPickerUIAdv1.TextAlign = ContentAlignment.MiddleCenter;

// Right-aligned
colorPickerUIAdv1.TextAlign = ContentAlignment.MiddleRight;

// Top-left
colorPickerUIAdv1.TextAlign = ContentAlignment.TopLeft;

// Top-center
colorPickerUIAdv1.TextAlign = ContentAlignment.TopCenter;

// Bottom-left
colorPickerUIAdv1.TextAlign = ContentAlignment.BottomLeft;
```

**Visual Basic:**
```vb
' Center-aligned
colorPickerUIAdv1.TextAlign = ContentAlignment.MiddleCenter

' Right-aligned
colorPickerUIAdv1.TextAlign = ContentAlignment.MiddleRight
```

### ContentAlignment Options

| Alignment | Description | Use Case |
|-----------|-------------|----------|
| **TopLeft** | Top-left corner | Title-style headers |
| **TopCenter** | Top-centered | Centered titles |
| **TopRight** | Top-right corner | Right-aligned labels |
| **MiddleLeft** | Middle-left (default) | Standard headers |
| **MiddleCenter** | Middle-centered | Symmetric layouts |
| **MiddleRight** | Middle-right | Right-aligned headers |
| **BottomLeft** | Bottom-left | Footer-style |
| **BottomCenter** | Bottom-centered | Centered footers |
| **BottomRight** | Bottom-right | Right-aligned footers |

### Example: Centered Headers

```csharp
private void ConfigureCenteredHeaders()
{
    colorPickerUIAdv1.TextAlign = ContentAlignment.MiddleCenter;
    
    // Increase header height for prominence
    colorPickerUIAdv1.ThemeGroup.HeaderHeight = 28;
    colorPickerUIAdv1.StandardGroup.HeaderHeight = 28;
}
```

### Example: Right-Aligned Headers

```csharp
private void ConfigureRightAlignedHeaders()
{
    colorPickerUIAdv1.TextAlign = ContentAlignment.MiddleRight;
    
    // Often used with RTL (right-to-left) languages
}
```

## Font Customization

The `Font` property customizes the appearance of all group header text.

### Font Property

```csharp
// Default font
colorPickerUIAdv1.Font = new Font("Microsoft Sans Serif", 8.25F);

// Bold font
colorPickerUIAdv1.Font = new Font("Microsoft Sans Serif", 9F, FontStyle.Bold);

// Larger font
colorPickerUIAdv1.Font = new Font("Segoe UI", 10F, FontStyle.Regular);

// Bold and italic
colorPickerUIAdv1.Font = new Font("Arial", 9F, FontStyle.Bold | FontStyle.Italic);

// Custom font family
colorPickerUIAdv1.Font = new Font("Calibri", 9.5F, FontStyle.Regular);
```

**Visual Basic:**
```vb
' Bold font
colorPickerUIAdv1.Font = New Font("Microsoft Sans Serif", 9.0F, FontStyle.Bold)

' Larger font
colorPickerUIAdv1.Font = New Font("Segoe UI", 10.0F, FontStyle.Regular)
```

### FontStyle Options

```csharp
// Regular (default)
FontStyle.Regular

// Bold
FontStyle.Bold

// Italic
FontStyle.Italic

// Underline
FontStyle.Underline

// Strikeout
FontStyle.Strikeout

// Combined styles
FontStyle.Bold | FontStyle.Italic
```

### Font Size Guidelines

| Size (pt) | Use Case | Appearance |
|-----------|----------|------------|
| **8-8.25** | Default, compact | Standard |
| **9-10** | Better readability | Comfortable |
| **11-12** | Prominent headers | Large |
| **13+** | High-visibility | Extra large |

### Example: Modern Font Styling

```csharp
private void ConfigureModernFont()
{
    // Segoe UI, semi-bold, larger size
    colorPickerUIAdv1.Font = new Font("Segoe UI", 10F, FontStyle.Bold);
    
    // Center alignment for modern look
    colorPickerUIAdv1.TextAlign = ContentAlignment.MiddleCenter;
    
    // Increase header height to accommodate
    colorPickerUIAdv1.ThemeGroup.HeaderHeight = 30;
    colorPickerUIAdv1.StandardGroup.HeaderHeight = 30;
}
```

### Example: Classic Bold Headers

```csharp
private void ConfigureClassicFont()
{
    // Traditional bold headers
    colorPickerUIAdv1.Font = new Font("Microsoft Sans Serif", 9F, FontStyle.Bold);
    colorPickerUIAdv1.TextAlign = ContentAlignment.MiddleLeft;
}
```

### Example: Accessibility-Enhanced Font

```csharp
private void ConfigureAccessibleFont()
{
    // Larger, clear font for better accessibility
    colorPickerUIAdv1.Font = new Font("Segoe UI", 11F, FontStyle.Bold);
    
    // Increase header heights
    colorPickerUIAdv1.ThemeGroup.HeaderHeight = 32;
    colorPickerUIAdv1.StandardGroup.HeaderHeight = 32;
    colorPickerUIAdv1.RecentGroup.HeaderHeight = 32;
}
```

## Design-Time Color Editing

Colors within groups can be edited directly in the Visual Studio designer.

### Steps:

1. **Select ColorPickerUIAdv** in the designer
2. **Click on a color item** within the control preview
3. **Properties window** displays the selected color item
4. **Modify Color property** using the color picker dialog
5. **Changes are saved** automatically to designer code

### Design-Time Editing Advantages

**Benefits:**
- Visual color selection
- Immediate preview
- No code writing required
- Easy color adjustments

**Use Cases:**
- Quick prototyping
- Testing color combinations
- Theme development
- Client demonstrations

### Design-Time Limitations

- Cannot add new groups (use Collection Editor)
- Cannot add new items (use Collection Editor)
- Can only modify existing item colors
- Limited to visible items

### Programmatic Alternative

For complex scenarios, use code:

```csharp
// Modify existing group colors programmatically
private void ModifyGroupColors()
{
    // Access theme group
    ColorUIAdvGroup themeGroup = colorPickerUIAdv1.ThemeGroup;
    
    // Modify first item color
    if (themeGroup.Items.Count > 0)
    {
        ((GroupColorItem)themeGroup.Items[0]).Color = Color.Navy;
    }
    
    // Refresh control
    colorPickerUIAdv1.Refresh();
}
```

## Complete Customization Examples

### Example 1: Elegant Professional Layout

```csharp
private void ApplyElegantLayout()
{
    // Large, well-spaced items
    colorPickerUIAdv1.ColorItemSize = new Size(22, 22);
    colorPickerUIAdv1.HorizontalItemsSpacing = 8;
    colorPickerUIAdv1.VerticalItemsSpacing = 8;
    
    // Centered, bold headers
    colorPickerUIAdv1.TextAlign = ContentAlignment.MiddleCenter;
    colorPickerUIAdv1.Font = new Font("Segoe UI", 10F, FontStyle.Bold);
    
    // Increased header heights
    colorPickerUIAdv1.ThemeGroup.HeaderHeight = 30;
    colorPickerUIAdv1.StandardGroup.HeaderHeight = 30;
    colorPickerUIAdv1.RecentGroup.HeaderHeight = 30;
    
    // Office2016 style
    colorPickerUIAdv1.Style = ColorPickerUIAdv.visualstyle.Office2016Colorful;
    
    // Border
    colorPickerUIAdv1.BorderStyle = BorderStyle.FixedSingle;
    
    // Adjust control size
    colorPickerUIAdv1.Size = new Size(280, 300);
}
```

### Example 2: Compact Space-Saving Layout

```csharp
private void ApplyCompactLayout()
{
    // Small items, minimal spacing
    colorPickerUIAdv1.ColorItemSize = new Size(12, 12);
    colorPickerUIAdv1.HorizontalItemsSpacing = 3;
    colorPickerUIAdv1.VerticalItemsSpacing = 2;
    
    // Compact headers
    colorPickerUIAdv1.Font = new Font("Arial", 8F, FontStyle.Regular);
    colorPickerUIAdv1.TextAlign = ContentAlignment.MiddleLeft;
    
    // Reduced header heights
    colorPickerUIAdv1.ThemeGroup.HeaderHeight = 20;
    colorPickerUIAdv1.StandardGroup.HeaderHeight = 20;
    colorPickerUIAdv1.RecentGroup.HeaderHeight = 20;
    
    // Minimal border
    colorPickerUIAdv1.BorderStyle = BorderStyle.None;
    
    // Compact control size
    colorPickerUIAdv1.Size = new Size(190, 160);
}
```

### Example 3: Touch-Optimized Layout

```csharp
private void ApplyTouchOptimizedLayout()
{
    // Large touch targets
    colorPickerUIAdv1.ColorItemSize = new Size(32, 32);
    colorPickerUIAdv1.HorizontalItemsSpacing = 10;
    colorPickerUIAdv1.VerticalItemsSpacing = 10;
    
    // Large, readable headers
    colorPickerUIAdv1.Font = new Font("Segoe UI", 12F, FontStyle.Bold);
    colorPickerUIAdv1.TextAlign = ContentAlignment.MiddleCenter;
    
    // Tall headers
    colorPickerUIAdv1.ThemeGroup.HeaderHeight = 36;
    colorPickerUIAdv1.StandardGroup.HeaderHeight = 36;
    colorPickerUIAdv1.RecentGroup.HeaderHeight = 36;
    
    // Metro style for modern touch devices
    colorPickerUIAdv1.Style = ColorPickerUIAdv.visualstyle.Metro;
    
    // Large control size
    colorPickerUIAdv1.Size = new Size(380, 400);
}
```

### Example 4: High-Contrast Accessibility Layout

```csharp
private void ApplyAccessibleLayout()
{
    // Medium-large items for visibility
    colorPickerUIAdv1.ColorItemSize = new Size(24, 24);
    colorPickerUIAdv1.HorizontalItemsSpacing = 8;
    colorPickerUIAdv1.VerticalItemsSpacing = 8;
    
    // Large, bold font
    colorPickerUIAdv1.Font = new Font("Segoe UI", 11F, FontStyle.Bold);
    colorPickerUIAdv1.TextAlign = ContentAlignment.MiddleLeft;
    
    // Generous header heights
    colorPickerUIAdv1.ThemeGroup.HeaderHeight = 32;
    colorPickerUIAdv1.StandardGroup.HeaderHeight = 32;
    colorPickerUIAdv1.RecentGroup.HeaderHeight = 32;
    
    // High-contrast border
    colorPickerUIAdv1.BorderStyle = BorderStyle.Fixed3D;
    colorPickerUIAdv1.BorderOffset = 4;
    
    // Default style for compatibility
    colorPickerUIAdv1.Style = ColorPickerUIAdv.visualstyle.Default;
}
```

### Example 5: Grid-Style Uniform Layout

```csharp
private void ApplyGridLayout()
{
    // Perfect squares with uniform spacing
    colorPickerUIAdv1.ColorItemSize = new Size(20, 20);
    colorPickerUIAdv1.HorizontalItemsSpacing = 6;
    colorPickerUIAdv1.VerticalItemsSpacing = 6;
    
    // Centered headers for symmetry
    colorPickerUIAdv1.Font = new Font("Segoe UI", 9F, FontStyle.Bold);
    colorPickerUIAdv1.TextAlign = ContentAlignment.MiddleCenter;
    
    // Consistent header heights
    colorPickerUIAdv1.ThemeGroup.HeaderHeight = 26;
    colorPickerUIAdv1.StandardGroup.HeaderHeight = 26;
    colorPickerUIAdv1.RecentGroup.HeaderHeight = 26;
    
    // Clean Office2016 White style
    colorPickerUIAdv1.Style = ColorPickerUIAdv.visualstyle.Office2016White;
    
    // Single border
    colorPickerUIAdv1.BorderStyle = BorderStyle.FixedSingle;
}
```

### Example 6: Dynamic Layout Based on DPI

```csharp
private void ApplyDpiAwareLayout()
{
    // Get current DPI
    using (Graphics g = this.CreateGraphics())
    {
        float dpiX = g.DpiX;
        float scaleFactor = dpiX / 96f; // 96 DPI is standard
        
        // Scale color item size
        int itemSize = (int)(16 * scaleFactor);
        colorPickerUIAdv1.ColorItemSize = new Size(itemSize, itemSize);
        
        // Scale spacing
        int spacing = (int)(5 * scaleFactor);
        colorPickerUIAdv1.HorizontalItemsSpacing = spacing;
        colorPickerUIAdv1.VerticalItemsSpacing = spacing;
        
        // Scale font
        float fontSize = 9F * scaleFactor;
        colorPickerUIAdv1.Font = new Font("Segoe UI", fontSize, FontStyle.Bold);
        
        // Scale header height
        int headerHeight = (int)(26 * scaleFactor);
        colorPickerUIAdv1.ThemeGroup.HeaderHeight = headerHeight;
        colorPickerUIAdv1.StandardGroup.HeaderHeight = headerHeight;
        colorPickerUIAdv1.RecentGroup.HeaderHeight = headerHeight;
    }
}
```

## Best Practices

### Layout Guidelines

1. **Maintain Proportions:** Item size should be proportional to spacing
2. **Consistent Headers:** Use same HeaderHeight for all groups
3. **Readability:** Ensure font size matches header height
4. **Touch Targets:** Minimum 24x24 pixels for touch interfaces
5. **Test Resolutions:** Verify layout at different screen resolutions

### Spacing Recommendations

```csharp
// Recommended spacing ratios
// Item size: 20x20 → Spacing: 5-8
// Item size: 15x15 → Spacing: 3-6
// Item size: 30x30 → Spacing: 8-12

private int CalculateRecommendedSpacing(int itemSize)
{
    return (int)(itemSize * 0.3); // 30% of item size
}
```

### Accessibility Considerations

1. **Contrast:** Ensure sufficient contrast with background
2. **Size:** Items should be large enough to distinguish
3. **Spacing:** Adequate spacing prevents misclicks
4. **Font:** Clear, readable fonts (Segoe UI recommended)
5. **High DPI:** Test on high-DPI displays

## Troubleshooting

**Issue:** Items overlapping  
**Solution:** Increase HorizontalItemsSpacing and/or VerticalItemsSpacing

**Issue:** Headers cut off  
**Solution:** Increase HeaderHeight property for affected groups

**Issue:** Font too small to read  
**Solution:** Increase Font size, consider minimum 9pt for readability

**Issue:** Control too large after customization  
**Solution:** Adjust control Size property to accommodate new layout

**Issue:** Design-time changes not saving  
**Solution:** Ensure you're modifying via Properties window, rebuild solution if needed
