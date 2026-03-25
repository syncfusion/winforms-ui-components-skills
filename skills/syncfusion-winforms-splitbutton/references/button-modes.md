# Button Modes and States

The SplitButton control supports two button modes: **Normal** and **Toggle**. This guide explains how to configure and use each mode.

## Overview

The `ButtonMode` property determines how the button portion of the SplitButton behaves:

- **Normal Mode:** Button executes a standard click command (like a regular button)
- **Toggle Mode:** Button maintains checked/unchecked state (like a toggle switch or checkbox)

## Normal Mode

### What is Normal Mode?

Normal mode is the default behavior where the button executes a command when clicked, similar to a standard Windows Forms Button. The button does not maintain any state between clicks.

**Use Normal Mode When:**
- Button performs an action without maintaining state (Save, Print, Send, etc.)
- Each click is independent of previous clicks
- Button should not remain in "pressed" or "active" state

### Setting Normal Mode

**C# Example:**
```csharp
// Set button to Normal mode
splitButton1.ButtonMode = Syncfusion.Windows.Forms.Tools.ButtonMode.Normal;
```

**VB.NET Example:**
```vb
' Set button to Normal mode
Me.splitButton1.ButtonMode = Syncfusion.Windows.Forms.Tools.ButtonMode.Normal
```

### Normal Mode Example

Complete example of a SplitButton in Normal mode:

```csharp
public Form1()
{
    InitializeComponent();
    
    SplitButton saveButton = new SplitButton();
    saveButton.Text = "Save";
    saveButton.Size = new Size(100, 35);
    saveButton.Location = new Point(20, 20);
    saveButton.ButtonMode = ButtonMode.Normal;
    
    // Add dropdown items
    saveButton.DropDownItems.Add(new ToolStripMenuItem("Save"));
    saveButton.DropDownItems.Add(new ToolStripMenuItem("Save As..."));
    saveButton.DropDownItems.Add(new ToolStripMenuItem("Save All"));
    
    // Handle button click
    saveButton.Click += (s, e) => {
        MessageBox.Show("Save button clicked");
    };
    
    this.Controls.Add(saveButton);
}
```

## Toggle Mode

### What is Toggle Mode?

Toggle mode allows the button to maintain a checked (pressed) or unchecked (released) state. When clicked, the button alternates between these two states, similar to a checkbox or toggle switch.

**Use Toggle Mode When:**
- Button represents an on/off state (Show/Hide, Enable/Disable, etc.)
- Visual feedback of button state is important
- Button state persists until user toggles it again
- Combined with dropdown options that relate to the toggle state

### Setting Toggle Mode

**C# Example:**
```csharp
// Set button to Toggle mode
splitButton1.ButtonMode = Syncfusion.Windows.Forms.Tools.ButtonMode.Toggle;
```

**VB.NET Example:**
```vb
' Set button to Toggle mode
Me.splitButton1.ButtonMode = Syncfusion.Windows.Forms.Tools.ButtonMode.Toggle
```

### IsButtonChecked Property

The `IsButtonChecked` property controls and queries the toggle state. This property is only active when the button is in Toggle mode.

