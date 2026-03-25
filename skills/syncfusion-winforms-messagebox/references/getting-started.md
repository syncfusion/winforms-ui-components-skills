# Getting Started with MessageBoxAdv

This guide covers the basics of setting up and using the Syncfusion MessageBoxAdv control in Windows Forms applications.

## Overview

MessageBoxAdv is an enhanced message box control that replaces the standard Windows Forms MessageBox with advanced styling, theming, and customization options. It uses a static `Show()` method pattern similar to the standard MessageBox, requiring no instantiation.

**Key differences from standard MessageBox:**
- Multiple visual themes (Office 2007/2010/2013/2016, Metro)
- Custom icon support with size specification
- Expandable details pane
- Complete localization support
- Runtime resizing capability
- Right-to-left layout support

---

## Assembly Deployment

### Required Assembly

MessageBoxAdv requires the following assembly:

**Assembly:** `Syncfusion.Shared.Base.dll`

This assembly contains the MessageBoxAdv class and related components.

### NuGet Package Installation

The easiest way to add MessageBoxAdv to your project is via NuGet:

**Package Name:** `Syncfusion.Shared.Base`

**Installation steps:**

1. Right-click your project in Solution Explorer
2. Select **Manage NuGet Packages**
3. Search for **Syncfusion.Shared.Base**
4. Click **Install**

**Package Manager Console:**
```powershell
Install-Package Syncfusion.Shared.Base
```

**.NET CLI:**
```bash
dotnet add package Syncfusion.Shared.Base
```

### Manual Assembly Reference

If not using NuGet, manually add the assembly reference:

