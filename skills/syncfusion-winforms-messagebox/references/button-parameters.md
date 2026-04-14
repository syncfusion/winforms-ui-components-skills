# Button Parameters and Features

This guide covers button combinations, icons, right-to-left support, details view, and resizing capabilities in MessageBoxAdv.

## Table of Contents
- [Caption Bar Text](#caption-bar-text)
- [Message Text](#message-text)
- [Button Combinations](#button-combinations)
- [Icon Support](#icon-support)
- [Right-to-Left Support](#right-to-left-support)
- [Details View](#details-view)
- [Resizing Support](#resizing-support)

---

## Caption Bar Text

The caption bar text appears in the title bar of the MessageBoxAdv window.

### Parameter

**Type:** `string`  
**Position:** 3rd parameter in `Show()` method

### Usage

**C#:**
```csharp
MessageBoxAdv.Show(this, "File saved successfully!", "Success");
//                                                    ^^^^^^^^^ Caption
```

**VB.NET:**
```vb
MessageBoxAdv.Show(Me, "File saved successfully!", "Success")
'                                                   ^^^^^^^^^ Caption
```

### Best Practices

- Keep caption concise (2-5 words)
- Use title case: "Save Changes", "Error", "Confirm Delete"
- Match caption to message context:
  - Success operations: "Success", "Complete", "Saved"
  - Errors: "Error", "Failed", "Connection Error"
  - Confirmations: "Confirm", "Are You Sure?", "Unsaved Changes"
  - Warnings: "Warning", "Caution", "Attention"

---

## Message Text

The main message displayed in the dialog body.

### Parameter

**Type:** `string`  
**Position:** 2nd parameter in `Show()` method

### Usage

**C#:**
```csharp
MessageBoxAdv.Show(this, "Do you want to save your changes?", "Unsaved Changes");
//                       ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Message text
```

**VB.NET:**
```vb
MessageBoxAdv.Show(Me, "Do you want to save your changes?", "Unsaved Changes")
'                      ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Message text
```

### Multi-line Messages

Use `\n` or `Environment.NewLine` for line breaks:

**C#:**
```csharp
string message = "Failed to connect to database.\n\n" +
                 "Server: db.company.com\n" +
                 "Error: Connection timeout";

MessageBoxAdv.Show(this, message, "Connection Error", 
    MessageBoxButtons.OK, MessageBoxIcon.Error);
```

**VB.NET:**
```vb
Dim message As String = "Failed to connect to database." & vbCrLf & vbCrLf & _
                        "Server: db.company.com" & vbCrLf & _
                        "Error: Connection timeout"

MessageBoxAdv.Show(Me, message, "Connection Error", _
    MessageBoxButtons.OK, MessageBoxIcon.Error)
```

---

## Button Combinations

MessageBoxAdv supports multiple button combinations using the `MessageBoxButtons` enum.


# Button Parameters and Features

Concise reference for `MessageBoxAdv` button/icon/behavior parameters. Examples are C#-first; a single VB.NET parity example is provided at the end.

## Table of Contents
- Caption
- Message text
- Buttons (enum)
- Icons
- RTL
- Details view
- Resizing
- Examples & VB parity

---

## Caption

- Type: `string` (3rd parameter to `Show`)
- Keep captions short and contextual (2–5 words).

**C#:**
```csharp
MessageBoxAdv.Show(this, "Operation completed.", "Success");
```

---

## Message text

- Type: `string` (2nd parameter)
- Use `\n` or `Environment.NewLine` for line breaks in multi-line messages.

**C#:**
```csharp
string msg = "Failed to connect to database.\nServer: db.company.com";
MessageBoxAdv.Show(this, msg, "Connection Error", MessageBoxButtons.OK, MessageBoxIcon.Error);
```

---

## Buttons (common `MessageBoxButtons` values)

| Value | Typical use |
|---|---|
| `OK` | Simple notification |
| `OKCancel` | Confirmable action |
| `YesNo` | Binary decision |
| `YesNoCancel` | Save/close prompts |
| `RetryCancel` | Recoverable errors |
| `AbortRetryIgnore` | Batch processing error handling |

**C# example (OKCancel):**
```csharp
var r = MessageBoxAdv.Show(this, "Proceed with update?", "Confirm Update", MessageBoxButtons.OKCancel, MessageBoxIcon.Question);
if (r == DialogResult.OK) ApplyUpdate();
```

---

## Icons

Use the `MessageBoxIcon` enum for standard icons; custom images may be passed as an `Image` with size.

**C# (built-in):**
```csharp
MessageBoxAdv.Show(this, "Delete completed.", "Done", MessageBoxButtons.OK, MessageBoxIcon.Information);
```

**C# (custom):**
```csharp
Image icon = Properties.Resources.AppIcon;
MessageBoxAdv.Show(this, "Upload complete", "Upload", MessageBoxButtons.OK, icon, new Size(48,48));
```

---

## Right-to-Left (RTL)

Set `MessageBoxAdv.RightToLeft = RightToLeft.Yes` to enable RTL layout (reversed button order, right-aligned text).

**C#:**
```csharp
MessageBoxAdv.RightToLeft = RightToLeft.Yes;
MessageBoxAdv.Show(this, "هل تريد حفظ التغييرات؟", "تغييرات غير محفوظة", MessageBoxButtons.YesNo, MessageBoxIcon.Question);
MessageBoxAdv.RightToLeft = RightToLeft.No;
```

---

## Details view

Pass a `details` string as the last parameter to `Show()` to enable an expandable details pane (stack traces, logs).

**C#:**
```csharp
string details = "Stack trace...\nMore info...";
MessageBoxAdv.Show(this, "Operation failed.", "Error", MessageBoxButtons.RetryCancel, MessageBoxIcon.Error, details);
```

---

## Resizing

Toggle `MessageBoxAdv.CanResize = true` to allow user resizing; combine with details for long content.

**C#:**
```csharp
MessageBoxAdv.CanResize = true;
MessageBoxAdv.Show(this, "Long message...", "Info", MessageBoxButtons.OK, MessageBoxIcon.Information);
MessageBoxAdv.CanResize = false;
```

---

## Short examples + VB parity

**C# (retry loop pattern):**
```csharp
bool success = false;
while (!success)
{
    try { Connect(); success = true; }
    catch (Exception ex)
    {
        var r = MessageBoxAdv.Show(this, $"Connect failed:\n{ex.Message}", "Connection Error", MessageBoxButtons.RetryCancel, MessageBoxIcon.Error);
        if (r == DialogResult.Cancel) break;
    }
}
```

**VB.NET parity (single example):**
```vb
### YesNo Buttons
Dim result As DialogResult = MessageBoxAdv.Show(Me, "Do you want to retry?", "Connection Error", MessageBoxButtons.RetryCancel, MessageBoxIcon.Error)
If result = DialogResult.Retry Then
    ' Retry logic here
End If
```

---

## Best practices
- Use appropriate button sets for the decision complexity
- Match icon severity to message importance
- Keep captions concise
- Use details for technical diagnostics only

---

## Next steps
- See [visual-styles.md](visual-styles.md) for theming
- See [localization.md](localization.md) for multilanguage guidance


Simple binary decision.

**C#:**
```csharp
DialogResult result = MessageBoxAdv.Show(this, 
    "Do you want to overwrite existing file?", 
    "File Exists", 
    MessageBoxButtons.YesNo, 
    MessageBoxIcon.Warning);

if (result == DialogResult.Yes)
{
    OverwriteFile();
}
else
{
    ChooseDifferentFile();
}
```

**VB.NET:**
```vb
Dim result As DialogResult = MessageBoxAdv.Show(Me, _
    "Do you want to overwrite existing file?", _
    "File Exists", _
    MessageBoxButtons.YesNo, _
    MessageBoxIcon.Warning)

If result = DialogResult.Yes Then
    OverwriteFile()
Else
    ChooseDifferentFile()
End If
```

**Return Values:** `DialogResult.Yes` or `DialogResult.No`

---

### YesNoCancel Buttons

Three-option decision, commonly for save confirmations.

**C#:**
```csharp
DialogResult result = MessageBoxAdv.Show(this, 
    "Do you want to save changes to Document1.txt?", 
    "Unsaved Changes", 
    MessageBoxButtons.YesNoCancel, 
    MessageBoxIcon.Question);

switch (result)
{
    case DialogResult.Yes:
        SaveDocument();
        CloseDocument();
        break;
    case DialogResult.No:
        CloseDocument();
        break;
    case DialogResult.Cancel:
        // Stay in editor
        break;
}
```

**VB.NET:**
```vb
Dim result As DialogResult = MessageBoxAdv.Show(Me, _
    "Do you want to save changes to Document1.txt?", _
    "Unsaved Changes", _
    MessageBoxButtons.YesNoCancel, _
    MessageBoxIcon.Question)

Select Case result
    Case DialogResult.Yes
        SaveDocument()
        CloseDocument()
    Case DialogResult.No
        CloseDocument()
    Case DialogResult.Cancel
        ' Stay in editor
End Select
```

**Return Values:** `DialogResult.Yes`, `DialogResult.No`, or `DialogResult.Cancel`

---

### RetryCancel Buttons

Error recovery with retry option.

**C#:**
```csharp
bool success = false;

while (!success)
{
    try
    {
        ConnectToDatabase();
        success = true;
    }
    catch (Exception ex)
    {
        DialogResult result = MessageBoxAdv.Show(this, 
            $"Database connection failed:\n{ex.Message}", 
            "Connection Error", 
            MessageBoxButtons.RetryCancel, 
            MessageBoxIcon.Error);

        if (result == DialogResult.Cancel)
        {
            break;
        }
    }
}
```

**VB.NET:**
```vb
Dim success As Boolean = False

While Not success
    Try
        ConnectToDatabase()
        success = True
    Catch ex As Exception
        Dim result As DialogResult = MessageBoxAdv.Show(Me, _
            $"Database connection failed:{vbCrLf}{ex.Message}", _
            "Connection Error", _
            MessageBoxButtons.RetryCancel, _
            MessageBoxIcon.Error)

        If result = DialogResult.Cancel Then
            Exit While
        End If
    End Try
End While
```

**Return Values:** `DialogResult.Retry` or `DialogResult.Cancel`

---

### AbortRetryIgnore Buttons

Multi-option error handling.

**C#:**
```csharp
DialogResult result = MessageBoxAdv.Show(this, 
    "An error occurred while processing file 5 of 10.\n\n" +
    "Abort: Stop all processing\n" +
    "Retry: Try processing this file again\n" +
    "Ignore: Skip this file and continue", 
    "Processing Error", 
    MessageBoxButtons.AbortRetryIgnore, 
    MessageBoxIcon.Error);

switch (result)
{
    case DialogResult.Abort:
        StopProcessing();
        break;
    case DialogResult.Retry:
        RetryCurrentFile();
        break;
    case DialogResult.Ignore:
        SkipToNextFile();
        break;
}
```

**VB.NET:**
```vb
Dim result As DialogResult = MessageBoxAdv.Show(Me, _
    "An error occurred while processing file 5 of 10." & vbCrLf & vbCrLf & _
    "Abort: Stop all processing" & vbCrLf & _
    "Retry: Try processing this file again" & vbCrLf & _
    "Ignore: Skip this file and continue", _
    "Processing Error", _
    MessageBoxButtons.AbortRetryIgnore, _
    MessageBoxIcon.Error)

Select Case result
    Case DialogResult.Abort
        StopProcessing()
    Case DialogResult.Retry
        RetryCurrentFile()
    Case DialogResult.Ignore
        SkipToNextFile()
End Select
```

**Return Values:** `DialogResult.Abort`, `DialogResult.Retry`, or `DialogResult.Ignore`

---

## Icon Support

MessageBoxAdv supports both built-in system icons and custom images.

### Built-in Icons

Use the `MessageBoxIcon` enum for standard icons.

**Available Icons:**

| Icon | Enum Value | Appearance | Use Cases |
|------|------------|------------|-----------|
| ℹ️ | `Information` or `Asterisk` | Blue (i) | Success, notifications, informational messages |
| ❓ | `Question` | Blue (?) | Confirmations, user decisions |
| ⚠️ | `Warning` or `Exclamation` | Yellow (!) | Cautions, potential issues, destructive actions |
| ❌ | `Error`, `Hand`, or `Stop` | Red (X) | Failures, exceptions, critical problems |
| - | `None` | No icon | Custom scenarios where icon not needed |

### Using Built-in Icons

**C#:**
```csharp
// Information icon
MessageBoxAdv.Show(this, "Operation completed.", "Success", 
    MessageBoxButtons.OK, MessageBoxIcon.Information);

// Question icon
MessageBoxAdv.Show(this, "Continue with operation?", "Confirm", 
    MessageBoxButtons.YesNo, MessageBoxIcon.Question);

// Warning icon
MessageBoxAdv.Show(this, "This action cannot be undone.", "Warning", 
    MessageBoxButtons.OKCancel, MessageBoxIcon.Warning);

// Error icon
MessageBoxAdv.Show(this, "Failed to save file.", "Error", 
    MessageBoxButtons.OK, MessageBoxIcon.Error);
```

**VB.NET:**
```vb
' Information icon
MessageBoxAdv.Show(Me, "Operation completed.", "Success", _
    MessageBoxButtons.OK, MessageBoxIcon.Information)

' Question icon
MessageBoxAdv.Show(Me, "Continue with operation?", "Confirm", _
    MessageBoxButtons.YesNo, MessageBoxIcon.Question)

' Warning icon
MessageBoxAdv.Show(Me, "This action cannot be undone.", "Warning", _
    MessageBoxButtons.OKCancel, MessageBoxIcon.Warning)

' Error icon
MessageBoxAdv.Show(Me, "Failed to save file.", "Error", _
    MessageBoxButtons.OK, MessageBoxIcon.Error)
```

---

### Custom Icons

Load custom image files with specified size.

**Method Signature:**
```csharp
DialogResult Show(IWin32Window owner, string text, string caption, 
    MessageBoxButtons buttons, Image icon, Size iconSize)
```

**C#:**
```csharp
// Load custom icon from file
Image customIcon = Image.FromFile("custom_icon.png");

MessageBoxAdv.Show(this, 
    "Upload completed successfully!", 
    "Cloud Backup", 
    MessageBoxButtons.OK, 
    customIcon, 
    new Size(48, 48));

// Load from embedded resource
Image resourceIcon = Properties.Resources.AppIcon;

MessageBoxAdv.Show(this, 
    "Welcome to the application!", 
    "Welcome", 
    MessageBoxButtons.OK, 
    resourceIcon, 
    new Size(64, 64));
```

**VB.NET:**
```vb
' Load custom icon from file
Dim customIcon As Image = Image.FromFile("custom_icon.png")

MessageBoxAdv.Show(Me, _
    "Upload completed successfully!", _
    "Cloud Backup", _
    MessageBoxButtons.OK, _
    customIcon, _
    New Size(48, 48))

' Load from embedded resource
Dim resourceIcon As Image = My.Resources.AppIcon

MessageBoxAdv.Show(Me, _
    "Welcome to the application!", _
    "Welcome", _
    MessageBoxButtons.OK, _
    resourceIcon, _
    New Size(64, 64))
```

**Icon Size Recommendations:**
- Small: 32x32 pixels
- Medium: 48x48 pixels (recommended)
- Large: 64x64 pixels

---

## Right-to-Left Support

Enable right-to-left layout for Arabic, Hebrew, or other RTL languages.

### RightToLeft Property

**Type:** `RightToLeft` enum  
**Values:** `Yes`, `No` (default)

### Usage

**C#:**
```csharp
// Enable RTL layout
MessageBoxAdv.RightToLeft = RightToLeft.Yes;

MessageBoxAdv.Show(this, 
    "هل تريد حفظ التغييرات؟",  // Arabic text
    "تغييرات غير محفوظة", 
    MessageBoxButtons.YesNo, 
    MessageBoxIcon.Question);

// Reset to LTR (optional)
MessageBoxAdv.RightToLeft = RightToLeft.No;
```

**VB.NET:**
```vb
' Enable RTL layout
MessageBoxAdv.RightToLeft = RightToLeft.Yes

MessageBoxAdv.Show(Me, _
    "هل تريد حفظ التغييرات؟", _  ' Arabic text
    "تغييرات غير محفوظة", _
    MessageBoxButtons.YesNo, _
    MessageBoxIcon.Question)

' Reset to LTR (optional)
MessageBoxAdv.RightToLeft = RightToLeft.No
```

**RTL Layout Effects:**
- Text alignment: Right-aligned
- Button order: Reversed (e.g., Cancel appears before OK)
- Icon position: Right side instead of left
- Dialog flow: Right-to-left reading order

---

## Details View

The details view provides an expandable pane for additional information without cluttering the main message.

### Details Parameter

**Type:** `string`  
**Position:** Last parameter in `Show()` method

### Usage

**C#:**
```csharp
string mainMessage = "Failed to connect to database.";
string details = "Connection Details:\n" +
                 "Server: db.company.com\n" +
                 "Port: 1433\n" +
                 "Database: ProductionDB\n" +
                 "User: app_user\n\n" +
                 "Error: Timeout expired. The timeout period elapsed prior to obtaining a connection from the pool.\n\n" +
                 "Stack Trace:\n" +
                 "at System.Data.SqlClient.SqlConnection.Open()";

MessageBoxAdv.Show(this, 
    mainMessage, 
    "Connection Error", 
    MessageBoxButtons.RetryCancel, 
    MessageBoxIcon.Error, 
    details);
```

**VB.NET:**
```vb
Dim mainMessage As String = "Failed to connect to database."
Dim details As String = "Connection Details:" & vbCrLf & _
                        "Server: db.company.com" & vbCrLf & _
                        "Port: 1433" & vbCrLf & _
                        "Database: ProductionDB" & vbCrLf & _
                        "User: app_user" & vbCrLf & vbCrLf & _
                        "Error: Timeout expired." & vbCrLf & vbCrLf & _
                        "Stack Trace:" & vbCrLf & _
                        "at System.Data.SqlClient.SqlConnection.Open()"

MessageBoxAdv.Show(Me, _
    mainMessage, _
    "Connection Error", _
    MessageBoxButtons.RetryCancel, _
    MessageBoxIcon.Error, _
    details)
```

**User Experience:**
1. Message box initially shows collapsed (compact view)
2. "Details" button appears at bottom
3. Clicking "Details" expands pane showing detail text
4. Clicking "Details" again collapses the pane
5. Message box resizes automatically when expanding/collapsing

### Details with Custom Icon

**C#:**
```csharp
Image customIcon = Properties.Resources.ErrorIcon;

string details = "System Information:\n" +
                 $"OS: {Environment.OSVersion}\n" +
                 $".NET Version: {Environment.Version}\n" +
                 $"Machine: {Environment.MachineName}\n" +
                 $"User: {Environment.UserName}";

MessageBoxAdv.Show(this, 
    "An unexpected error occurred.", 
    "Application Error", 
    MessageBoxButtons.OK, 
    customIcon, 
    new Size(48, 48), 
    details);
```

---

## Resizing Support

Allow users to resize the message box at runtime by dragging the gripper.

### CanResize Property

**Type:** `bool`  
**Default:** `false`

### Usage

**C#:**
```csharp
// Enable resizing
MessageBoxAdv.CanResize = true;

MessageBoxAdv.Show(this, 
    "This is a long message that users might want to resize to read more comfortably. " +
    "When CanResize is enabled, a gripper appears at the bottom-right corner allowing " +
    "the user to drag and resize the message box window.", 
    "Resizable Message Box", 
    MessageBoxButtons.OK, 
    MessageBoxIcon.Information);

// Disable resizing (optional)
MessageBoxAdv.CanResize = false;
```

**VB.NET:**
```vb
' Enable resizing
MessageBoxAdv.CanResize = True

MessageBoxAdv.Show(Me, _
    "This is a long message that users might want to resize to read more comfortably. " & _
    "When CanResize is enabled, a gripper appears at the bottom-right corner allowing " & _
    "the user to drag and resize the message box window.", _
    "Resizable Message Box", _
    MessageBoxButtons.OK, _
    MessageBoxIcon.Information)

' Disable resizing (optional)
MessageBoxAdv.CanResize = False
```

**Resizing Behavior:**
- Gripper icon appears at bottom-right corner
- User can drag to increase or decrease size
- Minimum size constraints prevent excessive shrinking
- Content reflows as window resizes
- Useful for long messages or detailed error information

### Combined with Details View

**C#:**
```csharp
MessageBoxAdv.CanResize = true;

string longDetails = "This is a very long details section with extensive information " +
                     "that benefits from both the expandable details pane and the ability " +
                     "to resize the window for better readability.\n\n" +
                     string.Join("\n", Enumerable.Range(1, 20).Select(i => $"Detail line {i}"));

MessageBoxAdv.Show(this, 
    "Operation completed with warnings.", 
    "Warnings", 
    MessageBoxButtons.OK, 
    MessageBoxIcon.Warning, 
    longDetails);
```

---

## Complete Examples

### Example: File Operation with All Features

**C#:**
```csharp
using Syncfusion.Windows.Forms;
using System.Drawing;

public void ProcessFile(string filePath)
{
    try
    {
        // ... file processing logic ...
        
        MessageBoxAdv.Show(this, 
            $"File processed successfully:\n{Path.GetFileName(filePath)}", 
            "Success", 
            MessageBoxButtons.OK, 
            MessageBoxIcon.Information);
    }
    catch (Exception ex)
    {
        // Enable resizing for long error messages
        MessageBoxAdv.CanResize = true;
        
        // Load custom error icon
        Image errorIcon = Properties.Resources.FileErrorIcon;
        
        // Prepare detailed error information
        string details = $"File Path: {filePath}\n" +
                        $"Error Type: {ex.GetType().Name}\n" +
                        $"Message: {ex.Message}\n\n" +
                        $"Stack Trace:\n{ex.StackTrace}";
        
        DialogResult result = MessageBoxAdv.Show(this, 
            "Failed to process file. Would you like to retry?", 
            "File Processing Error", 
            MessageBoxButtons.RetryCancel, 
            errorIcon, 
            new Size(48, 48), 
            details);
        
        if (result == DialogResult.Retry)
        {
            ProcessFile(filePath); // Retry
        }
        
        // Reset resizing setting
        MessageBoxAdv.CanResize = false;
    }
}
```

---

## Best Practices

### Button Selection Guidelines

- **OK**: Simple notifications, acknowledgments
- **OKCancel**: Optional operations users can decline
- **YesNo**: Binary decisions without cancellation
- **YesNoCancel**: Decisions with opt-out (e.g., save prompts)
- **RetryCancel**: Recoverable errors
- **AbortRetryIgnore**: Batch operations with failure handling

### Icon Consistency

Match icons to message severity:
```csharp
// ✓ Good: Warning icon for destructive action
MessageBoxAdv.Show(this, "Delete all files?", "Warning", 
    MessageBoxButtons.YesNo, MessageBoxIcon.Warning);

// ✗ Bad: Information icon for destructive action
MessageBoxAdv.Show(this, "Delete all files?", "Confirm", 
    MessageBoxButtons.YesNo, MessageBoxIcon.Information);
```

### Details View Usage

Use details for:
- Stack traces
- Technical diagnostics
- Extended error information
- System configuration details
- Log excerpts

Don't use details for:
- Critical information users must see
- Primary action instructions
- Short supplementary text (include in main message instead)

---

## Next Steps

- **Visual Styles:** Apply Office and Metro themes → [visual-styles.md](visual-styles.md)
- **Metro Customization:** Customize Metro theme colors → [metro-customization.md](metro-customization.md)
- **Localization:** Implement multilanguage support → [localization.md](localization.md)
