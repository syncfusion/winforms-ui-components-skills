# Display Methods and Control

This guide covers methods for showing, hiding, and controlling the SplashPanel display.

## Table of Contents
- [ShowSplash Method](#showsplash-method)
- [HideSplash Method](#hidesplash-method)
- [ShowDialogSplash Method](#showdialogsplash-method)
- [IsShowing Method](#isshowing-method)
- [Timer Interval](#timer-interval)
- [Location Settings](#location-settings)
- [Taskbar Display](#taskbar-display)

## ShowSplash Method

The `ShowSplash()` method displays the SplashPanel at runtime.

### Basic Usage

```csharp
// Simple display
this.splashPanel1.ShowSplash();
```

**VB.NET:**
```vb
Me.splashPanel1.ShowSplash()
```

The splash will be displayed at the position specified by the `DesktopAlignment` property.

### ShowSplash with Parameters

**Method Signature:**
```csharp
public void ShowSplash(Point location, Form ownerForm, bool disableOwner)
```

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `location` | Point | Point in screen coordinates (use `Point.Empty` for default) |
| `ownerForm` | Form | Form that will embed the splash form |
| `disableOwner` | bool | Whether to disable the owner form while splash is displayed |

**Example:**

```csharp
// Show at specific location
Point splashLocation = new Point(100, 100);
Form2 ownerForm = new Form2();
this.splashPanel1.ShowSplash(splashLocation, ownerForm, true);
```

**VB.NET:**
```vb
' Show at specific location
Dim splashLocation As New Point(100, 100)
Dim ownerForm As New Form2()
Me.splashPanel1.ShowSplash(splashLocation, ownerForm, True)
```

**Notes:**
- The `location` parameter is only effective when `DesktopAlignment` is set to `SplashAlignment.Custom`
- When `disableOwner` is `true`, the owner form will be disabled until the splash closes
- Use `Point.Empty` to use the default alignment

### Owner Form Behavior

```csharp
// Disable main form while splash is showing
this.splashPanel1.ShowSplash(Point.Empty, this, true);

// Keep main form enabled
this.splashPanel1.ShowSplash(Point.Empty, this, false);
```

## HideSplash Method

The `HideSplash()` method closes the splash panel immediately.

### Basic Usage

```csharp
// Hide the splash panel
this.splashPanel1.HideSplash();
```

**VB.NET:**
```vb
' Hide the splash panel
Me.splashPanel1.HideSplash()
```

### Use Cases

**Manual Control Pattern:**
```csharp
// Show splash
splashPanel1.TimerInterval = -1; // Disable auto-close
splashPanel1.ShowSplash();

// Perform lengthy operation
PerformDataLoad();

// Hide splash when done
splashPanel1.HideSplash();
```

**Button Click to Close:**
```csharp
private void closeButton_Click(object sender, EventArgs e)
{
    splashPanel1.HideSplash();
}
```

**Conditional Closing:**
```csharp
private void CheckAndCloseSplash()
{
    if (splashPanel1.IsShowing() && dataLoadComplete)
    {
        splashPanel1.HideSplash();
    }
}
```

## ShowDialogSplash Method

The `ShowDialogSplash()` method displays the SplashPanel as a modal dialog, blocking user interaction with the rest of the application.

### Basic Usage

```csharp
// Show as modal dialog
this.splashPanel1.ShowDialogSplash(this);
```

**VB.NET:**
```vb
' Show as modal dialog
Me.splashPanel1.ShowDialogSplash(Me)
```

**Behavior:**
- User cannot interact with other application windows until splash closes
- Application execution is blocked until the splash is closed
- Use for critical startup or loading sequences

### ShowDialogSplash with Location

**Method Signature:**
```csharp
public void ShowDialogSplash(Point location, Form ownerForm)
```

**Example:**
```csharp
// Show modal dialog at specific location
Point dialogLocation = new Point(700, 700);
this.splashPanel1.ShowDialogSplash(dialogLocation, this);
```

**VB.NET:**
```vb
' Show modal dialog at specific location
Dim dialogLocation As New Point(700, 700)
Me.splashPanel1.ShowDialogSplash(dialogLocation, Me)
```

### Modal vs Modeless

**Modeless (ShowSplash):**
```csharp
// User can interact with application
splashPanel1.ShowSplash();
// Code continues executing immediately
DoOtherWork();
```

**Modal (ShowDialogSplash):**
```csharp
// User cannot interact with application
splashPanel1.ShowDialogSplash(this);
// Code execution is blocked here until splash closes
// This line only executes after splash closes
DoWorkAfterSplash();
```

### Modal Dialog Pattern

```csharp
private void ShowModalLoadingDialog()
{
    SplashPanel loadingDialog = new SplashPanel();
    loadingDialog.Size = new Size(350, 200);
    loadingDialog.DesktopAlignment = SplashAlignment.Center;
    loadingDialog.TimerInterval = -1; // No auto-close
    
    Label loadingLabel = new Label();
    loadingLabel.Text = "Loading, please wait...";
    loadingLabel.Location = new Point(100, 90);
    loadingLabel.AutoSize = true;
    loadingDialog.Controls.Add(loadingLabel);
    
    this.Controls.Add(loadingDialog);
    
    // Show modal - blocks until closed
    loadingDialog.ShowDialogSplash(this);
}
```

## IsShowing Method

The `IsShowing()` method returns whether the splash panel is currently displayed.

### Usage

```csharp
bool isVisible = this.splashPanel1.IsShowing();
Console.WriteLine($"Splash visible: {isVisible}");
```

**VB.NET:**
```vb
Dim isVisible As Boolean = Me.splashPanel1.IsShowing()
Console.WriteLine($"Splash visible: {isVisible}")
```

### Practical Examples

**Check Before Closing:**
```csharp
private void CloseIfShowing()
{
    if (splashPanel1.IsShowing())
    {
        splashPanel1.HideSplash();
    }
}
```

**Status Indicator:**
```csharp
private void UpdateStatusLabel()
{
    statusLabel.Text = splashPanel1.IsShowing() 
        ? "Splash is visible" 
        : "Splash is hidden";
}
```

**Button State Management:**
```csharp
private void UpdateButtonStates()
{
    showButton.Enabled = !splashPanel1.IsShowing();
    hideButton.Enabled = splashPanel1.IsShowing();
}
```

## Timer Interval

The `TimerInterval` property controls how long the splash is displayed.

### Property

```csharp
public int TimerInterval { get; set; }
```

**Value:** Time in milliseconds. Use `-1` for no auto-close.

### Common Values

```csharp
// Show for 3 seconds
splashPanel1.TimerInterval = 3000;

// Show for 5 seconds
splashPanel1.TimerInterval = 5000;

// Show for 10 seconds
splashPanel1.TimerInterval = 10000;

// No auto-close (manual control only)
splashPanel1.TimerInterval = -1;
```

**VB.NET:**
```vb
' Show for 3 seconds
splashPanel1.TimerInterval = 3000

' No auto-close
splashPanel1.TimerInterval = -1
```

### Manual Control Pattern

```csharp
// Setup splash with no auto-close
splashPanel1.TimerInterval = -1;
splashPanel1.ShowSplash();

// Manually control lifetime
Task.Run(() => {
    DoLengthyOperation();
    
    // Close splash on UI thread
    this.Invoke(new Action(() => {
        splashPanel1.HideSplash();
    }));
});
```

### Suspend and Restore Auto-Close

**Suspend Auto-Close:**
```csharp
// Stop the timer temporarily
this.splashPanel1.SuspendAutoCloseMode();
```

**VB.NET:**
```vb
' Stop the timer temporarily
Me.splashPanel1.SuspendAutoCloseMode()
```

**Restore Auto-Close:**
```csharp
// Restart the timer
this.splashPanel1.RestoreAutoCloseMode();
```

**VB.NET:**
```vb
' Restart the timer
Me.splashPanel1.RestoreAutoCloseMode()
```

**Example:**
```csharp
private void button1_Click(object sender, EventArgs e)
{
    // Suspend auto-close when user interacts
    splashPanel1.SuspendAutoCloseMode();
}

private void button2_Click(object sender, EventArgs e)
{
    // Resume auto-close
    splashPanel1.RestoreAutoCloseMode();
}
```

## Location Settings

### DiscreetLocation Property

The `DiscreetLocation` property specifies the exact location to display the splash window.

```csharp
public Point DiscreetLocation { get; set; }
```

**Usage:**
```csharp
// Set specific screen coordinates
this.splashPanel1.DiscreetLocation = new Point(100, 100);
this.splashPanel1.DesktopAlignment = SplashAlignment.Custom;
this.splashPanel1.ShowSplash();
```

**VB.NET:**
```vb
' Set specific screen coordinates
Me.splashPanel1.DiscreetLocation = New Point(100, 100)
Me.splashPanel1.DesktopAlignment = SplashAlignment.Custom
Me.splashPanel1.ShowSplash()
```

**Note:** Only effective when `DesktopAlignment` is set to `SplashAlignment.Custom`.

### Dynamic Positioning

```csharp
// Position near mouse cursor
Point mousePos = Control.MousePosition;
splashPanel1.DesktopAlignment = SplashAlignment.Custom;
splashPanel1.ShowSplash(mousePos, this, false);
```

## Taskbar Display

The SplashPanel can be displayed in the Windows taskbar with custom icon and text.

### Properties

```csharp
public bool ShowInTaskbar { get; set; }
public Icon FormIcon { get; set; }
public string Text { get; set; }
```

### Configuration

```csharp
// Show splash in taskbar
this.splashPanel1.ShowInTaskbar = true;

// Set custom icon
this.splashPanel1.FormIcon = new Icon("app.ico");

// Set taskbar text
this.splashPanel1.Text = "Loading Application";
```

**VB.NET:**
```vb
' Show splash in taskbar
Me.splashPanel1.ShowInTaskbar = True

' Set custom icon
Me.splashPanel1.FormIcon = New Icon("app.ico")

' Set taskbar text
Me.splashPanel1.Text = "Loading Application"
```

### Complete Example

```csharp
private void SetupTaskbarSplash()
{
    SplashPanel splash = new SplashPanel();
    splash.Size = new Size(400, 250);
    splash.DesktopAlignment = SplashAlignment.Center;
    splash.TimerInterval = 5000;
    
    // Taskbar configuration
    splash.ShowInTaskbar = true;
    splash.Text = "My Application";
    splash.FormIcon = this.Icon; // Use form's icon
    
    this.Controls.Add(splash);
    splash.ShowSplash();
}
```

## Complete Display Control Example

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class DisplayControlForm : Form
{
    private SplashPanel splashPanel;
    private Button showButton;
    private Button hideButton;
    private Button showModalButton;
    private Label statusLabel;
    
    public DisplayControlForm()
    {
        InitializeControls();
    }
    
    private void InitializeControls()
    {
        // Setup splash
        splashPanel = new SplashPanel();
        splashPanel.Size = new Size(350, 200);
        splashPanel.DesktopAlignment = SplashAlignment.Center;
        splashPanel.TimerInterval = -1; // Manual control
        splashPanel.ShowAnimation = true;
        
        // Setup buttons
        showButton = new Button();
        showButton.Text = "Show Splash";
        showButton.Location = new Point(20, 20);
        showButton.Click += (s, e) => {
            splashPanel.ShowSplash();
            UpdateStatus();
        };
        
        hideButton = new Button();
        hideButton.Text = "Hide Splash";
        hideButton.Location = new Point(20, 60);
        hideButton.Click += (s, e) => {
            splashPanel.HideSplash();
            UpdateStatus();
        };
        
        showModalButton = new Button();
        showModalButton.Text = "Show Modal";
        showModalButton.Location = new Point(20, 100);
        showModalButton.Click += (s, e) => {
            splashPanel.TimerInterval = 3000; // Auto-close after 3s
            splashPanel.ShowDialogSplash(this);
            splashPanel.TimerInterval = -1; // Reset
            UpdateStatus();
        };
        
        statusLabel = new Label();
        statusLabel.Location = new Point(20, 150);
        statusLabel.AutoSize = true;
        
        // Add controls
        this.Controls.Add(splashPanel);
        this.Controls.Add(showButton);
        this.Controls.Add(hideButton);
        this.Controls.Add(showModalButton);
        this.Controls.Add(statusLabel);
        
        UpdateStatus();
    }
    
    private void UpdateStatus()
    {
        statusLabel.Text = splashPanel.IsShowing() 
            ? "Status: Splash is showing" 
            : "Status: Splash is hidden";
    }
}
```

## Next Steps

- **Animation:** See [animation-appearance.md](animation-appearance.md) for styling and animation
- **Transitions:** See [slide-transitions.md](slide-transitions.md) for slide effects
- **Events:** See [events.md](events.md) for handling display events
