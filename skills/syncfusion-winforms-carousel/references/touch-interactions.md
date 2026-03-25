# Touch Interactions

This guide covers touch support in the Windows Forms Carousel control, including pan, flick, pinch, and stretch gestures.

## Overview

The Carousel control responds to default touch interactions that substitute standard mouse operations. Touch gestures provide intuitive navigation and control on touch-enabled devices.

## Supported Touch Gestures

The carousel supports four main touch gestures:

1. **Pan**: Drag to move items
2. **Flick**: Quick swipe to initiate fast movement
3. **Pinch**: Two-finger pinch to decrease perspective
4. **Stretch**: Two-finger spread to increase perspective

## Pan Gesture

The pan gesture initiates moving the carousel items by dragging.

### How It Works

- Place finger on carousel
- Drag left or right
- Items move in the direction of the drag
- Release to stop at current position

### Pan Behavior

```csharp
// Pan gesture works automatically with Carousel
Carousel carousel1 = new Carousel();
carousel1.ImageSlides = true;
carousel1.Dock = DockStyle.Fill;
carousel1.FilePath = "Images";

// No additional configuration needed for pan
// Users can drag to rotate the carousel
this.Controls.Add(carousel1);
```

### Pan vs Mouse

Pan gesture is the touch equivalent of:
- Mouse click and drag to rotate carousel
- Mouse wheel to scroll through items

## Flick Gesture

The flick gesture performs a quick swipe to initiate fast movement of carousel items.

### How It Works

- Quick swipe gesture across carousel
- Carousel continues moving after finger lifts
- Movement gradually slows down (momentum)
- Swipe speed determines initial rotation speed

### Flick with TransitionSpeed

```csharp
Carousel carousel1 = new Carousel();
carousel1.ImageSlides = true;
carousel1.TransitionSpeed = 3.0f; // Higher speed for responsive flicks

// Flick gesture will respect TransitionSpeed
// Faster TransitionSpeed = more responsive flicks
carousel1.FilePath = "Gallery";
this.Controls.Add(carousel1);
```

### Flick Behavior

The flick gesture speed is influenced by:
- `TransitionSpeed` property
- Gesture velocity (how fast user swipes)
- Number of items in carousel

## Pinch Gesture

The pinch gesture decreases the perspective view using two fingers moving together.

### How It Works

- Place two fingers on carousel
- Move fingers closer together (pinching motion)
- Perspective value decreases
- Items appear smaller/farther away

### Pinch Effect

```csharp
Carousel carousel1 = new Carousel();
carousel1.ImageSlides = true;
carousel1.Perspective = 5.0f; // Start with high perspective

// Users can pinch to decrease perspective down to minimum
// Visual effect: items appear more distant
carousel1.ShowImagePreview = true;
carousel1.FilePath = "Photos";

this.Controls.Add(carousel1);
```

### Pinch Range

- **Starting Perspective**: Set initial value (e.g., 5.0f)
- **Minimum**: Pinch can decrease to approximately 0
- **Effect**: Items become smaller and more distant

## Stretch Gesture

The stretch gesture increases the perspective view using two fingers moving apart.

### How It Works

- Place two fingers on carousel
- Spread fingers apart (stretching motion)
- Perspective value increases
- Items appear larger/closer

### Stretch Effect

```csharp
Carousel carousel1 = new Carousel();
carousel1.ImageSlides = true;
carousel1.Perspective = 3.0f; // Start with moderate perspective

// Users can stretch to increase perspective
// Visual effect: items appear closer and larger
carousel1.ShowImagePreview = true;
carousel1.ShowImageShadow = true;
carousel1.FilePath = "Images";

this.Controls.Add(carousel1);
```

### Stretch Range

- **Starting Perspective**: Set initial value (e.g., 3.0f)
- **Maximum**: Stretch can increase to approximately 10.0 or higher
- **Effect**: Items become larger and closer

## Touch vs Mouse Interactions

### Comparison Table

