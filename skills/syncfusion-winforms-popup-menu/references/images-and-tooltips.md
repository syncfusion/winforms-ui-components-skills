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

### Using Image Property

The `Image` property sets the default icon displayed with a BarItem.

```csharp
BarItem saveItem = new BarItem();
saveItem.Text = "Save";
saveItem.Image = new ImageExt(System.Drawing.Image.FromFile(@"icons\save.png"));
saveItem.SizeToFit = true;

parentBarItem1.Items.Add(saveItem);
```

### VB.NET Example

```vb
Dim saveItem As New BarItem()
saveItem.Text = "Save"
saveItem.Image = New ImageExt(System.Drawing.Image.FromFile("icons\save.png"))
saveItem.SizeToFit = True

parentBarItem1.Items.Add(saveItem)
```

### Loading from Resources

```csharp
BarItem cutItem = new BarItem();
cutItem.Text = "Cut";
cutItem.Image = new ImageExt(Properties.Resources.CutIcon);
cutItem.Shortcut = Shortcut.CtrlX;
cutItem.SizeToFit = true;
```

### Loading from Embedded Resources

```csharp
BarItem openItem = new BarItem();
openItem.Text = "Open";

// Load from assembly embedded resource
var assembly = System.Reflection.Assembly.GetExecutingAssembly();
using (var stream = assembly.GetManifestResourceStream("MyApp.Resources.Open.png"))
{
    if (stream != null)
    {
        openItem.Image = new ImageExt(System.Drawing.Image.FromStream(stream));
    }
}

openItem.SizeToFit = true;
```

### Multiple Items with Images

```csharp
// File menu items with icons
BarItem newItem = new BarItem {
    Text = "New",
    Image = new ImageExt(Properties.Resources.NewIcon),
    Shortcut = Shortcut.CtrlN
};

BarItem openItem = new BarItem {
    Text = "Open",
    Image = new ImageExt(Properties.Resources.OpenIcon),
    Shortcut = Shortcut.CtrlO
};

BarItem saveItem = new BarItem {
    Text = "Save",
    Image = new ImageExt(Properties.Resources.SaveIcon),
    Shortcut = Shortcut.CtrlS
};

BarItem saveAsItem = new BarItem {
    Text = "Save As...",
    Image = new ImageExt(Properties.Resources.SaveAsIcon),
    Shortcut = Shortcut.CtrlShiftS
};

parentBarItem1.Items.AddRange(new BarItem[] { newItem, openItem, saveItem, saveAsItem });
```

### Images with ParentBarItem

```csharp
ParentBarItem fileMenu = new ParentBarItem();
fileMenu.Text = "File";
fileMenu.Image = new ImageExt(Properties.Resources.FileIcon);
fileMenu.SizeToFit = true;

// Add child items
fileMenu.Items.Add(new BarItem { Text = "New", Image = new ImageExt(Properties.Resources.NewIcon) });
fileMenu.Items.Add(new BarItem { Text = "Open", Image = new ImageExt(Properties.Resources.OpenIcon) });

parentBarItem1.Items.Add(fileMenu);
```

## State-Based Images

### Disabled State Images

The `DisabledImage` property displays a different icon when the item is disabled (`Enabled = false`).

```csharp
BarItem pasteItem = new BarItem();
pasteItem.Text = "Paste";
pasteItem.Image = new ImageExt(System.Drawing.Image.FromFile(@"icons\paste.png"));
pasteItem.DisabledImage = new ImageExt(System.Drawing.Image.FromFile(@"icons\paste_disabled.png"));
pasteItem.Enabled = false;  // Will show disabled image
pasteItem.SizeToFit = true;

parentBarItem1.Items.Add(pasteItem);
```

### VB.NET Example

```vb
Dim pasteItem As New BarItem()
pasteItem.Text = "Paste"
pasteItem.Image = New ImageExt(System.Drawing.Image.FromFile("icons\paste.png"))
pasteItem.DisabledImage = New ImageExt(System.Drawing.Image.FromFile("icons\paste_disabled.png"))
pasteItem.Enabled = False  ' Will show disabled image
pasteItem.SizeToFit = True

parentBarItem1.Items.Add(pasteItem)
```

### Highlighted State Images

The `HighlightedImage` property displays when the mouse hovers over or selects the item.

