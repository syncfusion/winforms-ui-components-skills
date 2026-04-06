# Getting Started with Office2010Form

Learn how to install, configure, and create your first Office2010Form in Windows Forms applications. This guide covers assembly deployment, namespace imports, and the inheritance pattern required to transform standard forms into Office 2010-styled forms.

## Installation and Assembly Deployment

### Required Assembly

Office2010Form requires the following Syncfusion assembly:

- **Syncfusion.Shared.Base.dll**

This assembly contains the Office2010Form class and related Office 2010 styling components.

### NuGet Package Installation

Install the Syncfusion Windows Forms package using NuGet:

**Package Manager Console:**
```powershell
Install-Package Syncfusion.Windows.Forms
```

**NuGet Package Manager:**
1. Right-click on your project in Solution Explorer
2. Select "Manage NuGet Packages"
3. Search for "Syncfusion.Windows.Forms"
4. Click "Install"

**dotnet CLI:**
```bash
dotnet add package Syncfusion.Windows.Forms
```

### Manual Assembly Reference

If not using NuGet, add assembly references manually:

1. Right-click on "References" in Solution Explorer
2. Select "Add Reference"
3. Browse to Syncfusion installation folder
4. Add `Syncfusion.Shared.Base.dll`

**Default Installation Path:**
```
C:\Program Files (x86)\Syncfusion\Essential Studio\<version>\precompiledassemblies\<.NET version>\
```

## Namespace Import

Import the required namespace at the top of your form file:

**C# Namespace:**
```csharp
using Syncfusion.Windows.Forms;
```

**VB.NET Namespace:**
```vb
Imports Syncfusion.Windows.Forms
```

## Creating Your First Office2010Form

### Step 1: Create Windows Forms Project

Create a new Windows Forms application in Visual Studio:

1. Open Visual Studio
2. Create New Project → Windows Forms App (.NET Framework) or Windows Forms App (.NET)
3. Name your project (e.g., "Office2010FormDemo")
4. Click "Create"

### Step 2: Install Required Assembly

Add the Syncfusion.Shared.Base assembly reference as described above.

### Step 3: Modify Form Inheritance

Change your form to inherit from `Office2010Form` instead of the standard `Form` class.

**C# Implementation:**

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;

namespace Office2010FormDemo
{
    // Change from: public partial class Form1 : Form
    // To: public partial class Form1 : Office2010Form
    public partial class Form1 : Office2010Form
    {
        public Form1()
        {
            InitializeComponent();
            
            // Set form title
            this.Text = "Office2010Form Demo";
            
            // Set form size
            this.Size = new Size(800, 600);
        }
    }
}
```

**VB.NET Implementation:**

```vb
Imports System.Drawing
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms

' Change from: Public Class Form1
' To: Public Class Form1 Inherits Office2010Form
Partial Public Class Form1
    Inherits Office2010Form
    
    Public Sub New()
        InitializeComponent()
        
        ' Set form title
        Me.Text = "Office2010Form Demo"
        
        ' Set form size
        Me.Size = New Size(800, 600)
    End Sub
End Class
```

### Step 4: Run the Application

Build and run your application. The form now displays with Office 2010 styling automatically applied.

**Default Appearance:**
- Office 2010-styled title bar
- Default Blue color scheme
- Professional Office 2010 look and feel
- All standard Form functionality preserved

## Minimal Working Example

Here's a complete, minimal working example:

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;

namespace MinimalOffice2010
{
    public partial class MainForm : Office2010Form
    {
        public MainForm()
        {
            InitializeComponent();
            
            // Basic form configuration
            this.Text = "Office2010 Form";
            this.Size = new Size(800, 600);
            this.StartPosition = FormStartPosition.CenterScreen;
            
            // Add a simple button to verify functionality
            Button button = new Button
            {
                Text = "Click Me",
                Location = new Point(350, 250),
                Size = new Size(100, 40)
            };
            button.Click += (s, e) => MessageBox.Show("Office2010Form works!");
            
            this.Controls.Add(button);
        }
    }
}
```

## Basic Form Configuration

### Setting Form Title

```csharp
this.Text = "My Application";
```

### Applying Default Color Scheme

Office2010Form uses Blue color scheme by default. To explicitly set it:

```csharp
this.ColorScheme = Office2010Theme.Blue;
```

### Synchronizing Background Color

Make the form background match the color scheme:

```csharp
this.UseOffice2010SchemeBackColor = true;
```

### Complete Basic Setup

