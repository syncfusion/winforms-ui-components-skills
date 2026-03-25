# Keyboard Navigation

This guide explains how to configure keyboard navigation behavior in PopupControlContainer, including default dialog key handling and custom keyboard control.

## Overview

PopupControlContainer automatically responds to common dialog keys (Alt, Enter, Tab, Esc, F4, F2) to provide intuitive keyboard navigation. By default, these keys close the popup, but this behavior can be customized or disabled based on your application requirements.

## Default Keyboard Behavior

When a popup is visible, PopupControlContainer automatically monitors the following dialog keys:

| Key | Default Action |
|-----|----------------|
| **Enter** | Closes the popup |
| **Escape** | Closes the popup |
| **Tab** | Closes the popup |
| **Alt** | Closes the popup |
| **F2** | Closes the popup |
| **F4** | Closes the popup |

This default behavior provides quick keyboard access for closing popups, similar to standard Windows dialogs and dropdowns.

## When Default Behavior Is Useful

The default keyboard handling works well for:
- Simple picker controls where Enter confirms selection
- Dropdown panels that should close when user presses Escape
- Scenarios where quick keyboard dismissal improves user experience
- Standard form dialogs with OK/Cancel buttons

## IgnoreDialogKey Property

The `IgnoreDialogKey` property controls whether PopupControlContainer responds to dialog keys automatically.

**Property:**
```csharp
public bool IgnoreDialogKey { get; set; }
```

**Default Value:** `false` (dialog keys close the popup)

### Disabling Automatic Keyboard Closing

Set `IgnoreDialogKey` to `true` to prevent dialog keys from automatically closing the popup:

```csharp
this.popupControlContainer1.IgnoreDialogKey = true;
```

**VB.NET:**
```vb
Me.popupControlContainer1.IgnoreDialogKey = True
```

When enabled, you must manually close the popup using the `HidePopup()` method.

## When to Disable Dialog Key Handling

Disable automatic keyboard closing (`IgnoreDialogKey = true`) when:
- The popup contains forms where Tab should navigate between fields
- Enter key should be used for submitting data within the popup
- Child controls need to process dialog keys themselves
- You want complete control over keyboard behavior
- Building complex multi-step wizards or forms within the popup

## Manual Popup Closing

When `IgnoreDialogKey = true`, you must implement manual closing logic:

```csharp
this.popupControlContainer1.IgnoreDialogKey = true;

// Handle OK button click
private void OkButton_Click(object sender, EventArgs e)
{
    this.popupControlContainer1.HidePopup(PopupCloseType.Done);
}

// Handle Cancel button click
private void CancelButton_Click(object sender, EventArgs e)
{
    this.popupControlContainer1.HidePopup(PopupCloseType.Canceled);
}
```

## Complete Examples

### Example 1: Default Keyboard Behavior

Simple popup that closes on any dialog key:

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;

public partial class DefaultKeyboardForm : Form
{
    private PopupControlContainer popupControlContainer1;
    private ListBox listBox1;
    private Button showButton;

    public DefaultKeyboardForm()
    {
        InitializeComponent();
        InitializeControls();
    }

    private void InitializeControls()
    {
        // Create popup
        this.popupControlContainer1 = new PopupControlContainer();
        this.popupControlContainer1.Size = new Size(200, 150);
        
        // IgnoreDialogKey = false (default)
        // User can press Enter, Esc, Tab, etc. to close
        
        // Add listbox
        this.listBox1 = new ListBox();
        this.listBox1.Dock = DockStyle.Fill;
        this.listBox1.Items.AddRange(new object[] 
        { 
            "Option 1", "Option 2", "Option 3" 
        });
        this.popupControlContainer1.Controls.Add(this.listBox1);
        
        // Create trigger button
        this.showButton = new Button();
        this.showButton.Text = "Select Option";
        this.showButton.Location = new Point(50, 50);
        this.showButton.Click += ShowButton_Click;
        
        this.popupControlContainer1.ParentControl = this.showButton;
        this.Controls.Add(this.showButton);
        
        // Handle CloseUp to get selected value
        this.popupControlContainer1.CloseUp += PopupControlContainer1_CloseUp;
    }

    private void ShowButton_Click(object sender, EventArgs e)
    {
        this.popupControlContainer1.ShowPopup(Point.Empty);
    }

