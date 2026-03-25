# Getting Started with GradientPanelExt

Complete guide to installing, setting up, and creating your first GradientPanelExt control in Windows Forms applications.

## Overview

GradientPanelExt is an enhanced panel control that provides gradient backgrounds, rounded corners, and the ability to host primitives (text, images, controls) in panel borders. It's perfect for creating visually appealing container panels.

**Prerequisites:**
- Visual Studio 2017 or later
- .NET Framework 4.5+ or .NET 6-8 (Windows)
- Syncfusion WinForms tools installed

---

## Assembly Deployment

### Required Assemblies

GradientPanelExt requires only one assembly:

**Assembly:**
- **Syncfusion.Shared.Base.dll**

**Namespaces:**
```csharp
using Syncfusion.Windows.Forms.Tools;  // For GradientPanelExt
using Syncfusion.Drawing;              // For BrushInfo
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms.Tools  ' For GradientPanelExt
Imports Syncfusion.Drawing              ' For BrushInfo
```

---

### NuGet Package Installation

Install via NuGet Package Manager:

**Package Manager Console:**
```bash
Install-Package Syncfusion.Windows.Forms
```

**Or for specific tools package:**
```bash
Install-Package Syncfusion.Tools.Windows
```

**.NET CLI:**
```bash
dotnet add package Syncfusion.Windows.Forms
```

**Package Manager UI:**
1. Right-click project → Manage NuGet Packages
2. Search "Syncfusion.Windows.Forms" or "Syncfusion.Tools.Windows"
3. Install the package

---

## Adding via Visual Studio Designer

### Step 1: Add to Toolbox

1. Open Visual Studio and create/open a WinForms project
2. Open a Form in Designer view
3. If Syncfusion controls aren't in Toolbox:
   - Right-click Toolbox → Choose Items
   - Browse to Syncfusion.Shared.Base.dll
   - Check GradientPanelExt → OK

### Step 2: Drag and Drop

1. Locate **GradientPanelExt** in Toolbox (Syncfusion section)
2. Drag it onto your Form
3. Resize and position as needed

![GradientPanelExt in Toolbox](../images/toolbox.png)

### Step 3: Configure Properties

**In Properties Window:**

**BackgroundColor:**
1. Expand **BackgroundColor** property
2. Set **Style** to "Gradient" (or Solid, Pattern, None)
3. If Gradient selected:
   - Set **GradientStyle** (Horizontal, Vertical, etc.)
   - Set **BackColor** (starting color)
   - Set **ForeColor** (ending color)

**CornerRadius:**
- Set **CornerRadius** to desired value (e.g., 10 for rounded corners, 0 for sharp)

**Size and Location:**
- Set **Size** (e.g., 400, 200)
- Set **Location** on form

### Step 4: Add Primitives (Optional)

1. Click **Primitives** property (shows "Collection")
2. Click **[...]** button to open **PrimitiveCollection Editor**
3. In dropdown, select primitive type:
   - CollapsePrimitive
   - ImagePrimitive
   - TextPrimitive
   - HostPrimitive
4. Click **Add** button
5. Configure primitive properties in right panel
6. Click **OK**

---

## Adding via Code

### Basic Code Setup

**C# Example:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Drawing;

namespace MyWinFormsApp
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
            
            // Create GradientPanelExt
            GradientPanelExt gradientPanel = new GradientPanelExt();
            
            // Set size and location
            gradientPanel.Size = new Size(400, 200);
            gradientPanel.Location = new Point(20, 20);
            
            // Set gradient background
            gradientPanel.BackgroundColor = new BrushInfo(
                GradientStyle.Horizontal,
                Color.Navy,
                Color.SkyBlue
            );
            
            // Rounded corners
            gradientPanel.CornerRadius = 10;
            
            // Add to form
            this.Controls.Add(gradientPanel);
        }
    }
}
```

**VB.NET Example:**
```vb
Imports System
Imports System.Drawing
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools
Imports Syncfusion.Drawing

