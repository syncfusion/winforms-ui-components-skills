# Button Caption Management

This guide explains how to set and manage the SplitButton caption (button text), including static text and dynamic updates based on dropdown item selection.

## Overview

The SplitButton caption is controlled by the `Text` property. You can set it to:
- **Static text:** Fixed caption that doesn't change (e.g., "Save", "Options")
- **Dynamic text:** Caption updates based on user selection from dropdown items

## Setting Static Caption

### Basic Text Property

Use the `Text` property to set a fixed button caption:

**C# Example:**
```csharp
splitButton1.Text = "Click";
```

**VB.NET Example:**
```vb
splitButton1.Text = "Click"
```

### Static Caption Example

Complete example with static caption:

```csharp
public Form1()
{
    InitializeComponent();
    
    SplitButton btn = new SplitButton();
    btn.Text = "Actions"; // Static caption
    btn.Size = new Size(100, 35);
    btn.Location = new Point(20, 20);
    
    // Add dropdown items
    btn.DropDownItems.Add(new ToolStripMenuItem("New"));
    btn.DropDownItems.Add(new ToolStripMenuItem("Open"));
    btn.DropDownItems.Add(new ToolStripMenuItem("Save"));
    
    this.Controls.Add(btn);
}
```

The button will always display "Actions" regardless of which dropdown item is selected.

## Dynamic Caption from Selected Item

### DropDownItemClicked Event

To update the button caption when a user selects a dropdown item, use the `DropDownItemClicked` event:

**C# Example:**
```csharp
private void splitButton1_DropDownItemClicked(object sender, ToolStripItemClickedEventArgs e)
{
    // Set button caption to the text of the selected item
    splitButton1.Text = e.ClickedItem.Text;
}
```

**VB.NET Example:**
```vb
Private Sub splitButton1_DropDownItemClicked(ByVal sender As Object, ByVal e As ToolStripItemClickedEventArgs)
    ' Set button caption to the text of the selected item
    splitButton1.Text = e.ClickedItem.Text
End Sub
```

### Complete Dynamic Caption Example

**C# Implementation:**
```csharp
public Form1()
{
    InitializeComponent();
    
    SplitButton splitButton = new SplitButton();
    splitButton.Text = "Select Country"; // Initial caption
    splitButton.Size = new Size(150, 35);
    splitButton.Location = new Point(20, 20);
    
    // Add dropdown items
    splitButton.DropDownItems.Add(new ToolStripMenuItem("USA"));
    splitButton.DropDownItems.Add(new ToolStripMenuItem("Canada"));
    splitButton.DropDownItems.Add(new ToolStripMenuItem("UK"));
    splitButton.DropDownItems.Add(new ToolStripMenuItem("Australia"));
    splitButton.DropDownItems.Add(new ToolStripMenuItem("India"));
    
    // Attach event handler for dynamic caption
    splitButton.DropDownItemClicked += SplitButton_DropDownItemClicked;
    
    this.Controls.Add(splitButton);
}

private void SplitButton_DropDownItemClicked(object sender, ToolStripItemClickedEventArgs e)
{
    // Update button caption to show selected country
    ((SplitButton)sender).Text = e.ClickedItem.Text;
}
```

**VB.NET Implementation:**
```vb
Public Sub New()
    InitializeComponent()
    
    Dim splitButton As New SplitButton()
    splitButton.Text = "Select Country" ' Initial caption
    splitButton.Size = New Size(150, 35)
    splitButton.Location = New Point(20, 20)
    
    ' Add dropdown items
    splitButton.DropDownItems.Add(New ToolStripMenuItem("USA"))
    splitButton.DropDownItems.Add(New ToolStripMenuItem("Canada"))
    splitButton.DropDownItems.Add(New ToolStripMenuItem("UK"))
    splitButton.DropDownItems.Add(New ToolStripMenuItem("Australia"))
    splitButton.DropDownItems.Add(New ToolStripMenuItem("India"))
    
    ' Attach event handler for dynamic caption
    AddHandler splitButton.DropDownItemClicked, AddressOf SplitButton_DropDownItemClicked
    
    Me.Controls.Add(splitButton)
End Sub

Private Sub SplitButton_DropDownItemClicked(ByVal sender As Object, ByVal e As ToolStripItemClickedEventArgs)
    ' Update button caption to show selected country
    CType(sender, SplitButton).Text = e.ClickedItem.Text
End Sub
```