    private void PopupControlContainer1_CloseUp(object sender, PopupClosedEventArgs e)
    {
        if (this.listBox1.SelectedItem != null)
        {
            this.showButton.Text = this.listBox1.SelectedItem.ToString();
        }
    }
}
```

### Example 2: Custom Keyboard Handling

Popup with tab navigation and manual closing:

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;

public partial class CustomKeyboardForm : Form
{
    private PopupControlContainer popupControlContainer1;
    private TextBox nameTextBox;
    private TextBox emailTextBox;
    private Button okButton;
    private Button cancelButton;

    public CustomKeyboardForm()
    {
        InitializeComponent();
        InitializeControls();
    }

    private void InitializeControls()
    {
        // Create popup
        this.popupControlContainer1 = new PopupControlContainer();
        this.popupControlContainer1.Size = new Size(300, 150);
        
        // Disable automatic dialog key handling
        this.popupControlContainer1.IgnoreDialogKey = true;
        
        // Prevent auto-close on outside clicks
        this.popupControlContainer1.IgnoreMouseMessages = true;
        
        // Create form controls
        Label nameLabel = new Label();
        nameLabel.Text = "Name:";
        nameLabel.Location = new Point(10, 15);
        nameLabel.AutoSize = true;
        
        this.nameTextBox = new TextBox();
        this.nameTextBox.Location = new Point(80, 12);
        this.nameTextBox.Size = new Size(180, 20);
        this.nameTextBox.TabIndex = 0;
        
        Label emailLabel = new Label();
        emailLabel.Text = "Email:";
        emailLabel.Location = new Point(10, 45);
        emailLabel.AutoSize = true;
        
        this.emailTextBox = new TextBox();
        this.emailTextBox.Location = new Point(80, 42);
        this.emailTextBox.Size = new Size(180, 20);
        this.emailTextBox.TabIndex = 1;
        
        this.okButton = new Button();
        this.okButton.Text = "OK";
        this.okButton.Size = new Size(75, 25);
        this.okButton.Location = new Point(80, 90);
        this.okButton.TabIndex = 2;
        this.okButton.Click += OkButton_Click;
        
        this.cancelButton = new Button();
        this.cancelButton.Text = "Cancel";
        this.cancelButton.Size = new Size(75, 25);
        this.cancelButton.Location = new Point(165, 90);
        this.cancelButton.TabIndex = 3;
        this.cancelButton.Click += CancelButton_Click;
        
        // Add controls to popup
        this.popupControlContainer1.Controls.Add(nameLabel);
        this.popupControlContainer1.Controls.Add(this.nameTextBox);
        this.popupControlContainer1.Controls.Add(emailLabel);
        this.popupControlContainer1.Controls.Add(this.emailTextBox);
        this.popupControlContainer1.Controls.Add(this.okButton);
        this.popupControlContainer1.Controls.Add(this.cancelButton);
        
        // Create trigger button
        Button showButton = new Button();
        showButton.Text = "Enter Details";
        showButton.Location = new Point(50, 50);
        showButton.Click += (s, e) => this.popupControlContainer1.ShowPopup(Point.Empty);
        
        this.popupControlContainer1.ParentControl = showButton;
        this.Controls.Add(showButton);
        
        // Handle Popup event to set focus
        this.popupControlContainer1.Popup += PopupControlContainer1_Popup;
    }

    private void PopupControlContainer1_Popup(object sender, EventArgs e)
    {
        // Focus first textbox
        this.nameTextBox.Focus();
    }

    private void OkButton_Click(object sender, EventArgs e)
    {
        // Validate before closing
        if (string.IsNullOrWhiteSpace(this.nameTextBox.Text))
        {
            MessageBox.Show("Please enter a name.", "Validation Error");
            this.nameTextBox.Focus();
            return;
        }
        
        // Close with Done
        this.popupControlContainer1.HidePopup(PopupCloseType.Done);
    }

    private void CancelButton_Click(object sender, EventArgs e)
    {
        // Close with Canceled
        this.popupControlContainer1.HidePopup(PopupCloseType.Canceled);
    }
}
```

### Example 3: Custom Escape Key Handling

Disable dialog keys but handle Escape manually:

```csharp
public partial class CustomEscapeForm : Form
{
    private PopupControlContainer popupControlContainer1;

    private void InitializeControls()
    {
        this.popupControlContainer1 = new PopupControlContainer();
        
        // Disable automatic dialog key handling
        this.popupControlContainer1.IgnoreDialogKey = true;
        
        // Handle KeyDown event for custom key processing
        this.popupControlContainer1.KeyDown += PopupControlContainer1_KeyDown;
        
        // Ensure popup can receive keyboard input
        this.popupControlContainer1.Popup += (s, e) => 
            this.popupControlContainer1.Focus();
    }

    private void PopupControlContainer1_KeyDown(object sender, KeyEventArgs e)
    {
        if (e.KeyCode == Keys.Escape)
        {
            // Custom Escape handling
            if (MessageBox.Show("Close without saving?", "Confirm", 
                MessageBoxButtons.YesNo) == DialogResult.Yes)
            {
                this.popupControlContainer1.HidePopup(PopupCloseType.Canceled);
            }
            
            e.Handled = true;
        }
        else if (e.Control && e.KeyCode == Keys.Enter)
        {
            // Custom Ctrl+Enter to save and close
            this.popupControlContainer1.HidePopup(PopupCloseType.Done);
            e.Handled = true;
        }
    }
}
```

