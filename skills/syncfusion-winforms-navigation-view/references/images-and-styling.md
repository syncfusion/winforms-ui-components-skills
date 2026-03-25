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

**Step 1: Create and Populate ImageList**

```csharp
// Create ImageList
ImageList imageList1 = new ImageList();
imageList1.ImageSize = new Size(16, 16); // Standard icon size
imageList1.ColorDepth = ColorDepth.Depth32Bit;

// Add images from resources
imageList1.Images.Add("computer", Properties.Resources.ComputerIcon);
imageList1.Images.Add("drive", Properties.Resources.DriveIcon);
imageList1.Images.Add("folder", Properties.Resources.FolderIcon);
imageList1.Images.Add("document", Properties.Resources.DocumentIcon);

// Or add from files
imageList1.Images.Add(new Bitmap(@"icons\computer.png"));
imageList1.Images.Add(new Bitmap(@"icons\drive.png"));
imageList1.Images.Add(new Bitmap(@"icons\folder.png"));
```

**Step 2: Assign ImageList to NavigationView**

```csharp
// Assign to NavigationView
navigationView1.ImageList = imageList1;
```

**Step 3: Set ImageIndex on Bars**

```csharp
// Create Bars with image indices
Bar computer = new Bar 
{ 
    Text = "This PC",
    ImageIndex = 0  // Computer icon
};

Bar cDrive = new Bar 
{ 
    Text = "Local Disk (C:)",
    ImageIndex = 1  // Drive icon
};

Bar documents = new Bar 
{ 
    Text = "Documents",
    ImageIndex = 2  // Folder icon
};
```

### Complete Image Setup Example

```csharp
private void SetupNavigationWithImages()
{
    // Create NavigationView
    NavigationView navigationView1 = new NavigationView();
    navigationView1.Width = 500;
    navigationView1.Height = 25;
    navigationView1.Location = new Point(20, 20);
    
    // Create and setup ImageList
    ImageList imageList = new ImageList();
    imageList.ImageSize = new Size(16, 16);
    
    // Add images (index 0, 1, 2, ...)
    imageList.Images.Add(Properties.Resources.ComputerIcon);  // 0
    imageList.Images.Add(Properties.Resources.DriveIcon);     // 1
    imageList.Images.Add(Properties.Resources.FolderIcon);    // 2
    
    // Assign ImageList
    navigationView1.ImageList = imageList;
    
    // Create Bars with images
    Bar computer = new Bar { Text = "Computer", ImageIndex = 0 };
    Bar cDrive = new Bar { Text = "C:", ImageIndex = 1 };
    Bar dDrive = new Bar { Text = "D:", ImageIndex = 1 };
    
    Bar users = new Bar { Text = "Users", ImageIndex = 2 };
    Bar programFiles = new Bar { Text = "Program Files", ImageIndex = 2 };
    
    // Build hierarchy
    cDrive.Bars.AddRange(new Bar[] { users, programFiles });
    computer.Bars.AddRange(new Bar[] { cDrive, dDrive });
    
    navigationView1.Bars.Add(computer);
    navigationView1.SelectedBar = computer;
    
    this.Controls.Add(navigationView1);
}
```

**Visual Result:** Each Bar displays its icon to the left of the text in both the breadcrumb and dropdowns.

### Using Named Images

Access images by key instead of index:

```csharp
// Add images with keys
imageList1.Images.Add("computer", Properties.Resources.ComputerIcon);
imageList1.Images.Add("drive", Properties.Resources.DriveIcon);
imageList1.Images.Add("folder", Properties.Resources.FolderIcon);

// Get index by key
int computerIndex = imageList1.Images.IndexOfKey("computer");
int driveIndex = imageList1.Images.IndexOfKey("drive");
int folderIndex = imageList1.Images.IndexOfKey("folder");

// Use in Bars
Bar computer = new Bar { Text = "Computer", ImageIndex = computerIndex };
Bar cDrive = new Bar { Text = "C:", ImageIndex = driveIndex };
```

### Dynamic Image Assignment

