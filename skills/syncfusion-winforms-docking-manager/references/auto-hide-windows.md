# Auto-Hide Windows

Auto-hide mode collapses dock windows to tabs at the form edges, providing more workspace. Windows slide out when hovering over the tab and auto-hide again when focus is lost.

## Overview

**What:** Windows that collapse to edge tabs and expand on hover.

**When to use:**
- Maximize workspace for main content
- Occasional-use tools (properties, toolbox)
- Visual Studio-style layouts
- Space-constrained interfaces

**How:** Use `SetAutoHideMode()` or user clicks pin button.

## Enabling Auto-Hide

### Set Auto-Hide Programmatically

```csharp
// Set a control to auto-hide mode
this.dockingManager1.SetAutoHideMode(panel1, true);

// Disable auto-hide mode (return to previous dock state)
this.dockingManager1.SetAutoHideMode(panel1, false);
```

**VB.NET:**

```vb
' Enable auto-hide
Me.dockingManager1.SetAutoHideMode(panel1, True)

' Disable auto-hide
Me.dockingManager1.SetAutoHideMode(panel1, False)
```

### Auto-Hide with Specific Side

```csharp
// Auto-hide to a specific side with size
this.dockingManager1.DockControlInAutoHideMode(panel1, 
    DockingStyle.Left, 250);

// Available sides: Left, Right, Top, Bottom
this.dockingManager1.DockControlInAutoHideMode(panel2, 
    DockingStyle.Right, 200);
```

**VB.NET:**

```vb
' Auto-hide to left side
Me.dockingManager1.DockControlInAutoHideMode(panel1, _
    DockingStyle.Left, 250)
```

### Auto-Hide on Application Load

```csharp
// Set control to auto-hide when application starts
this.dockingManager1.SetAutoHideOnLoad(panel1, true);

// This must be set BEFORE docking the control
```

Typically set in form constructor or `Load` event before arranging windows.

## User-Initiated Auto-Hide

### Enable Pin Button

By default, users can toggle auto-hide using the pin button in the caption.

**Pin Button States:**
- **Unpinned (horizontal)** - Control is in auto-hide mode
- **Pinned (vertical)** - Control is docked normally

### Disable Auto-Hide Functionality

```csharp
// Disable auto-hide for all windows (hide pin button)
this.dockingManager1.AutoHideEnabled = false;
```

Users cannot auto-hide any windows when `false`.

### Hide Pin Button for Specific Control

```csharp
// Hide pin button for specific control
this.dockingManager1.SetAutoHideButtonVisibility(panel1, false);
```

The control can still be auto-hidden programmatically.

## Animation Configuration

### Animation Speed

```csharp
// Set animation step (pixels per frame)
this.dockingManager1.AnimationStep = 20; // Default is 20

// Faster animation
this.dockingManager1.AnimationStep = 40;

// Slower animation
this.dockingManager1.AnimationStep = 10;
```

Higher values = faster animation.

### Disable Animation

```csharp
// Disable slide animation (instant show/hide)
this.dockingManager1.AnimateAutoHiddenWindow = false;
```

Window appears/disappears instantly without sliding effect.

### Auto-Hide Interval

```csharp
// Set hover delay before showing window (milliseconds)
this.dockingManager1.AutoHideInterval = 300; // Default is 300ms

// Shorter delay (more responsive)
this.dockingManager1.AutoHideInterval = 100;

// Longer delay (less sensitive)
this.dockingManager1.AutoHideInterval = 500;
```

## Auto-Hide Tab Customization

### Tab Selection Behavior

```csharp
// Show window on mouse hover (default)
this.dockingManager1.AutoHideSelectionStyle = AutoHideSelectionStyle.MouseHover;

// Show window only on click
this.dockingManager1.AutoHideSelectionStyle = AutoHideSelectionStyle.Click;
```

**MouseHover:** Window appears when hovering over tab.
**Click:** User must click tab to show window.

### Tab Font and Colors

```csharp
// Set auto-hide tab font
this.dockingManager1.AutoHideTabFont = new Font("Segoe UI", 9f, FontStyle.Bold);

// Set tab foreground color
this.dockingManager1.AutoHideTabForeColor = Color.DarkBlue;

// Set tab height
this.dockingManager1.AutoHideTabHeight = 25; // Default is about 20
```

