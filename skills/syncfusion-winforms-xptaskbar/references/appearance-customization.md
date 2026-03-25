# Appearance and Customization

## Table of Contents
- [Header Colors and Fonts](#header-colors-and-fonts)
- [Brush Customization Events](#brush-customization-events)
- [Custom Background Drawing](#custom-background-drawing)
- [Images in Headers and Items](#images-in-headers-and-items)
- [Tooltips](#tooltips)

## Header Colors and Fonts

### Basic Color Customization

Customize the header appearance with colors and fonts:

```csharp
XPTaskBarBox box = new XPTaskBarBox();
box.Text = "File Operations";

// Set header colors
box.HeaderBackColor = System.Drawing.Color.LightBlue;
box.HeaderForeColor = System.Drawing.Color.DarkBlue;
```

**VB.NET:**

```vb
Dim box As New XPTaskBarBox()
box.Text = "File Operations"
box.HeaderBackColor = System.Drawing.Color.LightBlue
box.HeaderForeColor = System.Drawing.Color.DarkBlue
```

### Font Customization

Control font family, size, and style:

```csharp
// Create custom font
var font = new System.Drawing.Font(
    "Segoe UI",                           // Family
    10F,                                  // Size
    System.Drawing.FontStyle.Bold,        // Style (Bold)
    System.Drawing.GraphicsUnit.Point     // Unit
);

box.HeaderFont = font;
```

**VB.NET:**

```vb
Dim font As New System.Drawing.Font("Segoe UI", 10F, System.Drawing.FontStyle.Bold, System.Drawing.GraphicsUnit.Point)
box.HeaderFont = font
```

### Font Styles

Combine font styles using bitwise OR:

```csharp
// Multiple styles
var fontStyle = System.Drawing.FontStyle.Bold | System.Drawing.FontStyle.Italic;
var font = new System.Drawing.Font("Arial", 10F, fontStyle, System.Drawing.GraphicsUnit.Point);

box.HeaderFont = font;
```

Available styles:
- `FontStyle.Regular` - Normal text
- `FontStyle.Bold` - Thick text
- `FontStyle.Italic` - Slanted text
- `FontStyle.Underline` - Underlined text
- `FontStyle.Strikeout` - Text with line through it

### Text Alignment

Align header text within the header area:

```csharp
// Left alignment (default)
box.HeaderTextAlign = System.Drawing.StringAlignment.Near;

// Center alignment
box.HeaderTextAlign = System.Drawing.StringAlignment.Center;

// Right alignment
box.HeaderTextAlign = System.Drawing.StringAlignment.Far;
```

**VB.NET:**

```vb
box.HeaderTextAlign = System.Drawing.StringAlignment.Center
```

## Brush Customization Events

### ProvideHeaderBackgroundBrush Event

Customize the header background with gradient, pattern, or solid brushes:

```csharp
box.ProvideHeaderBackgroundBrush += (sender, e) => {
    if (e.Bounds.Width > 0 && e.Bounds.Height > 0) {
        // Create a linear gradient brush
        var brush = new System.Drawing.Drawing2D.LinearGradientBrush(
            e.Bounds,
            System.Drawing.Color.LightBlue,      // Start color
            System.Drawing.Color.DarkBlue,       // End color
            0F,                                  // Angle (0 = left to right)
            false
        );
        
        e.Brush = brush;
    }
};
```

**VB.NET:**

```vb
AddHandler box.ProvideHeaderBackgroundBrush, Sub(sender, e)
    If e.Bounds.Width > 0 AndAlso e.Bounds.Height > 0 Then
        Dim brush As New System.Drawing.Drawing2D.LinearGradientBrush(
            e.Bounds,
            System.Drawing.Color.LightBlue,
            System.Drawing.Color.DarkBlue,
            0F,
            False
        )
        e.Brush = brush
    End If
End Sub
```

### Diagonal Gradient Example

```csharp
box.ProvideHeaderBackgroundBrush += (sender, e) => {
    if (e.Bounds.Width > 0 && e.Bounds.Height > 0) {
        // 45-degree diagonal gradient
        var brush = new System.Drawing.Drawing2D.LinearGradientBrush(
            e.Bounds,
            System.Drawing.Color.White,
            System.Drawing.Color.LightGray,
            45F,          // 45-degree angle
            true
        );
        
        e.Brush = brush;
    }
};
```

### Multi-Color Gradient with Blend

```csharp
box.ProvideHeaderBackgroundBrush += (sender, e) => {
    if (e.Bounds.Width > 0 && e.Bounds.Height > 0) {
        var brush = new System.Drawing.Drawing2D.LinearGradientBrush(
            e.Bounds,
            System.Drawing.Color.White,
            System.Drawing.Color.DarkBlue,
            0,
            true
        );
        
        // Define blend positions and factors
        var blend = new System.Drawing.Drawing2D.Blend();
        blend.Positions = new float[] { 0.0F, 0.5F, 1.0F };
        blend.Factors = new float[] { 0.0F, 0.5F, 1.0F };
        
        brush.Blend = blend;
        e.Brush = brush;
    }
};
```

### ProvideItemsBackgroundBrush Event

Customize the background of the items area:

```csharp
box.ProvideItemsBackgroundBrush += (sender, e) => {
    if (e.Bounds.Width > 0 && e.Bounds.Height > 0) {
        // Create subtle gradient for items area
        var brush = new System.Drawing.Drawing2D.LinearGradientBrush(
            e.Bounds,
            System.Drawing.Color.FromArgb(250, 250, 250),  // Light gray
            System.Drawing.Color.White,
            90F,  // Vertical gradient
            true
        );
        
        e.Brush = brush;
    }
};
```

**VB.NET:**

```vb
AddHandler box.ProvideItemsBackgroundBrush, Sub(sender, e)
    If e.Bounds.Width > 0 AndAlso e.Bounds.Height > 0 Then
        Dim brush As New System.Drawing.Drawing2D.LinearGradientBrush(
            e.Bounds,
            System.Drawing.Color.FromArgb(250, 250, 250),
            System.Drawing.Color.White,
            90F,
            True
        )
        e.Brush = brush
    End If
End Sub
```

## Custom Background Drawing

### Combining Header and Items Brushes

```csharp
private void SetupCustomAppearance(XPTaskBarBox box) {
    // Header with blue gradient
    box.ProvideHeaderBackgroundBrush += (sender, e) => {
        if (e.Bounds.Width > 0 && e.Bounds.Height > 0) {
            var brush = new System.Drawing.Drawing2D.LinearGradientBrush(
                e.Bounds,
                System.Drawing.Color.LightBlue,
                System.Drawing.Color.RoyalBlue,
                0,
                true
            );
            e.Brush = brush;
        }
    };
    
    // Items area with subtle texture
    box.ProvideItemsBackgroundBrush += (sender, e) => {
        if (e.Bounds.Width > 0 && e.Bounds.Height > 0) {
            var brush = new System.Drawing.Drawing2D.LinearGradientBrush(
                e.Bounds,
                System.Drawing.Color.AliceBlue,
                System.Drawing.Color.Azure,
                90,
                true
            );
            e.Brush = brush;
        }
    };
}
```

### Conditional Styling Based on State

```csharp
box.ProvideHeaderBackgroundBrush += (sender, e) => {
    XPTaskBarBox currentBox = sender as XPTaskBarBox;
    
    if (e.Bounds.Width > 0 && e.Bounds.Height > 0) {
        System.Drawing.Color startColor, endColor;
        
        if (currentBox != null && currentBox.Collapsed) {
            // Collapsed state - use gray
            startColor = System.Drawing.Color.LightGray;
            endColor = System.Drawing.Color.Gray;
        } else {
            // Expanded state - use blue
            startColor = System.Drawing.Color.LightBlue;
            endColor = System.Drawing.Color.DarkBlue;
        }
        
        var brush = new System.Drawing.Drawing2D.LinearGradientBrush(
            e.Bounds,
            startColor,
            endColor,
            0,
            true
        );
        
        e.Brush = brush;
    }
};
```

## Images in Headers and Items

### Adding Images to Headers

Use an ImageList to add images to box headers:

```csharp
// Create or get ImageList with images
ImageList headerImageList = new ImageList();
// Assume images have been added via designer or code

// Assign ImageList to box
box.HeaderImageList = headerImageList;

// Set specific image for header
box.HeaderImageIndex = 0;  // First image in the list
```

**VB.NET:**

```vb
box.HeaderImageList = headerImageList
box.HeaderImageIndex = 0
```

### Adding Images to Items

Display icons next to item text:

```csharp
// Create ImageList for items
ImageList itemImageList = new ImageList();
// Add images to itemImageList

// Assign to box
box.ImageList = itemImageList;

// Set image for each item
box.Items[0].ImageIndex = 0;  // Use first image
box.Items[1].ImageIndex = 1;  // Use second image
box.Items[2].ImageIndex = 2;  // Use third image
```

**VB.NET:**

```vb
box.ImageList = itemImageList
box.Items(0).ImageIndex = 0
box.Items(1).ImageIndex = 1
box.Items(2).ImageIndex = 2
```

### Complete Image Example

```csharp
private void SetupImagesForBox(XPTaskBarBox box) {
    // Create ImageList
    ImageList imageList = new ImageList();
    imageList.ImageSize = new System.Drawing.Size(16, 16);
    
    // Add images (assuming you have image resources)
    // imageList.Images.Add(Properties.Resources.NewFile);
    // imageList.Images.Add(Properties.Resources.OpenFile);
    // imageList.Images.Add(Properties.Resources.SaveFile);
    
    // Assign to box
    box.ImageList = imageList;
    
    // Create items with images
    box.Items.AddRange(new XPTaskBarItem[] {
        new XPTaskBarItem("New", System.Drawing.Color.Empty, 0, "file_new"),
        new XPTaskBarItem("Open", System.Drawing.Color.Empty, 1, "file_open"),
        new XPTaskBarItem("Save", System.Drawing.Color.Empty, 2, "file_save")
    });
}
```

## Tooltips

### Enabling Tooltips

```csharp
// Enable tooltip display
box.ShowToolTip = true;
```

**VB.NET:**

```vb
box.ShowToolTip = True
```

### Setting Tooltip Text

```csharp
// Set tooltip for individual items
box.Items[0].ToolTipText = "Create a new document";
box.Items[1].ToolTipText = "Open an existing document";
box.Items[2].ToolTipText = "Save the current document";
```

**VB.NET:**

```vb
box.Items(0).ToolTipText = "Create a new document"
box.Items(1).ToolTipText = "Open an existing document"
box.Items(2).ToolTipText = "Save the current document"
```

### Tooltip Example with Items

```csharp
private void AddItemsWithTooltips(XPTaskBarBox box) {
    box.ShowToolTip = true;
    
    var itemsData = new[] {
        ("New Document", "file_new", "Create a new document (Ctrl+N)"),
        ("Open File", "file_open", "Open an existing file (Ctrl+O)"),
        ("Save", "file_save", "Save current document (Ctrl+S)"),
        ("Save As", "file_saveas", "Save document with new name (Ctrl+Shift+S)")
    };
    
    foreach (var (text, tag, tooltip) in itemsData) {
        var item = new XPTaskBarItem(text, System.Drawing.Color.Empty, -1, tag);
        item.ToolTipText = tooltip;
        box.Items.Add(item);
    }
}
```

## Complete Customization Example

```csharp
private void CreateCustomizedBox() {
    var box = new XPTaskBarBox();
    box.Text = "Customized Tasks";
    
    // Header styling
    box.HeaderBackColor = System.Drawing.Color.LightBlue;
    box.HeaderForeColor = System.Drawing.Color.DarkBlue;
    box.HeaderFont = new System.Drawing.Font("Segoe UI", 10F, System.Drawing.FontStyle.Bold);
    box.HeaderTextAlign = System.Drawing.StringAlignment.Center;
    
    // Brush customization
    box.ProvideHeaderBackgroundBrush += (sender, e) => {
        if (e.Bounds.Width > 0 && e.Bounds.Height > 0) {
            var brush = new System.Drawing.Drawing2D.LinearGradientBrush(
                e.Bounds,
                System.Drawing.Color.LightBlue,
                System.Drawing.Color.RoyalBlue,
                0,
                true
            );
            e.Brush = brush;
        }
    };
    
    // Items with images and tooltips
    box.ShowToolTip = true;
    
    var items = new[] {
        ("New", "new", 0, "Create new"),
        ("Open", "open", 1, "Open file"),
        ("Save", "save", 2, "Save file")
    };
    
    foreach (var (text, tag, imgIdx, tooltip) in items) {
        var item = new XPTaskBarItem(text, System.Drawing.Color.Empty, imgIdx, tag);
        item.ToolTipText = tooltip;
        box.Items.Add(item);
    }
}
```

## Next Steps

- See [images-and-content.md](items-and-content.md) for more item management
- See [behavior-and-events.md](behavior-and-events.md) for event handling
- See [padding-spacing-scrolling.md](padding-spacing-scrolling.md) for layout control
