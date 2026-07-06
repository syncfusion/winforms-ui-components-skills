---
name: syncfusion-winforms-status-bar
description: Guide for implementing Syncfusion StatusBarAdv control in Windows Forms applications. Use when creating application status displays, bottom panels, or status information panels with StatusBarAdvPanel items. Covers installation, panel configuration, appearance styling, gradients, borders, themes, alignment, and event handling for enhanced status bar displays.
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
---

# Implementing Status Bars (StatusBarAdv)

This skill guides the implementation of the Syncfusion StatusBarAdv control in Windows Forms applications. StatusBarAdv is an advanced status bar that displays panels with enhanced backgrounds, appearances, and supports Office2016 themes.

## When to Use This Skill

Use this skill when you need to:
- Display application status information at the bottom of forms
- Implement enhanced status bars with gradient backgrounds and styled panels
- Show date/time, culture information, or key states in status panels
- Create professional-looking status bars with Office2016 or Metro themes
- Add custom controls or panels to status bars
- Configure panel alignment and spacing in status displays
- Apply advanced border and background styling to status bars
- Handle status bar property change events

## Component Overview

**StatusBarAdv** is an advanced status bar control that extends the standard Windows Forms StatusBar with:
- **StatusBarAdvPanel Collection**: Add multiple panels with custom content and styling
- **Enhanced Backgrounds**: Gradient, pattern, and solid color options using BrushInfo
- **Office2016 Themes**: Colorful, White, Black, and DarkGray theme support
- **Border Customization**: 2D and 3D border styles with custom colors
- **Panel Alignment**: Center, Near, Far, and custom positioning options
- **Auto-Sizing**: Automatic height adjustment for panels
- **Metro Style**: Modern flat design option
- **Sizing Grip**: Resizable form indicator
- **Child Controls**: Add any WinForms control to the status bar

**Assembly:** Syncfusion.Tools.Windows  
**Dependencies:** Syncfusion.Shared.Base, Syncfusion.Tools.Windows  
**Namespace:** `Syncfusion.Windows.Forms.Tools`

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

**When to read:** First time implementing StatusBarAdv, setting up assemblies, or creating basic status bars.

- Assembly requirements and installation
- NuGet package installation
- Designer-based setup with toolbox
- Programmatic creation and initialization
- Adding StatusBarAdvPanel controls
- Basic configuration and docking
- Troubleshooting common setup issues

### Panels and Layout
📄 **Read:** [references/panels-and-layout.md](references/panels-and-layout.md)

**When to read:** Managing multiple panels, configuring spacing, customizing panel alignment, or positioning controls.

- StatusBarAdvPanel collection management
- Panel spacing configuration
- CustomLayoutBounds for panel sizing
- Alignment options (Center, Near, Far, ChildConstraints)
- SetHAlign method for horizontal alignment
- Panel positioning and sizing
- Adding child controls to StatusBarAdv
- Layout customization patterns

### Appearance and Styling
📄 **Read:** [references/appearance-styling.md](references/appearance-styling.md)

**When to read:** Customizing status bar appearance, applying gradients, setting colors, or adding sizing grip.

- BackgroundColor property with BrushInfo
- Gradient backgrounds (6 gradient styles)
- Pattern styles and solid colors
- GradientColors array configuration
- MetroColor and UseMetroColorAsBorder
- ForeColor customization
- SizingGrip display
- Complete styling examples

### Borders and Themes
📄 **Read:** [references/borders-and-themes.md](references/borders-and-themes.md)

**When to read:** Applying borders, using Office2016 themes, enabling Metro style, or configuring themed backgrounds.

- BorderStyle property (None, FixedSingle, Fixed3D)
- Border3DStyle options (10 styles)
- 2D border configuration (BorderColor, BorderSingle, BorderSides)
- Office2016 themes (Colorful, White, Black, DarkGray)
- Metro style application
- ThemesEnabled and IgnoreThemeBackground
- Complete themed examples

### Events and Behavior
📄 **Read:** [references/events-and-behavior.md](references/events-and-behavior.md)

**When to read:** Implementing auto-sizing, handling property change events, or responding to border/gradient changes.

- AutoSize and AutoSizeMode configuration
- AutoHeightControls for panel height
- GetPreferredSize and SetPreferredSize methods
- 9 property change events
- BorderSidesChanged, BorderColorChanged events
- GradientBackgroundChanged event
- ThemeChanged event
- Event handling patterns

## Quick Start Example

### Basic StatusBarAdv with Panels