**VB.NET:**

```vb
' Customize auto-hide tabs
Me.dockingManager1.AutoHideTabFont = New Font("Segoe UI", 9.0F, FontStyle.Bold)
Me.dockingManager1.AutoHideTabForeColor = Color.DarkBlue
Me.dockingManager1.AutoHideTabHeight = 25
```

### Tab Background Color

```csharp
// Get auto-hide tab control to customize background
// For left-side tabs
Control ahTabControl = this.dockingManager1.GetAHTabControl(
    Syncfusion.Windows.Forms.Tools.DockTabAlignmentStyle.Left);

if (ahTabControl != null)
{
    ahTabControl.BackColor = Color.LightGray;
}

// For other sides: Right, Top, Bottom
Control ahTabRight = this.dockingManager1.GetAHTabControl(
    Syncfusion.Windows.Forms.Tools.DockTabAlignmentStyle.Right);
if (ahTabRight != null)
{
    ahTabRight.BackColor = Color.LightGreen;
}
```

### Full Caption in Auto-Hide Mode

```csharp
// Show full caption instead of vertical text
this.dockingManager1.FullCaptionsInAutoHideMode = true;
```

When `true`, auto-hide tabs display full horizontal captions instead of vertical text.

### Enable Tab Dragging

```csharp
// Allow dragging auto-hide tabs to reorder them
this.dockingManager1.EnableDragAutoHiddenTabs = true;
```

Users can reorder auto-hide tabs by dragging.

### Context Menu for Auto-Hide Tabs

```csharp
// Enable context menu on auto-hide tabs
this.dockingManager1.EnableAutoHideTabContextMenu = true;
```

Right-clicking an auto-hide tab shows a context menu with options.

## Caption Notification on Hover

```csharp
// Customize hover notification display time (milliseconds)
this.dockingManager1.AutoHideHoverTime = 1000; // Default is 1000ms

// Show tooltip when hovering over auto-hide tab
this.dockingManager1.ShowToolTips = true;
```

## Auto-Hide Events

### Animation Events

```csharp
// Handle when auto-hide animation starts
this.dockingManager1.AutoHideAnimationStart += (s, e) =>
{
    Console.WriteLine($"Animation started for: {e.DockBorder}");
    Console.WriteLine($"Window bounds: {e.Bounds}");
};

// Handle when auto-hide animation stops
this.dockingManager1.AutoHideAnimationStop += (s, e) =>
{
    Console.WriteLine($"Animation stopped for: {e.DockBorder}");
};
```

**AutoHideAnimationEventArgs Properties:**
- `Bounds` - Rectangle of the animating window
- `DockBorder` - Which side (Left, Right, Top, Bottom)

### State Change Events

```csharp
// Handle dock state changes
this.dockingManager1.DockStateChanged += (s, e) =>
{
    if (e.NewState == DockState.AutoHide)
    {
        Console.WriteLine($"Control entered auto-hide mode");
    }
    else if (e.OldState == DockState.AutoHide)
    {
        Console.WriteLine($"Control left auto-hide mode");
    }
};
```