```csharp
BarItem deleteItem = new BarItem();
deleteItem.Text = "Delete";
deleteItem.Image = new ImageExt(System.Drawing.Image.FromFile(@"icons\delete.png"));
deleteItem.HighlightedImage = new ImageExt(System.Drawing.Image.FromFile(@"icons\delete_highlighted.png"));
deleteItem.SizeToFit = true;

parentBarItem1.Items.Add(deleteItem);
```

### VB.NET Example

```vb
Dim deleteItem As New BarItem()
deleteItem.Text = "Delete"
deleteItem.Image = New ImageExt(System.Drawing.Image.FromFile("icons\delete.png"))
deleteItem.HighlightedImage = New ImageExt(System.Drawing.Image.FromFile("icons\delete_highlighted.png"))
deleteItem.SizeToFit = True

parentBarItem1.Items.Add(deleteItem)
```

### All Three States Combined

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
BarItem copyItem = new BarItem();
copyItem.Text = "Copy";
copyItem.Tooltip = "Copy the selected text to clipboard";
copyItem.ShowToolTipInPopUp = true;  // Enable tooltip
copyItem.SizeToFit = true;

parentBarItem1.Items.Add(copyItem);
```

### VB.NET Example

```vb
Dim copyItem As New BarItem()
copyItem.Text = "Copy"
copyItem.Tooltip = "Copy the selected text to clipboard"
copyItem.ShowToolTipInPopUp = True  ' Enable tooltip
copyItem.SizeToFit = True

parentBarItem1.Items.Add(copyItem)
```

### Tooltips with Keyboard Shortcuts

```csharp
BarItem saveItem = new BarItem();
saveItem.Text = "Save";
saveItem.Shortcut = Shortcut.CtrlS;
saveItem.Tooltip = "Save the current document (Ctrl+S)";
saveItem.ShowToolTipInPopUp = true;
saveItem.Image = new ImageExt(Properties.Resources.SaveIcon);

parentBarItem1.Items.Add(saveItem);
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

### Tooltips for All BarItem Types

Tooltips work with all BarItem types:

```csharp
// ParentBarItem
ParentBarItem fileMenu = new ParentBarItem {
    Text = "File",
    Tooltip = "File operations (New, Open, Save)",
    ShowToolTipInPopUp = true
};

// ComboBoxBarItem
ComboBoxBarItem fontCombo = new ComboBoxBarItem {
    Text = "Font",
    Tooltip = "Select or type font name",
    ShowToolTipInPopUp = true,
    TextBoxValue = "Arial"
};
fontCombo.ChoiceList.AddRange(new string[] { "Arial", "Calibri", "Times New Roman" });

// DropDownBarItem
DropDownBarItem colorDropdown = new DropDownBarItem {
    Text = "Color",
    Tooltip = "Choose text color",
    ShowToolTipInPopUp = true
};

// TextBoxBarItem
TextBoxBarItem searchBox = new TextBoxBarItem {
    Text = "Search",
    Tooltip = "Enter search terms",
    ShowToolTipInPopUp = true,
    MinWidth = 120
};
```

### Multi-Line Tooltips

```csharp
BarItem importItem = new BarItem();
importItem.Text = "Import...";
importItem.Tooltip = "Import data from external files\n" +
                     "Supported formats: CSV, XML, JSON\n" +
                     "Ctrl+I to open import dialog";
importItem.ShowToolTipInPopUp = true;
importItem.Shortcut = Shortcut.CtrlI;
```

## Best Practices

### Image Guidelines

**Size and Format:**
- Use consistent icon sizes (16x16 or 24x24)
- PNG format with transparency
- Design for both light and dark themes
- Test at different DPI settings

**Visual Design:**
- Use clear, recognizable icons
- Maintain consistent style across all icons
- Avoid overly detailed icons at small sizes
- Use color meaningfully (red for delete, green for add, etc.)

**Performance:**
- Load images once, reuse ImageExt instances
- Store images in project resources
- Avoid loading large images for menu icons
- Consider image caching for dynamic menus

### State Images

**When to Use:**
- **DisabledImage:** Always provide for better visual feedback
- **HighlightedImage:** Optional, use for important commands
- **Consistency:** Use same style for all state images

**Design Tips:**
- Disabled: Grayscale or 50% opacity
- Highlighted: Brighter or different color accent
- Test all states for visibility

### Tooltip Guidelines

