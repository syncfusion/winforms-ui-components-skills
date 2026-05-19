# Caption Customization

## Table of Contents
- [Overview](#overview)
- [Caption Labels](#caption-labels)
  - [Adding Labels Through Designer](#adding-labels-through-designer)
  - [Adding Labels Through Code](#adding-labels-through-code)
  - [Label Properties](#label-properties)
- [Caption Images](#caption-images)
  - [Adding Images Through Designer](#adding-images-through-designer)
  - [Adding Images Through Code](#adding-images-through-code)
  - [Image Properties](#image-properties)
- [Managing Collections](#managing-collections)
- [Common Patterns](#common-patterns)
- [Best Practices](#best-practices)

## Overview

MetroForm provides two primary ways to customize the caption bar with additional visual elements:

- **Caption Labels** - Text elements displayed in the caption bar
- **Caption Images** - Icons, logos, or other images displayed in the caption bar

Both elements can be added through the Visual Studio designer or programmatically through code.

## Caption Labels

Caption labels allow you to display additional text information in the caption bar beyond the standard form title.

### Adding Labels Through Designer

**Step 1:** Select your MetroForm in the designer

**Step 2:** In the Properties window, locate the `CaptionLabels` property

![Caption Labels Property](../images/CaptionLabels_Property.png)

**Step 3:** Click the ellipsis (...) button to open the CaptionLabel Collection Editor

![Caption Label Collection Editor](../images/CaptionLabel_CollectionEditor.png)

**Step 4:** Click "Add" to create a new caption label

**Step 5:** Configure the label properties in the right panel:
- **Text** - The text to display
- **Font** - Font family, size, and style
- **ForeColor** - Text color
- **Size** - Width and height of the label
- **Location** - Position within the caption bar
- **Name** - Identifier for the label

**Step 6:** Click "OK" to apply changes

### Adding Labels Through Code

Caption labels can be added dynamically at runtime or in the form constructor.

**C#:**
```csharp
// Create a new caption label
CaptionLabel captionLabel = new CaptionLabel();
captionLabel.Text = "This is MetroForm";
captionLabel.Font = new Font("Microsoft Sans Serif", 10F, FontStyle.Regular);
captionLabel.ForeColor = Color.White;
captionLabel.Size = new Size(400, 24);
captionLabel.Name = "CaptionLabel1";

// Add to the form's CaptionLabels collection
this.CaptionLabels.Add(captionLabel);
```

**VB.NET:**
```vb
' Create a new caption label
Dim captionLabel As New CaptionLabel()
captionLabel.Text = "This is MetroForm"
captionLabel.Font = New Font("Microsoft Sans Serif", 10F, FontStyle.Regular)
captionLabel.ForeColor = Color.White
captionLabel.Size = New Size(400, 24)
captionLabel.Name = "CaptionLabel1"

' Add to the form's CaptionLabels collection
Me.CaptionLabels.Add(captionLabel)
```

### Label Properties

Key properties for customizing caption labels:

| Property | Type | Description |
|----------|------|-------------|
| `Text` | string | The text content to display |
| `Font` | Font | Font family, size, and style |
| `ForeColor` | Color | Text color |
| `BackColor` | Color | Background color (usually transparent) |
| `Size` | Size | Width and height of the label |
| `Location` | Point | X, Y position in caption bar |
| `Name` | string | Identifier for the label |

## Caption Images

Caption images allow you to display icons, logos, or other visual elements in the caption bar.

### Adding Images Through Designer

**Step 1:** Select your MetroForm in the designer

**Step 2:** In the Properties window, locate the `CaptionImages` property

![Caption Images Property](../images/CaptionImages_Property.png)

**Step 3:** Click the ellipsis (...) button to open the CaptionImage Collection Editor

![Caption Image Collection Editor](../images/CaptionImage_CollectionEditor.png)

**Step 4:** Click "Add" to create a new caption image

**Step 5:** Configure the image properties:
- **Image** - Click the ellipsis to select an image from resources or file
- **Location** - Position (X, Y) within the caption bar
- **Size** - Width and height of the image
- **BackColor** - Background color (typically transparent)
- **Name** - Identifier for the image

**Step 6:** Click "OK" to apply changes

### Adding Images Through Code

Caption images can be added programmatically with full control over properties.

**C#:**
```csharp
// Create a new caption image
CaptionImage captionImage = new CaptionImage();
captionImage.BackColor = Color.Transparent;
captionImage.Image = Properties.Resources.AppIcon; // From resources
captionImage.Location = new Point(30, 5);
captionImage.Size = new Size(50, 50);
captionImage.Name = "CaptionImage1";

// Add to the form's CaptionImages collection
this.CaptionImages.Add(captionImage);
```

**VB.NET:**
```vb
' Create a new caption image
Dim captionImage As New CaptionImage()
captionImage.BackColor = Color.Transparent
captionImage.Image = My.Resources.AppIcon ' From resources
captionImage.Location = New Point(30, 5)
captionImage.Size = New Size(50, 50)
captionImage.Name = "CaptionImage1"

' Add to the form's CaptionImages collection
Me.CaptionImages.Add(captionImage)
```

**Loading Images from Different Sources:**

```csharp
// From embedded resources
captionImage.Image = Properties.Resources.Logo;

// From file path
captionImage.Image = Image.FromFile(@"C:\Images\logo.png");

// From stream
using (var stream = File.OpenRead(@"C:\Images\logo.png"))
{
    captionImage.Image = Image.FromStream(stream);
}

// From icon
captionImage.Image = SystemIcons.Information.ToBitmap();
```

### Image Properties

Key properties for customizing caption images:

| Property | Type | Description |
|----------|------|-------------|
| `Image` | Image | The image to display |
| `Location` | Point | X, Y position in caption bar |
| `Size` | Size | Width and height of the image |
| `BackColor` | Color | Background color (use Transparent) |
| `Name` | string | Identifier for the image |

## Managing Collections

### Accessing Collection Items

Both `CaptionLabels` and `CaptionImages` are collections that can be manipulated programmatically.

**C#:**
```csharp
// Access by index
CaptionLabel firstLabel = this.CaptionLabels[0];
CaptionImage firstImage = this.CaptionImages[0];

// Iterate through labels
foreach (CaptionLabel label in this.CaptionLabels)
{
    Console.WriteLine($"Label: {label.Text}");
}

// Iterate through images
foreach (CaptionImage image in this.CaptionImages)
{
    Console.WriteLine($"Image: {image.Name}");
}

// Get count
int labelCount = this.CaptionLabels.Count;
int imageCount = this.CaptionImages.Count;
```

### Adding Multiple Items

**C#:**
```csharp
// Add multiple labels
this.CaptionLabels.Add(new CaptionLabel { Text = "Label 1", ForeColor = Color.White });
this.CaptionLabels.Add(new CaptionLabel { Text = "Label 2", ForeColor = Color.White });
this.CaptionLabels.Add(new CaptionLabel { Text = "Label 3", ForeColor = Color.White });
```

### Removing Items

**C#:**
```csharp
// Remove by index
this.CaptionLabels.RemoveAt(0);

// Remove by reference
CaptionLabel labelToRemove = this.CaptionLabels[0];
this.CaptionLabels.Remove(labelToRemove);

// Clear all labels
this.CaptionLabels.Clear();

// Remove all images
this.CaptionImages.Clear();
```

### Updating Items at Runtime

**C#:**
```csharp
// Update label text dynamically
if (this.CaptionLabels.Count > 0)
{
    this.CaptionLabels[0].Text = "Updated Text";
    this.CaptionLabels[0].ForeColor = Color.Yellow;
}

// Update image dynamically
if (this.CaptionImages.Count > 0)
{
    this.CaptionImages[0].Image = Properties.Resources.NewIcon;
    this.CaptionImages[0].Size = new Size(40, 40);
}
```

## Common Patterns

### Pattern 1: Status Indicator Label

Display real-time status information in the caption bar:

**C#:**
```csharp
private CaptionLabel statusLabel;

public MainForm()
{
    InitializeComponent();
    
    // Create status label
    statusLabel = new CaptionLabel
    {
        Text = "Ready",
        Font = new Font("Segoe UI", 9F),
        ForeColor = Color.LimeGreen,
        Size = new Size(150, 24),
        Location = new Point(200, 10)
    };
    this.CaptionLabels.Add(statusLabel);
}

// Update status dynamically
private void UpdateStatus(string status, Color color)
{
    statusLabel.Text = status;
    statusLabel.ForeColor = color;
}
```

### Pattern 2: Company Logo with Label

Add branding to your application:

**C#:**
```csharp
public MainForm()
{
    InitializeComponent();
    
    // Add company logo
    CaptionImage logo = new CaptionImage
    {
        Image = Properties.Resources.CompanyLogo,
        Location = new Point(10, 8),
        Size = new Size(35, 35),
        BackColor = Color.Transparent
    };
    this.CaptionImages.Add(logo);
    
    // Add company name label
    CaptionLabel companyLabel = new CaptionLabel
    {
        Text = "Acme Corporation",
        Font = new Font("Segoe UI Semibold", 11F),
        ForeColor = Color.White,
        Location = new Point(50, 10)
    };
    this.CaptionLabels.Add(companyLabel);
}
```

### Pattern 3: User Information Display

Show logged-in user information:

**C#:**
```csharp
public MainForm(string userName, string userRole)
{
    InitializeComponent();
    
    // Add user avatar
    CaptionImage avatar = new CaptionImage
    {
        Image = LoadUserAvatar(userName),
        Location = new Point(this.Width - 150, 8),
        Size = new Size(30, 30),
        BackColor = Color.Transparent
    };
    this.CaptionImages.Add(avatar);
    
    // Add user name label
    CaptionLabel userLabel = new CaptionLabel
    {
        Text = $"{userName} ({userRole})",
        Font = new Font("Segoe UI", 9F),
        ForeColor = Color.White,
        Location = new Point(this.Width - 115, 12)
    };
    this.CaptionLabels.Add(userLabel);
}
```

### Pattern 4: Multiple Status Icons

Display multiple status indicators with icons:

**C#:**
```csharp
public MainForm()
{
    InitializeComponent();
    
    int startX = this.Width - 150;
    int iconSize = 20;
    int spacing = 30;
    
    // Connection status
    CaptionImage connectionIcon = new CaptionImage
    {
        Image = Properties.Resources.ConnectedIcon,
        Location = new Point(startX, 12),
        Size = new Size(iconSize, iconSize),
        BackColor = Color.Transparent,
        Name = "ConnectionIcon"
    };
    this.CaptionImages.Add(connectionIcon);
    
    // Notification status
    CaptionImage notificationIcon = new CaptionImage
    {
        Image = Properties.Resources.NotificationIcon,
        Location = new Point(startX + spacing, 12),
        Size = new Size(iconSize, iconSize),
        BackColor = Color.Transparent,
        Name = "NotificationIcon"
    };
    this.CaptionImages.Add(notificationIcon);
    
    // Settings icon
    CaptionImage settingsIcon = new CaptionImage
    {
        Image = Properties.Resources.SettingsIcon,
        Location = new Point(startX + spacing * 2, 12),
        Size = new Size(iconSize, iconSize),
        BackColor = Color.Transparent,
        Name = "SettingsIcon"
    };
    this.CaptionImages.Add(settingsIcon);
}
```

## Best Practices

### Positioning and Sizing

1. **Caption Bar Height** - Ensure `CaptionBarHeight` is sufficient for your content (minimum 30-35 pixels for small icons)
2. **Label/Image Sizes** - Keep sizes proportional to caption bar height (typically 60-80% of height)
3. **Spacing** - Leave adequate spacing between elements (10-15 pixels minimum)
4. **Right-Aligned Content** - Account for window control buttons (close, minimize, maximize) on the right

### Visual Consistency

1. **Font Choice** - Use system fonts like Segoe UI for consistency
2. **Color Contrast** - Ensure labels have sufficient contrast with caption bar background
3. **Image Format** - Use PNG with transparency for cleaner appearance
4. **Icon Sizes** - Use consistent icon sizes (16x16, 24x24, 32x32, or 48x48)

### Performance

1. **Image Resources** - Load images from embedded resources rather than files for better performance
2. **Collection Updates** - Minimize frequent additions/removals from collections
3. **Dynamic Updates** - Update existing items rather than removing and re-adding

### Code Organization

1. **Initialize in Constructor** - Set up caption elements in form constructor
2. **Use Named Items** - Give meaningful names to labels and images for easier reference
3. **Encapsulate Logic** - Create helper methods for common caption operations

## Troubleshooting

### Labels/Images Not Visible

**Problem:** Caption elements don't appear on the form.

**Solutions:**
- Verify `CaptionBarHeight` is sufficient for the content size
- Check that `ForeColor` contrasts with `CaptionBarColor`
- Ensure `Size` property is set (width and height > 0)
- Verify element is actually added to the collection
- Check that `Location` is within the caption bar bounds

### Images Appear Distorted

**Problem:** Caption images look stretched or compressed.

**Solutions:**
- Set `Size` property to match the source image aspect ratio
- Use images with appropriate resolution (avoid upscaling)
- Consider using vector-based images or multiple sizes

### Elements Overlap Window Controls

**Problem:** Caption elements cover minimize/maximize/close buttons.

**Solutions:**
- Calculate positions relative to form width, accounting for button space
- Reserve ~120-150 pixels on the right for window controls
- Use form resize events to reposition elements

### Collection Editor Changes Not Applied

**Problem:** Changes in designer collection editor don't appear.

**Solutions:**
- Click "OK" to save changes, not "Cancel"
- Rebuild the project
- Close and reopen the designer
- Check for code that clears collections in constructor
