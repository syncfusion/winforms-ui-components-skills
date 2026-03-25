# Getting Started with SplashControl

This guide covers the initial setup and basic implementation of the SplashControl component in Windows Forms applications.

## Assembly Deployment

### Required Assemblies

To use the SplashControl, you need to reference the following assemblies:

- **Syncfusion.Shared.Base.dll**
- **Syncfusion.Tools.Windows.dll**

These assemblies are automatically added when you drag and drop the SplashControl from the toolbox.

### NuGet Package Installation

Install the SplashControl via NuGet Package Manager:

```powershell
Install-Package Syncfusion.Tools.Windows
```

Or use the NuGet Package Manager UI in Visual Studio:
1. Right-click on your project → **Manage NuGet Packages**
2. Search for **Syncfusion.Tools.Windows**
3. Click **Install**

For more details, refer to [How to install NuGet packages](https://help.syncfusion.com/windowsforms/installation/install-nuget-packages).

## Creating the Project

Create a new Windows Forms project in Visual Studio:
1. Open Visual Studio
2. **File → New → Project**
3. Select **Windows Forms App (.NET Framework)** or **Windows Forms App**
4. Name your project and click **Create**

## Adding SplashControl Through Designer

The designer approach is the quickest way to add and configure SplashControl.

### Step 1: Add Control from Toolbox

1. Open your main form in the designer
2. Locate **SplashControl** in the toolbox (under Syncfusion Controls)
3. Drag and drop it onto your form

The control appears in the **component tray** at the bottom of the designer (not on the form surface, as it's a non-visual component).

![SplashControl in component tray](../images/splash-control-component-tray.png)

**Note:** The required assembly references are added automatically.

### Step 2: Configure Properties

Select the SplashControl in the component tray and configure basic properties in the Properties window:

**Essential properties to set:**

```
SplashImage: (Browse to select your splash image)
TimerInterval: 3000 (display for 3 seconds)
AutoMode: True (auto-display on form load)
HostForm: (Select your form from dropdown)
DesktopAlignment: Center
```

### Step 3: Set AutoMode

The **AutoMode** property controls how the splash screen is invoked:

- **True:** Splash screen automatically displays during the parent form's Load event
- **False:** You must manually call `ShowSplash()` method

For most scenarios, set this to **True** for automatic display.

### Step 4: Preview at Design Time

You can preview the splash screen without running the application:

1. Click the **Smart Tag** (small arrow) on the SplashControl
2. Select **Preview Splash**

This shows how your splash screen will appear at runtime.

### Step 5: Run the Application

Press **F5** to run your application. The splash screen displays automatically if AutoMode is True.

### Step 6: Manual Display (Optional)

If AutoMode is False, display the splash screen manually:

```csharp
// In a button click or other event
private void ShowSplashButton_Click(object sender, EventArgs e)
{
    splashControl1.ShowSplash(true);
}
```

### Step 7: Handle Events

Subscribe to the **SplashClosed** event to perform actions after the splash closes:

```csharp
private void InitializeComponent()
{
    // ... other initialization code ...
    
    this.splashControl1.SplashClosed += SplashControl1_SplashClosed;
}

private void SplashControl1_SplashClosed(object sender, EventArgs e)
{
    // Perform initialization after splash closes
    LoadApplicationData();
    InitializeComponents();
}
```

### Step 8: Hide Splash Programmatically

Cancel or hide the splash screen while it's displaying:

```csharp
private void CancelSplashButton_Click(object sender, EventArgs e)
{
    splashControl1.HideSplash();
}
```

## Adding SplashControl Through Code

For more control or dynamic scenarios, create the SplashControl programmatically.

### Step 1: Create Project

Create a new C# or VB.NET Windows Forms application in Visual Studio.

### Step 2: Add Assembly References

Manually add references to:
- `Syncfusion.Shared.Base.dll`
- `Syncfusion.Tools.Windows.dll`

**Steps:**
1. Right-click **References** in Solution Explorer
2. Click **Add Reference**
3. Browse to Syncfusion installation folder or use NuGet

### Step 3: Declare and Initialize

Add the using statement and declare the control:

**C# Example:**

```csharp
using Syncfusion.Windows.Forms.Tools;

public partial class Form1 : Form
{
    private SplashControl splashControl1;

    public Form1()
    {
        InitializeComponent();
        InitializeSplashControl();
    }

    private void InitializeSplashControl()
    {
        this.splashControl1 = new SplashControl();
        this.SuspendLayout();
        
        // Configure properties (see Step 4)
        
        this.ResumeLayout(false);
    }
}
```

**VB.NET Example:**

```vb
Imports Syncfusion.Windows.Forms.Tools

Public Class Form1
    Friend WithEvents splashControl1 As SplashControl

    Public Sub New()
        InitializeComponent()
        InitializeSplashControl()
    End Sub

    Private Sub InitializeSplashControl()
        Me.splashControl1 = New SplashControl()
        Me.SuspendLayout()
        
        ' Configure properties (see Step 4)
        
        Me.ResumeLayout(False)
    End Sub
End Class
```

### Step 4: Configure Properties

Set essential properties for the SplashControl:

**C# Example:**

```csharp
private void InitializeSplashControl()
{
    this.splashControl1 = new SplashControl();
    
    // Basic configuration
    this.splashControl1.CustomSplashPanel = null;
    this.splashControl1.HostForm = this;
    this.splashControl1.HostFormWindowState = FormWindowState.Normal;
    this.splashControl1.TimerInterval = 3000;
    
    // Set splash image
    this.splashControl1.SplashImage = Image.FromFile("splash.png");
    // Or from resources:
    // this.splashControl1.SplashImage = Properties.Resources.SplashImage;
    
    // Enable automatic display
    this.splashControl1.AutoMode = true;
    
    // Center on screen
    this.splashControl1.DesktopAlignment = SplashAlignment.Center;
    
    // Optional: Enable animation
    this.splashControl1.ShowAnimation = true;
}
```

**VB.NET Example:**

```vb
Private Sub InitializeSplashControl()
    Me.splashControl1 = New SplashControl()
    
    ' Basic configuration
    Me.splashControl1.CustomSplashPanel = Nothing
    Me.splashControl1.HostForm = Me
    Me.splashControl1.HostFormWindowState = FormWindowState.Normal
    Me.splashControl1.TimerInterval = 3000
    
    ' Set splash image
    Me.splashControl1.SplashImage = Image.FromFile("splash.png")
    ' Or from resources:
    ' Me.splashControl1.SplashImage = My.Resources.SplashImage
    
    ' Enable automatic display
    Me.splashControl1.AutoMode = True
    
    ' Center on screen
    Me.splashControl1.DesktopAlignment = SplashAlignment.Center
    
    ' Optional: Enable animation
    Me.splashControl1.ShowAnimation = True
End Sub
```

### Step 5: Run the Application

Press **F5** to run. The splash screen displays automatically for the configured duration.

## Common Getting Started Scenarios

### Scenario 1: Simple Auto-Display Splash

The most basic implementation:

```csharp
public Form1()
{
    InitializeComponent();
    
    splashControl1 = new SplashControl();
    splashControl1.HostForm = this;
    splashControl1.SplashImage = Properties.Resources.CompanyLogo;
    splashControl1.TimerInterval = 3000;
    splashControl1.AutoMode = true;
}
```

### Scenario 2: Manual Control with Button

Display splash on demand:

```csharp
public Form1()
{
    InitializeComponent();
    
    splashControl1 = new SplashControl();
    splashControl1.HostForm = this;
    splashControl1.SplashImage = Properties.Resources.CompanyLogo;
    splashControl1.TimerInterval = 3000;
    splashControl1.AutoMode = false; // Manual control
}

private void btnShowSplash_Click(object sender, EventArgs e)
{
    splashControl1.ShowSplash(false); // Non-modal
}
```

### Scenario 3: Splash with Event Handling

Handle splash lifecycle:

```csharp
public Form1()
{
    InitializeComponent();
    
    splashControl1 = new SplashControl();
    splashControl1.HostForm = this;
    splashControl1.SplashImage = Properties.Resources.CompanyLogo;
    splashControl1.TimerInterval = 3000;
    splashControl1.AutoMode = true;
    
    // Subscribe to events
    splashControl1.SplashDisplayed += (s, e) => 
    {
        // Splash is now visible
        StartBackgroundTasks();
    };
    
    splashControl1.SplashClosed += (s, e) => 
    {
        // Splash has closed
        ShowWelcomeMessage();
    };
}
```

## Troubleshooting

### Splash Image Not Displaying

**Problem:** The splash screen shows but the image is blank or missing.

**Solutions:**
- Verify the image file path is correct
- Ensure the image is added to project resources
- Check that Build Action is set to "Embedded Resource" or use `Properties.Resources`
- Verify image format is supported (PNG, JPG, BMP)

### Splash Not Auto-Displaying

**Problem:** Splash screen doesn't appear when the application starts.

**Solutions:**
- Confirm `AutoMode` property is set to `True`
- Verify `HostForm` property is set to the correct form
- Ensure SplashControl is initialized before the form is shown
- Check that `SplashImage` or `CustomSplashPanel` is configured

### Assembly Reference Errors

**Problem:** Compiler errors about missing Syncfusion types.

**Solutions:**
- Add references to `Syncfusion.Shared.Base.dll` and `Syncfusion.Tools.Windows.dll`
- Install the NuGet package: `Syncfusion.Tools.Windows`
- Ensure the using/import statement is present: `using Syncfusion.Windows.Forms.Tools;`

## Next Steps

Now that you have a basic splash screen working:

- **Configure timing:** Learn about AutoMode and timer settings in [automode-timing.md](automode-timing.md)
- **Customize appearance:** Add animation and transparency in [image-animation.md](image-animation.md)
- **Position splash:** Control screen placement in [alignment-positioning.md](alignment-positioning.md)
- **Create custom panels:** Design rich splash panels in [splashpanel-integration.md](splashpanel-integration.md)
