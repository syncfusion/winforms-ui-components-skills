# Getting Started with Windows Forms Carousel

This guide covers the installation, setup, and initial configuration of the Syncfusion WinForms Carousel control for creating 3D circular conveyors with rotating objects.

## Assembly Deployment

### Required Assemblies

The Carousel control requires the following assemblies:

- `Syncfusion.Grid.Base.dll`
- `Syncfusion.Grid.Windows.dll`
- `Syncfusion.Shared.Base.dll`
- `Syncfusion.Shared.Windows.dll`
- `Syncfusion.Tools.Base.dll`
- `Syncfusion.Tools.Windows.dll`

### NuGet Installation

Install the Carousel control via NuGet Package Manager:

```bash
Install-Package Syncfusion.Tools.Windows
```

Or search for "Syncfusion.Tools.Windows" in the NuGet Package Manager UI.

**Package Details:**
- Package ID: `Syncfusion.Tools.Windows`
- Contains: Carousel and other Tools controls
- Dependencies: Automatically installs required Syncfusion assemblies

### Manual Assembly Reference

If not using NuGet:

1. Right-click project → Add Reference
2. Browse to Syncfusion installation folder (typically `C:\Program Files (x86)\Syncfusion\Essential Studio\<version>\Assemblies`)
3. Add all six assemblies listed above
4. Ensure "Copy Local" is set to True

## Adding Carousel Control via Designer

### Step-by-Step Designer Setup

**Step 1: Create a Windows Forms Project**

1. Open Visual Studio
2. Create new project → Windows Forms App (.NET Framework or .NET 6.0+)
3. Name your project (e.g., "CarouselDemo")

**Step 2: Add Carousel to Toolbox**

1. Open the designer view (Form1.cs [Design])
2. Right-click Toolbox → Choose Items
3. Browse to Syncfusion.Tools.Windows.dll
4. Check "Carousel" in the list
5. Click OK

**Step 3: Drag Carousel to Form**

1. Locate Carousel in Toolbox (under "Syncfusion Controls" or "All Windows Forms")
2. Drag and drop onto your form
3. The following assemblies will be added automatically:
   - Syncfusion.Grid.Base
   - Syncfusion.Grid.Windows
   - Syncfusion.Shared.Base
   - Syncfusion.Shared.Windows
   - Syncfusion.Tools.Base
   - Syncfusion.Tools.Windows

**Step 4: Configure Properties**

In the Properties window, set:
- **Name**: `carousel1`
- **Size**: `600, 400` (or desired size)
- **Location**: `50, 50` (or desired position)
- **CarouselPath**: `Default` (or other path option)

## Adding Carousel Control via Code

### Programmatic Setup

**Step 1: Add Namespace**

```csharp
using Syncfusion.Windows.Forms.Tools;
```

```vb
Imports Syncfusion.Windows.Forms.Tools
```

**Step 2: Create and Configure Carousel**

```csharp
public partial class Form1 : Form
{
    private Carousel carousel1;
    
    public Form1()
    {
        InitializeComponent();
        
        // Create Carousel instance
        carousel1 = new Carousel();
        
        // Set basic properties
        carousel1.Size = new System.Drawing.Size(600, 400);
        carousel1.Location = new System.Drawing.Point(50, 50);
        carousel1.BackColor = System.Drawing.Color.White;
        
        // Add to form
        this.Controls.Add(carousel1);
    }
}
```

```vb
Public Partial Class Form1
    Inherits Form
    
    Private carousel1 As Carousel
    
    Public Sub New()
        InitializeComponent()
        
        ' Create Carousel instance
        carousel1 = New Carousel()
        
        ' Set basic properties
        carousel1.Size = New System.Drawing.Size(600, 400)
        carousel1.Location = New System.Drawing.Point(50, 50)
        carousel1.BackColor = System.Drawing.Color.White
        
        ' Add to form
        Me.Controls.Add(carousel1)
    End Sub
End Class
```

## Adding Custom Controls to Carousel

Custom controls (buttons, panels, user controls, etc.) can be added to the carousel for display and rotation.

