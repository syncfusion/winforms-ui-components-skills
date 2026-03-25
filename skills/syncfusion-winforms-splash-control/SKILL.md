---
name: syncfusion-winforms-splash-control
description: Implement Syncfusion WinForms SplashControl to display splash screens during application startup. Use this when building startup screens, loading screens, timed splash displays, or animated splash images in WinForms. Covers SplashControl setup, image configuration, timing, animation, and positioning.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WinForms SplashControl

The SplashControl component enables easy creation of professional splash screens for Windows Forms applications. Simply drag and drop the control onto your form, set an image, and the splash screen displays automatically at runtime with customizable timing, animation, and positioning.

## When to Use This Skill

Use this skill when you need to:
- Display a splash screen during application startup
- Show branding, logos, or loading information while the app initializes
- Create timed or manually-controlled splash displays
- Implement animated splash screens with visual effects
- Design custom splash panels with controls and content
- Control host form visibility during splash display
- Handle splash screen lifecycle events
- Position splash screens at specific desktop locations

## Component Overview

**SplashControl** provides two display modes:
1. **Image-based splash screen:** Display a static or animated image
2. **Custom SplashPanel:** Design a fully customizable panel with any controls

**Key capabilities:**
- Automatic or manual display control
- Configurable display duration with timer intervals
- Built-in animation effects (left-to-right reveal)
- Flexible desktop positioning (center, corners, system tray, custom)
- Host form visibility management
- Modal or non-modal display modes
- Transparency support
- Complete event lifecycle

## Documentation and Navigation Guide

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)

Start here for initial setup:
- Assembly references and NuGet packages
- Adding SplashControl through designer or code
- Basic property configuration
- AutoMode vs manual invocation
- Running your first splash screen

### Display and Timing Configuration

📄 **Read:** [references/automode-timing.md](references/automode-timing.md)

Configure when and how the splash screen appears:
- AutoMode for automatic display on form load
- AutoModeDisableOwner for modal splash screens
- ShowSplash() method for manual display control
- HideSplash() to close programmatically
- ShowDialogSplash() for modal dialog display
- TimerInterval to control display duration
- IsShowing property to check display state

### Image and Animation Settings

📄 **Read:** [references/image-animation.md](references/image-animation.md)

Customize the visual appearance:
- Setting the SplashImage property
- Enabling animation with ShowAnimation
- Configuring transparent colors
- ShowAsTopMost to control window layering
- Animation behavior and effects

### Alignment and Positioning

📄 **Read:** [references/alignment-positioning.md](references/alignment-positioning.md)

Control splash screen location on the desktop:
- DesktopAlignment options (Center, SystemTray, corners)
- Custom positioning strategies
- Visual examples for each alignment mode

### Host Form Management

📄 **Read:** [references/host-form-settings.md](references/host-form-settings.md)

Manage the parent form during splash display:
- HostForm property configuration
- HideHostForm to conceal the main form
- HostFormWindowState for controlling form state (Normal, Minimized, Maximized)
- Coordination between splash and host form

### Custom SplashPanel Integration

📄 **Read:** [references/splashpanel-integration.md](references/splashpanel-integration.md)

Create fully customized splash panels:
- Using SplashPanel instead of static images
- CustomSplashPanel property
- UseCustomSplashPanel to enable custom panels
- Designing panels with controls (labels, progress bars, etc.)
- SplashControlPanel property
- ShowInTaskbar and FormIcon configuration
- Complete integration workflow with code examples

### Event Handling

📄 **Read:** [references/events.md](references/events.md)

Handle splash screen lifecycle events:
- BeforeSplash event (with cancellation support)
- SplashDisplayed event
- SplashClosing event
- SplashClosed event
- Event notification methods
- Logging and debugging patterns

## Quick Start

### Basic Image-Based Splash Screen

```csharp
using Syncfusion.Windows.Forms.Tools;

public partial class Form1 : Form
{
    private SplashControl splashControl1;

    public Form1()
    {
        InitializeComponent();
        
        // Initialize SplashControl
        this.splashControl1 = new SplashControl();
        
        // Set the splash image
        this.splashControl1.SplashImage = Image.FromFile("splash.png");
        
        // Configure basic properties
        this.splashControl1.HostForm = this;
        this.splashControl1.TimerInterval = 3000; // Display for 3 seconds
        this.splashControl1.AutoMode = true; // Auto-display on form load
        this.splashControl1.DesktopAlignment = SplashAlignment.Center;
        
        // Optional: Enable animation
        this.splashControl1.ShowAnimation = true;
    }
}
```

### Manual Display Control

```csharp
// Display splash screen manually
private void ShowSplashButton_Click(object sender, EventArgs e)
{
    // Show splash and disable owner form
    splashControl1.ShowSplash(true);
}

// Hide splash screen programmatically
private void HideSplashButton_Click(object sender, EventArgs e)
{
    splashControl1.HideSplash();
}

// Show as modal dialog
private void ShowModalSplash_Click(object sender, EventArgs e)
{
    splashControl1.ShowDialogSplash(this);
}
```

## Common Patterns

### Automatic Splash with Hidden Host Form

Display splash screen while hiding the main form during initialization:

```csharp
private void InitializeSplash()
{
    splashControl1.HostForm = this;
    splashControl1.SplashImage = Properties.Resources.CompanyLogo;
    splashControl1.AutoMode = true;
    splashControl1.HideHostForm = true;
    splashControl1.TimerInterval = 4000;
    splashControl1.DesktopAlignment = SplashAlignment.Center;
    
    // Handle splash closed event to perform initialization
    splashControl1.SplashClosed += (s, e) => 
    {
        // Perform app initialization after splash closes
        LoadApplicationData();
    };
}
```