## Complete Example

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class AutoHideExample : Form
{
    private DockingManager dockingManager1;
    private Panel toolbox, properties, explorer, output;
    private Button btnToggleAutoHide, btnCheckState;
    
    public AutoHideExample()
    {
        InitializeComponent();
        SetupDocking();
        ConfigureAutoHide();
        SetupControls();
    }
    
    private void SetupDocking()
    {
        // Create DockingManager
        this.dockingManager1 = new DockingManager(this.components);
        this.dockingManager1.HostControl = this;
        
        // Create panels
        toolbox = new Panel { BackColor = Color.LightBlue };
        properties = new Panel { BackColor = Color.LightGreen };
        explorer = new Panel { BackColor = Color.LightYellow };
        output = new Panel { BackColor = Color.LightCoral };
        
        this.Controls.AddRange(new Control[] { 
            toolbox, properties, explorer, output 
        });
        
        // Enable docking
        this.dockingManager1.SetEnableDocking(toolbox, true);
        this.dockingManager1.SetEnableDocking(properties, true);
        this.dockingManager1.SetEnableDocking(explorer, true);
        this.dockingManager1.SetEnableDocking(output, true);
        
        // Set labels
        this.dockingManager1.SetDockLabel(toolbox, "Toolbox");
        this.dockingManager1.SetDockLabel(properties, "Properties");
        this.dockingManager1.SetDockLabel(explorer, "Solution Explorer");
        this.dockingManager1.SetDockLabel(output, "Output");
        
        // Set auto-hide on load for specific controls
        this.dockingManager1.SetAutoHideOnLoad(properties, true);
        this.dockingManager1.SetAutoHideOnLoad(output, true);
        
        // Arrange windows
        this.dockingManager1.DockControl(toolbox, this, 
            DockingStyle.Left, 200);
        this.dockingManager1.DockControl(properties, this, 
            DockingStyle.Right, 250);
        this.dockingManager1.DockControl(explorer, properties, 
            DockingStyle.Tabbed, 250);
        this.dockingManager1.DockControl(output, this, 
            DockingStyle.Bottom, 150);
    }
    
    private void ConfigureAutoHide()
    {
        // Animation settings
        this.dockingManager1.AnimateAutoHiddenWindow = true;
        this.dockingManager1.AnimationStep = 25;
        this.dockingManager1.AutoHideInterval = 250;
        
        // Tab behavior
        this.dockingManager1.AutoHideSelectionStyle = 
            AutoHideSelectionStyle.MouseHover;
        
        // Tab appearance
        this.dockingManager1.AutoHideTabFont = 
            new Font("Segoe UI", 9f, FontStyle.Regular);
        this.dockingManager1.AutoHideTabForeColor = Color.Navy;
        this.dockingManager1.AutoHideTabHeight = 24;
        
        // Display options
        this.dockingManager1.FullCaptionsInAutoHideMode = false;
        this.dockingManager1.EnableDragAutoHiddenTabs = true;
        this.dockingManager1.EnableAutoHideTabContextMenu = true;
        
        // Visual style
        this.dockingManager1.VisualStyle = VisualStyle.Office2016Colorful;
        
        // Customize tab background colors
        CustomizeTabBackgrounds();
        
        // Handle events
        this.dockingManager1.AutoHideAnimationStart += 
            DockingManager1_AutoHideAnimationStart;
        this.dockingManager1.AutoHideAnimationStop += 
            DockingManager1_AutoHideAnimationStop;
        this.dockingManager1.DockStateChanged += 
            DockingManager1_DockStateChanged;
    }
    
    private void CustomizeTabBackgrounds()
    {
        // Customize left side tabs
        Control leftTabs = this.dockingManager1.GetAHTabControl(
            Syncfusion.Windows.Forms.Tools.DockTabAlignmentStyle.Left);
        if (leftTabs != null)
            leftTabs.BackColor = Color.FromArgb(240, 240, 255);
        
        // Customize right side tabs
        Control rightTabs = this.dockingManager1.GetAHTabControl(
            Syncfusion.Windows.Forms.Tools.DockTabAlignmentStyle.Right);
        if (rightTabs != null)
            rightTabs.BackColor = Color.FromArgb(240, 255, 240);
        
        // Customize bottom tabs
        Control bottomTabs = this.dockingManager1.GetAHTabControl(
            Syncfusion.Windows.Forms.Tools.DockTabAlignmentStyle.Bottom);
        if (bottomTabs != null)
            bottomTabs.BackColor = Color.FromArgb(255, 240, 240);
    }
    
    private void SetupControls()
    {
        // Add control buttons
        btnToggleAutoHide = new Button 
        { 
            Text = "Toggle Auto-Hide", 
            Dock = DockStyle.Top 
        };
        btnToggleAutoHide.Click += BtnToggleAutoHide_Click;
        
        btnCheckState = new Button 
        { 
            Text = "Check State", 
            Dock = DockStyle.Top 
        };
        btnCheckState.Click += BtnCheckState_Click;
        
        toolbox.Controls.AddRange(new Control[] 
        { 
            btnCheckState, btnToggleAutoHide 
        });
    }
    
    private void BtnToggleAutoHide_Click(object sender, EventArgs e)
    {
        // Toggle auto-hide mode for toolbox
        DockState currentState = this.dockingManager1.GetDockState(toolbox);
        
        if (currentState == DockState.AutoHide)
        {
            // Return to docked state
            this.dockingManager1.SetAutoHideMode(toolbox, false);
        }
        else
        {
            // Enter auto-hide mode
            this.dockingManager1.SetAutoHideMode(toolbox, true);
        }
    }
    
    private void BtnCheckState_Click(object sender, EventArgs e)
    {
        // Check current dock state
        DockState state = this.dockingManager1.GetDockState(toolbox);
        
        string message = $"Toolbox State: {state}\n\n";
        
        if (state == DockState.AutoHide)
        {
            message += "The window is in auto-hide mode.\n" +
                      "Hover over the tab to expand it.";
        }
        else
        {
            message += "The window is docked normally.\n" +
                      "Click the pin button to auto-hide.";
        }
        
        MessageBox.Show(message, "Dock State");
    }
    
    private void DockingManager1_AutoHideAnimationStart(object sender, 
        AutoHideAnimationEventArgs e)
    {
        Console.WriteLine($"Auto-hide animation started: {e.DockBorder}");
        Console.WriteLine($"Window bounds: {e.Bounds}");
    }
    
    private void DockingManager1_AutoHideAnimationStop(object sender, 
        AutoHideAnimationEventArgs e)
    {
        Console.WriteLine($"Auto-hide animation stopped: {e.DockBorder}");
    }
    
    private void DockingManager1_DockStateChanged(object sender, 
        DockStateChangeEventArgs e)
    {
        if (e.Controls.Length > 0)
        {
            string label = this.dockingManager1.GetDockLabel(e.Controls[0]);
            
            if (e.NewState == DockState.AutoHide)
            {
                this.Text = $"{label} entered auto-hide mode";
            }
            else if (e.OldState == DockState.AutoHide)
            {
                this.Text = $"{label} left auto-hide mode";
            }
        }
    }
}
```

**VB.NET Example:**

```vb
Private Sub SetupAutoHide()
    ' Create DockingManager
    Me.dockingManager1 = New DockingManager(Me.components)
    Me.dockingManager1.HostControl = Me
    
    ' Enable docking
    Me.dockingManager1.SetEnableDocking(panel1, True)
    Me.dockingManager1.SetDockLabel(panel1, "Toolbox")
    
    ' Configure auto-hide
    Me.dockingManager1.AnimateAutoHiddenWindow = True
    Me.dockingManager1.AnimationStep = 25
    Me.dockingManager1.AutoHideInterval = 250
    Me.dockingManager1.AutoHideSelectionStyle = AutoHideSelectionStyle.MouseHover
    
    ' Set auto-hide on load
    Me.dockingManager1.SetAutoHideOnLoad(panel1, True)
    
    ' Dock control
    Me.dockingManager1.DockControl(panel1, Me, DockingStyle.Left, 200)