**Content:**
- Keep concise (one line preferred)
- Describe what the command does
- Include keyboard shortcut if applicable
- Use sentence case ("Save the document", not "save the document")

**When to Use:**
- Icon-only menu items (always)
- Commands with shortcuts (show shortcut in tooltip)
- Complex or uncommon features (provide clarification)
- Dynamic commands (explain current state)

**When to Skip:**
- Self-explanatory text-only items
- Very simple commands (Cut, Copy, Paste with text visible)
- When tooltip would just repeat the text

### Accessibility

**Images:**
- Always include text with images (don't rely on icons alone)
- Provide clear disabled state visuals
- Test icon visibility with color blindness simulators

**Tooltips:**
- Enable for screen reader compatibility
- Write descriptive text
- Include keyboard shortcuts

## Common Patterns

### Standard Edit Menu with Images

```csharp
BarItem[] editMenuItems = new BarItem[]
{
    new BarItem {
        Text = "Undo",
        Image = new ImageExt(Properties.Resources.Undo),
        DisabledImage = CreateDisabledImage(Properties.Resources.Undo),
        Shortcut = Shortcut.CtrlZ,
        Tooltip = "Undo the last action (Ctrl+Z)",
        ShowToolTipInPopUp = true
    },
    new BarItem {
        Text = "Redo",
        Image = new ImageExt(Properties.Resources.Redo),
        DisabledImage = CreateDisabledImage(Properties.Resources.Redo),
        Shortcut = Shortcut.CtrlY,
        Tooltip = "Redo the last undone action (Ctrl+Y)",
        ShowToolTipInPopUp = true
    },
    new BarItem {
        Text = "Cut",
        Image = new ImageExt(Properties.Resources.Cut),
        DisabledImage = CreateDisabledImage(Properties.Resources.Cut),
        Shortcut = Shortcut.CtrlX,
        Tooltip = "Cut selected text to clipboard (Ctrl+X)",
        ShowToolTipInPopUp = true
    },
    new BarItem {
        Text = "Copy",
        Image = new ImageExt(Properties.Resources.Copy),
        DisabledImage = CreateDisabledImage(Properties.Resources.Copy),
        Shortcut = Shortcut.CtrlC,
        Tooltip = "Copy selected text to clipboard (Ctrl+C)",
        ShowToolTipInPopUp = true
    },
    new BarItem {
        Text = "Paste",
        Image = new ImageExt(Properties.Resources.Paste),
        DisabledImage = CreateDisabledImage(Properties.Resources.Paste),
        Shortcut = Shortcut.CtrlV,
        Tooltip = "Paste from clipboard (Ctrl+V)",
        ShowToolTipInPopUp = true
    }
};

parentBarItem1.Items.AddRange(editMenuItems);

// Add grouping
parentBarItem1.BeginGroupAt(editMenuItems[2]);  // Separator before Cut
```

### Icon-Only Menu (Tooltips Essential)

```csharp
// For toolbar-style popup menus
BarItem[] toolbarItems = new BarItem[]
{
    new BarItem {
        Image = new ImageExt(Properties.Resources.Bold),
        Tooltip = "Bold (Ctrl+B)",
        ShowToolTipInPopUp = true,
        Tag = "Bold"
    },
    new BarItem {
        Image = new ImageExt(Properties.Resources.Italic),
        Tooltip = "Italic (Ctrl+I)",
        ShowToolTipInPopUp = true,
        Tag = "Italic"
    },
    new BarItem {
        Image = new ImageExt(Properties.Resources.Underline),
        Tooltip = "Underline (Ctrl+U)",
        ShowToolTipInPopUp = true,
        Tag = "Underline"
    }
};

parentBarItem1.Items.AddRange(toolbarItems);
```

## Troubleshooting

**Issue: Images don't appear**
- Verify file path is correct
- Check that image file exists
- Ensure ImageExt wrapper is used
- Verify SizeToFit = true on BarItem

**Issue: DisabledImage not showing**
- Confirm Enabled = false is set
- Verify DisabledImage property is assigned
- Check that image file exists and loads correctly

**Issue: Tooltips don't appear**
- Verify ShowToolTipInPopUp = true
- Check that Tooltip property has text
- Ensure menu is actually being shown (hover long enough)

**Issue: Images are blurry or pixelated**
- Use higher resolution images (24x24 or 32x32)
- Ensure DPI-appropriate images
- Use PNG format for best quality
- Test on high-DPI displays
