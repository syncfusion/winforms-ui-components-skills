# Freezing Control

## Overview

The **IsFrozen property** allows you to pause all tile animations, freezing the tile in its current state. This provides control over when animations play, useful for performance optimization, user preferences, or interactive scenarios.

**Key capability:**
- Pause/resume animations dynamically
- Maintain tile appearance while frozen
- Control multiple tiles simultaneously
- Optimize performance

**Use when:**
- User preference to disable animations
- Performance optimization (pause off-screen tiles)
- Interactive scenarios (freeze during editing)
- Conditional animation (freeze based on state)
- Battery-saving mode

## IsFrozen Property

### Basic Usage

```csharp
// Freeze tile (pause animation)
hubTile1.IsFrozen = true;

// Unfreeze tile (resume animation)
hubTile1.IsFrozen = false;
```

**Property values:**
- `true` - Animation paused, tile frozen in current state
- `false` - Animation active (if TurnLiveTileOn = true)

**Important:** `IsFrozen` only affects tiles where `TurnLiveTileOn = true`. Non-animated tiles are unaffected.

## Configuration Examples

### Freeze on Creation

```csharp
HubTile tile = new HubTile();
tile.Size = new Size(200, 200);
tile.TileType = HubTileType.DefaultTile;
tile.BackColor = Color.FromArgb(0, 120, 215);

// Enable animation
tile.TurnLiveTileOn = true;
tile.SlideTransition = TransitionDirection.BottomToTop;

// Initially frozen
tile.IsFrozen = true;

this.Controls.Add(tile);

// Unfreeze after 3 seconds
Timer unfreezeTimer = new Timer();
unfreezeTimer.Interval = 3000;
unfreezeTimer.Tick += (s, e) => {
    tile.IsFrozen = false;
    unfreezeTimer.Stop();
};
unfreezeTimer.Start();
```

### Toggle Freeze on Click

```csharp
HubTile interactiveTile = new HubTile();
interactiveTile.TileType = HubTileType.RotateTile;
interactiveTile.TurnLiveTileOn = true;
interactiveTile.RotationTransition = TileFlipDirection.Horizontal;

// Toggle freeze on click
interactiveTile.Click += (s, e) => {
    interactiveTile.IsFrozen = !interactiveTile.IsFrozen;
    
    // Update UI to show state
    if (interactiveTile.IsFrozen)
    {
        interactiveTile.Footer.Text = "Paused";
        interactiveTile.Footer.TextColor = Color.Yellow;
    }
    else
    {
        interactiveTile.Footer.Text = "Playing";
        interactiveTile.Footer.TextColor = Color.White;
    }
};

this.Controls.Add(interactiveTile);
```

### Freeze with Button Control

```csharp
HubTile controlledTile = new HubTile();
controlledTile.TileType = HubTileType.PulsingTile;
controlledTile.TurnLiveTileOn = true;
controlledTile.PulseDuration = 2;
controlledTile.PulseScale = 1.3f;

// Freeze button
Button freezeButton = new Button();
freezeButton.Text = "Pause Animation";
freezeButton.Click += (s, e) => {
    controlledTile.IsFrozen = true;
    freezeButton.Text = "Resume Animation";
};

// Unfreeze button
Button unfreezeButton = new Button();
unfreezeButton.Text = "Resume Animation";
unfreezeButton.Click += (s, e) => {
    controlledTile.IsFrozen = false;
    freezeButton.Text = "Pause Animation";
};
```

## Common Scenarios

### Scenario 1: User Preference - Disable All Animations

```csharp
List<HubTile> allTiles = new List<HubTile>();

private void DisableAnimations()
{
    foreach (HubTile tile in allTiles)
    {
        tile.IsFrozen = true;
    }
}

private void EnableAnimations()
{
    foreach (HubTile tile in allTiles)
    {
        tile.IsFrozen = false;
    }
}

// Settings checkbox
CheckBox animationToggle = new CheckBox();
animationToggle.Text = "Enable Tile Animations";
animationToggle.Checked = true;
animationToggle.CheckedChanged += (s, e) => {
    if (animationToggle.Checked)
        EnableAnimations();
    else
        DisableAnimations();
};
```