| Action | Touch Gesture | Mouse Equivalent |
|--------|---------------|------------------|
| Rotate carousel | Pan (drag) | Click and drag |
| Fast rotation | Flick (swipe) | Click and quick drag |
| Zoom in | Stretch (spread fingers) | Mouse wheel up / Ctrl+Scroll |
| Zoom out | Pinch (move fingers together) | Mouse wheel down / Ctrl+Scroll |
| Select item | Tap | Click |

### Touch-First Design

```csharp
public partial class TouchEnabledForm : Form
{
    private Carousel carousel1;
    
    private void InitializeTouchCarousel()
    {
        carousel1 = new Carousel();
        carousel1.Dock = DockStyle.Fill;
        carousel1.ImageSlides = true;
        
        // Optimize for touch
        carousel1.Perspective = 4.0f; // Moderate starting perspective
        carousel1.TransitionSpeed = 2.5f; // Responsive to flicks
        carousel1.ShowImagePreview = true;
        carousel1.ShowImageShadow = true;
        
        // Touch-friendly visual feedback
        carousel1.ImageHighlightColor = Color.FromArgb(0, 173, 181);
        
        carousel1.FilePath = "Gallery";
        this.Controls.Add(carousel1);
    }
}
```

## Complete Touch Examples

### Example 1: Touch-Enabled Photo Gallery

```csharp
using System;
using System.Drawing;
using System.IO;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace TouchPhotoGallery
{
    public partial class Form1 : Form
    {
        private Carousel photoCarousel;
        
        public Form1()
        {
            InitializeComponent();
            CreateTouchGallery();
        }
        
        private void CreateTouchGallery()
        {
            photoCarousel = new Carousel();
            photoCarousel.Dock = DockStyle.Fill;
            photoCarousel.ImageSlides = true;
            
            // Touch-optimized settings
            photoCarousel.CarouselPath = CarouselPath.Oval;
            photoCarousel.Perspective = 4.5f;
            photoCarousel.TransitionSpeed = 2.5f;
            
            // Visual enhancements
            photoCarousel.ShowImagePreview = true;
            photoCarousel.ShowImageShadow = true;
            photoCarousel.BackColor = Color.FromArgb(30, 30, 30);
            photoCarousel.ImageHighlightColor = Color.Gold;
            
            // Load photos
            photoCarousel.FilePath = Path.Combine(Application.StartupPath, "Photos");
            
            this.Controls.Add(photoCarousel);
            
            // Add instructions
            Label lblInstructions = new Label();
            lblInstructions.Text = "Pan: Drag to rotate | Flick: Quick swipe | Pinch/Stretch: Zoom";
            lblInstructions.Font = new Font("Segoe UI", 10F);
            lblInstructions.ForeColor = Color.White;
            lblInstructions.BackColor = Color.FromArgb(100, 0, 0, 0);
            lblInstructions.AutoSize = true;
            lblInstructions.Padding = new Padding(10);
            lblInstructions.Location = new Point(20, 20);
            
            this.Controls.Add(lblInstructions);
            lblInstructions.BringToFront();
        }
    }
}
```

