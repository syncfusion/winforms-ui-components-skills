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

### Adding ToolStripButton

**Via Designer:**
1. Select a ToolStripEx group
2. Click the dropdown button inside the group
3. Select **Button** from the controls grid
4. Configure properties in Properties window

**Via Code:**

```csharp
// Create button
ToolStripButton saveButton = new ToolStripButton();
saveButton.Text = "Save";
saveButton.Image = Image.FromFile("save.png");
saveButton.DisplayStyle = ToolStripItemDisplayStyle.ImageAndText;
saveButton.TextImageRelation = TextImageRelation.ImageAboveText;

// Handle click event
saveButton.Click += (s, e) => SaveDocument();

// Add to group
toolStripEx1.Items.Add(saveButton);
```

### Multiline Text in Buttons

```csharp
// Use \r\n for line breaks
button.Text = "New\r\nMail";

// Or use string concatenation
button.Text = "Reply" + Environment.NewLine + "All";
```

### CheckOnClick Property

Hold button selection state after click (toggle functionality):

```csharp
// Enable toggle behavior
boldButton.CheckOnClick = true;

// Check if button is checked
if (boldButton.Checked)
{
    // Apply bold formatting
}

// Handle CheckedChanged event
boldButton.CheckedChanged += (s, e) =>
{
    if (boldButton.Checked)
        ApplyBold();
    else
        RemoveBold();
};
```

### ToolStripButton in Simplified Layout

```csharp
// Button works in simplified layout by default
ToolStripButton cutButton = new ToolStripButton();
cutButton.Text = "Cut";

// Add medium-size image (20x20) for simplified layout
ImageListAdv mediumImages = new ImageListAdv();
mediumImages.Images.Add(Image.FromFile("cut20.png"));

ToolStripExImageProvider imageProvider = new ToolStripExImageProvider(toolStripEx1);
imageProvider.MediumImageList = mediumImages;
imageProvider.SetMediumItemImage(cutButton, 0);

toolStripEx1.Items.Add(cutButton);
```

## ToolStripRadioButton

Radio button for mutually exclusive selection within a group.

### Adding ToolStripRadioButton

**Via Code:**

```csharp
// Create radio buttons
ToolStripRadioButton readRadio = new ToolStripRadioButton();
readRadio.Text = "Read";

ToolStripRadioButton unreadRadio = new ToolStripRadioButton();
unreadRadio.Text = "Unread";

// Use ToolStripPanelItem to group radio buttons
ToolStripPanelItem radioPanel = new ToolStripPanelItem();
radioPanel.Items.AddRange(new ToolStripItem[] { readRadio, unreadRadio });

// Add panel to group
toolStripEx1.Items.Add(radioPanel);
```

### Radio Button Events

```csharp
readRadio.CheckedChanged += (s, e) =>
{
    if (readRadio.Checked)
    {
        FilterReadMessages();
    }
};

unreadRadio.CheckedChanged += (s, e) =>
{
    if (unreadRadio.Checked)
    {
        FilterUnreadMessages();
    }
};
```

### ToolStripRadioButton in Simplified Layout

```csharp
ToolStripRadioButton radioButton = new ToolStripRadioButton();
radioButton.Text = "Custom Option";
toolStripEx1.Items.Add(radioButton);
```

## ToolStripDropDownButton

Dropdown button that displays a menu when clicked.

### Adding ToolStripDropDownButton

**Via Code:**

