# Drag and Drop in TileLayout

This guide covers the drag-and-drop functionality in TileLayout, allowing users to reorder tiles at runtime.

## Overview

TileLayout supports **easy drag-and-drop** of ImageStreamer tiles at runtime. Users can:

- Drag tiles within the same LayoutGroup to reorder
- Drag tiles between different LayoutGroups to reorganize
- Dynamically customize their layout
- Persist custom arrangements

**Key Feature:** Drag-and-drop is **enabled by default**—no configuration required!

![Drag and Drop in Action](images/DragandDrop_img1.jpeg)

## How Drag and Drop Works

### Default Behavior

By default, all ImageStreamer items in TileLayout support drag-and-drop:

```csharp
// Create TileLayout with LayoutGroups and ImageStreamers
TileLayout tileLayout1 = new TileLayout();
LayoutGroup group1 = new LayoutGroup();

ImageStreamer tile1 = new ImageStreamer();
ImageStreamer tile2 = new ImageStreamer();
ImageStreamer tile3 = new ImageStreamer();

// Add images
tile1.Images.Add(Image.FromFile("icon1.png"));
tile2.Images.Add(Image.FromFile("icon2.png"));
tile3.Images.Add(Image.FromFile("icon3.png"));

// Add to group
group1.Controls.Add(tile1);
group1.Controls.Add(tile2);
group1.Controls.Add(tile3);

tileLayout1.Controls.Add(group1);

// Drag-and-drop is automatically enabled!
// Users can now drag tiles to reorder
```

No additional code or properties needed—drag-and-drop works out of the box.

## Drag Within Same LayoutGroup

Users can reorder tiles within a single group by dragging and dropping.

**User Action:**
1. Click and hold on a tile
2. Drag to desired position within same group
3. Release to drop

**Result:** Tiles reorder within the group

**Example - App launcher reordering:**
```csharp
LayoutGroup appsGroup = new LayoutGroup();
appsGroup.Text = "My Apps";

// Add 6 app tiles
for (int i = 1; i <= 6; i++)
{
    ImageStreamer appTile = new ImageStreamer();
    appTile.Images.Add(LoadAppIcon($"App{i}"));
    appTile.Tag = $"App{i}"; // Store app identifier
    appsGroup.Controls.Add(appTile);
}

tileLayout1.Controls.Add(appsGroup);

// Users can now drag apps to reorder favorites
```

**Use Cases:**
- Prioritizing frequently-used apps
- Organizing by user preference
- Customizing dashboard layout
- Creating personalized arrangements

## Drag Between Different LayoutGroups

Users can move tiles from one LayoutGroup to another.

**User Action:**
1. Click and hold on a tile in Group A
2. Drag to Group B
3. Release to drop

**Result:** Tile moves from Group A to Group B

**Example - Categorizing items:**
```csharp
LayoutGroup todoGroup = new LayoutGroup();
todoGroup.Text = "To Do";

LayoutGroup inProgressGroup = new LayoutGroup();
inProgressGroup.Text = "In Progress";

LayoutGroup doneGroup = new LayoutGroup();
doneGroup.Text = "Done";

// Add task tiles to "To Do"
foreach (var task in tasks)
{
    ImageStreamer taskTile = new ImageStreamer();
    taskTile.Images.Add(CreateTaskImage(task));
    taskTile.Tag = task; // Store task object
    todoGroup.Controls.Add(taskTile);
}

tileLayout1.Controls.Add(todoGroup);
tileLayout1.Controls.Add(inProgressGroup);
tileLayout1.Controls.Add(doneGroup);

// Users drag tasks between groups as status changes
```

**Use Cases:**
- Task/project management (Kanban-style boards)
- Content categorization
- Status tracking
- Workflow visualization

## Runtime Tile Reordering

### Detecting Reordering

Monitor LayoutGroup.ControlAdded and ControlRemoved events:

```csharp
// Track when tiles are moved
private void SetupReorderingTracking()
{
    foreach (LayoutGroup group in tileLayout1.Controls.OfType<LayoutGroup>())
    {
        group.ControlAdded += Group_ControlAdded;
        group.ControlRemoved += Group_ControlRemoved;
    }
}

private void Group_ControlAdded(object sender, ControlEventArgs e)
{
    LayoutGroup group = (LayoutGroup)sender;
    ImageStreamer tile = e.Control as ImageStreamer;
    
    if (tile != null)
    {
        Console.WriteLine($"Tile '{tile.Tag}' added to group '{group.Text}'");
        SaveLayout(); // Persist changes
    }
}

private void Group_ControlRemoved(object sender, ControlEventArgs e)
{
    LayoutGroup group = (LayoutGroup)sender;
    ImageStreamer tile = e.Control as ImageStreamer;
    
    if (tile != null)
    {
        Console.WriteLine($"Tile '{tile.Tag}' removed from group '{group.Text}'");
    }
}
```

### Programmatic Reordering

Reorder tiles programmatically:

```csharp
// Move tile to specific position within group
private void MoveTile(LayoutGroup group, ImageStreamer tile, int newIndex)
{
    group.Controls.Remove(tile);
    group.Controls.Add(tile);
    group.Controls.SetChildIndex(tile, newIndex);
}

// Move tile to different group
private void MoveTileToGroup(ImageStreamer tile, LayoutGroup targetGroup)
{
    // Remove from current parent
    if (tile.Parent is LayoutGroup currentGroup)
    {
        currentGroup.Controls.Remove(tile);
    }
    
    // Add to new group
    targetGroup.Controls.Add(tile);
}

// Example usage
MoveTile(appsGroup, favoriteTile, 0); // Move to first position
MoveTileToGroup(taskTile, completedGroup); // Move to completed group
```

## Persisting User Arrangements

Save and restore tile positions across sessions:

```csharp
// Save layout to file
private void SaveLayout()
{
    var layout = new TileLayoutData();
    
    foreach (LayoutGroup group in tileLayout1.Controls.OfType<LayoutGroup>())
    {
        var groupData = new GroupData { GroupName = group.Text };
        
        foreach (ImageStreamer tile in group.Controls.OfType<ImageStreamer>())
        {
            if (tile.Tag != null)
            {
                groupData.TileIds.Add(tile.Tag.ToString());
            }
        }
        
        layout.Groups.Add(groupData);
    }
    
    // Serialize to JSON/XML
    string json = JsonConvert.SerializeObject(layout);
    File.WriteAllText("tile_layout.json", json);
}

// Restore layout from file
private void RestoreLayout()
{
    if (!File.Exists("tile_layout.json"))
        return;
    
    string json = File.ReadAllText("tile_layout.json");
    var layout = JsonConvert.DeserializeObject<TileLayoutData>(json);
    
    // Rebuild layout from saved data
    foreach (var groupData in layout.Groups)
    {
        LayoutGroup group = FindGroupByName(groupData.GroupName);
        if (group == null) continue;
        
        // Clear and rebuild in saved order
        group.Controls.Clear();
        foreach (string tileId in groupData.TileIds)
        {
            ImageStreamer tile = CreateTileById(tileId);
            group.Controls.Add(tile);
        }
    }
}

// Supporting classes
[Serializable]
public class TileLayoutData
{
    public List<GroupData> Groups { get; set; } = new List<GroupData>();
}

[Serializable]
public class GroupData
{
    public string GroupName { get; set; }
    public List<string> TileIds { get; set; } = new List<string>();
}
```

**Use Case:** Remember user's custom arrangement between application sessions.

## User Interaction Patterns

### Visual Feedback During Drag

Provide feedback during drag operations:

```csharp
private ImageStreamer draggedTile = null;

private void SetupDragFeedback()
{
    foreach (LayoutGroup group in tileLayout1.Controls.OfType<LayoutGroup>())
    {
        foreach (ImageStreamer tile in group.Controls.OfType<ImageStreamer>())
        {
            tile.MouseDown += Tile_MouseDown;
            tile.MouseUp += Tile_MouseUp;
        }
    }
}

private void Tile_MouseDown(object sender, MouseEventArgs e)
{
    if (e.Button == MouseButtons.Left)
    {
        draggedTile = (ImageStreamer)sender;
        draggedTile.Cursor = Cursors.Hand;
    }
}

private void Tile_MouseUp(object sender, MouseEventArgs e)
{
    if (draggedTile != null)
    {
        draggedTile.Cursor = Cursors.Default;
        draggedTile = null;
        
        // Save new arrangement
        SaveLayout();
    }
}
```

### Reset to Default Layout

Provide option to restore original layout:

```csharp
private void ResetToDefaultLayout()
{
    // Clear all groups
    foreach (LayoutGroup group in tileLayout1.Controls.OfType<LayoutGroup>())
    {
        group.Controls.Clear();
    }
    
    // Rebuild default layout
    CreateDefaultLayout();
    
    // Delete saved layout
    if (File.Exists("tile_layout.json"))
    {
        File.Delete("tile_layout.json");
    }
}

// Add button for reset
Button resetButton = new Button();
resetButton.Text = "Reset Layout";
resetButton.Click += (s, e) => ResetToDefaultLayout();
```

## Restrictions and Limitations

### What You Can Drag

✅ **Supported:**
- ImageStreamer controls within LayoutGroups
- Any control added to LayoutGroup.Controls

❌ **Not Supported:**
- LayoutGroups themselves (groups cannot be reordered by drag-and-drop)
- Controls outside of LayoutGroups
- Tiles to locations outside TileLayout

### Programmatic Restrictions

If you need to restrict drag-and-drop:

```csharp
// Disable drag-and-drop for specific tiles
// Note: TileLayout doesn't have built-in disable property
// Workaround: Handle mouse events to prevent dragging

private void MakeTileNonDraggable(ImageStreamer tile)
{
    tile.Tag = "locked";
    
    // Capture mouse events to prevent drag
    tile.PreviewMouseDown  += (s, e) =>
    {
        if (tile.Tag?.ToString() == "locked")
        {
            e.Handled = true;
        }
    };
}
```

### Validation Before Moving

Validate moves between groups:

```csharp
private void Group_ControlAdded(object sender, ControlEventArgs e)
{
    LayoutGroup targetGroup = (LayoutGroup)sender;
    ImageStreamer tile = e.Control as ImageStreamer;
    
    // Validate move
    if (!IsValidMove(tile, targetGroup))
    {
        // Revert move
        MessageBox.Show("This tile cannot be placed in this group.");
        targetGroup.Controls.Remove(tile);
        
        // Return to original group
        RestoreTileToOriginalGroup(tile);
    }
}

private bool IsValidMove(ImageStreamer tile, LayoutGroup targetGroup)
{
    // Example: Limit certain tiles to specific groups
    if (targetGroup.Text == "Admin Tools" && !IsAdminUser())
    {
        return false;
    }
    
    return true;
}
```

## Best Practices

1. **Save Layouts:** Persist user arrangements between sessions
2. **Visual Feedback:** Show cursor changes during drag operations
3. **Provide Reset:** Allow users to restore default layout
4. **Validate Moves:** Check if tile moves are allowed based on business rules
5. **User Education:** Add tooltips or help text explaining drag-and-drop capability
6. **Performance:** For large numbers of tiles (50+), consider lazy-loading or pagination

## Complete Drag-and-Drop Example