1. Right-click **References** in Solution Explorer
2. Select **Add Reference**
3. Browse to Syncfusion installation folder (typically `C:\Program Files (x86)\Syncfusion\Essential Studio\<version>\precompiledassemblies\<framework>\`)
4. Navigate to the appropriate .NET Framework folder
5. Select `Syncfusion.Shared.Base.dll`
6. Click **OK**

---

## Namespace Requirements

Add the following namespace at the top of your code file:

**C#:**
```csharp
using Syncfusion.Windows.Forms;
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms
```

This namespace contains:
- `MessageBoxAdv` class (main static class)
- `MessageBoxAdv.Style` enum (theme selection)
- `MessageBoxAdvMetroColorTable` class (Metro customization)
- Theme-specific enums (`Office2007Theme`, `Office2010Theme`, etc.)

---

## Basic Show() Method Usage

MessageBoxAdv uses a static `Show()` method pattern. No instantiation is needed.

### Simplest Form

Display a message with default OK button:

**C#:**
```csharp
MessageBoxAdv.Show("Operation completed successfully!");
```

**VB.NET:**
```vb
MessageBoxAdv.Show("Operation completed successfully!")
```

### With Owner Window

Specify parent window for modal behavior:

**C#:**
```csharp
MessageBoxAdv.Show(this, "File saved successfully!");
```

**VB.NET:**
```vb
MessageBoxAdv.Show(Me, "File saved successfully!")
```

**Why specify owner:**
- Ensures message box is modal to parent form
- Centers message box on parent window
- Blocks parent form interaction until closed

### With Caption

Add a title bar caption:

**C#:**
```csharp
MessageBoxAdv.Show(this, "File saved successfully!", "Success");
```

**VB.NET:**
```vb
MessageBoxAdv.Show(Me, "File saved successfully!", "Success")
```

### With Buttons

Specify button combination:

**C#:**
```csharp
MessageBoxAdv.Show(this, "Save changes?", "Confirm", MessageBoxButtons.YesNo);
```

**VB.NET:**
```vb
MessageBoxAdv.Show(Me, "Save changes?", "Confirm", MessageBoxButtons.YesNo)
```

### With Icon

Add an icon for visual context:

**C#:**
```csharp
MessageBoxAdv.Show(this, "Save changes?", "Confirm", 
    MessageBoxButtons.YesNo, MessageBoxIcon.Question);
```

**VB.NET:**
```vb
MessageBoxAdv.Show(Me, "Save changes?", "Confirm", _
    MessageBoxButtons.YesNo, MessageBoxIcon.Question)
```

---

## Show() Method Overloads

MessageBoxAdv provides multiple overloads for flexibility:

### Common Overloads

```csharp
// Simple message
DialogResult Show(string text)

// With owner
DialogResult Show(IWin32Window owner, string text)

// With owner and caption
DialogResult Show(IWin32Window owner, string text, string caption)

// With owner, caption, and buttons
DialogResult Show(IWin32Window owner, string text, string caption, 
    MessageBoxButtons buttons)

// With owner, caption, buttons, and icon
DialogResult Show(IWin32Window owner, string text, string caption, 
    MessageBoxButtons buttons, MessageBoxIcon icon)

// With custom icon image
DialogResult Show(IWin32Window owner, string text, string caption, 
    MessageBoxButtons buttons, Image icon, Size iconSize)

// With details view
DialogResult Show(IWin32Window owner, string text, string caption, 
    MessageBoxButtons buttons, MessageBoxIcon icon, string details)

// With custom icon and details
DialogResult Show(IWin32Window owner, string text, string caption, 
    MessageBoxButtons buttons, Image icon, Size iconSize, string details)
```

---

## DialogResult Return Value

MessageBoxAdv returns a `DialogResult` enum indicating which button was clicked.

### DialogResult Values

| Value | Description |
|-------|-------------|
| `OK` | User clicked OK button |
| `Cancel` | User clicked Cancel button or closed dialog |
| `Yes` | User clicked Yes button |
| `No` | User clicked No button |
| `Retry` | User clicked Retry button |
| `Abort` | User clicked Abort button |
| `Ignore` | User clicked Ignore button |
| `None` | Dialog was closed without button click (rare) |

### Capturing Return Value

**C#:**
```csharp
DialogResult result = MessageBoxAdv.Show(this, 
    "Save changes before closing?", 
    "Unsaved Changes", 
    MessageBoxButtons.YesNoCancel, 
    MessageBoxIcon.Question);

if (result == DialogResult.Yes)
{
    SaveDocument();
    CloseDocument();
}
else if (result == DialogResult.No)
{
    CloseDocument();
}
else if (result == DialogResult.Cancel)
{
    // Stay in editor
}
```

**VB.NET:**
```vb
Dim result As DialogResult = MessageBoxAdv.Show(Me, _
    "Save changes before closing?", _
    "Unsaved Changes", _
    MessageBoxButtons.YesNoCancel, _
    MessageBoxIcon.Question)

If result = DialogResult.Yes Then
    SaveDocument()
    CloseDocument()
ElseIf result = DialogResult.No Then
    CloseDocument()
ElseIf result = DialogResult.Cancel Then
    ' Stay in editor
End If
```

### Switch Statement Pattern

**C#:**
```csharp
switch (result)
{
    case DialogResult.OK:
        ProcessAction();
        break;
    case DialogResult.Cancel:
        CancelAction();
        break;
    case DialogResult.Retry:
        RetryAction();
        break;
}
```

---

## Complete Examples

### Example 1: Simple Information Message

**C#:**
```csharp
using Syncfusion.Windows.Forms;

public partial class Form1 : Form
{
    public Form1()
    {
        InitializeComponent();
    }

    private void btnShowMessage_Click(object sender, EventArgs e)
    {
        MessageBoxAdv.Show(this, 
            "Backup completed successfully!", 
            "Backup Complete", 
            MessageBoxButtons.OK, 
            MessageBoxIcon.Information);
    }
}
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms

Public Class Form1
    Private Sub btnShowMessage_Click(sender As Object, e As EventArgs) Handles btnShowMessage.Click
        MessageBoxAdv.Show(Me, _
            "Backup completed successfully!", _
            "Backup Complete", _
            MessageBoxButtons.OK, _
            MessageBoxIcon.Information)
    End Sub
End Class
```

---

### Example 2: Yes/No Confirmation

**C#:**
```csharp
using Syncfusion.Windows.Forms;

private void DeleteRecord()
{
    DialogResult result = MessageBoxAdv.Show(this,
        "Are you sure you want to delete this record?",
        "Confirm Delete",
        MessageBoxButtons.YesNo,
        MessageBoxIcon.Warning);

    if (result == DialogResult.Yes)
    {
        // Proceed with deletion
        Database.DeleteRecord(recordId);
        MessageBoxAdv.Show(this, "Record deleted.", "Success", 
            MessageBoxButtons.OK, MessageBoxIcon.Information);
    }
}
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms

Private Sub DeleteRecord()
    Dim result As DialogResult = MessageBoxAdv.Show(Me, _
        "Are you sure you want to delete this record?", _
        "Confirm Delete", _
        MessageBoxButtons.YesNo, _
        MessageBoxIcon.Warning)

    If result = DialogResult.Yes Then
        ' Proceed with deletion
        Database.DeleteRecord(recordId)
        MessageBoxAdv.Show(Me, "Record deleted.", "Success", _
            MessageBoxButtons.OK, MessageBoxIcon.Information)
    End If
End Sub
```

---

### Example 3: Error with Retry Option

**C#:**
```csharp
using Syncfusion.Windows.Forms;

private void ConnectToServer()
{
    bool connected = false;
    
    while (!connected)
    {
        try
        {
            server.Connect();
            connected = true;
        }
        catch (Exception ex)
        {
            DialogResult result = MessageBoxAdv.Show(this,
                $"Failed to connect to server:\n{ex.Message}",
                "Connection Error",
                MessageBoxButtons.RetryCancel,
                MessageBoxIcon.Error);

            if (result == DialogResult.Cancel)
            {
                break;
            }
        }
    }
}
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms

Private Sub ConnectToServer()
    Dim connected As Boolean = False
    
    While Not connected
        Try
            server.Connect()
            connected = True
        Catch ex As Exception
            Dim result As DialogResult = MessageBoxAdv.Show(Me, _
                $"Failed to connect to server:{vbCrLf}{ex.Message}", _
                "Connection Error", _
                MessageBoxButtons.RetryCancel, _
                MessageBoxIcon.Error)

            If result = DialogResult.Cancel Then
                Exit While
            End If
        End Try
    End While
End Sub
```

---

### Example 4: Application with Metro Theme

**C#:**
```csharp
using Syncfusion.Windows.Forms;

public partial class MainForm : Form
{
    public MainForm()
    {
        InitializeComponent();
        
        // Set Metro theme globally for all message boxes
        MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro;
    }

    private void btnSave_Click(object sender, EventArgs e)
    {
        if (SaveData())
        {
            MessageBoxAdv.Show(this, 
                "Data saved successfully!", 
                "Success", 
                MessageBoxButtons.OK, 
                MessageBoxIcon.Information);
        }
        else
        {
            MessageBoxAdv.Show(this, 
                "Failed to save data. Please try again.", 
                "Error", 
                MessageBoxButtons.OK, 
                MessageBoxIcon.Error);
        }
    }
}
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms

Public Class MainForm
    Public Sub New()
        InitializeComponent()
        
        ' Set Metro theme globally for all message boxes
        MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro
    End Sub

    Private Sub btnSave_Click(sender As Object, e As EventArgs) Handles btnSave.Click
        If SaveData() Then
            MessageBoxAdv.Show(Me, _
                "Data saved successfully!", _
                "Success", _
                MessageBoxButtons.OK, _
                MessageBoxIcon.Information)
        Else
            MessageBoxAdv.Show(Me, _
                "Failed to save data. Please try again.", _
                "Error", _
                MessageBoxButtons.OK, _
                MessageBoxIcon.Error)
        End If
    End Sub
End Class
```

---

## Best Practices

### Always Specify Owner Window

**Do:**
```csharp
MessageBoxAdv.Show(this, "Message", "Title");
```

**Don't:**
```csharp
MessageBoxAdv.Show("Message"); // May appear behind other windows
```

### Choose Appropriate Icons

Match icon type to message severity:
- **Information** (ℹ️): Successful operations, notifications
- **Question** (❓): Confirmations, user decisions
- **Warning** (⚠️): Potential issues, destructive actions
- **Error** (❌): Failures, exceptions, critical problems

### Set Theme Once in Application Startup

Configure theme globally in Form constructor or Program.cs:

```csharp
public MainForm()
{
    InitializeComponent();
    
    // Set theme once for entire application
    MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2016;
    MessageBoxAdv.Office2016Theme = Office2016Theme.Colorful;
}
```

### Handle All DialogResult Cases

When using multiple buttons, handle all possible results:

```csharp
switch (result)
{
    case DialogResult.Yes:
        // Handle Yes
        break;
    case DialogResult.No:
        // Handle No
        break;
    case DialogResult.Cancel:
        // Handle Cancel
        break;
}
```

---

## Troubleshooting

### Message Box Not Showing

**Issue:** `MessageBoxAdv.Show()` doesn't display anything.

**Solution:** Ensure `Syncfusion.Shared.Base.dll` is referenced and namespace is imported.

### Assembly Not Found Error

**Issue:** "Could not load file or assembly 'Syncfusion.Shared.Base'" error.

**Solution:** 
1. Verify NuGet package is installed
2. Check assembly is in project references
3. Ensure correct version for target .NET Framework

### Theme Not Applying

**Issue:** Theme style doesn't change appearance.

**Solution:** Set `MessageBoxStyle` property before calling `Show()`:

```csharp
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro;
MessageBoxAdv.Show(this, "Message");
```

### Message Box Behind Other Windows

**Issue:** Message box appears behind parent form or other windows.

**Solution:** Always pass owner window (`this` or `Me`) as first parameter:

```csharp
MessageBoxAdv.Show(this, "Message", "Title");
```

---

## Next Steps

- **Button Parameters:** Learn about button combinations, icons, RTL, details view, and resizing → [button-parameters.md](button-parameters.md)
- **Visual Styles:** Explore Office and Metro themes → [visual-styles.md](visual-styles.md)
- **Metro Customization:** Customize Metro theme colors → [metro-customization.md](metro-customization.md)
- **Localization:** Implement multilanguage support → [localization.md](localization.md)
