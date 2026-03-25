# Themes and Styling

## Table of Contents

1. [Overview](#overview)
2. [Visual Styles Overview](#visual-styles-overview)
3. [Office2007 Themes](#office2007-themes)
   - [Office2007Blue](#office2007blue)
   - [Office2007Silver](#office2007silver)
   - [Office2007Black](#office2007black)
4. [Office2016 Themes](#office2016-themes)
   - [Office2016Colorful](#office2016colorful)
   - [Office2016White](#office2016white)
   - [Office2016DarkGray](#office2016darkgray)
   - [Office2016Black](#office2016black)
5. [Metro Theme](#metro-theme)
6. [Other Available Themes](#other-available-themes)
7. [Applying Themes](#applying-themes)
8. [Theme Consistency](#theme-consistency)
9. [RTL Support (Right-to-Left)](#rtl-support-right-to-left)
10. [Localization](#localization)
11. [Keyboard Shortcuts](#keyboard-shortcuts)
12. [Trigger BarItems](#trigger-baritems)
13. [Complete Theme Example](#complete-theme-example)
14. [Complete RTL Example](#complete-rtl-example)
15. [Complete Keyboard Shortcuts Example](#complete-keyboard-shortcuts-example)
16. [Best Practices](#best-practices)

## Overview

The Syncfusion WinForms XPToolBar control offers professional visual themes through the `Style` property, enabling you to create modern, polished toolbars that match current design standards. Beyond themes, the control supports right-to-left (RTL) layouts for Arabic and Hebrew languages, comprehensive localization capabilities, keyboard shortcuts for accessibility, and programmatic item triggering. These features combine to create a versatile, globally-ready toolbar solution.

## Visual Styles Overview

The `Style` property controls the visual appearance of the XPToolBar control. Syncfusion provides multiple built-in themes that follow industry-standard design patterns.

### Available Visual Styles

- **Default**: Standard Windows look
- **Metro**: Modern flat design
- **Office2003**: Classic Office appearance
- **Office2007**: Ribbon-style Office look
- **Office2007Outlook**: Outlook-specific variant
- **Office2010**: Windows 7-era Office design
- **Office2016Black**: Dark Office theme
- **Office2016Colorful**: Vibrant Office theme (recommended default)
- **Office2016DarkGray**: Medium-dark Office theme
- **Office2016White**: Clean, bright Office theme
- **OfficeXP**: Windows XP-era Office look
- **VS2005**: Visual Studio 2005 style
- **VS2010**: Visual Studio 2010 style

### When to Use Each Theme

**Office2016 Themes** (Recommended for new applications):
- Modern, professional appearance
- Matches current Microsoft Office applications
- Best for business applications
- Excellent user familiarity

**Metro Theme**:
- Flat, minimalist design
- Modern Windows aesthetic
- Touch-friendly
- Best for Windows 8/10/11 style apps

**Office2007/2010 Themes**:
- Legacy compatibility
- Familiar to users of older Office versions

**VS2005/VS2010 Themes**:
- Developer tools and IDEs
- Technical applications

## Office2007 Themes

The Office2007 theme family provides three color variations matching Microsoft Office 2007's design language.

### Office2007Blue

The blue color scheme from Office 2007, featuring blue accents and gradients:

```csharp
this.xpToolBar1.Style = Syncfusion.Windows.Forms.VisualStyle.Office2007;
```

```vb
Me.xpToolBar1.Style = Syncfusion.Windows.Forms.VisualStyle.Office2007
```

**Characteristics:**
- Blue gradient backgrounds
- Professional appearance
- Warm, inviting feel
- Good for business applications

### Office2007Silver

A silver/gray color scheme with subtle gradients:

```csharp
// Note: Use Office2007 style - silver may be a variant
this.xpToolBar1.Style = Syncfusion.Windows.Forms.VisualStyle.Office2007;
```

**Characteristics:**
- Neutral gray tones
- Professional and subdued
- Works well in any context
- Minimizes visual distraction

### Office2007Black

A dark theme with black and dark gray tones:

```csharp
// Note: Use Office2007 style - black may be a variant
this.xpToolBar1.Style = Syncfusion.Windows.Forms.VisualStyle.Office2007;
```

**Characteristics:**
- Dark, sophisticated appearance
- Reduces eye strain in low light
- Modern aesthetic
- Good for creative applications

### Office2007 Code Example

```csharp
// Apply Office2007 theme
XPToolBar toolbar = new XPToolBar();
toolbar.Style = Syncfusion.Windows.Forms.VisualStyle.Office2007;
toolbar.Dock = DockStyle.Top;

// Add items
BarItem item1 = new BarItem("File");
BarItem item2 = new BarItem("Edit");
BarItem item3 = new BarItem("View");

toolbar.Items.AddRange(new BarItem[] { item1, item2, item3 });
this.Controls.Add(toolbar);
```

## Office2016 Themes

The Office2016 theme family represents the most modern design, with four distinct color variations.

### Office2016Colorful

The default vibrant theme with colorful accents, matching Office 2016's default appearance:

```csharp
this.xpToolBar1.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016Colorful;
```

```vb
Me.xpToolBar1.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016Colorful
```

**Characteristics:**
- Bright, modern appearance
- Colorful accent elements
- High contrast for readability
- Recommended for most applications
- Matches current Office suite

**When to Use:**
- Default choice for new applications
- Business and productivity apps
- When you want a modern, professional look

### Office2016White

A clean, bright theme with minimal color and lots of white space:

```csharp
this.xpToolBar1.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016White;
```

```vb
Me.xpToolBar1.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016White
```

**Characteristics:**
- Very bright, clean appearance
- Maximum contrast
- Minimal visual clutter
- Best for well-lit environments

**When to Use:**
- Content-focused applications
- When you need maximum brightness
- Medical or technical applications requiring clarity

### Office2016DarkGray

A medium-dark theme that balances readability with reduced glare:

```csharp
this.xpToolBar1.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016DarkGray;
```

```vb
Me.xpToolBar1.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016DarkGray
```

**Characteristics:**
- Medium-dark gray background
- Comfortable for extended use
- Reduces eye strain
- Professional appearance

**When to Use:**
- Applications used for long periods
- Users who prefer darker themes
- Video editing or creative applications

### Office2016Black

The darkest Office2016 theme, providing maximum contrast with light text on dark backgrounds:

```csharp
this.xpToolBar1.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016Black;
```

```vb
Me.xpToolBar1.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016Black
```

**Characteristics:**
- Very dark background
- Light text and icons
- Dramatic, modern look
- Significant eye strain reduction in low light

**When to Use:**
- Dark mode preferences
- Low-light environments
- Developer tools and IDEs
- Creative professional applications

### Office2016 Code Examples

```csharp
// Office2016Colorful (recommended default)
xpToolBar1.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016Colorful;

// Office2016White (bright theme)
xpToolBar2.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016White;

// Office2016DarkGray (medium-dark theme)
xpToolBar3.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016DarkGray;

// Office2016Black (dark theme)
xpToolBar4.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016Black;
```

```vb
' Office2016Colorful (recommended default)
xpToolBar1.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016Colorful

' Office2016White (bright theme)
xpToolBar2.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016White

' Office2016DarkGray (medium-dark theme)
xpToolBar3.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016DarkGray

' Office2016Black (dark theme)
xpToolBar4.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016Black
```

## Metro Theme

The Metro theme provides a modern, flat design aesthetic inspired by Windows 8 and later:

```csharp
this.xpToolBar1.Style = Syncfusion.Windows.Forms.VisualStyle.Metro;
```

```vb
Me.xpToolBar1.Style = Syncfusion.Windows.Forms.VisualStyle.Metro
```

**Characteristics:**
- Flat design with no gradients or shadows
- Clean, minimalist appearance
- Modern Windows aesthetic
- Touch-friendly with larger hit areas
- Emphasizes content over chrome

**When to Use:**
- Modern Windows applications
- Touch-enabled interfaces
- Minimalist design preferences
- Applications targeting Windows 8/10/11 style

### Metro Theme Example

```csharp
XPToolBar metroToolbar = new XPToolBar();
metroToolbar.Style = Syncfusion.Windows.Forms.VisualStyle.Metro;
metroToolbar.Dock = DockStyle.Top;

// Create modern-looking items
BarItem homeItem = new BarItem("HOME");
BarItem designItem = new BarItem("DESIGN");
BarItem layoutItem = new BarItem("LAYOUT");

metroToolbar.Items.AddRange(new BarItem[] { homeItem, designItem, layoutItem });
this.Controls.Add(metroToolbar);
```

## Other Available Themes

Additional themes for compatibility and specific use cases:

### Default Theme

```csharp
this.xpToolBar1.Style = Syncfusion.Windows.Forms.VisualStyle.Default;
```

Basic Windows appearance without special styling.

### Office2003 Theme

```csharp
this.xpToolBar1.Style = Syncfusion.Windows.Forms.VisualStyle.Office2003;
```

Classic Office 2003 appearance for legacy applications.

### Office2010 Theme

```csharp
this.xpToolBar1.Style = Syncfusion.Windows.Forms.VisualStyle.Office2010;
```

Windows 7-era Office design.

### OfficeXP Theme

```csharp
this.xpToolBar1.Style = Syncfusion.Windows.Forms.VisualStyle.OfficeXP;
```

Windows XP-era Office appearance.

### VS2005 and VS2010 Themes

```csharp
// Visual Studio 2005 style
this.xpToolBar1.Style = Syncfusion.Windows.Forms.VisualStyle.VS2005;

// Visual Studio 2010 style
this.xpToolBar1.Style = Syncfusion.Windows.Forms.VisualStyle.VS2010;
```

Designed for developer tools and technical applications.

## Applying Themes

Themes are applied using the `Style` property of the XPToolBar control.

### Setting Theme at Design Time

In the Visual Studio Properties window:

1. Select the XPToolBar control
2. Find the `Style` property
3. Choose from the dropdown list
4. The theme applies immediately in the designer

### Setting Theme at Runtime

You can change themes programmatically:

```csharp
// Set theme when form loads
private void Form1_Load(object sender, EventArgs e)
{
    xpToolBar1.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016Colorful;
}

// Change theme based on user preference
private void ApplyDarkTheme()
{
    xpToolBar1.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016Black;
}

private void ApplyLightTheme()
{
    xpToolBar1.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016White;
}
```

```vb
' Set theme when form loads
Private Sub Form1_Load(sender As Object, e As EventArgs)
    xpToolBar1.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016Colorful
End Sub

' Change theme based on user preference
Private Sub ApplyDarkTheme()
    xpToolBar1.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016Black
End Sub

Private Sub ApplyLightTheme()
    xpToolBar1.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016White
End Sub
```

### Dynamic Theme Switching

Example of allowing users to switch themes:

```csharp
private void themeComboBox_SelectedIndexChanged(object sender, EventArgs e)
{
    switch (themeComboBox.SelectedItem.ToString())
    {
        case "Office 2016 Colorful":
            xpToolBar1.Style = VisualStyle.Office2016Colorful;
            break;
        case "Office 2016 Black":
            xpToolBar1.Style = VisualStyle.Office2016Black;
            break;
        case "Metro":
            xpToolBar1.Style = VisualStyle.Metro;
            break;
        case "Default":
            xpToolBar1.Style = VisualStyle.Default;
            break;
    }
}
```

## Theme Consistency

Maintaining consistent themes across your application is essential for a professional appearance.

### Matching Application Theme

All toolbars in your application should use the same theme:

```csharp
// Apply same theme to multiple toolbars
private void ApplyThemeToAllToolbars(VisualStyle style)
{
    xpToolBar1.Style = style;
    xpToolBar2.Style = style;
    xpToolBar3.Style = style;
}

// Use in Form_Load
private void Form1_Load(object sender, EventArgs e)
{
    ApplyThemeToAllToolbars(VisualStyle.Office2016Colorful);
}
```

### Multiple Toolbars Same Theme

When using multiple toolbars, ensure consistency:

```csharp
public class ThemedForm : Form
{
    private XPToolBar mainToolbar;
    private XPToolBar formattingToolbar;
    private VisualStyle currentTheme = VisualStyle.Office2016Colorful;
    
    private void InitializeToolbars()
    {
        mainToolbar = new XPToolBar();
        mainToolbar.Style = currentTheme;
        mainToolbar.Dock = DockStyle.Top;
        
        formattingToolbar = new XPToolBar();
        formattingToolbar.Style = currentTheme;
        formattingToolbar.Dock = DockStyle.Top;
        
        this.Controls.Add(formattingToolbar);
        this.Controls.Add(mainToolbar);
    }
}
```

### Best Practices for Theme Consistency

1. **Store Theme Setting**: Save user's theme preference
2. **Apply Globally**: Use a centralized theme manager
3. **Update All Controls**: When changing theme, update all toolbars
4. **Match Other Components**: Ensure other Syncfusion controls use same theme
5. **Test All Themes**: Verify appearance with each theme option

```csharp
// Centralized theme management
public static class ThemeManager
{
    public static VisualStyle CurrentTheme { get; set; } 
        = VisualStyle.Office2016Colorful;
    
    public static void ApplyToToolbar(XPToolBar toolbar)
    {
        toolbar.Style = CurrentTheme;
    }
}

// Usage
ThemeManager.ApplyToToolbar(xpToolBar1);
ThemeManager.ApplyToToolbar(xpToolBar2);
```

## RTL Support (Right-to-Left)

RTL (Right-to-Left) support enables proper display and interaction for languages like Arabic and Hebrew that read from right to left.

### RightToLeft Property

Enable RTL layout using the `RightToLeft` property:

```csharp
this.xpToolBar1.RightToLeft = System.Windows.Forms.RightToLeft.Yes;
```

```vb
Me.xpToolBar1.RightToLeft = System.Windows.Forms.RightToLeft.Yes
```

### RTL Behavior

When RTL is enabled:
- Toolbar items are arranged from right to left
- First item appears on the right
- Last item appears on the left
- Chevron button appears on the left side
- Text alignment adjusts automatically
- Separators maintain proper spacing

### Arabic/Hebrew Support

RTL is essential for proper display of Arabic and Hebrew text:

```csharp
// Configure for Arabic
xpToolBar1.RightToLeft = RightToLeft.Yes;
xpToolBar1.Font = new Font("Arial", 10F);  // Arabic-compatible font

// Create items with Arabic text
BarItem fileItem = new BarItem();
fileItem.Text = "ملف";  // "File" in Arabic

BarItem editItem = new BarItem();
editItem.Text = "تحرير";  // "Edit" in Arabic

xpToolBar1.Items.AddRange(new BarItem[] { fileItem, editItem });
```

### Layout Considerations

When implementing RTL:

1. **Full Form RTL**: Set form's RightToLeft property too
2. **Consistent Direction**: All controls should use same direction
3. **Icon Orientation**: Some icons may need flipping
4. **Testing**: Always test with native speakers

```csharp
// Set RTL for entire form
this.RightToLeft = RightToLeft.Yes;
xpToolBar1.RightToLeft = RightToLeft.Yes;
```

### Testing RTL Layouts

Test checklist:
- Text displays correctly
- Items are ordered right-to-left
- Chevron appears on left
- Dropdown menus open correctly
- Shortcuts work properly
- No text cutoff or overlap

## Localization

Localization enables your application to support multiple languages and cultures.

### CultureInfo Usage

Use `System.Globalization.CultureInfo` to set the application culture:

```csharp
using System.Globalization;
using System.Threading;

// Set application culture
Thread.CurrentThread.CurrentCulture = new CultureInfo("de-DE");  // German
Thread.CurrentThread.CurrentUICulture = new CultureInfo("de-DE");
```

### Multi-Language Support

Create toolbars with localized text:

```csharp
private void CreateLocalizedToolbar(string culture)
{
    XPToolBar toolbar = new XPToolBar();
    
    switch (culture)
    {
        case "en-US":  // English
            CreateEnglishToolbar(toolbar);
            break;
        case "de-DE":  // German
            CreateGermanToolbar(toolbar);
            break;
        case "fr-FR":  // French
            CreateFrenchToolbar(toolbar);
            break;
        case "ar-SA":  // Arabic
            CreateArabicToolbar(toolbar);
            toolbar.RightToLeft = RightToLeft.Yes;
            break;
    }
    
    this.Controls.Add(toolbar);
}

private void CreateGermanToolbar(XPToolBar toolbar)
{
    BarItem fileItem = new BarItem("Datei");  // File
    BarItem editItem = new BarItem("Bearbeiten");  // Edit
    BarItem viewItem = new BarItem("Ansicht");  // View
    
    toolbar.Items.AddRange(new BarItem[] { fileItem, editItem, viewItem });
}
```

### Resource File Approach

Best practice: Use resource files (.resx) for localization:

```csharp
// Create Resources.resx (default - English)
// Create Resources.de-DE.resx (German)
// Create Resources.fr-FR.resx (French)

private void CreateLocalizedToolbarFromResources()
{
    XPToolBar toolbar = new XPToolBar();
    
    BarItem fileItem = new BarItem(Resources.File);  // "File" or "Datei" or "Fichier"
    BarItem editItem = new BarItem(Resources.Edit);  // "Edit" or "Bearbeiten" or "Éditer"
    BarItem viewItem = new BarItem(Resources.View);  // "View" or "Ansicht" or "Affichage"
    
    toolbar.Items.AddRange(new BarItem[] { fileItem, editItem, viewItem });
    this.Controls.Add(toolbar);
}
```

### Localization Example (German)

Complete example with German localization:

```csharp
using System.Globalization;
using Syncfusion.Windows.Forms.Tools.XPMenus;

private void CreateGermanApplication()
{
    // Set German culture
    Thread.CurrentThread.CurrentCulture = new CultureInfo("de-DE");
    Thread.CurrentThread.CurrentUICulture = new CultureInfo("de-DE");
    
    XPToolBar xpToolBar = new XPToolBar();
    xpToolBar.Dock = DockStyle.Top;
    xpToolBar.Style = VisualStyle.Office2016Colorful;
    
    // German menu items
    ParentBarItem fileMenu = new ParentBarItem();
    fileMenu.Text = "Datei";  // File
    
    DropDownBarItem editMenu = new DropDownBarItem();
    editMenu.Text = "Bearbeiten";  // Edit
    
    BarItem viewItem = new BarItem();
    viewItem.Text = "Ansicht";  // View
    
    BarItem projectItem = new BarItem();
    projectItem.Text = "Projekt";  // Project
    
    ComboBoxBarItem debugCombo = new ComboBoxBarItem();
    debugCombo.TextBoxValue = "Debuggen";  // Debug
    debugCombo.MinWidth = 120;
    
    BarItem helpItem = new BarItem();
    helpItem.Text = "Hilfe";  // Help
    
    xpToolBar.Items.AddRange(new BarItem[] { 
        fileMenu, editMenu, viewItem, projectItem, debugCombo, helpItem 
    });
    
    this.Controls.Add(xpToolBar);
}
```

## Keyboard Shortcuts

Keyboard shortcuts provide quick access to toolbar functionality and improve accessibility.

### Assigning Shortcuts to BarItems

Use the `Shortcut` property to assign keyboard shortcuts:

```csharp
this.barItem1.Shortcut = System.Windows.Forms.Shortcut.CtrlN;
this.barItem2.Shortcut = System.Windows.Forms.Shortcut.CtrlO;
this.barItem3.Shortcut = System.Windows.Forms.Shortcut.CtrlS;
```

```vb
Me.barItem1.Shortcut = System.Windows.Forms.Shortcut.CtrlN
Me.barItem2.Shortcut = System.Windows.Forms.Shortcut.CtrlO
Me.barItem3.Shortcut = System.Windows.Forms.Shortcut.CtrlS
```

### Common Shortcut Keys

Standard shortcuts follow Windows conventions:

**File Operations:**
- `Ctrl+N`: New
- `Ctrl+O`: Open
- `Ctrl+S`: Save
- `Ctrl+P`: Print
- `Ctrl+W`: Close

**Editing:**
- `Ctrl+X`: Cut
- `Ctrl+C`: Copy
- `Ctrl+V`: Paste
- `Ctrl+Z`: Undo
- `Ctrl+Y`: Redo
- `Delete`: Delete

**Other:**
- `Ctrl+F`: Find
- `Ctrl+H`: Replace
- `F1`: Help
- `F5`: Refresh

### Shortcut Property

Available shortcut combinations include:

```csharp
// Ctrl combinations
barItem.Shortcut = Shortcut.CtrlA;
barItem.Shortcut = Shortcut.CtrlB;
barItem.Shortcut = Shortcut.CtrlC;
// ... through CtrlZ

// Alt combinations
barItem.Shortcut = Shortcut.AltF4;
barItem.Shortcut = Shortcut.AltLeftArrow;

// Function keys
barItem.Shortcut = Shortcut.F1;
barItem.Shortcut = Shortcut.F2;
// ... through F12

// Ctrl+Shift combinations
barItem.Shortcut = Shortcut.CtrlShiftN;
```

### ShortcutText Property

Display custom text instead of the default shortcut display:

```csharp
this.barItem1.Shortcut = System.Windows.Forms.Shortcut.CtrlN;
this.barItem1.ShortcutText = "Press Ctrl + N";
```

```vb
Me.barItem1.Shortcut = System.Windows.Forms.Shortcut.CtrlN
Me.barItem1.ShortcutText = "Press Ctrl + N"
```

This allows custom formatting of the shortcut display in menus and tooltips.

### Keyboard Shortcut Conventions

Follow these conventions:

1. **Standard Shortcuts**: Use Windows standard shortcuts
2. **Don't Conflict**: Avoid overriding system shortcuts
3. **Document Shortcuts**: Show in tooltips and menus
4. **Logical Assignments**: Match shortcut to function (Ctrl+S for Save)
5. **Consistency**: Use same shortcuts across your application

### Accessibility Considerations

Keyboard shortcuts improve accessibility:
- Users who can't use a mouse
- Power users who prefer keyboard
- Screen reader compatibility
- Faster workflow for experienced users

## Trigger BarItems

Trigger bar items programmatically or handle their click events for custom actions.

### Click Event

The `Click` event fires when a bar item is activated:

```csharp
// Subscribe to Click event
this.barItem1.Click += BarItem1_Click;

// Event handler
private void BarItem1_Click(object sender, EventArgs e)
{
    MessageBox.Show("Bar item clicked!");
}
```

```vb
' Subscribe to Click event (in designer or code)
AddHandler Me.barItem1.Click, AddressOf BarItem1_Click

' Event handler
Private Sub BarItem1_Click(sender As Object, e As EventArgs)
    MessageBox.Show("Bar item clicked!")
End Sub
```

### Multiple Item Events

Subscribe multiple items to the same handler or different handlers:

```csharp
// Subscribe all items
this.barItem1.Click += BarItem_Click;
this.parentBarItem1.Click += BarItem_Click;
this.dropDownBarItem1.Click += BarItem_Click;

// Common handler
private void BarItem_Click(object sender, EventArgs e)
{
    if (sender is BarItem barItem)
    {
        MessageBox.Show($"{barItem.Text} was clicked");
    }
}
```

### Programmatic Triggering

While there's no direct "trigger" method, you can invoke the click event handler:

```csharp
// Simulate a click programmatically
private void TriggerSaveAction()
{
    // Call the event handler directly
    SaveItem_Click(saveItem, EventArgs.Empty);
}
```

### Custom Actions via Events

Implement custom functionality in click handlers:

```csharp
private void NewItem_Click(object sender, EventArgs e)
{
    CreateNewDocument();
}

private void SaveItem_Click(object sender, EventArgs e)
{
    SaveCurrentDocument();
}

private void PrintItem_Click(object sender, EventArgs e)
{
    PrintDocument();
}

private void CreateNewDocument()
{
    // Your custom logic
    MessageBox.Show("Creating new document...");
}
```

## Complete Theme Example

Comprehensive example applying Office2016Colorful theme with full configuration:

```csharp
using Syncfusion.Windows.Forms.Tools.XPMenus;
using System.Drawing;
using System.Windows.Forms;

public class ThemedToolbarExample : Form
{
    private XPToolBar themedToolbar;
    
    public ThemedToolbarExample()
    {
        InitializeThemedToolbar();
    }
    
    private void InitializeThemedToolbar()
    {
        // Create toolbar with Office2016Colorful theme
        themedToolbar = new XPToolBar();
        themedToolbar.Dock = DockStyle.Top;
        themedToolbar.Style = VisualStyle.Office2016Colorful;
        themedToolbar.ShowChevron = true;
        
        // Create items
        BarItem newItem = new BarItem("New");
        newItem.Shortcut = Shortcut.CtrlN;
        newItem.Tooltip = "Create a new document (Ctrl+N)";
        newItem.Click += NewItem_Click;
        
        BarItem openItem = new BarItem("Open");
        openItem.Shortcut = Shortcut.CtrlO;
        openItem.Tooltip = "Open an existing document (Ctrl+O)";
        openItem.Click += OpenItem_Click;
        
        BarItem saveItem = new BarItem("Save");
        saveItem.Shortcut = Shortcut.CtrlS;
        saveItem.Tooltip = "Save the current document (Ctrl+S)";
        saveItem.Click += SaveItem_Click;
        
        ParentBarItem formatMenu = new ParentBarItem("Format");
        formatMenu.Tooltip = "Formatting options";
        
        BarItem printItem = new BarItem("Print");
        printItem.Shortcut = Shortcut.CtrlP;
        printItem.Tooltip = "Print the document (Ctrl+P)";
        printItem.Click += PrintItem_Click;
        
        // Add items to toolbar
        themedToolbar.Items.AddRange(new BarItem[] {
            newItem, openItem, saveItem, formatMenu, printItem
        });
        
        // Add separators
        themedToolbar.SeparatorIndices.AddRange(new int[] { 0, 3 });
        
        // Add to form
        this.Controls.Add(themedToolbar);
        
        // Form properties
        this.Text = "Themed Toolbar Example - Office2016Colorful";
        this.Size = new Size(800, 600);
    }
    
    private void NewItem_Click(object sender, EventArgs e)
    {
        MessageBox.Show("New document created", "New");
    }
    
    private void OpenItem_Click(object sender, EventArgs e)
    {
        MessageBox.Show("Open dialog would appear here", "Open");
    }
    
    private void SaveItem_Click(object sender, EventArgs e)
    {
        MessageBox.Show("Document saved", "Save");
    }
    
    private void PrintItem_Click(object sender, EventArgs e)
    {
        MessageBox.Show("Print dialog would appear here", "Print");
    }
}
```

## Complete RTL Example

Full example with RTL support for Arabic-style toolbar:

```csharp
using Syncfusion.Windows.Forms.Tools.XPMenus;
using System.Drawing;
using System.Globalization;
using System.Threading;
using System.Windows.Forms;

public class RTLToolbarExample : Form
{
    private XPToolBar rtlToolbar;
    
    public RTLToolbarExample()
    {
        InitializeRTLToolbar();
    }
    
    private void InitializeRTLToolbar()
    {
        // Set Arabic culture
        Thread.CurrentThread.CurrentCulture = new CultureInfo("ar-SA");
        Thread.CurrentThread.CurrentUICulture = new CultureInfo("ar-SA");
        
        // Create toolbar with RTL support
        rtlToolbar = new XPToolBar();
        rtlToolbar.Dock = DockStyle.Top;
        rtlToolbar.Style = VisualStyle.Office2016Colorful;
        rtlToolbar.RightToLeft = RightToLeft.Yes;  // Enable RTL
        rtlToolbar.ShowChevron = true;
        
        // Use Arabic-compatible font
        rtlToolbar.Font = new Font("Arial", 10F);
        
        // Create items with Arabic text
        BarItem fileItem = new BarItem();
        fileItem.Text = "ملف";  // File
        fileItem.Tooltip = "قائمة الملف";  // File menu
        fileItem.ShowTooltip = true;
        
        BarItem editItem = new BarItem();
        editItem.Text = "تحرير";  // Edit
        editItem.Tooltip = "قائمة التحرير";  // Edit menu
        editItem.ShowTooltip = true;
        
        BarItem viewItem = new BarItem();
        viewItem.Text = "عرض";  // View
        viewItem.Tooltip = "قائمة العرض";  // View menu
        viewItem.ShowTooltip = true;
        
        BarItem toolsItem = new BarItem();
        toolsItem.Text = "أدوات";  // Tools
        toolsItem.Tooltip = "قائمة الأدوات";  // Tools menu
        toolsItem.ShowTooltip = true;
        
        BarItem helpItem = new BarItem();
        helpItem.Text = "مساعدة";  // Help
        helpItem.Tooltip = "قائمة المساعدة";  // Help menu
        helpItem.ShowTooltip = true;
        
        // Add items to toolbar (they will display right-to-left)
        rtlToolbar.Items.AddRange(new BarItem[] {
            fileItem, editItem, viewItem, toolsItem, helpItem
        });
        
        // Add separator
        rtlToolbar.SeparatorIndices.Add(3);
        
        // Add to form
        this.Controls.Add(rtlToolbar);
        
        // Set form RTL as well
        this.RightToLeft = RightToLeft.Yes;
        this.Text = "مثال على شريط الأدوات";  // "Toolbar Example" in Arabic
        this.Size = new Size(800, 600);
    }
}
```

```vb
Imports Syncfusion.Windows.Forms.Tools.XPMenus
Imports System.Drawing
Imports System.Globalization
Imports System.Threading
Imports System.Windows.Forms

Public Class RTLToolbarExample
    Inherits Form
    
    Private rtlToolbar As XPToolBar
    
    Public Sub New()
        InitializeRTLToolbar()
    End Sub
    
    Private Sub InitializeRTLToolbar()
        ' Set Arabic culture
        Thread.CurrentThread.CurrentCulture = New CultureInfo("ar-SA")
        Thread.CurrentThread.CurrentUICulture = New CultureInfo("ar-SA")
        
        ' Create toolbar with RTL support
        rtlToolbar = New XPToolBar()
        rtlToolbar.Dock = DockStyle.Top
        rtlToolbar.Style = VisualStyle.Office2016Colorful
        rtlToolbar.RightToLeft = RightToLeft.Yes  ' Enable RTL
        rtlToolbar.ShowChevron = True
        
        ' Use Arabic-compatible font
        rtlToolbar.Font = New Font("Arial", 10F)
        
        ' Create items with Arabic text
        Dim fileItem As New BarItem()
        fileItem.Text = "ملف"  ' File
        fileItem.Tooltip = "قائمة الملف"  ' File menu
        fileItem.ShowTooltip = True
        
        Dim editItem As New BarItem()
        editItem.Text = "تحرير"  ' Edit
        editItem.Tooltip = "قائمة التحرير"  ' Edit menu
        editItem.ShowTooltip = True
        
        Dim viewItem As New BarItem()
        viewItem.Text = "عرض"  ' View
        viewItem.Tooltip = "قائمة العرض"  ' View menu
        viewItem.ShowTooltip = True
        
        Dim toolsItem As New BarItem()
        toolsItem.Text = "أدوات"  ' Tools
        toolsItem.Tooltip = "قائمة الأدوات"  ' Tools menu
        toolsItem.ShowTooltip = True
        
        Dim helpItem As New BarItem()
        helpItem.Text = "مساعدة"  ' Help
        helpItem.Tooltip = "قائمة المساعدة"  ' Help menu
        helpItem.ShowTooltip = True
        
        ' Add items to toolbar
        rtlToolbar.Items.AddRange(New BarItem() {
            fileItem, editItem, viewItem, toolsItem, helpItem
        })
        
        ' Add separator
        rtlToolbar.SeparatorIndices.Add(3)
        
        ' Add to form
        Me.Controls.Add(rtlToolbar)
        
        ' Set form RTL as well
        Me.RightToLeft = RightToLeft.Yes
        Me.Text = "مثال على شريط الأدوات"  ' "Toolbar Example" in Arabic
        Me.Size = New Size(800, 600)
    End Sub
End Class
```

## Complete Keyboard Shortcuts Example

Comprehensive example with keyboard shortcuts for menu bar functionality:

```csharp
using Syncfusion.Windows.Forms.Tools.XPMenus;
using System;
using System.Drawing;
using System.Windows.Forms;

public class KeyboardShortcutsExample : Form
{
    private XPToolBar menuToolbar;
    
    public KeyboardShortcutsExample()
    {
        InitializeMenuWithShortcuts();
    }
    
    private void InitializeMenuWithShortcuts()
    {
        // Create menu-style toolbar
        menuToolbar = new XPToolBar();
        menuToolbar.Dock = DockStyle.Top;
        menuToolbar.Style = VisualStyle.Office2016Colorful;
        
        // File menu items with shortcuts
        BarItem newItem = new BarItem("&New");
        newItem.Shortcut = Shortcut.CtrlN;
        newItem.Tooltip = "Create a new document (Ctrl+N)";
        newItem.ShowTooltip = true;
        newItem.Click += NewItem_Click;
        
        BarItem openItem = new BarItem("&Open");
        openItem.Shortcut = Shortcut.CtrlO;
        openItem.Tooltip = "Open an existing document (Ctrl+O)";
        openItem.ShowTooltip = true;
        openItem.Click += OpenItem_Click;
        
        BarItem saveItem = new BarItem("&Save");
        saveItem.Shortcut = Shortcut.CtrlS;
        saveItem.Tooltip = "Save the current document (Ctrl+S)";
        saveItem.ShowTooltip = true;
        saveItem.Click += SaveItem_Click;
        
        BarItem saveAsItem = new BarItem("Save &As");
        saveAsItem.Shortcut = Shortcut.CtrlShiftS;
        saveAsItem.Tooltip = "Save with a new name (Ctrl+Shift+S)";
        saveAsItem.ShowTooltip = true;
        saveAsItem.Click += SaveAsItem_Click;
        
        BarItem closeItem = new BarItem("&Close");
        closeItem.Shortcut = Shortcut.CtrlW;
        closeItem.Tooltip = "Close the document (Ctrl+W)";
        closeItem.ShowTooltip = true;
        closeItem.Click += CloseItem_Click;
        
        BarItem printItem = new BarItem("&Print");
        printItem.Shortcut = Shortcut.CtrlP;
        printItem.Tooltip = "Print the document (Ctrl+P)";
        printItem.ShowTooltip = true;
        printItem.Click += PrintItem_Click;
        
        // Edit menu items with shortcuts
        BarItem undoItem = new BarItem("&Undo");
        undoItem.Shortcut = Shortcut.CtrlZ;
        undoItem.Tooltip = "Undo the last action (Ctrl+Z)";
        undoItem.ShowTooltip = true;
        undoItem.Click += UndoItem_Click;
        
        BarItem redoItem = new BarItem("&Redo");
        redoItem.Shortcut = Shortcut.CtrlY;
        redoItem.Tooltip = "Redo the last undone action (Ctrl+Y)";
        redoItem.ShowTooltip = true;
        redoItem.Click += RedoItem_Click;
        
        BarItem cutItem = new BarItem("Cu&t");
        cutItem.Shortcut = Shortcut.CtrlX;
        cutItem.Tooltip = "Cut the selection (Ctrl+X)";
        cutItem.ShowTooltip = true;
        cutItem.Click += CutItem_Click;
        
        BarItem copyItem = new BarItem("&Copy");
        copyItem.Shortcut = Shortcut.CtrlC;
        copyItem.Tooltip = "Copy the selection (Ctrl+C)";
        copyItem.ShowTooltip = true;
        copyItem.Click += CopyItem_Click;
        
        BarItem pasteItem = new BarItem("&Paste");
        pasteItem.Shortcut = Shortcut.CtrlV;
        pasteItem.Tooltip = "Paste from clipboard (Ctrl+V)";
        pasteItem.ShowTooltip = true;
        pasteItem.Click += PasteItem_Click;
        
        BarItem selectAllItem = new BarItem("Select &All");
        selectAllItem.Shortcut = Shortcut.CtrlA;
        selectAllItem.Tooltip = "Select all (Ctrl+A)";
        selectAllItem.ShowTooltip = true;
        selectAllItem.Click += SelectAllItem_Click;
        
        // Search items
        BarItem findItem = new BarItem("&Find");
        findItem.Shortcut = Shortcut.CtrlF;
        findItem.Tooltip = "Find text (Ctrl+F)";
        findItem.ShowTooltip = true;
        findItem.Click += FindItem_Click;
        
        BarItem replaceItem = new BarItem("&Replace");
        replaceItem.Shortcut = Shortcut.CtrlH;
        replaceItem.Tooltip = "Find and replace (Ctrl+H)";
        replaceItem.ShowTooltip = true;
        replaceItem.Click += ReplaceItem_Click;
        
        // Help item
        BarItem helpItem = new BarItem("&Help");
        helpItem.Shortcut = Shortcut.F1;
        helpItem.Tooltip = "Show help (F1)";
        helpItem.ShowTooltip = true;
        helpItem.Click += HelpItem_Click;
        
        // Add all items
        menuToolbar.Items.AddRange(new BarItem[] {
            newItem, openItem, saveItem, saveAsItem, closeItem, printItem,
            undoItem, redoItem, cutItem, copyItem, pasteItem, selectAllItem,
            findItem, replaceItem, helpItem
        });
        
        // Add separators for grouping
        menuToolbar.SeparatorIndices.AddRange(new int[] { 0, 6, 12, 14 });
        
        // Add to form
        this.Controls.Add(menuToolbar);
        
        // Form properties
        this.Text = "Keyboard Shortcuts Example";
        this.Size = new Size(1000, 600);
        this.KeyPreview = true;  // Important for shortcuts to work
    }
    
    // Event handlers
    private void NewItem_Click(object sender, EventArgs e)
    {
        MessageBox.Show("New (Ctrl+N) - Creating new document", "Action");
    }
    
    private void OpenItem_Click(object sender, EventArgs e)
    {
        MessageBox.Show("Open (Ctrl+O) - Opening document", "Action");
    }
    
    private void SaveItem_Click(object sender, EventArgs e)
    {
        MessageBox.Show("Save (Ctrl+S) - Saving document", "Action");
    }
    
    private void SaveAsItem_Click(object sender, EventArgs e)
    {
        MessageBox.Show("Save As (Ctrl+Shift+S) - Save with new name", "Action");
    }
    
    private void CloseItem_Click(object sender, EventArgs e)
    {
        MessageBox.Show("Close (Ctrl+W) - Closing document", "Action");
    }
    
    private void PrintItem_Click(object sender, EventArgs e)
    {
        MessageBox.Show("Print (Ctrl+P) - Printing document", "Action");
    }
    
    private void UndoItem_Click(object sender, EventArgs e)
    {
        MessageBox.Show("Undo (Ctrl+Z) - Undoing last action", "Action");
    }
    
    private void RedoItem_Click(object sender, EventArgs e)
    {
        MessageBox.Show("Redo (Ctrl+Y) - Redoing action", "Action");
    }
    
    private void CutItem_Click(object sender, EventArgs e)
    {
        MessageBox.Show("Cut (Ctrl+X) - Cutting selection", "Action");
    }
    
    private void CopyItem_Click(object sender, EventArgs e)
    {
        MessageBox.Show("Copy (Ctrl+C) - Copying selection", "Action");
    }
    
    private void PasteItem_Click(object sender, EventArgs e)
    {
        MessageBox.Show("Paste (Ctrl+V) - Pasting from clipboard", "Action");
    }
    
    private void SelectAllItem_Click(object sender, EventArgs e)
    {
        MessageBox.Show("Select All (Ctrl+A) - Selecting all", "Action");
    }
    
    private void FindItem_Click(object sender, EventArgs e)
    {
        MessageBox.Show("Find (Ctrl+F) - Opening find dialog", "Action");
    }
    
    private void ReplaceItem_Click(object sender, EventArgs e)
    {
        MessageBox.Show("Replace (Ctrl+H) - Opening replace dialog", "Action");
    }
    
    private void HelpItem_Click(object sender, EventArgs e)
    {
        MessageBox.Show("Help (F1) - Showing help", "Action");
    }
}
```

## Best Practices

### Theme Selection Guidance

1. **Default Recommendation**: Use `Office2016Colorful` for most applications
2. **User Preference**: Allow users to choose their preferred theme
3. **Match Platform**: Consider OS version and conventions
4. **Test All Themes**: Verify your toolbar looks good in all selected themes
5. **Document Choice**: Explain theme selection in user documentation

### RTL Testing Recommendations

1. **Native Speakers**: Always test with native Arabic/Hebrew speakers
2. **Full Coverage**: Test all toolbar functionality in RTL mode
3. **Visual Verification**: Check that layout mirrors correctly
4. **Text Rendering**: Ensure fonts render properly
5. **Icon Direction**: Some icons may need to be flipped for RTL

### Localization Best Practices

1. **Use Resource Files**: Don't hardcode strings
2. **Plan Early**: Design for localization from the start
3. **String Length**: Allow for text expansion (German can be 30% longer)
4. **Cultural Considerations**: Colors and icons may have different meanings
5. **Test with Real Data**: Use actual translations, not placeholders

### Keyboard Shortcut Conventions

1. **Follow Windows Standards**: Use Ctrl+C for Copy, etc.
2. **Don't Conflict**: Avoid overriding system shortcuts
3. **Document Clearly**: Show shortcuts in tooltips and help
4. **Mnemonic Keys**: Use & for Alt+key access
5. **Consistency**: Same shortcuts throughout application

### Accessibility Considerations

1. **Keyboard Access**: All functions must be keyboard-accessible
2. **High Contrast**: Test with high contrast themes
3. **Screen Readers**: Provide proper text for screen readers
4. **Clear Labels**: Use descriptive text, not just icons
5. **Focus Indicators**: Ensure keyboard focus is visible

### Performance Considerations

1. **Theme Switching**: Minimize runtime theme changes
2. **Resource Loading**: Use embedded resources for faster loading
3. **Image Optimization**: Optimize image sizes
4. **Lazy Loading**: Load localized resources only when needed

### Deployment Considerations

1. **Resource Packaging**: Include all localization resources
2. **Font Availability**: Ensure required fonts are available
3. **Culture Detection**: Automatically detect user's culture
4. **Fallback Language**: Provide English as fallback
5. **Testing**: Test on clean machines without development tools

These best practices ensure your toolbar is professional, accessible, and globally-ready for diverse user bases.