### Scenario 2: Performance - Freeze Off-Screen Tiles

```csharp
FlowLayoutPanel tilePanel = new FlowLayoutPanel();
tilePanel.Scroll += (s, e) => {
    // Freeze tiles not in viewport
    foreach (Control control in tilePanel.Controls)
    {
        if (control is HubTile tile)
        {
            // Check if tile is visible in viewport
            bool isVisible = tilePanel.ClientRectangle.IntersectsWith(tile.Bounds);
            tile.IsFrozen = !isVisible;
        }
    }
};
```

### Scenario 3: Conditional Freeze - Based on Application State

```csharp
HubTile statusTile = new HubTile();
statusTile.TurnLiveTileOn = true;

// Freeze when application loses focus
this.Deactivate += (s, e) => {
    statusTile.IsFrozen = true;
};

this.Activated += (s, e) => {
    statusTile.IsFrozen = false;
};
```

### Scenario 4: Freeze During Editing

```csharp
HubTile editableTile = new HubTile();
editableTile.TurnLiveTileOn = true;

// Freeze when editing
Button editButton = new Button();
editButton.Text = "Edit Tile";
editButton.Click += (s, e) => {
    // Freeze animation during editing
    editableTile.IsFrozen = true;
    
    // Show edit dialog
    using (var editForm = new TileEditForm())
    {
        if (editForm.ShowDialog() == DialogResult.OK)
        {
            // Update tile content
            editableTile.Title.Text = editForm.TileTitle;
            editableTile.Footer.Text = editForm.TileFooter;
        }
    }
    
    // Resume animation after editing
    editableTile.IsFrozen = false;
};
```

### Scenario 5: Freeze All Except One

```csharp
List<HubTile> tiles = new List<HubTile>();

private void HighlightTile(HubTile activeTile)
{
    foreach (HubTile tile in tiles)
    {
        if (tile == activeTile)
        {
            // Keep active tile animated
            tile.IsFrozen = false;
            tile.BackColor = Color.FromArgb(0, 120, 215);  // Highlighted
        }
        else
        {
            // Freeze others
            tile.IsFrozen = true;
            tile.BackColor = Color.FromArgb(100, 100, 100);  // Dimmed
        }
    }
}

// Each tile highlights itself on click
foreach (HubTile tile in tiles)
{
    tile.Click += (s, e) => HighlightTile(tile);
}
```

### Scenario 6: Timed Freeze/Unfreeze Cycle

```csharp
HubTile cycleTile = new HubTile();
cycleTile.TurnLiveTileOn = true;
cycleTile.TileType = HubTileType.DefaultTile;

// Cycle: 5 seconds animated, 5 seconds frozen
Timer cycleTimer = new Timer();
cycleTimer.Interval = 5000;  // 5 seconds
cycleTimer.Tick += (s, e) => {
    cycleTile.IsFrozen = !cycleTile.IsFrozen;
    
    // Update footer to show state
    cycleTile.Footer.Text = cycleTile.IsFrozen ? "Paused" : "Animating";
};
cycleTimer.Start();
```

### Scenario 7: Battery-Saving Mode

```csharp
bool isBatterySavingMode = false;
List<HubTile> animatedTiles = new List<HubTile>();

private void EnableBatterySaving()
{
    isBatterySavingMode = true;
    
    // Freeze all animated tiles
    foreach (HubTile tile in animatedTiles)
    {
        tile.IsFrozen = true;
    }
    
    MessageBox.Show("Battery saving mode enabled. Tile animations paused.");
}

private void DisableBatterySaving()
{
    isBatterySavingMode = false;
    
    // Resume all animations
    foreach (HubTile tile in animatedTiles)
    {
        tile.IsFrozen = false;
    }
    
    MessageBox.Show("Battery saving mode disabled. Tile animations resumed.");
}

// Menu items
MenuItem batterySaveOn = new MenuItem("Enable Battery Saving");
batterySaveOn.Click += (s, e) => EnableBatterySaving();

MenuItem batterySaveOff = new MenuItem("Disable Battery Saving");
batterySaveOff.Click += (s, e) => DisableBatterySaving();
```