End Sub

Private Sub ToggleAutoHide()
    Dim currentState As DockState = Me.dockingManager1.GetDockState(panel1)
    
    If currentState = DockState.AutoHide Then
        Me.dockingManager1.SetAutoHideMode(panel1, False)
    Else
        Me.dockingManager1.SetAutoHideMode(panel1, True)
    End If
End Sub
```

## Best Practices

1. **Auto-hide occasional tools** - Ideal for properties, toolbox, rarely-used panels
2. **Keep main content docked** - Don't auto-hide primary work areas
3. **Use appropriate delays** - Balance responsiveness with accidental triggers
4. **Set on load for space savings** - Start with less important tools auto-hidden
5. **Customize tab appearance** - Match your application theme
6. **Enable hover selection** - More intuitive than click-only
7. **Allow tab dragging** - Let users organize auto-hide tabs

## Troubleshooting

**Auto-hide not working:**
- Check `AutoHideEnabled` is `true`
- Verify control is dockable (not floating or fill mode)
- Ensure pin button is visible
- Check `DockAbility` allows auto-hide

**Animation is too slow/fast:**
- Adjust `AnimationStep` property (10-40 range)
- Set `AnimateAutoHiddenWindow` to `false` for instant display
- Check `AutoHideInterval` isn't too long

**Tab doesn't expand on hover:**
- Verify `AutoHideSelectionStyle` is `MouseHover`
- Check `AutoHideInterval` isn't too high
- Ensure window is actually in auto-hide mode

**Cannot customize tab background:**
- Use `GetAHTabControl()` to get tab control
- Set `BackColor` on returned control
- Must be called after windows are docked
