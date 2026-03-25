---
name: syncfusion-winforms-statusbaradv-panel
description: Guide for implementing Syncfusion StatusBarAdvPanel control in Windows Forms applications. Use when creating custom status bar panels with marquee text animation, key state indicators, date time displays, or culture information panels. Covers panel types, appearance styling, gradients, marquee animation, alignment, borders, themes, and event handling for enhanced status bar panel displays.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Status Bar Panels (StatusBarAdvPanel)

This skill guides the implementation of the Syncfusion StatusBarAdvPanel control in Windows Forms applications. StatusBarAdvPanel is a panel-derived class that displays status bar information with enhanced appearance, marquee animation, and can be used both within StatusBarAdv controls and standalone on forms.

## When to Use This Skill

Use this skill when you need to:
- Create individual status bar panels with custom content and styling
- Display key state indicators (NumLock, CapsLock, ScrollLock, Insert key)
- Show date/time information in various formats (short date, long date, short time, long time)
- Display culture information (CurrentCulture) in status bars
- Implement marquee text animation with scrolling, sliding, or alternating effects
- Apply gradient backgrounds or patterns to status bar panels
- Configure panel alignment within StatusBarAdv or flow layouts
- Add custom borders (2D or 3D) to individual panels
- Handle property change events for dynamic panel updates
- Create themed status bar panels matching application design

## Component Overview

**StatusBarAdvPanel** is a versatile panel control designed for status bars with:
- **Panel Types**: Predefined types for key states, date/time, culture, or custom text
- **Marquee Animation**: Scrolling text with customizable speed, direction, and style
- **Enhanced Backgrounds**: Gradient, pattern, and solid color options using BrushInfo
- **Icon Support**: Display icons alongside panel text
- **Alignment Options**: Content alignment (Left, Right, Center) and flow alignment (including Justify)
- **Border Customization**: 2D and 3D border styles with custom colors
- **Auto-Sizing**: Automatic resizing based on content (SizeToContent)
- **Theme Support**: ThemesEnabled and IgnoreThemeBackground properties
- **ToolTip Support**: Add tooltips to provide additional information
- **18 Property Events**: Comprehensive event handling for property changes

**Assembly:** Syncfusion.Tools.Windows  
**Dependencies:** Syncfusion.Shared.Base, Syncfusion.Tools.Windows  
**Namespace:** `Syncfusion.Windows.Forms.Tools`

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

**When to read:** First time implementing StatusBarAdvPanel, setting up assemblies, or creating basic panels.

- Assembly requirements and installation
- Designer-based setup with drag-and-drop
- Programmatic creation and initialization
- Adding panels to StatusBarAdv or forms
- Basic configuration (Location, Size, BackgroundColor)
- PanelType and BorderColor setup
- Troubleshooting common setup issues

### Panel Types and Behavior
📄 **Read:** [references/panel-types-and-behavior.md](references/panel-types-and-behavior.md)

**When to read:** Configuring panel types, displaying key states or date/time, or customizing panel sizing.

- PanelType property options (key states, date/time, culture, custom)
- Key state indicators (NumLock, CapsLock, ScrollLock, InsertKey)
- Date/time formats (ShortDate, LongDate, ShortTime, LongTime)
- Custom text panels with Text property
- GetText() method for key state text retrieval
- SizeToContent for automatic panel resizing
- PreferredSize property configuration

### Appearance and Styling
📄 **Read:** [references/appearance-styling.md](references/appearance-styling.md)

**When to read:** Customizing panel appearance, applying gradients, setting background colors, or adding icons.

- BackgroundColor property with BrushInfo
- Gradient styles (6 types: ForwardDiagonal, BackwardDiagonal, Horizontal, Vertical, PathRectangle, PathEllipse)
- Pattern styles (Cross, DiagonalCross, DottedGrid, LargeGrid, Wave)
- GradientBackground and GradientColors configuration
- BackColor and ForeColor customization
- Icon property for panel icons
- Complete styling examples

### Text and Marquee Animation
📄 **Read:** [references/text-and-marquee.md](references/text-and-marquee.md)

**When to read:** Implementing marquee text animation, configuring scrolling effects, or controlling animation behavior.

- Text property configuration
- IsMarquee property for enabling animation
- AnimationDelay for timing control
- AnimationDirection (Left/Right)
- AnimationSpeed customization
- AnimationStyle (Scroll, Slide, Alternate)
- StartAnimation() and StopAnimation() methods
- Complete marquee examples

