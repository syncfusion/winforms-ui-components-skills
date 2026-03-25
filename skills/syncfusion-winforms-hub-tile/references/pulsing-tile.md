# PulsingTile

## Overview

**PulsingTile** creates attention-grabbing **zoom in/out effects** similar to Windows Phone Music and Video tiles. The tile pulses by scaling up and down continuously, creating a breathing or pulsating animation ideal for media content and notifications.

**Key capabilities:**
- Smooth zoom in/out animation
- Configurable pulse duration
- Adjustable scale factor
- Continuous pulsing effect
- Media-focused design

**Use when:**
- Media libraries (music, video, photos)
- Attention-grabbing notifications
- Active status indicators
- Promotional content
- Call-to-action tiles
- Live activity displays

## Key Properties

### PulseDuration Property

Controls the time interval (in seconds) between pulse animations.

```csharp
// Fast pulsing (1 second)
hubTile1.PulseDuration = 1;

// Medium pulsing (2 seconds) - Recommended
hubTile1.PulseDuration = 2;

// Slow pulsing (4 seconds)
hubTile1.PulseDuration = 4;

// Very slow (6 seconds)
hubTile1.PulseDuration = 6;
```

**Duration range:** 1-10 seconds
- **1-2 seconds:** Fast, energetic pulsing
- **2-4 seconds:** Medium, balanced (Recommended)
- **4-6 seconds:** Slow, subtle breathing effect
- **6-10 seconds:** Very slow, minimal motion

**Choosing duration:**
- **Fast (1-2s):** Urgent notifications, active status
- **Medium (2-4s):** General media content, promotions
- **Slow (4-6s):** Ambient effects, background tiles

### PulseScale Property

Sets how much the tile scales during pulsing.

```csharp
// Subtle pulse (10% larger)
hubTile1.PulseScale = 1.1f;

// Medium pulse (30% larger) - Recommended
hubTile1.PulseScale = 1.3f;

// Strong pulse (50% larger)
hubTile1.PulseScale = 1.5f;

// Maximum pulse (100% larger - doubles size)
hubTile1.PulseScale = 2.0f;
```

**Scale range:** 1.0 - 2.0
- **1.0:** No scaling (no pulse)
- **1.1-1.2:** Subtle (10-20% larger)
- **1.3-1.5:** Medium (30-50% larger) - **Recommended**
- **1.6-2.0:** Strong (60-100% larger)

**Choosing scale:**
- **Subtle (1.1-1.2):** Professional displays, minimal distraction
- **Medium (1.3-1.5):** Balanced attention, media content
- **Strong (1.6-2.0):** High urgency, promotional content

## Basic Configuration

### Simple Pulsing Tile

```csharp
HubTile tile = new HubTile();
tile.Size = new Size(200, 200);
tile.TileType = HubTileType.PulsingTile;

// Content
tile.Title.Text = "Music";
tile.Title.TextColor = Color.White;
tile.Footer.Text = "Now Playing";
tile.Footer.TextColor = Color.White;
tile.BackColor = Color.FromArgb(139, 0, 139);

// Pulsing effect
tile.TurnLiveTileOn = true;
tile.PulseDuration = 2;
tile.PulseScale = 1.3f;

this.Controls.Add(tile);
```

**Result:** Tile pulses every 2 seconds, scaling to 130% size

### Complete Configuration

```csharp
// Create PulsingTile
HubTile pulsingTile = new HubTile();
pulsingTile.Size = new Size(200, 200);
pulsingTile.Location = new Point(20, 20);
pulsingTile.TileType = HubTileType.PulsingTile;

// Title
pulsingTile.Title.Text = "Media Library";
pulsingTile.Title.TextColor = Color.White;
pulsingTile.Title.Font = new Font("Segoe UI", 12, FontStyle.Bold);

// Footer
pulsingTile.Footer.Text = "250 Songs";
pulsingTile.Footer.TextColor = Color.White;

// Background
pulsingTile.BackColor = Color.FromArgb(139, 0, 139);

// Image (optional)
// pulsingTile.ImageSource = Image.FromFile("music.jpg");

// Pulse configuration
pulsingTile.TurnLiveTileOn = true;
pulsingTile.PulseDuration = 2;
pulsingTile.PulseScale = 1.4f;

this.Controls.Add(pulsingTile);
```

## Configuration Examples

### Music Tile