## Practical Patterns

### Pattern 1: Font Selector

Button displays currently selected font:

```csharp
SplitButton fontSelector = new SplitButton();
fontSelector.Text = "Arial"; // Default font
fontSelector.Size = new Size(120, 30);

// Add font options
fontSelector.DropDownItems.Add(new ToolStripMenuItem("Arial"));
fontSelector.DropDownItems.Add(new ToolStripMenuItem("Times New Roman"));
fontSelector.DropDownItems.Add(new ToolStripMenuItem("Calibri"));
fontSelector.DropDownItems.Add(new ToolStripMenuItem("Verdana"));

fontSelector.DropDownItemClicked += (s, e) => {
    fontSelector.Text = e.ClickedItem.Text;
    // Apply font to document
    ApplyFont(e.ClickedItem.Text);
};
```

### Pattern 2: View Mode Selector

Button shows current view mode:

```csharp
SplitButton viewMode = new SplitButton();
viewMode.Text = "Grid View"; // Default view

viewMode.DropDownItems.Add(new ToolStripMenuItem("Grid View"));
viewMode.DropDownItems.Add(new ToolStripMenuItem("List View"));
viewMode.DropDownItems.Add(new ToolStripMenuItem("Details View"));

viewMode.DropDownItemClicked += (s, e) => {
    viewMode.Text = e.ClickedItem.Text;
    // Switch view mode
    SwitchView(e.ClickedItem.Text);
};
```

### Pattern 3: Prefix with Label

Add descriptive prefix to selected value:

```csharp
SplitButton sizeSelector = new SplitButton();
sizeSelector.Text = "Size: Medium"; // Initial text with label

sizeSelector.DropDownItems.Add(new ToolStripMenuItem("Small"));
sizeSelector.DropDownItems.Add(new ToolStripMenuItem("Medium"));
sizeSelector.DropDownItems.Add(new ToolStripMenuItem("Large"));
sizeSelector.DropDownItems.Add(new ToolStripMenuItem("Extra Large"));

sizeSelector.DropDownItemClicked += (s, e) => {
    // Prefix selected value with "Size: "
    sizeSelector.Text = $"Size: {e.ClickedItem.Text}";
};
```

### Pattern 4: Conditional Caption Updates

Update caption only for specific items:

```csharp
SplitButton actionBtn = new SplitButton();
actionBtn.Text = "Quick Actions";

actionBtn.DropDownItems.Add(new ToolStripMenuItem("Copy"));
actionBtn.DropDownItems.Add(new ToolStripMenuItem("Paste"));
actionBtn.DropDownItems.Add(new ToolStripMenuItem("Settings...")); // Don't update for this

actionBtn.DropDownItemClicked += (s, e) => {
    // Only update caption if it's not "Settings..."
    if (e.ClickedItem.Text != "Settings...")
    {
        actionBtn.Text = e.ClickedItem.Text;
    }
    
    // Perform action
    PerformAction(e.ClickedItem.Text);
};
```

### Pattern 5: Reset to Default Caption

Provide option to reset to default text:

```csharp
SplitButton filterBtn = new SplitButton();
filterBtn.Text = "All Filters"; // Default caption
string defaultCaption = "All Filters";

filterBtn.DropDownItems.Add(new ToolStripMenuItem("Active Items"));
filterBtn.DropDownItems.Add(new ToolStripMenuItem("Completed Items"));
filterBtn.DropDownItems.Add(new ToolStripMenuItem("Archived Items"));
filterBtn.DropDownItems.Add(new ToolStripSeparator());
filterBtn.DropDownItems.Add(new ToolStripMenuItem("Reset Filter"));

filterBtn.DropDownItemClicked += (s, e) => {
    if (e.ClickedItem.Text == "Reset Filter")
    {
        filterBtn.Text = defaultCaption; // Reset to default
    }
    else if (e.ClickedItem is ToolStripMenuItem)
    {
        filterBtn.Text = e.ClickedItem.Text;
    }
};
```

