---
name: syncfusion-winforms-splash-panel
description: Implement Syncfusion SplashPanel control for creating customizable splash screens and notification popups in Windows Forms applications. Use when displaying application startup screens, loading indicators, or non-obtrusive notification popups. Covers splash screen configuration, animations (slide, fade, marquee), desktop alignment, child controls, auto-close timers, background gradients/images, and splash events for creating professional startup experiences.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Windows Forms Splash Panel (SplashPanel)

The Syncfusion Windows Forms SplashPanel control enables creation of custom splash screens that display during application startup or as non-obtrusive notification windows. It supports animations, child controls, timed auto-close, and flexible positioning.

## When to Use This Skill

Use this skill when you need to:

- **Display splash screens** during application startup or loading
- **Show progress information** or branding during initialization
- **Create notification popups** similar to MSN Messenger or email notifications
- **Implement loading indicators** with custom content and animations
- **Show timed messages** that auto-close after a specified duration
- **Display modal dialogs** with custom splash content
- **Add animated transitions** (slide, fade, marquee) to splash windows
- **Position splash windows** at specific desktop locations (center, corners, system tray area, custom)
- **Include child controls** (buttons, labels, images) in splash screens
- **Handle splash lifecycle events** (before display, displayed, closing, closed)

## Component Overview

**Key Features:**
- **Animation styles:** Slide (horizontal/vertical/directional), Fade, Marquee
- **Desktop alignment:** Center, SystemTray, corners (LeftTop, LeftBottom, RightTop, RightBottom), Custom
- **Display methods:** ShowSplash (modeless), ShowDialogSplash (modal), HideSplash
- **Auto-close timer:** Configurable interval or manual control (-1 for no auto-close)
- **Child controls:** Add any Windows Forms controls to the splash panel
- **Appearance:** Gradient backgrounds, images, borders, transparency
- **Behavior:** AllowMove, AllowResize, CloseOnClick options
- **Events:** BeforeSplash, SplashDisplayed, SplashClosing, SplashClosed, mouse events

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

Read this reference when you need to:
- Install SplashPanel via NuGet or configure in designer
- Add SplashPanel to a Windows Forms project
- Create a SplashPanel instance programmatically
- Add child controls to the splash panel
- Call ShowSplash() method for basic display
- Understand assembly requirements and deployment

### Display Methods
📄 **Read:** [references/display-methods.md](references/display-methods.md)

Read this reference when you need to:
- Show splash panel with ShowSplash() method and parameters
- Hide splash panel with HideSplash() method
- Display as modal dialog with ShowDialogSplash()
- Check if splash is currently showing with IsShowing()
- Configure TimerInterval for auto-close duration
- Set splash location and owner form
- Display splash in taskbar with custom icon/text

### Animation and Appearance
📄 **Read:** [references/animation-appearance.md](references/animation-appearance.md)

Read this reference when you need to:
- Enable and configure animation speed
- Set SlideStyle (Horizontal, Vertical, FadeIn, etc.)
- Show splash as topmost window
- Customize background with gradients or images
- Configure border styles and transparency
- Set desktop alignment (Center, SystemTray, corners)
- Enable behavior options (AllowMove, AllowResize, CloseOnClick)
- Suspend auto-close when mouse is over splash

### Slide Transitions
📄 **Read:** [references/slide-transitions.md](references/slide-transitions.md)

Read this reference when you need to:
- Configure AnimationDirection (Default, LeftToRight, RightToLeft)
- Implement marquee transitions that traverse the screen
- Set MarqueeDirection (LeftToRight, RightToLeft, TopToBottom, BottomToTop)
- Combine slide transitions with desktop alignment
- Create advanced animation effects

### Events
📄 **Read:** [references/events.md](references/events.md)

Read this reference when you need to:
- Handle BeforeSplash event (cancel splash display)
- Respond to SplashDisplayed event
- Handle SplashClosing event (cancel closing)
- Respond to SplashClosed event (get close type)
- Handle mouse enter/leave events on splash
- Implement custom logic during splash lifecycle

## Quick Start

### Basic Splash Screen

