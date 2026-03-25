# ImageStreamer Live Tiles

## Table of Contents
- [Overview](#overview)
- [Adding Images to ImageStreamer](#adding-images-to-imagestreamer)
- [InternalBackColor](#internalbackcolor)
- [SlideShow and SliderSpeed](#slideshow-and-sliderspeed)
- [ShowNavigator](#shownavigator)
- [ImageStreamDirection](#imagestreamdirection)
  - [LeftToRight](#lefttoright)
  - [RightToLeft](#righttoLeft)
  - [TopToBottom](#toptobottom)
  - [BottomToTop](#bottomtotop)
  - [HorizontalFlip](#horizontalflip)
- [ImageStreamerType](#imagestreamertype)
- [Live Tile Examples](#live-tile-examples)
- [Common Configurations](#common-configurations)

## Overview

In TileLayout, **ImageStreamer** controls act as individual tiles within LayoutGroups. ImageStreamer is a powerful control that can display:

- Single static images
- Multiple rotating images (slideshow)
- Animated transitions with various directions
- Custom backgrounds and styles

Each ImageStreamer is added to a LayoutGroup's Controls collection and represents one tile in the layout.

**Key Features:**
- Image collection management
- Automatic slideshow animation
- Configurable transition direction (5 options)
- Navigation controls
- Normal or double-horizontal display modes

## Adding Images to ImageStreamer

Images are added to the **Images** collection property, which is an `ImageCollection` type.

### Using Collection Editor (Designer)

1. Select an ImageStreamer control
2. In PropertyGrid, find **Images** property
3. Click the ellipsis (...) button
4. In the Bitmap Collection Editor, click **Add**
5. Select image files to add

![Bitmap Collection Editor](images/CollectionEditorWindow.png)

### Adding Images in Code

```csharp
// Create ImageStreamer
ImageStreamer imageStreamer1 = new ImageStreamer();

// Add images from files
imageStreamer1.Images.Add(Image.FromFile("photo1.jpg"));
imageStreamer1.Images.Add(Image.FromFile("photo2.jpg"));
imageStreamer1.Images.Add(Image.FromFile("photo3.jpg"));

// Add images from resources
imageStreamer1.Images.Add(Properties.Resources.Image1);
imageStreamer1.Images.Add(Properties.Resources.Image2);

// Add to LayoutGroup
layoutGroup1.Controls.Add(imageStreamer1);
```

```vb
' Create ImageStreamer
Dim imageStreamer1 As New ImageStreamer()

' Add images from files
imageStreamer1.Images.Add(Image.FromFile("photo1.jpg"))
imageStreamer1.Images.Add(Image.FromFile("photo2.jpg"))
imageStreamer1.Images.Add(Image.FromFile("photo3.jpg"))

' Add to LayoutGroup
layoutGroup1.Controls.Add(imageStreamer1)
```

**Best Practices:**
- Use consistent image sizes for smooth appearance
- Recommended: 150x150 or 200x200 pixels for standard tiles
- Support common formats: JPG, PNG, BMP, GIF
- Use resource files for embedded images
- Handle `FileNotFoundException` when loading from disk

![Images in ImageStreamer](images/StreamerControl.png)

## InternalBackColor

The **InternalBackColor** property sets the background color of the ImageStreamer tile.

**Property:** `imageStreamer1.InternalBackColor`  
**Type:** `Color`  
**Default:** Control default

```csharp
// Set pink background
imageStreamer1.InternalBackColor = System.Drawing.Color.Pink;
```

```vb
' Set pink background
imageStreamer1.InternalBackColor = System.Drawing.Color.Pink
```

![Custom Background Color](images/Setting-backcolor-for-streamer_control.png)

**When to use:**
- Images with transparency
- Smaller images that don't fill entire tile
- Visual distinction between tiles
- Branding/color coding

**Example - Color-coded tiles:**
```csharp
// Status indicators with colored backgrounds
ImageStreamer successTile = new ImageStreamer();
successTile.InternalBackColor = Color.FromArgb(16, 124, 16); // Green
successTile.Images.Add(Properties.Resources.CheckIcon);

ImageStreamer warningTile = new ImageStreamer();
warningTile.InternalBackColor = Color.FromArgb(255, 185, 0); // Yellow
warningTile.Images.Add(Properties.Resources.WarningIcon);

ImageStreamer errorTile = new ImageStreamer();
errorTile.InternalBackColor = Color.FromArgb(232, 17, 35); // Red
errorTile.Images.Add(Properties.Resources.ErrorIcon);
```

## SlideShow and SliderSpeed

### SlideShow

The **SlideShow** property enables automatic rotation through images in the Images collection.

**Property:** `imageStreamer1.SlideShow`  
**Type:** `bool`  
**Default:** `false`

```csharp
// Enable slideshow
imageStreamer1.SlideShow = true;
```

```vb
' Enable slideshow
imageStreamer1.SlideShow = True
```

**Effect:**
- Automatically cycles through all images in Images collection
- Uses configured ImageStreamDirection for transitions
- Continuous loop animation
- Speed controlled by SliderSpeed property

### SliderSpeed

The **SliderSpeed** property controls the animation speed in milliseconds.

**Property:** `imageStreamer1.SliderSpeed`  
**Type:** `int`  
**Default:** 100 (milliseconds)

```csharp
// Slow animation (200ms per transition)
imageStreamer1.SliderSpeed = 200;
```

```vb
' Slow animation
imageStreamer1.SliderSpeed = 200
```

**Recommended values:**
- **Fast:** 50-75 ms (quick transitions)
- **Normal:** 100-150 ms (default, balanced)
- **Slow:** 200-300 ms (leisurely, readable)
- **Very Slow:** 400+ ms (emphasis, photography)

**Example - Live news tile:**
```csharp
ImageStreamer newsTile = new ImageStreamer();
newsTile.Images.Add(newsImage1);
newsTile.Images.Add(newsImage2);
newsTile.Images.Add(newsImage3);
newsTile.SlideShow = true;
newsTile.SliderSpeed = 150; // Moderate speed for reading headlines
```

## ShowNavigator

The **ShowNavigator** property displays navigation arrows for manual image control.

**Property:** `imageStreamer1.ShowNavigator`  
**Type:** `bool`  
**Default:** `false`  
**Requirement:** SlideShow must be `true`

```csharp
// Show navigation arrows
imageStreamer1.ShowNavigator = true;
imageStreamer1.SlideShow = true; // Required
```

```vb
' Show navigation arrows
imageStreamer1.ShowNavigator = True
imageStreamer1.SlideShow = True ' Required
```

![Navigation Controls](images/Navigator.png)

**Effect:**
- Adds left/right arrow buttons to tile
- Users can manually navigate images
- Pauses automatic slideshow during manual navigation
- Resumes automatic slideshow after inactivity

**When to use:**
- Photo galleries (user-controlled viewing)
- Detailed content (users need time to read)
- Interactive tiles (user-driven navigation)

**Example - Photo gallery tile:**
```csharp
ImageStreamer photoGallery = new ImageStreamer();
foreach (string photo in Directory.GetFiles("Gallery", "*.jpg"))
{
    photoGallery.Images.Add(Image.FromFile(photo));
}
photoGallery.SlideShow = true;
photoGallery.ShowNavigator = true; // Allow manual control
photoGallery.SliderSpeed = 200; // Slower for viewing
```

## ImageStreamDirection

The **ImageStreamDirection** property controls the animation direction for slideshow transitions.

**Property:** `imageStreamer1.ImageStreamDirection`  
**Type:** `Syncfusion.Windows.Forms.Tools.ImageStreamer.StreamDirection`  
**Default:** `LeftToRight`

**Available directions:**
- LeftToRight
- RightToLeft
- TopToBottom
- BottomToTop
- HorizontalFlip

### LeftToRight

Images slide from left to right.

```csharp
imageStreamer1.ImageStreamDirection = 
    Syncfusion.Windows.Forms.Tools.ImageStreamer.StreamDirection.LeftToRight;
```

```vb
imageStreamer1.ImageStreamDirection = 
    Syncfusion.Windows.Forms.Tools.ImageStreamer.StreamDirection.LeftToRight
```

![Left to Right Transition](images/StreamDirection1.png)

**Use case:** Standard forward progression, natural reading direction (LTR languages).

### RightToLeft

Images slide from right to left.

```csharp
imageStreamer1.ImageStreamDirection = 
    Syncfusion.Windows.Forms.Tools.ImageStreamer.StreamDirection.RightToLeft;
```

```vb
imageStreamer1.ImageStreamDirection = 
    Syncfusion.Windows.Forms.Tools.ImageStreamer.StreamDirection.RightToLeft
```

![Right to Left Transition](images/StreamDirection2.png)

**Use case:** RTL languages (Arabic, Hebrew), reverse chronology, "rewind" effect.

### TopToBottom

Images slide from top to bottom.

```csharp
imageStreamer1.ImageStreamDirection = 
    Syncfusion.Windows.Forms.Tools.ImageStreamer.StreamDirection.TopToBottom;
```

```vb
imageStreamer1.ImageStreamDirection = 
    Syncfusion.Windows.Forms.Tools.ImageStreamer.StreamDirection.TopToBottom
```

![Top to Bottom Transition](images/StreamDirection3.png)

**Use case:** Vertical scrolling effect, news tickers, status updates, "falling" content.

### BottomToTop

Images slide from bottom to top.

```csharp
imageStreamer1.ImageStreamDirection = 
    Syncfusion.Windows.Forms.Tools.ImageStreamer.StreamDirection.BottomToTop;
```

```vb
imageStreamer1.ImageStreamDirection = 
    Syncfusion.Windows.Forms.Tools.ImageStreamer.StreamDirection.BottomToTop
```

![Bottom to Top Transition](images/StreamDirection4.png)

**Use case:** "Rising" effect, upward progress, elevator-style transitions.

### HorizontalFlip

Images flip horizontally (card-flip effect).

```csharp
imageStreamer1.ImageStreamDirection = 
    Syncfusion.Windows.Forms.Tools.ImageStreamer.StreamDirection.HorizontalFlip;
```

```vb
imageStreamer1.ImageStreamDirection = 
    Syncfusion.Windows.Forms.Tools.ImageStreamer.StreamDirection.HorizontalFlip
```

![Horizontal Flip Transition](images/StreamDirection5.png)

**Use case:** Card-flip reveal, dramatic transitions, interactive "flip" effect.

**Example - Multiple direction tiles:**
```csharp
// Create tiles with different animation directions
ImageStreamer tile1 = CreateLiveTile(images, StreamDirection.LeftToRight);
ImageStreamer tile2 = CreateLiveTile(images, StreamDirection.TopToBottom);
ImageStreamer tile3 = CreateLiveTile(images, StreamDirection.HorizontalFlip);

private ImageStreamer CreateLiveTile(List<Image> images, StreamDirection direction)
{
    ImageStreamer tile = new ImageStreamer();
    foreach (Image img in images)
    {
        tile.Images.Add(img);
    }
    tile.SlideShow = true;
    tile.SliderSpeed = 150;
    tile.ImageStreamDirection = direction;
    return tile;
}
```

## ImageStreamerType

The **Type** property determines how many images are displayed simultaneously.

**Property:** `imageStreamer1.Type`  
**Type:** `Syncfusion.Windows.Forms.Tools.ImageStreamer.ImageStreamerType`  
**Values:** `Normal`, `DoubleHorizontal`

### Normal Type

Displays **one image at a time**.

```csharp
imageStreamer1.Type = 
    Syncfusion.Windows.Forms.Tools.ImageStreamer.ImageStreamerType.Normal;
```

```vb
imageStreamer1.Type = 
    Syncfusion.Windows.Forms.Tools.ImageStreamer.ImageStreamerType.Normal
```

**Use case:** Standard tiles, full-size image display, single-focus content.

### DoubleHorizontal Type

Displays **two images side-by-side** or resizes single images to double-width.

```csharp
imageStreamer2.Type = 
    Syncfusion.Windows.Forms.Tools.ImageStreamer.ImageStreamerType.DoubleHorizontal;
```

```vb
imageStreamer2.Type = 
    Syncfusion.Windows.Forms.Tools.ImageStreamer.ImageStreamerType.DoubleHorizontal
```

![Double Horizontal Display](images/Properties_img1.jpeg)

**Behavior:**
- **With 2+ images:** Shows two images side-by-side simultaneously
- **With 1 image:** Stretches single image to fill double-width space

**Use case:**
- Before/After comparisons
- Side-by-side photo displays
- Dual-content tiles
- Wide banner tiles

**Example - Comparison tile:**
```csharp
// Before/After comparison tile
ImageStreamer comparisonTile = new ImageStreamer();
comparisonTile.Type = ImageStreamer.ImageStreamerType.DoubleHorizontal;
comparisonTile.Images.Add(beforeImage);
comparisonTile.Images.Add(afterImage);
// Shows both images side-by-side
```

## Live Tile Examples

### News Feed Tile

```csharp
ImageStreamer newsFeedTile = new ImageStreamer();

// Add news headline images
newsFeedTile.Images.Add(CreateHeadlineImage("Breaking: Major Event"));
newsFeedTile.Images.Add(CreateHeadlineImage("Tech: New Product Launch"));
newsFeedTile.Images.Add(CreateHeadlineImage("Sports: Championship Game"));

// Configure live tile
newsFeedTile.SlideShow = true;
newsFeedTile.SliderSpeed = 200; // Slow enough to read
newsFeedTile.ImageStreamDirection = StreamDirection.TopToBottom; // Scrolling news
newsFeedTile.InternalBackColor = Color.FromArgb(0, 120, 215);

layoutGroup.Controls.Add(newsFeedTile);
```

### Photo Gallery Tile

```csharp
ImageStreamer photoTile = new ImageStreamer();

// Load photos
foreach (string file in Directory.GetFiles("Photos", "*.jpg").Take(5))
{
    photoTile.Images.Add(Image.FromFile(file));
}

// Configure with user control
photoTile.SlideShow = true;
photoTile.SliderSpeed = 250; // Slow viewing
photoTile.ShowNavigator = true; // Manual navigation
photoTile.ImageStreamDirection = StreamDirection.HorizontalFlip; // Dramatic flip
photoTile.InternalBackColor = Color.Black;

layoutGroup.Controls.Add(photoTile);
```

### Status Dashboard Tile

```csharp
ImageStreamer statusTile = new ImageStreamer();

// Add status images
statusTile.Images.Add(CreateStatusImage("CPU: 45%", Color.Green));
statusTile.Images.Add(CreateStatusImage("Memory: 60%", Color.Yellow));
statusTile.Images.Add(CreateStatusImage("Disk: 30%", Color.Green));
statusTile.Images.Add(CreateStatusImage("Network: Active", Color.Green));

// Configure for quick cycling
statusTile.SlideShow = true;
statusTile.SliderSpeed = 100; // Quick transitions
statusTile.ImageStreamDirection = StreamDirection.LeftToRight;
statusTile.InternalBackColor = Color.FromArgb(32, 32, 32); // Dark

layoutGroup.Controls.Add(statusTile);
```

### Wide Banner Tile

```csharp
ImageStreamer bannerTile = new ImageStreamer();

// Use DoubleHorizontal for wide display
bannerTile.Type = ImageStreamer.ImageStreamerType.DoubleHorizontal;
bannerTile.Images.Add(CreateWideBanner("Special Offer!"));
bannerTile.Images.Add(CreateWideBanner("New Features Available"));

bannerTile.SlideShow = true;
bannerTile.SliderSpeed = 300; // Slow for reading
bannerTile.ImageStreamDirection = StreamDirection.LeftToRight;

layoutGroup.Controls.Add(bannerTile);
```

## Common Configurations

### Static Tile (No Animation)

```csharp
ImageStreamer staticTile = new ImageStreamer();
staticTile.Images.Add(appIcon);
staticTile.SlideShow = false; // No animation
staticTile.InternalBackColor = Color.White;
```

**Use case:** App launchers, static icons, single-purpose tiles.

### Auto-Rotating Content

```csharp
ImageStreamer autoTile = new ImageStreamer();
autoTile.Images.Add(content1);
autoTile.Images.Add(content2);
autoTile.Images.Add(content3);
autoTile.SlideShow = true;
autoTile.SliderSpeed = 150;
autoTile.ShowNavigator = false; // Fully automatic
```

**Use case:** Ads, announcements, automatic content rotation.

### User-Controlled Gallery

```csharp
ImageStreamer galleryTile = new ImageStreamer();
foreach (var img in galleryImages)
{
    galleryTile.Images.Add(img);
}
galleryTile.SlideShow = true;
galleryTile.SliderSpeed = 300; // Slow
galleryTile.ShowNavigator = true; // User controls
```

**Use case:** Photo viewers, portfolios, detailed content.

### Dynamic Content Updates

```csharp
// Update images at runtime
void UpdateTileContent(ImageStreamer tile, List<Image> newImages)
{
    tile.Images.Clear();
    foreach (Image img in newImages)
    {
        tile.Images.Add(img);
    }
    tile.Refresh();
}

// Update every 5 minutes
Timer updateTimer = new Timer();
updateTimer.Interval = 300000; // 5 minutes
updateTimer.Tick += (s, e) => UpdateTileContent(newsTile, FetchLatestNews());
updateTimer.Start();
```

**Use case:** Live data feeds, real-time updates, dynamic content.

## Best Practices

1. **Image Optimization:** Use appropriately-sized images (150x150 or 200x200) to avoid memory issues
2. **Slideshow Speed:** Balance between readability and engagement (100-200ms typical)
3. **Consistent Directions:** Use same direction for related tiles in a group
4. **Performance:** Limit Images collection size (5-10 images per tile for memory)
5. **User Control:** Add ShowNavigator for content requiring attention
6. **Error Handling:** Wrap Image.FromFile in try-catch blocks

## Troubleshooting

**Issue:** Images not appearing
- Verify at least one image in Images collection
- Check image file paths are valid
- Ensure images are not null

**Issue:** SlideShow not animating
- Confirm `SlideShow = true`
- Verify multiple images in collection (need 2+ for animation)
- Check SliderSpeed is reasonable value

**Issue:** Navigator not showing
- Set `ShowNavigator = true` AND `SlideShow = true` (both required)
- Ensure control has sufficient size to display arrows

**Issue:** Performance degradation
- Reduce number of tiles with SlideShow enabled
- Optimize image sizes (reduce resolution)
- Limit images per ImageStreamer (5-10 max recommended)

**Issue:** Direction not changing
- Set ImageStreamDirection before enabling SlideShow
- Call `tile.Refresh()` after changing direction

## Summary

ImageStreamer tiles provide powerful live tile functionality:

- **Images Collection:** Add multiple images for rotation
- **SlideShow/SliderSpeed:** Automatic animation control
- **ShowNavigator:** User navigation controls
- **ImageStreamDirection:** 5 animation directions
- **ImageStreamerType:** Single or double-horizontal display

Use these properties to create engaging, animated tiles that bring your TileLayout to life with dynamic content.
