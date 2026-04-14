# Getting Started with GradientLabel

Complete guide to setting up and initializing the Syncfusion WinForms GradientLabel control in your Windows Forms applications.

## Assembly Deployment

### Required Dependencies

The GradientLabel control requires specific Syncfusion assemblies to function properly.

**Required Assemblies:**
- `Syncfusion.Shared.Base.dll` - Core shared components
- `Syncfusion.Shared.Windows.dll` - Windows-specific shared functionality
- `Syncfusion.Tools.Base.dll` - Tools package base
- `Syncfusion.Tools.Windows.dll` - Tools package containing GradientLabel

### NuGet Package Installation

Install the required package using NuGet Package Manager:

**Package Manager Console:**
```powershell
Install-Package Syncfusion.Tools.WinForms
```

**NuGet Package Manager UI:**
1. Right-click project → Manage NuGet Packages
2. Search for "Syncfusion.Tools.WinForms"
3. Click Install

**Package Reference (csproj):**
```xml
<PackageReference Include="Syncfusion.Tools.WinForms" Version="25.1.35" />
```

### Manual Assembly References

If not using NuGet, add manual references:

1. Right-click References → Add Reference
2. Browse to Syncfusion installation folder:
   - `C:\Program Files (x86)\Syncfusion\Essential Studio\<version>\precompiledassemblies\<.NET version>\`
3. Select all required assemblies listed above

**More Information:**
- [How to Install NuGet Packages](https://help.syncfusion.com/windowsforms/installation/install-nuget-packages)
- [Control Dependencies](https://help.syncfusion.com/windowsforms/control-dependencies#gradientlabel)

---

## Creating Application with GradientLabel

### Prerequisites

- Visual Studio 2015 or later
- .NET Framework 4.0 or later / .NET 6+ (Windows)
- Syncfusion WinForms components installed

### Creating the Project

1. Open Visual Studio
2. File → New → Project
3. Select "Windows Forms App (.NET Framework)" or "Windows Forms App"
4. Name your project (e.g., "GradientLabelDemo")
5. Click Create

---

## Adding GradientLabel via Designer

The easiest way to add GradientLabel is through the Visual Studio designer.

### Steps

1. **Open Form Designer**: Double-click Form1.cs in Solution Explorer
2. **Open Toolbox**: View → Toolbox (or Ctrl+Alt+X)
3. **Locate Control**: Expand "Syncfusion Controls" section
4. **Drag and Drop**: Find "GradientLabel" and drag it onto the form

![GradientLabel in Toolbox](images/gradientlabel-toolbox.png)

**Benefits:**
- Automatic assembly references added
- Designer code generated automatically
- Visual positioning and sizing
- Property Grid access for configuration

### Designer-Generated Code

When you drag and drop, Visual Studio generates initialization code:

```csharp
// In Form1.Designer.cs
private Syncfusion.Windows.Forms.Tools.GradientLabel gradientLabel1;

private void InitializeComponent()
{
    this.gradientLabel1 = new Syncfusion.Windows.Forms.Tools.GradientLabel();
    this.SuspendLayout();
    
    // gradientLabel1
    this.gradientLabel1.Location = new System.Drawing.Point(12, 12);
    this.gradientLabel1.Name = "gradientLabel1";
    this.gradientLabel1.Size = new System.Drawing.Size(200, 50);
    this.gradientLabel1.TabIndex = 0;
    this.gradientLabel1.Text = "gradientLabel1";
    
    // Form1
    this.Controls.Add(this.gradientLabel1);
    this.ResumeLayout(false);
}
```

---

## Adding GradientLabel via Code

For programmatic control creation, add GradientLabel manually in code.

### Step 1: Add Using Directives

```csharp
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Drawing;
using System.Drawing;
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms.Tools
Imports Syncfusion.Drawing
Imports System.Drawing
```

### Step 2: Create and Configure Control

**C# Example:**
```csharp
public partial class Form1 : Form
{
    private GradientLabel gradientLabel;
    
    public Form1()
    {
        InitializeComponent();
        InitializeGradientLabel();
    }
    
    private void InitializeGradientLabel()
    {
        // Create GradientLabel instance
        gradientLabel = new GradientLabel();
        
        // Set size and position
        gradientLabel.Size = new Size(200, 50);
        gradientLabel.Location = new Point(20, 20);
        
        // Set text properties
        gradientLabel.Text = "Gradient Label";
        gradientLabel.ForeColor = Color.White;
        gradientLabel.Font = new Font("Arial", 12, FontStyle.Bold);
        
        // Set name for reference
        gradientLabel.Name = "gradientLabel";
        gradientLabel.TabIndex = 0;
        
        // Add to form's controls collection
        this.Controls.Add(gradientLabel);
    }
}
```

**VB.NET Example:**
```vb
Public Class Form1
    Private gradientLabel As GradientLabel
    
    Public Sub New()
        InitializeComponent()
        InitializeGradientLabel()
    End Sub
    
    Private Sub InitializeGradientLabel()
        ' Create GradientLabel instance
        gradientLabel = New GradientLabel()
        
        ' Set size and position
        gradientLabel.Size = New Size(200, 50)
        gradientLabel.Location = New Point(20, 20)
        
        ' Set text properties
        gradientLabel.Text = "Gradient Label"
        gradientLabel.ForeColor = Color.White
        gradientLabel.Font = New Font("Arial", 12, FontStyle.Bold)
        
        ' Set name for reference
        gradientLabel.Name = "gradientLabel"
        gradientLabel.TabIndex = 0
        
        ' Add to form's controls collection
        Me.Controls.Add(gradientLabel)
    End Sub
