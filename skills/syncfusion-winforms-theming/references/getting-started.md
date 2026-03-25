# Getting Started with Windows Forms Theming

This guide covers the fundamental setup and basic usage of the Syncfusion Windows Forms SkinManager component for applying themes to your application.

## Assembly Deployment

### Required Assembly

The SkinManager component is included in the **`Syncfusion.Shared.Base`** assembly. This assembly must be referenced in your Windows Forms project.

### Theme Assembly Requirements

Different themes have different assembly requirements:

| Theme Name | Assembly Required |
|------------|-------------------|
| **Office2016Theme** | `Syncfusion.Office2016Theme.WinForms.dll` (only for SfDataGrid, SfButton, SfDateTimeEdit, SfNumericTextBox, SfToolTip, SfSmithChart) |
| **Office2019Theme** | `Syncfusion.Office2019Theme.WinForms.dll` |
| **HighContrastTheme** | `Syncfusion.HighContrastTheme.WinForms.dll` |
| **Other Themes** | Included in control assembly (no separate assembly needed) |

**Built-in themes** (no separate assembly required):
- Office2007 (Blue, Black, Silver, Managed)
- Office2010 (Blue, Black, Silver, Managed)
- Office2013 (White, Dark Gray, Black, Colorful)
- Metro

## Loading Theme Assemblies

Before applying themes that require separate assemblies, you must load them using the `LoadAssembly` method. This is typically done in `Program.cs` before the application starts.

### Loading Office2019 Theme Assembly

```csharp
using Syncfusion.WinForms.Themes;

// Loading Office2019Theme assembly
SkinManager.LoadAssembly(typeof(Office2019Theme).Assembly);
```

```vb
' VB.NET
Imports Syncfusion.WinForms.Themes

' Loading Office2019Theme assembly
SkinManager.LoadAssembly(GetType(Office2019Theme).Assembly)
```

### Loading HighContrast Theme Assembly

```csharp
using Syncfusion.WinForms.Themes;

// Loading HighContrastTheme assembly
SkinManager.LoadAssembly(typeof(HighContrastTheme).Assembly);
```

```vb
' VB.NET
Imports Syncfusion.WinForms.Themes

' Loading HighContrastTheme assembly
SkinManager.LoadAssembly(GetType(HighContrastTheme).Assembly)
```

### Loading Office2016 Theme Assembly

```csharp
using Syncfusion.WinForms.Themes;

// Loading Office2016Theme assembly
SkinManager.LoadAssembly(typeof(Syncfusion.WinForms.Themes.Office2016Theme).Assembly);
```

```vb
' VB.NET
Imports Syncfusion.WinForms.Themes

' Loading Office2016Theme assembly
SkinManager.LoadAssembly(GetType(Syncfusion.WinForms.Themes.Office2016Theme).Assembly)
```

## Adding SkinManager Component

There are two ways to add the SkinManager component to your Windows Forms application: through the designer or through code.

## Through Designer

### Step 1: Create Windows Forms Application

Create a new Windows Forms application in Visual Studio.

### Step 2: Add SkinManager from Toolbox

The SkinManager component can be added to the designer by dragging it from the toolbox to the design view.

**Location in Toolbox:** Find "SkinManager" in the Syncfusion controls section.

**Automatic Assembly Reference:** The following dependent assembly will be added automatically:
- `Syncfusion.Shared.Base`

### Step 3: Configure Through Properties Window

After adding the SkinManager component to the designer:

1. Select the SkinManager component in the component tray
2. Open the Properties window
3. Use the `Controls` property to specify which control or form to theme
4. Set the `VisualTheme` property to choose the desired theme

### Applying Theme to the Form (Designer)

To apply a theme to the entire form (and all its Syncfusion controls):

1. Set `Controls` property to the form (select the form from the dropdown)
2. Set `VisualTheme` property to your desired theme (e.g., `Office2016Black`)

This approach applies the theme to all Syncfusion controls within the form automatically.

## Through Code

You can add and configure the SkinManager component programmatically in your form's code.

### Basic Setup

```csharp
using Syncfusion.Windows.Forms;

public partial class Form1 : Form
{
    public Form1()
    {
        InitializeComponent();
        
        // Create component container if not exists
        if (this.components == null)
            this.components = new System.ComponentModel.Container();
        
        // Create SkinManager instance
        SkinManager skinManager1 = new SkinManager(this.components);
        
        // Apply theme to a specific control
        skinManager1.Controls = treeViewAdv1;
        skinManager1.VisualTheme = VisualTheme.Office2016Black;
    }
}
```

```vb
' VB.NET
Imports Syncfusion.Windows.Forms

Public Partial Class Form1
    Inherits Form
    
    Public Sub New()
        InitializeComponent()
        
        ' Create component container if not exists
        If Me.components Is Nothing Then
            Me.components = New System.ComponentModel.Container()
        End If
        
        ' Create SkinManager instance
        Dim skinManager1 As New SkinManager(Me.components)
        
        ' Apply theme to a specific control
        skinManager1.Controls = treeViewAdv1
        skinManager1.VisualTheme = VisualTheme.Office2016Black
    End Sub
End Class
```