### Basic Control Addition

**C# Example:**

```csharp
// Create Carousel
Carousel carousel1 = new Carousel();
carousel1.Size = new Size(600, 400);
carousel1.Location = new Point(50, 50);

// Create ButtonAdv controls
ButtonAdv button1 = new ButtonAdv();
button1.Text = "Button 1";
button1.Size = new Size(100, 80);
button1.BackColor = Color.FromArgb(22, 165, 220);
button1.ForeColor = Color.White;

ButtonAdv button2 = new ButtonAdv();
button2.Text = "Button 2";
button2.Size = new Size(100, 80);
button2.BackColor = Color.FromArgb(22, 165, 220);
button2.ForeColor = Color.White;

ButtonAdv button3 = new ButtonAdv();
button3.Text = "Button 3";
button3.Size = new Size(100, 80);
button3.BackColor = Color.FromArgb(22, 165, 220);
button3.ForeColor = Color.White;

// Add controls to Carousel (BOTH collections required)
carousel1.Controls.Add(button1);
carousel1.Items.Add(button1);

carousel1.Controls.Add(button2);
carousel1.Items.Add(button2);

carousel1.Controls.Add(button3);
carousel1.Items.Add(button3);

// Add Carousel to form
this.Controls.Add(carousel1);
```

**VB.NET Example:**

```vb
' Create Carousel
Dim carousel1 As New Carousel()
carousel1.Size = New Size(600, 400)
carousel1.Location = New Point(50, 50)

' Create ButtonAdv controls
Dim button1 As New ButtonAdv()
button1.Text = "Button 1"
button1.Size = New Size(100, 80)
button1.BackColor = Color.FromArgb(22, 165, 220)
button1.ForeColor = Color.White

Dim button2 As New ButtonAdv()
button2.Text = "Button 2"
button2.Size = New Size(100, 80)
button2.BackColor = Color.FromArgb(22, 165, 220)
button2.ForeColor = Color.White

Dim button3 As New ButtonAdv()
button3.Text = "Button 3"
button3.Size = New Size(100, 80)
button3.BackColor = Color.FromArgb(22, 165, 220)
button3.ForeColor = Color.White

' Add controls to Carousel (BOTH collections required)
carousel1.Controls.Add(button1)
carousel1.Items.Add(button1)

carousel1.Controls.Add(button2)
carousel1.Items.Add(button2)

carousel1.Controls.Add(button3)
carousel1.Items.Add(button3)

' Add Carousel to form
Me.Controls.Add(carousel1)
```

### Multiple Control Types

```csharp
Carousel carousel1 = new Carousel();

// Add buttons
ButtonAdv btn = new ButtonAdv { Text = "Action", Size = new Size(120, 60) };
carousel1.Controls.Add(btn);
carousel1.Items.Add(btn);

// Add panels
Panel panel = new Panel { Size = new Size(150, 100), BackColor = Color.LightBlue };
panel.Controls.Add(new Label { Text = "Panel Content", AutoSize = true });
carousel1.Controls.Add(panel);
carousel1.Items.Add(panel);

// Add user controls
MyCustomControl customCtrl = new MyCustomControl();
carousel1.Controls.Add(customCtrl);
carousel1.Items.Add(customCtrl);

this.Controls.Add(carousel1);
```

### Loop for Multiple Items

```csharp
Carousel carousel1 = new Carousel();
carousel1.Size = new Size(600, 400);

for (int i = 1; i <= 8; i++)
{
    ButtonAdv button = new ButtonAdv();
    button.Text = $"Item {i}";
    button.Size = new Size(100, 80);
    button.BackColor = Color.FromArgb(22, 165, 220);
    button.ForeColor = Color.White;
    button.Tag = i; // Store index for event handling
    
    // Add to both collections
    carousel1.Controls.Add(button);
    carousel1.Items.Add(button);
}

this.Controls.Add(carousel1);
```

## Adding Images to Carousel

Images are managed differently than custom controls. Enable `ImageSlides` mode and use `ImageListCollection`.

