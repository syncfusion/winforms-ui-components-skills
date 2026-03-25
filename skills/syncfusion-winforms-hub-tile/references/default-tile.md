# DefaultTile

## Table of Contents
- [Overview](#overview)
- [Key Properties](#key-properties)
- [Transition Effects](#transition-effects)
- [Transition Speed](#transition-speed)
- [Configuration Examples](#configuration-examples)
- [Common Scenarios](#common-scenarios)
- [Troubleshooting](#troubleshooting)

## Overview

**DefaultTile** provides notifications through various **slide transition effects**, similar to Windows 8 live tiles. Content slides in from different directions at specified intervals, creating a dynamic update display.

**Key capabilities:**
- 4 slide transition directions
- Configurable transition speed
- Title, footer, and image display
- Background customization
- Banner overlay support

**Use when:**
- Displaying sequential content updates
- News feeds or status updates
- Content that flows in a specific direction
- Windows 8-style live tile behavior

## Key Properties

### Title Property

Displays text at the top of the tile.

```csharp
// Set title text
hubTile1.Title.Text = "This is the title area. Display your image here";

// Set title color
hubTile1.Title.TextColor = Color.White;

// Set title font
hubTile1.Title.Font = new Font("Segoe UI", 12, FontStyle.Bold);
```

**Best practices:**
- Keep title concise (1-3 words)
- Use high contrast colors
- Choose readable fonts (Segoe UI, Arial)

### Footer Property

Displays text at the bottom of the tile.

```csharp
// Set footer text
hubTile1.Footer.Text = "HubTile";

// Set footer color
hubTile1.Footer.TextColor = Color.White;

// Set footer font
hubTile1.Footer.Font = new Font("Segoe UI", 9, FontStyle.Regular);
```

**Best practices:**
- Use footer for category or source (e.g., "News", "Weather")
- Smaller font than title
- Consistent styling across tiles

### ImageSource Property

Sets the background image for the tile.

```csharp
// From file
hubTile1.ImageSource = Image.FromFile("background.jpg");

// From resources
hubTile1.ImageSource = Properties.Resources.TileBackground;

// From ImageList
hubTile1.ImageSource = imageListAdv1.Images[0];
```

**Image recommendations:**
- Resolution: 200x200 or higher for clarity
- Format: JPG, PNG (PNG for transparency)
- Content: High contrast for text visibility
- Size: Optimize file size for performance

### Complete Configuration

```csharp
// Create and configure DefaultTile
HubTile tile = new HubTile();
tile.Size = new Size(200, 200);
tile.TileType = HubTileType.DefaultTile;

// Title
tile.Title.Text = "Breaking News";
tile.Title.TextColor = Color.White;

// Footer
tile.Footer.Text = "CNN";
tile.Footer.TextColor = Color.White;

// Background
tile.BackColor = Color.FromArgb(17, 158, 218);
tile.ImageSource = Image.FromFile("news.jpg");

// Enable live tile
tile.TurnLiveTileOn = true;
```

## Transition Effects

DefaultTile supports **four slide transition directions**:

### Bottom-to-Top Transition

Content slides upward from bottom to top.

```csharp
hubTile1.TileType = HubTileType.DefaultTile;
hubTile1.TurnLiveTileOn = true;
hubTile1.SlideTransition = TransitionDirection.BottomToTop;
```

**Use when:**
- News feeds (natural upward flow)
- Status updates moving up
- New content appearing from below

**Visual:** ⬆️ Content enters from bottom, exits to top

### Top-to-Bottom Transition

Content slides downward from top to bottom.

```csharp
hubTile1.SlideTransition = TransitionDirection.TopToBottom;
```

**Use when:**
- Dropdown-style content
- Hierarchical information flow
- Content cascading down

**Visual:** ⬇️ Content enters from top, exits to bottom

### Left-to-Right Transition

Content slides horizontally from left to right.

```csharp
hubTile1.SlideTransition = TransitionDirection.LeftToRight;
```

**Use when:**
- Timeline progression
- Next content indicator
- Forward navigation metaphor
- Languages reading left-to-right

**Visual:** ➡️ Content enters from left, exits to right

### Right-to-Left Transition

Content slides horizontally from right to left.

```csharp
hubTile1.SlideTransition = TransitionDirection.RightToLeft;
```

**Use when:**
- Previous content indicator
- Backward navigation
- Right-to-left languages (Arabic, Hebrew)
- Reverse chronological flow

**Visual:** ⬅️ Content enters from right, exits to left

### Transition Direction Comparison

```csharp
// Create 4 tiles with different transitions
HubTile[] tiles = new HubTile[4];
TransitionDirection[] directions = {
    TransitionDirection.BottomToTop,
    TransitionDirection.TopToBottom,
    TransitionDirection.LeftToRight,
    TransitionDirection.RightToLeft
};

string[] labels = { "Bottom→Top", "Top→Bottom", "Left→Right", "Right→Left" };

for (int i = 0; i < 4; i++)
{
    tiles[i] = new HubTile();
    tiles[i].Size = new Size(150, 150);
    tiles[i].Location = new Point(20 + (i * 160), 20);
    tiles[i].TileType = HubTileType.DefaultTile;
    tiles[i].Title.Text = labels[i];
    tiles[i].Title.TextColor = Color.White;
    tiles[i].BackColor = Color.FromArgb(0, 120, 215);
    tiles[i].TurnLiveTileOn = true;
    tiles[i].SlideTransition = directions[i];
    
    this.Controls.Add(tiles[i]);
}
```

## Transition Speed

Control how fast content transitions using the `ImageTransitionSpeed` property.

### Speed Configuration

```csharp
// Slow transition
hubTile1.ImageTransitionSpeed = 1;  // Slowest

// Medium transition (default)
hubTile1.ImageTransitionSpeed = 3;

// Fast transition
hubTile1.ImageTransitionSpeed = 5;

// Very fast transition
hubTile1.ImageTransitionSpeed = 8;  // Very fast

// Maximum speed
hubTile1.ImageTransitionSpeed = 10; // Fastest
```

**Speed range:** 1-10
- **1-2:** Slow, elegant transitions (3-4 seconds)
- **3-5:** Medium speed (1-2 seconds) - **Recommended**
- **6-8:** Fast transitions (<1 second)
- **9-10:** Very fast, rapid updates

**Choosing speed:**
- **Slow (1-2):** Formal content, professional displays
- **Medium (3-5):** General purpose, balanced
- **Fast (6-10):** Dynamic content, quick updates

### Speed Comparison Example

```csharp
// Create tiles with different speeds
for (int speed = 1; speed <= 5; speed++)
{
    HubTile tile = new HubTile();
    tile.Size = new Size(120, 120);
    tile.Location = new Point(20 + ((speed - 1) * 130), 20);
    tile.TileType = HubTileType.DefaultTile;
    tile.Title.Text = $"Speed {speed}";
    tile.Title.TextColor = Color.White;
    tile.BackColor = Color.FromArgb(0, 120, 215);
    tile.TurnLiveTileOn = true;
    tile.SlideTransition = TransitionDirection.BottomToTop;
    tile.ImageTransitionSpeed = speed;
    
    this.Controls.Add(tile);
}
```

## Configuration Examples

### News Feed Tile

```csharp
HubTile newsTile = new HubTile();
newsTile.Size = new Size(200, 200);
newsTile.TileType = HubTileType.DefaultTile;

// Content
newsTile.Title.Text = "Breaking News";
newsTile.Title.TextColor = Color.White;
newsTile.Footer.Text = "CNN Live";
newsTile.Footer.TextColor = Color.White;

// Colors
newsTile.BackColor = Color.FromArgb(17, 158, 218);

// Image (if available)
// newsTile.ImageSource = Image.FromFile("news.jpg");

// Animation
newsTile.TurnLiveTileOn = true;
newsTile.SlideTransition = TransitionDirection.BottomToTop;
newsTile.ImageTransitionSpeed = 3;

this.Controls.Add(newsTile);
```

### Weather Tile

```csharp
HubTile weatherTile = new HubTile();
weatherTile.Size = new Size(200, 200);
weatherTile.TileType = HubTileType.DefaultTile;

// Content
weatherTile.Title.Text = "Seattle";
weatherTile.Title.TextColor = Color.White;
weatherTile.Footer.Text = "72°F Sunny";
weatherTile.Footer.TextColor = Color.White;

// Colors
weatherTile.BackColor = Color.FromArgb(255, 140, 0);

// Animation
weatherTile.TurnLiveTileOn = true;
weatherTile.SlideTransition = TransitionDirection.LeftToRight;
weatherTile.ImageTransitionSpeed = 4;

this.Controls.Add(weatherTile);
```

### Status Update Tile

```csharp
HubTile statusTile = new HubTile();
statusTile.Size = new Size(200, 200);
statusTile.TileType = HubTileType.DefaultTile;

// Content
statusTile.Title.Text = "System Status";
statusTile.Title.TextColor = Color.White;
statusTile.Footer.Text = "All Systems OK";
statusTile.Footer.TextColor = Color.LightGreen;

// Colors
statusTile.BackColor = Color.FromArgb(16, 137, 62);

// Animation
statusTile.TurnLiveTileOn = true;
statusTile.SlideTransition = TransitionDirection.TopToBottom;
statusTile.ImageTransitionSpeed = 2;

this.Controls.Add(statusTile);
```

### Email Notification Tile

```csharp
HubTile emailTile = new HubTile();
emailTile.Size = new Size(200, 200);
emailTile.TileType = HubTileType.DefaultTile;

// Content
emailTile.Title.Text = "Inbox";
emailTile.Title.TextColor = Color.White;
emailTile.Footer.Text = "5 New Messages";
emailTile.Footer.TextColor = Color.Yellow;

// Colors
emailTile.BackColor = Color.FromArgb(0, 120, 215);

// Animation
emailTile.TurnLiveTileOn = true;
emailTile.SlideTransition = TransitionDirection.RightToLeft;
emailTile.ImageTransitionSpeed = 3;

this.Controls.Add(emailTile);
```

## Common Scenarios

### Scenario 1: Dynamic Content Update

Update tile content periodically:

```csharp
HubTile liveTile = new HubTile();
liveTile.TileType = HubTileType.DefaultTile;
liveTile.TurnLiveTileOn = true;
liveTile.SlideTransition = TransitionDirection.BottomToTop;

// Timer for content updates
Timer updateTimer = new Timer();
updateTimer.Interval = 5000;  // 5 seconds
updateTimer.Tick += (s, e) => {
    // Update content
    liveTile.Title.Text = $"Update {DateTime.Now.ToShortTimeString()}";
    liveTile.Footer.Text = GetLatestStatus();
};
updateTimer.Start();
```

### Scenario 2: Dashboard with Multiple Feeds

```csharp
private void CreateDashboard()
{
    FlowLayoutPanel dashboard = new FlowLayoutPanel();
    dashboard.Dock = DockStyle.Fill;
    dashboard.BackColor = Color.FromArgb(30, 30, 30);
    
    // News tile
    HubTile news = CreateDefaultTile("News", "Latest", Color.FromArgb(17, 158, 218));
    news.SlideTransition = TransitionDirection.BottomToTop;
    dashboard.Controls.Add(news);
    
    // Weather tile
    HubTile weather = CreateDefaultTile("Weather", "72°F", Color.FromArgb(255, 140, 0));
    weather.SlideTransition = TransitionDirection.LeftToRight;
    dashboard.Controls.Add(weather);
    
    // Stocks tile
    HubTile stocks = CreateDefaultTile("Stocks", "+2.5%", Color.FromArgb(16, 137, 62));
    stocks.SlideTransition = TransitionDirection.TopToBottom;
    dashboard.Controls.Add(stocks);
    
    this.Controls.Add(dashboard);
}

private HubTile CreateDefaultTile(string title, string footer, Color color)
{
    HubTile tile = new HubTile();
    tile.Size = new Size(150, 150);
    tile.Margin = new Padding(5);
    tile.TileType = HubTileType.DefaultTile;
    tile.Title.Text = title;
    tile.Title.TextColor = Color.White;
    tile.Footer.Text = footer;
    tile.Footer.TextColor = Color.White;
    tile.BackColor = color;
    tile.TurnLiveTileOn = true;
    tile.ImageTransitionSpeed = 3;
    return tile;
}
```

### Scenario 3: Direction-Based Content Flow

```csharp
// Timeline showing progression
HubTile[] timeline = new HubTile[3];
string[] stages = { "Planning", "Development", "Testing" };

for (int i = 0; i < 3; i++)
{
    timeline[i] = new HubTile();
    timeline[i].Size = new Size(150, 150);
    timeline[i].Location = new Point(20 + (i * 160), 20);
    timeline[i].TileType = HubTileType.DefaultTile;
    timeline[i].Title.Text = stages[i];
    timeline[i].Title.TextColor = Color.White;
    timeline[i].BackColor = Color.FromArgb(0, 120, 215);
    timeline[i].TurnLiveTileOn = true;
    timeline[i].SlideTransition = TransitionDirection.LeftToRight;  // Progressive flow
    timeline[i].ImageTransitionSpeed = 3;
    
    this.Controls.Add(timeline[i]);
}
```

## Troubleshooting

### Issue: Transition not visible

**Causes:**
- `TurnLiveTileOn = false`
- Tile is frozen (`IsFrozen = true`)
- ImageSource not set

**Solutions:**
```csharp
hubTile1.TurnLiveTileOn = true;
hubTile1.IsFrozen = false;
// Transitions work with or without ImageSource, but are more visible with it
```

### Issue: Transition too fast or too slow

**Cause:** Inappropriate ImageTransitionSpeed value

**Solution:**
```csharp
// Adjust speed to preferred rate
hubTile1.ImageTransitionSpeed = 3;  // Recommended: 2-4
```

### Issue: Title/Footer overlapping image

**Cause:** Image too bright or text color low contrast

**Solutions:**
```csharp
// Use semi-transparent overlay
// Or choose high-contrast text colors
hubTile1.Title.TextColor = Color.White;
hubTile1.BackColor = Color.FromArgb(100, 0, 0, 0);  // Semi-transparent black
```

### Issue: Transition direction confusing

**Cause:** Wrong direction for content type

**Solution:**
```csharp
// Match direction to content flow
// News/updates: BottomToTop (natural upward flow)
hubTile1.SlideTransition = TransitionDirection.BottomToTop;

// Timeline/progression: LeftToRight
hubTile2.SlideTransition = TransitionDirection.LeftToRight;
```

## Best Practices

1. **Match transition to content type** - BottomToTop for feeds, LeftToRight for timelines
2. **Use medium speed** - ImageTransitionSpeed 3-4 for balanced animations
3. **High contrast text** - White text on dark backgrounds or vice versa
4. **Consistent direction** - Use same direction for related tiles
5. **Enable selectively** - Not all tiles need to animate simultaneously
6. **Test visibility** - Ensure transitions are noticeable but not jarring
7. **Optimize images** - Use appropriate resolution and file size
8. **Update content** - Change Title/Footer periodically for live feel
