# Alignment and Positioning

This guide covers how to control the position of the splash screen on the desktop using the DesktopAlignment property and related positioning features.

## Overview

The SplashControl provides flexible positioning options to display the splash screen at various locations on the desktop, from centered displays to system tray positioning and custom locations.

## DesktopAlignment Property

### Property Details

**Property:** `DesktopAlignment`  
**Type:** `Syncfusion.Windows.Forms.Tools.SplashAlignment`  
**Default:** `SplashAlignment.Center`

### Available Alignment Options

| Alignment Value | Description | Visual Position |
|----------------|-------------|-----------------|
| **Center** | Centers splash on primary screen | Middle of screen |
| **SystemTray** | Near system tray area | Bottom-right corner |
| **LeftTop** | Top-left corner of screen | Upper-left |
| **LeftBottom** | Bottom-left corner of screen | Lower-left |
| **RightTop** | Top-right corner of screen | Upper-right |
| **RightBottom** | Bottom-right corner of screen | Lower-right |
| **Custom** | User-defined location | Via code positioning |

## Setting Alignment

### Using Center Alignment

**C# Example:**

```csharp
// Center on screen (most common)
splashControl1.DesktopAlignment = SplashAlignment.Center;
splashControl1.SplashImage = Properties.Resources.Logo;
splashControl1.TimerInterval = 3000;
```

**VB.NET Example:**

```vb
' Center on screen (most common)
splashControl1.DesktopAlignment = SplashAlignment.Center
splashControl1.SplashImage = My.Resources.Logo
splashControl1.TimerInterval = 3000
```

**When to use:**
- Default choice for most applications
- Best for branding and logo displays
- Ensures visibility on all screen sizes
- Professional and standard behavior

### Using SystemTray Alignment

**C# Example:**

```csharp
// Position near system tray
splashControl1.DesktopAlignment = SplashAlignment.SystemTray;
splashControl1.SplashImage = Properties.Resources.SmallNotification;
splashControl1.TimerInterval = 2000;
```

**VB.NET Example:**

```vb
' Position near system tray
splashControl1.DesktopAlignment = SplashAlignment.SystemTray
splashControl1.SplashImage = My.Resources.SmallNotification
splashControl1.TimerInterval = 2000
```

**When to use:**
- Non-intrusive notifications
- Quick status messages
- Background application updates
- Less prominent splash displays

![SystemTray alignment example](../images/systemtray-alignment.png)

### Using Corner Alignments

**C# Example:**

```csharp
// Top-left corner
splashControl1.DesktopAlignment = SplashAlignment.LeftTop;

// Top-right corner
splashControl1.DesktopAlignment = SplashAlignment.RightTop;

// Bottom-left corner
splashControl1.DesktopAlignment = SplashAlignment.LeftBottom;

// Bottom-right corner
splashControl1.DesktopAlignment = SplashAlignment.RightBottom;
```

**VB.NET Example:**

```vb
' Top-left corner
splashControl1.DesktopAlignment = SplashAlignment.LeftTop

' Top-right corner
splashControl1.DesktopAlignment = SplashAlignment.RightTop

' Bottom-left corner
splashControl1.DesktopAlignment = SplashAlignment.LeftBottom

' Bottom-right corner
splashControl1.DesktopAlignment = SplashAlignment.RightBottom
```

**When to use:**
- Specific design requirements
- Multi-monitor scenarios
- Custom application layouts
- Non-standard splash behavior

## Alignment Scenarios

### Scenario 1: Professional Centered Splash

Standard corporate application startup:

```csharp
private void ConfigureCenteredSplash()
{
    splashControl1.HostForm = this;
    splashControl1.SplashImage = Properties.Resources.CompanyLogo;
    splashControl1.DesktopAlignment = SplashAlignment.Center;
    splashControl1.ShowAnimation = true;
    splashControl1.TimerInterval = 4000;
    splashControl1.AutoMode = true;
}
```

### Scenario 2: Non-Intrusive System Tray Notification

Quick notification-style splash:

```csharp
private void ConfigureNotificationSplash()
{
    splashControl1.HostForm = this;
    splashControl1.SplashImage = Properties.Resources.UpdateNotification;
    splashControl1.DesktopAlignment = SplashAlignment.SystemTray;
    splashControl1.ShowAnimation = false; // Instant display
    splashControl1.TimerInterval = 3000;
    splashControl1.ShowAsTopMost = true;
}
```

### Scenario 3: Corner-Positioned Status Display

Display status information in a corner:

```csharp
private void ConfigureCornerSplash()
{
    splashControl1.HostForm = this;
    splashControl1.SplashImage = Properties.Resources.StatusMessage;
    splashControl1.DesktopAlignment = SplashAlignment.RightBottom;
    splashControl1.TimerInterval = 5000;
    splashControl1.ShowAnimation = false;
}
```

### Scenario 4: Multi-Monitor Aware Splash

Center on the monitor where the main form will appear:

```csharp
private void ConfigureMultiMonitorSplash()
{
    splashControl1.HostForm = this;
    splashControl1.SplashImage = Properties.Resources.Logo;
    
    // Center alignment will center on primary monitor
    splashControl1.DesktopAlignment = SplashAlignment.Center;
    
    // Or use ShowDialogSplash for specific monitor
    // See Custom Positioning section
    
    splashControl1.TimerInterval = 3000;
}
```

## Custom Positioning

For precise control over splash screen location, use the `ShowDialogSplash` method with explicit coordinates.

### Using ShowDialogSplash with Custom Location

**C# Example:**

```csharp
// Show at specific screen coordinates
private void ShowSplashAtCustomLocation()
{
    Point customLocation = new Point(700, 400);
    splashControl1.ShowDialogSplash(customLocation, this);
}

// Center on specific monitor
private void ShowSplashOnSecondMonitor()
{
    if (Screen.AllScreens.Length > 1)
    {
        Screen secondMonitor = Screen.AllScreens[1];
        
        // Calculate center of second monitor
        int centerX = secondMonitor.Bounds.X + (secondMonitor.Bounds.Width / 2) - 200;
        int centerY = secondMonitor.Bounds.Y + (secondMonitor.Bounds.Height / 2) - 150;
        
        Point centerLocation = new Point(centerX, centerY);
        splashControl1.ShowDialogSplash(centerLocation, this);
    }
}

// Position relative to main form
private void ShowSplashRelativeToForm()
{
    // Show splash slightly below main form
    Point formCenter = new Point(
        this.Location.X + (this.Width / 2) - 200,
        this.Location.Y + this.Height + 20
    );
    
    splashControl1.ShowDialogSplash(formCenter, this);
}
```

**VB.NET Example:**

```vb
' Show at specific screen coordinates
Private Sub ShowSplashAtCustomLocation()
    Dim customLocation As New Point(700, 400)
    splashControl1.ShowDialogSplash(customLocation, Me)
End Sub

' Center on specific monitor
Private Sub ShowSplashOnSecondMonitor()
    If Screen.AllScreens.Length > 1 Then
        Dim secondMonitor As Screen = Screen.AllScreens(1)
        
        ' Calculate center of second monitor
        Dim centerX As Integer = secondMonitor.Bounds.X + (secondMonitor.Bounds.Width \ 2) - 200
        Dim centerY As Integer = secondMonitor.Bounds.Y + (secondMonitor.Bounds.Height \ 2) - 150
        
        Dim centerLocation As New Point(centerX, centerY)
        splashControl1.ShowDialogSplash(centerLocation, Me)
    End If
End Sub
```

## Advanced Positioning Examples

### Example 1: Adaptive Positioning Based on Screen Size

```csharp
private void ConfigureAdaptiveSplash()
{
    Screen primaryScreen = Screen.PrimaryScreen;
    
    if (primaryScreen.Bounds.Width < 1280)
    {
        // Small screen - use top corner to not obscure content
        splashControl1.DesktopAlignment = SplashAlignment.RightTop;
    }
    else
    {
        // Large screen - use center
        splashControl1.DesktopAlignment = SplashAlignment.Center;
    }
    
    splashControl1.SplashImage = Properties.Resources.Logo;
    splashControl1.TimerInterval = 3000;
}
```

### Example 2: Position Based on Application Type

```csharp
public class PositioningExamples
{
    // Full-screen application - corner position
    private void ConfigureFullScreenAppSplash()
    {
        splashControl1.DesktopAlignment = SplashAlignment.RightBottom;
        splashControl1.ShowAsTopMost = true;
    }
    
    // Dialog-based application - center position
    private void ConfigureDialogAppSplash()
    {
        splashControl1.DesktopAlignment = SplashAlignment.Center;
        splashControl1.ShowAnimation = true;
    }
    
    // Tray application - system tray position
    private void ConfigureTrayAppSplash()
    {
        splashControl1.DesktopAlignment = SplashAlignment.SystemTray;
        splashControl1.TimerInterval = 2000;
    }
}
```

### Example 3: Dynamic Positioning Based on Mouse Location

```csharp
private void ShowSplashNearMouse()
{
    Point mousePosition = Cursor.Position;
    
    // Offset from mouse to avoid covering cursor
    Point splashLocation = new Point(
        mousePosition.X + 20,
        mousePosition.Y + 20
    );
    
    // Ensure splash stays on screen
    Screen currentScreen = Screen.FromPoint(mousePosition);
    if (splashLocation.X + 400 > currentScreen.Bounds.Right)
    {
        splashLocation.X = currentScreen.Bounds.Right - 400;
    }
    if (splashLocation.Y + 300 > currentScreen.Bounds.Bottom)
    {
        splashLocation.Y = currentScreen.Bounds.Bottom - 300;
    }
    
    splashControl1.ShowDialogSplash(splashLocation, this);
}
```

### Example 4: Remember Last Position