## Advanced Caption Management

### Store Original Caption

Keep reference to initial caption for reset scenarios:

```csharp
public class Form1 : Form
{
    private SplitButton splitButton;
    private string originalCaption = "Select Option";
    
    public Form1()
    {
        InitializeComponent();
        
        splitButton = new SplitButton();
        splitButton.Text = originalCaption;
        
        // ... add items ...
        
        splitButton.DropDownItemClicked += (s, e) => {
            splitButton.Text = e.ClickedItem.Text;
        };
    }
    
    private void ResetButton_Click(object sender, EventArgs e)
    {
        // Restore original caption
        splitButton.Text = originalCaption;
    }
}
```

### Format Caption with Icons

Combine text with visual indicators:

```csharp
splitButton.DropDownItemClicked += (s, e) => {
    // Add checkmark to show selection
    splitButton.Text = $"✓ {e.ClickedItem.Text}";
};
```

### Truncate Long Text

Prevent button from becoming too wide:

```csharp
splitButton.DropDownItemClicked += (s, e) => {
    string selectedText = e.ClickedItem.Text;
    
    // Truncate if too long
    if (selectedText.Length > 20)
    {
        selectedText = selectedText.Substring(0, 17) + "...";
    }
    
    splitButton.Text = selectedText;
};
```

## Caption with Toggle Mode

Combine dynamic caption with toggle state:

```csharp
SplitButton toggleBtn = new SplitButton
{
    Text = "Options",
    ButtonMode = ButtonMode.Toggle,
    IsButtonChecked = false
};

toggleBtn.DropDownItems.Add(new ToolStripMenuItem("Option A"));
toggleBtn.DropDownItems.Add(new ToolStripMenuItem("Option B"));
toggleBtn.DropDownItems.Add(new ToolStripMenuItem("Option C"));

// Update caption from dropdown selection
toggleBtn.DropDownItemClicked += (s, e) => {
    toggleBtn.Text = e.ClickedItem.Text;
};

// Update caption based on toggle state
toggleBtn.Click += (s, e) => {
    if (toggleBtn.IsButtonChecked)
    {
        toggleBtn.Text += " (Active)";
    }
    else
    {
        // Remove " (Active)" suffix
        toggleBtn.Text = toggleBtn.Text.Replace(" (Active)", "");
    }
};
```

## Best Practices

**Static Captions:**
- Use when button represents a category or fixed action (Save, Options, Tools)
- Keep text concise (1-2 words)
- Use clear, action-oriented language

**Dynamic Captions:**
- Use when showing current selection is important (Font selector, View mode, Filter)
- Update caption immediately when dropdown item is clicked
- Provide reasonable initial/default caption
- Consider button width - long text may require truncation

**General:**
- Ensure Text property is set (empty text makes button look incomplete)
- Test button width with longest expected caption
- Consider using tooltips for additional context
- For multi-language apps, store caption keys for localization

## Troubleshooting

**Issue: Caption not updating**
- Verify DropDownItemClicked event is attached
- Check that Text property is being assigned (not returned value)
- Ensure event handler is not throwing exceptions

**Issue: Button resizes unexpectedly**
- Set fixed Size or MaximumSize to prevent resizing
- Use AutoSize = false if using fixed dimensions
- Consider truncating long text

**Issue: Caption overlaps with dropdown arrow**
- Reduce text length or increase button width
- Adjust button Size property
- Consider using abbreviations for long text

## Next Steps

- **Button Modes:** Read [button-modes.md](button-modes.md) to combine caption updates with toggle state
- **Visual Styles:** Read [visual-styles.md](visual-styles.md) for caption appearance customization
- **Getting Started:** Return to [getting-started.md](getting-started.md) for basic setup
