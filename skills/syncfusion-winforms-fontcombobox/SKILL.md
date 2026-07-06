---
name: syncfusion-winforms-fontcombobox
description: Implement and configure Syncfusion WinForms FontComboBox control - a specialized combo box automatically populated with system fonts. Use this when working with font selection UI, font pickers, or font dropdown controls in Windows Forms applications. The skill covers font selection capability, system font display, and font enumeration in WinForms.
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
---

# Implementing FontComboBox

A specialized combo box control for Windows Forms that automatically populates with system fonts, providing an intuitive font selection interface with preview capabilities.

## When to Use This Skill

Use this skill when you need to:
- **Font Selection UI**: Implement font picker controls in Windows Forms applications
- **System Font Display**: Show all installed fonts with visual previews
- **Typography Tools**: Build text editors, design tools, or formatting interfaces
- **Font Enumeration**: Automatically populate dropdowns with available system fonts
- **Rich Text Applications**: Create word processors, document editors, or formatting panels
- **Design Applications**: Build UI customization tools requiring font selection

## Component Overview

The **FontComboBox** is a specialized combo box control derived from standard ComboBox that:
- Automatically populates with all system-installed fonts
- Displays font names in their respective typefaces
- Supports AutoComplete for quick font search
- Provides extensive dropdown customization options
- Includes built-in visual styles and theming
- Handles font selection events with type-safe access

**Key Capabilities:**
- Automatic system font loading
- Font preview in dropdown items
- AutoComplete and filtering
- Multiple visual themes (Office2016, Metro, Office2007)
- RTL support
- Symbol font rendering
- Custom event handling

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

**When to read:** Starting new implementation, setting up dependencies, first-time usage

**Topics covered:**
- Assembly deployment and NuGet packages
- Control dependencies for FontComboBox
- Adding via Visual Studio designer (toolbox drag-drop)
- Adding via code (manual instantiation)
- Basic initialization and configuration
- RTL (Right-to-Left) layout support
- Sample GitHub projects

---

### AutoComplete Features
📄 **Read:** [references/autocomplete.md](references/autocomplete.md)

**When to read:** Enabling font search, implementing type-ahead, configuring auto-completion behavior

**Topics covered:**
- UseAutoComplete property for enabling/disabling
- AutoCompleteSource options (ListItems, CustomSource)
- AutoCompleteMode settings (Suggest, Append, SuggestAppend)
- AutoCompleteCustomSource for custom font lists
- Best practices for AutoComplete configuration
- Code examples with filtering behavior

---

### DropDown Configuration
📄 **Read:** [references/dropdown-configuration.md](references/dropdown-configuration.md)

**When to read:** Customizing dropdown appearance, controlling popup behavior, configuring item display

**Topics covered:**
- DropDownStyle options (DropDown, DropDownList, Simple)
- DropDownHeight and DropDownWidth sizing
- MaxDropDownItems for visible item count
- ShowSymbolFontPreview for symbol fonts
- ItemHeight for item spacing
- Sorted property for alphabetical ordering
- Dropdown behavior customization

---

### Selection and Events
📄 **Read:** [references/selection-and-events.md](references/selection-and-events.md)

**When to read:** Handling font selection changes, responding to user input, applying selected fonts

**Topics covered:**
- Programmatic selection with SelectedItem and SelectedIndex
- SelectedIndexChanged event handling
- Custom FontSelected event implementation
- Applying selected fonts to other controls (labels, textboxes)
- Event-driven font updates
- Code patterns for selection management

---

### Visual Styles and Theming
📄 **Read:** [references/visual-styles.md](references/visual-styles.md)

**When to read:** Applying themes, customizing control appearance, matching application design

**Topics covered:**
- VisualStyle property (Office2016Colorful, Office2016White, Office2016Black, Office2016DarkGray, Metro, Office2010, Office2007, Default)
- Office2007ColorScheme options (Blue, Silver, Black)
- Custom color themes with Office2007ColorTheme.Managed
- ApplyManagedColors for custom color application
- Theme screenshots and visual examples

---

## Quick Start

### Basic FontComboBox Setup

```csharp
using Syncfusion.Windows.Forms.Tools;

// Create and configure FontComboBox
FontComboBox fontComboBox = new FontComboBox();
fontComboBox.Size = new Size(200, 25);
fontComboBox.Location = new Point(20, 20);

// Enable AutoComplete for font search
fontComboBox.UseAutoComplete = true;

// Set default selected font
fontComboBox.Text = "Arial";

// Add to form
this.Controls.Add(fontComboBox);
```