End Class
```

---

## Simple Gradient Setup

Apply a basic gradient background to the label.

### Horizontal Gradient

**C# Example:**
```csharp
// Create horizontal gradient from red to blue
gradientLabel.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.Red,
    Color.MediumBlue
);
```

**VB.NET Example:**
```vb
' Create horizontal gradient from red to blue
gradientLabel.BackgroundColor = New BrushInfo(
    GradientStyle.Horizontal,
    Color.Red,
    Color.MediumBlue
)
```

![Horizontal Gradient](images/horizontal-gradient.png)

### Vertical Gradient

**C# Example:**
```csharp
// Create vertical gradient from dark to light
gradientLabel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.DarkBlue,
    Color.LightSkyBlue
);
```

**VB.NET Example:**
```vb
' Create vertical gradient from dark to light
gradientLabel.BackgroundColor = New BrushInfo(
    GradientStyle.Vertical,
    Color.DarkBlue,
    Color.LightSkyBlue
)
```

---

## Complete Basic Example

Full working example combining initialization and gradient setup:

**C# Example:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Drawing;

namespace GradientLabelDemo
{
    public partial class Form1 : Form
    {
        private GradientLabel headerLabel;
        
        public Form1()
        {
            InitializeComponent();
            InitializeControls();
        }
        
        private void InitializeControls()
        {
            // Create GradientLabel for header
            headerLabel = new GradientLabel
            {
                Size = new Size(300, 60),
                Location = new Point(20, 20),
                Text = "Welcome to My Application",
                Font = new Font("Segoe UI", 14, FontStyle.Bold),
                ForeColor = Color.White,
                TextAlign = ContentAlignment.MiddleCenter
            };
            
            // Set gradient background
            headerLabel.BackgroundColor = new BrushInfo(
                GradientStyle.Vertical,
                Color.DarkSlateBlue,
                Color.MediumPurple
            );
            
            // Add to form
            this.Controls.Add(headerLabel);
        }
    }
}
```

**VB.NET Example:**
```vb
Imports System.Drawing
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools
Imports Syncfusion.Drawing

Public Class Form1
    Private headerLabel As GradientLabel
    
    Public Sub New()
        InitializeComponent()
        InitializeControls()
    End Sub
    
    Private Sub InitializeControls()
        ' Create GradientLabel for header
        headerLabel = New GradientLabel With {
            .Size = New Size(300, 60),
            .Location = New Point(20, 20),
            .Text = "Welcome to My Application",
            .Font = New Font("Segoe UI", 14, FontStyle.Bold),
            .ForeColor = Color.White,
            .TextAlign = ContentAlignment.MiddleCenter
        }
        
        ' Set gradient background
        headerLabel.BackgroundColor = New BrushInfo(
            GradientStyle.Vertical,
            Color.DarkSlateBlue,
            Color.MediumPurple
        )
        
        ' Add to form
        Me.Controls.Add(headerLabel)
    End Sub
End Class
```

---

## Text Alignment

Control how text is positioned within the gradient label.

**C# Example:**
```csharp
// Center alignment (most common for headers)
gradientLabel.TextAlign = ContentAlignment.MiddleCenter;

// Left alignment
gradientLabel.TextAlign = ContentAlignment.MiddleLeft;

// Top-right corner
gradientLabel.TextAlign = ContentAlignment.TopRight;
```

**VB.NET Example:**
```vb
' Center alignment (most common for headers)
gradientLabel.TextAlign = ContentAlignment.MiddleCenter

' Left alignment
gradientLabel.TextAlign = ContentAlignment.MiddleLeft

' Top-right corner
gradientLabel.TextAlign = ContentAlignment.TopRight
```

---

## Best Practices

### 1. Use Meaningful Text

```csharp
// Good: Descriptive text
gradientLabel.Text = "User Information";

// Avoid: Generic text
gradientLabel.Text = "gradientLabel1";
```

### 2. Choose Readable Colors

```csharp
// Ensure text contrast against gradient
gradientLabel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.DarkBlue,  // Dark background
    Color.Blue
);
gradientLabel.ForeColor = Color.White;  // Light text for contrast
```

### 3. Size Appropriately

```csharp
// Make height sufficient for text and visual effect
gradientLabel.Size = new Size(250, 50);  // Good height for gradient visibility

// Avoid too small
// gradientLabel.Size = new Size(250, 20);  // Gradient barely visible
```

### 4. Center Text for Headers

```csharp
// Headers look best centered
gradientLabel.TextAlign = ContentAlignment.MiddleCenter;
```

---

## Next Steps

- **Background Styling**: Learn gradient styles and multi-color options → [background-styling.md](background-styling.md)
- **Border Configuration**: Add and customize borders → [border-configuration.md](border-configuration.md)
- **Foreground Settings**: Configure text appearance → [foreground-text-settings.md](foreground-text-settings.md)
- **Serialization**: Save/load gradient settings → [serialization.md](serialization.md)

---

## Troubleshooting

### GradientLabel Not in Toolbox

**Solution:**
1. Verify Syncfusion installation
2. Tools → Choose Toolbox Items
3. Browse to Syncfusion.Tools.Windows.dll
4. Check GradientLabel in list
5. Click OK

### Assembly Reference Errors

**Solution:**
1. Verify NuGet package installed: `Syncfusion.Tools.WinForms`
2. Check all 6 required assemblies are referenced
3. Clean and rebuild solution
4. Ensure correct .NET Framework/version target

### Gradient Not Displaying

**Solution:**
1. Verify BackgroundColor property is set
2. Check BrushInfo constructor parameters
3. Ensure valid Color values used
4. Try simple two-color gradient first
5. Verify control Size is adequate (minimum 30x30)

### Text Not Visible

**Solution:**
1. Check ForeColor contrasts with gradient colors
2. Verify Text property is set
3. Ensure Font size is appropriate for control size
4. Check TextAlign property
