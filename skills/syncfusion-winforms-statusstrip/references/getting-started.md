# Getting Started with StatusStripEx

This guide covers the essential setup and initialization steps for implementing the Syncfusion StatusStripEx control in Windows Forms applications.

## Assembly and Namespace

### Required Assembly

To use StatusStripEx in your Windows Forms application, add a reference to the following assembly:

- **Syncfusion.Tools.Windows.dll**

### Required Namespace

Import the following namespace at the top of your code file:

```csharp
using Syncfusion.Windows.Forms.Tools;
```

```vb
Imports Syncfusion.Windows.Forms.Tools
```

## Adding StatusStripEx to a Form

The StatusStripEx control can be added to your Windows Forms application through the designer or programmatically through code.

### Adding Through Designer

1. **Open the Toolbox** in Visual Studio
2. **Locate the StatusStripEx control** in the Syncfusion Controls section
3. **Drag and drop** the StatusStripEx control onto your form
4. The control will be automatically added to the form's component tray

![StatusStripEx in Toolbox](../images/toolbox-statusstripex.png)

Once added, the control typically needs to be docked to the bottom of the form or RibbonControlAdv.

### Adding Through Code

You can create and configure a StatusStripEx control programmatically:

```csharp
using Syncfusion.Windows.Forms.Tools;
using System.Windows.Forms;

public partial class Form1 : Form
{
    private StatusStripEx statusStripEx1;

    public Form1()
    {
        InitializeComponent();
        InitializeStatusStrip();
    }

    private void InitializeStatusStrip()
    {
        // Create the StatusStripEx instance
        this.statusStripEx1 = new StatusStripEx();
        
        // Add the control to the form
        this.Controls.Add(this.statusStripEx1);
        
        // Configure docking (see Docking section below)
        this.statusStripEx1.Dock = DockStyleEx.Bottom;
    }
}
```

```vb
Imports Syncfusion.Windows.Forms.Tools
Imports System.Windows.Forms

Public Partial Class Form1
    Inherits Form
    
    Private statusStripEx1 As StatusStripEx
    
    Public Sub New()
        InitializeComponent()
        InitializeStatusStrip()
    End Sub
    
    Private Sub InitializeStatusStrip()
        ' Create the StatusStripEx instance
        Me.statusStripEx1 = New StatusStripEx()
        
        ' Add the control to the form
        Me.Controls.Add(Me.statusStripEx1)
        
        ' Configure docking (see Docking section below)
        Me.statusStripEx1.Dock = DockStyleEx.Bottom
    End Sub
End Class
```

## Docking the StatusStripEx

The StatusStripEx control is typically docked to the bottom of a form or positioned below a RibbonControlAdv. This ensures it remains fixed at the bottom edge regardless of form resizing.

### Docking Through Designer

1. **Select the StatusStripEx control** in the designer
2. **Locate the Dock property** in the Properties window
3. **Click the dropdown** on the Dock property
4. **Select Bottom** from the docking diagram

![Docking StatusStripEx](../images/dock-statusstripex.png)

### Docking Through Code

Set the `Dock` property to position the StatusStripEx at the bottom:

```csharp
// Dock to the bottom of the form
this.statusStripEx1.Dock = DockStyleEx.Bottom;
```

```vb
' Dock to the bottom of the form
Me.statusStripEx1.Dock = DockStyleEx.Bottom
```

### Docking with RibbonControlAdv

When using StatusStripEx with RibbonControlAdv, dock the StatusStripEx below the ribbon:

```csharp
// Assuming ribbonControlAdv1 is already docked to Top
this.ribbonControlAdv1.Dock = DockStyle.Top;

// Dock StatusStripEx to Bottom
this.statusStripEx1.Dock = DockStyleEx.Bottom;

// Add both controls to the form
this.Controls.Add(this.ribbonControlAdv1);
this.Controls.Add(this.statusStripEx1);
```

```vb
' Assuming ribbonControlAdv1 is already docked to Top
Me.ribbonControlAdv1.Dock = DockStyle.Top

' Dock StatusStripEx to Bottom
Me.statusStripEx1.Dock = DockStyleEx.Bottom

' Add both controls to the form
Me.Controls.Add(Me.ribbonControlAdv1)
Me.Controls.Add(Me.statusStripEx1)
```

## Items Collection

The StatusStripEx control uses an `Items` collection to hold various status bar items. This collection can contain status labels, progress bars, buttons, and other controls.

### Accessing the Items Collection

The Items collection is accessed through the `Items` property:

```csharp
// Access the Items collection
ToolStripItemCollection items = this.statusStripEx1.Items;
```

```vb
' Access the Items collection
Dim items As ToolStripItemCollection = Me.statusStripEx1.Items
```

### Adding Items Through Designer

