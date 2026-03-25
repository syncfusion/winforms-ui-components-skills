# Styling and Appearance Customization

## Table of Contents
- [Appearance Overview](#appearance-overview)
- [Drill Region Customization](#drill-region-customization)
- [Outer Rim Styling](#outer-rim-styling)
- [Arc Gap Configuration](#arc-gap-configuration)
- [Display Style Options](#display-style-options)
- [Image Size Customization](#image-size-customization)
- [Center Icon Configuration](#center-icon-configuration)
- [Complete Styling Examples](#complete-styling-examples)
- [Visual Customization Patterns](#visual-customization-patterns)
- [Best Practices](#best-practices)

## Appearance Overview

RadialMenu provides extensive customization options for visual appearance, allowing you to create menus that match your application's design language. You can customize colors, sizes, spacing, and display modes to create the perfect user experience.

**Customizable Elements:**
- **Drill Region** - The clickable area for navigating submenus
- **Outer Rim** - The static border ring around the menu
- **Arc Gap** - Space between rim and hover highlighting
- **Display Style** - How text and images are arranged
- **Image Sizes** - Icon dimensions for menu items
- **Center Icon** - The icon displayed at menu center

**When to Customize Appearance:**
- Brand alignment with corporate colors
- High-contrast accessibility requirements
- Touch vs. mouse interface optimization
- Dark mode vs. light mode theming
- Icon-focused vs. text-focused navigation

## Drill Region Customization

The drill region is the circular area that appears when an item has submenus. Users click this area to navigate into nested menu levels. You can customize both the default state and hover state colors.

### Default State Color

The `OuterArcColor` property sets the color of the drill region in its normal (non-hover) state.

```csharp
// Set drill region color
this.radialMenu1.OuterArcColor = Color.Black;
```

**Common Use Cases:**

```csharp
// Dark theme drill region
this.radialMenu1.OuterArcColor = Color.FromArgb(45, 45, 48);

// Light theme drill region  
this.radialMenu1.OuterArcColor = Color.FromArgb(240, 240, 240);

// Accent color drill region
this.radialMenu1.OuterArcColor = Color.FromArgb(0, 120, 215);  // Windows blue

// High contrast drill region
this.radialMenu1.OuterArcColor = Color.Black;
```

**Result:**
The drill region matches your color scheme, providing visual consistency throughout the menu.

### Hover State Color

The `OuterArcHighLightedColor` property sets the color when users hover over the drill region.

```csharp
// Set hover color for drill region
this.radialMenu1.OuterArcHighLightedColor = Color.FromArgb(64, 64, 64);
```

**Creating Effective Hover States:**

```csharp
private void ConfigureDrillRegionWithHover()
{
    // Dark gray normal state
    this.radialMenu1.OuterArcColor = Color.FromArgb(30, 30, 30);
    
    // Lighter gray on hover (provides clear feedback)
    this.radialMenu1.OuterArcHighLightedColor = Color.FromArgb(80, 80, 80);
}
```

**Color Relationship Best Practices:**

```csharp
// Pattern 1: Lighter hover (for dark themes)
this.radialMenu1.OuterArcColor = Color.Black;
this.radialMenu1.OuterArcHighLightedColor = Color.FromArgb(60, 60, 60);

// Pattern 2: Darker hover (for light themes)
this.radialMenu1.OuterArcColor = Color.FromArgb(220, 220, 220);
this.radialMenu1.OuterArcHighLightedColor = Color.FromArgb(180, 180, 180);

// Pattern 3: Accent color hover
this.radialMenu1.OuterArcColor = Color.Gray;
this.radialMenu1.OuterArcHighLightedColor = Color.DodgerBlue;
```

**Result:**
Users receive clear visual feedback when hovering over the drill region, improving usability.

### Complete Drill Region Example

```csharp
private void CustomizeDrillRegion()
{
    // Configure for modern dark theme
    this.radialMenu1.OuterArcColor = Color.FromArgb(37, 37, 38);  // VS Code dark
    this.radialMenu1.OuterArcHighLightedColor = Color.FromArgb(62, 62, 66);  // Hover shade
    
    // OR configure for light theme
    // this.radialMenu1.OuterArcColor = Color.FromArgb(245, 245, 245);
    // this.radialMenu1.OuterArcHighLightedColor = Color.FromArgb(200, 200, 200);
    
    // OR configure for accent theme
    // this.radialMenu1.OuterArcColor = Color.FromArgb(0, 99, 177);  // Deep blue
    // this.radialMenu1.OuterArcHighLightedColor = Color.FromArgb(0, 150, 255);  // Bright blue
}
```

**Result:**
A professionally styled drill region that provides clear visual feedback and matches your theme.

## Outer Rim Styling

The outer rim is the static border ring that frames the entire RadialMenu. It provides visual definition and can be customized for color and thickness.

### Rim Background Color

The `RimBackground` property sets the color of the outer rim.

```csharp
// Set outer rim color
this.radialMenu1.RimBackground = Color.Blue;
```

**Common Rim Color Patterns:**

```csharp
// Subtle neutral rim
this.radialMenu1.RimBackground = Color.FromArgb(200, 200, 200);

// Bold accent rim
this.radialMenu1.RimBackground = Color.FromArgb(0, 120, 215);  // Windows blue

// Dark theme rim
this.radialMenu1.RimBackground = Color.FromArgb(50, 50, 50);

// Gradient effect (requires custom rendering)
this.radialMenu1.RimBackground = Color.Navy;

// Transparent rim (blends with background)
this.radialMenu1.RimBackground = Color.Transparent;
```

**Result:**
The rim color creates a visual frame that can match your brand or provide contrast.

### Rim Thickness

The `OuterRimThickness` property controls the width of the rim in pixels.

```csharp
// Set rim thickness
this.radialMenu1.OuterRimThickness = 20;
```

**Thickness Guidelines:**

```csharp
// Thin rim (minimalist, more menu space)
this.radialMenu1.OuterRimThickness = 8;

// Medium rim (balanced, recommended default)
this.radialMenu1.OuterRimThickness = 16;

// Thick rim (bold, touch-friendly)
this.radialMenu1.OuterRimThickness = 28;

// Extra thick rim (very prominent frame)
this.radialMenu1.OuterRimThickness = 40;
```

**Choosing Rim Thickness:**

```csharp
private void ConfigureRimForContext()
{
    // For mouse-focused desktop applications
    this.radialMenu1.OuterRimThickness = 12;  // Thin, elegant
    
    // For touch-focused tablet applications
    this.radialMenu1.OuterRimThickness = 32;  // Thick, easy to tap
    
    // For balanced desktop/touch hybrid
    this.radialMenu1.OuterRimThickness = 20;  // Medium thickness
}
```

**Result:**
Rim thickness affects both aesthetics and usability, especially for touch interfaces.

### Complete Rim Customization Example

```csharp
private void CustomizeOuterRim()
{
    // Professional blue theme
    this.radialMenu1.RimBackground = Color.FromArgb(0, 99, 177);  // Corporate blue
    this.radialMenu1.OuterRimThickness = 24;  // Prominent but not overwhelming
    
    // Ensure rim is visible against background
    this.radialMenu1.BackColor = Color.White;
}

private void CreateGradientRimEffect()
{
    // Outer rim with complementary color
    this.radialMenu1.RimBackground = Color.DarkBlue;
    this.radialMenu1.OuterRimThickness = 28;
    
    // Inner region with lighter shade
    this.radialMenu1.OuterArcColor = Color.FromArgb(30, 60, 120);
    
    // Creates visual depth effect
}
```

**Result:**
A visually appealing rim that enhances the menu's professional appearance.

## Arc Gap Configuration

The `OuterArcGap` property controls the spacing between the outer rim and the highlighted arc that appears on hover. This gap creates visual breathing room and helps distinguish interactive elements.

### Setting Arc Gap

```csharp
// Set gap between rim and hover arc
this.radialMenu1.OuterArcGap = 50;
```

**Gap Size Guidelines:**

```csharp
// No gap (arc touches rim)
this.radialMenu1.OuterArcGap = 0;

// Small gap (subtle separation)
this.radialMenu1.OuterArcGap = 10;

// Medium gap (recommended default)
this.radialMenu1.OuterArcGap = 30;

// Large gap (prominent separation)
this.radialMenu1.OuterArcGap = 50;

// Extra large gap (very spacious)
this.radialMenu1.OuterArcGap = 80;
```

**Visual Impact of Gap Size:**

```csharp
private void DemonstrateArcGapEffects()
{
    // Compact appearance (minimal gap)
    this.radialMenu1.OuterArcGap = 15;
    this.radialMenu1.OuterRimThickness = 20;
    // Total: Rim + 15px gap creates focused look
    
    // Spacious appearance (large gap)
    this.radialMenu1.OuterArcGap = 60;
    this.radialMenu1.OuterRimThickness = 25;
    // Total: Rim + 60px gap creates airy feel
}
```

**Relationship with Menu Size:**

```csharp
private void ScaleArcGapWithMenuSize()
{
    // Small menu (280x280)
    this.radialMenu1.Size = new Size(280, 280);
    this.radialMenu1.OuterArcGap = 20;  // Proportionally smaller gap
    
    // Medium menu (350x350)
    this.radialMenu1.Size = new Size(350, 350);
    this.radialMenu1.OuterArcGap = 35;  // Balanced gap
    
    // Large menu (500x500)
    this.radialMenu1.Size = new Size(500, 500);
    this.radialMenu1.OuterArcGap = 60;  // Larger gap for bigger menu
}
```

**Result:**
The arc gap affects the menu's visual density and helps users distinguish between static and interactive elements.

### Complete Arc Gap Example

```csharp
private void CreateBalancedAppearance()
{
    // Configure menu size
    this.radialMenu1.Size = new Size(320, 320);
    
    // Set rim properties
    this.radialMenu1.RimBackground = Color.FromArgb(0, 120, 215);
    this.radialMenu1.OuterRimThickness = 24;
    
    // Set arc gap for visual balance
    this.radialMenu1.OuterArcGap = 30;
    
    // Set drill region colors
    this.radialMenu1.OuterArcColor = Color.FromArgb(80, 80, 80);
    this.radialMenu1.OuterArcHighLightedColor = Color.FromArgb(120, 120, 120);
    
    // Result: Harmonious spacing between all elements
}
```

**Result:**
Well-balanced visual spacing that creates a professional, uncluttered appearance.

## Display Style Options

The `DisplayStyle` property controls how text and images are arranged on menu items. Choose the style that best fits your application's needs and icon availability.

### Available Display Styles

```csharp
// Show only text (no images)
this.radialMenu1.DisplayStyle = DisplayStyle.Text;

// Show only images (no text)
this.radialMenu1.DisplayStyle = DisplayStyle.Image;

// Show text above images
this.radialMenu1.DisplayStyle = DisplayStyle.TextAboveImage;

// Show images above text (recommended default)
this.radialMenu1.DisplayStyle = DisplayStyle.ImageAboveText;
```

### Text-Only Display

Use when icons aren't available or when text clarity is most important.

```csharp
private void CreateTextOnlyMenu()
{
    this.radialMenu1.DisplayStyle = DisplayStyle.Text;
    
    // Add items with descriptive text
    RadialMenuItem item1 = new RadialMenuItem();
    item1.Text = "New Document";  // Clear, descriptive
    
    RadialMenuItem item2 = new RadialMenuItem();
    item2.Text = "Open File";
    
    RadialMenuItem item3 = new RadialMenuItem();
    item3.Text = "Save";
    
    this.radialMenu1.Items.Add(item1);
    this.radialMenu1.Items.Add(item2);
    this.radialMenu1.Items.Add(item3);
}
```

**When to Use:**
- Early prototypes without final icons
- Text-heavy business applications
- When icon meaning might be ambiguous
- Accessibility-first designs

**Result:**
Clean, text-focused menu that's immediately understandable.

### Image-Only Display

Use when icons are self-explanatory and screen space is limited.

```csharp
private void CreateIconOnlyMenu()
{
    // Set up image list
    ImageListAdv imageList = new ImageListAdv(this.components);
    imageList.Images.Add(Image.FromFile("icons/new.png"));
    imageList.Images.Add(Image.FromFile("icons/open.png"));
    imageList.Images.Add(Image.FromFile("icons/save.png"));
    imageList.Images.Add(Image.FromFile("icons/print.png"));
    
    this.radialMenu1.ImageList = imageList;
    this.radialMenu1.DisplayStyle = DisplayStyle.Image;
    
    // Add items with images but no visible text
    RadialMenuItem newItem = new RadialMenuItem();
    newItem.Text = "New";  // Used for tooltips, not displayed
    newItem.ImageIndex = 0;
    
    RadialMenuItem openItem = new RadialMenuItem();
    openItem.Text = "Open";
    openItem.ImageIndex = 1;
    
    this.radialMenu1.Items.Add(newItem);
    this.radialMenu1.Items.Add(openItem);
    
    // Enable tooltips so users can see text
    this.radialMenu1.ShowToolTip = true;
}
```

**When to Use:**
- Space-constrained mobile/tablet interfaces
- Graphics applications with standard icons
- Power users familiar with iconography
- Minimal, modern design aesthetics

**Result:**
Compact, icon-focused menu that maximizes available space.

### Image Above Text (Recommended)

The most common and balanced display style.

```csharp
private void CreateBalancedMenu()
{
    // Set up images
    ImageListAdv imageList = new ImageListAdv(this.components);
    imageList.Images.Add(Properties.Resources.NewIcon);
    imageList.Images.Add(Properties.Resources.OpenIcon);
    imageList.Images.Add(Properties.Resources.SaveIcon);
    
    this.radialMenu1.ImageList = imageList;
    this.radialMenu1.DisplayStyle = DisplayStyle.ImageAboveText;
    
    // Add items with both images and text
    RadialMenuItem newItem = new RadialMenuItem();
    newItem.Text = "New";
    newItem.ImageIndex = 0;
    
    RadialMenuItem openItem = new RadialMenuItem();
    openItem.Text = "Open";
    openItem.ImageIndex = 1;
    
    RadialMenuItem saveItem = new RadialMenuItem();
    saveItem.Text = "Save";
    saveItem.ImageIndex = 2;
    
    this.radialMenu1.Items.Add(newItem);
    this.radialMenu1.Items.Add(openItem);
    this.radialMenu1.Items.Add(saveItem);
}
```

**When to Use:**
- General-purpose applications
- Mixed user experience levels
- When both visual and textual clarity are important
- Standard desktop applications

**Result:**
Professional appearance with both visual icons and clear text labels.

### Text Above Image

Less common but useful for specific scenarios.

```csharp
private void CreateTextAboveImageMenu()
{
    this.radialMenu1.DisplayStyle = DisplayStyle.TextAboveImage;
    
    // Works well when text is most important but images add context
}
```

**When to Use:**
- Educational applications emphasizing reading
- Applications with small icons used as decorations
- Specific design requirements

## Image Size Customization

Control the size of icons displayed on menu items, either uniformly for all items or individually per item.

### Uniform Image Size for All Items

The `MenuItemImageSize` property sets a consistent image size for all menu items.

```csharp
// Set uniform image size for all items
this.radialMenu1.MenuItemImageSize = new Size(24, 24);
```

**Common Image Sizes:**

```csharp
// Small icons (compact appearance)
this.radialMenu1.MenuItemImageSize = new Size(16, 16);

// Standard icons (balanced)
this.radialMenu1.MenuItemImageSize = new Size(24, 24);

// Large icons (easier to see, touch-friendly)
this.radialMenu1.MenuItemImageSize = new Size(32, 32);

// Extra large icons (very prominent)
this.radialMenu1.MenuItemImageSize = new Size(48, 48);
```

**Complete Example with Uniform Sizing:**

```csharp
private void CreateMenuWithUniformIcons()
{
    // Set up menu
    this.radialMenu1.Style = RadialMenuStyle.Office2016Colorful;
    this.radialMenu1.DisplayStyle = DisplayStyle.ImageAboveText;
    this.radialMenu1.WedgeCount = 4;
    this.radialMenu1.Size = new Size(280, 280);
    
    // Set uniform image size
    this.radialMenu1.MenuItemImageSize = new Size(24, 24);
    
    // Set up image list
    ImageListAdv imageList = new ImageListAdv(this.components);
    imageList.Images.Add(Image.FromFile("icons/edit.png"));
    imageList.Images.Add(Image.FromFile("icons/cut.png"));
    imageList.Images.Add(Image.FromFile("icons/copy.png"));
    imageList.Images.Add(Image.FromFile("icons/paste.png"));
    this.radialMenu1.ImageList = imageList;
    
    // Create items - all will use 24x24 icons
    string[] itemNames = { "Edit", "Cut", "Copy", "Paste" };
    for (int i = 0; i < itemNames.Length; i++)
    {
        RadialMenuItem item = new RadialMenuItem();
        item.Text = itemNames[i];
        item.ImageIndex = i;
        this.radialMenu1.Items.Add(item);
    }
}
```

**Result:**
All menu items display with consistent 24x24 pixel icons, creating a uniform appearance.

### Individual Item Image Size

The `ImageSize` property on individual RadialMenuItem instances overrides the global setting.

```csharp
private void CreateMenuWithVariedIconSizes()
{
    // Set default size for most items
    this.radialMenu1.MenuItemImageSize = new Size(24, 24);
    
    // Create regular items
    RadialMenuItem cutItem = new RadialMenuItem();
    cutItem.Text = "Cut";
    cutItem.ImageIndex = 0;
    // Uses default 24x24
    
    RadialMenuItem copyItem = new RadialMenuItem();
    copyItem.Text = "Copy";
    copyItem.ImageIndex = 1;
    // Uses default 24x24
    
    // Create emphasized item with larger icon
    RadialMenuItem saveItem = new RadialMenuItem();
    saveItem.Text = "Save";
    saveItem.ImageIndex = 2;
    saveItem.ImageSize = new Size(32, 32);  // Override: larger icon
    
    RadialMenuItem pasteItem = new RadialMenuItem();
    pasteItem.Text = "Paste";
    pasteItem.ImageIndex = 3;
    // Uses default 24x24
    
    this.radialMenu1.Items.Add(cutItem);
    this.radialMenu1.Items.Add(copyItem);
    this.radialMenu1.Items.Add(saveItem);  // Stands out with larger icon
    this.radialMenu1.Items.Add(pasteItem);
}
```

**Result:**
The "Save" item has a larger icon (32x32) while others use the default size (24x24), drawing attention to important actions.

**When to Use Individual Sizing:**
- Emphasize primary actions (Save, Submit, Delete)
- Distinguish special items (Help, Settings)
- Create visual hierarchy
- Accommodate icons with different detail levels

### Scaling Icons Based on Menu Size

```csharp
private void ScaleIconsWithMenuSize(Size menuSize)
{
    // Calculate proportional icon size (roughly 8-10% of menu diameter)
    int iconSize = (int)(menuSize.Width * 0.09);
    
    // Ensure within reasonable bounds
    iconSize = Math.Max(16, Math.Min(iconSize, 48));
    
    this.radialMenu1.Size = menuSize;
    this.radialMenu1.MenuItemImageSize = new Size(iconSize, iconSize);
}

// Usage examples
ScaleIconsWithMenuSize(new Size(280, 280));  // ~25px icons
ScaleIconsWithMenuSize(new Size(400, 400));  // ~36px icons
ScaleIconsWithMenuSize(new Size(200, 200));  // ~18px icons
```

**Result:**
Icons automatically scale appropriately for different menu sizes.

## Center Icon Configuration

The `Icon` property sets the image displayed at the center of the RadialMenu, providing branding or contextual visual identity.

### Setting Center Icon

```csharp
// Set center icon from file
this.radialMenu1.Icon = Image.FromFile("icons/app-logo.png");

// Set center icon from resources
this.radialMenu1.Icon = Properties.Resources.ApplicationLogo;

// Remove center icon
this.radialMenu1.Icon = null;  // No icon displayed
```

### Center Icon Best Practices

```csharp
private void ConfigureCenterIcon()
{
    // Use high-resolution icon (64x64 or larger)
    this.radialMenu1.Icon = Properties.Resources.AppIcon64;
    
    // OR create programmatic icon
    this.radialMenu1.Icon = CreateCustomCenterIcon();
}

private Image CreateCustomCenterIcon()
{
    // Create a 64x64 icon programmatically
    Bitmap icon = new Bitmap(64, 64);
    using (Graphics g = Graphics.FromImage(icon))
    {
        g.SmoothingMode = System.Drawing.Drawing2D.SmoothingMode.AntiAlias;
        g.Clear(Color.Transparent);
        
        // Draw custom icon (example: colored circle with letter)
        g.FillEllipse(Brushes.DodgerBlue, 8, 8, 48, 48);
        g.DrawString("A", new Font("Arial", 28, FontStyle.Bold), 
                     Brushes.White, new PointF(18, 14));
    }
    return icon;
}
```

**Result:**
A professional center icon that reinforces brand identity or context.

## Complete Styling Examples

### Example 1: Modern Dark Theme

```csharp
private void ApplyModernDarkTheme()
{
    // Overall style
    this.radialMenu1.Style = RadialMenuStyle.Office2016Black;
    this.radialMenu1.Size = new Size(320, 320);
    this.radialMenu1.BackColor = Color.FromArgb(30, 30, 30);
    
    // Drill region - dark with lighter hover
    this.radialMenu1.OuterArcColor = Color.FromArgb(45, 45, 48);
    this.radialMenu1.OuterArcHighLightedColor = Color.FromArgb(70, 70, 75);
    
    // Outer rim - subtle gray
    this.radialMenu1.RimBackground = Color.FromArgb(60, 60, 60);
    this.radialMenu1.OuterRimThickness = 20;
    
    // Spacing
    this.radialMenu1.OuterArcGap = 30;
    
    // Display
    this.radialMenu1.DisplayStyle = DisplayStyle.ImageAboveText;
    this.radialMenu1.MenuItemImageSize = new Size(28, 28);
    
    // Center icon
    this.radialMenu1.Icon = CreateMonochromeIcon();
}
```

**Result:**
Sleek dark theme perfect for modern applications and low-light environments.

### Example 2: Light Professional Theme

```csharp
private void ApplyLightProfessionalTheme()
{
    // Overall style
    this.radialMenu1.Style = RadialMenuStyle.Office2016White;
    this.radialMenu1.Size = new Size(300, 300);
    this.radialMenu1.BackColor = Color.White;
    
    // Drill region - light gray with darker hover
    this.radialMenu1.OuterArcColor = Color.FromArgb(230, 230, 230);
    this.radialMenu1.OuterArcHighLightedColor = Color.FromArgb(190, 190, 190);
    
    // Outer rim - corporate blue
    this.radialMenu1.RimBackground = Color.FromArgb(0, 120, 215);
    this.radialMenu1.OuterRimThickness = 24;
    
    // Spacing
    this.radialMenu1.OuterArcGap = 35;
    
    // Display
    this.radialMenu1.DisplayStyle = DisplayStyle.ImageAboveText;
    this.radialMenu1.MenuItemImageSize = new Size(24, 24);
    
    // Center icon
    this.radialMenu1.Icon = Properties.Resources.CompanyLogo;
}
```

**Result:**
Clean, professional appearance suitable for business applications.

### Example 3: Touch-Optimized Tablet Theme

```csharp
private void ApplyTouchOptimizedTheme()
{
    // Larger overall size for touch
    this.radialMenu1.Size = new Size(400, 400);
    this.radialMenu1.Style = RadialMenuStyle.Office2016Colorful;
    
    // Thicker rim for easier edge tapping
    this.radialMenu1.RimBackground = Color.FromArgb(0, 99, 177);
    this.radialMenu1.OuterRimThickness = 40;
    
    // Larger gap for clearer separation
    this.radialMenu1.OuterArcGap = 50;
    
    // Drill region with high contrast
    this.radialMenu1.OuterArcColor = Color.FromArgb(80, 80, 80);
    this.radialMenu1.OuterArcHighLightedColor = Color.FromArgb(0, 150, 255);
    
    // Larger icons for touch
    this.radialMenu1.DisplayStyle = DisplayStyle.ImageAboveText;
    this.radialMenu1.MenuItemImageSize = new Size(36, 36);
    
    // Larger center icon
    this.radialMenu1.Icon = CreateLargeCenterIcon(80);
    
    // Fewer items per level for larger touch targets
    this.radialMenu1.WedgeCount = 5;
}
```

**Result:**
Optimized for finger touch with larger targets and clearer visual separation.

## Visual Customization Patterns

### Pattern 1: High Contrast Accessibility

```csharp
private void ApplyHighContrastTheme()
{
    // Strong contrasts for visibility
    this.radialMenu1.OuterArcColor = Color.Black;
    this.radialMenu1.OuterArcHighLightedColor = Color.Yellow;
    this.radialMenu1.RimBackground = Color.White;
    this.radialMenu1.OuterRimThickness = 8;
    this.radialMenu1.BackColor = Color.Black;
    
    // Larger text and icons
    this.radialMenu1.DisplayStyle = DisplayStyle.ImageAboveText;
    this.radialMenu1.MenuItemImageSize = new Size(32, 32);
    this.radialMenu1.Font = new Font("Arial", 12, FontStyle.Bold);
}
```

### Pattern 2: Minimal Modern Design

```csharp
private void ApplyMinimalDesign()
{
    // Clean, minimal styling
    this.radialMenu1.Style = RadialMenuStyle.Office2016White;
    this.radialMenu1.OuterArcColor = Color.FromArgb(250, 250, 250);
    this.radialMenu1.OuterArcHighLightedColor = Color.FromArgb(240, 240, 240);
    
    // Thin, subtle rim
    this.radialMenu1.RimBackground = Color.FromArgb(220, 220, 220);
    this.radialMenu1.OuterRimThickness = 10;
    
    // Small gap for compact look
    this.radialMenu1.OuterArcGap = 15;
    
    // Icon-focused with small text
    this.radialMenu1.DisplayStyle = DisplayStyle.Image;
    this.radialMenu1.ShowToolTip = true;  // Text in tooltips
}
```

### Pattern 3: Bold Colorful Design

```csharp
private void ApplyBoldColorfulDesign()
{
    // Vibrant, eye-catching colors
    this.radialMenu1.Style = RadialMenuStyle.Office2016Colorful;
    this.radialMenu1.OuterArcColor = Color.FromArgb(255, 0, 128);  // Hot pink
    this.radialMenu1.OuterArcHighLightedColor = Color.FromArgb(255, 100, 180);
    
    // Contrasting rim
    this.radialMenu1.RimBackground = Color.FromArgb(0, 200, 255);  // Cyan
    this.radialMenu1.OuterRimThickness = 28;
    
    // Generous spacing
    this.radialMenu1.OuterArcGap = 40;
    
    // Large, colorful icons
    this.radialMenu1.DisplayStyle = DisplayStyle.ImageAboveText;
    this.radialMenu1.MenuItemImageSize = new Size(32, 32);
}
```

## Best Practices

**1. Maintain Contrast Ratios**
```csharp
// Ensure sufficient contrast between drill region states
// Bad: Too similar
this.radialMenu1.OuterArcColor = Color.FromArgb(100, 100, 100);
this.radialMenu1.OuterArcHighLightedColor = Color.FromArgb(110, 110, 110);  // Only 10 difference

// Good: Clear difference
this.radialMenu1.OuterArcColor = Color.FromArgb(80, 80, 80);
this.radialMenu1.OuterArcHighLightedColor = Color.FromArgb(140, 140, 140);  // 60 difference
```

**2. Scale Elements Proportionally**
```csharp
// When increasing menu size, scale other elements
this.radialMenu1.Size = new Size(400, 400);  // Larger than default
this.radialMenu1.OuterRimThickness = 32;     // Proportionally thicker
this.radialMenu1.OuterArcGap = 50;           // Proportionally larger
this.radialMenu1.MenuItemImageSize = new Size(32, 32);  // Larger icons
```

**3. Consider Context and Platform**
```csharp
// Desktop mouse interface
this.radialMenu1.OuterRimThickness = 16;
this.radialMenu1.MenuItemImageSize = new Size(24, 24);

// Touch tablet interface
this.radialMenu1.OuterRimThickness = 36;
this.radialMenu1.MenuItemImageSize = new Size(40, 40);
```

**4. Test with Actual Content**
```csharp
// Always test styling with real menu items
private void TestStyling()
{
    ApplyCustomTheme();
    
    // Add real items to see actual appearance
    PopulateMenuWithRealContent();
    
    // Adjust as needed
}
```

**5. Provide Clear Visual Feedback**
```csharp
// Always differentiate hover state significantly
this.radialMenu1.OuterArcColor = Color.Gray;
this.radialMenu1.OuterArcHighLightedColor = Color.Blue;  // Clear change on hover
```

**6. Use Consistent Display Style**
```csharp
// Pick one display style and use throughout application
this.radialMenu1.DisplayStyle = DisplayStyle.ImageAboveText;
// Don't mix different styles across different menus
```