Public Class Form1
    Public Sub New()
        InitializeComponent()
        
        ' Create GradientPanelExt
        Dim gradientPanel As New GradientPanelExt()
        
        ' Set size and location
        gradientPanel.Size = New Size(400, 200)
        gradientPanel.Location = New Point(20, 20)
        
        ' Set gradient background
        gradientPanel.BackgroundColor = New BrushInfo( _
            GradientStyle.Horizontal, _
            Color.Navy, _
            Color.SkyBlue _
        )
        
        ' Rounded corners
        gradientPanel.CornerRadius = 10
        
        ' Add to form
        Me.Controls.Add(gradientPanel)
    End Sub
End Class
```

---

## Adding Child Controls to Panel

GradientPanelExt acts as a container - you can add any WinForms controls inside it.

**C# Example:**
```csharp
// Create panel
GradientPanelExt panel = new GradientPanelExt
{
    Size = new Size(350, 150),
    Location = new Point(30, 30),
    CornerRadius = 12
};

panel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.WhiteSmoke,
    Color.LightGray
);

// Add Label inside panel
Label lblTitle = new Label
{
    Text = "Welcome",
    Font = new Font("Arial", 14, FontStyle.Bold),
    ForeColor = Color.DarkBlue,
    BackColor = Color.Transparent,
    Location = new Point(20, 20),
    AutoSize = true
};
panel.Controls.Add(lblTitle);

// Add TextBox inside panel
TextBox txtName = new TextBox
{
    Location = new Point(20, 50),
    Size = new Size(300, 20),
    Font = new Font("Arial", 10)
};
panel.Controls.Add(txtName);

// Add Button inside panel
Button btnSubmit = new Button
{
    Text = "Submit",
    Location = new Point(20, 80),
    Size = new Size(100, 30)
};
panel.Controls.Add(btnSubmit);

// Add panel to form
this.Controls.Add(panel);
```

**VB.NET Example:**
```vb
' Create panel
Dim panel As New GradientPanelExt With {
    .Size = New Size(350, 150),
    .Location = New Point(30, 30),
    .CornerRadius = 12
}

panel.BackgroundColor = New BrushInfo( _
    GradientStyle.Vertical, _
    Color.WhiteSmoke, _
    Color.LightGray _
)

' Add Label inside panel
Dim lblTitle As New Label With {
    .Text = "Welcome",
    .Font = New Font("Arial", 14, FontStyle.Bold),
    .ForeColor = Color.DarkBlue,
    .BackColor = Color.Transparent,
    .Location = New Point(20, 20),
    .AutoSize = True
}
panel.Controls.Add(lblTitle)

' Add TextBox inside panel
Dim txtName As New TextBox With {
    .Location = New Point(20, 50),
    .Size = New Size(300, 20),
    .Font = New Font("Arial", 10)
}
panel.Controls.Add(txtName)

' Add Button inside panel
Dim btnSubmit As New Button With {
    .Text = "Submit",
    .Location = New Point(20, 80),
    .Size = New Size(100, 30)
}
panel.Controls.Add(btnSubmit)

' Add panel to form
Me.Controls.Add(panel)
```

**Important:** Set child control `BackColor = Color.Transparent` if you want the gradient to show through.

---

## Simple Examples

### Example 1: Basic Horizontal Gradient Panel

```csharp
GradientPanelExt simplePanel = new GradientPanelExt();
simplePanel.Size = new Size(300, 100);
simplePanel.Location = new Point(50, 50);

// Simple horizontal gradient: blue to light blue
simplePanel.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.Blue,
    Color.LightBlue
);

simplePanel.CornerRadius = 8;

this.Controls.Add(simplePanel);
```

---

### Example 2: Vertical Gradient with Content

```csharp
GradientPanelExt contentPanel = new GradientPanelExt
{
    Size = new Size(400, 200),
    Location = new Point(20, 20),
    CornerRadius = 10
};

// Vertical gradient: dark gray to white
contentPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.FromArgb(60, 60, 60),
    Color.White
);

// Add heading
Label heading = new Label
{
    Text = "Information Panel",
    Font = new Font("Segoe UI", 16, FontStyle.Bold),
    ForeColor = Color.White,
    BackColor = Color.Transparent,
    Location = new Point(20, 15),
    AutoSize = true
};
contentPanel.Controls.Add(heading);