```csharp
// Create dropdown button
ToolStripDropDownButton newItemsButton = new ToolStripDropDownButton();
newItemsButton.Text = "New Items";
newItemsButton.Image = Image.FromFile("newitems.png");

// Create menu items
ToolStripMenuItem emailMenuItem = new ToolStripMenuItem();
emailMenuItem.Text = "E-mail Message";
emailMenuItem.Click += (s, e) => CreateNewEmail();

ToolStripMenuItem appointmentMenuItem = new ToolStripMenuItem();
appointmentMenuItem.Text = "Appointment";
appointmentMenuItem.Click += (s, e) => CreateNewAppointment();

ToolStripMenuItem meetingMenuItem = new ToolStripMenuItem();
meetingMenuItem.Text = "Meeting";
meetingMenuItem.Click += (s, e) => CreateNewMeeting();

ToolStripMenuItem contactMenuItem = new ToolStripMenuItem();
contactMenuItem.Text = "Contact";
contactMenuItem.Click += (s, e) => CreateNewContact();

// Add menu items to dropdown
newItemsButton.DropDownItems.AddRange(new ToolStripItem[] {
    emailMenuItem,
    appointmentMenuItem,
    meetingMenuItem,
    contactMenuItem
});

// Add to group
toolStripEx1.Items.Add(newItemsButton);
```

### Adding Menu Items via Designer

1. Select ToolStripDropDownButton
2. Click **DropDownItems** property
3. Use Items Collection Editor to add menu items
4. Or click the dropdown arrow on the button and type directly

### Hiding Dropdown Arrow

```csharp
// Hide the dropdown arrow
dropDownButton.ShowDropDownArrow = false;
```

### DropDown Properties

| Property | Description |
|----------|-------------|
| `DropDown` | Gets/sets the ToolStripDropDown to display |
| `DropDownItems` | Collection of items in dropdown menu |
| `ShowDropDownArrow` | Show/hide dropdown arrow indicator |
| `DropDownDirection` | Direction dropdown opens (Default, Left, Right, AboveLeft, AboveRight, BelowLeft, BelowRight) |

### Text Alignment for Menu Items

```csharp
// Enable text alignment for menu items
toolStripEx1.AllowMenuTextAlignment = true;

// Set alignment for specific menu items
emailMenuItem.TextAlign = ContentAlignment.MiddleRight;
appointmentMenuItem.TextAlign = ContentAlignment.MiddleCenter;
meetingMenuItem.TextAlign = ContentAlignment.MiddleLeft;
```

### ToolStripDropDownButton in Simplified Layout

```csharp
ToolStripDropDownButton pasteButton = new ToolStripDropDownButton();
pasteButton.Text = "Paste";

// Add menu items
pasteButton.DropDownItems.Add(new ToolStripMenuItem("Paste"));
pasteButton.DropDownItems.Add(new ToolStripMenuItem("Paste Special"));

// Add medium image for simplified layout
ImageListAdv mediumImages = new ImageListAdv();
mediumImages.Images.Add(Image.FromFile("paste20.png"));

ToolStripExImageProvider imageProvider = new ToolStripExImageProvider(toolStripEx1);
imageProvider.MediumImageList = mediumImages;
imageProvider.SetMediumItemImage(pasteButton, 0);

toolStripEx1.Items.Add(pasteButton);
```

## ToolStripSplitButton and ToolStripSplitButtonEx

Split button combines a regular button action with a dropdown menu.

### Difference Between SplitButton and SplitButtonEx

- **ToolStripSplitButton:** Standard split button with `DropDownButtonWidth` property
- **ToolStripSplitButtonEx:** Syncfusion enhanced version with additional styling

### Adding ToolStripSplitButtonEx

**Via Code:**

```csharp
// Create split button
ToolStripSplitButtonEx undoButton = new ToolStripSplitButtonEx();
undoButton.Text = "Undo";
undoButton.Image = Image.FromFile("undo.png");

// Add dropdown items
undoButton.DropDownItems.Add("Undo Typing");
undoButton.DropDownItems.Add("Undo Bold");
undoButton.DropDownItems.Add("Undo Paste");

// Handle button click (main action)
undoButton.ButtonClick += (s, e) => UndoLastAction();

// Handle dropdown item click
undoButton.DropDownItemClicked += (s, e) =>
{
    string action = e.ClickedItem.Text;
    UndoSpecificAction(action);
};

// Add to group
toolStripEx1.Items.Add(undoButton);
```

