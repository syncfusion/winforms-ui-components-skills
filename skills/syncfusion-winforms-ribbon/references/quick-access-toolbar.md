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

**Runtime Methods:**
- Right-click ribbon item → "Add to Quick Access Toolbar"
- Right-click ribbon → "Customize Quick Access Toolbar"
- QAT dropdown button → "More Commands"

**Via Code:**
```csharp
// Add button to QAT
ToolStripButton saveButton = new ToolStripButton {
    Text = "Save",
    Image = Image.FromFile("save.png")
};
ribbonControlAdv1.Header.AddQuickItem(new QuickButtonReflectable(saveButton));

// Add split button to QAT
ToolStripSplitButtonEx undoSplitButton = new ToolStripSplitButtonEx { Text = "Undo" };
ribbonControlAdv1.Header.AddQuickItem(new QuickSplitButtonReflectable(undoSplitButton));
```

**Adding ToolStripDropDownButton:**
```csharp
// Add dropdown button to QAT
ToolStripDropDownButton newDropDown = new ToolStripDropDownButton();
newDropDown.Text = "New";

ribbonControlAdv1.Header.AddQuickItem(
    new QuickDropDownButtonReflectable(newDropDown));
```



## Removing Items from QAT

**Runtime**: Right-click QAT item → "Remove from Quick Access Toolbar" or use Customize Window.

**Via Code**:
```csharp
ribbonControlAdv1.Header.QuickItems.RemoveAt(0);  // Remove first item
ribbonControlAdv1.Header.QuickItems.Clear();      // Remove all items
```

## Restricting Items

```csharp
// Prevent item from being added to QAT
ribbonControlAdv1.SetUseInCustomQuickAccessDialog(restrictedButton, false);
```

**Effect**: Item won't appear in Customize dialog, context menu won't show "Add to Quick Access Toolbar".

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

```csharp
// Set QAT-specific images (16x16 recommended for QAT)
QATImageProvider qatImageProvider = new QATImageProvider(ribbonControlAdv1);

ToolStripButton saveButton = new ToolStripButton {
    Text = "Save",
    Image = Image.FromFile("save32.png")  // 32x32 for ribbon
};

qatImageProvider.SetQATImage(saveButton, Image.FromFile("save16.png"));  // 16x16 for QAT
ribbonControlAdv1.Header.AddQuickItem(new QuickButtonReflectable(saveButton));
```

## Adding BackStage Items

```csharp
// Add backstage tab to QAT
BackStageTab printTab = new BackStageTab { Text = "Print" };
ribbonControlAdv1.Header.AddQuickItem(new BackStageTabReflectable(printTab));
```

**Runtime**: Customize Window → "Choose commands from" dropdown → Select "File" → Add BackStage items.

## Creating New QAT Items

```csharp
// Create button directly for QAT (not in ribbon)
ToolStripButton customQATButton = new ToolStripButton {
    Image = Image.FromFile("custom.png"),
    ToolTipText = "Custom Quick Action"
};
customQATButton.Click += (s, e) => PerformCustomAction();
ribbonControlAdv1.Header.AddQuickItem(customQATButton);
```

## QAT Location

```csharp
// Position QAT above (false, default) or below (true) ribbon
ribbonControlAdv1.ShowQuickPanelBelowRibbon = true;
```

**Runtime**: Right-click ribbon → "Show Quick Access Toolbar Below/Above the Ribbon".

## QAT Events

```csharp
// Before adding item (can cancel)
ribbonControlAdv1.Header.QuickItems.BeforeAddItem += (s, e) => {
    if (e.Item.Text == "Restricted") {
        e.Cancel = true;
        MessageBox.Show("This item cannot be added to QAT.");
    }
};

// Before removing item (can cancel)
ribbonControlAdv1.Header.QuickItems.BeforeRemoveItem += (s, e) => {
    if (e.Item.Text == "Save" && MessageBox.Show("Remove Save?", "Confirm", MessageBoxButtons.YesNo) == DialogResult.No)
        e.Cancel = true;
};

// After item added
ribbonControlAdv1.Header.QuickItemAdded += (s, e) => {
    Console.WriteLine($"{e.Item.Text} added to QAT");
    SaveQATConfiguration();
};

// Before/after dropdown popup
ribbonControlAdv1.BeforeCustomizeDropDownPopup += (s, e) => Console.WriteLine("QAT dropdown opening");
ribbonControlAdv1.AfterCustomizeDropDownPopup += (s, e) => Console.WriteLine("QAT dropdown opened");
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
