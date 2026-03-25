# RotateTile

## Overview

**RotateTile** provides notifications through **rotation or flip transitions**, creating a flipping animation similar to Windows 8/Windows Phone style tiles. Content rotates around horizontal or vertical axes, ideal for displaying two states or alternate information.

**Key capabilities:**
- Horizontal rotation (flip left/right)
- Vertical rotation (flip top/bottom)
- Configurable rotation speed
- Dual-state content display
- Seamless transition effects

**Use when:**
- Showing front/back content (e.g., photo/details)
- Toggling between two states
- Creating flipbook-style animations
- Displaying alternate information views
- Windows Phone-style tile behavior

## Key Properties

### RotationTransition Property

Specifies the rotation direction:

```csharp
// Horizontal flip (rotate around vertical axis)
hubTile1.RotationTransition = TileFlipDirection.Horizontal;

// Vertical flip (rotate around horizontal axis)
hubTile2.RotationTransition = TileFlipDirection.Vertical;
```

**TileFlipDirection enum:**
- `Horizontal` - Flips left-to-right or right-to-left
- `Vertical` - Flips top-to-bottom or bottom-to-top

### RotationTransitionSpeed Property

Controls how fast the tile rotates:

```csharp
// Slow rotation
hubTile1.RotationTransitionSpeed = 1;  // Slowest

// Medium rotation (default)
hubTile1.RotationTransitionSpeed = 3;  // Recommended

// Fast rotation
hubTile1.RotationTransitionSpeed = 5;

// Very fast rotation
hubTile1.RotationTransitionSpeed = 10; // Fastest
```

**Speed range:** 1-10
- **1-2:** Slow, dramatic rotation (3-4 seconds)
- **3-5:** Medium speed (1-2 seconds) - **Recommended**
- **6-10:** Fast rotation (<1 second)

## Basic Configuration

### Horizontal Rotation

Tile flips around vertical axis (side-to-side):

```csharp
HubTile tile = new HubTile();
tile.Size = new Size(200, 200);
tile.TileType = HubTileType.RotateTile;

// Configure rotation
tile.TurnLiveTileOn = true;
tile.RotationTransition = TileFlipDirection.Horizontal;
tile.RotationTransitionSpeed = 3;

// Content
tile.Title.Text = "Horizontal Flip";
tile.Title.TextColor = Color.White;
tile.Footer.Text = "Flipping...";
tile.Footer.TextColor = Color.White;
tile.BackColor = Color.FromArgb(139, 0, 139);

this.Controls.Add(tile);
```

**Visual effect:** 🔄 Tile rotates side-to-side like turning a book page

**Use for:**
- Photo front, details back
- Question front, answer back
- Card flip effects
- Side-by-side comparisons

### Vertical Rotation

Tile flips around horizontal axis (top-to-bottom):

```csharp
HubTile tile = new HubTile();
tile.Size = new Size(200, 200);
tile.TileType = HubTileType.RotateTile;

// Configure rotation
tile.TurnLiveTileOn = true;
tile.RotationTransition = TileFlipDirection.Vertical;
tile.RotationTransitionSpeed = 3;

// Content
tile.Title.Text = "Vertical Flip";
tile.Title.TextColor = Color.White;
tile.Footer.Text = "Rotating...";
tile.Footer.TextColor = Color.White;
tile.BackColor = Color.FromArgb(0, 120, 215);

this.Controls.Add(tile);
```

**Visual effect:** 🔃 Tile rotates top-to-bottom like flipping a calendar page

**Use for:**
- Header top, content bottom
- Calendar page flips
- Countdown effects
- Vertical content transitions

## Configuration Examples

### Photo Tile with Horizontal Flip

```csharp
HubTile photoTile = new HubTile();
photoTile.Size = new Size(200, 200);
photoTile.TileType = HubTileType.RotateTile;

// Image and content
photoTile.Title.Text = "Photos";
photoTile.Title.TextColor = Color.White;
photoTile.Footer.Text = "124 Images";
photoTile.Footer.TextColor = Color.White;
photoTile.BackColor = Color.FromArgb(139, 0, 139);

// Add image (if available)
// photoTile.ImageSource = Image.FromFile("photo.jpg");

// Horizontal rotation
photoTile.TurnLiveTileOn = true;
photoTile.RotationTransition = TileFlipDirection.Horizontal;
photoTile.RotationTransitionSpeed = 4;

this.Controls.Add(photoTile);
```

### Weather Tile with Vertical Flip

```csharp
HubTile weatherTile = new HubTile();
weatherTile.Size = new Size(200, 200);
weatherTile.TileType = HubTileType.RotateTile;

// Content
weatherTile.Title.Text = "Seattle";
weatherTile.Title.TextColor = Color.White;
weatherTile.Footer.Text = "72°F Sunny";
weatherTile.Footer.TextColor = Color.White;
weatherTile.BackColor = Color.FromArgb(255, 140, 0);

// Vertical rotation
weatherTile.TurnLiveTileOn = true;
weatherTile.RotationTransition = TileFlipDirection.Vertical;
weatherTile.RotationTransitionSpeed = 3;

this.Controls.Add(weatherTile);
```

