# Status Items

The StatusStripEx control supports two categories of items: **StatusControl items** and **Notification items**. Understanding the difference between these item types and how to use them is essential for creating an effective status bar.

## Table of Contents

- [Overview of Item Types](#overview-of-item-types)
- [StatusControl Items (Right-Aligned)](#statuscontrol-items-right-aligned)
  - [StatusLabel](#statuslabel)
  - [ProgressBar](#progressbar)
  - [DropDownButton](#dropdownbutton)
  - [SplitButton](#splitbutton)
  - [PanelItem](#panelitem)
  - [TrackBarItem](#trackbaritem)
- [Notification Items (Left-Aligned)](#notification-items-left-aligned)
  - [StatusStripButton](#statusstripbutton)
  - [StatusStripLabel](#statusstriplabel)
  - [StatusStripProgressBar](#statusstripprogressbar)
  - [StatusStripDropDownButton](#statusstripdropdownbutton)
  - [StatusStripSplitButton](#statusstripsplitbutton)
  - [StatusStripPanelItem](#statusstrippanelitem)
- [Difference Between StatusControl and Notification Items](#difference-between-statuscontrol-and-notification-items)
- [Adding Items to StatusStripEx](#adding-items-to-statusstripex)
- [Item Positioning and Alignment](#item-positioning-and-alignment)
- [Complete Examples](#complete-examples)

## Overview of Item Types

The StatusStripEx control organizes items into two distinct categories based on their positioning:

1. **StatusControl Items** - Positioned on the **right side** of the status bar
2. **Notification Items** - Positioned on the **left side** of the status bar

Both categories offer similar control types (labels, buttons, progress bars, etc.), but their positioning differs based on which category they belong to.

## StatusControl Items (Right-Aligned)

StatusControl items are automatically positioned on the right side of the StatusStripEx. These items are typically used for displaying status information that relates to the overall application or document state.

### StatusLabel

The StatusLabel displays text information on the right side of the status bar.

#### Adding Through Designer

1. Open the **Items Collection Editor**
2. Click **Add** and select **StatusLabel**
3. Configure properties like `Text`, `AutoSize`, and `TextAlign`

#### Adding Through Code

```csharp
using Syncfusion.Windows.Forms.Tools;

// Create and configure StatusLabel
StatusLabel statusLabel1 = new StatusLabel();
statusLabel1.Text = "Line 1, Column 1";
statusLabel1.BorderSides = ToolStripStatusLabelBorderSides.Left;
statusLabel1.BorderStyle = Border3DStyle.Etched;

// Add to StatusStripEx
this.statusStripEx1.Items.Add(statusLabel1);
```

```vb
Imports Syncfusion.Windows.Forms.Tools

' Create and configure StatusLabel
Dim statusLabel1 As New StatusLabel()
statusLabel1.Text = "Line 1, Column 1"
statusLabel1.BorderSides = ToolStripStatusLabelBorderSides.Left
statusLabel1.BorderStyle = Border3DStyle.Etched

' Add to StatusStripEx
Me.statusStripEx1.Items.Add(statusLabel1)
```

#### Common StatusLabel Properties

| Property | Type | Description |
|----------|------|-------------|
| `Text` | `string` | Gets or sets the text displayed in the label |
| `AutoSize` | `bool` | Gets or sets whether the label automatically sizes to fit its content |
| `BorderSides` | `ToolStripStatusLabelBorderSides` | Specifies which sides of the label have borders |
| `BorderStyle` | `Border3DStyle` | Gets or sets the border style |
| `TextAlign` | `ContentAlignment` | Gets or sets the alignment of text within the label |

### ProgressBar

The ProgressBar displays progress information on the right side of the status bar.

#### Adding Through Designer

1. Open the **Items Collection Editor**
2. Click **Add** and select **ProgressBar**
3. Configure properties like `Minimum`, `Maximum`, and `Value`

#### Adding Through Code

```csharp
using System.Windows.Forms;

// Create and configure ProgressBar
ToolStripProgressBar progressBar1 = new ToolStripProgressBar();
progressBar1.Minimum = 0;
progressBar1.Maximum = 100;
progressBar1.Value = 45;
progressBar1.Size = new System.Drawing.Size(150, 16);

// Add to StatusStripEx
this.statusStripEx1.Items.Add(progressBar1);
```

```vb
Imports System.Windows.Forms

' Create and configure ProgressBar
Dim progressBar1 As New ToolStripProgressBar()
progressBar1.Minimum = 0
progressBar1.Maximum = 100
progressBar1.Value = 45
progressBar1.Size = New System.Drawing.Size(150, 16)

' Add to StatusStripEx
Me.statusStripEx1.Items.Add(progressBar1)
```

#### Updating Progress

```csharp
// Update progress value
progressBar1.Value = 75;

// Increment progress
progressBar1.Increment(10);

// Set to indeterminate style (marquee)
progressBar1.Style = ToolStripProgressBarStyle.Marquee;
progressBar1.MarqueeAnimationSpeed = 30;
```

```vb
' Update progress value
progressBar1.Value = 75

' Increment progress
progressBar1.Increment(10)

' Set to indeterminate style (marquee)
progressBar1.Style = ToolStripProgressBarStyle.Marquee
progressBar1.MarqueeAnimationSpeed = 30
```

### DropDownButton

The DropDownButton displays a button with a dropdown menu on the right side.

#### Adding Through Code

```csharp
using System.Windows.Forms;

// Create DropDownButton
ToolStripDropDownButton dropDownButton1 = new ToolStripDropDownButton();
dropDownButton1.Text = "View Options";
dropDownButton1.ShowDropDownArrow = true;

// Create dropdown items
ToolStripMenuItem item1 = new ToolStripMenuItem("Option 1");
ToolStripMenuItem item2 = new ToolStripMenuItem("Option 2");
ToolStripMenuItem item3 = new ToolStripMenuItem("Option 3");

// Add dropdown items
dropDownButton1.DropDownItems.AddRange(new ToolStripItem[] 
{ 
    item1, 
    item2, 
    item3 
});

// Add event handlers
item1.Click += (s, e) => MessageBox.Show("Option 1 selected");
item2.Click += (s, e) => MessageBox.Show("Option 2 selected");

// Add to StatusStripEx
this.statusStripEx1.Items.Add(dropDownButton1);
```

```vb
Imports System.Windows.Forms

' Create DropDownButton
Dim dropDownButton1 As New ToolStripDropDownButton()
dropDownButton1.Text = "View Options"
dropDownButton1.ShowDropDownArrow = True

' Create dropdown items
Dim item1 As New ToolStripMenuItem("Option 1")
Dim item2 As New ToolStripMenuItem("Option 2")
Dim item3 As New ToolStripMenuItem("Option 3")

' Add dropdown items
dropDownButton1.DropDownItems.AddRange(New ToolStripItem() {
    item1,
    item2,
    item3
})

' Add event handlers
AddHandler item1.Click, Sub(s, e) MessageBox.Show("Option 1 selected")
AddHandler item2.Click, Sub(s, e) MessageBox.Show("Option 2 selected")

' Add to StatusStripEx
Me.statusStripEx1.Items.Add(dropDownButton1)
```

### SplitButton

The SplitButton combines a regular button with a dropdown menu on the right side.

#### Adding Through Code

```csharp
using System.Windows.Forms;

// Create SplitButton
ToolStripSplitButton splitButton1 = new ToolStripSplitButton();
splitButton1.Text = "Save";

// Handle default button click
splitButton1.ButtonClick += (s, e) => 
{
    MessageBox.Show("Save clicked");
};

// Create dropdown items
ToolStripMenuItem saveAs = new ToolStripMenuItem("Save As...");
ToolStripMenuItem saveAll = new ToolStripMenuItem("Save All");

// Add dropdown items
splitButton1.DropDownItems.AddRange(new ToolStripItem[] 
{ 
    saveAs, 
    saveAll 
});

// Add to StatusStripEx
this.statusStripEx1.Items.Add(splitButton1);
```

```vb
Imports System.Windows.Forms

' Create SplitButton
Dim splitButton1 As New ToolStripSplitButton()
splitButton1.Text = "Save"

' Handle default button click
AddHandler splitButton1.ButtonClick, Sub(s, e)
    MessageBox.Show("Save clicked")
End Sub

' Create dropdown items
Dim saveAs As New ToolStripMenuItem("Save As...")
Dim saveAll As New ToolStripMenuItem("Save All")

' Add dropdown items
splitButton1.DropDownItems.AddRange(New ToolStripItem() {
    saveAs,
    saveAll
})

' Add to StatusStripEx
Me.statusStripEx1.Items.Add(splitButton1)
```

### PanelItem

The PanelItem is a container that can host other controls on the right side.

#### Adding Through Code

```csharp
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

// Create PanelItem
StatusPanelItem panelItem1 = new StatusPanelItem();
panelItem1.AutoSize = false;
panelItem1.Size = new System.Drawing.Size(100, 22);

// Create a control to host in the panel
ComboBox comboBox = new ComboBox();
comboBox.Items.AddRange(new object[] { "Item 1", "Item 2", "Item 3" });
comboBox.SelectedIndex = 0;
comboBox.Dock = DockStyle.Fill;

// Add control to panel
panelItem1.Control = comboBox;

// Add to StatusStripEx
this.statusStripEx1.Items.Add(panelItem1);
```

```vb
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

' Create PanelItem
Dim panelItem1 As New StatusPanelItem()
panelItem1.AutoSize = False
panelItem1.Size = New System.Drawing.Size(100, 22)

' Create a control to host in the panel
Dim comboBox As New ComboBox()
comboBox.Items.AddRange(New Object() {"Item 1", "Item 2", "Item 3"})
comboBox.SelectedIndex = 0
comboBox.Dock = DockStyle.Fill

' Add control to panel
panelItem1.Control = comboBox

' Add to StatusStripEx
Me.statusStripEx1.Items.Add(panelItem1)
```

### TrackBarItem

The TrackBarItem displays a track bar control on the right side, useful for zoom levels or volume control.

#### Adding Through Code

```csharp
using Syncfusion.Windows.Forms.Tools;

// Create TrackBarItem
StatusTrackBarItem trackBarItem1 = new StatusTrackBarItem();
trackBarItem1.Minimum = 0;
trackBarItem1.Maximum = 100;
trackBarItem1.Value = 50;
trackBarItem1.TickFrequency = 10;
trackBarItem1.AutoSize = false;
trackBarItem1.Width = 120;

// Handle value changed event
trackBarItem1.ValueChanged += (s, e) => 
{
    // Update zoom level or other setting
    UpdateZoomLevel(trackBarItem1.Value);
};

// Add to StatusStripEx
this.statusStripEx1.Items.Add(trackBarItem1);
```

```vb
Imports Syncfusion.Windows.Forms.Tools

' Create TrackBarItem
Dim trackBarItem1 As New StatusTrackBarItem()
trackBarItem1.Minimum = 0
trackBarItem1.Maximum = 100
trackBarItem1.Value = 50
trackBarItem1.TickFrequency = 10
trackBarItem1.AutoSize = False
trackBarItem1.Width = 120

' Handle value changed event
AddHandler trackBarItem1.ValueChanged, Sub(s, e)
    ' Update zoom level or other setting
    UpdateZoomLevel(trackBarItem1.Value)
End Sub

' Add to StatusStripEx
Me.statusStripEx1.Items.Add(trackBarItem1)
```

## Notification Items (Left-Aligned)

Notification items are automatically positioned on the left side of the StatusStripEx. These items are typically used for displaying notifications, alerts, or quick-access controls.

### StatusStripButton

The StatusStripButton displays a button on the left side of the status bar.

#### Adding Through Code

```csharp
using System.Windows.Forms;

// Create StatusStripButton
ToolStripButton statusStripButton1 = new ToolStripButton();
statusStripButton1.Text = "Notifications";
statusStripButton1.DisplayStyle = ToolStripItemDisplayStyle.ImageAndText;
statusStripButton1.Image = Properties.Resources.NotificationIcon;

// Handle click event
statusStripButton1.Click += (s, e) => 
{
    ShowNotifications();
};

// Add to StatusStripEx (will appear on left)
this.statusStripEx1.Items.Add(statusStripButton1);
```

```vb
Imports System.Windows.Forms

' Create StatusStripButton
Dim statusStripButton1 As New ToolStripButton()
statusStripButton1.Text = "Notifications"
statusStripButton1.DisplayStyle = ToolStripItemDisplayStyle.ImageAndText
statusStripButton1.Image = My.Resources.NotificationIcon

' Handle click event
AddHandler statusStripButton1.Click, Sub(s, e)
    ShowNotifications()
End Sub

' Add to StatusStripEx (will appear on left)
Me.statusStripEx1.Items.Add(statusStripButton1)
```

### StatusStripLabel

The StatusStripLabel displays text information on the left side of the status bar.

#### Adding Through Code

```csharp
using System.Windows.Forms;

// Create StatusStripLabel
ToolStripStatusLabel statusStripLabel1 = new ToolStripStatusLabel();
statusStripLabel1.Text = "Pages: 1/5";
statusStripLabel1.StatusString = "1/5";  // For context menu customization
statusStripLabel1.BorderSides = ToolStripStatusLabelBorderSides.Right;

// Add to StatusStripEx (will appear on left)
this.statusStripEx1.Items.Add(statusStripLabel1);
```

```vb
Imports System.Windows.Forms

' Create StatusStripLabel
Dim statusStripLabel1 As New ToolStripStatusLabel()
statusStripLabel1.Text = "Pages: 1/5"
statusStripLabel1.StatusString = "1/5"  ' For context menu customization
statusStripLabel1.BorderSides = ToolStripStatusLabelBorderSides.Right

' Add to StatusStripEx (will appear on left)
Me.statusStripEx1.Items.Add(statusStripLabel1)
```

#### StatusString Property

The `StatusString` property is used to customize the context menu display:

```csharp
// Set StatusString for Word-like context menu
statusStripLabel1.Text = "Words";
statusStripLabel1.StatusString = "1,234";
```

```vb
' Set StatusString for Word-like context menu
statusStripLabel1.Text = "Words"
statusStripLabel1.StatusString = "1,234"
```

### StatusStripProgressBar

The StatusStripProgressBar displays progress information on the left side.

#### Adding Through Code

```csharp
using System.Windows.Forms;

// Create StatusStripProgressBar
ToolStripProgressBar statusStripProgressBar1 = new ToolStripProgressBar();
statusStripProgressBar1.Minimum = 0;
statusStripProgressBar1.Maximum = 100;
statusStripProgressBar1.Value = 0;
statusStripProgressBar1.Size = new System.Drawing.Size(100, 16);
statusStripProgressBar1.Visible = false;  // Initially hidden

// Add to StatusStripEx (will appear on left)
this.statusStripEx1.Items.Add(statusStripProgressBar1);
```

```vb
Imports System.Windows.Forms

' Create StatusStripProgressBar
Dim statusStripProgressBar1 As New ToolStripProgressBar()
statusStripProgressBar1.Minimum = 0
statusStripProgressBar1.Maximum = 100
statusStripProgressBar1.Value = 0
statusStripProgressBar1.Size = New System.Drawing.Size(100, 16)
statusStripProgressBar1.Visible = False  ' Initially hidden

' Add to StatusStripEx (will appear on left)
Me.statusStripEx1.Items.Add(statusStripProgressBar1)
```

### StatusStripDropDownButton

The StatusStripDropDownButton displays a dropdown button on the left side.

#### Adding Through Code

```csharp
using System.Windows.Forms;

// Create StatusStripDropDownButton
ToolStripDropDownButton statusStripDropDown1 = new ToolStripDropDownButton();
statusStripDropDown1.Text = "Quick Actions";

// Create menu items
ToolStripMenuItem action1 = new ToolStripMenuItem("Action 1");
ToolStripMenuItem action2 = new ToolStripMenuItem("Action 2");

// Add menu items
statusStripDropDown1.DropDownItems.AddRange(new ToolStripItem[] 
{ 
    action1, 
    action2 
});

// Add to StatusStripEx (will appear on left)
this.statusStripEx1.Items.Add(statusStripDropDown1);
```

```vb
Imports System.Windows.Forms

' Create StatusStripDropDownButton
Dim statusStripDropDown1 As New ToolStripDropDownButton()
statusStripDropDown1.Text = "Quick Actions"

' Create menu items
Dim action1 As New ToolStripMenuItem("Action 1")
Dim action2 As New ToolStripMenuItem("Action 2")

' Add menu items
statusStripDropDown1.DropDownItems.AddRange(New ToolStripItem() {
    action1,
    action2
})

' Add to StatusStripEx (will appear on left)
Me.statusStripEx1.Items.Add(statusStripDropDown1)
```

### StatusStripSplitButton

The StatusStripSplitButton displays a split button on the left side.

#### Adding Through Code

```csharp
using System.Windows.Forms;

// Create StatusStripSplitButton
ToolStripSplitButton statusStripSplit1 = new ToolStripSplitButton();
statusStripSplit1.Text = "Refresh";

// Handle button click
statusStripSplit1.ButtonClick += (s, e) => 
{
    RefreshData();
};

// Create dropdown items
ToolStripMenuItem refreshAll = new ToolStripMenuItem("Refresh All");
ToolStripMenuItem refreshSelected = new ToolStripMenuItem("Refresh Selected");

// Add dropdown items
statusStripSplit1.DropDownItems.AddRange(new ToolStripItem[] 
{ 
    refreshAll, 
    refreshSelected 
});

// Add to StatusStripEx (will appear on left)
this.statusStripEx1.Items.Add(statusStripSplit1);
```

```vb
Imports System.Windows.Forms

' Create StatusStripSplitButton
Dim statusStripSplit1 As New ToolStripSplitButton()
statusStripSplit1.Text = "Refresh"

' Handle button click
AddHandler statusStripSplit1.ButtonClick, Sub(s, e)
    RefreshData()
End Sub

' Create dropdown items
Dim refreshAll As New ToolStripMenuItem("Refresh All")
Dim refreshSelected As New ToolStripMenuItem("Refresh Selected")

' Add dropdown items
statusStripSplit1.DropDownItems.AddRange(New ToolStripItem() {
    refreshAll,
    refreshSelected
})

' Add to StatusStripEx (will appear on left)
Me.statusStripEx1.Items.Add(statusStripSplit1)
```

### StatusStripPanelItem

The StatusStripPanelItem is a container for hosting controls on the left side.

#### Adding Through Code

```csharp
using System.Windows.Forms;

// Create StatusStripPanelItem
ToolStripControlHost panelItem1 = new ToolStripControlHost(new Panel());
panelItem1.AutoSize = false;
panelItem1.Size = new System.Drawing.Size(80, 20);

// Create a button to host
Button button = new Button();
button.Text = "Click";
button.Dock = DockStyle.Fill;
button.FlatStyle = FlatStyle.Flat;

// Set the control
((Panel)panelItem1.Control).Controls.Add(button);

// Add to StatusStripEx (will appear on left)
this.statusStripEx1.Items.Add(panelItem1);
```

```vb
Imports System.Windows.Forms

' Create StatusStripPanelItem
Dim panelItem1 As New ToolStripControlHost(New Panel())
panelItem1.AutoSize = False
panelItem1.Size = New System.Drawing.Size(80, 20)

' Create a button to host
Dim button As New Button()
button.Text = "Click"
button.Dock = DockStyle.Fill
button.FlatStyle = FlatStyle.Flat

' Set the control
CType(panelItem1.Control, Panel).Controls.Add(button)

' Add to StatusStripEx (will appear on left)
Me.statusStripEx1.Items.Add(panelItem1)
```

## Difference Between StatusControl and Notification Items

The key differences between StatusControl and Notification items:

| Aspect | StatusControl Items | Notification Items |
|--------|--------------------|--------------------|
| **Position** | Right side of StatusStripEx | Left side of StatusStripEx |
| **Purpose** | Display overall status information | Display notifications and quick actions |
| **Typical Use** | Line/column numbers, zoom level, document state | Alerts, page count, quick settings |
| **Alignment** | Right-aligned | Left-aligned |
| **Examples** | StatusLabel, ProgressBar | StatusStripLabel, StatusStripButton |

**Important Note:** StatusControl items and Notification items offer the same types of controls (labels, buttons, progress bars, etc.). The main difference is their positioning within the StatusStripEx.

## Adding Items to StatusStripEx

### Through Smart Tags

1. **Select the StatusStripEx** control in the designer
2. **Click the smart tag arrow** (small arrow icon)
3. **Choose from quick add options**:
   - **StatusControl Items**: Add StatusLabel, ProgressBar, DropDownButton, SplitButton, PanelItem, TrackBarItem
   - **Notification Items**: Add StatusStripButton, StatusStripLabel, StatusStripProgressBar, StatusStripDropDownButton, StatusStripSplitButton, StatusStripPanelItem

### Through Items Collection Editor

1. **Select the StatusStripEx** control
2. **Click the Items property** in Properties window
3. **Click the ellipsis (...)** button
4. **Click Add** dropdown and select item type
5. **Configure properties** and click OK

### Through Code - Single Item

```csharp
// Add a single item
ToolStripStatusLabel label = new ToolStripStatusLabel("Ready");
this.statusStripEx1.Items.Add(label);
```

```vb
' Add a single item
Dim label As New ToolStripStatusLabel("Ready")
Me.statusStripEx1.Items.Add(label)
```

### Through Code - Multiple Items

```csharp
// Add multiple items at once
this.statusStripEx1.Items.AddRange(new ToolStripItem[]
{
    new ToolStripStatusLabel("Status"),
    new ToolStripProgressBar(),
    new ToolStripStatusLabel("Ready")
});
```

```vb
' Add multiple items at once
Me.statusStripEx1.Items.AddRange(New ToolStripItem() {
    New ToolStripStatusLabel("Status"),
    New ToolStripProgressBar(),
    New ToolStripStatusLabel("Ready")
})
```

## Item Positioning and Alignment

### Controlling Item Alignment

Use the `Alignment` property to control whether items appear on the left or right:

```csharp
// Explicitly set alignment
ToolStripStatusLabel label = new ToolStripStatusLabel("Custom Position");
label.Alignment = ToolStripItemAlignment.Right;  // Right side
this.statusStripEx1.Items.Add(label);
```

```vb
' Explicitly set alignment
Dim label As New ToolStripStatusLabel("Custom Position")
label.Alignment = ToolStripItemAlignment.Right  ' Right side
Me.statusStripEx1.Items.Add(label)
```

### Spring Property

Use the `Spring` property to make an item fill available space:

```csharp
// Create a label that fills available space
ToolStripStatusLabel springLabel = new ToolStripStatusLabel();
springLabel.Text = "Status Message";
springLabel.Spring = true;  // Fill available space
springLabel.TextAlign = ContentAlignment.MiddleLeft;

this.statusStripEx1.Items.Add(springLabel);
```

```vb
' Create a label that fills available space
Dim springLabel As New ToolStripStatusLabel()
springLabel.Text = "Status Message"
springLabel.Spring = True  ' Fill available space
springLabel.TextAlign = ContentAlignment.MiddleLeft

Me.statusStripEx1.Items.Add(springLabel)
```

### Item Spacing

Add separators or adjust margins for spacing:

```csharp
// Add a separator
ToolStripSeparator separator = new ToolStripSeparator();
this.statusStripEx1.Items.Add(separator);

// Adjust item margins
ToolStripStatusLabel label = new ToolStripStatusLabel("Spaced");
label.Margin = new Padding(10, 0, 10, 0);  // Left and right spacing
this.statusStripEx1.Items.Add(label);
```

```vb
' Add a separator
Dim separator As New ToolStripSeparator()
Me.statusStripEx1.Items.Add(separator)

' Adjust item margins
Dim label As New ToolStripStatusLabel("Spaced")
label.Margin = New Padding(10, 0, 10, 0)  ' Left and right spacing
Me.statusStripEx1.Items.Add(label)
```

## Complete Examples

### Example 1: Document Editor Status Bar

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class DocumentStatusBar
{
    private StatusStripEx statusStripEx1;
    private ToolStripStatusLabel pageLabel;
    private ToolStripStatusLabel wordCountLabel;
    private ToolStripStatusLabel lineColLabel;
    private StatusTrackBarItem zoomTrackBar;
    private StatusLabel zoomLabel;

    public void InitializeStatusBar(Form form)
    {
        // Create StatusStripEx
        this.statusStripEx1 = new StatusStripEx();
        
        // Notification items (left side)
        this.pageLabel = new ToolStripStatusLabel();
        this.pageLabel.Text = "Page: 1 of 10";
        this.pageLabel.StatusString = "1/10";
        this.pageLabel.BorderSides = ToolStripStatusLabelBorderSides.Right;
        
        this.wordCountLabel = new ToolStripStatusLabel();
        this.wordCountLabel.Text = "Words: 1,234";
        this.wordCountLabel.StatusString = "1,234";
        this.wordCountLabel.BorderSides = ToolStripStatusLabelBorderSides.Right;
        
        // StatusControl items (right side)
        this.lineColLabel = new ToolStripStatusLabel();
        this.lineColLabel.Text = "Ln 1, Col 1";
        this.lineColLabel.Alignment = ToolStripItemAlignment.Right;
        
        this.zoomTrackBar = new StatusTrackBarItem();
        this.zoomTrackBar.Minimum = 10;
        this.zoomTrackBar.Maximum = 500;
        this.zoomTrackBar.Value = 100;
        this.zoomTrackBar.TickFrequency = 50;
        this.zoomTrackBar.Width = 100;
        this.zoomTrackBar.Alignment = ToolStripItemAlignment.Right;
        
        this.zoomLabel = new StatusLabel();
        this.zoomLabel.Text = "100%";
        this.zoomLabel.Alignment = ToolStripItemAlignment.Right;
        
        // Add all items
        this.statusStripEx1.Items.AddRange(new ToolStripItem[]
        {
            this.pageLabel,
            this.wordCountLabel,
            this.lineColLabel,
            this.zoomTrackBar,
            this.zoomLabel
        });
        
        // Setup zoom event
        this.zoomTrackBar.ValueChanged += (s, e) =>
        {
            this.zoomLabel.Text = $"{this.zoomTrackBar.Value}%";
        };
        
        // Add to form
        this.statusStripEx1.Dock = DockStyleEx.Bottom;
        form.Controls.Add(this.statusStripEx1);
    }
    
    public void UpdatePosition(int line, int column)
    {
        this.lineColLabel.Text = $"Ln {line}, Col {column}";
    }
    
    public void UpdatePageInfo(int currentPage, int totalPages)
    {
        this.pageLabel.Text = $"Page: {currentPage} of {totalPages}";
        this.pageLabel.StatusString = $"{currentPage}/{totalPages}";
    }
}
```

### Example 2: Application with Progress Tracking

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class ProgressStatusBar
{
    private StatusStripEx statusStripEx1;
    private ToolStripStatusLabel mainStatus;
    private ToolStripProgressBar progressBar;
    private ToolStripStatusLabel timeLabel;
    private Timer updateTimer;

    public void InitializeStatusBar(Form form)
    {
        // Create StatusStripEx
        this.statusStripEx1 = new StatusStripEx();
        
        // Main status label with spring
        this.mainStatus = new ToolStripStatusLabel();
        this.mainStatus.Text = "Ready";
        this.mainStatus.Spring = true;
        this.mainStatus.TextAlign = ContentAlignment.MiddleLeft;
        
        // Progress bar (initially hidden)
        this.progressBar = new ToolStripProgressBar();
        this.progressBar.Size = new System.Drawing.Size(150, 16);
        this.progressBar.Visible = false;
        this.progressBar.Alignment = ToolStripItemAlignment.Right;
        
        // Time label
        this.timeLabel = new ToolStripStatusLabel();
        this.timeLabel.Text = DateTime.Now.ToString("HH:mm:ss");
        this.timeLabel.Alignment = ToolStripItemAlignment.Right;
        
        // Add items
        this.statusStripEx1.Items.AddRange(new ToolStripItem[]
        {
            this.mainStatus,
            this.progressBar,
            this.timeLabel
        });
        
        // Setup timer for time updates
        this.updateTimer = new Timer();
        this.updateTimer.Interval = 1000;
        this.updateTimer.Tick += (s, e) =>
        {
            this.timeLabel.Text = DateTime.Now.ToString("HH:mm:ss");
        };
        this.updateTimer.Start();
        
        // Add to form
        this.statusStripEx1.Dock = DockStyleEx.Bottom;
        form.Controls.Add(this.statusStripEx1);
    }
    
    public void ShowProgress(string message, int value)
    {
        this.mainStatus.Text = message;
        this.progressBar.Value = value;
        this.progressBar.Visible = true;
    }
    
    public void HideProgress(string message = "Ready")
    {
        this.progressBar.Visible = false;
        this.mainStatus.Text = message;
    }
    
    public void UpdateStatus(string message)
    {
        this.mainStatus.Text = message;
    }
}
```

## Best Practices

1. **Use StatusControl items** for overall application or document state information
2. **Use Notification items** for alerts, counters, and quick-access controls
3. **Set Spring property** on one label to fill available space
4. **Add borders** to labels using `BorderSides` for visual separation
5. **Use StatusString property** for Word-like context menu customization
6. **Hide progress bars** when not in use to save space
7. **Update time displays** using a Timer for real-time information
8. **Use Alignment property** when you need explicit control over positioning
9. **Add separators** between groups of items for better visual organization
10. **Keep item count reasonable** to avoid cluttering the status bar

## Summary

The StatusStripEx provides flexible item positioning with two categories:
- **StatusControl items** (right-aligned) for status information
- **Notification items** (left-aligned) for notifications and quick actions

Both categories offer the same control types with the main difference being their automatic positioning within the status bar.
