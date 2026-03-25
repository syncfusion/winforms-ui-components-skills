# Getting Started with Windows Forms Toggle Button

This guide walks you through setting up and adding a Toggle Button control to your Windows Forms application.

## Assembly Dependencies

Before you can use the Toggle Button control, you must add the following assemblies to your project:

- `Syncfusion.Grid.Base`
- `Syncfusion.Grid.Windows`
- `Syncfusion.Shared.Base`
- `Syncfusion.Shared.Windows`
- `Syncfusion.Tools.Base`
- `Syncfusion.Tools.Windows`

### Installation via NuGet

The recommended way to add Syncfusion assemblies is through NuGet packages:

```
Install-Package Syncfusion.Tools.Windows
```

For detailed NuGet installation instructions, refer to the Syncfusion Windows Forms installation guide.

## Adding Toggle Button via Designer

**Step 1: Create Project**
- Open Visual Studio
- Create a new Windows Forms Application project
- This will create Form1 with the default designer

**Step 2: Add Toggle Button from Toolbox**
- Open the Toolbox panel (View → Toolbox)
- Locate the **ToggleButton** control in the Syncfusion Windows Forms section
- Drag and drop it onto your form design view
- The required assemblies will be automatically added as references

**Step 3: Configure in Designer**
- Select the Toggle Button in the designer
- In the Properties panel, you can:
  - Set the Name property (e.g., "toggleButton1")
  - Set the Size and Location
  - Configure initial properties like ToggleState, Text
- Double-click to generate default event handlers

**Step 4: Run the Application**
- Press F5 or click the Run button
- Your form will display with the Toggle Button control

## Adding Toggle Button via Code

### Step 1: Add Namespace Import

In your Form1.cs file, add the following namespace:

```csharp
using Syncfusion.Windows.Forms.Tools;
```

### Step 2: Create and Configure the Toggle Button

In the Form1 constructor (after `InitializeComponent()`), add the following code:

```csharp
public Form1()
{
    InitializeComponent();
    
    // Create Toggle Button instance
    ToggleButton toggleButton = new ToggleButton();
    
    // Set position and size
    toggleButton.Location = new System.Drawing.Point(50, 50);
    toggleButton.Size = new System.Drawing.Size(100, 40);
    
    // Set name for reference
    toggleButton.Name = "toggleButton1";
    
    // Configure Active state
    toggleButton.ActiveState.Text = "ON";
    toggleButton.ActiveState.BackColor = Color.FromArgb(1, 115, 199);
    toggleButton.ActiveState.BorderColor = Color.FromArgb(1, 115, 199);
    toggleButton.ActiveState.ForeColor = Color.White;
    
    // Configure Inactive state
    toggleButton.InactiveState.Text = "OFF";
    toggleButton.InactiveState.BackColor = Color.White;
    toggleButton.InactiveState.BorderColor = Color.FromArgb(150, 150, 150);
    toggleButton.InactiveState.ForeColor = Color.FromArgb(80, 80, 80);
    
    // Set initial state
    toggleButton.ToggleState = ToggleButtonState.Inactive;
    
    // Add to form controls
    this.Controls.Add(toggleButton);
}
```

### Visual Basic Equivalent

```vb
Imports Syncfusion.Windows.Forms.Tools

Public Class Form1
    Public Sub New()
        InitializeComponent()
        
        ' Create Toggle Button instance
        Dim toggleButton As ToggleButton = New ToggleButton()
        
        ' Set position and size
        toggleButton.Location = New System.Drawing.Point(50, 50)
        toggleButton.Size = New System.Drawing.Size(100, 40)
        toggleButton.Name = "toggleButton1"
        
        ' Configure Active state
        toggleButton.ActiveState.Text = "ON"
        toggleButton.ActiveState.BackColor = Color.FromArgb(1, 115, 199)
        toggleButton.ActiveState.BorderColor = Color.FromArgb(1, 115, 199)
        toggleButton.ActiveState.ForeColor = Color.White
        
        ' Configure Inactive state
        toggleButton.InactiveState.Text = "OFF"
        toggleButton.InactiveState.BackColor = Color.White
        toggleButton.InactiveState.BorderColor = Color.FromArgb(150, 150, 150)
        toggleButton.InactiveState.ForeColor = Color.FromArgb(80, 80, 80)
        
        ' Set initial state
        toggleButton.ToggleState = ToggleButtonState.Inactive
        
        ' Add to form controls
        Me.Controls.Add(toggleButton)
    End Sub
End Class
```

## Minimal Working Example

Here's a complete Form1.cs file with a basic Toggle Button:

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace ToggleButtonDemo
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
            SetupToggleButton();
        }
        
        private void SetupToggleButton()
        {
            ToggleButton toggleButton = new ToggleButton();
            toggleButton.Location = new Point(100, 100);
            toggleButton.Size = new Size(150, 50);
            toggleButton.Name = "mainToggle";
            
            // Active state
            toggleButton.ActiveState.Text = "Enabled";
            toggleButton.ActiveState.BackColor = Color.Green;
            toggleButton.ActiveState.ForeColor = Color.White;
            
            // Inactive state
            toggleButton.InactiveState.Text = "Disabled";
            toggleButton.InactiveState.BackColor = Color.LightGray;
            toggleButton.InactiveState.ForeColor = Color.Black;
            
            // Initial state
            toggleButton.ToggleState = ToggleButtonState.Inactive;
            
            this.Controls.Add(toggleButton);
        }
    }
}
```

## Key Takeaways

- **Designer Method**: Fastest for UI design; automatically adds references
- **Code Method**: Better for programmatic control and dynamic creation
- **Namespaces**: Always include `Syncfusion.Windows.Forms.Tools`
- **Assemblies**: Ensure all dependencies are properly referenced
- **Configuration**: Set both ActiveState and InactiveState for proper appearance
- **Initial State**: Use `ToggleState` property to set the starting state

## Troubleshooting

**Issue**: "ToggleButton not found in toolbox"
- **Solution**: Ensure Syncfusion NuGet package is installed and Visual Studio is restarted

**Issue**: "Type or namespace name 'Syncfusion' cannot be found"
- **Solution**: Add the required using statement and verify assembly references in the project

**Issue**: "Object reference not set to an instance of an object"
- **Solution**: Ensure Toggle Button is created before accessing its properties
