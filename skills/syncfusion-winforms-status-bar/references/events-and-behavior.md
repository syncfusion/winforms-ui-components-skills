# Events and Behavior

This guide covers auto-sizing behavior, preferred size methods, and the nine property change events available in StatusBarAdv.

## When to Read This

Read this reference when:
- Implementing auto-sizing for status bars
- Configuring automatic height adjustment for panels
- Handling border property change events
- Responding to gradient or background changes
- Implementing theme change notifications
- Using GetPreferredSize and SetPreferredSize methods

## AutoSize Settings

AutoSize properties control how StatusBarAdv automatically adjusts its size to fit content.

### AutoSize Property

**C#:**
```csharp
// Enable auto-sizing
statusBarAdv1.AutoSize = true;

// Disable auto-sizing (use fixed size)
statusBarAdv1.AutoSize = false;
```

**VB.NET:**
```vbnet
' Enable auto-sizing
statusBarAdv1.AutoSize = True

' Disable auto-sizing (use fixed size)
statusBarAdv1.AutoSize = False
```

### AutoSizeMode Property

When **AutoSize** is enabled, **AutoSizeMode** determines how the control resizes.

**AutoSizeMode.GrowAndShrink:**
```csharp
statusBarAdv1.AutoSizeMode = AutoSizeMode.GrowAndShrink;
```
Control grows and shrinks to fit content, potentially smaller or larger than the **Size** property value.

**AutoSizeMode.GrowOnly:**
```csharp
statusBarAdv1.AutoSizeMode = AutoSizeMode.GrowOnly;
```
Control grows to fit content but never shrinks below the **Size** property value.

### AutoSize Configuration Example

**C#:**
```csharp
public void ConfigureAutoSize()
{
    // Enable auto-sizing
    statusBarAdv1.AutoSize = true;
    
    // Grow and shrink mode
    statusBarAdv1.AutoSizeMode = AutoSizeMode.GrowAndShrink;
    
    // StatusBarAdv will automatically adjust height based on content
}
```

**VB.NET:**
```vbnet
Public Sub ConfigureAutoSize()
    ' Enable auto-sizing
    statusBarAdv1.AutoSize = True
    
    ' Grow and shrink mode
    statusBarAdv1.AutoSizeMode = AutoSizeMode.GrowAndShrink
End Sub
```

### GrowOnly vs GrowAndShrink

**C#:**
```csharp
// GrowOnly: Minimum size maintained
statusBarAdv1.Size = new Size(800, 25);
statusBarAdv1.AutoSizeMode = AutoSizeMode.GrowOnly;
// Result: Height will be at least 25, can grow larger

// GrowAndShrink: Fits content exactly
statusBarAdv1.AutoSizeMode = AutoSizeMode.GrowAndShrink;
// Result: Height adjusts to exact content size, may be < 25
```

## AutoHeightControls Property

The **AutoHeightControls** property automatically adjusts panel heights when the StatusBarAdv height changes.

**C#:**
```csharp
// Enable automatic panel height adjustment
statusBarAdv1.AutoHeightControls = true;

// Disable (panels maintain fixed height)
statusBarAdv1.AutoHeightControls = false;
```

**VB.NET:**
```vbnet
' Enable automatic panel height adjustment
statusBarAdv1.AutoHeightControls = True

' Disable (panels maintain fixed height)
statusBarAdv1.AutoHeightControls = False
```

### AutoHeightControls Example

**C#:**
```csharp
public void ConfigureAutoHeightPanels()
{
    // Set StatusBarAdv height
    statusBarAdv1.Height = 30;
    
    // Enable auto-height for panels
    statusBarAdv1.AutoHeightControls = true;
    
    // Add panels
    StatusBarAdvPanel panel1 = new StatusBarAdvPanel
    {
        Text = "Panel 1",
        Size = new Size(100, 20)  // Height will auto-adjust to 30
    };
    
    StatusBarAdvPanel panel2 = new StatusBarAdvPanel
    {
        Text = "Panel 2",
        Size = new Size(100, 20)  // Height will auto-adjust to 30
    };
    
    statusBarAdv1.Controls.Add(panel1);
    statusBarAdv1.Controls.Add(panel2);
    
    // Panels automatically resize height to match StatusBarAdv
}
```

**Default Value:** `AutoHeightControls` is **True** by default.

## PreferredSize Methods

Methods for getting and setting preferred sizes in the layout.

### GetPreferredSize Method

Returns the preferred size of a specified control.

**Method Signature:**
```csharp
Size GetPreferredSize(Control control)
```

**C#:**
```csharp
// Get preferred size for a panel
StatusBarAdvPanel panel = new StatusBarAdvPanel { Text = "Status" };
Size preferredSize = statusBarAdv1.GetPreferredSize(panel);

Console.WriteLine($"Preferred Width: {preferredSize.Width}");
Console.WriteLine($"Preferred Height: {preferredSize.Height}");
```