```csharp
public partial class BasicForm : Office2010Form
{
    public BasicForm()
    {
        InitializeComponent();
        
        // Form properties
        this.Text = "Office2010 Application";
        this.Size = new Size(1024, 768);
        this.StartPosition = FormStartPosition.CenterScreen;
        
        // Apply Office 2010 theme
        this.ColorScheme = Office2010Theme.Blue;
        this.UseOffice2010SchemeBackColor = true;
        
        // Optional: Set min/max sizes
        this.MinimumSize = new Size(800, 600);
        this.MaximumSize = new Size(1920, 1080);
    }
}
```

## Inheritance Pattern Explained

### Standard Form vs Office2010Form

**Standard Windows Form:**
```csharp
public partial class MyForm : Form
{
    // Standard Form - basic Windows appearance
}
```

**Office2010Form:**
```csharp
public partial class MyForm : Office2010Form
{
    // Office2010Form - Office 2010 styled appearance
}
```

### Why Inheritance?

Office2010Form inherits from the standard `Form` class, meaning:

- ✅ **All standard Form functionality is preserved** (events, properties, methods)
- ✅ **Drop-in replacement** for existing forms (minimal code changes)
- ✅ **Enhanced styling** automatically applied without custom drawing code
- ✅ **Compatible with existing controls** and components
- ✅ **Design-time support** in Visual Studio designer

### What Changes Automatically

When you inherit from Office2010Form:

1. **Title bar styling** changes to Office 2010 appearance
2. **Color scheme** defaults to Blue (Office 2010 theme)
3. **Border rendering** uses Office 2010 style
4. **Caption buttons** (minimize, maximize, close) styled accordingly
5. **Form controls** remain unchanged (standard Windows Forms controls)

## Common Setup Patterns

### Pattern 1: Application Base Form

Create a base form for your entire application:

```csharp
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;

namespace MyApplication
{
    // Base form for all application forms
    public class ApplicationBaseForm : Office2010Form
    {
        public ApplicationBaseForm()
        {
            // Shared configuration for all forms
            this.ColorScheme = Office2010Theme.Blue;
            this.UseOffice2010SchemeBackColor = true;
            this.StartPosition = FormStartPosition.CenterScreen;
            this.Font = new Font("Segoe UI", 9F);
        }
    }
    
    // Specific forms inherit from base
    public partial class MainForm : ApplicationBaseForm
    {
        public MainForm()
        {
            InitializeComponent();
            this.Text = "Main Window";
            this.Size = new Size(1024, 768);
        }
    }
    
    public partial class SettingsForm : ApplicationBaseForm
    {
        public SettingsForm()
        {
            InitializeComponent();
            this.Text = "Settings";
            this.Size = new Size(600, 400);
        }
    }
}
```

### Pattern 2: Converting Existing Forms

Convert an existing standard form to Office2010Form:

**Before:**
```csharp
public partial class ExistingForm : Form
{
    public ExistingForm()
    {
        InitializeComponent();
        // Existing code...
    }
}
```

**After:**
```csharp
using Syncfusion.Windows.Forms;  // Add namespace

public partial class ExistingForm : Office2010Form  // Change base class
{
    public ExistingForm()
    {
        InitializeComponent();
        // Existing code remains unchanged
        
        // Optional: Configure Office2010 theme
        this.ColorScheme = Office2010Theme.Blue;
    }
}
```

## Troubleshooting

### Issue: Assembly Not Found

**Error:** "The type or namespace name 'Office2010Form' could not be found"

**Solution:**
1. Verify Syncfusion.Shared.Base.dll is referenced
2. Check `using Syncfusion.Windows.Forms;` is added
3. Verify correct NuGet package is installed
4. Rebuild the solution

### Issue: Form Appears as Standard Form

**Problem:** Form doesn't show Office 2010 styling

**Solution:**
1. Verify inheritance: `public partial class Form1 : Office2010Form`
2. Check that assembly is correctly referenced
3. Ensure InitializeComponent() is called
4. Verify no DisableOffice2010Style = true setting

### Issue: Designer Error

**Error:** "The designer could not be shown for this file"

**Solution:**
1. Ensure Syncfusion assembly is available at design time
2. Close and reopen the designer
3. Rebuild the project
4. Check Visual Studio version compatibility

### Issue: Color Scheme Not Applied

**Problem:** Office2010Form created but color scheme doesn't apply

**Solution:**
1. Check `ApplyAeroTheme` is set to `false` (Aero prevents color schemes)
2. Explicitly set ColorScheme property
3. Set `UseOffice2010SchemeBackColor = true` for background

## Next Steps

- **Apply Color Schemes**: See [color-schemes.md](color-schemes.md) for Blue, Silver, Black, and Managed themes
- **Customize Caption**: See [caption-customization.md](caption-customization.md) for caption bar customization
- **Advanced Features**: See [advanced-features.md](advanced-features.md) for RTL, rounded corners, and more
