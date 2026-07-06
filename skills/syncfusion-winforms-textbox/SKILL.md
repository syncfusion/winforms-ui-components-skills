---
name: syncfusion-winforms-textbox
description: Implement and configure Syncfusion TextBoxExt control in Windows Forms - an enhanced textbox with custom borders, themes, and advanced text features. Use when you need styled textboxes with Office themes, custom border colors, multiline text with overflow indicators, character casing, text alignment, or RTL support. Covers theme integration, border customization, multiline configuration, and replacing standard TextBox controls with styled alternatives.
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
---

# Implementing TextBoxes (TextBoxExt)

An enhanced textbox control for Windows Forms that provides custom border styling, Office themes, multiline text support, character casing, overflow indicators, and complete appearance customization.

## When to Use This Skill

Use this skill when you need to:
- **Enhanced TextBox**: Replace standard TextBox with styled, theme-enabled alternatives
- **Custom Borders**: Apply custom border colors, 3D styles, and border side configurations
- **Office Themes**: Match textbox appearance to Office 2016/2019 or Metro application themes
- **Multiline Text**: Display multiline text with word wrap, scrollbars, and overflow indicators
- **Text Formatting**: Control character casing (uppercase/lowercase), text alignment, and RTL layouts
- **Readonly Displays**: Create styled readonly text fields for labels or non-editable displays
- **Overflow Tooltips**: Show tooltips when text exceeds textbox boundaries
- **Size Constraints**: Enforce minimum or maximum textbox dimensions

## Component Overview

The **TextBoxExt** is an advanced textbox control derived from the standard TextBox that provides:
- Custom border colors and 3D border styles
- Eight built-in visual themes (Office2016, Office2019, Metro, Office2007, Office2010, Default)
- Theme assembly integration with SkinManager
- Multiline text with word wrap and configurable scrollbars
- Character casing transformation (Upper, Lower, Normal)
- Text alignment (Left, Center, Right)
- Right-to-left layout support for international applications
- Overflow indicators with customizable tooltips
- Size constraints (MaximumSize/MinimumSize)
- ReadOnly mode with active drawing when disabled
- Twelve property-changed events for customization

**Key Capabilities:**
- `BorderColor` and `BorderStyle` for custom borders
- `Style` property for selecting visual themes
- `ThemeName` property for runtime theme switching
- `CharacterCasing` for automatic case transformation
- `ShowOverflowIndicator` for text overflow detection
- `MaxLength` for input length restrictions
- `DrawActiveWhenDisabled` for active appearance when disabled

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

**When to read:** Starting new implementation, setting up dependencies, first-time usage, basic textbox creation

**Topics covered:**
- Assembly deployment (Syncfusion.Shared.Base.dll)
- NuGet package installation (Syncfusion.Shared.WinForms)
- Namespace requirements (Syncfusion.Windows.Forms.Tools)
- Adding TextBoxExt via designer (drag-and-drop)
- Adding TextBoxExt programmatically
- Basic TextBoxExt instantiation and configuration
- Size configuration (MaximumSize/MinimumSize)
- Multiline text setup with word wrap and scrollbars
- Initial text display

---

### Text Configuration
📄 **Read:** [references/text-configuration.md](references/text-configuration.md)

**When to read:** Configuring text properties, multiline text, character casing, alignment, overflow indicators, RTL support

**Topics covered:**
- Text and SelectedText properties
- CharacterCasing (Upper, Lower, Normal case transformation)
- TextAlign (Left, Center, Right alignment)
- RightToLeft layout for Arabic/Hebrew applications
- DrawActiveWhenDisabled (active appearance when disabled)
- Multiline text configuration
- WordWrap property for line wrapping
- ScrollBars (Horizontal, Vertical, Both, None)
- Overflow indicators (ShowOverflowIndicator property)
- Overflow tooltip customization (OverflowIndicatorToolTipText)
- Text manipulation methods (AppendText, Cut, Copy, Paste, Select, SelectAll)
- ScrollToCaret() method for caret positioning

---

### Border Settings
📄 **Read:** [references/border-settings.md](references/border-settings.md)

**When to read:** Customizing border appearance, applying border colors, configuring 3D borders, selecting border sides

