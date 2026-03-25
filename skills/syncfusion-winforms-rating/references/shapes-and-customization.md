# Shapes and Customization - WinForms Rating Control

This reference covers all shape options available for the Rating control, including built-in shapes and custom image implementation with comprehensive edge case handling.

## Table of Contents

- [Built-in Shapes Overview](#built-in-shapes-overview)
- [Using Built-in Shapes](#using-built-in-shapes)
- [Custom Image Shapes](#custom-image-shapes)
- [CustomImageCollection Setup](#customimagecollection-setup)
- [Half-Image Support for Precision](#half-image-support-for-precision)
- [Custom Image Fallback Behavior](#custom-image-fallback-behavior)
- [Edge Cases and Troubleshooting](#edge-cases-and-troubleshooting)
- [Complete Examples](#complete-examples)

## Built-in Shapes Overview

The Rating control provides 6 predefined shapes accessible through the `Shape` property using the `Shapes` enum.

### Available Shapes

1. **Star** - Classic 5-pointed star (Default shape)
2. **Circle** - Circular/round shape
3. **Triangle** - Triangular shape pointing upward
4. **Heart** - Heart symbol for favorites/love ratings
5. **Diamond** - Diamond/rhombus shape
6. **Kite** - Kite-shaped symbol

### When to Use Each Shape

- **Star**: Universal rating symbol (product reviews, skills assessment)
- **Circle**: Minimalist UI, modern applications, progress indicators
- **Triangle**: Priority levels, hierarchy ratings
- **Heart**: Favorites, preferences, emotional ratings, wish lists
- **Diamond**: Premium/luxury ratings, achievement levels
- **Kite**: Playful interfaces, gamification, children's applications

## Using Built-in Shapes

### Shape Property

The `Shape` property uses the `Syncfusion.Windows.Forms.Tools.Shapes` enum:

```csharp
using Syncfusion.Windows.Forms.Tools;

// Set to Star (default)
ratingControl1.Shape = Shapes.Star;

// Set to Circle
ratingControl1.Shape = Shapes.Circle;

// Set to Triangle
ratingControl1.Shape = Shapes.Triangle;

// Set to Heart
ratingControl1.Shape = Shapes.Heart;

// Set to Diamond
ratingControl1.Shape = Shapes.Diamond;

// Set to Kite
ratingControl1.Shape = Shapes.Kite;
```

### Example: Multiple Rating Controls with Different Shapes

```csharp
private void InitializeShapeExamples()
{
    // Product quality - Stars
    var qualityRating = new RatingControl
    {
        Location = new Point(20, 20),
        Size = new Size(200, 40),
        Shape = Shapes.Star,
        Value = 4
    };

    // Favorite items - Hearts
    var favoriteRating = new RatingControl
    {
        Location = new Point(20, 80),
        Size = new Size(200, 40),
        Shape = Shapes.Heart,
        Value = 5
    };

    // Priority level - Triangles
    var priorityRating = new RatingControl
    {
        Location = new Point(20, 140),
        Size = new Size(200, 40),
        Shape = Shapes.Triangle,
        Value = 3
    };

    this.Controls.Add(qualityRating);
    this.Controls.Add(favoriteRating);
    this.Controls.Add(priorityRating);
}
```

## Custom Image Shapes

For unique branding or specific design requirements, use custom images instead of built-in shapes.

### Enabling Custom Images

Set the `Shape` property to `Shapes.CustomImages`:

```csharp
ratingControl1.Shape = Shapes.CustomImages;
```

### Required Image States

Custom images support three visual states:

1. **NormalImage** - Default unselected state
2. **HoverImage** - Mouse hover state (optional)
3. **SelectedImage** - Selected/rated state

## CustomImageCollection Setup

### Creating a CustomImageCollection

```csharp
using System.Drawing;
using Syncfusion.Windows.Forms.Tools;

// Initialize the CustomImageCollection
ratingControl1.CustomImages = new CustomImageCollection();

// Load images from resources or files
ratingControl1.CustomImages.NormalImage = Properties.Resources.StarNormal;
ratingControl1.CustomImages.HoverImage = Properties.Resources.StarHover;
ratingControl1.CustomImages.SelectedImage = Properties.Resources.StarSelected;
```

### Complete Custom Image Example

```csharp
private void SetupCustomImageRating()
{
    var customRating = new RatingControl
    {
        Location = new Point(50, 50),
        Size = new Size(250, 50),
        Shape = Shapes.CustomImages
    };

    // Create and configure custom image collection
    customRating.CustomImages = new CustomImageCollection();
    
    // Load images from embedded resources
    customRating.CustomImages.NormalImage = LoadImage("coin_normal.png");
    customRating.CustomImages.HoverImage = LoadImage("coin_hover.png");
    customRating.CustomImages.SelectedImage = LoadImage("coin_selected.png");
    
    customRating.Value = 3;
    
    this.Controls.Add(customRating);
}

private Image LoadImage(string filename)
{
    string path = Path.Combine(Application.StartupPath, "Images", filename);
    return Image.FromFile(path);
}
```

## Half-Image Support for Precision

To enable half-star (or half-shape) ratings with custom images, provide half-state images.

### Half-Image Properties

- **HalfNormalImage** - Displays the left half as unselected, right half as normal
- **HalfSelectedImage** - Displays the left half as selected, right half as normal

### Setting Up Half-Images

```csharp
private void SetupPrecisionCustomImages()
{
    ratingControl1.Shape = Shapes.CustomImages;
    ratingControl1.CustomImages = new CustomImageCollection();
    
    // Required images for custom shapes
    ratingControl1.CustomImages.NormalImage = Properties.Resources.IconNormal;
    ratingControl1.CustomImages.SelectedImage = Properties.Resources.IconSelected;
    
    // Half-images required for Half precision mode
    ratingControl1.CustomImages.HalfNormalImage = Properties.Resources.IconHalfNormal;
    ratingControl1.CustomImages.HalfSelectedImage = Properties.Resources.IconHalfSelected;
    
    // Enable Half precision
    ratingControl1.Precision = PrecisionMode.Half;
    ratingControl1.Value = 3.5f;
}
```

### Half-Image Design Guidelines

When creating half-images:
- **HalfNormalImage**: Left 50% shows unselected state, right 50% shows normal state
- **HalfSelectedImage**: Left 50% shows selected state, right 50% shows normal state
- Ensure smooth visual transition between halves
- Match dimensions with full images exactly
- Test at various DPI settings for clarity

## Custom Image Fallback Behavior

Understanding fallback behavior prevents unexpected visual results.

### Fallback Rule 1: Null Image Reverts to Built-in Shape

**Scenario:** `Shape` is set to `CustomImages` but image properties are null.

```csharp
// This will automatically revert to Star shape
ratingControl1.Shape = Shapes.CustomImages;
ratingControl1.CustomImages = null; // Shape reverts to Shapes.Star
```

**Fallback:** Control automatically resets to `Shapes.Star` (default built-in shape).

### Fallback Rule 2: Missing HoverImage Uses Color Highlight

**Scenario:** `HoverImage` is not provided.

```csharp
ratingControl1.Shape = Shapes.CustomImages;
ratingControl1.CustomImages = new CustomImageCollection
{
    NormalImage = Properties.Resources.Icon,
    SelectedImage = Properties.Resources.IconSelected
    // HoverImage is null
};
ratingControl1.ItemHighlightColor = Color.LightBlue;
```

**Fallback:** The `NormalImage` is displayed with `ItemHighlightColor` overlay on hover.

### Fallback Rule 3: Missing SelectedImage Uses Color Selection

**Scenario:** `SelectedImage` is not provided.

```csharp
ratingControl1.CustomImages = new CustomImageCollection
{
    NormalImage = Properties.Resources.Icon,
    HoverImage = Properties.Resources.IconHover
    // SelectedImage is null
};
ratingControl1.ItemSelectionColor = Color.Gold;
```

**Fallback:** The `NormalImage` is displayed with `ItemSelectionColor` overlay for selected items.

### Fallback Rule 4: Missing Half-Images Forces Standard Precision

**Scenario:** `Precision` is set to `Half` but half-images are not provided.

```csharp
ratingControl1.Shape = Shapes.CustomImages;
ratingControl1.CustomImages = new CustomImageCollection
{
    NormalImage = Properties.Resources.Icon,
    SelectedImage = Properties.Resources.IconSelected
    // HalfNormalImage and HalfSelectedImage are null
};
ratingControl1.Precision = PrecisionMode.Half; // Automatically changed to Standard
ratingControl1.Value = 3.5f; // Rounded to 4
```

**Fallback:** `Precision` is automatically set to `PrecisionMode.Standard`, and values are rounded to whole numbers.

## Edge Cases and Troubleshooting

### Edge Case 1: Shape Resets When Setting CustomImages

**Problem:** When you set `Shape = Shapes.CustomImages` but later set images to null, shape reverts.

```csharp
// Initially set to custom images
ratingControl1.Shape = Shapes.CustomImages;
ratingControl1.CustomImages = new CustomImageCollection { /* images */ };

// Later, setting CustomImages to null
ratingControl1.CustomImages = null; // Shape automatically becomes Shapes.Star
```

**Solution:** Always check if custom images are loaded before setting `Shape = CustomImages`.

```csharp
if (ratingControl1.CustomImages != null && 
    ratingControl1.CustomImages.NormalImage != null)
{
    ratingControl1.Shape = Shapes.CustomImages;
}
else
{
    ratingControl1.Shape = Shapes.Star; // Explicit fallback
}
```

### Edge Case 2: Image Size Mismatch

**Problem:** Custom images with different dimensions cause irregular spacing.

**Solution:** Ensure all custom images (Normal, Hover, Selected, HalfNormal, HalfSelected) have identical dimensions.

```csharp
private bool ValidateImageDimensions(CustomImageCollection images)
{
    if (images.NormalImage == null) return false;
    
    Size expectedSize = images.NormalImage.Size;
    
    if (images.HoverImage != null && images.HoverImage.Size != expectedSize)
        return false;
    if (images.SelectedImage != null && images.SelectedImage.Size != expectedSize)
        return false;
    if (images.HalfNormalImage != null && images.HalfNormalImage.Size != expectedSize)
        return false;
    if (images.HalfSelectedImage != null && images.HalfSelectedImage.Size != expectedSize)
        return false;
    
    return true;
}
```

### Edge Case 3: Half-Precision Without Half-Images

**Problem:** Setting `Precision = Half` without providing half-images causes unexpected rounding.

```csharp
// This configuration won't work as expected
ratingControl1.Shape = Shapes.CustomImages;
ratingControl1.CustomImages = new CustomImageCollection
{
    NormalImage = img1,
    SelectedImage = img2
    // Missing: HalfNormalImage, HalfSelectedImage
};
ratingControl1.Precision = PrecisionMode.Half; // Forced to Standard
```

**Solution:** Always provide half-images when using Half precision with custom images.

```csharp
// Correct configuration
ratingControl1.CustomImages = new CustomImageCollection
{
    NormalImage = img1,
    SelectedImage = img2,
    HalfNormalImage = imgHalf1,
    HalfSelectedImage = imgHalf2
};
ratingControl1.Precision = PrecisionMode.Half; // Works correctly
```

## Complete Examples

### Example 1: Brand-Specific Custom Rating

```csharp
private void CreateBrandedRating()
{
    var brandRating = new RatingControl
    {
        Location = new Point(100, 100),
        Size = new Size(300, 60),
        Shape = Shapes.CustomImages
    };

    // Load brand-specific images
    brandRating.CustomImages = new CustomImageCollection
    {
        NormalImage = Image.FromFile(@"assets\logo_gray.png"),
        HoverImage = Image.FromFile(@"assets\logo_blue.png"),
        SelectedImage = Image.FromFile(@"assets\logo_gold.png"),
        HalfNormalImage = Image.FromFile(@"assets\logo_half_gray.png"),
        HalfSelectedImage = Image.FromFile(@"assets\logo_half_gold.png")
    };

    brandRating.Precision = PrecisionMode.Half;
    brandRating.Value = 4.5f;
    brandRating.ShowTooltip = true;

    this.Controls.Add(brandRating);
}
```

### Example 2: Dynamic Shape Switching

```csharp
private void SwitchShape(Shapes newShape)
{
    if (newShape == Shapes.CustomImages)
    {
        // Ensure custom images are loaded
        if (ratingControl1.CustomImages == null || 
            ratingControl1.CustomImages.NormalImage == null)
        {
            MessageBox.Show("Custom images not loaded. Using Star shape.");
            ratingControl1.Shape = Shapes.Star;
            return;
        }
    }
    
    ratingControl1.Shape = newShape;
}

// Usage
private void btnStar_Click(object sender, EventArgs e)
{
    SwitchShape(Shapes.Star);
}

private void btnHeart_Click(object sender, EventArgs e)
{
    SwitchShape(Shapes.Heart);
}

private void btnCustom_Click(object sender, EventArgs e)
{
    SwitchShape(Shapes.CustomImages);
}
```