### Calendar Tile

```csharp
HubTile calendarTile = new HubTile();
calendarTile.Size = new Size(200, 200);
calendarTile.TileType = HubTileType.RotateTile;

// Date content
calendarTile.Title.Text = DateTime.Now.ToString("MMM dd");
calendarTile.Title.TextColor = Color.White;
calendarTile.Title.Font = new Font("Segoe UI", 16, FontStyle.Bold);
calendarTile.Footer.Text = DateTime.Now.ToString("dddd");
calendarTile.Footer.TextColor = Color.White;
calendarTile.BackColor = Color.FromArgb(0, 120, 215);

// Vertical flip for calendar page effect
calendarTile.TurnLiveTileOn = true;
calendarTile.RotationTransition = TileFlipDirection.Vertical;
calendarTile.RotationTransitionSpeed = 2;  // Slower for elegance

this.Controls.Add(calendarTile);
```

### Q&A Flashcard Tile

```csharp
HubTile flashcardTile = new HubTile();
flashcardTile.Size = new Size(200, 200);
flashcardTile.TileType = HubTileType.RotateTile;

// Question/Answer
flashcardTile.Title.Text = "What is C#?";
flashcardTile.Title.TextColor = Color.White;
flashcardTile.Footer.Text = "Click to reveal";
flashcardTile.Footer.TextColor = Color.Yellow;
flashcardTile.BackColor = Color.FromArgb(16, 137, 62);

// Horizontal flip for card effect
flashcardTile.TurnLiveTileOn = true;
flashcardTile.RotationTransition = TileFlipDirection.Horizontal;
flashcardTile.RotationTransitionSpeed = 3;

this.Controls.Add(flashcardTile);
```

## Rotation Speed Comparison

### Creating Speed Comparison Demo

```csharp
private void CreateRotationSpeedDemo()
{
    for (int speed = 1; speed <= 5; speed++)
    {
        // Create tile with specific speed
        HubTile tile = new HubTile();
        tile.Size = new Size(120, 120);
        tile.Location = new Point(20 + ((speed - 1) * 130), 20);
        tile.TileType = HubTileType.RotateTile;
        
        // Content
        tile.Title.Text = $"Speed {speed}";
        tile.Title.TextColor = Color.White;
        tile.Footer.Text = "Rotating";
        tile.Footer.TextColor = Color.White;
        tile.BackColor = Color.FromArgb(139, 0, 139);
        
        // Rotation
        tile.TurnLiveTileOn = true;
        tile.RotationTransition = TileFlipDirection.Horizontal;
        tile.RotationTransitionSpeed = speed;
        
        this.Controls.Add(tile);
    }
}
```

## Common Scenarios

### Scenario 1: Photo Gallery Tiles

```csharp
private void CreatePhotoGallery()
{
    FlowLayoutPanel gallery = new FlowLayoutPanel();
    gallery.Dock = DockStyle.Fill;
    gallery.BackColor = Color.FromArgb(30, 30, 30);
    gallery.Padding = new Padding(10);
    
    // Create multiple photo tiles
    string[] categories = { "Vacation", "Family", "Pets", "Events" };
    Color[] colors = {
        Color.FromArgb(139, 0, 139),
        Color.FromArgb(0, 120, 215),
        Color.FromArgb(255, 140, 0),
        Color.FromArgb(16, 137, 62)
    };
    
    for (int i = 0; i < categories.Length; i++)
    {
        HubTile photoTile = new HubTile();
        photoTile.Size = new Size(150, 150);
        photoTile.Margin = new Padding(5);
        photoTile.TileType = HubTileType.RotateTile;
        photoTile.Title.Text = categories[i];
        photoTile.Title.TextColor = Color.White;
        photoTile.Footer.Text = $"{(i + 1) * 20} Photos";
        photoTile.Footer.TextColor = Color.White;
        photoTile.BackColor = colors[i];
        photoTile.TurnLiveTileOn = true;
        photoTile.RotationTransition = TileFlipDirection.Horizontal;
        photoTile.RotationTransitionSpeed = 3;
        
        gallery.Controls.Add(photoTile);
    }
    
    this.Controls.Add(gallery);
}
```

### Scenario 2: Mixed Rotation Directions