### Handle Font Selection

```csharp
// Subscribe to selection changed event
fontComboBox.SelectedIndexChanged += FontComboBox_SelectedIndexChanged;

private void FontComboBox_SelectedIndexChanged(object sender, EventArgs e)
{
    if (fontComboBox.Text != null)
    {
        // Apply selected font to a label
        label1.Font = new Font(fontComboBox.Text.ToString(), 12, FontStyle.Regular);
    }
}
```

## Common Patterns

### Pattern 1: Font Picker with Preview
```csharp
// Configure FontComboBox with preview
FontComboBox fontComboBox = new FontComboBox
{
    UseAutoComplete = true,
    DropDownStyle = ComboBoxStyle.DropDownList,
    ShowSymbolFontPreview = true,
    DropDownHeight = 200,
    Sorted = true
};

// Handle selection to update preview label
fontComboBox.SelectedIndexChanged += (s, e) =>
{
    if (fontComboBox.Text != null)
    {
        previewLabel.Font = new Font(fontComboBox.Text.ToString(), 14);
        previewLabel.Text = "The quick brown fox jumps over the lazy dog";
    }
};
```

### Pattern 2: Themed Font Selector
```csharp
// Apply Office2016 theme
FontComboBox fontComboBox = new FontComboBox
{
    VisualStyle = ThemedComboBoxStyles.Office2016Colorful,
    UseAutoComplete = true,
    DropDownStyle = ComboBoxStyle.DropDown
};

// Set initial font
fontComboBox.Text = "Segoe UI";
```

### Pattern 3: Custom Font List with AutoComplete
```csharp
// Create FontComboBox with custom font list
FontComboBox fontComboBox = new FontComboBox();

// Configure custom AutoComplete source
fontComboBox.AutoCompleteCustomSource.AddRange(new string[] 
{
    "Arial", "Calibri", "Cambria", "Georgia", "Times New Roman", "Verdana"
});
fontComboBox.AutoCompleteMode = AutoCompleteMode.SuggestAppend;
fontComboBox.AutoCompleteSource = AutoCompleteSource.CustomSource;
```

## Key Properties

### Selection Properties
- **SelectedItem**: Gets or sets the currently selected font name (string)
- **SelectedIndex**: Gets or sets the index of the selected font (int)

### AutoComplete Properties
- **UseAutoComplete**: Enables/disables AutoComplete feature (bool)
- **AutoCompleteMode**: Sets completion behavior (Suggest, Append, SuggestAppend)
- **AutoCompleteSource**: Specifies source for completion (ListItems, CustomSource)
- **AutoCompleteCustomSource**: Custom string collection for AutoComplete

### DropDown Properties
- **DropDownStyle**: Controls edit/select behavior (DropDown, DropDownList, Simple)
- **DropDownHeight**: Height of dropdown in pixels (int)
- **DropDownWidth**: Width of dropdown in pixels (int)
- **MaxDropDownItems**: Maximum visible items in dropdown (int)
- **ShowSymbolFontPreview**: Renders symbol fonts with their typeface (bool)

### Item Properties
- **ItemHeight**: Height of each dropdown item in pixels (int)
- **Sorted**: Enables alphabetical sorting of fonts (bool)

### Styling Properties
- **VisualStyle**: Sets control theme (Office2016Colorful, Metro, Office2007, etc.)
- **Office2007ColorScheme**: Color scheme for Office2007 style (Blue, Silver, Black)
- **Office2007ColorTheme**: Custom theme mode (Managed for custom colors)

### Layout Properties
- **RightToLeft**: Enables RTL layout (RightToLeft.Yes/No)

## Common Use Cases

### Rich Text Editor Font Selector
Build a font picker for text formatting toolbars in document editors, word processors, or email clients.

### Design Tool Typography Panel
Implement font selection in graphic design applications, presentation software, or UI builders.

### Application Settings Font Configuration
Create font preferences dialogs for applications with customizable text display.

### Custom Control Property Editors
Build property grids or configuration panels where users select fonts for custom controls.

### Font Comparison Tools
Develop utilities for comparing fonts side-by-side with live previews.

## Notes

- FontComboBox automatically loads all system-installed fonts when `UseAutoComplete = true` is set — no manual font enumeration required.
- Symbol fonts can be displayed with their actual glyphs using ShowSymbolFontPreview
- Use DropDownList style to prevent manual text entry, allowing only font selection
- SelectedIndexChanged fires whenever selection changes programmatically or by user
- Control inherits from standard ComboBox, so all base properties/events are available
