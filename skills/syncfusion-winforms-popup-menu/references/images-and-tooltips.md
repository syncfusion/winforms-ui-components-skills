# Images and Tooltips in PopupMenu

## Table of Contents
- [Overview](#overview)
- [Adding Images to BarItems](#adding-images-to-baritems)
- [State-Based Images](#state-based-images)
- [ImageExt Class Usage](#imageext-class-usage)
- [Tooltip Configuration](#tooltip-configuration)
- [Best Practices](#best-practices)

## Overview

Images and tooltips enhance menu usability by providing visual cues and helpful descriptions. PopupMenu supports state-based images (normal, disabled, highlighted) and customizable tooltips for all BarItem types.

**Supported Image States:**
- **Image:** Default/normal state
- **DisabledImage:** When item is disabled
- **HighlightedImage:** When item is hovered/selected

**Tooltip Features:**
- Custom tooltip text per item
- ShowToolTipInPopUp property to enable/disable
- Helpful for icon-only menus or providing additional context

## Adding Images to BarItems

```csharp
// From file
BarItem item1 = new BarItem {
    Text = "Save",
    Image = new ImageExt(System.Drawing.Image.FromFile(@"icons\save.png")),
    SizeToFit = true
};

// From resources (recommended)
BarItem item2 = new BarItem {
    Text = "Cut",
    Image = new ImageExt(Properties.Resources.CutIcon),
    Shortcut = Shortcut.CtrlX,
    SizeToFit = true
};

// Multiple items
parentBarItem1.Items.AddRange(new BarItem[] {
    new BarItem { Text = "New", Image = new ImageExt(Properties.Resources.NewIcon) },
    new BarItem { Text = "Open", Image = new ImageExt(Properties.Resources.OpenIcon) },
    new BarItem { Text = "Save", Image = new ImageExt(Properties.Resources.SaveIcon) }
});
```

## State-Based Images

### State-Based Images (Normal, Disabled, Highlighted)

```csharp
BarItem saveItem = new BarItem();
saveItem.Text = "Save";
saveItem.SizeToFit = true;

// Normal state
saveItem.Image = new ImageExt(System.Drawing.Image.FromFile(@"icons\save_normal.png"));

// Disabled state (grayed out)
saveItem.DisabledImage = new ImageExt(System.Drawing.Image.FromFile(@"icons\save_disabled.png"));

// Highlighted state (brighter/different color)
saveItem.HighlightedImage = new ImageExt(System.Drawing.Image.FromFile(@"icons\save_highlighted.png"));

// Dynamic enabling based on document state
saveItem.Click += SaveItem_Click;
popupMenu1.BeforePopup += (s, e) => {
    saveItem.Enabled = currentDocument != null && currentDocument.IsModified;
};

parentBarItem1.Items.Add(saveItem);
```

### Creating Disabled Images Programmatically

If you don't have separate disabled images, create them from the normal image:

```csharp
private ImageExt CreateDisabledImage(Image normalImage)
{
    Bitmap disabledBitmap = new Bitmap(normalImage.Width, normalImage.Height);
    
    using (Graphics g = Graphics.FromImage(disabledBitmap))
    {
        // Create grayscale disabled effect
        ImageAttributes attributes = new ImageAttributes();
        
        // Grayscale color matrix
        float[][] matrix = {
            new float[] {0.3f, 0.3f, 0.3f, 0, 0},
            new float[] {0.59f, 0.59f, 0.59f, 0, 0},
            new float[] {0.11f, 0.11f, 0.11f, 0, 0},
            new float[] {0, 0, 0, 0.5f, 0},  // 50% opacity
            new float[] {0, 0, 0, 0, 1}
        };
        
        attributes.SetColorMatrix(new ColorMatrix(matrix));
        
        g.DrawImage(normalImage,
            new Rectangle(0, 0, normalImage.Width, normalImage.Height),
            0, 0, normalImage.Width, normalImage.Height,
            GraphicsUnit.Pixel, attributes);
    }
    
    return new ImageExt(disabledBitmap);
}

// Usage
BarItem item = new BarItem();
item.Image = new ImageExt(Properties.Resources.SaveIcon);
item.DisabledImage = CreateDisabledImage(Properties.Resources.SaveIcon);
```

## ImageExt Class Usage

The `ImageExt` class is Syncfusion's wrapper for System.Drawing.Image, providing additional functionality for menu images.

### Basic Usage

```csharp
// From file
ImageExt image1 = new ImageExt(System.Drawing.Image.FromFile(@"path\to\icon.png"));

// From resources
ImageExt image2 = new ImageExt(Properties.Resources.MyIcon);

// From bitmap
Bitmap bmp = new Bitmap(16, 16);
ImageExt image3 = new ImageExt(bmp);

// From icon file
Icon icon = new Icon(@"path\to\icon.ico");
ImageExt image4 = new ImageExt(icon.ToBitmap());
```

### Recommended Image Specifications

**Icon Size:**
- **Standard:** 16x16 pixels (most common)
- **High DPI:** 32x32 pixels (for high-DPI displays)
- **Format:** PNG with transparency preferred
- **Color Depth:** 32-bit RGBA

**File Formats:**
- PNG (recommended - supports transparency)
- BMP (supported - no transparency)
- ICO (supported - Windows icon format)
- JPEG (supported - no transparency, compression artifacts)

## Tooltip Configuration

### Basic Tooltip Setup

```csharp
// Basic tooltip
BarItem copyItem = new BarItem {
    Text = "Copy",
    Tooltip = "Copy the selected text to clipboard",
    ShowToolTipInPopUp = true,  // Enable tooltip
    SizeToFit = true
};

// Tooltip with keyboard shortcut
BarItem saveItem = new BarItem {
    Text = "Save",
    Shortcut = Shortcut.CtrlS,
    Tooltip = "Save the current document (Ctrl+S)",
    ShowToolTipInPopUp = true,
    Image = new ImageExt(Properties.Resources.SaveIcon)
};

parentBarItem1.Items.AddRange(new BarItem[] { copyItem, saveItem });
```

### Dynamic Tooltips

Update tooltip text based on application state:

```csharp
BarItem undoItem = new BarItem();
undoItem.Text = "Undo";
undoItem.Shortcut = Shortcut.CtrlZ;
undoItem.ShowToolTipInPopUp = true;
undoItem.Image = new ImageExt(Properties.Resources.UndoIcon);

popupMenu1.BeforePopup += (s, e) => {
    if (editor.CanUndo)
    {
        string lastAction = editor.GetLastActionName();
        undoItem.Tooltip = $"Undo {lastAction} (Ctrl+Z)";
        undoItem.Enabled = true;
    }
    else
    {
        undoItem.Tooltip = "Nothing to undo";
        undoItem.Enabled = false;
    }
};
```

### Multi-Line Tooltips

```csharp
BarItem importItem = new BarItem {
    Text = "Import...",
    Tooltip = "Import data from external files\nSupported formats: CSV, XML, JSON\nCtrl+I to open",
    ShowToolTipInPopUp = true,
    Shortcut = Shortcut.CtrlI
};
```

## Best Practices

**Images:**
- Use consistent sizes (16x16 or 32x32), PNG with transparency
- Always provide DisabledImage for visual feedback
- Load from resources, reuse ImageExt instances
- Don't rely on icons alone, include text

**Tooltips:**
- Keep concise, include keyboard shortcuts
- Always use for icon-only items
- Skip for self-explanatory text-only items
- Enable for screen reader compatibility

## Common Pattern - Edit Menu with Images

```csharp
BarItem[] editItems = new BarItem[] {
    new BarItem {
        Text = "Undo",
        Image = new ImageExt(Properties.Resources.Undo),
        DisabledImage = CreateDisabledImage(Properties.Resources.Undo),
        Shortcut = Shortcut.CtrlZ,
        Tooltip = "Undo the last action (Ctrl+Z)",
        ShowToolTipInPopUp = true
    },
    new BarItem {
        Text = "Cut",
        Image = new ImageExt(Properties.Resources.Cut),
        Shortcut = Shortcut.CtrlX,
        Tooltip = "Cut selected text (Ctrl+X)",
        ShowToolTipInPopUp = true
    }
};
parentBarItem1.Items.AddRange(editItems);
```

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Images don't appear | Verify file path, check ImageExt wrapper used, ensure SizeToFit = true |
| DisabledImage not showing | Confirm Enabled = false and DisabledImage assigned |
| Tooltips don't appear | Verify ShowToolTipInPopUp = true and Tooltip has text |
| Images blurry/pixelated | Use higher resolution (32x32), PNG format, test on high-DPI |
