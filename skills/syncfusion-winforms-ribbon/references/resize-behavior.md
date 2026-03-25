# Resize Behavior and Responsive Layout

## Overview

RibbonControlAdv automatically adapts to window size changes with intelligent resizing behavior, collapsing controls into compact representations or dropdown menus to maintain functionality at smaller widths.

## CollapseBehavior Property

Controls how ribbon groups (ToolStripEx) collapse when window size decreases.

### Collapse Behavior Options

**Default:**
- Groups collapse one by one from right to left
- Items within groups collapse progressively
- Large buttons → Medium buttons → Small buttons → Dropdown

**Office2010:**
- Groups show launcher button when collapsed
- All items hidden except group label
- Clicking launcher opens popup with all items

### Setting Collapse Behavior

```csharp
// Use Office 2010 collapse behavior
ribbonControlAdv1.CollapseBehavior = RibbonCollapseBehavior.Office2010;

// Use default collapse behavior
ribbonControlAdv1.CollapseBehavior = RibbonCollapseBehavior.Default;
```

## Image Sizing for Resize

### ToolStripExImageProvider

Provides multiple image sizes for ribbon items to display at different collapse states.

```csharp
// Create image provider with multiple sizes
ToolStripExImageProvider imageProvider = new ToolStripExImageProvider(ribbonControlAdv1);

// Set image lists
imageProvider.LargeImageList = largeImageList;   // 32x32
imageProvider.MediumImageList = mediumImageList; // 20x20
imageProvider.SmallImageList = smallImageList;   // 16x16

// Assign to ribbon
ribbonControlAdv1.ToolStripExImageProvider = imageProvider;

// Set item image index
copyButton.Image = largeImageList.Images[0];
copyButton.ImageIndex = 0; // Index in all image lists
```

### Image Lists Setup

```csharp
// Create image lists with different sizes
ImageList largeImages = new ImageList();
largeImages.ImageSize = new Size(32, 32);
largeImages.Images.Add("copy", Properties.Resources.Copy32);
largeImages.Images.Add("paste", Properties.Resources.Paste32);

ImageList mediumImages = new ImageList();
mediumImages.ImageSize = new Size(20, 20);
mediumImages.Images.Add("copy", Properties.Resources.Copy20);
mediumImages.Images.Add("paste", Properties.Resources.Paste20);

ImageList smallImages = new ImageList();
smallImages.ImageSize = new Size(16, 16);
smallImages.Images.Add("copy", Properties.Resources.Copy16);
smallImages.Images.Add("paste", Properties.Resources.Paste16);

// Assign to image provider
ToolStripExImageProvider provider = new ToolStripExImageProvider(ribbonControlAdv1);
provider.LargeImageList = largeImages;
provider.MediumImageList = mediumImages;
provider.SmallImageList = smallImages;

ribbonControlAdv1.ToolStripExImageProvider = provider;
```

## Resize Stages

Ribbon items resize in these stages as width decreases:

### Stage 1: Full Size
- All buttons at large size (32x32)
- Text below images
- Maximum visibility

### Stage 2: Medium Size
- Buttons reduce to medium (20x20)
- Text beside images
- Compact horizontal layout

### Stage 3: Small Size
- Buttons reduce to small (16x16)
- Text beside images
- Minimal horizontal layout

### Stage 4: Collapsed
- Items move to dropdown
- Group shows launcher button
- Click to show popup

## Launcher Button

Small button in group header that opens popup with group items.

### Adding Launcher Button

```csharp
// Add launcher button to group
homeGroup.LauncherStyle = LauncherStyle.SmallButton;

// Handle launcher click
homeGroup.LauncherClick += (s, e) =>
{
    // Show options dialog
    OptionsDialog optionsDialog = new OptionsDialog();
    optionsDialog.ShowDialog();
};
```

### Launcher Styles

```csharp
// Small button (default)
homeGroup.LauncherStyle = LauncherStyle.SmallButton;

// Large button
homeGroup.LauncherStyle = LauncherStyle.LargeButton;

// No launcher button
homeGroup.LauncherStyle = LauncherStyle.None;
```

## Complete Resize Example

