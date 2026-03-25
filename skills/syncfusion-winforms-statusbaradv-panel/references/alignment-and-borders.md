# Alignment and Borders

## Table of Contents
- [Overview](#overview)
- [Content Alignment](#content-alignment)
- [Flow Layout Alignment](#flow-layout-alignment)
- [Border Styles](#border-styles)
- [2D Border Configuration](#2d-border-configuration)
- [3D Border Configuration](#3d-border-configuration)
- [Complete Border Examples](#complete-border-examples)

This guide covers alignment options and border customization for StatusBarAdvPanel.

## Overview

## When to Read This

Read this guide when you need to:
- Align text and icons within panels (Left, Right, Center)
- Configure panel alignment in flow layouts
- Apply borders to panels (None, FixedSingle, Fixed3D)
- Customize 2D border colors and styles
- Configure 3D border effects
- Control which sides display borders
- Create visually distinct panel boundaries

## Content Alignment

The `Alignment` property controls how text and icons are aligned within the panel.

### Alignment Property

**Property:**
- **Type:** `HorizontalAlignment`
- **Options:** Left, Right, Center

**C#:**
```csharp
// Left alignment
var leftAlignPanel = new StatusBarAdvPanel
{
    Text = "Left Aligned",
    Alignment = HorizontalAlignment.Left,
    Size = new Size(150, 24),
    BackgroundColor = new BrushInfo(Color.LightBlue)
};

// Center alignment
var centerAlignPanel = new StatusBarAdvPanel
{
    Text = "Center Aligned",
    Alignment = HorizontalAlignment.Center,
    Size = new Size(150, 24),
    BackgroundColor = new BrushInfo(Color.LightGreen)
};

// Right alignment
var rightAlignPanel = new StatusBarAdvPanel
{
    Text = "Right Aligned",
    Alignment = HorizontalAlignment.Right,
    Size = new Size(150, 24),
    BackgroundColor = new BrushInfo(Color.LightCoral)
};
```

**VB.NET:**
```vb
' Left alignment
Dim leftAlignPanel = New StatusBarAdvPanel With {
    .Text = "Left Aligned",
    .Alignment = HorizontalAlignment.Left,
    .Size = New Size(150, 24),
    .BackgroundColor = New BrushInfo(Color.LightBlue)
}

' Center alignment
Dim centerAlignPanel = New StatusBarAdvPanel With {
    .Text = "Center Aligned",
    .Alignment = HorizontalAlignment.Center,
    .Size = New Size(150, 24),
    .BackgroundColor = New BrushInfo(Color.LightGreen)
}

' Right alignment
Dim rightAlignPanel = New StatusBarAdvPanel With {
    .Text = "Right Aligned",
    .Alignment = HorizontalAlignment.Right,
    .Size = New Size(150, 24),
    .BackgroundColor = New BrushInfo(Color.LightCoral)
}
```

## Flow Layout Alignment

The `HAlign` property controls horizontal alignment in flow layouts, particularly within StatusBarAdv.

### HAlign Property

**Property:**
- **Type:** `HorzFlowAlign`
- **Options:** Left, Right, Center, Justify

**HorzFlowAlign Options:**

| Option | Description |
|--------|-------------|
| **Left** | Aligns panel to the left side |
| **Right** | Aligns panel to the right side |
| **Center** | Centers the panel |
| **Justify** | Expands panel to fill available space |

**C#:**
```csharp
// Left flow alignment
var leftFlowPanel = new StatusBarAdvPanel
{
    Text = "Left Flow",
    HAlign = HorzFlowAlign.Left,
    Size = new Size(100, 24),
    BackgroundColor = new BrushInfo(Color.AliceBlue)
};

// Center flow alignment
var centerFlowPanel = new StatusBarAdvPanel
{
    Text = "Center Flow",
    HAlign = HorzFlowAlign.Center,
    Size = new Size(100, 24),
    BackgroundColor = new BrushInfo(Color.Honeydew)
};

// Right flow alignment
var rightFlowPanel = new StatusBarAdvPanel
{
    Text = "Right Flow",
    HAlign = HorzFlowAlign.Right,
    Size = new Size(100, 24),
    BackgroundColor = new BrushInfo(Color.MistyRose)
};

// Justify (expands to fill space)
var justifyPanel = new StatusBarAdvPanel
{
    Text = "Justified Panel",
    HAlign = HorzFlowAlign.Justify,
    Size = new Size(150, 24),
    BackgroundColor = new BrushInfo(Color.Lavender)
};
```

**VB.NET:**
```vb
' Left flow alignment
Dim leftFlowPanel = New StatusBarAdvPanel With {
    .Text = "Left Flow",
    .HAlign = HorzFlowAlign.Left,
    .Size = New Size(100, 24),
    .BackgroundColor = New BrushInfo(Color.AliceBlue)
}

' Center flow alignment
Dim centerFlowPanel = New StatusBarAdvPanel With {
    .Text = "Center Flow",
    .HAlign = HorzFlowAlign.Center,
    .Size = New Size(100, 24),
    .BackgroundColor = New BrushInfo(Color.Honeydew)
}

' Right flow alignment
Dim rightFlowPanel = New StatusBarAdvPanel With {
    .Text = "Right Flow",
    .HAlign = HorzFlowAlign.Right,
    .Size = New Size(100, 24),
    .BackgroundColor = New BrushInfo(Color.MistyRose)
}

' Justify (expands to fill space)
Dim justifyPanel = New StatusBarAdvPanel With {
    .Text = "Justified Panel",
    .HAlign = HorzFlowAlign.Justify,
    .Size = New Size(150, 24),
    .BackgroundColor = New BrushInfo(Color.Lavender)
}
```

### Justify Alignment Example

The Justify option expands the panel to occupy extra space:

**C#:**
```csharp
private void SetupJustifiedLayout()
{
    // Left panel with fixed size
    var leftPanel = new StatusBarAdvPanel
    {
        Text = "Status",
        HAlign = HorzFlowAlign.Left,
        Size = new Size(100, 24),
        BackgroundColor = new BrushInfo(Color.LightBlue)
    };
    
    // Center panel with Justify - expands to fill remaining space
    var centerPanel = new StatusBarAdvPanel
    {
        Text = "This panel expands to fill available space",
        HAlign = HorzFlowAlign.Justify,
        Size = new Size(200, 24),  // Minimum size
        BackgroundColor = new BrushInfo(Color.LightGreen),
        Alignment = HorizontalAlignment.Center
    };
    
    // Right panel with fixed size
    var rightPanel = new StatusBarAdvPanel
    {
        PanelType = StatusBarAdvPanelType.ShortTime,
        HAlign = HorzFlowAlign.Right,
        Size = new Size(80, 24),
        BackgroundColor = new BrushInfo(Color.LightCoral)
    };
    
    statusBarAdv1.Controls.Add(leftPanel);
    statusBarAdv1.Controls.Add(centerPanel);
    statusBarAdv1.Controls.Add(rightPanel);
}
```

## Border Styles

The `BorderStyle` property determines whether the panel has a border and its type.

### BorderStyle Property

**Property:**
- **Type:** `BorderStyle`
- **Options:** None, FixedSingle, Fixed3D

**C#:**
```csharp
// No border
var noBorderPanel = new StatusBarAdvPanel
{
    Text = "No Border",
    BorderStyle = BorderStyle.None,
    Size = new Size(120, 24),
    BackgroundColor = new BrushInfo(Color.LightBlue)
};

// Fixed single (2D) border
var singleBorderPanel = new StatusBarAdvPanel
{
    Text = "Single Border",
    BorderStyle = BorderStyle.FixedSingle,
    BorderColor = Color.Black,
    Size = new Size(120, 24),
    BackgroundColor = new BrushInfo(Color.LightGreen)
};

// Fixed 3D border
var threeDPanel = new StatusBarAdvPanel
{
    Text = "3D Border",
    BorderStyle = BorderStyle.Fixed3D,
    Border3DStyle = Border3DStyle.Sunken,
    Size = new Size(120, 24),
    BackgroundColor = new BrushInfo(Color.LightCoral)
};
```

**VB.NET:**
```vb
' No border
Dim noBorderPanel = New StatusBarAdvPanel With {
    .Text = "No Border",
    .BorderStyle = BorderStyle.None,
    .Size = New Size(120, 24),
    .BackgroundColor = New BrushInfo(Color.LightBlue)
}

' Fixed single (2D) border
Dim singleBorderPanel = New StatusBarAdvPanel With {
    .Text = "Single Border",
    .BorderStyle = BorderStyle.FixedSingle,
    .BorderColor = Color.Black,
    .Size = New Size(120, 24),
    .BackgroundColor = New BrushInfo(Color.LightGreen)
}

' Fixed 3D border
Dim threeDPanel = New StatusBarAdvPanel With {
    .Text = "3D Border",
    .BorderStyle = BorderStyle.Fixed3D,
    .Border3DStyle = Border3DStyle.Sunken,
    .Size = New Size(120, 24),
    .BackgroundColor = New BrushInfo(Color.LightCoral)
}
```

## 2D Border Configuration

When `BorderStyle = FixedSingle`, configure 2D borders with these properties.

### BorderColor Property

Sets the color of the 2D border.

**C#:**
```csharp
var coloredBorderPanel = new StatusBarAdvPanel
{
    Text = "Custom Border Color",
    BorderStyle = BorderStyle.FixedSingle,
    BorderColor = Color.DarkRed,
    Size = new Size(150, 24),
    BackgroundColor = new BrushInfo(Color.MistyRose)
};

// Using RGB values
var customColorPanel = new StatusBarAdvPanel
{
    Text = "RGB Border",
    BorderStyle = BorderStyle.FixedSingle,
    BorderColor = Color.FromArgb(100, 120, 140),
    Size = new Size(150, 24),
    BackgroundColor = new BrushInfo(Color.AliceBlue)
};
```

**VB.NET:**
```vb
Dim coloredBorderPanel = New StatusBarAdvPanel With {
    .Text = "Custom Border Color",
    .BorderStyle = BorderStyle.FixedSingle,
    .BorderColor = Color.DarkRed,
    .Size = New Size(150, 24),
    .BackgroundColor = New BrushInfo(Color.MistyRose)
}

' Using RGB values
Dim customColorPanel = New StatusBarAdvPanel With {
    .Text = "RGB Border",
    .BorderStyle = BorderStyle.FixedSingle,
    .BorderColor = Color.FromArgb(100, 120, 140),
    .Size = New Size(150, 24),
    .BackgroundColor = New BrushInfo(Color.AliceBlue)
}
```

### BorderSingle Property

Specifies the 2D border style.

**ButtonBorderStyle Options:**
- **Solid** - Solid line (default)
- **Dashed** - Dashed line
- **Dotted** - Dotted line
- **Inset** - Inset effect
- **Outset** - Outset effect
- **None** - No border

**C#:**
```csharp
// Solid border
var solidPanel = new StatusBarAdvPanel
{
    Text = "Solid Border",
    BorderStyle = BorderStyle.FixedSingle,
    BorderSingle = ButtonBorderStyle.Solid,
    BorderColor = Color.Navy,
    Size = new Size(120, 24)
};

// Dashed border
var dashedPanel = new StatusBarAdvPanel
{
    Text = "Dashed Border",
    BorderStyle = BorderStyle.FixedSingle,
    BorderSingle = ButtonBorderStyle.Dashed,
    BorderColor = Color.DarkGreen,
    Size = new Size(120, 24)
};

// Dotted border
var dottedPanel = new StatusBarAdvPanel
{
    Text = "Dotted Border",
    BorderStyle = BorderStyle.FixedSingle,
    BorderSingle = ButtonBorderStyle.Dotted,
    BorderColor = Color.DarkRed,
    Size = new Size(120, 24)
};

// Inset border
var insetPanel = new StatusBarAdvPanel
{
    Text = "Inset Border",
    BorderStyle = BorderStyle.FixedSingle,
    BorderSingle = ButtonBorderStyle.Inset,
    Size = new Size(120, 24)
};

// Outset border
var outsetPanel = new StatusBarAdvPanel
{
    Text = "Outset Border",
    BorderStyle = BorderStyle.FixedSingle,
    BorderSingle = ButtonBorderStyle.Outset,
    Size = new Size(120, 24)
};
```

**VB.NET:**
```vb
' Solid border
Dim solidPanel = New StatusBarAdvPanel With {
    .Text = "Solid Border",
    .BorderStyle = BorderStyle.FixedSingle,
    .BorderSingle = ButtonBorderStyle.Solid,
    .BorderColor = Color.Navy,
    .Size = New Size(120, 24)
}

' Dashed border
Dim dashedPanel = New StatusBarAdvPanel With {
    .Text = "Dashed Border",
    .BorderStyle = BorderStyle.FixedSingle,
    .BorderSingle = ButtonBorderStyle.Dashed,
    .BorderColor = Color.DarkGreen,
    .Size = New Size(120, 24)
}

' Dotted border
Dim dottedPanel = New StatusBarAdvPanel With {
    .Text = "Dotted Border",
    .BorderStyle = BorderStyle.FixedSingle,
    .BorderSingle = ButtonBorderStyle.Dotted,
    .BorderColor = Color.DarkRed,
    .Size = New Size(120, 24)
}
```

### BorderSides Property

Controls which sides of the panel display borders.

**Border3DSide Options:**
- **All** - All four sides
- **Top** - Top side only
- **Bottom** - Bottom side only
- **Left** - Left side only
- **Right** - Right side only
- **Middle** - Middle sections

Combine multiple sides using bitwise OR (`|`).

**C#:**
```csharp
// All sides
var allSidesPanel = new StatusBarAdvPanel
{
    Text = "All Sides",
    BorderStyle = BorderStyle.FixedSingle,
    BorderSides = Border3DSide.All,
    BorderColor = Color.Black,
    Size = new Size(120, 24)
};

// Top and bottom only
var topBottomPanel = new StatusBarAdvPanel
{
    Text = "Top & Bottom",
    BorderStyle = BorderStyle.FixedSingle,
    BorderSides = Border3DSide.Top | Border3DSide.Bottom,
    BorderColor = Color.DarkBlue,
    Size = new Size(120, 24)
};

// Left and right only
var leftRightPanel = new StatusBarAdvPanel
{
    Text = "Left & Right",
    BorderStyle = BorderStyle.FixedSingle,
    BorderSides = Border3DSide.Left | Border3DSide.Right,
    BorderColor = Color.DarkGreen,
    Size = new Size(120, 24)
};

// Top side only
var topOnlyPanel = new StatusBarAdvPanel
{
    Text = "Top Only",
    BorderStyle = BorderStyle.FixedSingle,
    BorderSides = Border3DSide.Top,
    BorderColor = Color.DarkRed,
    Size = new Size(120, 24)
};
```

**VB.NET:**
```vb
' All sides
Dim allSidesPanel = New StatusBarAdvPanel With {
    .Text = "All Sides",
    .BorderStyle = BorderStyle.FixedSingle,
    .BorderSides = Border3DSide.All,
    .BorderColor = Color.Black,
    .Size = New Size(120, 24)
}

' Top and bottom only
Dim topBottomPanel = New StatusBarAdvPanel With {
    .Text = "Top & Bottom",
    .BorderStyle = BorderStyle.FixedSingle,
    .BorderSides = Border3DSide.Top Or Border3DSide.Bottom,
    .BorderColor = Color.DarkBlue,
    .Size = New Size(120, 24)
}

' Left and right only
Dim leftRightPanel = New StatusBarAdvPanel With {
    .Text = "Left & Right",
    .BorderStyle = BorderStyle.FixedSingle,
    .BorderSides = Border3DSide.Left Or Border3DSide.Right,
    .BorderColor = Color.DarkGreen,
    .Size = New Size(120, 24)
}
```

### Complete 2D Border Example

**C#:**
```csharp
private StatusBarAdvPanel Create2DBorderedPanel()
{
    return new StatusBarAdvPanel
    {
        Text = "Custom 2D Border",
        BorderStyle = BorderStyle.FixedSingle,
        BorderColor = Color.DarkRed,
        BorderSingle = ButtonBorderStyle.Dashed,
        BorderSides = Border3DSide.All,
        Size = new Size(150, 26),
        BackgroundColor = new BrushInfo(
            GradientStyle.Vertical,
            Color.LightYellow,
            Color.LemonChiffon
        ),
        ForeColor = Color.DarkRed,
        Alignment = HorizontalAlignment.Center
    };
}
```

## 3D Border Configuration

When `BorderStyle = Fixed3D`, configure 3D borders with the Border3DStyle property.

### Border3DStyle Property

Specifies the style of 3D border.

**Border3DStyle Options:**

| Style | Description |
|-------|-------------|
| **RaisedOuter** | Raised outer edge |
| **SunkenOuter** | Sunken outer edge |
| **RaisedInner** | Raised inner edge |
| **SunkenInner** | Sunken inner edge |
| **Raised** | Combined raised effect |
| **Sunken** | Combined sunken effect (default) |
| **Etched** | Etched appearance |
| **Bump** | Bumped appearance |
| **Adjust** | Adjusted border |
| **Flat** | Flat border |

**C#:**
```csharp
// Raised border
var raisedPanel = new StatusBarAdvPanel
{
    Text = "Raised",
    BorderStyle = BorderStyle.Fixed3D,
    Border3DStyle = Border3DStyle.Raised,
    Size = new Size(100, 24),
    BackgroundColor = new BrushInfo(Color.LightBlue)
};

// Sunken border (default)
var sunkenPanel = new StatusBarAdvPanel
{
    Text = "Sunken",
    BorderStyle = BorderStyle.Fixed3D,
    Border3DStyle = Border3DStyle.Sunken,
    Size = new Size(100, 24),
    BackgroundColor = new BrushInfo(Color.LightGreen)
};

// Etched border
var etchedPanel = new StatusBarAdvPanel
{
    Text = "Etched",
    BorderStyle = BorderStyle.Fixed3D,
    Border3DStyle = Border3DStyle.Etched,
    Size = new Size(100, 24),
    BackgroundColor = new BrushInfo(Color.LightCoral)
};

// Bump border
var bumpPanel = new StatusBarAdvPanel
{
    Text = "Bump",
    BorderStyle = BorderStyle.Fixed3D,
    Border3DStyle = Border3DStyle.Bump,
    Size = new Size(100, 24),
    BackgroundColor = new BrushInfo(Color.LightYellow)
};

// RaisedInner border
var raisedInnerPanel = new StatusBarAdvPanel
{
    Text = "Raised Inner",
    BorderStyle = BorderStyle.Fixed3D,
    Border3DStyle = Border3DStyle.RaisedInner,
    Size = new Size(110, 24),
    BackgroundColor = new BrushInfo(Color.Lavender)
};

// SunkenOuter border
var sunkenOuterPanel = new StatusBarAdvPanel
{
    Text = "Sunken Outer",
    BorderStyle = BorderStyle.Fixed3D,
    Border3DStyle = Border3DStyle.SunkenOuter,
    Size = new Size(110, 24),
    BackgroundColor = new BrushInfo(Color.Honeydew)
};
```

**VB.NET:**
```vb
' Raised border
Dim raisedPanel = New StatusBarAdvPanel With {
    .Text = "Raised",
    .BorderStyle = BorderStyle.Fixed3D,
    .Border3DStyle = Border3DStyle.Raised,
    .Size = New Size(100, 24),
    .BackgroundColor = New BrushInfo(Color.LightBlue)
}

' Sunken border (default)
Dim sunkenPanel = New StatusBarAdvPanel With {
    .Text = "Sunken",
    .BorderStyle = BorderStyle.Fixed3D,
    .Border3DStyle = Border3DStyle.Sunken,
    .Size = New Size(100, 24),
    .BackgroundColor = New BrushInfo(Color.LightGreen)
}

' Etched border
Dim etchedPanel = New StatusBarAdvPanel With {
    .Text = "Etched",
    .BorderStyle = BorderStyle.Fixed3D,
    .Border3DStyle = Border3DStyle.Etched,
    .Size = New Size(100, 24),
    .BackgroundColor = New BrushInfo(Color.LightCoral)
}
```

### 3D Border with BorderSides

Combine 3D borders with specific sides:

**C#:**
```csharp
// 3D border on top and bottom only
var topBottom3DPanel = new StatusBarAdvPanel
{
    Text = "3D Top & Bottom",
    BorderStyle = BorderStyle.Fixed3D,
    Border3DStyle = Border3DStyle.Sunken,
    BorderSides = Border3DSide.Top | Border3DSide.Bottom,
    Size = new Size(140, 24),
    BackgroundColor = new BrushInfo(Color.AliceBlue)
};

// 3D border on right side only
var right3DPanel = new StatusBarAdvPanel
{
    Text = "3D Right Only",
    BorderStyle = BorderStyle.Fixed3D,
    Border3DStyle = Border3DStyle.Etched,
    BorderSides = Border3DSide.Right,
    Size = new Size(120, 24),
    BackgroundColor = new BrushInfo(Color.LightGreen)
};
```

## Complete Border Examples

### Example 1: Professional Status Bar with Mixed Borders

**C#:**
```csharp
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Drawing;
using System.Drawing;
using System.Windows.Forms;

public class BorderStylesForm : Form
{
    private StatusBarAdv statusBarAdv1;
    
    public BorderStylesForm()
    {
        InitializeComponent();
        SetupBorderedStatusBar();
    }
    
    private void SetupBorderedStatusBar()
    {
        statusBarAdv1 = new StatusBarAdv
        {
            Dock = DockStyle.Bottom,
            Height = 30,
            BackgroundColor = new BrushInfo(Color.FromArgb(240, 240, 240))
        };
        
        // Panel 1: 2D dashed border
        var panel1 = new StatusBarAdvPanel
        {
            Text = "Status: Ready",
            Size = new Size(140, 26),
            BorderStyle = BorderStyle.FixedSingle,
            BorderSingle = ButtonBorderStyle.Dashed,
            BorderColor = Color.DarkBlue,
            BorderSides = Border3DSide.All,
            BackgroundColor = new BrushInfo(
                GradientStyle.Horizontal,
                Color.AliceBlue,
                Color.LightSkyBlue
            ),
            ForeColor = Color.Navy,
            HAlign = HorzFlowAlign.Left,
            Alignment = HorizontalAlignment.Left
        };
        
        // Panel 2: 3D sunken border
        var panel2 = new StatusBarAdvPanel
        {
            Text = "Processing",
            Size = new Size(120, 26),
            BorderStyle = BorderStyle.Fixed3D,
            Border3DStyle = Border3DStyle.Sunken,
            BorderSides = Border3DSide.All,
            BackgroundColor = new BrushInfo(Color.LightGreen),
            ForeColor = Color.DarkGreen,
            HAlign = HorzFlowAlign.Center,
            Alignment = HorizontalAlignment.Center
        };
        
        // Panel 3: No border (separator effect)
        var panel3 = new StatusBarAdvPanel
        {
            Text = "|",
            Size = new Size(20, 26),
            BorderStyle = BorderStyle.None,
            BackgroundColor = new BrushInfo(Color.Transparent),
            HAlign = HorzFlowAlign.Center
        };
        
        // Panel 4: 2D solid border on sides only
        var panel4 = new StatusBarAdvPanel
        {
            PanelType = StatusBarAdvPanelType.ShortDate,
            Size = new Size(100, 26),
            BorderStyle = BorderStyle.FixedSingle,
            BorderSingle = ButtonBorderStyle.Solid,
            BorderColor = Color.Gray,
            BorderSides = Border3DSide.Left | Border3DSide.Right,
            BackgroundColor = new BrushInfo(Color.White),
            HAlign = HorzFlowAlign.Right,
            Alignment = HorizontalAlignment.Center
        };
        
        // Panel 5: 3D raised border
        var panel5 = new StatusBarAdvPanel
        {
            PanelType = StatusBarAdvPanelType.ShortTime,
            Size = new Size(80, 26),
            BorderStyle = BorderStyle.Fixed3D,
            Border3DStyle = Border3DStyle.Raised,
            BorderSides = Border3DSide.All,
            BackgroundColor = new BrushInfo(Color.LightYellow),
            HAlign = HorzFlowAlign.Right,
            Alignment = HorizontalAlignment.Center
        };
        
        statusBarAdv1.Controls.Add(panel1);
        statusBarAdv1.Controls.Add(panel2);
        statusBarAdv1.Controls.Add(panel3);
        statusBarAdv1.Controls.Add(panel4);
        statusBarAdv1.Controls.Add(panel5);
        
        this.Controls.Add(statusBarAdv1);
    }
    
    private void InitializeComponent()
    {
        this.Text = "Border Styles Demo";
        this.Size = new Size(700, 400);
    }
}
```

### Example 2: Alignment and Border Combination

**C#:**
```csharp
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Drawing;
using System.Drawing;
using System.Windows.Forms;

public class AlignmentBorderForm : Form
{
    private StatusBarAdv statusBarAdv1;
    
    public AlignmentBorderForm()
    {
        InitializeComponent();
        SetupAlignedBorderedPanels();
    }
    
    private void SetupAlignedBorderedPanels()
    {
        statusBarAdv1 = new StatusBarAdv
        {
            Dock = DockStyle.Bottom,
            Height = 32,
            BackgroundColor = new BrushInfo(Color.WhiteSmoke)
        };
        
        // Left-aligned panel with left border only
        var leftPanel = new StatusBarAdvPanel
        {
            Text = "← Left",
            Size = new Size(100, 28),
            HAlign = HorzFlowAlign.Left,
            Alignment = HorizontalAlignment.Left,
            BorderStyle = BorderStyle.FixedSingle,
            BorderSides = Border3DSide.Left,
            BorderColor = Color.Blue,
            BorderSingle = ButtonBorderStyle.Solid,
            BackgroundColor = new BrushInfo(Color.LightBlue)
        };
        
        // Center-aligned panel with top/bottom borders
        var centerPanel = new StatusBarAdvPanel
        {
            Text = "↔ Center",
            Size = new Size(120, 28),
            HAlign = HorzFlowAlign.Center,
            Alignment = HorizontalAlignment.Center,
            BorderStyle = BorderStyle.FixedSingle,
            BorderSides = Border3DSide.Top | Border3DSide.Bottom,
            BorderColor = Color.Green,
            BorderSingle = ButtonBorderStyle.Solid,
            BackgroundColor = new BrushInfo(Color.LightGreen)
        };
        
        // Right-aligned panel with right border only
        var rightPanel = new StatusBarAdvPanel
        {
            Text = "Right →",
            Size = new Size(100, 28),
            HAlign = HorzFlowAlign.Right,
            Alignment = HorizontalAlignment.Right,
            BorderStyle = BorderStyle.FixedSingle,
            BorderSides = Border3DSide.Right,
            BorderColor = Color.Red,
            BorderSingle = ButtonBorderStyle.Solid,
            BackgroundColor = new BrushInfo(Color.LightCoral)
        };
        
        // Justified panel with all borders
        var justifyPanel = new StatusBarAdvPanel
        {
            Text = "⟷ Justified (Expands)",
            Size = new Size(150, 28),
            HAlign = HorzFlowAlign.Justify,
            Alignment = HorizontalAlignment.Center,
            BorderStyle = BorderStyle.Fixed3D,
            Border3DStyle = Border3DStyle.Etched,
            BorderSides = Border3DSide.All,
            BackgroundColor = new BrushInfo(
                GradientStyle.Horizontal,
                Color.Lavender,
                Color.Thistle
            )
        };
        
        statusBarAdv1.Controls.Add(leftPanel);
        statusBarAdv1.Controls.Add(centerPanel);
        statusBarAdv1.Controls.Add(justifyPanel);
        statusBarAdv1.Controls.Add(rightPanel);
        
        this.Controls.Add(statusBarAdv1);
    }
    
    private void InitializeComponent()
    {
        this.Text = "Alignment & Border Demo";
        this.Size = new Size(700, 400);
    }
}
```

### Example 3: All 3D Border Styles Showcase

**C#:**
```csharp
private void ShowcaseAll3DBorders()
{
    var border3DStyles = new[]
    {
        Border3DStyle.RaisedOuter,
        Border3DStyle.SunkenOuter,
        Border3DStyle.RaisedInner,
        Border3DStyle.SunkenInner,
        Border3DStyle.Raised,
        Border3DStyle.Sunken,
        Border3DStyle.Etched,
        Border3DStyle.Bump,
        Border3DStyle.Adjust,
        Border3DStyle.Flat
    };
    
    int yPos = 10;
    foreach (var style in border3DStyles)
    {
        var panel = new StatusBarAdvPanel
        {
            Text = style.ToString(),
            BorderStyle = BorderStyle.Fixed3D,
            Border3DStyle = style,
            Size = new Size(150, 30),
            Location = new Point(10, yPos),
            BackgroundColor = new BrushInfo(Color.LightSteelBlue),
            Alignment = HorizontalAlignment.Center
        };
        
        this.Controls.Add(panel);
        yPos += 35;
    }
}
```

## Next Steps

After configuring alignment and borders, explore:
- **[Themes, ToolTips, and Events](themes-tooltips-events.md)** - Add themes, tooltips, and event handling