### Alignment and Borders
📄 **Read:** [references/alignment-and-borders.md](references/alignment-and-borders.md)

**When to read:** Configuring panel alignment, customizing borders, or setting up flow layout positioning.

- Alignment property (Left, Right, Center)
- HAlign property for flow layout (Left, Right, Center, Justify)
- BorderStyle property (None, FixedSingle, Fixed3D)
- Border3DStyle options (10 styles)
- BorderColor for 2D borders
- BorderSingle styles (Solid, Dashed, Dotted, Inset, Outset, None)
- BorderSides configuration (Left, Top, Right, Bottom, Middle, All)
- 2D vs 3D border examples

### Themes, ToolTips, and Events
📄 **Read:** [references/themes-tooltips-events.md](references/themes-tooltips-events.md)

**When to read:** Implementing themes, adding tooltips, or handling property change events.

- ThemesEnabled property
- IgnoreThemeBackground property
- ToolTip property configuration
- 18 property change events
- Event subscription examples
- Event handling patterns
- Complete event monitoring example

## Quick Start Example

### Basic StatusBarAdvPanel with Gradient Background

**C#:**
```csharp
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Drawing;
using System.Drawing;
using System.Windows.Forms;

public partial class MainForm : Form
{
    private StatusBarAdvPanel statusBarAdvPanel1;
    
    public MainForm()
    {
        InitializeComponent();
        SetupStatusBarPanel();
    }
    
    private void SetupStatusBarPanel()
    {
        // Create StatusBarAdvPanel
        statusBarAdvPanel1 = new StatusBarAdvPanel
        {
            // Set panel location and size
            Location = new Point(160, 184),
            Size = new Size(216, 48),
            
            // Configure panel type to show long date
            PanelType = StatusBarAdvPanelType.LongDate,
            
            // Apply gradient background
            BackgroundColor = new BrushInfo(
                GradientStyle.BackwardDiagonal, 
                Color.PaleVioletRed, 
                Color.PeachPuff
            ),
            
            // Set border and alignment
            BorderColor = Color.Black,
            HAlign = HorzFlowAlign.Left
        };
        
        // Add to form
        this.Controls.Add(statusBarAdvPanel1);
    }
}
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms.Tools
Imports Syncfusion.Drawing
Imports System.Drawing
Imports System.Windows.Forms

Public Partial Class MainForm
    Inherits Form
    
    Private statusBarAdvPanel1 As StatusBarAdvPanel
    
    Public Sub New()
        InitializeComponent()
        SetupStatusBarPanel()
    End Sub
    
    Private Sub SetupStatusBarPanel()
        ' Create StatusBarAdvPanel
        statusBarAdvPanel1 = New StatusBarAdvPanel With {
            .Location = New Point(160, 184),
            .Size = New Size(216, 48),
            .PanelType = StatusBarAdvPanelType.LongDate,
            .BackgroundColor = New BrushInfo(
                GradientStyle.BackwardDiagonal, 
                Color.PaleVioletRed, 
                Color.PeachPuff
            ),
            .BorderColor = Color.Black,
            .HAlign = HorzFlowAlign.Left
        }
        
        ' Add to form
        Me.Controls.Add(statusBarAdvPanel1)
    End Sub
End Class
```

## Common Patterns

### Pattern 1: Marquee Panel with Scrolling Text

Create an animated status panel with marquee scrolling text:

```csharp
// Configure marquee panel
var marqueePanel = new StatusBarAdvPanel
{
    Text = "Welcome to our application! Latest updates available...",
    IsMarquee = true,
    AnimationStyle = MarqueeStyle.Scroll,
    AnimationDirection = MarqueeDirection.Left,
    AnimationSpeed = 5,
    AnimationDelay = 10,
    Size = new Size(300, 30),
    BackColor = Color.LightBlue
};

// Start animation on form load
marqueePanel.StartAnimation();

// Stop animation when needed
// marqueePanel.StopAnimation();
```

### Pattern 2: Key State Indicator Panels

Create panels displaying keyboard key states:

```csharp
// NumLock indicator
var numLockPanel = new StatusBarAdvPanel
{
    PanelType = StatusBarAdvPanelType.NumLockState,
    Size = new Size(80, 24),
    HAlign = HorzFlowAlign.Right,
    BackgroundColor = new BrushInfo(Color.WhiteSmoke)
};

// CapsLock indicator
var capsLockPanel = new StatusBarAdvPanel
{
    PanelType = StatusBarAdvPanelType.CapsLockState,
    Size = new Size(80, 24),
    HAlign = HorzFlowAlign.Right,
    BackgroundColor = new BrushInfo(Color.WhiteSmoke)
};

// InsertKey indicator
var insertKeyPanel = new StatusBarAdvPanel
{
    PanelType = StatusBarAdvPanelType.InsertKeyState,
    Size = new Size(80, 24),
    HAlign = HorzFlowAlign.Right,
    BackgroundColor = new BrushInfo(Color.WhiteSmoke)
};
```

### Pattern 3: Custom Border with 3D Effect

Apply custom 3D border to a panel:

```csharp
// Create panel with 3D raised border
var borderedPanel = new StatusBarAdvPanel
{
    Text = "Status: Ready",
    Size = new Size(150, 30),
    BorderStyle = BorderStyle.Fixed3D,
    Border3DStyle = Border3DStyle.RaisedInner,
    BorderSides = Border3DSide.All,
    Alignment = HorizontalAlignment.Center,
    BackgroundColor = new BrushInfo(
        GradientStyle.Horizontal,
        Color.LavenderBlush,
        Color.RosyBrown
    )
};
```

## Key Properties

### StatusBarAdvPanel Properties

| Property | Type | Description |
|----------|------|-------------|
| **PanelType** | StatusBarAdvPanelType | Type of panel (Custom, NumLockState, CapsLockState, ScrollLockState, InsertKeyState, ShortDate, LongDate, ShortTime, LongTime, CurrentCulture) |
| **Text** | string | Text displayed in the panel (for Custom panel type) |
| **BackgroundColor** | BrushInfo | Background gradient, pattern, or solid color |
| **Icon** | Icon | Icon displayed in the panel |
| **Alignment** | HorizontalAlignment | Alignment of text and icon (Left, Right, Center) |
| **HAlign** | HorzFlowAlign | Horizontal alignment in flow layout (Left, Right, Center, Justify) |
| **IsMarquee** | bool | Enables marquee-style text animation |
| **AnimationStyle** | MarqueeStyle | Animation style (Scroll, Slide, Alternate) |
| **AnimationDirection** | MarqueeDirection | Animation direction (Left, Right) |
| **AnimationSpeed** | int | Speed of marquee animation |
| **AnimationDelay** | int | Delay before animation starts |
| **BorderStyle** | BorderStyle | Border style (None, FixedSingle, Fixed3D) |
| **Border3DStyle** | Border3DStyle | 3D border style (10 options) |
| **BorderColor** | Color | Color of 2D border |
| **BorderSingle** | ButtonBorderStyle | 2D border style (Solid, Dashed, Dotted, etc.) |
| **BorderSides** | Border3DSide | Border sides to display (All, Top, Bottom, Left, Right, Middle) |
| **SizeToContent** | bool | Auto-resize panel based on content |
| **PreferredSize** | Size | Preferred size in flow layout |
| **ThemesEnabled** | bool | Enables themed appearance |
| **IgnoreThemeBackground** | bool | Ignores theme background color |
| **ToolTip** | string | Tooltip text for the panel |

## Common Use Cases

1. **Application Status Display**: Show current application state with custom text and styling
2. **Keyboard State Indicators**: Display NumLock, CapsLock, ScrollLock, and Insert key states
3. **Date/Time Information**: Show current date and time in various formats
4. **Culture Information**: Display current culture settings
5. **Marquee Announcements**: Scrolling text for announcements or updates
6. **Progress Indicators**: Custom panels with text indicating processing status
7. **Document Information**: Show document-related information (line number, position, etc.)
8. **Themed Status Panels**: Match application theme with coordinated panel styling

## Additional Resources

For more information about StatusBarAdvPanel:
- Use within StatusBarAdv control for enhanced status bars
- Combine multiple panels with different types for comprehensive status displays
- Leverage events for dynamic panel updates based on application state
- Apply consistent theming across all panels for professional appearance

**Related Skills:**
- [Implementing Status Bars (StatusBarAdv)](../implementing-status-bars/) - Parent control for hosting panels