```csharp
private void CreateMixedRotations()
{
    // Horizontal rotation tile
    HubTile hTile = new HubTile();
    hTile.Size = new Size(150, 150);
    hTile.Location = new Point(20, 20);
    hTile.TileType = HubTileType.RotateTile;
    hTile.Title.Text = "Horizontal";
    hTile.Title.TextColor = Color.White;
    hTile.BackColor = Color.FromArgb(139, 0, 139);
    hTile.TurnLiveTileOn = true;
    hTile.RotationTransition = TileFlipDirection.Horizontal;
    hTile.RotationTransitionSpeed = 3;
    this.Controls.Add(hTile);
    
    // Vertical rotation tile
    HubTile vTile = new HubTile();
    vTile.Size = new Size(150, 150);
    vTile.Location = new Point(180, 20);
    vTile.TileType = HubTileType.RotateTile;
    vTile.Title.Text = "Vertical";
    vTile.Title.TextColor = Color.White;
    vTile.BackColor = Color.FromArgb(0, 120, 215);
    vTile.TurnLiveTileOn = true;
    vTile.RotationTransition = TileFlipDirection.Vertical;
    vTile.RotationTransitionSpeed = 3;
    this.Controls.Add(vTile);
}
```

### Scenario 3: Dynamic Content Rotation

```csharp
HubTile infoTile = new HubTile();
infoTile.TileType = HubTileType.RotateTile;
infoTile.TurnLiveTileOn = true;
infoTile.RotationTransition = TileFlipDirection.Horizontal;

// Timer to update content during rotation
string[] messages = { "Message 1", "Message 2", "Message 3" };
int currentIndex = 0;

Timer rotateTimer = new Timer();
rotateTimer.Interval = 3000;  // Match rotation speed
rotateTimer.Tick += (s, e) => {
    infoTile.Title.Text = messages[currentIndex];
    currentIndex = (currentIndex + 1) % messages.Length;
};
rotateTimer.Start();
```

### Scenario 4: Interactive Flip Control

```csharp
HubTile controlledTile = new HubTile();
controlledTile.TileType = HubTileType.RotateTile;
controlledTile.RotationTransition = TileFlipDirection.Horizontal;
controlledTile.TurnLiveTileOn = false;  // Manual control

// Button to trigger rotation
Button flipButton = new Button();
flipButton.Text = "Flip Tile";
flipButton.Click += (s, e) => {
    // Temporarily enable for one rotation
    controlledTile.TurnLiveTileOn = true;
    
    // Update content
    controlledTile.Title.Text = DateTime.Now.ToShortTimeString();
    
    // Disable after delay
    Timer stopTimer = new Timer();
    stopTimer.Interval = 1500;  // Rotation duration
    stopTimer.Tick += (sender, args) => {
        controlledTile.TurnLiveTileOn = false;
        stopTimer.Stop();
    };
    stopTimer.Start();
};
```

## Choosing Rotation Direction

### Horizontal vs Vertical

**Use Horizontal when:**
- Content represents "sides" (front/back)
- Card flip metaphor
- Photo galleries
- Question/answer displays
- Side-by-side comparisons
- Book page turning effect

**Use Vertical when:**
- Content flows top-to-bottom
- Calendar page flips
- Header/content separation
- Vertical content progression
- Countdown timers
- Stack or layer metaphor

### Direction Comparison Table

| Direction | Visual Metaphor | Best Use Cases |
|-----------|----------------|----------------|
| Horizontal | Book page, card | Photos, Q&A, dual states |
| Vertical | Calendar page, stack | Dates, headers, countdowns |

## Troubleshooting

### Issue: Rotation not occurring

**Causes:**
- `TurnLiveTileOn = false`
- Tile is frozen (`IsFrozen = true`)
- RotationTransition not set

**Solutions:**
```csharp
hubTile1.TurnLiveTileOn = true;
hubTile1.IsFrozen = false;
hubTile1.RotationTransition = TileFlipDirection.Horizontal;
```

### Issue: Rotation too fast to see content

**Cause:** RotationTransitionSpeed too high

**Solution:**
```csharp
// Slow down rotation
hubTile1.RotationTransitionSpeed = 2;  // Slower, more visible
```

### Issue: Content appears distorted during rotation

**Cause:** Normal 3D perspective effect during rotation

**Solution:** This is expected behavior. Ensure content is readable when tile is facing forward:
```csharp
// Use larger, bold fonts
hubTile1.Title.Font = new Font("Segoe UI", 14, FontStyle.Bold);

// High contrast colors
hubTile1.Title.TextColor = Color.White;
hubTile1.BackColor = Color.FromArgb(0, 0, 0);
```

### Issue: Rotation direction unclear

**Cause:** Wrong direction for content type

**Solution:**
```csharp
// Match direction to content metaphor
// For card flip: Horizontal
hubTile1.RotationTransition = TileFlipDirection.Horizontal;

// For calendar flip: Vertical
hubTile2.RotationTransition = TileFlipDirection.Vertical;
```

## Best Practices

1. **Match direction to content** - Horizontal for cards/photos, Vertical for calendars/lists
2. **Use medium speed** - RotationTransitionSpeed 3-4 for visibility
3. **Bold, large text** - Text should be readable during brief face-forward moments
4. **High contrast** - Ensure text stands out during rotation
5. **Consistent speed** - Use same speed for related tiles
6. **Consider dual-state content** - Design content for "front" and "back" views
7. **Test visibility** - Verify content is readable during rotation cycle
8. **Update during face-forward** - Change content when tile faces user for clarity
