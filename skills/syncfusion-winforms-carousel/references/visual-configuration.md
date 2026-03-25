# Visual Configuration

## Table of Contents
- [Overview](#overview)
- [Perspective Property](#perspective-property)
- [TransitionSpeed Property](#transitionspeed-property)
- [ShowImageShadow Property](#showimageshadow-property)
- [ShowImagePreview Property](#showimagepreview-property)
- [ImageHighlightColor Property](#imagehighlightcolor-property)
- [ImageShadeColor Property](#imageshadecolor-property)
- [Combining Visual Effects](#combining-visual-effects)
- [Performance Considerations](#performance-considerations)

## Overview

The Carousel control provides several visual configuration properties to customize the appearance and behavior of the carousel display. These properties control zoom levels, rotation speed, shadows, previews, and color highlighting.

## Perspective Property

The `Perspective` property controls the zoom level and depth of the elliptical view. It accepts float values to enlarge or shrink the 3D perspective.

### Property Details

- **Type**: `float`
- **Default**: 0
- **Range**: Typically 0.0 to 10.0
- **Recommended**: 2.5 to 5.0 for most scenarios

### Basic Usage

**C# Example:**

```csharp
Carousel carousel1 = new Carousel();
carousel1.Perspective = 4.0f; // Moderate 3D depth
```

**VB.NET Example:**

```vb
Dim carousel1 As New Carousel()
carousel1.Perspective = 4.0F ' Moderate 3D depth
```

### Perspective Values Guide

```csharp
// Low perspective (minimal 3D effect)
carousel1.Perspective = 2.0f;

// Medium perspective (balanced)
carousel1.Perspective = 4.0f;

// High perspective (strong 3D effect)
carousel1.Perspective = 6.0f;

// Very high perspective (extreme zoom)
carousel1.Perspective = 8.0f;
```

### Dynamic Perspective Adjustment

```csharp
private void trackBarPerspective_Scroll(object sender, EventArgs e)
{
    float perspectiveValue = trackBarPerspective.Value / 10.0f; // 0-100 range to 0-10
    carousel1.Perspective = perspectiveValue;
    lblPerspective.Text = $"Perspective: {perspectiveValue:F1}";
}
```

### Perspective by Carousel Path

Different paths work best with different perspective values:

```csharp
switch (carousel1.CarouselPath)
{
    case CarouselPath.Default:
        carousel1.Perspective = 4.5f; // Strong depth for elliptical
        break;
    
    case CarouselPath.Orbital:
        carousel1.Perspective = 3.5f; // Moderate for orbital
        break;
    
    case CarouselPath.Oval:
        carousel1.Perspective = 3.0f; // Lower for wide oval
        break;
    
    case CarouselPath.Linear:
        carousel1.Perspective = 0f; // No perspective for linear
        break;
}
```

## TransitionSpeed Property

The `TransitionSpeed` property controls how fast items rotate when the carousel moves.

### Property Details

- **Type**: `float`
- **Default**: 0
- **Range**: 0.5 to 10.0
- **Recommended**: 1.5 to 3.0 for comfortable viewing

### Basic Usage

**C# Example:**

```csharp
Carousel carousel1 = new Carousel();
carousel1.TransitionSpeed = 2.0f; // Moderate speed
```

**VB.NET Example:**

```vb
Dim carousel1 As New Carousel()
carousel1.TransitionSpeed = 2.0F ' Moderate speed
```

### Speed Values Guide

```csharp
// Slow rotation (leisurely browsing)
carousel1.TransitionSpeed = 1.0f;

// Normal speed (balanced)
carousel1.TransitionSpeed = 2.0f;

// Fast rotation (quick browsing)
carousel1.TransitionSpeed = 4.0f;

// Very fast (rapid navigation)
carousel1.TransitionSpeed = 6.0f;
```

### Speed Control with Slider

```csharp
private void trackBarSpeed_Scroll(object sender, EventArgs e)
{
    float speed = trackBarSpeed.Value / 10.0f; // 5-100 range to 0.5-10
    carousel1.TransitionSpeed = speed;
    lblSpeed.Text = $"Speed: {speed:F1}x";
}
```

### Speed for Different Use Cases

```csharp
// Photo gallery (slower for viewing)
photoCarousel.TransitionSpeed = 1.5f;

// Navigation menu (faster)
navCarousel.TransitionSpeed = 3.5f;

// Auto-rotating dashboard (moderate)
dashboardCarousel.TransitionSpeed = 2.0f;
dashboardCarousel.RotateAlways = true;
```

## ShowImageShadow Property

Enables or disables shadows for carousel images, adding depth to the display.

### Property Details

- **Type**: `bool`
- **Default**: false

### Basic Usage

```csharp
Carousel carousel1 = new Carousel();
carousel1.ImageSlides = true;
carousel1.ShowImageShadow = true; // Enable shadows
```

### Example with Shadow

```csharp
Carousel photoCarousel = new Carousel();
photoCarousel.ImageSlides = true;
photoCarousel.ShowImageShadow = true;
photoCarousel.Perspective = 4.5f;
photoCarousel.ShowImagePreview = true;

// Load images
foreach (string imagePath in imageFiles)
{
    CarouselImage img = new CarouselImage();
    img.ItemImage = Image.FromFile(imagePath);
    photoCarousel.ImageListCollection.Add(img);
}

this.Controls.Add(photoCarousel);
```

### Toggle Shadow at Runtime

```csharp
private void chkShowShadow_CheckedChanged(object sender, EventArgs e)
{
    carousel1.ShowImageShadow = chkShowShadow.Checked;
}
```

## ShowImagePreview Property

Displays a preview of the selected image at the center of the carousel.

### Property Details

- **Type**: `bool`
- **Default**: false

### Basic Usage

```csharp
Carousel carousel1 = new Carousel();
carousel1.ImageSlides = true;
carousel1.ShowImagePreview = true; // Show center preview
```

### Example with Preview

```csharp
Carousel productCarousel = new Carousel();
productCarousel.ImageSlides = true;
productCarousel.Dock = DockStyle.Fill;
productCarousel.ShowImagePreview = true; // Highlight selected item
productCarousel.ShowImageShadow = true;
productCarousel.ImageHighlightColor = Color.FromArgb(255, 215, 0); // Gold border

// Load product images
productCarousel.FilePath = Path.Combine(Application.StartupPath, "Products");

this.Controls.Add(productCarousel);
```

### Toggle Preview

```csharp
private void chkShowPreview_CheckedChanged(object sender, EventArgs e)
{
    carousel1.ShowImagePreview = chkShowPreview.Checked;
}
```

## ImageHighlightColor Property

Applies a color for highlighting the selected/centered image.

### Property Details

- **Type**: `Color`
- **Default**: System default

### Basic Usage

```csharp
Carousel carousel1 = new Carousel();
carousel1.ImageSlides = true;
carousel1.ShowImagePreview = true;
carousel1.ImageHighlightColor = Color.Gold;
```

### Highlight Color Examples

```csharp
// Gold highlight
carousel1.ImageHighlightColor = Color.FromArgb(255, 215, 0);

// Blue highlight
carousel1.ImageHighlightColor = Color.FromArgb(30, 144, 255);

// Green highlight
carousel1.ImageHighlightColor = Color.FromArgb(50, 205, 50);

// Red highlight
carousel1.ImageHighlightColor = Color.FromArgb(220, 20, 60);

// Custom RGB
carousel1.ImageHighlightColor = Color.FromArgb(255, 100, 150, 200);
```

### Dynamic Color Selection

```csharp
private void btnSelectHighlightColor_Click(object sender, EventArgs e)
{
    using (ColorDialog colorDialog = new ColorDialog())
    {
        if (colorDialog.ShowDialog() == DialogResult.OK)
        {
            carousel1.ImageHighlightColor = colorDialog.Color;
        }
    }
}
```

### Themed Highlights

```csharp
private void ApplyTheme(string themeName)
{
    carousel1.ImageSlides = true;
    carousel1.ShowImagePreview = true;
    carousel1.ShowImageShadow = true;
    
    switch (themeName)
    {
        case "Gold":
            carousel1.BackColor = Color.FromArgb(40, 40, 40);
            carousel1.ImageHighlightColor = Color.Gold;
            carousel1.ImageShadeColor = Color.FromArgb(80, 80, 80);
            break;
        
        case "Blue":
            carousel1.BackColor = Color.FromArgb(25, 25, 40);
            carousel1.ImageHighlightColor = Color.DodgerBlue;
            carousel1.ImageShadeColor = Color.FromArgb(60, 60, 80);
            break;
        
        case "Green":
            carousel1.BackColor = Color.FromArgb(25, 40, 25);
            carousel1.ImageHighlightColor = Color.LimeGreen;
            carousel1.ImageShadeColor = Color.FromArgb(60, 80, 60);
            break;
    }
}
```

## ImageShadeColor Property

Applies a color for shading images at the back of the carousel.

### Property Details

- **Type**: `Color`
- **Default**: System default

### Basic Usage

```csharp
Carousel carousel1 = new Carousel();
carousel1.ImageSlides = true;
carousel1.ImageShadeColor = Color.FromArgb(100, 100, 100); // Gray shade
```

### Shade Color Examples

```csharp
// Dark gray shade
carousel1.ImageShadeColor = Color.FromArgb(80, 80, 80);

// Light gray shade
carousel1.ImageShadeColor = Color.FromArgb(150, 150, 150);

// Blue tint shade
carousel1.ImageShadeColor = Color.FromArgb(80, 80, 120);

// Brown tint shade
carousel1.ImageShadeColor = Color.FromArgb(120, 100, 80);
```

### Coordinated Highlight and Shade

```csharp
Carousel carousel1 = new Carousel();
carousel1.ImageSlides = true;
carousel1.ShowImagePreview = true;
carousel1.ShowImageShadow = true;

// Bright highlight for selected image
carousel1.ImageHighlightColor = Color.FromArgb(255, 215, 0); // Gold

// Darker shade for background images
carousel1.ImageShadeColor = Color.FromArgb(60, 60, 60); // Dark gray

// Dark background for contrast
carousel1.BackColor = Color.FromArgb(30, 30, 30);
```

## Combining Visual Effects

### Premium Gallery Look

```csharp
Carousel premiumGallery = new Carousel();
premiumGallery.Dock = DockStyle.Fill;
premiumGallery.ImageSlides = true;

// Path and perspective
premiumGallery.CarouselPath = CarouselPath.Default;
premiumGallery.Perspective = 5.0f;
premiumGallery.TransitionSpeed = 2.0f;

// Visual effects
premiumGallery.ShowImageShadow = true;
premiumGallery.ShowImagePreview = true;

// Colors
premiumGallery.BackColor = Color.FromArgb(20, 20, 20);
premiumGallery.ImageHighlightColor = Color.FromArgb(255, 215, 0); // Gold
premiumGallery.ImageShadeColor = Color.FromArgb(70, 70, 70); // Gray

// Load images
premiumGallery.FilePath = "Gallery";

this.Controls.Add(premiumGallery);
```

### Modern Product Showcase

```csharp
Carousel productShowcase = new Carousel();
productShowcase.Size = new Size(900, 600);
productShowcase.Location = new Point(50, 50);
productShowcase.ImageSlides = true;

// Configuration
productShowcase.CarouselPath = CarouselPath.Default;
productShowcase.Perspective = 4.5f;
productShowcase.TransitionSpeed = 2.5f;
productShowcase.RotateAlways = false;

// Visual polish
productShowcase.ShowImageShadow = true;
productShowcase.ShowImagePreview = true;
productShowcase.BackColor = Color.White;
productShowcase.ImageHighlightColor = Color.FromArgb(41, 128, 185); // Blue
productShowcase.ImageShadeColor = Color.FromArgb(180, 180, 180); // Light gray

// Load products
foreach (var product in products)
{
    CarouselImage img = new CarouselImage();
    img.ItemImage = product.Image;
    productShowcase.ImageListCollection.Add(img);
}

this.Controls.Add(productShowcase);
```

### Dark Theme Carousel

```csharp
Carousel darkCarousel = new Carousel();
darkCarousel.Dock = DockStyle.Fill;
darkCarousel.ImageSlides = true;

// Dark theme settings
darkCarousel.BackColor = Color.FromArgb(18, 18, 18);
darkCarousel.CarouselPath = CarouselPath.Oval;
darkCarousel.Perspective = 4.0f;
darkCarousel.TransitionSpeed = 1.8f;

// Dark theme effects
darkCarousel.ShowImageShadow = true;
darkCarousel.ShowImagePreview = true;
darkCarousel.ImageHighlightColor = Color.FromArgb(0, 173, 181); // Cyan
darkCarousel.ImageShadeColor = Color.FromArgb(40, 40, 40);

darkCarousel.FilePath = "Photos";
this.Controls.Add(darkCarousel);
```

### Complete Customization Panel

```csharp
public partial class CarouselCustomizer : Form
{
    private Carousel carousel1;
    
    private void InitializeCarouselWithControls()
    {
        // Create carousel
        carousel1 = new Carousel();
        carousel1.ImageSlides = true;
        carousel1.Size = new Size(700, 500);
        carousel1.Location = new Point(20, 20);
        this.Controls.Add(carousel1);
        
        // Perspective control
        trackBarPerspective.Minimum = 10;
        trackBarPerspective.Maximum = 100;
        trackBarPerspective.Value = 40;
        trackBarPerspective.Scroll += (s, e) => {
            carousel1.Perspective = trackBarPerspective.Value / 10.0f;
            lblPerspective.Text = $"{carousel1.Perspective:F1}";
        };
        
        // Speed control
        trackBarSpeed.Minimum = 5;
        trackBarSpeed.Maximum = 100;
        trackBarSpeed.Value = 20;
        trackBarSpeed.Scroll += (s, e) => {
            carousel1.TransitionSpeed = trackBarSpeed.Value / 10.0f;
            lblSpeed.Text = $"{carousel1.TransitionSpeed:F1}x";
        };
        
        // Shadow toggle
        chkShowShadow.CheckedChanged += (s, e) => {
            carousel1.ShowImageShadow = chkShowShadow.Checked;
        };
        
        // Preview toggle
        chkShowPreview.CheckedChanged += (s, e) => {
            carousel1.ShowImagePreview = chkShowPreview.Checked;
        };
        
        // Highlight color picker
        btnHighlightColor.Click += (s, e) => {
            using (ColorDialog dlg = new ColorDialog())
            {
                if (dlg.ShowDialog() == DialogResult.OK)
                {
                    carousel1.ImageHighlightColor = dlg.Color;
                    panelHighlightColor.BackColor = dlg.Color;
                }
            }
        };
        
        // Shade color picker
        btnShadeColor.Click += (s, e) => {
            using (ColorDialog dlg = new ColorDialog())
            {
                if (dlg.ShowDialog() == DialogResult.OK)
                {
                    carousel1.ImageShadeColor = dlg.Color;
                    panelShadeColor.BackColor = dlg.Color;
                }
            }
        };
    }
}
```

## Performance Considerations

### Optimal Settings for Performance

```csharp
// For smooth performance with many images
Carousel carousel1 = new Carousel();
carousel1.ImageSlides = true;
carousel1.Perspective = 4.0f; // Moderate perspective
carousel1.TransitionSpeed = 2.0f; // Not too fast
carousel1.ShowImageShadow = false; // Shadows can impact performance
carousel1.ShowImagePreview = true; // Minimal impact

// Keep image count reasonable
// Recommended: 6-12 images for best performance
```

### Performance vs Visual Quality

```csharp
// High performance (sacrifice some effects)
void ConfigureForPerformance(Carousel carousel)
{
    carousel.ShowImageShadow = false; // Disable shadows
    carousel.Perspective = 3.5f; // Lower perspective
    carousel.TransitionSpeed = 2.5f; // Moderate speed
    carousel.ShowImagePreview = true; // Keep preview
}

// High visual quality (may impact performance)
void ConfigureForQuality(Carousel carousel)
{
    carousel.ShowImageShadow = true; // Enable shadows
    carousel.Perspective = 5.0f; // Higher perspective
    carousel.TransitionSpeed = 1.5f; // Slower, smoother
    carousel.ShowImagePreview = true;
    carousel.ImageHighlightColor = Color.Gold;
    carousel.ImageShadeColor = Color.FromArgb(80, 80, 80);
}
```

### Testing Configuration

```csharp
private void TestConfiguration()
{
    Carousel testCarousel = new Carousel();
    testCarousel.ImageSlides = true;
    testCarousel.Dock = DockStyle.Fill;
    
    // Start with balanced settings
    testCarousel.Perspective = 4.0f;
    testCarousel.TransitionSpeed = 2.0f;
    testCarousel.ShowImageShadow = true;
    testCarousel.ShowImagePreview = true;
    
    // Load test images
    for (int i = 1; i <= 10; i++)
    {
        CarouselImage img = new CarouselImage();
        img.ItemImage = Image.FromFile($"TestImages/img{i}.jpg");
        testCarousel.ImageListCollection.Add(img);
    }
    
    // Monitor performance
    System.Diagnostics.Stopwatch sw = new System.Diagnostics.Stopwatch();
    sw.Start();
    this.Controls.Add(testCarousel);
    sw.Stop();
    
    Debug.WriteLine($"Load time: {sw.ElapsedMilliseconds}ms");
}
```

## Best Practices

1. **Balance Effects**: Don't enable all effects at maximum - balance visual quality with performance

2. **Test with Real Data**: Test visual configuration with actual images and item counts

3. **Adjust by Path**: Different carousel paths may need different perspective values

4. **Consider Use Case**: Slower speeds for photo viewing, faster for navigation

5. **Dark Backgrounds**: Use dark backgrounds with bright highlights for modern look

6. **Shade Coordination**: Coordinate shade color with background for seamless appearance

7. **Performance First**: For large image collections, prioritize performance over effects

8. **User Control**: Consider providing users with customization controls for their preferences

## Next Steps

- **Rotation**: See [rotation-behavior.md](rotation-behavior.md) for auto-rotation with visual effects
- **Touch**: See [touch-interactions.md](touch-interactions.md) for pinch-to-zoom perspective control
- **Images**: See [image-slides.md](image-slides.md) for image-specific configuration
