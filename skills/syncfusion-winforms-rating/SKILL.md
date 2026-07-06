---
name: syncfusion-winforms-rating
description: Implement Syncfusion WinForms Rating control for star ratings and user feedback inputs. Use this when building product reviews, satisfaction surveys, or quality rating UIs in WinForms. Covers rating precision (standard/half), built-in shapes (star, circle, heart, diamond), custom image shapes, orientation and Office-themed gradient styling.
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
---

# Implementing Syncfusion WinForms Rating Control

The **Rating Control** provides an intuitive visual rating experience that allows users to select a number of stars (or other shapes) to represent a rating value. This control is ideal for user feedback, product reviews, quality ratings, and any scenario requiring visual rating input.

## When to Use This Skill

Use this skill when the user needs to:

- **Star Rating Systems:** Product ratings, reviews, user feedback with 1-5 stars
- **Custom Rating Shapes:** Hearts for favorites, diamonds for quality, circles for simplicity
- **Half-Star Ratings:** Fine-grained precision (3.5 stars, 4.5 stars) for detailed feedback
- **Feedback Forms:** Customer satisfaction, service quality, experience ratings
- **Review Interfaces:** Movie ratings, book reviews, app store ratings
- **Rating Display:** Show read-only ratings or interactive rating input
- **Themed Rating Controls:** Office 2007/2010/2016 styles with consistent UI appearance
- **Vertical/Horizontal Layouts:** Adapt rating orientation to form design

## Component Overview

**RatingControl** (`Syncfusion.Windows.Forms.Tools.RatingControl`) is a versatile rating input control supporting:

- **6 Built-in Shapes:** Star, Circle, Triangle, Heart, Diamond, Kite
- **Custom Images:** Use your own image assets for unique rating indicators
- **Precision Modes:** Standard (full shapes) or Half (half-shape granularity)
- **Office Themes:** Office2007, Office2010, Office2016 (Colorful, DarkGray, White, Black), Metro
- **Flexible Orientation:** Horizontal or Vertical layout
- **Tooltip Support:** Display additional information on hover
- **Custom Styling:** Gradient colors, highlight/selection colors, border customization

**Key Namespace:** `Syncfusion.Windows.Forms.Tools`

**Assembly:** `Syncfusion.Tools.Windows.dll` (and dependencies)

---

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

**When to read:** User needs to install, configure, or create their first Rating control.

**Topics covered:**
- Assembly deployment and NuGet package installation
- Adding Rating control via designer (toolbox)
- Adding Rating control via code
- Setting rating values with the Value property
- Built-in shapes quick overview
- Basic implementation examples

### Shapes and Customization
📄 **Read:** [references/shapes-and-customization.md](references/shapes-and-customization.md)

**When to read:** User wants built-in shapes (star, heart, circle) or needs custom image-based rating indicators.

**Topics covered:**
- 6 Built-in shapes: Star, Circle, Triangle, Heart, Diamond, Kite
- Changing shapes with the Shape property
- Custom image shapes using CustomImageCollection
- NormalImage, HoverImage, SelectedImage configuration
- Custom image fallback behavior
- Half-image support for precision modes

### Precision and Values
📄 **Read:** [references/precision-and-values.md](references/precision-and-values.md)

**When to read:** User needs half-star ratings (3.5 stars) or wants to control rating granularity.

**Topics covered:**
- Precision modes: Standard vs Half
- Half-star/half-shape rating configuration
- Value property for setting and retrieving ratings
- Precision with built-in shapes
- Precision with custom images (requires half-images)
- Rating value ranges and increments

### Styling and Appearance
📄 **Read:** [references/styling-and-appearance.md](references/styling-and-appearance.md)

**When to read:** User wants Office themes, custom colors, gradient effects, or visual style customization.

**Topics covered:**
- Visual styles: Default, Office2007, Office2010, Metro
- Office2016 variants: Colorful, DarkGray, White, Black
- OfficeColorScheme property (Blue, Silver, Black)
- Custom gradient coloring with ApplyGradientColors
- ItemHighlight and ItemSelection color properties
- BackColor and BorderColor customization

### Orientation and Tooltips
📄 **Read:** [references/orientation-and-tooltips.md](references/orientation-and-tooltips.md)