```csharp
HubTile musicTile = new HubTile();
musicTile.Size = new Size(200, 200);
musicTile.TileType = HubTileType.PulsingTile;

// Content
musicTile.Title.Text = "Music";
musicTile.Title.TextColor = Color.White;
musicTile.Footer.Text = "Now Playing";
musicTile.Footer.TextColor = Color.White;
musicTile.BackColor = Color.FromArgb(139, 0, 139);

// Image (if available)
// musicTile.ImageSource = Image.FromFile("album_art.jpg");

// Pulse effect
musicTile.TurnLiveTileOn = true;
musicTile.PulseDuration = 2;
musicTile.PulseScale = 1.3f;

this.Controls.Add(musicTile);
```

### Video Tile

```csharp
HubTile videoTile = new HubTile();
videoTile.Size = new Size(200, 200);
videoTile.TileType = HubTileType.PulsingTile;

// Content
videoTile.Title.Text = "Videos";
videoTile.Title.TextColor = Color.White;
videoTile.Footer.Text = "42 Videos";
videoTile.Footer.TextColor = Color.White;
videoTile.BackColor = Color.FromArgb(255, 140, 0);

// Pulse effect
videoTile.TurnLiveTileOn = true;
videoTile.PulseDuration = 3;
videoTile.PulseScale = 1.4f;

this.Controls.Add(videoTile);
```

### Notification Tile

```csharp
HubTile notificationTile = new HubTile();
notificationTile.Size = new Size(200, 200);
notificationTile.TileType = HubTileType.PulsingTile;

// Content
notificationTile.Title.Text = "Alert!";
notificationTile.Title.TextColor = Color.Yellow;
notificationTile.Footer.Text = "New Message";
notificationTile.Footer.TextColor = Color.White;
notificationTile.BackColor = Color.FromArgb(255, 0, 0);

// Urgent pulse
notificationTile.TurnLiveTileOn = true;
notificationTile.PulseDuration = 1;  // Fast
notificationTile.PulseScale = 1.5f;  // Strong

this.Controls.Add(notificationTile);
```

### Photo Gallery Tile

```csharp
HubTile photoTile = new HubTile();
photoTile.Size = new Size(200, 200);
photoTile.TileType = HubTileType.PulsingTile;

// Content
photoTile.Title.Text = "Photos";
photoTile.Title.TextColor = Color.White;
photoTile.Footer.Text = "1,234 Images";
photoTile.Footer.TextColor = Color.White;
photoTile.BackColor = Color.FromArgb(0, 120, 215);

// Image (if available)
// photoTile.ImageSource = Image.FromFile("gallery.jpg");

// Subtle pulse
photoTile.TurnLiveTileOn = true;
photoTile.PulseDuration = 3;
photoTile.PulseScale = 1.2f;  // Subtle

this.Controls.Add(photoTile);
```

## Pulse Intensity Comparison

### Creating Comparison Demo

```csharp
private void CreatePulseComparison()
{
    float[] scales = { 1.1f, 1.2f, 1.3f, 1.5f, 1.8f };
    string[] labels = { "Subtle", "Mild", "Medium", "Strong", "Maximum" };
    
    for (int i = 0; i < scales.Length; i++)
    {
        HubTile tile = new HubTile();
        tile.Size = new Size(120, 120);
        tile.Location = new Point(20 + (i * 130), 20);
        tile.TileType = HubTileType.PulsingTile;
        tile.Title.Text = labels[i];
        tile.Title.TextColor = Color.White;
        tile.Footer.Text = $"{scales[i]}x";
        tile.Footer.TextColor = Color.White;
        tile.BackColor = Color.FromArgb(139, 0, 139);
        tile.TurnLiveTileOn = true;
        tile.PulseDuration = 2;
        tile.PulseScale = scales[i];
        
        this.Controls.Add(tile);
    }
}
```

### Duration Comparison Demo

```csharp
private void CreateDurationComparison()
{
    int[] durations = { 1, 2, 3, 4, 6 };
    string[] labels = { "Fast", "Medium-Fast", "Medium", "Slow", "Very Slow" };
    
    for (int i = 0; i < durations.Length; i++)
    {
        HubTile tile = new HubTile();
        tile.Size = new Size(120, 120);
        tile.Location = new Point(20 + (i * 130), 20);
        tile.TileType = HubTileType.PulsingTile;
        tile.Title.Text = labels[i];
        tile.Title.TextColor = Color.White;
        tile.Footer.Text = $"{durations[i]}s";
        tile.Footer.TextColor = Color.White;
        tile.BackColor = Color.FromArgb(0, 120, 215);
        tile.TurnLiveTileOn = true;
        tile.PulseDuration = durations[i];
        tile.PulseScale = 1.3f;
        
        this.Controls.Add(tile);
    }
}
```