### Example 2: Product Viewer with Touch Controls

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace TouchProductViewer
{
    public partial class Form1 : Form
    {
        private Carousel productCarousel;
        private Label lblGesture;
        
        public Form1()
        {
            InitializeComponent();
            SetupProductViewer();
        }
        
        private void SetupProductViewer()
        {
            // Create carousel
            productCarousel = new Carousel();
            productCarousel.Size = new Size(900, 650);
            productCarousel.Location = new Point(50, 50);
            productCarousel.ImageSlides = true;
            
            // Touch-friendly configuration
            productCarousel.CarouselPath = CarouselPath.Default;
            productCarousel.Perspective = 5.0f;
            productCarousel.TransitionSpeed = 2.0f;
            productCarousel.ShowImagePreview = true;
            productCarousel.ShowImageShadow = true;
            productCarousel.ImageHighlightColor = Color.FromArgb(41, 128, 185);
            
            // Load product images
            productCarousel.FilePath = "Products";
            
            this.Controls.Add(productCarousel);
            
            // Gesture indicator
            lblGesture = new Label();
            lblGesture.Size = new Size(300, 40);
            lblGesture.Location = new Point(350, 720);
            lblGesture.Font = new Font("Segoe UI", 12F, FontStyle.Bold);
            lblGesture.ForeColor = Color.FromArgb(41, 128, 185);
            lblGesture.TextAlign = ContentAlignment.MiddleCenter;
            lblGesture.Text = "Touch to navigate";
            
            this.Controls.Add(lblGesture);
            
            // Event handlers for visual feedback
            productCarousel.MouseDown += (s, e) => {
                lblGesture.Text = "Panning...";
                lblGesture.ForeColor = Color.FromArgb(46, 204, 113);
            };
            
            productCarousel.MouseUp += (s, e) => {
                lblGesture.Text = "Touch to navigate";
                lblGesture.ForeColor = Color.FromArgb(41, 128, 185);
            };
        }
    }
}
```

### Example 3: Tablet-Optimized Carousel

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace TabletCarousel
{
    public partial class Form1 : Form
    {
        private Carousel tabletCarousel;
        private Panel instructionsPanel;
        
        public Form1()
        {
            InitializeComponent();
            
            // Maximize for tablet fullscreen
            this.WindowState = FormWindowState.Maximized;
            this.FormBorderStyle = FormBorderStyle.None;
            
            CreateTabletCarousel();
            CreateInstructions();
        }
        
        private void CreateTabletCarousel()
        {
            tabletCarousel = new Carousel();
            tabletCarousel.Dock = DockStyle.Fill;
            tabletCarousel.ImageSlides = true;
            
            // Tablet-optimized settings
            tabletCarousel.CarouselPath = CarouselPath.Default;
            tabletCarousel.Perspective = 4.0f;
            tabletCarousel.TransitionSpeed = 2.5f;
            tabletCarousel.ShowImagePreview = true;
            tabletCarousel.ShowImageShadow = true;
            tabletCarousel.BackColor = Color.White;
            tabletCarousel.ImageHighlightColor = Color.FromArgb(0, 122, 204);
            
            // Load images
            tabletCarousel.FilePath = "Gallery";
            
            this.Controls.Add(tabletCarousel);
        }
        
        private void CreateInstructions()
        {
            instructionsPanel = new Panel();
            instructionsPanel.Size = new Size(400, 150);
            instructionsPanel.Location = new Point(50, 50);
            instructionsPanel.BackColor = Color.FromArgb(200, 50, 50, 50);
            
            Label lblTitle = new Label();
            lblTitle.Text = "Touch Gestures";
            lblTitle.Font = new Font("Segoe UI", 14F, FontStyle.Bold);
            lblTitle.ForeColor = Color.White;
            lblTitle.Location = new Point(15, 15);
            lblTitle.AutoSize = true;
            
            Label lblPan = new Label();
            lblPan.Text = "• Drag: Pan/Rotate carousel";
            lblPan.Font = new Font("Segoe UI", 10F);
            lblPan.ForeColor = Color.White;
            lblPan.Location = new Point(15, 50);
            lblPan.AutoSize = true;
            
            Label lblFlick = new Label();
            lblFlick.Text = "• Swipe: Quick rotation";
            lblFlick.Font = new Font("Segoe UI", 10F);
            lblFlick.ForeColor = Color.White;
            lblFlick.Location = new Point(15, 75);
            lblFlick.AutoSize = true;
            
            Label lblPinch = new Label();
            lblPinch.Text = "• Pinch: Zoom out";
            lblPinch.Font = new Font("Segoe UI", 10F);
            lblPinch.ForeColor = Color.White;
            lblPinch.Location = new Point(15, 100);
            lblPinch.AutoSize = true;
            
            Label lblStretch = new Label();
            lblStretch.Text = "• Stretch: Zoom in";
            lblStretch.Font = new Font("Segoe UI", 10F);
            lblStretch.ForeColor = Color.White;
            lblStretch.Location = new Point(15, 125);
            lblStretch.AutoSize = true;
            
            instructionsPanel.Controls.Add(lblTitle);
            instructionsPanel.Controls.Add(lblPan);
            instructionsPanel.Controls.Add(lblFlick);
            instructionsPanel.Controls.Add(lblPinch);
            instructionsPanel.Controls.Add(lblStretch);
            
            this.Controls.Add(instructionsPanel);
            instructionsPanel.BringToFront();
            
            // Auto-hide instructions after 5 seconds
            Timer hideTimer = new Timer();
            hideTimer.Interval = 5000;
            hideTimer.Tick += (s, e) => {
                instructionsPanel.Visible = false;
                hideTimer.Stop();
            };
            hideTimer.Start();
        }
    }
}
```

