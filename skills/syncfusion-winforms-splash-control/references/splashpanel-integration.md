# SplashPanel Integration

## Table of Contents
- [Overview](#overview)
- [SplashPanel vs SplashImage](#splashpanel-vs-splashimage)
- [Key Properties](#key-properties)
- [Integration Workflow](#integration-workflow)
- [Designing Custom SplashPanels](#designing-custom-splashpanels)
- [Complete Integration Examples](#complete-integration-examples)
- [Advanced Customization](#advanced-customization)
- [Best Practices](#best-practices)

## Overview

The SplashControl supports two modes of splash screen display:
1. **SplashImage:** Display a static image (covered in image-animation.md)
2. **Custom SplashPanel:** Display a fully customizable panel with any controls

Custom SplashPanels enable rich splash screens with progress bars, status text, animations, and any Windows Forms controls.

## SplashPanel vs SplashImage

### Comparison

| Feature | SplashImage | Custom SplashPanel |
|---------|-------------|-------------------|
| **Simplicity** | Very simple | Requires design |
| **Flexibility** | Limited to static images | Full control with any controls |
| **Progress indication** | Not available | Easy to add |
| **Dynamic content** | Not available | Text, controls can update |
| **File size** | Single image file | Code-based |
| **Design time** | Quick | More involved |

### When to Use Each

**Use SplashImage when:**
- You have a static logo or branding image
- No dynamic content needed
- Quick implementation required
- Simple visual requirements

**Use Custom SplashPanel when:**
- Need progress indication
- Want dynamic status text
- Require multiple visual elements
- Need animation or effects
- Want fully branded, rich splash experience

## Key Properties

### SplashControl Properties for SplashPanel Integration

| Property | Type | Description |
|----------|------|-------------|
| **CustomSplashPanel** | SplashPanel | The custom panel to display |
| **UseCustomSplashPanel** | bool | Enable custom panel mode |
| **SplashControlPanel** | SplashPanel | Gets/sets the internal SplashPanel |
| **ShowInTaskbar** | bool | Show splash in Windows taskbar |
| **FormIcon** | Icon | Icon for the SplashPanel window |

### SplashPanel Properties

| Property | Type | Description |
|----------|------|-------------|
| **BackgroundColor** | BrushInfo | Background gradient or solid color |
| **DesktopAlignment** | SplashAlignment | Position on desktop |
| **ShowAnimation** | bool | Enable animation effect |
| **AnimationSpeed** | int | Speed of animation |
| **TimerInterval** | int | Display duration in milliseconds |
| **SuspendAutoCloseWhenMouseOver** | bool | Keep open when mouse hovers |

## Integration Workflow

### Step-by-Step Integration

**Step 1: Add Required Assemblies**

```csharp
// Required namespaces
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Drawing;
```

**Step 2: Create and Design SplashPanel**

Drag SplashPanel from toolbox or create programmatically:

```csharp
SplashPanel splashPanel1 = new SplashPanel();
```

**Step 3: Customize SplashPanel**

Design the panel with controls (labels, progress bars, etc.):

```csharp
// Set size and appearance
splashPanel1.Size = new Size(400, 250);
splashPanel1.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.White,
    Color.LightBlue
);

// Add controls
Label statusLabel = new Label
{
    Text = "Loading Application...",
    Location = new Point(50, 100),
    AutoSize = true,
    Font = new Font("Segoe UI", 12, FontStyle.Bold),
    BackColor = Color.Transparent
};
splashPanel1.Controls.Add(statusLabel);
```

**Step 4: Link to SplashControl**

```csharp
// Assign custom panel
splashControl1.CustomSplashPanel = splashPanel1;

// Enable custom panel mode (CRITICAL!)
splashControl1.UseCustomSplashPanel = true;
```

**Step 5: Configure Display Settings**

```csharp
splashControl1.HostForm = this;
splashControl1.TimerInterval = 5000;
splashControl1.AutoMode = true;
```

## Designing Custom SplashPanels

### Basic SplashPanel with Label

**C# Example:**

```csharp
private void CreateBasicSplashPanel()
{
    // Create SplashPanel
    SplashPanel splashPanel = new SplashPanel();
    splashPanel.Size = new Size(400, 200);
    splashPanel.BackgroundColor = new BrushInfo(Color.LightSteelBlue);
    
    // Add label
    Label titleLabel = new Label
    {
        Text = "My Application v1.0",
        Location = new Point(100, 80),
        AutoSize = true,
        Font = new Font("Arial", 16, FontStyle.Bold),
        BackColor = Color.Transparent,
        ForeColor = Color.DarkBlue
    };
    splashPanel.Controls.Add(titleLabel);
    
    // Integrate with SplashControl
    splashControl1.CustomSplashPanel = splashPanel;
    splashControl1.UseCustomSplashPanel = true;
    splashControl1.HostForm = this;
}
```

**VB.NET Example:**

```vb
Private Sub CreateBasicSplashPanel()
    ' Create SplashPanel
    Dim splashPanel As New SplashPanel()
    splashPanel.Size = New Size(400, 200)
    splashPanel.BackgroundColor = New BrushInfo(Color.LightSteelBlue)
    
    ' Add label
    Dim titleLabel As New Label With {
        .Text = "My Application v1.0",
        .Location = New Point(100, 80),
        .AutoSize = True,
        .Font = New Font("Arial", 16, FontStyle.Bold),
        .BackColor = Color.Transparent,
        .ForeColor = Color.DarkBlue
    }
    splashPanel.Controls.Add(titleLabel)
    
    ' Integrate with SplashControl
    splashControl1.CustomSplashPanel = splashPanel
    splashControl1.UseCustomSplashPanel = True
    splashControl1.HostForm = Me
End Sub
```

### SplashPanel with Progress Bar

```csharp
private SplashPanel CreateProgressSplashPanel()
{
    SplashPanel splashPanel = new SplashPanel();
    splashPanel.Size = new Size(450, 280);
    
    // Gradient background
    splashPanel.BackgroundColor = new BrushInfo(
        GradientStyle.Vertical,
        new Color[] {
            Color.WhiteSmoke,
            Color.SteelBlue,
            Color.LightSteelBlue
        }
    );
    
    // Title label
    Label titleLabel = new Label
    {
        Text = "Application Loading",
        Location = new Point(120, 60),
        AutoSize = true,
        Font = new Font("Segoe UI", 18, FontStyle.Bold),
        BackColor = Color.Transparent,
        ForeColor = Color.White
    };
    splashPanel.Controls.Add(titleLabel);
    
    // Status label
    Label statusLabel = new Label
    {
        Name = "statusLabel",
        Text = "Initializing...",
        Location = new Point(150, 120),
        AutoSize = true,
        Font = new Font("Segoe UI", 10),
        BackColor = Color.Transparent,
        ForeColor = Color.White
    };
    splashPanel.Controls.Add(statusLabel);
    
    // Progress bar
    ProgressBar progressBar = new ProgressBar
    {
        Name = "progressBar",
        Location = new Point(75, 160),
        Size = new Size(300, 25),
        Style = ProgressBarStyle.Continuous
    };
    splashPanel.Controls.Add(progressBar);
    
    // Version label
    Label versionLabel = new Label
    {
        Text = "Version 2.0.1",
        Location = new Point(180, 220),
        AutoSize = true,
        Font = new Font("Segoe UI", 8),
        BackColor = Color.Transparent,
        ForeColor = Color.White
    };
    splashPanel.Controls.Add(versionLabel);
    
    return splashPanel;
}
```

### SplashPanel with Multiple Elements

```csharp
private SplashPanel CreateRichSplashPanel()
{
    SplashPanel splashPanel = new SplashPanel();
    splashPanel.Size = new Size(500, 350);
    splashPanel.DesktopAlignment = SplashAlignment.Center;
    splashPanel.ShowAnimation = true;
    splashPanel.AnimationSpeed = 10;
    
    // Complex gradient background
    splashPanel.BackgroundColor = new BrushInfo(
        GradientStyle.Vertical,
        new Color[] {
            SystemColors.HighlightText,
            SystemColors.Highlight,
            Color.PeachPuff,
            Color.LightSeaGreen,
            Color.Firebrick
        }
    );
    
    // Logo PictureBox
    PictureBox logoPictureBox = new PictureBox
    {
        Image = Properties.Resources.CompanyLogo,
        Location = new Point(175, 30),
        Size = new Size(150, 60),
        SizeMode = PictureBoxSizeMode.Zoom,
        BackColor = Color.Transparent
    };
    splashPanel.Controls.Add(logoPictureBox);
    
    // Application name
    Label appNameLabel = new Label
    {
        Text = "Enterprise Suite",
        Location = new Point(150, 110),
        AutoSize = true,
        Font = new Font("Segoe UI", 20, FontStyle.Bold),
        BackColor = Color.Transparent,
        ForeColor = SystemColors.Info
    };
    splashPanel.Controls.Add(appNameLabel);
    
    // Loading message
    Label loadingLabel = new Label
    {
        Name = "loadingLabel",
        Text = "Loading modules...",
        Location = new Point(180, 160),
        AutoSize = true,
        Font = new Font("Segoe UI", 10),
        BackColor = Color.Transparent
    };
    splashPanel.Controls.Add(loadingLabel);
    
    // Progress indicator
    ProgressBar progressBar = new ProgressBar
    {
        Name = "progressBar",
        Location = new Point(100, 200),
        Size = new Size(300, 20),
        Style = ProgressBarStyle.Marquee
    };
    splashPanel.Controls.Add(progressBar);
    
    // Copyright notice
    Label copyrightLabel = new Label
    {
        Text = $"© {DateTime.Now.Year} Company Name. All rights reserved.",
        Location = new Point(120, 280),
        AutoSize = true,
        Font = new Font("Segoe UI", 8),
        BackColor = Color.Transparent
    };
    splashPanel.Controls.Add(copyrightLabel);
    
    return splashPanel;
}
```

## Complete Integration Examples

### Example 1: Designer-Based Integration

**Designer Steps:**

1. Drag SplashControl onto form (appears in component tray)
2. Drag SplashPanel onto form (visible control)
3. Design the SplashPanel with controls using designer
4. Set SplashControl properties:
   - CustomSplashPanel → (select your SplashPanel)
   - UseCustomSplashPanel → True
   - HostForm → (select your form)
   - TimerInterval → 5000

**Generated Designer Code:**

```csharp
private void InitializeComponent()
{
    this.splashControl1 = new SplashControl();
    this.splashPanel1 = new SplashPanel();
    this.label1 = new Label();
    this.splashPanel1.SuspendLayout();
    this.SuspendLayout();
    
    // splashControl1
    this.splashControl1.CustomSplashPanel = this.splashPanel1;
    this.splashControl1.HostForm = this;
    this.splashControl1.TimerInterval = 5000;
    this.splashControl1.UseCustomSplashPanel = true;
    
    // splashPanel1
    this.splashPanel1.BackgroundColor = new BrushInfo(
        GradientStyle.Vertical,
        Color.White,
        Color.LightBlue
    );
    this.splashPanel1.Controls.Add(this.label1);
    this.splashPanel1.DesktopAlignment = SplashAlignment.Center;
    this.splashPanel1.Size = new Size(400, 200);
    
    // label1
    this.label1.BackColor = Color.Transparent;
    this.label1.Font = new Font("Segoe UI", 14F, FontStyle.Bold);
    this.label1.Location = new Point(120, 80);
    this.label1.Text = "Loading...";
    
    this.splashPanel1.ResumeLayout(false);
}
```

### Example 2: Code-Based Integration with Progress Updates

```csharp
public class ProgressSplashExample : Form
{
    private SplashControl splashControl1;
    private SplashPanel splashPanel1;
    private Label statusLabel;
    private ProgressBar progressBar;
    
    public ProgressSplashExample()
    {
        InitializeComponent();
        CreateCustomSplashPanel();
        InitializeWithProgress();
    }
    
    private void CreateCustomSplashPanel()
    {
        // Create SplashPanel
        splashPanel1 = new SplashPanel();
        splashPanel1.Size = new Size(450, 250);
        splashPanel1.BackgroundColor = new BrushInfo(
            GradientStyle.Vertical,
            Color.FromArgb(45, 45, 48),
            Color.FromArgb(28, 28, 28)
        );
        splashPanel1.DesktopAlignment = SplashAlignment.Center;
        splashPanel1.ShowAnimation = true;
        splashPanel1.TimerInterval = 30000; // Long backup timer
        
        // Add title
        Label titleLabel = new Label
        {
            Text = "Application Starting",
            Location = new Point(120, 60),
            AutoSize = true,
            Font = new Font("Segoe UI", 16, FontStyle.Bold),
            BackColor = Color.Transparent,
            ForeColor = Color.White
        };
        splashPanel1.Controls.Add(titleLabel);
        
        // Add status label
        statusLabel = new Label
        {
            Name = "statusLabel",
            Text = "Initializing...",
            Location = new Point(150, 120),
            Size = new Size(300, 20),
            Font = new Font("Segoe UI", 9),
            BackColor = Color.Transparent,
            ForeColor = Color.LightGray
        };
        splashPanel1.Controls.Add(statusLabel);
        
        // Add progress bar
        progressBar = new ProgressBar
        {
            Name = "progressBar",
            Location = new Point(75, 160),
            Size = new Size(300, 20),
            Minimum = 0,
            Maximum = 100,
            Value = 0
        };
        splashPanel1.Controls.Add(progressBar);
        
        // Integrate with SplashControl
        splashControl1 = new SplashControl();
        splashControl1.CustomSplashPanel = splashPanel1;
        splashControl1.UseCustomSplashPanel = true;
        splashControl1.HostForm = this;
        splashControl1.HideHostForm = true;
        
        // Add SplashPanel to form's controls
        this.Controls.Add(splashPanel1);
    }
    
    private async void InitializeWithProgress()
    {
        splashControl1.ShowSplash(true);
        
        try
        {
            await UpdateProgress(20, "Loading configuration...");
            await Task.Delay(800);
            
            await UpdateProgress(40, "Connecting to database...");
            await Task.Delay(1200);
            
            await UpdateProgress(60, "Loading user preferences...");
            await Task.Delay(600);
            
            await UpdateProgress(80, "Initializing UI components...");
            await Task.Delay(900);
            
            await UpdateProgress(100, "Complete!");
            await Task.Delay(500);
        }
        finally
        {
            splashControl1.HideSplash();
        }
    }
    
    private Task UpdateProgress(int value, string status)
    {
        return Task.Run(() =>
        {
            this.Invoke(new Action(() =>
            {
                progressBar.Value = value;
                statusLabel.Text = status;
            }));
        });
    }
}
```

### Example 3: Animated Splash with Timer

```csharp
public class AnimatedSplashExample : Form
{
    private SplashControl splashControl1;
    private SplashPanel splashPanel1;
    private Label statusLabel;
    private System.Windows.Forms.Timer animationTimer;
    private int dotCount = 0;
    
    public AnimatedSplashExample()
    {
        InitializeComponent();
        CreateAnimatedSplashPanel();
    }
    
    private void CreateAnimatedSplashPanel()
    {
        // Create and configure SplashPanel
        splashPanel1 = new SplashPanel();
        splashPanel1.Size = new Size(400, 200);
        splashPanel1.BackgroundColor = new BrushInfo(Color.DarkSlateGray);
        
        // Status label with animated dots
        statusLabel = new Label
        {
            Text = "Loading",
            Location = new Point(150, 90),
            AutoSize = true,
            Font = new Font("Segoe UI", 14, FontStyle.Bold),
            BackColor = Color.Transparent,
            ForeColor = Color.White
        };
        splashPanel1.Controls.Add(statusLabel);
        
        // Animation timer
        animationTimer = new System.Windows.Forms.Timer();
        animationTimer.Interval = 500;
        animationTimer.Tick += AnimationTimer_Tick;
        
        // Configure SplashControl
        splashControl1 = new SplashControl();
        splashControl1.CustomSplashPanel = splashPanel1;
        splashControl1.UseCustomSplashPanel = true;
        splashControl1.HostForm = this;
        splashControl1.TimerInterval = 5000;
        
        // Start animation when splash displays
        splashControl1.SplashDisplayed += (s, e) => animationTimer.Start();
        splashControl1.SplashClosed += (s, e) => animationTimer.Stop();
        
        this.Controls.Add(splashPanel1);
    }
    
    private void AnimationTimer_Tick(object sender, EventArgs e)
    {
        dotCount = (dotCount + 1) % 4;
        statusLabel.Text = "Loading" + new string('.', dotCount);
    }
}
```

## Advanced Customization

### ShowInTaskbar and FormIcon

Configure how the splash panel appears in Windows:

```csharp
private void ConfigureTaskbarAppearance()
{
    splashControl1.ShowInTaskbar = true;
    
    // Set custom icon
    splashControl1.FormIcon = new Icon("company_icon.ico");
    // Or from resources:
    splashControl1.FormIcon = Properties.Resources.AppIcon;
}
```

### SuspendAutoCloseWhenMouseOver

Prevent splash from closing when user hovers mouse over it:

```csharp
splashPanel1.SuspendAutoCloseWhenMouseOver = true;
```

### Custom Fonts and Styling

```csharp
private void ApplyCustomStyling()
{
    splashPanel1.Font = new Font("Comic Sans MS", 12F, FontStyle.Bold);
    splashPanel1.ForeColor = SystemColors.Info;
    
    // Apply to child controls
    foreach (Control control in splashPanel1.Controls)
    {
        if (control is Label)
        {
            control.Font = new Font("Segoe UI", 10F);
        }
    }
}
```

## Best Practices

### Design Best Practices

1. **Keep it simple:** Don't overload with too many elements
2. **Use clear hierarchy:** Title > Status > Progress > Details
3. **Readable fonts:** Minimum 10pt for status text
4. **Contrast matters:** Ensure text is readable on background
5. **Brand consistency:** Match application's visual design

### Implementation Best Practices

1. **Always set UseCustomSplashPanel = true:** Critical for custom panels to display
2. **Add to form controls:** `this.Controls.Add(splashPanel1);`
3. **Test thoroughly:** Verify all controls display correctly
4. **Handle threading:** Use `Invoke` for UI updates from background threads
5. **Dispose properly:** Clean up resources when done

### Performance Best Practices

1. **Avoid heavy operations:** Keep splash panel lightweight
2. **Optimize images:** Use appropriately sized images
3. **Minimize controls:** Only add necessary controls
4. **Test on slow hardware:** Ensure smooth display on various systems

### Common Pitfalls

1. **Forgetting UseCustomSplashPanel:** Panel won't display without this
2. **Not adding to Controls:** SplashPanel must be added to form's Controls collection
3. **Threading issues:** Update controls from UI thread only
4. **Timer conflicts:** Manage timers carefully to avoid conflicts
5. **Memory leaks:** Dispose of custom panels and resources properly

### Complete Configuration Checklist

- [ ] SplashPanel created and designed
- [ ] CustomSplashPanel property set
- [ ] UseCustomSplashPanel = true
- [ ] SplashPanel added to form's Controls
- [ ] HostForm property configured
- [ ] TimerInterval set appropriately
- [ ] Background color/gradient configured
- [ ] All child controls added with proper positioning
- [ ] Threading handled correctly for updates
- [ ] Tested on various screen resolutions
