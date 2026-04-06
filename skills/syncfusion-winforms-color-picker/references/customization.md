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

### Size Guidelines

| Size | Use Case | Appearance |
|------|----------|------------|
| **13x13** | Default, compact layouts | Standard Office-style |
| **15x15** | Slightly larger, better visibility | Comfortable |
| **20x20** | Touch-friendly, prominent | Large |
| **25x25+** | Presentation, demos | Extra large |

## Spacing Between Items

Control horizontal and vertical spacing between color items for visual organization.

### HorizontalItemsSpacing Property

Sets the horizontal distance between color items (default: 4 pixels).

```csharp
// Default spacing
colorPickerUIAdv1.HorizontalItemsSpacing = 4;

// Wider spacing
colorPickerUIAdv1.HorizontalItemsSpacing = 10;
```

### VerticalItemsSpacing Property

Sets the vertical distance between color item rows (default: 0 pixels).

```csharp
// Add vertical breathing room
colorPickerUIAdv1.VerticalItemsSpacing = 5;

// Significant vertical separation
colorPickerUIAdv1.VerticalItemsSpacing = 10;
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
    colorPickerUIAdv1.ColorItemSize = new Size(18, 18);
    colorPickerUIAdv1.HorizontalItemsSpacing = 6;
    colorPickerUIAdv1.VerticalItemsSpacing = 6;
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
```

### Font Size Guidelines

| Size (pt) | Use Case | Appearance |
|-----------|----------|------------|
| **8-8.25** | Default, compact | Standard |
| **9-10** | Better readability | Comfortable |
| **11-12** | Prominent headers | Large |
| **13+** | High-visibility | Extra large |

## Design-Time Color Editing

Colors within groups can be edited directly in the Visual Studio designer.

### Steps:

1. **Select ColorPickerUIAdv** in the designer
2. **Click on a color item** within the control preview
3. **Properties window** displays the selected color item
4. **Modify Color property** using the color picker dialog
5. **Changes are saved** automatically to designer code

### Design-Time Editing Notes

**Benefits:** Visual color selection, immediate preview, no code required

**Limitations:** Cannot add new groups/items (use Collection Editor), only modify existing colors

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

### Example 1: Professional Layout

```csharp
private void ApplyProfessionalLayout()
{
    colorPickerUIAdv1.ColorItemSize = new Size(22, 22);
    colorPickerUIAdv1.HorizontalItemsSpacing = 8;
    colorPickerUIAdv1.VerticalItemsSpacing = 8;
    colorPickerUIAdv1.TextAlign = ContentAlignment.MiddleCenter;
    colorPickerUIAdv1.Font = new Font("Segoe UI", 10F, FontStyle.Bold);
    colorPickerUIAdv1.ThemeGroup.HeaderHeight = 30;
    colorPickerUIAdv1.StandardGroup.HeaderHeight = 30;
    colorPickerUIAdv1.Style = ColorPickerUIAdv.visualstyle.Office2016Colorful;
}
```

### Example 2: Touch-Optimized Layout

```csharp
private void ApplyTouchOptimizedLayout()
{
    colorPickerUIAdv1.ColorItemSize = new Size(32, 32);
    colorPickerUIAdv1.HorizontalItemsSpacing = 10;
    colorPickerUIAdv1.VerticalItemsSpacing = 10;
    colorPickerUIAdv1.Font = new Font("Segoe UI", 12F, FontStyle.Bold);
    colorPickerUIAdv1.ThemeGroup.HeaderHeight = 36;
    colorPickerUIAdv1.StandardGroup.HeaderHeight = 36;
}
```

### Example 3: DPI-Aware Layout

```csharp
private void ApplyDpiAwareLayout()
{
    using (Graphics g = this.CreateGraphics())
    {
        float scaleFactor = g.DpiX / 96f;
        colorPickerUIAdv1.ColorItemSize = new Size((int)(16 * scaleFactor), (int)(16 * scaleFactor));
        colorPickerUIAdv1.HorizontalItemsSpacing = (int)(5 * scaleFactor);
        colorPickerUIAdv1.VerticalItemsSpacing = (int)(5 * scaleFactor);
        colorPickerUIAdv1.Font = new Font("Segoe UI", 9F * scaleFactor, FontStyle.Bold);
    }
}
```

## Best Practices

1. **Maintain Proportions:** Item size should be proportional to spacing (30% of item size recommended)
2. **Consistent Headers:** Use same HeaderHeight for all groups
3. **Touch Targets:** Minimum 24x24 pixels for touch interfaces
4. **Accessibility:** Clear fonts (Segoe UI), sufficient contrast, test on high-DPI displays

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Items overlapping | Increase HorizontalItemsSpacing/VerticalItemsSpacing |
| Headers cut off | Increase HeaderHeight property |
| Font too small | Increase Font size (minimum 9pt) |
| Control too large | Adjust control Size property |
| Design-time changes not saving | Modify via Properties window, rebuild solution |
