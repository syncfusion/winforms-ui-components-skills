# Getting Started with StatusBarAdvPanel

This guide covers the initial setup and basic configuration of the StatusBarAdvPanel control in Windows Forms applications.

## When to Read This

Read this guide when you need to:
- Set up StatusBarAdvPanel for the first time
- Install required assemblies and packages
- Create panels using the designer
- Implement panels programmatically
- Understand basic panel configuration
- Troubleshoot setup issues

## Assembly Requirements

StatusBarAdvPanel requires two Syncfusion assemblies:

**Required Assemblies:**
1. **Syncfusion.Tools.Windows.dll** - Contains StatusBarAdvPanel control
2. **Syncfusion.Shared.Base.dll** - Provides shared functionality and BrushInfo

**Namespace:**
```csharp
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Drawing;  // For BrushInfo
```

```vb
Imports Syncfusion.Windows.Forms.Tools
Imports Syncfusion.Drawing  ' For BrushInfo
```

## Installation Methods

### Method 1: NuGet Package Installation

Install via Package Manager Console:

```powershell
Install-Package Syncfusion.Tools.Windows
```

**Or use NuGet Package Manager UI:**
1. Right-click project → **Manage NuGet Packages**
2. Select **Browse** tab
3. Search for **Syncfusion.Tools.Windows**
4. Select the package
5. Click **Install**
6. Accept license agreement

### Method 2: Manual Assembly Reference

If assemblies are already installed:

1. Right-click **References** in Solution Explorer
2. Select **Add Reference...**
3. Click **Browse** button
4. Navigate to Syncfusion installation folder (default):
   ```
   C:\Program Files (x86)\Syncfusion\Essential Studio\<Version>\precompiledassemblies\<.NET Version>\
   ```
5. Select **Syncfusion.Tools.Windows.dll** and **Syncfusion.Shared.Base.dll**
6. Click **OK**

## Designer-Based Setup

### Adding StatusBarAdvPanel from Toolbox

**Steps:**
1. Open your form in **Design view**
2. Locate **StatusBarAdvPanel** in the **Toolbox** under **Syncfusion Windows Forms Tools**
3. Drag and drop StatusBarAdvPanel onto your form or onto an existing StatusBarAdv control
4. The panel appears at the default location

**Configure in Properties Window:**

Set basic properties through the designer:

| Property | Example Value | Description |
|----------|---------------|-------------|
| **PanelType** | StatusBarAdvPanelType.LongDate | Type of panel content |
| **BackgroundColor** | (BrushInfo) | Background styling |
| **BorderColor** | Color.Black | Border color |
| **Size** | 150, 30 | Panel dimensions |
| **HAlign** | HorzFlowAlign.Left | Horizontal alignment |

**Example Designer Configuration:**

1. Set **PanelType** to **LongDate** in Properties window
2. Set **BackgroundColor** → expand → set **Style** to **Gradient**, **BackColor** to **LavenderBlush**, **ForeColor** to **RosyBrown**, **GradientStyle** to **PathRectangle**
3. Set **BorderColor** to **Black**
4. Build and run the application

## Programmatic Creation

