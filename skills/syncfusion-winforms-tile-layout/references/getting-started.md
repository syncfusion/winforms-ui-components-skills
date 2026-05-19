# Getting Started with TileLayout

This guide covers the basics of setting up and using the **TileLayout** control in Syncfusion WinForms applications.

## Overview

TileLayout is a Windows 8 Start screen-inspired control that acts as a container holding tile view items organized in groups. The control supports:

- **Matrix positioning** for optimal layout
- **Drag-and-drop** reordering of tiles
- **Live tiles** with rotating images and text
- **Grouping** tiles into logical categories
- **Flexible customization** of appearance and behavior

**Key Architecture:**
```
TileLayout (Main Container)
  └── LayoutGroup (Category/Section)
      └── ImageStreamer (Individual Tile)
```

## Assembly Dependencies

To use TileLayout, add these assembly references to your project:

- `Syncfusion.Grid.Base.dll`
- `Syncfusion.Grid.Windows.dll`
- `Syncfusion.Shared.Base.dll`
- `Syncfusion.Shared.Windows.dll`
- `Syncfusion.Tools.Base.dll`
- `Syncfusion.Tools.Windows.dll`

**NuGet Package:** `Syncfusion.Tools.Windows`

## Creating TileLayout via Designer

### Step 1: Add TileLayout Control

1. Open the **Toolbox** in Visual Studio
2. Search for **TileLayout**
3. Drag and drop onto your form

The required assemblies are automatically referenced.

![TileLayout in Toolbox](images/windowsforms-tile-layout-toolbox.png)

### Step 2: Add LayoutGroup

1. Click the **Smart Tag** on the TileLayout control
2. Select **Groups Collection**
3. Click **Add** to create a new LayoutGroup
4. Set properties in PropertyGrid:
   - `Text`: Group title (e.g., "Photos")
   - `BackColor`: Background color

![Adding LayoutGroup via Designer](images/windowsforms-tile-layout-added-by-designer.png)

### Step 3: Add ImageStreamer Tiles

1. Select a LayoutGroup
2. In PropertyGrid, find **Items** collection
3. Click the ellipsis (...) button
4. Add **ImageStreamer** controls
5. For each ImageStreamer:
   - Click **Images** collection
   - Add image files

![Adding ImageStreamer to LayoutGroup](images/windowsforms-tile-layout-adding-image-streamer.png)

## Creating TileLayout via Code

### Step 1: Add Namespace

Include the required namespace in your code file:

```csharp
using Syncfusion.Windows.Forms.Tools;
```

```vb
Imports Syncfusion.Windows.Forms.Tools
```

### Step 2: Create TileLayout Instance

Create the TileLayout control and add it to your form:

```csharp
// Create TileLayout
TileLayout tileLayout1 = new TileLayout();
tileLayout1.Dock = DockStyle.Fill;

// Add to form
this.Controls.Add(tileLayout1);
```

```vb
' Create TileLayout
Dim tileLayout1 As New TileLayout()
tileLayout1.Dock = DockStyle.Fill

' Add to form
Me.Controls.Add(tileLayout1)
```

### Step 3: Create and Add LayoutGroups

Create LayoutGroup instances to organize tiles:

```csharp
// Create LayoutGroups
LayoutGroup layoutGroup1 = new LayoutGroup();
layoutGroup1.Text = "Photos";
layoutGroup1.BackColor = ColorTranslator.FromHtml("#0078D4");

LayoutGroup layoutGroup2 = new LayoutGroup();
layoutGroup2.Text = "Documents";
layoutGroup2.BackColor = ColorTranslator.FromHtml("#107C10");

// Add to TileLayout
tileLayout1.Controls.Add(layoutGroup1);
tileLayout1.Controls.Add(layoutGroup2);
```

```vb
' Create LayoutGroups
Dim layoutGroup1 As New LayoutGroup()
layoutGroup1.Text = "Photos"
layoutGroup1.BackColor = ColorTranslator.FromHtml("#0078D4")

Dim layoutGroup2 As New LayoutGroup()
layoutGroup2.Text = "Documents"
layoutGroup2.BackColor = ColorTranslator.FromHtml("#107C10")

' Add to TileLayout
tileLayout1.Controls.Add(layoutGroup1)
tileLayout1.Controls.Add(layoutGroup2)
```