### SetPreferredSize Method

Sets the preferred size in the layout for a specified control.

**Method Signature:**
```csharp
void SetPreferredSize(Control control, Size size)
```

**C#:**
```csharp
// Set preferred size for a panel
StatusBarAdvPanel panel = new StatusBarAdvPanel { Text = "Status" };
statusBarAdv1.Controls.Add(panel);

// Set preferred size
statusBarAdv1.SetPreferredSize(panel, new Size(150, 25));
```

**VB.NET:**
```vbnet
' Set preferred size for a panel
Dim panel As New StatusBarAdvPanel With {.Text = "Status"}
statusBarAdv1.Controls.Add(panel)

' Set preferred size
statusBarAdv1.SetPreferredSize(panel, New Size(150, 25))
```

### PreferredSize Example

**C#:**
```csharp
public void ConfigurePanelSizes()
{
    // Add panels
    StatusBarAdvPanel statusPanel = new StatusBarAdvPanel 
    { 
        Text = "Ready" 
    };
    
    StatusBarAdvPanel datePanel = new StatusBarAdvPanel 
    { 
        PanelType = StatusBarAdvPanelType.ShortDate 
    };
    
    statusBarAdv1.Controls.Add(statusPanel);
    statusBarAdv1.Controls.Add(datePanel);
    
    // Set preferred sizes
    statusBarAdv1.SetPreferredSize(statusPanel, new Size(120, 25));
    statusBarAdv1.SetPreferredSize(datePanel, new Size(100, 25));
    
    // Get preferred size
    Size statusSize = statusBarAdv1.GetPreferredSize(statusPanel);
    Console.WriteLine($"Status panel preferred: {statusSize}");
}
```

## StatusBarAdv Events

StatusBarAdv provides nine property change events.

### BorderSidesChanged Event

Fires when the **BorderSides** property changes.

**C#:**
```csharp
// Subscribe to event
statusBarAdv1.BorderSidesChanged += StatusBarAdv1_BorderSidesChanged;

private void StatusBarAdv1_BorderSidesChanged(object sender, EventArgs e)
{
    Console.WriteLine("BorderSides changed");
    
    // Get current border sides
    Border3DSide sides = statusBarAdv1.BorderSides;
    Console.WriteLine($"Current sides: {sides}");
}
```

**VB.NET:**
```vbnet
' Subscribe to event
AddHandler statusBarAdv1.BorderSidesChanged, AddressOf StatusBarAdv1_BorderSidesChanged

Private Sub StatusBarAdv1_BorderSidesChanged(sender As Object, e As EventArgs)
    Console.WriteLine("BorderSides changed")
    
    ' Get current border sides
    Dim sides As Border3DSide = statusBarAdv1.BorderSides
    Console.WriteLine($"Current sides: {sides}")
End Sub
```

### BorderColorChanged Event

Fires when the **BorderColor** property changes.

**C#:**
```csharp
// Subscribe to event
statusBarAdv1.BorderColorChanged += StatusBarAdv1_BorderColorChanged;

private void StatusBarAdv1_BorderColorChanged(object sender, EventArgs e)
{
    Console.WriteLine("BorderColor changed");
    Console.WriteLine($"New color: {statusBarAdv1.BorderColor}");
}
```

### BorderSingleChanged Event

Fires when the **BorderSingle** property changes.

**C#:**
```csharp
// Subscribe to event
statusBarAdv1.BorderSingleChanged += StatusBarAdv1_BorderSingleChanged;

private void StatusBarAdv1_BorderSingleChanged(object sender, EventArgs e)
{
    Console.WriteLine("BorderSingle changed");
    Console.WriteLine($"New style: {statusBarAdv1.BorderSingle}");
}
```

### BorderStyleChanged Event

Fires when the **BorderStyle** property changes.

**C#:**
```csharp
// Subscribe to event
statusBarAdv1.BorderStyleChanged += StatusBarAdv1_BorderStyleChanged;

private void StatusBarAdv1_BorderStyleChanged(object sender, EventArgs e)
{
    Console.WriteLine("BorderStyle changed");
    Console.WriteLine($"New style: {statusBarAdv1.BorderStyle}");
}
```

### Border3DStyleChanged Event

Fires when the **Border3DStyle** property changes.

**C#:**
```csharp
// Subscribe to event
statusBarAdv1.Border3DStyleChanged += StatusBarAdv1_Border3DStyleChanged;

private void StatusBarAdv1_Border3DStyleChanged(object sender, EventArgs e)
{
    Console.WriteLine("Border3DStyle changed");
    Console.WriteLine($"New style: {statusBarAdv1.Border3DStyle}");
}
```

### GradientBackgroundChanged Event

Fires when the **GradientBackground** property changes.

