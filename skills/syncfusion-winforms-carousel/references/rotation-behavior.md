# Rotation Behavior

This guide covers the `RotateAlways` property and how to configure automatic continuous rotation in the Carousel control.

## RotateAlways Property

The `RotateAlways` property enables items in the carousel to rotate continuously without user interaction.

### Property Details

- **Type**: `bool`
- **Default**: `false`
- **Effect**: When `true`, carousel items rotate automatically

### Basic Usage

**C# Example:**

```csharp
Carousel carousel1 = new Carousel();
carousel1.RotateAlways = true; // Enable continuous rotation
carousel1.TransitionSpeed = 2.0f; // Control rotation speed
```

**VB.NET Example:**

```vb
Dim carousel1 As New Carousel()
carousel1.RotateAlways = True ' Enable continuous rotation
carousel1.TransitionSpeed = 2.0F ' Control rotation speed
```

## Auto-Play Configuration

### Simple Auto-Rotating Carousel

```csharp
Carousel autoCarousel = new Carousel();
autoCarousel.Size = new Size(600, 400);
autoCarousel.Location = new Point(50, 50);

// Enable auto-rotation
autoCarousel.RotateAlways = true;
autoCarousel.TransitionSpeed = 2.0f;

// Add items
for (int i = 1; i <= 8; i++)
{
    ButtonAdv btn = new ButtonAdv();
    btn.Text = $"Item {i}";
    btn.Size = new Size(100, 80);
    btn.BackColor = Color.FromArgb(22, 165, 220);
    btn.ForeColor = Color.White;
    
    autoCarousel.Controls.Add(btn);
    autoCarousel.Items.Add(btn);
}

this.Controls.Add(autoCarousel);
```

### Auto-Rotating Image Slideshow

```csharp
Carousel slideshowCarousel = new Carousel();
slideshowCarousel.Dock = DockStyle.Fill;
slideshowCarousel.ImageSlides = true;

// Auto-play slideshow
slideshowCarousel.RotateAlways = true;
slideshowCarousel.TransitionSpeed = 1.5f; // Slower for viewing

// Visual settings
slideshowCarousel.Perspective = 4.5f;
slideshowCarousel.ShowImagePreview = true;
slideshowCarousel.ShowImageShadow = true;
slideshowCarousel.BackColor = Color.Black;

// Load images
slideshowCarousel.FilePath = Path.Combine(Application.StartupPath, "Slideshow");

this.Controls.Add(slideshowCarousel);
```

### Dashboard with Auto-Rotating Panels

```csharp
Carousel dashboardCarousel = new Carousel();
dashboardCarousel.Size = new Size(800, 600);
dashboardCarousel.CarouselPath = CarouselPath.Orbital;

// Configure auto-rotation
dashboardCarousel.RotateAlways = true;
dashboardCarousel.TransitionSpeed = 1.0f; // Slow rotation for dashboards
dashboardCarousel.Perspective = 3.5f;

// Add dashboard panels
string[] metrics = { "Sales", "Inventory", "Orders", "Revenue", "Traffic", "Users" };

foreach (string metric in metrics)
{
    Panel panel = new Panel();
    panel.Size = new Size(150, 120);
    panel.BackColor = Color.FromArgb(52, 73, 94);
    
    Label lblTitle = new Label();
    lblTitle.Text = metric;
    lblTitle.Font = new Font("Segoe UI", 12F, FontStyle.Bold);
    lblTitle.ForeColor = Color.White;
    lblTitle.Location = new Point(10, 10);
    lblTitle.AutoSize = true;
    
    Label lblValue = new Label();
    lblValue.Text = "$0";
    lblValue.Font = new Font("Segoe UI", 18F, FontStyle.Bold);
    lblValue.ForeColor = Color.FromArgb(46, 204, 113);
    lblValue.Location = new Point(10, 50);
    lblValue.AutoSize = true;
    
    panel.Controls.Add(lblTitle);
    panel.Controls.Add(lblValue);
    
    dashboardCarousel.Controls.Add(panel);
    dashboardCarousel.Items.Add(panel);
}

this.Controls.Add(dashboardCarousel);
```

## Controlling Rotation Programmatically

### Start/Stop Rotation

```csharp
// Start rotation
private void btnStart_Click(object sender, EventArgs e)
{
    carousel1.RotateAlways = true;
    btnStart.Enabled = false;
    btnStop.Enabled = true;
}

// Stop rotation
private void btnStop_Click(object sender, EventArgs e)
{
    carousel1.RotateAlways = false;
    btnStart.Enabled = true;
    btnStop.Enabled = false;
}
```

