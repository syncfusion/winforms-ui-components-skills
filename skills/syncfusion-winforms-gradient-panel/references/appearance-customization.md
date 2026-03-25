# Appearance Customization

## Table of Contents
- [Overview](#overview)
- [Foreground Settings](#foreground-settings)
- [Image Settings](#image-settings)
- [Combining Styles](#combining-styles)
- [Common Scenarios](#common-scenarios)
- [Troubleshooting](#troubleshooting)

## Overview

Beyond background gradients and patterns, GradientPanel offers comprehensive appearance customization including text styling, background images, and layering effects.

**Customization options:**
- Foreground colors and fonts
- Background images with layout modes
- Combining gradients with images
- Text and graphics styling

## Foreground Settings

### Font Property

Customize the font style for text displayed in the panel:

```csharp
gradientPanel1.Font = new Font("Verdana", 8.25f, FontStyle.Bold);
```

**Font parameters:**
- Font family: "Segoe UI", "Arial", "Verdana", etc.
- Size: Float value (e.g., 8.25f, 12f, 16f)
- Style: Regular, Bold, Italic, Bold | Italic, Underline, Strikeout

### ForeColor Property

Sets the color for text and graphics in the panel:

```csharp
gradientPanel1.ForeColor = Color.Blue;
```

**Common uses:**
- Text color for labels and child controls (if they inherit)
- Default foreground color for graphics rendering

### Complete Foreground Example

```csharp
GradientPanel panel = new GradientPanel();
panel.Size = new Size(300, 200);
panel.Location = new Point(20, 20);

// Background
panel.BackgroundColor = new BrushInfo(Color.LightGray);

// Foreground styling
panel.Font = new Font("Comic Sans MS", 9.75f, FontStyle.Bold);
panel.ForeColor = Color.Blue;

this.Controls.Add(panel);
```

![Foreground Settings](../../../docs/GradientPanel-Images/Overview_img368.jpeg)

### Font Styling Variations

```csharp
// Modern sans-serif
panel.Font = new Font("Segoe UI", 12f, FontStyle.Regular);

// Bold heading style
panel.Font = new Font("Segoe UI", 16f, FontStyle.Bold);

// Italic emphasis
panel.Font = new Font("Arial", 10f, FontStyle.Italic);

// Combined styles
panel.Font = new Font("Times New Roman", 11f, FontStyle.Bold | FontStyle.Italic);
```

### Color Variations

```csharp
// Named colors
panel.ForeColor = Color.Navy;
panel.ForeColor = Color.DarkSlateGray;

// RGB colors
panel.ForeColor = Color.FromArgb(0, 120, 215);

// ARGB colors with transparency
panel.ForeColor = Color.FromArgb(200, 0, 0, 255);  // Semi-transparent blue
```

## Image Settings

### BackgroundImage Property

Set a background image for the panel:

```csharp
// From file
gradientPanel1.BackgroundImage = Image.FromFile("background.jpg");

// From resources
gradientPanel1.BackgroundImage = Properties.Resources.BackgroundImage;

// From embedded resources
var assembly = System.Reflection.Assembly.GetExecutingAssembly();
var stream = assembly.GetManifestResourceStream("Namespace.background.jpg");
gradientPanel1.BackgroundImage = Image.FromStream(stream);
```

### BackgroundImageLayout Property

Specifies how the background image is displayed:

```csharp
gradientPanel1.BackgroundImageLayout = ImageLayout.Stretch;
```

**ImageLayout options:**

#### None
Image displayed at top-left, no scaling:

```csharp
gradientPanel1.BackgroundImageLayout = ImageLayout.None;
```

**Use when:** Original image size is correct, no distortion wanted

#### Tile
Image repeated to fill panel:

```csharp
gradientPanel1.BackgroundImageLayout = ImageLayout.Tile;
```

**Use when:** Small pattern images, seamless textures

#### Center
Image centered, no scaling:

```csharp
gradientPanel1.BackgroundImageLayout = ImageLayout.Center;
```

**Use when:** Image fits panel, want center alignment

#### Stretch
Image stretched to fill panel (may distort):

```csharp
gradientPanel1.BackgroundImageLayout = ImageLayout.Stretch;
```

**Use when:** Image must fill entire panel, distortion acceptable

![Image Stretch Layout](../../../docs/GradientPanel-Images/Overview_img369.jpeg)

#### Zoom
Image scaled proportionally to fit panel:

```csharp
gradientPanel1.BackgroundImageLayout = ImageLayout.Zoom;
```

**Use when:** Maintain aspect ratio, image should fit without distortion

### Complete Image Example

```csharp
GradientPanel imagePanel = new GradientPanel();
imagePanel.Size = new Size(400, 300);
imagePanel.Location = new Point(20, 20);

// Set background image
imagePanel.BackgroundImage = Image.FromFile("landscape.jpg");
imagePanel.BackgroundImageLayout = ImageLayout.Stretch;

// Optional border
imagePanel.BorderStyle = BorderStyle.FixedSingle;
imagePanel.BorderColor = Color.DarkGray;

this.Controls.Add(imagePanel);
```

## Combining Styles

### Gradient + Image

Combine gradient backgrounds with images for layered effects:

```csharp
GradientPanel layeredPanel = new GradientPanel();

// Set gradient background
layeredPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.FromArgb(100, 0, 0, 255),    // Semi-transparent blue
    Color.FromArgb(100, 255, 255, 255) // Semi-transparent white
);

// Add background image
layeredPanel.BackgroundImage = Image.FromFile("texture.png");
layeredPanel.BackgroundImageLayout = ImageLayout.Tile;
```

**Visual effect:** Image shows through semi-transparent gradient

### Pattern + Image

```csharp
GradientPanel texturedPanel = new GradientPanel();

// Set pattern background
texturedPanel.BackgroundColor = new BrushInfo(
    PatternStyle.DottedGrid,
    Color.FromArgb(150, Color.Black),   // Semi-transparent
    Color.Transparent
);

// Add background image
texturedPanel.BackgroundImage = Image.FromFile("photo.jpg");
texturedPanel.BackgroundImageLayout = ImageLayout.Zoom;
```

**Visual effect:** Pattern overlay on image

### Solid + Image with Transparent Controls

```csharp
GradientPanel panel = new GradientPanel();

// Background image
panel.BackgroundImage = Image.FromFile("background.jpg");
panel.BackgroundImageLayout = ImageLayout.Stretch;

// Add transparent label
Label label = new Label();
label.Text = "Overlay Text";
label.Font = new Font("Segoe UI", 24, FontStyle.Bold);
label.ForeColor = Color.White;
label.BackColor = Color.Transparent;  // Show background through
label.AutoSize = true;
label.Location = new Point(20, 20);
panel.Controls.Add(label);
```

### Transparent Overlay Effect

```csharp
// Create semi-transparent overlay on image
GradientPanel overlayPanel = new GradientPanel();

// Background image
overlayPanel.BackgroundImage = Image.FromFile("photo.jpg");
overlayPanel.BackgroundImageLayout = ImageLayout.Zoom;

// Semi-transparent solid overlay
overlayPanel.BackColor = Color.FromArgb(150, 0, 0, 0);  // Semi-transparent black

// White text shows clearly over dark overlay
Label overlayText = new Label();
overlayText.Text = "Important Message";
overlayText.ForeColor = Color.White;
overlayText.BackColor = Color.Transparent;
overlayText.Font = new Font("Segoe UI", 16, FontStyle.Bold);
overlayText.AutoSize = true;
overlayText.Location = new Point(50, 50);
overlayPanel.Controls.Add(overlayText);
```

## Common Scenarios

### Scenario 1: Photo Background Panel

```csharp
GradientPanel photoPanel = new GradientPanel();
photoPanel.Size = new Size(500, 400);
photoPanel.Location = new Point(20, 20);

// Set photo as background
photoPanel.BackgroundImage = Image.FromFile("family_photo.jpg");
photoPanel.BackgroundImageLayout = ImageLayout.Zoom;

// Add semi-transparent overlay for text visibility
photoPanel.BackColor = Color.FromArgb(100, 0, 0, 0);

// Add caption
Label caption = new Label();
caption.Text = "Family Vacation 2026";
caption.Font = new Font("Segoe UI", 18, FontStyle.Bold);
caption.ForeColor = Color.White;
caption.BackColor = Color.Transparent;
caption.AutoSize = true;
caption.Location = new Point(20, 350);
photoPanel.Controls.Add(caption);

this.Controls.Add(photoPanel);
```

### Scenario 2: Textured Panel

```csharp
GradientPanel texturedPanel = new GradientPanel();
texturedPanel.Dock = DockStyle.Fill;

// Tiled texture image
texturedPanel.BackgroundImage = Image.FromFile("paper_texture.jpg");
texturedPanel.BackgroundImageLayout = ImageLayout.Tile;

// Dark text on light texture
texturedPanel.ForeColor = Color.Black;
texturedPanel.Font = new Font("Georgia", 11f, FontStyle.Regular);

this.Controls.Add(texturedPanel);
```

### Scenario 3: Logo Background

```csharp
GradientPanel logoPanel = new GradientPanel();
logoPanel.Size = new Size(300, 200);

// Gradient background
logoPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.DarkBlue,
    Color.LightBlue
);

// Centered logo
logoPanel.BackgroundImage = Image.FromFile("company_logo.png");
logoPanel.BackgroundImageLayout = ImageLayout.Center;

this.Controls.Add(logoPanel);
```

### Scenario 4: Dashboard Section with Icon

```csharp
GradientPanel dashboardSection = new GradientPanel();
dashboardSection.Size = new Size(250, 150);

// Gradient background
dashboardSection.BackgroundColor = new BrushInfo(
    GradientStyle.PathRectangle,
    Color.FromArgb(0, 120, 215),
    Color.FromArgb(0, 80, 150)
);

// Watermark icon (semi-transparent)
dashboardSection.BackgroundImage = Image.FromFile("icon_watermark.png");
dashboardSection.BackgroundImageLayout = ImageLayout.Zoom;

// Overlay text
Label titleLabel = new Label();
titleLabel.Text = "Statistics";
titleLabel.Font = new Font("Segoe UI", 16, FontStyle.Bold);
titleLabel.ForeColor = Color.White;
titleLabel.BackColor = Color.Transparent;
titleLabel.AutoSize = true;
titleLabel.Location = new Point(15, 15);
dashboardSection.Controls.Add(titleLabel);

Label valueLabel = new Label();
valueLabel.Text = "1,234";
valueLabel.Font = new Font("Segoe UI", 32, FontStyle.Bold);
valueLabel.ForeColor = Color.White;
valueLabel.BackColor = Color.Transparent;
valueLabel.AutoSize = true;
valueLabel.Location = new Point(15, 50);
dashboardSection.Controls.Add(valueLabel);
```

### Scenario 5: Hero Section

```csharp
GradientPanel heroPanel = new GradientPanel();
heroPanel.Dock = DockStyle.Top;
heroPanel.Height = 300;

// Hero image
heroPanel.BackgroundImage = Image.FromFile("hero_image.jpg");
heroPanel.BackgroundImageLayout = ImageLayout.Stretch;

// Dark gradient overlay for text
heroPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.FromArgb(150, 0, 0, 0),      // Dark at top
    Color.FromArgb(50, 0, 0, 0)        // Lighter at bottom
);

// Hero text
Label heroTitle = new Label();
heroTitle.Text = "Welcome to Our Application";
heroTitle.Font = new Font("Segoe UI", 36, FontStyle.Bold);
heroTitle.ForeColor = Color.White;
heroTitle.BackColor = Color.Transparent;
heroTitle.AutoSize = true;
heroTitle.Location = new Point(50, 100);
heroPanel.Controls.Add(heroTitle);

this.Controls.Add(heroPanel);
```

## Troubleshooting

### Issue: Image not displaying

**Causes:**
- File path incorrect
- Image file not found
- Image file locked by another process

**Solutions:**
```csharp
// Verify file exists
if (File.Exists("background.jpg"))
{
    gradientPanel1.BackgroundImage = Image.FromFile("background.jpg");
}
else
{
    MessageBox.Show("Image file not found");
}

// Use absolute path for testing
gradientPanel1.BackgroundImage = Image.FromFile(@"C:\Images\background.jpg");

// Use resources (recommended for deployment)
gradientPanel1.BackgroundImage = Properties.Resources.BackgroundImage;
```

### Issue: Image distorted

**Cause:** Using `Stretch` layout mode

**Solution:**
```csharp
// Use Zoom to maintain aspect ratio
gradientPanel1.BackgroundImageLayout = ImageLayout.Zoom;

// Or use Center if image is correct size
gradientPanel1.BackgroundImageLayout = ImageLayout.Center;
```

### Issue: Text not visible over image

**Causes:**
- Low contrast between text and image
- No overlay for text visibility

**Solutions:**
```csharp
// Add semi-transparent overlay
gradientPanel1.BackColor = Color.FromArgb(150, 0, 0, 0);  // Dark overlay

// Or use contrasting text color
label.ForeColor = Color.White;
label.Font = new Font("Segoe UI", 14, FontStyle.Bold);  // Larger, bold

// Or add text shadow/outline effect (requires custom painting)
```

### Issue: Gradient not visible with image

**Cause:** Image is opaque, covers gradient

**Solution:**
```csharp
// Use semi-transparent image (PNG with alpha)
// Or use semi-transparent gradient over image
gradientPanel1.BackgroundImage = Image.FromFile("background.jpg");
gradientPanel1.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.FromArgb(100, 0, 0, 255),    // Semi-transparent
    Color.FromArgb(100, 255, 255, 255)
);
```

### Issue: Font not applied to child controls

**Cause:** Child controls have their own Font property set

**Solution:**
```csharp
// Explicitly set child control fonts, or leave unset to inherit
foreach (Control control in gradientPanel1.Controls)
{
    if (control is Label label)
    {
        // Leave Font unset to inherit from panel
        // Or explicitly set: label.Font = gradientPanel1.Font;
    }
}
```

## Best Practices

1. **Use resources for images** - Embed images in project resources for reliable deployment
2. **Zoom for photos** - Use `ImageLayout.Zoom` to maintain aspect ratio
3. **Tile for textures** - Use `ImageLayout.Tile` for seamless textures
4. **Transparent overlays** - Add semi-transparent overlays for text visibility over images
5. **High contrast text** - Ensure text is readable over images/gradients
6. **Optimize image size** - Use appropriately sized images to reduce memory usage
7. **Transparent child controls** - Set child `BackColor = Color.Transparent` to show background
8. **Test visibility** - Verify text/controls are visible over all background styles
9. **Consistent fonts** - Use consistent font families across application
10. **Accessible colors** - Ensure sufficient contrast for accessibility (WCAG guidelines)
