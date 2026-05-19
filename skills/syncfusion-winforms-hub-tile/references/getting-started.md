# Getting Started

## Assembly Deployment

To use the HubTile control, reference the required assemblies in your Windows Forms project.

### Required Assemblies

```
Syncfusion.Shared.Base.dll
Syncfusion.Shared.Windows.dll
Syncfusion.Tools.Base.dll
Syncfusion.Tools.Windows.dll
Syncfusion.Grid.Base.dll
Syncfusion.Grid.Windows.dll
```

**Why these assemblies:**
- `Syncfusion.Tools.Windows.dll` - Contains HubTile control
- `Syncfusion.Shared.Base.dll` and `Syncfusion.Shared.Windows.dll` - Provide base classes and common functionality
- Grid assemblies - Required dependencies for HubTile functionality

### Framework Support

HubTile control supports:
- .NET Framework 4.5, 4.5.1, 4.6, 4.7, 4.8
- .NET 6.0, .NET 7.0, .NET 8.0

## Installation Methods

### Method 1: NuGet Package Manager

**Via Package Manager Console:**

```powershell
Install-Package Syncfusion.Tools.Windows
```

**Via NuGet Package Manager UI:**
1. Right-click project → Manage NuGet Packages
2. Search for "Syncfusion.Tools.Windows"
3. Select package → Install

**Note:** The Tools.Windows package includes HubTile plus additional controls.

### Method 2: Manual Assembly Reference

If Syncfusion Essential Studio is installed locally:

1. Right-click project → Add Reference
2. Browse to installation location:
   - **Windows 7/8/10/11:** `C:\Program Files (x86)\Syncfusion\Essential Studio\Windows\{version}\precompiledassemblies\{framework version}`
3. Select required DLLs → OK

## Namespace Imports

Add this using directive to your code files:

```csharp
using Syncfusion.Windows.Forms.Tools;
using System.Drawing;
using System.Windows.Forms;
```

**Namespace contents:**
- `Syncfusion.Windows.Forms.Tools` - Contains HubTile control and supporting enumerations

## Adding Control via Designer

### Step 1: Open Toolbox

1. Open your Windows Forms project in Visual Studio
2. Open the form in Designer view
3. Access the Toolbox (View → Toolbox or Ctrl+Alt+X)

### Step 2: Locate HubTile

1. In Toolbox, search for "HubTile" or browse Syncfusion Tools section
2. If not visible, right-click Toolbox → Choose Items
3. Browse to `Syncfusion.Tools.Windows.dll`
4. Check HubTile → OK

### Step 3: Add to Form

1. Drag HubTile from Toolbox
2. Drop onto form Designer
3. Position and resize as needed
4. Required assemblies are referenced automatically

![Search HubTile in toolbox](../../../docs/Overview_images/GettingStarted-img1.png)

### Step 4: Configure Properties

Use the Properties window to configure:
- `TileType` - DefaultTile, RotateTile, or PulsingTile
- `Title.Text` - Tile title
- `Footer.Text` - Tile footer
- `ImageSource` - Background image
- `BackColor` - Tile background color
- `TurnLiveTileOn` - Enable animations

## Adding Control via Code

### Step 1: Create Project

Create a new Windows Forms project in Visual Studio.

### Step 2: Add Assembly References

Manually add the 6 required assembly references listed above.

### Step 3: Import Namespace

```csharp
using Syncfusion.Windows.Forms.Tools;
```

### Step 4: Create HubTile Instance

```csharp
public partial class Form1 : Form
{
    public Form1()
    {
        InitializeComponent();
        CreateHubTile();
    }

    private void CreateHubTile()
    {
        // Create HubTile instance
        HubTile hubTile1 = new HubTile();
        
        // Set size and location
        hubTile1.Size = new Size(200, 200);
        hubTile1.Location = new Point(20, 20);
        
        // Add to form
        this.Controls.Add(hubTile1);
    }
}
```

## Basic Configuration

### Set Tile Type

HubTile supports three tile types:

```csharp
// DefaultTile - slide transitions
hubTile1.TileType = HubTileType.DefaultTile;

// RotateTile - rotation animations
hubTile2.TileType = HubTileType.RotateTile;

// PulsingTile - zoom effects
hubTile3.TileType = HubTileType.PulsingTile;
```

**Visual comparison:**

| Type | Animation | Best For |
|------|-----------|----------|
| DefaultTile | Slide transitions (4 directions) | News feeds, status updates |
| RotateTile | Horizontal/vertical rotation | Dual-state content |
| PulsingTile | Zoom in/out effects | Media content, attention |

### Set Title and Footer

```csharp
// Set title (top of tile)
hubTile1.Title.Text = "This is the title area";
hubTile1.Title.TextColor = Color.White;

// Set footer (bottom of tile)
hubTile1.Footer.Text = "HubTile";
hubTile1.Footer.TextColor = Color.White;

// Set background color
hubTile1.BackColor = Color.FromArgb(17, 158, 218);
```

### Add Background Image

**Via Designer:**
1. Select HubTile in Designer
2. Open Smart Tag (small arrow on control)
3. Click `ImageSource` property
4. Browse and select image file

**Via Code:**

```csharp
// From file
hubTile1.ImageSource = Image.FromFile("myimage.jpg");

// From resources
hubTile1.ImageSource = Properties.Resources.MyImage;

// From ImageList
hubTile1.ImageSource = imageListAdv1.Images[0];
```

## Enable Live Tile Animation

Turn on automatic animations:

```csharp
// Enable live tile functionality
hubTile1.TurnLiveTileOn = true;

// For DefaultTile: Set transition direction
hubTile1.SlideTransition = TransitionDirection.BottomToTop;

// For RotateTile: Set rotation direction
hubTile2.RotationTransition = TileFlipDirection.Horizontal;

// For PulsingTile: Set pulse properties
hubTile3.PulseDuration = 2;
hubTile3.PulseScale = 1.5f;
```

## Complete Example

### DefaultTile with All Features

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace HubTileDemo
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
            CreateDefaultTile();
        }

        private void CreateDefaultTile()
        {
            // Create HubTile
            HubTile newsTile = new HubTile();
            newsTile.Size = new Size(200, 200);
            newsTile.Location = new Point(20, 20);
            
            // Set tile type
            newsTile.TileType = HubTileType.DefaultTile;
            
            // Configure title
            newsTile.Title.Text = "Breaking News";
            newsTile.Title.TextColor = Color.White;
            newsTile.Title.Font = new Font("Segoe UI", 12, FontStyle.Bold);
            
            // Configure footer
            newsTile.Footer.Text = "CNN News";
            newsTile.Footer.TextColor = Color.White;
            newsTile.Footer.Font = new Font("Segoe UI", 9);
            
            // Set background
            newsTile.BackColor = Color.FromArgb(17, 158, 218);
            
            // Add image (if available)
            // newsTile.ImageSource = Image.FromFile("news.jpg");
            
            // Enable live tile with transition
            newsTile.TurnLiveTileOn = true;
            newsTile.SlideTransition = TransitionDirection.BottomToTop;
            newsTile.ImageTransitionSpeed = 3;
            
            // Add to form
            this.Controls.Add(newsTile);
        }
    }
}
```

### Creating Multiple Tiles

```csharp
private void CreateDashboard()
{
    // Create layout panel
    FlowLayoutPanel panel = new FlowLayoutPanel();
    panel.Dock = DockStyle.Fill;
    panel.BackColor = Color.FromArgb(30, 30, 30);
    panel.Padding = new Padding(10);
    
    // Add multiple tiles
    panel.Controls.Add(CreateTile("News", "Latest", Color.FromArgb(17, 158, 218), HubTileType.DefaultTile));
    panel.Controls.Add(CreateTile("Weather", "72°F", Color.FromArgb(255, 140, 0), HubTileType.RotateTile));
    panel.Controls.Add(CreateTile("Music", "Playing", Color.FromArgb(139, 0, 139), HubTileType.PulsingTile));
    panel.Controls.Add(CreateTile("Email", "Inbox", Color.FromArgb(0, 120, 215), HubTileType.DefaultTile));
    
    this.Controls.Add(panel);
}