### Toggle Rotation

```csharp
private void btnToggleRotation_Click(object sender, EventArgs e)
{
    carousel1.RotateAlways = !carousel1.RotateAlways;
    
    btnToggleRotation.Text = carousel1.RotateAlways 
        ? "⏸ Pause Rotation" 
        : "▶ Start Rotation";
}
```

### Rotation with Play/Pause

```csharp
public partial class Form1 : Form
{
    private Carousel carousel1;
    private bool isRotating = false;
    
    private void InitializeCarouselControls()
    {
        carousel1 = new Carousel();
        carousel1.ImageSlides = true;
        carousel1.Dock = DockStyle.Fill;
        carousel1.TransitionSpeed = 2.0f;
        carousel1.FilePath = "Images";
        
        // Initially not rotating
        carousel1.RotateAlways = false;
        
        // Play button
        Button btnPlay = new Button();
        btnPlay.Text = "▶ Play";
        btnPlay.Click += (s, e) => PlayRotation();
        
        // Pause button
        Button btnPause = new Button();
        btnPause.Text = "⏸ Pause";
        btnPause.Click += (s, e) => PauseRotation();
        
        this.Controls.Add(carousel1);
        this.Controls.Add(btnPlay);
        this.Controls.Add(btnPause);
    }
    
    private void PlayRotation()
    {
        carousel1.RotateAlways = true;
        isRotating = true;
    }
    
    private void PauseRotation()
    {
        carousel1.RotateAlways = false;
        isRotating = false;
    }
}
```

## Integration with Transition Speed

The rotation speed is controlled by the `TransitionSpeed` property when `RotateAlways` is enabled.

### Speed Presets for Auto-Rotation

```csharp
// Very slow (leisurely viewing)
carousel1.RotateAlways = true;
carousel1.TransitionSpeed = 0.8f;

// Slow (comfortable viewing)
carousel1.RotateAlways = true;
carousel1.TransitionSpeed = 1.5f;

// Medium (balanced)
carousel1.RotateAlways = true;
carousel1.TransitionSpeed = 2.0f;

// Fast (quick browsing)
carousel1.RotateAlways = true;
carousel1.TransitionSpeed = 3.0f;

// Very fast (rapid cycling)
carousel1.RotateAlways = true;
carousel1.TransitionSpeed = 4.5f;
```

### Adjustable Speed Control

```csharp
private void InitializeSpeedControl()
{
    carousel1.RotateAlways = true;
    
    // TrackBar for speed adjustment
    TrackBar trackBarSpeed = new TrackBar();
    trackBarSpeed.Minimum = 5;   // 0.5x speed
    trackBarSpeed.Maximum = 50;  // 5.0x speed
    trackBarSpeed.Value = 20;    // 2.0x speed (default)
    trackBarSpeed.TickFrequency = 5;
    
    trackBarSpeed.Scroll += (s, e) => {
        float speed = trackBarSpeed.Value / 10.0f;
        carousel1.TransitionSpeed = speed;
        lblSpeed.Text = $"Speed: {speed:F1}x";
    };
    
    this.Controls.Add(trackBarSpeed);
}
```

## User Interaction During Rotation

Users can still interact with the carousel while auto-rotation is enabled.

### Pause on User Interaction

```csharp
public partial class Form1 : Form
{
    private Carousel carousel1;
    private bool wasRotating = false;
    
    private void SetupInteractivePause()
    {
        carousel1.RotateAlways = true;
        carousel1.TransitionSpeed = 2.0f;
        
        // Pause when user clicks
        carousel1.MouseDown += (s, e) => {
            if (carousel1.RotateAlways)
            {
                wasRotating = true;
                carousel1.RotateAlways = false;
            }
        };
        
        // Resume after delay
        Timer resumeTimer = new Timer();
        resumeTimer.Interval = 3000; // 3 seconds
        resumeTimer.Tick += (s, e) => {
            if (wasRotating)
            {
                carousel1.RotateAlways = true;
                wasRotating = false;
            }
            resumeTimer.Stop();
        };
        
        carousel1.MouseUp += (s, e) => {
            if (wasRotating)
            {
                resumeTimer.Start();
            }
        };
    }
}
```

### Manual Override

