# Getting Started with TextBoxExt

This guide covers installation, basic setup, and initial configuration of the TextBoxExt control.

## Assembly Deployment

To use TextBoxExt in your Windows Forms application, you need to reference the required assembly.

### Required Assembly

**Assembly:** `Syncfusion.Shared.Base.dll`

This assembly contains the TextBoxExt control and other shared components.

### Assembly Location

After installing Syncfusion Essential Studio, the assembly can be found at:

```
C:\Program Files (x86)\Syncfusion\Essential Studio\<version>\precompiledassemblies\<.NET version>\
```

For example:
```
C:\Program Files (x86)\Syncfusion\Essential Studio\25.1.35\precompiledassemblies\4.8\Syncfusion.Shared.Base.dll
```

### NuGet Package Installation

The recommended approach is to install via NuGet Package Manager.

**Package Name:** `Syncfusion.Shared.WinForms`

**Install via Package Manager Console:**
```powershell
Install-Package Syncfusion.Shared.WinForms
```

**Install via .NET CLI:**
```bash
dotnet add package Syncfusion.Shared.WinForms
```

**NuGet Package Manager UI:**
1. Right-click project in Solution Explorer
2. Select "Manage NuGet Packages"
3. Search for "Syncfusion.Shared.WinForms"
4. Click "Install"

## Namespace Requirements

After adding the assembly reference, import the required namespace:

**C#:**
```csharp
using Syncfusion.Windows.Forms.Tools;
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms.Tools
```

## Adding TextBoxExt to Your Form

There are two ways to add TextBoxExt to your Windows Forms application:
1. Via Designer (drag-and-drop)
2. Via Code (programmatic)

### Method 1: Adding via Designer

The easiest way to add TextBoxExt is through the Visual Studio Designer.

**Steps:**

1. **Open your form in Designer view**
   - Double-click your form in Solution Explorer to open Designer

2. **Open the Toolbox**
   - View → Toolbox (or press Ctrl+Alt+X)

3. **Locate TextBoxExt**
   - Expand the "Syncfusion Controls" section
   - Find "TextBoxExt" in the list

4. **Drag and drop**
   - Drag TextBoxExt from Toolbox to your form
   - Position and resize as needed

5. **Configure properties**
   - Use the Properties window (F4) to configure the control

**Result:** The designer automatically adds the assembly reference and initializes the control in `InitializeComponent()`.

![Drag and drop TextBoxExt from toolbox](../../../../../docs/Getting-Started_images/windowsforms-textbox-drag-and-drop.png)

### Method 2: Adding via Code

For programmatic control creation, add TextBoxExt manually in code.

**C# Example:**
```csharp
using Syncfusion.Windows.Forms.Tools;
using System.Drawing;
using System.Windows.Forms;

public partial class Form1 : Form
{
    public Form1()
    {
        InitializeComponent();
        
        // Create TextBoxExt instance
        TextBoxExt textBoxExt1 = new TextBoxExt();
        
        // Set basic properties
        textBoxExt1.Location = new Point(50, 50);
        textBoxExt1.Size = new Size(200, 25);
        textBoxExt1.Text = "textboxext1";
        
        // Add to form's control collection
        this.Controls.Add(textBoxExt1);
    }
}
```

**VB.NET Example:**
```vb
Imports Syncfusion.Windows.Forms.Tools
Imports System.Drawing
Imports System.Windows.Forms

Public Partial Class Form1
    Inherits Form
    
    Public Sub New()
        InitializeComponent()
        
        ' Create TextBoxExt instance
        Dim textBoxExt1 As New TextBoxExt()
        
        ' Set basic properties
        textBoxExt1.Location = New Point(50, 50)
        textBoxExt1.Size = New Size(200, 25)
        textBoxExt1.Text = "textboxext1"
        
        ' Add to form's control collection
        Me.Controls.Add(textBoxExt1)
    End Sub
End Class
```

## Basic Configuration

### Setting Text Content

**C#:**
```csharp
// Set initial text
textBoxExt1.Text = "Enter your name";

// Get text value
string userInput = textBoxExt1.Text;

// Append text
textBoxExt1.AppendText(" - Additional text");
```

**VB.NET:**
```vb
' Set initial text
textBoxExt1.Text = "Enter your name"

' Get text value
Dim userInput As String = textBoxExt1.Text

' Append text
textBoxExt1.AppendText(" - Additional text")
```

### Configuring Size

Use `MaximumSize` and `MinimumSize` to constrain textbox dimensions.

**C#:**
```csharp
using System.Drawing;

// Set minimum size (200x25 pixels)
textBoxExt1.MinimumSize = new Size(200, 25);

// Set maximum size (400x25 pixels)
textBoxExt1.MaximumSize = new Size(400, 25);

// Or set both at once
textBoxExt1.MinimumSize = new Size(267, 104);
textBoxExt1.MaximumSize = new Size(267, 104);
```

**VB.NET:**
```vb
Imports System.Drawing

' Set minimum size (200x25 pixels)
textBoxExt1.MinimumSize = New Size(200, 25)

' Set maximum size (400x25 pixels)
textBoxExt1.MaximumSize = New Size(400, 25)

' Or set both at once
textBoxExt1.MinimumSize = New Size(267, 104)
textBoxExt1.MaximumSize = New Size(267, 104)
```

**Result:** The textbox will maintain the specified size constraints even when the form is resized.

![Windows Forms TextBoxExt showing size of the control](../../../../../docs/Creating-TextBoxExt_images/TextBoxExt_size.png)