**C#:**
```csharp
using Syncfusion.Windows.Forms.Tools;
using System.Drawing;
using System.Windows.Forms;

public partial class MainForm : Form
{
    private StatusBarAdv statusBarAdv1;
    private StatusBarAdvPanel culturePanel;
    private StatusBarAdvPanel datePanel;
    private StatusBarAdvPanel timePanel;
    
    public MainForm()
    {
        InitializeComponent();
        SetupStatusBar();
    }
    
    private void SetupStatusBar()
    {
        // Create StatusBarAdv
        statusBarAdv1 = new StatusBarAdv
        {
            Dock = DockStyle.Bottom,
            BackColor = Color.LightSteelBlue,
            BorderColor = Color.DarkSlateGray,
            BorderStyle = BorderStyle.FixedSingle,
            SizingGrip = true
        };
        
        // Create panels
        culturePanel = new StatusBarAdvPanel
        {
            PanelType = StatusBarAdvPanelType.CurrentCulture,
            Size = new Size(120, 27)
        };
        
        datePanel = new StatusBarAdvPanel
        {
            PanelType = StatusBarAdvPanelType.ShortDate,
            Size = new Size(100, 27)
        };
        
        timePanel = new StatusBarAdvPanel
        {
            PanelType = StatusBarAdvPanelType.ShortTime,
            Size = new Size(100, 27)
        };
        
        // Add panels to StatusBarAdv
        statusBarAdv1.Controls.Add(culturePanel);
        statusBarAdv1.Controls.Add(datePanel);
        statusBarAdv1.Controls.Add(timePanel);
        
        // Add to form
        this.Controls.Add(statusBarAdv1);
    }
}
```

**VB.NET:**
```vbnet
Imports Syncfusion.Windows.Forms.Tools
Imports System.Drawing
Imports System.Windows.Forms

Public Partial Class MainForm
    Inherits Form
    
    Private statusBarAdv1 As StatusBarAdv
    Private culturePanel As StatusBarAdvPanel
    Private datePanel As StatusBarAdvPanel
    Private timePanel As StatusBarAdvPanel
    
    Public Sub New()
        InitializeComponent()
        SetupStatusBar()
    End Sub
    
    Private Sub SetupStatusBar()
        ' Create StatusBarAdv
        statusBarAdv1 = New StatusBarAdv With {
            .Dock = DockStyle.Bottom,
            .BackColor = Color.LightSteelBlue,
            .BorderColor = Color.DarkSlateGray,
            .BorderStyle = BorderStyle.FixedSingle,
            .SizingGrip = True
        }
        
        ' Create panels
        culturePanel = New StatusBarAdvPanel With {
            .PanelType = StatusBarAdvPanelType.CurrentCulture,
            .Size = New Size(120, 27)
        }
        
        datePanel = New StatusBarAdvPanel With {
            .PanelType = StatusBarAdvPanelType.ShortDate,
            .Size = New Size(100, 27)
        }
        
        timePanel = New StatusBarAdvPanel With {
            .PanelType = StatusBarAdvPanelType.ShortTime,
            .Size = New Size(100, 27)
        }
        
        ' Add panels to StatusBarAdv
        statusBarAdv1.Controls.Add(culturePanel)
        statusBarAdv1.Controls.Add(datePanel)
        statusBarAdv1.Controls.Add(timePanel)
        
        ' Add to form
        Me.Controls.Add(statusBarAdv1)
    End Sub
End Class
```

## Common Patterns

### 1. Gradient Background with Metro Theme

Apply a modern gradient background with Metro styling:

```csharp
// Set gradient background (will be overridden if Metro style is applied)
statusBarAdv1.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.AliceBlue,
    Color.LightSteelBlue
);

// Apply Metro style (this overrides BackgroundColor)
statusBarAdv1.Style = StatusbarStyle.Metro;

// Set metro accent color
statusBarAdv1.MetroColor = ColorTranslator.FromHtml("#16a5dc");
statusBarAdv1.UseMetroColorAsBorder = true;
```
> **Note:** When `Style = StatusbarStyle.Metro` is set, any custom `BackgroundColor` gradients are ignored. Use `MetroColor` and `UseMetroColorAsBorder` for Metro styling instead.

### 2. Custom Panel Alignment

Align panels with custom positioning using ChildConstraints:

```csharp
// Enable custom alignment
statusBarAdv1.Alignment = FlowAlignment.ChildConstraints;

// Left-align first panel
statusBarAdv1.SetHAlign(panel1, HorzFlowAlign.Left);

// Right-align second panel
statusBarAdv1.SetHAlign(panel2, HorzFlowAlign.Right);

// Justify third panel to fill space
statusBarAdv1.SetHAlign(panel3, HorzFlowAlign.Justify);
```

