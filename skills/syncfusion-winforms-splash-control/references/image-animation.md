# Image and Animation Settings

## Table of Contents
- [Overview](#overview)
- [Splash Image Configuration](#splash-image-configuration)
- [Animation Settings](#animation-settings)
- [Transparency Configuration](#transparency-configuration)
- [Window Layering](#window-layering)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)

## Overview

The SplashControl provides rich visual customization options including image display, animation effects, transparency, and window positioning. These features enable you to create professional, visually appealing splash screens that match your application's branding.

## Splash Image Configuration

### SplashImage Property

The **SplashImage** property sets the image displayed as the background of the splash screen.

**Property Details:**
- **Type:** `System.Drawing.Image`
- **Default:** `null`
- **Supported formats:** PNG, JPG, BMP, GIF

### Setting Image from File

**C# Example:**

```csharp
// Load from file path
splashControl1.SplashImage = Image.FromFile("splash.png");

// With full path
string imagePath = Path.Combine(Application.StartupPath, "Images", "splash.png");
splashControl1.SplashImage = Image.FromFile(imagePath);

// With error handling
try
{
    splashControl1.SplashImage = Image.FromFile("splash.png");
}
catch (FileNotFoundException)
{
    MessageBox.Show("Splash image not found!");
}
```

**VB.NET Example:**

```vb
' Load from file path
splashControl1.SplashImage = Image.FromFile("splash.png")

' With full path
Dim imagePath As String = Path.Combine(Application.StartupPath, "Images", "splash.png")
splashControl1.SplashImage = Image.FromFile(imagePath)
```

### Setting Image from Resources

Using embedded resources is recommended for deployment:

**C# Example:**

```csharp
// From project resources (recommended)
splashControl1.SplashImage = Properties.Resources.SplashImage;

// From resource manager
ResourceManager rm = new ResourceManager("MyApp.Properties.Resources", 
                                        Assembly.GetExecutingAssembly());
splashControl1.SplashImage = (Image)rm.GetObject("SplashImage");
```

**VB.NET Example:**

```vb
' From project resources (recommended)
splashControl1.SplashImage = My.Resources.SplashImage

' From resource manager
Dim rm As New ResourceManager("MyApp.Resources", 
                             Assembly.GetExecutingAssembly())
splashControl1.SplashImage = CType(rm.GetObject("SplashImage"), Image)
```

### Designer Configuration

To set the image through the designer:

1. Select the SplashControl in the component tray
2. Find **SplashImage** property in the Properties window
3. Click the ellipsis button (...)
4. Choose **Local Resource** or **Project Resource File**
5. Select or import your image

**Designer code generated:**

```csharp
this.splashControl1.SplashImage = 
    ((System.Drawing.Image)(resources.GetObject("splashControl1.SplashImage")));
```

## Animation Settings

### ShowAnimation Property

The **ShowAnimation** property enables a left-to-right reveal animation when the splash screen displays.

**Property Details:**
- **Type:** `bool`
- **Default:** `false`
- **Effect:** Image draws progressively from left to right

**C# Example:**

```csharp
// Enable animation
splashControl1.ShowAnimation = true;
splashControl1.SplashImage = Properties.Resources.Logo;
splashControl1.TimerInterval = 3000;

// Disable animation (instant display)
splashControl1.ShowAnimation = false;
```

**VB.NET Example:**

```vb
' Enable animation
splashControl1.ShowAnimation = True
splashControl1.SplashImage = My.Resources.Logo
splashControl1.TimerInterval = 3000

' Disable animation (instant display)
splashControl1.ShowAnimation = False
```

### Animation Behavior

**With ShowAnimation = true:**
- Image reveals from left edge to right edge
- Animation duration is proportional to image width
- Creates a professional entrance effect
- Recommended for branding and logo displays

**With ShowAnimation = false:**
- Image appears instantly
- Faster perceived load time
- Better for informational splash screens

### Animation Configuration Example

```csharp
private void ConfigureAnimatedSplash()
{
    splashControl1.HostForm = this;
    splashControl1.SplashImage = Properties.Resources.CompanyLogo;
    
    // Enable smooth animation
    splashControl1.ShowAnimation = true;
    
    // Give enough time to see the animation
    splashControl1.TimerInterval = 4000;
    
    // Center on screen for best effect
    splashControl1.DesktopAlignment = SplashAlignment.Center;
    
    // Show on top
    splashControl1.ShowAsTopMost = true;
}
```

## Transparency Configuration

### TransparentColor Property

The **TransparentColor** property makes specific colors in the splash image transparent, creating non-rectangular splash screens.

**Property Details:**
- **Type:** `System.Drawing.Color`
- **Default:** `Color.Empty`
- **Usage:** All pixels matching this color become transparent

**C# Example:**

```csharp
// Make white pixels transparent
splashControl1.TransparentColor = Color.White;

// Make specific RGB color transparent
splashControl1.TransparentColor = Color.FromArgb(255, 0, 255); // Magenta

// Make background color transparent
splashControl1.TransparentColor = Color.FromArgb(240, 240, 240);

// Disable transparency
splashControl1.TransparentColor = Color.Empty;
```

**VB.NET Example:**

```vb
' Make white pixels transparent
splashControl1.TransparentColor = Color.White

' Make specific RGB color transparent
splashControl1.TransparentColor = Color.FromArgb(255, 0, 255) ' Magenta

' Disable transparency
splashControl1.TransparentColor = Color.Empty
```

### Creating Transparent Splash Screens

**Step 1: Prepare Image**
- Create image with solid background color (e.g., bright magenta #FF00FF)
- Design your splash content over this background
- Save as PNG or BMP

**Step 2: Configure SplashControl**

```csharp
private void ConfigureTransparentSplash()
{
    splashControl1.SplashImage = Properties.Resources.TransparentSplash;
    
    // Make magenta transparent
    splashControl1.TransparentColor = Color.FromArgb(255, 0, 255);
    
    // Enable animation for effect
    splashControl1.ShowAnimation = true;
    
    // Show on top of everything
    splashControl1.ShowAsTopMost = true;
}
```

### Complete Transparency Example

```csharp
public class TransparentSplashExample : Form
{
    private SplashControl splashControl1;
    
    public TransparentSplashExample()
    {
        InitializeComponent();
        ConfigureCircularSplash();
    }
    
    private void ConfigureCircularSplash()
    {
        splashControl1 = new SplashControl();
        
        // Load image with white background around circular logo
        splashControl1.SplashImage = Properties.Resources.CircularLogo;
        
        // Make white background transparent
        splashControl1.TransparentColor = Color.White;
        
        // Configure display
        splashControl1.HostForm = this;
        splashControl1.ShowAnimation = true;
        splashControl1.ShowAsTopMost = true;
        splashControl1.DesktopAlignment = SplashAlignment.Center;
        splashControl1.TimerInterval = 3000;
        splashControl1.AutoMode = true;
    }
}
```

## Window Layering

### ShowAsTopMost Property

The **ShowAsTopMost** property controls whether the splash screen displays as the topmost window.

**Property Details:**
- **Type:** `bool`
- **Default:** `true`
- **Effect:** Splash stays on top of all other windows

**C# Example:**

```csharp
// Always on top (default, recommended)
splashControl1.ShowAsTopMost = true;

// Allow other windows to cover splash
splashControl1.ShowAsTopMost = false;
```

**VB.NET Example:**

```vb
' Always on top (default, recommended)
splashControl1.ShowAsTopMost = True

' Allow other windows to cover splash
splashControl1.ShowAsTopMost = False
```

### When to Use ShowAsTopMost

**ShowAsTopMost = true (Recommended):**
- Ensures splash is always visible
- Prevents other applications from obscuring it
- Standard behavior for splash screens
- Better user experience during startup

**ShowAsTopMost = false:**
- Allows user to interact with other applications
- Less intrusive for non-critical splash screens
- Can be hidden by other windows

## Complete Examples

### Example 1: Professional Branded Splash

```csharp
private void ConfigureProfessionalSplash()
{
    splashControl1.HostForm = this;
    
    // High-quality logo image
    splashControl1.SplashImage = Properties.Resources.CompanyBrandLogo;
    
    // Smooth left-to-right animation
    splashControl1.ShowAnimation = true;
    
    // Display prominently
    splashControl1.ShowAsTopMost = true;
    
    // Center on screen
    splashControl1.DesktopAlignment = SplashAlignment.Center;
    
    // 4-second display
    splashControl1.TimerInterval = 4000;
    
    // Automatic display on startup
    splashControl1.AutoMode = true;
}
```

### Example 2: Transparent Animated Splash

```csharp
private void ConfigureTransparentAnimatedSplash()
{
    splashControl1.HostForm = this;
    
    // Image with magenta background
    splashControl1.SplashImage = Properties.Resources.LogoWithTransparency;
    
    // Make magenta transparent
    splashControl1.TransparentColor = Color.FromArgb(255, 0, 255);
    
    // Enable animation
    splashControl1.ShowAnimation = true;
    
    // Always on top
    splashControl1.ShowAsTopMost = true;
    
    // Center position
    splashControl1.DesktopAlignment = SplashAlignment.Center;
    
    // 3-second display
    splashControl1.TimerInterval = 3000;
}
```

### Example 3: Simple Non-Animated Splash

```csharp
private void ConfigureSimpleSplash()
{
    splashControl1.HostForm = this;
    splashControl1.SplashImage = Properties.Resources.QuickLogo;
    
    // No animation - instant display
    splashControl1.ShowAnimation = false;
    
    // Standard topmost
    splashControl1.ShowAsTopMost = true;
    
    // Quick 2-second display
    splashControl1.TimerInterval = 2000;
    
    // Auto-display
    splashControl1.AutoMode = true;
}
```

### Example 4: Complex Visual Configuration

```csharp
public class ComplexVisualSplash : Form
{
    private SplashControl splashControl1;
    
    public ComplexVisualSplash()
    {
        InitializeComponent();
        SetupAdvancedSplash();
    }
    
    private void SetupAdvancedSplash()
    {
        splashControl1 = new SplashControl();
        
        // Load high-resolution splash image
        splashControl1.SplashImage = LoadOptimizedImage();
        
        // Configure transparency
        splashControl1.TransparentColor = Color.White;
        
        // Enable all visual effects
        splashControl1.ShowAnimation = true;
        splashControl1.ShowAsTopMost = true;
        
        // Position and timing
        splashControl1.HostForm = this;
        splashControl1.DesktopAlignment = SplashAlignment.Center;
        splashControl1.TimerInterval = 5000;
        
        // Hide host form during splash
        splashControl1.HideHostForm = true;
        
        // Automatic modal display
        splashControl1.AutoMode = true;
        splashControl1.AutoModeDisableOwner = true;
    }
    
    private Image LoadOptimizedImage()
    {
        // Load and optimize image for display
        Image original = Properties.Resources.HighResSplash;
        
        // Optionally resize for performance
        if (original.Width > 800)
        {
            return ResizeImage(original, 800, 600);
        }
        
        return original;
    }
    
    private Image ResizeImage(Image img, int width, int height)
    {
        Bitmap result = new Bitmap(width, height);
        using (Graphics g = Graphics.FromImage(result))
        {
            g.InterpolationMode = System.Drawing.Drawing2D.InterpolationMode.HighQualityBicubic;
            g.DrawImage(img, 0, 0, width, height);
        }
        return result;
    }
}
```

## Best Practices

### Image Best Practices

1. **Resolution:** Use images sized appropriately for target displays (800x600 or 1024x768 typical)
2. **Format:** PNG for transparency support, JPG for photographs
3. **File size:** Keep under 1MB for fast loading
4. **Resources:** Embed images as resources, not external files
5. **Quality:** Use high-quality images that represent your brand well

### Animation Best Practices

1. **Enable for branding:** Use animation for logo/brand splash screens
2. **Disable for speed:** Skip animation when showing data loading messages
3. **Match duration:** Set TimerInterval long enough to see full animation (3-4 seconds minimum)
4. **Test on slow hardware:** Ensure animation performs well on various systems

### Transparency Best Practices

1. **Use distinct colors:** Choose transparency color that doesn't appear in your content (bright magenta works well)
2. **Test thoroughly:** Verify transparency works on different backgrounds
3. **Combine with topmost:** Use ShowAsTopMost = true with transparent splashes
4. **Consider performance:** Transparency may have slight performance impact

### Visual Configuration Checklist

- [ ] Image embedded as resource
- [ ] Image resolution appropriate for target displays
- [ ] ShowAnimation set based on desired effect
- [ ] TransparentColor configured if using transparency
- [ ] ShowAsTopMost set to true (recommended)
- [ ] TimerInterval allows enough time to view splash
- [ ] DesktopAlignment set appropriately
- [ ] Tested on multiple displays and resolutions

### Common Pitfalls to Avoid

1. **Missing images:** Always check image exists before setting
2. **Wrong transparency color:** Ensure TransparentColor matches image background exactly
3. **Animation too short:** Give at least 3 seconds to see full animation
4. **Oversized images:** Large images slow down display
5. **External file dependencies:** Embed images as resources for reliable deployment
