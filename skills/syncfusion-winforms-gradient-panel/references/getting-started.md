# Getting Started

## Overview

This guide covers the installation and initial setup of the **Syncfusion GradientPanel** control in Windows Forms applications. GradientPanel is a panel-derived container control with advanced background customization capabilities.

**What you'll learn:**
- Assembly deployment and dependencies
- NuGet package installation
- Adding GradientPanel via Designer
- Adding GradientPanel via code
- Basic configuration

## Assembly Deployment

### Required Assemblies

The GradientPanel control requires the following assembly:

```
Syncfusion.Shared.Base.dll
```

**Assembly location** (if installed locally):
```
C:\Program Files (x86)\Syncfusion\Essential Studio\Windows\{version}\precompiledassemblies\{framework version}\
```

### Framework Support

GradientPanel supports:
- .NET Framework 4.5, 4.5.1, 4.6, 4.7, 4.8
- .NET 6.0, .NET 7.0, .NET 8.0

## NuGet Installation

### Method 1: Package Manager Console

```powershell
Install-Package Syncfusion.Tools.Windows
```

This package includes GradientPanel and other Tools controls.

### Method 2: NuGet Package Manager UI

1. Right-click on your project in Solution Explorer
2. Select **Manage NuGet Packages**
3. Search for **"Syncfusion.Tools.Windows"**
4. Select the package
5. Click **Install**

### Method 3: .NET CLI

```bash
dotnet add package Syncfusion.Tools.Windows
```

**Note:** NuGet automatically adds required assembly references.

## Adding GradientPanel via Designer

### Step 1: Open Toolbox

1. Create or open a Windows Forms project in Visual Studio
2. Open a form in Designer view
3. Open the Toolbox (View → Toolbox or Ctrl+Alt+X)

### Step 2: Locate GradientPanel

1. In Toolbox, search for **"GradientPanel"**
2. Or browse the **Syncfusion Tools** section
3. If not visible:
   - Right-click Toolbox → **Choose Items**
   - Browse to `Syncfusion.Shared.Base.dll`
   - Check **GradientPanel**
   - Click **OK**

### Step 3: Add to Form

1. **Drag** GradientPanel from Toolbox
2. **Drop** onto form Designer
3. Position and resize as needed
4. Required assemblies are referenced automatically

![GradientPanel in Toolbox](../../../docs/GradientPanel-Images/GradientPanel_toolbox.png)

### Step 4: Configure via Property Grid

Use the **Properties window** to configure:
- `BackgroundColor` - Set gradient, pattern, or solid background
- `BorderStyle` - FixedSingle or Fixed3D
- `BorderColor` - For 2D borders
- `Size` - Panel dimensions
- `Location` - Panel position

![GradientPanel Style Configuration](../../../docs/GradientPanel-Images/GradientPanel_style.png)

## Adding GradientPanel via Code

### Step 1: Create Windows Forms Project

Create a new Windows Forms project in Visual Studio.

### Step 2: Add Assembly Reference

If not using NuGet, manually add reference:
1. Right-click project → **Add Reference**
2. Browse to `Syncfusion.Shared.Base.dll`
3. Click **OK**

### Step 3: Import Namespace

Add the required using directives:

```csharp
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Drawing;
using System.Drawing;
using System.Windows.Forms;
```

**Namespace contents:**
- `Syncfusion.Windows.Forms.Tools` - Contains GradientPanel
- `Syncfusion.Drawing` - Contains BrushInfo, GradientStyle, PatternStyle

### Step 4: Create GradientPanel Instance

```csharp
public partial class Form1 : Form
{
    private GradientPanel gradientPanel1;

    public Form1()
    {
        InitializeComponent();
        CreateGradientPanel();
    }

    private void CreateGradientPanel()
    {
        // Create instance
        gradientPanel1 = new GradientPanel();
        
        // Set size and location
        gradientPanel1.Size = new Size(400, 300);
        gradientPanel1.Location = new Point(20, 20);
        
        // Add to form
        this.Controls.Add(gradientPanel1);
    }
}
```

## Basic Configuration

### Set Solid Background

```csharp
// Solid color background
gradientPanel1.BackgroundColor = new BrushInfo(Color.MediumBlue);
```

### Set Gradient Background

```csharp
// Gradient from blue to red, forward diagonal
gradientPanel1.BackgroundColor = new BrushInfo(
    GradientStyle.ForwardDiagonal,
    Color.Blue,     // Start color
    Color.Red       // End color
);
```

### Set Pattern Background

```csharp
// Checkerboard pattern
gradientPanel1.BackgroundColor = new BrushInfo(
    PatternStyle.LargeCheckerBoard,
    Color.Turquoise,   // Foreground
    Color.MediumBlue   // Background
);
```

### Add Border

```csharp
// 2D border
gradientPanel1.BorderStyle = BorderStyle.FixedSingle;
gradientPanel1.BorderColor = Color.Red;

// Or 3D border
gradientPanel1.BorderStyle = BorderStyle.Fixed3D;
gradientPanel1.Border3DStyle = Border3DStyle.Etched;
```