### Custom SplashPanel with Progress

Create a custom splash panel with controls:

```csharp
private void SetupCustomSplashPanel()
{
    // Create and design SplashPanel
    SplashPanel splashPanel = new SplashPanel();
    splashPanel.BackgroundColor = new BrushInfo(GradientStyle.Vertical, 
        Color.White, Color.LightBlue);
    splashPanel.Size = new Size(400, 200);
    
    // Add label
    Label statusLabel = new Label
    {
        Text = "Loading Application...",
        AutoSize = true,
        Location = new Point(50, 80),
        Font = new Font("Segoe UI", 14, FontStyle.Bold)
    };
    splashPanel.Controls.Add(statusLabel);
    
    // Configure SplashControl to use custom panel
    splashControl1.CustomSplashPanel = splashPanel;
    splashControl1.UseCustomSplashPanel = true;
    splashControl1.HostForm = this;
    splashControl1.TimerInterval = 5000;
}
```

### Event-Driven Splash Display

Handle events for logging and control:

```csharp
private void ConfigureSplashEvents()
{
    splashControl1.BeforeSplash += (s, e) =>
    {
        // Log before display
        Debug.WriteLine("Splash screen about to display");
        
        // Cancel if needed
        // e.Cancel = true;
    };
    
    splashControl1.SplashDisplayed += (s, e) =>
    {
        Debug.WriteLine("Splash screen is now visible");
        StartBackgroundInitialization();
    };
    
    splashControl1.SplashClosing += (s, e) =>
    {
        Debug.WriteLine("Splash screen is closing");
    };
    
    splashControl1.SplashClosed += (s, e) =>
    {
        Debug.WriteLine("Splash screen closed");
        ShowMainForm();
    };
}
```

### Transparent Splash Screen

Create a splash screen with transparent regions:

```csharp
private void ConfigureTransparentSplash()
{
    splashControl1.SplashImage = Properties.Resources.LogoWithTransparency;
    splashControl1.TransparentColor = Color.White; // Make white pixels transparent
    splashControl1.ShowAnimation = true;
    splashControl1.ShowAsTopMost = true;
    splashControl1.DesktopAlignment = SplashAlignment.Center;
    splashControl1.TimerInterval = 3000;
}
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| **AutoMode** | bool | Automatically display splash on form load |
| **SplashImage** | Image | Image to display as splash screen |
| **TimerInterval** | int | Display duration in milliseconds (default: 5000) |
| **DesktopAlignment** | SplashAlignment | Position on desktop (Center, SystemTray, corners, Custom) |
| **HostForm** | Form | Parent form of the SplashControl |
| **HideHostForm** | bool | Hide parent form during splash display |
| **ShowAnimation** | bool | Enable left-to-right animation effect |
| **CustomSplashPanel** | SplashPanel | Custom panel to display instead of image |
| **UseCustomSplashPanel** | bool | Use CustomSplashPanel instead of SplashImage |
| **AutoModeDisableOwner** | bool | Display splash modally in AutoMode |
| **ShowAsTopMost** | bool | Display splash as topmost window |
| **TransparentColor** | Color | Color to make transparent in splash image |
| **IsShowing** | bool | Indicates if splash is currently displayed (read-only) |

## Key Methods

| Method | Description |
|--------|-------------|
| **ShowSplash(bool disableOwner)** | Display splash screen manually |
| **HideSplash()** | Close the splash screen programmatically |
| **ShowDialogSplash(Form owner)** | Display splash as modal dialog |
| **ShowDialogSplash(Point location, Form owner)** | Display splash at specific location as modal |

## Key Events

| Event | Description |
|-------|-------------|
| **BeforeSplash** | Raised before splash display (cancelable) |
| **SplashDisplayed** | Raised after splash is shown |
| **SplashClosing** | Raised before splash closes (cancelable) |
| **SplashClosed** | Raised after splash is closed |

## Common Use Cases

### Application Startup Branding
Display company logo and version information during application load, hiding the main form until initialization completes.

### Long Initialization Process
Show a custom splash panel with progress indicator while loading data, initializing components, or connecting to services.

### Version Update Notification
Display splash screen with "What's New" information when the application starts after an update.

### Professional User Experience
Create animated, transparent splash screens that match your application's branding and design language.

### Timed Information Display
Show important announcements, tips, or messages for a specific duration before allowing user interaction.

## Related Components

- **SplashPanel:** Standalone splash panel that can be used independently or integrated with SplashControl
- **SplashScreen:** Alternative splash screen implementation approaches in WinForms

## Tips and Best Practices

1. **Keep splash duration reasonable:** 2-5 seconds is ideal; longer durations frustrate users
2. **Match application theme:** Ensure splash screen design aligns with your application's visual identity
3. **Use AutoMode for simplicity:** Unless you need complex control, AutoMode handles most scenarios
4. **Handle SplashClosed event:** Perform initialization tasks when splash closes to ensure smooth transition
5. **Consider SplashPanel for dynamic content:** Use custom panels when you need progress bars or changing text
6. **Test different alignments:** Center usually works best, but SystemTray can be less intrusive
7. **Use HideHostForm judiciously:** Hiding the host form can create a cleaner startup experience
8. **Enable animation selectively:** Animation adds polish but may not suit all application types

## Assembly Dependencies

**Required assemblies:**
- `Syncfusion.Shared.Base.dll`
- `Syncfusion.Tools.Windows.dll`

**NuGet Package:**
```
Install-Package Syncfusion.Tools.Windows
```

Refer to [Syncfusion control dependencies documentation](https://help.syncfusion.com/windowsforms/control-dependencies#splashcontrol) for complete dependency information.
