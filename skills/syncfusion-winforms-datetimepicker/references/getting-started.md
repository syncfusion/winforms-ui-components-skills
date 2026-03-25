# Getting Started with SfDateTimeEdit

The **SfDateTimeEdit** is a Windows Forms control that allows you to edit DateTime values in text or mask format with support for minimum and maximum value validation, watermark, and globalization. It provides flexible options to display date-time according to the required format.

## Assembly Deployment

To use the SfDateTimeEdit control in your application, add the following assembly references:

* **Syncfusion.Core.WinForms** - Core functionality for WinForms controls
* **Syncfusion.SfInput.WinForms** - Input controls including SfDateTimeEdit
* **Syncfusion.Shared.Base** - Shared base assemblies

You can add these assemblies either by:
- Referencing them directly from the Syncfusion installation folder
- Installing the NuGet package: [Syncfusion.SfInput.WinForms](https://www.nuget.org/packages/Syncfusion.SfInput.WinForms/)

## Adding SfDateTimeEdit via Designer

Follow these steps to add SfDateTimeEdit through the designer:

1. Create a new Windows Forms application in Visual Studio
2. Open the Toolbox (View → Toolbox)
3. Locate **SfDateTimeEdit** in the Syncfusion WinForms toolbox section
4. Drag and drop the SfDateTimeEdit control onto your form

The required assemblies will be added automatically to your project references.

## Adding SfDateTimeEdit via Code

To add the control programmatically:

### Step 1: Add Assembly References

Add the following assemblies to your project:
- Syncfusion.Core.WinForms
- Syncfusion.SfInput.WinForms
- Syncfusion.Shared.Base

### Step 2: Import the Namespace

**C#:**
```csharp
using Syncfusion.WinForms.Input;
```

**VB.NET:**
```vb
Imports Syncfusion.WinForms.Input
```

### Step 3: Create and Add the Control

**C#:**
```csharp
// Create an instance of SfDateTimeEdit
SfDateTimeEdit dateTimeEdit = new SfDateTimeEdit();

// Set basic properties
dateTimeEdit.Location = new System.Drawing.Point(20, 20);
dateTimeEdit.Size = new System.Drawing.Size(200, 30);

// Add to form
this.Controls.Add(dateTimeEdit);
```

**VB.NET:**
```vb
' Create an instance of SfDateTimeEdit
Dim dateTimeEdit As New SfDateTimeEdit()

' Set basic properties
dateTimeEdit.Location = New System.Drawing.Point(20, 20)
dateTimeEdit.Size = New System.Drawing.Size(200, 30)

' Add to form
Me.Controls.Add(dateTimeEdit)
```

## Setting the Value

The Value property is used to set the current date and time:

**C#:**
```csharp
// Set a specific date and time
dateTimeEdit.Value = new DateTime(2018, 2, 16);

// Set to current date and time
dateTimeEdit.Value = DateTime.Now;
```

**VB.NET:**
```vb
' Set a specific date and time
dateTimeEdit.Value = New DateTime(2018, 2, 16)

' Set to current date and time
dateTimeEdit.Value = DateTime.Now
```

## Complete Example

Here's a complete example showing basic usage:

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.WinForms.Input;

namespace DateTimeEditDemo
{
    public partial class Form1 : Form
    {
        private SfDateTimeEdit dateTimeEdit;
        
        public Form1()
        {
            InitializeComponent();
            InitializeDateTimeEdit();
        }
        
        private void InitializeDateTimeEdit()
        {
            // Create the control
            dateTimeEdit = new SfDateTimeEdit();
            
            // Set position and size
            dateTimeEdit.Location = new Point(50, 50);
            dateTimeEdit.Size = new Size(250, 30);
            
            // Set the current date
            dateTimeEdit.Value = DateTime.Now;
            
            // Add to form
            this.Controls.Add(dateTimeEdit);
        }
    }
}
```

**VB.NET:**
```vb
Imports System
Imports System.Drawing
Imports System.Windows.Forms
Imports Syncfusion.WinForms.Input

Namespace DateTimeEditDemo
    Public Partial Class Form1
        Inherits Form
        
        Private dateTimeEdit As SfDateTimeEdit
        
        Public Sub New()
            InitializeComponent()
            InitializeDateTimeEdit()
        End Sub
        
        Private Sub InitializeDateTimeEdit()
            ' Create the control
            dateTimeEdit = New SfDateTimeEdit()
            
            ' Set position and size
            dateTimeEdit.Location = New Point(50, 50)
            dateTimeEdit.Size = New Size(250, 30)
            
            ' Set the current date
            dateTimeEdit.Value = DateTime.Now
            
            ' Add to form
            Me.Controls.Add(dateTimeEdit)
        End Sub
    End Class
End Namespace
```

## Key Features Overview

The SfDateTimeEdit control provides these essential features:

**Editing Modes:**
- Default text editing mode for free-form input
- Mask editing mode for field-by-field navigation

**Value Management:**
- Set minimum and maximum date ranges
- Support for null values with watermark text
- Value validation on focus loss

**Display Patterns:**
- Built-in patterns (LongDate, ShortDate, LongTime, etc.)
- Custom format patterns
- Culture-based formatting

**User Interface:**
- Up-down buttons for value increment/decrement
- Drop-down calendar for date selection
- Keyboard navigation support

## Next Steps

Explore these topics to learn more about SfDateTimeEdit:

- **[Date Range and Value Management](date-range-value.md)** - Learn about Value, MinDateTime, MaxDateTime, and AllowNull properties
- **[Display Patterns](display-patterns.md)** - Discover date-time display formats and custom patterns
- **[Editing Modes](editing-modes.md)** - Understand default and mask editing modes
- **[Appearance Styling](appearance-styling.md)** - Customize themes, colors, fonts, and borders
- **[Validation Features](validation-features.md)** - Implement validation, watermarks, and globalization
