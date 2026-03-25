# Getting Started with StatusBarAdv

This guide covers installation, setup, and basic configuration of the StatusBarAdv control in Windows Forms applications.

## When to Read This

Read this reference when:
- Installing StatusBarAdv for the first time
- Setting up assemblies and dependencies
- Creating your first status bar using designer or code
- Adding StatusBarAdvPanel controls
- Troubleshooting installation or setup issues

## Assembly Requirements

StatusBarAdv requires the following Syncfusion assemblies:

**Required Assemblies:**
1. **Syncfusion.Tools.Windows.dll** - Contains StatusBarAdv and StatusBarAdvPanel
2. **Syncfusion.Shared.Base.dll** - Shared base functionality

**Namespace:**
```csharp
using Syncfusion.Windows.Forms.Tools;
```

## NuGet Installation

### Package Manager Console

```powershell
Install-Package Syncfusion.Tools.Windows
```

This will install both required assemblies (Tools.Windows and Shared.Base).

### NuGet Package Manager UI

1. Right-click your project in Solution Explorer
2. Select "Manage NuGet Packages..."
3. Click the "Browse" tab
4. Search for **"Syncfusion.Tools.Windows"**
5. Select the package and click "Install"
6. Accept the license agreement

## Manual Assembly Reference

If not using NuGet, add assemblies manually:

1. Right-click **References** in Solution Explorer
2. Click **Add Reference...**
3. Click **Browse** button
4. Navigate to Syncfusion installation folder:
   - Default: `C:\Program Files (x86)\Syncfusion\Essential Studio\Windows\<version>\precompiledassemblies\<framework version>`
5. Select **Syncfusion.Tools.Windows.dll** and **Syncfusion.Shared.Base.dll**
6. Click **OK**

## Designer-Based Setup

### Adding StatusBarAdv from Toolbox

1. Open your Windows Forms designer
2. Expand **Syncfusion Controls** in the Toolbox
3. Drag **StatusBarAdv** onto your form
4. The control will dock to the bottom of the form automatically

**Note:** If StatusBarAdv is not visible in Toolbox, rebuild your solution or manually add the assemblies.

### Adding StatusBarAdvPanel Controls

**Method 1: Designer Controls**
1. Drag **StatusBarAdvPanel** from Toolbox
2. Drop it onto the StatusBarAdv control
3. Set panel properties in Property Grid

**Method 2: Panels Collection Editor**
1. Select the StatusBarAdv control
2. In Property Grid, find **Panels** property
3. Click the ellipsis button (...)
4. **StatusBarAdvPanel Collection Editor** opens
5. Click **Add** to create panels
6. Configure panel properties in the right panel
7. Click **OK** to apply

### Designer Properties

Set these key properties in the Property Grid:

| Property | Suggested Value | Description |
|----------|----------------|-------------|
| `Dock` | Bottom | Docks status bar to bottom of form |
| `BackColor` | LightSteelBlue | Background color |
| `BorderStyle` | FixedSingle | Adds border around status bar |
| `SizingGrip` | True | Shows resize grip (for resizable forms) |

## Programmatic Creation

### Basic Implementation

**C#:**
```csharp
using Syncfusion.Windows.Forms.Tools;
using System.Drawing;
using System.Windows.Forms;

public partial class MyForm : Form
{
    private StatusBarAdv statusBarAdv1;
    
    public MyForm()
    {
        InitializeComponent();
        CreateStatusBar();
    }
    
    private void CreateStatusBar()
    {
        // Initialize StatusBarAdv
        statusBarAdv1 = new StatusBarAdv
        {
            Dock = DockStyle.Bottom,
            Name = "statusBarAdv1",
            BackColor = Color.LightSteelBlue,
            BorderColor = Color.Black,
            BorderStyle = BorderStyle.FixedSingle
        };
        
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

Public Partial Class MyForm
    Inherits Form
    
    Private statusBarAdv1 As StatusBarAdv
    
    Public Sub New()
        InitializeComponent()
        CreateStatusBar()
    End Sub
    
    Private Sub CreateStatusBar()
        ' Initialize StatusBarAdv
        statusBarAdv1 = New StatusBarAdv With {
            .Dock = DockStyle.Bottom,
            .Name = "statusBarAdv1",
            .BackColor = Color.LightSteelBlue,
            .BorderColor = Color.Black,
            .BorderStyle = BorderStyle.FixedSingle
        }
        
        ' Add to form
        Me.Controls.Add(statusBarAdv1)
    End Sub
End Class
```

### Adding StatusBarAdvPanel Controls

