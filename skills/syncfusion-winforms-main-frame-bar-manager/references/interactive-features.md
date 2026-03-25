# Interactive Features

## Table of Contents
- [SuperTooltip Support](#supertooltip-support)
- [Context Menu Integration](#context-menu-integration)
- [Tooltip Configuration](#tooltip-configuration)
- [Advanced Tooltip Customization](#advanced-tooltip-customization)
- [Best Practices](#best-practices)

---

## SuperTooltip Support

SuperTooltip provides rich, formatted tooltip content with images, headers, and detailed descriptions - more advanced than standard tooltips.

### Adding SuperTooltip Control

Add the SuperToolTip control to your form:

```csharp
// Create SuperToolTip instance
SuperToolTip superToolTip1 = new SuperToolTip();

// Add to form
this.Controls.Add(superToolTip1);
```

### Designer Approach

1. Open Windows Forms designer
2. Locate **SuperToolTip** in toolbox (Syncfusion Tools)
3. Drag onto form (appears in component tray)
4. Rename to descriptive name (e.g., superToolTip1)

### Associating with Menu Items

```csharp
// Create menu item
BarItem saveItem = new BarItem() { Text = "&Save" };

// Get extended property from SuperToolTip
ToolTipInfo toolTipInfo = new ToolTipInfo();
toolTipInfo.Body.Text = "Save the current document to disk";
toolTipInfo.Header.Text = "Save Document";
toolTipInfo.Body.Image = imageList1.Images[0];  // Optional image

// Associate tooltip with menu item
superToolTip1.SetToolTip(saveItem, toolTipInfo);

mainFrameBarManager1.Items.Add(saveItem);
```

### Using Tooltip Editor

Configure rich tooltips via the designer:

1. Select a BarItem in the designer
2. In Properties, find the **ToolTip** property
3. Set to your SuperToolTip instance
4. Click ellipsis (...) button to open **Tooltip Editor**
5. Configure:
   - **Header:** Title text
   - **Body:** Description text with optional image
   - **Footer:** Additional information
6. Click OK to apply

### Tooltip Editor Features

The Tooltip Editor allows configuring:

- **Header Text:** Title displayed at top
- **Header Image:** Optional icon for header
- **Body Text:** Main content/description
- **Body Image:** Optional icon for body
- **Footer Text:** Additional notes
- **Footer Image:** Optional footer icon
- **Appearance:** Background color, text color, font

## Context Menu Integration

Right-click context menus provide on-demand customization options.

### Default Context Menu

By default, right-clicking a toolbar displays the context menu:

```
Customize this Toolbar
Reset Toolbar
New Toolbar...
Delete Toolbar...
```

This enables users to add/remove items and customize the toolbar layout.

### Disabling Context Menu

To prevent end-user customization:

```csharp
// Disable context menu entirely (if needed)
// Note: Property name may vary by framework version
mainFrameBarManager1.AllowContextMenu = false;
```

### Customization Dialog

Right-click context menu opens the **Customize** dialog, which allows:

- Adding/removing toolbars
- Adding/removing menu items
- Reordering items
- Resetting to defaults
- Assigning keyboard shortcuts

Users can access the same dialog via menu items:

```csharp
// Add Customize menu item
BarItem customizeItem = new BarItem() { Text = "Customize &Menu..." };
customizeItem.ItemClick += (sender, args) =>
{
    // Open customization dialog programmatically
    mainFrameBarManager1.ShowCustomizeDialog();
};
```

---

## Tooltip Configuration

Control tooltip display behavior for individual items.

### Default Tooltip

Every BarItem displays a default tooltip showing the Text property:

```csharp
BarItem openItem = new BarItem() { Text = "&Open File" };

// Tooltip automatically shows "Open File" on hover
mainFrameBarManager1.Items.Add(openItem);
```

### Enabling/Disabling Tooltip

```csharp
// Enable tooltip (default is true)
openItem.ShowTooltip = true;

// Disable tooltip
openItem.ShowTooltip = false;
```

### Custom Tooltip Text

Override default tooltip with custom text:

```csharp
BarItem printItem = new BarItem() { Text = "&Print" };

// Add explicit tooltip
ToolTipInfo customTip = new ToolTipInfo();
customTip.Body.Text = "Print the current document (Ctrl+P)";
customTip.Header.Text = "Print";

// Assign custom tooltip
superToolTip1.SetToolTip(printItem, customTip);

mainFrameBarManager1.Items.Add(printItem);
```

### Disabling Default Tooltip

To show only custom SuperTooltip without the default tooltip:

```csharp
printItem.ShowTooltip = false;  // Hide default tooltip
superToolTip1.SetToolTip(printItem, customTip);  // Show SuperTooltip only
```

---

## Advanced Tooltip Customization

### Rich Formatted Tooltips

Create detailed tooltips with multiple sections:

```csharp
BarItem helpItem = new BarItem() { Text = "&Help" };

ToolTipInfo helpTip = new ToolTipInfo();

// Header with icon
helpTip.Header.Text = "Help and Support";
helpTip.Header.Image = Properties.Resources.HelpIcon;

// Body with detailed description
helpTip.Body.Text = "Open the help documentation.\n\n" +
    "Press F1 anywhere for context-sensitive help.\n\n" +
    "Visit our support portal for additional resources.";
helpTip.Body.Image = Properties.Resources.DocumentIcon;

// Footer with additional info
helpTip.Footer.Text = "Keyboard Shortcut: F1";

superToolTip1.SetToolTip(helpItem, helpTip);
```

### Tooltip with Images

Add visual icons to enhance tooltips:

```csharp
BarItem zoomItem = new BarItem() { Text = "Zoom In" };

ToolTipInfo zoomTip = new ToolTipInfo();
zoomTip.Header.Text = "Zoom In";
zoomTip.Header.Image = imageList1.Images["zoom"];  // 16x16 icon

zoomTip.Body.Text = "Increase document zoom level";
zoomTip.Body.Image = imageList1.Images["magnify"];  // 32x32 icon

superToolTip1.SetToolTip(zoomItem, zoomTip);
```

### Dynamic Tooltip Content

Update tooltip content based on application state:

```csharp
// Method to update save button tooltip
void UpdateSaveTooltip()
{
    BarItem saveItem = GetSaveItem();
    
    ToolTipInfo tooltip = new ToolTipInfo();
    tooltip.Header.Text = "Save Document";
    
    if (DocumentHasChanges)
    {
        tooltip.Body.Text = "Save changes to the current document";
        tooltip.Footer.Text = "Ctrl+S";
        saveItem.Enabled = true;
    }
    else
    {
        tooltip.Body.Text = "No changes to save";
        saveItem.Enabled = false;
    }
    
    superToolTip1.SetToolTip(saveItem, tooltip);
}
```

### Tooltip Display Timing

Control when tooltips appear:

```csharp
// Set tooltip display delay (milliseconds)
superToolTip1.ShowDuration = 5000;  // Show for 5 seconds
superToolTip1.InitialDelay = 1000;  // Wait 1 second before showing
superToolTip1.ReshowDelay = 500;    // Reshowing delay for multiple items
```

### Styling Tooltips

Configure appearance globally:

```csharp
// Create ToolTipInfoProvider for consistent styling
var toolTipProvider = new ToolTipInfoProvider();

// Set background and text colors
toolTipProvider.BackColor = Color.LightYellow;
toolTipProvider.ForeColor = Color.Black;

// Apply to multiple items
BarItem item1 = new BarItem() { Text = "Item 1" };
BarItem item2 = new BarItem() { Text = "Item 2" };

// Associate items with styled tooltips
```

---

## Best Practices

### 1. Clear, Concise Descriptions

```csharp
// Good: Clear action description
goodTip.Body.Text = "Save the current document to disk";

// Poor: Too vague
poorTip.Body.Text = "Click to do something";
```

### 2. Include Keyboard Shortcuts in Tooltips

```csharp
BarItem openItem = new BarItem() { Text = "&Open", Shortcut = Shortcut.CtrlO };

ToolTipInfo tip = new ToolTipInfo();
tip.Body.Text = "Open an existing document";
tip.Footer.Text = "Keyboard: Ctrl+O";

superToolTip1.SetToolTip(openItem, tip);
```

### 3. Use Icons for Quick Recognition

Incorporate related icons in tooltips to help users quickly identify menu items by visual memory.

```csharp
tip.Header.Image = Properties.Resources.SaveIcon;
tip.Body.Image = Properties.Resources.DocumentIcon;
```

### 4. Disable Default Tooltip for SuperTooltips

Prevent tooltip overlap:

```csharp
barItem.ShowTooltip = false;  // Disable default
superToolTip1.SetToolTip(barItem, richTip);  // Show rich tooltip only
```

### 5. Context-Aware Tooltips

Update tooltip content based on application state:

```csharp
BarItem pasteItem = new BarItem() { Text = "&Paste" };

pasteItem.ItemSelected += (sender, args) =>
{
    // Update tooltip based on clipboard content
    if (Clipboard.ContainsText())
    {
        ToolTipInfo tip = new ToolTipInfo();
        tip.Body.Text = "Paste: " + GetClipboardPreview();
        superToolTip1.SetToolTip(pasteItem, tip);
    }
};
```

### 6. Consistency Across Application

Establish consistent tooltip format:
- Always include keyboard shortcut if available
- Use similar icon styles
- Follow consistent header/body/footer structure
- Use same color scheme for related items

### 7. Test Tooltip Display

Verify tooltips display correctly on different screen resolutions and DPI settings during testing.