Set images based on Bar type or content:

```csharp
private int GetImageIndexForPath(string path)
{
    if (path == "Computer")
        return 0; // Computer icon
    
    if (path.EndsWith(":") || path.EndsWith(":\\"))
        return 1; // Drive icon
    
    if (Directory.Exists(path))
        return 2; // Folder icon
    
    return 3; // Default icon
}

// Usage
Bar bar = new Bar 
{ 
    Text = "Projects",
    Tag = @"D:\Projects",
    ImageIndex = GetImageIndexForPath(@"D:\Projects")
};
```

### Image Display Behavior

**Where images appear:**
- **Breadcrumb path:** Selected Bar's image shows at the far left
- **Dropdown lists:** Each Bar displays its image in the dropdown
- **History dropdown:** Previously visited Bars show their images

**Image Rules:**
- Only the **selected Bar's image** displays in the main breadcrumb area
- All Bars show images in dropdown menus
- ImageIndex of -1 or out of range shows no image

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

### Office2007 Style

Provides a polished, professional appearance with subtle gradients:

```csharp
NavigationView nav = new NavigationView();
nav.VisualStyle = VisualStyles.Office2007;
nav.Width = 500;
nav.Height = 25;

// Create structure
Bar computer = new Bar { Text = "Computer" };
Bar cDrive = new Bar { Text = "C:" };
computer.Bars.Add(cDrive);

nav.Bars.Add(computer);
nav.SelectedBar = computer;
```

**Characteristics:**
- Smooth gradients
- Professional appearance
- Good for business applications
- Works well with Office-style interfaces

### Vista Style

Windows Vista-inspired theme:

```csharp
NavigationView nav = new NavigationView();
nav.VisualStyle = VisualStyles.Vista;
nav.Width = 500;
nav.Height = 25;

// Create structure
Bar computer = new Bar { Text = "Computer" };
nav.Bars.Add(computer);
nav.SelectedBar = computer;
```

**Characteristics:**
- Windows Vista look and feel
- Familiar to Windows users
- Subtle styling

### Metro Style

Modern, flat design inspired by Windows 8/10:

```csharp
NavigationView nav = new NavigationView();
nav.VisualStyle = VisualStyles.Metro;
nav.Width = 500;
nav.Height = 25;

// Create structure
Bar computer = new Bar { Text = "Computer" };
nav.Bars.Add(computer);
nav.SelectedBar = computer;
```

**Characteristics:**
- Flat, minimalist design
- Modern appearance
- Clean lines, no gradients
- Good for contemporary applications

### Setting Visual Style at Design Time

1. **Select NavigationView** in the designer
2. **Locate VisualStyle** property in Properties window
3. **Select style** from dropdown (Office2007, Vista, Metro)
4. **View changes** immediately in designer

### Matching Application Theme

Apply the same visual style across multiple controls:

```csharp
// Set consistent style for all Syncfusion controls
private void ApplyOffice2007Theme()
{
    navigationView1.VisualStyle = VisualStyles.Office2007;
    
    // Apply to other Syncfusion controls if present
    // button1.Appearance = ButtonAppearance.Office2007;
    // toolStrip1.VisualStyle = ToolStripVisualStyle.Office2007;
}

private void ApplyMetroTheme()
{
    navigationView1.VisualStyle = VisualStyles.Metro;
    
    // Apply to other controls
    // button1.Appearance = ButtonAppearance.Metro;
}
```

## Appearance Customization

### Size and Layout

```csharp
// Control dimensions
navigationView1.Width = 600;
navigationView1.Height = 28; // Recommended: 25-30 pixels

// Position
navigationView1.Location = new Point(10, 10);

// Docking
navigationView1.Dock = DockStyle.Top; // Full width at top of form
```

### Recommended Heights

| Height | Use Case |
|--------|----------|
| 21-23px | Compact interfaces |
| 25-28px | Standard (recommended) |
| 30-35px | Touch-friendly interfaces |

### Minimum Width Considerations