**C#:**
```csharp
private void CreateStatusBarWithPanels()
{
    // Create StatusBarAdv
    statusBarAdv1 = new StatusBarAdv
    {
        Dock = DockStyle.Bottom,
        BackColor = Color.LightSteelBlue,
        BorderStyle = BorderStyle.FixedSingle
    };
    
    // Create panels
    StatusBarAdvPanel panel1 = new StatusBarAdvPanel
    {
        PanelType = StatusBarAdvPanelType.CurrentCulture,
        Size = new Size(120, 27)
    };
    
    StatusBarAdvPanel panel2 = new StatusBarAdvPanel
    {
        PanelType = StatusBarAdvPanelType.ShortDate,
        Size = new Size(100, 27)
    };
    
    StatusBarAdvPanel panel3 = new StatusBarAdvPanel
    {
        PanelType = StatusBarAdvPanelType.ShortTime,
        Size = new Size(100, 27)
    };
    
    // Add panels to StatusBarAdv
    statusBarAdv1.Controls.Add(panel1);
    statusBarAdv1.Controls.Add(panel2);
    statusBarAdv1.Controls.Add(panel3);
    
    // Add to form
    this.Controls.Add(statusBarAdv1);
}
```

**VB.NET:**
```vbnet
Private Sub CreateStatusBarWithPanels()
    ' Create StatusBarAdv
    statusBarAdv1 = New StatusBarAdv With {
        .Dock = DockStyle.Bottom,
        .BackColor = Color.LightSteelBlue,
        .BorderStyle = BorderStyle.FixedSingle
    }
    
    ' Create panels
    Dim panel1 As New StatusBarAdvPanel With {
        .PanelType = StatusBarAdvPanelType.CurrentCulture,
        .Size = New Size(120, 27)
    }
    
    Dim panel2 As New StatusBarAdvPanel With {
        .PanelType = StatusBarAdvPanelType.ShortDate,
        .Size = New Size(100, 27)
    }
    
    Dim panel3 As New StatusBarAdvPanel With {
        .PanelType = StatusBarAdvPanelType.ShortTime,
        .Size = New Size(100, 27)
    }
    
    ' Add panels to StatusBarAdv
    statusBarAdv1.Controls.Add(panel1)
    statusBarAdv1.Controls.Add(panel2)
    statusBarAdv1.Controls.Add(panel3)
    
    ' Add to form
    Me.Controls.Add(statusBarAdv1)
End Sub
```

### Complete Application Status Bar Example

**C#:**
```csharp
public partial class MainApplicationForm : Form
{
    private StatusBarAdv statusBar;
    private StatusBarAdvPanel statusPanel;
    private StatusBarAdvPanel userPanel;
    private StatusBarAdvPanel datePanel;
    private StatusBarAdvPanel timePanel;
    
    public MainApplicationForm()
    {
        InitializeComponent();
        SetupApplicationStatusBar();
    }
    
    private void SetupApplicationStatusBar()
    {
        // Create status bar
        statusBar = new StatusBarAdv
        {
            Dock = DockStyle.Bottom,
            BackColor = Color.FromArgb(240, 245, 250),
            BorderStyle = BorderStyle.FixedSingle,
            BorderColor = Color.FromArgb(200, 210, 220),
            SizingGrip = true,
            Height = 28
        };
        
        // Status message panel
        statusPanel = new StatusBarAdvPanel
        {
            Text = "Ready",
            Size = new Size(150, 25)
        };
        
        // User info panel
        userPanel = new StatusBarAdvPanel
        {
            Text = $"User: {Environment.UserName}",
            Size = new Size(150, 25)
        };
        
        // Date panel
        datePanel = new StatusBarAdvPanel
        {
            PanelType = StatusBarAdvPanelType.ShortDate,
            Size = new Size(100, 25)
        };
        
        // Time panel
        timePanel = new StatusBarAdvPanel
        {
            PanelType = StatusBarAdvPanelType.ShortTime,
            Size = new Size(80, 25)
        };
        
        // Add panels
        statusBar.Controls.Add(statusPanel);
        statusBar.Controls.Add(userPanel);
        statusBar.Controls.Add(datePanel);
        statusBar.Controls.Add(timePanel);
        
        // Add to form
        this.Controls.Add(statusBar);
    }
    
    // Update status message
    public void UpdateStatus(string message)
    {
        if (statusPanel != null)
        {
            statusPanel.Text = message;
        }
    }
}
```

