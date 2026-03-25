# Getting Started with SplashPanel

This guide covers installation, setup, and basic usage of the SplashPanel control.

## Assembly Deployment

The SplashPanel control requires the following assemblies:

**Required Assemblies:**
- `Syncfusion.Shared.Base.dll`
- `Syncfusion.Tools.Windows.dll`

**NuGet Package:**
```bash
Install-Package Syncfusion.Tools.Windows
```

Or install via NuGet Package Manager in Visual Studio.

## Creating a Project

Create a new Windows Forms project in Visual Studio to display the SplashPanel control.

## Adding SplashPanel via Designer

The SplashPanel control provides full support for the Windows Forms designer.

### Step 1: Add Control to Toolbox

1. Open Visual Studio and create/open a WinForms project
2. Right-click on the Toolbox and select "Choose Items"
3. Browse to Syncfusion assemblies location
4. Select `Syncfusion.Tools.Windows.dll`
5. Click OK to add the SplashPanel control to the Toolbox

### Step 2: Drag and Drop

1. Drag the SplashPanel control from the Toolbox onto the form
2. The control will be added to the form's component tray (non-visual control)

### Step 3: Configure Properties

Use the Properties window to configure the SplashPanel:

```csharp
// Key properties to set in designer
this.splashPanel1.DesktopAlignment = SplashAlignment.Center;
this.splashPanel1.TimerInterval = 5000; // 5 seconds
this.splashPanel1.ShowAnimation = true;
this.splashPanel1.Size = new Size(400, 300);
```

### Step 4: Add Child Controls

1. Select the SplashPanel in the component tray
2. Drag controls (labels, buttons, images) onto the form
3. Set each control's `Parent` property to the SplashPanel

### Step 5: Show the Splash

Call the `ShowSplash()` method in the form's Load event or constructor:

```csharp
private void Form1_Load(object sender, EventArgs e)
{
    this.splashPanel1.ShowSplash();
}
```

## Adding SplashPanel Programmatically

To create a SplashPanel programmatically with child controls:

### Step 1: Create Windows Forms Project

Create a new Visual C# or VB.NET Windows Forms application in Visual Studio.

### Step 2: Add Namespaces

```csharp
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Drawing;
using System.Drawing;
using System.Windows.Forms;
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms.Tools
Imports Syncfusion.Drawing
Imports System.Drawing
Imports System.Windows.Forms
```

### Step 3: Declare Controls

```csharp
private SplashPanel splashPanel1;
private Label titleLabel;
private Label versionLabel;
```

**VB.NET:**
```vb
Private splashPanel1 As SplashPanel
Private titleLabel As Label
Private versionLabel As Label
```

### Step 4: Initialize Controls

```csharp
public Form1()
{
    InitializeComponent();
    
    // Initialize splash panel
    splashPanel1 = new SplashPanel();
    
    // Initialize child controls
    titleLabel = new Label();
    versionLabel = new Label();
}
```

**VB.NET:**
```vb
Public Sub New()
    InitializeComponent()
    
    ' Initialize splash panel
    splashPanel1 = New SplashPanel()
    
    ' Initialize child controls
    titleLabel = New Label()
    versionLabel = New Label()
End Sub
```

### Step 5: Configure SplashPanel Properties

```csharp
private void SetupSplashPanel()
{
    // Basic properties
    this.splashPanel1.Size = new Size(450, 300);
    this.splashPanel1.Location = new Point(20, 20);
    this.splashPanel1.Name = "splashPanel1";
    
    // Display settings
    this.splashPanel1.DesktopAlignment = SplashAlignment.Center;
    this.splashPanel1.TimerInterval = 5000; // 5 seconds
    
    // Animation settings
    this.splashPanel1.ShowAnimation = true;
    this.splashPanel1.AnimationSpeed = 20;
    this.splashPanel1.SlideStyle = SlideStyle.FadeIn;
    this.splashPanel1.ShowAsTopMost = true;
    
    // Background settings
    this.splashPanel1.BackgroundColor = new BrushInfo(
        GradientStyle.Vertical,
        Color.DarkBlue,
        Color.LightBlue);
    
    // Add to form
    this.Controls.Add(this.splashPanel1);
}
```

**VB.NET:**
```vb
Private Sub SetupSplashPanel()
    ' Basic properties
    Me.splashPanel1.Size = New Size(450, 300)
    Me.splashPanel1.Location = New Point(20, 20)
    Me.splashPanel1.Name = "splashPanel1"
    
    ' Display settings
    Me.splashPanel1.DesktopAlignment = SplashAlignment.Center
    Me.splashPanel1.TimerInterval = 5000 ' 5 seconds
    
    ' Animation settings
    Me.splashPanel1.ShowAnimation = True
    Me.splashPanel1.AnimationSpeed = 20
    Me.splashPanel1.SlideStyle = SlideStyle.FadeIn
    Me.splashPanel1.ShowAsTopMost = True
    
    ' Background settings
    Me.splashPanel1.BackgroundColor = New BrushInfo( _
        GradientStyle.Vertical, _
        Color.DarkBlue, _
        Color.LightBlue)
    
    ' Add to form
    Me.Controls.Add(Me.splashPanel1)
End Sub
```

### Step 6: Add Child Controls