**Topics covered:**
- BorderStyle property (Fixed3D, FixedSingle, None)
- Border3DStyle property (Raised, Sunken, Etched, etc.)
- BorderColor customization with Color values
- BorderSides configuration (All, Top, Bottom, Left, Right, combinations)
- Complete border examples with code
- Border customization patterns
- Matching borders to application themes

---

### Appearance and Styling
📄 **Read:** [references/appearance-styling.md](references/appearance-styling.md)

**When to read:** Applying visual themes, customizing colors, selecting Office styles

**Topics covered:**
- BackColor property for background customization
- ForeColor property for text color
- Style property overview
- Eight built-in visual styles:
  - Office2016Colorful
  - Office2016White
  - Office2016Black
  - Office2016DarkGray
  - Office2019Colorful
  - Metro (flat modern design)
  - Office2007 (classic Ribbon style)
  - Office2010 (refined Office look)
  - Default (standard appearance)
- Visual style selection and configuration
- Style comparison examples
- Matching textbox style to application design

---

### Themes Configuration
📄 **Read:** [references/themes-configuration.md](references/themes-configuration.md)

**When to read:** Loading theme assemblies, configuring application-wide themes, runtime theme switching, advanced theming

**Topics covered:**
- ThemesEnabled property (XP themes for Fixed3D borders)
- ThemeName property for runtime theme selection
- Theme assembly loading requirements
  - Syncfusion.Office2016Theme.WinForms (4 variations)
  - Syncfusion.Office2019Theme.WinForms
  - Syncfusion.HighContrastTheme.WinForms
- SkinManager.LoadAssembly() method
- Loading themes in Program.Main()
- Office2016 theme variations (Colorful, White, Black, DarkGray)
- Office2019Colorful theme
- HighContrastBlack accessibility theme
- Application-wide theme setup
- Theme switching at runtime
- Theme consistency across controls

---

### Behavior and Events
📄 **Read:** [references/behavior-events.md](references/behavior-events.md)

**When to read:** Configuring behavior properties, handling events, enforcing input restrictions, responding to property changes

**Topics covered:**
- MaxLength property (input length restriction)
- ReadOnly mode (prevent text editing)
- Layout settings (MaximumSize, MinimumSize)
- Twelve property-changed events:
  - Border3DStyleChanged
  - BorderColorChanged
  - BorderSidesChanged
  - BorderStyleChanged
  - CharacterCasingChanged
  - HideSelectionChanged
  - MaximumSizeChanged
  - MinimumSizeChanged
  - MultilineChanged
  - ReadOnlyChanged
  - TextAlignChanged
  - ThemesEnabledChanged
- Event handler implementation patterns
- Event-driven customization
- Validation and input control scenarios
- Dynamic property updates

---

## Quick Start Example

### Basic Themed TextBox with Custom Border

**C#:**
```csharp
using Syncfusion.Windows.Forms.Tools;
using System.Drawing;

// Create TextBoxExt instance
TextBoxExt textBoxExt1 = new TextBoxExt();
textBoxExt1.Location = new Point(50, 50);
textBoxExt1.Size = new Size(300, 25);
textBoxExt1.Text = "Enter your text here";

// Apply Office2016 Colorful theme
textBoxExt1.Style = TextBoxExt.theme.Office2016Colorful;

// Customize border
textBoxExt1.BorderColor = Color.FromArgb(0, 120, 215); // Blue border
textBoxExt1.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle;

// Add to form
this.Controls.Add(textBoxExt1);
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms.Tools
Imports System.Drawing

' Create TextBoxExt instance
Dim textBoxExt1 As New TextBoxExt()
textBoxExt1.Location = New Point(50, 50)
textBoxExt1.Size = New Size(300, 25)
textBoxExt1.Text = "Enter your text here"

' Apply Office2016 Colorful theme
textBoxExt1.Style = TextBoxExt.theme.Office2016Colorful

' Customize border
textBoxExt1.BorderColor = Color.FromArgb(0, 120, 215) ' Blue border
textBoxExt1.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle

' Add to form
Me.Controls.Add(textBoxExt1)
```

---

## Common Patterns

### Pattern 1: Multiline Text Editor with Overflow Indicator

**Scenario:** Create a multiline textbox for notes or comments with scrollbars and overflow tooltips