NavigationView dynamically adjusts content, but consider:
- Allow at least 200px for simple hierarchies
- 400-600px recommended for typical use
- Use Dock = DockStyle.Top for responsive width

### Custom Appearance Properties

```csharp
// Background and text colors (if supported)
navigationView1.BackColor = Color.White;
navigationView1.ForeColor = Color.Black;

// Font customization
navigationView1.Font = new Font("Segoe UI", 9F);
```

**Note:** Visual style may override some appearance properties. Apply VisualStyle first, then customize if needed.

## Complete Examples

### Example 1: File Browser with Icons and Office2007 Style

```csharp
private void CreateStyledFileBrowser()
{
    // Create NavigationView
    NavigationView nav = new NavigationView();
    nav.Width = 600;
    nav.Height = 28;
    nav.Location = new Point(10, 10);
    nav.VisualStyle = VisualStyles.Office2007;
    nav.ShowHistoryButtons = true;
    
    // Setup ImageList
    ImageList imgList = new ImageList();
    imgList.ImageSize = new Size(16, 16);
    imgList.Images.Add("computer", Properties.Resources.Computer);
    imgList.Images.Add("drive", Properties.Resources.Drive);
    imgList.Images.Add("folder", Properties.Resources.Folder);
    imgList.Images.Add("user", Properties.Resources.User);
    
    nav.ImageList = imgList;
    
    // Get image indices
    int computerIdx = imgList.Images.IndexOfKey("computer");
    int driveIdx = imgList.Images.IndexOfKey("drive");
    int folderIdx = imgList.Images.IndexOfKey("folder");
    int userIdx = imgList.Images.IndexOfKey("user");
    
    // Create hierarchy with images
    Bar computer = new Bar 
    { 
        Text = "This PC",
        ImageIndex = computerIdx
    };
    
    Bar cDrive = new Bar 
    { 
        Text = "Local Disk (C:)",
        ImageIndex = driveIdx
    };
    
    Bar users = new Bar 
    { 
        Text = "Users",
        ImageIndex = folderIdx
    };
    
    Bar currentUser = new Bar 
    { 
        Text = Environment.UserName,
        ImageIndex = userIdx
    };
    
    Bar documents = new Bar 
    { 
        Text = "Documents",
        ImageIndex = folderIdx
    };
    
    Bar downloads = new Bar 
    { 
        Text = "Downloads",
        ImageIndex = folderIdx
    };
    
    // Build hierarchy
    currentUser.Bars.AddRange(new Bar[] { documents, downloads });
    users.Bars.Add(currentUser);
    cDrive.Bars.Add(users);
    computer.Bars.Add(cDrive);
    
    nav.Bars.Add(computer);
    nav.SelectedBar = computer;
    
    this.Controls.Add(nav);
}
```

### Example 2: Metro Style Document Navigator

```csharp
private void CreateMetroDocumentNavigator()
{
    // Create NavigationView with Metro style
    NavigationView nav = new NavigationView();
    nav.Dock = DockStyle.Top;
    nav.Height = 30;
    nav.VisualStyle = VisualStyles.Metro;
    
    // Setup icons
    ImageList icons = new ImageList();
    icons.ImageSize = new Size(16, 16);
    icons.Images.Add("home", Properties.Resources.HomeIcon);
    icons.Images.Add("folder", Properties.Resources.FolderIcon);
    icons.Images.Add("document", Properties.Resources.DocumentIcon);
    
    nav.ImageList = icons;
    
    // Create structure
    Bar home = new Bar 
    { 
        Text = "Documents",
        ImageIndex = 0
    };
    
    Bar projects = new Bar 
    { 
        Text = "Projects",
        ImageIndex = 1
    };
    
    Bar project1 = new Bar 
    { 
        Text = "WebApp",
        ImageIndex = 1
    };
    
    Bar project2 = new Bar 
    { 
        Text = "MobileApp",
        ImageIndex = 1
    };
    
    projects.Bars.AddRange(new Bar[] { project1, project2 });
    home.Bars.Add(projects);
    
    nav.Bars.Add(home);
    nav.SelectedBar = home;
    
    this.Controls.Add(nav);
}
```

