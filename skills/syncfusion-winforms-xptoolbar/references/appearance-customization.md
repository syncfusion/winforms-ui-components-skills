# Appearance Customization

## Table of Contents

1. [Overview](#overview)
2. [Text Customization](#text-customization)
   - [Text Property](#text-property)
   - [Font Property](#font-property)
   - [Text Examples](#text-examples)
3. [Colors](#colors)
   - [Background Color](#background-color)
   - [Foreground Color](#foreground-color)
   - [Background Image](#background-image)
   - [Color Examples](#color-examples)
4. [Images and Icons](#images-and-icons)
   - [Image Property](#image-property)
   - [Image Size Recommendations](#image-size-recommendations)
   - [Image Resources](#image-resources)
   - [Icon vs Text+Icon](#icon-vs-texticon)
   - [Image Examples](#image-examples)
5. [Tooltips](#tooltips)
   - [Tooltip Property](#tooltip-property)
   - [ShowTooltip Property](#showtooltip-property)
   - [Tooltip Best Practices](#tooltip-best-practices)
   - [Tooltip Examples](#tooltip-examples)
6. [Sizing](#sizing)
   - [Toolbar Size](#toolbar-size)
   - [Item Sizing](#item-sizing)
   - [Sizing Examples](#sizing-examples)
7. [Custom Appearance Example](#custom-appearance-example)
8. [Font Configuration Example](#font-configuration-example)
9. [Image-Based Toolbar Example](#image-based-toolbar-example)
10. [Best Practices](#best-practices)

## Overview

The Syncfusion WinForms XPToolBar control provides rich customization options for appearance, allowing you to create professional-looking toolbars that match your application's design. You can customize text, colors, images, tooltips, sizing, and more at both the toolbar level and individual item level. This flexibility ensures your toolbar integrates seamlessly with your application's visual design.

## Text Customization

Text customization allows you to control how text appears on toolbar items and the toolbar itself.

### Text Property

Each `BarItem` has a `Text` property that defines the text displayed on the toolbar:

```csharp
// Set text for individual items
this.barItem1.Text = "New";
this.barItem2.Text = "Open";
this.barItem3.Text = "Save";
```

```vb
' Set text for individual items
Me.barItem1.Text = "New"
Me.barItem2.Text = "Open"
Me.barItem3.Text = "Save"
```

You can also use special characters like ampersand (&) for keyboard mnemonics:

```csharp
this.barItem1.Text = "&New";      // Alt+N will activate
this.barItem2.Text = "&Open";     // Alt+O will activate
this.barItem3.Text = "&Save";     // Alt+S will activate
```

### Font Property

The `Font` property controls the font family, size, and style. You can set fonts at both the toolbar level (affecting all items) and at the individual item level.

**Toolbar-Level Font:**

```csharp
// Set font for entire toolbar
this.xpToolBar1.Font = new System.Drawing.Font("Segoe UI", 10F, System.Drawing.FontStyle.Regular);
```

```vb
' Set font for entire toolbar
Me.xpToolBar1.Font = New System.Drawing.Font("Segoe UI", 10F, System.Drawing.FontStyle.Regular)
```

**Font Styles:**

You can apply various font styles including Bold, Italic, Underline, and Strikeout:

```csharp
// Bold font
this.xpToolBar1.Font = new System.Drawing.Font("Arial", 10F, System.Drawing.FontStyle.Bold);

// Bold and Italic
this.xpToolBar1.Font = new System.Drawing.Font("Arial", 10F, 
    System.Drawing.FontStyle.Bold | System.Drawing.FontStyle.Italic);

// Bold with Strikeout
this.xpToolBar1.Font = new System.Drawing.Font("Verdana", 12F, 
    System.Drawing.FontStyle.Bold | System.Drawing.FontStyle.Strikeout);
```

### Text Examples

Complete example with various text customizations:

```csharp
// Create items with different text
BarItem newItem = new BarItem();
newItem.Text = "&New Document";

BarItem openItem = new BarItem();
openItem.Text = "&Open File...";

BarItem saveItem = new BarItem();
saveItem.Text = "&Save";

// Set toolbar font
xpToolBar1.Font = new System.Drawing.Font("Segoe UI", 9.75F, System.Drawing.FontStyle.Regular);

// Add items to toolbar
xpToolBar1.Items.AddRange(new BarItem[] { newItem, openItem, saveItem });
```

## Colors

Color customization allows you to create visually appealing toolbars that match your application's color scheme.

### Background Color

The `BackColor` property sets the background color of the entire toolbar:

```csharp
// Set background color
this.xpToolBar1.BackColor = System.Drawing.Color.SkyBlue;
```

```vb
' Set background color
Me.xpToolBar1.BackColor = System.Drawing.Color.SkyBlue
```

You can use any color from the `System.Drawing.Color` structure or create custom colors:

```csharp
// Named colors
this.xpToolBar1.BackColor = System.Drawing.Color.LightGray;

// Custom RGB color
this.xpToolBar1.BackColor = System.Drawing.Color.FromArgb(240, 240, 245);

// ARGB with transparency (not commonly used for toolbars)
this.xpToolBar1.BackColor = System.Drawing.Color.FromArgb(255, 240, 240, 245);
```

### Foreground Color

The `ForeColor` property sets the text color for the toolbar:

```csharp
// Set text color
this.xpToolBar1.ForeColor = System.Drawing.Color.Red;
```

```vb
' Set text color
Me.xpToolBar1.ForeColor = System.Drawing.Color.Red
```

This affects all text displayed on toolbar items unless overridden at the item level.

### Background Image

You can set a background image for the toolbar using the `BackgroundImage` property:

```csharp
// Set background image from file
this.xpToolBar1.BackgroundImage = System.Drawing.Image.FromFile(@"C:\Images\toolbar-bg.png");

// Set image layout
this.xpToolBar1.BackgroundImageLayout = System.Windows.Forms.ImageLayout.Stretch;
```

```vb
' Set background image from file
Me.xpToolBar1.BackgroundImage = System.Drawing.Image.FromFile("C:\Images\toolbar-bg.png")

' Set image layout
Me.xpToolBar1.BackgroundImageLayout = System.Windows.Forms.ImageLayout.Stretch
```

**ImageLayout Options:**

- `None`: Image is positioned at top-left
- `Tile`: Image is tiled to fill the area
- `Center`: Image is centered
- `Stretch`: Image is stretched to fill the area
- `Zoom`: Image is scaled proportionally

### Color Examples

Complete example with color customization:

```csharp
// Professional blue theme
xpToolBar1.BackColor = System.Drawing.Color.FromArgb(0, 120, 215);
xpToolBar1.ForeColor = System.Drawing.Color.White;

// Light theme
xpToolBar1.BackColor = System.Drawing.Color.FromArgb(243, 243, 243);
xpToolBar1.ForeColor = System.Drawing.Color.FromArgb(32, 32, 32);

// Dark theme
xpToolBar1.BackColor = System.Drawing.Color.FromArgb(45, 45, 48);
xpToolBar1.ForeColor = System.Drawing.Color.FromArgb(241, 241, 241);
```

## Images and Icons

Images and icons enhance toolbar usability by providing visual cues for toolbar actions.

### Image Property

Individual `BarItem` objects can display images using the `Image` property or by referencing an `ImageList`:

**Using Direct Image:**

```csharp
// Set image directly
this.barItem1.Image = System.Drawing.Image.FromFile(@"C:\Icons\new.png");
```

**Using ImageList (Recommended):**

```csharp
// Create and populate ImageList
ImageList imageList1 = new ImageList();
imageList1.Images.Add(Image.FromFile(@"C:\Icons\new.png"));      // Index 0
imageList1.Images.Add(Image.FromFile(@"C:\Icons\open.png"));     // Index 1
imageList1.Images.Add(Image.FromFile(@"C:\Icons\save.png"));     // Index 2

// Assign ImageList to items
this.barItem1.ImageList = imageList1;
this.barItem1.ImageIndex = 0;  // New icon

this.barItem2.ImageList = imageList1;
this.barItem2.ImageIndex = 1;  // Open icon

this.barItem3.ImageList = imageList1;
this.barItem3.ImageIndex = 2;  // Save icon
```

```vb
' Create and populate ImageList
Dim imageList1 As New ImageList()
imageList1.Images.Add(Image.FromFile("C:\Icons\new.png"))      ' Index 0
imageList1.Images.Add(Image.FromFile("C:\Icons\open.png"))     ' Index 1
imageList1.Images.Add(Image.FromFile("C:\Icons\save.png"))     ' Index 2

' Assign ImageList to items
Me.barItem1.ImageList = imageList1
Me.barItem1.ImageIndex = 0  ' New icon

Me.barItem2.ImageList = imageList1
Me.barItem2.ImageIndex = 1  ' Open icon

Me.barItem3.ImageList = imageList1
Me.barItem3.ImageIndex = 2  ' Save icon
```

### Image Size Recommendations

For best results, use consistent image sizes:

**Standard Sizes:**
- **16x16 pixels**: Compact toolbars, good for dense layouts
- **24x24 pixels**: Standard toolbar size, most commonly used
- **32x32 pixels**: Large toolbars, better for touch interfaces

**Best Practices:**
- Use the same size for all images in a single toolbar
- Use high-quality PNG images with transparency
- Consider high-DPI displays (provide 2x images if needed)
- Keep images simple and recognizable at small sizes

```csharp
// Set ImageList size
imageList1.ImageSize = new System.Drawing.Size(24, 24);  // 24x24 icons
```

### Image Resources

Using embedded resources is recommended for production applications:

```csharp
// Add images to project resources (Properties > Resources)
// Then access them:
this.barItem1.Image = Properties.Resources.icon_new;
this.barItem2.Image = Properties.Resources.icon_open;
this.barItem3.Image = Properties.Resources.icon_save;
```

This approach:
- Embeds images in the executable
- Avoids missing file issues
- Simplifies deployment

### Icon vs Text+Icon

You can display items with icons only, text only, or both:

**Icon Only:**
```csharp
barItem1.Image = Properties.Resources.icon_new;
barItem1.Text = "";  // No text, icon only
barItem1.Tooltip = "Create new document";  // Tooltip is essential!
```

**Text Only:**
```csharp
barItem1.Text = "New";
// No image assigned
```

**Icon and Text:**
```csharp
barItem1.Image = Properties.Resources.icon_new;
barItem1.Text = "New";
```

**Important**: When using icon-only buttons, **always** provide tooltips!

### Image Examples

Complete example with images:

```csharp
// Create ImageList with standard 24x24 icons
ImageList toolbarImageList = new ImageList();
toolbarImageList.ImageSize = new System.Drawing.Size(24, 24);
toolbarImageList.ColorDepth = ColorDepth.Depth32Bit;

// Add images from resources
toolbarImageList.Images.Add(Properties.Resources.icon_new);
toolbarImageList.Images.Add(Properties.Resources.icon_open);
toolbarImageList.Images.Add(Properties.Resources.icon_save);
toolbarImageList.Images.Add(Properties.Resources.icon_print);

// Create items with images
BarItem newItem = new BarItem("New");
newItem.ImageList = toolbarImageList;
newItem.ImageIndex = 0;
newItem.Tooltip = "Create a new document";

BarItem openItem = new BarItem("Open");
openItem.ImageList = toolbarImageList;
openItem.ImageIndex = 1;
openItem.Tooltip = "Open an existing document";

BarItem saveItem = new BarItem("Save");
saveItem.ImageList = toolbarImageList;
saveItem.ImageIndex = 2;
saveItem.Tooltip = "Save the current document";

BarItem printItem = new BarItem("Print");
printItem.ImageList = toolbarImageList;
printItem.ImageIndex = 3;
printItem.Tooltip = "Print the document";

xpToolBar1.Items.AddRange(new BarItem[] { newItem, openItem, saveItem, printItem });
```

## Tooltips

Tooltips provide helpful hints when users hover over toolbar items.

### Tooltip Property

The `Tooltip` property sets the tooltip text for individual items:

```csharp
this.barItem1.Tooltip = "Create a new document";
this.barItem2.Tooltip = "Open an existing document";
this.barItem3.Tooltip = "Save the current document";
```

```vb
Me.barItem1.Tooltip = "Create a new document"
Me.barItem2.Tooltip = "Open an existing document"
Me.barItem3.Tooltip = "Save the current document"
```

### ShowTooltip Property

The `ShowTooltip` property controls whether tooltips are displayed:

```csharp
// Enable tooltip (default is true)
this.barItem1.ShowTooltip = true;
this.barItem1.Tooltip = "Custom tooltip text";

// Disable tooltip
this.barItem2.ShowTooltip = false;
```

**Default Behavior:**
- `ShowTooltip` is `true` by default
- If `Tooltip` is not set, the `Text` property value is shown
- Setting a custom `Tooltip` overrides the default text

### Tooltip Best Practices

1. **Always Use for Icon-Only Items**: Essential for usability
2. **Be Descriptive but Concise**: "Create a new document" not just "New"
3. **Include Keyboard Shortcuts**: "Save (Ctrl+S)"
4. **Use Title Case**: "Create New Document" not "create new document"
5. **Explain the Action**: Use verbs to describe what happens

### Tooltip Examples

Comprehensive tooltip configuration:

```csharp
// Descriptive tooltips with shortcuts
barItem1.ShowTooltip = true;
barItem1.Tooltip = "Create a new document (Ctrl+N)";

barItem2.ShowTooltip = true;
barItem2.Tooltip = "Open an existing document (Ctrl+O)";

barItem3.ShowTooltip = true;
barItem3.Tooltip = "Save the current document (Ctrl+S)";

barItem4.ShowTooltip = true;
barItem4.Tooltip = "Print the document (Ctrl+P)";

// Icon-only items MUST have tooltips
iconOnlyItem.Text = "";  // No text
iconOnlyItem.Image = Properties.Resources.icon_help;
iconOnlyItem.ShowTooltip = true;
iconOnlyItem.Tooltip = "Show help documentation (F1)";
```

```vb
' Descriptive tooltips with shortcuts
barItem1.ShowTooltip = True
barItem1.Tooltip = "Create a new document (Ctrl+N)"

barItem2.ShowTooltip = True
barItem2.Tooltip = "Open an existing document (Ctrl+O)"

barItem3.ShowTooltip = True
barItem3.Tooltip = "Save the current document (Ctrl+S)"

barItem4.ShowTooltip = True
barItem4.Tooltip = "Print the document (Ctrl+P)"

' Icon-only items MUST have tooltips
iconOnlyItem.Text = ""  ' No text
iconOnlyItem.Image = My.Resources.icon_help
iconOnlyItem.ShowTooltip = True
iconOnlyItem.Tooltip = "Show help documentation (F1)"
```

## Sizing

Control the size of the toolbar and its items for optimal layout.

### Toolbar Size

Set the overall toolbar dimensions:

```csharp
// Set size with Size property
this.xpToolBar1.Size = new System.Drawing.Size(800, 40);

// Or set width and height separately
this.xpToolBar1.Width = 800;
this.xpToolBar1.Height = 40;
```

```vb
' Set size with Size property
Me.xpToolBar1.Size = New System.Drawing.Size(800, 40)

' Or set width and height separately
Me.xpToolBar1.Width = 800
Me.xpToolBar1.Height = 40
```

**Common Heights:**
- Small toolbar: 30-35 pixels
- Standard toolbar: 40-45 pixels
- Large toolbar: 50-60 pixels

### Item Sizing

For certain item types like `TextBoxBarItem` and `ComboBoxBarItem`, you can set minimum widths:

```csharp
// Set minimum width for text box
TextBoxBarItem textBoxItem = new TextBoxBarItem();
textBoxItem.MinWidth = 200;

// Set minimum width for combo box
ComboBoxBarItem comboBoxItem = new ComboBoxBarItem();
comboBoxItem.MinWidth = 150;
```

```vb
' Set minimum width for text box
Dim textBoxItem As New TextBoxBarItem()
textBoxItem.MinWidth = 200

' Set minimum width for combo box
Dim comboBoxItem As New ComboBoxBarItem()
comboBoxItem.MinWidth = 150
```

### Sizing Examples

Complete sizing configuration:

```csharp
// Configure toolbar size
xpToolBar1.Size = new System.Drawing.Size(1000, 45);
xpToolBar1.Dock = DockStyle.Top;  // Dock to fill width

// Configure item sizes
ComboBoxBarItem fontCombo = new ComboBoxBarItem();
fontCombo.Text = "Font";
fontCombo.MinWidth = 120;
fontCombo.TextBoxValue = "Segoe UI";

ComboBoxBarItem sizeCombo = new ComboBoxBarItem();
sizeCombo.Text = "Size";
sizeCombo.MinWidth = 60;
sizeCombo.TextBoxValue = "10";

TextBoxBarItem searchBox = new TextBoxBarItem();
searchBox.MinWidth = 200;
searchBox.TextBoxValue = "Search...";

xpToolBar1.Items.AddRange(new BarItem[] { fontCombo, sizeCombo, searchBox });
```

## Custom Appearance Example

Complete example creating a themed toolbar with custom colors and styling:

```csharp
using Syncfusion.Windows.Forms.Tools.XPMenus;
using System.Drawing;
using System.Windows.Forms;

public void CreateCustomThemedToolbar()
{
    // Create toolbar with professional blue theme
    XPToolBar customToolBar = new XPToolBar();
    customToolBar.Dock = DockStyle.Top;
    
    // Set custom colors for professional appearance
    customToolBar.BackColor = System.Drawing.Color.FromArgb(0, 120, 215);  // Microsoft Blue
    customToolBar.ForeColor = System.Drawing.Color.White;
    customToolBar.Font = new System.Drawing.Font("Segoe UI", 9.75F, System.Drawing.FontStyle.Regular);
    
    // Create items
    BarItem homeItem = new BarItem();
    homeItem.Text = "Home";
    homeItem.Tooltip = "Go to home page";
    homeItem.ShowTooltip = true;
    
    BarItem dashboardItem = new BarItem();
    dashboardItem.Text = "Dashboard";
    dashboardItem.Tooltip = "View dashboard";
    dashboardItem.ShowTooltip = true;
    
    BarItem reportsItem = new BarItem();
    reportsItem.Text = "Reports";
    reportsItem.Tooltip = "View reports";
    reportsItem.ShowTooltip = true;
    
    BarItem settingsItem = new BarItem();
    settingsItem.Text = "Settings";
    settingsItem.Tooltip = "Application settings";
    settingsItem.ShowTooltip = true;
    
    // Add items to toolbar
    customToolBar.Items.AddRange(new BarItem[] { 
        homeItem, dashboardItem, reportsItem, settingsItem 
    });
    
    // Add separator
    customToolBar.SeparatorIndices.Add(2);
    
    // Add to form
    this.Controls.Add(customToolBar);
}
```

```vb
Imports Syncfusion.Windows.Forms.Tools.XPMenus
Imports System.Drawing
Imports System.Windows.Forms

Public Sub CreateCustomThemedToolbar()
    ' Create toolbar with professional blue theme
    Dim customToolBar As New XPToolBar()
    customToolBar.Dock = DockStyle.Top
    
    ' Set custom colors for professional appearance
    customToolBar.BackColor = System.Drawing.Color.FromArgb(0, 120, 215)  ' Microsoft Blue
    customToolBar.ForeColor = System.Drawing.Color.White
    customToolBar.Font = New System.Drawing.Font("Segoe UI", 9.75F, System.Drawing.FontStyle.Regular)
    
    ' Create items
    Dim homeItem As New BarItem()
    homeItem.Text = "Home"
    homeItem.Tooltip = "Go to home page"
    homeItem.ShowTooltip = True
    
    Dim dashboardItem As New BarItem()
    dashboardItem.Text = "Dashboard"
    dashboardItem.Tooltip = "View dashboard"
    dashboardItem.ShowTooltip = True
    
    Dim reportsItem As New BarItem()
    reportsItem.Text = "Reports"
    reportsItem.Tooltip = "View reports"
    reportsItem.ShowTooltip = True
    
    Dim settingsItem As New BarItem()
    settingsItem.Text = "Settings"
    settingsItem.Tooltip = "Application settings"
    settingsItem.ShowTooltip = True
    
    ' Add items to toolbar
    customToolBar.Items.AddRange(New BarItem() { 
        homeItem, dashboardItem, reportsItem, settingsItem 
    })
    
    ' Add separator
    customToolBar.SeparatorIndices.Add(2)
    
    ' Add to form
    Me.Controls.Add(customToolBar)
End Sub
```

## Font Configuration Example

Example showing different fonts for different toolbar items:

```csharp
using Syncfusion.Windows.Forms.Tools.XPMenus;
using System.Drawing;
using System.Windows.Forms;

public void CreateToolbarWithCustomFonts()
{
    XPToolBar toolbar = new XPToolBar();
    toolbar.Dock = DockStyle.Top;
    
    // Set default font for toolbar
    toolbar.Font = new System.Drawing.Font("Segoe UI", 9.75F, System.Drawing.FontStyle.Regular);
    
    // Create items with varied text emphasis
    BarItem titleItem = new BarItem();
    titleItem.Text = "APPLICATION TITLE";
    titleItem.Enabled = false;  // Display only, not clickable
    
    BarItem normalItem = new BarItem();
    normalItem.Text = "Normal Text";
    
    BarItem boldItem = new BarItem();
    boldItem.Text = "Important";
    
    // Note: Font styles are typically set at toolbar level
    // Individual item fonts follow the toolbar's font setting
    
    toolbar.Items.AddRange(new BarItem[] { titleItem, normalItem, boldItem });
    toolbar.SeparatorIndices.Add(0);
    
    this.Controls.Add(toolbar);
}
```

```vb
Imports Syncfusion.Windows.Forms.Tools.XPMenus
Imports System.Drawing
Imports System.Windows.Forms

Public Sub CreateToolbarWithCustomFonts()
    Dim toolbar As New XPToolBar()
    toolbar.Dock = DockStyle.Top
    
    ' Set default font for toolbar
    toolbar.Font = New System.Drawing.Font("Segoe UI", 9.75F, System.Drawing.FontStyle.Regular)
    
    ' Create items with varied text emphasis
    Dim titleItem As New BarItem()
    titleItem.Text = "APPLICATION TITLE"
    titleItem.Enabled = False  ' Display only, not clickable
    
    Dim normalItem As New BarItem()
    normalItem.Text = "Normal Text"
    
    Dim boldItem As New BarItem()
    boldItem.Text = "Important"
    
    ' Note: Font styles are typically set at toolbar level
    ' Individual item fonts follow the toolbar's font setting
    
    toolbar.Items.AddRange(New BarItem() {titleItem, normalItem, boldItem})
    toolbar.SeparatorIndices.Add(0)
    
    Me.Controls.Add(toolbar)
End Sub
```

## Image-Based Toolbar Example

Complete example of an icon-only toolbar with tooltips:

```csharp
using Syncfusion.Windows.Forms.Tools.XPMenus;
using System.Drawing;
using System.Windows.Forms;

public void CreateIconOnlyToolbar()
{
    // Create toolbar
    XPToolBar iconToolBar = new XPToolBar();
    iconToolBar.Dock = DockStyle.Top;
    iconToolBar.Style = VisualStyle.Office2016Colorful;
    iconToolBar.ShowChevron = true;
    
    // Create ImageList with 24x24 icons
    ImageList imageList = new ImageList();
    imageList.ImageSize = new System.Drawing.Size(24, 24);
    imageList.ColorDepth = ColorDepth.Depth32Bit;
    
    // Add images from resources (assumes you have these in Resources)
    imageList.Images.Add("new", Properties.Resources.icon_new);
    imageList.Images.Add("open", Properties.Resources.icon_open);
    imageList.Images.Add("save", Properties.Resources.icon_save);
    imageList.Images.Add("cut", Properties.Resources.icon_cut);
    imageList.Images.Add("copy", Properties.Resources.icon_copy);
    imageList.Images.Add("paste", Properties.Resources.icon_paste);
    imageList.Images.Add("undo", Properties.Resources.icon_undo);
    imageList.Images.Add("redo", Properties.Resources.icon_redo);
    
    // Create icon-only items (NO TEXT - tooltips are critical!)
    BarItem newItem = new BarItem();
    newItem.ImageList = imageList;
    newItem.ImageIndex = imageList.Images.IndexOfKey("new");
    newItem.ShowTooltip = true;
    newItem.Tooltip = "Create a new document (Ctrl+N)";
    newItem.Shortcut = Shortcut.CtrlN;
    
    BarItem openItem = new BarItem();
    openItem.ImageList = imageList;
    openItem.ImageIndex = imageList.Images.IndexOfKey("open");
    openItem.ShowTooltip = true;
    openItem.Tooltip = "Open an existing document (Ctrl+O)";
    openItem.Shortcut = Shortcut.CtrlO;
    
    BarItem saveItem = new BarItem();
    saveItem.ImageList = imageList;
    saveItem.ImageIndex = imageList.Images.IndexOfKey("save");
    saveItem.ShowTooltip = true;
    saveItem.Tooltip = "Save the current document (Ctrl+S)";
    saveItem.Shortcut = Shortcut.CtrlS;
    
    BarItem cutItem = new BarItem();
    cutItem.ImageList = imageList;
    cutItem.ImageIndex = imageList.Images.IndexOfKey("cut");
    cutItem.ShowTooltip = true;
    cutItem.Tooltip = "Cut the selection (Ctrl+X)";
    cutItem.Shortcut = Shortcut.CtrlX;
    
    BarItem copyItem = new BarItem();
    copyItem.ImageList = imageList;
    copyItem.ImageIndex = imageList.Images.IndexOfKey("copy");
    copyItem.ShowTooltip = true;
    copyItem.Tooltip = "Copy the selection (Ctrl+C)";
    copyItem.Shortcut = Shortcut.CtrlC;
    
    BarItem pasteItem = new BarItem();
    pasteItem.ImageList = imageList;
    pasteItem.ImageIndex = imageList.Images.IndexOfKey("paste");
    pasteItem.ShowTooltip = true;
    pasteItem.Tooltip = "Paste from clipboard (Ctrl+V)";
    pasteItem.Shortcut = Shortcut.CtrlV;
    
    BarItem undoItem = new BarItem();
    undoItem.ImageList = imageList;
    undoItem.ImageIndex = imageList.Images.IndexOfKey("undo");
    undoItem.ShowTooltip = true;
    undoItem.Tooltip = "Undo the last action (Ctrl+Z)";
    undoItem.Shortcut = Shortcut.CtrlZ;
    
    BarItem redoItem = new BarItem();
    redoItem.ImageList = imageList;
    redoItem.ImageIndex = imageList.Images.IndexOfKey("redo");
    redoItem.ShowTooltip = true;
    redoItem.Tooltip = "Redo the last undone action (Ctrl+Y)";
    redoItem.Shortcut = Shortcut.CtrlY;
    
    // Add items to toolbar
    iconToolBar.Items.AddRange(new BarItem[] {
        newItem, openItem, saveItem,
        cutItem, copyItem, pasteItem,
        undoItem, redoItem
    });
    
    // Add separators for grouping
    iconToolBar.SeparatorIndices.AddRange(new int[] { 0, 3, 6 });
    
    // Add to form
    this.Controls.Add(iconToolBar);
}
```

```vb
Imports Syncfusion.Windows.Forms.Tools.XPMenus
Imports System.Drawing
Imports System.Windows.Forms

Public Sub CreateIconOnlyToolbar()
    ' Create toolbar
    Dim iconToolBar As New XPToolBar()
    iconToolBar.Dock = DockStyle.Top
    iconToolBar.Style = VisualStyle.Office2016Colorful
    iconToolBar.ShowChevron = True
    
    ' Create ImageList with 24x24 icons
    Dim imageList As New ImageList()
    imageList.ImageSize = New System.Drawing.Size(24, 24)
    imageList.ColorDepth = ColorDepth.Depth32Bit
    
    ' Add images from resources
    imageList.Images.Add("new", My.Resources.icon_new)
    imageList.Images.Add("open", My.Resources.icon_open)
    imageList.Images.Add("save", My.Resources.icon_save)
    imageList.Images.Add("cut", My.Resources.icon_cut)
    imageList.Images.Add("copy", My.Resources.icon_copy)
    imageList.Images.Add("paste", My.Resources.icon_paste)
    imageList.Images.Add("undo", My.Resources.icon_undo)
    imageList.Images.Add("redo", My.Resources.icon_redo)
    
    ' Create icon-only items (NO TEXT - tooltips are critical!)
    Dim newItem As New BarItem()
    newItem.ImageList = imageList
    newItem.ImageIndex = imageList.Images.IndexOfKey("new")
    newItem.ShowTooltip = True
    newItem.Tooltip = "Create a new document (Ctrl+N)"
    newItem.Shortcut = Shortcut.CtrlN
    
    Dim openItem As New BarItem()
    openItem.ImageList = imageList
    openItem.ImageIndex = imageList.Images.IndexOfKey("open")
    openItem.ShowTooltip = True
    openItem.Tooltip = "Open an existing document (Ctrl+O)"
    openItem.Shortcut = Shortcut.CtrlO
    
    Dim saveItem As New BarItem()
    saveItem.ImageList = imageList
    saveItem.ImageIndex = imageList.Images.IndexOfKey("save")
    saveItem.ShowTooltip = True
    saveItem.Tooltip = "Save the current document (Ctrl+S)"
    saveItem.Shortcut = Shortcut.CtrlS
    
    Dim cutItem As New BarItem()
    cutItem.ImageList = imageList
    cutItem.ImageIndex = imageList.Images.IndexOfKey("cut")
    cutItem.ShowTooltip = True
    cutItem.Tooltip = "Cut the selection (Ctrl+X)"
    cutItem.Shortcut = Shortcut.CtrlX
    
    Dim copyItem As New BarItem()
    copyItem.ImageList = imageList
    copyItem.ImageIndex = imageList.Images.IndexOfKey("copy")
    copyItem.ShowTooltip = True
    copyItem.Tooltip = "Copy the selection (Ctrl+C)"
    copyItem.Shortcut = Shortcut.CtrlC
    
    Dim pasteItem As New BarItem()
    pasteItem.ImageList = imageList
    pasteItem.ImageIndex = imageList.Images.IndexOfKey("paste")
    pasteItem.ShowTooltip = True
    pasteItem.Tooltip = "Paste from clipboard (Ctrl+V)"
    pasteItem.Shortcut = Shortcut.CtrlV
    
    Dim undoItem As New BarItem()
    undoItem.ImageList = imageList
    undoItem.ImageIndex = imageList.Images.IndexOfKey("undo")
    undoItem.ShowTooltip = True
    undoItem.Tooltip = "Undo the last action (Ctrl+Z)"
    undoItem.Shortcut = Shortcut.CtrlZ
    
    Dim redoItem As New BarItem()
    redoItem.ImageList = imageList
    redoItem.ImageIndex = imageList.Images.IndexOfKey("redo")
    redoItem.ShowTooltip = True
    redoItem.Tooltip = "Redo the last undone action (Ctrl+Y)"
    redoItem.Shortcut = Shortcut.CtrlY
    
    ' Add items to toolbar
    iconToolBar.Items.AddRange(New BarItem() {
        newItem, openItem, saveItem,
        cutItem, copyItem, pasteItem,
        undoItem, redoItem
    })
    
    ' Add separators for grouping
    iconToolBar.SeparatorIndices.AddRange(New Integer() {0, 3, 6})
    
    ' Add to form
    Me.Controls.Add(iconToolBar)
End Sub
```

## Best Practices

### Consistent Icon Sizes

Use the same icon size throughout your toolbar:

- Choose one size (16x16, 24x24, or 32x32)
- Apply consistently to all items
- Mismatched sizes look unprofessional

```csharp
// Set consistent image size
imageList.ImageSize = new System.Drawing.Size(24, 24);
```

### Always Use Tooltips for Icons

Icon-only buttons **must** have tooltips:

- Users may not recognize all icons
- Tooltips provide context
- Include keyboard shortcuts in tooltips
- This is critical for usability and accessibility

### Color Contrast Considerations

Ensure good visibility:

- Text must be readable against background
- Sufficient contrast ratio (4.5:1 for normal text)
- Test with light and dark themes
- Consider colorblind users

```csharp
// Good contrast examples
// Dark background, light text
toolbar.BackColor = Color.FromArgb(45, 45, 48);
toolbar.ForeColor = Color.FromArgb(241, 241, 241);

// Light background, dark text
toolbar.BackColor = Color.FromArgb(243, 243, 243);
toolbar.ForeColor = Color.FromArgb(32, 32, 32);
```

### Font Readability

Choose readable fonts:

- Use standard system fonts (Segoe UI, Arial)
- Avoid decorative fonts
- Keep size reasonable (9-11pt for toolbars)
- Don't overuse bold or italic

```csharp
// Recommended fonts
toolbar.Font = new Font("Segoe UI", 9.75F);  // Windows default
toolbar.Font = new Font("Arial", 10F);       // Cross-platform safe
```

### Professional Appearance Tips

1. **Use Visual Styles**: Apply appropriate themes (Office2016, Metro)
2. **Group Related Items**: Use separators to create logical groups
3. **Limit Item Count**: Don't overcrowd toolbars
4. **Consistent Spacing**: Let the control handle spacing automatically
5. **Match Application Theme**: Toolbar should match your app's overall design
6. **High-Quality Icons**: Use professional icon sets
7. **Test at Different Sizes**: Verify appearance on different screens
8. **Disable Unused Items**: Gray out items that don't apply to current context

```csharp
// Disable items when not applicable
saveItem.Enabled = false;  // When nothing to save

// Enable when applicable
saveItem.Enabled = true;   // When document is modified
```

These best practices ensure your toolbar is both functional and visually appealing, providing an excellent user experience.
