---
name: syncfusion-winforms-messagebox
description: Implement and configure Syncfusion MessageBoxAdv control in Windows Forms - an enhanced message box with themes, custom icons, details view, and localization support. Use when displaying modal messages, confirmations, errors, warnings, or information dialogs. Covers Office themes, message box appearance customization, multilanguage dialogs, and replacing standard MessageBox with styled alternatives.
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
---

# Implementing Message Boxes (MessageBoxAdv)

An advanced message box control for Windows Forms that provides themed dialogs, custom icons, expandable details, localization support, and complete appearance customization.

## When to Use This Skill

Use this skill when you need to:
- **Modal Dialogs**: Display messages, confirmations, warnings, or errors in modal windows
- **Themed Message Boxes**: Apply Office 2007/2010/2013/2016 or Metro themes to match application style
- **Custom Icons**: Show built-in or custom icons in message boxes
- **Confirmation Dialogs**: Implement Yes/No, OK/Cancel, or Retry/Cancel user prompts
- **Details View**: Provide expandable detail panes for additional information
- **Multilanguage Dialogs**: Localize button text and messages for international applications
- **Resizable Dialogs**: Allow users to resize message boxes at runtime
- **Right-to-Left Support**: Display message boxes in RTL layout for Arabic/Hebrew applications

## Component Overview

The **MessageBoxAdv** is an enhanced replacement for the standard Windows Forms MessageBox that provides:
- Static `Show()` method pattern (no instantiation needed)
- Multiple button combinations (Ok, OkCancel, YesNo, YesNoCancel, RetryCancel, AbortRetryIgnore)
- Built-in and custom icon support
- Six visual themes with multiple color schemes (15+ style variations)
- Expandable details pane for additional information
- Complete localization support via `ILocalizationProvider`
- Runtime resizing with gripper
- Right-to-left layout support

**Key Capabilities:**
- `MessageBoxAdv.Show()` static method for displaying dialogs
- `DialogResult` return value for capturing user response
- `MessageBoxStyle` property for theme selection
- `MetroColorTable` for Metro theme customization
- `CanResize` property for runtime size adjustment
- Details view with expandable/collapsible detail text
- Custom icon loading with size specification

**Return Values:**
MessageBoxAdv returns `DialogResult` enum values: OK, Cancel, Yes, No, Retry, Abort, Ignore, None

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

**When to read:** Starting new implementation, setting up dependencies, first-time usage, basic Show() method

**Topics covered:**
- Assembly deployment (Syncfusion.Shared.Base.dll)
- NuGet package installation (Syncfusion.Shared.Base)
- Namespace requirements (Syncfusion.Windows.Forms)
- Static Show() method usage pattern
- Basic message box examples
- Show() method overloads
- DialogResult return value handling
- Simple alert and confirmation examples

---

### Button Parameters and Features
📄 **Read:** [references/button-parameters.md](references/button-parameters.md)

**When to read:** Configuring button combinations, adding icons, enabling features (RTL, details, resizing)

**Topics covered:**
- Caption bar text parameter
- Message text parameter
- Button combinations (MessageBoxButtons enum: OK, OKCancel, YesNo, YesNoCancel, RetryCancel, AbortRetryIgnore)
- Icon support (MessageBoxIcon enum: Asterisk, Error, Exclamation, Hand, Information, None, Question, Stop, Warning)
- Custom icon loading (Image parameter with Size)
- Right-to-left support (RightToLeft property)
- Details view (expandable/collapsible detail pane with "Details" button)
- Resizing support (CanResize property with gripper at bottom-right)
- Complete examples for each button type
- Icon positioning and sizing

---

### Visual Styles and Themes
📄 **Read:** [references/visual-styles.md](references/visual-styles.md)

**When to read:** Applying themes, matching application style, using Office color schemes

**Topics covered:**
- MessageBoxStyle property (Default, Office2007, Office2010, Metro, Office2013, Office2016)
- Default theme
- Office2007 theme with color schemes (Black, Blue, Silver, Managed)
- Office2010 theme with color schemes (Black, Blue, Silver, Managed)
- Metro theme (modern flat design)
- Office2013 theme (DarkGray, LightGray, White)
- Office2016 theme (Colorful, White, DarkGray)
- Theme enumeration usage (Office2007Theme, Office2010Theme, Office2013Theme, Office2016Theme)
- Managed color customization (Office2007Colors.ApplyManagedColors, Office2010Colors.ApplyManagedColors)
- Complete examples for all themes and color schemes

---

### Metro Theme Customization
📄 **Read:** [references/metro-customization.md](references/metro-customization.md)

**When to read:** Customizing Metro theme colors, branding message boxes, matching corporate colors

