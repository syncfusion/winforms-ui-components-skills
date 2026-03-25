# Getting Started with PopupControlContainer

This guide covers the essential steps to set up and use the PopupControlContainer control in Windows Forms applications, including assembly deployment, adding controls via designer or code, and basic popup operations.

## Assembly Deployment

### Required Assemblies

The PopupControlContainer control requires the following assemblies:
- **Syncfusion.Shared.Base.dll** - Core shared components
- **Syncfusion.Licensing.dll** - License management

### NuGet Package Installation

**Using NuGet Package Manager:**

1. Right-click on your project in Solution Explorer
2. Select **Manage NuGet Packages**
3. Search for `Syncfusion.Tools.WinForms` or `Syncfusion.Shared.WinForms`
4. Click **Install**
5. Accept the license agreement

**Using Package Manager Console:**

```powershell
Install-Package Syncfusion.Tools.Windows
```

### Manual Assembly Reference

If not using NuGet:

1. Right-click **References** in Solution Explorer
2. Select **Add Reference**
3. Browse to Syncfusion installation folder (typically `C:\Program Files (x86)\Syncfusion\Essential Studio\Windows\{version}\Assemblies`)
4. Select `Syncfusion.Shared.Base.dll` and `Syncfusion.Licensing.dll`
5. Click **OK**

## Adding Control via Designer

### Step 1: Add PopupControlContainer to Form

1. Open your Windows Form in **Design View**
2. Open the **Toolbox** (View > Toolbox or Ctrl+Alt+X)
3. Locate **PopupControlContainer** under the Syncfusion Controls section
4. Drag and drop it onto the form

The required assembly references are added automatically when you drag the control from the toolbox.

### Step 2: Add Child Controls

1. Select the PopupControlContainer in the designer
2. Add child controls from the Toolbox (e.g., Button, Label, TextBox, ColorPicker)
3. Position and configure the child controls as needed

**Example:** Adding a Button as a child control:

```
PopupControlContainer
└── Button1 (Text: "PopupControlContainer", Size: 174x35)
```

### Step 3: Create Parent Control

1. Drag a control (e.g., RichTextBox, Button, Label) onto the form
2. This will be the control that triggers the popup

### Step 4: Associate Parent Control

1. Select the **PopupControlContainer** in the designer
2. In the **Properties** panel, find the **ParentControl** property
3. Select the parent control from the dropdown (e.g., richTextBox1)

### Step 5: Handle Parent Control Event

1. Select the parent control (e.g., RichTextBox)
2. In the Properties panel, click the **Events** button (lightning bolt icon)
3. Double-click the **Click** event to generate an event handler
4. Add the ShowPopup call:

```csharp
private void richTextBox1_Click(object sender, EventArgs e)
{
    this.popupControlContainer1.ShowPopup(Point.Empty);
}
```

## Adding Control Manually in Code

### Step 1: Add Namespace

Add the Syncfusion namespace to your form class:

```csharp
using Syncfusion.Windows.Forms;
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms
```

### Step 2: Declare Controls

Declare the controls at the class level:

```csharp
private Syncfusion.Windows.Forms.PopupControlContainer popupControlContainer1;
private System.Windows.Forms.Button button1;
private System.Windows.Forms.RichTextBox richTextBox1;
```

**VB.NET:**
```vb
Private popupControlContainer1 As Syncfusion.Windows.Forms.PopupControlContainer
Private button1 As System.Windows.Forms.Button
Private richTextBox1 As System.Windows.Forms.RichTextBox
```

### Step 3: Initialize and Configure Controls

In the form constructor or initialization method:

```csharp
// Initialize PopupControlContainer
this.popupControlContainer1 = new Syncfusion.Windows.Forms.PopupControlContainer();

// Initialize child control (button)
this.button1 = new System.Windows.Forms.Button();
this.button1.Location = new System.Drawing.Point(13, 29);
this.button1.Name = "button1";
this.button1.Size = new System.Drawing.Size(174, 35);
this.button1.Text = "PopupControlContainer";

// Add child control to popup
this.popupControlContainer1.Controls.Add(this.button1);

// Configure popup properties
this.popupControlContainer1.Location = new System.Drawing.Point(33, 58);
this.popupControlContainer1.Name = "popupControlContainer1";
this.popupControlContainer1.Size = new System.Drawing.Size(200, 100);

// Initialize parent control (RichTextBox)
this.richTextBox1 = new System.Windows.Forms.RichTextBox();
this.richTextBox1.Location = new System.Drawing.Point(12, 12);
this.richTextBox1.Name = "richTextBox1";
this.richTextBox1.Size = new System.Drawing.Size(100, 96);
this.richTextBox1.Click += RichTextBox1_Click;

// Associate popup with parent control
this.popupControlContainer1.ParentControl = this.richTextBox1;

// Add parent control to form
this.Controls.Add(this.richTextBox1);
```