![LayoutGroups Added by Code](images/windowsforms-tile-layout-group-by-code.png)

### Step 4: Create and Add ImageStreamer Tiles

Add ImageStreamer controls as tiles within groups:

```csharp
// Create ImageStreamer instances
ImageStreamer imageStreamer1 = new ImageStreamer();
ImageStreamer imageStreamer2 = new ImageStreamer();
ImageStreamer imageStreamer3 = new ImageStreamer();
ImageStreamer imageStreamer4 = new ImageStreamer();

// Add images to ImageStreamers
imageStreamer1.Images.Add(Image.FromFile("photo1.jpg"));
imageStreamer1.Images.Add(Image.FromFile("photo2.jpg"));
imageStreamer2.Images.Add(Image.FromFile("photo3.jpg"));
imageStreamer3.Images.Add(Image.FromFile("doc1.png"));
imageStreamer4.Images.Add(Image.FromFile("doc2.png"));

// Configure for live tiles
imageStreamer1.SlideShow = true;
imageStreamer1.SliderSpeed = 100;

// Add to LayoutGroups
layoutGroup1.Controls.Add(imageStreamer1);
layoutGroup1.Controls.Add(imageStreamer2);
layoutGroup2.Controls.Add(imageStreamer3);
layoutGroup2.Controls.Add(imageStreamer4);
```

```vb
' Create ImageStreamer instances
Dim imageStreamer1 As New ImageStreamer()
Dim imageStreamer2 As New ImageStreamer()
Dim imageStreamer3 As New ImageStreamer()
Dim imageStreamer4 As New ImageStreamer()

' Add images to ImageStreamers
imageStreamer1.Images.Add(Image.FromFile("photo1.jpg"))
imageStreamer1.Images.Add(Image.FromFile("photo2.jpg"))
imageStreamer2.Images.Add(Image.FromFile("photo3.jpg"))
imageStreamer3.Images.Add(Image.FromFile("doc1.png"))
imageStreamer4.Images.Add(Image.FromFile("doc2.png"))

' Configure for live tiles
imageStreamer1.SlideShow = True
imageStreamer1.SliderSpeed = 100

' Add to LayoutGroups
layoutGroup1.Controls.Add(imageStreamer1)
layoutGroup1.Controls.Add(imageStreamer2)
layoutGroup2.Controls.Add(imageStreamer3)
layoutGroup2.Controls.Add(imageStreamer4)
```

![Images Added to LayoutGroups](images/windowsforms-tile-layout-adding-images-to-group.png)

## Complete Minimal Example

Here's a complete working example creating a TileLayout with two groups:

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace TileLayoutDemo
{
    public partial class Form1 : Form
    {
        private TileLayout tileLayout1;
        
        public Form1()
        {
            InitializeComponent();
            SetupTileLayout();
        }
        
        private void SetupTileLayout()
        {
            // Create TileLayout
            tileLayout1 = new TileLayout();
            tileLayout1.Dock = DockStyle.Fill;
            tileLayout1.ShowGroupTitle = true;
            
            // Create "Apps" group
            LayoutGroup appsGroup = new LayoutGroup();
            appsGroup.Text = "Applications";
            appsGroup.BackColor = Color.FromArgb(0, 120, 215);
            
            // Create "Media" group
            LayoutGroup mediaGroup = new LayoutGroup();
            mediaGroup.Text = "Media";
            mediaGroup.BackColor = Color.FromArgb(16, 124, 16);
            
            // Add tiles to Apps group
            for (int i = 1; i <= 4; i++)
            {
                ImageStreamer tile = new ImageStreamer();
                tile.Images.Add(Image.FromFile($"app{i}.png"));
                tile.InternalBackColor = Color.White;
                appsGroup.Controls.Add(tile);
            }
            
            // Add tiles to Media group with slideshow
            for (int i = 1; i <= 3; i++)
            {
                ImageStreamer tile = new ImageStreamer();
                tile.Images.Add(Image.FromFile($"media{i}.jpg"));
                tile.Images.Add(Image.FromFile($"media{i}_alt.jpg"));
                tile.SlideShow = true;
                tile.SliderSpeed = 150;
                mediaGroup.Controls.Add(tile);
            }
            
            // Add groups to TileLayout
            tileLayout1.Controls.Add(appsGroup);
            tileLayout1.Controls.Add(mediaGroup);
            
            // Add to form
            this.Controls.Add(tileLayout1);
            this.Text = "TileLayout Demo";
            this.Size = new Size(900, 600);
        }
    }
}
```

**Result:** A form with two tile groups ("Applications" and "Media"), containing multiple tiles. Media tiles animate automatically.

## Basic Configuration

### Show Group Titles

Display group names above each LayoutGroup:

```csharp
tileLayout1.ShowGroupTitle = true;
```

### Set Background Colors

Customize TileLayout and LayoutGroup colors:

```csharp
// TileLayout background (must set IgnoreThemeBackground first)
tileLayout1.IgnoreThemeBackground = true;
tileLayout1.BackColor = Color.LightGray;