**C#:**
```csharp
using Syncfusion.Windows.Forms.Tools;

TextBoxExt notesTextBox = new TextBoxExt();
notesTextBox.Location = new System.Drawing.Point(20, 20);
notesTextBox.Size = new System.Drawing.Size(400, 200);

// Enable multiline with word wrap
notesTextBox.Multiline = true;
notesTextBox.WordWrap = true;
notesTextBox.ScrollBars = System.Windows.Forms.ScrollBars.Vertical;

// Configure overflow indicator
notesTextBox.ShowOverflowIndicator = true;
notesTextBox.ShowOverflowIndicatorToolTip = true;
notesTextBox.OverflowIndicatorToolTipText = "Text content exceeds visible area. Scroll to view more.";

// Apply Metro theme
notesTextBox.Style = TextBoxExt.theme.Metro;

// Add to form
this.Controls.Add(notesTextBox);
```

---

### Pattern 2: Readonly Display Field with Active Appearance

**Scenario:** Display readonly information (e.g., calculated values, status) that looks active even when disabled

**C#:**
```csharp
using Syncfusion.Windows.Forms.Tools;
using System.Drawing;

TextBoxExt displayField = new TextBoxExt();
displayField.Location = new System.Drawing.Point(150, 100);
displayField.Size = new System.Drawing.Size(250, 25);
displayField.Text = "Total: $1,234.56";

// Make readonly but keep active appearance
displayField.ReadOnly = true;
displayField.DrawActiveWhenDisabled = true;

// Apply custom styling
displayField.BackColor = Color.WhiteSmoke;
displayField.ForeColor = Color.DarkGreen;
displayField.BorderColor = Color.LightGray;
displayField.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle;

// Align text to right (common for currency)
displayField.TextAlign = System.Windows.Forms.HorizontalAlignment.Right;

// Add to form
this.Controls.Add(displayField);
```

---

### Pattern 3: Uppercase Input Field with Length Restriction

**Scenario:** Create an input field that automatically converts to uppercase (e.g., product codes, license keys)

**C#:**
```csharp
using Syncfusion.Windows.Forms.Tools;
using System.Drawing;

TextBoxExt productCodeBox = new TextBoxExt();
productCodeBox.Location = new System.Drawing.Point(120, 80);
productCodeBox.Size = new System.Drawing.Size(200, 25);

// Enforce uppercase and length
productCodeBox.CharacterCasing = System.Windows.Forms.CharacterCasing.Upper;
productCodeBox.MaxLength = 10;

// Apply Office2019 theme
productCodeBox.Style = TextBoxExt.theme.Office2019Colorful;

// Customize border
productCodeBox.BorderColor = Color.SteelBlue;
productCodeBox.BorderStyle = System.Windows.Forms.BorderStyle.FixedSingle;

// Center align for codes
productCodeBox.TextAlign = System.Windows.Forms.HorizontalAlignment.Center;

// Add placeholder behavior
productCodeBox.ForeColor = Color.Gray;
productCodeBox.Text = "ENTER-CODE";

productCodeBox.Enter += (s, e) => {
    if (productCodeBox.Text == "ENTER-CODE")
    {
        productCodeBox.Text = "";
        productCodeBox.ForeColor = Color.Black;
    }
};

productCodeBox.Leave += (s, e) => {
    if (string.IsNullOrWhiteSpace(productCodeBox.Text))
    {
        productCodeBox.Text = "ENTER-CODE";
        productCodeBox.ForeColor = Color.Gray;
    }
};

// Add to form
this.Controls.Add(productCodeBox);
```

---

## Key Properties

### Appearance Properties

| Property | Type | Description |
|----------|------|-------------|
| `Style` | `TextBoxExt.theme` | Visual theme: Office2016Colorful, Office2016White, Office2016Black, Office2016DarkGray, Office2019Colorful, Metro, Office2007, Office2010, Default |
| `ThemeName` | `string` | Runtime theme name (requires theme assembly loaded) |
| `BackColor` | `Color` | Background color of textbox |
| `ForeColor` | `Color` | Text color |
| `BorderStyle` | `BorderStyle` | Border style: Fixed3D, FixedSingle, None |
| `BorderColor` | `Color` | Custom border color |
| `Border3DStyle` | `Border3DStyle` | 3D border effect: Raised, Sunken, Etched, etc. |
| `BorderSides` | `Border3DSide` | Border sides: All, Top, Bottom, Left, Right |

### Text Properties

