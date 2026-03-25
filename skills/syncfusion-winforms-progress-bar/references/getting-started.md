# Getting Started with ProgressBarAdv

This guide covers the essential steps to add, configure, and use the Syncfusion Windows Forms ProgressBarAdv control in your application.

## Assembly Deployment

Before using the ProgressBarAdv control, you need to add the required assembly references to your project.

### Required Assemblies

The ProgressBarAdv control requires the following assembly:

- `Syncfusion.Shared.Base.dll`

### NuGet Package Installation

You can install the required NuGet package using the NuGet Package Manager:

1. Right-click on your project in Solution Explorer
2. Select "Manage NuGet Packages..."
3. Search for "Syncfusion.Windows.Forms"
4. Install the package

**Package Manager Console:**
```powershell
Install-Package Syncfusion.Windows.Forms
```

**NuGet CLI:**
```bash
nuget install Syncfusion.Windows.Forms
```

For more details on installing NuGet packages in Windows Forms applications, refer to the [Syncfusion installation documentation](https://help.syncfusion.com/windowsforms/installation/install-nuget-packages).

## Adding Control via Designer

The ProgressBarAdv control can be added to your form using the Visual Studio designer.

### Steps to Add via Designer

1. Open your Windows Forms form in the designer
2. Locate the **ProgressBarAdv** control in the Toolbox
3. Drag and drop it onto your form

When added via the designer, the required assembly reference (`Syncfusion.Shared.Base.dll`) is added automatically.

![Adding ProgressBarAdv via designer](../assets/Toolbox.png)

![ProgressBarAdv added to form](../assets/GettingStarted-img1.png)

### Configuring Properties in Designer

After adding the control, you can configure its properties using the Properties window:

1. Select the ProgressBarAdv control
2. Open the Properties window (F4)
3. Set properties like `Value`, `Minimum`, `Maximum`, `TextStyle`, etc.

## Adding Control Manually in C#

You can also add the ProgressBarAdv control programmatically in your C# code.

### Step 1: Add Assembly References

If you're adding the control manually, ensure the required assembly is referenced in your project (see [Assembly Deployment](#assembly-deployment) section).

### Step 2: Import Namespace

Add the following using directive at the top of your code file:

```csharp
using Syncfusion.Windows.Forms.Tools;
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms.Tools
```

### Step 3: Create ProgressBarAdv Instance

Create a ProgressBarAdv instance and add it to your form's control collection:

```csharp
ProgressBarAdv progressBarAdv1 = new ProgressBarAdv();
progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016Colorful;
progressBarAdv1.Value = 60;
this.Controls.Add(progressBarAdv1);
```

**VB.NET:**
```vb
Dim progressBarAdv1 As ProgressBarAdv = New ProgressBarAdv()
progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016Colorful
progressBarAdv1.Value = 60
Me.Controls.Add(progressBarAdv1)
```

![ProgressBarAdv created programmatically](../assets/GettingStarted-img2.png)

### Step 4: Configure Position and Size

Set the location and size of the progress bar:

```csharp
progressBarAdv1.Location = new Point(20, 20);
progressBarAdv1.Size = new Size(300, 30);
```

**VB.NET:**
```vb
progressBarAdv1.Location = New Point(20, 20)
progressBarAdv1.Size = New Size(300, 30)
```

## Setting Value Range

Configure the minimum, maximum, and current value of the progress bar.

### Basic Value Configuration

```csharp
// Set the range
this.progressBarAdv1.Minimum = 0;
this.progressBarAdv1.Maximum = 100;

// Set current value
this.progressBarAdv1.Value = 50;
```

**VB.NET:**
```vb
' Set the range
Me.progressBarAdv1.Minimum = 0
Me.progressBarAdv1.Maximum = 100

' Set current value
Me.progressBarAdv1.Value = 50
```

**Important Notes:**
- Default `Minimum` is 0
- Default `Maximum` is 100
- `Value` must be between `Minimum` and `Maximum`
- Setting `Value` outside the range will throw an exception

### Custom Range

You can use custom ranges for specific scenarios:

```csharp
// For file sizes (0-1000 MB)
progressBarAdv1.Minimum = 0;
progressBarAdv1.Maximum = 1000;
progressBarAdv1.Value = 450; // 450 MB transferred
progressBarAdv1.TextStyle = ProgressBarTextStyles.Value;
```

## Configure Text Format

The ProgressBarAdv provides three text format options via the `TextStyle` property.

### Value Format

Displays the current value out of maximum value (e.g., "60/100"):

```csharp
this.progressBarAdv1.TextStyle = ProgressBarTextStyles.Value;
```

**VB.NET:**
```vb
Me.progressBarAdv1.TextStyle = ProgressBarTextStyles.Value
```

![Value format display](../assets/GettingStarted-img3.png)

**Use Cases:**
- File transfer (e.g., "450/1000 MB")
- Item processing (e.g., "25/100 items")
- Download progress with specific units

### Percentage Format

Displays progress as a percentage (e.g., "60%"):

```csharp
this.progressBarAdv1.TextStyle = ProgressBarTextStyles.Percentage;
```

**VB.NET:**
```vb
Me.progressBarAdv1.TextStyle = ProgressBarTextStyles.Percentage
```

![Percentage format display](../assets/GettingStarted-img5.png)

**Use Cases:**
- General progress indication
- Completion status
- Most common and user-friendly format

### Custom Format

Displays custom text set via the `CustomText` property:

```csharp
this.progressBarAdv1.CustomText = "Loading";
this.progressBarAdv1.TextStyle = ProgressBarTextStyles.Custom;
```

**VB.NET:**
```vb
Me.progressBarAdv1.CustomText = "Loading"
Me.progressBarAdv1.TextStyle = ProgressBarTextStyles.Custom
```

![Custom text format display](../assets/GettingStarted-img6.png)

**Use Cases:**
- Status messages ("Loading...", "Processing...", "Please wait...")
- Operation names ("Installing updates", "Copying files")
- Dynamic text that changes based on progress

### Dynamic Custom Text Example

```csharp
// Update custom text based on progress
private void UpdateProgressText()
{
    progressBarAdv1.TextStyle = ProgressBarTextStyles.Custom;
    
    if (progressBarAdv1.Value < 33)
    {
        progressBarAdv1.CustomText = "Starting...";
    }
    else if (progressBarAdv1.Value < 66)
    {
        progressBarAdv1.CustomText = "In Progress...";
    }
    else if (progressBarAdv1.Value < 100)
    {
        progressBarAdv1.CustomText = "Almost Done...";
    }
    else
    {
        progressBarAdv1.CustomText = "Complete!";
    }
}
```

## Configure Text Alignment

The `TextAlignment` property controls the horizontal alignment of the text within the progress bar.

### Center Alignment

Text aligned to the center (default):

```csharp
this.progressBarAdv1.TextAlignment = TextAlignment.Center;
```

**VB.NET:**
```vb
Me.progressBarAdv1.TextAlignment = TextAlignment.Center
```

![Center alignment](../assets/GettingStarted-img8.png)

**Best for:**
- Most common use case
- Balanced appearance
- General purpose progress bars

### Left Alignment

Text aligned to the left:

```csharp
this.progressBarAdv1.TextAlignment = TextAlignment.Left;
```

**VB.NET:**
```vb
Me.progressBarAdv1.TextAlignment = TextAlignment.Left
```

![Left alignment](../assets/GettingStarted-img4.png)

**Best for:**
- Left-to-right reading patterns
- Consistent with other left-aligned controls
- When progress bar is part of a left-aligned layout

### Right Alignment

Text aligned to the right:

```csharp
this.progressBarAdv1.TextAlignment = TextAlignment.Right;
```

**VB.NET:**
```vb
Me.progressBarAdv1.TextAlignment = TextAlignment.Right
```

![Right alignment](../assets/GettingStarted-img7.png)

**Best for:**
- Right-to-left languages
- Right-aligned layouts
- Alignment with other right-aligned controls

## Complete Example

Here's a complete example that puts everything together:

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class MainForm : Form
{
    private ProgressBarAdv progressBarAdv1;
    private Button startButton;
    private System.Windows.Forms.Timer progressTimer;
    
    public MainForm()
    {
        InitializeComponent();
        InitializeProgressBar();
    }
    
    private void InitializeProgressBar()
    {
        // Create ProgressBarAdv
        progressBarAdv1 = new ProgressBarAdv();
        progressBarAdv1.Location = new Point(20, 20);
        progressBarAdv1.Size = new Size(400, 30);
        
        // Configure range
        progressBarAdv1.Minimum = 0;
        progressBarAdv1.Maximum = 100;
        progressBarAdv1.Value = 0;
        
        // Configure appearance
        progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016Colorful;
        progressBarAdv1.TextStyle = ProgressBarTextStyles.Percentage;
        progressBarAdv1.TextAlignment = TextAlignment.Center;
        
        // Add to form
        this.Controls.Add(progressBarAdv1);
        
        // Create start button
        startButton = new Button();
        startButton.Text = "Start Operation";
        startButton.Location = new Point(20, 60);
        startButton.Click += StartButton_Click;
        this.Controls.Add(startButton);
        
        // Initialize timer for simulating progress
        progressTimer = new System.Windows.Forms.Timer();
        progressTimer.Interval = 100; // 100ms
        progressTimer.Tick += ProgressTimer_Tick;
    }
    
    private void StartButton_Click(object sender, EventArgs e)
    {
        progressBarAdv1.Value = 0;
        startButton.Enabled = false;
        progressTimer.Start();
    }
    
    private void ProgressTimer_Tick(object sender, EventArgs e)
    {
        if (progressBarAdv1.Value < progressBarAdv1.Maximum)
        {
            progressBarAdv1.Value += 5;
        }
        else
        {
            progressTimer.Stop();
            startButton.Enabled = true;
            MessageBox.Show("Operation complete!");
        }
    }
}
```

## Best Practices

### Value Bounds Checking

Always ensure the value is within the valid range:

```csharp
private void SetProgressValue(int newValue)
{
    if (newValue >= progressBarAdv1.Minimum && 
        newValue <= progressBarAdv1.Maximum)
    {
        progressBarAdv1.Value = newValue;
    }
}
```

### Responsive Progress Updates

Use proper threading to avoid UI freezing:

```csharp
private async Task PerformLongOperation()
{
    for (int i = 0; i <= 100; i++)
    {
        // Update progress on UI thread
        this.Invoke((Action)(() => 
        {
            progressBarAdv1.Value = i;
        }));
        
        // Simulate work
        await Task.Delay(50);
    }
}
```

### Clear Progress Indication

Choose the appropriate text style for your use case:
- **Percentage** - Best for most general purposes
- **Value** - When absolute numbers matter (file sizes, item counts)
- **Custom** - When you need descriptive status messages

## Next Steps

- **Text customization:** See [text-display.md](text-display.md) for advanced text formatting
- **Appearance:** See [appearance-styling.md](appearance-styling.md) for colors, gradients, and borders
- **Orientation:** See [orientation-layout.md](orientation-layout.md) for vertical progress bars
- **Themes:** See [themes.md](themes.md) for visual styling options
- **Events:** See [events-advanced.md](events-advanced.md) for event handling and custom rendering