1. **Select the StatusStripEx control** in the designer
2. **Click the Items property** in the Properties window (shows "Collection")
3. **Click the ellipsis button (...)** to open the Items Collection Editor
4. **Click Add** and select the type of item to add
5. **Configure item properties** on the right side of the editor
6. **Click OK** to close the editor

![Items Collection Editor](../images/items-collection-editor.png)

### Adding Items Through Code

Add items to the Items collection programmatically:

```csharp
using System.Windows.Forms;

// Create a ToolStripStatusLabel
ToolStripStatusLabel label = new ToolStripStatusLabel();
label.Text = "Ready";

// Add the label to the Items collection
this.statusStripEx1.Items.Add(label);
```

```vb
Imports System.Windows.Forms

' Create a ToolStripStatusLabel
Dim label As New ToolStripStatusLabel()
label.Text = "Ready"

' Add the label to the Items collection
Me.statusStripEx1.Items.Add(label)
```

### Adding Multiple Items at Once

Use `AddRange` to add multiple items in a single call:

```csharp
// Create multiple items
ToolStripStatusLabel statusLabel = new ToolStripStatusLabel("Ready");
ToolStripProgressBar progressBar = new ToolStripProgressBar();
progressBar.Value = 50;

// Add all items at once
this.statusStripEx1.Items.AddRange(new ToolStripItem[] 
{
    statusLabel,
    progressBar
});
```

```vb
' Create multiple items
Dim statusLabel As New ToolStripStatusLabel("Ready")
Dim progressBar As New ToolStripProgressBar()
progressBar.Value = 50

' Add all items at once
Me.statusStripEx1.Items.AddRange(New ToolStripItem() {
    statusLabel,
    progressBar
})
```

### Removing Items from the Collection

Remove items from the Items collection when they're no longer needed:

```csharp
// Remove a specific item
this.statusStripEx1.Items.Remove(statusLabel);

// Remove an item by index
this.statusStripEx1.Items.RemoveAt(0);

// Clear all items
this.statusStripEx1.Items.Clear();
```

```vb
' Remove a specific item
Me.statusStripEx1.Items.Remove(statusLabel)

' Remove an item by index
Me.statusStripEx1.Items.RemoveAt(0)

' Clear all items
Me.statusStripEx1.Items.Clear()
```

## Complete Example: Creating a Basic StatusStripEx

Here's a complete example that demonstrates creating a StatusStripEx with several items:

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace StatusStripExample
{
    public partial class MainForm : Form
    {
        private StatusStripEx statusStripEx1;
        private ToolStripStatusLabel statusLabel;
        private ToolStripProgressBar progressBar;
        private ToolStripStatusLabel dateTimeLabel;

        public MainForm()
        {
            InitializeComponent();
            InitializeStatusStrip();
        }

        private void InitializeStatusStrip()
        {
            // Create StatusStripEx
            this.statusStripEx1 = new StatusStripEx();
            
            // Create status label
            this.statusLabel = new ToolStripStatusLabel();
            this.statusLabel.Text = "Ready";
            this.statusLabel.Spring = true;
            this.statusLabel.TextAlign = ContentAlignment.MiddleLeft;
            
            // Create progress bar
            this.progressBar = new ToolStripProgressBar();
            this.progressBar.Size = new System.Drawing.Size(100, 16);
            this.progressBar.Visible = false;
            
            // Create date/time label
            this.dateTimeLabel = new ToolStripStatusLabel();
            this.dateTimeLabel.Text = DateTime.Now.ToString("HH:mm:ss");
            
            // Add items to StatusStripEx
            this.statusStripEx1.Items.AddRange(new ToolStripItem[] 
            {
                this.statusLabel,
                this.progressBar,
                this.dateTimeLabel
            });
            
            // Dock to bottom
            this.statusStripEx1.Dock = DockStyleEx.Bottom;
            
            // Add to form
            this.Controls.Add(this.statusStripEx1);
        }
        
        // Method to update status
        public void UpdateStatus(string message)
        {
            this.statusLabel.Text = message;
        }
        
        // Method to show progress
        public void ShowProgress(bool show)
        {
            this.progressBar.Visible = show;
            if (show)
            {
                this.progressBar.Value = 0;
            }
        }
    }
}
```

```vb
Imports System
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Namespace StatusStripExample
    Public Partial Class MainForm
        Inherits Form
        
        Private statusStripEx1 As StatusStripEx
        Private statusLabel As ToolStripStatusLabel
        Private progressBar As ToolStripProgressBar
        Private dateTimeLabel As ToolStripStatusLabel
        
        Public Sub New()
            InitializeComponent()
            InitializeStatusStrip()
        End Sub
        
        Private Sub InitializeStatusStrip()
            ' Create StatusStripEx
            Me.statusStripEx1 = New StatusStripEx()
            
            ' Create status label
            Me.statusLabel = New ToolStripStatusLabel()
            Me.statusLabel.Text = "Ready"
            Me.statusLabel.Spring = True
            Me.statusLabel.TextAlign = ContentAlignment.MiddleLeft
            
            ' Create progress bar
            Me.progressBar = New ToolStripProgressBar()
            Me.progressBar.Size = New System.Drawing.Size(100, 16)
            Me.progressBar.Visible = False
            
            ' Create date/time label
            Me.dateTimeLabel = New ToolStripStatusLabel()
            Me.dateTimeLabel.Text = DateTime.Now.ToString("HH:mm:ss")
            
            ' Add items to StatusStripEx
            Me.statusStripEx1.Items.AddRange(New ToolStripItem() {
                Me.statusLabel,
                Me.progressBar,
                Me.dateTimeLabel
            })
            
            ' Dock to bottom
            Me.statusStripEx1.Dock = DockStyleEx.Bottom
            
            ' Add to form
            Me.Controls.Add(Me.statusStripEx1)
        End Sub
        
        ' Method to update status
        Public Sub UpdateStatus(message As String)
            Me.statusLabel.Text = message
        End Sub
        
        ' Method to show progress
        Public Sub ShowProgress(show As Boolean)
            Me.progressBar.Visible = show
            If show Then
                Me.progressBar.Value = 0
            End If
        End Sub
    End Class