### Basic Image Addition

**C# Example:**

```csharp
// Create and configure Carousel for images
Carousel carousel1 = new Carousel();
carousel1.Size = new Size(600, 400);
carousel1.Location = new Point(50, 50);

// IMPORTANT: Enable ImageSlides mode
carousel1.ImageSlides = true;

// Create CarouselImage objects
CarouselImage image1 = new CarouselImage();
image1.ItemImage = Image.FromFile("Images/photo1.jpg");

CarouselImage image2 = new CarouselImage();
image2.ItemImage = Image.FromFile("Images/photo2.jpg");

CarouselImage image3 = new CarouselImage();
image3.ItemImage = Image.FromFile("Images/photo3.jpg");

// Add to ImageListCollection
carousel1.ImageListCollection.Add(image1);
carousel1.ImageListCollection.Add(image2);
carousel1.ImageListCollection.Add(image3);

// Add Carousel to form
this.Controls.Add(carousel1);
```

**VB.NET Example:**

```vb
' Create and configure Carousel for images
Dim carousel1 As New Carousel()
carousel1.Size = New Size(600, 400)
carousel1.Location = New Point(50, 50)

' IMPORTANT: Enable ImageSlides mode
carousel1.ImageSlides = True

' Create CarouselImage objects
Dim image1 As New CarouselImage()
image1.ItemImage = Image.FromFile("Images/photo1.jpg")

Dim image2 As New CarouselImage()
image2.ItemImage = Image.FromFile("Images/photo2.jpg")

Dim image3 As New CarouselImage()
image3.ItemImage = Image.FromFile("Images/photo3.jpg")

' Add to ImageListCollection
carousel1.ImageListCollection.Add(image1)
carousel1.ImageListCollection.Add(image2)
carousel1.ImageListCollection.Add(image3)

' Add Carousel to form
Me.Controls.Add(carousel1)
```

### Loading Images from Folder

```csharp
Carousel carousel1 = new Carousel();
carousel1.ImageSlides = true;
carousel1.Size = new Size(700, 500);

// Load all images from a folder
string imageFolderPath = Path.Combine(Application.StartupPath, "Images");

if (Directory.Exists(imageFolderPath))
{
    string[] imageFiles = Directory.GetFiles(imageFolderPath, "*.jpg");
    
    foreach (string imageFile in imageFiles)
    {
        CarouselImage carouselImg = new CarouselImage();
        carouselImg.ItemImage = Image.FromFile(imageFile);
        carousel1.ImageListCollection.Add(carouselImg);
    }
}

this.Controls.Add(carousel1);
```

### Using Embedded Resources

```csharp
Carousel carousel1 = new Carousel();
carousel1.ImageSlides = true;

// Load from embedded resources
CarouselImage img1 = new CarouselImage();
img1.ItemImage = Properties.Resources.Image1; // Embedded resource

CarouselImage img2 = new CarouselImage();
img2.ItemImage = Properties.Resources.Image2;

carousel1.ImageListCollection.Add(img1);
carousel1.ImageListCollection.Add(img2);

this.Controls.Add(carousel1);
```

## Initial Configuration

### Essential Properties to Set

```csharp
Carousel carousel1 = new Carousel();

// Size and position
carousel1.Size = new Size(600, 400);
carousel1.Location = new Point(50, 50);
carousel1.Dock = DockStyle.Fill; // Or specific docking

// Path configuration
carousel1.CarouselPath = CarouselPath.Default; // Default, Orbital, Oval, Linear

// Visual settings
carousel1.Perspective = 4.0f; // Zoom level
carousel1.TransitionSpeed = 2.0f; // Rotation speed

// For image mode
carousel1.ImageSlides = true; // Only if using images
carousel1.ShowImagePreview = true;
carousel1.ShowImageShadow = true;

this.Controls.Add(carousel1);
```

## Complete Example: Getting Started