**VB.NET:**
```vb
' Initialize PopupControlContainer
Me.popupControlContainer1 = New Syncfusion.Windows.Forms.PopupControlContainer()

' Initialize child control (button)
Me.button1 = New System.Windows.Forms.Button()
Me.button1.Location = New System.Drawing.Point(13, 29)
Me.button1.Name = "button1"
Me.button1.Size = New System.Drawing.Size(174, 35)
Me.button1.Text = "PopupControlContainer"

' Add child control to popup
Me.popupControlContainer1.Controls.Add(Me.button1)

' Configure popup properties
Me.popupControlContainer1.Location = New System.Drawing.Point(33, 58)
Me.popupControlContainer1.Name = "popupControlContainer1"
Me.popupControlContainer1.Size = New System.Drawing.Size(200, 100)

' Initialize parent control (RichTextBox)
Me.richTextBox1 = New System.Windows.Forms.RichTextBox()
Me.richTextBox1.Location = New System.Drawing.Point(12, 12)
Me.richTextBox1.Name = "richTextBox1"
Me.richTextBox1.Size = New System.Drawing.Size(100, 96)
AddHandler Me.richTextBox1.Click, AddressOf RichTextBox1_Click

' Associate popup with parent control
Me.popupControlContainer1.ParentControl = Me.richTextBox1

' Add parent control to form
Me.Controls.Add(Me.richTextBox1)
```

### Step 4: Implement Event Handler

```csharp
private void RichTextBox1_Click(object sender, EventArgs e)
{
    this.popupControlContainer1.ShowPopup(Point.Empty);
}
```

**VB.NET:**
```vb
Private Sub RichTextBox1_Click(sender As Object, e As EventArgs)
    Me.popupControlContainer1.ShowPopup(Point.Empty)
End Sub
```

## Show or Hide Popup

### ShowPopup Method

The `ShowPopup` method displays the popup at a specified screen coordinate position.

**Syntax:**
```csharp
public void ShowPopup(Point screenCoordinates)
```

**Show at default position:**
```csharp
this.popupControlContainer1.ShowPopup(Point.Empty);
```

**Show at specific screen coordinates:**
```csharp
this.popupControlContainer1.ShowPopup(new Point(100, 200));
```

**Show below parent control:**
```csharp
// Convert parent control location to screen coordinates
Point parentLocation = this.richTextBox1.PointToScreen(new Point(0, 0));
// Show popup below the parent
this.popupControlContainer1.ShowPopup(
    new Point(parentLocation.X, parentLocation.Y + this.richTextBox1.Height));
```

**Show at mouse cursor position:**
```csharp
this.popupControlContainer1.ShowPopup(Cursor.Position);
```

### HidePopup Method

The `HidePopup` method closes the popup.

**Syntax:**
```csharp
public void HidePopup()
public void HidePopup(PopupCloseType closeType)
```

**Simple hide:**
```csharp
this.popupControlContainer1.HidePopup();
```

**Hide with close type:**
```csharp
// Close with changes applied
this.popupControlContainer1.HidePopup(Syncfusion.Windows.Forms.PopupCloseType.Done);

// Close without applying changes
this.popupControlContainer1.HidePopup(Syncfusion.Windows.Forms.PopupCloseType.Canceled);

// Close when deactivated
this.popupControlContainer1.HidePopup(Syncfusion.Windows.Forms.PopupCloseType.Deactivated);
```

## Complete Working Examples

### Example 1: Basic Popup with Button

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;

public partial class Form1 : Form
{
    private PopupControlContainer popupControlContainer1;
    private Button button1;
    private Button triggerButton;

