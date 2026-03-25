# Metro Theme Customization

This guide covers comprehensive customization of the Metro theme using the MetroColorTable class to create branded and custom-styled message boxes.

## Table of Contents
- [Overview](#overview)
- [MetroColorTable Properties](#metrocolortable-properties)
- [Background and Foreground Colors](#background-and-foreground-colors)
- [Caption Bar Customization](#caption-bar-customization)
- [Button Color Customization](#button-color-customization)
- [Border and Close Button](#border-and-close-button)
- [Complete Examples](#complete-examples)

---

## Overview

The Metro theme provides extensive customization through the `MetroColorTable` property, allowing you to match corporate branding, create dark modes, or design unique color schemes.

### Why Customize Metro Theme?

- **Brand Consistency:** Match message boxes to corporate colors
- **User Experience:** Create cohesive UI across application
- **Dark Mode:** Implement dark-themed message boxes
- **Accessibility:** Adjust colors for better contrast
- **Unique Design:** Stand out with custom styling

### Basic Customization Pattern

**C#:**
```csharp
// 1. Set Metro style
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro;

// 2. Customize colors via MetroColorTable
MessageBoxAdv.MetroColorTable.CaptionBarColor = Color.FromArgb(0, 120, 215);
MessageBoxAdv.MetroColorTable.YesButtonBackColor = Color.FromArgb(0, 120, 215);

// 3. Show message box
MessageBoxAdv.Show(this, "Message", "Title");
```

---

## MetroColorTable Properties

The `MessageBoxAdvMetroColorTable` class provides 15+ properties for customization.

### Property Categories

| Category | Properties | Count |
|----------|-----------|-------|
| **Background** | BackColor, ForeColor | 2 |
| **Caption Bar** | CaptionBarColor, CaptionForeColor | 2 |
| **Borders** | BorderColor | 1 |
| **Button Backgrounds** | OKButtonBackColor, YesButtonBackColor, NoButtonBackColor, CancelButtonBackColor, RetryButtonBackColor, AbortButtonBackColor, IgnoreButtonBackColor | 7 |
| **Button Foregrounds** | (Each button has corresponding ForeColor property) | 7 |
| **Close Button** | CloseButtonColor, CloseButtonHoverColor | 2 |

### Complete Property List

```csharp
// Dialog colors
MessageBoxAdv.MetroColorTable.BackColor
MessageBoxAdv.MetroColorTable.ForeColor
MessageBoxAdv.MetroColorTable.BorderColor

// Caption bar
MessageBoxAdv.MetroColorTable.CaptionBarColor
MessageBoxAdv.MetroColorTable.CaptionForeColor

// Button background colors
MessageBoxAdv.MetroColorTable.OKButtonBackColor
MessageBoxAdv.MetroColorTable.YesButtonBackColor
MessageBoxAdv.MetroColorTable.NoButtonBackColor
MessageBoxAdv.MetroColorTable.CancelButtonBackColor
MessageBoxAdv.MetroColorTable.RetryButtonBackColor
MessageBoxAdv.MetroColorTable.AbortButtonBackColor
MessageBoxAdv.MetroColorTable.IgnoreButtonBackColor

// Button foreground colors
MessageBoxAdv.MetroColorTable.OKButtonForeColor
MessageBoxAdv.MetroColorTable.YesButtonForeColor
MessageBoxAdv.MetroColorTable.NoButtonForeColor
MessageBoxAdv.MetroColorTable.CancelButtonForeColor
MessageBoxAdv.MetroColorTable.RetryButtonForeColor
MessageBoxAdv.MetroColorTable.AbortButtonForeColor
MessageBoxAdv.MetroColorTable.IgnoreButtonForeColor

// Close button
MessageBoxAdv.MetroColorTable.CloseButtonColor
MessageBoxAdv.MetroColorTable.CloseButtonHoverColor
```

---

## Background and Foreground Colors

Control the main dialog colors.

### BackColor Property

Sets the message box background color.

**C#:**
```csharp
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro;
MessageBoxAdv.MetroColorTable.BackColor = Color.FromArgb(45, 45, 48); // Dark gray

MessageBoxAdv.Show(this, 
    "Dark mode message box", 
    "Dark Theme", 
    MessageBoxButtons.OK, 
    MessageBoxIcon.Information);
```

**VB.NET:**
```vb
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro
MessageBoxAdv.MetroColorTable.BackColor = Color.FromArgb(45, 45, 48) ' Dark gray

MessageBoxAdv.Show(Me, _
    "Dark mode message box", _
    "Dark Theme", _
    MessageBoxButtons.OK, _
    MessageBoxIcon.Information)
```

### ForeColor Property

Sets the message text color.

**C#:**
```csharp
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro;
MessageBoxAdv.MetroColorTable.BackColor = Color.FromArgb(45, 45, 48);
MessageBoxAdv.MetroColorTable.ForeColor = Color.White; // White text on dark background

MessageBoxAdv.Show(this, 
    "High contrast text for better readability", 
    "Contrast", 
    MessageBoxButtons.OK, 
    MessageBoxIcon.Information);
```

**VB.NET:**
```vb
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro
MessageBoxAdv.MetroColorTable.BackColor = Color.FromArgb(45, 45, 48)
MessageBoxAdv.MetroColorTable.ForeColor = Color.White ' White text on dark background

MessageBoxAdv.Show(Me, _
    "High contrast text for better readability", _
    "Contrast", _
    MessageBoxButtons.OK, _
    MessageBoxIcon.Information)
```

---

## Caption Bar Customization

Customize the title bar appearance.

### CaptionBarColor Property

Sets the caption bar background color.

**C#:**
```csharp
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro;
MessageBoxAdv.MetroColorTable.CaptionBarColor = Color.FromArgb(0, 120, 215); // Windows blue

MessageBoxAdv.Show(this, 
    "Message with blue caption bar", 
    "Custom Caption", 
    MessageBoxButtons.OK, 
    MessageBoxIcon.Information);
```

**VB.NET:**
```vb
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro
MessageBoxAdv.MetroColorTable.CaptionBarColor = Color.FromArgb(0, 120, 215) ' Windows blue

MessageBoxAdv.Show(Me, _
    "Message with blue caption bar", _
    "Custom Caption", _
    MessageBoxButtons.OK, _
    MessageBoxIcon.Information)
```

### CaptionForeColor Property

Sets the caption text color.

**C#:**
```csharp
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro;
MessageBoxAdv.MetroColorTable.CaptionBarColor = Color.FromArgb(0, 120, 215);
MessageBoxAdv.MetroColorTable.CaptionForeColor = Color.White;

MessageBoxAdv.Show(this, 
    "White text on blue caption bar", 
    "Branded Dialog", 
    MessageBoxButtons.OK, 
    MessageBoxIcon.Information);
```

**VB.NET:**
```vb
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro
MessageBoxAdv.MetroColorTable.CaptionBarColor = Color.FromArgb(0, 120, 215)
MessageBoxAdv.MetroColorTable.CaptionForeColor = Color.White

MessageBoxAdv.Show(Me, _
    "White text on blue caption bar", _
    "Branded Dialog", _
    MessageBoxButtons.OK, _
    MessageBoxIcon.Information)
```

### Combined Caption Customization

**C#:**
```csharp
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro;

// Corporate purple branding
MessageBoxAdv.MetroColorTable.CaptionBarColor = Color.FromArgb(106, 13, 173);
MessageBoxAdv.MetroColorTable.CaptionForeColor = Color.White;

MessageBoxAdv.Show(this, 
    "Welcome to Acme Corporation Portal!", 
    "Acme Portal", 
    MessageBoxButtons.OK, 
    MessageBoxIcon.Information);
```

---

## Button Color Customization

Customize individual button colors for visual hierarchy.

### Yes/No Button Colors

**C#:**
```csharp
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro;

// Green "Yes" button, Red "No" button
MessageBoxAdv.MetroColorTable.YesButtonBackColor = Color.FromArgb(34, 139, 34);  // Green
MessageBoxAdv.MetroColorTable.YesButtonForeColor = Color.White;
MessageBoxAdv.MetroColorTable.NoButtonBackColor = Color.FromArgb(178, 34, 34);    // Red
MessageBoxAdv.MetroColorTable.NoButtonForeColor = Color.White;

DialogResult result = MessageBoxAdv.Show(this, 
    "Delete all files permanently?", 
    "Confirm Delete", 
    MessageBoxButtons.YesNo, 
    MessageBoxIcon.Warning);
```

**VB.NET:**
```vb
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro

' Green "Yes" button, Red "No" button
MessageBoxAdv.MetroColorTable.YesButtonBackColor = Color.FromArgb(34, 139, 34)  ' Green
MessageBoxAdv.MetroColorTable.YesButtonForeColor = Color.White
MessageBoxAdv.MetroColorTable.NoButtonBackColor = Color.FromArgb(178, 34, 34)    ' Red
MessageBoxAdv.MetroColorTable.NoButtonForeColor = Color.White

Dim result As DialogResult = MessageBoxAdv.Show(Me, _
    "Delete all files permanently?", _
    "Confirm Delete", _
    MessageBoxButtons.YesNo, _
    MessageBoxIcon.Warning)
```

### OK/Cancel Button Colors

**C#:**
```csharp
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro;

// Blue OK button, Gray Cancel button
MessageBoxAdv.MetroColorTable.OKButtonBackColor = Color.FromArgb(0, 120, 215);
MessageBoxAdv.MetroColorTable.OKButtonForeColor = Color.White;
MessageBoxAdv.MetroColorTable.CancelButtonBackColor = Color.FromArgb(130, 130, 130);
MessageBoxAdv.MetroColorTable.CancelButtonForeColor = Color.White;

MessageBoxAdv.Show(this, 
    "Proceed with update?", 
    "Confirm Update", 
    MessageBoxButtons.OKCancel, 
    MessageBoxIcon.Question);
```

### Retry/Cancel Button Colors

**C#:**
```csharp
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro;

// Orange Retry button, Gray Cancel button
MessageBoxAdv.MetroColorTable.RetryButtonBackColor = Color.FromArgb(255, 140, 0);
MessageBoxAdv.MetroColorTable.RetryButtonForeColor = Color.White;
MessageBoxAdv.MetroColorTable.CancelButtonBackColor = Color.FromArgb(82, 82, 82);
MessageBoxAdv.MetroColorTable.CancelButtonForeColor = Color.White;

MessageBoxAdv.Show(this, 
    "Connection failed. Retry?", 
    "Connection Error", 
    MessageBoxButtons.RetryCancel, 
    MessageBoxIcon.Error);
```

### Abort/Retry/Ignore Button Colors

**C#:**
```csharp
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro;

// Red Abort, Orange Retry, Gray Ignore
MessageBoxAdv.MetroColorTable.AbortButtonBackColor = Color.FromArgb(178, 34, 34);
MessageBoxAdv.MetroColorTable.AbortButtonForeColor = Color.White;
MessageBoxAdv.MetroColorTable.RetryButtonBackColor = Color.FromArgb(255, 140, 0);
MessageBoxAdv.MetroColorTable.RetryButtonForeColor = Color.White;
MessageBoxAdv.MetroColorTable.IgnoreButtonBackColor = Color.FromArgb(82, 82, 82);
MessageBoxAdv.MetroColorTable.IgnoreButtonForeColor = Color.White;

MessageBoxAdv.Show(this, 
    "Error processing file 5 of 10.", 
    "Processing Error", 
    MessageBoxButtons.AbortRetryIgnore, 
    MessageBoxIcon.Error);
```

---

## Border and Close Button

Customize borders and close button appearance.

### BorderColor Property

Sets the dialog border color.

**C#:**
```csharp
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro;
MessageBoxAdv.MetroColorTable.BorderColor = Color.FromArgb(0, 120, 215); // Blue border

MessageBoxAdv.Show(this, 
    "Message with blue border", 
    "Custom Border", 
    MessageBoxButtons.OK, 
    MessageBoxIcon.Information);
```

### Close Button Colors

**C#:**
```csharp
MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro;
MessageBoxAdv.MetroColorTable.CloseButtonColor = Color.FromArgb(82, 82, 82);
MessageBoxAdv.MetroColorTable.CloseButtonHoverColor = Color.FromArgb(232, 17, 35); // Red on hover

MessageBoxAdv.Show(this, 
    "Close button turns red on hover", 
    "Hover Effect", 
    MessageBoxButtons.OK, 
    MessageBoxIcon.Information);
```

---

## Complete Examples

### Example 1: Corporate Branding (Blue Theme)

**C#:**
```csharp
using Syncfusion.Windows.Forms;
using System.Drawing;

public void ApplyCorporateBranding()
{
    MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro;
    
    // Corporate blue theme
    Color corporateBlue = Color.FromArgb(0, 102, 204);
    Color darkGray = Color.FromArgb(45, 45, 48);
    
    // Dialog
    MessageBoxAdv.MetroColorTable.BackColor = Color.White;
    MessageBoxAdv.MetroColorTable.ForeColor = darkGray;
    MessageBoxAdv.MetroColorTable.BorderColor = corporateBlue;
    
    // Caption
    MessageBoxAdv.MetroColorTable.CaptionBarColor = corporateBlue;
    MessageBoxAdv.MetroColorTable.CaptionForeColor = Color.White;
    
    // Buttons
    MessageBoxAdv.MetroColorTable.YesButtonBackColor = corporateBlue;
    MessageBoxAdv.MetroColorTable.YesButtonForeColor = Color.White;
    MessageBoxAdv.MetroColorTable.NoButtonBackColor = Color.FromArgb(82, 82, 82);
    MessageBoxAdv.MetroColorTable.NoButtonForeColor = Color.White;
    MessageBoxAdv.MetroColorTable.OKButtonBackColor = corporateBlue;
    MessageBoxAdv.MetroColorTable.OKButtonForeColor = Color.White;
    MessageBoxAdv.MetroColorTable.CancelButtonBackColor = Color.FromArgb(82, 82, 82);
    MessageBoxAdv.MetroColorTable.CancelButtonForeColor = Color.White;
    
    // Close button
    MessageBoxAdv.MetroColorTable.CloseButtonColor = Color.FromArgb(82, 82, 82);
    MessageBoxAdv.MetroColorTable.CloseButtonHoverColor = Color.FromArgb(232, 17, 35);
}

// Usage
ApplyCorporateBranding();
MessageBoxAdv.Show(this, 
    "Welcome to Acme Corporation Portal!", 
    "Acme Portal", 
    MessageBoxButtons.OK, 
    MessageBoxIcon.Information);
```

---

### Example 2: Dark Mode Theme

**C#:**
```csharp
using Syncfusion.Windows.Forms;
using System.Drawing;

public void ApplyDarkModeTheme()
{
    MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro;
    
    // Dark mode colors
    Color darkBackground = Color.FromArgb(30, 30, 30);
    Color mediumGray = Color.FromArgb(45, 45, 48);
    Color lightGray = Color.FromArgb(200, 200, 200);
    Color accentBlue = Color.FromArgb(0, 120, 215);
    
    // Dialog
    MessageBoxAdv.MetroColorTable.BackColor = darkBackground;
    MessageBoxAdv.MetroColorTable.ForeColor = lightGray;
    MessageBoxAdv.MetroColorTable.BorderColor = mediumGray;
    
    // Caption
    MessageBoxAdv.MetroColorTable.CaptionBarColor = mediumGray;
    MessageBoxAdv.MetroColorTable.CaptionForeColor = Color.White;
    
    // Buttons - all dark with accent for primary
    MessageBoxAdv.MetroColorTable.YesButtonBackColor = accentBlue;
    MessageBoxAdv.MetroColorTable.YesButtonForeColor = Color.White;
    MessageBoxAdv.MetroColorTable.NoButtonBackColor = mediumGray;
    MessageBoxAdv.MetroColorTable.NoButtonForeColor = lightGray;
    MessageBoxAdv.MetroColorTable.OKButtonBackColor = accentBlue;
    MessageBoxAdv.MetroColorTable.OKButtonForeColor = Color.White;
    MessageBoxAdv.MetroColorTable.CancelButtonBackColor = mediumGray;
    MessageBoxAdv.MetroColorTable.CancelButtonForeColor = lightGray;
    MessageBoxAdv.MetroColorTable.RetryButtonBackColor = accentBlue;
    MessageBoxAdv.MetroColorTable.RetryButtonForeColor = Color.White;
    
    // Close button
    MessageBoxAdv.MetroColorTable.CloseButtonColor = mediumGray;
    MessageBoxAdv.MetroColorTable.CloseButtonHoverColor = Color.FromArgb(232, 17, 35);
}

// Usage
ApplyDarkModeTheme();
MessageBoxAdv.Show(this, 
    "Dark mode reduces eye strain in low-light environments.", 
    "Dark Mode", 
    MessageBoxButtons.OK, 
    MessageBoxIcon.Information);
```

---

### Example 3: Success/Error Color-Coded Messages

**C#:**
```csharp
using Syncfusion.Windows.Forms;
using System.Drawing;

public void ShowSuccessMessage(string message)
{
    MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro;
    
    // Green success theme
    MessageBoxAdv.MetroColorTable.CaptionBarColor = Color.FromArgb(34, 139, 34);
    MessageBoxAdv.MetroColorTable.CaptionForeColor = Color.White;
    MessageBoxAdv.MetroColorTable.BackColor = Color.FromArgb(240, 255, 240);
    MessageBoxAdv.MetroColorTable.ForeColor = Color.FromArgb(0, 100, 0);
    MessageBoxAdv.MetroColorTable.BorderColor = Color.FromArgb(34, 139, 34);
    MessageBoxAdv.MetroColorTable.OKButtonBackColor = Color.FromArgb(34, 139, 34);
    MessageBoxAdv.MetroColorTable.OKButtonForeColor = Color.White;
    
    MessageBoxAdv.Show(this, message, "Success", 
        MessageBoxButtons.OK, MessageBoxIcon.Information);
}

public void ShowErrorMessage(string message)
{
    MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro;
    
    // Red error theme
    MessageBoxAdv.MetroColorTable.CaptionBarColor = Color.FromArgb(178, 34, 34);
    MessageBoxAdv.MetroColorTable.CaptionForeColor = Color.White;
    MessageBoxAdv.MetroColorTable.BackColor = Color.FromArgb(255, 240, 240);
    MessageBoxAdv.MetroColorTable.ForeColor = Color.FromArgb(139, 0, 0);
    MessageBoxAdv.MetroColorTable.BorderColor = Color.FromArgb(178, 34, 34);
    MessageBoxAdv.MetroColorTable.RetryButtonBackColor = Color.FromArgb(255, 140, 0);
    MessageBoxAdv.MetroColorTable.RetryButtonForeColor = Color.White;
    MessageBoxAdv.MetroColorTable.CancelButtonBackColor = Color.FromArgb(82, 82, 82);
    MessageBoxAdv.MetroColorTable.CancelButtonForeColor = Color.White;
    
    MessageBoxAdv.Show(this, message, "Error", 
        MessageBoxButtons.RetryCancel, MessageBoxIcon.Error);
}

public void ShowWarningMessage(string message)
{
    MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro;
    
    // Orange warning theme
    MessageBoxAdv.MetroColorTable.CaptionBarColor = Color.FromArgb(255, 140, 0);
    MessageBoxAdv.MetroColorTable.CaptionForeColor = Color.White;
    MessageBoxAdv.MetroColorTable.BackColor = Color.FromArgb(255, 250, 240);
    MessageBoxAdv.MetroColorTable.ForeColor = Color.FromArgb(139, 69, 0);
    MessageBoxAdv.MetroColorTable.BorderColor = Color.FromArgb(255, 140, 0);
    MessageBoxAdv.MetroColorTable.YesButtonBackColor = Color.FromArgb(255, 140, 0);
    MessageBoxAdv.MetroColorTable.YesButtonForeColor = Color.White;
    MessageBoxAdv.MetroColorTable.NoButtonBackColor = Color.FromArgb(82, 82, 82);
    MessageBoxAdv.MetroColorTable.NoButtonForeColor = Color.White;
    
    MessageBoxAdv.Show(this, message, "Warning", 
        MessageBoxButtons.YesNo, MessageBoxIcon.Warning);
}

// Usage
ShowSuccessMessage("File saved successfully!");
ShowErrorMessage("Failed to connect to database.");
ShowWarningMessage("This action cannot be undone. Continue?");
```

---

### Example 4: Application-Wide Metro Configuration Class

**C#:**
```csharp
using Syncfusion.Windows.Forms;
using System.Drawing;

public static class AppMessageBox
{
    static AppMessageBox()
    {
        // Initialize Metro theme with default corporate colors
        ApplyDefaultTheme();
    }

    public static void ApplyDefaultTheme()
    {
        MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Metro;
        
        // Corporate theme
        Color primaryColor = Color.FromArgb(0, 120, 215);
        Color secondaryColor = Color.FromArgb(82, 82, 82);
        
        MessageBoxAdv.MetroColorTable.CaptionBarColor = primaryColor;
        MessageBoxAdv.MetroColorTable.CaptionForeColor = Color.White;
        MessageBoxAdv.MetroColorTable.BackColor = Color.White;
        MessageBoxAdv.MetroColorTable.ForeColor = Color.Black;
        MessageBoxAdv.MetroColorTable.BorderColor = primaryColor;
        
        // Primary action buttons
        MessageBoxAdv.MetroColorTable.YesButtonBackColor = primaryColor;
        MessageBoxAdv.MetroColorTable.YesButtonForeColor = Color.White;
        MessageBoxAdv.MetroColorTable.OKButtonBackColor = primaryColor;
        MessageBoxAdv.MetroColorTable.OKButtonForeColor = Color.White;
        MessageBoxAdv.MetroColorTable.RetryButtonBackColor = primaryColor;
        MessageBoxAdv.MetroColorTable.RetryButtonForeColor = Color.White;
        
        // Secondary action buttons
        MessageBoxAdv.MetroColorTable.NoButtonBackColor = secondaryColor;
        MessageBoxAdv.MetroColorTable.NoButtonForeColor = Color.White;
        MessageBoxAdv.MetroColorTable.CancelButtonBackColor = secondaryColor;
        MessageBoxAdv.MetroColorTable.CancelButtonForeColor = Color.White;
        MessageBoxAdv.MetroColorTable.IgnoreButtonBackColor = secondaryColor;
        MessageBoxAdv.MetroColorTable.IgnoreButtonForeColor = Color.White;
        
        // Abort button (destructive action)
        MessageBoxAdv.MetroColorTable.AbortButtonBackColor = Color.FromArgb(232, 17, 35);
        MessageBoxAdv.MetroColorTable.AbortButtonForeColor = Color.White;
        
        // Close button
        MessageBoxAdv.MetroColorTable.CloseButtonColor = secondaryColor;
        MessageBoxAdv.MetroColorTable.CloseButtonHoverColor = Color.FromArgb(232, 17, 35);
    }

    public static DialogResult Show(IWin32Window owner, string text, string caption,
        MessageBoxButtons buttons, MessageBoxIcon icon)
    {
        return MessageBoxAdv.Show(owner, text, caption, buttons, icon);
    }

    public static void ShowInformation(IWin32Window owner, string message, string title = "Information")
    {
        MessageBoxAdv.Show(owner, message, title, MessageBoxButtons.OK, MessageBoxIcon.Information);
    }

    public static DialogResult ShowConfirmation(IWin32Window owner, string message, string title = "Confirm")
    {
        return MessageBoxAdv.Show(owner, message, title, MessageBoxButtons.YesNo, MessageBoxIcon.Question);
    }

    public static void ShowError(IWin32Window owner, string message, string title = "Error")
    {
        MessageBoxAdv.Show(owner, message, title, MessageBoxButtons.OK, MessageBoxIcon.Error);
    }
}

// Usage throughout application
AppMessageBox.ShowInformation(this, "Operation completed successfully!");
DialogResult result = AppMessageBox.ShowConfirmation(this, "Save changes?");
AppMessageBox.ShowError(this, "Failed to connect to server.");
```

---

## Best Practices

### Color Selection Guidelines

**Readability:**
- Ensure sufficient contrast between background and foreground colors
- Use WCAG AA standards (4.5:1 contrast ratio for normal text)
- Test colors with accessibility tools

**Button Colors:**
- **Primary actions** (Yes, OK, Retry): Use brand/accent color
- **Secondary actions** (No, Cancel): Use neutral gray
- **Destructive actions** (Abort, Delete): Use red/orange warning colors

**Visual Hierarchy:**
- Make primary action buttons stand out
- Use color to guide user attention
- Maintain consistency across all message boxes

### Performance

- Set Metro colors once during application startup
- Avoid changing colors dynamically for each message box
- Consider creating theme presets for different message types

### Maintenance

- Centralize color definitions in constants or configuration
- Use helper classes (like `AppMessageBox` example) for consistency
- Document your color scheme for team members

---

## Color Scheme Examples

### Windows 10 Theme
```csharp
// Accent Blue
Color.FromArgb(0, 120, 215)
// Background White
Color.White
// Foreground Black
Color.Black
```

### Dark Mode
```csharp
// Background
Color.FromArgb(30, 30, 30)
// Foreground
Color.FromArgb(200, 200, 200)
// Accent
Color.FromArgb(0, 120, 215)
```

### Material Design
```csharp
// Primary
Color.FromArgb(33, 150, 243)
// Background
Color.White
// Text
Color.FromArgb(33, 33, 33)
```

---

## Next Steps

- **Localization:** Implement multilanguage support → [localization.md](localization.md)
- **Visual Styles:** Explore other themes → [visual-styles.md](visual-styles.md)
- **Button Parameters:** Configure buttons and features → [button-parameters.md](button-parameters.md)
