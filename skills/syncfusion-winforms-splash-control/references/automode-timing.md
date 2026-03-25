# AutoMode and Timing Configuration

## Table of Contents
- [Overview](#overview)
- [AutoMode Properties](#automode-properties)
- [Automatic Launching](#automatic-launching)
- [Manual Display Methods](#manual-display-methods)
- [Timer Interval Settings](#timer-interval-settings)
- [Display State Management](#display-state-management)
- [Common Scenarios](#common-scenarios)
- [Best Practices](#best-practices)

## Overview

The SplashControl provides flexible display control through automatic and manual modes. You can configure when the splash screen appears, how long it displays, and whether it blocks user interaction with the application.

## AutoMode Properties

### Core Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| **AutoMode** | bool | True | Automatically launch splash screen on form load |
| **AutoModeDisableOwner** | bool | False | Display splash modally in AutoMode |
| **TimerInterval** | int | 5000 | Display duration in milliseconds |
| **IsShowing** | bool | False | Indicates if splash is currently visible (read-only) |

## Automatic Launching

### AutoMode Property

The **AutoMode** property determines whether the splash screen displays automatically when the parent form loads.

**When AutoMode = True:**
- Splash screen displays during the parent form's `Load` event
- No manual code required to show the splash
- Ideal for application startup scenarios

**When AutoMode = False:**
- Splash screen must be displayed manually via `ShowSplash()` or `ShowDialogSplash()`
- Provides complete control over display timing
- Useful for on-demand splash screens

**C# Example:**

```csharp
public Form1()
{
    InitializeComponent();
    
    // Enable automatic display
    splashControl1.AutoMode = true;
    splashControl1.HostForm = this;
    splashControl1.SplashImage = Properties.Resources.Logo;
    splashControl1.TimerInterval = 3000;
}
```

**VB.NET Example:**

```vb
Public Sub New()
    InitializeComponent()
    
    ' Enable automatic display
    splashControl1.AutoMode = True
    splashControl1.HostForm = Me
    splashControl1.SplashImage = My.Resources.Logo
    splashControl1.TimerInterval = 3000
End Sub
```

### AutoModeDisableOwner Property

The **AutoModeDisableOwner** property controls whether the splash screen displays modally when AutoMode is enabled.

**When AutoModeDisableOwner = True:**
- Splash screen displays modally
- User cannot interact with the host form until splash closes
- Host form is disabled during splash display

**When AutoModeDisableOwner = False:**
- Splash screen displays non-modally
- User can interact with other parts of the application
- Splash and host form are both active

**C# Example:**

```csharp
private void ConfigureModalAutoSplash()
{
    splashControl1.AutoMode = true;
    splashControl1.AutoModeDisableOwner = true; // Modal display
    splashControl1.TimerInterval = 4000;
}
```

**VB.NET Example:**

```vb
Private Sub ConfigureModalAutoSplash()
    splashControl1.AutoMode = True
    splashControl1.AutoModeDisableOwner = True ' Modal display
    splashControl1.TimerInterval = 4000
End Sub
```

## Manual Display Methods

When AutoMode is False or when you need explicit control, use these methods to display and hide the splash screen.

### ShowSplash Method

Displays the splash screen manually with optional owner disabling.

**Signature:**
```csharp
public void ShowSplash(bool disableOwner)
```

**Parameters:**
- **disableOwner:** If `true`, disables the owner form while splash is displayed (modal behavior)

**C# Example:**

```csharp
// Non-modal display
private void ShowNonModalSplash()
{
    splashControl1.ShowSplash(false);
}

// Modal display - disables owner form
private void ShowModalSplash()
{
    splashControl1.ShowSplash(true);
}

// Button click example
private void btnShowSplash_Click(object sender, EventArgs e)
{
    splashControl1.ShowSplash(true);
}
```

**VB.NET Example:**

```vb
' Non-modal display
Private Sub ShowNonModalSplash()
    splashControl1.ShowSplash(False)
End Sub

' Modal display - disables owner form
Private Sub ShowModalSplash()
    splashControl1.ShowSplash(True)
End Sub

' Button click example
Private Sub btnShowSplash_Click(sender As Object, e As EventArgs)
    splashControl1.ShowSplash(True)
End Sub
```

### HideSplash Method

Closes the splash screen programmatically before the timer expires.

**Signature:**
```csharp
public void HideSplash()
```

**C# Example:**

```csharp
// Hide splash immediately
private void btnHideSplash_Click(object sender, EventArgs e)
{
    if (splashControl1.IsShowing)
    {
        splashControl1.HideSplash();
    }
}

// Hide after background task completes
private async void Form1_Load(object sender, EventArgs e)
{
    splashControl1.ShowSplash(true);
    
    await LoadApplicationDataAsync();
    
    splashControl1.HideSplash();
}
```

**VB.NET Example:**

```vb
' Hide splash immediately
Private Sub btnHideSplash_Click(sender As Object, e As EventArgs)
    If splashControl1.IsShowing Then
        splashControl1.HideSplash()
    End If
End Sub

' Hide after background task completes
Private Async Sub Form1_Load(sender As Object, e As EventArgs)
    splashControl1.ShowSplash(True)
    
    Await LoadApplicationDataAsync()
    
    splashControl1.HideSplash()
End Sub
```

### ShowDialogSplash Method

Displays the splash screen as a modal dialog with explicit owner and optional positioning.

**Overloads:**

```csharp
// Show at default location
public void ShowDialogSplash(Form ownerForm)

// Show at specific location
public void ShowDialogSplash(Point location, Form ownerForm)
```

**Parameters:**
- **ownerForm:** The owner form for the modal dialog
- **location:** Specific screen coordinates for splash display

**C# Example:**

```csharp
// Show as modal dialog at default position
private void btnShowDialog_Click(object sender, EventArgs e)
{
    splashControl1.ShowDialogSplash(this);
}

// Show at specific screen location
private void btnShowDialogCustom_Click(object sender, EventArgs e)
{
    Point customLocation = new Point(700, 700);
    splashControl1.ShowDialogSplash(customLocation, this);
}

// Show centered on secondary screen
private void ShowSplashOnSecondaryScreen()
{
    if (Screen.AllScreens.Length > 1)
    {
        Screen secondScreen = Screen.AllScreens[1];
        Point center = new Point(
            secondScreen.Bounds.X + (secondScreen.Bounds.Width / 2) - 100,
            secondScreen.Bounds.Y + (secondScreen.Bounds.Height / 2) - 100
        );
        splashControl1.ShowDialogSplash(center, this);
    }
}
```

**VB.NET Example:**

```vb
' Show as modal dialog at default position
Private Sub btnShowDialog_Click(sender As Object, e As EventArgs)
    splashControl1.ShowDialogSplash(Me)
End Sub

' Show at specific screen location
Private Sub btnShowDialogCustom_Click(sender As Object, e As EventArgs)
    Dim customLocation As New Point(700, 700)
    splashControl1.ShowDialogSplash(customLocation, Me)
End Sub
```

## Timer Interval Settings

### TimerInterval Property

The **TimerInterval** property determines how long the splash screen displays before automatically closing.

**Key characteristics:**
- Value is in **milliseconds** (1000 ms = 1 second)
- Default value: **5000** (5 seconds)
- Applies to both AutoMode and manual display
- Timer starts when splash becomes visible

**C# Example:**

```csharp
// Display for 3 seconds
splashControl1.TimerInterval = 3000;

// Display for 8 seconds
splashControl1.TimerInterval = 8000;

// Display for 1 second (quick flash)
splashControl1.TimerInterval = 1000;

// Very long display (15 seconds)
splashControl1.TimerInterval = 15000;
```

**VB.NET Example:**

```vb
' Display for 3 seconds
splashControl1.TimerInterval = 3000

' Display for 8 seconds
splashControl1.TimerInterval = 8000

' Display for 1 second (quick flash)
splashControl1.TimerInterval = 1000
```

### Choosing the Right Interval

**Recommended durations:**

| Scenario | Duration | Notes |
|----------|----------|-------|
| Quick branding | 1500-2000 ms | Brief logo display |
| Standard startup | 3000-5000 ms | Most common scenario |
| Initialization process | 5000-8000 ms | Loading data or resources |
| Manual close | 30000+ ms | Use with `HideSplash()` when ready |

**Example with different scenarios:**

```csharp
public class SplashTimingExamples
{
    // Quick branding splash
    private void ConfigureQuickSplash()
    {
        splashControl1.TimerInterval = 2000; // 2 seconds
        splashControl1.ShowAnimation = true;
    }
    
    // Standard application startup
    private void ConfigureStandardSplash()
    {
        splashControl1.TimerInterval = 4000; // 4 seconds
    }
    
    // Long initialization with manual close
    private void ConfigureLongSplash()
    {
        splashControl1.TimerInterval = 30000; // 30 seconds backup
        splashControl1.ShowSplash(true);
        
        // Close when initialization completes
        Task.Run(async () =>
        {
            await PerformLongInitialization();
            this.Invoke(new Action(() => splashControl1.HideSplash()));
        });
    }
}
```

## Display State Management

### IsShowing Property

The **IsShowing** property (read-only) indicates whether the splash screen is currently visible.

**C# Example:**

```csharp
private void CheckSplashState()
{
    if (splashControl1.IsShowing)
    {
        Console.WriteLine("Splash is currently displayed");
        // Can hide it if needed
        splashControl1.HideSplash();
    }
    else
    {
        Console.WriteLine("Splash is not showing");
        // Can show it if needed
        splashControl1.ShowSplash(false);
    }
}

// Prevent multiple displays
private void btnToggleSplash_Click(object sender, EventArgs e)
{
    if (!splashControl1.IsShowing)
    {
        splashControl1.ShowSplash(false);
    }
}
```

**VB.NET Example:**

```vb
Private Sub CheckSplashState()
    If splashControl1.IsShowing Then
        Console.WriteLine("Splash is currently displayed")
        ' Can hide it if needed
        splashControl1.HideSplash()
    Else
        Console.WriteLine("Splash is not showing")
        ' Can show it if needed
        splashControl1.ShowSplash(False)
    End If
End Sub
```

## Common Scenarios

### Scenario 1: Auto-Display with Modal Behavior

```csharp
private void ConfigureAutoModalSplash()
{
    splashControl1.HostForm = this;
    splashControl1.SplashImage = Properties.Resources.Logo;
    splashControl1.AutoMode = true;
    splashControl1.AutoModeDisableOwner = true;
    splashControl1.TimerInterval = 5000;
}
```

### Scenario 2: Manual Display with Task Completion

```csharp
private async void Form1_Load(object sender, EventArgs e)
{
    splashControl1.AutoMode = false;
    splashControl1.ShowSplash(true);
    
    try
    {
        await LoadConfigurationAsync();
        await ConnectToDatabaseAsync();
        await LoadUserPreferencesAsync();
    }
    finally
    {
        splashControl1.HideSplash();
    }
}
```

### Scenario 3: Conditional Display Based on Startup Time

```csharp
private async void Form1_Load(object sender, EventArgs e)
{
    var stopwatch = Stopwatch.StartNew();
    
    await PerformInitialization();
    
    stopwatch.Stop();
    
    // Only show splash if initialization takes > 2 seconds
    if (stopwatch.ElapsedMilliseconds > 2000)
    {
        splashControl1.TimerInterval = 2000;
        splashControl1.ShowSplash(false);
    }
}
```

### Scenario 4: Splash with Progress-Based Close

```csharp
private void ShowProgressBasedSplash()
{
    splashControl1.TimerInterval = 60000; // Long backup timer
    splashControl1.ShowSplash(true);
    
    var progress = new Progress<int>(percent =>
    {
        if (percent >= 100)
        {
            splashControl1.HideSplash();
        }
    });
    
    Task.Run(() => LoadWithProgress(progress));
}
```

## Best Practices

### Timing Best Practices

1. **Keep it short:** 2-5 seconds is ideal for most applications
2. **Use HideSplash for long operations:** Don't rely solely on TimerInterval for initialization tasks
3. **Test on slower machines:** Ensure timing works well on various hardware
4. **Consider user patience:** Longer than 8 seconds feels too long without progress indication

### AutoMode Best Practices

1. **Use AutoMode for simple scenarios:** When you just need a startup splash
2. **Disable AutoMode for complex initialization:** When you need precise control
3. **Combine with HideHostForm:** Create a professional startup experience
4. **Use AutoModeDisableOwner cautiously:** Only when you need to force user attention

### Manual Control Best Practices

1. **Always check IsShowing:** Prevent duplicate splash displays
2. **Use try-finally with HideSplash:** Ensure splash closes even on errors
3. **Provide fallback timer:** Even with manual close, set a reasonable TimerInterval
4. **Handle threading carefully:** Use `Invoke` when closing from background threads

### Example: Comprehensive Timing Setup

```csharp
public class ComprehensiveTimingExample : Form
{
    private SplashControl splashControl1;
    
    public ComprehensiveTimingExample()
    {
        InitializeComponent();
        ConfigureSplash();
    }
    
    private void ConfigureSplash()
    {
        splashControl1.HostForm = this;
        splashControl1.SplashImage = Properties.Resources.Logo;
        splashControl1.AutoMode = false; // Manual control
        splashControl1.TimerInterval = 10000; // 10-second fallback
        splashControl1.DesktopAlignment = SplashAlignment.Center;
    }
    
    private async void ComprehensiveTimingExample_Load(object sender, EventArgs e)
    {
        splashControl1.ShowSplash(true);
        
        try
        {
            await InitializeApplication();
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Initialization error: {ex.Message}");
        }
        finally
        {
            // Always hide splash, even on error
            if (splashControl1.IsShowing)
            {
                splashControl1.HideSplash();
            }
        }
    }
    
    private async Task InitializeApplication()
    {
        await Task.Delay(2000); // Simulate initialization
    }
}
```