    public Form1()
    {
        InitializeComponent();
        
        // Create popup container
        this.popupControlContainer1 = new PopupControlContainer();
        this.popupControlContainer1.Size = new Size(200, 100);
        
        // Create child button
        this.button1 = new Button();
        this.button1.Text = "Click Me!";
        this.button1.Size = new Size(150, 40);
        this.button1.Location = new Point(25, 30);
        this.button1.Click += Button1_Click;
        this.popupControlContainer1.Controls.Add(this.button1);
        
        // Create trigger button
        this.triggerButton = new Button();
        this.triggerButton.Text = "Show Popup";
        this.triggerButton.Size = new Size(120, 30);
        this.triggerButton.Location = new Point(50, 50);
        this.triggerButton.Click += TriggerButton_Click;
        
        // Associate and add to form
        this.popupControlContainer1.ParentControl = this.triggerButton;
        this.Controls.Add(this.triggerButton);
    }
    
    private void TriggerButton_Click(object sender, EventArgs e)
    {
        this.popupControlContainer1.ShowPopup(Point.Empty);
    }
    
    private void Button1_Click(object sender, EventArgs e)
    {
        MessageBox.Show("Popup button clicked!");
        this.popupControlContainer1.HidePopup();
    }
}
```

### Example 2: Popup with Data Entry

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;

public partial class Form2 : Form
{
    private PopupControlContainer popupControlContainer1;
    private TextBox popupTextBox;
    private Button okButton;
    private Button cancelButton;
    private TextBox displayTextBox;

    public Form2()
    {
        InitializeComponent();
        
        // Create display textbox (parent)
        this.displayTextBox = new TextBox();
        this.displayTextBox.Size = new Size(200, 20);
        this.displayTextBox.Location = new Point(20, 20);
        this.displayTextBox.ReadOnly = true;
        this.displayTextBox.Click += DisplayTextBox_Click;
        this.Controls.Add(this.displayTextBox);
        
        // Create popup container
        this.popupControlContainer1 = new PopupControlContainer();
        this.popupControlContainer1.Size = new Size(220, 100);
        this.popupControlContainer1.ParentControl = this.displayTextBox;
        
        // Add controls to popup
        this.popupTextBox = new TextBox();
        this.popupTextBox.Size = new Size(180, 20);
        this.popupTextBox.Location = new Point(10, 10);
        
        this.okButton = new Button();
        this.okButton.Text = "OK";
        this.okButton.Size = new Size(75, 25);
        this.okButton.Location = new Point(10, 50);
        this.okButton.Click += OkButton_Click;
        
        this.cancelButton = new Button();
        this.cancelButton.Text = "Cancel";
        this.cancelButton.Size = new Size(75, 25);
        this.cancelButton.Location = new Point(95, 50);
        this.cancelButton.Click += CancelButton_Click;
        
        this.popupControlContainer1.Controls.Add(this.popupTextBox);
        this.popupControlContainer1.Controls.Add(this.okButton);
        this.popupControlContainer1.Controls.Add(this.cancelButton);
    }
    
    private void DisplayTextBox_Click(object sender, EventArgs e)
    {
        this.popupTextBox.Text = this.displayTextBox.Text;
        this.popupControlContainer1.ShowPopup(Point.Empty);
        this.popupTextBox.Focus();
    }
    
    private void OkButton_Click(object sender, EventArgs e)
    {
        this.displayTextBox.Text = this.popupTextBox.Text;
        this.popupControlContainer1.HidePopup(PopupCloseType.Done);
    }
    
    private void CancelButton_Click(object sender, EventArgs e)
    {
        this.popupControlContainer1.HidePopup(PopupCloseType.Canceled);
    }
}
```

## Key Takeaways

1. **Assembly Reference:** Always include `Syncfusion.Shared.Base.dll` either via NuGet or manual reference
2. **Parent Association:** Set the `ParentControl` property to associate the popup with a trigger control
3. **ShowPopup:** Call `ShowPopup(Point)` to display the popup at screen coordinates
4. **HidePopup:** Call `HidePopup()` to close the popup, optionally with a close type
5. **Event Handling:** Use parent control events (Click, MouseDown, etc.) to trigger popup display
6. **Child Controls:** Add any standard or custom controls to the popup's Controls collection

## Next Steps

- Configure auto-close behavior: [auto-close-behavior.md](auto-close-behavior.md)
- Handle popup lifecycle events: [events.md](events.md)
- Enable auto-scroll for large content: [autoscroll.md](autoscroll.md)
- Set up keyboard navigation: [keyboard-navigation.md](keyboard-navigation.md)
