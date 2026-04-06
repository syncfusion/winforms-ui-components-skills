# Ribbon Controls

## Table of Contents
- [Overview](#overview)
- [ToolStripButton](#toolstripbutton)
- [ToolStripRadioButton](#toolstripradiobutton)
- [ToolStripDropDownButton](#toolstripdropdownbutton)
- [ToolStripSplitButton and ToolStripSplitButtonEx](#toolstripsplitbutton-and-toolstripsplitbuttonex)
- [ToolStripComboBoxEx](#toolstripcomboboxex)
- [ToolStripGallery](#toolstripgallery)
- [ToolStripCheckBox](#toolstripcheckbox)
- [ToolStripTextBox](#toolstriptextbox)
- [ToolStripProgressBar](#toolstripprogressbar)
- [ToolStripLabel](#toolstriplabel)
- [ToolStripSeparator](#toolstripseparator)
- [ToolStripPanelItem](#toolstrippanelitem)
- [Common Properties](#common-properties)
- [Simplified Layout Support](#simplified-layout-support)

## Overview

The RibbonControlAdv supports a rich collection of control types that can be added to ToolStripEx groups. Each control type serves specific purposes and can be configured for different display modes, layouts, and user interactions.

**Key Concepts:**
- Controls are added to **ToolStripEx** (groups)
- Controls can be added via designer or code
- Most controls support both normal and simplified layouts
- Controls inherit standard ToolStrip properties plus ribbon-specific enhancements

## ToolStripButton

Standard clickable button for executing commands.

```csharp
// Create button
ToolStripButton saveButton = new ToolStripButton {
    Text = "Save",  // Use \r\n for multiline: "New\r\nMail"
    Image = Image.FromFile("save.png"),
    DisplayStyle = ToolStripItemDisplayStyle.ImageAndText,
    TextImageRelation = TextImageRelation.ImageAboveText
};
saveButton.Click += (s, e) => SaveDocument();
toolStripEx1.Items.Add(saveButton);

// Toggle behavior
boldButton.CheckOnClick = true;
boldButton.CheckedChanged += (s, e) => { if (boldButton.Checked) ApplyBold(); else RemoveBold(); };
```

**Designer**: Select group → Click dropdown → Select Button → Configure properties.

## ToolStripRadioButton

Radio button for mutually exclusive selection within a group.

```csharp
// Create radio buttons
ToolStripRadioButton readRadio = new ToolStripRadioButton();
readRadio.Text = "Read";

ToolStripRadioButton unreadRadio = new ToolStripRadioButton();
unreadRadio.Text = "Unread";

// Use ToolStripPanelItem to group radio buttons
ToolStripPanelItem radioPanel = new ToolStripPanelItem();
radioPanel.Items.AddRange(new ToolStripItem[] { readRadio, unreadRadio });

toolStripEx1.Items.Add(radioPanel);

// Handle events
readRadio.CheckedChanged += (s, e) =>
{
    if (readRadio.Checked) FilterReadMessages();
};
```

## ToolStripDropDownButton

```csharp
// Create dropdown button with menu items
ToolStripDropDownButton newItemsButton = new ToolStripDropDownButton {
    Text = "New Items",
    Image = Image.FromFile("newitems.png")
};

newItemsButton.DropDownItems.AddRange(new ToolStripItem[] {
    new ToolStripMenuItem("E-mail Message", null, (s, e) => CreateNewEmail()),
    new ToolStripMenuItem("Appointment", null, (s, e) => CreateNewAppointment()),
    new ToolStripMenuItem("Contact", null, (s, e) => CreateNewContact())
});

toolStripEx1.Items.Add(newItemsButton);
```

## ToolStripSplitButton and ToolStripSplitButtonEx

```csharp
// Create split button (button + dropdown)
ToolStripSplitButtonEx undoButton = new ToolStripSplitButtonEx {
    Text = "Undo",
    Image = Image.FromFile("undo.png")
};

undoButton.DropDownItems.AddRange(new object[] { "Undo Typing", "Undo Bold" });
undoButton.ButtonClick += (s, e) => UndoLastAction();  // Main action
undoButton.DropDownItemClicked += (s, e) => UndoSpecificAction(e.ClickedItem.Text);

toolStripEx1.Items.Add(undoButton);
```

## ToolStripComboBoxEx

```csharp
ToolStripComboBoxEx fontComboBox = new ToolStripComboBoxEx {
    DropDownStyle = ComboBoxStyle.DropDownList  // DropDown (editable) or DropDownList (non-editable)
};
fontComboBox.Items.AddRange(new object[] { "Arial", "Calibri", "Times New Roman", "Verdana" });
fontComboBox.SelectedIndex = 1;
fontComboBox.SelectedIndexChanged += (s, e) => ApplyFont(fontComboBox.SelectedItem.ToString());
toolStripEx1.Items.Add(fontComboBox);
```

## ToolStripGallery

```csharp
// Create gallery with items
ToolStripGallery quickStepsGallery = new ToolStripGallery {
    ItemSize = new Size(100, 40),
    ColumnCount = 3,
    Height = 120,
    ScrollerType = ScrollerType.StandardScroller
};

quickStepsGallery.Items.AddRange(new ToolStripGalleryItem[] {
    new ToolStripGalleryItem { Text = "Move to ?", Image = Image.FromFile("moveto.png") },
    new ToolStripGalleryItem { Text = "To Manager", Image = Image.FromFile("tomanager.png") },
    new ToolStripGalleryItem { Text = "Team Email", Image = Image.FromFile("teamemail.png") }
});

quickStepsGallery.ItemClick += (s, e) => ExecuteQuickStep(((ToolStripGalleryItem)e.Item).Text);
toolStripEx1.Items.Add(quickStepsGallery);
```

## ToolStripCheckBox

Checkbox control for boolean options. Can be added via designer or code.

```csharp
// Checked: Boolean state, ThreeState: Enable indeterminate, CheckState: Checked/Unchecked/Indeterminate
ToolStripCheckBox showConversationsCheckBox = new ToolStripCheckBox {
    Text = "Show As Conversations",
    Checked = true
};
showConversationsCheckBox.CheckedChanged += (s, e) => 
    if (showConversationsCheckBox.Checked) EnableConversationView(); else DisableConversationView();
toolStripEx1.Items.Add(showConversationsCheckBox);
```

## ToolStripTextBox

Text input field in the ribbon. Can be added via designer or code.

```csharp
// Text: Current value, Multiline: Enable multiple lines, MaxLength: Character limit, ReadOnly: Prevent editing
ToolStripTextBox searchTextBox = new ToolStripTextBox {
    Text = "Search...",
    Size = new Size(200, 25)
};
searchTextBox.TextChanged += (s, e) => PerformSearch(searchTextBox.Text);
toolStripEx1.Items.Add(new ToolStripPanelItem { Items = { searchTextBox } });
```

## ToolStripProgressBar

Progress indicator for long-running operations. Can be added via designer or code.

```csharp
// Minimum/Maximum: Value range (0-100), Value: Current progress, Style: Continuous/Marquee/Blocks
ToolStripProgressBar uploadProgressBar = new ToolStripProgressBar {
    Minimum = 0, Maximum = 100, Value = 0, Size = new Size(150, 20)
};
toolStripEx1.Items.Add(new ToolStripPanelItem { 
    Items = { new ToolStripLabel("Uploading:"), uploadProgressBar }
});
```

## ToolStripLabel

Static text label. Can be added via designer or code.

```csharp
// Text: Label text, Image: Icon, ImageAlign/TextAlign: Alignment, Font/ForeColor: Styling
toolStripEx1.Items.Add(new ToolStripLabel { 
    Text = "Ready", Font = new Font("Segoe UI", 9, FontStyle.Bold), ForeColor = Color.Blue 
});
```

## ToolStripSeparator

Visual separator between items. Can be added via designer or code.

```csharp
toolStripEx1.Items.Add(cutButton);
toolStripEx1.Items.Add(copyButton);
toolStripEx1.Items.Add(new ToolStripSeparator());
toolStripEx1.Items.Add(formatButton);
```

## ToolStripPanelItem

Container for arranging items in multiple rows. Set `RowCount` to organize items. Can be added via designer or code.

```csharp
toolStripEx1.Items.Add(new ToolStripPanelItem {
    RowCount = 2,
    Items = { 
        new ToolStripButton("Button 1"), new ToolStripButton("Button 2"),
        new ToolStripButton("Button 3"), new ToolStripButton("Button 4")
    }
});
```

## Common Properties

All ribbon controls inherit from ToolStripItem:

| Category | Properties |
|----------|-----------|
| **Display** | Text, Image, DisplayStyle, TextImageRelation, Size, AutoSize |
| **Alignment** | Alignment, TextAlign, ImageAlign, Margin, Padding |
| **Behavior** | Enabled, Visible, ToolTipText, Tag |
| **RTL Support** | RightToLeft, RightToLeftAutoMirrorImage |

## Simplified Layout Support

Configure controls for simplified layout using `RibbonItemDisplayMode`. Provide 20x20 medium-size images via `ToolStripExImageProvider`.

```csharp
// Set display modes: Simplified, Normal, or both (Normal | Simplified)
ribbonControlAdv1.SetDisplayMode(pasteButton, RibbonItemDisplayMode.Simplified);
ribbonControlAdv1.SetDisplayMode(saveButton, RibbonItemDisplayMode.Normal | RibbonItemDisplayMode.Simplified);

// Configure medium images for simplified layout
ToolStripExImageProvider imageProvider = new ToolStripExImageProvider(toolStripEx1);
imageProvider.MediumImageList = new ImageListAdv { Images = { Image.FromFile("cut20.png") } };
imageProvider.SetMediumItemImage(cutButton, 0);
```

## Best Practices

- Choose appropriate control types matching user intent
- Set `ToolTipText` for all controls, especially image-only buttons
- Maintain size consistency for similar controls
- Provide multiple image sizes (32x32, 20x20, 16x16) for different layouts
- Use ToolStripPanelItem to organize related controls
- Set Text property even for image-only displays (accessibility)
- Prefer disabling controls over hiding them for layout stability
- Test proper collapse behavior during window resize