### Example 3: Dynamic Image Loading

```csharp
private void CreateDynamicImageNavigation()
{
    NavigationView nav = new NavigationView();
    nav.Width = 600;
    nav.Height = 28;
    nav.VisualStyle = VisualStyles.Office2007;
    
    // Create ImageList with system icons
    ImageList imgList = new ImageList();
    imgList.ImageSize = new Size(16, 16);
    
    // Add icons for different folder types
    imgList.Images.Add("default", SystemIcons.Application.ToBitmap());
    imgList.Images.Add("folder", SystemIcons.Shield.ToBitmap());
    
    nav.ImageList = imgList;
    
    // Create root
    Bar computer = new Bar { Text = "Computer", ImageIndex = 0 };
    nav.Bars.Add(computer);
    
    // Dynamically load drives with icons
    foreach (DriveInfo drive in DriveInfo.GetDrives())
    {
        if (drive.IsReady)
        {
            Bar driveBar = new Bar
            {
                Text = $"{drive.Name} ({drive.VolumeLabel})",
                Tag = drive.RootDirectory.FullName,
                ImageIndex = 0
            };
            
            computer.Bars.Add(driveBar);
        }
    }
    
    nav.SelectedBar = computer;
    
    // Load folders with appropriate icons on demand
    nav.BarPopup += (s, e) =>
    {
        if (e.CurrentBar.Bars.Count == 0 && e.CurrentBar.Tag is string path)
        {
            LoadFoldersWithIcons(e.CurrentBar, path);
        }
    };
    
    this.Controls.Add(nav);
}

private void LoadFoldersWithIcons(Bar parentBar, string path)
{
    try
    {
        string[] folders = Directory.GetDirectories(path);
        
        foreach (string folder in folders)
        {
            Bar childBar = new Bar
            {
                Text = Path.GetFileName(folder),
                Tag = folder,
                ImageIndex = 1 // Folder icon
            };
            
            parentBar.Bars.Add(childBar);
        }
    }
    catch
    {
        // Handle access errors
    }
}
```

## Best Practices

### Images
1. **Use 16x16 icons:** Standard size for most applications
2. **Consistent style:** Use icons from the same icon set
3. **Meaningful icons:** Choose icons that clearly represent content type
4. **High contrast:** Ensure icons are visible on different backgrounds
5. **Fallback handling:** Provide default icon for missing/invalid ImageIndex

### Visual Styles
1. **Match application theme:** Use style consistent with rest of UI
2. **Set early:** Apply VisualStyle during initialization before showing form
3. **User preference:** Consider allowing users to choose theme
4. **Consistency:** Apply same style to all Syncfusion controls in application

### Performance
1. **Reuse ImageList:** Share ImageList across multiple NavigationViews if possible
2. **Optimize image size:** Don't use oversized images scaled down
3. **Lazy load images:** Load images on demand for large hierarchies

### Accessibility
1. **Text clarity:** Ensure text is readable with chosen visual style
2. **Icon meaning:** Don't rely solely on icons; include clear text labels
3. **Color contrast:** Test visibility for users with visual impairments

## Troubleshooting

**Issue:** Images not appearing
- **Check:** ImageList is assigned to NavigationView.ImageList
- **Check:** ImageIndex values are within ImageList.Images.Count range
- **Check:** Images are properly loaded into ImageList

**Issue:** ImageIndex shows wrong image
- **Check:** Index corresponds to correct image in collection
- **Check:** Images added to ImageList in expected order

**Issue:** Visual style not applied
- **Check:** VisualStyle property set after control initialization
- **Check:** Namespace includes `using Syncfusion.Windows.Forms.Tools.Navigation;`
- **Check:** Syncfusion assemblies are correct version

**Issue:** Custom colors ignored
- **Check:** Visual style may override custom colors
- **Solution:** Set VisualStyle first, then apply custom colors

## Next Steps

- Learn about edit mode and popup customization in [advanced-features.md](advanced-features.md)
- Review complete examples in parent SKILL.md