**When to read:** User needs vertical rating layout or wants tooltips on rating items.

**Topics covered:**
- Horizontal and Vertical orientation modes
- Orientation property configuration
- Tooltip enablement with ShowTooltip
- Tooltip appearance customization
- Layout considerations for vertical ratings

---

## Quick Start Example

### Basic Star Rating (5 Stars)

```csharp
using Syncfusion.Windows.Forms.Tools;

// Create rating control
RatingControl productRating = new RatingControl();
productRating.Location = new Point(20, 20);
productRating.Size = new Size(150, 30);

// Set initial value (3 out of 5 stars)
productRating.Value = 3;

// Add to form
this.Controls.Add(productRating);
```

### Half-Star Precision Rating

```csharp
// Enable half-star precision
RatingControl preciseRating = new RatingControl();
preciseRating.Precision = PrecisionMode.Half;
preciseRating.Value = 3.5f; // 3.5 stars
```

### Custom Shape (Hearts for Favorites)

```csharp
// Use heart shapes instead of stars
RatingControl favoriteRating = new RatingControl();
favoriteRating.Shape = Shapes.Heart;
favoriteRating.Value = 4;
```

### Office 2016 Themed Rating

```csharp
// Apply Office 2016 Colorful theme
RatingControl themedRating = new RatingControl();
themedRating.VisualStyle  = RatingControl.Style.Office2016Colorful;
themedRating.Value = 5;
```

---

## Common Patterns

### Product Review Rating (Read-Only Display)

```csharp
// Display average rating (read-only)
RatingControl avgRating = new RatingControl();
avgRating.Value = 4.5f;
avgRating.Precision = PrecisionMode.Half;
avgRating.Enabled = false; // Read-only
```

**When to use:** Displaying aggregated ratings on product listings.

### Interactive Feedback Form

```csharp
// User can select rating
RatingControl feedbackRating = new RatingControl();
feedbackRating.Value = 0; // No initial selection
feedbackRating.Precision = PrecisionMode.Half;

// Handle rating changes
feedbackRating.RatingChanged  += (s, e) => {
    float rating = feedbackRating.Value;
    MessageBox.Show($"User rated: {rating} stars");
};
```

**When to use:** Customer feedback forms, surveys, review submission.

### Vertical Rating Layout

```csharp
// Vertical orientation for sidebar layouts
RatingControl verticalRating = new RatingControl();
verticalRating.Orientation = Orientationmode.Vertical;
verticalRating.Size = new Size(50, 200);
verticalRating.Value = 3;
```

**When to use:** Sidebar designs, narrow column layouts.

### Custom Color Gradient Rating

```csharp
// Custom gradient colors (green to yellow)
RatingControl customRating = new RatingControl();
customRating.ApplyGradientColors = true;
customRating.ItemHighlightStartColor = Color.Green;
customRating.ItemHighlightEndColor = Color.YellowGreen;
customRating.ItemSelectionStartColor = Color.Gold;
customRating.ItemSelectionEndColor = Color.Orange;
customRating.Value = 4;
```

**When to use:** Branded color schemes, matching application themes.

### Custom Image Rating

```csharp
// Use custom images for rating
RatingControl customImageRating = new RatingControl();
customImageRating.Shape = Shapes.CustomImages;

CustomImageCollection images = new CustomImageCollection();
images.NormalImage = Properties.Resources.rating_normal;
images.HoverImage = Properties.Resources.rating_hover;
images.SelectedImage = Properties.Resources.rating_selected;

customImageRating.Images = images;
customImageRating.Value = 3;
```

**When to use:** Unique branding, custom icons, non-standard rating indicators.

---

## Key Properties

### Core Properties

| Property | Type | Description | When to Use |
|----------|------|-------------|-------------|
| **Value** | `float` | Current rating value (0 to number of items) | Set/get rating selection |
| **Precision** | `PrecisionMode` | Standard or Half precision | Enable half-star ratings |
| **Shape** | `Shapes` | Star, Circle, Triangle, Heart, Diamond, Kite, CustomImages | Change rating indicator shape |
| **Orientation** | `Orientationmode` | Horizontal or Vertical | Vertical layout for sidebars |
| **ShowTooltip** | `bool` | Enable tooltips on rating items | Show rating values on hover |