### 3. Office2016 Theme with Auto-Sizing

Apply Office2016 theme with automatic height adjustment:

```csharp
// Apply Office2016 Colorful theme
statusBarAdv1.Style = StatusbarStyle.Office2016Colorful;

// Enable auto-sizing
statusBarAdv1.AutoSize = true;
statusBarAdv1.AutoSizeMode = AutoSizeMode.GrowAndShrink;

// Auto-adjust panel heights
statusBarAdv1.AutoHeightControls = true;
```

## Key Properties

### StatusBarAdv Properties

| Property | Type | Description |
|----------|------|-------------|
| `BackgroundColor` | BrushInfo | Background gradient, pattern, or solid color configuration |
| `BorderStyle` | BorderStyle | Border type: None, FixedSingle, or Fixed3D |
| `Border3DStyle` | Border3DStyle | 3D border style (10 options: Raised, Sunken, etc.) |
| `BorderColor` | Color | 2D border color (requires BorderStyle.FixedSingle) |
| `BorderSingle` | ButtonBorderStyle | 2D border style: Solid, Dashed, Dotted, etc. |
| `BorderSides` | Border3DSide | Border sides to display: Left, Top, Right, Bottom, All |
| `Style` | StatusbarStyle | Theme: Default, Metro, Office2016Colorful, Office2016White, Office2016Black, Office2016DarkGray |
| `Panels` | Collection | StatusBarAdvPanel controls in the StatusBarAdv |
| `Alignment` | FlowAlignment | Panel alignment: Center, Near, Far, ChildConstraints |
| `Spacing` | Size | Space between panels |
| `CustomLayoutBounds` | Rectangle | Custom rectangle for panel layout |
| `SizingGrip` | bool | Display sizing grip for resizable forms |
| `AutoSize` | bool | Automatically resize to fit contents |
| `AutoSizeMode` | AutoSizeMode | Resize mode: GrowAndShrink or GrowOnly |
| `AutoHeightControls` | bool | Auto-adjust panel heights to StatusBarAdv height |
| `MetroColor` | Color | Metro theme accent color |
| `UseMetroColorAsBorder` | bool | Use MetroColor as border color |
| `ThemesEnabled` | bool | Enable themed background drawing |
| `IgnoreThemeBackground` | bool | Ignore theme background and use BackColor instead |

### StatusBarAdvPanel Properties

| Property   | Type                  | Description |
|------------|-----------------------|-------------|
| `PanelType` | StatusBarAdvPanelType | Panel content type. Values include: `None` (no content), `Text` (custom text), `CurrentCulture` (culture info), `ShortDate` (date), `ShortTime` (time), `Owner` (owner‑drawn content). |
| `Size`      | Size                  | Panel dimensions |
| `Text`      | string                | Custom text to display in panel |

### StatusBarAdv Extender Methods

| Method | Description |
|--------|-------------|
| `SetHAlign(StatusBarAdvPanel, HorzFlowAlign)` | Sets horizontal alignment for a specific panel (Left, Center, Right, Justify). |
| `GetHAlign(StatusBarAdvPanel)` | Gets the current horizontal alignment of a specific panel. |

## Common Use Cases

1. **Application Status Display**: Show ready state, connection status, or operation progress
2. **Date/Time Information**: Display current date and time using built-in panel types
3. **Culture and Localization**: Show current culture information with CurrentCulture panel
4. **Progress Indicators**: Add ProgressBar controls to status bar for long operations
5. **Document Information**: Display file name, line/column position, or word count
6. **Network Status**: Show connectivity state or server status with custom panels
7. **User Session Info**: Display logged-in user name or session details
8. **Themed Applications**: Apply Office2016 or Metro themes for consistent UI

## Additional Resources

- **Syncfusion Documentation**: [StatusBarAdv Official Docs](https://help.syncfusion.com/windowsforms/statusbar/overview)
- **Panel Reference**: See StatusBarAdvPanel documentation for panel-specific properties
- **Theme Support**: StatusBarAdv supports VisualStyleManager for application-wide theming

## Next Steps

After implementing basic StatusBarAdv:
1. **Add Multiple Panels** → Read: [references/panels-and-layout.md](references/panels-and-layout.md)
2. **Customize Appearance** → Read: [references/appearance-styling.md](references/appearance-styling.md)
3. **Apply Themes** → Read: [references/borders-and-themes.md](references/borders-and-themes.md)