```csharp
private void carousel1_MouseEnter(object sender, EventArgs e)
{
    // Pause rotation when mouse enters
    if (carousel1.RotateAlways)
    {
        carousel1.RotateAlways = false;
        lblStatus.Text = "Paused (hover)";
    }
}

private void carousel1_MouseLeave(object sender, EventArgs e)
{
    // Resume rotation when mouse leaves
    carousel1.RotateAlways = true;
    lblStatus.Text = "Auto-rotating";
}
```

## Complete Auto-Rotation Examples

### Example 1: Product Showcase with Auto-Rotation

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace ProductShowcase
{
    public partial class Form1 : Form
    {
        private Carousel productCarousel;
        
        public Form1()
        {
            InitializeComponent();
            CreateProductShowcase();
        }
        
        private void CreateProductShowcase()
        {
            productCarousel = new Carousel();
            productCarousel.Size = new Size(800, 600);
            productCarousel.Location = new Point(50, 50);
            productCarousel.ImageSlides = true;
            
            // Auto-rotation settings
            productCarousel.RotateAlways = true;
            productCarousel.TransitionSpeed = 2.0f;
            
            // Visual settings
            productCarousel.CarouselPath = CarouselPath.Default;
            productCarousel.Perspective = 4.5f;
            productCarousel.ShowImagePreview = true;
            productCarousel.ShowImageShadow = true;
            productCarousel.ImageHighlightColor = Color.Gold;
            
            // Load product images
            productCarousel.FilePath = "Products";
            
            this.Controls.Add(productCarousel);
        }
    }
}
```

### Example 2: Slideshow with Controls

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace Slideshow
{
    public partial class Form1 : Form
    {
        private Carousel slideshowCarousel;
        private Button btnPlayPause;
        private TrackBar trackBarSpeed;
        private bool isPlaying = false;
        
        public Form1()
        {
            InitializeComponent();
            InitializeSlideshow();
            CreateControls();
        }
        
        private void InitializeSlideshow()
        {
            slideshowCarousel = new Carousel();
            slideshowCarousel.Size = new Size(900, 600);
            slideshowCarousel.Location = new Point(50, 50);
            slideshowCarousel.ImageSlides = true;
            slideshowCarousel.FilePath = "Slideshow";
            
            // Initial settings
            slideshowCarousel.RotateAlways = false;
            slideshowCarousel.TransitionSpeed = 1.5f;
            slideshowCarousel.ShowImagePreview = true;
            slideshowCarousel.BackColor = Color.Black;
            
            this.Controls.Add(slideshowCarousel);
        }
        
        private void CreateControls()
        {
            // Play/Pause button
            btnPlayPause = new Button();
            btnPlayPause.Text = "▶ Play";
            btnPlayPause.Size = new Size(100, 40);
            btnPlayPause.Location = new Point(400, 670);
            btnPlayPause.Click += BtnPlayPause_Click;
            this.Controls.Add(btnPlayPause);
            
            // Speed control
            Label lblSpeed = new Label();
            lblSpeed.Text = "Speed:";
            lblSpeed.Location = new Point(520, 675);
            lblSpeed.AutoSize = true;
            this.Controls.Add(lblSpeed);
            
            trackBarSpeed = new TrackBar();
            trackBarSpeed.Minimum = 5;
            trackBarSpeed.Maximum = 40;
            trackBarSpeed.Value = 15;
            trackBarSpeed.Size = new Size(200, 45);
            trackBarSpeed.Location = new Point(580, 665);
            trackBarSpeed.Scroll += TrackBarSpeed_Scroll;
            this.Controls.Add(trackBarSpeed);
        }
        
        private void BtnPlayPause_Click(object sender, EventArgs e)
        {
            isPlaying = !isPlaying;
            slideshowCarousel.RotateAlways = isPlaying;
            btnPlayPause.Text = isPlaying ? "⏸ Pause" : "▶ Play";
        }
        
        private void TrackBarSpeed_Scroll(object sender, EventArgs e)
        {
            float speed = trackBarSpeed.Value / 10.0f;
            slideshowCarousel.TransitionSpeed = speed;
        }
    }
}
```

