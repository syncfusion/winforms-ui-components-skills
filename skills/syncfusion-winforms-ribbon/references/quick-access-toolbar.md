# Quick Access Toolbar (QAT)

## Table of Contents
- [Overview](#overview)
- [Enabling QAT](#enabling-qat)
- [Adding Items to QAT](#adding-items-to-qat)
- [Removing Items from QAT](#removing-items-from-qat)
- [Restricting Items](#restricting-items)
- [Quick Access Menu](#quick-access-menu)
- [Custom Images for QAT Items](#custom-images-for-qat-items)
- [Adding BackStage Items](#adding-backstage-items)
- [Creating New QAT Items](#creating-new-qat-items)
- [QAT Location](#qat-location)
- [QAT Events](#qat-events)
- [Simplified Layout Support](#simplified-layout-support)

## Overview

The Quick Access Toolbar (QAT) is a customizable toolbar that provides one-click access to frequently used commands. It remains visible regardless of which ribbon tab is active, making it ideal for frequently used operations.

**Key Benefits:**
- Always visible above or below ribbon
- Quick access to most-used commands
- User-customizable at runtime
- Persists across ribbon state changes
- Works in both normal and simplified layouts

## Enabling QAT

### Show/Hide QAT

```csharp
// Show Quick Access Toolbar (default)
ribbonControlAdv1.QuickPanelVisible = true;

// Hide Quick Access Toolbar
ribbonControlAdv1.QuickPanelVisible = false;
```

### Show/Hide Quick Access Menu Button

```csharp
// Show quick access menu dropdown button (default)
ribbonControlAdv1.ShowQuickItemsDropDownButton = true;

// Hide quick access menu dropdown button
ribbonControlAdv1.ShowQuickItemsDropDownButton = false;
```

## Adding Items to QAT

### Method 1: Via Context Menu (Runtime)

**User Action:**
1. Right-click any ribbon item
2. Select "Add to Quick Access Toolbar"
3. Item appears in QAT

**This is the most user-friendly method** and should be enabled by default.

### Method 2: Via Customize Window

**User Action:**
1. Right-click on ribbon and select "Customize Quick Access Toolbar"
2. Or click QAT dropdown button → "More Commands"
3. Select items from left panel
4. Click "Add >>" to add to QAT
5. Click OK

**Opening Customize Window via Code:**
```csharp
// Open customize QAT dialog
// User triggers this via context menu
// No direct API to open programmatically
```

### Method 3: Via Code (Design Time)

Add different control types to QAT programmatically:

**Adding ToolStripButton:**
```csharp
// Create or reference existing button
ToolStripButton saveButton = new ToolStripButton();
saveButton.Text = "Save";
saveButton.Image = Image.FromFile("save.png");

// Add button to QAT using QuickButtonReflectable
ribbonControlAdv1.Header.AddQuickItem(new QuickButtonReflectable(saveButton));
```

**Adding ToolStripSplitButton:**
```csharp
// Add split button to QAT
ToolStripSplitButtonEx undoSplitButton = new ToolStripSplitButtonEx();
undoSplitButton.Text = "Undo";

ribbonControlAdv1.Header.AddQuickItem(
    new QuickSplitButtonReflectable(undoSplitButton));
```

**Adding ToolStripDropDownButton:**
```csharp
// Add dropdown button to QAT
ToolStripDropDownButton newDropDown = new ToolStripDropDownButton();
newDropDown.Text = "New";

ribbonControlAdv1.Header.AddQuickItem(
    new QuickDropDownButtonReflectable(newDropDown));
```

**Adding ToolStripRadioButton:**
```csharp
// Add radio button to QAT
ToolStripRadioButton radioButton = new ToolStripRadioButton();
radioButton.Text = "Option 1";

ribbonControlAdv1.Header.AddQuickItem(
    new QuickRadioButtonReflectable(radioButton));
```

**Adding ToolStripTextBox:**
```csharp
// Add text box to QAT
ToolStripTextBox searchBox = new ToolStripTextBox();
searchBox.Text = "Search";

ribbonControlAdv1.Header.AddQuickItem(
    new QuickTextboxReflectable(searchBox));
```

**Adding ToolStripProgressBar:**
```csharp
// Add progress bar to QAT
ToolStripProgressBar progressBar = new ToolStripProgressBar();
progressBar.Value = 50;

ribbonControlAdv1.Header.AddQuickItem(
    new ProgressbarReflectable(progressBar));
```

### Complete QAT Setup Example

```csharp
private void SetupQuickAccessToolbar()
{
    // Create commonly used buttons
    ToolStripButton saveButton = new ToolStripButton();
    saveButton.Text = "Save";
    saveButton.Image = Image.FromFile("save.png");
    saveButton.Click += (s, e) => SaveDocument();

    ToolStripButton undoButton = new ToolStripButton();
    undoButton.Text = "Undo";
    undoButton.Image = Image.FromFile("undo.png");
    undoButton.Click += (s, e) => Undo();

    ToolStripButton redoButton = new ToolStripButton();
    redoButton.Text = "Redo";
    redoButton.Image = Image.FromFile("redo.png");
    redoButton.Click += (s, e) => Redo();

    // Add to QAT
    ribbonControlAdv1.Header.AddQuickItem(new QuickButtonReflectable(saveButton));
    ribbonControlAdv1.Header.AddQuickItem(new QuickButtonReflectable(undoButton));
    ribbonControlAdv1.Header.AddQuickItem(new QuickButtonReflectable(redoButton));
}
```

## Removing Items from QAT

### Method 1: Via Context Menu (Runtime)

**User Action:**
1. Right-click QAT item
2. Select "Remove from Quick Access Toolbar"
3. Item is removed

### Method 2: Via Customize Window

**User Action:**
1. Open Customize Quick Access Toolbar dialog
2. Select item in right panel (QAT items)
3. Click "Remove"
4. Click OK

### Method 3: Via Code

```csharp
// Remove item at specific index
ribbonControlAdv1.Header.QuickItems.RemoveAt(0); // Remove first item

// Remove all items
ribbonControlAdv1.Header.QuickItems.Clear();

// Remove specific item by reference
ToolStripButton buttonToRemove = ...; // Get reference
int index = ribbonControlAdv1.Header.QuickItems.IndexOf(buttonToRemove);
if (index >= 0)
{
    ribbonControlAdv1.Header.QuickItems.RemoveAt(index);
}
```

## Restricting Items

### Prevent Item from Being Added to QAT

Some items should not be available in QAT (e.g., rarely used or contextual commands).

```csharp
// Restrict button from QAT
ToolStripButton restrictedButton = new ToolStripButton();
restrictedButton.Text = "Restricted";

// Set to false to prevent addition to QAT
ribbonControlAdv1.SetUseInCustomQuickAccessDialog(restrictedButton, false);

// Set to true to allow (default)
ribbonControlAdv1.SetUseInCustomQuickAccessDialog(allowedButton, true);
```

**Effect:**
- Item won't appear in Customize Quick Access Toolbar dialog
- Context menu won't show "Add to Quick Access Toolbar" option
- Cannot be added to QAT by any means

### Bulk Restriction

```csharp
private void RestrictMultipleItems(params ToolStripItem[] items)
{
    foreach (ToolStripItem item in items)
    {
        ribbonControlAdv1.SetUseInCustomQuickAccessDialog(item, false);
    }
}

// Usage
RestrictMultipleItems(advancedButton1, advancedButton2, contextualButton);
```

## Quick Access Menu

The Quick Access Menu is a dropdown that can contain additional quick commands.

### Adding Item to Quick Access Menu

**Via Designer:**
1. Select ribbon item
2. In Properties window, find **UseInQuickAccessMenu on ribbonControlAdv1**
3. Set to **true**
4. Item appears in QAT dropdown menu

**Via Code:**
```csharp
// Add item to quick access menu (dropdown)
// This makes item available in QAT dropdown, not directly in QAT
ToolStripButton menuButton = new ToolStripButton();
menuButton.Text = "Quick Menu Item";

// Set property via extended properties
ribbonControlAdv1.SetUseInQuickAccessMenu(menuButton, true);
```

**Difference between QAT and Quick Access Menu:**
- **QAT**: Items directly visible in toolbar
- **Quick Access Menu**: Items in dropdown menu (accessed via QAT dropdown button)

## Custom Images for QAT Items

QAT items can have different images than their ribbon counterparts for better visual clarity at small sizes.

### Using QATImageProvider

```csharp
// Create QAT image provider
QATImageProvider qatImageProvider = new QATImageProvider(ribbonControlAdv1);

// Create button with normal image
ToolStripButton saveButton = new ToolStripButton();
saveButton.Text = "Save";
saveButton.Image = Image.FromFile("save32.png"); // 32x32 for ribbon

// Set smaller QAT-specific image (16x16 recommended)
Image qatImage = Image.FromFile("save16.png");
qatImageProvider.SetQATImage(saveButton, qatImage);

// Add to QAT
ribbonControlAdv1.Header.AddQuickItem(new QuickButtonReflectable(saveButton));
```

### Complete QAT Image Example

```csharp
private void SetupQATWithCustomImages()
{
    // Create image provider
    QATImageProvider qatImageProvider = new QATImageProvider(ribbonControlAdv1);

    // Create buttons
    ToolStripButton saveButton = new ToolStripButton();
    saveButton.Text = "Save";
    saveButton.Image = Image.FromFile("save32.png");

    ToolStripButton printButton = new ToolStripButton();
    printButton.Text = "Print";
    printButton.Image = Image.FromFile("print32.png");

    // Set QAT-specific images (16x16)
    qatImageProvider.SetQATImage(saveButton, Image.FromFile("save16.png"));
    qatImageProvider.SetQATImage(printButton, Image.FromFile("print16.png"));

    // Add to QAT
    ribbonControlAdv1.Header.AddQuickItem(new QuickButtonReflectable(saveButton));
    ribbonControlAdv1.Header.AddQuickItem(new QuickButtonReflectable(printButton));
}
```

### Removing QAT Image

```csharp
// Remove custom QAT image (revert to default)
qatImageProvider.SetQATImage(saveButton, null);
```

## Adding BackStage Items

BackStage tabs and buttons can be added to QAT for quick access.

### Adding BackStage Tab to QAT

```csharp
// Create backstage tab
BackStageTab printTab = new BackStageTab();
printTab.Text = "Print";

// Add backstage tab to QAT
ribbonControlAdv1.Header.AddQuickItem(new BackStageTabReflectable(printTab));
```

### Adding via Customize Window

**User Action:**
1. Open Customize Quick Access Toolbar dialog
2. In "Choose commands from:" dropdown, select **File**
3. All BackStage items are listed
4. Select item and click "Add >>"
5. Click OK

**Effect:** Clicking QAT item opens BackStage to that tab/button.

## Creating New QAT Items

Create items specifically for QAT that don't exist in the ribbon.

### Creating New Button for QAT

```csharp
// Create new button (not in ribbon)
ToolStripButton customQATButton = new ToolStripButton();
customQATButton.Image = Image.FromFile("custom.png");
customQATButton.ToolTipText = "Custom Quick Action";
customQATButton.Click += (s, e) => PerformCustomAction();

// Add directly to QAT
ribbonControlAdv1.Header.AddQuickItem(customQATButton);
```

### Creating QAT Items via Customize Window

**User Action:**
1. Open Customize Quick Access Toolbar dialog
2. In "Choose commands from:" dropdown, select **QuickItems**
3. Select control type from second dropdown (Button, Split Button, etc.)
4. Configure properties in property grid below
5. Click "Add >>"
6. Click OK

**This allows runtime creation** of new QAT items without coding.

## QAT Location

### Positioning QAT

QAT can be positioned above or below the ribbon.

```csharp
// Show QAT above ribbon (default)
ribbonControlAdv1.ShowQuickPanelBelowRibbon = false;

// Show QAT below ribbon
ribbonControlAdv1.ShowQuickPanelBelowRibbon = true;
```

**User can also change location:**
- Right-click ribbon
- Select "Show Quick Access Toolbar Below the Ribbon" or "Show Quick Access Toolbar Above the Ribbon"

### Handling Location Changes

```csharp
// Monitor location changes (no direct event, use property)
bool isQATBelowRibbon = ribbonControlAdv1.ShowQuickPanelBelowRibbon;

if (isQATBelowRibbon)
{
    // QAT is below ribbon
}
else
{
    // QAT is above ribbon
}
```

## QAT Events

### BeforeAddItem Event

Fires before an item is added to QAT.

```csharp
// Subscribe to event
ribbonControlAdv1.Header.QuickItems.BeforeAddItem += QuickItems_BeforeAddItem;

private void QuickItems_BeforeAddItem(object sender, RibbonItemsEventArgs e)
{
    // Get item being added
    ToolStripItem item = e.Item;
    
    // Log action
    Console.WriteLine($"Adding {item.Text} to QAT");
    
    // Cancel addition if needed
    if (item.Text == "Restricted")
    {
        e.Cancel = true;
        MessageBox.Show("This item cannot be added to QAT.");
    }
}
```

### BeforeRemoveItem Event

Fires before an item is removed from QAT.

```csharp
// Subscribe to event
ribbonControlAdv1.Header.QuickItems.BeforeRemoveItem += QuickItems_BeforeRemoveItem;

private void QuickItems_BeforeRemoveItem(object sender, RibbonItemsEventArgs e)
{
    // Get item being removed
    ToolStripItem item = e.Item;
    
    // Confirm removal for important items
    if (item.Text == "Save")
    {
        DialogResult result = MessageBox.Show(
            "Remove Save button from QAT?",
            "Confirm",
            MessageBoxButtons.YesNo);
            
        if (result == DialogResult.No)
        {
            e.Cancel = true; // Prevent removal
        }
    }
}
```

### QuickItemAdded Event

Fires after an item is added to QAT.

```csharp
// Subscribe to event
ribbonControlAdv1.Header.QuickItemAdded += Header_QuickItemAdded;

private void Header_QuickItemAdded(object sender, ToolStripItemEventArgs e)
{
    // Item successfully added
    ToolStripItem addedItem = e.Item;
    
    Console.WriteLine($"Item {addedItem.Text} was added to QAT");
    
    // Save to settings
    SaveQATConfiguration();
}
```

### BeforeCustomizeDropDownPopup Event

Fires before the QAT dropdown menu is shown.

```csharp
ribbonControlAdv1.BeforeCustomizeDropDownPopup += RibbonControlAdv1_BeforeCustomizeDropDownPopup;

private void RibbonControlAdv1_BeforeCustomizeDropDownPopup(object sender, DropDownEventArgs e)
{
    // Modify dropdown before showing
    Console.WriteLine("QAT dropdown opening");
    
    // Can cancel popup if needed
    // e.Cancel = true;
}
```

### AfterCustomizeDropDownPopup Event

Fires after the QAT dropdown menu is shown.

```csharp
ribbonControlAdv1.AfterCustomizeDropDownPopup += RibbonControlAdv1_AfterCustomizeDropDownPopup;

private void RibbonControlAdv1_AfterCustomizeDropDownPopup(object sender, EventArgs e)
{
    Console.WriteLine("QAT dropdown opened");
}
```

## Simplified Layout Support

QAT works seamlessly with both normal and simplified layouts.

### Cross-Layout Visibility

Items added to QAT in either layout remain visible when switching:

```csharp
// Enable simplified layout switching
ribbonControlAdv1.EnableSimplifiedLayoutMode = true;

// Items added to QAT in normal layout
ribbonControlAdv1.LayoutMode = RibbonLayoutMode.Normal;
ribbonControlAdv1.Header.AddQuickItem(new QuickButtonReflectable(saveButton));

// Switch to simplified - QAT item still visible
ribbonControlAdv1.LayoutMode = RibbonLayoutMode.Simplified;
// saveButton still in QAT
```

**Key Point:** QAT persists across layout modes, providing consistent quick access.

## Best Practices

1. **Pre-populate common commands:** Add 3-5 most frequently used commands to QAT by default

2. **Enable user customization:** Allow users to add/remove items via context menu

3. **Use appropriate images:** Provide 16x16 QAT-specific images for clarity

4. **Restrict appropriately:** Only restrict truly inappropriate items from QAT

5. **Save preferences:** Persist QAT configuration to user settings

6. **Position wisely:** Consider above-ribbon for most users (default)

7. **Handle events:** Use events to validate additions/removals

8. **Limit initial items:** Don't overcrowd default QAT (3-5 items maximum)

9. **Provide tooltips:** Ensure QAT items have clear ToolTipText

10. **Test both layouts:** Verify QAT works in normal and simplified modes

## Troubleshooting

### Issue: Context Menu Doesn't Show "Add to QAT"

**Cause:** Item restricted via `SetUseInCustomQuickAccessDialog`.

**Solution:**
```csharp
// Enable item for QAT
ribbonControlAdv1.SetUseInCustomQuickAccessDialog(item, true);
```

### Issue: QAT Item Shows Wrong Image

**Cause:** Need custom QAT image.

**Solution:** Use QATImageProvider:
```csharp
QATImageProvider provider = new QATImageProvider(ribbonControlAdv1);
provider.SetQATImage(button, smallImage);
```

### Issue: QAT Items Not Persisting

**Cause:** Configuration not saved.

**Solution:** Save/load QAT state (see serialization support in advanced-features.md).

## Related Topics

- **Customization** - Learn about runtime customization dialogs
- **Ribbon States** - QAT visibility during different ribbon states
- **Simplified Layout** - QAT behavior in simplified layout mode