### Styling Properties

| Property | Type | Description | When to Use |
|----------|------|-------------|-------------|
| **VisualStyle** | `RatingControl.Style` | Visual style (Office2007/2010/2016, Metro) | Apply Office themes |
| **OfficeColorScheme** | `OfficeColorSchemes` | Blue, Silver, Black color scheme | Office theme color variants |
| **ApplyGradientColors** | `bool` | Enable gradient color properties | Custom gradient effects |
| **ItemHighlightColor** | `Color` | Hover state color | Custom hover appearance |
| **ItemSelectionColor** | `Color` | Selected state color | Custom selection appearance |

### Custom Image Properties

| Property | Type | Description | When to Use |
|----------|------|-------------|-------------|
| **Images** | `CustomImageCollection` | Custom image collection | Use custom rating images |
| **Images.NormalImage** | `Image` | Unselected state image | Custom default appearance |
| **Images.HoverImage** | `Image` | Hover state image | Custom hover appearance |
| **Images.SelectedImage** | `Image` | Selected state image | Custom selected appearance |

---

## Common Use Cases

### 1. Product Rating Display
**Scenario:** E-commerce product listing with average ratings.
- Use `Value` to display average (e.g., 4.3 stars)
- Set `Precision = PrecisionMode.Half` for accurate display
- Set `Enabled = false` for read-only
- Read: [precision-and-values.md](references/precision-and-values.md)

### 2. Customer Feedback Form
**Scenario:** Service quality rating in feedback form.
- Use `Value = 0` for no initial selection
- Handle `ValueChanged` event to capture user input
- Apply Office2016 theme for professional appearance
- Read: [getting-started.md](references/getting-started.md), [styling-and-appearance.md](references/styling-and-appearance.md)

### 3. Multi-Criteria Rating
**Scenario:** Rate multiple aspects (Quality, Service, Value).
- Create multiple RatingControl instances
- Use descriptive labels for each rating
- Consistent shape and style across all ratings
- Read: [shapes-and-customization.md](references/shapes-and-customization.md)

### 4. Favorite/Like System
**Scenario:** Heart-based favorites (1-5 hearts).
- Set `Shape = Shapes.Heart`
- Use custom colors for romantic/favorite themes
- Read: [shapes-and-customization.md](references/shapes-and-customization.md)

### 5. Branded Rating UI
**Scenario:** Custom images matching brand identity.
- Use `Shape = Shapes.CustomImages`
- Provide NormalImage, HoverImage, SelectedImage
- Include HalfImages for precision support
- Read: [shapes-and-customization.md](references/shapes-and-customization.md)

### 6. Vertical Rating Sidebar
**Scenario:** Sidebar with vertical rating display.
- Set `Orientation = Orientationmode.Vertical`
- Adjust Size for vertical layout
- Use tooltips for value display
- Read: [orientation-and-tooltips.md](references/orientation-and-tooltips.md)

---

## Events

**RatingChanged:** Raised when the rating value changes (user interaction).

```csharp
ratingControl1.RatingChanged  += (sender, e) => {
    float newRating = ratingControl1.Value;
    // Process rating change
};
```

---

## Best Practices

1. **Precision for Averages:** Use `PrecisionMode.Half` when displaying aggregated ratings (4.5 stars)
2. **Read-Only Display:** Set `Enabled = false` for non-interactive rating displays
3. **Consistent Shapes:** Use the same shape across related rating controls
4. **Tooltips for Clarity:** Enable `ShowTooltip` to show numeric values
5. **Custom Images:** Provide all image states (Normal, Hover, Selected, HalfNormal, HalfSelected) for complete custom experience
6. **Theme Consistency:** Match rating style to application theme (Office2016 variants)
7. **Value Initialization:** Set `Value = 0` for new ratings, specific value for defaults

---

## See Also

- [Syncfusion Rating Control Documentation](https://help.syncfusion.com/windowsforms/rating/overview)
- [RatingControl API Reference](https://help.syncfusion.com/cr/windowsforms/Syncfusion.Windows.Forms.Tools.RatingControl.html)