private HubTile CreateTile(string title, string footer, Color backColor, HubTileType tileType)
{
    HubTile tile = new HubTile();
    tile.Size = new Size(150, 150);
    tile.Margin = new Padding(5);
    tile.TileType = tileType;
    tile.Title.Text = title;
    tile.Title.TextColor = Color.White;
    tile.Footer.Text = footer;
    tile.Footer.TextColor = Color.White;
    tile.BackColor = backColor;
    tile.TurnLiveTileOn = true;
    
    // Configure based on tile type
    if (tileType == HubTileType.DefaultTile)
        tile.SlideTransition = TransitionDirection.BottomToTop;
    else if (tileType == HubTileType.RotateTile)
        tile.RotationTransition = TileFlipDirection.Horizontal;
    else if (tileType == HubTileType.PulsingTile)
    {
        tile.PulseDuration = 2;
        tile.PulseScale = 1.3f;
    }
    
    return tile;
}
```

## Transition Effects

### DefaultTile Transitions

HubTile supports four slide transition directions:

```csharp
// Bottom to Top (default)
hubTile1.SlideTransition = TransitionDirection.BottomToTop;

// Top to Bottom
hubTile2.SlideTransition = TransitionDirection.TopToBottom;

// Left to Right
hubTile3.SlideTransition = TransitionDirection.LeftToRight;

// Right to Left
hubTile4.SlideTransition = TransitionDirection.RightToLeft;
```

**Use case guide:**
- **BottomToTop:** News feeds, status updates (natural upward flow)
- **TopToBottom:** Dropdown-style content
- **LeftToRight:** Timeline progression, next content
- **RightToLeft:** Previous content, backward navigation

## Troubleshooting

### Issue: HubTile not appearing in Toolbox

**Solutions:**
1. Verify assemblies are referenced in project
2. Clean and rebuild solution
3. Restart Visual Studio
4. Manually add via Choose Items (browse to DLL)
5. Check target framework matches assembly version

### Issue: Animations not working

**Causes:**
- `TurnLiveTileOn = false`
- `IsFrozen = true`
- Tile not visible or size is too small

**Solutions:**
```csharp
hubTile1.TurnLiveTileOn = true;  // Enable animations
hubTile1.IsFrozen = false;       // Unfreeze if frozen
hubTile1.Visible = true;         // Ensure visible
hubTile1.Size = new Size(150, 150);  // Adequate size
```

### Issue: Images not displaying

**Causes:**
- Image path incorrect
- Image file not found
- ImageSource property not set

**Solutions:**
```csharp
// Verify file exists
if (System.IO.File.Exists("image.jpg"))
{
    hubTile1.ImageSource = Image.FromFile("image.jpg");
}

// Use absolute path for testing
hubTile1.ImageSource = Image.FromFile(@"C:\Images\tile.jpg");

// Or use resources
hubTile1.ImageSource = Properties.Resources.TileImage;
```

### Issue: Title or Footer text not visible

**Causes:**
- Text color matches background color
- Font size too large
- Text property not set

**Solutions:**
```csharp
// Set contrasting colors
hubTile1.BackColor = Color.Blue;
hubTile1.Title.TextColor = Color.White;  // High contrast

// Adjust font size
hubTile1.Title.Font = new Font("Segoe UI", 10);

// Verify text is set
hubTile1.Title.Text = "My Title";
```

### Issue: FileNotFoundException at runtime

**Solutions:**
1. Verify all 6 assemblies are copied to output directory
2. Check Copy Local = True for assembly references
3. Verify target framework compatibility
4. Rebuild solution

## Next Steps

Once installation is complete:

- **DefaultTile:** Read [default-tile.md](default-tile.md) for slide transition details
- **RotateTile:** Read [rotate-tile.md](rotate-tile.md) for rotation animations
- **PulsingTile:** Read [pulsing-tile.md](pulsing-tile.md) for zoom effects
- **Styling:** Read [appearance-styling.md](appearance-styling.md) for customization
- **Control:** Read [freezing-control.md](freezing-control.md) for animation control