## Common Scenarios

### Scenario 1: Media Dashboard

```csharp
private void CreateMediaDashboard()
{
    FlowLayoutPanel mediaPanel = new FlowLayoutPanel();
    mediaPanel.Dock = DockStyle.Fill;
    mediaPanel.BackColor = Color.FromArgb(30, 30, 30);
    mediaPanel.Padding = new Padding(10);
    
    // Music tile
    HubTile music = CreatePulsingTile("Music", "250 Songs", Color.FromArgb(139, 0, 139));
    music.PulseScale = 1.3f;
    mediaPanel.Controls.Add(music);
    
    // Video tile
    HubTile videos = CreatePulsingTile("Videos", "42 Videos", Color.FromArgb(255, 140, 0));
    videos.PulseScale = 1.4f;
    mediaPanel.Controls.Add(videos);
    
    // Photos tile
    HubTile photos = CreatePulsingTile("Photos", "1,234 Images", Color.FromArgb(0, 120, 215));
    photos.PulseScale = 1.2f;
    mediaPanel.Controls.Add(photos);
    
    this.Controls.Add(mediaPanel);
}

private HubTile CreatePulsingTile(string title, string footer, Color color)
{
    HubTile tile = new HubTile();
    tile.Size = new Size(150, 150);
    tile.Margin = new Padding(5);
    tile.TileType = HubTileType.PulsingTile;
    tile.Title.Text = title;
    tile.Title.TextColor = Color.White;
    tile.Footer.Text = footer;
    tile.Footer.TextColor = Color.White;
    tile.BackColor = color;
    tile.TurnLiveTileOn = true;
    tile.PulseDuration = 2;
    return tile;
}
```

### Scenario 2: Urgent Notification

```csharp
HubTile urgentTile = new HubTile();
urgentTile.TileType = HubTileType.PulsingTile;
urgentTile.Title.Text = "Alert!";
urgentTile.Title.TextColor = Color.Yellow;
urgentTile.Footer.Text = "Action Required";
urgentTile.Footer.TextColor = Color.White;
urgentTile.BackColor = Color.FromArgb(255, 0, 0);

// Urgent pulsing: fast and strong
urgentTile.TurnLiveTileOn = true;
urgentTile.PulseDuration = 1;  // Very fast
urgentTile.PulseScale = 1.5f;  // Strong scale
```

### Scenario 3: Active Status Indicator

```csharp
HubTile statusTile = new HubTile();
statusTile.TileType = HubTileType.PulsingTile;
statusTile.Title.Text = "Online";
statusTile.Title.TextColor = Color.LightGreen;
statusTile.Footer.Text = "Active Now";
statusTile.Footer.TextColor = Color.White;
statusTile.BackColor = Color.FromArgb(16, 137, 62);

// Breathing effect: slow and subtle
statusTile.TurnLiveTileOn = true;
statusTile.PulseDuration = 4;  // Slow
statusTile.PulseScale = 1.15f;  // Very subtle
```

### Scenario 4: Promotional Content

```csharp
HubTile promoTile = new HubTile();
promoTile.Size = new Size(200, 200);
promoTile.TileType = HubTileType.PulsingTile;

// Promotional content
promoTile.Title.Text = "50% OFF!";
promoTile.Title.TextColor = Color.Yellow;
promoTile.Title.Font = new Font("Segoe UI", 18, FontStyle.Bold);
promoTile.Footer.Text = "Limited Time";
promoTile.Footer.TextColor = Color.White;
promoTile.BackColor = Color.FromArgb(255, 0, 0);

// Eye-catching pulse
promoTile.TurnLiveTileOn = true;
promoTile.PulseDuration = 2;
promoTile.PulseScale = 1.4f;

this.Controls.Add(promoTile);
```

### Scenario 5: Dynamic Content Update

