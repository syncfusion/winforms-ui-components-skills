# Button Types & Content in SfButton

## Table of Contents

- [Text and Image Button](#text-and-image-button)
- [Image-Only Button](#image-only-button)
- [Icon-Only Button](#icon-only-button)
- [Positioning Text and Image](#positioning-text-and-image)
- [Customizing Image Size](#customizing-image-size)
- [Auto-Sizing](#auto-sizing)
- [Spacing Between Text and Image](#spacing-between-text-and-image)
- [Rich Text Support](#rich-text-support)
- [Text Wrapping](#text-wrapping)
- [Text Trimming with Ellipsis](#text-trimming-with-ellipsis)
- [Content Alignment](#content-alignment)

---

## Text and Image Button

### Basic Text + Image Button

Display both text and an image on a single button:

```csharp
// Adding text
this.sfButton1.Text = "Print";

// Adding an image
this.sfButton1.Image = Image.FromFile(@"path\to\print-icon.png");
```

**Result:** Button displays both the text "Print" and the icon.

### Designer Setup

1. Select SfButton in Designer
2. In Properties, find `Image` property
3. Click the `...` button to browse and select an image file
4. Set `Text` property to desired text
5. Preview shows text and image arrangement

---

## Image-Only Button

Create buttons that display only an image, no text:

```csharp
// Set empty text
this.sfButton1.Text = "";

// Set the image
this.sfButton1.Image = Image.FromFile(@"path\to\save-icon.png");

// Adjust image size as needed
this.sfButton1.ImageSize = new Size(32, 32);
```

**Use Case:** Toolbar buttons with standard sizes, icon galleries, navigation buttons with visual clarity.

---

## Icon-Only Button

Create clean icon buttons that blend with backgrounds (useful for transparent or borderless buttons):

```csharp
// Empty text and image
this.sfButton1.Text = "";
this.sfButton1.Image = Image.FromFile(@"path\to\icon.png");

// Match button background to form background (make it invisible)
this.sfButton1.Style.BackColor = Color.White;  // Same as form background
this.sfButton1.Style.DisabledBackColor = Color.White;
this.sfButton1.Style.FocusedBackColor = Color.White;
this.sfButton1.Style.HoverBackColor = Color.White;

// Remove borders for clean appearance
this.sfButton1.Style.Border = null;
this.sfButton1.Style.HoverBorder = null;
this.sfButton1.Style.FocusedBorder = null;
this.sfButton1.Style.PressedBorder = null;
```

**Result:** Icon appears to float on the form background with hover effects only on the image.

---

## Positioning Text and Image

### TextImageRelation Property

Control where the image appears relative to text:

```csharp
// Image to the left of text (default)
sfButton1.TextImageRelation = TextImageRelation.ImageBeforeText;
// Result: [IMAGE] Text

// Image to the right of text
sfButton1.TextImageRelation = TextImageRelation.TextBeforeImage;
// Result: Text [IMAGE]

// Image above text
sfButton1.TextImageRelation = TextImageRelation.ImageAboveText;
// Result:    [IMAGE]
//            Text

// Image below text
sfButton1.TextImageRelation = TextImageRelation.TextAboveImage;
// Result:      Text
//           [IMAGE]

// Image overlays text (centered)
sfButton1.TextImageRelation = TextImageRelation.Overlay;
// Result: [IMAGE + Text mixed]
```

### Complete Example

```csharp
// Create button with image and text positioned below image
sfButton1.Text = "Download";
sfButton1.Image = Image.FromFile(@"download-icon.png");
sfButton1.TextImageRelation = TextImageRelation.ImageAboveText;
sfButton1.ImageSize = new Size(24, 24);
```

---

## Customizing Image Size

### ImageSize Property

Control the dimensions of the displayed image:

```csharp
// Setting image size to 15x15 pixels
sfButton1.ImageSize = new Size(15, 15);

// Setting image size to 32x32 pixels (larger icon)
sfButton1.ImageSize = new Size(32, 32);

// Setting image size to 48x48 pixels (extra large)
sfButton1.ImageSize = new Size(48, 48);
```

**Important:** The original image file can be larger; `ImageSize` controls how it's displayed. The control automatically scales the image.

### Size Recommendations

| Use Case | Recommended Size |
|----------|-----------------|
| Small toolbar icons | 16x16 or 20x20 |
| Standard buttons | 24x24 or 32x32 |
| Large buttons | 40x40 or 48x48 |
| Mobile/Touch | 48x48 or 64x64 |

---

## Auto-Sizing

### AutoSize Property

Enable automatic button resizing based on content:

```csharp
// Enable auto-sizing
sfButton1.AutoSize = true;
```

When enabled, the button automatically adjusts its width and height to fit:
- Text length and font size
- Image dimensions
- Padding and margins

### AutoSize Example

```csharp
sfButton1.Text = "SfButton with auto sizing";
sfButton1.Image = Image.FromFile(@"icon.png");
sfButton1.ImageSize = new Size(20, 20);
sfButton1.TextImageRelation = TextImageRelation.ImageBeforeText;
sfButton1.AutoSize = true;  // Button resizes to fit content
```

### AutoSize Constraints

**Disable AutoSize when:**
- You need fixed button dimensions
- Placing buttons in a grid with uniform sizes
- Using `AllowWrapText = true` (incompatible)

---

## Spacing Between Text and Image

### TextMargin Property

Adjust padding around text:

```csharp
// Add 3 pixels padding on all sides of text
sfButton1.TextMargin = new Padding(3, 3, 3, 3);

// Add 5 pixels left/right, 2 pixels top/bottom
sfButton1.TextMargin = new Padding(5, 2, 5, 2);
```

### ImageMargin Property

Adjust padding around image:

```csharp
// Add 3 pixels padding on all sides of image
sfButton1.ImageMargin = new Padding(3, 3, 3, 3);

// Add 4 pixels left/right, 2 pixels top/bottom
sfButton1.ImageMargin = new Padding(4, 2, 4, 2);
```

### Combined Example

```csharp
sfButton1.Text = "Save File";
sfButton1.Image = Image.FromFile(@"save-icon.png");
sfButton1.ImageSize = new Size(24, 24);
sfButton1.TextImageRelation = TextImageRelation.ImageBeforeText;

// Add spacing between image and text
sfButton1.ImageMargin = new Padding(5, 0, 0, 0);  // 5px right of image
sfButton1.TextMargin = new Padding(0, 0, 0, 0);   // No text padding
```

---

## Rich Text Support

### Enabling Rich Text

Enable Rich Text Format (RTF) in buttons:

```csharp
// Enable rich text support
this.sfButton1.AllowRichText = true;

// Add rich text with formatting
this.sfButton1.Text = "{\\rtf1\\ansi\\deff0{\\colortbl;\\red0\\green0\\blue0;\\red255\\green0\\blue0;}" +
    "{\\fonttbl{\\f0 Monotype Corsiva;\r\n}}\\qc\\f0\\fs30 {\\i Italic} {\\b Bold} \\cf2 Red}";
```

### RTF Formatting Examples

```csharp
// Enable first
sfButton1.AllowRichText = true;

// Example 1: Bold and Italic
sfButton1.Text = @"{\rtf1 \b Bold Text \i Italic Text}";

// Example 2: Colored text
sfButton1.Text = @"{\rtf1 {\colortbl;\red0\green0\blue0;\red255\green0\blue0;} 
                   Normal {\cf2 Red Text}}";

// Example 3: Center aligned
sfButton1.Text = @"{\rtf1 \qc Centered Text}";
```

### Important Note

When `AllowRichText = false` (default), RTF codes display as plain text, not formatted. Always set to `true` before adding RTF content.

---

## Text Wrapping

### AllowWrapText Property

Enable text to wrap to multiple lines:

```csharp
// Initialize the text
sfButton1.Text = "This is a button with wrap text on multiple lines";

// Enable text wrapping
sfButton1.AllowWrapText = true;

// Set button size to force wrapping
sfButton1.Size = new Size(80, 80);
```

### Constraints

- **Not compatible with `AutoSize = true`**
- When `AutoSize` is enabled, text does not wrap
- Set fixed button size first, then enable wrapping
- Text wraps within the button boundaries

### Example

```csharp
sfButton1.Text = "Click to Save File";
sfButton1.Size = new Size(60, 60);  // Small square button
sfButton1.AllowWrapText = true;     // Text wraps to fit width
```

---

## Text Trimming with Ellipsis

### AutoEllipsis Property

Show ellipsis (`...`) when text is too long:

```csharp
// Enable AutoEllipsis (ellipsis for trimmed text)
sfButton1.AutoEllipsis = true;
```

### When AutoEllipsis Shows

Ellipsis appears when:
- `AutoSize = false` (disabled)
- Text length exceeds button width
- Text would be cut off

```csharp
// Setup for ellipsis
sfButton1.Text = "This is a very long button text that will be trimmed";
sfButton1.Size = new Size(100, 40);  // Fixed, small size
sfButton1.AutoEllipsis = true;

// Result: "This is a very..." (with ellipsis)
```

---

## Content Alignment

### Text Alignment

Control how text is positioned within the button:

```csharp
// Top-left alignment
sfButton1.TextAlign = ContentAlignment.TopLeft;

// Top-center alignment
sfButton1.TextAlign = ContentAlignment.TopCenter;

// Center alignment (default for most buttons)
sfButton1.TextAlign = ContentAlignment.MiddleCenter;

// Bottom-right alignment
sfButton1.TextAlign = ContentAlignment.BottomRight;

// Complete list of alignments:
// TopLeft, TopCenter, TopRight
// MiddleLeft, MiddleCenter, MiddleRight
// BottomLeft, BottomCenter, BottomRight
```

### Image Alignment

Control how image is positioned within the button:

```csharp
// Top-left alignment
sfButton1.ImageAlign = ContentAlignment.TopLeft;

// Centered alignment
sfButton1.ImageAlign = ContentAlignment.MiddleCenter;

// Bottom-right alignment
sfButton1.ImageAlign = ContentAlignment.BottomRight;
```

### Combined Example

```csharp
sfButton1.Text = "Upload";
sfButton1.Image = Image.FromFile(@"upload-icon.png");
sfButton1.ImageSize = new Size(32, 32);

// Align image top-left, text bottom-center
sfButton1.ImageAlign = ContentAlignment.TopCenter;
sfButton1.TextAlign = ContentAlignment.BottomCenter;
sfButton1.TextImageRelation = TextImageRelation.ImageAboveText;
```

---

## Complete Example: Multi-Button Toolbar

```csharp
// Create a toolbar with different button types
private void CreateToolbar()
{
    // Button 1: Text and Image
    SfButton btnSave = new SfButton();
    btnSave.Text = "Save";
    btnSave.Image = Image.FromFile(@"save.png");
    btnSave.ImageSize = new Size(20, 20);
    btnSave.TextImageRelation = TextImageRelation.ImageBeforeText;
    btnSave.Location = new Point(10, 10);
    btnSave.Size = new Size(100, 30);
    this.Controls.Add(btnSave);

    // Button 2: Image Only
    SfButton btnOpen = new SfButton();
    btnOpen.Text = "";
    btnOpen.Image = Image.FromFile(@"open.png");
    btnOpen.ImageSize = new Size(24, 24);
    btnOpen.Location = new Point(120, 10);
    btnOpen.Size = new Size(40, 30);
    this.Controls.Add(btnOpen);

    // Button 3: Icon Button (borderless)
    SfButton btnDelete = new SfButton();
    btnDelete.Text = "";
    btnDelete.Image = Image.FromFile(@"delete.png");
    btnDelete.ImageSize = new Size(24, 24);
    btnDelete.Style.BackColor = Color.White;
    btnDelete.Style.Border = null;
    btnDelete.Location = new Point(170, 10);
    btnDelete.Size = new Size(40, 30);
    this.Controls.Add(btnDelete);

    // Button 4: Auto-sizing
    SfButton btnCustom = new SfButton();
    btnCustom.Text = "Custom Label";
    btnCustom.Image = Image.FromFile(@"custom.png");
    btnCustom.ImageSize = new Size(20, 20);
    btnCustom.TextImageRelation = TextImageRelation.ImageBeforeText;
    btnCustom.AutoSize = true;
    btnCustom.Location = new Point(220, 10);
    this.Controls.Add(btnCustom);
}
```