### Example 3: Auto-Rotating Dashboard

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace Dashboard
{
    public partial class Form1 : Form
    {
        private Carousel dashboardCarousel;
        private Timer updateTimer;
        
        public Form1()
        {
            InitializeComponent();
            CreateDashboard();
            StartDataUpdates();
        }
        
        private void CreateDashboard()
        {
            dashboardCarousel = new Carousel();
            dashboardCarousel.Dock = DockStyle.Fill;
            dashboardCarousel.CarouselPath = CarouselPath.Orbital;
            
            // Slow auto-rotation for dashboards
            dashboardCarousel.RotateAlways = true;
            dashboardCarousel.TransitionSpeed = 1.0f;
            dashboardCarousel.Perspective = 3.5f;
            dashboardCarousel.BackColor = Color.FromArgb(20, 20, 20);
            
            // Add metric panels
            AddMetricPanel("CPU Usage", "45%", Color.FromArgb(52, 152, 219));
            AddMetricPanel("Memory", "2.1 GB", Color.FromArgb(46, 204, 113));
            AddMetricPanel("Network", "125 Mbps", Color.FromArgb(155, 89, 182));
            AddMetricPanel("Disk", "67%", Color.FromArgb(241, 196, 15));
            AddMetricPanel("Active Users", "1,234", Color.FromArgb(230, 126, 34));
            AddMetricPanel("Requests", "5.2K/s", Color.FromArgb(231, 76, 60));
            
            this.Controls.Add(dashboardCarousel);
        }
        
        private void AddMetricPanel(string title, string value, Color color)
        {
            Panel panel = new Panel();
            panel.Size = new Size(180, 140);
            panel.BackColor = color;
            
            Label lblTitle = new Label();
            lblTitle.Text = title;
            lblTitle.Font = new Font("Segoe UI", 11F, FontStyle.Bold);
            lblTitle.ForeColor = Color.White;
            lblTitle.Location = new Point(15, 15);
            lblTitle.AutoSize = true;
            
            Label lblValue = new Label();
            lblValue.Text = value;
            lblValue.Font = new Font("Segoe UI", 22F, FontStyle.Bold);
            lblValue.ForeColor = Color.White;
            lblValue.Location = new Point(15, 55);
            lblValue.AutoSize = true;
            lblValue.Tag = title; // For updates
            
            panel.Controls.Add(lblTitle);
            panel.Controls.Add(lblValue);
            
            dashboardCarousel.Controls.Add(panel);
            dashboardCarousel.Items.Add(panel);
        }
        
        private void StartDataUpdates()
        {
            updateTimer = new Timer();
            updateTimer.Interval = 2000; // Update every 2 seconds
            updateTimer.Tick += UpdateMetrics;
            updateTimer.Start();
        }
        
        private void UpdateMetrics(object sender, EventArgs e)
        {
            Random rnd = new Random();
            
            foreach (Control item in dashboardCarousel.Controls)
            {
                if (item is Panel panel)
                {
                    foreach (Control ctrl in panel.Controls)
                    {
                        if (ctrl is Label lbl && lbl.Tag != null)
                        {
                            // Simulate metric updates
                            lbl.Text = $"{rnd.Next(10, 99)}%";
                        }
                    }
                }
            }
        }
    }
}
```

## Best Practices

1. **Appropriate Speed**: Use slower speeds (1.0-2.0) for auto-rotation to allow comfortable viewing

2. **User Control**: Provide play/pause buttons for user control over auto-rotation

3. **Pause on Interaction**: Consider pausing rotation when user interacts with carousel

4. **Dashboard Use**: Auto-rotation works well for dashboards showing multiple metrics

5. **Slideshow Mode**: Perfect for image slideshows and product showcases

6. **Performance**: Test auto-rotation with your actual content to ensure smooth performance

7. **Initial State**: Consider starting with rotation paused, letting users opt-in

8. **Visual Feedback**: Clearly indicate when carousel is auto-rotating vs manual control

## Troubleshooting

**Issue**: Rotation is too fast
- Decrease `TransitionSpeed` value (try 1.0-2.0 for comfortable viewing)

**Issue**: Rotation is choppy
- Reduce number of items in carousel
- Disable `ShowImageShadow` for better performance
- Optimize/compress images if using `ImageSlides`

**Issue**: Can't stop rotation
- Ensure `RotateAlways = false` is being set
- Check for conflicting event handlers

**Issue**: Rotation doesn't start
- Verify `RotateAlways = true` is set
- Ensure `TransitionSpeed` is > 0
- Check that carousel has items

## Next Steps

- **Visual Effects**: See [visual-configuration.md](visual-configuration.md) for combining auto-rotation with visual effects
- **Touch**: See [touch-interactions.md](touch-interactions.md) for manual control during auto-rotation
- **Paths**: See [carousel-paths.md](carousel-paths.md) for choosing best path for auto-rotation