**C#:**
```csharp
// Subscribe to event
statusBarAdv1.GradientBackgroundChanged += StatusBarAdv1_GradientBackgroundChanged;

private void StatusBarAdv1_GradientBackgroundChanged(object sender, EventArgs e)
{
    Console.WriteLine("GradientBackground changed");
    
    // Check if gradient is enabled
    bool hasGradient = statusBarAdv1.BackgroundColor.Style == BrushStyle.Gradient;
    Console.WriteLine($"Gradient enabled: {hasGradient}");
}
```

### GradientColorsChanged Event

Fires when the **GradientColors** property changes.

**C#:**
```csharp
// Subscribe to event
statusBarAdv1.GradientColorsChanged += StatusBarAdv1_GradientColorsChanged;

private void StatusBarAdv1_GradientColorsChanged(object sender, EventArgs e)
{
    Console.WriteLine("GradientColors changed");
    
    // Access gradient colors
    Color[] colors = statusBarAdv1.BackgroundColor.GradientColors;
    Console.WriteLine($"Number of gradient colors: {colors?.Length ?? 0}");
}
```

### ThemeChanged Event

Fires when the **ThemesEnabled** property changes.

**C#:**
```csharp
// Subscribe to event
statusBarAdv1.ThemeChanged += StatusBarAdv1_ThemeChanged;

private void StatusBarAdv1_ThemeChanged(object sender, EventArgs e)
{
    Console.WriteLine("ThemesEnabled changed");
    Console.WriteLine($"Themes enabled: {statusBarAdv1.ThemesEnabled}");
}
```

### VerticalGradientChanged Event

Fires when the **VerticalGradient** property changes.

**C#:**
```csharp
// Subscribe to event
statusBarAdv1.VerticalGradientChanged += StatusBarAdv1_VerticalGradientChanged;

private void StatusBarAdv1_VerticalGradientChanged(object sender, EventArgs e)
{
    Console.WriteLine("VerticalGradient changed");
    Console.WriteLine($"Vertical gradient: {statusBarAdv1.VerticalGradient}");
}
```

## Complete Event Handling Example

**C#:**
```csharp
public partial class EventMonitorForm : Form
{
    private StatusBarAdv statusBar;
    private ListBox eventLog;
    
    public EventMonitorForm()
    {
        InitializeComponent();
        SetupStatusBar();
        SubscribeToEvents();
    }
    
    private void SetupStatusBar()
    {
        statusBar = new StatusBarAdv
        {
            Dock = DockStyle.Bottom,
            Height = 28,
            BackColor = Color.LightSteelBlue,
            BorderStyle = BorderStyle.FixedSingle
        };
        
        // Add panel
        statusBar.Controls.Add(new StatusBarAdvPanel
        {
            Text = "Monitoring events...",
            Size = new Size(200, 25)
        });
        
        this.Controls.Add(statusBar);
        
        // Add event log ListBox
        eventLog = new ListBox
        {
            Dock = DockStyle.Fill
        };
        this.Controls.Add(eventLog);
    }
    
    private void SubscribeToEvents()
    {
        // Border events
        statusBar.BorderSidesChanged += (s, e) => 
            LogEvent($"BorderSidesChanged: {statusBar.BorderSides}");
        
        statusBar.BorderColorChanged += (s, e) => 
            LogEvent($"BorderColorChanged: {statusBar.BorderColor.Name}");
        
        statusBar.BorderSingleChanged += (s, e) => 
            LogEvent($"BorderSingleChanged: {statusBar.BorderSingle}");
        
        statusBar.BorderStyleChanged += (s, e) => 
            LogEvent($"BorderStyleChanged: {statusBar.BorderStyle}");
        
        statusBar.Border3DStyleChanged += (s, e) => 
            LogEvent($"Border3DStyleChanged: {statusBar.Border3DStyle}");
        
        // Gradient events
        statusBar.GradientBackgroundChanged += (s, e) => 
            LogEvent("GradientBackgroundChanged");
        
        statusBar.GradientColorsChanged += (s, e) => 
            LogEvent("GradientColorsChanged");
        
        // Theme events
        statusBar.ThemeChanged += (s, e) => 
            LogEvent($"ThemeChanged: Enabled={statusBar.ThemesEnabled}");
        
        statusBar.VerticalGradientChanged += (s, e) => 
            LogEvent($"VerticalGradientChanged: {statusBar.VerticalGradient}");
    }
    
    private void LogEvent(string message)
    {
        string timestamp = DateTime.Now.ToString("HH:mm:ss.fff");
        eventLog.Items.Insert(0, $"[{timestamp}] {message}");
        
        // Keep log size manageable
        if (eventLog.Items.Count > 100)
        {
            eventLog.Items.RemoveAt(eventLog.Items.Count - 1);
        }
    }
    
    // Example: Programmatically trigger events
    public void DemonstrateBorderChange()
    {
        // Change border style (triggers BorderStyleChanged)
        statusBar.BorderStyle = BorderStyle.Fixed3D;
        
        // Change 3D style (triggers Border3DStyleChanged)
        statusBar.Border3DStyle = Border3DStyle.Sunken;
        
        // Change border color (triggers BorderColorChanged)
        statusBar.BorderColor = Color.Navy;
    }
    
    public void DemonstrateGradientChange()
    {
        // Apply gradient (triggers GradientBackgroundChanged)
        statusBar.BackgroundColor = new BrushInfo(
            GradientStyle.Vertical,
            Color.White,
            Color.SteelBlue
        );
        
        // Change to vertical (triggers VerticalGradientChanged)
        statusBar.VerticalGradient = true;
    }
}
```

