# Images and Visual Styling

## Table of Contents
- [Overview](#overview)
- [Adding Images to Bars](#adding-images-to-bars)
- [Visual Styles](#visual-styles)
- [Appearance Customization](#appearance-customization)
- [Complete Examples](#complete-examples)

## Overview

NavigationView supports visual customization through:

1. **Images:** Add icons to Bars using ImageList
2. **Visual Styles:** Apply predefined themes (Office2007, Vista, Metro)
3. **Appearance Properties:** Customize size, colors, and layout

These features help NavigationView blend seamlessly with your application's design.

## Adding Images to Bars

Images enhance navigation by providing visual cues for different Bar types (folders, drives, categories).

### Setting Up ImageList

```csharp
// Create and populate ImageList
ImageList imageList1 = new ImageList();
imageList1.ImageSize = new Size(16, 16);
imageList1.Images.Add("computer", Properties.Resources.ComputerIcon);
imageList1.Images.Add("drive", Properties.Resources.DriveIcon);
imageList1.Images.Add("folder", Properties.Resources.FolderIcon);

// Assign to NavigationView
navigationView1.ImageList = imageList1;

// Create Bars with image indices
Bar computer = new Bar { Text = "This PC", ImageIndex = 0 };
Bar cDrive = new Bar { Text = "C:", ImageIndex = 1 };
Bar users = new Bar { Text = "Users", ImageIndex = 2 };

// Build hierarchy
cDrive.Bars.Add(users);
computer.Bars.Add(cDrive);
navigationView1.Bars.Add(computer);
navigationView1.SelectedBar = computer;
```

**Visual Result:** Each Bar displays its icon to the left of text in both breadcrumb and dropdowns.

### Dynamic Image Assignment

```csharp
// Access images by key
int folderIndex = imageList1.Images.IndexOfKey("folder");

// Set images based on content
private int GetImageIndexForPath(string path)
{
    if (path == "Computer") return 0;
    if (path.EndsWith(":")) return 1;
    if (Directory.Exists(path)) return 2;
    return -1; // No image
}

// Usage
Bar bar = new Bar 
{ 
    Text = "Projects",
    Tag = @"D:\Projects",
    ImageIndex = GetImageIndexForPath(@"D:\Projects")
};
```

**Image Display:** Selected Bar's image shows in breadcrumb; all Bars show images in dropdowns. ImageIndex of -1 shows no image.

## Visual Styles

Apply predefined visual themes to match your application's appearance.

### Available Visual Styles

NavigationView supports these styles:

1. **Office2007** - Modern Office look with gradients
2. **Vista** - Windows Vista theme
3. **Metro** - Flat, modern design

### Applying Visual Styles

```csharp
using Syncfusion.Windows.Forms.Tools.Navigation;

// Office 2007 style
navigationView1.VisualStyle = VisualStyles.Office2007;

// Vista style
navigationView1.VisualStyle = VisualStyles.Vista;

// Metro style
navigationView1.VisualStyle = VisualStyles.Metro;
```

### Visual Style Examples

```csharp
// Office2007: Professional with gradients
nav.VisualStyle = VisualStyles.Office2007;

// Vista: Windows Vista theme
nav.VisualStyle = VisualStyles.Vista;

// Metro: Flat, modern design
nav.VisualStyle = VisualStyles.Metro;
```

**Characteristics:**
- **Office2007:** Smooth gradients, professional, good for business apps
- **Vista:** Windows-familiar, subtle styling
- **Metro:** Flat, minimalist, contemporary

Set in designer via Properties window → VisualStyle, or apply programmatically for consistent theming across Syncfusion controls.

## Appearance Customization

```csharp
// Size and layout
navigationView1.Width = 600;
navigationView1.Height = 28;  // Standard: 25-28px; Touch: 30-35px
navigationView1.Dock = DockStyle.Top;

// Colors and font (may be overridden by VisualStyle)
navigationView1.BackColor = Color.White;
navigationView1.ForeColor = Color.Black;
navigationView1.Font = new Font("Segoe UI", 9F);
```

**Tip:** Allow 400-600px width for typical use. Apply VisualStyle first, then customize appearance.

## Complete Example: Styled File Browser

```csharp
private void CreateStyledFileBrowser()
{
    NavigationView nav = new NavigationView();
    nav.Width = 600;
    nav.Height = 28;
    nav.VisualStyle = VisualStyles.Office2007;
    
    // Setup ImageList
    ImageList imgList = new ImageList { ImageSize = new Size(16, 16) };
    imgList.Images.Add("computer", Properties.Resources.Computer);
    imgList.Images.Add("drive", Properties.Resources.Drive);
    imgList.Images.Add("folder", Properties.Resources.Folder);
    imgList.Images.Add("user", Properties.Resources.User);
    nav.ImageList = imgList;
    
    // Get indices
    int computerIdx = imgList.Images.IndexOfKey("computer");
    int driveIdx = imgList.Images.IndexOfKey("drive");
    int folderIdx = imgList.Images.IndexOfKey("folder");
    int userIdx = imgList.Images.IndexOfKey("user");
    
    // Build hierarchy
    Bar computer = new Bar { Text = "This PC", ImageIndex = computerIdx };
    Bar cDrive = new Bar { Text = "C:", ImageIndex = driveIdx };
    Bar users = new Bar { Text = "Users", ImageIndex = folderIdx };
    Bar currentUser = new Bar { Text = Environment.UserName, ImageIndex = userIdx };
    Bar documents = new Bar { Text = "Documents", ImageIndex = folderIdx };
    
    currentUser.Bars.Add(documents);
    users.Bars.Add(currentUser);
    cDrive.Bars.Add(users);
    computer.Bars.Add(cDrive);
    
    nav.Bars.Add(computer);
    nav.SelectedBar = computer;
    
    this.Controls.Add(nav);
}
```

## Best Practices

**Images:**
- Use 16x16 icons; ensure consistent style and high contrast
- Provide fallback for missing ImageIndex (-1)
- Share ImageList across controls for efficiency

**Visual Styles:**
- Match application theme; set VisualStyle during initialization
- Apply consistently across Syncfusion controls

**Accessibility:**
- Ensure text readability; don't rely solely on icons
- Test color contrast for visual impairments