## Multiline Text Configuration

Enable multiline text display with word wrapping and scrollbars.

### Basic Multiline Setup

**C#:**
```csharp
using System.Windows.Forms;

// Enable multiline mode
textBoxExt1.Multiline = true;

// Enable word wrap (text wraps to next line)
textBoxExt1.WordWrap = true;

// Add vertical scrollbar for long content
textBoxExt1.ScrollBars = ScrollBars.Vertical;
```

**VB.NET:**
```vb
Imports System.Windows.Forms

' Enable multiline mode
textBoxExt1.Multiline = True

' Enable word wrap (text wraps to next line)
textBoxExt1.WordWrap = True

' Add vertical scrollbar for long content
textBoxExt1.ScrollBars = ScrollBars.Vertical
```

**Result:** The textbox now supports multiple lines of text with automatic word wrapping.

![Windows Forms TextBoxExt shows multiline text](../../../../../docs/Creating-TextBoxExt_images/TextBoxExt_multiline.png)

### Scrollbar Options

The `ScrollBars` property accepts these values:

| Value | Description |
|-------|-------------|
| `ScrollBars.None` | No scrollbars (default) |
| `ScrollBars.Horizontal` | Horizontal scrollbar only |
| `ScrollBars.Vertical` | Vertical scrollbar only |
| `ScrollBars.Both` | Both horizontal and vertical scrollbars |

**Example with horizontal scrollbar:**
```csharp
textBoxExt1.Multiline = true;
textBoxExt1.WordWrap = false; // Disable word wrap for horizontal scroll
textBoxExt1.ScrollBars = ScrollBars.Horizontal;
```

**Example with both scrollbars:**
```csharp
textBoxExt1.Multiline = true;
textBoxExt1.WordWrap = false;
textBoxExt1.ScrollBars = ScrollBars.Both;
```

## Complete Getting Started Example

Here's a complete example creating a styled TextBoxExt with multiline support:

**C#:**
```csharp
using Syncfusion.Windows.Forms.Tools;
using System.Drawing;
using System.Windows.Forms;

public partial class Form1 : Form
{
    private TextBoxExt notesTextBox;
    
    public Form1()
    {
        InitializeComponent();
        CreateNotesTextBox();
    }
    
    private void CreateNotesTextBox()
    {
        // Create instance
        notesTextBox = new TextBoxExt();
        
        // Position and size
        notesTextBox.Location = new Point(20, 20);
        notesTextBox.Size = new Size(400, 200);
        
        // Enable multiline
        notesTextBox.Multiline = true;
        notesTextBox.WordWrap = true;
        notesTextBox.ScrollBars = ScrollBars.Vertical;
        
        // Set initial text
        notesTextBox.Text = "Enter your notes here...\n\n" +
                           "This is a multiline textbox with word wrapping " +
                           "and vertical scrollbar support.";
        
        // Apply simple styling
        notesTextBox.BorderStyle = BorderStyle.FixedSingle;
        notesTextBox.BorderColor = Color.SteelBlue;
        
        // Add to form
        this.Controls.Add(notesTextBox);
    }
}
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms.Tools
Imports System.Drawing
Imports System.Windows.Forms

Public Partial Class Form1
    Inherits Form
    
    Private notesTextBox As TextBoxExt
    
    Public Sub New()
        InitializeComponent()
        CreateNotesTextBox()
    End Sub
    
    Private Sub CreateNotesTextBox()
        ' Create instance
        notesTextBox = New TextBoxExt()
        
        ' Position and size
        notesTextBox.Location = New Point(20, 20)
        notesTextBox.Size = New Size(400, 200)
        
        ' Enable multiline
        notesTextBox.Multiline = True
        notesTextBox.WordWrap = True
        notesTextBox.ScrollBars = ScrollBars.Vertical
        
        ' Set initial text
        notesTextBox.Text = "Enter your notes here..." & vbCrLf & vbCrLf & _
                           "This is a multiline textbox with word wrapping " & _
                           "and vertical scrollbar support."
        
        ' Apply simple styling
        notesTextBox.BorderStyle = BorderStyle.FixedSingle
        notesTextBox.BorderColor = Color.SteelBlue
        
        ' Add to form
        Me.Controls.Add(notesTextBox)
    End Sub
End Class
```

## Next Steps

Now that you have TextBoxExt set up, explore these features:

- **Text Configuration** - Learn about character casing, alignment, overflow indicators
- **Border Settings** - Customize border colors and 3D styles
- **Appearance and Styling** - Apply Office themes and custom colors
- **Themes Configuration** - Load and apply theme assemblies
- **Behavior and Events** - Handle events and configure behavior properties

## Troubleshooting

### TextBoxExt not appearing in Toolbox

**Solution:**
1. Verify Syncfusion.Shared.Base.dll is referenced in your project
2. Clean and rebuild solution (Build → Clean Solution, then Build → Rebuild Solution)
3. Reset Toolbox: Right-click Toolbox → Reset Toolbox
4. Close and reopen Visual Studio

### Assembly reference errors

**Solution:**
- Ensure you're using compatible .NET Framework or .NET version
- Verify the assembly version matches your Syncfusion license
- Install via NuGet to handle dependencies automatically

### Control not displaying correctly

**Solution:**
- Call `InitializeComponent()` in form constructor before adding controls
- Ensure control is added to form's `Controls` collection
- Check that control's `Visible` property is `true`
- Verify control's location is within form's visible area
