# Color Groups in ColorPickerUIAdv

## Table of Contents
- [Overview](#overview)
- [Default Color Groups](#default-color-groups)
- [Color Group Anatomy](#color-group-anatomy)
- [Custom Color Groups](#custom-color-groups)
- [Adding Color Items](#adding-color-items)
- [Sub-Items and Color Variations](#sub-items-and-color-variations)
- [Header Configuration](#header-configuration)
- [Complete Examples](#complete-examples)

## Overview

ColorPickerUIAdv organizes colors into logical groups, similar to Microsoft Word's color picker. Each group contains color items that can have sub-items (color variations/tints).

**Color Group Features:**
- Organized color collections (Recent, Standard, Theme, Custom)
- Hierarchical structure with sub-items
- Customizable headers and names
- Configurable visibility and depth
- Design-time and runtime configuration

## Default Color Groups

ColorPickerUIAdv provides three built-in color groups:

### 1. RecentGroup
Displays recently selected colors for quick access.

**Properties:**
- Automatically populated when colors are picked
- Stores user's color history
- Limited to recent selections

### 2. StandardGroup
Contains standard preset colors (Red, Blue, Green, Yellow, etc.).

**Properties:**
- Fixed standard color palette
- Common colors for general use
- Similar to Windows standard colors

### 3. ThemeGroup
Displays theme-based colors that match document themes.

**Properties:**
- Office-style theme colors
- Coordinated color schemes
- Professional color combinations

### Visual Structure

```
┌─────────────────────────┐
│ Recent Colors           │ ← Header (RecentGroup.Name)
├─────────────────────────┤
│ ■ ■ ■ ■ ■ ■ ■ ■ ■ ■    │ ← Color items
│   ■ ■ ■ ■ ■ ■ ■ ■ ■    │ ← Sub-items (tints/shades)
├─────────────────────────┤
│ Theme Colors            │ ← Header (ThemeGroup.Name)
├─────────────────────────┤
│ ■ ■ ■ ■ ■ ■ ■ ■ ■ ■    │
│   ■ ■ ■ ■ ■ ■ ■ ■ ■    │
├─────────────────────────┤
│ Standard Colors         │ ← Header (StandardGroup.Name)
├─────────────────────────┤
│ ■ ■ ■ ■ ■ ■ ■ ■ ■ ■    │
└─────────────────────────┘
```

### Accessing Default Groups

```csharp
// Access default groups
ColorUIAdvGroup recentGroup = colorPickerUIAdv1.RecentGroup;
ColorUIAdvGroup standardGroup = colorPickerUIAdv1.StandardGroup;
ColorUIAdvGroup themeGroup = colorPickerUIAdv1.ThemeGroup;

// Modify properties
recentGroup.Name = "Recently Used";
themeGroup.HeaderHeight = 30;
```

## Color Group Anatomy

Each color group consists of:

**Components:**
1. **Header** - Group name display
2. **Color Items** - Base colors (GroupColorItem)
3. **Sub-Items** - Color variations (ColorItem)

**Key Properties:**
- `Name` - Group header text
- `HeaderHeight` - Height of header section
- `Items` - Collection of GroupColorItem objects
- `IsSubItemsVisible` - Show/hide sub-items
- `SubItemsDepth` - Number of sub-item levels (0-n)

### GroupColorItem vs ColorItem

**GroupColorItem:**
- Top-level color in a group
- Can contain sub-items
- Has Index property for positioning

**ColorItem:**
- Sub-item (variation) of a GroupColorItem
- Cannot have further sub-items
- Represents tints, shades, or related colors

## Custom Color Groups

Create custom groups to organize domain-specific colors (brand colors, palette presets, etc.).

### Basic Custom Group Creation

```csharp
// Create custom group
ColorUIAdvGroup customGroup = new ColorUIAdvGroup();
customGroup.Name = "Brand Colors";
customGroup.HeaderHeight = 25;

// Add to control
colorPickerUIAdv1.CustomGroups.Add(customGroup);
```

### Using ColorUIAdvGroup Collection Editor (Designer)

1. Select ColorPickerUIAdv in designer
2. In Properties window, click **CustomGroups** property
3. Click ellipsis (...) button
4. **ColorUIAdvGroup Collection Editor** opens
5. Click **Add** to create new group
6. Configure properties in right panel
7. Click **OK**

## Adding Color Items

Add color items to groups using the Items collection.

### Programmatic Approach

```csharp
// Create color group
ColorUIAdvGroup customGroup = new ColorUIAdvGroup();
customGroup.Name = "Custom User Colors";

// Create color item
GroupColorItem groupColorItem1 = new GroupColorItem(customGroup, Color.Crimson);
groupColorItem1.Index = 0;

// Add item to group
customGroup.Items.Add(groupColorItem1);

// Add group to control
colorPickerUIAdv1.CustomGroups.Add(customGroup);
```

### Design-Time Item Addition

1. Open **CustomGroups** collection editor
2. Select a group
3. Click **Items** property
4. Click ellipsis (...)
5. **ColorItem Collection Editor** opens
6. Click **Add** → Creates GroupColorItem
7. Set **Color** property
8. Set **Index** for ordering
9. Click **OK**

## Sub-Items and Color Variations

Sub-items provide color variations (lighter/darker tints) for each base color.

### Enabling Sub-Items

```csharp
ColorUIAdvGroup group = new ColorUIAdvGroup();
group.Name = "Colors with Variations";

// Enable sub-item display
group.IsSubItemsVisible = true;

// Set depth (number of sub-item rows)
group.SubItemsDepth = 1; // One row of variations
```

### Adding Sub-Items to Color Items

```csharp
// Create base color
GroupColorItem baseColor = new GroupColorItem(group, Color.Blue);
baseColor.Index = 0;

// Add lighter variations as sub-items
baseColor.SubItems.Add(new ColorItem(baseColor, Color.LightBlue));
baseColor.SubItems.Add(new ColorItem(baseColor, Color.LightSkyBlue));
baseColor.SubItems.Add(new ColorItem(baseColor, Color.PowderBlue));

// Add darker variations
baseColor.SubItems.Add(new ColorItem(baseColor, Color.DarkBlue));
baseColor.SubItems.Add(new ColorItem(baseColor, Color.MidnightBlue));

// Add to group
group.Items.Add(baseColor);
```

### Programmatic Color Tint Generation

Helper method to generate tints automatically:

```csharp
private List<Color> GenerateTints(Color baseColor, int count)
{
    List<Color> tints = new List<Color>();
    
    for (int i = 1; i <= count; i++)
    {
        float factor = i / (float)(count + 1);
        
        int r = (int)(baseColor.R + (255 - baseColor.R) * factor);
        int g = (int)(baseColor.G + (255 - baseColor.G) * factor);
        int b = (int)(baseColor.B + (255 - baseColor.B) * factor);
        
        tints.Add(Color.FromArgb(r, g, b));
    }
    
    return tints;
}

// Usage
GroupColorItem colorItem = new GroupColorItem(group, Color.Green);
List<Color> tints = GenerateTints(Color.Green, 5);

foreach (Color tint in tints)
{
    colorItem.SubItems.Add(new ColorItem(colorItem, tint));
}
```

## Header Configuration

Customize group headers for better organization and visual hierarchy.

### Header Properties

```csharp
ColorUIAdvGroup group = new ColorUIAdvGroup();

// Set header text
group.Name = "Custom Color Collection";

// Set header height (default: 20)
group.HeaderHeight = 30;

// Global text alignment (affects all groups)
colorPickerUIAdv1.TextAlign = ContentAlignment.MiddleCenter;

// Global font (affects all groups)
colorPickerUIAdv1.Font = new Font("Segoe UI", 9F, FontStyle.Bold);
```

### Per-Group Customization

```csharp
// Theme group with larger header
colorPickerUIAdv1.ThemeGroup.Name = "Office Theme Colors";
colorPickerUIAdv1.ThemeGroup.HeaderHeight = 28;

// Standard group with different name
colorPickerUIAdv1.StandardGroup.Name = "Standard Palette";
colorPickerUIAdv1.StandardGroup.HeaderHeight = 25;

// Recent group customization
colorPickerUIAdv1.RecentGroup.Name = "Recently Used Colors";
colorPickerUIAdv1.RecentGroup.HeaderHeight = 25;
```

### Text Alignment Options

```csharp
// Left-aligned (default)
colorPickerUIAdv1.TextAlign = ContentAlignment.MiddleLeft;

// Center-aligned
colorPickerUIAdv1.TextAlign = ContentAlignment.MiddleCenter;

// Right-aligned
colorPickerUIAdv1.TextAlign = ContentAlignment.MiddleRight;

// Top-aligned
colorPickerUIAdv1.TextAlign = ContentAlignment.TopLeft;

// Bottom-aligned
colorPickerUIAdv1.TextAlign = ContentAlignment.BottomLeft;
```

## Complete Examples

### Example 1: Brand Color Palette

```csharp
private void CreateBrandColorPalette()
{
    ColorUIAdvGroup brandGroup = new ColorUIAdvGroup();
    brandGroup.Name = "Brand Colors";
    brandGroup.HeaderHeight = 28;
    brandGroup.IsSubItemsVisible = true;
    brandGroup.SubItemsDepth = 1;
    
    // Primary brand color with tints
    GroupColorItem primary = new GroupColorItem(brandGroup, Color.FromArgb(0, 120, 215));
    primary.Index = 0;
    primary.SubItems.Add(new ColorItem(primary, Color.FromArgb(204, 228, 247))); // Lightest
    primary.SubItems.Add(new ColorItem(primary, Color.FromArgb(153, 204, 239)));
    primary.SubItems.Add(new ColorItem(primary, Color.FromArgb(102, 163, 224)));
    primary.SubItems.Add(new ColorItem(primary, Color.FromArgb(51, 141, 219)));
    primary.SubItems.Add(new ColorItem(primary, Color.FromArgb(0, 90, 158)));    // Darkest
    brandGroup.Items.Add(primary);
    
    // Secondary brand color with tints
    GroupColorItem secondary = new GroupColorItem(brandGroup, Color.FromArgb(16, 110, 190));
    secondary.Index = 1;
    secondary.SubItems.Add(new ColorItem(secondary, Color.FromArgb(204, 230, 244)));
    secondary.SubItems.Add(new ColorItem(secondary, Color.FromArgb(153, 205, 233)));
    secondary.SubItems.Add(new ColorItem(secondary, Color.FromArgb(102, 180, 222)));
    secondary.SubItems.Add(new ColorItem(secondary, Color.FromArgb(51, 155, 211)));
    secondary.SubItems.Add(new ColorItem(secondary, Color.FromArgb(12, 83, 143)));
    brandGroup.Items.Add(secondary);
    
    // Accent color with tints
    GroupColorItem accent = new GroupColorItem(brandGroup, Color.FromArgb(255, 185, 0));
    accent.Index = 2;
    accent.SubItems.Add(new ColorItem(accent, Color.FromArgb(255, 243, 204)));
    accent.SubItems.Add(new ColorItem(accent, Color.FromArgb(255, 231, 153)));
    accent.SubItems.Add(new ColorItem(accent, Color.FromArgb(255, 220, 102)));
    accent.SubItems.Add(new ColorItem(accent, Color.FromArgb(255, 208, 51)));
    accent.SubItems.Add(new ColorItem(accent, Color.FromArgb(204, 148, 0)));
    brandGroup.Items.Add(accent);
    
    colorPickerUIAdv1.CustomGroups.Add(brandGroup);
}
```

### Example 2: Web-Safe Colors

```csharp
private void CreateWebSafeColorGroup()
{
    ColorUIAdvGroup webSafeGroup = new ColorUIAdvGroup();
    webSafeGroup.Name = "Web-Safe Colors";
    webSafeGroup.HeaderHeight = 25;
    webSafeGroup.IsSubItemsVisible = false;
    
    // Common web-safe colors
    Color[] webColors = new Color[]
    {
        Color.FromArgb(0, 0, 0),       // Black
        Color.FromArgb(255, 255, 255), // White
        Color.FromArgb(255, 0, 0),     // Red
        Color.FromArgb(0, 255, 0),     // Lime
        Color.FromArgb(0, 0, 255),     // Blue
        Color.FromArgb(255, 255, 0),   // Yellow
        Color.FromArgb(0, 255, 255),   // Cyan
        Color.FromArgb(255, 0, 255),   // Magenta
        Color.FromArgb(192, 192, 192), // Silver
        Color.FromArgb(128, 128, 128), // Gray
    };
    
    for (int i = 0; i < webColors.Length; i++)
    {
        GroupColorItem item = new GroupColorItem(webSafeGroup, webColors[i]);
        item.Index = i;
        webSafeGroup.Items.Add(item);
    }
    
    colorPickerUIAdv1.CustomGroups.Add(webSafeGroup);
}
```

### Example 3: Traffic Light Status Colors

```csharp
private void CreateStatusColorGroup()
{
    ColorUIAdvGroup statusGroup = new ColorUIAdvGroup();
    statusGroup.Name = "Status Colors";
    statusGroup.HeaderHeight = 25;
    statusGroup.IsSubItemsVisible = true;
    statusGroup.SubItemsDepth = 1;
    
    // Success (Green)
    GroupColorItem success = new GroupColorItem(statusGroup, Color.FromArgb(16, 124, 16));
    success.Index = 0;
    success.SubItems.Add(new ColorItem(success, Color.FromArgb(198, 239, 206)));
    success.SubItems.Add(new ColorItem(success, Color.FromArgb(76, 175, 80)));
    statusGroup.Items.Add(success);
    
    // Warning (Yellow/Orange)
    GroupColorItem warning = new GroupColorItem(statusGroup, Color.FromArgb(255, 152, 0));
    warning.Index = 1;
    warning.SubItems.Add(new ColorItem(warning, Color.FromArgb(255, 243, 224)));
    warning.SubItems.Add(new ColorItem(warning, Color.FromArgb(255, 193, 7)));
    statusGroup.Items.Add(warning);
    
    // Error (Red)
    GroupColorItem error = new GroupColorItem(statusGroup, Color.FromArgb(211, 47, 47));
    error.Index = 2;
    error.SubItems.Add(new ColorItem(error, Color.FromArgb(255, 235, 238)));
    error.SubItems.Add(new ColorItem(error, Color.FromArgb(244, 67, 54)));
    statusGroup.Items.Add(error);
    
    // Info (Blue)
    GroupColorItem info = new GroupColorItem(statusGroup, Color.FromArgb(2, 136, 209));
    info.Index = 3;
    info.SubItems.Add(new ColorItem(info, Color.FromArgb(225, 245, 254)));
    info.SubItems.Add(new ColorItem(info, Color.FromArgb(3, 169, 244)));
    statusGroup.Items.Add(info);
    
    colorPickerUIAdv1.CustomGroups.Add(statusGroup);
}
```

### Example 4: Modifying Default Groups

```csharp
private void CustomizeDefaultGroups()
{
    // Customize Recent Group
    colorPickerUIAdv1.RecentGroup.Name = "Recently Used";
    colorPickerUIAdv1.RecentGroup.HeaderHeight = 26;
    
    // Customize Theme Group
    colorPickerUIAdv1.ThemeGroup.Name = "Office Theme Colors";
    colorPickerUIAdv1.ThemeGroup.HeaderHeight = 26;
    colorPickerUIAdv1.ThemeGroup.IsSubItemsVisible = true;
    colorPickerUIAdv1.ThemeGroup.SubItemsDepth = 1;
    
    // Customize Standard Group
    colorPickerUIAdv1.StandardGroup.Name = "Standard Palette";
    colorPickerUIAdv1.StandardGroup.HeaderHeight = 26;
    
    // Global header settings
    colorPickerUIAdv1.TextAlign = ContentAlignment.MiddleCenter;
    colorPickerUIAdv1.Font = new Font("Segoe UI", 9F, FontStyle.Bold);
}
```

## Design-Time Color Editing

Colors within groups are editable at design time:

1. Select ColorPickerUIAdv in designer
2. Click on a color item within the control
3. Properties window shows color properties
4. Change the **Color** property using color picker
5. Changes are saved automatically

## Best Practices

1. **Meaningful Names:** Use descriptive group names ("Brand Colors", not "Group1")
2. **Consistent Depth:** Keep SubItemsDepth consistent across similar groups
3. **Logical Organization:** Group related colors together
4. **Tint Hierarchy:** Order sub-items from lightest to darkest (or vice versa)
5. **Limited Groups:** Avoid too many groups (3-5 is ideal)
6. **Header Heights:** Use consistent header heights for visual harmony

## Troubleshooting

**Issue:** Sub-items not appearing  
**Solution:** Verify `IsSubItemsVisible = true` and `SubItemsDepth > 0`

**Issue:** Colors not in expected order  
**Solution:** Check `Index` property of GroupColorItem objects

**Issue:** Custom group not showing  
**Solution:** Ensure `CustomGroups.Add(group)` is called after populating items

**Issue:** Headers overlapping  
**Solution:** Increase `HeaderHeight` or reduce font size