### Applying Theme to the Entire Form

The most efficient approach is to apply the theme to the form itself, which automatically themes all Syncfusion controls within it.

```csharp
using Syncfusion.Windows.Forms;

public partial class Form1 : Form
{
    public Form1()
    {
        InitializeComponent();
        
        // Create SkinManager
        SkinManager skinManager1 = new SkinManager(this.components);
        
        // Apply theme to entire form
        skinManager1.Controls = this; // 'this' refers to the form
        skinManager1.VisualTheme = VisualTheme.Office2016Black;
    }
}
```

```vb
' VB.NET
Imports Syncfusion.Windows.Forms

Public Partial Class Form1
    Inherits Form
    
    Public Sub New()
        InitializeComponent()
        
        ' Create SkinManager
        Dim skinManager1 As New SkinManager(Me.components)
        
        ' Apply theme to entire form
        skinManager1.Controls = Me ' 'Me' refers to the form
        skinManager1.VisualTheme = VisualTheme.Office2016Black
    End Sub
End Class
```

**Benefits of Form-Level Theming:**
- No need to apply theme to each control individually
- Automatically themes all child Syncfusion controls
- Easier to maintain and update
- Consistent appearance across the entire form

## Complete Example: Application Setup

Here's a complete example showing proper setup in `Program.cs` and form initialization:

### Program.cs

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;
using Syncfusion.WinForms.Themes;

namespace MyWinFormsApp
{
    static class Program
    {
        [STAThread]
        static void Main()
        {
            // Load required theme assembly
            SkinManager.LoadAssembly(typeof(Office2019Theme).Assembly);
            
            Application.EnableVisualStyles();
            Application.SetCompatibleTextRenderingDefault(false);
            Application.Run(new Form1());
        }
    }
}
```

### Form1.cs

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace MyWinFormsApp
{
    public partial class Form1 : Form
    {
        private SkinManager skinManager1;
        
        public Form1()
        {
            InitializeComponent();
            
            // Initialize SkinManager
            this.components = new System.ComponentModel.Container();
            skinManager1 = new SkinManager(this.components);
            
            // Apply Office2019 theme to entire form
            skinManager1.Controls = this;
            skinManager1.VisualTheme = VisualTheme.Office2019Colorful;
        }
    }
}
```

## Common Setup Scenarios

### Scenario 1: Single Control Theme

Apply theme to one specific control:

```csharp
skinManager1.Controls = dataGridView1;
skinManager1.VisualTheme = VisualTheme.Office2016Colorful;
```

### Scenario 2: Container Theme

Apply theme to a panel or groupbox (themes all controls inside):

```csharp
skinManager1.Controls = panel1; // Themes all controls in panel1
skinManager1.VisualTheme = VisualTheme.Office2016White;
```

### Scenario 3: Multiple Forms

Each form can have its own SkinManager instance:

```csharp
// Form1
skinManager1.Controls = this;
skinManager1.VisualTheme = VisualTheme.Office2016Black;

// Form2
skinManager2.Controls = this;
skinManager2.VisualTheme = VisualTheme.Office2016White;
```

## Important Notes

### Assembly Loading Order

Always load theme assemblies **before** applying themes:

```csharp
// CORRECT: Load first, then apply
SkinManager.LoadAssembly(typeof(Office2019Theme).Assembly);
skinManager1.VisualTheme = VisualTheme.Office2019Colorful;

// INCORRECT: Apply before loading (will fail)
skinManager1.VisualTheme = VisualTheme.Office2019Colorful;
SkinManager.LoadAssembly(typeof(Office2019Theme).Assembly);
```

### Theme Assembly Compatibility

- Built-in themes (Office2007, Office2010, Office2013, Metro) do not require `LoadAssembly()`
- Office2016, Office2019, and HighContrast themes require separate assemblies
- Custom Theme Studio themes require the exported custom assembly

### Component Requirement

SkinManager must be added to the form's component container:

```csharp
// Create component container if needed
if (this.components == null)
    this.components = new System.ComponentModel.Container();

SkinManager skinManager1 = new SkinManager(this.components);
```

## Troubleshooting

**Problem:** Theme not applying to controls  
**Solution:** Ensure the theme assembly is loaded before setting VisualTheme. Check that `Controls` property is set to the correct control or form.

**Problem:** "Theme not found" exception  
**Solution:** Verify the theme assembly is referenced in the project and `LoadAssembly()` is called in Program.cs.

**Problem:** Some controls not themed  
**Solution:** Apply theme to the parent form or container instead of individual controls to ensure all child controls are themed.

**Problem:** Theme assembly not found at runtime  
**Solution:** Ensure the theme assembly DLL is copied to the output directory. Check the assembly reference properties.
