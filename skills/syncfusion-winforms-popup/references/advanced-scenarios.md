# Advanced Scenarios

## Table of Contents
- [Overview](#overview)
- [Hosting ComboBoxBase Control](#hosting-comboboxbase-control)
- [Creating Transparent Popups](#creating-transparent-popups)
- [Checking Popup State](#checking-popup-state)
- [BeforeCloseUp Workaround](#beforecloseup-workaround)
- [Troubleshooting Tips](#troubleshooting-tips)

## Overview

This guide covers advanced scenarios and edge cases when working with PopupControlContainer, including hosting complex controls like ComboBoxBase, creating transparent popups, checking popup visibility state, and handling special close behaviors.

## Hosting ComboBoxBase Control

### The Problem

When you place a ComboBoxBase control (or similar dropdown controls) within a PopupControlContainer, the popup closes prematurely when the ComboBoxBase's dropdown is displayed. This happens because:

1. The ComboBoxBase displays its own dropdown popup
2. The PopupControlContainer loses focus
3. The PopupControlContainer automatically closes due to focus loss

### The Solution

To prevent premature closing, you need to:
1. Create a custom PopupControlContainer that overrides `OnPopup()`
2. Set up the parent-child relationship between the ComboBoxBase dropdown and PopupControlContainer

### Step 1: Create Custom PopupControlContainer

Create a derived class that overrides the `OnPopup` method to maintain focus:

```csharp
using Syncfusion.Windows.Forms;

public class CustomPopupControlContainer : Syncfusion.Windows.Forms.PopupControlContainer
{
    public CustomPopupControlContainer()
    {
    }
    
    public CustomPopupControlContainer(IContainer container) : this()
    {
        container.Add(this);
    }
    
    protected override void OnPopup(EventArgs args)
    {
        base.OnPopup(args);
        
        // Set focus back to the popup to prevent premature closing
        this.Focus();
    }
}
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms

Public Class CustomPopupControlContainer
    Inherits Syncfusion.Windows.Forms.PopupControlContainer
    
    Public Sub New()
    End Sub
    
    Public Sub New(ByVal container As IContainer)
        MyBase.New()
        container.Add(Me)
    End Sub
    
    Protected Overrides Sub OnPopup(ByVal args As EventArgs)
        MyBase.OnPopup(args)
        
        ' Set focus back to the popup to prevent premature closing
        Me.Focus()
    End Sub
End Class
```

### Step 2: Set Up Parent-Child Relationship

Handle the ComboBoxBase's `DropDown` event to establish the relationship:

```csharp
using System;
using System.ComponentModel;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class ComboBoxHostingForm : Form
{
    private CustomPopupControlContainer popupControlContainer1;
    private ComboBoxBase comboBoxBase1;
    private RichTextBox richTextBox1;

    public ComboBoxHostingForm()
    {
        InitializeComponent();
        InitializeControls();
    }

    private void InitializeControls()
    {
        // Create parent control (RichTextBox)
        this.richTextBox1 = new RichTextBox();
        this.richTextBox1.Location = new Point(12, 12);
        this.richTextBox1.Name = "richTextBox1";
        this.richTextBox1.Size = new Size(100, 96);
        this.richTextBox1.Click += RichTextBox1_Click;
        this.Controls.Add(this.richTextBox1);

        // Create custom popup container
        this.popupControlContainer1 = new CustomPopupControlContainer();
        this.popupControlContainer1.Location = new Point(33, 58);
        this.popupControlContainer1.Name = "popupControlContainer1";
        this.popupControlContainer1.ParentControl = this.richTextBox1;
        this.popupControlContainer1.Size = new Size(200, 100);

        // Create ComboBoxBase
        this.comboBoxBase1 = new ComboBoxBase();
        this.comboBoxBase1.Location = new Point(29, 28);
        this.comboBoxBase1.Name = "comboBoxBase1";
        this.comboBoxBase1.Size = new Size(121, 24);
        this.comboBoxBase1.Text = "comboBoxBase1";
        
        // Add items to ComboBoxBase
        this.comboBoxBase1.Items.AddRange(new object[] 
        { 
            "Option 1", "Option 2", "Option 3" 
        });
        
        // Handle DropDown event - CRITICAL for preventing premature close
        this.comboBoxBase1.DropDown += ComboBoxBase1_DropDown;

        // Add ComboBoxBase to popup
        this.popupControlContainer1.Controls.Add(this.comboBoxBase1);
    }

    private void RichTextBox1_Click(object sender, EventArgs e)
    {
        this.popupControlContainer1.ShowPopup(Point.Empty);
    }

    private void ComboBoxBase1_DropDown(object sender, EventArgs e)
    {
        // Setup the relationship between the ComboBoxBase's dropdown and its parent
        // PopupControlContainer, so that the popup will not close when the 
        // ComboBoxBase's dropdown is shown
        this.comboBoxBase1.PopupContainer.PopupParent = this.popupControlContainer1;
        this.popupControlContainer1.CurrentPopupChild = this.comboBoxBase1.PopupContainer;
    }
}
```

**VB.NET:**
```vb
Public Partial Class ComboBoxHostingForm
    Inherits Form
    
    Private popupControlContainer1 As CustomPopupControlContainer
    Private comboBoxBase1 As ComboBoxBase
    Private richTextBox1 As RichTextBox
    
    Public Sub New()
        InitializeComponent()
        InitializeControls()
    End Sub
    
    Private Sub InitializeControls()
        ' Create parent control (RichTextBox)
        Me.richTextBox1 = New RichTextBox()
        Me.richTextBox1.Location = New Point(12, 12)
        Me.richTextBox1.Name = "richTextBox1"
        Me.richTextBox1.Size = New Size(100, 96)
        AddHandler Me.richTextBox1.Click, AddressOf RichTextBox1_Click
        Me.Controls.Add(Me.richTextBox1)
        
        ' Create custom popup container
        Me.popupControlContainer1 = New CustomPopupControlContainer()
        Me.popupControlContainer1.Location = New Point(33, 58)
        Me.popupControlContainer1.Name = "popupControlContainer1"
        Me.popupControlContainer1.ParentControl = Me.richTextBox1
        Me.popupControlContainer1.Size = New Size(200, 100)
        
        ' Create ComboBoxBase
        Me.comboBoxBase1 = New ComboBoxBase()
        Me.comboBoxBase1.Location = New Point(29, 28)
        Me.comboBoxBase1.Name = "comboBoxBase1"
        Me.comboBoxBase1.Size = New Size(121, 24)
        Me.comboBoxBase1.Text = "comboBoxBase1"
        
        ' Add items to ComboBoxBase
        Me.comboBoxBase1.Items.AddRange(New Object() {"Option 1", "Option 2", "Option 3"})
        
        ' Handle DropDown event - CRITICAL for preventing premature close
        AddHandler Me.comboBoxBase1.DropDown, AddressOf ComboBoxBase1_DropDown
        
        ' Add ComboBoxBase to popup
        Me.popupControlContainer1.Controls.Add(Me.comboBoxBase1)
    End Sub
    
    Private Sub RichTextBox1_Click(sender As Object, e As EventArgs)
        Me.popupControlContainer1.ShowPopup(Point.Empty)
    End Sub
    
    Private Sub ComboBoxBase1_DropDown(sender As Object, e As EventArgs)
        ' Setup the relationship between the ComboBoxBase's dropdown and its parent
        ' PopupControlContainer, so that the popup will not close when the 
        ' ComboBoxBase's dropdown is shown
        Me.comboBoxBase1.PopupContainer.PopupParent = Me.popupControlContainer1
        Me.popupControlContainer1.CurrentPopupChild = Me.comboBoxBase1.PopupContainer
    End Sub
End Class
```

### Key Points

1. **Custom class required:** You must derive from PopupControlContainer
2. **Override OnPopup:** Set focus back to prevent premature closing
3. **DropDown event:** Establish parent-child relationship in this event
4. **PopupContainer property:** Access ComboBoxBase's internal popup via this property
5. **CurrentPopupChild:** Set this to inform PopupControlContainer about the nested popup

### Alternative: Standard ComboBox

If you're using standard Windows Forms ComboBox instead of ComboBoxBase, you don't need the custom class, but you may still need to handle focus carefully:

```csharp
this.popupControlContainer1.IgnoreMouseMessages = true;

private void ComboBox1_DropDown(object sender, EventArgs e)
{
    // Prevent popup from closing during dropdown
    this.popupControlContainer1.Focus();
}
```

## Creating Transparent Popups

### Basic Transparency

Set the popup's opacity to create a transparent effect:

```csharp
private void PopupControlContainer1_BeforePopup(object sender, CancelEventArgs e)
{
    // Set opacity (0.0 = fully transparent, 1.0 = fully opaque)
    this.popupControlContainer1.PopupHost.Opacity = 0.75;
}
```

**VB.NET:**
```vb
Private Sub PopupControlContainer1_BeforePopup(sender As Object, e As CancelEventArgs)
    ' Set opacity
    Me.popupControlContainer1.PopupHost.Opacity = 0.75
End Sub
```

### Transparency Levels

```csharp
// Common opacity values
this.popupControlContainer1.PopupHost.Opacity = 1.0;   // Fully opaque (default)
this.popupControlContainer1.PopupHost.Opacity = 0.9;   // Slightly transparent
this.popupControlContainer1.PopupHost.Opacity = 0.75;  // Semi-transparent
this.popupControlContainer1.PopupHost.Opacity = 0.5;   // Half transparent
this.popupControlContainer1.PopupHost.Opacity = 0.25;  // Highly transparent
```

### Complete Transparency Example

```csharp
using System;
using System.ComponentModel;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;

public partial class TransparentPopupForm : Form
{
    private PopupControlContainer popupControlContainer1;
    private Panel contentPanel;
    private Button showButton;
    private TrackBar opacityTrackBar;

    public TransparentPopupForm()
    {
        InitializeComponent();
        InitializeControls();
    }

    private void InitializeControls()
    {
        // Create opacity control
        Label label = new Label();
        label.Text = "Opacity:";
        label.Location = new Point(20, 20);
        label.AutoSize = true;
        this.Controls.Add(label);

        this.opacityTrackBar = new TrackBar();
        this.opacityTrackBar.Location = new Point(80, 15);
        this.opacityTrackBar.Size = new Size(200, 45);
        this.opacityTrackBar.Minimum = 10;  // 10% opacity
        this.opacityTrackBar.Maximum = 100; // 100% opacity
        this.opacityTrackBar.Value = 75;    // Default 75%
        this.Controls.Add(this.opacityTrackBar);

        // Create show button
        this.showButton = new Button();
        this.showButton.Text = "Show Transparent Popup";
        this.showButton.Location = new Point(20, 70);
        this.showButton.Size = new Size(180, 30);
        this.showButton.Click += ShowButton_Click;
        this.Controls.Add(this.showButton);

        // Create popup
        this.popupControlContainer1 = new PopupControlContainer();
        this.popupControlContainer1.Size = new Size(300, 200);
        this.popupControlContainer1.ParentControl = this.showButton;
        this.popupControlContainer1.BeforePopup += PopupControlContainer1_BeforePopup;

        // Add content to popup
        this.contentPanel = new Panel();
        this.contentPanel.Dock = DockStyle.Fill;
        this.contentPanel.BackColor = Color.LightBlue;

        Label popupLabel = new Label();
        popupLabel.Text = "This is a transparent popup!";
        popupLabel.Font = new Font("Arial", 14, FontStyle.Bold);
        popupLabel.ForeColor = Color.Navy;
        popupLabel.AutoSize = true;
        popupLabel.Location = new Point(50, 80);
        this.contentPanel.Controls.Add(popupLabel);

        this.popupControlContainer1.Controls.Add(this.contentPanel);
    }

    private void ShowButton_Click(object sender, EventArgs e)
    {
        this.popupControlContainer1.ShowPopup(Point.Empty);
    }

    private void PopupControlContainer1_BeforePopup(object sender, CancelEventArgs e)
    {
        // Set opacity based on trackbar value
        double opacity = this.opacityTrackBar.Value / 100.0;
        this.popupControlContainer1.PopupHost.Opacity = opacity;
        
        // Optional: Style the popup
        this.popupControlContainer1.PopupHost.FormBorderStyle = FormBorderStyle.None;
    }
}
```

### Transparency Best Practices

1. **Readability:** Maintain opacity ≥ 0.70 for readable text
2. **Purpose:** Use transparency for overlay effects, not primary content
3. **Performance:** Transparency can impact rendering performance
4. **Accessibility:** Ensure sufficient contrast for visually impaired users
5. **Context:** Works best over solid backgrounds

## Checking Popup State

### IsShowing Method

Use the `IsShowing()` method to check if the popup is currently visible:

```csharp
public bool IsShowing()
```

**Returns:** `true` if popup is visible, `false` otherwise

### Basic Usage

```csharp
if (this.popupControlContainer1.IsShowing())
{
    Console.WriteLine("Popup is currently displayed");
}
else
{
    Console.WriteLine("Popup is hidden");
}
```

**VB.NET:**
```vb
If Me.popupControlContainer1.IsShowing() Then
    Console.WriteLine("Popup is currently displayed")
Else
    Console.WriteLine("Popup is hidden")
End If
```

### Practical Examples

#### Example 1: Toggle Popup

```csharp
private void ToggleButton_Click(object sender, EventArgs e)
{
    if (this.popupControlContainer1.IsShowing())
    {
        this.popupControlContainer1.HidePopup();
        this.toggleButton.Text = "Show Popup";
    }
    else
    {
        this.popupControlContainer1.ShowPopup(Point.Empty);
        this.toggleButton.Text = "Hide Popup";
    }
}
```

#### Example 2: Prevent Multiple Opens

```csharp
private void ShowPopupButton_Click(object sender, EventArgs e)
{
    // Only show if not already visible
    if (!this.popupControlContainer1.IsShowing())
    {
        this.popupControlContainer1.ShowPopup(Point.Empty);
    }
}
```

#### Example 3: Update UI Based on State

```csharp
private void Timer1_Tick(object sender, EventArgs e)
{
    // Update status indicator
    if (this.popupControlContainer1.IsShowing())
    {
        this.statusLabel.Text = "Popup: Visible";
        this.statusLabel.ForeColor = Color.Green;
    }
    else
    {
        this.statusLabel.Text = "Popup: Hidden";
        this.statusLabel.ForeColor = Color.Gray;
    }
}
```

#### Example 4: Conditional Actions

```csharp
private void PerformAction()
{
    if (this.popupControlContainer1.IsShowing())
    {
        // Save popup data before performing action
        SavePopupData();
        this.popupControlContainer1.HidePopup();
    }
    
    // Proceed with action
    ExecuteMainAction();
}
```

## BeforeCloseUp Workaround

### The Issue

When the `BeforeCloseUp` event is used to cancel popup closing, certain scenarios might require a workaround to properly manage the close behavior, especially with focus-sensitive controls.

### The Workaround

Use a Boolean flag to control the cancel behavior:

```csharp
using System;
using System.ComponentModel;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class BeforeCloseUpForm : Form
{
    private PopupControlContainer popupContainer;
    private ComboDropDown comboDropDown1;
    private bool allowClose;

    public BeforeCloseUpForm()
    {
        InitializeComponent();
        InitializeControls();
    }

    private void InitializeControls()
    {
        // Initialize flag
        this.allowClose = false;

        // Create popup
        this.popupContainer = new PopupControlContainer();
        this.popupContainer.Size = new Size(200, 100);

        // Subscribe to events
        this.popupContainer.Popup += PopupContainer_Popup;
        this.popupContainer.BeforeCloseUp += PopupContainer_BeforeCloseUp;

        // Add ComboDropDown or other control
        this.comboDropDown1 = new ComboDropDown();
        this.comboDropDown1.Location = new Point(10, 10);
        this.comboDropDown1.Size = new Size(150, 25);
        this.comboDropDown1.LostFocus += ComboDropDown1_LostFocus;
        this.popupContainer.Controls.Add(this.comboDropDown1);

        // Handle form click to allow close
        this.Click += Form_Click;
    }

    private void PopupContainer_Popup(object sender, EventArgs e)
    {
        // Reset flag when popup opens
        this.allowClose = true;
    }

    private void PopupContainer_BeforeCloseUp(object sender, CancelEventArgs e)
    {
        if (this.allowClose)
        {
            // Cancel the close
            e.Cancel = true;
        }
        // If allowClose is false, popup will close normally
    }

    private void ComboDropDown1_LostFocus(object sender, EventArgs e)
    {
        // Allow close when control loses focus
        this.allowClose = false;
    }

    private void Form_Click(object sender, EventArgs e)
    {
        // Allow close when clicking form
        this.allowClose = false;
    }
}
```

**VB.NET:**
```vb
Public Partial Class BeforeCloseUpForm
    Inherits Form
    
    Private popupContainer As PopupControlContainer
    Private comboDropDown1 As ComboDropDown
    Private allowClose As Boolean
    
    Public Sub New()
        InitializeComponent()
        InitializeControls()
    End Sub
    
    Private Sub InitializeControls()
        ' Initialize flag
        Me.allowClose = False
        
        ' Create popup
        Me.popupContainer = New PopupControlContainer()
        Me.popupContainer.Size = New Size(200, 100)
        
        ' Subscribe to events
        AddHandler Me.popupContainer.Popup, AddressOf PopupContainer_Popup
        AddHandler Me.popupContainer.BeforeCloseUp, AddressOf PopupContainer_BeforeCloseUp
        
        ' Add ComboDropDown
        Me.comboDropDown1 = New ComboDropDown()
        Me.comboDropDown1.Location = New Point(10, 10)
        Me.comboDropDown1.Size = New Size(150, 25)
        AddHandler Me.comboDropDown1.LostFocus, AddressOf ComboDropDown1_LostFocus
        Me.popupContainer.Controls.Add(Me.comboDropDown1)
        
        ' Handle form click
        AddHandler Me.Click, AddressOf Form_Click
    End Sub
    
    Private Sub PopupContainer_Popup(sender As Object, e As EventArgs)
        ' Reset flag when popup opens
        Me.allowClose = True
    End Sub
    
    Private Sub PopupContainer_BeforeCloseUp(sender As Object, e As CancelEventArgs)
        If Me.allowClose Then
            ' Cancel the close
            e.Cancel = True
        End If
    End Sub
    
    Private Sub ComboDropDown1_LostFocus(sender As Object, e As EventArgs)
        ' Allow close when control loses focus
        Me.allowClose = False
    End Sub
    
    Private Sub Form_Click(sender As Object, e As EventArgs)
        ' Allow close when clicking form
        Me.allowClose = False
    End Sub
End Class
```

### When to Use This Pattern

- Hosting controls with their own focus management (ComboDropDown, DateTimePicker)
- Preventing accidental closes during complex interactions
- Managing nested popups or dropdowns
- Requiring precise control over close timing

## Troubleshooting Tips

### Issue: ComboBoxBase Closes Popup Immediately

**Solution:**
1. Create custom PopupControlContainer with overridden OnPopup
2. Handle DropDown event and set parent-child relationship
3. Ensure PopupContainer and CurrentPopupChild are set correctly

### Issue: Transparent Popup Not Working

**Solution:**
- Set opacity in BeforePopup event, not after ShowPopup
- Access PopupHost.Opacity, not the container itself
- Verify opacity value is between 0.0 and 1.0

### Issue: IsShowing Returns False When Popup Is Visible

**Solution:**
- Check timing - call after ShowPopup completes
- Verify popup wasn't immediately closed due to validation
- Ensure auto-close hasn't triggered

### Issue: BeforeCloseUp Cancel Not Working

**Solution:**
- Use Boolean flag pattern shown above
- Set flag in Popup event (when opened)
- Clear flag in LostFocus and Click events
- Cancel in BeforeCloseUp based on flag state

### Issue: Popup Performance with Transparency

**Solution:**
- Use opacity ≥ 0.70 for better performance
- Avoid frequent opacity changes
- Consider disabling transparency on low-end systems
- Test with actual content and data

## Summary

- **ComboBoxBase Hosting:** Requires custom PopupControlContainer and DropDown event handling
- **Transparency:** Set PopupHost.Opacity in BeforePopup event (0.0-1.0)
- **State Checking:** Use IsShowing() to determine visibility
- **BeforeCloseUp:** Use Boolean flag pattern for complex close scenarios
- Test advanced scenarios thoroughly with actual use cases