// LayoutGroup background
layoutGroup1.BackColor = Color.FromArgb(0, 120, 215);
```

### Configure Layout Margins

Add spacing around the tile layout:

```csharp
tileLayout1.MainLayout.HorzNearMargin = 20;  // Left margin
tileLayout1.MainLayout.HorzFarMargin = 20;   // Right margin
tileLayout1.MainLayout.TopMargin = 20;       // Top margin
tileLayout1.MainLayout.BottomMargin = 20;    // Bottom margin
```

## Common Setup Patterns

### Dashboard with Multiple Categories

```csharp
string[] categories = { "Productivity", "Entertainment", "Tools", "Settings" };
Color[] colors = {
    Color.FromArgb(0, 120, 215),    // Blue
    Color.FromArgb(16, 124, 16),    // Green
    Color.FromArgb(232, 17, 35),    // Red
    Color.FromArgb(255, 140, 0)     // Orange
};

for (int i = 0; i < categories.Length; i++)
{
    var group = new LayoutGroup();
    group.Text = categories[i];
    group.BackColor = colors[i];
    
    // Add tiles to group
    for (int j = 0; j < 6; j++)
    {
        var tile = new ImageStreamer();
        tile.Images.Add(GetIcon(categories[i], j));
        group.Controls.Add(tile);
    }
    
    tileLayout1.Controls.Add(group);
}
```

### Photo Gallery

```csharp
var photoGroup = new LayoutGroup();
photoGroup.Text = "Recent Photos";

foreach (string photoPath in Directory.GetFiles("Photos", "*.jpg"))
{
    var tile = new ImageStreamer();
    tile.Images.Add(Image.FromFile(photoPath));
    tile.SlideShow = false; // Click to view, no auto-rotation
    tile.InternalBackColor = Color.Black;
    photoGroup.Controls.Add(tile);
}

tileLayout1.Controls.Add(photoGroup);
```

## When to Use

Use TileLayout when you need:
- Windows 8/10 Metro-style interfaces
- Grouped, visual navigation (app launchers, dashboards)
- Live tiles with rotating content
- Draggable, user-customizable layouts
- Modern, touch-friendly UIs

## Next Steps

- **Layout Customization:** Learn about margins, alignment, and positioning
- **Appearance Styling:** Customize visual appearance and themes
- **ImageStreamer Configuration:** Create live tiles with animations
- **Drag and Drop:** Enable user tile reordering

## Troubleshooting

**Tiles not visible:**
- Ensure each ImageStreamer has at least one image in Images collection
- Check that ImageStreamers are added to LayoutGroup.Controls
- Verify LayoutGroups are added to TileLayout.Controls

**Images not loading:**
- Use absolute paths or ensure relative paths are correct
- Check file exists and is accessible
- Use try-catch when loading images from files

**Layout not displaying correctly:**
- Ensure TileLayout.Dock = DockStyle.Fill or set explicit size
- Check form size is adequate for tile content
- Verify parent form has enough space