## Touch Optimization Tips

### Responsive Touch Settings

```csharp
private void ConfigureForTouch(Carousel carousel)
{
    // Moderate perspective for pinch/stretch control
    carousel.Perspective = 4.0f;
    
    // Responsive transition speed for flicks
    carousel.TransitionSpeed = 2.5f;
    
    // Visual feedback
    carousel.ShowImagePreview = true;
    carousel.ShowImageShadow = true;
    
    // Touch-friendly size (larger targets)
    carousel.Dock = DockStyle.Fill; // Use full screen
}
```

### Performance for Touch Devices

```csharp
private void OptimizeForTouchPerformance(Carousel carousel)
{
    // Limit image count for smooth gestures
    // Recommended: 6-12 images maximum
    
    // Optimize images before loading
    // Keep file sizes reasonable (< 500KB per image)
    
    // Balance visual quality with performance
    carousel.ShowImageShadow = true; // Moderate impact
    carousel.Perspective = 4.0f; // Not too high
    carousel.TransitionSpeed = 2.5f; // Responsive but not too fast
}
```

## Testing Touch Interactions

### Testing on Actual Devices

To properly test touch gestures:

1. **Deploy to touch device** (tablet, touch-screen laptop)
2. **Test all four gestures**: Pan, Flick, Pinch, Stretch
3. **Check responsiveness**: Gestures should feel natural and immediate
4. **Test multi-touch**: Pinch and stretch require two-finger support
5. **Verify performance**: Smooth animations without lag

### Simulating Touch (Limited)

While development machines may not have touch screens, limited testing is possible:

```csharp
// Mouse drag simulates pan gesture
// Mouse wheel can simulate zoom (perspective adjustment)
// But pinch/stretch require actual multi-touch hardware
```

## Troubleshooting

**Issue**: Touch gestures not working
- Ensure application runs on touch-enabled device
- Verify Windows touch drivers are installed
- Check that .NET framework version supports touch (4.0+)

**Issue**: Pinch/stretch not responsive
- Test on device with multi-touch support (not all touch screens support multi-touch)
- Ensure no other applications are consuming touch events

**Issue**: Flick gesture too sensitive
- Reduce `TransitionSpeed` value
- Test with actual touch hardware (not mouse simulation)

**Issue**: Pan gesture choppy
- Reduce image sizes
- Optimize carousel settings for performance
- Disable `ShowImageShadow` if needed

## Best Practices

1. **Test on Real Hardware**: Always test touch gestures on actual touch devices

2. **Provide Instructions**: Show touch gesture hints, especially for first-time users

3. **Optimize for Performance**: Keep image count and sizes reasonable for smooth gestures

4. **Responsive Feedback**: Provide visual feedback during touch interactions

5. **Support Both**: Ensure mouse and touch interactions work well

6. **Moderate Settings**: Use balanced perspective and speed values for touch

7. **Full Screen**: Consider maximizing carousel size for better touch targets

8. **Auto-Hide Help**: Show gesture instructions initially, then auto-hide

## Framework Support

Touch gestures require:
- **.NET Framework 4.0** or higher
- **Windows 7** or higher
- **Touch-enabled hardware**

## Next Steps

- **Visual Effects**: See [visual-configuration.md](visual-configuration.md) for perspective control that works with pinch/stretch
- **Rotation**: See [rotation-behavior.md](rotation-behavior.md) for combining auto-rotation with touch control
- **Paths**: See [carousel-paths.md](carousel-paths.md) for choosing touch-friendly carousel paths
