# Getting Started with Navigation Drawer

This guide covers the essential steps to add, configure, and use the Syncfusion Windows Forms Navigation Drawer control in your application.

## Assembly Deployment

Before using the NavigationDrawer control, you need to add the required assembly references to your project.

### Required Assemblies

The NavigationDrawer control requires the following assemblies:

- `Syncfusion.Grid.Base.dll`
- `Syncfusion.Grid.Windows.dll`
- `Syncfusion.Shared.Base.dll`
- `Syncfusion.Shared.Windows.dll`
- `Syncfusion.Tools.Base.dll`
- `Syncfusion.Tools.Windows.dll`

### NuGet Package Installation

You can install the required NuGet package using the NuGet Package Manager:

1. Right-click on your project in Solution Explorer
2. Select "Manage NuGet Packages..."
3. Search for "Syncfusion.Tools.Windows"
4. Install the package

**Package Manager Console:**
```powershell
Install-Package Syncfusion.Tools.Windows
```

**NuGet CLI:**
```bash
nuget install Syncfusion.Tools.Windows
```

For more details on installing NuGet packages in Windows Forms applications, refer to the [Syncfusion installation documentation](https://help.syncfusion.com/windowsforms/installation/install-nuget-packages).

## Adding Control via Designer

The NavigationDrawer control can be added to your form using the Visual Studio designer.

### Steps to Add via Designer

1. Open your Windows Forms form in the designer
2. Locate the **NavigationDrawer** control in the Toolbox
3. Drag and drop it onto your form

When added via the designer, the required assembly references are added automatically.

![Navigation Drawer added by designer](../assets/wf-navigation-drawer-control-added-by-designer.png)

### Adding Items via Smart Tags

After adding the NavigationDrawer control, you can add header and menu items using the smart tags:

1. Click on the NavigationDrawer control
2. Click the smart tag (arrow icon) in the upper-right corner
3. Click on "Edit Items" in the smart tag panel
4. Add DrawerHeader and DrawerMenuItem objects

![Navigation Drawer items added by designer](../assets/wf-navigation-drawer-control-items-added-by-designer.png)

## Adding Control Manually in C#

You can also add the NavigationDrawer control programmatically in your C# code.

### Step 1: Add Assembly References

If you're adding the control manually, ensure the required assemblies are referenced in your project (see [Assembly Deployment](#assembly-deployment) section).

### Step 2: Import Namespace

Add the following using directive at the top of your code file:

```csharp
using Syncfusion.Windows.Forms.Tools;
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms.Tools
```

### Step 3: Create NavigationDrawer Instance

Create a NavigationDrawer instance and add it to your form's control collection:

```csharp
NavigationDrawer navigationDrawer1 = new NavigationDrawer();
this.Controls.Add(navigationDrawer1);
```

**VB.NET:**
```vb
Dim navigationDrawer1 As NavigationDrawer = New NavigationDrawer()
Me.Controls.Add(navigationDrawer1)
```

### Step 4: Set Drawer Dimensions

Configure the width and height of the drawer panel:

```csharp
this.navigationDrawer1.DrawerWidth = this.Width / 4;
this.navigationDrawer1.DrawerHeight = this.Height;
```

**VB.NET:**
```vb
Me.navigationDrawer1.DrawerWidth = Me.Width / 4
Me.navigationDrawer1.DrawerHeight = Me.Height
```

**Tips:**
- Set `DrawerWidth` relative to form width for responsive layouts
- Set `DrawerHeight` to match form height for full-height drawers
- Adjust dimensions based on drawer position (Left/Right use width, Top/Bottom use height)

## Adding Header

The DrawerHeader provides a header section at the top of the drawer panel.

### Create and Add DrawerHeader

```csharp
DrawerHeader drawerHeader1 = new DrawerHeader();
drawerHeader1.Text = "Navigation Menu";
this.navigationDrawer1.Items.Add(drawerHeader1);
```

**VB.NET:**
```vb
Dim drawerHeader1 As DrawerHeader = New DrawerHeader()
drawerHeader1.Text = "Navigation Menu"
Me.navigationDrawer1.Items.Add(drawerHeader1)
```

### Header with Image

```csharp
DrawerHeader header = new DrawerHeader();
header.Text = "My Application";
header.Image = Image.FromFile(@"../../logo.png");
this.navigationDrawer1.Items.Add(header);
```

![Navigation Drawer with header](../assets/wf-navigation-drawer-header-added-by-code.png)

## Adding Menu Items

Menu items are the primary interactive elements in the drawer.

### Create and Add DrawerMenuItems

```csharp
DrawerMenuItem drawerMenuItem1 = new DrawerMenuItem();
drawerMenuItem1.Text = "Home";

DrawerMenuItem drawerMenuItem2 = new DrawerMenuItem();
drawerMenuItem2.Text = "Products";

DrawerMenuItem drawerMenuItem3 = new DrawerMenuItem();
drawerMenuItem3.Text = "Services";

DrawerMenuItem drawerMenuItem4 = new DrawerMenuItem();
drawerMenuItem4.Text = "About";

DrawerMenuItem drawerMenuItem5 = new DrawerMenuItem();
drawerMenuItem5.Text = "Contact";

this.navigationDrawer1.Items.Add(drawerMenuItem1);
this.navigationDrawer1.Items.Add(drawerMenuItem2);
this.navigationDrawer1.Items.Add(drawerMenuItem3);
this.navigationDrawer1.Items.Add(drawerMenuItem4);
this.navigationDrawer1.Items.Add(drawerMenuItem5);
```

**VB.NET:**
```vb
Dim drawerMenuItem1 As DrawerMenuItem = New DrawerMenuItem()
drawerMenuItem1.Text = "Home"

Dim drawerMenuItem2 As DrawerMenuItem = New DrawerMenuItem()
drawerMenuItem2.Text = "Products"

Dim drawerMenuItem3 As DrawerMenuItem = New DrawerMenuItem()
drawerMenuItem3.Text = "Services"

Me.navigationDrawer1.Items.Add(drawerMenuItem1)
Me.navigationDrawer1.Items.Add(drawerMenuItem2)
Me.navigationDrawer1.Items.Add(drawerMenuItem3)
```

![Navigation Drawer with menu items](../assets/wf-navigation-drawer-menuitems-added-by-code.png)

## Setting Images to Menu Items

Enhance menu items with icons to improve visual recognition.

### Add Image to MenuItem

```csharp
// Set images using file paths
this.drawerMenuItem1.Image = Image.FromFile(@"../../Home-Icon.png");
this.drawerMenuItem2.Image = Image.FromFile(@"../../Products-Icon.png");
this.drawerMenuItem3.Image = Image.FromFile(@"../../Services-Icon.png");
```

**VB.NET:**
```vb
Me.drawerMenuItem1.Image = Image.FromFile("../../Home-Icon.png")
Me.drawerMenuItem2.Image = Image.FromFile("../../Products-Icon.png")
Me.drawerMenuItem3.Image = Image.FromFile("../../Services-Icon.png")
```

### Using Embedded Resources

```csharp
// Load images from embedded resources
this.drawerMenuItem1.Image = Properties.Resources.HomeIcon;
this.drawerMenuItem2.Image = Properties.Resources.ProductsIcon;
this.drawerMenuItem3.Image = Properties.Resources.ServicesIcon;
```

![Navigation Drawer menu item with image](../assets/setting_Image_to_an_Item.png)

**Best Practices:**
- Use consistent icon sizes (e.g., 24x24 or 32x32 pixels)
- Prefer embedded resources over file paths for deployment
- Use PNG format with transparency for clean appearance
- Provide high-DPI versions for better display on high-resolution screens

## Positioning Text and Image

Control the layout of text and images within menu items.

### TextAlign Property

Set the alignment of text within the menu item:

```csharp
// Text alignment options
this.drawerMenuItem1.TextAlign = TextAlignment.Left;
this.drawerMenuItem2.TextAlign = TextAlignment.Center;
this.drawerMenuItem3.TextAlign = TextAlignment.Right;
```

**TextAlignment Options:**
- `TextAlignment.Left` - Text aligns to the left
- `TextAlignment.Center` - Text aligns to the center
- `TextAlignment.Right` - Text aligns to the right

### TextImageRelation Property

Define the relationship between text and image:

```csharp
// Set text and image relationship
this.drawerMenuItem1.TextAlign = TextAlignment.Center;
this.drawerMenuItem1.TextImageRelation = TextImageRelation.ImageBeforeText;
```

**VB.NET:**
```vb
Me.drawerMenuItem1.TextAlign = TextAlignment.Center
Me.drawerMenuItem1.TextImageRelation = TextImageRelation.TextBeforeImage
```

**TextImageRelation Options:**
- `TextImageRelation.ImageBeforeText` - Image on the left, text on the right
- `TextImageRelation.TextBeforeImage` - Text on the left, image on the right
- `TextImageRelation.ImageAboveText` - Image above text
- `TextImageRelation.TextAboveImage` - Text above image
- `TextImageRelation.Overlay` - Text overlays the image

![Text and Image positioning](../assets/positioning_text_and_image.png)

## Sidebar Placement

Configure where the drawer panel slides from using the Position property.

### Setting Drawer Position

```csharp
// Position the drawer on the left side
this.navigationDrawer1.Position = SlidePosition.Left;
```

**VB.NET:**
```vb
Me.navigationDrawer1.Position = SlidePosition.Left
```

### Available Positions

**Left Position:**
```csharp
this.navigationDrawer1.Position = SlidePosition.Left;
```
Drawer slides from the left edge of the form.

![Left side drawer](../assets/wf-navigation-drawer-left-side-drawer-view.png)

**Right Position:**
```csharp
this.navigationDrawer1.Position = SlidePosition.Right;
```
Drawer slides from the right edge of the form.

![Right side drawer](../assets/wf-navigation-drawer-right-side-drawer-view.png)

**Top Position:**
```csharp
this.navigationDrawer1.Position = SlidePosition.Top;
```
Drawer slides from the top edge of the form.

![Top drawer](../assets/wf-navigation-drawer-top-view-drawer-view.png)

**Bottom Position:**
```csharp
this.navigationDrawer1.Position = SlidePosition.Bottom;
```
Drawer slides from the bottom edge of the form.

![Bottom drawer](../assets/wf-navigation-drawer-bottom-side-drawer-view.png)

### Position Selection Guidelines

- **Left/Right**: Best for vertical menu lists, most common for application navigation
- **Top**: Suitable for toolbars or notification panels
- **Bottom**: Good for contextual menus or quick actions

## Complete Example

Here's a complete example that puts everything together:

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class MainForm : Form
{
    private NavigationDrawer navigationDrawer1;
    
    public MainForm()
    {
        InitializeComponent();
        InitializeNavigationDrawer();
    }
    
    private void InitializeNavigationDrawer()
    {
        // Create NavigationDrawer
        navigationDrawer1 = new NavigationDrawer();
        navigationDrawer1.DrawerWidth = 250;
        navigationDrawer1.DrawerHeight = this.Height;
        navigationDrawer1.Position = SlidePosition.Left;
        
        // Add header
        DrawerHeader header = new DrawerHeader();
        header.Text = "Main Menu";
        navigationDrawer1.Items.Add(header);
        
        // Add menu items with images
        string[] menuTexts = { "Dashboard", "Reports", "Settings", "Help" };
        string[] imagePaths = { "dashboard.png", "reports.png", "settings.png", "help.png" };
        
        for (int i = 0; i < menuTexts.Length; i++)
        {
            DrawerMenuItem item = new DrawerMenuItem();
            item.Text = menuTexts[i];
            item.Image = Image.FromFile($@"../../{imagePaths[i]}");
            item.TextAlign = TextAlignment.Left;
            item.TextImageRelation = TextImageRelation.ImageBeforeText;
            navigationDrawer1.Items.Add(item);
        }
        
        // Add to form
        this.Controls.Add(navigationDrawer1);
    }
}
```

## Next Steps

- **Configure transitions:** See [drawer-features.md](drawer-features.md) for transition types (SlideOnTop, Push, Reveal)
- **Apply themes:** See [customization.md](customization.md) for styling and theming options
- **Handle events:** See [events.md](events.md) for drawer event handling
- **Advanced scenarios:** See [advanced-usage.md](advanced-usage.md) for complex configurations