End Namespace
```

## Smart Tag Support

The StatusStripEx control provides Smart Tag support for quick configuration through the designer.

### Accessing Smart Tags

1. **Select the StatusStripEx control** in the designer
2. **Click the small arrow** (smart tag glyph) that appears in the top-right corner
3. **The Tasks window** will display available options

### Smart Tag Options

The Tasks window provides quick access to:

- **Dock** - Change docking position (Top, Bottom, Left, Right, Fill, None)
- **Add StatusControl items** - Quick buttons to add StatusLabel, ProgressBar, DropDownButton, SplitButton, PanelItem, and TrackBarItem
- **Add Notification items** - Quick buttons to add StatusStripButton, StatusStripLabel, StatusStripProgressBar, StatusStripDropDownButton, StatusStripSplitButton, and StatusStripPanelItem

### Using Smart Tags to Add Items

Click any of the "Add" buttons in the Smart Tag Tasks window to quickly add items to the StatusStripEx without opening the Items Collection Editor.

## Layout Considerations

### Z-Order with Other Controls

When adding StatusStripEx to a form that contains other docked controls, pay attention to the z-order:

```csharp
// Correct order: Add in reverse of desired visual order
this.Controls.Add(this.statusStripEx1);  // Last (bottom)
this.Controls.Add(this.ribbonControlAdv1);  // First (top)
```

```vb
' Correct order: Add in reverse of desired visual order
Me.Controls.Add(Me.statusStripEx1)  ' Last (bottom)
Me.Controls.Add(Me.ribbonControlAdv1)  ' First (top)
```

### Sizing

The StatusStripEx automatically adjusts its height based on its content. The width spans the entire width of its container (when docked to Top or Bottom).

```csharp
// StatusStripEx automatically sizes, but you can set explicit size if needed
this.statusStripEx1.AutoSize = false;
this.statusStripEx1.Height = 30;
```

```vb
' StatusStripEx automatically sizes, but you can set explicit size if needed
Me.statusStripEx1.AutoSize = False
Me.statusStripEx1.Height = 30
```

## Common Properties

Here are some commonly used properties for basic configuration:

| Property | Type | Description |
|----------|------|-------------|
| `Dock` | `DockStyleEx` | Gets or sets which edge of the parent container the control is docked to |
| `Items` | `ToolStripItemCollection` | Gets the collection of items contained in the StatusStripEx |
| `AutoSize` | `bool` | Gets or sets whether the control automatically resizes based on its contents |
| `Height` | `int` | Gets or sets the height of the control |
| `SizingGrip` | `bool` | Gets or sets whether a sizing grip is displayed in the lower-right corner |
| `Visible` | `bool` | Gets or sets whether the control is visible |

## Troubleshooting

### StatusStripEx Not Appearing

If the StatusStripEx doesn't appear after adding it:

1. **Check the Visible property** - Ensure it's set to `true`
2. **Verify docking** - Make sure the Dock property is set appropriately
3. **Check z-order** - Ensure other controls aren't covering it
4. **Add items** - An empty StatusStripEx may be very thin; add at least one item to make it visible

### Items Not Displaying

If items don't appear in the StatusStripEx:

1. **Check Visible property** of each item
2. **Verify items were added** to the Items collection
3. **Check item sizing** - Some items may have zero width/height
4. **Inspect layout** - Use LayoutStyle property to control item arrangement

## Next Steps

Now that you have a basic StatusStripEx set up, explore these related topics:

- **Status Items** - Learn about the different types of items you can add to StatusStripEx
- **Sizing Grip** - Configure the sizing grip for resizable windows
- **Styling and Appearance** - Customize the visual style to match your application theme