**VB.NET:**
```vbnet
Public Partial Class MainApplicationForm
    Inherits Form
    
    Private statusBar As StatusBarAdv
    Private statusPanel As StatusBarAdvPanel
    Private userPanel As StatusBarAdvPanel
    Private datePanel As StatusBarAdvPanel
    Private timePanel As StatusBarAdvPanel
    
    Public Sub New()
        InitializeComponent()
        SetupApplicationStatusBar()
    End Sub
    
    Private Sub SetupApplicationStatusBar()
        ' Create status bar
        statusBar = New StatusBarAdv With {
            .Dock = DockStyle.Bottom,
            .BackColor = Color.FromArgb(240, 245, 250),
            .BorderStyle = BorderStyle.FixedSingle,
            .BorderColor = Color.FromArgb(200, 210, 220),
            .SizingGrip = True,
            .Height = 28
        }
        
        ' Status message panel
        statusPanel = New StatusBarAdvPanel With {
            .Text = "Ready",
            .Size = New Size(150, 25)
        }
        
        ' User info panel
        userPanel = New StatusBarAdvPanel With {
            .Text = $"User: {Environment.UserName}",
            .Size = New Size(150, 25)
        }
        
        ' Date panel
        datePanel = New StatusBarAdvPanel With {
            .PanelType = StatusBarAdvPanelType.ShortDate,
            .Size = New Size(100, 25)
        }
        
        ' Time panel
        timePanel = New StatusBarAdvPanel With {
            .PanelType = StatusBarAdvPanelType.ShortTime,
            .Size = New Size(80, 25)
        }
        
        ' Add panels
        statusBar.Controls.Add(statusPanel)
        statusBar.Controls.Add(userPanel)
        statusBar.Controls.Add(datePanel)
        statusBar.Controls.Add(timePanel)
        
        ' Add to form
        Me.Controls.Add(statusBar)
    End Sub
    
    ' Update status message
    Public Sub UpdateStatus(message As String)
        If statusPanel IsNot Nothing Then
            statusPanel.Text = message
        End If
    End Sub
End Class
```

## Next Steps

After creating basic StatusBarAdv:

1. **Configure Panels and Layout** → Read: [panels-and-layout.md](panels-and-layout.md)
   - Customize panel spacing
   - Set panel alignment
   - Use custom layout bounds

2. **Customize Appearance** → Read: [appearance-styling.md](appearance-styling.md)
   - Apply gradient backgrounds
   - Set Metro colors
   - Configure sizing grip

3. **Apply Themes and Borders** → Read: [borders-and-themes.md](borders-and-themes.md)
   - Use Office2016 themes
   - Configure border styles
   - Enable themed backgrounds

## Troubleshooting

### Issue: StatusBarAdv Not Visible in Toolbox

**Solutions:**
1. Verify Syncfusion.Tools.Windows assembly is referenced
2. Rebuild the solution (Build → Rebuild Solution)
3. Right-click Toolbox → "Choose Items..." → Browse to assembly location
4. Close and reopen Visual Studio
5. Check that assembly version matches your .NET framework version

### Issue: Designer Shows Errors

**Solutions:**
1. Clean and rebuild solution
2. Verify all required assemblies are referenced (Tools.Windows and Shared.Base)
3. Check that assembly versions match (don't mix versions)
4. Close designer, rebuild, then reopen designer

### Issue: Namespace Not Found

**Solutions:**
1. Add `using Syncfusion.Windows.Forms.Tools;` (C#) or `Imports Syncfusion.Windows.Forms.Tools` (VB.NET)
2. Verify Syncfusion.Tools.Windows.dll is referenced in project
3. Check assembly reference properties - "Copy Local" should be True
4. Verify correct Syncfusion version is installed
5. For .NET Core/.NET 5+, ensure you're using compatible Syncfusion version

### Issue: Panels Not Displaying

**Solutions:**
1. Verify panels are added to statusBarAdv1.Controls (not Panels collection)
2. Set panel Size property explicitly
3. Check Dock property of StatusBarAdv is set to Bottom
4. Ensure form has sufficient height to display status bar
5. Verify panel PanelType is set correctly

### Issue: Panels Collection Editor Title Issue

**Note:** In .NET Core/.NET 5+, the collection editor may display "ControlProxy`1 CollectionEditor" instead of "StatusBarAdvPanel CollectionEditor". This is a known Visual Studio issue ([GitHub #14049](https://github.com/dotnet/winforms/issues/14049)) and does not affect functionality. The editor works normally despite the title display.

### Issue: Status Bar Too Tall or Short

**Solutions:**
1. Set Height property explicitly: `statusBarAdv1.Height = 28;`
2. Enable AutoSize: `statusBarAdv1.AutoSize = true;`
3. Use AutoHeightControls to adjust panel heights automatically
4. Check panel Size.Height values match desired height