```csharp
HubTile dynamicTile = new HubTile();
dynamicTile.TileType = HubTileType.PulsingTile;
dynamicTile.TurnLiveTileOn = true;
dynamicTile.PulseDuration = 2;
dynamicTile.PulseScale = 1.3f;

// Update content periodically
Timer updateTimer = new Timer();
updateTimer.Interval = 5000;
updateTimer.Tick += (s, e) => {
    // Update tile content
    dynamicTile.Title.Text = $"Update {DateTime.Now.ToShortTimeString()}";
    dynamicTile.Footer.Text = GetLatestData();
};
updateTimer.Start();
```

## Choosing Pulse Settings

### Scale Selection Guide

| Scale | Visibility | Best Use Cases |
|-------|-----------|----------------|
| 1.1-1.2 | Subtle | Background tiles, professional displays |
| 1.3-1.4 | Medium | Media content, balanced attention |
| 1.5-1.7 | Strong | Notifications, promotions, urgent items |
| 1.8-2.0 | Maximum | Critical alerts, high priority |

### Duration Selection Guide

| Duration | Frequency | Best Use Cases |
|----------|-----------|----------------|
| 1-2s | Fast | Urgent notifications, active indicators |
| 2-3s | Medium | Media tiles, general content |
| 3-4s | Slow | Ambient effects, subtle attention |
| 4-6s | Very Slow | Background tiles, breathing effects |

### Combining Scale and Duration

**High urgency:** Fast duration (1-2s) + Strong scale (1.5-1.7)
```csharp
urgentTile.PulseDuration = 1;
urgentTile.PulseScale = 1.6f;
```

**Balanced media:** Medium duration (2-3s) + Medium scale (1.3-1.4)
```csharp
mediaTile.PulseDuration = 2;
mediaTile.PulseScale = 1.3f;
```

**Subtle ambient:** Slow duration (4-6s) + Subtle scale (1.1-1.2)
```csharp
ambientTile.PulseDuration = 5;
ambientTile.PulseScale = 1.15f;
```

## Troubleshooting

### Issue: Pulsing not visible

**Causes:**
- `TurnLiveTileOn = false`
- Tile is frozen (`IsFrozen = true`)
- PulseScale too close to 1.0

**Solutions:**
```csharp
hubTile1.TurnLiveTileOn = true;
hubTile1.IsFrozen = false;
hubTile1.PulseScale = 1.3f;  // At least 1.2 for visibility
```

### Issue: Pulsing too fast or distracting

**Cause:** Short PulseDuration or high PulseScale

**Solution:**
```csharp
// Slow down and reduce intensity
hubTile1.PulseDuration = 4;  // Slower
hubTile1.PulseScale = 1.2f;  // More subtle
```

### Issue: Tile appears to "jump" during pulse

**Cause:** PulseScale too high (>1.7) can look jarring

**Solution:**
```csharp
// Use moderate scale
hubTile1.PulseScale = 1.4f;  // Smooth appearance
```

### Issue: Pulsing conflicts with nearby tiles

**Cause:** Multiple tiles pulsing at same rate creates visual chaos

**Solution:**
```csharp
// Stagger pulse timings
tile1.PulseDuration = 2;
tile2.PulseDuration = 3;  // Offset
tile3.PulseDuration = 2.5;  // Different timing
```

### Issue: Content difficult to read during pulse

**Cause:** Scaling effect makes text briefly blurry

**Solution:**
```csharp
// Use larger, bold text
hubTile1.Title.Font = new Font("Segoe UI", 14, FontStyle.Bold);

// High contrast
hubTile1.Title.TextColor = Color.White;
hubTile1.BackColor = Color.Black;

// Reduce scale for readability
hubTile1.PulseScale = 1.2f;  // Less distortion
```

## Best Practices

1. **Match intensity to urgency** - Fast/strong for alerts, slow/subtle for ambient
2. **Use for media content** - Perfect for music, video, photo tiles
3. **Avoid overuse** - Too many pulsing tiles create visual chaos
4. **Moderate scale** - 1.3-1.4 provides good balance
5. **Stagger timings** - Offset PulseDuration for adjacent tiles
6. **Bold, large text** - Ensures readability during scaling
7. **High contrast** - Text should stand out during pulse
8. **Test visibility** - Verify pulse is noticeable but not jarring
9. **Consider context** - Appropriate for media/promotional, not professional documents
10. **Combine with images** - PulsingTile works beautifully with background images
