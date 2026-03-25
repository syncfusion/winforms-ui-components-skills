# Host Form Settings

## Table of Contents
- [Overview](#overview)
- [HostForm Property](#hostform-property)
- [HideHostForm Property](#hidehostform-property)
- [HostFormWindowState Property](#hostformwindowstate-property)
- [Coordination Patterns](#coordination-patterns)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)

## Overview

The SplashControl provides fine-grained control over the host form's visibility and state during splash screen display. This enables professional startup experiences where the main application window appears only after initialization completes.

## HostForm Property

### Property Details

**Property:** `HostForm`  
**Type:** `System.Windows.Forms.Form`  
**Default:** `null`  
**Description:** Gets or sets the parent form of the SplashControl

### Setting the Host Form

The HostForm property identifies which form owns the SplashControl and controls its lifecycle.

**C# Example:**

```csharp
public Form1()
{
    InitializeComponent();
    
    // Set this form as the host
    splashControl1.HostForm = this;
    splashControl1.SplashImage = Properties.Resources.Logo;
    splashControl1.TimerInterval = 3000;
}
```

**VB.NET Example:**

```vb
Public Sub New()
    InitializeComponent()
    
    ' Set this form as the host
    splashControl1.HostForm = Me
    splashControl1.SplashImage = My.Resources.Logo
    splashControl1.TimerInterval = 3000
End Sub
```

### Why HostForm is Important

The HostForm property:
- Establishes parent-child relationship
- Enables AutoMode functionality
- Controls form visibility coordination
- Manages focus and activation
- Determines splash screen positioning context

**Without HostForm set:**
```csharp
// This will not work properly in AutoMode
splashControl1.AutoMode = true; // HostForm is null - AutoMode won't trigger
```

**With HostForm set:**
```csharp
// This works correctly
splashControl1.HostForm = this;
splashControl1.AutoMode = true; // Splash will display on form load
```

## HideHostForm Property

### Property Details

**Property:** `HideHostForm`  
**Type:** `bool`  
**Default:** `false`  
**Description:** Specifies if the host form should be hidden when the splash screen is displayed

### Basic Usage

**C# Example:**

```csharp
// Hide the main form during splash display
splashControl1.HostForm = this;
splashControl1.HideHostForm = true;
splashControl1.SplashImage = Properties.Resources.Logo;
splashControl1.TimerInterval = 4000;
splashControl1.AutoMode = true;
```

**VB.NET Example:**

```vb
' Hide the main form during splash display
splashControl1.HostForm = Me
splashControl1.HideHostForm = True
splashControl1.SplashImage = My.Resources.Logo
splashControl1.TimerInterval = 4000
splashControl1.AutoMode = True
```

### Behavior Comparison

**HideHostForm = false (Default):**
- Host form visible behind or alongside splash
- User may see partially loaded form
- Faster perceived startup
- Less polished appearance

**HideHostForm = true:**
- Host form completely hidden until splash closes
- Clean, professional startup experience
- Form appears fully initialized
- Better for complex initialization

### When to Use HideHostForm

**Use HideHostForm = true when:**
- Application requires significant initialization time
- You want to hide UI construction from users
- Creating a polished, professional experience
- Form has complex layouts that load progressively

**Use HideHostForm = false when:**
- Startup is very fast
- You want minimal delay to functionality
- Form is simple and loads quickly
- Application philosophy favors transparency

### Complete Example with HideHostForm

```csharp
public class ProfessionalStartup : Form
{
    private SplashControl splashControl1;
    
    public ProfessionalStartup()
    {
        InitializeComponent();
        ConfigureProfessionalSplash();
    }
    
    private void ConfigureProfessionalSplash()
    {
        splashControl1.HostForm = this;
        splashControl1.SplashImage = Properties.Resources.CompanyLogo;
        
        // Hide form during splash
        splashControl1.HideHostForm = true;
        
        // Display splash for initialization period
        splashControl1.TimerInterval = 4000;
        splashControl1.AutoMode = true;
        splashControl1.DesktopAlignment = SplashAlignment.Center;
        
        // Handle splash close to finalize setup
        splashControl1.SplashClosed += (s, e) =>
        {
            // Form will become visible now
            FinalizeInitialization();
        };
    }
    
    private void FinalizeInitialization()
    {
        // Perform final setup now that form is visible
        this.Focus();
        this.BringToFront();
    }
}
```

## HostFormWindowState Property

### Property Details

**Property:** `HostFormWindowState`  
**Type:** `System.Windows.Forms.FormWindowState`  
**Default:** `FormWindowState.Normal`  
**Description:** Specifies the window state of the host form when splash screen is displayed

**Note:** HideHostForm must be set to `true` for this property to take effect.

### Available Window States

| State | Description | Use Case |
|-------|-------------|----------|
| **Normal** | Standard window size and position | Default startup |
| **Minimized** | Form starts minimized to taskbar | Background applications |
| **Maximized** | Form fills entire screen | Full-screen applications |

### Setting Window State

**C# Example:**

```csharp
// Normal window state
splashControl1.HostForm = this;
splashControl1.HideHostForm = true;
splashControl1.HostFormWindowState = FormWindowState.Normal;

// Start maximized after splash
splashControl1.HostForm = this;
splashControl1.HideHostForm = true;
splashControl1.HostFormWindowState = FormWindowState.Maximized;

// Start minimized (for background apps)
splashControl1.HostForm = this;
splashControl1.HideHostForm = true;
splashControl1.HostFormWindowState = FormWindowState.Minimized;
```

**VB.NET Example:**

```vb
' Normal window state
splashControl1.HostForm = Me
splashControl1.HideHostForm = True
splashControl1.HostFormWindowState = FormWindowState.Normal

' Start maximized after splash
splashControl1.HostForm = Me
splashControl1.HideHostForm = True
splashControl1.HostFormWindowState = FormWindowState.Maximized
```

### Window State Scenarios

#### Scenario 1: Normal Desktop Application

```csharp
private void ConfigureNormalStartup()
{
    splashControl1.HostForm = this;
    splashControl1.HideHostForm = true;
    splashControl1.HostFormWindowState = FormWindowState.Normal;
    splashControl1.SplashImage = Properties.Resources.Logo;
    splashControl1.TimerInterval = 3000;
    splashControl1.AutoMode = true;
}
```

#### Scenario 2: Maximized Application Startup

```csharp
private void ConfigureMaximizedStartup()
{
    splashControl1.HostForm = this;
    splashControl1.HideHostForm = true;
    splashControl1.HostFormWindowState = FormWindowState.Maximized;
    splashControl1.SplashImage = Properties.Resources.Logo;
    splashControl1.TimerInterval = 3000;
    splashControl1.AutoMode = true;
    
    // Application will appear maximized after splash closes
}
```

#### Scenario 3: Background Application with Tray Icon

```csharp
private void ConfigureBackgroundStartup()
{
    splashControl1.HostForm = this;
    splashControl1.HideHostForm = true;
    splashControl1.HostFormWindowState = FormWindowState.Minimized;
    splashControl1.SplashImage = Properties.Resources.Logo;
    splashControl1.TimerInterval = 2000;
    splashControl1.AutoMode = true;
    
    // Show tray icon
    notifyIcon1.Visible = true;
    notifyIcon1.Text = "Application Running";
    
    // Form will be minimized to taskbar after splash
}
```

## Coordination Patterns

### Pattern 1: Splash During Form Load

Hide form while showing splash, then display form fully initialized:

```csharp
public class FormLoadPattern : Form
{
    private SplashControl splashControl1;
    
    public FormLoadPattern()
    {
        InitializeComponent();
        
        splashControl1.HostForm = this;
        splashControl1.HideHostForm = true;
        splashControl1.SplashImage = Properties.Resources.Logo;
        splashControl1.TimerInterval = 5000;
        splashControl1.AutoMode = true;
    }
    
    private void FormLoadPattern_Load(object sender, EventArgs e)
    {
        // Splash is showing, form is hidden
        // Perform initialization
        LoadConfiguration();
        ConnectToDatabase();
        LoadUserPreferences();
        
        // Form will become visible when splash closes
    }
}
```

### Pattern 2: Manual Control with Initialization Tasks

Explicitly control when form appears:

```csharp
public class ManualControlPattern : Form
{
    private SplashControl splashControl1;
    
    public ManualControlPattern()
    {
        InitializeComponent();
        
        splashControl1.HostForm = this;
        splashControl1.HideHostForm = true;
        splashControl1.HostFormWindowState = FormWindowState.Normal;
        splashControl1.AutoMode = false; // Manual control
    }
    
    private async void ManualControlPattern_Load(object sender, EventArgs e)
    {
        // Show splash manually
        splashControl1.ShowSplash(true);
        
        try
        {
            // Perform async initialization
            await InitializeApplicationAsync();
        }
        finally
        {
            // Hide splash when done
            splashControl1.HideSplash();
            
            // Form becomes visible now
            this.Show();
            this.Focus();
        }
    }
    
    private async Task InitializeApplicationAsync()
    {
        await Task.Delay(3000); // Simulate initialization
    }
}
```

### Pattern 3: Progress-Based Visibility Control

Show form only after specific progress threshold:

```csharp
public class ProgressBasedPattern : Form
{
    private SplashControl splashControl1;
    private int initializationProgress = 0;
    
    public ProgressBasedPattern()
    {
        InitializeComponent();
        
        splashControl1.HostForm = this;
        splashControl1.HideHostForm = true;
        splashControl1.TimerInterval = 30000; // Long backup timer
        splashControl1.AutoMode = false;
    }
    
    private async void ProgressBasedPattern_Load(object sender, EventArgs e)
    {
        splashControl1.ShowSplash(true);
        
        var progress = new Progress<int>(percent =>
        {
            initializationProgress = percent;
            
            if (percent >= 100)
            {
                splashControl1.HideSplash();
                this.Show();
                this.Focus();
            }
        });
        
        await PerformInitializationWithProgress(progress);
    }
    
    private async Task PerformInitializationWithProgress(IProgress<int> progress)
    {
        for (int i = 0; i <= 100; i += 20)
        {
            await Task.Delay(500);
            progress.Report(i);
        }
    }
}
```

## Complete Examples

### Example 1: Standard Business Application

```csharp
public class BusinessAppStartup : Form
{
    private SplashControl splashControl1;
    
    public BusinessAppStartup()
    {
        InitializeComponent();
        SetupSplash();
    }
    
    private void SetupSplash()
    {
        // Configure host form settings
        splashControl1.HostForm = this;
        splashControl1.HideHostForm = true;
        splashControl1.HostFormWindowState = FormWindowState.Normal;
        
        // Configure splash appearance
        splashControl1.SplashImage = Properties.Resources.CompanyBranding;
        splashControl1.DesktopAlignment = SplashAlignment.Center;
        splashControl1.ShowAnimation = true;
        
        // Configure timing
        splashControl1.TimerInterval = 4000;
        splashControl1.AutoMode = true;
        
        // Handle splash closed to complete setup
        splashControl1.SplashClosed += OnSplashClosed;
    }
    
    private void OnSplashClosed(object sender, EventArgs e)
    {
        // Form is now visible, complete final setup
        this.Activate();
        ShowWelcomeMessage();
    }
    
    private void ShowWelcomeMessage()
    {
        // Welcome user after splash closes
        statusStrip1.Text = $"Welcome, {Environment.UserName}!";
    }
}
```

### Example 2: Data-Heavy Application

```csharp
public class DataHeavyApplication : Form
{
    private SplashControl splashControl1;
    
    public DataHeavyApplication()
    {
        InitializeComponent();
        ConfigureSplashForDataLoading();
    }
    
    private void ConfigureSplashForDataLoading()
    {
        splashControl1.HostForm = this;
        splashControl1.HideHostForm = true;
        splashControl1.HostFormWindowState = FormWindowState.Maximized;
        splashControl1.SplashImage = Properties.Resources.LoadingScreen;
        splashControl1.TimerInterval = 15000; // Long backup
        splashControl1.AutoMode = false;
    }
    
    private async void DataHeavyApplication_Load(object sender, EventArgs e)
    {
        splashControl1.ShowSplash(true);
        
        try
        {
            await LoadLargeDataset();
            await InitializeCharts();
            await PopulateGrids();
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Initialization failed: {ex.Message}");
        }
        finally
        {
            splashControl1.HideSplash();
            // Form appears maximized with all data loaded
        }
    }
    
    private async Task LoadLargeDataset()
    {
        await Task.Delay(2000); // Simulate data loading
    }
    
    private async Task InitializeCharts()
    {
        await Task.Delay(1500); // Simulate chart setup
    }
    
    private async Task PopulateGrids()
    {
        await Task.Delay(1000); // Simulate grid population
    }
}
```

### Example 3: Minimized Background Service

```csharp
public class BackgroundServiceApp : Form
{
    private SplashControl splashControl1;
    private NotifyIcon trayIcon;
    
    public BackgroundServiceApp()
    {
        InitializeComponent();
        SetupBackgroundService();
    }
    
    private void SetupBackgroundService()
    {
        // Configure splash
        splashControl1.HostForm = this;
        splashControl1.HideHostForm = true;
        splashControl1.HostFormWindowState = FormWindowState.Minimized;
        splashControl1.SplashImage = Properties.Resources.ServiceLogo;
        splashControl1.TimerInterval = 2000;
        splashControl1.AutoMode = true;
        
        // Setup tray icon
        trayIcon = new NotifyIcon();
        trayIcon.Icon = this.Icon;
        trayIcon.Text = "Background Service";
        trayIcon.Visible = true;
        trayIcon.DoubleClick += (s, e) => this.WindowState = FormWindowState.Normal;
        
        // Hide from taskbar when minimized
        this.ShowInTaskbar = false;
    }
}
```

## Best Practices

### Host Form Configuration

1. **Always set HostForm:** Required for AutoMode and proper coordination
2. **Set HostForm in constructor:** Before any splash display
3. **Use 'this' in form constructor:** `splashControl1.HostForm = this;`
4. **Don't change HostForm at runtime:** Set once during initialization

### HideHostForm Usage

1. **Use for professional startup:** Creates polished user experience
2. **Combine with initialization:** Hide form while loading data
3. **Consider user expectations:** Business apps typically hide, utilities may not
4. **Test with complex forms:** Ensure form state is correct when revealed

### HostFormWindowState Guidelines

1. **Match application purpose:**
   - Normal: Standard applications
   - Maximized: Full-screen applications, dashboards
   - Minimized: Background services, tray applications

2. **Remember to set HideHostForm = true:** Required for HostFormWindowState to work

3. **Provide user control:** Consider saving and restoring user's preferred window state

### Coordination Best Practices

1. **Use events for timing:** SplashClosed event for post-splash actions
2. **Handle errors gracefully:** Always hide splash in finally block
3. **Manage focus explicitly:** Call Focus() or Activate() after splash closes
4. **Test state transitions:** Verify smooth transition from splash to form

### Common Pitfalls to Avoid

1. **Forgetting HideHostForm = true:** HostFormWindowState won't work without it
2. **Not handling exceptions:** Splash may stay visible if initialization fails
3. **Incorrect timing:** Form appears before fully initialized
4. **Focus issues:** Form doesn't gain focus after splash closes
