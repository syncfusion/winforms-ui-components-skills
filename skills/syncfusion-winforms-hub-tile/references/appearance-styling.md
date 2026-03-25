# Appearance and Styling

## Table of Contents
- [Overview](#overview)
- [Banner Feature](#banner-feature)
- [Selection Markers](#selection-markers)
- [Hover Effects](#hover-effects)
- [Tile Press Effect](#tile-press-effect)
- [Color Customization](#color-customization)
- [Complete Styling Examples](#complete-styling-examples)
- [Windows 8 Design Guidelines](#windows-8-design-guidelines)
- [Troubleshooting](#troubleshooting)

## Overview

HubTile offers comprehensive appearance customization to create visually appealing Windows 8/Windows Phone-style interfaces. Customize banners, selection markers, hover effects, colors, and interactive behaviors.

**Customization capabilities:**
- Banner overlays with text and icons
- Selection markers for state indication
- Hover color effects
- Expand-on-hover animations
- Tile press/slide effects
- Background colors and images

## Banner Feature

Banners display as **overlays at the bottom** of tiles, providing additional information or context without obscuring the main content.

### Enabling Banner

```csharp
// Show banner
hubTile1.ShowBanner = true;
```

**ShowBanner property:**
- `true` - Banner visible
- `false` - Banner hidden (default)

### Banner Text

```csharp
// Set banner text
hubTile1.Banner.Text = "New Update Available";

// Banner text color
hubTile1.Banner.TextColor = Color.White;
```

**Best practices:**
- Keep text short (5-15 characters)
- Use contrasting colors
- Provide actionable or informative content

### Banner Icon

Add an icon to the banner using `BannerIcon` property:

```csharp
// From file
hubTile1.BannerIcon = Image.FromFile("icon.png");

// From resources
hubTile1.BannerIcon = Properties.Resources.IconImage;

// From ImageList
hubTile1.BannerIcon = imageListAdv1.Images[0];
```

**Icon recommendations:**
- Size: 16x16 or 24x24 pixels
- Format: PNG with transparency
- Style: Simple, recognizable symbols
- Color: High contrast with banner background

### Banner Color

```csharp
// Set banner background color
hubTile1.BannerColor = Color.FromArgb(200, 0, 0, 0);  // Semi-transparent black

// Solid colors
hubTile1.BannerColor = Color.FromArgb(17, 158, 218);  // Blue
hubTile1.BannerColor = Color.FromArgb(255, 140, 0);   // Orange
```

**Color tips:**
- Use semi-transparency (alpha 150-200) for overlay effect
- Match or complement tile BackColor
- Ensure text contrast

### Complete Banner Example

```csharp
HubTile newsTile = new HubTile();
newsTile.Size = new Size(200, 200);
newsTile.TileType = HubTileType.DefaultTile;
newsTile.BackColor = Color.FromArgb(0, 120, 215);

// Main content
newsTile.Title.Text = "Breaking News";
newsTile.Title.TextColor = Color.White;
newsTile.Footer.Text = "CNN";
newsTile.Footer.TextColor = Color.White;

// Banner configuration
newsTile.ShowBanner = true;
newsTile.Banner.Text = "Live";
newsTile.Banner.TextColor = Color.White;
newsTile.BannerColor = Color.FromArgb(200, 255, 0, 0);  // Semi-transparent red
// newsTile.BannerIcon = Image.FromFile("live_icon.png");

// Enable live tile
newsTile.TurnLiveTileOn = true;
newsTile.SlideTransition = TransitionDirection.BottomToTop;

this.Controls.Add(newsTile);
```

## Selection Markers

Selection markers indicate when a tile is selected or marked, displaying a **checkmark or indicator** in the top-right corner.

### Enable Selection Marker

```csharp
// Show selection marker
hubTile1.IsSelectionMarked = true;
```

**IsSelectionMarked property:**
- `true` - Selection marker visible
- `false` - No marker (default)

### Selection Marker Color

```csharp
// Set marker color
hubTile1.SelectionMarkerColor = Color.White;

// Match tile color scheme
hubTile1.SelectionMarkerColor = Color.FromArgb(17, 158, 218);

// High visibility
hubTile1.SelectionMarkerColor = Color.Yellow;
```

**Color guidelines:**
- Use high contrast with tile background
- White or yellow for dark tiles
- Dark colors for light tiles
- Match application theme

### Selection Example

```csharp
HubTile selectionTile = new HubTile();
selectionTile.Size = new Size(200, 200);
selectionTile.BackColor = Color.FromArgb(139, 0, 139);
selectionTile.Title.Text = "Selected Item";
selectionTile.Title.TextColor = Color.White;

// Enable selection marker
selectionTile.IsSelectionMarked = true;
selectionTile.SelectionMarkerColor = Color.White;

this.Controls.Add(selectionTile);
```

### Interactive Selection

```csharp
HubTile interactiveTile = new HubTile();
interactiveTile.Click += (s, e) => {
    // Toggle selection on click
    interactiveTile.IsSelectionMarked = !interactiveTile.IsSelectionMarked;
    
    // Update appearance based on selection
    if (interactiveTile.IsSelectionMarked)
    {
        interactiveTile.BackColor = Color.FromArgb(0, 120, 215);
    }
    else
    {
        interactiveTile.BackColor = Color.FromArgb(100, 100, 100);
    }
};
```

## Hover Effects

Enhance interactivity with hover effects that change tile appearance when the mouse enters.

### Enable Hover Color

```csharp
// Enable hover effect
hubTile1.EnableHoverColor = true;

// Set hover border color
hubTile1.HoveredBorderColor = Color.Yellow;
```

**EnableHoverColor property:**
- `true` - Border changes color on hover
- `false` - No hover effect (default)

### Hover Border Color

```csharp
// Bright hover border
hubTile1.HoveredBorderColor = Color.Yellow;

// Subtle hover
hubTile1.HoveredBorderColor = Color.FromArgb(200, 200, 200);

// Brand color hover
hubTile1.HoveredBorderColor = Color.FromArgb(17, 158, 218);
```

**Color selection:**
- Bright colors (yellow, white) for high visibility
- Subtle colors for professional appearance
- Brand colors for consistency

### Expand on Hover

Tiles can slightly expand when hovered, creating a "lift" effect:

```csharp
// Enable expand on hover
hubTile1.ExpandOnHover = true;
```

**ExpandOnHover property:**
- `true` - Tile slightly scales up on hover
- `false` - No expansion (default)

**Visual effect:** Tile grows ~5-10% larger, creating depth

### Complete Hover Example

```csharp
HubTile hoverTile = new HubTile();
hoverTile.Size = new Size(200, 200);
hoverTile.BackColor = Color.FromArgb(0, 120, 215);
hoverTile.Title.Text = "Hover Me";
hoverTile.Title.TextColor = Color.White;
hoverTile.Footer.Text = "Interactive";
hoverTile.Footer.TextColor = Color.White;

// Hover configuration
hoverTile.EnableHoverColor = true;
hoverTile.HoveredBorderColor = Color.Yellow;
hoverTile.ExpandOnHover = true;

this.Controls.Add(hoverTile);
```

### Hover with Cursor Change

```csharp
HubTile clickableTile = new HubTile();
clickableTile.EnableHoverColor = true;
clickableTile.HoveredBorderColor = Color.Yellow;
clickableTile.ExpandOnHover = true;

// Change cursor on hover
clickableTile.Cursor = Cursors.Hand;

// Handle click
clickableTile.Click += (s, e) => {
    MessageBox.Show("Tile clicked!");
};
```

## Tile Press Effect

Create visual feedback when tiles are pressed/clicked with a sliding effect.

### Enable Tile Slide Effect

```csharp
// Enable press effect
hubTile1.EnableTileSlideEffect = true;
```

**EnableTileSlideEffect property:**
- `true` - Tile slides slightly when clicked
- `false` - No press effect (default)

**Visual feedback:** Brief slide animation on click, providing tactile response

### Complete Press Effect Example

```csharp
HubTile pressTile = new HubTile();
pressTile.Size = new Size(200, 200);
pressTile.BackColor = Color.FromArgb(16, 137, 62);
pressTile.Title.Text = "Press Me";
pressTile.Title.TextColor = Color.White;

// Enable press feedback
pressTile.EnableTileSlideEffect = true;

// Combine with hover for full interactivity
pressTile.EnableHoverColor = true;
pressTile.HoveredBorderColor = Color.White;
pressTile.ExpandOnHover = true;

// Handle click
pressTile.Click += (s, e) => {
    pressTile.Title.Text = "Pressed!";
};

this.Controls.Add(pressTile);
```

## Color Customization

### Background Color

```csharp
// Solid colors
hubTile1.BackColor = Color.FromArgb(0, 120, 215);   // Blue
hubTile2.BackColor = Color.FromArgb(16, 137, 62);   // Green
hubTile3.BackColor = Color.FromArgb(255, 140, 0);   // Orange
hubTile4.BackColor = Color.FromArgb(139, 0, 139);   // Purple
```

### Windows 8 Color Palette

Standard Windows 8 tile colors:

```csharp
// Primary colors
Color blue = Color.FromArgb(0, 120, 215);
Color green = Color.FromArgb(16, 137, 62);
Color red = Color.FromArgb(232, 17, 35);
Color orange = Color.FromArgb(255, 140, 0);
Color purple = Color.FromArgb(139, 0, 139);
Color teal = Color.FromArgb(0, 188, 242);
Color yellow = Color.FromArgb(255, 185, 0);
Color pink = Color.FromArgb(232, 17, 135);

// Neutral colors
Color darkGray = Color.FromArgb(50, 50, 50);
Color mediumGray = Color.FromArgb(100, 100, 100);
Color lightGray = Color.FromArgb(200, 200, 200);
```

### Text Color Customization

```csharp
// Title styling
hubTile1.Title.Text = "Custom Title";
hubTile1.Title.TextColor = Color.White;
hubTile1.Title.Font = new Font("Segoe UI", 14, FontStyle.Bold);

// Footer styling
hubTile1.Footer.Text = "Footer Text";
hubTile1.Footer.TextColor = Color.LightGray;
hubTile1.Footer.Font = new Font("Segoe UI", 9, FontStyle.Regular);
```

### Image Background

```csharp
// Set background image
hubTile1.ImageSource = Image.FromFile("background.jpg");
hubTile1.BackColor = Color.Transparent;  // Or complementary color

// Overlay semi-transparent color over image
hubTile1.BackColor = Color.FromArgb(150, 0, 0, 0);  // Dark overlay
```

## Complete Styling Examples

### Example 1: Full-Featured Interactive Tile

```csharp
HubTile featureTile = new HubTile();
featureTile.Size = new Size(200, 200);
featureTile.Location = new Point(20, 20);
featureTile.TileType = HubTileType.DefaultTile;

// Content
featureTile.Title.Text = "Featured Item";
featureTile.Title.TextColor = Color.White;
featureTile.Title.Font = new Font("Segoe UI", 14, FontStyle.Bold);
featureTile.Footer.Text = "Click to view";
featureTile.Footer.TextColor = Color.White;

// Colors
featureTile.BackColor = Color.FromArgb(0, 120, 215);

// Banner
featureTile.ShowBanner = true;
featureTile.Banner.Text = "NEW";
featureTile.Banner.TextColor = Color.White;
featureTile.BannerColor = Color.FromArgb(200, 255, 0, 0);

// Selection
featureTile.IsSelectionMarked = true;
featureTile.SelectionMarkerColor = Color.Yellow;

// Hover effects
featureTile.EnableHoverColor = true;
featureTile.HoveredBorderColor = Color.Yellow;
featureTile.ExpandOnHover = true;

// Press effect
featureTile.EnableTileSlideEffect = true;

// Animation
featureTile.TurnLiveTileOn = true;
featureTile.SlideTransition = TransitionDirection.BottomToTop;

// Interactivity
featureTile.Cursor = Cursors.Hand;
featureTile.Click += (s, e) => {
    MessageBox.Show("Featured item clicked!");
};

this.Controls.Add(featureTile);
```

### Example 2: Notification Tile with Banner

```csharp
HubTile notificationTile = new HubTile();
notificationTile.Size = new Size(200, 200);
notificationTile.TileType = HubTileType.PulsingTile;

// Content
notificationTile.Title.Text = "Messages";
notificationTile.Title.TextColor = Color.White;
notificationTile.Footer.Text = "Inbox";
notificationTile.Footer.TextColor = Color.White;

// Colors
notificationTile.BackColor = Color.FromArgb(0, 120, 215);

// Banner with count
notificationTile.ShowBanner = true;
notificationTile.Banner.Text = "5 New";
notificationTile.Banner.TextColor = Color.Yellow;
notificationTile.BannerColor = Color.FromArgb(180, 0, 0, 0);

// Animation
notificationTile.TurnLiveTileOn = true;
notificationTile.PulseDuration = 2;
notificationTile.PulseScale = 1.3f;

this.Controls.Add(notificationTile);
```

### Example 3: Media Tile with All Features

```csharp
HubTile mediaTile = new HubTile();
mediaTile.Size = new Size(200, 200);
mediaTile.TileType = HubTileType.PulsingTile;

// Content
mediaTile.Title.Text = "Music";
mediaTile.Title.TextColor = Color.White;
mediaTile.Footer.Text = "Now Playing";
mediaTile.Footer.TextColor = Color.White;

// Background
mediaTile.BackColor = Color.FromArgb(139, 0, 139);
// mediaTile.ImageSource = Image.FromFile("album_art.jpg");

// Banner
mediaTile.ShowBanner = true;
mediaTile.Banner.Text = "♫ Playing";
mediaTile.Banner.TextColor = Color.White;
mediaTile.BannerColor = Color.FromArgb(200, 0, 0, 0);

// Hover
mediaTile.EnableHoverColor = true;
mediaTile.HoveredBorderColor = Color.White;
mediaTile.ExpandOnHover = true;

// Press
mediaTile.EnableTileSlideEffect = true;

// Animation
mediaTile.TurnLiveTileOn = true;
mediaTile.PulseDuration = 2;
mediaTile.PulseScale = 1.3f;

// Interactivity
mediaTile.Cursor = Cursors.Hand;

this.Controls.Add(mediaTile);
```

### Example 4: Dashboard Grid with Varied Styling

```csharp
private void CreateStyledDashboard()
{
    FlowLayoutPanel dashboard = new FlowLayoutPanel();
    dashboard.Dock = DockStyle.Fill;
    dashboard.BackColor = Color.FromArgb(30, 30, 30);
    dashboard.Padding = new Padding(10);
    
    // Tile configurations: title, footer, color, banner text
    var tileConfigs = new[]
    {
        ("News", "Latest", Color.FromArgb(17, 158, 218), "LIVE"),
        ("Weather", "72°F", Color.FromArgb(255, 140, 0), ""),
        ("Email", "Inbox", Color.FromArgb(0, 120, 215), "5 NEW"),
        ("Calendar", "Today", Color.FromArgb(16, 137, 62), ""),
        ("Photos", "Gallery", Color.FromArgb(139, 0, 139), ""),
        ("Music", "Library", Color.FromArgb(232, 17, 135), "♫")
    };
    
    foreach (var config in tileConfigs)
    {
        HubTile tile = new HubTile();
        tile.Size = new Size(150, 150);
        tile.Margin = new Padding(5);
        tile.TileType = HubTileType.DefaultTile;
        
        // Content
        tile.Title.Text = config.Item1;
        tile.Title.TextColor = Color.White;
        tile.Footer.Text = config.Item2;
        tile.Footer.TextColor = Color.White;
        tile.BackColor = config.Item3;
        
        // Banner if provided
        if (!string.IsNullOrEmpty(config.Item4))
        {
            tile.ShowBanner = true;
            tile.Banner.Text = config.Item4;
            tile.Banner.TextColor = Color.White;
            tile.BannerColor = Color.FromArgb(200, 0, 0, 0);
        }
        
        // Interactive features
        tile.EnableHoverColor = true;
        tile.HoveredBorderColor = Color.White;
        tile.ExpandOnHover = true;
        tile.EnableTileSlideEffect = true;
        tile.Cursor = Cursors.Hand;
        
        // Animation
        tile.TurnLiveTileOn = true;
        tile.SlideTransition = TransitionDirection.BottomToTop;
        tile.ImageTransitionSpeed = 3;
        
        dashboard.Controls.Add(tile);
    }
    
    this.Controls.Add(dashboard);
}
```

## Windows 8 Design Guidelines

### Size Recommendations

**Standard tile sizes:**
- Small: 150x150 pixels
- Medium: 310x150 pixels (wide)
- Large: 310x310 pixels

```csharp
// Small tile
hubTile1.Size = new Size(150, 150);

// Wide tile
hubTile2.Size = new Size(310, 150);

// Large tile
hubTile3.Size = new Size(310, 310);
```

### Color Guidelines

**Follow Windows 8 principles:**
1. **Vibrant colors:** Use bold, saturated colors
2. **High contrast:** Ensure text is readable
3. **Consistent palette:** Use standard Windows colors
4. **Semantic colors:** Red for alerts, green for success

### Typography

**Font recommendations:**
- Primary font: Segoe UI
- Title: 12-16pt, Bold
- Footer: 9-11pt, Regular
- High contrast: White on dark, Black on light

```csharp
// Title font
hubTile1.Title.Font = new Font("Segoe UI", 14, FontStyle.Bold);
hubTile1.Title.TextColor = Color.White;

// Footer font
hubTile1.Footer.Font = new Font("Segoe UI", 10, FontStyle.Regular);
hubTile1.Footer.TextColor = Color.White;
```

### Spacing and Layout

**Layout principles:**
- 5-10px margin between tiles
- FlowLayoutPanel or TableLayoutPanel for grids
- Consistent tile sizes within groups
- Dark background (30-50 gray) for tile container

```csharp
FlowLayoutPanel layout = new FlowLayoutPanel();
layout.Dock = DockStyle.Fill;
layout.BackColor = Color.FromArgb(30, 30, 30);
layout.Padding = new Padding(10);

// Add tiles with margins
tile.Margin = new Padding(5);
```

## Troubleshooting

### Issue: Banner not visible

**Causes:**
- `ShowBanner = false`
- Banner text/icon not set
- BannerColor matches BackColor

**Solutions:**
```csharp
hubTile1.ShowBanner = true;
hubTile1.Banner.Text = "Banner Text";
hubTile1.BannerColor = Color.FromArgb(200, 0, 0, 0);  // Contrasting color
```

### Issue: Selection marker not visible

**Causes:**
- `IsSelectionMarked = false`
- SelectionMarkerColor matches BackColor

**Solutions:**
```csharp
hubTile1.IsSelectionMarked = true;
hubTile1.SelectionMarkerColor = Color.White;  // High contrast
```

### Issue: Hover effect not working

**Causes:**
- `EnableHoverColor = false`
- HoveredBorderColor not set

**Solutions:**
```csharp
hubTile1.EnableHoverColor = true;
hubTile1.HoveredBorderColor = Color.Yellow;
```

### Issue: Expand on hover too subtle

**Cause:** Normal behavior (small expansion)

**Solution:** Combine with hover color for more noticeable effect:
```csharp
hubTile1.ExpandOnHover = true;
hubTile1.EnableHoverColor = true;
hubTile1.HoveredBorderColor = Color.Yellow;
```

### Issue: Press effect not visible

**Cause:** `EnableTileSlideEffect = false`

**Solution:**
```csharp
hubTile1.EnableTileSlideEffect = true;
```

### Issue: Colors appear washed out

**Causes:**
- Low saturation colors
- Poor contrast

**Solutions:**
```csharp
// Use vibrant Windows 8 colors
hubTile1.BackColor = Color.FromArgb(0, 120, 215);  // Vibrant blue

// High contrast text
hubTile1.Title.TextColor = Color.White;
```

## Best Practices

1. **Use banners sparingly** - Only for important updates or counts
2. **High contrast selection markers** - White or yellow on dark tiles
3. **Enable hover for clickable tiles** - Provides visual feedback
4. **Combine effects judiciously** - Don't overwhelm with all features
5. **Consistent color palette** - Use Windows 8 standard colors
6. **Readable typography** - Segoe UI, adequate size, high contrast
7. **Appropriate tile sizes** - 150x150 for most cases
8. **Dark backgrounds** - 30-50 gray for tile containers
9. **Test interactivity** - Verify hover/click feedback
10. **Semantic colors** - Red for alerts, green for success, blue for info