**Topics covered:**
- MetroColorTable class overview
- ForeColor property (message text color)
- BackColor property (dialog background)
- BorderColor property (dialog border)
- CaptionBarColor property (title bar background)
- CaptionForeColor property (title bar text color)
- Button background colors (OKButtonBackColor, YesButtonBackColor, NoButtonBackColor, CancelButtonBackColor, RetryButtonBackColor, AbortButtonBackColor, IgnoreButtonBackColor)
- Button foreground colors (button text colors)
- CloseButtonColor and CloseButtonHoverColor
- Complete Metro customization examples
- Color coordination best practices
- Custom branded message box creation

---

### Localization Support
📄 **Read:** [references/localization.md](references/localization.md)

**When to read:** Implementing multilanguage applications, localizing button text, international deployments

**Topics covered:**
- Localization overview and workflow
- ILocalizationProvider interface implementation
- LocalizationProvider.Provider property initialization
- GetLocalizedString() method implementation
- ResourceIdentifiers class constants (Yes, No, OK, Cancel, Retry, Abort, Ignore, Details, Close)
- Culture-specific string mapping
- Initialization before InitializeComponent
- Switch statement pattern for resource lookup
- Complete localization examples (German, Spanish, French)
- Best practices for multilanguage support
- Custom Localizer class creation

---

## Quick Start Example

### Basic Message Box with Metro Theme

**C#:**
```csharp
using Syncfusion.Windows.Forms;

// Set Metro theme globally
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro;

// Display simple message box
MessageBoxAdv.Show(this, "File saved successfully!", "Success", 
    MessageBoxButtons.OK, MessageBoxIcon.Information);

// Confirmation dialog with Yes/No
DialogResult result = MessageBoxAdv.Show(this, "Save changes?", "File Modified", 
    MessageBoxButtons.YesNo, MessageBoxIcon.Question);

if (result == DialogResult.Yes)
{
    // Save changes
}
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms

' Set Metro theme globally
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro

' Display simple message box
MessageBoxAdv.Show(Me, "File saved successfully!", "Success", _
    MessageBoxButtons.OK, MessageBoxIcon.Information)

' Confirmation dialog with Yes/No
Dim result As DialogResult = MessageBoxAdv.Show(Me, "Save changes?", "File Modified", _
    MessageBoxButtons.YesNo, MessageBoxIcon.Question)

If result = DialogResult.Yes Then
    ' Save changes
End If
```

---

## Common Patterns

### Pattern 1: File Save Confirmation Dialog

**Scenario:** Prompt user to save changes before closing

**C#:**
```csharp
using Syncfusion.Windows.Forms;

// Set Office2016 Colorful theme
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2016;
MessageBoxAdv.Office2016Theme = Office2016Theme.Colorful;

// Show confirmation with YesNoCancel
DialogResult result = MessageBoxAdv.Show(
    this,
    "Do you want to save changes to Document1.txt?",
    "Unsaved Changes",
    MessageBoxButtons.YesNoCancel,
    MessageBoxIcon.Question
);

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
        // Do nothing, stay in editor
        break;
}
```

---

### Pattern 2: Error Message with Custom Icon and Details View

**Scenario:** Display error with detailed stack trace or log information

**C#:**
```csharp
using Syncfusion.Windows.Forms;
using System.Drawing;

// Load custom error icon
Image customIcon = Image.FromFile("error_icon.png");

// Set theme
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro;

// Show with details view
MessageBoxAdv.Show(
    this,
    "Failed to connect to database server.",
    "Connection Error",
    MessageBoxButtons.RetryCancel,
    customIcon,
    new Size(48, 48),
    "Error Details:\n" +
    "Server: db.company.com\n" +
    "Port: 1433\n" +
    "Timeout: Connection timeout after 30 seconds\n" +
    "Stack Trace: at System.Data.SqlClient.SqlConnection.Open()"
);
```

---

### Pattern 3: Localized Message Box (German)

**Scenario:** Display message box with German button text and messages

**C#:**
```csharp
using Syncfusion.Windows.Forms;
using System.Globalization;

// Implement custom localizer
public class GermanLocalizer : ILocalizationProvider
{
    public string GetLocalizedString(CultureInfo culture, string name, object obj)
    {
        switch (name)
        {
            case ResourceIdentifiers.Yes:
                return "Ja";
            case ResourceIdentifiers.No:
                return "Nein";
            case ResourceIdentifiers.OK:
                return "OK";
            case ResourceIdentifiers.Cancel:
                return "Abbrechen";
            case ResourceIdentifiers.Retry:
                return "Wiederholen";
            case ResourceIdentifiers.Abort:
                return "Abbrechen";
            case ResourceIdentifiers.Ignore:
                return "Ignorieren";
            case ResourceIdentifiers.Details:
                return "Details";
            default:
                return string.Empty;
        }
    }
}

// Initialize localizer before showing message box
LocalizationProvider.Provider = new GermanLocalizer();

// Show localized message box
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2013;
MessageBoxAdv.Office2013Theme = Office2013Theme.White;

MessageBoxAdv.Show(
    this,
    "Möchten Sie die Änderungen speichern?",
    "Datei geändert",
    MessageBoxButtons.YesNoCancel,
    MessageBoxIcon.Question
);
```

