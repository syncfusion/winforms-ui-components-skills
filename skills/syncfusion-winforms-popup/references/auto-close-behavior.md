# Auto-Close Behavior

This guide explains how to control the automatic closing behavior of PopupControlContainer, including the default auto-close functionality, preventing auto-close with IgnoreMouseMessages, and using different PopupCloseType modes.

## Overview

By default, PopupControlContainer automatically closes when the user clicks anywhere outside the popup control. This behavior is ideal for dropdown-like scenarios but can be customized based on your application requirements.

## Default Auto-Close Behavior

When a popup is displayed:
- Clicking anywhere **outside** the PopupControlContainer closes it automatically
- Clicking **inside** the PopupControlContainer keeps it open
- This mimics standard dropdown behavior seen in combo boxes and menus

**Default scenario:**
```csharp
// Show popup
this.popupControlContainer1.ShowPopup(Point.Empty);

// Popup automatically closes when user clicks outside
// No additional code needed
```

## Preventing Auto-Close with IgnoreMouseMessages

### IgnoreMouseMessages Property

Set the `IgnoreMouseMessages` property to `true` to prevent the popup from closing automatically when the user clicks outside.

**Property:**
```csharp
public bool IgnoreMouseMessages { get; set; }
```

**Default Value:** `false` (auto-close is enabled)

### When to Use IgnoreMouseMessages

Use `IgnoreMouseMessages = true` when:
- You need to validate input before closing the popup
- The popup should only close via explicit user action (OK/Cancel buttons)
- Multiple interactions are required before closing
- You want to prevent accidental dismissal

### Example: Conditional Closing

```csharp
public partial class Form1 : Form
{
    private PopupControlContainer popupControlContainer1;
    private RichTextBox richTextBox1;
    private TextBox popupTextBox;
    private Button validateButton;

    public Form1()
    {
        InitializeComponent();
        
        // Disable auto-close
        this.popupControlContainer1.IgnoreMouseMessages = true;
        
        // User must click the validate button to close
        this.validateButton.Click += ValidateButton_Click;
    }
    
    private void RichTextBox1_Click(object sender, EventArgs e)
    {
        this.popupControlContainer1.ShowPopup(new Point(700, 600));
    }
    
    private void ValidateButton_Click(object sender, EventArgs e)
    {
        // Only close if textbox is not empty
        if (!string.IsNullOrWhiteSpace(this.popupTextBox.Text))
        {
            this.popupControlContainer1.HidePopup(PopupCloseType.Done);
        }
        else
        {
            MessageBox.Show("Please enter a value before closing.");
        }
    }
}
```

**VB.NET:**
```vb
Public Partial Class Form1
    Inherits Form
    
    Private popupControlContainer1 As PopupControlContainer
    Private richTextBox1 As RichTextBox
    Private popupTextBox As TextBox
    Private validateButton As Button
    
    Public Sub New()
        InitializeComponent()
        
        ' Disable auto-close
        Me.popupControlContainer1.IgnoreMouseMessages = True
        
        ' User must click the validate button to close
        AddHandler Me.validateButton.Click, AddressOf ValidateButton_Click
    End Sub
    
    Private Sub RichTextBox1_Click(sender As Object, e As EventArgs)
        Me.popupControlContainer1.ShowPopup(New Point(700, 600))
    End Sub
    
    Private Sub ValidateButton_Click(sender As Object, e As EventArgs)
        ' Only close if textbox is not empty
        If Not String.IsNullOrWhiteSpace(Me.popupTextBox.Text) Then
            Me.popupControlContainer1.HidePopup(PopupCloseType.Done)
        Else
            MessageBox.Show("Please enter a value before closing.")
        End If
    End Sub
End Class
```

## PopupCloseType Modes

When calling `HidePopup()`, you can specify a `PopupCloseType` to indicate how and why the popup was closed. This information is available in the `CloseUp` event.

### PopupCloseType Enumeration

```csharp
public enum PopupCloseType
{
    Done,        // Changes accepted
    Canceled,    // Changes discarded
    Deactivated  // Closed due to focus loss
}
```

### 1. Done - Accept Changes

Use `Done` when the user confirms or accepts the changes made in the popup.

```csharp
// Close with changes applied
this.popupControlContainer1.HidePopup(PopupCloseType.Done);
```

**Typical scenarios:**
- OK button clicked
- Valid input entered and submitted
- Selection confirmed

### 2. Canceled - Discard Changes

Use `Canceled` when the user cancels or rejects the changes made in the popup.

```csharp
// Close without applying changes
this.popupControlContainer1.HidePopup(PopupCloseType.Canceled);
```

**Typical scenarios:**
- Cancel button clicked
- Escape key pressed (if not using IgnoreDialogKey)
- User wants to revert changes

### 3. Deactivated - Focus Lost

Use `Deactivated` when the popup closes because the user switched to a different application or window.

```csharp
// Close due to deactivation
this.popupControlContainer1.HidePopup(PopupCloseType.Deactivated);
```

**Typical scenarios:**
- User clicks on another application
- Application loses focus
- Window is minimized

## Complete Example: OK/Cancel Pattern

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;

public partial class DataEntryForm : Form
{
    private PopupControlContainer popupControlContainer1;
    private TextBox displayTextBox;
    private TextBox popupTextBox;
    private Button okButton;
    private Button cancelButton;

