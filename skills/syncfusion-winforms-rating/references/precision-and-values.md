# Precision and Values - WinForms Rating Control

This reference covers precision modes for rating granularity, value handling, and the relationship between precision settings and shape configuration.

## Precision Modes Overview

The Rating control supports two precision modes that determine how granular rating values can be:

- **Standard Precision**: Full-shape increments only (1, 2, 3, 4, 5)
- **Half Precision**: Half-shape increments allowed (1, 1.5, 2, 2.5, 3, 3.5, etc.)

## PrecisionMode Enum

The `Precision` property uses the `PrecisionMode` enumeration:

```csharp
using Syncfusion.Windows.Forms.Tools;

// Standard precision (whole numbers only)
ratingControl1.Precision = PrecisionMode.Standard;

// Half precision (allows .5 increments)
ratingControl1.Precision = PrecisionMode.Half;
```

### Default Value

The default precision mode is `PrecisionMode.Standard`.

## Standard Precision Mode

Standard precision allows only whole-number ratings. Each click selects or deselects a complete rating item.

### Use Cases for Standard Precision

- Simple yes/no quality indicators
- Count-based ratings (number of completed tasks)
- Quick surveys where granularity isn't critical
- Touch-based interfaces where precision is difficult
- Child-friendly applications

### Standard Precision Example

```csharp
private void SetupStandardPrecision()
{
    var standardRating = new RatingControl
    {
        Location = new Point(50, 50),
        Size = new Size(200, 40),
        Precision = PrecisionMode.Standard,
        Shape = Shapes.Star
    };

    // Valid values: 0, 1, 2, 3, 4, 5
    standardRating.Value = 4; // ✓ Valid
    
    // Invalid values are rounded
    standardRating.Value = 3.7f; // Rounded to 4
    standardRating.Value = 2.3f; // Rounded to 2

    this.Controls.Add(standardRating);
}
```

### Rounding Behavior in Standard Mode

When setting a fractional value in Standard precision:
- Values are rounded to the nearest whole number
- 0.5 rounds up (e.g., 2.5 → 3)
- 0.4 and below rounds down (e.g., 2.4 → 2)

```csharp
ratingControl1.Precision = PrecisionMode.Standard;

ratingControl1.Value = 3.5f; // Displays as 4
ratingControl1.Value = 3.4f; // Displays as 3
ratingControl1.Value = 3.6f; // Displays as 4
```

## Half Precision Mode

Half precision enables half-step ratings, allowing more nuanced feedback. Users can click on the left or right half of a rating item.

### Use Cases for Half Precision

- Detailed product reviews (e.g., 4.5-star ratings)
- Professional evaluations requiring granularity
- Movie/book ratings matching common systems (e.g., IMDb)
- Performance metrics with fine-grained scoring
- Data visualization showing fractional progress

### Half Precision Example

```csharp
private void SetupHalfPrecision()
{
    var halfRating = new RatingControl
    {
        Location = new Point(50, 50),
        Size = new Size(200, 40),
        Precision = PrecisionMode.Half,
        Shape = Shapes.Star
    };

    // Valid values: 0, 0.5, 1, 1.5, 2, 2.5, 3, 3.5, 4, 4.5, 5
    halfRating.Value = 3.5f; // ✓ Valid - displays 3.5 stars
    halfRating.Value = 4.5f; // ✓ Valid - displays 4.5 stars

    this.Controls.Add(halfRating);
}
```

### Click Behavior in Half Precision

With half precision enabled:
- Clicking the **left half** of an item selects a half-step (e.g., 2.5)
- Clicking the **right half** of an item selects a full step (e.g., 3)
- The visual feedback shows the appropriate half or full shape

```csharp
// User interaction example
// Click on left half of 3rd star → Value becomes 2.5
// Click on right half of 3rd star → Value becomes 3.0
```

## Value Property

The `Value` property represents the current rating as a float value.

### Value Range

- **Minimum**: 0 (no rating)
- **Maximum**: Number of items (default is 5)
- **Type**: float

### Setting Values Programmatically

