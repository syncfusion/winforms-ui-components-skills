# Getting Started with Office2007Form

This guide covers the initial setup and basic implementation of the Syncfusion Office2007Form control in Windows Forms applications.

## Overview

The `Office2007Form` is an advanced standard Form that replaces the base `System.Windows.Forms.Form` class to provide Microsoft Office 2007-inspired UI and appearance. It allows developers to create visually appealing user interfaces with minimal code changes.

## Assembly Deployment

Before using Office2007Form, you need to reference the required assemblies in your project.

### Required Assembly

- **Syncfusion.Shared.Base.dll** - Contains the Office2007Form control and related components

### Adding Assembly Reference

**Via Project References:**
1. Right-click on **References** in Solution Explorer
2. Select **Add Reference**
3. Browse to Syncfusion installation folder
4. Select `Syncfusion.Shared.Base.dll`
5. Click **OK**

**Via NuGet Package Manager:**
```powershell
Install-Package Syncfusion.Windows.Forms
```

Or use Visual Studio's NuGet Package Manager UI:
1. Right-click on project → **Manage NuGet Packages**
2. Search for "Syncfusion.Windows.Forms"
3. Click **Install**

### Control Dependencies

Office2007Form is part of the Syncfusion.Shared.Base assembly. For detailed dependency information, refer to the [control dependencies documentation](https://help.syncfusion.com/windowsforms/control-dependencies#office2007form).

## NuGet Package Installation

For detailed instructions on installing NuGet packages in Windows Forms applications, see:
- [How to install nuget packages](https://help.syncfusion.com/windowsforms/installation/install-nuget-packages)

## Creating Your First Office2007Form

### Step 1: Create Windows Forms Project

Create a new Windows Forms project in Visual Studio:
1. File → New → Project
2. Select **Windows Forms App (.NET Framework)** or **Windows Forms App**
3. Name your project and click **Create**

### Step 2: Add Assembly Reference

Add the required assembly reference as described in the [Assembly Deployment](#assembly-deployment) section above.

### Step 3: Import Namespace

Add the Syncfusion namespace to your form's code file:

**C# Example:**
```csharp
using Syncfusion.Windows.Forms;
```

**VB.NET Example:**
```vb
Imports Syncfusion.Windows.Forms
```

### Step 4: Inherit from Office2007Form

Change your form class to inherit from `Office2007Form` instead of the standard `Form`:

**C# Example:**
```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;

namespace MyApplication
{
    public partial class Form1 : Office2007Form
    {
        public Form1()
        {
            InitializeComponent();
            
            // Set form title
            this.Text = "Office2007Form";
        }
    }
}
```

**VB.NET Example:**
```vb
Imports Syncfusion.Windows.Forms

Partial Public Class Form1
    Inherits Office2007Form
    
    Public Sub New()
        InitializeComponent()
        
        ' Set form title
        Me.Text = "Office2007Form"
    End Sub
End Class
```

### Step 5: Run the Application

Build and run your application. You should see your form with the Office 2007-style:

## Basic Configuration

### Setting the Form Title

```csharp
this.Text = "My Office 2007 Application";
```

### Setting Form Size

```csharp
this.Size = new System.Drawing.Size(800, 600);
```

### Complete Basic Example

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;

namespace MyApplication
{
    public partial class MainForm : Office2007Form
    {
        public MainForm()
        {
            InitializeComponent();
            
            // Basic configuration
            this.Text = "My Office 2007 Application";
            this.Size = new Size(800, 600);
            this.StartPosition = FormStartPosition.CenterScreen;
        }
    }
}
```

## What You Get Out of the Box

By inheriting from Office2007Form, you automatically get:

1. **Office 2007-style appearance** - Modern, professional look matching Microsoft Office
2. **Default color scheme** - Blue theme applied by default
3. **Enhanced caption bar** - Office-style title bar with rounded corners
4. **Aero support** - Compatible with Windows Vista/7 Aero effects
5. **Minimal code changes** - Just change the base class inheritance

## Next Steps

After basic setup, you can customize your Office2007Form:

- **Apply Color Schemes** - Choose from Blue, Silver, Black, or Managed themes → See [color-schemes.md](color-schemes.md)
- **Customize Caption Bar** - Adjust alignment, font, color, and height → See [caption-customization.md](caption-customization.md)
- **Advanced Features** - RTL support, rounded corners, style toggling → See [advanced-features.md](advanced-features.md)

## Common Setup Issues

### Assembly Reference Not Found

**Problem:** Compiler error about missing Syncfusion.Windows.Forms namespace

**Solution:** 
- Verify `Syncfusion.Shared.Base.dll` is referenced in your project
- Check that the NuGet package is properly installed
- Ensure the assembly version matches your Syncfusion license

### Designer Issues

**Problem:** Form doesn't display correctly in Visual Studio Designer

**Solution:**
- Build the project before opening the designer
- Close and reopen the form in designer
- Check that all required assemblies are in the output directory

### License Key Required

**Problem:** License dialog appears when running the application

**Solution:**
- Register your Syncfusion license key in your application
- For evaluation, use the trial license key provided by Syncfusion
- For commercial use, ensure proper license registration

## Migration from Standard Form

If you have an existing Windows Forms application using standard `Form`:

### Before:
```csharp
public partial class MyForm : Form
{
    // Your existing code
}
```

### After:
```csharp
public partial class MyForm : Office2007Form
{
    // Your existing code (unchanged)
}
```

**Additional Steps:**
1. Add `using Syncfusion.Windows.Forms;`
2. Reference `Syncfusion.Shared.Base.dll`
3. Rebuild the project

Your existing form code and controls will continue to work without modification. The Office2007Form enhances the visual appearance without breaking functionality.

## Summary

Office2007Form provides an easy way to modernize Windows Forms applications with minimal code changes. By simply changing the base class inheritance and adding the required assembly reference, you get a professional Office 2007-inspired appearance. From this foundation, you can further customize the appearance using color schemes, caption customization, and advanced features covered in the other reference documents.