## Freezing Multiple Tiles

### Freeze by Type

```csharp
private void FreezeTilesByType(HubTileType targetType)
{
    foreach (Control control in this.Controls)
    {
        if (control is HubTile tile && tile.TileType == targetType)
        {
            tile.IsFrozen = true;
        }
    }
}

// Freeze all DefaultTiles
FreezeTilesByType(HubTileType.DefaultTile);

// Freeze all PulsingTiles
FreezeTilesByType(HubTileType.PulsingTile);
```

### Freeze by Group/Tag

```csharp
// Tag tiles for grouping
tile1.Tag = "Group1";
tile2.Tag = "Group1";
tile3.Tag = "Group2";

private void FreezeGroup(string groupTag)
{
    foreach (Control control in this.Controls)
    {
        if (control is HubTile tile && tile.Tag?.ToString() == groupTag)
        {
            tile.IsFrozen = true;
        }
    }
}

// Freeze specific group
FreezeGroup("Group1");
```

### Freeze All Tiles

```csharp
private void FreezeAllTiles()
{
    foreach (Control control in this.Controls)
    {
        if (control is HubTile tile)
        {
            tile.IsFrozen = true;
        }
    }
}

private void UnfreezeAllTiles()
{
    foreach (Control control in this.Controls)
    {
        if (control is HubTile tile)
        {
            tile.IsFrozen = false;
        }
    }
}

// Global controls
Button freezeAll = new Button { Text = "Freeze All" };
freezeAll.Click += (s, e) => FreezeAllTiles();

Button unfreezeAll = new Button { Text = "Unfreeze All" };
unfreezeAll.Click += (s, e) => UnfreezeAllTiles();
```

## Troubleshooting

### Issue: IsFrozen has no effect

**Causes:**
- `TurnLiveTileOn = false` (tile not animated)
- Tile already not animating

**Solutions:**
```csharp
// Ensure tile is configured for animation
hubTile1.TurnLiveTileOn = true;
hubTile1.SlideTransition = TransitionDirection.BottomToTop;  // Or other animation

// Then freeze/unfreeze
hubTile1.IsFrozen = true;
```

### Issue: Tile doesn't resume after unfreezing

**Cause:** `TurnLiveTileOn` was set to false

**Solution:**
```csharp
// Ensure animation is enabled before unfreezing
hubTile1.TurnLiveTileOn = true;
hubTile1.IsFrozen = false;
```

### Issue: Some tiles not freezing in loop

**Cause:** Control type not checked, or tile reference not in collection

**Solution:**
```csharp
// Ensure type checking
foreach (Control control in this.Controls)
{
    if (control is HubTile tile)  // Type check
    {
        tile.IsFrozen = true;
    }
}

// Or maintain explicit tile list
List<HubTile> tiles = new List<HubTile>();
// Add tiles to list when created
foreach (HubTile tile in tiles)
{
    tile.IsFrozen = true;
}
```

### Issue: Freeze state not persisting after reload

**Cause:** IsFrozen not saved/restored

**Solution:**
```csharp
// Save state
Properties.Settings.Default.TilesFrozen = hubTile1.IsFrozen;
Properties.Settings.Default.Save();

// Restore state
hubTile1.IsFrozen = Properties.Settings.Default.TilesFrozen;
```

## Best Practices

1. **Maintain tile references** - Keep list of tiles for batch freeze/unfreeze operations
2. **User control** - Provide UI controls (checkbox, button) for freeze functionality
3. **Optimize performance** - Freeze off-screen or hidden tiles
4. **Indicate state** - Update tile appearance to show frozen state (e.g., change footer text)
5. **Consistent behavior** - Apply freeze uniformly across similar tiles
6. **Settings persistence** - Save user preference for animations enabled/disabled
7. **Conditional freezing** - Freeze based on application state (focus, battery, editing)
8. **Verify animation enabled** - Ensure `TurnLiveTileOn = true` before checking frozen state
9. **Test unfreeze** - Verify tiles resume correctly after being frozen
10. **Document behavior** - Inform users that frozen tiles retain their state