| Property | Type | Description |
|----------|------|-------------|
| `Text` | `string` | Textbox content |
| `SelectedText` | `string` | Currently selected text |
| `CharacterCasing` | `CharacterCasing` | Case transformation: Normal, Upper, Lower |
| `TextAlign` | `HorizontalAlignment` | Text alignment: Left, Center, Right |
| `RightToLeft` | `RightToLeft` | RTL layout: Yes, No |
| `DrawActiveWhenDisabled` | `bool` | Draw text as active even when control is disabled |

### Multiline Properties

| Property | Type | Description |
|----------|------|-------------|
| `Multiline` | `bool` | Enable multiline text display |
| `WordWrap` | `bool` | Wrap text to next line automatically |
| `ScrollBars` | `ScrollBars` | Scrollbar visibility: None, Horizontal, Vertical, Both |

### Overflow Indicator Properties

| Property | Type | Description |
|----------|------|-------------|
| `ShowOverflowIndicator` | `bool` | Show indicator when text overflows |
| `ShowOverflowIndicatorToolTip` | `bool` | Show tooltip on overflow indicator |
| `OverflowIndicatorToolTipText` | `string` | Custom tooltip text for overflow |

### Behavior Properties

| Property | Type | Description |
|----------|------|-------------|
| `MaxLength` | `int` | Maximum character count (default: 32767) |
| `ReadOnly` | `bool` | Prevent text editing |
| `MaximumSize` | `Size` | Maximum control dimensions |
| `MinimumSize` | `Size` | Minimum control dimensions |

### Theme Properties

| Property | Type | Description |
|----------|------|-------------|
| `ThemesEnabled` | `bool` | Enable XP themes for Fixed3D borders |

---

## Common Use Cases

### 1. Product Search Box with Themed Appearance
Styled textbox for searching products in inventory system:
```csharp
TextBoxExt searchBox = new TextBoxExt();
searchBox.Style = TextBoxExt.theme.Office2016Colorful;
searchBox.BorderColor = Color.DodgerBlue;
searchBox.Text = "Search products...";
```

### 2. Multiline Description Field
Large textbox for product descriptions or notes:
```csharp
TextBoxExt descriptionBox = new TextBoxExt();
descriptionBox.Multiline = true;
descriptionBox.WordWrap = true;
descriptionBox.ScrollBars = ScrollBars.Vertical;
descriptionBox.Size = new Size(500, 150);
```

### 3. Uppercase License Key Entry
Automatic uppercase conversion for license keys:
```csharp
TextBoxExt licenseBox = new TextBoxExt();
licenseBox.CharacterCasing = CharacterCasing.Upper;
licenseBox.MaxLength = 25;
licenseBox.TextAlign = HorizontalAlignment.Center;
```

### 4. Readonly Status Display
Display-only field that shows active appearance:
```csharp
TextBoxExt statusBox = new TextBoxExt();
statusBox.ReadOnly = true;
statusBox.DrawActiveWhenDisabled = true;
statusBox.BackColor = Color.LightYellow;
statusBox.Text = "Status: Active";
```

### 5. RTL Text Input for Arabic
Right-to-left textbox for international applications:
```csharp
TextBoxExt arabicTextBox = new TextBoxExt();
arabicTextBox.RightToLeft = RightToLeft.Yes;
arabicTextBox.TextAlign = HorizontalAlignment.Right;
```

### 6. Custom Bordered Input Field
Textbox with custom border color matching brand:
```csharp
TextBoxExt brandedBox = new TextBoxExt();
brandedBox.BorderStyle = BorderStyle.FixedSingle;
brandedBox.BorderColor = Color.FromArgb(255, 87, 34); // Orange
brandedBox.Style = TextBoxExt.theme.Metro;
```

### 7. Size-Constrained Textbox
Textbox with enforced dimensions:
```csharp
TextBoxExt constrainedBox = new TextBoxExt();
constrainedBox.MinimumSize = new Size(200, 25);
constrainedBox.MaximumSize = new Size(400, 25);
```

---

## Additional Resources

**Assembly:** Syncfusion.Shared.Base.dll  
**Namespace:** Syncfusion.Windows.Forms.Tools  
**NuGet Package:** Syncfusion.Shared.WinForms  
**Minimum .NET Framework:** 4.5  

**Key Classes:**
- `TextBoxExt` - Main enhanced textbox control class
- `SkinManager` - Theme assembly loading manager

**Related Skills:**
- Form theming and styling
- Input validation
- Multiline text editors
- Readonly field displays
