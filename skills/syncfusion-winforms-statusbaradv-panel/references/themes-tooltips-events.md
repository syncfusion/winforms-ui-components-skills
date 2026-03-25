# Themes, ToolTips, and Events

## Table of Contents
- [Overview](#overview)
- [Theme Support](#theme-support)
- [ToolTip Configuration](#tooltip-configuration)
- [Property Change Events](#property-change-events)
- [Complete Event Monitoring Example](#complete-event-monitoring-example)

This guide covers theme support, tooltip configuration, and comprehensive event handling for StatusBarAdvPanel.

## Overview

## When to Read This

Read this guide when you need to:
- Enable themed appearance for panels
- Configure theme background behavior
- Add tooltips to panels for additional information
- Handle property change events
- Monitor alignment changes
- Track animation property modifications
- Respond to border or gradient changes
- Implement event-driven panel updates

## Theme Support

StatusBarAdvPanel supports themed appearance to match application design.

### ThemesEnabled Property

Enables themed appearance for the panel.

**Property:**
- **Type:** `bool`
- **Default:** `false`
- **Effect:** When `true`, sets background to transparent and applies indicated settings (BorderSides = Right, BorderStyle = Fixed3D, Border3DStyle = Etched)

**C#:**
```csharp
var themedPanel = new StatusBarAdvPanel
{
    Text = "Themed Panel",
    ThemesEnabled = true,
    Size = new Size(150, 24)
};
```

**VB.NET:**
```vb
Dim themedPanel = New StatusBarAdvPanel With {
    .Text = "Themed Panel",
    .ThemesEnabled = True,
    .Size = New Size(150, 24)
}
```

### IgnoreThemeBackground Property

Controls whether the panel uses the theme's background color or a custom BackColor.

**Property:**
- **Type:** `bool`
- **Default:** `false`
- **Effect:** When `true`, ignores theme background and uses custom BackColor instead

**C#:**
```csharp
// Use theme background
var themeBackgroundPanel = new StatusBarAdvPanel
{
    Text = "Theme Background",
    ThemesEnabled = true,
    IgnoreThemeBackground = false,  // Use theme background
    Size = new Size(150, 24)
};

// Use custom background despite theme
var customBackgroundPanel = new StatusBarAdvPanel
{
    Text = "Custom Background",
    ThemesEnabled = true,
    IgnoreThemeBackground = true,  // Use custom background
    BackColor = Color.LightBlue,
    Size = new Size(150, 24)
};
```

**VB.NET:**
```vb
' Use theme background
Dim themeBackgroundPanel = New StatusBarAdvPanel With {
    .Text = "Theme Background",
    .ThemesEnabled = True,
    .IgnoreThemeBackground = False,  ' Use theme background
    .Size = New Size(150, 24)
}

' Use custom background despite theme
Dim customBackgroundPanel = New StatusBarAdvPanel With {
    .Text = "Custom Background",
    .ThemesEnabled = True,
    .IgnoreThemeBackground = True,  ' Use custom background
    .BackColor = Color.LightBlue,
    .Size = New Size(150, 24)
}
```

### Theme Configuration Example

**C#:**
```csharp
private void ConfigureThemedPanel()
{
    var themedPanel = new StatusBarAdvPanel
    {
        Text = "Application Theme",
        Size = new Size(160, 26),
        
        // Enable theming
        ThemesEnabled = true,
        
        // Use custom background color
        IgnoreThemeBackground = true,
        BackColor = Color.FromArgb(240, 245, 250),
        ForeColor = Color.FromArgb(40, 70, 110),
        
        // BorderStyle is automatically set when ThemesEnabled = true
        // but can be customized
        BorderStyle = BorderStyle.None,
        
        HAlign = HorzFlowAlign.Left,
        Alignment = HorizontalAlignment.Left
    };
    
    statusBarAdv1.Controls.Add(themedPanel);
}
```

**VB.NET:**
```vb
Private Sub ConfigureThemedPanel()
    Dim themedPanel = New StatusBarAdvPanel With {
        .Text = "Application Theme",
        .Size = New Size(160, 26),
        .ThemesEnabled = True,
        .IgnoreThemeBackground = True,
        .BackColor = Color.FromArgb(240, 245, 250),
        .ForeColor = Color.FromArgb(40, 70, 110),
        .BorderStyle = BorderStyle.None,
        .HAlign = HorzFlowAlign.Left,
        .Alignment = HorizontalAlignment.Left
    }
    
    statusBarAdv1.Controls.Add(themedPanel)
End Sub
```

## ToolTip Configuration

Add tooltips to panels to provide additional information.

### ToolTip Property

Sets the tooltip text displayed when the mouse hovers over the panel.

**Property:**
- **Type:** `string`
- **Effect:** Displays tooltip on mouse hover

**C#:**
```csharp
var tooltipPanel = new StatusBarAdvPanel
{
    Text = "Status",
    ToolTip = "Current application status",
    Size = new Size(100, 24),
    BackgroundColor = new BrushInfo(Color.LightBlue)
};

// Date panel with tooltip
var datePanel = new StatusBarAdvPanel
{
    PanelType = StatusBarAdvPanelType.ShortDate,
    ToolTip = "Current date (click to open calendar)",
    Size = new Size(100, 24),
    BackgroundColor = new BrushInfo(Color.LightGreen)
};

// Time panel with tooltip
var timePanel = new StatusBarAdvPanel
{
    PanelType = StatusBarAdvPanelType.ShortTime,
    ToolTip = "Current system time",
    Size = new Size(80, 24),
    BackgroundColor = new BrushInfo(Color.LightYellow)
};
```

**VB.NET:**
```vb
Dim tooltipPanel = New StatusBarAdvPanel With {
    .Text = "Status",
    .ToolTip = "Current application status",
    .Size = New Size(100, 24),
    .BackgroundColor = New BrushInfo(Color.LightBlue)
}

' Date panel with tooltip
Dim datePanel = New StatusBarAdvPanel With {
    .PanelType = StatusBarAdvPanelType.ShortDate,
    .ToolTip = "Current date (click to open calendar)",
    .Size = New Size(100, 24),
    .BackgroundColor = New BrushInfo(Color.LightGreen)
}

' Time panel with tooltip
Dim timePanel = New StatusBarAdvPanel With {
    .PanelType = StatusBarAdvPanelType.ShortTime,
    .ToolTip = "Current system time",
    .Size = New Size(80, 24),
    .BackgroundColor = New BrushInfo(Color.LightYellow)
}
```

### Dynamic Tooltip Updates

**C#:**
```csharp
// Update tooltip based on status
private void UpdateStatusWithTooltip(string status, string details)
{
    statusPanel.Text = status;
    statusPanel.ToolTip = $"{status}\n\nDetails: {details}";
}

// Example usage
UpdateStatusWithTooltip("Processing", "Processing 150 of 500 records...");
UpdateStatusWithTooltip("Complete", "All 500 records processed successfully");
```

**VB.NET:**
```vb
' Update tooltip based on status
Private Sub UpdateStatusWithTooltip(status As String, details As String)
    statusPanel.Text = status
    statusPanel.ToolTip = $"{status}{vbCrLf}{vbCrLf}Details: {details}"
End Sub

' Example usage
UpdateStatusWithTooltip("Processing", "Processing 150 of 500 records...")
UpdateStatusWithTooltip("Complete", "All 500 records processed successfully")
```

## Property Change Events

StatusBarAdvPanel provides 18 property change events for monitoring panel modifications.

### Event List

| Event | Triggers When | EventArgs Type |
|-------|---------------|----------------|
| **AlignChanged** | Alignment property changes | EventArgs |
| **AnimationDelayChanged** | AnimationDelay property changes | EventArgs |
| **AnimationDirectionChanged** | AnimationDirection property changes | EventArgs |
| **AnimationSpeedChanged** | AnimationSpeed property changes | EventArgs |
| **AnimationStyleChanged** | AnimationStyle property changes | EventArgs |
| **BorderSidesChanged** | BorderSides property changes | EventArgs |
| **BorderColorChanged** | BorderColor property changes | EventArgs |
| **BorderSingleChanged** | BorderSingle property changes | EventArgs |
| **BorderStyleChanged** | BorderStyle property changes | EventArgs |
| **Border3DStyleChanged** | Border3DStyle property changes | EventArgs |
| **ConstraintsChanged** | Constraints list changes | EventArgs |
| **GradientBackgroundChanged** | GradientBackground property changes | EventArgs |
| **GradientColorsChanged** | GradientColors property changes | EventArgs |
| **IconChanged** | Icon property changes | EventArgs |
| **IsMarqueeChanged** | IsMarquee property changes | EventArgs |
| **PreferredSizeChanged** | PreferredSize property changes | EventArgs |
| **ThemeChanged** | ThemesEnabled property changes | EventArgs |
| **TypeChanged** | PanelType property changes | EventArgs |

### Alignment Event

**AlignChanged** - Fires when the Alignment property changes.

**C#:**
```csharp
// Subscribe to event
statusBarAdvPanel1.AlignChanged += StatusBarAdvPanel1_AlignChanged;

// Event handler
private void StatusBarAdvPanel1_AlignChanged(object sender, EventArgs e)
{
    var panel = sender as StatusBarAdvPanel;
    Console.WriteLine($"Alignment changed to: {panel.Alignment}");
}
```

**VB.NET:**
```vb
' Subscribe to event
AddHandler statusBarAdvPanel1.AlignChanged, AddressOf StatusBarAdvPanel1_AlignChanged

' Event handler
Private Sub StatusBarAdvPanel1_AlignChanged(sender As Object, e As EventArgs)
    Dim panel = TryCast(sender, StatusBarAdvPanel)
    Console.WriteLine($"Alignment changed to: {panel.Alignment}")
End Sub
```

### Animation Events

**AnimationDelayChanged, AnimationDirectionChanged, AnimationSpeedChanged, AnimationStyleChanged**

**C#:**
```csharp
// Subscribe to animation events
statusBarAdvPanel1.AnimationDelayChanged += (s, e) =>
{
    var panel = s as StatusBarAdvPanel;
    Console.WriteLine($"Animation delay changed to: {panel.AnimationDelay}");
};

statusBarAdvPanel1.AnimationDirectionChanged += (s, e) =>
{
    var panel = s as StatusBarAdvPanel;
    Console.WriteLine($"Animation direction changed to: {panel.AnimationDirection}");
};

statusBarAdvPanel1.AnimationSpeedChanged += (s, e) =>
{
    var panel = s as StatusBarAdvPanel;
    Console.WriteLine($"Animation speed changed to: {panel.AnimationSpeed}");
};

statusBarAdvPanel1.AnimationStyleChanged += (s, e) =>
{
    var panel = s as StatusBarAdvPanel;
    Console.WriteLine($"Animation style changed to: {panel.AnimationStyle}");
};
```

**VB.NET:**
```vb
' Subscribe to animation events
AddHandler statusBarAdvPanel1.AnimationDelayChanged, Sub(s, e)
    Dim panel = TryCast(s, StatusBarAdvPanel)
    Console.WriteLine($"Animation delay changed to: {panel.AnimationDelay}")
End Sub

AddHandler statusBarAdvPanel1.AnimationDirectionChanged, Sub(s, e)
    Dim panel = TryCast(s, StatusBarAdvPanel)
    Console.WriteLine($"Animation direction changed to: {panel.AnimationDirection}")
End Sub

AddHandler statusBarAdvPanel1.AnimationSpeedChanged, Sub(s, e)
    Dim panel = TryCast(s, StatusBarAdvPanel)
    Console.WriteLine($"Animation speed changed to: {panel.AnimationSpeed}")
End Sub

AddHandler statusBarAdvPanel1.AnimationStyleChanged, Sub(s, e)
    Dim panel = TryCast(s, StatusBarAdvPanel)
    Console.WriteLine($"Animation style changed to: {panel.AnimationStyle}")
End Sub
```

### Border Events

**BorderSidesChanged, BorderColorChanged, BorderSingleChanged, BorderStyleChanged, Border3DStyleChanged**

**C#:**
```csharp
// BorderSidesChanged
statusBarAdvPanel1.BorderSidesChanged += (s, e) =>
{
    var panel = s as StatusBarAdvPanel;
    Console.WriteLine($"Border sides changed to: {panel.BorderSides}");
};

// BorderColorChanged
statusBarAdvPanel1.BorderColorChanged += (s, e) =>
{
    var panel = s as StatusBarAdvPanel;
    Console.WriteLine($"Border color changed to: {panel.BorderColor.Name}");
};

// BorderSingleChanged
statusBarAdvPanel1.BorderSingleChanged += (s, e) =>
{
    var panel = s as StatusBarAdvPanel;
    Console.WriteLine($"Border single changed to: {panel.BorderSingle}");
};

// BorderStyleChanged
statusBarAdvPanel1.BorderStyleChanged += (s, e) =>
{
    var panel = s as StatusBarAdvPanel;
    Console.WriteLine($"Border style changed to: {panel.BorderStyle}");
};

// Border3DStyleChanged
statusBarAdvPanel1.Border3DStyleChanged += (s, e) =>
{
    var panel = s as StatusBarAdvPanel;
    Console.WriteLine($"Border 3D style changed to: {panel.Border3DStyle}");
};
```

### Gradient Events

**GradientBackgroundChanged, GradientColorsChanged**

**C#:**
```csharp
// GradientBackgroundChanged
statusBarAdvPanel1.GradientBackgroundChanged += (s, e) =>
{
    var panel = s as StatusBarAdvPanel;
    bool hasGradient = panel.BackgroundColor.Style == BrushStyle.Gradient;
    Console.WriteLine($"Gradient background changed. Has gradient: {hasGradient}");
};

// GradientColorsChanged
statusBarAdvPanel1.GradientColorsChanged += (s, e) =>
{
    var panel = s as StatusBarAdvPanel;
    int colorCount = panel.BackgroundColor.GradientColors?.Length ?? 0;
    Console.WriteLine($"Gradient colors changed. Color count: {colorCount}");
};
```

**VB.NET:**
```vb
' GradientBackgroundChanged
AddHandler statusBarAdvPanel1.GradientBackgroundChanged, Sub(s, e)
    Dim panel = TryCast(s, StatusBarAdvPanel)
    Dim hasGradient = panel.BackgroundColor.Style = BrushStyle.Gradient
    Console.WriteLine($"Gradient background changed. Has gradient: {hasGradient}")
End Sub

' GradientColorsChanged
AddHandler statusBarAdvPanel1.GradientColorsChanged, Sub(s, e)
    Dim panel = TryCast(s, StatusBarAdvPanel)
    Dim colorCount = If(panel.BackgroundColor.GradientColors?.Length, 0)
    Console.WriteLine($"Gradient colors changed. Color count: {colorCount}")
End Sub
```

### Miscellaneous Events

**IconChanged, IsMarqueeChanged, PreferredSizeChanged, ThemeChanged, TypeChanged**

**C#:**
```csharp
// IconChanged
statusBarAdvPanel1.IconChanged += (s, e) =>
{
    Console.WriteLine("Icon property changed");
};

// IsMarqueeChanged
statusBarAdvPanel1.IsMarqueeChanged += (s, e) =>
{
    var panel = s as StatusBarAdvPanel;
    Console.WriteLine($"IsMarquee changed to: {panel.IsMarquee}");
};

// PreferredSizeChanged
statusBarAdvPanel1.PreferredSizeChanged += (s, e) =>
{
    var panel = s as StatusBarAdvPanel;
    Console.WriteLine($"Preferred size changed to: {panel.PreferredSize}");
};

// ThemeChanged
statusBarAdvPanel1.ThemeChanged += (s, e) =>
{
    var panel = s as StatusBarAdvPanel;
    Console.WriteLine($"ThemesEnabled changed to: {panel.ThemesEnabled}");
};

// TypeChanged
statusBarAdvPanel1.TypeChanged += (s, e) =>
{
    var panel = s as StatusBarAdvPanel;
    Console.WriteLine($"Panel type changed to: {panel.PanelType}");
};
```

**VB.NET:**
```vb
' IconChanged
AddHandler statusBarAdvPanel1.IconChanged, Sub(s, e)
    Console.WriteLine("Icon property changed")
End Sub

' IsMarqueeChanged
AddHandler statusBarAdvPanel1.IsMarqueeChanged, Sub(s, e)
    Dim panel = TryCast(s, StatusBarAdvPanel)
    Console.WriteLine($"IsMarquee changed to: {panel.IsMarquee}")
End Sub

' PreferredSizeChanged
AddHandler statusBarAdvPanel1.PreferredSizeChanged, Sub(s, e)
    Dim panel = TryCast(s, StatusBarAdvPanel)
    Console.WriteLine($"Preferred size changed to: {panel.PreferredSize}")
End Sub

' ThemeChanged
AddHandler statusBarAdvPanel1.ThemeChanged, Sub(s, e)
    Dim panel = TryCast(s, StatusBarAdvPanel)
    Console.WriteLine($"ThemesEnabled changed to: {panel.ThemesEnabled}")
End Sub

' TypeChanged
AddHandler statusBarAdvPanel1.TypeChanged, Sub(s, e)
    Dim panel = TryCast(s, StatusBarAdvPanel)
    Console.WriteLine($"Panel type changed to: {panel.PanelType}")
End Sub
```

## Complete Event Monitoring Example

**C#:**
```csharp
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Drawing;
using System;
using System.Drawing;
using System.Windows.Forms;

public class EventMonitoringForm : Form
{
    private StatusBarAdv statusBarAdv1;
    private StatusBarAdvPanel monitoredPanel;
    private ListBox eventLog;
    private Button btnChangeAlignment;
    private Button btnChangeType;
    private Button btnToggleMarquee;
    private Button btnChangeBorder;
    private Button btnChangeGradient;
    
    public EventMonitoringForm()
    {
        InitializeComponent();
        SetupMonitoredPanel();
        SubscribeToEvents();
        SetupControls();
    }
    
    private void SetupMonitoredPanel()
    {
        statusBarAdv1 = new StatusBarAdv
        {
            Dock = DockStyle.Bottom,
            Height = 35,
            BackgroundColor = new BrushInfo(Color.WhiteSmoke)
        };
        
        monitoredPanel = new StatusBarAdvPanel
        {
            Text = "Monitoring events...",
            Size = new Size(600, 30),
            BackgroundColor = new BrushInfo(
                GradientStyle.Horizontal,
                Color.AliceBlue,
                Color.LightSkyBlue
            ),
            BorderStyle = BorderStyle.FixedSingle,
            BorderColor = Color.Navy,
            HAlign = HorzFlowAlign.Left,
            Alignment = HorizontalAlignment.Left
        };
        
        statusBarAdv1.Controls.Add(monitoredPanel);
        this.Controls.Add(statusBarAdv1);
    }
    
    private void SubscribeToEvents()
    {
        // Alignment event
        monitoredPanel.AlignChanged += (s, e) =>
        {
            var panel = s as StatusBarAdvPanel;
            LogEvent($"AlignChanged: {panel.Alignment}");
        };
        
        // Animation events
        monitoredPanel.AnimationDelayChanged += (s, e) =>
        {
            var panel = s as StatusBarAdvPanel;
            LogEvent($"AnimationDelayChanged: {panel.AnimationDelay}");
        };
        
        monitoredPanel.AnimationDirectionChanged += (s, e) =>
        {
            var panel = s as StatusBarAdvPanel;
            LogEvent($"AnimationDirectionChanged: {panel.AnimationDirection}");
        };
        
        monitoredPanel.AnimationSpeedChanged += (s, e) =>
        {
            var panel = s as StatusBarAdvPanel;
            LogEvent($"AnimationSpeedChanged: {panel.AnimationSpeed}");
        };
        
        monitoredPanel.AnimationStyleChanged += (s, e) =>
        {
            var panel = s as StatusBarAdvPanel;
            LogEvent($"AnimationStyleChanged: {panel.AnimationStyle}");
        };
        
        // Border events
        monitoredPanel.BorderSidesChanged += (s, e) =>
        {
            var panel = s as StatusBarAdvPanel;
            LogEvent($"BorderSidesChanged: {panel.BorderSides}");
        };
        
        monitoredPanel.BorderColorChanged += (s, e) =>
        {
            var panel = s as StatusBarAdvPanel;
            LogEvent($"BorderColorChanged: {panel.BorderColor.Name}");
        };
        
        monitoredPanel.BorderSingleChanged += (s, e) =>
        {
            var panel = s as StatusBarAdvPanel;
            LogEvent($"BorderSingleChanged: {panel.BorderSingle}");
        };
        
        monitoredPanel.BorderStyleChanged += (s, e) =>
        {
            var panel = s as StatusBarAdvPanel;
            LogEvent($"BorderStyleChanged: {panel.BorderStyle}");
        };
        
        monitoredPanel.Border3DStyleChanged += (s, e) =>
        {
            var panel = s as StatusBarAdvPanel;
            LogEvent($"Border3DStyleChanged: {panel.Border3DStyle}");
        };
        
        // Gradient events
        monitoredPanel.GradientBackgroundChanged += (s, e) =>
        {
            var panel = s as StatusBarAdvPanel;
            bool hasGradient = panel.BackgroundColor.Style == BrushStyle.Gradient;
            LogEvent($"GradientBackgroundChanged: HasGradient={hasGradient}");
        };
        
        monitoredPanel.GradientColorsChanged += (s, e) =>
        {
            var panel = s as StatusBarAdvPanel;
            int count = panel.BackgroundColor.GradientColors?.Length ?? 0;
            LogEvent($"GradientColorsChanged: ColorCount={count}");
        };
        
        // Miscellaneous events
        monitoredPanel.IconChanged += (s, e) =>
        {
            LogEvent("IconChanged");
        };
        
        monitoredPanel.IsMarqueeChanged += (s, e) =>
        {
            var panel = s as StatusBarAdvPanel;
            LogEvent($"IsMarqueeChanged: {panel.IsMarquee}");
        };
        
        monitoredPanel.PreferredSizeChanged += (s, e) =>
        {
            var panel = s as StatusBarAdvPanel;
            LogEvent($"PreferredSizeChanged: {panel.PreferredSize}");
        };
        
        monitoredPanel.ThemeChanged += (s, e) =>
        {
            var panel = s as StatusBarAdvPanel;
            LogEvent($"ThemeChanged: ThemesEnabled={panel.ThemesEnabled}");
        };
        
        monitoredPanel.TypeChanged += (s, e) =>
        {
            var panel = s as StatusBarAdvPanel;
            LogEvent($"TypeChanged: {panel.PanelType}");
        };
    }
    
    private void SetupControls()
    {
        // Event log
        eventLog = new ListBox
        {
            Location = new Point(10, 10),
            Size = new Size(660, 250),
            Font = new Font("Consolas", 9)
        };
        this.Controls.Add(eventLog);
        
        // Buttons
        int yPos = 270;
        
        btnChangeAlignment = new Button
        {
            Text = "Change Alignment",
            Location = new Point(10, yPos),
            Size = new Size(130, 30)
        };
        btnChangeAlignment.Click += (s, e) =>
        {
            monitoredPanel.Alignment = monitoredPanel.Alignment == HorizontalAlignment.Left 
                ? HorizontalAlignment.Center 
                : monitoredPanel.Alignment == HorizontalAlignment.Center
                    ? HorizontalAlignment.Right
                    : HorizontalAlignment.Left;
        };
        
        btnChangeType = new Button
        {
            Text = "Change Type",
            Location = new Point(150, yPos),
            Size = new Size(130, 30)
        };
        btnChangeType.Click += (s, e) =>
        {
            monitoredPanel.PanelType = monitoredPanel.PanelType == StatusBarAdvPanelType.Custom
                ? StatusBarAdvPanelType.ShortDate
                : StatusBarAdvPanelType.Custom;
        };
        
        btnToggleMarquee = new Button
        {
            Text = "Toggle Marquee",
            Location = new Point(290, yPos),
            Size = new Size(130, 30)
        };
        btnToggleMarquee.Click += (s, e) =>
        {
            monitoredPanel.IsMarquee = !monitoredPanel.IsMarquee;
            if (monitoredPanel.IsMarquee)
            {
                monitoredPanel.StartAnimation();
            }
        };
        
        btnChangeBorder = new Button
        {
            Text = "Change Border",
            Location = new Point(430, yPos),
            Size = new Size(130, 30)
        };
        btnChangeBorder.Click += (s, e) =>
        {
            if (monitoredPanel.BorderStyle == BorderStyle.FixedSingle)
            {
                monitoredPanel.BorderStyle = BorderStyle.Fixed3D;
                monitoredPanel.Border3DStyle = Border3DStyle.Raised;
            }
            else if (monitoredPanel.BorderStyle == BorderStyle.Fixed3D)
            {
                monitoredPanel.BorderStyle = BorderStyle.None;
            }
            else
            {
                monitoredPanel.BorderStyle = BorderStyle.FixedSingle;
                monitoredPanel.BorderColor = Color.DarkBlue;
            }
        };
        
        btnChangeGradient = new Button
        {
            Text = "Change Gradient",
            Location = new Point(570, yPos),
            Size = new Size(100, 30)
        };
        btnChangeGradient.Click += (s, e) =>
        {
            monitoredPanel.BackgroundColor = new BrushInfo(
                GradientStyle.Vertical,
                Color.LightGreen,
                Color.PaleGreen
            );
        };
        
        this.Controls.Add(btnChangeAlignment);
        this.Controls.Add(btnChangeType);
        this.Controls.Add(btnToggleMarquee);
        this.Controls.Add(btnChangeBorder);
        this.Controls.Add(btnChangeGradient);
    }
    
    private void LogEvent(string message)
    {
        string timestamp = DateTime.Now.ToString("HH:mm:ss.fff");
        eventLog.Items.Insert(0, $"[{timestamp}] {message}");
        
        // Keep only last 100 entries
        while (eventLog.Items.Count > 100)
        {
            eventLog.Items.RemoveAt(eventLog.Items.Count - 1);
        }
    }
    
    private void InitializeComponent()
    {
        this.Text = "Event Monitoring Demo";
        this.Size = new Size(700, 400);
        this.FormClosing += (s, e) =>
        {
            if (monitoredPanel.IsMarquee)
            {
                monitoredPanel.StopAnimation();
            }
        };
    }
}
```

## Event Handling Patterns

### Pattern 1: Track Appearance Changes

**C#:**
```csharp
private void MonitorAppearanceChanges()
{
    // Subscribe to all appearance-related events
    panel.GradientBackgroundChanged += (s, e) => 
        LogChange("Gradient background modified");
    
    panel.GradientColorsChanged += (s, e) => 
        LogChange("Gradient colors modified");
    
    panel.BorderStyleChanged += (s, e) => 
        LogChange("Border style modified");
    
    panel.BorderColorChanged += (s, e) => 
        LogChange("Border color modified");
}

private void LogChange(string change)
{
    Console.WriteLine($"[{DateTime.Now:HH:mm:ss}] {change}");
    // Optionally update UI or log to file
}
```

### Pattern 2: Animation State Tracking

**C#:**
```csharp
private bool isAnimationConfigured = false;

private void TrackAnimationConfiguration()
{
    panel.IsMarqueeChanged += (s, e) =>
    {
        isAnimationConfigured = panel.IsMarquee;
        if (isAnimationConfigured)
        {
            panel.StartAnimation();
        }
    };
    
    panel.AnimationSpeedChanged += (s, e) =>
    {
        if (isAnimationConfigured)
        {
            panel.StopAnimation();
            panel.StartAnimation();
        }
    };
    
    panel.AnimationDirectionChanged += (s, e) =>
    {
        if (isAnimationConfigured)
        {
            panel.StopAnimation();
            panel.StartAnimation();
        }
    };
}
```

### Pattern 3: Synchronize Multiple Panels

**C#:**
```csharp
private void SynchronizePanelAppearance()
{
    // When one panel's gradient changes, update all panels
    panel1.GradientBackgroundChanged += (s, e) =>
    {
        var sourcePanel = s as StatusBarAdvPanel;
        panel2.BackgroundColor = sourcePanel.BackgroundColor;
        panel3.BackgroundColor = sourcePanel.BackgroundColor;
    };
    
    // Synchronize border styles
    panel1.BorderStyleChanged += (s, e) =>
    {
        var sourcePanel = s as StatusBarAdvPanel;
        panel2.BorderStyle = sourcePanel.BorderStyle;
        panel3.BorderStyle = sourcePanel.BorderStyle;
    };
}
```

## Next Steps

You have now completed all StatusBarAdvPanel documentation! Review:
- **[Getting Started](getting-started.md)** - Setup and basic configuration
- **[Panel Types and Behavior](panel-types-and-behavior.md)** - Different panel types
- **[Appearance and Styling](appearance-styling.md)** - Visual customization
- **[Text and Marquee](text-and-marquee.md)** - Animated text
- **[Alignment and Borders](alignment-and-borders.md)** - Positioning and borders

For related components:
- **[Implementing Status Bars (StatusBarAdv)](../../implementing-status-bars/)** - Parent control for hosting panels