### Basic Programmatic Implementation

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
        CreateStatusBarAdvPanel();
    }
    
    private void CreateStatusBarAdvPanel()
    {
        // Create the panel
        statusBarAdvPanel1 = new StatusBarAdvPanel();
        
        // Set location and size
        statusBarAdvPanel1.Location = new Point(160, 184);
        statusBarAdvPanel1.Size = new Size(216, 48);
        
        // Configure panel type
        statusBarAdvPanel1.PanelType = StatusBarAdvPanelType.LongDate;
        
        // Set background with gradient
        statusBarAdvPanel1.BackgroundColor = new BrushInfo(
            GradientStyle.BackwardDiagonal,
            Color.PaleVioletRed,
            Color.PeachPuff
        );
        
        // Set border
        statusBarAdvPanel1.BorderColor = Color.Black;
        
        // Set alignment
        statusBarAdvPanel1.HAlign = HorzFlowAlign.Left;
        
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
        CreateStatusBarAdvPanel()
    End Sub
    
    Private Sub CreateStatusBarAdvPanel()
        ' Create the panel
        statusBarAdvPanel1 = New StatusBarAdvPanel()
        
        ' Set location and size
        statusBarAdvPanel1.Location = New Point(160, 184)
        statusBarAdvPanel1.Size = New Size(216, 48)
        
        ' Configure panel type
        statusBarAdvPanel1.PanelType = StatusBarAdvPanelType.LongDate
        
        ' Set background with gradient
        statusBarAdvPanel1.BackgroundColor = New BrushInfo(
            GradientStyle.BackwardDiagonal,
            Color.PaleVioletRed,
            Color.PeachPuff
        )
        
        ' Set border
        statusBarAdvPanel1.BorderColor = Color.Black
        
        ' Set alignment
        statusBarAdvPanel1.HAlign = HorzFlowAlign.Left
        
        ' Add to form
        Me.Controls.Add(statusBarAdvPanel1)
    End Sub
End Class
```

### Adding Panel to StatusBarAdv

StatusBarAdvPanel is typically used within a StatusBarAdv control:

**C#:**
```csharp
private void AddPanelToStatusBar()
{
    // Assuming statusBarAdv1 already exists on the form
    
    // Create date panel
    var datePanel = new StatusBarAdvPanel
    {
        PanelType = StatusBarAdvPanelType.ShortDate,
        Size = new Size(100, 25),
        HAlign = HorzFlowAlign.Right,
        BackgroundColor = new BrushInfo(Color.LightSteelBlue)
    };
    
    // Create time panel
    var timePanel = new StatusBarAdvPanel
    {
        PanelType = StatusBarAdvPanelType.ShortTime,
        Size = new Size(80, 25),
        HAlign = HorzFlowAlign.Right,
        BackgroundColor = new BrushInfo(Color.LightSteelBlue)
    };
    
    // Add panels to StatusBarAdv
    statusBarAdv1.Controls.Add(datePanel);
    statusBarAdv1.Controls.Add(timePanel);
}
```

**VB.NET:**
```vb
Private Sub AddPanelToStatusBar()
    ' Assuming statusBarAdv1 already exists on the form
    
    ' Create date panel
    Dim datePanel = New StatusBarAdvPanel With {
        .PanelType = StatusBarAdvPanelType.ShortDate,
        .Size = New Size(100, 25),
        .HAlign = HorzFlowAlign.Right,
        .BackgroundColor = New BrushInfo(Color.LightSteelBlue)
    }
    
    ' Create time panel
    Dim timePanel = New StatusBarAdvPanel With {
        .PanelType = StatusBarAdvPanelType.ShortTime,
        .Size = New Size(80, 25),
        .HAlign = HorzFlowAlign.Right,
        .BackgroundColor = New BrushInfo(Color.LightSteelBlue)
    }
    
    ' Add panels to StatusBarAdv
    statusBarAdv1.Controls.Add(datePanel)
    statusBarAdv1.Controls.Add(timePanel)
End Sub
```

## Complete Application Example

**C#:**
```csharp
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Drawing;
using System.Drawing;
using System.Windows.Forms;

public class StatusPanelDemoForm : Form
{
    private StatusBarAdv statusBarAdv1;
    private StatusBarAdvPanel statusPanel;
    private StatusBarAdvPanel datePanel;
    private StatusBarAdvPanel timePanel;
    private StatusBarAdvPanel capsLockPanel;
    
    public StatusPanelDemoForm()
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
            BackColor = Color.WhiteSmoke,
            Height = 30
        };
        
        // Create status message panel
        statusPanel = new StatusBarAdvPanel
        {
            Text = "Ready",
            Size = new Size(150, 25),
            HAlign = HorzFlowAlign.Left,
            Alignment = HorizontalAlignment.Left,
            BackgroundColor = new BrushInfo(Color.LightBlue),
            BorderStyle = BorderStyle.FixedSingle,
            BorderColor = Color.Gray
        };
        
        // Create date panel
        datePanel = new StatusBarAdvPanel
        {
            PanelType = StatusBarAdvPanelType.ShortDate,
            Size = new Size(100, 25),
            HAlign = HorzFlowAlign.Right,
            BackgroundColor = new BrushInfo(Color.LightGreen)
        };
        
        // Create time panel
        timePanel = new StatusBarAdvPanel
        {
            PanelType = StatusBarAdvPanelType.ShortTime,
            Size = new Size(80, 25),
            HAlign = HorzFlowAlign.Right,
            BackgroundColor = new BrushInfo(Color.LightYellow)
        };
        
        // Create CapsLock indicator panel
        capsLockPanel = new StatusBarAdvPanel
        {
            PanelType = StatusBarAdvPanelType.CapsLockState,
            Size = new Size(70, 25),
            HAlign = HorzFlowAlign.Right,
            BackgroundColor = new BrushInfo(Color.LightCoral)
        };
        
        // Add panels to StatusBarAdv
        statusBarAdv1.Controls.Add(statusPanel);
        statusBarAdv1.Controls.Add(datePanel);
        statusBarAdv1.Controls.Add(timePanel);
        statusBarAdv1.Controls.Add(capsLockPanel);
        
        // Add StatusBarAdv to form
        this.Controls.Add(statusBarAdv1);
    }
    
    // Method to update status message
    public void UpdateStatus(string message)
    {
        if (statusPanel != null)
        {
            statusPanel.Text = message;
        }
    }
    
    private void InitializeComponent()
    {
        this.Text = "StatusBarAdvPanel Demo";
        this.Size = new Size(600, 400);
    }
}
```

## Next Steps

After setting up StatusBarAdvPanel, explore these guides:
- **[Panel Types and Behavior](panel-types-and-behavior.md)** - Configure different panel types
- **[Appearance and Styling](appearance-styling.md)** - Customize panel appearance
- **[Text and Marquee](text-and-marquee.md)** - Implement marquee text animation

## Troubleshooting

### StatusBarAdvPanel Not Visible in Toolbox

**Solutions:**
1. **Verify assembly references** - Ensure Syncfusion.Tools.Windows.dll is referenced
2. **Rebuild solution** - Build → Rebuild Solution
3. **Reset toolbox** - Right-click Toolbox → Reset Toolbox
4. **Manually add to toolbox** - Right-click Toolbox → Choose Items → Browse to Syncfusion.Tools.Windows.dll
5. **Check .NET Framework version** - Ensure assembly version matches project target framework

### Panel Not Displaying on Form

**Solutions:**
1. **Check Location and Size** - Ensure panel is within form boundaries
2. **Verify BackColor** - Make sure background color contrasts with form
3. **Check Visible property** - Ensure `Visible = true`
4. **Add to correct parent** - Confirm panel is added to correct container (form or StatusBarAdv)
5. **Verify z-order** - Call `panel.BringToFront()` if needed

### Designer Shows Errors

**Solutions:**
1. **Clean and rebuild** - Build → Clean Solution, then Rebuild
2. **Check assembly versions** - Ensure all Syncfusion assemblies are same version
3. **Remove and re-add reference** - Remove assembly reference and add it again
4. **Check licensing** - Ensure valid Syncfusion license

### Namespace Not Found Error

**Solutions:**
1. **Add using statement** - Include `using Syncfusion.Windows.Forms.Tools;`
2. **Verify assembly reference** - Check that Syncfusion.Tools.Windows is referenced
3. **Check assembly path** - Ensure assembly path is correct
4. **Rebuild project** - Clean and rebuild the solution
5. **Check NuGet package** - If using NuGet, ensure package is properly restored

### Gradient Background Not Showing

**Solutions:**
1. **Use BrushInfo** - Set BackgroundColor using `new BrushInfo(GradientStyle, Color1, Color2)`
2. **Check GradientStyle** - Ensure valid gradient style is specified
3. **Verify colors** - Use contrasting colors for visible gradient
4. **Check BackgroundColor.Style** - Should be set to BrushStyle.Gradient (automatic with constructor)