```csharp
// Set integer values
ratingControl1.Value = 0; // No rating
ratingControl1.Value = 3; // 3 stars
ratingControl1.Value = 5; // Maximum rating

// Set fractional values (requires Half precision)
ratingControl1.Precision = PrecisionMode.Half;
ratingControl1.Value = 3.5f; // 3.5 stars
ratingControl1.Value = 4.5f; // 4.5 stars
```

### Reading Values

```csharp
float currentRating = ratingControl1.Value;

// Display to user
lblRating.Text = $"Rating: {currentRating:F1} stars";

// Check for specific values
if (currentRating >= 4.0f)
{
    lblFeedback.Text = "Excellent!";
}
else if (currentRating >= 3.0f)
{
    lblFeedback.Text = "Good";
}
else if (currentRating >= 2.0f)
{
    lblFeedback.Text = "Average";
}
else
{
    lblFeedback.Text = "Needs Improvement";
}
```

### Value Changed Event

Track rating changes with the `RatingChanged` event:

```csharp
private void InitializeRatingWithEvent()
{
    ratingControl1.Precision = PrecisionMode.Half;
    ratingControl1.RatingChanged += RatingControl1_RatingChanged;
}

private void RatingControl1_RatingChanged(object sender, EventArgs e)
{
    float newValue = ratingControl1.Value;
    Console.WriteLine($"Rating changed to: {newValue}");
    
    // Update database, UI, or perform validation
    SaveRatingToDatabase(newValue);
}
```

## Precision with Built-in Shapes

All built-in shapes support both Standard and Half precision modes without additional configuration.

### Half-Star Rating Example

```csharp
private void CreateHalfStarRating()
{
    var starRating = new RatingControl
    {
        Location = new Point(100, 50),
        Size = new Size(220, 45),
        Shape = Shapes.Star,
        Precision = PrecisionMode.Half,
        Value = 4.5f,
        ShowTooltip = true
    };

    this.Controls.Add(starRating);
}
```

### Half-Heart Rating Example

```csharp
private void CreateHalfHeartRating()
{
    var heartRating = new RatingControl
    {
        Location = new Point(100, 120),
        Size = new Size(220, 45),
        Shape = Shapes.Heart,
        Precision = PrecisionMode.Half,
        Value = 3.5f,
        ShowTooltip = true
    };

    this.Controls.Add(heartRating);
}
```

## Precision with Custom Images

Custom images require additional setup to support Half precision mode.

### Requirements for Half Precision with Custom Images

To use Half precision with custom images, you MUST provide:

1. **NormalImage** - Unselected state image
2. **SelectedImage** - Selected state image
3. **HalfNormalImage** - Half-selected, half-normal image
4. **HalfSelectedImage** - Half-selected, half-normal image (alternate)

### Custom Image Half-Precision Example

```csharp
private void SetupCustomImageHalfPrecision()
{
    ratingControl1.Shape = Shapes.CustomImages;
    
    // Initialize custom image collection
    ratingControl1.CustomImages = new CustomImageCollection
    {
        // Required for any custom image usage
        NormalImage = Properties.Resources.IconNormal,
        SelectedImage = Properties.Resources.IconSelected,
        
        // Required for Half precision
        HalfNormalImage = Properties.Resources.IconHalfNormal,
        HalfSelectedImage = Properties.Resources.IconHalfSelected
    };

    // Now Half precision will work
    ratingControl1.Precision = PrecisionMode.Half;
    ratingControl1.Value = 2.5f;
}
```

### Edge Case: Custom Images Without Half-Images

**Critical Rule:** If you set `Precision = Half` with custom images but don't provide half-images, the precision is automatically forced to Standard.

```csharp
// This configuration will NOT support Half precision
ratingControl1.Shape = Shapes.CustomImages;
ratingControl1.CustomImages = new CustomImageCollection
{
    NormalImage = Properties.Resources.Icon,
    SelectedImage = Properties.Resources.IconSelected
    // Missing: HalfNormalImage and HalfSelectedImage
};

// This gets automatically changed to PrecisionMode.Standard
ratingControl1.Precision = PrecisionMode.Half; 

// Value gets rounded
ratingControl1.Value = 3.5f; // Actually displays as 4
```