**Setting Checked State (C#):**
```csharp
// Set button to checked state
splitButton1.IsButtonChecked = true;

// Set button to unchecked state
splitButton1.IsButtonChecked = false;
```

**Setting Checked State (VB.NET):**
```vb
' Set button to checked state
splitButton1.IsButtonChecked = True

' Set button to unchecked state
splitButton1.IsButtonChecked = False
```

**Querying Button State:**
```csharp
// Check if button is in checked state
if (splitButton1.IsButtonChecked)
{
    // Button is checked - perform checked action
    Console.WriteLine("Button is checked");
}
else
{
    // Button is unchecked - perform unchecked action
    Console.WriteLine("Button is unchecked");
}
```

### Toggle Mode Example

Complete example of a SplitButton in Toggle mode:

```csharp
public Form1()
{
    InitializeComponent();
    
    SplitButton viewButton = new SplitButton();
    viewButton.Text = "Show Details";
    viewButton.Size = new Size(120, 35);
    viewButton.Location = new Point(20, 20);
    viewButton.ButtonMode = ButtonMode.Toggle;
    viewButton.IsButtonChecked = false; // Initially unchecked
    
    // Add dropdown items
    viewButton.DropDownItems.Add(new ToolStripMenuItem("Details View"));
    viewButton.DropDownItems.Add(new ToolStripMenuItem("Summary View"));
    viewButton.DropDownItems.Add(new ToolStripMenuItem("Compact View"));
    
    // Handle button click
    viewButton.Click += (s, e) => {
        if (viewButton.IsButtonChecked)
        {
            MessageBox.Show("Details shown");
            viewButton.Text = "Hide Details";
        }
        else
        {
            MessageBox.Show("Details hidden");
            viewButton.Text = "Show Details";
        }
    };
    
    this.Controls.Add(viewButton);
}
```

## Practical Examples

### Example 1: Save Button (Normal Mode)

A typical "Save" button with save variations:

```csharp
SplitButton saveBtn = new SplitButton
{
    Text = "Save",
    ButtonMode = ButtonMode.Normal,
    Size = new Size(80, 30),
    Location = new Point(10, 10)
};

saveBtn.DropDownItems.AddRange(new ToolStripItem[]
{
    new ToolStripMenuItem("Save"),
    new ToolStripMenuItem("Save As..."),
    new ToolStripMenuItem("Save Copy...")
});

saveBtn.Click += (s, e) => {
    // Perform default save action
    SaveDocument();
};

saveBtn.DropDownItemClicked += (s, e) => {
    switch (e.ClickedItem.Text)
    {
        case "Save":
            SaveDocument();
            break;
        case "Save As...":
            SaveDocumentAs();
            break;
        case "Save Copy...":
            SaveCopy();
            break;
    }
};
```

### Example 2: Filter Toggle Button (Toggle Mode)

A toggle button for showing/hiding filters with filter options:

```csharp
SplitButton filterBtn = new SplitButton
{
    Text = "Filters",
    ButtonMode = ButtonMode.Toggle,
    IsButtonChecked = false,
    Size = new Size(90, 30),
    Location = new Point(10, 50)
};

filterBtn.DropDownItems.AddRange(new ToolStripItem[]
{
    new ToolStripMenuItem("All Items"),
    new ToolStripMenuItem("Active Only"),
    new ToolStripMenuItem("Archived")
});

filterBtn.Click += (s, e) => {
    if (filterBtn.IsButtonChecked)
    {
        // Show filter panel
        filterPanel.Visible = true;
    }
    else
    {
        // Hide filter panel
        filterPanel.Visible = false;
    }
};
```

### Example 3: Auto-Toggle on Item Selection

Automatically change toggle state when dropdown item is selected:

```csharp
SplitButton viewBtn = new SplitButton
{
    Text = "View Options",
    ButtonMode = ButtonMode.Toggle,
    IsButtonChecked = false
};

viewBtn.DropDownItems.Add(new ToolStripMenuItem("Enable View"));
viewBtn.DropDownItems.Add(new ToolStripMenuItem("Disable View"));

viewBtn.DropDownItemClicked += (s, e) => {
    if (e.ClickedItem.Text == "Enable View")
    {
        viewBtn.IsButtonChecked = true;
        viewBtn.Text = "View Enabled";
    }
    else if (e.ClickedItem.Text == "Disable View")
    {
        viewBtn.IsButtonChecked = false;
        viewBtn.Text = "View Disabled";
    }
};
```

## Mode Comparison

| Feature | Normal Mode | Toggle Mode |
|---------|-------------|-------------|
| **State** | No persistent state | Maintains checked/unchecked state |
| **Visual** | Standard button appearance | Visual indication of checked state |
| **Click Behavior** | Execute action | Toggle between states |
| **IsButtonChecked** | Not applicable | Active (true/false) |
| **Use Case** | Commands (Save, Print, Send) | On/Off switches (Show/Hide, Enable/Disable) |
| **Example** | Save button with Save As options | View toggle with view mode options |

## Event Handling

### Click Event

Both modes trigger the `Click` event when the button portion (not dropdown arrow) is clicked:

```csharp
splitButton1.Click += SplitButton1_Click;

private void SplitButton1_Click(object sender, EventArgs e)
{
    if (splitButton1.ButtonMode == ButtonMode.Toggle)
    {
        // Check toggle state
        bool isChecked = splitButton1.IsButtonChecked;
        // Perform action based on state
    }
    else
    {
        // Perform normal button action
    }
}
```

### DropDownItemClicked Event

Handle dropdown item selection:

```csharp
splitButton1.DropDownItemClicked += SplitButton1_DropDownItemClicked;

private void SplitButton1_DropDownItemClicked(object sender, ToolStripItemClickedEventArgs e)
{
    string selectedItem = e.ClickedItem.Text;
    // Handle item selection
}
```

## Best Practices

**For Normal Mode:**
- Use clear action-oriented text (Save, Print, Export, Send)
- Provide related command variations in dropdown
- Keep the primary button action as the most common operation

**For Toggle Mode:**
- Use text that clearly indicates state (Show/Hide, Enable/Disable, On/Off)
- Update button text to reflect current state
- Provide visual feedback through IsButtonChecked state
- Ensure initial state (IsButtonChecked) matches UI expectations

**General:**
- Choose the mode that matches user expectations
- Don't mix stateful and stateless actions in the same button
- For toggle buttons, consider updating button text to reflect current state
- Test both button click and dropdown click behaviors

## Troubleshooting

**Issue: IsButtonChecked not working**
- Verify ButtonMode is set to Toggle (IsButtonChecked only works in Toggle mode)
- Check that property is set after ButtonMode is configured

**Issue: Toggle state not visually updating**
- Ensure control is repainting (call `splitButton1.Refresh()` if needed)
- Verify theme supports toggle state visualization
- Check that IsButtonChecked is being set correctly

**Issue: Click event firing twice**
- Ensure Click event is not attached multiple times
- Check for event handlers in both Designer and code

## Next Steps

- **Dynamic Captions:** Read [button-caption.md](button-caption.md) to update button text based on mode and state
- **Visual Styles:** Read [visual-styles.md](visual-styles.md) for toggle state appearance customization
- **Getting Started:** Return to [getting-started.md](getting-started.md) for basic setup