### Adding ToolStripSplitButton

```csharp
// Standard split button with dropdown width control
ToolStripSplitButton splitButton = new ToolStripSplitButton();
splitButton.Text = "Split Action";
splitButton.DropDownButtonWidth = 20; // Set dropdown arrow width

// Add dropdown items
splitButton.DropDownItems.Add("Option 1");
splitButton.DropDownItems.Add("Option 2");

toolStripEx1.Items.Add(splitButton);
```

### Split Button Events

```csharp
// ButtonClick - fires when button part is clicked
splitButton.ButtonClick += (s, e) =>
{
    // Execute default action
    ExecuteDefaultAction();
};

// DropDownOpening - fires before dropdown opens
splitButton.DropDownOpening += (s, e) =>
{
    // Populate dropdown items dynamically
    PopulateRecentActions();
};

// DropDownItemClicked - fires when dropdown item clicked
splitButton.DropDownItemClicked += (s, e) =>
{
    // Handle specific dropdown item
    string selectedItem = e.ClickedItem.Text;
    ProcessSelection(selectedItem);
};
```

### Split Button in Simplified Layout

```csharp
ToolStripSplitButtonEx pasteButton = new ToolStripSplitButtonEx();
pasteButton.Text = "Paste";

// Add medium image
ImageListAdv mediumImages = new ImageListAdv();
mediumImages.Images.Add(Image.FromFile("paste20.png"));

ToolStripExImageProvider imageProvider = new ToolStripExImageProvider(toolStripEx1);
imageProvider.MediumImageList = mediumImages;
imageProvider.SetMediumItemImage(pasteButton, 0);

toolStripEx1.Items.Add(pasteButton);
```

## ToolStripComboBoxEx

Enhanced combo box for dropdown selection lists.

### Adding ToolStripComboBoxEx

**Via Code:**

```csharp
// Create combo box
ToolStripComboBoxEx fontComboBox = new ToolStripComboBoxEx();

// Add items
fontComboBox.Items.AddRange(new object[] {
    "Arial",
    "Calibri",
    "Times New Roman",
    "Verdana",
    "Tahoma"
});

// Set default selection
fontComboBox.SelectedIndex = 1; // Calibri

// Handle selection changed
fontComboBox.SelectedIndexChanged += (s, e) =>
{
    string selectedFont = fontComboBox.SelectedItem.ToString();
    ApplyFont(selectedFont);
};

// Add to group
toolStripEx1.Items.Add(fontComboBox);
```

### DropDownStyle Options

```csharp
// Simple - No dropdown button, all items shown below
fontComboBox.DropDownStyle = ComboBoxStyle.Simple;

// DropDown - Has dropdown button, text is editable
fontComboBox.DropDownStyle = ComboBoxStyle.DropDown;

// DropDownList - Has dropdown button, text is NOT editable (default)
fontComboBox.DropDownStyle = ComboBoxStyle.DropDownList;
```

### Combo Box Sizing

```csharp
// Set size
fontComboBox.Size = new Size(150, 25);

// Set dropdown height
fontComboBox.DropDownHeight = 200;

// Set maximum dropdown width
fontComboBox.DropDownWidth = 180;
```

### ToolStripComboBoxEx in Simplified Layout

```csharp
ToolStripComboBoxEx styleComboBox = new ToolStripComboBoxEx();
styleComboBox.Items.AddRange(new object[] {
    "Office 2019 Style",
    "Office 2016 Style",
    "Touch Style",
    "Office 2013 Style"
});
styleComboBox.SelectedIndex = 0;

toolStripEx1.Items.Add(styleComboBox);
```

## ToolStripGallery

Visual gallery control for displaying a collection of items with scrolling support.

### Adding ToolStripGallery

**Via Code:**