```csharp
using Syncfusion.Windows.Forms.Tools;

// Create splash panel
SplashPanel splashPanel = new SplashPanel();
splashPanel.Size = new Size(400, 300);
splashPanel.DesktopAlignment = SplashAlignment.Center;
splashPanel.TimerInterval = 3000; // Show for 3 seconds
splashPanel.ShowAnimation = true;
splashPanel.SlideStyle = SlideStyle.FadeIn;

// Add to form
this.Controls.Add(splashPanel);

// Show splash
splashPanel.ShowSplash();
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms.Tools

' Create splash panel
Dim splashPanel As New SplashPanel()
splashPanel.Size = New Size(400, 300)
splashPanel.DesktopAlignment = SplashAlignment.Center
splashPanel.TimerInterval = 3000 ' Show for 3 seconds
splashPanel.ShowAnimation = True
splashPanel.SlideStyle = SlideStyle.FadeIn

' Add to form
Me.Controls.Add(splashPanel)

' Show splash
splashPanel.ShowSplash()
```

## Common Patterns

### Pattern 1: Startup Splash with Branding

```csharp
// Application startup splash
SplashPanel startupSplash = new SplashPanel();
startupSplash.Size = new Size(500, 350);
startupSplash.BackgroundImage = Image.FromFile("splash_image.png");
startupSplash.DesktopAlignment = SplashAlignment.Center;
startupSplash.TimerInterval = 5000;
startupSplash.ShowAnimation = true;
startupSplash.AnimationSpeed = 20;

// Add label with version info
Label versionLabel = new Label();
versionLabel.Text = "Version 2.0";
versionLabel.Location = new Point(20, 300);
versionLabel.AutoSize = true;
startupSplash.Controls.Add(versionLabel);

this.Controls.Add(startupSplash);
startupSplash.ShowSplash();
```

### Pattern 2: Notification-Style Popup

```csharp
// Non-obtrusive notification (like MSN Messenger)
SplashPanel notificationPanel = new SplashPanel();
notificationPanel.Size = new Size(300, 100);
notificationPanel.DesktopAlignment = SplashAlignment.RightBottom;
notificationPanel.SlideStyle = SlideStyle.BottomToTop;
notificationPanel.AnimationDirection = AnimationDirection.Default;
notificationPanel.TimerInterval = 5000;
notificationPanel.ShowAnimation = true;
notificationPanel.CloseOnClick = true;
notificationPanel.SuspendAutoCloseWhenMouseOver = true;

// Add notification content
Label messageLabel = new Label();
messageLabel.Text = "New message received!";
messageLabel.Location = new Point(10, 40);
notificationPanel.Controls.Add(messageLabel);

this.Controls.Add(notificationPanel);
notificationPanel.ShowSplash();
```

### Pattern 3: Modal Loading Dialog

```csharp
// Modal splash dialog that blocks interaction
SplashPanel modalSplash = new SplashPanel();
modalSplash.Size = new Size(400, 200);
modalSplash.TimerInterval = -1; // No auto-close
modalSplash.DesktopAlignment = SplashAlignment.Center;

// Add progress indicator
Label loadingLabel = new Label();
loadingLabel.Text = "Loading, please wait...";
loadingLabel.Location = new Point(120, 90);
loadingLabel.AutoSize = true;
modalSplash.Controls.Add(loadingLabel);

this.Controls.Add(modalSplash);

// Show as modal dialog (blocks until closed)
modalSplash.ShowDialogSplash(this);
```

### Pattern 4: Animated Splash with Gradient

```csharp
// Splash with gradient background and animation
SplashPanel animatedSplash = new SplashPanel();
animatedSplash.Size = new Size(450, 300);
animatedSplash.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical, 
    Color.DarkBlue, 
    Color.LightBlue);
animatedSplash.DesktopAlignment = SplashAlignment.Center;
animatedSplash.SlideStyle = SlideStyle.Horizontal;
animatedSplash.ShowAnimation = true;
animatedSplash.AnimationSpeed = 30;
animatedSplash.TimerInterval = 4000;
animatedSplash.ShowAsTopMost = true;

this.Controls.Add(animatedSplash);
animatedSplash.ShowSplash();
```

### Pattern 5: Custom Positioned Splash

```csharp
// Splash at custom screen location
SplashPanel customSplash = new SplashPanel();
customSplash.Size = new Size(350, 250);
customSplash.DesktopAlignment = SplashAlignment.Custom;
customSplash.TimerInterval = 3000;

// Show at mouse position
Point mousePos = Control.MousePosition;
customSplash.ShowSplash(mousePos, this, false);
```