// Add description
Label description = new Label
{
    Text = "This panel contains important information.\nPlease review the details below.",
    Font = new Font("Arial", 10),
    ForeColor = Color.Black,
    BackColor = Color.Transparent,
    Location = new Point(20, 60),
    AutoSize = true
};
contentPanel.Controls.Add(description);

this.Controls.Add(contentPanel);
```

---

### Example 3: Panel with Simple Text Primitive

```csharp
GradientPanelExt panelWithTitle = new GradientPanelExt
{
    Size = new Size(350, 180),
    Location = new Point(40, 40),
    CornerRadius = 12
};

// Gradient background
panelWithTitle.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.DarkSlateBlue,
    Color.CornflowerBlue
);

// Add title primitive at top border
TextPrimitive titlePrimitive = new TextPrimitive
{
    Text = "Settings",
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top,
    TextColor = Color.White,
    TextFont = new Font("Segoe UI", 12, FontStyle.Bold),
    Size = new Size(80, 25),
    BackColor = Color.Transparent
};

panelWithTitle.Primitives.Add(titlePrimitive);

this.Controls.Add(panelWithTitle);
```

---

## PrimitiveCollection Editor

The **PrimitiveCollection Editor** is the visual designer tool for adding and configuring primitives.

### Opening the Editor

**Via Properties Window:**
1. Select GradientPanelExt in Designer
2. Find **Primitives** property
3. Click **[...]** button

**What You'll See:**
- **Left panel:** List of added primitives
- **Dropdown:** Select primitive type to add
- **Add/Remove buttons:** Manage primitives
- **Right panel:** Properties for selected primitive

### Adding a Primitive

1. Select type from dropdown (CollapsePrimitive, ImagePrimitive, TextPrimitive, HostPrimitive)
2. Click **Add**
3. Configure properties on right:
   - **Alignment:** Top, Bottom, Left, Right
   - **Position:** Pixel offset along border
   - **Size:** Width and Height
   - **BackColor:** Background color
   - Type-specific properties (Text, Image, HostControl, etc.)
4. Click **OK** to apply

---

## Troubleshooting

### Issue: Control not showing in Toolbox

**Solution:**
1. Right-click Toolbox → Reset Toolbox
2. Or manually add: Choose Items → Browse to Syncfusion.Shared.Base.dll
3. Ensure NuGet package is installed

### Issue: Gradient not visible

**Check:**
- BackgroundColor.Style is set to "Gradient" (not None)
- BackColor and ForeColor are different colors
- GradientStyle is set (Horizontal, Vertical, etc.)

### Issue: Child controls covering gradient

**Solution:** Set child control `BackColor = Color.Transparent`

```csharp
label.BackColor = Color.Transparent;  // Gradient shows through
```

### Issue: Rounded corners not showing

**Check:**
- CornerRadius > 0 (try 10 or higher)
- Panel size is adequate (very small panels may not show rounding)

### Issue: Cannot find Syncfusion.Drawing namespace

**Solution:** Add reference to Syncfusion.Shared.Base.dll

```csharp
using Syncfusion.Drawing;  // For BrushInfo, GradientStyle
```

### Issue: Primitives not appearing

**Check:**
- Primitive Size is adequate (width and height > 0)
- Primitive Position is within panel bounds
- Alignment is set correctly
- Panel size accommodates primitives in borders

---

## Next Steps

Now that you have basic setup working:

1. **Customize backgrounds:** See [background-styling.md](background-styling.md) for gradient styles, multi-color gradients, and patterns
2. **Adjust corners and borders:** See [border-corner-settings.md](border-corner-settings.md) for CornerRadius and BorderGap
3. **Add border elements:** See [primitives.md](primitives.md) for complete primitive documentation
4. **Enable collapse:** See [collapse-expand-animation.md](collapse-expand-animation.md) for collapsible panels
5. **Handle events:** See [scroll-settings-events.md](scroll-settings-events.md) for scrolling and events

---

## Related Topics

- **Background Styling**: Gradients and colors → [background-styling.md](background-styling.md)
- **Primitives**: Border elements → [primitives.md](primitives.md)
- **Border Settings**: Corners and gaps → [border-corner-settings.md](border-corner-settings.md)
