# Getting Started with CheckBoxAdv

This guide covers the essential steps to add and configure the CheckBoxAdv control in Windows Forms applications.

## Assembly Deployment

The CheckBoxAdv control requires the following assemblies:

- **Syncfusion.Shared.Base.dll** - Contains the CheckBoxAdv control and related classes
- **Syncfusion.Tools.Windows.dll** - Required for Tools namespace support

### Using NuGet Package

Install the CheckBoxAdv control via NuGet Package Manager:

1. Open your Windows Forms project in Visual Studio
2. Go to **Tools > NuGet Package Manager > Manage NuGet Packages for Solution**
3. Search for `Syncfusion.Windows.Forms.Tools`
4. Install the package to your project

The NuGet package automatically adds the required assembly references.

## Adding CheckBoxAdv via Designer

The designer approach provides a visual way to add and configure the control:

### Step 1: Open Form Designer

1. Create a new Windows Forms project or open an existing form
2. Open the form in Design view

### Step 2: Add Control from Toolbox

1. Locate **CheckBoxAdv** in the Toolbox under the Syncfusion section
2. Drag and drop the control onto your form
3. The dependent assemblies are added automatically

### Step 3: Configure Properties

Use the Properties window to configure:
- **Text**: The label displayed next to the checkbox
- **Checked**: Set initial checked state (true/false)
- **Size**: Width and height of the control
- **Location**: Position on the form

```csharp
// Designer-generated code example
this.checkBoxAdv1 = new Syncfusion.Windows.Forms.Tools.CheckBoxAdv();
this.checkBoxAdv1.Text = "CheckBoxAdv";
this.checkBoxAdv1.Location = new System.Drawing.Point(20, 20);
this.checkBoxAdv1.Size = new System.Drawing.Size(150, 25);
```

## Adding CheckBoxAdv via Code

For programmatic control creation, follow these steps:

### Step 1: Add Assembly References

Manually add references to your project:

1. Right-click **References** in Solution Explorer
2. Select **Add Reference**
3. Browse to the Syncfusion installation folder
4. Add:
   - Syncfusion.Shared.Base.dll
   - Syncfusion.Tools.Windows.dll

### Step 2: Include Namespace

Add the required namespace at the top of your code file:

```csharp
using Syncfusion.Windows.Forms.Tools;
```

```vb
Imports Syncfusion.Windows.Forms.Tools
```

### Step 3: Create and Configure Control

Instantiate the CheckBoxAdv control and set its properties:

```csharp
// Create instance
CheckBoxAdv checkBoxAdv1 = new CheckBoxAdv();

// Set basic properties
checkBoxAdv1.Text = "CheckBoxAdv";
checkBoxAdv1.Height = 25;
checkBoxAdv1.Width = 200;
checkBoxAdv1.Location = new Point(20, 20);

// Add to form
this.Controls.Add(checkBoxAdv1);
```

```vb
' Create instance
Dim checkBoxAdv1 As CheckBoxAdv = New CheckBoxAdv()

' Set basic properties
checkBoxAdv1.Text = "CheckBoxAdv"
checkBoxAdv1.Height = 25
checkBoxAdv1.Width = 200
checkBoxAdv1.Location = New Point(20, 20)

' Add to form
Me.Controls.Add(checkBoxAdv1)
```

## Basic Checkbox State Configuration

After adding the control, configure its initial state:

### Setting Checked State

Use the `Checked` property for boolean state:

```csharp
// Set to checked
checkBoxAdv1.Checked = true;

// Set to unchecked
checkBoxAdv1.Checked = false;

// Get current state
bool isChecked = checkBoxAdv1.Checked;
```

```vb
' Set to checked
checkBoxAdv1.Checked = True

' Set to unchecked
checkBoxAdv1.Checked = False

' Get current state
Dim isChecked As Boolean = checkBoxAdv1.Checked
```

### Setting CheckState

Use the `CheckState` property for more control, including indeterminate state:

```csharp
// Set to checked
checkBoxAdv1.CheckState = CheckState.Checked;

// Set to unchecked
checkBoxAdv1.CheckState = CheckState.Unchecked;

// Set to indeterminate
checkBoxAdv1.CheckState = CheckState.Indeterminate;

// Get current state
CheckState currentState = checkBoxAdv1.CheckState;
```

```vb
' Set to checked
checkBoxAdv1.CheckState = CheckState.Checked

' Set to unchecked
checkBoxAdv1.CheckState = CheckState.Unchecked

' Set to indeterminate
checkBoxAdv1.CheckState = CheckState.Indeterminate

' Get current state
Dim currentState As CheckState = checkBoxAdv1.CheckState
```

## Complete Initialization Example

Here's a complete example showing initialization with common properties:

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class Form1 : Form
{
    private CheckBoxAdv checkBoxAdv1;
    
    public Form1()
    {
        InitializeComponent();
        InitializeCheckBox();
    }
    
    private void InitializeCheckBox()
    {
        // Create control
        checkBoxAdv1 = new CheckBoxAdv();
        
        // Basic properties
        checkBoxAdv1.Text = "Enable Feature";
        checkBoxAdv1.Location = new Point(20, 20);
        checkBoxAdv1.Size = new Size(150, 25);
        
        // State
        checkBoxAdv1.Checked = true;
        
        // Add to form
        this.Controls.Add(checkBoxAdv1);
    }
}
```

```vb
Imports System
Imports System.Drawing
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Public Partial Class Form1
    Inherits Form
    
    Private checkBoxAdv1 As CheckBoxAdv
    
    Public Sub New()
        InitializeComponent()
        InitializeCheckBox()
    End Sub
    
    Private Sub InitializeCheckBox()
        ' Create control
        checkBoxAdv1 = New CheckBoxAdv()
        
        ' Basic properties
        checkBoxAdv1.Text = "Enable Feature"
        checkBoxAdv1.Location = New Point(20, 20)
        checkBoxAdv1.Size = New Size(150, 25)
        
        ' State
        checkBoxAdv1.Checked = True
        
        ' Add to form
        Me.Controls.Add(checkBoxAdv1)
    End Sub
End Class
```

## Default Values

When you create a new CheckBoxAdv instance, these are the default values:

| Property | Default Value |
|----------|---------------|
| Checked | false |
| CheckState | CheckState.Unchecked |
| Text | "" (empty string) |
| Tristate | false |
| AutoHeight | false |
| ReadOnlyMode | false |
| DrawFocusRectangle | true |

## Next Steps

After successfully adding the CheckBoxAdv control:

- Configure states and values for data binding
- Customize text appearance and alignment
- Apply visual styling (backgrounds, borders, shadows)
- Set up event handlers for state changes
- Bind to data sources for database integration