### Pattern 6: Splash with Lifecycle Events

```csharp
// Splash with event handlers
SplashPanel eventSplash = new SplashPanel();
eventSplash.Size = new Size(400, 250);
eventSplash.DesktopAlignment = SplashAlignment.Center;
eventSplash.TimerInterval = 5000;

// Subscribe to events
eventSplash.BeforeSplash += (s, e) => {
    Console.WriteLine("About to show splash");
};

eventSplash.SplashDisplayed += (s, e) => {
    Console.WriteLine("Splash is now visible");
};

eventSplash.SplashClosing += (s, e) => {
    Console.WriteLine("Splash is closing");
    // e.Cancel = true; // Uncomment to prevent closing
};

eventSplash.SplashClosed += (s, e) => {
    Console.WriteLine($"Splash closed: {e.SplashCloseType}");
};

this.Controls.Add(eventSplash);
eventSplash.ShowSplash();
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `TimerInterval` | int | Duration in milliseconds to display splash (-1 for no auto-close) |
| `DesktopAlignment` | SplashAlignment | Position on desktop (Center, SystemTray, corners, Custom) |
| `SlideStyle` | SlideStyle | Animation style (Horizontal, Vertical, FadeIn, etc.) |
| `AnimationSpeed` | int | Speed of animation transition (higher = faster) |
| `ShowAnimation` | bool | Enable/disable animation on display |
| `ShowAsTopMost` | bool | Display splash as topmost window |
| `BackgroundColor` | BrushInfo | Gradient or solid background color |
| `BackgroundImage` | Image | Background image for splash |
| `AllowMove` | bool | Allow user to move splash at runtime |
| `AllowResize` | bool | Allow user to resize splash at runtime |
| `CloseOnClick` | bool | Close splash when user clicks on it |
| `SuspendAutoCloseWhenMouseOver` | bool | Suspend auto-close timer when mouse is over splash |
| `AnimationDirection` | AnimationDirection | Direction of slide animation |
| `MarqueeDirection` | MarqueeDirection | Direction of marquee transition |
| `BorderStyle` | Border3DStyle | 3D border style for splash |
| `TransparentColor` | Color | Transparent color for background |
| `ShowInTaskbar` | bool | Display splash in Windows taskbar |

## Key Methods

| Method | Description |
|--------|-------------|
| `ShowSplash()` | Display the splash panel |
| `ShowSplash(Point, Form, bool)` | Display at specific location with owner form |
| `ShowDialogSplash(Form)` | Display as modal dialog |
| `ShowDialogSplash(Point, Form)` | Display as modal dialog at specific location |
| `HideSplash()` | Hide the splash panel |
| `IsShowing()` | Check if splash is currently displayed |
| `SuspendAutoCloseMode()` | Suspend auto-close timer |
| `RestoreAutoCloseMode()` | Restore auto-close timer |

## Common Use Cases

### Use Case 1: Application Startup Screen
Display branding and version information while the application initializes.

### Use Case 2: Loading Indicator
Show a splash panel with progress information during lengthy operations.

### Use Case 3: Notification Popup
Display non-obtrusive messages (new mail, updates) that auto-close or close on click.

### Use Case 4: About/Info Dialog
Show application information as a modal splash dialog.

### Use Case 5: Custom Message Window
Create themed message windows with custom controls and animations.

## Best Practices

1. **Set appropriate TimerInterval**: Use 2000-5000ms for startup splashes, 3000-8000ms for notifications
2. **Enable ShowAsTopMost**: Ensure splash appears above other windows during startup
3. **Use CloseOnClick for notifications**: Allow users to dismiss notification-style popups
4. **Handle BeforeSplash event**: Initialize data or cancel display based on conditions
5. **Choose appropriate animations**: FadeIn for subtle, Slide for dynamic, Marquee for attention-grabbing
6. **Add child controls carefully**: Ensure controls are visible and properly positioned within splash bounds
7. **Test different alignments**: Verify splash appears correctly on different screen resolutions
8. **Dispose properly**: Ensure splash panel is properly disposed when form closes

## Related Components

- **MessageBoxAdv**: For standard message dialogs
- **ProgressBarAdv**: For progress indication within splash screens
- **StatusBarAdv**: For application status information