## Common Patterns

### Pattern 1: Form with Tab Navigation

```csharp
// Disable dialog keys for form navigation
this.popupControlContainer1.IgnoreDialogKey = true;

// Set tab order on child controls
this.textBox1.TabIndex = 0;
this.textBox2.TabIndex = 1;
this.textBox3.TabIndex = 2;
this.okButton.TabIndex = 3;

// Users can Tab between fields without closing popup
```

### Pattern 2: Enter Key Submits Form

```csharp
this.popupControlContainer1.IgnoreDialogKey = true;

// Handle Enter key on textbox
this.textBox1.KeyDown += (s, e) =>
{
    if (e.KeyCode == Keys.Enter)
    {
        SubmitForm();
        e.Handled = true;
        e.SuppressKeyPress = true;
    }
};

private void SubmitForm()
{
    if (ValidateForm())
    {
        this.popupControlContainer1.HidePopup(PopupCloseType.Done);
    }
}
```

### Pattern 3: Escape for Quick Dismiss (Default)

```csharp
// Keep IgnoreDialogKey = false (default)
// User can press Escape to quickly close

// Optional: Handle CloseUp to detect Escape key close
this.popupControlContainer1.CloseUp += (s, e) =>
{
    // When closed via Escape, it's typically Canceled type
    if (e.PopupCloseType == PopupCloseType.Canceled)
    {
        // Revert changes
    }
};
```

### Pattern 4: Mixed Approach

```csharp
// Disable dialog keys
this.popupControlContainer1.IgnoreDialogKey = true;

// But handle specific keys manually
this.popupControlContainer1.PreviewKeyDown += (s, e) =>
{
    e.IsInputKey = true; // Ensure all keys are processed
};

this.popupControlContainer1.KeyDown += (s, e) =>
{
    switch (e.KeyCode)
    {
        case Keys.Escape:
            // Allow Escape to close
            this.popupControlContainer1.HidePopup(PopupCloseType.Canceled);
            break;
            
        case Keys.F1:
            // Show help
            ShowHelp();
            break;
    }
};
```

## Best Practices

1. **Default Behavior:** Use `IgnoreDialogKey = false` for simple pickers, dropdowns, and selection lists
2. **Custom Forms:** Enable `IgnoreDialogKey = true` for complex forms with multiple input fields
3. **Tab Order:** When disabling dialog keys, set appropriate `TabIndex` values on child controls
4. **Focus Management:** Always set focus in the Popup event when using custom keyboard handling
5. **User Feedback:** Provide visual indicators (buttons, instructions) when auto-close is disabled
6. **Escape Key:** Consider allowing Escape to always cancel, even with custom keyboard handling
7. **Keyboard Shortcuts:** Use standard shortcuts (Alt+O for OK, Alt+C for Cancel) for better usability

## Troubleshooting

**Problem:** Tab doesn't navigate between controls
- **Solution:** Set `IgnoreDialogKey = true` and configure TabIndex on child controls

**Problem:** Enter key doesn't submit form
- **Solution:** Enable `IgnoreDialogKey = true` and handle KeyDown event for Enter key

**Problem:** Escape key doesn't close popup
- **Solution:** If `IgnoreDialogKey = true`, manually handle Escape in KeyDown event

**Problem:** Popup closes immediately when typing
- **Solution:** Check if `IgnoreDialogKey` is set correctly for your scenario

**Problem:** Dialog keys not working at all
- **Solution:** Ensure popup has focus; handle Popup event and call `this.popupControlContainer1.Focus()`

## Summary

- **Default:** Dialog keys (Enter, Esc, Tab, Alt, F2, F4) automatically close popup
- **IgnoreDialogKey = false:** Use for simple selection/picker controls
- **IgnoreDialogKey = true:** Use for forms with multiple inputs requiring tab navigation
- When disabling dialog keys, implement manual closing with OK/Cancel buttons
- Set focus in Popup event for proper keyboard handling
- Consider user experience when customizing keyboard behavior