### Example 1: Simple Button Carousel

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace CarouselGettingStarted
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
            CreateCarousel();
        }
        
        private void CreateCarousel()
        {
            // Create Carousel
            Carousel carousel1 = new Carousel();
            carousel1.Size = new Size(600, 400);
            carousel1.Location = new Point(50, 50);
            carousel1.CarouselPath = CarouselPath.Default;
            carousel1.Perspective = 4.0f;
            carousel1.TransitionSpeed = 2.0f;
            
            // Add buttons
            for (int i = 1; i <= 6; i++)
            {
                ButtonAdv btn = new ButtonAdv();
                btn.Text = $"Button {i}";
                btn.Size = new Size(100, 80);
                btn.BackColor = Color.FromArgb(22, 165, 220);
                btn.ForeColor = Color.White;
                
                carousel1.Controls.Add(btn);
                carousel1.Items.Add(btn);
            }
            
            this.Controls.Add(carousel1);
        }
    }
}
```

### Example 2: Image Gallery Carousel

```csharp
using System;
using System.Drawing;
using System.IO;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace CarouselImageGallery
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
            CreateImageCarousel();
        }
        
        private void CreateImageCarousel()
        {
            // Create Carousel
            Carousel carousel1 = new Carousel();
            carousel1.Dock = DockStyle.Fill;
            carousel1.ImageSlides = true;
            carousel1.CarouselPath = CarouselPath.Default;
            carousel1.Perspective = 5.0f;
            carousel1.TransitionSpeed = 2.5f;
            carousel1.ShowImagePreview = true;
            carousel1.ShowImageShadow = true;
            carousel1.ImageHighlightColor = Color.Gold;
            
            // Load images
            string[] imagePaths = {
                "Images/img1.jpg",
                "Images/img2.jpg",
                "Images/img3.jpg",
                "Images/img4.jpg",
                "Images/img5.jpg",
                "Images/img6.jpg"
            };
            
            foreach (string path in imagePaths)
            {
                if (File.Exists(path))
                {
                    CarouselImage carouselImage = new CarouselImage();
                    carouselImage.ItemImage = Image.FromFile(path);
                    carousel1.ImageListCollection.Add(carouselImage);
                }
            }
            
            this.Controls.Add(carousel1);
        }
    }
}
```

## Troubleshooting

### Items Not Appearing

**Problem:** Controls added but not visible in carousel

**Solutions:**
- Ensure controls are added to BOTH `Controls` and `Items` collections
- Verify carousel size is sufficient (at least 400x300)
- Check that control sizes are reasonable (not too large or small)
- Ensure controls have visible BackColor or content

### Images Not Loading

**Problem:** Images don't appear when using ImageSlides

**Solutions:**
- Confirm `ImageSlides = true` is set BEFORE adding images
- Verify image file paths are correct
- Check that image files exist and are accessible
- Ensure images are added to `ImageListCollection`, not `Items`

### Assembly Reference Errors

**Problem:** "Carousel does not exist in the namespace" error

**Solutions:**
- Add all six required assembly references
- Use NuGet package installation (recommended)
- Check that assemblies match your framework version
- Add `using Syncfusion.Windows.Forms.Tools;` namespace

### Designer Toolbox Issues

**Problem:** Carousel not appearing in Toolbox

**Solutions:**
- Install Syncfusion controls properly
- Right-click Toolbox → Reset Toolbox
- Manually add via Choose Items
- Restart Visual Studio after installation

## Best Practices

1. **Use NuGet**: Install via NuGet for automatic dependency management
2. **Set ImageSlides Early**: Enable ImageSlides BEFORE adding images
3. **Add to Both Collections**: Always add custom controls to both Controls and Items
4. **Reasonable Sizing**: Keep carousel size at least 400x300 for visibility
5. **Item Count**: Limit to 6-12 items for optimal performance
6. **Image Optimization**: Compress images before loading for better performance

## Next Steps

- **Explore Paths:** See [carousel-paths.md](carousel-paths.md) for different layout options
- **Image Features:** See [image-slides.md](image-slides.md) for advanced image handling
- **Visual Effects:** See [visual-configuration.md](visual-configuration.md) for customization options
