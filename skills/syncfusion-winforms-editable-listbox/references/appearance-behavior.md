# Appearance and Behavior Settings

Comprehensive guide for customizing the appearance and behavior of EditableList control, including embedded controls configuration, scrolling, layout, and interactive features.

## Table of Contents
- [Embedded Controls Overview](#embedded-controls-overview)
- [Button Control Configuration](#button-control-configuration)
- [TextBox Control Configuration](#textbox-control-configuration)
- [ListBox Control Configuration](#listbox-control-configuration)
- [AutoScroll Configuration](#autoscroll-configuration)
- [Dock Padding](#dock-padding)
- [WantButton Property](#wantbutton-property)
- [Control Sizing and Layout](#control-sizing-and-layout)
- [Complete Examples](#complete-examples)

## Embedded Controls Overview

EditableList is a composite control containing three embedded child controls. Understanding and configuring these controls is essential for customizing the EditableList behavior and appearance.

### The Three Embedded Controls

| Control | Purpose | Access Property |
|---------|---------|-----------------|
| **Button** | Optional action button shown during editing | `editableList1.Button` |
| **TextBox** | In-place editor for modifying list items | `editableList1.TextBox` |
| **ListBox** | Main list display showing all items | `editableList1.ListBox` |

### Why Embedded Controls Matter

Each embedded control:
- Has its own complete set of properties and events
- Can be configured independently
- Inherits standard Windows Forms control capabilities
- Provides access to native control functionality

## Button Control Configuration

The Button control provides additional functionality during editing, such as clearing text, accepting edits, or triggering custom actions.

### Accessing Button Properties

```csharp
// Access the embedded Button control
Button btn = this.editableList1.Button;

// Configure button appearance
btn.Text = "Clear";
btn.Width = 70;
btn.Height = 25;
btn.BackColor = System.Drawing.Color.LightCoral;
btn.ForeColor = System.Drawing.Color.White;
btn.Font = new System.Drawing.Font("Arial", 9F, System.Drawing.FontStyle.Bold);
```

### Handling Button Events

```csharp
private void SetupButtonHandlers()
{
    this.editableList1.Button.Click += Button_Click;
}

private void Button_Click(object sender, EventArgs e)
{
    this.editableList1.TextBox.Clear();
}
```

### Button Visibility Control

The button's visibility is controlled by the `WantButton` property (see [WantButton Property](#wantbutton-property) section).

## TextBox Control Configuration

The TextBox control is used for inline editing of list items. Customizing it enhances the editing experience.

### Configuring TextBox Properties

```csharp
// Access the embedded TextBox
TextBox txt = this.editableList1.TextBox;

// Set appearance and behavior
txt.Font = new System.Drawing.Font("Consolas", 10F);
txt.BackColor = System.Drawing.Color.LightYellow;
txt.MaxLength = 100;
txt.BorderStyle = BorderStyle.FixedSingle;
```

### Handling TextBox Events

```csharp
private void SetupTextBoxHandlers()
{
    // Select all text when editing starts
    this.editableList1.TextBox.Enter += (s, e) => {
        this.editableList1.TextBox.SelectAll();
    };
    
    // Validate on leaving
    this.editableList1.TextBox.Leave += (s, e) => {
        string text = this.editableList1.TextBox.Text.Trim();
        if (string.IsNullOrEmpty(text))
        {
            MessageBox.Show("Item cannot be empty!", "Validation Error");
        }
    };
    
    // Handle Enter key to commit edit
    this.editableList1.TextBox.KeyPress += (s, e) => {
        if (e.KeyChar == (char)Keys.Enter)
        {
            e.Handled = true;
            this.editableList1.ListBox.Focus();
        }
    };
}
```

## ListBox Control Configuration

The ListBox control displays all items and handles selection. It provides the full functionality of a standard Windows Forms ListBox.

### Configuring ListBox Properties

```csharp
// Access the embedded ListBox
ListBox lst = this.editableList1.ListBox;

// Set appearance
lst.Font = new System.Drawing.Font("Segoe UI", 10F);
lst.BackColor = System.Drawing.Color.WhiteSmoke;
lst.ForeColor = System.Drawing.Color.DarkSlateGray;
lst.BorderStyle = BorderStyle.FixedSingle;

// Set behavior
lst.SelectionMode = SelectionMode.One;
lst.HorizontalScrollbar = true;
lst.IntegralHeight = false; // Allow partial item display
lst.ScrollAlwaysVisible = false;
```

### Handling ListBox Events

```csharp
private void SetupListBoxHandlers()
{
    // Selection changed
    this.editableList1.ListBox.SelectedIndexChanged += (s, e) => {
        if (this.editableList1.ListBox.SelectedItem != null)
        {
            string selected = this.editableList1.ListBox.SelectedItem.ToString();
            Console.WriteLine($"Selected: {selected}");
        }
    };
}
```



## AutoScroll Configuration

AutoScroll automatically shows scrollbars when the list content exceeds the control's visible area.

### Enabling AutoScroll

```csharp
// Enable automatic scrollbars
this.editableList1.AutoScroll = true;
```

### AutoScroll Properties

| Property | Description | Example |
|----------|-------------|---------|
| **AutoScroll** | Enable/disable automatic scrollbars | `true` or `false` |
| **AutoScrollMargin** | Margin around controls for scroll region | `new Size(2, 2)` |
| **AutoScrollMinSize** | Minimum logical size for scroll region | `new Size(3, 3)` |

### Complete AutoScroll Configuration

```csharp
private void ConfigureAutoScroll()
{
    this.editableList1.AutoScroll = true;
    this.editableList1.AutoScrollMargin = new System.Drawing.Size(2, 2);
    this.editableList1.AutoScrollMinSize = new System.Drawing.Size(3, 3);
}
```

### When to Use AutoScroll

Use AutoScroll for lists with variable number of items that may grow beyond the visible area, or when the control size is constrained by form layout.

## Dock Padding

DockPadding determines the size of the border for docked controls, creating space between the EditableList edges and its embedded controls.

### Understanding DockPadding

DockPadding creates internal margins around the control's content area. Think of it as internal padding that affects how embedded controls are positioned.

### Setting DockPadding

```csharp
// Set padding for all edges equally
this.editableList1.DockPadding.All = 5;

// Or set each edge individually
this.editableList1.DockPadding.Left = 10;
this.editableList1.DockPadding.Right = 10;
this.editableList1.DockPadding.Top = 5;
this.editableList1.DockPadding.Bottom = 5;
```

### Visual Effect and Use Cases

DockPadding creates space between control edges and embedded controls. Use it for visual separation or to create border-like effects by combining with different BackColors.

## WantButton Property

The `WantButton` property controls whether the Button control is visible during editing operations.

### Enabling/Disabling Button

```csharp
// Show button while editing
this.editableList1.WantButton = true;

// Hide button (button not visible during editing)
this.editableList1.WantButton = false;
```

### When Button Appears

When `WantButton = true`:
1. User selects an item
2. User clicks again to edit
3. **TextBox AND Button both appear**
4. Button is positioned to the right of the TextBox

### Practical Use Cases

```csharp
// Clear Button
private void SetupClearButton()
{
    this.editableList1.WantButton = true;
    this.editableList1.Button.Text = "✕";
    this.editableList1.Button.Click += (s, e) => {
        this.editableList1.TextBox.Clear();
    };
}

// Delete Button
private void SetupDeleteButton()
{
    this.editableList1.WantButton = true;
    this.editableList1.Button.Text = "Delete";
    this.editableList1.Button.Click += (s, e) => {
        int index = this.editableList1.ListBox.SelectedIndex;
        if (index >= 0) this.editableList1.ListBox.Items.RemoveAt(index);
    };
}
```

## Control Sizing and Layout

Proper sizing ensures EditableList displays correctly and provides good user experience.

### Basic Sizing

```csharp
// Set control size and position
this.editableList1.Size = new System.Drawing.Size(300, 250);
this.editableList1.Location = new System.Drawing.Point(20, 50);

// Anchor to resize with form
this.editableList1.Anchor = AnchorStyles.Top | AnchorStyles.Bottom | 
                             AnchorStyles.Left | AnchorStyles.Right;

// Set size constraints
this.editableList1.MinimumSize = new System.Drawing.Size(200, 100);
this.editableList1.MaximumSize = new System.Drawing.Size(500, 400);
```

## Complete Examples

### Example 1: Fully Configured EditableList

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class ConfiguredListForm : Form
{
    private EditableList editableList1;
    
    public ConfiguredListForm()
    {
        InitializeComponent();
        SetupEditableList();
    }
    
    private void SetupEditableList()
    {
        // Create control
        this.editableList1 = new EditableList();
        this.editableList1.Location = new Point(20, 20);
        this.editableList1.Size = new Size(350, 300);
        
        // Configure AutoScroll
        this.editableList1.AutoScroll = true;
        this.editableList1.AutoScrollMargin = new Size(2, 2);
        this.editableList1.AutoScrollMinSize = new Size(3, 3);
        
        // Configure DockPadding
        this.editableList1.DockPadding.All = 5;
        
        // Enable Button
        this.editableList1.WantButton = true;
        
        // Configure Button
        this.editableList1.Button.Text = "Clear";
        this.editableList1.Button.Width = 60;
        this.editableList1.Button.Click += (s, e) => {
            this.editableList1.TextBox.Clear();
        };
        
        // Configure TextBox
        this.editableList1.TextBox.MaxLength = 100;
        this.editableList1.TextBox.Font = new Font("Segoe UI", 10F);
        this.editableList1.TextBox.Leave += ValidateEdit;
        
        // Configure ListBox
        this.editableList1.ListBox.Font = new Font("Segoe UI", 10F);
        this.editableList1.ListBox.SelectedIndexChanged += ShowSelection;
        
        // Populate
        string[] items = {
            "Documentation", "Development", "Testing",
            "Deployment", "Maintenance", "Support"
        };
        this.editableList1.ListBox.Items.AddRange(items);
        
        // Add to form
        this.Controls.Add(this.editableList1);
    }
    
    private void ValidateEdit(object sender, EventArgs e)
    {
        string text = this.editableList1.TextBox.Text.Trim();
        if (string.IsNullOrEmpty(text))
        {
            MessageBox.Show("Item cannot be empty!", "Validation");
        }
    }
    
    private void ShowSelection(object sender, EventArgs e)
    {
        if (this.editableList1.ListBox.SelectedItem != null)
        {
            this.Text = $"Selected: {this.editableList1.ListBox.SelectedItem}";
        }
    }
}
```



## Troubleshooting

| Issue | Solution |
|-------|----------|
| Button not appearing | Set `WantButton = true` |
| Scrollbars not showing | Enable `AutoScroll = true` and ensure control has fixed size |
| Embedded controls not responding | Attach event handlers after control initialization |
| DockPadding not visible | Use different BackColor for control and embedded controls |
| Layout looks cramped | Adjust `DockPadding.All` and control `Size` |