```csharp
// Create gallery
ToolStripGallery quickStepsGallery = new ToolStripGallery();

// Create gallery items
ToolStripGalleryItem moveToItem = new ToolStripGalleryItem();
moveToItem.Text = "Move to ?";
moveToItem.Image = Image.FromFile("moveto.png");

ToolStripGalleryItem toManagerItem = new ToolStripGalleryItem();
toManagerItem.Text = "To Manager";
toManagerItem.Image = Image.FromFile("tomanager.png");

ToolStripGalleryItem teamEmailItem = new ToolStripGalleryItem();
teamEmailItem.Text = "Team Email";
teamEmailItem.Image = Image.FromFile("teamemail.png");

ToolStripGalleryItem replyDeleteItem = new ToolStripGalleryItem();
replyDeleteItem.Text = "Reply and Delete";
replyDeleteItem.Image = Image.FromFile("replydelete.png");

// Add items to gallery
quickStepsGallery.Items.AddRange(new ToolStripGalleryItem[] {
    moveToItem,
    toManagerItem,
    teamEmailItem,
    replyDeleteItem
});

// Add to group
toolStripEx1.Items.Add(quickStepsGallery);
```

### Gallery Scroller Settings

```csharp
// Standard Scroller - Normal up/down scrolling
quickStepsGallery.ScrollerType = ScrollerType.StandardScroller;

// Compact Scroller - Up/down arrows + dropdown for all items
quickStepsGallery.ScrollerType = ScrollerType.CompactScroller;
```

### Gallery Item Events

```csharp
// Handle item click
quickStepsGallery.ItemClick += (s, e) =>
{
    ToolStripGalleryItem clickedItem = e.Item as ToolStripGalleryItem;
    if (clickedItem != null)
    {
        ExecuteQuickStep(clickedItem.Text);
    }
};
```

### Gallery Appearance

```csharp
// Set item size
quickStepsGallery.ItemSize = new Size(100, 40);

// Set number of columns
quickStepsGallery.ColumnCount = 3;

// Set gallery height
quickStepsGallery.Height = 120;
```

### ToolStripGallery in Simplified Layout

```csharp
ToolStripGallery gallery = new ToolStripGallery();

// Add items
ToolStripGalleryItem item1 = new ToolStripGalleryItem();
item1.Text = "Quick Action 1";

ToolStripGalleryItem item2 = new ToolStripGalleryItem();
item2.Text = "Quick Action 2";

gallery.Items.AddRange(new ToolStripGalleryItem[] { item1, item2 });

toolStripEx1.Items.Add(gallery);
```

## ToolStripCheckBox

Checkbox control for boolean options.

### Adding ToolStripCheckBox

**Via Code:**

```csharp
// Create checkbox
ToolStripCheckBox showConversationsCheckBox = new ToolStripCheckBox();
showConversationsCheckBox.Text = "Show As Conversations";
showConversationsCheckBox.Checked = true;

// Handle checked changed event
showConversationsCheckBox.CheckedChanged += (s, e) =>
{
    if (showConversationsCheckBox.Checked)
        EnableConversationView();
    else
        DisableConversationView();
};

// Add to group
toolStripEx1.Items.Add(showConversationsCheckBox);
```

### Checkbox States

```csharp
// Check the checkbox
checkBox.Checked = true;

// Uncheck the checkbox
checkBox.Checked = false;

// Three-state checkbox
checkBox.ThreeState = true;
checkBox.CheckState = CheckState.Indeterminate;
```

### ToolStripCheckBox in Simplified Layout

```csharp
ToolStripCheckBox checkBox = new ToolStripCheckBox();
checkBox.Text = "Enable Feature";
checkBox.Checked = false;

toolStripEx1.Items.Add(checkBox);
```

## ToolStripTextBox

Text input field in the ribbon.

### Adding ToolStripTextBox

**Via Code:**