```csharp
public class RememberPositionExample : Form
{
    private SplashControl splashControl1;
    private const string PositionSettingKey = "SplashPosition";
    
    private void ShowSplashWithSavedPosition()
    {
        // Try to load saved position
        string savedPosition = Properties.Settings.Default.SplashPosition;
        
        if (!string.IsNullOrEmpty(savedPosition))
        {
            // Parse saved position
            string[] parts = savedPosition.Split(',');
            if (parts.Length == 2 && 
                int.TryParse(parts[0], out int x) && 
                int.TryParse(parts[1], out int y))
            {
                Point savedPoint = new Point(x, y);
                splashControl1.ShowDialogSplash(savedPoint, this);
                return;
            }
        }
        
        // Use default center position
        splashControl1.DesktopAlignment = SplashAlignment.Center;
        splashControl1.ShowSplash(false);
    }
    
    // Save position when splash closes
    private void SaveSplashPosition(Point location)
    {
        Properties.Settings.Default.SplashPosition = $"{location.X},{location.Y}";
        Properties.Settings.Default.Save();
    }
}
```

## Best Practices

### Alignment Selection Guidelines

| Application Type | Recommended Alignment | Reason |
|-----------------|----------------------|--------|
| Business Applications | Center | Professional, expected behavior |
| Utilities | SystemTray | Non-intrusive |
| Games | Center | Full attention on branding |
| Background Services | RightBottom or SystemTray | Minimal distraction |
| Kiosks | Center | Consistent experience |

### General Best Practices

1. **Default to Center:** Unless you have specific reasons, use Center alignment
2. **Consider screen sizes:** Test positioning on various resolutions
3. **Respect multi-monitor setups:** Use ShowDialogSplash for monitor-specific positioning
4. **Keep consistency:** Use the same alignment throughout your application
5. **Test with animations:** Some positions work better with or without animation
6. **Avoid edges:** If using custom positioning, leave margin from screen edges

### Multi-Monitor Considerations

```csharp
private void ConfigureMultiMonitorAwareSplash()
{
    // Get the screen where the main form will appear
    Screen targetScreen = Screen.FromControl(this);
    
    if (Screen.AllScreens.Length > 1)
    {
        // Calculate center of target screen
        int centerX = targetScreen.Bounds.X + (targetScreen.Bounds.Width / 2) - 200;
        int centerY = targetScreen.Bounds.Y + (targetScreen.Bounds.Height / 2) - 150;
        
        splashControl1.ShowDialogSplash(new Point(centerX, centerY), this);
    }
    else
    {
        // Single monitor - use standard center alignment
        splashControl1.DesktopAlignment = SplashAlignment.Center;
        splashControl1.ShowSplash(false);
    }
}
```

### Positioning Checklist

- [ ] Alignment appropriate for application type
- [ ] Tested on different screen resolutions
- [ ] Tested on multi-monitor setups
- [ ] Splash doesn't obscure important UI elements
- [ ] Position works well with animation (if enabled)
- [ ] ShowAsTopMost configured appropriately
- [ ] Custom positioning accounts for screen boundaries
- [ ] Positioning consistent with application design language

## Common Issues and Solutions

### Issue: Splash Appears on Wrong Monitor

**Problem:** Splash displays on primary monitor even though application is on secondary monitor.

**Solution:**

```csharp
private void EnsureSplashOnCorrectMonitor()
{
    Screen appScreen = Screen.FromControl(this);
    
    if (appScreen != Screen.PrimaryScreen)
    {
        // Calculate position on correct monitor
        int centerX = appScreen.Bounds.X + (appScreen.Bounds.Width / 2) - 200;
        int centerY = appScreen.Bounds.Y + (appScreen.Bounds.Height / 2) - 150;
        
        splashControl1.ShowDialogSplash(new Point(centerX, centerY), this);
    }
    else
    {
        splashControl1.DesktopAlignment = SplashAlignment.Center;
        splashControl1.ShowSplash(false);
    }
}
```

### Issue: Splash Partially Off-Screen

**Problem:** Custom positioning places splash partially outside visible area.

**Solution:**

```csharp
private Point ValidateSplashPosition(Point desiredLocation, Size splashSize)
{
    Screen targetScreen = Screen.FromPoint(desiredLocation);
    
    // Ensure X position is within bounds
    if (desiredLocation.X < targetScreen.Bounds.Left)
        desiredLocation.X = targetScreen.Bounds.Left + 10;
    if (desiredLocation.X + splashSize.Width > targetScreen.Bounds.Right)
        desiredLocation.X = targetScreen.Bounds.Right - splashSize.Width - 10;
    
    // Ensure Y position is within bounds
    if (desiredLocation.Y < targetScreen.Bounds.Top)
        desiredLocation.Y = targetScreen.Bounds.Top + 10;
    if (desiredLocation.Y + splashSize.Height > targetScreen.Bounds.Bottom)
        desiredLocation.Y = targetScreen.Bounds.Bottom - splashSize.Height - 10;
    
    return desiredLocation;
}
```

### Issue: Inconsistent Positioning Across Windows Versions

**Problem:** Splash position varies between Windows 10 and Windows 11.

**Solution:** Use DesktopAlignment properties rather than custom coordinates when possible, as they automatically adapt to different Windows versions and DPI settings.