## Complete Setup Example

### Full Implementation

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Drawing;

namespace GradientPanelDemo
{
    public partial class Form1 : Form
    {
        private GradientPanel gradientPanel1;

        public Form1()
        {
            InitializeComponent();
            SetupGradientPanel();
        }

        private void SetupGradientPanel()
        {
            // Create GradientPanel
            gradientPanel1 = new GradientPanel();
            gradientPanel1.Size = new Size(400, 300);
            gradientPanel1.Location = new Point(50, 50);
            
            // Set gradient background
            gradientPanel1.BackgroundColor = new BrushInfo(
                GradientStyle.ForwardDiagonal,
                Color.FromArgb(0, 120, 215),  // Blue
                Color.FromArgb(0, 80, 150)    // Darker blue
            );
            
            // Set border
            gradientPanel1.BorderStyle = BorderStyle.FixedSingle;
            gradientPanel1.BorderColor = Color.White;
            
            // Add to form
            this.Controls.Add(gradientPanel1);
            
            // Add child controls
            AddChildControls();
        }

        private void AddChildControls()
        {
            // Add label
            Label titleLabel = new Label();
            titleLabel.Text = "Welcome to GradientPanel";
            titleLabel.Font = new Font("Segoe UI", 16, FontStyle.Bold);
            titleLabel.ForeColor = Color.White;
            titleLabel.BackColor = Color.Transparent;
            titleLabel.AutoSize = true;
            titleLabel.Location = new Point(20, 20);
            gradientPanel1.Controls.Add(titleLabel);
            
            // Add button
            Button actionButton = new Button();
            actionButton.Text = "Click Me";
            actionButton.Size = new Size(100, 30);
            actionButton.Location = new Point(20, 60);
            gradientPanel1.Controls.Add(actionButton);
        }
    }
}
```

## Common Setup Patterns

### Pattern 1: Form Background Panel

```csharp
// Full-form background panel
GradientPanel backgroundPanel = new GradientPanel();
backgroundPanel.Dock = DockStyle.Fill;
backgroundPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.WhiteSmoke,
    Color.LightGray
);
this.Controls.Add(backgroundPanel);
```

### Pattern 2: Header Panel

```csharp
// Top-docked header panel
GradientPanel headerPanel = new GradientPanel();
headerPanel.Dock = DockStyle.Top;
headerPanel.Height = 80;
headerPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.DarkSlateBlue,
    Color.MediumPurple
);
this.Controls.Add(headerPanel);
```

### Pattern 3: Sidebar Panel

```csharp
// Left-docked sidebar
GradientPanel sidebarPanel = new GradientPanel();
sidebarPanel.Dock = DockStyle.Left;
sidebarPanel.Width = 200;
sidebarPanel.BackgroundColor = new BrushInfo(
    PatternStyle.DarkDownwardDiagonal,
    Color.Gray,
    Color.DarkGray
);
this.Controls.Add(sidebarPanel);
```

## Troubleshooting

### Issue: GradientPanel not in Toolbox

**Causes:**
- Syncfusion assemblies not referenced
- Toolbox not refreshed

**Solutions:**
1. Verify `Syncfusion.Shared.Base.dll` is referenced in project
2. Clean and rebuild solution
3. Restart Visual Studio
4. Manually add via Choose Items (browse to DLL)
5. Check target framework matches assembly version

### Issue: Gradient not displaying

**Causes:**
- Using `BackColor` instead of `BackgroundColor`
- BrushInfo not configured correctly

**Solution:**
```csharp
// INCORRECT
gradientPanel1.BackColor = Color.Blue;  // Only solid color

// CORRECT
gradientPanel1.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.Blue,
    Color.Red
);
```

### Issue: Border not visible

**Causes:**
- BorderStyle not set
- BorderColor matches background

**Solution:**
```csharp
// Ensure BorderStyle is set
gradientPanel1.BorderStyle = BorderStyle.FixedSingle;

// Set contrasting BorderColor
gradientPanel1.BorderColor = Color.Black;
```

### Issue: Child controls obscure gradient

**Cause:** Child controls have opaque backgrounds

**Solution:**
```csharp
// Set child control background to transparent
label.BackColor = Color.Transparent;
button.FlatStyle = FlatStyle.Flat;
button.BackColor = Color.Transparent;
```

### Issue: FileNotFoundException at runtime

**Solutions:**
1. Verify `Syncfusion.Shared.Base.dll` is in output directory
2. Check **Copy Local = True** for assembly reference
3. Verify target framework compatibility (.NET version)
4. Rebuild solution

## Next Steps

Once installation is complete, explore:

- **Background Styles**: Read [background-styles.md](background-styles.md) for gradient, pattern, and solid backgrounds
- **Appearance**: Read [appearance-customization.md](appearance-customization.md) for text, font, and image customization
- **Borders**: Read [border-settings.md](border-settings.md) for 2D and 3D border configuration
- **Scrolling**: Read [scroll-sizing.md](scroll-sizing.md) for auto-scroll and auto-size features
