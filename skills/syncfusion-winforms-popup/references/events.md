# Events and Lifecycle

## Table of Contents
- [Overview](#overview)
- [Event Lifecycle](#event-lifecycle)
- [BeforePopup Event](#beforepopup-event)
- [Popup Event](#popup-event)
- [CloseUp Event](#closeup-event)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)

## Overview

PopupControlContainer provides three key events that allow you to control the popup lifecycle, customize appearance, manage focus, and transfer data between the popup and parent form.

**Key Events:**
- **BeforePopup** - Raised before the popup is shown (cancelable)
- **Popup** - Raised after the popup is visible
- **CloseUp** - Raised when the popup is closed

These events enable scenarios such as resizing popups, setting focus, enabling mnemonics, validating input, and transferring data based on how the popup was closed.

## Event Lifecycle

The typical event sequence when showing and hiding a popup:

```
1. User triggers popup (e.g., clicks parent control)
2. BeforePopup event fires
   - Customize popup appearance
   - Resize popup
   - Cancel popup display if needed
3. Popup is displayed
4. Popup event fires
   - Set focus to popup controls
   - Enable mnemonic support
5. User interacts with popup
6. Popup is closed (auto-close, HidePopup(), or keyboard)
7. CloseUp event fires
   - Transfer data to parent form
   - Restore focus
   - Handle different close types
```

## BeforePopup Event

The `BeforePopup` event occurs **before** the popup is shown. This is the ideal place to customize the popup's appearance, resize it, or cancel the display.

### Event Signature

```csharp
public event CancelEventHandler BeforePopup;
```

**Event Arguments:**
- **CancelEventArgs**
  - `Cancel` (bool) - Set to `true` to prevent the popup from showing

### Common Use Cases

1. Resizing the popup dynamically
2. Customizing the popup host window
3. Setting popup transparency
4. Adjusting popup size based on content
5. Canceling popup display based on conditions

### Example 1: Resizable Popup

Make the popup resizable by changing the border style:

```csharp
private void PopupControlContainer1_BeforePopup(object sender, CancelEventArgs e)
{
    // Make the popup host's border style resizable
    this.popupControlContainer1.PopupHost.FormBorderStyle = FormBorderStyle.SizableToolWindow;
    this.popupControlContainer1.PopupHost.BackColor = this.BackColor;
    
    // Set minimum size
    if (this.popupControlContainer1.PopupHost.Size.Width < 160)
    {
        this.popupControlContainer1.PopupHost.Size = new Size(160, 176);
    }
    
    // Fill the popup host when resized
    this.popupControlContainer1.Dock = DockStyle.Fill;
}
```

**VB.NET:**
```vb
Private Sub PopupControlContainer1_BeforePopup(sender As Object, e As CancelEventArgs)
    ' Make the popup host's border style resizable
    Me.popupControlContainer1.PopupHost.FormBorderStyle = FormBorderStyle.SizableToolWindow
    Me.popupControlContainer1.PopupHost.BackColor = Me.BackColor
    
    ' Set minimum size
    If Me.popupControlContainer1.PopupHost.Size.Width < 160 Then
        Me.popupControlContainer1.PopupHost.Size = New Size(160, 176)
    End If
    
    ' Fill the popup host when resized
    Me.popupControlContainer1.Dock = DockStyle.Fill
End Sub
```

### Example 2: Transparent Popup

Set popup opacity for transparency effect:

```csharp
private void PopupControlContainer1_BeforePopup(object sender, CancelEventArgs e)
{
    // Set opacity for transparency (0.0 = fully transparent, 1.0 = fully opaque)
    this.popupControlContainer1.PopupHost.Opacity = 0.75;
}
```

**VB.NET:**
```vb
Private Sub PopupControlContainer1_BeforePopup(sender As Object, e As CancelEventArgs)
    ' Set opacity for transparency
    Me.popupControlContainer1.PopupHost.Opacity = 0.75
End Sub
```

### Example 3: Conditional Display

Cancel popup display based on a condition:

```csharp
private void PopupControlContainer1_BeforePopup(object sender, CancelEventArgs e)
{
    // Don't show popup if form is in read-only mode
    if (this.IsReadOnly)
    {
        e.Cancel = true;
        MessageBox.Show("Cannot show popup in read-only mode.");
    }
}
```

### Example 4: Dynamic Sizing

Adjust popup size based on content:

```csharp
private void PopupControlContainer1_BeforePopup(object sender, CancelEventArgs e)
{
    // Calculate required size based on child controls
    int maxWidth = 0;
    int totalHeight = 0;
    
    foreach (Control ctrl in this.popupControlContainer1.Controls)
    {
        if (ctrl.Right > maxWidth)
            maxWidth = ctrl.Right;
        if (ctrl.Bottom > totalHeight)
            totalHeight = ctrl.Bottom;
    }
    
    // Add padding
    this.popupControlContainer1.PopupHost.ClientSize = 
        new Size(maxWidth + 20, totalHeight + 20);
}
```

## Popup Event

The `Popup` event occurs **after** the popup has been displayed and is visible. Use this event to set focus or enable mnemonics.

### Event Signature

```csharp
public event EventHandler Popup;
```

**Event Arguments:**
- **EventArgs** (standard event args)

### Common Use Cases

1. Setting focus to a specific control in the popup
2. Enabling mnemonic (access key) support
3. Initializing popup state after display
4. Starting animations or timers

### Example 1: Mnemonic Support

Controls within PopupControlContainer don't respond to mnemonics (access keys like Alt+K) by default because the main form regains focus immediately. Fix this by setting focus back to the popup:

```csharp
private void PopupControlContainer1_Popup(object sender, EventArgs e)
{
    // Set focus to popup for mnemonic support
    this.popupControlContainer1.Focus();
}
```

**VB.NET:**
```vb
Private Sub PopupControlContainer1_Popup(sender As Object, e As EventArgs)
    ' Set focus to popup for mnemonic support
    Me.popupControlContainer1.Focus()
End Sub
```

Now access keys like Alt+O, Alt+C will work on buttons within the popup:
```csharp
this.okButton.Text = "&OK";     // Alt+O
this.cancelButton.Text = "&Cancel"; // Alt+C
```

### Example 2: Set Focus to Specific Control

```csharp
private void PopupControlContainer1_Popup(object sender, EventArgs e)
{
    // Focus the first textbox in the popup
    this.popupTextBox.Focus();
    this.popupTextBox.SelectAll();
}
```

### Example 3: Initialize Popup State

```csharp
private void PopupControlContainer1_Popup(object sender, EventArgs e)
{
    // Set current date in date picker
    this.dateTimePicker1.Value = DateTime.Now;
    
    // Clear previous selections
    this.checkedListBox1.ClearSelected();
    
    // Focus first control
    this.popupControlContainer1.Controls[0].Focus();
}
```

## CloseUp Event

The `CloseUp` event occurs when the popup is closed. This is the ideal place to transfer data from the popup to the parent form and handle different close types.

### Event Signature

```csharp
public event PopupClosedEventHandler CloseUp;
```

**Event Arguments:**
- **PopupClosedEventArgs**
  - `PopupCloseType` - Indicates how the popup was closed (Done, Canceled, Deactivated)

### Common Use Cases

1. Transferring data from popup to parent form
2. Validating and applying changes
3. Restoring focus to parent controls
4. Handling different close scenarios
5. Updating UI based on popup results

### Example 1: Basic Data Transfer

```csharp
private void PopupControlContainer1_CloseUp(object sender, PopupClosedEventArgs e)
{
    if (e.PopupCloseType == PopupCloseType.Done)
    {
        // Apply changes
        this.richTextBox1.Text = this.popupTextBox.Text;
    }
    
    // Restore focus
    if (e.PopupCloseType == PopupCloseType.Done || 
        e.PopupCloseType == PopupCloseType.Canceled)
    {
        this.richTextBox1.Focus();
    }
}
```

**VB.NET:**
```vb
Private Sub PopupControlContainer1_CloseUp(sender As Object, e As PopupClosedEventArgs)
    If e.PopupCloseType = PopupCloseType.Done Then
        ' Apply changes
        Me.richTextBox1.Text = Me.popupTextBox.Text
    End If
    
    ' Restore focus
    If e.PopupCloseType = PopupCloseType.Done OrElse 
       e.PopupCloseType = PopupCloseType.Canceled Then
        Me.richTextBox1.Focus()
    End If
End Sub
```

### Example 2: Handle All Close Types

```csharp
private void PopupControlContainer1_CloseUp(object sender, PopupClosedEventArgs e)
{
    switch (e.PopupCloseType)
    {
        case PopupCloseType.Done:
            // User clicked OK or confirmed changes
            ApplyChanges();
            this.statusLabel.Text = "Changes applied";
            this.statusLabel.ForeColor = Color.Green;
            break;
            
        case PopupCloseType.Canceled:
            // User clicked Cancel or Escape
            DiscardChanges();
            this.statusLabel.Text = "Changes canceled";
            this.statusLabel.ForeColor = Color.Orange;
            break;
            
        case PopupCloseType.Deactivated:
            // Popup closed due to focus loss
            SaveDraft();
            this.statusLabel.Text = "Draft saved";
            this.statusLabel.ForeColor = Color.Blue;
            break;
    }
    
    // Always restore focus
    this.parentControl.Focus();
}

private void ApplyChanges()
{
    this.textBox1.Text = this.popupTextBox.Text;
    this.colorBox.BackColor = this.popupColorPicker.SelectedColor;
}

private void DiscardChanges()
{
    // Reset popup to original values
    this.popupTextBox.Text = this.textBox1.Text;
}

private void SaveDraft()
{
    // Save to temporary storage
    Settings.Default.DraftText = this.popupTextBox.Text;
    Settings.Default.Save();
}
```

### Example 3: Validation on Close

```csharp
private string originalValue;

private void ParentControl_Click(object sender, EventArgs e)
{
    // Store original value before showing popup
    originalValue = this.displayTextBox.Text;
    this.popupControlContainer1.ShowPopup(Point.Empty);
}

private void PopupControlContainer1_CloseUp(object sender, PopupClosedEventArgs e)
{
    if (e.PopupCloseType == PopupCloseType.Done)
    {
        // Validate before applying
        if (ValidateInput(this.popupTextBox.Text))
        {
            this.displayTextBox.Text = this.popupTextBox.Text;
            this.displayTextBox.BackColor = Color.White;
        }
        else
        {
            // Revert to original
            this.displayTextBox.Text = originalValue;
            this.displayTextBox.BackColor = Color.LightPink;
            MessageBox.Show("Invalid input. Changes not applied.");
        }
    }
    else
    {
        // Canceled - revert to original
        this.popupTextBox.Text = originalValue;
    }
}

private bool ValidateInput(string value)
{
    return !string.IsNullOrWhiteSpace(value) && value.Length <= 50;
}
```

## Complete Examples

### Example 1: Full Event Lifecycle

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;

public partial class FullEventExample : Form
{
    private PopupControlContainer popupControlContainer1;
    private TextBox displayTextBox;
    private TextBox popupTextBox;
    private Button okButton;
    private Button cancelButton;
    private string originalValue;

    public FullEventExample()
    {
        InitializeComponent();
        InitializeControls();
        SubscribeToEvents();
    }

    private void InitializeControls()
    {
        // Display textbox
        this.displayTextBox = new TextBox();
        this.displayTextBox.Size = new Size(200, 20);
        this.displayTextBox.Location = new Point(20, 20);
        this.displayTextBox.Click += DisplayTextBox_Click;
        this.Controls.Add(this.displayTextBox);

        // Popup container
        this.popupControlContainer1 = new PopupControlContainer();
        this.popupControlContainer1.Size = new Size(250, 120);
        this.popupControlContainer1.ParentControl = this.displayTextBox;
        this.popupControlContainer1.IgnoreMouseMessages = true;

        // Popup controls
        this.popupTextBox = new TextBox();
        this.popupTextBox.Size = new Size(200, 20);
        this.popupTextBox.Location = new Point(20, 20);
        
        this.okButton = new Button();
        this.okButton.Text = "&OK";
        this.okButton.Size = new Size(75, 25);
        this.okButton.Location = new Point(40, 60);
        this.okButton.Click += (s, e) => 
            this.popupControlContainer1.HidePopup(PopupCloseType.Done);
        
        this.cancelButton = new Button();
        this.cancelButton.Text = "&Cancel";
        this.cancelButton.Size = new Size(75, 25);
        this.cancelButton.Location = new Point(125, 60);
        this.cancelButton.Click += (s, e) => 
            this.popupControlContainer1.HidePopup(PopupCloseType.Canceled);

        this.popupControlContainer1.Controls.Add(this.popupTextBox);
        this.popupControlContainer1.Controls.Add(this.okButton);
        this.popupControlContainer1.Controls.Add(this.cancelButton);
    }

    private void SubscribeToEvents()
    {
        this.popupControlContainer1.BeforePopup += PopupControlContainer1_BeforePopup;
        this.popupControlContainer1.Popup += PopupControlContainer1_Popup;
        this.popupControlContainer1.CloseUp += PopupControlContainer1_CloseUp;
    }

    private void DisplayTextBox_Click(object sender, EventArgs e)
    {
        // Store original value
        originalValue = this.displayTextBox.Text;
        
        // Initialize popup with current value
        this.popupTextBox.Text = this.displayTextBox.Text;
        
        // Show popup
        this.popupControlContainer1.ShowPopup(Point.Empty);
    }

    private void PopupControlContainer1_BeforePopup(object sender, System.ComponentModel.CancelEventArgs e)
    {
        // Customize popup appearance
        this.popupControlContainer1.PopupHost.FormBorderStyle = FormBorderStyle.FixedToolWindow;
        this.popupControlContainer1.PopupHost.Text = "Edit Value";
        
        // Ensure minimum size
        if (this.popupControlContainer1.PopupHost.Width < 250)
        {
            this.popupControlContainer1.PopupHost.Width = 250;
        }
    }

    private void PopupControlContainer1_Popup(object sender, EventArgs e)
    {
        // Enable mnemonics
        this.popupControlContainer1.Focus();
        
        // Focus and select textbox content
        this.popupTextBox.Focus();
        this.popupTextBox.SelectAll();
    }

    private void PopupControlContainer1_CloseUp(object sender, PopupClosedEventArgs e)
    {
        if (e.PopupCloseType == PopupCloseType.Done)
        {
            // Apply changes
            this.displayTextBox.Text = this.popupTextBox.Text;
            this.displayTextBox.BackColor = Color.LightGreen;
        }
        else if (e.PopupCloseType == PopupCloseType.Canceled)
        {
            // Revert changes
            this.popupTextBox.Text = originalValue;
            this.displayTextBox.BackColor = Color.LightYellow;
        }
        
        // Restore focus
        this.displayTextBox.Focus();
    }
}
```

## Best Practices

1. **BeforePopup:**
   - Use for appearance customization and sizing
   - Cancel if conditions aren't met for display
   - Access `PopupHost` property for form-level customization

2. **Popup:**
   - Always set focus for mnemonic support
   - Initialize popup state here, not in BeforePopup
   - Keep processing minimal to avoid display delays

3. **CloseUp:**
   - Always check `PopupCloseType` before applying changes
   - Restore focus to appropriate parent control
   - Handle all three close types appropriately

4. **General:**
   - Subscribe to events early in initialization
   - Unsubscribe from events when disposing
   - Store original values before showing popup for cancel support

## Summary

- **BeforePopup:** Customize appearance, resize, or cancel display
- **Popup:** Set focus, enable mnemonics, initialize state
- **CloseUp:** Transfer data, handle close types, restore focus
- Use `e.PopupCloseType` to distinguish between Done, Canceled, and Deactivated
- Access `PopupHost` property in BeforePopup for form-level customization