```csharp
public partial class Form1 : RibbonForm
{
    public Form1()
    {
        InitializeComponent();
        SetupRibbonResize();
    }
    
    private void SetupRibbonResize()
    {
        // Set collapse behavior
        ribbonControlAdv1.CollapseBehavior = RibbonCollapseBehavior.Office2010;
        
        // Create image lists
        ImageList largeImages = CreateLargeImages();
        ImageList mediumImages = CreateMediumImages();
        ImageList smallImages = CreateSmallImages();
        
        // Setup image provider
        ToolStripExImageProvider imageProvider = new ToolStripExImageProvider(ribbonControlAdv1);
        imageProvider.LargeImageList = largeImages;
        imageProvider.MediumImageList = mediumImages;
        imageProvider.SmallImageList = smallImages;
        
        ribbonControlAdv1.ToolStripExImageProvider = imageProvider;
        
        // Add launcher buttons
        AddLauncherButtons();
    }
    
    private ImageList CreateLargeImages()
    {
        ImageList images = new ImageList();
        images.ImageSize = new Size(32, 32);
        images.ColorDepth = ColorDepth.Depth32Bit;
        
        // Load 32x32 images
        images.Images.Add("new", LoadImage("new_32.png"));
        images.Images.Add("open", LoadImage("open_32.png"));
        images.Images.Add("save", LoadImage("save_32.png"));
        
        return images;
    }
    
    private ImageList CreateMediumImages()
    {
        ImageList images = new ImageList();
        images.ImageSize = new Size(20, 20);
        images.ColorDepth = ColorDepth.Depth32Bit;
        
        // Load 20x20 images
        images.Images.Add("new", LoadImage("new_20.png"));
        images.Images.Add("open", LoadImage("open_20.png"));
        images.Images.Add("save", LoadImage("save_20.png"));
        
        return images;
    }
    
    private ImageList CreateSmallImages()
    {
        ImageList images = new ImageList();
        images.ImageSize = new Size(16, 16);
        images.ColorDepth = ColorDepth.Depth32Bit;
        
        // Load 16x16 images
        images.Images.Add("new", LoadImage("new_16.png"));
        images.Images.Add("open", LoadImage("open_16.png"));
        images.Images.Add("save", LoadImage("save_16.png"));
        
        return images;
    }
    
    private Image LoadImage(string fileName)
    {
        // Load from resources or file
        string path = Path.Combine(Application.StartupPath, "Images", fileName);
        return Image.FromFile(path);
    }
    
    private void AddLauncherButtons()
    {
        // Add launcher to clipboard group
        clipboardGroup.LauncherStyle = LauncherStyle.SmallButton;
        clipboardGroup.LauncherClick += (s, e) =>
        {
            MessageBox.Show("Clipboard Options", "Options");
        };
        
        // Add launcher to font group
        fontGroup.LauncherStyle = LauncherStyle.SmallButton;
        fontGroup.LauncherClick += (s, e) =>
        {
            // Open font dialog
            FontDialog fontDialog = new FontDialog();
            fontDialog.ShowDialog();
        };
    }
}
```

## Resize Events

Monitor ribbon resize behavior with events.

### SizeChanged Event

```csharp
ribbonControlAdv1.SizeChanged += (s, e) =>
{
    Console.WriteLine($"Ribbon size: {ribbonControlAdv1.Size}");
};
```

### Ribbon Height

```csharp
// Get current ribbon height
int ribbonHeight = ribbonControlAdv1.Height;

// Ribbon height varies by DisplayOption:
// - ShowTabsAndCommands: ~150 pixels
// - ShowTabs: ~30 pixels
// - AutoHide: 0 pixels (hidden)
```

## Minimum Ribbon Width

Set minimum width to prevent excessive collapse.

```csharp
// Set minimum form width
this.MinimumSize = new Size(800, 600);

// Prevents ribbon from collapsing beyond usability
```

## Best Practices

1. **Provide multiple image sizes:** Use ToolStripExImageProvider with 32x32, 20x20, 16x16 images

2. **Use Office2010 collapse behavior:** Cleaner appearance when groups collapse

3. **Add launcher buttons:** Provide access to group options when collapsed

4. **Test at different widths:** Ensure usability at minimum width

5. **Set minimum form size:** Prevent unusable collapsed state

6. **Organize by priority:** Place most important items leftmost (collapse last)

7. **Use consistent image quality:** Ensure images look good at all sizes

8. **Handle launcher clicks:** Provide meaningful actions for launcher buttons

## Troubleshooting

### Issue: Images disappear when resizing

**Cause:** ToolStripExImageProvider not configured or missing image sizes.

**Solution:**
```csharp
// Ensure all three image lists are set
ToolStripExImageProvider provider = new ToolStripExImageProvider(ribbonControlAdv1);
provider.LargeImageList = largeImages;   // Required
provider.MediumImageList = mediumImages; // Required
provider.SmallImageList = smallImages;   // Required

ribbonControlAdv1.ToolStripExImageProvider = provider;

// Verify ImageIndex matches across all lists
copyButton.ImageIndex = 0; // Must exist in all three lists
```

### Issue: Launcher button doesn't appear

**Cause:** LauncherStyle not set or Office2010 collapse behavior not used.

**Solution:**
```csharp
// Set collapse behavior
ribbonControlAdv1.CollapseBehavior = RibbonCollapseBehavior.Office2010;

// Set launcher style
homeGroup.LauncherStyle = LauncherStyle.SmallButton;

// Handle launcher click
homeGroup.LauncherClick += (s, e) => { /* Handle */ };
```

### Issue: Ribbon collapses too aggressively

**Cause:** Form width too small.

**Solution:**
```csharp
// Set minimum form size
this.MinimumSize = new Size(800, 600);

// Or reduce ribbon items
```

## Related Topics

- **Ribbon Controls** - Button sizes and display modes
- **Simplified Layout** - Alternative compact layout mode
- **Customization** - User control over ribbon appearance
