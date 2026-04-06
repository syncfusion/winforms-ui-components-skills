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

public class CustomPopupControlContainer : PopupControlContainer
{
    public CustomPopupControlContainer() { }
    
    public CustomPopupControlContainer(IContainer container) : this()
    {
        container.Add(this);
    }
    
    protected override void OnPopup(EventArgs args)
    {
        base.OnPopup(args);
        this.Focus(); // Prevent premature closing
    }
}
```

### Step 2: Set Up Parent-Child Relationship

Handle the ComboBoxBase's `DropDown` event to establish the relationship:

```csharp
public partial class ComboBoxHostingForm : Form
{
    private CustomPopupControlContainer popupControlContainer1;
    private ComboBoxBase comboBoxBase1;
    private RichTextBox richTextBox1;

    private void InitializeControls()
    {
        // Create parent control
        this.richTextBox1 = new RichTextBox();
        this.richTextBox1.Click += RichTextBox1_Click;
        this.Controls.Add(this.richTextBox1);

        // Create custom popup container
        this.popupControlContainer1 = new CustomPopupControlContainer();
        this.popupControlContainer1.ParentControl = this.richTextBox1;
        this.popupControlContainer1.Size = new Size(200, 100);

        // Create ComboBoxBase with items
        this.comboBoxBase1 = new ComboBoxBase();
        this.comboBoxBase1.Items.AddRange(new object[] { "Option 1", "Option 2", "Option 3" });
        this.comboBoxBase1.DropDown += ComboBoxBase1_DropDown; // CRITICAL event
        
        this.popupControlContainer1.Controls.Add(this.comboBoxBase1);
    }

    private void RichTextBox1_Click(object sender, EventArgs e)
    {
        this.popupControlContainer1.ShowPopup(Point.Empty);
    }

    private void ComboBoxBase1_DropDown(object sender, EventArgs e)
    {
        // Setup parent-child relationship to prevent premature close
        this.comboBoxBase1.PopupContainer.PopupParent = this.popupControlContainer1;
        this.popupControlContainer1.CurrentPopupChild = this.comboBoxBase1.PopupContainer;
    }
}
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
public partial class TransparentPopupForm : Form
{
    private PopupControlContainer popupControlContainer1;
    private TrackBar opacityTrackBar;

    private void InitializeControls()
    {
        // Create opacity control
        this.opacityTrackBar = new TrackBar();
        this.opacityTrackBar.Minimum = 10;  // 10% opacity
        this.opacityTrackBar.Maximum = 100; // 100% opacity
        this.opacityTrackBar.Value = 75;    // Default 75%
        this.Controls.Add(this.opacityTrackBar);

        // Create popup
        this.popupControlContainer1 = new PopupControlContainer();
        this.popupControlContainer1.Size = new Size(300, 200);
        this.popupControlContainer1.BeforePopup += PopupControlContainer1_BeforePopup;

        // Add content
        Panel contentPanel = new Panel();
        contentPanel.Dock = DockStyle.Fill;
        contentPanel.BackColor = Color.LightBlue;
        this.popupControlContainer1.Controls.Add(contentPanel);
    }

    private void PopupControlContainer1_BeforePopup(object sender, CancelEventArgs e)
    {
        // Set opacity based on trackbar value
        double opacity = this.opacityTrackBar.Value / 100.0;
        this.popupControlContainer1.PopupHost.Opacity = opacity;
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

### Usage Examples

```csharp
// Check if popup is visible
if (this.popupControlContainer1.IsShowing())
{
    Console.WriteLine("Popup is currently displayed");
}

// Toggle popup
private void ToggleButton_Click(object sender, EventArgs e)
{
    if (this.popupControlContainer1.IsShowing())
        this.popupControlContainer1.HidePopup();
    else
        this.popupControlContainer1.ShowPopup(Point.Empty);
}

// Prevent multiple opens
private void ShowPopupButton_Click(object sender, EventArgs e)
{
    if (!this.popupControlContainer1.IsShowing())
        this.popupControlContainer1.ShowPopup(Point.Empty);
}

// Conditional actions
private void PerformAction()
{
    if (this.popupControlContainer1.IsShowing())
    {
        SavePopupData();
        this.popupControlContainer1.HidePopup();
    }
    ExecuteMainAction();
}
```

## BeforeCloseUp Workaround

### The Issue

When the `BeforeCloseUp` event is used to cancel popup closing, certain scenarios might require a workaround to properly manage the close behavior, especially with focus-sensitive controls.

### The Workaround

Use a Boolean flag to control the cancel behavior:

```csharp
public partial class BeforeCloseUpForm : Form
{
    private PopupControlContainer popupContainer;
    private ComboDropDown comboDropDown1;
    private bool allowClose;

    private void InitializeControls()
    {
        this.allowClose = false;

        // Create popup
        this.popupContainer = new PopupControlContainer();
        this.popupContainer.Size = new Size(200, 100);
        this.popupContainer.Popup += PopupContainer_Popup;
        this.popupContainer.BeforeCloseUp += PopupContainer_BeforeCloseUp;

        // Add ComboDropDown
        this.comboDropDown1 = new ComboDropDown();
        this.comboDropDown1.LostFocus += ComboDropDown1_LostFocus;
        this.popupContainer.Controls.Add(this.comboDropDown1);

        this.Click += Form_Click;
    }

    private void PopupContainer_Popup(object sender, EventArgs e)
    {
        this.allowClose = true; // Reset flag when popup opens
    }

    private void PopupContainer_BeforeCloseUp(object sender, CancelEventArgs e)
    {
        if (this.allowClose)
            e.Cancel = true; // Cancel the close
    }

    private void ComboDropDown1_LostFocus(object sender, EventArgs e)
    {
        this.allowClose = false; // Allow close when control loses focus
    }

    private void Form_Click(object sender, EventArgs e)
    {
        this.allowClose = false; // Allow close when clicking form
    }
}
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