```csharp
using System;
using System.Drawing;
using System.IO;
using System.Linq;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;
using Newtonsoft.Json;

public class DraggableTileLayoutForm : Form
{
    private TileLayout tileLayout1;
    
    public DraggableTileLayoutForm()
    {
        SetupDraggableLayout();
        RestoreLayout(); // Load saved layout if exists
    }
    
    private void SetupDraggableLayout()
    {
        tileLayout1 = new TileLayout();
        tileLayout1.Dock = DockStyle.Fill;
        tileLayout1.ShowGroupTitle = true;
        
        // Create groups
        CreateGroups();
        
        // Setup tracking
        SetupReorderingTracking();
        
        // Add reset button
        Button resetBtn = new Button();
        resetBtn.Text = "Reset Layout";
        resetBtn.Dock = DockStyle.Top;
        resetBtn.Click += (s, e) => ResetToDefaultLayout();
        
        this.Controls.Add(tileLayout1);
        this.Controls.Add(resetBtn);
        this.Text = "Draggable Tile Layout";
        this.Size = new Size(1000, 700);
    }
    
    private void CreateGroups()
    {
        string[] groupNames = { "Favorites", "Work", "Entertainment" };
        
        foreach (string name in groupNames)
        {
            LayoutGroup group = new LayoutGroup();
            group.Text = name;
            group.BackColor = GetGroupColor(name);
            
            // Add sample tiles
            for (int i = 1; i <= 4; i++)
            {
                ImageStreamer tile = new ImageStreamer();
                tile.Tag = $"{name}_{i}";
                tile.Images.Add(CreatePlaceholderImage($"{name} {i}"));
                group.Controls.Add(tile);
            }
            
            tileLayout1.Controls.Add(group);
        }
    }
    
    private void SetupReorderingTracking()
    {
        foreach (LayoutGroup group in tileLayout1.Controls.OfType<LayoutGroup>())
        {
            group.ControlAdded += (s, e) => SaveLayout();
            group.ControlRemoved += (s, e) => SaveLayout();
        }
    }
    
    private void SaveLayout()
    {
        // Implementation from earlier example
        // Save tile positions to JSON file
    }
    
    private void RestoreLayout()
    {
        // Implementation from earlier example
        // Load tile positions from JSON file
    }
    
    private void ResetToDefaultLayout()
    {
        // Clear and rebuild
        foreach (LayoutGroup group in tileLayout1.Controls.OfType<LayoutGroup>())
        {
            group.Controls.Clear();
        }
        CreateGroups();
    }
    
    private Color GetGroupColor(string name)
    {
        switch (name)
        {
            case "Favorites": return Color.FromArgb(0, 120, 215);
            case "Work": return Color.FromArgb(16, 124, 16);
            case "Entertainment": return Color.FromArgb(255, 140, 0);
            default: return Color.Gray;
        }
    }
    
    private Image CreatePlaceholderImage(string text)
    {
        Bitmap bmp = new Bitmap(150, 150);
        using (Graphics g = Graphics.FromImage(bmp))
        {
            g.FillRectangle(Brushes.White, 0, 0, 150, 150);
            g.DrawString(text, new Font("Arial", 10), Brushes.Black, 
                new PointF(10, 60));
        }
        return bmp;
    }
}
```

## Troubleshooting

**Issue:** Drag-and-drop not working
- **Check:** Ensure items are ImageStreamer controls within LayoutGroups
- **Check:** Verify no conflicting mouse event handlers
- **Check:** Make sure controls are not disabled

**Issue:** Tiles snap back to original position
- **Cause:** Event handler preventing the move
- **Solution:** Review ControlAdded/ControlRemoved event handlers for restrictions

**Issue:** Performance degradation during dragging
- **Cause:** Too many tiles or complex event handlers
- **Solution:** Reduce tile count, optimize event handlers, use BeginUpdate/EndUpdate

**Issue:** Layout not persisting between sessions
- **Cause:** SaveLayout not being called or file permissions
- **Solution:** Verify SaveLayout is called on ControlAdded/ControlRemoved events

## Summary

TileLayout's drag-and-drop functionality provides:

- **Automatic Support:** Enabled by default, no configuration needed
- **Within-Group Reordering:** Users can rearrange tiles in same group
- **Cross-Group Moving:** Users can move tiles between groups
- **Event Tracking:** Monitor reordering with ControlAdded/ControlRemoved
- **Persistence:** Save and restore user arrangements

This creates highly customizable, user-friendly tile layouts that adapt to individual preferences and workflows.