```csharp
// Create text box
ToolStripTextBox searchTextBox = new ToolStripTextBox();
searchTextBox.Text = "Search...";
searchTextBox.Size = new Size(200, 25);

// Handle text changed event
searchTextBox.TextChanged += (s, e) =>
{
    PerformSearch(searchTextBox.Text);
};

// Handle key press for Enter key
searchTextBox.KeyPress += (s, e) =>
{
    if (e.KeyChar == (char)Keys.Enter)
    {
        ExecuteSearch(searchTextBox.Text);
        e.Handled = true;
    }
};

// Add to panel (text boxes often need panel for layout)
ToolStripPanelItem searchPanel = new ToolStripPanelItem();
searchPanel.Items.Add(searchTextBox);

toolStripEx1.Items.Add(searchPanel);
```

### TextBox Properties

```csharp
// Set placeholder behavior
searchTextBox.ForeColor = SystemColors.GrayText;

// Multiline textbox
searchTextBox.Multiline = true;

// Max length
searchTextBox.MaxLength = 100;

// Read-only
searchTextBox.ReadOnly = true;
```

### ToolStripTextBox in Simplified Layout

```csharp
ToolStripTextBox textBox = new ToolStripTextBox();
textBox.Text = "Enter Text";
textBox.Size = new Size(150, 20);

toolStripEx1.Items.Add(textBox);
```

## ToolStripProgressBar

Progress indicator for long-running operations.

### Adding ToolStripProgressBar

**Via Code:**

```csharp
// Create progress bar
ToolStripProgressBar uploadProgressBar = new ToolStripProgressBar();
uploadProgressBar.Minimum = 0;
uploadProgressBar.Maximum = 100;
uploadProgressBar.Value = 0;
uploadProgressBar.Size = new Size(150, 20);

// Add label for context
ToolStripLabel progressLabel = new ToolStripLabel();
progressLabel.Text = "Uploading:";

// Add both to panel
ToolStripPanelItem progressPanel = new ToolStripPanelItem();
progressPanel.Items.AddRange(new ToolStripItem[] {
    progressLabel,
    uploadProgressBar
});

toolStripEx1.Items.Add(progressPanel);

// Update progress programmatically
uploadProgressBar.Value = 50; // 50% complete
```

### Progress Bar Styles

```csharp
// Continuous progress bar (default)
progressBar.Style = ProgressBarStyle.Continuous;

// Marquee style (indeterminate)
progressBar.Style = ProgressBarStyle.Marquee;
progressBar.MarqueeAnimationSpeed = 30;

// Blocks style
progressBar.Style = ProgressBarStyle.Blocks;
```

### ToolStripProgressBar in Simplified Layout

```csharp
ToolStripProgressBar progressBar = new ToolStripProgressBar();
progressBar.Value = 50;
progressBar.Size = new Size(120, 18);

toolStripEx1.Items.Add(progressBar);
```

## ToolStripLabel

Static text label for displaying information.

### Adding ToolStripLabel

**Via Code:**

```csharp
// Create label
ToolStripLabel statusLabel = new ToolStripLabel();
statusLabel.Text = "Ready";
statusLabel.Font = new Font("Segoe UI", 9, FontStyle.Bold);
statusLabel.ForeColor = Color.Blue;

// Add to group
toolStripEx1.Items.Add(statusLabel);
```

### Label with Icon

```csharp
// Label with image
ToolStripLabel infoLabel = new ToolStripLabel();
infoLabel.Text = "Information";
infoLabel.Image = Image.FromFile("info.png");
infoLabel.ImageAlign = ContentAlignment.MiddleLeft;
infoLabel.TextAlign = ContentAlignment.MiddleRight;
```

### ToolStripLabel in Simplified Layout

```csharp
ToolStripLabel label = new ToolStripLabel();
label.Text = "Status: Active";
label.ForeColor = Color.Green;

toolStripEx1.Items.Add(label);
```

## ToolStripSeparator