```csharp
private void AddChildControls()
{
    // Configure title label
    titleLabel.Text = "My Application";
    titleLabel.Font = new Font("Arial", 24, FontStyle.Bold);
    titleLabel.ForeColor = Color.White;
    titleLabel.Location = new Point(100, 100);
    titleLabel.AutoSize = true;
    
    // Configure version label
    versionLabel.Text = "Version 1.0";
    versionLabel.Font = new Font("Arial", 12);
    versionLabel.ForeColor = Color.White;
    versionLabel.Location = new Point(150, 150);
    versionLabel.AutoSize = true;
    
    // Add labels to splash panel
    this.splashPanel1.Controls.Add(titleLabel);
    this.splashPanel1.Controls.Add(versionLabel);
}
```

**VB.NET:**
```vb
Private Sub AddChildControls()
    ' Configure title label
    titleLabel.Text = "My Application"
    titleLabel.Font = New Font("Arial", 24, FontStyle.Bold)
    titleLabel.ForeColor = Color.White
    titleLabel.Location = New Point(100, 100)
    titleLabel.AutoSize = True
    
    ' Configure version label
    versionLabel.Text = "Version 1.0"
    versionLabel.Font = New Font("Arial", 12)
    versionLabel.ForeColor = Color.White
    versionLabel.Location = New Point(150, 150)
    versionLabel.AutoSize = True
    
    ' Add labels to splash panel
    Me.splashPanel1.Controls.Add(titleLabel)
    Me.splashPanel1.Controls.Add(versionLabel)
End Sub
```

### Step 7: Show the Splash

```csharp
public Form1()
{
    InitializeComponent();
    
    SetupSplashPanel();
    AddChildControls();
    
    // Show splash on form load
    this.Load += (s, e) => splashPanel1.ShowSplash();
}
```

**VB.NET:**
```vb
Public Sub New()
    InitializeComponent()
    
    SetupSplashPanel()
    AddChildControls()
    
    ' Show splash on form load
    AddHandler Me.Load, Sub(s, e) splashPanel1.ShowSplash()
End Sub
```

## Complete Example

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Drawing;

public class SplashForm : Form
{
    private SplashPanel splashPanel1;
    private Label welcomeLabel;
    
    public SplashForm()
    {
        InitializeComponent();
        SetupSplash();
    }
    
    private void InitializeComponent()
    {
        this.Text = "Splash Panel Example";
        this.Size = new Size(600, 400);
        this.StartPosition = FormStartPosition.CenterScreen;
    }
    
    private void SetupSplash()
    {
        // Create splash panel
        splashPanel1 = new SplashPanel();
        splashPanel1.Size = new Size(400, 250);
        splashPanel1.DesktopAlignment = SplashAlignment.Center;
        splashPanel1.TimerInterval = 4000;
        splashPanel1.ShowAnimation = true;
        splashPanel1.AnimationSpeed = 25;
        splashPanel1.SlideStyle = SlideStyle.FadeIn;
        splashPanel1.ShowAsTopMost = true;
        splashPanel1.BackgroundColor = new BrushInfo(
            GradientStyle.Vertical,
            Color.Navy,
            Color.SkyBlue);
        
        // Create welcome label
        welcomeLabel = new Label();
        welcomeLabel.Text = "Welcome to My App!";
        welcomeLabel.Font = new Font("Arial", 20, FontStyle.Bold);
        welcomeLabel.ForeColor = Color.White;
        welcomeLabel.AutoSize = true;
        welcomeLabel.Location = new Point(80, 100);
        
        // Add label to splash
        splashPanel1.Controls.Add(welcomeLabel);
        
        // Add splash to form
        this.Controls.Add(splashPanel1);
        
        // Show splash when form loads
        this.Load += (s, e) => splashPanel1.ShowSplash();
    }
    
    [STAThread]
    static void Main()
    {
        Application.EnableVisualStyles();
        Application.SetCompatibleTextRenderingDefault(false);
        Application.Run(new SplashForm());
    }
}
```

## Basic Usage Patterns

### Simple Timed Splash

```csharp
SplashPanel splash = new SplashPanel();
splash.Size = new Size(300, 200);
splash.DesktopAlignment = SplashAlignment.Center;
splash.TimerInterval = 3000; // Show for 3 seconds
this.Controls.Add(splash);
splash.ShowSplash();
```

### Splash with Background Image

```csharp
SplashPanel splash = new SplashPanel();
splash.Size = new Size(500, 350);
splash.BackgroundImage = Image.FromFile("splash.png");
splash.DesktopAlignment = SplashAlignment.Center;
splash.TimerInterval = 5000;
this.Controls.Add(splash);
splash.ShowSplash();
```

### Manual Control (No Auto-Close)

```csharp
SplashPanel splash = new SplashPanel();
splash.Size = new Size(400, 300);
splash.TimerInterval = -1; // No auto-close
this.Controls.Add(splash);
splash.ShowSplash();

// Later, manually hide
splash.HideSplash();
```

## Next Steps

- **Display methods:** See [display-methods.md](display-methods.md) for ShowSplash, HideSplash, ShowDialogSplash
- **Animation:** See [animation-appearance.md](animation-appearance.md) for animation and styling options
- **Transitions:** See [slide-transitions.md](slide-transitions.md) for slide and marquee animations
- **Events:** See [events.md](events.md) for handling splash lifecycle events