    public DataEntryForm()
    {
        InitializeComponent();
        InitializeControls();
    }

    private void InitializeControls()
    {
        // Create display textbox
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
        
        // Prevent auto-close on outside clicks
        this.popupControlContainer1.IgnoreMouseMessages = true;
        
        // Handle CloseUp event
        this.popupControlContainer1.CloseUp += PopupControlContainer1_CloseUp;

        // Create popup controls
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

        // Add to popup
        this.popupControlContainer1.Controls.Add(this.popupTextBox);
        this.popupControlContainer1.Controls.Add(this.okButton);
        this.popupControlContainer1.Controls.Add(this.cancelButton);
    }

    private void DisplayTextBox_Click(object sender, EventArgs e)
    {
        // Initialize popup with current value
        this.popupTextBox.Text = this.displayTextBox.Text;
        this.popupControlContainer1.ShowPopup(Point.Empty);
        this.popupTextBox.Focus();
        this.popupTextBox.SelectAll();
    }

    private void OkButton_Click(object sender, EventArgs e)
    {
        // Validate before closing
        if (!string.IsNullOrWhiteSpace(this.popupTextBox.Text))
        {
            // Close with Done type
            this.popupControlContainer1.HidePopup(PopupCloseType.Done);
        }
        else
        {
            MessageBox.Show("Please enter a value.", "Validation Error", 
                MessageBoxButtons.OK, MessageBoxIcon.Warning);
        }
    }

    private void CancelButton_Click(object sender, EventArgs e)
    {
        // Close with Canceled type
        this.popupControlContainer1.HidePopup(PopupCloseType.Canceled);
    }

    private void PopupControlContainer1_CloseUp(object sender, PopupClosedEventArgs e)
    {
        // Handle based on close type
        if (e.PopupCloseType == PopupCloseType.Done)
        {
            // Apply changes
            this.displayTextBox.Text = this.popupTextBox.Text;
            this.displayTextBox.BackColor = Color.LightGreen;
        }
        else if (e.PopupCloseType == PopupCloseType.Canceled)
        {
            // Discard changes
            this.displayTextBox.BackColor = Color.LightYellow;
        }
        else if (e.PopupCloseType == PopupCloseType.Deactivated)
        {
            // Handle deactivation
            this.displayTextBox.BackColor = Color.LightGray;
        }

        // Always return focus to display textbox
        this.displayTextBox.Focus();
    }
}
```

## Common Patterns

### Pattern 1: Simple Validation Before Close

```csharp
this.popupControlContainer1.IgnoreMouseMessages = true;

private void ApplyButton_Click(object sender, EventArgs e)
{
    if (IsValid())
    {
        this.popupControlContainer1.HidePopup(PopupCloseType.Done);
    }
}

private bool IsValid()
{
    // Your validation logic
    return !string.IsNullOrEmpty(this.textBox1.Text);
}
```

### Pattern 2: Multi-Step Process

```csharp
private int currentStep = 0;
this.popupControlContainer1.IgnoreMouseMessages = true;

private void NextButton_Click(object sender, EventArgs e)
{
    currentStep++;
    
    if (currentStep >= totalSteps)
    {
        // Complete the process
        this.popupControlContainer1.HidePopup(PopupCloseType.Done);
    }
    else
    {
        // Show next step
        ShowStep(currentStep);
    }
}
```

### Pattern 3: Required Selection

```csharp
private bool itemSelected = false;
this.popupControlContainer1.IgnoreMouseMessages = true;

private void ListBox1_SelectedIndexChanged(object sender, EventArgs e)
{
    itemSelected = true;
}

private void ConfirmButton_Click(object sender, EventArgs e)
{
    if (itemSelected)
    {
        this.popupControlContainer1.HidePopup(PopupCloseType.Done);
    }
    else
    {
        MessageBox.Show("Please select an item.");
    }
}
```

## Best Practices

1. **Default Behavior:** Use default auto-close for simple dropdowns and pickers
2. **IgnoreMouseMessages:** Enable for complex forms requiring validation
3. **PopupCloseType:** Always specify the close type to enable proper event handling
4. **User Feedback:** Provide clear OK/Cancel buttons when auto-close is disabled
5. **Validation:** Validate input before closing with `Done` type
6. **Focus Management:** Return focus to appropriate control after closing

## Troubleshooting

**Problem:** Popup closes immediately when clicking inside
- **Solution:** Check if child controls are properly added to the popup's Controls collection

**Problem:** Popup doesn't close at all
- **Solution:** Verify you're calling `HidePopup()` in button click handlers when `IgnoreMouseMessages = true`

**Problem:** Need to distinguish between OK and Cancel in CloseUp event
- **Solution:** Use different `PopupCloseType` values and check `e.PopupCloseType` in the event handler

**Problem:** Popup closes when clicking child combo box dropdown
- **Solution:** See [advanced-scenarios.md](advanced-scenarios.md) for ComboBoxBase hosting

## Summary

- **Default:** Popup auto-closes on outside clicks
- **IgnoreMouseMessages = true:** Prevents auto-close, requires manual HidePopup() call
- **PopupCloseType:** Indicates how/why popup was closed (Done, Canceled, Deactivated)
- Use CloseUp event to handle different close types appropriately