Visual separator between ribbon items.

### Adding ToolStripSeparator

**Via Code:**

```csharp
// Create separator
ToolStripSeparator separator = new ToolStripSeparator();

// Add between items
toolStripEx1.Items.Add(cutButton);
toolStripEx1.Items.Add(copyButton);
toolStripEx1.Items.Add(separator); // Visual separation
toolStripEx1.Items.Add(formatButton);
```

### Separator Appearance

```csharp
// Vertical separator (default in horizontal layout)
separator.Size = new Size(6, 25);

// Horizontal separator (in vertical layout)
separator.Size = new Size(100, 2);
```

### ToolStripSeparator in Simplified Layout

```csharp
ToolStripButton cutButton = new ToolStripButton();
cutButton.Text = "Cut";

ToolStripSeparator separator = new ToolStripSeparator();

ToolStripButton copyButton = new ToolStripButton();
copyButton.Text = "Copy";

toolStripEx1.Items.AddRange(new ToolStripItem[] {
    cutButton,
    separator,
    copyButton
});
```

## ToolStripPanelItem

Container for arranging items in multiple rows.

### Adding ToolStripPanelItem

**Via Code:**

```csharp
// Create panel
ToolStripPanelItem multiRowPanel = new ToolStripPanelItem();
multiRowPanel.RowCount = 2; // Two rows

// Create items for first row
ToolStripButton button1 = new ToolStripButton("Button 1");
ToolStripButton button2 = new ToolStripButton("Button 2");

// Create items for second row
ToolStripButton button3 = new ToolStripButton("Button 3");
ToolStripButton button4 = new ToolStripButton("Button 4");

// Add items to panel
multiRowPanel.Items.AddRange(new ToolStripItem[] {
    button1,
    button2,
    button3,
    button4
});

// Add panel to group
toolStripEx1.Items.Add(multiRowPanel);
```

### Advanced Multi-Row Layout

```csharp
// Create 3-row panel for complex layouts
ToolStripPanelItem threeRowPanel = new ToolStripPanelItem();
threeRowPanel.RowCount = 3;

// Row 1: Large button
ToolStripButton largeButton = new ToolStripButton();
largeButton.Text = "Large\r\nAction";
largeButton.DisplayStyle = ToolStripItemDisplayStyle.ImageAndText;
largeButton.TextImageRelation = TextImageRelation.ImageAboveText;

// Row 2: Two medium buttons
ToolStripButton medium1 = new ToolStripButton("Action 1");
ToolStripButton medium2 = new ToolStripButton("Action 2");

// Row 3: Three small buttons
ToolStripButton small1 = new ToolStripButton("A");
ToolStripButton small2 = new ToolStripButton("B");
ToolStripButton small3 = new ToolStripButton("C");

threeRowPanel.Items.AddRange(new ToolStripItem[] {
    largeButton,
    medium1,
    medium2,
    small1,
    small2,
    small3
});

toolStripEx1.Items.Add(threeRowPanel);
```

### Nested Panels

```csharp
// Panels can contain other panels
ToolStripPanelItem outerPanel = new ToolStripPanelItem();
outerPanel.RowCount = 1;

ToolStripPanelItem innerPanel1 = new ToolStripPanelItem();
innerPanel1.RowCount = 2;
innerPanel1.Items.Add(new ToolStripButton("1A"));
innerPanel1.Items.Add(new ToolStripButton("1B"));

ToolStripPanelItem innerPanel2 = new ToolStripPanelItem();
innerPanel2.RowCount = 2;
innerPanel2.Items.Add(new ToolStripButton("2A"));
innerPanel2.Items.Add(new ToolStripButton("2B"));

outerPanel.Items.AddRange(new ToolStripItem[] {
    innerPanel1,
    innerPanel2
});

toolStripEx1.Items.Add(outerPanel);
```

## Common Properties

All ribbon controls inherit from ToolStripItem and share these common properties:

### Display Properties

| Property | Type | Description |
|----------|------|-------------|
| `Text` | string | Display text |
| `Image` | Image | Display icon/image |
| `DisplayStyle` | ToolStripItemDisplayStyle | Image, Text, ImageAndText, None |
| `TextImageRelation` | TextImageRelation | Image/text layout relationship |
| `Size` | Size | Control size |
| `AutoSize` | bool | Auto-size based on content |

### Alignment Properties

| Property | Type | Description |
|----------|------|-------------|
| `Alignment` | ToolStripItemAlignment | Left or Right alignment |
| `TextAlign` | ContentAlignment | Text alignment within control |
| `ImageAlign` | ContentAlignment | Image alignment within control |
| `Margin` | Padding | Space around control |
| `Padding` | Padding | Space inside control |

### Behavior Properties

| Property | Type | Description |
|----------|------|-------------|
| `Enabled` | bool | Enable/disable control |
| `Visible` | bool | Show/hide control |
| `ToolTipText` | string | Tooltip on hover |
| `Tag` | object | Custom data storage |

### RTL Support

| Property | Type | Description |
|----------|------|-------------|
| `RightToLeft` | RightToLeft | RTL layout support |
| `RightToLeftAutoMirrorImage` | bool | Mirror image in RTL mode |

## Simplified Layout Support

All ribbon controls can be configured for simplified layout mode using `RibbonItemDisplayMode`.

### Controlling Visibility Across Layouts

```csharp
// Display in simplified layout only
ribbonControlAdv1.SetDisplayMode(pasteButton, RibbonItemDisplayMode.Simplified);

// Display in normal layout only
ribbonControlAdv1.SetDisplayMode(boldButton, RibbonItemDisplayMode.Normal);

// Display in both layouts
ribbonControlAdv1.SetDisplayMode(saveButton, 
    RibbonItemDisplayMode.Normal | RibbonItemDisplayMode.Simplified);

// Display in normal and overflow menu during simplified
ribbonControlAdv1.SetDisplayMode(formatButton, 
    RibbonItemDisplayMode.Normal | RibbonItemDisplayMode.OverflowMenu);
```

### Medium-Size Images for Simplified Layout

Simplified layout uses 20x20 pixel images by default:

```csharp
// Create medium image list (20x20)
ImageListAdv mediumImages = new ImageListAdv();
mediumImages.Images.Add(Image.FromFile("cut20.png"));
mediumImages.Images.Add(Image.FromFile("copy20.png"));
mediumImages.Images.Add(Image.FromFile("paste20.png"));

// Set up image provider
ToolStripExImageProvider imageProvider = new ToolStripExImageProvider(toolStripEx1);
imageProvider.MediumImageList = mediumImages;

// Assign medium images to controls
imageProvider.SetMediumItemImage(cutButton, 0);
imageProvider.SetMediumItemImage(copyButton, 1);
imageProvider.SetMediumItemImage(pasteButton, 2);
```

## Best Practices

1. **Use appropriate control types:** Choose controls that match user intent (button for action, combo for selection, etc.)

2. **Provide tooltips:** Always set `ToolTipText` for controls, especially image-only buttons

3. **Size consistency:** Keep similar controls the same size for visual harmony

4. **Image quality:** Provide multiple image sizes (32x32, 20x20, 16x16) for different layouts and states

5. **Event handling:** Always handle appropriate events (Click for buttons, SelectedIndexChanged for combos, etc.)

6. **Panel usage:** Use ToolStripPanelItem to organize related controls in multiple rows

7. **Simplified layout:** Configure visibility and provide medium-size images for simplified layout support

8. **Accessibility:** Set Text property even for image-only displays (used by screen readers)

9. **Disable vs Hide:** Prefer disabling controls over hiding them to maintain layout stability

10. **Test resize:** Test all controls during window resize to ensure proper collapse behavior