---

## Key Properties

### Static Properties

| Property | Type | Description |
|----------|------|-------------|
| `MessageBoxStyle` | `MessageBoxAdv.Style` | Sets theme: Default, Office2007, Office2010, Metro, Office2013, Office2016 |
| `Office2007Theme` | `Office2007Theme` | Color scheme for Office2007: Black, Blue, Silver, Managed |
| `Office2010Theme` | `Office2010Theme` | Color scheme for Office2010: Black, Blue, Silver, Managed |
| `Office2013Theme` | `Office2013Theme` | Color scheme for Office2013: DarkGray, LightGray, White |
| `Office2016Theme` | `Office2016Theme` | Color scheme for Office2016: Colorful, White, DarkGray |
| `MetroColorTable` | `MessageBoxAdvMetroColorTable` | Metro theme color customization |
| `RightToLeft` | `RightToLeft` | Enable RTL layout: Yes, No |
| `CanResize` | `bool` | Enable runtime resizing with gripper |

### Show() Method Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `owner` | `IWin32Window` | Parent window (typically `this` or form instance) |
| `text` | `string` | Message text to display |
| `caption` | `string` | Title bar text |
| `buttons` | `MessageBoxButtons` | Button combination: OK, OKCancel, YesNo, YesNoCancel, RetryCancel, AbortRetryIgnore |
| `icon` | `MessageBoxIcon` | Built-in icon: Asterisk, Error, Exclamation, Hand, Information, None, Question, Stop, Warning |
| `icon` | `Image` | Custom icon image |
| `iconSize` | `Size` | Custom icon size (width, height) |
| `details` | `string` | Expandable detail text (shows "Details" button) |

### Return Value

**Type:** `DialogResult`

**Values:** OK, Cancel, Yes, No, Retry, Abort, Ignore, None

---

## Common Use Cases

### 1. Application Exit Confirmation
User tries to close application with unsaved work:
```csharp
DialogResult result = MessageBoxAdv.Show(this, 
    "You have unsaved changes. Exit anyway?", 
    "Confirm Exit", 
    MessageBoxButtons.YesNo, 
    MessageBoxIcon.Warning);

if (result == DialogResult.Yes)
{
    Application.Exit();
}
```

### 2. File Operation Errors
Display error with retry option:
```csharp
DialogResult result = MessageBoxAdv.Show(this, 
    "File is locked by another process.", 
    "File Access Error", 
    MessageBoxButtons.RetryCancel, 
    MessageBoxIcon.Error);

if (result == DialogResult.Retry)
{
    AttemptFileAccess();
}
```

### 3. Information Messages
Simple notification without choices:
```csharp
MessageBoxAdv.Show(this, 
    "Backup completed successfully!", 
    "Backup Complete", 
    MessageBoxButtons.OK, 
    MessageBoxIcon.Information);
```

### 4. Delete Confirmation
Confirm destructive action:
```csharp
DialogResult result = MessageBoxAdv.Show(this, 
    "Are you sure you want to delete this record? This cannot be undone.", 
    "Confirm Delete", 
    MessageBoxButtons.YesNo, 
    MessageBoxIcon.Exclamation);

if (result == DialogResult.Yes)
{
    DeleteRecord();
}
```

### 5. Themed Corporate Dialogs
Match message box to corporate branding:
```csharp
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro;
MessageBoxAdv.MetroColorTable.CaptionBarColor = Color.FromArgb(0, 120, 215); // Corporate blue
MessageBoxAdv.MetroColorTable.YesButtonBackColor = Color.FromArgb(0, 120, 215);
MessageBoxAdv.MetroColorTable.NoButtonBackColor = Color.FromArgb(82, 82, 82);

MessageBoxAdv.Show(this, 
    "Upload to cloud storage?", 
    "Company Portal", 
    MessageBoxButtons.YesNo, 
    MessageBoxIcon.Question);
```

---

## Additional Resources

**Assembly:** Syncfusion.Shared.Base.dll  
**Namespace:** Syncfusion.Windows.Forms  
**NuGet Package:** Syncfusion.Shared.Base  
**Minimum .NET Framework:** 4.5  

**Key Classes:**
- `MessageBoxAdv` - Main static class for showing message boxes
- `MessageBoxAdvMetroColorTable` - Metro theme color customization
- `ILocalizationProvider` - Interface for localization
- `LocalizationProvider` - Localization provider registration
- `ResourceIdentifiers` - Resource string constants for localization

**Related Skills:**
- Form styling and theming
- Dialog management
- User input validation
- Error handling patterns