**Solution:** Always provide half-images when using Half precision with custom images.

```csharp
private bool CanUseHalfPrecision(RatingControl rating)
{
    if (rating.Shape != Shapes.CustomImages)
        return true; // Built-in shapes always support Half precision

    // Check if custom images have half-images
    return rating.CustomImages != null &&
           rating.CustomImages.HalfNormalImage != null &&
           rating.CustomImages.HalfSelectedImage != null;
}
```

## When to Use Standard vs. Half Precision

### Choose Standard Precision When:

- Users need quick, simple rating input
- Interface is touch-based or mobile
- Rating granularity isn't important
- Target audience includes children
- Screen space is limited

### Choose Half Precision When:

- Detailed feedback is required
- Matching industry standards (e.g., 4.5-star reviews)
- Professional or technical evaluations
- Displaying aggregate ratings from multiple sources
- Data accuracy is critical

## Complete Implementation Example

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace RatingPrecisionExample
{
    public partial class RatingForm : Form
    {
        private RatingControl standardRating;
        private RatingControl halfRating;
        private Label lblStandardValue;
        private Label lblHalfValue;

        public RatingForm()
        {
            InitializeComponent();
            SetupRatingControls();
        }

        private void SetupRatingControls()
        {
            // Standard Precision Rating
            Label lblStandard = new Label
            {
                Text = "Standard Precision (whole numbers):",
                Location = new Point(20, 20),
                AutoSize = true
            };

            standardRating = new RatingControl
            {
                Location = new Point(20, 45),
                Size = new Size(200, 40),
                Precision = PrecisionMode.Standard,
                Shape = Shapes.Star,
                Value = 3
            };
            standardRating.RatingChanged += StandardRating_Changed;

            lblStandardValue = new Label
            {
                Text = "Value: 3.0",
                Location = new Point(230, 50),
                AutoSize = true
            };

            // Half Precision Rating
            Label lblHalf = new Label
            {
                Text = "Half Precision (allows 0.5 increments):",
                Location = new Point(20, 100),
                AutoSize = true
            };

            halfRating = new RatingControl
            {
                Location = new Point(20, 125),
                Size = new Size(200, 40),
                Precision = PrecisionMode.Half,
                Shape = Shapes.Heart,
                Value = 3.5f
            };
            halfRating.RatingChanged += HalfRating_Changed;

            lblHalfValue = new Label
            {
                Text = "Value: 3.5",
                Location = new Point(230, 130),
                AutoSize = true
            };

            // Add controls
            this.Controls.AddRange(new Control[] {
                lblStandard, standardRating, lblStandardValue,
                lblHalf, halfRating, lblHalfValue
            });
        }

        private void StandardRating_Changed(object sender, EventArgs e)
        {
            lblStandardValue.Text = $"Value: {standardRating.Value:F1}";
        }

        private void HalfRating_Changed(object sender, EventArgs e)
        {
            lblHalfValue.Text = $"Value: {halfRating.Value:F1}";
        }
    }
}
```

## Troubleshooting

### Issue: Half values are being rounded

**Cause:** Precision is set to Standard, or custom images lack half-images.

**Solution:**
```csharp
// Verify precision setting
if (ratingControl1.Precision != PrecisionMode.Half)
{
    ratingControl1.Precision = PrecisionMode.Half;
}

// If using custom images, verify half-images exist
if (ratingControl1.Shape == Shapes.CustomImages)
{
    if (ratingControl1.CustomImages.HalfNormalImage == null ||
        ratingControl1.CustomImages.HalfSelectedImage == null)
    {
        // Load half-images or switch to built-in shapes
    }
}
```

### Issue: Half-click detection not working

**Cause:** Control size is too small for precise click detection.

**Solution:** Ensure adequate control size (minimum 150x30 recommended).

```csharp
// Increase control size for better half-click detection
ratingControl1.Size = new Size(250, 50);
```