**VB.NET:**
```vbnet
Public Partial Class EventMonitorForm
    Inherits Form
    
    Private statusBar As StatusBarAdv
    Private eventLog As ListBox
    
    Public Sub New()
        InitializeComponent()
        SetupStatusBar()
        SubscribeToEvents()
    End Sub
    
    Private Sub SetupStatusBar()
        statusBar = New StatusBarAdv With {
            .Dock = DockStyle.Bottom,
            .Height = 28,
            .BackColor = Color.LightSteelBlue,
            .BorderStyle = BorderStyle.FixedSingle
        }
        
        statusBar.Controls.Add(New StatusBarAdvPanel With {
            .Text = "Monitoring events...",
            .Size = New Size(200, 25)
        })
        
        Me.Controls.Add(statusBar)
        
        eventLog = New ListBox With {.Dock = DockStyle.Fill}
        Me.Controls.Add(eventLog)
    End Sub
    
    Private Sub SubscribeToEvents()
        AddHandler statusBar.BorderSidesChanged, Sub(s, e) _
            LogEvent($"BorderSidesChanged: {statusBar.BorderSides}")
        
        AddHandler statusBar.BorderColorChanged, Sub(s, e) _
            LogEvent($"BorderColorChanged: {statusBar.BorderColor.Name}")
        
        AddHandler statusBar.BorderStyleChanged, Sub(s, e) _
            LogEvent($"BorderStyleChanged: {statusBar.BorderStyle}")
        
        AddHandler statusBar.GradientBackgroundChanged, Sub(s, e) _
            LogEvent("GradientBackgroundChanged")
        
        AddHandler statusBar.ThemeChanged, Sub(s, e) _
            LogEvent($"ThemeChanged: Enabled={statusBar.ThemesEnabled}")
    End Sub
    
    Private Sub LogEvent(message As String)
        Dim timestamp As String = DateTime.Now.ToString("HH:mm:ss.fff")
        eventLog.Items.Insert(0, $"[{timestamp}] {message}")
        
        If eventLog.Items.Count > 100 Then
            eventLog.Items.RemoveAt(eventLog.Items.Count - 1)
        End If
    End Sub
End Class
```

## Behavior Configuration Patterns

### Responsive Auto-Sizing

**C#:**
```csharp
public void ConfigureResponsiveBehavior()
{
    // Enable auto-sizing
    statusBarAdv1.AutoSize = true;
    statusBarAdv1.AutoSizeMode = AutoSizeMode.GrowOnly;
    
    // Auto-adjust panel heights
    statusBarAdv1.AutoHeightControls = true;
    
    // StatusBarAdv will adapt to content while maintaining minimum size
}
```

### Fixed Size Behavior

**C#:**
```csharp
public void ConfigureFixedSizeBehavior()
{
    // Disable auto-sizing
    statusBarAdv1.AutoSize = false;
    
    // Set fixed height
    statusBarAdv1.Height = 28;
    
    // Panels maintain their own heights
    statusBarAdv1.AutoHeightControls = false;
}
```

### Event-Driven Theme Switching

**C#:**
```csharp
public void SetupThemeSwitchingEvents()
{
    // Monitor theme changes
    statusBarAdv1.ThemeChanged += (s, e) =>
    {
        if (statusBarAdv1.ThemesEnabled)
        {
            // Adjust other UI elements to match
            UpdateApplicationTheme();
        }
    };
    
    // Monitor gradient changes
    statusBarAdv1.GradientBackgroundChanged += (s, e) =>
    {
        // Update related controls
        SynchronizeControlAppearance();
    };
}

private void UpdateApplicationTheme()
{
    // Update form and controls to match StatusBarAdv theme
}

private void SynchronizeControlAppearance()
{
    // Synchronize other controls with StatusBarAdv appearance
}
```

## Next Steps

After configuring events and behavior:

1. **Return to Main Guide** → Read: [../SKILL.md](../SKILL.md)
   - Review complete StatusBarAdv capabilities
   - Access all reference guides
   - Explore additional use cases
