# Caption Buttons and Customization

The DockingManager provides default caption buttons (Close, Pin, Menu, Maximize) and supports adding custom buttons. This guide covers button visibility, customization, and event handling.

## Table of Contents
- [Default Caption Buttons](#default-caption-buttons)
- [Show/Hide Caption Buttons](#showhide-caption-buttons)
- [Custom Caption Buttons](#custom-caption-buttons)
- [Button Click Handling](#button-click-handling)
- [Button Appearance Customization](#button-appearance-customization)
- [Button Tooltips](#button-tooltips)

## Default Caption Buttons

DockingManager provides four default caption buttons:

| Button | Function | Default State |
|--------|----------|---------------|
| **Close** | Close/hide the dock window | Visible |
| **Pin (Auto-Hide)** | Toggle auto-hide mode | Visible |
| **Menu** | Display context menu | Visible |
| **Maximize** | Maximize/restore window | Hidden (enable via property) |

## Show/Hide Caption Buttons

### Hide Close Button

```csharp
// Hide close button for specific control
this.dockingManager1.SetCloseButtonVisibility(panel1, false);

// Show close button again
this.dockingManager1.SetCloseButtonVisibility(panel1, true);

// Check visibility
bool isVisible = this.dockingManager1.GetCloseButtonVisibility(panel1);
```

**VB.NET:**

```vb
' Hide close button
Me.dockingManager1.SetCloseButtonVisibility(panel1, False)

' Show close button
Me.dockingManager1.SetCloseButtonVisibility(panel1, True)

' Check visibility
Dim isVisible As Boolean = Me.dockingManager1.GetCloseButtonVisibility(panel1)
```

### Hide Pin (Auto-Hide) Button

```csharp
// Hide auto-hide button
this.dockingManager1.SetAutoHideButtonVisibility(panel1, false);

// Show auto-hide button
this.dockingManager1.SetAutoHideButtonVisibility(panel1, true);

// Check visibility
bool isPinVisible = this.dockingManager1.GetAutoHideButtonVisibility(panel1);
```

### Hide Menu Button

```csharp
// Hide menu button
this.dockingManager1.SetMenuButtonVisibility(panel1, false);

// Show menu button
this.dockingManager1.SetMenuButtonVisibility(panel1, true);

// Check visibility
bool isMenuVisible = this.dockingManager1.GetMenuButtonVisibility(panel1);
```

### Enable Maximize Button

```csharp
// Enable maximize/restore functionality
this.dockingManager1.MaximizeButtonEnabled = true;
```

Note: Maximize button appears only when another control is docked below the window.

### Hide All Caption Buttons

```csharp
// Hide all buttons for a specific control
this.dockingManager1.SetCloseButtonVisibility(panel1, false);
this.dockingManager1.SetAutoHideButtonVisibility(panel1, false);
this.dockingManager1.SetMenuButtonVisibility(panel1, false);
```

### Remove Caption Button Globally

Remove a button type from all dock windows:

```csharp
// Remove close button from all dock windows
for (int i = 0; i < this.dockingManager1.CaptionButtons.Count; i++)
{
    if (this.dockingManager1.CaptionButtons[i].Name == "CloseButton")
    {
        this.dockingManager1.CaptionButtons.RemoveAt(i);
        break;
    }
}
```

**Button Names:**
- `"CloseButton"` - Close button
- `"PinButton"` - Auto-hide (pin) button
- `"MenuButton"` - Context menu button
- `"MaximizeButton"` - Maximize button
- `"RestoreButton"` - Restore button

## Custom Caption Buttons

Add custom buttons to dock window captions.

### Add Custom Button Programmatically

```csharp
using Syncfusion.Windows.Forms.Tools;

// Create ImageList for button icons
ImageList imageList = new ImageList();
imageList.Images.Add(Image.FromFile(@"C:\Icons\custom.png"));
imageList.Images.Add(Image.FromFile(@"C:\Icons\save.png"));

// Assign ImageList to DockingManager
this.dockingManager1.ImageList = imageList;

// Create custom button
CaptionButton customButton = new CaptionButton();
customButton.Name = "CustomButton";
customButton.ImageIndex = 0; // Index in ImageList
customButton.Type = CaptionButtonType.Custom;
customButton.TransparentImageColor = Color.Transparent;

// Add button to collection
this.dockingManager1.CaptionButtons.Add(customButton);

// Create save button
CaptionButton saveButton = new CaptionButton();
saveButton.Name = "SaveButton";
saveButton.ImageIndex = 1;
saveButton.Type = CaptionButtonType.Custom;
this.dockingManager1.CaptionButtons.Add(saveButton);
```

**VB.NET:**

```vb
' Create ImageList
Dim imageList As New ImageList()
imageList.Images.Add(Image.FromFile("C:\Icons\custom.png"))

' Assign to DockingManager
Me.dockingManager1.ImageList = imageList

' Create custom button
Dim customButton As New CaptionButton()
customButton.Name = "CustomButton"
customButton.ImageIndex = 0
customButton.Type = CaptionButtonType.Custom
customButton.TransparentImageColor = Color.Transparent

' Add button
Me.dockingManager1.CaptionButtons.Add(customButton)
```

### Add Custom Button via Designer

1. Select DockingManager in designer
2. Find `CaptionButtons` property in Properties window
3. Click the ellipsis (...) button to open Collection Editor
4. Click "Add" to create a new button
5. Set properties:
   - `Name` - Unique identifier
   - `ImageIndex` - Icon index from ImageList
   - `Type` - Set to `Custom`
   - `SuperToolTipInfo` - Tooltip information

### Show Custom Buttons in Floating Windows

```csharp
// Display custom buttons when window is floating
this.dockingManager1.ShowCustomButtonsInFloating = true;
```

Note: Not applicable for VS2005 visual style.

## Button Click Handling

### Handle Custom Button Click

Custom buttons don't have built-in click events. Handle clicks using caption button click detection:

```csharp
// Method 1: Using DockContextMenu event (for menu button)
this.dockingManager1.DockContextMenu += (s, e) =>
{
    // Detect which button area was clicked
    Console.WriteLine($"Context menu for: {e.Owner.Name}");
};

// Method 2: Track mouse clicks on caption
// This requires accessing the internal caption host
// Complex implementation - better to use context menu
```

For reliable custom button handling, consider these approaches:

**Approach 1: Use Context Menu Items**

```csharp
this.dockingManager1.DockContextMenu += (s, e) =>
{
    // Add custom menu item
    BarItem customItem = new BarItem();
    customItem.Text = "Custom Action";
    customItem.Click += (sender, args) =>
    {
        MessageBox.Show($"Custom action for {e.Owner.Name}");
    };
    
    e.ContextMenu.ParentBarItem.Items.Add(customItem);
};
```

**Approach 2: Overlay Buttons on Panel**

```csharp
// Add clickable controls inside the dock panel
Button actionButton = new Button();
actionButton.Text = "Action";
actionButton.Dock = DockStyle.Top;
actionButton.Click += (s, e) => MessageBox.Show("Action clicked");
panel1.Controls.Add(actionButton);
```

## Button Appearance Customization

### Button Foreground Colors

```csharp
// Active window button color
this.dockingManager1.ActiveCaptionButtonForeColor = Color.Red;

// Inactive window button color
this.dockingManager1.InActiveCaptionButtonForeColor = Color.Gray;
```

**VB.NET:**

```vb
' Active window button color
Me.dockingManager1.ActiveCaptionButtonForeColor = Color.Red

' Inactive window button color
Me.dockingManager1.InActiveCaptionButtonForeColor = Color.Gray
```

### Caption Height

```csharp
// Increase caption height to accommodate larger buttons
this.dockingManager1.CaptionHeight = 30; // Default is about 22

// Maximum value is 60
this.dockingManager1.CaptionHeight = 60;
```

Note: Not applicable for Default and VS2005 visual styles.

## Button Tooltips

### Set Standard Tooltips

```csharp
// Enable tooltips
this.dockingManager1.ShowToolTips = true;

// Set close button tooltip
this.dockingManager1.SetCloseButtonToolTip("Close Window");

// Set auto-hide button tooltip
this.dockingManager1.SetAutoHideButtonToolTip("Pin Window");

// Set menu button tooltip
this.dockingManager1.SetMenuButtonToolTip("Window Options");
```

### Get Current Tooltips

```csharp
string closeTooltip = this.dockingManager1.GetCloseButtonToolTip();
string pinTooltip = this.dockingManager1.GetAutoHideButtonToolTip();
string menuTooltip = this.dockingManager1.GetMenuButtonToolTip();
```

### Configure Tooltip Behavior

```csharp
// Set tooltip display interval (milliseconds)
this.dockingManager1.ToolTipInterval = 2000; // Default is 5000

// Use balloon style tooltips
this.dockingManager1.UseBalloonStyleToolTip = true;

// Disable tooltips
this.dockingManager1.ShowToolTips = false;
```

### SuperTooltip Support

Use rich SuperTooltips with images and formatting:

```csharp
using Syncfusion.Windows.Forms.Tools;

// Create SuperToolTip control
SuperToolTip superToolTip = new SuperToolTip(this);

// Enable SuperTooltip for DockingManager
this.dockingManager1.EnableSuperToolTip = true;
this.dockingManager1.SuperToolTip = superToolTip;
this.dockingManager1.ShowToolTips = true;

// Configure button with SuperTooltip via CaptionButtons collection
CaptionButton closeBtn = this.dockingManager1.CaptionButtons
    .Cast<CaptionButton>()
    .FirstOrDefault(b => b.Name == "CloseButton");

if (closeBtn != null)
{
    ToolTipInfo toolTipInfo = new ToolTipInfo();
    toolTipInfo.Header.Text = "Close Window";
    toolTipInfo.Body.Text = "Closes this docking window";
    toolTipInfo.Footer.Text = "Click to close";
    closeBtn.SuperToolTipInfo = toolTipInfo;
}
```

## Complete Example

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Windows.Forms.Tools.XPMenus;

public class CaptionButtonExample : Form
{
    private DockingManager dockingManager1;
    private Panel panel1, panel2, panel3;
    
    public CaptionButtonExample()
    {
        InitializeComponent();
        SetupDocking();
        CustomizeCaptionButtons();
    }
    
    private void SetupDocking()
    {
        // Create DockingManager
        this.dockingManager1 = new DockingManager(this.components);
        this.dockingManager1.HostControl = this;
        
        // Create panels
        panel1 = new Panel { BackColor = Color.LightBlue };
        panel2 = new Panel { BackColor = Color.LightGreen };
        panel3 = new Panel { BackColor = Color.LightYellow };
        
        this.Controls.AddRange(new Control[] { panel1, panel2, panel3 });
        
        // Enable docking
        this.dockingManager1.SetEnableDocking(panel1, true);
        this.dockingManager1.SetEnableDocking(panel2, true);
        this.dockingManager1.SetEnableDocking(panel3, true);
        
        // Set labels
        this.dockingManager1.SetDockLabel(panel1, "Locked Window");
        this.dockingManager1.SetDockLabel(panel2, "Custom Actions");
        this.dockingManager1.SetDockLabel(panel3, "Standard Window");
        
        // Arrange
        this.dockingManager1.DockControl(panel1, this, DockingStyle.Left, 200);
        this.dockingManager1.DockControl(panel2, this, DockingStyle.Right, 200);
        this.dockingManager1.DockControl(panel3, this, DockingStyle.Bottom, 150);
    }
    
    private void CustomizeCaptionButtons()
    {
        // Customize button colors
        this.dockingManager1.ActiveCaptionButtonForeColor = Color.DarkBlue;
        this.dockingManager1.InActiveCaptionButtonForeColor = Color.Gray;
        
        // Increase caption height
        this.dockingManager1.CaptionHeight = 30;
        
        // Configure tooltips
        this.dockingManager1.ShowToolTips = true;
        this.dockingManager1.ToolTipInterval = 1000;
        this.dockingManager1.SetCloseButtonToolTip("Close this window");
        this.dockingManager1.SetAutoHideButtonToolTip("Auto-hide this window");
        
        // Hide close button for panel1 (locked window)
        this.dockingManager1.SetCloseButtonVisibility(panel1, false);
        this.dockingManager1.SetAutoHideButtonVisibility(panel1, false);
        
        // Add custom buttons
        AddCustomButtons();
        
        // Enable maximize button
        this.dockingManager1.MaximizeButtonEnabled = true;
        
        // Apply visual style
        this.dockingManager1.VisualStyle = VisualStyle.Office2016Colorful;
    }
    
    private void AddCustomButtons()
    {
        // Create ImageList
        ImageList imageList = new ImageList();
        imageList.Images.Add(Properties.Resources.SaveIcon);
        imageList.Images.Add(Properties.Resources.RefreshIcon);
        
        this.dockingManager1.ImageList = imageList;
        
        // Add Save button
        CaptionButton saveBtn = new CaptionButton
        {
            Name = "SaveButton",
            ImageIndex = 0,
            Type = CaptionButtonType.Custom,
            TransparentImageColor = Color.Transparent
        };
        this.dockingManager1.CaptionButtons.Add(saveBtn);
        
        // Add Refresh button
        CaptionButton refreshBtn = new CaptionButton
        {
            Name = "RefreshButton",
            ImageIndex = 1,
            Type = CaptionButtonType.Custom,
            TransparentImageColor = Color.Transparent
        };
        this.dockingManager1.CaptionButtons.Add(refreshBtn);
        
        // Show custom buttons in floating state
        this.dockingManager1.ShowCustomButtonsInFloating = true;
        
        // Handle button clicks via context menu
        this.dockingManager1.DockContextMenu += DockingManager1_DockContextMenu;
    }
    
    private void DockingManager1_DockContextMenu(object sender, 
        DockContextMenuEventArgs e)
    {
        // Add custom menu items for custom button functionality
        BarItem saveItem = new BarItem { Text = "Save Content" };
        saveItem.Click += (s, args) => 
            MessageBox.Show($"Saving {e.Owner.Name}");
        
        BarItem refreshItem = new BarItem { Text = "Refresh Content" };
        refreshItem.Click += (s, args) => 
            MessageBox.Show($"Refreshing {e.Owner.Name}");
        
        e.ContextMenu.ParentBarItem.Items.Insert(0, saveItem);
        e.ContextMenu.ParentBarItem.Items.Insert(1, refreshItem);
    }
}
```

## Best Practices

1. **Hide unnecessary buttons** - Reduce clutter by hiding buttons users don't need
2. **Use meaningful tooltips** - Help users understand button functions
3. **Consider button colors** - Ensure buttons are visible with your theme
4. **Test with floating windows** - Verify custom buttons appear when floating
5. **Provide alternative actions** - Use context menu for complex operations
6. **Keep captions readable** - Don't add too many custom buttons
7. **Use standard buttons** - Leverage built-in buttons before adding custom ones

## Troubleshooting

**Custom buttons don't appear:**
- Verify `ImageList` is assigned to DockingManager
- Check `ImageIndex` is valid for the ImageList
- Ensure button `Type` is set to `CaptionButtonType.Custom`
- Verify the button was added to `CaptionButtons` collection

**Buttons don't show when floating:**
- Set `ShowCustomButtonsInFloating` to `true`
- Not supported in VS2005 visual style - use different style

**Cannot click custom buttons:**
- Custom buttons don't have built-in click events
- Use context menu items for actions
- Consider adding controls inside the dock panel instead

**Tooltips don't appear:**
- Set `ShowToolTips` to `true`
- Disable `EnableSuperToolTip` if using standard tooltips
- Check `ToolTipInterval` isn't too high
