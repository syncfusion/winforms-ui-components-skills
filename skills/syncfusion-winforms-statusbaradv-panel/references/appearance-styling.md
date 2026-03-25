# Appearance and Styling

## Table of Contents
- [Overview](#overview)
- [Background Settings](#background-settings)
- [Gradient Backgrounds](#gradient-backgrounds)
- [Pattern Styles](#pattern-styles)
- [Icon Settings](#icon-settings)
- [Complete Styling Examples](#complete-styling-examples)

This guide covers appearance customization options for StatusBarAdvPanel including backgrounds, gradients, patterns, and icons.

## Overview

## When to Read This

Read this guide when you need to:
- Customize panel background colors
- Apply gradient backgrounds to panels
- Use pattern styles for backgrounds
- Configure BackColor and ForeColor
- Add icons to panels
- Create visually appealing styled panels
- Match panel appearance to application themes

## Background Settings

The `BackgroundColor` property provides comprehensive background styling options using the BrushInfo class.

### BackgroundColor Property Options

The BackgroundColor property accepts a BrushInfo object with these configurable options:

| Property | Description | Options |
|----------|-------------|---------|
| **Style** | Brush style type | Solid, Pattern, Gradient |
| **BackColor** | Background color | Any System.Drawing.Color |
| **ForeColor** | Foreground color (for patterns/gradients) | Any System.Drawing.Color |
| **PatternStyle** | Pattern type (when Style = Pattern) | Various HatchStyle values |
| **GradientStyle** | Gradient direction (when Style = Gradient) | ForwardDiagonal, BackwardDiagonal, Horizontal, Vertical, PathRectangle, PathEllipse |
| **GradientColors** | Multi-color gradient array | Color array |

### Solid Color Background

Use solid colors for simple, clean panel backgrounds.

**C#:**
```csharp
// Using direct color
var solidPanel = new StatusBarAdvPanel
{
    BackgroundColor = new BrushInfo(Color.LightBlue),
    Text = "Solid Background",
    Size = new Size(150, 24)
};

// Using RGB values
var customColorPanel = new StatusBarAdvPanel
{
    BackgroundColor = new BrushInfo(Color.FromArgb(220, 235, 250)),
    Text = "Custom Color",
    Size = new Size(150, 24)
};
```

**VB.NET:**
```vb
' Using direct color
Dim solidPanel = New StatusBarAdvPanel With {
    .BackgroundColor = New BrushInfo(Color.LightBlue),
    .Text = "Solid Background",
    .Size = New Size(150, 24)
}

' Using RGB values
Dim customColorPanel = New StatusBarAdvPanel With {
    .BackgroundColor = New BrushInfo(Color.FromArgb(220, 235, 250)),
    .Text = "Custom Color",
    .Size = New Size(150, 24)
}
```

### BackColor and ForeColor

Set background and foreground colors separately.

**C#:**
```csharp
var coloredPanel = new StatusBarAdvPanel
{
    BackColor = Color.Navy,
    ForeColor = Color.White,  // Text color
    Text = "Dark Panel",
    Size = new Size(120, 24),
    Alignment = HorizontalAlignment.Center
};
```

**VB.NET:**
```vb
Dim coloredPanel = New StatusBarAdvPanel With {
    .BackColor = Color.Navy,
    .ForeColor = Color.White,  ' Text color
    .Text = "Dark Panel",
    .Size = New Size(120, 24),
    .Alignment = HorizontalAlignment.Center
}
```

## Gradient Backgrounds

Gradient backgrounds create smooth color transitions for professional-looking panels.

### Available Gradient Styles

**GradientStyle Options:**
1. **ForwardDiagonal** - Gradient from top-left to bottom-right
2. **BackwardDiagonal** - Gradient from top-right to bottom-left
3. **Horizontal** - Left to right gradient
4. **Vertical** - Top to bottom gradient
5. **PathRectangle** - Rectangular gradient from center
6. **PathEllipse** - Elliptical gradient from center

### Basic Gradient Configuration

**C#:**
```csharp
// Horizontal gradient
var horizontalGradientPanel = new StatusBarAdvPanel
{
    BackgroundColor = new BrushInfo(
        GradientStyle.Horizontal,
        Color.AliceBlue,      // Start color
        Color.SteelBlue       // End color
    ),
    Text = "Horizontal Gradient",
    Size = new Size(180, 24)
};

// Vertical gradient
var verticalGradientPanel = new StatusBarAdvPanel
{
    BackgroundColor = new BrushInfo(
        GradientStyle.Vertical,
        Color.LightGreen,
        Color.DarkSeaGreen
    ),
    Text = "Vertical Gradient",
    Size = new Size(180, 24)
};

// Forward diagonal gradient
var diagonalPanel = new StatusBarAdvPanel
{
    BackgroundColor = new BrushInfo(
        GradientStyle.ForwardDiagonal,
        Color.Lavender,
        Color.MediumPurple
    ),
    Text = "Diagonal Gradient",
    Size = new Size(180, 24)
};
```

**VB.NET:**
```vb
' Horizontal gradient
Dim horizontalGradientPanel = New StatusBarAdvPanel With {
    .BackgroundColor = New BrushInfo(
        GradientStyle.Horizontal,
        Color.AliceBlue,      ' Start color
        Color.SteelBlue       ' End color
    ),
    .Text = "Horizontal Gradient",
    .Size = New Size(180, 24)
}

' Vertical gradient
Dim verticalGradientPanel = New StatusBarAdvPanel With {
    .BackgroundColor = New BrushInfo(
        GradientStyle.Vertical,
        Color.LightGreen,
        Color.DarkSeaGreen
    ),
    .Text = "Vertical Gradient",
    .Size = New Size(180, 24)
}

' Forward diagonal gradient
Dim diagonalPanel = New StatusBarAdvPanel With {
    .BackgroundColor = New BrushInfo(
        GradientStyle.ForwardDiagonal,
        Color.Lavender,
        Color.MediumPurple
    ),
    .Text = "Diagonal Gradient",
    .Size = New Size(180, 24)
}
```

### PathRectangle and PathEllipse Gradients

These create radial gradients from the center outward.

**C#:**
```csharp
// PathRectangle gradient
var pathRectPanel = new StatusBarAdvPanel
{
    BackgroundColor = new BrushInfo(
        GradientStyle.PathRectangle,
        Color.NavajoWhite,
        Color.IndianRed
    ),
    Text = "Path Rectangle",
    Size = new Size(150, 24)
};

// PathEllipse gradient
var pathEllipsePanel = new StatusBarAdvPanel
{
    BackgroundColor = new BrushInfo(
        GradientStyle.PathEllipse,
        Color.LavenderBlush,
        Color.RosyBrown
    ),
    Text = "Path Ellipse",
    Size = new Size(150, 24)
};
```

**VB.NET:**
```vb
' PathRectangle gradient
Dim pathRectPanel = New StatusBarAdvPanel With {
    .BackgroundColor = New BrushInfo(
        GradientStyle.PathRectangle,
        Color.NavajoWhite,
        Color.IndianRed
    ),
    .Text = "Path Rectangle",
    .Size = New Size(150, 24)
}

' PathEllipse gradient
Dim pathEllipsePanel = New StatusBarAdvPanel With {
    .BackgroundColor = New BrushInfo(
        GradientStyle.PathEllipse,
        Color.LavenderBlush,
        Color.RosyBrown
    ),
    .Text = "Path Ellipse",
    .Size = New Size(150, 24)
}
```

### Multi-Color Gradients

Use the `GradientColors` property for gradients with more than two colors.

**C#:**
```csharp
// Create multi-color gradient
var multiColorPanel = new StatusBarAdvPanel
{
    Text = "Rainbow Gradient",
    Size = new Size(200, 24)
};

// Configure gradient with multiple colors
multiColorPanel.BackgroundColor = new BrushInfo
{
    Style = BrushStyle.Gradient,
    GradientStyle = GradientStyle.Horizontal,
    GradientColors = new Color[]
    {
        Color.Red,
        Color.Orange,
        Color.Yellow,
        Color.Green,
        Color.Blue
    }
};
```

**VB.NET:**
```vb
' Create multi-color gradient
Dim multiColorPanel = New StatusBarAdvPanel With {
    .Text = "Rainbow Gradient",
    .Size = New Size(200, 24)
}

' Configure gradient with multiple colors
multiColorPanel.BackgroundColor = New BrushInfo With {
    .Style = BrushStyle.Gradient,
    .GradientStyle = GradientStyle.Horizontal,
    .GradientColors = New Color() {
        Color.Red,
        Color.Orange,
        Color.Yellow,
        Color.Green,
        Color.Blue
    }
}
```

### GradientBackground Property

Boolean property indicating whether the background uses a gradient.

**C#:**
```csharp
// Check if gradient is enabled
if (panel.BackgroundColor.Style == BrushStyle.Gradient)
{
    Console.WriteLine("Panel uses gradient background");
}
```

## Pattern Styles

Pattern styles apply repeating patterns to panel backgrounds.

### Available Pattern Styles

Common HatchStyle patterns include:
- **Cross** - Horizontal and vertical lines
- **DiagonalCross** - Diagonal crossing lines
- **DottedGrid** - Grid of dots
- **LargeGrid** - Large grid pattern
- **Wave** - Wave pattern
- **Horizontal** - Horizontal lines
- **Vertical** - Vertical lines
- **DarkDownwardDiagonal** - Dark diagonal lines

### Applying Pattern Styles

**C#:**
```csharp
// Cross pattern
var crossPatternPanel = new StatusBarAdvPanel
{
    Text = "Cross Pattern",
    Size = new Size(150, 24)
};

crossPatternPanel.BackgroundColor = new BrushInfo
{
    Style = BrushStyle.Pattern,
    BackColor = Color.White,
    ForeColor = Color.LightGray,
    PatternStyle = System.Drawing.Drawing2D.HatchStyle.Cross
};

// Dotted grid pattern
var dottedPanel = new StatusBarAdvPanel
{
    Text = "Dotted Grid",
    Size = new Size(150, 24)
};

dottedPanel.BackgroundColor = new BrushInfo
{
    Style = BrushStyle.Pattern,
    BackColor = Color.White,
    ForeColor = Color.DarkGray,
    PatternStyle = System.Drawing.Drawing2D.HatchStyle.DottedGrid
};

// Wave pattern
var wavePanel = new StatusBarAdvPanel
{
    Text = "Wave Pattern",
    Size = new Size(150, 24)
};

wavePanel.BackgroundColor = new BrushInfo
{
    Style = BrushStyle.Pattern,
    BackColor = Color.LightBlue,
    ForeColor = Color.Navy,
    PatternStyle = System.Drawing.Drawing2D.HatchStyle.Wave
};
```

**VB.NET:**
```vb
' Cross pattern
Dim crossPatternPanel = New StatusBarAdvPanel With {
    .Text = "Cross Pattern",
    .Size = New Size(150, 24)
}

crossPatternPanel.BackgroundColor = New BrushInfo With {
    .Style = BrushStyle.Pattern,
    .BackColor = Color.White,
    .ForeColor = Color.LightGray,
    .PatternStyle = System.Drawing.Drawing2D.HatchStyle.Cross
}

' Dotted grid pattern
Dim dottedPanel = New StatusBarAdvPanel With {
    .Text = "Dotted Grid",
    .Size = New Size(150, 24)
}

dottedPanel.BackgroundColor = New BrushInfo With {
    .Style = BrushStyle.Pattern,
    .BackColor = Color.White,
    .ForeColor = Color.DarkGray,
    .PatternStyle = System.Drawing.Drawing2D.HatchStyle.DottedGrid
}

' Wave pattern
Dim wavePanel = New StatusBarAdvPanel With {
    .Text = "Wave Pattern",
    .Size = New Size(150, 24)
}

wavePanel.BackgroundColor = New BrushInfo With {
    .Style = BrushStyle.Pattern,
    .BackColor = Color.LightBlue,
    .ForeColor = Color.Navy,
    .PatternStyle = System.Drawing.Drawing2D.HatchStyle.Wave
}
```

## Icon Settings

Add icons to panels for visual indicators.

### Icon Property

The `Icon` property displays an icon alongside panel text.

**C#:**
```csharp
// Add icon from resources
var iconPanel = new StatusBarAdvPanel
{
    Text = "With Icon",
    Size = new Size(120, 24),
    BackgroundColor = new BrushInfo(Color.LightYellow),
    Alignment = HorizontalAlignment.Left
};

// Load icon from resources
iconPanel.Icon = Properties.Resources.StatusIcon;

// Or load from file
// iconPanel.Icon = new Icon("path/to/icon.ico");
```

**VB.NET:**
```vb
' Add icon from resources
Dim iconPanel = New StatusBarAdvPanel With {
    .Text = "With Icon",
    .Size = New Size(120, 24),
    .BackgroundColor = New BrushInfo(Color.LightYellow),
    .Alignment = HorizontalAlignment.Left
}

' Load icon from resources
iconPanel.Icon = My.Resources.StatusIcon

' Or load from file
' iconPanel.Icon = New Icon("path/to/icon.ico")
```

### Icon with Text Alignment

**C#:**
```csharp
private StatusBarAdvPanel CreateIconPanel(string text, Icon icon)
{
    return new StatusBarAdvPanel
    {
        Text = text,
        Icon = icon,
        Size = new Size(140, 24),
        Alignment = HorizontalAlignment.Left,  // Align text and icon left
        BackgroundColor = new BrushInfo(
            GradientStyle.Horizontal,
            Color.LightCyan,
            Color.PaleTurquoise
        )
    };
}

// Usage
var statusIcon = new Icon("status.ico");
var statusPanel = CreateIconPanel("Status: Ready", statusIcon);
```

## Complete Styling Examples

### Example 1: Professional Blue Gradient Panel

**C#:**
```csharp
private StatusBarAdvPanel CreateProfessionalPanel()
{
    return new StatusBarAdvPanel
    {
        PanelType = StatusBarAdvPanelType.Custom,
        Text = "Application Ready",
        Size = new Size(160, 26),
        BackgroundColor = new BrushInfo(
            GradientStyle.Horizontal,
            Color.FromArgb(229, 241, 251),  // Light blue
            Color.FromArgb(180, 215, 245)   // Darker blue
        ),
        ForeColor = Color.FromArgb(30, 57, 91),  // Dark blue text
        BorderStyle = BorderStyle.FixedSingle,
        BorderColor = Color.FromArgb(160, 190, 220),
        Alignment = HorizontalAlignment.Left,
        HAlign = HorzFlowAlign.Left
    };
}
```

### Example 2: Success Status Panel with Green Gradient

**C#:**
```csharp
private StatusBarAdvPanel CreateSuccessPanel()
{
    return new StatusBarAdvPanel
    {
        Text = "Operation Successful",
        Size = new Size(180, 26),
        BackgroundColor = new BrushInfo(
            GradientStyle.Vertical,
            Color.FromArgb(220, 248, 220),  // Light green
            Color.FromArgb(144, 238, 144)   // Medium green
        ),
        ForeColor = Color.DarkGreen,
        BorderStyle = BorderStyle.FixedSingle,
        BorderColor = Color.Green,
        Alignment = HorizontalAlignment.Center,
        HAlign = HorzFlowAlign.Left
    };
}
```

### Example 3: Error Panel with Red Gradient

**C#:**
```csharp
private StatusBarAdvPanel CreateErrorPanel()
{
    return new StatusBarAdvPanel
    {
        Text = "Error Occurred",
        Size = new Size(150, 26),
        BackgroundColor = new BrushInfo(
            GradientStyle.Horizontal,
            Color.FromArgb(255, 220, 220),  // Light red
            Color.FromArgb(255, 180, 180)   // Medium red
        ),
        ForeColor = Color.DarkRed,
        BorderStyle = BorderStyle.FixedSingle,
        BorderColor = Color.Red,
        Alignment = HorizontalAlignment.Center,
        HAlign = HorzFlowAlign.Left
    };
}
```

### Example 4: Warning Panel with Pattern

**C#:**
```csharp
private StatusBarAdvPanel CreateWarningPanel()
{
    var warningPanel = new StatusBarAdvPanel
    {
        Text = "Warning: Check Settings",
        Size = new Size(190, 26),
        ForeColor = Color.DarkOrange,
        BorderStyle = BorderStyle.FixedSingle,
        BorderColor = Color.Orange,
        Alignment = HorizontalAlignment.Left,
        HAlign = HorzFlowAlign.Left
    };
    
    warningPanel.BackgroundColor = new BrushInfo
    {
        Style = BrushStyle.Pattern,
        BackColor = Color.LightYellow,
        ForeColor = Color.FromArgb(255, 220, 100),
        PatternStyle = System.Drawing.Drawing2D.HatchStyle.DottedGrid
    };
    
    return warningPanel;
}
```

### Example 5: Complete Themed Status Bar

**C#:**
```csharp
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Drawing;
using System.Drawing;
using System.Windows.Forms;

public class StyledStatusBarForm : Form
{
    private StatusBarAdv statusBarAdv1;
    private StatusBarAdvPanel statusPanel;
    private StatusBarAdvPanel progressPanel;
    private StatusBarAdvPanel datePanel;
    
    public StyledStatusBarForm()
    {
        InitializeComponent();
        SetupStyledStatusBar();
    }
    
    private void SetupStyledStatusBar()
    {
        // Create StatusBarAdv with gradient
        statusBarAdv1 = new StatusBarAdv
        {
            Dock = DockStyle.Bottom,
            Height = 30
        };
        
        statusBarAdv1.BackgroundColor = new BrushInfo(
            GradientStyle.Horizontal,
            Color.FromArgb(240, 245, 250),
            Color.FromArgb(220, 230, 240)
        );
        
        // Status panel with blue gradient
        statusPanel = new StatusBarAdvPanel
        {
            Text = "Ready",
            Size = new Size(150, 26),
            BackgroundColor = new BrushInfo(
                GradientStyle.Vertical,
                Color.FromArgb(220, 235, 250),
                Color.FromArgb(200, 220, 245)
            ),
            ForeColor = Color.FromArgb(40, 70, 110),
            BorderStyle = BorderStyle.FixedSingle,
            BorderColor = Color.FromArgb(180, 200, 220),
            HAlign = HorzFlowAlign.Left,
            Alignment = HorizontalAlignment.Left
        };
        
        // Progress panel with green gradient
        progressPanel = new StatusBarAdvPanel
        {
            Text = "0%",
            Size = new Size(80, 26),
            BackgroundColor = new BrushInfo(
                GradientStyle.Vertical,
                Color.FromArgb(220, 248, 220),
                Color.FromArgb(180, 235, 180)
            ),
            ForeColor = Color.DarkGreen,
            BorderStyle = BorderStyle.FixedSingle,
            BorderColor = Color.Green,
            HAlign = HorzFlowAlign.Center,
            Alignment = HorizontalAlignment.Center
        };
        
        // Date panel with pattern
        datePanel = new StatusBarAdvPanel
        {
            PanelType = StatusBarAdvPanelType.ShortDate,
            Size = new Size(100, 26),
            ForeColor = Color.Navy,
            BorderStyle = BorderStyle.FixedSingle,
            BorderColor = Color.Gray,
            HAlign = HorzFlowAlign.Right,
            Alignment = HorizontalAlignment.Center
        };
        
        datePanel.BackgroundColor = new BrushInfo
        {
            Style = BrushStyle.Pattern,
            BackColor = Color.White,
            ForeColor = Color.FromArgb(230, 230, 250),
            PatternStyle = System.Drawing.Drawing2D.HatchStyle.DottedGrid
        };
        
        // Add panels
        statusBarAdv1.Controls.Add(statusPanel);
        statusBarAdv1.Controls.Add(progressPanel);
        statusBarAdv1.Controls.Add(datePanel);
        
        this.Controls.Add(statusBarAdv1);
    }
    
    // Update status with color coding
    public void UpdateStatus(string message, StatusType type)
    {
        statusPanel.Text = message;
        
        switch (type)
        {
            case StatusType.Success:
                statusPanel.BackgroundColor = new BrushInfo(
                    GradientStyle.Vertical,
                    Color.FromArgb(220, 248, 220),
                    Color.FromArgb(180, 235, 180)
                );
                statusPanel.ForeColor = Color.DarkGreen;
                break;
                
            case StatusType.Error:
                statusPanel.BackgroundColor = new BrushInfo(
                    GradientStyle.Vertical,
                    Color.FromArgb(255, 220, 220),
                    Color.FromArgb(255, 180, 180)
                );
                statusPanel.ForeColor = Color.DarkRed;
                break;
                
            case StatusType.Warning:
                statusPanel.BackgroundColor = new BrushInfo(
                    GradientStyle.Vertical,
                    Color.FromArgb(255, 245, 220),
                    Color.FromArgb(255, 230, 180)
                );
                statusPanel.ForeColor = Color.DarkOrange;
                break;
                
            default: // Normal
                statusPanel.BackgroundColor = new BrushInfo(
                    GradientStyle.Vertical,
                    Color.FromArgb(220, 235, 250),
                    Color.FromArgb(200, 220, 245)
                );
                statusPanel.ForeColor = Color.FromArgb(40, 70, 110);
                break;
        }
    }
    
    // Update progress
    public void UpdateProgress(int percentage)
    {
        progressPanel.Text = $"{percentage}%";
        
        // Change color based on progress
        if (percentage >= 100)
        {
            progressPanel.BackgroundColor = new BrushInfo(
                GradientStyle.Vertical,
                Color.FromArgb(200, 255, 200),
                Color.FromArgb(144, 238, 144)
            );
        }
        else if (percentage >= 50)
        {
            progressPanel.BackgroundColor = new BrushInfo(
                GradientStyle.Vertical,
                Color.FromArgb(255, 255, 200),
                Color.FromArgb(255, 255, 150)
            );
        }
    }
    
    private void InitializeComponent()
    {
        this.Text = "Styled Status Bar Demo";
        this.Size = new Size(700, 450);
    }
}

public enum StatusType
{
    Normal,
    Success,
    Warning,
    Error
}
```

## Next Steps

After customizing panel appearance, explore:
- **[Text and Marquee](text-and-marquee.md)** - Implement animated marquee text
- **[Alignment and Borders](alignment-and-borders.md)** - Configure panel alignment and borders
- **[Themes, ToolTips, and Events](themes-tooltips-events.md)** - Add themes and event handling
