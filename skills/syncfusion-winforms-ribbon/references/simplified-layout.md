# Simplified Layout

## Table of Contents
- [Overview](#overview)
- [Enabling Simplified Layout](#enabling-simplified-layout)
- [Switching Between Layouts](#switching-between-layouts)
- [Item Visibility Control](#item-visibility-control)
- [Medium-Size Images](#medium-size-images)
- [Overflow Menu](#overflow-menu)
- [Runtime Customization](#runtime-customization)
- [Resizing Behavior](#resizing-behavior)

## Overview

Simplified layout is a modern, compact ribbon design that displays commands in a single-line interface. It maximizes screen space for content while keeping essential commands easily accessible, with additional commands available in an overflow menu.

**Benefits:**
- Maximum screen space for documents/content
- Single-line interface reduces visual clutter
- Overflow menu for less-used commands
- User can switch between normal and simplified modes
- Modern, clean aesthetic

**When to use:**
- Applications where content focus is priority
- Tablet or touch-oriented interfaces
- Modern UI aesthetic preference
- Users want workspace flexibility

## Enabling Simplified Layout

### LayoutMode Property

```csharp
// Set simplified layout
ribbonControlAdv1.LayoutMode = RibbonLayoutMode.Simplified;

// Set normal layout (default)
ribbonControlAdv1.LayoutMode = RibbonLayoutMode.Normal;
```

### Setting at Initialization

```csharp
public Form1()
{
    InitializeComponent();
    
    // Start in simplified layout
    ribbonControlAdv1.LayoutMode = RibbonLayoutMode.Simplified;
}
```

## Switching Between Layouts

### Enable Runtime Switching

Allow users to switch between normal and simplified layouts:

```csharp
// Enable layout switching via minimize button
ribbonControlAdv1.EnableSimplifiedLayoutMode = true;
```

**Effect:** Clicking minimize button cycles between:
- Normal layout (full ribbon)
- Simplified layout (single line)

### Programmatic Switching

```csharp
// Toggle between layouts
private void ToggleLayout()
{
    if (ribbonControlAdv1.LayoutMode == RibbonLayoutMode.Normal)
    {
        ribbonControlAdv1.LayoutMode = RibbonLayoutMode.Simplified;
    }
    else
    {
        ribbonControlAdv1.LayoutMode = RibbonLayoutMode.Normal;
    }
}

// Add button to toggle
ToolStripButton toggleButton = new ToolStripButton();
toggleButton.Text = "Toggle Layout";
toggleButton.Click += (s, e) => ToggleLayout();
```

## Item Visibility Control

### RibbonItemDisplayMode Enumeration

Control where items appear:

```csharp
public enum RibbonItemDisplayMode
{
    Normal = 1,           // Show in normal layout only
    Simplified = 2,       // Show in simplified layout only
    OverflowMenu = 4      // Show in overflow menu (simplified)
}
```

### SetDisplayMode Function

```csharp
// Show only in simplified layout
ribbonControlAdv1.SetDisplayMode(pasteButton, 
    RibbonItemDisplayMode.Simplified);

// Show only in normal layout
ribbonControlAdv1.SetDisplayMode(advancedButton, 
    RibbonItemDisplayMode.Normal);

// Show in both layouts (default)
ribbonControlAdv1.SetDisplayMode(saveButton, 
    RibbonItemDisplayMode.Normal | RibbonItemDisplayMode.Simplified);

// Show in normal + overflow menu in simplified
ribbonControlAdv1.SetDisplayMode(formatButton, 
    RibbonItemDisplayMode.Normal | RibbonItemDisplayMode.OverflowMenu);
```

### Complete Visibility Example

```csharp
private void ConfigureItemVisibility()
{
    // Essential commands - visible in both layouts
    ribbonControlAdv1.SetDisplayMode(saveButton, 
        RibbonItemDisplayMode.Normal | RibbonItemDisplayMode.Simplified);
    ribbonControlAdv1.SetDisplayMode(undoButton, 
        RibbonItemDisplayMode.Normal | RibbonItemDisplayMode.Simplified);
    ribbonControlAdv1.SetDisplayMode(redoButton, 
        RibbonItemDisplayMode.Normal | RibbonItemDisplayMode.Simplified);
    
    // Commonly used - simplified layout + normal
    ribbonControlAdv1.SetDisplayMode(pasteButton, 
        RibbonItemDisplayMode.Simplified | RibbonItemDisplayMode.Normal);
    ribbonControlAdv1.SetDisplayMode(cutButton, 
        RibbonItemDisplayMode.Simplified | RibbonItemDisplayMode.Normal);
    
    // Less common - normal layout + overflow menu
    ribbonControlAdv1.SetDisplayMode(advancedFormatButton, 
        RibbonItemDisplayMode.Normal | RibbonItemDisplayMode.OverflowMenu);
    ribbonControlAdv1.SetDisplayMode(specialOptionsButton, 
        RibbonItemDisplayMode.Normal | RibbonItemDisplayMode.OverflowMenu);
    
    // Rarely used - overflow menu only in simplified
    ribbonControlAdv1.SetDisplayMode(expertButton, 
        RibbonItemDisplayMode.OverflowMenu);
}
```

## Medium-Size Images

Simplified layout uses 20x20 pixel images as standard.

### Using ToolStripExImageProvider

```csharp
// Create medium image list (20x20)
ImageListAdv mediumImageList = new ImageListAdv();
mediumImageList.Images.Add(Image.FromFile("paste20.png"));
mediumImageList.Images.Add(Image.FromFile("cut20.png"));
mediumImageList.Images.Add(Image.FromFile("copy20.png"));

// Set up image provider
ToolStripExImageProvider imageProvider = new ToolStripExImageProvider(toolStripEx1);
imageProvider.MediumImageList = mediumImageList;

// Assign medium images to buttons
imageProvider.SetMediumItemImage(pasteButton, 0);
imageProvider.SetMediumItemImage(cutButton, 1);
imageProvider.SetMediumItemImage(copyButton, 2);
```

### Complete Image Setup

```csharp
private void SetupImagesForAllLayouts()
{
    // Create image lists for different sizes
    ImageListAdv largeImages = new ImageListAdv();   // 32x32 for normal layout
    ImageListAdv mediumImages = new ImageListAdv();  // 20x20 for simplified
    ImageListAdv smallImages = new ImageListAdv();   // 16x16 for collapsed
    
    // Load images
    largeImages.Images.Add(Image.FromFile("save32.png"));
    mediumImages.Images.Add(Image.FromFile("save20.png"));
    smallImages.Images.Add(Image.FromFile("save16.png"));
    
    // Set up image provider
    ToolStripExImageProvider imageProvider = new ToolStripExImageProvider(toolStripEx1);
    imageProvider.LargeImageList = largeImages;
    imageProvider.MediumImageList = mediumImages;
    imageProvider.SmallImageList = smallImages;
    
    // Assign to button
    imageProvider.SetLargeItemImage(saveButton, 0);
    imageProvider.SetMediumItemImage(saveButton, 0);
    imageProvider.SetSmallItemImage(saveButton, 0);
}
```

## Overflow Menu

Items not fitting in simplified layout appear in the overflow menu.

### Automatic Overflow

When window is resized, items automatically move to overflow:

```csharp
// Items with OverflowMenu display mode go to overflow
ribbonControlAdv1.SetDisplayMode(formatPainterButton, 
    RibbonItemDisplayMode.Simplified | RibbonItemDisplayMode.OverflowMenu);
```

### Overflow Behavior

- Items appear in overflow from right to left
- Most recently added items overflow first
- User can access via overflow button (»)
- Clicking overflow shows dropdown menu

## Runtime Customization

Users can customize ribbon through QAT window in simplified layout.

### QAT Window Behavior

**In Simplified Layout:**
- New tabs/groups created in simplified layout stay in simplified only
- Items added to QAT visible in both layouts
- Customization specific to active layout

**Example:**
```csharp
// Enable simplified layout mode
ribbonControlAdv1.EnableSimplifiedLayoutMode = true;

// User creates tab in simplified layout via QAT window
// That tab only appears in simplified layout
// Switching to normal layout won't show that tab
```

### QAT Cross-Layout Visibility

Items added to QAT persist across layouts:

```csharp
// Add item to QAT
ribbonControlAdv1.Header.AddQuickItem(new QuickButtonReflectable(saveButton));

// This item visible in both normal and simplified layouts
```

## Resizing Behavior

### Dynamic Overflow on Resize

When window width decreases:
1. Rightmost items move to overflow menu
2. Process continues as window gets smaller
3. All items can end up in overflow if needed

### Example

```csharp
// No special code needed - automatic behavior
// Items automatically move to overflow as window resizes
// Items return to ribbon as window expands
```

## Best Practices

1. **Choose essential commands for simplified:** Only show most frequently used commands directly in simplified layout

2. **Use appropriate display modes:**
   - Essential: `Normal | Simplified`
   - Common: `Simplified | OverflowMenu`
   - Advanced: `Normal | OverflowMenu`

3. **Provide medium-size images:** Always provide 20x20 images for simplified layout

4. **Enable layout switching:** Let users choose their preferred layout

5. **Test both layouts:** Verify commands work correctly in both modes

6. **Limit simplified items:** Keep 10-15 items max in simplified layout directly

7. **Group logically:** Related commands should overflow together

8. **Save user preference:** Remember user's layout choice

## Complete Example

```csharp
private void SetupSimplifiedLayoutRibbon()
{
    // Enable simplified layout
    ribbonControlAdv1.LayoutMode = RibbonLayoutMode.Simplified;
    ribbonControlAdv1.EnableSimplifiedLayoutMode = true; // Allow switching
    
    // Create buttons
    ToolStripButton saveButton = new ToolStripButton("Save");
    ToolStripButton pasteButton = new ToolStripButton("Paste");
    ToolStripButton formatButton = new ToolStripButton("Format");
    ToolStripButton advancedButton = new ToolStripButton("Advanced");
    
    // Set display modes
    ribbonControlAdv1.SetDisplayMode(saveButton, 
        RibbonItemDisplayMode.Normal | RibbonItemDisplayMode.Simplified);
    ribbonControlAdv1.SetDisplayMode(pasteButton, 
        RibbonItemDisplayMode.Simplified);
    ribbonControlAdv1.SetDisplayMode(formatButton, 
        RibbonItemDisplayMode.Normal | RibbonItemDisplayMode.OverflowMenu);
    ribbonControlAdv1.SetDisplayMode(advancedButton, 
        RibbonItemDisplayMode.Normal);
    
    // Set up images
    ImageListAdv mediumImages = new ImageListAdv();
    mediumImages.Images.Add(Image.FromFile("save20.png"));
    mediumImages.Images.Add(Image.FromFile("paste20.png"));
    
    ToolStripExImageProvider imageProvider = new ToolStripExImageProvider(toolStripEx1);
    imageProvider.MediumImageList = mediumImages;
    imageProvider.SetMediumItemImage(saveButton, 0);
    imageProvider.SetMediumItemImage(pasteButton, 1);
    
    // Add to group
    toolStripEx1.Items.AddRange(new ToolStripItem[] {
        saveButton,
        pasteButton,
        formatButton,
        advancedButton
    });
}
```

## Related Topics

- **Ribbon States** - Display options and minimize/maximize behavior
- **Ribbon Controls** - All available control types for simplified layout
- **Quick Access Toolbar** - QAT works with both normal and simplified layouts
